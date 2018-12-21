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

# Utilizzo dei client MQ Light
{: #mql_clients}

** L'API MQ Light è disponibile solo come parte del piano Standard.**
<br/>
## Utilizzo del client Java MQ Light
{: #mql_java}

Per utilizzare l'API, aggiungi un riferimento all'ultima API client {{site.data.keyword.mql}} disponibile per Java nel seguente modo:

Aggiungi il seguente riferimento al file <code>Maven pom</code>:

```
<dependency>
    <groupId>com.ibm.mqlight</groupId>
    <artifactId>mqlight-api</artifactId>
    <version>LATEST</version>
</dependency>
```
{:codeblock}

<!-- 12/11/18: info was in eventstreams102.md, moved because of doc app changes -->

## Utilizzo del client Node.js MQ Light 
{: #mql_node}


Per utilizzare l'API, aggiungi un riferimento all'ultima API client {{site.data.keyword.mql}} disponibile per Node.js nel seguente modo:

Aggiungi il seguente riferimento alla sezione delle dipendenze del tuo file <code>package.json</code>:

<pre class="pre"><code>"mqlight" : "1.0.x"</code></pre>
{: codeblock}

E aggiungi la seguente istruzione require al tuo file di
origine:

<pre class="pre"><code>var mqlight = require(&lsquo;mqlight&rsquo;);</code></pre>
{: codeblock}

<!-- 14/11/18: info was in eventstreams103.md, moved because of doc app changes -->

## Utilizzo del client Ruby MQ Light
{: #mql_ruby}


Per utilizzare l'API, aggiungi un riferimento all'ultima API client {{site.data.keyword.mql}} disponibile per Ruby nel seguente modo:

Aggiungi il seguente riferimento al <code>Gemfile</code>:

```
gem 'mqlight', '~> 1.0'
```
{: codeblock}

E aggiungi la seguente istruzione require al tuo file di origine:

<pre class="pre"><code>require &lsquo;mqlight&rsquo;</code></pre>
{: codeblock}

<!-- 14/11/18: info was in eventstreams101.md, moved because of doc app changes -->

## Utilizzo del client Python MQ Light
{: #mql_python}

Per utilizzare l'API, aggiungi un riferimento all'ultima API client {{site.data.keyword.mql}} disponibile per Python nel seguente modo:

Aggiungi il seguente riferimento al file
<code>requirements.txt</code>:

```
git+git://github.com/mqlight/python-mqlight.git@readthedocs
```
{:codeblock}

<br>
E aggiungi la seguente istruzione import al tuo file di origine:

```
import mqlight
```
{:codeblock}
<!-- Comment from Andrew
Instructions for getting started, with links for more info
Simple send source and receive source in-line

-->
