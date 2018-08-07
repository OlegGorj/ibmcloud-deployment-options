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





# {{site.data.keyword.Bluemix_dedicated_notm}} イメージ・レジストリー用の {{site.data.keyword.registryshort_notm}} トークンの作成
{: #cs_dedicated_tokens}

{{site.data.keyword.containerlong}} 内のクラスターで単一グループとスケーラブル・グループに使用したイメージ・レジストリー用の無期限トークンを作成します。
{:shortdesc}

1.  {{site.data.keyword.Bluemix_dedicated_notm}} 環境にログインします。

    ```
    bx login -a api.<dedicated_domain>
    ```
    {: pre}

2.  現行セッションの `oauth-token` を要求し、変数として保存します。

    ```
    OAUTH_TOKEN=`bx iam oauth-tokens | awk 'FNR == 2 {print $3 " " $4}'`
    ```
    {: pre}

3.  現行セッションの組織の ID を要求し、変数として保存します。

    ```
    ORG_GUID=`bx iam org <org_name> --guid`
    ```
    {: pre}

4.  現行セッションの永続レジストリー・トークンを要求します。 <dedicated_domain> を、ご使用の {{site.data.keyword.Bluemix_dedicated_notm}} 環境のドメインに置き換えてください。 このトークンは、現在の名前空間内のイメージへのアクセス権限を付与します。

    ```
    curl -XPOST -H "Authorization: ${OAUTH_TOKEN}" -H "Organization: ${ORG_GUID}" https://registry.<dedicated_domain>/api/v1/tokens?permanent=true
    ```
    {: pre}

    出力:

    ```
    {
        "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI2MzdiM2Q4Yy1hMDg3LTVhZjktYTYzNi0xNmU3ZWZjNzA5NjciLCJpc3MiOiJyZWdpc3RyeS5jZnNkZWRpY2F0ZWQxLnVzLXNvdXRoLmJsdWVtaXgubmV0"
    }
    ```
    {: screen}

5.  Kubernetes シークレットを確認します。

    ```
    kubectl describe secrets
    ```
    {: pre}

    このシークレットにより、{{site.data.keyword.containerlong}} を使用することができます。

6.  トークン情報を保管する Kubernetes シークレットを作成します。

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_url> --docker-username=token --docker-password=<token_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>このコマンドの構成要素について</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> このコマンドの構成要素について</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace &lt;kubernetes_namespace&gt;</code></td>
    <td>必須。 シークレットを使用してコンテナーをデプロイする、クラスターの Kubernetes 名前空間。 クラスター内の名前空間をすべてリストするには、<code>kubectl get namespaces</code> を実行します。</td>
    </tr>
    <tr>
    <td><code>&lt;secret_name&gt;</code></td>
    <td>必須。 imagePullSecret に使用する名前。</td>
    </tr>
    <tr>
    <td><code>--docker-server &lt;registry_url&gt;</code></td>
    <td>必須。 名前空間がセットアップされているイメージ・レジストリーの URL: registry.&lt;dedicated_domain&gt;</li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username &lt;docker_username&gt;</code></td>
    <td>必須。 プライベート・レジストリーにログインするためのユーザー名。</td>
    </tr>
    <tr>
    <td><code>--docker-password &lt;token_value&gt;</code></td>
    <td>必須。 以前に取得したレジストリー・トークンの値。</td>
    </tr>
    <tr>
    <td><code>--docker-email &lt;docker-email&gt;</code></td>
    <td>必須。 Docker E メール・アドレスがある場合は、その値を入力します。 ない場合は、例えば a@b.c のような架空の E メール・アドレスを入力します。 この E メールは、Kubernetes シークレットを作成する際には必須ですが、作成後は使用されません。</td>
    </tr>
    </tbody></table>

7.  imagePullSecret を参照するポッドを作成します。

    1.  任意のテキスト・エディターを開き、mypod.yaml という名前のポッド構成スクリプトを作成します。
    2.  レジストリーへのアクセスに使用するポッドと imagePullSecret を定義します。 名前空間からプライベート・イメージを使用するには、次のようにします。

        ```
        apiVersion: v1
        kind: Pod
        metadata:
          name: <pod_name>
        spec:
          containers:
            - name: <container_name>
              image: registry.<dedicated_domain>/<my_namespace>/<my_image>:<tag>
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
        <td><code>&lt;pod_name&gt;</code></td>
        <td>作成するポッドの名前。</td>
        </tr>
        <tr>
        <td><code>&lt;container_name&gt;</code></td>
        <td>クラスターにデプロイするコンテナーの名前。</td>
        </tr>
        <tr>
        <td><code>&lt;my_namespace&gt;</code></td>
        <td>イメージが保管されている名前空間。 使用可能な名前空間をリストするには、`bx cr namespace-list` を実行します。</td>
        </tr>
        <td><code>&lt;my_image&gt;</code></td>
        <td>使用するイメージの名前。 {{site.data.keyword.Bluemix_notm}} アカウント内の使用可能なイメージをリストするには、<code>bx cr image-list</code> を実行します。</td>
        </tr>
        <tr>
        <td><code>&lt;tag&gt;</code></td>
        <td>使用するイメージのバージョン。 タグを指定しないと、デフォルトでは <strong>latest</strong> のタグが付いたイメージが使用されます。</td>
        </tr>
        <tr>
        <td><code>&lt;secret_name&gt;</code></td>
        <td>以前に作成した imagePullSecret の名前。</td>
        </tr>
        </tbody></table>

    3.  変更を保存します。

    4.  クラスター内にデプロイメントを作成します。

          ```
          kubectl apply -f mypod.yaml
          ```
          {: pre}

