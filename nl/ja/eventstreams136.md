---

copyright:
  years: 2015, 2019
lastupdated: "2018-06-06"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# クラシック・プランの 3 つの API からの選択 
{: #choose_api_classic}

クラシック・プランでは、{{site.data.keyword.messagehub}} は 3 つの API をサポートします。 ここでは、最適な API を選択する際に役立つ情報を示します。
{: shortdesc}

## Kafka API を使用する理由
{: #why_kafka_classic notoc}

{{site.data.keyword.messagehub}} で提供されている他のインターフェースよりも Kafka API を使用することを選択するのにはいくつかの理由があります。 これらの理由としては以下のようなものがあります。
{:shortdesc}


* Kafka サポートがある既存のシステム (例えば、{{site.data.keyword.IBM}} {{site.data.keyword.streaminganalyticsshort}} および {{site.data.keyword.sparks}}) にアプリを統合するのが容易です。
* 他の API より待ち時間が短く、高いスループットが提供されます。
* Kafka REST API より優れた API が提供されます。

## Kafka REST API を使用する理由
{: #why_rest_classic notoc}

**Kafka REST API は、クラシック・プランの一部としてのみ使用可能です。**
<br/>

Kafka REST API は、以下のシチュエーションで使用できる便利なインターフェースです。

* 開発者が {{site.data.keyword.messagehub}} を使用して開始したい場合
* 待ち時間は重要なファクターではなく、スループットが低い一部のユース・ケース
* デバッグおよび障害検出の目的

Kafka REST API は、スループットが高く待ち時間が短いインターフェースを目的としたものではありません。こうしたタイプの要件がある場合、Kafka API を使用して {{site.data.keyword.messagehub}} と相互に接続することをお勧めします。 詳細については、[Kafka クライアントの使用](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_using)を参照してください。

## {{site.data.keyword.mql}} API を使用する理由
{: #why_mql_classic notoc}

**MQ Light API は、クラシック・プランの一部としてのみ使用可能です。**
<br/>

{{site.data.keyword.mql}} API は、Java™、Node.js、Python、Ruby 用に AMQP ベースのメッセージング・インターフェースを提供します。 この API は、以前の {{site.data.keyword.mql}} サービスとの後方互換性のために提供されています。
















