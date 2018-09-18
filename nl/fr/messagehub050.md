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

# Utilisation de l'API Kafka
{: #kafka_using}

Si vous utilisez les clients Java, vous pouvez utiliser les clients Kafka 0.10.x ou ultérieurs disponibles.  

Les clients Kafka existent en plusieurs langages et nous fournissons des instructions pour certains d'entre eux. Vous pouvez en utiliser d'autres, mais vous aurez besoin d'une prise en charge SASL PLAIN pour les données d'identification. De plus, si vous utilisez le plan Enterprise, vous devrez également utiliser l'extension SNI (Service Name Identification) vers le protocole TLSv1.2.

<table>
    <caption>Tableau 1. Prise en charge du client Kafka dans les plans Standard et Enterprise</caption>
      <tr>
	        <th></th>
		    <th>Plan Standard</th>
		    <th>Plan Enterprise</th>
        </tr>
	  		<tr>
			<td>**Version Kafka sur le cluster**</td>
			<td>Kafka 0.10.2</td>
			<td>Kafka 1.1</td>
		</tr>
	  		<tr>
			<td>**Versions du client prises en charge**</td>
			<td>Kafka 0.10.x ou version ultérieure</td>
			<td>Kafka 0.10.x ou version ultérieure</td>
		</tr>
		<tr>
			<td>**Prise en charge de Kafka Connect, Kafka Streams et KSQL**</td>
			<td>Oui</td>
			<td>Oui</td>
		</tr>

			<td>**Exigences d'authentification**</td>
			<td>Le client doit prendre en charge l'authentification à l'aide du mécanisme SASL Plain</td>
			<td>Le client doit prendre en charge l'authentification à l'aide du mécanisme SASL Plain et utiliser l'extension SNI (Service Name Identification) vers le protocole TLSv1.2</td>
		</tr>

</table>

Pour plus d'informations sur les API producteur et consommateur, voir
[API producteur Kafka 0.11.0.X ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://kafka.apache.org/0110/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} et
[API consommateur Kafka 0.11.0.X ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://kafka.apache.org/0110/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html){:new_window}. 

