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

# 使用 Kafka Java 客户机
{: #kafka_java_using}

<!-- 21/06/18 - removing until some content ready

## To do: instructions for getting started, with links for more information


## To do: simple send source and receive source in-line


## How to use, download, and run the Java Kafka API sample

-->

Java&trade; Kafka API 样本是用 Java 编写的示例生产者和使用者，此样本直接使用 Kafka API。可以在本地或在 {{site.data.keyword.Bluemix_short}} 中运行此样本。
{: shortdesc}

样本代码位于 [event-streams-samples GitHub 项目 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-console-sample){:new_window} 中。虽然该样本使用 Kafka API 来发送和接收消息，但该样本使用 {{site.data.keyword.messagehub}} 管理 API 来创建主题，以便向该主题发送消息和从该主题接收消息。

有关设置和运行此样本的更多信息，请参阅 [README.md ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-console-sample){:new_window}。

有关如何运行样本的详细说明，请参阅 [{{site.data.keyword.messagehub}} 入门](/docs/services/EventStreams?topic=eventstreams-getting_started#getting_started_steps)。

## 如何使用、下载和运行 Liberty for Java 样本
{: #liberty_sample notoc}

Liberty for Java 样本实现部署到 Liberty 运行时上的简单应用程序。应用程序将 Kafka API 用于 {{site.data.keyword.messagehub}} 来生成和使用消息。应用程序还提供了一个可用于管理的 Web 前端。

您可以在 [event-streams-samples GitHub 项目 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-liberty-sample){:new_window} 中找到样本代码。

<!--
17/10/17 - Karen: following info duplicated at messagehub063 
-->

## 使用 sasl.jaas.config 属性
{: #sasl_prop notoc}
如果使用的是 0.10.2.1 或更高版本的 Kafka 客户机，那么可以使用 <code>sasl.jaas.config</code> 属性进行客户机配置，而不使用 JAAS 文件。要连接到 {{site.data.keyword.messagehub}}，请按如下所示设置 <code>sasl.jaas.config</code>：
<pre>
<code>    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
    username="USERNAME" \
    password="PASSWORD";</code>
</pre>
{:codeblock}

其中，USERNAME 和 PASSWORD 是 {{site.data.keyword.Bluemix_notm}} 的 {{site.data.keyword.messagehub}} **服务凭证**选项卡中的值。

如果使用 <code>sasl.jaas.config</code>，那么在同一 JVM 中运行的客户机可以使用不同的凭证。有关更多信息，请参阅[配置 Kafka 客户机 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://kafka.apache.org/documentation/#security_sasl_plain_clientconfig){:new_window}。

对于较早的 Kafka 客户机而言，您必须使用 JAAS 配置文件来指定凭证。此机制不是很方便，因此我们建议改为使用 <code>sasl.jaas.config</code> 属性。

<!--
23/04/18 - Karen: following migration info on production in messagehub084 
-->

## 将 Kafka 客户机从 0.9.X 或 0.10.X 迁移到更高客户机版本
{: #kafka_migrate}


如果使用的是 Java 客户机，那么可以使用公共可用的 0.10 或更高版本的 Kafka 客户机。 

强烈建议您从 0.9.X 迁移到最新版本。您可以从 [https://kafka.apache.org/downloads ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://kafka.apache.org/downloads){:new_window} 下载 Kafka 客户机。

有关使用 0.9.X 客户机的影响的信息，请参阅[向后兼容性](/docs/services/EventStreams?topic=eventstreams-kafka_clients#compatibility)。



### 将 Kafka 客户机迁移到 0.10.2.X 或更高版本

从 0.10.2 开始，您可以直接在客户机属性中配置 SASL 认证，无需再使用 JAAS 文件。通过这一简化措施，您可以使用不同的凭证集在同一 JVM 中运行多个客户机，而这是使用 JAAS 文件无法实现的。

请完成以下步骤：

1. 删除 JAAS 文件。请注意，也不再需要 JVM 属性 java.security.auth.login.config=<PATH TO JAAS>。
2. 如果是从 0.9.X 进行迁移，请删除 {{site.data.keyword.messagehub}} 登录 JAR 模块。
2. 将以下内容添加到客户机属性：
    ```
	sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
	```

	其中，USERNAME 和 PASSWORD 是 {{site.data.keyword.Bluemix_notm}} 的 {{site.data.keyword.messagehub}} **服务凭证**选项卡中的值。
	
	

### 将 Kafka 客户机从 0.9.X 迁移到 0.10.0.X 或 0.10.1.X

请完成以下步骤：

1. 删除 {{site.data.keyword.messagehub}} 登录 JAR 模块。
2. 将 <code>jaas.conf</code> 文件更改为以下内容：
    ```
        KafkaClient {
          org.apache.kafka.common.security.plain.PlainLoginModule required
          serviceName="kafka"
            username="USERNAME"
            password="PASSWORD";
        };
    ```
    {: codeblock}

	其中，USERNAME 和 PASSWORD 是 {{site.data.keyword.Bluemix_notm}} 的 {{site.data.keyword.messagehub}} **服务凭证**选项卡中的值。
	
3. 将以下行添加到使用者和生产者属性：<code>sasl.mechanism=PLAIN</code>
