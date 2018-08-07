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



# 指導教學：將應用程式從 Cloud Foundry 移轉至叢集
{: #cf_tutorial}

您可以採用先前使用 Cloud Foundry 部署的應用程式，並將容器中的相同程式碼部署至 {{site.data.keyword.containershort_notm}} 中的 Kubernet 叢集。
{: shortdesc}


## 目標

- 瞭解將容器中的應用程式部署至 Kubernetes 叢集的一般處理程序。
- 從您的應用程式碼建立 Dockerfile，以建置容器映像檔。
- 將容器從該映像檔部署至 Kubernet 叢集。

## 所需時間
30 分鐘

## 適用對象
本指導教學的適用對象為 Cloud Foundry 應用程式開發人員。

## 必要條件

- [在 {{site.data.keyword.registrylong_notm}} 中建立專用映像檔登錄](../services/Registry/index.html)。
- [建立叢集](cs_clusters.html#clusters_ui)。
- [將 CLI 的目標設為叢集](cs_cli_install.html#cs_cli_configure)。
- [瞭解 Docker 和 Kubernetes 術語](cs_tech.html)。


<br />



## 課程 1：下載應用程式碼

請備妥程式碼。還沒有任何程式碼？您可以下載入門範本程式碼，以在本指導教學中使用。
{: shortdesc}

1. 建立一個名為 `cf-py` 的目錄，然後導覽至其中。在此目錄中，您將儲存建置 Docker 映像檔及執行應用程式所需的所有檔案。

  ```
  mkdir cf-py && cd cf-py
  ```
  {: pre}

2. 將應用程式碼及所有相關檔案複製到目錄。您可以使用自己的應用程式碼或是從型錄下載樣板。本指導教學使用 Python Flask 樣板。不過，您可以搭配 Node.js、Java 或 [Kitura](https://github.com/IBM-Cloud/Kitura-Starter) 應用程式來使用相同的基本步驟。

    若要下載 Python Flask 應用程式碼，請執行下列動作：

    a. 在型錄的**樣板**中，按一下 **Python Flask**。此樣板包括 Python 2 及 Python 3 應用程式的運行環境。

    b. 輸入應用程式名稱 `cf-py-<name>`，然後按一下**建立**。若要存取樣板的應用程式碼，您必須先將 CF 應用程式部署至雲端。您可以為應用程式使用任何名稱。如果您使用範例中的名稱，請將 `<name>` 取代為唯一 ID，例如 `cf-py-msx`。
    
    **注意**：請勿在任何應用程式、容器映像檔或 Kubernetes 資源名稱中使用個人資訊。

    部署應用程式時，會顯示「請使用指令行下載、修改及重新部署應用程式」的指示。

    c. 從 GUI 指示中的步驟 1 ，按一下**下載入門範本程式碼**。

    d. 將 .zip 檔解壓縮，並將其內容儲存至 `cf.py` 目錄。

您的應用程式碼已經準備好容器化！


<br />



## 課程 2：使用您的應用程式碼建立 Docker 映像檔

建立 Dockerfile，其中包含您的應用程式碼及容器的必要配置。然後，從該 Dockerfile 建置 Docker 映像檔，並將它推送至您的專用映像檔登錄。
{: shortdesc}

1. 在您於前一個課程中建立的 `cf-py` 目錄中，建立 `Dockerfile`，這是建立容器映像檔的基礎。您可以在電腦上使用您偏好的 CLI 編輯器或文字編輯器來建立 Dockerfile。下列範例顯示如何使用 nano 編輯器來建立 Dockerfile 檔案。

  ```
  nano Dockerfile
  ```
  {: pre}

2. 將下列 Script 複製到 Dockerfile 中。這個 Dockerfile 專門適用於 Python 應用程式。如果您使用另一種類型的程式碼，則 Dockerfile 必須包含不同的基本映像檔，且可能需要定義其他欄位。

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

3. 按 `Ctrl + O` 在 nano 編輯器中儲存變更。按 `Enter` 鍵確認變更。按 `Ctrl + X` 結束 nano 編輯器。

4. 建置包含應用程式碼的 Docker 映像檔，並將它推送至您的專用登錄。

  ```
  bx cr build -t registry.<region>.bluemix.net/namespace/cf-py .
  ```
  {: pre}

  <table>
  <caption>瞭解此指令的元件</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="此圖示指出有相關資訊可供您瞭解這個指令的元件。"/> 瞭解這個指令的元件</th>
  </thead>
  <tbody>
  <tr>
  <td>參數</td>
  <td>說明</td>
  </tr>
  <tr>
  <td><code>build</code></td>
  <td>build 指令。</td>
  </tr>
  <tr>
  <td><code>-t registry.&lt;region&gt;.bluemix.net/namespace/cf-py</code></td>
  <td>您的專用登錄路徑，它包含您的唯一名稱空間及映像檔名稱。在此範例中，映像檔會使用與應用程式目錄相同的名稱，但您可以為專用登錄中的映像檔選擇任何名稱。如果您不確定名稱空間，請執行 `bx cr namespaces` 指令來尋找它。</td>
  </tr>
  <tr>
  <td><code>.</code></td>
  <td>Dockerfile 的位置。如果您是從包含 Dockerfile 的目錄執行 build 指令，請輸入句點 (.)。否則，請使用 Dockerfile 的相對路徑。</td>
  </tr>
  </tbody>
  </table>

  映像檔會建立在您的專用登錄中。您可以執行 `bx cr images` 指令，以驗證已建立映像檔。

  ```
  REPOSITORY                                     NAMESPACE   TAG      DIGEST         CREATED         SIZE     VULNERABILITY STATUS   
  registry.ng.bluemix.net/namespace/cf-py        namespace   latest   cb03170b2cb2   3 minutes ago   271 MB   OK
  ```
  {: screen}


<br />



## 課程 3：從映像檔部署容器

將您的應用程式部署為 Kubernetes 叢集中的容器。
{: shortdesc}

1. 建立名為 `cf-py.yaml` 的配置 YAML 檔案，然後將 `<registry_namespace>` 更新為您的專用映像檔登錄名稱。此配置檔會從您在前一課程建立的映像檔定義容器部署，以及一個服務，以便將應用程式公開給大眾使用。

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
  <caption>瞭解 YAML 檔案元件</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解 YAML 檔案元件</th>
  </thead>
  <tbody>
  <tr>
  <td><code>image</code></td>
  <td>在 `registry.ng.bluemix.net/<registry_namespace>/cf-py:latest` 中，將 &lt;registry_namespace&gt; 取代為您的專用映像檔登錄的名稱空間。如果您不確定名稱空間，請執行 `bx cr namespaces` 指令來尋找它。</td>
  </tr>
  <tr>
  <td><code>nodePort</code></td>
  <td>建立類型為 NodePort 的 Kubernetes 服務，以公開您的應用程式。NodePort 範圍在 30000 - 32767。稍後，您可以使用此埠在瀏覽器中測試您的應用程式。</td>
  </tr>
  </tbody></table>

2. 套用配置檔，以在叢集中建立部署及服務。

  ```
  kubectl apply -f <filepath>/cf-py.yaml
  ```
  {: pre}

  輸出：

  ```
  deployment "cf-py" configured
  service "cf-py-nodeport" configured
  ```
  {: screen}

3. 現在，所有部署工作都已完成，您可以在瀏覽器中測試您的應用程式。取得用來構成 URL 的詳細資料。

    a.  取得工作者節點在叢集中的公用 IP 位址。

    ```
        bx cs workers <cluster_name>
    ```
    {: pre}

    輸出：

    ```
    ID                                                 Public IP        Private IP     Machine Type        State    Status   Zone    Version   
    kube-dal10-cr18e61e63c6e94b658596ca93d087eed9-w1   169.xx.xxx.xxx   10.xxx.xx.xxx   u2c.2x4.encrypted   normal   Ready    dal10   1.9.7
    ```
    {: screen}

    b. 開啟瀏覽器，並使用下列 URL 來查看應用程式：`http://<public_IP_address>:<NodePort>`。使用範例值的 URL 為 `http://169.xx.xxx.xxx:30872`。您可以將這個 URL 提供給同事試用，或是在行動電話的瀏覽器中輸入，就會看到應用程式真的可以公開使用。

    <img src="images/python_flask.png" alt="已部署的樣板 Python Flask 應用程式的畫面擷取。" />

5.  [啟動 Kubernetes 儀表板](cs_app.html#cli_dashboard)。

    如果您在 [{{site.data.keyword.Bluemix_notm}} GUI](https://console.bluemix.net/) 中選取叢集，則可以使用 **Kubernetes 儀表板**按鈕，透過按一下來啟動儀表板。
    {: tip}

6. 在**工作負載**標籤中，您可以看到所建立的資源。

恭喜！您的應用程式已部署在容器中！
