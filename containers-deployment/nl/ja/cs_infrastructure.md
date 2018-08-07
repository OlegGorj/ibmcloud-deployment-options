---

copyright:
  years: 2014, 2018
lastupdated: "2018-4-20"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオへのアクセス
{: #infrastructure}

標準の Kubernetes クラスターを作成するには、IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオにアクセスする必要があります。 {{site.data.keyword.containerlong}} で、Kubernetes クラスターのワーカー・ノード、ポータブル・パブリック IP アドレス、永続ストレージなど、有料のインフラストラクチャー・リソースを要求するには、このアクセスが必要です。
{:shortdesc}

## IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオへのアクセス
{: #unify_accounts}

自動アカウント・リンクを有効にした後に作成した {{site.data.keyword.Bluemix_notm}} の従量制課金アカウントは、IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオにアクセスできるように既にセットアップされています。 追加の構成を行わなくても、クラスターのためのインフラストラクチャー・リソースを購入できます。
{:shortdesc}

他の {{site.data.keyword.Bluemix_notm}} アカウント・タイプを持つユーザー、または {{site.data.keyword.Bluemix_notm}} アカウントにリンクされていない既存の IBM Cloud インフラストラクチャー (SoftLayer) アカウントを持つユーザーは、標準クラスターを作成できるようにアカウントを構成する必要があります。
{:shortdesc}

以下の表を確認して、各アカウント・タイプの選択肢を見つけてください。

|アカウント・タイプ|説明|標準クラスターを作成するための選択肢|
|------------|-----------|----------------------------------------------|
|ライト・アカウント|ライト・アカウントではクラスターをプロビジョンできません。|[ライト・アカウントを {{site.data.keyword.Bluemix_notm}} 従量課金 (PAYG) アカウントにアップグレードします](/docs/account/index.html#billableacts)。従量課金アカウントには、IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオへのアクセスがセットアップされています。|
|以前の従量制課金アカウント|自動アカウント・リンクが使用できるようになる前に作成された従量制課金アカウントには、IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオにアクセスする機能がありません。<p>既存の IBM Cloud インフラストラクチャー (SoftLayer) アカウントがあっても、そのアカウントを以前の従量制課金アカウントにリンクすることはできません。</p>|選択肢 1: [新しい従量制課金アカウントを作成します](/docs/account/index.html#billableacts)。このアカウントには、IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオへのアクセスがセットアップされます。 この選択肢を取る場合は、2 つの異なる {{site.data.keyword.Bluemix_notm}} アカウントを所有し、2 つの異なる課金が行われることになります。<p>以前の従量制課金アカウントを引き続き使用して標準クラスターを作成する場合は、新しい従量制課金アカウントを使用して、IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオにアクセスするための API キーを生成します。 そして、その API キーを以前の従量制課金アカウントに設定する必要があります。 詳しくは、[以前の従量制課金アカウントとサブスクリプション・アカウント用の API キーの生成](#old_account)を参照してください。 IBM Cloud インフラストラクチャー (SoftLayer) リソースは新しい従量制課金アカウントを介して課金されることに注意してください。</p></br><p>選択肢 2: 既存の IBM Cloud インフラストラクチャー (SoftLayer) アカウントを使用したい場合は、{{site.data.keyword.Bluemix_notm}} アカウントに[資格情報を設定](cs_cli_reference.html#cs_credentials_set)します。</p><p>**注:** {{site.data.keyword.Bluemix_notm}} アカウントで使用する IBM Cloud インフラストラクチャー (SoftLayer) アカウントには、スーパーユーザー権限がセットアップされている必要があります。</p>|
|サブスクリプション・アカウント|サブスクリプション・アカウントには、IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオへのアクセスがセットアップされていません。|選択肢 1: [新しい従量制課金アカウントを作成します](/docs/account/index.html#billableacts)。このアカウントには、IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオへのアクセスがセットアップされます。 この選択肢を取る場合は、2 つの異なる {{site.data.keyword.Bluemix_notm}} アカウントを所有し、2 つの異なる課金が行われることになります。<p>サブスクリプション・アカウントを引き続き使用して標準クラスターを作成する場合は、新しい従量制課金アカウントを使用して、IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオにアクセスするための API キーを生成します。 そして、その API キーをサブスクリプション・アカウントに設定する必要があります。 詳しくは、[以前の従量制課金アカウントとサブスクリプション・アカウント用の API キーの生成](#old_account)を参照してください。 IBM Cloud インフラストラクチャー (SoftLayer) リソースは新しい従量制課金アカウントを介して課金されることに注意してください。</p></br><p>選択肢 2: 既存の IBM Cloud インフラストラクチャー (SoftLayer) アカウントを使用したい場合は、{{site.data.keyword.Bluemix_notm}} アカウントに[資格情報を設定](cs_cli_reference.html#cs_credentials_set)します。<p>**注:** {{site.data.keyword.Bluemix_notm}} アカウントで使用する IBM Cloud インフラストラクチャー (SoftLayer) アカウントには、スーパーユーザー権限がセットアップされている必要があります。</p>|
|IBM Cloud インフラストラクチャー (SoftLayer) アカウントがあり、{{site.data.keyword.Bluemix_notm}} アカウントはない|標準クラスターを作成するには、{{site.data.keyword.Bluemix_notm}} アカウントが必要です。|<p>[新しい従量制課金アカウントを作成します](/docs/account/index.html#billableacts)。このアカウントには、IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオへのアクセスがセットアップされます。 この選択肢を取る場合は、自動で IBM Cloud インフラストラクチャー (SoftLayer) が作成されます。 2 つの別個の IBM Cloud インフラストラクチャー (SoftLayer) アカウントを所有し、課金されることになります。</p>|

<br />


## {{site.data.keyword.Bluemix_notm}} アカウントで使用する IBM Cloud インフラストラクチャー (SoftLayer) API キーの生成
{: #old_account}

以前の従量制課金アカウントまたはサブスクリプション・アカウントを使用して標準クラスターを作成する場合は、新しい従量制課金アカウントを使用して API キーを生成し、その API キーを以前のアカウントに設定します。
{:shortdesc}

開始する前に、IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオへのアクセスが自動的にセットアップされる {{site.data.keyword.Bluemix_notm}} 従量制課金アカウントを作成します。

1.  新しい従量制課金アカウント用に作成した {{site.data.keyword.ibmid}} とパスワードを使用して、[IBM Cloud インフラストラクチャー (SoftLayer) ポータル ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://control.softlayer.com/) にログインします。
2.  **「アカウント」**を選択し、次に**「ユーザー」**を選択します。
3.  **「生成」**をクリックして、新しい従量制課金アカウント用の IBM Cloud インフラストラクチャー (SoftLayer) の API キーを生成します。
4.  その API キーをコピーします。
5.  CLI から、以前の従量制課金アカウントまたはサブスクリプション・アカウントの {{site.data.keyword.ibmid}} とパスワードを使用して {{site.data.keyword.Bluemix_notm}} にログインします。

  ```
  bx login
  ```
  {: pre}

6.  IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオにアクセスするために、先ほど生成した API キーを設定します。 `<API_key>` を API キーに置き換え、`<username>` を新しい従量制課金アカウントの {{site.data.keyword.ibmid}} に置き換えます。

  ```
  bx cs credentials-set --infrastructure-api-key <API_key> --infrastructure-username <username>
  ```
  {: pre}

7.  [標準クラスターの作成](cs_clusters.html#clusters_cli)を開始します。

**注:** 生成した後に API キーを確認するには、手順 1 と 2 の後に、**「API キー」**セクションで**「表示 (View)」**をクリックして、ユーザー ID の API キーを参照します。

