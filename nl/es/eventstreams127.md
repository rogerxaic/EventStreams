---

copyright:
  years: 2015, 2019
lastupdated: "2018-11-07"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Conexión a {{site.data.keyword.messagehub}}
{: #connecting}

La forma de conectar con {{site.data.keyword.messagehub}} varía en función de si utiliza el plan Estándar o Empresa y también de si se conecta desde una aplicación de Cloud Foundry o desde cualquier otro cliente externo. Debe recopilar dos fragmentos de información para conectarse a cualquiera de las API de {{site.data.keyword.messagehub}}:
{: shortdesc}

* Los URL de punto final correspondientes a las API
* Credenciales para la autenticación

Revise la siguiente información sobre cómo obtener esta información. Los pasos pueden variar ligeramente, por lo que debe asegurarse de que ha completado los pasos adecuados para la instancia.

## Suministro de una instancia de {{site.data.keyword.messagehub}}

Como requisito previo, en primer lugar debe suministrar una instancia de servicio de {{site.data.keyword.messagehub}} para el plan Estándar o Empresa. A continuación, obtenga los detalles de la conexión de API de {{site.data.keyword.messagehub}}; para ello debe llevar a cabo las siguientes tareas.

## Visión general del plan Estándar
{: #connect_standard}

Los servicios que se suministran mediante el plan Estándar son servicios de Cloud Foundry. Esto significa que se despliegan en una organización y espacio de Cloud Foundry, y se agrupan en el panel de control bajo la cabecera **Servicios de Cloud Foundry**. El método que utilice para conectar una aplicación dependerá de dónde se haya desplegado, es decir, en Cloud Foundry o fuera del mismo; por ejemplo, en el servicio Kubernetes.


## Aplicaciones de Cloud Foundry en el plan Estándar
{: #connect_standard_cf}

Para las apps que se ejecutan dentro de Cloud Foundry, enlace la app a la instancia de servicio de {{site.data.keyword.messagehub}}. Cuando esté enlazada, los detalles de la conexión pasan a estar disponibles para la app en formato JSON en la variable de entorno VCAP_SERVICES. Puede enlazar una app y un servicio mediante la consola de IBM Cloud o la CLI de IBM Cloud.

A continuación se muestra un ejemplo de VCAP_SERVICES:

```
{
  "credentials": {
    "mqlight_lookup_url": "https://mqlight-lookup.messagehub.services.us-south.bluemix.net/Lookup?serviceId=584e8436-e7f5-43db-96ac-2864fccae5ae",
    "api_key": "d9JSx1SYsmLzNRbbgUFneDm2DtkedlVeViObYJIvrPAf2kJA",
    "kafka_admin_url": "https://kafka-admin.messagehub.services.us-south.bluemix.net:443",
    "kafka_rest_url": "https://kafka-rest.messagehub.services.us-south.bluemix.net:443",
    "kafka_brokers_sasl": [
      "kafka01.messagehub.services.us-south.bluemix.net:9093",
      "kafka02.messagehub.services.us-south.bluemix.net:9093",
      "kafka03.messagehub.services.us-south.bluemix.net:9093",
      "kafka04.messagehub.services.us-south.bluemix.net:9093",
      "kafka05.messagehub.services.us-south.bluemix.net:9093"
    ],
    "user": "d9JSx1SYsmLzNRbb",
    "password": "gUFneDm2DtkedlVeViObYJIvrPAf2kJA"
  }
}
```

{: codeblock}

El contenido de la variable de entorno es el mismo, independientemente de la API que utilice para conectarse a {{site.data.keyword.messagehub}}. Su
app {{site.data.keyword.Bluemix_notm}} selecciona las credenciales correctas de la variable de entorno VCAP_SERVICES dependiendo de la interfaz en uso.
 
Solo se listarán los primeros cinco intermediarios en VCAP_SERVICES. Si tiene más de cinco intermediarios, utilice un cliente Kafka para recuperar los detalles del resto de los intermediarios. 


### Obtención de credenciales y conexión mediante la consola de IBM Cloud
{: #connect_standard_cf_console }

1. Asegúrese de que está en la organización y el espacio de Cloud Foundry que desea.
2. Localice la aplicación Cloud Foundry en el panel de control. Si aún no tiene una aplicación de Cloud Foundry, puede crear una pulsando el botón **Crear recurso**.
3. Pulse el icono de la aplicación.
4. Pulse **Conexiones**.
5. Pulse **Crear conexión**.
6. Seleccione el mosaico de servicio de {{site.data.keyword.messagehub}} con el que desea enlazar y pulse **Conectar**. Es posible que tenga que volver a transferir la aplicación para que los cambios entren en vigor.
7. Pulse el separador **Tiempo de ejecución** a la izquierda y seleccione el separador **Variables de entorno** en el centro. Ahora puede verificar la información de VCAP_SERVICES y ahora la aplicación puede acceder a esta información como variables de entorno. 


### Obtención de credenciales mediante la CLI de IBM Cloud 
{: #connect_standard_cf_cli }

<ol>
<li>Asegúrese de que está en la organización y el espacio de Cloud Foundry que desea. Puede navegar de forma interactiva ejecutando el mandato siguiente:<br/>
<code>ibmcloud target --cf</code>
</li>
<li>Busque la app:<br/> <code>ibmcloud app list</code> <br/>
</br>
Si tiene un archivo de manifiesto, puede crear una app nueva con este mandato:</br>
<code>ibmcloud app push</code>
</li>
<li>Busque el servicio:</br> 
<code>ibmcloud service list</code>
</li>
<li>Enlace la app con el servicio:</br>
<code>ibmcloud service bind <var class="keyword varname">nombre_app</var> <var class="keyword varname">nombre_servicio</var></code>
</li>
<li>Verifique que la variable de entorno VCAP_SERVICES está disponible en el tiempo de ejecución de la aplicación con este mandato:</br> 
 <code>ibmcloud app env <var class="keyword varname">nombre_app</var></code>. 
</li>
<li>Pase estas credenciales a la aplicación. Especifique <code>token</code> como nombre de usuario y <var class="keyword varname">api_key</var> como contraseña. Separe <code>token</code>
y <var class="keyword varname">api_key</var> con un signo de dos puntos. Para obtener más información, consulte [Configuración del cliente](/docs/services/EventStreams/eventstreams063.html).
<p>Es posible que tenga que volver a transferir la aplicación para que los cambios entren en vigor.</p></li>
</ol>

## Aplicaciones externas en el plan Estándar
{: #connect_standard_external}

Para las aplicaciones que se ejecutan fuera de Cloud Foundry, las credenciales se generan creando una clave de servicio. Cuando haya obtenido una clave de servicio, pase manualmente los detalles de la clave a la aplicación mediante el método elegido.

### Obtención de credenciales mediante la consola de IBM Cloud
{: #connect_standard_external_console}

1. Asegúrese de que está en la organización y el espacio de Cloud Foundry que desea.
2. Localice el servicio {{site.data.keyword.messagehub}} de Cloud Foundry en el panel de control.
3. Pulse el mosaico del servicio.
4. Pulse **Credenciales de servicio**.
5. Pulse **Nueva credencial**.
6. Especifique los detalles de la nueva credencial como un nombre y pulse **Añadir**. Aparece una nueva credencial en la lista de credenciales.
7. Pulse esta credencial en **Ver credenciales** para ver los detalles en formato JSON.
8. Pase estas credenciales a la aplicación. Especifique <code>token</code> como nombre de usuario y <var class="keyword varname">api_key</var> como contraseña. Separe <code>token</code>
y <var class="keyword varname">api_key</var> con un signo de dos puntos. Para obtener más información, consulte [Configuración del cliente](/docs/services/EventStreams/eventstreams063.html).

### Obtención de credenciales mediante la CLI de IBM Cloud 
{: #connect_standard_external_cli }

<ol>
<li>Asegúrese de que está en la organización y el espacio de Cloud Foundry que desea. Puede navegar de forma interactiva ejecutando el mandato siguiente:<br>
<code>ibmcloud target --cf</code>
</li>
<li>Busque el servicio:<br>
<code>ibmcloud service list</code>
</li>
<li>Puede crear una clave de servicio:<br>
<code>ibmcloud service key-create <var class="keyword varname">nombre_servicio</var> <var class="keyword varname">nuevo_nombre_clave_servicio</var></code><br>
<br/>
o utilizar una clave de servicio existente: <br/>
<code>ibmcloud service keys <var class="keyword varname">nombre_servicio</var></code> 
</li>
<li>Obtenga los detalles de la clave:</br>
<code>ibmcloud service key-show <var class="keyword varname">nombre_servicio</var> <var class="keyword varname">nombre_clave_servicio</var></code></br>
Esto devuelve los detalles de la clave de servicio en formato JSON.</li>
<li>Pase estas credenciales a la aplicación. Especifique <code>token</code> como nombre de usuario y <var class="keyword varname">api_key</var> como contraseña. Separe <code>token</code>
y <var class="keyword varname">api_key</var> con un signo de dos puntos. Para obtener más información, consulte [Configuración del cliente](/docs/services/EventStreams/eventstreams063.html).</li>
</ol>

 
## Visión general del plan Empresa
{: #connect_enterprise}

Los servicios suministrados mediante el plan Empresa se agrupan en el panel de control bajo la cabecera **Servicios**. El plan Empresa está [habilitado para IAM ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/docs/iam/quickstart.html#getstarted){:new_window}. No es necesario que entienda IAM para empezar a trabajar, pero se recomienda tener algunos conocimientos si desea proteger el servicio {{site.data.keyword.messagehub}}. Para obtener más información, consulte
[Protección de los recursos de {{site.data.keyword.messagehub}}](/docs/services/EventStreams/eventstreams124.html)

Siga los pasos siguientes para enlazar la aplicación y obtener claves de servicio para el servicio. Para tener autorización para crear temas, la aplicación o la clave de servicio deben tener un rol con acceso de gestor.

Para conectar una aplicación, el método utilizado depende de dónde se haya desplegado, es decir, en Cloud Foundry o fuera del mismo; por ejemplo, en el servicio Kubernetes.

## Aplicaciones de Cloud Foundry en el plan Empresa
{: #connect_enterprise_cf}

La aplicación debe estar enlazada a la instancia de servicio de {{site.data.keyword.messagehub}}. Para enlazar una aplicación de Cloud Foundry a un servicio que no sea de Cloud Foundry, cree primero un alias de servicio de Cloud Foundry y, a continuación, haga referencia a este alias desde la aplicación Cloud Foundry cuando la enlace.

Cuando esté enlazada, los detalles de la conexión pasan a estar disponibles para la aplicación en formato JSON en la variable de entorno VCAP_SERVICES. Puede enlazar una aplicación y un servicio mediante la consola de IBM Cloud o la CLI de IBM Cloud.

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
 

### Enlace de una app mediante la CLI de IBM Cloud
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
y <var class="keyword varname">api_key</var> con un signo de dos puntos. Para obtener más información, consulte [Configuración del cliente](/docs/services/EventStreams/eventstreams063.html). 
<p>Es posible que tenga que volver a transferir la aplicación para que los cambios entren en vigor.</p></li>
</ol>


## Aplicaciones externas en el plan Empresa
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
y <var class="keyword varname">api_key</var> con un signo de dos puntos. Para obtener más información, consulte [Configuración del cliente](/docs/services/EventStreams/eventstreams063.html).
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
y <var class="keyword varname">api_key</var> con un signo de dos puntos. Para obtener más información, consulte [Configuración del cliente](/docs/services/EventStreams/eventstreams063.html).
<p>Asegúrese de que la aplicación analiza los detalles.</p></li>
</ol>

## Qué hacer a continuación
{: #after_connecting}

Ahora que tiene la información de conexión y de credenciales, puede elegir un cliente de {{site.data.keyword.messagehub}}. Su elección dependerá de su plan.

* Si utiliza el plan Estándar, consulte [Elección entre las tres API](/docs/services/EventStreams/eventstreams087.html) para obtener información sobre qué cliente elegir y cómo conectarse.
* Si utiliza el plan Empresa, consulte [Utilización de la API Kafka](/docs/services/EventStreams/eventstreams050.html).

	Solo puede ver el tema <code>__consumer_offsets</code> interno de Kafka en modalidad de solo lectura si utiliza el plan Empresa. Se recomienda encarecidamente que no intente gestionar el tema de ninguna forma. 

<!--
Charlie said:

"Add some info describing how to take the information made available from above e.g. like the info in the Connecting a client to the Kafka API section of the alpha docs on stage 1? https://console.stage1.bluemix.net/docs/services/EventStreams/eventstreams122.html#alpha_about "
-->







 















