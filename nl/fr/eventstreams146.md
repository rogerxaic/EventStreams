---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-15"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Migration d'un client Kafka Java dans le plan Classic 
{: #kafka_java_migrating_classic}


## Migration d'un client Kafka Java de la version 0.9.X ou 0.10.X à une version client ultérieure
{: #kafka_migrate_classic}


Si vous utilisez les clients Java, vous pouvez utiliser
les clients Kafka 0.10 ou ultérieurs disponibles. 

Il est fortement conseillé de migrer la version 0.9.X vers la
version la plus récente. Vous pouvez télécharger un client Kafka depuis [https://kafka.apache.org/downloads ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://kafka.apache.org/downloads){:new_window}.

Pour plus d'informations sur les implications de l'utilisation d'un client 0.9.X, voir
[Compatibilité avec les versions antérieures](/docs/services/EventStreams?topic=eventstreams-kafka_clients#compatibility).



### Migration d'un client Kafka Java à 0.10.2.X ou à une version ultérieure

A compter de la version 0.10.2, vous pouvez configurer l'authentification SASL directement dans les propriétés du client au lieu d'utiliser un fichier JAAS. Cette simplification vous permet d'exécuter plusieurs clients dans la même machine virtuelle Java en utilisant différents jeux de données d'identification, ce qui est impossible avec un fichier JAAS.

Procédez comme suit :

1. Supprimez le fichier JAAS. La propriété JVM (machine virtuelle Java) java.security.auth.login.config=<PATH TO JAAS> n'est plus nécessaire.
2. Si vous effectuez une migration depuis 0.9.X, supprimez le module jar de connexion {{site.data.keyword.messagehub}}.
2. Ajoutez les éléments suivants aux propriétés du client :
    ```
	sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
	```

	Où USERNAME et PASSWORD sont les valeurs issues de l'onglet **Données d'identification pour le service** {{site.data.keyword.messagehub}} dans {{site.data.keyword.Bluemix_notm}}.
	
	

### Migration d'un client Kafka de la version 0.9.X à la version 0.10.0.X ou 0.10.1.X

Procédez comme suit :

1. Supprimez le module jar de connexion {{site.data.keyword.messagehub}}.
2. Modifiez le fichier <code>jaas.conf</code> comme suit :
    ```
        KafkaClient {
          org.apache.kafka.common.security.plain.PlainLoginModule required
          serviceName="kafka"
            username="USERNAME"
            password="PASSWORD";
        };
    ```
    {: codeblock}

	Où USERNAME et PASSWORD sont les valeurs issues de l'onglet **Données d'identification pour le service** {{site.data.keyword.messagehub}} dans {{site.data.keyword.Bluemix_notm}}.
	
3. Ajoutez cette ligne aux propriétés de consommateur et de producteur :
<code>sasl.mechanism=PLAIN</code>
