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


# Erste Schritte mit dem Alpha-Programm
{: #alpha_program}

Das Alpha-Programm bietet einen Schnellzugriff auf die nächste Version des {{site.data.keyword.messagehub}}-Service. 

Gehen Sie folgendermaßen vor, um eine App mit {{site.data.keyword.messagehub}} Alpha auszuführen:


## Den {{site.data.keyword.messagehub}}-Service erstellen
{: alpha_create}


  1. Klicken Sie auf die Kachel **Message Hub vNext - Production**, einem experimentellen Service im [Katalog ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.stage1.bluemix.net/catalog/labs/?search=vnext).</li>

  2. Erstellen Sie einen {{site.data.keyword.messagehub}}-Service. Dieser Dienst bietet einen Single-Tenant-Cluster nach dem <code>Premium</code>-Preistarif und dauert in der Regel 1 bis 3 Stunden. 
 


## Berechtigungsnachweise mit der Konsolenoption abrufen: eine Testapp erstellen und verbinden
{: alpha_app}

Um mit {{site.data.keyword.messagehub}} zu arbeiten, benötigen Sie Berechtigungsnachweise.
Wenn Sie noch keine App haben, die Sie verwenden können, erstellen Sie eine Testapp. Verwenden Sie die Berechtigungsnachweise, mit der sich diese App mit {{site.data.keyword.messagehub}} verbindet. Beispiel: Verwenden Sie für die Testapp den Service **SDK for Node.js**. 

  1. Navigieren Sie zur Kachel **SDK for Node.js** im [Katalog ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.stage1.bluemix.net/catalog/starters/sdk-for-nodejs).
   
  Bevor Sie einen App-Namen eingeben, stellen Sie sicher, dass Sie eine Region im Süden der USA auswählen. Erstellen Sie die App.

  2. Wenn die App ausgeführt wird, klicken Sie links auf die Registerkarte **Verbindungen**.

  3. Klicken Sie auf die Schaltfläche **Verbindung erstellen**.

  4. Wählen Sie Ihren neuen {{site.data.keyword.messagehub}}-Service aus der Liste kompatibler Services aus und klicken Sie auf die Schaltfläche **Verbindung herstellen**.

  5. Akzeptieren Sie im Fenster **Für IAM aktivierter Service** die Standardeinstellungen und klicken Sie auf **Verbindung herstellen**.
  Stellen Sie sicher, dass der {{site.data.keyword.messagehub}}-Service bereitgestellt wird, damit Sie eine Verbindung zu ihm herstellen können.

  6. Klicken Sie links auf die Registerkarte **Laufzeit** und wählen Sie die Registerkarte **Umgebungsvariablen** in der Mitte aus. Suchen Sie im Abschnitt **VCAP_SERVICES** die Informationen <code>kafka_admin_url</code>, <code>apikey</code> und <code>kafka_brokers_sasl</code>, die zum Senden einer Nachricht erforderlich sind.
  
## Berechtigungsnachweise mit der Befehlszeilenoption abrufen
Alternativ können Sie die erforderlichen Berechtigungsnachweise über die Befehlszeile abrufen. Führen Sie die folgenden Schritte aus:

  1. Installieren Sie das {{site.data.keyword.Bluemix_notm}}-Befehlszeilentool über die [{{site.data.keyword.Bluemix_notm}}-Befehlszeilenschnittstelle (CLI) ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/cli/index.html#overview).
  
  2. Melden Sie sich bei der {{site.data.keyword.Bluemix_notm}}-Befehlszeilenschnittstelle (CLI) an.
  
  3. Verwenden Sie die {{site.data.keyword.Bluemix_notm}}-Befehlszeilenschnittstelle (CLI), um einen Serviceschlüssel mit der Rolle für Managementaufgaben mit dem folgenden Befehl zu erstellen:
  ```
  bx resource service-key-create <NAME> Manager --instance-name <MESSAGEHUB_SERVICE_INSTANCE_NAME>
  ```
  4. Die Ausgabe enthält den API-Schlüssel, die Administrator-REST-Endpunkt-URL und die Brokerliste. Sie können diese Informationen erneut anzeigen, indem Sie den folgenden Befehl ausführen:
  ```
  bx resource service-key <NAME>
  ```

## Ein {{site.data.keyword.messagehub}}-Topic erstellen und eine Nachricht senden

Sie können einen CURL-Befehl verwenden, um ein Topic und dann das kafkacat-Tool zu erstellen, um eine Nachricht zu erstellen und zu verarbeiten. 

Ersetzen Sie in jedem Befehl APIKEY und KAFKA_ADMIN_URL durch die Werte Ihrer Umgebungsvariable VCAP_SERVICES.

  1. Erstellen Sie in der Befehlszeile ein {{site.data.keyword.messagehub}}-Topic mit dem folgenden CURL-Befehl:
  
    ```
    curl -i -X POST -H "Content-Type: application/json" -H "X-Auth-Token: APIKEY" --data '{ "name": "newtop"}' KAFKA_ADMIN_URL/admin/topics
    ```
    {: codeblock}

  2. Installieren Sie das [kafkacat-Tool![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/edenhill/kafkacat#install), das für einen Kafka-Schnelltest nützlich ist.
  
  3. Sie benötigen die folgenden Informationen, um die nächsten Befehle auszuführen:
  
    * Ihre Brokerliste, die in Ihren Anmeldeinformationen `kafka_brokers_sasl` zurückgegeben wurde. Ihre Brokerliste muss eine durch Kommas getrennte Liste für kafkacat sein.
	Nur Ihre ersten fünf Broker sind in VCAP_SERVICES aufgeführt. Wenn Sie mehr als fünf Broker haben, verwenden Sie einen Kafka-Client, um die Details Ihrer anderen Broker abzurufen. 
  
    * Ihr <code>API-Schlüssel</code>. Die ersten 8 Zeichen aus Ihrer sasl.username und der Rest des <code>API-Schlüssels</code> bilden Ihr sasl.password.
    * Die Position Ihrer SSL-Zertifikate. Unter Ubuntu befindet sich SSL_CERTS_DIRECTORY unter <code>/etc/ssl/certs/</code>.
  
  4. Erzeugen Sie einige Nachrichten, indem Sie einen Befehl wie den folgenden ausführen:
  ```
  kafkacat -X "security.protocol=sasl_ssl" -X 'sasl.mechanisms=PLAIN' -X 'sasl.username=<FIRST_8_CHARS_FROM_APIKEY>' -X 'sasl.password=<REMAINING_CHARS_FROM_APIKEY>' -X "ssl.ca.location=<SSL_CERTS_DIRECTORY>" -b <BROKERS_LIST> -P -t <TOPIC_NAME>
    ```
		
	<br/>
  Geben Sie nach dem Ausführen des Befehls Text wie <code>HelloWorld</code> in das Producer-Terminal ein.
  
  5. Verarbeiten Sie die Nachrichten, indem Sie einen Befehl wie den folgenden ausführen:
  ```
  kafkacat -X "security.protocol=sasl_ssl" -X 'sasl.mechanisms=PLAIN' -X 'sasl.username=<FIRST_8_CHARS_FROM_APIKEY>' -X 'sasl.password=<REMAINING_CHARS_FROM_APIKEY>' -X "ssl.ca.location=<SSL_CERTS_DIRECTORY>" -b <BROKERS_LIST> -C -t <TOPIC_NAME> -f 'Topic %t [%p] at offset %o: key %k: %s\n'
  ```
	
	<br/>
  Jetzt sollte <code>HelloWorld</code> im Consumer-Terminal angezeigt werden.

