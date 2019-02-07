---

copyright:
  years: 2015, 2019
lastupdated: "2018-07-04"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.messagehub}} リソースへのアクセス管理 (エンタープライズ・プラン)
{: #security }

{{site.data.keyword.messagehub}} リソースの微細な保護により、各ユーザーに付与する各リソースへのアクセス権限を管理することができます。
{: shortdesc}

## 保護できる内容

{{site.data.keyword.messagehub}} 内で、以下のリソースへのアクセスを保護できます。
* クラスター (cluster): どのアプリケーションおよびユーザーがサービスに接続できるのかを制御できます
* トピック (topic): ユーザーおよびアプリケーションが、トピックの作成、削除、読み取り、および書き込みを行えるかどうかを制御できます 
* コンシューマー・グループ (group): アプリケーションがコンシューマー・グループに参加できるかどうかを制御できます 
* プロデューサー・トランザクション (txnid): Kafka のトランザクション・プロデューサー機能 (つまり、複数のパーティションにまたがる単一のアトミックな書き込み) を使用できるかどうかを制御できます

各ユーザーに割り当てることのできる各リソースへのアクセスのレベル (役割とも呼ばれる) は次のとおりです。

| アクセス役割 | アクションの説明 | アクションの例 |
|:-----------------|:-----------------|:-----------------|
|  リーダー | リソースの表示など、{{site.data.keyword.messagehub}} 内で読み取り専用アクションを実行する | cluster リソース・タイプへの読み取りアクセス権限の割り当てによって、アプリがクラスターに接続するのを許可する |
| ライター | ライターは、{{site.data.keyword.messagehub}} リソースの作成および編集など、リーダー役割を超える権限を持ちます。 | topic リソース・タイプおよびトピック名タイプへの書き込みアクセス権限の割り当てによって、アプリがトピックにプロデュースするのを許可する|
| 管理者 | 管理者は、特権付きアクションを実行する、ライター役割を超えるアクセス権を持つ。 それに加え、{{site.data.keyword.messagehub}} リソースを作成および編集できます。 | {{site.data.keyword.messagehub}} インスタンスへの管理アクセス権限の割り当てによって、すべてのリソースへの全アクセス権限を許可する|
{: caption="表 1. {{site.data.keyword.messagehub}} の役割とアクションの例" caption-side="top"}

<!-- comment from Charlie and my reply 
CM: need to confirm if hierarchical e.g. write includes read - and doc. 
KR: I think they do inherit the lower level access https://console.bluemix.net/docs/iam/users_roles.html#iamusermanrol 
-->


## アクセス権限の割り当て方法

制御されるリソースに、Cloud ID およびアクセス (IAM) ポリシーが関連付けられます。 各ポリシーは、ある特定のユーザーがどのリソース (またはリソースの集合) に対してどのレベルのアクセス権限を持つ必要があるのかを定義します。 ポリシーは、以下の情報から構成されます。 
* ポリシーが適用されるサービスのタイプ。 例えば、{{site.data.keyword.messagehub}} です。 ポリシーの適用範囲にすべてのサービス・タイプを含めることができます。 
* 保護されるサービスのインスタンス。 ポリシーの適用範囲に、あるサービス・タイプのすべてのインスタンスを含めることができます。 
* 保護されるリソースのタイプ。 有効な値は、<code>cluster</code>、<code>topic</code>、<code>group</code>、または <code>txnid</code> です。 タイプの指定はオプションです。 タイプを指定しないと、ポリシーはサービス・インスタンス内のすべてのリソースに適用されます。 
* 保護されるリソース。 タイプが <code>topic</code>、<code>group</code>、および <code>txnid</code> のリソースの場合に指定します。 リソースを指定しないと、ポリシーはサービス・インスタンス内の指定されたタイプのすべてのリソースに適用されます。 
* ユーザーに割り当てられた役割。 例えば、リーダー、ライター、または管理者です。 

## デフォルトのセキュリティー設定について

デフォルトでは、{{site.data.keyword.messagehub}} がプロビジョンされるときに、プロビジョンを行ったユーザーに、そのインスタンスのすべてのリソースに対する管理者役割が付与されます。 さらに、同じアカウントの「すべて」のサービスまたは「すべて」の {{site.data.keyword.messagehub}} サービス・インスタンスのいずれかに対する管理者役割を持つユーザーも、全アクセス権限を持ちます。 

追加ポリシーを適用して、他のユーザーにアクセス権限を拡張できます。 ポリシーの適用範囲を、{{site.data.keyword.messagehub}} の全体とするか、または、{{site.data.keyword.messagehub}} 内の個別リソースにすることができます。 詳しくは、[一般的なシナリオ](#security_scenarios)を参照してください。

アカウントの管理役割を持つユーザーのみが、ポリシーをユーザーに割り当てることができます。 ポリシーを割り当てるには、IBM Cloud ダッシュボードを使用するか、**ibmcloud** コマンドを使用します。 
<!--
For example steps for {{site.data.keyword.messagehub}}, see [Examples](#security_examples).
-->


## 一般的なシナリオ
{: #security_scenarios }

次の表に、いくつかの一般的な {{site.data.keyword.messagehub}} シナリオと、割り当てる必要があるアクセス権限の要約を示します。

| アクション | リーダー役割 | ライター役割 | 管理者役割 |
|---------|----------------|
| すべてのリソースへの全アクセス権限を許可する|適用外   |適用外  |サービス・インスタンス: <var class="keyword varname">your_service_instance</var>|
| アプリまたはユーザーがトピックの作成または削除を行うのを許可する |リソース・タイプ: <code>cluster</code>   |適用外  |リソース・タイプ: topic <br/><br/>オプション: リソース ID: <var class="keyword varname">name_of_topic</var> |
| グループ、トピック、およびオフセットをリストする <br/> グループ、トピック、およびブローカーの構成を記述する | リソース・タイプ: <code>cluster</code>      |適用外  |適用外      |
| アプリがクラスターに接続するのを許可する  |リソース・タイプ: <code>cluster</code>| 適用外     |適用外      |
| アプリが任意のトピックにプロデュースするのを許可する  |リソース・タイプ: <code>cluster</code>|リソース・タイプ: <code>topic</code> |適用外     |
| アプリが特定のトピックにプロデュースするのを許可する  |リソース・タイプ: <code>cluster</code>|リソース・タイプ: <code>topic</code><br/>リソース ID: <var class="keyword varname">name_of_topic</var>      |適用外     |
| アプリが任意のトピックに接続してそこからコンシュームするのを許可する (コンシューマー・グループなし)  |リソース・タイプ: <code>cluster</code> <br/>リソース・タイプ: <code>topic</code> |リソース・タイプ: <code>topic</code>|適用外     |
| アプリが特定のトピックに接続してそこからコンシュームするのを許可する (コンシューマー・グループなし)  | リソース・タイプ: <code>cluster</code> <br/>リソース・タイプ: <code>topic</code><br/>リソース ID: <var class="keyword varname">name_of_topic</var> |適用外     |適用外     |
| アプリがトピックをコンシュームするのを許可する (コンシューマー・グループ)  |リソース・タイプ: <code>cluster</code> <br/>リソース・タイプ: <code>topic</code><br/> リソース・タイプ: <code>group</code> |適用外      |適用外     |
| アプリが一時的にトピックにプロデュースするのを許可する  |リソース・タイプ: <code>cluster</code> <br/> リソース・タイプ: <code>group</code>|リソース・タイプ: <code>topic</code> <br/>リソース ID: <var class="keyword varname">name_of_topic</var> <br/>リソース・タイプ: <code>txnid</code> |適用外     |
| コンシューマー・グループを削除する |リソース・タイプ: <code>cluster</code> |適用外  |リソース・タイプ: <code>group</code> <br/>リソース ID: <var class="keyword varname">group_ID</var>      |
| Streams を使用する |リソース・タイプ: <code>cluster</code></br>リソース・タイプ: <code>group</code>| 適用外  |リソース・タイプ: <code>topic</code>    |

IAM について詳しくは、[IBM Cloud Identity and Access Management](/docs/iam/index.html#iamoverview) を参照してください。

ポリシーの設定方法に関する例については、[IBM Cloud IAM Service IDs and API Keys ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/blogs/bluemix/2017/10/introducing-ibm-cloud-iam-service-ids-api-keys/){:new_window} を参照してください。


## {{site.data.keyword.messagehub}} への接続
{: #connect_message_enterprise }

Cloud Foundry アプリケーションのバインド方法または外部アプリケーション用のセキュリティー・キー資格情報の取得方法について詳しくは、[{{site.data.keyword.messagehub}} への接続](/docs/services/EventStreams/eventstreams127.html#connect_messagehub)を参照してください。

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















