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

# Abilitazione dell'API {{site.data.keyword.mql}} in {{site.data.keyword.messagehub}}
{: #mql_enable}

** L'API MQ Light è disponibile solo come parte del piano Standard.**
<br/>

**Prima di poter utilizzare l'API, devi creare in modo esplicito un argomento Kafka denominato "MQLight" in quanto tutti i messaggi passano attraverso l'argomento "MQLight". Questo argomento deve avere una singola partizione. La creazione di questo argomento abilita l'API MQ Light per la tua istanza del servizio. **  

L'argomento "MQLight" è utilizzato dall'API MQ Light per memorizzare i dati di messaggi e per interagire con altri client Kafka. Tieni presente che quando si crea
questo argomento, vengono applicati dei costi alla tariffa standard descritta nel piano di pagamento dei servizi.

Per disabilitare l'API MQ Light, elimina l'argomento "MQLight". Ricorda che all'eliminazione dell'argomento vengono eliminati tutti i dati.
