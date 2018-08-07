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



# 為何要用 {{site.data.keyword.containerlong_notm}}
{: #cs_ov}

{{site.data.keyword.containerlong}} 藉由結合 Docker 和 Kubernetes 技術、直覺式使用者體驗以及內建安全和隔離來提供功能強大的工具，以在運算主機的叢集中自動部署、操作、擴充及監視容器化應用程式。
{:shortdesc}


## 使用服務的優點
{: #benefits}

叢集部署於提供原生 Kubernetes 及 {{site.data.keyword.IBM_notm}} 特有功能的運算主機上。
{:shortdesc}

|優點|說明|
|-------|-----------|
|具有運算、網路及儲存空間基礎架構隔離的單一承租戶 Kubernetes 叢集|<ul><li>建立符合組織需求的專屬自訂基礎架構。</li><li>使用 IBM Cloud 基礎架構 (SoftLayer) 所提供的資源，來佈建專用且受保護的 Kubernetes 主節點、工作者節點、虛擬網路及儲存空間。</li><li>完全受管理的 Kubernetes 主節點，它持續由 {{site.data.keyword.IBM_notm}} 監視及更新，以保持叢集可供使用。</li><li>可以選擇使用「授信運算」將工作者節點佈建為裸機伺服器。</li><li>儲存持續性資料、在 Kubernetes Pod 之間共用資料，以及在需要時使用整合且安全的磁區服務來還原資料。</li><li>獲得所有原生 Kubernetes API 的完整支援的好處。</li></ul>|
|「漏洞警告器」的映像檔安全規範|<ul><li>設定您自己的受保護 Docker 專用映像檔登錄，而組織中的所有使用者都會在這裡儲存及共用映像檔。</li><li>獲得自動掃描專用 {{site.data.keyword.Bluemix_notm}} 登錄中映像檔的好處。</li><li>檢閱映像檔中所使用的作業系統特有的建議，來修正潛在漏洞。</li></ul>|
|叢集性能的持續監視|<ul><li>使用叢集儀表板，快速查看及管理叢集、工作者節點及容器部署的性能。</li><li>使用 {{site.data.keyword.monitoringlong}} 來尋找詳細的耗用度量值，並快速擴充叢集以符合工作負載。</li><li>使用 {{site.data.keyword.loganalysislong}} 來檢閱記載資訊，以查看詳細的叢集活動。</li></ul>|
|安全地將應用程式公開給大眾使用|<ul><li>選擇公用 IP 位址、{{site.data.keyword.IBM_notm}} 所提供的路徑，或您自己的自訂網域，以從網際網路存取叢集中的服務。</li></ul>|
|{{site.data.keyword.Bluemix_notm}} 服務整合|<ul><li>透過整合 {{site.data.keyword.Bluemix_notm}} 服務（例如 Watson API、Blockchain、資料服務或 Internet of Things），為應用程式增加額外的功能。</li></ul>|
{: caption="{{site.data.keyword.containerlong_notm}} 的優點" caption-side="top"}



<br />


## 供應項目及其組合的比較
{: #differentiation}

您可以在 {{site.data.keyword.Bluemix_notm}} Public 或 Dedicated、在 {{site.data.keyword.Bluemix_notm}} Private，或在混合式設定中執行 {{site.data.keyword.containershort_notm}}。
{:shortdesc}


<table>
<caption>{{site.data.keyword.containershort_notm}} 設定之間的差異</caption>
<col width="22%">
<col width="78%">
 <thead>
 <th>{{site.data.keyword.containershort_notm}} 設定</th>
 <th>說明</th>
 </thead>
 <tbody>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Public
 </td>
 <td>利用[共用或專用硬體上或祼機機器上](cs_clusters.html#shared_dedicated_node)的 {{site.data.keyword.Bluemix_notm}} Public，您可以使用 {{site.data.keyword.containershort_notm}} 來管理雲端上叢集中的應用程式。{{site.data.keyword.Bluemix_notm}} Public 上的 {{site.data.keyword.containershort_notm}} 藉由結合 Docker 和 Kubernetes 技術、直覺式使用者體驗以及內建安全和隔離來提供功能強大的工具，以在運算主機的叢集中自動部署、操作、擴充及監視容器化應用程式。<br><br>如需相關資訊，請參閱 [{{site.data.keyword.containershort_notm}} 技術](cs_tech.html)。</td>
 </tr>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Dedicated
 </td>
 <td>{{site.data.keyword.Bluemix_notm}} Dedicated 在雲端上提供與 {{site.data.keyword.Bluemix_notm}} Public 相同的 {{site.data.keyword.containershort_notm}} 功能。不過，使用 {{site.data.keyword.Bluemix_notm}} Dedicated 帳戶，可用的[實體資源只供您的叢集專用](cs_clusters.html#shared_dedicated_node)，無法與其他 {{site.data.keyword.IBM_notm}} 客戶的叢集共用。當需要隔離您的叢集與您使用的其他 {{site.data.keyword.Bluemix_notm}} 服務時，您可能選擇設定 {{site.data.keyword.Bluemix_notm}} Dedicated 環境。<br><br>如需相關資訊，請參閱[開始使用 {{site.data.keyword.Bluemix_notm}} Dedicated 中的叢集](cs_dedicated.html#dedicated)。
 </td>
 </tr>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Private
 </td>
 <td>{{site.data.keyword.Bluemix_notm}} Private 是可在您自己機器本端安裝的應用程式平台。當您需要在防火牆後面的專屬控制環境中開發及管理內部部署容器化應用程式時，您可能選擇使用 {{site.data.keyword.Bluemix_notm}} Private 中的 {{site.data.keyword.containershort_notm}}。<br><br>如需相關資訊，請參閱 [{{site.data.keyword.Bluemix_notm}} Private 產品資訊 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/cloud/private)，以及[文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_1.2.0/kc_welcome_containers.html)。
 </td>
 </tr>
 <tr>
 <td>混合式設定
 </td>
 <td>混合式可結合使用 {{site.data.keyword.Bluemix_notm}} Public 或 Dedicated 中執行的服務，以及在內部部署上執行的其他服務（例如 {{site.data.keyword.Bluemix_notm}} Private 中的應用程式）。混合式設定的範例：<ul><li>使用 {{site.data.keyword.Bluemix_notm}} Public 中的 {{site.data.keyword.containershort_notm}} 佈建叢集，但將該叢集連接至內部部署資料庫。</li><li>使用 {{site.data.keyword.Bluemix_notm}} Private 中的 {{site.data.keyword.containershort_notm}} 佈建叢集，並將該應用程式部署至該叢集。不過，此應用程式可能使用 {{site.data.keyword.Bluemix_notm}} Public 中的 {{site.data.keyword.ibmwatson}} 服務，例如 {{site.data.keyword.toneanalyzershort}}。</li></ul><br>若要在 {{site.data.keyword.Bluemix_notm}} Public 或 Dedicated 執行的服務與執行內部部署的服務之間啟用通訊，您必須[設定 VPN 連線](cs_vpn.html)。
 </td>
 </tr>
 </tbody>
</table>

<br />


## 免費與標準叢集的比較
{: #cluster_types}

您可以建立一個免費叢集或任意數目的標準叢集。請試用免費叢集，以熟悉並測試一些 Kubernetes 功能，或是建立標準叢集，以使用完整的 Kubernetes 功能來部署應用程式。
{:shortdesc}

|特徵|免費叢集|標準叢集|
|---------------|-------------|-----------------|
|[叢集內網路](cs_secure.html#in_cluster_network)
|[含磁區的持續性 NFS 檔案型儲存空間](cs_storage.html#planning)| |<img src="images/confirm.svg" width="32" alt="可用的特性" style="width:32px;" />|
|[負載平衡器服務對於穩定 IP 位址的公用或專用網路應用程式存取](cs_loadbalancer.html#planning)| |<img src="images/confirm.svg" width="32" alt="可用的特性" style="width:32px;" />|
|[Ingress 服務對於穩定 IP 位址及可自訂 URL 的公用網路應用程式存取](cs_ingress.html#planning)| |<img src="images/confirm.svg" width="32" alt="可用的特性" style="width:32px;" />|
|[可攜式公用 IP 位址](cs_subnets.html#manage)| |<img src="images/confirm.svg" width="32" alt="可用的特性" style="width:32px;" />|
|[記載及監視](cs_health.html#logging)| |<img src="images/confirm.svg" width="32" alt="可用的特性" style="width:32px;" />|
|[可以選擇在實體（裸機）伺服器上佈建工作者節點](cs_clusters.html#shared_dedicated_node) | |<img src="images/confirm.svg" width="32" alt="可用的特性" style="width:32px;" />|
|[可以使用授信運算佈建裸機工作者節點](cs_clusters.html#shared_dedicated_node) | |<img src="images/confirm.svg" width="32" alt="可用的特性" style="width:32px;" />|
|[在 {{site.data.keyword.Bluemix_dedicated_notm}} 中提供](cs_dedicated.html#dedicated_environment)| |<img src="images/confirm.svg" width="32" alt="可用的特性" style="width:32px;" />|
{: caption="免費及標準叢集的性質" caption-side="top"}

<br />




{: #responsibilities}
**附註**：要在您使用服務時，尋找您該負的責任及 {{site.data.keyword.containerlong}} 條款嗎？

{: #terms}
請參閱 [{{site.data.keyword.containershort_notm}} 責任](cs_responsibilities.html)。
