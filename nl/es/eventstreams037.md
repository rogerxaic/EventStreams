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

# Uso de la API REST de administración
{: #admin_api}

{{site.data.keyword.messagehub}} proporciona una API RESTful de administración que puede utilizar para crear, suprimir, listar y actualizar temas.
{: shortdesc}

Debe recuperar los detalles de URL y de credenciales necesarios para conectarse a la API desde un objeto de credenciales de servicio o una clave de servicio para la instancia de servicio. Para obtener información sobre cómo crear estos objetos, consulte
[Conexión a {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting).

El URL para el punto final de la API se proporciona en la propiedad <code>kafka_admin_url</code>.

Las credenciales dependen del método de autenticación y se admiten tres tipos de credenciales:

* **Para autenticarse utilizando la autenticación básica**:<br/>
    Utilice las propiedades <code>user</code> y <code>api_key</code> de los objetos anteriores como nombre de usuario y contraseña. Coloque estos valores en la cabecera <code>Authorization</code> de la solicitud HTTP con el formato <code>Basic <codificación en base64 de username:password unidos por un signo de dos puntos (:)></code>.

* **Para autenticarse utilizando una señal portadora:**<br/>
    Para obtener la señal utilizando la CLI de IBM Cloud, primero inicie sesión en IBM Cloud y después ejecute el mandato siguiente: 

    ```
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

    Coloque esta señal en la cabecera de autorización de la solicitud HTTP con el formato <code>Bearer <señal></code>. Se admiten tanto señales de clave de API como señales JWT. 

* ** Para autenticarse directamente utilizando la clave_api:**<br/>
    Coloque la clave directamente como el valor de la cabecera HTTP <code>X-Auth-Token</code>.

Para las instancias de servicio creadas en el plan Clásico, esta información está disponible en la variable de entorno VCAP_SERVICES de la aplicación.

Para obtener una descripción de la API con ejemplos, consulte
[admin-rest de {{site.data.keyword.messagehub}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-messaging/event-streams-docs/tree/master/admin-rest-api){:new_window}.

Puede descargar la especificación completa de la API del [Archivo yaml de API REST de administración de {{site.data.keyword.messagehub}}![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-messaging/event-streams-docs/blob/master/admin-rest-api/admin-rest-api.yaml){:new_window}.
Para ver el archivo swagger, utilice las herramientas de Swagger, por ejemplo el [Editor de Swagger ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://editor.swagger.io/#/){:new_window}.




