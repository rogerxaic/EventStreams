---

copyright:
  years: 2015, 2019
lastupdated: "2018-12-17"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Segnalazione di un problema al team di {{site.data.keyword.messagehub}} - piano Enterprise
{: #report_problem_enterprise}

Se riscontri un problema con {{site.data.keyword.messagehub}}, per prima cosa controlla la [pagina sugli stati di {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/status?selected=status){:new_window}.
{: shortdesc}

Se vuoi aiuto da parte del team di {{site.data.keyword.messagehub}}, raccogli tutte le seguenti informazioni. Più informazioni puoi fornire in anticipo, e più efficientemente il team può aiutarti con il problema:
{:shortdesc}

1. Qual è l'ID CRN del servizio {{site.data.keyword.messagehub}} che stai
   utilizzando?  Puoi fornire questo ID incollando l'URL della console
   {{site.data.keyword.Bluemix_notm}} completo dopo aver fatto clic sul servizio
   o incollando l'output dal seguente comando CLI:<br/>
   <pre class="pre">
   ibmcloud resource service-instance NAME
   </pre>
	{: codeblock}
2. Quando si è verificato il primo errore (nello specifico ora, data e fuso orario)?
   Per quanto tempo è rimasta in esecuzione l'applicazione prima dell'errore?
3. Si sta verificando ancora l'errore? Puoi replicarlo?
4. Quale client Kafka sta utilizzando la tua applicazione? Quali sono i dettagli della versione?
5. Quali sono i dettagli della configurazione del client? Abbiamo bisogno di conoscere le impostazioni dei tuoi produttore e consumatore, per cui elenca tutte le opzioni non predefinite che hai passato durante la creazione dei tuoi produttore e consumatore.
6. Hai dei frammenti di log dell'applicazione che visualizzano il problema?
7. Qual è il problema che vedi? Quali argomenti, ID client, ID gruppo e ID
   transazione sono interessati?
8. Qual è l'impatto del problema sul tuo servizio?

Puoi fornire le informazioni che hai raccolto da IBM in un ticket di supporto [inviando una richiesta di supporto ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){:new_window}.










