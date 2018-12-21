---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

<!-- 12/11/18: info moved to eventstreams075.md, moved because of doc app changes -->
# 接続および認証の方法
{: #mql_connect}

**MQ Light API は、標準プランのみの一部として使用可能です。**
<br/>

アプリを当サービスに接続するには、アプリは <code>user</code>、
<code>password</code>、および <code>mqlight_lookup_url</code> 詳細を [VCAP_SERVICES 環境変数](/docs/services/EventStreams/eventstreams127.html)から使用する必要があります。 選択した言語に応じて、以下のガイドを使用してください。

**Java の場合**

create() 呼び出しの endpointService パラメーターとして「null」を指定する場合、これは、クライアントに <code>user</code>、<code>password</code>、および
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
