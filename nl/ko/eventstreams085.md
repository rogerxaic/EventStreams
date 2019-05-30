---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-16"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:note: .note}

# 플랜 선택 
{: #plan_choose}

{{site.data.keyword.messagehub}}는 사용자의 요구사항에 따라 세 가지 다른 플랜(표준, 엔터프라이즈 및 클래식)으로 사용 가능합니다.
 

<!--
For information about the Classic plan, see
[Classic plan](/docs/services/EventStreams?topic=eventstreams-plan_choose_classic#plan_choose_classic).
-->
{: shortdesc}

## 표준 플랜

표준 플랜은 이벤트 삽입 및 분배 기능이 필요하지만 엔터프라이즈 플랜의 추가 기능은 필요하지 않은 경우 적합합니다. 표준 플랜은 멀티 테넌트 {{site.data.keyword.messagehub}} 클러스터에 대한 공유 액세스를 제공합니다.

## 엔터프라이즈 플랜 

엔터프라이즈 플랜은 데이터 격리, 보장된 성능 및 늘어난 보존 기간이 중요한 고려사항인 경우 적합합니다. 엔터프라이즈 플랜은 전용 {{site.data.keyword.messagehub}} 클러스터에 대한 독점적 액세스를 제공합니다.

## 클래식 플랜

클래식 플랜은 표준 플랜의 이전 에디션에 대한 액세스를 제공하며 기존 워크로드 및 역호환성을 위해서만 제공됩니다. 표준 플랜에 대해 새 워크로드를 프로비저닝해야 합니다.


## 표준, 엔터프라이즈 및 클래식 플랜에서 지원되는 항목

다음 표에는 각 플랜의 지원 항목이 요약되어 있습니다.

<table>
    <caption>표 1. 표준, 엔터프라이즈 및 클래식 플랜에서의 지원</caption>
      <tr>
	        <th></th>
		    <th>표준 플랜</th>
		    <th>엔터프라이즈 플랜</th>
		    <th>클래식 플랜</th>
        </tr>
		<tr>
			<td>**테넌시**</td>
			<td>멀티 테넌트 </td>
			<td>싱글 테넌트</td>
			<td>멀티 테넌트</td>
		</tr>
        <tr>
			<td>**가용성 구역**</td>
			<td>3</td>
			<td>3</td>
			<td>지원되지 않음</td>
		</tr>
        <tr>
			<td>**가용성**</td>
			<td>99.95%</td>
			<td>99.95%</td>
			<td>99.5%</td>
		</tr>
	  		<tr>
			<td>**클러스터의 Kafka 버전**</td>
			<td>Kafka 2.2</td>
			<td>Kafka 1.1 <br/>(Kafka 2.2 서비스 예정)</td>
			<td>Kafka 1.1</td>
		</tr>
		<tr>
			<td>**액세스 권한 세부 제어**</td>
			<td>예</td>
			<td>예</td>
			<td>아니오</td>
		</tr>
				<tr>
			<td>**클라우드 서비스 엔드포인트 지원**</td>
			<td>아니오</td>
			<td>예</td>
			<td>아니오</td>
		</tr>
		<tr>
			<td>**지원되는 Kafka Connect 및 Kafka Streams **</td>
			<td>예</td>
			<td>예</td>
			<td>예</td>
		</tr>
		<tr>
			<td>**최대 파티션 수**</td>
			<td>100</td>
			<td>1000</td>
			<td>100</td>
		</tr>
		<tr>
			<td>**최대 보존 기간**</td>
			<td>최대 30일 동안 파티션당 1GB </td>
			<td>플랜의 스토리지 한계까지 제한 없음 </td>
			<td>최대 30일 동안 파티션당 1GB </td>
		</tr>
		<tr>
			<td>**최대 처리량**</td>
			<td>파티션별 초당 1MB(최대 초당 20MB)</td>
			<td>초당 40MB(최대 처리량은 초당 90MB)</td>
			<td>파티션셜 초당 1MB</td>
		</tr>
		<tr>
			<td>**최대 메시지 크기**</td>
			<td>1MB</td>
			<td>1MB</td>
			<td>1MB</td>
		</tr>
		<tr>
			<td>**위치(지역) 가용성**</td>
			<td>댈러스(us-south)</br>
 </td>
			<td>댈러스(us-south)</br>
			워싱턴(us-east)<br/>
			런던(eu-gb)<br/>
			시드니(au-syd)</br>
			프랑크푸르트(eu-de)<br/>
			도쿄(jp-tok)<br/>
			<br/>
			</td>
			<td>댈러스(us-south)</br>
			런던(eu-gb)</br>
			시드니(au-syd)</br>
			프랑크푸르트(eu-de) - {{site.data.keyword.mql}} API가 없음</td>
		</tr>
		<tr>
     	    <td>**지원되는 API**</td>
			<td>Kafka API</br>
			관리 REST API<br/>
			REST 제작자 API</br>
		    </td>
			<td>Kafka API<br/>
			관리 REST API</td>
			<td>Kafka API</br>
			관리 REST API<br/>
			Kafka REST API</br>
			MQ Light API</br>
		    </td>
		</tr>
		</tr>
			<td>**Cloud Object Storage 브릿지 및<br/>
			MQ 브릿지가 지원됨**</td>
			<td>아니오</td>
			<td>아니오</td>
			<td>예</td>
		</tr>
		<tr>
			<td>**배치 시간 범위**</td>
			<td>즉시 프로비저닝</td>
			<td>프로비저닝에 최대 3시간이 소요됩니다. 엔터프라이즈에는 각 클러스터마다 자체 전용 리소스가 있으므로 프로비저닝에 더 많은 시간이 필요합니다.</td>
			<td>즉시 프로비저닝</td>
		</tr>

</table>




<!--
## {{site.data.keyword.Bluemix_notm}} Public environment
{: notoc}

{{site.data.keyword.Bluemix_notm}} Public provides an
economical public cloud service where you pay for what you use and share infrastructure with
others.

In {{site.data.keyword.Bluemix_notm}} Public, the cost of
{{site.data.keyword.messagehub}} is determined by two factors: the
number of partitions that you use and the number of messages that you send and receive. There is no
charge for message data while it is retained on the topics, but the data that each partition retains
is capped at 1 GB.

For more information, see [{{site.data.keyword.Bluemix_notm}} Public ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud-computing/bluemix/public){:new_window}.
-->

