---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-23"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}

# REST 제작자 API 사용
{: #rest_producer_using}


** REST 제작자 API는 {{site.data.keyword.messagehub}} 표준 및 엔터프라이즈 플랜의 일부로만 사용 가능합니다.**
<br/>

{{site.data.keyword.messagehub}}는 기존 시스템을 {{site.data.keyword.messagehub}} Kafka 클러스터에 연결하는 데 도움이 되는 REST API를 제공합니다. API를 사용하여 RESTful API를 지원하는 시스템과 {{site.data.keyword.messagehub}}를 통합할 수 있습니다.

REST 제작자 API는 보안 HTTP 엔드포인트를 통해 {{site.data.keyword.messagehub}}에 메시지를 생성하기 위한 확장 가능한 REST 인터페이스입니다. 이벤트 데이터를 {{site.data.keyword.messagehub}}에 전송하고 Kafka 기술을 활용하여 데이터 피드를 처리하며 {{site.data.keyword.messagehub}} 기능을 이용하여 데이터를 관리하십시오.

API를 사용하여 기존 시스템을 {{site.data.keyword.messagehub}}에 연결하십시오. 메시지 키, 헤더 및 메시지를 작성할 토픽 지정을 포함하여 시스템에서 {{site.data.keyword.messagehub}}로의 생성 요청을 작성하십시오.


## REST를 사용하여 메시지 생성
{: #rest_produce_messages}

토픽에 메시지를 작성하려면 제작자 API를 사용하십시오. 토픽을 생성할 수 있으려면 다음 정보가 사용 가능해야 합니다.

* 포트 번호를 포함한 {{site.data.keyword.messagehub}} API 엔드포인트의 URL
* 생성할 토픽
* 선택한 토픽에 연결하고 생성할 수 있는 권한을 제공하는 API 키

서비스 인스턴스에 대한 서비스 인증 정보 오브젝트 또는 서비스 키에서 API에 연결하는 데 필요한 URL 및 인증 정보 세부사항을 검색해야 합니다. 이러한 오브젝트 작성에 대한 정보는 [{{site.data.keyword.messagehub}}에 연결](/docs/services/EventStreams?topic=eventstreams-connecting)을 참조하십시오.

API 엔드포인트에 대한 URL은 <code>kafka_http_url</code> 특성에서 제공됩니다.

인증하려면 다음 방법 중 하나를 사용하십시오.

* **기본 인증을 사용하여 인증:**<br/>
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

<br/>
다음 코드는 curl을 사용하여 메시지를 전송하는 예제를 보여줍니다.

```
curl -v -X POST -H "Authorization: Basic <base64 username:password>" -H "Content-Type: text/plain" -H "Accept: application/json" -d 'test message' "<kafka_http_url>/topics/<topic_name>/records"
```
{: codeblock}


## API 참조
{: #rest_api_reference}

API에 대한 전체 세부사항은 [{{site.data.keyword.messagehub}} REST 제작자 API 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://ibm.github.io/event-streams/api/){:new_window}를 참조하십시오.












