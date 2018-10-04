---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# O que é API do MQ Light e como ela é diferente?
{: #mqlight}

** A API do MQ Light está disponível como parte somente do plano Standard.**
<br/>

A API do {{site.data.keyword.mql}} fornece um nível mais alto de abstração para o sistema de mensagens comparado ao
Kafka.
{:shortdesc}

A escolha entre o uso de um cliente Kafka ou da API do {{site.data.keyword.mql}} depende da
topologia do sistema de mensagens que você deseja construir.

* Com o Kafka, você usa um pequeno número de tópicos e pode ter múltiplas partições para cada tópico
para escalabilidade adicional. É possível compartilhar mensagens entre os consumidores usando grupos de
consumidores, mas cada consumidor deve poder acompanhar a taxa de mensagens para as partições designadas a
ele.
* Com a API do {{site.data.keyword.mql}}, é possível usar um número muito maior de tópicos
e os nomes de tópicos são hierárquicos (por exemplo: <code>'/sports/footbal'</code> e
<code>'/sports/tiddlywinks'</code>). 

Os tópicos na API do {{site.data.keyword.mql}} não são os mesmos que os tópicos do Kafka. Em vez
disso, a API do {{site.data.keyword.mql}} usa um tópico do Kafka único chamado "MQLight" e todas as
mensagens enviadas e recebidas usando a API do {{site.data.keyword.mql}} passam por esse tópico
do Kafka.

O {{site.data.keyword.mql}} está disponível somente nas seguintes
regiões do {{site.data.keyword.Bluemix_notm}}: sul dos EUA, Reino Unido e Sydney. A API do MQ Light
não está disponível na região da Alemanha ou no {{site.data.keyword.Bluemix_notm}} Dedicated.

<!-- begin STAGING ONLY -->
Para obter mais informações sobre como escolher entre as APIs, consulte [Escolhendo entre as três APIs](/docs/services/EventStreams/eventstreams087.html).
<!-- end STAGING ONLY -->

