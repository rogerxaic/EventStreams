---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

<!-- Notes from chat with Charlie 

Different plan for provisioning

Quality of service from each plan

Life of a user through cycle - APIs, feature sets

-->

# Informationen zum Alpha-Programm
{: #alpha_about }

Das Alpha-Programm bietet einen Schnellzugriff auf neue Funktionen im {{site.data.keyword.messagehub}}-Service. Das aktuelle Feature ist die Einführung eines neuen Plans: {{site.data.keyword.messagehub}}-Premium-Plan.

## Premium-Plan
{: premium_plan}

Der Premium-Plan wurde für Benutzer entwickelt, die über Leistungs- und funktionale Anforderungen verfügen, die über den öffentlichen Service hinausgehen. Er bietet eine Single-Tenant-Version des {{site.data.keyword.messagehub}}-Services an, bei dem Sie ausschließlich einen Apache Kafka-Cluster verwenden. Mit dieser Version können Sie Folgendes:

* Nutzen Sie die Kapazität und Leistung des Clusters optimal aus.

* Erhöhen Sie die Grenzwerte und die Anzahl der Partitionen.

* Profitieren Sie von Null-Verwaltungskosten mit einem vollständig verwalteten Cluster, der automatisch gepflegt und aktualisiert wird.

## Informationen zu Ihrem Alpha-Cluster
{: alpha_cluster}

Ihr Alpha-Cluster wird mit der Apache Kafka Version 1.1 bereitgestellt und kann einen maximalen Nachrichtendurchsatz von 90.000 KB pro Sekunde liefern. 

Sie können maximal 1.000 Partitionen erstellen. Jede Partition kann maximal 1 GB Daten für bis zu 30 Tage speichern. Aus Gründen der Ausfallsicherheit werden Daten über 3 Replikate hinweg gespeichert. Der festgeschriebene Offset für jede Partition wird maximal 7 Tage gespeichert. 

Die folgenden zwei APIs werden unterstützt:

* Für das Messaging werden Kafka-Clients ab der Version 0.10.x unterstützt, einschließlich der Möglichkeit, Kafka Streams, Kafka Connect und KSQL zu verwenden.

* Für die Verwaltung steht eine REST-API zum Erstellen, Löschen und Auflisten von Themen zur Verfügung. 

Das Alpha-Programm ist nur für die Region "USA (Süden)" verfügbar. Der Zugriff auf Cluster wird über IAM verwaltet.

## Eine Verbindung zu Ihrem Cluster herstellen
{: alpha_connect}

Um eine Verbindung im Cluster zu einer API herzustellen, benötigen Sie die Endpunkt-URL und einen API-Schlüssel zur Authentifizierung. Sie können diese Details mit einer der folgenden Methoden von IAM abrufen:

### Cloud Foundry-Anwendung
Für eine Cloud Foundry-Anwendung:
1. Klicken Sie in der Registerkarte **Verbindungen** für die App (links) auf die Schaltfläche **Verbindung erstellen**. 
2. Wählen Sie die Instanz des {{site.data.keyword.messagehub}}-Services aus, den Sie verbinden möchten, und klicken Sie auf **Verbindung herstellen**. Akzeptieren Sie die Standardoptionen.  
3. Wenn die Verbindung hergestellt wurde, klicken Sie auf die Registerkarte **Laufzeit** für die App. Klicken Sie dann auf **Umgebungsvariablen**, um **VCAP_SERVICES** anzuzeigen.

### Konsole für eine externe Anwendung
Erstellen Sie über die Konsole für eine externe Anwendung einen Service-API-Schlüssel, indem Sie den folgenden Befehl **bx** verwenden: 

```
bx resource service-key-create name-of-key Manager --instance-name name-of-your-service
``` 

Kopieren Sie die Felder <code>kafka_brokers_sasl</code>, <code>kafka_admin_url</code> und <code>apikey</code> aus den generierten Informationen.
Nur Ihre ersten fünf Broker sind in VCAP_SERVICES aufgeführt. Wenn Sie mehr als fünf Broker haben, verwenden Sie einen Kafka-Client, um die Details Ihrer anderen Broker abzurufen. 

## Einen Client mit der Kafka-API verbinden

Führen Sie die folgenden Schritte aus, um einen Client mit der Kafka-API zu verbinden:

1. Legen Sie die Client-Eigenschaft <code>bootstrap.servers</code> auf eine durch Kommas getrennte Brokerliste in <code>kafka_brokers_sasl</code> fest.

2. Legen Sie das Client-USERNAME-Feld <code>sasl.jaas.config</code> auf die ersten 8 Zeichen von <code>apikey</code> und das PASSWORD-Feld auf die restlichen Zeichen fest. (Diese Aufteilung wird in zukünftigen Versionen nicht mehr benötigt.)

Der von Ihnen verwendete Kafka-Client muss folgende Funktionen unterstützen: 

* Client-Version 0.10.x oder höher

* Authentifizierung mithilfe des SASL Plain-Mechanismus

* Die SNI-Erweiterung (Service Name Identification) für das TLS v1.2-Protokoll

Diese Methode zum Abrufen der Endpunkt- und Berechtigungsinformationen unterscheidet sich vom {{site.data.keyword.messagehub}}-Service. Apps, die zurzeit für {{site.data.keyword.messagehub}} ausgeführt werden, müssen geändert werden, damit die alternativen Feldnamen angezeigt werden, die von VCAP_SERVICES und von den an Kafka übermittelten Benutzernamen/Kennwort-Feldern benötigt werden. Diese Änderungen werden in zukünftigen Versionen von Alpha nicht mehr benötigt.

## Einen Client mit der REST-API verbinden

Führen Sie die folgenden Schritte aus, um einen Client mit der REST-API zu verbinden:

* Die URI für die REST-API wird in <code>kafka_admin_url</code> bereitgestellt.

* Legen Sie den HTTP-Header <code>Content-Type</code> auf <code>application/json</code> fest.

* Legen Sie den HTTP-Header <code>X-Auth-Token</code> auf den Wert von <code>apikey</code> fest.

Informationen zur Einrichtung und Ausführung von Alpha finden Sie in [Einführung in das Alpha-Programm](/docs/services/MessageHub/messagehub120.html).


## {{site.data.keyword.messagehub}} verwalten
{: alpha_admin}

Die einzigen Verwaltungsaufgaben, die in einem Cluster erforderlich sind, sind das Erstellen, Auflisten und Löschen der benötigten Themen. Sie können die Verwaltung über eine der folgenden Methoden durchführen:

* Mit den Kafka-Administrator-APIs direkt in Ihrer Anwendung. Beispiel für Java: Verwendung der Methoden <code>createTopics()</code>, <code>deleteTopics()</code> oder <code>listTopics()</code> von [AdminClient ![Symbol für externen Link](http://kafka.apache.org/11/javadoc/index.html?org/apache/kafka/clients/admin/AdminClient.html){:new_window}.

* Interaktiv mithilfe der Webbenutzerschnittstelle für die Serviceinstanz, die im IBM Cloud-Portal verfügbar ist. 

* Die Administrator-REST-API, die im Cluster bereitgestellt wird.

Weitere Informationen zu den Funktionen zum Erstellen, Auflisten und Löschen, die von der REST-API für Administratoren bereitgestellt werden (die mit der vorhandenen {{site.data.keyword.messagehub}}-Administrator-API kompatibel ist), finden Sie in der vollständigen Spezifikation für die über [admin-rest-api.yaml ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-messaging/message-hub-docs/blob/master/admin-rest-api/admin-rest-api.yaml){:new_window} verfügbare API.
Um die Swagger-Datei anzuzeigen, verwenden Sie Swagger-Tools, zum Beispiel [Swagger Editor ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://editor.swagger.io/#/){:new_window}.

Ein einfaches Beispiel, das zeigt, wie ein Topic mit Curl erstellt wird, finden Sie unter [Einführung in das Alpha-Programm](/docs/services/MessageHub/messagehub120.html).

In Zukunft werden auch andere Konfigurationsoptionen verfügbar sein. 


## Beispiele

Bald verfügbar...

Informationen zur Einrichtung und Ausführung von Alpha finden Sie in [Einführung in das Alpha-Programm](/docs/services/MessageHub/messagehub120.html).

## Alpha-Einschränkungen
{: alpha_limitations}

Die aktuellen Einschränkungen des Alpha-Programms sind wie folgt:

### Bald verfügbar

* Unterstützung für mehrere Verfügbarkeitszonen

* Benutzergesteuerte Skalierung und Auslastungsgrenzen

* Integration in den {{site.data.keyword.cloudaccesstrailfull_notm}}-Service (früher AccessTrail genannt) 

### Noch nicht geplant

* Bridges

* REST-Messaging

* MQ Light-APIs










