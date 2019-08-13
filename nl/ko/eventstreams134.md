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
{:table: .aria-labeledby="caption"}

# 클래식 플랜 
{: #plan_choose_classic}

{{site.data.keyword.messagehub}}는 요구사항에 따라 여러 플랜으로 사용 가능합니다. 표준 및 엔터프라이즈 플랜에 대한 정보는 [플랜 선택](/docs/services/EventStreams?topic=eventstreams-plan_choose#plan_choose)을 참조하십시오.
{: shortdesc}
 
## 클래식 플랜 개요
클래식 플랜은 이벤트 삽입 및 분배 기능이 필요하지만 엔터프라이즈 플랜의 추가 기능은 필요하지 않은 경우 적합합니다. 클래식 플랜은 멀티 테넌트 {{site.data.keyword.messagehub}} 클러스터에 대한 공유 액세스를 제공합니다.


## 클래식 플랜에서 지원되는 항목

다음 표에는 플랜에서 지원되는 항목이 요약되어 있습니다.

<table>
    <caption>표 1. 클래식 플랜에서의 지원</caption>
      <tr>
	        <th></th>
		    <th>클래식 플랜</th>
        </tr>
		<tr>
			<td>**테넌시**</td>
			<td>멀티 테넌트 </td>
		</tr>
        <tr>
			<td>**가용성 구역**</td>
			<td>지원되지 않음</td>
		</tr>
        <tr>
			<td>**가용성**</td>
			<td>99.5%</td>
		</tr>
	  		<tr>
			<td>**클러스터의 Kafka 버전**</td>
			<td>Kafka 1.1</td>
		</tr>
		<tr>
			<td>**액세스 권한 세부 제어**</td>
			<td>아니오</td>
		</tr>
				<tr>
			<td>**클라우드 서비스 엔드포인트 지원**</td>
			<td>아니오</td>
		</tr>
		<tr>
			<td>**지원되는 Kafka Connect 및 Kafka Streams **</td>
			<td>예</td>

		</tr>
		<tr>
			<td>**최대 파티션 수**</td>
			<td>100</td>

		</tr>
		<tr>
			<td>**최대 보존 기간**</td>
			<td>최대 30일 동안 파티션당 1GB </td>

		</tr>
		<tr>
			<td>**최대 처리량**</td>
			<td>파티션셜 초당 1MB</td>
		</tr>
		<tr>
			<td>**최대 메시지 크기**</td>
			<td>1MB</td>
		</tr>
		<tr>
			<td>**위치(지역) 가용성**</td>
			<td>댈러스(us-south)</br>
			런던(eu-gb)</br>
			시드니(au-syd)</br>
			프랑크푸르트(eu-de) - {{site.data.keyword.mql}} API가 없음 </td>

		</tr>
		<tr>
     	    <td>**지원되는 API**</td>
			<td>Kafka API</br>
			관리 REST API<br/>
			Kafka REST API</br>
			MQ Light API</br>
		    </td>
		</tr>
			<td>**Cloud Object Storage 브릿지 및<br/>
			MQ 브릿지가 지원됨**</td>
			<td>예</td>
		</tr>
		<tr>
			<td>**배치 시간 범위**</td>
			<td>즉시 프로비저닝</td>
		</tr>

</table>

