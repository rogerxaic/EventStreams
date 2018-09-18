---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-23"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Produzione di messaggi
{: #producing_messages }


Un produttore è un'applicazione che pubblica flussi di messaggi ad argomenti Kafka. Queste informazioni si concentrano sull'interfaccia di programmazione Java, che fa parte del progetto Apache Kafka. I concetti si applicano anche ad altri linguaggi ma i nomi sono a volte leggermente differenti.

Nell'interfaccia di programmazione, un messaggio è in effetto detto record. Ad esempio, la classe Java org.apache.kafka.clients.producer.ProducerRecord viene utilizzata per rappresentare un messaggio dal punto di vista dell'API del produttore. I termini _record_ e _messaggio_ possono essere utilizzati in modo interscambievole ma, essenzialmente, un record viene utilizzato per rappresentare un messaggio.

Quando stabilisce una connessione a Kafka, un produttore esegue una connessione bootstrap iniziale. Tale connessione può essere stabilita con qualsiasi server nel cluster. Il produttore richiede le informazioni della partizione e della leadership sull'argomento in cui desidera pubblicare. Il produttore stabilisce quindi un'altra connessione al leader della partizione e può iniziare a pubblicare messaggi. Tali azioni si verificano automaticamente internamente quando il tuo produttore stabilisce una connessione al cluster Kafka.
 
Quando viene inviato al leader della partizione, tale messaggio non è immediatamente disponibile per i consumatori. Il leader accoda il record per il messaggio alla partizione, assegnando ad esso il numero di offset successivo per tale partizione. Dopo che tutti i follower per le repliche sincronizzate hanno replicato il record e riconosciuto che hanno scritto il record nelle loro repliche, per il record si considera ora eseguito il *commit* e diventa disponibile per i consumatori.

Ciascun messaggio è rappresentato come un record che si articola in due parti: chiave e valore. La chiave viene di solito usata per i dati relativi al messaggio e il valore è il corpo del messaggio. Poiché molti strumenti nell'ecosistema Kafka (quali i connettori ad altri sistemi) utilizzano solo il valore e ignorano la chiave, è meglio inserire tutti i dati del messaggio nel valore e utilizzare la chiave solo per il partizionamento o la compattazione del log. Non devi contare sul fatto che tutte le applicazioni che leggono da Kafka facciano uso della chiave.

Anche molti altri sistemi di messaggistica hanno un modo per trasportare altre informazioni insieme ai messaggi. A tale scopo, Kafka 0.11 introduce delle intestazioni di record, che sono supportate dal piano {{site.data.keyword.messagehub}} Enterprise. Il piano {{site.data.keyword.messagehub}} Standard è attualmente basato su Kafka 0.10.2.1, quindi non supporta ancora le intestazioni di record. 

Potresti trovare utile leggere queste informazioni insieme al [consumo di messaggi](/docs/services/MessageHub/messagehub114.html) in {{site.data.keyword.messagehub}}.

## Impostazioni di configurazione
{: #config_settings}

Ci sono molte impostazioni di configurazione per il produttore. Puoi controllare degli aspetti del produttore, compresi l'organizzazione in batch, i nuovi tentativi e il riconoscimento dei messaggi. Sono qui di seguito riportati quelli più importanti:

| Nome     |Descrizione   | Valori validi   | Valore predefinito   |
|----------|---------|----------|---------|
|key.serializer     | La classe utilizzata per serializzare le chiavi. | La classe Java che implementa l'interfaccia Serializer, come ad esempio org.apache.kafka.common.serialization.StringSerializer |Nessun valore predefinito - devi specificare un valore|
|value.serializer     | La classe utilizzata per serializzare i valori. | La classe Java che implementa l'interfaccia Serializer, come ad esempio org.apache.kafka.common.serialization.StringSerializer  | Nessun valore predefinito - devi specificare un valore |
|acks     | Il numero di server a cui è richiesto di riconoscere ogni messaggio pubblicato. Questo controlla le garanzie di durabilità richieste dal produttore. | 0, 1, all (oppure -1)  | 1 |
|retries     | Quante volte il client invia nuovamente un messaggio quando l'invio riscontra un errore. | 0,...  | 0 |
|max.block.ms     | Il numero di millisecondi per cui una richiesta di invio o di metadati può bloccarsi in attesa. | 0,...  | 60000 (1 minuto) |
|max.in.flight.requests.per.connection     | Il numero massimo di richieste non riconosciute che il client invia su una connessione prima di bloccare ulteriori richieste| 1,...  | 5 |
|request.timeout.ms     | Il periodo di tempo massimo per cui un produttore attende una risposta a una richiesta. Se la risposta non viene ricevuta prima che il timeout sia trascorso, la richiesta viene ritentata oppure non riesce se è stato fatto il numero di nuovi tentativi massimo consentito.| 0,...  | 30000 (30 secondi) |

Sono disponibili molte altre impostazioni di configurazione, ma assicurati di leggere la [documentazione di Apache Kafka ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://kafka.apache.org/documentation/){:new_window} attentamente prima di provarle.

## Partizionamento
{: #partitioning}

Quando pubblica un messaggio su un argomento, il produttore può scegliere quale partizione utilizzare. Se l'ordinamento è importante, devi ricordarti che una partizione è una sequenza ordinata di record ma che un argomento include una o più partizioni. Se vuoi che un insieme di messaggi venga recapitato in ordine, assicurati che vadano tutti nella stessa partizione. La soluzione migliore per ottenere questo risultato consiste nel dare a tutti questi messaggi la stessa chiave. 
 
Il produttore può specificare esplicitamente un numero di partizione quando pubblica un messaggio. Ciò offre un controllo diretto ma rende più complesso il codice del produttore poiché si assume la responsabilità della gestione della selezione della partizione. Per ulteriori informazioni, vedi la chiamata al metodo Producer.partitionsFor. Ad esempio, la chiamata è descritta per [Kafka 0.11.0.1 ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://kafka.apache.org/0110/javadoc/org/apache/kafka/clients/producer/KafkaProducer.html){:new_window}
 
Se il produttore non specifica un numero partizione, la selezione della partizione viene eseguita da un partitioner. Il partitioner predefinito integrato nel produttore Kafka funziona nel seguente modo:

* Se il record non ha una chiave, seleziona la partizione in modo round-robin.
* Se il record ha una chiave, seleziona la partizione calcolando un valore hash per la chiave. In questo modo, viene selezionata la stessa partizione per tutti i messaggi con la stessa chiave.

Puoi anche scrivere un tuo partitioner personalizzato. Un partitioner personalizzato può scegliere qualsiasi schema per assegnare i record alle partizioni. Usa ad esempio solo un sottoinsieme delle informazioni nella chiave oppure un identificativo specifico dell'applicazione.


## Ordinamento dei messaggi
{: #message_ordering}

Kafka di norma scrive i messaggi nell'ordine in cui vengono inviati dal produttore. Ci sono tuttavia delle situazioni in cui dei nuovi tentativi possono causare la duplicazione o il riordino dei messaggi. Se vuoi che una sequenza di messaggi venga inviata in ordine, è importante che ti assicuri che siano tutti scritti nella stessa partizione.
 
Il produttore può anche tentare nuovamente l'invio di messaggi automaticamente. È spesso una buona idea abilitare questa funzione di nuovi tentativi perché l'alternativa è che il tuo codice applicativo debba esso stesso eseguire eventuali nuovi tentativi. La combinazione di organizzazione in batch in Kafka e dei nuovi tentativi automatici può avere l'effetto di duplicare i messaggi e riordinarli.
 
Ad esempio, se pubblici una sequenza di tre messaggi &lt;M1, M2, M3&gt; su un argomento, i record potrebbero tutti rientrare nello stesso batch e, pertanto, vengono effettivamente inviati tutti insieme al leader nella partizione. Il leader li scrive quindi nella partizione e li replica come record separati. Nel caso di un malfunzionamento, è possibile che M1 ed M2 vengano aggiunti alla partizioni ma che ciò non accada per M3. Il produttore non riceve un riconoscimento, quindi prova nuovamente a inviare &lt;M1, M2, M3&gt;. Il nuovo leader scrive semplicemente M1, M2 e M3 nella partizione, che contiene ora &lt;M1, M2, M1, M2, M3&gt;, dove l'M1 duplicato in effetti viene dopo l'M2 originale. Se limiti il numero di richieste in-flight a ciascun broker a uno solo, puoi evitare questo riordino. Potresti comunque rilevare che un singolo record è duplicato, come ad esempio &lt;M1, M2, M2, M3&gt;, ma non otterrai mai delle sequenze non ordinate. In Kafka 0.11 (non ancora disponibile in {{site.data.keyword.messagehub}}), puoi anche utilizzare la funzione produttore idempotent per impedire la duplicazione di M2.
 
È prassi normale con Kafka scrive le applicazioni per gestire occasionali duplicati di messaggi perché l'impatto sulle prestazioni derivante dall'avere solo una singola richiesta in-flight è considerevole.

## Riconoscimenti dei messaggi
{: #message_acknowledgments}

Quando pubblichi un messaggio, puoi scegliere il livello di riconoscimenti richiesto utilizzando la configurazione del produttore `acks`. La scelta rappresenta un bilanciamento tra la velocità effettiva e l'affidabilità. Ci sono tre livelli, come di seguito indicato:

<dl>
<dt>acks=0 (meno affidabile)</dt>
<dd>Il messaggio viene considerato come inviato appena è stato scritto nella rete. Non c'è alcun riconoscimento dal leader della partizione. Di conseguenza, i messaggi possono andare perduti se la leadership della partizione cambia. Questo livello di riconoscimento è molto veloce ma, in alcune situazioni, presenta la possibilità di una perdita dei messaggi.</dd>
<dt>acks=1 (il valore predefinito)</dt>
<dd>Il messaggio viene riconosciuto al produttore appena il leader della partizione ha scritto correttamente il suo record nella partizione. Poiché il riconoscimento si verifica prima che sia rilevato il fatto che il record abbia raggiunto le repliche sincronizzate, il messaggio potrebbe andare perduto se si verifica un malfunzionamento del leader ma i follower non hanno ancora il messaggio. Se la leadership della partizione cambia, il vecchio leader informa il produttore, che può gestire l'errore e ritentare l'invio del messaggio al nuovo leader. Poiché i messaggi vengono riconosciuti prima che la loro ricezione sia stata confermata da tutte le repliche, i messaggi che sono stati riconosciuti ma non ancora completamente replicati possono andare perduti se la leadership della partizione cambia.</dd>
<dt>acks=all (più affidabile)</dt>
<dd>Il messaggio viene riconosciuto al produttore quando il leader della partizione ha scritto correttamente il suo record nella partizione e tutte le repliche sincronizzate hanno fatto lo stesso. Il messaggio non va perduto se il leader della partizione cambia, a condizione che sia disponibile almeno una replica sincronizzata.</dd>
</dl>

Anche se non attendi che i messaggi vengano riconosciuti al produttore, i messaggi sono comunque disponibili per il consumo solo quando ne è stato eseguito il commit, cosa che significa che la replica alle repliche sincronizzate è completa. In altre parole, la latenza dell'invio di messaggi dal punto di vista del produttore è inferiore alla latenza end-to-end misurata dal produttore che invia un messaggio a un consumatore che riceve il messaggio.

Se possibile, evita di attendere il riconoscimento di un messaggio prima di pubblicare il messaggio successivo. L'attesa impedisce al produttore di poter organizzare insieme in batch i messaggi e riduce anche il tasso a cui messaggi possono essere pubblicati al di sotto della latenza di round-trip della rete.

## Organizzazione in batch, limitazione e compressione
{: #batching}

Ai fini dell'efficienza, il produttore raccoglie effettivamente dei batch di record insieme per l'invio ai server. Se abiliti la compressione, il produttore comprime ogni batch, il che può migliorare le prestazioni perché richiede il trasferimento di una quantità inferiore di dati sulla rete.

Se probi a pubblicare i messaggi più rapidamente di quanto possano essere inviati al server, il produttore li memorizza automaticamente in buffer in richieste organizzate in batch. Il produttore conserva un buffer di record non inviati per ciascuna partizione. Ovviamente si arriva a un punto in cui anche l'organizzazione in batch non consente di raggiungere il tasso desiderato.
 
C'è un altro fattore che ha un impatto. per evitare che i singoli produttori o consumatori sommergano il cluster, {{site.data.keyword.messagehub}} applica delle quote di velocità effettiva. Il tasso a cui ciascun produttore sta inviando i dati viene calcolato e ogni produttore che prova a superare la sua quota viene limitato. La limitazione viene applicata ritardando leggermente l'invio di risposte al produttore. Di norma, questo agisce solo come un freno naturale.
 
Riepilogando, quando un messaggio viene pubblicato, il suo record viene prima scritto in un buffer nel produttore. In background, il produttore organizza in batch e invia i record al server. Il server risponde quindi al produttore, possibilmente applicando un ritardo di limitazione se il produttore sta pubblicando troppo rapidamente. Se il buffer nel produttore si riempie, la chiamata di invio del produttore viene ritardata ma, alla fine, potrebbe non riuscire con un'eccezione.

## Frammenti di codice
{: #code_snippets}

Questi frammenti di codice sono a un livello molto alto per illustrare i concetti coinvolti. Per degli esempi completi, vedi gli esempi {{site.data.keyword.messagehub}} in GitHub https://github.com/ibm-messaging/message-hub-samples.

Per stabilire una connessione a {{site.data.keyword.messagehub}}, devi prima creare l'insieme di proprietà di configurazione. Tutte le connessioni a {{site.data.keyword.messagehub}} sono protette utilizzando TLS e l'autenticazione utente/password, quindi ti servono come minimo queste proprietà. Sostituisci KAFKA_BROKERS_SASL, USER e PASSWORD con le tue credenziali di servizio:

```
Properties props = new Properties();
 props.put("bootstrap.servers", KAFKA_BROKERS_SASL);
 props.put("sasl.jaas.config", "org.apache.kafka.common.security.plain.PlainLoginModule required username=\"USER\" password=\"PASSWORD\";");
 props.put("security.protocol", "SASL_SSL");
 props.put("sasl.mechanism", "PLAIN");
 props.put("ssl.protocol", "TLSv1.2");
 props.put("ssl.enabled.protocols", "TLSv1.2");
 props.put("ssl.endpoint.identification.algorithm", "HTTPS");
```

Per inviare i messaggi, dovrai anche specificare i serializzatori per le chiavi e i valori, ad esempio:

```
 props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
 props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");
```

Usa quindi un KafkaProducer per inviare i messaggi, dove ogni messaggio è rappresentato da un ProducerRecord. Non dimenticarti di chiudere il KafkaProducer, quando hai finito. Questo codice invia solo il messaggio ma non attende per appurare se l'invio è riuscito.

```
 Producer<String, String> producer = new KafkaProducer<>(props);
 producer.send(new ProducerRecord<String, String>("T1", "key", "value"));
 producer.close();
 ```
 
Il metodo `send()` è asincrono e restituisce una Future che puoi usare per controllarne il completamento:

```
 Future<RecordMetadata> f = producer.send(new ProducerRecord<String, String>("T1", "key", "value"));
// Do some other stuff
// Now wait for the result of the send
 RecordMetadata rm = f.get();
 long offset = rm.offset; 
```

In alternativa, puoi fornire una callback in fase di invio del messaggio:

```
producer.send(new ProducerRecord<String,String>("T1","key","value", new Callback() {
          public void onCompletion(RecordMetadata metadata, Exception exception) {
                     // This is called when the send completes, either successfully or with an exception
          }
});
```

Per ulteriori informazioni, vedi la [Javadoc per il client Kafka ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://kafka.apache.org/0110/javadoc/index.html){:new_window}, che è molto dettagliata. 

