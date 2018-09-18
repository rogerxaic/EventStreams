---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Kafka 클라이언트를 0.9.X 또는 0.10.X에서 이후 클라이언트 버전으로 마이그레이션
{: #kafka_migrate}


Java 클라이언트를 사용 중인 경우에는
오픈 소스 방식으로 사용 가능한 Kafka 클라이언트 0.10 이상을 사용할 수 있습니다. 0.9.X 버전은 최신 버전으로 업그레이드하는 것이
좋습니다. Kafka 클라이언트는
[https://kafka.apache.org/downloads ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://kafka.apache.org/downloads){:new_window}에서 다운로드할 수 있습니다. 


## Kafka 클라이언트를 0.10.2.X 이상 버전으로 마이그레이션

0.10.2부터는 JAAS 파일을 사용하는 대신 클라이언트의 특성에서 직접 SASL 인증을 구성할 수 있습니다. 이러한 단순화를 통해, 다양한 신임 정보 세트를 사용하여 동일한 JVM에서 여러 클라이언트를 실행할 수 있습니다(JAAS 파일을 사용해서는 불가능함).

다음 단계를 완료하십시오.

1. JAAS 파일을 삭제하십시오. JVM 특성 java.security.auth.login.config=<PATH TO JAAS>는 더 이상 필요하지 않다는 점을 유의하십시오.
2. 0.9.X로부터 마이그레이션하는 경우에는 {{site.data.keyword.messagehub}} 로그인 jar 모듈을 삭제하십시오.
2. 다음 항목을 클라이언트의 특성에 추가하십시오.
    ```
	sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
	```

	여기서 USERNAME 및 PASSWORD는 {{site.data.keyword.Bluemix_notm}}의 {{site.data.keyword.messagehub}} **서비스 신임 정보** 탭에 있는 값입니다.
	
	
## Kafka 클라이언트를 0.9.X에서 0.10.0.X 또는 0.10.1.X로 마이그레이션

다음 단계를 완료하십시오.

1. {{site.data.keyword.messagehub}} 로그인 jar 모듈을 삭제하십시오.
2. <code>jaas.conf</code> 파일을 다음으로 변경하십시오.
    ```
        KafkaClient {
          org.apache.kafka.common.security.plain.PlainLoginModule required
          serviceName="kafka"
            username="USERNAME"
            password="PASSWORD";
        };
    ```
    {: codeblock}

	여기서 USERNAME 및 PASSWORD는 {{site.data.keyword.Bluemix_notm}}의 {{site.data.keyword.messagehub}} **서비스 신임 정보** 탭에 있는 값입니다.
	
3. 다음 행을 이용자 및 제작자 특성에 추가하십시오. <code>sasl.mechanism=PLAIN</code>


