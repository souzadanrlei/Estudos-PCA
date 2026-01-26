# 🧮 Funções Importantes no PromQL

As funções do PromQL são essenciais para transformar métricas brutas em **informação útil**, principalmente quando lidamos com métricas de **counter**, **gauge** e **histogram**.

---

### 🔁 rate()

Calcula a **taxa média de incremento por segundo** de um *counter* em um intervalo de tempo.

```promql
rate(http_requests_total[5m])
```

***📌 Use quando:***
- Métricas do tipo counter
- Visualizações contínuas
- Dashboards estáveis

❌ Evite usar com gauges.
___
### ⚡ irate()

Calcula a taxa usando apenas os dois últimos pontos do intervalo.
```
irate(http_requests_total[5m])
```

***📌 Use quando:***

- Quer detectar picos rápidos
- Análises mais sensíveis

***⚠️ Pode gerar gráficos mais “nervosos”.***
___
### ➕ increase()

Retorna o total acumulado de aumento de um counter no intervalo.
```
increase(http_requests_total[1h])
```

***📌 Ideal para:***

- Relatórios
- Total de eventos em um período
- Contagem de erros, requisições, falhas
___

### 📉 avg_over_time()

Calcula a ***média dos valores*** de uma métrica ao longo do tempo.
```
avg_over_time(cpu_usage[10m])
```

***📌 Usado com:***

- Gauges
- Consumo médio de CPU, memória, latência

___

### 📈 max_over_time()

Retorna o ***maior valor*** observado no intervalo.
```
max_over_time(memory_usage[15m])
```

***📌 Ótimo para:***

- Detectar picos
- Dimensionamento de recursos

___
## 📊 min_over_time()

Retorna o ***menor valor*** no intervalo.
```
min_over_time(disk_free_bytes[30m])
```
___
### 🔢 sum_over_time()

Soma todos os valores no intervalo.

```
sum_over_time(requests_per_minute[1h])
```

***📌 Use com cuidado:***

- Não recomendado para counters (prefira increase())
___
### 🧠 Boas Práticas

- Counters → rate(), irate(), increase()
- Gauges → avg_over_time(), max_over_time(), min_over_time()
- Sempre escolha o intervalo de tempo correto
- Intervalos curtos demais podem gerar ruído

### 🚨 Erros Comuns

- ❌ Usar rate() em gauges
- ❌ Usar sum_over_time() em counters
- ❌ Intervalos muito pequenos para métricas com scrape lento

### 🎯 Resumo rápido

- rate() → tendência estável
- irate() → picos rápidos
- increase() → total acumulado
- *_over_time() → análise histórica
___

# 🧮 Funções Importantes no PromQL — Avaliação

***1️⃣ Quando devemos usar `rate()` em vez de `increase()`?***

A) Quando queremos o valor total acumulado em um período  
B) Quando a métrica é do tipo gauge  
C) Quando queremos a taxa média por segundo de um counter  
D) Quando precisamos detectar picos instantâneos  

---

***2️⃣ Qual a principal diferença entre `rate()` e `irate()`?***

A) `rate()` funciona apenas com gauges  
B) `irate()` calcula a média de todo o intervalo  
C) `irate()` usa apenas os dois últimos pontos do intervalo  
D) `rate()` ignora resets de counter  

---

***3️⃣ Qual função é mais adequada para calcular o total de requisições em 24h?***

A) `rate(http_requests_total[24h])`  
B) `sum_over_time(http_requests_total[24h])`  
C) `increase(http_requests_total[24h])`  
D) `avg_over_time(http_requests_total[24h])`  

---

***4️⃣ Qual função você deve usar para analisar o consumo médio de CPU nos últimos 10 minutos?***

A) `rate(cpu_usage[10m])`  
B) `increase(cpu_usage[10m])`  
C) `avg_over_time(cpu_usage[10m])`  
D) `irate(cpu_usage[10m])`  

---

***5️⃣ Qual dessas opções representa um uso incorreto de função?***

A) `rate()` em counters  
B) `increase()` em counters  
C) `avg_over_time()` em gauges  
D) `sum_over_time()` em counters  

---

***6️⃣ Você precisa identificar picos rápidos de erro em um serviço. Qual função escolher?***

A) `rate()`  
B) `irate()`  
C) `increase()`  
D) `avg_over_time()`  

---

***7️⃣ O que acontece se o intervalo da função for menor que o scrape interval?***

A) A query fica mais precisa  
B) A função falha automaticamente  
C) Pode retornar valores incorretos ou vazios  
D) O Prometheus ajusta automaticamente  

---

***8️⃣ Qual função é ideal para detectar o maior pico de memória em 30 minutos?***

A) `avg_over_time()`  
B) `min_over_time()`  
C) `sum_over_time()`  
D) `max_over_time()`  

---

 ***9️⃣ Por que `sum_over_time()` não é recomendado para counters?***

A) Porque ignora labels  
B) Porque não lida corretamente com resets  
C) Porque retorna valores negativos  
D) Porque funciona apenas com gauges  

---

***🔟 Qual combinação está correta?***

A) Gauge + rate()  
B) Counter + avg_over_time()  
C) Counter + increase()  
D) Counter + min_over_time()  

