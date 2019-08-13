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


# Gerenciando terminais de serviço usando o plano Enterprise
{: #manage_endpoints}

Usando um terminal interno ou privado, o terminal em serviço do {{site.data.keyword.Bluemix_short}} permite a conexão com o serviço {{site.data.keyword.messagehub}} por meio da rede interna do IBM Cloud. Essa habilidade significa que qualquer dado que você publicar ou consumir por meio do serviço {{site.data.keyword.messagehub}}
será na rede privada e não nas interfaces públicas.
{:shortdesc}

Agora é possível incluir um terminal privado para acessar e gerenciar a sua instância de serviço do {{site.data.keyword.messagehub}}.

## Pré-Requisitos
{: #prereqs_endpoint}

Assegure-se de atender aos requisitos a seguir:
- Crie a sua instância de serviço usando o plano Enterprise. Para obter mais informações, consulte [Escolhendo o seu plano](/docs/services/EventStreams?topic=eventstreams-plan_choose).
- Crie a sua instância de serviço na região Dallas (us-south) ou Frankfurt (eu-de) do {{site.data.keyword.Bluemix_notm}}.
- Ative o [Virtual Route Forwarding (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) para a sua conta do {{site.data.keyword.Bluemix_notm}}.

## Incluindo um terminal privado
{: #add_endpoint}

Para incluir um terminal privado:

* Crie um [chamado ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){:new_window} para solicitar um terminal privado. Forneça as informações a seguir no chamado:

    * Seu ID do cluster, se você souber.

    Se você não souber o ID do cluster, forneça a URL do painel, os terminais de broker do Kafka ou o ID da instância de serviço.
  

Para obter mais informações sobre os terminais em serviço, consulte a [documentação de terminal em serviço do {{site.data.keyword.Bluemix_notm}}](/docs/resources?topic=resources-service-endpoints#about){:new_window}.


## Depois de alternar para um terminal privado
{: #after_endpoint}

Quando você tiver alternado para um terminal privado, os terminais externos ou públicos não estarão mais disponíveis para você.


### Acessando o console do IBM {{site.data.keyword.messagehub}}

Quando um cluster tiver terminais privados ativados, a URL do administrador que você usa para acessar o console do {{site.data.keyword.messagehub}} mudará.

O console do {{site.data.keyword.messagehub}} é acessível somente por meio de uma URL de administrador privada. Para descobrir seus terminais privados, incluindo a URL de administrador privada, é possível criar uma nova credencial de serviço.

<!--
1. On the service details page, click **Manage endpoints**. You can see the external endpoint assigned to your service instance.
2. Click  **Add internal endpoint**. An internal endpoint is assigned to your service instance.
3. **Optional.** Use the endpoint toggle to enable or disable endpoints as needed.
-->

