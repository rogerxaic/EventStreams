---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-31"
keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 플랜 선택 
{: #plan_choose}

{{site.data.keyword.messagehub}}는 사용자의 요구사항에 따라 두 가지 다른 플랜(표준 및 엔터프라이즈)으로 사용 가능합니다.
{: shortdesc}

## 표준 플랜

표준 플랜은 이벤트 삽입 및 분배 기능이 필요하지만 엔터프라이즈 플랜의 추가 기능은 필요하지 않은 경우 적합합니다. 표준 플랜은 멀티 테넌트 {{site.data.keyword.messagehub}} 클러스터에 대한 공유 액세스를 제공합니다.

## 엔터프라이즈 플랜 

엔터프라이즈 플랜은 데이터 격리, 보장된 성능 및 늘어난 보존 기간이 중요한 고려사항인 경우 적합합니다. 엔터프라이즈 플랜은 전용 {{site.data.keyword.messagehub}} 클러스터에 대한 독점적 액세스를 제공합니다.

## 표준 및 엔터프라이즈 플랜의 지원 항목

다음 표에는 각 플랜의 지원 항목이 요약되어 있습니다.

<table>
    <caption>표 1. 표준 및 엔터프라이즈 플랜에서의 지원</caption>
      <tr>
	        <th></th>
		    <th>표준 플랜</th>
		    <th>엔터프라이즈 플랜</th>
        </tr>
		<tr>
			<td>**테넌시**</td>
			<td>멀티 테넌트 </td>
			<td>싱글 테넌트</td>
		</tr>
        <tr>
			<td>**가용성 구역**</td>
			<td>지원되지 않음</td>
			<td>3</td>
		</tr>
	  		<tr>
			<td>**클러스터의 Kafka 버전**</td>
			<td>Kafka 1.1</td>
			<td>Kafka 1.1</td>
		</tr>
		<tr>
			<td>**액세스 권한 세부 제어**</td>
			<td>아니오</td>
			<td>예</td>
		</tr>
		<tr>
			<td>**지원되는 Kafka Connect 및 Kafka Streams **</td>
			<td>예</td>
			<td>예</td>
		</tr>
		<tr>
			<td>**최대 파티션 수**</td>
			<td>100</td>
			<td>1000</td>
		</tr>
		<tr>
			<td>**최대 보존 기간**</td>
			<td>최대 30일 동안 파티션당 1GB </td>
			<td>플랜의 스토리지 한계까지 제한 없음 </td>
		</tr>
		<tr>
			<td>**위치(지역) 가용성**</td>
			<td>댈러스(us-south)</br>
			런던(eu-gb)</br>
			시드니(au-syd)</br>
			프랑크푸르트(eu-de) - {{site.data.keyword.mql}} API가 없음 </td>
			<td>댈러스(us-south)</br>
			워싱턴(us-east)<br/>
			런던(eu-gb)<br/>
			시드니(au-syd)</br>
			프랑크푸르트(eu-de)<br/>
			도쿄(jp-tok)<br/>

			<br/>
			</td>
		</tr>
		<tr>
     	    <td>**지원되는 API**</td>
			<td>Kafka API</br>
			관리 REST API<br/>
			Kafka REST API</br>
			MQ Light API</br>
		    </td>
			<td>Kafka API<br/>
			관리 REST API</td>
		</tr>
			<td>**지원되는 Cloud Object Storage 브릿지 및<br/>
			MQ 브릿지**</td>
			<td>예</td>
			<td>아니오</td>
		</tr>
		<tr>
			<td>**배치 시간 범위**</td>
			<td>즉시 프로비저닝</td>
			<td>프로비저닝에 최대 3시간 소요</td>
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

