---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

<!-- Notes from chat with Charlie 

Different plan for provisioning

Quality of service from each plan

Life of a user through cycle - APIs, feature sets

-->

# Acerca del programa Alpha
{: #alpha_about }

El programa Alpha proporciona un acceso previo a las nuevas características del servicio de {{site.data.keyword.messagehub}}. La característica actual es la introducción de un plan nuevo, el plan Premium de {{site.data.keyword.messagehub}}.

## Plan Premium
{: premium_plan}

El plan Premium está diseñado para usuarios que tienen requisitos de rendimiento y otros requisitos funcionales que van más allá del servicio público. Proporciona una versión de un solo arrendatario del servicio de {{site.data.keyword.messagehub}}, donde tiene un uso exclusivo de un clúster de Apache Kafka. Esta versión le permite:

* Sacar el máximo partido a la capacidad y al rendimiento del clúster

* Tener límites y número de particiones muy aumentados

* Benefíciese de cero costes de gestión con un clúster completamente gestionado que se mantiene y actualiza automáticamente

## Acerca del clúster Alpha
{: alpha_cluster}

El clúster Alpha se despliega con Apache Kafka versión 1.1 y puede suministrar una capacidad de proceso de mensajes máxima de 90.000 KB por segundo. 

Puede crear un máximo de 1.000 particiones, y cada partición puede retener un máximo de 1 GB de datos durante un máximo de 30 días. Para la resiliencia, los datos se almacenan en 3 réplicas y el desplazamiento confirmado para cada partición se mantiene durante un máximo de 7 días.

Se da soporte a las dos API siguientes:

* Para la mensajería, están soportados los clientes Kafka de versión 0.10.x y posterior, incluida la opción de utilizar Kafka Streams, Kafka Connect y KSQL.

* Para la administración, hay disponible una API REST para crear, suprimir y listar temas.

El programa Alpha solo está disponible en la región EE.UU. Sur. El acceso a los clústeres se gestiona utilizando IAM.

## Conexión a su clúster
{: alpha_connect}

Para conectarse a una API en el clúster, necesita el URL de su punto final y una clave de API para la autenticación. Puede recuperar estos detalles desde IAM utilizando uno de los métodos siguientes:

### Aplicación de Cloud Foundry
Para una aplicación de Cloud Foundry:
1. En el separador **Conexiones** para la app (a la izquierda), pulse el botón **Crear conexión**. 
2. Seleccione la instancia del servicio de {{site.data.keyword.messagehub}} al que desee conectarse y pulse **Conectar**. Acepte las opciones predeterminadas. 
3. Cuando la conexión haya finalizado, pulse el separador **Tiempo de ejecución** para la app, y luego pulse **Variables de entorno** para mostrar **VCAP_SERVICES**.

### Consola para una aplicación externa
Desde la consola para una aplicación externa, cree una clave de API de servicio utilizando el mandato **bx** siguiente: 

```
bx resource service-key-create name-of-key Manager --instance-name name-of-your-service
``` 

Copie los campos <code>kafka_brokers_sasl</code>, <code>kafka_admin_url</code> y <code>apikey</code> desde la información generada.
Solo se listarán los primeros cinco intermediarios en VCAP_SERVICES. Si tiene más de cinco intermediarios, utilice un cliente Kafka para recuperar los detalles del resto de los intermediarios. 

## Conexión de un cliente a la API Kafka

Para conectar un cliente a la API Kafka, siga estos pasos:

1. Establezca la propiedad <code>bootstrap.servers</code> de los clientes a una lista separada por comas de los intermediarios listados en <code>kafka_brokers_sasl</code>.

2. Establezca el campo USERNAME <code>sasl.jaas.config</code> de los clientes en los primeros 8 caracteres de la <code>apikey</code>, y el campo PASSWORD en los caracteres restantes (esta partición no se necesitará en versiones futuras)

El cliente Kafka que utilice debe soportar las características siguientes:

* Versión de cliente 0.10.x o posterior

* Autenticación utilizando el mecanismo SASL Plain

* La extensión de la SNI (Service Name Identification, identificación de nombre de servicio) en el protocolo TLS v1.2

Este método de recuperar la información del punto final y de las credenciales difiere del servicio existente de {{site.data.keyword.messagehub}}. Las apps que se ejecutan actualmente en {{site.data.keyword.messagehub}} requerirán cambios para reflejar los nombres de campos alternativos necesarios desde VCAP_SERVICES y para los campos nombre de usuario/contraseña enviados a Kafka. Estos cambios no serán necesarios en versiones futuras de Alpha.

## Conexión de un cliente a la API REST

Para conectar un cliente a la API REST, siga estos pasos:

* El URI para la API REST se proporciona en <code>kafka_admin_url</code>

* Establezca la cabecera HTTP <code>Content-Type</code> en <code>application/json</code>

* Establezca la cabecera HTTP <code>X-Auth-Token</code> en el valor de <code>apikey</code>

Para que los pasos sencillos se preparen y se ejecuten con Alpha, consulte [Iniciación al programa Alpha](/docs/services/MessageHub/messagehub120.html).


## Administración de {{site.data.keyword.messagehub}}
{: alpha_admin}

Las únicas tareas de administración necesarias en un clúster son crear, listar y suprimir los temas que necesite. Puede administrar utilizando uno de los métodos siguientes:

* Las API de administración de Kafka directamente desde su aplicación. Por ejemplo, para Java, utilizando los métodos <code>createTopics()</code>, <code>deleteTopics()</code> o <code>listTopics()</code> desde [AdminClient ![Icono de enlace externo](http://kafka.apache.org/11/javadoc/index.html?org/apache/kafka/clients/admin/AdminClient.html){:new_window}.

* Interactivamente, utilizando la IU web para la instancia de servicio disponible en el portal de IBM Cloud.

* La API REST de administrador proporcionada en el clúster.

Puede encontrar más detalles sobre las funciones crear, listar y suprimir proporcionadas por la API REST de administrador (que es compatible con la API de administrador existente de {{site.data.keyword.messagehub}}) en la especificación completa para la API disponible desde [admin-rest-api.yaml ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-messaging/message-hub-docs/blob/master/admin-rest-api/admin-rest-api.yaml){:new_window}.
Para ver el archivo swagger, utilice las herramientas de Swagger, por ejemplo el [Editor de Swagger ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://editor.swagger.io/#/){:new_window}.

Para obtener un ejemplo sencillo que demuestra cómo crear un tema utilizando Curl, consulte [Iniciación al programa de Alpha](/docs/services/MessageHub/messagehub120.html).

En el futuro, también estarán disponibles otras opciones de configuración.


## Muestras

Próximamente...

Para que los pasos sencillos se preparen y se ejecuten con Alpha, consulte [Iniciación al programa Alpha](/docs/services/MessageHub/messagehub120.html).

## Limitaciones de Alpha
{: alpha_limitations}

Las limitaciones actuales de este programa Alpha son las siguientes:

### No disponibles aún, pero lo estarán próximamente

* Soporte para varias zonas de disponibilidad

* Límites de escalado y de carga controlados por el usuario

* Integración con el servicio de {{site.data.keyword.cloudaccesstrailfull_notm}} (previamente conocido como AccessTrail) 

### No planeado actualmente

* Puentes

* Mensajería de REST

* API MQ Light










