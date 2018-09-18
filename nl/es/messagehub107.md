---

copyright:
  years: 2015, 2018
lastupdated: "2017-09-26"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# ¿Qué es Message Hub?

{{site.data.keyword.messagehub}} implementa la mensajería de publicación/suscripción utilizando temas. Al contrario de lo que sucede con los sistemas de mensajería tradicionales, {{site.data.keyword.messagehub}} proporciona temas pero no colas. {{site.data.keyword.messagehub}} también ofrece funciones adicionales, como puentes con otros sistemas.

{{site.data.keyword.messagehub}} se basa en el proyecto de código abierto Apache Kafka, una red troncal de mensajería de alto rendimiento y sumamente escalable probada en numerosos entornos de producción. Para obtener más información, consulte [{{site.data.keyword.messagehub}} y Apache Kafka](/docs/services/MessageHub/messagehub073.html).
Las herramientas de Apache Kafka suelen funcionar directamente con {{site.data.keyword.messagehub}}, aunque no necesita proporcionar una configuración adicional ya que las conexiones con {{site.data.keyword.messagehub}} siempre se autentican mediante credenciales.

{{site.data.keyword.messagehub}} tiene tres API: la API Kafka, la API REST de Kafka y la API {{site.data.keyword.mql}}.
En la mayoría de los casos, la API Kafka constituye la mejor opción. Para obtener más información sobre cómo crear aplicaciones de mensajería utilizando las API, consulte [Utilización de un cliente Kafka](/docs/services/MessageHub/messagehub050.html), [Utilización de la API REST de Kafka](/docs/services/MessageHub/messagehub025.html) y [Utilización de un cliente API MQ Light](/docs/services/MessageHub/messagehub075.html).

{{site.data.keyword.messagehub}} también soporta puentes con otros sistemas seleccionados. Un puente es un enlace unidireccional a otro sistema. Un puente puede tener mensajes del otro sistema y publicarlos en un tema, o consumir mensajes de un tema y enviarlos al otro sistema. De esta forma, puede utilizar {{site.data.keyword.messagehub}} para integrar con otros sistemas sin escribir código. Para más información, consulte [Cómo enlazar con otros servicios utilizando puentes](/docs/services/MessageHub/messagehub088.html).
