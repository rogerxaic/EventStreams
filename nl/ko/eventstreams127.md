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


# {{site.data.keyword.messagehub}}에 연결
{: #connecting}

{{site.data.keyword.messagehub}}에 연결하는 방법은 애플리케이션이 기본적으로 실행 중인지 아니면 Cloud Foundry 애플리케이션으로 실행 중인지에 따라 달라집니다. 그러나 두 경우 모두에 두 가지 정보가 필요합니다. 
{: shortdesc}

* API용 엔드포인트 URL
* 인증을 위한 인증 정보

세부사항을 확인하려면 다음 정보를 읽어 보십시오. 단계는 약간 다를 수 있으므로 인스턴스에 맞는 적절한 단계를 완료했는지 확인하십시오.

클래식 플랜을 사용 중인 경우 {{site.data.keyword.messagehub}}에 연결하는 방법에 대한 정보는 [클래식 플랜을 사용하여 연결](/docs/services/EventStreams?topic=eventstreams-connecting_classic)을 참조하십시오.


## 개요
{: #connect_enterprise}

표준 또는 엔터프라이즈 플랜을 사용하여 프로비저닝된 서비스는 대시보드에서 **서비스** 표제 아래에 그룹화됩니다. 이러한 플랜은 [IAM 사용 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/iam?topic=iam-getstarted#getstarted){:new_window} 플랜입니다. 시작하기 위해 IAM을 이해할 필요는 없지만 {{site.data.keyword.messagehub}} 서비스의 보안을 설정하려면 어느 정도 알고 있는 것이 좋습니다. 자세한 정보는 [{{site.data.keyword.messagehub}} 리소스에 대한 액세스 관리](/docs/services/EventStreams?topic=eventstreams-security)를 참조하십시오.

애플리케이션을 바인드하고 서비스에 대한 서비스 키를 가져오려면 다음 단계를 완료하십시오. 토픽 작성을 위한 권한이 부여되려면 애플리케이션 또는 서비스 키에 관리자 액세스 역할이 있어야 합니다.

애플리케이션에 연결하는 경우 사용하는 방법은 애플리케이션이 배치된 위치, 즉 Cloud Foundry 내부인지 또는 Cloud Foundry 외부(예: Kubernetes 서비스)에 배치되었는지에 따라 달라집니다.

## {{site.data.keyword.messagehub}} 인스턴스 프로비저닝
{: #provision_instance}

전제조건으로 먼저 표준 또는 엔터프라이즈 플랜 중 하나에 대한 {{site.data.keyword.messagehub}} 서비스 인스턴스를 프로비저닝해야 합니다. 그런 다음, 다음 태스크를 완료하여 {{site.data.keyword.messagehub}} API 연결 세부사항을 얻으십시오.

## 애플리케이션 연결 
{: #connect_enterprise_external}

Cloud Foundry 외부에서 실행 중인 애플리케이션의 경우 인증 정보는 서비스 키를 작성하여 작성됩니다. 서비스 키를 가져온 경우 선택된 방법을 사용하여 키의 세부사항을 애플리케이션에 수동으로 전달하십시오.

### IBM Cloud 콘솔을 사용하여 인증 정보 가져오기
{: #connect_enterprise_external_console}

1. 대시보드에서 {{site.data.keyword.messagehub}} 서비스를 찾으십시오.
2. 서비스 타일을 클릭하십시오.
3. **서비스 인증 정보**를 클릭하십시오.
4. **새 인증 정보**를 클릭하십시오. 
5. 이름 및 역할과 같은 새 인증 정보에 대한 세부사항을 완료한 후 **추가**를 클릭하십시오. 새 인증 정보가 인증 정보 목록에 표시됩니다.
6. JSON 형식으로 세부사항을 표시하려면 **인증 정보 보기**를 사용하여 이 인증 정보를 클릭하십시오.
7. 인증 정보를 애플리케이션에 전달하십시오. 사용자 이름으로 <code>token</code>을 지정하고 비밀번호로 <var class="keyword varname">api_key</var>를 지정하십시오. 콜론으로 <code>token</code>과 <var class="keyword varname">api_key</var>를 구분하십시오. 자세한 정보는 [Kafka API 클라이언트 구성](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_api_client)을 참조하십시오.
   <br/><br/>애플리케이션이 세부사항을 구문 분석하는지 확인하십시오.

### IBM Cloud CLI를 사용하여 인증 정보 가져오기
{: #connect_enterprise_external_cli}

<ol>
<li>서비스 찾기:<br/>
<code>ibmcloud resource service-instances</code></li>
<li>서비스 키 작성:<br/>
<code>ibmcloud resource service-key-create <var class="keyword varname">key_name</var> <var class="keyword varname">key_role</var> --instance-name <var class="keyword varname">your_service_name</var></code></li>
<li>서비스 키 인쇄:<br/>
<code>ibmcloud resource service-key <var class="keyword varname">key_name</var></code></li>
<li>인증 정보를 애플리케이션에 전달하십시오. 사용자 이름으로 <code>token</code>을 지정하고 비밀번호로 <var class="keyword varname">api_key</var>를 지정하십시오. 콜론으로 <code>token</code>과 <var class="keyword varname">api_key</var>를 구분하십시오. 자세한 정보는 [클라이언트에 연결](/docs/services/EventStreams?topic=eventstreams-kafka_connect)을 참조하십시오.
<p>애플리케이션이 세부사항을 구문 분석하는지 확인하십시오.</p></li>
</ol>

## Cloud Foundry 애플리케이션 연결
{: #connect_enterprise_cf}

애플리케이션은 {{site.data.keyword.messagehub}} 서비스 인스턴스에 바인드되어야 합니다. Cloud Foundry 애플리케이션을 비Cloud Foundry 서비스에 바인드하려면 먼저 Cloud Foundry 서비스 별명을 작성한 후 바인딩 시 Cloud Foundry 애플리케이션에서 이 별명을 참조하십시오. 

바인드되면 연결 세부사항이 VCAP_SERVICES 환경 변수를 사용하여 JSON 형식으로 애플리케이션에 제공됩니다. [IBM Cloud 콘솔](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_cf_console) 또는 [IBM Cloud CLI](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_cf_cli)를 사용하여 애플리케이션 및 서비스를 바인드할 수 있습니다.

### IBM Cloud 콘솔을 사용하여 애플리케이션 바인드
{: #connect_enterprise_cf_console}

1. 원하는 Cloud Foundry 조직 및 영역에 있는지 확인하십시오.
2. 대시보드에서 Cloud Foundry 애플리케이션을 찾거나 **리소스 작성** 단추를 클릭하여 애플리케이션을 작성하십시오.
3. 애플리케이션 타일을 클릭하십시오.
4. **연결**을 클릭하십시오.
5. **연결 작성**을 클릭하십시오.
6. 바인드할 {{site.data.keyword.messagehub}} 서비스 타일을 선택하고 **연결**을 클릭하십시오. 
7. 표시되는 **IAM 사용 서비스 연결** 창의 **연결을 위한 액세스 역할**에서 액세스 역할을 선택하고 **연결을 위한 서비스 ID** 목록에서 서비스 ID를 선택하십시오(자동 생성된 ID 수락 가능). **연결**을 클릭하십시오. 

  이렇게 하면 {{site.data.keyword.messagehub}} 서비스의 Cloud Foundry 서비스 별명이 작성된 후 애플리케이션이 이 별명에 바인드됩니다. 

  변경사항을 적용하려면 애플리케이션을 다시 스테이징하십시오.<br/>
8. 왼쪽의 **런타임** 탭을 클릭하고 가운데에 있는 **환경 변수** 탭을 선택하십시오. 이제 VCAP_SERVICES 정보를 확인할 수 있습니다. 이제 애플리케이션이 이러한 항목을 환경 변수로서 액세스할 수 있습니다. 
 

### IBM Cloud CLI를 사용하여 애플리케이션 바인드
{: #connect_enterprise_cf_cli}

<ol>
<li>원하는 Cloud Foundry 조직 및 영역에 있는지 확인하십시오. 다음 명령을 실행하여 대화식으로 탐색할 수 있습니다.<br/>
 <code>ibmcloud target --cf</code></li>
<li>앱 찾기:</br>
<code>ibmcloud app list</code><br/>
<br/>
Manifest 파일이 있으면 다음을 실행하여 새 앱을 작성할 수 있습니다.<br/>
<code>ibmcloud app push</code><br/>
<br/>
앱이 아직 {{site.data.keyword.messagehub}}에 바인드되지 않았으므로 앱이 연결을 설정할 수 없습니다. 그러므로 <code>--no-start</code> 매개변수를 통해 애플리케이션을 푸시하여 불필요한 연결 실패를 방지하는 것이 좋습니다.</li>
<li>서비스 찾기:</br>
<code>ibmcloud resource service-instances</code></li>
<li>Cloud Foundry 서비스 별명 작성:<br/>
<code>ibmcloud resource service-alias-create <var class="keyword varname">alias_name</var> --instance-name <var class="keyword varname">your_service_name</var></code></li>
<li>이전에 작성된 서비스 별명에 앱 바인드:<br/>
<code>ibmcloud service bind <var class="keyword varname">your_ app_name</var> <var class="keyword varname">alias_name</var></code><br/>
<br/>
또는 Manifest 파일을 업데이트하여 애플리케이션을 다시 푸시할 수 있습니다.</li>
<li>VCAP_SERVICES 환경 변수가 애플리케이션 런타임에서 사용 가능한지 확인하십시오.<br/>
<code>ibmcloud app env <var class="keyword varname">your_app_name</var></code></li>
<li>인증 정보를 애플리케이션에 전달하십시오. 사용자 이름으로 <code>token</code>을 지정하고 비밀번호로 <var class="keyword varname">api_key</var>를 지정하십시오. 콜론으로 <code>token</code>과 <var class="keyword varname">api_key</var>를 구분하십시오. 자세한 정보는 [클라이언트에 연결](/docs/services/EventStreams?topic=eventstreams-kafka_connect)을 참조하십시오. 
<p>변경사항을 적용하기 위해 애플리케이션을 다시 스테이징해야 할 수 있습니다.</p></li>
</ol>


## 다음에 수행할 작업
{: #after_connecting}

연결하여 인증 정보를 확보했으므로 이제 {{site.data.keyword.messagehub}} 클라이언트를 선택할 수 있습니다. 자세한 정보는 [Kafka API 사용](/docs/services/EventStreams?topic=eventstreams-kafka_using)을 참조하십시오.

<!--
Charlie said:

"Add some info describing how to take the information made available from above e.g. like the info in the Connecting a client to the Kafka API section of the alpha docs on stage 1? https://test.cloud.ibm.com/docs/services/EventStreams?topic=eventstreams-alpha_about#alpha_about"
-->







 















