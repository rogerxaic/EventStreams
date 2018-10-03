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

# MQ Light-Python-Client verwenden
{: #mql_python}

** Die MQ Light-API ist nur als Bestandteil des Plans "Standard" verfügbar.**
<br/>

Um die API zu verwenden, fügen Sie einen Verweis auf die neueste verfügbare {{site.data.keyword.mql}}-Client-API für Python wie folgt hinzu:

Fügen Sie den folgenden Verweis in Ihrer Datei <code>requirements.txt</code>
hinzu:

```
git+git://github.com/mqlight/python-mqlight.git@readthedocs
```
{:codeblock}

<br>
Fügen Sie außerdem die folgende Importanweisung in Ihrer Quellendatei hinzu:

```
import mqlight
```
{:codeblock}

<!-- Comment from Andrew
Instructions for getting started, with links for more info
Simple send source and receive source in-line

-->

