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

# 将 {{site.data.keyword.iamshort}} (IAM) 与 Alpha 程序配合使用
{: #alpha_iam }

{{site.data.keyword.messagehub}} 许可权使用 IAM 策略进行配置。IAM 策略包含以下信息：

* 服务标识
* {{site.data.keyword.Bluemix_short}} 资源，通过服务名称、服务实例、区域、IAM 资源类型和 IAM 资源进行定义
* 角色（读者、作者或管理员）

有关 IAM 的更多信息，请参阅：[IBM Cloud Identity and Access Management](/docs/iam/index.html#iamoverview)。

有关如何设置策略的示例，请参阅：[Introducing IBM Cloud IAM Service IDs and API Keys ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/blogs/bluemix/2017/10/introducing-ibm-cloud-iam-service-ids-api-keys/){:new_window}。

## {{site.data.keyword.messagehub}} IAM 资源
{: iam_resources}

{{site.data.keyword.messagehub}} 公开了四种 IAM 资源类型：

* 集群
* 主题 
* 组
* txnid

可以通过设置 IAM 资源来识别主题、组和 txnid 的唯一资源。集群资源类型是单例，因此无需进行唯一标识。

## 常见场景
{: iam_scenarios}

下面是一些常见 {{site.data.keyword.messagehub}} 场景以及需要分配的访问权：

某些主题的生产者（与幂等生产者相同）：
* 集群资源类型的读者角色
* 每个主题资源类型和主题名称资源的作者角色

事务生产者：
* 与生产者相同
* txnid 资源类型和事务标识资源的作者角色
* 组资源类型和组标识资源的读者角色

使用者（无分组）：
* 集群资源类型的读者角色
* 每个主题资源类型和主题名称资源的读者角色

使用者（已分组）：
* 与使用者（无分组）相同
* 组资源类型和组标识资源的读者角色

创建或删除主题：
* 集群资源类型的读者角色
* 每个主题资源类型和主题名称资源的管理员角色

删除使用者组：
* 集群资源类型的读者角色
* 组资源类型和组标识资源的管理员角色

列出组、主题和偏移量，以及描述组、主题和代理程序配置
* 集群资源类型的读者角色














