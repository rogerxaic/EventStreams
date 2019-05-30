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

# Utilizzo dell'API del produttore REST
{: #rest_producer_using}


** L'API del produttore REST è disponibile solo come parte del nuovo piano Standard di {{site.data.keyword.messagehub}}.**
<br/>

{{site.data.keyword.messagehub}} fornisce un'API REST per aiutarti a connettere i tuoi sistemi esistenti al tuo cluster Kafka {{site.data.keyword.messagehub}}. Utilizzando l'API, puoi integrare {{site.data.keyword.messagehub}} con tutti i sistemi che supportano le API RESTful.

L'API del produttore REST è un'interfaccia REST scalabile per la produzione di messaggi in {{site.data.keyword.messagehub}} su un endpoint HTTP sicuro. Invia i dati dell'evento a {{site.data.keyword.messagehub}}, utilizza la tecnologia Kafka per gestire i feed di dati e avvalerti delle funzioni {{site.data.keyword.messagehub}} per gestire i tuoi dati.

Utilizza l'API per connettere i sistemi esistenti a {{site.data.keyword.messagehub}}. Crea le richieste di produzione dai tuoi sistemi in {{site.data.keyword.messagehub}}, specificando anche la chiave del messaggio, le intestazioni e gli argomenti che vuoi scrivere nel messaggio.


## Produzione dei messaggi utilizzando REST
{: #rest_produce_messages}

Utilizza l'API del produttore per scrivere i messaggi sugli argomenti. Per poter produrre un argomento, devi disporre delle seguenti informazioni:

* L'URL dell'endpoint API {{site.data.keyword.messagehub}}, incluso il numero di porta.
* L'argomento che vuoi produrre.
* La chiave API che fornisce l'autorizzazione per la connessione e la produzione dell'argomento selezionato.

Devi richiamare i dettagli dell'URL e delle credenziali necessari per la connessione all'API da un oggetto di credenziali del servizio o da una chiave del servizio per l'istanza del servizio. Per informazioni sulla creazione di questi oggetti, vedi
[Connessione a {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting).

L'URL per l'endpoint dell'API viene fornito nella proprietà <code>kafka_http_url</code>.

Utilizza uno dei seguenti metodi per l'autenticazione:

* **Per l'autenticazione utilizzando l'autenticazione di base:**<br/>
    Utilizza le proprietà <code>user</code> e <code>api_key</code> dei precedenti oggetti come nome utente e password. Inserisci questi valori nell'intestazione <code>Authorization</code> della richiesta HTTP nel formato <code>Basic <base64 encoding of username:password joined by a single colon (:)></code>.

* **Per l'autenticazione utilizzando un token di connessione:**<br/>
    Per ottenere il tuo token utilizzando la CLI IBM Cloud, per prima cosa accedi a IBM Cloud e poi immetti il seguente comando: 

    ```
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

    Inserisci questo token nell'intestazione Authorization della richiesta HTTP nel formato <code>Bearer <token></code>. Sono supportati sia la chiave API che i token JWT. 

* **Per l'autenticazione utilizzando direttamente la chiave API:**<br/>
    Inserisci la chiave direttamente come il valore dell'intestazione HTTP <code>X-Auth-Token</code>.

<br/>
Il seguente codice mostra un esempio di invio di un messaggio utilizzando curl:

```
curl -v -X POST -H "Authorization: Basic <base64 username:password>" -H "Content-Type: text/plain" -H "Accept: application/json" -d 'test message' "<kafka_http_url>/topics/<topic_name>/records"
```
{: codeblock}


## Riferimento API
{: #rest_api_reference}

Per tutti i dettagli dell'API, vedi il
[Riferimento dell'API del produttore REST {{site.data.keyword.messagehub}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://ibm.github.io/event-streams/api/){:new_window}.












