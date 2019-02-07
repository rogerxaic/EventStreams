---

copyright:
  years: 2015, 2019
lastupdated: "2018-12-17"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 向 {{site.data.keyword.messagehub}} 团队报告问题 - 企业套餐
{: #report_problem}

如果使用 {{site.data.keyword.messagehub}} 时遇到问题，请首先检查 [{{site.data.keyword.Bluemix_notm}} 状态页面 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/status){:new_window}。
{: shortdesc}

如果希望获得 {{site.data.keyword.messagehub}} 团队的帮助，请收集以下所有信息。预先提供的信息越多，团队帮助您解决问题的效率就会越高：
{:shortdesc}

1. 您使用的 {{site.data.keyword.messagehub}} 服务的 CRN 标识是什么？您可以在单击该服务后通过粘贴完整的 {{site.data.keyword.Bluemix_notm}} 控制台 URL 来提供此标识，也可以通过粘贴以下 CLI 命令的输出来提供此标识：<br/>
   <pre class="pre">
   ibmcloud resource service-instance NAME
   </pre>
	{: codeblock}
2. 第一次出现问题是什么时候（具体到时间、日期和时区）？在此之前，应用程序已经运行了多久？
3. 问题仍会发生吗？能重现问题吗？
4. 您的应用程序使用的是哪个 Kafka 客户机？版本详细信息是什么？
5. 客户机配置详细信息是什么？我们需要了解生产者和使用者设置，因此请列出您传递给生产者或使用者创建的任何非缺省选项。
6. 有应用程序日志片段显示此问题吗？
7. 您遇到的问题是什么？哪些主题、客户机标识、组标识和事务标识受影响？
8. 问题对服务有什么影响？

您可以通过[提交支持请求 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/get-support/howtogetsupport.html#using-avatar)，在支持凭单中向 IBM 提供所收集的信息。










