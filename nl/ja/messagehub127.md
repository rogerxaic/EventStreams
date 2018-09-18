---

copyright:
  years: 2015, 2018
lastupdated: "2018-07-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Message Hub への接続
{: #connect_messagehub}

{{site.data.keyword.messagehub}} に接続する方法は、標準プランとエンタープライズ・プランのどちらを使用するのかと、Cloud Foundry アプリケーションと他の外部クライアントのどちらから接続するのかによって異なります。{{site.data.keyword.messagehub}} のどの API に接続する場合でも、次の 2 つの情報を収集する必要があります。

* API のエンドポイント URL 
* 認証のための資格情報

これらの詳細を取得する方法については、以下の説明をお読みください。ここで示す手順は微妙に変わる可能性があるため、ご使用のインスタンスに合った適切な手順を実行するように注意してください。

## {{site.data.keyword.messagehub}} インスタンスのプロビジョン

前提条件として、まず最初に、標準プランまたはエンタープライズ・プランのいずれかで {{site.data.keyword.messagehub}} サービス・インスタンスをプロビジョンする必要があります。{{site.data.keyword.messagehub}} インスタンスのプロビジョンには料金がかかる可能性があります。次に、以下のタスクを実行することによって、{{site.data.keyword.messagehub}} API 接続詳細を取得します。

## 標準プランの概要
{: #connect_standard}

標準プランを使用してプロビジョンされるサービスは、Cloud Foundry サービスです。これは、それらのサービスが 1 つの Cloud Foundry 組織およびスペースにデプロイされ、ダッシュボードの**「Cloud Foundry サービス」**というヘッダーの下にグループ化されることを意味します。アプリケーションを接続するために使用する方法は、アプリケーションがどこにデプロイされているのか、つまり、Cloud Foundry の内側か外側かによって異なります。


## 標準プランの Cloud Foundry アプリケーション
{: #connect_standard_cf}

Cloud Foundry の内側で実行されているアプリの場合、アプリを {{site.data.keyword.messagehub}} サービス・インスタンスにバインドします。バインドされると、JSON 形式の接続詳細が VCAP_SERVICES 環境変数でアプリに使用可能になります。IBM Cloud コンソールまたは IBM Cloud CLI のいずれかを使用して、アプリとサービスをバインドできます。

以下に VCAP_SERVICES の例を示します。

```
{
  "credentials": {
    "mqlight_lookup_url": "https://mqlight-lookup.messagehub.services.us-south.bluemix.net/Lookup?serviceId=584e8436-e7f5-43db-96ac-2864fccae5ae",
    "api_key": "d9JSx1SYsmLzNRbbgUFneDm2DtkedlVeViObYJIvrPAf2kJA",
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
{: #connect_standard_cf_console }

1. 対象の Cloud Foundry 組織およびスペースにいることを確認します。
2. ダッシュボードで Cloud Foundry アプリケーションを見つけます。まだ Cloud Foundry アプリケーションがない場合、**「リソースの作成」**ボタンをクリックして作成できます。
3. アプリケーション・タイルをクリックします。
4. **「接続」**をクリックします。
5. **「接続の作成」**をクリックします。
6. バインドする先の {{site.data.keyword.messagehub}} サービスのタイルを選択し、**「接続」**をクリックします。 変更を有効にするために、アプリケーションの再ステージが必要な場合があります。
7. 左側の**「ランタイム」**タブをクリックし、中央の**「環境変数」**タブを選択します。 これで、VCAP_SERVICES 情報を検証できるようになり、アプリケーションは環境変数としてこれにアクセスできます。 


### IBM Cloud CLI を使用した資格情報の取得 
{: #connect_standard_cf_cli }

<ol>
<li>対象の Cloud Foundry 組織およびスペースにいることを確認します。次のコマンドを実行することによって、対話式にナビゲートすることができます。<br/>
<code>ibmcloud target -cf</code>
</li>
<li>アプリを検出します。<br/> <code>ibmcloud app list</code> <br/>
</br>
マニフェスト・ファイルがある場合、以下を実行して新しいアプリを作成できます。</br>
<code>ibmcloud app push</code>
</li>
<li>サービスを検出します。</br> 
<code>ibmcloud service list</code>
</li>
<li>アプリをサービスにバインドします。</br>
<code>ibmcloud service bind <var class="keyword varname">your_app_name</var> <var class="keyword varname">your_service_name</var></code>
</li>
<li>以下を実行して、アプリケーション・ランタイムで VCAP_SERVICES 環境変数が使用可能であることを検証します。</br> 
 <code>ibmcloud app env <var class="keyword varname">your_app_name</var></code> 
</li>
<li>これらの資格情報をアプリケーションに渡します。変更を有効にするために、アプリケーションの再ステージが必要な場合があります。</li>
</ol>

## 標準プランの外部アプリケーション
{: #connect_standard_external}

Cloud Foundry の外側で実行されているアプリケーションの場合、資格情報はサービス・キーを作成することによって生成されます。サービス・キーを取得したら、選択した方法でキーの詳細を手動でアプリケーションに渡します。

### IBM Cloud コンソールを使用した資格情報の取得
{: #connect_standard_external_console}

1. 対象の Cloud Foundry 組織およびスペースにいることを確認します。
2. ダッシュボードで Cloud Foundry {{site.data.keyword.messagehub}} サービスを見つけます。
3. サービス・タイルをクリックします。
4. **「サービス資格情報」**をクリックします。
5. **「新規資格情報」**をクリックします。
6. 名前など、新規資格情報の詳細を入力し、**「追加」**をクリックします。 新規資格情報が資格情報リストに表示されます。
7. **「資格情報の表示」**を使用してこの資格情報をクリックして、JSON 形式の詳細を表示します。
8. これらの資格情報をアプリケーションに渡します。

### IBM Cloud CLI を使用した資格情報の取得 
{: #connect_standard_external_cli }

<ol>
<li>対象の Cloud Foundry 組織およびスペースにいることを確認します。次のコマンドを実行することによって、対話式にナビゲートすることができます。<br>
<code>ibmcloud target -cf</code>
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
JSON 形式のサービス・キー詳細が返されます。</li>
<li>これらの資格情報をアプリケーションに渡します。</li>
</ol>

 
## エンタープライズ・プランの概要
{: #connect_enterprise}

エンタープライズ・プランを使用してプロビジョンされたサービスは、ダッシュボードの**「サービス」**というヘッダーの下にグループ化されます。エンタープライズ・プランでは [IAM が有効 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/docs/iam/quickstart.html#getstarted){:new_window} です。開始するにあたって IAM を理解する必要はありませんが、{{site.data.keyword.messagehub}} サービスを保護したい場合は、いくらかの知識を持つことが推奨されます。詳しくは、[{{site.data.keyword.messagehub}} リソースの保護](/docs/services/MessageHub/messagehub124.html)を参照してください。

以下の手順を実行して、アプリケーションをバインドし、サービスのサービス・キーを取得します。トピックを作成する許可を得るために、アプリケーションまたはサービス・キーには管理者アクセス役割がなければなりません。

アプリケーションを接続するために使用される方法は、アプリケーションがどこにデプロイされているのか、つまり、Cloud Foundry の内側か外側かによって異なります。

## エンタープライズ・プランの Cloud Foundry アプリケーション
{: #connect_enterprise_cf}

アプリケーションは {{site.data.keyword.messagehub}} サービス・インスタンスにバインドされる必要があります。Cloud Foundry アプリケーションを One Cloud の非 Cloud Foundry サービスにバインドするには、最初に Cloud Foundry サービス別名を作成し、次に、バインド時に Cloud Foundry アプリケーションからこの別名を参照します。

バインドされると、JSON 形式の接続詳細が、VCAP_SERVICES 環境変数を使用してアプリケーションで使用可能になります。IBM Cloud コンソールまたは IBM Cloud CLI のいずれかを使用して、アプリケーションとサービスをバインドできます。

### IBM Cloud コンソールを使用したアプリケーションのバインド
{: #connect_enterprise_cf_console}

1. 対象の Cloud Foundry 組織およびスペースにいることを確認します。
2. ダッシュボードで Cloud Foundry アプリケーションを見つけるか、**「リソースの作成」**ボタンをクリックしてアプリケーションを作成します。
3. アプリケーション・タイルをクリックします。
4. **「接続」**をクリックします。
5. **「接続の作成」**をクリックします。
6. バインドする先の {{site.data.keyword.messagehub}} サービスのタイルを選択し、**「接続」**をクリックします。 これによって、{{site.data.keyword.messagehub}} サービス用の Cloud Foundry サービス別名が自動的に作成され、アプリケーションがこの別名にバインドされます。変更を有効にするために、アプリケーションの再ステージが必要な場合があります。
8. 左側の**「ランタイム」**タブをクリックし、中央の**「環境変数」**タブを選択します。 これで、VCAP_SERVICES 情報を検証できるようになります。アプリケーションはこれらに環境変数としてアクセスできます。 
 

### IBM Cloud CLI を使用したアプリケーションのバインド
{: #connect_enterprise_cf_cli}

<ol>
<li>対象の Cloud Foundry 組織およびスペースにいることを確認します。次のコマンドを実行することによって、対話式にナビゲートすることができます。<br/>
 <code>ibmcloud target -cf</code></li>
<li>アプリを見つけます。</br>
<code>ibmcloud app list</code><br/>
<br/>
マニフェスト・ファイルがある場合、以下を実行して新しいアプリを作成できます。<br/>
<code>ibmcloud app push</code><br/>
<br/>
アプリは、まだ {{site.data.keyword.messagehub}} にバインドされていないため、接続を確立できません。したがって、不必要な接続障害を回避するために <code>--no-start</code> パラメーターを指定してアプリケーションをプッシュすることをお勧めします。</li>
<li>サービスを見つけます。</br>
<code>ibmcloud resource service-instances</code></li>
<li>Cloud Foundry サービス別名を作成します。<br/>
<code>ibmcloud resource service-alias-create <var class="keyword varname">alias_name</var> --instance-name <var class="keyword varname">your_service_name</var></code></li>
<li>前に作成したサービス別名にアプリをバインドします。<br/>
<code>ibmcloud service bind <var class="keyword varname">your_ app_name</var> <var class="keyword varname">alias_name</var></code>.<br/>
<br/>
あるいは、マニフェスト・ファイルを更新し、アプリケーションをもう一度プッシュすることもできます。</li>
<li>アプリケーション・ランタイムで VCAP_SERVICES 環境変数が使用可能であることを検証します。<br/>
<code>ibmcloud app env <var class="keyword varname">your_app_name</var></code></li>
<li>これらの資格情報をアプリケーションに渡します。<br/> 変更を有効にするために、アプリケーションの再ステージが必要な場合があります。</li>
</ol>


## エンタープライズ・プランの外部アプリケーション
{: #connect_enterprise_external}

Cloud Foundry の外側で実行されているアプリケーションの場合、資格情報はサービス・キーを作成することによって生成されます。サービス・キーを取得したら、選択した方法でキーの詳細を手動でアプリケーションに渡します。

### IBM Cloud コンソールを使用した資格情報の取得
{: #connect_enterprise_external_console}

1. ダッシュボードで {{site.data.keyword.messagehub}} サービスを見つけます。
2. サービス・タイルをクリックします。
3. **「サービス資格情報」**をクリックします。
4. **「新規資格情報」**をクリックします。 
5. 名前や役割など、新規資格情報の詳細を入力し、**「追加」**をクリックします。 新規資格情報が資格情報リストに表示されます。
6. **「資格情報の表示」**を使用してこの資格情報をクリックして、JSON 形式の詳細を表示します。
7. これらの資格情報をアプリケーションに渡します。アプリケーションが詳細を構文解析することを確認してください。

### IBM Cloud CLI を使用した資格情報の取得
{: #connect_enterprise_external_cli}

<ol>
<li>サービスを見つけます。<br/>
<code>ibmcloud resource service-instances</code></li>
<li>サービス・キーを作成します。<br/>
<code>ibmcloud resource service-key-create <var class="keyword varname">key_name</var> <var class="keyword varname">key_role</var> --instance-name <var class="keyword varname">your_service_name</var></code></li>
<li>サービス・キーを表示します。<br/>
<code>ibmcloud resource service-key <var class="keyword varname">key_name</var></code></li>
<li>これらの資格情報をアプリケーションに渡します。アプリケーションが詳細を構文解析することを確認してください。</li>
</ol>

## 次に行うこと
{: #after_connecting}

これで、接続情報および資格情報を取得したので、{{site.data.keyword.messagehub}} クライアントを選択できます。選択はプランによって異なります。

* 標準プランを使用している場合、選択するクライアントおよび接続方法について、[3 つの API からの選択](/docs/services/MessageHub/messagehub087.html)を参照してください。
* エンタープライズ・プランを使用している場合、[Kafka API の使用](/docs/services/MessageHub/messagehub050.html)を参照してください。

	エンタープライズ・プランを使用している場合、内部 Kafka <code>__consumer_offsets</code> トピックが読み取り専用として可視になります。このトピックは、いかなる場合も絶対に管理しないでください。 

<!--
Charlie said:

"Add some info describing how to take the information made available from above e.g. like the info in the Connecting a client to the Kafka API section of the alpha docs on stage 1? https://console.stage1.bluemix.net/docs/services/MessageHub/messagehub122.html#alpha_about "
-->







 















