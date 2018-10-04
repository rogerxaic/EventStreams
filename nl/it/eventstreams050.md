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

# Utilizzo dell'API Kafka
{: #kafka_using}

Se stai utilizzando i client Java, puoi usare i client Kafka pubblicamente disponibili alla 0.10.x o successiva. 

I client Kafka sono disponibili in più lingue e forniamo istruzioni per alcune di queste lingue. Puoi utilizzarne altre, ma avrai bisogno del supporto SASL PLAIN per fornire le credenziali. Inoltre, se stai utilizzando il piano Enterprise, devi anche utilizzare l'estensione SNI (Server Name Indication) al protocollo TLSv1.2.

<table>
    <caption>Tabella 1. Supporto del client Kafka nei piani Standard ed Enterprise</caption>
      <tr>
	        <th></th>
		    <th>Piano Standard</th>
		    <th>Piano Enterprise</th>
        </tr>
	  		<tr>
			<td>**Versione Kafka sul cluster **</td>
			<td>Kafka 0.10.2</td>
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
			<td>Il client deve supportare l'autenticazione utilizzando il meccanismo SASL Plain</td>
			<td>Il client deve supportare l'autenticazione utilizzando il meccanismo SASL Plain e utilizzare l'estensione SNI (Server Name Indication) al protocollo TLSv1.2</td>
		</tr>

</table>

Per ulteriori informazioni sulle API Producer e Consumer, vedi
[Kafka Producer API 0.11.0.X ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://kafka.apache.org/0110/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} e
[Kafka Consumer API 0.11.0.X ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://kafka.apache.org/0110/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html){:new_window}. 

