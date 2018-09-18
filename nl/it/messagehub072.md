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


#Monitoraggio e registrazione
{: #monitoring}


{{site.data.keyword.messagehub}} raccoglie automaticamente le metriche e gli eventi in modo che tu possa
monitorare l'utilizzo che fai di {{site.data.keyword.messagehub}}.
{:shortdesc}

**Nota:** le metriche e gli eventi non sono disponibili in {{site.data.keyword.messagehub}} negli
ambienti di {{site.data.keyword.Bluemix_short}} dedicato.

Puoi monitorare le seguenti informazioni di log:

<dl>
<dt>Metriche argomenti</dt>
<dd>Inviamo il numero di byte in entrata e in uscita per ciascuno dei tuoi argomenti (un punto di controllo
                          viene eseguito ogni 15 minuti). Puoi accedere a tali metriche facendo clic sul
pulsante **Grafana** nel dashboard {{site.data.keyword.messagehub}} della console {{site.data.keyword.Bluemix_notm}}.
</dd>
<dt>Eventi argomenti</dt>
<dd>Eseguiamo anche il push di eventi ogni volta che crei o elimini un argomento. Puoi accedere a
tali eventi facendo clic sul pulsante **Kibana** nel dashboard {{site.data.keyword.messagehub}} della console {{site.data.keyword.Bluemix_notm}}.</dd>
</dl>


Si consiglia di non modificare i dashboard {{site.data.keyword.messagehub}}
in quanto {{site.data.keyword.messagehub}} effettua degli aggiornamenti che potrebbero sovrascrivere le tue
modifiche. Puoi tuttavia includere tali
                metriche ed eventi nei tuoi dashboard.
