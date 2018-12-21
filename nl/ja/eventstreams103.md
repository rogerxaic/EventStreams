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

<!-- 14/11/18: info moved to eventstreams099.md, moved because of doc app changes -->
# MQ Light Ruby クライアントの使用
{: #mql_how}

**MQ Light API は、標準プランのみの一部として使用可能です。**
<br/>

この API を使用するには、以下のように、Ruby 用の最新の使用可能な {{site.data.keyword.mql}} クライアント API への参照を追加する必要があります。

<code>Gemfile</code> ファイルに次の参照を追加します。

```
gem 'mqlight', '~> 1.0'
```
{: codeblock}

ソース・ファイルに次の require ステートメントを追加します。

```
require ‘mqlight’
```
{: codeblock}

<!-- Comment from Andrew
Instructions for getting started, with links for more info
Simple send source and receive source in-line

-->


