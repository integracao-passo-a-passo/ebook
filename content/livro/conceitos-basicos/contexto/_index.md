---
title: "Contexto"
draft: false
weight: 4
---

Um [contexto](https://www.javadoc.io/doc/org.apache.camel/camel-api/latest/org/apache/camel/CamelContext.html) do Camel agrupa um conjunto de regras de roteamento. Além disso, também agrupa as instâncias de componentes e registros de conversores.


O contexto está disponível em diversas classes utilizadas para definir rodas e configurar o Camel. Na maioria dos casos, você pode acessar o contexto do Camel durante a configuração da rota. Por exemplo:

```java
public class MyRouteBuilder extends RouteBuilder {

	/**
	* Let's configure the Camel routing rules using Java code...
	*/
	public void configure() {
		getContext().getRegistry().bind("someBeanName", new MyCustomBean());

		// código da rota
	}
```

Em alguns casos mais complexos pode ser necessário injetar um bean antes da inicialização da rota. Nesse caso, a configuração pode variar de acordo com o runtime utilizado. Uma solução usando `camel-main` será mostrada mais adiante.

