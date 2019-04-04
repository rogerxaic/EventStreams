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


# 使用消息
{: #consuming_messages }

使用者是使用 Kafka 主题中消息流的应用程序。使用者可以预订一个或多个主题或分区。此处的信息主要针对 Java 编程接口；Java 编程接口是 Apache Kafka 项目的一部分。这些概念也适用于其他语言，但名称有时会略有不同。
{: shortdesc}

使用者连接到 Kafka 时，会建立初始引导程序连接。此连接可以是与集群中任一服务器的连接。使用者要使用主题中的消息时，会请求有关该主题的分区和领导权信息。接下来，使用者会与分区领导者另外建立一个连接，然后可以开始使用消息。使用者连接到 Kafka 集群时，这些操作会在内部自动执行。

使用者通常是一个长时间运行的应用程序。使用者会通过调用 `Consumer.poll(...)` 定期向 Kafka 请求消息。使用者调用 `poll()`，接收一批消息，对其及时进行处理，然后再次调用 `poll()`。

使用者处理消息后，不会从其主题中除去该消息。要让 Kafka 知道使用者已经处理了哪些消息，有很多种方式，使用者可以从中进行选择。此过程称为落实偏移量。

在编程接口中，消息实际上称为记录。例如，对于使用者 API，Java 类 org.apache.kafka.clients.consumer.ConsumerRecord 用于表示消息。术语_记录_和_消息_可以互换使用，但基本上会用记录来表示消息。

您可能会发现，阅读这些信息以及在 {{site.data.keyword.messagehub}} 中[生成消息](/docs/services/EventStreams?topic=eventstreams-producing_messages)会非常有用。

## 配置使用者属性 
{: #configuring_consumer_properties }

有许多针对使用者的配置设置，可以控制其行为的各个方面。下面是其中一些最重要的方面：

|名称|描述                                                         |有效值|缺省值|
|----------|---------|----------|---------|
|key.deserializer|用于对键进行反序列化的类。|用于实现反序列化器接口的 Java 类，例如 org.apache.kafka.common.serialization.StringDeserializer|无缺省值 - 必须指定值|
|value.deserializer|用于对值进行反序列化的类。|用于实现反序列化器接口的 Java 类，例如 org.apache.kafka.common.serialization.StringDeserializer|无缺省值 - 必须指定值|
|group.id|使用者所属的使用者组的标识。|字符串|无缺省值|
|auto.offset.reset|使用者没有初始偏移量或当前偏移量在集群中不再可用时的行为。| latest, earliest, none | latest |
|enable.auto.commit|确定是否在后台自动落实使用者的偏移量。|true，false |true|
|auto.commit.interval.ms|定期落实偏移量的间隔毫秒数。|0，...|5000（5 秒）|
|max.poll.records|poll() 调用中返回的最大记录数|1，...|500|
|session.timeout.ms|毫秒数，在此时间内必须接收到使用者脉动信号才能保持使用者在使用者组中的成员资格。|6000-300000|10000（10 秒）|
|max.poll.interval.ms|使用者离开组之前轮询的最大时间间隔。|1，...|300000（5 分钟）|
| | | | |

还有更多配置设置可用，但在试用之前，请务必认真阅读整个 [Apache Kafka 文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://kafka.apache.org/documentation/){:new_window}。

## 使用者组

_使用者组_是一组使用者，可合作使用一个或多个主题中的消息。组中的使用者全部使用相同的 `group.id` 配置值。如果您需要多个使用者来处理工作负载，可以在同一使用者组中运行多个使用者。即便您只需要一个使用者，通常也要为 `group.id` 指定值。

每个使用者组在集群中都有一个称为_协调者_的服务器，负责将分区分配给组中的使用者。这项责任会分布在集群中的各个服务器上，以均衡负载。每次进行组重新均衡时，为使用者分配的分区都会更改。

使用者加入使用者组时，会发现该组的协调者。接下来，使用者会通知协调者它想要加入该组，随后协调者会开始在包含新成员的组上对分区进行重新均衡。

当使用者组中发生了以下某种更改时，会进行组重新均衡，调整分配给组成员的分区，以适应更改：

* 使用者加入组
* 使用者离开组
* 协调者认为使用者不再处于活动状态
* 向现有主题添加新分区

对于每个使用者组，Kafka 都会记住正在使用的每个分区的已落实偏移量。

如果您拥有的使用者组已重新均衡，请注意，任何已离开该组的使用者的落实都会被拒绝，直到其重新加入该组。在这种情况下，使用者需要重新加入该组，在该组中为其分配的分区可能会与先前使用的不同。

## 使用者活动性

Kafka 会自动检测失败的使用者，以便可以将分区重新分配给有效的使用者。它使用两种机制来实现这一目的：轮询和发送脉动信号。

如果从 `Consumer.poll(...)` 返回的一批消息很大，或者处理过程非常耗时，那么再次调用 `poll()` 之前的延迟时间可能会很长或不可预测。在某些情况下，有必要配置较长的最大轮询时间间隔，这样就不会因为消息处理需要一段时间而将使用者从组中除去。假如只有这一种机制，那就意味着也会花费很长时间来检测失败的使用者。

为了更轻松地处理使用者活动性问题，Kafka 0.10.1 中添加了后台脉动信号发送功能。组协调者期望组成员定期向其发送脉动信号，以指明自己仍然处于活动状态。只要使用者定期向协调者发送脉动信号，后台脉动信号线程就会保持运行。如果协调者在_会话超时_内没有收到来自某个组成员的脉动信号，那么协调者会从组中除去该成员，并开始对组重新均衡。会话超时可能会比最大轮询时间间隔短得多，因此即便消息处理需要很长时间，检测失败的使用者所需的时间也可能很短。

可以使用 `max.poll.interval.ms` 属性来配置最大轮询时间间隔，使用 `session.timeout.ms` 属性来配置会话超时。通常并不需要使用这些设置，除非处理一批消息需要 5 分钟以上的时间。

## 管理偏移量

对于每个使用者组，Kafka 会维护正在使用的每个分区的已落实偏移量。使用者处理消息后，不会从分区中除去该消息。它仅会使用称为“落实偏移量”的进程来更新其当前偏移量。

{{site.data.keyword.messagehub}} 会将已落实偏移量信息保留 7 天。

### 如果没有现有的已落实偏移量怎么办？
当使用者已启动并已为其分配要使用的分区后，它会从所属组的已落实偏移量处开始。如果没有现有的已落实偏移量，使用者可以根据 `auto.offset.reset` 属性的设置，选择从最早可用消息开始或从最新可用消息开始，如下所示：

- `latest`（缺省值）。使用者只会收到也只能使用在其预订后到达的消息。使用者不会知道预订之前发送的消息，因此您不应该期望一个主题中的所有消息都会被使用。
- `earliest`。使用者使用从头开始的所有消息，因为它知道已发送的所有消息。

如果使用者在处理消息之后但在落实其偏移量之前失败，那么已落实偏移量信息不会反映出消息已处理。这意味着该消息会被组中分配有该分区的下一个使用者再次处理。

已落实偏移量保存在 Kafka 中，并且使用者重新启动后，使用者会从其上次停止的点恢复。存在已落实偏移量时，不会使用 `auto.offset.reset` 属性。

### 自动落实偏移量

落实偏移量最简单的方法是使 Kafka 使用者自动执行此操作。这样虽然简单，但相比手动落实，控制力较弱。缺省情况下，使用者每 5 秒自动落实一次偏移量。此缺省落实每 5 秒钟执行一次，与使用者处理消息的进度无关。此外，使用者调用 `poll()` 时，还会导致落实上一次 `poll()` 调用返回的最新偏移量（因为有可能对其进行了处理）。

如果已落实偏移量超过消息处理，并且使用者失败，那么可能会发生部分消息未处理的情况。这是因为处理会在已落实偏移量处重新开始，此点晚于失败之前处理的最后一个消息。为此，如果可靠性比简单更重要，那么通常最好是手动落实偏移量。

### 手动落实偏移量

如果 `enable.auto.commit` 设置为 `false`，那么使用者会手动落实其偏移量。使用者可以手动同步或异步落实其偏移量。常用模式是基于定期计时器落实最新处理的消息的偏移量。此模式意味着每个消息至少处理一次，但是已落实偏移量绝不会超过当前正在处理的消息进度。定期计时器的频率可控制使用者失败后可以重新处理的消息数量。应用程序重新启动或组重新均衡时，会再次从上次保存的已落实偏移量开始检索消息。

已落实偏移量是指从此位置恢复处理的消息的偏移量。此偏移量通常为最近处理的消息*加一*。

### 使用者滞后

分区的使用者滞后是指最近发布的消息的偏移量与使用者的已落实偏移量之间的差异。虽然通常生产速率和使用速率中会有自然的变化，但是在较长的时间段内，使用速率不应该比生产速率慢。

如果您观察到使用者在成功处理消息，但偶尔会看到跳过一组消息，这可能表示使用者无法跟上进度。对于不使用日志压缩的主题，可通过定期删除旧日志段来管理日志空间量。如果使用者远远落后，而导致其使用已删除日志段中的消息，那么使用者会突然向前跳转到下一个日志段的开头。如果使用者有必要处理所有消息，那么从这个使用者的角度来说，此行为指示消息丢失。

可以使用 <code>kafka-consumer-groups</code> 工具来查看使用者滞后。还可以使用使用者 API 和使用者度量值来达到相同目的。


## 控制消息使用速度
{: #message_consumption_speed }

如果由于消息泛滥导致消息处理出现问题，您可以设置使用者选项来控制消息使用速度。使用 `fetch.max.bytes` 和 `max.poll.records` 可控制调用一次 `poll()` 可以返回多少数据。


## 处理使用者重新均衡
将使用者添加到组或从组中除去使用者时，会执行组重新均衡，在此过程中使用者无法使用消息。这会导致使用者组中的所有使用者在短时间内不可用。

可以使用 ConsumerRebalanceListener 在收到“on partition assigned”回调的通知时手动落实偏移量（如果使用的不是自动落实），并且暂停进一步处理，直到收到使用“on partitions revoked”回调成功重新均衡的通知。


## 代码片段
{: #consumer_code_snippets notoc}

以下代码片段在很高的级别展示了所涉及的概念。有关完整示例，请参阅 GitHub 中的 {{site.data.keyword.messagehub}} 样本：https://github.com/ibm-messaging/event-streams-samples。

要连接到 {{site.data.keyword.messagehub}}，首先需要构建一组配置属性。所有到 {{site.data.keyword.messagehub}} 的连接都会使用 TLS 和用户/密码认证来确保安全，所以您至少需要这些属性。请使用您自己的服务凭证替换 KAFKA_BROKERS_SASL、USER 和 PASSWORD：

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

要使用消息，您还需要为键和值指定反序列化器，例如：

```
 props.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
 props.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
```

然后，通过 KafkaConsumer 来使用消息，其中每条消息由一个 ConsumerRecord 表示。使用消息最常用的方法是通过设置组标识，将使用者放入使用者组中，然后调用 `subscribe()` 来获取主题列表。将为使用者分配一些分区来使用，但是如果组中的使用者多于主题中的分区，那么可能不会为使用者分配任何分区。接下来，在一个循环中调用 `poll()`，接收一批要处理的消息，其中每条消息由一个 ConsumerRecord 表示。

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

此使用者循环会永远运行，但是可以通过其他调用 `Consumer.wakeup()` 的线程来中断，以实现正常关闭。

要手动落实偏移量，首先必须将 `enable.auto.commit` 配置设置为 `false`。然后，使用 `Consumer.commmitSync()` 或 `Consumer.commitAsync()` 定期更新使用者的已落实偏移量。为了简单起见，此示例处理每个分区的记录并分别落实最后一个偏移量。

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

## 异常处理

任何使用 Kafka 客户机的稳健应用程序都需要处理某些预期情境的异常。在某些情况下，不会直接抛出异常，因为某些方法是异步的，并且使用 `Future` 或回调来传递其结果。可以在 [GitHub ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-messaging/event-streams-samples) 中找到示例代码，其中显示了完整的示例。

下面是应该在代码中处理的异常列表：

### org.apache.kafka.common.errors.WakeupException
由 `Consumer.poll(...)` 作为调用 `Consumer.wakeup()` 的结果抛出。这是中断使用者轮询循环的标准方法。轮询循环应该会退出，并且应该会调用 `Consumer.close()` 来正常断开连接。
### org.apache.kafka.common.errors.NotLeaderForPartitionException
分区的领导权更改时，作为 `Producer.send(...)` 的结果抛出。客户机会自动刷新其元数据，以查找最新的领导者信息。请重试操作，该操作应该会使用更新后的元数据成功执行。
### org.apache.kafka.common.errors.CommitFailedException
发生不可恢复的错误时，作为 `Consumer.commitSync(...)` 的结果抛出。在某些情况下，无法简单地重复操作，因为分区分配可能已更改，并且使用者可能无法再落实其偏移量。由于 `Consumer.commitSync(...)` 用于单个调用的多个分区时可能只有部分成功，因此对每个分区使用单独的 `Consumer.commitSync(...)` 调用可以简化错误恢复。
### org.apache.kafka.common.errors.TimeoutException
无法检索元数据时，由 `Producer.send(...),  Consumer.listTopics()` 抛出。请求的确认未在 `request.timeout.ms` 内返回时，在 send 回调（或返回的 Future）中也会看到此异常。客户机可以重试操作，但重复操作的结果取决于具体操作。例如，如果重试发送消息，那么消息可能会重复。

