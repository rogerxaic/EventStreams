---

copyright:
  years: 2015, 2019
lastupdated: "2018-07-04"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 管理對 {{site.data.keyword.messagehub}} 資源的存取（企業方案）
{: #security }

您可以用精細的方式保護 {{site.data.keyword.messagehub}} 資源，以管理您要授與每位使用者對每項資源的存取權。
{: shortdesc}

## 我可以保護什麼？

在 {{site.data.keyword.messagehub}} 內，您可以保護下列資源的存取權：
* 叢集 (cluster)：您可以控制哪些應用程式及使用者可以連接至服務
* 主題 (topic)：您可以控制使用者及應用程式能否建立、刪除、讀取及寫入主題 
* 消費者群組 (group)：您可以控制應用程式能否加入消費者群組 
* 生產者交易 (txnid)：您可以控制能否在 Kafka 中使用交易式生產者功能（亦即多個分割區之間的單一基本寫入）

您可以指派給使用，對每項資源的存取層次（也稱為角色）如下所示：

|存取角色|動作的說明|範例動作|
|:-----------------|:-----------------|:-----------------|
|讀者|在 {{site.data.keyword.messagehub}} 內執行唯讀動作，例如檢視資源|藉由指派對叢集資源類型的讀取權，容許應用程式連接至叢集|
|撰寫者|撰寫者具備超越讀者角色的許可權，包括建立及編輯 {{site.data.keyword.messagehub}} 資源。|藉由指派對主題資源及主題名稱類型的寫入權，容許應用程式產生訊息至主題|
|管理員|管理員具備超越撰寫者角色的許可權，可以完成特許動作。此外，您可以建立及編輯 {{site.data.keyword.messagehub}} 資源。|藉由指派對 {{site.data.keyword.messagehub}} 實例的管理權，容許對所有資源的完整存取|
{: caption="表 1. 範例 {{site.data.keyword.messagehub}} 使用者角色及動作" caption-side="top"}

<!-- comment from Charlie and my reply 
CM: need to confirm if hierarchical e.g. write includes read - and doc. 
KR: I think they do inherit the lower level access https://console.bluemix.net/docs/iam/users_roles.html#iamusermanrol 
-->


## 如何指派存取權？

Cloud Identity and Access Management (IAM) 原則會連接至要被控制的資源。每個原則定義特定使用者應該具備的存取層次，以及是對於哪些資源或資源集的存取權。原則包含下列資訊： 
* 原則適用的服務類型。例如，{{site.data.keyword.messagehub}}。您可以設定原則的範圍，以包含所有服務類型。 
* 要被保護的服務實例。您可以設定原則的範圍，以包含服務類型的所有實例。 
* 要被保護的資源類型。有效值為 <code>cluster</code>、<code>topic</code>、<code>group</code> 或 <code>txnid</code>。您可以選擇是否指定類型。如果不指定類型，原則便會適用於服務實例中的所有資源。 
* 要被保護的資源。針對類型 <code>topic</code>、<code>group</code> 及 <code>txnid</code> 的資源指定。如果不指定資源，原則便會適用於服務實例中指定類型的所有資源。 
* 指派給使用者的角色。例如，讀者、撰寫者或管理員。 

## 預設安全設定為何？

依預設，佈建 {{site.data.keyword.messagehub}} 之後，佈建它的使用者會被授與對所有實例資源的管理員角色。此外，對於相同帳戶中的「所有」服務或「所有 {{site.data.keyword.messagehub}} 服務實例」具有管理員角色的任何使用者，也會具有完整存取權。 

您便可以套用其他原則，將存取權延伸到其他使用者。您可以設定原則的範圍，以適用於 {{site.data.keyword.messagehub}} 整體，或是適用於 {{site.data.keyword.messagehub}} 內的個別資源。如需相關資訊，請參閱[常見情境](#security_scenarios)。

只有具有帳戶管理角色的使用者可以指派原則給使用者。指派原則的方法是使用 IBM Cloud 儀表板，或使用 **ibmcloud** 指令。 
<!--
For example steps for {{site.data.keyword.messagehub}}, see [Examples](#security_examples).
-->


## 常見情境
{: #security_scenarios }

下表彙總了一些常見的 {{site.data.keyword.messagehub}} 情境，以及您需要指派的存取權：

|動作|讀者角色|撰寫者角色|管理員角色|
|---------|----------------|
|容許對所有資源的完整存取|不適用|不適用|服務實例：<var class="keyword varname">your_service_instance</var>|
|容許應用程式或使用者建立或刪除主題|資源類型：<code>cluster</code>   |不適用|資源類型：topic <br/><br/>選用項目：資源 ID：<var class="keyword varname">name_of_topic</var> |
|列出群組、主題和偏移<br/> 說明群組、主題和分配管理系統配置|資源類型：<code>cluster</code>   |不適用|不適用|
|容許應用程式連接至叢集|資源類型：<code>cluster</code>   |不適用|不適用|
|容許應用程式產生訊息至任何主題|資源類型：<code>cluster</code>   |資源類型：<code>topic</code>   |不適用|
|容許應用程式產生訊息至特定主題|資源類型：<code>cluster</code>   |資源類型：<code>topic</code>   <br/>資源 ID：<var class="keyword varname">name_of_topic</var>      |不適用|
|容許應用程式連接至任何主題，以及從任何主題取用（無消費者群組）|資源類型：<code>cluster</code>   <br/>資源類型：<code>topic</code>   |資源類型：<code>topic</code>   |不適用|
|容許應用程式連接至特定主題，以及從特定主題取用（無消費者群組）|資源類型：<code>cluster</code>   <br/>資源類型：<code>topic</code>   <br/>資源 ID：<var class="keyword varname">name_of_topic</var>      |不適用|不適用|
|容許應用程式取用主題（消費者群組）|資源類型：<code>cluster</code>   <br/>資源類型：<code>topic</code>   <br/> 資源類型：<code>group</code>   |不適用|不適用|
|容許應用程式以交易方式產生訊息至特定主題|資源類型：<code>cluster</code>   <br/> 資源類型：<code>group</code>   |資源類型：<code>topic</code>   <br/>資源 ID：<var class="keyword varname">name_of_topic</var>      <br/>資源類型：<code>txnid</code>   |不適用|
|刪除消費者群組|資源類型：<code>cluster</code>   |不適用|資源類型：<code>group</code>   <br/>資源 ID：<var class="keyword varname">group_ID</var>      |
|使用 Streams |資源類型：<code>cluster</code>   </br>資源類型：<code>group</code>   |不適用|資源類型：<code>topic</code>   |

如需 IAM 的相關資訊，請參閱 [IBM Cloud Identity and Access Management](/docs/iam?topic=iam-iamoverview#iamoverview)。

如需如何設定原則的範例，請參閱：[IBM Cloud IAM Service IDs and API Keys ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/blogs/bluemix/2017/10/introducing-ibm-cloud-iam-service-ids-api-keys/){:new_window}。


## 連接至 {{site.data.keyword.messagehub}}
{: #connect_message_enterprise }

如需如何連結 Cloud Foundry 應用程式或取得外部應用程式安全金鑰認證的相關資訊，請參閱[連接至 {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting)。

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















