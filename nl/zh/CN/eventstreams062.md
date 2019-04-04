---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-14"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 选择与 {{site.data.keyword.messagehub}} 一起使用的 Kafka 客户机
{: #kafka_clients}

要将 {{site.data.keyword.messagehub}} 与 Kafka API 一起使用，请选择以下其中一种客户机类型：

* 官方 Java 客户机。这是最好的选择，因为它包含 Apache Kafka 可用的最新功能。
* [建议的第三方客户机](/docs/services/EventStreams?topic=eventstreams-kafka_clients#clients_table)之一。

对于这两种客户机类型，我们建议始终选择最新版本的客户机。
{: shortdesc}

## 连接事件流的客户机需求

要连接 {{site.data.keyword.messagehub}}，客户机必须支持使用 SASL Plain 机制的认证，还必须使用 TLSv1.2 协议的服务器名称指示 (SNI) 扩展。

我们支持的最低 Kafka 协议是 0.10。

<!--
## Support summary for the official Apache Kafka client (Java)

<table>
    <caption>Table 1. Kafka client support in Standard and Enterprise plans</caption>
      <tr>
	        <th></th>
		    <th>Standard and Enterprise Plans</th>
		    <th></th>
        </tr>
	  		<tr>
			<td>**Kafka version on cluster**</td>
			<td>Kafka 1.1</td>
		</tr>
	  		<tr>
			<td>**Supported client versions**</td>
			<td>Kafka 1.1, or later</td>
		</tr>
			<td>**Authentication requirements**</td>
			<td>Client must support authentication using the SASL Plain mechanism and use the Server Name Indication (SNI) extension to the TLSv1.2 protocol</td>
		</tr>

</table>
-->
	
## 第三方客户机
{: #third_party_clients}

如果无法运行官方 Java 客户机，我们建议运行一个[建议的第三方客户机](/docs/services/EventStreams?topic=eventstreams-kafka_clients#clients_table)，这些客户机全都通过了 {{site.data.keyword.messagehub}} 的测试。 

其他支持最低客户机需求的第三方客户机可能也可用于 {{site.data.keyword.messagehub}}。但是，我们只测试了建议的第三方客户机，而且只有这些客户机的使用经验。

## 所有建议的客户机的支持摘要
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
			<td colspan="3">**官方客户机**</td>
			</tr>
	  		<tr>
			<td>[Apache Kafka 客户机 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://kafka.apache.org/downloads)</td>
			<td>Java</td>
			<td>最新版</td>
			<td>0.10.2<p> 有关旧版客户机的信息，请参阅[向后兼容性](/docs/services/EventStreams?topic=eventstreams-kafka_clients#compatibility)。</p></td>
			<td>[Java 控制台样本 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-console-sample)<br/>
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
1. {: #footnote1}此版本是我们在持续测试中验证的最早版本。通常，这是在过去 12 个月内提供的初始版本，但如果已知存在严重问题，那么可能会更新

## 向后兼容性 - 标准套餐
{: #compatibility}

对于向后兼容性，可以与 {{site.data.keyword.messagehub}} 标准套餐一起使用 Apache Kafka 0.9 Java 客户机。但是，由于此客户机的年限，我们强烈建议不要使用它。如果选择使用此版本客户机，那么您需要附加的[登录模块 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-0.9/message-hub-login-library)。

低于 0.11 的客户机版本可能会造成性能降低，因为需要附加的协议转换才能连接到更新版本的 Kafka 服务器。

<!--
## Unsupported clients

The following clients are not supported by {{site.data.keyword.messagehub}}:

### kafka-node
The kafka-node client does not fully support SASL authentication with the PLAIN mechanism so cannot currently be used with {{site.data.keyword.messagehub}}.


### no-kafka 
The no-kafka client does not fully support SASL authentication with the PLAIN mechanism so cannot currently be used with {{site.data.keyword.messagehub}}.

-->

## 将客户机连接到 {{site.data.keyword.messagehub}}
{: #connect_client}

有关如何配置 Java 客户机以连接到 {{site.data.keyword.messagehub}} 的信息，请参阅[配置客户机](/docs/services/EventStreams?topic=eventstreams-kafka_connect)。












