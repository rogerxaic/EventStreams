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

<!-- 14/11/18: info moved to eventstreams099.md, moved because of doc app changes -->
# MQ Light-Ruby-Client verwenden
{: #mql_how}

** Die MQ Light-API ist nur als Bestandteil des Plans "Standard" verfügbar.**
<br/>

Um die API zu verwenden, fügen Sie einen Verweis auf die neueste verfügbare {{site.data.keyword.mql}}-Client-API für Ruby wie folgt hinzu:
{: shortdesc}

Fügen Sie den folgenden Verweis in der Datei <code>Gem</code> hinzu:

```
gem 'mqlight', '~> 1.0'
```
{: codeblock}

Fügen Sie außerdem die folgende Anweisung 'require' in Ihrer Quellendatei hinzu:

```
require ‘mqlight’
```
{: codeblock}

<!-- Comment from Andrew
Instructions for getting started, with links for more info
Simple send source and receive source in-line

-->


