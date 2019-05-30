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

# REST-Producer-API verwenden
{: #rest_producer_using}


** Die REST-Producer-API ist nur als Bestandteil des neuen {{site.data.keyword.messagehub}}-Plans "Standard" verfügbar.**
<br/>

{{site.data.keyword.messagehub}} stellt eine REST-API bereit, die Sie beim Verbinden Ihrer vorhandenen Systeme mit Ihrem {{site.data.keyword.messagehub}}-Kafka-Cluster unterstützt. Mit der API können Sie {{site.data.keyword.messagehub}} in jedes System integrieren, das REST-konforme APIs unterstützt.

Die REST-Producer-API ist eine skalierbare REST-Schnittstelle für die Erstellung von Nachrichten an {{site.data.keyword.messagehub}} über einen sicheren HTTP-Endpunkt. Senden Sie Ereignisdaten an {{site.data.keyword.messagehub}}, verwenden Sie die Kafka-Technologie, um Datenfeeds zu verarbeiten, und nutzen Sie die {{site.data.keyword.messagehub}}-Funktionen zur Verwaltung Ihrer Daten.

Verwenden Sie die API, um vorhandene Systeme mit {{site.data.keyword.messagehub}} zu verbinden. Erstellen Sie Erstellungsanforderungen aus Ihren Systemen in {{site.data.keyword.messagehub}}, einschließlich der Angabe des Nachrichtenschlüssels, der Header und der Topics, an die Nachrichten geschrieben werden sollen.


## Nachrichten mit REST erstellen
{: #rest_produce_messages}

Verwenden Sie die Producer-API, um Nachrichten an Topics zu schreiben. Damit Sie Nachrichten an ein Topic senden können, müssen Sie über die folgenden Informationen verfügen:

* Die URL des {{site.data.keyword.messagehub}}-API-Endpunkts, einschließlich der Portnummer.
* Das Topic, an das Sie Nachrichten senden möchten.
* Der API-Schlüssel, der die Berechtigung zum Herstellen der Verbindung zu dem ausgewählten Topic und zum Senden von Nachrichten an dieses Topic erteilt.

Sie müssen die URL und die Berechtigungsnachweisdetails, die für die Verbindung mit der API erforderlich sind, von einem Serviceberechtigungsnachweisobjekt oder Serviceschlüssel für die Serviceinstanz abrufen. Informationen zum Erstellen dieser Objekte finden Sie in [Verbindung zu {{site.data.keyword.messagehub}} herstellen](/docs/services/EventStreams?topic=eventstreams-connecting).

Die URL für den Endpunkt der API ist in der Eigenschaft <code>kafka_http_url</code> angegeben.

Verwenden Sie eine der folgenden Methoden zur Authentifizierung:

* **Authentifizierung mit Basisauthentifizierung:**<br/>
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

<br/>
Der folgende Code zeigt ein Beispiel für das Senden einer Nachricht unter Verwendung von 'curl':

```
curl -v -X POST -H "Authorization: Basic <Base64-Benutzername:Kennwort>" -H "Content-Type: text/plain" -H "Accept: application/json" -d 'test message' "<Kafka-http-url>/topics/<Topicname>/records"
```
{: codeblock}


## API-Referenz
{: #rest_api_reference}

Vollständige Informationen zur API finden Sie in der
[Referenzdokumentation zur {{site.data.keyword.messagehub}}-REST-Producer-API![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://ibm.github.io/event-streams/api/){:new_window}.












