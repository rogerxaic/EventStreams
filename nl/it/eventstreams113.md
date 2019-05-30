---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-02"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Utilizzo di Kafka Connect con {{site.data.keyword.messagehub}}
{: #kafka_connect }

Puoi utilizzare Kafka Connect con {{site.data.keyword.messagehub}} e puoi eseguire i nodi di lavoro all'interno o all'esterno di {{site.data.keyword.Bluemix_short}}.
{: shortdesc}

Kafka Connect può essere eseguito in modalità distribuita o autonoma. La modalità autonoma è destinata per la verifica e per le connessioni temporanee tra i sistemi. La modalità distribuita è più appropriata per l'utilizzo nella produzione. La configurazione necessaria per utilizzare {{site.data.keyword.messagehub}} con queste due modalità è leggermente differente.
{:shortdesc}

## Configurazione del nodo di lavoro autonomo
{: #standalone_worker notoc}

Il nodo di lavoro autonomo non utilizza alcun argomento interno. Invece, utilizza un file per l'archiviazione delle informazioni di offset.

Devi fornire i server di avvio e le informazioni sulle credenziali SASL nel file delle proprietà del nodo di lavoro che fornisci quando avvii un nodo di lavoro autonomo Kafka Connect. Il seguente esempio elenca le proprietà che devi fornire al tuo file delle proprietà:

<pre>
<code>
  bootstrap.servers=KAFKA_BROKERS_SASL
  sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  security.protocol=SASL_SSL
  sasl.mechanism=PLAIN
  ssl.protocol=TLSv1.2
  ssl.enabled.protocols=TLSv1.2
  ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

Sostituisci KAFKA_BROKERS_SASL, USER e PASSWORD con i valori dalla tua scheda **Credenziali del servizio** {{site.data.keyword.messagehub}} nella console
{{site.data.keyword.Bluemix_notm}}.

### Connettore di origine
{: #source_connector notoc }

Il seguente esempio elenca le proprietà che devi fornire al tuo file delle proprietà:

<pre>
<code>
  bootstrap.servers=KAFKA_BROKERS_SASL
  producer.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  producer.security.protocol=SASL_SSL
  producer.sasl.mechanism=PLAIN
  producer.ssl.protocol=TLSv1.2
  producer.ssl.enabled.protocols=TLSv1.2
  producer.ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

Sostituisci KAFKA_BROKERS_SASL, USER e PASSWORD con i valori dalla tua scheda **Credenziali del servizio** {{site.data.keyword.messagehub}} nella console
{{site.data.keyword.Bluemix_notm}}.

### Connettore sink
{: #sink_connector }

Il seguente esempio elenca le proprietà che devi fornire al tuo file delle proprietà:

<pre>
<code>
  bootstrap.servers=KAFKA_BROKERS_SASL
  consumer.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  consumer.security.protocol=SASL_SSL
  consumer.sasl.mechanism=PLAIN
  consumer.ssl.protocol=TLSv1.2
  consumer.ssl.enabled.protocols=TLSv1.2
  consumer.ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

Sostituisci KAFKA_BROKERS_SASL, USER e PASSWORD con i valori dalla tua scheda **Credenziali del servizio** {{site.data.keyword.messagehub}} nella console
{{site.data.keyword.Bluemix_notm}}.

## Configurazione del nodo di lavoro distribuito
{: #distributed_worker notoc}

Devi fornire i server di avvio e le informazioni sulle credenziali SASL nel file delle proprietà che fornisci quando avvii i nodi di lavoro distribuiti Kafka Connect. Il seguente esempio elenca le proprietà che devi fornire al tuo file delle proprietà:

<pre>
<code>
  bootstrap.servers=KAFKA_BROKERS_SASL
  sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  security.protocol=SASL_SSL
  sasl.mechanism=PLAIN
  ssl.protocol=TLSv1.2
  ssl.enabled.protocols=TLSv1.2
  ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

Sostituisci KAFKA_BROKERS_SASL, USER e PASSWORD con i valori dalla tua scheda **Credenziali del servizio** {{site.data.keyword.messagehub}} nella console
{{site.data.keyword.Bluemix_notm}}.

Se desideri utilizzare un connettore di origine, devi specificare anche la configurazione SASL e SSL per il produttore nel seguente modo:

<pre>
<code>
  producer.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  producer.security.protocol=SASL_SSL
  producer.sasl.mechanism=PLAIN
  producer.ssl.protocol=TLSv1.2
  producer.ssl.enabled.protocols=TLSv1.2
  producer.ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

Se desideri utilizzare un connettore sink, devi specificare anche la configurazione SASL e SSL per il consumatore nel seguente modo:

<pre>
<code>
  consumer.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  consumer.security.protocol=SASL_SSL
  consumer.sasl.mechanism=PLAIN
  consumer.ssl.protocol=TLSv1.2
  consumer.ssl.enabled.protocols=TLSv1.2
  consumer.ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

In aggiunta, Kafka Connect in modalità distribuita utilizza tre argomenti internamente. Questi argomenti vengono creati automaticamente quando si avvia un nodo di lavoro, se utilizzi Kafka Connect in Apache Kafka versione 0.11 o successiva. Fornisci i nomi degli argomenti come parametri di configurazione. Assicurati che i valori siano uguali per tutti i nodi di lavoro con lo stesso valore di configurazione `group.id`.

| Configurazione               | Descrizione                                                         |
| --------------------------- | ------------------------------------------------------------------- |
| `offset.storage.topic`      | Argomento degli offset del connettore                                             |
| `offset.storage.partitions` | Numero di partizioni per l'argomento degli offset del connettore (valore predefinito 25) |
| `config.storage.topic`      | Argomento di configurazione del connettore                                       |
| `status.storage.topic`      | Argomento di stato del connettore                                              |
| `status.storage.partitions` | Numero di partizioni per l'argomento di stato del connettore (valore predefinito 5)          |

Ad esempio, puoi utilizzare le seguenti coppie chiave-valore nel tuo file delle proprietà:

<pre>
<code>
  offset.storage.topic=connect-offsets
  config.storage.topic=connect-configs
  status.storage.topic=connect-status
</code>
</pre>
{:codeblock}

Prendi in considerazione di diminuire il numero di partizioni se stai facendo un utilizzo minimo di Kafka Connect.



