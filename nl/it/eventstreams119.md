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


# Bridge Watson IoT Platform
{: #consuming_messages_watson}

{{site.data.keyword.iot_full}} fornisce un bridge gestito che ti consente di creare un collegamento unidirezionale a {{site.data.keyword.messagehub_full}}.
{: shortdesc}

Connettere {{site.data.keyword.messagehub}} a {{site.data.keyword.iot_short_notm}} significa che puoi usare {{site.data.keyword.messagehub}} come una pipeline di eventi per consumare i tuoi eventi di dispositivo da {{site.data.keyword.iot_short_notm}} e per rendere gli eventi disponibili in tempo reale al resto della piattaforma. 

Un modello comune consiste nell'utilizzare il bridge {{site.data.keyword.iot_short_notm}}, {{site.data.keyword.messagehub}} e il bridge Cloud Object Storage per creare una pipeline end-to-end, il che rende più facili le interazioni in tempo reale e quelle batch.

Per informazioni su come creare questo bridge, vedi: [Connecting and configuring a historian connector to use {{site.data.keyword.messagehub}}  ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/support/knowledgecenter/SSQP8H/iot/platform/message_hub.html){:new_window}.






