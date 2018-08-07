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



# チュートリアル: Cloud Foundry からクラスターへのアプリのマイグレーション
{: #cf_tutorial}

Cloud Foundry を使用して以前にデプロイしたアプリを取得し、コンテナー内の同じコードを {{site.data.keyword.containershort_notm}} 内の Kubernetes クラスターにデプロイできます。
{: shortdesc}


## 達成目標

- コンテナー内のアプリを Kubernetes クラスターにデプロイする一般的なプロセスを学習します。
- コンテナー・イメージを構築するための Dockerfile をアプリ・コードから作成します。
- そのイメージからコンテナーを Kubernetes クラスターにデプロイします。

## 所要時間
30 分

## 対象読者
このチュートリアルは、Cloud Foundry アプリの開発者を対象にしています。

## 前提条件

- [プライベート・イメージ・レジストリーを {{site.data.keyword.registrylong_notm}} に作成します](../services/Registry/index.html)。
- [クラスターを作成します](cs_clusters.html#clusters_ui)。
- [CLI のターゲットを自分のクラスターに設定します](cs_cli_install.html#cs_cli_configure)。
- [Docker および Kubernetes の用語について学習します](cs_tech.html)。


<br />



## レッスン 1: アプリのコードをダウンロードする

コードを準備します。 コードを持っていない場合は、 このチュートリアルで使用するスターター・コードをダウンロードできます。
{: shortdesc}

1. `cf-py` という名前のディレクトリーを作成し、その中にナビゲートします。 このディレクトリーには、Docker イメージをビルドし、アプリを実行するために必要なすべてのファイルを保存します。

  ```
  mkdir cf-py && cd cf-py
  ```
  {: pre}

2. このディレクトリーに、アプリのコード・ファイルと、すべての関連ファイルをコピーします。 独自のアプリのコードを使用することも、カタログからボイラープレートをダウンロードすることもできます。 このチュートリアルでは、Python Flask ボイラープレートを使用します。 ただし、Node.js、Java、[Kitura](https://github.com/IBM-Cloud/Kitura-Starter) アプリでも同じ基本手順を使用できます。

    Python Flask アプリ・コードをダウンロードするには、以下のようにします。

    a. カタログの**ボイラープレート**で、**Python Flask** をクリックします。 このボイラープレートには、Python 2 と Python 3 の両方のアプリ用のランタイム環境が含まれています。

    b. アプリ名 `cf-py-<name>` を入力して、**「作成」**をクリックします。 ボイラープレートのアプリ・コードにアクセスするには、まず CF アプリをクラウドにデプロイする必要があります。 アプリには任意の名前を使用できます。 例にある名前を使用する場合は、`<name>` を固有 ID で置き換えます (例: `cf-py-msx`)。
    
    **注意**: アプリ、コンテナー・イメージ、Kubernetes リソースの名前には、個人情報を使用しないでください。

    アプリをデプロイすると、「コマンド・ライン・インターフェースでアプリをダウンロード、変更、および再デプロイする」ための指示が表示されます。

    c. GUI の指示のステップ 1 で、**「スターター・コードをダウンロードする」**をクリックします。

    d. .zip ファイルを解凍し、その内容を `cf-py` ディレクトリーに保存します。

これで、アプリのコードをコンテナー化する用意ができました。


<br />



## レッスン 2: アプリのコードを使用して Docker イメージを作成する

アプリのコードとコンテナーに必要な構成を含む Dockerfile を作成します。 その後、その Dockerfile から Docker イメージをビルドし、それをプライベート・イメージ・レジストリーにプッシュします。
{: shortdesc}

1. 直前のレッスンで作成した `cf-py` ディレクトリーに、コンテナー・イメージを作成するための基礎となる `Dockerfile` を作成します。 コンピューター上の任意の CLI エディターまたはテキスト・エディターを使用して、Dockerfile を作成できます。 次の例は、nano editor を使用して Dockerfile ファイルを作成する方法を示しています。

  ```
  nano Dockerfile
  ```
  {: pre}

2. 次のスクリプトを Dockerfile にコピーします。 この Dockerfile は Python アプリだけに適用されます。 別の種類のコードを使用する場合は、Dockerfile に別の基本イメージが含まれている必要があります。また、他のフィールドを定義する必要がある可能性があります。

  ```
  #Use the Python image from DockerHub as a base image
  FROM python:3-slim

  #Expose the port for your python app
  EXPOSE 5000

  #Copy all app files from the current directory into the app
  #directory in your container. Set the app directory
  #as the working directory
  WORKDIR /cf-py/
  COPY .  .

  #Install any requirements that are defined
  RUN pip install --no-cache-dir -r requirements.txt

  #Update the openssl package
  RUN apt-get update && apt-get install -y \
  openssl

  #Start the app.
  CMD ["python", "welcome.py"]
  ```
  {: codeblock}

3. `Ctrl + o` を押して、変更内容を nano エディターに保存します。 `enter` を押して、変更を確認します。 `ctrl + x` を押して、nano エディターを終了します。

4. アプリ・コードを含むイメージを作成し、それを専用レジストリーにプッシュします。

  ```
  bx cr build -t registry.<region>.bluemix.net/namespace/cf-py .
  ```
  {: pre}

  <table>
  <caption>このコマンドの構成要素について</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="このアイコンは、このコマンドの構成要素の詳細を表示できることを示しています。"/> このコマンドの構成要素について理解する</th>
  </thead>
  <tbody>
  <tr>
  <td>パラメーター</td>
  <td>説明</td>
  </tr>
  <tr>
  <td><code>build</code></td>
  <td>build コマンド。</td>
  </tr>
  <tr>
  <td><code>-t registry.&lt;region&gt;.bluemix.net/namespace/cf-py</code></td>
  <td>専用  レジストリーのパス。これには、固有の名前空間とイメージの名前が含まれます。 この例では、イメージの名前にアプリのディレクトリーと同じ名前を使用していますが、専用レジストリーのイメージには任意の名前を選択できます。 名前空間がわからない場合は、`bx cr namespaces` コマンドを実行して調べることができます。</td>
  </tr>
  <tr>
  <td><code>.</code></td>
  <td>Dockerfile の場所。 Dockerfile の入ったディレクトリーからビルド・コマンドを実行している場合は、ピリオド (.) を入力します。 その他の場合は、Dockerfile への相対パスを使用します。</td>
  </tr>
  </tbody>
  </table>

  プライベート・レジストリー内にイメージが作成されます。 `bx cr images` コマンドを実行すると、イメージが作成されたことを確認できます。

  ```
  REPOSITORY                                     NAMESPACE   TAG      DIGEST         CREATED         SIZE     VULNERABILITY STATUS   
  registry.ng.bluemix.net/namespace/cf-py        namespace   latest   cb03170b2cb2   3 minutes ago   271 MB   OK
  ```
  {: screen}


<br />



## レッスン 3: イメージからコンテナーをデプロイする

アプリを Kubernetes クラスターのコンテナーとしてデプロイします。
{: shortdesc}

1. `cf-py.yaml` という名前の構成 YAML ファイルを作成し、`<registry_namespace>` をプライベート・イメージ・レジストリーの名前に更新します。 この構成ファイルは、直前のレッスンで作成したイメージからのコンテナー・デプロイメント、およびアプリを一般に公開するためのサービスを定義します。

  ```
  apiVersion: extensions/v1beta1
  kind: Deployment
  metadata:
    labels:
      app: cf-py
    name: cf-py
    namespace: default
  spec:
    selector:
      matchLabels:
        app: cf-py
    replicas: 1
    template:
      metadata:
        labels:
          app: cf-py
      spec:
        containers:
        - image: registry.ng.bluemix.net/<registry_namespace>/cf-py:latest
          name: cf-py
  ---
  apiVersion: v1
  kind: Service
  metadata:
    name: cf-py-nodeport
    labels:
      app: cf-py
  spec:
    selector:
      app: cf-py
    type: NodePort
    ports:
     - port: 5000
       nodePort: 30872
  ```
  {: codeblock}

  <table>
  <caption>YAML ファイルの構成要素について</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
  </thead>
  <tbody>
  <tr>
  <td><code>イメージ</code></td>
  <td>`registry.ng.bluemix.net/<registry_namespace>/cf-py:latest` で、&lt;registry_namespace&gt; をプライベート・イメージ・レジストリーの名前空間に置き換えます。 名前空間がわからない場合は、`bx cr namespaces` コマンドを実行して調べることができます。</td>
  </tr>
  <tr>
  <td><code>nodePort</code></td>
  <td>NodePort タイプの Kubernetes サービスを作成してアプリを公開します。 NodePort は 30000 から 32767 までの範囲にあります。 このポートを後でブラウザーで使用してアプリをテストします。</td>
  </tr>
  </tbody></table>

2. 構成ファイルを適用して、デプロイメントおよびサービスをクラスター内に作成します。

  ```
  kubectl apply -f <filepath>/cf-py.yaml
  ```
  {: pre}

  出力:

  ```
  deployment "cf-py" configured
  service "cf-py-nodeport" configured
  ```
  {: screen}

3. デプロイメントの作業がすべて完了したので、ブラウザーでアプリをテストできます。 URL を作成するための詳細情報を取得します。

    a.  クラスター内のワーカー・ノードのパブリック IP アドレスを取得します。

    ```
    bx cs workers <cluster_name>
    ```
    {: pre}

    出力:

    ```
    ID                                                 Public IP        Private IP     Machine Type        State    Status   Zone    Version   
    kube-dal10-cr18e61e63c6e94b658596ca93d087eed9-w1   169.xx.xxx.xxx   10.xxx.xx.xxx   u2c.2x4.encrypted   normal   Ready    dal10   1.9.7
    ```
    {: screen}

    b. ブラウザーを開き、`http://<public_IP_address>:<NodePort>` という形式の URL でアプリを確認します。 この例の値を使用した場合、URL は `http://169.xx.xxx.xxx:30872` になります。 その URL を同僚に伝えて試してもらうか、自分のスマートフォンなどのブラウザーにそれを入力することによって、だれでもアプリを利用できることを確認できます。

    <img src="images/python_flask.png" alt="デプロイされたボイラープレート Python Flask アプリの画面キャプチャー。" />

5.  [Kubernetes ダッシュボードを起動](cs_app.html#cli_dashboard)します。

    [{{site.data.keyword.Bluemix_notm}} GUI](https://console.bluemix.net/) でクラスターを選択した場合は、**「Kubernetes ダッシュボード (Kubernetes Dashboard)」**ボタンを使用して、1 回のクリックでダッシュボードを起動できます。
    {: tip}

6. **「ワークロード」**タブで、作成したリソースを表示します。

これで完了です。 アプリがコンテナーにデプロイされました。
