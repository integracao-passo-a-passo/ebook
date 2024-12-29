---
title: "Apache ActiveMQ"
draft: false
weight: 3
---

O [Apache ActiveMQ Artemis](https://activemq.apache.org/components/artemis/) é um sistema moderno de mensageria de código aberto, escrito em Java, e mantido pela Fundação de Software Apache. Atualmente encontra-se em desenvolvimento constante sendo trabalhando como o sucessor natural do tradicional [Apache ActiveMQ](https://activemq.apache.org/) e a próxima evolução em termos de mensageria tradicional. É comumente utilizado como uma solução de mensageria prática e de alto desempenho. Suportando dois mecanismos de persistência diferentes, baseados em NIO ou AIO, o Artemis consegue prover um misto de robustez, baixa latência e boa velocidade mesmo quando atuando em modo persistente.

Antes de criarmos o projeto com o Camel, precisamos criar uma instância de um broker Artemis.
Esse passo pode ser realizado facilmente por meio de um container.
O projeto Artemis fornece, com seu código-fonte, instruções para a criação de containers.
Podemos, também, utilizar containers fornecidos pela comunidade. Por exemplo:

```shell
docker run -it --rm -p 8161:8161 -p 61616:61616 -p 5672:5672 -e ARTEMIS_USERNAME=admin -e ARTEMIS_PASSWORD=admin vromero/activemq-artemis:2.15.0-alpine
```

Com isso temos uma instância do Artemis ouvindo nas portas 5672 (protocolo AMQP 1.0), 61616 (OpenWire, CORE, AMQP, Stomp) e 8161 (HTTP/admin). Podemos acessar a interface de administração para verificar se a instância está rodando com sucesso. Para isso basta acessar o endereço http://localhost:8161.

Uma vez que o Artemis esteja rodando com sucesso podemos seguir adiante e criar um projeto simples usando o Camel. Para isso, executamos o seguinte comando:

```shell
mvn archetype:generate -B -DarchetypeGroupId=org.apache.camel.archetypes -DarchetypeArtifactId=camel-archetype-java -DarchetypeVersion=3.18.2 -DgroupId=camel-passo-a-passo -DartifactId=activemq-app-camel -Dversion=1.0.0-SNAPSHOT -Dpackage=activemq.app.camel
```

Assim como no primeiro projeto criado anteriormente, o resultado do comando acima será um projeto básico com o Camel e alguns dos arquivos mínimos necessários para a execução. Modificaremos esse projeto para que ele envie os arquivos para filas diferentes na nossa instância do Apache Artemis.

Para a comunicação com o nosso broker Artemis, podemos utilizar o componente _Simple JMS 2_, também conhecido como `SJMS2`. Esse é um componente que implementa boas práticas do padrão Jakarta Messaging API (JMS) e fornece um mecanismo simples para troca de dados com soluções e protocolos suportados por este padrão.

O primeiro passo para modificar nosso projeto para este propósito é adicionar o componente `camel-sjms2` na lista de dependências do projeto. Fazemos isso modificando o arquivo `pom.xml` e adicionando o seguinte na lista de dependências:

```xml
<dependency>
     <groupId>org.apache.camel</groupId>
     <artifactId>camel-sjms2</artifactId>
</dependency>
```

Precisamos, também, de um provedor para os _“Connection Factories”_. Uma implementação concreta que provê o código necessário para a comunicação com o broker JMS. Para tal, podemos usar diversos projetos como QPid JMS, Artemis JMS e outros. Neste exemplo, usamos o QPid JMS. Desta forma, precisamos adicionar a seguinte dependência:

```xml
<dependency>
    <groupId>org.apache.qpid</groupId>
    <artifactId>qpid-jms-client</artifactId>
    <version>0.54.0</version>
</dependency>
```

**Dica**: ao implementar uma solução profissional, é bastante provável, também, que será necessário utilizar um _pool_ de conexões. Como, por exemplo, o projeto [PooledJMS](https://github.com/messaginghub/pooled-jms).

A partir de então, pode-se criar uma instância do objeto de configuração da _connection factory_, configura-la com os parâmetros de conexão adiciona-lo ao contexto do Camel. Podemos adicionar os códigos a seguir no método configure da classe MyRouteBuilder.

O exemplo abaixo mostra uma configuração simples de acesso a um broker Active MQ configurado com autenticação por usuário e senha (admin/admin, os mesmos utilizados para rodar a instância do Artemis):

```java
JmsConnectionFactory connFactory = new JmsConnectionFactory("amqp://localhost:5672");

// Usuário e senha da instância do broker Artemis
connFactory.setUsername("admin");
connFactory.setPassword("admin");
```

Uma vez que o objeto de configuração foi instanciado, é possível utiliza-lo para instanciar um novo componente Simple JMS 2:

```java
Sjms2Component component = new Sjms2Component();

component.setConnectionFactory(connFactory);
```

Então adicionamos o componente no contexto do Camel, para que ele mapeie os _endpoints_ sjms2 para esse componente configurado previamente:

```java
// Mapeamos endpoint sjms2 para o componente previamente configurado
getContext().addComponent("sjms2", component);
```

Por fim, podemos modificar a rota para que redirecione os arquivos XML para as filas `uk` ou `others`, baseado no conteúdo dos arquivos:

```java
from("file:src/data?noop=true")
    .choice()
        .when(xpath("/person/city = 'London'"))
            .log("UK message")
            .to("sjms2://queue/uk")
        .otherwise()
            .log("Other message")
            .to("sjms2://queue/others");
```

Para executar o projeto, enviando as mensagens para o Artemis, podemos rodar o seguinte comando:

```shell
mvn clean package && mvn exec:java -Dexec.mainClass="activemq.app.camel.MainApp"
```

Podemos checar se as mensagens foram enviadas com sucesso para o Artemis, acessando a interface de administração e verificando a existência de, pelo menos, 1 mensagem em cada um das filas “uk" e “others”.


**Dica**: Alguns dos connection factories comumente utilizados com o SJMS2 são:
org.apache.activemq.artemis.jms.client.ActiveMQConnectionFactory
org.apache.activemq.ActiveMQConnectionFactory



**Dica**: Fique atento para importar corretamente as classes utilizadas neste exemplo:
import org.apache.camel.component.sjms2.Sjms2Component;
import org.apache.qpid.jms.JmsConnectionFactory;





