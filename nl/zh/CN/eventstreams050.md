---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-28"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 使用 Kafka API
{: #kafka_using}

如果使用的是 Java 客户机，那么可以使用公共可用的 0.10.x 或更高版本的 Kafka 客户机。 

Kafka 客户机存在多种语言版本，我们提供了其中一些语言的指示信息。您可以使用其他语言，但需要 SASL PLAIN 支持来提供凭证。此外，如果使用的是企业套餐，那么还需要使用 TLSv1.2 协议的服务器名称指示 (SNI) 扩展。

<table>
    <caption>表 1. 标准和企业套餐中的 Kafka 客户机支持</caption>
      <tr>
	        <th></th>
		    <th>标准套餐</th>
		    <th>企业套餐</th>
        </tr>
	  		<tr>
			<td>**集群上的 Kafka 版本**</td>
			<td>Kafka 1.1</td>
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

			<td>**认证需求**</td>
			<td>客户机必须支持使用 SASL Plain 机制进行认证，还必须使用 TLSv1.2 协议的服务器名称指示 (SNI) 扩展</td>
			<td>客户机必须支持使用 SASL Plain 机制进行认证，还必须使用 TLSv1.2 协议的服务器名称指示 (SNI) 扩展</td>
		</tr>

</table>

有关 Producer 和 Consumer API 的信息，请参阅 [Kafka Producer API 0.11.0.X ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://kafka.apache.org/0110/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} 和 [Kafka Consumer API 0.11.0.X ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://kafka.apache.org/0110/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html){:new_window}。 

