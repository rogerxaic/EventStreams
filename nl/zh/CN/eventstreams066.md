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

# 如何连接和认证
{: #rest_connect}

**Kafka REST API 仅在标准套餐中提供。**
<br/>

为了连接到 {{site.data.keyword.messagehub}}，Kafka REST API 使用 [VCAP_SERVICES 环境变量](/docs/services/EventStreams/eventstreams127.html)中的 <code>api_key</code> 和 <code>kafka_rest_url</code> 凭证。

要向 {{site.data.keyword.messagehub}} Kafka REST API 进行认证，您必须在请求的 X-Auth-Token 头中指定 <code>api_key</code>。
