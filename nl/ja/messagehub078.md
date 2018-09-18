---

copyright:
  years: 2015, 2018
lastupdated: "2016-11-22"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.messagehub}} での {{site.data.keyword.mql}} API の使用法
{: #mql_how}


この API を使用するには、以下のように、選択した言語用の最新の使用可能な {{site.data.keyword.mql}} クライアント API への参照を追加します。

## Java の場合
{: #mql_java_how notoc}

<code>Maven pom</code> ファイルに次の参照を追加します。

```
<dependency>
    <groupId>com.ibm.mqlight</groupId>
    <artifactId>mqlight-api</artifactId>
    <version>LATEST</version>
</dependency>
```
{:codeblock}



## Node.js の場合
{: #mql_node_how notoc}

<code>package.json</code> ファイルの dependency セクションに次の参照を追加します。

<pre class="pre"><code>"mqlight" : "1.0.x"</code></pre>
{: codeblock}

ソース・ファイルに次の require ステートメントを追加します。

<pre class="pre"><code>var mqlight = require(‘mqlight’);</code></pre>
{: codeblock}


## Ruby の場合
{: #mql_ruby_how notoc}

<code>Gemfile</code> ファイルに次の参照を追加します。

```
gem 'mqlight', '~> 1.0'
```
{: codeblock}

<br>
ソース・ファイルに次の require ステートメントを追加します。

```
require ‘mqlight’
```
{: codeblock}



## Python の場合
{: #mql_python_how notoc}

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


