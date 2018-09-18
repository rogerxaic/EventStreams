---

copyright:
  years: 2015, 2018
lastupdated: "2018-07-05"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 관리 Kafka Java 클라이언트 API 사용
{: #kafka_java_api}


<!-- 
17/10/17 - Karen: following info duplicated at messagehub108
 -->

0.11 이상에서 Kafka 클라이언트를 사용 중이거나 0.10.2.0 이상에서 Kafka Streams를 사용 중인 경우, API를 사용하여 토픽을 작성하고 삭제할 수 있습니다. 토픽 작성 시 허용되는 설정에 대한 제한사항이 있습니다. 현재는 다음 설정만 수정할 수 있습니다.

<dl>
<dt>cleanup.policy</dt>
<dd><code>delete</code>(기본값), <code>compact</code> 또는 <code>delete,compact</code>로 설정하십시오.
<p>**참고:**
정리 정책이 <code>compact</code> 전용인 경우, <code>delete</code>를 자동으로 추가하지만, 시간에 따른 삭제를 사용 불가능하게 합니다. 토픽의 메시지는 삭제되기 전에 최대 1GB로 압축됩니다.</p>
</dd>

<dt>retention.ms</dt>
<dd>기본 보존 기간은 24시간입니다. 최소 1시간이며 최대 30일입니다. 이 값을 시간 배수로 지정하십시오.

<p>**참고:**
엔터프라이즈 플랜에서는 이를 임의의 값으로 설정할 수 있습니다. </p>
</dd>

<dt>retention.bytes</dt>
<dd>이전 로그 세그먼트를 삭제하여 여유 공간을 확보하기 전에 최대 파티션의 크기(로그 세그먼트로 구성됨)는 늘어날 수 있습니다.

<p>**참고:**
엔터프라이즈 플랜에만 해당됩니다. 1MB보다 큰 임의의 값으로 설정하십시오. </p>
</dd>

<dt>segment.bytes</dt>
<dd>로그를 위한 세그먼트 파일 크기입니다.

<p>**참고:**
엔터프라이즈 플랜에만 해당됩니다. 100KB보다 큰 임의의 값으로 설정하십시오. </p>
</dd>

<dt>segment.index.bytes</dt>
<dd>오프셋을 파일 위치로 맵핑하는 인덱스의 크기입니다. 

<p>**참고:**
엔터프라이즈 플랜에만 해당됩니다. 100KB - 2GB 범위의 임의의 값으로 설정하십시오. </p>
</dd>

<dt>segment.ms</dt>
<dd>이 기간이 지나면 세그먼트 파일이 가득 차지 않아도 Kafka에서 로그가 롤링하도록 강제 실행하게 되는 기간입니다.  

<p>**참고:**
엔터프라이즈 플랜에만 해당됩니다. 5분 - 30일 범위의 임의의 값으로 설정하십시오. </p>
</dd>
</dl>

