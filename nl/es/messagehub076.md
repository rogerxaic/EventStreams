---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-30"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# ¿Por qué utilizar la API {{site.data.keyword.mql}} ?
{: #why_mql}

**La API MQ Light solo está disponible como parte del plan Estándar.**
<br/>

La API {{site.data.keyword.mql}} proporciona un nivel superior de abstracción que la API Kafka. {{site.data.keyword.mql}} permite escribir las apps de forma portátil en un modelo de mensajería unificada que admite tanto los patrones de mensajería en cola como los de publicación/suscripción. 
{:shortdesc}

Las apps se intercambian mensajes utilizando destinos creados dinámicamente, que se pueden estructurar de forma jerárquica (por ejemplo, <code>'/sports/football'</code>), agrupar utilizando comodines (por ejemplo,
<code>‘/sports/#’</code>) y a los que se pueden añadir controles para garantizar la entrega y controlar la caducidad del mensaje.
Esto permite implementar escenarios como descarga de trabajador, notificación de sucesos y procesamiento por lotes.

Además de enviar mensajes entre otras apps utilizando la API {{site.data.keyword.mql}}, también se pueden intercambiar mensajes con apps que utilizan la API de Kafka o la API REST de Kafka. Como alternativa, también puede utilizar las apps en sistemas con {{site.data.keyword.IBM}} MQ.

