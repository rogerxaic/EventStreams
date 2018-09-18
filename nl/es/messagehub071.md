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

# Variable de entorno VCAP_SERVICES
{: #vcap}

Cuando la aplicación está enlazada con el servicio {{site.data.keyword.messagehub}}, los detalles del servicio se almacenan en formato JSON en la variable de entorno VCAP_SERVICES para la app. Aquí tiene un ejemplo:

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
