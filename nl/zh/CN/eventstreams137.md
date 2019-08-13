---

copyright:
  years: 2015, 2019
lastupdated: "2018-05-25"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 在经典套餐上使用 Kafka REST API 
{: #rest_using_classic}

**此版本的 Kafka REST API 仅在经典套餐中提供。**
<br/>

Kafka REST API 提供了一个用于访问 Kafka 集群的 RESTful 界面。您可以使用该 API 生成和使用消息。有关更多信息（包括 API 参考文档），请参阅 [Kafka REST Proxy 文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://docs.confluent.io/2.0.0/kafka-rest/docs/index.html){:new_window}。在 {{site.data.keyword.messagehub}} 中，仅支持二进制嵌入格式的请求和响应。不支持 Avro 和 JSON 嵌入格式。
{: shortdesc}

如果您使用 CURL，那么您可以使用类似以下内容的示例来生产：
<pre class="pre"><code>
curl -X POST -H "X-Auth-Token:<var class="keyword varname">APIKEY</var>" -H "Content-Type: application/vnd.kafka.binary.v1+json" <var class="keyword varname">kafka-rest endpoint</var>/topics/<var class="keyword varname">topic name</var> -d 
 

'
{
  "records": [
    {
      "value": "<var class="keyword varname">A base 64 encoded value string</var>"
    }
  ]
}
'
</code></pre>
{: codeblock}
<br/>
<br/>
如果您使用 CURL，那么您可以使用类似以下内容的示例来消耗：
<pre class="pre"><code>
curl -X GET -H "X-Auth-Token:<var class="keyword varname">APIKEY</var>" -H "Accept: application/vnd.kafka.binary.v1+json" <var class="keyword varname">kafka-rest endpoint</var>/topics/<var class="keyword varname">topic name</var>/partitions/<var class="keyword varname">partition ID</var>/messages?offset=<var class="keyword varname">offset to start from</var>
</code></pre>
{: codeblock}

<br/>
<br/>
对于 CURL，您还可以通过将以下行添加到命令行，调整 [Confluent 文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://docs.confluent.io/2.0.0/){:new_window} 中详细描述的代码示例：
<pre class="pre">-H "X-Auth-Token: <var class="keyword varname">APIKEY</var>"</pre>
{: codeblock}

<br/>
## 如何连接和认证
{: #rest_connect_classic}

<!-- info was in eventstreams066.md -->

为了连接到 {{site.data.keyword.messagehub}}，Kafka REST API 使用 [VCAP_SERVICES 环境变量](/docs/services/EventStreams?topic=eventstreams-connecting#connect_classic_cf)中的 <code>api_key</code> 和 <code>kafka_rest_url</code> 凭证。

要向 {{site.data.keyword.messagehub}} Kafka REST API 进行认证，您必须在请求的 X-Auth-Token 头中指定 <code>api_key</code>。


## 如何使用 API
{: #rest_how_classic}

<!-- info was in eventstreams097.md -->

{{site.data.keyword.messagehub}} Kafka REST API 样本是一个 Node.js 应用程序，它通过 Kafka REST API 连接到 {{site.data.keyword.messagehub}} 来生成和使用消息。

样本代码位于 [event-streams-samples GitHub 项目 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample){:new_window} 中。

请遵循该项目 [README.md ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample){:new_window} 文件中的指示信息来构建和运行样本。

	以下博客很好地介绍了 Kafka REST API 的优点和缺点。[全面的开放式源代码 Kafka REST 代理。![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://www.confluent.io/blog/a-comprehensive-open-source-rest-proxy-for-kafka/){: new_window}。








