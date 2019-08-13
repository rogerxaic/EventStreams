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

# Utilizzo dell'API REST di amministrazione
{: #admin_api}

{{site.data.keyword.messagehub}} fornisce un'API RESTful di amministrazione che puoi usare per creare, eliminare, elencare ed aggiornare gli argomenti.
{: shortdesc}

Devi richiamare i dettagli dell'URL e delle credenziali necessari per la connessione all'API da un oggetto di credenziali del servizio o da una chiave del servizio per l'istanza del servizio. Per informazioni sulla creazione di questi oggetti, vedi
[Connessione a {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting).

L'URL per l'endpoint dell'API viene fornito nella proprietà <code>kafka_admin_url</code>.

Le credenziali dipendono dal metodo di autenticazione e sono supportati tre tipi di credenziali:

* **Per l'autenticazione utilizzando l'autenticazione di base**:<br/>
    Utilizza le proprietà <code>user</code> e <code>api_key</code> dei precedenti oggetti come nome utente e password. Inserisci questi valori nell'intestazione <code>Authorization</code> della richiesta HTTP nel formato <code>Basic &lt;base64 encoding of username and password joined by a single colon (:)&gt;</code>.

* **Per l'autenticazione utilizzando un token di connessione:**<br/>
    Per ottenere il tuo token utilizzando la CLI IBM Cloud, per prima cosa accedi a IBM Cloud e poi immetti il seguente comando: 

    ```
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

    Inserisci questo token nell'intestazione Authorization della richiesta HTTP nel formato <code>Bearer <token></code>. Sono supportati sia la chiave API che i token JWT. 

* **Per l'autenticazione utilizzando direttamente la chiave API:**<br/>
    Inserisci la chiave direttamente come il valore dell'intestazione HTTP <code>X-Auth-Token</code>.

Per le istanze del servizio create sul piano Classic, queste informazioni sono invece disponibili dalla variabile di ambiente VCAP_SERVICES della tua applicazione.

Per una descrizione dell'API con degli esempi, vedi
[{{site.data.keyword.messagehub}} admin-rest ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-messaging/event-streams-docs/tree/master/admin-rest-api){:new_window}.

Puoi scaricare la specifica completa per l'API da [{{site.data.keyword.messagehub}} Admin REST API YAML file ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-messaging/event-streams-docs/blob/master/admin-rest-api/admin-rest-api.yaml){:new_window}.
Per visualizzare il file swagger, utilizzare gli strumenti Swagger, ad esempio l'[editor Swagger ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://editor.swagger.io/#/){:new_window}.




