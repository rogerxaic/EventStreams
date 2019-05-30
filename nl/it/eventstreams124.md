---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-14"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}
{:important: .important}

# Gestione dell'accesso alle tue risorse {{site.data.keyword.messagehub}}  
{: #security }

Puoi proteggere le tue risorse {{site.data.keyword.messagehub}} in modo dettagliato per gestire l'accesso che vuoi concedere a ogni utente per ogni risorsa.
{: shortdesc}

Quando apporti delle modifiche alle autorizzazioni e politiche IAM, a volte sono necessari diversi minuti prima che vengano riflesse nel servizio sottostante.
{: important}

## Cosa posso proteggere?

In {{site.data.keyword.messagehub}}, puoi proteggere l'accesso alle seguenti risorse:
* Cluster (cluster): puoi controllare quali applicazioni e utenti possono connettersi al servizio
* Argomenti (topic): puoi controllare la capacità di utenti e applicazioni di creare, eliminare, leggere, e scrivere in, un argomento 
* Gruppi di consumatori (group): puoi controllare la capacità di un'applicazione di unirsi a un gruppo di consumatori 
* Transazioni produttore (txnid): puoi controllare la capacità di utilizzare la funzionalità di produttore transazionale in Kafka (ossia, singole scritture atomiche su più partizioni)

I livelli di accesso (noti anche come ruolo) che puoi assegnare a un utente per ciascuna risorsa sono i seguenti:

| Ruolo di accesso | Descrizione delle azioni | Azioni di esempio |
|:-----------------|:-----------------|:-----------------|
|  Lettore | Eseguire azioni di sola lettura in {{site.data.keyword.messagehub}} come ad esempio la visualizzazione di risorse. | Consentire a un'applicazione di stabilire una connessione a un cluster assegnando l'accesso in lettura al tipo di risorsa cluster |
| Scrittore | Gli scrittori hanno autorizzazioni che vanno oltre quelle del ruolo di lettore, comprese la creazione e la modifica di risorsa {{site.data.keyword.messagehub}}. | Consentire a un'applicazione di produrre per gli argomenti assegnando l'accesso in scrittura ai tipi di risorsa argomento e nome argomento|
| Gestore | I gestori hanno autorizzazioni che vanno oltre quelle del ruolo scrittore per completare delle azioni privilegiate. Puoi inoltre creare e modificare risorse {{site.data.keyword.messagehub}}. | Consentire l'accesso completo a tutte le risorse assegnando l'accesso di gestione all'istanza {{site.data.keyword.messagehub}}|
{: caption="Tabella 1. Ruoli utente e azioni di esempio {{site.data.keyword.messagehub}}" caption-side="top"}

<!-- comment from Charlie and my reply 
CM: need to confirm if hierarchical e.g. write includes read - and doc. 
KR: I think they do inherit the lower level access https://console.bluemix.net/docs/iam/users_roles.html#iamusermanrol 
-->


## Come assegno l'accesso?

Le politiche Cloud IAM (Identity and Access Management) sono collegate alle risorse da controllare. Ogni politica definisce il livello di accesso che uno specifico utente deve avere e a quale risorsa o insieme di risorse. Una politica consiste nelle seguenti informazioni: 
* Il tipo di servizio a cui si applica la politica. Ad esempio, {{site.data.keyword.messagehub}}. Puoi definire l'ambito di una politica per includere tutti i tipi di servizio. 
* L'istanza del servizio da proteggere. Puoi definire l'ambito di una politica per includere tutte le istanze di un tipo di servizio. 
* Il tipo di risorsa da proteggere. I valori validi sono <code>cluster</code>, <code>topic</code>, <code>group</code> oppure <code>txnid</code>. La specifica di un tipo è facoltativa. Se non specifichi un tipo, la politica si applica allora a tutte le risorse nell'istanza del servizio. 
* La risorsa da proteggere. Specifica le risorse di tipo <code>topic</code>, <code>group</code> e <code>txnid</code>. Se non specifichi la risorsa, la politica si applica allora a tutte le risorse del tipo specificato nell'istanza del servizio. 
* Il ruolo assegnato all'utente. Ad esempio, Lettore, Scrittore o Gestore. 

## Quali sono le impostazioni di sicurezza predefinite?

Per impostazione predefinita, quando viene eseguito il provisioning di {{site.data.keyword.messagehub}}, all'utente che ne ha eseguito il provisioning viene concesso il ruolo di gestore per tutte le risorse dell'istanza. Inoltre, anche qualsiasi utente che ha un ruolo di gestore per 'tutti' i servizi o 'tutte' le istanze del sevizio {{site.data.keyword.messagehub}} nello stesso account ha l'accesso completo. 

Puoi quindi applicare delle politiche aggiuntive per estendere l'accesso ad altri utenti. Puoi definire l'ambito di una politica da applicare a {{site.data.keyword.messagehub}} nel suo inseme oppure a singole risorse in {{site.data.keyword.messagehub}}. Per ulteriori informazioni, vedi [Scenari comuni](#security_scenarios).

Solo gli utenti con un ruolo di amministrazione per un account possono assegnare delle politiche agli utenti. Assegnare le politiche utilizzando il dashboard IBM Cloud oppure utilizzando i comandi ** ibmcloud **. 
<!--
For example steps for {{site.data.keyword.messagehub}}, see [Examples](#security_examples).
-->


## Scenari comuni
{: #security_scenarios }

Questa tabella riepiloga alcuni scenari {{site.data.keyword.messagehub}} comuni e l'accesso che è necessario assegnare.

| Azione | Ruolo lettore | Ruolo scrittore | Ruolo gestore |
|---------|----------------|
| Consentire l' accesso completo a tutte le risorse|Non applicabile   |Non applicabile  |Istanza del servizio <var class="keyword varname">la_tua_istanza_del_servizio</var>|
| Consentire a un'applicazione o a un utente di creare o eliminare un argomento |Tipo di risorsa: <code>cluster</code>   |Non applicabile  |Tipo di risorsa: argomento <br/><br/>Facoltativo: ID risorsa: <var class="keyword varname">name_of_topic</var> |
| Elencare gruppi, argomenti e offset <br/> Descrivere le configurazioni di broker, gruppo e argomento | Tipo di risorsa: <code>cluster</code>      |Non applicabile  |Non applicabile      |
| Consentire a un'applicazione di stabilire una connessione al cluster  |Tipo di risorsa: <code>cluster</code>| Non applicabile     |Non applicabile      |
| Consentire a un'applicazione di produrre per qualsiasi argomento.  |Tipo di risorsa: <code>cluster</code>|Tipo di risorsa: <code>topic</code> |Non applicabile     |
| Consentire a un'applicazione di produrre per uno specifico argomento.  |Tipo di risorsa: <code>cluster</code>|Tipo di risorsa: <code>topic</code><br/>ID risorsa: <var class="keyword varname">name_of_topic</var>      |Non applicabile     |
| Consentire a un'applicazione di stabilire una connessione a, e consumare da, qualsiasi argomento (nessun gruppo di consumatori)  |Tipo di risorsa: <code>cluster</code> <br/>Tipo di risorsa: <code>topic</code> |Non applicabile    |Non applicabile     |
| Consentire a un'applicazione di stabilire una connessione a, e consumare da, uno specifico argomento (nessun gruppo di consumatori)  | Tipo di risorsa: <code>cluster</code> <br/>Tipo di risorsa: <code>topic</code><br/>ID risorsa: <var class="keyword varname">name_of_topic</var> |Non applicabile     |Non applicabile     |
| Consentire a un'applicazione di consumare un argomento (gruppo di consumatori)  |Tipo di risorsa: <code>cluster</code> <br/>Tipo di risorsa: <code>topic</code><br/> Tipo di risorsa: <code>group</code> |Non applicabile      |Non applicabile     |
| Consentire a un'applicazione di produrre per un argomento in modo transazionale  |Tipo di risorsa: <code>cluster</code> <br/> Tipo di risorsa: <code>group</code>|Tipo di risorsa: <code>topic</code> <br/>ID risorsa: <var class="keyword varname">name_of_topic</var> <br/>Tipo di risorsa: <code>txnid</code> |Non applicabile     |
| Eliminare il gruppo di consumatori |Tipo di risorsa: <code>cluster</code> |Non applicabile  |Tipo di risorsa: <code>group</code> <br/>ID risorsa: <var class="keyword varname">group_ID</var>      |
| Per utilizzare Streams |Tipo di risorsa: <code>cluster</code></br> Tipo di risorsa: <code>group</code>| Non applicabile  |Tipo di risorsa: <code>topic</code>    |

Per ulteriori informazioni su IAM, vedi
[IBM Cloud Identity and Access Management](/docs/iam?topic=iam-iamoverview#iamoverview).

Per un esempio di come impostare le politiche, vedi:
[IBM Cloud IAM Service IDs and API Keys ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/blogs/bluemix/2017/10/introducing-ibm-cloud-iam-service-ids-api-keys/){:new_window}.


## Connessione a {{site.data.keyword.messagehub}}
{: #connect_message_enterprise }

Per informazioni su come eseguire il bind di un'applicazione Cloud Foundry od ottenere una credenziale della chiave di sicurezza per un'applicazione esterna, vedi
[Connessione a {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting).

<!-- 28/06/18 - Karen: draft info only

## Examples
{: #security_examples }

I want to give a user access to create or delete a topic:

1. From the IBM Cloud dashboard, go to the **Manage** tab &gt; **Security** &gt; **Identity and Access**, and then select **Users**.
2. Click **Invite users**.
3. Specify the email address of the user that you want to invite.
4. In the **Access** section, expand the **Services** option.
5. Choose to assign access to a **Resource**.
6. In the **Services** section, select **{{site.data.keyword.messagehub}}**
7. In the **Region** section, make your selection.
8. In the **Service instance** section, locate your instance and select it.
9. In the **Resource type** section, enter **cluster**.
10. In the **Select roles** section, check the **Reader** box.
11. In the **Resource type** section, enter **topic**.
12. In the **Select roles** section, check the **Manager** box.
13. Click **Invite users**.

-->















