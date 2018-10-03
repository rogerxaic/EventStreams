---

copyright:
  years: 2016, 2018
lastupdated: "2018-08-14"

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

# {{site.data.keyword.cloudaccesstrailshort}}-Ereignisse (Plan "Enterprise")
{: #at_events}

Mit dem {{site.data.keyword.cloudaccesstrailfull}}-Service können Sie im Rahmen des Plans "Enterprise" verfolgen, wie Benutzer und Anwendungen mit dem {{site.data.keyword.messagehub}}-Service in {{site.data.keyword.Bluemix}} interagieren.
{: shortdesc}

Der {{site.data.keyword.cloudaccesstrailfull_notm}}-Service zeichnet vom Benutzer initiierte Aktivitäten auf, die eine Statusänderung eines Service in {{site.data.keyword.Bluemix_notm}} bewirken. Weitere Informationen finden Sie in [{{site.data.keyword.cloudaccesstrailshort}}](/docs/services/cloud-activity-tracker/index.html#getting-started-with-cla).

<!-- You can create different sections to group events by area. -->

## Ereignisliste
{: #events}

<!-- Make sure you introduce the table with a detailed description that immediately precedes it. For example, see https://console.bluemix.net/docs/services/cloud-activity-tracker/services/at_events_cf.html#catalog. -->

{{site.data.keyword.messagehub}} generiert im Rahmen des Plans "Enterprise" automatisch Ereignisse, mit denen Sie Aktivitäten verfolgen können, die für Ihren Service ausgeführt werden.

| Aktion | Beschreibung |
|:-------|:------------|
| messagehub.topic.create | Beim Erstellen eines Topics wird ein Ereignis erstellt|
| messagehub.topic.delete | Beim Löschen eines Topics wird ein Ereignis erstellt|
{: caption="Tabelle 1. {{site.data.keyword.messagehub}}-Ereignisse" caption-side="top"}

## Ereignisse anzeigen
{: #ui}

<!-- For example, choose one of the following two options. -->

<!-- Option 2: Add the following sentence if your service sends events to the account domain. -->

{{site.data.keyword.cloudaccesstrailshort}}-Ereignisse stehen in der {{site.data.keyword.cloudaccesstrailshort}}-**Kontodomäne** zur Verfügung, die in der {{site.data.keyword.Bluemix_notm}}-Region verfügbar ist, in der die Ereignisse generiert werden.










