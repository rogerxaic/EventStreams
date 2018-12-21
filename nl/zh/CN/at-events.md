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

# {{site.data.keyword.cloudaccesstrailshort}} 事件（企业套餐）
{: #at_events}

使用 {{site.data.keyword.cloudaccesstrailfull}} 服务可跟踪用户和应用程序如何与 {{site.data.keyword.Bluemix}} 中企业套餐内的 {{site.data.keyword.messagehub}} 服务进行交互。
{: shortdesc}

{{site.data.keyword.cloudaccesstrailfull_notm}} 服务会记录 {{site.data.keyword.Bluemix_notm}} 中用户发起的更改服务状态的活动。有关更多信息，请参阅 [{{site.data.keyword.cloudaccesstrailshort}}](/docs/services/cloud-activity-tracker/index.html#getting-started-with-cla)。

<!-- You can create different sections to group events by area. -->

## 事件列表
{: #events}

<!-- Make sure you introduce the table with a detailed description that immediately precedes it. For example, see https://console.bluemix.net/docs/services/cloud-activity-tracker/services/at_events_cf.html#catalog. -->

企业套餐中的 {{site.data.keyword.messagehub}} 会自动生成事件，以便您可以跟踪服务相关活动。

|操作 |描述                                                         |
|:-------|:------------|
| messagehub.topic.create |创建主题时会创建一个事件|
| messagehub.topic.delete |删除主题时会创建一个事件|
{: caption="表 1. {{site.data.keyword.messagehub}} 事件" caption-side="top"}

## 在何处查看事件
{: #ui}

<!-- For example, choose one of the following two options. -->

<!-- Option 2: Add the following sentence if your service sends events to the account domain. -->

在生成 {{site.data.keyword.cloudaccesstrailshort}} 事件的 {{site.data.keyword.Bluemix_notm}} 位置（区域）内的 {{site.data.keyword.cloudaccesstrailshort}} **帐户域**中提供了这些事件。










