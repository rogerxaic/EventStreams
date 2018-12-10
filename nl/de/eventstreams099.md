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

# MQ Light-Clients verwenden
{: #mql_clients}

** Die MQ Light-API ist nur als Bestandteil des Plans "Standard" verfügbar.**
<br/>
## MQ Light-Java-Client verwenden
{: #mql_java}

Um die API zu verwenden, fügen Sie einen Verweis auf die neueste verfügbare {{site.data.keyword.mql}}-Client-API für Java wie folgt hinzu:

Fügen Sie den folgenden Verweis in Ihrer Datei <code>Maven pom</code> hinzu:

```
<dependency>
    <groupId>com.ibm.mqlight</groupId>
    <artifactId>mqlight-api</artifactId>
    <version>LATEST</version>
</dependency>
```
{:codeblock}

<!-- 12/11/18: info was in eventstreams102.md, moved because of doc app changes -->

## MQ Light-Node.js-Client verwenden 
{: #mql_node}


Um die API zu verwenden, fügen Sie einen Verweis auf die neueste verfügbare {{site.data.keyword.mql}}-Client-API für Node.js wie folgt hinzu:

Fügen Sie den folgenden Verweis im Abschnitt für Abhängigkeiten in Ihrer Datei <code>package.json</code> hinzu:

<pre class="pre"><code>"mqlight" : "1.0.x"</code></pre>
{: codeblock}

Fügen Sie außerdem die folgende Anweisung 'require' in Ihrer Quellendatei hinzu:

<pre class="pre"><code>var mqlight = require(&lsquo;mqlight&rsquo;);</code></pre>
{: codeblock}

<!-- 14/11/18: info was in eventstreams103.md, moved because of doc app changes -->

## MQ Light-Ruby-Client verwenden
{: #mql_ruby}


Um die API zu verwenden, fügen Sie einen Verweis auf die neueste verfügbare {{site.data.keyword.mql}}-Client-API für Ruby wie folgt hinzu:

Fügen Sie den folgenden Verweis zur <code>Gemfile</code> hinzu:

```
gem 'mqlight', '~> 1.0'
```
{: codeblock}

Fügen Sie außerdem die folgende Anweisung 'require' in Ihrer Quellendatei hinzu:

<pre class="pre"><code>require &lsquo;mqlight&rsquo;</code></pre>
{: codeblock}

<!-- 14/11/18: info was in eventstreams101.md, moved because of doc app changes -->

## MQ Light-Python-Client verwenden
{: #mql_python}

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
