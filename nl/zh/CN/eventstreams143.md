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
{:note: .note}

# 升级到新的 {{site.data.keyword.messagehub}} 标准套餐 
{: #migrate_classic_plan}

多租户标准套餐的这一新发行版在弹性、功能和易用性方面有显著改进。有关更多信息，请参阅[新的标准套餐博客声明 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud/blog/announcements/ibm-event-streams-releases-a-new-and-enhanced-standard-plan)。
{: shortdesc}

要将应用程序从先前的标准套餐（现在称为经典套餐）迁移到新的套餐，请考虑以下信息。

服务实例现在作为 {{site.data.keyword.cloud_notm}} 服务而不是 Cloud Foundry 服务供应。这将启用服务以支持最新的 {{site.data.keyword.cloud_notm}} 标准和功能，包括多专区部署和详细的访问控制，但会影响服务的使用方式。尤其是需要考虑以下方面：

* **创建、删除和列出服务**
<br/>
    像以前一样，您可以使用 {{site.data.keyword.cloud_notm}} 控制台或 {{site.data.keyword.cloud_notm}} CLI 命令行工具管理服务的生命周期。如果您使用的是控制台，那么服务现在将在**服务**下而不是 **Cloud Foundry 服务**下列出。 
    
    如果您使用的是 CLI，将使用资源命令管理实例。例如，<code>ibmcloud resource service-instance-create</code>。而不使用 **cf** 命令，例如 <code>ibmcloud cf create-service</code>。

* **控制访问**
<br/>
    现在使用 Cloud Identity and Access Management (IAM) 服务管理认证和授权。为了控制用户的连接能力，IAM 还支持您配置对底层资源（例如主题）的详细访问权。访问权是通过向用户分配策略来控制的。有关更多信息，请参阅[管理 {{site.data.keyword.messagehub}} 资源的访问权](/docs/services/EventStreams?topic=eventstreams-security)。

<ul>
<li><strong>连接应用程序</strong>
<br/>
    应用程序进行连接所需的信息不变，即，需要 <code>bootstrap.servers</code>、<code>username</code> 和 <code>password</code> 的列表。但是，检索这些属性的方式发生了变化。

<ul>
<li>
      <strong>对于本机应用程序</strong>
        <br/>
        您必须分别使用 IBM Cloud 控制台或 CLI，创建凭证或服务密钥对象。然后，您可以检索所需的属性。有关更多信息，请参阅[连接应用程序](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_external)。</li>
<br/>
<li><strong>对于 Cloud Foundry 应用程序</strong>
        <br/>
        您必须通过创建服务别名，先将服务绑定到应用程序的组织和空间。然后，您可以从 VCAP_SERVICES 环境变量正常检索所需的属性。有关更多信息，请参阅[连接 {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting)。
</li>
</ul>
<br/>
注意，客户机必须支持 TLS 的 SNI 扩展，其中服务器的主机名包含在 TLS 握手中。在[选择 Kafka 客户机以用于 {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_clients) 中建议的所有客户机版本中，通常会提供并支持此功能。
</li>
</ul>

<br/>
您还应注意如下所示的其他一些变更：

* **Kafka 版本**
<br/>
    此套餐提供对最新稳定 Kafka 发行版 2.2 的访问权。无需更改就可以运行针对 Kafka 1.1 开发的应用程序，但是对于建议的客户机版本和测试的组合，请参阅以下信息：
[选择 Kafka 客户机以用于 {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_clients)。 

* **支持的区域**
<br/>
    本套餐最初仅在 us-south 中提供。其余区域将在未来几周内引进。

* **集成**
<br/>
    从 {{site.data.keyword.iot_short_notm}} 或 {{site.data.keyword.openwhisk_short}} 之类的其他服务进行的连接使用 Cloud Foundry 组织和空间绑定到服务，需要创建服务别名。有关更多信息，请参阅[将 Cloud Foundry 应用程序连接到 {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_cf)。
    

* **支持的功能**
<br/>
    经典套餐和新的标准套餐的功能有不同之处。要与提供的产品保持一致，请采用新的技术选项，并除去使用较少的功能，并非所有功能都会继承下去。[选择您的套餐](/docs/services/EventStreams?topic=eventstreams-plan_choose)中提供了功能的比较。如果您依赖这些功能，稍后将提供进一步的信息以帮助您迁移。
   
<br/>
每天会向生产环境小幅增加代码。因此，在 2019 年余下的时间以及往后，您将看到我们的用户体验（以及其他方面）会有许多进一步的改进。即将发布：

* **客户度量值**
<br/>
    监视服务实例中活动的功能。

<br/>
要获取快速入门和熟悉运用新标准套餐的步骤的快速说明，请试用[入门教程](/docs/services/EventStreams?topic=eventstreams-getting_started)。


