---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-13"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:faq: data-hd-content-type='faq'}

# 클래식 플랜에 대한 FAQ 
{: #faqs_classic}

클래식 플랜의 {{site.data.keyword.IBM}} {{site.data.keyword.messagehub}} 서비스와 관련된 일반적인 질문에 대한 답변입니다.


모든 {{site.data.keyword.messagehub}} 플랜과 관련된 질문에 대한 답변은 [FAQ](docs/services/EventStreams?topic=eventstreams-faqs#faqs)를 참조하십시오.
{: shortdesc}

<!--17/10/17 - Karen: same info duplicated at messagehub104 -->
## 토픽 작성 및 삭제를 위해 Kafka API를 사용하는 방법
{: #topic_admin_classic}
{: faq}

0.11 이상에서 Kafka 클라이언트를 사용 중이거나 0.10.2.0 이상에서 Kafka Streams를 사용 중인 경우, API를 사용하여 토픽을 작성하고 삭제할 수 있습니다. 토픽 작성 시 허용되는 설정에 대한 제한사항이 있습니다. 현재는 다음 설정만 수정할 수 있습니다.

<dl>
<dt>cleanup.policy</dt>
<dd><code>delete</code>(기본값), <code>compact</code> 또는 <code>delete,compact</code>로 설정하십시오.
<p>**참고:**
정리 정책이 <code>compact</code> 전용인 경우, <code>delete</code>를 자동으로 추가하지만, 시간에 따른 삭제를 사용 불가능하게 합니다. 토픽의 메시지는 삭제되기 전에 최대 1GB로 압축됩니다.</p>
</dd>

<dt>retention.ms</dt>
<dd>기본 보존 기간은 24시간입니다. 최소 1시간이며 최대 30일입니다. 이 값을 시간 배수로 지정하십시오.
</dd>
</dl>


## {{site.data.keyword.messagehub}}에서 이용자 오프셋 토픽의 로그 보존 기간으로 설정하는 기간
{: #offsets_classic }
{: faq}

{{site.data.keyword.messagehub}}에서는 이용자 오프셋을 7일 동안 보존합니다. 이는 Kafka 구성 offsets.retention.minutes에 해당됩니다. 

오프셋 보존은 시스템 전체에 해당하므로 개별 토픽 레벨에서 설정할 수 없습니다. 모든 이용자 그룹은 토픽의 로그 보존이 최대 30일로 늘어났더라도 저장된 오프셋의 기간은 7일 동안만 가능합니다. 

<!--following message retention info duplicted in eventstreams057 and evenstreams108-->

## 메시지가 얼마 동안 보존됩니까?
{: #messages_retained}

기본적으로 메시지는 Kafka에서 각 파티션당 최대 1GB까지 최대 24시간 동안 보존됩니다. 1GB 한계에 도달하면 한계를 넘지 않도록 가장 오래된 메시지가 삭제됩니다.

사용자 인터페이스 또는 관리 API 중 하나를 사용하여 토픽을 작성할 때 메시지 보유에 대한 시간 한계를
변경할 수 있습니다. 시간 한계는 최소 1시간이며 최대 30일입니다.

Kafka 클라이언트 또는 Kafka Streams를 사용하여 토픽을 작성할 때 허용되는 설정의 제한사항에 대한 정보는 [토픽 작성 및 삭제를 위해 Kafka API를 사용하는 방법](/docs/services/EventStreams?topic=eventstreams-faqs_classic#topic_admin_classic)을 참조하십시오.


## {{site.data.keyword.messagehub}}의 가용성 작동은 무엇입니까?
{: #availability_classic}
{: faq}

{{site.data.keyword.messagehub}} 앱을 작성하는 경우, 이 정보를 사용하여 보통 {{site.data.keyword.messagehub}} 가용성 작동이 무엇이며 사용자의 앱에서 무엇을 처리해야 하는지 이해하십시오.

### API
{: #api_availability_classic }

{{site.data.keyword.messagehub}}의 일반 오퍼레이션의 일부로서 Kafka 클러스터의 노드가 가끔씩 다시 시작됩니다.
어떤 경우에는 클러스터가 리소스를 재지정하면 사용자의 앱이 인식하게 됩니다. 이러한 변경사항에 복원력이 있으며
다시 연결해서 오퍼레이션을 재시도할 수 있도록 앱을 작성하십시오.

### {{site.data.keyword.messagehub}} 브릿지 
{: #bridge_availability_classic }

브릿지가 가끔씩 다시 시작할 수 있는 가능성을 감당할 수 있도록 앱을 작성하십시오.

## {{site.data.keyword.messagehub}}의 최대 메시지 크기는 얼마입니까? 
{: #max_message_size_classic }
{: faq}

{{site.data.keyword.messagehub}}의 최대 메시지 크기는 1MB이며 이는 Kafka의 기본값입니다. 

## {{site.data.keyword.messagehub}}의 복제 설정은 무엇입니까? 
{: #replication_classic }
{: faq}

{{site.data.keyword.messagehub}}는 높은 가용성과 지속성을 제공하도록 구성되어 있습니다.
다음 구성 설정은 모든 토픽에 적용되며 변경될 수 없습니다.
* replication.factor = 3
* min.insync.replicas = 2

## 클래식 플랜에서 {{site.data.keyword.messagehub}}의 비용 청구는 어떻게 이루어집니까? 
{: #billing_classic }
{: faq}

클래식 플랜에서 {{site.data.keyword.messagehub}}는 주기적으로 사용자의 토픽 수를 샘플링하고 {{site.data.keyword.Bluemix_notm}}는 매일 최대 샘플 값을 기록합니다. {{site.data.keyword.messagehub}}는 매일 송수신되는 메시지 수의 합계 및 표시된 동시 파티션의 최대 수에 대해 비용을 청구합니다.

예를 들어, 하루에 한 개의 토픽을 열 번 작성하고 삭제하는 경우 최대 한 개의 토픽에 대해 비용이 청구됩니다. 하지만 10개의 토픽을 작성하고 삭제하면 샘플링 시기에 따라 0개 또는 10개의 토픽에 대해 비용이 청구될 수 있습니다.

{{site.data.keyword.messagehub}}는 메시지마다 또는 64K마다 비용을 청구합니다. 메시지는 64K까지 한 개의 청구 가능한 메시지로 간주됩니다. 64K가 넘는 메시지는 다음 수의 청구 가능한 메시지로 간주됩니다. <code><var class="keyword varname">message_size</var> &divide; 64 k</code>.

<!--12/04/18 - Karen: same info duplicated at messagehub057 -->
## Kafka REST API는 얼마나 자주 다시 시작합니까? 
{: #REST_restart_classic }
{: faq}

Kafka REST API는 단기간 동안 하루에 한 번 다시 시작합니다. 

이 기간 동안에 Kafka REST API를
사용하지 못하게 될 수 있습니다. 이런 상황이 발생하는 경우, 요청을 재시도하십시오. REST API가 다시 시작된 후에
Kafka 이용자 인스턴스를 재작성해야 합니다. 이런 경우, REST API는 다음과 같은 JSON을 리턴합니다.

```'{"error_code":40403,"message":"Consumer instance not found."}'
```
{:screen}

## {{site.data.keyword.messagehub}} 플랜 사이의 차이점은 무엇입니까?
{: #plan_compare_classic }
{: faq}

다른 {{site.data.keyword.messagehub}} 플랜에 대한 자세한 정보를 알아보려면 [플랜 선택](/docs/services/EventStreams?topic=eventstreams-plan_choose)을 참조하십시오. 클래식 플랜에 대한 자세한 정보는 [클래식 플랜 개요](/docs/services/EventStreams?topic=eventstreams-plan_choose_classic#plan_choose_classic)를 참조하십시오.


## 재해 복구는 어떻게 수행해야 합니까?
{: #disaster_recovery_classic }
{: faq}

현재 {{site.data.keyword.messagehub}} 재해 복구를 관리하는 것은 사용자의 책임입니다. {{site.data.keyword.messagehub}} 데이터는 한 위치(지역)의 {{site.data.keyword.messagehub}} 인스턴스와 다른 위치의 다른 인스턴스 간에 복제될 수 있습니다. 그러나 원격 {{site.data.keyword.messagehub}} 인스턴스를 제공하고 복제를 관리하는 것은 사용자의 책임입니다.

사용자는 메시지 페이로드 데이터의 백업 또한 책임져야 합니다. 이 데이터는 클러스터 내의 여러 Kafka 브로커에 복제(따라서 대부분의 장애 발생 상황에 대한 보호를 제공함)되지만, 이러한 복제 방법이 위치 단위 장애에 대한 보호까지 제공하지는 않습니다. 

토픽 이름은 {{site.data.keyword.messagehub}}에 의해 백업됩니다.















