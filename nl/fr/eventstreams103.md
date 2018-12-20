---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

<!-- 14/11/18: info moved to eventstreams099.md, moved because of doc app changes -->
# Utilisation du client Ruby MQ Light
{: #mql_how}

**L'API MQ Light est disponible uniquement dans le cadre du plan Standard.**
<br/>

Pour utiliser l'API, ajoutez une référence à l'API du client {{site.data.keyword.mql}} pour Ruby la plus récente comme suit :

Ajoutez la référence suivante au fichier <code>Gemfile</code> :

```
gem 'mqlight', '~> 1.0'
```
{: codeblock}

Ajoutez l'instruction require suivante à votre fichier source :

```
require ‘mqlight’
```
{: codeblock}

<!-- Comment from Andrew
Instructions for getting started, with links for more info
Simple send source and receive source in-line

-->


