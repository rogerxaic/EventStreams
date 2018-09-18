---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

<!-- Notes from chat with Charlie 

Different plan for provisioning

Quality of service from each plan

Life of a user through cycle - APIs, feature sets

-->

# 关于 Alpha 程序
{: #alpha_about }

通过 Alpha 程序，您可以及早访问 {{site.data.keyword.messagehub}} 服务的新功能。当前功能是引入新套餐，即 {{site.data.keyword.messagehub}} 高端套餐。

## 高端套餐
{: premium_plan}

高端套餐适用于对性能和其他功能要求高出公共服务的用户。高端套餐提供单租户版本的 {{site.data.keyword.messagehub}} 服务，您是该服务中使用 Apache Kafka 集群的唯一租户。此版本支持：

* 充分利用集群的能力和性能

* 显著增加限制和分区数

* 利用自动维护和更新的完全受管集群，没有任何管理成本

## 关于 Alpha 集群
{: alpha_cluster}

Alpha 集群部署了 Apache Kafka V1.1，能够提供的最大消息吞吐量为 90 000 KB/秒。 

最多可以创建 1000 个分区，每个分区最多可以保留 1 GB 数据，保留时间最长 30 天。为了实现弹性，数据跨 3 个副本存储，并且每个分区的已落实偏移量最长保留 7 天。

支持以下两个 API：

* 对于消息传递，支持 Kafka 客户机 V0.10.x 以及更高版本，包括使用 Kafka Streams、Kafka Connect 和 KSQL 的选项。

* 对于管理，可使用 REST API 来创建、删除和列出主题。

Alpha 程序仅在美国南部地区可用。对集群的访问权使用 IAM 进行管理。

## 连接到集群
{: alpha_connect}

要连接到集群中的 API，您需要其端点 URL 和用于认证的 apikey。可以使用下列其中一种方法，从 IAM 中检索到这些详细信息：

### Cloud Foundry 应用程序
对于 Cloud Foundry 应用程序：
1. 在应用程序的**连接**选项卡（左侧）中，单击**创建连接**按钮。 
2. 选择要连接的 {{site.data.keyword.messagehub}} 服务实例，然后单击**连接**。接受缺省选项。 
3. 连接完成时，单击应用程序的**运行时**选项卡，然后单击**环境变量**以显示 **VCAP_SERVICES**。

### 外部应用程序的控制台
在外部应用程序的控制台中，使用以下 **bx** 命令创建服务 apikey： 

```
bx resource service-key-create name-of-key Manager --instance-name name-of-your-service
``` 

从生成的信息中，复制 <code>kafka_brokers_sasl</code>、<code>kafka_admin_url</code> 和 <code>apikey</code> 字段。
VCAP_SERVICES 中仅列出前 5 个代理程序。如果您具有 5 个以上的代理程序，请使用 Kafka 客户机来检索其他代理程序的详细信息。 

## 将客户机连接到 Kafka API

要将客户机连接到 Kafka API，请完成以下步骤：

1. 将客户机的 <code>bootstrap.servers</code> 属性设置为 <code>kafka_brokers_sasl</code> 中列出的代理程序的逗号分隔列表。

2. 将客户机的 <code>sasl.jaas.config</code> USERNAME 字段设置为 <code>apikey</code> 的前 8 个字符，将 PASSWORD 字段设置为 apikey 的其余字符（未来版本中不需要进行这样拆分）

使用的 Kafka 客户机必须支持以下功能：

* 客户机 V0.10.x 或更新版本

* 使用 SASL Plain 机制进行认证

* TLS V1.2 协议的服务名称标识 (SNI) 扩展

此端点和凭证信息检索方法不同于现有的 {{site.data.keyword.messagehub}} 服务。当前针对 {{site.data.keyword.messagehub}} 运行的应用程序需要更改以反映出 VCAP_SERVICES 中必需的替代字段名称，还需要更改提交到 Kafka 的用户名/密码字段。未来的 Alpha 版本无需这些更改。

## 将客户机连接到 REST API

要将客户机连接到 REST API，请完成以下步骤：

* 在 <code>kafka_admin_url</code> 中提供 REST API 的 URI

* 将 HTTP <code>Content-Type</code> 头设置为 <code>application/json</code>

* 将 HTTP <code>X-Auth-Token</code> 头设置为 <code>apikey</code> 的值 

有关快速入门和熟悉运用 Alpha 的简单步骤，请参阅 [Alpha 程序入门](/docs/services/MessageHub/messagehub120.html)。


## 管理 {{site.data.keyword.messagehub}}
{: alpha_admin}

集群中需要执行的管理任务只有创建、列出和删除您需要的主题。以使用下列其中一种方法进行管理：

* 直接通过应用程序使用 Kafka 管理 API。例如，对于 Java，通过 AdminClient ![外部链接图标](http://kafka.apache.org/11/javadoc/index.html?org/apache/kafka/clients/admin/AdminClient.html){:new_window} 使用 <code>createTopics()</code>、<code>deleteTopics()</code> 或 <code>listTopics()</code> 方法。


* 以交互方式使用 IBM Cloud 门户网站中提供的服务实例的 Web UI。

* 集群中提供的管理 REST API。

可以在 [admin-rest-api.yaml ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-messaging/message-hub-docs/blob/master/admin-rest-api/admin-rest-api.yaml){:new_window} 中提供的 API 的完整规范中，找到有关通过管理 REST API（与现有 {{site.data.keyword.messagehub}} 管理 API 兼容）提供的创建、列出和删除管理功能的进一步详细信息。
要查看 Swagger 文件，请使用 Swagger 工具，例如 [Swagger 编辑器 ![外部链接图标](../../icons/launch-glyph.svg " 外部链接图标")](http://editor.swagger.io/#/){:new_window}。

有关演示如何使用 Curl 创建主题的简单示例，请参阅 [Alpha 程序入门](/docs/services/MessageHub/messagehub120.html)。

未来，还将提供其他配置选项。


## 样本

即将推出...

有关快速入门和熟悉运用 Alpha 的简单步骤，请参阅 [Alpha 程序入门](/docs/services/MessageHub/messagehub120.html)。

## Alpha 限制
{: alpha_limitations}

此 Alpha 程序当前具有以下限制：

### 尚不可用，但即将推出的功能

* 支持多个可用性区域

* 用户控制的扩展和负载限制

* 与 {{site.data.keyword.cloudaccesstrailfull_notm}} 服务（先前称为 AccessTrail）集成 

### 目前未计划的功能

* 网桥

* REST 消息传递

* MQ Light API










