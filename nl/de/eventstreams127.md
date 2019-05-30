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


# Verbindung zu {{site.data.keyword.messagehub}} herstellen
{: #connecting}

Die Art und Weise, wie Sie eine Verbindung zu {{site.data.keyword.messagehub}} herstellen, hängt davon ab, ob Ihre Anwendung nativ oder als Cloud Foundry-Anwendung ausgeführt wird. In beiden Fällen sind jedoch zwei Informationen erforderlich:
{: shortdesc}

* Endpunkt-URLs für die APIs
* Berechtigungsnachweise für die Authentifizierung

Den nachfolgenden Informationen können Sie entnehmen, wie Sie diese Details abrufen können. Die beschriebenen Schritte können sich geringfügig unterscheiden. Stellen Sie daher sicher, dass Sie die für Ihre Instanz geeigneten Schritte ausführen.

Informationen zum Herstellen einer Verbindung zu {{site.data.keyword.messagehub}}, wenn Sie den Plan "Classic" verwenden, finden Sie in [Verbindung mit dem Plan "Classic" herstellen](/docs/services/EventStreams?topic=eventstreams-connecting_classic).


## Übersicht
{: #connect_enterprise}

Services, die unter Verwendung des Plans "Standard" oder "Enterprise" bereitgestellt werden, werden im Dashboard unter der Überschrift **Services** gruppiert. Die Pläne sind [für IAM aktiviert ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/iam?topic=iam-getstarted#getstarted){:new_window}. Sie müssen keine Kenntnisse über IAM besitzen, um erste Schritte auszuführen, es empfiehlt sich jedoch ein gewisses Grundwissen, wenn Sie Ihren {{site.data.keyword.messagehub}}-Service schützen möchten. Weitere Informationen finden Sie in [Zugriff auf Ihre {{site.data.keyword.messagehub}}-Ressourcen verwalten](/docs/services/EventStreams?topic=eventstreams-security).

Führen Sie die folgenden Schritte aus, um Ihre Anwendung zu binden und Serviceschlüssel für Ihren Service abzurufen. Ihre Anwendung oder Ihr Serviceschlüssel muss die Zugriffsrolle "Manager" aufweisen, damit sie bzw. er zum Erstellen von Topics berechtigt ist.

Die zum Verbinden einer Anwendung verwendete Methode ist davon abhängig, ob die Anwendung innerhalb von Cloud Foundry bereitgestellt wird oder außerhalb von Cloud Foundry, z. B. im Kubernetes-Service.

## {{site.data.keyword.messagehub}}-Instanz bereitstellen

Als Voraussetzung müssen Sie zuerst eine {{site.data.keyword.messagehub}}-Serviceinstanz für den Plan "Standard" oder den Plan "Enterprise" bereitstellen. Rufen Sie anschließend die Verbindungsdetails für die {{site.data.keyword.messagehub}}-API ab, indem Sie die folgenden Tasks ausführen.

## Anwendungen verbinden 
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
7. Übergeben Sie diese Berechtigungsnachweise an Ihre Anwendung. Geben Sie <code>token</code> als Ihren Benutzernamen an und den <var class="keyword varname">api_key</var> als Kennwort. Trennen Sie <code>token</code> und den <var class="keyword varname">api_key</var> durch einen Doppelpunkt voneinander. Weitere Informationen finden Sie in [Kafka-API-Client konfigurieren](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_api_client).
   <br/><br/>Stellen Sie sicher, dass Ihre Anwendung ein Parsing der Details durchführt.

### Berechtigungsnachweise über die IBM Cloud-CLI abrufen
{: #connect_enterprise_external_cli}

<ol>
<li>Suchen Sie Ihren Service:<br/>
<code>ibmcloud resource service-instances</code></li>
<li>Erstellen Sie einen Serviceschlüssel:<br/>
<code>ibmcloud resource service-key-create <var class="keyword varname">Schlüsselname</var> <var class="keyword varname">Schlüsselrolle</var> --instance-name <var class="keyword varname">Ihr_Servicename</var></code></li>
<li>Geben Sie den Serviceschlüssel aus:<br/>
<code>ibmcloud resource service-key <var class="keyword varname">Schlüsselname</var></code></li>
<li>Übergeben Sie diese Berechtigungsnachweise an Ihre Anwendung. Geben Sie <code>token</code> als Ihren Benutzernamen an und den <var class="keyword varname">api_key</var> als Kennwort. Trennen Sie <code>token</code> und den <var class="keyword varname">api_key</var> durch einen Doppelpunkt voneinander. Weitere Informationen finden Sie in [Client konfigurieren](/docs/services/EventStreams?topic=eventstreams-kafka_connect).
<p>Stellen Sie sicher, dass Ihre Anwendung ein Parsing der Details durchführt.</p></li>
</ol>

## Cloud Foundry-Anwendungen verbinden
{: #connect_enterprise_cf}

Ihre Anwendung muss an die {{site.data.keyword.messagehub}}-Serviceinstanz gebunden werden. Zum Binden einer Cloud Foundry-Anwendung an einen externen Service müssen Sie zunächst einen Cloud Foundry-Servicealias erstellen und anschließend beim Binden diesen Alias von Ihrer Cloud Foundry-Anwendung referenzieren. 

Nach dem Binden werden die Verbindungsdetails mithilfe der Umgebungsvariablen VCAP_SERVICES in JSON-Formatierung für die Anwendung verfügbar gemacht. Sie können eine Anwendung und einen Service entweder über die [IBM Cloud-Konsole](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_cf_console) oder die [IBM Cloud-Befehlszeilenschnittstelle](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_cf_cli) binden.

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
 

### Anwendung über die IBM Cloud-Befehlszeilenschnittstelle binden
{: #connect_enterprise_cf_cli}

<ol>
<li>Stellen Sie sicher, dass Sie sich in der beabsichtigten Cloud Foundry-Organisation und im entsprechenden Cloud Foundry-Bereich befinden. Sie können interaktiv navigieren, indem Sie den folgenden Befehl ausführen:<br/>
 <code>ibmcloud target --cf</code></li>
<li>Suchen Sie Ihre App:</br>
<code>ibmcloud app list</code><br/>
<br/>
Wenn Sie über eine Manifestdatei verfügen, können Sie eine neue App erstellen, indem Sie folgenden Befehl ausführen:<br/>
<code>ibmcloud app push</code><br/>
<br/>
Da die App noch nicht an {{site.data.keyword.messagehub}} gebunden ist, kann sie keine Verbindung herstellen. Daher wird empfohlen, dass Sie eine Push-Operation der Anwendung mit dem Parameter <code>--no-start</code> durchführen, um unnötige Verbindungsfehler zu vermeiden.</li>
<li>Suchen Sie Ihren Service:</br>
<code>ibmcloud resource service-instances</code></li>
<li>Erstellen Sie einen Cloud Foundry-Servicealias:<br/>
<code>ibmcloud resource service-alias-create <var class="keyword varname">Aliasname</var> --instance-name <var class="keyword varname">Ihr_Servicename</var></code>.</li>
<li>Binden Sie Ihre App an den zuvor erstellten Servicealias:<br/>
<code>ibmcloud service bind <var class="keyword varname">Ihr_Anwendungsname</var> <var class="keyword varname">Aliasname</var></code>.<br/>
<br/>
Alternativ können Sie Ihre Manifestdatei aktualisieren und die Anwendung erneut mit Push-Operation übertragen.</li>
<li>Stellen Sie sicher, dass die Umgebungsvariable VCAP_SERVICES in Ihrer Anwendungslaufzeit verfügbar ist:<br/>
<code>ibmcloud app env <var class="keyword varname">Ihr_Anwendungsname</var></code></li>
<li>Übergeben Sie diese Berechtigungsnachweise an Ihre Anwendung. Geben Sie <code>token</code> als Ihren Benutzernamen an und den <var class="keyword varname">api_key</var> als Kennwort. Trennen Sie <code>token</code> und den <var class="keyword varname">api_key</var> durch einen Doppelpunkt voneinander. Weitere Informationen finden Sie in [Client konfigurieren](/docs/services/EventStreams?topic=eventstreams-kafka_connect). 
<p>Sie müssen möglicherweise ein erneutes Staging Ihrer Anwendung durchführen, damit die Änderungen wirksam werden.</p></li>
</ol>


## Nächste Schritte
{: #after_connecting}

Nachdem Sie über eine Verbindung und Berechtigungsinformationen verfügen, können Sie einen {{site.data.keyword.messagehub}}-Client auswählen. Weitere Informationen finden Sie in [Kafka-API verwenden](/docs/services/EventStreams?topic=eventstreams-kafka_using).

<!--
Charlie said:

"Add some info describing how to take the information made available from above e.g. like the info in the Connecting a client to the Kafka API section of the alpha docs on stage 1? https://console.stage1.bluemix.net/docs/services/EventStreams/eventstreams122.html#alpha_about "
-->







 















