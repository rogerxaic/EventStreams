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

# クラシック・プランの Kafka REST API の使用 
{: #rest_using_classic}

**このバージョンの Kafka REST API は、クラシック・プランの一部としてのみ使用可能です。**
<br/>

Kafka REST API は、Kafka クラスターへの RESTful インターフェースを提供します。 この API を使用して、メッセージの生成とコンシュームを行うことができます。 API 参照資料などについて詳しくは、[Kafka REST Proxy 資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.confluent.io/2.0.0/kafka-rest/docs/index.html){:new_window}を参照してください。 {{site.data.keyword.messagehub}} の要求と応答では、バイナリー埋め込み形式のみがサポートされます。 Avro および JSON 埋め込み形式はサポートされません。
{: shortdesc}

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
<br/>
<br/>
CURL を使用している場合、以下のような例を使用してコンシュームすることができます。
<pre class="pre"><code>
curl -X GET -H "X-Auth-Token:<var class="keyword varname">APIKEY</var>" -H "Accept: application/vnd.kafka.binary.v1+json" <var class="keyword varname">kafka-rest endpoint</var>/topics/<var class="keyword varname">topic name</var>/partitions/<var class="keyword varname">partition ID</var>/messages?offset=<var class="keyword varname">offset to start from</var>
</code></pre>
{: codeblock}

<br/>
<br/>
CURL の場合、[Confluent 資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.confluent.io/2.0.0/){:new_window} に詳しく説明されているように、コマンド・ラインに以下の行を追加することによってコード例を適応させることもできます。
<pre class="pre">-H "X-Auth-Token: <var class="keyword varname">APIKEY</var>"</pre>
{: codeblock}

<br/>
## 接続および認証の方法
{: #rest_connect_classic}

<!-- info was in eventstreams066.md -->

{{site.data.keyword.messagehub}} に接続するために、Kafka REST API は [VCAP_SERVICES 環境変数](/docs/services/EventStreams?topic=eventstreams-connecting#connect_classic_cf)からの <code>api_key</code> 資格情報および <code>kafka_rest_url</code> 資格情報を使用します。

{{site.data.keyword.messagehub}} Kafka REST API で認証するには、
要求の X-Auth-Token ヘッダーに <code>api_key</code> を指定する必要があります。


## API の使用法
{: #rest_how_classic}

<!-- info was in eventstreams097.md -->

{{site.data.keyword.messagehub}} Kafka REST API サンプルは、Kafka REST API で {{site.data.keyword.messagehub}} に接続して、メッセージを作成およびコンシュームする Node.js アプリケーションです。

サンプル・コードは [event-streams-samples GitHub プロジェクト ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample){:new_window} にあります。

プロジェクトの [README.md ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample){:new_window} ファイルに記載された説明に従って、サンプルをビルドし、実行してください。

	以下のブログには、Kafka REST API の強みと弱みの概要が示されています。 [A Comprehensive, Open Source REST Proxy for Kafka ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://www.confluent.io/blog/a-comprehensive-open-source-rest-proxy-for-kafka/){: new_window}。








