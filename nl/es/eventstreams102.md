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

# Uso del cliente MQ Light Node.js 
{: #mql_node}

**La API MQ Light solo está disponible como parte del plan Estándar.**
<br/>

Para utilizar la API, añada una referencia a la última API de cliente {{site.data.keyword.mql}} disponible para Node.js de la siguiente manera:

Añada la siguiente referencia a la sección dependencia del archivo <code>package.json</code>:

<pre class="pre"><code>"mqlight" : "1.0.x"</code></pre>
{: codeblock}

A continuación, añada la siguiente sentencia necesaria al archivo de origen:

<pre class="pre"><code>var mqlight = require(‘mqlight’);</code></pre>
{: codeblock}

<!-- Comment from Andrew
Instructions for getting started, with links for more info
Simple send source and receive source in-line

-->


