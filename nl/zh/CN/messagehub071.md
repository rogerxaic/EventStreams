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

# VCAP_SERVICES 环境变量
{: #vcap}

当您的应用程序绑定到 {{site.data.keyword.messagehub}} 服务时，服务的详细信息都会以 JSON 格式存储在应用程序的 VCAP_SERVICES 环境变量中。如下所示：

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
