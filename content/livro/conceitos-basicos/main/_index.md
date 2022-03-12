---
title: "Camel Main"
draft: false
weight: 5
---

O Camel pode ser executado em diferentes _runtimes_, como servidores de aplicação como o [Wildlfy](https://www.wildfly.org/), Oracle Weblogic, IBM WebSphere e outros.

Entretanto, com a popularização dos microserviços e a containerização de soluções, onde, por exemplo, os projetos são executados em containers rodando dentro de clusters Kubernetes, é cada vez mais comum a criação de aplicações leves.

Muitas vezes essas aplicações necessitam apenas uma camada fina de abstração capaz de inicializar a aplicação, ler um arquivo configuração ou variáveis de ambiente e executar as rotas. O Camel fornece uma classe [`Main`](https://www.javadoc.io/static/org.apache.camel/camel-main/3.14.2/index.html) que facilita o trabalho de rodar o Camel sem a necessidade de um runtime adicional. Essa classe permite o que se chama de inicialização em modo _standalone_.

O código abaixo mostra um exemplo de como utilizar essa classe:

```java
package primeiro.app.camel;

import org.apache.camel.main.Main;

/**
 * A Camel Application
 */
public class MainApp {
    public static void main(String... args) throws Exception {
        Main main = new Main();
        main.configure().addRoutesBuilder(new MyRouteBuilder());
        main.run(args);
    }

}
```

Para utilizar essa classe é necessário incluir a dependência `camel-main` no projeto.