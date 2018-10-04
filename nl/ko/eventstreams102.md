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

# MQ Light Node.js 클라이언트 사용 
{: #mql_node}

** MQ Light API는 표준 플랜의 일부로만 사용 가능합니다.**
<br/>

API를 사용하려면 다음과 같이 사용 가능한 최신 Node.js용 {{site.data.keyword.mql}} 클라이언트 API에 참조를 추가하십시오.

<code>package.json</code> 파일의 종속성 섹션에 다음 참조를 추가하십시오.

<pre class="pre"><code>"mqlight" : "1.0.x"</code></pre>
{: codeblock}

그리고 소스 파일에 다음 require 문을 추가하십시오.

<pre class="pre"><code>var mqlight = require(‘mqlight’);</code></pre>
{: codeblock}

<!-- Comment from Andrew
Instructions for getting started, with links for more info
Simple send source and receive source in-line

-->


