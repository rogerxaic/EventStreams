---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-15"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}

# Actualización al nuevo plan Estándar de {{site.data.keyword.messagehub}} 
{: #migrate_classic_plan}

Este nuevo release del plan Estándar multiarrendatario ofrece mejoras significativas en resiliencia, funcionalidad y usabilidad. Para obtener más información, consulte [Anuncio en el blog del nuevo plan Estándar ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud/blog/announcements/ibm-event-streams-releases-a-new-and-enhanced-standard-plan). 
{: shortdesc}

Para migrar las aplicaciones del plan Estándar anterior (denominado ahora el plan Clásico) al nuevo plan, tenga en cuenta la siguiente información.

Las instancias de servicio ahora se suministran como {{site.data.keyword.cloud_notm}} Servicios en lugar de como servicios de Cloud Foundry. Esto permite al servicio dar soporte a los últimos estándares y prestaciones de {{site.data.keyword.cloud_notm}}, incluyendo los despliegues multizona y los controles de acceso granular, pero tiene implicaciones en la forma en de utilizar el servicio. Concretamente, tenga en cuenta los aspectos siguientes:

* **Creación, supresión y listado de servicios**
<br/>
    Como antes, puede gestionar el ciclo de vida de los servicios utilizando la consola de {{site.data.keyword.cloud_notm}} o la herramienta de línea de mandatos CLI de {{site.data.keyword.cloud_notm}}. Si utiliza la consola, ahora los servicios se listan bajo **Servicios** en lugar de **Servicios de Cloud Foundry**. 
    
    Si utiliza CLI, las instancias se gestionan con los mandatos de recursos. Por ejemplo,  <code>ibmcloud resource service-instance-create</code>. En lugar de los mandatos **cf**, por ejemplo <code>ibmcloud cf create-service</code>.

* **Control de acceso**
<br/>
    La autenticación y la autorización se gestionan ahora utilizando el servicio Cloud Identity and Access Management (IAM). Además de controlar la capacidad de conexión de un usuario, IAM también permite configurar el acceso granular a los recursos subyacentes, tales como por ejemplo los temas. El acceso se controla mediante la asignación de políticas a los usuarios. Para obtener más información, consulte
    [Gestión del acceso a los recursos de {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-security).

<ul>
<li><strong>Conexión de aplicaciones</strong>
<br/>
    La información que una aplicación necesita para conectarse no ha cambiado, es decir, se requiere una lista de <code>bootstrap.servers</code>, <code>username</code> y <code>password</code>. Sin embargo, la forma en que se recuperan estas propiedades ha cambiado.

<ul>
<li>
      <strong>Para aplicaciones nativas</strong>
        <br/>
        Debe crear un objeto de Credenciales o de Clave de servicio utilizando la consola o la CLI de IBM Cloud respectivamente. A continuación, puede recuperar las propiedades necesarias. Para obtener más información, consulte
        [Conexión de aplicaciones](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_external).
</li>
<br/>
<li><strong>Para aplicaciones Cloud Foundry</strong>
        <br/>
        Primero debe enlazar el servicio con la organización y el espacio de la aplicación creando un alias de servicio. A continuación, puede recuperar las propiedades necesarias de la variable de entorno VCAP_SERVICES como normalmente. Para obtener más información, consulte
        [Conexión a {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting).
</li>
</ul>
<br/>
Tenga en cuenta que los clientes deben dar soporte a la extensión SNI de TLS, donde el nombre de host del servidor se incluye en el reconocimiento de TLS. Esta característica es de disponibilidad común y está soportada en todas las versiones de cliente recomendadas en [Elección de un cliente Kafka para utilizarlo con {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_clients).
</li>
</ul>

<br/>
Debe tener presentes algunos otros cambios, como los siguientes:

* **Versión de Kafka**
<br/>
    Este plan proporciona acceso a la última versión estable de Kafka 2.2. Las aplicaciones desarrolladas contra Kafka 1.1 se pueden ejecutar sin cambios, pero consulte la siguiente información para las versiones de cliente recomendadas y las combinaciones probadas: [Elección de un cliente Kafka para utilizarlo con {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_clients). 

* **Regiones soportadas**
<br/>
    Inicialmente este plan sólo está disponible en us-south. Las demás regiones se introducirán en las próximas semanas.

* **Integraciones**
<br/>
    La conexión desde otros servicios, como por ejemplo {{site.data.keyword.iot_short_notm}} o {{site.data.keyword.openwhisk_short}}, que se enlazan al servicio utilizando una organización y un espacio de Cloud Foundry requiere la creación de un alias de servicio. Para obtener más información, consulte
    [Conexión de aplicaciones Cloud Foundry a {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_cf).
    

* **Funciones soportadas**
<br/>
    Existen diferencias entre las capacidades del plan Clásico y el nuevo plan Estándar. Para alinear las ofertas de productos, adopte nuevas opciones tecnológicas y elimine las características menos utilizadas, no se han portado todas las características. Hay disponible una comparación de las características en [Elección del plan](/docs/services/EventStreams?topic=eventstreams-plan_choose). Si necesita esas funciones, próximamente se proporcionará más información para ayudarle a migrarlas.
   
<br/>
Diariamente se envían pequeños deltas de código a producción. Como resultado, puede esperar ver muchas mejoras adicionales en la experiencia de usuario (y otras áreas) durante todo el resto de 2019 y más adelante. Próximamente:

* **Métricas de cliente**
<br/>
    La capacidad de supervisar la actividad en una instancia de servicio.

<br/>
Para ver una guía paso a paso rápida de los pasos para empezar a trabajar con el nuevo plan Estándar, pruebe la [Guía de aprendizaje de iniciación](/docs/services/EventStreams?topic=eventstreams-getting_started).


