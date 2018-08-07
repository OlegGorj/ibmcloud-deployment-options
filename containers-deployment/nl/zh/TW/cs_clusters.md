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


# 設定叢集
{: #clusters}

使用 {{site.data.keyword.containerlong}} 來設計 Kubernetes 叢集設定，以達到最大容器可用性及容量。
{:shortdesc}



## 叢集配置規劃
{: #planning_clusters}

使用標準叢集，以增加應用程式可用性。
{:shortdesc}

當您將設定分佈到多個工作者節點及叢集時，使用者遇到應用程式關閉的可能性越低。內建功能（例如負載平衡及隔離）可提高對於潛在主機、網路或應用程式失敗的備援。


檢閱這些可能的叢集設定，它們依遞增的可用性程度進行排序：

![叢集高可用性階段](images/cs_cluster_ha_roadmap.png)

1.  一個具有多個工作者節點的叢集
2.  在相同地區的不同位置執行的兩個叢集，各具有多個工作者節點
3.  在不同地區執行的兩個叢集，各具有多個工作者節點


### 增加叢集的可用性

<dl>
  <dt>將應用程式分散到工作者節點</dt>
    <dd>容許開發人員針對每個叢集將其在容器中的應用程式分散到多個工作者節點。這三個工作者節點中每一個的應用程式實例都容許關閉一個工作者節點，而不會岔斷應用程式的使用。您可以指定從 [{{site.data.keyword.Bluemix_notm}} GUI](cs_clusters.html#clusters_ui) 或 [CLI](cs_clusters.html#clusters_cli) 建立叢集時要包含多少個工作者節點。Kubernetes 會限制您在叢集中可以具有的工作者節點數目上限，因此，請記住[工作者節點及 Pod 配額 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/admin/cluster-large/)。
<pre class="codeblock"><code>bx cs cluster-create --location dal10 --workers 3 --public-vlan &lt;public_VLAN_ID&gt; --private-vlan &lt;private_VLAN_ID&gt; --machine-type &lt;u2c.2x4&gt; --name &lt;cluster_name_or_ID&gt;</code></pre></dd>
<dt>將應用程式分散到叢集</dt>
    <dd>建立多個叢集，各具有多個工作者節點。如果其中一個叢集的運作中斷，使用者仍然可以存取同時部署在另一個叢集中的應用程式。<p>叢集 1：</p>
        <pre class="codeblock"><code>bx cs cluster-create --location dal10 --workers 3 --public-vlan &lt;public_VLAN_ID&gt; --private-vlan &lt;private_VLAN_ID&gt; --machine-type &lt;u2c.2x4&gt; --name &lt;cluster_name_or_ID&gt;</code></pre>
<p>叢集 2：</p>
        <pre class="codeblock"><code>bx cs cluster-create --location dal12 --workers 3 --public-vlan &lt;public_VLAN_ID&gt; --private-vlan &lt;private_VLAN_ID&gt; --machine-type &lt;u2c.2x4&gt; --name &lt;cluster_name_or_ID&gt;</code></pre></dd>
  <dt>將應用程式分散到不同地區中的叢集</dt>
    <dd>當您將應用程式分散到不同地區中的叢集時，可以容許根據使用者所在地區來進行負載平衡。如果一個地區的叢集、硬體，甚至整個位置關閉，資料流量會遞送至部署在另一個位置的容器。<p><strong>重要事項：</strong>配置自訂網域之後，即可使用這些指令來建立叢集。</p>
      <p>位置 1：</p>
        <pre class="codeblock"><code>bx cs cluster-create --location dal10 --workers 3 --public-vlan &lt;public_VLAN_ID&gt; --private-vlan &lt;private_VLAN_ID&gt; --machine-type &lt;u2c.2x4&gt; --name &lt;cluster_name_or_ID&gt;</code></pre>
<p>位置 2：</p>
        <pre class="codeblock"><code>bx cs cluster-create --location ams03 --workers 3 --public-vlan &lt;public_VLAN_ID&gt; --private-vlan &lt;private_VLAN_ID&gt; --machine-type &lt;u2c.2x4&gt; --name &lt;cluster_name_or_ID&gt;</code></pre></dd>
</dl>

<br />







## 工作者節點配置規劃
{: #planning_worker_nodes}

Kubernetes 叢集包含工作者節點，並且由 Kubernetes 主節點集中進行監視及管理。叢集管理者可以決定如何設定工作者節點的叢集，以確保叢集使用者具有部署及執行叢集中應用程式的所有資源。
{:shortdesc}

當您建立標準叢集時，會代表您在 IBM Cloud 基礎架構 (SoftLayer) 中訂購工作者節點，並將它們新增至叢集中的預設工作者節點儲存區。每個工作者節點都會獲指派建立叢集之後即不得變更的唯一工作者節點 ID 及網域名稱。

您可以選擇虛擬或實體（裸機）伺服器。視您選擇的硬體隔離層次而定，虛擬工作者節點可以設定為共用或專用節點。您也可以選擇想要工作者節點連接至公用 VLAN 及專用 VLAN，還是僅連接至專用 VLAN。每個工作者節點都會佈建特定機型，而此機型可決定部署至工作者節點的容器可用的 vCPU 數目、記憶體及磁碟空間。Kubernetes 會限制您在叢集中可以有的工作者節點數目上限。如需相關資訊，請檢閱[工作者節點及 Pod 配額 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/admin/cluster-large/)。



### 工作者節點的硬體
{: #shared_dedicated_node}

當您在 {{site.data.keyword.Bluemix_notm}} 中建立標準叢集時，可以選擇將工作者節點佈建為實體機器（裸機），或佈建為在實體硬體上執行的虛擬機器。當您建立免費叢集時，工作者節點會自動佈建為 IBM Cloud 基礎架構 (SoftLayer) 帳戶中的虛擬共用節點。
{:shortdesc}

![標準叢集中的工作者節點的硬體選項](images/cs_clusters_hardware.png)

請檢閱下列資訊，以決定您想要的工作者儲存區類型。計劃時，請考量[工作者節點最低臨界值](#resource_limit_node)，即總記憶體容量的 10%。

<dl>
<dt>為何要使用實體機器（裸機）？</dt>
<dd><p><strong>更多運算資源</strong>：您可以將工作者節點佈建為單一承租戶實體伺服器，也稱為裸機。裸機可讓您直接存取機器上的實體資源，例如記憶體或 CPU。此設定可免除虛擬機器 Hypervisor，該 Hypervisor 將實體資源配置給在主機上執行的虛擬機器。相反地，裸機的所有資源將由工作者節點專用，因此您不需要擔心「吵雜的鄰居」共用資源或降低效能。實體機型的本端儲存空間多於虛擬機型，有些實體機型具有 RAID 可用來備份本端資料。</p>
<p><strong>按月計費</strong>：裸機伺服器的成本高於虛擬伺服器，最適合需要更多資源及主機控制的高效能應用程式。裸機伺服器按月計費。如果您在月底之前取消裸機伺服器，則會向您收取該月整個月的費用。訂購及取消裸機伺服器是透過 IBM Cloud 基礎架構 (SoftLayer) 帳戶進行的手動程序。可能需要多個營業日才能完成。</p>
<p><strong>啟用授信運算的選項</strong>：啟用「授信運算」，以驗證工作者節點是否遭到竄改。如果您在叢集建立期間未啟用信任，但後來想要啟用，您可以使用 `bx cs feature-enable` [指令](cs_cli_reference.html#cs_cluster_feature_enable)。啟用信任之後，以後您就無法再予以停用。無需信任，即可建立新叢集。如需在節點啟動處理程序期間信任運作方式的相關資訊，請參閱[使用授信運算的 {{site.data.keyword.containershort_notm}}](cs_secure.html#trusted_compute)。「授信運算」適用於執行 Kubernets 1.9 版或更新版本，且具有特定裸機機型的叢集。當您執行 `bx cs machine-types <location>` [指令](cs_cli_reference.html#cs_machine_types)時，可以檢閱 **Trustable** 欄位來查看哪些機器支援信任。例如，`mgXc` GPU 特性不支援「授信運算」。</p></dd>
<dt>為何要使用虛擬機器？</dt>
<dd><p>使用 VM，您可以取得更大的彈性、更快速的佈建時間，以及比裸機還要多的自動可擴充性功能，而且價格更加划算。您可以對大部分一般用途的使用案例使用 VM，例如測試與開發環境、暫置及 Prod 環境、微服務，以及商務應用程式。不過，會犧牲效能。對於 RAM、資料或 GPU 密集的工作負載，如果您需要高效能運算，請使用裸機。</p>
<p><strong>決定單一承租戶或多方承租戶</strong>：當您建立標準虛擬叢集時，必須選擇是要由多個 {{site.data.keyword.IBM_notm}} 客戶（多方承租戶）共用基礎硬體，還是只供您一人專用（單一承租戶）。</p>
<p>在多方承租戶設定中，會在所有部署至相同實體硬體的虛擬機器之間共用實體資源（例如 CPU 及記憶體）。為了確保每台虛擬機器都可以獨立執行，虛擬機器監視器（也稱為 Hypervisor）會將實體資源分段為隔離實體，並將它們當成專用資源配置至虛擬機器（Hypervisor 隔離）。</p>
<p>在單一承租戶設定中，所有實體資源都只供您專用。您可以將多個工作者節點部署為相同實體主機上的虛擬機器。與多方承租戶設定類似，Hypervisor 確保每個工作者節點都可以共用可用的實體資源。</p>
<p>因為基礎硬體的成本是由多個客戶分攤，所以共用節點成本通常會比專用節點成本低。不過，當您決定共用或專用節點時，可能會想要與法務部門討論應用程式環境所需的基礎架構隔離及法規遵循層次。</p>
<p><strong>虛擬 `u2c` 或 `b2c` 機器特性</strong>：這些機器使用本端磁碟而非儲存區域網路 (SAN) 來提供可靠性。可靠性優點包括將位元組序列化到本端磁碟時的更高傳輸量，以及減少檔案系統由於網路故障而造成的退化。這些機型包含 25GB 主要本端磁碟儲存空間，供 OS 檔案系統使用，以及包含 100GB 次要本端磁碟儲存空間，供 `/var/lib/docker` 使用，所有容器資料都會寫入至這個目錄中。</p>
<p><strong>如果我已淘汰 `u1c` 或 `b1c` 機型，怎麼辨？</strong>若要開始使用 `u2c` 及 `b2c` 機型，[請藉由新增工作者節點來更新機型](cs_cluster_update.html#machine_type)。</p></dd>
<dt>可以選擇哪些虛擬及實體機器特性？</dt>
<dd><p>許多！選取最適合您使用案例的機器類型。請記住，工作者儲存區是由特性相同的機器所組成。如果要在您的叢集中混合各種機型，請對每一個特性建立個別的工作者儲存區。</p>
<p>機型因區域而異。若要查看您區域中可用的機器類型，請執行 `bx cs machine-types<zone_name>`。</p>
<p><table>
<caption>{{site.data.keyword.containershort_notm}} 中可用的實體（裸機）及虛擬機器類型。</caption>
<thead>
<th>名稱及使用案例</th>
<th>核心 / 記憶體</th>
<th>主要 / 次要磁碟</th>
<th>網路速度</th>
</thead>
<tbody>
<tr>
<td><strong>虛擬，u2c.2x4</strong>：針對快速測試、概念證明，以及其他輕型工作負載，使用這個大小最小的 VM。</td>
<td>2 / 4GB</td>
<td>25GB / 100GB</td>
<td>1000Mbps</td>
</tr>
<tr>
<td><strong>虛擬，b2c.4x16</strong>：針對測試與開發，以及其他輕型工作負載，選取這個已平衡的 VM。</td>
<td>4 / 16GB</td>
<td>25GB / 100GB</td>
<td>1000Mbps</td>
</tr>
<tr>
<td><strong>虛擬，b2c.16x64</strong>：針對中型工作負載，選取這個已平衡的 VM。</td></td>
<td>16 / 64GB</td>
<td>25GB / 100GB</td>
<td>1000Mbps</td>
</tr>
<tr>
<td><strong>虛擬，b2c.32x128</strong>：針對中型或大型工作負載（例如一個資料庫及一個具有許多並行使用者的動態網站），選取這個已平衡的 VM。</td></td>
<td>32 / 128GB</td>
<td>25GB / 100GB</td>
<td>1000Mbps</td>
</tr>
<tr>
<td><strong>虛擬，b2c.56x242</strong>：針對大型工作負載（例如一個資料庫及多個具有許多並行使用者的應用程式），選取這個已平衡的 VM。</td></td>
<td>56 / 242GB</td>
<td>25GB / 100GB</td>
<td>1000Mbps</td>
</tr>
<tr>
<td><strong>RAM 密集的裸機，mr1c.28x512</strong>：將工作者節點可用的 RAM 最大化。</td>
<td>28 / 512GB</td>
<td>2TB SATA / 960GB SSD</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>GPU 裸機，mg1c.16x128</strong>：針對數學運算密集的工作負載（例如高效能運算、機器學習或 3D 應用程式）選擇這種類型。此特性有 1 張 Tesla K80 實體卡，而每張卡有 2 個圖形處理裝置 (GPU)，總計 2 個 GPU。</td>
<td>16 / 128GB</td>
<td>2TB SATA / 960GB SSD</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>GPU 裸機，mg1c.28x256</strong>：針對數學運算密集的工作負載（例如高效能運算、機器學習或 3D 應用程式）選擇這種類型。此特性有 2 張 Tesla K80 實體卡，而每張卡有 2 個 GPU，總計 4 個 GPU。</td>
<td>28 / 256GB</td>
<td>2TB SATA / 960GB SSD</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>資料密集的裸機，md1c.16x64.4x4tb</strong>：適用於大量的本端磁碟儲存空間，包括 RAID 來備份儲存在本端機器上的資料。用於分散式檔案系統、大型資料庫及海量資料分析工作負載之類的案例。</td>
<td>16 / 64GB</td>
<td>2x2TB RAID1 / 4x4TB SATA RAID10</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>資料密集的裸機，md1c.28x512.4x4tb</strong>：適用於大量的本端磁碟儲存空間，包括 RAID 來備份儲存在本端機器上的資料。用於分散式檔案系統、大型資料庫及海量資料分析工作負載之類的案例。</td>
<td>28 / 512 GB</td>
<td>2x2TB RAID1 / 4x4TB SATA RAID10</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>平衡的裸機，mb1c.4x32</strong>：用於所需的運算資源比虛擬機器提供的還多的已平衡工作負載。</td>
<td>4 / 32GB</td>
<td>2TB SATA / 2TB SATA</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>平衡的裸機，mb1c.16x64</strong>：用於所需的運算資源比虛擬機器提供的還多的已平衡工作負載。</td>
<td>16 / 64GB</td>
<td>2TB SATA / 960GB SSD</td>
<td>10000Mbps</td>
</tr>
</tbody>
</table>
</p>
</dd>
</dl>


您可以使用[主控台使用者介面](#clusters_ui)或 [CLI](#clusters_cli) 來部署叢集。

### 工作者節點的 VLAN 連線
{: #worker_vlan_connection}

當您建立叢集時，工作者節點會自動從 IBM Cloud 基礎架構 (SoftLayer) 帳戶連接至 VLAN。
{:shortdesc}

VLAN 會配置一組工作者節點及 Pod，就像它們已連接至相同的實體佈線。
* 公用 VLAN 具有兩個可在其上自動佈建的子網路。主要公用子網路會判定在建立叢集期間指派給工作者節點的公用 IP 位址，而可攜式公用子網路會提供 Ingress 及負載平衡器網路服務的公用 IP 位址。
* 專用 VLAN 也有兩個可在其上自動佈建的子網路。主要專用子網路會判定在建立叢集期間指派給工作者節點的專用 IP 位址，而可攜式專用子網路會提供 Ingress 及負載平衡器網路服務的專用 IP 位址。

若為免費叢集，叢集的工作者節點會在建立叢集期間連接至 IBM 擁有的公用 VLAN 及專用 VLAN。

若為標準叢集，第一次在位置中建立叢集時，會自動為您佈建公用 VLAN 及專用 VLAN。針對您在該位置建立的每個後續叢集，您可以選擇要使用的 VLAN。您可以將工作者節點同時連接至公用 VLAN 及專用 VLAN，或僅連接至專用 VLAN。如果您只想要將工作者節點連接至專用 VLAN，則可以使用現有專用 VLAN 的 ID，或[建立專用 VLAN](/docs/cli/reference/softlayer/index.html#sl_vlan_create)，並在建立叢集期間使用 ID。如果將工作者節點設定為僅具有專用 VLAN，則您必須配置替代方案以進行網路連線，例如 [Virtual Router Appliance](cs_vpn.html#vyatta)，讓工作者節點可以與主節點通訊。

**附註**：如果一個叢集具有多個 VLAN，或相同的 VLAN 上具有多個子網路，您必須開啟 VLAN 跨越，讓工作者節點可以在專用網路上彼此通訊。如需指示，請參閱[啟用或停用 VLAN 跨越](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning)。

### 工作者節點記憶體限制
{: #resource_limit_node}

{{site.data.keyword.containershort_notm}} 會在每一個工作者節點上設定記憶體限制。當工作者節點上執行的 Pod 超出此記憶體限制時，就會移除這些 Pod。在 Kubernetes 中，此限制稱為[強制收回臨界值 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/tasks/administer-cluster/out-of-resource/#hard-eviction-thresholds)。
{:shortdesc}

如果經常移除 Pod，請將更多的工作者節點新增至叢集，或在 Pod 上設定[資源限制 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#resource-requests-and-limits-of-pod-and-container)。

**每一部機器都有最低臨界值，等於其總記憶體容量的 10%**。如果工作者節點上可用的記憶體少於容許的最低臨界值，Kubernetes 會立即移除 Pod。如果有工作者節點可供使用，則 Pod 會重新排定至另一個工作者節點。例如，如果您有一台 `b2c.4x16` 虛擬機器，它的總記憶體容量是 16GB。如果可用記憶體少於 1600MB (10%)，則無法在這個工作者節點上排定新 Pod，而會在另一個工作者節點上排定。如果沒有其他工作者節點可用，則新 Pod 會保持未排定狀態。

若要檢閱工作者節點上使用的記憶體數量，請執行 [kubectltop node ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/reference/kubectl/overview/#top)。

### 工作者節點的自動回復
`Docker`、`kubelet`、`kube-proxy` 及 `calico` 是重要元件，它們必須發揮作用，才能有一個健全的 Kubernetes 工作者節點。經過一段時間後，這些元件可能會中斷，而讓工作者節點處於無功能狀態。無功能工作者節點會降低叢集的總容量，並可能導致應用程式關閉。

您可以[為工作者節點配置性能檢查，並啟用自動回復](cs_health.html#autorecovery)。如果「自動回復」根據配置的檢查，偵測到性能不佳的工作者節點，則「自動回復」會觸發更正動作，如在工作者節點上重新載入 OS。如需自動回復運作方式的相關資訊，請參閱[自動回復部落格![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/)。

<br />



## 使用 GUI 建立叢集
{: #clusters_ui}

Kubernetes 叢集的用途是要定義一組資源、節點、網路及儲存裝置，讓應用程式保持高可用性。您必須先建立叢集並設定該叢集中工作者節點的定義，才能部署應用程式。
{:shortdesc}





開始之前，您必須具有「隨收隨付制」或「訂閱」的 [{{site.data.keyword.Bluemix_notm}} 帳戶](https://console.bluemix.net/registration/)，其會配置為[存取 IBM Cloud 基礎架構 (SoftLayer) 組合](cs_troubleshoot_clusters.html#cs_credentials)。若要嘗試部分功能，您可以建立在 21 天後到期的免費叢集。您一次最多可以有 1 個免費叢集。

您可以隨時移除您的免費叢集，但在 21 天後，會刪除免費叢集及其資料，且無法還原。請務必備份您的資料。
{: tip}

若要完全自訂您的叢集，並選擇硬體隔離、區域、API 版本等等，請建立標準叢集。

若要建立叢集，請執行下列動作：

1. 在型錄中，選取 **Kubernetes 叢集**。

2. 選取要部署叢集的區域。

3. 選取叢集方案的類型。您可以選擇**免費**或**標準**。使用標準叢集，您可以存取諸如多個工作者節點的特性，以達到高可用性環境。

4. 配置叢集詳細資料。請完成適用於您要建立之叢集類型的步驟。

    1. **免費及標準**：提供叢集名稱。此名稱必須以字母開頭，可以包含字母、數字及連字號 (-)，且長度不得超過 35 個字元。叢集名稱和叢集部署所在的地區會形成 Ingress 子網域的完整網域名稱。為了確保 Ingress 子網域在地區內是唯一的，叢集名稱可能會被截斷，並附加 Ingress 網域名稱內的隨機值。
 
    2. **標準**：選取部署叢集的位置。如需最佳效能，請選取實際上與您最接近的位置。當您選取所在國家/地區以外的位置時，請謹記，您可能需要有合法授權，才能將資料實際儲存在其他國家/地區。

    3. **標準**：選擇叢集主節點的 Kubernetes API 伺服器版本。

    4. **標準**：選取硬體隔離的類型。虛擬伺服器是按小時計費，裸機伺服器是按月計費。

        - **虛擬 - 專用**：工作者節點是在您帳戶專用的基礎架構上進行管理。實體資源完全被隔離。

        - **虛擬 - 共用**：您與其他 IBM 客戶之間可以共用基礎架構資源（例如 Hypervisor 和實體硬體），但每一個工作者節點僅供您存取。雖然此選項的成本比較低，而且在大部分情況下已夠用，您可能要根據公司原則來驗證效能及基礎架構需求。

        - **裸機**：按月計費的裸機伺服器是透過與 IBM Cloud 基礎架構 (SoftLayer) 之間的手動互動來進行佈建，可能需要多個營業日才能完成。裸機伺服器最適合需要更多資源和主機控制的高效能應用程式。

        請確定您要佈建裸機機器。因為它是按月計費，如果您不慎下訂之後立即取消，仍會向您收取整個月的費用。
        {:tip}

    5.  **標準**：選取機型。機型會定義設定於每一個工作者節點中，且可供容器使用的虛擬 CPU、記憶體及磁碟空間量。可用的裸機及虛擬機器類型會因您部署叢集所在的位置而不同。建立叢集之後，您可以藉由將節點新增至叢集來新增不同的機型。

    6. **標準**：指定您在叢集中需要的工作者節點數目。

    7. **標準**：從 IBM Cloud 基礎架構 (SoftLayer) 帳戶中，選取一個公用 VLAN（選用）及專用 VLAN（必要）。兩個 VLAN 會在工作者節點之間進行通訊，但公用 VLAN 也與 IBM 管理的 Kubernetes 主節點通訊。您可以將相同的 VLAN 用於多個叢集。**附註**：如果將工作者節點設定為僅具有專用 VLAN，則您必須配置替代方案以進行網路連線。如需相關資訊，請參閱[工作者節點的 VLAN 連線](cs_clusters.html#worker_vlan_connection)。

    8. 依預設，會選取**加密本端磁碟**。如果您選擇清除這個勾選框，則主機的 Docker 資料不會加密。


4. 按一下**建立叢集**。您可以在**工作者節點**標籤中查看工作者節點部署的進度。部署完成時，您可以在**概觀**標籤中看到叢集已備妥。**附註：**每個工作者節點都會獲指派唯一的工作者節點 ID 及網域名稱，在叢集建立之後即不得手動予以變更。變更 ID 或網域名稱會讓 Kubernetes 主節點無法管理叢集。



**下一步為何？**

當叢集開始執行時，您可以查看下列作業：


-   [安裝 CLI 以開始使用您的叢集。](cs_cli_install.html#cs_cli_install)
-   [在叢集中部署應用程式。](cs_app.html#app_cli)
-   [在 {{site.data.keyword.Bluemix_notm}} 中設定您自己的專用登錄，以儲存 Docker 映像檔，並將它與其他使用者共用。](/docs/services/Registry/index.html)
- 如果一個叢集具有多個 VLAN，或相同的 VLAN 上具有多個子網路，您必須[開啟 VLAN 跨越](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning)，讓工作者節點可以在專用網路上彼此通訊。
- 如果您有防火牆，則可能需要[開啟必要埠](cs_firewall.html#firewall)，才能使用 `bx`、`kubectl` 或 `calicotl` 指令以容許來自叢集的出埠資料流量，或容許網路服務的入埠資料流量。

<br />


## 使用 CLI 建立叢集
{: #clusters_cli}

Kubernetes 叢集的用途是要定義一組資源、節點、網路及儲存裝置，讓應用程式保持高可用性。您必須先建立叢集並設定該叢集中工作者節點的定義，才能部署應用程式。
{:shortdesc}

開始之前：
- 您必須具有「隨收隨付制」或「訂閱」的 [{{site.data.keyword.Bluemix_notm}} 帳戶](https://console.bluemix.net/registration/)，其會配置為[存取 IBM Cloud 基礎架構 (SoftLayer) 組合](cs_troubleshoot_clusters.html#cs_credentials)。您可以建立 1 個免費叢集來試用部分功能 21 天，或建立完全可自訂的標準叢集，並選擇硬體隔離。
- [請確定您在 IBM Cloud 基礎架構 (SoftLayer) 中具有佈建標準叢集所需的最少許可權](cs_users.html#infra_access)。

若要建立叢集，請執行下列動作：

1.  安裝 {{site.data.keyword.Bluemix_notm}} CLI 及 [{{site.data.keyword.containershort_notm}} 外掛程式](cs_cli_install.html#cs_cli_install)。

2.  登入 {{site.data.keyword.Bluemix_notm}} CLI。系統提示時，請輸入您的 {{site.data.keyword.Bluemix_notm}} 認證。

    ```
        bx login
    ```
    {: pre}

    **附註：**如果您具有聯合 ID，請使用 `bx login --sso` 來登入 {{site.data.keyword.Bluemix_notm}} CLI。請輸入使用者名稱，並使用 CLI 輸出中提供的 URL 來擷取一次性密碼。若沒有 `--sso` 時登入失敗，而有 `--sso` 選項時登入成功，即表示您有聯合 ID。

3. 如果您有多個 {{site.data.keyword.Bluemix_notm}} 帳戶，請選取您要在其中建立 Kubernetes 叢集的帳戶。

4.  如果您要在先前所選取 {{site.data.keyword.Bluemix_notm}} 地區以外的地區中建立或存取 Kubernetes 叢集，請執行 `bx cs region-set`。

6.  建立叢集。

    1.  **標準叢集**：檢閱可用的位置。顯示的位置取決於您所登入的 {{site.data.keyword.containershort_notm}} 地區。

        ```
        bx cs locations
        ```
        {: pre}
        
        您的 CLI 輸出符合 [{{site.data.keyword.containerlong}} 地區的位置](cs_regions.html#locations)。
        
    2.  **標準叢集**：選擇位置，並檢閱該位置中可用的機型。機型指定每一個工作者節點可用的虛擬或實體運算主機。

        -  檢視**伺服器類型**欄位，以選擇虛擬或實體（裸機）機器。
        -  **虛擬**：按小時計費的虛擬機器是佈建在共用或專用硬體上。
        -  **實體**：按月計費的裸機伺服器透過與 IBM Cloud 基礎架構 (SoftLayer) 之間的手動互動來進行佈建，可能需要多個營業日才能完成。裸機伺服器最適合需要更多資源和主機控制的高效能應用程式。
        - **使用授信運算的實體機器**：對於執行 Kubernetes 1.9 版或更新版本的裸機叢集，您也可以選擇啟用[授信運算](cs_secure.html#trusted_compute)來驗證裸機工作者節點是否遭到竄改。「授信運算」可用於選取裸機的機型。例如，`mgXc` GPU 特性不支援「授信運算」。如果您在叢集建立期間未啟用信任，但後來想要啟用，您可以使用 `bx cs feature-enable` [指令](cs_cli_reference.html#cs_cluster_feature_enable)。啟用信任之後，以後您就無法再予以停用。
        -  **機型**：若要決定要部署的機型，請檢閱核心、記憶體及儲存空間組合，或參閱 `bx cs machine-types` [指令文件](cs_cli_reference.html#cs_machine_types)。建立叢集之後，您可以使用 `bx cs worker-add` [指令](cs_cli_reference.html#cs_worker_add)來新增不同的實體或虛擬機器類型。

           請確定您要佈建裸機機器。因為它是按月計費，如果您不慎下訂之後立即取消，仍會向您收取整個月的費用。
        {:tip}

        ```
        bx cs machine-types <location>
        ```
        {: pre}

    3.  **標準叢集**：查看 IBM Cloud 基礎架構 (SoftLayer) 中是否已存在此帳戶的公用及專用 VLAN。

        ```
        bx cs vlans <location>
        ```
        {: pre}

        ```
        ID        Name   Number   Type      Router
        1519999   vlan   1355     private   bcr02a.dal10
        1519898   vlan   1357     private   bcr02a.dal10
        1518787   vlan   1252     public    fcr02a.dal10
        1518888   vlan   1254     public    fcr02a.dal10
        ```
        {: screen}

        如果公用及專用 VLAN 已存在，請記下相符的路由器。專用 VLAN 路由器的開頭一律為 <code>bcr</code>（後端路由器），而公用 VLAN 路由器的開頭一律為 <code>fcr</code>（前端路由器）。當建立叢集並指定公用和專用 VLAN 時，那些字首之後的數字和字母組合必須相符。在範例輸出中，任何專用 VLAN 都可以與任何公用 VLAN 搭配使用，因為路由器都會包括 `02a.dal10`。

        您必須將工作者節點連接至專用 VLAN，也可以選擇性地將工作者節點連接至公用 VLAN。**附註**：如果將工作者節點設定為僅具有專用 VLAN，則您必須配置替代方案以進行網路連線。如需相關資訊，請參閱[工作者節點的 VLAN 連線](cs_clusters.html#worker_vlan_connection)。

    4.  **免費及標準叢集**：執行 `cluster-create` 指令。您可以選擇免費叢集（包括一個已設定 2vCPU 及 4GB 記憶體的工作者節點），且會在 21 天後自動刪除。當您建立標準叢集時，依預設，會加密工作者節點磁碟，其硬體由多位 IBM 客戶所共用，並且按使用時數計費。</br>標準叢集的範例。指定叢集的選項：

        ```
        bx cs cluster-create --location dal10 --machine-type u2c.2x4 --hardware <shared_or_dedicated> --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID> --workers 3 --name <cluster_name> --kube-version <major.minor.patch> [--disable-disk-encrypt][--trusted]
        ```
        {: pre}

        免費叢集的範例。指定叢集名稱：

        ```
        bx cs cluster-create --name my_cluster
        ```
        {: pre}

        <table>
        <caption>cluster-create 元件</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解此指令的元件</th>
        </thead>
        <tbody>
        <tr>
        <td><code>cluster-create</code></td>
        <td>在 {{site.data.keyword.Bluemix_notm}} 組織中建立叢集的指令。</td>
        </tr>
        <tr>
        <td><code>--location <em>&lt;location&gt;</em></code></td>
        <td>**標準叢集**：將 <em>&lt;location&gt;</em> 取代為您要建立叢集的 {{site.data.keyword.Bluemix_notm}} 位置 ID。[可用的位置](cs_regions.html#locations)取決於您所登入的 {{site.data.keyword.containershort_notm}} 地區。</td>
        </tr>
        <tr>
        <td><code>--machine-type <em>&lt;machine_type&gt;</em></code></td>
        <td>**標準叢集**：選擇機型。您可以將工作者節點部署為共用或專用硬體上的虛擬機器，或部署為裸機上的實體機器。可用的實體及虛擬機器類型會因您部署叢集所在的位置而不同。如需相關資訊，請參閱 `bx cs machine-type` [指令](cs_cli_reference.html#cs_machine_types) 的文件。若為免費叢集，您不需要定義機型。</td>
        </tr>
        <tr>
        <td><code>--hardware <em>&lt;shared_or_dedicated&gt;</em></code></td>
        <td>**標準叢集，僅限虛擬**：您的工作者節點的硬體隔離層次。若要讓可用的實體資源只供您專用，請使用 dedicated，或者，若要容許與其他 IBM 客戶共用實體資源，請使用 shared。預設值為 shared。此值對於標準叢集是選用的，不適用於免費叢集。</td>
        </tr>
        <tr>
        <td><code>--public-vlan <em>&lt;public_vlan_id&gt;</em></code></td>
        <td><ul>
          <li>**免費叢集**：您不需要定義公用 VLAN。免費叢集會自動連接至 IBM 所擁有的公用 VLAN。</li>
          <li>**標準叢集**：如果您在 IBM Cloud 基礎架構 (SoftLayer) 帳戶中已設定用於該位置的公用 VLAN，請輸入公用 VLAN 的 ID。如果您只要將工作者節點連接至專用 VLAN，請不要指定此選項。**附註**：如果將工作者節點設定為僅具有專用 VLAN，則您必須配置替代方案以進行網路連線。如需相關資訊，請參閱[工作者節點的 VLAN 連線](cs_clusters.html#worker_vlan_connection)。<br/><br/>
          <strong>附註</strong>：專用 VLAN 路由器的開頭一律為 <code>bcr</code>（後端路由器），而公用 VLAN 路由器的開頭一律為 <code>fcr</code>（前端路由器）。當建立叢集並指定公用和專用 VLAN 時，那些字首之後的數字和字母組合必須相符。</li>
        </ul></td>
        </tr>
        <tr>
        <td><code>--private-vlan <em>&lt;private_vlan_id&gt;</em></code></td>
        <td><ul><li>**免費叢集**：您不需要定義專用 VLAN。免費叢集會自動連接至 IBM 所擁有的專用 VLAN。</li><li>**標準叢集**：如果您在 IBM Cloud 基礎架構 (SoftLayer) 帳戶中已設定用於該位置的專用 VLAN，請輸入專用 VLAN 的 ID。如果您的帳戶中沒有專用 VLAN，請不要指定此選項。{{site.data.keyword.containershort_notm}} 會自動為您建立一個專用 VLAN。<br/><br/><strong>附註</strong>：專用 VLAN 路由器的開頭一律為 <code>bcr</code>（後端路由器），而公用 VLAN 路由器的開頭一律為 <code>fcr</code>（前端路由器）。當建立叢集並指定公用和專用 VLAN 時，那些字首之後的數字和字母組合必須相符。</li></ul></td>
        </tr>
        <tr>
        <td><code>--name <em>&lt;name&gt;</em></code></td>
        <td>**免費及標準叢集**：將 <em>&lt;name&gt;</em> 取代為您的叢集名稱。此名稱必須以字母開頭，可以包含字母、數字及連字號 (-)，且長度不得超過 35 個字元。叢集名稱和叢集部署所在的地區會形成 Ingress 子網域的完整網域名稱。為了確保 Ingress 子網域在地區內是唯一的，叢集名稱可能會被截斷，並在 Ingress 網域名稱內附加一個隨機值。
</td>
        </tr>
        <tr>
        <td><code>--workers <em>&lt;number&gt;</em></code></td>
        <td>**標準叢集**：要內含在叢集中的工作者節點數目。如果未指定 <code>--workers</code> 選項，會建立 1 個工作者節點。</td>
        </tr>
        <tr>
          <td><code>--kube-version <em>&lt;major.minor.patch&gt;</em></code></td>
          <td>**標準叢集**：叢集主節點的 Kubernetes 版本。這是選用值。未指定版本時，會使用支援的 Kubernetes 版本的預設值來建立叢集。若要查看可用的版本，請執行 <code>bx cs kube-versions</code>。</td>
        </tr>
        <tr>
        <td><code>--disable-disk-encrypt</code></td>
        <td>**免費及標準叢集**：依預設，工作者節點的特色為磁碟加密；[進一步瞭解](cs_secure.html#encrypted_disks)。如果您要停用加密，請包括此選項。</td>
        </tr>
        <tr>
        <td><code>--trusted</code></td>
        <td>**標準裸機叢集**：啟用[授信運算](cs_secure.html#trusted_compute)，以驗證裸機工作者節點是否遭到竄改。「授信運算」可用於選取裸機的機型。例如，`mgXc` GPU 特性不支援「授信運算」。如果您在叢集建立期間未啟用信任，但後來想要啟用，您可以使用 `bx cs feature-enable` [指令](cs_cli_reference.html#cs_cluster_feature_enable)。啟用信任之後，以後您就無法再予以停用。</td>
        </tr>
        </tbody></table>

7.  驗證已要求建立叢集。

    ```
    bx cs clusters
    ```
    {: pre}

    **附註：**若為虛擬機器，要訂購工作者節點機器並且在您的帳戶中設定及佈建叢集，可能需要一些時間。裸機實體機器是透過與 IBM Cloud 基礎架構 (SoftLayer) 之間的手動互動來進行佈建，可能需要多個營業日才能完成。

    叢集佈建完成之後，叢集的狀態會變更為 **deployed**。

    ```
    Name         ID                                   State      Created          Workers   Location   Version
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1         mil01      1.9.7
    ```
    {: screen}

8.  檢查工作者節點的狀態。

    ```
    bx cs workers <cluster_name_or_ID>
    ```
    {: pre}

    工作者節點備妥後，狀態會變更為 **Normal**，而且狀態為 **Ready**。當節點狀態為 **Ready** 時，您就可以存取叢集。

    **附註：**每個工作者節點都會獲指派唯一的工作者節點 ID 及網域名稱，在叢集建立之後即不得手動予以變更。變更 ID 或網域名稱會讓 Kubernetes 主節點無法管理叢集。

    ```
    ID                                                 Public IP       Private IP      Machine Type   State    Status   Location   Version
    kube-mil01-paf97e8843e29941b49c598f516de72101-w1   169.xx.xxx.xxx  10.xxx.xx.xxx   free           normal   Ready    mil01      1.9.7
    ```
    {: screen}

9. 將您建立的叢集設為此階段作業的環境定義。請在您每次使用叢集時，完成下列配置步驟。
    1.  取得指令來設定環境變數，並下載 Kubernetes 配置檔。

        ```
        bx cs cluster-config <cluster_name_or_ID>
        ```
        {: pre}

        配置檔下載完成之後，會顯示一個指令，可讓您用來將本端 Kubernetes 配置檔的路徑設定為環境變數。

        OS X 的範例：

        ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

    2.  複製並貼上終端機中顯示的指令，以設定 `KUBECONFIG` 環境變數。
    3.  驗證 `KUBECONFIG` 環境變數已適當設定。

        OS X 的範例：

        ```
        echo $KUBECONFIG
        ```
        {: pre}

        輸出：

        ```
        /Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml

        ```
        {: screen}

10. 使用預設埠 `8001` 來啟動 Kubernetes 儀表板。
    1.  使用預設埠號來設定 Proxy。

        ```
        kubectl proxy
        ```
        {: pre}

        ```
        Starting to serve on 127.0.0.1:8001
        ```
        {: screen}

    2.  在 Web 瀏覽器中開啟下列 URL，以查看 Kubernetes 儀表板。

        ```
        http://localhost:8001/ui
        ```
        {: codeblock}


**下一步為何？**


-   [在叢集中部署應用程式。](cs_app.html#app_cli)
-   [使用 `kubectl` 指令行管理叢集 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")。](https://kubernetes.io/docs/reference/kubectl/overview/)
-   [在 {{site.data.keyword.Bluemix_notm}} 中設定您自己的專用登錄，以儲存 Docker 映像檔，並將它與其他使用者共用。](/docs/services/Registry/index.html)
- 如果一個叢集具有多個 VLAN，或相同的 VLAN 上具有多個子網路，您必須[開啟 VLAN 跨越](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning)，讓工作者節點可以在專用網路上彼此通訊。
- 如果您有防火牆，則可能需要[開啟必要埠](cs_firewall.html#firewall)，才能使用 `bx`、`kubectl` 或 `calicotl` 指令以容許來自叢集的出埠資料流量，或容許網路服務的入埠資料流量。

<br />





## 檢視叢集狀態
{: #states}

檢閱 Kubernetes 叢集的狀態，以取得叢集可用性及容量的相關資訊，以及可能已發生的潛在問題。
{:shortdesc}

若要檢視特定叢集的相關資訊，例如其位置、主要 URL、Ingress 子網域、版本、擁有者及監視儀表板，請使用 `bx cs cluster-get <cluster_name_or_ID>` [指令](cs_cli_reference.html#cs_cluster_get)。包括 `--showResources` 旗標，以檢視其他叢集資源，例如，儲存空間 Pod 的附加程式，或是公用及專用 IP 的子網路 VLAN。

您可以執行 `bx cs clusters` 指令並找出 **State** 欄位，以檢視現行叢集狀態。若要對叢集及工作者節點進行疑難排解，請參閱[叢集的疑難排解](cs_troubleshoot.html#debug_clusters)。

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


## 移除叢集
{: #remove}

不再需要使用「隨收隨付制」帳戶所建立的免費及標準叢集時，必須手動移除這些叢集，以便它們不再耗用資源。
{:shortdesc}

**警告：**
  - 不會在持續性儲存空間中建立叢集或資料的備份。刪除叢集或持續性儲存空間是永久性的，無法復原。
  - 移除叢集時，您也會移除在建立叢集時自動佈建的任何子網路，以及您使用 `bx cs cluster-subnet-create` 指令所建立的任何子網路。不過，如果您使用 `bx cs cluster-subnet-add` 指令，手動將現有子網路新增至叢集，則不會從您的 IBM Cloud 基礎架構 (SoftLayer) 帳戶中移除這些子網路，而且您可以在其他叢集中重複使用它們。

開始之前，請記下您的叢集 ID。您可能需要叢集 ID，才能調查及移除未隨叢集自動刪除的相關 IBM Cloud 基礎架構 (SoftLayer) 資源，例如持續性儲存空間。
{: tip}

若要移除叢集，請執行下列動作：

-   從 {{site.data.keyword.Bluemix_notm}} GUI
    1.  選取叢集，然後按一下**其他動作...** 功能表中的**刪除**。

-   從 {{site.data.keyword.Bluemix_notm}} CLI
    1.  列出可用的叢集。

        ```
                bx cs clusters
        ```
        {: pre}

    2.  刪除叢集。

        ```
                bx cs cluster-rm <cluster_name_or_ID>
        ```
        {: pre}

    3.  遵循提示，然後選擇是否刪除叢集資源，其中包括容器、Pod、連結服務、持續性儲存空間及密碼。
      - **持續性儲存空間**：持續性儲存空間為您的資料提供高可用性。如果您使用[現有的檔案共用](cs_storage.html#existing)建立了持續性磁區宣告，則在刪除叢集時，無法刪除檔案共用。您必須稍後從 IBM Cloud 基礎架構 (SoftLayer) 組合中手動刪除檔案共用。

          **附註**：由於每月計費週期，不能在月底最後一天刪除持續性磁區宣告。如果您在該月份的最後一天刪除持續性磁區宣告，則刪除會保持擱置，直到下個月開始為止。

後續步驟：
- 當您執行 `bx cs clusters` 指令時，在可用的叢集清單中不再列出已移除的叢集之後，您可以重複使用該叢集的名稱。
- 如果保留子網路，則可以[在新的叢集中重複使用它們](cs_subnets.html#custom)，或稍後從 IBM Cloud 基礎架構 (SoftLayer) 組合中手動刪除它們。
- 如果保留持續性儲存空間，則稍後可以透過 {{site.data.keyword.Bluemix_notm}} GUI 中的 IBM Cloud 基礎架構 (SoftLayer) 儀表板來刪除您的儲存空間。
