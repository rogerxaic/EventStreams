---

copyright:
  years: 2015, 2018
lastupdated: "2016-11-22"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


#Verschlüsselungsprotokolle
{: #cryptographic}


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
