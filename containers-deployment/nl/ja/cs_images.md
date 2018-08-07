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




# イメージからのコンテナーのビルド
{: #images}

Docker イメージが、{{site.data.keyword.containerlong}} で作成するすべてのコンテナーの基礎です。
{:shortdesc}

イメージは、Dockerfile (イメージをビルドするための指示が入ったファイル) から作成されます。 Dockerfile の別個に保管されている指示の中で、ビルド成果物 (アプリ、アプリの構成、その従属関係) が参照されることもあります。

## イメージ・レジストリーの計画
{: #planning}

イメージは通常、レジストリーに保管されます。だれでもアクセスできるレジストリー (パブリック・レジストリー) を使用することも、小さなグループのユーザーだけにアクセスを限定したレジストリー (プライベート・レジストリー) をセットアップすることもできます。
{:shortdesc}

Docker Hub などのパブリック・レジストリーは、Docker および Kubernetes の入門として最初のコンテナー化アプリをクラスター内に作成する時に使用できます。 しかしエンタープライズ・アプリケーションを作成する場合は、許可されていないユーザーが勝手にイメージを使用したり変更したりすることがないように、 {{site.data.keyword.registryshort_notm}} で提供されているようなプライベート・レジストリーを使用してください。 プライベート・レジストリーはクラスター管理者がセットアップする必要があります。プライベート・レジストリーにアクセスするための資格情報をクラスター・ユーザーに提供する必要があるためです。


{{site.data.keyword.containershort_notm}} では、複数のレジストリーを使用してアプリをクラスターにデプロイできます。

|レジストリー|説明|利点|
|--------|-----------|-------|
|[{{site.data.keyword.registryshort_notm}}](/docs/services/Registry/index.html)|このオプションを使用すると、保護された独自の Docker イメージ・リポジトリーを {{site.data.keyword.registryshort_notm}} 内にセットアップできます。そこでイメージを安全に保管してクラスター・ユーザー間で共有することができます。|<ul><li>アカウント内でイメージへのアクセス権限を管理できる。</li><li>{{site.data.keyword.IBM_notm}} 提供のイメージとサンプル・アプリ ({{site.data.keyword.IBM_notm}} Liberty など) を親イメージとして使用し、それに独自のアプリ・コードを追加できる。</li><li>Vulnerability Advisor によるイメージの潜在的脆弱性の自動スキャン (それらの脆弱性を修正するための OS 固有の推奨事項を含む)。</li></ul>|
|他のプライベート・レジストリー|[imagePullSecret ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/containers/images/) を作成して、既存のプライベート・レジストリーをクラスターに接続します。 このシークレットは、レジストリーの URL と資格情報を Kubernetes シークレットに安全に保存するために使用されます。|<ul><li>ソース (Docker Hub、組織が所有するレジストリー、または他のプライベート・クラウド・レジストリー) と無関係に既存のプライベート・レジストリーを使用できる。</li></ul>|
|[パブリック Docker ハブ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://hub.docker.com/){: #dockerhub}|このオプションを使用すると、Dockerfile に変更を加える必要がない場合に、Docker Hub にある既存のパブリック・イメージを [Kubernetes デプロイメント ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) で直接使用できます。 <p>**注:** このオプションは組織のセキュリティー要件 (アクセス管理、脆弱性スキャン、アプリ・プライバシーなど) を満たさない可能性があるということに留意してください。</p>|<ul><li>クラスターに関して追加のセットアップは不要です。</li><li>さまざまなオープン・ソース・アプリケーションを含めることができる。</li></ul>|
{: caption="パブリック・イメージ・レジストリーおよびプライベート・イメージ・レジストリーのオプション" caption-side="top"}

イメージ・レジストリーをセットアップすると、クラスター・ユーザーがイメージを使用してクラスターにアプリをデプロイできるようになります。

コンテナー・イメージを使用する際の[個人情報の保護](cs_secure.html#pi)の詳細を確認してください。

<br />


## コンテナー・イメージのための信頼できるコンテンツのセットアップ
{: #trusted_images}

署名ありで {{site.data.keyword.registryshort_notm}} に保管されている信頼できるイメージからコンテナーを構築できます。また、署名なしのイメージや脆弱なイメージのデプロイを防止できます。
{:shortdesc}

1.  [信頼できるコンテンツとしてイメージに署名します](/docs/services/Registry/registry_trusted_content.html#registry_trustedcontent)。 イメージに信頼をセットアップしたら、信頼できるコンテンツと、レジストリーにイメージをプッシュできる署名者を管理できるようになります。
2.  署名されたイメージしかクラスターのコンテナー作成に使用できないようにするポリシーを適用するには、[Container Image Security Enforcement (ベータ) を追加します](/docs/services/Registry/registry_security_enforce.html#security_enforce)。
3.  アプリをデプロイします。
    1. [`default` の Kubernetes 名前空間にデプロイします](#namespace)。
    2. [別の Kubernetes 名前空間にデプロイするか、別の {{site.data.keyword.Bluemix_notm}} 地域またはアカウントからデプロイします](#other)。

<br />


## {{site.data.keyword.registryshort_notm}} イメージから `default` の Kubernetes 名前空間へのコンテナーのデプロイ
{: #namespace}

{{site.data.keyword.registryshort_notm}} 内の名前空間に保管されている IBM 提供のパブリック・イメージやプライベート・イメージからクラスターにコンテナーをデプロイできます。
{:shortdesc}

クラスターを作成すると、有効期限のないレジストリー・トークンおよびシークレットが、[最も近い地域レジストリーとグローバル・レジストリー](/docs/services/Registry/registry_overview.html#registry_regions)の両方に対して自動的に作成されます。 グローバル・レジストリーには、ユーザーが各地域レジストリーに保管されたイメージを別個に参照する代わりにデプロイメント全体で参照することのできる、IBM 提供のパブリック・イメージが安全に保管されます。 地域レジストリーには、グローバル・レジストリーに保管されるパブリック・イメージと同じイメージに加えて、独自のプライベート Docker イメージが安全に保管されます。 トークンは、{{site.data.keyword.registryshort_notm}} 内にセットアップした名前空間への読み取り専用アクセスを許可するために使用されるもので、これにより、パブリック (グローバル・レジストリー) およびプライベート (地域レジストリー) のイメージの処理が可能になります。

コンテナー化アプリのデプロイ時に Kubernetes クラスターがトークンにアクセスできるように、各トークンは Kubernetes の `imagePullSecret` 内に保管されている必要があります。 クラスターが作成されると、{{site.data.keyword.containershort_notm}} により、グローバル・レジストリー (IBM 提供のパブリック・イメージ) および地域レジストリーのトークンが Kubernetes イメージ・プル・シークレット内に自動的に保管されます。 イメージ・プル・シークレットは、`default` Kubernetes 名前空間、その名前空間の `ServiceAccount` 内のデフォルトのシークレット・リスト、`kube-system` 名前空間に追加されます。

**注:** この初期セットアップを使用すると、{{site.data.keyword.Bluemix_notm}} アカウントの名前空間にある任意のイメージのコンテナーを、クラスターの **default** 名前空間にデプロイできます。 クラスターのその他の名前空間にコンテナーをデプロイする場合や、別の {{site.data.keyword.Bluemix_notm}} 地域か別の {{site.data.keyword.Bluemix_notm}} アカウントに保管されているイメージを使用する場合は、その[クラスター用に独自の imagePullSecret を作成](#other)する必要があります。

開始前に、以下のことを行います。
1. [{{site.data.keyword.Bluemix_notm}} Public または {{site.data.keyword.Bluemix_dedicated_notm}} の {{site.data.keyword.registryshort_notm}} に名前空間をセットアップし、その名前空間にイメージをプッシュします](/docs/services/Registry/registry_setup_cli_namespace.html#registry_namespace_add)。
2. [クラスターを作成します](cs_clusters.html#clusters_cli)。
3. [CLI のターゲットを自分のクラスターに設定します](cs_cli_install.html#cs_cli_configure)。

コンテナーをクラスターの **default** 名前空間にデプロイするために、構成ファイルを作成します。

1.  `mydeployment.yaml` という名前のデプロイメント構成ファイルを作成します。
2.  デプロイメントと、{{site.data.keyword.registryshort_notm}}内の名前空間にある、使用するイメージを定義します。

    {{site.data.keyword.registryshort_notm}}内の名前空間にあるプライベート・イメージを使用するには、次のようにします。

    ```
    apiVersion: apps/v1beta1
    kind: Deployment
    metadata:
      name: ibmliberty-deployment
    spec:
      replicas: 3
      template:
        metadata:
          labels:
            app: ibmliberty
        spec:
          containers:
          - name: ibmliberty
            image: registry.<region>.bluemix.net/<namespace>/<my_image>:<tag>
    ```
    {: codeblock}

    **ヒント:** 名前空間の情報を取得するには、`bx cr namespace-list` を実行します。

3.  クラスター内にデプロイメントを作成します。

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

    **ヒント:** IBM 提供のパブリック・イメージなど、既存の構成ファイルをデプロイすることもできます。 この例では、米国南部地域の **ibmliberty** イメージを使用しています。

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/deploy-ibmliberty.yaml
    ```
    {: pre}


<br />



## 他の Kubernetes 名前空間、{{site.data.keyword.Bluemix_notm}} 地域、アカウント内の {{site.data.keyword.Bluemix_notm}} または外部プライベート・レジストリーにアクセスするための `imagePullSecret` の作成
{: #other}

独自の `imagePullSecret` を作成して、他の Kubernetes 名前空間にコンテナーをデプロイしたり、他の {{site.data.keyword.Bluemix_notm}} 地域またはアカウント内に保管されているイメージを使用したり、{{site.data.keyword.Bluemix_dedicated_notm}} に保管されているイメージを使用したり、外部プライベート・レジストリーに保管されているイメージを使用したりできます。
{:shortdesc}

ImagePullSecrets は、それらが作成された対象の Kubernetes 名前空間に対してのみ有効です。 コンテナーをデプロイする名前空間ごとに、以下の手順を繰り返してください。 [DockerHub](#dockerhub) のイメージの場合は ImagePullSecrets は必要ありません。
{: tip}

開始前に、以下のことを行います。

1.  [{{site.data.keyword.Bluemix_notm}} Public または {{site.data.keyword.Bluemix_dedicated_notm}} の {{site.data.keyword.registryshort_notm}} に名前空間をセットアップし、その名前空間にイメージをプッシュします](/docs/services/Registry/registry_setup_cli_namespace.html#registry_namespace_add)。
2.  [クラスターを作成します](cs_clusters.html#clusters_cli)。
3.  [CLI のターゲットを自分のクラスターに設定します](cs_cli_install.html#cs_cli_configure)。

<br/>
独自の imagePullSecret を作成するには、以下のいずれかのオプションを選択できます。
- [デフォルトの名前空間からクラスターの他の名前空間に imagePullSecret をコピーする](#copy_imagePullSecret)。
- [他の {{site.data.keyword.Bluemix_notm}} 地域およびアカウントのイメージにアクセスするための imagePullSecret を作成する](#other_regions_accounts)。
- [外部プライベート・レジストリーのイメージにアクセスするための imagePullSecret を作成する](#private_images)。

<br/>
デプロイメントで使用する名前空間に imagePullSecret を既に作成した場合は、[作成済みの imagePullSecret を使用したコンテナーのデプロイ](#use_imagePullSecret)を参照してください。

### デフォルトの名前空間からクラスターの他の名前空間への imagePullSecret のコピー
{: #copy_imagePullSecret}

`default` の Kubernetes 名前空間用に自動的に作成された imagePullSecret を、クラスターの他の名前空間にコピーできます。
{: shortdesc}

1. クラスターで使用可能な名前空間をリストします。
   ```
   kubectl get namespaces
   ```
   {: pre}

   出力例:
   ```
   default          Active    79d
   ibm-cert-store   Active    79d
   ibm-system       Active    79d
   istio-system     Active    34d
   kube-public      Active    79d
   kube-system      Active    79d
   ```
   {: screen}

2. オプション: クラスターに名前空間を作成します。
   ```
   kubectl create namespace <namespace_name>
   ```
   {: pre}

3. `default` の名前空間から任意の名前空間に imagePullSecrets をコピーします。 新しい imagePullSecret の名前は、`bluemix-<namespace_name>-secret-regional` と `bluemix-<namespace_name>-secret-international` になります。
   ```
   kubectl get secret bluemix-default-secret-regional -o yaml | sed 's/default/<namespace_name>/g' | kubectl -n <namespace_name> create -f -
   ```
   {: pre}
   
   ```
   kubectl get secret bluemix-default-secret-international -o yaml | sed 's/default/<namespace_name>/g' | kubectl -n <namespace_name> create -f -
   ```
   {: pre}

4.  シークレットが正常に作成されたことを確認します。
    ```
    kubectl get secrets --namespace <namespace_name>
    ```
    {: pre}

5. その [imagePullSecret を名前空間で使用してコンテナーをデプロイします](#use_imagePullSecret)。


### 他の {{site.data.keyword.Bluemix_notm}} 地域およびアカウントのイメージにアクセスするための imagePullSecret の作成
{: #other_regions_accounts}

他の {{site.data.keyword.Bluemix_notm}} 地域またはアカウントのイメージにアクセスするには、レジストリー・トークンを作成し、資格情報を imagePullSecret に保存する必要があります。
{: shortdesc}

1.  トークンがない場合は、[アクセスするレジストリーのトークンを作成します。](/docs/services/Registry/registry_tokens.html#registry_tokens_create)
2.  {{site.data.keyword.Bluemix_notm}} アカウント内のトークンのリストを表示します。

    ```
    bx cr token-list
    ```
    {: pre}

3.  使用するトークン ID をメモします。
4.  トークンの値を取得します。 <em>&lt;token_ID&gt;</em> を、前のステップで取得したトークンの ID に置き換えます。

    ```
    bx cr token-get <token_id>
    ```
    {: pre}

    CLI 出力の **Token** フィールドに、トークン値が表示されます。

5.  トークン情報を保管する Kubernetes シークレットを作成します。

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_URL> --docker-username=token --docker-password=<token_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>このコマンドの構成要素について</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> このコマンドの構成要素について</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>必須。 シークレットを使用してコンテナーをデプロイする、クラスターの Kubernetes 名前空間。 クラスター内の名前空間をすべてリストするには、<code>kubectl get namespaces</code> を実行します。</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>必須。 imagePullSecret に使用する名前。</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>必須。 名前空間をセットアップするイメージ・レジストリーの URL。<ul><li>米国南部と米国東部でセットアップした名前空間の場合: registry.ng.bluemix.net</li><li>英国南部でセットアップした名前空間の場合: registry.eu-gb.bluemix.net</li><li>中欧 (フランクフルト) でセットアップした名前空間の場合: registry.eu-de.bluemix.net</li><li>オーストラリア (シドニー) でセットアップした名前空間の場合: registry.au-syd.bluemix.net</li><li>{{site.data.keyword.Bluemix_dedicated_notm}} でセットアップした名前空間の場合: registry.<em>&lt;dedicated_domain&gt;</em></li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>必須。 プライベート・レジストリーにログインするためのユーザー名。 {{site.data.keyword.registryshort_notm}} の場合、ユーザー名は <code>token</code> に設定されます。</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>必須。 以前に取得したレジストリー・トークンの値。</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>必須。 Docker E メール・アドレスがある場合は、その値を入力します。 ない場合は、a@b.c のような架空の E メール・アドレスを入力します。この E メールは、Kubernetes シークレットを作成する際には必須ですが、作成後は使用されません。</td>
    </tr>
    </tbody></table>

6.  シークレットが正常に作成されたことを確認します。 <em>&lt;kubernetes_namespace&gt;</em> を、imagePullSecret を作成した名前空間に置き換えます。

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

7.  その [imagePullSecret を名前空間で使用してコンテナーをデプロイします](#use_imagePullSecret)。

### 他のプライベート・レジストリー内に保管されているイメージへのアクセス
{: #private_images}

プライベート・レジストリーが既にある場合は、そのレジストリーの資格情報を Kubernetes imagePullSecret に保管し、構成ファイルからそのシークレットを参照する必要があります。
{:shortdesc}

開始前に、以下のことを行います。

1.  [クラスターを作成します](cs_clusters.html#clusters_cli)。
2.  [CLI のターゲットを自分のクラスターに設定します](cs_cli_install.html#cs_cli_configure)。

imagePullSecret を作成するには、以下のようにします。

1.  プライベート・レジストリーの資格情報を保管する Kubernetes シークレットを作成します。

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_URL> --docker-username=<docker_username> --docker-password=<docker_password> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>このコマンドの構成要素について</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> このコマンドの構成要素について</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>必須。 シークレットを使用してコンテナーをデプロイする、クラスターの Kubernetes 名前空間。 クラスター内の名前空間をすべてリストするには、<code>kubectl get namespaces</code> を実行します。</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>必須。 imagePullSecret に使用する名前。</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>必須。 プライベート・イメージが保管されているレジストリーの URL。</td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>必須。 プライベート・レジストリーにログインするためのユーザー名。</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>必須。 以前に取得したレジストリー・トークンの値。</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>必須。 Docker E メール・アドレスがある場合は、その値を入力します。 ない場合は、例えば a@b.c のような架空の E メール・アドレスを入力します。 この E メールは、Kubernetes シークレットを作成する際には必須ですが、作成後は使用されません。</td>
    </tr>
    </tbody></table>

2.  シークレットが正常に作成されたことを確認します。 <em>&lt;kubernetes_namespace&gt;</em> を、imagePullSecret を作成した名前空間の名前に置き換えます。

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

3.  [imagePullSecret を参照するポッドを作成します](#use_imagePullSecret)。

## 作成済み imagePullSecret を使用したコンテナーのデプロイ
{: #use_imagePullSecret}

ポッド・デプロイメントに imagePullSecret を定義するか、Kubernetes サービス・アカウントに imagePullSecret を保管すると、サービス・アカウントを指定しないすべてのデプロイメントで imagePullSecret を使用できるようになります。
{: shortdesc}

次の選択肢から選択します。
* [ポッド・デプロイメントで imagePullSecret を参照する](#pod_imagePullSecret): 名前空間内のすべてのポッドにデフォルトでレジストリーへのアクセス権限を付与したくない場合は、このオプションを使用します。
* [Kubernetes サービス・アカウントに imagePullSecret を保管する](#store_imagePullSecret): 選択した Kubernetes 名前空間にデプロイするためにレジストリー内のイメージへのアクセス権限を付与する場合は、このオプションを使用します。

開始前に、以下のことを行います。
* [他のレジストリー、Kubernetes 名前空間、{{site.data.keyword.Bluemix_notm}} 地域、またはアカウントのイメージにアクセスするために imagePullSecret を作成します](#other)。
* [CLI のターゲットを自分のクラスターに設定します](cs_cli_install.html#cs_cli_configure)。

### ポッド・デプロイメントで `imagePullSecret` を参照する
{: #pod_imagePullSecret}

ポッド・デプロイメントで imagePullSecret を参照する場合、その imagePullSecret はそのポッドでのみ有効です。名前空間内の複数のポッドで共有することはできません。
{:shortdesc}

1.  `mypod.yaml` という名前のポッド構成ファイルを作成します。
2.  ポッドと、プライベート {{site.data.keyword.registrylong_notm}} にアクセスするための imagePullSecret を定義します。

    プライベート・イメージにアクセスするには、以下のようにします。
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
        - name: <container_name>
          image: registry.<region>.bluemix.net/<namespace_name>/<image_name>:<tag>
      imagePullSecrets:
        - name: <secret_name>
    ```
    {: codeblock}

    {{site.data.keyword.Bluemix_notm}} パブリック・イメージにアクセスするには、以下のようにします。
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
        - name: <container_name>
          image: registry.bluemix.net/<image_name>:<tag>
      imagePullSecrets:
        - name: <secret_name>
    ```
    {: codeblock}

    <table>
    <caption>YAML ファイルの構成要素について</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;container_name&gt;</em></code></td>
    <td>クラスターにデプロイするコンテナーの名前。</td>
    </tr>
    <tr>
    <td><code><em>&lt;namespace_name&gt;</em></code></td>
    <td>イメージが保管されている名前空間。 使用可能な名前空間をリストするには、`bx cr namespace-list` を実行します。</td>
    </tr>
    <tr>
    <td><code><em>&lt;image_name&gt;</em></code></td>
    <td>使用するイメージの名前。 {{site.data.keyword.Bluemix_notm}} アカウント内の使用可能なイメージをリストするには、`bx cr image-list` を実行します。</td>
    </tr>
    <tr>
    <td><code><em>&lt;tag&gt;</em></code></td>
    <td>使用するイメージのバージョン。 タグを指定しないと、デフォルトでは <strong>latest</strong> のタグが付いたイメージが使用されます。</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>以前に作成した imagePullSecret の名前。</td>
    </tr>
    </tbody></table>

3.  変更を保存します。
4.  クラスター内にデプロイメントを作成します。
    ```
    kubectl apply -f mypod.yaml
    ```
    {: pre}

### 選択した名前空間の Kubernetes サービス・アカウントに imagePullSecret を保管する
{:#store_imagePullSecret}

すべての名前空間に、`default` という名前の Kubernetes サービス・アカウントがあります。 imagePullSecret をこのサービス・アカウントに追加すると、レジストリー内のイメージへのアクセス権限を付与できます。 サービス・アカウントを指定しないデプロイメントでは、その名前空間の `default` サービス・アカウントが自動的に使用されます。
{:shortdesc}

1. default サービス・アカウントに imagePullSecret が既に存在しているかどうかを確認します。
   ```
   kubectl describe serviceaccount default -n <namespace_name>
   ```
   {: pre}
   **「Image pull secrets」**項目に `<none>` と表示された場合、imagePullSecret は存在しません。  
2. imagePullSecret を default サービス・アカウントに追加します。
   - **imagePullSecret が定義されていない場合に imagePullSecret を追加するには、以下のようにします。**
       ```
       kubectl patch -n <namespace_name> serviceaccount/default -p '{"imagePullSecrets":[{"name": "bluemix-<namespace_name>-secret-regional"}]}'
       ```
       {: pre}
   - **imagePullSecret が既に定義されている場合に imagePullSecret を追加するには、以下のようにします。**
       ```
       kubectl patch -n <namespace_name> serviceaccount/default --type='json' -p='[{"op":"add","path":"/imagePullSecrets/-","value":{"name":"bluemix-<namespace_name>-secret-regional"}}]'
       ```
       {: pre}
3. imagePullSecret が default サービス・アカウントに追加されたことを確認します。
   ```
   kubectl describe serviceaccount default -n <namespace_name>
   ```
   {: pre}

   出力例:
   ```
   Name:                default
   Namespace:           <namespace_name>
   Labels:              <none>
   Annotations:         <none>
   Image pull secrets:  bluemix-namespace_name-secret-regional
   Mountable secrets:   default-token-sh2dx
   Tokens:              default-token-sh2dx
   Events:              <none>
   ```
   {: pre}

4. レジストリーのイメージからコンテナーをデプロイします。
   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: mypod
   spec:
     containers:
       - name: <container_name>
         image: registry.<region>.bluemix.net/<namespace_name>/<image_name>:<tag>
   ```
   {: codeblock}

5. クラスター内にデプロイメントを作成します。
   ```
   kubectl apply -f mypod.yaml
   ```
   {: pre}

<br />


