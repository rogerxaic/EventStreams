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

# Kafka REST API の使用
{: #rest_using}

**Kafka REST API は、標準プランのみの一部として使用可能です。**
<br/>

Kafka REST API は、Kafka クラスターへの RESTful インターフェースを提供します。 この API を使用して、メッセージの生成とコンシュームを行うことができます。参考資料については、[Kafka REST Proxy 資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.confluent.io/2.0.0/kafka-rest/docs/index.html){:new_window} を参照してください。 {{site.data.keyword.messagehub}} の要求と応答では、バイナリー埋め込み形式のみがサポートされます。 Avro および JSON 埋め込み形式はサポートされません。

CURL を使用している場合、以下のような例を使用して生成することができます。
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

CURL を使用している場合、以下のような例を使用してコンシュームすることができます。
<pre class="pre"><code>
curl -X GET -H "X-Auth-Token:<var class="keyword varname">APIKEY</var>" -H "Accept: application/vnd.kafka.binary.v1+json" <var class="keyword varname">kafka-rest endpoint</var>/topics/<var class="keyword varname">topic name</var>/partitions/<var class="keyword varname">partition ID</var>/messages?offset=<var class="keyword varname">offset to start from</var>
</code></pre>
{: codeblock}


CURL の場合、[Confluent 資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://docs.confluent.io/2.0.0/){:new_window} に詳しく説明されているように、コマンド・ラインに以下の行を追加することによってコード例を適応させることもできます。
<pre class="pre">-H "X-Auth-Token: <var class="keyword varname">APIKEY</var>"</pre>
{: codeblock}


<!-- Comment from Andrew
basic introduction, definitely including health warning
-->

