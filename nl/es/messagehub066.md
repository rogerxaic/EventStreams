---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Cómo conectarse y autenticarse
{: #rest_connect}

**La API REST de Kafka solo está disponible como parte del plan Estándar.**
<br/>

Para conectarse a {{site.data.keyword.messagehub}}, la API REST de Kafka utiliza las credenciales <code>api_key</code> y <code>kafka_rest_url</code> de la variable de entorno [VCAP_SERVICES](/docs/services/MessageHub/messagehub127.html).

Para autenticarse con la API REST de Kafka {{site.data.keyword.messagehub}}, debe
especificar <code>api_key</code> en el encabezado X-Auth-Token de sus solicitudes.
