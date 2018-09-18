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


#Protocolli crittografici
{: #cryptographic}


*  Le connessioni alle interfacce REST e native Kafka devono essere effettuate utilizzando TLS 1.2.
*  Le connessioni sono limitate alle seguenti suite di cifratura complesse:

      * ECDHE-RSA-AES128-GCM-SHA256
      * ECDHE-RSA-AES256-GCM-SHA384
      * DHE-RSA-AES128-GCM-SHA256
      * kEDH+AESGCM
      * ECDHE-RSA-AES128-SHA256
      * ECDHE-RSA-AES256-SHA384
      * DHE-RSA-AES128-SHA256
      * DHE-RSA-AES256-SHA256



*  Per accedere al dashboard
                        {{site.data.keyword.messagehub}},
                    devi utilizzare un browser che supporta TLS 1.2.
