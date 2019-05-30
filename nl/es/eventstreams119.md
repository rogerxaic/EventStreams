---

copyright:
  years: 2015, 2019
lastupdated: "2018-06-01"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Puente de Watson IoT Platform
{: #consuming_messages_watson}

{{site.data.keyword.iot_full}} proporciona un puente gestionado que le permite crear un enlace unidireccional a {{site.data.keyword.messagehub_full}}.
{: shortdesc}

Conectar {{site.data.keyword.messagehub}} con {{site.data.keyword.iot_short_notm}} significa que puede utilizar {{site.data.keyword.messagehub}} como conducto de sucesos para consumir los sucesos de su dispositivo desde {{site.data.keyword.iot_short_notm}} y hacer que los sucesos estén disponibles en tiempo real para el resto de la plataforma. 

Un patrón común es utilizar el puente de {{site.data.keyword.iot_short_notm}}, {{site.data.keyword.messagehub}} y el puente de Cloud Object Storage para crear un conducto global que facilite las interacciones en tiempo real y por lotes.

Para obtener información sobre cómo crear este puente, consulte: [Conexión y configuración de un conector de historiador para utilizar {{site.data.keyword.messagehub}}  ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/support/knowledgecenter/SSQP8H/iot/platform/message_hub.html){:new_window}.






