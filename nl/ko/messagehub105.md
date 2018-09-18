---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-01"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# MQ 브릿지
{: #mq_bridge}

** {{site.data.keyword.messagehub}} 브릿지는 표준 플랜의 일부로만 사용 가능합니다.**
<br/>

MQ 브릿지를 통해 {{site.data.keyword.IBM_notm}} MQ 큐에서 {{site.data.keyword.messagehub}} Kafka 토픽으로 메시지 데이터를 전송할 수 있습니다. MQ 브릿지로 엔터프라이즈 내에서 생성된 {{site.data.keyword.IBM_notm}} MQ 메시지 데이터에 대한 클라우드 스타일 워크로드(예: 데이터 분석)를 효율적으로 수행할 수 있습니다.
 {:shortdesc}

MQ 클라이언트로 MQ 브릿지를 {{site.data.keyword.IBM_notm}} MQ 큐 관리자에 연결하고 MQ 큐에서 MQ 메시지 데이터를 이용합니다. 브릿지는 각 MQ 메시지를 Kafka 레코드로 변환하고 해당 메시지를 {{site.data.keyword.messagehub}} Kafka 토픽에 전송합니다.

## {{site.data.keyword.IBM_notm}} MQ의 지원 버전
{: #mq_support}

브릿지가 작업하는 {{site.data.keyword.IBM_notm}} MQ의 버전은 다음과 같습니다.

* {{site.data.keyword.IBM_notm}} MQ 버전 8.0 이상

## MQ 브릿지 보안
{: #mq_bridge_security}

사용자 ID 및 비밀번호를 사용하여 {{site.data.keyword.IBM_notm}} MQ와 인증하도록 MQ 브릿지를 구성할 수 있습니다. MQ 브릿지의 인스턴스와 연관된 ID에만 다음 권한을 부여하는 것이 좋습니다.

* CONNECT 권한. MQ 브릿지가 MQ 큐 관리자에 연결할 수 있어야 합니다.
* MQ 브릿지가 이용하도록 구성된 큐에 대한 GET 권한.

## 메시지 신뢰성 및 순서 지정
{: #mq_bridge_reliability}

MQ 브릿지는 전송하는 메시지 데이터에 대해 최소 한 번(at-least-once) 수준의 안정성으로
구현합니다. 이는 브릿지가 메시지 데이터를 유실하지는 않지만 주어진 시퀀스의 MQ 메시지에 대해
Kafka 레코드의 중복 시퀀스가 종종 생성될 수 있음을 의미합니다. 일반적으로 이벤트가
브릿지의 메시지 데이터 처리를 중단할 때 중복이 발생합니다. 예를 들어, 다음과 같은 경우입니다.

* MQ 큐 관리자 다시 시작
* MQ 큐 관리자와 브릿지 간의 네트워크 중단
* Kafka 토픽 파티션 지정 재밸런싱
* 브릿지와 Kafka 간의 네트워크 중단

MQ 브릿지가 MQ에서 메시지를 확실하게 전송할 수 있도록 브릿지는
메시지를 읽는 MQ 큐에 대한 독점 액세스 권한을 요청합니다. 이 액세스 권한은 브릿지가 사용하고 있는 동안에 다른
애플리케이션이 MQ 큐에서 메시지를 수신하지 못하게 합니다. 다른 애플리케이션이
큐에서 메시지를 이미 수신하고 있는 경우, 해당 애플리케이션이 종료될 때까지
MQ 브릿지를 시작하지 못할 수 있습니다.

일반적으로 MQ 브릿지는 MQ 메시지를 {{site.data.keyword.IBM_notm}} MQ에서 수신된 순서로 Kafka에 전달합니다. 그러나 때때로 MQ 메시지 데이터는 Kafka 토픽 내에서 중복됩니다(이전에 설명된 이유로). 이러한 중복 가능성으로 인해 MQ 메시지의 순서와 해당하는 레코드가 Kafka에 저장된 순서 간에 차이가 있을 수 있습니다.

## Kafka 파티션 지정
{: #mq_bridge_partition}

MQ 브릿지는 Kafka 토픽 파티션에 {{site.data.keyword.IBM_notm}} MQ 메시지 데이터 지정을 제어하는 옵션을 지원합니다. 특정 파티션 내에서 메시지 순서 지정은 메시지 데이터 순서 지정을 위해 선택한 옵션에 좌우됩니다([메시지 신뢰성 및 순서 지정](#mq_bridge_reliability) 참조). 지원되는 값은 다음과 같습니다.
<dl><dt>기본값</dt>
<dd>Kafka의 기본 파티션 지정이 사용되며, MQ 메시지의 데이터가
Kafka 토픽의 파티션 전체에 고르게 분산됨을 의미합니다.</dd>
<dt>CorrelationId</dt>
<dd>같은 상관 ID가 있는 MQ 메시지가 같은 Kafka 파티션에 지정됩니다.</dd>
<dt>GroupId</dt>
<dd>같은 그룹 ID가 있는 MQ 메시지가 같은 Kafka 파티션에 지정됩니다.
</dd>
</dl>

## 메시지 크기 제한
{: #mq_message}

{{site.data.keyword.messagehub}} Kafka 레코드에 맞추기에는 너무 큰 메시지를 저장하도록 {{site.data.keyword.IBM_notm}} MQ를 구성할 수 있습니다. MQ 상관 ID 또는 MQ 그룹 ID를 기반으로
Kafka 파티션 지정을 수행하도록 브릿지가 구성된 경우 이 용량의 일부가 사용되더라도 최대 Kafka 레코드
크기는 1,000,000바이트입니다. 950KB 미만의 메시지는 MQ 브릿지를 사용하여 보내는 것이 좋습니다.

MQ 브릿지에 Kafka에 전달하기에 너무 큰 메시지가 발생하는 경우,
브릿지는 메시지를 버리고 Kibana 대시보드에 로그 항목을 작성합니다. MQ 애플리케이션에서
MQ 브릿지를 사용하여 전송할 수 없는 큐에 메시지를 보내지 못하게 하려면 브릿지가 메시지를 수신하는 MQ 큐의 MAXMSGL 속성을
설정하는 것에 대해 고려하십시오.
