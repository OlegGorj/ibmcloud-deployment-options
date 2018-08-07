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




# よく閲覧された {{site.data.keyword.containershort_notm}} のトピック
{: #cs_popular_topics}

{{site.data.keyword.containerlong}} の最新情報を確認しましょう。知っておくべき新機能、試してみたい便利な方法、他の開発者が役に立つと感じている、よく閲覧されたトピックをご覧ください。
{:shortdesc}

## 2018 年 5 月によく閲覧されたトピック
{: #may18}


<table summary="この表には、よく閲覧されたトピックを示しています。行は左から右に読みます。1 列目は日付、2 列目は機能のタイトル、3 列目は説明です。">
<caption>2018 年 5 月によく閲覧されたコンテナーと Kubernetes クラスターについてのトピック</caption>
<thead>
<th>日</th>
<th>タイトル</th>
<th>説明</th>
</thead>
<tbody>
<tr>
<td>5 月 24 日</td>
<td>[新しい Ingress サブドメイン形式](cs_ingress.html)</td>
<td>5 月 24 日より後に作成されたクラスターには、新しい形式の <code>&lt;cluster_name&gt;.&lt;region&gt;.containers.appdomain.cloud</code> で Ingress サブドメインが割り当てられます。Ingress を使用してアプリを公開する場合、新しいサブドメインを使用してインターネットからアプリにアクセスできます。</td>
</tr>
<tr>
<td>5 月 14 日</td>
<td>[更新: 世界中の GPU ベアメタルへのワークロードのデプロイ](cs_app.html#gpu_app)</td>
<td>クラスター内に[ベアメタルのグラフィックス・プロセッシング・ユニット (GPU) マシン・タイプ](cs_clusters.html#shared_dedicated_node)がある場合、大量の数学的処理が行われるアプリをスケジュールできます。GPU ワーカー・ノードは、CPU と GPU の両方にわたってアプリのワークロードを処理することで、パフォーマンスを向上させることができます。</td>
</tr>
<tr>
<td>5 月 3 日</td>
<td>[Container Image Security Enforcement (ベータ)](/docs/services/Registry/registry_security_enforce.html#security_enforce)</td>
<td>チームでは、アプリ・コンテナーでプルするイメージを決める際の支援が必要ではありませんか。コンテナー・イメージを検証してからデプロイできる、Container Image Security Enforcement ベータをお試しください。Kubernetes 1.9 以降を実行するクラスターで使用可能です。</td>
</tr>
<tr>
<td>5 月 1 日</td>
<td>[GUI からの Kubernetes ダッシュボードのデプロイ](cs_app.html#cli_dashboard)</td>
<td>ワンクリックで Kubernetes ダッシュボードにアクセスしたいと思ったことはありませんか。{{site.data.keyword.Bluemix_notm}} GUI の**「Kubernetes ダッシュボード (Kubernetes Dashboard)」**ボタンをお試しください。</td>
</tr>
</tbody></table>




## 2018 年 4 月によく閲覧されたトピック
{: #apr18}

<table summary="この表には、よく閲覧されたトピックを示しています。行は左から右に読みます。1 列目は日付、2 列目は機能のタイトル、3 列目は説明です。">
<caption>2018 年 4 月によく閲覧されたコンテナーと Kubernetes クラスターについてのトピック</caption>
<thead>
<th>日</th>
<th>タイトル</th>
<th>説明</th>
</thead>
<tbody>
<tr>
<td>4 月 17 日</td>
<td>{{site.data.keyword.Bluemix_notm}} Block Storage</td>
<td>永続データをブロック・ストレージに保存するための {{site.data.keyword.Bluemix_notm}} Block Storage [プラグイン](cs_storage.html#install_block)をインストールします。 そして、クラスターのために[ブロック・ストレージを新規作成](cs_storage.html#create)するか、または[既存のブロック・ストレージを使用](cs_storage.html#existing_block)することができます。</td>
</tr>
<tr>
<td>4 月 13 日</td>
<td>[Cloud Foundry アプリをクラスターにマイグレーションするための新しいチュートリアル](cs_tutorials_cf.html#cf_tutorial)</td>
<td>Cloud Foundry アプリをお持ちですか? そのアプリの同じコードを、Kubernetes クラスターで実行されるコンテナーにデプロイする方法を示します。</td>
</tr>
<tr>
<td>4 月 5 日</td>
<td>[ログのフィルタリング](cs_health.html#filter-logs)</td>
<td>特定のログを転送対象から除外することができます。 特定の名前空間、コンテナー名、ログ・レベル、メッセージ・ストリングを基準にしてログを除外できます。</td>
</tr>
</tbody></table>

## 2018 年 3 月によく閲覧されたトピック
{: #mar18}

<table summary="この表には、よく閲覧されたトピックを示しています。行は左から右に読みます。1 列目は日付、2 列目は機能のタイトル、3 列目は説明です。">
<caption>2018 年 3 月によく閲覧されたコンテナーと Kubernetes クラスターについてのトピック</caption>
<thead>
<th>日</th>
<th>タイトル</th>
<th>説明</th>
</thead>
<tbody>
<tr>
<td>3 月 16 日</td>
<td>[トラステッド・コンピューティングを使用するベア・メタル・クラスターのプロビジョン](cs_clusters.html#shared_dedicated_node)</td>
<td>[Kubernetes バージョン 1.9](cs_versions.html#cs_v19) 以降を実行するベア・メタル・クラスターを作成し、トラステッド・コンピューティングを有効にしてワーカー・ノードが改ざんされていないことを検証できます。</td>
</tr>
<tr>
<td>3 月 14 日</td>
<td>[{{site.data.keyword.appid_full}} による安全なサインイン](cs_integrations.html#appid)</td>
<td>ユーザーに対してサインインを要求することにより、{{site.data.keyword.containershort_notm}} で実行されるアプリのセキュリティーを強化します。</td>
</tr>
<tr>
<td>3 月 13 日</td>
<td>[サンパウロで使用可能なロケーション](cs_regions.html)</td>
<td>米国南部地域の新しいロケーションとしてブラジルのサンパウロが追加されました。 ファイアウォールを使用している場合は、このロケーションと、ご使用のクラスターがある地域内の他のロケーションに対し、必ず[ファイアウォール・ポートを開いてください](cs_firewall.html#firewall)。</td>
</tr>
<tr>
<td>3 月 12 日</td>
<td>トライアル・アカウントで [{{site.data.keyword.Bluemix_notm}} に参加したばかりですか? 無料の Kubernetes クラスターをお試しください。](container_index.html#clusters)</td>
<td>トライアルの [{{site.data.keyword.Bluemix_notm}} アカウント](https://console.bluemix.net/registration/)で、無料クラスターを 1 つデプロイして、21 日間 Kubernetes の機能をテストできます。</td>
</tr>
</tbody></table>

## 2018 年 2 月によく閲覧されたトピック
{: #feb18}

<table summary="この表には、よく閲覧されたトピックを示しています。行は左から右に読みます。1 列目は日付、2 列目は機能のタイトル、3 列目は説明です。">
<caption>2018 年 2 月によく閲覧されたコンテナーと Kubernetes クラスターについてのトピック</caption>
<thead>
<th>日</th>
<th>タイトル</th>
<th>説明</th>
</thead>
<tbody>
<tr>
<td>2 月 27 日</td>
<td>ワーカー・ノード用のハードウェア仮想マシン (HVM) のイメージ</td>
<td>HVM イメージを使用して、ワークロードの入出力パフォーマンスを向上させます。 `bx cs worker-reload` [コマンド](cs_cli_reference.html#cs_worker_reload)または `bx cs worker-update` [コマンド](cs_cli_reference.html#cs_worker_update)を使用して、既存の各ワーカー・ノード上でアクティブ化します。</td>
</tr>
<tr>
<td>2 月 26 日</td>
<td>[KubeDNS 自動スケーリング](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/)</td>
<td>KubeDNS は、クラスターの拡大に合わせてスケーリングされるようになりました。 スケーリングの比率は、コマンド `kubectl -n kube-system edit cm kube-dns-autoscaler` を使用して調整できます。</td>
</tr>
<tr>
<td>2 月 23 日</td>
<td>Web UI での[ロギング](cs_health.html#view_logs)および[メトリック](cs_health.html#view_metrics)の表示</td>
<td>改善された Web UI を使用して、クラスターとそのコンポーネントのログとメトリック・データを簡単に表示できます。 アクセス方法については、クラスターの詳細ページを参照してください。</td>
</tr>
<tr>
<td>2 月 20 日</td>
<td>暗号化されたイメージと[署名された信頼できるコンテンツ](../services/Registry/registry_trusted_content.html#registry_trustedcontent)</td>
<td>{{site.data.keyword.registryshort_notm}} では、イメージをレジストリー名前空間に保管する際に、イメージに署名して暗号化することで、イメージの保全性を確保できます。コンテナー・インスタンスは、信頼できるコンテンツのみを使用して実行してください。</td>
</tr>
<tr>
<td>2 月 19 日</td>
<td>[strongSwan IPSec VPN のセットアップ](cs_vpn.html#vpn-setup)</td>
<td>strongSwan IPSec VPN Helm チャートを素早くデプロイして、Virtual Router Appliance なしで {{site.data.keyword.containershort_notm}} クラスターをオンプレミス・データ・センターに安全に接続します。</td>
</tr>
<tr>
<td>2 月 14 日</td>
<td>[ソウルで使用可能なロケーション](cs_regions.html)</td>
<td>オリンピックの時期に合わせて、Kubernetes クラスターを北アジア太平洋地域のソウルにデプロイします。 ファイアウォールを使用している場合は、このロケーションと、ご使用のクラスターがある地域内の他のロケーションに対し、必ず[ファイアウォール・ポートを開いてください](cs_firewall.html#firewall)。</td>
</tr>
<tr>
<td>2 月 8 日</td>
<td>[Kubernetes 1.9 の更新](cs_versions.html#cs_v19)</td>
<td>Kubernetes 1.9 に更新する前に、クラスターに対して加える変更を確認してください。</td>
</tr>
</tbody></table>

## 2018 年 1 月によく閲覧されたトピック
{: #jan18}

<table summary="この表には、よく閲覧されたトピックを示しています。行は左から右に読みます。1 列目は日付、2 列目は機能のタイトル、3 列目は説明です。">
<caption>2018 年 1 月によく閲覧されたコンテナーと Kubernetes クラスターについてのトピック</caption>
<thead>
<th>日</th>
<th>タイトル</th>
<th>説明</th>
</thead>
<tbody>
<td>1 月 25 日</td>
<td>[グローバル・レジストリーの提供](../services/Registry/registry_overview.html#registry_regions)</td>
<td>{{site.data.keyword.registryshort_notm}} では、グローバル `registry.bluemix.net` を使用して、IBM 提供のパブリック・イメージをプルできます。</td>
</tr>
<tr>
<td>1 月 23 日</td>
<td>[シンガポールおよびカナダのモントリオールで使用可能なロケーション](cs_regions.html)</td>
<td>シンガポールおよびモントリオールは、{{site.data.keyword.containershort_notm}} の北アジア太平洋地域と米国東部地域で使用可能なロケーションです。 ファイアウォールを使用している場合は、これらのロケーションと、ご使用のクラスターがある地域内の他のロケーションに対し、必ず[ファイアウォール・ポートを開いてください](cs_firewall.html#firewall)。</td>
</tr>
<tr>
<td>1 月 8 日</td>
<td>[拡張されたフレーバーが使用可能](cs_cli_reference.html#cs_machine_types)</td>
<td>Series 2 仮想マシン・タイプには、ローカル SSD ストレージとディスク暗号化が含まれます。 パフォーマンスと安定性を向上させるために、これらのフレーバーに[ワークロードを移動](cs_cluster_update.html#machine_type)してください。</td>
</tr>
</tbody></table>

## 同じ関心を持つ開発者との Slack でのチャット
{: #slack}

[{{site.data.keyword.containershort_notm}} Slack ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://ibm-container-service.slack.com) で、他の開発者の間で話題となっている内容を確認したり、質問をしたりすることができます。
{:shortdesc}

{{site.data.keyword.Bluemix_notm}} アカウントに IBM ID を使用していない場合は、この Slack への[招待を要求](https://bxcs-slack-invite.mybluemix.net/)してください。
{: tip}
