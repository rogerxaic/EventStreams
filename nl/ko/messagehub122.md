---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

<!-- Notes from chat with Charlie 

Different plan for provisioning

Quality of service from each plan

Life of a user through cycle - APIs, feature sets

-->

# Alpha 프로그램 정보
{: #alpha_about }

Alpha 프로그램은 {{site.data.keyword.messagehub}} 서비스의 새 기능에 대한 조기 액세스를 제공합니다. 현재 기능은 새 플랜인 {{site.data.keyword.messagehub}} Premium 플랜의 소개입니다. 

## Premium 플랜
{: premium_plan}

Premium 플랜은 공용 서비스의 범위를 벗어난 성능 및 기타 기능 요구사항이 있는 사용자를 위해 디자인되었습니다. 이는 사용자가 하나의 Apache Kafka 클러스터를 전용하는 {{site.data.keyword.messagehub}} 서비스의 단일 테넌트 버전을 제공합니다. 이 버전은 사용자에게 다음을 제공합니다. 

* 클러스터의 용량 및 성능을 모두 이용

* 크게 늘어난 한계 및 파티션 수

* 자동으로 유지보수 및 관리되는 전체 관리 클러스터를 통한 무료 관리 비용 혜택

## Alpha 클러스터 정보
{: alpha_cluster}

Alpha 클러스터는 Apache Kafka 버전 1.1을 사용하여 배치되며 최대 초당 90000KB의 메시지 처리량을 제공할 수 있습니다.  

최대 1000개의 파티션을 작성할 수 있으며 각 파티션은 최장 30일 동안 최대 1GB의 데이터를 보유할 수 있습니다. 복원성을 위해 데이터는 3개의 복제본으로 보관되며 각 파티션에 대해 커미트된 오프셋은 최장 7일 동안 유지됩니다. 

다음 두 API가 지원됩니다. 

* 메시징의 경우에는 Kafka Streams, Kafka Connect 및 KSQL 사용 옵션을 포함, 버전 0.10.x 이상의 Kafka 클라이언트가 지원됩니다. 

* 관리의 경우에는 토픽 작성, 삭제 및 나열에 REST API를 사용할 수 있습니다. 

Alpha 프로그램은 미국 남부 지역에서만 사용할 수 있습니다. 클러스터에 대한 액세스는 IAM을 통해 관리됩니다. 

## 클러스터에 연결
{: alpha_connect}

클러스터의 API에 연결하려면 해당 엔드포인트 URL과 인증을 위한 apikey가 필요합니다. 다음 방법 중 하나를 사용하여 이러한 세부사항을 IAM에서 검색할 수 있습니다. 

### Cloud Foundry 애플리케이션
Cloud Foundry 애플리케이션의 경우에는 다음 작업을 수행하십시오. 
1. 앱의 **연결** 탭(왼쪽에 있음)에서 **연결 작성** 단추를 클릭하십시오.  
2. 연결할 {{site.data.keyword.messagehub}} 서비스의 인스턴스를 선택하고 **연결**을 클릭하십시오. 기본 옵션을 수락하십시오.  
3. 연결이 완료되면 앱의 **런타임** 탭을 클릭한 후 **환경 변수**를 클릭하여 **VCAP_SERVICES**를 표시하십시오. 

### 외부 애플리케이션의 콘솔
외부 애플리케이션의 콘솔에서 다음 **bx** 명령을 사용하여 서비스 apikey를 작성하십시오.  

```
bx resource service-key-create name-of-key Manager --instance-name name-of-your-service
``` 

생성된 정보에서 <code>kafka_brokers_sasl</code>, <code>kafka_admin_url</code> 및 <code>apikey</code> 필드를 복사하십시오.
VCAP_SERVICES에는 처음 다섯 개의 브로커만 나열됩니다. 다섯 개보다 많은 브로커가 있는 경우에는 Kafka 클라이언트를 사용하여 다른 브로커의 세부사항을 검색하십시오.  

## 클라이언트를 Kafka API에 연결

클라이언트를 Kafka API에 연결하려면 다음 단계를 완료하십시오. 

1. 클라이언트 <code>bootstrap.servers</code> 특성을 <code>kafka_brokers_sasl</code>에 나열된 브로커의 쉼표로 구분된 목록으로 설정하십시오. 

2. 클라이언트 <code>sasl.jaas.config</code> USERNAME 필드를 <code>apikey</code>의 처음 8자로, PASSWORD 필드를 나머지 문자로 설정하십시오(이후 버전에서는 이러한 분할이 필요 없게 될 예정임).

사용하는 Kafka 클라이언트는 다음 기능을 지원해야 합니다. 

* 클라이언트 버전 0.10.x 이상

* SASL PLAIN 메커니즘을 사용한 인증

* TLS v1.2 프로토콜의 SNI(Service Name Identification) 확장

이 엔드포인트 및 신임 정보 관련 정보 검색 방법은 기존 {{site.data.keyword.messagehub}} 서비스와 다릅니다. {{site.data.keyword.messagehub}}에 대해 현재 실행되는 앱은 VCAP_SERVICES에서 필요로 하고 Kafka에 제출되는 사용자 이름/비밀번호 필드에 필요한 대체 필드 이름을 반영하도록 변경해야 합니다. 이러한 변경은 이후 Alpha 버전에서는 필요하지 않게 될 예정입니다. 

## 클라이언트를 REST API에 연결

클라이언트를 REST API에 연결하려면 다음 단계를 완료하십시오. 

* REST API의 URI는 <code>kafka_admin_url</code>에서 제공됩니다. 

* <code>Content-Type</code> 헤더를 <code>application/json</code>으로 설정하십시오. 

* HTTP <code>X-Auth-Token</code> 헤더를 <code>apikey</code>의 값으로 설정하십시오. 

Alpha를 시작하고 실행하는 간단한 단계는 [Alpha 프로그램 시작하기](/docs/services/MessageHub/messagehub120.html)를 참조하십시오. 


## {{site.data.keyword.messagehub}} 관리
{: alpha_admin}

클러스터에서 필요한 관리 태스크는 필요한 토픽을 작성하고 나열하고 삭제하는 것뿐입니다. 다음 방법 중 하나를 사용하여 관리할 수 있습니다. 

* 애플리케이션의 Kafka 관리자 API를 통해 직접 관리합니다. Java의 예를 들면, AdminClient ![외부 링크 아이콘](http://kafka.apache.org/11/javadoc/index.html?org/apache/kafka/clients/admin/AdminClient.html){:new_window} 문서의 <code>createTopics()</code>, <code>deleteTopics()</code> 또는 <code>listTopics()</code> 메소드를 사용합니다. 

* IBM Cloud 포털에서 사용 가능한, 서비스 인스턴스에 대한 웹 UI를 사용하여 대화식으로 관리합니다. 

* 클러스터에 제공된 관리자 REST API를 사용하여 관리합니다. 

관리자 REST API(기존 {{site.data.keyword.messagehub}} 관리자 API와 호환됨)에서 제공하는 작성, 나열 및 삭제 기능에 대한 추가적인 세부사항은 [admin-rest-api.yaml ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-messaging/message-hub-docs/blob/master/admin-rest-api/admin-rest-api.yaml){:new_window}에 있는, 이 API의 전체 스펙에서 찾을 수 있습니다.
Swagger 파일을 보려면 [Swagger 편집기 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://editor.swagger.io/#/){:new_window}와 같은 Swagger 도구를 사용하십시오. 

Curl을 사용하여 토픽을 작성하는 방법을 보여주는 간단한 예는 [Alpha 프로그램 시작하기](/docs/services/MessageHub/messagehub120.html)를 참조하십시오. 

이후에는 다른 구성 옵션도 사용 가능하게 될 예정입니다. 


## 샘플

곧 제공될 예정입니다...

Alpha를 시작하고 실행하는 간단한 단계는 [Alpha 프로그램 시작하기](/docs/services/MessageHub/messagehub120.html)를 참조하십시오. 

## Alpha 제한사항
{: alpha_limitations}

이 Alpha 프로그램의 현재 제한사항은 다음과 같습니다. 

### 아직 사용 가능하지 않지만 곧 도입될 항목

* 여러 가용성 지역에 대한 지원

* 사용자 제어 스케일링 및 로드 한계

* {{site.data.keyword.cloudaccesstrailfull_notm}} 서비스(이전 이름은 AccessTrail)와의 통합 

### 현재 계획되지 않은 항목

* 브릿지

* REST 메시징

* MQ Light API










