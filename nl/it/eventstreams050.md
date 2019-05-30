---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-15"

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

I client Kafka sono disponibili in più lingue e forniamo istruzioni per alcune di queste lingue. Puoi utilizzarne altre, ma avrai bisogno del supporto SASL PLAIN per fornire le credenziali. Inoltre, se stai utilizzando il piano Enterprise, devi anche utilizzare l'estensione SNI (Server Name Indication) al protocollo TLSv1.2.

Per informazioni sull'utilizzo dell'API Kafka sul piano Classic, vedi [API Kafka - Classic](/docs/services/EventStreams?topic=eventstreams-kafka_using_classic).

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

			<td>**Requisiti di autenticazione**</td>
			<td>Il client deve supportare l'autenticazione utilizzando il meccanismo SASL Plain e utilizzare l'estensione SNI (Server Name Indication) al protocollo TLSv1.2</td>
			<td>Il client deve supportare l'autenticazione utilizzando il meccanismo SASL Plain e utilizzare l'estensione SNI (Server Name Indication) al protocollo TLSv1.2</td>
		</tr>

</table>

Per ulteriori informazioni sulle API Producer e Consumer V2.2, vedi
[Kafka Producer API 2.2 ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} e
[Kafka Consumer API 2.2 0 ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html){:new_window}. 


## Scelta di un client Kafka da utilizzare con {{site.data.keyword.messagehub}}
{: #kafka_clients}

Per utilizzare l'API Kafka con {{site.data.keyword.messagehub}}, scegli uno dei seguenti tipi di client:

* Il client Java ufficiale. È la scelta migliore perché contiene le funzioni più recenti disponibili per Apache Kafka.
* Uno dei [client di terze parti consigliati](/docs/services/EventStreams?topic=eventstreams-kafka_clients#clients_table).

Per entrambi i tipi di client, ti consigliamo di scegliere sempre la versione del client più recente. 

### Requisiti del client per il collegamento a Event Streams

Per il collegamento a {{site.data.keyword.messagehub}}, i client devono supportare l'autenticazione utilizzando il meccanismo SASL Plain e utilizzare l'estensione SNI (Server Name Indication) al protocollo TLSv1.2.

Il protocollo Kafka minimo che supportiamo è 0.10.

	
### Client di terze parti
{: #third_party_clients}

Se non puoi eseguire i client Java ufficiali, ti consigliamo di eseguire uno dei [client di terze parti consigliati](/docs/services/EventStreams?topic=eventstreams-kafka_clients#clients_table), che sono stati tutti ben sperimentati con {{site.data.keyword.messagehub}}. 
Potrebbero funzionare anche altri client di terze parti che supportano un insieme minimo di requisiti del client con {{site.data.keyword.messagehub}}. Tuttavia, noi eseguiamo test e abbiamo esperienza solo dei client di terze parti consigliati.

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
			<td colspan="3">**Client ufficiale**</td>
			</tr>
	  		<tr>
			<td>[Client Apache Kafka ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://kafka.apache.org/downloads)</td>
			<td>Java</td>
			<td>La più recente</td>
			<td>0.10.2 </td>
			<td>[Java console sample](/docs/services/EventStreams?topic=eventstreams-kafka_java_using)<br/>
			[Liberty sample ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-liberty-sample)
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
1. {: #footnote1}Questa versione è la meno recente che abbiamo convalidato nella verifica continua. Normalmente, questa è la versione iniziale disponibile negli ultimi 12 mesi, ma può essere aggiornata se si conosce che sono presenti dei problemi significativi.


<!--
## Unsupported clients

The following clients are not supported by {{site.data.keyword.messagehub}}:

### kafka-node
The kafka-node client does not fully support SASL authentication with the PLAIN mechanism so cannot currently be used with {{site.data.keyword.messagehub}}.


### no-kafka 
The no-kafka client does not fully support SASL authentication with the PLAIN mechanism so cannot currently be used with {{site.data.keyword.messagehub}}.

-->

<br/>
### Collegamento del tuo client a {{site.data.keyword.messagehub}}
{: #connect_client}

Per informazioni su come configurare il tuo client Java per il collegamento a {{site.data.keyword.messagehub}}, consulta [Configurazione del tuo client](/docs/services/EventStreams?topic=eventstreams-kafka_connect).

## Configurazione del tuo client API Kafka
{: #kafka_api_client}

Per stabilire una connessione, i client devono essere configurati per utilizzare almeno SASL_SSL PLAIN su TLSv1.2 e richiedere un nome utente e un elenco di server di avvio. 

Per richiamare il nome utente, la password e l'elenco di server di avvio, è necessario un oggetto delle credenziali del servizio o una chiave del servizio per l'istanza del servizio. Per ulteriori informazioni sulla creazione di questi oggetti, vedi <link to Connecting to event Streams>
[Connessione a {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting).

Da tali oggetti:
* Utilizza <code>kafka_brokers_sasl property</code> come elenco dei server di avvio. Formatta questo elenco come un elenco separato da virgole di voci host:port. Ad esempio, <code>host1:port1,host2:port2</code>. Ti consigliamo di includere i dettagli di tutti gli host elencati nella proprietà <code>kafka_brokers_sasl</code>.
* Utilizza le proprietà <code>user</code> e <code>api_key</code> come nome utente e password.

Per le istanze sul piano Classic, queste informazioni sono invece disponibili dalla variabile di ambiente VCAP_SERVICES della tua applicazione. Per ulteriori informazioni, vedi [Connessione a {{site.data.keyword.messagehub}} - Classic](/docs/services/EventStreams?topic=eventstreams-connecting_classic).

Per un client Java, il seguente esempio mostra la serie minima di proprietà, dove USERNAME, PASSWORD e KAFKA_BROKERS_SASL dovrebbero essere sostituiti dai valori richiamati precedentemente.

```
bootstrap.servers=KAFKA_BROKERS_SASL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
ssl.protocol=TLSv1.2
ssl.enabled.protocols=TLSv1.2
ssl.endpoint.identification.algorithm=HTTPS

# To send or receive messages, the following are also required
key.deserializer=org.apache.kafka.common.serialization.StringDeserializer
value.deserializer=org.apache.kafka.common.serialization.StringDeserializer
```
{: codeblock}

<br/>
Tieni presente che se stai utilizzando un client Kafka precedente alla 0.10.2.1, la proprietà <code>sasl.jaas.config</code> non è supportata e devi invece fornire la configurazione del client in un file di configurazione JAAS. 

### Connessione e autenticazione a un'applicazione diversa da Java
{: #kafka_notjava }

Qualsiasi client che supporti Kafka 0.10 con SASL PLAIN e TLSv1.2 dovrebbero funzionare con {{site.data.keyword.messagehub}}. 

Tieni presente che il client deve supportare l'estensione SNI per TLS dove il nome dell'host del server viene incluso nell'handshake TLS. 

I client di esempio sono i seguenti:
* [librdkafka ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/edenhill/librdkafka/){:new_window} 
* [confluent-kafka-python ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/confluentinc/confluent-kafka-python){:new_window} 




 




