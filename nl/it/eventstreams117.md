---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-19"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Limiti e quote
{: #kafka_quotas }

{{site.data.keyword.messagehub}} utilizza le quote per controllare le risorse, ad esempio la larghezza di banda della rete che un servizio può utilizzare. I tipi e i livelli delle quote dipendono dal fatto se stai utilizzando il piano Standard o Enterprise.

## Piano Standard
{: #limits_standard }

### Velocità effettiva della rete
{: #standard_throughput }

La velocità effettiva massima per ogni istanza del servizio equivale a 1 MB al secondo per partizione fino a un massimo di 20 MB al secondo. Ad esempio, per un'istanza del servizio con 10 partizioni, la velocità effettiva massima è 10 MB al secondo e per 30 partizioni è 20 MB al secondo.

La velocità effettiva viene misurata separatamente per i produttori e i consumatori. Quando superata, viene applicata una limitazione ritardando leggermente le risposte alle richieste, applicando effettivamente un freno ai produttori e ai consumatori finché non viene ridotta la larghezza di banda.

### Partizioni
{: #standard_partitions}

100 partizioni per ogni istanza del servizio.

### Conservazione
{: #standard_retention}

Un massimo di 1 GB per ogni partizione.

### Altri limiti
{: #standard_limits}

* Dimensione massima del messaggio: 1 MB
* Numero massimo di client Kafka attivi simultaneamente: 100
* Velocità di richiesta massima [HTTP Produce API]: 100 al secondo
* Velocità di richiesta massima [HTTP Admin API]: 10 al secondo

## Piano Enterprise
{: #limits_enterprise }

### Velocità effettiva della rete
{: #enterprise_throughput }

Si consiglia un massimo di 40 MB al secondo con un limite di picco di 75 MB al secondo. La velocità effettiva viene espressa come il numero di byte al secondo che può essere inviato e ricevuto in un cluster.

La figura consigliata si basa su un carico di lavoro tipico e tiene conto del possibile impatto di azioni operative come gli aggiornamenti interni o le modalità di errore come la perdita di una zona di disponibilità. Se la velocità effettiva media supera la figura consigliata, potresti riscontrare una diminuzione delle prestazioni durante tali condizioni.


### Partizioni
{: #enterprise_partitions}

1000 partizioni per ogni istanza del servizio.

### Conservazione
{: #enterprise_retention}

Illimitato, fino al limite di archiviazione del tuo piano.

### Altri limiti
{: #enterprise_limits}

*  Dimensione massima del messaggio: 1 MB
*  Numero massimo di client Kafka attivi simultaneamente: 10000




















