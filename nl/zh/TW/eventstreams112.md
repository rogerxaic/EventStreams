---

copyright:
  years: 2015, 2019
lastupdated: "2018-06-23"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 產生訊息
{: #producing_messages }


生產者是一個將訊息串流發佈至 Kafka 主題的應用程式。此資訊主要討論作為 Apache Kafka 專案一部分的 Java 程式設計介面。這些概念也套用於其他語言，但名稱有時會稍有不同。
{: shortdesc}

在程式設計介面中，訊息實際上被稱為記錄。例如，Java 類別 org.apache.kafka.clients.producer.ProducerRecord 用來表示來自生產者 API 觀點的訊息。可交換使用_記錄_ 及_訊息_ 術語，但基本上記錄是用來表示訊息。

當生產者連接至 Kafka 時，即會起始引導連線。此連線可連接至叢集中的任何伺服器。生產者會要求與其想要發佈至其中的主題有關的分割區及領導權資訊。然後，生產者會建立另一個連接至分割區領導者的連線，且可以開始發佈訊息。當您的生產者連接至 Kafka 叢集時，即會在內部自動發生這些動作。
 
當訊息傳送至分割區領導者時，消費者無法立即取用該訊息。領導者會將訊息記錄附加至分割區，為其指派該分割區的下一個偏移數。在所有同步抄本的追隨者已抄寫記錄並確認它們已將記錄寫入其抄本中之後，此記錄現在*已確定* 且可供消費者使用。

每一則訊息都表示為一筆記錄，其包含兩個部分：索引鍵及值。索引鍵通常用於有關訊息的資料，值則是訊息內文。因為 Kafka 生態系統中的許多工具（例如連接至其他系統的連接器）都只使用值而忽略索引鍵，所以最好是將所有訊息資料都放入值中，並只使用索引鍵進行分割或日誌壓縮。您不應該根據從 Kafka 讀取的任何內容來使用索引鍵。

許多其他傳訊系統也有一種攜帶其他資訊與訊息的方式。Kafka 0.11 基於此目的引進了記錄標頭。

您可能會發現在 {{site.data.keyword.messagehub}} 中使用[取用訊息](/docs/services/EventStreams?topic=eventstreams-consuming_messages)一起讀取此資訊會很有用。

## 配置設定
{: #config_settings}

生產者有多種配置設定。您可以控制生產者的各個層面，包括分批處理、重試次數及訊息確認通知。以下是最重要的部分：

|名稱     |說明|有效值   |預設值|
|----------|---------|----------|---------|
|key.serializer|此類別用來序列化索引鍵。|用於實作序列化程式介面的 Java 類別，例如 org.apache.kafka.common.serialization.StringSerializer |無預設值 - 您必須指定一個值|
|value.serializer|此類別用來序列化值。|用於實作序列化程式介面的 Java 類別，例如 org.apache.kafka.common.serialization.StringSerializer |無預設值 - 您必須指定一個值|
|acks|用來確認每一則已發佈訊息所需的伺服器數目。這會控制生產者所需的延續性保證。|0、1、all（或 -1） |1|
|retries     |傳送發生錯誤時，用戶端重送訊息的次數。|0、...  |0 |
|max.block.ms     |傳送或 meta 資料要求可以阻擋等待的毫秒數。|0、...  |60000（1 分鐘）|
|max.in.flight.requests.per.connection     |在阻擋進一步要求之前，用戶端在連線上傳送的未確認要求的最大數目。|1、...  |5 |
|request.timeout.ms     |生產者等待要求回應的最長時間量。如果在逾時之前未收到回應，則會重試要求，或者，如果重試次數已耗盡，則會失敗。|0、...  |30000（30 秒）|

有許多配置設定可供使用，但在試驗之前，請先確定您已仔細閱讀 [Apache Kafka 文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://kafka.apache.org/documentation/){:new_window}。

## 分割區
{: #partitioning}

當生產者在主題上發佈訊息時，生產者可以選擇要使用的分割區。如果排序很重要，則必須記住，分割區是有序的記錄序列，但是一個主題包含一個以上分割區。如果您要依序遞送一組訊息，請確定它們全部都在同一個分割區中。達成此目的的最直接方式為對所有這些訊息提供相同的索引鍵。 
 
在生產者發佈訊息時，可以明確指定分割區號碼。這提供直接控制，但會讓生產者程式碼更複雜，因為它要負責管理分割區選擇。如需相關資訊，請參閱呼叫 Producer.partitionsFor 的方法。例如，針對 [Kafka 1.1.0 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://kafka.apache.org/11/javadoc/org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} 所說明的呼叫。
 
如果生產者未指定分割區號碼，則由分割程式進行分割區選擇。Kafka 生產者的內建預設分割程式的運作方式如下：

* 如果記錄沒有索引鍵，請依循環方式選取分割區。
* 如果記錄具有索引鍵，請藉由計算索引鍵的雜湊值來選取分割區。這具有為所有含有相同索引鍵的訊息選取相同分割區的效果。

您也可以撰寫自己的自訂分割程式。自訂分割程式可以選擇任何方法來將記錄指派給分割區。例如，只使用索引鍵中的資訊子集或使用應用程式特有的 ID。


## 訊息排序
{: #message_ordering}

Kafka 通常會依據生產者傳送的順序來撰寫訊息。不過，在某些情況下，重試可能會導致訊息被複製或重新排序。如果您要依序傳送一系列訊息，請務必確定它們全部都已寫入同一個分割區中。
 
生產者也可以自動重試傳送訊息。啟用此重試特性通常是一個好主意，因為替代方案是您的應用程序碼必須自行執行所有重試。Kafka 中的分批處理及自動重試的組合可具有複製訊息並重新予以排序的效果。
 
例如，如果您在主題上發佈一系列三則訊息 &lt;M1、M2、M3&gt;，則記錄可能全部都在同一個批次中，因此它們實際上會一起傳送至分割區領導者。然後，領導者會將它們寫入分割區，並將它們作為個別記錄進行抄寫。如果失敗，有可能是 M1 及 M2 已加入分割區，但 M3 沒有。生產者沒有收到確認通知，所以它會重新傳送 &lt;M1、M2、M3&gt;。新領導者單純地將 M1、M2 及 M3 寫入分割區，該分割區現在包含 &lt;M1、M2、M1、M2、M3&gt;，其中重複的 M1 實際上遵循原始的 M2。如果您將每一個分配管理系統的進行中要求數目限制為一個，則可以防止此重新排序。您可能仍然會發現單一記錄是重複的，例如 &lt;M1、M2、M2、M3&gt;，但是永遠不會出現亂序。在 Kafka 0.11（{{site.data.keyword.messagehub}} 尚未提供）中，您也可以使用等冪生產者特性來防止複製 M2。
 
Kafka 通常會撰寫應用程序來處理偶然的訊息重複，因為只有單一要求在進行中的效能影響是很顯著的。

## 訊息確認通知
{: #message_acknowledgments}

當您發佈訊息時，可以使用 `acks` 生產者配置來選擇所需的確認通知等級。該選項表示傳輸量與可靠性之間的平衡。共有三個等級，如下所示：

<dl>
<dt>acks=0（最不可靠）</dt>
<dd>訊息一旦寫入網路中，即被視為已傳送。沒有來自分割區領導者的確認通知。因此，如果分割區領導權發生變更，則訊息可能會遺失。這個確認通知等級非常快速，但在某些情況下可能會出現訊息流失。</dd>
<dt>acks=1（預設值）</dt>
<dd>一旦分割區領導者順利地將其記錄寫入分割區中，訊息就會被確認給生產者。因為確認通知發生在已知記錄已到達同步抄本之前，所以如果領導者失敗，但追隨者尚未收到訊息，則該訊息可能會遺失。如果分割區領導權發生變更，舊的領導者會通知生產者，生產者可處理錯誤並重試將訊息傳送給新的領導者。因為訊息在所有抄本確認收到之前已經確認，所以如果分割區領導權發生變更，已經確認但尚未完整抄寫的訊息可能會遺失。</dd>
<dt>acks=all（最可靠）</dt>
<dd>當分割區領導者順利地撰寫其記錄且所有同步抄本都執行相同作業，訊息就會被確認給生產者。如果分割區領導權發生變更，訊息也不會遺失，前提是至少有一個同步抄本可供使用。</dd>
</dl>

即使您不等待訊息被確認給生產者，訊息仍然只能在已確定時被取用，這表示抄寫到同步抄本已完成。換言之，從生產者的觀點來看傳送訊息的延遲低於從生產者傳送訊息到接收訊息的消費者所測量的端對端延遲。

如果可能的話，在發佈下一則訊息之前，請避免等待訊息的確認通知。等待防止生產者能夠將訊息分批在一起，並且還可以將訊息發佈的速率降低到低於網絡的來回轉換延遲。

## 分批處理、節流控制及壓縮
{: #batching}

為了提高效率，生產者實際上會同時收集各記錄批次來傳送給伺服器。如果啟用壓縮，生產者會壓縮每一個批次，如此即可藉由減少在網絡上傳送資料而提高性能。

如果您嘗試發佈訊息的速度比訊息傳送給伺服器的速度快，則生產者會自動將其緩衝至分批處理要求中。生產者會維護每一個分割區的未傳送記錄的緩衝區。當然，即使分批處理也不能達到所需的速度。
 
還有另一個影響因素。為了防止個體生產者或消費者徹底擊敗叢集，{{site.data.keyword.messagehub}} 會套用傳輸量配額。計算每一個生產者傳送資料的速率，並節流控制嘗試超出其配額的任何生產者。藉由稍稍延遲向生產者傳送回應來套用節流控制。通常，這只是一個自然的約束。
 
總之，當訊息發佈時，其記錄會先寫入生產者的緩衝區中。生產者會在背景中分批處理記錄並將其傳送到伺服器。然後，伺服器會回應生產者，如果生產者發佈得太快，則可能會套用節流控制延遲。如果生產者中的緩衝區已滿載，則生產者的傳送呼叫會被延遲，但最終可能會因異常狀況而失敗。

## 程式碼 Snippet
{: #code_snippets}

這些程式碼 Snippet 的層級非常高，用來說明所涉及的概念。如需完整範例，請參閱 GitHub 中的 {{site.data.keyword.messagehub}} 範例：https://github.com/ibm-messaging/event-streams-samples

若要連接至 {{site.data.keyword.messagehub}}，您需要先建置一組配置內容。所有連接至 {{site.data.keyword.messagehub}} 的連線都使用傳輸層安全 (TLS) 及使用者/密碼鑑別進行安全保護，所以您至少需要這些內容。請將 KAFKA_BROKERS_SASL、USER 及 PASSWORD 取代為您自己的服務認證：

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

若要傳送訊息，您還需要為索引鍵及值指定序列化程序，例如：

```
 props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
 props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");
```

然後，使用 KafkaProducer 來傳送訊息，每一則訊息都由一個 ProducerRecord 表示。完成後，別忘了關閉 KafkaProducer。此程式碼只會傳送訊息，但不等待以查看傳送是否成功。

```
 Producer<String, String> producer = new KafkaProducer<>(props);
 producer.send(new ProducerRecord<String, String>("T1", "key", "value"));
 producer.close();
 ```
 
`send()` 方法非同步，會傳回一個可用來確認其完成的 Future：

```
 Future<RecordMetadata> f = producer.send(new ProducerRecord<String, String>("T1", "key", "value"));
// Do some other stuff
// Now wait for the result of the send
 RecordMetadata rm = f.get();
 long offset = rm.offset; 
```

或者，您也可以在傳送訊息時提供一個回呼：

```
producer.send(new ProducerRecord<String,String>("T1","key","value", new Callback() {
          public void onCompletion(RecordMetadata metadata, Exception exception) {
                     // This is called when the send completes, either successfully or with an exception
          }
});
```

如需相關資訊，請參閱 [Kafka 用戶端的 Javadoc ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://kafka.apache.org/11/javadoc/index.html?overview-summary.html){:new_window}，其提供非常全面的資訊。 

