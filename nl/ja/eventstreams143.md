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

# 新規 {{site.data.keyword.messagehub}} 標準プランへのアップグレード 
{: #migrate_classic_plan}

この新規リリースであるマルチテナントの標準プランは、回復力、機能性、使いやすさが大幅に改善しています。詳しくは、[新しい標準プランのブログでの発表![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud/blog/announcements/ibm-event-streams-releases-a-new-and-enhanced-standard-plan)を参照してください。
{: shortdesc}

以前の標準プラン (今後の名称はクラシック・プラン) からこの新規プランにアプリケーションをマイグレーションするには、以下の情報について考慮してください。

サービス・インスタンスが Cloud Foundry サービスではなく {{site.data.keyword.cloud_notm}} サービスとしてプロビジョンされるようになります。これにより、このサービスでは最新の {{site.data.keyword.cloud_notm}} 標準と機能 (マルチゾーン・デプロイメントときめ細かなアクセス制御など) をサポートすることができますが、サービスの使用方法に影響があります。特に以下の特徴について考慮してください。

* **サービスの作成、削除、およびリスト表示**
<br/>
    以前と同じように、{{site.data.keyword.cloud_notm}} コンソールまたは {{site.data.keyword.cloud_notm}} CLI コマンド・ライン・ツールのどちらかを使用してサービスのライフサイクルを管理できます。コンソールを使用する場合、サービスは**「Cloud Foundry サービス」**ではなく**「サービス」**の下にリスト表示されるようになりました。 
    
    CLI を使用する場合、インスタンスの管理にリソース・コマンドが使用されます。例えば、<code>ibmcloud resource service-instance-create</code> のようなコマンドです。これは、**cf** コマンド (<code>ibmcloud cf create-service</code> など) の代わりに使用されます。

* **アクセスの制御**
<br/>
    認証と許可は、Cloud ID とアクセス管理 (IAM) サービスを使用して管理されるようになりました。IAM は、あるユーザーが接続できるかどうかを制御するだけではく、トピックのような基礎となるリソースへのきめ細かなアクセスを構成することもできます。ポリシーをユーザーに割り当てることでアクセスが制御されます。詳しくは、[{{site.data.keyword.messagehub}} リソースへのアクセスの管理](/docs/services/EventStreams?topic=eventstreams-security)を参照してください。

<ul>
<li><strong>アプリケーションの接続</strong>
<br/>
    アプリケーションの接続に必要な情報は変更されていません。つまり、<code>bootstrap.servers</code>、<code>username</code>、および <code>password</code> のリストが必要です。ただし、これらのプロパティーの取得方法は変更されました。

<ul>
<li>
      <strong>ネイティブ・アプリケーションの場合</strong>
        <br/>
        IBM Cloud コンソールまたは CLI のどちらかを使用して、資格情報またはサービス・キー・オブジェクトを作成する必要があります。その上で、必要なプロパティーを取得することができます。詳しくは、[アプリケーションの接続](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_external)を参照してください。
</li>
<br/>
<li><strong>Cloud Foundry アプリケーションの場合</strong>
        <br/>
        まず、サービス別名を作成してアプリケーションの組織とスペースにサービスをバインドする必要があります。その上で、VCAP_SERVICES 環境変数から正常に必要なプロパティーを取得することができます。詳しくは、[{{site.data.keyword.messagehub}} への接続](/docs/services/EventStreams?topic=eventstreams-connecting)を参照してください。
</li>
</ul>
<br/>
サーバーのホスト名が TLS ハンドシェークに組み込まれる場合には、TLS に対する SNI 拡張機能をクライアントがサポートする必要があることにご注意ください。このフィーチャーは、[{{site.data.keyword.messagehub}} で使用する Kafka クライアントの選択](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_clients)で推奨されているすべてのクライアント・バージョンに共通で使用可能であり、サポートされています。
</li>
</ul>

<br/>
以下のその他の変更点にもご注意ください。

* **Kafka バージョン**
<br/>
    このプランは、最新の安定した Kafka リリース 2.2 へのアクセスを提供します。Kafka 1.1 に対して開発されたアプリケーションは変更なく実行できますが、推奨されるクライアント・バージョンとテストされた組み合わせについては、[{{site.data.keyword.messagehub}}で使用する Kafka クライアントの選択
](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_clients)の情報を参照してください。 

* **サポートされる地域**
<br/>
    このプランは最初は米国南部でのみご利用可能です。他の地域は今後数週間にわたって導入される予定です。

* **統合**
<br/>
    Cloud Foundry の組織とスペースを使用してサービスにバインドする他のサービス ({{site.data.keyword.iot_short_notm}} または {{site.data.keyword.openwhisk_short}} など) からの接続はサービス別名を作成する必要があります。 詳しくは、[Cloud Foundry アプリケーションの {{site.data.keyword.messagehub}} への接続](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_cf)を参照してください。

    

* **サポートされる機能**
<br/>
    クラシック・プランと新しい標準プランの機能には違いがあります。製品オファリング間の調整、選択した新規テクノロジーの適用、使用頻度が低いフィーチャーの削除を行うために、すべての機能が持ち越されるわけではありません。フィーチャーの比較は、[プランの選択](/docs/services/EventStreams?topic=eventstreams-plan_choose)を参照してください。これらの機能を使用している場合、マイグレーションに役立つ詳しい情報がまもなく提供される予定です。
   
<br/>
小さなコード・デルタが毎日実動環境に対してシップされています。そのため、2019 年の残りの期間およびそれ以降を通して、ユーザー・エクスペリエンス (そして他の分野) に対するさらに多くの機能改善を期待できます。近日提供予定の機能は以下のとおりです。

* **カスタマー・メトリック**
<br/>
    サービス・インスタンスのアクティビティーをモニターする機能。

<br/>
新しい標準プランを稼働するためのステップを紹介するクイック・ウォークスルーについては、[入門チュートリアル](/docs/services/EventStreams?topic=eventstreams-getting_started)を参照してください。


