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


#監視及記載（標準方案）
{: #monitoring}

標準方案上的 {{site.data.keyword.messagehub}} 會自動收集度量值及事件，以便您可以監視 {{site.data.keyword.messagehub}} 的用量。
{:shortdesc}

**附註：**度量值及事件只適用於美國南部、英國及雪梨的 {{site.data.keyword.messagehub}} 標準方案。 


您可以監視下列日誌資訊：

<dl>
<dt>主題度量值</dt>
<dd>我們會傳送針對您的每個主題輸入及輸出的位元組數（每隔 15 分鐘設定一個檢查點）。您可以在 {{site.data.keyword.Bluemix_notm}} 主控台的 {{site.data.keyword.messagehub}} 儀表板上，按一下
**Grafana** 按鈕來存取這些度量值。
</dd>
<dt>主題事件</dt>
<dd>我們也會在每次建立或刪除主題時推送事件。您可以在 {{site.data.keyword.Bluemix_notm}} 主控台的 {{site.data.keyword.messagehub}} 儀表板上，按一下
**Kibana** 按鈕來存取這些事件。
</dd>
</dl>


建議您不要編輯 {{site.data.keyword.messagehub}} 儀表板，因為 {{site.data.keyword.messagehub}} 會進行更新，可能會改寫您的變更。不過，您可以將這些度量值及事件包含在您專屬的儀表板中。


