---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-16"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Kafka-Client zur Verwendung mit {{site.data.keyword.messagehub}} auswählen
{: #kafka_clients}

Wenn Sie die Kafka-API mit {{site.data.keyword.messagehub}} verwenden möchten, wählen Sie einen der folgenden Clienttypen aus:

* Einen offiziellen Java-Client der Version 1.1 oder höher, z. B. [Apache Kafka 1.1-Client ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.apache.org/dyn/closer.cgi?path=/kafka/1.1.0/kafka_2.11-1.1.0.tgz){:new_window}.
	Der offizielle Java-Client stellt die am besten geeignete Option dar, da er die neuesten für Apache Kafka verfügbaren Features enthält.
* Einen der [empfohlenen Clients anderer Anbieter](/docs/services/EventStreams/eventstreams062.html#clients_table).

Für beide Clienttypen sollte stets die neueste Clientversion ausgewählt werden. 

## Clientvoraussetzungen für die Verbindung mit Event Streams

Für eine Verbindung mit {{site.data.keyword.messagehub}} müssen Clients die Authentifizierung mit dem SASL Plain-Mechanismus unterstützen und die SNI-Erweiterung (Server Name Indication) für das TLSv1.2-Protokoll verwenden.

Die Mindestversion des Kafka-Protokolls, die unterstützt wird, ist 0.10.

<!--
## Support summary for the official Apache Kafka client (Java)

<table>
    <caption>Table 1. Kafka client support in Standard and Enterprise plans</caption>
      <tr>
	        <th></th>
		    <th>Standard and Enterprise Plans</th>
		    <th></th>
        </tr>
	  		<tr>
			<td>**Kafka version on cluster**</td>
			<td>Kafka 1.1</td>
		</tr>
	  		<tr>
			<td>**Supported client versions**</td>
			<td>Kafka 1.1, or later</td>
		</tr>
			<td>**Authentication requirements**</td>
			<td>Client must support authentication using the SASL Plain mechanism and use the Server Name Indication (SNI) extension to the TLSv1.2 protocol</td>
		</tr>

</table>
-->

<!--
* [Apache Kafka 0.11.0.X client ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.apache.org/dyn/closer.cgi?path=/kafka/0.11.0.1/kafka_2.11-0.11.0.1.tgz){:new_window}
* [Apache Kafka 0.10.2.X client ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.apache.org/dyn/closer.cgi?path=/kafka/0.10.2.1/kafka_2.11-0.10.2.1.tgz){:new_window} 
-->
	

	
## Clients anderer Anbieter
{: #third_party_clients}

Wenn Sie die offiziellen Java-Clients nicht ausführen können, sollten Sie einen der [empfohlenen Clients anderer Anbieter](/docs/services/EventStreams/eventstreams062.html#clients_table) ausführen, die alle mit {{site.data.keyword.messagehub}} getestet wurden. 

<!--
* [sarama (Go) ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/Shopify/sarama){:new_window}
-->  

Möglicherweise können weitere Clients anderer Anbieter, die die Mindestanforderungen für Clients unterstützen, mit {{site.data.keyword.messagehub}} eingesetzt werden. Die durchgeführten Test und die vorhandenen Erfahrungen beziehen sich jedoch ausschließlich auf die empfohlenen Clients anderer Anbieter. 

## Zusammenfassung der Unterstützung für alle empfohlenen Clients
{: #client_summary}

<table id="clients_table">
    <caption>Tabelle 2. Zusammenfassung der Clientunterstützung</caption>
      <tr>
		    <th>Client</th>
		    <th>Sprache</th>
			<th>Empfohlene Version</th>
		    <th>Unterstützte Mindestversion</th>
			<th>Link zum Beispiel</th>
        </tr>
			<tr>
			<td colspan="3">**Offizieller Client**</td>
			</tr>
	  		<tr>
			<td>[Apache Kafka 1.1-Client ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.apache.org/dyn/closer.cgi?path=/kafka/1.1.0/kafka_2.11-1.1.0.tgz)</td>
			<td>Java</td>
			<td>Neueste Version</td>
			<td>0.10.2</td>
			<td>[Beispiel für Java-Konsole ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-console-sample)<br/>
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
			<td>2.3.3</td>
			<td>[Beispiel für Node.js ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-python ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/confluentinc/confluent-kafka-python)</td>
			<td>Python</td>
			<td>Neueste Version</td>
			<td>0.11.6</td>
			<td>[Beispiel für Kafka Python ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-python-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-go ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/confluentinc/confluent-kafka-go)</td>
			<td>Golang</td>
			<td>Neueste Version</td>
			<td>0.11.6</td>
			<td></td>
		</tr>
		<tr>
			<td>[librdkafka ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/edenhill/librdkafka)</td>
			<td>C oder C++</td>
			<td>Neueste Version</td>
			<td>0.11.6</td>
			<td></td>
		</tr>

</table>

<!--
## Unsupported clients

The following clients are not supported by {{site.data.keyword.messagehub}}:

### kafka-node
The kafka-node client does not fully support SASL authentication with the PLAIN mechanism so cannot currently be used with {{site.data.keyword.messagehub}}.


### no-kafka 
The no-kafka client does not fully support SASL authentication with the PLAIN mechanism so cannot currently be used with {{site.data.keyword.messagehub}}.

-->

## Clients mit {{site.data.keyword.messagehub}} verbinden
{: #connect_client}

Informationen zur Konfiguration des Java-Clients für die Verbindung mit {{site.data.keyword.messagehub}} finden Sie in [Client konfigurieren](/docs/services/EventStreams/eventstreams063.html).












