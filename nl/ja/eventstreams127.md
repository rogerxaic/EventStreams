---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-15"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}


# {{site.data.keyword.messagehub}} への接続
{: #connecting}

{{site.data.keyword.messagehub}} に接続する方法は、アプリケーションがネイティブに実行されているか、または Cloud Foundry アプリケーションとして実行されているかによって異なります。ただし、どちらの場合も 2 つの情報が必要です。
{: shortdesc}

* API のエンドポイント URL
* 認証のための資格情報

これらの詳細を取得する方法については、以下の説明をお読みください。 ここで示す手順は微妙に変わる可能性があるため、ご使用のインスタンスに合った適切な手順を実行するように注意してください。

クラシック・プランを使用しているときに {{site.data.keyword.messagehub}} に接続する方法は、[クラシック・プランを使用した接続](/docs/services/EventStreams?topic=eventstreams-connecting_classic)を参照してください。


## 概説
{: #connect_enterprise}

標準プランまたはエンタープライズ・プランを使用してプロビジョンされたサービスは、ダッシュボードの**「サービス」**というヘッダーの下にグループ化されます。 プランでは [IAM が有効 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/iam?topic=iam-getstarted#getstarted){:new_window} です。 開始するにあたって IAM を理解する必要はありませんが、{{site.data.keyword.messagehub}} サービスを保護したい場合は、いくらかの知識を持つことが推奨されます。 詳しくは、[{{site.data.keyword.messagehub}} リソースへのアクセスの管理](/docs/services/EventStreams?topic=eventstreams-security)を参照してください。

以下の手順を実行して、アプリケーションをバインドし、サービスのサービス・キーを取得します。 トピックを作成する許可を得るために、アプリケーションまたはサービス・キーには管理者アクセス役割がなければなりません。

アプリケーションを接続するために使用する方法は、アプリケーションがどこにデプロイされているのか、つまり、Cloud Foundry の内側か外側か (例えば、Kubernetes サービスの中か) によって異なります。

## {{site.data.keyword.messagehub}} インスタンスのプロビジョン

前提条件として、最初に、標準プランまたはエンタープライズ・プランのいずれかで {{site.data.keyword.messagehub}} サービス・インスタンスをプロビジョンする必要があります。 次に、以下のタスクを実行することによって、{{site.data.keyword.messagehub}} API 接続詳細を取得します。

## アプリケーションの接続 
{: #connect_enterprise_external}

Cloud Foundry の外側で実行されているアプリケーションの場合、資格情報はサービス・キーを作成することによって生成されます。 サービス・キーを取得したら、選択した方法でキーの詳細を手動でアプリケーションに渡します。

### IBM Cloud コンソールを使用した資格情報の取得
{: #connect_enterprise_external_console}

1. ダッシュボードで {{site.data.keyword.messagehub}} サービスを見つけます。
2. サービス・タイルをクリックします。
3. **「サービス資格情報」**をクリックします。
4. **「新規資格情報」**をクリックします。 
5. 名前や役割など、新規資格情報の詳細を入力し、**「追加」**をクリックします。 新規資格情報が資格情報リストに表示されます。
6. **「資格情報の表示」**を使用してこの資格情報をクリックして、JSON 形式の詳細を表示します。
7. これらの資格情報をアプリケーションに渡します。 ユーザー名として <code>token</code> を、パスワードとして <var class="keyword varname">api_key</var> を指定します。 <code>token</code> と <var class="keyword varname">api_key</var> はコロンで区切ってください。 詳しくは、[Kafka API クライアントの構成](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_api_client)を参照してください。
   <br/><br/>アプリケーションが詳細を構文解析することを確認してください。

### IBM Cloud CLI を使用した資格情報の取得
{: #connect_enterprise_external_cli}

<ol>
<li>サービスを見つけます。<br/>
<code>ibmcloud resource service-instances</code></li>
<li>サービス・キーを作成します。<br/>
<code>ibmcloud resource service-key-create <var class="keyword varname">key_name</var> <var class="keyword varname">key_role</var> --instance-name <var class="keyword varname">your_service_name</var></code></li>
<li>サービス・キーを表示します。<br/>
<code>ibmcloud resource service-key <var class="keyword varname">key_name</var></code></li>
<li>これらの資格情報をアプリケーションに渡します。 ユーザー名として <code>token</code> を、パスワードとして <var class="keyword varname">api_key</var> を指定します。 <code>token</code> と <var class="keyword varname">api_key</var> はコロンで区切ってください。 詳しくは、[クライアントの構成](/docs/services/EventStreams?topic=eventstreams-kafka_connect)を参照してください。
<p>アプリケーションが詳細を構文解析することを確認してください。</p></li>
</ol>

## Cloud Foundry アプリケーションの接続
{: #connect_enterprise_cf}

アプリケーションは {{site.data.keyword.messagehub}} サービス・インスタンスにバインドされる必要があります。 Cloud Foundry アプリケーションを非 Cloud Foundry サービスにバインドするには、最初に Cloud Foundry サービス別名を作成し、次に、バインド時に Cloud Foundry アプリケーションからこの別名を参照します。 

バインドされると、JSON 形式の接続詳細が、VCAP_SERVICES 環境変数を使用してアプリケーションで使用可能になります。 [IBM Cloud コンソール](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_cf_console) または [IBM Cloud CLI](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_cf_cli) のいずれかを使用して、アプリケーションとサービスをバインドできます。

### IBM Cloud コンソールを使用したアプリケーションのバインド
{: #connect_enterprise_cf_console}

1. 対象の Cloud Foundry 組織およびスペースにいることを確認します。
2. ダッシュボードで Cloud Foundry アプリケーションを見つけるか、**「リソースの作成」**ボタンをクリックしてアプリケーションを作成します。
3. アプリケーション・タイルをクリックします。
4. **「接続」**をクリックします。
5. **「接続の作成」**をクリックします。
6. バインドする先の {{site.data.keyword.messagehub}} サービスのタイルを選択し、**「接続」**をクリックします。 
7. 表示された**「IAM 対応サービスの接続」**ウィンドウで、**「接続のためのアクセス役割」**からアクセス役割を選択し、**「接続用のサービス ID」**リストからサービス ID を選択します (自動生成された ID を受け入れることができます)。 **「接続」**をクリックします。 

  これによって、{{site.data.keyword.messagehub}} サービス用の Cloud Foundry サービス別名が作成され、アプリケーションがこの別名にバインドされます。 

  アプリケーションを再ステージして、変更を有効にします。<br/>
8. 左側の**「ランタイム」**タブをクリックし、中央の**「環境変数」**タブを選択します。 これで、VCAP_SERVICES 情報を検証できるようになります。 アプリケーションはこれらに環境変数としてアクセスできます。 
 

### IBM Cloud CLI を使用したアプリケーションのバインド
{: #connect_enterprise_cf_cli}

<ol>
<li>対象の Cloud Foundry 組織およびスペースにいることを確認します。 次のコマンドを実行することによって、対話式にナビゲートすることができます。<br/>
 <code>ibmcloud target --cf</code></li>
<li>アプリを見つけます。</br>
<code>ibmcloud app list</code><br/>
<br/>
マニフェスト・ファイルがある場合、以下を実行して新しいアプリを作成できます。<br/>
<code>ibmcloud app push</code><br/>
<br/>
アプリはまだ {{site.data.keyword.messagehub}} にバインドされていないため、接続を確立できません。したがって、不必要な接続障害を回避するために <code>--no-start</code> パラメーターを指定してアプリケーションをプッシュすることをお勧めします。</li>
<li>サービスを見つけます。</br>
<code>ibmcloud resource service-instances</code></li>
<li>Cloud Foundry サービス別名を作成します。<br/>
<code>ibmcloud resource service-alias-create <var class="keyword varname">alias_name</var> --instance-name <var class="keyword varname">your_service_name</var></code></li>
<li>前に作成したサービス別名にアプリをバインドします。<br/>
<code>ibmcloud service bind <var class="keyword varname">your_ app_name</var> <var class="keyword varname">alias_name</var></code>。<br/>
<br/>
あるいは、マニフェスト・ファイルを更新し、アプリケーションをもう一度プッシュすることもできます。</li>
<li>アプリケーション・ランタイムで VCAP_SERVICES 環境変数が使用可能であることを検証します。<br/>
<code>ibmcloud app env <var class="keyword varname">your_app_name</var></code></li>
<li>これらの資格情報をアプリケーションに渡します。 ユーザー名として <code>token</code> を、パスワードとして <var class="keyword varname">api_key</var> を指定します。 <code>token</code> と <var class="keyword varname">api_key</var> はコロンで区切ってください。 詳しくは、[クライアントの構成](/docs/services/EventStreams?topic=eventstreams-kafka_connect)を参照してください。 
<p>変更を有効にするために、アプリケーションの再ステージが必要な場合があります。</p></li>
</ol>


## 次に行うこと
{: #after_connecting}

これで、接続情報および資格情報を取得したので、{{site.data.keyword.messagehub}} クライアントを選択できます。 詳しくは、[ Kafka API の使用](/docs/services/EventStreams?topic=eventstreams-kafka_using)を参照してください。

<!--
Charlie said:

"Add some info describing how to take the information made available from above e.g. like the info in the Connecting a client to the Kafka API section of the alpha docs on stage 1? https://console.stage1.bluemix.net/docs/services/EventStreams/eventstreams122.html#alpha_about "
-->







 















