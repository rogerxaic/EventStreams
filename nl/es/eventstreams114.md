---

copyright:
  years: 2015, 2018
lastupdated: "2018-03-23"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Consumo de mensajes
{: #consuming_messages }

Un consumidor es una aplicación que consume secuencias de mensajes de temas Kafka. Un consumidor puede suscribirse a uno o más temas o particiones. Esta información se centra en la interfaz de programación Java que forma parte del proyecto Apache Kafka. Los conceptos se aplican también a otros idiomas, pero a veces son un poco distintos.

Cuando un consumidor se conecta a Kafka, realiza una conexión de arranque inicial. Esta conexión se puede realizar a cualquiera de los servidores del clúster. El consumidor solicita la información de partición y liderazgo sobre el tema del que desea consumir. A continuación, el consumidor establece otra conexión al líder de la partición y puede empezar a consumir mensajes. Estas acciones se producen automáticamente de forma interna cuando el consumidor se conecta al clúster Kafka.

Un consumidor es normalmente una aplicación de larga ejecución. Un consumidor solicita mensajes de Kafka llamando a `Consumer.poll(...)` de forma regular. El consumidor llama a `poll()`, recibe un lote de mensajes, los procesa de inmediato y vuelve a llamar a `poll()`.

Cuando un consumidor procesa un mensaje, el mensaje no se elimina de su tema. En su lugar, los consumidores pueden elegir entre varias formas de hacer saber a Kafka qué mensajes se han procesado. Este proceso se conoce como confirmar el desplazamiento.

En las interfaces de programación, un mensaje realmente se denomina un registro. Por ejemplo, la clase Java org.apache.kafka.clients.consumer.ConsumerRecord se utiliza para representar un mensaje para la API de consumidor. Los términos _registro_ y _mensaje_ se pueden utilizar indistintamente, pero en esencia un registro se utiliza para representar un mensaje.

Puede resultarle útil leer esta información conjuntamente con [Generación de mensajes](/docs/services/EventStreams/eventstreams112.html) en {{site.data.keyword.messagehub}}.

## Configuración de propiedades de consumidor 
{: #configuring_consumer_properties }

Hay muchos valores de configuración para el consumidor, que controlan aspectos de su comportamiento. Estos son algunos de los más importantes:

| Nombre     |Descripción   | Valores válidos   | Valor predeterminado   |
|----------|---------|----------|---------|
|key.deserializer     | La clase utilizada para deserializar claves. | Clase Java que implementa la interfaz del deserializador, como por ejemplo org.apache.kafka.common.serialization.StringDeserializer  |Ningún valor predeterminado. Debe especificar un valor.|
|value.deserializer     | La clase utilizada para deserializar valores. | Clase Java que implementa la interfaz del deserializador, como por ejemplo org.apache.kafka.common.serialization.StringDeserializer  | Ningún valor predeterminado. Debe especificar un valor. |
|group.id | Un identificador para el grupo de consumidores al que pertenece el consumidor. | string |Ningún valor predeterminado|
|auto.offset.reset | El comportamiento cuando el consumidor no tiene desplazamiento inicial o si el desplazamiento actual no está disponible en el clúster. | latest, earliest, none | latest |
|enable.auto.commit | Determina si se debe confirmar el desplazamiento del consumidor automáticamente en segundo plano. | true, false | true |
|auto.commit.interval.ms | El número de milisegundos entre confirmaciones periódicas de desplazamientos. | 0,... | 5000 (5 segundos) |
|max.poll.records | El número máximo de registros devueltos en una llamada a poll() | 1,... | 500 |
|session.timeout.ms | El número de milisegundos en el que un latido del consumidor debe ser recibido para mantener la pertenencia de un consumidor de un grupo de consumidores. | 6000-300000 | 10000 (10 segundos) |
|max.poll.interval.ms |El intervalo de tiempo máximo entre sondeos antes de que el consumidor deje el grupo. | 1,... | 300000 (5 minutos) |
| | | | |

Hay muchos más valores de configuración disponibles, pero asegúrese de leer detenidamente la [documentación de Apache Kafka ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://kafka.apache.org/documentation/){:new_window} antes de experimentar con ellos.

## Grupos de consumidores

Un _grupo de consumidores_ es un grupo de consumidores que cooperan para consumir mensajes de uno o varios temas. Los consumidores de un grupo utilizan todos el mismo valor para la configuración de `group.id`. Si necesita más de un consumidor para gestionar su carga, puede ejecutar varios consumidores en el mismo grupo de consumidores. Incluso si sólo necesita un consumidor, es habitual también especificar un valor para `group.id`.

Cada grupo de consumidores tiene un servidor en el clúster denominado _coordinador_ responsable de asignar particiones a los consumidores del grupo. Esta responsabilidad se reparte entre los servidores del clúster para repartir la carga. La asignación de particiones a los consumidores puede cambiar en cada reequilibrio del grupo.

Cuando un consumidor se une a un grupo de consumidores, descubre el coordinador del grupo. A continuación, el consumidor indica al coordinador que desea unirse al grupo y el coordinador inicia un reequilibrio de las particiones en el grupo incluyendo al nuevo miembro.

Cuando uno de los siguientes mensajes se produce en un grupo de consumidores, el grupo se reequilibra cambiando la asignación de particiones a los miembros del grupo para acomodar el cambio:

* un consumidor se une al grupo
* un consumidor deja el grupo
* el coordinador considera que un consumidor ya no está activo
* se añaden particiones nuevas a un tema existente

Para cada grupo de consumidores, Kafka recuerda el desplazamiento comprometido para cada partición consumida.

Si tiene un grupo de consumidores que se ha reequilibrado, tenga en cuenta que cualquier consumidor que haya dejado el grupo tendrá sus confirmaciones rechazadas hasta que se vuelva a unir al grupo. En este caso, el consumidor deberá volverse a unir al grupo, donde se le podría asignar a una partición distinta de la que estaba consumiendo anteriormente.

## Actividad de consumidor

Kafka detecta automáticamente los consumidores fallidos para poder reasignar particiones a los consumidores en funcionamiento. Utiliza dos mecanismos para lograrlo: sondeo y latido.

Si el lote de mensajes devueltos por `Consumer.poll(...)` es grande o el proceso tarda mucho tiempo, el retraso antes de volver a llamar a `poll()` puede ser significativo o impredecible. En algunos casos, es necesario configurar un intervalo de tiempo máximo de sondeo para que no se elimine a los consumidores de sus grupos sólo porque el procesamiento de mensajes tarda un tiempo. Si este fuera el único mecanismo, significaría que el tiempo necesario para detectar un consumidor fallido también es largo.

Para que la actividad de los consumidores sea más fácil de gestionar, se añadió el latido de fondo en Kafka 0.10.1. El coordinador del grupo espera que los miembros del grupo le envíen latidos regulares para indicar que siguen activos. Una hebra de latido de fondo se ejecuta en el consumidor y envía latidos regulares al coordinador. Si el coordinador no recibe un latido de un miembro del grupo en el _tiempo de espera de sesión_, el coordinador elimina el miembro del grupo e inicia un reequilibrio del grupo. El tiempo de espera de sesión puede ser mucho más corto que el intervalo máximo de sondeo, de modo que el tiempo necesario para detectar un consumidor fallido puede ser corto incluso si el proceso de mensajes tarda mucho tiempo.

Puede configurar el intervalo de sondeo máximo utilizando la propiedad `max.poll.interval.ms` y el tiempo de espera de sesión utilizando la propiedad `session.timeout.ms`. Normalmente, no necesitará utilizar estos valores a menos que tarde más de 5 minutos para procesar un lote de mensajes.

## Gestión de desplazamientos

Para cada grupo de consumidores, Kafka mantiene el desplazamiento comprometido para cada partición consumida. Cuando un consumidor procesa un mensaje, no lo elimina de la partición. En su lugar, sólo actualiza su desplazamiento actual utilizando un proceso denominado confirmar el desplazamiento.

{{site.data.keyword.messagehub}} retiene la información de desplazamientos confirmados durante 7 días.

### ¿Qué ocurre si no hay ningún desplazamiento confirmado?
Cuando un consumidor se inicia y se le asigna una partición para consumir, se iniciará en el desplazamiento confirmado de su grupo. Si no existe ningún desplazamiento confirmado, el consumidor puede elegir si empezar con el primero o el último mensaje disponible según el valor de la propiedad `auto.offset.reset`, como se indica a continuación:

- `latest` (valor predeterminado). El consumidor sólo recibe y consume mensajes que llegan después de suscribirse. El consumidor no tiene conocimiento de los mensajes que fueron enviados antes de suscribirse, por lo que no debería esperar que se consuman todos los mensajes de un tema.
- `earliest`. El consumidor consume todos los mensajes desde el principio porque no es consciente de todos los mensajes que se han enviado.

Si un consumidor falla después de procesar un mensaje pero antes de confirmar su desplazamiento, la información del desplazamiento confirmado no reflejará el procesamiento del mensaje. Esto significa que el siguiente consumidor de dicho grupo al que se asigne la partición volverá a procesar el mensaje.

Cuando los desplazamientos confirmados se guardan en Kafka y se reinician los consumidores, los consumidores se reanudan a partir del punto en el que se detuvieron por última vez. Cuando hay un desplazamiento confirmado, la propiedad `auto.offset.reset` no se utiliza.

### Confirmación automática de desplazamientos

La manera más fácil de confirmar desplazamientos es dejar que el consumidor Kafka lo haga automáticamente. Este método es sencillo, pero permite menos control que confirmarlos manualmente. De forma predeterminada, un consumidor confirma automáticamente los desplazamientos cada 5 segundos. Esta confirmación predeterminada se produce cada 5 segundos, independientemente del progreso que esté realizando el consumidor hacia procesar los mensajes. Además, cuando el consumidor llama a `poll()`, también se confirma el último desplazamiento devuelto desde la llamada anterior a `poll()` (porque probablemente haya sido procesado).

Si el desplazamiento confirmado supera el procesamiento de los mensajes y hay un fallo del consumidor, es posible que algunos mensajes no se procesen. Esto es debido a que el proceso se reinicia en el desplazamiento confirmado, que es después de que el último mensaje se proceso antes del error. Por esta razón, si la fiabilidad es más importante que la simplicidad, normalmente es mejor confirmar los desplazamientos manualmente.

### Confirmación manual de desplazamientos

Si `enable.auto.commit` se establece en `false`, el consumidor confirmará sus desplazamientos manualmente. Puede hacerlo de forma síncrona o asíncrona. Un patrón común es confirmar el desplazamiento del último mensaje procesado según un temporizador periódico. Este patrón significa que cada mensaje se procesa como mínimo una vez, pero el desplazamiento nunca supera el progreso de los mensajes que están procesados activamente. La frecuencia del temporizador periódico controla el número de mensajes que se pueden reprocesar tras un error del consumidor. Los mensajes se vuelven a recuperar desde el último desplazamiento confirmado que se ha guardado cuando la aplicación se reinicia o cuando el grupo se reequilibra.

El desplazamiento confirmado es el desplazamiento de los mensajes a partir del que se reanuda el procesamiento. Normalmente es el desplazamiento del mensaje procesado más recientemente *más uno*.

### Retardo de consumidor

El retardo de consumidor de una partición es la diferencia entre el desplazamiento del mensaje publicado más recientemente y el desplazamiento confirmado del consumidor. Aunque es normal que haya variaciones naturales en las velocidades de producción y consumo, la velocidad de consumo no puede ser inferior a la velocidad de producción durante un período largo.

Si observa que un consumidor está procesando mensajes correctamente pero en ocasiones parece omitir un grupo de mensajes, podría ser una señal de que el consumidor no es capaz de seguir el ritmo. En el caso de temas que no utilizan la compactación de registros, la cantidad de espacio de registro se gestiona suprimiendo periódicamente los segmentos de registro antiguos. Si un consumidor se ha quedado tan atrás que está consumiendo mensajes en un registro que se ha suprimido, saltará de repente hacia delante al inicio del siguiente segmento de registro. Si es importante que el consumidor procese todos los mensajes, este comportamiento indica la pérdida de mensajes desde el punto de vista de este consumidor.

Puede utilizar la herramienta <code>kafka-consumer-groups</code> para ver el retardo del consumidor. También puede utilizar la API de consumidor y las métricas de consumidor con el mismo propósito.


## Control de la velocidad de consumo de mensajes
{: #message_consumption_speed }

Si tiene problemas con la gestión de mensajes provocada por el desbordamiento de mensajes, puede establecer una opción de consumidor para controlar la velocidad del consumo de mensajes. Utilice `fetch.max.bytes` y `max.poll.records` para controlar el volumen de datos que puede devolver una llamada a `poll()`.


## Gestión del reequilibrio de consumidores
Cuando se añaden o se eliminan consumidores de un grupo, se produce un reequilibrio del grupo y los consumidores no pueden consumir mensajes. Esto provoca que todos los consumidores de un grupo de consumidores estén disponibles durante un período breve.

Puede utilizar un ConsumerRebalanceListener para confirmar desplazamientos manualmente (si no utiliza la confirmación automática) cuando reciba la devolución de llamada "en particiones revocadas" y detener el procesamiento hasta que se le notifique la devolución de llamada "en partición asignada".


## Fragmentos de código
{: #consumer_code_snippets notoc}

Estos fragmentos de código tienen un nivel muy alto para ilustrar los conceptos implicados. Para ver ejemplos completos, consulte los ejemplos de {{site.data.keyword.messagehub}} en GitHub https://github.com/ibm-messaging/event-streams-samples.

Para conectarse a {{site.data.keyword.messagehub}}, primero debe crear el conjunto de propiedades de configuración. Todas las conexiones a {{site.data.keyword.messagehub}} están protegidas mediante TLS y autenticación de usuario/contraseña, de modo que necesita como mínimo estas propiedades. Sustituya KAFKA_BROKERS_SASL, USER y PASSWORD por sus credenciales de servicio:

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

Para consumir mensajes, también debe especificar deserializadores para las claves y valores, por ejemplo:

```
 props.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
 props.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
```

A continuación, utilice un KafkaConsumer para consumir mensajes, donde cada mensaje está representado por un ConsumerRecord. La forma más común de consumir mensajes es poner el consumidor en un grupo de consumidores estableciendo el ID de grupo y después llamar a `subscribe()` para ver una lista de temas. Al consumidor se le asignarán algunas particiones para consumir, aunque si hay más consumidores en el grupo que particiones en el tema, es posible que no se asigne ninguna partición al consumidor. A continuación, llame a `poll()` en bucle, recibiendo un lote de mensajes para procesar, donde cada mensaje está representado por un ConsumerRecord.

```
props.put("group.id", "G1");
Consumer<String, String> consumer = new KafkaConsumer<>(props);
consumer.subscribe(Arrays.asList("T1"));
while (true) {
   ConsumerRecords<String, String> records = consumer.poll(100);
   for (ConsumerRecord<String, String> record : records)
     System.out.printf("offset = %d, key = %s, value = %s%n", record.offset(), record.key(), record.value());
}
```

Este bucle de consumidor se ejecuta para siempre, pero se puede interrumpir desde otra hebra llamando a `Consumer.wakeup()` para conseguir un cierre ordenado.

Para confirmar los desplazamientos manualmente, primero es necesario establecer la configuración de `enable.auto.commit` en `false`. A continuación, utilice `Consumer.commmitSync()` o bien `Consumer.commitAsync()` para actualizar el desplazamiento confirmado del consumidor periódicamente. Por simplicidad, este ejemplo procesa los registros para cada partición y confirma el último desplazamiento por separado.

```
props.put("group.id", "G1");
props.put("enable.auto.commit", "false");
Consumer<String, String> consumer = new KafkaConsumer<>(props);
consumer.subscribe(Arrays.asList("T1"));
try {
  while (true) {
    ConsumerRecords<String, String> records = consumer.poll(100);
    for (TopicPartition tp : records.partitions()) {
      List<ConsumerRecord<String, String>> partRecords = records.records(tp);
      long lastOffset = 0;
      for (ConsumerRecord<String, String> record : partRecords) {
        System.out.printf("offset = %d, key = %s, value = %s%n", record.offset(), record.key(), record.value());
        lastOffset = record.offset();
      }

      consumer.commitSync(Collections.singletonMap(tp, new OffsetAndMetadata(lastOffset + 1)));
    }
  }
}
finally {
  consumer.close();
}
```

## Gestión de excepciones

Cualquier aplicación robusta que utiliza el cliente Kafka necesita gestionar excepciones para determinadas situaciones previstas. En algunos casos, las excepciones no se lanzan directamente porque algunos métodos son asíncronos y entregan sus resultados utilizando un `Future` o una devolución de llamada. Encontrará un código de ejemplo en [GitHub ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-messaging/event-streams-samples) que muestra los ejemplos completos.

A continuación, se muestra una lista de excepciones que debe manejar en su código:

### org.apache.kafka.common.errors.WakeupException
Lanzado por `Consumer.poll(...)` como resultado de llamar a `Consumer.wakeup()`. Este es el método estándar para interrumpir el bucle de sondeo del consumidor. El bucle de sondeo debe salir y se debe llamar a `Consumer.close()` para desconectar correctamente.
### org.apache.kafka.common.errors.NotLeaderForPartitionException
Se lanza como resultado de `Producer.send(...)` cuando cambia el liderazgo de una partición. El cliente renueva automáticamente sus metadatos para encontrar la información de líder actualizada. Reintente la operación, que debería ejecutarse correctamente con los metadatos actualizados.
### org.apache.kafka.common.errors.CommitFailedException
Se lanza como resultado de `Consumer.commitSync(...)` cuando se produce un error irrecuperable. En algunos casos, no es posible simplemente repetir la operación porque puede haber cambiado la asignación de particiones y es posible que el consumidor ya no pueda confirmar sus desplazamientos. Puesto que `Consumer.commitSync(...)` puede tener éxito parcialmente cuando se utiliza con diversas suscripciones en una única llamada, la recuperación de errores se puede simplificar utilizando una llamada `Consumer.commitSync(...)` separada para cada partición.
### org.apache.kafka.common.errors.TimeoutException
Lanzado por `Producer.send(...),  Consumer.listTopics()` si no se pueden recuperar los metadatos. La excepción también se ve en la devolución de llamada de envío (o el Futuro devuelto) cuando el acuse de recibo solicitado no regresa en `request.timeout.ms`. El cliente puede reintentar la operación, pero el efecto de una operación repetida depende de la operación determinada. Por ejemplo, si se reintenta enviar un mensaje, el mensaje podría ser duplicado.

