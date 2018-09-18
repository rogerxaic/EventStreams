---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-30"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Kafka REST API を使用する理由
{: #why_rest}

**Kafka REST API は、標準プランのみの一部として使用可能です。**
<br/>

Kafka REST API は、以下のシチュエーションで使用できる便利なインターフェースです。

* 待ち時間は重要なファクターではなく、スループットが低い一部のユース・ケース
* デバッグおよび障害検出の目的

Kafka REST API は、スループットが高く待ち時間が短いインターフェースを目的としたものではありません。 こうしたタイプの要件がある場合、Kafka API を使用して {{site.data.keyword.messagehub}} と相互に接続することをお勧めします。 詳細については、[Kafka クライアントの使用](/docs/services/MessageHub/messagehub050.html#kafka_using)を参照してください。


