---

copyright:
  years: 2015, 2018
lastupdated: "2018-07-13"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Einführung in Message Hub 
{: #messagehub}

Als Einführung in die Verwendung von {{site.data.keyword.messagehub}} zum
Senden und Empfangen von Nachrichten können Sie das bereitgestellte Java™-Beispiel verwenden. Das
Beispiel verdeutlicht, wie Nachrichten unter Verwendung eines Topics von einem Producer an einen
Consumer gesendet werden. Das gleiche Beispielprogramm wird verwendet, um Nachrichten zu verarbeiten und
Nachrichten zu erstellen.

Weitere Informationen zur Funktionsweise von {{site.data.keyword.messagehub}} finden Sie im Abschnitt [Was ist {{site.data.keyword.messagehub}}?](/docs/services/MessageHub/messagehub010.html).

Um auf andere {{site.data.keyword.messagehub}}-Beispiele zuzugreifen, beispielsweise auf "Node.js" und "Python" siehe die [{{site.data.keyword.messagehub}}-Beispiele ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-messaging/message-hub-samples){:new_window}.

<!-- 11/01/18 - Karen - removing diagram as requested by James
![Java sample overview diagram](getting_started_sample.gif "Overview diagram of Java sample showing the flow of messages.")
-->

Führen Sie die folgenden Schritte aus:
{: #getting_started_steps}
 
1. Erstellen Sie eine {{site.data.keyword.messagehub}}-Serviceinstanz:

  a. Melden Sie sich über die Webbenutzerschnittstelle bei {{site.data.keyword.Bluemix_notm}} an. 
  
  b. Klicken Sie auf **KATALOG**.
  
  c. Wählen Sie im Abschnitt **Anwendungsservices** den **{{site.data.keyword.messagehub}}-Plan "Standard"** aus. Die Seite für die {{site.data.keyword.messagehub}}-Serviceinstanz wird geöffnet.
  
  d. Lassen Sie den Service im Menü **Verbindung herstellen zu** ohne Bindung und geben Sie Namen für Ihren Service und Ihre Berechtigungsnachweise ein. Sie können die Standardwerte verwenden.
  
  e. Klicken Sie auf **Erstellen**.

2. Installieren Sie die folgenden vorausgesetzten Komponenten (falls noch nicht vorhanden):

    * [git ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://git-scm.com/){:new_window}
	* [Gradle ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://gradle.org/){:new_window}
    * Java 8 oder höher
 
3. Klonen Sie das Git-Repository 'message-hub-samples', indem Sie den folgenden Befehl über die Befehlszeile ausführen:

    <pre class="pre">
    git clone https://github.com/ibm-messaging/message-hub-samples.git
    </pre>
	{: codeblock}

4. Wechseln Sie in das Verzeichnis mit dem Beispiel für die Java-Konsole, indem Sie den folgenden Befehl ausführen:

    <pre class="pre">
    cd message-hub-samples/kafka-java-console-sample
    </pre>
	{: codeblock}

5. Führen Sie die folgenden Erstellungsbefehle aus:

    <pre class="pre">
    gradle clean && gradle build
    </pre>
	{: codeblock}

6. Starten Sie den Consumer in Ihrer Konsole, indem Sie den folgenden Befehl ausführen:

    <pre class="pre">java -jar build/libs/kafka-java-console-sample-2.0.jar 
	<var class="keyword varname">kafka_brokers_sasl</var> <var class="keyword varname">kafka_admin_url</var> token<var class="keyword varname">:api_key</var> -consumer</pre>
    {: codeblock}
    
    In dem Beispiel wird ein Topic mit dem Namen `kafka-java-console-sample-topic` verwendet. Wenn das Topic noch nicht
    vorhanden ist, wird es in dem Beispiel mithilfe der {{site.data.keyword.messagehub}}-Verwaltungs-API erstellt. Zum Senden und Empfangen
    von Nachrichten wird in dem Beispiel die Apache Kafka-Java-API verwendet.

    Um die Werte für *kafka_brokers_sasl*, *kafka_admin_url*
    und *api_key* zu ermitteln, wechseln Sie in Ihre {{site.data.keyword.messagehub}}-Instanz in {{site.data.keyword.Bluemix_notm}}, navigieren Sie zur Registerkarte **Serviceberechtigungsnachweise** und wählen Sie die **Berechtigungsnachweise** aus, die Sie verwenden möchten.
	
	Geben Sie <code>token</code> als Ihren Benutzernamen an und den <var class="keyword varname">api_key</var> als Kennwort. Trennen Sie <code>token</code> und den <var class="keyword varname">api_key</var> durch einen Doppelpunkt voneinander.
    
	**Wichtig:** *kafka_brokers_sasl* muss eine einzige Zeichenfolge sein, die in Anführungszeichen angegeben ist. Beispiel:

    <pre class="pre">
    "host1:port1,host2:port2"
    </pre>
	{: codeblock}

    Es wird empfohlen, alle Kafka-Hosts zu verwenden, die in den von Ihnen ausgewählten **Berechtigungsnachweisen** aufgelistet sind.

7. Starten Sie den Producer in Ihrer Konsole, indem Sie den folgenden Befehl ausführen:
   
    <pre class="pre">java -jar build/libs/kafka-java-console-sample-2.0.jar 
	<var class="keyword varname">kafka_brokers_sasl</var> <var class="keyword varname">kafka_admin_url</var> token<var class="keyword varname">:api_key</var> -producer</pre>
 {: codeblock}
  
8. Nun müssten die vom Producer gesendeten Nachrichten im Consumer angezeigt werden. Im Folgenden ist eine
Beispielausgabe aufgelistet:

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
	
9. Der Beispielprozess wird fortgesetzt, bis Sie ihn stoppen. Um den Prozess zu stoppen, führen Sie einen
Befehl wie den folgenden aus: <code>Strg+C</code>

<!-- 07/06/18 - Karen: removing until a newer version available
To watch a video that walks
you through getting a Java sample to run against {{site.data.keyword.messagehub}}, see [{{site.data.keyword.messagehub}} - Getting started with IBM's Kafka in the cloud ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.youtube.com/watch?v=tt-bLtFzC_4){:new_window}.
-->



