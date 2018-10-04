---

copyright:
  years: 2015, 2018
lastupdated: "2018-07-04"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.messagehub}} 리소스에 대한 액세스 관리(엔터프라이즈 플랜)
{: #security }

각 리소스에 대해 각 사용자에게 부여할 액세스 권한을 관리하는 위한 세부적인 방식으로 {{site.data.keyword.messagehub}} 리소스의 보안을 설정할 수 있습니다.

## 어떻게 보안을 설정할 수 있습니까?

{{site.data.keyword.messagehub}} 내에서 다음 리소스에 대한 액세스의 보안을 설정할 수 있습니다.
* 클러스터(cluster): 서비스에 연결할 수 있는 애플리케이션 및 사용자를 제어할 수 있습니다.
* 토픽(topic): 사용자 및 애플리케이션이 토픽을 작성, 삭제, 읽기 및 쓸 수 있는 기능을 제어할 수 있습니다. 
* 이용자 그룹(group): 이용자 그룹에 참여할 수 있는 애플리케이션의 기능을 제어할 수 있습니다. 
* 제작자 트랜잭션(txnid): Kafka에서 트랜잭션 제작자 기능을 사용할 수 있는 기능을 제어할 수 있습니다(즉, 다중 파티션 전체에서 한 번의 원자적 쓰기).

각 리소스에 대해 사용자에게 지정할 수 있는 액세스의 레벨(역할이라고도 함)은 다음과 같습니다.

| 액세스 역할 |조치 설명 | 조치 예제 |
|:-----------------|:-----------------|:-----------------|
| 독자 | 리소스 보기와 같이 {{site.data.keyword.messagehub}} 내의 읽기 전용 조치를 수행합니다. | 클러스터 리소스 유형에 대한 읽기 액세스 권한을 지정하여 앱이 클러스터에 연결하도록 허용합니다. |
| 작성자 | 작성자는 {{site.data.keyword.messagehub}} 리소스 작성 및 편집을 포함하여 독자 역할 이상의 권한을 보유합니다. | 토픽 리소스 및 토픽 이름 유형에 대한 쓰기 액세스 권한을 지정하여 앱이 토픽을 생성하도록 허용합니다.|
| 관리자 | 관리자는 권한 부여된 조치를 완료하기 위해 작성자 역할 이상의 권한을 보유합니다. 또한 {{site.data.keyword.messagehub}} 리소스를 작성하고 편집할 수 있습니다. | {{site.data.keyword.messagehub}} 인스턴스에 대한 관리 액세스 권한을 지정하여 모든 리소스에 대한 전체 액세스를 허용합니다.|
{: caption="표 1. {{site.data.keyword.messagehub}} 사용자 역할 및 조치 예제" caption-side="top"}

<!-- comment from Charlie and my reply 
CM: need to confirm if hierarchical e.g. write includes read - and doc. 
KR: I think they do inherit the lower level access https://console.bluemix.net/docs/iam/users_roles.html#iamusermanrol 
-->


## 액세스 권한을 지정할 수 있는 방법은 무엇입니까?

Cloud IAM(Identity and Access Management) 정책이 제어될 리소스에 첨부되어 있습니다. 각 정책은 특정 사용자가 보유할 액세스 레벨 및 리소스 또는 리소스 세트를 정의합니다. 정책은 다음 정보로 구성됩니다. 
* 정책이 적용할 서비스의 유형입니다. 예를 들면 {{site.data.keyword.messagehub}}가 있습니다. 모든 서비스 유형을 포함하도록 정책의 범위를 설정할 수 있습니다. 
* 보안을 설정할 서비스의 인스턴스입니다. 서비스 유형의 모든 인스턴스를 포함하도록 정책의 범위를 설정할 수 있습니다. 
* 보안을 설정할 리소스의 유형입니다. 올바른 값은 <code>cluster</code>, <code>topic</code>, <code>group</code> 또는 <code>txnid</code>입니다. 유형 지정은 선택사항입니다. 유형을 지정하지 않으면 정책은 서비스 인스턴스의 모든 리소스에 적용됩니다. 
* 보안을 설정할 리소스입니다. <code>topic</code>, <code>group</code> and <code>txnid</code> 유형의 리소스에 지정하십시오. 리소스를 지정하지 않으면 정책은 서비스 인스턴스에 지정된 유형의 모든 리소스에 적용됩니다. 
* 사용자에게 지정된 역할입니다. 예를 들면, 독자, 작성자 또는 관리자입니다. 

## 기본 보안 설정은 무엇입니까?

기본적으로, {{site.data.keyword.messagehub}}가 프로비저닝되는 경우 프로비저닝한 사용자에게는 모든 인스턴스의 리소스에 대한 관리자 역할이 부여됩니다. 또한 동일한 계정에서 '모든' 서비스 또는 '모든' {{site.data.keyword.messagehub}} 서비스 인스턴스에 대한 관리자 역할을 보유한 사용자에게도 전체 액세스 권한이 있습니다.  

그런 다음 추가 정책을 적용하여 다른 사용자에 대한 액세스를 확장할 수 있습니다. 전체로 {{site.data.keyword.messagehub}}에 적용하거나 또는 {{site.data.keyword.messagehub}} 내의 개별 리소스에 적용하도록 정책의 범위를 설정할 수 있습니다. 자세한 정보는 [일반 시나리오](#security_scenarios)를 참조하십시오.

계정에 대한 관리 권한이 있는 사용자만 사용자에게 정책을 지정할 수 있습니다. IBM Cloud 대시보드를 사용하거나 **ibmcloud** 명령을 사용하여 정책을 지정하십시오. 
<!--
For example steps for {{site.data.keyword.messagehub}}, see [Examples](#security_examples).
-->


## 일반 시나리오
{: #security_scenarios }

다음 표에는 몇 가지 일반 {{site.data.keyword.messagehub}} 시나리오와 사용자가 지정해야 하는 액세스 권한이 요약되어 있습니다.

| 조치 |독자 역할 | 작성자 역할 | 관리자 역할 |
|---------|----------------|
| 모든 리소스에 대한 전체 액세스 허용|해당 사항 없음   |해당 사항 없음  |서비스 인스턴스: <var class="keyword varname">your_service_instance</var>|
| 앱 또는 사용자가 토픽을 작성하거나 삭제할 수 있음 |리소스 유형: <code>cluster</code>   |해당 사항 없음  |리소스유형: topic <br/><br/>선택사항: 리소스 ID: <var class="keyword varname">name_of_topic</var> |
| 그룹, 토픽 및 오프셋 나열 <br/> 그룹, 토픽 및 브로커 구성 설명 |리소스 유형: <code>cluster</code>      |해당 사항 없음  |해당 사항 없음      |
| 앱이 클러스터에 연결할 수 있음  |리소스 유형: <code>cluster</code>|해당 사항 없음     |해당 사항 없음      |
| 앱이 임의의 토픽을 생성할 수 있음  |리소스 유형: <code>cluster</code>|리소스 유형: <code>topic</code> |해당 사항 없음     |
| 앱이 특정 토픽을 생성할 수 있음  |리소스 유형: <code>cluster</code>|리소스 유형: <code>topic</code><br/>리소스 ID: <var class="keyword varname">name_of_topic</var>      |해당 사항 없음     |
| 앱이 임의의 토픽에서 연결하고 이용할 수 있음(이용자 그룹 없음)  |리소스 유형: <code>cluster</code> <br/>리소스 유형: <code>topic</code> |리소스 유형: <code>topic</code>|해당 사항 없음     |
| 앱이 특정 토픽에서 연결하고 이용할 수 있음(이용자 그룹 없음)  |리소스 유형: <code>cluster</code> <br/>리소스 유형: <code>topic</code><br/>리소스 ID: <var class="keyword varname">name_of_topic</var> |해당 사항 없음     |해당 사항 없음     |
| 앱이 토픽을 이용할 수 있음(이용자 그룹)  |리소스 유형: <code>cluster</code> <br/>리소스 유형: <code>topic</code><br/> 리소스 유형: <code>group</code> |해당 사항 없음      |해당 사항 없음     |
| 앱이 트랜잭션적으로 토픽을 생성할 수 있음  |리소스 유형: <code>cluster</code> <br/> 리소스 유형: <code>group</code>|리소스 유형: <code>topic</code> <br/>리소스 ID: <var class="keyword varname">name_of_topic</var> <br/>리소스 유형: <code>txnid</code> |해당 사항 없음     |
| 이용자 그룹 삭제 |리소스 유형: <code>cluster</code> |해당 사항 없음  |리소스 유형: <code>group</code> <br/>리소스 ID: <var class="keyword varname">group_ID</var>      |
| Streams 사용 목적 |리소스 유형: <code>cluster</code></br>리소스 유형: <code>group</code>|해당 사항 없음  |리소스 유형: <code>topic</code>    |

IAM에 대한 자세한 정보는
[IBM Cloud Identity and Access Management](/docs/iam/index.html#iamoverview)를 참조하십시오.

정책을 설정하는 방법에 대한 예는
[IBM Cloud IAM 서비스 ID 및 API 키 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/blogs/bluemix/2017/10/introducing-ibm-cloud-iam-service-ids-api-keys/){:new_window}를 참조하십시오.


## {{site.data.keyword.messagehub}}에 연결
{: #connect_message_enterprise }

Cloud Foundry 애플리케이션 바인드 방법 또는 외부 애플리케이션에 대한 보안 키 인증 정보 가져오기에 대한 정보는
[{{site.data.keyword.messagehub}}에 연결](/docs/services/EventStreams/eventstreams127.html#connect_messagehub)을 참조하십시오. 

<!-- 28/06/18 - Karen: draft info only

## Examples
{: #security_examples }

I want to give a user access to create or delete a topic:

1. From the IBM Cloud dashboard, go to the **Manage** tab &gt; **Security** &gt; **Identity and Access**, and then select **Users**.
2. Click **Invite users**.
3. Specify the email address of the user that you want to invite.
4. In the **Access** section, expand the **Services** option.
5. Choose to assign access to a **Resource**.
6. In the **Services** section, select **{{site.data.keyword.messagehub}}**
7. In the **Region** section, make your selection.
8. In the **Service instance** section, locate your instance and select it.
9. In the **Resource type** section, enter **cluster**.
10. In the **Select roles** section, check the **Reader** box.
11. In the **Resource type** section, enter **topic**.
12. In the **Select roles** section, check the **Manager** box.
13. Click **Invite users**.

-->















