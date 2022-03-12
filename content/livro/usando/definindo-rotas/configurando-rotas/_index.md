---
title: "Configurando Rotas"
draft: false
weight: 1
---

Uma rota pode ser definida através da Java DSL da seguinte forma:

```java
from("especificação do endpoint")
	.to("especificação do endpoint");
```

Para entender melhor, é necessário desconstruir a declaração da rota. Desta forma:

* `from()`: é um [método](https://www.javadoc.io/static/org.apache.camel/camel-core-model/3.14.2/org/apache/camel/builder/RouteBuilder.html#from-java.lang.String-) utilizado para definir o endpoint inicial da rota. É a partir das mensagens recebidas no endpoint especificado nessa declaração que o Camel inicia uma troca de mensagens. Essa troca de mensagens é o que o Camel chama de [Exchange](https://www.javadoc.io/doc/org.apache.camel/camel-api/3.14.2/index.html) – representado por um objeto de mesmo nome.
	* Especificação do endpoint: é uma URI utilizada para especificar o endereçamento do endpoint e suas opções, incluindo, por exemplo, caminhos e diretórios quando necessário. Seu formato assemelha-se ao seguinte: _${componente}:${endereçamento e opções do componente$}{opções}_. Onde:
		* Componente: é um dos componentes do Camel, conforme citado no começo do capítulo.
		* Endereçamento: é um endereçamento específico ao componente.
		* Opções: opções do componente ou do endereçamento. Seu formato segue o padrão utilizado para a parte _"query"_ utilizadas em URI. Ou seja, assemelhasse ao seguinte formato: ?opção1=valor1&opção2=valor2. As opções e valores são específicas para cada componente.
* `to()`: é um [método](https://www.javadoc.io/static/org.apache.camel/camel-core-model/3.14.2/org/apache/camel/model/ProcessorDefinition.html#to-java.lang.String-) utilizado para definir o(s) endpoint(s) final(is) da rota. É opcional e pode estar presente ou não, de acordo com o padrão de integração utilizado.

Abaixo é mostrado um exemplo de uma declaração simples de rota utilizando a Java DSL:

```java
@Override
    public void configure() throws Exception {
        JaxbDataFormat readerFormat = FormatBuilder.getReaderFormat();
        JaxbDataFormat writerFormat = FormatBuilder.getWriterFormat();

        from("activemq:queue:sas.request?" +
                "concurrentConsumers=2&" +
                "maxConcurrentConsumers=4")
                .unmarshal(readerFormat)
                .process(new EvalServiceProcessor())
                .marshal(writerFormat);
}
```
