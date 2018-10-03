---

copyright:
  years: 2015, 2018
lastupdated: "2018-09-04"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Verbindung zu {{site.data.keyword.messagehub}} herstellen
{: #connecting}

Die Art der Herstellung einer Verbindung zu {{site.data.keyword.messagehub}} variiert abhängig davon, ob Sie den Plan "Standard" oder den Plan "Enterprise" verwenden und auch je nachdem, ob die Verbindung von einer Cloud Foundry-Anwendung oder einem beliebigen anderen externen Client hergestellt wird. Sie müssen zwei Einzelinformationen abrufen, um eine Verbindung zu einer der {{site.data.keyword.messagehub}}-APIs herstellen zu können:

* Endpunkt-URLs für die APIs
* Berechtigungsnachweise für die Authentifizierung

Den nachfolgenden Informationen können Sie entnehmen, wie Sie diese Details abrufen können. Die beschriebenen Schritte können sich geringfügig unterscheiden. Stellen Sie daher sicher, dass Sie die für Ihre Instanz geeigneten Schritte ausführen.

## {{site.data.keyword.messagehub}}-Instanz bereitstellen

Als Voraussetzung müssen Sie zuerst eine {{site.data.keyword.messagehub}}-Serviceinstanz für den Plan "Standard" oder den Plan "Enterprise" bereitstellen. Für die Bereitstellung einer {{site.data.keyword.messagehub}}-Instanz fallen möglicherweise Gebühren an. Rufen Sie anschließend die Verbindungsdetails für die {{site.data.keyword.messagehub}}-API ab, indem Sie die folgenden Tasks ausführen.

## Plan "Standard" - Übersicht
{: #connect_standard}

Services, die unter Verwendung des Plans "Standard" bereitgestellt werden, sind Cloud Foundry-Services. Dies bedeutet, dass sie in einer Cloud Foundry-Organisation und einem Cloud Foundry-Bereich bereitgestellt und im Dashboard unter der Überschrift **Cloud Foundry-Services** gruppiert werden. Die verwendete Methode zum Verbinden einer Anwendung ist davon abhängig, ob die Anwendung innerhalb von Cloud Foundry oder extern bereitgestellt wird.


## Cloud Foundry-Anwendungen beim Plan "Standard"
{: #connect_standard_cf}

Wenn Apps innerhalb von Cloud Foundry ausgeführt werden, binden Sie Ihre App an die {{site.data.keyword.messagehub}}-Serviceinstanz. Nach dem Binden werden die Verbindungsdetails in der Umgebungsvariablen VCAP_SERVICES in JSON-Formatierung für die App verfügbar gemacht. Sie können eine App und einen Service entweder über die IBM Cloud-Konsole oder über die IBM Cloud-CLI binden.

Nachfolgend finden Sie ein Beispiel für VCAP_SERVICES:

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

Der Inhalt der Umgebungsvariablen ist identisch, unabhängig davon, welche API Sie für die Verbindung mit {{site.data.keyword.messagehub}} verwenden. Ihre {{site.data.keyword.Bluemix_notm}}-App
wählt je nach verwendeter Schnittstelle die entsprechenden Berechtigungsnachweise aus der Umgebungsvariablen VCAP_SERVICES aus.
 
Nur Ihre ersten fünf Broker sind in VCAP_SERVICES aufgeführt. Wenn Sie über mehr als fünf Broker verfügen, verwenden Sie einen Kafka-Client, um die Details der anderen Broker abzurufen. 


### Berechtigungsnachweise abrufen und Verbindung über die IBM Cloud-Konsole herstellen
{: #connect_standard_cf_console }

1. Stellen Sie sicher, dass Sie sich in der beabsichtigten Cloud Foundry-Organisation und im entsprechenden Cloud Foundry-Bereich befinden.
2. Suchen Sie Ihre Cloud Foundry-Anwendung im Dashboard. Falls Sie nicht über eine Cloud Foundry-Anwendung verfügen, können Sie durch Klicken auf die Schaltfläche **Ressource erstellen** eine solche erstellen.
3. Klicken Sie auf die Anwendungskachel.
4. Klicken Sie auf **Verbindungen**.
5. Klicken Sie auf **Verbindung erstellen**.
6. Wählen Sie die Kachel für den zu bindenden {{site.data.keyword.messagehub}}-Service aus und klicken Sie auf **Verbindung herstellen**. Sie müssen möglicherweise ein erneutes Staging Ihrer Anwendung durchführen, damit die Änderungen wirksam werden.
7. Klicken Sie links auf die Registerkarte **Laufzeit** und wählen Sie die Registerkarte **Umgebungsvariablen** in der Mitte aus. Sie können nun die Informationen zu VCAP_SERVICES überprüfen und Ihre Anwendung kann auf diese nun als Umgebungsvariablen zugreifen. 


### Berechtigungsnachweise über die IBM Cloud-CLI abrufen 
{: #connect_standard_cf_cli }

<ol>
<li>Stellen Sie sicher, dass Sie sich in der beabsichtigten Cloud Foundry-Organisation und im entsprechenden Cloud Foundry-Bereich befinden. Sie können interaktiv navigieren, indem Sie den folgenden Befehl ausführen:<br/>
<code>ibmcloud target --cf</code>
</li>
<li>Suchen Sie Ihre App:<br/> <code>ibmcloud app list</code> <br/>
</br>
Wenn Sie über eine Manifestdatei verfügen, können Sie mithilfe des folgenden Befehls eine neue App erstellen:</br>
<code>ibmcloud app push</code>
</li>
<li>Suchen Sie Ihren Service:</br> 
<code>ibmcloud service list</code>
</li>
<li>Binden Sie Ihre App an den Service:</br>
<code>ibmcloud service bind <var class="keyword varname">your_app_name</var> <var class="keyword varname">your_service_name</var></code>
</li>
<li>Überprüfen Sie, ob die Umgebungsvariable VCAP_SERVICES in Ihrer Anwendungslaufzeit verfügbar ist, indem Sie den folgenden Befehl ausführen:</br> 
 <code>ibmcloud app env <var class="keyword varname">your_app_name</var></code>. 
</li>
<li>Übergeben Sie diese Berechtigungsnachweise an Ihre Anwendung. Geben Sie <code>token</code> als Ihren Benutzernamen an und den <var class="keyword varname">api_key</var> als Kennwort. Trennen Sie <code>token</code> und den <var class="keyword varname">api_key</var> durch einen Doppelpunkt voneinander. Weitere Informationen finden Sie in [Client konfigurieren](/docs/services/EventStreams/eventstreams063.html).
<p>Sie müssen möglicherweise ein erneutes Staging Ihrer Anwendung durchführen, damit die Änderungen wirksam werden.</p></li>
</ol>

## Externe Anwendungen beim Plan "Standard"
{: #connect_standard_external}

Für Anwendungen, die außerhalb von Cloud Foundry ausgeführt werden, werden die Berechtigungsnachweise durch Erstellen eines Serviceschlüssels generiert. Nachdem Sie einen Serviceschlüssel abgerufen haben, übergeben Sie die Details des Schlüssels mit der von Ihnen ausgewählten Methode an Ihre Anwendung.

### Berechtigungsnachweise über die IBM Cloud-Konsole abrufen
{: #connect_standard_external_console}

1. Stellen Sie sicher, dass Sie sich in der beabsichtigten Cloud Foundry-Organisation und im entsprechenden Cloud Foundry-Bereich befinden.
2. Suchen Sie Ihren Cloud Foundry-{{site.data.keyword.messagehub}}-Service im Dashboard.
3. Klicken Sie auf die Servicekachel.
4. Klicken Sie auf **Serviceberechtigungsnachweise**.
5. Klicken Sie auf **Neuer Berechtigungsnachweis**.
6. Geben Sie die Details zu Ihrem neuen Berechtigungsnachweis, z. B. einen Namen, ein und klicken Sie auf **Hinzufügen**. In der Berechtigungsnachweisliste wird ein neuer Berechtigungsnachweis angezeigt.
7. Klicken Sie auf diesen Berechtigungsnachweis mithilfe von **Berechtigungsnachweise anzeigen**, um die Details in JSON-Formatierung sichtbar zu machen.
8. Übergeben Sie diese Berechtigungsnachweise an Ihre Anwendung. Geben Sie <code>token</code> als Ihren Benutzernamen an und den <var class="keyword varname">api_key</var> als Kennwort. Trennen Sie <code>token</code> und den <var class="keyword varname">api_key</var> durch einen Doppelpunkt voneinander. Weitere Informationen finden Sie in [Client konfigurieren](/docs/services/EventStreams/eventstreams063.html).


### Berechtigungsnachweise über die IBM Cloud-CLI abrufen 
{: #connect_standard_external_cli }

<ol>
<li>Stellen Sie sicher, dass Sie sich in der beabsichtigten Cloud Foundry-Organisation und im entsprechenden Cloud Foundry-Bereich befinden. Sie können interaktiv navigieren, indem Sie den folgenden Befehl ausführen:<br>
<code>ibmcloud target --cf</code>
</li>
<li>Suchen Sie Ihren Service:<br>
<code>ibmcloud service list</code>
</li>
<li>Sie können entweder einen Serviceschlüssel erstellen:<br>
<code>ibmcloud service key-create <var class="keyword varname">your_service_name</var> <var class="keyword varname">new_service_key_name</var></code><br>
<br/>
oder einen vorhandenen Serviceschlüssel verwenden: <br/>
<code>ibmcloud service keys <var class="keyword varname">your_service_name</var></code> 
</li>
<li>Rufen Sie die Details zu dem Schlüssel ab:</br>
<code>ibmcloud service key-show <var class="keyword varname">your_service_name</var> <var class="keyword varname">service_key_name</var></code></br>
Auf diese Weise werden die Details zu dem Serviceschlüssel in JSON-Formatierung zurückgegeben.</li>
<li>Übergeben Sie diese Berechtigungsnachweise an Ihre Anwendung. Geben Sie <code>token</code> als Ihren Benutzernamen an und den <var class="keyword varname">api_key</var> als Kennwort. Trennen Sie <code>token</code> und den <var class="keyword varname">api_key</var> durch einen Doppelpunkt voneinander. Weitere Informationen finden Sie in [Client konfigurieren](/docs/services/EventStreams/eventstreams063.html).</li>
</ol>

 
## Plan "Enterprise" - Übersicht
{: #connect_enterprise}

Services, die unter Verwendung des Plans "Enterprise" bereitgestellt werden, werden im Dashboard unter der Überschrift **Services** gruppiert. Der Plan "Enterprise" ist für [IAM aktiviert ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/docs/iam/quickstart.html#getstarted){:new_window}. Sie müssen keine Kenntnisse über IAM besitzen, um erste Schritte auszuführen, es empfiehlt sich jedoch ein gewisses Grundwissen, wenn Sie Ihren {{site.data.keyword.messagehub}}-Service schützen möchten. Weitere Informationen finden Sie in
[{{site.data.keyword.messagehub}}-Ressourcen schützen](/docs/services/EventStreams/eventstreams124.html).

Führen Sie die folgenden Schritte aus, um Ihre Anwendung zu binden und Serviceschlüssel für Ihren Service abzurufen. Ihre Anwendung oder Ihr Serviceschlüssel muss die Zugriffsrolle "Manager" aufweisen, damit sie bzw. er zum Erstellen von Topics berechtigt ist.

Die zum Verbinden einer Anwendung verwendete Methode ist davon abhängig, ob die Anwendung innerhalb von Cloud Foundry oder extern bereitgestellt wird.

## Cloud Foundry-Anwendungen beim Plan "Enterprise"
{: #connect_enterprise_cf}

Ihre Anwendung muss an die {{site.data.keyword.messagehub}}-Serviceinstanz gebunden werden. Zum Binden einer Cloud Foundry-Anwendung an einen externen Service mit One Cloud müssen Sie zunächst einen Cloud Foundry-Servicealias erstellen und anschließend beim Binden diesen Alias von Ihrer Cloud Foundry-Anwendung referenzieren.

Nach dem Binden werden die Verbindungsdetails mithilfe der Umgebungsvariablen VCAP_SERVICES in JSON-Formatierung für die Anwendung verfügbar gemacht. Sie können eine Anwendung und einen Service entweder über die IBM Cloud-Konsole oder über die IBM Cloud-CLI binden.

### Anwendung über die IBM Cloud-Konsole binden
{: #connect_enterprise_cf_console}

1. Stellen Sie sicher, dass Sie sich in der beabsichtigten Cloud Foundry-Organisation und im entsprechenden Cloud Foundry-Bereich befinden.
2. Suchen Sie Ihre Cloud Foundry-Anwendung im Dashboard oder erstellen Sie eine Anwendung durch Klicken auf die Schaltfläche **Ressource erstellen**.
3. Klicken Sie auf die Anwendungskachel.
4. Klicken Sie auf **Verbindungen**.
5. Klicken Sie auf **Verbindung erstellen**.
6. Wählen Sie die Kachel für den zu bindenden {{site.data.keyword.messagehub}}-Service aus und klicken Sie auf **Verbindung herstellen**. 
7. Wählen Sie in dem daraufhin angezeigten Fenster **Verbindung für IAM-fähigen Service** eine Zugriffsrolle unter **Zugriffsrolle für Verbindung** und eine Service-ID in der Liste **Service-ID für Verbindung** aus (Sie können auch die automatisch generierte ID akzeptieren). Klicken Sie auf **Verbinden**. 

  Auf diese Weise wird ein Cloud Foundry-Servicealias für Ihren {{site.data.keyword.messagehub}}-Service erstellt und die Anwendung wird anschließend an diesen Alias gebunden.  

  Führen Sie ein erneutes Staging Ihrer Anwendung durch, damit die Änderungen wirksam werden.<br/>
8. Klicken Sie links auf die Registerkarte **Laufzeit** und wählen Sie die Registerkarte **Umgebungsvariablen** in der Mitte aus. Sie können nun die Informationen zu VCAP_SERVICES überprüfen. Ihre Anwendung kann auf diese nun als Umgebungsvariablen zugreifen. 
 

### Anwendung über die IBM Cloud-CLI binden
{: #connect_enterprise_cf_cli}

<ol>
<li>Stellen Sie sicher, dass Sie sich in der beabsichtigten Cloud Foundry-Organisation und im entsprechenden Cloud Foundry-Bereich befinden. Sie können interaktiv navigieren, indem Sie den folgenden Befehl ausführen:<br/>
 <code>ibmcloud target --cf</code></li>
<li>Suchen Sie Ihre App:</br>
<code>ibmcloud app list</code><br/>
<br/>
Wenn Sie über eine Manifestdatei verfügen, können Sie mithilfe des folgenden Befehls eine neue App erstellen:<br/>
<code>ibmcloud app push</code><br/>
<br/>
Da die App noch nicht an {{site.data.keyword.messagehub}} gebunden ist, kann die App keine Verbindung herstellen. Daher wird empfohlen, dass Sie eine Push-Operation der Anwendung mit dem Parameter <code>--no-start</code> durchführen, um unnötige Verbindungsfehler zu vermeiden.</li>
<li>Suchen Sie Ihren Service:</br>
<code>ibmcloud resource service-instances</code></li>
<li>Erstellen Sie einen Cloud Foundry-Servicealias:<br/>
<code>ibmcloud resource service-alias-create <var class="keyword varname">alias_name</var> --instance-name <var class="keyword varname">your_service_name</var></code></li>
<li>Binden Sie Ihre App an den zuvor erstellten Servicealias:<br/>
<code>ibmcloud service bind <var class="keyword varname">your_ app_name</var> <var class="keyword varname">alias_name</var></code>.<br/>
<br/>
Alternativ können Sie Ihre Manifestdatei aktualisieren und die Anwendung erneut mit Push-Operation übertragen.</li>
<li>Überprüfen Sie, ob die Umgebungsvariable VCAP_SERVICES in Ihrer Anwendungslaufzeit verfügbar ist:<br/>
<code>ibmcloud app env <var class="keyword varname">your_app_name</var></code></li>
<li>Übergeben Sie diese Berechtigungsnachweise an Ihre Anwendung. Geben Sie <code>token</code> als Ihren Benutzernamen an und den <var class="keyword varname">api_key</var> als Kennwort. Trennen Sie <code>token</code> und den <var class="keyword varname">api_key</var> durch einen Doppelpunkt voneinander. Weitere Informationen finden Sie in [Client konfigurieren](/docs/services/EventStreams/eventstreams063.html).
<p>Sie müssen möglicherweise ein erneutes Staging Ihrer Anwendung durchführen, damit die Änderungen wirksam werden.</p></li>
</ol>


## Externe Anwendungen beim Plan "Enterprise"
{: #connect_enterprise_external}

Für Anwendungen, die außerhalb von Cloud Foundry ausgeführt werden, werden die Berechtigungsnachweise durch Erstellen eines Serviceschlüssels generiert. Nachdem Sie den Serviceschlüssel abgerufen haben, übergeben Sie die Details des Schlüssels mit der von Ihnen ausgewählten Methode an Ihre Anwendung.

### Berechtigungsnachweise über die IBM Cloud-Konsole abrufen
{: #connect_enterprise_external_console}

1. Suchen Sie Ihren {{site.data.keyword.messagehub}}-Service im Dashboard.
2. Klicken Sie auf die Servicekachel.
3. Klicken Sie auf **Serviceberechtigungsnachweise**.
4. Klicken Sie auf **Neuer Berechtigungsnachweis**. 
5. Geben Sie die Details zu Ihrem neuen Berechtigungsnachweis, z. B. einen Namen und eine Rolle, ein und klicken Sie auf **Hinzufügen**. In der Berechtigungsnachweisliste wird ein neuer Berechtigungsnachweis angezeigt.
6. Klicken Sie auf diesen Berechtigungsnachweis mithilfe von **Berechtigungsnachweise anzeigen**, um die Details in JSON-Formatierung sichtbar zu machen.
7. Übergeben Sie diese Berechtigungsnachweise an Ihre Anwendung. Geben Sie <code>token</code> als Ihren Benutzernamen an und den <var class="keyword varname">api_key</var> als Kennwort. Trennen Sie <code>token</code> und den <var class="keyword varname">api_key</var> durch einen Doppelpunkt voneinander. Weitere Informationen finden Sie in [Client konfigurieren](/docs/services/EventStreams/eventstreams063.html).
   <br/><br/>Stellen Sie sicher, dass Ihre Anwendung ein Parsing der Details durchführt.

### Berechtigungsnachweise über die IBM Cloud-CLI abrufen
{: #connect_enterprise_external_cli}

<ol>
<li>Suchen Sie Ihren Service:<br/>
<code>ibmcloud resource service-instances</code></li>
<li>Erstellen Sie einen Serviceschlüssel:<br/>
<code>ibmcloud resource service-key-create <var class="keyword varname">key_name</var> <var class="keyword varname">key_role</var> --instance-name <var class="keyword varname">your_service_name</var></code></li>
<li>Geben Sie den Serviceschlüssel aus:<br/>
<code>ibmcloud resource service-key <var class="keyword varname">key_name</var></code></li>
<li>Übergeben Sie diese Berechtigungsnachweise an Ihre Anwendung. Geben Sie <code>token</code> als Ihren Benutzernamen an und den <var class="keyword varname">api_key</var> als Kennwort. Trennen Sie <code>token</code> und den <var class="keyword varname">api_key</var> durch einen Doppelpunkt voneinander. Weitere Informationen finden Sie in [Client konfigurieren](/docs/services/EventStreams/eventstreams063.html).
<p>Stellen Sie sicher, dass Ihre Anwendung ein Parsing der Details durchführt.</p></li>
</ol>

## Nächste Schritte
{: #after_connecting}

Nachdem Sie über eine Verbindung und Berechtigungsinformationen verfügen, können Sie einen {{site.data.keyword.messagehub}}-Client auswählen. Ihre Auswahl ist von Ihrem Plan abhängig.

* Wenn Sie den Plan "Standard" verwenden, finden Sie im Abschnitt
[Geeignete API auswählen](/docs/services/EventStreams/eventstreams087.html) Informationen zur Auswahl des Clients und zur Verbindungsherstellung.
* Wenn Sie den Plan "Enterprise" verwenden, finden Sie im Abschnitt [Kafka-API verwenden](/docs/services/EventStreams/eventstreams050.html) geeignete Informationen.

	Das interne Kafka-Topic <code>__consumer_offsets</code> ist für Sie schreibgeschützt sichtbar,
	wenn Sie den Plan "Enterprise" verwenden. Es wird dringend empfohlen, keinen Versuch zu unternehmen, dieses Topic in irgendeiner Weise zu steuern. 

<!--
Charlie said:

"Add some info describing how to take the information made available from above e.g. like the info in the Connecting a client to the Kafka API section of the alpha docs on stage 1? https://console.stage1.bluemix.net/docs/services/EventStreams/eventstreams122.html#alpha_about "
-->







 















