---

copyright:
  years: 2015, 2019
lastupdated: "2018-05-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

<!-- 12/11/18: info moved to eventstreams075.md, moved because of doc app changes -->
# Come effettuare la connessione e l'autenticazione
{: #mql_connect}

** L'API MQ Light è disponibile solo come parte del piano Standard.**
<br/>

Per collegare un'applicazione al servizio, l'applicazione deve utilizzare i dettagli <code>user</code>,
<code>password</code> e <code>mqlight_lookup_url</code> dalla [variabile di ambiente VCAP_SERVICES](/docs/services/EventStreams/eventstreams127.html). Utilizza le seguenti indicazioni per il linguaggio che hai scelto:
{: shortdesc}

**Per Java**

Se specifichi ‘null’ come parametro endpointService della chiamata create(), questo indica al
client di leggere i dettagli <code>user</code>, <code>password</code> e
<code>mqlight_lookup_url</code> da VCAP_SERVICES:

<pre>
<code>NonBlockingClient.create(null, new NonBlockingClientAdapter<Void>() {
    public void onStarted(NonBlockingClient client, Void context) {
        client.send("my/topic", "Hello World!", null);
    }
}, null);</code>
</pre>
{:codeblock}

<br>

**Per Node.js**

Richiama i dettagli <code>user</code>, <code>password</code> e
<code>mqlight_lookup_url</code> da VCAP_SERVICES e usali per creare il client
nel seguente modo:

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

**Per Ruby**

Richiama i dettagli <code>user</code>, <code>password</code> e
<code>mqlight_lookup_url</code> da VCAP_SERVICES e usali per creare il client
nel seguente modo:
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

**Per Python**

Richiama i dettagli <code>user</code>, <code>password</code> e
<code>mqlight_lookup_url</code> da VCAP_SERVICES e usali per creare il client
nel seguente modo:
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

Per ulteriori informazioni sulle API {{site.data.keyword.mql}},
consulta il [sito di {{site.data.keyword.mql}} developerWorks&reg; ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/messaging/mq-light/){:new_window}.
