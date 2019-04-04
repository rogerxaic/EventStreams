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

# MQ Light API を {{site.data.keyword.messagehub}} で使用するために必要なもの
{: #mql_api_reqs}

<!-- 30/10/18: info moved to eventstreams075.md because of doc app changes -->
** MQ Light API は、標準プランのみの一部として使用可能です。**
<br/>

{{site.data.keyword.messagehub}} で {{site.data.keyword.mql}} API を使用するには、以下のものが必要です。 

**この API を使用するには、その前に「MQLight」という名前の Kafka トピックを明示的に作成する必要があります。これは、すべてのメッセージが「MQLight」トピックを経由するためです。 このトピックのパーティションは 1 つでなければなりません。 このトピックを作成すると、サービス・インスタンスに対して MQ Light API が有効になります。 MQ Light API で使用されるトピックは、ユーザーが使用するのに合わせて自動的に作成されますが、すべてのメッセージは実際には単一の Kafka トピック「MQLight」上にあります。** 
{: shortdesc}

「MQLight」トピックは、MQ Light API によって、メッセージ・データを保管するため、および他の Kafka クライアントと対話するために使用されます。 このトピックが作成されると、サービス支払いプランに概説されている標準レートで課金が発生することに注意してください。

MQ Light API を無効にするには、「MQLight」トピックを削除します。 トピックを削除するとすべてのデータが破棄されることに注意してください。
