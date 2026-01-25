# 🧪 Revisão + Simulado Parcial PCA (38%)

### 🔁 BLOCO 1 — Revisão Rápida (cola mental)

***📊 Observabilidade***

- Métrica → valores numéricos ao longo do tempo
- Log → eventos discretos
- Trace → fluxo entre serviços
- Golden Triangle: Metrics, Logs, Traces

***🔑 Prova ama:***

- SLI ≠ SLO ≠ SLA
- Métrica ≠ Log
- 🧠 Prometheus – Fundamentos
- Modelo pull
- HTTP /metrics
- Exporters expondo, Prometheus coletando
- Série = métrica + labels
- Não é banco de longo prazo
- Não é distribuído nativo

***🔧 Internals***

- TSDB local
- WAL → Head → Blocks
- Scrape interval define resolução
- Retenção é tempo, não volume
- Single node

***⚙️ Configuração***
- prometheus.yml
- job_name = rótulo lógico
- instance = target real
- relabel_configs ≠ metric_relabel_configs
- Service Discovery ≠ exporter

# 🧪 BLOCO 2 — SIMULADO (Estilo PCA)

***⏱️ Regra: responda sem pesquisar.***
📝 Formato: múltipla escolha + cenário.

***1️⃣ Qual afirmação descreve corretamente o modelo do Prometheus?***

A) Push, com agentes
B) Pull, via HTTP
C) Streaming contínuo
D) Baseado em eventos

***2️⃣ Qual conjunto define uma série única?***

A) Nome + timestamp
B) Nome + valor
C) Nome + labels
D) Nome + job

***3️⃣ Onde o Prometheus armazena dados imediatamente após o scrape?***

A) Blocks
B) Cache
C) WAL
D) API

***4️⃣ O scrape_interval afeta diretamente:***

A) Retenção
B) Cardinalidade
C) Resolução dos dados
D) Nome das métricas

***5️⃣ Qual afirmação é verdadeira sobre arquitetura?***

A) Prometheus é distribuído
B) Prometheus é HA nativo
C) Prometheus é single node
D) Prometheus replica dados

***6️⃣ Qual é a função do job_name?***

A) Identificar o host
B) Identificar o exporter
C) Agrupar targets logicamente
D) Definir o endpoint

***7️⃣ O label instance representa:***

A) O nome do job
B) O hostname lógico
C) O endpoint real
D) O exporter

***8️⃣ Quando relabel_configs é aplicado?***

A) Após salvar no TSDB
B) Antes do scrape
C) Durante a query
D) Após o scrape

***9️⃣ Para remover métricas específicas antes de salvar:***

A) relabel_configs
B) job_name
C) metric_relabel_configs
D) static_configs

***🔟 Qual cenário exige Thanos ou Cortex?***

A) Mais exporters
B) Scrape mais rápido
C) Retenção de longo prazo
D) Menos labels