---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-07"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}


# 将 Kafka 控制台工具与 {{site.data.keyword.messagehub}} 配合使用
{: #kafka_console_tools }

Apache Kafka 附带多种控制台工具，用于简单的管理和消息传递操作。虽然 {{site.data.keyword.messagehub}} 不允许连接到其 ZooKeeper 集群，但是您仍可以将其中的许多工具与 {{site.data.keyword.messagehub}} 配合使用。由于 Kafka 已经得到了长足发展，之前需要连接到 ZooKeeper 的许多工具现在已经没有这样的需求了。
{: shortdesc}

您可以在 Kafka 下载文件的 <code>bin</code> 目录中找到这些控制台工具。您可以从 [Apache Kafka 下载 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://kafka.apache.org/downloads){:new_window} 中下载客户机。

要向这些工具提供 SASL 凭证，请基于以下示例创建属性文件：

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

将 USER 和 PASSWORD 替换为 {{site.data.keyword.Bluemix_notm}} 控制台的 {{site.data.keyword.messagehub}} **服务凭证**选项卡中的值。



## 控制台生产者
{: #console_producer }

您可以将 Kafka 控制台生产者工具与 {{site.data.keyword.messagehub}} 配合使用。您必须提供代理程序和 SASL 凭证的列表。

在您如上所述创建属性文件之后，您可以在终端运行控制台生产者，如下所示：

<pre>
<code>
  $ kafka-console-producer.sh --broker-list KAFKA_BROKERS_SASL --producer.config CONFIG_FILE --topic TOPIC_NAME
</code>
</pre>
{:codeblock}

将示例中的以下变量替换为自己的值：
* 将 KAFKA_BROKERS_SASL 替换为 {{site.data.keyword.Bluemix_notm}} 控制台的 {{site.data.keyword.messagehub}} **服务凭证**选项卡中的值，作为以逗号分隔的 host:port 对的列表（例如 `host1:port1,host2:port2`）。 
* 将 CONFIG_FILE 替换为配置文件的路径。 

您可以使用此工具的许多其他选项，但需要访问 ZooKeeper 的选项除外。


## 控制台使用者
{: #console_consumer }

您可以将 Kafka 控制台使用者工具与 {{site.data.keyword.messagehub}} 配合使用。您必须提供引导程序服务器和 SASL 凭证。

在您如上所述创建属性文件之后，您可以在终端运行控制台使用者，如下所示：

<pre>
<code>
  $ kafka-console-consumer.sh --bootstrap-server KAFKA_BROKERS_SASL --consumer.config CONFIG_FILE --topic TOPIC_NAME 
</code>
</pre>
{:codeblock}

将示例中的以下变量替换为自己的值：
* 将 KAFKA_BROKERS_SASL 替换为 {{site.data.keyword.Bluemix_notm}} 控制台的 {{site.data.keyword.messagehub}} **服务凭证**选项卡中的值，作为以逗号分隔的 host:port 对的列表（例如 `host1:port1,host2:port2`）。 
* 将 CONFIG_FILE 替换为配置文件的路径。 

您可以使用此工具的许多其他选项，但需要访问 ZooKeeper 的选项除外。


## 使用者组
{: #consumer_groups_tool }

您可以将 Kafka 使用者组工具与 {{site.data.keyword.messagehub}} 配合使用。由于 {{site.data.keyword.messagehub}} 不允许连接到其 ZooKeeper 集群，所以有一些选项不可用。

在您如上所述创建属性文件之后，您可以在终端运行使用者组工具。例如，您可以列出使用者组，如下所示：

<pre>
<code>
  $ kafka-consumer-groups.sh --bootstrap-server KAFKA_BROKERS_SASL --command-config CONFIG_FILE --list
</code>
</pre>
{:codeblock}

将示例中的以下变量替换为自己的值：
* 将 KAFKA_BROKERS_SASL 替换为 {{site.data.keyword.Bluemix_notm}} 控制台的 {{site.data.keyword.messagehub}} **服务凭证**选项卡中的值，作为以逗号分隔的 host:port 对的列表（例如 `host1:port1,host2:port2`）。 
* 将 CONFIG_FILE 替换为配置文件的路径。

使用此工具，您还可以显示详细信息，如使用者的当前位置、其延迟和组的每个分区的客户机标识。例如：

<pre>
<code>
  $ kafka-consumer-groups.sh --bootstrap-server KAFKA_BROKERS_SASL --command-config CONFIG_FILE --describe --group GROUP
</code>
</pre>
{:codeblock}

将示例中的 GROUP 替换为要检索其详细信息的组名。 


## 主题
{: #topics_tool }

您无法将 Kafka 主题工具 `kafka-topics` 与 {{site.data.keyword.messagehub}} 配合使用，因为该工具需要访问 ZooKeeper。

但是，您可以使用 {{site.data.keyword.Bluemix_notm}} 控制台中的 {{site.data.keyword.messagehub}} 仪表板或使用 REST API 来管理主题。


## Kafka Streams 重置
{: #kafka_streams_reset }

您可以将此工具与 {{site.data.keyword.messagehub}} 配合使用，来重置 Kafka Streams 应用程序的处理状态，以便您可以从头开始重新处理其输入。在运行此工具之前，请确保 Streams 应用程序已完全停止。

例如：

<pre>
<code>
  $ kafka-streams-application-reset.sh --bootstrap-servers KAFKA_BROKERS_SASL --config-file CONFIG_FILE --application-id APP_ID
</code>
</pre>
{:codeblock}

将示例中的以下变量替换为自己的值：
* 将 KAFKA_BROKERS_SASL 替换为 {{site.data.keyword.Bluemix_notm}} 控制台的 {{site.data.keyword.messagehub}} **服务凭证**选项卡中的值，作为以逗号分隔的 host:port 对的列表（例如 `host1:port1,host2:port2`）。 
* 将 CONFIG_FILE 替换为配置文件的路径。 
* 将 APP_ID 替换为 Streams 应用程序标识。

