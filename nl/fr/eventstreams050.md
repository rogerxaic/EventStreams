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

# Utilisation de l'API Kafka
{: #kafka_using}

Les clients Kafka existent en plusieurs langages et nous fournissons des instructions pour certains d'entre eux. Vous pouvez en utiliser d'autres, mais vous aurez besoin d'une prise en charge SASL PLAIN pour les données d'identification. De plus, si vous utilisez le plan Enterprise, vous devrez également utiliser l'extension SNI (Server Name Indication) au protocole TLSv1.2.

Pour obtenir des informations sur l'utilisation de l'API Kafka dans le plan Classic, voir [API Kafka - Classic](/docs/services/EventStreams?topic=eventstreams-kafka_using_classic).

<table>
    <caption>Tableau 1. Prise en charge du client Kafka dans les plans Standard et Enterprise</caption>
      <tr>
	        <th></th>
		    <th>Plan Standard</th>
		    <th>Plan Enterprise</th>
        </tr>
	  		<tr>
			<td>**Version Kafka sur le cluster**</td>
			<td>Kafka 2.2</td>
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
			<td>Le client doit prendre en charge l'authentification à l'aide du mécanisme SASL Plain et utiliser l'extension SNI (Server Name Indication) au protocole TLSv1.2</td>
			<td>Le client doit prendre en charge l'authentification à l'aide du mécanisme SASL Plain et utiliser l'extension SNI (Server Name Indication) au protocole TLSv1.2</td>
		</tr>

</table>

Pour obtenir des informations sur les API Producer et Consumer V2.2, voir [Kafka Producer API 2.2 ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} et
[Kafka Consumer API 2.2 ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html){:new_window}. 


## Choix d'un client Kafka pour une utilisation avec {{site.data.keyword.messagehub}}
{: #kafka_clients}

Pour utiliser l'API Kafka avec {{site.data.keyword.messagehub}}, choisissez un type de client parmi les suivants :

* Le client Java officiel. Il s'agit du meilleur choix, car il contient les dernières fonctions disponibles pour Apache Kafka.
* L'un des [clients tiers recommandés](/docs/services/EventStreams?topic=eventstreams-kafka_clients#clients_table).

Pour ces deux types de clients, il est recommandé de toujours utiliser la dernière version du client. 

### Configuration requise au niveau des clients pour la connexion à Event Streams

Pour se connecter à {{site.data.keyword.messagehub}}, les clients doivent prendre en charge l'authentification via le mécanisme SASL Plain et utiliser l'extension SNI (Server Name Indication) au protocole TLSv1.2.

Le protocole Kafka minimum pris en charge est 0.10.

	
### Clients tiers
{: #third_party_clients}

Si vous ne pouvez pas exécuter les clients Java officiels, il est recommandé d'exécuter l'un des [clients tiers recommandés](/docs/services/EventStreams?topic=eventstreams-kafka_clients#clients_table), qui sont tous parfaitement testés avec {{site.data.keyword.messagehub}}. 
D'autres clients tiers prenant en charge les conditions minimales requises pour le client peuvent fonctionner avec {{site.data.keyword.messagehub}}. Cependant, nos tests et notre expérience sont limités à ces clients tiers recommandés.

### Récapitulatif de la prise en charge de tous les clients recommandés
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
			<td>0.10.2 </td>
			<td>[Exemple de console Java](/docs/services/EventStreams?topic=eventstreams-kafka_java_using)<br/>
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

<br/>
### Connexion de votre client à {{site.data.keyword.messagehub}}
{: #connect_client}

Pour savoir comment configurer votre client Java pour se connecter à {{site.data.keyword.messagehub}}, voir [Configuration de votre client](/docs/services/EventStreams?topic=eventstreams-kafka_connect).

## Configuration de votre client API Kafka
{: #kafka_api_client}

Pour établir une connexion, les clients doivent être configurés pour utiliser SASL_SSL PLAIN sur TLSv1.2 au minimum et pour nécessiter un nom d'utilisateur et une liste des serveurs d'amorce. 

Pour extraire le nom d'utilisateur, le mot de passe et une liste des serveurs d'amorce, un objet de données d'identification de service ou une clé de service est requise pour l'instance de service. Pour en savoir plus sur la création de ces objets, voir <link to Connecting to event Streams>
[Connexion à {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting).

Depuis ces objets :
* Utilisez la propriété <code>kafka_brokers_sasl</code> comme la liste des serveurs d'amorce. Formatez cette liste comme une liste d'entrées hôte:port séparée par des virgules. Par exemple, <code>host1:port1,host2:port2</code>. Il est recommandé d'inclure des détails pour tous les hôtes répertoriés dans la propriété <code>kafka_brokers_sasl</code>.
* Utilisez les propriétés <code>user</code> et <code>api_key</code> en tant que nom d'utilisateur et mot de passe

Pour les instances de services du plan Classic, vous trouverez ces informations dans la variable d'environnement VCAP_SERVICES de votre application. Pour plus d'informations, voir [Connexion à {{site.data.keyword.messagehub}} - Classic](/docs/services/EventStreams?topic=eventstreams-connecting_classic).

Pour un client Java, l'exemple suivant montre le jeu de propriétés minimum, où USERNAME, PASSWORD et KAFKA_BROKERS_SASL doivent être remplacés par les valeurs extraites antérieurement.

```
bootstrap.servers=KAFKA_BROKERS_SASL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
ssl.protocol=TLSv1.2
ssl.enabled.protocols=TLSv1.2
ssl.endpoint.identification.algorithm=HTTPS

# Pour envoyer ou recevoir des messages, vous avez également besoin de ce qui suit :
key.deserializer=org.apache.kafka.common.serialization.StringDeserializer
value.deserializer=org.apache.kafka.common.serialization.StringDeserializer
```
{: codeblock}

<br/>
Notez que si vous utilisez un client Kafka antérieur à la version 0.10.2.1, la propriété <code>sasl.jaas.config</code> n'est pas prise en charge et vous devrez à la place fournir la configuration client dans un fichier de configuration JAAS. 

### Connexion et authentification dans une application autre que Java
{: #kafka_notjava }

Les clients prenant en charge Kafka 0.10 avec SASL PLAIN et TLSv1.2 doivent travailler avec {{site.data.keyword.messagehub}}.

Notez que le client doit prendre en charge l'extension SNI à TLS où le nom d'hôte du serveur est inclus dans l'établissement de liaison TLS. 

Exemples de client :
* [librdkafka ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/edenhill/librdkafka/){:new_window} 
* [confluent-kafka-python ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/confluentinc/confluent-kafka-python){:new_window} 




 




