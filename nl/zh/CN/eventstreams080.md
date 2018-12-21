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

<!-- 14/11/18: info moved to eventstreams075.md, moved because of doc app changes -->
# 如何连接现有 MQ Light 应用程序
{: #mql_exist_apps}

**MQ Light API 仅在标准套餐中提供。**
<br/>

可以将当前针对 {{site.data.keyword.IBM_notm}} MQ 或 {{site.data.keyword.mql}}
{{site.data.keyword.Bluemix_notm}} 服务运行的现有应用程序连接到该服务。应用程序会以同样方式继续工作。
{:shortdesc}

要连接现有应用程序，请完成以下检查：

* 确保应用程序使用的是适合您语言的最新可用的 {{site.data.keyword.mql}} API 客户机版本。
* 检查从 VCAP_SERVICES 中提取的连接详细信息是否引用的是 <code>messagehub</code> 服务类型，在 <code>credentials.user</code> 属性（而不是 <code>credentials.username</code> 属性）中检索连接用户名，以及在 <code>credentials.mqlight_lookup_url</code> 属性（而不是 <code>credentials.connectionLookupURI</code> 属性）中检索连接查找 URL。有关更多信息，请参阅 [VCAP_SERVICES 环境变量](/docs/services/EventStreams/eventstreams127.html)。

	请注意，如果使用的是 Java&trade; 客户机，并且将“null”指定为 create() 调用中的 endpointService 参数，以便客户机自行检索信息，那么此步骤已完成。
	
* 应用程序必须支持 TLS V1.2 连接。VCAP_SERVICES 不再包含用于创建非 TLS 连接的 <code>credentials.nonTLSConnectionLookupURI</code> 属性。

此外，还应该注意以下信息：

* 消息限制与 {{site.data.keyword.messagehub}} 一致，但可能与支持 {{site.data.keyword.mql}} API 的其他服务器不同。有关更多信息，请参阅[最大限制](/docs/services/EventStreams/eventstreams083.html)。
* 不支持 JMS。
