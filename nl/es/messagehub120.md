---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Iniciación al programa Alpha
{: #alpha_program}

El programa Alpha proporciona un acceso previo a la siguiente versión del servicio de {{site.data.keyword.messagehub}}. 

Realice los pasos siguientes para hacer que una app se ejecute con Alpha de {{site.data.keyword.messagehub}}:


## Cree el servicio de {{site.data.keyword.messagehub}}
{: alpha_create}


  1. Pulse el mosaico **Message Hub vNext - Producción**, que es un servicio experimental del
[catálogo ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.stage1.bluemix.net/catalog/labs/?search=vnext).</li>

  2. Cree un servicio de {{site.data.keyword.messagehub}}. Este servicio proporciona un clúster de un solo arrendatario bajo el plan de precios <code>Premium</code> y normalmente tarda entre 1 y 3 horas en suministrarse.
 


## Obtenga credenciales utilizando la opción de la consola: crear y conectar una app de prueba
{: alpha_app}

Necesitará credenciales para trabajar con {{site.data.keyword.messagehub}}.
Si aún no tiene ninguna app que pueda utilizar, cree una app de prueba y utilice las credenciales que utiliza la app para conectarse a {{site.data.keyword.messagehub}}. Por ejemplo, utilice el servicio de **SDK for Node.js** para una app de prueba. 

  1. Navegue hasta el mosaico **SDK for Node.js** del [catálogo ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.stage1.bluemix.net/catalog/starters/sdk-for-nodejs).
   
  Antes de especificar un nombre de app, asegúrese de que haya seleccionado una región de EE.UU. sur. Cree la app.

  2. Cuando la app se esté ejecutando, pulse el separador **Conexiones** a la izquierda.

  3. Pulse el botón **Crear conexión**.

  4. Seleccione el nuevo servicio de {{site.data.keyword.messagehub}} de la lista de servicios compatibles existentes y pulse el botón **Conectar**.

  5. En la ventana **Conectar servicio habilitado para IAM**, acepte los valores predeterminados y pulse **Conectar**.
  Asegúrese de que se proporcione el servicio de {{site.data.keyword.messagehub}} para que pueda conectarse al mismo.

  6. Pulse el separador **Tiempo de ejecución** a la izquierda y seleccione el separador **Variables de entorno** en el centro. En la sección **VCAP_SERVICES**, localice la información <code>kafka_admin_url</code>, <code>apikey</code> y <code>kafka_brokers_sasl</code>, que necesitará para poder enviar un mensaje.
  
## Obtenga credenciales utilizando la opción de línea de mandatos
Como alternativa, puede obtener las credenciales necesarias utilizando la línea de mandatos. Siga estos pasos:

  1. Instale la herramienta de línea de mandatos de {{site.data.keyword.Bluemix_notm}} desde [la CLI de {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/cli/index.html#overview).
  
  2. Inicie la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}.
  
  3. Utilice la CLI de {{site.data.keyword.Bluemix_notm}} para crear una clave de servicio con el rol Gestor utilizando el mandato siguiente:
  ```
  bx resource service-key-create <NAME> Manager --instance-name <MESSAGEHUB_SERVICE_INSTANCE_NAME>
  ```
  4. La salida contiene la clave de API, el URL de punto final de REST de administrador, y una lista de intermediario. Puede ver esta información de nuevo ejecutando el mandato siguiente:
  ```
  bx resource service-key <NAME>
  ```

## Cree un tema de {{site.data.keyword.messagehub}} y envíe un mensaje

Puede utilizar un mandato CURL para crear un tema y, a continuación, la herramienta kafkacat para producir y consumir un mensaje. 

Para cada mandato, sustituya APIKEY y KAFKA_ADMIN_URL por valores de su variable de entorno VCAP_SERVICES.

  1. Desde la línea de mandatos, cree un tema de {{site.data.keyword.messagehub}} utilizando el siguiente mandato de CURL:
  
    ```
    curl -i -X POST -H "Content-Type: application/json" -H "X-Auth-Token: APIKEY" --data '{ "name": "newtop"}' KAFKA_ADMIN_URL/admin/topics
    ```
    {: codeblock}

  2. Instale la [herramienta kafkacat![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/edenhill/kafkacat#install), que es útil para una prueba rápida de Kafka.
  
  3. Para ejecutar los mandatos siguientes, necesita la información siguiente:
  
    * La lista de intermediarios, que se devuelve en las credenciales `kafka_brokers_sasl`. Su lista de intermediarios debe ser una lista separada por comas para kafkacat.
	Solo se listarán los primeros cinco intermediarios en VCAP_SERVICES. Si tiene más de cinco intermediarios, utilice un cliente Kafka para recuperar los detalles del resto de los intermediarios. 
  
    * Su <code>apikey</code>. Los primeros 8 caracteres forman su sasl.username y el resto de la <code>apikey</code> forma su sasl.password.
    * La ubicación de los certificados SSL. Por ejemplo, en Ubuntu, el SSL_CERTS_DIRECTORY es <code>/etc/ssl/certs/</code>
  
  4. Produzca algunos mensajes ejecutando un mandato como el siguiente:
  ```
  kafkacat -X "security.protocol=sasl_ssl" -X 'sasl.mechanisms=PLAIN' -X 'sasl.username=<FIRST_8_CHARS_FROM_APIKEY>' -X 'sasl.password=<REMAINING_CHARS_FROM_APIKEY>' -X "ssl.ca.location=<SSL_CERTS_DIRECTORY>" -b <BROKERS_LIST> -P -t <TOPIC_NAME>
    ```
		
	<br/>
  Después de ejecutar el mandato, puede especificar algún texto como <code>HelloWorld</code> en el terminal del productor.
  
  5. Consuma los mensajes ejecutando un mandato como el siguiente:
  ```
  kafkacat -X "security.protocol=sasl_ssl" -X 'sasl.mechanisms=PLAIN' -X 'sasl.username=<FIRST_8_CHARS_FROM_APIKEY>' -X 'sasl.password=<REMAINING_CHARS_FROM_APIKEY>' -X "ssl.ca.location=<SSL_CERTS_DIRECTORY>" -b <BROKERS_LIST> -C -t <TOPIC_NAME> -f 'Topic %t [%p] at offset %o: key %k: %s\n'
  ```
	
	<br/>
  Debería ver <code>HelloWorld</code> en el terminal del consumidor.

