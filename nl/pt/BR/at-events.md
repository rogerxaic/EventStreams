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

# {{site.data.keyword.cloudaccesstrailshort}}  events (Enterprise plan)
{: #at_events}

Use o serviço {{site.data.keyword.cloudaccesstrailfull}} para controlar como os usuários e os aplicativos interagem com o serviço {{site.data.keyword.messagehub}} no plano Enterprise no {{site.data.keyword.Bluemix}}. 
{: shortdesc}

O serviço {{site.data.keyword.cloudaccesstrailfull_notm}} registra atividades iniciadas pelo usuário que mudam o estado de um serviço no {{site.data.keyword.Bluemix_notm}}. Para obter mais informações, veja o  [ {{site.data.keyword.cloudaccesstrailshort}} ](/docs/services/cloud-activity-tracker/index.html#getting-started-with-cla).

<!-- You can create different sections to group events by area. -->

## Lista de eventos
{: #events}

<!-- Make sure you introduce the table with a detailed description that immediately precedes it. For example, see https://console.bluemix.net/docs/services/cloud-activity-tracker/services/at_events_cf.html#catalog. -->

O {{site.data.keyword.messagehub}} no plano Enterprise gera eventos automaticamente para que você possa controlar a atividade em seu serviço.

| Ações | Descrição |
|:-------|:------------|
| messagehub.topic.create | Um evento é criado quando você cria um tópico|
| messagehub.topic.delete | Um evento é criado quando você exclui um tópico|
{: caption="Tabela 1. Eventos do {{site.data.keyword.messagehub}}" caption-side="top"}

## Onde visualizar os eventos
{: #ui}

<!-- For example, choose one of the following two options. -->

<!-- Option 2: Add the following sentence if your service sends events to the account domain. -->

Os eventos do {{site.data.keyword.cloudaccesstrailshort}} estão disponíveis no **domínio de contas** do {{site.data.keyword.cloudaccesstrailshort}} que está disponível na localização (região) do {{site.data.keyword.Bluemix_notm}} em que os eventos são gerados.










