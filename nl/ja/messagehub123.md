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

# Alpha プログラムでの {{site.data.keyword.iamshort}} (IAM) の使用
{: #alpha_iam }

{{site.data.keyword.messagehub}} の許可は、IAM ポリシーを使用して構成されます。IAM ポリシーは、以下の情報から構成されます。

* サービス ID
* サービス名、サービス・インスタンス、地域、IAM リソース・タイプ、および IAM リソースによって定義される {{site.data.keyword.Bluemix_short}} リソース
* 役割 (リーダー、ライター、または管理者)

IAM について詳しくは、
[IBM Cloud Identity and Access Management](/docs/iam/index.html#iamoverview) を参照してください。

ポリシーの設定方法に関する例については、
[Introducing IBM Cloud IAM Service IDs and API Keys ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/blogs/bluemix/2017/10/introducing-ibm-cloud-iam-service-ids-api-keys/){:new_window} を参照してください。

## {{site.data.keyword.messagehub}} IAM リソース
{: iam_resources}

{{site.data.keyword.messagehub}} は、4 つの IAM リソース・タイプを公開します。

* クラスター
* トピック
* グループ
* txnid

トピック、グループ、および txnid については、IAM リソースを設定することにより固有のリソースを識別できます。クラスター・リソース・タイプは singleton であるため、一意的に識別する必要はありません。

## 一般的なシナリオ
{: iam_scenarios}

以下に、いくつかの一般的な {{site.data.keyword.messagehub}} シナリオと割り当てる必要があるアクセス権限を示します。

いくつかのトピックに対するプロデューサー (べき等と同様):
* クラスター・リソース・タイプに対するリーダーの役割
* 各トピック・リソース・タイプおよびトピック名リソースに対するライターの役割

トランザクションのプロデューサー:
* プロデューサーと同様
* txnid リソース・タイプおよびトランザクション ID リソースに対するライターの役割
* グループ・リソース・タイプおよびグループ ID リソースに対するリーダーの役割

コンシューマー (グループなし):
* クラスター・リソース・タイプに対するリーダーの役割
* 各トピック・リソース・タイプおよびトピック名リソースに対するリーダーの役割

グループ付きのコンシューマー:
* コンシューマーと同じ (グループなし)
* グループ・リソース・タイプおよびグループ ID リソースに対するリーダーの役割

トピックの作成または削除:
* クラスター・リソース・タイプに対するリーダーの役割
* 各トピック・リソース・タイプおよびトピック名リソースに対する管理者の役割

コンシューマー・グループの削除:
* クラスター・リソース・タイプに対するリーダーの役割
* グループ・リソース・タイプおよびグループ ID リソースに対する管理者の役割

グループ、トピック、およびオフセットのリスト、ならびにグループ、トピック、およびブローカーの構成の記述
* クラスター・リソース・タイプに対するリーダーの役割














