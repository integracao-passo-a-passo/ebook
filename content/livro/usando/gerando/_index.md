---
title: "Gerando um Novo Projeto"
draft: false
weight: 1
---

O Camel conta com diversos arquétipos (archetypes) do Apache Maven que podem ser utilizados para gerar os projetos.

Inicializar os projetos dessa maneira facilita a padronização de características comuns aos projetos, configura as dependências mínimas necessárias para começar o projeto, provê facilidades e evita todo o trabalho de ajustar manualmente arquivos e diretórios de acordo com o padrão utilizado pelo Maven.

O seguinte comando pode ser usado para gerar um projeto Camel usando a Java DSL:

```shell
mvn archetype:generate -B -DarchetypeGroupId=org.apache.camel.archetypes -DarchetypeArtifactId=camel-archetype-java -DarchetypeVersion=3.14.2 -DgroupId=camel-passo-a-passo -DartifactId=primeiro-app-camel -Dversion=1.0.0-SNAPSHOT -Dpackage=primeiro.app.camel
```

**Dica**: também é possível usar o projeto [Kameleon](https://kameleon.dev), que provê uma interface web, para gerar o projeto.

**Dica**: o Camel fornece arquétipos adicionais para inicializar outros tipos de projetos. Não deixe de conferir quais estão disponíveis na versão atual!

O projeto pode ser compilado e empacotado em no formato jar usando os alvos _clean _e _package_ do Maven.

```shell
mvn clean package
```

Da mesma forma, para executar o projeto podemos usar o alvo exec:java do Maven passando como argumento o nome qualificado da classe `MainApp` (que é criada automaticamente pelo arquétipo).

```shell
mvn exec:java -Dexec.mainClass="primeiro.app.camel.MainApp"
```

A execução desse projeto simples deve projeto inicial deve fazer com o que algumas mensagens de log sejam mostradas na tela até as seguintes mensagens sejam mostradas na tela:

```
[1) thread #1 - file://src/data] route1                         INFO  Other message
[1) thread #1 - file://src/data] route1                         INFO  UK message
```

A partir desse ponto o programa fica em espera de mais dados e nada mais é mostrado. A partir desse ponto é possível abortar a execução do programa.