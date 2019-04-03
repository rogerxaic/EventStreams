---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-06"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Acuerdo de nivel de servicio (SLA) para disponibilidad de {{site.data.keyword.messagehub}} (Plan Empresa)
{: #sla}

El servicio {{site.data.keyword.messagehub}} se proporciona con una disponibilidad del 99,95% en el plan Empresa. 
Para obtener más información sobre el acuerdo de nivel de servicio para {{site.data.keyword.Bluemix}}, consulte la [descripción del servicio de {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www-03.ibm.com/software/sla/sladb.nsf/pdf/6605-14/$file/i126-6605-14_08-2018_en_US.pdf){:new_window}.

## ¿Qué significa el 99,95% de disponibilidad?
La disponibilidad se refiere a la capacidad de las aplicaciones para producir y consumir mensajes desde temas Kafka.

## ¿Cómo la medimos?
Las instancias de servicio se supervisan constantemente en cuanto a rendimiento, índice de errores y respuesta a operaciones sintéticas. Las paradas se registran.

## ¿Qué se necesita tener en cuenta para alcanzar esta disponibilidad?
Para alcanzar altos niveles de disponibilidad desde la perspectiva de las aplicaciones, debe tenerse en cuenta la [conectividad](/docs/services/EventStreams?topic=eventstreams-sla#connectivity), el [rendimiento](/docs/services/EventStreams?topic=eventstreams-sla#throughput) y la [coherencia y durabilidad de los mensajes](/docs/services/EventStreams?topic=eventstreams-sla#message_consistency). Los usuarios tienen la responsabilidad de diseñar sus aplicaciones para optimizar estos tres elementos para su negocio.

### Conectividad
{: #connectivity}

Debido a la naturaleza dinámica de la nube, las aplicaciones deben esperar interrupciones de la conexión. Una interrupción de la conexión no se considera una anomalía de servicio.

**Reintentos**<br/>
Los clientes de Kafka proporcionan lógica de reconexión, pero deben habilitarse explícitamente las reconexiones para los productores. Para obtener más información, consulte la propiedad de código [ <code>retries</code> ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://kafka.apache.org/11/documentation.html#producerconfigs){:new_window}. Las conexiones se restauran en 60 segundos.
 
**Duplicados**<br/>
Es posible que habilitar los reintentos resulte en mensajes duplicados. Dependiendo de cuándo se pierde una conexión, es posible que el productor no pueda determinar si un mensaje ha sido procesado correctamente por el servidor y por lo tanto debe volver a enviar el mensaje al reconectarse. Se le recomienda que diseñe aplicaciones que esperen mensajes duplicados. 

Si no pueden tolerarse los duplicados, puede utilizar la característica del productor <code>idempotent</code> (de Kafka 1.1) para prevenir los duplicados durante los reintentos. Para obtener más información, consulte la propiedad de código [ <code>enable.idempotence</code> ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://kafka.apache.org/11/documentation.html#producerconfigs){:new_window}.

### Rendimiento
{: #throughput}

El rendimiento se expresa como el número de bytes por segundo que pueden enviarse y recibirse en un clúster. 

**Recomendación**<br/>
40 MB por segundo con un límite pico máximo de 90 MB por segundo. <br/>
La cifra recomendada se basa en una carga de trabajo típica y tiene en cuenta el posible impacto de acciones operativas como actualizaciones internas o modalidades de fallo tales como la pérdida de una zona de disponibilidad.  Por ejemplo, mensajes con una carga útil pequeña (menos de 10K). Si el rendimiento medio supera esta cifra, es posible que experimente una pérdida de rendimiento mientras duren estas condiciones.

**Medida**<br/>
Se le recomienda que instrumente las aplicaciones para ser consciente de su rendimiento. Por ejemplo, el número de mensajes enviados y recibidos, los tamaños de los mensajes y los códigos de retorno. Entender el uso de una aplicación le ayuda a configurar adecuadamente sus recursos, como por ejemplo el tiempo de retención de los mensajes por temas.

**Saturación**<br/>
A medida que se aproxima el límite del tráfico que se puede producir en el clúster, los productores empiezan a regularse, aumenta la latencia, y finalmente se producen errores de tiempo de espera excedido. Según la configuración, es posible que también haya repercusión en la coherencia y la durabilidad de los mensajes. Para obtener más información, consulte [Coherencia y durabilidad de los mensajes](/docs/services/EventStreams?topic=eventstreams-sla#message_consistency).

### Coherencia y durabilidad de los mensajes
{: #message_consistency}

Kafka alcanza su disponibilidad y durabilidad replicando los mensajes que recibe en otros nodos del clúster, réplicas que pueden utilizarse en caso de anomalía. {{site.data.keyword.messagehub}} utiliza tres réplicas (default.replication.factor = 3), lo que significa que cada mensaje recibido por un nodo se replica a otros dos nodos en diferentes zonas de disponibilidad. De esta forma puede tolerarse la pérdida de un nodo o de una zona de disponibilidad sin pérdida de datos ni de capacidades.

**Modo <code>acks</code> de productor**<br/>
Aunque todos los mensajes se replican, las aplicaciones pueden controlar la solidez de la transferencia de los mensajes producidos al servicio utilizando las propiedades de mod <code>ack</code> del productor. Esta propiedad proporciona una opción entre la velocidad y el riesgo de perder mensajes. El valor predeterminado es <code>acks=1</code>, lo que significa que el productor devuelve un código de éxito en cuanto el nodo al que está conectado reconoce la recepción del mensaje, pero antes de que se complete la replicación. El valor recomendado y más seguro es <code>acks=all</code>, donde el productor solamente devuelve un código de éxito una vez que el mensaje se ha copiado a todas las réplicas. Esto asegura que las réplicas se mantienen en el mismo punto, lo que previene pérdidas de mensajes si una anomalía hace que se conmute a una réplica.


