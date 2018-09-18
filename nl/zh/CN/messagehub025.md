---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 使用 Kafka REST API
{: #rest_using}

**Kafka REST API 仅在标准套餐中提供。**
<br/>

Kafka REST API 提供了一个用于访问 Kafka 集群的 RESTful 界面。您可以使用该 API 生成和使用消息。有关参考文档，请参阅 [Kafka REST Proxy 文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://docs.confluent.io/2.0.0/kafka-rest/docs/index.html){:new_window}。在 {{site.data.keyword.messagehub}} 中，仅支持二进制嵌入格式的请求和响应。不支持 Avro 和 JSON 嵌入格式。

如果您使用 CURL，那么您可以使用类似以下的示例来产生：
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

如果您使用 CURL，那么您可以使用类似以下的示例来消耗：
<pre class="pre"><code>
curl -X GET -H "X-Auth-Token:<var class="keyword varname">APIKEY</var>" -H "Accept: application/vnd.kafka.binary.v1+json" <var class="keyword varname">kafka-rest endpoint</var>/topics/<var class="keyword varname">topic name</var>/partitions/<var class="keyword varname">partition ID</var>/messages?offset=<var class="keyword varname">offset to start from</var>
</code></pre>
{: codeblock}


对于 CURL，您还可以通过将以下行添加到命令行，调整 [Confluent 文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://docs.confluent.io/2.0.0/){:new_window} 中详细描述的代码示例：
<pre class="pre">-H "X-Auth-Token: <var class="keyword varname">APIKEY</var>"</pre>
{: codeblock}


<!-- Comment from Andrew
basic introduction, definitely including health warning
-->

