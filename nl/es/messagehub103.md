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

# Uso del cliente MQ Light Ruby cliente
{: #mql_how}

**La API MQ Light solo está disponible como parte del plan Estándar.**
<br/>

Para utilizar la API, añada una referencia a la última API de cliente {{site.data.keyword.mql}} disponible para Ruby de la siguiente manera:

Añada la siguiente referencia a <code>Gemfile</code>:

```
gem 'mqlight', '~> 1.0'
```
{: codeblock}

A continuación, añada la siguiente sentencia necesaria al archivo de origen:

```
require ‘mqlight’
```
{: codeblock}

<!-- Comment from Andrew
Instructions for getting started, with links for more info
Simple send source and receive source in-line

-->


