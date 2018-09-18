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

# Variable d'environnement VCAP_SERVICES
{: #vcap}

Quand votre application est liée au service {{site.data.keyword.messagehub}}, les détails relatifs au service sont stockés au format JSON dans la variable d'environnement VCAP_SERVICES pour votre application. Exemple :

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

Le contenu de la variable d'environnement est le même, quelle que soit l'API que vous utilisez pour vous connecter à {{site.data.keyword.messagehub}}. Votre application {{site.data.keyword.Bluemix_notm}} sélectionne les données d'identification appropriées depuis la variable d'environnement VCAP_SERVICES, selon l'interface utilisée.
 
Seuls vos cinq premiers courtiers sont répertoriés dans VCAP_SERVICES. Si vous avez plus de cinq courtiers, utilisez un client Kafka pour extraire les détails de vos autres courtiers. 
