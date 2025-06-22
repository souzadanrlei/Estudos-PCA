# 📘 Conceitos de Observabilidade – PCA

> 📅 Segunda-feira – 18% da prova PCA  
> Este conteúdo cobre o domínio: **Observability Concepts**

---

## 🎯 Objetivos

- Entender os fundamentos de observabilidade em sistemas distribuídos.
- Compreender os papéis de **métricas**, **logs** e **tracing**.
- Diferenciar **SLI**, **SLO** e **SLA**.
- Aplicar o modelo do **Golden Triangle** de observabilidade.

---

## 🔍 O que é Observabilidade?

> Observabilidade é a **capacidade de entender o que está acontecendo internamente em um sistema apenas com base nos sinais externos que ele emite**.

Esse conceito é essencial em ambientes de **microsserviços** e **Cloud Native**, onde sistemas são altamente distribuídos, dinâmicos e mutáveis.

---

## 🧪 Golden Triangle

O modelo de observabilidade moderno se apoia em **três pilares principais**, conhecidos como o **Golden Triangle**:

| Pilar     | Descrição                                                                 |
|-----------|---------------------------------------------------------------------------|
| 📊 Métricas | Dados numéricos em série temporal, úteis para entender tendências e alertas. |
| 📂 Logs     | Registros detalhados de eventos e mensagens em texto.                      |
| 🔍 Traces   | Rastreamento de requisições ponta a ponta entre múltiplos serviços.        |

💡 Embora esses três pilares sejam importantes, **observabilidade não se limita apenas a eles**. O foco é permitir **respostas rápidas** a perguntas como:  
_"Por que esse serviço está lento?"_ ou _"Qual o impacto dessa falha?"_

---

## 📏 SLI, SLO e SLA

| Termo | Significado | Definição | Exemplo |
|-------|-------------|-----------|---------|
| **SLI** | Service Level Indicator | Indicador de desempenho observado. | Latência < 200ms |
| **SLO** | Service Level Objective | Meta interna baseada em SLIs. | 99.9% das requisições com latência < 200ms |
| **SLA** | Service Level Agreement | Acordo formal (geralmente legal) com o cliente. | Reembolsar se uptime < 99% |

🔎 **Dica para a prova**: SLI → o que você mede, SLO → sua meta, SLA → contrato com penalidade.

---

## 📘 Métricas vs Logs vs Tracing

| Tipo     | Quando usar                     | Ferramentas comuns           |
|----------|----------------------------------|------------------------------|
| Métricas | Para detectar *o que* está errado | Prometheus, Grafana, Datadog |
| Logs     | Para entender *por que* aconteceu | Loki, Elasticsearch, Fluentd |
| Tracing  | Para saber *onde* e *quando* algo falhou | Jaeger, Tempo, OpenTelemetry |

---

## 🛠️ Ferramentas relacionadas

- **Prometheus** – Coleta e armazena métricas.
- **Grafana** – Visualização e dashboards.
- **Jaeger / Tempo** – Tracing distribuído.
- **Loki** – Logs centralizados.
- **OpenTelemetry** – Padrão para instrumentação.

---

## ✅ Checklist de aprendizado

- [x] Entendi o conceito de observabilidade.
- [x] Sei explicar o Golden Triangle (Métricas, Logs, Tracing).
- [x] Sei diferenciar SLI, SLO e SLA com exemplos.
- [x] Conheço as ferramentas mais comuns.

---

## 🧪 Exercício sugerido

1. Dê um exemplo real de SLI, SLO e SLA de um sistema que você usa ou mantém.
2. Escolha um serviço no seu stack e responda:
   - Quais métricas estão disponíveis?
   - Existem logs centralizados?
   - O serviço está instrumentado com tracing?

---

## 📚 Leitura complementar

- [SLI vs SLO vs SLA (Google SRE Book)](https://sre.google/sre-book/service-level-objectives/)
- [Prometheus Docs – Overview](https://prometheus.io/docs/introduction/overview/)
- [OpenTelemetry Docs](https://opentelemetry.io/docs/)
- [Grafana Labs - Observability Guide](https://grafana.com/observability/)

---

##### ✨ **Próximo tópico**: [📉 Pull vs Push / Service Discovery]('./Pull-vs-Pull-Service-Discovery/README.md')
