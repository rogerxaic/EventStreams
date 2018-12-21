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
# 使用 MQ Light Ruby 客户机
{: #mql_how}

**MQ Light API 仅在标准套餐中提供。**
<br/>

要使用 API，请添加对用于 Ruby 的最新可用 {{site.data.keyword.mql}} 客户机 API 的引用，如下所示：

将以下引用添加到 <code>Gemfile</code>：

```
gem 'mqlight', '~> 1.0'
```
{: codeblock}

将以下 require 语句添加到源文件：

```
require ‘mqlight’
```
{: codeblock}

<!-- Comment from Andrew
Instructions for getting started, with links for more info
Simple send source and receive source in-line

-->


