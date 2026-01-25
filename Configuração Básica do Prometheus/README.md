# ⚙️ Configuração Básica do Prometheus

***prometheus.yml · jobs · targets · relabel***

### 📌 O que CAI na prova PCA

Você precisa saber:

- Estrutura do prometheus.yml
- O que é job, target e instance
- Como o Prometheus descobre targets
- Conceitos de relabel_configs
- Diferença entre relabel_configs e metric_relabel_configs
- Onde erros comuns quebram coleta

❌ Não cai: YAML avançado, regex mirabolante ou config de Kubernetes profunda.
___

### 📄 prometheus.yml – Estrutura mínima (prova)
```
global:
  scrape_interval: 15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: "node"
    static_configs:
      - targets:
          - "localhost:9100"
```
| Bloco            | Função               |
| ---------------- | -------------------- |
| `global`         | Configurações padrão |
| `scrape_configs` | Onde ficam os jobs   |
| `job_name`       | Identificador lógico |
| `targets`        | Endpoints reais      |


***⚠️ Pegadinha:***
- job_name ***não é hostname***
- É apenas um ***rótulo lógico***

### 🧩 Jobs, Targets e Labels (muito cobrado)
***Conceitos***
- ***Job*** → grupo lógico de targets
- ***Target*** → endpoint real (host:port)
- ***Instance*** → label automático = target

Exemplo real:
```
job_name: "node"
targets:
  - "10.0.0.1:9100"
  - "10.0.0.2:9100"
```

Labels geradas automaticamente:
```
job="node"
instance="10.0.0.1:9100"
```

🔴 Cai em prova:

Se dois targets têm o mesmo job, eles ainda são ***séries diferentes*** por causa do instance.

### 🎯 Service Discovery (nível básico)

Formas comuns:

| Tipo             | Uso                  |
| ---------------- | -------------------- |
| `static_configs` | Ambientes simples    |
| Kubernetes SD    | K8s                  |
| EC2 / GCE        | Cloud                |
| File SD          | Dinâmico via arquivo |


⚠️ Prova costuma perguntar:

Prometheus ***descobre targets***, não exporters.
___
### 🔁 Relabeling (parte mais traiçoeira)
***O que é?***

Processo que ***modifica labels antes da ingestão.***

📌 Executado:
➡️ ANTES de salvar no TSDB
___
***relabel_configs (target-level)***

Usado para:
- Remover targets
- Ajustar labels
- Criar labels novos
- Filtrar targets

Exemplo:
```
relabel_configs:
  - source_labels: [__address__]
    target_label: instance
    replacement: "server-01"
```

**metric_relabel_configs (metric-level)**

Executado:
**➡️ DEPOIS do scrape**, antes do TSDB

Usado para:
- Dropar métricas específicas
- Reduzir cardinalidade

Exemplo:
```
metric_relabel_configs:
  - source_labels: [__name__]
    regex: "node_cpu_seconds_total"
    action: drop
```

⚠️ Pegadinha de prova:

```relabel_configs``` ≠ ```metric_relabel_configs```
___
### 🧠 Ordem mental de execução (DECORA)
```
Service Discovery
   ↓
relabel_configs
   ↓
Scrape
   ↓
metric_relabel_configs
   ↓
TSDB
```

Isso cai literal na prova.

### ⚠️ Erros comuns que caem em prova

- Achar que job_name é hostname
- Confundir relabel com metric_relabel
- Usar relabel para filtrar métricas (ERRADO)
- Achar que Prometheus “envia” dados
___

# 📝 Questionário – Estilo PCA

***1️⃣ O que é um target no Prometheus?***

A) Um job
B) Um exporter
C) Um endpoint acessível via HTTP
D) Um dashboard

***2️⃣ Qual label identifica unicamente uma instância?***

A) job
B) target
C) instance
D) name

***3️⃣ Em que momento relabel_configs é aplicado?***

A) Após salvar no TSDB
B) Antes do scrape
C) Depois do scrape
D) Durante a query

***4️⃣ Para reduzir cardinalidade de métricas, você deve usar:***

A) relabel_configs
B) static_configs
C) metric_relabel_configs
D) scrape_interval

***5️⃣ Qual afirmação é verdadeira?***

A) Prometheus envia métricas
B) Exporters puxam dados
C) Prometheus descobre targets
D) Grafana coleta métricas