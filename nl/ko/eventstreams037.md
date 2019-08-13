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

# 관리 REST API 사용
{: #admin_api}

{{site.data.keyword.messagehub}}는 토픽을 작성, 삭제, 나열 및 업데이트하는 데 사용할 수 있는 관리 RESTful API를 제공합니다.
{: shortdesc}

서비스 인스턴스에 대한 서비스 인증 정보 오브젝트 또는 서비스 키에서 API에 연결하는 데 필요한 URL 및 인증 정보 세부사항을 검색해야 합니다. 이러한 오브젝트 작성에 대한 정보는 [{{site.data.keyword.messagehub}}에 연결](/docs/services/EventStreams?topic=eventstreams-connecting)을 참조하십시오.

API 엔드포인트에 대한 URL은 <code>kafka_admin_url</code> 특성에서 제공됩니다.

인증 정보는 인증 방법에 따라 다르며 세 가지 유형의 인증 정보가 지원됩니다.

* **기본 인증을 사용하여 인증:**:<br/>
    위의 오브젝트의 <code>user</code> 및 <code>api_key</code> 특성을 사용자 이름 및 비밀번호로 사용하십시오. 이러한 값을 <code>Basic &lt;단일 콜론(:)으로 결합된 사용자 이름과 비밀번호의 base64 인코딩&gt;</code> 양식으로 HTTP 요청의 <code>Authorization</code> 헤더에 배치하십시오.

* **전달자 토큰을 사용하여 인증:**<br/>
    IBM Cloud CLI를 사용하여 토큰을 얻으려면 먼저 IBM Cloud에 로그인한 후 다음 명령을 실행하십시오. 

    ```
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

    이 토큰을 <code>Bearer <token></code> 양식으로 HTTP 요청의 Authorization 헤더에 배치하십시오. API 키 또는 JWT 토큰이 모두 지원됩니다. 

* **api_key를 사용하여 직접 인증:**<br/>
    키를 직접 <code>X-Auth-Token</code> HTTP 헤더의 값으로 배치하십시오.

클래식 플랜에서 작성된 서비스 인스턴스의 경우 이 정보는 대신 애플리케이션의 VCAP_SERVICES 환경 변수에서 사용할 수 있습니다.

예제가 포함된 API에 대한 설명은 [{{site.data.keyword.messagehub}} admin-rest ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-messaging/event-streams-docs/tree/master/admin-rest-api){:new_window}를 참조하십시오.

[{{site.data.keyword.messagehub}} 관리 REST API YAML 파일 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-messaging/event-streams-docs/blob/master/admin-rest-api/admin-rest-api.yaml){:new_window}에서 API에 대한 전체 스펙을 다운로드할 수 있습니다.
Swagger 파일을 보려면 [Swagger 편집기 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://editor.swagger.io/#/){:new_window}와 같은 Swagger 도구를 사용하십시오.




