---

copyright:
  years: 2015, 2019
lastupdated: "2018-12-07"
keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}



# Notificación de un problema al equipo de {{site.data.keyword.messagehub}} - plan Estándar
{: #report_problem}

Si tiene problemas con {{site.data.keyword.messagehub}}, compruebe primero la página de estado de [{{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/status?selected=status){:new_window}. 

Si desea ayuda del equipo de {{site.data.keyword.messagehub}}, recopile toda la información siguiente. Cuanta más información proporcione de antemano, más eficientemente podrá ayudar el equipo con el problema:
{:shortdesc}

1. ¿En qué ubicación (región) de {{site.data.keyword.Bluemix_notm}} se ha suministrado la instancia de {{site.data.keyword.messagehub}}?  Por ejemplo, Dallas o Londres. 
2. ¿Qué interfaz presenta problemas? ¿Kafka, REST, AMQP o puentes?
3. ¿Cuándo se produjo el primer problema (específicamente hora, fecha y huso horario)? ¿Cuánto tiempo estuvo ejecutando la app antes del problema?
4. ¿El problema sigue ocurriendo? ¿Lo puede replicar?
5. ¿Qué ID de instancia del servicio {{site.data.keyword.messagehub}} está utilizando? 
Puede encontrar este ID buscando en el campo "instance_id" en el JSON VCAP_SERVICES del servicio. Por ejemplo:
 ```
 "instance_id": "e662a61b-d1ff-4cce-aab9-5eae9adb9827"
 ```
6. ¿Qué cliente Kafka o REST utiliza la aplicación? ¿Cuáles son los detalles de la versión?
7. ¿Cuáles son los detalles de la configuración de cliente?
8. ¿Hay fragmentos del registro de aplicaciones con el mismo problema?
9. ¿Cuál es el problema que ve? ¿Qué temas, ID de cliente e ID de grupo se han visto afectados?
10. ¿Qué impacto tiene el problema en el servicio?


Puede proporcionar la información que ha obtenido a IBM en una incidencia de soporte [mediante el envío de una solicitud de soporte ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){:new_window}.










