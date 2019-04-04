---

copyright:
  years: 2015, 2019
lastupdated: "2018-01-16"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Leadership della partizione
{: #partition_leadership }

Ogni partizione ha un server nel cluster che funge da suo leader e altri server che fungono da follower. Tutte le richieste di produzione e consumo per la partizione sono gestite dal leader. I follower replicano i dati della partizione dal leader con l'obiettivo di tenere il passo con quest'ultimo. Se un follower sta tenendo il passo con un leader di una partizione, la sua replica è sincronizzata. 
{: shortdesc}

Quando viene inviato al leader della partizione, tale messaggio non è immediatamente disponibile per i consumatori. Il leader accoda il record per il messaggio alla partizione, assegnando ad esso il numero di offset successivo per tale partizione. Dopo che tutti i follower per le repliche sincronizzate hanno replicato il record e riconosciuto che hanno scritto il record nelle loro repliche, per il record si considera ora eseguito il *commit*. Il messaggio è disponibile per i consumatori.

Se si verifica un malfunzionamento del leader per una partizione, uno dei follower con una replica sincronizzata subentra automaticamente al ruolo di leader della partizione. In pratica, ogni server è il leader per alcune partizioni e il follower per altre. La leadership di partizioni è dinamica e cambia man mano che i server vanno e vengono.

Le applicazioni non devono eseguire delle azioni specifiche per gestire la modifica nella leadership di una partizione. La libreria client Kafka ristabilisce automaticamente una connessione al nuovo leader, anche se riscontrerai una latenza aumentata mentre il cluster si aggiorna.
