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

<!-- 12/11/18: info moved to eventstreams099.md, moved because of doc app changes -->
# 使用 MQ Light Node.js 用戶端 
{: #mql_node}

** MQ Light API 只提供於標準方案中。**
<br/>

若要使用 API，請為 Node.js 新增參照到最新的可用 {{site.data.keyword.mql}} 用戶端 API，如下所示：

新增下列參照到 <code>package.json</code> 檔案的 dependency 區段：

<pre class="pre"><code>"mqlight" : "1.0.x"</code></pre>
{: codeblock}

然後新增下列 require 陳述式到您的原始檔：

<pre class="pre"><code>var mqlight = require('mqlight');</code></pre>
{: codeblock}

<!-- Comment from Andrew
Instructions for getting started, with links for more info
Simple send source and receive source in-line

-->


