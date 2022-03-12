---
title: "Por Manipulador de Erros (Error Handler)"
draft: false
weight: 2
---

O segundo mecanismo disponível fornece uma maneira de tratar qualquer tipo de erro lançado durante a execução da rota. Com este mecanismo é possível, por exemplo, configurar a política de re-entrega, atrasos e tentavias.

A configuração do manipulador de erros é feita através do método [`errorHandler`](https://www.javadoc.io/static/org.apache.camel/camel-core-model/3.14.2/org/apache/camel/builder/RouteBuilder.html#errorHandler-org.apache.camel.builder.ErrorHandlerBuilder-). Esse método recebe um [construtor de um manipulador de erros](https://www.javadoc.io/static/org.apache.camel/camel-core-model/3.14.2/org/apache/camel/builder/ErrorHandlerBuilder.html). Por padrão o Camel já provê alguns tratadores de erros para situações comuns.

Em muitas situações onde o manipulador de erros padrão pode ser suficiente. Entretanto, pode ser necessário configurar parametros operacionais como número máximo de tentativas, intervalo entre cada tentativa e outros. O próprio _errorHandler_ permite faze-lo de maneira bem simples. Por exemplo, para configurar um atraso de 1 segundo com um máximo de 5 tentativas:

```java
public void configure() throws Exception {
	errorHandler(defaultErrorHandler().redeliveryDelay(1000).maximumRedeliveries(5));

	// definição da rota
}
```

Um padrão bastante comum de integração é enviar os dados em erro ou não entregáveis, para uma para um canal “dead letter”. Para fazer o tratamento de erros dessa forma é necessário definir um gerenciador de erros através do método errorHandler. Por exemplo, para usar o gerenciador de erros que redirecionaria o dado em trânsito para um canal “dead letter”:

```java
public void configure() throws Exception {
	errorHandler(deadLetterChannel(“direct:errors”));

	// definição da rota
}
```

Para situações mais complexas é possível criar o seu próprio manipulador de erros.

