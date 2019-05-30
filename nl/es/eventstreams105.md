---

copyright:
  years: 2015, 2019
lastupdated: "2018-06-01"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# MQ bridge en el plan Clásico 
{: #mq_bridge}

**El puente de MQ solo está disponible como parte del plan Clásico.**
<br/>

El puente de MQ permite transferir datos de mensajes de una cola de MQ de {{site.data.keyword.IBM_notm}} a un tema de Kafka de {{site.data.keyword.messagehub}}. El puente de MQ permite realizar con eficacia cargas de trabajo al estilo de la nube (por ejemplo, análisis de datos) en los datos de mensajes MQ de {{site.data.keyword.IBM_notm}} generados en la empresa.
 {:shortdesc}

El puente de MQ se conecta con un gestor de cola de MQ de {{site.data.keyword.IBM_notm}} como un cliente de MQ y consume datos de mensajes MQ desde una cola de MQ. El puente convierte cada mensaje MQ en un registro de Kafka y envía el mensaje a un tema de Kafka de {{site.data.keyword.messagehub}}.

## Versiones soportadas de {{site.data.keyword.IBM_notm}} MQ
{: #mq_support}

Las versiones de {{site.data.keyword.IBM_notm}} MQ con las que funciona el puente son las siguientes:

* {{site.data.keyword.IBM_notm}} MQ versión 8.0 y posteriores

## Protección del puente de MQ
{: #mq_bridge_security}

Puede configurar el puente de MQ para autenticarse con {{site.data.keyword.IBM_notm}} MQ con otro usuario y contraseña. Recomendamos que otorgue los permisos siguientes sólo a la identidad asociada con una instancia del puente de MQ:

* Autorización CONNECT. El puente de MQ debe poder conectarse al gestor de colas de MQ.
* Autorización GET para la cola de la que el puente de MQ consumirá según su configuración.

## Fiabilidad de mensajes y pedidos
{: #mq_bridge_reliability}

El puente de MQ implementa al menos una vez un nivel de fiabilidad para los datos del mensaje que transfiere. Esto significa que el puente no pierde los datos del mensaje, pero ocasionalmente puede generar secuencias de registros de Kafka duplicadas para una secuencia determinada de mensajes MQ. Normalmente, la duplicación se produce cuando un suceso interrumpe el proceso del puente de datos de mensajes. Por ejemplo:

* Reinicio del gestor de colas de MQ
* Interrupción de la red entre el gestor de colas de MQ y el puente
* Nuevo equilibrio de la asignación de partición de temas de Kafka
* Interrupción de la red entre el puente y Kafka

Para garantizar que el puente de MQ pueda transferir mensajes de MQ de forma fiable, el puente solicita acceso exclusivo a la cola de MQ de la que lee los mensajes. Este acceso impide que otras aplicaciones reciban mensajes de la cola de MQ mientras el puente la esté utilizando. Si otras aplicaciones ya están recibiendo mensajes de la cola, es posible que el puente de MQ no se pueda iniciar hasta que estas aplicaciones hayan finalizado.

En general, el puente MQ reenvía mensajes a Kafka en el orden en que se reciben de {{site.data.keyword.IBM_notm}} MQ. Sin embargo, en ocasiones los datos de mensajes de MQ se duplican en un tema de Kafka (por las razones descritas anteriormente). Esta duplicación puede dar lugar a diferencias entre el orden de los mensajes de MQ y el orden en que se almacenan en Kafka los registros correspondientes.

## Asignación de partición de Kafka
{: #mq_bridge_partition}

El puente de MQ soporta opciones para controlar la asignación de datos de mensajes de {{site.data.keyword.IBM_notm}} MQ a particiones de temas de Kafka. El orden de mensajes dentro de una partición determinada depende de la opción seleccionada para el orden de datos de mensaje (consulte [Fiabilidad y orden de mensajes](#mq_bridge_reliability)). Los valores soportados son los siguientes:
<dl><dt>Valor predeterminado</dt>
<dd>Como valor predeterminado, se utiliza la asignación de particiones predeterminada de Kafka, que distribuye los datos de los mensajes de MQ de manera uniforme entre las particiones del tema de Kafka.</dd>
<dt>ID de correlación</dt>
<dd>Los mensajes de MQ con el mismo ID de correlación están asignados a la misma partición de Kafka.</dd>
<dt>ID de grupo</dt>
<dd>Los mensajes de MQ con el mismo ID de grupo están asignados a la misma partición de Kafka.
</dd>
</dl>

## Restricciones de tamaño de mensaje
{: #mq_message}

Puede configurar {{site.data.keyword.IBM_notm}} MQ para que almacene los mensajes que sean demasiado grandes para un registro de {{site.data.keyword.messagehub}} Kafka. El tamaño máximo del registro de Kafka es de 1.000.000 de bytes, aunque parte de esta capacidad se utiliza cuando el puente está configurado para realizar la asignación de particiones de Kafka basándose en el identificador de correlación de MQ o en el identificador de grupo de MQ. Se recomienda enviar mensajes con un tamaño inferior a los 950 kilobytes utilizando el puente de MQ.

Si el puente de MQ detecta un mensaje demasiado grande para reenviarlo a Kafka, lo descarta y escribe una entrada de registro en el panel de control de Kibana. Considere la posibilidad de establecer el atributo MAXMSGL de la cola de MQ de la que el puente recibe los mensajes de modo que se impida que las aplicaciones MQ envíen a la cola mensajes que no se pueden transferir utilizando el puente de MQ.
