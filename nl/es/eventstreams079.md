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

# Cómo conectarse y autenticarse
{: #mql_connect}

**La API MQ Light solo está disponible como parte del plan Estándar.**
<br/>

Para conectar una app al servicio, esta debe usar los detalles <code>user</code>,
<code>password</code> y <code>mqlight_lookup_url</code> de la variable de entorno [VCAP_SERVICES](/docs/services/EventStreams/eventstreams127.html). Utilice la siguiente guía para el idioma elegido:

**Para Java**

Si especifica ‘null’ como parámetro endpointService de la llamada create(), se indicará al cliente que lea los detalles de <code>user</code>, <code>password</code> y
<code>mqlight_lookup_url</code> de VCAP_SERVICES:

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

Recupere los detalles de <code>user</code>, <code>password</code> y
<code>mqlight_lookup_url</code> de VCAP_SERVICES y utilícelos para crear el cliente de la siguiente manera:

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

Recupere los detalles de <code>user</code>, <code>password</code> y
<code>mqlight_lookup_url</code> de VCAP_SERVICES y utilícelos para crear el cliente de la siguiente manera:
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

Recupere los detalles de <code>user</code>, <code>password</code> y
<code>mqlight_lookup_url</code> de VCAP_SERVICES y utilícelos para crear el cliente de la siguiente manera:
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

Para obtener más información sobre las API {{site.data.keyword.mql}}, consulte el sitio: [{{site.data.keyword.mql}} developerWorks&reg; ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/messaging/mq-light/){:new_window}.
