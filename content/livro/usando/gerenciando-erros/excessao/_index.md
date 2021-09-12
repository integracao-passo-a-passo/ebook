---
title: "Por Tratamento de Excessão"
draft: true
weight: 3
---

O último dos métodos faz uma espécie de armadilha (trap), capturando todas as excessões do tipo especificado em qualquer ponto da rota. Usando como exemplo a rota definida anteriormente, temos:

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