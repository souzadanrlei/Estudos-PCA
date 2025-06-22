# 🗓️ Plano de Estudos – PCA (4 semanas)

> **Carga horária sugerida**: 1h30 por dia útil (segunda a sexta)  
> **Domingo**: revisão + simulado  
> 💡 *Se tiver mais tempo, você pode concluir em menos de 3 semanas.*

---

## 🔹 Semana 1 – Conceitos e Fundamentos  
**Foco**: 38% da prova (Observability Concepts + Prometheus Fundamentals)

| Dia  | Tema                          | Ação sugerida                                                                 |
|------|-------------------------------|-------------------------------------------------------------------------------|
| Seg  | [📊 Conceitos de Observabilidade](./Conceitos-de-Observabilidade/README.md) | SLI, SLO, SLA, métricas, logs, tracing. Estude o "Golden Triangle".           |
| Ter  | [📉 Pull vs Push / Service Discovery](./Pull-vs-Push-Service-Discovery/README.md) | Entenda como Prometheus coleta dados, autodiscovery, targets.            |
| Qua  | [🧠 Data Model do Prometheus](./Data-Model-do-Prometheus/README.md)   | Métricas: Counter, Gauge, Histogram, Summary. Labels.                        |
| Qui  | 🔧 Prometheus Internals       | TSDB, scrape intervals, retenção, arquitetura.                                |
| Sex  | ⚙️ Configuração básica        | `prometheus.yml`, jobs, targets, relabel.                                     |
| Dom  | 🧪 Revisão e Simulado Parcial (38%) | Faça exercícios do LFS241, Udemy ou GitHub.                             |

---

## 🔹 Semana 2 – PromQL  
**Foco**: 28% da prova (maior peso)

| Dia  | Tema                          | Ação sugerida                                                                 |
|------|-------------------------------|-------------------------------------------------------------------------------|
| Seg  | 📐 Sintaxe básica PromQL      | Como fazer queries simples, `up`, `rate()`, `sum()`, `avg()`                  |
| Ter  | 🔄 Operadores e filtros       | `by()`, `without()`, `offset`, `ignoring`, `group_left`                       |
| Qua  | 🧮 Funções importantes        | `rate()`, `irate()`, `increase()`, `avg_over_time()` etc.                     |
| Qui  | 📊 Agregações e agrupamentos | `sum`, `min`, `max`, `count`, `count_over_time`                               |
| Sex  | 🧪 Exercícios práticos        | Resolva queries reais, dashboards reais (Grafana Labs Play)                   |
| Dom  | 🧪 Simulado + revisão intensiva PromQL | Valide pontos fracos, repita queries no Prometheus Playground        |

---

## 🔹 Semana 3 – Instrumentação e Exporters  
**Foco**: 16% da prova

| Dia  | Tema                           | Ação sugerida                                                              |
|------|--------------------------------|----------------------------------------------------------------------------|
| Seg  | 🛠️ Instrumentação Manual        | Expor métricas em apps com client libraries (Go, Python etc).              |
| Ter  | 🔌 Exporters oficiais e não-oficiais | `node_exporter`, `blackbox_exporter`, `mysqld_exporter` etc.       |
| Qua  | ⚙️ Configuração de exporters    | Como configurar e visualizar métricas de exporters.                        |
| Qui  | 🧪 Lab prático com node_exporter | Rodar localmente com Docker ou em VM.                                      |
| Sex  | 🧪 Revisão + Exercícios de exporters | Entender métricas de sistema (CPU, disco, RAM).                        |
| Dom  | ✅ Simulado parcial (16%)       | Foco em instrumentação e exporters.                                        |

---

## 🔹 Semana 4 – Alertas e Dashboards  
**Foco**: 18% da prova

| Dia  | Tema                             | Ação sugerida                                                                   |
|------|----------------------------------|----------------------------------------------------------------------------------|
| Seg  | 🚨 Alertas no Prometheus         | Regras de alerta, `for`, `expr`, `labels`, `annotations`.                       |
| Ter  | 💬 Alertmanager                  | Agrupamento, rotas, silences, receivers.                                        |
| Qua  | 📈 Dashboards com Grafana        | Conectar Prometheus ao Grafana, criar painéis básicos.                          |
| Qui  | 🧪 Criação de alertas + dashboards reais | Faça uma stack local (Prometheus + node_exporter + Grafana).            |
| Sex  | ✅ Revisão geral e checklist final | Revise todos os tópicos com base no peso do exame.                              |
| Dom  | 📋 Simulado final completo (100%) | Aplique prova simulada com tempo. Avalie pontos fracos.                         |

---

## 📦 Recursos recomendados

- [Linux Foundation – LFS241](https://training.linuxfoundation.org/training/observability-fundamentals-lfs241/)
- [Documentação oficial do Prometheus](https://prometheus.io/docs/introduction/overview/)
- [PromQL Playground (Grafana Labs)](https://play.promlabs.com/)
- Simulados:
  - Udemy
  - GitHub Repositórios: `pca-study-guide`, `PCA practice questions`

