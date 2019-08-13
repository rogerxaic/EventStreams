---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-24"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka, service endpoints

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Gestione degli endpoint del servizio utilizzando il piano Enterprise
{: #manage_endpoints}

Utilizzando un endpoint interno o privato, l'endpoint del servizio {{site.data.keyword.Bluemix_short}} abilita la connessione al servizio {{site.data.keyword.messagehub}} tramite la rete IBM Cloud interna. Questa funzionalità significa che tutti i dati che pubblichi o utilizzi dal servizio {{site.data.keyword.messagehub}}
sono sulla rete privata e non sulle interfacce pubbliche.
{:shortdesc}

Puoi ora aggiungere un endpoint privato per accedere e gestire la tua istanza del servizio {{site.data.keyword.messagehub}}.

## Prerequisiti 
{: #prereqs_endpoint}

Assicurati di soddisfare i seguenti requisiti:
- Crea la tua istanza del servizio tramite il piano Enterprise. Per ulteriori informazioni, vedi [Scelta del tuo piano](/docs/services/EventStreams?topic=eventstreams-plan_choose).
- Crea la tua istanza del servizio nelle regioni {{site.data.keyword.Bluemix_notm}} Dallas (us-south) o Francoforte (eu-de).
- Abilita il [VRF (Virtual Route Forwarding)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) per il tuo account {{site.data.keyword.Bluemix_notm}}.

## Aggiunta di un endpoint privato
{: #add_endpoint}

Per aggiungere un endpoint privato:

* Apri un [ticket ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){:new_window} per richiedere un endpoint privato. Fornisci le seguenti informazioni nel ticket:

    * Il tuo ID cluster, se lo conosci.

    Se non conosci l'ID cluster, fornisci invece il tuo URL del dashboard, gli endpoint del broker Kafka o il tuo ID dell'istanza del servizio.
  

Per informazioni sugli endpoint del servizio, vedi la [Documentazione dell'endpoint del servizio {{site.data.keyword.Bluemix_notm}}](/docs/resources?topic=resources-service-endpoints#about){:new_window}.


## Dopo il passaggio a un endpoint privato
{: #after_endpoint}

Dopo aver eseguito il passaggio a un endpoint privato, gli endpoint esterni o pubblici non sono più disponibili.


### Accesso alla console IBM {{site.data.keyword.messagehub}} 

Quando un cluster ha degli endpoint privati abilitati, l'URL di gestione che utilizzi per accedere alla console {{site.data.keyword.messagehub}} viene modificato.

La console {{site.data.keyword.messagehub}} è raggiungibile solo da un URL di gestione privato. Per scoprire i tuoi endpoint privati, incluso l'URL di gestione privato, puoi creare una nuova credenziale del servizio.

<!--
1. On the service details page, click **Manage endpoints**. You can see the external endpoint assigned to your service instance.
2. Click  **Add internal endpoint**. An internal endpoint is assigned to your service instance.
3. **Optional.** Use the endpoint toggle to enable or disable endpoints as needed.
-->

