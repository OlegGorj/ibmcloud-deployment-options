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



# 您使用 {{site.data.keyword.containerlong_notm}} 的責任
瞭解使用 {{site.data.keyword.containerlong}} 時，您所擁有的叢集管理責任及條款。
{:shortdesc}

## 叢集管理責任
{: #responsibilities}

檢閱您與 IBM 共同分擔的叢集管理責任。
{:shortdesc}

**IBM 負責：**

- 在建立叢集時，在叢集內部署主節點、工作者節點及管理元件，例如 Ingress 應用程式負載平衡器
- 管理叢集的 Kubernetes 主節點安全更新、監視、隔離及回復
- 監視工作者節點的性能，並為那些工作者節點提供自動化更新及回復
- 對基礎架構帳戶執行自動化作業，包括新增工作者節點、移除工作者節點及建立預設子網路
- 管理、更新及回復叢集內的作業元件，例如 Ingress 應用程式負載平衡器及儲存空間外掛程式
- 在持續性磁區宣告要求時，佈建儲存磁區
- 在所有工作者節點上提供安全設定

</br>

**您負責：**

- [配置 {{site.data.keyword.Bluemix_notm}} 帳戶存取 IBM Cloud 基礎架構 (SoftLayer) 組合](cs_troubleshoot_clusters.html#cs_credentials)
- [在叢集內部署及管理 Kubernetes 資源，例如 Pod、服務及部署](cs_app.html#app_cli)
- [利用服務及 Kubernetes 的功能以確保應用程式的高可用性](cs_app.html#highly_available_apps)
- [使用 CLI 新增或移除工作者節點，以新增或移除叢集容量](cs_cli_reference.html#cs_worker_add)
- [在 IBM Cloud 基礎架構 (SoftLayer) 建立公用及專用 VLAN，以進行叢集的網路隔離](/docs/infrastructure/vlans/getting-started.html#getting-started-with-vlans)
- [確保所有工作者節點都具有 Kubernetes 主節點 URL 的網路連線功能](cs_firewall.html#firewall) <p>**附註**：如果工作者節點同時具有公用和專用 VLAN，則已配置網路連線功能。如果將工作者節點設定為僅具有專用 VLAN，則您必須配置替代方案以進行網路連線。如需相關資訊，請參閱[工作者節點的 VLAN 連線](cs_clusters.html#worker_vlan_connection)。</p>
- [當 Kubernetes 版本更新可用時，更新主節點 kube-apiserver](cs_cluster_update.html#master)
- [讓工作者節點保持最新的主要、次要及修補程式版本](cs_cluster_update.html#worker_node)
- [回復出問題的工作者節點，方法為執行 `kudectl` 指令，例如 `coron` 或 `drain`，以及執行 `bx cs` 指令，例如 `reboot`、`reload` 或 `delete`](cs_cli_reference.html#cs_worker_reboot)
- [視需要新增或移除 IBM Cloud 基礎架構 (SoftLayer) 中的子網路](cs_subnets.html#subnets)
- [在 IBM Cloud 基礎架構 (SoftLayer) 備份及還原持續性儲存空間裡的資料 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](../services/RegistryImages/ibm-backup-restore/index.html)
- [針對具有自動回復的工作者節點配置性能監視](cs_health.html#autorecovery)

<br />


## {{site.data.keyword.containerlong_notm}} 濫用
{: #terms}

客戶不得誤用 {{site.data.keyword.containershort_notm}}。
{:shortdesc}

誤用包括：

*   任何不合法的活動
*   散佈或執行惡意軟體
*   危害 {{site.data.keyword.containershort_notm}} 或干擾任何人使用 {{site.data.keyword.containershort_notm}}
*   危害或干擾任何人使用任何其他服務或系統
*   未經授權即存取任何服務或系統
*   未經授權即修改任何服務或系統
*   違反他人權利


如需整體使用條款，請參閱[雲端服務條款](https://console.bluemix.net/docs/overview/terms-of-use/notices.html#terms)。
