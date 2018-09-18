---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Alpha プログラム入門
{: #alpha_program}

Alpha プログラムは、{{site.data.keyword.messagehub}} サービスの次期バージョンへの早期アクセスを可能にします。 

{{site.data.keyword.messagehub}} Alpha を使用してアプリを実行するには、以下のステップを実行します。


## {{site.data.keyword.messagehub}} サービスの作成
{: alpha_create}


  1. **「Message Hub vNext - Production」**タイルをクリックします。これは、
[カタログ ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.stage1.bluemix.net/catalog/labs/?search=vnext) にある試験的サービスです。</li>

  2. {{site.data.keyword.messagehub}} サービスを作成します。このサービスは、<code>プレミアム</code>価格設定プランに従って単一テナント・クラスターを提供し、通常、プロビジョンされるのに 1 時間から 3 時間かかります。
 


## コンソール・オプションを使用した資格情報の取得: テスト・アプリの作成と接続
{: alpha_app}

{{site.data.keyword.messagehub}} を使用して処理を行うには、資格情報が必要です。
使用できるアプリがまだない場合、テスト・アプリを作成し、アプリが使用する資格情報を使用して {{site.data.keyword.messagehub}} に接続してください。例えば、テスト・アプリには **SDK for Node.js** サービスを使用できます。 

  1. [カタログ ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.stage1.bluemix.net/catalog/starters/sdk-for-nodejs) 内の **SDK for Node.js** タイルにナビゲートします。
   
  アプリ名を入力する前に、米国南部地域を選択していることを確認してください。アプリを作成します。

  2. アプリが実行中になったら、左側にある**「接続」**タブをクリックします。

  3. **「接続の作成」**ボタンをクリックします。

  4. 既存の互換性のあるサービスのリストから新しい {{site.data.keyword.messagehub}} サービスを選択し、**「接続」**ボタンをクリックします。

  5. **「IAM 対応サービスの接続」**ウィンドウで、デフォルトを受け入れ、**「接続」**をクリックします。
  {{site.data.keyword.messagehub}} サービスがプロビジョン済みであり、サービスに接続できることを確認します。

  6. 左側の**「ランタイム」**タブをクリックし、中央の**「環境変数」**タブを選択します。**「VCAP_SERVICES」**セクションで、<code>kafka_admin_url</code>、<code>apikey</code>、および <code>kafka_brokers_sasl</code> の情報を見つけます。メッセージを送信するために、これらの情報が必要になります。
  
## コマンド・ライン・オプションを使用した資格情報の取得
代わりの方法として、コマンド・ラインを使用して必要な資格情報を取得できます。以下のステップを実行してください。

  1. [{{site.data.keyword.Bluemix_notm}} CLI ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/cli/index.html#overview) から {{site.data.keyword.Bluemix_notm}} コマンド・ライン・ツールをインストールします。
  
  2. {{site.data.keyword.Bluemix_notm}} CLI にログインします。
  
  3. {{site.data.keyword.Bluemix_notm}} CLI を使用して、管理者の役割を持つサービス・キーを作成します。そのためには、以下のコマンドを使用します。
  ```
  bx resource service-key-create <NAME> Manager --instance-name <MESSAGEHUB_SERVICE_INSTANCE_NAME>
  ```
  4. 出力には、apikey、管理 REST エンドポイント URL、およびブローカー・リストが含まれます。この情報は、以下のコマンドを実行することで再度表示できます。
  ```
  bx resource service-key <NAME>
  ```

## {{site.data.keyword.messagehub}} トピックの作成とメッセージの送信

CURL コマンドを使用してトピックを作成し、その後、kafkacat ツールを使用してメッセージを生成およびコンシュームできます。 

各コマンドで、APIKEY および KAFKA_ADMIN_URL は VCAP_SERVICES 環境変数から取得した値に置換してください。

  1. コマンド・ラインから、以下の CURL コマンドを使用して {{site.data.keyword.messagehub}} トピックを作成します。
  
    ```
    curl -i -X POST -H "Content-Type: application/json" -H "X-Auth-Token: APIKEY" --data '{ "name": "newtop"}' KAFKA_ADMIN_URL/admin/topics
    ```
    {: codeblock}

  2. [kafkacat ツール ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/edenhill/kafkacat#install) をインストールします。このツールは、Kafka の簡易テストに便利です。
  
  3. 次のコマンドを実行するには、以下の情報が必要です。
  
    * 資格情報の `kafka_brokers_sasl` 内で返されたブローカー・リスト。ブローカー・リストは、kafkacat のコンマ区切りリストでなければなりません。
	VCAP_SERVICES には、最初の 5 つのブローカーのみがリストされます。5 個を超えるブローカーがある場合、他のブローカーの詳細を取得するには Kafka クライアントを使用してください。 
  
    * <code>apikey</code>。<code>apikey</code> の先頭 8 文字が sasl.username を形成し、残りの文字が sasl.password を形成します。
    * SSL 証明書の場所。例えば、Ubuntu の場合、SSL_CERTS_DIRECTORY は <code>/etc/ssl/certs/</code> です。
  
  4. 以下のようなコマンドを実行して、いくつかのメッセージを生成します。
  ```
  kafkacat -X "security.protocol=sasl_ssl" -X 'sasl.mechanisms=PLAIN' -X 'sasl.username=<FIRST_8_CHARS_FROM_APIKEY>' -X 'sasl.password=<REMAINING_CHARS_FROM_APIKEY>' -X "ssl.ca.location=<SSL_CERTS_DIRECTORY>" -b <BROKERS_LIST> -P -t <TOPIC_NAME>
    ```
		
	<br/>
  コマンドを実行したら、プロデューサー・ターミナルで <code>HelloWorld</code> などのテキストを入力できます。
  
  5. 以下のようなコマンドを実行して、メッセージをコンシュームします。
  ```
  kafkacat -X "security.protocol=sasl_ssl" -X 'sasl.mechanisms=PLAIN' -X 'sasl.username=<FIRST_8_CHARS_FROM_APIKEY>' -X 'sasl.password=<REMAINING_CHARS_FROM_APIKEY>' -X "ssl.ca.location=<SSL_CERTS_DIRECTORY>" -b <BROKERS_LIST> -C -t <TOPIC_NAME> -f 'Topic %t [%p] at offset %o: key %k: %s\n'
  ```
	
	<br/>
  コンシューマー・ターミナルに、<code>HelloWorld</code> が表示されます。

