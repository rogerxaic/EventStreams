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

# Variável de ambiente VCAP_SERVICES
{: #vcap}

Quando seu aplicativo estiver ligado ao serviço {{site.data.keyword.messagehub}}, os detalhes relacionados ao
serviço serão armazenados no formato JSON na variável de ambiente VCAP_SERVICES para seu aplicativo. Aqui há um
        exemplo:

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

O conteúdo da variável de ambiente é o mesmo, independente da API que você usar para se conectar
      ao {{site.data.keyword.messagehub}}. Seu aplicativo {{site.data.keyword.Bluemix_notm}}
seleciona as credenciais apropriadas na variável de ambiente VCAP_SERVICES, dependendo da interface em uso.
 
Somente os seus primeiros cinco brokers são listados em VCAP_SERVICES. Se você tiver mais de cinco brokers, use um cliente Kafka para
recuperar os detalhes de seus outros brokers. 
