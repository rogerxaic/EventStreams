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
{:note: .note}

# Utilizzo dell'API Kafka
{: #kafka_using}

Kafka fornisce un'ampia gamma di API e client in una vasta gamma di linguaggi. Ad esempio:

* **API principali di Kafka (API Consumer, Producer e Admin)**<br/>
    Utilizza per inviare e ricevere i messaggi direttamente da uno o più argomenti Kafka.
* **API Streams**<br/>
    Un'API di elaborazione flussi di livello superiore per utilizzare, trasformare e creare facilmente degli eventi tra gli argomenti.
* **API Connect**<br/>
    Un framework che consente integrazioni riutilizzabili o standard per inviare in flusso gli eventi in entrata e in uscita di sistemi esterni, ad esempio i database.
* **KSQL**<br/>
    Un'interfaccia per l'elaborazione e l'unione di eventi dagli argomenti utilizzando una sintassi simile a SQL.

La seguente tabella riepiloga cosa puoi utilizzare con {{site.data.keyword.messagehub}}:

<table>
    <caption>Tabella 1. Supporto del client Kafka nei piani Standard ed Enterprise</caption>
      <tr>
	        <th></th>
		    <th>Piano Standard</th>
		    <th>Piano Enterprise</th>
        </tr>
	  		<tr>
			<td>**Versione Kafka sul cluster **</td>
			<td>Kafka 2.2</td>
			<td>Kafka 1.1</td>
		</tr>
	  		<tr>
			<td>**Versioni client supportate**</td>
			<td>Kafka 0.10.x o successiva</td>
			<td>Kafka 0.10.x o successiva</td>
		</tr>
		<tr>
			<td>**Kafka Connect, Kafka Streams e KSQL supportati? **</td>
			<td>Sì</td>
			<td>Sì</td>
		</tr>
		<tr>
			<td>**Requisiti di autenticazione**</td>
			<td>Il client deve supportare l'autenticazione utilizzando il meccanismo SASL Plain e utilizzare l'estensione SNI (Server Name Indication) al protocollo TLSv1.2</td>
			<td>Il client deve supportare l'autenticazione utilizzando il meccanismo SASL Plain e utilizzare l'estensione SNI (Server Name Indication) al protocollo TLSv1.2</td>
		</tr>

</table>
<br/>
Per informazioni sull'utilizzo dell'API Kafka sul piano Classic, vedi [API Kafka - Classic](/docs/services/EventStreams?topic=eventstreams-kafka_using_classic).


## Scelta di un client Kafka da utilizzare con {{site.data.keyword.messagehub}}
{: #kafka_clients}

Il client ufficiale per l'API Kafka è scritto in Java e di conseguenza contiene le funzioni e le correzioni di bug più recenti. Per informazioni su questa API, vedi [Kafka Producer API 2.2 ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} e
[Kafka Consumer API 2.2 ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html){:new_window}. 

Per gli altri linguaggi, ti consigliamo di eseguire uno dei seguenti client, ognuno dei quali è stato ben verificato con {{site.data.keyword.messagehub}}.

### Riepilogo del supporto per tutti i client consigliati
{: #client_summary}

<table id="clients_table">
    <caption>Tabella 2. Riepilogo del supporto client</caption>
      <tr>
		    <th id="client" scope="col">Client</th>
		    <th id="language" scope="col">Linguaggio</th>
			<th id="version" scope="col">Versione consigliata</th>
		    <th id="minimum version" scope="col">Versione minima supportata [<sup>1</sup>](/docs/services/EventStreams?topic=eventstreams-kafka_clients#footnote1)</th>
			<th id="sample link" scope="col">Link all'esempio</th>
        </tr>
			<tr>
			<td colspan="3">**Client Apache Kafka ufficiale**</td>
			</tr>
	  		<tr>
			<td>[Client Apache Kafka ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://kafka.apache.org/downloads)</td>
			<td>Java</td>
			<td>La più recente</td>
			<td>0.10.2 </td>
			<td>[Esempio console Java](/docs/services/EventStreams?topic=eventstreams-kafka_java_using)<br/>
			[Esempio Liberty ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-liberty-sample)
			</td>
			</tr>
			<tr>
			<td colspan="3">**Client di terze parti**</td>
			</tr>
	  		<tr>
			<td>[node-rdkafka ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/Blizzard/node-rdkafka)</td>
			<td>Node.js</td>
			<td>La più recente</td>
			<td>2.2.2</td>
			<td>[Esempio Node.js ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-python ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/confluentinc/confluent-kafka-python)</td>
			<td>Python</td>
			<td>La più recente</td>
			<td>0.11.0</td>
			<td>[Esempio Kafka Python ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-python-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-go ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/confluentinc/confluent-kafka-go)</td>
			<td>Golang</td>
			<td>La più recente</td>
			<td>0.11.0</td>
			<td></td>
		</tr>
		<tr>
			<td>[librdkafka ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/edenhill/librdkafka)</td>
			<td>C o C++</td>
			<td>La più recente</td>
			<td>0.11.0</td>
			<td></td>
		</tr>

</table>
### Nota a piè di pagina
{: #footnote_clients notoc}
1. {: #footnote1 notoc}Questa versione è la meno recente che abbiamo convalidato nella verifica continua. Normalmente, questa è la versione iniziale disponibile negli ultimi 12 mesi, ma può essere aggiornata se si conosce che sono presenti dei problemi significativi.

<br/>
Se non puoi eseguire alcuno dei client elencati, puoi utilizzare altri client di terze parti che soddisfano i seguenti requisiti minimi (ad esempio, [librdkafka ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/edenhill/librdkafka/){:new_window}).
* Supporta Kafka 0.10 o successiva
* Può eseguire la connessione e l'autenticazione utilizzando SASL PLAIN con TLSv1.2
* Supporta le estensioni SNI per TLS dove il nome host del server è incluso nell'handshake TLS.
* Supporta la crittografia a curva ellittica
Tuttavia, noi eseguiamo test e abbiamo esperienza solo dei client di terze parti consigliati.

In tutti i casi, si consiglia la versione più recente del client.

<br/>
### Collegamento del tuo client a {{site.data.keyword.messagehub}}
{: #connect_client}

Per informazioni su come configurare il tuo client Java per il collegamento a {{site.data.keyword.messagehub}}, consulta [Configurazione del tuo client](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_api_client).

## Configurazione del tuo client API Kafka
{: #kafka_api_client}

Per stabilire una connessione, i client devono essere configurati per utilizzare almeno SASL_SSL PLAIN su TLSv1.2 e richiedere un nome utente e un elenco di server di avvio. 

Per richiamare il nome utente, la password e l'elenco di server di avvio, è necessario un oggetto delle credenziali del servizio o una chiave del servizio per l'istanza del servizio. Per ulteriori informazioni sulla creazione di questi oggetti, vedi <link to Connecting to event Streams>
[Connessione a {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting).

Da tali oggetti:
* Utilizza <code>kafka_brokers_sasl property</code> come elenco dei server di avvio. Formatta questo elenco come un elenco separato da virgole di voci host:port. Ad esempio, <code>host1:port1,host2:port2</code>. Ti consigliamo di includere i dettagli di tutti gli host elencati nella proprietà <code>kafka_brokers_sasl</code>.
* Utilizza le proprietà <code>user</code> e <code>api_key</code> come nome utente e password.

Per le istanze del servizio sul piano Classic, queste informazioni sono invece disponibili dalla variabile di ambiente VCAP_SERVICES della tua applicazione.Per ulteriori informazioni, vedi [Connessione a {{site.data.keyword.messagehub}} - Classic](/docs/services/EventStreams?topic=eventstreams-connecting_classic).

Per un client Java, il seguente esempio mostra la serie minima di proprietà, dove USERNAME, PASSWORD e KAFKA_BROKERS_SASL dovrebbero essere sostituiti dai valori richiamati precedentemente.

```
bootstrap.servers=KAFKA_BROKERS_SASL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
ssl.protocol=TLSv1.2
ssl.enabled.protocols=TLSv1.2
ssl.endpoint.identification.algorithm=HTTPS
```
{: codeblock}

<br/>
Tieni presente che se stai utilizzando un client Kafka precedente alla 0.10.2.1, la proprietà <code>sasl.jaas.config</code> non è supportata e devi invece fornire la configurazione del client in un file di configurazione JAAS. 








 




