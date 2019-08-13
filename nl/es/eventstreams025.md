---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-23"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}

# Uso de la API REST de productor
{: #rest_producer_using}


**La API REST de productor sólo está disponible como parte de los planes Estándar y Empresa de {{site.data.keyword.messagehub}}.**
<br/>

{{site.data.keyword.messagehub}} proporciona una API REST para ayudar a conectar los sistemas existentes al clúster Kafka de {{site.data.keyword.messagehub}}. Utilizando la API, puede integrar {{site.data.keyword.messagehub}} con cualquier sistema que dé soporte a las API RESTful.

La API REST de productor es una interfaz REST escalable para la producción de mensajes en {{site.data.keyword.messagehub}} a través de un punto final HTTP seguro. Envíe datos de sucesos a {{site.data.keyword.messagehub}}, utilice la tecnología Kafka para gestionar los canales de información de datos y aproveche las características de {{site.data.keyword.messagehub}} para gestionar los datos.

Utilice la API para conectar los sistemas existentes a {{site.data.keyword.messagehub}}. Cree solicitudes de producción desde sus sistemas a {{site.data.keyword.messagehub}}, especificando la clave de mensaje, las cabeceras y los temas en los que desea escribir mensajes.


## Generación de mensajes utilizando REST
{: #rest_produce_messages}

Utilice la API de productor para escribir mensajes en los temas. Para poder producir en un tema, debe disponer de la siguiente información:

* El URL del punto final de la API de {{site.data.keyword.messagehub}} incluyendo el número de puerto.
* El tema en el que desea producir.
* La clave de API que otorga permiso para conectarse y producir en el tema seleccionado.

Debe recuperar los detalles de URL y de credenciales necesarios para conectarse a la API desde un objeto de credenciales de servicio o una clave de servicio para la instancia de servicio. Para obtener información sobre cómo crear estos objetos, consulte
[Conexión a {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting).

El URL para el punto final de la API se proporciona en la propiedad <code>kafka_http_url</code>.

Utilice uno de los métodos siguientes para autenticarse:

* **Para autenticarse utilizando la autenticación básica:**<br/>
    Utilice las propiedades <code>user</code> y <code>api_key</code> de los objetos anteriores como nombre de usuario y contraseña. Coloque estos valores en la cabecera <code>Authorization</code> de la solicitud HTTP con el formato <code>Basic &lt;codificación en base64 de nombre de usuario y contraseña unidos por un signo de dos puntos (:)&gt;</code>.

* **Para autenticarse utilizando una señal portadora:**<br/>
    Para obtener la señal utilizando la CLI de IBM Cloud, primero inicie sesión en IBM Cloud y después ejecute el mandato siguiente: 

    ```
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

    Coloque esta señal en la cabecera de autorización de la solicitud HTTP con el formato <code>Bearer <señal></code>. Se admiten tanto señales de clave de API como señales JWT. 

* ** Para autenticarse directamente utilizando la clave_api:**<br/>
    Coloque la clave directamente como el valor de la cabecera HTTP <code>X-Auth-Token</code>.

<br/>
El código siguiente muestra un ejemplo de envío de un mensaje utilizando curl:

```
curl -v -X POST -H "Authorization: Basic <base64 username:password>" -H "Content-Type: text/plain" -H "Accept: application/json" -d 'test message' "<kafka_http_url>/topics/<topic_name>/records"
```
{: codeblock}


## Referencia de API
{: #rest_api_reference}

Para obtener información detallada completa de la API, consulte la [Referencia de API REST de productor de {{site.data.keyword.messagehub}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://ibm.github.io/event-streams/api/){:new_window}.












