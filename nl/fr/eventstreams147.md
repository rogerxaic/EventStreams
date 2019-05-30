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

# Utilisation de l'API Kafka dans le plan Classic
{: #kafka_using_classic}

Les clients Kafka existent en plusieurs langages et nous fournissons des instructions pour certains d'entre eux. Vous pouvez en utiliser d'autres, mais vous aurez besoin d'une prise en charge SASL PLAIN pour les données d'identification. De plus, si vous utilisez le plan Enterprise, vous devrez également utiliser l'extension SNI (Server Name Indication) au protocole TLSv1.2.

<table>
    <caption>Tableau 1. Prise en charge du client Kafka dans les plans Standard et Enterprise</caption>
      <tr>
	        <th></th>
		    <th>Plan Classic</th>
		    
        </tr>
	  		<tr>
			<td>**Version Kafka sur le cluster**</td>
			<td>Kafka 1.1</td>
					</tr>
	  		<tr>
			<td>**Versions du client prises en charge**</td>
			<td>Kafka 0.10.x ou version ultérieure</td>
		</tr>
		<tr>
			<td>**Prise en charge de Kafka Connect, Kafka Streams et KSQL**</td>
			<td>Oui</td>
		</tr>

			<td>**Exigences d'authentification**</td>
			<td>Le client doit prendre en charge l'authentification à l'aide du mécanisme SASL Plain et utiliser l'extension SNI (Server Name Indication) au protocole TLSv1.2</td>
				</tr>

</table>

Pour obtenir des informations sur les API Producer et Consumer de version 1.1, voir
[Kafka Producer API 1.1 ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://kafka.apache.org/11/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} et
[Kafka Consumer API 1.1 ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://kafka.apache.org/11/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html){:new_window}. 












