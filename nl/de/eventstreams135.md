---

copyright:
  years: 2015, 2019
lastupdated: "2018-11-07"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Verbindung zu {{site.data.keyword.messagehub}} mit dem Plan "Classic" herstellen 
{: #connecting_classic}

Die Art und Weise, wie Sie eine Verbindung zu {{site.data.keyword.messagehub}} herstellen, hängt davon ab, ob Sie eine Verbindung von einer Cloud Foundry-Anwendung oder von einem anderen externen Client aus herstellen. Sie müssen zwei Einzelinformationen abrufen, um eine Verbindung zu einer der {{site.data.keyword.messagehub}}-APIs herstellen zu können:
{: shortdesc}

* Endpunkt-URLs für die APIs
* Berechtigungsnachweise für die Authentifizierung

Den nachfolgenden Informationen können Sie entnehmen, wie Sie diese Details abrufen können. Die beschriebenen Schritte können sich geringfügig unterscheiden. Stellen Sie daher sicher, dass Sie die für Ihre Instanz geeigneten Schritte ausführen.

## {{site.data.keyword.messagehub}}-Instanz bereitstellen
{: #provision_classic}

Als Voraussetzung müssen Sie zuerst eine {{site.data.keyword.messagehub}}-Serviceinstanz für den Plan "Classic" bereitstellen. Rufen Sie anschließend die Verbindungsdetails für die {{site.data.keyword.messagehub}}-API ab, indem Sie die folgenden Tasks ausführen.

## Übersicht über den Plan "Classic"
{: #connect_classic_plan}

Services, die unter Verwendung des Plans "Classic" bereitgestellt werden, sind Cloud Foundry-Services. Dies bedeutet, dass sie in einer Cloud Foundry-Organisation und einem Cloud Foundry-Bereich bereitgestellt und im Dashboard unter der Überschrift **Cloud Foundry-Services** gruppiert werden. Die zum Verbinden einer Anwendung verwendete Methode ist davon abhängig, ob die Anwendung innerhalb von Cloud Foundry bereitgestellt wird oder außerhalb von Cloud Foundry, z. B. im Kubernetes-Service.


## Cloud Foundry-Anwendungen beim Plan "Classic"
{: #connect_classic_cf_plan}

Wenn Apps innerhalb von Cloud Foundry ausgeführt werden, binden Sie Ihre App an die {{site.data.keyword.messagehub}}-Serviceinstanz. Nach dem Binden werden die Verbindungsdetails in der Umgebungsvariablen VCAP_SERVICES in JSON-Formatierung für die App verfügbar gemacht. Sie können eine App und einen Service entweder über die IBM Cloud-Konsole oder über die IBM Cloud-CLI binden.

Nachfolgend finden Sie ein Beispiel für VCAP_SERVICES:

```
{
  "credentials": {
    "mqlight_lookup_url": "https://mqlight-lookup.messagehub.services.us-south.bluemix.net/Lookup?serviceId=584e8436-e7f5-43db-96ac-2864fccae5ae",
    "api_key": <IHR_API-SCHLÜSSEL>,
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

Der Inhalt der Umgebungsvariablen ist identisch, unabhängig davon, welche API Sie für die Verbindung mit {{site.data.keyword.messagehub}} verwenden. Ihre {{site.data.keyword.Bluemix_notm}}-App
wählt je nach verwendeter Schnittstelle die entsprechenden Berechtigungsnachweise aus der Umgebungsvariablen VCAP_SERVICES aus.
 
Nur Ihre ersten fünf Broker sind in VCAP_SERVICES aufgeführt. Wenn Sie über mehr als fünf Broker verfügen, verwenden Sie einen Kafka-Client, um die Details der anderen Broker abzurufen. 


### Berechtigungsnachweise abrufen und Verbindung über die IBM Cloud-Konsole herstellen
{: #connect_classic_plan_cf_console }

1. Stellen Sie sicher, dass Sie sich in der beabsichtigten Cloud Foundry-Organisation und im entsprechenden Cloud Foundry-Bereich befinden.
2. Suchen Sie Ihre Cloud Foundry-Anwendung im Dashboard. Falls Sie nicht über eine Cloud Foundry-Anwendung verfügen, können Sie durch Klicken auf die Schaltfläche **Ressource erstellen** eine solche erstellen.
3. Klicken Sie auf die Anwendungskachel.
4. Klicken Sie auf **Verbindungen**.
5. Klicken Sie auf **Verbindung erstellen**.
6. Wählen Sie die Kachel für den zu bindenden {{site.data.keyword.messagehub}}-Service aus und klicken Sie auf **Verbindung herstellen**. Sie müssen möglicherweise ein erneutes Staging Ihrer Anwendung durchführen, damit die Änderungen wirksam werden.
7. Klicken Sie links auf die Registerkarte **Laufzeit** und wählen Sie die Registerkarte **Umgebungsvariablen** in der Mitte aus. Sie können nun die Informationen zu VCAP_SERVICES überprüfen und Ihre Anwendung kann auf diese nun als Umgebungsvariablen zugreifen. 


### Berechtigungsnachweise über die IBM Cloud-CLI abrufen 
{: #connect_classic_plan_cf_cli }

<ol>
<li>Stellen Sie sicher, dass Sie sich in der beabsichtigten Cloud Foundry-Organisation und im entsprechenden Cloud Foundry-Bereich befinden. Sie können interaktiv navigieren, indem Sie den folgenden Befehl ausführen:<br/>
<code>ibmcloud target --cf</code>
</li>
<li>Suchen Sie Ihre App:<br/> <code>ibmcloud app list</code> <br/>
</br>
Wenn Sie über eine Manifestdatei verfügen, können Sie eine neue App erstellen, indem Sie folgenden Befehl ausführen:</br>
<code>ibmcloud app push</code>
</li>
<li>Suchen Sie Ihren Service:</br>
<code>ibmcloud service list</code>
</li>
<li>Binden Sie Ihre App an den Service:</br>
<code>ibmcloud service bind <var class="keyword varname">Ihr_App-Name</var> <var class="keyword varname">Ihr_Servicename</var></code>
</li>
<li>Stellen Sie sicher, dass die Umgebungsvariable VCAP_SERVICES in Ihrer Anwendungslaufzeit verfügbar ist, indem Sie den folgenden Befehl ausführen:</br>
 <code>ibmcloud app env <var class="keyword varname">Ihr_App-Name</var></code>. 
</li>
<li>Übergeben Sie diese Berechtigungsnachweise an Ihre Anwendung. Geben Sie <code>token</code> als Ihren Benutzernamen an und den <var class="keyword varname">api_key</var> als Kennwort. Trennen Sie <code>token</code> und den <var class="keyword varname">api_key</var> durch einen Doppelpunkt voneinander. Weitere Informationen finden Sie in [Client konfigurieren](/docs/services/EventStreams?topic=eventstreams-kafka_connect).
<p>Sie müssen möglicherweise ein erneutes Staging Ihrer Anwendung durchführen, damit die Änderungen wirksam werden.</p></li>
</ol>

## Externe Anwendungen beim Plan "Classic"
{: #connect_classic_plan_external}

Für Anwendungen, die außerhalb von Cloud Foundry ausgeführt werden, werden die Berechtigungsnachweise durch Erstellen eines Serviceschlüssels generiert. Nachdem Sie einen Serviceschlüssel abgerufen haben, übergeben Sie die Details des Schlüssels mit der von Ihnen ausgewählten Methode an Ihre Anwendung.

### Berechtigungsnachweise über die IBM Cloud-Konsole abrufen
{: #connect_classic_plan_external_console}

1. Stellen Sie sicher, dass Sie sich in der beabsichtigten Cloud Foundry-Organisation und im entsprechenden Cloud Foundry-Bereich befinden.
2. Suchen Sie Ihren Cloud Foundry-{{site.data.keyword.messagehub}}-Service im Dashboard.
3. Klicken Sie auf die Servicekachel.
4. Klicken Sie auf **Serviceberechtigungsnachweise**.
5. Klicken Sie auf **Neuer Berechtigungsnachweis**.
6. Geben Sie die Details zu Ihrem neuen Berechtigungsnachweis, z. B. einen Namen, ein und klicken Sie auf **Hinzufügen**. In der Berechtigungsnachweisliste wird ein neuer Berechtigungsnachweis angezeigt.
7. Klicken Sie auf diesen Berechtigungsnachweis mithilfe von **Berechtigungsnachweise anzeigen**, um die Details in JSON-Formatierung sichtbar zu machen.
8. Übergeben Sie diese Berechtigungsnachweise an Ihre Anwendung. Geben Sie <code>token</code> als Ihren Benutzernamen an und den <var class="keyword varname">api_key</var> als Kennwort. Trennen Sie <code>token</code> und den <var class="keyword varname">api_key</var> durch einen Doppelpunkt voneinander. Weitere Informationen finden Sie in [Client konfigurieren](/docs/services/EventStreams?topic=eventstreams-kafka_connect).

### Berechtigungsnachweise über die IBM Cloud-CLI abrufen 
{: #connect_classic_plan_external_cli }

<ol>
<li>Stellen Sie sicher, dass Sie sich in der beabsichtigten Cloud Foundry-Organisation und im entsprechenden Cloud Foundry-Bereich befinden. Sie können interaktiv navigieren, indem Sie den folgenden Befehl ausführen:<br>
<code>ibmcloud target --cf</code>
</li>
<li>Suchen Sie Ihren Service:<br>
<code>ibmcloud service list</code>
</li>
<li>Sie können entweder einen Serviceschlüssel erstellen:<br>
<code>ibmcloud service key-create <var class="keyword varname">Ihr_Servicename</var> <var class="keyword varname">neuer_Serviceschlüsselname</var></code><br>
<br/>
oder einen vorhandenen Serviceschlüssel verwenden: <br/>
<code>ibmcloud service keys <var class="keyword varname">Ihr_Servicename</var></code> 
</li>
<li>Rufen Sie die Details für den Schlüssel ab:</br>
<code>ibmcloud service key-show <var class="keyword varname">Ihr_Servicename</var> <var class="keyword varname">Serviceschlüsselname</var></code></br>
Dadurch werden die Serviceschlüsseldetails im JSON-Format zurückgegeben.</li>
<li>Übergeben Sie diese Berechtigungsnachweise an Ihre Anwendung. Geben Sie <code>token</code> als Ihren Benutzernamen an und den <var class="keyword varname">api_key</var> als Kennwort. Trennen Sie <code>token</code> und den <var class="keyword varname">api_key</var> durch einen Doppelpunkt voneinander. Weitere Informationen finden Sie in [Client konfigurieren](/docs/services/EventStreams?topic=eventstreams-kafka_connect).</li>
</ol>
 
## Nächste Schritte
{: #after_connecting_next}

Nachdem Sie über eine Verbindung und Berechtigungsinformationen verfügen, können Sie einen {{site.data.keyword.messagehub}}-Client auswählen. Informationen zur Auswahl des Clients und zur Verbindungsherstellung finden Sie in [Geeignete API auswählen](/docs/services/EventStreams?topic=eventstreams-choose_api_classic).










 















