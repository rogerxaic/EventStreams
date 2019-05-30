---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-30"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}

# Configuration de votre client API Kafka
{: #kafka_api_client}

Pour se connecter à {{site.data.keyword.messagehub}}, l'API Kafka utilise l'un des ensembles de données d'identification suivants : 
* Les données d'identification <code>kafka_brokers_sasl</code> ainsi que <code>user</code> et <code>password</code> issus de la
[variable d'environnement VCAP_SERVICES](/docs/services/EventStreams?topic=eventstreams-connecting#connect_standard_cf).
* la clé de service. Pour plus d'informations, voir [Connexion à votre cluster](/docs/services/EventStreams?topic=eventstreams-connecting).
{: shortdesc}



Où USERNAME et PASSWORD sont les valeurs issues de l'onglet **Données d'identification pour le service** {{site.data.keyword.messagehub}} dans {{site.data.keyword.Bluemix_notm}}.

Si vous utilisez <code>sasl.jaas.config</code>, les clients exécutés dans la même machine virtuelle Java peuvent employer des données d'identification différentes. Pour plus d'informations, voir [Configuring Kafka clients ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://kafka.apache.org/documentation/#security_sasl_plain_clientconfig){:new_window}

L'exemple présente un fichier de configuration nommé <code>consumer.properties</code> :

```
key.deserializer=org.apache.kafka.common.serialization.StringDeserializer
value.deserializer=org.apache.kafka.common.serialization.StringDeserializer
#
client.id=kafka-java-console-sample-consumer
group.id=kafka-java-console-sample-group
#
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
ssl.protocol=TLSv1.2
ssl.enabled.protocols=TLSv1.2
ssl.endpoint.identification.algorithm=HTTPS
#
# please read the Kafka docs about this setting
auto.offset.reset=latest
```
{: codeblock}

<!--17/10/17 - Karen: following info duplicated at messagehub104 -->
## Utilisation de la propriété sasl.jaas.config (connexion et authentification dans une application Java )
{: #kafka_java notoc}
Si vous utilisez la version 0.10.2.1 ou une version ultérieure du client Kafka, vous pouvez employer la propriété <code>sasl.jaas.config</code> pour la configuration du client, à la place d'un fichier JAAS. Pour vous connecter à {{site.data.keyword.messagehub}}, définissez <code>sasl.jaas.config</code> comme suit :
<pre>
<code>    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
    username="USERNAME" \
    password="PASSWORD";</code>
</pre>
{:codeblock}

Dans le cas d'un client Kafka antérieur, vous devez utiliser un fichier de configuration JAAS pour spécifier les données d'identification. Ce mécanisme étant moins pratique, nous recommandons d'utiliser plutôt la propriété <code>sasl.jaas.config</code>.
## Connexion et authentification dans une application autre que Java
{: #kafka_notjava notoc}

Le service {{site.data.keyword.messagehub}} authentifie actuellement les clients à l'aide de SASL PLAIN via TLS. Les données d'identification sont transmises via une connexion cryptée.
Il s'agit d'une nouvelle fonction de Kafka 0.10.0.X. 

N'importe quel client prenant en charge Kafka 0.10 avec SASL PLAIN doit fonctionner avec {{site.data.keyword.messagehub}}. Voici des exemples de client :

* [librdkafka ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/edenhill/librdkafka/){:new_window} 
* [confluent-kafka-python ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/confluentinc/confluent-kafka-python){:new_window} 




