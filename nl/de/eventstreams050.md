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

# Kafka-API verwenden
{: #kafka_using}

Mit Kafka steht eine umfassende Palette von APIs und Clients für eine Vielzahl verschiedener Sprachen zur Verfügung. Beispiel:

* **Kern-API von Kafka (Consumer-, Producer- und Admin-API)**<br/>
    Zum Senden und Empfangen von Nachrichten direkt von einem oder mehreren Kafka-Topics. 
* **Streams-API**<br/>
    Eine Streamverarbeitungs-API der höheren Ebene zur einfachen Verarbeitung, Umwandlung und Generierung von Ereignissen zwischen Topics. 
* **Connect-API**<br/>
    Ein Framework, das wiederverwendbaren oder Standardintegrationen das Streaming von Ereignissen in externe Systeme, wie z. B. Datenbanken, und aus diesen Systemen ermöglicht. 
* **KSQL**<br/>
    Eine Schnittstelle für die Verarbeitung und Verknüpfung von Ereignissen aus Topics mit einer SQL-ähnlichen Syntax. 

Die folgende Tabelle enthält eine Zusammenfassung der Verwendungsmöglichkeiten mit {{site.data.keyword.messagehub}}:

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
		<tr>
			<td>**Authentifizierungsanforderungen**</td>
			<td>Client muss Authentifizierung mithilfe des SASL Plain-Mechanismus unterstützen und die SNI-Erweiterung (Server Name Identification) für das TLSv1.2-Protokoll verwenden</td>
			<td>Client muss Authentifizierung mithilfe des SASL Plain-Mechanismus unterstützen und die SNI-Erweiterung (Server Name Identification) für das TLSv1.2-Protokoll verwenden</td>
		</tr>

</table>
<br/>
Informationen zur Verwendung der Kafka-API beim Plan "Classic" finden Sie unter [Kafka-API - Classic](/docs/services/EventStreams?topic=eventstreams-kafka_using_classic).


## Kafka-Client zur Verwendung mit {{site.data.keyword.messagehub}} auswählen
{: #kafka_clients}

Der offizielle Client für die Kafka-API ist in Java geschrieben und enthält die neuesten Features und Fehlerkorrekturen. Weitere Informationen zu dieser API finden Sie in [Kafka-Producer-API 2.2 ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} und
[Kafka-Consumer-API 2.2 ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html){:new_window}. 

Für andere Sprachen wird die Ausführung eines der folgenden Clients empfohlen. Diese wurden alle mit {{site.data.keyword.messagehub}} getestet. 

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
			<td colspan="3">**Offizieller Apache Kafka-Client**</td>
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
{: #footnote_clients notoc}
1. {: #footnote1 notoc}Hierbei handelt es sich um die früheste Version, die in kontinuierlichen Tests validiert wurde. Normalerweise ist dies ursprüngliche, innerhalb der letzten 12 Monate verfügbare Version, es kann jedoch auch eine neuere Version sein, falls signifikante Probleme bekannt sind.

<br/>
Wenn Sie keinen der aufgeführten Clients ausführen können, können Sie Clients anderer Anbieter verwenden, die die folgenden Mindestanforderungen erfüllen (z. B. [librdkafka ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/edenhill/librdkafka/){:new_window}).
* Unterstützt Kafka 0.10 oder höher.
* Kann eine Verbindung herstellen und eine Authentifizierung durchführen, indem SASL PLAIN mit TLSv1.2 verwendet wird. 
* Unterstützt die SNI-Erweiterungen für TLS, wobei der Hostname des Servers im TLS-Handshake enthalten ist. 
* Unterstützt Elliptic Curve Cryptography.
Die durchgeführten Test und die vorhandenen Erfahrungen beziehen sich jedoch ausschließlich auf die empfohlenen Clients anderer Anbieter.

In allen Fällen wird die neueste Version des Clients empfohlen. 

<br/>
### Clients mit {{site.data.keyword.messagehub}} verbinden
{: #connect_client}

Informationen zur Konfiguration des Java-Clients für die Verbindung mit {{site.data.keyword.messagehub}} finden Sie in [Client konfigurieren](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_api_client).

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
```
{: codeblock}

<br/>
Wenn Sie einen Kafka-Client vor Version 0.10.2.1 verwenden, beachten Sie, dass die Eigenschaft <code>sasl.jaas.config</code> nicht unterstützt wird und Sie stattdessen die Clientkonfiguration in einer JAAS-Konfigurationsdatei bereitstellen müssen. 








 




