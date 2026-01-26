# 📐 Sintaxe Básica de PromQL  
**Queries simples · up · rate() · sum() · avg()**

> 📌 Peso alto na PCA: **PromQL é ~28% da prova**  
> Aqui a banca cobra **leitura correta de queries**, não truques avançados.

---

## 🎯 O que CAI na prova (direto ao ponto)

Você precisa saber:
- Tipos de dados do PromQL
- Como montar queries simples
- O significado de `up`
- Uso correto de `rate()`
- Diferença entre `sum()` e `avg()`
- Erros comuns que geram respostas erradas na prova

❌ Não cai: joins complexos, subqueries avançadas, funções obscuras.

---

## 🧠 Tipos de dados no PromQL (essencial)

| Tipo | Descrição |
|---|---|
| **Instant Vector** | Valor em um ponto no tempo |
| **Range Vector** | Série em um intervalo de tempo |
| **Scalar** | Número simples |
| **String** | Raramente usado |

📌 Regra de prova:
> `rate()` **sempre recebe Range Vector**

---

## 🟢 Query mais simples possível

promql
```
up
```
O que retorna?

- 1 → target acessível
- 0 → target inacessível

🔴 Pegadinha:

- ```up``` não mede latência, só disponibilidade do scrape.
___
🔍 Filtrando métricas por labels
```
up{job="node"}
```
- Retorna apenas targets do job node
- Filtros são sempre por labels
___
### 🔄 Função rate()
Quando usar?

- Counters (*_total)
```
rate(http_requests_total[5m])
```
O que faz?

Calcula taxa por segundo
Usa diferença entre amostras
Considera resets de counter

***⚠️ Pegadinha de prova:***

Usar rate() em Gauge está errado.
___

### ➕ Agregação com sum()
- Somar séries
```
sum(rate(http_requests_total[5m]))
```
- Soma todas as séries
- Remove labels por padrão

Com agrupamento:
```
sum by (method) (
  rate(http_requests_total[5m])
)
```
___
### ➗ Agregação com avg()
Média entre séries
```
avg(rate(http_requests_total[5m]))
```

Com agrupamento:
```
avg by (instance) (
  rate(http_requests_total[5m])
)
```

***📌 Diferença-chave:***

- sum() → volume total
- avg() → comportamento médio

***⚠️ Erros comuns (CAEM NA PCA)***

- Usar rate() sem range
- Usar rate() em Gauge
- Achar que up=1 significa aplicação saudável
- Esquecer que agregações removem labels

🧠 Leitura de query (modelo mental)
sum by (job) (
  rate(http_requests_total[5m])
)


***📖 Leia assim:***
> “Taxa por segundo de requisições HTTP, somada por job, nos últimos 5 minutos.”

# 📝 Questionário de Validação (estilo PCA)

***1️⃣ Qual tipo de dado rate() retorna?***

A) Scalar
B) String
C) Instant Vector
D) Range Vector

***2️⃣ Qual métrica é mais adequada para rate()?***

A) cpu_usage
B) memory_free
C) http_requests_total
D) temperature

***3️⃣ O que a query up{job="api"} retorna?***

A) Latência da API
B) Disponibilidade do scrape
C) Erros HTTP
D) Uso de CPU

***4️⃣ Qual query mostra o volume total de requisições por segundo?***

A) avg(http_requests_total)
B) sum(http_requests_total)
C) sum(rate(http_requests_total[5m]))
D) rate(sum(http_requests_total)[5m])

***5️⃣ Qual é a principal diferença entre sum() e avg()?***

A) Uma usa range, outra instant
B) Uma soma valores, outra calcula média
C) Uma remove labels, outra não
D) Não há diferença