---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-23"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}

# 使用 REST producer API
{: #rest_producer_using}


**REST producer API 仅在 {{site.data.keyword.messagehub}} 标准和企业套餐中提供。**
<br/>

{{site.data.keyword.messagehub}} 提供 REST API 以帮助您将现有系统连接到 {{site.data.keyword.messagehub}} Kafka 集群。使用 API，您可以将 {{site.data.keyword.messagehub}} 与支持 RESTful API 的任何系统集成。

REST producer API 是可缩放的 REST 接口，用于通过安全 HTTP 端点向 {{site.data.keyword.messagehub}} 生成消息。将事件数据发送到 {{site.data.keyword.messagehub}}，利用 Kafka 技术处理数据订阅源，并利用 {{site.data.keyword.messagehub}} 功能来管理数据。

使用 API 将现有系统连接到 {{site.data.keyword.messagehub}}。从系统创建针对 {{site.data.keyword.messagehub}} 的生成请求，包括指定消息密钥、头、以及您想要将消息写入的主题。


## 使用 REST 生成消息 
{: #rest_produce_messages}

使用 producer API 将消息写入主题。要能够生成到主题中，您必须具有以下信息：

* {{site.data.keyword.messagehub}} API 端点的 URL，包括端口号。
* 想要向其生成内容的主题。
* 用于提供连接到所选主题并向其生成内容的许可权的 API 密钥。

您必须检索从服务实例的服务凭证对象或服务密钥连接到 API 所需的 URL 和凭证详细信息。有关创建这些对象的信息，请参阅[连接到 {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting)。

<code>kafka_http_url</code> 属性中提供了 API 的端点的 URL。

使用以下某个方法进行认证：

* **使用基本认证进行认证：**<br/>
    将上述对象的 <code>user</code> 和 <code>api_key</code> 属性用作用户名和密码。以 <code>Basic &lt;base64 encoding of username and password joined by a single colon (:)&gt;</code> 形式将这些值放入 HTTP 请求的 <code>Authorization</code> 头中。

* **使用不记名令牌进行认证：**<br/>
    要使用 IBM Cloud CLI 获取令牌，请先登录到 IBM Cloud，然后运行以下命令： 

    ```
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

    以 <code>Bearer <token></code> 形式将此令牌放在 HTTP 请求的 Authorization 头中。支持 API 密钥或 JWT 令牌。 

* **使用 api_key 直接进行认证：**<br/>
    将密钥直接用作 <code>X-Auth-Token</code> HTTP 头的值。

<br/>
以下代码显示使用 curl 发送消息的示例：

```
curl -v -X POST -H "Authorization: Basic <base64 username:password>" -H "Content-Type: text/plain" -H "Accept: application/json" -d 'test message' "<kafka_http_url>/topics/<topic_name>/records"
```
{: codeblock}


## API 参考
{: #rest_api_reference}

有关 API 的完整详细信息，请参阅 [{{site.data.keyword.messagehub}}REST Producer API 参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://ibm.github.io/event-streams/api/){:new_window}。












