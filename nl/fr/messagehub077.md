---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-30"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Activation de l'API {{site.data.keyword.mql}} dans {{site.data.keyword.messagehub}}
{: #mql_enable}

**L'API MQ Light est disponible uniquement dans le cadre du plan Standard.**
<br/>

**Vous devez créer de façon explicite un sujet Kafka nommé "MQLight" pour pouvoir utiliser l'API car tous les messages passent par le sujet "MQLight". Ce sujet doit posséder une partition unique. La création de ce sujet active l'API MQ Light pour votre instance de service. **  

Le sujet "MQLight" est utilisé par l'API MQ Light pour stocker ses données de messages et interagir avec d'autres clients Kafka. Notez que la création de ce sujet entraîne la facturation de frais au tarif standard indiqué dans le plan de paiement des services.

Pour désactiver l'API MQ Light, supprimez le sujet "MQLight". La suppression de ce sujet entraîne la destruction de toutes les données.
