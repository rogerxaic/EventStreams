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

# MQ Light クライアントの使用
{: #mql_clients}

**MQ Light API は、標準プランのみの一部として使用可能です。**
<br/>
## MQ Light Java クライアントの使用
{: #mql_java}

この API を使用するには、以下のように、Java 用の最新の使用可能な {{site.data.keyword.mql}} クライアント API への参照を追加する必要があります。

<code>Maven pom</code> ファイルに次の参照を追加します。

```
<dependency>
    <groupId>com.ibm.mqlight</groupId>
    <artifactId>mqlight-api</artifactId>
    <version>LATEST</version>
</dependency>
```
{:codeblock}

<!-- 12/11/18: info was in eventstreams102.md, moved because of doc app changes -->

## MQ Light Node.js クライアントの使用 
{: #mql_node}


この API を使用するには、以下のように、Node.js 用の最新の使用可能な {{site.data.keyword.mql}} クライアント API への参照を追加する必要があります。

<code>package.json</code> ファイルの dependency セクションに次の参照を追加します。

<pre class="pre"><code>"mqlight" : "1.0.x"</code></pre>
{: codeblock}

ソース・ファイルに次の require ステートメントを追加します。

<pre class="pre"><code>var mqlight = require(&lsquo;mqlight&rsquo;);</code></pre>
{: codeblock}

<!-- 14/11/18: info was in eventstreams103.md, moved because of doc app changes -->

## MQ Light Ruby クライアントの使用
{: #mql_ruby}


この API を使用するには、以下のように、Ruby 用の最新の使用可能な {{site.data.keyword.mql}} クライアント API への参照を追加する必要があります。

<code>Gemfile</code> ファイルに次の参照を追加します。

```
gem 'mqlight', '~> 1.0'
```
{: codeblock}

ソース・ファイルに次の require ステートメントを追加します。

<pre class="pre"><code>require &lsquo;mqlight&rsquo;</code></pre>
{: codeblock}

<!-- 14/11/18: info was in eventstreams101.md, moved because of doc app changes -->

## MQ Light Python クライアントの使用
{: #mql_python}

この API を使用するには、以下のように、Python 用の最新の使用可能な {{site.data.keyword.mql}} クライアント API への参照を追加する必要があります。

<code>requirements.txt</code> ファイルに次の参照を追加します。

```
git+git://github.com/mqlight/python-mqlight.git@readthedocs
```
{:codeblock}

<br>
ソース・ファイルに次の import ステートメントを追加します。

```
import mqlight
```
{:codeblock}
<!-- Comment from Andrew
Instructions for getting started, with links for more info
Simple send source and receive source in-line

-->
