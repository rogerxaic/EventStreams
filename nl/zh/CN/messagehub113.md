---

copyright:
  years: 2015, 2018
lastupdated: "2017-11-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 将 Kafka Connect 与 {{site.data.keyword.messagehub}} 配合使用
{: #kafka_connect }

您可以将 Kafka Connect 与 {{site.data.keyword.messagehub}} 配合使用，且可以在 {{site.data.keyword.Bluemix_short}} 内部或外部运行工作程序。

Kafka Connect 可以单机或分发方式运行。单机方式旨在进行测试以及临时在系统之间进行连接。分发方式更适用于生产目的。将 {{site.data.keyword.messagehub}} 与这两种方式配合使用所需的配置稍有不同。
{:shortdesc}

## 单机工作程序配置
{: #standalone_worker notoc}

当您启动 Kafka Connect 单机工作程序时，必须在所提供的工作程序属性文件中提供引导程序服务器和 SASL 凭证信息。

单机工作程序不使用任何内部主题。相反，它使用文件来存储偏移量信息。

### 源连接器
{: #source_connector notoc }

以下示例列出必须在属性文件中提供的属性：

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

将 KAFKA_BROKERS_SASL、USER 和 PASSWORD 替换为 {{site.data.keyword.Bluemix_notm}} 控制台的 {{site.data.keyword.messagehub}} **服务凭证**选项卡中的值。


### 接收器连接器
{: #sink_connector }

以下示例列出必须在属性文件中提供的属性：

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

将 KAFKA_BROKERS_SASL、USER 和 PASSWORD 替换为 {{site.data.keyword.Bluemix_notm}} 控制台的 {{site.data.keyword.messagehub}} **服务凭证**选项卡中的值。


## 分发工作程序配置
{: #distributed_worker notoc}

当您启动 Kafka Connect 分发工作程序时，必须在所提供的属性文件中提供引导程序服务器和 SASL 凭证信息。以下示例列出必须在属性文件中提供的属性：

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

将 KAFKA_BROKERS_SASL、USER 和 PASSWORD 替换为 {{site.data.keyword.Bluemix_notm}} 控制台的 {{site.data.keyword.messagehub}} **服务凭证**选项卡中的值。


如果您想要使用源连接器，那么您还必须为生产者指定 SSL 和 SASL 配置，如下所示：

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

如果您想要使用接收器连接器，那么您还必须为使用者指定 SSL 和 SASL 配置，如下所示：

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

此外，分发方式下的 Kafka Connect 在内部使用三个主题。如果您使用的是 Apache Kafka V0.11 或更高版本中的 Kafka Connect，那么当工作程序启动时会自动创建这些主题。您提供主题的名称作为配置参数。请确保具有相同 `group.id` 配置值的所有工作程序的值相同。

|配置               |描述                                                         |
| --------------------------- | ------------------------------------------------------------------- |
|`offset.storage.topic`      |连接器偏移量主题                                             |
|`offset.storage.partitions` |连接器偏移量主题的分区数（缺省值为 25）|
|`config.storage.topic`      |连接器配置主题                                       |
|`status.storage.topic`      |连接器状态主题                                              |
|`status.storage.partitions` |连接器状态主题的分区数（缺省值为 5）|

例如，您可以在属性文件中使用以下键值对：

<pre>
<code>
  offset.storage.topic=connect-offsets
  config.storage.topic=connect-configs
  status.storage.topic=connect-status
</code>
</pre>
{:codeblock}

如果您只是轻度利用 Kafka Connect，请考虑减少分区数。



