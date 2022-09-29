---
title: "Camel Main"
draft: false
weight: 5
---

O Camel pode ser executado em diferentes _runtimes_, como servidores de aplicação como o [Wildlfy](https://www.wildfly.org/), Oracle Weblogic, IBM WebSphere e outros.

Entretanto, com a popularização dos micro-serviços e a containerização de soluções, onde, por exemplo, os projetos são executados em contêineres rodando dentro de clusters Kubernetes, é cada vez mais comum a criação de aplicações leves.

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


Eventualmente pode ser necessário durante alguma parte do ciclo de vida da classe `Main`. Para isso o Camel oferece uma interface chamada [`MainListener`](https://www.javadoc.io/doc/org.apache.camel/camel-main/latest/org/apache/camel/main/MainListener.html) que provê métodos que são executados em diferentes pontos do ciclo de vida da classe `Main`.

```java
public class ExampleMainListener implements MainListener {
    @Override
    public void afterConfigure(BaseMainSupport main) {

    }

    @Override
    public void afterStart(BaseMainSupport main) {
        // Roda depois que o contexto foi criado
    }

    @Override
    public void afterStop(BaseMainSupport main) {
        // Roda depois que o contexto foi parado
    }

    @Override
    public void beforeConfigure(BaseMainSupport main) {
        // Roda depois de o contexto ter sido criado e antes da auto-configuração
    }

    @Override
    public void beforeInitialize(BaseMainSupport main) {
        // Roda depois de o contexto ter sido inicializado e antes da auto-configuração
    }

    @Override
    public void beforeStart(BaseMainSupport main) {
        // Roda antes de o contexto ter sido criado e inicializado
    }


    @Override
    public void beforeStop(BaseMainSupport main) {
        // Roda depois de o contexto ter sido parado
    }

    @Override
    public void configure(CamelContext context) {
        // Roda após a configuração (note que esse método não deve mais ser utilizado, pois está depreciado)
    }
}
```

Posteriormente, uma instância dessa classe pode ser adicionada a uma instância da classe `Main` da seguinte forma:


```java
Main main = new Main();

main.addMainListener(new ExampleMainListener());
main.run(args);

```


