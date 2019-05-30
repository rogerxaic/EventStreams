---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-30"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}

# 配置 Kafka API 客户机
{: #kafka_api_client}

要连接 {{site.data.keyword.messagehub}}，Kafka API 使用以下其中一套凭证信息： 
*  <code>kafka_brokers_sasl</code> 凭证以及 [VCAP_SERVICES 环境变量](/docs/services/EventStreams?topic=eventstreams-connecting#connect_standard_cf)中的 <code>user</code> 和 <code>password</code>。
* 服务密钥。有关更多信息，请参阅[连接集群](/docs/services/EventStreams?topic=eventstreams-connecting)。
{: shortdesc}



其中，USERNAME 和 PASSWORD 是 {{site.data.keyword.Bluemix_notm}} 的 {{site.data.keyword.messagehub}} **服务凭证**选项卡中的值。

如果使用 <code>sasl.jaas.config</code>，那么在同一 JVM 中运行的客户机可以使用不同的凭证。有关更多信息，请参阅[配置 Kafka 客户机 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://kafka.apache.org/documentation/#security_sasl_plain_clientconfig){:new_window}。

以下示例是名为 <code>consumer.properties</code> 的样本配置文件：

```
key.deserializer=org.apache.kafka.common.serialization.StringDeserializer
value.deserializer=org.apache.kafka.common.serialization.StringDeserializer
#
client.id=kafka-java-console-sample-consumer
group.id=kafka-java-console-sample-group
#
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
ssl.protocol=TLSv1.2
ssl.enabled.protocols=TLSv1.2
ssl.endpoint.identification.algorithm=HTTPS
#
# please read the Kafka docs about this setting
auto.offset.reset=latest
```
{: codeblock}

<!--17/10/17 - Karen: following info duplicated at messagehub104 -->
## 使用 sasl.jaas.config 属性（在 Java 应用程序中进行连接和认证）
{: #kafka_java notoc}
如果使用的是 0.10.2.1 或更高版本的 Kafka 客户机，那么可以使用 <code>sasl.jaas.config</code> 属性进行客户机配置，而不使用 JAAS 文件。要连接到 {{site.data.keyword.messagehub}}，请按如下所示设置 <code>sasl.jaas.config</code>：
<pre>
<code>    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
    username="USERNAME" \
    password="PASSWORD";</code>
</pre>
{:codeblock}

对于较早的 Kafka 客户机而言，您必须使用 JAAS 配置文件来指定凭证。此机制不是很方便，因此我们建议改为使用 <code>sasl.jaas.config</code> 属性。
## 在非 Java 应用程序中进行连接和认证
{: #kafka_notjava notoc}

{{site.data.keyword.messagehub}} 服务当前使用 TLS 上的 SASL PLAIN 认证客户机。凭证将随过加密连接传递。这是 Kafka 0.10.0.X 中添加的新功能。 

支持使用 SASL PLAIN 的 Kafka 0.10 的任何客户机都应该使用 {{site.data.keyword.messagehub}}。示例客户机如下所示：



* [librdkafka ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/edenhill/librdkafka/){:new_window} 
* [confluent-kafka-python ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/confluentinc/confluent-kafka-python){:new_window} 




