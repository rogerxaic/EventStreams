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

# 在經典方案上移轉 Kafka Java 用戶端  
{: #kafka_java_migrating_classic}


## 將 Kafka Java 用戶端從 0.9.X 或 0.10.X 移轉到更新的用戶端版本
{: #kafka_migrate_classic}


如果您使用 Java 用戶端，可以使用公開提供的 Kafka 用戶端 0.10 或更新版本。 

我們強烈鼓勵您從 0.9.X 移至最新的版本。您可以從 [https://kafka.apache.org/downloads ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://kafka.apache.org/downloads){:new_window} 下載 Kafka 用戶端。

如需使用 0.9.X 用戶端之影響的相關資訊，請參閱[舊版相容性](/docs/services/EventStreams?topic=eventstreams-kafka_clients#compatibility)。



### 將 Kafka Java 用戶端移轉到 0.10.2.X 或更新版本

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
