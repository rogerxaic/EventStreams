---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-16"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Leadership de partition
{: #partition_leadership }

Chaque partition dispose dans le cluster d'un serveur qui fait office de responsable (leader) de la partition, les autres serveurs faisant office de suiveurs. Toutes les demandes de production et de consommation de la partition sont gérées par le responsable. Les suiveurs répliquent les données de la partition depuis le serveur responsable avec pour objectif de suivre le rythme du responsable. Lorsqu'un suiveur suit le rythme du responsable d'une partition, les répliques de ce suiveur sont totalement synchronisées. 

Un message envoyé au responsable de la partition n'est pas immédiatement disponible pour les consommateurs. Le responsable ajoute l'enregistrement du message à la partition, en lui affectant le numéro de position suivant de cette partition. Une fois que tous les suiveurs des répliques totalement synchronisées ont répliqué l'enregistrement et confirmé qu'ils l'ont écrit dans leurs répliques, l'enregistrement est *validé*. Le message est disponible pour les consommateurs.

Si le responsable d'une partition échoue, l'un des suiveurs ayant une réplique totalement synchronisée prend automatiquement le rôle de responsable de la partition. En fait, chaque serveur est le responsable de quelques partitions et le suiveur des autres. Le leadership des partitions est un processus dynamique qui change à mesure que les serveurs vont et viennent.

Les applications n'ont pas à effectuer d'actions spécifique pour gérer les modifications de leadership d'une partition. La bibliothèque client Kafka se reconnecte automatiquement au nouveau responsable, mais vous constaterez une augmentation du temps d'attente tandis que le cluster se régule.
