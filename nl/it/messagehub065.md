---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-30"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Perché utilizzare l'API REST Kafka?
{: #why_rest}

** L'API Kafka REST è disponibile solo come parte del piano Standard.**
<br/>

L'API REST Kafka è una pratica interfaccia che può essere utilizzata nelle
            seguenti situazioni:

* In alcuni casi d'uso a bassa velocità effettiva dove la latenza non è un fattore critico
* Per eseguire il debug e la ricerca di malfunzionamenti

L'API REST Kafka non è stata concepita per essere un'interfaccia ad elevata velocità effettiva e a bassa latenza. Per questi tipi di requisiti, si consiglia di utilizzare l'API Kafka per la connessione da e verso {{site.data.keyword.messagehub}}. Per ulteriori informazioni, vedi [Utilizzo di un client Kafka](/docs/services/MessageHub/messagehub050.html#kafka_using).


