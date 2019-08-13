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


# 连接到 {{site.data.keyword.messagehub}}
{: #connecting}

连接到 {{site.data.keyword.messagehub}} 的方式会有所不同，具体取决于您的应用程序是以本机方式运行还是作为 Cloud Foundry 应用程序运行。但是，在两种情况下，都需要两条信息：
{: shortdesc}

* API 的端点 URL
* 认证凭证

阅读以下信息以了解获取这些详细信息的方式。步骤可能有细微差别，因此请确保完成您的实例的相应步骤。

有关如何连接 {{site.data.keyword.messagehub}} 的信息（如果使用经典套餐），请参阅[使用经典套餐连接](/docs/services/EventStreams?topic=eventstreams-connecting_classic)。


## 概述
{: #connect_enterprise}

使用标准套餐或企业套餐供应的服务将分组到仪表板中标题**服务**下。套餐已[启用 IAM ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/iam?topic=iam-getstarted#getstarted){:new_window}。您不需要了解 IAM 即可入门，但是如果您想要保护 {{site.data.keyword.messagehub}} 服务，建议您了解一些相关知识。有关更多信息，请参阅[管理 {{site.data.keyword.messagehub}} 资源的访问权](/docs/services/EventStreams?topic=eventstreams-security)。

完成以下步骤，以绑定您的应用程序并获取服务的服务密钥。要获得授权以创建主题，您的应用程序或服务密钥必须具有管理者访问角色。

要连接应用程序，使用的方法取决于该应用程序的部署位置，即在 Cloud Foundry 内部还是外部，例如在 Kubernetes 服务中。

## 供应 {{site.data.keyword.messagehub}} 实例
{: #provision_instance}

作为使用服务的先决条件，无论是标准套餐还是企业套餐，都必须先供应 {{site.data.keyword.messagehub}} 服务实例。接下来，通过完成以下任务获取
{{site.data.keyword.messagehub}} API 连接详细信息。

## 连接应用程序 
{: #connect_enterprise_external}

对于 Cloud Foundry 之外运行的应用程序，创建服务密钥将生成凭证。当您获取服务密钥时，请使用所选方法将密钥详细信息手动传递给应用程序。

### 使用 IBM Cloud 控制台获取凭证 
{: #connect_enterprise_external_console}

1. 在仪表板上找到您的 {{site.data.keyword.messagehub}} 服务。
2. 单击服务磁贴。
3. 单击**服务凭证**。
4. 单击**新建凭证**。 
5. 填写新凭证的详细信息，例如名称和角色，并单击**添加**。凭证列表中将显示新的凭证。
6. 通过**查看凭证**单击此凭证，以采用 JSON 格式显示详细信息。
7. 将这些凭证传递给应用程序。指定 <code>token</code> 作为用户名，<var class="keyword varname">api_key</var> 作为密码。使用冒号分隔 <code>token</code> 和 <var class="keyword varname">api_key</var>。有关更多信息，请参阅[配置 Kafka API 客户机](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_api_client)。
   <br/><br/>确保您的应用程序会解析这些详细信息。

### 使用 IBM Cloud CLI 获取凭证 
{: #connect_enterprise_external_cli}

<ol>
<li>找到您的服务：<br/>
<code>ibmcloud resource service-instances</code></li>
<li>创建服务密钥：<br/>
<code>ibmcloud resource service-key-create <var class="keyword varname">key_name</var> <var class="keyword varname">key_role</var> --instance-name <var class="keyword varname">your_service_name</var></code></li>
<li>打印服务密钥：<br/>
<code>ibmcloud resource service-key <var class="keyword varname">key_name</var></code></li>
<li>将这些凭证传递给应用程序。指定 <code>token</code> 作为用户名，<var class="keyword varname">api_key</var> 作为密码。使用冒号分隔 <code>token</code> 和 <var class="keyword varname">api_key</var>。有关更多信息，请参阅[配置客户机](/docs/services/EventStreams?topic=eventstreams-kafka_connect)。<p>确保您的应用程序会解析这些详细信息。</p></li>
</ol>

## 连接 Cloud Foundry 应用程序
{: #connect_enterprise_cf}

您的应用程序必须绑定到 {{site.data.keyword.messagehub}} 服务实例。要将 Cloud Foundry 应用程序绑定到非 Cloud Foundry 服务，请先创建 Cloud Foundry 服务别名，然后在绑定时从 Cloud Foundry 应用程序引用此别名。 

绑定后，连接详细信息将通过 VCAP_SERVICES 环境变量以 JSON 格式提供给应用程序。您可以使用 [IBM Cloud 控制台](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_cf_console)或 [IBM Cloud CLI](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_cf_cli) 绑定应用程序和服务。

### 使用 IBM Cloud 控制台绑定应用程序
{: #connect_enterprise_cf_console}

1. 确保您位于计划使用的 Cloud Foundry 组织和空间中。
2. 在仪表板上找到您的 Cloud Foundry 应用程序，或者通过单击**创建资源**按钮创建应用程序。
3. 单击应用程序磁贴。
4. 单击**连接**。
5. 单击**创建连接**。
6. 选择要绑定到的 {{site.data.keyword.messagehub}} 服务磁贴，并单击**连接**。 
7. 在出现的**连接启用 IAM 的服务**窗口中，从**用于连接的访问角色**中选择访问角色，从**用于连接的服务标识**列表中选择服务标识（您可以接受自动生成的标识）。然后，单击**连接**。 

  这会为 {{site.data.keyword.messagehub}} 服务创建 Cloud Foundry 服务别名，然后将您的应用程序绑定到此别名。 

  重新编译打包您的应用程序，以使更改生效。<br/>
8. 单击左侧的**运行时**选项卡，并选择当中的**环境变量**选项卡。您现在可以验证 VCAP_SERVICES 信息。您的应用程序现在可以将这些信息作为环境变量进行访问。 
 

### 使用 IBM Cloud CLI 绑定应用程序
{: #connect_enterprise_cf_cli}

<ol>
<li>确保您位于计划使用的 Cloud Foundry 组织和空间中。您可以通过运行以下命令进行交互式导航：<br/>
 <code>ibmcloud target --cf</code></li>
<li>查找您的应用程序：</br>
<code>ibmcloud app list</code><br/>
<br/>
如果您具有清单文件，您可以通过运行以下命令新建应用程序：<br/>
<code>ibmcloud app push</code><br/>
<br/>
因为应用程序尚未绑定到 {{site.data.keyword.messagehub}}，所以应用程序无法建立连接。因此，建议您使用 <code>--no-start</code> 参数推送应用程序，以避免不必要的连接故障。</li>
<li>找到您的服务：</br>
<code>ibmcloud resource service-instances</code></li>
<li>创建 Cloud Foundry 服务别名：<br/>
<code>ibmcloud resource service-alias-create <var class="keyword varname">alias_name</var> --instance-name <var class="keyword varname">your_service_name</var></code></li>
<li>将您的应用程序绑定到先前创建的服务别名：<br/>
<code>ibmcloud service bind <var class="keyword varname">your_ app_name</var> <var class="keyword varname">alias_name</var></code>。<br/>
<br/>
或者，您可以更新清单文件，并再次推送应用程序。</li>
<li>验证 VCAP_SERVICES 环境变量在应用程序运行时中是否可用：<br/>
<code>ibmcloud app env <var class="keyword varname">your_app_name</var></code></li>
<li>将这些凭证传递给应用程序。指定 <code>token</code> 作为用户名，<var class="keyword varname">api_key</var> 作为密码。使用冒号分隔 <code>token</code> 和 <var class="keyword varname">api_key</var>。有关更多信息，请参阅[配置客户机](/docs/services/EventStreams?topic=eventstreams-kafka_connect)。<p>您可能需要重新编译打包您的应用程序，以使更改生效。</p></li>
</ol>


## 后续步骤
{: #after_connecting}

现在，您有了连接和凭证信息，可以选择一个 {{site.data.keyword.messagehub}} 客户机。有关更多信息，请参阅[使用 Kafka API](/docs/services/EventStreams?topic=eventstreams-kafka_using)。

<!--
Charlie said:

"Add some info describing how to take the information made available from above e.g. like the info in the Connecting a client to the Kafka API section of the alpha docs on stage 1? https://test.cloud.ibm.com/docs/services/EventStreams?topic=eventstreams-alpha_about#alpha_about"
-->







 















