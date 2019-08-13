---

copyright:
  years: 2016, 2018
lastupdated: "2019-05-14"

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

# Sucesos de {{site.data.keyword.cloudaccesstrailshort}} 
{: #at_events}

Utilice el servicio {{site.data.keyword.cloudaccesstrailfull}} para realizar el seguimiento de cómo interactúan los usuarios y las aplicaciones con el servicio {{site.data.keyword.messagehub}} en los planes Estándar y Empresa de {{site.data.keyword.Bluemix}}. 
{: shortdesc}

El servicio {{site.data.keyword.cloudaccesstrailfull_notm}} registra actividades iniciadas por el usuario que cambian el estado de un servicio en {{site.data.keyword.Bluemix_notm}}. Para obtener más información, consulte [{{site.data.keyword.cloudaccesstrailshort}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started){:new_window}.

<!-- You can create different sections to group events by area. -->

## Lista de sucesos
{: #events}

<!-- Make sure you introduce the table with a detailed description that immediately precedes it. For example, see https://cloud.ibm.com/docs/services/cloud-activity-tracker/services?topic=cloud-activity-tracker-cf#catalog. -->

{{site.data.keyword.messagehub}}, en los planes Estándar y Empresa, genera sucesos automáticamente, para que se pueda realizar el seguimiento de la actividad del servicio.

| Acción | Descripción |
|:-------|:------------|
| event-streams.topic.create | Se crea un suceso al crear un tema|
| event-streams.topic.delete | Se crea un suceso al suprimir un tema|
{: caption="Tabla 1. Sucesos de {{site.data.keyword.messagehub}}" caption-side="top"}

## Dónde ver los sucesos
{: #ui}

<!-- For example, choose one of the following two options. -->

<!-- Option 2: Add the following sentence if your service sends events to the account domain. -->

Los sucesos de {{site.data.keyword.cloudaccesstrailshort}} están disponibles en el **dominio de cuenta** de {{site.data.keyword.cloudaccesstrailshort}} que está disponible en la ubicación (región) {{site.data.keyword.Bluemix_notm}} en la que se generan los sucesos.










