---

copyright:
  years: 2015, 2019
lastupdated: "2018-12-07"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}



# Segnalazione di un problema al team di {{site.data.keyword.messagehub}} - piano Standard
{: #report_problem}

Se riscontri un problema con {{site.data.keyword.messagehub}}, per prima cosa controlla la [pagina sugli stati di {{site.data.keyword.Bluemix_notm}}  ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/status){:new_window}. 

Se vuoi aiuto da parte del team di {{site.data.keyword.messagehub}}, raccogli tutte le seguenti informazioni. Più informazioni puoi fornire in anticipo, e più efficientemente il team può aiutarti con il problema:
{:shortdesc}

1. In quale ubicazione (regione) {{site.data.keyword.Bluemix_notm}} è stato eseguito il provisioning della tua istanza di {{site.data.keyword.messagehub}}?  Ad esempio, Dallas o Londra. 
2. In quale interfaccia riscontri i problemi? Kafka, REST, AMQP o bridge?
3. Quando si è verificato il primo errore (nello specifico ora, data e fuso orario)? Per quanto tempo è rimasta in esecuzione l'applicazione prima dell'errore?
4. Si sta verificando ancora l'errore? Puoi replicarlo?
5. Qual è l'ID istanza del servizio {{site.data.keyword.messagehub}} che stai utilizzando? 
Puoi trovare questo ID nel campo "instance_id" nel JSON VCAP_SERVICES del servizio. Ad esempio:
 ```
 "instance_id": "e662a61b-d1ff-4cce-aab9-5eae9adb9827"
 ```
6. Quale client Kafka o REST utilizza la tua applicazione? Quali sono i dettagli della versione?
7. Quali sono i dettagli della configurazione del client?
8. Hai dei frammenti di log dell'applicazione che visualizzano il problema?
9. Qual è il problema che vedi? Quali sono gli argomenti, ID client e ID gruppo interessati?
10. Qual è l'impatto del problema sul tuo servizio?


Puoi fornire le informazioni che hai raccolto da IBM in un ticket di supporto [inviando una richiesta di supporto ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/get-support/howtogetsupport.html#using-avatar).










