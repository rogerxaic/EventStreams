---

copyright:
  years: 2015, 2019
lastupdated: "2018-06-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 将 Kafka Streams 与 {{site.data.keyword.messagehub}} 配合使用
{: #kafka_streams }

主题 API 使用 {{site.data.keyword.messagehub}} 时无需进行任何设置。请使用 <code>sasl.jaas.config</code> 或 JAAS 文件指定 SASL 凭证，并将 <code>replication.factor</code> 设置为 3。
{: shortdesc}

确保您使用的是 Streams 0.10.2 或更高版本。   

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

其中，BOOTSTRAP_SERVERS、USERNAME 和 PASSWORD 是 {{site.data.keyword.Bluemix_notm}} 的 {{site.data.keyword.messagehub}} **服务凭证**选项卡中的值。

<!--
new topic that includes content from existing topics about samples and migration
-->
