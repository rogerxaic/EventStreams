---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-13"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:faq: data-hd-content-type='faq'}

# Domande frequenti (FAQ)
{: #faqs}

Risposte alle domande più frequenti sul servizio {{site.data.keyword.IBM}} {{site.data.keyword.messagehub}}.

Per risposte a domande specifiche sul piano Classic, vedi [FAQ per il piano Classic](/docs/services/EventStreams?topic=eventstreams-faqs_classic).
{: shortdesc}

<!--17/10/17 - Karen: same info duplicated at messagehub104 -->
## Come posso utilizzare le API Kafka per creare ed eliminare gli argomenti?
{: #topic_admin}
{: faq}

Se utilizzi un client Kafka alla versione 0.11 o successiva, oppure Kafka Streams alla versione 0.10.2.0 o successiva, puoi utilizzare le API per creare ed eliminare gli argomenti. Abbiamo posto alcune restrizioni sulle impostazioni consentite quando crei gli argomenti. Attualmente, puoi modificare solo le seguenti impostazioni:

<dl>
<dt>cleanup.policy</dt>
<dd>Imposta su <code>delete</code> (predefinito), <code>compact</code> o <code>delete,compact</code>
<p>**Nota:**
se la politica di cleanup è solo <code>compact</code>, aggiungiamo automaticamente <code>delete</code> ma disabilitiamo l'eliminazione in base al tempo. I messaggi nell'argomento vengono compattati fino a 1 GB prima di essere eliminati.</p>
</dd>

<dt>retention.ms</dt>
<dd>Il periodo di conservazione predefinito è 24 ore. Il minimo è 1 ora e il massimo è
30 giorni. Specifica questo valore come multipli di ore.

<p>**Nota:**
nel piano Enterprise, puoi impostarlo su qualsiasi valore.</p>
</dd>

<dt>retention.bytes</dt>
<dd>La dimensione massima di una partizione (che consiste in segmenti di log) può crescere fino a prima che eliminiamo i segmenti di log obsoleti per liberare spazio.

<p>**Nota:**
solo piano Enterprise. Imposta su un qualsiasi valore maggiore di 1 MB.</p>
</dd>

<dt>segment.bytes</dt>
<dd>La dimensione del file di segmenti per il log.

<p>**Nota:**
solo piano Enterprise. Imposta su un qualsiasi valore maggiore di 100 kB.</p>
</dd>

<dt>segment.index.bytes</dt>
<dd>La dimensione dell'indice che associa gli offset alle posizioni file. 

<p>**Nota:**
solo piano Enterprise. Imposta su qualsiasi valore compreso tra 100 kB e 2 GB.</p>
</dd>

<dt>segment.ms</dt>
<dd>Il periodo di tempo dopo il quale Kafka forzerà la rotazione del log anche se il file di segmenti non è pieno. 

<p>**Nota:**
solo piano Enterprise. Imposta su qualsiasi valore compreso tra 5 minuti e 30 giorni</p>
</dd>
</dl>


## Per quanto tempo {{site.data.keyword.messagehub}} imposta la finestra di conservazione del log per l'argomento degli offset del consumatore?
{: #offsets }
{: faq}

{{site.data.keyword.messagehub}} conserva gli offset del consumatore per 7 giorni. Questo corrisponde alla configurazione Kafka offsets.retention.minutes. 

a conservazione di offset avviene a livello di sistema, pertanto non puoi impostarla a livello di un singolo argomento. Tutti i gruppi di consumatori ottengono solo 7 giorni di offset memorizzati anche se utilizzano un argomento con una conservazione del log che è stata aumentata fino al massimo di 30 giorni. 

L'argomento <code>__consumer_offsets</code> Kafka interno è visibile per te in sola lettura. 
Ti consigliamo vivamente di non provare a gestire l'argomento in alcun modo. 

<!--following message retention info duplicted in eventstreams057-->

## Per quanto vengono conservati i messaggi?
{: #messages_retained}

Per impostazione predefinita, i messaggi vengono conservati in Kafka per un massimo di 24 ore e
ciascuna partizione ha un limite massimo di 1 GB. Se viene raggiunto il limite massimo di 1 GB, i messaggi vengono eliminati per restare entro il limite.

Puoi modificare il limite di tempo per la conservazione dei messaggi quando
crei un argomento utilizzando l'interfaccia utente o
l'API di amministrazione. Il limite di tempo è pari a un minimo di un'ora e
a un massimo di 30 giorni.

Per informazioni sulle limitazioni sulle impostazioni consentite quando crei degli argomenti utilizzando un client Kafka o Kafka Streams, vedi [Come posso utilizzare le API Kafka per creare ed eliminare gli argomenti?](/docs/services/EventStreams?topic=eventstreams-faqs#topic_admin).

## Che cos'è il comportamento di disponibilità di {{site.data.keyword.messagehub}}?
{: #availability}
{: faq}

Se scrivi applicazioni {{site.data.keyword.messagehub}}, utilizza queste informazioni per comprendere quale sia il normale comportamento di disponibilità di {{site.data.keyword.messagehub}} e cosa deve essere gestito dalle tue applicazioni.

### API
{: #api_availability }

Come parte del normale funzionamento di {{site.data.keyword.messagehub}}, i nodi dei cluster Kafka vengono riavviati occasionalmente.
In alcuni casi, le tue applicazioni riconosceranno quando il cluster riassegna risorse. Scrivi le tue applicazioni in modo che siano resilienti
a queste modifiche e che possano riconnettersi e ritentare le operazioni.

## Qual è la dimensione massima del messaggio di {{site.data.keyword.messagehub}}? 
{: #max_message_size }
{: faq}

La dimensione massima del messaggio di {{site.data.keyword.messagehub}} è 1 MB, che è il valore predefinito di Kafka. 

## Quali sono le impostazioni della replica di {{site.data.keyword.messagehub}}? 
{: #replication }
{: faq}

{{site.data.keyword.messagehub}} è configurato per fornire elevate disponibilità e durabilità.
Le seguenti impostazioni di configurazione si applicano a tutti gli argomenti e non possono essere modificate:
* replication.factor = 3 
* min.insync.replicas = 2

## Quali sono le differenze tra i piani {{site.data.keyword.messagehub}} Standard e {{site.data.keyword.messagehub}} Enterprise?
{: #plan_compare }
{: faq}

Per trovare ulteriori informazioni sui diversi piani {{site.data.keyword.messagehub}}, consulta [Scelta del tuo piano](/docs/services/EventStreams?topic=eventstreams-plan_choose).

## Come gestisco il ripristino di emergenza?
{: #disaster_recovery }
{: faq}

Al momento, è responsabilità dell'utente gestire i propri ripristini di emergenza {{site.data.keyword.messagehub}}. I dati di {{site.data.keyword.messagehub}} possono essere replicati tra un'istanza {{site.data.keyword.messagehub}} in un'ubicazione (regione) e un'altra istanza in un'ubicazione diversa. Tuttavia, l'utente è responsabile del provisioning di un'istanza {{site.data.keyword.messagehub}} remota e della gestione della replica.

L'utente è anche responsabile del backup dei dati del payload del messaggio. Anche se questi dati vengono replicati tra più broker Kafka all'interno di un cluster, che protegge contro la maggior parte degli errori, questa replica non copre un errore al livello dell'ubicazione. 

Viene eseguito il backup dei nomi argomento da {{site.data.keyword.messagehub}}.















