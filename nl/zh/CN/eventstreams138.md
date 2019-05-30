---

copyright:
  years: 2015, 2019
lastupdated: "2018-07-05"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 在经典套餐上使用 Administration REST API
{: #admin_api_standard}

{{site.data.keyword.messagehub}} 提供了一个管理 RESTful API，可用于创建、删除和列出主题。
{: shortdesc}

要发现管理 RESTful API 的端点，您的 {{site.data.keyword.Bluemix_short}} 应用程序可以使用应用程序 VCAP_SERVICES 环境变量中的
`kafka_admin_url` 属性。

可以从 [admin-rest-api.yaml ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-messaging/event-streams-docs/blob/master/admin-rest-api/admin-rest-api.yaml){:new_window} 下载该 API 的完整规范。要查看 Swagger 文件，请使用 Swagger 工具，例如 [Swagger 编辑器 ![外部链接图标](../../icons/launch-glyph.svg " 外部链接图标")](http://editor.swagger.io/#/){:new_window}。

[{{site.data.keyword.messagehub}} admin-rest-api ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-messaging/event-streams-docs/tree/master/admin-rest-api){:new_window} 目录还包含一组有用的示例，用于说明如何调用 Administration RESTful API。


