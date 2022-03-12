---
title: "Rotas"
draft: false
weight: 1
---

As rotas são o principal mecanismo de funcionamento do Camel. Através delas é possível definir as regras de ligação entre diferentes pontos de interconexão (endpoints), as políticas de gerenciamento de erro, mediação e transformação dos dados em trânsito e muito mais.

Para declarar uma rota é necessário especializar a classe [`RouteBuilder`](https://www.javadoc.io/static/org.apache.camel/camel-core-model/3.14.2/org/apache/camel/builder/RouteBuilder.html) e implementar o esquema de ligação entre os endpoints no método [`configure`](https://www.javadoc.io/static/org.apache.camel/camel-core-model/3.14.2/org/apache/camel/builder/RouteBuilder.html#configure--).

O código abaixo mostra uma classe de exemplo na qual uma rota pode ser configurada:

```java
public class MyRouteBuilder extends RouteBuilder {

	/**
	* Let's configure the Camel routing rules using Java code...
	*/
	public void configure() {
		// código da rota
	}
```

O Camel permite que as rotas sejam definidas de diversas maneiras, de acordo com a linguagem ou o runtime sendo utilizado, mas a principal maneira é descrevendo-as usando a linguagem Java.