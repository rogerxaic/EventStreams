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
# Usando o cliente Python do MQ Light
{: #mql_python}

** A API do MQ Light está disponível como parte somente do plano Standard.**
<br/>

Para usar a API, inclua uma referência à API do cliente {{site.data.keyword.mql}} mais recente
disponível para Python como segue:

Inclua a referência a seguir em seu arquivo <code>requirements.txt</code>:

```
git+git://github.com/mqlight/python-mqlight.git@readthedocs
```
{:codeblock}

<br>
E inclua a instrução de importação a seguir em seu arquivo de origem:

```
import mqlight
```
{:codeblock}

<!-- Comment from Andrew
Instructions for getting started, with links for more info
Simple send source and receive source in-line

-->

