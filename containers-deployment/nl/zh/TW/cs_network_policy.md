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



# 使用網路原則控制資料流量
{: #network_policies}

每個 Kubernetes 叢集都會設定稱為 Calico 的網路外掛程式。已設定預設的網路原則來保護 {{site.data.keyword.containerlong}} 中每個工作者節點的公用網路介面。
{: shortdesc}

如果您有獨特的安全需求，則可以使用 Calico 及 Kubernetes 來建立叢集的網路原則。使用 Kubernetes 網路原則，您可以指定要容許或封鎖進出叢集內 Pod 的網路資料流量。若要設定其他進階網路原則，例如封鎖入埠 (Ingress) 資料流量至 LoadBalancer 服務，請使用 Calico 網路原則。

<ul>
  <li>
  [Kubernetes 網路原則 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/services-networking/network-policies/)：這些原則指定 Pod 如何與其他 Pod 及外部端點通訊。自 Kubernetes 1.8 版開始，可根據通訊協定、埠及來源或目的地 IP 位址，容許或封鎖送入及送出的網路資料流量。也可以根據 Pod 及名稱空間標籤來過濾資料流量。您可以使用 `kubectl` 指令或 Kubernetes API 來套用 Kubernetes 網路原則。這些原則在套用時會自動轉換為 Calico 網路原則，而 Calico 會強制執行這些原則。</li>
  <li>
  Kubernetes [1.10 版以及更新版本叢集 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.projectcalico.org/v3.1/getting-started/kubernetes/tutorials/advanced-policy) 或 [1.9 版以及更早版本叢集 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.projectcalico.org/v2.6/getting-started/kubernetes/tutorials/advanced-policy) 的 Calico 網路原則：這些原則是 Kubernetes 網路原則的超集，並使用 `calicoctl` 指令來套用。Calico 原則新增下列特性。
    <ul>
    <li>不論 Kubernetes Pod 來源或目的地 IP 位址或 CIDR，都容許或封鎖特定網路介面上的網路資料流量。</li>
    <li>容許或封鎖名稱空間的 Pod 之間的網路資料流量。</li>
    <li>[封鎖 LoadBalancer 或 NodePort Kubernetes 服務的入埠 (Ingress) 資料流量](#block_ingress)。</li>
    </ul>
  </li>
  </ul>

Calico 透過在 Kubernetes 工作者節點上設定 Linux iptables 規則，來強制執行這些原則（包括任何會自動轉換為 Calico 原則的 Kubernetes 網路原則）。iptables 規則作為工作者節點的防火牆，以定義網路資料流量必須符合才能轉遞至目標資源的特徵。

<br />


## 預設 Calico 及 Kubernetes 網路原則
{: #default_policy}

建立具有公用 VLAN 的叢集時，會針對每一個工作者節點及其公用網路介面，自動建立具有 `ibm.role: worker_public` 標籤的 HostEndpoint 資源。若要保護工作者節點的公用網路介面，預設 Calico 原則會套用至任何具有 `ibm.role: worker_public` 標籤的主機端點。
{:shortdesc}

這些預設 Calico 原則容許所有出埠網路資料流量，並容許特定叢集元件（例如 Kubernetes NodePort、LoadBalancer 及 Ingress 服務）的入埠資料流量。會封鎖從網際網路至工作者節點的任何其他入埠網路資料流量（預設原則中未指定的）。預設原則不會影響 Pod 至 Pod 的資料流量。

檢閱下列自動套用至叢集的預設 Calico 網路原則。

**重要事項：**除非您完全瞭解原則，否則請不要移除套用至主機端點的原則。確定您不需要原則所容許的資料流量。

 <table summary="表格中的第一列跨這兩個直欄。請由左至右讀取其餘的列，第一欄為伺服器位置，第二欄則為要符合的 IP 位址。">
 <caption>每一個叢集的預設 Calico 原則</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 每一個叢集的預設 Calico 原則</th>
  </thead>
  <tbody>
    <tr>
      <td><code>allow-all-outbound</code></td>
      <td>容許所有出埠資料流量。</td>
    </tr>
    <tr>
      <td><code>allow-bixfix-port</code></td>
      <td>容許埠 52311 上 BigFix 應用程式的送入資料流量，以容許必要的工作者節點更新。</td>
    </tr>
    <tr>
      <td><code>allow-icmp</code></td>
      <td>容許送入的 ICMP 封包 (ping)。</td>
     </tr>
    <tr>
      <td><code>allow-node-port-dnat</code></td>
      <td>容許 NodePort、LoadBalancer 及 Ingress 服務將公開至 Pod 的送入 NodePort、LoadBalancer 及 Ingress 服務的資料流量。<strong>附註</strong>：您不需要指定公開的埠，因為 Kubernetes 會使用目的地網址轉譯 (DNAT) 將服務要求轉遞至正確的 Pod。該轉遞是在 iptables 套用主機端點原則之前進行。</td>
   </tr>
   <tr>
      <td><code>allow-sys-mgmt</code></td>
      <td>容許用來管理工作者節點之特定 IBM Cloud 基礎架構 (SoftLayer) 系統的送入連線。</td>
   </tr>
   <tr>
    <td><code>allow-vrrp</code></td>
    <td>容許 VRRP 封包，用來監視及移動工作者節點之間的虛擬 IP 位址。</td>
   </tr>
  </tbody>
</table>

在 Kibernetes 1.10 版以及更新版本的叢集中，還建立了預設 Kubernetes 原則，用來限制對「Kubernetes 儀表板」的存取。Kubernetes 原則不適用於主機端點，反而適用於 `kube-dashboard` Pod。此原則適用於僅連接至專用 VLAN 的叢集，以及連接至公用及專用 VLAN 的叢集。

<table>
<caption>每一個叢集的預設 Kubernetes 原則</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 每一個叢集的預設 Kubernetes 原則</th>
</thead>
<tbody>
 <tr>
  <td><code>kubernetes-dashboard</code></td>
  <td><b>僅限在 Kubernetes 1.10 版</b>（於 <code>kube-system</code> 名稱空間中提供）：封鎖所有 Pod 存取「Kubernetes 儀表板」。此原則不會影響從 {{site.data.keyword.Bluemix_notm}} 使用者介面，或使用 <code>kubectl proxy</code> 存取儀表板。如果 Pod 需要存取儀表板，請在名稱空間中部署具有 <code>kubernetes-dashboard-policy: allow</code> 標籤的 Pod。</td>
 </tr>
</tbody>
</table>

<br />


## 安裝並配置 Calico CLI
{: #cli_install}

若要檢視、管理及新增 Calico 原則，請安裝並配置 Calico CLI。
{:shortdesc}

CLI 配置及原則的 Calico 版本相容性會根據您叢集的 Kubernetes 版本而不同。若要安裝並配置 Calico CLI，請根據您的叢集版本按一下下列其中一個鏈結：

* [Kubernetes 1.10 版或更新版本的叢集](#1.10_install)
* [Kubernetes 1.9 版或更早版本的叢集](#1.9_install)

將叢集從 Kiberneves 1.9 版或更早版本更新至 1.10 版或更新版本之前，請檢閱[準備更新至 Calico 第 3 版](cs_versions.html#110_calicov3)。
{: tip}

### 針對執行 Kubernets 1.10 版或更新版本的叢集，安裝並配置 3.1.1 版 Calico CLI
{: #1.10_install}

開始之前，請[將 Kubernetes CLI 的目標設為叢集](cs_cli_install.html#cs_cli_configure)。請在 `bx cs cluster-config` 指令包含 `--admin` 選項，這用來下載憑證及許可權檔案。此下載還包括「超級使用者」角色的金鑰，您需要此金鑰才能執行 Calico 指令。

  ```
  bx cs cluster-config <cluster_name> --admin
  ```
  {: pre}

若要安裝並配置 3.1.1 Calico CLI，請執行下列動作：

1. [下載 Calico CLI ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/projectcalico/calicoctl/releases/tag/v3.1.1)。

    如果您使用的是 OSX，請下載 `-darwin-amd64` 版本。如果您使用的是 Windows，請將 Calico CLI 安裝在與 {{site.data.keyword.Bluemix_notm}} CLI 相同的目錄中。當您稍後執行指令時，此設定可為您省去一些檔案路徑變更。{: tip}

2. 若為 OSX 及 Linux 使用者，請完成下列步驟。
    1. 將執行檔移至 _/usr/local/bin_ 目錄。
        - Linux：

          ```
                        mv filepath/calicoctl /usr/local/bin/calicoctl
              ```
          {: pre}

        - OS X：

          ```
                        mv filepath/calicoctl-darwin-amd64 /usr/local/bin/calicoctl
              ```
          {: pre}

    2. 讓檔案成為可執行檔。

        ```
                    chmod +x /usr/local/bin/calicoctl
            ```
        {: pre}

3. 檢查 Calico CLI 用戶端版本，驗證已適當地執行 `calico` 指令。

    ```
            calicoctl version
        ```
    {: pre}

4. 如果組織網路原則使用 Proxy 或防火牆，來阻止從本端系統存取公用端點，請[容許 Calico 指令的 TCP 存取](cs_firewall.html#firewall)。

5. 若為 Linux 及 OS X，請建立 `/etc/calico` 目錄。若為 Windows，任何目錄皆可使用。

  ```
      sudo mkdir -p /etc/calico/
      ```
  {: pre}

6. 建立 `calicoctl.cfg` 檔案。
    - Linux 及 OS X：

      ```
                sudo vi /etc/calico/calicoctl.cfg
          ```
      {: pre}

    - Windows：使用文字編輯器建立檔案。

7. 在 <code>calicoctl.cfg</code> 檔案中，輸入下列資訊。

    ```
    apiVersion: projectcalico.org/v3
    kind: CalicoAPIConfig
    metadata:
    spec:
        datastoreType: etcdv3
        etcdEndpoints: <ETCD_URL>
        etcdKeyFile: <CERTS_DIR>/admin-key.pem
        etcdCertFile: <CERTS_DIR>/admin.pem
        etcdCACertFile: <CERTS_DIR>/<ca-*pem_file>
    ```
    {: codeblock}

    1. 擷取 `<ETCD_URL>`。

      - Linux 及 OS X：

          ```
                        kubectl get cm -n kube-system calico-config -o yaml | grep "etcd_endpoints:" | awk '{ print $2 }'
              ```
          {: pre}

          輸出範例：

          ```
                        https://169.xx.xxx.xxx:30001
              ```
          {: screen}

      - Windows：<ol>
        <li>從 configmap 取得 calico 配置值。</br><pre class="codeblock"><code>kubectl get cm -n kube-system calico-config -o yaml</code></pre></br>
        <li>在 `data` 區段中，找到 etcd_endpoints 值。範例：<code>https://169.xx.xxx.xxx:30001</code>
            </ol>

    2. 擷取 `<CERTS_DIR>`，這是將 Kubernetes 憑證下載至其中的目錄。

        - Linux 及 OS X：

          ```
                        dirname $KUBECONFIG
              ```
          {: pre}

          輸出範例：

          ```
                        /home/sysadmin/.bluemix/plugins/container-service/clusters/<cluster_name>-admin/
              ```
          {: screen}

        - Windows：

          ```
                        ECHO %KUBECONFIG%
              ```
          {: pre}

            輸出範例：

          ```
                        C:/Users/<user>/.bluemix/plugins/container-service/mycluster-admin/kube-config-prod-dal10-mycluster.yml
              ```
          {: screen}

        **附註**：若要取得目錄路徑，請移除輸出結尾中的檔名 `kube-config-prod-<location>-<cluster_name>.yml`。

    3. 擷取 `ca-*pem_file`。

        - Linux 及 OS X：

          ```
          ls `dirname $KUBECONFIG` | grep "ca-"
          ```
          {: pre}

        - Windows：<ol><li>開啟您在最後一個步驟中擷取的目錄。</br><pre class="codeblock"><code>C:\Users\<user>\.bluemix\plugins\container-service\&lt;cluster_name&gt;-admin\</code></pre>
          <li> 找出 <code>ca-*pem_file</code> 檔案。</ol>

    4. 驗證 Calico 配置正確運作。

        - Linux 及 OS X：

          ```
                        calicoctl get nodes
              ```
          {: pre}

        - Windows：

          ```
                        calicoctl get nodes --config=filepath/calicoctl.cfg
              ```
          {: pre}

          輸出：

          ```
                        NAME
              kube-dal10-crc21191ee3997497ca90c8173bbdaf560-w1.cloud.ibm
              kube-dal10-crc21191ee3997497ca90c8173bbdaf560-w2.cloud.ibm
              kube-dal10-crc21191ee3997497ca90c8173bbdaf560-w3.cloud.ibm
              ```
          {: screen}


### 針對執行 Kubernets 1.9 版或更早版本的叢集，安裝並配置 1.6.3 版 Calico CLI
{: #1.9_install}

開始之前，請[將 Kubernetes CLI 的目標設為叢集](cs_cli_install.html#cs_cli_configure)。請在 `bx cs cluster-config` 指令包含 `--admin` 選項，這用來下載憑證及許可權檔案。此下載還包括「超級使用者」角色的金鑰，您需要此金鑰才能執行 Calico 指令。

  ```
  bx cs cluster-config <cluster_name> --admin
  ```
  {: pre}

若要安裝並配置 1.6.3 Calico CLI，請執行下列動作：

1. [下載 Calico CLI ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/projectcalico/calicoctl/releases/tag/v1.6.3)。

    如果您使用的是 OSX，請下載 `-darwin-amd64` 版本。如果您使用的是 Windows，請將 Calico CLI 安裝在與 {{site.data.keyword.Bluemix_notm}} CLI 相同的目錄中。當您稍後執行指令時，此設定可為您省去一些檔案路徑變更。{: tip}

2. 若為 OSX 及 Linux 使用者，請完成下列步驟。
    1. 將執行檔移至 _/usr/local/bin_ 目錄。
        - Linux：

          ```
                        mv filepath/calicoctl /usr/local/bin/calicoctl
              ```
          {: pre}

        - OS X：

          ```
                        mv filepath/calicoctl-darwin-amd64 /usr/local/bin/calicoctl
              ```
          {: pre}

    2. 讓檔案成為可執行檔。

        ```
                    chmod +x /usr/local/bin/calicoctl
            ```
        {: pre}

3. 檢查 Calico CLI 用戶端版本，驗證已適當地執行 `calico` 指令。

    ```
            calicoctl version
        ```
    {: pre}

4. 如果組織網路原則使用 Proxy 或防火牆，來阻止從本端系統存取公用端點：請參閱[從防火牆後面執行 `calicoctl` 指令](cs_firewall.html#firewall)，以取得如何容許 Calico 指令的 TCP 存取的指示。

5. 若為 Linux 及 OS X，請建立 `/etc/calico` 目錄。若為 Windows，任何目錄皆可使用。
    ```
    sudo mkdir -p /etc/calico/
    ```
    {: pre}

6. 建立 `calicoctl.cfg` 檔案。
    - Linux 及 OS X：

      ```
      sudo vi /etc/calico/calicoctl.cfg
      ```
      {: pre}

    - Windows：使用文字編輯器建立檔案。

7. 在 <code>calicoctl.cfg</code> 檔案中，輸入下列資訊。

    ```
    apiVersion: v1
    kind: calicoApiConfig
    metadata:
    spec:
        etcdEndpoints: <ETCD_URL>
        etcdKeyFile: <CERTS_DIR>/admin-key.pem
        etcdCertFile: <CERTS_DIR>/admin.pem
        etcdCACertFile: <CERTS_DIR>/<ca-*pem_file>
    ```
    {: codeblock}

    1. 擷取 `<ETCD_URL>`。

      - Linux 及 OS X：

          ```
          kubectl get cm -n kube-system calico-config -o yaml | grep "etcd_endpoints:" | awk '{ print $2 }'
          ```
          {: pre}

      - 輸出範例：

          ```
          https://169.xx.xxx.xxx:30001
          ```
          {: screen}

      - Windows：<ol>
        <li>從 configmap 取得 calico 配置值。</br><pre class="codeblock"><code>kubectl get cm -n kube-system calico-config -o yaml</code></pre></br>
        <li>在 `data` 區段中，找到 etcd_endpoints 值。範例：<code>https://169.xx.xxx.xxx:30001</code>
            </ol>

    2. 擷取 `<CERTS_DIR>`，這是將 Kubernetes 憑證下載至其中的目錄。

        - Linux 及 OS X：

          ```
          dirname $KUBECONFIG
          ```
          {: pre}

          輸出範例：

          ```
          /home/sysadmin/.bluemix/plugins/container-service/clusters/<cluster_name>-admin/
          ```
          {: screen}

        - Windows：

          ```
          ECHO %KUBECONFIG%
          ```
          {: pre}

          輸出範例：

          ```
          C:/Users/<user>/.bluemix/plugins/container-service/mycluster-admin/kube-config-prod-dal10-mycluster.yml
          ```
          {: screen}

        **附註**：若要取得目錄路徑，請移除輸出結尾中的檔名 `kube-config-prod-<location>-<cluster_name>.yml`。

    3. 擷取 `ca-*pem_file`。

        - Linux 及 OS X：

          ```
          ls `dirname $KUBECONFIG` | grep "ca-"
          ```
          {: pre}

        - Windows：<ol><li>開啟您在最後一個步驟中擷取的目錄。</br><pre class="codeblock"><code>C:\Users\<user>\.bluemix\plugins\container-service\&lt;cluster_name&gt;-admin\</code></pre>
          <li> 找出 <code>ca-*pem_file</code> 檔案。</ol>

    4. 驗證 Calico 配置正確運作。

        - Linux 及 OS X：

          ```
          calicoctl get nodes
          ```
          {: pre}

        - Windows：

          ```
          calicoctl get nodes --config=filepath/calicoctl.cfg
          ```
          {: pre}

          輸出：

          ```
          NAME
          kube-dal10-crc21191ee3997497ca90c8173bbdaf560-w1.cloud.ibm
          kube-dal10-crc21191ee3997497ca90c8173bbdaf560-w2.cloud.ibm
          kube-dal10-crc21191ee3997497ca90c8173bbdaf560-w3.cloud.ibm
          ```
          {: screen}

<br />


## 檢視網路原則
{: #view_policies}

檢視預設值的詳細資料，以及任何已套用至叢集的新增網路原則。
{:shortdesc}

開始之前：
1. [安裝並配置 Calico CLI。](#cli_install)
2. [將 Kubernetes CLI 的目標設為叢集](cs_cli_install.html#cs_cli_configure)。請在 `bx cs cluster-config` 指令包含 `--admin` 選項，這用來下載憑證及許可權檔案。此下載還包括「超級使用者」角色的金鑰，您需要此金鑰才能執行 Calico 指令。
    ```
    bx cs cluster-config <cluster_name> --admin
    ```
    {: pre}

CLI 配置及原則的 Calico 版本相容性會根據您叢集的 Kubernetes 版本而不同。若要安裝並配置 Calico CLI，請根據您的叢集版本按一下下列其中一個鏈結：

* [Kubernetes 1.10 版或更新版本的叢集](#1.10_examine_policies)
* [Kubernetes 1.9 版或更早版本的叢集](#1.9_examine_policies)

將叢集從 Kiberneves 1.9 版或更早版本更新至 1.10 版或更新版本之前，請檢閱[準備更新至 Calico 第 3 版](cs_versions.html#110_calicov3)。
{: tip}

### 檢視執行 Kubernetes 1.10 版或更新版本之叢集中的網路原則
{: #1.10_examine_policies}

1. 檢視 Calico 主機端點。

    ```
    calicoctl get hostendpoint -o yaml
    ```
    {: pre}

2. 檢視已為叢集建立的所有 Calico 及 Kubernetes 網路原則。這份清單包括可能尚未套用至任何 Pod 或主機的原則。若要強制執行網路原則，則必須找到符合 Calico 網路原則中所定義之選取器的 Kubernetes 資源。

    [網路原則 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/networkpolicy) 的範圍會限制為特定的名稱空間：
    ```
    calicoctl get NetworkPolicy --all-namespaces -o wide
    ```

    [廣域網路原則 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/globalnetworkpolicy) 的範圍不會限制為特定的名稱空間：
    ```
    calicoctl get GlobalNetworkPolicy -o wide
    ```
    {: pre}

3. 檢視網路原則的詳細資料。

    ```
    calicoctl get NetworkPolicy -o yaml <policy_name> --namespace <policy_namespace>
    ```
    {: pre}

4. 檢視叢集之所有廣域網路原則的詳細資料。

    ```
    calicoctl get GlobalNetworkPolicy -o yaml
    ```
    {: pre}

### 檢視執行 Kubernetes 1.9 版或更早版本之叢集中的網路原則
{: #1.9_examine_policies}

1. 檢視 Calico 主機端點。

    ```
    calicoctl get hostendpoint -o yaml
    ```
    {: pre}

2. 檢視已為叢集建立的所有 Calico 及 Kubernetes 網路原則。這份清單包括可能尚未套用至任何 Pod 或主機的原則。若要強制執行網路原則，則必須找到符合 Calico 網路原則中所定義之選取器的 Kubernetes 資源。

    ```
    calicoctl get policy -o wide
    ```
    {: pre}

3. 檢視網路原則的詳細資料。

    ```
    calicoctl get policy -o yaml <policy_name>
    ```
    {: pre}

4. 檢視叢集的所有網路原則的詳細資料。

    ```
    calicoctl get policy -o yaml
    ```
    {: pre}

<br />


## 新增網路原則
{: #adding_network_policies}

在大部分情況下，不需要變更預設原則。只有進階情境會有可能需要變更。如果發現必須進行變更，則您可以建立自己的網路原則。
{:shortdesc}

若要建立 Kubernetes 網路原則，請參閱 [Kubernetes 網路原則文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/services-networking/network-policies/)。

若要建立 Calico 原則，請使用下列步驟。

開始之前：
1. [安裝並配置 Calico CLI。](#cli_install)
2. [將 Kubernetes CLI 的目標設為叢集](cs_cli_install.html#cs_cli_configure)。請在 `bx cs cluster-config` 指令包含 `--admin` 選項，這用來下載憑證及許可權檔案。此下載還包括「超級使用者」角色的金鑰，您需要此金鑰才能執行 Calico 指令。
    ```
    bx cs cluster-config <cluster_name> --admin
    ```
    {: pre}

CLI 配置及原則的 Calico 版本相容性會根據您叢集的 Kubernetes 版本而不同。根據您的叢集版本，按一下下列其中一個鏈結：

* [Kubernetes 1.10 版或更新版本的叢集](#1.10_create_new)
* [Kubernetes 1.9 版或更早版本的叢集](#1.9_create_new)

將叢集從 Kiberneves 1.9 版或更早版本更新至 1.10 版或更新版本之前，請檢閱[準備更新至 Calico 第 3 版](cs_versions.html#110_calicov3)。
{: tip}

### 在執行 Kubernetes 1.10 版或更新版本的叢集中新增 Calico 原則
{: #1.10_create_new}

1. 定義您的 Calico [網路原則 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/networkpolicy) 或 [廣域網路原則 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/globalnetworkpolicy)，方法為建立配置 Script (`.yaml`)。這些配置檔包含選取器，其說明這些原則適用的 Pod、名稱空間或主機。請參閱這些 [Calico 原則範例 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://docs.projectcalico.org/v3.1/getting-started/kubernetes/tutorials/advanced-policy)，以協助您建立自己的原則。**附註**：Kubernetes 1.10 版或更新版本的叢集必須使用 Calico 第 3 版原則語法。

2. 將原則套用至叢集。
    - Linux 及 OS X：

      ```
      calicoctl apply -f policy.yaml
      ```
      {: pre}

    - Windows：

      ```
      calicoctl apply -f filepath/policy.yaml --config=filepath/calicoctl.cfg
      ```
      {: pre}

### 在執行 Kubernetes 1.9 版或更早版本的叢集中新增 Calico 原則
{: #1.9_create_new}

1. 定義您的 [Calico 網路原則 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://docs.projectcalico.org/v2.6/reference/calicoctl/resources/policy)，方法為建立配置 Script (`.yaml`)。這些配置檔包含選取器，其說明這些原則適用的 Pod、名稱空間或主機。請參閱這些 [Calico 原則範例 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://docs.projectcalico.org/v2.6/getting-started/kubernetes/tutorials/advanced-policy)，以協助您建立自己的原則。**附註**：Kubernetes 1.9 版或更早版本的叢集必須使用 Calico 第 2 版原則語法。


2. 將原則套用至叢集。
    - Linux 及 OS X：

      ```
      calicoctl apply -f policy.yaml
      ```
      {: pre}

    - Windows：

      ```
      calicoctl apply -f filepath/policy.yaml --config=filepath/calicoctl.cfg
      ```
      {: pre}

<br />


## 封鎖 LoadBalancer 或 NodePort 服務的入埠資料流量
{: #block_ingress}

[依預設](#default_policy)，Kubernetes NodePort 及 LoadBalancer 服務的設計是要讓您的應用程式能夠在所有公用和專用叢集介面上使用。不過，您可以根據資料流量來源或目的地，封鎖對您服務的送入資料流量。
{:shortdesc}

Kubernetes LoadBalancer 服務也是 NodePort 服務。LoadBalancer 服務可讓您的應用程式透過 LoadBalancer IP 位址及埠提供使用，並讓您的應用程式透過服務的 NodePort 提供使用。叢集內每個節點的每個 IP 位址（公開和專用）上都可以存取 NodePort。

叢集管理者可以使用 Calico `preDNAT` 網路原則來封鎖：

  - 對 NodePort 服務的資料流量。容許 LoadBalancer 服務的資料流量。
  - 根據來源位址或 CIDR 的資料流量。

Calico `preDNAT` 網路原則的一些常見用途：

  - 封鎖專用 LoadBalancer 服務之公用 NodePort 的資料流量。
  - 封鎖執行[邊緣工作者節點](cs_edge.html#edge)之叢集上公用 NodePort 的資料流量。封鎖 NodePort 可確保邊緣工作者節點是處理送入資料流量的唯一工作者節點。

預設的 Kubernetes 及 Calico 原則很難套用來保護 Kubernetes NodePort 和 LoadBalancer 服務，這是針對這些服務產生之 DNAT iptables 規則的緣故。

Calico `preDNAT` 網路原則可以協助您，因為它們會根據 Calico 網路原則資源產生 iptables 規則。Kubernetes 1.10 版或更新版本的叢集會使用[網路原則與 `calicoctl.cfg` 第 3 版語法搭配 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/networkpolicy)。Kubernetes 1.9 版或更早版本的叢集會使用[原則與 `calicoctl.cfg` 第 2 版語法搭配 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.projectcalico.org/v2.6/reference/calicoctl/resources/policy)。

1. 針對 Kubernetes 服務的 Ingress（入埠資料流量）存取，定義 Calico `preDNAT` 網路原則。

    * Kubernetes 1.10 版或更新版本的叢集必須使用 Calico 第 3 版原則語法。

        封鎖所有 NodePort 的資源範例：

        ```
        apiVersion: projectcalico.org/v3
        kind: GlobalNetworkPolicy
        metadata:
          name: deny-kube-node-port-services
        spec:
          applyOnForward: true
          ingress:
          - action: Deny
            destination:
              ports:
              - 30000:32767
            protocol: TCP
            source: {}
          - action: Deny
            destination:
              ports:
              - 30000:32767
            protocol: UDP
            source: {}
          preDNAT: true
          selector: ibm.role in { 'worker_public', 'master_public' }
          types:
          - Ingress
        ```
        {: codeblock}

    * Kubernetes 1.9 版或更早版本的叢集必須使用 Calico 第 2 版原則語法。

        封鎖所有 NodePort 的資源範例：

        ```
        apiVersion: v1
        kind: policy
        metadata:
          name: deny-kube-node-port-services
        spec:
          preDNAT: true
          selector: ibm.role in { 'worker_public', 'master_public' }
          ingress:
          - action: deny
            protocol: tcp
            destination:
              ports:
              - 30000:32767
          - action: deny
            protocol: udp
            destination:
              ports:
              - 30000:32767
        ```
        {: codeblock}

2. 套用 Calico preDNAT 網路原則。大約需要 1 分鐘，才能在整個叢集中套用原則變更。

  ```
  calicoctl apply -f deny-kube-node-port-services.yaml
  ```
  {: pre}
