---

copyright:
  years: 2015, 2019
lastupdated: "2018-05-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

<!-- 15/11/18: info moved to eventstreams075.md, moved because of doc app changes -->
# Exemples de l'API MQ Light
{: #mql_samples}

**L'API MQ Light est disponible uniquement dans le cadre du plan Standard.**
<br/>

Si vous ne disposez pas déjà d'une application, essayez l'API {{site.data.keyword.mql}} avec l'un des exemples.
{:shortdesc}

Le modèle d'application se compose en fait de deux applications simples : une application Web frontale qui envoie des messages à une application de back end, et une
application de back end qui traite les messages, met les lettres en majuscules,
puis renvoie les messages à l'application frontale. Cet exemple illustre comment vous pouvez faire communiquer vos applications entre elles grâce à l'API {{site.data.keyword.mql}}. Vous
pouvez également utiliser l'API {{site.data.keyword.mql}} pour décharger les
agents, fonctionnalité essentielle à la génération d'applications évolutives, réparties
et à couplage lâche.

L'exemple de code est disponible dans le [projet GitHub event-streams-samples![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-messaging/event-streams-samples/tree/master/mqlight){:new_window}.
