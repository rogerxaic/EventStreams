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

# Preguntas más frecuentes
{: #faqs}

Respuestas a preguntas comunes acerca del servicio {{site.data.keyword.IBM}} {{site.data.keyword.messagehub}}.

Para obtener respuestas a preguntas específicas del plan Clásico, consulte [Preguntas más frecuentes sobre el plan Clásico](/docs/services/EventStreams?topic=eventstreams-faqs_classic).
{: shortdesc}

<!--17/10/17 - Karen: same info duplicated at messagehub104 -->
## ¿Cómo se utilizan las API de Kafka para crear y suprimir temas?
{: #topic_admin}
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

<p>**Nota:**
en el plan de Empresa, puede definir el valor que desee.</p>
</dd>

<dt>retention.bytes</dt>
<dd>El tamaño máximo que puede alcanzar una partición (que consiste en segmentos de registro) hasta que se descartan segmentos de registro antiguos para liberar espacio.

<p>**Nota:** solo plan de Empresa. Defina cualquier valor mayor que 1 MB.</p>
</dd>

<dt>segment.bytes</dt>
<dd>El tamaño del archivo de segmento para el registro.

<p>**Nota:** solo plan de Empresa. Defina cualquier valor mayor que 100 kB.</p>
</dd>

<dt>segment.index.bytes</dt>
<dd>El tamaño del índice que correlaciona desplazamientos con posiciones de archivo. 

<p>**Nota:** solo plan de Empresa. Defina cualquier valor comprendido entre 100 kB y 2 GB.</p>
</dd>

<dt>segment.ms</dt>
<dd>El periodo de tiempo tras el que Kafka impondrá la acumulación del registro aunque el archivo de segmento no esté lleno. 

<p>**Nota:** solo plan de Empresa. Defina cualquier valor comprendido entre 5 minutos y 30 días.</p>
</dd>
</dl>


## ¿Durante cuánto tiempo establece {{site.data.keyword.messagehub}} la ventana de retención de registros para el tema de desplazamientos de consumidor?
{: #offsets }
{: faq}

{{site.data.keyword.messagehub}} retiene los desplazamientos de consumidor durante 7 días. Esto corresponde al valor de configuración de Kafka offsets.retention.minutes. 

La retención de desplazamiento abarca todo el sistema y no se puede establecer en temas individuales. Todos los grupos de consumidores obtienen solo 7 días de desplazamientos almacenados, aunque utilicen un tema con una retención de registro que se ha aumentado al máximo de 30 días. 

Solo puede ver el tema <code>__consumer_offsets</code> interno de Kafka en modalidad de solo lectura. 
Se recomienda encarecidamente que no intente gestionar el tema de ninguna forma. 

<!--following message retention info duplicted in eventstreams057-->

## ¿Cuánto tiempo se conservan los mensajes?
{: #messages_retained}

De forma predeterminada, los mensajes se retienen un máximo de 24 horas
en Kafka y cada partición está limitada a 1 GB. Si se alcanza 1 GB, se descartarán los mensajes más antiguos para mantenerse dentro
del límite.

Puede cambiar el límite de tiempo para la retención de mensajes cuando cree un tema utilizando la interfaz de usuario o la API de administración. El límite de tiempo es un mínimo de una hora y un máximo de 30 días.

Para obtener información sobre las restricciones de los valores permitidos al crear temas utilizando un cliente Kafka o Kafka Streams, consulte [¿Cómo se utilizan las API de Kafka para crear y suprimir temas?](/docs/services/EventStreams?topic=eventstreams-faqs#topic_admin).

## ¿Cuál es el comportamiento de la disponibilidad de {{site.data.keyword.messagehub}}?
{: #availability}
{: faq}

Si escribe apps de {{site.data.keyword.messagehub}}, utilice esta información para comprender cuál es el comportamiento de disponibilidad normal de {{site.data.keyword.messagehub}} y qué se espera que gestionen las apps.

### API
{: #api_availability }

Como parte del funcionamiento normal de {{site.data.keyword.messagehub}}, los nodos de los clústeres de Kafka se reinician ocasionalmente.
En algunos casos, las apps detectarán que el clúster reasigna recursos. Escriba las apps de modo que resistan estos cambios y puedan reconectarse y reintentar operaciones.

## ¿Cuál es el tamaño de mensaje máximo de {{site.data.keyword.messagehub}}? 
{: #max_message_size }
{: faq}

El tamaño de mensaje máximo de {{site.data.keyword.messagehub}} es 1 MB, que es el valor predeterminado de Kafka. 

## ¿Cuáles son los valores de réplica de {{site.data.keyword.messagehub}}? 
{: #replication }
{: faq}

{{site.data.keyword.messagehub}} está configurado para proporcionar una fuerte disponibilidad y duración.
Los siguientes valores de configuración se aplican a todos los temas y no se pueden cambiar:
* replication.factor = 3 
* min.insync.replicas = 2

## ¿Cuáles son las diferencias entre los planes Estándar de {{site.data.keyword.messagehub}} y Empresa de {{site.data.keyword.messagehub}}?
{: #plan_compare }
{: faq}

Para encontrar más información sobre los distintos planes de {{site.data.keyword.messagehub}}, consulte [Elección del plan](/docs/services/EventStreams?topic=eventstreams-plan_choose).

## ¿Cómo manejo la recuperación tras desastre?
{: #disaster_recovery }
{: faq}

En este momento, es responsabilidad del usuario gestionar su propia recuperación tras desastre de {{site.data.keyword.messagehub}}. Los datos de {{site.data.keyword.messagehub}} se pueden replicar entre una instancia de {{site.data.keyword.messagehub}} en una ubicación (región) y otra instancia en otra ubicación distinta. Sin embargo, el usuario es responsable de proporcionar una instancia remota de {{site.data.keyword.messagehub}} y de gestionar la réplica.

El usuario también es responsable de la copia de seguridad de los datos de carga útil de mensaje. Aunque estos datos se replican entre varios intermediarios de Kafka dentro de un clúster, que protege frente a la mayoría de las anomalías, esta réplica no cubre la anomalía de toda una ubicación. 

{{site.data.keyword.messagehub}} realiza copia de seguridad de los nombres de los temas.















