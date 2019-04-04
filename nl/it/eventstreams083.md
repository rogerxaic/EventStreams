---

copyright:
  years: 2015, 2019
lastupdated: "2018-05-30"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

<!-- 15/11/18: info moved to eventstreams075.md, moved because of doc app changes -->
# Limiti massimi
{: #maximum_limits}

** L'API MQ Light è disponibile solo come parte del piano Standard.**
<br/>

Per l'API {{site.data.keyword.mql}} vengono applicati seguenti limiti:
{:shortdesc}

* La quantità massima di dati che è possibile memorizzare è coerente con una singola partizione Kafka per il tuo piano di pagamento (in genere 1 GB). Se questo limite di dati viene superato, i messaggi più vecchi nella partizione vengono rimossi quando vengono inviati nuovi messaggi.
* Il tempo massimo per la memorizzazione di un messaggio è coerente con una singola partizione Kafka per il tuo piano di pagamento (in genere 24 ore). Non puoi recuperare i messaggi più vecchi di questo periodo di tempo.
* La dimensione massima di un messaggio (escludendo le intestazioni) è di 1 MB.
* Il numero massimo di client che possono essere collegati in una sola volta è 25.
* Il numero massimo di destinazioni che possono essere attive in una sola volta è 25. Una destinazione attiva è definita come segue:
  - Una destinazione con un TimeToLive > 0 con o senza un client attualmente connesso.
  - Una destinazione con un TimeToLive = 0 (valore predefinito) in cui è connesso un client. 
  <p>Una destinazione condivisa tra più client conta come una singola destinazione.</p>
