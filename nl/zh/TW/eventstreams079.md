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

# 如何連接及鑑別
{: #mql_connect}

** MQ Light API 只提供於標準方案中。**
<br/>

若要將應用程式連接至服務，應用程式必須使用來自 [VCAP_SERVICES 環境變數](/docs/services/EventStreams/eventstreams127.html)的 <code>user</code>、
<code>password</code> 及 <code>mqlight_lookup_url</code> 詳細資料。請使用適用於您的選擇語言的下列指引：



**針對 Java**

如果您指定 'null' 作為 create() 呼叫的 endpointService 參數，這會指示用戶端從 VCAP_SERVICES 讀取 <code>user</code>、<code>password</code> 及
<code>mqlight_lookup_url</code> 詳細資料：



<pre>
<code>NonBlockingClient.create(null, new NonBlockingClientAdapter<Void>() {
    public void onStarted(NonBlockingClient client, Void context) {
        client.send("my/topic", "Hello World!", null);
    }
}, null);</code>
</pre>
{:codeblock}

<br>

**針對 Node.js**

從 VCAP_SERVICES 擷取 <code>user</code>、<code>password</code> 及
<code>mqlight_lookup_url</code> 詳細資料，然後使用它們建立用戶端，如下所示：



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

**針對 Ruby**

從 VCAP_SERVICES 擷取 <code>user</code>、<code>password</code> 及
<code>mqlight_lookup_url</code> 詳細資料，然後使用它們建立用戶端，如下所示：

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

**針對 Python**

從 VCAP_SERVICES 擷取 <code>user</code>、<code>password</code> 及
<code>mqlight_lookup_url</code> 詳細資料，然後使用它們建立用戶端，如下所示：

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

如需 {{site.data.keyword.mql}} API 的相關資訊，請參閱：[{{site.data.keyword.mql}} developerWorks&reg; 網站 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/messaging/mq-light/){:new_window}。
