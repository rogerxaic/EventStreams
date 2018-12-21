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

# Usando os clientes do MQ Light
{: #mql_clients}

** A API do MQ Light está disponível como parte somente do plano Standard.**
<br/>
## Usando o cliente MQ Light Java
{: #mql_java}

Para usar a API, inclua uma referência à API do cliente {{site.data.keyword.mql}} mais recente
disponível para Java como segue:

Inclua a referência a seguir em seu arquivo <code>Maven pom</code>:

```
<dependency>
    <groupId>com.ibm.mqlight</groupId>
    <artifactId>mqlight-api</artifactId>
    <version>LATEST</version>
</dependency>
```
{:codeblock}

<!-- 12/11/18: info was in eventstreams102.md, moved because of doc app changes -->

## Usando o cliente Node.js do MQ Light 
{: #mql_node}


Para usar a API, inclua uma referência à API do cliente {{site.data.keyword.mql}} mais recente
disponível para Node.js como segue:

Inclua a referência a seguir na seção de dependência de seu arquivo <code>package.json</code>:

<pre class="pre"><code>"mqlight" : "1.0.x"</code></pre>
{: codeblock}

E inclua a instrução de solicitação a seguir em seu arquivo de origem:

<pre class="pre"><code>var mqlight = require(&lsquo;mqlight&rsquo;);</code></pre>
{: codeblock}

<!-- 14/11/18: info was in eventstreams103.md, moved because of doc app changes -->

## Usando o cliente Ruby do MQ Light
{: #mql_ruby}


Para usar a API, inclua uma referência à API do cliente {{site.data.keyword.mql}} mais recente
disponível para Ruby como segue:

Inclua a referência a seguir no <code>Gemfile</code>:

```
gem 'mqlight', '~> 1.0'
```
{: codeblock}

E inclua a instrução de solicitação a seguir em seu arquivo de origem:

<pre class="pre"><code>require &lsquo;mqlight&rsquo;</code></pre>
{: codeblock}

<!-- 14/11/18: info was in eventstreams101.md, moved because of doc app changes -->

## Usando o cliente Python do MQ Light
{: #mql_python}

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
