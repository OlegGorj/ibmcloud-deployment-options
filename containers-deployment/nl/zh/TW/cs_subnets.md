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




# 配置叢集的子網路
{: #subnets}

藉由將子網路新增至 {{site.data.keyword.containerlong}} 中的 Kubernetes 叢集，來變更可用的可攜式公用或專用 IP 位址的儲存區。
{:shortdesc}

在 {{site.data.keyword.containershort_notm}} 中，您可以將網路子網路新增至叢集，為 Kubernetes 服務新增穩定的可攜式 IP 位址。在此情況下，子網路不會與網路遮罩搭配使用，以在一個以上的叢集中建立連線功能。而是使用子網路，從可用來存取服務的叢集中提供該服務的永久固定 IP 位址。

<dl>
  <dt>建立叢集依預設包含建立子網路</dt>
  <dd>當您建立標準叢集時，{{site.data.keyword.containershort_notm}} 會自動佈建下列子網路：
    <ul><li>主要公用子網路，可判定在建立叢集期間工作者節點的公用 IP 位址</li>
    <li>主要專用子網路，可判定在建立叢集期間工作者節點的專用 IP 位址</li>
    <li>可攜式公用子網路，可為 Ingress 及負載平衡器網路服務提供 5 個公用 IP 位址</li>
    <li>可攜式專用子網路，可為 Ingress 及負載平衡器網路服務提供 5 個專用 IP 位址</li></ul>
      可攜式公用及專用 IP 位址是靜態的，而且不會在移除工作者節點時變更。針對每一個子網路，將一個可攜式公用 IP 位址及一個可攜式專用 IP 位址用於預設 [Ingress 應用程式負載平衡器](cs_ingress.html)。您可以使用 Ingress 應用程式負載平衡器，公開叢集中的多個應用程式。藉由[建立負載平衡器服務](cs_loadbalancer.html)，即可使用其餘四個可攜式公用 IP 位址及四個可攜式專用 IP 位址，將單一應用程式公開至公用或專用網路。</dd>
  <dt>[訂購及管理您自己的現有子網路](#custom)</dt>
  <dd>您可以在 IBM Cloud 基礎架構 (SoftLayer) 帳戶中訂購及管理現有的可攜式子網路，而不是使用自動佈建的子網路。使用此選項，可在移除及建立叢集時保留穩定的靜態 IP 位址，或訂購更大的 IP 位址區塊。請先使用 `cluster-create --no-subnet` 指令來建立無子網路的叢集，然後使用 `cluster-subnet-add` 指令將子網路新增至叢集。</dd>
</dl>

**附註：**可攜式公用 IP 位址為按月收費。如果您在佈建叢集之後移除可攜式公用 IP 位址，則仍須支付一個月的費用，即使您只是短時間使用也是一樣。

## 要求叢集的其他子網路
{: #request}

您可以藉由將子網路指派給叢集，來為叢集新增穩定的可攜式公用或專用 IP 位址。
{:shortdesc}

**附註：**當您讓叢集可以使用子網路時，會使用這個子網路的 IP 位址來進行叢集網路連線。若要避免 IP 位址衝突，請確定您使用的子網路只有一個叢集。請不要同時將子網路用於多個叢集或 {{site.data.keyword.containershort_notm}} 以外的其他用途。

開始之前，請先將 [CLI 的目標](cs_cli_install.html#cs_cli_configure)設為您的叢集。

若要在 IBM Cloud 基礎架構 (SoftLayer) 帳戶中建立子網路，並將它設為可供指定的叢集使用，請執行下列動作：

1. 佈建新的子網路。

    ```
        bx cs cluster-subnet-create <cluster_name_or_id> <subnet_size> <VLAN_ID>
    ```
    {: pre}

    <table>
    <caption>瞭解此指令的元件</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解此指令的元件</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-subnet-create</code></td>
    <td>佈建叢集子網路的指令。</td>
    </tr>
    <tr>
    <td><code><em>&lt;cluster_name_or_id&gt;</em></code></td>
    <td>將 <code>&lt;cluster_name_or_id&gt;</code> 取代為叢集的名稱或 ID。</td>
    </tr>
    <tr>
    <td><code><em>&lt;subnet_size&gt;</em></code></td>
    <td>將 <code>&lt;subnet_size&gt;</code> 取代為您要從可攜式子網路新增的 IP 位址數目。接受值為 8、16、32 或 64。<p>**附註：**當您新增子網路的可攜式 IP 位址時，會使用三個 IP 位址來建立叢集內部網路。您無法將這三個 IP 位址用於應用程式負載平衡器，或是用它們來建立負載平衡器服務。例如，如果您要求八個可攜式公用 IP 位址，則可以使用其中的五個將您的應用程式公開給大眾使用。</p> </td>
    </tr>
    <tr>
    <td><code><em>&lt;VLAN_ID&gt;</em></code></td>
    <td>將 <code>&lt;VLAN_ID&gt;</code> 取代為您要在其上配置可攜式公用或專用 IP 位址之公用或專用 VLAN 的 ID。您必須選取現有工作者節點所連接的公用或專用 VLAN。若要檢閱工作者節點的公用或專用 VLAN，請執行 <code>bx cs worker-get &lt;worker_id&gt;</code> 指令。</td>
    </tr>
    </tbody></table>

2.  驗證已順利建立子網路並將其新增至叢集。子網路 CIDR 列在 **VLAN** 區段中。

    ```
        bx cs cluster-get --showResources <cluster_name_or_ID>
    ```
    {: pre}

3. 選用項目：[在相同 VLAN 上的子網路之間啟用遞送](#vlan-spanning)。

<br />


## 在 Kubernetes 叢集中新增或重複使用自訂及現有子網路
{: #custom}

您可以將現有可攜式公用或專用子網路新增至 Kubernetes 叢集，或重複使用來自已刪除叢集的子網路。
{:shortdesc}

開始之前，
- [將 CLI 的目標設為](cs_cli_install.html#cs_cli_configure)您的叢集。
- 若要重複使用來自您不再需要之叢集的子網路，請刪除不需要的叢集。這些子網路會在 24 小時內刪除。

   ```
   bx cs cluster-rm <cluster_name_or_ID
   ```
   {: pre}

若要使用 IBM Cloud 基礎架構 (SoftLayer) 組合中的現有子網路來搭配自訂防火牆規則或可用的 IP 位址，請執行下列動作：

1.  識別要使用的子網路。請記下子網路的 ID 及 VLAN ID。在此範例中，子網路 ID 是 `1602829`，而 VLAN ID 是 `2234945`。

    ```
        bx cs subnets
    ```
    {: pre}

    ```
        Getting subnet list...
    OK
    ID        Network             Gateway          VLAN ID   Type      Bound Cluster
    1550165   10.xxx.xx.xxx/26    10.xxx.xx.xxx    2234947   private
    1602829   169.xx.xxx.xxx/28   169.xx.xxx.xxx   2234945   public

    

    ```
    {: screen}

2.  確認 VLAN 所在的位置。在此範例中，位置是 dal10。

    ```
        bx cs vlans dal10
    ```
    {: pre}

    ```
        Getting VLAN list...
    OK
    ID        Name   Number   Type      Router         Supports Virtual Workers
    2234947          1813     private   bcr01a.dal10   true
    2234945          1618     public    fcr01a.dal10   true
    ```
    {: screen}

3.  使用您所識別的位置及 VLAN ID 來建立叢集。若要重複使用現有的子網路，請包含 `--no-subnet` 旗標，以防止自動建立新的可攜式公用 IP 子網路及新的可攜式專用 IP 子網路。

    ```
        bx cs cluster-create --location dal10 --machine-type u2c.2x4 --no-subnet --public-vlan 2234945 --private-vlan 2234947 --workers 3 --name my_cluster
    ```
    {: pre}

4.  驗證已要求建立叢集。

    ```
        bx cs clusters
    ```
    {: pre}

    **附註：**訂購工作者節點機器，並在您的帳戶中設定及佈建叢集，最多可能需要 15 分鐘。

    叢集佈建完成之後，叢集的狀態會變更為 **deployed**。

    ```
    Name         ID                                   State      Created          Workers   Location   Version
    mycluster    aaf97a8843a29941b49a598f516da72101   deployed   20170201162433   3         dal10      1.9.7
    ```
    {: screen}

5.  檢查工作者節點的狀態。

    ```
        bx cs workers <cluster>
    ```
    {: pre}

    工作者節點就緒後，狀態會變更為 **Normal**，而且狀態為 **Ready**。當節點狀態為 **Ready** 時，您就可以存取叢集。

    ```
    ID                                                  Public IP        Private IP     Machine Type   State      Status   Location   Version
    prod-dal10-pa8dfcc5223804439c87489886dbbc9c07-w1    169.xx.xxx.xxx   10.xxx.xx.xxx  free           normal     Ready    dal10      1.9.7
    ```
    {: screen}

6.  指定子網路 ID，以將子網路新增至叢集。當您讓子網路可供叢集使用時，系統會為您建立 Kubernetes ConfigMap，其中包括您可以使用的所有可用的可攜式公用 IP 位址。如果您的叢集還沒有應用程式負載平衡器，則會自動使用一個可攜式公用 IP 位址及一個可攜式專用 IP 位址，來建立公用及專用應用程式負載平衡器。所有其他可攜式公用或專用 IP 位址，則可用來為您的應用程式建立負載平衡器服務。

    ```
        bx cs cluster-subnet-add mycluster 807861
    ```
    {: pre}

7. 選用項目：[在相同 VLAN 上的子網路之間啟用遞送](#vlan-spanning)。

<br />


## 將使用者管理的子網路及 IP 位址新增至 Kubernetes 叢集
{: #user_managed}

從您要 {{site.data.keyword.containershort_notm}} 存取的內部部署網路，提供一個子網路。然後，您可以將來自該子網路的專用 IP 位址，新增到 Kubernetes 叢集中的負載平衡器服務。
{:shortdesc}

需求：
- 使用者管理的子網路只能新增至專用 VLAN。
- 子網路字首長度限制為 /24 到 /30。例如，`169.xx.xxx.xxx/24` 指定 253 個可用的專用 IP 位址，而 `169.xx.xxx.xxx/30` 指定 1 個可用的專用 IP 位址。
- 子網路中的第一個 IP 位址必須用來作為子網路的閘道。

開始之前：
- 配置進出外部子網路的網路資料流量遞送。
- 確認您在內部部署資料中心網路閘道與專用網路 Virtual Router Appliance 或您叢集中執行的 strongSwan VPN 服務之間，具有 VPN 連線功能。如需相關資訊，請參閱[設定 VPN 連線功能](cs_vpn.html)。

若要從內部部署網路新增子網路，請執行下列動作：

1. 檢視叢集「專用 VLAN」的 ID。找出 **VLAN** 區段。在 **User-managed** 欄位中，將 VLAN ID 識別為 _false_。

    ```
        bx cs cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
        VLANs
    VLAN ID   Subnet CIDR       Public   User-managed
    2234947   10.xxx.xx.xxx/29  false    false
    2234945   169.xx.xxx.xxx/29 true     false
    ```
    {: screen}

2. 將外部子網路新增至您的專用 VLAN。可攜式專用 IP 位址會新增至叢集的 ConfigMap。

    ```
        bx cs cluster-user-subnet-add <cluster_name> <subnet_CIDR> <VLAN_ID>
    ```
    {: pre}

    範例：

    ```
        bx cs cluster-user-subnet-add mycluster 10.xxx.xx.xxx/24 2234947
    ```
    {: pre}

3. 驗證已新增使用者提供的子網路。**User-managed** 欄位為 _true_。

    ```
        bx cs cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
        VLANs
    VLAN ID   Subnet CIDR       Public   User-managed
    2234947   10.xxx.xx.xxx/29  false    false
    2234945   169.xx.xxx.xxx/29 true   false
    2234947   10.xxx.xx.xxx/24  false    true
    ```
    {: screen}

4. 選用項目：[在相同 VLAN 上的子網路之間啟用遞送](#vlan-spanning)。

5. 新增專用負載平衡器服務或專用 Ingress 應用程式負載平衡器，以透過專用網路存取您的應用程式。若要使用您所新增之子網路中的專用 IP 位址，您必須指定 IP 位址。否則，會從 IBM Cloud 基礎架構 (SoftLayer) 子網路或專用 VLAN 上使用者提供的子網路中，隨機選擇一個 IP 位址。如需相關資訊，請參閱[使用 LoadBalancer 服務來啟用應用程式的公用或專用存取](cs_loadbalancer.html#config)或[啟用專用應用程式負載平衡器](cs_ingress.html#private_ingress)。

<br />


## 管理 IP 位址及子網路
{: #manage}

請檢閱下列選項，以列出可用的公用 IP 位址、釋放已使用的 IP 位址，以及在相同 VLAN 上的多個子網路之間進行遞送。
{:shortdesc}

### 檢視可用的可攜式公用 IP 位址
{: #review_ip}

若要列出您叢集中的所有 IP 位址，包括已用及可用的 IP 位址，您可以執行：

  ```
  kubectl get cm ibm-cloud-provider-vlan-ip-config -n kube-system -o yaml
  ```
  {: pre}

若只要列出叢集中可用的公用 IP 位址，您可以使用下列步驟：

開始之前，[請為您要使用的叢集設定環境定義。](cs_cli_install.html#cs_cli_configure)

1.  建立名為 `myservice.yaml` 的 Kubernetes 服務配置檔，並且使用虛擬負載平衡器 IP 位址來定義類型為 `LoadBalancer` 的服務。下列範例使用 IP 位址 1.1.1.1 作為負載平衡器 IP 位址。

    ```
    apiVersion: v1
    kind: Service
    metadata:
      labels:
        run: myservice
      name: myservice
      namespace: default
    spec:
      ports:
      - port: 80
        protocol: TCP
        targetPort: 80
      selector:
        run: myservice
      sessionAffinity: None
      type: LoadBalancer
      loadBalancerIP: 1.1.1.1
    ```
    {: codeblock}

2.  在叢集中建立服務。

    ```
        kubectl apply -f myservice.yaml
    ```
    {: pre}

3.  檢查服務。

    ```
        kubectl describe service myservice
    ```
    {: pre}

    **附註：**建立此服務失敗，因為 Kubernetes 主節點在 Kubernetes ConfigMap 中找不到指定的負載平衡器 IP 位址。當您執行此指令時，可以看到錯誤訊息以及叢集可用的公用 IP 位址清單。

    ```
        Error on cloud load balancer a8bfa26552e8511e7bee4324285f6a4a for service default/myservice with UID 8bfa2655-2e85-11e7-bee4-324285f6a4af: Requested cloud provider IP 1.1.1.1 is not available. The following cloud provider IP addresses are available: <list_of_IP_addresses>
    ```
    {: screen}

### 釋放已使用的 IP 位址
{: #free}

您可以刪除使用可攜式 IP 位址的負載平衡器服務，以釋放已使用的可攜式 IP 位址。
{:shortdesc}

開始之前，[請為您要使用的叢集設定環境定義。](cs_cli_install.html#cs_cli_configure)

1.  列出叢集中可用的服務。

    ```
        kubectl get services
    ```
    {: pre}

2.  移除使用公用或專用 IP 位址的負載平衡器服務。

    ```
        kubectl delete service <service_name>
    ```
    {: pre}

### 在相同 VLAN 上的子網路之間啟用遞送
{: #vlan-spanning}

當您建立叢集時，結尾為 `/26` 的子網路會佈建在叢集所在的相同 VLAN 中。這個主要子網路可以存放最多 62 個工作者節點。
{:shortdesc}

大型叢集或相同 VLAN 的單一地區中的數個較小叢集，可能會超出這 62 個工作者節點的限制。達到 62 個工作者節點的限制時，會在相同 VLAN 中訂購第二個主要子網路。

若要在相同 VLAN 上的子網路之間進行遞送，您必須開啟 VLAN 跨越。如需指示，請參閱[啟用或停用 VLAN 跨越](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning)。
