---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-24"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}



# {{site.data.keyword.containerlong_notm}} を使用する理由
{: #cs_ov}

{{site.data.keyword.containerlong}} は、Docker と Kubernetes テクノロジーを結合させた強力なツール、直観的なユーザー・エクスペリエンス、標準装備のセキュリティーと分離機能を提供します。これらの機能を使用することで、コンピュート・ホストから成るクラスター内でコンテナー化アプリのデプロイメント、操作、スケーリング、モニタリングを自動化することができます。
{:shortdesc}


## サービスを使用する利点
{: #benefits}

クラスターは、ネイティブの Kubernetes と {{site.data.keyword.IBM_notm}} に固有の機能を提供するコンピュート・ホストにデプロイされます。
{:shortdesc}

|利点|説明|
|-------|-----------|
|コンピュート、ネットワーク、およびストレージそれぞれのインフラストラクチャーを分離する、単一テナントの Kubernetes クラスター|<ul><li>組織の要件に適合する、カスタマイズされた独自のインフラストラクチャーを作成できる。</li><li>IBM Cloud インフラストラクチャー (SoftLayer) によって提供されるリソースを使用して、機密保護機能のある専用の Kubernetes マスター、ワーカー・ノード、仮想ネットワーク、そしてストレージをプロビジョンできる。</li><li>クラスターを使用可能にしておくために {{site.data.keyword.IBM_notm}} によって継続的にモニターされて更新される、完全に管理された Kubernetes マスターを利用できる。</li><li>トラステッド・コンピューティングを使用するベア・メタル・サーバーとしてワーカー・ノードをプロビジョンできるオプションがある。</li><li>統合されたセキュアなボリューム・サービスによって、永続データを保管し、Kubernetes ポッド間でデータを共有し、必要に応じてデータを復元することができる。</li><li>すべてのネイティブ Kubernetes API をフルサポートすることによる利点。</li></ul>|
|Vulnerability Advisor によるイメージ・セキュリティー・コンプライアンス|<ul><li>保護された独自の Docker プライベート・イメージ・レジストリーをセットアップできる。このレジストリーにイメージが格納され、組織内のすべてのユーザーによって共有される。</li><li>プライベート {{site.data.keyword.Bluemix_notm}} レジストリー内のイメージを自動スキャンすることによる利点。</li><li>イメージ内で使用されるオペレーティング・システムに固有の推奨を確認して、潜在的な脆弱性を修正できる。</li></ul>|
|クラスターの正常性に関する継続的なモニター|<ul><li>クラスター・ダッシュボードを使用して、クラスター、ワーカー・ノード、およびコンテナー・デプロイメントの正常性を素早く参照して管理できる。</li><li>{{site.data.keyword.monitoringlong}}を使用して詳細な使用量メトリックを確認し、ワークロードに合わせてクラスターを素早く拡張することができる。</li><li>{{site.data.keyword.loganalysislong}}を使用してロギング情報を参照して、クラスター・アクティビティーの詳細を確認できる。</li></ul>|
|アプリにパブリック・アクセスできるよう安全に公開する|<ul><li>インターネットからクラスター内のサービスにアクセスする方法を、パブリック IP アドレス、{{site.data.keyword.IBM_notm}} 提供の経路、独自のカスタム・ドメインの中から選択できる。</li></ul>|
|{{site.data.keyword.Bluemix_notm}} サービスの統合|<ul><li>Watson API、Blockchain、データ・サービス、モノのインターネットなどの {{site.data.keyword.Bluemix_notm}} サービスを統合してアプリに付加的な機能を追加する。</li></ul>|
{: caption="{{site.data.keyword.containerlong_notm}} の利点" caption-side="top"}



<br />


## オファーとそれらの組み合わせの比較
{: #differentiation}

{{site.data.keyword.containershort_notm}} は、{{site.data.keyword.Bluemix_notm}} Public、Dedicated、{{site.data.keyword.Bluemix_notm}} Private、またはハイブリッド構成で実行できます。
{:shortdesc}


<table>
<caption>{{site.data.keyword.containershort_notm}} の各構成の相違点</caption>
<col width="22%">
<col width="78%">
 <thead>
 <th>{{site.data.keyword.containershort_notm}} 構成</th>
 <th>説明</th>
 </thead>
 <tbody>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Public
 </td>
 <td>[共有ハードウェアまたは専用ハードウェア、あるいはベア・メタル・マシン上の](cs_clusters.html#shared_dedicated_node) {{site.data.keyword.Bluemix_notm}} Public では、{{site.data.keyword.containershort_notm}} を使用してクラウド上のクラスターでアプリをホストできます。 {{site.data.keyword.Bluemix_notm}} Public 上の {{site.data.keyword.containershort_notm}} では、Docker と Kubernetes のテクノロジーを組み合わせた強力なツール、直観的なユーザー・エクスペリエンス、標準装備のセキュリティーと分離機能を利用できます。それにより、コンピュート・ホストのクラスター内でのコンテナー化アプリのデプロイメント、運用、スケーリング、モニタリングを自動化できます。<br><br>詳しくは、[{{site.data.keyword.containershort_notm}} テクノロジー](cs_tech.html)を参照してください。
 </td>
 </tr>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Dedicated
 </td>
 <td>{{site.data.keyword.Bluemix_notm}} Dedicated は、クラウド上で {{site.data.keyword.Bluemix_notm}} Public と同じ {{site.data.keyword.containershort_notm}} 機能を提供します。 ただし、{{site.data.keyword.Bluemix_notm}} Dedicated アカウントを使用する場合、使用可能な[物理リソースはお客様のクラスター専用であり](cs_clusters.html#shared_dedicated_node)、{{site.data.keyword.IBM_notm}} の他のお客様のクラスターと共有されません。 クラスターや使用する他の {{site.data.keyword.Bluemix_notm}} サービスのために分離が必要な場合は、{{site.data.keyword.Bluemix_notm}} Dedicated 環境のセットアップを選ぶことができます。<br><br>詳しくは、[{{site.data.keyword.Bluemix_notm}} Dedicated でのクラスターの概説](cs_dedicated.html#dedicated)を参照してください。
 </td>
 </tr>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Private
 </td>
 <td>{{site.data.keyword.Bluemix_notm}} Private は、ユーザー自身のマシン上にローカルにインストールできるアプリケーション・プラットフォームです。 ファイアウォールの内側にあるユーザー所有の制御された環境で、社内用のコンテナー化アプリを開発して管理する必要がある場合は、{{site.data.keyword.Bluemix_notm}} Private で {{site.data.keyword.containershort_notm}} を使用することを選択できます。 <br><br>詳しくは、[{{site.data.keyword.Bluemix_notm}} Private の製品情報 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud/private) および [資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_1.2.0/kc_welcome_containers.html) を参照してください。
 </td>
 </tr>
 <tr>
 <td>ハイブリッド構成
 </td>
 <td>ハイブリッドとは、{{site.data.keyword.Bluemix_notm}} Public または Dedicated で実行されるサービスと、{{site.data.keyword.Bluemix_notm}} Private 内のアプリなどのオンプレミスで実行される他のサービスを組み合わせて使用することです。 ハイブリッド構成の例を以下に示します。 <ul><li>{{site.data.keyword.Bluemix_notm}} Public で {{site.data.keyword.containershort_notm}} を使用してクラスターをプロビジョンしながら、そのクラスターを社内データベースに接続する。</li><li>{{site.data.keyword.Bluemix_notm}} Private で {{site.data.keyword.containershort_notm}} を使用してクラスターをプロビジョンしながら、アプリをそのクラスターにデプロイする。 ただし、そのアプリは {{site.data.keyword.Bluemix_notm}} Public の {{site.data.keyword.ibmwatson}} サービス ({{site.data.keyword.toneanalyzershort}} など) を使用する可能性がある。</li></ul><br>{{site.data.keyword.Bluemix_notm}} Public または Dedicated で実行しているサービスとオンプレミスで実行しているサービスの間の通信を有効にするには、[VPN 接続をセットアップする](cs_vpn.html)必要があります。
 </td>
 </tr>
 </tbody>
</table>

<br />


## フリー・クラスターと標準クラスターの比較
{: #cluster_types}

1 つのフリー・クラスターまたは任意の数の標準クラスターを作成できます。 フリー・クラスターを試して、いくつかの Kubernetes 機能の使用法を習得してテストすることもできますし、標準クラスターを作成して、Kubernetes の機能全体を使用してアプリをデプロイすることもできます。
{:shortdesc}

|特性|フリー・クラスター|標準クラスター|
|---------------|-------------|-----------------|
|[クラスター内ネットワーキング](cs_secure.html#in_cluster_network)
|[NFS ファイル・ベースの永続ストレージのボリューム](cs_storage.html#planning)| |<img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" />|
|[ロード・バランサー・サービスによる安定 IP アドレスへのパブリック・ネットワークまたはプライベート・ネットワークのアプリ・アクセス](cs_loadbalancer.html#planning)| |<img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" />|
|[Ingress サービスによる安定 IP アドレスとカスタマイズ可能 URL へのパブリック・ネットワークのアプリ・アクセス](cs_ingress.html#planning)| |<img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" />|
|[ポータブル・パブリック IP アドレス](cs_subnets.html#manage)| |<img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" />|
|[ロギングとモニタリング](cs_health.html#logging)| |<img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" />|
|[物理 (ベア・メタル) サーバー上にワーカー・ノードをプロビジョンするためのオプション](cs_clusters.html#shared_dedicated_node) | |<img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" />|
|[トラステッド・コンピューティングを使用するベア・メタル・ワーカーをプロビジョンするためのオプション](cs_clusters.html#shared_dedicated_node) | |<img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" />|
|[{{site.data.keyword.Bluemix_dedicated_notm}} で使用可能](cs_dedicated.html#dedicated_environment)| |<img src="images/confirm.svg" width="32" alt="機能は使用可能" style="width:32px;" />|
{: caption="フリー・クラスターと標準クラスターの特性" caption-side="top"}

<br />




{: #responsibilities}
**注**: サービスを使用する際の責任と {{site.data.keyword.containerlong}} のご利用条件をお探しですか?

{: #terms}
[{{site.data.keyword.containershort_notm}} の責任](cs_responsibilities.html)を参照してください。
