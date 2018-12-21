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
# 연결 및 인증 방법
{: #mql_connect}

** MQ Light API는 표준 플랜의 일부로만 사용 가능합니다.**
<br/>

앱을 서비스에 연결하려면 앱은 [VCAP_SERVICES 환경 변수](/docs/services/EventStreams/eventstreams127.html)에서 <code>user</code>,
<code>password</code> 및 <code>mqlight_lookup_url</code> 세부사항을 사용해야 합니다. 사용자가 선택한 언어에 대한 다음 안내를 사용하십시오.

**Java의 경우**

create() 호출의 endpointService 매개변수로 'null'을 지정하는 경우,
이는 클라이언트가 <code>user</code>, <code>password</code> 및 
<code>mqlight_lookup_url</code> 세부사항을 VCAP_SERVICES에서 읽도록 지시합니다.

<pre>
<code>NonBlockingClient.create(null, new NonBlockingClientAdapter<Void>() {
    public void onStarted(NonBlockingClient client, Void context) {
        client.send("my/topic", "Hello World!", null);
    }
}, null);</code>
</pre>
{:codeblock}

<br>

**Node.js의 경우**

<code>user</code>, <code>password</code> 및
<code>mqlight_lookup_url</code> 세부사항을 VCAP_SERVICES에서 검색하고 그것을 사용하여 다음과 같이 클라이언트를 작성하십시오.

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

**Ruby의 경우**

<code>user</code>, <code>password</code> 및
<code>mqlight_lookup_url</code> 세부사항을 VCAP_SERVICES에서 검색하고 그것을 사용하여 다음과 같이 클라이언트를 작성하십시오.
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

**Python의 경우**

<code>user</code>, <code>password</code> 및
<code>mqlight_lookup_url</code> 세부사항을 VCAP_SERVICES에서 검색하고 그것을 사용하여 다음과 같이 클라이언트를 작성하십시오.
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

{{site.data.keyword.mql}} API에 대한 자세한 정보는
[{{site.data.keyword.mql}} developerWorks&reg; 사이트 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.ibm.com/messaging/mq-light/){:new_window}을 참조하십시오.
