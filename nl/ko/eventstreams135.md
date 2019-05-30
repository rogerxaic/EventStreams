---

copyright:
  years: 2015, 2019
lastupdated: "2018-11-07"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 클래식 플랜을 사용하여 {{site.data.keyword.messagehub}}에 연결 
{: #connecting_classic}

{{site.data.keyword.messagehub}}에 연결하는 방법은 연결을 시작하는 위치(Cloud Foundry 애플리케이션 또는 기타 외부 클라이언트)에 따라 달라집니다. {{site.data.keyword.messagehub}}의 API에 연결하려면 다음과 같은 두 가지 정보를 수집해야 합니다.
{: shortdesc}

* API용 엔드포인트 URL
* 인증을 위한 인증 정보

세부사항을 확인하려면 다음 정보를 읽어 보십시오. 단계는 약간 다를 수 있으므로 인스턴스에 맞는 적절한 단계를 완료했는지 확인하십시오.

## {{site.data.keyword.messagehub}} 인스턴스 프로비저닝

전제조건으로 먼저 클래식 플랜에 대한 {{site.data.keyword.messagehub}} 서비스 인스턴스를 프로비저닝해야 합니다. 그런 다음, 다음 태스크를 완료하여 {{site.data.keyword.messagehub}} API 연결 세부사항을 얻으십시오.

## 클래식 플랜 개요
{: #connect_classic_plan}

클래식 플랜을 사용하여 프로비저닝되는 서비스는 Cloud Foundry 서비스입니다. 이 서비스는 Cloud Foundry 조직 및 영역에 배치되고 대시보드에서 **Cloud Foundry 서비스** 표제 아래에 그룹화됩니다. 애플리케이션 연결에 사용하는 방법은 애플리케이션이 배치된 위치, 즉 Cloud Foundry 내부인지 또는 Cloud Foundry 외부(예: Kubernetes 서비스)에 배치되었는지에 따라 달라집니다.


## 클래식 플랜의 Cloud Foundry 애플리케이션
{: #connect_classic_cf_plan}

Cloud Foundry 내부에서 실행되는 앱의 경우 앱을 {{site.data.keyword.messagehub}} 서비스 인스턴스에 바인드하십시오. 바인드되면 연결 세부사항이 VCAP_SERVICES 환경 변수에서 JSON 형식으로 앱에 제공됩니다. IBM Cloud 콘솔 또는 IBM Cloud CLI를 사용하여 앱 및 서비스를 바인드할 수 있습니다.

다음은 VCAP_SERVICES의 예제입니다.

```
{
  "credentials": {
    "mqlight_lookup_url": "https://mqlight-lookup.messagehub.services.us-south.bluemix.net/Lookup?serviceId=584e8436-e7f5-43db-96ac-2864fccae5ae",
    "api_key": <YOUR_API_KEY>,
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

환경 변수의 컨텐츠는 {{site.data.keyword.messagehub}}에 연결하기 위해 사용하는 API에 관계없이 동일합니다. 사용자의 {{site.data.keyword.Bluemix_notm}} 앱은 사용 중인 인터페이스에 따라 VCAP_SERVICES 환경 변수에서 적합한 인증 정보를 선택합니다.
 
VCAP_SERVICES에는 처음 다섯 개의 브로커만 나열됩니다. 다섯 개보다 많은 브로커가 있는 경우에는 Kafka 클라이언트를 사용하여 다른 브로커의 세부사항을 검색하십시오. 


### IBM Cloud 콘솔을 사용하여 인증 정보 가져오기 및 연결
{: #connect_classic_plan_cf_console }

1. 원하는 Cloud Foundry 조직 및 영역에 있는지 확인하십시오.
2. 대시보드에서 Cloud Foundry 애플리케이션을 찾으십시오. 아직 Cloud Foundry 애플리케이션이 없으면 **리소스 작성** 단추를 클릭하여 작성할 수 있습니다.
3. 애플리케이션 타일을 클릭하십시오.
4. **연결**을 클릭하십시오.
5. **연결 작성**을 클릭하십시오.
6. 바인드할 {{site.data.keyword.messagehub}} 서비스 타일을 선택하고 **연결**을 클릭하십시오. 변경사항을 적용하기 위해 애플리케이션을 다시 스테이징해야 할 수 있습니다.
7. 왼쪽의 **런타임** 탭을 클릭하고 가운데에 있는 **환경 변수** 탭을 선택하십시오. 이제 VCAP_SERVICES 정보를 확인할 수 있고 애플리케이션이 환경 변수로 이 정보에 액세스할 수 있습니다. 


### IBM Cloud CLI를 사용하여 인증 정보 가져오기 
{: #connect_classic_plan_cf_cli }

<ol>
<li>원하는 Cloud Foundry 조직 및 영역에 있는지 확인하십시오. 다음 명령을 실행하여 대화식으로 탐색할 수 있습니다.<br/>
<code>ibmcloud target --cf</code>
</li>
<li>앱 찾기:<br/> <code>ibmcloud app list</code> <br/>
</br>
Manifest 파일이 있으면 다음을 실행하여 새 앱을 작성할 수 있습니다.</br>
<code>ibmcloud app push</code>
</li>
<li>서비스 찾기:</br>
<code>ibmcloud service list</code>
</li>
<li>서비스에 앱 바인드:</br>
<code>ibmcloud service bind <var class="keyword varname">your_app_name</var> <var class="keyword varname">your_service_name</var></code>
</li>
<li>다음을 실행하여 VCAP_SERVICES 환경 변수가 애플리케이션 런타임에서 사용 가능한지 확인하십시오.</br>
 <code>ibmcloud app env <var class="keyword varname">your_app_name</var></code>
</li>
<li>인증 정보를 애플리케이션에 전달하십시오. 사용자 이름으로 <code>token</code>을 지정하고 비밀번호로 <var class="keyword varname">api_key</var>를 지정하십시오. 콜론으로 <code>token</code>과 <var class="keyword varname">api_key</var>를 구분하십시오. 자세한 정보는 [클라이언트에 연결](/docs/services/EventStreams?topic=eventstreams-kafka_connect)을 참조하십시오.
<p>변경사항을 적용하기 위해 애플리케이션을 다시 스테이징해야 할 수 있습니다.</p></li>
</ol>

## 클래식 플랜의 외부 애플리케이션
{: #connect_classic_plan_external}

Cloud Foundry 외부에서 실행 중인 애플리케이션의 경우 인증 정보는 서비스 키를 작성하여 작성됩니다. 서비스 키를 가져온 경우 선택된 방법을 사용하여 키의 세부사항을 애플리케이션에 수동으로 전달하십시오.

### IBM Cloud 콘솔을 사용하여 인증 정보 가져오기
{: #connect_classic_plan_external_console}

1. 원하는 Cloud Foundry 조직 및 영역에 있는지 확인하십시오.
2. 대시보드에서 Cloud Foundry {{site.data.keyword.messagehub}} 서비스를 찾으십시오.
3. 서비스 타일을 클릭하십시오.
4. **서비스 인증 정보**를 클릭하십시오.
5. **새 인증 정보**를 클릭하십시오.
6. 이름과 같은 새 인증 정보에 대한 세부사항을 입력하고 **추가**를 클릭하십시오. 새 인증 정보가 인증 정보 목록에 표시됩니다.
7. JSON 형식으로 세부사항을 표시하려면 **인증 정보 보기**를 사용하여 이 인증 정보를 클릭하십시오.
8. 인증 정보를 애플리케이션에 전달하십시오. 사용자 이름으로 <code>token</code>을 지정하고 비밀번호로 <var class="keyword varname">api_key</var>를 지정하십시오. 콜론으로 <code>token</code>과 <var class="keyword varname">api_key</var>를 구분하십시오. 자세한 정보는 [클라이언트에 연결](/docs/services/EventStreams?topic=eventstreams-kafka_connect)을 참조하십시오.

### IBM Cloud CLI를 사용하여 인증 정보 가져오기 
{: #connect_classic_plan_external_cli }

<ol>
<li>원하는 Cloud Foundry 조직 및 영역에 있는지 확인하십시오. 다음 명령을 실행하여 대화식으로 탐색할 수 있습니다.<br>
<code>ibmcloud target --cf</code>
</li>
<li>서비스 찾기:<br>
<code>ibmcloud service list</code>
</li>
<li>다음 명령을 실행하여 서비스 키를 작성할 수 있습니다.<br>
<code>ibmcloud service key-create <var class="keyword varname">your_service_name</var> <var class="keyword varname">new_service_key_name</var></code><br>
<br/>
또는 다음 명령을 실행하여 기존 서비스 키를 사용할 수 있습니다. <br/>
<code>ibmcloud service keys <var class="keyword varname">your_service_name</var></code>
</li>
<li>키에 대한 세부사항을 가져오십기:</br>
<code>ibmcloud service key-show <var class="keyword varname">your_service_name</var> <var class="keyword varname">service _key_name</var></code></br>
이를 통해 서비스 키 세부사항이 JSON 형식으로 리턴됩니다.</li>
<li>인증 정보를 애플리케이션에 전달하십시오. 사용자 이름으로 <code>token</code>을 지정하고 비밀번호로 <var class="keyword varname">api_key</var>를 지정하십시오. 콜론으로 <code>token</code>과 <var class="keyword varname">api_key</var>를 구분하십시오. 자세한 정보는 [클라이언트에 연결](/docs/services/EventStreams?topic=eventstreams-kafka_connect)을 참조하십시오.</li>
</ol>
 
## 다음에 수행할 작업
{: #after_connecting_next}

연결하여 인증 정보를 확보했으므로 이제 {{site.data.keyword.messagehub}} 클라이언트를 선택할 수 있습니다. 선택할 클라이언트와 연결 방법에 대한 정보는 [세 개의 API 중에서 선택](/docs/services/EventStreams?topic=eventstreams-choose_api_classic)을 참조하십시오.










 















