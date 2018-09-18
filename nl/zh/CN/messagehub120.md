---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Alpha 程序入门
{: #alpha_program}

通过 Alpha 程序，您可以及早使用 {{site.data.keyword.messagehub}} 服务的下一个版本。 

完成以下步骤将应用程序与 {{site.data.keyword.messagehub}} Alpha 一起运行：


## 创建 {{site.data.keyword.messagehub}} 服务
{: alpha_create}


  1. 单击 **Message Hub vNext - 生产** 磁贴，这是[目录 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://console.stage1.bluemix.net/catalog/labs/?search=vnext) 中的试验性服务。</li>

  2. 创建 {{site.data.keyword.messagehub}} 服务。在<code>高端</code>价格套餐中，此服务提供单租户集群，通常需要 1-3 个小时才能完成供应。
 


## 使用控制台选项获取凭证：创建和连接测试应用程序
{: alpha_app}

您需要凭证才能使用 {{site.data.keyword.messagehub}}。
如果您还没有可以使用的应用程序，请创建测试应用程序，并且使用该应用程序所用的凭证来连接到 {{site.data.keyword.messagehub}}。例如，对测试应用程序使用 **SDK for Node.js** 服务。 

  1. 导航至[目录 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://console.stage1.bluemix.net/catalog/starters/sdk-for-nodejs) 中的
**SDK for Node.js** 磁贴。
   
  输入应用程序名称之前，请确保选择的区域是美国南部。创建应用程序。

  2. 应用程序运行后，单击左侧的**连接**选项卡。

  3. 单击**创建连接**按钮。

  4. 从现有的兼容服务列表中选择新的 {{site.data.keyword.messagehub}} 服务，然后单击**连接**按钮。

  5. 在**连接支持 IAM 的服务**窗口中，接受缺省值，然后单击**连接**。确保已供应 {{site.data.keyword.messagehub}} 服务，这样您才能连接该服务。

  6. 单击左侧的**运行时**选项卡，并选择当中的**环境变量**选项卡。在 **VCAP_SERVICES** 部分中，找到 <code>kafka_admin_url</code>、<code>apikey</code> 和 <code>kafka_brokers_sasl</code> 信息，您需要这些信息才能发送消息。
  
## 使用命令行选项获取凭证
或者，您可以使用命令行来获取所需的凭证。请完成以下步骤：

  1. 在 [{{site.data.keyword.Bluemix_notm}} CLI ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/cli/index.html#overview) 中安装 {{site.data.keyword.Bluemix_notm}} 命令行工具。
  
  2. 登录到 {{site.data.keyword.Bluemix_notm}} CLI。
  
  3. 使用 {{site.data.keyword.Bluemix_notm}} CLI 通过以下命令以管理员角色创建服务密钥：
  ```
  bx resource service-key-create <NAME> Manager --instance-name <MESSAGEHUB_SERVICE_INSTANCE_NAME>
  ```
  4. 输出包含 apikey、管理 REST 端点 URL 以及代理程序列表。您可以通过运行以下命令，再次查看这些信息：
  ```
  bx resource service-key <NAME>
  ```

## 创建 {{site.data.keyword.messagehub}} 主题并发送消息

可以使用 CURL 命令来创建主题，然后使用 kafkacat 工具来生成和使用消息。 

对于每个命令，将 APIKEY 和 KAFKA_ADMIN_URL 替换为 VCAP_SERVICES 环境变量中的值。

  1. 在命令行中，使用以下 CURL 命令来创建 {{site.data.keyword.messagehub}} 主题：
  
    ```
    curl -i -X POST -H "Content-Type: application/json" -H "X-Auth-Token: APIKEY" --data '{ "name": "newtop"}' KAFKA_ADMIN_URL/admin/topics
    ```
    {: codeblock}

  2. 安装 [kafkacat 工具 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/edenhill/kafkacat#install)，该工具对于快速测试 Kafka 非常有用。
  
  3. 要运行后续命令，您需要以下信息：
  
    * 凭证中返回的代理程序列表：`kafka_brokers_sasl`。代理程序列表必须是 kafkacat 的逗号分隔列表。VCAP_SERVICES 中仅列出前 5 个代理程序。如果您具有 5 个以上的代理程序，请使用 Kafka 客户机来检索其他代理程序的详细信息。 
  
    * <code>apikey</code>。<code>apikey</code> 的前 8 个字符构成 sasl.username，其余部分构成 sasl.password。
    * SSL 证书的位置。例如，在 Ubuntu 上，SSL_CERTS_DIRECTORY 为 <code>/etc/ssl/certs/</code>
  
  4. 通过运行类似下面的命令生成一些消息：
  ```
  kafkacat -X "security.protocol=sasl_ssl" -X 'sasl.mechanisms=PLAIN' -X 'sasl.username=<FIRST_8_CHARS_FROM_APIKEY>' -X 'sasl.password=<REMAINING_CHARS_FROM_APIKEY>' -X "ssl.ca.location=<SSL_CERTS_DIRECTORY>" -b <BROKERS_LIST> -P -t <TOPIC_NAME>
    ```
		
	<br/>
运行该命令后，可以在生产者终端输入一些文本，如 <code>HelloWorld</code>。
  
  5. 通过运行类似下面的命令使用消息：
  ```
  kafkacat -X "security.protocol=sasl_ssl" -X 'sasl.mechanisms=PLAIN' -X 'sasl.username=<FIRST_8_CHARS_FROM_APIKEY>' -X 'sasl.password=<REMAINING_CHARS_FROM_APIKEY>' -X "ssl.ca.location=<SSL_CERTS_DIRECTORY>" -b <BROKERS_LIST> -C -t <TOPIC_NAME> -f 'Topic %t [%p] at offset %o: key %k: %s\n'
  ```
	
	<br/>
  在使用者终端上，您将看到 <code>HelloWorld</code>。

