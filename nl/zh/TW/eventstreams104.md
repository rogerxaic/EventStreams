---

copyright:
  years: 2015, 2019
lastupdated: "2018-12-21"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 使用 Kafka Java client
{: #kafka_java_using}

<!-- 21/06/18 - removing until some content ready

## To do: instructions for getting started, with links for more information


## To do: simple send source and receive source in-line


## How to use, download, and run the Java Kafka API sample

-->

Java&trade; Kafka API 範例是以 Java 撰寫的範例生產者與消費者，它會直接使用 Kafka API。您可以在本端執行此範例，或是在 {{site.data.keyword.Bluemix_short}} 執行。
{: shortdesc}

範例程式碼位於 [event-streams-samples GitHub 專案 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-console-sample){:new_window}。雖然範例使用 Kafka API 來傳送及接收訊息，範例會使用 {{site.data.keyword.messagehub}} 管理 API 來建立與它傳送及接收訊息的主題。

如需設定及執行範例的相關資訊，請參閱 [README.md ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-console-sample){:new_window}。

如需如何執行範例的詳細逐步演練，請參閱[開始使用 {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-getting_started#getting_started_steps)。

## 如何使用、下載及執行 Liberty for Java 範例
{: #liberty_sample notoc}

Liberty for Java 範例會實作一個簡單的應用程式，部署至 Liberty 運行環境。應用程式使用 {{site.data.keyword.messagehub}} 的 Kafka API 來生產及取用訊息。應用程式也會提供 Web 前端，您可以用來進行管理。

範例程式碼位於 [event-streams-samples GitHub 專案 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-liberty-sample){:new_window}。

<!--
17/10/17 - Karen: following info duplicated at messagehub063 
-->

## 使用 sasl.jaas.config 內容
{: #sasl_prop notoc}
如果您使用 Kafka 用戶端 0.10.2.1 或更新版本，可以使用 <code>sasl.jaas.config</code> 內容進行用戶端配置，而不要使用 JAAS 檔案。若要連接至
{{site.data.keyword.messagehub}}，請如下所示設定 <code>sasl.jaas.config</code>：
<pre>
<code>    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
    username="USERNAME" \
    password="PASSWORD";</code>
</pre>
{:codeblock}

其中 USERNAME 及 PASSWORD 是來自 {{site.data.keyword.Bluemix_notm}} 中 {{site.data.keyword.messagehub}} **服務認證**標籤的值。

如果您使用 <code>sasl.jaas.config</code>，在相同 JVM 中執行的用戶端可以使用不同的認證。如需相關資訊，請參閱 [Configuring Kafka clients ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://kafka.apache.org/documentation/#security_sasl_plain_clientconfig){:new_window}。

若為較早的 Kafka 用戶端，您必須使用 JAAS 配置檔來指定認證。此機制較不方便，因此我們建議使用改用 <code>sasl.jaas.config</code> 內容。

<!--
23/04/18 - Karen: following migration info on production in messagehub084 
-->

## 將 Kafka 用戶端從 0.9.X 或 0.10.X 移轉或更新的用戶端版本
{: #kafka_migrate}


如果您使用 Java 用戶端，可以使用公開提供的 Kafka 用戶端 0.10 或更新版本。 

我們強烈鼓勵您從 0.9.X 移至最新的版本。您可以從 [https://kafka.apache.org/downloads ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://kafka.apache.org/downloads){:new_window} 下載 Kafka 用戶端。

如需使用 0.9.X 用戶端之影響的相關資訊，請參閱[舊版相容性](/docs/services/EventStreams?topic=eventstreams-kafka_clients#compatibility)。



### 將 Kafka 用戶端移轉至 0.10.2.X 或更新版本

從 0.10.2，您可以直接在用戶端的內容中配置 SASL 鑑別，而不使用 JAAS 檔案。這項簡化讓您能在相同的 JVM 中，使用不同的認證集來執行多個用戶端，使用 JAAS 檔案並無法做到。

請完成下列步驟：

1. 刪除 JAAS 檔案。請注意，JVM 內容 java.security.auth.login.config=<PATH TO JAAS> 也不再需要。
2. 如果您從 0.9.X 移轉，請刪除 {{site.data.keyword.messagehub}} 登入 jar 模組。
2. 將下列內容新增至用戶端的內容：
    ```
	   sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
    ```

	其中 USERNAME 及 PASSWORD 是來自 {{site.data.keyword.Bluemix_notm}} 中 {{site.data.keyword.messagehub}} **服務認證**標籤的值。
	
	

### 將 Kafka 用戶端從 0.9.X 移轉至 0.10.0.X 或 0.10.1.X

請完成下列步驟：

1. 刪除 {{site.data.keyword.messagehub}} 登入 jar 模組。
2. 將 <code>jaas.conf</code> 檔案變更如下：
    ```
        KafkaClient {
          org.apache.kafka.common.security.plain.PlainLoginModule required
          serviceName="kafka"
            username="USERNAME"
            password="PASSWORD";
        };
    ```
    {: codeblock}

	其中 USERNAME 及 PASSWORD 是來自 {{site.data.keyword.Bluemix_notm}} 中 {{site.data.keyword.messagehub}} **服務認證**標籤的值。
	
3. 將此行新增到您的消費者和生產者內容：<code>sasl.mechanism=PLAIN</code>
