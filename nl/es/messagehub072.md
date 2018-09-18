---

copyright:
  years: 2015, 2018
lastupdated: "2017-03-14"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


#Supervisión y registro
{: #monitoring}


{{site.data.keyword.messagehub}} recopila automáticamente métricas y eventos para que el usuario pueda supervisar el uso de {{site.data.keyword.messagehub}}.
{:shortdesc}

**Nota:** las métricas y los eventos no están disponibles en {{site.data.keyword.messagehub}} en entornos de {{site.data.keyword.Bluemix_short}} Dedicated.

Puede supervisar la siguiente información de registro:

<dl>
<dt>Métrica de temas</dt>
<dd>Enviamos el número de bytes de entrada y salida para cada uno de sus temas (cada 15 minutos hay un punto de comprobación). Puede acceder a estas métricas pulsando el botón **Grafana** en el panel de control de {{site.data.keyword.messagehub}} de la consola de {{site.data.keyword.Bluemix_notm}}.
</dd>
<dt>Sucesos de temas</dt>
<dd>También enviamos sucesos cada vez que crea o suprime un tema. Puede acceder a estos eventos pulsando el botón **Kibana** en el panel de control de {{site.data.keyword.messagehub}} de la consola de {{site.data.keyword.Bluemix_notm}}.</dd>
</dl>


Le recomendamos que no edite los paneles de control {{site.data.keyword.messagehub}} porque {{site.data.keyword.messagehub}} aplica actualizaciones que podrían sobrescribir sus cambios. Sin embargo, puede incluir dichas métricas y sucesos en sus propios paneles de control.
