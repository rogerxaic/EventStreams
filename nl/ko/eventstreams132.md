---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-06"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.messagehub}} 가용성(엔터프라이즈 플랜)을 위한 서비스 레벨 계약(SLA)
{: #sla}

{{site.data.keyword.messagehub}} 서비스는 엔터프라이즈 플랜에서 99.95%의 가용성으로 제공됩니다.  
{{site.data.keyword.Bluemix}}의 SLA에 대한 자세한 정보는 [{{site.data.keyword.Bluemix_notm}} 서비스 명세서![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www-03.ibm.com/software/sla/sladb.nsf/pdf/6605-14/$file/i126-6605-14_08-2018_en_US.pdf){:new_window}를 참조하십시오.

## 99.95% 가용성은 무엇을 의미합니까?
가용성은 Kafka 항목에서 메시지를 생성하고 사용할 수 있는 애플리케이션의 기능을 말합니다. 

## 어떻게 측정합니까?
서비스 인스턴스는 성능, 오류율 및 통합 오퍼레이션에 대한 응답을 지속적으로 모니터링합니다. 가동 중단이 기록됩니다. 

## 이러한 가용성을 달성하기 위해 고려해야 할 사항은 무엇입니까? 
애플리케이션 관점에서 높은 수준의 가용성을 달성하려면 [연결성](/docs/services/EventStreams?topic=eventstreams-sla#connectivity), [처리량](/docs/services/EventStreams?topic=eventstreams-sla#throughput) 및 [메시지 일관성 및 지속성](/docs/services/EventStreams?topic=eventstreams-sla#message_consistency)을 고려해야 합니다. 사용자는 비즈니스에 이 세 가지 요소를 최적화하기 위해 애플리케이션을 설계할 책임이 있습니다. 

### 연결성
{: #connectivity}

클라우드의 동적 특성으로 인해 애플리케이션은 연결 중단을 예상해야 합니다. 연결 중단은 서비스 장애로 간주되지 않습니다.

**재시도**<br/>
Kafka 클라이언트는 재연결 논리를 제공하지만, 작성자에 대한 재연결을 명시적으로 활성화해야 합니다. 자세한 정보는 [ <code>retries</code> 특성![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://kafka.apache.org/11/documentation.html#producerconfigs){:new_window}을 참조하십시오. 연결이 60초 이내에 재설정됩니다. **중복**<br/>
재시도를 사용하도록 설정하면 메시지가 중복될 수 있습니다. 연결 중단 시점에 따라 작성자는 서버에서 메시지가 성공적으로 처리되었는지 확인하지 못할 수 있으므로 다시 연결했을 때 메시지를 다시 전송해야 합니다. 중복 메시지를 예상할 수 있도록 애플리케이션을 설계하는 것이 좋습니다.  

중복이 허용될 수 없는 경우, 재시도 중 중복이 발생하지 않도록 <code>idempotent</code> 작성자 기능(from Kafka 1.1부터 이용 가능)을 사용할 수 있습니다. 자세한 정보는 [<code>enable.idempotence</code> 특성![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://kafka.apache.org/11/documentation.html#producerconfigs){:new_window}을 참조하십시오.

### 처리량
{: #throughput}

처리량은 클러스터에서 보내고 받을 수 있는 초당 바이트 수로 표시됩니다. 

**권장사항**<br/>
초당 40MB이며 최대 피크 제한은 초당 90MB입니다. <br/>
권장 수치는 일반적인 워크로드를 기반으로 하며 내부 업데이트 또는 가용성 영역 손실 등의 장애 모드와 같은 운영 조치의 가능한 영향을 고려한 것입니다. 예를 들면 작은 페이로드(10K 미만)가 포함된 메시지입니다. 평균 처리량이 이 수치를 초과할 경우 이러한 조건에서 성능이 저하될 수 있습니다. 

**측정**<br/>
애플리케이션의 작동 방식을 이해하기 위해 애플리케이션을 계측하는 것이 좋습니다. 예를 들어, 보내고 받은 메시지 수, 메시지 크기 및 리턴 코드 등이 있습니다. 애플리케이션의 사용량을 이해하면 해당 애플리케이션의 리소스를 적절하게 구성하는 데 도움이 됩니다(예: 항목에 대한 메시지 보존 시간). 

**포화**<br/>
클러스터로 생성될 수 있는 트래픽의 한계에 도달하면 작성자가 제한되기 시작하고 지연 시간이 증가하며 궁극적으로 제한시간 초과와 같은 오류가 발생합니다. 구성에 따라 메시지 일관성 및 지속성에도 영향을 줄 수 있습니다. 자세한 정보는 [메시지의 일관성 및 지속성](/docs/services/EventStreams?topic=eventstreams-sla#message_consistency)을 참조하십시오.

### 메시지의 일관성 및 지속성
{: #message_consistency}

Kafka는 클러스터의 다른 노드에서 수신하는 메시지를 복제하여 가용성과 지속성을 달성합니다. 이 메시지는 장애 발생 시 사용할 수 있습니다. {{site.data.keyword.messagehub}}에서는 3개의 복제본(default.replication.factor = 3)을 사용합니다. 즉, 노드에서 수신한 각 메시지는 서로 다른 가용성 영역의 다른 두 노드에 복제됩니다. 이렇게 하면 데이터나 기능의 손실 없이 노드 또는 가용성 영역의 손실을 허용할 수 있습니다. 

**작성자 <code>acks</code> 모드**<br/>
모든 메시지가 복제되지만 애플리케이션은 작성자의 <code>acks</code> 모드 특성을 사용하여 작성자가 생성한 메시지가 서비스로 얼마나 강력하게 전송되는지를 제어할 수 있습니다. 이 특성은 속도와 메시지 손실 위험 사이에서 선택할 수 있게 합니다. 기본 설정은 <code>acks=1</code>입니다. 즉, 연결된 노드가 메시지를 수신한 즉시, 하지만 복제가 완료되기 전에, 작성자가 성공을 리턴합니다. 권장되면서 가장 확실한 설정은 모든 복제본에 메시지가 복사된 후에만 작성자가 성공을 리턴하는 <code>acks=all</code>입니다. 이렇게 하면 복제본이 단계별로 유지되므로 오류가 발생하여 복제본으로 전환되는 경우 메시지 손실을 방지할 수 있습니다.


