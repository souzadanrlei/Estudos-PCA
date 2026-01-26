# 🧪 Simulado + Revisão Intensiva — PromQL (PCA)

📌 **Instruções**
- Nível: Certificação PCA
- Não chute no automático — pense no **tipo da métrica**, **intervalo**, **ordem da query**
- Responda no formato:  
  `1- A, 2- B, 3- C ...`

---

## 🔁 BLOCO 1 — Conceitos Fundamentais

### 1️⃣ Qual afirmação sobre `rate()` está correta?

A) Retorna o total acumulado no período  
B) Funciona melhor com gauges  
C) Retorna a taxa média por segundo de um counter  
D) Usa apenas os dois últimos pontos  

---

### 2️⃣ Qual função é mais indicada para detectar **spikes rápidos**?

A) rate()  
B) increase()  
C) avg_over_time()  
D) irate()  

---

### 3️⃣ O que acontece se o range vector for **menor que o scrape_interval**?

A) O Prometheus ajusta automaticamente  
B) A query falha  
C) Pode retornar valores errados ou vazios  
D) Nada muda  

---

## 🧮 BLOCO 2 — Funções no Tempo

### 4️⃣ Qual função calcula corretamente o total de erros em 1h?

A) rate(errors_total[1h])  
B) sum_over_time(errors_total[1h])  
C) increase(errors_total[1h])  
D) count_over_time(errors_total[1h])  

---

### 5️⃣ Para analisar o **uso médio de memória** em 10 minutos, você usa:

A) rate(memory_usage[10m])  
B) avg_over_time(memory_usage[10m])  
C) increase(memory_usage[10m])  
D) sum(memory_usage)  

---

### 6️⃣ Qual dessas combinações é **conceitualmente errada**?

A) Counter + rate()  
B) Counter + increase()  
C) Gauge + avg_over_time()  
D) Counter + sum_over_time()  

---

## 📊 BLOCO 3 — Agregações

### 7️⃣ O que `sum(rate(http_requests_total[5m])) by (job)` retorna?

A) Média de requisições por job  
B) Throughput total por job  
C) Total acumulado por job  
D) Quantidade de séries por job  

---

### 8️⃣ Qual função responde:  
“Quantos targets estão UP agora?”

A) sum(up)  
B) count(up)  
C) count(up == 1)  
D) count_over_time(up[5m])  

---

### 9️⃣ O que acontece se você usar `sum()` sem `by()`?

A) Erro de execução  
B) Labels são preservadas  
C) Tudo vira uma única série  
D) Apenas instance é removida  

---

## 🔀 BLOCO 4 — Operadores e Filtros

### 🔟 Qual é a diferença correta entre `by()` e `without()`?

A) Ambos fazem a mesma coisa  
B) `by()` remove labels  
C) `without()` remove labels  
D) Nenhum afeta labels  

---

### 1️⃣1️⃣ Qual operador evita conflito de cardinalidade em joins?

A) offset  
B) ignoring  
C) group_left  
D) unless  

---

### 1️⃣2️⃣ Quando usar `offset`?

A) Para comparar métricas diferentes  
B) Para atrasar ou adiantar uma série no tempo  
C) Para corrigir scrape interval  
D) Para lidar com resets  

---

## ⚠️ BLOCO 5 — Pegadinhas de Prova

### 1️⃣3️⃣ Qual query está **errada conceitualmente**?

A) sum(rate(cpu_seconds_total[5m]))  
B) max(node_memory_Active_bytes)  
C) count_over_time(http_requests_total[5m])  
D) count(up)  

---

### 1️⃣4️⃣ `count_over_time()` retorna:

A) Quantidade de eventos  
B) Total acumulado  
C) Quantidade de amostras  
D) Média por segundo  

---

### 1️⃣5️⃣ Qual afirmação é verdadeira?

A) `rate()` funciona melhor em gauges  
B) Counters podem diminuir naturalmente  
C) `increase()` lida corretamente com resets  
D) `irate()` usa todo o range  

---

## 🧠 BLOCO 6 — Mentalidade PCA

### 1️⃣6️⃣ Antes de escrever uma query PromQL, a pergunta mais importante é:

A) Qual dashboard vou usar  
B) Qual é o scrape_interval  
C) Qual é o tipo da métrica  
D) Qual label vou agrupar  

---

### 1️⃣7️⃣ Qual erro mais reprova candidatos?

A) Esquecer parênteses  
B) Usar função errada para o tipo de métrica  
C) Usar Grafana em vez do Prometheus  
D) Não usar aliases  

---

### 1️⃣8️⃣ Counters devem ser:

A) Agregados antes do rate  
B) Usados diretamente  
C) Passados por rate/increase antes de agregar  
D) Sempre normalizados  

---

## 📈 BLOCO 7 — Cenário Real

### 1️⃣9️⃣ Você quer saber o throughput total do sistema. Qual query escolher?

A) sum(http_requests_total)  
B) sum(rate(http_requests_total[5m]))  
C) rate(sum(http_requests_total)[5m])  
D) count(http_requests_total)  

---

### 2️⃣0️⃣ Você quer detectar se **algum serviço parou de expor métricas**. Qual função usar?

A) rate()  
B) count()  
C) absent()  
D) offset()  

---

## 🧠 Gabarito escondido
👉 Responde tudo primeiro 😈  
Depois eu corrijo, explico **questão por questão**, e te digo **se você passaria na PCA hoje**.
