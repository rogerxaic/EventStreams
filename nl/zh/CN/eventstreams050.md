---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-19"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}

# 使用 Kafka API
{: #kafka_using}

Kafka 在范围广泛的语言中提供一组丰富的 API 和客户机。例如：

* **Kafka 的核心 API（Consumer、Producer 和 Admin API）**<br/>
    用于直接发送和接收来自一个或多个 Kafka 主题的消息。
* **Streams API**<br/>
    这是更高级别的流处理 API，可在主题之间轻松使用、转换和生成事件。
* **Connect API**<br/>
    此框架允许使用可复用或标准集成来将事件传入或传出外部系统，例如数据库。
* **KSQL**<br/>
    此接口使用类似 SQL 的语法处理和连接来自主题的事件。

下表总结了使用 {{site.data.keyword.messagehub}} 可实现的功能：

<table>
    <caption>表 1. 标准和企业套餐中的 Kafka 客户机支持</caption>
      <tr>
	        <th></th>
		    <th>标准套餐</th>
		    <th>企业套餐</th>
        </tr>
	  		<tr>
			<td>**集群上的 Kafka 版本**</td>
			<td>Kafka 2.2</td>
			<td>Kafka 1.1</td>
		</tr>
	  		<tr>
			<td>**支持的客户机版本**</td>
			<td>Kafka 0.10.x 或更高版本</td>
			<td>Kafka 0.10.x 或更高版本</td>
		</tr>
		<tr>
			<td>**是否支持 Kafka Connect、Kafka Streams 以及 KSQL？**</td>
			<td>是</td>
			<td>是</td>
		</tr>
		<tr>
			<td>**认证需求**</td>
			<td>客户机必须支持使用 SASL Plain 机制进行认证，还必须使用 TLSv1.2 协议的服务器名称指示 (SNI) 扩展</td>
			<td>客户机必须支持使用 SASL Plain 机制进行认证，还必须使用 TLSv1.2 协议的服务器名称指示 (SNI) 扩展</td>
		</tr>

</table>
<br/>
有关使用经典套餐上 Kafka API 的信息，请参阅 [Kafka API - 经典](/docs/services/EventStreams?topic=eventstreams-kafka_using_classic)。


## 选择与 {{site.data.keyword.messagehub}} 一起使用的 Kafka 客户机
{: #kafka_clients}

Kafka API 的官方客户机以 Java 编写，因此包含最新的功能和错误修订。有关此 API 的更多信息，请参阅 [Kafka Producer API 2.2 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} 和 [Kafka Consumer API 2.2 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html){:new_window}。 

对于其他语言，我们建议运行以下其中一个客户机，这些客户机都已使用 {{site.data.keyword.messagehub}} 进行了良好测试。

### 所有建议的客户机的支持摘要
{: #client_summary}

<table id="clients_table">
    <caption>表 2. 客户机支持摘要</caption>
      <tr>
		    <th id="client" scope="col">客户机</th>
		    <th id="language" scope="col">语言</th>
			<th id="version" scope="col">建议的版本</th>
		    <th id="minimum version" scope="col">支持的最低版本 [<sup>1</sup>](/docs/services/EventStreams?topic=eventstreams-kafka_clients#footnote1)</th>
			<th id="sample link" scope="col">样本链接</th>
        </tr>
			<tr>
			<td colspan="3">**官方 Apache Kafka 客户机**</td>
			</tr>
	  		<tr>
			<td>[Apache Kafka 客户机 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://kafka.apache.org/downloads)</td>
			<td>Java</td>
			<td>最新版</td>
			<td>0.10.2</td>
			<td>[Java 控制台样本](/docs/services/EventStreams?topic=eventstreams-kafka_java_using)<br/>
			[Liberty 样本 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-liberty-sample)
			</td>
			</tr>
			<tr>
			<td colspan="3">**第三方客户机**</td>
			</tr>
	  		<tr>
			<td>[node-rdkafka ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/Blizzard/node-rdkafka)</td>
			<td>Node.js</td>
			<td>最新版</td>
			<td>2.2.2</td>
			<td>[Node.js 样本 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-python ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/confluentinc/confluent-kafka-python)</td>
			<td>Python</td>
			<td>最新版</td>
			<td>0.11.0</td>
			<td>[Kafka Python 样本 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-python-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-go ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/confluentinc/confluent-kafka-go)</td>
			<td>Golang</td>
			<td>最新版</td>
			<td>0.11.0</td>
			<td></td>
		</tr>
		<tr>
			<td>[librdkafka ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/edenhill/librdkafka)</td>
			<td>C 或 C++</td>
			<td>最新版</td>
			<td>0.11.0</td>
			<td></td>
		</tr>

</table>
### 脚注
{: #footnote_clients notoc}
1. {: #footnote1 notoc}此版本是我们在持续测试中验证的最早版本。通常，这是在过去 12 个月内提供的初始版本，但如果已知存在严重问题，那么可能会更新

<br/>
如果无法运行列出的任何客户机，您可以使用满足以下最低需求的其他第三方客户机（例如，[librdkafka ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/edenhill/librdkafka/){:new_window}）。
* 支持 Kafka 0.10 或更高版本
* 可使用 SASL PLAIN 和 TLSv1.2 连接并认证
* 支持 TLS 的 SNI 扩展，其中服务器的主机名包含在 TLS 握手中
* 支持椭圆曲线密码术
但是，我们只测试了建议的第三方客户机，而且只有这些客户机的使用经验。

在所有情况下，建议使用客户机的最新版本。

<br/>
### 将客户机连接到 {{site.data.keyword.messagehub}}
{: #connect_client}

有关如何配置 Java 客户机以连接到 {{site.data.keyword.messagehub}} 的信息，请参阅[配置客户机](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_api_client)。

## 配置 Kafka API 客户机
{: #kafka_api_client}

要建立连接，客户机必须配置为最低通过 TLSv1.2 使用 SASL_SSL PLAIN，并且需要用户名和引导程序服务器列表。 

要检索用户名、密码和引导程序服务器的列表，服务实例需要服务凭证对象或服务密钥。有关创建这些对象的更多信息，请参阅<link to Connecting to event Streams>[连接到 {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting)。

从这些对象：
* 将 <code>kafka_brokers_sasl property</code> 用作引导程序服务器的列表。将此列表的格式设置为主机:端口条目的逗号分隔列表。例如，<code>host1:port1,host2:port2</code>。我们建议包含 <code>kafka_brokers_sasl</code> 属性中列出的所有主机的详细信息。
* 将 <code>user</code> 和 <code>api_key</code> 属性用作用户名和密码。

对于经典套餐上的服务实例，可以改为从应用程序的 VCAP_SERVICES 环境变量中获取此信息。有关更多信息，请参阅[连接 {{site.data.keyword.messagehub}} - 经典](/docs/services/EventStreams?topic=eventstreams-connecting_classic)。


对于 Java 客户机，以下示例显示最小的属性集，其中 USERNAME、PASSWORD 和 KAFKA_BROKERS_SASL 应替换为先前检索的值。

```
bootstrap.servers=KAFKA_BROKERS_SASL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
ssl.protocol=TLSv1.2
ssl.enabled.protocols=TLSv1.2
ssl.endpoint.identification.algorithm=HTTPS

```
{: codeblock}

<br/>
请注意，如果您使用 0.10.2.1 之前的 Kafka 客户机，那么不支持 <code>sasl.jaas.config</code> 属性，您必须改为在 JAAS 配置文件中提供客户机配置。 








 




