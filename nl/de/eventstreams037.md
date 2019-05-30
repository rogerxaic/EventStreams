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

# REST-konforme Verwaltungs-API verwenden
{: #admin_api}

{{site.data.keyword.messagehub}} stellt eine REST-konforme Verwaltungs-API bereit, mit der Sie Topics erstellen, löschen, auflisten und aktualisieren können.
{: shortdesc}

Sie müssen die URL und die Berechtigungsnachweisdetails, die für die Verbindung mit der API erforderlich sind, von einem Serviceberechtigungsnachweisobjekt oder Serviceschlüssel für die Serviceinstanz abrufen. Informationen zum Erstellen dieser Objekte finden Sie in [Verbindung zu {{site.data.keyword.messagehub}} herstellen](/docs/services/EventStreams?topic=eventstreams-connecting).

Die URL für den Endpunkt der API ist in der Eigenschaft <code>kafka_admin_url</code> angegeben.

Die Berechtigungsnachweise hängen von der Authentifizierungsmethode ab, und es werden drei Typen von Berechtigungsnachweisen unterstützt:

* **Authentifizierung mit Basisauthentifizierung**:<br/>
    Verwenden Sie die Eigenschaften <code>user</code> und <code>api_key</code> der oben genannten Objekte als Benutzername und Kennwort. Geben Sie diese Werte im Header <code>Authorization</code> der HTTP-Anforderung in der Form <code>Basic <Base64-Codierung von Benutzername:Kennwort, verbunden durch einen Doppelpunkt (:)></code> an.

* **Authentifizierung mit Trägertoken:**<br/>
    Um Ihr Token über die IBM Cloud-Befehlszeilenschnittstelle abzurufen, melden Sie sich zuerst bei IBM Cloud an und führen Sie dann den folgenden Befehl aus: 

    ```
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

    Stellen Sie dieses Token in den Authentifizierungsheader der HTTP-Anforderung in der Form <code>Bearer <Token></code>. Es werden sowohl API-Schlüssel als auch JWT-Token unterstützt. 

* ** Authentifizierung direkt über API-Schlüssel (api_key):**<br/>
    Geben Sie den Schlüssel direkt als Wert für den HTTP-Header <code>X-Auth-Token</code> an.

Für Serviceinstanzen, die im Plan "Classic" erstellt werden, sind diese Informationen stattdessen in der Umgebungsvariablen VCAP_SERVICES Ihrer Anwendung verfügbar.

Eine Beschreibung der API mit Beispielen finden Sie in [{{site.data.keyword.messagehub}}-Admin-REST ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-messaging/event-streams-docs/tree/master/admin-rest-api){:new_window}.

Sie können die vollständige Spezifikation für die API über die [{{site.data.keyword.messagehub}}-Admin-REST-API-yaml-Datei ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-messaging/event-streams-docs/blob/master/admin-rest-api/admin-rest-api.yaml){:new_window} herunterladen. Verwenden Sie zum Anzeigen der Swagger-Datei Swagger-Tools, zum Beispiel den [Swagger-Editor ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://editor.swagger.io/#/){:new_window}.




