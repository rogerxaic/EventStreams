---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-19"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Utilisation de {{site.data.keyword.iamshort}} (IAM) avec le programme Alpha
{: #alpha_iam }

Les droits {{site.data.keyword.messagehub}} sont configurés à l'aide de règles IAM. Une règle IAM est constituée des informations suivantes :

* un ID de service
* une ressource {{site.data.keyword.Bluemix_short}} définie par un nom de service, une instance de service, une région, un type de ressource IAM et une ressource IAM
* un rôle (Lecteur, Auteur ou Responsable)

Pour plus d'informations sur IAM, voir
[IBM Cloud Identity and Access Management](/docs/iam/index.html#iamoverview).

Pour un exemple de définition de règles, voir
[Présentation des ID de service et des clés d'API du service IAM d'IBM Cloud![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/blogs/bluemix/2017/10/introducing-ibm-cloud-iam-service-ids-api-keys/){:new_window}.

## Ressources IAM de {{site.data.keyword.messagehub}}
{: iam_resources}

{{site.data.keyword.messagehub}} expose quatre types de ressource IAM :

* cluster
* sujet
* groupe
* txnid

Vous pouvez identifier des ressources uniques pour les types sujet, groupe et txnid en définissant la ressource IAM. Le type de ressource cluster est un singleton, de sorte qu'il n'a pas besoin d'être identifié de manière unique.

## Scénarios courants
{: iam_scenarios}

Voici quelques scénarios {{site.data.keyword.messagehub}} courants et les droits que vous devez affecter :

Producteur de certains sujets (identique pour idempotent) :
* Rôle de lecteur sur les ressources de type cluster
* Rôle d'auteur sur chaque ressource de type sujet et la ressource de nom de sujet

Producteur transactionnel :
* Identique au producteur
* Rôle d'auteur sur les ressources de type txnid et la ressource d'ID transaction
* Rôle de lecteur sur les ressources de type groupe et la ressource d'ID groupe

Consommateur (sans groupe) :
* Rôle de lecteur sur les ressources de type cluster
* Rôle de lecteur sur chaque ressource de type sujet et la ressource de nom de sujet

Consommateur avec groupe :
* Identique à consommateur (sans groupe)
* Rôle de lecteur sur les ressources de type groupe et la ressource d'ID groupe

Création ou suppression de sujet :
* Rôle de lecteur sur les ressources de type cluster
* Rôle de responsable sur chaque ressource de type sujet et la ressource de nom de sujet

Suppression de groupe de consommateurs :
* Rôle de lecteur sur les ressources de type cluster
* Rôle de responsable sur les ressources de type groupe et la ressource d'ID groupe

Liste de groupes, sujets et positions, et description de groupe, sujet et configurations de courtier
* Rôle de lecteur sur les ressources de type cluster














