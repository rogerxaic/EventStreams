---

copyright:
  years: 2015, 2019
lastupdated: "2017-11-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.messagehub}} での Kafka Connect の使用
{: #kafka_connect }

Kafka Connect を {{site.data.keyword.messagehub}} と共に使用して、{{site.data.keyword.Bluemix_short}} の内外のワーカーを実行することができます。
{: shortdesc}

Kafka Connect は、スタンドアロン・モードまたは分散モードで実行することができます。 スタンドアロン・モードは、テストのため、そしてシステム間の一時的な接続のためのものです。 分散モードは、実動環境での使用に適しています。 これらの 2 つのモードでは、{{site.data.keyword.messagehub}} を使用するために必要な構成が少し異なります。
{:shortdesc}

## スタンドアロン・ワーカーの構成
{: #standalone_worker notoc}

Kafka Connect スタンドアロン・ワーカーを開始するときに提供するワーカー・プロパティー・ファイルに、ブートストラップ・サーバーと SASL 資格情報の情報を指定する必要があります。

スタンドアロン・ワーカーは、内部トピックを使用しません。 代わりにファイルを使用してオフセット情報を保管します。

### ソース・コネクター
{: #source_connector notoc }

以下の例では、プロパティー・ファイルに指定する必要のあるプロパティーがリストされています。

<pre>
<code>
  bootstrap.servers=KAFKA_BROKERS_SASL
  producer.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  producer.security.protocol=SASL_SSL
  producer.sasl.mechanism=PLAIN
  producer.ssl.protocol=TLSv1.2
  producer.ssl.enabled.protocols=TLSv1.2
  producer.ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

KAFKA_BROKERS_SASL、USER、PASSWORD を、{{site.data.keyword.Bluemix_notm}} コンソール内の {{site.data.keyword.messagehub}}**「サービス資格情報」**タブにある値に置き換えます。

### シンク・コネクター
{: #sink_connector }

以下の例では、プロパティー・ファイルに指定する必要のあるプロパティーがリストされています。

<pre>
<code>
  bootstrap.servers=KAFKA_BROKERS_SASL
  consumer.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  consumer.security.protocol=SASL_SSL
  consumer.sasl.mechanism=PLAIN
  consumer.ssl.protocol=TLSv1.2
  consumer.ssl.enabled.protocols=TLSv1.2
  consumer.ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

KAFKA_BROKERS_SASL、USER、PASSWORD を、{{site.data.keyword.Bluemix_notm}} コンソール内の {{site.data.keyword.messagehub}}**「サービス資格情報」**タブにある値に置き換えます。

## 分散ワーカーの構成
{: #distributed_worker notoc}

Kafka Connect 分散ワーカーを開始するときに提供するプロパティー・ファイルに、ブートストラップ・サーバーと SASL 資格情報の情報を指定する必要があります。 以下の例では、プロパティー・ファイルに指定する必要のあるプロパティーがリストされています。

<pre>
<code>
  bootstrap.servers=KAFKA_BROKERS_SASL
  sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  security.protocol=SASL_SSL
  sasl.mechanism=PLAIN
  ssl.protocol=TLSv1.2
  ssl.enabled.protocols=TLSv1.2
  ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

KAFKA_BROKERS_SASL、USER、PASSWORD を、{{site.data.keyword.Bluemix_notm}} コンソール内の {{site.data.keyword.messagehub}}**「サービス資格情報」**タブにある値に置き換えます。

ソース・コネクターを使用する場合は、以下のように、プロデューサーの SSL 構成と SASL 構成も指定する必要があります。

<pre>
<code>
  producer.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  producer.security.protocol=SASL_SSL
  producer.sasl.mechanism=PLAIN
  producer.ssl.protocol=TLSv1.2
  producer.ssl.enabled.protocols=TLSv1.2
  producer.ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

シンク・コネクターを使用する場合は、以下のように、コンシューマーの SSL 構成と SASL 構成も指定する必要があります。

<pre>
<code>
  consumer.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  consumer.security.protocol=SASL_SSL
  consumer.sasl.mechanism=PLAIN
  consumer.ssl.protocol=TLSv1.2
  consumer.ssl.enabled.protocols=TLSv1.2
  consumer.ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

さらに、分散モードの Kafka Connect は、内部的に 3 つのトピックを使用します。 Kafka Connect を Apache Kafka バージョン 0.11 以降で使用する場合、これらのトピックはワーカーの開始時に自動的に作成されます。 トピックの名前は構成パラメーターとして指定します。 これらの値は、`group.id` 構成値が等しいすべてのワーカーで同じになるようにしてください。

| 構成               | 説明                                                         |
| --------------------------- | ------------------------------------------------------------------- |
| `offset.storage.topic`      | コネクター・オフセット・トピック                                             |
| `offset.storage.partitions` | コネクター・オフセット・トピックのパーティション数 (デフォルトは 25) |
| `config.storage.topic`      | コネクター構成トピック                                       |
| `status.storage.topic`      | コネクター状況トピック                                              |
| `status.storage.partitions` | コネクター状況トピックのパーティション数 (デフォルトは 5)          |

例えば、以下のキーと値のペアをプロパティー・ファイルで使用できます。

<pre>
<code>
  offset.storage.topic=connect-offsets
  config.storage.topic=connect-configs
  status.storage.topic=connect-status
</code>
</pre>
{:codeblock}

Kafka Connect を少し使用するだけの場合には、パーティションの数を減らすことを検討してください。



