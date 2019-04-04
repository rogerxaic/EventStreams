---

copyright:
  years: 2015, 2019
lastupdated: "2018-03-23"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Consumo di messaggi
{: #consuming_messages }

Un consumatore è un'applicazione che consuma flussi di messaggi da argomenti Kafka. Un consumatore può sottoscrivere uno o più argomenti o partizioni. Queste informazioni si concentrano sull'interfaccia di programmazione Java, che fa parte del progetto Apache Kafka. I concetti si applicano anche ad altri linguaggi ma i nomi sono a volte leggermente differenti.
{: shortdesc}

Quando stabilisce una connessione a Kafka, un consumatore esegue una connessione bootstrap iniziale. Tale connessione può essere stabilita con qualsiasi server nel cluster. Il consumatore richiede le informazioni della partizione e della leadership sull'argomento da cui desidera consumare. Il consumatore stabilisce quindi un'altra connessione al leader della partizione e può iniziare a consumare messaggi. Tali azioni si verificano automaticamente internamente quando il tuo consumatore stabilisce una connessione al cluster Kafka.

Un consumatore è di norma un'applicazione a lunga esecuzione. Un consumatore richiede messaggi da Kafka richiamando `Consumer.poll(...)` regolarmente. Il consumatore richiama `poll()`, riceve un batch di messaggi, li elabora immediatamente e richiama quindi nuovamente `poll()`.

Quando un consumatore elabora un messaggio, quest'ultimo non viene rimosso dal suo argomento. I consumatori possono invece scegliere tra diversi modi per fare in modo che Kafka rilevi quali messaggi sono stati elaborati. Questo processo è noto come esecuzione del commit dell'offset.

Nell'interfaccia di programmazione, un messaggio è in effetto detto record. Ad esempio, la classe Java org.apache.kafka.clients.consumer.ConsumerRecord viene utilizzata per rappresentare un messaggio per l'API consumatore. I termini _record_ e _messaggio_ possono essere utilizzati in modo interscambievole ma, essenzialmente, un record viene utilizzato per rappresentare un messaggio.

Potresti trovare utile leggere queste informazioni insieme alla [produzione di messaggi](/docs/services/EventStreams?topic=eventstreams-producing_messages) in {{site.data.keyword.messagehub}}.

## Configurazione delle proprietà del consumatore 
{: #configuring_consumer_properties }

Ci sono molte impostazioni di configurazione per il consumatore che controllano gli aspetti della sua modalità di funzionamento. Sono qui di seguito riportate alcune delle più importanti:

| Nome     |Descrizione   | Valori validi   | Valore predefinito   |
|----------|---------|----------|---------|
|key.deserializer     | La classe utilizzata per deserializzare le chiavi. | La classe Java che implementa l'interfaccia Deserializer, come ad esempio org.apache.kafka.common.serialization.StringDeserializer  |Nessun valore predefinito - devi specificare un valore|
|value.deserializer     | La classe utilizzata per deserializzare i valori. | La classe Java che implementa l'interfaccia Deserializer, come ad esempio org.apache.kafka.common.serialization.StringDeserializer  | Nessun valore predefinito - devi specificare un valore |
|group.id | Un identificativo per il gruppo di consumatori a cui appartiene il consumatore. | string |Nessun valore predefinito|
|auto.offset.reset | La modalità di funzionamento quando il consumatore non ha alcun offset iniziale oppure l'offset corrente non è più disponibile nel cluster. | latest, earliest, none | latest |
|enable.auto.commit | Determina se eseguire il commit dell'offset del consumatore automaticamente in background. | true, false | true |
|auto.commit.interval.ms | Il numero di millisecondi tra i commit periodici degli offset. | 0,... | 5000 (5 secondi) |
|max.poll.records | Il numero massimo di record restituiti in una chiamata a poll() | 1,... | 500 |
|session.timeout.ms | Il numero di millisecondi entro cui deve essere ricevuto un heartbeat del consumatore per mantenere l'appartenenza del consumatore a un gruppo di consumatori. | 6000-300000 | 10000 (10 secondi) |
|max.poll.interval.ms |L'intervallo di tempo massimo tra i polling prima che il consumatore lasci il gruppo. | 1,... | 300000 (5 minuti) |
| | | | |

Sono disponibile molte altre impostazioni di configurazione, ma assicurati di leggere la [documentazione di Apache Kafka ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://kafka.apache.org/documentation/){:new_window} attentamente prima di provarle.

## Gruppi di consumatori

Un _gruppo di consumatori_ è un gruppo di consumatori che coopera per consumare messaggi da uno o più argomenti. I consumatori in un gruppo utilizzano tutti lo stesso valore per la configurazione `group.id`. Se ti serve più di un consumatore per gestire il tuo carico di lavoro, puoi eseguire più consumatori nello stesso gruppo di consumatori. Anche se ti serve solo un singolo consumatore, di solito si specifica anche un valore per `group.id`.

Ciascun gruppo di consumatori ha un server nel cluster denominato _coordinatore_ che è responsabile per l'assegnazione delle partizioni ai consumatori nel gruppo. Questa responsabilità è distribuita tra i server nel cluster per uniformare il carico. L?assegnazione di partizioni ai consumatori può cambiare a ogni riequilibratura dei gruppi.

Quando si unisce a un gruppo di consumatori, un consumatore scopre il coordinatore per il gruppo. Il consumatore indica quindi al coordinatore che vuole unirsi al gruppo e il coordinatore avvia una riequilibratura della partizione in tutto il gruppo includendo il nuovo membro.

Quando in un gruppo di consumatori si verifica una delle seguenti modifiche, viene eseguita un ribilanciamento del gruppo spostando l'assegnazione di partizioni ai membri del groppo al fine di adattarsi alla modifica:

* Un consumatore si unisce al gruppo
* un consumatore lascia il gruppo
* un consumatore è considerato non più attivo dal coordinatore
* delle nuove partizioni vengono aggiunte all'argomento esistente

Per ogni gruppo di consumatori, Kafka ricorda l'offset di cui è stato eseguito il commit per ciascuna partizione consumata.

Se hai un gruppo di consumatori che è stato ribilanciato, tieni presente che i commit di qualsiasi consumatore che ha lasciato il gruppo verranno rifiutati finché non si unisce nuovamente al gruppo. in questo caso, il consumatore deve unirsi nuovamente al gruppo, dove gli potrebbe essere assegnata una partizione diversa da quella da cui stava consumando in precedenza.

## Attività del consumatore

Kafka rileva automaticamente i consumatori in errore in modo da poter riassegnare le partizioni ai consumatori funzionanti. Usa due meccanismi per raggiungere tale obiettivo: polling e heartbeating.

Se un batch di messaggi restituito da `Consumer.poll(...)` è di notevoli dimensioni oppure l'elaborazione richiede molto tempo, il ritardo prima di richiamare nuovamente `poll()` può essere significativo o imprevedibile. In alcuni casi, è necessario configurare un lungo intervallo di polling massimo in modo che i consumatori non vengano rimossi dai loro gruppi solo perché l'elaborazione dei messaggi sta impiegando del tempo. Se questo fosse il solo meccanismo, vorrebbe dire che anche il tempo impiegato per rilevare un consumatore in errore sarebbe lungo.

Per rendere lo stato di attività del consumatore facile da gestire, in Kafka 0.10.1 è stato aggiunto un heartbeat di background. Il coordinatore del gruppo prevede che i membri del gruppo gli inviino degli heartbeat regolari per indicare che rimangono attivi. Nel consumatore viene eseguito un thread di heartbeating in background che invia degli heartbeat regolari al coordinatore. Se non riceve un heartbeat da un membro del gruppo entro il _timeout di sessione_, il coordinatore rimuove il membro dal gruppo e avvia un ribilanciamento del gruppo. Il timeout di sessione può essere molto più breve dell'intervallo massimo di polling e, pertanto, il tempo impiegato per rilevare un consumatore in errore può essere breve anche se l'elaborazione dei messaggi impiega del tempo.

Puoi configurare l'intervallo massimo di polling utilizzando la proprietà `max.poll.interval.ms` e il timeout di sessione utilizzando la proprietà `session.timeout.ms`. Di norma non avrai bisogno di usare queste impostazioni a meno che non occorrano più di 5 minuti per elaborare un batch di messaggi.

## Gestione degli offset

Per ogni gruppo di consumatori, Kafka conserva l'offset di cui è stato eseguito il commit per ciascuna partizione consumata. Quando un consumatore elabora un messaggio, non lo rimuove dalla partizione. Si limita invece ad aggiornare il suo offset corrente utilizzando un processo denominato commit dell'offset.

{{site.data.keyword.messagehub}} conserva le informazioni sugli offset di cui è stato eseguito il commit per 7 giorni.

### Che succede se non esiste alcun offset di cui è stato eseguito il commit?
Quando un consumatore viene avviato e gli viene assegnata una partizione da consumare, inizierà dall'offset di cui è stato eseguito il commit del suo gruppo. Se non esiste alcun offset di cui è stato eseguito il commit, il consumatore può scegliere se iniziare con il messaggio meno recente o con quello più recente in base all'impostazione della proprietà `auto.offset.reset` nel seguente modo:

- `latest` (il valore predefinito). Il tuo consumatore riceve e consuma solo i messaggi che arrivano dopo la sottoscrizione. Il tuo consumatore non rileva i messaggi che erano stati inviati prima che eseguisse la sottoscrizione e, pertanto, non devi prevedere che verranno consumati tutti i messaggi da un argomento.
- `earliest`. Il tuo consumatore consuma tutti i messaggi dall'inizio perché rileva tutti i messaggi che sono stati inviati.

Se si verifica un errore del consumatore dopo l'elaborazione di un messaggio ma prima dell'esecuzione del commit del suo offset, le informazioni sull'offset di cui è stato eseguito il commit non rifletteranno l'elaborazione del messaggio. Ciò significa che il messaggio verrà elaborato nuovamente dal successivo consumatore nel gruppo a cui viene assegnata la partizione.

Quando gli offset di cui è stato eseguito il commit vengono salvati in Kafka e i consumatori vengono riavviati, i consumatori riprendono dal punto del loro ultimo arresto. Quando c'è un offset di cui è stato eseguito il commit, la proprietà `auto.offset.reset` non viene utilizzata.

### Esecuzione automatica del commit degli offset

Il modo più facile per eseguire il commit degli offset consiste nel lasciare che il consumatore Kafka esegua tale operazione automaticamente. Ciò è semplice ma dà meno controllo dell'esecuzione manuale del commit. Per impostazione predefinita, un consumatore esegue automaticamente il commit degli offset ogni 5 secondi. Questo commit predefinito avviene ogni 5 secondi a prescindere dai progressi che il consumatore sta facendo relativamente all'elaborazione dei messaggi. Inoltre, quando il consumatore richiama `poll()`, si verifica anche il commit dell'offset più recente restituito dalla chiamata precedente a `poll()` (perché probabilmente è stato elaborato).

Se l'offset di cui è stato eseguito il commit subentra nell'elaborazione dei messaggi, è possibile che alcuni messaggi non sono stati elaborati. Ciò è dovuto al fatto che l'elaborazione ricomincia dall'offset di cui era stato eseguito il commit, che è posteriore all'ultimo messaggio da elaborare prima dell'errore. Per questo motivo, se l'affidabilità è più importante della semplicità, è di norma meglio eseguire il commit degli offset manualmente.

### Esecuzione manuale del commit degli offset

Se `enable.auto.commit` è impostata su `false`, il consumatore esegue il commit dei suoi offset manualmente. Può eseguire tale operazione in modo sincrono o asincrono. Uno schema comune consiste nell'eseguire il commit dell'offset del messaggio elaborato più recente sulla base di un timer periodico. Questo modello significa che ogni messaggio viene elaborato almeno una volta ma l'offset di cui viene eseguito il commit non subentra mai all'avanzamento dei messaggi che stanno venendo elaborati in modo attivo. La frequenza del timer periodico controlla il numero di messaggi che possono essere rielaborati dopo un errore del consumatore. I messaggi vengono recuperati nuovamente dall'offset di cui è stato eseguito il commit salvato per ultimo quando l'applicazione viene riavviata o quando viene eseguito il ribilanciamento del gruppo.

L'offset di cui è stato eseguito il commit è l'offset dei messaggi da cui viene ripresa l'elaborazione. Si tratta di norma dell'offset del messaggio elaborato più di recente *più uno*.

### Ritardo del consumatore

Il ritardo del consumatore per una partizione è la differenza tra l'offset del messaggio pubblicato più di recente e l'offset di cui è stato eseguito il commit del consumatore. Anche se è comune avere delle variazioni naturali nei tassi di produzione e consumo, il tasso di consumo non deve essere più lento del tasso di produzione per un lungo periodo.

Se osservi che un consumatore sta elaborando i messaggi correttamente ma che, di tanto in tanto, sembra che salti un gruppo di messaggi, potrebbe essere un segno che il consumatore non è in grado di reggere il ritmo. Per gli argomenti che non stanno utilizzando la compressione del log, la quantità di spazio del log viene gestita eliminando periodicamente i vecchi segmenti del log. Se un consumatore si ritrova così indietro che sta consumando i messaggi in un segmento del log che è stato eliminato, salterà bruscamente in avanti all'inizio del successivo segmento del log. Se è importante che il consumatore elabori tutti i messaggi, questa modalità di funzionamento indica una perdita di messaggi dal punto di vista di questo consumatore.

Puoi utilizzare lo strumento <code>kafka-consumer-groups</code> per visualizzare il log del consumatore. Per lo stesso scopo puoi anche utilizzare l'API consumatore e le metriche del consumatore.


## Controllo della velocità di consumo dei messaggi
{: #message_consumption_speed }

Se hai problemi con la gestione dei messaggi causata da una saturazione di messaggi, puoi impostare un'opzione del consumatore per controllare la velocità di consumo dei messaggi. Utilizza `fetch.max.bytes` e `max.poll.records` per controllare quanti dati può restituire una chiamata a `poll()`.


## Gestione del ribilanciamento dei consumatori
Quando i consumatori vengono aggiunti a o rimossi da un gruppo, viene eseguito un ribilanciamento del gruppo e i consumatori non sono in grado di consumare messaggi. Di conseguenza, tutti i consumatori in un gruppo di consumatori non sono disponibili per un breve periodo.

Puoi utilizzare ConsumerRebalanceListener per eseguire manualmente il commit degli offset (se non stai utilizzando il commit automatico) in caso di segnalazione di una "on partitions revoked" e per mettere in pausa l'ulteriore elaborazione finché non viene segnalato che il ribilanciamento è stato eseguito correttamente utilizzando la callback "on partition assigned".


## Frammenti di codice
{: #consumer_code_snippets notoc}

Questi frammenti di codice sono a un livello molto alto per illustrare i concetti coinvolti. Per degli esempi completi, vedi gli esempi {{site.data.keyword.messagehub}} in GitHub https://github.com/ibm-messaging/event-streams-samples.

Per stabilire una connessione a {{site.data.keyword.messagehub}}, devi prima creare l'insieme di proprietà di configurazione. Tutte le connessioni a {{site.data.keyword.messagehub}} sono protette utilizzando TLS e l'autenticazione utente/password, quindi ti servono almeno queste proprietà. Sostituisci KAFKA_BROKERS_SASL, USER e PASSWORD con le tue credenziali di servizio:

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

Per consumare i messaggi, dovrai anche specificare i deserializzatori per le chiavi e i valori, ad esempio:

```
 props.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
 props.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
```

Usa quindi un KafkaConsumer per consumare i messaggi, dove ogni messaggio è rappresentato da un ConsumerRecord. Il modo più comune per consumare i messaggi consiste nell'inserire il consumatore in un gruppo di consumatori impostando l'ID gruppo e richiamare quindi `subscribe()` per un elenco di argomenti. Al consumatore verranno assegnate alcune partizioni da consumare, anche se, nel caso in cui ci siano più consumatori nel gruppo che partizioni nell'argomento, al consumatore potrebbe non essere assegnata alcuna partizione. Richiama quindi `poll()` in un loop, ricevendo un batch di messaggi da elaborare, dove ogni messaggio è rappresentato da un ConsumerRecord.

```
props.put("group.id", "G1");
Consumer<String, String> consumer = new KafkaConsumer<>(props);
consumer.subscribe(Arrays.asList("T1"));
while (true) {
   ConsumerRecords<String, String> records = consumer.poll(100);
   for (ConsumerRecord<String, String> record : records)
     System.out.printf("offset = %d, key = %s, value = %s%n", record.offset(), record.key(), record.value());
}
```

Questo loop consumatore è in esecuzione in modo ininterrotto ma può essere interrotto da un altro thread che richiama `Consumer.wakeup()` per ottenere un arresto pulito.

Per eseguire il commit degli offset manualmente, è prima necessario impostare la configurazione `enable.auto.commit` su `false`. Usa quindi `Consumer.commmitSync()` o `Consumer.commitAsync()` per aggiornare periodicamente l'offset di cui è stato eseguito il commit del consumatore. Per semplicità, questo esempio elabora i record per ciascuna partizione ed esegue il commit dell'ultimo offset separatamente.

```
props.put("group.id", "G1");
props.put("enable.auto.commit", "false");
Consumer<String, String> consumer = new KafkaConsumer<>(props);
consumer.subscribe(Arrays.asList("T1"));
try {
  while (true) {
    ConsumerRecords<String, String> records = consumer.poll(100);
    for (TopicPartition tp : records.partitions()) {
      List<ConsumerRecord<String, String>> partRecords = records.records(tp);
      long lastOffset = 0;
      for (ConsumerRecord<String, String> record : partRecords) {
        System.out.printf("offset = %d, key = %s, value = %s%n", record.offset(), record.key(), record.value());
        lastOffset = record.offset();
      }

      consumer.commitSync(Collections.singletonMap(tp, new OffsetAndMetadata(lastOffset + 1)));
    }
  }
}
finally {
  consumer.close();
}
```

## Gestione delle eccezioni

Qualsiasi solida applicazione che utilizza il client Kafka deve gestire le eccezioni per delle specifiche situazioni previste. In alcuni casi, le eccezioni non vengono generate direttamente perché alcun metodi sono asincroni e forniscono i loro risultati utilizzando una `Future` oppure una callback. Puoi trovare il codice di esempio in [GitHub ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-messaging/event-streams-samples) che mostra degli esempi completi.

Viene qui di seguito riportato un elenco delle eccezioni che devi gestire nel tuo codice:

### org.apache.kafka.common.errors.WakeupException
Generato da `Consumer.poll(...)` in seguito al richiamo di `Consumer.wakeup()`. Questo è il modo standard per interrompere il loop di polling del consumatore. Per eseguire una disconnessione pulita, si dovrebbe uscire dal loop di e dovrebbe essere richiamata `Consumer.close()`.
### org.apache.kafka.common.errors.NotLeaderForPartitionException
Generata in seguito a `Producer.send(...)` quando la leadership per una partizione cambia. Il client aggiorna automaticamente i suoi metadati per trovare le informazioni aggiornate sul leader. Ritenta l'operazione, che dovrebbe riuscire con i metadati aggiornati.
### org.apache.kafka.common.errors.CommitFailedException
Generata in seguito a `Consumer.commitSync(...)` quando si verifica un errore irreversibile. In alcuni casi, non è possibile ripetere semplicemente l'operazione perché l'assegnazione delle partizioni potrebbe essere cambiata e il consumatore potrebbe non essere più in grado di eseguire il commit dei suoi offset. Poiché `Consumer.commitSync(...)` può riuscire parzialmente quando viene utilizzato con più partizioni in una singola chiamata, il ripristino a seguito di un errore può essere semplificando utilizzando una chiamata `Consumer.commitSync(...)` separata per ciascuna partizione.
### org.apache.kafka.common.errors.TimeoutException
Generata da `Producer.send(...),  Consumer.listTopics()` se non è possibile recuperare i metadati. L'eccezione viene osservata anche nella callback send (o nella Future restituita) quando il riconoscimento richiesto non viene restituito entro `request.timeout.ms`. Il client può ritentare l'operazione ma l'effetto di un'operazione ripetuta dipende dalla specifica operazione. Ad esempio, se viene ritentato l'invio di un messaggio, tale messaggio potrebbe essere duplicato.

