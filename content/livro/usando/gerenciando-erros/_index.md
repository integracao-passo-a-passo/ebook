---
title: "Gerenciando Erros"
draft: false
weight: 6
---

O tópico de gerenciamento de erros é bastante extenso. De modo geral, o Camel fornece três mecanismos para gerenciar excessões na rota. Desta forma, é possível ter um bom nível de flexibilidade para gerenciar erros e excessões da maneira mais adequada para a aplicação. Além disso, cada componente pode oferecer funcionalidades específicas de gerenciamento de erro relevantes para o contexto do componente.

Requerimentos específicos de cada aplicação fazem com que seja necessário avaliar com cautela a melhor estratégia para gerenciar os erros. Detalhes como gerenciamento de transações, rastreamento e monitoramento são algumas das razões pelas quais é preciso pensar com cautela na estratégia adequada para tratamento de erros.

No Camel o gerenciamento de erros é definido durante a configuração da rota e pode ser aplicado tanto para toda a extensão da rota, quanto para a determinadas partes. Não sendo definido nenhum mecanismo, comportamento padrão do Camel será o de logar as excessões e parar a execução da rota para o dado em trânsito.

Os mecanismos de tratamento de erro do Camel são ativados sempre que uma excessão não tratada ocorre em algum ponto da rota.

**Dica**: não deixe de conferir [um exemplo completo](https://github.com/integracao-passo-a-passo/camel-passo-a-passo/tree/master/excessoes-app-camel) mostrando as opções de tratamento de erros discutidas nessa seção.
