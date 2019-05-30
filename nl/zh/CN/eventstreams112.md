---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-15"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}

# 生成消息
{: #producing_messages }


生产者是用于将消息流发布到 Kafka 主题的应用程序。此处的信息主要针对 Java 编程接口；Java 编程接口是 Apache Kafka 项目的一部分。这些概念也适用于其他语言，但名称有时会略有不同。
{: shortdesc}

在编程接口中，消息实际上称为记录。例如，从生产者 API 的角度来看，Java 类 org.apache.kafka.clients.producer.ProducerRecord 用于表示消息。术语_记录_和_消息_可以互换使用，但基本上会用记录来表示消息。

生产者连接到 Kafka 时，会建立初始引导程序连接。此连接可以是与集群中任一服务器的连接。生产者要将消息发布到主题时，会请求该主题的分区和领导权信息。接下来，生产者会与分区领导者另外建立一个连接，然后可以开始发布消息。生产者连接到 Kafka 集群时，这些操作会在内部自动执行。
 
消息发送到分区领导者时，该消息不会立即可供使用者使用。领导者会将消息的记录附加到分区，并为其分配该分区的下一个偏移量数字。等到同步副本的所有追随者都已复制记录并确认已将记录写入自己的副本后，记录就会变为*已落实*状态，可供使用者使用。

每条消息表示为一条记录，由两部分组成：键和值。键通常用于表示消息相关数据，值是消息的主体。由于 Kafka 生态系统中的许多工具（例如，连接其他系统的连接器）只使用值而忽略键，因此最好将所有消息数据都放入值中，并仅在分区或记录压缩时使用键。不应该为了使用键而依赖从 Kafka 读取的所有内容。

其他许多消息传递系统也可以随消息一起传递其他信息。针对此用途，Kafka 0.11 引入了记录头。

您可能会发现，阅读这些信息以及在 {{site.data.keyword.messagehub}} 中[使用消息](/docs/services/EventStreams?topic=eventstreams-consuming_messages)会非常有用。

## 配置设置
{: #config_settings}

有许多针对生产者的配置设置。您可以控制生产者的各个方面，包括批处理、重试和消息确认。下面是最重要的方面：

|名称|描述                                                         |有效值|缺省值|
|----------|---------|----------|---------|
|key.serializer|用于对键进行序列化的类。|用于实现序列化器接口的 Java 类，例如 org.apache.kafka.common.serialization.StringSerializer|无缺省值 - 必须指定值|
|value.serializer|用于对值进行序列化的类。|用于实现序列化器接口的 Java 类，例如 org.apache.kafka.common.serialization.StringSerializer|无缺省值 - 必须指定值|
|acks|确认发布的每条消息所需的服务器数。这可控制生产者需要的耐久性保证。|0，1，all（或 -1）|1|
|retries|发送遇到错误时，客户机重新发送消息的次数。|0，...|0|
|max.block.ms|发送或元数据请求可以阻止等待的毫秒数。|0，...|60000（1 分钟）|
|max.in.flight.requests.per.connection|当客户机发送的连接请求中未被确认的请求数达到此最大值时，就会阻止进一步的请求。|1，...|5|
|request.timeout.ms|生产者等待对请求的响应的最长时间量。在此超时到期之前，如果没有收到响应，将会重试请求，或者如果达到了重试次数，请求会失败。|0，...|30000（30 秒）|

还有更多配置设置可用，但在试用之前，请务必认真阅读整个 [Apache Kafka 文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://kafka.apache.org/documentation/){:new_window}。

## 分区
{: #partitioning}

生产者在主题上发布消息时，生产者可以选择使用哪个分区。如果排序很重要，请务必记住，分区是有顺序的记录序列，但一个主题可包含一个或多个分区。如果要按顺序传递一组消息，请确保这些消息全部传递到同一分区上。实现这一操作的最直接方法是为所有这些消息提供相同的键。 
 
生产者发布消息时，可以明确指定分区号。这样可以直接控制，但也会使生产者代码更加复杂，因为它将承担管理分区选择的责任。有关更多信息，请参阅方法调用 Producer.partitionsFor。例如，针对 [Kafka 2.2.0 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://kafka.apache.org/22/javadoc/org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} 描述了该调用。
 
如果生产者未指定分区号，那么分区的选择由分区程序执行。Kafka 生产者中内置缺省分区程序的工作方式如下：

* 如果记录没有键，将以循环方式选择分区。
* 如果记录有键，将通过计算键的散列值来选择分区。这相当于为具有相同键的所有消息选择同一分区。

您还可以编写自己的定制分区程序。定制分区程序可以选择用于将记录分配给分区的任何方案。例如，仅使用键中的信息子集或特定于应用程序的标识。


## 消息排序
{: #message_ordering}

Kafka 通常会按生产者发送消息的顺序写入消息。但是，有些情况下重试可能会导致消息重复或重新排序。如果要按顺序发送消息序列，那么确保这些消息全部写入同一分区非常重要。
 
生产者也可以自动重试发送消息。通常最好启用此重试功能，因为另一个选择是应用程序代码必须自行执行任何重试。将 Kafka 中的批处理和自动重试结合使用可能会导致消息的重复和重新排序。
 
例如，如果在主题上发布了包含三条消息 &lt;M1, M2, M3&gt; 的序列。这些记录可能全部位于同一批次中，所以实际上会将其一起发送给分区领导者。然后领导者将这些记录写入分区，并将其复制为单独的记录。发生故障时，M1 和 M2 可能已添加到分区，但 M3 还没添加。生产者没有收到确认，因此重试发送 &lt;M1, M2, M3&gt;。新领导者简单地将 M1、M2 和 M3 写入分区，该分区现在包含 &lt;M1, M2, M1, M2, M3&gt;，其中重复的 M1 实际上跟随的是原始 M2。如果将每个代理程序中的动态请求数限制为只有一个，就可以防止这种重新排序操作。您可能仍然会发现单个记录是重复的，例如 &lt;M1, M2, M2, M3&gt;，但是永远不会出现序列排序混乱。在 Kafka 0.11 或更高版本中，还可以使用幂等生产者功能来防止 M2 重复。
 
Kafka 通常的做法是编写应用程序来处理偶然出现的消息重复，因为只有一个动态请求对性能的影响十分显著。

## 消息确认
{: #message_acknowledgments}

发布消息时，可以使用 `acks` 生产者配置来选择所需的确认级别。该选择代表吞吐量和可靠性之间的平衡。有如下三个级别：

<dl>
<dt>acks=0（最不可靠）</dt>
<dd>消息写入网络后，即立即被视为已发送。无需分区领导者的确认。因此，如果分区领导权更改，消息可能会丢失。这种确认级别的速度非常快，但在某些情况下可能会发生消息丢失。</dd>
<dt>acks=1（缺省值）</dt>
<dd>只要分区领导者成功将消息记录写入分区，就会向生产者确认。因为确认发生时还不知道记录是否已经到达同步副本，所以如果领导者失败，而追随者还未获得该消息，那么消息可能会丢失。如果分区领导者更改，旧领导者会通知生产者，后者可以处理错误，然后重试将该消息发送给新领导者。由于消息是在所有副本确认收到该消息之前进行确认，所以如果分区领导者更改，已经确认但尚未完全复制的消息可能会丢失。</dd>
<dt>acks=all（最可靠）</dt>
<dd>分区领导者成功写入消息记录，并且所有同步副本已执行相同操作后，会向生产者确认。如果分区领导者更改，那么只要至少有一个同步副本可用，消息就不会丢失。</dd>
</dl>

即便不等待向生产者确认消息，消息也仍然在落实后才可使用，这意味着复制到同步副本完成。换句话说，从生产者角度来看，发送消息的等待时间短于从生产者发送消息到使用者接收消息之间所测得的端到端等待时间。

如果可能，请避免要等待一条消息的确认才能发布下一条消息的情况。因为这种等待会让生产者无法将消息一起批处理，并且还会使消息发布速率降低到低于网络的来回延迟时间。

## 批处理、调速和压缩
{: #batching}

为了提高效率，生产者实际上会将多批记录收集在一起发送给服务器。如果启用压缩，生产者会压缩每个批次，这可以减少需要通过网络传输的数据，从而提高性能。

如果尝试使用的消息发布速度比向服务器发送消息的速度快，那么生产者会自动将消息缓存到批处理请求中。生产者会为每个分区维护一个缓冲区，用于缓存未发送的记录。当然，存在一个临界点，达到此临界点之后，即使批处理也不允许实现您想要的速率。
 
另外还有一个影响因素。为了防止单个生产者或使用者大量涌入集群而使集群应接不暇，{{site.data.keyword.messagehub}} 应用了吞吐量配额。程序会计算每个生产者发送数据的速率，并对试图超过其配额的任何生产者进行调速。应用调速的方式是略微延迟向生产者发送响应。通常，此行为就像是一个天然制动器。

有关吞吐量指导信息，请参阅[限制和配额](/docs/services/EventStreams?topic=eventstreams-kafka_quotas#kafka_quotas)。 
 
总之，发布消息时，其记录会首先写入生产者中的缓冲区。生产者在后台对记录分批后，将记录发送到服务器。然后服务器会响应生产者，如果生产者发布速度太快，可能会应用调速延迟。如果生产者中的缓冲区已满，将延迟生产者的 send 调用，但最终可能会失败并返回异常。

## 代码片段
{: #code_snippets}

以下代码片段在很高的级别展示了所涉及的概念。有关完整示例，请参阅 [GitHub ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-messaging/event-streams-samples) 中的 {{site.data.keyword.messagehub}} 样本。

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

要发送消息，您还需要为键和值指定序列化器，例如：

```
 props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
 props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");
```

然后，使用 KafkaProducer 来发送消息，其中每条消息由一个 ProducerRecord 表示。完成后别忘了关闭 KafkaProducer。以下代码仅发送消息，而不会等待以确认发送是否成功。

```
 Producer<String, String> producer = new KafkaProducer<>(props);
 producer.send(new ProducerRecord<String, String>("T1", "key", "value"));
 producer.close();
 ```
 
`send()` 方法是异步的，并会返回 Future，可用于检查其完成情况：

```
 Future<RecordMetadata> f = producer.send(new ProducerRecord<String, String>("T1", "key", "value"));
// 执行其他一些操作
// 现在等待 send 的结果
 RecordMetadata rm = f.get();
 long offset = rm.offset;
```

或者，可以在发送消息时提供回调：

```
producer.send(new ProducerRecord<String,String>("T1","key","value", new Callback() {
          public void onCompletion(RecordMetadata metadata, Exception exception) {
                     // 发送完成会调用此项，结果为成功或返回异常
          }
});
```

有关更多信息，请参阅非常全面的 [Kafka 客户机的 Javadoc ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://kafka.apache.org/11/javadoc/index.html?overview-summary.html){:new_window}。 

