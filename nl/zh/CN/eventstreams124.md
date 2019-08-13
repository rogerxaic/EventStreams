---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-14"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}
{:important: .important}

# 管理对 {{site.data.keyword.messagehub}} 资源的访问权 
{: #security }

您可以采用细颗粒度方式保护 {{site.data.keyword.messagehub}} 资源，以管理您想要授予每个用户对每个资源的访问权。
{: shortdesc}

变更 IAM 策略和许可权时，有时候底层的服务需要几分钟后才能反映出来。
{: important}

## 我可以保护什么内容？

在 {{site.data.keyword.messagehub}} 中，您可以保护对以下资源的访问权：
* 集群 (cluster)：您可以控制哪些应用程序和用户可以连接到服务
* 主题 (topic)：您可以控制用户和应用程序创建、删除、读取和写入主题的能力 
* 使用者组 (group)：您可以控制应用程序加入使用者组的能力 
* 生产者事务 (txnid)：您可以控制在 Kafka 中使用事务生产者的能力（即跨多个分区的单一、原子写入）

可以向用户分配的对每个资源的访问级别（也称为角色）如下所示：

|访问角色|操作描述|操作示例|
|:-----------------|:-----------------|:-----------------|
|读取者 | 在 {{site.data.keyword.messagehub}} 中执行只读操作，例如查看资源|通过向集群资源类型分配读访问权，以允许应用程序连接到集群|
|写入者 | 写入者的许可权超过读取者角色，包括创建和编辑 {{site.data.keyword.messagehub}} 资源。|通过分配对主题资源和主题名称类型的写访问权，允许应用程序向主题生成内容。|
|管理者|管理者的许可权超过写入者角色，可以完成特权操作。此外，您可以创建和编辑 {{site.data.keyword.messagehub}} 资源。|通过分配对 {{site.data.keyword.messagehub}} 实例的管理访问权，允许对所有资源的完全访问权|
{: caption="表 1. {{site.data.keyword.messagehub}} 用户角色和操作示例" caption-side="top"}

<!-- comment from Charlie and my reply 
CM: need to confirm if hierarchical e.g. write includes read - and doc. 
KR: I think they do inherit the lower level access https://cloud.ibm.com/docs/iam?topic=iam-userroles#iamusermanrol 
-->


## 我如何分配访问权？

Cloud Identity 和 Access Management (IAM) 策略附加到要进行控制的资源。每个策略定义特定用户针对哪一个或哪一些资源应拥有什么访问级别。策略由以下信息构成： 
* 策略适用的服务类型。例如，{{site.data.keyword.messagehub}}。您可以将策略范围限定为包含所有服务类型。 
* 要保护的服务实例。您可以将策略范围限定为包含某个服务类型的所有实例。 
* 要保护的资源类型。有效值为 <code>cluster</code>、<code>topic</code>、<code>group</code> 或 <code>txnid</code>。指定类型是可选操作。如果不指定类型，那么策略将应用于服务实例中的所有资源。 
* 要保护的资源。指定类型 <code>topic</code>、<code>group</code> 和 <code>txnid</code> 的资源。如果不指定资源，那么策略将应用于服务实例中指定类型的所有资源。 
* 分配给用户的角色。例如，“读取者”、“写入者”或“管理者”。 

## 什么是缺省安全设置？

缺省情况下，供应 {{site.data.keyword.messagehub}} 时，进行供应的用户将被授予对该实例的所有资源的管理者角色。此外，同一帐户中具有“全部”服务或“全部 {{site.data.keyword.messagehub}} 服务实例”的管理者角色的任何用户也将具有完全访问权。 

然后，您可以应用其他策略以扩展其他用户的访问权。您可以将策略范围限定为适用于整个 {{site.data.keyword.messagehub}} 或者 {{site.data.keyword.messagehub}} 中的个别资源。有关更多信息，请参阅[常见场景](#security_scenarios)。

只有具有帐户管理角色的用户才能将策略分配给用户。请使用 IBM Cloud 仪表板或 **ibmcloud** 命令来分配策略。 
<!--
For example steps for {{site.data.keyword.messagehub}}, see [Examples](#security_examples).
-->


## 常见场景
{: #security_scenarios }

此表总结了一些常见的 {{site.data.keyword.messagehub}} 场景以及您需要分配的访问权：

|操作 |读取者角色|写入者角色|管理者角色|
|---------|----------------|
|允许对所有资源的完全访问权|不适用|不适用|服务实例：<var class="keyword varname">your_service_instance</var>|
|允许应用程序或用户创建或删除主题|资源类型：<code>cluster</code>   |不适用|资源类型：主题 <br/><br/>可选：资源标识：<var class="keyword varname">name_of_topic</var> |
|列出组、主题和偏移量<br/> 描述组、主题和代理程序配置|资源类型：<code>cluster</code>   |不适用|不适用|
|允许应用程序连接到集群|资源类型：<code>cluster</code>|不适用|不适用|
|允许应用程序向任何主题生成内容|资源类型：<code>cluster</code>|资源类型：<code>topic</code>   |不适用|
|允许应用程序向特定主题生成内容|资源类型：<code>cluster</code>|资源类型：<code>topic</code><br/>资源标识：<var class="keyword varname">name_of_topic</var>      |不适用|
| 允许应用程序连接到任何主题并使用其中的内容（无使用者组）|资源类型：<code>cluster</code>   <br/>资源类型：<code>topic</code>   |不适用|不适用|
| 允许应用程序连接到特定主题并使用其中的内容（无使用者组）|资源类型：<code>cluster</code>   <br/>资源类型：<code>topic</code><br/>资源标识：<var class="keyword varname">name_of_topic</var>      |不适用|不适用|
| 允许应用程序使用主题（使用者组）|资源类型：<code>cluster</code>   <br/>资源类型：<code>topic</code><br/>   资源类型：<code>group</code>|不适用|不适用|
|允许应用程序按事务向主题生成内容|资源类型：<code>cluster</code>   <br/> 资源类型：<code>group</code>|资源类型：<code>topic</code>   <br/>资源标识：<var class="keyword varname">name_of_topic</var>      <br/>资源类型：<code>txnid</code>|不适用|
|删除使用者组|资源类型：<code>cluster</code>   |不适用|资源类型：<code>group</code> <br/>资源标识：<var class="keyword varname">group_ID</var>      |
| 要使用 Streams |资源类型：<code>cluster</code></br> 资源类型：<code>group</code>|不适用|资源类型：<code>topic</code>   |

有关 IAM 的更多信息，请参阅 [IBM Cloud Identity and Access Management](/docs/iam?topic=iam-iamoverview#iamoverview)。

有关如何设置策略的示例，请参阅：[IBM Cloud IAM Service IDs and API Keys ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud/blog/introducing-ibm-cloud-iam-service-ids-api-keys){:new_window}。


## 连接到 {{site.data.keyword.messagehub}}
{: #connect_message_enterprise }

有关如何绑定 Cloud Foundry 应用程序或者获取外部应用程序的安全密钥凭证的信息，请参阅[连接到 {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting)。

<!-- 28/06/18 - Karen: draft info only

## Examples
{: #security_examples }

I want to give a user access to create or delete a topic:

1. From the IBM Cloud dashboard, go to the **Manage** tab &gt; **Security** &gt; **Identity and Access**, and then select **Users**.
2. Click **Invite users**.
3. Specify the email address of the user that you want to invite.
4. In the **Access** section, expand the **Services** option.
5. Choose to assign access to a **Resource**.
6. In the **Services** section, select **{{site.data.keyword.messagehub}}**
7. In the **Region** section, make your selection.
8. In the **Service instance** section, locate your instance and select it.
9. In the **Resource type** section, enter **cluster**.
10. In the **Select roles** section, check the **Reader** box.
11. In the **Resource type** section, enter **topic**.
12. In the **Select roles** section, check the **Manager** box.
13. Click **Invite users**.

-->















