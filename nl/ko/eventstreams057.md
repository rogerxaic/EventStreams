---

copyright:
  years: 2015, 2019
lastupdated: "2018-06-26"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 알려진 제한사항
{: #restrictions}

{{site.data.keyword.messagehub}} 사용 중에 문제점을 발견하면 알려진 제한사항 및 임시 해결책을 검토하십시오. 
{: shortdesc}

## Kafka 부트스트랩 서버가 실패하는 경우 Java Kafka 호출은 장애 복구를 수행하지 않음
{: #calls_failover}

### 문제점
{: #calls_failover_problem notoc}

JVM(Java Virtual Machine)은 DNS 검색을 캐시합니다. JVM이 호스트 이름의 IP 주소를 분석하는 경우 TTL(Time to Live)이라고 하는 지정된 기간 동안 IP 주소를 캐시합니다. JVM이 다시 시작될 때까지 호스트 이름의 IP 주소를 새로 고치지 않도록 일부 Java 구성에서 JVM TTL을 설정합니다. 구성 예제는 보안 관리자가 있는 구성입니다.

### 임시 해결책
{: #calls_failover_workaround notoc}

{{site.data.keyword.messagehub}}가 고가용성을 위해 여러 IP 주소가 포함된 Kafka 부트스트랩 서버 URL을 사용하므로, 모든 브로커 IP 주소는 Kafka 클라이언트에 알려져 있지 않습니다. 이에 따라 작동 중인 브로커에 대한 장애 복구가 발생하지 않습니다. 해당 경우, 장애 복구 시 작동 중인 IP 주소를 가져오도록 브로커 URL에 대한 IP 주소를 다시 조회해야 합니다. 30 - 60초의 TTL 값으로 JVM을 구성하는 것이 좋습니다. 이 값을 통해 부트스트랩 서버의 IP 주소에 문제가 있으면 Kafka 클라이언트는 DNS를 조회하여 새 IP 주소를 검색한 후 사용할 수 있습니다.

<code>java.security</code> 파일에서: 

```
# The Java-level namelookup cache policy for successful lookups:
#
# any negative value: caching forever
# any positive value: the number of seconds to cache an address for
# zero: do not cache
#
# default value is forever (FOREVER). For security reasons, this
# caching is made forever when a security manager is set. When a security
# manager is not set, the default behavior in this implementation
# is to cache for 30 seconds.
#
# NOTE: setting this to anything other than the default value can have
#       serious security implications. Do not set it unless
#       you are sure you are not exposed to DNS spoofing attack.
#
#networkaddress.cache.ttl=-1
```

### JVM의 TTL 수정 방법
* 모든 애플리케이션을 위한 JVM의 TTL을 수정하려면 <code><var class="keyword varname">$JAVA_HOME</var>/jre/lib/security/java.security</code> 파일에서 <code>networkaddress.cache.ttl</code> 값을 설정하십시오.
* 지정된 애플리케이션을 위한 JVM의 TTL을 수정하려면 애플리케이션 코드에서 다음과 같이 <code>networkaddress.cache.ttl</code>을 설정하십시오.
```
java.security.Security.setProperty("networkaddress.cache.ttl" , "30");
```

## Java Kafka 호출이 제한시간을 초과할 수 있음
{: #calls_timeout}

### 문제점
{: #calls_timeout_problem notoc}

때때로 Kafka Java 클라이언트 호출이 Kafka를 찾는 데 실패하기도 합니다. 실패의 원인은 Kafka 클라이언트가 각 부트스트랩 서버에 대해 실패하는 동일한 IP 주소를 사용하도록 결정했기 때문입니다. Kafka 클라이언트는 각 브로커의 IP 주소(실패하는 동일한 IP 주소)를 시도하고 Kafka가 작동 중지되도록 잘못 결정합니다. DNS 조회에서 여러 IP 주소가 리턴되는 경우 Kafka 클라이언트는 리턴된 첫 번째 IP 주소를 사용한다는 점을 유의하십시오.

### 임시 해결책
{: #calls_timeout_workaround notoc}

브로커 URL에 대한 JVM DNS 캐시가 만료되도록 충분히 오랜 시간 동안 기다린 후 호출을 재시도하십시오. 후속 Kafka 호출에서는 작동되는 브로커 IP 주소가 DNS 조회에서 리턴되고 사용되어야 합니다. 

KIP(Kafka Improvement Proposal) #302는 Kafka 클라이언트가 사용 가능한 모든 브로커 IP 주소(서브셋 아님)를 시도했음을 확인하기 위해 작성되었으므로
하나의 IP 주소에서의 실패로 인해 실패가 발생하지는 않습니다.


## 토픽 및 파티션
{: #topics_partitions}

*  토픽 이름은 최대 100자로 제한됩니다.
*  토픽의 기본 파티션 수는 하나입니다.
*  각 {{site.data.keyword.Bluemix_notm}} 영역에는 파티션의 수가 100개로 제한됩니다. 더 많은 파티션을 작성하려면,
새 {{site.data.keyword.Bluemix_notm}} 영역을 사용해야 합니다.

## 메시지 보유
{: #message_retention}

기본적으로 메시지는 Kafka에서 각 파티션당 최대 1GB까지 최대 24시간 동안 보존됩니다. 1GB 한계에 도달하면 한계를 넘지 않도록 가장 오래된 메시지가 삭제됩니다.

사용자 인터페이스 또는 관리 API 중 하나를 사용하여 토픽을 작성할 때 메시지 보유에 대한 시간 한계를
변경할 수 있습니다. 시간 한계는 최소 1시간이며 최대 30일입니다.

Kafka 클라이언트 또는 Kafka Streams를 사용하여 토픽을 작성할 때 허용되는 설정의 제한사항에 대한 정보는 [토픽 관리용 API](/docs/services/EventStreams/eventstreams104.html)를 참조하십시오.

## Kafka에서 토픽 작성 및 삭제
{: #create_delete}

Kafka에서 토픽 작성 및 삭제는 비동기 작업으로 완료하는 데 약간의 시간이 소요될 수
있습니다. 토픽의 빠른 작성 및 삭제 또는 토픽의 빠른 삭제 및 재작성에 의존하는
사용법 패턴은 피하는 것이 좋습니다.

## Kafka REST API
{: #trouble_rest}

*  요청 및 응답에 2진 임베디드 형식만 지원됩니다. Avro 및 JSON 임베디드 형식은 지원되지 않습니다.
*  동시 요청은 이용자 인스턴스에 대해 지원되지 않습니다.
   이용자 인스턴스에 해당하는 읽기, 커미트 또는 삭제 요청은 해당 인스턴스의 미해결 요청에 대한 응답을 수신한 후에만 전송해야 합니다.

## Kafka REST API 비율 제한사항
{: #kafka_rate}

Kafka REST API를 사용하는 애플리케이션은 각 ApiKey에 대한 비율 제한 대상이 될 수 있습니다. 이 제한이 발생할 때 API는 다음과 같은 HTTP 오류로 응답합니다.

<code>429 Too Many Requests</code>
{:screen}

이 오류가 발생하는 경우 잠시 대기한 후에 요청을 다시 제출하십시오.

<!--12/04/18 - Karen: same info duplicated at messagehub108 -->
## Kafka REST API 일별 다시 시작
{: #rest_restart}

Kafka REST API는 단기간 동안 하루에 한 번 다시 시작합니다. 이 기간 동안에 Kafka REST API를
사용하지 못하게 될 수 있습니다. 이런 상황이 발생하는 경우, 요청을 재시도하십시오. REST API가 다시 시작된 후에
Kafka 이용자 인스턴스를 재작성해야 합니다. 이런 경우, REST API는 다음과 같은 JSON을 리턴합니다.

```'{"error_code":40403,"message":"Consumer instance not found."}'
```
{:screen}

## Kafka 상위 레벨 이용자 API
{: #kafka_consumer}

Apache Kafka 0.8.2 단순 또는 상위 레벨 이용자 API는
{{site.data.keyword.messagehub}}와 함께 사용할 수 없습니다. 대신, 가장 초기에 지원된 Kafka 이용자 API 즉, 0.9를 사용할 수 있습니다.
