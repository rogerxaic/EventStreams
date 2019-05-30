---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-13"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 使用 Kafka Streams 搭配 {{site.data.keyword.messagehub}}
{: #kafka_streams }

主題 API 可搭配 {{site.data.keyword.messagehub}} 運作，不需要設定。請使用 <code>sasl.jaas.config</code> 或 JAAS 檔案指定 SASL 認證，並將 <code>replication.factor</code> 設為 3。
{: shortdesc}

請確定您使用 Streams 0.10.2 或更新版本。   

例如：

<pre>
<code>
    props.put(StreamsConfig.REPLICATION_FACTOR_CONFIG, "3");
    props.put(StreamsConfig.BOOTSTRAP_SERVERS_CONFIG, BOOTSTRAP_SERVERS);
    props.put("security.protocol","SASL_SSL");
    props.put("sasl.mechanism","PLAIN");
    props.put("ssl.protocol","TLSv1.2");
    props.put("ssl.enabled.protocols","TLSv1.2");
    props.put("sasl.jaas.config","org.apache.kafka.common.security.plain.PlainLoginModule required username=\"USERNAME\" password=\"PASSWORD\";");
</code>
</pre>
{:codeblock}

其中 BOOTSTRAP_SERVERS、USERNAME 及 PASSWORD 是來自 {{site.data.keyword.Bluemix_notm}} 中 {{site.data.keyword.messagehub}} **服務認證**標籤的值。

<!--
new topic that includes content from existing topics about samples and migration
-->
