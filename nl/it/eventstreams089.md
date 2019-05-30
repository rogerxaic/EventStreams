---

copyright:
  years: 2015, 2019
lastupdated: "2018-11-08"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .deprecated}

# Bridge Object Storage sul piano Classic
{: #object_storage_bridge }

** Il bridge {{site.data.keyword.objectstorageshort}} è diventato obsoleto dal primo agosto 2018.**
<br/>

Poiché il servizio sottostante a cui si collega il bridge {{site.data.keyword.objectstorageshort}} è obsoleto, anche il bridge {{site.data.keyword.objectstorageshort}} è diventato obsoleto dal primo agosto 2018. 
{: shortdesc}

Quando il servizio {{site.data.keyword.objectstorageshort}} raggiunge il termine del suo ciclo di vita e viene disattivato, saranno disattivate anche tutte le istanze del bridge {{site.data.keyword.objectstorageshort}}. Per ulteriori informazioni, vedi l'[annuncio di funzionalità deprecata: {{site.data.keyword.objectstorageshort}} OpenStack Swift (PaaS) ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/blogs/bluemix/2018/05/end-marketing-object-storage-openstack-swift-paas/){:new_window}. 

Come alternativa, puoi utilizzare il [Bridge Cloud Object Storage](/docs/services/EventStreams?topic=eventstreams-cloud_object_storage_bridge). 
{:deprecated}

Il bridge {{site.data.keyword.objectstorageshort}} ti consente
di archiviare i dati dagli argomenti Kafka in {{site.data.keyword.messagehub}} in un'istanza del servizio {{site.data.keyword.Bluemix_short}}. Il bridge consuma
batch di messaggi provenienti da Kafka e carica i dati del messaggio come oggetti in un contenitore nel
servizio {{site.data.keyword.objectstorageshort}}.

Nota: il servizio di storage di oggetti preferito in {{site.data.keyword.Bluemix_short}} è ora il servizio [{{site.data.keyword.IBM_notm}} Cloud Object Storage. ![Icona collegamento esterno](../../icons/launch-glyph.svg "Icona collegamento esterno")](docs/services/cloud-object-storage?topic=cloud-object-storage-about#about){:new_window}.

Configurando il bridge {{site.data.keyword.objectstorageshort}}, puoi controllare il modo in cui i dati vengono caricati come oggetti in {{site.data.keyword.objectstorageshort}}. Ad esempio, le proprietà che
puoi configurare sono le seguenti:

* Il nome del contenitore in cui sono scritti gli oggetti.
* La frequenza con cui gli oggetti vengono caricati nel servizio {{site.data.keyword.objectstorageshort}}.
* La quantità di dati scritti in ciascun oggetto prima che questo venga caricato nel servizio {{site.data.keyword.objectstorageshort}}.

Il formato di output del bridge è un oggetto del servizio Object Storage che contiene uno o più
record concatenati con caratteri di nuova riga come separatori.

## Come funziona il trasferimento dei dati utilizzando il bridge {{site.data.keyword.objectstorageshort}}
{: notoc}

La funzione del bridge {{site.data.keyword.objectstorageshort}} consiste nella
lettura di una serie di record Kafka da un argomento e la scrittura dei dati da questi record in un
oggetto. Questo oggetto viene caricato in un'istanza del servizio {{site.data.keyword.objectstorageshort}}. Ogni bridge {{site.data.keyword.objectstorageshort}} legge i dati del messaggio da un
singolo argomento Kafka, sebbene sia possibile che i dati di un argomento vengano letti da
più bridge. Una nuova istanza del bridge {{site.data.keyword.objectstorageshort}}
inizia la lettura sempre dal primo offset presente nell'argomento Kafka. Il bridge {{site.data.keyword.objectstorageshort}} utilizza la gestione degli offset dei consumatori Kafka
per trasferire i dati in modo affidabile da Kafka senza perdite, ma con una minima possibilità di
duplicazione.

Puoi controllare il numero di record che vengono letti da Kafka prima che i dati vengano scritti nell'istanza del servizio {{site.data.keyword.objectstorageshort}} utilizzando
le seguenti proprietà. Specifica queste proprietà quando crei o aggiorni un bridge:
<dl><dt>`"uploadDurationThresholdSeconds"`</dt> 
<dd>Definisce un periodo di tempo, in secondi, dopo il quale i dati accumulati da Kafka vengono caricati nel
servizio {{site.data.keyword.objectstorageshort}}.</dd>
<dt>`"uploadSizeThresholdKB"`</dt>
<dd>Controlla la quantità di dati, in kilobyte, che vengono accumulati da Kafka prima che i dati
vengano caricati nel servizio
{{site.data.keyword.objectstorageshort}}.</dd>
</dl>

L'attivazione del bridge {{site.data.keyword.objectstorageshort}}
per caricare i dati letti da Kafka nel servizio {{site.data.keyword.objectstorageshort}} avviene quando viene raggiunto per primo
uno di questi valori. Il bridge {{site.data.keyword.objectstorageshort}} non garantisce che i dati vengano trasferiti nel servizio {{site.data.keyword.objectstorageshort}} nel momento esatto in cui viene raggiunta
una di queste soglie. Pertanto, i dati trasferiti possono arrivare successivamente o essere più
grandi dei valori specificati da queste proprietà.

Il bridge {{site.data.keyword.objectstorageshort}} concatena
i messaggi utilizzando i caratteri di nuova riga come separatori mentre scrive i dati in {{site.data.keyword.objectstorageshort}}. Questi separatori rendono il
bridge inadatto per i messaggi che contengono caratteri di nuova riga incorporati e per i dati dei messaggi binari.

## Come funziona il partizionamento dei dati in oggetti con il bridge {{site.data.keyword.objectstorageshort}}
{: notoc}

Una della funzioni del bridge {{site.data.keyword.objectstorageshort}} è la sua capacità di partizionare
i messaggi Kafka e di memorizzarli come oggetti denominati con un prefisso comune. Un gruppo di oggetti denominati con un
prefisso comune è chiamato partizione. Il partizionamento del bridge {{site.data.keyword.objectstorageshort}} è un concetto diverso
dal partizionamento di Kafka.

Ogni volta che il bridge {{site.data.keyword.objectstorageshort}}
ha un batch di dati da caricare nel servizio {{site.data.keyword.objectstorageshort}}, crea uno o più
oggetti contenenti i dati. La decisione su come partizionare i messaggi e denominare gli oggetti risultanti
dipende dal modo in cui è configurato il bridge. I nomi degli oggetti possono contenere i metadati di Kafka
ed eventualmente i dati dei messaggi stessi. Attualmente, il bridge supporta i seguenti due modi
per partizionare i messaggi Kafka in oggetti {{site.data.keyword.objectstorageshort}}:

* Per offset dei messaggi Kafka.
* Per una data ISO 8601 presente in ogni messaggio Kafka. Ciò richiede che i messaggi Kafka includano un oggetto di formato JSON valido.

## Partizionamento utilizzando l'offset dei messaggi Kafka
{: notoc}

Per partizionare i dati per offset dei messaggi Kafka, completa la seguente procedura:

1. Configura un bridge senza la proprietà `"inputFormat"`.
2. Specifica un oggetto con un proprietà `"type"` del valore `"kafkaOffset"` nell'array `"partitioning"`. 

    Ad esempio:
    <pre class="pre"><code>
        ```
        {
          "topic": "topic1",
          "type": "objectStorageOut",
          "name": "bridge1",
          "configuration" : {
            "credentials" : { ... },
            "container" : "container1",
            "uploadDurationThresholdSeconds" : "1000",
            "uploadSizeThresholdKB" : "1000",
            "partitioning" : [ {
                "type" : "kafkaOffset"
              }
            ]
          }
        }
        ```
     	</code></pre>
    {:codeblock}

    I nomi dell'oggetto generati da un bridge configurato in questo modo contengono il prefisso
    `"offset=<kafka_offset>"` in cui `"<kafka_offset>"` corrisponde al
    primo messaggio Kafka memorizzato in tale partizione (il gruppo di oggetti con questo prefisso). Ad esempio,
    se un bridge genera degli oggetti con nomi simili al seguente esempio,
    `<object_a>` e `<object_b>` contengono dei messaggi con gli offset
    nell'intervallo 0 - 999, `<object_c>` contiene dei messaggi con gli offset nell'intervallo 1000 -
    1999 e così via.

    <pre class="pre"><code>
        ```
        &lt;container_name&gt;/offset=0/&lt;object_a&gt;
        &lt;container_name&gt;/offset=0/&lt;object_b&gt;
        &lt;container_name&gt;/offset=1000/&lt;object_c&gt;
        &lt;container_name&gt;/offset=2000/&lt;object_d&gt;
        ```       
    </code></pre>
    {:codeblock}
  
    
## Partizionamento per data ISO 8601
{: notoc}

Per partizionare i dati per la data ISO 8601, completa la seguente procedura:

1. Configura un bridge con la proprietà `"inputFormat"` impostata su `"json"`. Non puoi utilizzare una proprietà `"inputFormat"` diversa da
`"json"`.
2. Specifica un oggetto con un proprietà `"type"` del valore `"dateIso8601"` e una proprietà `"propertyName"` nell'array `"partitioning"`. 

	Ad esempio:
    <pre class="pre"><code>
    ```
    {
      "topic": "topic2",
      "type": "objectStorageOut",
      "name": "bridge2",
      "configuration" : {
        "credentials" : { ... },
        "container" : "container2",
        "inputFormat" : "json",
        "uploadDurationThresholdSeconds" : "1000",
        "uploadSizeThresholdKB" : "1000",
        "partitioning": [
          {
            "type": "dateIso8601",
            "propertyName": "timestamp"
          }
        ]
      }
    }
    ```
    </code></pre>
    {:codeblock}

	Il partizionamento per la data ISO 8601 richiede che i massaggi Kafka abbiano un formato JSON valido. Il valore di
	`"propertyName"` nel JSON utilizzato per configurare il bridge deve corrispondere al campo della data ISO
	8601 in ogni messaggio Kafka. In questo esempio, il campo `"timestamp"` deve
	contenere un valore di data ISO 8601 valido. I messaggi vengono quindi partizionati in base alle loro date.
	
	Un bridge configurato come questo esempio genera degli oggetti con i nomi specificati nel seguente modo:
	`<object_a>` contiene dei messaggi JSON con i campi `"timestamp"` con la data
	2016-12-07 e `<object_b>` e `<object_c>` contengono dei messaggi JSON con i campi `"timestamp"` con una data
	2016-12-08.

    <pre class="pre"><code>
        ```
        &lt;container_name&gt;/dt=2016-12-07/&lt;object_a&gt;
        &lt;container_name&gt;/dt=2016-12-08/&lt;object_b&gt;
        &lt;container_name&gt;/dt=2016-12-08/&lt;object_c&gt;
        ```       
    </code></pre>
    {:codeblock}

	Tutti i dati del messaggio con formato JSON corretto ma senza un campo o un valore di data valido vengono scritti in
	un oggetto con il prefisso `"dt=1970-01-01"`.

## Metriche del bridge {{site.data.keyword.objectstorageshort}}
{: notoc}

Il bridge {{site.data.keyword.objectstorageshort}}
riporta le metriche, che possono essere visualizzate sul tuo dashboard utilizzando Grafana. Le metriche di interesse includono:
<dl>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.bytes-consumed-rate</code></dt>
<dd>Misura la velocità con cui il bridge consuma i dati (in byte al secondo).</dd>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.records-lag-max</code></dt>
<dd>Misura il ritardo massimo nel numero di record consumati dal bridge per qualsiasi partizione in
questo argomento. Un valore crescente nel tempo indica che il bridge non mantiene il passo con i produttori
per l'argomento.</dd>
</dl>
