---

copyright:
  years: 2015, 2019
lastupdated: "2018-10-29"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Monitoraggio e registrazione (piano Standard)
{: #monitoring}

{{site.data.keyword.messagehub}} nel piano Standard raccoglie automaticamente le metriche e gli eventi in modo che tu possa
monitorare il tuo utilizzo di {{site.data.keyword.messagehub}}.
{:shortdesc}

**Nota:** le metriche e gli eventi sono disponibili come parte del piano Standard di {{site.data.keyword.messagehub}} solo in Dallas (us-south), Londra (eu-gb) e Sydney (au-syd). 


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


