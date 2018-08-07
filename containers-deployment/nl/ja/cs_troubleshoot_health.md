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



# ロギングとモニタリングのトラブルシューティング
{: #cs_troubleshoot_health}

{{site.data.keyword.containerlong}} を使用する際は、ここに示すロギングとモニタリングの問題のトラブルシューティング手法を検討してください。
{: shortdesc}

より一般的な問題が起きている場合は、[クラスターのデバッグ](cs_troubleshoot.html)を試してください。
{: tip}

## ログが表示されない
{: #cs_no_logs}

{: tsSymptoms}
Kibana ダッシュボードにアクセスしてもログが表示されません。

{: tsResolve}
クラスター・ログが表示されない理由と、対応するトラブルシューティング手順について以下に説明します。

<table>
<caption>表示されないログのトラブルシューティング</caption>
  <col width="40%">
  <col width="60%">
  <thead>
    <tr>
      <th>現象の理由</th>
      <th>修正方法</th>
    </tr>
 </thead>
 <tbody>
  <tr>
    <td>ロギング構成がセットアップされていない。</td>
    <td>ログが送信されるようにするには、ロギング構成を作成する必要があります。 そのためには、<a href="cs_health.html#logging">クラスター・ロギングの構成</a>を参照してください。</td>
  </tr>
  <tr>
    <td>クラスターが <code>Normal</code> 状態ではない。</td>
    <td>クラスターの状態を確認する方法については、<a href="cs_troubleshoot.html#debug_clusters">クラスターのデバッグ</a>を参照してください。</td>
  </tr>
  <tr>
    <td>ログ・ストレージの割り当て量に達している。</td>
    <td>ログ・ストレージの限度を増やす方法については、<a href="/docs/services/CloudLogAnalysis/troubleshooting/error_msgs.html">{{site.data.keyword.loganalysislong_notm}} の資料</a>を参照してください。</td>
  </tr>
  <tr>
    <td>クラスター作成の際にスペースを指定した場合、アカウント所有者に、そのスペースに対する管理者、開発者、または監査員の権限がない。</td>
      <td>アカウント所有者のアクセス許可を変更するには、以下のようにします。
      <ol><li>クラスターのアカウント所有者を見つけるために、<code>bx cs api-key-info &lt;cluster_name_or_ID&gt;</code> を実行します。</li>
      <li>そのアカウント所有者にスペースに対する管理者、開発者、または監査員の {{site.data.keyword.containershort_notm}} アクセス許可を付与する方法については、<a href="cs_users.html">クラスター・アクセス権限の管理</a>を参照してください。</li>
      <li>許可が変更された後にロギング・トークンをリフレッシュするには、<code>bx cs logging-config-refresh &lt;cluster_name_or_ID&gt;</code> を実行します。</li></ol></td>
    </tr>
    <tr>
      <td>アプリケーション・ロギング構成でアプリのパスにシンボリック・リンクが使用されている。</td>
      <td><p>ログが送信されるようにするには、ロギング構成で絶対パスを使用する必要があります。そうしないと、ログを読み取れません。 パスがワーカー・ノードにマウントされている場合は、シンボリック・リンクが作成されている可能性があります。</p> <p>例: 指定されたパスが <code>/usr/local/<b>spark</b>/work/app-0546/0/stderr</code> であるのに、ログが <code>/usr/local/<b>spark-1.0-hadoop-1.2</b>/work/app-0546/0/stderr</code> に送信されている場合、ログは読み取れません。</p></td>
    </tr>
  </tbody>
</table>

トラブルシューティング中に行った変更をテストするには、複数のログ・イベントを生成するサンプルのポッド *Noisy* をクラスター内のワーカー・ノードにデプロイすることができます。

  1. クラスター上のログの生成を開始する場所に [CLI のターゲット](cs_cli_install.html#cs_cli_configure)を設定します。

  2. `deploy-noisy.yaml` 構成ファイルを作成します。

      ```
      apiVersion: v1
      kind: Pod
      metadata:
        name: noisy
      spec:
        containers:
        - name: noisy
          image: ubuntu:16.04
          command: ["/bin/sh"]
          args: ["-c", "while true; do sleep 10; echo 'Hello world!'; done"]
          imagePullPolicy: "Always"
        ```
        {: codeblock}

  3. クラスターのコンテキストで構成ファイルを実行します。

        ```
        kubectl apply -f noisy.yaml
        ```
        {:pre}

  4. 数分後に、Kibana ダッシュボードにログが表示されます。 Kibana ダッシュボードにアクセスするには、以下のいずれかの URL にアクセスし、クラスターを作成した {{site.data.keyword.Bluemix_notm}} アカウントを選択します。 クラスター作成の際にスペースを指定した場合は、代わりにそのスペースに移動します。
      - 米国南部および米国東部: https://logging.ng.bluemix.net
      - 英国南部: https://logging.eu-gb.bluemix.net
      - EU 中央: https://logging.eu-fra.bluemix.net
      - 南アジア太平洋地域: https://logging.au-syd.bluemix.net

<br />


## Kubernetes ダッシュボードに使用状況グラフが表示されない
{: #cs_dashboard_graphs}

{: tsSymptoms}
Kubernetes ダッシュボードにアクセスするとき、使用状況グラフが表示されません。

{: tsCauses}
クラスターの更新やワーカー・ノードのリブートの後に、`kube-dashboard` ポッドが更新されないことがあります。

{: tsResolve}
`kube-dashboard` ポッドを削除して、強制的に再始動してください。 ポッドは RBAC ポリシーを使用して再作成され、heapster にアクセスして使用状況情報を取得します。

  ```
  kubectl delete pod -n kube-system $(kubectl get pod -n kube-system --selector=k8s-app=kubernetes-dashboard -o jsonpath='{.items..metadata.name}')
  ```
  {: pre}

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

