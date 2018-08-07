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



# 指導教學：在 {{site.data.keyword.containerlong_notm}} 上安裝 Istio
{: #istio_tutorial}

[Istio](https://www.ibm.com/cloud/info/istio) 是一種開放平台，能夠連接、保護及管理雲端平台上的微服務網路（也稱為服務網），例如 {{site.data.keyword.containerlong}} 中的 Kubernetes。使用 Istio，您可以管理網路資料流量、微服務之間的負載平衡、強制執行存取原則、驗證服務身分等等。
{:shortdesc}

在本指導教學中，您可以看到如何為一個稱為 BookInfo 的簡單模擬書店應用程式，一起安裝 Istio 及四個微服務。微服務包括產品網頁、書籍詳細資料、檢閱及評等。將 BookInfo 的微服務部署至已安裝 Istio 的 {{site.data.keyword.containershort}} 叢集時，即會在每一個微服務的 Pod 中注入 Istio Envoy Sidecar Proxy。

**附註**：Istio 平台的一些配置及特性仍在開發中，而且會根據使用者意見進行變更。請先等待幾個月的時間，待其穩定後再於正式作業環境中使用 Istio。 

## 目標

-   在叢集中下載並安裝 Istio
-   部署 BookInfo 範例應用程式
-   將 Envoy Sidecar Proxy 注入至應用程式的四個微服務的 Pod，以連接服務網中的微服務
-   驗證 BookInfo 應用程式部署並循環使用三個版本的評等服務

## 所需時間

30 分鐘

## 適用對象

本指導教學的適用對象是第一次使用 Istio 的軟體開發人員及網路管理者。

## 必要條件

-  [安裝 CLI](cs_cli_install.html#cs_cli_install_steps)。Istio 需要 Kubernetes 1.9 版或以上版本。請務必安裝符合叢集的 Kubernetes 版本的 `kubectl` CLI 版本。
-  [建立叢集](cs_clusters.html#clusters_cli)，其 Kubernetes 版本為 1.9 或以上版本。
-  [將 CLI 的目標設為叢集](cs_cli_install.html#cs_cli_configure)。

## 課程 1：下載並安裝 Istio
{: #istio_tutorial1}

在叢集中下載並安裝 Istio。
{:shortdesc}

1. 直接從 [https://github.com/istio/istio/releases ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/istio/istio/releases) 下載 Istio，或使用 curl 取得最新版本：

   ```
   curl -L https://git.io/getLatestIstio | sh -
   ```
   {: pre}

2. 解壓縮安裝檔案。

3. 將 `istioctl` 用戶端新增至 PATH。例如，在 MacOS 或 Linux 系統上執行下列指令：

   ```
   export PATH=$PWD/istio-0.4.0/bin:$PATH
   ```
   {: pre}

4. 將目錄切換至 Istio 檔案位置。

   ```
   cd filepath/istio-0.4.0
   ```
   {: pre}

5. 在 Kubernetes 叢集上安裝 Istio。Istio 是部署在 Kubernetes 名稱空間 `istio-system` 中。

   ```
   kubectl apply -f install/kubernetes/istio.yaml
   ```
   {: pre}

   **附註**：如果您需要在 Sidecar 之間啟用相互 TLS 鑑別，則可以改為安裝 `istio-auth` 檔案：`kubectl apply -f install/kubernetes/istio-auth.yaml`

6. 先確定已完整部署 Kubernetes 服務 `istio-pilot`、`istio-mixer` 及 `istio-ingress`，再繼續進行。

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

7. 也先確定已完整部署對應的 Pod `istio-pilot-*`、`istio-mixer-*`、`istio-ingress-*` 及 `istio-ca-*`，再繼續進行。

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


做得好！您已順利將 Istio 安裝至叢集。接下來，請將 BookInfo 範例應用程式部署至您的叢集。


## 課程 2：部署 BookInfo 應用程式
{: #istio_tutorial2}

將 BookInfo 範例應用程式的微服務部署至 Kubernetes 叢集。
{:shortdesc}

這四個微服務包括產品網頁、書籍詳細資料、檢閱（含數個版本的檢閱微服務）及評等。您可以在 Istio 安裝的 `samples/bookinfo` 目錄中找到此範例所使用的所有檔案。

當您部署 BookInfo 時，會先將 Envoy Sidecar Proxy 當成容器注入至應用程式微服務的 Pod，再部署微服務 Pod。Istio 會使用延伸版本的 Envoy Proxy 來調解服務網中所有微服務的所有入埠及出埠資料流量。如需 Envoy 的相關資訊，請參閱 [Istio 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://istio.io/docs/concepts/what-is-istio/overview.html#envoy)。

1. 部署 BookInfo 應用程式。`kube-inject` 指令會將 Envoy 新增至 `bookinfo.yaml` 檔案，並使用這個已更新的檔案來部署應用程式。部署應用程式微服務時，也會將 Envoy Sidecar 部署在每一個微服務 Pod 中。

   ```
   kubectl apply -f <(istioctl kube-inject -f samples/bookinfo/kube/bookinfo.yaml)
   ```
   {: pre}

2. 確定已部署微服務及其對應的 Pod：

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

3. 若要驗證應用程式部署，請取得叢集的公用位址。

    * 如果您要使用標準叢集，請執行下列指令來取得叢集的 Ingress IP 及埠：

       ```
              kubectl get ingress
       ```
       {: pre}

       輸出範例：

       ```
              NAME      HOSTS     ADDRESS          PORTS     AGE
       gateway   *         169.xx.xxx.xxx   80        3m
       ```
       {: screen}

       針對此範例所產生的 Ingress 位址是 `169.48.221.218:80`。使用下列指令，將位址匯出為閘道 URL。您將在下一步中使用閘道 URL 來存取 BookInfo 產品頁面。

       ```
              export GATEWAY_URL=169.xx.xxx.xxx:80
       ```
       {: pre}

    * 如果您要使用免費叢集，則必須使用工作者節點的公用 IP 及 NodePort。執行下列指令，以取得工作者節點的公用 IP：

       ```
              bx cs workers <cluster_name_or_ID>
       ```
       {: pre}

       使用下列指令，將工作者節點的公用 IP 匯出為閘道 URL。您將在下一步中使用閘道 URL 來存取 BookInfo 產品頁面。

       ```
              export GATEWAY_URL=<worker_node_public_IP>:$(kubectl get svc istio-ingress -n istio-system -o jsonpath='{.spec.ports[0].nodePort}')
       ```
       {: pre}

4. 對 `GATEWAY_URL` 變數進行 Curl 處理，以檢查 BookInfo 是否正在執行中。`200` 回應表示 BookInfo 與 Istio 適當地執行中。

   ```
   curl -I http://$GATEWAY_URL/productpage
   ```
   {: pre}

5. 在瀏覽器中，移至 `http://$GATEWAY_URL/productpage` 以檢視 BookInfo 網頁。

6. 嘗試多次重新整理頁面。不同版本的檢閱區段會循環使用紅色星星、黑色星星及無任何星星。

做得好！您已順利使用 Istio Envoy Sidecar 來部署 BookInfo 範例應用程式。接下來，您可以清理資源，或繼續執行其他指導教學來進一步探索 Istio。

## 清理
{: #istio_tutorial_cleanup}

如果您完成使用 Istio，而且不想要[繼續探索](#istio_tutorial_whatsnext)，則可以清理叢集中的 Istio 資源。
{:shortdesc}

1. 刪除叢集中的所有 BookInfo 服務、Pod 及部署。

   ```
   samples/bookinfo/kube/cleanup.sh
   ```
   {: pre}

2. 解除安裝 Istio。

   ```
   kubectl delete -f install/kubernetes/istio.yaml
   ```
   {: pre}

## 下一步為何？
{: #istio_tutorial_whatsnext}

若要進一步探索 Istio，您可以在 [Istio 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://istio.io/) 中找到更多手冊。

* [Intelligent Routing ![外部鏈結圖示 ](../icons/launch-glyph.svg "外部鏈結圖示")](https://istio.io/docs/guides/intelligent-routing.html)：此範例顯示如何使用 Istio 的資料流量管理功能，將資料流量遞送至特定版本之 BookInfo 的檢閱及評等微服務。

* [In-Depth Telemetry ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://istio.io/docs/guides/telemetry.html)：此範例包括如何使用 Istio Mixer 及 Envoy Proxy 來取得 BookInfo 微服務的統一度量值、日誌及追蹤。
