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




# 在防火牆中開啟必要埠及 IP 位址
{: #firewall}

請檢閱下列狀況，在下列狀況時，您可能需要在防火牆中為 {{site.data.keyword.containerlong}} 開啟特定的埠和 IP 位址。
{:shortdesc}

* 當組織網路原則導致無法透過 Proxy 或防火牆存取公用網際網路端點時，從本端系統[執行 `bx` 指令](#firewall_bx)。
* 當組織網路原則導致無法透過 Proxy 或防火牆存取公用網際網路端點時，從本端系統[執行 `kubectl` 指令](#firewall_kubectl)。
* 當組織網路原則導致無法透過 Proxy 或防火牆存取公用網際網路端點時，從本端系統[執行 `calicoctl` 指令](#firewall_calicoctl)。
* 當已針對工作者節點設定防火牆，或是在 IBM Cloud 基礎架構 (SoftLayer) 帳戶中自訂防火牆設定時，[容許 Kubernetes 主節點與工作者節點之間的通訊](#firewall_outbound)。
* [從叢集外部存取 NodePort 服務、LoadBalancer 服務或 Ingress](#firewall_inbound)。

<br />


## 在防火牆的保護下執行 `bx cs` 指令
{: #firewall_bx}

如果組織網路原則導致無法透過 Proxy 或防火牆從本端系統存取公用端點，則若要執行 `bx cs` 指令，您必須容許 {{site.data.keyword.containerlong_notm}} 的 TCP 存取。
{:shortdesc}

1. 容許埠 443 上對 `containers.bluemix.net` 的存取。
2. 驗證連線。如果已正確配置存取，則會在輸出中顯示船。
   ```
   curl https://containers.bluemix.net/v1/
   ```
   {: pre}

   輸出範例：
   ```
                                     )___(
                              _______/__/_
                     ___     /===========|   ___
    ____       __   [\\\]___/____________|__[///]   __
    \   \_____[\\]__/___________________________\__[//]___
     \                                                    |
      \                                                  /
   ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ```
   {: screen}

<br />


## 在防火牆的保護下執行 `kubectl` 指令
{: #firewall_kubectl}

如果組織網路原則導致無法透過 Proxy 或防火牆從本端系統存取公用端點，則若要執行 `kubectl` 指令，您必須容許叢集的 TCP 存取。
{:shortdesc}

建立叢集時，會從 20000-32767 內隨機指派主要 URL 中的埠。您可以針對任何可能建立的叢集選擇開啟埠範圍 20000-32767，或選擇容許存取特定現有叢集。

開始之前，請容許[執行 `bx cs` 指令](#firewall_bx)的存取。

若要容許存取特定叢集，請執行下列動作：

1. 登入 {{site.data.keyword.Bluemix_notm}} CLI。系統提示時，請輸入您的 {{site.data.keyword.Bluemix_notm}} 認證。如果您有聯合帳戶，請包含 `--sso` 選項。

   ```
    bx login [--sso]
    ```
   {: pre}

2. 選取您叢集所在的地區。

   ```
   bx cs region-set
   ```
   {: pre}

3. 取得叢集的名稱。

   ```
      bx cs clusters
      ```
   {: pre}

4. 擷取叢集的**主要 URL**。

   ```
   bx cs cluster-get <cluster_name_or_ID>
   ```
   {: pre}

   輸出範例：
   ```
   ...
   Master URL:		https://169.xx.xxx.xxx:31142
   ...
   ```
   {: screen}

5. 容許在埠上存取**主要 URL**，例如來自前一個範例的埠 `31142`。

6. 驗證連線。

   ```
   curl --insecure <master_URL>/version
   ```
   {: pre}

   範例指令：
   ```
   curl --insecure https://169.xx.xxx.xxx:31142/version
   ```
   {: pre}

   輸出範例：
   ```
   {
     "major": "1",
     "minor": "7+",
     "gitVersion": "v1.7.4-2+eb9172c211dc41",
     "gitCommit": "eb9172c211dc4108341c0fd5340ee5200f0ec534",
     "gitTreeState": "clean",
     "buildDate": "2017-11-16T08:13:08Z",
     "goVersion": "go1.8.3",
     "compiler": "gc",
     "platform": "linux/amd64"
   }
   ```
   {: screen}

7. 選用項目：針對您需要公開的每一個叢集，重複這些步驟。

<br />


## 在防火牆的保護下執行 `calicoctl` 指令
{: #firewall_calicoctl}

如果組織網路原則導致無法透過 Proxy 或防火牆從本端系統存取公用端點，則若要執行 `calicoctl` 指令，您必須容許 Calico 指令的 TCP 存取。
{:shortdesc}

開始之前，請容許執行 [`bx` 指令](#firewall_bx)及 [`kubectl` 指令](#firewall_kubectl)的存取。

1. 從用來容許 [`kubectl` 指令](#firewall_kubectl)的主要 URL 中擷取 IP 位址。

2. 取得 ETCD 的埠。

  ```
  kubectl get cm -n kube-system calico-config -o yaml | grep etcd_endpoints
  ```
  {: pre}

3. 容許透過主要 URL IP 位址及 ETCD 埠存取 Calico 原則。

<br />


## 容許叢集存取基礎架構資源及其他服務
{: #firewall_outbound}

讓您的叢集從防火牆之後存取基礎架構資源及服務，例如 {{site.data.keyword.containershort_notm}} 地區、{{site.data.keyword.registrylong_notm}}、{{site.data.keyword.monitoringlong_notm}}、{{site.data.keyword.loganalysislong_notm}}、IBM Cloud 基礎架構 (SoftLayer) 專用 IP，以及持續性磁區要求的出口。
{:shortdesc}

  1.  記下叢集中所有工作者節點的公用 IP 位址。

      ```
             bx cs workers <cluster_name_or_ID>
       ```
      {: pre}

  2.  容許從來源 _<each_worker_node_publicIP>_ 到目的地 TCP/UDP 埠範圍 20000-32767 和埠 443 以及下列 IP 位址和網路群組的送出網路資料流量。如果您的組織防火牆導致本端機器無法存取公用網際網路端點，請針對來源工作者節點及本端機器執行此步驟。
      - **重要事項**：對於地區內的所有位置，您必須容許對埠 443 的送出資料流量，以平衡引導處理程序期間的負載。例如，如果您的叢集是在美國南部，則對於所有位置（dal10、dal12 及 dal13），您必須容許從埠 443 到 IP 位址的資料流量。
      <p>
  <table summary="表格中的第一列跨這兩個直欄。其餘的列應該從左到右閱讀，第一欄為伺服器位置，第二欄則為要符合的 IP 位址。">
  <caption>針對送出資料流量開啟的 IP 位址</caption>
      <thead>
      <th>地區</th>
      <th>位置</th>
      <th>IP 位址</th>
      </thead>
    <tbody>
      <tr>
        <td>亞太地區北部</td>
        <td>hkg02<br>seo01<br>sng01<br>tok02</td>
        <td><code>169.56.132.234</code><br><code>169.56.69.242</code><br><code>161.202.186.226</code><br><code>161.202.126.210</code></td>
       </tr>
      <tr>
         <td>亞太地區南部</td>
         <td>mel01<br>syd01<br>syd04</td>
         <td><code>168.1.97.67</code><br><code>168.1.8.195</code><br><code>130.198.64.19, 130.198.66.34</code></td>
      </tr>
      <tr>
         <td>歐盟中部</td>
         <td>ams03<br>fra02<br>mil01<br>par01</td>
         <td><code>169.50.169.110, 169.50.154.194</code><br><code>169.50.56.174</code><br><code>159.122.190.98</code><br><code>159.8.86.149, 159.8.98.170</code></td>
        </tr>
      <tr>
        <td>英國南部</td>
        <td>lon02<br>lon04</td>
        <td><code>159.122.242.78</code><br><code>158.175.65.170, 158.175.74.170, 158.175.76.2</code></td>
      </tr>
      <tr>
        <td>美國東部</td>
         <td>mon01<br>tor01<br>wdc06<br>wdc07</td>
         <td><code>169.54.126.219</code><br><code>169.53.167.50</code><br><code>169.60.73.142</code><br><code>169.61.83.62</code></td>
      </tr>
      <tr>
        <td>美國南部</td>
        <td>dal10<br>dal12<br>dal13<br>hou02<br>sao01</td>
        <td><code>169.47.234.18, 169.46.7.238</code><br><code>169.47.70.10</code><br><code>169.60.128.2</code><br><code>184.173.44.62</code><br><code>169.57.151.10</code></td>
      </tr>
      </tbody>
    </table>
</p>

  3.  容許從工作者節點到 [{{site.data.keyword.registrylong_notm}} 地區](/docs/services/Registry/registry_overview.html#registry_regions)的送出網路資料流量：
      - `TCP port 443 FROM <each_worker_node_publicIP> TO <registry_publicIP>`
      - 將 <em>&lt;registry_publicIP&gt;</em> 取代為您要容許資料流量的登錄 IP 位址。全球登錄會儲存 IBM 提供的公用映像檔，地區登錄會儲存您自己的專用或公用映像檔。
        <p>
<table summary="表格中的第一列跨這兩個直欄。其餘的列應該從左到右閱讀，第一欄為伺服器位置，第二欄則為要符合的 IP 位址。">
  <caption>針對「登錄」資料流量開啟的 IP 位址</caption>
      <thead>
        <th>{{site.data.keyword.containershort_notm}} 地區</th>
        <th>登錄位址</th>
        <th>登錄 IP 位址</th>
      </thead>
      <tbody>
        <tr>
          <td>跨 {{site.data.keyword.containershort_notm}} 地區的全球登錄</td>
          <td>registry.bluemix.net</td>
          <td><code>169.60.72.144/28</code><br><code>169.61.76.176/28</code></td>
        </tr>
        <tr>
          <td>亞太地區北部、亞太地區南部</td>
          <td>registry.au-syd.bluemix.net</td>
          <td><code>168.1.45.160/27</code></br><code>168.1.139.32/27</code></td>
        </tr>
        <tr>
          <td>歐盟中部</td>
          <td>registry.eu-de.bluemix.net</td>
          <td><code>169.50.56.144/28</code></br><code>159.8.73.80/28</code></td>
         </tr>
         <tr>
          <td>英國南部</td>
          <td>registry.eu-gb.bluemix.net</td>
          <td><code>159.8.188.160/27</code></br><code>169.50.153.64/27</code></td>
         </tr>
         <tr>
          <td>美國東部、美國南部</td>
          <td>registry.ng.bluemix.net</td>
          <td><code>169.55.39.112/28</code></br><code>169.46.9.0/27</code></br><code>169.55.211.0/27</code></td>
         </tr>
        </tbody>
      </table>
</p>

  4.  選用項目：容許從工作者節點到 {{site.data.keyword.monitoringlong_notm}} 及 {{site.data.keyword.loganalysislong_notm}} 服務的送出網路資料流量：
      - `TCP port 443, port 9095 FROM <each_worker_node_public_IP> TO <monitoring_public_IP>`
      - 將 <em>&lt;monitoring_public_IP&gt;</em> 取代為您要容許資料流量的監視地區的所有位址：
        <p><table summary="表格中的第一列跨這兩個直欄。其餘的列應該從左到右閱讀，第一欄為伺服器位置，第二欄則為要符合的 IP 位址。">
  <caption>針對監視資料流量開啟的 IP 位址</caption>
        <thead>
        <th>{{site.data.keyword.containershort_notm}} 地區</th>
        <th>監視位址</th>
        <th>監視 IP 位址</th>
        </thead>
      <tbody>
        <tr>
         <td>歐盟中部</td>
         <td>metrics.eu-de.bluemix.net</td>
         <td><code>159.122.78.136/29</code></td>
        </tr>
        <tr>
         <td>英國南部</td>
         <td>metrics.eu-gb.bluemix.net</td>
         <td><code>169.50.196.136/29</code></td>
        </tr>
        <tr>
          <td>美國東部、美國南部、亞太地區北部</td>
          <td>metrics.ng.bluemix.net</td>
          <td><code>169.47.204.128/29</code></td>
         </tr>
         
        </tbody>
      </table>
</p>
      - `TCP port 443, port 9091 FROM <each_worker_node_public_IP> TO <logging_public_IP>`
      - 將 <em>&lt;logging_public_IP&gt;</em> 取代為您要容許資料流量的記載地區的所有位址：
        <p><table summary="表格中的第一列跨這兩個直欄。其餘的列應該從左到右閱讀，第一欄為伺服器位置，第二欄則為要符合的 IP 位址。">
  <caption>針對記載資料流量開啟的 IP 位址</caption>
        <thead>
        <th>{{site.data.keyword.containershort_notm}} 地區</th>
        <th>記載位址</th>
        <th>記載 IP 位址</th>
        </thead>
        <tbody>
          <tr>
            <td>美國東部、美國南部</td>
            <td>ingest.logging.ng.bluemix.net</td>
            <td><code>169.48.79.236</code><br><code>169.46.186.113</code></td>
           </tr>
          <tr>
           <td>英國南部</td>
           <td>ingest.logging.eu-gb.bluemix.net</td>
           <td><code>169.50.115.113</code></td>
          </tr>
          <tr>
           <td>歐盟中部</td>
           <td>ingest-eu-fra.logging.bluemix.net</td>
           <td><code>158.177.88.43</code><br><code>159.122.87.107</code></td>
          </tr>
          <tr>
           <td>亞太地區南部、亞太地區北部</td>
           <td>ingest-au-syd.logging.bluemix.net</td>
           <td><code>130.198.76.125</code><br><code>168.1.209.20</code></td>
          </tr>
         </tbody>
       </table>
</p>

  5. 對於專用防火牆，容許適當的 IBM Cloud 基礎架構 (SoftLayer) 專用 IP 範圍。請參閱[此鏈結](https://knowledgelayer.softlayer.com/faq/what-ip-ranges-do-i-allow-through-firewall)，從 **Backend (private) Network** 小節開始。
      - 新增所使用[地區內的所有位置](cs_regions.html#locations)。
      - 請注意，您必須新增 dal01 位置（資料中心）。
      - 開啟埠 80 及 443，以容許叢集引導處理程序。

  6. {: #pvc}若要建立資料儲存空間的持續性磁區宣告，請透過防火牆容許對叢集所在位置（資料中心）的 [IBM Cloud 基礎架構 (SoftLayer) IP 位址](https://knowledgelayer.softlayer.com/faq/what-ip-ranges-do-i-allow-through-firewall)進行 Egress 存取。
      - 若要尋找叢集的位置（資料中心），請執行 `bx cs clusters`。
      - 容許存取**前端（公用）網路**及**後端（專用）網路**的 IP 範圍。
      - 請注意，您必須針對**後端（專用）網路**新增 dal01 位置（資料中心）。

<br />


## 從叢集外部存取 NodePort、負載平衡器及 Ingress 服務
{: #firewall_inbound}

您可以容許對 NodePort、負載平衡器及 Ingress 服務進行送入存取。
{:shortdesc}

<dl>
  <dt>NodePort 服務</dt>
  <dd>開啟您在將服務部署至所有工作者節點的公用 IP 位址時所配置的埠，以容許接收資料流量。若要尋找埠，請執行 `kubectl get svc`。該埠在 20000-32000 的範圍內。<dd>
  <dt>LoadBalancer 服務</dt>
  <dd>開啟您在將服務部署至負載平衡器服務的公用 IP 位址時所配置的埠。</dd>
  <dt>Ingress</dt>
  <dd>針對 Ingress 應用程式負載平衡器的 IP 位址，開啟埠 80（適用於 HTTP）或埠 443（適用於 HTTPS）。</dd>
</dl>
