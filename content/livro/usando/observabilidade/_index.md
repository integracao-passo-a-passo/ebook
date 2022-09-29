---
title: "Observabilidade"
draft: false
weight: 9
---

O camel fornece alguns componentes para observabilidade. Com esses componentes é possível coletar métricas internas do Camel, criar métricas customizáveis e muito mais. O Camel também conta com componentes para Tracing e Telemetria.

| Componente | Descrição |
|------------|-----------|
| Metrics | Componente de métricas baseado no projeto [Metrics](https://metrics.dropwizard.io) |
| Micrometer | Componente de métricas baseado no projeto [Micrometer](http://micrometer.io) |
| Microprofile Metrics | Componente de métricas baseado no projeto [Microprofile Metrics](https://github.com/eclipse/microprofile-metrics) |
| OpenTracking | Componente para telemetria e rastreamento baseado no projeto [OpenTracking](https://opentracing.io) |
| OpenTelemetry | Componente para telemetria e rastreamento baseado no projeto [OpenTelemetry](https://opentelemetry.io) |
| Zipking | Componente para telemetria e rastreamento baseado no projeto [Zipking](https://zipkin.io) |


É possível que cada componente de métricas apresente pequenas variações na sua configuração. Entretanto, de um modo geral, ela deve ser semelhante a configuração mostrada abaixo, que apresenta um exemplo para o componente Micrometer.


```java
	@Override
    public void configure() throws Exception {
		// Cria um registro de medição
		final MeterRegistry registry = getMetricRegistry();
		// Adiciona o registro de medição no registro do Camel usando um identificador pre-configurado específico para o componente
		getContext().getRegistry().bind(MicrometerConstants.METRICS_REGISTRY_NAME, registry);
		// Configura a opção de expor estatísticas da rota
		getContext().addRoutePolicyFactory(new MicrometerRoutePolicyFactory());
		// Configura a opção de expor informações sobre rotas adicionadas e em execução
		getContext().getManagementStrategy().addEventNotifier(new MicrometerRouteEventNotifier());

		// Configuração normal da rota
	}
```

Será necessário, também, garantir que as métricas coletadas sejam acessíveis para a infraestrutura de observabilidade usada pela aplicação. Esse é um processo que pode variar de acordo com o sistema utilizado para coletar essas métricas.

O [Prometheus](https://prometheus.io) é uma ferramenta de código aberto bastante popular para monitoramento de sistemas. Esse projeto funciona muito bem com diversos dos componentes de métricas do Camel. O exemplo abaixo mostra a configuração de um registro de medição do Prometheus, com um servidor HTTP rodando na porta 8180 em conjunto com a mesma instância do Camel, oferecendo as estatísticas através do __endpoint__ `/metrics`.


```java
public final class PrometheusRegistryUtil {
    private PrometheusRegistryUtil() {}

    public static MeterRegistry getMetricRegistry() {
        PrometheusMeterRegistry prometheusRegistry = new PrometheusMeterRegistry(PrometheusConfig.DEFAULT);

        try {
            HttpServer server = HttpServer.create(new InetSocketAddress(8180), 0);
            server.createContext("/metrics", httpExchange -> {
                String response = prometheusRegistry.scrape();
                httpExchange.sendResponseHeaders(200, response.getBytes().length);
                try (OutputStream os = httpExchange.getResponseBody()) {
                    os.write(response.getBytes());
                }
            });

            new Thread(server::start).start();
        } catch (IOException e) {
            throw new RuntimeException(e);
        }

        return prometheusRegistry;
    }
}
```

```yaml
# my global config
global:
  scrape_interval: 10s
  evaluation_interval: 30s
  # scrape_timeout is set to the global default (10s).

rule_files:
  - "first.rules"
  - "my/*.rules"

scrape_configs:
  - job_name: my-camel-service
    metrics_path: /metrics
    static_configs:
      - targets:
          - my-camel-service:8180
```