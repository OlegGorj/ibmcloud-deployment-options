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


# 設定 VPN 連線功能
{: #vpn}

使用 VPN 連線功能，您可以在 {{site.data.keyword.containerlong}} 上將 Kubernetes 叢集中的應用程式安全地連接至內部部署網路。您也可以將叢集外部的應用程式連接至叢集內部執行的應用程式。
{:shortdesc}

若要將工作者節點及應用程式連接至內部部署資料中心，您可以配置下列其中一個選項。

- **strongSwan IPSec VPN 服務**：您可以設定 [strongSwan IPSec VPN 服務 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.strongswan.org/)，以安全地連接 Kubernetes 叢集與內部部署網路。在根據業界標準網際網路通訊協定安全 (IPsec) 通訊協定套組的網際網路上，strongsWan IPSec VPN 服務提供安全的端對端通訊通道。若要設定叢集與內部部署網路之間的安全連線，請直接在叢集的 Pod 中[配置及部署 strongSwan IPSec VPN 服務](#vpn-setup)。

- **Virtual Router Appliance (VRA) 或 Fortigate Security Appliance (FSA)**：您可以選擇設定 [VRA](/docs/infrastructure/virtual-router-appliance/about.html) 或 [FSA](/docs/infrastructure/fortigate-10g/about.html)，以配置 IPSec VPN 端點。當您有較大的叢集、要透過 VPN 存取非 Kubernetes 資源，或要透過單一 VPN 存取多個叢集時，此選項十分有用。若要配置 VRA，請參閱[使用 VRA 設定 VPN 連線功能](#vyatta)。

## 使用 strongSwan IPSec VPN 服務 Helm 圖表設定 VPN 連線功能
{: #vpn-setup}

使用 Helm 圖表，可以在 Kubernetes Pod 中配置及部署 strongSwan IPSec VPN 服務。
{:shortdesc}

因為 strongSwan 已整合在您的叢集內，所以您不需要外部閘道裝置。建立 VPN 連線功能時，會在叢集中的所有工作者節點上自動配置路徑。這些路線容許透過 VPN 通道在任何工作者節點與遠端系統上的 Pod 之間進行雙向連線。例如，下圖顯示 {{site.data.keyword.containershort_notm}} 中的應用程式如何透過 strongSwan VPN 連線與內部部署伺服器通訊：

<img src="images/cs_vpn_strongswan.png" width="700" alt="使用負載平衡器公開 {{site.data.keyword.containershort_notm}} 中的應用程式" style="width:700px; border-style: none"/>

1. 叢集中的應用程式 (`myapp`) 會接收來自 Ingress 或 LoadBalancer 服務的要求，且需要安全地連接至內部部署網路中的資料。

2. 內部部署資料中心的要求會轉遞至 IPSec strongSwan VPN Pod。目的地 IP 位址是用來判定要傳送至 IPSec strongSwan VPN pod 的網路封包。

3. 此要求會加密並透過 VPN 通道傳送至內部部署資料中心。

4. 送入的要求會通過內部部署防火牆，並遞送至解密所在的 VPN 通道端點（路由器）。

5. VPN 通道端點（路由器）會將要求轉遞至內部部署伺服器或大型主機，視步驟 2 中指定的目的地 IP 位址而定。必要資料會藉由相同的處理程序，透過 VPN 連線傳回至 `myapp`。

### 配置 strongSwan Helm 圖表
{: #vpn_configure}

開始之前：
* [在內部部署資料中心內安裝 IPsec VPN 閘道。](/docs/infrastructure/iaas-vpn/set-up-ipsec-vpn.html#setting-up-an-ipsec-connection).
* [建立標準叢集](cs_clusters.html#clusters_cli)或[將現有叢集更新為 1.7.16 版或更新版本](cs_cluster_update.html#master)。
* 叢集必須至少有一個可用的公用「負載平衡器」IP 位址。[您可以查看可用的公用 IP 位址](cs_subnets.html#manage)或[釋放已使用的 IP 位址](cs_subnets.html#free)。
* [將 Kubernetes CLI 的目標設為叢集](cs_cli_install.html#cs_cli_configure)。

如需用來設定 strongSwan 圖表之 Helm 指令的相關資訊，請參閱 <a href="https://docs.helm.sh/helm/" target="_blank">Helm 文件 <img src="../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。

若要配置 Helm 圖表，請執行下列動作：

1. [安裝叢集的 Helm，並將 {{site.data.keyword.Bluemix_notm}} 儲存庫新增至 Helm 實例](cs_integrations.html#helm)。

2. 將 strongSwan Helm 圖表的預設配置設定儲存在本端 YAML 檔案中。

    ```
        helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. 開啟 `config.yaml` 檔案，並根據您要的 VPN 配置，對預設值進行下列變更。您可以在配置檔註解中找到其他進階設定的說明。

    **重要事項**：如果您不需要變更內容，請在它前面加上 `#` 來註銷該內容。

    <table>
    <caption>瞭解這個 YAML 元件</caption>
    <col width="22%">
    <col width="78%">
    <thead>
    <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解 YAML 檔案元件</th>
    </thead>
    <tbody>
    <tr>
    <td><code>localSubnetNAT</code></td>
    <td>子網路的「網址轉換 (NAT)」提供本端與內部部署網路之間子網路衝突的暫行解決方法。您可以使用 NAT，將叢集的專用本端 IP 子網路、Pod 子網路 (172.30.0.0/16) 或 Pod 服務子網路 (172.21.0.0/16) 重新對映至不同的專用子網路。VPN 通道會看到重新對映的 IP 子網路，而非原始子網路。重新對映發生在封包透過 VPN 通道傳送之前，以及封包從 VPN 通道抵達之後。您可以透過 VPN 同時公開重新對映及非重新對映的子網路。<br><br>若要啟用 NAT，您可以新增整個子網路或個別 IP 位址。如果您新增整個子網路（格式為 <code>10.171.42.0/24=10.10.10.0/24</code>），則重新對映是 1 對 1：內部網路子網路中的所有 IP 位址都會對映到外部網路子網路，反之亦然。如果您新增個別 IP 位址（格式為 <code>10.171.42.17/32=10.10.10.2/32, 10.171.42.29/32=10.10.10.3/32</code>），則只有那些內部 IP 位址會對映到指定的外部 IP 位址。<br><br>如果您使用此選項，則透過 VPN 連線公開的本端子網路即為「內部」子網路所對映到的「外部」子網路。</td>
    </tr>
    <tr>
    <td><code>loadBalancerIP</code></td>
    <td>如果您想要針對 strongSwan VPN 服務指定可攜式公用 IP 位址，進行入埠 VPN 連線，請新增該 IP 位址。當您需要穩定的 IP 位址時（例如，當您必須指定允許哪些 IP 位址通過內部部署防火牆時），指定 IP 位址很有用。<br><br>若要檢視指派給此叢集的可攜性公用 IP 位址，請參閱[管理 IP 位址及子網路](cs_subnets.html#manage)。如果您將此設定保留空白，則會使用免費的可攜式公用 IP 位址。如果從內部部署閘道起始 VPN 連線（<code>ipsec.auto</code> 設為 <code>add</code>），則您可以使用此內容，在叢集的內部部署閘道上配置持續性公用 IP 位址。</td>
    </tr>
    <tr>
    <td><code>connectUsingLoadBalancerIP</code></td>
    <td>使用您在 <code>loadBalancerIP</code> 中所新增的負載平衡器 IP 位址，也可以建立出埠 VPN 連線。如果啟用此選項，則所有叢集工作者節點都必須位於相同的公用 VLAN 上。否則，您必須使用 <code>nodeSelector</code> 設定，以確保 VPN Pod 部署至與 <code>loadBalancerIP</code> 相同的公用 VLAN 上的工作者節點。如果將 <code>ipsec.auto</code> 設為 <code>add</code>，則會忽略此選項。<p>接受值：</p><ul><li><code>"false"</code>：不使用負載平衡器 IP 來連接 VPN。改為使用 VPN Pod 執行所在之工作者節點的 IP 位址。</li><li><code>"true"</code>：使用負載平衡器 IP 作為本端來源 IP 來建立 VPN。如果未設定 <code>loadBalancerIP</code>，則會使用指派給負載平衡器服務的外部 IP 位址。</li><li><code>"auto"</code>：將 <code>ipsec.auto</code> 設為 <code>start</code> 並設定 <code>loadBalancerIP</code> 時，請使用負載平衡器 IP 作為本端來源 IP 來建立 VPN。</li></ul></td>
    </tr>
    <tr>
    <td><code>nodeSelector</code></td>
    <td>若要限制 strongSwan VPN Pod 部署至哪些節點，請新增特定工作者節點的 IP 位址或工作者節點標籤。例如，<code>kubernetes.io/hostname: 10.xxx.xx.xxx</code> 值限制 VPN Pod 只能在該工作者節點上執行。<code>strongswan: vpn</code> 值限制 VPN Pod 在任何具有該標籤的工作者節點上執行。您可以使用任何工作者節點標籤，但建議您使用 <code>strongswan: &lt;release_name&gt;</code>，這樣不同的工作者節點才可以與這個圖表的不同部署搭配使用。<br><br>如果由叢集起始 VPN 連線（<code>ipsec.auto</code> 設為 <code>start</code>），您可以使用此內容來限制公開給內部部署閘道的 VPN 連線的來源 IP 位址。這是選用值。</td>
    </tr>
    <tr>
    <td><code>ipsec.keyexchange</code></td>
    <td>如果您的內部部署 VPN 通道端點不支援使用 <code>ikev2</code> 作為起始設定連線的通訊協定，請將此值變更為 <code>ikev1</code> 或 <code>ike</code>。</td>
    </tr>
    <tr>
    <td><code>ipsec.esp</code></td>
    <td>新增內部部署 VPN 通道端點用於連線之 ESP 加密及鑑別演算法的清單。<ul><li>如果將 <code>ipsec.keyexchange</code> 設為 <code>ikev1</code>，則必須指定此設定。</li><li>如果將 <code>ipsec.keyexchange</code> 設為 <code>ikev2</code>，則此設定為選用項目。如果您將此設定保留空白，則連線會使用預設 strongSwan 演算法 <code>aes128-sha1,3des-sha1</code>。</li></ul></td>
    </tr>
    <tr>
    <td><code>ipsec.ike</code></td>
    <td>新增內部部署 VPN 通道端點用於連線之 IKE/ISAKMP SA 加密及鑑別演算法的清單。<ul><li>如果將 <code>ipsec.keyexchange</code> 設為 <code>ikev1</code>，則必須指定此設定。</li><li>如果將 <code>ipsec.keyexchange</code> 設為 <code>ikev2</code>，則此設定為選用項目。如果您將此設定保留空白，則連線會使用預設 strongSwan 演算法 <code>aes128-sha1-modp2048,3des-sha1-modp1536</code>。</li></ul></td>
    </tr>
    <tr>
    <td><code>ipsec.auto</code></td>
    <td>如果您希望由叢集起始 VPN 連線，請將此值變更為 <code>start</code>。</td>
    </tr>
    <tr>
    <td><code>local.subnet</code></td>
    <td>將此值變更為要透過 VPN 連線公開給內部部署網路使用之叢集子網路 CIDR 的清單。此清單可能包括下列子網路：<ul><li>Kubernetes Pod 子網路 CIDR：<code>172.30.0.0/16</code></li><li>Kubernetes 服務子網路 CIDR：<code>172.21.0.0/16</code></li><li>如果您的應用程式是由專用網路上的 NodePort 服務公開，則為工作者節點的專用子網路 CIDR。藉由執行 <code>bx cs worker &lt;cluster_name&gt;</code>，來擷取工作者專用 IP 位址的前三個八位元組。比方說，如果它是 <code>&lt;10.176.48.xx&gt;</code>，請注意 <code>&lt;10.176.48&gt;</code>。接下來，執行下列指令，將 <code>&lt;xxx.yyy.zz&gt;</code> 取代為您先前擷取的八位元組，來取得工作者專用子網路 CIDR：<code>bx cs subnets | grep &lt;xxx.yyy.zzz&gt;</code>。</li><li>如果您的應用程式是由專用網路上的 LoadBalancer 服務公開，則為叢集的專用或使用者管理的子網路 CIDR。若要尋找這些值，請執行 <code>bx cs cluster-get &lt;cluster_name&gt; --showResources</code>。在 **VLANS** 區段中，請尋找 **Public** 值為 <code>false</code> 的 CIDR。</li></ul>**附註**：如果將 <code>ipsec.keyexchange</code> 設為 <code>ikev1</code>，則您只能指定一個子網路。</td>
    </tr>
    <tr>
    <td><code>local.id</code></td>
    <td>將此值變更為 VPN 通道端點用於連線的本端 Kubernetes 叢集端的字串 ID。</td>
    </tr>
    <tr>
    <td><code>remote.gateway</code></td>
    <td>將此值變更為內部部署 VPN 閘道的公用 IP 位址。當 <code>ipsec.auto</code> 設為 <code>start</code> 時，這是必要值。</td>
    </tr>
    <td><code>remote.subnet</code></td>
    <td>將此值變更為容許 Kubernetes 叢集存取的內部部署專用子網路 CIDR 清單。**附註**：如果將 <code>ipsec.keyexchange</code> 設為 <code>ikev1</code>，則您只能指定一個子網路。</td>
    </tr>
    <tr>
    <td><code>remote.id</code></td>
    <td>將此值變更為 VPN 通道端點用於連線的遠端內部部署端的字串 ID。</td>
    </tr>
    <tr>
    <td><code>remote.privateIPtoPing</code></td>
    <td>新增遠端子網路中 Helm 測試驗證程式要用於 VPN ping 連線功能測試的專用 IP 位址。這是選用值。</td>
    </tr>
    <tr>
    <td><code>preshared.secret</code></td>
    <td>將此值變更為內部部署 VPN 通道端點閘道用於連線的預先共用密碼。此值儲存在 <code>ipsec.secrets</code> 中。</td>
    </tr>
    </tbody></table>

4. 儲存已更新的 `config.yaml` 檔案。

5. 使用已更新的 `config.yaml` 檔案，將 Helm 圖表安裝至叢集。已更新的內容會儲存在圖表的 ConfigMap 中。

    **附註**：如果單一叢集中有多個 VPN 部署，您可以選擇比 `vpn` 更具描述性的版次名稱，以避免命名衝突並區分您的部署。為了避免截斷版次名稱，請將版次名稱限制為 35 個字元以內。

    ```
    helm install -f config.yaml --name=vpn ibm/strongswan
    ```
    {: pre}

6. 檢查圖表部署狀態。圖表就緒時，輸出頂端附近的 **STATUS** 欄位值為 `DEPLOYED`。

    ```
        helm status vpn
    ```
    {: pre}

7. 部署圖表之後，請驗證已使用 `config.yaml` 檔案中的已更新設定。

    ```
        helm get values vpn
    ```
    {: pre}


### 測試及驗證 VPN 連線功能
{: #vpn_test}

部署 Helm 圖表之後，請測試 VPN 連線功能。
{:shortdesc}

1. 如果內部部署閘道上的 VPN 不在作用中，請啟動 VPN。

2. 設定 `STRONGSWAN_POD` 環境變數。

    ```
    export STRONGSWAN_POD=$(kubectl get pod -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
    ```
    {: pre}

3. 檢查 VPN 的狀態。`ESTABLISHED` 狀態表示 VPN 連線成功。

    ```
    kubectl exec $STRONGSWAN_POD -- ipsec status
    ```
    {: pre}

    輸出範例：

    ```
    Security Associations (1 up, 0 connecting):
    k8s-conn[1]: ESTABLISHED 17 minutes ago, 172.30.xxx.xxx[ibm-cloud]...192.xxx.xxx.xxx[on-premises]
    k8s-conn{2}: INSTALLED, TUNNEL, reqid 12, ESP in UDP SPIs: c78cb6b1_i c5d0d1c3_o
    k8s-conn{2}: 172.21.0.0/16 172.30.0.0/16 === 10.91.152.xxx/26
    ```
    {: screen}

    **附註**：

    <ul>
    <li>當您嘗試使用 strongSwan Helm 圖表建立 VPN 連線功能時，有可能第一次的 VPN 狀態不是 `ESTABLISHED`。您可能需要檢查內部部署 VPN 端點設定，並數次變更配置檔，連線才會成功：<ol><li>執行 `helm delete --purge <release_name>`</li><li>修正配置檔中不正確的值。</li><li>執行 `helm install -f config.yaml --name=<release_name> ibm/strongswan`</li></ol>您也可以在下一步執行其他檢查。</li>
    <li>如果 VPN Pod 處於 `ERROR` 狀態，或持續當機並重新啟動，則可能是由於圖表的 ConfigMap 中 `ipsec.conf` 設定的參數驗證所造成。<ol><li>請執行 `kubectl logs -n $STRONGSWAN_POD`，檢查 strongSwan Pod 日誌中是否有任何驗證錯誤。</li><li>如果有驗證錯誤，請執行 `helm delete --purge <release_name>`<li>修正配置檔中不正確的值。</li><li>執行 `helm install -f config.yaml --name=<release_name> ibm/strongswan`</li></ol>如果叢集具有大量工作者節點，您也可以使用 `helm upgrade` 更快速地套用您的變更，而不是執行 `helm delete` 及 `helm install`。</li>
    </ul>

4. 您可以藉由執行內含在 strongSwan 圖表定義中的五個 Helm 測試，來進一步測試 VPN 連線功能。

    ```
        helm test vpn
    ```
    {: pre}

    * 如果所有的測試都通過，您的 strongSwan VPN 連線便已順利設定。

    * 如果有任何測試失敗，請繼續下一步。

5. 查看測試 Pod 的日誌，以檢視失敗測試的輸出。

    ```
    kubectl logs <test_program>
    ```
    {: pre}

    **附註**：部分測試的需求是 VPN 配置中的選用設定。如果某些測試失敗，則根據您是否指定這些選用設定，失敗也許是可接受的。如需每一個測試及其失敗原因的相關資訊，請參閱下表。

    {: #vpn_tests_table}
    <table>
    <caption>瞭解 Helm VPN 連線功能測試</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="構想圖示"/>瞭解 Helm VPN 連線功能測試</th>
    </thead>
    <tbody>
    <tr>
    <td><code>vpn-strongswan-check-config</code></td>
    <td>驗證從 <code>config.yaml</code> 檔案產生的 <code>ipsec.conf</code> 檔案的語法。此測試可能由於 <code>config.yaml</code> 檔案中的值不正確而失敗。</td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-check-state</code></td>
    <td>確認 VPN 連線的狀態為 <code>ESTABLISHED</code>。此測試可能因下列原因而失敗：<ul><li><code>config.yaml</code> 檔案中的值與內部部署 VPN 端點設定之間的差異。</li><li>如果叢集處於「接聽」模式（<code>ipsec.auto</code> 設為 <code>add</code>），則不會在內部部署端建立連線。</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-gw</code></td>
    <td>對您在 <code>config.yaml</code> 檔案中配置的 <code>remote.gateway</code> 公用 IP 位址進行連線測試。此測試可能因下列原因而失敗：<ul><li>您未指定內部部署 VPN 閘道 IP 位址。如果 <code>ipsec.auto</code> 是設為 <code>start</code>，則需要 <code>remote.gateway</code> IP 位址。</li><li>VPN 連線沒有 <code>ESTABLISHED</code> 狀態。如需相關資訊，請參閱 <code>vpn-strongswan-check-state</code>。</li><li>VPN 連線功能是 <code>ESTABLISHED</code>，但 ICMP 封包遭到防火牆封鎖。</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-ip-1</code></td>
    <td>從叢集中的 VPN Pod，對內部部署 VPN 閘道的 <code>remote.privateIPtoPing</code> 專用 IP 位址進行連線測試。此測試可能因下列原因而失敗：<ul><li>您未指定 <code>remote.privateIPtoPing</code> IP 位址。如果您是故意不指定 IP 位址，則此失敗是可接受的。</li><li>您未在 <code>local.subnet</code> 清單中指定叢集 Pod 子網路 CIDR <code>172.30.0.0/16</code>。</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-ip-2</code></td>
    <td>從叢集中的工作者節點，對內部部署 VPN 閘道的 <code>remote.privateIPtoPing</code> 專用 IP 位址進行連線測試。此測試可能因下列原因而失敗：<ul><li>您未指定 <code>remote.privateIPtoPing</code> IP 位址。如果您是故意不指定 IP 位址，則此失敗是可接受的。</li><li>您未在 <code>local.subnet</code> 清單中指定叢集工作者節點專用子網路 CIDR。</li></ul></td>
    </tr>
    </tbody></table>

6. 刪除現行 Helm 圖表。

    ```
        helm delete --purge vpn
    ```
    {: pre}

7. 開啟 `config.yaml` 檔案，並修正不正確的值。

8. 儲存已更新的 `config.yaml` 檔案。

9. 使用已更新的 `config.yaml` 檔案，將 Helm 圖表安裝至叢集。已更新的內容會儲存在圖表的 ConfigMap 中。

    ```
    helm install -f config.yaml --name=<release_name> ibm/strongswan
    ```
    {: pre}

10. 檢查圖表部署狀態。圖表就緒時，輸出頂端附近的 **STATUS** 欄位值為 `DEPLOYED`。

    ```
        helm status vpn
    ```
    {: pre}

11. 部署圖表之後，請驗證已使用 `config.yaml` 檔案中的已更新設定。

    ```
        helm get values vpn
    ```
    {: pre}

12. 清除現行測試 Pod。

    ```
    kubectl get pods -a -l app=strongswan-test
    ```
    {: pre}

    ```
    kubectl delete pods -l app=strongswan-test
    ```
    {: pre}

13. 重新執行測試。

    ```
        helm test vpn
    ```
    {: pre}

<br />


## 升級 strongSwan Helm 圖表
{: #vpn_upgrade}

藉由升級 strongSwan Helm 圖表，確定它保持最新。
{:shortdesc}

若要將 strongSwan Helm 圖表升級至最新版本，請執行下列動作：

  ```
  helm upgrade -f config.yaml <release_name> ibm/strongswan
  ```
  {: pre}

**重要事項**：strongSwan 2.0.0 Helm 圖表不會使用 Calico 第 3 版或 Kubernetes 1.10。[將叢集更新至 1.10](cs_versions.html#cs_v110) 之前，請將 strongSwan 更新至 2.1.0 Helm 圖表，其與 Calico 2.6 及 Kubernetes 1.7、1.8 及 1.9 舊版相容。


### 從 1.0.0 版升級
{: #vpn_upgrade_1.0.0}

由於在 1.0.0 版 Helm 圖表中使用的部分設定，您無法使用 `helm upgrade` 從 1.0.0 更新至最新版本。
{:shortdesc}

若要從 1.0.0 版升級，您必須刪除 1.0.0 圖表，並安裝最新版本：

1. 刪除 1.0.0 Helm 圖表。

    ```
      helm delete --purge <release_name>
  ```
    {: pre}

2. 將 strongSwan Helm 圖表的最新版本的預設配置設定儲存在本端 YAML 檔案中。

    ```
        helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. 更新配置檔，並儲存含有您所做變更的檔案。

4. 使用已更新的 `config.yaml` 檔案，將 Helm 圖表安裝至叢集。

    ```
    helm install -f config.yaml --name=<release_name> ibm/strongswan
    ```
    {: pre}

此外，1.0.0 中某些寫在程式中的 `ipsec.conf` 逾時設定，在未來版本中公開為可配置的內容。其中一些可配置的 `ipsec.conf` 逾時設定的名稱及預設值，也變更為更符合 strongSwan 標準。如果您要從 1.0.0 升級 Helm 圖表，且要保留 1.0.0 版的逾時設定預設值，請使用舊的預設值，將新設定新增至圖表配置檔中。



  <table>
  <caption>1.0.0 版與最新版本之間的 ipsec.conf 設定差異</caption>
  <thead>
  <th>1.0.0 設定名稱</th>
  <th>1.0.0 預設值</th>
  <th>最新版本設定名稱</th>
  <th>最新版本預設值</th>
  </thead>
  <tbody>
  <tr>
  <td><code>ikelifetime</code></td>
  <td>60m</td>
  <td><code>ikelifetime</code></td>
  <td>3h</td>
  </tr>
  <tr>
  <td><code>keylife</code></td>
  <td>20m</td>
  <td><code>lifetime</code></td>
  <td>1h</td>
  </tr>
  <tr>
  <td><code>rekeymargin</code></td>
  <td>3m</td>
  <td><code>margintime</code></td>
  <td>9m</td>
  </tr>
  </tbody></table>


## 停用 strongSwan IPSec VPN 服務
{: vpn_disable}

您可以藉由刪除 Helm 圖表來停用 VPN 連線。
{:shortdesc}

  ```
  helm delete --purge <release_name>
  ```
  {: pre}

<br />


## 利用 Virtual Router Appliance 設定 VPN 連線功能
{: #vyatta}

[Virtual Router Appliance (VRA)](/docs/infrastructure/virtual-router-appliance/about.html) 提供適用於 x86 裸機伺服器的最新 Vyatta 5600 作業系統。您可以使用 VRA 作為 VPN 閘道，以安全地連接至內部部署網路。
{:shortdesc}

進入或結束叢集 VLAN 的所有公用及專用網路資料流量都會透過 VRA 遞送。您可以使用 VRA 作為 VPN 端點，在 IBM Cloud 基礎架構 (SoftLayer) 及內部部署資源中的伺服器之間建立已加密的 IPSec 通道。例如，下圖顯示 {{site.data.keyword.containershort_notm}} 中僅限專用工作者節點上的應用程式如何透過 VRA VPN 連線與內部部署伺服器通訊：

<img src="images/cs_vpn_vyatta.png" width="725" alt="使用負載平衡器公開 {{site.data.keyword.containershort_notm}} 中的應用程式" style="width:725px; border-style: none"/>

1. 叢集中的應用程式 (`myapp2`) 會接收來自 Ingress 或 LoadBalancer 服務的要求，且需要安全地連接至內部部署網路中的資料。

2. 因為 `myapp2` 位於僅限專用 VLAN 的工作者節點上，所以 VRA 在工作者節點與內部部署網路之間充當安全連線。VRA 會使用目的地 IP 位址，來判定要傳送至內部部署網路的網路封包。

3. 此要求會加密並透過 VPN 通道傳送至內部部署資料中心。

4. 送入的要求會通過內部部署防火牆，並遞送至解密所在的 VPN 通道端點（路由器）。

5. VPN 通道端點（路由器）會將要求轉遞至內部部署伺服器或大型主機，視步驟 2 中指定的目的地 IP 位址而定。必要資料會藉由相同的處理程序，透過 VPN 連線傳回至 `myapp2`。

若要設定 Virtual Router Appliance，請執行下列動作：

1. [訂購 VRA](/docs/infrastructure/virtual-router-appliance/getting-started.html)。

2. [在 VRA 上配置專用 VLAN](/docs/infrastructure/virtual-router-appliance/manage-vlans.html)。

3. 若要使用 VRA 啟用 VPN 連線，請[在 VRA 上配置 VRRP](/docs/infrastructure/virtual-router-appliance/vrrp.html#high-availability-vpn-with-vrrp)。
