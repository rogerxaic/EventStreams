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


# Gestión de puntos finales de servicio utilizando el plan Empresa
{: #manage_endpoints}

Mediante un punto final interno o privado, Punto final de servicio de {{site.data.keyword.Bluemix_short}} permite la conexión al servicio {{site.data.keyword.messagehub}} a través de la red interna de IBM Cloud. Esta función significa que los datos que publique o que consuma del servicio {{site.data.keyword.messagehub}} van a través de la red privada en lugar de interfaces públicas.
{:shortdesc}

Ahora puede añadir un punto final privado para acceder y gestionar su instancia de servicio de {{site.data.keyword.messagehub}}.

## Requisitos previos
{: #prereqs_endpoint}

Asegúrese de que cumple los requisitos siguientes:
- Cree una instancia de servicio utilizando el plan Empresa. Para obtener más información, consulte [Elección del plan](/docs/services/EventStreams?topic=eventstreams-plan_choose).
- Cree una instancia de servicio en las regiones Dallas (us-south) o Frankfurt (eu-de) de {{site.data.keyword.Bluemix_notm}}.
- Habilite [Virtual Route Forwarding (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) para su cuenta de {{site.data.keyword.Bluemix_notm}}.

## Adición de un punto final privado
{: #add_endpoint}

Para añadir un punto final privado:

* Genere una [incidencia ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){:new_window} para solicitar un punto final privado. Proporcione la información siguiente en la incidencia:

    * Su ID de clúster, si lo sabe.

    Si no sabe el ID de clúster, proporcione el URL de su panel de control, los puntos finales del intermediario
Kafka o el ID de la instancia de servicio.
  

Para obtener más información sobre los puntos finales de servicio, consulte la documentación [Puntos finales de servicio de {{site.data.keyword.Bluemix_notm}}](/docs/resources?topic=resources-service-endpoints#about){:new_window}.


## Tras conmutar a un punto final privado
{: #after_endpoint}

Cuando haya conmutado a un punto final privado, los puntos finales externos o públicos ya no estarán disponibles.


### Acceso a la consola de {{site.data.keyword.messagehub}} de IBM

Cuando un clúster tiene habilitados los puntos finales privados, el URL de administración que se utiliza para acceder a la consola de {{site.data.keyword.messagehub}} cambia.

Solamente se puede llegar a la consola de {{site.data.keyword.messagehub}} desde un URL de administración privado. Para detectar sus puntos finales privados, incluido el URL de administración privado, puede crear una nueva credencial de servicio.

<!--
1. On the service details page, click **Manage endpoints**. You can see the external endpoint assigned to your service instance.
2. Click  **Add internal endpoint**. An internal endpoint is assigned to your service instance.
3. **Optional.** Use the endpoint toggle to enable or disable endpoints as needed.
-->

