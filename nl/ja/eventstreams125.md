---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-04"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}

# {{site.data.keyword.messagehub}} チームへの問題の報告 - 標準プランおよびエンタープライズ・プラン
{: #report_problem_enterprise}


{{site.data.keyword.messagehub}} で問題が起こった場合、最初に [{{site.data.keyword.Bluemix_notm}} 状況ページ ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/status?selected=status){:new_window} を確認してください。
{: shortdesc}

{{site.data.keyword.messagehub}} チームから支援を受けたい場合は、以下の情報をすべて収集してください。 事前に提供できる情報が多ければ多いほど、チームは問題解決をより効率的に支援できます。
{:shortdesc}

1. 使用している {{site.data.keyword.messagehub}} サービスの CRN ID は?  このサービスをクリックした後で {{site.data.keyword.Bluemix_notm}} コンソール URL 全体を貼り付けるか、または、次の CLI コマンドの出力を貼り付けることによって、この ID を提供できます。<br/>
   <pre class="pre">
   ibmcloud resource service-instance NAME
   </pre>
	{: codeblock}
2. 問題が最初に起こったのはいつですか (時刻、日付、タイム・ゾーンを具体的に)?
   それまでにアプリケーションはどのくらい長く実行されていましたか?
3. 問題は引き続き発生していますか? 再生成できますか?
4. アプリケーションは Kafka クライアントのどちらを使用していますか? バージョンの詳細は?
5. クライアント構成の詳細は? プロデューサーとコンシューマーの設定を知る必要があるので、プロデューサーまたはコンシューマーの作成に渡されたデフォルト以外のオプションをリストしてください。
6. 問題が示されているアプリケーション・ログ・スニペットがありますか?
7. 発生している問題は何ですか? 影響を受けているトピック、クライアント ID、グループ ID、およびトランザクション ID は?
8. あなたのサービスへの問題の影響は?

収集した情報は、[サポート要求を送信する ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){:new_window} ことにより、サポート・チケットとして IBM に提出することができます。










