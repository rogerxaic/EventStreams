---

copyright:
  years: 2015, 2019
lastupdated: "2018-10-29"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# MQ Light API について、および他の API との違い
{: #mqlight_api}

<!-- 30/10/18: info moved to eventstreams075.md because of doc app changes -->
** MQ Light API は、標準プランのみの一部として使用可能です。**
<br/>

{{site.data.keyword.mql}} API は、Kafka に比べて、より高いレベルでのメッセージングの抽象化を提供します。
{:shortdesc}

Kafka クライアントと {{site.data.keyword.mql}} API のどちらを使用するのかの選択は、構築したいメッセージング・トポロジーに基づきます。

* Kafka を使用する場合は、少数のトピックを使用し、スケーラビリティーを高めるために各トピックに複数のパーティションを含めることが可能です。 コンシューマー・グループを使用することによってコンシューマー間でメッセージを共有できますが、各コンシューマーは、割り当てられたパーティションでメッセージ速度に遅れずに対応できなければなりません。
* {{site.data.keyword.mql}} API を使用する場合は、ずっと多くのトピックを使用でき、トピック名は階層的です (例: <code>‘/sports/football’</code> および <code>‘/sports/tiddlywinks’</code>)。 

{{site.data.keyword.mql}} API のトピックは、Kafka トピックと同じではありません。 そうではなく、{{site.data.keyword.mql}} API は「MQLight」という名前の単一の Kafka トピックを使用し、{{site.data.keyword.mql}} API を使用して送受信されるすべてのメッセージはこの 1 つの Kafka トピックを経由します。

{{site.data.keyword.mql}} が使用可能な {{site.data.keyword.Bluemix_notm}} 地域は、米国南部 (ダラス)、英国南部 (ロンドン)、および南アジア太平洋 (シドニー) のみです。 MQ Light API は、中欧 (フランクフルト) 地域および {{site.data.keyword.Bluemix_notm}} 専用では使用不可です。

<!-- begin STAGING ONLY -->
どの API を使用するのかの選択について詳しくは、[3 つの API からの選択](/docs/services/EventStreams?topic=eventstreams-choose_api)を参照してください。
<!-- end STAGING ONLY -->

