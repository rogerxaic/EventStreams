---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-03"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Che cos'è {{site.data.keyword.messagehub}}?
{: #about}

{{site.data.keyword.messagehub_full}} è un bus di messaggi ad elevata velocità effettiva creato con Apache Kafka. È un Apache Kafka completamente gestito come un servizio, ottimizzato per l'inserimento di eventi in {{site.data.keyword.Bluemix_notm}} e la distribuzione del flusso di eventi tra i tuoi servizi e le tue applicazioni. {{site.data.keyword.messagehub}} era precedentemente conosciuto come Message Hub.
{: shortdesc}

Puoi utilizzare {{site.data.keyword.messagehub}} per completare
le seguenti attività:

* Scaricare il lavoro nelle applicazioni di lavoro di back-end.
* Connettere i flussi di eventi alle analisi del flusso per realizzare delle potenti analisi approfondite.
* Pubblicare dati di evento in più applicazioni per delle reazioni in tempo reale.
* Trasferire dati in un altro servizio. Ad esempio, a Cloud Object Storage.

Essendo creato con Apache Kafka, si avvale direttamente di tutta l'innovazione che ha luogo nella community e supporta le API client Kafka, Kafka Streams, Kafka Connect e anche KSQL.

Gli strumenti Apache Kafka normalmente lavorano direttamente con {{site.data.keyword.messagehub}}, anche se devi fornire ulteriori informazioni perché
le connessioni a {{site.data.keyword.messagehub}} eseguano sempre l'autenticazione utilizzando le credenziali.

In {{site.data.keyword.messagehub}}, le applicazioni inviano i dati
creando un messaggio e inviandolo a un argomento. Per ricevere i messaggi, le applicazioni si sottoscrivono a un argomento
e scelgono di ricevere tutti i messaggi dell'argomento o di condividere i messaggi tra loro.
{{site.data.keyword.messagehub}} ospita e mantiene i messaggi
in una sequenza ordinata. 




