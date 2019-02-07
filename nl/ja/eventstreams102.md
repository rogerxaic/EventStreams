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

<!-- 12/11/18: info moved to eventstreams099.md, moved because of doc app changes -->
# MQ Light Node.js クライアントの使用 
{: #mql_node}

**MQ Light API は、標準プランのみの一部として使用可能です。**
<br/>

この API を使用するには、以下のように、Node.js 用の最新の使用可能な {{site.data.keyword.mql}} クライアント API への参照を追加する必要があります。
{: shortdesc}

<code>package.json</code> ファイルの dependency セクションに次の参照を追加します。

<pre class="pre"><code>"mqlight" : "1.0.x"</code></pre>
{: codeblock}

ソース・ファイルに次の require ステートメントを追加します。

<pre class="pre"><code>var mqlight = require(‘mqlight’);</code></pre>
{: codeblock}

<!-- Comment from Andrew
Instructions for getting started, with links for more info
Simple send source and receive source in-line

-->


