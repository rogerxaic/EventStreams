---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Utilizzo degli strumenti della console Kafka con {{site.data.keyword.messagehub}}
{: #kafka_console_tools }

Apache Kafka viene fornito con molti strumenti della console per operazioni di messaggistica e gestione semplici. Puoi utilizzare molti di questi strumenti con {{site.data.keyword.messagehub}}, anche se {{site.data.keyword.messagehub}} non consente la connessione al proprio cluster ZooKeeper. Per come è stato sviluppato Kafka, molti degli strumenti che precedentemente richiedevano la connessione a ZooKeeper non hanno più questo requisito.
Puoi trovare questi strumenti della console nella directory <code>bin</code> del tuo download di Kafka. Ad esempio, [Apache Kafka 0.10.2.X client ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.apache.org/dyn/closer.cgi?path=/kafka/0.10.2.1/kafka_2.11-0.10.2.1.tgz){:new_window}.

Per fornire le credenziali SASL a questi strumenti, crea un file delle proprietà basato sul seguente esempio:

<pre>
<code>
  sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  security.protocol=SASL_SSL
  sasl.mechanism=PLAIN
  ssl.protocol=TLSv1.2
  ssl.enabled.protocols=TLSv1.2
  ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

Sostituisci USER e PASSWORD con i valori dalla tua scheda **Credenziali del servizio** {{site.data.keyword.messagehub}} nella console
{{site.data.keyword.Bluemix_notm}}.


## Produttore console
{: #console_producer }

Puoi utilizzare lo strumento produttore console con {{site.data.keyword.messagehub}}. Devi fornire un elenco di broker e le credenziali SASL.

Dopo aver creato il file delle proprietà come illustrato precedentemente, puoi eseguire il produttore console in un terminale nel seguente modo:

<pre>
<code>
  $ kafka-console-producer.sh --broker-list KAFKA_BROKERS_SASL --producer.config CONFIG_FILE --topic TOPIC_NAME
</code>
</pre>
{:codeblock}

Sostituisci le seguenti variabili nell'esempio con i tuoi propri valori:
* KAFKA_BROKERS_SASL con il valore dalla tua scheda **Credenziali del servizio** {{site.data.keyword.messagehub}} nella console {{site.data.keyword.Bluemix_notm}}, come un elenco di coppie host:porta separate da virgole (ad esempio, `host1:port1,host2:port2`). 
* CONFIG_FILE con il percorso del file di configurazione. 

Puoi utilizzare molte altre opzioni di questo strumento, con l'eccezione di quelle che richiedono l'accesso a ZooKeeper.


## Consumatore console
{: #console_consumer }

Puoi utilizzare lo strumento consumatore console con {{site.data.keyword.messagehub}}. Devi fornire un server di avvio e le credenziali SASL.

Dopo aver creato il file delle proprietà come illustrato precedentemente, puoi eseguire il consumatore console in un terminale nel seguente modo:

<pre>
<code>
  $ kafka-console-consumer.sh --bootstrap-server KAFKA_BROKERS_SASL --consumer.config CONFIG_FILE --topic TOPIC_NAME 
</code>
</pre>
{:codeblock}

Sostituisci le seguenti variabili nell'esempio con i tuoi propri valori:
* KAFKA_BROKERS_SASL con il valore dalla tua scheda **Credenziali del servizio** {{site.data.keyword.messagehub}} nella console {{site.data.keyword.Bluemix_notm}}, come un elenco di coppie host:porta separate da virgole (ad esempio, `host1:port1,host2:port2`). 
* CONFIG_FILE con il percorso del file di configurazione. 

Puoi utilizzare molte altre opzioni di questo strumento, con l'eccezione di quelle che richiedono l'accesso a ZooKeeper.


## Gruppi di consumatori
{: #consumer_groups_tool }

Puoi utilizzare lo strumento gruppi di consumatori Kafka con {{site.data.keyword.messagehub}}. Poiché {{site.data.keyword.messagehub}} non consente la connessione al proprio cluster ZooKeeper, alcune delle opzioni non sono disponibili.

Dopo aver creato il file delle proprietà come illustrato precedentemente, puoi eseguire lo strumento gruppi di consumatori in un terminale. Ad esempio, puoi elencare i gruppi di consumatori nel seguente modo:

<pre>
<code>
  $ kafka-consumer-groups.sh --bootstrap-server KAFKA_BROKERS_SASL --command-config CONFIG_FILE --list
</code>
</pre>
{:codeblock}

Sostituisci le seguenti variabili nell'esempio con i tuoi propri valori:
* KAFKA_BROKERS_SASL con il valore dalla tua scheda **Credenziali del servizio** {{site.data.keyword.messagehub}} nella console {{site.data.keyword.Bluemix_notm}}, come un elenco di coppie host:porta separate da virgole (ad esempio, `host1:port1,host2:port2`). 
* CONFIG_FILE con il percorso del file di configurazione.

Utilizzando questo strumento, puoi anche visualizzare dettagli come le posizioni correnti dei consumatori, il loro ritardo e l'ID client di ogni partizione di un gruppo. Ad esempio:

<pre>
<code>
  $ kafka-consumer-groups.sh --bootstrap-server KAFKA_BROKERS_SASL --command-config CONFIG_FILE --describe --group GROUP
</code>
</pre>
{:codeblock}

Sostituisci GROUP nell'esempio con un nome del gruppo per cui desideri richiamare i dettagli. 


## Argomenti
{: #topics_tool }

Non puoi utilizzare lo strumento degli argomenti Kafka `kafka-topics` con {{site.data.keyword.messagehub}} perché lo strumento richiede l'accesso a ZooKeeper.

Tuttavia, puoi gestire gli argomenti utilizzando il dashboard {{site.data.keyword.messagehub}} nella console {{site.data.keyword.Bluemix_notm}} o l'API REST.


## Reimpostazione di Kafka Streams
{: #kafka_streams_reset }

Puoi utilizzare questo strumento con {{site.data.keyword.messagehub}} per reimpostare lo stato di elaborazione di un'applicazione Kafka Streams, in modo da poterne rielaborare l'input da zero. Prima di eseguire questo strumento, assicurati che la tua applicazione Streams sia stata completamente arrestata.

Ad esempio:

<pre>
<code>
  $ kafka-streams-application-reset.sh --bootstrap-servers KAFKA_BROKERS_SASL --config-file CONFIG_FILE --application-id APP_ID
</code>
</pre>
{:codeblock}

Sostituisci le seguenti variabili nell'esempio con i tuoi propri valori:
* KAFKA_BROKERS_SASL con il valore dalla tua scheda **Credenziali del servizio** {{site.data.keyword.messagehub}} nella console {{site.data.keyword.Bluemix_notm}}, come un elenco di coppie host:porta separate da virgole (come, `host1:port1,host2:port2`). 
* CONFIG_FILE con il percorso del file di configurazione. 
* APP_ID con l'ID della tua applicazione Streams.

