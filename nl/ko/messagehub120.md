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


# Alpha 프로그램 시작하기
{: #alpha_program}

Alpha 프로그램은 다음 {{site.data.keyword.messagehub}} 서비스 버전에 대한 조기 액세스를 제공합니다.  

{{site.data.keyword.messagehub}} Alpha와 함께 실행되는 앱을 얻으려면 다음 단계를 완료하십시오. 


## {{site.data.keyword.messagehub}} 서비스 작성
{: alpha_create}


  1. [카탈로그 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.stage1.bluemix.net/catalog/labs/?search=vnext)에서
시범 서비스인 **Message Hub vNext - 프로덕션** 타일을 클릭하십시오. </li>

  2. {{site.data.keyword.messagehub}} 서비스를 작성하십시오. 이 서비스는 <code>Premium</code> 가격 책정 플랜의 단일 테넌트 클러스터를 제공하며 일반적으로 프로비저닝에 1 - 3시간이 소요됩니다. 
 


## 콘솔 옵션을 사용하여 신임 정보 가져오기: 테스트 앱 작성 및 연결
{: alpha_app}

{{site.data.keyword.messagehub}} 관련 작업을 수행하려면 신임 정보가 필요합니다.
사용할 수 있는 앱이 아직 없는 경우에는 테스트 앱을 작성하고, 이 앱이 {{site.data.keyword.messagehub}}에 연결하는 데 사용하는 신임 정보를 사용하십시오. 예를 들면, 테스트 앱에 대해 **SDK for Node.js** 서비스를 사용하십시오.  

  1. [카탈로그 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.stage1.bluemix.net/catalog/starters/sdk-for-nodejs)에서 **SDK for Node.js** 타일로 이동하십시오. 
   
  앱 이름을 입력하기 전에 지역을 미국 남부로 선택했는지 확인하십시오. 앱을 작성하십시오. 

  2. 앱이 실행 중인 상태에서 왼쪽의 **연결** 탭을 클릭하십시오. 

  3. **연결 작성** 단추를 클릭하십시오. 

  4. 기존 호환 가능 서비스의 목록에서 새 {{site.data.keyword.messagehub}} 서비스를 선택하고 **연결** 단추를 클릭하십시오. 

  5. **IAM 사용 서비스 연결** 창에서 기본값을 수락하고 **연결**을 클릭하십시오.
  연결할 수 있도록 {{site.data.keyword.messagehub}} 서비스가 프로비저닝되었는지 확인하십시오. 

  6. 왼쪽의 **런타임** 탭을 클릭하고 가운데에 있는 **환경 변수** 탭을 선택하십시오. **VCAP_SERVICES** 섹션에서 메시지를 전송하는 데 필요한 <code>kafka_admin_url</code>, <code>apikey</code> 및 <code>kafka_brokers_sasl</code> 정보를 찾으십시오. 
  
## 명령행 옵션을 사용하여 신임 정보 가져오기
또는, 명령행을 사용하여 필수 신임 정보를 가져올 수도 있습니다. 다음 단계를 완료하십시오.

  1. [{{site.data.keyword.Bluemix_notm}} CLI ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/cli/index.html#overview)로부터 {{site.data.keyword.Bluemix_notm}} 명령행 도구를 설치하십시오. 
  
  2. {{site.data.keyword.Bluemix_notm}} CLI에 로그인하십시오. 
  
  3. {{site.data.keyword.Bluemix_notm}} CLI를 사용해, 다음 명령을 사용하여 관리자 역할이 있는 서비스 키를 작성하십시오. 
  ```
  bx resource service-key-create <NAME> Manager --instance-name <MESSAGEHUB_SERVICE_INSTANCE_NAME>
  ```
  4. 출력에는 apikey, 관리자 REST 엔드포인트 URL 및 브로커 목록이 포함되어 있습니다. 다음 명령을 실행하여 이 정보를 다시 볼 수 있습니다. 
  ```
  bx resource service-key <NAME>
  ```

## {{site.data.keyword.messagehub}} 토픽 작성 및 메시지 전송

CURL 명령을 사용하여 토픽을 작성한 후 kafkacat 도구를 사용하여 메시지를 작성하고 이용할 수 있습니다.  

각 명령에서 APIKEY 및 KAFKA_ADMIN_URL을 VCAP_SERVICES 환경 변수의 값으로 대체하십시오. 

  1. 명령행에서 다음 CURL 명령을 사용하여 {{site.data.keyword.messagehub}} 토픽을 작성하십시오. 
  
    ```
    curl -i -X POST -H "Content-Type: application/json" -H "X-Auth-Token: APIKEY" --data '{ "name": "newtop"}' KAFKA_ADMIN_URL/admin/topics
    ```
    {: codeblock}

  2. 빠른 Kafka 테스트를 수행하는 데 유용한 [kafkacat 도구 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/edenhill/kafkacat#install)를 설치하십시오. 
  
  3. 다음 명령을 실행하려면 다음 정보가 필요합니다. 
  
    * 신임 정보 `kafka_brokers_sasl`에서 리턴된 브로커 목록. kafkacat에 사용하려면 브로커 목록이 쉼표로 구분된 목록이어야 합니다.
	VCAP_SERVICES에는 처음 다섯 개의 브로커만 나열됩니다. 다섯 개보다 많은 브로커가 있는 경우에는 Kafka 클라이언트를 사용하여 다른 브로커의 세부사항을 검색하십시오.  
  
    * <code>apikey</code>. 처음 8자가 sasl.username을 형성하고 나머지 <code>apikey</code>가 sasl.password를 형성합니다. 
    * SSL 인증서의 위치. 예를 들면, Ubuntu에서 SSL_CERTS_DIRECTORY는 <code>/etc/ssl/certs/</code>입니다. 
  
  4. 다음과 같은 명령을 실행하여 몇 가지 메시지를 작성하십시오. 
  ```
  kafkacat -X "security.protocol=sasl_ssl" -X 'sasl.mechanisms=PLAIN' -X 'sasl.username=<FIRST_8_CHARS_FROM_APIKEY>' -X 'sasl.password=<REMAINING_CHARS_FROM_APIKEY>' -X "ssl.ca.location=<SSL_CERTS_DIRECTORY>" -b <BROKERS_LIST> -P -t <TOPIC_NAME>
    ```
		
	<br/>
  명령을 실행한 후에는 제작자 터미널에 <code>HelloWorld</code>와 같은 텍스트를 입력할 수 있습니다. 
  
  5. 다음과 같은 명령을 실행하여 메시지를 이용하십시오. 
  ```
  kafkacat -X "security.protocol=sasl_ssl" -X 'sasl.mechanisms=PLAIN' -X 'sasl.username=<FIRST_8_CHARS_FROM_APIKEY>' -X 'sasl.password=<REMAINING_CHARS_FROM_APIKEY>' -X "ssl.ca.location=<SSL_CERTS_DIRECTORY>" -b <BROKERS_LIST> -C -t <TOPIC_NAME> -f 'Topic %t [%p] at offset %o: key %k: %s\n'
  ```
	
	<br/>
  이용자 터미널에 <code>HelloWorld</code>가 표시됩니다. 

