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


# 取用訊息
{: #consuming_messages }

消費者是一個取用 Kafka 主題之訊息串流的應用程式。消費者可以訂閱一個以上的主題或分割區。此資訊主要討論作為 Apache Kafka 專案一部分的 Java 程式設計介面。這些概念也套用於其他語言，但名稱有時會稍有不同。
{: shortdesc}

當消費者連接至 Kafka 時，即會起始引導連線。此連線可連接至叢集中的任何伺服器。消費者會要求與其想要取用的主題有關的分割區及領導權資訊。然後，消費者會建立另一個連接至分割區領導者的連線，且可以開始取用訊息。當您的消費者連接至 Kafka 叢集時，即會在內部自動發生這些動作。

消費者通常是一個長時間執行的應用程式。消費者藉由定期呼叫 `Consumer.poll(...)` 來要求 Kafka 的訊息。消費者會呼叫 `poll()`，接收訊息批次，及時進行處理，然後再重新呼叫 `poll()`。

當消費者處理訊息時，訊息並不會從其主題中移除。相反地，消費者有數種方式可以選擇，可讓 Kafka 知道哪些訊息已經過處理。此處理程序稱為確定偏移。

在程式設計介面中，訊息實際上被稱為記錄。例如，Java 類別 org.apache.kafka.clients.consumer.ConsumerRecord 用來表示消費者 API 的訊息。可交換使用_記錄_ 及_訊息_ 術語，但基本上記錄是用來表示訊息。

您可能會發現在 {{site.data.keyword.messagehub}} 中使用[產生訊息](/docs/services/EventStreams?topic=eventstreams-producing_messages)一起讀取此資訊會很有用。

## 配置消費者內容 
{: #configuring_consumer_properties }

消費者有多種配置設定，可控制其行為的各個層面。以下是一些最重要的部分：

|名稱     |說明|有效值   |預設值|
|----------|---------|----------|---------|
|key.deserializer     |此類別用來解除索引鍵的序列化。|用於實作解除序列化程式介面的 Java 類別，例如 org.apache.kafka.common.serialization.StringDeserializer  |無預設值 - 您必須指定一個值|
|value.deserializer     |此類別用來解除值的序列化。|用於實作解除序列化程式介面的 Java 類別，例如 org.apache.kafka.common.serialization.StringDeserializer  |無預設值 - 您必須指定一個值|
|group.id |消費者所屬的消費者群組的 ID。|字串|無預設值|
|auto.offset.reset |消費者沒有起始偏移、或現行偏移不再適用於叢集時的行為。|最新、最舊、無 |最新 |
|enable.auto.commit |判斷是否在背景中自動確定消費者的偏移。|true、false |true |
|auto.commit.interval.ms |偏移定期確定之間的毫秒數。|0、...  |5000（5 秒）|
|max.poll.records |呼叫 poll() 時所傳回的記錄數目上限|1、...  |500 |
|session.timeout.ms |必須接收消費者活動訊號以維持消費者群組的消費者成員資格的毫秒數。|6000-300000 |10000（10 秒）|
|max.poll.interval.ms |消費者離開群組之前，輪詢之間的最大時間間隔。|1、...  |300000（5 分鐘）|
| | | | |

有許多配置設定可供使用，但在試驗之前，請先確定您已仔細閱讀 [Apache Kafka 文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://kafka.apache.org/documentation/){:new_window}。

## 消費者群組

_消費者群組_ 是一組合作取用一個以上主題之訊息的消費者。群組中的消費者都使用相同的 `group.id` 配置值。如果您需要有多個消費者來處理工作負載，則可以在同一個消費者群組中執行多個消費者。即使您只需要一個消費者，通常也會指定一個 `group.id` 值。

每一個消費者群組在叢集中都有一個稱為_協調者_ 的伺服器，其負責將分割區指派給群組中的消費者。此責任分佈在叢集中的伺服器上，甚至是負載。每次群組重新平衡時，都可以變更消費者的分割區指派。

當消費者結合消費者群組時，它會探索群組的協調者。然後，消費者會告知協調者它想要結合群組，協調者就會開始重新平衡群組間（包括新成員）的分割區。

當消費者群組中執行下列其中一項變更時，群組就會藉由將分割區指派移轉至群組成員以符合變更來重新平衡：

* 某消費者加入群組
* 某消費者離開群組
* 協調者將某消費者視為不復存在
* 新分割區已新增至現有主題

對於每一個消費者群組，Kafka 都會記住所取用的每一個分割區的已確定偏移。

如果您有一個已重新平衡的消費者群組，請注意，任何離開該群組的消費者的確定都將被拒絕，直到它重新加入群組。在此情況下，消費者需要重新加入群組，在群組中它可能獲指派一個不同於先前所取用的分割區。

## 消費者活動

Kafka 會自動偵測失敗的消費者，以將分割區重新指派給工作中消費者。它使用以下兩種機制來達到此目的：輪詢及活動訊號。

如果 `Consumer.poll(...)` 所傳回的訊息批次很大或處理過程非常耗時，則重新呼叫 `poll()` 之前的延遲可能很顯著或是無法預期。在某些情況下，需要配置較長的最大輪詢間隔，這樣消費者就不會因為訊息處理需要較長的時間而被從其群組中移除。如果這是唯一的機制，則表示偵測失敗消費者所需的時間也會很長。

為了讓消費者活動更容易處理，Kafka 0.10.1 新增了背景活動訊號。群組協調者期望群組成員會定期傳送活動訊號，以表示它們仍處於活動狀態。消費者中執行的背景活動訊號執行緒會定期傳送活動訊號給協調者。如果協調者未在_階段作業逾時值_ 內收到來自群組成員的活動訊號，則協調者會從群組中移除該成員，並開始重新平衡群組。階段作業逾時值可能比最大輪詢間隔短得多，因此即使訊息處理需要很長的時間，偵測失敗消費者所需的時間也可能很短。

您可以使用 `max.poll.interval.ms` 內容配置最大輪詢間隔，使用 `session.timeout.ms` 內容配置階段作業逾時值。通常不需要使用這些設定，除非需要超過 5 分鐘才能處理訊息批次。

## 管理偏移

對於每一個消費者群組，Kafka 都會維護所取用的每一個分割區的已確定偏移。當消費者處理訊息時，並不會將訊息從分割區中移除。相反地，它只是使用一個稱為確定偏移的處理程序來更新其現行偏移。

{{site.data.keyword.messagehub}} 會將已確定的偏移資訊保留 7 天。

### 如果沒有已確定偏移，該怎麼辨？
當消費者啟動並獲指派一個要取用的分割區時，它將從其群組的已確定偏移開始。如果沒有已確定偏移，則消費者會根據 `auto.offset.reset` 內容的設定選擇是要從最舊的還是最新的可用訊息開始，如下所示：

- `latest`（預設值）。您的消費者只接收及取用訂閱後送達的訊息。您的消費者並不知道訂閱之前所傳送的訊息，因此您不應該期望會取用主題中的所有訊息。
- `earliest`。您的消費者會取用從一開始的所有訊息，因為它知道所有已傳送的訊息。

如果消費者在處理訊息之後、但在確定其偏移之前失敗，則已確定偏移訊息不會反映訊息的處理。這表示此訊息將被該群組中的下一個消費者重新處理，以指派分割區。

當已確定偏移儲存在 Kafka 中且消費者已重新啟動時，消費者將從它們前次停止的點繼續。若有已確定偏移，則不使用 `auto.offset.reset` 內容。

### 自動確定偏移

確定偏移最簡單的方式就是讓 Kafka 消費者自動執行。這很簡單，但它提供的控制比手動確定更少。依預設，消費者每 5 秒會自動確定一次偏移。這項預設確定每 5 秒鐘進行一次，不論消費者處理訊息的進度如何。此外，當消費者呼叫 `poll()` 時，這也會導致從前一次呼叫 `poll()` 所傳回的最新偏移被確定（因為它可能已經過處理）。

如果已確定的偏移超過訊息的處理，且發生消費者失敗，則可能有某些訊息未處理。這是因為處理在已確定偏移處重新啟動，該偏移比失敗之前要處理的最後一則訊息晚。因此，如果可靠性比簡單更重要，通常最好手動確定偏移。

### 手動確定偏移

如果 `enable.auto.commit` 設為 `false`，則消費者會手動確定其偏移。它可以採取同步或非同步方式執行。常見的模式是根據定期計時器確定最新處理訊息的偏移。這個模式表示每個訊息至少被處理一次，但已確定的偏移永遠不會超過積極處理中的訊息進度。定期計時器的頻率控制了消費者失敗後可以重新處理的訊息數。當應用程序重新啟動或群組重新平衡時，會從上次儲存的已確定偏移中再次擷取訊息。

已確定偏移是開始繼續處理的訊息的偏移。這通常是最近已處理的訊息*加上一* 的偏移。

### 消費者遲滯

分割區的消費遲滯是最近發佈訊息的偏移與消費者已確定偏移之間的差額。雖然生產率與消費率通常會有自然的變異，但消費率不應該長期低於生產率。

如果觀察到消費者順利處理訊息，但是偶爾會看到跳過一組訊息，這可能表示消費者無法跟上。對於未使用日誌壓縮的主題，可藉由定期刪除舊的日誌區段來管理日誌空間量。如果消費者遠遠落後於已刪除的日誌區段中的取用訊息，它會突然跳轉到下一個日誌區段的開始。如果讓消費者處理所有訊息是很重要的，則此行為會從該消費者的觀點指出訊息流失。

您可以使用 <code>kafka-consumer-groups</code> 工具來查看消費者遲滯。您也可以使用消費者 API 及消費度量值來達到相同的目的。


## 控制訊息取用速度
{: #message_consumption_speed }

如果因為訊息氾濫導致訊息處理出現問題，您可以設定消費者選項來控制訊息取用的速度。使用 `fetch.max.bytes` 及 `max.poll.records` 來控制呼叫 `poll()` 可傳回的資料量。


## 處理消費者重新平衡
當消費者新增至群組中或從群組中移除時，會發生群組重新平衡，且消費者無法取用訊息。這會導致消費者群組中的所有消費者在短期間內無法使用。

您可以使用 ConsumerRebalanceListener 手動確定偏移（如果未使用自動確定），並在通知「在分割區撤銷」回呼時，暫停進一步處理，直到使用「在分割區重新指派」回呼通知成功重平衡為止。


## 程式碼 Snippet
{: #consumer_code_snippets notoc}

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

若要取用訊息，您還需要為索引鍵及值指定解除序列化程序，例如：

```
 props.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
 props.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
```

然後，使用 KafkaConsumer 來取用訊息，每一則訊息都由一個 ConsumerRecord 表示。取用訊息最常見的方式是藉由設定群組 ID 來將消費者放入消費者群組中，然後呼叫 `subscribe()` 以取得主題清單。消費者將獲指派部分分割區來取用，雖然群組中的消費者比主題中的分割區多，但消費者可能未獲指派任何分割區。接下來，請在迴圈中呼叫 `poll()`，接收要處理的訊息批次，每一則訊息都由一個 ConsumerRecord 表示。

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

此消費者迴圈將永遠執行，但可以從另一個呼叫 `Consumer.wakeup()` 的執行緒中斷，以實現整潔關機。

若要手動確定偏移，必須先將 `enable.auto.commit` 配置設為 `false`。然後，使用 `Consumer.commmitSync()` 或 `Consumer.commitAsync()`，定期更新消費者的已確定偏移。為了簡單起見，此範例處理每一個分割區的記錄並分別確定最後的偏移。

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

## 異常狀況處理

任何使用 Kafka 用戶端的健全應用程序都需要處理某些預期狀況下的異常狀況。在某些情況下，不會直接擲出異常狀況，因為部分方法是非同步的，並且使用 `Future` 或回呼來交付其結果。程式碼範例位於 [GitHub ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-messaging/event-streams-samples)，其顯示完整的範例。

以下是您在程式碼中應該處理的異常狀況清單：

### org.apache.kafka.common.errors.WakeupException
由 `Consumer.poll(...)` 擲出，為呼叫 `Consumer.wakeup()` 的結果。這是中斷消費者輪詢迴圈的標準方式。應該退出輪詢迴圈，並呼叫 `Consumer.close()` 以完全中斷連線。
### org.apache.kafka.common.errors.NotLeaderForPartitionException
分割區領導權變更時，因為 `Producer.send(...)` 而擲出。用戶端會自動重新整理其 meta 資料，以尋找最新的領導者資訊。請重試該作業，它應該會成功地使用更新的 meta 資料。
### org.apache.kafka.common.errors.CommitFailedException
發生無法復原的錯誤時，因為 `Consumer.commitSync(...)` 而擲出。在某些情況下，無法簡單地重複作業，因為分割區指派可能已變更，且消費者可能無法再確定其偏移。因為在單一呼叫中與多個分割區搭配使用時 `Consumer.commitSync(...)` 可能會局部成功，所以可針對每一個分割區使用個別的 `Consumer.commitSync(...)` 呼叫來簡化錯誤回復。
### org.apache.kafka.common.errors.TimeoutException
如果 meta 資料無法擷取，由 `Producer.send(...),  Consumer.listTopics()` 擲出。當所要求的確認通知未在 `request.timeout.ms` 內送回，則傳送的回呼（或傳回的 Future）中也會出現異常狀況。用戶端可以重試作業，但重複作業的效果取決於具體的作業。例如，如果重試傳送訊息，則訊息可能會被複製。

