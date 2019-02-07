---

copyright:
  years: 2015, 2019
lastupdated: "2018-05-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

<!-- 14/11/18: info moved to eventstreams075.md, moved because of doc app changes -->
# Cómo conectar con aplicaciones MQ Light existentes
{: #mql_exist_apps}

**La API MQ Light solo está disponible como parte del plan Estándar.**
<br/>

Puede conectar al servicio aplicaciones existentes que actualmente se ejecuten en {{site.data.keyword.IBM_notm}} MQ o en el servicio {{site.data.keyword.mql}} {{site.data.keyword.Bluemix_notm}}. Las apps seguirán funcionando del mismo modo.
{:shortdesc}

Para conectar apps existentes, realice las siguientes comprobaciones:

* Asegúrese de que la app esté utilizando la versión de cliente de API {{site.data.keyword.mql}} para su idioma.
* Compruebe que los detalles de conexión extraídos de VCAP_SERVICES hagan referencia al tipo de servicio <code>messagehub</code> y recupere el nombre de usuario de las conexiones de la propiedad <code>credentials.user</code> en lugar de hacerlo de la propiedad <code>credentials.username</code> y recupere el URL de búsqueda de conexión de la propiedad <code>credentials.mqlight_lookup_url</code> en lugar de recuperarlo de la propiedad <code>credentials.connectionLookupURI</code>. Para obtener más información, consulte [Variable de entorno VCAP_SERVICES](/docs/services/EventStreams/eventstreams127.html).

	Tenga en cuenta que este paso se realiza en su nombre si se utiliza el cliente Java &trade; y se especifica 'null' como parámetro endpointService en la llamada create(), de modo que el cliente recupera la información por sí mismo.
	
* La app debe soportar las conexiones TLS v1.2. VCAP_SERVICES ya no incluye la propiedad <code>credentials.nonTLSConnectionLookupURI</code> para crear una conexión no TLS.

También debe tener en cuenta la siguiente información:

* Los límites de mensajes son coherentes con {{site.data.keyword.messagehub}}, pero podrían ser diferentes de otros servidores que soportan la API {{site.data.keyword.mql}}. Para obtener más información, consulte [Límites máximos](/docs/services/EventStreams/eventstreams083.html).
* JMS no está soportado.
