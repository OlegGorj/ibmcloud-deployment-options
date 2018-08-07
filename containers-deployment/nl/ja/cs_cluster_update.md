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





# クラスターとワーカー・ノードの更新
{: #update}

更新をインストールして、{{site.data.keyword.containerlong}} の Kubernetes クラスターを最新に保つことができます。
{:shortdesc}

## Kubernetes マスターの更新
{: #master}

Kubernetes は、定期的に[メジャー、マイナー、またはパッチの更新](cs_versions.html#version_types)をリリースしています。 更新のタイプによっては、Kubernetes マスター・コンポーネントの更新を自分で行う必要があります。
{:shortdesc}

更新は、Kubernetes API サーバーのバージョンや Kubernetes マスター内の他のコンポーネントに影響を与える可能性があります。  ワーカー・ノードについては、必ず自分で最新状態に保つ必要があります。 更新を適用する際には、Kubernetes マスターを更新してから、ワーカー・ノードを更新します。

デフォルトでは、Kubernetes API サーバーを正常に更新できるのは、Kubernetes マスターの現行バージョンから 2 つ先のマイナー・バージョンまでに制限されています。 例えば、現在の Kubernetes API サーバーのバージョンが 1.7 の場合に、1.10 に更新するためには、まず 1.8 または 1.9 に更新する必要があります。更新を強制実行することは可能ですが、3 つ以上先のマイナー・バージョンの更新を試みると、予期しない結果が生じるおそれがあります。サポートされない Kubernetes バージョンがクラスターで実行されている場合は、更新を強制する必要がある場合があります。

以下の図は、マスターを更新するときに行えるプロセスを示しています。

![マスター更新のベスト・プラクティス](/images/update-tree.png)

図 1. Kubernetes マスター更新プロセスの図

**注意**: 更新プロセスが開始されたら、クラスターを前のバージョンにロールバックすることはできません。 必ず、テスト・クラスターを使用し、手順に従って潜在する問題に対処してから、実動マスターを更新してください。

_メジャー_ または_マイナー_ の更新の場合、以下の手順を実行します。

1. [Kubernetes の変更点](cs_versions.html)を確認し、『_マスターの前に行う更新_』というマークのある更新を実行します。
2. GUI を使用するか [CLI コマンド](cs_cli_reference.html#cs_cluster_update)を実行して、Kubernetes API サーバーおよび関連する Kubernetes マスター・コンポーネントを更新します。 Kubernetes API サーバーを更新する際、API サーバーは約 5 分間から 10 分間ダウンします。 更新中、クラスターにアクセスすることも変更することもできません。 ただし、クラスター・ユーザーがデプロイしたワーカー・ノード、アプリ、リソースは変更されず、引き続き実行されます。
3. 更新が完了したことを確認します。 {{site.data.keyword.Bluemix_notm}} ダッシュボードで Kubernetes API サーバーのバージョンを確認するか、`bx cs clusters` を実行します。
4. Kubernetes マスターで実行されている Kubernetes API サーバーと同じバージョンの [`kubectl cli`](cs_cli_install.html#kubectl) をインストールします。

Kubernetes API サーバーの更新が完了したら、ワーカー・ノードを更新できます。

<br />


## ワーカー・ノードの更新
{: #worker_node}


ワーカー・ノードの更新を促す通知を受信する場合があります。 これは何を意味しているでしょうか。 セキュリティー更新とパッチが Kubernetes API サーバーと他の Kubernetes マスター・コンポーネントに適用されたので、ワーカー・ノードを同期する必要があります。
{: shortdesc}

ワーカー・ノードの Kubernetes バージョンを、Kubernetes マスターで実行される Kubernetes API サーバーのバージョンより高くすることはできません。 始める前に、[Kubernetes マスター](#master)を更新してください。

**注意**:
<ul><li>ワーカー・ノードを更新すると、アプリとサービスにダウン時間が発生する可能性があります。</li>
<li>データは、ポッドの外部に保管されていない場合、削除されます。</li>
<li>使用可能なノードにポッドをスケジュール変更するには、デプロイメントで[レプリカ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#replicas) を使用します。</li></ul>

ダウン時間を許容できない場合はどうしますか?

更新プロセスの過程で、特定のノードが一定時間ダウンします。 アプリケーションのダウン時間を回避するため、アップグレード・プロセス中の、特定のタイプのノードのしきい値パーセンテージを指定する構成マップで、固有キーを定義することができます。 標準の Kubernetes ラベルに基づいたルールを定義し、使用不可にできるノード最大量のパーセンテージを指定することで、確実にアプリを稼働したままにすることができます。 ノードは、デプロイ・プロセスを完了していない場合、使用不可と見なされます。

キーをどのように定義しますか?

構成マップのデータ情報セクションで、最大 10 個のルールを個別に定義していつでも実行できます。 アップグレードするためには、定義されたすべてのルールにワーカー・ノードが合格する必要があります。

キーが定義されました。 次に何をしますか?

ルールを定義したら、`bx cs worker-update` コマンドを実行します。 成功を示す応答が返された場合、ワーカー・ノードは更新待機中となります。 ただし、すべてのルールが満たされるまでは、ノードの更新プロセスが実行されません。 待機中は一定の間隔でルールが検査され、いずれかのノードを更新できないか判別されます。

構成マップを定義しないことを選択した場合はどうなりますか?

構成マップを定義しない場合、デフォルトが使用されます。 デフォルトでは、更新処理中、クラスターごとに全ワーカー・ノードの最大 20% が使用不可になります。

ワーカー・ノードを更新するには、以下のようにします。

1. [Kubernetes の変更](cs_versions.html)の『_マスターの後に行う更新_』に記載されている変更作業を行います。

2. オプション: 構成マップを定義します。
    例:

    ```
    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: ibm-cluster-update-configuration
      namespace: kube-system
    data:
     drain_timeout_seconds: "120"
     zonecheck.json: |
       {
         "MaxUnavailablePercentage": 70,
         "NodeSelectorKey": "failure-domain.beta.kubernetes.io/zone",
         "NodeSelectorValue": "dal13"
       }
     regioncheck.json: |
       {
         "MaxUnavailablePercentage": 80,
         "NodeSelectorKey": "failure-domain.beta.kubernetes.io/region",
         "NodeSelectorValue": "us-south"
       }
     defaultcheck.json: |
       {
         "MaxUnavailablePercentage": 100
       }
    ```
    {:pre}
  <table summary="表の 1 行目は 2 列にまたがっています。残りの行は左から右に読みます。1 列目はパラメーター、2 列目は対応する説明です。">
  <caption>ConfigMap コンポーネント</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> 構成要素について </th>
    </thead>
    <tbody>
      <tr>
        <td><code>drain_timeout_seconds</code></td>
        <td> オプション: ワーカー・ノードの更新中に発生するドレーンのタイムアウト (秒)。 ドレーンはノードを `unschedulable` に設定します。これにより、新しいポッドがそのノードにデプロイされるのを防ぎます。 ドレーンはさらに、ノードからポッドを削除します。 指定可能な値は、1 から 180 までの整数です。 デフォルト値は	30 です。</td>
      </tr>
      <tr>
        <td><code>zonecheck.json</code></br><code>regioncheck.json</code></td>
        <td> ルールを設定する固有キーの例。 キーの名前は、任意に設定できます。キー内に設定された構成によって、情報が解析されます。 定義したキーごとに、<code>NodeSelectorKey</code> と <code>NodeSelectorValue</code> に値を 1 つだけ設定できます。 複数の地域またはロケーション (データ・センター) のルールを設定する場合は、新規キー項目を作成します。 </td>
      </tr>
      <tr>
        <td><code>defaultcheck.json</code></td>
        <td> デフォルトとして、<code>ibm-cluster-update-configuration</code> マップが有効な方法で定義されていない場合に、一度にクラスターの 20% のみを使用不可にすることができます。 グローバル・デフォルトなしで、1 つ以上の有効なルールが定義されている場合、新規デフォルトでは、一度にワーカーの 100% を使用不可にすることを許容します。 これを制御するには、デフォルト・パーセンテージを作成します。 </td>
      </tr>
      <tr>
        <td><code>MaxUnavailablePercentage</code></td>
        <td> 使用不可にできる、指定されたキーのノードの最大量 (パーセンテージで指定)。 ノードは、デプロイ、再ロード、またはプロビジョニングのプロセス中に使用不可になります。 定義した使用不可ノードの最大パーセンテージを超える場合、待機中のワーカー・ノードのアップグレードはブロックされます。 </td>
      </tr>
      <tr>
        <td><code>NodeSelectorKey</code></td>
        <td> 指定されたキーのルールを設定するラベルのタイプ。 IBM 提供のデフォルト・ラベルと、自分で作成したラベルにルールを設定できます。 </td>
      </tr>
      <tr>
        <td><code>NodeSelectorValue</code></td>
        <td> 評価するルールが設定される、指定されたキー内のノードのサブセット。 </td>
      </tr>
    </tbody>
  </table>

    **注**: 最大 10 個のルールを定義できます。 10 を超えるキーを 1 つのファイルに追加した場合、情報のサブセットのみが解析されます。

3. GUI で、または CLI コマンドを実行して、ワーカー・ノードを更新します。
  * {{site.data.keyword.Bluemix_notm}} ダッシュボードから更新するには、クラスターの`「ワーカー・ノード (Worker Nodes)」`セクションにナビゲートし、`「ワーカーの更新 (Update Worker)」`をクリックします。
  * ワーカー・ノード ID を取得するには、`bx cs workers <cluster_name_or_ID>` を実行します。 複数のワーカー・ノードを選択した場合、ワーカー・ノードは更新評価のためにキューに置かれます。 評価の後、準備できていると見なされるなら、構成に設定されたルールに従って更新されます

    ```
    bx cs worker-update <cluster_name_or_ID> <worker_node1_ID> <worker_node2_ID>
    ```
    {: pre}

4. オプション: 以下のコマンドを実行し、**Events** を参照することで、構成マップによってトリガーされたイベントと、発生した検証エラーを確認します。
    ```
    kubectl describe -n kube-system cm ibm-cluster-update-configuration
    ```
    {: pre}

5. 更新が完了したことを確認します。
  * {{site.data.keyword.Bluemix_notm}} ダッシュボードで Kubernetes バージョンを確認するか、`bx cs workers<cluster_name_or_ID>` を実行します。
  * `kubectl get nodes` を実行して、ワーカー・ノードの Kubernets のバージョンを確認します。
  * 場合によっては、更新後に古いクラスターで、**NotReady** 状況の重複したワーカー・ノードがリスト表示されることがあります。 重複を削除するには、[トラブルシューティング](cs_troubleshoot_clusters.html#cs_duplicate_nodes)を参照してください。

次のステップ:
  - 他のクラスターで更新処理を繰り返します。
  - クラスターで作業を行う開発者に、`kubectl` CLI を Kubernetes マスターのバージョンに更新するように伝えます。
  - Kubernetes ダッシュボードに使用状況グラフが表示されない場合は、[`kube-dashboard` ポッドを削除](cs_troubleshoot_health.html#cs_dashboard_graphs)します。
  





<br />



## マシン・タイプの更新
{: #machine_type}

ワーカー・ノードのマシン・タイプを更新するには、新しいワーカー・ノードを追加し、古いワーカー・ノードを削除します。例えば、名前に `u1c` または `b1c` を含む非推奨のマシン・タイプに仮想ワーカー・ノードがある場合は、名前に `u2c` または `b2c` を含むマシン・タイプを使用するワーカー・ノードを作成してください。
{: shortdesc}

開始前に、以下のことを行います。
- [CLI のターゲットを](cs_cli_install.html#cs_cli_configure)自分のクラスターに設定します。
- ワーカー・ノードにデータを保管している場合、[ワーカー・ノードの外部に保管](cs_storage.html#storage)されていなければデータは削除されます。


**注意**: ワーカー・ノードを更新すると、アプリとサービスにダウン時間が発生する可能性があります。 データは、[ポッドの外部に保管](cs_storage.html#storage)されていない場合、削除されます。



1. 更新するワーカー・ノードの名前と場所をメモします。
    ```
    bx cs workers <cluster_name>
    ```
    {: pre}

2. 使用可能なマシン・タイプを表示します。
    ```
    bx cs machine-types <location>
    ```
    {: pre}

3. [bx cs worker-add](cs_cli_reference.html#cs_worker_add) コマンドを使用してワーカー・ノードを追加します。 マシン・タイプを指定します。

    ```
    bx cs worker-add --cluster <cluster_name> --machine-type <machine_type> --number <number_of_worker_nodes> --private-vlan <private_VLAN_ID> --public-vlan <public_VLAN_ID>
    ```
    {: pre}

4. ワーカー・ノードが追加されていることを確認します。

    ```
    bx cs workers <cluster_name>
    ```
    {: pre}

5. 追加されたワーカー・ノードが `Normal` 状態の場合、古いワーカー・ノードを削除できます。 **注**: 月次で請求されるマシン・タイプ (ベア・メタルなど) を削除する場合は、その月全体の料金を請求されます。

    ```
    bx cs worker-rm <cluster_name> <worker_node>
    ```
    {: pre}

6. 上記の手順を繰り返して、他のワーカー・ノードを別のマシン・タイプにアップグレードします。










