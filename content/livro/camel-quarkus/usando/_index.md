---
title: "Usando o Camel Quarkus"
draft: false
weight: 5
---

Assim como nos exemplos anteriores, podemos utilizar arquétipos do Maven para gerar um projeto Camel Quarkus de exemplo. Para criar um projeto de exemplo, consumindo mensagens de um tópico do Apache Kafka, podemos executar os seguintes passos:

```shell
mvn io.quarkus:quarkus-maven-plugin:1.9.2.Final:create -DprojectGroupId=camel-passo-a-passo -DprojectArtifactId=kafka-camel-quarkus -Dextensions=camel-quarkus-log,camel-quarkus-kafka -DclassName=kafka.app.quarkus.KafkaRoute
```

Isso criará um projeto de exemplo do Quarkus com todos os módulos (no caso, `camel-quarkus-log` e `camel-quarkus-kafka`) e dependências pré-configurados, bem como uma classe `KafkaRoute` onde podemos codificar a rota. A criação do projeto através desse arquétipo criará, entretanto, uma classe _“resource"_ padrão do do Quarkus ao invés de uma rota do Camel. Para transforma-la em uma rota, o processo é bastante simples.

A primeira dessas alterações é anotar a classe com a anotação `@ApplicationScoped`. Essa anotação é necessária para que a injeção da configuração, feita através da anotação `@ConfigProperty`, funcione corretamente. De modo geral, não seria necessário e, até mesmo, não recomendável, pois esse escopo tem um ciclo de vida mais complexo e causa um aumento no tempo de inicialização. Posteriormente definimos uma variável membro do tipo `String` que irá conter o endereço do Kafka. Essa variável será anotada com `@ConfigProperty` para permitir que a configuração do endereço do Kafka seja feita através de qualquer um dos mecanismos de configuração suportados pelo Quarkus.

O segundo passo é fazer com que a rota especialize a classe `RouteBuilder`. Esse passo já é conhecido e aplicado ao usar Camel com outros frameworks. Conforme requerido pela super-classe, precisamos definir um método `configure` onde será definido a lógica da rota. Utilizaremos o método [`fromF`](https://www.javadoc.io/static/org.apache.camel/camel-core-model/3.18.2/org/apache/camel/builder/RouteBuilder.html#fromF-java.lang.String-java.lang.Object...-) para passar parâmetros de configuração da rota sem a necessidade de concatenar strings.

Após essas alterações, o código deve ficar semelhante ao seguinte:

```java
import javax.enterprise.context.ApplicationScoped;

import org.apache.camel.builder.RouteBuilder;
import org.eclipse.microprofile.config.inject.ConfigProperty;

@ApplicationScoped
public class KafkaRoute extends RouteBuilder {

   @ConfigProperty(name = "kafka.bootstrap.address")
    String bootstrapAddress;

    @Override
    public void configure() throws Exception {
        fromF("kafka:exemplo?brokers=%s", bootstrapAddress)
            .to("log:info ${body}");
    }
}
```

## Rodando o Exemplo

Para testar o [exemplo](https://github.com/integracao-passo-a-passo/camel-passo-a-passo/tree/master/kafka-camel-quarkus), será necessário subir uma instância do Apache Kafka. Este processo costuma ser um pouco trabalhoso. Para simplificar, podemos utilizar o Docker Compose com o template disponível nos exemplo kafka-camel-quarkus do ebook.

Primeiramente rodamos o docker compose para subir o Kafka e o Zookeeper:

```shell
docker-compose up
```

Assim que estiverem rodando, podemos abrir outro terminal e rodar o código de exemplo. Note como o endereço do Kafka é passado como argumento para o comando. Essa é apenas uma das muitas outras formas suportadas pelo Quarkus. Entre essas formas, inclui-se a possiblidade de configurar o parâmetro no arquivo `application.properties`, passar o valor através de variáveis de ambiente e utilizar diferentes valores de acordo com o ambiente em execução (desenvolvimento, testes ou produção). Para executar o código, usamos o comando:

```shell
mvn -Dkafka.bootstrap.address=localhost:9092 clean compile quarkus:dev
```

O último passo é executar o cliente de linha de comando do Kafka para inserir algumas mensagens no tópico exemplo. Note que o comando abaixo é levemente mais complicado do que o necessário para evitar o trabalho de procurar manualmente o nome do container do Kafka na saída do comando docker ps:

```shell
docker exec -it $(docker ps --format '{{.Names}}' --filter name=^kafka-camel-quarkus_kafka) /opt/kafka/bin/kafka-console-producer.sh --bootstrap-server localhost:9092 --topic exemplo
```

No terminal, você pode digitar as mensagens a serem consumidas pelo código de exemplo:

```
>Olá mundo!
>
```

O log do exemplo Camel Quarkus deve mostrar uma mensagem semelhante ao seguinte:

```
2020-11-15 12:36:22,954 INFO  [info ${body}] (Camel (camel-1) thread #0 - KafkaConsumer[exemplo]) Exchange[ExchangePattern: InOnly, BodyType: String, Body: Olá mundo!]
```

Para sair da console do cliente do Kafka, utilize `Ctrl-D`. Para terminar a execução do código de exemplo e o docker compose, utilize `Ctrl+C` em ambos.



