---
title: "Conversores de Dados"
draft: false
weight: 7
---

 Os conversores de dados permitem a implementação de rotinas de conversão de dados entre o formato utilizado para a comunicação entre as aplicações. O Camel contém uma série de conversores de tipos que podem ser configurados para facilitar a conversão de dados ao longo das rotas.

O Apache Camel contém diversos tipos de conversores de dados. Alguns deles são específicos para determinados componentes. Exemplos de conversores de dados bastante populares e comumente utilizados são:

* [Avro](https://camel.apache.org/components/latest/dataformats/avro-dataformat.html)
* [Bindy](https://camel.apache.org/components/latest/dataformats/bindy-dataformat.html)
* [CSV](https://camel.apache.org/components/latest/dataformats/csv-dataformat.html)
* [JAXB](https://camel.apache.org/components/latest/dataformats/jaxb-dataformat.html)
* [JSon Jackson](https://camel.apache.org/components/latest/dataformats/json-jackson-dataformat.html)
* [Protobuf](https://camel.apache.org/components/latest/dataformats/protobuf-dataformat.html)


Muitos outros formatos estão disponíveis e é possível implementar o seu próprio caso necessário. Não deixe de conferir a [documentação oficial](https://camel.apache.org/components/latest/dataformats/) do projeto antes de implementar o seu próprio conversor.
