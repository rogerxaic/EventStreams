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

# Uso de la API REST de Kafka
{: #rest_using}

**La API REST de Kafka solo está disponible como parte del plan Estándar.**
<br/>

La API REST de Kafka proporciona una interfaz RESTful a
un clúster Kafka. Puede producir y consumir mensajes mediante la API. Para obtener más información, incluida la documentación de referencia de la API, consulte [Documentos de Proxy REST Kafka ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.confluent.io/2.0.0/kafka-rest/docs/index.html){:new_window}. Para solicitudes y respuestas en {{site.data.keyword.messagehub}}, solo se admite el
formato compacto binario. No se da soporte a los formatos incorporado Avro y JSON.

Si utiliza CURL, puede utilizar un ejemplo como el siguiente para generar:
<pre class="pre"><code>
curl -X POST -H "X-Auth-Token:<var class="keyword varname">APIKEY</var>" -H "Content-Type: application/vnd.kafka.binary.v1+json" <var class="keyword varname">kafka-rest endpoint</var>/topics/<var class="keyword varname">nombre tema</var> -d 

'
{
  "records": [
    {
      "value": "<var class="keyword varname">Serie de valor codificado en base 64</var>"
    }
  ]
}
'
</code></pre>
{: codeblock}
<br/>
<br/>
Si utiliza CURL, puede utilizar un ejemplo como el siguiente para consumir:
<pre class="pre"><code>
curl -X GET -H "X-Auth-Token:<var class="keyword varname">APIKEY</var>" -H "Accept: application/vnd.kafka.binary.v1+json" <var class="keyword varname">kafka-rest endpoint</var>/topics/<var class="keyword varname">nombre tema</var>/partitions/<var class="keyword varname">partition ID</var>/messages?offset=<var class="keyword varname">desplazamiento desde el que empezar</var>
</code></pre>
{: codeblock}

<br/>
<br/>
Para CURL, también puede adaptar los ejemplos de código detallados en [Confluent docs ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://docs.confluent.io/2.0.0/){:new_window} añadiendo la siguiente línea a la línea de mandatos:
<pre class="pre">-H "X-Auth-Token: <var class="keyword varname">APIKEY</var>"</pre>
{: codeblock}

<br/>
## Cómo conectarse y autenticarse
{: #rest_connect}

<!-- info was in eventstreams066.md -->

<!-- Comment from Andrew
basic introduction, definitely including health warning
-->
Para conectarse a {{site.data.keyword.messagehub}}, la API REST de Kafka utiliza las credenciales <code>api_key</code> y <code>kafka_rest_url</code> de la variable de entorno [VCAP_SERVICES](/docs/services/EventStreams/eventstreams127.html).

Para autenticarse con la API REST de Kafka {{site.data.keyword.messagehub}}, debe
especificar <code>api_key</code> en el encabezado X-Auth-Token de sus solicitudes.


## Cómo utilizar la API
{: #rest_how}

<!-- info was in eventstreams097.md -->

La API REST de Kafka de
ejemplo
de
{{site.data.keyword.messagehub}}
es una aplicación Node.js que se conecta a
{{site.data.keyword.messagehub}}
a través de la API REST de Kafka para generar y consumir
mensajes.

El código de ejemplo está en el [proyecto github event-streams-samples ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample){:new_window}.

Siga las instrucciones del archivo [README.md ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample){:new_window} del proyecto para crear y ejecutar el ejemplo.


