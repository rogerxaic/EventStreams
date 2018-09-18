---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-19"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Utilizzo di {{site.data.keyword.iamshort}} (IAM) con il programma Alpha
{: #alpha_iam }

Le autorizzazioni di{{site.data.keyword.messagehub}} vengono configurate utilizzando le politiche IAM. Una politica IAM è composta dalle seguenti informazioni: 

* un ID servizio 
* una risorsa {{site.data.keyword.Bluemix_short}} definita da un nome servizio, un'istanza del servizio, una regione, un tipo di risorsa IAM e una risorsa IAM
* un ruolo (lettore, scrittore o gestore)

Per ulteriori informazioni su IAM, consulta:
[IBM Cloud Identity and Access Management](/docs/iam/index.html#iamoverview).

Per un esempio su come configurare le politiche, vedi:
[Introducing IBM Cloud IAM Service IDs and API Keys ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/blogs/bluemix/2017/10/introducing-ibm-cloud-iam-service-ids-api-keys/){:new_window}.

## Risorse IAM {{site.data.keyword.messagehub}} 
{: iam_resources}

{{site.data.keyword.messagehub}} espone quattro tipi di risorse IAM:

* cluster
* argomento 
* gruppo
* txnid

Puoi identificare le risorse univoche per argomento, gruppo e txnid configurando la risorsa IAM. Il tipo di risorsa del cluster è singleton, quindi non deve essere identificato in modo univoco. 

## Scenari comuni 
{: iam_scenarios}

Ecco alcuni scenari {{site.data.keyword.messagehub}} comuni e l'accesso che devi assegnare: 

Produttore di alcuni argomenti (stesso per idempotente): 
* Ruolo di lettore sul tipo di risorsa del cluster 
* Ruolo di scrittore su ogni tipo di risorsa dell'argomento e di risorsa del nome dell'argomento.

Produttore transazionale: 
* Come il produttore
* Ruolo di scrittura sul tipo di risorsa txnid e sulla risorsa dell'ID della transazione 
* Ruolo di lettore sul tipo di risorsa del gruppo e sulla risorsa dell'ID del gruppo 

Consumatore (nessun gruppo)
* Ruolo di lettore sul tipo di risorsa del cluster  
* Ruolo di lettore su ogni tipo di risorsa dell'argomento e di risorsa del nome dell'argomento. 

Consumatore con gruppo:
* Come il consumatore (nessun gruppo) 
* Ruolo di lettore sul tipo di risorsa del gruppo e sulla risorsa dell'ID del gruppo 

Creare o eliminare l'argomento:
* Ruolo di lettore sul tipo di risorsa del cluster  
* Ruolo di gestore su ogni tipo di risorsa dell'argomento e di risorsa del nome dell'argomento. 

Eliminare il gruppo di consumatori:
* Ruolo di lettore sul tipo di risorsa del cluster  
* Ruolo di gestore sul tipo di risorsa del gruppo e sulla risorsa dell'ID del gruppo 

Elencare i gruppi, gli argomenti e gli offset e descrivere le configurazioni del broker, gruppo e argomento.
* Ruolo di lettore sul tipo di risorsa del cluster  














