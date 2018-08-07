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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# クラスターのデバッグ
{: #cs_troubleshoot}

{{site.data.keyword.containerlong}} を使用する際は、ここに示す一般的なクラスターのトラブルシューティング手法とデバッグ手法を検討してください。 [{{site.data.keyword.Bluemix_notm}} システムの状況 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.ibm.com/bluemix/support/#status) を確認することもできます。
{: shortdesc}

以下の一般的な手順を実行して、クラスターが最新の状態であることを確認できます。
- [ワーカー・ノードを更新](cs_cli_reference.html#cs_worker_update)するためのセキュリティー・パッチやオペレーティング・システム・パッチが使用可能になっていないか、毎月確認してください。
- クラスターを {{site.data.keyword.containershort_notm}} 用の [Kubernetes の最新のデフォルト・バージョン](cs_versions.html)に[更新](cs_cli_reference.html#cs_cluster_update)します

## クラスターのデバッグ
{: #debug_clusters}

クラスターをデバッグするためのオプションを確認し、障害の根本原因を探します。

1.  クラスターをリストし、クラスターの `State` を見つけます。

  ```
  bx cs clusters
  ```
  {: pre}

2.  クラスターの `State` を確認します。 クラスターが **Critical**、**Delete failed**、または **Warning** 状態の場合、あるいは **Pending** 状態が長時間続いている場合は、[ワーカー・ノードのデバッグ](#debug_worker_nodes)を開始してください。

    <table summary="表の行はすべて左から右に読みます。1 列目はクラスターの状態、2 列目は説明です。">
<caption>クラスターの状態</caption>
   <thead>
   <th>クラスターの状態</th>
   <th>説明</th>
   </thead>
   <tbody>
<tr>
   <td>Aborted</td>
   <td>Kubernetes マスターがデプロイされる前にユーザーからクラスターの削除が要求されました。 クラスターの削除が完了すると、クラスターはダッシュボードから除去されます。 クラスターが長時間この状態になっている場合は、[{{site.data.keyword.Bluemix_notm}} サポート・チケット](cs_troubleshoot.html#ts_getting_help)を開いてください。</td>
   </tr>
 <tr>
     <td>Critical</td>
     <td>Kubernetes マスターにアクセスできないか、クラスター内のワーカー・ノードがすべてダウンしています。 </td>
    </tr>
   <tr>
     <td>Delete failed</td>
     <td>Kubernetes マスターまたは 1 つ以上のワーカー・ノードを削除できません。  </td>
   </tr>
   <tr>
     <td>Deleted</td>
     <td>クラスターは削除されましたが、まだダッシュボードからは除去されていません。 クラスターが長時間この状態になっている場合は、[{{site.data.keyword.Bluemix_notm}} サポート・チケット](cs_troubleshoot.html#ts_getting_help)を開いてください。 </td>
   </tr>
   <tr>
   <td>Deleting</td>
   <td>クラスターの削除とクラスター・インフラストラクチャーの破棄を実行中です。 クラスターにアクセスできません。  </td>
   </tr>
   <tr>
     <td>Deploy failed</td>
     <td>Kubernetes マスターのデプロイメントを完了できませんでした。 この状態はお客様には解決できません。 [{{site.data.keyword.Bluemix_notm}} サポート・チケット](cs_troubleshoot.html#ts_getting_help)を開いて、IBM Cloud サポートに連絡してください。</td>
   </tr>
     <tr>
       <td>Deploying</td>
       <td>Kubernetes マスターがまだ完全にデプロイされていません。 クラスターにアクセスできません。 クラスターが完全にデプロイされるまで待ってからクラスターの正常性を確認してください。</td>
      </tr>
      <tr>
       <td>Normal</td>
       <td>クラスター内のすべてのワーカー・ノードが稼働中です。 クラスターにアクセスし、アプリをクラスターにデプロイできます。 この状態は正常と見なされるので、アクションは必要ありません。 **注**: ワーカー・ノードは正常であっても、[ネットワーキング](cs_troubleshoot_network.html)や[ストレージ](cs_troubleshoot_storage.html)などの他のインフラストラクチャー・リソースには注意が必要な可能性もあります。</td>
    </tr>
      <tr>
       <td>Pending</td>
       <td>Kubernetes マスターはデプロイされています。 ワーカー・ノードはプロビジョン中であるため、まだクラスターでは使用できません。 クラスターにはアクセスできますが、アプリをクラスターにデプロイすることはできません。  </td>
     </tr>
   <tr>
     <td>Requested</td>
     <td>クラスターを作成し、Kubernetes マスターとワーカー・ノードのインフラストラクチャーを注文するための要求が送信されました。 クラスターのデプロイメントが開始されると、クラスターの状態は「<code>Deploying</code>」に変わります。 クラスターが長時間「<code>Requested</code>」状態になっている場合は、[{{site.data.keyword.Bluemix_notm}} サポート・チケット](cs_troubleshoot.html#ts_getting_help)を開いてください。 </td>
   </tr>
   <tr>
     <td>Updating</td>
     <td>Kubernetes マスターで実行される Kubernetes API サーバーが、新しい Kubernetes API バージョンに更新されています。 更新中、クラスターにアクセスすることも変更することもできません。 ユーザーがデプロイしたワーカー・ノード、アプリ、リソースは変更されず、引き続き実行されます。更新が完了するまで待ってから、クラスターの正常性を確認してください。 </td>
   </tr>
    <tr>
       <td>Warning</td>
       <td>クラスター内の 1 つ以上のワーカー・ノードが使用不可です。ただし、他のワーカー・ノードが使用可能であるため、ワークロードを引き継ぐことができます。 </td>
    </tr>
   </tbody>
 </table>


<br />


## ワーカー・ノードのデバッグ
{: #debug_worker_nodes}

ワーカー・ノードをデバッグするためのオプションを確認し、障害の根本原因を探します。


1.  クラスターが **Critical**、**Delete failed**、または **Warning** 状態の場合、あるいは **Pending** 状態が長時間続いている場合は、ワーカー・ノードの状態を確認してください。

  ```
  bx cs workers <cluster_name_or_id>
  ```
  {: pre}

2.  CLI 出力内ですべてのワーカー・ノードの `State` フィールドと `Status` フィールドを確認します。

  <table summary="表の行はすべて左から右に読みます。1 列目はクラスターの状態、2 列目は説明です。">
  <caption>ワーカー・ノードの状態</caption>
    <thead>
    <th>ワーカー・ノードの状態</th>
    <th>説明</th>
    </thead>
    <tbody>
  <tr>
      <td>Critical</td>
      <td>ワーカー・ノードは、次のようなさまざまな理由で Critical 状態になることがあります。<ul><li>閉鎖と排出を行わずに、ワーカー・ノードのリブートを開始した。。 ワーカー・ノードをリブートすると、<code>docker</code>、<code>kubelet</code>、<code>kube-proxy</code>、<code>calico</code> でデータ破損が発生する可能性があります。 </li><li>ワーカー・ノードにデプロイしたポッドが、[メモリー ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/tasks/configure-pod-container/assign-memory-resource/) と [CPU ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/tasks/configure-pod-container/assign-cpu-resource/) のリソース制限を使用していない。 リソース制限を使用しないと、ポッドが、使用可能なリソースをすべて使い果たして、このワーカー・ノード上の他のポッドを実行するためのリソースがなくなる可能性があります。 この過剰なワークロードにより、ワーカー・ノードに障害が発生します。 </li><li>何百、何千ものコンテナーを長時間実行した後、<code>Docker</code>、<code>kubelet</code>、または <code>calico</code> がリカバリー不能な状態になった。</li><li>ワーカー・ノード用にセットアップした Virtual Router Appliance が停止したために、ワーカー・ノードと Kubernetes マスターの間の通信が切断された。</li><li> {{site.data.keyword.containershort_notm}} または IBM Cloud インフラストラクチャー (SoftLayer) の現在のネットワーキングの問題によって、ワーカー・ノードと Kubernetes マスターが通信できなくなっている。</li><li>ワーカー・ノードが容量を使い尽くした。 ワーカー・ノードの <strong>Status</strong> に <strong>Out of disk</strong> または <strong>Out of memory</strong> と表示されていないか確認します。ワーカー・ノードが容量を使い尽くしている場合は、ワーカー・ノードのワークロードを減らすか、ワークロードの負荷を分散できるようにクラスターにワーカー・ノードを追加してください。</li></ul> 多くの場合、ワーカー・ノードを[再ロードする](cs_cli_reference.html#cs_worker_reload)と問題を解決できます。 ワーカー・ノードを再ロードすると、最新の[パッチ・バージョン](cs_versions.html#version_types)がワーカー・ノードに適用されます。 メジャー・バージョンとマイナー・バージョンは変更されません。 必ず、ワーカー・ノードを再ロードする前に、ワーカー・ノードを閉鎖して排出してください。これにより、既存のポッドが正常終了し、残りのワーカー・ノードに再スケジュールされます。 </br></br> ワーカー・ノードを再ロードしても問題が解決しない場合は、次の手順に進み、ワーカー・ノードのトラブルシューティングを続けてください。 </br></br><strong>ヒント:</strong> [ワーカー・ノードのヘルス・チェックを構成し、Autorecovery を有効にする](cs_health.html#autorecovery)ことができます。 Autorecovery は、構成された検査に基づいて正常でないワーカー・ノードを検出すると、ワーカー・ノードの OS の再ロードのような修正アクションをトリガーします。 Autorecovery の仕組みについて詳しくは、[Autorecovery のブログ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/) を参照してください。
      </td>
     </tr>
      <tr>
        <td>Deploying</td>
        <td>ワーカー・ノードの Kubernetes バージョンを更新する際には、更新をインストールするためにワーカー・ノードが再デプロイされます。 ワーカー・ノードが長時間この状態になっている場合は、次のステップに進み、デプロイメント中に問題が発生したかどうかを調べてください。</td>
     </tr>
        <tr>
        <td>Normal</td>
        <td>ワーカー・ノードは完全にプロビジョンされ、クラスターで使用できる状態です。 この状態は正常と見なされるので、ユーザーのアクションは必要ありません。 **注**: ワーカー・ノードは正常であっても、[ネットワーキング](cs_troubleshoot_network.html)や[ストレージ](cs_troubleshoot_storage.html)などの他のインフラストラクチャー・リソースには注意が必要な可能性もあります。</td>
     </tr>
   <tr>
        <td>Provisioning</td>
        <td>ワーカー・ノードはプロビジョン中であるため、まだクラスターでは使用できません。 CLI 出力の <strong>Status</strong> 列で、プロビジョニングのプロセスをモニターできます。 ワーカー・ノードが長時間この状態になっている場合は、次のステップに進み、プロビジョニング中に問題が発生したかどうかを調べてください。</td>
      </tr>
      <tr>
        <td>Provision_failed</td>
        <td>ワーカー・ノードをプロビジョンできませんでした。 次のステップに進んで、障害に関する詳細を調べてください。</td>
      </tr>
   <tr>
        <td>Reloading</td>
        <td>ワーカー・ノードは再ロード中であるため、クラスターでは使用できません。 CLI 出力の <strong>Status</strong> 列で、再ロードのプロセスをモニターできます。 ワーカー・ノードが長時間この状態になっている場合は、次のステップに進み、再ロード中に問題が発生したかどうかを調べてください。</td>
       </tr>
       <tr>
        <td>Reloading_failed </td>
        <td>ワーカー・ノードを再ロードできませんでした。 次のステップに進んで、障害に関する詳細を調べてください。</td>
      </tr>
      <tr>
        <td>Reload_pending </td>
        <td>ワーカー・ノードの Kubernetes バージョンを再ロードまたは更新する要求が送信されました。 ワーカー・ノードが再ロードされると、状態は「<code>Reloading</code>」に変わります。 </td>
      </tr>
      <tr>
       <td>Unknown</td>
       <td>次のいずれかの理由で、Kubernetes マスターにアクセスできません。<ul><li>Kubernetes マスターの更新を要求しました。 更新中は、ワーカー・ノードの状態を取得できません。</li><li>ワーカー・ノードを保護している別のファイアウォールが存在するか、最近ファイアウォールの設定を変更した可能性があります。{{site.data.keyword.containershort_notm}} では、ワーカー・ノードと Kubernetes マスター間で通信を行うには、特定の IP アドレスとポートが開いている必要があります。 詳しくは、[ファイアウォールがあるためにワーカー・ノードが接続しない](cs_troubleshoot_clusters.html#cs_firewall)を参照してください。</li><li>Kubernetes マスターがダウンしています。 [{{site.data.keyword.Bluemix_notm}} サポート・チケット](#ts_getting_help)を開いて、{{site.data.keyword.Bluemix_notm}} サポートに連絡してください。</li></ul></td>
  </tr>
     <tr>
        <td>Warning</td>
        <td>ワーカー・ノードが、メモリーまたはディスク・スペースの限度に達しています。 ワーカー・ノードのワークロードを減らすか、またはワークロードを分散できるようにクラスターにワーカー・ノードを追加してください。</td>
  </tr>
    </tbody>
  </table>

5.  ワーカー・ノードの詳細情報をリストします。 詳細情報にエラー・メッセージが含まれている場合は、[ワーカー・ノードに関する一般的なエラー・メッセージ](#common_worker_nodes_issues)のリストを参照して、問題の解決方法を確認してください。

   ```
   bx cs worker-get <worker_id>
   ```
   {: pre}

  ```
  bx cs worker-get [<cluster_name_or_id>] <worker_node_id>
  ```
  {: pre}

<br />


## ワーカー・ノードに関する一般的な問題
{: #common_worker_nodes_issues}

一般的なエラー・メッセージを確認し、解決方法を調べます。

  <table>
  <caption>一般的なエラー・メッセージ</caption>
    <thead>
    <th>エラー・メッセージ</th>
    <th>説明と解決
    </thead>
    <tbody>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} Infrastructure Exception: Your account is currently prohibited from ordering 'Computing Instances'.</td>
        <td>ご使用の IBM Cloud インフラストラクチャー (SoftLayer) アカウントは、コンピュート・リソースの注文を制限されている可能性があります。 [{{site.data.keyword.Bluemix_notm}} サポート・チケット](#ts_getting_help)を開いて、{{site.data.keyword.Bluemix_notm}} サポートに連絡してください。</td>
      </tr>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} Infrastructure Exception: Could not place order. There are insufficient resources behind router 'router_name' to fulfill the request for the following guests: 'worker_id'.</td>
        <td>選択した VLAN に関連付けられているデータ・センター内のポッドのスペースが不足しているため、ワーカー・ノードをプロビジョンできません。 以下の選択肢があります。<ul><li>別のデータ・センターを使用してワーカー・ノードをプロビジョンします。 使用可能なデータ・センターをリストするには、<code>bx cs locations</code> を実行します。<li>データ・センター内の別のポッドに関連付けられているパブリック VLAN とプライベート VLAN の既存のペアがある場合は、代わりにその VLAN ペアを使用します。<li>[{{site.data.keyword.Bluemix_notm}} サポート・チケット](#ts_getting_help)を開いて、{{site.data.keyword.Bluemix_notm}} サポートに連絡してください。</ul></td>
      </tr>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} Infrastructure Exception: Could not obtain network VLAN with ID: &lt;vlan id&gt;.</td>
        <td>次のいずれかの理由で、選択した VLAN ID が見つからなかったため、ワーカー・ノードをプロビジョンできませんでした。<ul><li>VLAN ID ではなく VLAN 番号を指定した可能性があります。 VLAN 番号の長さは 3 桁または 4 桁ですが、VLAN ID の長さは 7 桁です。 VLAN ID を取得するには、<code>bx cs vlans &lt;location&gt;</code> を実行してください。<li>ご使用の IBM Cloud インフラストラクチャー (SoftLayer) アカウントに VLAN ID が関連付けられていない可能性があります。 アカウントの使用可能な VLAN ID をリストするには、<code>bx cs vlans &lt;location&gt;</code> を実行します。 IBM Cloud インフラストラクチャー (SoftLayer) アカウントを変更するには、[`bx cs credentials-set`](cs_cli_reference.html#cs_credentials_set) を参照してください。 </ul></td>
      </tr>
      <tr>
        <td>SoftLayer_Exception_Order_InvalidLocation: The location provided for this order is invalid. (HTTP 500)</td>
        <td>ご使用の IBM Cloud インフラストラクチャー (SoftLayer) は、選択したデータ・センター内のコンピュート・リソースを注文するようにセットアップされていません。 [{{site.data.keyword.Bluemix_notm}} サポート](#ts_getting_help)に問い合わせて、アカウントが正しくセットアップされているか確認してください。</td>
       </tr>
       <tr>
        <td>{{site.data.keyword.Bluemix_notm}} Infrastructure Exception: The user does not have the necessary {{site.data.keyword.Bluemix_notm}} Infrastructure permissions to add servers
        </br></br>
        {{site.data.keyword.Bluemix_notm}} Infrastructure Exception: 'Item' must be ordered with permission.</td>
        <td>IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオからワーカー・ノードをプロビジョンするために必要なアクセス権限がない可能性があります。 [IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオへのアクセス権限を構成して標準の Kubernetes クラスターを作成する](cs_troubleshoot_clusters.html#cs_credentials)を参照してください。</td>
      </tr>
      <tr>
       <td>Worker unable to talk to {{site.data.keyword.containershort_notm}} servers. Please verify your firewall setup is allowing traffic from this worker.
       <td><ul><li>ファイアウォールがある場合は、[該当するポートと IP アドレスへの発信トラフィックを許可するようにファイアウォール設定を構成します](cs_firewall.html#firewall_outbound)。</li><li>`bx cs workers<mycluster>` を実行して、クラスターにパブリック IP が含まれていないかどうかを確認します。パブリック IP がリストされない場合、クラスターにはプライベート VLAN だけがあります。<ul><li>クラスターにプライベート VLAN のみが含まれるようにするには、[VLAN 接続](cs_clusters.html#worker_vlan_connection)と[ファイアウォール](cs_firewall.html#firewall_outbound)をセットアップします。</li><li>パブリック IP があるクラスターにするには、パブリック VLAN とプライベート VLAN の両方を指定して[新しいワーカー・ノードを追加](cs_cli_reference.html#cs_worker_add)します。</li></ul></li></ul></td>
     </tr>
      <tr>
  <td>Cannot create IMS portal token, as no IMS account is linked to the selected BSS account</br></br>Provided user not found or active</br></br>SoftLayer_Exception_User_Customer_InvalidUserStatus: User account is currently cancel_pending.</br></br>Waiting for machine to be visible to the user</td>
  <td>IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオへのアクセスに使用される API キーの所有者には、このアクションを実行するために必要な権限がありません。あるいは、その所有者が削除の保留中である可能性があります。</br></br><strong>ユーザーは</strong>、以下の手順に従ってください。 <ol><li>複数のアカウントを利用できる場合は、{{site.data.keyword.containerlong_notm}} を操作したいアカウントにログインしていることを確認します。 </li><li><code>bx cs api-key-info</code> を実行して、IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオへのアクセスに使用される現在の API キーの所有者を表示します。 </li><li><code>bx account list</code> を実行して、現在使用している {{site.data.keyword.Bluemix_notm}} アカウントの所有者を表示します。 </li><li>{{site.data.keyword.Bluemix_notm}} アカウントの所有者に連絡して、API キーの所有者に IBM Cloud インフラストラクチャー (SoftLayer) での十分な権限がないか、その所有者が削除の保留中である可能性があることを報告します。</li></ol></br><strong>アカウント所有者は</strong>、以下の手順に従ってください。 <ol><li>先ほど失敗したアクションを実行するために [IBM Cloud インフラストラクチャー (SoftLayer) に必要な権限](cs_users.html#infra_access)を確認します。 </li><li>API キーの所有者の権限を修正するか、[<code>bx cs api-key-reset</code>](cs_cli_reference.html#cs_api_key_reset) コマンドを使用して新しい API キーを作成します。 </li><li>自分または別のアカウント管理者が、IBM Cloud インフラストラクチャー (SoftLayer) の資格情報を手動でアカウントに設定した場合は、[<code>bx cs credentials-unset</code>](cs_cli_reference.html#cs_credentials_unset) を実行してアカウントから資格情報を削除します。</li></ol></td>
  </tr>
    </tbody>
  </table>



<br />




## アプリ・デプロイメントのデバッグ
{: #debug_apps}

アプリ・デプロイメントをデバッグするためのオプションを確認し、障害の根本原因を探します。

1. `describe` コマンドを実行して、サービス・リソースまたはデプロイメント・リソース内の異常を見つけます。

 例:
 <pre class="pre"><code>kubectl describe service &lt;service_name&gt; </code></pre>

2. [コンテナーが ContainerCreating 状態で停滞しているかどうかを確認します](cs_troubleshoot_storage.html#stuck_creating_state)。

3. クラスターが `Critical` 状態かどうかを確認します。 クラスターが `Critical` 状態の場合、ファイアウォール・ルールを調べて、マスターがワーカー・ノードと通信できることを検査します。

4. サービスが正しいポートで listen していることを確認します。
   1. ポッドの名前を取得します。
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. コンテナーにログインします。
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   3. コンテナー内からアプリに対して curl を実行します。 ポートにアクセスできない場合、サービスは正しいポートで listen していないか、またはアプリに問題が生じている可能性があります。 正しいポートを指定してサービスの構成ファイルを更新して再デプロイするか、またはアプリに存在する可能性がある問題を調査します。
     <pre class="pre"><code>curl localhost: &lt;port&gt;</code></pre>

5. サービスがポッドに正しくリンクされていることを確認します。
   1. ポッドの名前を取得します。
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. コンテナーにログインします。
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   3. サービスのクラスター IP アドレスとポートに対して curl を実行します。 IP アドレスとポートにアクセスできない場合、サービスのエンドポイントを調べます。 エンドポイントがリストされない場合は、サービスのセレクターがポッドと一致していません。エンドポイントがリストされている場合は、サービスの宛先ポート・フィールドを調べて、宛先ポートがポッドに使用されているポートと同じであることを確認します。
     <pre class="pre"><code>curl &lt;cluster_IP&gt;:&lt;port&gt;</code></pre>

6. Ingress サービスの場合、クラスター内からサービスにアクセスできることを検証します。
   1. ポッドの名前を取得します。
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. コンテナーにログインします。
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   2. Ingress サービス用に指定された URL に対して curl を実行します。 URL にアクセスできない場合、クラスターと外部エンドポイントの間にファイアウォールの問題が生じていないかを調べます。 
     <pre class="pre"><code>curl &lt;host_name&gt;.&lt;domain&gt;</code></pre>

<br />



## ヘルプとサポートの取得
{: #ts_getting_help}

まだクラスターに問題がありますか?
{: shortdesc}

-   {{site.data.keyword.Bluemix_notm}} が使用可能かどうかを確認するために、[{{site.data.keyword.Bluemix_notm}} 状況ページを確認します![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.ibm.com/bluemix/support/#status)。
-   [{{site.data.keyword.containershort_notm}} Slack ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://ibm-container-service.slack.com) に質問を投稿します。

    {{site.data.keyword.Bluemix_notm}} アカウントに IBM ID を使用していない場合は、この Slack への[招待を要求](https://bxcs-slack-invite.mybluemix.net/)してください。
    {: tip}
-   フォーラムを確認して、同じ問題が他のユーザーで起こっているかどうかを調べます。 フォーラムを使用して質問するときは、{{site.data.keyword.Bluemix_notm}} 開発チームの目に止まるように、質問にタグを付けてください。

    -   {{site.data.keyword.containershort_notm}} を使用したクラスターまたはアプリの開発やデプロイに関する技術的な質問がある場合は、[Stack Overflow![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) に質問を投稿し、`ibm-cloud`、`kubernetes`、`containers` のタグを付けてください。
    -   サービスや概説の説明について質問がある場合は、[IBM developerWorks dW Answers ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) フォーラムを使用してください。 `ibm-cloud` と `containers` のタグを含めてください。
    フォーラムの使用について詳しくは、[ヘルプの取得](/docs/get-support/howtogetsupport.html#using-avatar)を参照してください。

-   チケットを開いて、IBM サポートに連絡してください。 IBM サポート・チケットを開く方法や、サポート・レベルとチケットの重大度については、[サポートへのお問い合わせ](/docs/get-support/howtogetsupport.html#getting-customer-support)を参照してください。

{: tip}
問題を報告する際に、クラスター ID も報告してください。 クラスター ID を取得するには、`bx cs clusters` を実行します。

