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

# Uso del cliente MQ Light Python
{: #mql_python}

**La API MQ Light solo está disponible como parte del plan Estándar.**
<br/>

Para utilizar la API, añada una referencia a la última API de cliente {{site.data.keyword.mql}} disponible para Python de la siguiente manera:

Añada la siguiente referencia al archivo <code>requirements.txt</code>:

```
git+git://github.com/mqlight/python-mqlight.git@readthedocs
```
{:codeblock}

<br>
A continuación, añada la siguiente sentencia al archivo de origen:

```
import mqlight
```
{:codeblock}

<!-- Comment from Andrew
Instructions for getting started, with links for more info
Simple send source and receive source in-line

-->

