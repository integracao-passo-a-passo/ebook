---
title: "Componentes e Endpoints"
draft: false
weight: 2
---

A ligação do Camel com os endpoints é feita através de componentes. Os componentes são a  implementação de mais baixo nível do meio utilizado para a troca de mensagens através do Camel. A diferença entre um endpoint e um componente é que enquanto o primeiro declara um terminador da rota, o segundo determina qual é o protocolo ou tecnologia utilizada pelo terminador.

O Camel conta, atualmente em sua versão 3.18.2, com suporte a [mais de 300 componentes](https://camel.apache.org/components/latest/). Através destes, é possível ligar componentes que vão desde os mais tradicionais protocolos de mensageria, passando por redes sociais como o Twitter e o Facebook, até sistemas de CRM como Salesforce. Alguns dos principais componentes disponibilizados pelo projeto são:

**Dica**: todos os componentes do Camel contam com uma enorme gama de opções, sendo possível ajusta-los para os mais diversos propósitos.

| Componente | Descrição |
|------------|-----------|
| AMQP | Suporte ao protocolo AMQP 1.0 utilizado por sistemas de mensageria como Apache Artemis, Apache Qpid, Azure Service Bus e outros. |
| Cassandra | Para consumir e produzir dados do Apache Cassandra. |
| CXF | Suporte a web services e REST APIs através do projeto Apache CXF. |
| Direct | Para chamadas síncronas dentro de um mesmo contexto do Camel. |
| ElasticSearch Rest | Para produzir dados para um instância do ElasticSearch. |
| File | Para trocas de dados com arquivos, através da leitura e gravação de arquivos em locais pré-definidos. |
| FTP | Suporte para troca de dados através do protocolo FTP. |
| HTTP | Suporte para troca de dados através do protocolo HTTP. |
| JMS | Suporte para troca de dados através de mensageria compatível com o padrão JMS. |
| Kafka | Suporte para troca de dados usando o Apache Kafka. |
| Netty | Para comunicação de mais baixo nível via sockets através do projeto Netty. |
| Pulsar | Para troca de dados com o Apache Pulsar. |
| Rest | Para consumir dados via REST. |
| SJMS2 | Suporte para troca de dados através de mensageria compatível com o padrão JMS utilizando uma implementação simples. |
| Quartz | Para troca periódica ou pré-agendada de dados através do projeto Quartz. |
| Seda | Para troca de mensagens assíncrona dentro de um mesmo contexto do Camel. |


O Camel fornece, ainda, a possibilidade de estender o suporte a outros componentes através da implementação de interfaces customizadas que podem ser facilmente adicionadas ao Camel.
