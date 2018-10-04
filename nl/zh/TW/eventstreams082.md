---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# MQ Light API 範例
{: #mql_samples}

** MQ Light API 只提供於標準方案中。**
<br/>

如果您尚沒有應用程式，請用其中一個範例來試試 {{site.data.keyword.mql}} API。
{:shortdesc}

範例應用程式實際上包含兩個簡單的應用程式：Web 前端（負責傳送訊息給後端），以及後端（負責處理訊息、將文字改為大寫，然後將訊息傳回前端）。此範例顯示如何使用 {{site.data.keyword.mql}} API 讓應用程式彼此對談。您也可以使用 {{site.data.keyword.mql}} API 執行工作者卸載，這是建立可擴充、鬆散耦合且分散式應用程式所需的重要功能。

範例程式碼位於 [event-streams-samples GitHub 專案 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-messaging/event-streams-samples/tree/master/mqlight){:new_window}。
