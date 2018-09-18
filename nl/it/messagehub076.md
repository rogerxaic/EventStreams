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

# Perché utilizzare l'API {{site.data.keyword.mql}}?
{: #why_mql}

** L'API MQ Light è disponibile solo come parte del piano Standard.**
<br/>

L'API {{site.data.keyword.mql}} fornisce un livello di astrazione
superiore rispetto all'API Kafka. {{site.data.keyword.mql}} consente di scrivere le applicazioni in modo portabile in un modello di messaggistica unificata che supporta entrambi i modelli di messaggistica di accodamento e di pubblicazione/sottoscrizione.
{:shortdesc}

Le applicazioni scambiano messaggi utilizzando destinazioni create in modo
dinamico, che puoi strutturare in forma gerarchica (ad esempio, <code>‘/sports/football’</code>), raggruppare usando caratteri jolly (ad esempio,
<code>‘/sports/#’</code>) e dotare di controlli per l'assicurazione della consegna e la scadenza del messaggio.
Ciò ti consente di implementare scenari come lo scaricamento del carico di lavoro, la notifica evento e l'elaborazione batch.

Oltre a inviare messaggi tra applicazioni diverse utilizzando l'API {{site.data.keyword.mql}}, puoi anche scambiare messaggi con le applicazioni che utilizzano le API REST o Kafka. In alternativa, puoi anche utilizzare le applicazioni in loco con {{site.data.keyword.IBM}} MQ.

