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
{:note: .note}


# Limitazioni note
{: #restrictions}

Se rilevi un problema mentre stai usando {{site.data.keyword.messagehub}}, riesamina queste restrizioni note e le soluzioni temporanee. 
{: shortdesc}

## Non viene eseguito il failover delle chiamate Java Kafka se si verifica un malfuzionamento di un server di avvio Kafka
{: #calls_failover}

### Problema
{: #calls_failover_problem notoc}

La JVM (Java Virtual Machine) memorizza nella cache le ricerche DNS. Quando la JVM risolve un indirizzo IP per un nome host, memorizza nella cache l'indirizzo IP per un periodo di tempo specificato, noto come TTL (time to live). Alcune configurazioni Java impostano il TTL JVM in modo che non aggiorni mai un indirizzo IP del nome host fino a quando la JVM non viene riavviata. Una configurazione di esempio è una configurazione che dispone di un gestore della sicurezza.

### Soluzione temporanea
{: #calls_failover_workaround notoc}

Poiché {{site.data.keyword.messagehub}} utilizza gli URL del server di avvio Kafka con più indirizzi IP per l'alta disponibilità, non tutti gli indirizzi IP del broker sono noti al client Kafka, il che impedisce il failover a un broker funzionante. In questi casi, il failover richiede una riesecuzione della query degli indirizzi IP per gli URL del broker per ottenere un indirizzo IP funzionante. Ti consigliamo di configurare la tua JVM con un valore TTL compreso tra i 30 a i 60 secondi. Questo valore assicura che, nel caso in cui l'indirizzo IP di un server di avvio presenti dei problemi, il client Kafka sarà in grado di ricercare e utilizzare un nuovo indirizzo IP eseguendo una query del DNS.

Dal file <code>java.security</code>: 

```
# La politica di cache namelookup a livello di Java per delle ricerche eseguite con esito positivo:
#
# qualsiasi valore negativo: memorizzazione in cache per sempre
# qualsiasi valore positivo: il numero di secondi per cui memorizzare in cache un indirizzo
# zero: non memorizzare in cache
#
# il valore predefinito è per sempre (FOREVER). Per motivi di sicurezza, questa memorizzazione
# in cache viene eseguita per sempre quando è impostato un gestore della sicurezza. Quando un
# gestore della sicurezza non è impostato, la modalità di funzionamento predefinita in questa implementazione
# prevede la memorizzazione in cache per 30 secondi.
#
# NOTA: eseguire questa impostazione su qualsiasi valore diverso da quello predefinito può avere serie
#       implicazioni per la sicurezza. Non eseguire questa impostazione a meno che non si sia certi di non essere esposti a
#       un attacco di spoofing DNS.
#
#networkaddress.cache.ttl=-1
```

### Come modificare il TTL della JVM
{: #jvm_ttl notoc}
* Per modificare il TTL della JVM per tutte le applicazioni, imposta il valore <code>networkaddress.cache.ttl</code> nel file <code><var class="keyword varname">$JAVA_HOME</var>/jre/lib/security/java.security</code>.
* Per modificare il TTL della JVM per una specifica applicazione, imposta <code>networkaddress.cache.ttl</code> nel tuo codice applicativo nel seguente modo:
```
java.security.Security.setProperty("networkaddress.cache.ttl" , "30");
```

## Le chiamate Java Kafka potrebbero andare in timeout
{: #calls_timeout_kafka}

### Problema
{: #calls_timeout_problem notoc}

Talvolta una chiamata del client Java Kafka non riesce a trovare Kafka. La causa del malfunzionamento è che il client Kafka ha determinato lo stesso indirizzo IP malfunzionante per ciascuno dei server di avvio. Il client Kafka tenta l'indirizzo IP di ogni broker (che è lo stesso indirizzo IP malfunzionante) e determina erroneamente che Kafka non è attivo. Nota: il client Kafka utilizza il primo indirizzo IP restituito nell'elenco, se nella query DNS vengono restituiti più indirizzi IP.

### Soluzione temporanea
{: #calls_timeout_workaround notoc}

Ritenta le tue chiamate dopo aver atteso per un lasso di tempo sufficiente perché la cache DNS JVM per gli URL del broker scada. Alle successive chiamate Kafka, un indirizzo IP del broker funzionante dovrebbe essere restituito dalla query DNS e utilizzato. 

È stato creato un KIP (Kafka Improvement Proposal) #302 per garantire che i client Kafka tentino tutti gli indirizzi IP di broker e non un sottoinsieme in modo che un malfunzionamento di un singolo indirizzo IP non causi una condizione di malfunzionamento.


## Argomenti e partizioni
{: #topics_partitions}

*  I nomi argomento sono limitati a un massimo di 100 caratteri.
*  Il numero predefinito di partizioni per un argomento è uno.
*  Ogni spazio {{site.data.keyword.Bluemix_notm}} ha un limite di 100 partizioni. Per creare
                    più partizioni, devi utilizzare un nuovo spazio {{site.data.keyword.Bluemix_notm}}.

<!--following message retention info duplicted in FAQs eventstreams108-->

## Conservazione dei messaggi
{: #message_retention}

Per impostazione predefinita, i messaggi vengono conservati in Kafka per un massimo di 24 ore e
ciascuna partizione ha un limite massimo di 1 GB. Se viene raggiunto il limite massimo di 1 GB, i messaggi vengono eliminati per restare entro il limite.

Puoi modificare il limite di tempo per la conservazione dei messaggi quando
crei un argomento utilizzando l'interfaccia utente o
l'API di amministrazione. Il limite di tempo è pari a un minimo di un'ora e
a un massimo di 30 giorni.

Per informazioni sulle limitazioni sulle impostazioni consentite quando crei degli argomenti utilizzando un client Kafka o Kafka Streams, vedi [Come posso utilizzare le API Kafka per creare ed eliminare gli argomenti?](/docs/services/EventStreams?topic=eventstreams-faqs#topic_admin).

## Creazione ed eliminazione di argomenti in Kafka
{: #create_delete}

In Kafka, la creazione e l'eliminazione di argomenti sono delle operazioni asincrone
il cui completamento potrebbe richiedere un certo tempo. Si consiglia di evitare
i modelli di utilizzo che si basano sulla creazione ed eliminazione rapida
di argomenti o sull'eliminazione e ricreazione rapida di argomenti.

<!--
## Kafka REST API
{: #trouble_rest}

<br/>
**Is this specific to old Standard only? If so I'll move to specific Standard topic.**
{: note}

*  Only the binary-embedded format is supported for requests and
   responses. The Avro and JSON embedded formats are not supported.
*  Concurrent requests are not supported for a consumer instance.
   Read, commit, or delete requests corresponding to a consumer
   instance should be sent only after a response is received for
   any outstanding requests of that instance.

-->
<!--
<br/>
**Is this specific to old Standard only? If so I'll move to specific Standard topic.**
{: note}

## Kafka REST API rate limitation
{: #kafka_rate}

Applications using the Kafka REST API can be subject to rate
limiting for each ApiKey. When this limiting occurs, the API
responds with the following HTTP error:

<code>429 Too Many Requests</code>
{:screen}

If you see this error, wait and submit the request again.

<br/>
**Is this specific to old Standard only? If so I'll move to specific Standard topic.**
{: note}
-->
<!--12/04/18 - Karen: same info duplicated at messagehub108 -->
<!--
## Kafka REST API daily restart
{: #rest_restart}

The Kafka REST API restarts once a day for a short period of
time. During this period, the Kafka REST API might become
unavailable. If this happens, you are recommended to retry your
request. After the REST API has restarted, you will have to
create your Kafka consumer instances again. If this is the case, the
REST API returns the following JSON:

```'{"error_code":40403,"message":"Consumer instance not found."}'
```
{:screen}
-->
<!--
## Kafka high-level consumer API
{: #kafka_consumer}

You cannot use the Apache Kafka 0.8.2 simple or high-level
consumer API with {{site.data.keyword.messagehub}}. Instead, you can use the earliest supported Kafka consumer API, which is 0.10.
-->
