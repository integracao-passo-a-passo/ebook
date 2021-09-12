---
title: "Usando Conversores de Dados"
draft: false
weight: 1
---

Um uso comum para os conversores de dados é serializar ou de-serializar dados em formato JSON. O conversor JacksonDataFormat é um dos conversores fornecidos por padrão com o Camel.

Este conversor é bastante util quando você quer serializar e deserializar dados em trânsito para facilitar a manipulação e mediação.

Para configurar e usar esse conversor em uma rota, é possível fazer o seguinte:

```java
public void configure() throws Exception {
	JacksonDataFormat format = new JacksonDataFormat();
	format.setUnmarshalType(MinhaClasse.class);

	from("endpoint")
		.unmarshal(format)
		// código da rota
}
```

Cada formato de dados tem particularides que podem necessitar ou não de passos adicionais para configurar os conversores, portanto é necessário verificar na documentação do conversor se algum passo extra é necessário.