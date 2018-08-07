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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# 叢集除錯
{: #cs_troubleshoot}

在您使用 {{site.data.keyword.containerlong}} 時，請考慮使用這些技術來進行一般疑難排解以及叢集的除錯。您也可以檢查 [{{site.data.keyword.Bluemix_notm}} 系統的狀態 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/bluemix/support/#status)。
{: shortdesc}

您可以採取這些一般步驟來確保叢集保持最新：
- 每月檢查可用的安全及作業系統修補程式，以[更新工作者節點](cs_cli_reference.html#cs_worker_update)。
- [將叢集更新](cs_cli_reference.html#cs_cluster_update)為 {{site.data.keyword.containershort_notm}} 的最新預設 [Kubernetes 版本](cs_versions.html)。

## 叢集除錯
{: #debug_clusters}

請檢閱選項以進行叢集除錯，並找出失敗的主要原因。

1.  列出叢集，並尋找叢集的 `State`。

  ```
  bx cs clusters
  ```
  {: pre}

2.  檢閱叢集的 `State`。如果叢集處於 **Critical**、**Delete failed** 或 **Warning** 狀態，或停留在 **Pending** 狀態很長一段時間，請開始[針對工作者節點進行除錯](#debug_worker_nodes)。

    <table summary="每個表格列都應該從左到右閱讀，第一欄為叢集狀態，第二欄則為說明。">
    <caption>叢集狀態</caption>
   <thead>
   <th>叢集狀態</th>
   <th>說明</th>
   </thead>
   <tbody>
<tr>
   <td>Aborted</td>
   <td>在部署 Kubernetes 主節點之前，使用者要求刪除叢集。叢集刪除完成之後，即會從儀表板中移除該叢集。如果叢集停留在此狀態很長一段時間，請開立 [{{site.data.keyword.Bluemix_notm}} 支援問題單](cs_troubleshoot.html#ts_getting_help)。</td>
   </tr>
 <tr>
     <td>Critical</td>
     <td>無法聯繫 Kubernetes 主節點，或叢集中的所有工作者節點都已關閉。</td>
    </tr>
   <tr>
     <td>Delete failed</td>
     <td>無法刪除 Kubernetes 主節點或至少一個工作者節點。</td>
   </tr>
   <tr>
     <td>Deleted</td>
     <td>叢集已刪除，但尚未從儀表板中移除。如果叢集停留在此狀態很長一段時間，請開立 [{{site.data.keyword.Bluemix_notm}} 支援問題單](cs_troubleshoot.html#ts_getting_help)。</td>
   </tr>
   <tr>
   <td>Deleting</td>
   <td>正在刪除叢集，且正在拆除叢集基礎架構。您無法存取該叢集。</td>
   </tr>
   <tr>
     <td>Deploy failed</td>
     <td>無法完成 Kubernetes 主節點的部署。您無法解決此狀態。請開立 [{{site.data.keyword.Bluemix_notm}} 支援問題單](cs_troubleshoot.html#ts_getting_help)，與 IBM Cloud 支援中心聯絡。</td>
   </tr>
     <tr>
       <td>Deploying</td>
       <td>Kubernetes 主節點尚未完整部署。您無法存取叢集。請等到叢集完成部署後，再檢閱叢集的性能。</td>
      </tr>
      <tr>
       <td>Normal</td>
       <td>叢集中的所有工作者節點都已開始執行。您可以存取叢集，並將應用程式部署至叢集。此狀態被視為健全，您不需要採取動作。**附註**：雖然工作者節點可能是正常的，但可能仍需注意其他的基礎架構資源（例如[網路](cs_troubleshoot_network.html)和[儲存空間](cs_troubleshoot_storage.html)）。</td>
    </tr>
      <tr>
       <td>Pending</td>
       <td>已部署 Kubernetes 主節點。正在佈建工作者節點，因此還無法在叢集中使用。您可以存取叢集，但無法將應用程式部署至叢集。</td>
     </tr>
   <tr>
     <td>Requested</td>
     <td>已傳送要建立叢集及訂購 Kubernetes 主節點和工作者節點之基礎架構的要求。當開始部署叢集時，叢集狀態會變更為 <code>Deploying</code>。如果叢集停留在 <code>Requested</code> 狀態很長一段時間，請開立 [{{site.data.keyword.Bluemix_notm}} 支援問題單](cs_troubleshoot.html#ts_getting_help)。</td>
   </tr>
   <tr>
     <td>Updating</td>
     <td>在 Kubernetes 主節點中執行的 Kubernetes API 伺服器正更新為新的 Kubernetes API 版本。在更新期間，您無法存取或變更叢集。使用者所部署的工作者節點、應用程式及資源不會遭到修改，並會繼續執行。請等到更新完成後，再檢閱叢集的性能。</td>
   </tr>
    <tr>
       <td>Warning</td>
       <td>叢集中至少有一個工作者節點無法使用，但有其他工作者節點可用，並且可以接管工作負載。</td>
    </tr>
   </tbody>
 </table>


<br />


## 針對工作者節點進行除錯
{: #debug_worker_nodes}

檢閱針對工作者節點進行除錯的選項，並尋找失敗的主要原因。


1.  如果叢集處於 **Critical**、**Delete failed** 或 **Warning** 狀態，或停留在 **Pending** 狀態很長一段時間，請檢閱工作者節點的狀態。

  ```
  bx cs workers <cluster_name_or_id>
  ```
  {: pre}

2.  請檢閱 CLI 輸出中每個工作者節點的 `State` 及 `Status` 欄位。

  <table summary="每個表格列都應該從左到右閱讀，第一欄為叢集狀態，第二欄則為說明。">
    <caption>工作者節點狀態</caption>
    <thead>
    <th>工作者節點狀態</th>
    <th>說明</th>
    </thead>
    <tbody>
  <tr>
      <td>Critical</td>
      <td>工作者節點可能因為各種原因而進入 Critical 狀態：<ul><li>您針對工作者節點起始重新啟動，但未隔離及排除您的工作者節點。重新啟動工作者節點會導致 <code>docker</code>、<code>kubelet</code>、<code>kube-proxy</code> 及 <code>calico</code> 中的資料毀損。</li><li>部署至工作者節點的 Pod 未使用[記憶體 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/tasks/configure-pod-container/assign-memory-resource/) 及 [CPU ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/tasks/configure-pod-container/assign-cpu-resource/) 的資源限制。沒有資源限制，Pod 可能會取用所有可用的資源，而未留下任何資源給在此工作者節點上執行的其他 Pod。工作負載的過度使用會導致工作者節點失敗。</li><li>在一段時間內執行數百或數千個容器之後，<code>Docker</code>、<code>kubelet</code> 或 <code>calico</code> 會進入無法復原的狀態。</li><li>您為工作者節點設定 Virtual Router Appliance，而工作者節點關閉並中斷工作者節點與 Kubernetes 主節點之間的通訊。</li><li> {{site.data.keyword.containershort_notm}} 或 IBM Cloud 基礎架構 (SoftLayer) 中的現行網路問題，導致工作者節點與 Kubernetes 主節點之間的通訊失敗。</li><li>工作者節點容量不足。請檢查工作者節點的<strong>狀態</strong>，以查看它是否顯示<strong>磁碟不足</strong>或<strong>記憶體不足</strong>。如果工作者節點容量不足，請考慮減少工作者節點上的工作負載，或將工作者節點新增至叢集，以協助對工作負載進行負載平衡。</li></ul> 在許多情況下，[重新載入](cs_cli_reference.html#cs_worker_reload)工作者節點可以解決此問題。當您重新載入工作者節點時，最新的[修補程式版本](cs_versions.html#version_types)會套用至您的工作者節點。主要版本和次要版本不會變更。在您重新載入工作者節點之前，請確定隔離及排除工作者節點，以確保現有 Pod 會溫和終止並重新排程其餘的工作者節點。</br></br> 如果重新載入工作者節點未解決此問題，請移至下一步，以繼續進行工作者節點的疑難排解。</br></br><strong>提示：</strong>您可以[為工作者節點配置性能檢查，並啟用自動回復](cs_health.html#autorecovery)。如果「自動回復」根據配置的檢查，偵測到性能不佳的工作者節點，則「自動回復」會觸發更正動作，如在工作者節點上重新載入 OS。如需自動回復運作方式的相關資訊，請參閱[自動回復部落格![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/)。</td>
     </tr>
      <tr>
        <td>Deploying</td>
        <td>當您更新工作者節點的 Kubernetes 版本時，會重新部署工作者節點以安裝更新。如果工作者節點停留在此狀態很長一段時間，請繼續下一步，以查看部署期間是否發生問題。</td>
     </tr>
        <tr>
        <td>Normal</td>
        <td>工作者節點已完整佈建，並已準備好用於叢集。此狀態被視為健全，不需要使用者採取動作。**附註**：雖然工作者節點可能是正常的，但可能仍需注意其他的基礎架構資源（例如[網路](cs_troubleshoot_network.html)和[儲存空間](cs_troubleshoot_storage.html)）。</td>
     </tr>
   <tr>
        <td>Provisioning</td>
        <td>正在佈建工作者節點，因此還無法在叢集中使用。您可以在 CLI 輸出的 <strong>Status</strong> 直欄中監視佈建處理程序。如果工作者節點停留在此狀態很長一段時間，請繼續下一步，以查看佈建期間是否發生問題。</td>
      </tr>
      <tr>
        <td>Provision_failed</td>
        <td>無法佈建工作者節點。請繼續進行下一步，以尋找失敗的詳細資料。</td>
      </tr>
   <tr>
        <td>Reloading</td>
        <td>正在重新載入工作者節點，因此無法在叢集中使用。您可以在 CLI 輸出的 <strong>Status</strong> 直欄中監視重新載入處理程序。如果工作者節點停留在此狀態很長一段時間，請繼續下一步，以查看重新載入期間是否發生問題。</td>
       </tr>
       <tr>
        <td>Reloading_failed</td>
        <td>無法重新載入工作者節點。請繼續進行下一步，以尋找失敗的詳細資料。</td>
      </tr>
      <tr>
        <td>Reload_pending </td>
        <td>傳送重新載入或更新工作者節點的 Kubernetes 版本的要求。重新載入工作者節點時，狀態會變更為 <code>Reloading</code>。</td>
      </tr>
      <tr>
       <td>Unknown</td>
       <td>因下列其中一個原因而無法聯繫 Kubernetes 主節點：<ul><li>您已要求更新 Kubernetes 主節點。在更新期間，無法擷取工作者節點的狀態。</li><li>您可能有另一個防火牆保護工作者節點，或最近變更過防火牆設定。{{site.data.keyword.containershort_notm}} 需要開啟特定 IP 位址及埠，以容許從工作者節點到 Kubernetes 主節點的通訊，反之亦然。如需相關資訊，請參閱[防火牆阻止工作者節點連接](cs_troubleshoot_clusters.html#cs_firewall)。</li><li>Kubernetes 主節點已關閉。請開立 [{{site.data.keyword.Bluemix_notm}} 支援問題單](#ts_getting_help)，以與 {{site.data.keyword.Bluemix_notm}} 支援中心聯絡。</li></ul></td>
  </tr>
     <tr>
        <td>Warning</td>
        <td>工作者節點將達到記憶體或磁碟空間的限制。您可以減少工作者節點上的工作負載，或將工作者節點新增至叢集，以協助對工作負載進行負載平衡。</td>
  </tr>
    </tbody>
  </table>

5.  列出工作者節點的詳細資料。如果詳細資料包括錯誤訊息，請檢閱[工作者節點的一般錯誤訊息](#common_worker_nodes_issues)清單，以瞭解如何解決問題。

   ```
   bx cs worker-get <worker_id>
   ```
   {: pre}

  ```
  bx cs worker-get [<cluster_name_or_id>] <worker_node_id>
  ```
  {: pre}

<br />


## 工作者節點的一般問題
{: #common_worker_nodes_issues}

檢閱一般錯誤訊息，並瞭解其解決方式。

  <table>
  <caption>一般錯誤訊息</caption>
    <thead>
    <th>錯誤訊息</th>
    <th>說明及解決方式
    </thead>
    <tbody>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} 基礎架構異常狀況：您的帳戶目前被禁止訂購「運算實例」。</td>
        <td>您的 IBM Cloud 基礎架構 (SoftLayer) 帳戶可能無法訂購運算資源。請開立 [{{site.data.keyword.Bluemix_notm}} 支援問題單](#ts_getting_help)，以與 {{site.data.keyword.Bluemix_notm}} 支援中心聯絡。</td>
      </tr>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} 基礎架構異常狀況：無法下單。路由器 'router_name' 的資源不足，無法滿足下列來賓的要求：'worker_id'。</td>
        <td>您選取的 VLAN 與空間不足無法佈建工作者節點之資料中心內的 Pod 相關聯。您可以選擇下列選項：<ul><li>使用不同的資料中心來佈建工作者節點。執行 <code>bx cs locations</code> 來列出可用的資料中心。<li>如果您的現有公用及專用 VLAN 配對與資料中心內的另一個 Pod 相關聯，請改用此 VLAN 配對。<li>請開立 [{{site.data.keyword.Bluemix_notm}} 支援問題單](#ts_getting_help)，以與 {{site.data.keyword.Bluemix_notm}} 支援中心聯絡。</ul></td>
      </tr>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} 基礎架構異常狀況：無法取得 ID 為 &lt;vlan id&gt; 的網路 VLAN。</td>
        <td>無法佈建工作者節點，因為因下列其中一個原因而找不到選取的 VLAN ID：<ul><li>您可能已指定 VLAN 號碼，而非 VLAN ID。VLAN 號碼的長度是 3 或 4 位數，而 VLAN ID 的長度是 7 位數。執行 <code>bx cs vlans &lt;location&gt;</code>，以擷取 VLAN ID。<li>VLAN ID 可能未與您使用的 IBM Cloud 基礎架構 (SoftLayer) 帳戶相關聯。執行 <code>bx cs vlans &lt;location&gt;</code>，以列出帳戶的可用 VLAN ID。若要變更 IBM Cloud 基礎架構 (SoftLayer) 帳戶，請參閱 [`bx cs credentials-set`](cs_cli_reference.html#cs_credentials_set)。</ul></td>
      </tr>
      <tr>
        <td>SoftLayer_Exception_Order_InvalidLocation：針對此訂單提供的位置無效。(HTTP 500)</td>
        <td>您的 IBM Cloud 基礎架構 (SoftLayer) 未設定成訂購所選取資料中心內的運算資源。請與 [{{site.data.keyword.Bluemix_notm}} 支援中心](#ts_getting_help)聯絡，驗證已正確設定帳戶。</td>
       </tr>
       <tr>
        <td>{{site.data.keyword.Bluemix_notm}} 基礎架構異常狀況：使用者沒有新增伺服器的必要 {{site.data.keyword.Bluemix_notm}} 基礎架構許可權
</br></br>
        {{site.data.keyword.Bluemix_notm}} 基礎架構異常狀況：必須要有許可權才能訂購「項目」。</td>
        <td>您可能沒有從 IBM Cloud 基礎架構 (SoftLayer) 組合佈建工作者節點的必要許可權。請參閱[配置對 IBM Cloud 基礎架構 (SoftLayer) 組合的存取權以建立標準 Kubernetes 叢集](cs_troubleshoot_clusters.html#cs_credentials)。</td>
      </tr>
      <tr>
       <td>工作者節點無法與 {{site.data.keyword.containershort_notm}} 伺服器交談。請驗證您的防火牆設定容許來自此工作者節點的資料流量。
       <td><ul><li>如果您有防火牆，請[配置防火牆設定，以容許將送出的資料流量傳送至適當的埠和 IP 位址](cs_firewall.html#firewall_outbound)。</li><li>執行 `bx cs workers <mycluster>`，來檢查您的叢集是否沒有公用 IP。如果未列出任何公用 IP，則您的叢集只有專用 VLAN。<ul><li>如果您要叢集只有專用 VLAN，則請設定 [VLAN 連線](cs_clusters.html#worker_vlan_connection)及[防火牆](cs_firewall.html#firewall_outbound)。</li><li>如果您希望叢集具有公用 IP，請同時使用公用及專用 VLAN 來[新增工作者節點](cs_cli_reference.html#cs_worker_add)。</li></ul></li></ul></td>
     </tr>
      <tr>
  <td>無法建立 IMS 入口網站記號，因為沒有 IMS 帳戶鏈結至選取的 BSS 帳戶</br></br>找不到提供的使用者，或使用者非作用中</br></br>SoftLayer_Exception_User_Customer_InvalidUserStatus：使用者帳戶目前為 cancel_pending。</br></br>等待使用者看見機器</td>
  <td>用來存取 IBM Cloud 基礎架構 (SoftLayer) 組合的 API 金鑰擁有者沒有執行此動作的必要許可權，或可能處於擱置刪除狀態。</br></br><strong>身為使用者</strong>，請遵循下列步驟：<ol><li>如果您可以存取多個帳戶，請確定您已登入想要使用 {{site.data.keyword.containerlong_notm}} 的帳戶。</li><li>執行 <code>bx cs api-key-info</code>，以檢視用來存取 IBM Cloud 基礎架構 (SoftLayer) 組合的現行 API 金鑰擁有者。</li><li>執行 <code>bx account list</code>，以檢視您目前使用的 {{site.data.keyword.Bluemix_notm}} 帳戶的擁有者。</li><li>請聯絡 {{site.data.keyword.Bluemix_notm}} 帳戶的擁有者，並報告 API 金鑰擁有者對 IBM Cloud 基礎架構 (SoftLayer) 的許可權不足，或可能處於擱置刪除狀態。</li></ol></br><strong>身為帳戶擁有者</strong>，請遵循下列步驟：<ol><li>請檢閱 [IBM Cloud 基礎架構 (SoftLayer) 中的必要許可權](cs_users.html#infra_access)，以執行先前失敗的動作。</li><li>修正 API 金鑰擁有者的許可權，或使用 [<code>bx cs api-key-reset</code>](cs_cli_reference.html#cs_api_key_reset) 指令來建立新的 API 金鑰。</li><li>如果您或另一位帳戶管理者在您的帳戶中手動設定 IBM Cloud 基礎架構 (SoftLayer) 認證，請執行 [<code>bx cs credentials-unset</code>](cs_cli_reference.html#cs_credentials_unset) 來移除您帳戶中的認證。</li></ol></td>
  </tr>
    </tbody>
  </table>



<br />




## 應用程式部署除錯
{: #debug_apps}

請檢閱您既有的選項以進行應用程式部署除錯，並找出失敗的主要原因。

1. 執行 `describe` 指令，以尋找服務或部署資源中的異常狀況。

 範例：
 <pre class="pre"><code>kubectl describe service &lt;service_name&gt; </code></pre>

2. [檢查容器是否停留在 ContainerCreating 狀態](cs_troubleshoot_storage.html#stuck_creating_state)。

3. 檢查叢集是否處於 `Critical` 狀態。如果叢集處於 `Critical` 狀態中，請檢查防火牆規則，確認主節點可以與工作者節點通訊。

4. 驗證服務正在接聽正確的埠。
   1. 取得 Pod 的名稱。
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. 登入容器。
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   3. 從容器內 Curl 應用程式。如果無法存取埠，服務可能未接聽正確的埠，或是應用程式可能有問題。請使用正確的埠來更新服務的配置檔，並重新部署或調查應用程式的潛在問題。
     <pre class="pre"><code>curl localhost: &lt;port&gt;</code></pre>

5. 驗證服務已正確鏈結至 Pod。
   1. 取得 Pod 的名稱。
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. 登入容器。
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   3. Curl 服務的叢集 IP 位址及埠。如果無法存取 IP 位址及埠，請查看服務的端點。如果未列出端點，則服務的選取器與 Pod 不符。如果列出端點，請查看服務上的目標埠欄位，並確定目標埠與用於 Pod 的埠相同。
     <pre class="pre"><code>curl &lt;cluster_IP&gt;:&lt;port&gt;</code></pre>

6. 對於 Ingress 服務，驗證可以從叢集內存取服務。
   1. 取得 Pod 的名稱。
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. 登入容器。
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   2. Curl 為 Ingress 服務指定的 URL。如果無法存取 URL，請檢查叢集與外部端點之間是否有防火牆問題。
     <pre class="pre"><code>curl &lt;host_name&gt;.&lt;domain&gt;</code></pre>

<br />



## 取得協助及支援
{: #ts_getting_help}

叢集仍有問題？
{: shortdesc}

-   若要查看 {{site.data.keyword.Bluemix_notm}} 是否可用，請[檢查 {{site.data.keyword.Bluemix_notm}} 狀態頁面 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/bluemix/support/#status)。
-   將問題張貼到 [{{site.data.keyword.containershort_notm}} Slack ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://ibm-container-service.slack.com)。

如果您的 {{site.data.keyword.Bluemix_notm}} 帳戶未使用 IBM ID，請[要求邀請](https://bxcs-slack-invite.mybluemix.net/)以加入此 Slack。
    {: tip}
-   檢閱討論區，以查看其他使用者是否發生過相同的問題。使用討論區提問時，請標記您的問題，以便 {{site.data.keyword.Bluemix_notm}} 開發團隊能看到它。

    -   如果您在使用 {{site.data.keyword.containershort_notm}} 開發或部署叢集或應用程式時有技術方面的問題，請將問題張貼到 [Stack Overflow ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers)，並使用 `ibm-cloud`、`kubernetes` 及 `containers` 來標記問題。
    -   若為服務及開始使用指示的相關問題，請使用 [IBM developerWorks dW Answers ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) 討論區。請包含 `ibm-cloud` 及 `containers` 標籤。如需使用討論區的詳細資料，請參閱[取得協助](/docs/get-support/howtogetsupport.html#using-avatar)。

-   開立問題單以與 IBM 支援中心聯絡。若要瞭解開立 IBM 支援問題單或是支援層次與問題單嚴重性，請參閱[與支援中心聯絡](/docs/get-support/howtogetsupport.html#getting-customer-support)。

{: tip}
當您報告問題時，請包含您的叢集 ID。若要取得叢集 ID，請執行 `bx cs clusters`。

