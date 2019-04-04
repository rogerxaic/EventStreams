---

copyright:
  years: 2015, 2019
lastupdated: "2018-02-12"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 使用 Kafka 主控台工具搭配 {{site.data.keyword.messagehub}}
{: #kafka_console_tools }

Apache Kafka 具有各種主控台工具，可以執行簡單的管理及傳訊作業。您可以使用當中的許多種來搭配 {{site.data.keyword.messagehub}}，不過 {{site.data.keyword.messagehub}} 並不允許對其 ZooKeeper 叢集的連線。隨著 Kafka 的發展，先前需要 ZooKeeper 連線的許多工具已不再有該項需求。
{: shortdesc}

您可以在 Kafka 下載的 <code>bin</code> 目錄中找到這些主控台工具。例如，[Apache Kafka 1.1.0 用戶端 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://archive.apache.org/dist/kafka/1.1.0/kafka-1.1.0-src.tgz){:new_window}。

若要提供 SASL 認證給這些工具，請根據下列範例建立內容檔：

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

請將 USER 及 PASSWORD 取代為來自 {{site.data.keyword.Bluemix_notm}} 主控台中 {{site.data.keyword.messagehub}} **服務認證**標籤的值。


## 主控台生產者
{: #console_producer }

您可以使用 Kafka 主控台生產者工具搭配 {{site.data.keyword.messagehub}}。您必須提供分配管理系統及 SASL 認證的清單。

如前所述地建立完內容檔之後，您可以在終端機中執行主控台生產者，如下所示：

<pre>
<code>
  $ kafka-console-producer.sh --broker-list KAFKA_BROKERS_SASL --producer.config CONFIG_FILE --topic TOPIC_NAME
</code>
</pre>
{:codeblock}

將範例中的下列變數取代為您自己的值：
* KAFKA_BROKERS_SASL 取代為 {{site.data.keyword.Bluemix_notm}} 主控台中 {{site.data.keyword.messagehub}} **服務認證**標籤的值，格式為以逗點區隔的 host:port 配對清單（例如 `host1:port1,host2:port2`）。 
* CONFIG_FILE 取代為配置檔的路徑。 

您可以使用這個工具的許多其他選項，只有需要存取 ZooKeeper 的選項例外。


## 主控台消費者
{: #console_consumer }

您可以使用 Kafka 主控台消費者工具搭配 {{site.data.keyword.messagehub}}。您必須提供引導伺服器及 SASL 認證。

如前所述地建立完內容檔之後，您可以在終端機中執行主控台消費者，如下所示：

<pre>
<code>
  $ kafka-console-consumer.sh --bootstrap-server KAFKA_BROKERS_SASL --consumer.config CONFIG_FILE --topic TOPIC_NAME 
</code>
</pre>
{:codeblock}

將範例中的下列變數取代為您自己的值：
* KAFKA_BROKERS_SASL 取代為 {{site.data.keyword.Bluemix_notm}} 主控台中 {{site.data.keyword.messagehub}} **服務認證**標籤的值，格式為以逗點區隔的 host:port 配對清單（例如 `host1:port1,host2:port2`）。 
* CONFIG_FILE 取代為配置檔的路徑。 

您可以使用這個工具的許多其他選項，只有需要存取 ZooKeeper 的選項例外。


## 消費者群組
{: #consumer_groups_tool }

您可以使用 Kafka 消費者群組工具搭配 {{site.data.keyword.messagehub}}。因為 {{site.data.keyword.messagehub}} 不允許對其 ZooKeeper 叢集的連線，所以無法使用部分選項。

如前所述地建立完內容檔之後，您可以在終端機中執行消費者群組工具。例如，您可以列出消費者群組，如下所示：

<pre>
<code>
  $ kafka-consumer-groups.sh --bootstrap-server KAFKA_BROKERS_SASL --command-config CONFIG_FILE --list
</code>
</pre>
{:codeblock}

將範例中的下列變數取代為您自己的值：
* KAFKA_BROKERS_SASL 取代為 {{site.data.keyword.Bluemix_notm}} 主控台中 {{site.data.keyword.messagehub}} **服務認證**標籤的值，格式為以逗點區隔的 host:port 配對清單（例如 `host1:port1,host2:port2`）。 
* CONFIG_FILE 取代為配置檔的路徑。

使用此工具，您也可以顯示詳細資料，例如消費者的現行位置、他們的落後程度，以及群組的每個分割區的 client-id。例如：

<pre>
<code>
  $ kafka-consumer-groups.sh --bootstrap-server KAFKA_BROKERS_SASL --command-config CONFIG_FILE --describe --group GROUP
</code>
</pre>
{:codeblock}

請將範例中的 GROUP 取代為您要擷取其詳細資料的群組名稱。 


## 主題
{: #topics_tool }

您無法使用 Kafka 主題工具 `kafka-topics` 搭配 {{site.data.keyword.messagehub}}，因為該工具需要存取 ZooKeeper。

不過，您可以在 {{site.data.keyword.Bluemix_notm}} 主控台中使用 {{site.data.keyword.messagehub}} 儀表板或使用 REST API 管理主題。


## Kafka Streams 重設
{: #kafka_streams_reset }

您可以使用此工具搭配 {{site.data.keyword.messagehub}} 以重設 Kafka Streams 應用程式的處理狀態，以便可以從頭重新處理它的輸入。執行此工具之前，請確定您的 Streams 應用程式已完全停止。

例如：

<pre>
<code>
  $ kafka-streams-application-reset.sh --bootstrap-servers KAFKA_BROKERS_SASL --config-file CONFIG_FILE --application-id APP_ID
</code>
</pre>
{:codeblock}

將範例中的下列變數取代為您自己的值：
* KAFKA_BROKERS_SASL 取代為 {{site.data.keyword.Bluemix_notm}} 主控台中 {{site.data.keyword.messagehub}} **服務認證**標籤的值，格式為以逗點區隔的 host:port 配對清單（例如 `host1:port1,host2:port2`）。 
* CONFIG_FILE 取代為配置檔的路徑。 
* APP_ID 取代為您的 Streams 應用程式 ID。

