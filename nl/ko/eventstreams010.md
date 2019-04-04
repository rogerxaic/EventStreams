---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-03"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.messagehub}}의 개념
{: #about}

{{site.data.keyword.messagehub_full}}는 Apache Kafka로 빌드된 높은 처리량의 메시지 버스입니다. 이는 {{site.data.keyword.Bluemix_notm}}로의 이벤트 삽입과 서비스 및 애플리케이션 간의 이벤트 스트림 분배에 최적화된 서비스로 완전히 관리되는 Apache Kafka입니다. {{site.data.keyword.messagehub}}의 이전 이름은 Message Hub입니다.
{: shortdesc}

{{site.data.keyword.messagehub}}를 사용하여 다음과 같은 태스크를 완료할 수 있습니다.

* 백엔드 작업자 애플리케이션에 작업을 오프로드합니다.
* 이벤트 스트림을 스트리밍 분석에 연결하여 강력한 인사이트를 실현합니다.
* 실시간으로 반응하도록 이벤트 데이터를 여러 애플리케이션에 공개합니다.
* 데이터를 다른 서비스로 전송합니다 (예: Cloud Object Storage).

Apache Kafka로 빌드하여 커뮤니티에서 발생하는 모든 호출로부터 직접적으로 혜택을 받고 Kafka 클라이언트 API, Kafka Streams, Kafka Connect 및 KSQL도 지원합니다.

 {{site.data.keyword.messagehub}}에 대한 연결이 항상 인증 정보 사용을 인증하므로 추가 구성을 제공해야 하는 경우에도
Apache Kafka 도구는 일반적으로 {{site.data.keyword.messagehub}}와 함께 직접적으로 작동합니다.

{{site.data.keyword.messagehub}}에서 애플리케이션은
메시지를 작성하고 해당 메시지를 토픽에 보냄으로써 데이터를 전송합니다. 메시지를 수신하기 위해 애플리케이션은
토픽을 구독하고 모든 토픽의 메시지를 수신할지 또는 그 사이에서 메시지를 공유할지 선택합니다.
{{site.data.keyword.messagehub}}는 정렬된 순서로 메시지를 호스팅하고
유지보수합니다. 




