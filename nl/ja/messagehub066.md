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

# 接続および認証の方法
{: #rest_connect}

**Kafka REST API は、標準プランのみの一部として使用可能です。**
<br/>

{{site.data.keyword.messagehub}} に接続するために、Kafka REST API は、<code>api_key</code> および <code>kafka_rest_url</code> 資格情報を [VCAP_SERVICES 環境変数](/docs/services/MessageHub/messagehub127.html)から使用します。

{{site.data.keyword.messagehub}} Kafka REST API で認証するには、
要求の X-Auth-Token ヘッダーに <code>api_key</code> を指定する必要があります。
