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




# 從映像檔建置容器
{: #images}

Docker 映像檔是您使用 {{site.data.keyword.containerlong}} 建立的每個容器的基礎。
{:shortdesc}

映像檔是從 Dockerfile 所建立的，該 Dockerfile 檔案包含建置映像檔的指示。Dockerfile 可能會參照其指示中個別儲存的建置構件（例如應用程式、應用程式的配置及其相依關係）。


## 規劃映像檔登錄
{: #planning}

映像檔通常會儲存在登錄中，而該登錄可供公開存取（公用登錄）或已設定一小組使用者的有限存取（專用登錄）。
{:shortdesc}

公用登錄（例如 Docker Hub）可用來開始使用 Docker 及 Kubernetes 在叢集中建立第一個容器化應用程式。但是，如果是企業應用程式，則請使用專用登錄（例如 {{site.data.keyword.registryshort_notm}} 中提供的登錄）來防止未授權使用者使用及變更映像檔。叢集管理者必須設定專用登錄，以確保叢集使用者可以使用存取專用登錄的認證。


您可以搭配使用多個登錄與 {{site.data.keyword.containershort_notm}}，以將應用程式部署至叢集。

|登錄|說明|優點|
|--------|-----------|-------|
|[{{site.data.keyword.registryshort_notm}}](/docs/services/Registry/index.html)|使用此選項，您可以在 {{site.data.keyword.registryshort_notm}} 中設定您自己的安全 Docker 映像檔儲存庫，您可以在其中放心地儲存映像檔並且在叢集使用者之間進行共用。|<ul><li>管理帳戶中的映像檔存取。</li><li>使用 {{site.data.keyword.IBM_notm}} 所提供的映像檔及範例應用程式（例如 {{site.data.keyword.IBM_notm}} Liberty）作為主映像檔，並在其中新增您自己的應用程式碼。</li><li>「漏洞警告器」會自動掃描映像檔的潛在漏洞（包括修正它們的 OS 特定建議）。</li></ul>|
|任何其他專用登錄|建立 [imagePullSecret ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/containers/images/)，以將任何現有專用登錄連接至叢集。密碼是用來將登錄 URL 及認證安全地儲存在 Kubernetes 密碼中。|<ul><li>使用現有專用登錄，而不管其來源（Docker Hub、組織所擁有的登錄或其他專用 Cloud 登錄）。</li></ul>|
|[公用 Docker Hub![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://hub.docker.com/){: #dockerhub}|使用此選項，在不需要 Dockerfile 變更時，即可在 [Kubernetes 部署 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) 直接使用 Docker Hub 中的現有公用映像檔。<p>**附註：**請記住，此選項可能不符合組織的安全需求（例如存取管理、漏洞掃描或應用程式保密）。</p>|<ul><li>您的叢集不需要其他設定。</li><li>包括各種開放程式碼應用程式。</li></ul>|
{: caption="公用及專用映像檔登錄選項" caption-side="top"}

在您設定映像檔登錄之後，叢集使用者可以使用映像檔，以將其應用程式部署至叢集。

當您使用容器映像檔時，進一步瞭解[保護您的個人資訊](cs_secure.html#pi)。

<br />


## 設定容器映像檔的授信內容
{: #trusted_images}

您可以從已簽署並儲存在 {{site.data.keyword.registryshort_notm}} 中的授信映像檔中建置容器，並防止從未簽署或有漏洞的映像檔進行部署。
{:shortdesc}

1.  [簽署映像檔以取得授信內容](/docs/services/Registry/registry_trusted_content.html#registry_trustedcontent)。在設定映像檔的信任之後，您可以管理可將映像檔推送至登錄的授信內容及簽章者。
2.  若要施行僅簽署的映像檔可以用來在叢集中建置容器的原則，請[新增 Container Image Security Enforcement（測試版）](/docs/services/Registry/registry_security_enforce.html#security_enforce)。
3.  部署應用程式。
    1. [部署至 `default` Kubernetes 名稱空間](#namespace)。
    2. [部署至不同的 Kubernetes 名稱空間，或從不同的 {{site.data.keyword.Bluemix_notm}} 地區或帳戶進行部署](#other)。

<br />


## 將容器從 {{site.data.keyword.registryshort_notm}} 映像檔部署至 `default` Kubernetes 名稱空間
{: #namespace}

您可以將容器從 IBM 提供的公用映像檔或 {{site.data.keyword.registryshort_notm}} 的名稱空間中所儲存的專用映像檔部署至叢集中。
{:shortdesc}

當您建立叢集時，會自動為[最近的地區登錄和全球登錄](/docs/services/Registry/registry_overview.html#registry_regions)建立不會過期的登錄記號與密碼。全球登錄會安全地儲存公用、IBM 提供的映像檔，您可以在各部署之間參照它們，而不必針對每個地區登錄中儲存的映像檔有不同的參照。地區登錄會安全地儲存您自己的專用 Docker 映像檔，以及全球登錄中儲存的相同公用映像檔。記號用來授權對 {{site.data.keyword.registryshort_notm}} 中所設定之任何名稱空間的唯讀存取，因此您可以使用這些公用（全球登錄）及專用（地區登錄）映像檔。

當您部署容器化應用程式時，每個記號必須儲存在 Kubernetes `imagePullSecret` 中，才能供 Kubernetes 叢集存取。建立叢集時，{{site.data.keyword.containershort_notm}} 會自動將全球（IBM 提供之公用映像檔）與地區登錄的記號儲存在 Kubernetes 映像檔取回密碼中。映像檔取回密碼會新增至 `default` Kubernetes 名稱空間、該名稱空間的 `ServiceAccount` 中的預設密碼清單，以及 `kube-system` 名稱空間。

**附註：**使用此起始設定，即可將容器從 {{site.data.keyword.Bluemix_notm}} 帳戶之名稱空間中可用的任何映像檔，部署至叢集的 **default** 名稱空間。若要將容器部署至叢集的其他名稱空間，或者若要使用儲存在另一個 {{site.data.keyword.Bluemix_notm}} 地區或另一個 {{site.data.keyword.Bluemix_notm}} 帳戶中的映像檔，則必須[為叢集建立您自己的 imagePullSecret](#other)。

開始之前：
1. [在 {{site.data.keyword.Bluemix_notm}} Public 或 {{site.data.keyword.Bluemix_dedicated_notm}} 上，於 {{site.data.keyword.registryshort_notm}} 中設定名稱空間，並將映像檔推送至此名稱空間](/docs/services/Registry/registry_setup_cli_namespace.html#registry_namespace_add)。
2. [建立叢集](cs_clusters.html#clusters_cli)。
3. [將 CLI 的目標設為叢集](cs_cli_install.html#cs_cli_configure)。

若要將容器部署至叢集的 **default** 名稱空間，請建立配置檔。

1.  建立名稱為 `mydeployment.yaml` 的部署配置檔。
2.  從 {{site.data.keyword.registryshort_notm}} 中的名稱空間，定義您要使用的部署及映像檔。

    若要使用 {{site.data.keyword.registryshort_notm}} 的名稱空間中的專用映像檔：

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

    **提示：**若要擷取名稱空間資訊，請執行 `bx cr namespace-list`。

3.  在叢集中建立部署。

    ```
        kubectl apply -f mydeployment.yaml
    ```
    {: pre}

    **提示：**您也可以部署現有配置檔，例如其中一個 IBM 提供的公用映像檔。此範例在美國南部地區使用 **ibmliberty** 映像檔。

    ```
        kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/deploy-ibmliberty.yaml
    ```
    {: pre}


<br />



## 建立 `imagePullSecret`，以在其他 Kubernetes 名稱空間、{{site.data.keyword.Bluemix_notm}} 地區及帳戶中存取 {{site.data.keyword.Bluemix_notm}} 或外部專用登錄
{: #other}

建立您自己的 `imagePullSecret` 以將容器部署至其他 Kubernetes 名稱空間、使用儲存在其他 {{site.data.keyword.Bluemix_notm}} 地區或帳戶中的映像檔、使用儲存在 {{site.data.keyword.Bluemix_dedicated_notm}} 中的映像檔，或使用儲存在外部專用登錄中的映像檔。
{:shortdesc}

ImagePullSecret 僅適用於建立它們的 Kubernetes 名稱空間。請針對您要部署容器的每個名稱空間，重複這些步驟。來自 [DockerHub](#dockerhub) 的映像檔不需要 ImagePullSecret。
{: tip}

開始之前：

1.  [在 {{site.data.keyword.Bluemix_notm}} Public 或 {{site.data.keyword.Bluemix_dedicated_notm}} 上，於 {{site.data.keyword.registryshort_notm}} 中設定名稱空間，並將映像檔推送至此名稱空間](/docs/services/Registry/registry_setup_cli_namespace.html#registry_namespace_add)。
2.  [建立叢集](cs_clusters.html#clusters_cli)。
3.  [將 CLI 的目標設為叢集](cs_cli_install.html#cs_cli_configure)。

<br/>
若要建立您自己的 imagePullSecret，您可以在下列選項之間進行選擇：
- [將 imagePullSecret 從預設名稱空間複製到叢集中的其他名稱空間](#copy_imagePullSecret)。
- [建立 imagePullSecret 以存取其他 {{site.data.keyword.Bluemix_notm}} 地區及帳戶中的映像檔](#other_regions_accounts)。
- [建立 imagePullSecret 以存取外部專用登錄中的映像檔](#private_images)。

<br/>
如果已在名稱空間中建立您要在部署中使用的 imagePullSecret，請參閱[使用已建立的 imagePullSecret 來部署容器](#use_imagePullSecret)。

### 將 imagePullSecret 從預設名稱空間複製到叢集中的其他名稱空間
{: #copy_imagePullSecret}

您可以將自動針對 `default` Kubernetes 名稱空間建立的 imagePullSecret 複製到叢集中的其他名稱空間。
{: shortdesc}

1. 列出叢集中可用的名稱空間。
   ```
        kubectl get namespaces
        ```
   {: pre}

   輸出範例：
   ```
   default          Active    79d
   ibm-cert-store   Active    79d
   ibm-system       Active    79d
   istio-system     Active    34d
   kube-public      Active    79d
   kube-system      Active    79d
   ```
   {: screen}

2. 選用項目：在叢集中建立名稱空間。
   ```
        kubectl create namespace <namespace_name>
        ```
   {: pre}

3. 將 imagePullSecrets 從 `default` 名稱空間複製到您選擇的名稱空間。新的 imagePullSecrets 名為 `bluemix-<namespace_name>-secret-regional` 及 `bluemix-<namespace_name>-secret-international`。
   ```
   kubectl get secret bluemix-default-secret-regional -o yaml | sed 's/default/<namespace_name>/g' | kubectl -n <namespace_name> create -f -
   ```
   {: pre}
   
   ```
   kubectl get secret bluemix-default-secret-international -o yaml | sed 's/default/<namespace_name>/g' | kubectl -n <namespace_name> create -f -
   ```
   {: pre}

4.  驗證已順利建立密碼。
    ```
    kubectl get secrets --namespace <namespace_name>
    ```
    {: pre}

5. [在名稱空間中使用 imagePullSecret](#use_imagePullSecret) 來部署容器。


### 建立 imagePullSecret 以存取其他 {{site.data.keyword.Bluemix_notm}} 地區及帳戶中的映像檔
{: #other_regions_accounts}

若要存取其他 {{site.data.keyword.Bluemix_notm}} 地區或帳戶中的映像檔，您必須建立登錄記號，並在 imagePullSecret 中儲存您的認證。
{: shortdesc}

1.  如果您沒有記號，請[為您要存取的登錄建立記號](/docs/services/Registry/registry_tokens.html#registry_tokens_create)。
2.  列出 {{site.data.keyword.Bluemix_notm}} 帳戶中的記號。

    ```
    bx cr token-list
    ```
    {: pre}

3.  記下您要使用的記號 ID。
4.  擷取記號的值。將 <em>&lt;token_ID&gt;</em> 取代為您在前一個步驟中所擷取的記號 ID。

    ```
    bx cr token-get <token_id>
    ```
    {: pre}

    您的記號值會顯示在 CLI 輸出的**記號**欄位中。

5.  建立用來儲存記號資訊的 Kubernetes 密碼。

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_URL> --docker-username=token --docker-password=<token_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>瞭解此指令的元件</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解此指令的元件</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>必要。您要使用密碼並在其中部署容器之叢集的 Kubernetes 名稱空間。執行 <code>kubectl get namespaces</code>，以列出叢集中的所有名稱空間。</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>必要。您要用於 imagePullSecret 的名稱。</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>必要。已設定名稱空間的映像檔登錄的 URL。<ul><li>對於美國南部及美國東部所設定的名稱空間：registry.ng.bluemix.net</li><li>對於英國南部所設定的名稱空間：registry.eu-gb.bluemix.net</li><li>對於歐盟中部（法蘭克福）所設定的名稱空間：registry.eu-de.bluemix.net</li><li>對於澳洲（雪梨）所設定的名稱空間：registry.au-syd.bluemix.net</li><li>對於 {{site.data.keyword.Bluemix_dedicated_notm}} 所設定的名稱空間：registry.<em>&lt;dedicated_domain&gt;</em></li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>必要。登入專用登錄的使用者名稱。對於 {{site.data.keyword.registryshort_notm}}，使用者名稱會設為 <code>token</code>。</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>必要。先前所擷取的登錄記號值。</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>必要。如果您有 Docker 電子郵件位址，請輸入它。否則，請輸入虛構的電子郵件位址，例如 a@b.c。此電子郵件是建立 Kubernetes 密碼的必要項目，但在建立之後就不再使用。</td>
    </tr>
    </tbody></table>

6.  驗證已順利建立密碼。將 <em>&lt;kubernetes_namespace&gt;</em> 取代為已在其中建立 imagePullSecret 的名稱空間。

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

7.  [在名稱空間中使用 imagePullSecret](#use_imagePullSecret) 來部署容器。

### 存取儲存在其他專用登錄中的映像檔
{: #private_images}

如果您已有專用登錄，則必須將登錄認證儲存至 Kubernetes imagePullSecret 中，然後從配置檔中參照這個密碼。
{:shortdesc}

開始之前：

1.  [建立叢集](cs_clusters.html#clusters_cli)。
2.  [將 CLI 的目標設為叢集](cs_cli_install.html#cs_cli_configure)。

若要建立 imagePullSecret，請執行下列動作：

1.  建立用來儲存專用登錄認證的 Kubernetes 密碼。

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_URL> --docker-username=<docker_username> --docker-password=<docker_password> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>瞭解此指令的元件</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解此指令的元件</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>必要。您要使用密碼並在其中部署容器的叢集的 Kubernetes 名稱空間。執行 <code>kubectl get namespaces</code>，以列出叢集中的所有名稱空間。</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>必要。您要用於 imagePullSecret 的名稱。</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>必要。儲存專用映像檔的登錄的 URL。</td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>必要。登入專用登錄的使用者名稱。</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>必要。先前所擷取的登錄記號值。</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>必要。如果您有 Docker 電子郵件位址，請輸入它。否則，請輸入虛構的電子郵件位址，例如 a@b.c。此電子郵件是建立 Kubernetes 密碼的必要項目，但在建立之後就不再使用。</td>
    </tr>
    </tbody></table>

2.  驗證已順利建立密碼。將 <em>&lt;kubernetes_namespace&gt;</em> 取代為已建立 imagePullSecret 的名稱空間的名稱。

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

3.  [建立一個參照 imagePullSecret 的 Pod](#use_imagePullSecret)。

## 使用已建立的 imagePullSecret 來部署容器
{: #use_imagePullSecret}

您可以在 Pod 部署中定義 imagePullSecret，或將 imagePullSecret 儲存在 Kubernet 服務帳戶中，使其可用於未指定服務帳戶的所有部署。
{: shortdesc}

請選擇下列選項：
* [在 Pod 部署中參照 imagePullSecret](#pod_imagePullSecret)：如果您不想要依預設為名稱空間中的所有 Pod 授與登錄的存取權，請使用此選項。
* [在 Kubernets 服務帳戶中儲存 imagePullSecret](#store_imagePullSecret)：使用此選項，可將您登錄中的映像檔存取權授與所選取 Kubernetes 名稱空間中的部署。

開始之前：
* [建立 imagePullSecret](#other) 以存取其他登錄、Kubernetes 名稱空間、{{site.data.keyword.Bluemix_notm}} 地區或帳戶中的映像檔。
* [將 CLI 的目標設為叢集](cs_cli_install.html#cs_cli_configure)。

### 在 Pod 部署中參照 `imagePullSecret`
{: #pod_imagePullSecret}

當您在 Pod 部署中參照 imagePullSecret 時，imagePullSecret 僅適用於此 Pod，且無法跨名稱空間中的 Pod 共用。
{:shortdesc}

1.  建立名稱為 `mypod.yaml` 的 Pod 配置檔。
2.  定義 Pod 及 imagePullSecret 以存取專用 {{site.data.keyword.registrylong_notm}}。

    若要存取專用映像檔，請執行下列程式碼：
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

    若要存取 {{site.data.keyword.Bluemix_notm}} 公用映像檔，請執行下列程式碼︰
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
    <caption>瞭解 YAML 檔案元件</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解 YAML 檔案元件</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;container_name&gt;</em></code></td>
    <td>要部署至叢集的容器名稱。</td>
    </tr>
    <tr>
    <td><code><em>&lt;namespace_name&gt;</em></code></td>
    <td>在其中儲存映像檔的名稱空間。若要列出可用的名稱空間，請執行 `bx cr namespace-list`。</td>
    </tr>
    <tr>
    <td><code><em>&lt;image_name&gt;</em></code></td>
    <td>您要使用的映像檔的名稱。若要列出 {{site.data.keyword.Bluemix_notm}} 帳戶中的可用映像檔，請執行 `bx cr image-list`。</td>
    </tr>
    <tr>
    <td><code><em>&lt;tag&gt;</em></code></td>
    <td>您要使用的映像檔的版本。如果未指定任何標籤，預設會使用標記為<strong>最新</strong>的映像檔。</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>您先前建立的 imagePullSecret 的名稱。</td>
    </tr>
    </tbody></table>

3.  儲存變更。
4.  在叢集中建立部署。
    ```
    kubectl apply -f mypod.yaml
    ```
    {: pre}

### 在所選取名稱空間的 Kubernetes 服務帳戶中儲存 imagePullSecret
{:#store_imagePullSecret}

每個名稱空間都有一個稱為 `default` 的服務帳戶。您可以將 imagePullSecret 新增至這個服務帳戶，以授與登錄中映像檔的存取權。未指定服務帳戶的部署會自動對此名稱空間使用 `default` 服務帳戶。
{:shortdesc}

1. 檢查預設服務帳戶是否已有 imagePullSecret。
   ```
   kubectl describe serviceaccount default -n <namespace_name>
   ```
   {: pre}
   當 `<none>` 顯示在 **Image pull secrets** 項目時，沒有 imagePullSecret 存在。  
2. 將 imagePullSecret 新增至預設服務帳戶。
   - **若要在未定義 imagePullSecret 時新增 imagePullSecret，請執行下列指令︰**
       ```
       kubectl patch -n <namespace_name> serviceaccount/default -p '{"imagePullSecrets":[{"name": "bluemix-<namespace_name>-secret-regional"}]}'
       ```
       {: pre}
   - **若要在已定義 imagePullSecret 時新增 imagePullSecret，請執行下列指令︰**
       ```
       kubectl patch -n <namespace_name> serviceaccount/default --type='json' -p='[{"op":"add","path":"/imagePullSecrets/-","value":{"name":"bluemix-<namespace_name>-secret-regional"}}]'
       ```
       {: pre}
3. 驗證 imagePullSecret 是否已新增至預設服務帳戶。
   ```
   kubectl describe serviceaccount default -n <namespace_name>
   ```
   {: pre}

   輸出範例：
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

4. 從登錄中的映像檔部署容器。
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

5. 在叢集中建立部署。
   ```
   kubectl apply -f mypod.yaml
   ```
   {: pre}

<br />


