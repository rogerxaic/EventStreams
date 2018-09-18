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


#Monitorando e criando logs
{: #monitoring}


O {{site.data.keyword.messagehub}} coleta métricas e eventos automaticamente para que seja
possível monitorar o uso do {{site.data.keyword.messagehub}}.
{:shortdesc}

**Nota:** métricas e eventos não estão disponíveis no
{{site.data.keyword.messagehub}} em ambientes do {{site.data.keyword.Bluemix_short}} Dedicated.

É possível monitorar as informações de log a seguir:

<dl>
<dt>Métrica de tópico</dt>
<dd>Enviamos o número de bytes de entrada e saída para cada um de seus tópicos (um ponto de
                            verificação é obtido a cada 15 minutos). É possível acessar essas métricas
clicando no botão **Grafana** no painel do {{site.data.keyword.messagehub}}
no console do {{site.data.keyword.Bluemix_notm}}.
</dd>
<dt>Eventos do tópico</dt>
<dd>Também enviamos eventos de push toda vez que você criar ou excluir um tópico. É possível acessar esses
eventos clicando no botão **Kibana** no painel do {{site.data.keyword.messagehub}}
no console do {{site.data.keyword.Bluemix_notm}}.</dd>
</dl>


Recomendamos não editar os painéis do {{site.data.keyword.messagehub}}, já que
o {{site.data.keyword.messagehub}} faz atualizações que podem sobrescrever suas mudanças. No entanto, é possível
                incluir essas métricas e eventos em seus próprios painéis.
