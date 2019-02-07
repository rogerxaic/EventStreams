---

copyright:
  years: 2015, 2019
lastupdated: "2018-02-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# {{site.data.keyword.messagehub}} での Kafka コンソール・ツールの使用
{: #kafka_console_tools }

Apache Kafka には、管理とメッセージングを簡単に操作するための各種のコンソール・ツールが付属しています。 それらの多くを {{site.data.keyword.messagehub}} で使用できますが、{{site.data.keyword.messagehub}} ではその ZooKeeper クラスターとの接続は許可されません。 Kafka の開発が進むにつれ、以前は ZooKeeper への接続が必要であったツールの多くで、その要件がなくなりました。
{: shortdesc}

これらのコンソール・ツールは、Kafka ダウンロードの <code>bin</code> ディレクトリーにあります。 例えば、[Apache Kafka 0.10.2.X クライアント ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.apache.org/dyn/closer.cgi?path=/kafka/0.10.2.1/kafka_2.11-0.10.2.1.tgz){:new_window} です。

それらのツールに SASL 資格情報を提供するには、以下の例に基づいてプロパティー・ファイルを作成します。

<pre>
<code>
  sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  security.protocol=SASL_SSL
  sasl.mechanism=PLAIN
  ssl.protocol=TLSv1.2
  ssl.enabled.protocols=TLSv1.2
  ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

USER と PASSWORD を、{{site.data.keyword.Bluemix_notm}} コンソール内の {{site.data.keyword.messagehub}}**「サービス資格情報」**タブにある値に置き換えます。


## コンソール・プロデューサー
{: #console_producer }

Kafka コンソール・プロデューサー・ツールを {{site.data.keyword.messagehub}} で使用できます。 ブローカーのリストと SASL 資格情報を指定する必要があります。

既に説明した方法でプロパティー・ファイルを作成した後に、以下のように端末でコンソール・プロデューサーを実行できます。

<pre>
<code>
  $ kafka-console-producer.sh --broker-list KAFKA_BROKERS_SASL --producer.config CONFIG_FILE --topic TOPIC_NAME
</code>
</pre>
{:codeblock}

例にある以下の変数を独自の値に置き換えます。
* KAFKA_BROKERS_SASL を、コンマ区切りの「ホスト:ポート」のペアのリスト (例えば `host1:port1,host2:port2`) として、{{site.data.keyword.Bluemix_notm}} コンソール内の {{site.data.keyword.messagehub}}**「サービス資格情報」**タブにある値に置き換えます。 
* CONFIG_FILE を構成ファイルのパスに置き換えます。 

このツールにある他の多くのオプションを使用できますが、ZooKeeper へのアクセス権限が必要なものは使用できません。


## コンソール・コンシューマー
{: #console_consumer }

Kafka コンソール・コンシューマー・ツールを {{site.data.keyword.messagehub}} で使用できます。 ブートストラップ・サーバーと SASL 資格情報を指定する必要があります。

既に説明した方法でプロパティー・ファイルを作成した後に、以下のように端末でコンソール・コンシューマーを実行できます。

<pre>
<code>
  $ kafka-console-consumer.sh --bootstrap-server KAFKA_BROKERS_SASL --consumer.config CONFIG_FILE --topic TOPIC_NAME 
</code>
</pre>
{:codeblock}

例にある以下の変数を独自の値に置き換えます。
* KAFKA_BROKERS_SASL を、コンマ区切りの「ホスト:ポート」のペアのリスト (例えば `host1:port1,host2:port2`) として、{{site.data.keyword.Bluemix_notm}} コンソール内の {{site.data.keyword.messagehub}}**「サービス資格情報」**タブにある値に置き換えます。 
* CONFIG_FILE を構成ファイルのパスに置き換えます。 

このツールにある他の多くのオプションを使用できますが、ZooKeeper へのアクセス権限が必要なものは使用できません。


## コンシューマー・グループ
{: #consumer_groups_tool }

Kafka コンシューマー・グループ・ツールを {{site.data.keyword.messagehub}} で使用できます。 {{site.data.keyword.messagehub}} ではその ZooKeeper クラスターとの接続は許可されないので、一部のオプションは使用できません。

既に説明した方法でプロパティー・ファイルを作成した後に、端末でコンシューマー・グループ・ツールを実行できます。 例えば、以下のようにコンシューマー・グループをリストできます。

<pre>
<code>
  $ kafka-consumer-groups.sh --bootstrap-server KAFKA_BROKERS_SASL --command-config CONFIG_FILE --list
</code>
</pre>
{:codeblock}

例にある以下の変数を独自の値に置き換えます。
* KAFKA_BROKERS_SASL を、コンマ区切りの「ホスト:ポート」のペアのリスト (例えば `host1:port1,host2:port2`) として、{{site.data.keyword.Bluemix_notm}} コンソール内の {{site.data.keyword.messagehub}}**「サービス資格情報」**タブにある値に置き換えます。 
* CONFIG_FILE を構成ファイルのパスに置き換えます。

このツールを使用して、コンシューマーの現在位置、それらのラグ、グループの各パーティションのクライアント ID などの詳細を表示することもできます。 以下に例を示します。

<pre>
<code>
  $ kafka-consumer-groups.sh --bootstrap-server KAFKA_BROKERS_SASL --command-config CONFIG_FILE --describe --group GROUP
</code>
</pre>
{:codeblock}

例にある GROUP を、詳細情報の検索対象となるグループ名に置き換えます。 


## トピック
{: #topics_tool }

Kafka トピック・ツール `kafka-topics` は、ZooKeeper へのアクセス権限を必要とするので、{{site.data.keyword.messagehub}} で使用することはできません。

ただし、{{site.data.keyword.Bluemix_notm}} コンソール内の {{site.data.keyword.messagehub}} ダッシュボードを使用して、または REST API を使用して、トピックを管理することができます。


## Kafka Streams リセット
{: #kafka_streams_reset }

このツールを {{site.data.keyword.messagehub}} で使用して、Kafka Streams アプリケーションの処理状態をリセットし、その入力を最初から再処理することができます。 このツールを実行する前に、Streams アプリケーションが完全に停止していることを確認してください。

以下に例を示します。

<pre>
<code>
  $ kafka-streams-application-reset.sh --bootstrap-servers KAFKA_BROKERS_SASL --config-file CONFIG_FILE --application-id APP_ID
</code>
</pre>
{:codeblock}

例にある以下の変数を独自の値に置き換えます。
* KAFKA_BROKERS_SASL を、コンマ区切りの「ホスト:ポート」のペアのリスト (`host1:port1,host2:port2` など) として、{{site.data.keyword.Bluemix_notm}} コンソール内の {{site.data.keyword.messagehub}}**「サービス資格情報」**タブにある値に置き換えます。 
* CONFIG_FILE を構成ファイルのパスに置き換えます。 
* APP_ID を Streams アプリケーション ID に置き換えます。

