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




# サービスの統合
{: #integrations}

{{site.data.keyword.containerlong}} の標準の Kubernetes クラスターでは、さまざまな外部サービスとカタログ・サービスを使用できます。
{:shortdesc}


## アプリケーション・サービス
<table summary="アクセシビリティーについての要約">
<caption>アプリケーション・サービス</caption>
<thead>
<tr>
<th>サービス</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.blockchainfull}}</td>
<td>{{site.data.keyword.containerlong_notm}} の Kubernetes クラスターに、だれでも利用できる IBM Blockchain 開発環境をデプロイします。 この環境を使用して、独自のブロックチェーン・ネットワークを開発してカスタマイズし、トランザクションの履歴を記録するために変更不可能な台帳を共有するアプリをデプロイできます。 詳しくは、<a href="https://ibm-blockchain.github.io" target="_blank">Develop in a cloud sandbox
IBM Blockchain Platform <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。 </td>
</tr>
</tbody>
</table>

<br />



## DevOps サービス
<table summary="アクセシビリティーについての要約">
<caption>DevOps サービス</caption>
<thead>
<tr>
<th>サービス</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Codeship</td>
<td>コンテナーの継続的な統合とデリバリーのために <a href="https://codeship.com" target="_blank">Codeship <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を使用できます。 詳しくは、<a href="https://www.ibm.com/blogs/bluemix/2017/10/using-codeship-pro-deploy-workloads-ibm-container-service/" target="_blank">Using Codeship Pro To Deploy Workloads to {{site.data.keyword.containershort_notm}} <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。 </td>
</tr>
<tr>
<td>Helm</td>
<td> <a href="https://helm.sh/" target="_blank">Helm <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> は Kubernetes パッケージ・マネージャーです。 新しい Helm チャートを作成するか、既存の Helm チャートを使用して、{{site.data.keyword.containerlong_notm}} クラスターで実行される複雑な Kubernetes アプリケーションの定義、インストール、アップグレードを行うことができます。 <p>詳しくは、[{{site.data.keyword.containershort_notm}} での Helm のセットアップ](cs_integrations.html#helm)を参照してください。</p></td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>アプリのビルドと Kubernetes クラスターへのコンテナーのデプロイメントを、ツールチェーンを使用して自動化します。 セットアップ情報については、<a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Deploy Kubernetes pods to the {{site.data.keyword.containerlong_notm}} using DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> というブログを参照してください。 </td>
</tr>
<tr>
<td>Istio</td>
<td><a href="https://www.ibm.com/cloud/info/istio" target="_blank">Istio <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> は 、Kubernetes などのクラウド・オーケストレーション・プラットフォームでマイクロサービス・ネットワーク (別名、サービス・メッシュ) の接続、保護、管理、モニターを行うための方法を開発者に提供するオープン・ソース・サービスです。 このオープン・ソース・プロジェクトの詳細については、<a href="https://developer.ibm.com/dwblog/2017/istio/" target="_blank">IBM が Istio を共同創設して立ち上げた経過 <img src="../icons/launch-glyph.svg" alt=" 外部リンク・アイコン"></a> に関するブログ投稿を参照してください。 {{site.data.keyword.containershort_notm}} の Kubernetes クラスターに Istio をインストールしてサンプル・アプリの使用を開始するには、[Istio を使用したマイクロサービスの管理に関するチュートリアル](cs_tutorials_istio.html#istio_tutorial)を参照してください。</td>
</tr>
</tbody>
</table>

<br />



## サービスのロギングとモニタリング
<table summary="アクセシビリティーについての要約">
<caption>サービスのロギングとモニタリング</caption>
<thead>
<tr>
<th>サービス</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
<td>CoScale</td>
<td><a href="https://www.coscale.com/" target="_blank">CoScale <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を使用して、ワーカー・ノード、コンテナー、レプリカ・セット、レプリケーション・コントローラー、サービスをモニターします。 詳しくは、<a href="https://www.ibm.com/blogs/bluemix/2017/06/monitoring-ibm-bluemix-container-service-coscale/" target="_blank">Monitoring {{site.data.keyword.containershort_notm}} with CoScale <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。 </td>
</tr>
<tr>
<td>Datadog</td>
<td><a href="https://www.datadoghq.com/" target="_blank">Datadog <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を使用して、クラスターをモニターし、インフラストラクチャーとアプリケーションのパフォーマンス・メトリックを表示します。 詳しくは、<a href="https://www.ibm.com/blogs/bluemix/2017/07/monitoring-ibm-bluemix-container-service-datadog/" target="_blank">Monitoring {{site.data.keyword.containershort_notm}} with Datadog <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。 </td>
</tr>
<tr>
<td>{{site.data.keyword.loganalysisfull}}</td>
<td>{{site.data.keyword.loganalysisfull_notm}} を使用して、ログの収集、保存、検索の機能を拡張します。 詳しくは、<a href="../services/CloudLogAnalysis/containers/containers_kube_other_logs.html" target="_blank">クラスター・ログの自動収集の有効化 <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。 </td>
</tr>
<tr>
<td>{{site.data.keyword.monitoringlong}}</td>
<td>{{site.data.keyword.monitoringlong_notm}} を使用してルールとアラートを定義して、メトリックの収集と保存の機能を拡張します。 詳しくは、<a href="../services/cloud-monitoring/tutorials/container_service_metrics.html" target="_blank">Kubernetes クラスターにデプロイされたアプリに関する Grafana でのメトリックの分析 <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。 </td>
</tr>
<tr>
<td>Instana</td>
<td> <a href="https://www.instana.com/" target="_blank">Instana <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> は、アプリを自動的に検出してマップする GUI を使用して、インフラストラクチャーとアプリのパフォーマンス・モニターを提供します。 Istana はアプリに対するすべての要求をキャプチャーするので、その情報を使用してトラブルシューティングと根本原因分析を行い、問題の再発を防げるようになります。詳しくは、<a href="https://www.instana.com/blog/precise-visibility-applications-ibm-bluemix-container-service/" target="_blank">{{site.data.keyword.containershort_notm}} での Istana のデプロイ <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> に関するブログ投稿を参照してください。</td>
</tr>
<tr>
<td>Prometheus</td>
<td>Prometheus は、特に Kubernetes のために設計された、モニタリング、ロギング、アラートのためのオープン・ソース・ツールです。Prometheus は、Kubernetes のロギング情報に基づいて、クラスター、ワーカー・ノード、デプロイメントの正常性に関する詳細情報を取得します。クラスターで実行されているコンテナーごとに、CPU、メモリー、I/O、ネットワーク・アクティビティーの情報が収集されます。収集されたデータをカスタム・クエリーやアラートで使用して、クラスター内のパフォーマンスとワークロードをモニターできます。

<p>Prometheus を使用する場合は、<a href="https://github.com/coreos/prometheus-operator/tree/master/contrib/kube-prometheus" target="_blank">CoreOS の説明 <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> に従ってください。</p>
</td>
</tr>
<tr>
<td>Sematext</td>
<td><a href="https://sematext.com/" target="_blank">Sematext <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を使用して、コンテナー化アプリケーションのメトリックとログを表示します。 詳しくは、<a href="https://www.ibm.com/blogs/bluemix/2017/09/monitoring-logging-ibm-bluemix-container-service-sematext/" target="_blank">Monitoring and logging for containers with Sematext <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。</td>
</tr>
<tr>
<td>Sysdig</td>
<td><a href="https://sysdig.com/" target="_blank">Sysdig <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を使用して、単一の計測ポイントでアプリ、コンテナー、statsd、ホストのメトリックをキャプチャーします。 詳しくは、<a href="https://www.ibm.com/blogs/bluemix/2017/08/monitoring-ibm-bluemix-container-service-sysdig-container-intelligence/" target="_blank">Monitoring {{site.data.keyword.containershort_notm}} with Sysdig Container Intelligence <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。 </td>
</tr>
<tr>
<td>Weave Scope</td>
<td>Weave Scope は、Kubernetes クラスター内のリソース (サービス、ポッド、コンテナー、プロセス、ノードなど) のビジュアル図を表示します。 Weave Scope は CPU とメモリーのインタラクティブ・メトリックを示し、コンテナーの中で追尾したり実行したりするツールも備えています。<p>詳しくは、[Weave Scope と {{site.data.keyword.containershort_notm}} での Kubernetes クラスター・リソースの視覚化](cs_integrations.html#weavescope)を参照してください。</p></li></ol>
</td>
</tr>
</tbody>
</table>

<br />



## セキュリティー・サービス
<table summary="アクセシビリティーについての要約">
<caption>セキュリティー・サービス</caption>
<thead>
<tr>
<th>サービス</th>
<th>説明</th>
</tr>
</thead>
<tbody>
  <tr id="appid">
    <td>{{site.data.keyword.appid_full}}</td>
    <td>ユーザーに対してサインインを要求することにより、[{{site.data.keyword.appid_short}}](/docs/services/appid/index.html#gettingstarted) によって、アプリのセキュリティー・レベルを強化します。 アプリに対する Web または API の HTTP /HTTPS 要求を認証するために、[{{site.data.keyword.appid_short_notm}} 認証 Ingress アノテーション](cs_annotations.html#appid-auth)を使用して、{{site.data.keyword.appid_short_notm}} を Ingress サービスと統合できます。</td>
  </tr>
<tr>
<td>Aqua Security</td>
  <td><a href="/docs/services/va/va_index.html" target="_blank">Vulnerability Advisor</a> を補完するために、<a href="https://www.aquasec.com/" target="_blank">Aqua Security <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を使用して、アプリで実行できる動作を減らすことで、コンテナー・デプロイメントのセキュリティーを強化できます。 詳しくは、<a href="https://www.ibm.com/blogs/bluemix/2017/06/protecting-container-deployments-bluemix-aqua-security/" target="_blank">Protecting container deployments on {{site.data.keyword.Bluemix_notm}} with Aqua Security <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。 </td>
</tr>
<tr>
<td>{{site.data.keyword.cloudcerts_full}}</td>
<td><a href="../services/certificate-manager/index.html" target="_blank">{{site.data.keyword.cloudcerts_long}} <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を使用して、アプリの SSL 証明書を保管および管理できます。 詳しくは、<a href="https://www.ibm.com/blogs/bluemix/2018/01/use-ibm-cloud-certificate-manager-ibm-cloud-container-service-deploy-custom-domain-tls-certificates/" target="_blank">Use {{site.data.keyword.cloudcerts_long_notm}} with {{site.data.keyword.containershort_notm}} to deploy custom domain TLS Certificates <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。 </td>
</tr>
<tr>
  <td>{{site.data.keyword.registrylong}}</td>
  <td>保護された独自の Docker イメージ・リポジトリーをセットアップします。そこでイメージを安全に保管し、クラスター・ユーザー間で共有することができます。 詳しくは、<a href="/docs/services/Registry/index.html" target="_blank">{{site.data.keyword.registrylong}} の資料 <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。</td>
</tr>
<tr>
<td>NeuVector</td>
<td><a href="https://neuvector.com/" target="_blank">NeuVector <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を使用して、コンテナーをクラウド・ネイティブ・ファイアウォールによって保護します。 詳しくは、<a href="https://www.ibm.com/us-en/marketplace/neuvector-container-security" target="_blank">NeuVector Container Security <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。 </td>
</tr>
<tr>
<td>Twistlock</td>
<td><a href="/docs/services/va/va_index.html" target="_blank">Vulnerability Advisor</a> を補完するために、<a href="https://www.twistlock.com/" target="_blank">Twistlock <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を使用して、ファイアウォール、脅威防御、インシデント対応を管理できます。 詳しくは、<a href="https://www.ibm.com/blogs/bluemix/2017/07/twistlock-ibm-bluemix-container-service/" target="_blank">Twistlock on {{site.data.keyword.containershort_notm}} <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。 </td>
</tr>
</tbody>
</table>

<br />



## ストレージ・サービス
<table summary="アクセシビリティーについての要約">
<caption>ストレージ・サービス</caption>
<thead>
<tr>
<th>サービス</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Heptio Ark</td>
  <td><a href="https://github.com/heptio/ark" target="_blank">Heptio Ark <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を使用して、クラスター・リソースと永続ボリュームのバックアップとリストアを実行できます。詳しくは、Heptio Ark の <a href="https://github.com/heptio/ark/blob/master/docs/use-cases.md#use-cases" target="_blank">Use cases for disaster recovery and cluster migration <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。</td>
</tr>
<tr>
  <td>{{site.data.keyword.cos_full}}</td>
  <td>{{site.data.keyword.cos_short}} で保管するデータは、暗号化され、複数の地理的ロケーションに分散され、REST API を使用して HTTP によってアクセスされます。 [ibm-backup-restore image](/docs/services/RegistryImages/ibm-backup-restore/index.html) を使用して、クラスターのデータの一回限りのバックアップ、またはスケジュールしたバックアップを実行するようにサービスを構成できます。 このサービスに関する一般情報については、<a href="/docs/services/cloud-object-storage/about-cos.html" target="_blank">{{site.data.keyword.cos_short}} の資料 <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。</td>
</tr>
  <tr>
    <td>{{site.data.keyword.cloudantfull}}</td>
    <td>{{site.data.keyword.cloudant_short_notm}} は、データを JSON 形式の文書として保管する文書指向の DataBase as a Service (DBaaS) です。 このサービスは、スケーラビリティー、高可用性、耐久性を実現するように設計されています。 詳しくは、<a href="/docs/services/Cloudant/getting-started.html" target="_blank">{{site.data.keyword.cloudant_short_notm}} の資料 <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。</td>
  </tr>
  <tr>
    <td>{{site.data.keyword.composeForMongoDB_full}}</td>
    <td>{{site.data.keyword.composeForMongoDB}} は、高可用性と冗長性、自動およびオンデマンドのノンストップ・バックアップ、モニター・ツール、アラート・システムへの統合、パフォーマンス分析ビューなどを提供します。 詳しくは、<a href="/docs/services/ComposeForMongoDB/index.html" target="_blank">{{site.data.keyword.composeForMongoDB}} の資料 <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。</td>
  </tr>
</tbody>
</table>


<br />



## Cloud Foundry サービスをクラスターに追加する
{: #adding_cluster}

既存の Cloud Foundry サービス・インスタンスをクラスターに追加すると、クラスターのユーザーがアプリをクラスターにデプロイする際にその  サービスにアクセスして使用できるようになります。
{:shortdesc}

開始前に、以下のことを行います。

1. [CLI のターゲットを](cs_cli_install.html#cs_cli_configure)自分のクラスターに設定します。
2. [{{site.data.keyword.Bluemix_notm}} サービスのインスタンスを要求します](/docs/apps/reqnsi.html#req_instance)。
   **注:** ワシントン DC のロケーションでサービスのインスタンスを作成するには、CLI を使用する必要があります。
3. Cloud Foundry サービスはクラスターとのバインドがサポートされていますが、他のサービスはサポートされていません。 サービス・インスタンスを作成するとさまざまなサービス・タイプが表示され、それらのサービスはダッシュボード内で**「Cloud Foundry サービス」**および**「サービス」**としてグループ化されます。 **「サービス」**セクション内のサービスをクラスターにバインドするには、[まず Cloud Foundry 別名を作成します](#adding_resource_cluster)。

**注:**
<ul><ul>
<li>サービス・キーをサポートする {{site.data.keyword.Bluemix_notm}} サービスだけを追加できます。 このサービスでサービス・キーがサポートされていない場合は、[{{site.data.keyword.Bluemix_notm}} サービスを使用するための外部アプリの使用可能化](/docs/apps/reqnsi.html#accser_external)を参照してください。</li>
<li>サービスを追加するには、その前にクラスターとワーカー・ノードを完全にデプロイしておく必要があります。</li>
</ul></ul>


サービスを追加するには、以下のようにします。
2.  使用可能な {{site.data.keyword.Bluemix_notm}} サービスをリストします。

    ```
    bx service list
    ```
    {: pre}

    CLI 出力例:

    ```
    name                      service           plan    bound apps   last operation
    <service_instance_name>   <service_name>    spark                create succeeded
    ```
    {: screen}

3.  クラスターに追加するサービス・インスタンスの**名前**をメモします。
4.  サービスを追加するために使用するクラスターの名前空間を識別します。 次のいずれかのオプションを選択します。
    -   既存の名前空間をリストして、使用する名前空間を選択します。

        ```
        kubectl get namespaces
        ```
        {: pre}

    -   クラスターに名前空間を作成します。

        ```
        kubectl create namespace <namespace_name>
        ```
        {: pre}

5.  サービスをクラスターに追加します。

    ```
    bx cs cluster-service-bind <cluster_name_or_ID> <namespace> <service_instance_name>
    ```
    {: pre}

    サービスがクラスターに正常に追加されると、サービス・インスタンスの資格情報を保持するクラスター・シークレットが作成されます。 CLI 出力例:

    ```
    bx cs cluster-service-bind mycluster mynamespace cleardb
    Binding service instance to namespace...
    OK
    Namespace:	mynamespace
    Secret name:     binding-<service_instance_name>
    ```
    {: screen}

6.  クラスターの名前空間内にシークレットが作成されたことを確認します。

    ```
    kubectl get secrets --namespace=<namespace>
    ```
    {: pre}

クラスター・ユーザーは、クラスターにデプロイされたポッドでサービスを使用するために、サービス資格情報にアクセスする必要があります。ユーザーは、この [Kubernetes シークレットをシークレット・ボリュームとしてポッドにマウントする](#adding_app)ことで、{{site.data.keyword.Bluemix_notm}} サービスのサービス資格情報にアクセスできます。

<br />


## 他の {{site.data.keyword.Bluemix_notm}} サービス・リソース用のクラウド Foundry 別名の作成
{: #adding_resource_cluster}

Cloud Foundry サービスは、クラスターとのバインディングをサポートされます。 Cloud Foundry サービスではない {{site.data.keyword.Bluemix_notm}} サービスをクラスターにバインドするには、サービス・インスタンスの Cloud Foundry 別名を作成します。
{:shortdesc}

始める前に、[{{site.data.keyword.Bluemix_notm}} サービスのインスタンスを要求します](/docs/apps/reqnsi.html#req_instance)。

サービス・インスタンスの Cloud Foundry 別名を作成するには、以下のようにします。

1. サービス・インスタンスが作成された組織とスペースをターゲットにします。

    ```
    bx target -o <org_name> -s <space_name>
    ```
    {: pre}

2. サービス・インスタンス名をメモします。
    ```
    bx resource service-instances
    ```
    {: pre}

3. サービス・インスタンスの Cloud Foundry 別名を作成します。
    ```
    bx resource service-alias-create <service_alias_name> --instance-name <service_instance>
    ```
    {: pre}

4. サービス別名が作成されたことを確認します。

    ```
    bx service list
    ```
    {: pre}

5. [Cloud Foundry の別名をクラスターにバインドします](#adding_cluster)。



<br />


## サービスをアプリに追加する
{: #adding_app}

暗号化した Kubernetes シークレットを使用して、{{site.data.keyword.Bluemix_notm}} サービスの詳細情報や資格情報を保管し、サービスとクラスターの間のセキュアな通信を確保します。
{:shortdesc}

Kubernetes シークレットは、機密情報 (ユーザー名、パスワード、鍵など) を安全に保管するための手段です。 機密情報を環境変数として公開したり、Dockerfile に直接書き込んだりする代わりに、シークレットをポッドにマウントできます。それにより、それらのシークレットに、ポッド内で実行中のコンテナーからアクセスできます。

シークレット・ボリュームをポッドにマウントすると、`binding` という名前のファイルがボリューム・マウント・ディレクトリーに保管されます。`binding` ファイルに、{{site.data.keyword.Bluemix_notm}} サービスにアクセスするために必要なすべての情報と資格情報が格納されます。

始めに、[CLI のターゲット](cs_cli_install.html#cs_cli_configure)を自分のクラスターに設定してください。 アプリで使用する {{site.data.keyword.Bluemix_notm}} サービスが、クラスター管理者によって[クラスターに追加されていること](cs_integrations.html#adding_cluster)を確認してください。

1.  クラスターの名前空間で使用できるシークレットのリストを表示します。

    ```
    kubectl get secrets --namespace=<my_namespace>
    ```
    {: pre}

    出力例:

    ```
    NAME                              TYPE            DATA      AGE
    binding-<service_instance_name>   Opaque          1         3m

    ```
    {: screen}

2.  **Opaque** タイプのシークレットを探して、そのシークレットの **NAME** を書き留めます。 複数のシークレットが存在する場合は、クラスター管理者に連絡して、サービスに対応する正しいシークレットを確認してください。

3.  任意のエディターを開きます。

4.  YAML ファイルを作成し、シークレット・ボリュームによってサービスの詳細情報にアクセスできるポッドを構成します。 複数のサービスをバインドした場合は、各シークレットが正しいサービスに関連付けられていることを確認してください。

    ```
    apiVersion: apps/v1beta1
    kind: Deployment
    metadata:
      labels:
        app: secret-test
      name: secret-test
      namespace: <my_namespace>
    spec:
      selector:
        matchLabels:
          app: secret-test
      replicas: 1
      template:
        metadata:
          labels:
            app: secret-test
        spec:
          containers:
          - image: nginx
            name: secret-test
            volumeMounts:
            - mountPath: /opt/service-bind
              name: service-bind-volume
          volumes:
          - name: service-bind-volume
            secret:
              defaultMode: 420
              secretName: binding-<service_instance_name>
    ```
    {: codeblock}

    <table>
    <caption>YAML ファイルの構成要素について</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
    </thead>
    <tbody>
    <tr>
    <td><code>volumeMounts/mountPath</code></td>
    <td>コンテナーにマウントするシークレット・ボリュームの名前。</td>
    </tr>
    <tr>
    <td><code>volumes/name</code></td>
    <td>コンテナーにマウントするシークレット・ボリュームの名前を入力します。</td>
    </tr>
    <tr>
    <td><code>secret/defaultMode</code></td>
    <td>サービスのシークレットに対する読み取り専用アクセス権を設定します。</td>
    </tr>
    <tr>
    <td><code>secret/secretName</code></td>
    <td>先ほど書き留めたシークレットの名前を入力します。</td>
    </tr></tbody></table>

5.  ポッドを作成して、シークレット・ボリュームをマウントします。

    ```
    kubectl apply -f secret-test.yaml
    ```
    {: pre}

6.  ポッドが作成されたことを確認します。

    ```
    kubectl get pods --namespace=<my_namespace>
    ```
    {: pre}

    CLI 出力例:

    ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
    {: screen}

7.  ポッドの **NAME** を書き留めます。
8.  ポッドの詳細情報を取得して、シークレットの名前を探します。

    ```
    kubectl describe pod <pod_name>
    ```
    {: pre}

    出力:

    ```
    ...
    Volumes:
      service-bind-volume:
        Type:       Secret (a volume populated by a Secret)
        SecretName: binding-<service_instance_name>
    ...
    ```
    {: screen}

    

9.  マウント・ディレクトリーで `binding` シークレット・ファイルを見つけ、その JSON コンテンツを解析して、{{site.data.keyword.Bluemix_notm}} サービスにアクセスするための URL とサービス資格情報を判別するという一連の操作を行うようにアプリを構成します。

{{site.data.keyword.Bluemix_notm}} サービスの詳細情報や資格情報にアクセスできるようになりました。 {{site.data.keyword.Bluemix_notm}} サービスを利用するために、マウント・ディレクトリーでサービスのシークレット・ファイルを見つけ、JSON コンテンツを解析してサービスの詳細情報を判別できるようにアプリを構成してください。

<br />


## {{site.data.keyword.containershort_notm}} での Helm のセットアップ
{: #helm}

[Helm ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://helm.sh/) は Kubernetes パッケージ・マネージャーです。 Helm チャートを作成するか、既存の Helm チャートを使用して、{{site.data.keyword.containerlong_notm}} クラスターで実行される複雑な Kubernetes アプリケーションの定義、インストール、アップグレードを行うことができます。
{:shortdesc}

Helm チャートを {{site.data.keyword.containershort_notm}} で使用する前に、クラスターに Helm インスタンスをインストールして初期化する必要があります。 その後、{{site.data.keyword.Bluemix_notm}} Helm リポジトリーを Helm インスタンスに追加できます。

始めに、[CLI のターゲット](cs_cli_install.html#cs_cli_configure)を、Helm チャートを使用するクラスターに設定してください。

1. <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> をインストールします。

2. **重要**: クラスターのセキュリティーを維持するため、Tiller のサービス・アカウントを `kube-system` 名前空間に作成し、`tiller-deploy` ポッドに対する Kubernetes RBAC クラスター役割バインディングを作成します。

    1. 任意のエディターで、以下のファイルを作成し、`rbac-config.yaml` として保存します。
      **注:**
        * `cluster-admin` クラスター役割は、デフォルトで Kubernetes クラスターに作成されるので、明示的に定義する必要はありません。
        * バージョン 1.7.x のクラスターを使用する場合、`apiVersion` を `rbac.authorization.k8s.io/v1beta1` に変更してください。

      ```
      apiVersion: v1
      kind: ServiceAccount
      metadata:
        name: tiller
        namespace: kube-system
      ---
      apiVersion: rbac.authorization.k8s.io/v1
      kind: ClusterRoleBinding
      metadata:
        name: tiller
      roleRef:
        apiGroup: rbac.authorization.k8s.io
        kind: ClusterRole
        name: cluster-admin
      subjects:
        - kind: ServiceAccount
          name: tiller
          namespace: kube-system
      ```
      {: codeblock}

    2. サービス・アカウントとクラスター役割バインディングを作成します。

        ```
        kubectl create -f rbac-config.yaml
        ```
        {: pre}

3. 作成したサービス・アカウントを使用して、Helm を初期化し、`tiller` をインストールします。

    ```
    helm init --service-account tiller
    ```
    {: pre}

4. クラスター内の `tiller-deploy` ポッドの**「状況」**が`「実行中」`になっていることを確認します。

    ```
    kubectl get pods -n kube-system -l app=helm
    ```
    {: pre}

    出力例:

    ```
    NAME                            READY     STATUS    RESTARTS   AGE
    tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
    ```
    {: screen}

5. {{site.data.keyword.Bluemix_notm}} Helm リポジトリーを Helm インスタンスに追加します。

    ```
    helm repo add ibm  https://registry.bluemix.net/helm/ibm
    ```
    {: pre}

6. {{site.data.keyword.Bluemix_notm}} リポジトリーで現在使用可能な Helm チャートをリストします。

    ```
    helm search ibm
    ```
    {: pre}

7. チャートの詳細を確認するには、その設定とデフォルト値をリストします。

    例えば、strongSwan IPSec VPN サービス Helm チャートの設定、文書、デフォルト値を確認するには、次のようにします。

    ```
    helm inspect ibm/strongswan
    ```
    {: pre}


### Helm の関連リンク
{: #helm_links}

* strongSwan Helm チャートを使用するには、[strongSwan IPSec VPN サービスの Helm Chart を使用した VPN 接続のセットアップ](cs_vpn.html#vpn-setup)を参照してください。
* [Helm チャートのカタログ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/containers-kubernetes/solutions/helm-charts) GUI で、{{site.data.keyword.Bluemix_notm}} で使用可能な Helm チャートを参照します。
* Helm チャートのセットアップと管理に使用する Helm コマンドについて詳しくは、<a href="https://docs.helm.sh/helm/" target="_blank">Helm の資料 <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。
* Kubernetes Helm チャートを使用して開発速度を上げる方法について詳しくは、[こちら ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.ibm.com/recipes/tutorials/increase-deployment-velocity-with-kubernetes-helm-charts/) を参照してください。

## Kubernetes クラスター・リソースの視覚化
{: #weavescope}

Weave Scope は、Kubernetes クラスター内のリソース (サービス、ポッド、コンテナーなど) のビジュアル図を表示します。 Weave Scope には、CPU とメモリーのインタラクティブ・メトリックと、コンテナーの中で追跡したり実行したりできるツールが備わっています。
{:shortdesc}

開始前に、以下のことを行います。

-   クラスター情報を公共のインターネットに公開しないようにしてください。 安全に Weave Scope をデプロイして Web ブラウザーからローカル・アクセスするには、以下の手順を実行します。
-   標準クラスターがまだない場合は、[標準クラスターを作成します](cs_clusters.html#clusters_ui)。 Weave Scope は、アプリでは特に、CPU の負荷が大きくなります。 フリー・クラスターではなく、比較的大規模な標準クラスターで Weave Scope を実行してください。
-   対象のクラスターに対して `kubectl` コマンドを実行するように [CLI のターゲット設定を行います](cs_cli_install.html#cs_cli_configure)。


Weave Scope をクラスターで使用するには、以下のようにします。
2.  提供された RBAC 許可構成ファイルをクラスターにデプロイします。

    読み取り/書き込みアクセス権を有効にするには、以下のようにします。

    ```
    kubectl apply -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac.yaml"
    ```
    {: pre}

    読み取り専用アクセス権を有効にするには、以下のようにします。

    ```
    kubectl apply --namespace weave -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac-readonly.yaml"
    ```
    {: pre}

    出力:

    ```
    clusterrole "weave-scope-mgr" created
    clusterrolebinding "weave-scope-mgr-role-binding" created
    ```
    {: screen}

3.  Weave Scope サービスをデプロイします。このサービスは、クラスター IP アドレスでのプライベート・アクセスが可能です。

    <pre class="pre">
    <code>kubectl apply --namespace weave -f "https://cloud.weave.works/k8s/scope.yaml?k8s-version=$(kubectl version | base64 | tr -d '&bsol;n')"</code>
    </pre>

    出力:

    ```
    serviceaccount "weave-scope" created
    deployment "weave-scope-app" created
    service "weave-scope-app" created
    daemonset "weave-scope-agent" created
    ```
    {: screen}

4.  ポート転送コマンドを実行して、コンピューター上でサービスを開きます。次回 Weave Scope にアクセスする時は、上記の構成ステップを再度実行する必要はなく、次のポート転送コマンドを実行することができます。

    ```
    kubectl port-forward -n weave "$(kubectl get -n weave pod --selector=weave-scope-component=app -o jsonpath='{.items..metadata.name}')" 4040
    ```
    {: pre}

    出力:

    ```
    Forwarding from 127.0.0.1:4040 -> 4040
    Forwarding from [::1]: :1]:4040 -> 4040
    Handling connection for 4040
    ```
    {: screen}

5.  Web ブラウザーで `http://localhost:4040` を開きます。 デフォルトのコンポーネントをデプロイしないと、次の図が表示されます。 クラスター内の Kubernetes リソースのトポロジー・ダイアグラムまたはテーブルを表示するように選択できます。

     <img src="images/weave_scope.png" alt="Weave Scope のトポロジーの例" style="width:357px;" />


[Weave Scope の機能についての詳細 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.weave.works/docs/scope/latest/features/)。

<br />

