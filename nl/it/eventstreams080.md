---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Come connettere applicazioni MQ Light esistenti
{: #mql_exist_apps}

** L'API MQ Light è disponibile solo come parte del piano Standard.**
<br/>

Puoi connettere al servizio delle applicazioni esistenti attualmente in esecuzione in {{site.data.keyword.IBM_notm}} MQ o nel servizio {{site.data.keyword.mql}}
{{site.data.keyword.Bluemix_notm}}. Le applicazioni continueranno a funzionare allo stesso modo.
{:shortdesc}

Per connettere applicazioni esistenti, completa i seguenti controlli:

* Assicurati che l'applicazione utilizzi l'ultima versione disponibile del client API {{site.data.keyword.mql}} per il tuo linguaggio.
* Controlla che i dettagli di connessione estratti da VCAP_SERVICES si riferiscano al tipo di servizio <code>messagehub</code> e che richiamino il nome utente di connessione dalla proprietà <code>credentials.user</code> anziché dalla proprietà <code>credentials.username</code> e richiamino l'URL di ricerca della connessione dalla proprietà <code>credentials.mqlight_lookup_url</code> anziché dalla proprietà <code>credentials.connectionLookupURI</code>. Per ulteriori informazioni, vedi [Variabile di ambiente VCAP_SERVICES](/docs/services/EventStreams/eventstreams127.html).

	Nota che questo passaggio viene completato automaticamente se utilizzi il client Java&trade; e specifichi 'null' come parametro endpointService nella chiamata create(), pertanto è il client stesso a recuperare le informazioni.
	
* La tua applicazione deve supportare le connessioni TLS v1.2. VCAP_SERVICES non contiene più una proprietà <code>credentials.nonTLSConnectionLookupURI</code> per la creazione di una connessione diversa da TLS.

Nota anche le seguenti informazioni:

* I limiti dei messaggi sono coerenti con {{site.data.keyword.messagehub}} ma potrebbero essere diversi da altri server che supportano l'API {{site.data.keyword.mql}}. Per ulteriori informazioni, vedi [Limiti massimi](/docs/services/EventStreams/eventstreams083.html).
* JMS non è supportato.
