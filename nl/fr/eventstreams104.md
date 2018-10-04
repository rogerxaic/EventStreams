---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-22"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Utilisation du client Java Kafka
{: #kafka_using}

<!-- 21/06/18 - removing until some content ready

## To do: instructions for getting started, with links for more information


## To do: simple send source and receive source in-line


## How to use, download, and run the Java Kafka API sample

-->

L'exemple d'API Java&trade; Kafka contient un producteur et un consommateur, codés en Java, qui utilisent directement l'API Kafka. Cet exemple peut être exécuté en local ou dans {{site.data.keyword.Bluemix_short}}.

L'exemple de code est disponible dans le [projet GitHub event-streams-samples ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-console-sample){:new_window}. Bien qu'il utilise l'API Kafka pour envoyer et recevoir des messages, il utilise l'API d'administration de {{site.data.keyword.messagehub}} pour créer le sujet auquel il en envoie et dont il en reçoit.

Pour plus d'informations sur la configuration et l'exécution de l'exemple, voir le fichier [README.md ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-console-sample){:new_window}.

Pour la procédure détaillée de l'exécution de l'exemple, voir [Initiation à {{site.data.keyword.messagehub}}](/docs/services/EventStreams/index.html#getting_started_steps).

## Utilisation, téléchargement et exécution de l'exemple Liberty for Java
{: #liberty_sample notoc}

L'exemple Liberty for Java implémente une application simple déployée dans le contexte d'exécution Liberty. L'application utilise l'API Kafka pour {{site.data.keyword.messagehub}} afin de produire et de consommer des messages.
Elle sert également un système frontal Web que vous pouvez utiliser pour l'administration.

L'exemple de code est disponible dans le [projet GitHub event-streams-samples![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-liberty-sample){:new_window}.

<!--
17/10/17 - Karen: following info duplicated at messagehub063 
-->

## Utilisation de la propriété sasl.jaas.config
{: #sasl_prop notoc}
Si vous utilisez la version 0.10.2.1 ou une version ultérieure du client Kafka, vous pouvez employer la propriété <code>sasl.jaas.config</code> pour la configuration du client, à la place d'un fichier JAAS. Pour vous connecter à {{site.data.keyword.messagehub}}, définissez <code>sasl.jaas.config</code> comme suit :
<pre>
<code>    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
    username="USERNAME" \
    password="PASSWORD";</code>
</pre>
{:codeblock}

Où USERNAME et PASSWORD sont les valeurs issues de l'onglet **Données d'identification pour le service** {{site.data.keyword.messagehub}} dans {{site.data.keyword.Bluemix_notm}}.

Si vous utilisez <code>sasl.jaas.config</code>, les clients exécutés dans la même machine virtuelle Java peuvent employer des données d'identification différentes. Pour plus d'informations, voir [Configuring Kafka clients ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://kafka.apache.org/documentation/#security_sasl_plain_clientconfig){:new_window}

Dans le cas d'un client Kafka antérieur, vous devez utiliser un fichier de configuration JAAS pour spécifier les données d'identification. Ce mécanisme étant moins pratique, nous recommandons d'utiliser plutôt la propriété <code>sasl.jaas.config</code>.

<!--
23/04/18 - Karen: following migration info on production in messagehub084 
-->

## Migration d'un client Kafka depuis la version 0.9.X ou 0.10.X vers des versions client ultérieures
{: #kafka_migrate}


Si vous utilisez les clients Java, vous pouvez utiliser
les clients Kafka 0.10 ou ultérieurs disponibles. Il est fortement conseillé de migrer la version 0.9.X vers la
version la plus récente. Vous pouvez télécharger un client Kafka depuis
[https://kafka.apache.org/downloads ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://kafka.apache.org/downloads){:new_window} 



### Migration d'un client Kafka vers la version 0.10.2.X ou ultérieure

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
	
	

### Migration d'un client Kafka depuis la version 0.9.X vers la version 0.10.0.X ou 0.10.1.X

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
