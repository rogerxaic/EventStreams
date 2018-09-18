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

# Variabile di ambiente VCAP_SERVICES
{: #vcap}

Quando la tua applicazione è associata al servizio {{site.data.keyword.messagehub}}, i dettagli del servizio sono
memorizzati in formato JSON nella variabile di ambiente VCAP_SERVICES per la tua applicazione. Ecco un esempio:

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

Il contenuto della variabile di ambiente è lo stesso, indipendentemente dall'API che usi per stabilire una connessione a {{site.data.keyword.messagehub}}. La tua applicazione {{site.data.keyword.Bluemix_notm}} seleziona le credenziali appropriate dalla variabile di ambiente VCAP_SERVICES, a seconda dell'interfaccia in
uso.
 
Solo i primi cinque broker sono elencati in VCAP_SERVICES. Se hai più di cinque broker, utilizza un client Kafka per richiamare i dettagli dei tuoi altri broker. 
