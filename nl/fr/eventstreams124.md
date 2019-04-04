---

copyright:
  years: 2015, 2019
lastupdated: "2018-07-04"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Gestion de l'accès à vos ressources {{site.data.keyword.messagehub}} (plan Enterprise)
{: #security }

Vous pouvez sécuriser vos ressources {{site.data.keyword.messagehub}} de manière très affinée afin de gérer l'accès que vous voulez accorder à chaque ressource pour chaque utilisateur.
{: shortdesc}

## Que puis-je sécuriser ?

Dans {{site.data.keyword.messagehub}}, vous pouvez sécuriser l'accès aux ressources suivantes :
* Cluster (cluster) : vous pouvez contrôler quelles applications et quels utilisateurs peuvent se connecter au service
* Sujets (sujet) : vous pouvez contrôler la possibilité des utilisateurs et des applications de créer, supprimer, lire et écrire dans un sujet 
* Groupes de consommateurs (groupe) : vous pouvez contrôler la possibilité d'une application de rejoindre un groupe de consommateurs 
* Transactions de producteur (txnid) : vous pouvez contrôler la possibilité d'utiliser la fonctionnalité de producteur transactionnel de Kafka (c'est-à-dire, écritures atomiques uniques dans plusieurs partitions)

Les niveaux d'accès (ou rôles) que vous pouvez affecter à un utilisateur sur chaque ressource sont les suivants :

| Rôle d'accès | Description des actions | Exemples d'actions |
|:-----------------|:-----------------|:-----------------|
|  Lecteur | Action en lecture seule dans {{site.data.keyword.messagehub}}, telles que l'affichage de ressources | Autoriser une application à se connecter à un cluster en affectant un accès en lecture au type de ressource cluster |
| Auteur | Les auteurs disposent de droits supérieurs à ceux des lecteurs, y compris de création et d'édition des ressources {{site.data.keyword.messagehub}}. | Autoriser une application à produire dans des sujets en affectant un accès en écriture à des types de nom de sujet et des ressources de type sujet et de nom de sujet|
| Responsable | Les responsables disposent de droits supérieurs à ceux des auteurs leurs permettant d'accomplir des actions privilégiées. Ils autorisent en plus la création et l'édition de ressources {{site.data.keyword.messagehub}}. | Accorder un accès complet à toutes les ressources affectant l'accès Responsable à l'instance {{site.data.keyword.messagehub}}|
{: caption="Tableau 1. Exemple d'actions et de rôles utilisateur {{site.data.keyword.messagehub}}" caption-side="top"}

<!-- comment from Charlie and my reply 
CM: need to confirm if hierarchical e.g. write includes read - and doc. 
KR: I think they do inherit the lower level access https://console.bluemix.net/docs/iam/users_roles.html#iamusermanrol 
-->


## Comment affecter un accès

Les règles Cloud Identity and Access Management (IAM) sont jointes aux ressources à contrôler. Chaque règle définit le niveau d'accès qu'un utilisateur particulier doit avoir et à quelle ressource ou un ensemble de ressources. Une règle est constituée des informations suivantes : 
* Le type de service auquel s'applique la règle. Par exemple, {{site.data.keyword.messagehub}}. Vous pouvez définir la portée d'une règle de manière à y inclure tous les types de service. 
* L'instance de service à sécuriser. Vous pouvez définir la portée d'une règle de manière à y inclure toutes les instances d'un type de service. 
* Le type de ressource à sécuriser. Les valeurs admises sont <code>cluster</code>, <code>sujet</code>, <code>groupe</code> et <code>txnid</code>. La spécification du type est facultative. Si vous n'indiquez aucun type, la règle s'applique alors à toutes les ressources de l'instance de service. 
* La ressource à sécuriser. Spécifiez pour les ressources de type <code>sujet</code>, <code>groupe</code> et <code>txnid</code>. Si vous n'indiquez de ressource, la règle s'applique alors à toutes les ressources du type spécifié dans l'instance de service. 
* Le rôle affecté à l'utilisateur. Par exemple, Lecteur, Auteur ou Responsable. 

## Quels sont les paramètres de sécurité par défaut ?

Par défaut, à la mise à disposition de {{site.data.keyword.messagehub}}, le rôle de responsable sur toutes les ressources de l'instance est accordé à l'utilisateur qui a procédé à la mise à disposition. En outre, tout utilisateur doté du rôle de responsable sur TOUS les services ou TOUTES les instances de service de {{site.data.keyword.messagehub}} dans le même compte dispose d'un accès complet. 

Vous pouvez ensuite appliquer les règles supplémentaires afin d'étendre l'accès à d'autres utilisateurs. Vous pouvez définir l'étendue d'une règle pour qu'elle s'applique à {{site.data.keyword.messagehub}} dans son intégralité ou à des ressources individuelles au sein de {{site.data.keyword.messagehub}}. Pour plus d'informations, voir [Scénarios courants](#security_scenarios).

Seuls les utilisateurs ayant un rôle d'administration pour un compte peuvent affecter des règles aux utilisateurs. Affectez des règles à l'aide du tableau de bord IBM Cloud ou à l'aide des commandes **ibmcloud**. 
<!--
For example steps for {{site.data.keyword.messagehub}}, see [Examples](#security_examples).
-->


## Scénarios courants
{: #security_scenarios }

Ce tableau résume quelques scénarios {{site.data.keyword.messagehub}} courants et les acès que vous devez affecter :

| Action | Rôle de lecteur | Rôle d'auteur | Rôle de responsable |
|---------|----------------|
| Accorder l'accès complet à toutes les ressources|Non applicable   |Non applicable  |Instance de service : <var class="keyword varname">votre_instance_service</var>|
| Autoriser une application ou un utilisateur à créer ou supprimer un sujet |Type de ressource : <code>cluster</code>   |Non applicable  |Type de ressource : sujet <br/><br/>Facultatif : ID ressource : <var class="keyword varname">nom_sujet</var> |
| Lister des groupes, des sujets et des positions <br/> Décrire des configurations de groupe, de sujet et de courtier | Type de ressource : <code>cluster</code>      |Non applicable  |Non applicable      |
| Autoriser une application à se connecter au cluster  |Type de ressource : <code>cluster</code>| Non applicable     |Non applicable      |
| Autoriser une application à produire pour un sujet  |Type de ressource : <code>cluster</code>|Type de ressource : <code>sujet</code> |Non applicable     |
| Autoriser une application à produire pour un sujet spécifique  |Type de ressource : <code>cluster</code>|Type de ressource : <code>sujet</code><br/>ID ressource : <var class="keyword varname">nom_sujet</var>      |Non applicable     |
| Autoriser une application à se connecter et à consommer dans tout sujet (aucun groupe de consommateurs)  |Type de ressource : <code>cluster</code> <br/>Type de ressource : <code>sujet</code> |Type de ressource : <code>sujet</code>|Non applicable     |
| Autoriser une application à se connecter et à consommer dans un sujet spécifique (aucun groupe de consommateurs)  | Type de ressource : <code>cluster</code> <br/>Type de ressource : <code>sujet</code><br/>ID ressource : <var class="keyword varname">nom_sujet</var> |Non applicable     |Non applicable     |
| Autoriser une application de consommer un sujet (groupe de consommateurs)  |Type de ressource : <code>cluster</code> <br/>Type de ressource : <code>sujet</code><br/> Type de ressource : <code>groupe</code> |Non applicable      |Non applicable     |
| Autoriser une application à produire ponctuellement pour un sujet  |Type de ressource : <code>cluster</code> <br/> Type de ressource : <code>groupe</code>|Type de ressource : <code>sujet</code> <br/>ID ressource : <var class="keyword varname">nom_sujet</var> <br/>Type de ressource : <code>txnid</code> |Non applicable     |
| Supprimer un groupe de consommateurs |Type de ressource : <code>cluster</code> |Non applicable  |Type de ressource : <code>groupe</code> <br/>ID ressource : <var class="keyword varname">ID_groupe</var>      |
| Utiliser Streams |Type de ressource : <code>cluster</code></br>Type de ressource : <code>groupe</code>| Non applicable  |Type de ressource : <code>sujet</code>    |

Pour plus d'informations sur IAM, voir
[IBM Cloud Identity and Access Management](/docs/iam?topic=iam-iamoverview#iamoverview).

Pour un exemple de définition de règles, voir
[ID de service et clés d'API IAM d'IBM Cloud![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/blogs/bluemix/2017/10/introducing-ibm-cloud-iam-service-ids-api-keys/){:new_window}.


## Connexion à {{site.data.keyword.messagehub}}
{: #connect_message_enterprise }

Pour savoir comment lier une application Cloud Foundry ou obtenir des données d'identification de clé de sécurité pour une application externe, voir [Connexion à {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting).

<!-- 28/06/18 - Karen: draft info only

## Examples
{: #security_examples }

I want to give a user access to create or delete a topic:

1. From the IBM Cloud dashboard, go to the **Manage** tab &gt; **Security** &gt; **Identity and Access**, and then select **Users**.
2. Click **Invite users**.
3. Specify the email address of the user that you want to invite.
4. In the **Access** section, expand the **Services** option.
5. Choose to assign access to a **Resource**.
6. In the **Services** section, select **{{site.data.keyword.messagehub}}**
7. In the **Region** section, make your selection.
8. In the **Service instance** section, locate your instance and select it.
9. In the **Resource type** section, enter **cluster**.
10. In the **Select roles** section, check the **Reader** box.
11. In the **Resource type** section, enter **topic**.
12. In the **Select roles** section, check the **Manager** box.
13. Click **Invite users**.

-->















