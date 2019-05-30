---

copyright:
  years: 2015, 2019
lastupdated: "2018-06-01"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Bridge MQ sul piano Classic 
{: #mq_bridge}

** Il bridge MQ è disponibile solo come parte del piano Classic.**
<br/>

Il bridge MQ ti consente di trasferire i dati del messaggio da una coda {{site.data.keyword.IBM_notm}}
 MQ a un argomento Kafka {{site.data.keyword.messagehub}}. Il bridge MQ ti permette di eseguire in modo efficiente carichi di lavoro in stile cloud (ad esempio, analisi dei dati) in relazione ai dati del messaggio {{site.data.keyword.IBM_notm}} MQ generati all'interno della tua azienda.
 {:shortdesc}

Il bridge MQ si connette a un gestore code {{site.data.keyword.IBM_notm}} MQ come client MQ e consuma i dati del messaggio MQ da un coda MQ. Il bridge converte ogni messaggio MQ in un record Kafka e invia il messaggio a un argomento Kafka {{site.data.keyword.messagehub}}.

## Versioni supportate di {{site.data.keyword.IBM_notm}} MQ
{: #mq_support}

Le versioni di {{site.data.keyword.IBM_notm}} MQ con cui funziona il bridge sono:

* {{site.data.keyword.IBM_notm}} MQ versione 8.0 e successive

## Protezione del bridge MQ
{: #mq_bridge_security}

Puoi configurare il bridge MQ per l'autenticazione con {{site.data.keyword.IBM_notm}} MQ utilizzando un identificativo utente e una password. Si consiglia di concedere le seguenti autorizzazioni solo all'identità associata a un'istanza del bridge MQ:

* Autorizzazione CONNECT. Il bridge MQ deve essere in grado di connettersi al gestore code MQ.
* Autorizzazione GET per la coda con cui è configurato il bridge MQ per il consumo.

## Affidabilità e ordinamento dei messaggi
{: #mq_bridge_reliability}

Il bridge MQ implementa un livello di affidabilità di tipo at-least-once per i dati del messaggio che
trasferisce. Ciò significa che il bridge non perde i dati del messaggio, ma ogni tanto potrebbe generare
sequenze duplicate di record Kafka per una determinata sequenza di messaggi MQ. In genere, la duplicazione si
verifica quando un evento interrompe l'elaborazione del bridge per i dati del messaggio. Ad esempio:

* Riavvio del gestore code MQ
* Interruzione della rete tra il gestore code MQ e il bridge
* Ribilanciamento dell'assegnazione della partizione dell'argomento Kafka
* Interruzione della rete tra il bridge e Kafka

Per garantire che il bridge MQ possa trasferire in modo affidabile i messaggi da MQ, il bridge richiede
l'accesso esclusivo alla coda MQ da cui legge i messaggi. Questo accesso impedisce ad altre
applicazioni di ricevere messaggi dalla coda MQ mentre è utilizzata dal bridge. Se altre
applicazioni stanno già ricevendo messaggi dalla coda, potrebbe non essere possibile avviare il bridge MQ
fino a quando queste applicazioni non terminano.

Generalmente, il bridge MQ inoltra i messaggi MQ a Kafka nell'ordine in cui vengono ricevuti da {{site.data.keyword.IBM_notm}} MQ. Tuttavia, talvolta i dati dei messaggi MQ vengono duplicati all'interno di un argomento Kafka (per i motivi descritti in precedenza). Questa possibile duplicazione può causare differenze tra l'ordine dei messaggi MQ e l'ordine in cui i record corrispondenti vengono memorizzati in Kafka.

## Assegnazione della partizione Kafka
{: #mq_bridge_partition}

Il bridge MQ supporta le opzioni per controllare l'assegnazione dei dati dei messaggi {{site.data.keyword.IBM_notm}} MQ alle partizioni dell'argomento Kafka. L'ordine dei messaggi all'interno di una particolare partizione dipende dall'opzione selezionata per l'ordinamento dei dati dei messaggi (vedi [Affidabilità e ordinamento dei messaggi](#mq_bridge_reliability)). I valori supportati sono i seguenti:
<dl><dt>Valore predefinito</dt>
<dd>Viene utilizzata l'assegnazione della partizione predefinita di Kafka, il che significa che i dati dei messaggi MQ
vengono distribuiti in modo uniforme attraverso le partizioni nell'argomento Kafka.</dd>
<dt>CorrelationId</dt>
<dd>I messaggi MQ con lo stesso ID correlazione vengono assegnati alla stessa partizione Kafka.</dd>
<dt>GroupId</dt>
<dd>I messaggi MQ con lo stesso ID gruppo vengono assegnati alla stessa partizione Kafka.
</dd>
</dl>

## Restrizioni della dimensione dei messaggi
{: #mq_message}

Puoi configurare {{site.data.keyword.IBM_notm}} MQ per memorizzare i messaggi che sono troppo grandi per rientrare in un record Kafka {{site.data.keyword.messagehub}}. La dimensione massima del record Kafka
è di 1 000 000 byte, anche se parte di questa capacità viene utilizzata quando il bridge è configurato per eseguire
l'assegnazione della partizione Kafka in base all'identificativo Correlazione MQ o all'identificativo Gruppo MQ. Si consiglia di
inviare messaggi che non superino i 950 kilobyte utilizzando il bridge MQ.

Se il bridge MQ rileva un messaggio troppo grande da inoltrare a Kafka, il bridge elimina il
messaggio e scrive una voce di log nel dashboard Kibana. Valuta la possibilità di impostare l'attributo MAXMSGL
della coda MQ da cui il bridge riceve i messaggi in modo da impedire alle applicazioni MQ di inviare alla coda
i messaggi che non possono essere trasferiti utilizzando il bridge MQ.
