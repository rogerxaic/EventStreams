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
# Procédure de connexion et d'authentification
{: #mql_connect}

**L'API MQ Light est disponible uniquement dans le cadre du plan Standard.**
<br/>

Pour se connecter au service, une application doit utiliser les informations <code>user</code>, <code>password</code> et <code>mqlight_lookup_url</code> qui figurent dans la [variable d'environnement VCAP_SERVICES](/docs/services/EventStreams/eventstreams127.html). Suivez les recommandations ci-après en fonction du langage choisi :

**Java**

Si vous spécifiez 'null' comme paramètre endpointService de l'appel create(), le client reçoit l'instruction de lire les informations correspondant aux éléments <code>user</code>, <code>password</code> et
<code>mqlight_lookup_url</code> de VCAP_SERVICES :

<pre>
<code>NonBlockingClient.create(null, new NonBlockingClientAdapter<Void>() {
    public void onStarted(NonBlockingClient client, Void context) {
        client.send("my/topic", "Hello World!", null);
    }
}, null);</code>
</pre>
{:codeblock}

<br>

**Node.js**

Procédez à l'extraction des informations <code>user</code>, <code>password</code> et
<code>mqlight_lookup_url</code> de VCAP_SERVICES et employez-les pour créer le client comme suit :

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

**Ruby**

Procédez à l'extraction des informations <code>user</code>, <code>password</code> et
<code>mqlight_lookup_url</code> de VCAP_SERVICES et employez-les pour créer le client comme suit :
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

**Python**

Procédez à l'extraction des informations <code>user</code>, <code>password</code> et
<code>mqlight_lookup_url</code> de VCAP_SERVICES et employez-les pour créer le client comme suit :
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

Pour plus d'informations sur les API {{site.data.keyword.mql}}, voir [{{site.data.keyword.mql}} sur le site developerWorks&reg; ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://developer.ibm.com/messaging/mq-light/){:new_window}.
