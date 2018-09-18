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

# Usando o cliente Node.js do MQ Light 
{: #mql_node}

** A API do MQ Light está disponível como parte somente do plano Standard.**
<br/>

Para usar a API, inclua uma referência à API do cliente {{site.data.keyword.mql}} mais recente
disponível para Node.js como segue:

Inclua a referência a seguir na seção de dependência de seu arquivo <code>package.json</code>:

<pre class="pre"><code>"mqlight" : "1.0.x"</code></pre>
{: codeblock}

E inclua a instrução de solicitação a seguir em seu arquivo de origem:

<pre class="pre"><code>var mqlight = require(‘mqlight’);</code></pre>
{: codeblock}

<!-- Comment from Andrew
Instructions for getting started, with links for more info
Simple send source and receive source in-line

-->


