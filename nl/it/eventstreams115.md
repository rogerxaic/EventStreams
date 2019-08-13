---

copyright:
  years: 2015, 2019
lastupdated: "2018-06-01"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Bridge Cloud Object Storage sul piano Classic
{: #cloud_object_storage_bridge }


** Il bridge Cloud Object Storage è disponibile solo come parte del piano Classic.**
<br/>

Il bridge {{site.data.keyword.IBM}} Cloud Object Storage fornisce un modo per leggere i dati da un argomento Kafka {{site.data.keyword.messagehub}}
e inserire i dati in [{{site.data.keyword.IBM_notm}} Cloud Object Storage ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/services/cloud-object-storage?topic=cloud-object-storage-about#about){:new_window}.
{: shortdesc}

Il bridge Cloud Object Storage ti consente di archiviare i dati dagli argomenti Kafka in {{site.data.keyword.messagehub}} un'istanza del servizio [Cloud Object Storage ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/services/cloud-object-storage?topic=cloud-object-storage-about#about){:new_window}. Il bridge consuma
batch di messaggi provenienti da Kafka e carica i dati del messaggio come oggetti in un bucket nel
servizio Cloud Object Storage. Configurando il bridge Cloud Object Storage, puoi controllare il modo in cui i dati vengono caricati come oggetti in Cloud Object Storage. Ad esempio, le proprietà che
puoi configurare sono le seguenti:

* Il nome del bucket in cui sono scritti gli oggetti.
* La frequenza con cui gli oggetti vengono caricati nel servizio Cloud Object Storage.
* La quantità di dati scritti in ciascun oggetto prima che questo venga caricato nel servizio Cloud Object Storage.

Il formato di output del bridge è un oggetto del servizio Object Storage che contiene uno o più
record concatenati con caratteri di nuova riga come separatori.

## Modalità di trasferimento dei dati utilizzando il bridge Cloud Object Storage
{: #data_transfer notoc}

Il bridge Cloud Object Storage funziona leggendo un certo numero di record Kafka da un argomento e scrivendo i dati da questi record in un oggetto. Questo oggetto viene caricato in un'istanza del servizio Cloud Object Storage. Ogni bridge Cloud Object Storage legge i dati del messaggio da un
singolo argomento Kafka, sebbene sia possibile che i dati di un argomento vengano letti da
più bridge. Una nuova istanza del bridge Cloud Object Storage inizia la lettura sempre dal primo offset presente nell'argomento Kafka. Il bridge Cloud Object Storage utilizza la gestione degli offset dei consumatori Kafka
per trasferire i dati in modo affidabile da Kafka senza perdite, ma con una minima possibilità di
duplicazione.

Puoi controllare il numero di record che vengono letti da Kafka prima che i dati vengano scritti nell'istanza del servizio Cloud Object Storage utilizzando
le seguenti proprietà. Specifica queste proprietà quando crei o aggiorni un bridge:
<dl><dt>Upload Duration Threshold (seconds)</dt> 
<dd>Definisce un periodo di tempo, in secondi, dopo il quale i dati accumulati da Kafka vengono caricati nel
servizio Cloud Object Storage.</dd>
<dt>Upload Size Threshold (kB)</dt>
<dd>Controlla la quantità di dati, in kilobyte, che vengono accumulati da Kafka prima che i dati
vengano caricati nel servizio Cloud Object Storage.</dd>
</dl>

Il trigger per il bridge Cloud Object Storage
per caricare i dati letti da Kafka nel servizio Cloud Object Storage avviene quando viene raggiunto per primo uno di questi valori. Il bridge Cloud Object Storage non garantisce che i dati vengano trasferiti nel servizio Cloud Object Storage esattamente al punto in cui viene raggiunta una di queste soglie. Pertanto, i dati trasferiti possono arrivare successivamente o essere più
grandi dei valori specificati da queste proprietà.

Il bridge Cloud Object Storage concatena i messaggi utilizzando i caratteri di nuova riga come separatori mentre scrive i dati in Cloud Object Storage. Questi separatori rendono il
bridge inadatto per i messaggi che contengono caratteri di nuova riga incorporati e per i dati dei messaggi binari.


## Acquisizione delle credenziali da utilizzare con il bridge Cloud Object Storage
{: notoc}

Devi fornire le credenziali per consentire al bridge Cloud Object Storage si stabilire una connessione nella tua istanza Cloud Object Storage. Richiedi che il proprietario o l'amministratore della tua istanza Cloud Object Storage creino le credenziali utilizzando l'IU Cloud Object Storage nel seguente modo: 

1. Seleziona **Credenziali del servizio** e aggiungi quindi una **Nuova credenziale**. 
2. Per la **Nuova credenziale**, seleziona un **Ruolo di accesso** **Scrittore** e un **ID del servizio** **Genera automaticamente**.
   
   Puoi copiare il JSON risultante da questa nuova credenziale nel dashboard {{site.data.keyword.messagehub}} nella console {{site.data.keyword.Bluemix_notm}} quando crei un bridge. 
   
   In alternativa, puoi prendere i campi <code>apikey</code> e <code>resource_instance_id</code> e immetterli nel dashboard {{site.data.keyword.messagehub}} o impostarli nel JSON di creazione del bridge se stai creando il bridge direttamente utilizzando una chiamata REST.

Le credenziale che crei concede l'accesso di scrittore all'intera istanza Cloud Object Storage e, pertanto,
sarebbe meglio questo accesso a uno specifico bucket con cui interagirà il bridge.
1. Vai alla pagina [Manage access and users ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/iam/overview){:new_window}. 
2. Su questa pagina, dovresti vedere l'ID servizio generato automaticamente. Dopo che hai identificato l'ID specifico, seleziona l'azione **Gestisci ID servizio**. 
3. Seleziona l'azione **Modifica politica** per limitarla ulteriormente a uno specifico **Tipo di risorsa**, che è bucket, e un **ID risorsa**, che è il nome del bucket. Fai clic su **Salva**.


## Creazione di un bridge Cloud Object Storage
{: notoc}

Per creare un nuovo bridge Cloud Object Storage utilizzando l'API REST Kafka, usa JSON come nel seguente esempio. Assicurati che i tuoi nomi di bucket siano univoci a livello globale e che non siano univoci solo all'interno della tua istanza Cloud Object Storage.

<pre class="pre"><code>
{
  "name": "cosbridge",
  "topic": "kafka-java-console-sample-topic",
  "type": "objectStorageS3Out",
  "configuration" : {
    "credentials" : {
      "endpoint" : "https://s3-api.us-geo.objectstorage.softlayer.net",
      "resourceInstanceId" : "crn::",
      "apiKey" : "your_api_key"
    },
    "bucket" : "cosbridge0",
    "uploadDurationThresholdSeconds" : 600,
    "uploadSizeThresholdKB" : 1024,
    "partitioning" : [ {
        "type" : "kafkaOffset"
      }
    ]
  }
}
</code></pre>
{: codeblock}


## In che modo il bridge Cloud Object Storage partiziona i dati in oggetti
{: notoc}

Una delle funzioni del bridge Cloud Object Storage è la sua capacità di partizionare i messaggi Kafka
e memorizzarli come oggetti denominati con un prefisso comune. Un gruppo di oggetti denominati con un
prefisso comune è chiamato partizione. Il partizionamento del bridge Cloud Object Storage è un concetto diverso dal
partizionamento di Kafka.

Ogni volta che il bridge Cloud Object Storage ha un batch di dati da caricare nel servizio Cloud Object Storage, crea uno o più
oggetti contenenti i dati. La decisione su come partizionare i messaggi e denominare gli oggetti risultanti
dipende dal modo in cui è configurato il bridge. I nomi degli oggetti possono contenere i metadati di Kafka
ed eventualmente i dati dei messaggi stessi. Attualmente, il bridge supporta i seguenti due modi
per partizionare i messaggi Kafka in oggetti Cloud Object Storage:

* Per offset dei messaggi Kafka.
* Per una data ISO 8601 presente in ogni messaggio Kafka. Ciò richiede che i messaggi Kafka includano un oggetto di formato JSON valido.

## Partizionamento per offset dei messaggi Kafka
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
            "bucket" : "bucket1",
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
        &lt;bucket_name&gt;/offset=0/&lt;object_a&gt;
        &lt;bucket_name&gt;/offset=0/&lt;object_b&gt;
        &lt;bucket_name&gt;/offset=1000/&lt;object_c&gt;
        &lt;bucket_name&gt;/offset=2000/&lt;object_d&gt;
        ```       
    </code></pre>
    {:codeblock}
  
    
## Partizionamento per data ISO 8601
{: #partition_iso notoc}

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
        "bucket" : "bucket2",
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
        &lt;bucket_name&gt;/dt=2016-12-07/&lt;object_a&gt;
        &lt;bucket_name&gt;/dt=2016-12-08/&lt;object_b&gt;
        &lt;bucket_name&gt;/dt=2016-12-08/&lt;object_c&gt;
        ```       
    </code></pre>
    {:codeblock}

	Tutti i dati del messaggio con formato JSON corretto ma senza un campo o un valore di data valido vengono scritti in
	un oggetto con il prefisso `"dt=1970-01-01"`.

## Metriche del bridge Cloud Object Storage
{: notoc}

Il bridge Cloud Object Storage riporta le metriche, che possono essere visualizzate sul tuo dashboard utilizzando Grafana. Le metriche di interesse includono:
<dl>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.bytes-consumed-rate</code></dt>
<dd>Misura la velocità con cui il bridge consuma i dati (in byte al secondo).</dd>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.records-lag-max</code></dt>
<dd>Misura il ritardo massimo nel numero di record consumati dal bridge per qualsiasi partizione in
questo argomento. Un valore crescente nel tempo indica che il bridge non mantiene il passo con i produttori
per l'argomento.</dd>
</dl>
