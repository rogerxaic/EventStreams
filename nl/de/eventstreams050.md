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

# Kafka-API verwenden
{: #kafka_using}

Kafka-Clients sind in vielen Sprachen verfügbar. Anweisungen für einige dieser Sprachen werden von uns bereitgestellt. Wenn andere Kafka-Clients verwendet werden, ist Unterstützung für SASL PLAIN erforderlich, damit Berechtigungsnachweise bereitgestellt werden können. Wenn Sie mit dem Plan "Enterprise" arbeiten, müssen Sie darüber hinaus die SNI-Erweiterung (Server Name Identification) für das TLSv1.2-Protokoll verwenden.

Informationen zur Verwendung der Kafka-API beim Plan "Classic" finden Sie unter [Kafka-API - Classic](/docs/services/EventStreams?topic=eventstreams-kafka_using_classic).

<table>
    <caption>Tabelle 1. Unterstützung von Kafka-Clients in den Plänen "Standard" und "Enterprise"</caption>
      <tr>
	        <th></th>
		    <th>Plan "Standard"</th>
		    <th>Plan "Enterprise"</th>
        </tr>
	  		<tr>
			<td>**Kafka-Version in Cluster**</td>
			<td>Kafka 2.2</td>
			<td>Kafka 1.1</td>
		</tr>
	  		<tr>
			<td>**Unterstützte Clientversionen**</td>
			<td>Kafka 0.10.x oder höher</td>
			<td>Kafka 0.10.x oder höher</td>
		</tr>
		<tr>
			<td>**Werden Kafka Connect, Kafka Streams und KSQL unterstützt? **</td>
			<td>Ja</td>
			<td>Ja</td>
		</tr>

			<td>**Authentifizierungsanforderungen**</td>
			<td>Client muss Authentifizierung mithilfe des SASL Plain-Mechanismus unterstützen und die SNI-Erweiterung (Server Name Identification) für das TLSv1.2-Protokoll verwenden</td>
			<td>Client muss Authentifizierung mithilfe des SASL Plain-Mechanismus unterstützen und die SNI-Erweiterung (Server Name Identification) für das TLSv1.2-Protokoll verwenden</td>
		</tr>

</table>

Informationen zu den Producer- und Consumer-APIs Version 2.2 finden Sie in [Kafka-Producer-API 2.2 ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} und
[Kafka-Consumer-API 2.2 ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html){:new_window}. 


## Kafka-Client zur Verwendung mit {{site.data.keyword.messagehub}} auswählen
{: #kafka_clients}

Wenn Sie die Kafka-API mit {{site.data.keyword.messagehub}} verwenden möchten, wählen Sie einen der folgenden Clienttypen aus:

* Der offizielle Java-Client. Dieser stellt die am besten geeignete Option dar, da er die neuesten für Apache Kafka verfügbaren Features enthält.
* Einen der [empfohlenen Clients anderer Anbieter](/docs/services/EventStreams?topic=eventstreams-kafka_clients#clients_table).

Für beide Clienttypen sollte stets die neueste Clientversion ausgewählt werden. 

### Clientvoraussetzungen für die Verbindung mit Event Streams

Für eine Verbindung mit {{site.data.keyword.messagehub}} müssen Clients die Authentifizierung mit dem SASL Plain-Mechanismus unterstützen und die SNI-Erweiterung (Server Name Indication) für das TLSv1.2-Protokoll verwenden.

Die Mindestversion des Kafka-Protokolls, die unterstützt wird, ist 0.10.

	
### Clients anderer Anbieter
{: #third_party_clients}

Wenn Sie die offiziellen Java-Clients nicht ausführen können, sollten Sie einen der [empfohlenen Clients anderer Anbieter](/docs/services/EventStreams?topic=eventstreams-kafka_clients#clients_table) ausführen, die alle mit {{site.data.keyword.messagehub}} getestet wurden. 
Möglicherweise können weitere Clients anderer Anbieter, die die Mindestanforderungen für Clients unterstützen, mit {{site.data.keyword.messagehub}} eingesetzt werden. Die durchgeführten Test und die vorhandenen Erfahrungen beziehen sich jedoch ausschließlich auf die empfohlenen Clients anderer Anbieter.

### Zusammenfassung der Unterstützung für alle empfohlenen Clients
{: #client_summary}

<table id="clients_table">
    <caption>Tabelle 2. Zusammenfassung der Clientunterstützung</caption>
      <tr>
		    <th id="client" scope="col">Client</th>
		    <th id="language" scope="col">Sprache</th>
			<th id="version" scope="col">Empfohlene Version</th>
		    <th id="minimum version" scope="col">Unterstützte Mindestversion [<sup>1</sup>](/docs/services/EventStreams?topic=eventstreams-kafka_clients#footnote1)</th>
			<th id="sample link" scope="col">Link zum Beispiel</th>
        </tr>
			<tr>
			<td colspan="3">**Offizieller Client**</td>
			</tr>
	  		<tr>
			<td>[Apache Kafka-Client ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://kafka.apache.org/downloads)</td>
			<td>Java</td>
			<td>Neueste Version</td>
			<td>0.10.2 </td>
			<td>[Beispiel für Java-Konsole](/docs/services/EventStreams?topic=eventstreams-kafka_java_using)<br/>
			[Beispiel für Liberty ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-liberty-sample)
			</td>
			</tr>
			<tr>
			<td colspan="3">**Clients anderer Anbieter**</td>
			</tr>
	  		<tr>
			<td>[node-rdkafka ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/Blizzard/node-rdkafka)</td>
			<td>Node.js</td>
			<td>Neueste Version</td>
			<td>2.2.2</td>
			<td>[Beispiel für Node.js ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-python ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/confluentinc/confluent-kafka-python)</td>
			<td>Python</td>
			<td>Neueste Version</td>
			<td>0.11.0</td>
			<td>[Beispiel für Kafka Python ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-python-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-go ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/confluentinc/confluent-kafka-go)</td>
			<td>Golang</td>
			<td>Neueste Version</td>
			<td>0.11.0</td>
			<td></td>
		</tr>
		<tr>
			<td>[librdkafka ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/edenhill/librdkafka)</td>
			<td>C oder C++</td>
			<td>Neueste Version</td>
			<td>0.11.0</td>
			<td></td>
		</tr>

</table>
### Fußnote
1. {: #footnote1}Hierbei handelt es sich um die früheste Version, die in kontinuierlichen Tests validiert wurde. Normalerweise ist dies ursprüngliche, innerhalb der letzten 12 Monate verfügbare Version, es kann jedoch auch eine neuere Version sein, falls signifikante Probleme bekannt sind.


<!--
## Unsupported clients

The following clients are not supported by {{site.data.keyword.messagehub}}:

### kafka-node
The kafka-node client does not fully support SASL authentication with the PLAIN mechanism so cannot currently be used with {{site.data.keyword.messagehub}}.


### no-kafka 
The no-kafka client does not fully support SASL authentication with the PLAIN mechanism so cannot currently be used with {{site.data.keyword.messagehub}}.

-->

<br/>
### Clients mit {{site.data.keyword.messagehub}} verbinden
{: #connect_client}

Informationen zur Konfiguration des Java-Clients für die Verbindung mit {{site.data.keyword.messagehub}} finden Sie in [Client konfigurieren](/docs/services/EventStreams?topic=eventstreams-kafka_connect).

## Kafka-API-Client konfigurieren
{: #kafka_api_client}

Um eine Verbindung herstellen zu können, müssen Clients so konfiguriert werden, dass sie SASL_SSL PLAIN über TLSv1.2 als Minimum verwenden und einen Benutzernamen sowie eine Liste der Bootstrap-Server benötigen. 

Für das Abrufen des Benutzernamens, des Kennworts und der Liste der Bootstrap-Server ist ein Serviceberechtigungsnachweisobjekt oder ein Serviceschlüssel für die Serviceinstanz erforderlich. Weitere Informationen zum Erstellen dieser Objekte finden Sie in <link to Connecting to event Streams>
[Verbindung zu {{site.data.keyword.messagehub}} herstellen](/docs/services/EventStreams?topic=eventstreams-connecting).

Aus diesen Objekten:
* Verwenden Sie die Eigenschaft <code>kafka_brokers_sasl</code> als Liste der Bootstrap-Server. Formatieren Sie diese Liste als eine durch Kommas getrennte Liste mit Host:Port-Einträgen. Beispiel: <code>host1:port1,host2:port2</code>. Es wird empfohlen, alle Hosts zu einzubeziehen, die in der Eigenschaft <code>kafka_brokers_sasl</code> aufgelistet sind.
* Verwenden Sie die Eigenschaften <code>user</code> und <code>api_key</code> als Benutzername und Kennwort

Für Serviceinstanzen im Plan "Classic" sind diese Informationen stattdessen in der Umgebungsvariablen VCAP_SERVICES Ihrer Anwendung verfügbar. Weitere Informationen hierzu finden Sie in [Verbindung zu {{site.data.keyword.messagehub}} herstellen - Classic](/docs/services/EventStreams?topic=eventstreams-connecting_classic).

Für einen Java-Client zeigt das folgende Beispiel die minimale Gruppe von Eigenschaften an, wobei USERNAME, PASSWORD und KAFKA_BROKERS_SASL durch die zuvor abgerufenen Werte ersetzt werden sollten.

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
Wenn Sie einen Kafka-Client vor Version 0.10.2.1 verwenden, beachten Sie, dass die Eigenschaft <code>sasl.jaas.config</code> nicht unterstützt wird und Sie stattdessen die Clientkonfiguration in einer JAAS-Konfigurationsdatei bereitstellen müssen. 

### Herstellen einer Verbindung und Authentifizierung in einer Anwendung, bei der es sich nicht um eine Java-Anwendung handelt
{: #kafka_notjava }

Jeder Client, der Kafka 0.10 mit SASL PLAIN und TLSv1.2 unterstützt, sollte auch mit {{site.data.keyword.messagehub}} verwendet werden können.

Beachten Sie, dass der Client die SNI-Erweiterung für TLS unterstützen muss, bei der der Hostname des Servers im TLS-Handshake einbezogen wird. 

Dies gilt beispielsweise für die folgenden Clients:
* [librdkafka ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/edenhill/librdkafka/){:new_window} 
* [confluent-kafka-python ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/confluentinc/confluent-kafka-python){:new_window} 




 




