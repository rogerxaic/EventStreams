---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

<!-- Notes from chat with Charlie 

Different plan for provisioning

Quality of service from each plan

Life of a user through cycle - APIs, feature sets

-->

# Informazioni sul programma Alpha
{: #alpha_about }

Il programma Alpha fornisce un accesso anticipato ai nuovi dispositivi nel servizio {{site.data.keyword.messagehub}}. La funzione corrente è l'introduzione di un nuovo piano, il piano {{site.data.keyword.messagehub}} Premium. 

## Piano Premium 
{: premium_plan}

Il piano Premium è progettato per gli utenti che hanno le prestazioni e altri requisiti funzionali che vanno oltre il servizio pubblico. Fornisce una versione a singolo tenant del servizio {{site.data.keyword.messagehub}} in cui hai l'utilizzo esclusivo di un cluster Apache Kafka. Questa versione di consente di:

* Sfruttare a pieno la capacità e le prestazioni del cluster 

* Avere notevolmente aumentato i limiti e il numero di partizioni 

* Sfruttare la gestione gratuita con un cluster completamente gestito che viene automaticamente conservato e aggiornato.

## Informazioni sul tuo cluster Alpha
{: alpha_cluster}

Il tuo cluster Alpha viene distribuito con Apache Kafka versione 1.1 ed è in grado di fornire una velocità massima di 90 000 KB al secondo. 

Puoi creare un massimo di 1000 partizioni e ciascuna partizione può conservare un massimo di 1 GB di dati fino a 30 giorni. Per la resilienza, i dati vengono archiviati in 3 repliche e l'offset commesso per ogni partizione è conservato per un massimo di 7 giorni. 

Le seguenti due API sono supportate: 

* Per la messaggistica, i client Kafka dalla versione 0.10.x e successive sono supportati, inclusa l'opzione di utilizzare Kafka Streams, Kafka Connect e KSQL. 

* Per la gestione, un'API REST è disponibile per la creazione, l'eliminazione e l'elenco degli argomenti. 

Il programma Alpha è disponibile solo nella regione Stati Uniti Sud. L'accesso ai cluster viene gestito utilizzando IAM. 

## Connessione al tuo cluster 
{: alpha_connect}

Per collegarti a un'API nel cluster hai bisogno del suo URL dell'endpoint e di una apikey per l'autenticazione. Puoi richiamare questi dettagli da IAM utilizzando uno dei seguenti metodi: 

### Applicazione Cloud Foundry
Per un'applicazione Cloud Foundry: 
1. Nella scheda **Connections** dell'applicazione (a sinistra), fai clic sul pulsante **Create connection**. 
2. Selezionare l'istanza del servizio {{site.data.keyword.messagehub}} che desideri collegare e fai clic su **Connect**. Accetta le opzioni predefinite. 
3. Quando la connessione è completa, fai clic sulla scheda **Runtime** dell'applicazione, poi su **Environment variables** per visualizzare **VCAP_SERVICES**.

### Console di un'applicazione esterna
Dalla console di un'applicazione esterna, crea una apikey del servizio utilizzando il seguente **bx**: 

```
bx resource service-key-create name-of-key Manager --instance-name name-of-your-service
``` 

Copia i campi <code>kafka_brokers_sasl</code>, <code>kafka_admin_url</code> e <code>apikey</code> dalle informazioni generate.
Solo i primi cinque broker sono elencati in VCAP_SERVICES. Se hai più di cinque broker, utilizza un client Kafka per richiamare i dettagli dei tuoi altri broker.  

## Connessione di un cliente all'API Kafka 

Per collegare un cliente all'API Kafka, completa la seguente procedura: 

1. Imposta la proprietà <code>bootstrap.servers</code> dei client su un elenco separato da virgole dei broker elencati in <code>kafka_brokers_sasl</code>.

2. Imposta il campo USERNAME <code>sasl.jaas.config</code> dei client con i primi 8 caratteri di <code>apikey</code> e il campo PASSWORD con i rimanenti caratteri (questa suddivisione non sarà necessaria nelle versioni future) 

Il client Kafka che utilizzi deve supportare le seguenti funzioni: 

* Versione client 0.10.x o più recente

* Autenticazione utilizzando il meccanismo SASL Plain 

* L'estensione Service Name Identification (SNI) al protocollo TLS v1.2

Questo metodo di richiamare l'endpoint e le informazioni delle credenziali differisce dal servizio {{site.data.keyword.messagehub}} esistente. Le applicazioni che attualmente esegui con {{site.data.keyword.messagehub}} richiederà delle modifiche per riflettere i nomi del campo alternativi richiesti da VCAP_SERVICES e i campi nome utente/password inoltrati a Kafka. Queste modifiche non verranno richiesti in versioni future di Alpha. 

## Connessione di un cliente all'API REST 

Per collegare un cliente all'API REST, completa la seguente procedura: 

* L'URI dell'API REST viene fornito in <code>kafka_admin_url</code>

* Imposta l'intestazione <code>Content-Type</code> HTTP su <code>application/json</code>

* Imposta l'intestazione <code>X-Auth-Token</code> HTTP sul valore di <code>apikey</code>

Per una procedura semplice per essere pronto in pochi minuti ad utilizzare Alpha, consulta [Introduzione al programma Alpha](/docs/services/MessageHub/messagehub120.html).


## Gestione di {{site.data.keyword.messagehub}}
{: alpha_admin}

Le uniche attività di gestione necessarie in un cluster sono di creare, elencare ed eliminare gli argomenti di cui hai bisogno. Puoi gestire utilizzando uno dei seguenti metodi: 

* Le API di gestione Kafka direttamente dalla tua applicazione. Ad esempio per Java, utilizzando i metodi <code>createTopics()</code>, <code>deleteTopics()</code> o <code>listTopics()</code> da [AdminClient ![Icona link esterno](http://kafka.apache.org/11/javadoc/index.html?org/apache/kafka/clients/admin/AdminClient.html){:new_window}.

* In modo interattivo utilizzando la IU Web dell'istanza del servizio disponibile nel portale IBM Cloud. 

* L'API REST di gestione fornita nel cluster. 

Puoi trovare ulteriori dettagli per creare, elencare ed eliminare le funzioni fornite dall'API REST di gestione (che è compatibile con l'API di gestione {{site.data.keyword.messagehub}} esistente) nella specifica completa dell'API disponibile da [admin-rest-api.yaml ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-messaging/message-hub-docs/blob/master/admin-rest-api/admin-rest-api.yaml){:new_window}.
Per visualizzare il file swagger, usa gli strumenti Swagger, ad esempio [Swagger Editor ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://editor.swagger.io/#/){:new_window}.

Per un esempio semplice che illustra come creare un argomento utilizzando Curl, vedi [Introduzione al programma Alpha](/docs/services/MessageHub/messagehub120.html).

In futuro, saranno disponibili anche altre opzioni di configurazione.


## Esempi

Presto disponibile...

Per una procedura semplice per essere pronto in pochi minuti ad utilizzare Alpha, consulta [Introduzione al programma Alpha](/docs/services/MessageHub/messagehub120.html).

## Limitazioni di Alpha
{: alpha_limitations}

Le attuali limitazioni di questo programma Alpha sono le seguenti:

### Non ancora disponibile, ma lo sarà presto

* Supporto per più zone di disponibilità 

* Limiti di carico e ridimensionamento controllati dall'utente

* Integrazione con il servizio {{site.data.keyword.cloudaccesstrailfull_notm}} (precedentemente noto come AccessTrail)  

### Non attualmente pianificato 

* Bridge

* Messaggistica REST 

* API MQ Light










