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




# 整合服務
{: #integrations}

在 {{site.data.keyword.containerlong}} 中，您可以使用各種外部服務及型錄服務來與標準 Kubernetes 叢集搭配使用。
{:shortdesc}


## 應用程式服務
<table summary="可存取性摘要">
<caption>應用程式服務</caption>
<thead>
<tr>
<th>服務</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.blockchainfull}}</td>
<td>將 IBM Blockchain 的可公開使用開發環境部署至 {{site.data.keyword.containerlong_notm}} 中的 Kubernetes 叢集。使用此環境開發及自訂您自己的區塊鏈網路，以部署應用程式來共用可記錄交易歷程的不可變分類帳。如需相關資訊，請參閱<a href="https://ibm-blockchain.github.io" target="_blank">在雲端沙盤推演 IBM Blockchain 平台中開發 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
</tbody>
</table>

<br />



## DevOps Services
<table summary="可存取性摘要">
<caption>DevOps Services</caption>
<thead>
<tr>
<th>服務</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Codeship</td>
<td>您可以使用 <a href="https://codeship.com" target="_blank">Codeship <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>，以持續整合及遞送容器。如需相關資訊，請參閱<a href="https://www.ibm.com/blogs/bluemix/2017/10/using-codeship-pro-deploy-workloads-ibm-container-service/" target="_blank">使用 Codeship Pro 將工作負載部署至 {{site.data.keyword.containershort_notm}} <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
<td>Helm</td>
<td> <a href="https://helm.sh/" target="_blank">Helm <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 是 Kubernetes 套件管理程式。您可以建立新的 Helm 圖表或使用預先存在的 Helm 圖表，來定義、安裝及升級在 {{site.data.keyword.containerlong_notm}} 叢集中執行的複式 Kubernetes 應用程式。<p>如需相關資訊，請參閱[在 {{site.data.keyword.containershort_notm}} 中設定 Helm](cs_integrations.html#helm)。</p></td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>使用工具鏈自動建置應用程式，並將容器部署至 Kubernetes 叢集。如需設定資訊，請參閱部落格 <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Deploy Kubernetes pods to the {{site.data.keyword.containerlong_notm}} using DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
<td>Istio</td>
<td><a href="https://www.ibm.com/cloud/info/istio" target="_blank">Istio<img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 是一種開放程式碼服務，提供方法讓開發人員連接、保護、管理及監視雲端編排平台上微服務（也稱為服務網）的網路，例如 Kubernetes。請參閱關於 <a href="https://developer.ibm.com/dwblog/2017/istio/" target="_blank">IBM 如何共同建立及啟動 Istio <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 的部落格文章，以進一步瞭解開放程式碼專案的相關資訊。若要在 {{site.data.keyword.containershort_notm}} 的 Kubernetes 叢集上安裝 Istio，並開始使用範例應用程式，請參閱[指導教學：使用 Istio 管理微服務](cs_tutorials_istio.html#istio_tutorial)。</td>
</tr>
</tbody>
</table>

<br />



## 記載及監視服務
<table summary="可存取性摘要">
<caption>記載及監視服務</caption>
<thead>
<tr>
<th>服務</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
<td>CoScale</td>
<td>使用 <a href="https://www.coscale.com/" target="_blank">CoScale <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 來監視工作者節點、容器、抄本集、抄寫控制器及服務。如需相關資訊，請參閱<a href="https://www.ibm.com/blogs/bluemix/2017/06/monitoring-ibm-bluemix-container-service-coscale/" target="_blank">使用 CoScale 監視 {{site.data.keyword.containershort_notm}} <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
<td>Datadog</td>
<td>使用 <a href="https://www.datadoghq.com/" target="_blank">Dataloadog <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 來監視您的叢集，並檢視基礎架構和應用程式效能度量值。如需相關資訊，請參閱<a href="https://www.ibm.com/blogs/bluemix/2017/07/monitoring-ibm-bluemix-container-service-datadog/" target="_blank">使用 Datadog 監視 {{site.data.keyword.containershort_notm}} <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
<td>{{site.data.keyword.loganalysisfull}}</td>
<td>使用 {{site.data.keyword.loganalysisfull_notm}} 來擴充日誌收集、保留及搜尋能力。如需相關資訊，請參閱<a href="../services/CloudLogAnalysis/containers/containers_kube_other_logs.html" target="_blank">啟用叢集日誌的自動收集 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
<td>{{site.data.keyword.monitoringlong}}</td>
<td>使用 {{site.data.keyword.monitoringlong_notm}} 定義規則及警示，以擴充度量值收集及保留功能。如需相關資訊，請參閱<a href="../services/cloud-monitoring/tutorials/container_service_metrics.html" target="_blank">針對已部署在 Kubernetes 叢集中的應用程式，在 Grafana 中分析度量值 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
<td>Instana</td>
<td> <a href="https://www.instana.com/" target="_blank">Instana <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 透過自動探索及對映應用程式的 GUI，提供基礎架構及應用程式效能監視。Istana 會擷取向應用程式提出的每個要求，您可以用來進行疑難排解，並執行主要原因分析，以避免問題再次發生。如需進一步瞭解，請參閱關於<a href="https://www.instana.com/blog/precise-visibility-applications-ibm-bluemix-container-service/" target="_blank">在 {{site.data.keyword.containershort_notm}} 中部署 Istana <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 的部落格文章。</td>
</tr>
<tr>
<td>Prometheus</td>
<td>Prometheus 是一個特別針對 Kubernetes 所設計的開放程式碼監視、記載及警示工具。Prometheus 會根據 Kubernetes 記載資訊來擷取叢集、工作者節點及部署性能的詳細資訊。針對叢集中執行的每一個容器收集 CPU、記憶體、I/O 及網路活動。您可以使用自訂查詢或警示中所收集的資料，來監視叢集中的效能及工作負載。

<p>若要使用 Prometheus，請遵循 <a href="https://github.com/coreos/prometheus-operator/tree/master/contrib/kube-prometheus" target="_blank">CoreOS 指示 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</p>
</td>
</tr>
<tr>
<td>Sematext</td>
<td>使用 <a href="https://sematext.com/" target="_blank">Sematext <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 來檢視容器化應用程式的度量值和日誌。如需相關資訊，請參閱<a href="https://www.ibm.com/blogs/bluemix/2017/09/monitoring-logging-ibm-bluemix-container-service-sematext/" target="_blank">使用 Sematext 監視及記載容器 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
<td>Sysdig</td>
<td>使用 <a href="https://sysdig.com/" target="_blank">Sysindig <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>，以單一檢測點來擷取應用程式、容器、statsd 及主機度量值。如需相關資訊，請參閱<a href="https://www.ibm.com/blogs/bluemix/2017/08/monitoring-ibm-bluemix-container-service-sysdig-container-intelligence/" target="_blank">使用 Sysdig Container Intelligence 監視 {{site.data.keyword.containershort_notm}} <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
<td>Weave Scope</td>
<td>Weave Scope 提供 Kubernetes 叢集內資源（包括服務、Pod、容器、處理程序、節點等項目）的視覺圖。Weave Scope 提供 CPU 及記憶體的互動式度量值，也提供工具來調整並執行至容器。
<p>如需相關資訊，請參閱[使用 Weave Scope 及 {{site.data.keyword.containershort_notm}} 視覺化 Kubernetes 叢集資源](cs_integrations.html#weavescope)。</p></li></ol>
</td>
</tr>
</tbody>
</table>

<br />



## 安全服務
<table summary="可存取性摘要">
<caption>安全服務</caption>
<thead>
<tr>
<th>服務</th>
<th>說明</th>
</tr>
</thead>
<tbody>
  <tr id="appid">
    <td>{{site.data.keyword.appid_full}}</td>
    <td>要求使用者登入，以 [{{site.data.keyword.appid_short}}](/docs/services/appid/index.html#gettingstarted) 新增應用程式的安全層次。若要鑑別針對您應用程式的 API HTTP/HTTPS 要求，您可以使用 [{{site.data.keyword.appid_short_notm}} 鑑別 Ingress 註釋](cs_annotations.html#appid-auth)，來整合 {{site.data.keyword.appid_short_notm}} 與 Ingress 服務。</td>
  </tr>
<tr>
<td>Aqua Security</td>
  <td>作為<a href="/docs/services/va/va_index.html" target="_blank">漏洞警告器</a>的補充，您可以使用 <a href="https://www.aquasec.com/" target="_blank">Aqua Security<img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>，透過減少容許應用程式執行的內容來改善容器部署的安全。如需相關資訊，請參閱<a href="https://www.ibm.com/blogs/bluemix/2017/06/protecting-container-deployments-bluemix-aqua-security/" target="_blank">使用 Aqua Security 保護 {{site.data.keyword.Bluemix_notm}} 上的容器部署 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
<td>{{site.data.keyword.cloudcerts_full}}</td>
<td>您可以使用 <a href="../services/certificate-manager/index.html" target="_blank">{{site.data.keyword.cloudcerts_long}}<img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 來儲存及管理應用程式的 SSL 憑證。如需相關資訊，請參閱<a href="https://www.ibm.com/blogs/bluemix/2018/01/use-ibm-cloud-certificate-manager-ibm-cloud-container-service-deploy-custom-domain-tls-certificates/" target="_blank">使用 {{site.data.keyword.cloudcerts_long_notm}} 與 {{site.data.keyword.containershort_notm}} 搭配來部署自訂網域 TLS 憑證 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
  <td>{{site.data.keyword.registrylong}}</td>
  <td>設定您自己的安全 Docker 映像檔儲存庫，您可以在其中安全地儲存映像檔，以及在叢集使用者之間共用映像檔。如需相關資訊，請參閱 <a href="/docs/services/Registry/index.html" target="_blank">{{site.data.keyword.registrylong}} 文件 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
<td>NeuVector</td>
<td>使用 <a href="https://neuvector.com/" target="_blank">Neutretor <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 來保護容器與雲端原生防火牆。如需相關資訊，請參閱 <a href="https://www.ibm.com/us-en/marketplace/neuvector-container-security" target="_blank">NeuVector Container Security <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
<td>Twistlock</td>
<td>作為<a href="/docs/services/va/va_index.html" target="_blank">漏洞警告器</a>的補充，您可以使用 <a href="https://www.twistlock.com/" target="_blank">Twistlock <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 來管理防火牆、威脅保護及突發事件回應。如需相關資訊，請參閱 <a href="https://www.ibm.com/blogs/bluemix/2017/07/twistlock-ibm-bluemix-container-service/" target="_blank">{{site.data.keyword.containershort_notm}} 上的 Twistlock <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
</tbody>
</table>

<br />



## 儲存服務
<table summary="可存取性摘要">
<caption>儲存服務</caption>
<thead>
<tr>
<th>服務</th>
<th>說明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Heptio Ark</td>
  <td>您可以使用 <a href="https://github.com/heptio/ark" target="_blank">Heptio Ark <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>，來備份及還原叢集資源與持續性磁區。如需相關資訊，請參閱 Heptio Ark <a href="https://github.com/heptio/ark/blob/master/docs/use-cases.md#use-cases" target="_blank">災難回復及叢集移轉的使用案例 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
<tr>
  <td>{{site.data.keyword.cos_full}}</td>
  <td>與 {{site.data.keyword.cos_short}} 一起儲存的資料會加密及分散在多個地理位置，並使用 REST API 透過 HTTP 進行存取。您可以使用 [ibm-backup-restore image](/docs/services/RegistryImages/ibm-backup-restore/index.html) 來配置服務，以針對叢集中的資料進行一次性或排定的備份。如需服務的一般資訊，請參閱 <a href="/docs/services/cloud-object-storage/about-cos.html" target="_blank">{{site.data.keyword.cos_short}} 文件 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
</tr>
  <tr>
    <td>{{site.data.keyword.cloudantfull}}</td>
    <td>{{site.data.keyword.cloudant_short_notm}} 是文件導向的「資料庫即服務 (DBaaS)」，用來將資料儲存為 JSON 格式的文件。此服務是針對可調整性、高可用性和延續性而建置。如需相關資訊，請參閱 <a href="/docs/services/Cloudant/getting-started.html" target="_blank">{{site.data.keyword.cloudant_short_notm}} 文件 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
  </tr>
  <tr>
    <td>{{site.data.keyword.composeForMongoDB_full}}</td>
    <td>{{site.data.keyword.composeForMongoDB}} 提供高可用性及備援、自動化及隨需應變持續備份、監視工具、整合至警示系統、效能分析視圖等。如需相關資訊，請參閱 <a href="/docs/services/ComposeForMongoDB/index.html" target="_blank">{{site.data.keyword.composeForMongoDB}} 文件 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。</td>
  </tr>
</tbody>
</table>


<br />



## 將 Cloud Foundry 服務新增至叢集
{: #adding_cluster}

將現有的 Cloud Foundry 服務實例新增至叢集，讓叢集使用者在將應用程式部署至叢集時能夠存取及使用該服務。
{:shortdesc}

開始之前：

1. [將 CLI 的目標設為](cs_cli_install.html#cs_cli_configure)您的叢集。
2. [要求 {{site.data.keyword.Bluemix_notm}} 服務的實例](/docs/apps/reqnsi.html#req_instance)。
   **附註：**若要在華盛頓特區中建立服務實例，您必須使用 CLI。
3. 支援 Cloud Foundry 服務與叢集連結，但不支援其他服務。在建立服務實例之後，您會看到不同的服務類型，且會在儀表板中將服務分組為 **Cloud Foundry 服務**及**服務**。若要連結**服務**區段中的服務與叢集，[請先建立 Cloud Foundry 別名](#adding_resource_cluster)。

**附註：**
<ul><ul>
<li>您只能新增支援服務金鑰的 {{site.data.keyword.Bluemix_notm}} 服務。如果服務不支援服務金鑰，請參閱[啟用外部應用程式以使用 {{site.data.keyword.Bluemix_notm}} 服務](/docs/apps/reqnsi.html#accser_external)。</li>
<li>必須完全部署叢集及工作者節點，才能新增服務。</li>
</ul></ul>


若要新增服務，請執行下列動作：
2.  列出可用的 {{site.data.keyword.Bluemix_notm}} 服務。

    ```
        bx service list
    ```
    {: pre}

    CLI 輸出範例：

    ```
        name                      service           plan    bound apps   last operation   
    <service_instance_name>   <service_name>    spark                create succeeded
    ```
    {: screen}

3.  記下您要新增至叢集的服務實例的**名稱**。
4.  識別您要用來新增服務的叢集名稱空間。請選擇下列選項。
    -   列出現有名稱空間，並選擇您要使用的名稱空間。

        ```
                kubectl get namespaces
        ```
        {: pre}

    -   在叢集中建立名稱空間。

        ```
                kubectl create namespace <namespace_name>
        ```
        {: pre}

5.  將服務新增至叢集。

    ```
        bx cs cluster-service-bind <cluster_name_or_ID> <namespace> <service_instance_name>
    ```
    {: pre}

    將服務順利新增至叢集之後，即會建立叢集密碼，以保留服務實例認證。CLI 輸出範例：

    ```
        bx cs cluster-service-bind mycluster mynamespace cleardb 
    Binding service instance to namespace...
    OK
    Namespace: mynamespace
    Secret name:     binding-<service_instance_name>
    ```
    {: screen}

6.  驗證已在叢集名稱空間中建立密碼。

    ```
        kubectl get secrets --namespace=<namespace>
    ```
    {: pre}

若要在叢集中所部署的 Pod 中使用服務，叢集使用者必須存取服務認證。使用者可以存取 {{site.data.keyword.Bluemix_notm}} 服務的服務認證，方法為[將 Kubernetes 密碼以密碼磁區形式裝載至 Pod](#adding_app)。

<br />


## 為其他 {{site.data.keyword.Bluemix_notm}} 服務資源建立 Cloud Foundry 別名
{: #adding_resource_cluster}

支援 Cloud Foundry 服務與叢集連結。若要將不是 Cloud Foundry 服務的 {{site.data.keyword.Bluemix_notm}} 服務連結至叢集，請為該服務實例建立 Cloud Foundry 別名。
{:shortdesc}

開始之前，先[要求 {{site.data.keyword.Bluemix_notm}} 服務的實例](/docs/apps/reqnsi.html#req_instance)。

若要建立服務實例的 Cloud Foundry 別名，請執行下列動作：

1. 將建立服務實例的組織及空間設為目標。

    ```
    bx target -o <org_name> -s <space_name>
    ```
    {: pre}

2. 記下服務實例名稱。
    ```
    bx resource service-instances
    ```
    {: pre}

3. 建立服務實例的 Cloud Foundry 別名。
    ```
    bx resource service-alias-create <service_alias_name> --instance-name <service_instance>
    ```
    {: pre}

4. 驗證已建立服務別名。

    ```
    bx service list
    ```
    {: pre}

5. [將 Cloud Foundry 別名連結至叢集](#adding_cluster)。



<br />


## 將服務新增至應用程式
{: #adding_app}

加密 Kubernetes 密碼是用來儲存 {{site.data.keyword.Bluemix_notm}} 服務詳細資料及認證，以及容許服務與叢集之間的安全通訊。
{:shortdesc}

Kubernetes 密碼是一種儲存機密資訊（例如使用者名稱、密碼或金鑰）的安全方式。您可以將密碼裝載至 Pod，而不是透過環境變數或直接在 Dockerfile 中公開機密資訊。然後，Pod 中的執行容器可以存取那些密碼。

當您將密碼磁區裝載至 Pod 時，會將名為 `binding` 的檔案儲存在磁區裝載目錄中。`binding` 檔案包括您存取 {{site.data.keyword.Bluemix_notm}} 服務所需的所有資訊及認證。

開始之前，請先將 [CLI 的目標](cs_cli_install.html#cs_cli_configure)設為您的叢集。請確定叢集管理者已將您要用在應用程式中的 {{site.data.keyword.Bluemix_notm}} 服務[新增至叢集](cs_integrations.html#adding_cluster)。

1.  列出叢集名稱空間中的可用密碼。

    ```
    kubectl get secrets --namespace=<my_namespace>
    ```
    {: pre}

    輸出範例：

    ```
    NAME                              TYPE            DATA      AGE
    binding-<service_instance_name>   Opaque          1         3m

    ```
    {: screen}

2.  尋找 **Opaque** 類型的密碼，並記下密碼的**名稱**。如果有多個密碼，請與叢集管理者聯絡，以識別正確的服務密碼。

3.  開啟您偏好的編輯器。

4.  建立 YAML 檔案，以配置可透過密碼磁區存取服務詳細資料的 Pod。如果您連結了多個服務，請驗證每一個密碼是否與正確的服務相關聯。

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
    <caption>瞭解 YAML 檔案元件</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解 YAML 檔案元件</th>
    </thead>
    <tbody>
    <tr>
    <td><code>volumeMounts/mountPath</code></td>
    <td>您要裝載至容器之密碼磁區的名稱。</td>
    </tr>
    <tr>
    <td><code>volumes/name</code></td>
    <td>輸入您要裝載至容器之密碼磁區的名稱。</td>
    </tr>
    <tr>
    <td><code>secret/defaultMode</code></td>
    <td>設定服務密碼的唯讀許可權。</td>
    </tr>
    <tr>
    <td><code>secret/secretName</code></td>
    <td>輸入您稍早記下之密碼的名稱。</td>
    </tr></tbody></table>

5.  建立 Pod，並裝載密碼磁區。

    ```
        kubectl apply -f secret-test.yaml
    ```
    {: pre}

6.  驗證已建立 Pod。

    ```
        kubectl get pods --namespace=<my_namespace>
    ```
    {: pre}

    CLI 輸出範例：

    ```
        NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
    {: screen}

7.  記下 Pod 的**名稱**。
8.  取得有關 Pod 的詳細資料，並尋找密碼名稱。

    ```
        kubectl describe pod <pod_name>
    ```
    {: pre}

    輸出：

    ```
    ...
    Volumes:
      service-bind-volume:
        Type:       Secret (a volume populated by a Secret)
        SecretName: binding-<service_instance_name>
    ...
    ```
    {: screen}

    

9.  將應用程式配置為在裝載目錄中尋找 `binding` 密碼檔、剖析 JSON 內容，以及判定 URL 及服務認證來存取您的 {{site.data.keyword.Bluemix_notm}} 服務。

您現在可以存取 {{site.data.keyword.Bluemix_notm}} 服務詳細資料及認證。若要使用 {{site.data.keyword.Bluemix_notm}} 服務，請確定應用程式已配置為在裝載目錄中尋找服務密碼檔案、剖析 JSON 內容，以及判定服務詳細資料。

<br />


## 在 {{site.data.keyword.containershort_notm}} 中設定 Helm
{: #helm}

[Helm ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://helm.sh/) 是 Kubernetes 套件管理程式。您可以建立 Helm 圖表或使用預先存在的 Helm 圖表，來定義、安裝及升級在 {{site.data.keyword.containerlong_notm}} 叢集中執行的複式 Kubernetes 應用程式。
{:shortdesc}

在使用 Helm 圖表與 {{site.data.keyword.containershort_notm}} 搭配之前，您必須先在叢集中安裝及起始設定 Helm 實例。然後，您可以將 {{site.data.keyword.Bluemix_notm}} Helm 儲存庫新增至 Helm 實例。

開始之前，請先將 [CLI 的目標](cs_cli_install.html#cs_cli_configure)設為您要在其中使用 Helm 圖表的叢集。

1. 安裝 <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。

2. **重要事項**：若要維護叢集安全，請在 `kube-system` 名稱空間中建立 Tiller 的服務帳戶，以及針對 `tiller-deploy` Pod 建立 Kubernetes RBAC 叢集角色連結。

    1. 在偏好的編輯器中，建立下列檔案，並將它儲存為`rbac-config.yaml`。
      **附註**：
        * 在 Kubernetes 叢集中，依預設會建立 `cluster-admin` 叢集角色，因此您不需要明確定義它。
        * 如果您使用的是 1.7.x 版叢集，請將 `apiVersion` 變更為 `rbac.authorization.k8s.io/v1beta1`。

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

    2. 建立服務帳戶及叢集角色連結。

        ```
                kubectl create -f rbac-config.yaml
        ```
        {: pre}

3. 利用您所建立的服務帳戶，來起始設定 Helm 並安裝 `tiller`。

    ```
        helm init --service-account tiller
    ```
    {: pre}

4. 驗證 `tiller - deploy` Pod 在叢集中的 **Status** 為 `Running`。

    ```
            kubectl get pods -n kube-system -l app=helm
        ```
    {: pre}

    輸出範例：

    ```
        NAME                            READY     STATUS    RESTARTS   AGE
    tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
    ```
    {: screen}

5. 將 {{site.data.keyword.Bluemix_notm}} Helm 儲存庫新增至 Helm 實例。

    ```
        helm repo add ibm  https://registry.bluemix.net/helm/ibm
    ```
    {: pre}

6. 列出 {{site.data.keyword.Bluemix_notm}} 儲存庫中目前可用的 Helm 圖表。

    ```
        helm search ibm
    ```
    {: pre}

7. 若要進一步瞭解圖表，請列出其設定及預設值。

    例如，若要檢視 strongSwan IPSec 服務 helm 圖表的設定、文件及預設值，請執行下列指令：

    ```
        helm inspect ibm/strongswan
    ```
    {: pre}


### 相關的 Helm 鏈結
{: #helm_links}

* 若要使用 strongSwan Helm 圖表，請參閱[使用 strongSwan IPSec VPN 服務 Helm 圖表設定 VPN 連線功能](cs_vpn.html#vpn-setup)。
* 在 [Helm 圖表型錄 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/containers-kubernetes/solutions/helm-charts) GUI 中，檢視您可以與 {{site.data.keyword.Bluemix_notm}} 搭配使用的可用 Helm 圖表。
* 進一步瞭解 <a href="https://docs.helm.sh/helm/" target="_blank">Helm 文件 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 中有關用來設定及管理 Helm 圖表的 Helm 指令。
* 進一步瞭解如何[使用 Kubernetes Helm 圖表來加快部署 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/recipes/tutorials/increase-deployment-velocity-with-kubernetes-helm-charts/)。

## 視覺化 Kubernetes 叢集資源
{: #weavescope}

Weave Scope 提供 Kubernetes 叢集內的資源（包括服務、Pod、容器等等）的視覺圖。Weave Scope 提供 CPU 及記憶體的互動式度量值，以及對容器執行 tail 及 exec 的工具。
{:shortdesc}

開始之前：

-   請記住不要在公用網際網路上公開叢集資訊。請完成下列步驟，以安全地部署 Weave Scope，並於本端從 Web 瀏覽器進行存取。
-   如果您還沒有叢集，請[建立標準叢集](cs_clusters.html#clusters_ui)。Weave Scope 可能需要大量 CPU，特別是應用程式。請搭配執行 Weave Scope 與較大的標準叢集，而非免費叢集。
-   [將 CLI 的目標設為](cs_cli_install.html#cs_cli_configure)叢集，才能執行 `kubectl` 指令。


若要搭配使用 Weave Scope 與叢集，請執行下列動作：
2.  在叢集中部署其中一個提供的 RBAC 許可權配置檔。

    若要啟用讀寫許可權，請執行下列指令：

    ```
        kubectl apply -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac.yaml"
    ```
    {: pre}

    若要啟用唯讀許可權，請執行下列動作：

    ```
        kubectl apply --namespace weave -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac-readonly.yaml"
    ```
    {: pre}

    輸出：

    ```
        clusterrole "weave-scope-mgr" created
    clusterrolebinding "weave-scope-mgr-role-binding" created
    ```
    {: screen}

3.  部署 Weave Scope 服務（只能透過叢集 IP 位址進行私密存取）。

    <pre class="pre">
    <code>kubectl apply --namespace weave -f "https://cloud.weave.works/k8s/scope.yaml?k8s-version=$(kubectl version | base64 | tr -d '&bsol;n')"</code>
    </pre>

    輸出：

    ```
        serviceaccount "weave-scope" created
    deployment "weave-scope-app" created
    service "weave-scope-app" created
    daemonset "weave-scope-agent" created
    ```
    {: screen}

4.  在您的電腦上執行埠轉遞指令，以開啟服務。下次存取 Weave Scope 時，您就可以執行此埠轉遞指令，而不需要再次完成先前的配置步驟。

    ```
        kubectl port-forward -n weave "$(kubectl get -n weave pod --selector=weave-scope-component=app -o jsonpath='{.items..metadata.name}')" 4040
    ```
    {: pre}

    輸出：

    ```
    Forwarding from 127.0.0.1:4040 -> 4040
    Forwarding from [::1]: :1]:4040 -> 4040
    Handling connection for 4040
    ```
    {: screen}

5.  開啟 Web 瀏覽器，並前往 `http://localhost:4040`。若未部署預設元件，您會看到下圖。您可以選擇檢視拓蹼圖，或叢集中的 Kubernetes 資源表格。

     <img src="images/weave_scope.png" alt="來自 Weave Scope 的拓蹼範例" style="width:357px;" />


[進一步瞭解 Weave Scope 特性 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.weave.works/docs/scope/latest/features/)。

<br />

