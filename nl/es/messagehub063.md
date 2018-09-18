---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-22"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Configuración del cliente
{: #kafka_connect}


Para conectarse a {{site.data.keyword.messagehub}}, la API de Kafka utiliza uno de los siguientes conjuntos de información de credenciales: 
* las credenciales <code>kafka_brokers_sasl</code> y los valores de <code>user</code> y <code>password</code> de la [variable de entorno VCAP_SERVICES](/docs/services/MessageHub/messagehub127.html#vcap).
* la clave de servicio. Para obtener más información, consulte [Conexión con el clúster](/docs/services/MessageHub/messagehub127.html#enterprise_connect).


<!--17/10/17 - Karen: following info duplicated at messagehub104 -->
## Utilización de la propiedad sasl.jaas.config (conexión y autenticación en una aplicación Java)
{: #kafka_java notoc}
Si está utilizando un cliente Kafka de la versión 0.10.2.1 o posterior, puede utilizar la propiedad <code>sasl.jaas.config</code> para la configuración del cliente en lugar de un archivo JAAS. Para conectarse a {{site.data.keyword.messagehub}}, establezca <code>sasl.jaas.config</code> de la siguiente manera:
<pre>
<code>    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
    username="USERNAME" \
    password="PASSWORD";</code>
</pre>
{:codeblock}

donde USERNAME y PASSWORD son los valores del separador de {{site.data.keyword.messagehub}} **Credenciales de servicio** en {{site.data.keyword.Bluemix_notm}}.

Si utiliza <code>sasl.jaas.config</code>, los clientes que se ejecuten en la misma JVM podrán utilizar credenciales diferentes. Para obtener más información, consulte [Configuración de clientes Kafka ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://kafka.apache.org/documentation/#security_sasl_plain_clientconfig){:new_window}

Para un cliente Kafka anterior, debe utilizar el archivo de configuración JAAS para especificar las credenciales. Este mecanismo le resultará menos cómodo, por lo que recomendamos utilizar en su lugar la propiedad <code>sasl.jaas.config</code>.
## Conexión y autenticación en una aplicación que no sea Java
{: #kafka_notjava notoc}

El servicio {{site.data.keyword.messagehub}} actualmente autentica los clientes utilizando
SASL PLAIN sobre TLS. Las credenciales se envía a través de una conexión cifrada.
Esta es una nueva característica añadida en Kafka 0.10.0.X. 

El siguiente ejemplo es un archivo de configuración de muestra denominado <code>consumer.properties</code>:

```
key.deserializer=org.apache.kafka.common.serialization.StringDeserializer
value.deserializer=org.apache.kafka.common.serialization.StringDeserializer
#
client.id=kafka-java-console-sample-consumer
group.id=kafka-java-console-sample-group
#
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
ssl.protocol=TLSv1.2
ssl.enabled.protocols=TLSv1.2
ssl.endpoint.identification.algorithm=HTTPS
#
# please read the Kafka docs about this setting
auto.offset.reset=latest
```
{: codeblock}

Cualquier cliente que soporte Kafka 0.10 con SASL PLAIN debe trabajar con {{site.data.keyword.messagehub}}. Los clientes de ejemplo son los siguientes:

* [librdkafka ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/edenhill/librdkafka/){:new_window} 
* [confluent-kafka-python ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/confluentinc/confluent-kafka-python){:new_window} 

Si utiliza un cliente Kafka 0.9.0.0 anterior, debe utilizar un módulo de inicio de sesión personalizado, que puede descargar desde [módulo de inicio de sesión de {{site.data.keyword.messagehub}}![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-messaging/message-hub-samples/blob/master/kafka-0.9/message-hub-login-library/messagehub.login-1.0.0.jar){:new_window}. 

