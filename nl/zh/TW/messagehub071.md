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

# VCAP_SERVICES 環境變數
{: #vcap}

將應用程式連結至 {{site.data.keyword.messagehub}} 服務時，服務的詳細資料會以 JSON 格式儲存在應用程式的 VCAP_SERVICES 環境變數中。範例如下：

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

不論您用來連接 {{site.data.keyword.messagehub}} 的 API 為何，環境變數的內容都相同。您的 {{site.data.keyword.Bluemix_notm}} 應用程式會視使用的介面而定，從 VCAP_SERVICES 環境變數中選取適當的認證。
 
只有前五個分配管理系統會列在 VCAP_SERVICES 中。如果您有超過五個分配管理系統，請使用 Kafka 用戶端來擷取其他分配管理系統的詳細資料。 
