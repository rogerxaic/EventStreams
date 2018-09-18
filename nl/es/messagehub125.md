---

copyright:
  years: 2015, 2018
lastupdated: "2018-07-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Notificación de un problema al equipo de {{site.data.keyword.messagehub}} - plan Empresa
{: #report_problem}

Si tiene problemas con {{site.data.keyword.messagehub}}, compruebe primero la página de estado de [{{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/status){:new_window}.

Si desea ayuda del equipo de {{site.data.keyword.messagehub}}, recopile tanta de la siguiente información como sea posible:
{:shortdesc}

1. ¿Qué ID de CRN del servicio {{site.data.keyword.messagehub}} está utilizando?  Para especificar este ID, puede pegar el URL completo de la consola de {{site.data.keyword.Bluemix_notm}} después de pulsar en el servicio o puede copiar la salida del mandato de CLI:<br/>
   <code>ibmcloud resource service-instance NAME</code>
1. ¿Cuándo se produjo el primer problema (específicamente hora, fecha y huso horario)?
   ¿Cuánto tiempo estuvo ejecutando la app antes del problema?
1. ¿El problema sigue ocurriendo? ¿Lo puede replicar?
1. ¿Qué cliente Kafka utiliza la aplicación? ¿Cuáles son los detalles de la versión?
1. ¿Cuáles son los detalles de la configuración de cliente?
1. ¿Hay fragmentos del registro de aplicaciones con el mismo problema?
1. ¿Cuál es el problema que ve? ¿Qué temas, ID de cliente, ID de grupo e ID de transacción se han visto afectados?
1. ¿Qué impacto tiene el problema en el servicio?

Puede proporcionar la información que ha obtenido a IBM en una incidencia de soporte [mediante el envío de una solicitud de soporte ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/get-support/howtogetsupport.html#open-ticket).










