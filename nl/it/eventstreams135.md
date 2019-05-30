---

copyright:
  years: 2015, 2019
lastupdated: "2018-11-07"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Connessione a {{site.data.keyword.messagehub}} utilizzando il piano Classic 
{: #connecting_classic}

Il modo in cui stabilisci una connessione a {{site.data.keyword.messagehub}} varia a seconda se lo stai facendo da un'applicazione Cloud Foundry o da un qualsiasi altro client esterno. Devi raccogliere due informazioni per stabilire una connessione a una qualsiasi delle API di {{site.data.keyword.messagehub}}:
{: shortdesc}

* Gli URL endpoint per le API
* Le credenziali per l'autenticazione

Leggi le seguenti informazioni su come ottenere questi dettagli. I passi possono variare leggermente per garantire che tu completi la procedura appropriata per la tua istanza.

## Esegui il provisioning di un'istanza {{site.data.keyword.messagehub}}

Come prerequisito, devi prima eseguire il provisioning di un'istanza del servizio {{site.data.keyword.messagehub}} per il piano Classic. Ottieni quindi i dettagli della connessione API {{site.data.keyword.messagehub}} completando le seguenti attività:

## Panoramica del piano Classic
{: #connect_classic_plan}

I servizi di cui viene eseguito il provisioning utilizzando il piano Classic sono servizi Cloud Foundry. Questo significa che vengono distribuiti in un'organizzazione e uno spazio Cloud Foundry e vengono raggruppati nel dashboard sotto l'intestazione **Servizi Cloud Foundry**. Il metodo che utilizzi per collegare un'applicazione dipende da dove viene distribuita l'applicazione, ossia in Cloud Foundry o all'esterno di Cloud Foundry, ad esempio nel servizio Kubernetes.


## Applicazioni Cloud Foundry nel piano Classic
{: #connect_classic_cf_plan}

Per le applicazioni in esecuzione all'interno di Cloud Foundry, esegui il bind della tua applicazione all'istanza del servizio {{site.data.keyword.messagehub}}. Una volta eseguito il bind, i dettagli della connessione vengono resi disponibili all'applicazione in formato JSON nella variabile di ambiente VCAP_SERVICES. Puoi eseguire il bind di un'applicazione e di un servizio utilizzando la console IBM Cloud oppure la CLI IBM Cloud.

Ecco un esempio di VCAP_SERVICES:

```
{
  "credentials": {
    "mqlight_lookup_url": "https://mqlight-lookup.messagehub.services.us-south.bluemix.net/Lookup?serviceId=584e8436-e7f5-43db-96ac-2864fccae5ae",
    "api_key": <YOUR_API_KEY>,
    "kafka_admin_url": "https://kafka-admin.messagehub.services.us-south.bluemix.net:443",
    "kafka_rest_url": "https://kafka-rest.messagehub.services.us-south.bluemix.net:443",
    "kafka_brokers_sasl": [
      "kafka01.messagehub.services.us-south.bluemix.net:9093",
      "kafka02.messagehub.services.us-south.bluemix.net:9093",
      "kafka03.messagehub.services.us-south.bluemix.net:9093",
      "kafka04.messagehub.services.us-south.bluemix.net:9093",
      "kafka05.messagehub.services.us-south.bluemix.net:9093"
    ],
    "user": "d9JSx1SYsmLzNRbb",
    "password": "gUFneDm2DtkedlVeViObYJIvrPAf2kJA"
  }
}
```

{: codeblock}

Il contenuto della variabile di ambiente è lo stesso, indipendentemente dall'API che usi per stabilire una connessione a {{site.data.keyword.messagehub}}. La tua applicazione {{site.data.keyword.Bluemix_notm}} seleziona le credenziali appropriate dalla variabile di ambiente VCAP_SERVICES, a seconda dell'interfaccia in
uso.
 
Solo i primi cinque broker sono elencati in VCAP_SERVICES. Se hai più di cinque broker, utilizza un client Kafka per richiamare i dettagli degli altri tuoi broker. 


### Ottieni le credenziali e stabilisci la connessione utilizzando la console IBM Cloud
{: #connect_classic_plan_cf_console }

1. Assicurati di trovarti nell'organizzazione e nello spazio Cloud Foundry previsti.
2. Individua la tua applicazione Cloud Foundry nel dashboard. Se non hai ancora un'applicazione Cloud Foundry, puoi crearne una facendo clic sul pulsante **Crea risorsa**,
3. Fai clic sul tuo tile dell'applicazione.
4. Fai clic su **Connessioni**.
5. Fai clic su **Crea connessione**.
6. Seleziona il tile del servizio {{site.data.keyword.messagehub}} a cui vuoi eseguire il bind e fai clic su **Connetti**. Potresti dover ripreparare la tua applicazione per rendere effettive le modifiche.
7. Fai clic sulla scheda **Runtime** a sinistra e seleziona la scheda **Variabili di ambiente** al centro. Puoi ora verificare le tue informazioni VCAP_SERVICES e la tua applicazione può ora accedevi come variabili di ambiente. 


### Ottieni le credenziali utilizzando la CLI IBM Cloud 
{: #connect_classic_plan_cf_cli }

<ol>
<li>Assicurati di trovarti nell'organizzazione e nello spazio Cloud Foundry previsti. Puoi spostarti in modo interattivo eseguendo questo comando:<br/>
 <code>ibmcloud target --cf</code>
</li>
<li>Trova la tua applicazione:<br/> <code>ibmcloud app list</code> <br/>
</br>
Se hai un file manifest, puoi creare una nuova applicazione eseguendo:</br>
<code>ibmcloud app push</code>
</li>
<li>Trova il tuo servizio:</br>
<code>ibmcloud service list</code>
</li>
<li>Esegui il bind della tua applicazione al servizio:</br>
<code>ibmcloud service bind <var class="keyword varname">your_app_name</var> <var class="keyword varname">your_service_name</var></code>
</li>
<li>Verifica che la variabile di ambiente VCAP_SERVICES sia disponibile nel runtime dell'applicazione eseguendo:</br>
 <code>ibmcloud app env <var class="keyword varname">your_app_name</var></code>.
</li>
<li>Passa queste credenziali alla tua applicazione. Specifica <code>token</code> come tuo nome utente e la <var class="keyword varname">api_key</var> come tua password. Separa <code>token</code> e la <var class="keyword varname">api_key</var> con un carattere due punti. Per ulteriori informazioni, vedi [Configurazione del tuo client](/docs/services/EventStreams?topic=eventstreams-kafka_connect).
<p>Potresti dover ripreparare la tua applicazione per rendere effettive le modifiche.</p></li>
</ol>

## Applicazioni esterne sul piano Classic
{: #connect_classic_plan_external}

Per le applicazioni in esecuzione esternamente a Cloud Foundry, le credenziali vengono generate creando una chiave di servizio. Dopo che hai ottenuto una chiave di servizio, passa manualmente i dettagli della chiave alla tua applicazione utilizzando il metodo da te scelto.

### Ottieni le credenziali utilizzando la console IBM Cloud
{: #connect_classic_plan_external_console}

1. Assicurati di trovarti nell'organizzazione e nello spazio Cloud Foundry previsti.
2. Individua il tuo servizio {{site.data.keyword.messagehub}} Cloud Foundry nel dashboard.
3. Fai clic sul tuo tile del servizio.
4. Fare clic su **Credenziali del servizio**.
5. Fai clic su ** Nuova credenziale**.
6. Immetti i dettagli per la tua nuova credenziale, come ad esempio un nome, e fai clic su **Aggiungi**. Viene visualizzata una nuova credenziale nell'elenco delle credenziali.
7. Fai clic su questa credenziale utilizzando **Visualizza credenziali** per rivelare i dettagli in formato JSON.
8. Passa queste credenziali alla tua applicazione. Specifica <code>token</code> come tuo nome utente e la <var class="keyword varname">api_key</var> come tua password. Separa <code>token</code> e la <var class="keyword varname">api_key</var> con un carattere due punti. Per ulteriori informazioni, vedi [Configurazione del tuo client](/docs/services/EventStreams?topic=eventstreams-kafka_connect).

### Ottieni le credenziali utilizzando la CLI IBM Cloud 
{: #connect_classic_plan_external_cli }

<ol>
<li>Assicurati di trovarti nell'organizzazione e nello spazio Cloud Foundry previsti. Puoi spostarti in modo interattivo eseguendo questo comando:<br>
<code>ibmcloud target --cf</code>
</li>
<li>Trova il tuo servizio:<br>
<code>ibmcloud service list</code>
</li>
<li>Puoi creare una chiave di servizio:<br>
<code>ibmcloud service key-create <var class="keyword varname">tuo_nome_servizio</var> <var class="keyword varname">nome_della_nuova_chiave_di_servizio</var></code><br>
<br/>
oppure utilizzare una chiave di servizio esistente: <br/>
<code>ibmcloud service keys <var class="keyword varname">your_service_name</var></code>
</li>
<li>Ottieni i dettagli per la chiave:</br>
<code>ibmcloud service key-show <var class="keyword varname">your_service_name</var> <var class="keyword varname">service _key_name</var></code></br>
Questo restituisce i dettagli della chiave di servizio in formato JSON.</li>
<li>Passa queste credenziali alla tua applicazione. Specifica <code>token</code> come tuo nome utente e la <var class="keyword varname">api_key</var> come tua password. Separa <code>token</code> e la <var class="keyword varname">api_key</var> con un carattere due punti. Per ulteriori informazioni, vedi [Configurazione del tuo client](/docs/services/EventStreams?topic=eventstreams-kafka_connect).</li>
</ol>
 
## Cosa fare successivamente
{: #after_connecting_next}

Ora che disponi delle informazioni di connessione e credenziale, puoi scegliere un client {{site.data.keyword.messagehub}}. Vedi
[Scegli tra le tre API](/docs/services/EventStreams?topic=eventstreams-choose_api_classic) per informazioni su quale client scegliere e come stabilire la connessione.










 















