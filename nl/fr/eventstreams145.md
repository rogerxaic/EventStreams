---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-24"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka, service endpoints

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Gestion des noeuds finaux de service en utilisant le plan Enterprise
{: #manage_endpoints}

En utilisant un noeud final interne ou privé, {{site.data.keyword.Bluemix_short}} Service Endpoint active la connexion au service {{site.data.keyword.messagehub}} via le réseau IBM Cloud interne. Cette fonctionnalité signifie que toutes les données que vous publiez ou consommez à partir du service {{site.data.keyword.messagehub}} se trouvent sur le réseau privé et non sur les interfaces publiques.
{:shortdesc}

Vous pouvez désormais ajouter un noeud final privé pour accéder à votre instance de service {{site.data.keyword.messagehub}} et la gérer.

## Prérequis
{: #prereqs_endpoint}

Vérifiez que vous remplissez les conditions suivantes :
- Créez votre instance de service en utilisant le plan Enterprise. Pour plus d'informations, voir [Choix d'un plan](/docs/services/EventStreams?topic=eventstreams-plan_choose).
- Créez votre instance de service dans les régions {{site.data.keyword.Bluemix_notm}} Dallas (us-south) ou Francfort (eu-de).
- Activez [Virtual Route Forwarding (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) pour votre compte {{site.data.keyword.Bluemix_notm}}.

## Ajout d'un noeud final privé
{: #add_endpoint}

Pour ajouter un noeud final privé :

* Générez un [ticket ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){:new_window} pour demander un noeud final privé. Fournissez les informations suivantes dans le ticket :

    * Votre ID de cluster, si vous le connaissez.

    Si ce n'est pas le cas, fournissez à la place votre URL de tableau de bord, les noeuds finaux du courtier Kafka ou votre ID d'instance de service.
  

Pour plus d'informations sur les noeuds finaux de service, voir [Documentation {{site.data.keyword.Bluemix_notm}} Service Endpoint](/docs/resources?topic=resources-service-endpoints#about){:new_window}.


## Après le passage à un noeud final privé
{: #after_endpoint}

Quand vous avez basculé vers un noeud final privé, vous ne pouvez plus accéder aux noeuds finaux externes ou publics.


### Accès à la console IBM {{site.data.keyword.messagehub}}

Quand un cluster a des noeuds finaux privés activés, l'URL admin que vous utilisez pour accéder à la console {{site.data.keyword.messagehub}} change.

La console {{site.data.keyword.messagehub}} n'est accessible que depuis une URL admin privée. Pour reconnaître vos noeuds finaux privés, y compris l'URL admin privée, vous devez créer de nouvelles données d'identification de service.

<!--
1. On the service details page, click **Manage endpoints**. You can see the external endpoint assigned to your service instance.
2. Click  **Add internal endpoint**. An internal endpoint is assigned to your service instance.
3. **Optional.** Use the endpoint toggle to enable or disable endpoints as needed.
-->

