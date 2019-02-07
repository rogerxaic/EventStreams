---

copyright:
  years: 2015, 2019
lastupdated: "2018-10-29"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


#Supervisión y registro (plan Estándar)
{: #monitoring}

{{site.data.keyword.messagehub}} en el plan Estándar recopila automáticamente métricas y sucesos para que el usuario
pueda supervisar el uso de {{site.data.keyword.messagehub}}.
{:shortdesc}

**Nota:** Las métricas y los sucesos solo están disponibles como parte del plan Estándar de {{site.data.keyword.messagehub}} en Dallas (us-south), Londres (eu-gb) y Sídney (au-syd). 


Puede supervisar la siguiente información de registro:

<dl>
<dt>Métrica de temas</dt>
<dd>Enviamos el número de bytes de entrada y salida para cada uno de sus temas (cada 15 minutos hay un punto de comprobación). Puede acceder a estas métricas pulsando el botón **Grafana** en el panel de control de {{site.data.keyword.messagehub}} de la consola de {{site.data.keyword.Bluemix_notm}}.
</dd>
<dt>Sucesos de temas</dt>
<dd>También enviamos sucesos cada vez que crea o suprime un tema. Puede acceder a estos eventos pulsando el botón **Kibana** en el panel de control de {{site.data.keyword.messagehub}} de la consola de {{site.data.keyword.Bluemix_notm}}.</dd>
</dl>


Le recomendamos que no edite los paneles de control {{site.data.keyword.messagehub}} porque {{site.data.keyword.messagehub}} aplica actualizaciones que podrían sobrescribir sus cambios. Sin embargo, puede incluir dichas métricas y sucesos en sus propios paneles de control.


