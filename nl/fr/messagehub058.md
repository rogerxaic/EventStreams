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


#Protocoles de chiffrement
{: #cryptographic}


*  Les connections aux interfaces REST et natives Kafka doivent être établies avec le protocole TLS 1.2.
*  Elles ne peuvent utiliser que des suites de chiffrement fortes suivantes :

      * ECDHE-RSA-AES128-GCM-SHA256
      * ECDHE-RSA-AES256-GCM-SHA384
      * DHE-RSA-AES128-GCM-SHA256
      * kEDH+AESGCM
      * ECDHE-RSA-AES128-SHA256
      * ECDHE-RSA-AES256-SHA384
      * DHE-RSA-AES128-SHA256
      * DHE-RSA-AES256-SHA256



*  Pour accéder au tableau de bord
{{site.data.keyword.messagehub}}, vous devez utiliser un navigateur qui prend en charge le protocole TLS 1.2.
