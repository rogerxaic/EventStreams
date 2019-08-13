---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-15"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 使用 Administration REST API
{: #admin_api}

{{site.data.keyword.messagehub}} 提供了一个 Administration RESTful API，可用于创建、删除、列出和更新主题。
{: shortdesc}

您必须检索从服务实例的服务凭证对象或服务密钥连接到 API 所需的 URL 和凭证详细信息。有关创建这些对象的信息，请参阅[连接到 {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting)。

<code>kafka_admin_url</code> 属性中提供了 API 的端点的 URL。

凭证依赖认证方法，并且支持三种类型的凭证：

* **使用基本认证进行认证**：<br/>
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

对于经典套餐上创建的服务实例，可以改为从应用程序的 VCAP_SERVICES 环境变量中获取此信息。

有关 API 的描述及示例，请参阅 [{{site.data.keyword.messagehub}} admin-rest ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-messaging/event-streams-docs/tree/master/admin-rest-api){:new_window}。

您可以从 [{{site.data.keyword.messagehub}} Admin REST API YAML 文件 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-messaging/event-streams-docs/blob/master/admin-rest-api/admin-rest-api.yaml){:new_window} 下载 API 的完整规范。
要查看 Swagger 文件，请使用 Swagger 工具，例如 [Swagger 编辑器 ![外部链接图标](../../icons/launch-glyph.svg " 外部链接图标")](http://editor.swagger.io/#/){:new_window}。




