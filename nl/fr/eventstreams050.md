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

# Utilisation de l'API Kafka
{: #kafka_using}

Kafka fournit un ensemble enrichi d'interfaces API et de clients dans un large éventail de langues. Par exemple :

* **API principale de Kafka (API Consumer, Producer et Admin)**<br/>
    Permet d'envoyer et de recevoir des messages directement depuis une ou plusieurs rubriques Kafka.
* **API Streams**<br/>
    API de traitement de flux de niveau supérieur permettant de consommer, transformer et produire facilement des événements entre des rubriques.
* **API Connect**<br/>
    Infrastructure permettant des intégrations réutilisables ou standard dans des événements de flux circulant vers ou en dehors de systèmes externes, tels que des bases de données.
* **KSQL**<br/>
    Interface permettant de traiter et d'associer des événements à partir de rubriques en utilisant une syntaxe de type SQL.

Le tableau suivant récapitule ce que vous pouvez utiliser avec {{site.data.keyword.messagehub}} :

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
		<tr>
			<td>**Exigences d'authentification**</td>
			<td>Le client doit prendre en charge l'authentification à l'aide du mécanisme SASL Plain et utiliser l'extension SNI (Server Name Indication) au protocole TLSv1.2</td>
			<td>Le client doit prendre en charge l'authentification à l'aide du mécanisme SASL Plain et utiliser l'extension SNI (Server Name Indication) au protocole TLSv1.2</td>
		</tr>

</table>
<br/>
Pour obtenir des informations sur l'utilisation de l'API Kafka dans le plan Classic, voir [API Kafka - Classic](/docs/services/EventStreams?topic=eventstreams-kafka_using_classic).


## Choix d'un client Kafka pour une utilisation avec {{site.data.keyword.messagehub}}
{: #kafka_clients}

Le client officiel de l'API Kafka, qui est écrit en Java, contient les dernières fonctionnalités et les correctifs de bogue. Pour plus d'informations sur cette API, voir dans l'API kafka 2.2.0, [Class KafkaProducer<K,V> ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} et
[Class KafkaConsumer<K,V> ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html){:new_window}. 

Pour les autres langages, l'utilisation des clients ci-après est recommandée, car ils ont tous été testés avec {{site.data.keyword.messagehub}}.

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
			<td colspan="3">**Client officiel Apache Kafka**</td>
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
{: #footnote_clients notoc}
1. {: #footnote1 notoc}Cette version est la toute première qui a été validée en test continu. Typiquement, il s'agit de la version initiale disponible dans les 12 derniers mois mais cela peut changer si des problèmes significatifs sont identifiés.

<br/>
Si vous ne pouvez exécuter aucun des clients répertoriés, il vous est possible d'utiliser d'autres clients tiers répondant aux exigences minimales suivantes présentées ci-dessous ([librdkafka, par exemple). ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/edenhill/librdkafka/){:new_window}).
* Prise en charge de Kafka 0.10 ou version ultérieure
* Possibilité de se connecter et de s'authentifier en utilisant SASL PLAIN avec TLSv1.2
* Prise en charge des extensions SNI pour TLS dans lesquelles le nom d'hôte du serveur est inclus dans l'établissement de liaison TLS
* Prise en charge de la cryptographie de courbe elliptique. Notez cependant que seuls les clients tiers recommandés ont été testés et utilisés par nos soins.

Dans tous les cas, la version la plus récente du client est recommandée.

<br/>
### Connexion de votre client à {{site.data.keyword.messagehub}}
{: #connect_client}

Pour savoir comment configurer votre client Java pour se connecter à {{site.data.keyword.messagehub}}, voir [Configuration de votre client](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_api_client).

## Configuration de votre client API Kafka
{: #kafka_api_client}

Pour établir une connexion, les clients doivent être configurés pour utiliser SASL_SSL PLAIN sur TLSv1.2 au minimum et pour nécessiter un nom d'utilisateur et une liste des serveurs d'amorce. 

Pour extraire le nom d'utilisateur, le mot de passe et une liste des serveurs d'amorce, un objet de données d'identification de service ou une clé de service est requise pour l'instance de service. Pour en savoir plus sur la création de ces objets, voir <link to Connecting to event Streams>
[Connexion à {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting).

Depuis ces objets :
* Utilisez la propriété <code>kafka_brokers_sasl</code> comme la liste des serveurs d'amorce. Formatez cette liste comme une liste d'entrées hôte:port séparée par des virgules. Par exemple, <code>host1:port1,host2:port2</code>. Il est recommandé d'inclure des détails pour tous les hôtes répertoriés dans la propriété <code>kafka_brokers_sasl</code>.
* Utilisez les propriétés <code>user</code> et <code>api_key</code> en tant que nom d'utilisateur et mot de passe

Pour les instances de services du plan Classic, ces informations se trouvent plutôt dans la variable d'environnement VCAP_SERVICES de votre application. Pour plus d'informations, voir [Connexion à {{site.data.keyword.messagehub}} - Classic](/docs/services/EventStreams?topic=eventstreams-connecting_classic).

Pour un client Java, l'exemple suivant montre le jeu de propriétés minimum, où USERNAME, PASSWORD et KAFKA_BROKERS_SASL doivent être remplacés par les valeurs extraites antérieurement.

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
Notez que si vous utilisez un client Kafka antérieur à la version 0.10.2.1, la propriété <code>sasl.jaas.config</code> n'est pas prise en charge et vous devrez à la place fournir la configuration client dans un fichier de configuration JAAS. 








 




