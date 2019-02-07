---

copyright:
  years: 2015, 2019
lastupdated: "2018-11-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Initiation à {{site.data.keyword.messagehub}} 
{: #getting_started}

Pour vous familiariser avec {{site.data.keyword.messagehub}}
et commencer à envoyer et à recevoir des messages, utilisez l'exemple Java™. Cet exemple montre comment
un producteur envoie des messages à un consommateur à l'aide d'un sujet. Le même
exemple de programme permet de consommer et de produire des messages.
{: shortdesc}

Pour mieux comprendre comment fonctionne {{site.data.keyword.messagehub}}, voir [A propos de {{site.data.keyword.messagehub}}](/docs/services/EventStreams/eventstreams010.html). {{site.data.keyword.messagehub}} était auparavant connu sous le nom de Message Hub.

Pour accéder à d'autres exemples de {{site.data.keyword.messagehub}}, y compris des exemples pour Node.js et Python, voir [Exemples de {{site.data.keyword.messagehub}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-messaging/event-streams-samples){:new_window}.

<!-- 11/01/18 - Karen - removing diagram as requested by James
![Java sample overview diagram](getting_started_sample.gif "Overview diagram of Java sample showing the flow of messages.")
-->

Procédez comme suit :
{: #getting_started_steps}
 
1. Créez une instance de service {{site.data.keyword.messagehub}} :

  a. Connectez-vous à la console {{site.data.keyword.Bluemix_notm}}. 
  
  b. Cliquez sur **Catalogue**.
  
  c. Dans la section **Intégration**, sélectionnez **Plan Standard {{site.data.keyword.messagehub}}**. La page de l'instance de service {{site.data.keyword.messagehub}} s'ouvre.
  
  d. Entrez un nom pour votre service. Vous pouvez utiliser la valeur par défaut.
  
  e. Cliquez sur **Créer**.

2. {: #create_credentials_step notoc} Pour créer des données d'identification {{site.data.keyword.messagehub}}, suivez les étapes décrites dans [Obtention des données d'identification et connexion à l'aide de la console IBM Cloud](/docs/services/EventStreams/eventstreams127.html#connect_standard_cf_console).
   <br/>
   <br/>Vous aurez besoin des valeurs de *kafka_brokers_sasl*, *kafka_admin_url* et *api_key* pour [l'étape 7](/docs/services/EventStreams/index.html#start_consumer_step) de cette tâche.   

3. Si ce n'est pas déjà fait, installez la configuration requise suivante :

    * [git ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://git-scm.com/){:new_window}
	* [Gradle ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://gradle.org/){:new_window}
    * Java 8 ou version ultérieure
 
4. Clonez le référentiel Git event-streams-samples en exécutant la commande suivante sur la ligne de commande :

    <pre class="pre">
    git clone https://github.com/ibm-messaging/event-streams-samples.git
    </pre>

5. Accédez au répertoire de l'exemple de console Java en exécutant la commande suivante :

    <pre class="pre">
    cd event-streams-samples/kafka-java-console-sample
    </pre>

6. Exécutez les commandes de génération suivantes :

    <pre class="pre">
    gradle clean && gradle build
    </pre>

7. {: #start_consumer_step notoc} Démarrez le consommateur sur votre console en exécutant la commande suivante :

    <pre class="pre">java -jar build/libs/kafka-java-console-sample-2.0-all.jar
	<var class="keyword varname">kafka_brokers_sasl</var> <var class="keyword varname">url_admin_kafka</var> token<var class="keyword varname">:api_key</var> -consumer</pre>
    
    Cet exemple utilise un sujet nommé `kafka-java-console-sample-topic`. Si le sujet n'existe pas déjà, l'exemple le crée à l'aide de l'API d'administration {{site.data.keyword.messagehub}}. Pour envoyer et recevoir des messages, l'exemple utilise l'API Java Apache Kafka.

    Utilisez les valeurs de *kafka_brokers_sasl*, *kafka_admin_url* et *api_key* des données d'identification créées à [l'étape 2](/docs/services/EventStreams/index.html#create_credentials_step).
	
	Indiquez <code>token</code> comme nom d'utilisateur et <var class="keyword varname">api_key</var> comme mot de passe. Séparez <code>token</code> et <var class="keyword varname">api_key</var> par une virgule.
    
	**Important :** *kafka_brokers_sasl* doit être une chaîne unique, encadrée de guillemets. Par exemple :

    <pre class="pre">
    "host1:port1,host2:port2"
    </pre>

    Il est conseillé d'utiliser tous les hôtes Kafka répertoriés dans les **Données d'identification** que vous avez sélectionnées.

8. Démarrez le producteur sur votre console en exécutant la commande suivante :
   
    <pre class="pre">java -jar build/libs/kafka-java-console-sample-2.0-all.jar
	<var class="keyword varname">kafka_brokers_sasl</var> <var class="keyword varname">kafka_admin_url</var> token<var class="keyword varname">:api_key</var> -producer</pre>
  
9. Vous devriez voir les messages envoyés par le producteur apparaître sur dans le consommateur. Voici
un exemple de sortie :

    ```
    [2018-07-02 14:54:50,788] INFO Running in local mode. (com.messagehub.samples.MessageHubConsoleSample)
    [2018-07-02 14:54:50,789] INFO Kafka Endpoints: kafka-0.mh-zarjkgtnzzspbkfrkqgdhmq.us-south.containers.appdomain.cloud:9093,kafka-1.mh-zarjkgtnzzspbkfrkqgdhmq.us-south.containers.appdomain.cloud:9093,kafka-2.mh-zarjkgtnzzspbkfrkqgdhmq.us-south.containers.appdomain.cloud:9093 (com.messagehub.samples.MessageHubConsoleSample)
    [2018-07-02 14:54:50,789] INFO Admin REST Endpoint: https://mh-zarjkgtnzzspbkfrkqgdhmq.us-south.containers.appdomain.cloud (com.messagehub.samples.MessageHubConsoleSample)
    [2018-07-02 14:54:50,789] INFO Creating the topic kafka-java-console-sample-topic (com.messagehub.samples.MessageHubConsoleSample)
    [2018-07-02 14:54:52,680] INFO Admin REST response : (com.messagehub.samples.MessageHubConsoleSample)
    [2018-07-02 14:54:53,351] INFO Admin REST Listing Topics: [{"name":"kafka-java-console-sample-topic","partitions":1,"retentionMs":86400000,"cleanupPolicy":"delete"},{"name":"__consumer_offsets","partitions":50,"retentionMs":86400000,"cleanupPolicy":"compact"}] (com.messagehub.samples.MessageHubConsoleSample)
    [2018-07-02 14:54:55,126] INFO [Partition(topic = kafka-java-console-sample-topic, partition = 0, leader = 0, replicas = [0,2,1], isr = [0,2,1], offlineReplicas = [])] (com.messagehub.samples.ConsumerRunnable)
    [2018-07-02 14:54:55,126] INFO class com.messagehub.samples.ConsumerRunnable is starting. (com.messagehub.samples.ConsumerRunnable)
    [2018-07-02 14:54:56,328] INFO [Partition(topic = kafka-java-console-sample-topic, partition = 0, leader = 0, replicas = [0,2,1], isr = [0,2,1], offlineReplicas = [])] (com.messagehub.samples.ProducerRunnable)
    [2018-07-02 14:54:56,328] INFO MessageHubConsoleSample will run until interrupted. (com.messagehub.samples.MessageHubConsoleSample)
    [2018-07-02 14:54:56,328] INFO class com.messagehub.samples.ProducerRunnable is starting. (com.messagehub.samples.ProducerRunnable)
    [2018-07-02 14:54:57,514] INFO Message produced, offset: 0 (com.messagehub.samples.ProducerRunnable)
    [2018-07-02 14:54:59,652] INFO Message produced, offset: 1 (com.messagehub.samples.ProducerRunnable)
    [2018-07-02 14:55:00,671] INFO No messages consumed (com.messagehub.samples.ConsumerRunnable)
    [2018-07-02 14:55:01,788] INFO Message produced, offset: 2 (com.messagehub.samples.ProducerRunnable)
    [2018-07-02 14:55:01,797] INFO Message consumed: ConsumerRecord(topic = kafka-java-console-sample-topic, partition = 0, offset = 2, CreateTime = 1530539701655, serialized key size = 3, serialized value size = 25, headers = RecordHeaders(headers = [], isReadOnly = false), key = key, value = This is a test message #2) (com.messagehub.samples.ConsumerRunnable)
    [2018-07-02 14:55:03,921] INFO Message consumed: ConsumerRecord(topic = kafka-java-console-sample-topic, partition = 0, offset = 3, CreateTime = 1530539703789, serialized key size = 3, serialized value size = 25, headers = RecordHeaders(headers = [], isReadOnly = false), key = key, value = This is a test message #3) (com.messagehub.samples.ConsumerRunnable)
    [2018-07-02 14:55:03,921] INFO Message produced, offset: 3 (com.messagehub.samples.ProducerRunnable)
    [2018-07-02 14:55:06,053] INFO Message consumed: ConsumerRecord(topic = kafka-java-console-sample-topic, partition = 0, offset = 4, CreateTime = 1530539705922, serialized key size = 3, serialized value size = 25, headers = RecordHeaders(headers = [], isReadOnly = false), key = key, value = This is a test message #4) (com.messagehub.samples.ConsumerRunnable)
    [2018-07-02 14:55:06,054] INFO Message produced, offset: 4 (com.messagehub.samples.ProducerRunnable)
    [2018-07-02 14:55:08,186] INFO Message consumed: ConsumerRecord(topic = kafka-java-console-sample-topic, partition = 0, offset = 5, CreateTime = 1530539708055, serialized key size = 3, serialized value size = 25, headers = RecordHeaders(headers = [], isReadOnly = false), key = key, value = This is a test message #5) (com.messagehub.samples.ConsumerRunnable)
    ```
	{: codeblock}
	
10. L'exemple s'exécute indéfiniment jusqu'à ce que vous l'arrêtiez. Pour arrêter le processus, exécutez une commande similaire à celle-ci : <code>Ctrl+C</code>

<!-- 07/06/18 - Karen: removing until a newer version available
To watch a video that walks
you through getting a Java sample to run against {{site.data.keyword.messagehub}}, see [{{site.data.keyword.messagehub}} - Getting started with IBM's Kafka in the cloud ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.youtube.com/watch?v=tt-bLtFzC_4){:new_window}.
-->



