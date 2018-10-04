---

copyright:
  years: 2015, 2018
lastupdated: "2017-11-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Utilisation de Kafka Connect avec {{site.data.keyword.messagehub}}
{: #kafka_connect }

Vous pouvez utiliser Kafka Connect avec {{site.data.keyword.messagehub}} et exécuter les agents dans ou en dehors de {{site.data.keyword.Bluemix_short}}.

Kafka Connect peut s'exécuter en mode autonome ou distribué. Le mode autonome est destiné aux tests et aux connexions temporaires entre systèmes. Le mode distribué est plus approprié pour une utilisation en production. La configuration nécessaire varie légèrement selon que vous voulez utiliser {{site.data.keyword.messagehub}} avec l'un ou l'autre mode.
{:shortdesc}

## Configuration d'un agent autonome
{: #standalone_worker notoc}

Vous devez indiquer les serveurs d'amorce et les données d'identification SASL dans le fichier de propriétés de l'agent que vous fournissez lorsque vous démarrez un agent autonome Kafka Connect.

L'agent autonome n'utilise pas de sujet interne. A la place, il utilise un fichier pour stocker les informations de décalage.

### Connecteur source
{: #source_connector notoc }

L'exemple suivant répertorie les propriétés que vous devez fournir dans votre fichier de propriétés :

<pre>
<code>
  bootstrap.servers=KAFKA_BROKERS_SASL
  producer.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  producer.security.protocol=SASL_SSL
  producer.sasl.mechanism=PLAIN
  producer.ssl.protocol=TLSv1.2
  producer.ssl.enabled.protocols=TLSv1.2
  producer.ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

Remplacez KAFKA_BROKERS_SASL, USER et PASSWORD par les valeurs issues de l'onglet **Données d'identification pour le service** de {{site.data.keyword.messagehub}} dans la console {{site.data.keyword.Bluemix_notm}}.

### Connecteur du collecteur
{: #sink_connector }

L'exemple suivant répertorie les propriétés que vous devez fournir dans votre fichier de propriétés :

<pre>
<code>
  bootstrap.servers=KAFKA_BROKERS_SASL
  consumer.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  consumer.security.protocol=SASL_SSL
  consumer.sasl.mechanism=PLAIN
  consumer.ssl.protocol=TLSv1.2
  consumer.ssl.enabled.protocols=TLSv1.2
  consumer.ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

Remplacez KAFKA_BROKERS_SASL, USER et PASSWORD par les valeurs issues de l'onglet **Données d'identification pour le service** de {{site.data.keyword.messagehub}} dans la console {{site.data.keyword.Bluemix_notm}}.

## Configuration d'agent distribué
{: #distributed_worker notoc}

Vous devez indiquer les serveurs d'amorce et les données d'identification SASL dans le fichier de propriétés que vous fournissez lorsque vous démarrez des agents distribués Kafka Connect. L'exemple suivant répertorie les propriétés que vous devez fournir dans votre fichier de propriétés :

<pre>
<code>
  bootstrap.servers=KAFKA_BROKERS_SASL
  sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  security.protocol=SASL_SSL
  sasl.mechanism=PLAIN
  ssl.protocol=TLSv1.2
  ssl.enabled.protocols=TLSv1.2
  ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

Remplacez KAFKA_BROKERS_SASL, USER et PASSWORD par les valeurs issues de l'onglet **Données d'identification pour le service** de {{site.data.keyword.messagehub}} dans la console {{site.data.keyword.Bluemix_notm}}.

Si vous voulez utiliser un connecteur source, vous devez également indiquer la configuration SASL et SSL du fournisseur, comme suit :

<pre>
<code>
  producer.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  producer.security.protocol=SASL_SSL
  producer.sasl.mechanism=PLAIN
  producer.ssl.protocol=TLSv1.2
  producer.ssl.enabled.protocols=TLSv1.2
  producer.ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

Si vous voulez utiliser un connecteur de collecteur, vous devez également indiquer la configuration SASL et SSL du consommateur, comme suit :

<pre>
<code>
  consumer.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  consumer.security.protocol=SASL_SSL
  consumer.sasl.mechanism=PLAIN
  consumer.ssl.protocol=TLSv1.2
  consumer.ssl.enabled.protocols=TLSv1.2
  consumer.ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

De plus, en mode distribué, Kafka Connect utilise trois sujets en interne. Ces sujets sont créés automatiquement au démarrage d'un agent si vous utilisez Kafka Connect dans Apache Kafka version 0.11 ou ultérieure. Vous indiquez les noms de sujets sous forme de paramètres de configuration. Vérifiez que les valeurs sont identiques pour tous les agents ayant la même valeur de configuration `group.id`.

| Configuration               | Description                                                         |
| --------------------------- | ------------------------------------------------------------------- |
| `offset.storage.topic`      | Sujet Décalages de connecteur                                             |
| `offset.storage.partitions` | Sujet Nombre de partitions pour les décalages de connecteur (par défaut, 25) |
| `config.storage.topic`      | Sujet Configuration de connecteur                                       |
| `status.storage.topic`      | Sujet Statut de connecteur                                              |
| `status.storage.partitions` | Sujet Nombre de partitions pour les statuts de connecteur (par défaut, 5)          |

Par exemple, vous pouvez utiliser les paires clé-valeur suivantes dans votre fichier de propriétés :

<pre>
<code>
  offset.storage.topic=connect-offsets
  config.storage.topic=connect-configs
  status.storage.topic=connect-status
</code>
</pre>
{:codeblock}

Envisagez de réduire le nombre de partitions si vous utilisez peu Kafka Connect.



