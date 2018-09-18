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

# Uso del cliente Kafka Java
{: #kafka_using}

<!-- 21/06/18 - removing until some content ready

## To do: instructions for getting started, with links for more information


## To do: simple send source and receive source in-line


## How to use, download, and run the Java Kafka API sample

-->

La API Java&trade; Kafka es una muestra de productor y consumidor escrita en Java, que utiliza directamente la API Kafka. Este ejemplo puede ejecutarse de manera local o en {{site.data.keyword.Bluemix_short}}.

El código de ejemplo está en [message-hub-samples GitHub project ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-messaging/message-hub-samples/tree/master/kafka-java-console-sample){:new_window}. A pesar de que el ejemplo
utiliza la API Kafka para enviar y recibir mensajes, el
ejemplo utiliza la API de administración de
{{site.data.keyword.messagehub}}
para crear un tema desde el cual envía y recibe mensajes.

Para obtener más información sobre cómo configurar y ejecutar el ejemplo, consulte [README.md ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-messaging/message-hub-samples/tree/master/kafka-java-console-sample){:new_window}.

Para obtener una explicación detallada sobre cómo ejecutar el ejemplo, consulte [Iniciación a {{site.data.keyword.messagehub}}](/docs/services/MessageHub/index.html#getting_started_steps).

## Cómo utilizar, descargar y ejecutar el ejemplo de Liberty for Java
{: #liberty_sample notoc}

El ejemplo de Liberty for Java implementa una aplicación simple que se despliega en el tiempo de ejecución de Liberty. La aplicación utiliza la API Kafka para que {{site.data.keyword.messagehub}} produzca y consuma mensajes.
La aplicación también sirve un frontal web que se puede utilizar para la administración.

El código de ejemplo está en el [proyecto GitHub message-hub-samples ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-messaging/message-hub-samples/tree/master/kafka-java-liberty-sample){:new_window}.

<!--
17/10/17 - Karen: following info duplicated at messagehub063 
-->

## Uso de la propiedad sasl.jaas.config
{: #sasl_prop notoc}
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

<!--
23/04/18 - Karen: following migration info on production in messagehub084 
-->

## Migración de un cliente Kafka de 0.9.X o 0.10.X a versiones de cliente posteriores
{: #kafka_migrate}


Si está utilizando los clientes Java, puede utilizar los clientes Kafka públicamente disponibles en 0.10 o posterior. Se recomienda encarecidamente actualizar la versión 0.9.X a la última
versión. Puede descargar un cliente Kafka desde
[https://kafka.apache.org/downloads ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://kafka.apache.org/downloads){:new_window} 



### Migración de un cliente Kafka a 0.10.2.X o versiones posteriores

Desde 0.10.2, puede configurar la autenticación SASL directamente en las propiedades del cliente en lugar de utilizar un archivo JAAS. Esta simplificación le permite ejecutar varios clientes en la misma JVM utilizando conjuntos distintos de credenciales, lo que no es posible con un archivo JAAS.

Siga estos pasos:

1. Suprima el archivo JAAS. Tenga en cuenta que la propiedad de JVM java.security.auth.login.config=<PATH TO JAAS> tampoco es necesaria ya.
2. Si está migrando desde 0.9.X, suprima el módulo jar de inicio de sesión de {{site.data.keyword.messagehub}}.
2. Añada lo siguiente a las propiedades del cliente:
    ```
	sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
	```

	donde USERNAME y PASSWORD son los valores del separador de {{site.data.keyword.messagehub}} **Credenciales de servicio** en {{site.data.keyword.Bluemix_notm}}.
	
	

### Migración de un cliente Kafka de 0.9.X a 0.10.0.X o 0.10.1.X

Siga estos pasos:

1. Suprimir el módulo jar de inicio de sesión de {{site.data.keyword.messagehub}}.
2. Cambie el archivo <code>jaas.conf</code> de la siguiente forma:
    ```
        KafkaClient {
          org.apache.kafka.common.security.plain.PlainLoginModule required
          serviceName="kafka"
            username="USERNAME"
            password="PASSWORD";
        };
    ```
    {: codeblock}

	donde USERNAME y PASSWORD son los valores del separador de {{site.data.keyword.messagehub}} **Credenciales de servicio** en {{site.data.keyword.Bluemix_notm}}.
	
3. Añada esta línea a las propiedades de consumidor y productor: <code>sasl.mechanism=PLAIN</code>
