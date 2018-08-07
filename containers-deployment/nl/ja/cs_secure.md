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




# {{site.data.keyword.containerlong_notm}}のセキュリティー
{: #security}

{{site.data.keyword.containerlong}} の標準装備のセキュリティー・フィーチャーをリスク分析とセキュリティー保護に使用できます。 これらのフィーチャーは、Kubernetes クラスター・インフラストラクチャーとネットワーク通信を保護し、コンピュート・リソースを分離し、インフラストラクチャー・コンポーネントとコンテナー・デプロイメントにおけるセキュリティー・コンプライアンスを確保するためのものです。
{: shortdesc}



## クラスター・コンポーネントによるセキュリティー
{: #cluster}

各 {{site.data.keyword.containerlong_notm}} クラスターには、その[マスター](#master)と[ワーカー](#worker)・ノードに組み込まれたセキュリティー・フィーチャーがあります。
{: shortdesc}

ファイアウォールを使用している場合、または公共のインターネットのエンドポイントへのアクセスが企業ネットワーク・ポリシーによって禁止されているときにローカル・システムから `kubectl` コマンドを実行する場合は、[ファイアウォールでポートを開きます](cs_firewall.html#firewall)。クラスター内のアプリをオンプレミス・ネットワークまたはクラスター外の他のアプリに接続するには、[VPN 接続をセットアップ](cs_vpn.html#vpn)します。

次の図には、セキュリティー・フィーチャーが、Kubernetes マスター、ワーカー・ノード、コンテナー・イメージのグループ別に示されています。

<img src="images/cs_security.png" width="400" alt="{{site.data.keyword.containershort_notm}} クラスター・セキュリティー" style="width:400px; border-style: none"/>


<table summary="表の 1 行目は両方の列にまたがっています。残りの行は左から右に読みます。1 列目はセキュリティー・コンポーネント、2 列目は対応する機能です。">
<caption>コンポーネントによるセキュリティー</caption>
  <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> {{site.data.keyword.containershort_notm}} での組み込みのクラスター・セキュリティー設定</th>
  </thead>
  <tbody>
    <tr>
      <td>Kubernetes マスター</td>
      <td>各クラスターの Kubernetes マスターは IBM によって管理され、高い可用性を備えています。 マスターには、セキュリティー・コンプライアンスを確保し、ワーカー・ノードとの間の通信を保護するための {{site.data.keyword.containershort_notm}} セキュリティー設定が含まれています。 セキュリティー更新は、IBM が必要に応じて行います。 専用 Kubernetes マスターは、クラスター内のすべての Kubernetes リソースを一元的に制御してモニターします。 デプロイメント要件とクラスターの容量に基づき、Kubernetes マスターは、コンテナー化されたアプリが使用可能なワーカー・ノードにデプロイされるように自動的にスケジュール設定します。 詳しくは、[Kubernetes マスターのセキュリティー](#master)を参照してください。</td>
    </tr>
    <tr>
      <td>ワーカー・ノード</td>
      <td>コンテナーはワーカー・ノードにデプロイされます。ワーカー・ノードは特定の 1 つのクラスターのために専用で使用されるので、IBM のお客様に提供されるコンピュート、ネットワーク、およびストレージがそれぞれ分離されます。 {{site.data.keyword.containershort_notm}} が備える組み込みセキュリティー機能により、プライベート・ネットワークとパブリック・ネットワークのワーカー・ノードのセキュリティーを維持し、ワーカー・ノードのセキュリティー・コンプライアンスを遵守できます。 詳しくは、[ワーカー・ノードのセキュリティー](#worker)を参照してください。 さらに、[Calico ネットワーク・ポリシー](cs_network_policy.html#network_policies)を追加して、ワーカー・ノード上のポッドとの間のネットワーク・トラフィックのどれを許可し、どれをブロックするかを指定することができます。</td>
    </tr>
    <tr>
      <td>イメージ</td>
      <td>クラスター管理者は、保護された独自の Docker イメージ・リポジトリーを {{site.data.keyword.registryshort_notm}} にセットアップできます。このレジストリーに Docker イメージを保管してクラスター・ユーザー間で共有することができます。 安全にコンテナー・デプロイメントを行うために、プライベート・レジストリー内のすべてのイメージが Vulnerability Advisor によってスキャンされます。 Vulnerability Advisor は {{site.data.keyword.registryshort_notm}}のコンポーネントであり、潜在的な脆弱性がないかスキャンし、セキュリティーに関する推奨を行い、脆弱性を解決するための方法を提示します。 詳しくは、[{{site.data.keyword.containershort_notm}} のイメージ・セキュリティー](#images)を参照してください。</td>
    </tr>
  </tbody>
</table>

<br />


## Kubernetes マスター
{: #master}

Kubernetes マスターを保護し、クラスター・ネットワーク通信をセキュリティーで保護するための Kubernetes マスターの組み込みセキュリティー機能について説明します。
{: shortdesc}

<dl>
  <dt>完全に管理される専用 Kubernetes マスター</dt>
    <dd>{{site.data.keyword.containershort_notm}} に含まれる Kubernetes クラスターはすべて、IBM が所有する IBM Cloud インフラストラクチャー (SoftLayer) アカウントで、IBM が管理する専用 Kubernetes マスターにより制御されます。 Kubernetes マスターは、他の IBM のお客様や同じ IBM アカウント内の異なるクラスターとは共有されない以下の専用コンポーネントを使用してセットアップされます。
      <ul><li>etcd データ・ストア: サービス、デプロイメント、ポッドなどのクラスターのすべての Kubernetes リソースを保管します。 Kubernetes ConfigMaps および Secrets は、ポッドで実行されるアプリで使用できるように、キー値ペアとして保管されるアプリ・データです。 etcd のデータは、IBM 管理の暗号化ディスクに保管され、毎日バックアップされます。 ポッドに送信されるデータは、TLS で暗号化されるので、データの保護と完全性が確保されます。 </li>
      <li>kube-apiserver: ワーカー・ノードから Kubernetes マスターへのすべての要求のメインエントリー・ポイントとなります。 kube-apiserver は要求を検証して処理し、etcd データ・ストアに対する読み取り/書き込みを行うことができます。</li>
      <li>kube-scheduler: ポッドをどこにデプロイするかを決定します。このとき、キャパシティーとパフォーマンスのニーズ、ハードウェアとソフトウェアのポリシー制約、アンチアフィニティー仕様、およびワークロード要件が考慮されます。要件に合致するワーカー・ノードが見つからなければ、ポッドはクラスターにデプロイされません。</li>
      <li>kube-controller-manager: レプリカ・セットをモニターし、対応するポッドを作成して目的の状態を実現するためのコンポーネントです。</li>
      <li>OpenVPN: {{site.data.keyword.containershort_notm}} 固有のコンポーネントであり、Kubernetes マスターからワーカー・ノードへのすべての通信のためのセキュア・ネットワーク接続を提供します。</li></ul></dd>
  <dt>ワーカー・ノードから Kubernetes マスターへのすべての通信のための TLS セキュア・ネットワーク接続</dt>
    <dd>Kubernetes マスターへの接続を保護するために、{{site.data.keyword.containershort_notm}} によって、kube-apiserver および etcd データ・ストアとの間でやり取りされる通信を暗号化する TLS 証明書が生成されます。これらの証明書は、クラスター間で共有されたり、Kubernetes マスター・コンポーネント間で共有されたりすることはありません。</dd>
  <dt>Kubernetes マスターからワーカー・ノードへのすべての通信のための OpenVPN セキュア・ネットワーク接続</dt>
    <dd>Kubernetes は、`https` プロトコルを使用して Kubernetes マスターとワーカー・ノードの間の通信を保護しますが、デフォルトでは、ワーカー・ノードで認証は行われません。 この通信を保護するために、{{site.data.keyword.containershort_notm}} は、クラスターの作成時に、Kubernetes マスターとワーカー・ノードの間の OpenVPN 接続を自動的にセットアップします。</dd>
  <dt>継続的な Kubernetes マスター・ネットワーク・モニタリング</dt>
    <dd>どの Kubernetes マスターも IBM によって継続的にモニターされ、処理レベルのサービス妨害 (DOS) 攻撃を制御して対処します。</dd>
  <dt>Kubernetes マスター・ノードのセキュリティー・コンプライアンス</dt>
    <dd>{{site.data.keyword.containershort_notm}} は、Kubernetes マスターがデプロイされたすべてのノードにおいて、Kubernetes および OS 固有のセキュリティー修正で検出された脆弱性を自動的にスキャンします。脆弱性が見つかった場合、{{site.data.keyword.containershort_notm}} はユーザーに代わって自動的に修正を適用し、脆弱性を解決して、マスター・ノードが確実に保護されるようにします。</dd>
</dl>

<br />


## ワーカー・ノード
{: #worker}

ワーカー・ノード環境を保護し、リソース、ネットワーク、ストレージの分離を保証する組み込みのワーカー・ノード・セキュリティー機能について説明します。
{: shortdesc}

<dl>
  <dt>ワーカー・ノードの所有権</dt>
    <dd>ワーカー・ノードの所有権は、作成するクラスターのタイプによって異なります。 <p> フリー・クラスター内のワーカー・ノードは、IBM の所有する IBM Cloud インフラストラクチャー (SoftLayer) アカウントにプロビジョンされます。 ユーザーはワーカー・ノードにアプリをデプロイすることはできますが、ワーカー・ノードで設定を変更したり、追加のソフトウェアをインストールしたりすることはできません。</p>
    <p>標準クラスター内のワーカー・ノードは、お客様の IBM Cloud パブリック・アカウントまたは専用アカウントに関連付けられている IBM Cloud インフラストラクチャー (SoftLayer) アカウントにプロビジョンされます。 ワーカー・ノードは、お客様が所有します。 お客様は、{{site.data.keyword.containerlong}} の規定に従って、ワーカー・ノードでセキュリティー設定を変更したり、追加のソフトウェアをインストールしたりすることができます。</p> </dd>
  <dt>コンピュート、ネットワーク、およびストレージ・インフラストラクチャーの分離</dt>
    <dd><p>お客様がクラスターを作成すると、IBM はワーカー・ノードを仮想マシンとしてプロビジョンします。 ワーカー・ノードは特定のクラスターの専用であり、他のクラスターのワークロードをホストすることはありません。</p>
    <p> ワーカー・ノードのネットワーク・パフォーマンスと分離の品質を保証するために、すべての {{site.data.keyword.Bluemix_notm}} アカウントには、IBM Cloud インフラストラクチャー (SoftLayer) VLAN がセットアップされます。 また、ワーカー・ノードをプライベート VLAN だけに接続して、プライベートとして指定することもできます。</p> <p>クラスターにデータを保持する場合は、IBM Cloud インフラストラクチャー (SoftLayer) の NFS ベースの専用ファイル・ストレージをプロビジョンし、このプラットフォームの組み込みデータ・セキュリティー機能を利用することができます。</p></dd>
  <dt>保護されたワーカー・ノードのセットアップ</dt>
    <dd><p>すべてのワーカー・ノードは Ubuntu オペレーティング・システムを使用してセットアップされ、ワーカー・ノードの所有者が変更することはできません。 ワーカー・ノードのオペレーティング・システムが Ubuntu であるため、ワーカー・ノードにデプロイするすべてのコンテナーは、Ubuntu カーネルを使用する Linux ディストリビューションを使用する必要があります。 別の方法でカーネルと通信する必要がある Linux ディストリビューションは使用できません。 ワーカー・ノードのオペレーティング・システムを潜在的な攻撃から保護するために、すべてのワーカー・ノードに、Linux iptable ルールによって適用されるエキスパート・ファイアウォール設定が構成されます。</p>
    <p>Kubernetes 上で実行されるコンテナーはすべて、定義済みの Calico ネットワーク・ポリシー設定で保護されます。この設定は、クラスター作成時にワーカー・ノードごとに構成されます。 このようなセットアップにより、ワーカー・ノードとポッドの間のセキュア・ネットワーク通信が確保されます。</p>
    <p>ワーカー・ノード上の SSH アクセスは無効になっています。 標準クラスターを使用していて、ワーカー・ノードに追加機能をインストールする場合は、あらゆるワーカー・ノードで実行するあらゆるアクションについて、[Kubernetes デーモン・セット ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset) を使用できます。1 回限りのアクションを実行する必要がある場合は、[Kubernetes ジョブ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/) を使用します。</p></dd>
  <dt>Kubernetes ワーカー・ノードのセキュリティー・コンプライアンス</dt>
    <dd>IBM は、社内と社外のセキュリティー顧問チームと協力して、セキュリティー・コンプライアンスにおける潜在的な脆弱性と取り組んでいます。 <b>重要</b>: `bx cs worker-update` [コマンド](cs_cli_reference.html#cs_worker_update)を定期的 (毎月など) に使用して、更新とセキュリティー・パッチをオペレーティング・システムに導入し、Kubernetes バージョンを更新してください。 更新が利用可能になると、`bx cs workers <cluster_name>` や `bx cs worker-get <cluster_name> <worker_ID>` コマンドなどを使用してワーカー・ノードの情報を表示したときに通知されます。</dd>
  <dt>物理 (ベア・メタル) サーバーにノードをデプロイするオプション</dt>
    <dd>ワーカー・ノードを (仮想サーバー・インスタンスではなく) ベアメタル物理サーバーにプロビジョンするよう選択する場合は、メモリーや CPU などのコンピュート・ホストをさらに細かく制御できます。このセットアップには、ホスト上で稼働する仮想マシンに物理リソースを割り振る仮想マシン・ハイパーバイザーは含まれません。 むしろ、ベア・メタル・マシンのすべてのリソースがこのワーカーの専用であるため、リソースを共有したりパフォーマンスを低下させたりする「ノイジー・ネイバー」について心配する必要がありません。 ベア・メタル・サーバーはお客様専用であり、そのすべてのリソースをクラスターに使用できます。</dd>
  <dt id="trusted_compute">トラステッド・コンピューティングを使用する {{site.data.keyword.containershort_notm}}</dt>
    <dd><p>トラステッド・コンピューティングをサポートする[ベア・メタルにクラスターをデプロイ](cs_clusters.html#clusters_ui)した場合は、トラストを有効にすることができます。 トラステッド・コンピューティングをサポートするクラスター内の各ベア・メタル・ワーカー・ノード (クラスターに将来追加するノードも含む) の Trusted Platform Module (TPM) チップが有効になります。 したがって、トラストを有効にした後に、クラスターのトラストを無効にすることはできません。 トラスト・サーバーがマスター・ノードにデプロイされ、トラスト・エージェントがワーカー・ノードにポッドとしてデプロイされます。 ワーカー・ノードが始動すると、トラスト・エージェント・ポッドがプロセスの各ステージをモニターします。</p>
    <p>例えば、無許可ユーザーがシステムにアクセスし、データを収集するためのロジックを追加して OS カーネルを変更すると、トラスト・エージェントはこの変更を検出し、そのノードを信頼できないものとしてマークします。トラステッド・コンピューティングを使用すると、ワーカー・ノードが改ざんされていないことを検証できます。</p>
    <p><strong>注</strong>: トラステッド・コンピューティングは、一部のベアメタル・マシン・タイプでのみ使用できます。例えば、`mgXc` GPU フレーバーではトラステッド・コンピューティングはサポートされません。</p></dd>
  <dt id="encrypted_disks">暗号化ディスク</dt>
    <dd><p>{{site.data.keyword.containershort_notm}} は、ワーカー・ノードのプロビジョン時にデフォルトですべてのワーカー・ノード用に 2 つのローカル SSD 暗号化データ・パーティションを提供します。 1 つ目のパーティションは暗号化されず、2 つ目のパーティションは _/var/lib/docker_ にマウントされ、LUKS 暗号鍵を使用してアンロックされます。 各 Kubernetes クラスター内の各ワーカーには、独自の固有の LUKS 暗号キーがあり、{{site.data.keyword.containershort_notm}} によって管理されます。 クラスターを作成する際やワーカー・ノードを既存のクラスターに追加する際に、この鍵は安全にプルされてから、暗号化ディスクのアンロック後に破棄されます。</p>
    <p><b>注</b>: 暗号化はディスク入出力のパフォーマンスに影響します。 高性能のディスク入出力が必要なワークロードの場合、暗号化を有効/無効にした両方のクラスターをテストし、暗号化をオフにするかどうかの決定に役立ててください。</p></dd>
  <dt>IBM Cloud インフラストラクチャー (SoftLayer) ネットワーク・ファイアウォールのサポート</dt>
    <dd>{{site.data.keyword.containershort_notm}} は、すべての [ IBM Cloud インフラストラクチャー (SoftLayer) ファイアウォール・オファリング ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud-computing/bluemix/network-security) に対応しています。 {{site.data.keyword.Bluemix_notm}} Public では、カスタム・ネットワーク・ポリシーをファイアウォールにセットアップして、標準クラスターのための専用ネットワーク・セキュリティーを設定し、ネットワーク侵入を検出して対処することができます。 例えば、ファイアウォールとして機能し、不要なトラフィックをブロックするように [Virtual Router Appliance](/docs/infrastructure/virtual-router-appliance/about.html) をセットアップできます。ファイアウォールをセットアップする場合は、マスター・ノードとワーカー・ノードが通信できるように、地域ごとに[必要なポートと IP アドレスを開く](cs_firewall.html#firewall)必要もあります。</dd>
  <dt>サービスをプライベートにしておくか、サービスとアプリを選択的に公共のインターネットに公開する</dt>
    <dd>サービスとアプリをプライベートにしておくことで、組み込みのセキュリティー機能を利用してワーカー・ノードとポッドの間のセキュアな通信を確保できます。サービスとアプリを公共のインターネットに公開する場合は、Ingress とロード・バランサーのサポートを活用してサービスを安全に公開することができます。</dd>
  <dt>ワーカー・ノードとアプリをオンプレミス・データ・センターにセキュアに接続する</dt>
    <dd><p>ワーカー・ノードとアプリをオンプレミス・データ・センターに接続するには、strongSwan サービス、Virtual Router Appliance、または Fortigate Security Appliance を使用して VPN IPsec エンドポイントを構成します。</p>
    <ul><li><b>strongSwan IPsec VPN サービス</b>: Kubernetes クラスターをオンプレミス・ネットワークとセキュアに接続する [strongSwan IPSec VPN サービス ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.strongswan.org/) をセットアップできます。strongSwan IPSec VPN サービスは、業界標準の Internet Protocol Security (IPsec) プロトコル・スイートに基づき、インターネット上にセキュアなエンドツーエンドの通信チャネルを確立します。 クラスターとオンプレミス・ネットワークの間にセキュアな接続をセットアップするためには、クラスター内のポッドに直接、[strongSwan IPSec VPN サービスを構成してデプロイします](cs_vpn.html#vpn-setup)。
    </li>
    <li><b>Virtual Router Appliance (VRA) または Fortigate Security Appliance (FSA)</b>: [VRA](/docs/infrastructure/virtual-router-appliance/about.html) または [FSA](/docs/infrastructure/fortigate-10g/about.html) をセットアップして、IPSec VPN エンドポイントを構成することもできます。このオプションは、クラスターが大きい場合、VPN 経由で非 Kubernetes リソースにアクセスする場合、または 1 つの VPN で複数のクラスターにアクセスする場合に役立ちます。VRA を構成するには、[Vyatta を使用した VRA 接続のセットアップ](cs_vpn.html#vyatta)を参照してください。</li></ul></dd>

  <dt>クラスター・アクティビティーの継続的なモニタリングとロギング</dt>
    <dd>標準クラスターの場合、すべてのクラスター関連イベントをログに記録して、{{site.data.keyword.loganalysislong_notm}} および {{site.data.keyword.monitoringlong_notm}} に送信できます。このようなイベントには、ワーカー・ノードの追加、ローリング更新の進行状況、キャパシティー使用状況情報などがあります。[クラスター・ロギング](/docs/containers/cs_health.html#logging)および[クラスター・モニタリング](/docs/containers/cs_health.html#view_metrics)を構成して、モニターするイベントを決定できます。</dd>
</dl>

<br />


## イメージ
{: #images}

標準装備のセキュリティー・フィーチャーを使用して、イメージのセキュリティーと保全性を管理します。
{: shortdesc}

<dl>
<dt>{{site.data.keyword.registryshort_notm}} の保護された Docker プライベート・イメージ・リポジトリー</dt>
  <dd>IBM によってホストおよび管理されている、マルチテナントで可用性が高く、スケーラブルなプライベート・イメージ・レジストリー内に独自の Docker イメージ・リポジトリーをセットアップします。このレジストリーを使用することで、Docker イメージを作成して安全に保管し、クラスター・ユーザー間で共有することができます。
  <p>コンテナー・イメージを使用する際の[個人情報の保護](cs_secure.html#pi)の詳細を確認してください。</p></dd>
<dt>イメージのセキュリティー・コンプライアンス</dt>
  <dd>{{site.data.keyword.registryshort_notm}}を使用する際、Vulnerability Advisor に標準装備のセキュリティー・スキャンを利用できます。 名前空間にプッシュされるどのイメージも、CentOS、Debian、Red Hat、および Ubuntu の既知の問題のデータベースに基づいて、脆弱性が自動的にスキャンされます。 脆弱性が検出されると、それらを解決してイメージの保全性とセキュリティーを確保する方法を Vulnerability Advisor が提示します。</dd>
</dl>

イメージの脆弱性評価を表示する方法については、[Vulnerability Advisor の文書を参照してください](/docs/services/va/va_index.html#va_registry_cli)。

<br />


## クラスター内ネットワーキング
{: #in_cluster_network}

ワーカー・ノードとポッド間の保護されたクラスター内ネットワーク通信は、プライベート仮想ローカル・エリア・ネットワーク (VLAN) で実現されます。 VLAN では、ワーカー・ノードとポッドをまとめたグループが同じ物理ワイヤーに接続されているかのように構成されます。
{:shortdesc}

クラスターを作成すると、すべてのクラスターはプライベート VLAN に自動的に接続されます。 ワーカー・ノードに割り当てられるプライベート IP アドレスは、クラスター作成時にプライベート VLAN によって決定されます。

|クラスター・タイプ|クラスターのプライベート VLAN の管理者|
|------------|-------------------------------------------|
|{{site.data.keyword.Bluemix_notm}} 内のフリー・クラスター|{{site.data.keyword.IBM_notm}}|
|{{site.data.keyword.Bluemix_notm}} 内の標準クラスター|IBM Cloud インフラストラクチャー (SoftLayer) アカウントを使用するお客様 <p>**ヒント:** アカウント内のすべての VLAN にアクセスできるようにするには、[VLAN Spanning](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning)をオンにします。</p>|
{: caption="プライベート VLAN のマネージャー" caption-side="top"}

ワーカー・ノードにデプロイされるすべてのポッドにも、プライベート IP アドレスが割り当てられます。 ポッドには 172.30.0.0/16 のプライベート・アドレス範囲で IP が割り当てられ、ワーカー・ノード間でのみルーティングされます。 競合を避けるために、ご使用のワーカー・ノードと通信するノードにはこの IP 範囲を使用しないでください。ワーカー・ノードとポッドは、プライベート IP アドレスを使用してプライベート・ネットワーク上で安全に通信できます。 しかし、ポッドが異常終了した場合やワーカー・ノードを再作成する必要がある場合は、新しいプライベート IP アドレスが割り当てられます。

デフォルトでは、高可用性が必要とされるアプリの変化するプライベート IP アドレスを追跡することは困難です。これを回避するには、組み込みの Kubernetes サービス・ディスカバリー機能を使用して、プライベート・ネットワーク上のクラスター IP サービスとしてアプリを公開します。Kubernetes サービスによってポッドのセットをグループ化し、クラスター内の他のサービスからそれらのポッドにアクセスするためのネットワーク接続を提供します。そうすれば、各ポッドの実際のプライベート IP アドレスを公開する必要はありません。 クラスター IP サービスを作成すると、10.10.10.0/24 のプライベート・アドレス範囲からそのサービスにプライベート IP アドレスが割り当てられます。 ポッドのプライベート・アドレス範囲と同様に、この IP 範囲はワーカー・ノードと通信するノードには使用しないでください。この IP アドレスはクラスター内でのみアクセス可能です。 インターネットからこの IP アドレスにアクセスすることはできません。 同時に、サービスの DNS 参照エントリーが作成され、クラスターの kube-dns コンポーネントに保管されます。 DNS エントリーには、サービスの名前、サービスが作成された名前空間、割り当てられたプライベート・クラスター IP アドレスへのリンクが設定されます。

クラスター IP サービスの背後にあるポッドにアクセスする場合、アプリでは、サービスのプライベート・クラスター IP アドレスを使用するか、サービスの名前を使用して要求を送信します。サービスの名前を使用した場合、名前は kube-dns コンポーネント内で検索され、サービスのプライベート・クラスター IP アドレスにルーティングされます。 要求がサービスに到達すると、ポッドのプライベート IP アドレスやデプロイ先のワーカー・ノードに関係なく、すべての要求がサービスによってポッドに等しく転送されます。

クラスター IP タイプのサービスを作成する方法について詳しくは、[Kubernetes サービス ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services---service-types) を参照してください。

Kubernetes クラスター内のアプリをオンプレミス・ネットワークに安全に接続するには、[VPN 接続のセットアップ](cs_vpn.html#vpn)を参照してください。外部ネットワーク通信のためにアプリを公開するには、[アプリへのパブリック・アクセスを許可する方法](cs_network_planning.html#public_access)を参照してください。


<br />


## クラスターの信頼
{: cs_trust}

セキュリティー機能が豊富な環境でコンテナー化アプリをデプロイできるように、{{site.data.keyword.containerlong_notm}} は[クラスター・コンポーネント向けの機能](#cluster)をデフォルトで数多く提供します。 クラスターの信頼レベルを高めて、より確実に、意図したことだけがクラスターで行われるようにします。 以下の図に示すように、さまざまな方法でクラスターに信頼を実装できます。
{:shortdesc}

![信頼できるコンテンツを使用してコンテナーをデプロイする](images/trusted_story.png)

1.  **トラステッド・コンピューティングを使用した {{site.data.keyword.containerlong_notm}}**: ベア・メタル・クラスターでは、トラストを有効にすることができます。 トラスト・エージェントがハードウェアの始動処理をモニターして変更を報告するので、ベア・メタル・ワーカー・ノードが改ざんされていないことを検証できます。 トラステッド・コンピューティングを使用すると、検証されたベア・メタル・ホストにコンテナーをデプロイして、信頼できるハードウェア上でワークロードを実行できます。 GPU などの一部のベアメタル・マシンでは、トラステッド・コンピューティングがサポートされないことに注意してください。[トラステッド・コンピューティングの仕組みについて詳しくは、こちらをご覧ください](#trusted_compute)。

2.  **イメージのコンテンツの信頼**: {{site.data.keyword.registryshort_notm}} でコンテンツの信頼を有効にして、イメージの完全性を確保します。 信頼できるコンテンツを使用する場合は、信頼できるイメージとしてイメージに署名できるユーザーを制御できます。 信頼できる署名者がイメージをレジストリーにプッシュすると、ユーザーはその署名付きのコンテンツをプルして、イメージのソースを確認できます。 詳しくは、[Signing images for trusted content](/docs/services/Registry/registry_trusted_content.html#registry_trustedcontent) を参照してください。

3.  **Container Image Security Enforcement (ベータ)**: コンテナー・イメージを検証してからデプロイできるように、アドミッション・コントローラーとカスタム・ポリシーを作成します。 Container Image Security Enforcement を使用すると、イメージのデプロイ元を制御できるうえに、イメージが [Vulnerability Advisor](/docs/services/va/va_index.html) ポリシーまたは[コンテンツの信頼](/docs/services/Registry/registry_trusted_content.html#registry_trustedcontent)要件を満たしていることを確認できます。 設定したポリシーをデプロイメントが満たしていなければ、Security Enforcement によって、クラスターの変更が防止されます。 詳しくは、[Enforcing container image security (beta)](/docs/services/Registry/registry_security_enforce.html#security_enforce) を参照してください。

4.  **コンテナー脆弱性スキャナー**: デフォルトでは Vulnerability Advisor が、{{site.data.keyword.registryshort_notm}} に保管されているイメージをスキャンします。 クラスター内で実行されているライブ・コンテナーの状況を確認するには、コンテナー・スキャナーをインストールします。 詳しくは、[コンテナー・スキャナーのインストール](/docs/services/va/va_index.html#va_install_livescan)を参照してください。

5.  **Security Advisor (プレビュー) によるネットワーク分析**: {{site.data.keyword.Bluemix_notm}} Security Advisor を使用すると、Vulnerability Advisor や {{site.data.keyword.cloudcerts_short}} などの {{site.data.keyword.Bluemix_notm}} サービスから得られるセキュリティー分析データを集約できます。 クラスターで Security Advisor を有効にすると、不審な着信および発信ネットワーク・トラフィックに関するレポートを参照できます。 詳しくは、[Network Analytics](/docs/services/security-advisor/network-analytics.html#network-analytics) を参照してください。 インストールするには、[Setting up monitoring of suspicious clients and server IP addresses for a Kubernetes cluster](/docs/services/security-advisor/setup_cluster.html) を参照してください。

6.  **{{site.data.keyword.cloudcerts_long_notm}} (ベータ)**: クラスターが米国南部にあり、[TLS ありでカスタム・ドメインを使用してアプリを公開する](https://console.bluemix.net/docs/containers/cs_ingress.html#ingress_expose_public)場合は、TLS 証明書を {{site.data.keyword.cloudcerts_short}} に保管できます。 既に期限切れか、まもなく期限切れになる証明書も Security Advisor ダッシュボードに報告されます。 詳しくは、[{{site.data.keyword.cloudcerts_short}} 概説](/docs/services/certificate-manager/index.html#gettingstarted)を参照してください。

<br />


## 個人情報の保管
{: #pi}

Kubernetes リソースおよびコンテナー・イメージ内の個人情報のセキュリティーを確保する責任を持ちます。個人情報には、自分の名前、住所、電話番号、E メール・アドレス、および自分自身や顧客、その他の人を特定したり、連絡を取ったり、居場所を見つけたりすることができるその他の情報が含まれます。
{: shortdesc}

<dl>
  <dt>Kubernetes シークレットを使用した個人情報の保管</dt>
  <dd>個人情報を保持するように設計された Kubernetes リソースにのみ個人情報を保管します。例えば、Kubernetes 名前空間、デプロイメント、サービス、または構成マップの名前に自分の名前を使用しないでください。適切な保護と暗号化を行うには、代わりに <a href="cs_app.html#secrets">Kubernetes シークレット</a>に個人情報を保管してください。</dd>

  <dt>Kubernetes `imagePullSecret` を使用したイメージ・レジストリー資格情報の保管</dt>
  <dd>個人情報をコンテナー・イメージまたはレジストリー名前空間に保管しないでください。適切な保護と暗号化を行うには、代わりに、レジストリー資格情報は <a href="cs_images.html#other">Kubernetes imagePullSecrets</a> に保管し、その他の個人情報は <a href="cs_app.html#secrets">Kubernetes シークレット</a>に保管してください。個人情報がイメージの前のレイヤーに保管されている場合、イメージを削除するだけではこの個人情報を削除できない場合があることに注意してください。</dd>
  </dl>

<br />






