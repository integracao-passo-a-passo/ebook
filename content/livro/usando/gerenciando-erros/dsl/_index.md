---
title: "Via DSL"
draft: true
weight: 1
---

O primeiro mecanismo disponível permite o tratamento de erros em partes específicas da rota usando um forma similar ao _try/catch/finally_ do Java.

Ao definir a rota é possível usar os métodos [`doTry`](https://www.javadoc.io/static/org.apache.camel/camel-core-model/3.11.1/org/apache/camel/model/ProcessorDefinition.html#doTry--), [`doCatch`](https://www.javadoc.io/static/org.apache.camel/camel-core-model/3.11.1/org/apache/camel/model/TryDefinition.html#doCatch-java.lang.Class-), [`doFinally`](https://www.javadoc.io/static/org.apache.camel/camel-core-model/3.11.1/org/apache/camel/model/TryDefinition.html#doFinally--) e [`end`](https://www.javadoc.io/static/org.apache.camel/camel-core-model/3.11.1/org/apache/camel/model/ProcessorDefinition.html#end--) para capturar e tratar excessões em pontos específicos da rota. Usando a Java DSL, uma rota definida através desse mecanismo seria parecida com o seguinte:

```java
public void configure() throws Exception {
	from("componente:rota")
	.doTry()
		// código da rota que pode ou não lançar excessões
	.doCatch(ClasseDaExcessao.class)
		// tratamento da rota, por exemplo, logar o erro
	.doFinally()
		/* Bloco opcional contento código da rota que será
		 * aplicado em qualquer circunstância
		 */
	.end()
}
```