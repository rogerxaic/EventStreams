---

copyright:
  years: 2015, 2018
lastupdated: "2017-03-14"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


#モニタリングとロギング
{: #monitoring}


{{site.data.keyword.messagehub}} ではメトリックおよびイベントが自動的に収集されるため、{{site.data.keyword.messagehub}} の使用をモニターすることができます。
{:shortdesc}

**注:** {{site.data.keyword.Bluemix_short}} Dedicated 環境の {{site.data.keyword.messagehub}} では、メトリックおよびイベントは使用可能ではありません。

以下のログ情報をモニターできます。

<dl>
<dt>トピックのメトリック</dt>
<dd>各トピックの入力バイト数および出力バイト数を送信します (チェックポイントは 15 分ごと)。 これらのメトリックには、{{site.data.keyword.Bluemix_notm}} コンソールで {{site.data.keyword.messagehub}} ダッシュボードの**「Grafana」**ボタンをクリックすることによってアクセスできます。
</dd>
<dt>トピックのイベント</dt>
<dd>トピックの作成または削除ごとに、イベントのプッシュも行います。 これらのイベントには、{{site.data.keyword.Bluemix_notm}} コンソールで {{site.data.keyword.messagehub}} ダッシュボードの**「Kibana」**ボタンをクリックすることによってアクセスできます。</dd>
</dl>


ユーザーの変更を上書きするような更新を {{site.data.keyword.messagehub}} が行う可能性があるため、{{site.data.keyword.messagehub}} ダッシュボードを編集しないことをお勧めします。 それでも、これらのメトリックおよびイベントをダッシュボードに組み込むことは可能です。
