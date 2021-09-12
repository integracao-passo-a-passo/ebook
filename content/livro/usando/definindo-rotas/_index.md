---
title: "Definindo Rotas"
draft: false
weight: 4
---

Conforme explicado anteriormente, as rotas do Camel são definidas através de uma _DSL_. No caso da Java DSL, para definir uma rota é necessário especializar a classe abstrata [`RouteBuilder`](https://www.javadoc.io/static/org.apache.camel/camel-core-model/3.11.1/org/apache/camel/builder/RouteBuilder.html) e declarar a ligação entre os _Endpoints_ através do método [`configure`](https://www.javadoc.io/static/org.apache.camel/camel-core-model/3.11.1/org/apache/camel/builder/RouteBuilder.html#configure--).

A classe `RouteBuilder` permite um arranjo bastante flexível da declaração da rota, de modo que aplicações complexas podem implementar mecanismos para auxiliar na reutilização de código.


