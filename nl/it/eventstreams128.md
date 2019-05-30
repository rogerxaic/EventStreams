---

copyright:
  years: 2015, 2019
lastupdated: "2018-07-05"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Utilizzo dell'API client Java Kafka di amministrazione
{: #kafka_java_api}


<!-- 
17/10/17 - Karen: following info duplicated at eventstreams108
 -->

Se utilizzi un client Kafka alla versione 0.11 o successiva, oppure Kafka Streams alla versione 0.10.2.0 o successiva, puoi utilizzare le API per creare ed eliminare gli argomenti. Abbiamo posto alcune restrizioni sulle impostazioni consentite quando crei gli argomenti. Attualmente, puoi modificare solo le seguenti impostazioni:
{: shortdesc}

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

