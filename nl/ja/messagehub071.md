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

# VCAP_SERVICES 環境変数
{: #vcap}

アプリケーションが {{site.data.keyword.messagehub}} サービスにバインドされると、そのサービスの詳細が JSON 形式でアプリケーションの VCAP_SERVICES 環境変数に格納されます。 以下に例を示します。

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

{{site.data.keyword.messagehub}} への接続に使用する API に関係なく、環境変数の内容は同じです。 {{site.data.keyword.Bluemix_notm}} アプリケーションは、使用しているインターフェースに応じて、VCAP_SERVICES 環境変数から適切な資格情報を選択します。
 
VCAP_SERVICES には、最初の 5 つのブローカーのみがリストされます。 5 個を超えるブローカーがある場合、他のブローカーの詳細を取得するには Kafka クライアントを使用してください。 
