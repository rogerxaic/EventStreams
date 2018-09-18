---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-19"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.iamshort}}(IAM)를 Alpha 프로그램과 함께 사용
{: #alpha_iam }

{{site.data.keyword.messagehub}} 권한은 IAM 정책을 사용하여 구성됩니다. IAM 정책은 다음 정보로 구성됩니다. 

* 서비스 ID
* 서비스 이름, 서비스 인스턴스, 지역, IAM 리소스 유형 및 IAM 리소스로 정의된 {{site.data.keyword.Bluemix_short}} 리소스
* 역할(독자, 작성자 또는 관리자)

IAM에 대한 자세한 정보는
[IBM Cloud Identity and Access Management](/docs/iam/index.html#iamoverview)를 참조하십시오. 

정책을 설정하는 방법에 대한 예는
[IBM Cloud IAM 서비스 ID 및 API 키 소개 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/blogs/bluemix/2017/10/introducing-ibm-cloud-iam-service-ids-api-keys/){:new_window}를 참조하십시오. 

## {{site.data.keyword.messagehub}} IAM 리소스
{: iam_resources}

{{site.data.keyword.messagehub}}는 네 가지 IAM 리소스 유형을 노출시킵니다. 

* 클러스터
* 토픽
* 그룹
* txnid

IAM 리소스를 설정하여 토픽, 그룹 및 txnid에 대해 고유한 리소스를 식별할 수 있습니다. 클러스터 리소스 유형은 싱글톤이므로 고유하게 식별할 필요가 없습니다. 

## 일반 시나리오
{: iam_scenarios}

다음은 몇 가지 일반 {{site.data.keyword.messagehub}} 시나리오와 사용자가 지정해야 하는 액세스 권한입니다. 

일부 토픽에 대한 제작자(멱등의 경우에도 동일):
* 클러스터 리소스 유형에 대한 독자 역할
* 각 토픽 리소스 유형 및 토픽 이름 리소스에 대한 작성자 역할

트랜잭션 제작자:
* 제작자와 동일
* txnid 리소스 유형 및 트랜잭션 ID 리소스에 대한 작성자 역할
* 그룹 리소스 유형 및 그룹 ID 리소스에 대한 독자 역할

이용자(그룹 없음):
* 클러스터 리소스 유형에 대한 독자 역할
* 각 토픽 리소스 유형 및 토픽 이름 리소스에 대한 독자 역할

그룹이 있는 이용자:
* 이용자(그룹 없음)와 동일
* 그룹 리소스 유형 및 그룹 ID 리소스에 대한 독자 역할

토픽 작성 또는 삭제:
* 클러스터 리소스 유형에 대한 독자 역할
* 각 토픽 리소스 유형 및 토픽 이름 리소스에 대한 관리자 역할

이용자 그룹 삭제:
* 클러스터 리소스 유형에 대한 독자 역할
* 그룹 리소스 유형 및 그룹 ID 리소스에 대한 관리자 역할

그룹, 토픽, 오프셋 나열 및 그룹, 토픽, 브로커 구성 설명
* 클러스터 리소스 유형에 대한 독자 역할














