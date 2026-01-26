## 📊 Agregações e Agrupamentos no PromQL
### sum, min, max, count, count_over_time

---

## 🔹 O que são agregações?
Agregações servem para **reduzir múltiplas séries temporais em um único valor** (ou menos séries), aplicando operações matemáticas.

Elas **sempre operam sobre vetores instantâneos** e geralmente são combinadas com:
- `by()`
- `without()`

---

## 🔹 Principais funções de agregação

### 🔸 sum()
Soma os valores de múltiplas séries.

Exemplo:
```promql
sum(rate(http_requests_total[5m]))
```
Com agrupamento:
```
sum(rate(http_requests_total[5m])) by (job)
```
***📌 Muito usada para throughput total.***
___
### 🔸 min() e max()

Retornam o menor ou maior valor entre séries.

Exemplo:
```
max(node_memory_Active_bytes)
```

Com agrupamento:
```
max(node_memory_Active_bytes) by (instance)
```

***📌 Usadas para detectar picos ou gargalos.***
___
### 🔸 count()

Conta quantas séries existem após filtros.

Exemplo:
```
count(up == 1)
```

***📌 Comum para saber quantos targets estão UP.***
___

### 🔸 count_over_time()

Conta quantas amostras existem em um intervalo de tempo.

Exemplo:
```
count_over_time(up[5m])
```

***📌 Atenção:***
- Não conta eventos
- Conta amostras coletadas

Cai MUITO em prova.
___

### ⚠️ Armadilhas comuns de prova

- ❌ Usar count() esperando total de eventos
- ❌ Usar count_over_time() como se fosse increase()
- ❌ Esquecer by() e colapsar tudo sem querer
- ❌ Agregar antes do rate() (ordem importa!)

### ✅ Boas práticas

- ✔️ Counters → rate() ou increase() ANTES de agregar
- ✔️ Gauges → agregação direta ok
- ✔️ Sempre pense: quantas séries existem antes e depois da query?
___
# 🧪 Avaliação — Agregações e Agrupamentos

***1️⃣ O que `count(up == 1)` retorna?***

A) Total de requisições bem-sucedidas
B) Número de targets ativos
C) Quantidade de scrapes realizados
D) Total de amostras no TSDB

***2️⃣ Qual função retorna o maior valor entre várias séries?***

A) sum()
B) count()
C) max()
D) min()

***3️⃣ Qual é o uso correto de count_over_time()?***

A) Contar eventos em counters
B) Contar requisições HTTP
C) Contar amostras coletadas em um período
D) Somar valores ao longo do tempo

***4️⃣ Qual query calcula corretamente o throughput total?***

A) sum(http_requests_total)
B) rate(sum(http_requests_total)[5m])
C) sum(rate(http_requests_total[5m]))
D) count(rate(http_requests_total[5m]))

**5️⃣ Qual dessas opções é uma armadilha clássica de prova?**

A) Usar max() para picos
B) Usar sum() com by(job)
C) Usar count_over_time() em gauges
D) Usar count_over_time() como se fosse increase()

***6️⃣ O que acontece se você usar sum() sem by()?***

A) O Prometheus retorna erro
B) As labels são preservadas
C) Todas as séries são agregadas em uma só
D) Apenas a label instance é removida

***7️⃣ Qual função responde:***

“Quantos pods estão UP agora?”

A) count_over_time(up[5m])
B) sum(up)
C) count(up == 1)
D) max(up)

***8️⃣ Qual dessas queries está conceitualmente errada?***

A) sum(rate(cpu_seconds_total[5m]))
B) max(node_memory_Active_bytes)
C) count_over_time(http_requests_total[5m])
D) count(up)