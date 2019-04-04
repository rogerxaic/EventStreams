---

copyright:
  years: 2015, 2019
lastupdated: "2018-11-20"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# MQ Light API の使用 
{: #mql_using}

**MQ Light API は、標準プランのみの一部として使用可能です。**
<br/>

{{site.data.keyword.mql}} API は、以前の {{site.data.keyword.mql}} サービスとの後方互換性のために提供されています。 API は、Java&trade;、Node.js、Python、Ruby 用に AMQP ベースのメッセージング・インターフェースを提供します。 
{:shortdesc}

<!-- 02/07/18 - removing words to help deprecate MQ Light
In most cases, {{site.data.keyword.messagehub}} is best used with a Kafka client. The {{site.data.keyword.mql}} API is simple to learn but has very limited scalability and does not offer interoperability with other {{site.data.keyword.messagehub}} APIs.
The {{site.data.keyword.mql}} API is available in the following {{site.data.keyword.Bluemix_short}} regions only: US South, United Kingdom, and Sydney. The {{site.data.keyword.mql}} API not available in the Germany region or in {{site.data.keyword.Bluemix_notm}} Dedicated.
-->

## MQ Light API について、および他の API との違い
{: #mqlight}

<!-- 30/10/18: info was in eventstreams106.md, moved because of doc app changes -->

{{site.data.keyword.mql}} API は、Kafka に比べて、より高いレベルでのメッセージングの抽象化を提供します。

Kafka クライアントと {{site.data.keyword.mql}} API のどちらを使用するのかの選択は、構築したいメッセージング・トポロジーに基づきます。

* Kafka を使用する場合は、少数のトピックを使用し、スケーラビリティーを高めるために各トピックに複数のパーティションを含めることが可能です。 コンシューマー・グループを使用することによってコンシューマー間でメッセージを共有できますが、各コンシューマーは、割り当てられたパーティションでメッセージ速度に遅れずに対応できなければなりません。
* {{site.data.keyword.mql}} API を使用する場合は、ずっと多くのトピックを使用でき、トピック名は階層的です (例: <code>&lsquo;/sports/football&rsquo;</code> および <code>&lsquo;/sports/tiddlywinks&rsquo;</code>)。  

{{site.data.keyword.mql}} API のトピックは、Kafka トピックと同じではありません。 そうではなく、{{site.data.keyword.mql}} API は「MQLight」という名前の単一の Kafka トピックを使用し、{{site.data.keyword.mql}} API を使用して送受信されるすべてのメッセージはこの 1 つの Kafka トピックを経由します。

{{site.data.keyword.mql}} は、ダラス (us-south)、ロンドン (eu-gb)、およびシドニー (au-syd) の
{{site.data.keyword.Bluemix_notm}} ロケーション (地域) でのみ使用できます。 MQ Light API は、フランクフルト (eu-de) ロケーションおよび {{site.data.keyword.Bluemix_notm}} 専用では使用不可です。

どの API を使用するのかの選択について詳しくは、[3 つの API からの選択](/docs/services/EventStreams?topic=eventstreams-choose_api)を参照してください。


## MQ Light API を {{site.data.keyword.messagehub}} で使用するために必要なもの
{: #mql_reqs}

<!-- 30/10/18: info was in eventstreams098.md, moved because of doc app changes -->

{{site.data.keyword.messagehub}} で {{site.data.keyword.mql}} API を使用するには、以下のものが必要です。 

**この API を使用するには、その前に「MQLight」という名前の Kafka トピックを明示的に作成する必要があります。これは、すべてのメッセージが「MQLight」トピックを経由するためです。 このトピックのパーティションは 1 つでなければなりません。 このトピックを作成すると、サービス・インスタンスに対して MQ Light API が有効になります。 MQ Light API で使用されるトピックは、ユーザーが使用するのに合わせて自動的に作成されますが、すべてのメッセージは実際には単一の Kafka トピック「MQLight」上にあります。** 

「MQLight」トピックは、MQ Light API によって、メッセージ・データを保管するため、および他の Kafka クライアントと対話するために使用されます。 このトピックが作成されると、サービス支払いプランに概説されている標準レートで課金が発生することに注意してください。

MQ Light API を無効にするには、「MQLight」トピックを削除します。 トピックを削除するとすべてのデータが破棄されることに注意してください。

<!-- 12/11/18: following info was in eventstreams079.md, moved because of doc app changes -->

## 接続および認証の方法
{: #mql_connect}

アプリを当サービスに接続するには、アプリは <code>user</code>、
<code>password</code>、および <code>mqlight_lookup_url</code> 詳細を [VCAP_SERVICES 環境変数](/docs/services/EventStreams?topic=eventstreams-connecting#connect_standard_cf)から使用する必要があります。選択した言語に応じて、以下のガイドを使用してください。

**Java の場合**

create() 呼び出しの endpointService パラメーターとして <code>null</code> を指定する場合、これは、クライアントに <code>user</code>、<code>password</code>、および
<code>mqlight_lookup_url</code> 詳細を VCAP_SERVICES から読み取ることを指示します。

<pre>
<code>NonBlockingClient.create(null, new NonBlockingClientAdapter<Void>() {
    public void onStarted(NonBlockingClient client, Void context) {
        client.send("my/topic", "Hello World!", null);
    }
}, null);</code>
</pre>
{:codeblock}

<br>

**Node.js の場合**

以下に示すように、<code>user</code>、<code>password</code>、および 
<code>mqlight_lookup_url</code> 詳細を VCAP_SERVICES から取り出し、それらを使用してクライアントを作成します。

<pre>
<code>var services = JSON.parse(process.env.VCAP_SERVICES);
mqlightService = services['messagehub'][0];
opts.service = mqlightService.credentials.mqlight_lookup_url;
opts.user = mqlightService.credentials.user;
opts.password = mqlightService.credentials.password;
var mqlightClient = mqlight.createClient(opts, function(err) {
...</code>
</pre>
{:codeblock}

<br>

**Ruby の場合**

以下に示すように、<code>user</code>、<code>password</code>、および 
<code>mqlight_lookup_url</code> 詳細を VCAP_SERVICES から取り出し、それらを使用してクライアントを作成します。
<pre>
<code>vcap_services = JSON.parse(ENV['VCAP_SERVICES'])
conn_details = vcap_services['messagehub']
credentials = conn_details.first['credentials']
service = credentials['mqlight_lookup_url']
opts[:user] = credentials['user']
opts[:password] = credentials['password']
set :client, Mqlight::BlockingClient.new(service, opts)
...</code>
</pre>
{:codeblock}

<br>

**Python の場合**

以下に示すように、<code>user</code>、<code>password</code>、および 
<code>mqlight_lookup_url</code> 詳細を VCAP_SERVICES から取り出し、それらを使用してクライアントを作成します。
<pre>
<code>vcap_services = json.loads(os.environ.get('VCAP_SERVICES'))
conn_details = vcap_services['messagehub'][0]
service = str(conn_details['credentials']['mqlight_lookup_url'])
security_options = {
      'user': str(conn_details['credentials']['user']),
      'password': str(conn_details['credentials']['password'])
}
client = mqlight.Client(service=service, 
                        security_options=security_options,
                        on_started=on_started)</code>
</pre>
{:codeblock}

<br>

{{site.data.keyword.mql}} API について詳しくは、
[{{site.data.keyword.mql}} developerWorks&reg; サイト ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.ibm.com/messaging/mq-light/){:new_window} を参照してください。


<!-- 14/11/18: info was in eventstreams080.md, moved because of doc app changes -->

## 既存 MQ Light アプリケーションへの接続方法
{: #mql_exist_apps}


現在は {{site.data.keyword.IBM_notm}} MQ または {{site.data.keyword.mql}} {{site.data.keyword.Bluemix_notm}} サービスのいずれかに対して実行されている既存のアプリケーションを当サービスに接続できます。 アプリケーションは同じように機能し続けます。

既存のアプリケーションを接続するには、以下を確認してください。

* アプリケーションが、ご使用の言語での最新の使用可能なバージョンの {{site.data.keyword.mql}} API クライアントを使用していることを確認します。
* VCAP_SERVICES から取り出した接続詳細が、<code>messagehub</code> サービス・タイプを参照していること、接続ユーザー名を <code>credentials.username</code> プロパティーではなく <code>credentials.user</code> プロパティーから取り出していること、および、接続ルックアップ URL を <code>credentials.connectionLookupURI</code> プロパティーではなく <code>credentials.mqlight_lookup_url</code> プロパティーから取り出していることを確認します。 詳しくは、[VCAP_SERVICES 環境変数](/docs/services/EventStreams?topic=eventstreams-connecting)を参照してください。

	このステップが行われるのは、Java&trade; クライアントを使用していて、クライアント自体が情報を取り出すように create() 呼び出しの endpointService パラメーターとして「null」を指定しているユーザーのためであることに注意してください。
	
* アプリケーションは TLS v1.2 接続をサポートしている必要があります。 VCAP_SERVICES には、非 TLS 接続を作成するための <code>credentials.nonTLSConnectionLookupURI</code> プロパティーは含まれなくなりました。

以下の点にも注意する必要があります。

* メッセージ限度は {{site.data.keyword.messagehub}} と整合していますが、{{site.data.keyword.mql}} API をサポートする他のサーバーとは異なっている可能性があります。 詳しくは、[最大限度](/docs/services/EventStreams?topic=eventstreams-mql_using#max_limits)を参照してください。
* JMS はサポートされません。

<!-- 15/11/18: info was in eventstreams081.md, moved because of doc app changes -->

## MQ Light API と Kafka API または Kafka REST API との間でのメッセージ交換
{: #mql_exchange}

{{site.data.keyword.mql}} メッセージは、基礎となっている単一の Kafka トピック「MQLight」に保管され、次の表に詳細が示されているようにエンコードされます。 {{site.data.keyword.mql}} API を使用してアプリケーションとメッセージを交換するために、他の API タイプ (Kafka や Kafka REST など) でもこのエンコードを使用できます。

### Kafka メッセージ・フォーマット
{: #kafka_format notoc}

<table border='1'>
<caption>表 1. Kafka メッセージ・フォーマット</caption>
  <tr>
    <th> キー </th>
    <th> 値 </th>
  </tr>
  <tr>
    <td> オプション (API では使用されません)
	<p></p>
	</td>
    <td>**1 バイト**
	<p>		     MQ Light API の目印であり、常に 0xFA です。</p>
    <p><var class="keyword varname">**n**</var> **バイト**</p>
    <p>		    AMQP エンコードされたメッセージ (AMQP ワイヤー・フォーマットに基づいてフォーマットされます)。 </p></td>
  </tr>
</table>


<!-- 15/11/18: info was in eventstreams082.md, moved because of doc app changes -->

## MQ Light API サンプル
{: #mql_samples}

まだアプリケーションがない場合、サンプルの 1 つを使用して {{site.data.keyword.mql}} API を試してみてください。

サンプル・アプリケーションは、実際は 2 つの単純なアプリケーションからなります。
1 つは、メッセージをバックエンドに送信する Web フロントエンドであり、もう 1 つは、メッセージを処理し、語を大文字化してからメッセージをフロントエンドに返すバックエンドです。 このサンプルでは、{{site.data.keyword.mql}} API を使用して、アプリが互いに対話できるようにする方法を示します。 また、{{site.data.keyword.mql}} API を使用してワーカーの負荷を軽減することもできます。
これは、スケーラブルで疎結合な分散型のアプリケーションを構築するために必要な主要な機能の 1 つです。

サンプル・コードは [event-streams-samples GitHub プロジェクト ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-messaging/event-streams-samples/tree/master/mqlight){:new_window} にあります。


<!-- 15/11/18: info was in eventstreams083.md, moved because of doc app changes -->

## 最大限度
{: #max_limits}

{{site.data.keyword.mql}} API には以下の限度が適用されます。

* 保管できるデータの最大量は、ご使用の支払いプランに応じて単一 Kafka パーティションと一貫性のある量になります (通常は 1 GB)。 このデータ限度を超えると、新しいメッセージが送信されたときに、パーティション内の最も古いメッセージが削除されます。
* メッセージが保管される最大時間は、ご使用の支払いプランに応じて単一 Kafka パーティションと一貫性のある時間になります (通常は 24 時間)。 この時間よりも前のメッセージを取り出すことはできません。
* 1 つのメッセージの最大サイズ (ヘッダーを除く) は 1 MB です。
* 一度に接続できるクライアントの最大数は 25 です。
* 一度にアクティブにできる宛先の最大数は 25 です。 アクティブな宛先は次のように定義されます。
  - 現在接続されているクライアントがある場合とない場合の両方とも、TimeToLive > 0 の宛先。
  - クライアントが接続されている、TimeToLive = 0 (デフォルト) の宛先。 
  <p>複数クライアントで共有されている宛先は、単一の宛先としてカウントされます。</p>
