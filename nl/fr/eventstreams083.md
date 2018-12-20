---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-30"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

<!-- 15/11/18: info moved to eventstreams075.md, moved because of doc app changes -->
# Limites maximales
{: #max_limits}

**L'API MQ Light est disponible uniquement dans le cadre du plan Standard.**
<br/>

L'API {{site.data.keyword.mql}} est soumise aux limites suivantes :
{:shortdesc}

* La quantité maximale de données pouvant être stockées est cohérente avec une seule partition Kafka de votre plan de paiement (généralement 1 Go). Si cette limite de données est dépassée, les messages les plus anciens de la partition sont supprimés au fur et à mesure de l'envoi de nouveaux messages.
* La durée de stockage maximale d'un message est cohérente avec une seule partition Kafka de votre plan de paiement (généralement 24 heures). Vous ne pouvez pas extraire de messages plus anciens.
* La taille maximale d'un message (à l'exclusion des en-têtes) est de 1 Mo.
* Le nombre maximal de clients pouvant être connectés simultanément est de 25.
* Le nombre maximal de destinations pouvant être actives simultanément est de 25. Une destination active se définit comme suit :
  - Une destination où la durée de vie est supérieure à 0 (TimeToLive > 0) avec ou sans client connecté.
  - Une destination où la durée de vie est nulle (TimeToLive = 0) (valeur par défaut) avec un client connecté. 
  <p>Une destination partagée entre des clients compte comme une seule destination.</p>
