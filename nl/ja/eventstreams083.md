---

copyright:
  years: 2015, 2019
lastupdated: "2018-05-30"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

<!-- 15/11/18: info moved to eventstreams075.md, moved because of doc app changes -->
# 最大限度
{: #maximum_limits}

**MQ Light API は、標準プランのみの一部として使用可能です。**
<br/>

{{site.data.keyword.mql}} API には以下の限度が適用されます。
{:shortdesc}

* 保管できるデータの最大量は、ご使用の支払いプランに応じて単一 Kafka パーティションと一貫性のある量になります (通常は 1 GB)。 このデータ限度を超えると、新しいメッセージが送信されたときに、パーティション内の最も古いメッセージが削除されます。
* メッセージが保管される最大時間は、ご使用の支払いプランに応じて単一 Kafka パーティションと一貫性のある時間になります (通常は 24 時間)。 この時間よりも前のメッセージを取り出すことはできません。
* 1 つのメッセージの最大サイズ (ヘッダーを除く) は 1 MB です。
* 一度に接続できるクライアントの最大数は 25 です。
* 一度にアクティブにできる宛先の最大数は 25 です。 アクティブな宛先は次のように定義されます。
  - 現在接続されているクライアントがある場合とない場合の両方とも、TimeToLive > 0 の宛先。
  - クライアントが接続されている、TimeToLive = 0 (デフォルト) の宛先。 
  <p>複数クライアントで共有されている宛先は、単一の宛先としてカウントされます。</p>
