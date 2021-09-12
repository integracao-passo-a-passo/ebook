---
title: "Processadores"
draft: true
weight: 1
---

A forma mais flexível de manipular dados em trânsito (_exchange_) é através de processadores. Um processador é uma classe com propósito de fazer manipulações complexas no conteúdo de um exchange. Esse processador, conhecido como _processor_ no jargão do Camel, pode ser utilizado, por exemplo, para implementar um _parser_, interagir com um _bean_, enriquecer os dados e qualquer outra operação de mais baixo nível que não possa ser feito através da Java DSL.

No Camel um processador é feito implementando a interface [`org.apache.camel.Processor`](https://www.javadoc.io/static/org.apache.camel/camel-api/3.11.1/org/apache/camel/Processor.html) e, então, implementando a lógica de processamento no método [`process`](https://www.javadoc.io/static/org.apache.camel/camel-api/3.11.1/org/apache/camel/Processor.html#process-org.apache.camel.Exchange-). Esse método recebe uma instância do dado em trânsito, do tipo [Exchange](https://www.javadoc.io/static/org.apache.camel/camel-api/3.11.1/org/apache/camel/Exchange.html), através do qual é possível manipula-los.

```java
public void process(Exchange exchange) throws Exception {
       Status status = exchange.getIn().getBody(Status.class);
}
```

No exemplo acima, tirado de uma aplicação de exemplo que interage com o _feed_ do twitter, o processador utiliza os métodos do Exchange para obter um objeto _Status_ que representa uma postagem do Twitter.

Cada sistema com o qual o Camel pode interagir é complexo de uma maneira diferente. Cada ferramenta, sistema ou protocolo com o qual o Camel pode interagir tem o seu conjunto específico de funcionalidades, requerimentos e formas de trabalhar. Embora o Camel abstraia muita da complexidade em torno da integração, muitas vezes expor essas características é fundamental para implementar uma integração de maneira efetiva. Portanto, na grande maioria das vezes é necessário consultar a documentação do componente para entender como essas características ficam acessíveis para as integrações.

**Dica**: os componentes do Camel são [documentados](https://camel.apache.org/components/latest/) de maneira bastante extensa na página oficial do projeto.




