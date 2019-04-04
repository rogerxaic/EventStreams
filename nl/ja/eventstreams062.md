---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-14"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.messagehub}} で使用する Kafka クライアントの選択
{: #kafka_clients}

{{site.data.keyword.messagehub}} で Kafka API を使用するには、以下のいずれかのタイプのクライアントを選択します。

* 公式 Java クライアント。 これは、Apache Kafka で利用できる最新のフィーチャーが含まれているため、最適な選択です。
* [推奨されるサード・パーティーのクライアント](/docs/services/EventStreams?topic=eventstreams-kafka_clients#clients_table)の 1 つ。

どちらのタイプのクライアントについても、常に最新のクライアントのバージョンを選択することをお勧めします。 
{: shortdesc}

## Event Streams に接続するためのクライアントの要件

{{site.data.keyword.messagehub}} に接続するには、クライアントは SASL Plain メカニズムを使用した認証をサポートする必要があり、TLSv1.2 プロトコルの Server Name Indication (SNI) 拡張機能を使用する必要があります。

サポートされる最小の Kafka プロトコルは 0.10 です。

<!--
## Support summary for the official Apache Kafka client (Java)

<table>
    <caption>Table 1. Kafka client support in Standard and Enterprise plans</caption>
      <tr>
	        <th></th>
		    <th>Standard and Enterprise Plans</th>
		    <th></th>
        </tr>
	  		<tr>
			<td>**Kafka version on cluster**</td>
			<td>Kafka 1.1</td>
		</tr>
	  		<tr>
			<td>**Supported client versions**</td>
			<td>Kafka 1.1, or later</td>
		</tr>
			<td>**Authentication requirements**</td>
			<td>Client must support authentication using the SASL Plain mechanism and use the Server Name Indication (SNI) extension to the TLSv1.2 protocol</td>
		</tr>

</table>
-->
	
## サード・パーティーのクライアント
{: #third_party_clients}

公式 Java クライアントを実行できない場合は、[推奨されているサード・パーティーのクライアント](/docs/services/EventStreams?topic=eventstreams-kafka_clients#clients_table)の 1 つを実行することをお勧めします。これらは、{{site.data.keyword.messagehub}} で実証済みのものです。 

クライアント要件の最小セットをサポートする他のサード・パーティーのクライアントが {{site.data.keyword.messagehub}} で動作する場合もあります。 しかし、IBM でテストし、使用した経験があるのは、推奨されるサード・パーティーのクライアントのみです。

## すべての推奨されるクライアントのサポートの概要
{: #client_summary}

<table id="clients_table">
    <caption>表 2. クライアントのサポートの概要</caption>
     <tr>
		    <th id="client" scope="col">クライアント</th>
		    <th id="language" scope="col">言語</th>
			<th id="version" scope="col">推奨されるバージョン</th>
		    <th id="minimum version" scope="col">サポートされる最小バージョン [<sup>1</sup>](/docs/services/EventStreams?topic=eventstreams-kafka_clients#footnote1)</th>
			<th id="sample link" scope="col">サンプルのリンク先</th>
        </tr>
			<tr>
			<td colspan="3">**公式のクライアント**</td>
			</tr>
	  		<tr>
			<td>[Apache Kafka クライアント![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://kafka.apache.org/downloads)</td>
			<td>Java</td>
			<td>最新</td>
			<td>0.10.2 <p> これよりも古いクライアントについて詳しくは、[後方互換性](/docs/services/EventStreams?topic=eventstreams-kafka_clients#compatibility)を参照してください。</p></td>
			<td>[ Java コンソール・サンプル ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-console-sample)<br/>
			[Liberty サンプル ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-liberty-sample)
			</td>
			</tr>
			<tr>
			<td colspan="3">**サード・パーティーのクライアント**</td>
			</tr>
	  		<tr>
			<td>[node-rdkafka ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/Blizzard/node-rdkafka)</td>
			<td>Node.js</td>
			<td>最新</td>
			<td>2.2.2</td>
			<td>[Node.js サンプル ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-python ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/confluentinc/confluent-kafka-python)</td>
			<td>Python</td>
			<td>最新</td>
			<td>0.11.0</td>
			<td>[Kafka Python サンプル ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-python-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-go ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/confluentinc/confluent-kafka-go)</td>
			<td>Golang</td>
			<td>最新</td>
			<td>0.11.0</td>
			<td></td>
		</tr>
		<tr>
			<td>[librdkafka ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/edenhill/librdkafka)</td>
			<td>C または C++</td>
			<td>最新</td>
			<td>0.11.0</td>
			<td></td>
		</tr>

</table>
### 脚注
1. {: #footnote1}このバージョンは、継続的なテストで検証した最も古いバージョンです。 通常、これは過去 12 カ月以内に使用可能な初期バージョンですが、重大な問題が存在することがわかっている場合は、それよりも新しい可能性があります。

## 後方互換性 - 標準プラン
{: #compatibility}

後方互換性を保つために、{{site.data.keyword.messagehub}} 標準プランで Apache Kafka 0.9 Java クライアントを使用できます。 しかし、このクライアントの存続期間のため、使用しないことを強くお勧めします。 このクライアント・バージョンを使用することにした場合は、追加の[ログイン・モジュール ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-0.9/message-hub-login-library)が必要です。

0.11 より前のクライアント・バージョンでは、より新しい Kafka サーバー・バージョンに接続するために必要な追加のプロトコル変換があるため、パフォーマンスが低下する可能性があります。

<!--
## Unsupported clients

The following clients are not supported by {{site.data.keyword.messagehub}}:

### kafka-node
The kafka-node client does not fully support SASL authentication with the PLAIN mechanism so cannot currently be used with {{site.data.keyword.messagehub}}.


### no-kafka 
The no-kafka client does not fully support SASL authentication with the PLAIN mechanism so cannot currently be used with {{site.data.keyword.messagehub}}.

-->

## クライアントの {{site.data.keyword.messagehub}} への接続
{: #connect_client}

{{site.data.keyword.messagehub}} に接続するように Java クライアントを構成する方法については、[クライアントの構成](/docs/services/EventStreams?topic=eventstreams-kafka_connect)を参照してください。












