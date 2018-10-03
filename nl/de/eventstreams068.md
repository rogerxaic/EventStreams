---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


#Datensicherheit und Datenschutz
{: #data_security}


{{site.data.keyword.IBM}} verwendet die folgenden Methoden, um die Datensicherheit und den
Datenschutz zu gewährleisten:
{:shortdesc}

## Verschlüsselungsprotokolle
{: #cryptographic notoc}


*  Verbindungen zu nativen Kafka- sowie zu REST-Schnittstellen müssen mithilfe von TLS 1.2 hergestellt werden.
*  Verbindungen sind auf die folgenden starken Cipher-Suites begrenzt:

      * ECDHE-RSA-AES128-GCM-SHA256
      * ECDHE-RSA-AES256-GCM-SHA384
      * DHE-RSA-AES128-GCM-SHA256
      * kEDH+AESGCM
      * ECDHE-RSA-AES128-SHA256
      * ECDHE-RSA-AES256-SHA384
      * DHE-RSA-AES128-SHA256
      * DHE-RSA-AES256-SHA256



*  Für den Zugriff auf das
{{site.data.keyword.messagehub}}-Dashboard
müssen Sie einen Browser verwenden, der TLS 1.2 unterstützt.
   
## Verschlüsselung von Nachrichtennutzdaten, Abschnittsnamen und Consumergruppen
{: #encryption_payloads notoc}

Nachrichtendaten werden für die Übertragung zwischen {{site.data.keyword.messagehub}} und Clients als Ergebnis von TLS verschlüsselt. {{site.data.keyword.messagehub}} speichert ruhende Nachrichtendaten
und Nachrichtenprotokolle auf verschlüsselten Datenträgern.

Abschnittsnamen und Consumergruppen werden für die Übertragung zwischen {{site.data.keyword.messagehub}} und Clients als Ergebnis von TLS verschlüsselt. {{site.data.keyword.messagehub}} verschlüsselt jedoch diese ruhenden Werte nicht. Daher wird empfohlen, keine vertraulichen Informationen in Topicnamen fzu verwenden.



