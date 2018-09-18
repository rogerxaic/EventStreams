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

# VCAP_SERVICES 환경 변수
{: #vcap}

애플리케이션이 {{site.data.keyword.messagehub}} 서비스에 바인드되면 서비스의 세부사항이 앱에 대한 VCAP_SERVICES 환경 변수에 JSON 형식으로 저장됩니다. 다음은 예제입니다.

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

환경 변수의 컨텐츠는 {{site.data.keyword.messagehub}}에 연결하기 위해 사용하는 API에 관계없이 동일합니다. 사용자의 {{site.data.keyword.Bluemix_notm}} 앱은 사용 중인 인터페이스에 따라 VCAP_SERVICES 환경 변수에서 적합한 신임 정보를 선택합니다.
 
VCAP_SERVICES에는 처음 다섯 개의 브로커만 나열됩니다. 다섯 개보다 많은 브로커가 있는 경우에는 Kafka 클라이언트를 사용하여 다른 브로커의 세부사항을 검색하십시오. 
