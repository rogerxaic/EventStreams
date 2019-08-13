---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-04"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}

# Utilización de la API MQ Light en el plan Clásico
{: #mql_using}

**La API MQ Light solo está disponible como parte del plan Clásico.**
<br/>

La API {{site.data.keyword.mql}} se proporciona por motivos de compatibilidad con versiones anteriores del servicio {{site.data.keyword.mql}}. La API proporciona una interfaz de mensajería basada en AMQP para Java&trade;, Node.js, Python y Ruby. 
{:shortdesc}


## ¿Qué es la API MQ Light y cuáles son sus particularidades?
{: #mqlight}

<!-- 30/10/18: info was in eventstreams106.md, moved because of doc app changes -->

La API {{site.data.keyword.mql}} proporciona un nivel superior de abstracción para mensajería que la API Kafka.

La elección entre el uso de un cliente Kafka o la API {{site.data.keyword.mql}} depende de la topología de mensajería que desee crear:

* Con Kafka, se utiliza un número reducido de temas y puede haber varias particiones para cada tema para obtener mayor escalabilidad. Puede compartir mensajes entre consumidores utilizando grupos de consumidores, pero cada consumidor debe poder mantener la tasa de mensajes de las particiones asignadas.
* Con la API {{site.data.keyword.mql}}, puede utilizar un número de temas mucho mayor y los nombres de temas son jerárquicos (por ejemplo: <code>&lsquo;/sports/football&rsquo;</code> y <code>&lsquo;/sports/tiddlywinks&rsquo;</code>). 

Los temas de la API {{site.data.keyword.mql}} no son los mismos que los temas de Kafka. En su lugar, la API {{site.data.keyword.mql}} utiliza un tema único denominado "MQLight Kafka" y todos los mensajes enviados y recibidos utilizando la API {{site.data.keyword.mql}} pasan por ese tema de Kafka concreto.

{{site.data.keyword.mql}} solo está disponible en las siguientes ubicaciones (regiones) de {{site.data.keyword.Bluemix_notm}}: Dallas (us-south), Londres (eu-gb) y Sídney (au-syd). La API MQ Light no está disponible en la ubicación Frankfurt (eu-de) o en {{site.data.keyword.Bluemix_notm}} dedicado.

Para obtener más información sobre qué API elegir, consulte [Elección entre las tres API](/docs/services/EventStreams?topic=eventstreams-choose_api_classic).


## ¿Qué se necesita para utilizar la API MQ Light con {{site.data.keyword.messagehub}}?
{: #mql_reqs}

<!-- 30/10/18: info was in eventstreams098.md, moved because of doc app changes -->

Deben cumplirse los requisitos siguientes para utilizar la API {{site.data.keyword.mql}} con {{site.data.keyword.messagehub}}: 

**Debe crear explícitamente un tema llamado "MQLight Kafka" para poder utilizar la API porque todos los mensajes pasan por el tema "MQLight". Este tema tiene una única partición. La creación de este tema habilita la API MQ Light para la instancia de servicio. Los temas utilizados en la API MQ Light se crean automáticamente a medida que se utilizan, pero todos los mensajes están realmente en el tema de Kafka "MQLight".** 

La API MQ Light utiliza el tema "MQLight" para almacenar sus datos de mensajes e interactuar con otros clientes de Kafka. Tenga en cuenta que, cuando se crea este tema, se incurre en cargos en la tarifa estándar tal como se describe en el plan de pago de servicios.

Para inhabilitar la API MQ Light, suprima el tema "MQLight". Tenga en cuenta que todos los datos se destruyen cuando se suprime el tema.

<!-- 12/11/18: following info was in eventstreams079.md, moved because of doc app changes -->

## Cómo conectarse y autenticarse
{: #mql_connect}

Para conectar una app al servicio, esta debe usar los detalles <code>user</code>,
<code>password</code> y <code>mqlight_lookup_url</code> de la variable de entorno [VCAP_SERVICES](/docs/services/EventStreams?topic=eventstreams-connecting#connect_classic_cf). Utilice la siguiente guía para el idioma elegido:

**Para Java**

Si especifica <code>null</code> como parámetro endpointService de la llamada create(), se indicará al cliente que lea los detalles de <code>user</code>, <code>password</code> y
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

Para obtener más información sobre las API, consulte: [API de {{site.data.keyword.mql}} en GitHub ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/mqlight){:new_window}.


<!-- 14/11/18: info was in eventstreams080.md, moved because of doc app changes -->

## Cómo conectar con aplicaciones MQ Light existentes
{: #mql_exist_apps}


Puede conectar al servicio aplicaciones existentes que actualmente se ejecuten en {{site.data.keyword.IBM_notm}} MQ o en el servicio {{site.data.keyword.mql}} {{site.data.keyword.Bluemix_notm}}. Las apps seguirán funcionando del mismo modo.

Para conectar apps existentes, realice las siguientes comprobaciones:

* Asegúrese de que la app esté utilizando la versión de cliente de API {{site.data.keyword.mql}} para su idioma.
* Compruebe que los detalles de conexión extraídos de VCAP_SERVICES hagan referencia al tipo de servicio <code>messagehub</code> y recupere el nombre de usuario de las conexiones de la propiedad <code>credentials.user</code> en lugar de hacerlo de la propiedad <code>credentials.username</code> y recupere el URL de búsqueda de conexión de la propiedad <code>credentials.mqlight_lookup_url</code> en lugar de recuperarlo de la propiedad <code>credentials.connectionLookupURI</code>. Para obtener más información, consulte [Variable de entorno VCAP_SERVICES](/docs/services/EventStreams?topic=eventstreams-connecting).

	Tenga en cuenta que este paso se realiza en su nombre si se utiliza el cliente Java &trade; y se especifica 'null' como parámetro endpointService en la llamada create(), de modo que el cliente recupera la información por sí mismo.
	
* La app debe soportar las conexiones TLS v1.2. VCAP_SERVICES ya no incluye la propiedad <code>credentials.nonTLSConnectionLookupURI</code> para crear una conexión no TLS.

También debe tener en cuenta la siguiente información:

* Los límites de mensajes son coherentes con {{site.data.keyword.messagehub}}, pero podrían ser diferentes de otros servidores que soportan la API {{site.data.keyword.mql}}. Para obtener más información, consulte [Límites máximos](/docs/services/EventStreams?topic=eventstreams-mql_using#max_limits).
* JMS no está soportado.

<!-- 15/11/18: info was in eventstreams081.md, moved because of doc app changes -->

## Intercambio de mensajes entre la API MQ Light y Kafka o la API REST de Kafka
{: #mql_exchange}

Los mensajes de {{site.data.keyword.mql}} se almacenan en un tema único de Kafka subyacente denominado "MQLight" y se codifican como se detalla en la tabla siguiente. Esta codificación también puede ser utilizada por otros tipos de API, como Kafka o REST Kafka, para intercambiar mensajes con aplicaciones utilizando la API de {{site.data.keyword.mql}}.

### Formato de mensaje de Kafka
{: #kafka_format notoc}

<table border='1'>
<caption>Tabla 1. Formato de mensaje de Kafka</caption>
  <tr>
    <th> Clave </th>
    <th> Valor </th>
  </tr>
  <tr>
    <td> Opcional (no utilizado por la API)
	<p></p>
	</td>
    <td>**1 byte**
	<p>		     Captador de atención de la API MQ Light, que siempre es 0xFA.</p>
    <p><var class="keyword varname">**n**</var> **bytes**</p>
    <p>		    Mensaje codificado AMQP (formateado basándose en el formato de conexión AMQP). </p></td>
  </tr>
</table>


<!-- 15/11/18: info was in eventstreams082.md, moved because of doc app changes -->

## Ejemplos de la API MQ Light
{: #mql_samples}

Si aún no tiene ninguna app, pruebe la API {{site.data.keyword.mql}} con uno de los ejemplos.

La app de ejemplo consta de dos simples apps: un frontal web que envía mensajes a un back-end y un back-end que procesa los mensajes, aprovecha las palabras y, a continuación, devuelve los mensajes al frontal. Este ejemplo muestra cómo puede hacer que las apps se comuniquen entre sí mediante la API {{site.data.keyword.mql}}. También puede utilizar la API {{site.data.keyword.mql}} para realizar la descarga de trabajador; una función clave necesaria para crear apps escalables, ligeramente acopladas y distribuidas.

El código de ejemplo está en el [proyecto GitHub event-streams-samples ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-messaging/event-streams-samples/tree/master/mqlight){:new_window}.


<!-- 15/11/18: info was in eventstreams083.md, moved because of doc app changes -->

## Límites máximos
{: #max_limits}

Se aplican los siguientes límites a la API de {{site.data.keyword.mql}}:

* La cantidad máxima de datos que pueden almacenarse está en consonancia con una única partición de Kafka para su plan de pago (normalmente 1 GB). Si se supera este límite de datos, se eliminarán los mensajes más antiguos de la partición a medida que se envíen mensajes nuevos.
* La cantidad máxima de tiempo que puede almacenarse un mensaje está en consonancia con una única partición de Kafka para su plan de pago (normalmente 24 horas). No se pueden recuperar mensajes con una antigüedad superior a este periodo de tiempo.
* El tamaño máximo de un mensaje (sin incluir cabeceras) es de 1 MB.
* El número máximo de clientes que pueden estar conectados a la vez es de 25.
* El número máximo de destinos que pueden estar activos a la vez es de 25. Un destino activo se define de la siguiente manera:
  - Un destino con un TimeToLive > 0 tanto con cliente conectado actualmente como sin él.
  - Un destino con un TimeToLive = 0 (valor predeterminado) donde un cliente está conectado. 
  <p>Un destino que se comparte entre los clientes cuenta como un destino único.</p>
