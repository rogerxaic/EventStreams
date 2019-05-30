---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-15"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}


# Connessione a {{site.data.keyword.messagehub}}
{: #connecting}

Il modo in cui stabilisci una connessione a {{site.data.keyword.messagehub}} varia a seconda del fatto che la tua applicazione sia in esecuzione in modo nativo o come un'applicazione Cloud Foundry. Tuttavia, in entrambi i casi sono necessarie alcune informazioni:
{: shortdesc}

* Gli URL endpoint per le API
* Le credenziali per l'autenticazione

Leggi le seguenti informazioni su come ottenere questi dettagli. I passi possono variare leggermente per garantire che tu completi la procedura appropriata per la tua istanza.

Per informazioni su come eseguire la connessione a {{site.data.keyword.messagehub}} se stai utilizzando il piano Classic, vedi [Connessione utilizzando il piano Classic](/docs/services/EventStreams?topic=eventstreams-connecting_classic).


## Panoramica
{: #connect_enterprise}

I servizi di cui è stato eseguito il provisioning utilizzando i piani Standard e Enterprise sono raggruppati nel dashboard sotto l'intestazione **Servizi**. I piani sono [abilitati IAM ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/iam?topic=iam-getstarted#getstarted){:new_window}. Non devi necessariamente comprendere IAM per iniziare a lavorare ma una certa dimestichezza è consigliata, se vuoi proteggere il tuo servizio {{site.data.keyword.messagehub}}. Per ulteriori informazioni, vedi
[Gestione dell'accesso alle tue risorse {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-security).

Completa la seguente procedura per eseguire il bind della tua applicazione e ottenere le chiavi di servizio per il tuo servizio. Per disporre dell'autorizzazione a creare gli argomenti, la tua applicazione o la chiave di servizio devono disporre di un ruolo di accesso di Gestore.

Per collegare un'applicazione, il metodo utilizzato dipende da dove viene distribuita l'applicazione, ossia in Cloud Foundry o all'esterno di Cloud Foundry, ad esempio nel servizio Kubernetes.

## Esegui il provisioning di un'istanza {{site.data.keyword.messagehub}}

Come prerequisito, devi prima eseguire il provisioning di un'istanza del servizio {{site.data.keyword.messagehub}} per il piano Standard o quello Enterprise. Ottieni quindi i dettagli della connessione API {{site.data.keyword.messagehub}} completando le seguenti attività:

## Connetti le applicazioni 
{: #connect_enterprise_external}

Per le applicazioni in esecuzione esternamente a Cloud Foundry, le credenziali vengono generate creando una chiave di servizio. Dopo che hai ottenuto una chiave di servizio, passa manualmente i dettagli della chiave alla tua applicazione utilizzando il metodo da te scelto.

### Ottieni le credenziali utilizzando la console IBM Cloud
{: #connect_enterprise_external_console}

1. Individua il tuo servizio {{site.data.keyword.messagehub}} nel dashboard.
2. Fai clic sul tuo tile del servizio.
3. Fare clic su **Credenziali del servizio**.
4. Fai clic su ** Nuova credenziale**. 
5. Completa i dettagli per la tua nuova credenziale, come ad esempio un nome e un ruolo, e fai clic su **Aggiungi**. Viene visualizzata una nuova credenziale nell'elenco delle credenziali.
6. Fai clic su questa credenziale utilizzando **Visualizza credenziali** per rivelare i dettagli in formato JSON.
7. Passa queste credenziali alla tua applicazione. Specifica <code>token</code> come tuo nome utente e la <var class="keyword varname">api_key</var> come tua password. Separa <code>token</code> e la <var class="keyword varname">api_key</var> con un carattere due punti. Per ulteriori informazioni, vedi [Configurazione del tuo client API Kafka](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_api_client).
   <br/><br/>Assicurati che la tua applicazione analizzi i dettagli.

### Ottieni le credenziali utilizzando la CLI IBM Cloud
{: #connect_enterprise_external_cli}

<ol>
<li>Individua il tuo servizio:<br/>
<code>ibmcloud resource service-instances</code></li>
<li>Crea una chiave del servizio:<br/>
<code>ibmcloud resource service-key-create <var class="keyword varname">key_name</var> <var class="keyword varname">key_role</var> --instance-name <var class="keyword varname">your_service_name</var></code></li>
<li>Stampa la chiave del servizio:<br/>
<code>ibmcloud resource service-key <var class="keyword varname">key_name</var></code></li>
<li>Passa queste credenziali alla tua applicazione. Specifica <code>token</code> come tuo nome utente e la <var class="keyword varname">api_key</var> come tua password. Separa <code>token</code> e la <var class="keyword varname">api_key</var> con un carattere due punti. Per ulteriori informazioni, vedi [Configurazione del tuo client](/docs/services/EventStreams?topic=eventstreams-kafka_connect).
<p>Assicurati che la tua applicazione analizzi i dettagli.</p></li>
</ol>

## Connetti le applicazioni Cloud Foundry
{: #connect_enterprise_cf}

È necessario che della tua applicazione sia stato eseguito il bind all'istanza del servizio {{site.data.keyword.messagehub}}. Per eseguire il bind di un'applicazione Cloud Foundry a un servizio non Cloud Foundry, crea prima un alias del servizio Cloud Foundry e fai quindi riferimento a questo alias dalla tua applicazione Cloud Foundry quando esegui il bind. 

Una volta eseguito il bind, i dettagli della connessione vengono resi disponibili alla tua applicazione in formato JSON utilizzando la variabile di ambiente VCAP_SERVICES. Puoi eseguire il bind di un'applicazione e di un servizio utilizzando la console [IBM Cloud](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_cf_console) o la CLI [IBM Cloud](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_cf_cli).

### Esegui il bind di un'applicazione utilizzando la console IBM Cloud
{: #connect_enterprise_cf_console}

1. Assicurati di trovarti nell'organizzazione e nello spazio Cloud Foundry previsti.
2. Individua la tua applicazione Cloud Foundry sul dashboard oppure crea un'applicazione facendo clic sul pulsante **Crea risorsa**.
3. Fai clic sul tuo tile dell'applicazione.
4. Fai clic su **Connessioni**.
5. Fai clic su **Crea connessione**.
6. Seleziona il tile del servizio {{site.data.keyword.messagehub}} a cui vuoi eseguire il bind e fai clic su **Connetti**. 
7. Nella finestra **Connect IAM-Enabled Service** che viene visualizzata, seleziona un ruolo di accesso da **Access Role for Connection** e un ID servizio dall'elenco **Service ID for Connection** (puoi accettare l'ID generato automaticamente). Fai clic su **Connect**. 

  Questo crea un alias del servizio Cloud Foundry per il tuo servizio {{site.data.keyword.messagehub}} ed esegue quindi il bind della tua applicazione a questo alias. 

  Riprepara la tua applicazione per rendere effettive le modifiche.<br/>
8. Fai clic sulla scheda **Runtime** a sinistra e seleziona la scheda **Variabili di ambiente** al centro. Puoi ora verificare le tue informazioni VCAP_SERVICES . Alla tua applicazione è ora possibile accedere come a variabili di ambiente. 
 

### Esegui il bind di un'applicazione utilizzando la CLI IBM Cloud
{: #connect_enterprise_cf_cli}

<ol>
<li>Assicurati di trovarti nell'organizzazione e nello spazio Cloud Foundry previsti. Puoi spostarti in modo interattivo eseguendo questo comando:<br/>
 <code>ibmcloud target --cf</code></li>
<li>Individua la tua applicazione::</br>
<code>ibmcloud app list</code><br/>
<br/>
Se hai un file manifest, puoi creare una nuova applicazione eseguendo:<br/>
<code>ibmcloud app push</code><br/>
<br/>
Poiché della tua applicazione non è stato ancora eseguito il bind a {{site.data.keyword.messagehub}}, l'applicazione non può stabilire una connessione. Pertanto, ti consigliamo di eseguire il push dell'applicazione con il parametro <code>--no-start</code> per evitare errori di connessione non necessari.</li>
<li>Individua il tuo servizio:</br>
<code>ibmcloud resource service-instances</code></li>
<li>Crea un alias del servizio Cloud Foundry:<br/>
<code>ibmcloud resource service-alias-create <var class="keyword varname">alias_name</var> --instance-name <var class="keyword varname">your_service_name</var></code></li>
<li>Esegui il bind della tua applicazione all'alias del servizio creato precedentemente:<br/>
<code>ibmcloud service bind <var class="keyword varname">your_ app_name</var> <var class="keyword varname">alias_name</var></code>.<br/>
<br/>
In alternativa, puoi aggiornare il tuo file manifest ed eseguire nuovamente il push dell'applicazione.</li>
<li>Verifica che la variabile di ambiente VCAP_SERVICES sia disponibile nel tuo runtime dell'applicazione:<br/>
<code>ibmcloud app env <var class="keyword varname">your_app_name</var></code></li>
<li>Passa queste credenziali alla tua applicazione. Specifica <code>token</code> come tuo nome utente e la <var class="keyword varname">api_key</var> come tua password. Separa <code>token</code> e la <var class="keyword varname">api_key</var> con un carattere due punti. Per ulteriori informazioni, vedi [Configurazione del tuo client](/docs/services/EventStreams?topic=eventstreams-kafka_connect). 
<p>Potresti dover ripreparare la tua applicazione per rendere effettive le modifiche.</p></li>
</ol>


## Cosa fare successivamente
{: #after_connecting}

Ora che disponi delle informazioni di connessione e credenziale, puoi scegliere un client {{site.data.keyword.messagehub}}. Per ulteriori informazioni, vedi [Utilizzo dell'API Kafka](/docs/services/EventStreams?topic=eventstreams-kafka_using).

<!--
Charlie said:

"Add some info describing how to take the information made available from above e.g. like the info in the Connecting a client to the Kafka API section of the alpha docs on stage 1? https://console.stage1.bluemix.net/docs/services/EventStreams/eventstreams122.html#alpha_about "
-->







 















