---

copyright:
  years: 2015, 2019
lastupdated: "2018-02-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Utilisation des outils de la console Kafka avec {{site.data.keyword.messagehub}}
{: #kafka_console_tools }

Apache Kafka est fourni avec un certain nombre d'outils de console pour des opération simples d'administration et de messagerie. Vous pouvez utiliser la plupart d'entre eux avec {{site.data.keyword.messagehub}}, même si ce dernier ne permet pas la connexion à son cluster ZooKeeper. Tels qu'ils ont été développés par Kafka, bon nombre des outils ne nécessitent plus dorénavant la connexion à ZooKeeper précédemment requise.
{: shortdesc}

Ces outils de console sont disponibles dans le répertoire <code>bin</code> de votre téléchargement Kafka. Par exemple, [Client Apache Kafka 0.10.2.X ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.apache.org/dyn/closer.cgi?path=/kafka/0.10.2.1/kafka_2.11-0.10.2.1.tgz){:new_window}.

Pour fournir les données d'identification SASL à ces outils, créez un fichier de propriétés à l'image de l'exemple suivant :

<pre>
<code>
  sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  security.protocol=SASL_SSL
  sasl.mechanism=PLAIN
  ssl.protocol=TLSv1.2
  ssl.enabled.protocols=TLSv1.2
  ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

Remplacez USER et PASSWORD par les valeurs issues de l'onglet **Données d'identification pour le service** de {{site.data.keyword.messagehub}} dans la console {{site.data.keyword.Bluemix_notm}}.


## Fournisseur de console
{: #console_producer }

Vous pouvez utiliser l'outil Fournisseur de console Kafka avec {{site.data.keyword.messagehub}}. Vous devez fournir une liste des données d'identification SASL et des courtiers.

Après avoir créé le fichier de propriétés comme indiqué précédemment, vous pouvez exécuter le fournisseur de console dans un terminal comme suit :

<pre>
<code>
  $ kafka-console-producer.sh --broker-list KAFKA_BROKERS_SASL --producer.config CONFIG_FILE --topic TOPIC_NAME
</code>
</pre>
{:codeblock}

Remplacez les variables de l'exemple par vos propres valeurs :
* KAFKA_BROKERS_SASL par la valeur de l'onglet **Données d'identification pour le service** de {{site.data.keyword.messagehub}} dans la console {{site.data.keyword.Bluemix_notm}}, sous forme de liste de paires hôte:port séparées par des virgules (par exemple, `hôte1:port1,hôte2:port2`). 
* CONFIG_FILE par le chemin d'accès au fichier de configuration. 

Vous pouvez utiliser bon nombre des autres options de cet outil, à l'exception de celles qui nécessitent un accès à ZooKeeper.


## Consommateur de console
{: #console_consumer }

Vous pouvez utiliser l'outil Consommateur de console Kafka avec {{site.data.keyword.messagehub}}. Vous devez fournir un serveur d'amorce et des données d'identification SASL.

Après avoir créé le fichier de propriétés comme indiqué précédemment, vous pouvez exécuter le consommateur de console dans un terminal comme suit :

<pre>
<code>
  $ kafka-console-consumer.sh --bootstrap-server KAFKA_BROKERS_SASL --consumer.config CONFIG_FILE --topic TOPIC_NAME 
</code>
</pre>
{:codeblock}

Remplacez les variables de l'exemple par vos propres valeurs :
* KAFKA_BROKERS_SASL par la valeur de l'onglet **Données d'identification pour le service** de {{site.data.keyword.messagehub}} dans la console {{site.data.keyword.Bluemix_notm}}, sous forme de liste de paires hôte:port séparées par des virgules (par exemple, `hôte1:port1,hôte2:port2`). 
* CONFIG_FILE par le chemin d'accès au fichier de configuration. 

Vous pouvez utiliser bon nombre des autres options de cet outil, à l'exception de celles qui nécessitent un accès à ZooKeeper.


## Groupes de consommateurs
{: #consumer_groups_tool }

Vous pouvez utiliser l'outil Groupes de consommateurs Kafka avec {{site.data.keyword.messagehub}}. Etant donné que {{site.data.keyword.messagehub}} ne permet pas la connexion à son cluster ZooKeeper, certaines options ne sont pas disponibles.

Après avoir créé le fichier de propriétés comme indiqué précédemment, vous pouvez exécuter les outils de groupes de consommateurs dans un terminal. Par exemple, répertorier les groupes de consommateurs comme suit :

<pre>
<code>
  $ kafka-consumer-groups.sh --bootstrap-server KAFKA_BROKERS_SASL --command-config CONFIG_FILE --list
</code>
</pre>
{:codeblock}

Remplacez les variables de l'exemple par vos propres valeurs :
* KAFKA_BROKERS_SASL par la valeur de l'onglet **Données d'identification pour le service** de {{site.data.keyword.messagehub}} dans la console {{site.data.keyword.Bluemix_notm}}, sous forme de liste de paires hôte:port séparées par des virgules (par exemple, `hôte1:port1,hôte2:port2`). 
* CONFIG_FILE par le chemin d'accès au fichier de configuration.

L'utilisation de cet outil permet également d'afficher des détails tels que les positions actuelles des consommateurs, leur décalage et l'identificateur de client de chaque partition d'un groupe. Par exemple :

<pre>
<code>
  $ kafka-consumer-groups.sh --bootstrap-server KAFKA_BROKERS_SASL --command-config CONFIG_FILE --describe --group GROUP
</code>
</pre>
{:codeblock}

Remplacez GROUP de l'exemple par le nom du groupe dont vous voulez extraire les détails. 


## Sujets
{: #topics_tool }

Vous ne pouvez pas utiliser l'outil Sujets Kafka, `kafka-topics`, avec {{site.data.keyword.messagehub}} car cet outil nécessite un accès à ZooKeeper.

Toutefois, vous pouvez administrer des sujets à l'aide du tableau de bord {{site.data.keyword.messagehub}} dans la console {{site.data.keyword.Bluemix_notm}} ou à l'aide de l'API REST.


## Réinitialisation de Kafka Streams
{: #kafka_streams_reset }

Vous pouvez utiliser cet outil avec {{site.data.keyword.messagehub}} pour réinitialiser l'état de traitement d'une application Kafka Streams, afin de traiter de nouveau son entrée à partir de zéro. Avant d'exécuter cet outil, vérifiez que votre application Streams est totalement arrêtée.

Par exemple :

<pre>
<code>
  $ kafka-streams-application-reset.sh --bootstrap-servers KAFKA_BROKERS_SASL --config-file CONFIG_FILE --application-id APP_ID
</code>
</pre>
{:codeblock}

Remplacez les variables de l'exemple par vos propres valeurs :
* KAFKA_BROKERS_SASL par la valeur de l'onglet **Données d'identification pour le service** de {{site.data.keyword.messagehub}} dans la console {{site.data.keyword.Bluemix_notm}}, sous forme de liste de paires hôte:port séparées par des virgules (par exemple, `hôte1:port1,hôte2:port2`). 
* CONFIG_FILE par le chemin d'accès au fichier de configuration. 
* APP_ID par l'ID de votre application Streams.

