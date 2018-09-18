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

# Como conectar e autenticar
{: #mql_connect}

** A API do MQ Light está disponível como parte somente do plano Standard.**
<br/>

Para conectar um aplicativo ao serviço, o aplicativo deve usar os detalhes de <code>user</code>,
<code>password</code> e <code>mqlight_lookup_url</code> da
[variável de ambiente VCAP_SERVICES](/docs/services/MessageHub/messagehub127.html). Use a
seguinte orientação para sua linguagem escolhida:

**Para Java**

Se você especificar 'null' como o parâmetro endpointService da chamada create(), isso
instruirá o cliente a ler os detalhes de <code>user</code>, <code>password</code> e
<code>mqlight_lookup_url</code> no VCAP_SERVICES:

<pre>
<code>NonBlockingClient.create(null, new NonBlockingClientAdapter<Void>() {
    public void onStarted(NonBlockingClient client, Void context) {
        client.send("my/topic", "Hello World!", null);
    }
}, null);</code>
</pre>
{:codeblock}

<br>

**Para Node.js**

Recupere <code>user</code>, <code>password</code> e
detalhes de <code>mqlight_lookup_url</code> de VCAP_SERVICES; use para criar o cliente conforme a seguir:

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

**Para Ruby**

Recupere <code>user</code>, <code>password</code> e
detalhes de <code>mqlight_lookup_url</code> de VCAP_SERVICES; use para criar o cliente conforme a seguir:
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

**Para Python**

Recupere <code>user</code>, <code>password</code> e
detalhes de <code>mqlight_lookup_url</code> de VCAP_SERVICES; use para criar o cliente conforme a seguir:
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

Para obter mais informações sobre as APIs do {{site.data.keyword.mql}}, consulte: site
[{{site.data.keyword.mql}} developerWorks
&reg; ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/messaging/mq-light/){:new_window}.
