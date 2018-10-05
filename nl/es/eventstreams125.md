---

copyright:
  years: 2015, 2018
lastupdated: "2018-09-11"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Notificación de un problema al equipo de {{site.data.keyword.messagehub}} - plan Empresa
{: #report_problem}

Si tiene problemas con {{site.data.keyword.messagehub}}, compruebe primero la página de estado de [{{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/status){:new_window}.

Si desea ayuda del equipo de {{site.data.keyword.messagehub}}, recopile toda la información siguiente. Cuanta más información proporcione de antemano, más eficientemente podrá ayudar el equipo con el problema:
{:shortdesc}

1. ¿Qué ID de CRN del servicio {{site.data.keyword.messagehub}} está utilizando?  Puede especificar este ID, puede pegar el URL completo de la consola de {{site.data.keyword.Bluemix_notm}} después de pulsar en el
   servicio o puede pegar la salida del mandato de CLI siguiente:<br/>
   <pre class="pre">
   ibmcloud resource service-instance NAME
   </pre>
	{: codeblock}
2. ¿Cuándo se produjo el primer problema (específicamente hora, fecha y huso horario)?
   ¿Cuánto tiempo estuvo ejecutando la app antes del problema?
3. ¿El problema sigue ocurriendo? ¿Lo puede replicar?
4. ¿Qué cliente Kafka utiliza la aplicación? ¿Cuáles son los detalles de la versión?
5. ¿Cuáles son los detalles de la configuración de cliente?
6. ¿Hay fragmentos del registro de aplicaciones con el mismo problema?
7. ¿Cuál es el problema que ve? ¿Qué temas, ID de cliente, ID de grupo e ID de transacción se han visto afectados?
8. ¿Qué impacto tiene el problema en el servicio?

Puede proporcionar la información que ha obtenido a IBM en una incidencia de soporte [mediante el envío de una solicitud de soporte ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/get-support/howtogetsupport.html#open-ticket).










