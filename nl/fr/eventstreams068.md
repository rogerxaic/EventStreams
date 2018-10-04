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


#Sécurité et confidentialité des données
{: #data_security}


{{site.data.keyword.IBM}} utilise les techniques suivantes pour garantir la sécurité et
la confidentialité de vos données :
{:shortdesc}

## Protocoles de chiffrement
{: #cryptographic notoc}


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
   
## Chiffrement des charges de message, des noms de sujet et des groupes de consommateurs
{: #encryption_payloads notoc}

Le protocole TLS chiffre les données des messages en vue de leur transmission entre {{site.data.keyword.messagehub}} et les clients. {{site.data.keyword.messagehub}}
stocke les données des messages au repos et les journaux des messages sur des disques chiffrés.

Le protocole TLS chiffre les noms de sujet et les groupes de consommateurs en vue de leur transmission entre {{site.data.keyword.messagehub}} et les clients. Cependant, {{site.data.keyword.messagehub}} ne chiffre pas ces valeurs au repos. Il est par conséquent déconseillé d'utiliser des informations confidentielles dans vos noms de sujet.



