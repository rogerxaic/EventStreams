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

# Utilisation du client Python MQ Light
{: #mql_python}

**L'API MQ Light est disponible uniquement dans le cadre du plan Standard.**
<br/>

Pour utiliser l'API, ajoutez une référence à l'API du client {{site.data.keyword.mql}} pour Python la plus récente comme suit :

Ajoutez la référence suivante dans le fichier <code>requirements.txt</code> :

```
git+git://github.com/mqlight/python-mqlight.git@readthedocs
```
{:codeblock}

<br>
Puis, ajoutez l'instruction import suivante dans le fichier source :

```
import mqlight
```
{:codeblock}

<!-- Comment from Andrew
Instructions for getting started, with links for more info
Simple send source and receive source in-line

-->

