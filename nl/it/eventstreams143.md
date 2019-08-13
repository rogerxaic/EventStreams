---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-18"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}

# Upgrade al nuovo piano Standard {{site.data.keyword.messagehub}} 
{: #migrate_classic_plan}

Questa nuova release del piano multitenant Standard offre significativi miglioramenti nella resilienza, nella funzionalità e nell'usabilità. Per ulteriori informazioni, vedi [Annuncio del blog sul nuovo piano Standard ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/cloud/blog/announcements/ibm-event-streams-releases-a-new-and-enhanced-standard-plan). 
{: shortdesc}

Per migrare le applicazioni da un piano Standard precedente (ora denominato piano Classic) al nuovo piano, tieni presenti le seguenti informazioni.

Viene ora eseguito il provisioning delle istanze del servizio come servizi {{site.data.keyword.cloud_notm}} invece che servizi Cloud Foundry. Questo consente al servizio di supportare gli ultimi standard e funzionalità {{site.data.keyword.cloud_notm}}, inclusi le distribuzioni multizona e i controlli dell'accesso granulare, ma comporta delle implicazioni su come viene utilizzato il servizio. In particolare, considera i seguenti aspetti:

## Creazione, eliminazione ed elenco dei servizi 
{: #create_services}

Come in precedenza, puoi gestire il ciclo di vita dei servizi utilizzando la console {{site.data.keyword.cloud_notm}} o lo strumento riga di comando CLI {{site.data.keyword.cloud_notm}}. Se stai utilizzando la console, i servizi sono ora elencati in **Services** invece che in **Cloud Foundry Services**. 

Se stai utilizzando la CLI, le istanze sono gestite utilizzando i comandi resource. Ad esempio, <code>ibmcloud resource service-instance-create</code>. Ciò in sostituzione dei comandi **cf**, ad esempio <code>ibmcloud cf create-service</code>.

## Controllo dell'accesso
{: #controlling_access}

L'autenticazione e l'autorizzazione sono ora gestite utilizzando il servizio Cloud IAM (Identity and Access Management). Oltre a controllare come si connette un utente, IAM ti consente anche di configurare l'accesso granulare alle risorse sottostanti, come ad esempio gli argomenti. L'accesso viene controllando assegnando delle politiche agli utenti. Per ulteriori informazioni, vedi
[Gestione dell'accesso alle tue risorse {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-security).

## Connessione alle applicazioni
{: #connecting_apps}

Le informazioni di cui ha bisogno un'applicazione per la connessione non sono state modificate, ossia, è necessario un elenco di <code>bootstrap.servers</code>, <code>username</code> e <code>password</code>. Tuttavia, il modo in cui vengono richiamate queste proprietà è cambiato.

<ul>
<li>
      <strong>Per le applicazioni native</strong>
        <br/>
        Devi creare un oggetto delle credenziali utilizzando la console IBM Cloud o un oggetto della chiave del servizio utilizzando la CLI. Puoi quindi richiamare le proprietà richieste. Per ulteriori informazioni, vedi
        [Connessione alle applicazioni](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_external).
</li>
<br/>
<li><strong>Per le applicazioni Cloud Foundry</strong>
        <br/>
        Devi prima associare il servizio all'organizzazione e allo spazio dell'applicazione creando un alias del servizio. Puoi quindi richiamare normalmente le proprietà richieste dalla variabile di ambiente VCAP_SERVICES. Per ulteriori informazioni, vedi
        [Connessione a {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting).
</li>
</ul>
<br/>
Tieni presente che i client devono supportare l'estensione SNI per TLS dove il nome dell'host del server viene incluso nell'handshake TLS. Questa funzione è normalmente disponibile e supportata in tutte le versioni del client consigliate in [Scelta di un client Kafka da utilizzare con {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_clients).
</li>
</ul>

<br>
Dovresti inoltre prestare attenzione alle seguenti modifiche:
## Versione Kafka
{: #kafka_version}

Questo piano fornisce l'accesso all'ultima release Kafka stabile 2.2. Le applicazioni sviluppate con Kafka 1.1 possono essere eseguite invariate, ma fai riferimento alle seguenti informazioni per le versioni del client consigliate e le combinazioni verificate: [Scelta di un client Kafka da utilizzare con {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_clients). 

## Regioni supportate
{: #supported_regions}

Il piano è disponibile nelle seguenti regioni:
* Dallas (us-south)
* Washington (us-east)
* Londra (eu-gb)
* Sydney (au-syd)
* Francoforte (eu-de)
* Tokyo (jp-tok)

## Integrazioni
{: #integrations}

La connessione da altri servizi, come ad esempio {{site.data.keyword.iot_short_notm}} o {{site.data.keyword.openwhisk_short}}, che si collegano al servizio utilizzando un'organizzazione e uno spazio Cloud Foundry richiedono un alias del servizio per poter essere creati. Per ulteriori informazioni, vedi
    [Connessione delle applicazioni Cloud Foundry a {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_cf).

## Funzionalità supportate
{: #capabilities}

Esistono delle differenze tra le funzionalità del piano Classic e il nuovo piano Standard. Per allineare le offerte del prodotto, utilizza le nuove scelte tecnologiche e rimuovi le funzioni meno utilizzate, non tutte le funzionalità vengono riportate. Un confronto delle funzioni è disponibile in [Scelta del tuo piano](/docs/services/EventStreams?topic=eventstreams-plan_choose). Se utilizzi queste funzioni, saranno fornite a breve ulteriori informazioni per aiutarti nella migrazione.

<br/>
Piccoli delta di codice sono spediti giornalmente alla produzione. Di conseguenza, puoi attenderti di vedere molti ulteriori miglioramenti sulla nostra esperienza utente (e in altre aree) per tutto il resto del 2019 e oltre. Presto disponibile:

* **Metriche personalizzate**
    La possibilità di monitorare l'attività in un'istanza del servizio.

<br/>
Per una veloce spiegazione della procedura su come essere operativo con il nuovo piano Standard, prova l'[Esercitazione introduttiva](/docs/services/EventStreams?topic=eventstreams-getting_started).


