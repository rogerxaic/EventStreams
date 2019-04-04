---

copyright:
  years: 2016, 2018
lastupdated: "2018-08-14"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:table: .aria-labeledby="caption"}

<!-- Name your file `at-events.md` and include it in the Reference nav group in your toc file. -->

# Eventi {{site.data.keyword.cloudaccesstrailshort}} (piano Enterprise)
{: #at_events}

Utilizza il servizio {{site.data.keyword.cloudaccesstrailfull}} per tenere traccia di come gli utenti e le applicazioni interagiscono con il servizio {{site.data.keyword.messagehub}} nel piano Enterprise in {{site.data.keyword.Bluemix}}. 
{: shortdesc}

Il servizio {{site.data.keyword.cloudaccesstrailfull_notm}} registra le attività avviate dall'utente che modificano lo stato di un servizio in {{site.data.keyword.Bluemix_notm}}. Per ulteriori informazioni, vedi [{{site.data.keyword.cloudaccesstrailshort}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started-with-cla#getting-started-with-cla){:new_window}.

<!-- You can create different sections to group events by area. -->

## Elenco degli eventi
{: #events}

<!-- Make sure you introduce the table with a detailed description that immediately precedes it. For example, see https://console.bluemix.net/docs/services/cloud-activity-tracker/services/at_events_cf.html#catalog. -->

{{site.data.keyword.messagehub}} nel piano Enterprise genera automaticamente gli eventi in modo che puoi tenere traccia dell'attività sul tuo servizio.

| Azione | Descrizione |
|:-------|:------------|
| messagehub.topic.create | Un evento viene creato quando crei un argomento|
| messagehub.topic.delete | Un evento viene creato quando elimini un argomento|
{: caption="Tabella 1. Eventi {{site.data.keyword.messagehub}}" caption-side="top"}

## Dove visualizzare gli eventi
{: #ui}

<!-- For example, choose one of the following two options. -->

<!-- Option 2: Add the following sentence if your service sends events to the account domain. -->

Gli eventi {{site.data.keyword.cloudaccesstrailshort}} sono disponibili nel **dominio dell'account** {{site.data.keyword.cloudaccesstrailshort}} disponibile nell'ubicazione (regione) {{site.data.keyword.Bluemix_notm}} in cui vengono generati gli eventi.










