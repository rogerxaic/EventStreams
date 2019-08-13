---

copyright:
  years: 2015, 2019
lastupdated: "2018-11-07"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# クラシック・プランを使用した {{site.data.keyword.messagehub}} への接続 
{: #connecting_classic}

{{site.data.keyword.messagehub}} に接続する方法は、Cloud Foundry アプリケーションと他の外部クライアントのどちらから接続するのかによって異なります。 {{site.data.keyword.messagehub}} のどの API に接続する場合でも、次の 2 つの情報を収集する必要があります。
{: shortdesc}

* API のエンドポイント URL
* 認証のための資格情報

これらの詳細を取得する方法については、以下の説明をお読みください。 ここで示す手順は微妙に変わる可能性があるため、ご使用のインスタンスに合った適切な手順を実行するように注意してください。

## {{site.data.keyword.messagehub}} インスタンスのプロビジョン
{: #provision_classic}

前提条件として、最初に、クラシック・プランで {{site.data.keyword.messagehub}} サービス・インスタンスをプロビジョンする必要があります。 次に、以下のタスクを実行することによって、{{site.data.keyword.messagehub}} API 接続詳細を取得します。

## クラシック・プランの概要
{: #connect_classic_plan}

クラシック・プランを使用してプロビジョンされるサービスは、Cloud Foundry サービスです。 これは、それらのサービスが 1 つの Cloud Foundry 組織およびスペースにデプロイされ、ダッシュボードの**「Cloud Foundry サービス」**というヘッダーの下にグループ化されることを意味します。 アプリケーションを接続するために使用する方法は、アプリケーションがどこにデプロイされているのか、つまり、Cloud Foundry の内側か外側か (例えば、Kubernetes サービスの中か) によって異なります。


## クラシック・プランの Cloud Foundry アプリケーション
{: #connect_classic_cf_plan}

Cloud Foundry の内側で実行されているアプリの場合、アプリを {{site.data.keyword.messagehub}} サービス・インスタンスにバインドします。 バインドされると、JSON 形式の接続詳細が VCAP_SERVICES 環境変数でアプリに使用可能になります。 IBM Cloud コンソールまたは IBM Cloud CLI のいずれかを使用して、アプリとサービスをバインドできます。

以下に VCAP_SERVICES の例を示します。

```
{
  "credentials": {
    "mqlight_lookup_url": "https://mqlight-lookup.messagehub.services.us-south.bluemix.net/Lookup?serviceId=584e8436-e7f5-43db-96ac-2864fccae5ae",
    "api_key": <YOUR_API_KEY>,
    "kafka_admin_url": "https://kafka-admin.messagehub.services.us-south.bluemix.net:443",
    "kafka_rest_url": "https://kafka-rest.messagehub.services.us-south.bluemix.net:443",
    "kafka_brokers_sasl": [
      "kafka01.messagehub.services.us-south.bluemix.net:9093",
      "kafka02.messagehub.services.us-south.bluemix.net:9093",
      "kafka03.messagehub.services.us-south.bluemix.net:9093",
      "kafka04.messagehub.services.us-south.bluemix.net:9093",
      "kafka05.messagehub.services.us-south.bluemix.net:9093"
    ],
    "user": "d9JSx1SYsmLzNRbb",
    "password": "gUFneDm2DtkedlVeViObYJIvrPAf2kJA"
  }
}
```

{: codeblock}

{{site.data.keyword.messagehub}} への接続に使用する API に関係なく、環境変数の内容は同じです。 {{site.data.keyword.Bluemix_notm}} アプリケーションは、使用しているインターフェースに応じて、VCAP_SERVICES 環境変数から適切な資格情報を選択します。
 
VCAP_SERVICES には、最初の 5 つのブローカーのみがリストされます。 5 個を超えるブローカーがある場合、Kafka クライアントを使用して、その他のブローカーの詳細を取得してください。 


### IBM Cloud コンソールを使用した資格情報の取得および接続
{: #connect_classic_plan_cf_console }

1. 対象の Cloud Foundry 組織およびスペースにいることを確認します。
2. ダッシュボードで Cloud Foundry アプリケーションを見つけます。 まだ Cloud Foundry アプリケーションがない場合、**「リソースの作成」**ボタンをクリックして作成できます。
3. アプリケーション・タイルをクリックします。
4. **「接続」**をクリックします。
5. **「接続の作成」**をクリックします。
6. バインドする先の {{site.data.keyword.messagehub}} サービスのタイルを選択し、**「接続」**をクリックします。 変更を有効にするために、アプリケーションの再ステージが必要な場合があります。
7. 左側の**「ランタイム」**タブをクリックし、中央の**「環境変数」**タブを選択します。 これで、VCAP_SERVICES 情報を検証できるようになり、アプリケーションは環境変数としてこれにアクセスできます。 


### IBM Cloud CLI を使用した資格情報の取得 
{: #connect_classic_plan_cf_cli }

<ol>
<li>対象の Cloud Foundry 組織およびスペースにいることを確認します。 次のコマンドを実行することによって、対話式にナビゲートすることができます。<br/>
<code>ibmcloud target --cf</code>
</li>
<li>アプリを検出します。<br/> <code>ibmcloud app list</code> <br/>
</br>
マニフェスト・ファイルがある場合、以下を実行して新しいアプリを作成できます。</br>
<code>ibmcloud app push</code>
</li>
<li>サービスを見つけます。</br>
<code>ibmcloud service list</code>
</li>
<li>アプリをサービスにバインドします。</br>
<code>ibmcloud service bind <var class="keyword varname">your_app_name</var> <var class="keyword varname">your_service_name</var></code>
</li>
<li>以下を実行して、アプリケーション・ランタイムで VCAP_SERVICES 環境変数が使用可能であることを検証します。</br>
 <code>ibmcloud app env <var class="keyword varname">your_app_name</var></code>。 
</li>
<li>これらの資格情報をアプリケーションに渡します。 ユーザー名として <code>token</code> を、パスワードとして <var class="keyword varname">api_key</var> を指定します。 <code>token</code> と <var class="keyword varname">api_key</var> はコロンで区切ってください。 詳しくは、[クライアントの構成](/docs/services/EventStreams?topic=eventstreams-kafka_connect)を参照してください。
<p>変更を有効にするために、アプリケーションの再ステージが必要な場合があります。</p></li>
</ol>

## クラシック・プランの外部アプリケーション
{: #connect_classic_plan_external}

Cloud Foundry の外側で実行されているアプリケーションの場合、資格情報はサービス・キーを作成することによって生成されます。 サービス・キーを取得したら、選択した方法でキーの詳細を手動でアプリケーションに渡します。

### IBM Cloud コンソールを使用した資格情報の取得
{: #connect_classic_plan_external_console}

1. 対象の Cloud Foundry 組織およびスペースにいることを確認します。
2. ダッシュボードで Cloud Foundry {{site.data.keyword.messagehub}} サービスを見つけます。
3. サービス・タイルをクリックします。
4. **「サービス資格情報」**をクリックします。
5. **「新規資格情報」**をクリックします。
6. 名前など、新規資格情報の詳細を入力し、**「追加」**をクリックします。 新規資格情報が資格情報リストに表示されます。
7. **「資格情報の表示」**を使用してこの資格情報をクリックして、JSON 形式の詳細を表示します。
8. これらの資格情報をアプリケーションに渡します。 ユーザー名として <code>token</code> を、パスワードとして <var class="keyword varname">api_key</var> を指定します。 <code>token</code> と <var class="keyword varname">api_key</var> はコロンで区切ってください。 詳しくは、[クライアントの構成](/docs/services/EventStreams?topic=eventstreams-kafka_connect)を参照してください。

### IBM Cloud CLI を使用した資格情報の取得 
{: #connect_classic_plan_external_cli }

<ol>
<li>対象の Cloud Foundry 組織およびスペースにいることを確認します。 次のコマンドを実行することによって、対話式にナビゲートすることができます。<br>
<code>ibmcloud target --cf</code>
</li>
<li>サービスを検出します。<br>
<code>ibmcloud service list</code>
</li>
<li>サービス・キーを作成します。<br>
<code>ibmcloud service key-create <var class="keyword varname">your_service_name</var> <var class="keyword varname">new_service_key_name</var></code><br>
<br/>
または、既存のサービス・キーを使用します。<br/>
<code>ibmcloud service keys <var class="keyword varname">your_service_name</var></code> 
</li>
<li>キーの詳細を取得します。</br>
<code>ibmcloud service key-show <var class="keyword varname">your_service_name</var> <var class="keyword varname">service _key_name</var></code></br>
これはサービス・キーの詳細を JSON フォーマットで返します。</li>
<li>これらの資格情報をアプリケーションに渡します。 ユーザー名として <code>token</code> を、パスワードとして <var class="keyword varname">api_key</var> を指定します。 <code>token</code> と <var class="keyword varname">api_key</var> はコロンで区切ってください。 詳しくは、[クライアントの構成](/docs/services/EventStreams?topic=eventstreams-kafka_connect)を参照してください。</li>
</ol>
 
## 次に行うこと
{: #after_connecting_next}

これで、接続情報および資格情報を取得したので、{{site.data.keyword.messagehub}} クライアントを選択できます。 選択するクライアントおよび接続方法について、
[3 つの API からの選択](/docs/services/EventStreams?topic=eventstreams-choose_api_classic)を参照してください。










 















