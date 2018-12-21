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
# 如何连接和认证
{: #mql_connect}

**MQ Light API 仅在标准套餐中提供。**
<br/>

要将应用程序连接到服务，应用程序必须使用 [VCAP_SERVICES 环境变量](/docs/services/EventStreams/eventstreams127.html)中的 <code>user</code>、
<code>password</code> 和 <code>mqlight_lookup_url</code> 详细信息。使用与所选语言对应的以下指导信息：


**对于 Java**

如果将“null”指定为 create() 调用的 endpointService 参数，这将指示客户机从 VCAP_SERVICES 读取 <code>user</code>、<code>password</code> 和 
<code>mqlight_lookup_url</code> 详细信息：



<pre>
<code>NonBlockingClient.create(null, new NonBlockingClientAdapter<Void>() {
    public void onStarted(NonBlockingClient client, Void context) {
        client.send("my/topic", "Hello World!", null);
    }
}, null);</code>
</pre>
{:codeblock}

<br>

**对于 Node.js**

在 VCAP_SERVICES 中检索 <code>user</code>、 <code>password</code> 和 
<code>mqlight_lookup_url</code> 详细信息，并将其用于创建客户机，如下所示：



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

**对于 Ruby**

在 VCAP_SERVICES 中检索 <code>user</code>、 <code>password</code> 和 
<code>mqlight_lookup_url</code> 详细信息，并将其用于创建客户机，如下所示：
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

**对于 Python**

在 VCAP_SERVICES 中检索 <code>user</code>、 <code>password</code> 和 
<code>mqlight_lookup_url</code> 详细信息，并将其用于创建客户机，如下所示：
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

有关 {{site.data.keyword.mql}} API 的更多信息，请参阅 [{{site.data.keyword.mql}} developerWorks&reg; 站点 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/messaging/mq-light/){:new_window}。
