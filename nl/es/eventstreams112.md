---

copyright:
  years: 2015, 2019
lastupdated: "2018-06-23"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Generación de mensajes
{: #producing_messages }


Un productor es una aplicación que publica secuencias de mensajes en temas Kafka. Esta información se centra en la interfaz de programación Java que forma parte del proyecto Apache Kafka. Los conceptos se aplican también a otros idiomas, pero a veces son un poco distintos.
{: shortdesc}

En las interfaces de programación, un mensaje realmente se denomina un registro. Por ejemplo la clase Java org.apache.kafka.clients.producer.ProducerRecord se utiliza para representar un mensaje desde el punto de vista de la API de productor. Los términos _registro_ y _mensaje_ se pueden utilizar indistintamente, pero en esencia un registro se utiliza para representar un mensaje.

Cuando un productor se conecta a Kafka, realiza una conexión de arranque inicial. Esta conexión se puede realizar a cualquiera de los servidores del clúster. El productor solicita la información de partición y liderazgo sobre el tema que desea publicar. A continuación, el productor establece otra conexión al líder de la partición y puede empezar a publicar mensajes. Estas acciones se producen automáticamente de forma interna cuando el productor se conecta al clúster Kafka.
 
Cuando se envía un mensaje al líder de la partición, este mensaje no está disponible inmediatamente para los consumidores. El líder añade el registro del mensaje a la partición, asignándole el siguiente número de desplazamiento para dicha partición. Después de que todos los seguidores de las réplicas sincronizadas hayan replicado el registro y reconocido que han anotado el registro en sus réplicas, el registro ahora está *confirmado* y está disponible para los consumidores.

Cada mensaje está representado en forma de registro que consta de dos partes: clave y valor. La clave se utiliza habitualmente para los datos sobre el mensaje y el valor es el cuerpo del mensaje. Puesto que muchas herramientas del ecosistema de Kafka (como los conectores para otros sistemas) utilizan sólo el valor e ignoran la clave, es mejor poner todos los datos del mensaje en el valor y sólo utilizar la clave para el particionamiento o la compactación de registros. No debería confiar en todo lo que se lee de Kafka para hacer uso de la clave.

Muchos otros sistemas de mensajería también tienen una manera de llevar otra información junto con los mensajes. Kafka 0.11 introduce las cabeceras de registro con este propósito, lo que recibe soporte del plan Empresa de {{site.data.keyword.messagehub}}. Actualmente el plan Estándar de {{site.data.keyword.messagehub}} se basa en Kafka 0.10.2.1, por lo que aún no da soporte a las cabeceras de registro. 

Puede resultarle útil leer esta información junto con [Consumo de mensajes](/docs/services/EventStreams/eventstreams114.html) en {{site.data.keyword.messagehub}}.

## Valores de configuración
{: #config_settings}

Hay muchos valores de configuración para el productor. Puede controlar aspectos del productor incluidos el trabajo por lotes, los reintentos y el reconocimiento de mensajes. Estos son los más importantes:

| Nombre     |Descripción   | Valores válidos   | Valor predeterminado   |
|----------|---------|----------|---------|
|key.serializer     | La clase utilizada para serializar claves. | Clase Java que implementa la interfaz del serializador, como org.apache.kafka.common.serialization.StringSerializer |Ningún valor predeterminado. Debe especificar un valor.|
|value.serializer     | La clase utilizada para serializar valores. | Clase Java que implementa la interfaz del serializador, como org.apache.kafka.common.serialization.StringSerializer  | Ningún valor predeterminado. Debe especificar un valor. |
|acks     | El número de servidores necesarios para reconocer cada mensaje publicado. Controla las garantías de duración que requiere el productor. | 0, 1, all (o -1)  | 1 |
|retries     | El número de veces que el cliente reenvía un mensaje cuando el envío encuentra un error. | 0,...  | 0 |
|max.block.ms     | El número de milisegundos que una solicitud de envío o de metadatos puede bloquear la espera. | 0,...  | 60000 (1 minuto) |
|max.in.flight.requests.per.connection     | El número máximo de solicitudes no reconocidas que el cliente envía en una conexión antes de bloquear nuevas solicitudes.| 1,...  | 5 |
|request.timeout.ms     | La cantidad máxima de tiempo que el productor esperará una respuesta a una solicitud. Si la respuesta no se recibe antes de que finalice el tiempo de espera, se reintenta realizar la solicitud o falla si se ha agotado el número de reintentos.| 0,...  | 30000 (30 segundos) |

Hay muchos más valores de configuración disponibles, pero asegúrese de leer detenidamente la [documentación de Apache Kafka ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://kafka.apache.org/documentation/){:new_window} antes de experimentar con ellos.

## Particionamiento
{: #partitioning}

Cuando el productor publica un mensaje en un tema, el productor puede elegir qué partición utilizar. Si el orden es importante, debe recordar que una partición es una secuencia ordenada de registros, pero un tema consta de una o varias particiones. Si desea un conjunto de mensajes se entreguen en orden, asegúrese de que todos ellos vayan en la misma partición. La forma más sencilla de lograrlo es dar la misma clave a todos los mensajes. 
 
El productor puede especificar explícitamente un número de partición cuando publica un mensaje. Esto permite el control directo, pero hace que el código de productor sea más complejo porque toma la responsabilidad de gestionar la selección de partición. Para obtener más información, consulte la llamada de método Producer.partitionsFor. Por ejemplo, la llamada se describe para [Kafka 0.11.0.1 ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://kafka.apache.org/0110/javadoc/org/apache/kafka/clients/producer/KafkaProducer.html){:new_window}
 
Si el productor no especifica un número de partición, un particionador realiza la selección de la partición. El particionador predeterminado incluido en el productor de Kafka funciona de la siguiente manera:

* Si el registro no tiene ninguna clave, seleccione la partición de forma rotativa.
* Si el registro tiene una clave, seleccione la partición calculando un valor hash para la clave. Esto tiene el efecto de seleccionar la misma partición para todos los mensajes con la misma clave.

También puede escribir su propio particionador personalizado. Un particionador personalizado puede elegir cualquier esquema para asignar registros a particiones. Por ejemplo, utilice sólo un subconjunto de la información en la clave o un identificador específico para la aplicación.


## Ordenación de mensajes
{: #message_ordering}

Kafka normalmente escribe los mensajes en el orden en el que el productor los ha enviado. Sin embargo, hay situaciones en las que los reintentos pueden provocar que los mensajes se dupliquen o se reordenen. Si desea que una secuencia de mensajes se envíen en orden, es muy importante asegurarse de que todos estén escritos en la misma partición.
 
El productor también puede reintentar enviar mensajes automáticamente. A menudo es una buena idea habilitar esta característica de reintento porque la alternativa es que el código de la aplicación tenga que realizar los reintentos él mismo. La combinación de lotes en Kafka y reintentos automáticos puede tener el efecto de duplicar los mensajes y reordenarlos.
 
Por ejemplo, si publica una secuencia de tres mensajes &lt; M1, M2, M3 &gt; en un tema. Los registros pueden ajustarse todos al mismo lote, de modo que en realidad todos se envían juntos al líder de la partición. El líder luego los escribe en la partición y los replica como registros separados. En el caso de que se produzca una anomalía, es posible que M1 y M2 se añadan a la partición, pero no M3. El productor no recibe ningún acuse de recibo, de modo que vuelve a intentar enviar &lt;M1, M2, M3&gt;. El nuevo líder simplemente escribe M1, M2 y M3 en la partición, que ahora contiene &lt;M1, M2, M1, M2, M3&gt;, donde el M1 duplicado en realidad sigue al M2 original. Si restringe el número de solicitudes en curso a cada intermediario a sólo una, puede evitar esta reordenación. Aún puede encontrar que un único registro esté duplicado, como &lt;M1, M2, M2, M3 &gt;, pero nunca se desordenarán las secuencias. En Kafka 0,11 (todavía no disponible en {{site.data.keyword.messagehub}}), también puede utilizar la función de productor idempotente para evitar la duplicación de M2.
 
Es una práctica normal con Kafka escribir aplicaciones para que gestionen duplicados de mensajes ocasionales porque el impacto en el rendimiento de tener sólo una única solicitud en curso es significativo.

## Acuses de recibo de mensajes
{: #message_acknowledgments}

Cuando se publica un mensaje, puede elegir el nivel de acuses de recibo necesarios utilizando la configuración de productor `acks`. La elección representa un equilibrio entre rendimiento y fiabilidad. Existen los tres niveles siguientes:

<dl>
<dt>acks=0 (menos fiable)</dt>
<dd>El mensaje se considera enviado en cuanto se ha escrito en la red. No hay ningún acuse de recibo por parte del líder de la partición. En consecuencia, los mensajes se pueden perder si cambia el liderazgo de la partición. Este nivel de acuse de recibo es muy rápido, pero existe la posibilidad de que se pierdan mensajes en algunas situaciones.</dd>
<dt>acks=1 (valor predeterminado)</dt>
<dd>Se acusa el recibo del mensaje al productor tan pronto como el líder de la partición ha escrito correctamente su registro en la partición. Como el acuse de recibo se produce antes de que se sepa que el registro ha alcanzado las réplicas sincronizadas, el mensaje se podría perder si el líder fallada pero los seguidores todavía no tuvieran el mensaje. Si cambia el liderazgo de la partición, el antiguo líder informa al productor, que puede manejar el error y reintentar el envío del mensaje al nuevo líder. Dado que los mensajes se confirman antes de que todas las réplicas hayan confirmado su recepción, los mensajes que se hayan confirmado pero todavía no se hayan replicado completamente se pueden perder si cambia el liderazgo de la partición.</dd>
<dt>acks=all (más fiable)</dt>
<dd>Se acusa el recibo del mensaje al productor cuando el líder de la partición ha escrito correctamente su registro y todas las réplicas sincronizadas han hecho lo mismo. El mensaje no se pierde si el liderazgo cambia siempre que como mínimo haya una réplica sincronizada disponible.</dd>
</dl>

Incluso si no espera que el productor acuse el recibo de los mensajes, los mensajes seguirán estando disponibles sólo para ser consumidos cuando se confirmen y esto significa que la replicación a las réplicas sincronizadas está completa. En otras palabras, la latencia del envío de mensajes desde el punto de vista del productor es inferior a la latencia de extremo a extremo desde el productor que envía un mensaje a un consumidor que lo recibe.

Si es posible, evite esperar al acuse de recibo de un mensaje antes de publicar el siguiente. Esperar impide que el productor agrupe mensajes en lotes y también reduce la velocidad con la que se pueden publicar mensajes para reducir la latencia de ida y vuelva de la red.

## Agrupación por lotes, limitación y compresión
{: #batching}

Para mejorar la eficiencia, el productor agrupa lotes de registros para enviarlos a los servidores. Si habilita la compresión, el productor comprime cada lote, lo cual puede mejorar el rendimiento al requerir que se transfieran menos datos a través de la red.

Si intenta publicar mensajes de forma más rápida que la necesaria para que se envíen a un servidor, el productor las almacena automáticamente en solicitudes por lotes. El productor mantiene un almacenamiento intermedio de registros no enviados para cada partición. Por supuesto, llega un momento en que incluso la agrupación por lotes no permite lograr la velocidad deseada.
 
Hay otro factor que tiene un impacto. Para evitar que productores o consumidores individuales inunden el clúster, {{site.data.keyword.messagehub}} aplica cuotas de rendimiento. Se calcula la velocidad con la que cada productor envía datos y se regula a cualquier productor que intente superar su cuota. La regulación se aplica retrasando ligeramente el envío de respuestas al productor. Normalmente, este método actúa como un freno natural.
 
En resumen, cuando un mensaje se publica, su primer registro se escribe primero en un almacenamiento intermedio en el productor. En segundo plano, el productor agrupa los registros en lotes y los envía al servidor. A continuación, el servidor responde al productor, posiblemente aplicando un retraso de limitación si el productor está publicando demasiado rápido. Si se llena el almacenamiento intermedio del productor, la llamada de envío del productor se retrasa, pero podría fallar con una excepción.

## Fragmentos de código
{: #code_snippets}

Estos fragmentos de código tienen un nivel muy alto para ilustrar los conceptos implicados. Para ver ejemplos completos, consulte los ejemplos de {{site.data.keyword.messagehub}} en GitHub https://github.com/ibm-messaging/event-streams-samples.

Para conectarse a {{site.data.keyword.messagehub}}, primero debe crear el conjunto de propiedades de configuración. Todas las conexiones a {{site.data.keyword.messagehub}} están protegidas mediante TLS y autenticación de usuario/contraseña, de modo que necesita estas propiedades como mínimo. Sustituya KAFKA_BROKERS_SASL, USER y PASSWORD por sus credenciales de servicio:

```
Properties props = new Properties();
 props.put("bootstrap.servers", KAFKA_BROKERS_SASL);
 props.put("sasl.jaas.config", "org.apache.kafka.common.security.plain.PlainLoginModule required username=\"USER\" password=\"PASSWORD\";");
 props.put("security.protocol", "SASL_SSL");
 props.put("sasl.mechanism", "PLAIN");
 props.put("ssl.protocol", "TLSv1.2");
 props.put("ssl.enabled.protocols", "TLSv1.2");
 props.put("ssl.endpoint.identification.algorithm", "HTTPS");
```

Para enviar mensajes, también debe especificar serializadores para las claves y valores, por ejemplo:

```
 props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
 props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");
```

A continuación, utilice un KafkaProducer para enviar mensajes, donde cada mensaje está representado por un ProducerRecord. No olvide cerrar el KafkaProducer cuando haya terminado. Este código sólo envía el mensaje, pero no espera para ver si el envío ha sido satisfactorio.

```
 Producer<String, String> producer = new KafkaProducer<>(props);
 producer.send(new ProducerRecord<String, String>("T1", "key", "value"));
 producer.close();
 ```
 
El método `send()` es asíncrono y devuelve un Future que puede utilizar para comprobar su finalización:

```
 Future<RecordMetadata> f = producer.send(new ProducerRecord<String, String>("T1", "key", "value"));
// Do some other stuff
// Now wait for the result of the send
 RecordMetadata rm = f.get();
 long offset = rm.offset; 
```

Como alternativa, puede proporcionar una devolución de llamada cuando envíe el mensaje:

```
producer.send(new ProducerRecord<String,String>("T1","key","value", new Callback() {
          public void onCompletion(RecordMetadata metadata, Exception exception) {
                     // This is called when the send completes, either successfully or with an exception
          }
});
```

Para obtener más información, consulte el [Javadoc para el cliente Kafka ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://kafka.apache.org/0110/javadoc/index.html){:new_window}, que es muy completo. 

