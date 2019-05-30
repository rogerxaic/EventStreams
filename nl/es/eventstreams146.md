---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-15"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Migración de un cliente Kafka Java en el plan Clásico 
{: #kafka_java_migrating_classic}


## Migración de un cliente Kafka Java de la versión 0.9.X o 0.10.X a una versión de cliente posterior
{: #kafka_migrate}


Si está utilizando los clientes Java, puede utilizar los clientes Kafka públicamente disponibles en 0.10 o posterior. 

Se recomienda encarecidamente actualizar la versión 0.9.X a la última
versión. Puede descargar un cliente Kafka desde [https://kafka.apache.org/downloads ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://kafka.apache.org/downloads){:new_window}.

Para obtener más información sobre las implicaciones de utilizar un cliente 0.9.X, consulte [Compatibilidad con versiones anteriores](/docs/services/EventStreams?topic=eventstreams-kafka_clients#compatibility).



### Migración de un cliente Kafka Java a la versión 0.10.2.X o a versiones posteriores

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
