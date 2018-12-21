---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-07"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# ¿Qué es {{site.data.keyword.messagehub}}?
{: #about}

{{site.data.keyword.messagehub_full}} es un bus de mensajes de alto rendimiento creado con Apache Kafka. Está optimizado para la ingestión de sucesos en {{site.data.keyword.Bluemix_notm}} y para la distribución de secuencias de sucesos entre los servicios y las aplicaciones. {{site.data.keyword.messagehub}} se conocía anteriormente como Message Hub.
{: shortdesc}

Puede utilizar {{site.data.keyword.messagehub}} para completar las tareas siguientes:

* Trabajo de descarga a aplicaciones de trabajador back-end.
* Conectar la secuencia de sucesos a Streaming Analytics para obtener información muy valiosa.
* Publicar datos de sucesos a múltiples aplicaciones para
reaccionar
en tiempo real.
* Transferir datos a otro servicio. Por ejemplo, a Cloud Object Storage.

Al estar creado con Apache Kafka, se beneficia directamente de toda la innovación que se incorpora en la comunidad y da soporte a las API de cliente Kafka, Kafka Streams, Kafka Connect y también KSQL.
{:shortdesc}

Las herramientas de Apache Kafka suelen funcionar directamente con {{site.data.keyword.messagehub}}, aunque no necesita proporcionar una configuración adicional ya que las conexiones con {{site.data.keyword.messagehub}} siempre se autentican mediante credenciales.

En {{site.data.keyword.messagehub}}, las aplicaciones envía datos creando un mensaje y enviándolo a un tema. Para recibir mensajes, las aplicaciones se suscriben a un tema y eligen si desean recibir todos los mensajes de los temas o compartir mensajes entre sí.
{{site.data.keyword.messagehub}} aloja y mantiene los mensajes en una secuencia ordenada. 




