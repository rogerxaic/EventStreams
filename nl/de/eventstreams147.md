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

# Kafka-API beim Plan "Classic" verwenden
{: #kafka_using_classic}

Kafka-Clients sind in vielen Sprachen verfügbar. Anweisungen für einige dieser Sprachen werden von uns bereitgestellt. Wenn andere Kafka-Clients verwendet werden, ist Unterstützung für SASL PLAIN erforderlich, damit Berechtigungsnachweise bereitgestellt werden können. Wenn Sie mit dem Plan "Enterprise" arbeiten, müssen Sie darüber hinaus die SNI-Erweiterung (Server Name Identification) für das TLSv1.2-Protokoll verwenden.

<table>
    <caption>Tabelle 1. Unterstützung von Kafka-Clients in den Plänen "Standard" und "Enterprise"</caption>
      <tr>
	        <th></th>
		    <th>Plan "Classic"</th>
		    
        </tr>
	  		<tr>
			<td>**Kafka-Version in Cluster**</td>
			<td>Kafka 1.1</td>
					</tr>
	  		<tr>
			<td>**Unterstützte Clientversionen**</td>
			<td>Kafka 0.10.x oder höher</td>
		</tr>
		<tr>
			<td>**Werden Kafka Connect, Kafka Streams und KSQL unterstützt? **</td>
			<td>Ja</td>
		</tr>

			<td>**Authentifizierungsanforderungen**</td>
			<td>Client muss Authentifizierung mithilfe des SASL Plain-Mechanismus unterstützen und die SNI-Erweiterung (Server Name Identification) für das TLSv1.2-Protokoll verwenden</td>
				</tr>

</table>

Informationen zu den Producer- und Consumer-APIs Version 1.1 finden Sie in
[Kafka Producer API 1.1 ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://kafka.apache.org/11/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} und
[Kafka Consumer API 1.1 ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://kafka.apache.org/11/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html){:new_window}. 












