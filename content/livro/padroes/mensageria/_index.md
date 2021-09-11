---
title: "Sistemas de Mensageria"
draft: true
weight: 2
---

## Sistemas de Mensageria

| Nome | Nome em Inglês | Descrição |
|------|----------------|-----------|
| Canais | Message channel | Virtualiza um meio de comunicação entre transmissor e o receptor. Isso significa que tanto o transmissor quanto o receptor não precisam, necessariamente, saber qual aplicação está emitindo ou recebendo o evento. Isso favorece o desacoplamento arquitetural entre as aplicações. |
| Mensagens | Message | É uma unidade de troca de dados entre sistemas. Pode ser interpretada como um envelope, da onde os dados devem ser extraídos para processamento pelos integrantes. |
| Tubos e filtros | Pipes and filters | Uma abstração do processamento (incluindo, mas não limitado a filtragem) ordenado de mensagens em que cada passo do processamento é ligado por canais. |
| Roteamento de mensagens | Message routing | Processo de encaminhar as mensagens para os receptores corretos. |
| Tradutor de mensagens | Message translator | Processo de modificação das mensagens de tal modo que cada integrante recebe a mensagem no formato que lhe é adequado. |
| Terminadores | Message endpoint | Ponto final/inicial de conexão onde se inicia ou termina o transporte da mensagem. Serve como uma divisão lógica entre a camada da aplicação e a camada que opera em conjunto com sistema de mensageria. |

## Canais de Mensageria

| Nome | Nome em Inglês | Descrição |
|------|----------------|-----------|
| Canal ponto a ponto | Point-to-point channel | Tipo de ligação que garante que só um receptor receberá a mensagem. |
| Publicação/assinante | Publish/subscribe | Tipo de ligação em que um dos integrantes publica uma mensagem e todos os outros integrantes recebem uma cópia da mesma. |
| Canal por tipo de dado | Datatype channel | Forma de ligação que visa garantir que o receptor da mensagem será capaz de interpreta-la e processa-la. |
| Canal de mensagem inválida | Invalid message channel | Canal para onde as mensagens inválidas são encaminhadas. |
| Canal “dead letter” | Dead-letter channel | Canal para onde vão as mensagens que não podem ser entregues. |
| | Entrega garantida | Guaranteed delivery | É uma propriedade do sistema de mensageria que garante a entrega da mensagem. Pode ser feita através de um sistema de persistência, para garantia de entrega mesmo quando o próprio sistema de mensageria falha. |
| Adaptador de canel | Channel adapter | Fornece interfaces, através de APIs (Application Programming Interfaces), para que aplicações se acessem o canal. |
| Ponte de mensageria | Messaging bridge | Permite que sistemas de mensageria se conectem de modo que as mensagens emitidas em um sistema estejam disponíveis em outro. |
| Barramento | Message bus | Combinação de elementos de mensageria que garante o desacoplamento de sistemas mas garantindo que as aplicações conversem entre si através de uma infraestrutura comum. |

## Construção de Mensagens

| Nome | Nome em Inglês | Descrição |
|------|----------------|-----------|
| Mensagem de comando | Command message | Mensagem, sem um tipo especificamente definido, que contém um comando. |
| Mensagem de documento | Document message | Mensagem cujo conteúdo deve ser analisado (ou descartado) pelo receptor  e que não têm uma ligação específica com uma invocação de método. |
| Mensagem de evento | Event message | Uma mensagem cujos dados são referentes a um evento anunciado pelo emissor. |
| Requisição e resposta | Request/reply | Comportamento de troca de dados em que o emissor envia uma requisição, através de um canal específico, e recebe uma resposta, igualmente, em um canal específico para respostas. |
| Endereço de retorno | Return address | Padrão onde o emitente da requisição informa o endereço de retorno – canal – da resposta. Desta forma é possível desacoplar o receptor de um canal resposta previamente definido. |
| Identificador de correlação | Correlation identifier | Fornece uma maneira de associar uma resposta a uma requisição. |
| Sequência de mensagem | Message sequence | Permite sequenciar uma mensagem de modo que grandes quantidades de dados, maiores do que seria possível transmitir em uma única mensagem, podem ser quebrados em partes menores e transmitidos através do canal. |
| Expiração de mensagem | Message expiration | Permite indicar um tempo limite dentro do qual é aceitável consumir a mensagem. |
| Identificador de formato | Format identifier | Fornece ao receptor uma maneira de identificar o formato da mensagem. |


## Roteamento de Mensagens

| Nome | Nome em Inglês | Descrição |
|------|----------------|-----------|
| Roteador baseado em conteúdo | Content-based router | Fornece uma solução para o problema de rotear uma mensagem de acordo com o seu conteúdo. |
| Filtro de mensagem | Message filter | Estabelece a possibilidade de filtrar os dados de uma mensagem de modo a eliminar conteúdo que seja inútil ao receptor. |
| Roteador dinâmico | Dynamic router | Uma solução para o problema de determinar o destino de uma mensagem em tempo de execução – ou seja, quando a mensagem está trafegando pela rota. |
| Lista de receptores | Recipient list | Fornece uma solução para o problema de rotear dinamicamente uma mensagem para uma lista de recipientes.
| Divisor | Splitter | Habilidade de dividir a mensagem em partes menores. |
| Agregador | Aggregator | O agregador está relacionado a habilidade de coletar e armazenar mensagens de tal forma que elas podem ser agrupadas a uma única mensagem. |
| Re-sequenciador | Re-sequencer | Fornece uma reposta a necessidade de reordenar as mensagens para processamento ou envio. |
| Processador de mensagens compostas | Composed message processor | Solução para processar mensagens compostas, de modo que cada sub-mensagem pode ser roteada para um destino específico e cujas respostas, posteriormente, podem ser agregadas em uma única mensagem. |
| Dispersão e recolhimento | Scatter-gather | Está relacionado a agregação das respostas de uma mensagem que fora previamente anunciada a múltiplos receptores (broadcast). |
| Lista de circulação | Routing-slip | Informação de roteamento adicionada a mensagem que permite definir a sequência de processamento. |
| Gerenciador de processos | Process manager | Componente com a responsabilidade de gerenciar o estado, sequenciamento e determinar os próximos passos de processamento. |
| Corretor de mensagens | Message broker | Componente com a responsabilidade de orquestrar a execução das transações. Desacopla o destinatário de uma mensagem de seu receptor. É elaborado a partir da implementação dos padrões de integração de sistemas. |

## Transformação de Mensagens

| Nome | Nome em Inglês | Descrição |
|------|----------------|-----------|
| Enriquecedor de conteúdo | Content enricher | Fornece uma solução para a necessidade de incrementar a carga de dados de uma mensagem. |
| Filtro de conteúdo | Content filter | Fornece uma solução para a necessidade de filtrar dados de uma mensagem, de modo que dados irrelevantes sejam removidos da transação. |

## Gerenciamento de Sistemas

### Escuta (Wire Tap)

Esse padrão detalha uma solução para a necessidade de inspecionar as mensagens trafegando em um canal, com a garantia de que elas não serão modificadas no processo.

## Outros

- Transformação de mensagens
	- Normalizador (normalizer)
	- Modelo de dados canônico (canonical data model)
	- Invólucro de envelope (envelope wrapper)
	- Recibo (claim check)
- Terminadores ou pontos finais de mensageria (endpoints)
	- Portal de entrada de mensagens (messaging gateway)
	- Mapeamento de mensagens (message mapper)
	- Cliente transacional (transactional client)
	- Consumidor por captação (polling consumer)
	- Consumidor dirigido por evento (event-driver consumer)
	- Consumidor concorrente (concurrent consumer)
	- Despachante de mensagens (message dispatcher)
	- Consumidor seletivo (selective consumer)
	- Assinante persistente (persistent subscriber)
	- Receptor idempotente (idempotent receiver)
	- Ativador de Serviço (service activator)
- Gerenciamento de sistemas
	- Controlle de barramento (control bus)
	- Histórico de mensagens (message history)
	- Desvio (Detour)
	- Loja de mensagens (message store)
	- Proxy esperto (smart proxy)
	- Canal de expurgo (channel purger)