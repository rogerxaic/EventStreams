---

copyright:
  years: 2015, 2019
lastupdated: "2018-11-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Liaison à d'autres services à l'aide de ponts
{: #bridges}

**Des ponts {{site.data.keyword.messagehub}} sont disponibles dans le cadre du plan Standard uniquement.**
<br/>

Le plan Standard {{site.data.keyword.messagehub}} prend également
en charge des ponts vers une sélection d'autres systèmes. Les ponts sont des liens unidirectionnels entre {{site.data.keyword.messagehub}} et un autre service. Ils
permettent la lecture de données à partir de {{site.data.keyword.messagehub}} et leur écriture dans un autre service, ou la lecture de données à partir d'un autre service
et leur écriture dans {{site.data.keyword.messagehub}}. Il
peut prendre des messages provenant d'un autre système et les publier sur un sujet,
ou consommer les messages d'un sujet et les envoyer à l'autre système. Vous pouvez ainsi
utiliser {{site.data.keyword.messagehub}} et l'intégrer à d'autres systèmes sans écrire de code.
{:shortdesc}

Les principaux avantages de l'utilisation de ponts {{site.data.keyword.messagehub}} sont les suivants :  

* Vous pouvez définir les ponts de façon administrative pour ne pas avoir à rédiger le code de l'application.
* Le cycle de vie de chaque pont est surveillé et géré par le service {{site.data.keyword.messagehub}}. Par exemple, si un pont rencontre une erreur, il est automatiquement redémarré par {{site.data.keyword.messagehub}}.
* Les ponts sont intégrés à la plateforme {{site.data.keyword.Bluemix_short}}. Ainsi, les informations de journalisation et de surveillance générées par les ponts sont acheminées vers les tableaux de bord Kibana et Grafana.

Les ponts sont utiles dans les deux cas courants suivants :

* La consommation de données depuis une ou plusieurs sources de données vers {{site.data.keyword.messagehub}}.
* Le transfert des données depuis {{site.data.keyword.messagehub}} vers un autre service, par exemple, le stockage à long terme.

## Ponts {{site.data.keyword.messagehub}}
{: notoc}

* Les types de pont suivants sont fournis : 
  - Le [pont MQ](/docs/services/EventStreams/eventstreams105.html){:new_window}, qui prend les messages depuis {{site.data.keyword.IBM}} MQ et les transfère vers un sujet dans {{site.data.keyword.messagehub}}. La prise en charge d'une gamme plus vaste de ponts est prévue à l'avenir.
  - Le [pont Cloud Object Storage](/docs/services/EventStreams/eventstreams115.html){:new_window}, qui transfère des données {{site.data.keyword.messagehub}} vers une instance du service [{{site.data.keyword.IBM_notm}} Cloud Object Storage ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/services/cloud-object-storage/about-cos.html){:new_window}. 
  - Le [pont {{site.data.keyword.objectstorageshort}}](/docs/services/EventStreams/eventstreams089.html){:new_window} a été déprécié le 1er août 2018. Pour en savoir plus, veuillez consulter [l'annonce d'obsolescence : {{site.data.keyword.objectstorageshort}} OpenStack Swift (PaaS) ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/blogs/bluemix/2018/05/end-marketing-object-storage-openstack-swift-paas/){:new_window}.
* Actuellement, les ponts sont disponibles dans tous les environnements {{site.data.keyword.Bluemix_notm}} Public. Les ponts ne sont pas disponibles dans {{site.data.keyword.Bluemix_short}} Dedicated.
* Vous pouvez administrez les ponts des deux manières suivantes :
  - Grâce à une [API REST ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-messaging/event-streams-docs){:new_window}, qui est une extension de l'API d'administration {{site.data.keyword.messagehub}} existante. Vous trouverez des exemples d'utilisation de curl pour la gestion du cycle de vie des ponts sur la page [message-hub-docs ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-messaging/event-streams-docs){:new_window}. Cette API REST est susceptible d'être modifiée au fur et à mesure du développement des ponts. Notre souhait est de la stabiliser.
  - Grâce au tableau de bord {{site.data.keyword.messagehub}} dans la console {{site.data.keyword.Bluemix_notm}}.
* Vous pouvez associer un maximum de deux ponts de n'importe quel type à une instance du service {{site.data.keyword.messagehub}}. Cette limitation est susceptible d'être modifiée au fur et à mesure du développement des ponts.
* L'utilisation des ponts au-delà de leur fonctionnement de messagerie n'entraîne pas de frais supplémentaires.
* Le pont MQ ne prend pas en charge l'utilisation de SSL/TLS pour la protection de la confidentialité et de l'intégrité des données lors de leur transfert entre le pont et le gestionnaire de files d'attente MQ. La prise en charge de SSL/TLS pour les ponts est prévue ultérieurement. 
* Vous pouvez cependant utiliser le service [{{site.data.keyword.SecureGatewayfull}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/services/SecureGateway/index.html#getting-started-with-sg){:new_window} pour envoyer vos données via un tunnel sécurisé entre {{site.data.keyword.Bluemix_notm}} et un client {{site.data.keyword.SecureGateway}} que vous pouvez installer sur site. Dans cette configuration, la communication à l'une ou l'autre extrémité du tunnel n'est pas sécurisée via SSL/TLS.
* Le pont Cloud Object Storage concatène les messages en utilisant des caractères de retour à la ligne comme séparateurs lorsqu'il écrit les
données dans Cloud Object Storage. Ce pont ne convient donc ni aux messages contenant des caractères de retour à la ligne imbriqués, ni aux données de message binaires.
* Les conventions de dénomination des objets actuellement utilisées par le pont Cloud Object Storage sont susceptibles de changer à l'avenir.

## Ponts d'autres services dans {{site.data.keyword.messagehub}}
{: notoc}

* {{site.data.keyword.iot_full}} fournit son propre pont [ dans {{site.data.keyword.messagehub}}](/docs/services/EventStreams/eventstreams119.html){:new_window}. Le pont offre une liaison unidirectionnelle vers {{site.data.keyword.messagehub}}, qui permet de stocker des données d'historique. En connectant {{site.data.keyword.messagehub}} à {{site.data.keyword.iot_short_notm}}, vous pouvez utiliser {{site.data.keyword.messagehub}} en tant que pipeline d'événement pour consommer vos événements de périphérique depuis la plateforme Watson IoT Platform et rendre les événements disponibles en temps réel pour le reste de la plateforme. 


