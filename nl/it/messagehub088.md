---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-01"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Collegamento ad altri servizi utilizzando i bridge
{: #bridges}

I bridge ** {{site.data.keyword.messagehub}} sono disponibili solo come parte del piano Standard.**
<br/>

Il piano {{site.data.keyword.messagehub}} Standard supporta anche i bridge a una selezione di altri sistemi. I bridge sono dei link unidirezionali tra {{site.data.keyword.messagehub}} e un altro servizio. I bridge consentono
di leggere i dati da {{site.data.keyword.messagehub}} e di scriverli in
un altro servizio o di leggere i dati da un servizio e di scriverli in {{site.data.keyword.messagehub}}. Un bridge può prendere i messaggi dall'altro sistema e pubblicarli su un argomento o consumare
i messaggi da un argomento e inviarli all'altro sistema. In questo modo, puoi utilizzare {{site.data.keyword.messagehub}} per l'integrazione con altri sistemi senza dover scrivere il codice.
{:shortdesc}

I vantaggi fondamentali dell'utilizzo dei bridge {{site.data.keyword.messagehub}} sono i seguenti:  

* Puoi definire i bridge in modo amministrativo, pertanto non hai bisogno di scrivere il codice applicativo.
* Il ciclo di vita di ogni bridge è monitorato e gestito dal servizio {{site.data.keyword.messagehub}}. Ad esempio, se un bridge rileva un errore, viene riavviato automaticamente da {{site.data.keyword.messagehub}}.
* I bridge sono integrati con la piattaforma {{site.data.keyword.Bluemix_short}}. Ad esempio, le informazioni di registrazione e di monitoraggio generate dai bridge vengono indirizzate ai dashboard Kibana e Grafana.

Potresti trovare i bridge particolarmente utili nei seguenti due scenari comuni:

* Consumo di dati da una o più origini dati in {{site.data.keyword.messagehub}}.
* Trasferimento di dati da {{site.data.keyword.messagehub}} in un altro servizio. Ad esempio, nell'archiviazione di lungo termine.

## Bridge {{site.data.keyword.messagehub}}
{: notoc}

* Forniamo i seguenti tipi di bridge: 
  - [Bridge MQ](/docs/services/MessageHub/messagehub105.html){:new_window}, che prende i dati dei messaggi da {{site.data.keyword.IBM}} MQ e li trasferisce in un argomento in {{site.data.keyword.messagehub}}. Abbiamo intenzione di supportare una gamma di bridge più ampia in futuro.
  - [Bridge Cloud Object Storage](/docs/services/MessageHub/messagehub115.html){:new_window}, che trasferisce i dati {{site.data.keyword.messagehub}} a un'istanza del servizio [{{site.data.keyword.IBM_notm}} Cloud Object Storage ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/services/cloud-object-storage/about-cos.html){:new_window}. 
    
    Il servizio Cloud Object Storage è il servizio di archivio oggetti preferito in {{site.data.keyword.Bluemix_short}}. 
  - [Bridge {{site.data.keyword.objectstorageshort}}](/docs/services/MessageHub/messagehub089.html){:new_window}, che trasferisce i dati {{site.data.keyword.messagehub}} a un'istanza del servizio [Object Storage ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/services/ObjectStorage/index.html){:new_window}.
* Attualmente, i bridge sono disponibili in tutti gli ambienti {{site.data.keyword.Bluemix_notm}} pubblici. I bridge non sono disponibili in {{site.data.keyword.Bluemix_short}} dedicato.
* Puoi amministrare i bridge nei seguenti due modi:
  - Utilizzando un'[API REST ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-messaging/message-hub-docs){:new_window}, che è un'estensione dell'attuale API di amministrazione {{site.data.keyword.messagehub}}. Puoi anche trovare esempi su come utilizzare curl per gestire il ciclo di vita dei bridge all'indirizzo [message-hub-docs ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-messaging/message-hub-docs){:new_window}. Potremmo cambiare questa API REST mentre continuiamo a sviluppare i bridge. Intendiamo stabilizzare questa API.
  - Utilizzando il dashboard {{site.data.keyword.messagehub}} nella console {{site.data.keyword.Bluemix_notm}}.
* Puoi associare un massimo di due bridge di qualsiasi tipo a un'istanza del servizio {{site.data.keyword.messagehub}}. Continueremo a riesaminare questa limitazione mentre continuiamo a sviluppare i bridge.
* Non è previsto alcun costo aggiuntivo per l'utilizzo dei bridge oltre le loro operazioni di messaggistica.
* Il bridge MQ non supporta l'utilizzo di SSL/TLS per proteggere la riservatezza e l'integrità dei dati mentre vengono trasferiti tra il bridge e il gestore code MQ. Abbiamo intenzione di aggiungere il supporto per l'utilizzo di SSL / TLS al bridge. 
* Tuttavia, puoi utilizzare il servizio [{{site.data.keyword.SecureGatewayfull}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/services/SecureGateway/secure_gateway.html){:new_window} per inviare i tuoi dati
attraverso un tunnel protetto tra {{site.data.keyword.Bluemix_notm}}
e un client {{site.data.keyword.SecureGateway}} che puoi installare
in loco. In questa configurazione, la comunicazione su entrambe le estremità del tunnel non è
protetta con SSL/TLS.
* I bridge Cloud Object Storage e {{site.data.keyword.objectstorageshort}} concatenano i messaggi utilizzando caratteri di nuova riga come separatori mentre scrivono i dati rispettivamente in Cloud Object Storage o {{site.data.keyword.objectstorageshort}}. Ciò rende questi bridge inadatti per i messaggi che contengono caratteri di nuova riga incorporati e per i dati dei messaggi binari.
* Le convenzioni di denominazione degli oggetti attualmente utilizzate dai bridge Cloud Object Storage e {{site.data.keyword.objectstorageshort}} potrebbero cambiare in futuro.

## Bridge da altri servizi in {{site.data.keyword.messagehub}}
{: notoc}

* {{site.data.keyword.iot_full}} fornisce un suo bridge [ in {{site.data.keyword.messagehub}}](/docs/services/MessageHub/messagehub119.html){:new_window}. Il bridge fornisce un collegamento unidirezionale in {{site.data.keyword.messagehub}} che ti consente di archiviare i dati cronologici. Connettendo {{site.data.keyword.messagehub}} a {{site.data.keyword.iot_short_notm}}, puoi utilizzare Message Hub come una pipeline di eventi per consumare i tuoi eventi dispositivo da Watson IoT Platform e rendere gli eventi disponibili in tempo reale al resto della piattaforma. 


