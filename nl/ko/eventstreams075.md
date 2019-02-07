---

copyright:
  years: 2015, 2019
lastupdated: "2018-11-20"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# MQ Light API 사용 
{: #mql_using}

** MQ Light API는 표준 플랜의 일부로만 사용 가능합니다.**
<br/>

{{site.data.keyword.mql}} API는 초기 {{site.data.keyword.mql}} 서비스와의 역호환성을 위해 제공됩니다. API는 AMQP 기반 메시징 인터페이스를 Java&trade;, Node.js, Python 및 Ruby에 제공합니다. 
{:shortdesc}

<!-- 02/07/18 - removing words to help deprecate MQ Light
In most cases, {{site.data.keyword.messagehub}} is best used with a Kafka client. The {{site.data.keyword.mql}} API is simple to learn but has very limited scalability and does not offer interoperability with other {{site.data.keyword.messagehub}} APIs.
The {{site.data.keyword.mql}} API is available in the following {{site.data.keyword.Bluemix_short}} regions only: US South, United Kingdom, and Sydney. The {{site.data.keyword.mql}} API not available in the Germany region or in {{site.data.keyword.Bluemix_notm}} Dedicated.
-->

## MQ Light API의 개념 및 차이점
{: #mqlight}

<!-- 30/10/18: info was in eventstreams106.md, moved because of doc app changes -->

{{site.data.keyword.mql}} API에서는 Kafka와 비교하여 메시징에 대해 더 상위 레벨의 추상을 제공합니다.

Kafka 클라이언트 또는 {{site.data.keyword.mql}} API 사용 중에서 선택하는 것은
빌드하려는 메시징 토폴로지에 좌우됩니다.

* Kafka로 적은 수의 토픽을 사용하여 추가 확장성을 위해 각 토픽에 여러 개의 파티션이 생기도록 할 수 있습니다. 이용자 그룹을 사용하여 이용자들 간에서 메시지를 공유할 수 있지만, 각 이용자가 지정된 파티션에 대한 메시지의 비율과 맞출 수 있어야 합니다.
* {{site.data.keyword.mql}} API로 훨씬 더 많은 수의 토픽을 사용할 수 있으며 토픽 이름은 계층 구조(예: <code>&lsquo;/sports/football&rsquo;</code> 및 <code>&lsquo;/sports/tiddlywinks&rsquo;</code>)입니다.  

{{site.data.keyword.mql}} API의 토픽은 Kafka 토픽과 같지 않습니다. 그 대신, {{site.data.keyword.mql}} API는
"MQLight"로 이름 지정된 단일 Kafka 토픽을 사용하며 {{site.data.keyword.mql}} API를 사용하여 보내고 받은 모든 메시지가 그 하나의 Kafka 토픽을 통해 이동합니다.

{{site.data.keyword.mql}}은 댈러스(us-south), 런던(eu-gb) 및 시드니(au-syd)와 같은
{{site.data.keyword.Bluemix_notm}} 위치(지역)에서만 사용 가능합니다. MQ Light API는 프랑크프루트(eu-de) 지역 또는
{{site.data.keyword.Bluemix_notm}} 데디케이티드에서는 사용할 수 없습니다.

API 중에서의 선택에 대한 자세한 정보는 [세 개의 API 중에서 선택](/docs/services/EventStreams/eventstreams087.html)을 참조하십시오.


## {{site.data.keyword.messagehub}}에서 MQ Light API를 사용하는 데 필요한 사항
{: #mql_reqs}

<!-- 30/10/18: info was in eventstreams098.md, moved because of doc app changes -->

{{site.data.keyword.messagehub}}에서 {{site.data.keyword.mql}} API를 사용하려면 다음 요구사항이 필요합니다. 

**모든 메시지가 "MQLight" 토픽을 통해서 이동하므로 "MQLight"로 이름 지정된 Kafka 토픽을 우선 명시적으로 작성해야 API를 사용할 수 있습니다. 이 토픽에는 단일 파티션이 있어야 합니다. 이 토픽을 작성하면 사용자의 서비스 인스턴스에 대해 MQ Light API를 사용할 수 있습니다. MQ Light API에서 사용된 토픽은 해당 토픽을 사용할 때 자동으로 작성되지만 실제로 모든 메시지가 단일 "MQLight" Kafka 토픽에 있습니다.** 

"MQLight" 토픽은 그 메시지 데이터를 저장하고 다른 Kafka 클라이언트와 상호작용하기 위해 MQ Light API에서 사용됩니다. 이 토픽이
작성되면 서비스 결제 플랜에 개요가 나와 있는 표준 비율대로 비용이 발생한다는 점에 주의하십시오.

MQ Light API를 사용 안함으로 설정하려면 "MQLight" 토픽을 삭제하십시오. 토픽을 삭제하면 모든 데이터가 영구 삭제됨에 유의하십시오.

<!-- 12/11/18: following info was in eventstreams079.md, moved because of doc app changes -->

## 연결 및 인증 방법
{: #mql_connect}

앱을 서비스에 연결하려면 앱은 [VCAP_SERVICES 환경 변수](/docs/services/EventStreams/eventstreams127.html)에서 <code>user</code>,
<code>password</code> 및 <code>mqlight_lookup_url</code> 세부사항을 사용해야 합니다. 사용자가 선택한 언어에 대한 다음 안내를 사용하십시오.

**Java의 경우**

create() 호출의 endpointService 매개변수로 &lsquo;null&rsquo;을 지정하는 경우,
이는 클라이언트가 <code>user</code>, <code>password</code> 및 
<code>mqlight_lookup_url</code> 세부사항을 VCAP_SERVICES에서 읽도록 지시합니다.

<pre>
<code>NonBlockingClient.create(null, new NonBlockingClientAdapter<Void>() {
    public void onStarted(NonBlockingClient client, Void context) {
        client.send("my/topic", "Hello World!", null);
    }
}, null);</code>
</pre>
{:codeblock}

<br>

**Node.js의 경우**

<code>user</code>, <code>password</code> 및
<code>mqlight_lookup_url</code> 세부사항을 VCAP_SERVICES에서 검색하고 그것을 사용하여 다음과 같이 클라이언트를 작성하십시오.

<pre>
<code>var services = JSON.parse(process.env.VCAP_SERVICES);
mqlightService = services['messagehub'][0];
opts.service = mqlightService.credentials.mqlight_lookup_url;
opts.user = mqlightService.credentials.user;
opts.password = mqlightService.credentials.password;
var mqlightClient = mqlight.createClient(opts, function(err) {
...</code>
</pre>
{:codeblock}

<br>

**Ruby의 경우**

<code>user</code>, <code>password</code> 및
<code>mqlight_lookup_url</code> 세부사항을 VCAP_SERVICES에서 검색하고 그것을 사용하여 다음과 같이 클라이언트를 작성하십시오.
<pre>
<code>vcap_services = JSON.parse(ENV['VCAP_SERVICES'])
conn_details = vcap_services['messagehub']
credentials = conn_details.first['credentials']
service = credentials['mqlight_lookup_url']
opts[:user] = credentials['user']
opts[:password] = credentials['password']
set :client, Mqlight::BlockingClient.new(service, opts)
...</code>
</pre>
{:codeblock}

<br>

**Python의 경우**

<code>user</code>, <code>password</code> 및
<code>mqlight_lookup_url</code> 세부사항을 VCAP_SERVICES에서 검색하고 그것을 사용하여 다음과 같이 클라이언트를 작성하십시오.
<pre>
<code>vcap_services = json.loads(os.environ.get('VCAP_SERVICES'))
conn_details = vcap_services['messagehub'][0]
service = str(conn_details['credentials']['mqlight_lookup_url'])
security_options = {
      'user': str(conn_details['credentials']['user']),
      'password': str(conn_details['credentials']['password'])
}
client = mqlight.Client(service=service, 
                        security_options=security_options,
                        on_started=on_started)</code>
</pre>
{:codeblock}

<br>

{{site.data.keyword.mql}} API에 대한 자세한 정보는
[{{site.data.keyword.mql}} developerWorks&reg; 사이트 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.ibm.com/messaging/mq-light/){:new_window}을 참조하십시오.


<!-- 14/11/18: info was in eventstreams080.md, moved because of doc app changes -->

## 기존 MQ Light 애플리케이션 연결 방법
{: #mql_exist_apps}


{{site.data.keyword.IBM_notm}} MQ 또는 {{site.data.keyword.mql}}
{{site.data.keyword.Bluemix_notm}} 서비스에 대해 현재 실행되는 기존 애플리케이션을 서비스에 연결할 수 있습니다. 앱은 같은 방법으로 계속 작업합니다.

기존 앱에 연결하려면 다음 확인사항을 완료하십시오.

* 앱이 사용자의 언어에 사용 가능한 최신 {{site.data.keyword.mql}} API 클라이언트 버전을 사용 중인지 확인하십시오.
* VCAP_SERVICES에서 추출된 연결 세부사항이 <code>messagehub</code> 서비스 유형을 참조하고, <code>credentials.username</code> 특성보다는 <code>credentials.user</code> 특성에서 연결 사용자 이름을 검색하고, <code>credentials.connectionLookupURI</code> 특성보다는 <code>credentials.mqlight_lookup_url</code> 특성에서 연결 검색 URL을 검색하는지 확인하십시오. 자세한 정보는 [VCAP_SERVICES 환경 변수](/docs/services/EventStreams/eventstreams127.html)를 참조하십시오.

	Java&trade; 클라이언트를 사용하는 경우 이 단계가 완료되도록 하고 클라이언트가 스스로 정보를 검색하도록 create() 호출에서 'null'을 endpointService 매개변수로 지정하십시오.
	
* 사용자의 앱이 TLS v1.2 연결을 지원해야 합니다. VCAP_SERVICES에는 비TLS 연결 작성을 위한 <code>credentials.nonTLSConnectionLookupURI</code> 특성이 더 이상 포함되지 않습니다.

다음 정보에도 주의해야 합니다.

* 메시지 한계는 {{site.data.keyword.messagehub}}와 일치하지만 {{site.data.keyword.mql}} API를 지원하는 다른 서버와는 다를 수 있습니다. 자세한 정보는 [최대 한계](/docs/services/EventStreams/eventstreams075.html#max_limits)를 참조하십시오.
* JMS가 지원되지 않습니다.

<!-- 15/11/18: info was in eventstreams081.md, moved because of doc app changes -->

## MQ Light API 및 Kafka 또는 Kafka REST API 간의 메시지 교환
{: #mql_exchange}

{{site.data.keyword.mql}} 메시지는 "MQLight"로 이름 지정된 하나의 기본 Kafka 토픽에 저장되며 다음 표에 세부사항이 나와 있는대로 인코딩됩니다. 이 인코딩은 {{site.data.keyword.mql}} API를 사용하여 애플리케이션과 메시지를 교환하기 위해 다른 API 유형(예: Kafka 또는 Kafka REST)에서도 사용될 수 있습니다.

### Kafka 메시지 형식
{: #kafka_format notoc}

<table border='1'>
<caption>표 1. Kafka 메시지 형식</caption>
  <tr>
    <th> 키 </th>
    <th> 값 </th>
  </tr>
  <tr>
    <td> 선택사항(API에서 사용되지 않음)
	<p></p>
	</td>
    <td>**1바이트**
	<p>		     MQ Light API eye catcher이며, 항상 0xFA입니다.</p>
    <p><var class="keyword varname">**n**</var>**바이트**</p>
    <p>		    AMQP 인코딩된 메시지(AMQP 연결 형식을 기반으로 형식화됨)입니다. </p></td>
  </tr>
</table>


<!-- 15/11/18: info was in eventstreams082.md, moved because of doc app changes -->

## MQ Light API 샘플
{: #mql_samples}

앱이 아직 없는 경우, 샘플 중 하나로 {{site.data.keyword.mql}} API를 사용해 보십시오.

샘플 앱은 실제로 두 개의 단순한 앱으로 구성됩니다. 백엔드에 메시지를 보내는
웹 프론트 엔드와 그 메시지를 처리하고, 단어를 대문자화해서 그 메시지를 프론트 엔드에 리턴하는
백엔드입니다. 이 샘플은 {{site.data.keyword.mql}} API를 사용하여 앱이 서로
대화하도록 하는 방법을 보여줍니다. 또한, {{site.data.keyword.mql}} API를 사용하여
확장 가능하고 느슨하게 결합된 분산 앱 빌드에 필요한 키 기능인 작업자 오프로드를 수행할 수 있습니다.

[event-streams-samples GitHub 프로젝트 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-messaging/event-streams-samples/tree/master/mqlight){:new_window}에서 샘플 코드를 찾을 수 있습니다.


<!-- 15/11/18: info was in eventstreams083.md, moved because of doc app changes -->

## 최대 한계
{: #max_limits}

{{site.data.keyword.mql}} API에 대해 다음과 같은 한계가 적용됩니다.

* 저장될 수 있는 최대 데이터 양은 사용자의 결제 플랜에 대한 단일 Kafka 파티션과 일치합니다(일반적으로 1GB). 이 데이터 한계를 초과하는 경우, 새 메시지가 전송되면 파티션에서 가장 오래된 메시지가 제거됩니다.
* 메시지가 저장되는 최대 시간은 사용자의 결제 플랜에 대한 단일 Kafka 파티션과 일치합니다(일반적으로 24시간). 이 기간보다 더 이전의 메시지는 검색할 수 없습니다.
* 메시지의 최대 크기(헤더 제외)는 1MB입니다.
* 한 번에 연결할 수 있는 클라이언트의 최대수는 25입니다.
* 한 번에 활성 상태일 수 있는 대상의 최대수는 25입니다. 활성 대상은 다음과 같이 정의됩니다.
  - 클라이언트가 현재 연결되어 있거나 연결되지 않은 TimeToLive > 0인 대상입니다.
  - 클라이언트가 연결되는 TimeToLive = 0(기본값)인 대상입니다. 
  <p>클라이언트 간에 공유되는 대상은 하나의 대상으로 간주합니다.</p>
