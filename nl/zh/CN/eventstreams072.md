---

copyright:
  years: 2015, 2018
lastupdated: "2018-07-31"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


#监视和日志记录（标准套餐）
{: #monitoring}

标准套餐中的 {{site.data.keyword.messagehub}} 会自动收集度量值和事件，以便您可以监视 {{site.data.keyword.messagehub}} 的使用情况。
{:shortdesc}

**注：**度量值和事件会作为 {{site.data.keyword.messagehub}} 标准套餐的一部分提供，但仅限美国南部、英国和悉尼区域。 


您可以监视以下日志信息：

<dl>
<dt>主题度量</dt>
<dd>我们为您的每个主题送入和送出字节数（每 15 分钟检查一次）。可以通过单击 {{site.data.keyword.Bluemix_notm}} 控制台中 {{site.data.keyword.messagehub}} 仪表板上的 **Grafana** 按钮来访问这些度量。</dd>
<dt>主题事件</dt>
<dd>您每次创建或删除主题时，我们还推送事件。可以通过单击 {{site.data.keyword.Bluemix_notm}} 控制台中 {{site.data.keyword.messagehub}} 仪表板上的 **Kibana** 按钮来访问这些事件。</dd>
</dl>


我们建议不要编辑 {{site.data.keyword.messagehub}} 仪表板，因为 {{site.data.keyword.messagehub}} 会进行更新，这可能会覆盖您的更改。但是，您可以在您自己的仪表板中包含这些度量和事件。


