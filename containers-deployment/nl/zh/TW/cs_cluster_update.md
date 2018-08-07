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





# 更新叢集和工作者節點
{: #update}

您可以安裝更新，讓 Kubernetes 叢集在 {{site.data.keyword.containerlong}} 中保持最新。
{:shortdesc}

## 更新 Kubernetes 主節點
{: #master}

Kubernetes 會定期發行[主要、次要或修補程式更新](cs_versions.html#version_types)。視更新類型而定，您可能要負責更新 Kubernetes 主要元件。
{:shortdesc}

更新會影響 Kubernetes API 伺服器版本或 Kubernetes 主節點的其他元件。您一律要負責將工作者節點保持為最新。進行更新時，Kubernetes 主節點會在工作者節點之前先更新。

依預設，Kubernetes 主節點限制了更新 Kubernetes API 伺服器的能力，其限制為不超過現行版本兩個次要版本。例如，如果現行 Kubernetes API 伺服器版本是 1.7，而您要更新至 1.10，則必須先更新至 1.8 或 1.9。您可以強制進行更新，但更新三個以上次要版本可能會造成非預期的結果。如果您的叢集是執行不受支援的 Kubernetes 版本，則可能需要強制更新。

下圖顯示您可以採取來更新主節點的處理程序。

![主節點更新最佳作法](/images/update-tree.png)

圖 1. 更新 Kubernetes 主節點程序圖

**注意**：在更新程序發生之後，您無法將叢集回復至舊版。請務必使用測試叢集，並遵循指示來解決可能的問題，然後才更新正式作業主節點。

對於_主要_ 或_次要_ 更新，請完成下列步驟：

1. 檢閱 [Kubernetes 變更](cs_versions.html)，並更新標示為_在主節點之前更新_ 的任何項目。
2. 使用 GUI 或執行 [CLI 指令](cs_cli_reference.html#cs_cluster_update)，來更新 Kubernetes API 伺服器及關聯的 Kubernetes 主節點元件。當您更新 Kubernetes API 伺服器時，API 伺服器會關閉大約 5-10 分鐘。在更新期間，您無法存取或變更叢集。不過，叢集使用者部署的工作者節點、應用程式和資源不會修改，並將繼續執行。
3. 確認更新已完成。請檢閱 {{site.data.keyword.Bluemix_notm}} 儀表板上的 Kubernetes API 伺服器版本，或執行 `bx cs clusters`。
4. 安裝符合在 Kubernetes 主節點中執行的 Kubernetes API 伺服器版本的 [`kibectl cli`](cs_cli_install.html#kubectl) 版本。

當 Kubernetes API 伺服器更新完成時，您可以更新工作者節點。

<br />


## 更新工作者節點
{: #worker_node}


您收到更新工作者節點的通知。這代表什麼意思？當 Kubernetes API 伺服器及其他 Kubernetes 主節點元件的安全更新和修補程式備妥之後，您需要確定工作者節點保持同步。
{: shortdesc}

工作者節點 Kubernetes 版本不得高於在 Kubernetes 主節點中執行的 Kubernetes API 伺服器版本。開始之前，請[更新 Kubernetes 主節點](#master)。

**注意**：
<ul><li>更新工作者節點可能會導致應用程式及服務關閉。</li>
<li>如果資料未儲存在 Pod 外，便會刪除資料。</li>
<li>在您的部署中，使用[抄本 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#replicas)，在可用節點上重新排定 Pod。</li></ul>

但如果我不能有任何關閉時間怎麼辦？

在更新程序中，特定節點會關閉一段時間。要協助避免應用程式的關閉時間，您可以在配置對映定義唯一索引鍵，該配置對映指定在升級程序期間，特定節點類型的臨界值百分比。藉由根據標準 Kubernetes 標籤來定義規則，以及提供允許無法使用的最大節點量百分比，您可以確保您的應用程式維持啟動與執行。如果節點尚未完成部署程序，會將它視為無法使用。

索引鍵如何定義？

在配置對映的資料資訊區段中，您可以定義最多 10 個不同規則，以在任何給定時間執行。若要升級，工作者節點必須通過每一個已定義的規則。

索引鍵定義好了。現在該怎麼做？

在定義規則之後，請執行 `bx cs worker-update` 指令。如果傳回成功的回應，工作者節點會排入佇列進行更新。不過，在所有規則都滿足之前，節點不會經歷更新程序。排入佇列時，會依照間隔檢查規則，查看是否有任何節點能夠更新。

我選擇不要定義配置對映的話會怎樣？

未定義配置對映時，會使用預設值。依預設，在每個叢集中，您所有的工作者節點會有最多 20% 在更新程序期間無法使用。

若要更新您的工作者節點，請執行下列動作：

1. 進行 [Kubernetes 變更](cs_versions.html)中標示為_在主節點之後更新_ 的任何變更。

2. 選用項目：定義您的配置對映。
    範例：

    ```
        apiVersion: v1
    kind: ConfigMap
    metadata:
      name: ibm-cluster-update-configuration
      namespace: kube-system
    data:
     drain_timeout_seconds: "120"
     zonecheck.json: |
       {
         "MaxUnavailablePercentage": 70,
         "NodeSelectorKey": "failure-domain.beta.kubernetes.io/zone",
         "NodeSelectorValue": "dal13"
       }
     regioncheck.json: |
       {
         "MaxUnavailablePercentage": 80,
         "NodeSelectorKey": "failure-domain.beta.kubernetes.io/region",
         "NodeSelectorValue": "us-south"
       }
    defaultcheck.json: |
       {
         "MaxUnavailablePercentage": 100
       }
    ```
    {:pre}
  <table summary="表格中的第一列跨這兩個直欄。其餘的列應該從左到右閱讀，第一欄為參數，第二欄則為符合的說明。">
    <caption>ConfigMap 元件</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解元件</th>
    </thead>
    <tbody>
      <tr>
        <td><code>drain_timeout_seconds</code></td>
        <td> 選用項目：工作者節點更新期間發生的排除逾時（以秒為單位）。排除會將節點設為 `unschedulable`，這可防止將新的 Pod 部署至該節點。排除也會刪除節點的 Pod。接受值是從 1 到 180 的整數。預設值為 30。</td>
      </tr>
      <tr>
        <td><code>zonecheck.json</code></br><code>regioncheck.json</code></td>
        <td> 您要為其設定規則的唯一索引鍵範例。索引鍵的名稱可以是您要的任何內容，索引鍵內的配置集會剖析資訊。針對您定義的每個索引鍵，您只能為 <code>NodeSelectorKey</code> 和 <code>NodeSelectorValue</code> 設定一個值。如果您想要為多個地區或位置（資料中心）設定規則，請建立新的索引鍵項目。</td>
      </tr>
      <tr>
        <td><code>defaultcheck.json</code></td>
        <td> 依預設，如果 <code>ibm-cluster-update-configuration</code> 對映未以有效的方式定義，則您的叢集在同一時間只能有 20% 無法使用。如果已定義一個以上的有效規則，但沒有廣域預設值，則新的預設值是允許在同一時間有 100% 的工作者節點無法使用。您可以建立預設百分比來控制此情況。</td>
      </tr>
      <tr>
        <td><code>MaxUnavailablePercentage</code></td>
        <td> 針對指定索引鍵，允許無法使用的節點量上限，以百分比的格式指定。當節點在部署、重新載入或佈建的程序中，便無法使用該節點。已排入佇列的工作者節點，如果超出任何已定義的無法使用百分比上限，便會被封鎖而無法升級。</td>
      </tr>
      <tr>
        <td><code>NodeSelectorKey</code></td>
        <td> 您要為指定索引鍵設定規則的標籤類型。您可以為 IBM 提供的預設標籤設定規則，也可以為您建立的標籤設定規則。</td>
      </tr>
      <tr>
        <td><code>NodeSelectorValue</code></td>
        <td> 指定索引鍵內，規則已設定要評估的節點子集。</td>
      </tr>
    </tbody>
  </table>

    **附註**：最多可以定義 10 個規則。如果您新增超過 10 個索引鍵到一個檔案裡，則只會剖析部分的資訊。

3. 從 GUI 或藉由執行 CLI 指令更新您的工作者節點。
  * 若要從 {{site.data.keyword.Bluemix_notm}} 儀表板更新，請導覽至叢集的`工作者節點`區段，然後按一下`更新工作者`。
  * 若要取得工作者節點 ID，請執行 `bx cs wokers<cluster_name_or_ID>`。如果您選取多個工作者節點，則工作者節點會放入佇列以便進行更新評估。如果評估之後認為它們已備妥，則會根據配置中的規則來更新。

    ```
        bx cs worker-update <cluster_name_or_ID> <worker_node1_ID> <worker_node2_ID>
    ```
    {: pre}

4. 選用項目：執行下列指令並查看 **Events** 以驗證配置對映所觸發的事件，以及發生的任何驗證錯誤。
    ```
    kubectl describe -n kube-system cm ibm-cluster-update-configuration
    ```
    {: pre}

5. 確認更新已完成：
  * 在 {{site.data.keyword.Bluemix_notm}} 儀表板上檢閱 Kubernetes 版本，或執行 `bx cs workers<cluster_name_or_ID>`。
  * 執行 `kudectl get nodes` 來檢閱工作者節點的 Kibernets 版本。
  * 在部分情況下，較舊的叢集可能在更新之後將重複的工作者節點列為 **NotReady** 狀態。若要移除重複項目，請參閱[疑難排解](cs_troubleshoot_clusters.html#cs_duplicate_nodes)。

後續步驟：
  - 對其他叢集重複更新程序。
  - 通知在叢集內工作的開發人員，將 `kubectl` CLI 更新至 Kubernetes 主節點的版本。
  - 如果 Kubernetes 儀表板未顯示使用率圖形，則會[刪除 `kudbe-dashboard` Pod](cs_troubleshoot_health.html#cs_dashboard_graphs)。
  





<br />



## 更新機型
{: #machine_type}

您可以藉由新增工作者節點及移除舊的工作者節點，來更新工作者節點的機型。例如，假設您在專用的機型上有虛擬工作者節點，且機型的名稱中有 `u1c` 或 `b1c`，請建立使用名稱中有 `u2c` 或 `b2c` 的機型的工作者節點。
{: shortdesc}

開始之前：
- [將 CLI 的目標設為](cs_cli_install.html#cs_cli_configure)您的叢集。
- 如果您在工作者節點上儲存資料，若資料不是[儲存在工作者節點外，即會刪除資料](cs_storage.html#storage)。


**注意**：更新工作者節點可能會導致應用程式及服務關閉。如果[資料未儲存在 Pod 外](cs_storage.html#storage)，即會刪除資料。



1. 記下要更新的工作者節點的名稱及位置。
    ```
    bx cs workers <cluster_name>
    ```
    {: pre}

2. 檢視可用的機型。
    ```
    bx cs machine-types <location>
    ```
    {: pre}

3. 使用 [bx cs worker-add](cs_cli_reference.html#cs_worker_add) 指令來新增工作者節點。請指定機型。

    ```
    bx cs worker-add --cluster <cluster_name> --machine-type <machine_type> --number <number_of_worker_nodes> --private-vlan <private_VLAN_ID> --public-vlan <public_VLAN_ID>
    ```
    {: pre}

4. 驗證是否已新增工作者節點。

    ```
    bx cs workers <cluster_name>
    ```
    {: pre}

5. 當新增的工作者節點處於 `Normal` 狀態時，您可以移除過期的工作者節點。**附註**：如果您要移除按月計費的機型（例如裸機），則會向您收取整個月的費用。

    ```
    bx cs worker-rm <cluster_name> <worker_node>
    ```
    {: pre}

6. 重複這些步驟，將其他工作者節點升級至不同機型。










