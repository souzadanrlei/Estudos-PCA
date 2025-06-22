# 🧠 Data Model do Prometheus
### 🧩 Conceito Geral

O modelo de dados do Prometheus é multi-dimensional, baseado em séries temporais.
Cada série temporal é identificada por:
- Um nome de métrica (ex: http_requests_total)
- Um conjunto de labels (rótulos): pares chave-valor que adicionam contexto.

**🧠 Exemplo:**
```
http_requests_total{method="POST", handler="/login", status="200"}
```
**🏷️ Labels (Rótulos)**

- Adicionam dimensões às métricas.
- São fundamentais para filtragens e agregações no PromQL.
- Podem ser definidos estaticamente ou dinamicamente com relabeling.

🛑 Evite usar labels com alta cardinalidade (como user_id), pois isso gera explosão de séries.

### 📊 Tipos de Métricas

Prometheus suporta 4 tipos principais de métricas, cada um com casos de uso específicos:

**1. 🔁 Counter**
- Só aumenta (ou zera).
- Ideal para contar eventos ao longo do tempo (ex: requisições, erros, execuções).

Exemplo:
```
http_requests_total{method="GET", handler="/home"} 45324
```
Use com rate() para calcular a taxa de crescimento por segundo.

[🔗 Doc oficial - Counter]('https://prometheus.io/docs/concepts/metric_types/#counter')

**2. ⚖️ Gauge**

- Pode aumentar ou diminuir.
- Ideal para medir valores que flutuam: uso de memória, temperatura, número de conexões abertas etc.

    Exemplo:
```
memory_usage_bytes{pod="api-server"} 832478208
```
[🔗 Doc oficial - Gauge]('https://prometheus.io/docs/concepts/metric_types/#gauge')

**3. 📈 Histogram**

- Mede a distribuição de valores em faixas (buckets).
- Ideal para tempos de resposta, latência, tamanhos de payload.

xemplo (auto gerado por SDKs):

![alt text](image-2.png)

- Use funções como rate() e histogram_quantile() no PromQL.

[🔗 Doc oficial - Histogram]('https://prometheus.io/docs/concepts/metric_types/#histogram')

**4. 📉 Summary**

- Também mede distribuição, mas gera os percentis diretamente no exporter.
- Ideal para casos em que você quer ver p95/p99, mas perde flexibilidade no PromQL.

    Exemplo:
```
http_request_duration_seconds{quantile="0.95"} 0.423
```
    ⚠️ Resumidamente:
    | Tipo | Percentis | Buckets configuráveis | Agregável no PromQL |
    |------------|-----------|------------------------|----------------------|
    | Histogram | Não | Sim | Sim |
    | Summary | Sim | Não | Não |

[🔗 Doc oficial - Summary]('https://prometheus.io/docs/concepts/metric_types/#summary')

**🧪 Exemplo prático real (em Go)**

![alt text](image-3.png)

**🔎 Visualizando no Prometheus**

Exemplo de query para ver taxa de requisições:
```
rate(http_requests_total[5m])
```
Para agrupar por método:
```
sum by (method) (rate(http_requests_total[1m]))
```
**📚 Materiais complementares**

[📘 Prometheus Docs - Metric Types]('https://prometheus.io/docs/concepts/metric_types/')
[🎥 Vídeo introdutório do Prometheus por Julius Volz (co-criador)]('https://www.youtube.com/watch?v=h4Sl21AKiDg')
[📘 Livro gratuito - Prometheus: Up & Running (O'Reilly) (em inglês)]('https://theswissbay.ch/pdf/Books/Computer%20science/prometheus_upandrunning.pdf')

**✅ Checklist do aprendizado**

- [x] Entendo a diferença entre Counter, Gauge, Histogram e Summary
- [x] Sei quando usar cada tipo de métrica
- [x] Consigo montar métricas com labels que façam sentido
- [x] Entendo a diferença entre distribuição local (Summary) e agregável (Histogram)

---
# 📘 Simulado Oficial — PCA (10 questões)

**1. Qual das seguintes afirmações sobre métricas do tipo Counter são verdadeiras? (Escolha todas que se aplicam)**
a) Podem aumentar ou diminuir ao longo do tempo
b) São redefinidas para zero ao reiniciar a aplicação
c) Devem ser usadas para contar eventos acumulativos
d) Calculam automaticamente a média dos valores

**2. Quais são características corretas de uma Gauge? (Escolha todas que se aplicam)**
a) Pode aumentar ou diminuir
b) Ideal para uso de CPU e memória
c) É reiniciada a cada coleta
d) Armazena distribuição de valores em buckets
**3. Dada a seguinte métrica:**
```
http_requests_total{method="GET", status="200", job="frontend"}
```
Quantos labels essa métrica possui?
a) 1
b) 2
c) 3
d) 4
**4. Quais ferramentas ou funcionalidades o Prometheus oferece para lidar com service discovery? (Escolha todas que se aplicam)**
a) static_configs
b) Kubernetes Service Discovery (kubernetes_sd_configs)
c) PushGateway
d) DNS SRV lookup

**5. Qual é a função da seguinte consulta PromQL?**
```
rate(http_requests_total[5m])
```
a) Calcula a média dos tempos de resposta
b) Exibe o total de requisições nos últimos 5 minutos
c) Retorna a taxa de crescimento por segundo da métrica
d) Agrupa as métricas por status code

**6. Quais são as diferenças principais entre Histogram e Summary? (Escolha todas que se aplicam)**
a) Summary calcula percentis diretamente no exporter
b) Histogram é mais fácil de agregar entre instâncias
c) Summary permite usar histogram_quantile() no PromQL
d) Histogram trabalha com buckets pré-definidos

**7. Por que é uma má prática adicionar um label como user_id em uma métrica?**
a) Labels precisam ser números, e user_id pode ser string
b) Pode causar alta cardinalidade, dificultando performance
c) Labels com underscore (_) não são suportados
d) Porque o Prometheus remove automaticamente labels únicos

**8. Ao reiniciar um exporter, você percebe que a métrica my_app_total_errors foi reiniciada para zero. Qual tipo de métrica isso indica?**
a) Gauge
b) Counter
c) Histogram
d) Summary

**9. Quais das métricas abaixo indicam que você está lidando com um Histogram? (Escolha todas que se aplicam)**
a) http_request_duration_seconds_sum
b) http_request_duration_seconds_bucket{le="0.5"}
c) http_request_duration_seconds{quantile="0.95"}
d) http_request_duration_seconds_count

**10. Qual das opções abaixo melhor descreve o propósito de labels em Prometheus?**
a) Adicionam contexto às métricas e permitem agrupamento no PromQL
b) Servem apenas para identificar o nome da métrica
c) São usados para definir quando uma métrica é resetada
d) São aplicáveis somente a métricas do tipo Gauge e Histogram