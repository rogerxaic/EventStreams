---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-09"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 한계 및 할당량
{: #kafka_quotas }

{{site.data.keyword.messagehub}}는 할당량을 사용하여 서비스에서 이용할 수 있는 리소스(예: 네트워크 대역폭)를 제어합니다. 할당량의 유형 및 레벨은 표준 또는 엔터프라이즈 플랜을 사용 중인지에 따라 달라집니다.

## 표준 플랜
{: #limits_standard }

### 네트워크 처리량
{: #standard_throughput }

각 서비스 인스턴스에 대한 최대 처리량은 파티션별 초당 1MB에 해당되며 최대 초당 20MB입니다. 예를 들어, 10개의 파티션이 있는 서비스 인스턴스의 경우 최대 처리량은 초당 10MB이며 30개의 파티션의 경우 초당 20MB입니다.

처리량은 제작자와 이용자에 대해 별도로 측정됩니다. 초과되는 경우 요청에 대한 응답을 약간 지연시키고 대역폭이 감소될 때까지 제작자와 이용자에게 효과적으로 완만한 브레이크를 적용하여 조절이 적용됩니다.

### 파티션
{: #standard_partitions}

각 서비스 인스턴스마다 100개의 파티션

### 보존
{: #standard_retention}

각 파티션마다 최대 1GB

### 기타 한계
{: #standard_limits}

* 최대 메시지 크기: 1MB
* 최대 동시 활성 Kafka 클라이언트 수: 100
* 초당 최대 요청 비율 [HTTP Produce API]: 100
* 초당 최대 요청 비율 [HTTP Admin API]: 10

## 엔터프라이즈 플랜
{: #limits_enterprise }

### 네트워크 처리량
{: #enterprise_throughput }

권장되는 최대값은 초당 40MB이며 최대 한계는 초당 90MB입니다. 처리량은 클러스터에서 보내고 받을 수 있는 초당 바이트 수로 표시됩니다. 

권장 수치는 일반적인 워크로드를 기반으로 하며 내부 업데이트 또는 가용성 영역 손실 등의 장애 모드와 같은 운영 조치의 가능한 영향을 고려한 것입니다. 평균 처리량이 권장 수치를 초과할 경우 이러한 조건에서 성능 저하가 발생할 수 있습니다.


### 파티션
{: #enterprise_partitions}

각 서비스 인스턴스마다 1000개의 파티션

### 보존
{: #enterprise_retention}

플랜의 스토리지 한계까지 제한 없음

### 기타 한계
{: #enterprise_limits}

최대 메시지 크기: 1MB




















