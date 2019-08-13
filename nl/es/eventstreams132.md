---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-03"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}

# Acuerdo de nivel de servicio (SLA) para disponibilidad de {{site.data.keyword.messagehub}} 
{: #sla}

## Plan Estándar
{: #sla_standard}
El servicio {{site.data.keyword.messagehub}} se proporciona con una disponibilidad del 99,95% en el plan Estándar.
Para obtener más información sobre el acuerdo de nivel de servicio para servicios de alta disponibilidad en {{site.data.keyword.Bluemix}}, consulte [Descripción de servicios de {{site.data.keyword.Bluemix_notm}}![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www-03.ibm.com/software/sla/sladb.nsf/8bd55c6b9fa8039c86256c6800578854/c4ceb9f019f9eb4c862582f9001b3994/$FILE/i126-6605-16_04-2019_en_US.pdf){:new_window}.


## Plan Empresa
{: #sla_enterprise}

El servicio {{site.data.keyword.messagehub}} se proporciona con una disponibilidad del 99,95% en el plan Empresa como entorno público de alta disponibilidad. Cuando se ejecuta el servicio de {{site.data.keyword.messagehub}} en entornos distintos de HA como, por ejemplo,
[ubicaciones de una sola zona](#sla_szr), la disponibilidad es del 99,5%. 
Para obtener más información sobre el acuerdo de nivel de servicio para los servicios de alta disponibilidad en
{{site.data.keyword.Bluemix_notm}}, consulte
[Descripción de servicios de {{site.data.keyword.Bluemix_notm}}![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www-03.ibm.com/software/sla/sladb.nsf/8bd55c6b9fa8039c86256c6800578854/c4ceb9f019f9eb4c862582f9001b3994/$FILE/i126-6605-16_04-2019_en_US.pdf){:new_window}.

## Plan Clásico
{: #sla_classic}
El servicio {{site.data.keyword.messagehub}} se proporciona con una disponibilidad del 99,5% en el plan Clásico. 
Para obtener más información sobre el acuerdo de nivel de servicio para {{site.data.keyword.Bluemix_notm}}, consulte [Descripción de servicios de {{site.data.keyword.Bluemix_notm}}![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www-03.ibm.com/software/sla/sladb.nsf/8bd55c6b9fa8039c86256c6800578854/c4ceb9f019f9eb4c862582f9001b3994/$FILE/i126-6605-16_04-2019_en_US.pdf){:new_window}.

<!--
## What does 99.95% availability mean?
Availability refers to the ability of applications to produce and consume messages from Kafka topics.
-->

## ¿Cómo la medimos?
Las instancias de servicio se supervisan constantemente en cuanto a rendimiento, índice de errores y respuesta a operaciones sintéticas. Las paradas se registran. Para obtener más información, consulte [Estado de servicio para Event Streams ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/status?component=messagehub&selected=status){:new_window}.

La disponibilidad se refiere a la capacidad de las aplicaciones para producir y consumir mensajes desde temas Kafka.

## ¿Qué se necesita tener en cuenta para alcanzar esta disponibilidad?
Para alcanzar altos niveles de disponibilidad desde la perspectiva de las aplicaciones, debe tenerse en cuenta la [conectividad](/docs/services/EventStreams?topic=eventstreams-sla#connectivity), el [rendimiento](/docs/services/EventStreams?topic=eventstreams-sla#throughput) y la [coherencia y durabilidad de los mensajes](/docs/services/EventStreams?topic=eventstreams-sla#message_consistency). Los usuarios tienen la responsabilidad de diseñar sus aplicaciones para optimizar estos tres elementos para su negocio.

### Conectividad
{: #connectivity}

Debido a la naturaleza dinámica de la nube, las aplicaciones deben esperar interrupciones de la conexión. Una interrupción de la conexión no se considera una anomalía de servicio.

**Reintentos**<br/>
Los clientes de Kafka proporcionan lógica de reconexión, pero deben habilitarse explícitamente las reconexiones para los productores. Para obtener más información, consulte la propiedad de código [ <code>retries</code> ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://kafka.apache.org/11/documentation.html#producerconfigs){:new_window}. Las conexiones se restauran en 60 segundos.
 
**Duplicados**<br/>
Es posible que el hecho de habilitar los reintentos dé como resultado mensajes duplicados. Dependiendo de cuándo se pierde una conexión, es posible que el productor no pueda determinar si un mensaje ha sido procesado correctamente por el servidor y por lo tanto debe volver a enviar el mensaje al reconectarse. Se le recomienda que diseñe aplicaciones que esperen mensajes duplicados. 

Si no pueden tolerarse los duplicados, puede utilizar la característica del productor <code>idempotent</code> (de Kafka 1.1) para prevenir los duplicados durante los reintentos. Para obtener más información, consulte la propiedad de código [ <code>enable.idempotence</code> ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://kafka.apache.org/11/documentation.html#producerconfigs){:new_window}.

### Rendimiento
{: #throughput}

El rendimiento se expresa como el número de bytes por segundo que pueden enviarse y recibirse en un clúster. 

**Orientaciones específicas para el plan Estándar**<br/>
Para obtener información de orientaciones para un mejor rendimiento, consulte [Límites y cuotas- Estándar](/docs/services/EventStreams?topic=eventstreams-kafka_quotas#standard_throughput). 

**Orientaciones específicas para el plan Empresa**<br/>

Para obtener información de orientaciones para un mejor rendimiento, consulte [Límites y cuotas - Empresa](/docs/services/EventStreams?topic=eventstreams-kafka_quotas#enterprise_throughput). 

**Mediciones**<br/>
Se recomienda instrumentar las aplicaciones para conocer su rendimiento. Por ejemplo, el número de mensajes enviados y recibidos, los tamaños de los mensajes y los códigos de retorno. Entender el uso de una aplicación le ayuda a configurar adecuadamente sus recursos, como por ejemplo el tiempo de retención de los mensajes por temas.

**Saturación**<br/>
A medida que se aproxima el límite del tráfico que se puede producir en el clúster, los productores empiezan a regularse, aumenta la latencia, y finalmente se producen errores de tiempo de espera excedido. Según la configuración, es posible que también haya repercusión en la coherencia y la durabilidad de los mensajes. Para obtener más información, consulte [Coherencia y durabilidad de los mensajes](/docs/services/EventStreams?topic=eventstreams-sla#message_consistency).

### Coherencia y durabilidad de los mensajes
{: #message_consistency}

Kafka alcanza su disponibilidad y durabilidad replicando los mensajes que recibe en otros nodos del clúster, réplicas que pueden utilizarse en caso de anomalía. {{site.data.keyword.messagehub}} utiliza tres réplicas (default.replication.factor = 3), lo que significa que cada mensaje recibido por un nodo se replica a otros dos nodos en diferentes zonas de disponibilidad. De esta forma puede tolerarse la pérdida de un nodo o de una zona de disponibilidad sin pérdida de datos ni de capacidades.

**Modo <code>acks</code> de productor**<br/>
Aunque todos los mensajes se replican, las aplicaciones pueden controlar la solidez de la transferencia de los mensajes producidos al servicio utilizando la propiedad de modo <code>acks</code> del productor. Esta propiedad proporciona una opción entre la velocidad y el riesgo de perder mensajes. El valor predeterminado es <code>acks=1</code>, lo que significa que el productor devuelve un código de éxito en cuanto el nodo al que está conectado reconoce la recepción del mensaje, pero antes de que se complete la réplica. El valor recomendado y más seguro es <code>acks=all</code>, donde el productor solamente devuelve un código de éxito una vez que el mensaje se ha copiado a todas las réplicas. Esto asegura que las réplicas se mantienen en el mismo punto, lo que previene pérdidas de mensajes si una anomalía hace que se conmute a una réplica.

## Despliegues en ubicación de una sola zona
{: #sla_szr}

Si desea obtener la máxima disponibilidad, le recomendamos nuestros entornos públicos de alta disponibilidad que se han diseñado en nuestras ubicaciones multizona. En una ubicación multizona, nuestros clústeres de Kafka se distribuyen en 3 zonas de disponibilidad, lo que significa que el clúster es resistente al error de una sola zona o de cualquier componente dentro de dicha zona.
Algunos clientes precisan de la localidad geográfica y, por lo tanto, desean suministrar un clúster de {{site.data.keyword.messagehub}} en una ubicación local a nivel geográfico pero en una sola zona. {{site.data.keyword.messagehub}} admite este modelo de despliegue; sin embargo, tenga en cuenta las siguientes contrapartidas de disponibilidad:
* En una ubicación de una sola zona, hay categorías de errores únicos que pueden dejar el clúster fuera de línea durante un período de tiempo. Por ejemplo, el error de todo un centro de datos o la actualización o error de un componente compartido como, por ejemplo, el hipervisor subyacente, SAN o la red. Estas anomalías se reflejan en un acuerdo de nivel de servicio reducido para ubicaciones de una sola zona.
* Una ventaja de dispersar Kafka en diversas zonas es que se reduce la posibilidad de un error que podría desactivar todo el clúster. Por otro lado, existe una mínima posibilidad de que un único error pudiera desactivar todo el clúster dentro de una zona. En casos extremos, también podrían perderse datos. Por ejemplo, incluso si los productores utilizan <code>acks=all</code>, si todos los nodos de Kafka se desactivaran de forma simultánea, podrían darse ciertos mensajes indicando que los intermediarios han acusado recibo pero el sistema de archivos subyacente no ha completado la acción de vaciado en disco. Potencialmente, estos mensajes de no vaciado se podrían perder. 

    Para obtener más información, consulte [Acuses de recibo de mensajes](/docs/services/EventStreams?topic=eventstreams-producing_messages#message_acknowledgments). En muchos casos de uso, no constituye ningún problema. Sin embargo, si la pérdida de mensajes no es aceptable bajo ninguna circunstancia, tenga en cuenta otras estrategias como, por ejemplo, utilizar un clúster de zona de disponibilidad múltiple, una réplica de regiones cruzadas o la sincronización por puntos de comprobación de los mensajes del lado del productor.

Para obtener más información, consulte [clústeres de una sola zona](/docs/containers?topic=containers-regions-and-zones#regions_single_zone) y [clústeres multizonas](/docs/containers?topic=containers-regions-and-zones#regions_multizone).
