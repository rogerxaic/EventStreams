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

# Choix d'un client Kafka pour une utilisation avec {{site.data.keyword.messagehub}}
{: #kafka_clients}

Pour utiliser l'API Kafka avec {{site.data.keyword.messagehub}}, choisissez un type de client parmi les suivants :

* Un client Java officiel de version 1.1 ou ultérieure, par exemple le [client Apache Kafka 1.1![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.apache.org/dyn/closer.cgi?path=/kafka/1.1.0/kafka_2.11-1.1.0.tgz){:new_window}.
	Le client Java officiel est le meilleur choix car il contient les dernières fonctions disponibles pour Apache Kafka.
* L'un des [clients tiers recommandés](/docs/services/EventStreams/eventstreams062.html#clients_table).

Pour ces deux types de clients, il est recommandé de toujours utiliser la dernière version du client. 

## Configuration requise au niveau des clients pour la connexion à Event Streams

Pour se connecter à {{site.data.keyword.messagehub}}, les clients doivent prendre en charge l'authentification via le mécanisme SASL Plain et utiliser l'extension SNI (Server Name Indication) au protocole TLSv1.2.

Le protocole Kafka minimum pris en charge est 0.10.

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
	

	
## Clients tiers
{: #third_party_clients}

Si vous ne pouvez pas exécuter les clients Java officiels, il est recommandé d'exécuter l'un des [clients tiers recommandés](/docs/services/EventStreams/eventstreams062.html#clients_table), qui sont tous parfaitement testés avec {{site.data.keyword.messagehub}}. 

<!--
* [sarama (Go) ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/Shopify/sarama){:new_window}
-->  

D'autres clients tiers prenant en charge les conditions minimales requises pour le client peuvent fonctionner avec {{site.data.keyword.messagehub}}. Cependant, nos tests et notre expérience sont limités à ces clients tiers recommandés. 

## Récapitulatif de la prise en charge de tous les clients recommandés
{: #client_summary}

<table id="clients_table">
    <caption>Tableau 2. Récapitulatif de la prise en charge des clients</caption>
      <tr>
		    <th>Client</th>
		    <th>Langage</th>
			<th>Version recommandée</th>
		    <th>Version min. prise en charge</th>
			<th>Lien vers un exemple</th>
        </tr>
			<tr>
			<td colspan="3">**Client officiel**</td>
			</tr>
	  		<tr>
			<td>[Client Apache Kafka 1.1 ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.apache.org/dyn/closer.cgi?path=/kafka/1.1.0/kafka_2.11-1.1.0.tgz)</td>
			<td>Java</td>
			<td>Dernière</td>
			<td>0.10.2</td>
			<td>[Exemple de console Java ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-console-sample)<br/>
			[Exemple pour Liberty ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-liberty-sample)
			</td>
			</tr>
			<tr>
			<td colspan="3">**Clients tiers**</td>
			</tr>
	  		<tr>
			<td>[node-rdkafka ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/Blizzard/node-rdkafka)</td>
			<td>Node.js</td>
			<td>Dernière</td>
			<td>2.3.3</td>
			<td>[Exemple pour Node.js ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-python ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/confluentinc/confluent-kafka-python)</td>
			<td>Python</td>
			<td>Dernière</td>
			<td>0.11.6</td>
			<td>[Exemple pour Kafka Python ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-python-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-go ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/confluentinc/confluent-kafka-go)</td>
			<td>Golang</td>
			<td>Dernière</td>
			<td>0.11.6</td>
			<td></td>
		</tr>
		<tr>
			<td>[librdkafka ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/edenhill/librdkafka)</td>
			<td>C ou C++</td>
			<td>Dernière</td>
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

## Connexion de votre client à {{site.data.keyword.messagehub}}
{: #connect_client}

Pour savoir comment configurer votre client Java pour se connecter à {{site.data.keyword.messagehub}}, voir [Configuration de votre client](/docs/services/EventStreams/eventstreams063.html).












