---

copyright:
  years: 2015, 2019
lastupdated: "2018-06-01"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Watson IoT Platform 橋接器
{: #consuming_messages_watson}

{{site.data.keyword.iot_full}} 提供一個受管理的橋接器，容許您建立與 {{site.data.keyword.messagehub_full}} 的單向鏈結。
{: shortdesc}

將 {{site.data.keyword.messagehub}} 連接至 {{site.data.keyword.iot_short_notm}}，表示您可以使用 {{site.data.keyword.messagehub}} 作為事件管線，以從 {{site.data.keyword.iot_short_notm}} 中取用裝置事件，並即時將事件提供給平台的其餘部分使用。 

常見的模式是使用 {{site.data.keyword.iot_short_notm}} 橋接器、{{site.data.keyword.messagehub}} 及 Cloud Object Storage 橋接器來建立端對端管線，這讓即時及批次互動更為容易。

如需如何建立此橋接器的相關資訊，請參閱：[Connecting and configuring a historian connector to use {{site.data.keyword.messagehub}}  ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/support/knowledgecenter/SSQP8H/iot/platform/reference/dsc/eventstreams.html){:new_window}。






