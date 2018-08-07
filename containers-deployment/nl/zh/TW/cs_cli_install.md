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





# 設定 CLI 及 API
{: #cs_cli_install}

您可以使用 {{site.data.keyword.containerlong}} CLI 或 API 來建立及管理 Kubernetes 叢集。
{:shortdesc}

<br />


## 安裝 CLI
{: #cs_cli_install_steps}

安裝必要 CLI 以在 {{site.data.keyword.containershort_notm}} 中建立及管理 Kubernetes 叢集，並且將容器化應用程式部署至叢集。
{:shortdesc}

此作業包括安裝這些 CLI 及外掛程式的資訊：

-   {{site.data.keyword.Bluemix_notm}} CLI 0.5.0 版或更新版本
-   {{site.data.keyword.containershort_notm}} 外掛程式
-   符合叢集 `major.minor` 版本的 Kubernetes CLI 版本
-   選用項目：{{site.data.keyword.registryshort_notm}} 外掛程式
-   選用項目：Docker 1.9 版或更新版本

<br>
若要安裝 CLI，請執行下列動作：



1.  安裝 [{{site.data.keyword.Bluemix_notm}} CLI ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://clis.ng.bluemix.net/ui/home.html)，它是 {{site.data.keyword.containershort_notm}} 外掛程式的必要條件。使用 {{site.data.keyword.Bluemix_notm}} CLI 來執行指令的字首是 `bx`。

2.  登入 {{site.data.keyword.Bluemix_notm}} CLI。系統提示時，請輸入您的 {{site.data.keyword.Bluemix_notm}} 認證。

    ```
          bx login
      ```
    {: pre}

    **附註：**如果您具有聯合 ID，請使用 `bx login --sso` 來登入 {{site.data.keyword.Bluemix_notm}} CLI。請輸入使用者名稱，並使用 CLI 輸出中提供的 URL 來擷取一次性密碼。若沒有 `--sso` 時登入失敗，而有 `--sso` 選項時登入成功，即表示您有聯合 ID。

3.  若要建立 Kubernetes 叢集，以及管理工作者節點，請安裝 {{site.data.keyword.containershort_notm}} 外掛程式。使用 {{site.data.keyword.containershort_notm}} 外掛程式來執行指令的字首是 `bx cs`。

    ```
        bx plugin install container-service -r Bluemix
    ```
    {: pre}

    若要驗證已適當安裝外掛程式，請執行下列指令：

    ```
            bx plugin list
        ```
    {: pre}

    在結果中，{{site.data.keyword.containershort_notm}} 外掛程式會顯示為 container-service。

4.  {: #kubectl}若要檢視本端版本的 Kubernetes 儀表板，以及將應用程式部署至叢集，請[安裝 Kubernetes CLI ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/tasks/tools/install-kubectl/)。使用 Kubernetes CLI 來執行指令的字首是 `kubectl`。

    1.  下載與您計劃使用的 Kubernetes 叢集 `major.minor` 版本相符的 Kubernetes CLI `major.minor` 版本。現行 {{site.data.keyword.containershort_notm}} 預設 Kubernetes 版本為 1.9.7。**附註**：如果您使用的 `kubectl` CLI 版本未至少符合叢集的 `major.minor` 版本，則您可能會看到非預期的結果。請確定 Kubernetes 叢集及 CLI 版本保持最新。

        - **OS X**：[https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/darwin/amd64/kubectl ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/darwin/amd64/kubectl)
        - **Linux**：[https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/linux/amd64/kubectl ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/linux/amd64/kubectl)
        - **Windows**：[https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/windows/amd64/kubectl.exe ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/windows/amd64/kubectl.exe)

    2.  **若為 OSX 及 Linux**：請完成下列步驟。
        1.  將執行檔移至 `/usr/local/bin` 目錄。

            ```
                        mv /filepath/kubectl /usr/local/bin/kubectl
            ```
            {: pre}

        2.  確定 `/usr/local/bin` 列在 `PATH` 系統變數中。`PATH` 變數包含作業系統可在其中找到執行檔的所有目錄。`PATH` 變數中所列的目錄用於不同的用途。`/usr/local/bin` 是用來儲存軟體的執行檔，該軟體不是作業系統的一部分，並且已由系統管理者手動安裝。

            ```
                        echo $PATH
            ```
            {: pre}

            CLI 輸出範例：

            ```
                        /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
            ```
            {: screen}

        3.  使檔案成為可執行檔。

            ```
                        chmod +x /usr/local/bin/kubectl
            ```
            {: pre}

    3.  **若為 Windows**：請將 Kubernetes CLI 安裝在與 {{site.data.keyword.Bluemix_notm}} CLI 相同的目錄中。當您稍後執行指令時，此設定可為您省去一些檔案路徑變更。

5.  若要管理專用映像檔儲存庫，請安裝 {{site.data.keyword.registryshort_notm}} 外掛程式。您可以使用此外掛程式，在多方承租戶、高可用性並且由 IBM 所管理的可擴充專用映像檔登錄中設定您自己的名稱空間，以及儲存 Docker 映像檔，並將其與其他使用者共用。需要有 Docker 映像檔，才能將容器部署至叢集。執行登錄指令的字首是 `bx cr`。

    ```
        bx plugin install container-registry -r Bluemix
    ```
    {: pre}

    若要驗證已適當安裝外掛程式，請執行下列指令：

    ```
            bx plugin list
        ```
    {: pre}

    在結果中，外掛程式會顯示為 container-registry。

6.  若要在本端建置映像檔，並將它們推送至您的登錄名稱空間，請[安裝 Docker ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.docker.com/community-edition#/download)。如果您使用的是 Windows 8 或更早版本，則可以改為安裝 [Docker Toolbox ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.docker.com/toolbox/toolbox_install_windows/)。Docker CLI 是用來將應用程式建置成映像檔。使用 Docker CLI 來執行指令的字首是 `docker`。

接下來，開始[使用 {{site.data.keyword.containershort_notm}} 從 CLI 建立 Kubernetes 叢集](cs_clusters.html#clusters_cli)。

如需這些 CLI 的相關參考資訊，請參閱那些工具的文件。

-   [`bx` 指令](/docs/cli/reference/bluemix_cli/bx_cli.html)
-   [`bx cs` 指令](cs_cli_reference.html#cs_cli_reference)
-   [`kubectl` 指令 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/reference/kubectl/overview/)
-   [`bx cr` 指令](/docs/cli/plugins/registry/index.html)

<br />




## 在電腦上的容器中執行 CLI
{: #cs_cli_container}

不是在您的電腦上個別安裝每一個 CLI，而是您可以將 CLI 安裝至在您電腦上執行的容器。
{:shortdesc}

1. 從提供的 Dockerfile 建立映像檔。

    ```
    docker build -t <image_name> https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/install-clis-container/Dockerfile
    ```
    {: pre}

2. 在本端部署映像檔作為容器，並裝載磁區來存取本端檔案。
  
    ```
    docker run -it -v /local/path:/container/volume <image_name>
    ```
    {: pre}
  
3. 從互動式 Shell 開始執行 `bx cs` 及 `kubectl` 指令。如果您建立了要儲存的資料，請將該資料儲存至您所裝載的磁區。當您結束 Shell 時，容器就會停止。 

<br />



## 配置 CLI 以執行 `kubectl`
{: #cs_cli_configure}

您可以使用 Kubernetes CLI 隨附的指令來管理 {{site.data.keyword.Bluemix_notm}} 中的叢集。
{:shortdesc}

支援 Kubernetes 1.9.7 中所有可用的 `kubectl` 指令與 {{site.data.keyword.Bluemix_notm}} 中的叢集搭配使用。建立叢集之後，使用環境變數將本端 CLI 的環境定義設定為該叢集。然後，您可以執行 Kubernetes `kubectl` 指令，在 {{site.data.keyword.Bluemix_notm}} 中使用您的叢集。


您必須先[安裝必要的 CLI](#cs_cli_install) 及[建立叢集](cs_clusters.html#clusters_cli)，才能執行 `kubectl` 指令。

1.  登入 {{site.data.keyword.Bluemix_notm}} CLI。系統提示時，請輸入您的 {{site.data.keyword.Bluemix_notm}} 認證。若要指定 {{site.data.keyword.Bluemix_notm}} 地區，請[包括 API 端點](cs_regions.html#bluemix_regions)。

    ```
          bx login
      ```
    {: pre}

    **附註：**如果您具有聯合 ID，請使用 `bx login --sso` 來登入 {{site.data.keyword.Bluemix_notm}} CLI。請輸入使用者名稱，並使用 CLI 輸出中提供的 URL 來擷取一次性密碼。若沒有 `--sso` 時登入失敗，而有 `--sso` 選項時登入成功，即表示您有聯合 ID。

2.  選取 {{site.data.keyword.Bluemix_notm}} 帳戶。如果您被指派到多個 {{site.data.keyword.Bluemix_notm}} 組織，請選取在其中建立叢集的組織。叢集是組織特有的，但與 {{site.data.keyword.Bluemix_notm}} 空間無關。因此，您不需要選取空間。

3.  若要在先前所選取 {{site.data.keyword.Bluemix_notm}} 地區以外的地區中建立或存取 Kubernetes 叢集，請執行 `bx cs region-set`。

4.  列出帳戶中的所有叢集，以取得叢集的名稱。

    ```
          bx cs clusters
      ```
    {: pre}

5.  將您建立的叢集設為此階段作業的環境定義。請在您每次使用叢集時，完成下列配置步驟。
    1.  取得指令來設定環境變數，並下載 Kubernetes 配置檔。

        ```
            bx cs cluster-config <cluster_name_or_ID>
    ```
        {: pre}

        下載配置檔之後，會顯示一個指令，可讓您用來將本端 Kubernetes 配置檔的路徑設定為環境變數。

        範例：

        ```
            export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
    ```
        {: screen}

    2.  複製並貼上終端機中顯示的指令，以設定 `KUBECONFIG` 環境變數。

    3.  驗證 `KUBECONFIG` 環境變數已適當設定。

        範例：

        ```
                  echo $KUBECONFIG
          ```
        {: pre}

        輸出：
        ```
        /Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

6.  檢查 Kubernetes CLI 伺服器版本，驗證叢集已適當地執行 `kubectl` 指令。

    ```
    kubectl version  --short
    ```
    {: pre}

    輸出範例：

    ```
    Client Version: v1.9.7
    Server Version: v1.9.7
    ```
    {: screen}

您現在可以執行 `kubectl` 指令，以在 {{site.data.keyword.Bluemix_notm}} 中管理叢集。如需完整指令清單，請參閱 [Kubernetes 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/reference/kubectl/overview/)。

**提示：**如果您使用的是 Windows，但未在與 {{site.data.keyword.Bluemix_notm}} CLI 相同的目錄中安裝 Kubernetes CLI，則必須將目錄切換至 Kubernetes CLI 安裝所在的路徑，才能順利執行 `kubectl` 指令。


<br />


## 更新 CLI
{: #cs_cli_upgrade}

您可能想要定期更新 CLI，以使用新特性。
{:shortdesc}

此作業包括更新這些 CLI 的資訊。

-   {{site.data.keyword.Bluemix_notm}} CLI 0.5.0 版或更新版本
-   {{site.data.keyword.containershort_notm}} 外掛程式
-   Kubernetes CLI 1.9.7 版或更新版本
-   {{site.data.keyword.registryshort_notm}} 外掛程式
-   Docker 1.9 版或更新版本

<br>
若要更新 CLI，請執行下列動作：



1.  更新 {{site.data.keyword.Bluemix_notm}} CLI。下載[最新版本 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://clis.ng.bluemix.net/ui/home.html)，並執行安裝程式。

2. 登入 {{site.data.keyword.Bluemix_notm}} CLI。系統提示時，請輸入您的 {{site.data.keyword.Bluemix_notm}} 認證。若要指定 {{site.data.keyword.Bluemix_notm}} 地區，請[包括 API 端點](cs_regions.html#bluemix_regions)。

    ```
    bx login
    ```
    {: pre}

     **附註：**如果您具有聯合 ID，請使用 `bx login --sso` 來登入 {{site.data.keyword.Bluemix_notm}} CLI。請輸入使用者名稱，並使用 CLI 輸出中提供的 URL 來擷取一次性密碼。若沒有 `--sso` 時登入失敗，而有 `--sso` 選項時登入成功，即表示您有聯合 ID。

3.  更新 {{site.data.keyword.containershort_notm}} 外掛程式。
    1.  從 {{site.data.keyword.Bluemix_notm}} 外掛程式儲存庫中安裝更新。

        ```
        bx plugin update container-service -r Bluemix
        ```
        {: pre}

    2.  執行下列指令，並檢查已安裝的外掛程式清單，以驗證外掛程式安裝。

        ```
        bx plugin list
        ```
        {: pre}

        在結果中，{{site.data.keyword.containershort_notm}} 外掛程式會顯示為 container-service。

    3.  起始設定 CLI。

        ```
        bx cs init
        ```
        {: pre}

4.  [更新 Kubernetes CLI](#kubectl)。

5.  更新 {{site.data.keyword.registryshort_notm}} 外掛程式。
    1.  從 {{site.data.keyword.Bluemix_notm}} 外掛程式儲存庫中安裝更新。

        ```
        bx plugin update container-registry -r Bluemix
        ```
        {: pre}

    2.  執行下列指令，並檢查已安裝的外掛程式清單，以驗證外掛程式安裝。

        ```
        bx plugin list
        ```
        {: pre}

        在結果中，登錄外掛程式會顯示為 container-registry。

6.  更新 Docker。
    -   如果您使用 Docker Community Edition，請啟動 Docker、按一下 **Docker** 圖示，然後按一下 **Check for updates**。
    -   如果您使用 Docker Toolbox，請下載[最新版本 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.docker.com/toolbox/toolbox_install_windows/)，並執行安裝程式。

<br />


## 解除安裝 CLI
{: #cs_cli_uninstall}

如果不再需要 CLI，則可以將它解除安裝。
{:shortdesc}

此作業包括移除這些 CLI 的資訊：


-   {{site.data.keyword.containershort_notm}} 外掛程式
-   Kubernetes CLI
-   {{site.data.keyword.registryshort_notm}} 外掛程式
-   Docker 1.9 版或更新版本

<br>
若要解除安裝 CLI，請執行下列動作：

1.  解除安裝 {{site.data.keyword.containershort_notm}} 外掛程式。

    ```
    bx plugin uninstall container-service
    ```
    {: pre}

2.  解除安裝 {{site.data.keyword.registryshort_notm}} 外掛程式。

    ```
    bx plugin uninstall container-registry
    ```
    {: pre}

3.  執行下列指令，並檢查已安裝的外掛程式清單，以驗證已解除安裝外掛程式。

    ```
    bx plugin list
    ```
    {: pre}

    container-service 及 container-registry 外掛程式不會顯示在結果中。

6.  解除安裝 Docker。根據您使用的作業系統，解除安裝 Docker 的指示會不同。

    - [OSX ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.docker.com/docker-for-mac/#uninstall-or-reset)
    - [Linux ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/#uninstall-docker-ce)
    - [Windows ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.docker.com/toolbox/toolbox_install_windows/#how-to-uninstall-toolbox)

<br />


## 使用 API 自動化進行叢集部署
{: #cs_api}

您可以使用 {{site.data.keyword.containershort_notm}} API 來自動化進行 Kubernetes 叢集的建立、部署及管理。
{:shortdesc}

{{site.data.keyword.containershort_notm}} API 需要標頭資訊，您必須在 API 要求中提供它，且它會視您要使用的 API 而變。若要判斷 API 所需的標頭資訊，請參閱 [{{site.data.keyword.containershort_notm}} API 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://us-south.containers.bluemix.net/swagger-api)。

**附註：**若要向 {{site.data.keyword.containershort_notm}} 進行鑑別，您必須提供以 {{site.data.keyword.Bluemix_notm}} 認證產生且包含建立叢集所在 {{site.data.keyword.Bluemix_notm}} 帳戶 ID 的「身分及存取管理 (IAM)」記號。根據您向 {{site.data.keyword.Bluemix_notm}} 進行鑑別的方式，您可以在下列選項之間進行選擇，以自動建立 IAM 記號。

<table>
<caption>ID 類型和選項</caption>
<thead>
<th>{{site.data.keyword.Bluemix_notm}} ID</th>
<th>我的選項</th>
</thead>
<tbody>
<tr>
<td>未聯合 ID</td>
<td><ul><li><strong>{{site.data.keyword.Bluemix_notm}} 使用者名稱及密碼：</strong>您可以遵循本主題的步驟，以便完全自動建立 IAM 存取記號。</li>
<li><strong>產生 {{site.data.keyword.Bluemix_notm}} API 金鑰：</strong>作為使用 {{site.data.keyword.Bluemix_notm}} 使用者名稱和密碼的替代方案，您可以<a href="../iam/apikeys.html#manapikey" target="_blank">使用 {{site.data.keyword.Bluemix_notm}} API 金鑰</a> {{site.data.keyword.Bluemix_notm}} API 金鑰相依於為其產生金鑰的 {{site.data.keyword.Bluemix_notm}} 帳戶。您無法將 {{site.data.keyword.Bluemix_notm}} API 金鑰與不同帳戶 ID 結合在相同 IAM 記號中。若要存取使用您 {{site.data.keyword.Bluemix_notm}} API 金鑰根據帳戶以外之帳戶建立的叢集，您必須登入帳戶才能產生新的 API 金鑰。</li></ul></tr>
<tr>
<td>聯合 ID</td>
<td><ul><li><strong>產生 {{site.data.keyword.Bluemix_notm}} API 金鑰：</strong><a href="../iam/apikeys.html#manapikey" target="_blank">{{site.data.keyword.Bluemix_notm}} API 金鑰</a>金鑰相依於為其產生金鑰的 {{site.data.keyword.Bluemix_notm}} 帳戶。您無法將 {{site.data.keyword.Bluemix_notm}} API 金鑰與不同帳戶 ID 結合在相同 IAM 記號中。若要存取使用您 {{site.data.keyword.Bluemix_notm}} API 金鑰根據帳戶以外之帳戶建立的叢集，您必須登入帳戶才能產生新的 API 金鑰。</li><li><strong>使用一次性密碼：</strong>如果使用一次性密碼來向 {{site.data.keyword.Bluemix_notm}} 進行鑑別，則無法完全自動建立 IAM 記號，因為擷取一次性密碼需要與 Web 瀏覽器進行手動互動。若要完全自動建立 IAM 記號，您必須改為建立一個 {{site.data.keyword.Bluemix_notm}} API 金鑰。</ul></td>
</tr>
</tbody>
</table>

1.  建立 IAM（身分及存取管理）存取記號。要求中包含的內文資訊會根據您使用的 {{site.data.keyword.Bluemix_notm}} 鑑別方法而有所不同。請取代下列值：
  - _&lt;username&gt;_：您的 {{site.data.keyword.Bluemix_notm}} 使用者名稱。
  - _&lt;password&gt;_：您的 {{site.data.keyword.Bluemix_notm}} 密碼。
  - _&lt;api_key&gt;_：您的 {{site.data.keyword.Bluemix_notm}} API 金鑰。
  - _&lt;passcode&gt;_：您的 {{site.data.keyword.Bluemix_notm}} 一次性密碼。執行 `bx login --sso`，並遵循 CLI 輸出中的指示，利用 Web 瀏覽器來擷取一次性密碼。

    ```
    POST https://iam.<region>.bluemix.net/oidc/token
    ```
    {: codeblock}

    範例：
    ```
    POST https://iam.ng.bluemix.net/oidc/token
    ```
    {: codeblock}

    若要指定 {{site.data.keyword.Bluemix_notm}} 地區，請[檢閱 API 端點中所使用的地區縮寫](cs_regions.html#bluemix_regions)。

    <table summary-"Input parameters to retrieve tokens">
    <caption>取得記號的輸入參數</caption>
    <thead>
        <th>輸入參數</th>
        <th>值</th>
    </thead>
    <tbody>
    <tr>
    <td>Header</td>
    <td><ul><li>Content-Type:application/x-www-form-urlencoded</li> <li>Authorization: Basic Yng6Yng=<p><strong>附註</strong>：<code>Yng6Yng=</code> 等於使用者名稱 <strong>bx</strong> 及密碼 <strong>bx</strong> 的 URL 編碼授權。</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>{{site.data.keyword.Bluemix_notm}} 使用者名稱和密碼的內文</td>
    <td><ul><li>grant_type: password</li>
    <li>response_type: cloud_iam uaa</li>
    <li>username: <em>&lt;username&gt;</em></li>
    <li>password: <em>&lt;password&gt;</em></li>
    <li>uaa_client_ID: cf</li>
    <li>uaa_client_secret: </li></ul>
    <strong>附註</strong>：新增未指定任何值的 <code>uaa_client_secret</code> 金鑰。</td>
    </tr>
    <tr>
    <td>{{site.data.keyword.Bluemix_notm}} API 金鑰的內文</td>
    <td><ul><li>grant_type: urn:ibm:params:oauth:grant-type:apikey</li>
    <li>response_type: cloud_iam uaa</li>
    <li>apikey: <em>&lt;api_key&gt;</em></li>
    <li>uaa_client_ID: cf</li>
    <li>uaa_client_secret: </li></ul>
    <strong>附註</strong>：新增未指定任何值的 <code>uaa_client_secret</code> 金鑰。</td>
    </tr>
    <tr>
    <td>{{site.data.keyword.Bluemix_notm}} 一次性密碼的內文</td>
    <td><ul><li>grant_type: urn:ibm:params:oauth:grant-type:passcode</li>
    <li>response_type: cloud_iam uaa</li>
    <li>passcode: <em>&lt;passcode&gt;</em></li>
    <li>uaa_client_ID: cf</li>
    <li>uaa_client_secret: </li></ul>
    <strong>附註</strong>：新增未指定任何值的 <code>uaa_client_secret</code> 金鑰。</td>
    </tr>
    </tbody>
    </table>

    API 輸出範例：

    ```
    {
    "access_token": "<iam_token>",
    "refresh_token": "<iam_refresh_token>",
    "uaa_token": "<uaa_token>",
    "uaa_refresh_token": "<uaa_refresh_token>",
    "token_type": "Bearer",
    "expires_in": 3600,
    "expiration": 1493747503
    }

    

    ```
    {: screen}

    您可以在 API 輸出的 **access_token** 欄位中找到 IAM 記號。請記下 IAM 記號，以在接下來的步驟中擷取其他標頭資訊。

2.  擷取建立叢集所在的 {{site.data.keyword.Bluemix_notm}} 帳戶 ID。將 _&lt;iam_token&gt;_ 取代為您在前一個步驟中所擷取的 IAM 記號。

    ```
    GET https://accountmanagement.<region>.bluemix.net/v1/accounts
    ```
    {: codeblock}

    <table summary="取得 {{site.data.keyword.Bluemix_notm}} 帳戶 ID 的輸入參數">
    <caption>取得 {{site.data.keyword.Bluemix_notm}} 帳戶 ID 的輸入參數</caption>
    <thead>
  	<th>輸入參數</th>
  	<th>值</th>
    </thead>
    <tbody>
  	<tr>
  		<td>Headers</td>
  		<td><ul><li>Content-Type: application/json</li>
      <li>Authorization: bearer &lt;iam_token&gt;</li>
      <li>Accept: application/json</li></ul></td>
  	</tr>
    </tbody>
    </table>

    API 輸出範例：

    ```
    {
      "total_results": 3,
      "total_pages": 1,
      "prev_url": null,
      "next_url": null,
      "resources":
        {
          "metadata": {
            "guid": "<account_ID>",
            "url": "/v1/accounts/<account_ID>",
            "created_at": "2016-01-07T18:55:09.726Z",
            "updated_at": "2017-04-28T23:46:03.739Z",
            "origin": "BSS"
    ...
    ```
    {: screen}

    您可以在 API 輸出的 **resources/metadata/guid** 欄位中找到 {{site.data.keyword.Bluemix_notm}} 帳戶的 ID。

3.  產生包含 {{site.data.keyword.Bluemix_notm}} 認證及建立叢集所在帳戶 ID 的新 IAM 記號。將 _&lt;account_ID&gt;_ 取代為您在前一個步驟中所擷取的 {{site.data.keyword.Bluemix_notm}} 帳戶 ID。

    **附註：**如果您是使用 {{site.data.keyword.Bluemix_notm}} API 金鑰，則必須使用為其建立 API 金鑰的 {{site.data.keyword.Bluemix_notm}} 帳戶 ID。若要存取其他帳戶中的叢集，請登入此帳戶，並建立以此帳戶為基礎的 {{site.data.keyword.Bluemix_notm}} API 金鑰。

    ```
    POST https://iam.<region>.bluemix.net/oidc/token
    ```
    {: codeblock}

    範例：
    ```
    POST https://iam.ng.bluemix.net/oidc/token
    ```
    {: codeblock}

    若要指定 {{site.data.keyword.Bluemix_notm}} 地區，請[檢閱 API 端點中所使用的地區縮寫](cs_regions.html#bluemix_regions)。

    <table summary-"Input parameters to retrieve tokens">
    <caption>取得記號的輸入參數</caption>
    <thead>
        <th>輸入參數</th>
        <th>值</th>
    </thead>
    <tbody>
    <tr>
    <td>Header</td>
    <td><ul><li>Content-Type:application/x-www-form-urlencoded</li> <li>Authorization: Basic Yng6Yng=<p><strong>附註</strong>：<code>Yng6Yng=</code> 等於使用者名稱 <strong>bx</strong> 及密碼 <strong>bx</strong> 的 URL 編碼授權。</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>{{site.data.keyword.Bluemix_notm}} 使用者名稱和密碼的內文</td>
    <td><ul><li>grant_type: password</li>
    <li>response_type: cloud_iam uaa</li>
    <li>username: <em>&lt;username&gt;</em></li>
    <li>password: <em>&lt;password&gt;</em></li>
    <li>uaa_client_ID: cf</li>
    <li>uaa_client_secret: </li>
    <li>bss_account: <em>&lt;account_ID&gt;</em></li></ul>
    <strong>附註</strong>：新增未指定任何值的 <code>uaa_client_secret</code> 金鑰。</td>
    </tr>
    <tr>
    <td>{{site.data.keyword.Bluemix_notm}} API 金鑰的內文</td>
    <td><ul><li>grant_type: urn:ibm:params:oauth:grant-type:apikey</li>
    <li>response_type: cloud_iam uaa</li>
    <li>apikey: <em>&lt;api_key&gt;</em></li>
    <li>uaa_client_ID: cf</li>
    <li>uaa_client_secret: </li>
    <li>bss_account: <em>&lt;account_ID&gt;</em></li></ul>
      <strong>附註</strong>：新增未指定任何值的 <code>uaa_client_secret</code> 金鑰。</td>
    </tr>
    <tr>
    <td>{{site.data.keyword.Bluemix_notm}} 一次性密碼的內文</td>
    <td><ul><li>grant_type: urn:ibm:params:oauth:grant-type:passcode</li>
    <li>response_type: cloud_iam uaa</li>
    <li>passcode: <em>&lt;passcode&gt;</em></li>
    <li>uaa_client_ID: cf</li>
    <li>uaa_client_secret: </li>
    <li>bss_account: <em>&lt;account_ID&gt;</em></li></ul><strong>附註</strong>：新增未指定任何值的 <code>uaa_client_secret</code> 金鑰。</td>
    </tr>
    </tbody>
    </table>

    API 輸出範例：

    ```
    {
      "access_token": "<iam_token>",
    "refresh_token": "<iam_refresh_token>",
    "uaa_token": "<uaa_token>",
    "uaa_refresh_token": "<uaa_refresh_token>",
    "token_type": "Bearer",
    "expires_in": 3600,
    "expiration": 1493747503
    }

    

    ```
    {: screen}

    您可以在 **access_token** 中找到 IAM 記號，並且在 **refresh_token** 中找到 IAM 重新整理記號。

4.  列出帳戶中的所有 Kubernetes 叢集。使用您在先前步驟中所擷取的資訊，以建置標頭資訊。

     ```
     GET https://containers.bluemix.net/v1/clusters
     ```
     {: codeblock}

     <table summary="使用 API 的輸入參數">
         <caption>使用 API 的輸入參數</caption>
     <thead>
     <th>輸入參數</th>
     <th>值</th>
     </thead>
     <tbody>
     <tr>
     <td>Header</td>
     <td><ul><li>Authorization: bearer <em>&lt;iam_token&gt;</em></li>
     <li>X-Auth-Refresh-Token: <em>&lt;refresh_token&gt;</em></li></ul></td>
     </tr>
     </tbody>
     </table>

5.  請檢閱 [{{site.data.keyword.containershort_notm}} API 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://containers.bluemix.net/swagger-api)，以尋找所支援 API 的清單。

<br />


## 重新整理 IAM 存取記號及取得新的重新整理記號
{: #cs_api_refresh}

每個透過 API 發出的 IAM（身分及存取管理）存取記號都會在一個小時後到期。您必須定期重新整理存取記號，以確保 {{site.data.keyword.Bluemix_notm}} API 的存取權。您可以使用相同的步驟來取得新的重新整理記號。
{:shortdesc}

開始之前，請確定您有可用來要求新存取記號的 IAM 重新整理記號或 {{site.data.keyword.Bluemix_notm}} API 金鑰。
- **重新整理記號：**請遵循[使用 {{site.data.keyword.Bluemix_notm}} API 自動化叢集建立及管理處理程序](#cs_api)中的指示。
- **API 金鑰：**如下所示，擷取 [{{site.data.keyword.Bluemix_notm}} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/) API 金鑰。
   1. 從功能表列中，按一下**管理** > **安全** > **平台 API 金鑰**。
   2. 按一下**建立**。
   3. 輸入 API 金鑰的**名稱**和**說明**，然後按一下**建立**。
   4. 按一下**顯示**來查看為您產生的 API 金鑰。
   5. 複製 API 金鑰，以便您可以用它來擷取新的 IAM 存取記號。

如果您想要建立 IAM 記號，或想要取得新的重新整理記號，請使用下列步驟。

1.  使用重新整理記號或 {{site.data.keyword.Bluemix_notm}} API 金鑰來產生新的 IAM 存取記號。
    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="新 IAM 記號的輸入參數">
     <caption>新 IAM 記號的輸入參數</caption>
    <thead>
    <th>輸入參數</th>
    <th>值</th>
    </thead>
    <tbody>
    <tr>
    <td>Header</td>
    <td><ul><li>Content-Type: application/x-www-form-urlencoded</li>
      <li>Authorization: Basic Yng6Yng=</br></br><strong>附註</strong>：<code>Yng6Yng=</code> 等於使用者名稱 <strong>bx</strong> 及密碼 <strong>bx</strong> 的 URL 編碼授權。</li></ul></td>
    </tr>
    <tr>
    <td>使用重新整理記號時的內文</td>
    <td><ul><li>grant_type: refresh_token</li>
    <li>response_type: cloud_iam uaa</li>
    <li>refresh_token: <em>&lt;iam_refresh_token&gt;</em></li>
    <li>uaa_client_ID: cf</li>
    <li>uaa_client_secret: </li>
    <li>bss_account: <em>&lt;account_ID&gt;</em></li></ul><strong>附註</strong>：新增未指定任何值的 <code>uaa_client_secret</code> 金鑰。</td>
    </tr>
    <tr>
      <td>使用 {{site.data.keyword.Bluemix_notm}} API 金鑰時的內文</td>
      <td><ul><li>grant_type: <code>urn:ibm:params:oauth:grant-type:apikey</code></li>
    <li>response_type: cloud_iam uaa</li>
    <li>apikey: <em>&lt;api_key&gt;</em></li>
    <li>uaa_client_ID: cf</li>
        <li>uaa_client_secret: </li></ul><strong>附註</strong>：新增未指定任何值的 <code>uaa_client_secret</code> 金鑰。</td>
    </tr>
    </tbody>
    </table>

    API 輸出範例：

    ```
    {
      "access_token": "<iam_token>",
    "refresh_token": "<iam_refresh_token>",
    "uaa_token": "<uaa_token>",
    "uaa_refresh_token": "<uaa_refresh_token>",
    "token_type": "Bearer",
    "expires_in": 3600,
    "expiration": 1493747503
    }

    

    ```
    {: screen}

    您可以在 API 輸出的 **access_token** 欄位中尋找新的 IAM 記號，並 **refresh_token** 欄位中尋找 IAM 重新整理記號。

2.  使用前一個步驟中的記號，繼續使用 [{{site.data.keyword.containershort_notm}} API 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://us-south.containers.bluemix.net/swagger-api)。

