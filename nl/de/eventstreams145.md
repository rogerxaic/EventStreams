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


# Serviceendpunkte im Rahmen des Enterprise-Plans verwalten
{: #manage_endpoints}

Mithilfe eines internen oder privaten Endpunkts ermöglicht {{site.data.keyword.Bluemix_short}} Service Endpoint die Verbindung zum {{site.data.keyword.messagehub}}-Service über das interne IBM Cloud-Netz. Durch diese Funktionalität werden alle mit dem {{site.data.keyword.messagehub}}-Service publizierten oder verarbeiteten Daten über das private Netz, nicht über öffentliche Schnittstellen, übertragen.
{:shortdesc}

Sie können nun einen privaten Endpunkt hinzufügen, um auf Ihre {{site.data.keyword.messagehub}}-Serviceinstanzen zuzugreifen und diese zu verwalten. 

## Voraussetzungen
{: #prereqs_endpoint}

Stellen Sie sicher, dass die folgenden Anforderungen erfüllt sind:
- Erstellen Sie Ihre Serviceinstanz im Rahmen des Enterprise-Plans. Weitere Informationen finden Sie in [Plan auswählen](/docs/services/EventStreams?topic=eventstreams-plan_choose).
- Erstellen Sie Ihre Serviceinstanz in der {{site.data.keyword.Bluemix_notm}}-Region Dallas (us-south) oder Frankfurt (eu-de). 
- Aktivieren Sie [Virtual Route Forwarding (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) für Ihr {{site.data.keyword.Bluemix_notm}}-Konto. 

## Privaten Endpunkt hinzufügen
{: #add_endpoint}

Gehen Sie wie folgt vor, um einen privaten Endpunkt hinzuzufügen:

* Öffnen Sie ein [Ticket ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){:new_window}, mit dem Sie einen privaten Endpunkt anfordern. Geben Sie im Ticket die folgenden Informationen an:

    * Ihre Cluster-ID, falls Sie diese kennen. 

    Wenn Ihnen die Cluster-ID nicht bekannt ist, geben Sie stattdessen die URL Ihres Dashboards, die Kafka-Broker-Endpunkte oder die Serviceinstanz-ID an. 
  

Weitere Informationen zu Serviceendpunkten finden Sie in der Dokumentation zu [{{site.data.keyword.Bluemix_notm}} Service Endpoint](/docs/resources?topic=resources-service-endpoints#about){:new_window}. 


## Nach dem Wechsel zu einem privaten Endpunkt
{: #after_endpoint}

Nach dem Wechsel zu einem privaten Endpunkt stehen Ihnen die externen oder öffentlichen Endpunkte nicht mehr zur Verfügung. 


### Zugriff auf die IBM {{site.data.keyword.messagehub}}-Konsole

Wenn für einen Cluster private Endpunkte aktiviert sind, ändert sich die Admin-URL, die Sie für den Zugriff auf die {{site.data.keyword.messagehub}}-Konsole verwenden. 

Die {{site.data.keyword.messagehub}}-Konsole ist nur über eine private Admin-URL erreichbar. Sie können einen neuen Serviceberechtigungsnachweis erstellen, um die privaten Endpunkte und die private Admin-URL zu ermitteln. 

<!--
1. On the service details page, click **Manage endpoints**. You can see the external endpoint assigned to your service instance.
2. Click  **Add internal endpoint**. An internal endpoint is assigned to your service instance.
3. **Optional.** Use the endpoint toggle to enable or disable endpoints as needed.
-->

