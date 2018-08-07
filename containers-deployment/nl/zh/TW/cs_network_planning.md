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



# 規劃搭配 NodePort、LoadBalancer 或 Ingress 服務的網路
{: #planning}

當您在 {{site.data.keyword.containerlong}} 中建立 Kubernetes 叢集時，每個叢集都必須連接至公用 VLAN。公用 VLAN 會判定在建立叢集期間指派給工作者節點的公用 IP 位址。
{:shortdesc}

免費和標準叢集中的工作者節點公用網路介面都受到 Calico 網路原則的保護。依預設，這些原則會封鎖大部分入埠資料流量。不過，會容許 Kubernetes 運作所需的入埠資料流量，也會容許與 NodePort、LoadBalancer 及 Ingress 服務的連線。如需這些原則的相關資訊，包括如何修改它們，請參閱[網路原則](cs_network_policy.html#network_policies)。

|叢集類型|叢集之公用 VLAN 的管理員|
|------------|------------------------------------------|
|免費叢集|{{site.data.keyword.IBM_notm}}|
|標準叢集|在您的 IBM Cloud 基礎架構 (SoftLayer) 帳戶中時|
{: caption="依叢集類型的公用 VLAN 管理員" caption-side="top"}

如需工作者節點與 Pod 之間叢集內網路通訊的相關資訊，請參閱[叢集內網路](cs_secure.html#in_cluster_network)。如需將 Kubernetes 叢集中執行的應用程式安全地連接至內部部署網路，或連接至您叢集外部應用程式的相關資訊，請參閱[設定 VPN 連線功能](cs_vpn.html)。

## 容許對應用程式的公用存取
{: #public_access}

若要將應用程式設為可在網際網路上公開使用，您必須先更新配置檔，再將應用程式部署至叢集。
{:shortdesc}

*{{site.data.keyword.containershort_notm}} 中的 Kubernetes 資料平面*

![{{site.data.keyword.containerlong_notm}} Kubernetes 架構](images/networking.png)

此圖顯示 Kubernetes 在 {{site.data.keyword.containershort_notm}} 中如何攜帶使用者網路資料流量。若要讓您的應用程式可從網際網路存取，您的方法取決於您是建立免費叢集還是標準叢集而有所不同。

<dl>
<dt><a href="cs_nodeport.html#planning" target="_blank">NodePort 服務</a>（免費及標準叢集）</dt>
<dd>
 <ul>
  <li>公開每個工作者節點上的公用埠，並使用任何工作者節點的公用 IP 位址來公開存取您在叢集中的服務。</li>
  <li>Iptables 是一種 Linux Kernel 特性，可對應用程式 Pod 之間的要求進行負載平衡、提供高效能網路遞送，以及提供網路存取控制。</li>
  <li>工作者節點的公用 IP 位址不是永久性的。移除或重建工作者節點時，會將新的公用 IP 位址指派給工作者節點。</li>
  <li>NodePort 服務十分適用於測試公用存取。如果您只是短時間需要公用存取，也可以使用此選項。</li>
 </ul>
</dd>
<dt><a href="cs_loadbalancer.html#planning" target="_blank">LoadBalancer 服務</a>（僅限標準叢集）</dt>
<dd>
 <ul>
  <li>每個標準叢集中都會佈建四個可攜式公用 IP 位址及四個可攜式專用 IP 位址，您可用來建立應用程式的外部 TCP/UDP 負載平衡器。</li>
  <li>Iptables 是一種 Linux Kernel 特性，可對應用程式 Pod 之間的要求進行負載平衡、提供高效能網路遞送，以及提供網路存取控制。</li>
  <li>指派給負載平衡器的可攜式公用 IP 位址是永久性的，因此在叢集中重建工作者節點時並不會變更。</li>
  <li>您可以公開應用程式所需的任何埠來自訂負載平衡器。</li></ul>
</dd>
<dt><a href="cs_ingress.html#planning" target="_blank">Ingress</a>（僅限標準叢集）</dt>
<dd>
 <ul>
  <li>藉由建立一個外部 HTTP 或 HTTPS、TCP 或 UDP 負載平衡器，來公開叢集中的多個應用程式。負載平衡器會使用安全且唯一的公用進入點，將送入要求遞送至您的應用程式。</li>
  <li>您可以使用一個公用路徑，將叢集中的多個應用程式公開為服務。</li>
  <li>Ingress 由兩個元件組成：
   <ul>
    <li>Ingress 資源會定義如何遞送及負載平衡應用程式送入要求的規則。</li>
    <li>應用程式負載平衡器 (ALB) 會接聽送入的 HTTP 或 HTTPS、TCP 或 UDP 服務要求。它會根據您在 Ingress 資源中所定義的規則，在應用程式的 Pod 之間轉遞要求。</li>
   </ul>
  <li>使用 Ingress 以利用自訂遞送規則來實作自己的 ALB，而且您的應用程式需要 SSL 終止。</li>
 </ul>
</dd></dl>

若要選擇應用程式的最佳網路選項，您可以遵循這個決策樹狀結構。如需規劃資訊及配置指示，請按一下您選擇的網路服務選項。

<img usemap="#networking_map" border="0" class="image" src="images/networkingdt.png" width="500px" alt="此圖會引導您完成選擇應用程式的最佳網路選項。如果未顯示此圖，仍然可以在文件中找到這項資訊。" style="width:500px;" />
<map name="networking_map" id="networking_map">
<area href="/docs/containers/cs_nodeport.html" alt="Nodeport 服務" shape="circle" coords="52, 283, 45"/>
<area href="/docs/containers/cs_loadbalancer.html" alt="LoadBalancer 服務" shape="circle" coords="247, 419, 44"/>
<area href="/docs/containers/cs_ingress.html" alt="Ingress 服務" shape="circle" coords="445, 420, 45"/>
</map>
