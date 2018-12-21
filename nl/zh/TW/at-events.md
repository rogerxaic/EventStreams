---

copyright:
  years: 2016, 2018
lastupdated: "2018-08-14"

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

# {{site.data.keyword.cloudaccesstrailshort}} 事件（企業方案）
{: #at_events}

使用 {{site.data.keyword.cloudaccesstrailfull}} 服務，以追蹤使用者及應用程式如何與 {{site.data.keyword.Bluemix}} 之企業方案上的 {{site.data.keyword.messagehub}} 服務互動。
{: shortdesc}

{{site.data.keyword.cloudaccesstrailfull_notm}} 服務會記錄由使用者起始並且會變更 {{site.data.keyword.Bluemix_notm}} 中服務狀態的活動。如需相關資訊，請參閱 [{{site.data.keyword.cloudaccesstrailshort}}](/docs/services/cloud-activity-tracker/index.html#getting-started-with-cla)。

<!-- You can create different sections to group events by area. -->

## 事件清單
{: #events}

<!-- Make sure you introduce the table with a detailed description that immediately precedes it. For example, see https://console.bluemix.net/docs/services/cloud-activity-tracker/services/at_events_cf.html#catalog. -->

企業方案上的 {{site.data.keyword.messagehub}} 會自動產生事件，以讓您追蹤服務上的活動。

|動作|說明|
|:-------|:------------|
| messagehub.topic.create |當您建立主題時，會建立事件|
| messagehub.topic.delete |當您刪除主題時，會建立事件|
{: caption="表 1. {{site.data.keyword.messagehub}} 事件" caption-side="top"}

## 事件的檢視位置
{: #ui}

<!-- For example, choose one of the following two options. -->

<!-- Option 2: Add the following sentence if your service sends events to the account domain. -->

{{site.data.keyword.cloudaccesstrailshort}} 事件提供於 {{site.data.keyword.cloudaccesstrailshort}} **帳戶網域**中，而帳戶網域提供於產生事件的 {{site.data.keyword.Bluemix_notm}} 位置（地區）。










