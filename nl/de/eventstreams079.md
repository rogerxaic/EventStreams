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
# Vorgehensweise zum Verbinden und Authentifizieren
{: #mql_connect}

** Die MQ Light-API ist nur als Bestandteil des Plans "Standard" verfügbar.**
<br/>

Zum Verbinden einer App mit dem Service muss die App den Benutzer <code>user</code>,
das Kennwort <code>password</code> und die Details für <code>mqlight_lookup_url</code> aus
[Umgebungsvariable VCAP_SERVICES](/docs/services/EventStreams/eventstreams127.html) verwenden. Verwenden Sie die nachfolgende Anleitung für Ihre gewünschte Sprache:
{: shortdesc}

**Für Java**

Wenn Sie 'null' als Parameter 'endpointService' im Aufruf 'create()' angeben, wird der Client angewiesen,
die Details für <code>user</code>, <code>password</code> und
<code>mqlight_lookup_url</code> aus VCAP_SERVICES abzurufen:

<pre>
<code>NonBlockingClient.create(null, new NonBlockingClientAdapter<Void>() {
    public void onStarted(NonBlockingClient client, Void context) {
        client.send("my/topic", "Hello World!", null);
    }
}, null);</code>
</pre>
{:codeblock}

<br>

**Für Node.js**

Rufen Sie die Details für <code>user</code>, <code>password</code> und
<code>mqlight_lookup_url</code> aus VCAP_SERVICES ab und verwenden Sie diese Details
wie nachfolgend angegeben, um den Client zu erstellen:

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

**Für Ruby**

Rufen Sie die Details für <code>user</code>, <code>password</code> und
<code>mqlight_lookup_url</code> aus VCAP_SERVICES ab und verwenden Sie diese Details
wie nachfolgend angegeben, um den Client zu erstellen:
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

**Für Python**

Rufen Sie die Details für <code>user</code>, <code>password</code> und
<code>mqlight_lookup_url</code> aus VCAP_SERVICES ab und verwenden Sie diese Details
wie nachfolgend angegeben, um den Client zu erstellen:
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

Weitere Informationen zu den {{site.data.keyword.mql}}-APIs finden Sie
auf der Website für [{{site.data.keyword.mql}} developerWorks&reg; ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/messaging/mq-light/){:new_window}.
