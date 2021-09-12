---
title: "Por Tratamento de Excessão"
draft: true
weight: 3
---

O último dos métodos faz uma espécie de armadilha (_trap_), capturando todas as excessões do tipo especificado em qualquer ponto da rota. Essa armadilha é ativada através do método [`onException`](https://www.javadoc.io/static/org.apache.camel/camel-core-model/3.11.1/org/apache/camel/builder/RouteBuilder.html#onException-java.lang.Class...-), através do qual podemos [definir uma politica de tratamento](https://www.javadoc.io/static/org.apache.camel/camel-core-model/3.11.1/org/apache/camel/model/OnExceptionDefinition.html) para a excessão especificada como parametro para o método.

 Usando como exemplo a rota definida anteriormente, temos:

```java
public void configure() throws Exception {
	onException(ClasseDaExcessao.class)
		.handled(true)
		// código da rota de excessão

	// declaração da rota
	from("direct:start")
	 // código da rota
}
```