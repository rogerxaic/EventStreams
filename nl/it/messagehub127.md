---

copyright:
  years: 2015, 2018
lastupdated: "2018-07-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Connessione a Message Hub
{: #connect_messagehub}

Il modo in cui stabilisci una connessione a {{site.data.keyword.messagehub}} varia in base al fatto che tu stia utilizzando il piano Standard oppure quello Enterprise e anche dal fatto che tu stia stabilendo la connessione da un'applicazione Cloud Foundry oppure da qualsiasi altro client esterno. Devi raccogliere due informazioni per stabilire una connessione a una qualsiasi delle API di {{site.data.keyword.messagehub}}:

* Gli URL endpoint per le API
* Le credenziali per l'autenticazione

Leggi le seguenti informazioni su come ottenere questi dettagli. I passi possono variare leggermente per garantire che tu completi la procedura appropriata per la tua istanza.

## Esegui il provisioning di un'istanza {{site.data.keyword.messagehub}}

Come prerequisito, devi prima eseguire il provisioning di un'istanza del servizio {{site.data.keyword.messagehub}} per il piano Standard o quello Enterprise. Il provisioning di un'istanza {{site.data.keyword.messagehub}} potrebbe comportare un addebito. Ottieni quindi i dettagli della connessione API {{site.data.keyword.messagehub}} completando le seguenti attività:

## Panoramica del piano Standard
{: #connect_standard}

I servizi di cui viene eseguito il provisioning utilizzando il piano Standard sono servizi Cloud Foundry. Questo significa che vengono distribuiti in un'organizzazione e uno spazio Cloud Foundry e vengono raggruppati nel dashboard sotto l'intestazione **Servizi Cloud Foundry**. Il metodo da te utilizzato per connettere un'applicazione dipende da dove viene distribuita l'applicazione, ossia in Cloud Foundry o al suo esterno.


## Applicazioni Cloud Foundry nel piano Standard
{: #connect_standard_cf}

Per le applicazioni in esecuzione all'interno di Cloud Foundry, esegui il bind della tua applicazione all'istanza del servizio {{site.data.keyword.messagehub}}. Una volta eseguito il bind, i dettagli della connessione vengono resi disponibili all'applicazione in formato JSON nella variabile di ambiente VCAP_SERVICES. Puoi eseguire il bind di un'applicazione e di un servizio utilizzando la console IBM Cloud oppure la CLI IBM Cloud.

Ecco un esempio di VCAP_SERVICES:

```
{
  "credentials": {
    "mqlight_lookup_url": "https://mqlight-lookup.messagehub.services.us-south.bluemix.net/Lookup?serviceId=584e8436-e7f5-43db-96ac-2864fccae5ae",
    "api_key": "d9JSx1SYsmLzNRbbgUFneDm2DtkedlVeViObYJIvrPAf2kJA",
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
{: #connect_standard_cf_console }

1. Assicurati di essere nell'organizzazione e nello spazio Cloud Foundry previsti.
2. Individua la tua applicazione Cloud Foundry nel dashboard. Se non hai ancora un'applicazione Cloud Foundry, puoi crearne una facendo clic sul pulsante **Crea risorsa**,
3. Fai clic sul tuo tile dell'applicazione.
4. Fai clic su **Connessioni**.
5. Fai clic su **Crea connessione**.
6. Seleziona il tile del servizio {{site.data.keyword.messagehub}} a cui vuoi eseguire il bind e fai clic su **Connetti**. Potresti dover ripreparare la tua applicazione per rendere effettive le modifiche.
7. Fai clic sulla scheda **Runtime** a sinistra e seleziona la scheda **Variabili di ambiente** al centro. Puoi ora verificare le tue informazioni VCAP_SERVICES e la tua applicazione può ora accedevi come variabili di ambiente. 


### Ottieni le credenziali utilizzando la CLI IBM Cloud 
{: #connect_standard_cf_cli }

<ol>
<li>Assicurati di essere nell'organizzazione e nello spazio Cloud Foundry previsti.Puoi spostarti in modo interattivo eseguendo questo comando:<br/>
<code>ibmcloud target -cf</code>
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
<code>ibmcloud service bind <var class="keyword varname">tuo_nome_applicazione</var> <var class="keyword varname">tuo_nome_servizio</var></code>
</li>
<li>Verifica che la variabile di ambiente VCAP_SERVICES sia disponibile nel runtime dell'applicazione eseguendo:</br> 
 <code>ibmcloud app env <var class="keyword varname">tuo_nome_applicazione</var></code>.
</li>
<li>Passa queste credenziali alla tua applicazione.Potresti dover ripreparare la tua applicazione per rendere effettive le modifiche.</li>
</ol>

## Applicazioni esterne sul piano Standard
{: #connect_standard_external}

Per le applicazioni in esecuzione esternamente a Cloud Foundry, le credenziali vengono generate creando una chiave di servizio. Dopo che hai ottenuto una chiave di servizio, passa manualmente i dettagli della chiave alla tua applicazione utilizzando il metodo da te scelto.

### Ottieni le credenziali utilizzando la console IBM Cloud
{: #connect_standard_external_console}

1. Assicurati di essere nell'organizzazione e nello spazio Cloud Foundry previsti.
2. Individua il tuo servizio {{site.data.keyword.messagehub}} Cloud Foundry nel dashboard.
3. Fai clic sul tuo tile del servizio.
4. Fare clic su **Credenziali del servizio**.
5. Fai clic su  ** Nuova credenziale**.
6. Immetti i dettagli per la tua nuova credenziale, come ad esempio un nome, e fai clic su **Aggiungi**. Viene visualizzata una nuova credenziale nell'elenco delle credenziali.
7. Fai clic su questa credenziale utilizzando **Visualizza credenziali** per rivelare i dettagli in formato JSON.
8. Passa queste credenziali alla tua applicazione.

### Ottieni le credenziali utilizzando la CLI IBM Cloud 
{: #connect_standard_external_cli }

<ol>
<li>Assicurati di essere nell'organizzazione e nello spazio Cloud Foundry previsti.Puoi spostarti in modo interattivo eseguendo questo comando:<br>
<code>ibmcloud target -cf</code>
</li>
<li>Trova il tuo servizio:<br>
<code>ibmcloud service list</code>
</li>
<li>Puoi creare una chiave di servizio:<br>
<code>ibmcloud service key-create <var class="keyword varname">tuo_nome_servizio</var> <var class="keyword varname">nome_della_nuova_chiave_di_servizio</var></code><br>
<br/>
oppure utilizzare una chiave di servizio esistente:<br/>
<code>ibmcloud service keys <var class="keyword varname">tuo_nome_servizio</var></code>
</li>
<li>Ottieni i dettagli per la chiave:</br>
<code>ibmcloud service key-show <var class="keyword varname">tuo_nome_servizio</var> <var class="keyword varname">nome_chiave_servizio</var></code></br>
Questo restituisce i dettagli della chiave di servizio in formato JSON.</li>
<li>Passa queste credenziali alla tua applicazione. </li>
</ol>

 
## Panoramica del piano Enterprise
{: #connect_enterprise}

I servizi di cui è stato eseguito il provisioning utilizzando il piano Enterprise sono raggruppati nel dashboard sotto l'intestazione **Servizi**. Il piano Enterprise è [abilitato a IAM ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/docs/iam/quickstart.html#getstarted){:new_window}. Non devi necessariamente comprendere IAM per iniziare a lavorare ma una certa dimestichezza è consigliata, se vuoi proteggere il tuo servizio {{site.data.keyword.messagehub}}. Per ulteriori informazioni, vedi [Protezione delle tue risorse {{site.data.keyword.messagehub}}](/docs/services/MessageHub/messagehub124.html)

Completa la seguente procedura per eseguire il bind della tua applicazione e ottenere le chiavi di servizio per il tuo servizio. Per disporre dell'autorizzazione a creare gli argomenti, la tua applicazione o la chiave di servizio devono disporre di un ruolo di accesso di Gestore.

Per connettere un'applicazione, il metodo utilizzato dipende da dove viene distribuita l'applicazione, ossia in Cloud Foundry o al suo esterno.

## Applicazioni Cloud Foundry nel piano Enterprise
{: #connect_enterprise_cf}

È necessario che della tua applicazione sia stato eseguito il bind all'istanza del servizio {{site.data.keyword.messagehub}}. Per eseguire il bind di un'applicazione Cloud Foundry a un servizio non Cloud Foundry con One Cloud, crea prima un alias del servizio Cloud Foundry e fai quindi riferimento a questo alias dalla tua applicazione Cloud Foundry quando esegui il bind.

Una volta eseguito il bind, i dettagli della connessione vengono resi disponibili alla tua applicazione in formato JSON utilizzando la variabile di ambiente VCAP_SERVICES. Puoi eseguire il bind di un'applicazione e di un servizio utilizzando la console IBM Cloud oppure la CLI IBM Cloud.

### Esegui il bind di un'applicazione utilizzando la console IBM Cloud
{: #connect_enterprise_cf_console}

1. Assicurati di essere nell'organizzazione e nello spazio Cloud Foundry previsti.
2. Individua la tua applicazione Cloud Foundry sul dashboard oppure crea un'applicazione facendo clic sul pulsante **Crea risorsa**.
3. Fai clic sul tuo tile dell'applicazione.
4. Fai clic su **Connessioni**.
5. Fai clic su **Crea connessione**.
6. Seleziona il tile del servizio {{site.data.keyword.messagehub}} a cui vuoi eseguire il bind e fai clic su **Connetti**. Questo crea automaticamente un alias del servizio Cloud Foundry per il tuo servizio {{site.data.keyword.messagehub}} ed esegue quindi il bind della tua applicazione a questo alias. Potresti dover ripreparare la tua applicazione per rendere effettive le modifiche.
8. Fai clic sulla scheda **Runtime** a sinistra e seleziona la scheda **Variabili di ambiente** al centro. Puoi ora verificare le tue informazioni VCAP_SERVICES . La tua applicazione vi può ora accedere come variabili di ambiente. 
 

### Esegui il bind di un'applicazione utilizzando la CLI IBM Cloud
{: #connect_enterprise_cf_cli}

<ol>
<li>Assicurati di essere nell'organizzazione e nello spazio Cloud Foundry previsti.Puoi spostarti in modo interattivo eseguendo questo comando:<br/>
 <code>ibmcloud target -cf</code></li>
<li>Individua la tua applicazione:</br>
<code>ibmcloud app list</code><br/>
<br/>
Se hai un file manifest, puoi creare una nuova applicazione eseguendo:<br/>
<code>ibmcloud app push</code><br/>
<br/>
Poiché della tua applicazione non è stato ancora eseguito il bind a {{site.data.keyword.messagehub}}, l'applicazione non può stabilire una connessione. Pertanto, ti consigliamo di eseguire il push dell'applicazione con il parametro <code>--no-start</code> per evitare errori di connessione non necessari.</li>
<li>Individua il tuo servizio:</br>
<code>ibmcloud resource service-instances</code></li>
<li>Crea un alias del servizio Cloud Foundry:<br/>
<code>ibmcloud resource service-alias-create <var class="keyword varname">nome_alias</var> --instance-name <var class="keyword varname">tuo_nome_servizio</var></code></li>
<li>Esegui il bind della tua applicazione all'alias del servizio creato precedentemente:<br/>
<code>ibmcloud service bind <var class="keyword varname">tuo_nome_applicazione</var> <var class="keyword varname">nome_alias</var></code>.<br/>
<br/>
In alternativa, puoi aggiornare il tuo file manifest ed eseguire nuovamente il push dell'applicazione.</li>
<li>Verifica che variabile di ambiente VCAP_SERVICES sia disponibile nel tuo runtime dell'applicazione:<br/>
<code>ibmcloud app env <var class="keyword varname">tuo_nome_applicazione</var></code></li>
<li>Passa queste credenziali alla tua applicazione.<br/> Potresti dover ripreparare la tua applicazione per rendere effettive le modifiche.</li>
</ol>


## Applicazioni esterne nel piano Enterprise
{: #connect_enterprise_external}

Per le applicazioni in esecuzione esternamente a Cloud Foundry, le credenziali vengono generate creando una chiave di servizio. Dopo che hai ottenuto una chiave di servizio, passa manualmente i dettagli della chiave alla tua applicazione utilizzando il metodo da te scelto.

### Ottieni le credenziali utilizzando la console IBM Cloud
{: #connect_enterprise_external_console}

1. Individua il tuo servizio {{site.data.keyword.messagehub}} nel dashboard.
2. Fai clic sul tuo tile del servizio.
3. Fare clic su **Credenziali del servizio**.
4. Fai clic su  ** Nuova credenziale**. 
5. Completa i dettagli per la tua nuova credenziale, come ad esempio un nome e un ruolo, e fai clic su **Aggiungi**. Viene visualizzata una nuova credenziale nell'elenco delle credenziali.
6. Fai clic su questa credenziale utilizzando **Visualizza credenziali** per rivelare i dettagli in formato JSON.
7. Passa queste credenziali alla tua applicazione. Assicurati che la tua applicazione analizzi i dettagli.

### Ottieni le credenziali utilizzando la CLI IBM Cloud
{: #connect_enterprise_external_cli}

<ol>
<li>Individua il tuo servizio:<br/>
<code>ibmcloud resource service-instances</code></li>
<li>Crea una chiave di servizio:<br/>
<code>ibmcloud resource service-key-create <var class="keyword varname">nome_chiave</var> <var class="keyword varname">ruolo_chiave</var> --instance-name <var class="keyword varname">tuo_nome_servizio</var></code></li>
<li>Stampa la chiave di servizio:<br/>
<code>ibmcloud resource service-key <var class="keyword varname">nome_chiave</var></code></li>
<li>Passa queste credenziali alla tua applicazione. Assicurati che la tua applicazione analizzi i dettagli.</li>
</ol>

## Cosa fare successivamente
{: #after_connecting}

Ora che disponi delle informazioni di connessione e credenziale, puoi scegliere un client {{site.data.keyword.messagehub}}. La tua scelta dipende dal tuo piano.

* Se stai utilizzando il piano Standard, vedi [Scegli tra le tre API](/docs/services/MessageHub/messagehub087.html) per informazioni su quale client scegliere e come stabilire la connessione.
* Se stai utilizzando il piano Enterprise, vedi [Utilizzo dell'API Kafka](/docs/services/MessageHub/messagehub050.html).

	L'argomento <code>__consumer_offsets</code> Kafka interno è visibile per te in sola lettura
	se stai utilizzando il piano Enterprise. Ti consigliamo vivamente di non provare a gestire l'argomento in alcun modo. 

<!--
Charlie said:

"Add some info describing how to take the information made available from above e.g. like the info in the Connecting a client to the Kafka API section of the alpha docs on stage 1? https://console.stage1.bluemix.net/docs/services/MessageHub/messagehub122.html#alpha_about "
-->







 















