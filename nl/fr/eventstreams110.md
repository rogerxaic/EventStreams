---

copyright:
  years: 2015, 2019
lastupdated: "2018-02-06"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Utilisation de KSQL avec {{site.data.keyword.messagehub}}
{: #ksql_using}

Vous pouvez utiliser [KSQL ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/confluentinc/ksql){:new_window} avec {{site.data.keyword.messagehub}} pour le traitement des flux. Vérifiez que vous utilisez KSQL version 0.4 ou une version ultérieure. 
{: shortdesc}

Procédez comme suit :

1. Créez un fichier de configuration KSQL {{site.data.keyword.messagehub}}.

    Exemple :
    ```
    bootstrap.servers=BOOTSTRAP_SERVERS
    application.id=ksql_server_quickstart
    ksql.command.topic.suffix=commands
    listeners=http://localhost:8080
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    ssl.protocol=TLSv1.2
    ssl.enabled.protocols=TLSv1.2
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
    ksql.sink.replications.default=3
    ```
    Où BOOTSTRAP_SERVERS, USERNAME et PASSWORD correspondent aux valeurs issues de l'onglet **Données d'identification pour le service** {{site.data.keyword.messagehub}} dans {{site.data.keyword.Bluemix_notm}}.

2. Utilisez le tableau de bord {{site.data.keyword.messagehub}} dans la console {{site.data.keyword.Bluemix_notm}} pour créer un sujet nommé <code>ksql__commands</code> avec une seule partition et la durée de conservation par défaut.
3. A partir d'un terminal Docker, démarrez KSQL avec la commande suivante :
<pre class="pre">/bin/ksql-cli local 
--<var class="keyword varname">properties-file</var> ./config/ksqlserver.properties
</pre>
4. Utilisez le tableau de bord {{site.data.keyword.messagehub}} dans la console {{site.data.keyword.Bluemix_notm}} pour créer deux sujets avec une partition chacun : <code>users</code> et <code>pageviews</code>.

    Puis deux fois démarrez <code>DataGen</code> comme suit :
	
    i. Avec <code>bootstrap-server=HOSTNAME:PORTNUMBER quickstart=users format=json topic=users maxInterval=10000</code> pour lancer la création d'événements <code>users</code>.
	
    ii. Avec <code>bootstrap-server=HOSTNAME:PORTNUMBER quickstart=pageviews format=delimited topic=pageviews maxInterval=10000</code> pour lancer la création d'événements <code>pageviews</code>.
	
	Vérifiez que vous avez inséré tous les hôtes Kafka répertoriés sur la page **Données d'identification pour le service** comme valeurs de <code>bootstrap-server</code>. Pour trouver ces informations, accédez à votre instance	{{site.data.keyword.messagehub}} dans {{site.data.keyword.Bluemix_notm}}, puis à l'onglet **Données d'identification pour le service** et sélectionnez les **Données d'identification** que vous voulez utiliser.

Une fois ces étapes accomplies, vous pouvez exécuter toutes les requêtes répertoriées dans le [Guide de démarrage rapide KSQL ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/confluentinc/ksql/tree/0.1.x/docs/quickstart#create-a-stream-and-table){:new_window}

