# 🔄 Operadores e Filtros no PromQL  
**by() · without() · offset · ignoring · group_left**

> 📌 Peso alto na PCA  
> Aqui a prova cobra **leitura correta de queries**, não memorização de sintaxe solta.

---

## 🎯 O que CAI na prova

Você precisa saber:
- Diferença entre `by()` e `without()`
- Como funciona `offset`
- Matching de labels em operações binárias
- Quando usar `ignoring` e `on`
- Para que serve `group_left`

❌ Não cai: joins complexos com múltiplos `group_left` + `group_right`.

---

## ➕ Agregações: `by()` vs `without()`

### 🔹 `by()`
Mantém **somente** os labels listados.

```promql
sum by (job) (
  rate(http_requests_total[5m])
)
```
___
### 🔹 `without()`

Remove apenas os labels listados.
```
sum without (instance) (
  rate(http_requests_total[5m])
)
```

***📌 Regra mental:***

- by() → o que fica
- without() → o que sai

### ⏱️ `Operador offset`

Consulta dados do passado.
```
rate(http_requests_total[5m] offset 1h)
```

***📖 Leitura:***

> Taxa de requisições de 1 hora atrás, calculada em janelas de 5 minutos.

**📌 Uso típico na prova:**
- Comparação “agora vs passado”

### `⚖️ Operações Binárias e Matching de Labels`

Exemplo simples:
```
node_memory_MemFree_bytes
/
node_memory_MemTotal_bytes
```

***🔴 Problema:***

Labels não batem → query falha ou retorna vazio.

### 🎯 `ignoring` (descartar labels no match)
```
node_cpu_seconds_total
/ ignoring (cpu)
node_cpu_seconds_total
```

***📌 O Prometheus:***

- Ignora o label cpu
- Usa os demais para fazer o match

### 🎯 `on` (oposto do ignoring)
```
metric_a / on (instance) metric_b
```
📌 Só considera `instance` para casar séries.

### 🔗 `group_left` (N:1)

Usado quando:

- Lado esquerdo tem mais séries
- Lado direito tem menos séries
```
node_cpu_seconds_total
/ on (instance) group_left
node_memory_MemTotal_bytes
```

***📌 Muito cobrado:***

> group_left não cria série, só permite o match.

🧠 Regra de ouro (PROVA)
| Cenário           | Solução           |
| ----------------- | ----------------- |
| Labels diferentes | `on` / `ignoring` |
| Muitos → poucos   | `group_left`      |
| Comparar passado  | `offset`          |
| Manter labels     | `by()`            |
| Remover labels    | `without()`       |

***⚠️ Erros comuns (CAEM NA PCA)***

- Usar `group_left` sem `on` ou `ignoring`
- Achar que `offset` altera dados reais
- Confundir `by` com `without`
- Tentar usar `group_left` em agregação (ERRADO)

### 🧠 Leitura de query (exemplo de prova)
```
sum by (job) (
  rate(http_requests_total[5m])
)
/
on (job) group_left
sum by (job) (
  rate(http_requests_total[5m] offset 1h)
)
```

***📖 Leitura:***

> Comparação da taxa atual de requisições por job com a taxa de 1 hora atrás.

# 📝 Questionário de Validação (estilo PCA)

***1️⃣ Qual a principal diferença entre by() e without()?***

A) Uma soma, outra média
B) Uma mantém labels, outra remove
C) Uma usa range, outra instant
D) Não há diferença

**2️⃣ Para comparar métricas atuais com métricas do passado, usa-se:**

A) group_left
B) ignoring
C) offset
D) by

***3️⃣ Quando group_left é necessário?***

A) Agregações
B) Quando labels são iguais
C) Match N:1
D) Range vectors

***4️⃣ O operador ignoring(cpu) faz o quê?***

A) Remove o label cpu da série
B) Ignora o label cpu apenas no matching
C) Apaga métricas
D) Agrega valores

***5️⃣ Qual erro comum causa query vazia?***

A) Uso de rate()
B) Labels não casarem
C) Uso de sum()
D) Uso de offset