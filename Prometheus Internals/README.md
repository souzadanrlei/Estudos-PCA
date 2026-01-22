# 🔧 Prometheus Internals

**TSDB · Scrape Intervals · Retenção · Arquitetura**


### 📌 O que CAI na prova PCA (muito importante)

A CNCF costuma cobrar se você entende:

- 📦 Como o Prometheus armazena dados (TSDB)

- ⏱️ Impacto do scrape_interval

- 🗄️ Retenção de dados (--storage.tsdb.retention)

- 🏗️ Arquitetura do Prometheus (componentes principais)

- ⚠️ Diferença entre armazenamento local x longo prazo

- ⚠️ Limitações do Prometheus (single node, não HA nativo)

- 💡 NÃO cai: detalhes de código interno, WAL byte a byte, nem tuning avançado de kernel.
___
### 🏗️ Arquitetura do Prometheus (visão de prova)

**Componentes principais**
```
[ Exporters / Apps ]
        ↓  (HTTP /metrics)
[ Prometheus Server ]
   ├─ Scraper
   ├─ TSDB
   ├─ Rule Engine
   └─ HTTP API
        ↓
[ Grafana / Alertmanager ]
```
**Função de cada parte**

| Componente      | Função                               |
| --------------- | ------------------------------------ |
| **Scraper**     | Faz HTTP GET em `/metrics`           |
| **TSDB**        | Armazena séries temporais            |
| **Rule Engine** | Avalia regras (recording e alerting) |
| **HTTP API**    | Serve dados para Grafana e PromQL    |

***⚠️ Cai muito:***
➡️ Prometheus ***puxa*** métricas (pull)
➡️ Exporters ***não enviam dados sozinhos***
___
### 📦 TSDB (Time Series Database)
O que é?
Banco de dados embutido no Prometheus, otimizado para séries temporais.

***Como os dados são organizados***
Cada série é definida por:
```
metric_name + labels
```
Exemplo:
```
http_requests_total{method="GET", status="200"}
```

***🔴 Isso já é uma série única no TSDB.***

Estrutura interna (nível prova)
```
/data
 ├── wal/        → Write Ahead Log
 ├── chunks/    → Dados recentes
 └── blocks/    → Dados compactados
```

Conceitos-chave:
- WAL → garante que dados não se percam se o Prometheus cair
- Blocks → dados compactados por janela de tempo
- Imutável → blocos antigos não mudam

***⚠️ Pegadinha de prova:*** 

Prometheus não é um banco relacional
Ele não atualiza linhas, só anexa novos pontos
___
### ⏱️ Scrape Interval
O que é?
Frequência com que o Prometheus coleta métricas.

Exemplo:
```
global:
  scrape_interval: 15s
```
***Impactos diretos***

| Intervalo         | Consequência                      |
| ----------------- | --------------------------------- |
| Muito curto (5s)  | Mais carga, mais cardinalidade    |
| Muito longo (1m+) | Menos precisão em rate()          |
| Padrão            | **15s (o mais cobrado na prova)** |


***💡 Regra de ouro (prova):***

O ```scrape_interval``` define a resolução dos dados

***Relação com PromQL***

Funções como rate() dependem diretamente do intervalo.

***Exemplo ruim:***
```
rate(http_requests_total[10s])
```

***✔️ Correto:***
```
rate(http_requests_total[1m])
```
___

### 🗄️ Retenção de Dados
O que é?
Tempo que o Prometheus mantém dados no disco.

***Configurado via flag:***
```
--storage.tsdb.retention.time=15d
```

📌 Default mais comum:

***15 dias*** (pode variar por versão, mas é o valor “mental” de prova)
___
***Limitação importante (CAI)***

Prometheus ***não é feito para long-term storage***

Para longo prazo:
- Thanos
- Cortex
- Mimir
- VictoriaMetrics

⚠️ Pegadinha clássica:

Aumentar retenção ***não resolve*** problema de escala
___
***⚠️ Limitações Arquiteturais (muito cobrado)***

- ❌ Single node
- ❌ Sem HA nativo
- ❌ Sem replicação interna
- ❌ Memória cresce com cardinalidade

***✔️ Soluções comuns:***

- Sharding
- Remote Write
- Thanos Sidecar
___
***🧠 Resumo mental de prova (decora isso)***

- Prometheus ***puxa métricas***
- TSDB é ***local***
- Série = métrica + labels
- Scrape interval define resolução
- Retenção é tempo, não volume
- Não é banco de longo prazo

----
# 📝 Questionário – Validação de Conhecimento (estilo PCA)

***1️⃣ O que define uma série única no TSDB?***

A) Nome da métrica
B) Nome + valor
C) Nome + labels
D) Nome + timestamp

***2️⃣ Qual impacto direto de reduzir o scrape_interval?***

A) Menos cardinalidade
B) Menos uso de CPU
C) Mais precisão e mais carga
D) Nenhuma diferença

***3️⃣ Onde o Prometheus armazena dados antes da compactação?***

A) Blocks
B) WAL
C) Cache
D) API

***4️⃣ Qual afirmação é verdadeira?***

A) Prometheus suporta HA nativo
B) Prometheus é distribuído por padrão
C) Prometheus é single node
D) Prometheus replica dados automaticamente

***5️⃣ Para retenção de longo prazo, qual abordagem é correta?***

A) Aumentar scrape_interval
B) Aumentar memória
C) Usar Remote Write + Thanos/VictoriaMetrics
D) Usar mais exporters