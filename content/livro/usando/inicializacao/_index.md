---
title: "Inicialização"
draft: true
weight: 2
---

Explorando o projeto que foi gerado é possível identificar duas classes Java. A primeira delas, a MainApp, executa a inicialização do projeto criando uma instância de um wrapper de execução fornecido pelo próprio Camel. Esse wrapper facilita a execução do Camel como uma aplicação Java padrão sem a necessidade de runtimes adicionais e é definido na classe `org.apache.camel.main.Main`.

Esse wrapper permite uma execução limpa do Camel, sem a necessidade de uso de sleeps, busy spins e outros métodos rudimentares para controlar a execução do Camel.

Nesta classe podemos ver o uso de uma segunda classe auto-gerada, a `MyRouteBuilder`. Esta classe, uma especialização da classe abstrata `org.apache.camel.builder.RouteBuilder` é utilizada para configurar a rota utilizada pelo programa.

Ela é adicionada as propriedades de configuração do wrapper, o que é feito através dos métodos encadeados `configure().addRoutesBuilder()`, para que seja possível executa-la quando o programar rodar.

A configuração da rota dentro do método configure na classe `MyRouteBuilder` provê uma rota bastante simples mas que apresenta diversos conceitos importantes do Camel. De modo geral, a rota copia arquivos de um diretório para outro baseado no seu conteúdo. A rota do projeto de exemplo é a seguinte:

```java
from("file:src/data?noop=true")
     .choice()
         .when(xpath("/person/city = 'London'"))
              .log("UK message")
              .to("file:target/messages/uk")
         .otherwise()
              .log("Other message")
              .to("file:target/messages/others");
```

Avaliando linha a linha a definição desta rota, temos o seguinte:

```java
from("file:src/data?noop=true")
```

Um endpoint inicial from é declarado usando o componente `file`, utilizado para ler, monitorar arquivos e diretórios em disco, usando como entrada o diretório `src/data` que é parte da estrutura de diretórios do projeto recém criado. O componente é configurado com a opção `noop`, para evitar que os arquivos sejam movidos ou removidos do diretório.

Na linha seguinte, o código utiliza o método choice da DSL para aplicar uma condição na rota e definir que os dados devem seguir um rumo ou outro de acordo com uma expressão xpath.

```java
.choice()
         .when(xpath("/person/city = ‘London’"))
             // … código
         .otherwise()
             // … código
```

Dependendo do resultado dessa expressão mostra uma mensagem de log e copia o arquivo para diretórios diferentes. O código usa o método `log` para acessar o _logger_ configurado como parte da configuração inicial do projeto e definido no arquivo `log4j.properties`. Em seguida, define os terminadores da rota usando o método `to`. O terminador usa, novamente, o componente `file` para definir o diretório onde os arquivos serão salvos (`target/messages/uk` caso a expressão _xpath_ resulte em verdadeiro ou `target/messages/others` caso contrário).
