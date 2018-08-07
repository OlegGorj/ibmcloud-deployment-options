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



# バージョンの変更ログ
{: #changelog}

{{site.data.keyword.containerlong}} Kubernetes クラスターに利用可能なメジャー、マイナー、パッチの更新に関するバージョン変更の情報を表示します。 変更には、Kubernetes および {{site.data.keyword.Bluemix_notm}} Provider コンポーネントへの更新が含まれます。 
{:shortdesc}

マスターには IBM がパッチ・レベルの更新を自動的に適用しますが、[ワーカー・ノードはお客様が更新する](cs_cluster_update.html#worker_node)必要があります。 利用可能な更新がないか毎月確認してください。 更新が利用可能になると、`bx cs workers <cluster>` や `bx cs worker-get <cluster> <worker>` コマンドなどを使用してワーカー・ノードの情報を表示したときに通知されます。

マイグレーション操作の要約については、[Kubernetes のバージョン](cs_versions.html)を参照してください。
{: tip}

前のバージョンからの変更点については、以下の変更ログを参照してください。
-  バージョン 1.10 [変更ログ](#110_changelog)。
-  バージョン 1.9 [変更ログ](#19_changelog)。
-  バージョン 1.8 [変更ログ](#18_changelog)。
-  **非推奨**: バージョン 1.7 [変更ログ](#17_changelog)。

## バージョン 1.10 変更ログ
{: #110_changelog}

以下の変更点を確認します。

### ワーカー・ノード・フィックスパック 1.10.1_1510 (2018 年 5 月 18 日リリース) の変更ログ
{: #1101_1510}

<table summary="バージョン 1.10.1_1509 からの変更点">
<caption>バージョン 1.10.1_1509 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>該当なし</td>
<td>該当なし</td>
<td>ブロック・ストレージ・プラグインを使用したときに発生するバグに対応するための修正です。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.10.1_1509 (2018 年 5 月 16 日リリース) の変更ログ
{: #1101_1509}

<table summary="バージョン 1.10.1_1508 からの変更点">
<caption>バージョン 1.10.1_1508 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`kubelet` ルート・ディレクトリーに保管したデータが、ワーカー・ノード・マシンのより大きい 2 次ディスクに保存されるようになりました。</td>
</tr>
</tbody>
</table>

### 1.10.1_1508 (2018 年 5 月 1 日リリース) の変更ログ
{: #1101_1508}

<table summary="バージョン 1.9.7_1510 からの変更点">
<caption>バージョン 1.9.7_1510 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v2.6.5</td>
<td>v3.1.1</td>
<td>Calico [リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.projectcalico.org/v3.1/releases/#v311) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes Heapster</td>
<td>v1.5.0</td>
<td>v1.5.2</td>
<td>Kubernetes Heapster [リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/heapster/releases/tag/v1.5.2) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.7</td>
<td>v1.10.1</td>
<td>Kubernetes [リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.1) を参照してください。</td>
</tr>
<tr>
<td>Kubernetes 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>クラスターの Kubernetes API サーバーの <code>--enable-admission-plugins</code> オプションに <code>StorageObjectInUseProtection</code> が追加されました。</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>1.14.8</td>
<td>1.14.10</td>
<td>Kubernetes DNS [リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/dns/releases/tag/1.14.10) を参照してください。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.9.7-102</td>
<td>v1.10.1-52</td>
<td>Kubernetes 1.10 リリースをサポートするように更新されました。</td>
</tr>
<tr>
<td>GPU サポート</td>
<td>該当なし</td>
<td>該当なし</td>
<td>スケジューリングおよび実行で、[グラフィックス・プロセッシング・ユニット (GPU) コンテナー・ワークロード](cs_app.html#gpu_app)がサポートされるようになりました。使用可能な GPU マシン・タイプのリストについては、[ワーカー・ノード用のハードウェア](cs_clusters.html#shared_dedicated_node)を参照してください。詳しくは、[GPU のスケジュール ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/) に関する Kubernetes の資料を参照してください。</td>
</tr>
</tbody>
</table>

## バージョン 1.9 変更ログ
{: #19_changelog}

以下の変更点を確認します。

### ワーカー・ノード・フィックスパック 1.9.7_1512 (2018 年 5 月 18 日リリース) の変更ログ
{: #197_1512}

<table summary="バージョン 1.9.7_1511 からの変更点">
<caption>バージョン 1.9.7_1511 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>該当なし</td>
<td>該当なし</td>
<td>ブロック・ストレージ・プラグインを使用したときに発生するバグに対応するための修正です。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.9.7_1511 (2018 年 5 月 16 日リリース) の変更ログ
{: #197_1511}

<table summary="バージョン 1.9.7_1510 からの変更点">
<caption>バージョン 1.9.7_1510 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`kubelet` ルート・ディレクトリーに保管したデータが、ワーカー・ノード・マシンのより大きい 2 次ディスクに保存されるようになりました。</td>
</tr>
</tbody>
</table>

### 1.9.7_1510 (2018 年 4 月 30 日リリース) の変更ログ
{: #197_1510}

<table summary="バージョン 1.9.3_1506 からの変更点">
<caption>バージョン 1.9.3_1506 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.9.3</td>
<td>v1.9.7	</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.7) を参照してください。 このリリースで、[CVE-2017-1002101 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) および [CVE-2017-1002102 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102) の脆弱性に対応しています。</td>
</tr>
<tr>
<td>Kubernetes 構成</td>
<td>該当なし</td>
<td>該当なし</td>
<td>クラスターの Kubernetes API サーバーの `--runtime-config` オプションに `admissionregistration.k8s.io/v1alpha1=true` が追加されました。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.9.3-71</td>
<td>v1.9.7-102</td>
<td>`NodePort` と `LoadBalancer` サービスは、`service.spec.externalTrafficPolicy` を `Local` に設定すると、[クライアント・ソース IP の保持](cs_loadbalancer.html#node_affinity_tolerations)をサポートするようになりました。</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>以前のクラスターの[エッジ・ノード](cs_edge.html#edge)耐障害性セットアップを修正します。</td>
</tr>
</tbody>
</table>

## バージョン 1.8 変更ログ
{: #18_changelog}

以下の変更点を確認します。

### ワーカー・ノード・フィックスパック 1.8.11_1511 (2018 年 5 月 18 日リリース) の変更ログ
{: #1811_1511}

<table summary="バージョン 1.8.11_1510 からの変更点">
<caption>バージョン 1.8.11_1510 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>該当なし</td>
<td>該当なし</td>
<td>ブロック・ストレージ・プラグインを使用したときに発生するバグに対応するための修正です。</td>
</tr>
</tbody>
</table>

### ワーカー・ノード・フィックスパック 1.8.11_1510 (2018 年 5 月 16 日リリース) の変更ログ
{: #1811_1510}

<table summary="バージョン 1.8.11_1509 からの変更点">
<caption>バージョン 1.8.11_1509 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`kubelet` ルート・ディレクトリーに保管したデータが、ワーカー・ノード・マシンのより大きい 2 次ディスクに保存されるようになりました。</td>
</tr>
</tbody>
</table>


### 1.8.11_1509 (2018 年 4 月 19 日リリース) の変更ログ
{: #1811_1509}

<table summary="バージョン 1.8.8_1507 からの変更点">
<caption>バージョン 1.8.8_1507 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.8.8</td>
<td>v1.8.11	</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.11) を参照してください。 このリリースで、[CVE-2017-1002101 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) および [CVE-2017-1002102 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102) の脆弱性に対応しています。</td>
</tr>
<tr>
<td>一時停止コンテナーのイメージ</td>
<td>3.0</td>
<td>3.1</td>
<td>継承したオーファン・ゾンビ・プロセスを削除します。</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.8.8-86</td>
<td>v1.8.11-126</td>
<td>`NodePort` と `LoadBalancer` サービスは、`service.spec.externalTrafficPolicy` を `Local` に設定すると、[クライアント・ソース IP の保持](cs_loadbalancer.html#node_affinity_tolerations)をサポートするようになりました。</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>以前のクラスターの[エッジ・ノード](cs_edge.html#edge)耐障害性セットアップを修正します。</td>
</tr>
</tbody>
</table>

## アーカイブ
{: #changelog_archive}

### バージョン 1.7 変更ログ (非推奨)
{: #17_changelog}

以下の変更点を確認します。

#### ワーカー・ノード・フィックスパック 1.7.16_1513 (2018 年 5 月 18 日リリース) の変更ログ
{: #1716_1513}

<table summary="バージョン 1.7.16_1512 からの変更点">
<caption>バージョン 1.7.16_1512 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>該当なし</td>
<td>該当なし</td>
<td>ブロック・ストレージ・プラグインを使用したときに発生するバグに対応するための修正です。</td>
</tr>
</tbody>
</table>

#### ワーカー・ノード・フィックスパック 1.7.16_1512 (2018 年 5 月 16 日リリース) の変更ログ
{: #1716_1512}

<table summary="バージョン 1.7.16_1511 からの変更点">
<caption>バージョン 1.7.16_1511 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>該当なし</td>
<td>該当なし</td>
<td>`kubelet` ルート・ディレクトリーに保管したデータが、ワーカー・ノード・マシンのより大きい 2 次ディスクに保存されるようになりました。</td>
</tr>
</tbody>
</table>

#### 1.7.16_1511 (2018 年 4 月 19 日リリース) の変更ログ
{: #1716_1511}

<table summary="バージョン 1.7.4_1509 からの変更点">
<caption>バージョン 1.7.4_1509 からの変更点</caption>
<thead>
<tr>
<th>コンポーネント</th>
<th>以前</th>
<th>現在</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.7.4</td>
<td>v1.7.16	</td>
<td>[Kubernetes リリース・ノート ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/kubernetes/kubernetes/releases/tag/v1.7.16) を参照してください。 このリリースで、[CVE-2017-1002101 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) および [CVE-2017-1002102 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102) の脆弱性に対応しています。</td>
</tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.7.4-133</td>
<td>v1.7.16-17</td>
<td>`NodePort` と `LoadBalancer` サービスは、`service.spec.externalTrafficPolicy` を `Local` に設定すると、[クライアント・ソース IP の保持](cs_loadbalancer.html#node_affinity_tolerations)をサポートするようになりました。</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>以前のクラスターの[エッジ・ノード](cs_edge.html#edge)耐障害性セットアップを修正します。</td>
</tr>
</tbody>
</table>
