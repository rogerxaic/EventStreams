---

copyright:
  years: 2015, 2019
lastupdated: "2018-11-08"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Cómo enlazar con otros servicios utilizando puentes en el plan Clásico 
{: #bridges}

Los puentes de ** {{site.data.keyword.messagehub}} solo están disponibles como parte del plan Clásico.**
<br/>

El plan Clásico de {{site.data.keyword.messagehub}} también da soporte a puentes con una selección de otros sistemas. Los puentes son enlaces unidireccionales entre {{site.data.keyword.messagehub}} y otro servicio. Los puentes permiten leer datos desde {{site.data.keyword.messagehub}} y grabarlos en otro servicio, o bien leer datos desde otro servicio y escribirlos en {{site.data.keyword.messagehub}}. Un puente puede tener mensajes del otro sistema y publicarlos en un tema, o consumir mensajes de un tema y enviarlos al otro sistema. De esta forma, puede utilizar {{site.data.keyword.messagehub}} para integrar con otros sistemas sin escribir código.
{:shortdesc}

Los principales beneficios de utilizar {{site.data.keyword.messagehub}} puentes son los siguientes:  

* Puede definir puentes administrativamente, por lo que no es necesario escribir código de aplicación.
* El ciclo de cada puente es supervisado y gestionado por el servicio {{site.data.keyword.messagehub}}. Por ejemplo, si un puente encuentra un error, se reinicia automáticamente mediante {{site.data.keyword.messagehub}}.
* Los puentes se integran con la plataforma {{site.data.keyword.Bluemix_short}}. Por ejemplo, la información de registro y supervisión generada por los puentes se direcciona a los paneles de control de Kibana y Grafana.

Puede encontrar puentes útiles en los dos siguientes casos de ejemplo habituales:

* Consumo de datos de uno o varios orígenes de datos en {{site.data.keyword.messagehub}}.
* Transferencia de datos de {{site.data.keyword.messagehub}} a otro servicio. Por ejemplo, para el almacenamiento a largo plazo.

## Puentes de {{site.data.keyword.messagehub}}
{: notoc}

* Proporcionamos los siguientes tipos de puente: 
  - El [puente de MQ](/docs/services/EventStreams?topic=eventstreams-mq_bridge), que obtiene datos de mensajes de {{site.data.keyword.IBM}} MQ y los transfiere a un tema en {{site.data.keyword.messagehub}}. A largo plazo, tenemos la intención de dar soporte a una variedad más amplia de puentes.
  - El [puente de Cloud Object Storage](/docs/services/EventStreams?topic=eventstreams-cloud_object_storage_bridge), que transfiere datos de {{site.data.keyword.messagehub}} a una instancia del servicio [{{site.data.keyword.IBM_notm}} Cloud Object Storage ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/services/cloud-object-storage?topic=cloud-object-storage-about#about){:new_window}. 
  - El [puente de {{site.data.keyword.objectstorageshort}}](/docs/services/EventStreams?topic=eventstreams-object_storage_bridge) está en desuso desde el 1 de agosto de 2018. Para obtener más información, consulte el anuncio acerca del desuso: [deprecation announcement: {{site.data.keyword.objectstorageshort}} OpenStack Swift (PaaS) ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/blogs/cloud-archive/2018/05/end-marketing-object-storage-openstack-swift-paas/){:new_window}.
* Actualmente, los puentes están disponibles en todos los entornos públicos de {{site.data.keyword.Bluemix_notm}}. Los puentes no están disponibles en {{site.data.keyword.Bluemix_short}} Dedicated.
* Puede administrar puentes de las dos formas siguientes:
  - Utilizando una [API REST ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-messaging/event-streams-docs){:new_window}, que es la ampliación de la API de administración de {{site.data.keyword.messagehub}} existente. Encontrará ejemplos sobre cómo utilizar de curl para gestionar el ciclo de vida de los puentes en [message-hub-docs ![Icono de mensaje externo](../../icons/launch-glyph.svg "Icono de mensaje externo")](https://github.com/ibm-messaging/event-streams-docs){:new_window}. Es posible que, durante el desarrollo de puentes, esta API REST cambie. Intentaremos estabilizar esta API.
  - Uso del panel de control de {{site.data.keyword.messagehub}} en la consola de {{site.data.keyword.Bluemix_notm}}.
* Puede asociar un máximo de dos puentes de cualquier tipo con una instancia del servicio de {{site.data.keyword.messagehub}}. Durante el desarrollo de puentes, revisaremos esta limitación.
* No hay cargos adicionales por utilizar puentes con operaciones distintas a las de mensajería.
* El puente de MQ no da soporte al uso de SSL/TLS para proteger la privacidad y la integridad de los datos que se transfieren entre el puente y el gestor de colas de MQ. Tenemos la intención de añadir soporte para el uso de SSL/TLS en el puente. 
* Sin embargo, puede utilizar el servicio de [{{site.data.keyword.SecureGatewayfull}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/services/SecureGateway?topic=securegateway-getting-started-with-sg#getting-started-with-sg){:new_window} para enviar los datos a través de un túnel seguro entre {{site.data.keyword.Bluemix_notm}} y un cliente {{site.data.keyword.SecureGateway}} que se puede instalar localmente. En esta configuración, la comunicación en los extremos del túnel no está protegida mediante SSL/TLS.
* El puente de Cloud Object Storage concatena mensajes utilizando caracteres de nueva línea como separadores a medida que escribe los datos en Cloud Object Storage. Por este motivo, este puente no es apto para los mensajes que contienen caracteres de línea incluida y datos para mensajes binarios.
* Las convenciones de nomenclatura de objetos que actualmente utilizan el puente de Cloud Object Storage podrían cambiar en el futuro.

## Puentes de otros servicios en {{site.data.keyword.messagehub}}
{: notoc}

* {{site.data.keyword.iot_full}} proporciona su propio [puente en {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-consuming_messages). El puente proporciona un enlace unidireccional a {{site.data.keyword.messagehub}} que le permite almacenar datos históricos. Conectar {{site.data.keyword.messagehub}} con {{site.data.keyword.iot_short_notm}} significa que puede utilizar {{site.data.keyword.messagehub}} como conducto de sucesos para consumir los sucesos de su dispositivo desde Watson IoT Platform y hacer que los sucesos estén disponibles en tiempo real para el resto de la plataforma. 


