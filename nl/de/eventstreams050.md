---

copyright:
  years: 2015, 2018
lastupdated: "2018-07-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Kafka-API verwenden
{: #kafka_using}

Wenn Sie mit den Java-Clients arbeiten, können Sie die allgemein zugänglichen Kafka-Clients der Version 0.10.x oder höher verwenden. 

Kafka-Clients sind in vielen Sprachen verfügbar. Anweisungen für einige dieser Sprachen werden von uns bereitgestellt. Wenn andere Kafka-Clients verwendet werden, ist Unterstützung für SASL PLAIN erforderlich, damit Berechtigungsnachweise bereitgestellt werden können. Wenn Sie mit dem Plan "Enterprise" arbeiten, müssen Sie darüber hinaus die SNI-Erweiterung (Server Name Identification) für das TLSv1.2-Protokoll verwenden.

<table>
    <caption>Tabelle 1. Unterstützung von Kafka-Clients in den Plänen "Standard" und "Enterprise"</caption>
      <tr>
	        <th></th>
		    <th>Plan "Standard"</th>
		    <th>Plan "Enterprise"</th>
        </tr>
	  		<tr>
			<td>**Kafka-Version in Cluster**</td>
			<td>Kafka 0.10.2</td>
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
			<td>Client muss Authentifizierung mithilfe des SASL Plain-Mechanismus unterstützen</td>
			<td>Client muss Authentifizierung mithilfe des SASL Plain-Mechanismus unterstützen und die SNI-Erweiterung (Server Name Identification) für das TLSv1.2-Protokoll verwenden</td>
		</tr>

</table>

Weitere Informationen zu den Producer- und Consumer-APIs finden Sie unter
[Kafka Producer API 0.11.0.X ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://kafka.apache.org/0110/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} und
[Kafka Consumer API 0.11.0.X ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://kafka.apache.org/0110/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html){:new_window}. 

