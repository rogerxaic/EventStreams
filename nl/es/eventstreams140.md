---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-13"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:faq: data-hd-content-type='faq'}

# Preguntas frecuentes sobre el plan Clásico 
{: #faqs_classic}

Respuestas a preguntas comunes acerca del servicio de {{site.data.keyword.IBM}} {{site.data.keyword.messagehub}} para el plan Clásico.

Para obtener respuestas a las preguntas relacionadas con todos los planes de {{site.data.keyword.messagehub}}, consulte [Preguntas más frecuentes](/docs/services/EventStreams?topic=eventstreams-faqs#faqs).
{: shortdesc}

<!--17/10/17 - Karen: same info duplicated at messagehub104 -->
## ¿Cómo se utilizan las API de Kafka para crear y suprimir temas?
{: #topic_admin_classic}
{: faq}

Si está utilizando un cliente Kafka de la versión 0.11 o posterior, o bien Kafka Streams versión 0.10.2.0 o posterior, puede utilizar la API para crear y suprimir temas. Existen varias restricciones para la configuración permitida al crear temas. Actualmente, solo se pueden modificar los siguientes valores:

<dl>
<dt>cleanup.policy</dt>
<dd>Establezca este valor en <code>delete</code> (predeterminado), <code>compact</code> o <code>delete,compact</code>
<p>**Nota:**
si la política de limpieza es solo <code>compact</code>, se añade automáticamente <code>delete</code>, pero se inhabilita la supresión en función del tiempo. Los mensajes del tema se compactan a 1 GB antes de la supresión.</p>
</dd>

<dt>retention.ms</dt>
<dd>El período de retención predeterminado es de 24 horas. El valor mínimo es de 1 hora y el máximo de 30 días. Especifique este valor como múltiplo de horas.
</dd>
</dl>


## ¿Durante cuánto tiempo establece {{site.data.keyword.messagehub}} la ventana de retención de registros para el tema de desplazamientos de consumidor?
{: #offsets_classic }
{: faq}

{{site.data.keyword.messagehub}} retiene los desplazamientos de consumidor durante 7 días. Esto corresponde al valor de configuración de Kafka offsets.retention.minutes. 

La retención de desplazamiento abarca todo el sistema y no se puede establecer en temas individuales. Todos los grupos de consumidores obtienen solo 7 días de desplazamientos almacenados, aunque utilicen un tema con una retención de registro que se ha aumentado al máximo de 30 días. 

<!--following message retention info duplicted in eventstreams057 and evenstreams108-->

## ¿Cuánto tiempo se conservan los mensajes?
{: #messages_retained_classic}

De forma predeterminada, los mensajes se retienen un máximo de 24 horas
en Kafka y cada partición está limitada a 1 GB. Si se alcanza 1 GB, se descartarán los mensajes más antiguos para mantenerse dentro
del límite.

Puede cambiar el límite de tiempo para la retención de mensajes cuando cree un tema utilizando la interfaz de usuario o la API de administración. El límite de tiempo es un mínimo de una hora y un máximo de 30 días.

Para obtener información sobre las restricciones de los valores permitidos al crear temas utilizando un cliente Kafka o Kafka Streams, consulte [¿Cómo se utilizan las API de Kafka para crear y suprimir temas?](/docs/services/EventStreams?topic=eventstreams-faqs_classic#topic_admin_classic).


## ¿Cuál es el comportamiento de la disponibilidad de {{site.data.keyword.messagehub}}?
{: #availability_classic}
{: faq}

Si escribe apps de {{site.data.keyword.messagehub}}, utilice esta información para comprender cuál es el comportamiento de disponibilidad normal de {{site.data.keyword.messagehub}} y qué se espera que gestionen las apps.

### API
{: #api_availability_classic }

Como parte del funcionamiento normal de {{site.data.keyword.messagehub}}, los nodos de los clústeres de Kafka se reinician ocasionalmente.
En algunos casos, las apps detectarán que el clúster reasigna recursos. Escriba las apps de modo que resistan estos cambios y puedan reconectarse y reintentar operaciones.

### Puentes de {{site.data.keyword.messagehub}} 
{: #bridge_availability_classic }

Escriba sus apps para que tengan en cuenta la posibilidad de reinicio ocasional de los puentes.

## ¿Cuál es el tamaño de mensaje máximo de {{site.data.keyword.messagehub}}? 
{: #max_message_size_classic }
{: faq}

El tamaño de mensaje máximo de {{site.data.keyword.messagehub}} es 1 MB, que es el valor predeterminado de Kafka. 

## ¿Cuáles son los valores de réplica de {{site.data.keyword.messagehub}}? 
{: #replication_classic }
{: faq}

{{site.data.keyword.messagehub}} está configurado para proporcionar una fuerte disponibilidad y duración.
Los siguientes valores de configuración se aplican a todos los temas y no se pueden cambiar:
* replication.factor = 3
* min.insync.replicas = 2

## ¿Cómo funciona la facturación de {{site.data.keyword.messagehub}} en el plan Clásico? 
{: #billing_classic }
{: faq}

{{site.data.keyword.messagehub}} en el plan Clásico toma regularmente muestras de un recuento de temas de un usuario y {{site.data.keyword.Bluemix_notm}} registra el valor de muestra máximo cada día. {{site.data.keyword.messagehub}} factura por el número máximo de particiones simultáneas visto y por la suma de mensajes enviados y recibidos diariamente.

Por ejemplo, si crea y suprime 1 tema 10 veces en un día, se le facturará por un máximo de 1 tema. Sin embargo, si crea 10 temas y los suprime, se le facturará por 0 o por 10 temas en función del momento en que se tome la muestra.

{{site.data.keyword.messagehub}} factura por cada mensaje o bien por cada 64 k. Un mensaje de hasta 64 k cuenta como un mensaje facturable. Los mensajes mayores de 64 k se cuentan como el siguiente número de mensajes facturables: <code><var class="keyword varname">tamaño_mensaje</var> &divide; 64 k</code>.

<!--12/04/18 - Karen: same info duplicated at messagehub057 -->
## ¿Con cuánta frecuencia se reinicia la API REST de Kafka? 
{: #REST_restart_classic }
{: faq}

La API REST de Kafka se reinicia una vez al día durante un breve período de tiempo. 

Durante este período, la API REST de Kafka puede estar disponible. Si esto sucede, se recomienda reintentar la solicitud. Una vez reiniciada la API REST, deberá volver a crear las instancias de consumidor de Kafka. Si este es el caso, la API REST devuelve el siguiente JSON:

```'{"error_code":40403,"message":"Consumer instance not found."}'
```
{:screen}

## ¿Qué diferencias hay entre los distintos planes de {{site.data.keyword.messagehub}}?
{: #plan_compare_classic }
{: faq}

Para encontrar más información sobre los demás planes de {{site.data.keyword.messagehub}}, consulte [Elección del plan](/docs/services/EventStreams?topic=eventstreams-plan_choose).Para obtener información sobre el plan Clásico, consulte [Visión general del plan Clásico](/docs/services/EventStreams?topic=eventstreams-plan_choose_classic#plan_choose_classic).


## ¿Cómo manejo la recuperación tras desastre?
{: #disaster_recovery_classic }
{: faq}

En este momento, es responsabilidad del usuario gestionar su propia recuperación tras desastre de {{site.data.keyword.messagehub}}. Los datos de {{site.data.keyword.messagehub}} se pueden replicar entre una instancia de {{site.data.keyword.messagehub}} en una ubicación (región) y otra instancia en otra ubicación distinta. Sin embargo, el usuario es responsable de proporcionar una instancia remota de {{site.data.keyword.messagehub}} y de gestionar la réplica.

El usuario también es responsable de la copia de seguridad de los datos de carga útil de mensaje. Aunque estos datos se replican entre varios intermediarios de Kafka dentro de un clúster, que protege frente a la mayoría de las anomalías, esta réplica no cubre la anomalía de toda una ubicación. 

{{site.data.keyword.messagehub}} realiza copia de seguridad de los nombres de los temas.















