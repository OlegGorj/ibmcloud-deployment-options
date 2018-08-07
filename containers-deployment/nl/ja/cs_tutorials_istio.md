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



# チュートリアル: {{site.data.keyword.containerlong_notm}} での Istio のインストール
{: #istio_tutorial}

[Istio](https://www.ibm.com/cloud/info/istio) は、{{site.data.keyword.containerlong}} の Kubernetes のようなクラウド・プラットフォームでマイクロサービスのネットワーク (サービス・メッシュともいう) を接続、保護、管理するためのオープン・プラットフォームです。 Istio を使用すると、ネットワーク・トラフィックの管理、マイクロサービス間のロード・バランシング、アクセス・ポリシーの実施、サービス ID の検証などを行うことができます。
{:shortdesc}

このチュートリアルでは、BookInfo と呼ばれる単純な演習用ブックストア・アプリ用に、Istio と一緒に 4 つのマイクロサービスをインストールする方法を確認できます。 このマイクロサービスには、製品 Web ページ、本の詳細情報、レビュー、評価が含まれます。 Istio をインストールする {{site.data.keyword.containershort}} クラスターに BookInfo のマイクロサービスをデプロイする際には、各マイクロサービスのポッド内に Istio Envoy サイドカー・プロキシーを挿入します。

**注**: Istio プラットフォームの一部の構成とフィーチャーはまだ開発途上であるため、ユーザーのフィードバックに基づいて変更される可能性があります。 Istio を実動使用する前に、安定するのに 2、3 カ月の期間がかかることを見込んでください。 

## 達成目標

-   クラスターに Istio をダウンロードしてインストールします
-   BookInfo サンプル・アプリをデプロイします
-   Envoy サイドカー・プロキシーをアプリの 4 つのマイクロサービスのポッド内に挿入して、サービス・メッシュ内のマイクロサービスを接続します
-   BookInfo アプリのデプロイメントを確認し、3 つのバージョンの評価サービス間でラウンドロビンします

## 所要時間

30 分

## 対象読者

このチュートリアルは、Istio を初めて使用するソフトウェア開発者やネットワーク管理者を対象にしています。

## 前提条件

-  [CLI をインストールします](cs_cli_install.html#cs_cli_install_steps)。 Istio には Kubernetes バージョン 1.9 以上が必要です。必ず、ご使用のクラスターの Kubernetes バージョンに一致する `kubectl` CLI バージョンをインストールしてください。
-  バージョン 1.9 以上の Kubernetes で[クラスターを作成します](cs_clusters.html#clusters_cli)。
-  [CLI のターゲットを自分のクラスターに設定します](cs_cli_install.html#cs_cli_configure)。

## レッスン 1: Istio をダウンロードしてインストールする
{: #istio_tutorial1}

クラスターに Istio をダウンロードしてインストールします。
{:shortdesc}

1. Istio を [https://github.com/istio/istio/releases ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/istio/istio/releases) から直接ダウンロードするか、以下のように curl を使用して最新バージョンを取得します。

   ```
   curl -L https://git.io/getLatestIstio | sh -
   ```
   {: pre}

2. インストール・ファイルを解凍します。

3. `istioctl` クライアントを PATH に追加します。 例えば、MacOS または Linux システムでは以下のコマンドを実行します。

   ```
   export PATH=$PWD/istio-0.4.0/bin:$PATH
   ```
   {: pre}

4. ディレクトリーを Istio ファイルの場所に切り替えます。

   ```
   cd filepath/istio-0.4.0
   ```
   {: pre}

5. Kubernetes クラスター上に Istio をインストールします。 Istio を Kubernetes 名前空間 `istio-system` 内にデプロイします。

   ```
   kubectl apply -f install/kubernetes/istio.yaml
   ```
   {: pre}

   **注**: サイドカー間の相互 TLS 認証を有効にする必要がある場合は、`kubectl apply -f install/kubernetes/istio-auth.yaml` を実行する代わりに `istio-auth` ファイルをインストールできます

6. Kubernetes サービス `istio-pilot`、`istio-mixer`、`istio-ingress` が完全にデプロイされていることを確認してから続行します。

   ```
   kubectl get svc -n istio-system
   ```
   {: pre}

   ```
   NAME            TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)                                                            AGE
   istio-ingress   LoadBalancer   172.21.xxx.xxx   169.xx.xxx.xxx   80:31176/TCP,443:30288/TCP                                         2m
   istio-mixer     ClusterIP      172.21.xxx.xxx     <none>           9091/TCP,15004/TCP,9093/TCP,9094/TCP,9102/TCP,9125/UDP,42422/TCP   2m
   istio-pilot     ClusterIP      172.21.xxx.xxx    <none>           15003/TCP,443/TCP                                                  2m
   ```
   {: screen}

7. 対応するポッド `istio-pilot-*`、`istio-mixer-*`、`istio-ingress-*`、`istio-ca-*` も完全にデプロイされていることを確認してから続行します。

   ```
   kubectl get pods -n istio-system
   ```
   {: pre}

   ```
   istio-ca-3657790228-j21b9           1/1       Running   0          5m
   istio-ingress-1842462111-j3vcs      1/1       Running   0          5m
   istio-pilot-2275554717-93c43        1/1       Running   0          5m
   istio-mixer-2104784889-20rm8        2/2       Running   0          5m
   ```
   {: screen}


おつかれさまでした。 クラスターに Istio を正常にインストールしました。 次に、BookInfo サンプル・アプリをクラスター内にデプロイします。


## レッスン 2: BookInfo アプリをデプロイする
{: #istio_tutorial2}

BookInfo サンプル・アプリのマイクロサービスを Kubernetes クラスターにデプロイします。
{:shortdesc}

これらの 4 つのマイクロサービスには、製品 Web ページ、本の詳細情報、レビュー (レビューのマイクロサービスには複数のバージョンがあります)、評価が含まれます。 この例で使用されるすべてのファイルは、Istio インストール済み環境の `samples/bookinfo` ディレクトリーにあります。

BookInfo をデプロイする際には、Envoy サイドカー・プロキシーがコンテナーとしてアプリのマイクロサービスのポッド内に挿入された後に、マイクロサービスのポッドがデプロイされます。 Istio は拡張バージョンの Envoy プロキシーを使用して、サービス・メッシュ内のすべてのマイクロサービスに関するすべてのインバウンド・トラフィックとアウトバウンド・トラフィックを仲介します。 Envoy について詳しくは、[Istio の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://istio.io/docs/concepts/what-is-istio/overview.html#envoy) を参照してください。

1. BookInfo アプリをデプロイします。 `kube-inject` コマンドは、Envoy を `bookinfo.yaml` ファイルに追加し、この更新ファイルを使用してアプリをデプロイします。 アプリのマイクロサービスをデプロイする際に、Envoy サイドカーも各マイクロサービス・ポッド内にデプロイされます。

   ```
   kubectl apply -f <(istioctl kube-inject -f samples/bookinfo/kube/bookinfo.yaml)
   ```
   {: pre}

2. マイクロサービスとその対応するポッドがデプロイされていることを確認します。

   ```
   kubectl get svc
   ```
   {: pre}

   ```
   NAME                       CLUSTER-IP   EXTERNAL-IP   PORT(S)              AGE
   details                    10.xxx.xx.xxx    <none>        9080/TCP             6m
   kubernetes                 10.xxx.xx.xxx     <none>        443/TCP              30m
   productpage                10.xxx.xx.xxx   <none>        9080/TCP             6m
   ratings                    10.xxx.xx.xxx    <none>        9080/TCP             6m
   reviews                    10.xxx.xx.xxx   <none>        9080/TCP             6m
   ```
   {: screen}

   ```
   kubectl get pods
   ```
   {: pre}

   ```
   NAME                                        READY     STATUS    RESTARTS   AGE
   details-v1-1520924117-48z17                 2/2       Running   0          6m
   productpage-v1-560495357-jk1lz              2/2       Running   0          6m
   ratings-v1-734492171-rnr5l                  2/2       Running   0          6m
   reviews-v1-874083890-f0qf0                  2/2       Running   0          6m
   reviews-v2-1343845940-b34q5                 2/2       Running   0          6m
   reviews-v3-1813607990-8ch52                 2/2       Running   0          6m
   ```
   {: screen}

3. アプリケーション・デプロイメントを確認するには、クラスターのパブリック・アドレスを取得します。

    * 標準クラスターで作業している場合は、以下のコマンドを実行して、クラスターの Ingress IP とポートを取得します。

       ```
       kubectl get ingress
       ```
       {: pre}

       出力例:

       ```
       NAME      HOSTS     ADDRESS          PORTS     AGE
       gateway   *         169.xx.xxx.xxx   80        3m
       ```
       {: screen}

       この例では、結果の Ingress アドレスは `169.48.221.218:80` になります。 以下のコマンドを使用して、このアドレスをゲートウェイ URL としてエクスポートします。 次のステップで、このゲートウェイ URL を使用して BookInfo 製品ページにアクセスすることになります。

       ```
       export GATEWAY_URL=169.xx.xxx.xxx:80
       ```
       {: pre}

    * フリー・クラスターで作業している場合は、ワーカー・ノードのパブリック IP と NodePort を使用する必要があります。 以下のコマンドを実行して、ワーカー・ノードのパブリック IP を取得します。

       ```
       bx cs workers <cluster_name_or_ID>
       ```
       {: pre}

       以下のコマンドを使用して、ワーカー・ノードのパブリック IP をゲートウェイ URL としてエクスポートします。 次のステップで、このゲートウェイ URL を使用して BookInfo 製品ページにアクセスすることになります。

       ```
       export GATEWAY_URL=<worker_node_public_IP>:$(kubectl get svc istio-ingress -n istio-system -o jsonpath='{.spec.ports[0].nodePort}')
       ```
       {: pre}

4. `GATEWAY_URL` 変数に対して curl を実行し、BookInfo が稼働中であることを確認します。 応答 `200` は、Istio で BookInfo が適切に稼働していることを意味します。

   ```
   curl -I http://$GATEWAY_URL/productpage
   ```
   {: pre}

5. ブラウザーで、`http://$GATEWAY_URL/productpage` に移動し、BookInfo の Web ページを表示します。

6. ページの最新表示を複数回試行します。 さまざまなバージョンのレビュー・セクションが、赤い星形、黒い星形、星形なしの間でラウンドロビンします。

おつかれさまでした。 BookInfo サンプル・アプリと Istio Envoy サイドカーを正常にデプロイしました。 次は、リソースをクリーンアップします。または、他のチュートリアルを引き続き試し、Istio についてさらに詳しく知ることもできます。

## クリーンアップ
{: #istio_tutorial_cleanup}

Istio での作業を終了し、[次の作業](#istio_tutorial_whatsnext)に進まない場合は、クラスター内の Istio リソースをクリーンアップできます。
{:shortdesc}

1. クラスター内の BookInfo サービス、ポッド、デプロイメントをすべて削除します。

   ```
   samples/bookinfo/kube/cleanup.sh
   ```
   {: pre}

2. Istio をアンインストールします。

   ```
   kubectl delete -f install/kubernetes/istio.yaml
   ```
   {: pre}

## 次の作業
{: #istio_tutorial_whatsnext}

Istio についてさらに詳しく知るには、[Istio の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://istio.io/) でその他のガイドを参照できます。

* [Intelligent Routing ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://istio.io/docs/guides/intelligent-routing.html): この例では、Istio のトラフィック管理機能を使用して、特定のバージョンの BookInfo のレビューと評価のマイクロサービスにトラフィックをルーティングする方法を示しています。

* [In-Depth Telemetry ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://istio.io/docs/guides/telemetry.html): この例では、Istio Mixer と Envoy プロキシーを使用して、BookInfo のマイクロサービス間で統一されたメトリック、ログ、トレースを取得する方法を示しています。
