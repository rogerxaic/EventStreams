---

copyright:
  years: 2015, 2019
lastupdated: "2018-05-25"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Utilización de clientes de MQ Light
{: #mql_clients}

**La API MQ Light solo está disponible como parte del plan Estándar.**
<br/>
## Uso del cliente MQ Light Java
{: #mql_java}

Para utilizar la API, añada una referencia a la última API de cliente {{site.data.keyword.mql}} disponible para Java de la siguiente manera:
{: shortdesc}

Añada la siguiente referencia al archivo <code>Maven</code>:

```
<dependency>
    <groupId>com.ibm.mqlight</groupId>
    <artifactId>mqlight-api</artifactId>
    <version>LATEST</version>
</dependency>
```
{:codeblock}

<!-- 12/11/18: info was in eventstreams102.md, moved because of doc app changes -->

## Uso del cliente MQ Light Node.js 
{: #mql_node}


Para utilizar la API, añada una referencia a la última API de cliente {{site.data.keyword.mql}} disponible para Node.js de la siguiente manera:

Añada la siguiente referencia a la sección dependencia del archivo <code>package.json</code>:

<pre class="pre"><code>"mqlight" : "1.0.x"</code></pre>
{: codeblock}

A continuación, añada la siguiente sentencia necesaria al archivo de origen:

<pre class="pre"><code>var mqlight = require(&lsquo;mqlight&rsquo;);</code></pre>
{: codeblock}

<!-- 14/11/18: info was in eventstreams103.md, moved because of doc app changes -->

## Uso del cliente MQ Light Ruby cliente
{: #mql_ruby}


Para utilizar la API, añada una referencia a la última API de cliente {{site.data.keyword.mql}} disponible para Ruby de la siguiente manera:

Añada la siguiente referencia a <code>Gemfile</code>:

```
gem 'mqlight', '~> 1.0'
```
{: codeblock}

A continuación, añada la siguiente sentencia necesaria al archivo de origen:

<pre class="pre"><code>require &lsquo;mqlight&rsquo;</code></pre>
{: codeblock}

<!-- 14/11/18: info was in eventstreams101.md, moved because of doc app changes -->

## Uso del cliente MQ Light Python
{: #mql_python}

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
