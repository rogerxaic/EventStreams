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
{:note: .note}


# Conexión a {{site.data.keyword.messagehub}}
{: #connecting}

La forma de conectar con {{site.data.keyword.messagehub}} varía según si la aplicación se ejecuta de forma nativa o como una aplicación de Cloud Foundry. No obstante, en los dos casos se requiere dos tipos de información: 
{: shortdesc}

* Los URL de punto final correspondientes a las API
* Credenciales para la autenticación

Revise la siguiente información sobre cómo obtener esta información. Los pasos pueden variar ligeramente, por lo que debe asegurarse de que ha completado los pasos adecuados para la instancia.

Para obtener información sobre cómo conectarse a {{site.data.keyword.messagehub}} si utiliza el plan Clásico, consulte [Conectar utilizando el plan Clásico](/docs/services/EventStreams?topic=eventstreams-connecting_classic).


## Visión general
{: #connect_enterprise}

Los servicios suministrados utilizando los planes Estándar o Empresa se agrupan en el panel de control bajo la cabecera **Servicios**. Estos planes están [habilitados para IAM ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/iam?topic=iam-getstarted#getstarted){:new_window}. No es necesario que entienda IAM para empezar a trabajar, pero se recomienda tener algunos conocimientos si desea proteger el servicio {{site.data.keyword.messagehub}}. Para obtener más información, consulte
[Gestión del acceso a los recursos de {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-security).

Siga los pasos siguientes para enlazar la aplicación y obtener claves de servicio para el servicio. Para tener autorización para crear temas, la aplicación o la clave de servicio deben tener un rol con acceso de gestor.

Para conectar una aplicación, el método utilizado depende de dónde se haya desplegado, es decir, en Cloud Foundry o fuera del mismo; por ejemplo, en el servicio Kubernetes.

## Suministro de una instancia de {{site.data.keyword.messagehub}}

Como requisito previo, en primer lugar debe suministrar una instancia de servicio de {{site.data.keyword.messagehub}} para el plan Estándar o Empresa. A continuación, obtenga los detalles de la conexión de API de {{site.data.keyword.messagehub}}; para ello debe llevar a cabo las siguientes tareas.

## Conectar aplicaciones 
{: #connect_enterprise_external}

Para las aplicaciones que se ejecutan fuera de Cloud Foundry, las credenciales se generan creando una clave de servicio. Cuando haya obtenido una clave de servicio, pase manualmente los detalles de la clave a la aplicación mediante el método elegido.

### Obtención de credenciales mediante la consola de IBM Cloud
{: #connect_enterprise_external_console}

1. Localice el servicio {{site.data.keyword.messagehub}} en el panel de control.
2. Pulse el mosaico del servicio.
3. Pulse **Credenciales de servicio**.
4. Pulse **Nueva credencial**. 
5. Especifique los detalles de la nueva credencial como un nombre y un rol y pulse **Añadir**. Aparece una nueva credencial en la lista de credenciales.
6. Pulse esta credencial en **Ver credenciales** para ver los detalles en formato JSON.
7. Pase estas credenciales a la aplicación. Especifique <code>token</code> como nombre de usuario y <var class="keyword varname">api_key</var> como contraseña. Separe <code>token</code>
y <var class="keyword varname">api_key</var> con un signo de dos puntos. Para obtener más información, consulte [Configuración del cliente de API de Kafka](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_api_client).
   <br/><br/>Asegúrese de que la aplicación analiza los detalles.

### Obtención de credenciales mediante la CLI de IBM Cloud
{: #connect_enterprise_external_cli}

<ol>
<li>Localice el servicio:<br/>
<code>ibmcloud resource service-instances</code></li>
<li>Cree una clave de servicio:<br/>
<code>ibmcloud resource service-key-create <var class="keyword varname">nombre_clave</var> <var class="keyword varname">rol_clave</var> --instance-name <var class="keyword varname">nombre_servicio</var></code></li>
<li>Imprima la clave de servicio:<br/>
<code>ibmcloud resource service-key <var class="keyword varname">nombre_clave</var></code></li>
<li>Pase estas credenciales a la aplicación. Especifique <code>token</code> como nombre de usuario y <var class="keyword varname">api_key</var> como contraseña. Separe <code>token</code>
y <var class="keyword varname">api_key</var> con un signo de dos puntos. Para obtener más información, consulte [Configuración del cliente](/docs/services/EventStreams?topic=eventstreams-kafka_connect).
<p>Asegúrese de que la aplicación analiza los detalles.</p></li>
</ol>

## Conectar aplicaciones de Cloud Foundry
{: #connect_enterprise_cf}

La aplicación debe estar enlazada a la instancia de servicio de {{site.data.keyword.messagehub}}. Para enlazar una aplicación de Cloud Foundry a un servicio que no sea de Cloud Foundry, cree primero un alias de servicio de Cloud Foundry y, a continuación, haga referencia a este alias desde la aplicación Cloud Foundry cuando la enlace. 

Cuando esté enlazada, los detalles de la conexión pasan a estar disponibles para la aplicación en formato JSON en la variable de entorno VCAP_SERVICES. Puede enlazar una aplicación y un servicio utilizando la [consola de IBM Cloud](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_cf_console) o la [CLI de IBM Cloud](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_cf_cli).

### Enlace de una aplicación mediante la consola de IBM Cloud
{: #connect_enterprise_cf_console}

1. Asegúrese de que está en la organización y el espacio de Cloud Foundry que desea.
2. Localice la aplicación de Cloud Foundry en el panel de control o cree una aplicación pulsando el botón **Crear recurso**.
3. Pulse el icono de la aplicación.
4. Pulse **Conexiones**.
5. Pulse **Crear conexión**.
6. Seleccione el mosaico de servicio de {{site.data.keyword.messagehub}} con el que desea enlazar y pulse **Conectar**. 
7. En la ventana **Conectar servicio habilitado para IAM** que aparece, seleccione un rol de acceso desde **Rol de acceso de conexión** y un ID de servicio desde la lista **ID de servicio para conexión** (puede aceptar el ID generado automáticamente). Pulse **Conectar**. 

  Esto crea un alias de servicio de Cloud Foundry para el servicio {{site.data.keyword.messagehub}} y luego enlaza la aplicación con este alias. 

  Vuelva a transferir la aplicación para que los cambios entren en vigor.<br/>
8. Pulse el separador **Tiempo de ejecución** a la izquierda y seleccione el separador **Variables de entorno** en el centro. Ahora puede verificar la información de VCAP_SERVICES. Ahora la aplicación puede acceder a esta información como variables de entorno. 
 

### Enlace de una aplicación mediante la CLI de IBM Cloud
{: #connect_enterprise_cf_cli}

<ol>
<li>Asegúrese de que está en la organización y el espacio de Cloud Foundry que desea. Puede navegar de forma interactiva ejecutando el mandato siguiente:<br/>
 <code>ibmcloud target --cf</code></li>
<li>Localice la app:</br>
<code>ibmcloud app list</code><br/>
<br/>
Si tiene un archivo de manifiesto, puede crear una app nueva con este mandato:<br/>
<code>ibmcloud app push</code><br/>
<br/>
Puesto que la app aún no está enlazada a {{site.data.keyword.messagehub}}, la app no puede establecer una conexión. Por lo tanto, se recomienda enviar por push la aplicación con el parámetro <code>--no-start</code> para evitar errores de conexión innecesarios.</li>
<li>Localice el servicio:</br>
<code>ibmcloud resource service-instances</code></li>
<li>Cree un alias de servicio de Cloud Foundry:<br/>
<code>ibmcloud resource service-alias-create <var class="keyword varname">nombre_alias</var> --instance-name <var class="keyword varname">nombre_servicio</var></code></li>
<li>Enlace la app con el alias de servicio creado anteriormente:<br/>
<code>ibmcloud service bind <var class="keyword varname">nombre_app</var> <var class="keyword varname">nombre_alias</var></code>.<br/>
<br/>
Como alternativa, puede actualizar el archivo de manifiesto y volver a enviar por push la aplicación.</li>
<li>Verifique que la variable de entorno VCAP_SERVICES está disponible en el tiempo de ejecución de la aplicación:<br/>
<code>ibmcloud app env <var class="keyword varname">nombre_app</var></code></li>
<li>Pase estas credenciales a la aplicación. Especifique <code>token</code> como nombre de usuario y <var class="keyword varname">api_key</var> como contraseña. Separe <code>token</code>
y <var class="keyword varname">api_key</var> con un signo de dos puntos. Para obtener más información, consulte [Configuración del cliente](/docs/services/EventStreams?topic=eventstreams-kafka_connect). 
<p>Es posible que tenga que volver a transferir la aplicación para que los cambios entren en vigor.</p></li>
</ol>


## Qué hacer a continuación
{: #after_connecting}

Ahora que tiene la información de conexión y de credenciales, puede elegir un cliente de {{site.data.keyword.messagehub}}. Para obtener más información, consulte [Utilización de la API de Kafka](/docs/services/EventStreams?topic=eventstreams-kafka_using).

<!--
Charlie said:

"Add some info describing how to take the information made available from above e.g. like the info in the Connecting a client to the Kafka API section of the alpha docs on stage 1? https://console.stage1.bluemix.net/docs/services/EventStreams/eventstreams122.html#alpha_about "
-->







 















