---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-01"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Cloud Object Storage-Bridge 
{: #cloud_object_storage_bridge }

** {{site.data.keyword.messagehub}}-Bridges sind nur als Bestandteil des Plans "Standard" verfügbar.**
<br/>

Die {{site.data.keyword.IBM}} Cloud Object Storage-Bridge bietet die Möglichkeit, Daten aus einem {{site.data.keyword.messagehub}}-Kafka-Topic zu lesen
und die Daten in [{{site.data.keyword.IBM_notm}} Cloud Object Storage ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/services/cloud-object-storage/about-cos.html){:new_window} zu platzieren.

Die Cloud Object Storage-Bridge ermöglicht das Archivieren von Daten aus den Kafka-Topics von {{site.data.keyword.messagehub}} in einer Instanz des [Cloud Object Storage-Service ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/services/cloud-object-storage/about-cos.html){:new_window}. Die Bridge verarbeitet Nachrichten von Kafka im Stapelbetrieb und lädt die Nachrichtendaten als Objekte in ein Bucket im Cloud Object Storage-Service. Durch Konfigurieren der Cloud Object Storage-Bridge können Sie steuern, wie die Daten als Objekte in Cloud Object Storage hochgeladen werden. Sie können zum Beispiel
die folgenden Eigenschaften konfigurieren:

* Name des Buckets, in den die Objekte geschrieben werden
* Wie oft Objekte in den Cloud Object Storage-Service hochgeladen werden
* Welches Datenvolumen in ein Objekt geschrieben wird, bevor es in den Cloud Object Storage-Service hochgeladen wird

Das Ausgabeformat der Bridge ist ein Objektspeicher-Serviceobjekt mit Datensätzen, die
durch Zeilenvorschubzeichen getrennt sind.

## Vorgehensweise bei der Datenübertragung mit der Cloud Object Storage-Bridge
{: #data_transfer notoc}

Die Cloud Object Storage-Bridge liest eine Anzahl von Kafka-Datensätzen aus einem Topic und schreibt die Daten aus diesen Datensätzen in ein Objekt. Dieses Objekt wird in eine Instanz des Cloud Object Storage-Service hochgeladen. Jede Cloud Object Storage-Bridge liest Nachrichtendaten aus einem bestimmten Kafka-Topic (es kann auch vorkommen, dass mehrere Bridges Daten aus einem Topic lesen). Eine neue Instanz der Cloud Object Storage-Bridge beginnt mit dem Lesen jeweils bei dem ersten vorhandenen Offset im jeweiligen Kafka-Topic. Die Cloud Object Storage-Bridge nutzt die Consumer-Offset-Verwaltung von Kafka für die zuverlässige und verlustfreie Übertragung von Daten aus Kafka. Es besteht jedoch ein geringes Risiko für die Duplizierung von Daten.

Sie können mithilfe der folgenden Eigenschaften steuern, wie viele Datensätze aus Kafka gelesen werden, bevor Daten in die Cloud Object Storage-Serviceinstanz geschrieben werden. Geben Sie beim Erstellen oder Aktualisieren einer Bridge die folgenden Eigenschaften an:
<dl><dt>Schwellenwert für die Upload-Dauer (Sekunden)</dt> 
<dd>Definiert einen Zeitraum in Sekunden, nach dem kumulierten Kafka-Daten in den Cloud Object Storage-Service hochgeladen werden.</dd>
<dt>Schwellenwert für die Upload-Größe (kB)</dt>
<dd>Legt fest, welches Datenvolumen aus Kafka kumuliert wird, bevor die Daten in den Cloud Object Storage-Service hochgeladen werden.</dd>
</dl>

Der Auslöser für die Cloud Object Storage-Bridge, um die aus Kafka gelesenen Daten in den Cloud Object Storage-Service hochzuladen, ist der erste dieser Werte, der erreicht wird. Die Cloud Object Storage-Bridge stellt jedoch nicht sicher, dass genau beim Erreichen dieser Grenzwerte Daten in den Cloud Object Storage-Service übertragen werden. Dies bedeutet, die übertragenen Daten können auch mit Verzögerung eintreffen
oder umfangreicher sein, als die in diesen Eigenschaften angegebenen Werte.

Die Cloud Object Storage-Bridge verwendet das Zeilenvorschubzeichen als Trennzeichen beim Verketten der Nachrichten, die als Daten in Cloud Object Storage geschrieben werden. Aufgrund dieser
Trennzeichen ist die Bridge nicht geeignet für Nachrichten, die eingebettete Zeilenvorschubzeichen enthalten,
und nicht geeignet für binäre Nachrichtendaten.


## Berechtigungsnachweise für die Verwendung der Cloud Object Storage-Bridge abrufen
{: notoc}

Sie müssen Berechtigungsnachweise angeben, damit die Cloud Object Storage-Bridge eine Verbindung mit Ihrer Cloud Object Storage-Instanz herstellen kann. Fordern Sie an, dass der Eigner oder Administrator Ihrer Cloud Object Storage-Instanz die Berechtigungsnachweise mithilfe der Cloud Object Storage-Benutzeroberfläche wie folgt erstellt: 

1. Wählen Sie **Serviceberechtigungsnachweise** aus und fügen Sie einen **Neuen Berechtigungsnachweis** hinzu. 
2. Wählen Sie für **Neuer Berechtigungsnachweis** eine **Zugriffsrolle** des **Schreibberechtigten** und eine **Service-ID** von **Automatisch generieren** aus.
   
   Sie können das sich daraus ergebende JSON aus dem neuen Berechtigungsnachweis in das {{site.data.keyword.messagehub}}-Dashboard in der {{site.data.keyword.Bluemix_notm}}-Konsole kopieren, wenn Sie eine Bridge erstellen. 
   
   Alternativ können Sie die Felder <code>apikey</code> und <code>resource_instance_id</code> in das {{site.data.keyword.messagehub}}-Dashboard eingeben oder in JSON für die Erstellung der Bridge festlegen, wenn Sie die Bridge mit einem REST-Aufruf direkt erstellen.

Der von Ihnen erstellte Berechtigungsnachweis gewährt Schreibzugriff auf die gesamte Cloud Object Storage-Instanz. Daher sollten Sie diesen Zugriff möglicherweise auf den bestimmten Bucket beschränken, mit dem die Bridge interagiert.
1. Gehen Sie zur Seite "[Identity & Access Management" ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/iam/?env_id=ibm%3Ayp%3Aus-south#/serviceids){:new_window}.
2. Die automatisch generierte Service-ID sollte auf dieser Seite angezeigt werden. Wenn Sie die spezifische ID identifiziert haben, wählen Sie die Aktion **Service-ID verwalten** aus. 
3. Wählen Sie die Aktion **Richtlinie bearbeiten** auf, um sie auf mit einem **Ressourcentyp** näher einzugrenzen. Dieser ist ein Bucket und eine **Ressourcen-ID**, die der Name des Buckets ist. Klicken Sie auf **Speichern**.


## Eine Cloud Object Storage-Bridge erstellen
{: notoc}

Verwenden Sie JSON wie im folgenden Beispiel, um eine neue Cloud Object Storage-Bridge mithilfe der Kafka-REST-API zu erstellen. Stellen Sie sicher, dass Ihre Bucket-Namen global und nicht nur innerhalb Ihrer Cloud Object Storage-Instanz eindeutig sind.

<pre class="pre"><code>
{
  "name": "cosbridge",
  "topic": "kafka-java-console-sample-topic",
  "type": "objectStorageS3Out",
  "configuration" : {
    "credentials": {
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


## Wie die Cloud Object Storage-Bridge Partitionsdaten in Objekte aufteilt
{: notoc}

Eine der Funktionen der Cloud Object Storage-Bridge ist das Partitionieren (Aufteilen) der Kafka-Nachrichten und das Speichern der partitionierten Daten in Objekten, deren Namen ein gemeinsames Präfix aufweisen. Eine Gruppe von Objekten, deren Namen ein gemeinsames Präfix aufweisen,
wird als Partition bezeichnet. Das Konzept der Partitionierung in der Cloud Object Storage-Bridge unterscheidet sich von der Kafka-Partitionierung.

Sobald die Cloud Object Storage-Bridge bereit ist, einen Datenstapel in den Cloud Object Storage-Service hochzuladen, wird mindestens ein Objekt erstellt, das die Daten enthält. Die Vorgehensweise beim Partitionieren von Nachrichten
und beim Benennen der resultierenden Datenobjekte hängt von der Konfiguration der Bridge ab. Die Objektnamen
können Kafka-Metadaten sowie gegebenenfalls Daten aus den Nachrichten selbst enthalten. Die Bridge unterstützt gegenwärtig die beiden folgenden Methoden zum Partitionieren von Kafka-Nachrichten in Cloud Object Storage-Objekte:

* Nach dem Offset der Kafka-Nachricht
* Nach dem Datum im ISO 8601-Format, das in jeder Kafka-Nachricht enthalten ist. Bei dieser Methode wird vorausgesetzt, dass die Kafka-Nachrichten ein gültiges Objekt im JSON-Format enthalten.

## Partitionierung nach Offset der Kafka-Nachricht
{: notoc}

Führen Sie die folgenden Schritte aus, um Daten nach dem Offset der Kafka-Nachricht zu partitionieren:

1. Konfigurieren Sie eine Bridge ohne die Eigenschaft `"inputFormat"`.
2. Geben Sie ein Objekt an, dessen Eigenschaft `"type"` den Wert `"kafkaOffset"` im Array `"partitioning"` aufweist. 

    Zum Beispiel:
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

    Die Namen der Objekte, die von einer Bridge mit dieser Konfiguration generiert werden, enthalten das Präfix
    `"offset=<kafka_offset>"`. Dabei gibt `"<kafka_offset>"` die erste
    Kafka-Nachricht an, die in der betreffenden Partition (d. h. in der Objektgruppe mit diesem Präfix) gespeichert wird. Wenn
    eine Bridge beispielsweise Objekte mit ähnlichen Namen wie im folgenden Beispiel generiert, enthalten
    `<object_a>` und `<object_b>` Nachrichten mit Offset-Werten
    im Bereich von 0 bis 999, `<object_c>` enthält Nachrichten mit Offset-Werten im Bereich von 1000 bis
    1999 usw.

    <pre class="pre"><code>
        ```
        &lt;bucket_name&gt;/offset=0/&lt;object_a&gt;
        &lt;bucket_name&gt;/offset=0/&lt;object_b&gt;
        &lt;bucket_name&gt;/offset=1000/&lt;object_c&gt;
        &lt;bucket_name&gt;/offset=2000/&lt;object_d&gt;
        ```       
    </code></pre>
    {:codeblock}
  
    
## Partitionierung nach ISO 8601-Datum
{: #partition_iso notoc}

Führen Sie die folgenden Schritte aus, um Daten nach dem ISO 8601-Datum zu partitionieren:

1. Konfigurieren Sie eine Bridge mit dem Wert `"json"` für die Eigenschaft `"inputFormat"`. Nur eine Eigenschaft `"inputFormat"` mit dem Wert
`"json"` ist zulässig.
2. Geben Sie ein Objekt mit dem Wert `"dateIso8601"` für die Eigenschaft `"type"`und der Eigenschaft `"propertyName"` im Array `"partitioning"` an. 

	Zum Beispiel:
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

	Die Partitioning nach dem ISO 8601-Datum setzt voraus, dass die Kafka-Nachrichten ein gültiges JSON-Format aufweisen. Der Wert
für	`"propertyName"` in der JSON-Konfiguration muss mit dem Feld für das ISO 8601-Datum
in jeder Kafka-Nachricht übereinstimmen. Im vorliegenden Beispiel muss das Feld `"timestamp"`
einen gültigen ISO 8601-Datumswert enthalten. Wenn dies der Fall ist, werden die Nachrichten nach den zugehörigen Datumswerten partitioniert.
	
	Eine Bridge, die entsprechend diesem Beispiel konfiguriert ist, generiert Objekte mit der folgenden Namenskonvention:
	`<object_a>` enthält JSON-Nachrichten, deren Felder `"timestamp"`
das Datum 2016-12-07 enthalten, und sowohl `<object_b>` und `<object_c>` enthalten JSON-Nachrichten, deren Felder `"timestamp"` das Datum 2016-12-08 enthalten.

    <pre class="pre"><code>
        ```
        &lt;bucket_name&gt;/dt=2016-12-07/&lt;object_a&gt;
        &lt;bucket_name&gt;/dt=2016-12-08/&lt;object_b&gt;
        &lt;bucket_name&gt;/dt=2016-12-08/&lt;object_c&gt;
        ```       
    </code></pre>
    {:codeblock}

	Alle Nachrichtendaten, die zwar ein gültiges JSON-Format aufweisen, jedoch kein gültiges Datumsfeld bzw. keinen gültigen Datumswert, werden
in ein Objekt mit dem Präfix `"dt=1970-01-01"` geschrieben.

## Metriken zur Cloud Object Storage-Bridge
{: notoc}

Die Cloud Object Storage-Bridge erfasst Metriken, die mithilfe von Grafana in Ihrem Dashboard angezeigt werden können. Zu den Metriken, die von Interesse sind, gehören die folgenden:
<dl>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.bytes-consumed-rate</code></dt>
<dd>Misst die Datenverarbeitungsgeschwindigkeit der Bridge (in Byte pro Sekunde).</dd>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.records-lag-max</code></dt>
<dd>Misst die maximale Verzögerung in der Anzahl der von der Bridge verarbeiteten Datensätze für jede Partition
in diesem Topic. Ein Anstieg des Werts pro Zeiteinheit gibt an, dass die Bridge nicht mit den Producern
für das Topic Schritt halten kann.</dd>
</dl>
