---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Domande frequenti (FAQ)
{: #faqs}

Risposte alle domande più frequenti sul servizio {{site.data.keyword.IBM}} {{site.data.keyword.messagehub}}.
{: shortdesc}

<!--17/10/17 - Karen: same info duplicated at messagehub104 -->
## Come posso utilizzare le API Kafka per creare ed eliminare gli argomenti?
{: #topic_admin}

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
{{site.data.keyword.messagehub}} conserva gli offset del consumatore per 7 giorni. Questo corrisponde alla configurazione Kafka offsets.retention.minutes. 

a conservazione di offset avviene a livello di sistema, pertanto non puoi impostarla a livello di un singolo argomento. Tutti i gruppi di consumatori ottengono solo 7 giorni di offset memorizzati anche se utilizzano un argomento con una conservazione del log che è stata aumentata fino al massimo di 30 giorni. 

## Che cos'è il comportamento di disponibilità di {{site.data.keyword.messagehub}}?
{: #availability}

Se scrivi applicazioni {{site.data.keyword.messagehub}}, utilizza queste informazioni per comprendere quale sia il normale comportamento di disponibilità di {{site.data.keyword.messagehub}} e cosa deve essere gestito dalle tue applicazioni.

### API
{: #api_availability }

Come parte del normale funzionamento di {{site.data.keyword.messagehub}}, i nodi dei cluster Kafka vengono riavviati occasionalmente.
In alcuni casi, le tue applicazioni riconosceranno quando il cluster riassegna risorse. Scrivi le tue applicazioni in modo che siano resilienti
a queste modifiche e che possano riconnettersi e ritentare le operazioni.

### Bridge {{site.data.keyword.messagehub}} (solo piano Standard)
{: #bridge_availability }

Scrivi le tue applicazioni per gestire la possibilità che i bridge possano riavviarsi di volta in volta.

## Qual è la dimensione massima del messaggio di {{site.data.keyword.messagehub}}? 
{: #max_message_size }

La dimensione massima del messaggio di {{site.data.keyword.messagehub}} è 1 MB, che è il valore predefinito di Kafka. 

## Quali sono le impostazioni della replica di {{site.data.keyword.messagehub}}? 
{: #replication }

{{site.data.keyword.messagehub}} è configurato per fornire elevate disponibilità e durabilità.
Le seguenti impostazioni di configurazione si applicano a tutti gli argomenti e non possono essere modificate:
* replication.factor = 3
* min.insync.replicas = 2

## Come funziona la fatturazione di {{site.data.keyword.messagehub}} nel piano Standard? 
{: #billing }

{{site.data.keyword.messagehub}} nel piano Standard esegue regolarmente il campionamento del conteggio di argomenti di un utente e {{site.data.keyword.Bluemix_notm}} registra il valore di campione massimo ogni giorno. La fatturazione di {{site.data.keyword.messagehub}} riguarda il numero massimo di partizioni simultanee e la somma di messaggi inviati e ricevuti ogni giorno.

Ad esempio, se crei ed elimini 1 argomento 10 volte in un giorno, ti viene addebitato un massimo di 1 argomento. Tuttavia, se crei 10 argomenti e li elimini, ti potrebbero essere abitati 0 o 10 argomenti, a seconda di quando viene eseguito il campionamento.

La fatturazione di {{site.data.keyword.messagehub}} riguarda ciascun messaggio oppure ogni 64 k. Un messaggio fino a 64 k conta come 1 messaggio addebitabile in fattura. I messaggi più grandi di 64 k contano come il successivo numero di messaggi fatturabili: <code><var class="keyword varname">message_size</var> &divide; 64 k</code>.

<!--12/04/18 - Karen: same info duplicated at messagehub057 -->
## Quanto spesso l'API REST Kafka si riavvia? 
{: #REST_restart }

L'API REST Kafka si riavvia una volta al giorno per un breve periodo di
tempo. 

Durante questo periodo, l'API REST Kafka potrebbe non essere
disponibile. Se ciò accade, si consiglia di ritentare la
richiesta. Una volta riavviata l'API REST, dovrai ricreare
le tue istanze consumatore Kafka. In questo caso,
l'API REST restituisce il seguente JSON:

```'{"error_code":40403,"message":"Consumer instance not found."}'
```
{:screen}

## Quali sono le differenze tra i piani {{site.data.keyword.messagehub}} Standard e {{site.data.keyword.messagehub}} Enterprise?
{: #plan_compare }

Per trovare ulteriori informazioni sui due diversi piani {{site.data.keyword.messagehub}}, consulta [Scelta del tuo piano](/docs/services/EventStreams/eventstreams085.html).

## Come gestisco il ripristino di emergenza?
{: #disaster_recovery }

Al momento, è responsabilità dell'utente gestire i propri ripristini di emergenza {{site.data.keyword.messagehub}}. I dati {{site.data.keyword.messagehub}} possono essere replicati tra un'istanza {{site.data.keyword.messagehub}} in una regione e un'altra istanza in una regione diversa. Tuttavia, l'utente è responsabile del provisioning di un'istanza {{site.data.keyword.messagehub}} remota e della gestione della replica.

L'utente è anche responsabile del backup dei dati del payload del messaggio. Anche se questi dati vengono replicati tra più broker Kafka all'interno di un cluster, che protegge contro la maggior parte degli errori, questa replica non copre un errore al livello della regione. 

Viene eseguito il backup dei nomi argomento da {{site.data.keyword.messagehub}}.















