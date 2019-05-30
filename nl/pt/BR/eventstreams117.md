---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-09"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Limites e cotas
{: #kafka_quotas }

O {{site.data.keyword.messagehub}} usa cotas para controlar os recursos que um serviço pode consumir, como a largura da banda da rede. Os tipos e os níveis de cotas dependem de se você está usando o plano Padrão ou Corporativo.

## Plano padrão
{: #limits_standard }

### Rendimento da rede
{: #standard_throughput }

O rendimento máximo para cada instância de serviço é igual a 1 MB por segundo por partição até um máximo de 20 MB por segundo. Por exemplo, para uma instância de serviço com 10 partições, o rendimento máximo é de 10 MB por segundo e para 30 partições, é 20 MB por segundo.

O rendimento é medido separadamente para produtores e consumidores. Quando excedido, a limitação é aplicada atrasando um pouco as respostas das solicitações, aplicando efetivamente um freio suave a produtores e consumidores até que a largura da banda seja reduzida.

### Partições
{: #standard_partitions}

100 partições para cada instância de serviço.

### Retenção
{: #standard_retention}

Um máximo de 1 GB para cada partição.

### Outros limites
{: #standard_limits}

* Tamanho máximo da mensagem: 1 MB
* Máximo de clientes Kafka ativos simultaneamente: 100
* Taxa máxima de solicitação [HTTP Produce API]: 100 por segundo
* Taxa máxima de solicitação [HTTP Admin API]: 10 por segundo

## Plano Enterprise
{: #limits_enterprise }

### Rendimento da rede
{: #enterprise_throughput }

Um máximo recomendado de 40 MB por segundo com um limite de pico de 90 MB por segundo. O rendimento é expresso como o número de bytes por segundo que podem ser enviados e recebidos em um cluster. 

O valor recomendado é baseado em uma carga de trabalho típica e leva em conta o possível impacto de ações operacionais como as atualizações internas, os modos de falha ou a perda de uma zona de disponibilidade. Se o rendimento médio exceder o valor recomendado, uma perda no desempenho poderá ser experimentada durante essas condições.


### Partições
{: #enterprise_partitions}

1000 partições para cada instância de serviço.

### Retenção
{: #enterprise_retention}

Ilimitado, até o limite de armazenamento de seu plano.

### Outros limites
{: #enterprise_limits}

Tamanho máximo da mensagem: 1 MB




















