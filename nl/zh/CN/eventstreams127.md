---

copyright:
  years: 2015, 2019
lastupdated: "2018-11-07"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 连接到 {{site.data.keyword.messagehub}}
{: #connecting}

连接到 {{site.data.keyword.messagehub}} 的方式会有所不同，具体取决于您使用的是标准套餐，还是企业套餐，以及您是从 Cloud Foundry 应用程序进行连接，还是从任何其他外部客户机进行连接。要连接到任何
{{site.data.keyword.messagehub}} API，您需要收集两条信息：
{: shortdesc}

* API 的端点 URL
* 认证凭证

阅读以下信息以了解获取这些详细信息的方式。步骤可能有细微差别，因此请确保完成您的实例的相应步骤。

## 供应 {{site.data.keyword.messagehub}} 实例

作为使用服务的先决条件，无论是标准套餐还是企业套餐，都必须先供应 {{site.data.keyword.messagehub}} 服务实例。接下来，通过完成以下任务获取
{{site.data.keyword.messagehub}} API 连接详细信息。

## 标准套餐概述
{: #connect_standard}

使用标准套餐供应的服务为 Cloud Foundry 服务。这意味着它们可以部署到 Cloud Foundry 组织和空间，并分组到仪表板中 **Cloud Foundry 服务**标题下。连接应用程序所使用的方法取决于该应用程序的部署位置，即在 Cloud Foundry 内部还是外部，例如在 Kubernetes 服务中。


## 标准套餐中的 Cloud Foundry 应用程序
{: #connect_standard_cf}

对于在 Cloud Foundry 内运行的应用程序，将应用程序绑定到 {{site.data.keyword.messagehub}} 服务实例。绑定后，连接详细信息将通过 VCAP_SERVICES 环境变量以 JSON 格式提供给应用程序。您可以使用 IBM Cloud 控制台或 IBM Cloud CLI 绑定应用程序和服务。

下面是 VCAP_SERVICES 的示例：

```
{
  "credentials": {
"mqlight_lookup_url": "https://mqlight-lookup.messagehub.services.us-south.bluemix.net/Lookup?serviceId=584e8436-e7f5-43db-96ac-2864fccae5ae",
    "api_key": "d9JSx1SYsmLzNRbbgUFneDm2DtkedlVeViObYJIvrPAf2kJA",
    "kafka_admin_url": "https://kafka-admin.messagehub.services.us-south.bluemix.net:443",
    "kafka_rest_url": "https://kafka-rest.messagehub.services.us-south.bluemix.net:443",
    "kafka_brokers_sasl": [
      "kafka01.messagehub.services.us-south.bluemix.net:9093",
      "kafka02.messagehub.services.us-south.bluemix.net:9093",
      "kafka03.messagehub.services.us-south.bluemix.net:9093",
      "kafka04.messagehub.services.us-south.bluemix.net:9093",
      "kafka05.messagehub.services.us-south.bluemix.net:9093"
    ],
    "user": "d9JSx1SYsmLzNRbb",
    "password": "gUFneDm2DtkedlVeViObYJIvrPAf2kJA"
  }
}
```

{: codeblock}

环境变量的内容是相同的，与您用于连接到 {{site.data.keyword.messagehub}} 的 API 无关。您的
{{site.data.keyword.Bluemix_notm}} 应用程序根据使用的接口从 VCAP_SERVICES 环境变量选择相应的凭证。
 
VCAP_SERVICES 中仅列出前 5 个代理程序。如果您具有 5 个以上的代理程序，请使用 Kafka 客户机来检索其他代理程序的详细信息。 


### 使用 IBM Cloud 控制台获取凭证并进行连接
{: #connect_standard_cf_console }

1. 确保您位于计划使用的 Cloud Foundry 组织和空间中。
2. 在仪表板上找到您的 Cloud Foundry 应用程序。如果您还没有 Cloud Foundry 应用程序，您可以通过单击**创建资源**按钮创建一个。
3. 单击应用程序磁贴。
4. 单击**连接**。
5. 单击**创建连接**。
6. 选择要绑定到的 {{site.data.keyword.messagehub}} 服务磁贴，并单击**连接**。您可能需要重新编译打包您的应用程序，以使更改生效。
7. 单击左侧的**运行时**选项卡，并选择当中的**环境变量**选项卡。您现在可以验证 VCAP_SERVICES 信息，现在您的应用程序可以将此信息作为环境变量进行访问。 


### 使用 IBM Cloud CLI 获取凭证  
{: #connect_standard_cf_cli }

<ol>
<li>确保您位于计划使用的 Cloud Foundry 组织和空间中。您可以通过运行以下命令进行交互式导航：<br/>
<code>ibmcloud target --cf</code>
</li>
<li>查找您的应用程序：<br/> <code>ibmcloud app list</code> <br/>
</br>
如果您具有清单文件，您可以通过运行以下命令来创建新的应用程序：</br>
<code>ibmcloud app push</code>
</li>
<li>查找您的服务：</br> 
<code>ibmcloud service list</code>
</li>
<li>将您的应用程序绑定到服务：</br>
<code>ibmcloud service bind <var class="keyword varname">your_app_name</var> <var class="keyword varname">your_service_name</var></code>
</li>
<li>通过运行以下命令验证 VCAP_SERVICES 环境变量在应用程序运行时中是否可用：</br> 
 <code>ibmcloud app env <var class="keyword varname">your_app_name</var></code>。  </li>
<li>将这些凭证传递给应用程序。指定 <code>token</code> 作为用户名，<var class="keyword varname">api_key</var> 作为密码。使用冒号分隔 <code>token</code> 和 <var class="keyword varname">api_key</var>。有关更多信息，请参阅[配置客户机](/docs/services/EventStreams/eventstreams063.html)。<p>您可能需要重新编译打包您的应用程序，以使更改生效。</p></li>
</ol>

## 外部应用程序（标准套餐）
{: #connect_standard_external}

对于 Cloud Foundry 之外运行的应用程序，创建服务密钥将生成凭证。当您获取服务密钥时，请使用所选方法将密钥详细信息手动传递给应用程序。

### 使用 IBM Cloud 控制台获取凭证 
{: #connect_standard_external_console}

1. 确保您位于计划使用的 Cloud Foundry 组织和空间中。
2. 在仪表板上找到您的 Cloud Foundry {{site.data.keyword.messagehub}} 服务。
3. 单击服务磁贴。
4. 单击**服务凭证**。
5. 单击**新建凭证**。
6. 输入新凭证的详细信息，例如名称，并单击**添加**。凭证列表中将显示新的凭证。
7. 通过**查看凭证**单击此凭证，以采用 JSON 格式显示详细信息。
8. 将这些凭证传递给应用程序。指定 <code>token</code> 作为用户名，<var class="keyword varname">api_key</var> 作为密码。使用冒号分隔 <code>token</code> 和 <var class="keyword varname">api_key</var>。有关更多信息，请参阅[配置客户机](/docs/services/EventStreams/eventstreams063.html)。

### 使用 IBM Cloud CLI 获取凭证  
{: #connect_standard_external_cli }

<ol>
<li>确保您位于计划使用的 Cloud Foundry 组织和空间中。您可以通过运行以下命令进行交互式导航：<br>
<code>ibmcloud target --cf</code>
</li>
<li>查找您的服务：<br>
<code>ibmcloud service list</code>
</li>
<li>您可以创建服务密钥：<br>
<code>ibmcloud service key-create <var class="keyword varname">your_service_name</var> <var class="keyword varname">new_service_key_name</var></code><br>
<br/>
或者使用现有的服务密钥：<br/>
<code>ibmcloud service keys <var class="keyword varname">your_service_name</var></code> 
</li>
<li>获取密钥的详细信息：</br>
<code>ibmcloud service key-show <var class="keyword varname">your_service_name</var> <var class="keyword varname">service _key_name</var></code></br>
这将返回 JSON 格式的服务密钥详细信息。</li>
<li>将这些凭证传递给应用程序。指定 <code>token</code> 作为用户名，<var class="keyword varname">api_key</var> 作为密码。使用冒号分隔 <code>token</code> 和 <var class="keyword varname">api_key</var>。有关更多信息，请参阅[配置客户机](/docs/services/EventStreams/eventstreams063.html)。</li>
</ol>

 
## 企业套餐概述
{: #connect_enterprise}

使用企业套餐供应的服务将分组到仪表板中标题**服务**下。企业套餐[支持 IAM ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/docs/iam/quickstart.html#getstarted){:new_window}。您不需要了解 IAM 即可入门，但是如果您想要保护 {{site.data.keyword.messagehub}} 服务，建议您了解一些相关知识。有关更多信息，请参阅
[保护您的 {{site.data.keyword.messagehub}} 资源](/docs/services/EventStreams/eventstreams124.html)

完成以下步骤，以绑定您的应用程序并获取服务的服务密钥。要获得授权以创建主题，您的应用程序或服务密钥必须具有管理员访问角色。

要连接应用程序，使用的方法取决于该应用程序的部署位置，即在 Cloud Foundry 内部还是外部，例如在 Kubernetes 服务中。

## 企业套餐中的 Cloud Foundry 应用程序
{: #connect_enterprise_cf}

您的应用程序必须绑定到 {{site.data.keyword.messagehub}} 服务实例。要将 Cloud Foundry 应用程序绑定到非 Cloud Foundry 服务，请先创建 Cloud Foundry 服务别名，然后在绑定时从 Cloud Foundry 应用程序引用此别名。

绑定后，连接详细信息将通过 VCAP_SERVICES 环境变量以 JSON 格式提供给应用程序。您可以使用 IBM Cloud 控制台或 IBM Cloud CLI 绑定应用程序和服务。

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
<li>找到您的应用程序：</br>
<code>ibmcloud app list</code><br/>
<br/>
如果您具有清单文件，您可以通过运行以下命令来创建新的应用程序：<br/>
<code>ibmcloud app push</code><br/>
<br/>
因为应用程序未绑定到 {{site.data.keyword.messagehub}}，所以应用程序无法建立连接。因此，建议您使用 <code>--no-start</code> 参数推送应用程序，以避免不必要的连接故障。</li>
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
<li>将这些凭证传递给应用程序。指定 <code>token</code> 作为用户名，<var class="keyword varname">api_key</var> 作为密码。使用冒号分隔 <code>token</code> 和 <var class="keyword varname">api_key</var>。有关更多信息，请参阅[配置客户机](/docs/services/EventStreams/eventstreams063.html)。<p>您可能需要重新编译打包您的应用程序，以使更改生效。</p></li>
</ol>


## 外部应用程序（企业套餐）
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
7. 将这些凭证传递给应用程序。指定 <code>token</code> 作为用户名，<var class="keyword varname">api_key</var> 作为密码。使用冒号分隔 <code>token</code> 和 <var class="keyword varname">api_key</var>。有关更多信息，请参阅[配置客户机](/docs/services/EventStreams/eventstreams063.html)。
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
<li>将这些凭证传递给应用程序。指定 <code>token</code> 作为用户名，<var class="keyword varname">api_key</var> 作为密码。使用冒号分隔 <code>token</code> 和 <var class="keyword varname">api_key</var>。有关更多信息，请参阅[配置客户机](/docs/services/EventStreams/eventstreams063.html)。<p>确保您的应用程序会解析这些详细信息。</p></li>
</ol>

## 后续步骤
{: #after_connecting}

现在，您有了连接和凭证信息，可以选择一个 {{site.data.keyword.messagehub}} 客户机。您的选择取决于您的套餐。

* 如果您使用的是标准套餐，请参阅[在三个 API 中进行选择](/docs/services/EventStreams/eventstreams087.html)，以获取有关选择哪个客户机以及如何连接的信息。
* 如果您使用的是企业套餐，请参阅[使用 Kafka API](/docs/services/EventStreams/eventstreams050.html)。

	如果您使用企业套餐，内部 Kafka <code>__consumer_offsets</code> 主题将以只读方式显示给您。强烈建议您不要尝试以任何方式管理主题。 

<!--
Charlie said:

"Add some info describing how to take the information made available from above e.g. like the info in the Connecting a client to the Kafka API section of the alpha docs on stage 1? https://console.stage1.bluemix.net/docs/services/EventStreams/eventstreams122.html#alpha_about "
-->







 















