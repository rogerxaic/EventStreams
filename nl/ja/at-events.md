---

copyright:
  years: 2016, 2018
lastupdated: "2019-05-14"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:table: .aria-labeledby="caption"}

<!-- Name your file `at-events.md` and include it in the Reference nav group in your toc file. -->

# {{site.data.keyword.cloudaccesstrailshort}} イベント 
{: #at_events}

標準プランとエンタープライズ・プランで、ユーザーおよびアプリケーションが {{site.data.keyword.Bluemix}} 内の {{site.data.keyword.messagehub}} サービスとどのように対話するのかを {{site.data.keyword.cloudaccesstrailfull}} サービスを使用してトラッキングします。 
{: shortdesc}

{{site.data.keyword.cloudaccesstrailfull_notm}} サービスは、{{site.data.keyword.Bluemix_notm}} 内のサービスの状態を変更するユーザー開始アクティビティーを記録します。 詳しくは、[{{site.data.keyword.cloudaccesstrailshort}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started){:new_window}を参照してください。

<!-- You can create different sections to group events by area. -->

## イベントのリスト
{: #events}

<!-- Make sure you introduce the table with a detailed description that immediately precedes it. For example, see https://cloud.ibm.com/docs/services/cloud-activity-tracker/services?topic=cloud-activity-tracker-cf#catalog. -->

標準プランとエンタープライズ・プランの {{site.data.keyword.messagehub}} は、お客様がサービスのアクティビティーをトラッキングできるように、イベントを自動的に生成します。

| アクション | 説明 |
|:-------|:------------|
| event-streams.topic.create | トピックを作成するとイベントが作成される|
| event-streams.topic.delete | トピックを削除するとイベントが作成される|
{: caption="表 1. {{site.data.keyword.messagehub}} イベント" caption-side="top"}

## イベントの表示場所
{: #ui}

<!-- For example, choose one of the following two options. -->

<!-- Option 2: Add the following sentence if your service sends events to the account domain. -->

{{site.data.keyword.cloudaccesstrailshort}} イベントは、イベントが生成される {{site.data.keyword.Bluemix_notm}} ロケーション (地域) 内にある {{site.data.keyword.cloudaccesstrailshort}} **アカウント・ドメイン**で使用可能です。










