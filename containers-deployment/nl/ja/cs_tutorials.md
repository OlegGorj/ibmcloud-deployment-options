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



# チュートリアル: クラスターの作成
{: #cs_cluster_tutorial}

Kubernetes クラスターを {{site.data.keyword.containerlong}} にデプロイして管理します。 クラスター上でのコンテナー化アプリのデプロイメント、操作、スケーリング、モニタリングを自動化することができます。
{:shortdesc}

このチュートリアル・シリーズでは、架空の PR 会社が Kubernetes 機能を使用してコンテナー化アプリを {{site.data.keyword.Bluemix_notm}} にデプロイする方法を示します。 この PR 会社は、{{site.data.keyword.toneanalyzerfull}} を利用してプレス・リリースを分析し、フィードバックを受け取ります。


## 達成目標

この最初のチュートリアルでは、あなたがこの PR 会社のネットワーキング管理者になったつもりで学習を進めていきます。 あなたは、アプリの Hello World バージョンをデプロイしてテストするために使用されるカスタム Kubernetes クラスターを構成します。

インフラストラクチャーをセットアップするには、以下のようにします。

-   1 つのワーカー・ノードがあるクラスターを作成します。
-   Kubernetes コマンドを実行し Docker イメージを管理するための CLI をインストールします。
-   イメージを格納するためのプライベート・イメージ・リポジトリーを {{site.data.keyword.registrylong_notm}} で作成します。
-   {{site.data.keyword.toneanalyzershort}} サービスをクラスターに追加して、そのサービスをクラスター内のすべてのアプリが使用できるようにします。


## 所要時間

40 分


## 対象読者

このチュートリアルは、Kubernetes クラスターを初めて作成するソフトウェア開発者やネットワーク管理者を対象にしています。


## 前提条件

-  従量課金 (PAYG) またはサブスクリプションの [{{site.data.keyword.Bluemix_notm}} アカウント ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/registration/)
-  作業を行うクラスター・スペース内の [Cloud Foundry 開発者役割](/docs/iam/mngcf.html#mngcf)


## レッスン 1: クラスターを作成して CLI をセットアップする
{: #cs_cluster_tutorial_lesson1}

GUI 内にクラスターを作成して必要な CLI をインストールします。
{: shortdesc}

**クラスターを作成するには、以下のようにします。**

クラスターがプロビジョンされるまでには数分かかることがあるため、CLI をインストールする前にクラスターを作成します。

1.  [GUI![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/containers-kubernetes/catalog/cluster/create) で、1 つのワーカー・ノードを持つフリー・クラスターまたは標準クラスターを作成します。

    [CLI でクラスター](cs_clusters.html#clusters_cli)を作成することもできます。
    {: tip}

クラスターがプロビジョンされたら、クラスターを管理するために使用する以下の CLI をインストールします。
-   {{site.data.keyword.Bluemix_notm}} CLI
-   {{site.data.keyword.containershort_notm}} プラグイン
-   Kubernetes CLI
-   {{site.data.keyword.registryshort_notm}} プラグイン
-   Docker CLI

</br>
**CLI とその前提条件をインストールするには、以下のようにします。**

1.  {{site.data.keyword.containershort_notm}} プラグインの前提条件として、[{{site.data.keyword.Bluemix_notm}} CLI ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://clis.ng.bluemix.net/ui/home.html) をインストールします。 {{site.data.keyword.Bluemix_notm}} CLI コマンドを実行するには、接頭部 `bx` を使用します。
2.  プロンプトに従ってアカウントと {{site.data.keyword.Bluemix_notm}} 組織を選択します。 クラスターはアカウントに固有のものですが、{{site.data.keyword.Bluemix_notm}} 組織またはスペースからは独立しています。

4.  Kubernetes クラスターを作成してワーカー・ノードを管理するために、{{site.data.keyword.containershort_notm}} プラグインをインストールします。 {{site.data.keyword.containershort_notm}} プラグイン・コマンドを実行するには、接頭部 `bx cs` を使用します。

    ```
    bx plugin install container-service -r Bluemix
    ```
    {: pre}

5.  クラスターにアプリをデプロイするには、[Kubernetes CLI をインストールします ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/tasks/tools/install-kubectl/)。Kubernetes CLI を使用してコマンドを実行するには、接頭部 `kubectl` を使用します。
    1.  機能の完全な互換性を確保するには、使用する予定の Kubernetes クラスター・バージョンと一致する Kubernetes CLI バージョンをダウンロードします。 現在の {{site.data.keyword.containershort_notm}} のデフォルト Kubernetes バージョンは 1.9.7 です。

        OS X:   [https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/darwin/amd64/kubectl ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/darwin/amd64/kubectl)

        Linux:   [https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/linux/amd64/kubectl ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/linux/amd64/kubectl)

        Windows:   [https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/windows/amd64/kubectl.exe ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/windows/amd64/kubectl.exe)

          **ヒント:** Windows を使用している場合、Kubernetes CLI を {{site.data.keyword.Bluemix_notm}} CLI と同じディレクトリーにインストールします。 このようにセットアップすると、後でコマンドを実行するとき、ファイル・パスの変更を行う手間がいくらか少なくなります。

    2.  OS X または Linux を使用している場合、以下の手順を実行します。
        1.  実行可能ファイルを `/usr/local/bin` ディレクトリーに移動します。

            ```
            mv filepath/kubectl /usr/local/bin/kubectl
            ```
            {: pre}

        2.  `/usr/local/bin` が `PATH` システム変数にリストされていることを確認します。 `PATH` 変数には、オペレーティング・システムが実行可能ファイルを見つけることのできるすべてのディレクトリーが含まれています。 `PATH` 変数にリストされた複数のディレクトリーには、それぞれ異なる目的があります。 `/usr/local/bin` は実行可能ファイルを保管するために使用されますが、保管対象となるのは、オペレーティング・システムの一部ではなく、システム管理者によって手動でインストールされたソフトウェアです。

            ```
            echo $PATH
            ```
            {: pre}

            CLI 出力は、以下のようになります。

            ```
            /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
            ```
            {: screen}

        3.  ファイルを実行可能にします。

            ```
            chmod +x /usr/local/bin/kubectl
            ```
            {: pre}

6. {{site.data.keyword.registryshort_notm}} でプライベート・イメージ・リポジトリーをセットアップして管理するには、{{site.data.keyword.registryshort_notm}} プラグインをインストールします。 レジストリー・コマンドを実行するには、接頭部 `bx cr` を使用します。

    ```
    bx plugin install container-registry -r Bluemix
    ```
    {: pre}

    container-service プラグインと container-registry プラグインが正常にインストールされたことを検証するには、以下のコマンドを実行します。

    ```
    bx plugin list
    ```
    {: pre}

7. ローカルにイメージを作成して、それらをプライベート・イメージ・リポジトリーにプッシュするには、[Docker Community Edition CLI をインストールします ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.docker.com/community-edition#/download)。Windows 8 以前を使用している場合、代わりに [Docker Toolbox ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.docker.com/toolbox/toolbox_install_windows/) をインストールしてください。

これで完了です。 CLI のインストールを正常に行うことができたので、次のレッスンとチュートリアルに進むことができます。 次に、クラスター環境をセットアップして {{site.data.keyword.toneanalyzershort}} サービスを追加します。


## レッスン 2: プライベート・レジストリーをセットアップする
{: #cs_cluster_tutorial_lesson2}

{{site.data.keyword.registryshort_notm}} でプライベート・イメージ・リポジトリーをセットアップし、シークレットをクラスターに追加して、アプリが {{site.data.keyword.toneanalyzershort}} サービスにアクセスできるようにします。
{: shortdesc}

1.  プロンプトが出されたら、{{site.data.keyword.Bluemix_notm}} 資格情報を使用して {{site.data.keyword.Bluemix_notm}} CLI にログインします。

    ```
    bx login [--sso]
    ```
    {: pre}

    **注:** フェデレーテッド ID がある場合、`--sso` フラグを使用してログインします。 ユーザー名を入力し、CLI 出力に示された URL を使用して、ワンタイム・パスコードを取得してください。

2.  独自のプライベート・イメージ・リポジトリーを {{site.data.keyword.registryshort_notm}} にセットアップすることによって、Docker イメージを安全に保管し、すべてのクラスター・ユーザーと共有します。 {{site.data.keyword.Bluemix_notm}} 内のプライベート・イメージ・リポジトリーは、名前空間によって識別されます。 イメージ・リポジトリーの固有の URL を作成するために名前空間が使用されます。開発者はこれを使用してプライベート Dockerイメージにアクセスできます。

    コンテナー・イメージを使用する際の[個人情報の保護](cs_secure.html#pi)の詳細を確認してください。

    この例で PR 会社はイメージ・リポジトリーを {{site.data.keyword.registryshort_notm}} に 1 つだけを作成するので、アカウント内のすべてのイメージをグループする名前空間として _pr_firm_ を選択します。 _&lt;namespace&gt;_ を、このチュートリアルに関係のない任意の名前空間に置き換えてください。

    ```
    bx cr namespace-add <namespace>
    ```
    {: pre}

3.  次のステップに進む前に、ワーカー・ノードのデプロイメントが完了したことを確認します。

    ```
    bx cs workers <cluster_name_or_ID>
    ```
    {: pre}

    ワーカー・ノードのプロビジョニングが終了すると、状況が **Ready** に変わり、{{site.data.keyword.Bluemix_notm}} サービスのバインドを開始できます。

    ```
    ID                                                 Public IP       Private IP       Machine Type   State    Status   Location   Version
    kube-mil01-pafe24f557f070463caf9e31ecf2d96625-w1   169.xx.xxx.xxx   10.xxx.xx.xxx   free           normal   Ready    mil01      1.9.7
    ```
    {: screen}

## レッスン 3: クラスター環境をセットアップする
{: #cs_cluster_tutorial_lesson3}

CLI でクラスターのコンテキストを設定します。
{: shortdesc}

クラスターの作業を行うために {{site.data.keyword.containerlong}} CLI にログインするたびに、これらのコマンドを実行して、クラスターの構成ファイルのパスをセッション変数として設定する必要があります。 Kubernetes CLI はこの変数を使用して、{{site.data.keyword.Bluemix_notm}} 内のクラスターと接続するために必要なローカル構成ファイルと証明書を検索します。

1.  環境変数を設定して Kubernetes 構成ファイルをダウンロードするためのコマンドを取得します。

    ```
    bx cs cluster-config <cluster_name_or_ID>
    ```
    {: pre}

    構成ファイルのダウンロードが完了すると、そのローカルの Kubernetes 構成ファイルのパスを環境変数として設定するために使用できるコマンドが表示されます。

    OS X の場合の例:

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/pr_firm_cluster/kube-config-prod-par02-pr_firm_cluster.yml
    ```
    {: screen}

2.  `KUBECONFIG` 環境変数を設定するためのコマンドとしてターミナルに表示されたものを、コピーして貼り付けます。

3.  `KUBECONFIG` 環境変数が適切に設定されたことを確認します。

    OS X の場合の例:

    ```
    echo $KUBECONFIG
    ```
    {: pre}

    出力:

    ```
    /Users/<user_name>/.bluemix/plugins/container-service/clusters/pr_firm_cluster/kube-config-prod-par02-pr_firm_cluster.yml
    ```
    {: screen}

4.  Kubernetes CLI サーバーのバージョンを調べて、ご使用のクラスターで `kubectl` コマンドが正常に実行することを確認します。

    ```
    kubectl version  --short
    ```
    {: pre}

    出力例:

    ```
    Client Version: v1.9.7
    Server Version: v1.9.7
    ```
    {: screen}

## レッスン 4: クラスターにサービスを追加する
{: #cs_cluster_tutorial_lesson4}

{{site.data.keyword.Bluemix_notm}} サービスを使用すると、既に開発された機能をアプリで活用できます。 クラスターにバインドされているすべての {{site.data.keyword.Bluemix_notm}} サービスは、そのクラスターにデプロイされたアプリで使用できます。 アプリで使用する {{site.data.keyword.Bluemix_notm}} サービスごとに、以下の手順を繰り返してください。

1.  {{site.data.keyword.toneanalyzershort}} サービスを {{site.data.keyword.Bluemix_notm}} アカウントに追加します。 <service_name> をサービス・インスタンスの名前に置き換えます。

    **注:** {{site.data.keyword.toneanalyzershort}} サービスをアカウントに追加すると、そのサービスが無料ではないことを示すメッセージが表示されます。 API 呼び出しを制限している場合には、このチュートリアルによって {{site.data.keyword.watson}} サービスからの課金は発生しません。 [{{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} サービスの料金情報を確認します ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/watson/developercloud/tone-analyzer.html#pricing-block)。

    ```
    bx service create tone_analyzer standard <service_name>
    ```
    {: pre}

2.  {{site.data.keyword.toneanalyzershort}} インスタンスをクラスターの `default` の Kubernetes 名前空間にバインドします。 あとで独自の名前空間を作成して Kubernetes リソースへのユーザー・アクセスを管理できますが、現時点では `default` 名前空間を使用します。 Kubernetes 名前空間は、以前に作成したレジストリー名前空間とは異なります。

    ```
    bx cs cluster-service-bind <cluster_name> default <service_name>
    ```
    {: pre}

    出力:

    ```
    bx cs cluster-service-bind pr_firm_cluster default mytoneanalyzer
    Binding service instance to namespace...
    OK
    Namespace:	default
    Secret name:	binding-mytoneanalyzer
    ```
    {: screen}

3.  クラスターの名前空間内に Kubernetes シークレットが作成されたことを確認します。 すべての {{site.data.keyword.Bluemix_notm}} サービスは、ユーザー名、パスワード、コンテナーがアクセスするために使用する URL など、機密情報を含む JSON ファイルによって定義されます。この情報を安全に保管するために、Kubernetes シークレットが使用されます。 この例では、アカウントにプロビジョンされる、{{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} にアクセスするための資格情報がシークレットに格納されます。

    ```
    kubectl get secrets --namespace=default
    ```
    {: pre}

    出力:

    ```
    NAME                       TYPE                                  DATA      AGE
    binding-mytoneanalyzer     Opaque                                1         1m
    bluemix-default-secret     kubernetes.io/dockercfg               1         1h
    default-token-kf97z        kubernetes.io/service-account-token   3         1h
    ```
    {: screen}

</br>
おつかれさまでした。 クラスターが構成され、ローカル環境でアプリをクラスターにデプロイする準備が整いました。

## 次の作業
{: #next}

* [クイズに答えて 知識を試してみましょう ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://ibmcloud-quizzes.mybluemix.net/containers/cluster_tutorial/quiz.php)。

* [チュートリアル: {{site.data.keyword.containershort_notm}} の Kubernetes クラスターにアプリをデプロイする方法](cs_tutorials_apps.html#cs_apps_tutorial)を試して、作成したクラスターに PR 会社のアプリをデプロイします。
