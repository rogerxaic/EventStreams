---

copyright:
  years: 2015, 2018
lastupdated: "2017-09-26"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Qu'est-ce que Message Hub ?

{{site.data.keyword.messagehub}} implémente une messagerie de publication/abonnement utilisant des sujets. A
la différence d'autres systèmes de messagerie, {{site.data.keyword.messagehub}} fournit des sujets, mais pas de files d'attente. {{site.data.keyword.messagehub}} offre également des fonctionnalités supplémentaires, telles que des ponts vers d'autres systèmes.

{{site.data.keyword.messagehub}} repose sur le projet open-source Apache Kafka, un réseau principal de messagerie haute performance et hautement évolutif, ayant fait ses preuves dans de nombreux environnements de production. Pour plus d'informations, voir [{{site.data.keyword.messagehub}} et Apache Kafka](/docs/services/MessageHub/messagehub073.html).
En général, les outils Apache Kafka fonctionnent directement avec {{site.data.keyword.messagehub}}, mais vous devez cependant fournir une configuration supplémentaire car les connexions à {{site.data.keyword.messagehub}} sont toujours authentifiées à l'aide de données d'identification.

{{site.data.keyword.messagehub}} dispose de trois API : l'API Kafka, l'API REST Kafka et l'API {{site.data.keyword.mql}}.
Dans la plupart des cas, l'API Kafka est la meilleure solution. Pour plus d'informations sur la création d'applications de messagerie avec ces API, voir [Utilisation d'un client Kafka](/docs/services/MessageHub/messagehub050.html), [Utilisation de l'API REST Kafka](/docs/services/MessageHub/messagehub025.html) et [Utilisation d'un client d'API MQ Light](/docs/services/MessageHub/messagehub075.html).

{{site.data.keyword.messagehub}} prend également en charge des ponts vers
une sélection d'autres systèmes. Un pont est un lien unidirectionnel vers un autre système. Il
peut prendre des messages provenant d'un autre système et les publier sur un sujet,
ou consommer les messages d'un sujet et les envoyer à l'autre système. Vous pouvez ainsi
utiliser {{site.data.keyword.messagehub}} et l'intégrer à d'autres systèmes sans écrire de code. Pour
plus d'informations, voir [Liaison à d'autres services à l'aide de ponts](/docs/services/MessageHub/messagehub088.html).
