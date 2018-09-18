---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-19"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 使用 {{site.data.keyword.iamshort}} (IAM) 搭配 Alpha 計畫
{: #alpha_iam }

{{site.data.keyword.messagehub}} 許可權是使用 IAM 原則來配置。IAM 原則包含下列資訊：

* 服務 ID
* {{site.data.keyword.Bluemix_short}} 資源，由服務名稱、服務實例、地區、IAM 資源類型及 IAM 資源所定義
* 角色（讀者、撰寫者或管理員）

如需 IAM 的相關資訊，請參閱：[IBM Cloud Identity and Access Management](/docs/iam/index.html#iamoverview)。

如需如何設定原則的範例，請參閱：[Introducing IBM Cloud IAM Service IDs and API Keys ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/blogs/bluemix/2017/10/introducing-ibm-cloud-iam-service-ids-api-keys/){:new_window}。

## {{site.data.keyword.messagehub}} IAM 資源
{: iam_resources}

{{site.data.keyword.messagehub}} 會公開四個 IAM 資源類型：

* cluster
* topic
* group
* txnid

您可以藉由設定 IAM 資源來識別 topic、group 及 txnid 的唯一資源。cluster 資源類型屬於單態，因此不需要唯一地識別。

## 常見情境
{: iam_scenarios}

以下是一些常見的 {{site.data.keyword.messagehub}} 情境，以及您需要指派的存取權：

部分主題的生產者（對等冪而言相同）：
* cluster 資源類型的讀者角色
* 每個 topic 資源類型及主題名稱資源的撰寫者角色

交易式生產者：
* 與生產者相同
* txnid 資源類型及交易 ID 資源的撰寫者角色
* group 資源類型及群組 ID 資源的讀者角色

消費者（無群組）：
* cluster 資源類型的讀者角色
* 每個 topic 資源類型及主題名稱資源的讀者角色

有群組的消費者：
* 與消費者相同（無群組）
* group 資源類型及群組 ID 資源的讀者角色

建立或刪除主題：
* cluster 資源類型的讀者角色
* 每個 topic 資源類型及主題名稱資源的管理員角色

刪除消費者群組：
* cluster 資源類型的讀者角色
* group 資源類型及群組 ID 資源的管理員角色

列出群組、主題和偏移，以及說明群組、主題和分配管理系統配置
* cluster 資源類型的讀者角色














