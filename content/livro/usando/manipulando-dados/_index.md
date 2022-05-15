---
title: "Manipulando Dados"
draft: false
weight: 5
---

Durante o roteamento e a mediação dos dados em trânsito, uma necessidade bastante comum é a de manipular, transformar e enriquece-los. O Camel fornece diversos mecanismos para esse fim, desde extensões disponíveis na própria Java DSL até processadores com capacidade de manipular diretamente os dados em trânsito.

Adicionar cabeçalhos (_headers_) através do método [`setHeader`](https://www.javadoc.io/static/org.apache.camel/camel-core-model/3.14.2/org/apache/camel/model/ProcessorDefinition.html#setHeader-java.lang.String-org.apache.camel.Expression-) é um exemplo bem comum desse tipo de manipulação:

```java
from("direct:start")
	.setHeader(Exchange.CONTENT_TYPE, constant("application/x-www-form-urlencoded"))
	.setHeader(Exchange.HTTP_METHOD, constant("POST"))
	// código
 	.to("http://meu.endpoint.rest")
```

O Camel oferece diferentes maneiras de manipular os dados em trânsito de maneira simples. Além do `setHeader` mencionado acima, é possível citar:
* `bean`: para setar um bean que pode ser executado para transformar os dados ou definir o terminador da rota.
* `pollEnrich`: para enriquecer um exchange com dados obtidos a partir de uma URI externa.
* `process`: para passar os dados em trânsito para um processador, que pode transformar, enriquecer ou definir um terminador para a rota.
* `setBody`: para setar valor do corpo da mensagem.
* `setHeader`: para setar o valor de um cabeçalho.
* `removeHeader`: para remover um cabeçalho.

Na Java DSL, quando uma definição de rota é criada a partir do método `from`, é criado uma instância de uma especialização da [`ProcessorDefinition`](https://www.javadoc.io/static/org.apache.camel/camel-core-model/3.14.2/org/apache/camel/model/ProcessorDefinition.html). É possível verificar esses mecanismos de transformação e muitos verificando a API fornecida por essa classe abstrata.

## Expressões

Em alguns casos, como o [`setBody`](https://www.javadoc.io/static/org.apache.camel/camel-core-model/3.14.2/org/apache/camel/model/ProcessorDefinition.html#setBody-org.apache.camel.Expression-) e o [`setHeader`](https://www.javadoc.io/static/org.apache.camel/camel-core-model/3.14.2/org/apache/camel/model/ProcessorDefinition.html#setHeader-java.lang.String-org.apache.camel.Expression-), é necessário passar uma implementação de uma [`Expression`](https://www.javadoc.io/static/org.apache.camel/camel-api/3.14.2/org/apache/camel/Expression.html). Isso representa uma expressão que será resolvida durante o trânsito do dado pela rota.

Essas expressões podem definir tanto valores constantes quanto expressões nos mais variados tipos de linguages de expressões suportadas pelo Camel. Ao definir uma transformação de rota que receba uma expressão, é possível utilizar os seguintes métodos para instanciar a instância da expressão desejada:

- [`constant()`](https://www.javadoc.io/static/org.apache.camel/camel-core-model/3.14.2/org/apache/camel/builder/BuilderSupport.html#constant-java.lang.Object-): para definir uma expressão de valor constante.
- [`jsonpath()`](https://www.javadoc.io/static/org.apache.camel/camel-core-model/3.14.2/org/apache/camel/builder/BuilderSupport.html#jsonpath-java.lang.String-): para definir uma expressão do tipo [_JSON Path_](https://camel.apache.org/components/latest/languages/jsonpath-language.html).
- [`simple()`](https://www.javadoc.io/static/org.apache.camel/camel-core-model/3.14.2/org/apache/camel/builder/BuilderSupport.html#simple-java.lang.String-): para definir uma expressão do tipo [_Simple_](https://camel.apache.org/components/latest/languages/simple-language.html).
- [`xpath()`](https://www.javadoc.io/static/org.apache.camel/camel-core-model/3.14.2/org/apache/camel/builder/BuilderSupport.html#xpath-java.lang.String-): para definir uma expressão do tipo [_XPath_](https://camel.apache.org/components/latest/languages/xpath-language.html).

Outros tipos de expressões estão disponíveis e, pra casos mais complexos é possível implementar a sua própria expressão.