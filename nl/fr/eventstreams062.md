---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-14"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Choix d'un client Kafka pour une utilisation avec {{site.data.keyword.messagehub}}
{: #kafka_clients}

Pour utiliser l'API Kafka avec {{site.data.keyword.messagehub}}, choisissez un type de client parmi les suivants :

* Le client Java officiel. Il s'agit du meilleur choix, car il contient les dernières fonctions disponibles pour Apache Kafka.
* L'un des [clients tiers recommandés](/docs/services/EventStreams?topic=eventstreams-kafka_clients#clients_table).

Pour ces deux types de clients, il est recommandé de toujours utiliser la dernière version du client. 
{: shortdesc}

## Configuration requise au niveau des clients pour la connexion à Event Streams

Pour se connecter à {{site.data.keyword.messagehub}}, les clients doivent prendre en charge l'authentification via le mécanisme SASL Plain et utiliser l'extension SNI (Server Name Indication) au protocole TLSv1.2.

Le protocole Kafka minimum pris en charge est 0.10.

	
## Clients tiers
{: #third_party_clients}

Si vous ne pouvez pas exécuter les clients Java officiels, il est recommandé d'exécuter l'un des [clients tiers recommandés](/docs/services/EventStreams?topic=eventstreams-kafka_clients#clients_table), qui sont tous parfaitement testés avec {{site.data.keyword.messagehub}}. 

D'autres clients tiers prenant en charge les conditions minimales requises pour le client peuvent fonctionner avec {{site.data.keyword.messagehub}}. Cependant, nos tests et notre expérience sont limités à ces clients tiers recommandés.

## Récapitulatif de la prise en charge de tous les clients recommandés
{: #client_summary}

<table id="clients_table">
    <caption>Tableau 2. Récapitulatif de la prise en charge des clients</caption>
      <tr>
		    <th id="client" scope="col">Client</th>
		    <th id="language" scope="col">Langage</th>
			<th id="version" scope="col">Version recommandée</th>
		    <th id="minimum version" scope="col">Version min. prise en charge [<sup>1</sup>](/docs/services/EventStreams?topic=eventstreams-kafka_clients#footnote1)</th>
			<th id="sample link" scope="col">Lien vers un exemple</th>
        </tr>
			<tr>
			<td colspan="3">**Client officiel**</td>
			</tr>
	  		<tr>
			<td>[Client Apache Kafka ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://kafka.apache.org/downloads)</td>
			<td>Java</td>
			<td>Dernière</td>
			<td>0.10.2 <p> Pour plus d'informations sur les clients plus anciens, voir [Compatibilité avec les versions antérieures](/docs/services/EventStreams?topic=eventstreams-kafka_clients_classic#compatibility_classic).</p></td>
			<td>[Exemple de console Java ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-console-sample)<br/>
			[Exemple Liberty ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-liberty-sample)
			</td>
			</tr>
			<tr>
			<td colspan="3">**Clients tiers**</td>
			</tr>
	  		<tr>
			<td>[node-rdkafka ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/Blizzard/node-rdkafka)</td>
			<td>Node.js</td>
			<td>Dernière</td>
			<td>2.2.2</td>
			<td>[Exemple pour Node.js ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-python ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/confluentinc/confluent-kafka-python)</td>
			<td>Python</td>
			<td>Dernière</td>
			<td>0.11.0</td>
			<td>[Exemple pour Kafka Python ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-python-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-go ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/confluentinc/confluent-kafka-go)</td>
			<td>Golang</td>
			<td>Dernière</td>
			<td>0.11.0</td>
			<td></td>
		</tr>
		<tr>
			<td>[librdkafka ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/edenhill/librdkafka)</td>
			<td>C ou C++</td>
			<td>Dernière</td>
			<td>0.11.0</td>
			<td></td>
		</tr>

</table>
### Note de bas de page
1. {: #footnote1}Cette version est la toute première qui a été validée en test continu. Typiquement, il s'agit de la version initiale disponible dans les 12 derniers mois mais cela peut changer si des problèmes significatifs sont identifiés.


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

Pour savoir comment configurer votre client Java pour se connecter à {{site.data.keyword.messagehub}}, voir [Configuration de votre client](/docs/services/EventStreams?topic=eventstreams-kafka_connect).












