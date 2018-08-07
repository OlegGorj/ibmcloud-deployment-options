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



# バージョン情報および更新操作
{: #cs_versions}

## Kubernetes バージョンのタイプ
{: #version_types}

{{site.data.keyword.containerlong}} は、複数のバージョンの Kubernetes を同時にサポートします。 最新バージョン (n) がリリースされると、2 つ前のバージョン (n-2) までサポートされます。 最新バージョンから 2 つより前のバージョン (n-3) は、まず非推奨になり、その後サポートされなくなります。
{:shortdesc}

現在サポートされている Kubernetes のバージョンは以下のとおりです。

- 最新: 1.10.1
- デフォルト: 1.9.7
- サポート対象: 1.8.11

**非推奨バージョン**: 非推奨 Kubernetes でクラスターを実行している場合は、そのバージョンがサポートされなくなるまでの 30 日の間に、サポートされるバージョンの Kubernetes を確認したうえで更新してください。 非推奨期間中は、制限されたコマンドをクラスターで実行して、ワーカーの追加、ワーカーの再ロード、クラスターの更新を行えます。 非推奨バージョンでは新規クラスターを作成できません。

**非サポート・バージョン**: サポートされないバージョンの Kubernetes でクラスターを実行している場合は、更新が[与える可能性のある影響を確認](#version_types)したうえで、ただちに[クラスターを更新](cs_cluster_update.html#update)して、重要なセキュリティー更新とサポートを継続して受けられるようにしてください。

クラスターのサーバー・バージョンを確認するには、以下のコマンドを実行します。

```
kubectl version  --short | grep -i server
```
{: pre}

出力例:

```
Server Version: v1.9.7+9d6e0610086578
```
{: screen}


## 更新タイプ
{: #update_types}

Kubernetes クラスターには、メジャー、マイナー、およびパッチという 3 つのタイプの更新があります。
{:shortdesc}

|更新タイプ|バージョン・ラベルの例|更新の実行者|影響
|-----|-----|-----|-----|
|メジャー|1.x.x|お客様|スクリプトやデプロイメントを含むクラスターの操作変更。|
|マイナー|x.9.x|お客様|スクリプトやデプロイメントを含むクラスターの操作変更。|
|パッチ|x.x.4_1510|IBM とお客様|Kubernetes のパッチ、および、セキュリティー・パッチやオペレーティング・システム・パッチなどの他の {{site.data.keyword.Bluemix_notm}} Provider コンポーネントの更新。 マスターは IBM が自動的に更新しますが、ワーカー・ノードへのパッチはお客様が適用する必要があります。|
{: caption="Kubernetes 更新の影響" caption-side="top"}

更新が利用可能になると、`bx cs workers <cluster>` や `bx cs worker-get <cluster> <worker>` コマンドなどを使用してワーカー・ノードの情報を表示したときに通知されます。
-  **メジャー更新とマイナー更新**: まず[マスター・ノードを更新して](cs_cluster_update.html#master)、それから[ワーカー・ノードを更新します](cs_cluster_update.html#worker_node)。 
   - デフォルトでは、Kubernetes マスターを更新できるのは 2 つ先のマイナー・バージョンまでです。例えば、現在のマスターがバージョン 1.5 であり 1.8 に更新する計画の場合、まず 1.7 に更新する必要があります。 続けて更新を強制実行することは可能ですが、2 つのマイナー・バージョンを超える更新は予期しない結果を生じることがあります。
   - 少なくともクラスターの `major.minor` バージョンに一致する `kubectl` CLI バージョンを使用しないと、予期しない結果になる可能性があります。 Kubernetes クラスターと [CLI のバージョン](cs_cli_install.html#kubectl)を最新の状態に保つようにしてください。
-  **パッチ更新**: 更新が使用可能かどうかを毎月確認し、`bx cs worker-update` [コマンド](cs_cli_reference.html#cs_worker_update)または `bx cs worker-reload` [コマンド](cs_cli_reference.html#cs_worker_reload)を使用して、これらのセキュリティー・パッチおよびオペレーティング・システム・パッチを適用してください。詳しくは、[バージョンの変更ログ](cs_versions_changelog.html)を参照してください。

<br/>

この情報は、クラスターを前のバージョンから新しいバージョンに更新した場合に、デプロイされているアプリに影響を与える可能性がある更新についてまとめたものです。
-  バージョン 1.10 [マイグレーションの操作](#cs_v110)。
-  バージョン 1.9 [マイグレーションの操作](#cs_v19)。
-  バージョン 1.8 [マイグレーションの操作](#cs_v18)。
-  非推奨または非サポートのバージョンの[アーカイブ](#k8s_version_archive)。

<br/>

変更内容の完全なリストは、以下の情報を参照してください。
* [Kubernetes の変更ログ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md)。
* [IBM バージョンの変更ログ](cs_versions_changelog.html)。

## バージョン 1.10
{: #cs_v110}

<p><img src="images/certified_kubernetes_1x10.png" style="padding-right: 10px;" align="left" alt="このバッジは、IBM Cloud Container Service が Kubernetes バージョン 1.10 の認定を受けたことを示しています。"/> {{site.data.keyword.containerlong_notm}} は、CNCF Kubernetes Software Conformance Certification プログラムのもとで認定を受けたバージョン 1.10 の Kubernetes 製品です。 __</p>

Kubernetes を前のバージョンから 1.10 に更新する場合に必要な可能性がある変更作業について説明します。

**重要**: Kubernetes 1.10 に正常に更新するには、その前に、[Calico v3 への更新の準備](#110_calicov3)に記載の手順を実行しておく必要があります。

<br/>

### マスターの前に行う更新
{: #110_before}

<table summary="バージョン 1.10 の Kubernetes の更新">
<caption>マスターを Kubernetes 1.10 に更新する前に行う変更</caption>
<thead>
<tr>
<th>タイプ</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico v3</td>
<td>Kubernetes バージョン 1.10 に更新すると、Calico も v2.6.5 から v3.1.1 に更新されます。<strong>重要</strong>: Kubernetes v1.10 に正常に更新するには、その前に、[Calico v3 への更新の準備](#110_calicov3)に記載の手順を実行しておく必要があります。</td>
</tr>
<tr>
<td>Kubernetes ダッシュボードのネットワーク・ポリシー</td>
<td>Kubernetes 1.10 では、<code>kube-system</code> 名前空間の <code>kubernetes-dashboard</code> ネットワーク・ポリシーによって、すべてのポッドが Kubernetes ダッシュボードへのアクセスをブロックされます。ただしこれは、{{site.data.keyword.Bluemix_notm}} コンソールからの、または <code>kubectl proxy</code> を使用したダッシュボードへのアクセスには影響<strong>しません</strong>。ポッドがダッシュボードにアクセスする必要がある場合は、名前空間に <code>kubernetes-dashboard-policy: allow</code> ラベルを追加し、その名前空間にポッドをデプロイします。</td>
</tr>
<tr>
<td>Kubelet API アクセス</td>
<td>Kubelet API 許可は、<code>Kubernetes API サーバー</code>に委任されました。Kubelet API へのアクセスは、<strong>ノード</strong>・サブリソースへのアクセス許可を付与する <code>ClusterRoles</code> に基づきます。Kubernetes Heapster にはデフォルトで、<code>ClusterRole</code> および <code>ClusterRoleBinding</code> が付与されています。ただし、他のユーザーやアプリが Kubelet API を使用する場合は、それらのユーザーやアプリに API を使用する許可を付与する必要があります。[Kubelet 許可 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/admin/kubelet-authentication-authorization/#kubelet-authorization) に関する Kubernetes 資料を参照してください。</td>
</tr>
<tr>
<td>暗号スイート</td>
<td><code>Kubernetes API サーバー</code>および Kubelet API に対してサポートされる暗号スイートが、高強度の暗号化 (128 ビット以上) を備えたサブセットに限定されるようになりました。既存の自動化またはリソースで、これより弱い暗号を使用し、<code>Kubernetes API サーバー</code>または Kubelet API との通信を利用している場合は、マスターを更新する前に、より強力な暗号サポートを有効にしてください。</td>
</tr>
</tbody>
</table>

### マスターの後に行う更新
{: #110_after}

<table summary="バージョン 1.10 の Kubernetes の更新">
<caption>マスターを Kubernetes 1.10 に更新した後に行う変更</caption>
<thead>
<tr>
<th>タイプ</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico v3</td>
<td>クラスターが更新されると、そのクラスターに適用される既存のすべての Calico データが Calico v3 構文を使用するように自動的にマイグレーションされます。Calico v3 構文の Calico リソースを表示、追加、または変更するには、[Calico CLI 構成をバージョン 3.1.1](#110_calicov3) に更新してください。</td>
</tr>
<tr>
<td>ノードの <code>ExternalIP</code> アドレス</td>
<td>ノードの <code>ExternalIP</code> フィールドが、ノードのパブリック IP アドレス値に設定されるようになりました。この値に依存するすべてのリソースを確認して更新してください。</td>
</tr>
<tr>
<td><code>kubectl port-forward</code></td>
<td><code>kubectl port-forward</code> コマンドを使用する際に、<code>-p</code> フラグがサポートされなくなりました。以前の動作に依存するスクリプトがある場合は、<code>-p</code> フラグをポッド名で置き換えるようにスクリプトを更新してください。</td>
</tr>
<tr>
<td>読み取り専用 API データ・ボリューム</td>
<td>`secret`、`configMap`、`downwardAPI`、および投影ボリュームは、読み取り専用でマウントされるようになります。
これまでは、システムによって自動的に元の状態に戻されることがあるこれらのボリュームに、アプリがデータを書き込めました。 このマイグレーション操作は、セキュリティーの脆弱性 [CVE-2017-1002102![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2017-1002102) を修正するために必要です。アプリが以前の非セキュアな動作に依存している場合は、適切に変更してください。</td>
</tr>
</tbody>
</table>

### Calico v3 への更新の準備
{: #110_calicov3}

始める前に、クラスター・マスターとすべてのワーカー・ノードで Kubernetes バージョン 1.8 以降が実行されている必要があり、少なくとも 1 つのワーカー・ノードが必要です。

**重要**: マスターを更新する前に、Calico v3 の更新の準備をします。Kubernetes v1.10 へのマスター・アップグレード中は、新規ポッドおよび新規 Kubernetes または Calico ネットワーク・ポリシーはスケジュールされません。更新によって新しいスケジュールが妨げられる時間はさまざまです。小規模なクラスターでは数分かかり、ノードが 10 個増えるごとにさらに数分追加されます。既存のネットワーク・ポリシーとポッドは引き続き実行されます。

1.  Calico ポッドが正常であることを確認します。
    ```
    kubectl get pods -n kube-system -l k8s-app=calico-node -o wide
    ```
    {: pre}
    
2.  **Running** 状態になっていないポッドがある場合は、そのポッドを削除し、**Running** 状態になるまで待ってから続行します。

3.  Calico ポリシーまたはその他の Calico リソースを自動生成する場合は、これらのリソースを生成するための自動化ツールを [Calico v3 構文 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/) で更新します。

4.  VPN 接続に [strongSwan](cs_vpn.html#vpn-setup) を使用している場合、strongSwan 2.0.0 Helm チャートは Calico v3 または Kubernetes 1.10 では機能しません。Calico 2.6、および Kubernetes 1.7、1.8、および 1.9 との後方互換性のある 2.1.0 Helm チャートに [strongSwan を更新](cs_vpn.html#vpn_upgrade)してください。

5.  [クラスター・マスターを Kubernetes v1.10 に更新します](cs_cluster_update.html#master)。

<br />


## バージョン 1.9
{: #cs_v19}

<p><img src="images/certified_kubernetes_1x9.png" style="padding-right: 10px;" align="left" alt="このバッジは、IBM Cloud Container Service が Kubernetes バージョン 1.9 の認定を受けたことを示しています。"/> {{site.data.keyword.containerlong_notm}} は、CNCF Kubernetes Software Conformance Certification プログラムのもとで認定を受けたバージョン 1.9 の Kubernetes 製品です。__</p>

Kubernetes を前のバージョンから 1.9 に更新する場合に必要な可能性がある変更作業について説明します。

<br/>

### マスターの前に行う更新
{: #19_before}

<table summary="バージョン 1.9 の Kubernetes の更新">
<caption>マスターを Kubernetes 1.9 に更新する前に行う変更</caption>
<thead>
<tr>
<th>タイプ</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Web フックのアドミッション API</td>
<td>API サーバーがアドミッション制御 Web フックを呼び出すときに使用されるアドミッション API が、<code>admission.v1alpha1</code> から <code>admission.v1beta1</code> に変更されました。 <em>クラスターをアップグレードする前に既存の Web フックを削除し</em>、最新の API を使用するように Web フック構成ファイルを更新する必要があります。 この変更には後方互換性がありません。</td>
</tr>
</tbody>
</table>

### マスターの後に行う更新
{: #19_after}

<table summary="バージョン 1.9 の Kubernetes の更新">
<caption>マスターを Kubernetes 1.9 に更新した後に行う変更</caption>
<thead>
<tr>
<th>タイプ</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>`kubectl` 出力</td>
<td>`-o custom-columns` を指定した `kubectl` コマンドの使用時に、列がオブジェクト内に見つからなかった場合に、`<none>` という出力が表示されるようになりました。<br>
以前は、このような操作は失敗し、エラー・メッセージ `xxx is not found` が表示されていました。 以前の動作に依存したスクリプトがある場合は更新してください。</td>
</tr>
<tr>
<td>`kubectl patch`</td>
<td>パッチを適用したリソースに変更が行われない場合に、`kubectl patch` コマンドが`終了コード 1` で失敗するようになりました。以前の動作に依存したスクリプトがある場合は更新してください。</td>
</tr>
<tr>
<td>Kubernetes ダッシュボードの権限</td>
<td>ユーザーは、クラスター・リソースを参照するために、資格情報を使用して Kubernetes ダッシュボードにログインしなければなりません。 デフォルトの Kubernetes ダッシュボード `ClusterRoleBinding` RBAC 許可は削除されました。 手順については、[Kubernetes ダッシュボードの起動](cs_app.html#cli_dashboard)を参照してください。</td>
</tr>
<tr>
<td>読み取り専用 API データ・ボリューム</td>
<td>`secret`、`configMap`、`downwardAPI`、および投影ボリュームは、読み取り専用でマウントされるようになります。
これまでは、システムによって自動的に元の状態に戻されることがあるこれらのボリュームに、アプリがデータを書き込めました。 このマイグレーション操作は、
セキュリティーの脆弱性 [CVE-2017-1002102](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2017-1002102) を修正するために必要です。
アプリが以前の非セキュアな動作に依存している場合は、適切に変更してください。</td>
</tr>
<tr>
<td>テイントと耐障害性</td>
<td>`node.alpha.kubernetes.io/notReady` テイントと `node.alpha.kubernetes.io/unreachable` テイントは、それぞれ `node.kubernetes.io/not-ready` と `node.kubernetes.io/unreachable` に変更されました。<br>
テイントは自動的に更新されますが、これらのテイントの耐障害性は手動で更新する必要があります。 `ibm-system` と `kube-system` 以外の名前空間ごとに、耐障害性を変更する必要があるかどうかを判別します。<br>
<ul><li><code>kubectl get pods -n &lt;namespace&gt; -o yaml | grep "node.alpha.kubernetes.io/notReady" && echo "Action required"</code></li><li>
<code>kubectl get pods -n &lt;namespace&gt; -o yaml | grep "node.alpha.kubernetes.io/unreachable" && echo "Action required"</code></li></ul><br>
`Action required` が戻された場合は、適宜、ポッドの耐障害性を変更してください。</td>
</tr>
<tr>
<td>Web フックのアドミッション API</td>
<td>クラスターを更新する前に既存の Web フックを削除した場合は、新しい Web フックを作成してください。</td>
</tr>
</tbody>
</table>

<br />



## バージョン 1.8
{: #cs_v18}

<p><img src="images/certified_kubernetes_1x8.png" style="padding-right: 10px;" align="left" alt="このバッジは、IBM Cloud Container Service が Kubernetes バージョン 1.8 の認定を受けたことを示しています。"/> {{site.data.keyword.containerlong_notm}} は、CNCF Kubernetes Software Conformance Certification プログラムのもとで認定を受けたバージョン 1.8 の Kubernetes 製品です。__</p>

Kubernetes を前のバージョンから 1.8 に更新する場合に必要な可能性がある変更作業について説明します。

<br/>

### マスターの前に行う更新
{: #18_before}

<table summary="バージョン 1.8 の Kubernetes の更新">
<caption>マスターを Kubernetes 1.8 に更新する前に行う変更</caption>
<thead>
<tr>
<th>タイプ</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>なし</td>
<td>マスターを更新する前に必要な変更はありません</td>
</tr>
</tbody>
</table>

### マスターの後に行う更新
{: #18_after}

<table summary="バージョン 1.8 の Kubernetes の更新">
<caption>マスターを Kubernetes 1.8 に更新した後に行う変更</caption>
<thead>
<tr>
<th>タイプ</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes ダッシュボード・ログイン</td>
<td>バージョン 1.8 で Kubernetes ダッシュボードにアクセスするための URL は変更され、ログイン・プロセスには新しい認証ステップが含まれています。 詳しくは、[Kubernetes ダッシュボードへのアクセス](cs_app.html#cli_dashboard)を参照してください。</td>
</tr>
<tr>
<td>Kubernetes ダッシュボードの権限</td>
<td>バージョン 1.8 のクラスター・リソースを表示するユーザーが資格情報を使用してログインするように強制するには、1.7 ClusterRoleBinding RBAC 許可を除去します。 `kubectl delete clusterrolebinding -n kube-system kubernetes-dashboard` を実行します。</td>
</tr>
<tr>
<td>`kubectl delete`</td>
<td>`kubectl delete` コマンドは、ワークロード API オブジェクトを削除する前に、ポッドの場合のようにオブジェクトをスケールダウンすることはなくなりました。 オブジェクトのスケールダウンが必要な場合は、オブジェクトを削除する前に [`kubectl scale ` ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/reference/kubectl/overview/#scale) を使用してください。</td>
</tr>
<tr>
<td>`kubectl run`</td>
<td>`kubectl run` コマンドは、`--env` のために、コンマ区切りの引数ではなく複数のフラグを使用する必要があります。 例えば、<code>kubectl run --env &lt;x&gt;=&lt;y&gt;,&lt;z&gt;=&lt;a&gt;</code> ではなく <code>kubectl run --env &lt;x&gt;=&lt;y&gt; --env &lt;z&gt;=&lt;a&gt;</code> を実行します。 </td>
</tr>
<tr>
<td>`kubectl stop`</td>
<td>`kubectl stop` コマンドは使用できなくなりました。</td>
</tr>
<tr>
<td>読み取り専用 API データ・ボリューム</td>
<td>`secret`、`configMap`、`downwardAPI`、および投影ボリュームは、読み取り専用でマウントされるようになります。
これまでは、システムによって自動的に元の状態に戻されることがあるこれらのボリュームに、アプリがデータを書き込めました。 このマイグレーション操作は、
セキュリティーの脆弱性 [CVE-2017-1002102](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2017-1002102) を修正するために必要です。
アプリが以前の非セキュアな動作に依存している場合は、適切に変更してください。</td>
</tr>
</tbody>
</table>

<br />



## アーカイブ
{: #k8s_version_archive}

### バージョン 1.7 (非推奨)
{: #cs_v17}

**2018 年 5 月 22 日をもって、Kubernetes バージョン 1.7 を実行する {{site.data.keyword.containershort_notm}} クラスターは非推奨になりました**。2018 年 6 月 21 日を過ぎると、バージョン 1.7 クラスターは次に最新のバージョン ([Kubernetes 1.8](#cs_v18)) に更新しない限り、セキュリティー更新もサポートも受けられなくなります。

各 Kubernetes バージョンの更新が[与える可能性のある影響を確認](cs_versions.html#cs_versions)したうえで、ただちに[クラスターを更新](cs_cluster_update.html#update)してください。

まだ Kubernetes バージョン 1.5 を実行していますか。以下の情報を参照して、クラスターを v1.5 から v1.7 に更新した場合の影響を評価してください。[クラスターを更新](cs_cluster_update.html#update)して v1.7 にしたら、すぐに v1.8 以上に更新してください。
{: tip}

<p><img src="images/certified_kubernetes_1x7.png" style="padding-right: 10px;" align="left" alt="このバッジは、IBM Cloud Container Service に関する Kubernetes バージョン 1.7 証明書を示しています。"/> {{site.data.keyword.containerlong_notm}} は、CNCF Kubernetes Software Conformance Certification プログラムにおけるバージョン 1.7 の認定 Kubernetes 製品です。</p>

Kubernetes を前のバージョンから 1.7 に更新する場合に必要な可能性がある変更作業について説明します。

<br/>

#### マスターの前に行う更新
{: #17_before}

<table summary="バージョン 1.7 と 1.6 の Kubernetes の更新">
<caption>マスターを Kubernetes 1.7 に更新する前に行う変更</caption>
<thead>
<tr>
<th>タイプ</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>ストレージ</td>
<td>構成スクリプトの中で、`hostPath` や `mountPath` に親ディレクトリー参照 (`../to/dir` など) を指定することはできません。 パスを `/path/to/dir` などの単純な絶対パスに変更してください。
<ol>
  <li>ストレージ・パスを変更する必要があるかどうかを判別します。</br>
  ```
  kubectl get pods --all-namespaces -o yaml | grep "\.\." && echo "Action required"
  ```
  </br>

  <li>`「Action required」`が返された場合、すべてのワーカー・ノードを更新する前に、ポッドを変更して絶対パスを参照するようにします。 ポッドがデプロイメントなど別のリソースによって所有されている場合は、そのリソース内の [_PodSpec_ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/api-reference/v1.7/#podspec-v1-core) を変更します。
</ol>
</td>
</tr>
</tbody>
</table>

#### マスターの後に行う更新
{: #17_after}

<table summary="バージョン 1.7 と 1.6 の Kubernetes の更新">
<caption>マスターを Kubernetes 1.7 に更新した後に行う変更</caption>
<thead>
<tr>
<th>タイプ</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>デプロイメント `apiVersion`</td>
<td>Kubernetes 1.5 からクラスターを更新した後は、新しい `Deployment` YAML ファイル内の `apiVersion` フィールドには `apps/v1beta1` を使用してください。 `Ingress` などの他のリソースには、引き続き `extensions/v1beta1` を使用してください。</td>
</tr>
<tr>
<td>'kubectl'</td>
<td>`kubectl` CLI の更新の後、以下の `kubectl create` コマンドは、コンマ区切りの引数ではなく複数のフラグを使用する必要があります。<ul>
 <li>`role`
 <li>`clusterrole`
 <li>`rolebinding`
 <li>`clusterrolebinding`
 <li>`secret`
 </ul>
</br>  例えば、`kubectl create role --resource-name <x>,<y>` ではなく `kubectl create role --resource-name <x> --resource-name <y>` のように実行します。</td>
</tr>
<tr>
<td>ネットワーク・ポリシー</td>
<td>`net.beta.kubernetes.io/network-policy` アノテーションは使用できなくなりました。
<ol>
  <li>ポリシーを変更する必要があるかどうかを判別します。</br>
  ```
  kubectl get ns -o yaml | grep "net.beta.kubernetes.io/network-policy" | grep "DefaultDeny" && echo "Action required"
  ```
  <li>`「Action required」`が返された場合、リストされたそれぞれの Kubernetes 名前空間に以下のネットワーク・ポリシーを追加します。</br>

  <pre class="codeblock">
  <code>
  kubectl create -n &lt;namespace&gt; -f - &lt;&lt;EOF
  kind: NetworkPolicy
  apiVersion: networking.k8s.io/v1
  metadata:
    name: default-deny
    namespace: &lt;namespace&gt;
  spec:
    podSelector: {}
  EOF
  </code>
  </pre>

  <li> ネットワーキング・ポリシーを追加した後、`net.beta.kubernetes.io/network-policy` アノテーションを削除します。
  ```
  kubectl annotate ns <namespace> --overwrite "net.beta.kubernetes.io/network-policy-"
  ```
  </li></ol>
</td></tr>
<tr>
<td>ポッド・アフィニティー・スケジューリング</td>
<td> `scheduler.alpha.kubernetes.io/affinity` アノテーションは推奨されていません。
<ol>
  <li>`ibm-system` と `kube-system` 以外の名前空間ごとに、ポッド・アフィニティー・スケジューリングを更新する必要があるかどうかを判別します。</br>
  ```
  kubectl get pods -n <namespace> -o yaml | grep "scheduler.alpha.kubernetes.io/affinity" && echo "Action required"
  ```
  </br></li>
  <li>`「Action required」`が返された場合、`scheduler.alpha.kubernetes.io/affinity` アノテーションではなく [_PodSpec_ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/api-reference/v1.7/#podspec-v1-core) の _affinity_ フィールドを使用します。</li>
</ol>
</td></tr>
<tr>
<td>`default` `ServiceAccount` の RBAC</td>
<td><p>`default` 名前空間の `default` `ServiceAccount` の管理者 `ClusterRoleBinding` は、クラスターのセキュリティー向上のために削除されました。 `default` 名前空間で実行されるアプリケーションは、Kubernetes API に対するクラスター管理者特権を失うので、`RBAC DENY` 権限エラーが発生する可能性があります。 使用するアプリとその `.yaml` ファイルを調べて、アプリが `default` 名前空間で実行され、`default ServiceAccount` を使用し、Kubernetes API にアクセスするかどうかを確認してください。</p>
<p>これらの特権にアプリケーションが依存している場合は、アプリのために [RBAC 権限リソースを作成![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview)してください。</p>
  <p>アプリの RBAC ポリシーを更新する際に、前の `default` に一時的に戻す必要がある場合があります。 `kubectl apply -f FILENAME` コマンドを使用して、以下のファイルをコピー、保存、適用してください。 <strong>注</strong>: 長期的な解決策としてではなく、すべてのアプリケーション・リソースを更新する時間を確保するために戻ります。</p>

<p><pre class="codeblock">
<code>
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
 name: admin-binding-nonResourceURLSs-default
subjects:
  - kind: ServiceAccount
    name: default
    namespace: default
roleRef:
 kind: ClusterRole
 name: admin-role-nonResourceURLSs
 apiGroup: rbac.authorization.k8s.io
---
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
 name: admin-binding-resourceURLSs-default
subjects:
  - kind: ServiceAccount
    name: default
    namespace: default
roleRef:
 kind: ClusterRole
 name: admin-role-resourceURLSs
 apiGroup: rbac.authorization.k8s.io
</code>
</pre></p>
</td>
</tr>
<tr>
<td>読み取り専用 API データ・ボリューム</td>
<td>`secret`、`configMap`、`downwardAPI`、および投影ボリュームは、読み取り専用でマウントされるようになります。
これまでは、システムによって自動的に元の状態に戻されることがあるこれらのボリュームに、アプリがデータを書き込めました。 このマイグレーション操作は、
セキュリティーの脆弱性 [CVE-2017-1002102](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2017-1002102) を修正するために必要です。
アプリが以前の非セキュアな動作に依存している場合は、適切に変更してください。</td>
</tr>
<tr>
<td>StatefulSet ポッド DNS</td>
<td>マスターの更新後、StatefulSet ポッドはその Kubernetes DNS エントリーを失います。DNS エントリーを復元するには、StatefulSet ポッドを削除します。 Kubernetes はポッドを再作成して、DNS エントリーを自動的に復元します。 詳しくは、[Kubernetes issue ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/issues/48327) を参照してください。</td>
</tr>
<tr>
<td>耐障害性</td>
<td>`scheduler.alpha.kubernetes.io/tolerations` アノテーションは使用できなくなりました。
<ol>
  <li>`ibm-system` と `kube-system` 以外の名前空間ごとに、耐障害性を変更する必要があるかどうかを判別します。</br>
  ```
  kubectl get pods -n <namespace> -o yaml | grep "scheduler.alpha.kubernetes.io/tolerations" && echo "Action required"
  ```
  </br>

  <li>`「Action required」`が返された場合、`scheduler.alpha.kubernetes.io/tolerations` アノテーションではなく [_PodSpec_ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/api-reference/v1.7/#podspec-v1-core) の _tolerations_ フィールドを使用します。
</ol>
</td></tr>
<tr>
<td>テイント</td>
<td>`scheduler.alpha.kubernetes.io/taints` アノテーションは使用できなくなりました。
<ol>
  <li>テイントを変更する必要があるかどうかを判別します。</br>
  ```
  kubectl get nodes -o yaml | grep "scheduler.alpha.kubernetes.io/taints" && echo "Action required"
  ```
  <li>`「Action required」`が返された場合、各ノードの `scheduler.alpha.kubernetes.io/taints` アノテーションを削除します。</br>
  `kubectl annotate nodes <node> scheduler.alpha.kubernetes.io/taints-`
  <li>各ノードにテイントを追加します。</br>
  `kubectl taint node <node> <taint>`
  </li></ol>
</td></tr>
</tbody>
</table>

<br />


### バージョン 1.5 (サポート対象外)
{: #cs_v1-5}

2018 年 4 月 4 日現在、[Kubernetes バージョン 1.5](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.5.md) を実行する {{site.data.keyword.containershort_notm}} クラスターはサポートされていません。 バージョン 1.5 クラスターは、次に最新のバージョン ([Kubernetes 1.7](#cs_v17)) に更新しない限り、セキュリティー更新もサポートも受けられません。

各 Kubernetes バージョンの更新が[与える可能性のある影響を確認](cs_versions.html#cs_versions)したうえで、ただちに[クラスターを更新](cs_cluster_update.html#update)してください。 更新する際は、あるバージョンからその次に最新のバージョンに (例えば、1.5 から 1.7 に、または 1.8 から 1.9 に) 更新する必要があります。
