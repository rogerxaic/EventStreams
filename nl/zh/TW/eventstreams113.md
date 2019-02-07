---

copyright:
  years: 2015, 2019
lastupdated: "2017-11-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 使用 Kafka Connect 搭配 {{site.data.keyword.messagehub}}
{: #kafka_connect }

您可以使用 Kafka Connect 搭配 {{site.data.keyword.messagehub}}，且可以在 {{site.data.keyword.Bluemix_short}} 內部或外部執行工作者節點。
{: shortdesc}

Kafka Connect 可以用獨立式或分散式模式執行。獨立式模式是用於系統之間的測試及暫存連線。分散式模式更適合正式作業使用。這兩種模式要與 {{site.data.keyword.messagehub}} 搭配使用所需的配置略有不同。
{:shortdesc}

## 獨立式工作者節點配置
{: #standalone_worker notoc}

在您啟動 Kafka Connect 獨立式工作者節點時，必須在提供的工作者節點內容檔中提供引導伺服器和 SASL 認證資訊。

獨立式工作者節點不會使用任何內部主題。相反地，它使用檔案來儲存偏移資訊。

### 來源連接器
{: #source_connector notoc }

下列範例列出您必須在內容檔中提供的內容：

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

請將 KAFKA_BROKERS_SASL、USER 及 PASSWORD 取代為來自 {{site.data.keyword.Bluemix_notm}} 主控台中 {{site.data.keyword.messagehub}} **服務認證**標籤的值。

### 接收槽連接器
{: #sink_connector }

下列範例列出您必須在內容檔中提供的內容：

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

請將 KAFKA_BROKERS_SASL、USER 及 PASSWORD 取代為來自 {{site.data.keyword.Bluemix_notm}} 主控台中 {{site.data.keyword.messagehub}} **服務認證**標籤的值。

## 分散式工作者節點配置
{: #distributed_worker notoc}

在您啟動 Kafka Connect 分散式工作者節點時，您必須在提供的內容檔裡提供引導伺服器和 SASL 認證資訊。下列範例列出您必須在內容檔中提供的內容：

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

請將 KAFKA_BROKERS_SASL、USER 及 PASSWORD 取代為來自 {{site.data.keyword.Bluemix_notm}} 主控台中 {{site.data.keyword.messagehub}} **服務認證**標籤的值。

如果您要使用來源連接器，則也必須指定生產者的 SSL 及 SASL 配置，如下所示：

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

如果您想要使用接收槽連接器，則也必須指定消費者的 SSL 及 SASL 配置，如下所示：

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

此外，分散式的 Kafka Connect 在內部使用三個主題。如果您使用 Apache Kafka 0.11 版或更新版本中的 Kafka Connect，則這些主題會在工作者節點啟動時自動建立。請將主題名稱提供為配置參數。請確保值對於具有相同 `group.id` 配置值的所有工作者節點都相同。

|配置|說明|
| --------------------------- | ------------------------------------------------------------------- |
|`offset.storage.topic`      |連接器偏移主題|
|`offset.storage.partitions` |連接器偏移主題的分割區數目（預設值 25）|
|`config.storage.topic`      |連接器配置主題|
|`status.storage.topic`      |連接器狀態主題|
|`status.storage.partitions` |連接器狀態主題的分割區數目（預設值 5）|

例如，您可以在內容檔中使用下列鍵值配對：

<pre>
<code>
  offset.storage.topic=connect-offsets
  config.storage.topic=connect-configs
  status.storage.topic=connect-status
</code>
</pre>
{:codeblock}

如果您只會少量使用 Kafka Connect，請考慮減少分割區數目。



