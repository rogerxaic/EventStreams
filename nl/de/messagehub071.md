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

# Umgebungsvariable VCAP_SERVICES
{: #vcap}

Wenn Ihre Anwendung an den {{site.data.keyword.messagehub}}-Service gebunden ist, werden alle den Service betreffenden Details im JSON-Format in der Umgebungsvariablen VCAP_SERVICES für Ihre App gespeichert. Nachfolgend finden Sie ein Beispiel:

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

Der Inhalt der Umgebungsvariablen ist gleich, ganz gleich, welche API Sie für die Verbindung mit {{site.data.keyword.messagehub}} verwenden. Ihre {{site.data.keyword.Bluemix_notm}}-App
wählt je nach verwendeter Schnittstelle die entsprechenden Berechtigungsnachweise aus der Umgebungsvariablen VCAP_SERVICES aus.
 
Nur Ihre ersten fünf Broker sind in VCAP_SERVICES aufgeführt. Wenn Sie mehr als fünf Broker haben, verwenden Sie einen Kafka-Client, um die Details Ihrer anderen Broker abzurufen. 
