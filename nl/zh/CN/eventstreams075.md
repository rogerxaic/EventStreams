---

copyright:
  years: 2015, 2019
lastupdated: "2018-11-20"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 使用 MQ Light API 
{: #mql_using}

**MQ Light API 仅在标准套餐中提供。**
<br/>

{{site.data.keyword.mql}} API 旨在实现与旧版 {{site.data.keyword.mql}} 服务的向后兼容性。
该 API 为 Java&trade;、Node.js、Python 和 Ruby 提供了基于 AMQP 的消息传递接口。
{:shortdesc}

<!-- 02/07/18 - removing words to help deprecate MQ Light
In most cases, {{site.data.keyword.messagehub}} is best used with a Kafka client. The {{site.data.keyword.mql}} API is simple to learn but has very limited scalability and does not offer interoperability with other {{site.data.keyword.messagehub}} APIs.
The {{site.data.keyword.mql}} API is available in the following {{site.data.keyword.Bluemix_short}} regions only: US South, United Kingdom, and Sydney. The {{site.data.keyword.mql}} API not available in the Germany region or in {{site.data.keyword.Bluemix_notm}} Dedicated.
-->

## 什么是 MQ Light API，它有何独到之处？
{: #mqlight}

<!-- 30/10/18: info was in eventstreams106.md, moved because of doc app changes -->

{{site.data.keyword.mql}} API 提供的消息传递抽象级别高于 Kafka。


选择使用 Kafka 客户机还是 {{site.data.keyword.mql}} API 取决于要构建的消息传递拓扑：

* 使用 Kafka 时，您可使用数量不多的主题，并且每个主题可以有多个分区，以实现额外的可伸缩性。可以通过使用者组在使用者之间共享消息，但每个使用者必须能够跟上为其分配的分区的消息速率。
* 使用 {{site.data.keyword.mql}} API 时，您可以使用的主题数量要多得多，并且主题名称是分层的（例如：<code>&lsquo;/sports/football&rsquo;</code> 和 <code>&lsquo;/sports/tiddlywinks&rsquo;</code>）。  

{{site.data.keyword.mql}} API 中的主题与 Kafka 主题不同。即，{{site.data.keyword.mql}} API 使用名为“MQLight”的单个 Kafka 主题，并且使用 {{site.data.keyword.mql}} API 发送和接收的所有消息都经过这一个 Kafka 主题。

{{site.data.keyword.mql}} 仅在以下 {{site.data.keyword.Bluemix_notm}} 位置（区域）中可用：达拉斯 (us-south)、伦敦 (eu-gb) 和悉尼 (au-syd)。MQ Light API 在法兰克福 (eu-de) 位置或 {{site.data.keyword.Bluemix_notm}} Dedicated 中不可用。

有关在 API 之间进行选择的更多信息，请参阅[在三个 API 之间选择](/docs/services/EventStreams/eventstreams087.html)。


## 将 MQ Light API 与 {{site.data.keyword.messagehub}} 配合使用时需要满足哪些需求？
{: #mql_reqs}

<!-- 30/10/18: info was in eventstreams098.md, moved because of doc app changes -->

要将 {{site.data.keyword.mql}} API 与 {{site.data.keyword.messagehub}} 配合使用，需要满足以下需求： 

**必须显式创建名为“MQLight”的 Kafka 主题后才能使用该 API，因为所有消息都将经过“MQLight”主题。此主题必须具有单个分区。创建此主题后，即可将 MQ Light API 用于您的服务实例。当您使用 MQ Light API 中使用的主题时会自动创建这些主题，但是所有消息实际上位于单个“MQLight”Kafka 主题中。** 

MQ Light API 使用“MQLight”主题来存储其消息数据，以及与其他 Kafka 客户机进行交互。请注意，创建此主题时，会按服务支付套餐中概述的标准费率收费。

要禁用 MQ Light API，请删除“MQLight”主题。请注意，删除该主题时，将销毁所有数据。

<!-- 12/11/18: following info was in eventstreams079.md, moved because of doc app changes -->

## 如何连接和认证
{: #mql_connect}

要将应用程序连接到服务，应用程序必须使用 [VCAP_SERVICES 环境变量](/docs/services/EventStreams/eventstreams127.html)中的 <code>user</code>、
<code>password</code> 和 <code>mqlight_lookup_url</code> 详细信息。使用与所选语言对应的以下指导信息：


**对于 Java**

如果将 &lsquo;null&rsquo; 指定为 create() 调用的 endpointService 参数，这将指示客户机从 VCAP_SERVICES 读取 <code>user</code>、<code>password</code> 和 
<code>mqlight_lookup_url</code> 详细信息：



<pre>
<code>NonBlockingClient.create(null, new NonBlockingClientAdapter<Void>() {
    public void onStarted(NonBlockingClient client, Void context) {
        client.send("my/topic", "Hello World!", null);
    }
}, null);</code>
</pre>
{:codeblock}

<br>

**对于 Node.js**

在 VCAP_SERVICES 中检索 <code>user</code>、 <code>password</code> 和 
<code>mqlight_lookup_url</code> 详细信息，并将其用于创建客户机，如下所示：



<pre>
<code>var services = JSON.parse(process.env.VCAP_SERVICES);
mqlightService = services['messagehub'][0];
opts.service = mqlightService.credentials.mqlight_lookup_url;
opts.user = mqlightService.credentials.user;
opts.password = mqlightService.credentials.password;
var mqlightClient = mqlight.createClient(opts, function(err) {
...</code>
</pre>
{:codeblock}

<br>

**对于 Ruby**

在 VCAP_SERVICES 中检索 <code>user</code>、 <code>password</code> 和 
<code>mqlight_lookup_url</code> 详细信息，并将其用于创建客户机，如下所示：
<pre>
<code>vcap_services = JSON.parse(ENV['VCAP_SERVICES'])
conn_details = vcap_services['messagehub']
credentials = conn_details.first['credentials']
service = credentials['mqlight_lookup_url']
opts[:user] = credentials['user']
opts[:password] = credentials['password']
set :client, Mqlight::BlockingClient.new(service, opts)
...</code>
</pre>
{:codeblock}

<br>

**对于 Python**

在 VCAP_SERVICES 中检索 <code>user</code>、 <code>password</code> 和 
<code>mqlight_lookup_url</code> 详细信息，并将其用于创建客户机，如下所示：
<pre>
<code>vcap_services = json.loads(os.environ.get('VCAP_SERVICES'))
conn_details = vcap_services['messagehub'][0]
service = str(conn_details['credentials']['mqlight_lookup_url'])
security_options = {
      'user': str(conn_details['credentials']['user']),
      'password': str(conn_details['credentials']['password'])
}
client = mqlight.Client(service=service, 
                        security_options=security_options,
                        on_started=on_started)</code>
</pre>
{:codeblock}

<br>

有关 {{site.data.keyword.mql}} API 的更多信息，请参阅 [{{site.data.keyword.mql}} developerWorks&reg; 站点 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/messaging/mq-light/){:new_window}。


<!-- 14/11/18: info was in eventstreams080.md, moved because of doc app changes -->

## 如何连接现有 MQ Light 应用程序
{: #mql_exist_apps}


可以将当前针对 {{site.data.keyword.IBM_notm}} MQ 或 {{site.data.keyword.mql}}
{{site.data.keyword.Bluemix_notm}} 服务运行的现有应用程序连接到该服务。应用程序会以同样方式继续工作。


要连接现有应用程序，请完成以下检查：

* 确保应用程序使用的是适合您语言的最新可用的 {{site.data.keyword.mql}} API 客户机版本。
* 检查从 VCAP_SERVICES 中提取的连接详细信息是否引用的是 <code>messagehub</code> 服务类型，在 <code>credentials.user</code> 属性（而不是 <code>credentials.username</code> 属性）中检索连接用户名，以及在 <code>credentials.mqlight_lookup_url</code> 属性（而不是 <code>credentials.connectionLookupURI</code> 属性）中检索连接查找 URL。有关更多信息，请参阅 [VCAP_SERVICES 环境变量](/docs/services/EventStreams/eventstreams127.html)。

	请注意，如果使用的是 Java&trade; 客户机，并且将“null”指定为 create() 调用中的 endpointService 参数，以便客户机自行检索信息，那么此步骤已完成。
	
* 应用程序必须支持 TLS V1.2 连接。VCAP_SERVICES 不再包含用于创建非 TLS 连接的 <code>credentials.nonTLSConnectionLookupURI</code> 属性。

此外，还应该注意以下信息：

* 消息限制与 {{site.data.keyword.messagehub}} 一致，但可能与支持 {{site.data.keyword.mql}} API 的其他服务器不同。有关更多信息，请参阅[最大限制](/docs/services/EventStreams/eventstreams075.html#max_limits)。
* 不支持 JMS。

<!-- 15/11/18: info was in eventstreams081.md, moved because of doc app changes -->

## 在 MQ Light API 和 Kafka 或 Kafka REST API 之间交换消息
{: #mql_exchange}

{{site.data.keyword.mql}} 消息存储在名为“MQLight”的单个底层 Kafka 主题中并经过编码，如下表中所详述。其他 API 类型（如 Kafka 或 Kafka REST）也可使用此编码通过 {{site.data.keyword.mql}} API 来与应用程序交换消息。

### Kafka 消息格式
{: #kafka_format notoc}

<table border='1'>
<caption>表 1. Kafka 消息格式</caption>
  <tr>
    <th> 键</th>
    <th> 值</th>
  </tr>
  <tr>
    <td> 可选（API 不予使用）<p></p>
	</td>
    <td>**1 字节**
	<p>		     MQ Light API 视觉焦点，始终为 0xFA。</p>
    <p><var class="keyword varname">**n**</var> **字节**</p>
    <p>		    AMQP 编码的消息（格式基于 AMQP 有线格式）。</p></td>
  </tr>
</table>


<!-- 15/11/18: info was in eventstreams082.md, moved because of doc app changes -->

## MQ Light API 样本
{: #mql_samples}

如果还没有应用程序，请通过其中一个样本来试用 {{site.data.keyword.mql}} API。


样本应用程序实际上由两个简单应用程序组成：一个 Web 前端和一个后端；前者用于将消息发送到后端，后者用于处理消息，将词语首字母转换为大写，然后将消息返回给前端。此样本显示了如何使用 {{site.data.keyword.mql}} API 让应用程序彼此进行对话。还可以使用 {{site.data.keyword.mql}} API 来执行工作程序分流；这是构建可扩展、松耦合的分布式应用程序所需的关键功能。

您可以在 [event-streams-samples GitHub 项目 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-messaging/event-streams-samples/tree/master/mqlight){:new_window} 中找到样本代码。


<!-- 15/11/18: info was in eventstreams083.md, moved because of doc app changes -->

## 最大限制
{: #max_limits}

对于 {{site.data.keyword.mql}} API，将强制实施以下限制：


* 可以存储的最大数据量与支付套餐的单个 Kafka 分区一致（通常为 1 GB）。如果超出此数据限制，将在发送新消息时，除去分区中的最旧消息。
* 消息存储的最长时间与支付套餐的单个 Kafka 分区一致（通常为 24 小时）。无法检索早于此时间段的消息。
* 消息的最大大小（不包括头）为 1 MB。
* 一次可连接的最大客户机数为 25 个。
* 一次可以处于活动状态的最大目标数为 25 个。活动目标的定义如下：
  - 当前连接或没有连接客户机且 TimeToLive > 0 的目标。
  - 连接了客户机且 TimeToLive = 0（缺省值）的目标。 
  <p>客户机之间共享的目标计为一个目标。</p>
