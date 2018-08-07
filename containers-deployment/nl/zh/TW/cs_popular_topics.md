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




# {{site.data.keyword.containershort_notm}} 的熱門主題
{: #cs_popular_topics}

掌握 {{site.data.keyword.containerlong}} 中發生的情況。瞭解要探索的新特性、要試用的訣竅，或其他開發人員目前發現很有用的一些熱門主題。
{:shortdesc}

## 2018 年 5 月的熱門主題
{: #may18}


<table summary="此表格顯示熱門主題。列應該從左到右閱讀，第一欄為日期，第二欄為特性的標題，第三欄為說明。">
<caption>2018 年 5 月關於容器及 Kubernetes 叢集的熱門主題</caption>
<thead>
<th>日期</th>
<th>標題</th>
<th>說明</th>
</thead>
<tbody>
<tr>
<td>5 月 24 日</td>
<td>[新的 Ingress 子網域格式](cs_ingress.html)</td>
<td>在 5 月 24 日之後建立的叢集會獲指派新格式的子網域，即 <code>&lt;cluster_name&gt;.&lt;region&gt;.containers.appdomain.cloud</code>。使用 Ingress 公開您的應用程式時，您可以使用新的子網域，從網際網路存取您的應用程式。</td>
</tr>
<tr>
<td>5 月 14 日</td>
<td>[更新：在全球 GPU 裸機上部署工作負載](cs_app.html#gpu_app)</td>
<td>如果您的叢集中有[裸機圖形處理裝置 (GPU) 機型](cs_clusters.html#shared_dedicated_node)，則可以排定數學運算密集的應用程式。GPU 工作者節點可以同時跨 CPU 及 GPU 處理應用程式的工作負載，以增進效能。</td>
</tr>
<tr>
<td>5 月 3 日</td>
<td>[Container Image Security Enforcement（測試版）](/docs/services/Registry/registry_security_enforce.html#security_enforce)</td>
<td>您的團隊是否需要一些額外的協助，才能知道要在應用程式容器中取回哪個映像檔？在部署容器映像檔之前，請試用 Container Image Security Enforcement 測試版以驗證它們。可供執行 Kubernetes 1.9 或更新版本的叢集使用。</td>
</tr>
<tr>
<td>5 月 1 日</td>
<td>[從 GUI 部署 Kubernetes 儀表板](cs_app.html#cli_dashboard)</td>
<td>您是否曾想要利用按一下滑鼠來存取 Kubernetes 儀表板？請查看 {{site.data.keyword.Bluemix_notm}} GUI 中的 **Kubernetes 儀表板**按鈕。</td>
</tr>
</tbody></table>




## 2018 年 4 月的熱門主題
{: #apr18}

<table summary="此表格顯示熱門主題。列應該從左到右閱讀，第一欄為日期，第二欄為特性的標題，第三欄為說明。">
<caption>2018 年 4 月關於容器及 Kubernetes 叢集的熱門主題</caption>
<thead>
<th>日期</th>
<th>標題</th>
<th>說明</th>
</thead>
<tbody>
<tr>
<td>4 月 17 日</td>
<td>{{site.data.keyword.Bluemix_notm}} Block Storage</td>
<td>安裝 {{site.data.keyword.Bluemix_notm}} Block Storage [外掛程式](cs_storage.html#install_block)，以在區塊儲存空間中儲存持續資料。然後，您可以針對叢集[建立新的](cs_storage.html#create)或[使用現有的](cs_storage.html#existing_block)區塊儲存空間。</td>
</tr>
<tr>
<td>4 月 13 日</td>
<td>[將 Cloud Foundry 應用程式移轉至叢集的新指導教學](cs_tutorials_cf.html#cf_tutorial)</td>
<td>您有 Cloud Foundry 應用程式嗎？學習如何將該應用程式中的相同程式碼部署至 Kubernetes 叢集中執行的容器。</td>
</tr>
<tr>
<td>4 月 5 日</td>
<td>[過濾日誌](cs_health.html#filter-logs)</td>
<td>過濾出特定日誌，不進行轉遞。可以針對特定的名稱空間、容器名稱、記載層次及訊息字串來濾出日誌。</td>
</tr>
</tbody></table>

## 2018 年 3 月的熱門主題
{: #mar18}

<table summary="此表格顯示熱門主題。列應該從左到右閱讀，第一欄為日期，第二欄為特性的標題，第三欄為說明。">
<caption>2018 年 3 月關於容器及 Kubernetes 叢集的熱門主題</caption>
<thead>
<th>日期</th>
<th>標題</th>
<th>說明</th>
</thead>
<tbody>
<tr>
<td>3 月 16 日</td>
<td>[使用授信運算佈建裸機叢集](cs_clusters.html#shared_dedicated_node)</td>
<td>建立執行 [Kubernetes 1.9 版](cs_versions.html#cs_v19)或更新版本的裸機叢集，並啟用「授信運算」來驗證工作者節點是否遭到竄改。</td>
</tr>
<tr>
<td>3 月 14 日</td>
<td>[安全登入 {{site.data.keyword.appid_full}}](cs_integrations.html#appid)</td>
<td>要求使用者登入，以加強在 {{site.data.keyword.containershort_notm}} 中執行的應用程式。</td>
</tr>
<tr>
<td>3 月 13 日</td>
<td>[聖保羅現在為可用位置](cs_regions.html)</td>
<td>巴西聖保羅已成為美國南部地區的新位置。如果您有防火牆，務必針對此位置以及您叢集所在地區內的其他位置[開啟防火牆埠](cs_firewall.html#firewall)。</td>
</tr>
<tr>
<td>3 月 12 日</td>
<td>[要使用「試用」帳戶加入 {{site.data.keyword.Bluemix_notm}} 嗎？歡迎試用免費的 Kubernetes 叢集！](container_index.html#clusters)</td>
<td>使用「試用」[{{site.data.keyword.Bluemix_notm}} 帳戶](https://console.bluemix.net/registration/)，您可以部署一個免費使用 21 天的叢集，來測試 Kubernetes 功能。</td>
</tr>
</tbody></table>

## 2018 年 2 月的熱門主題
{: #feb18}

<table summary="此表格顯示熱門主題。列應該從左到右閱讀，第一欄為日期，第二欄為特性的標題，第三欄為說明。">
<caption>2018 年 2 月關於容器及 Kubernetes 叢集的熱門主題</caption>
<thead>
<th>日期</th>
<th>標題</th>
<th>說明</th>
</thead>
<tbody>
<tr>
<td>2 月 27 日</td>
<td>工作者節點的硬體虛擬機器 (HVM) 映像檔</td>
<td>增加 HVM 映像檔工作負載的 I/O 效能。使用 `bx cs worker-reload` [指令](cs_cli_reference.html#cs_worker_reload)或 `bx cs worker-update` [指令](cs_cli_reference.html#cs_worker_update)，在每一個現有的工作者節點上啟動。</td>
</tr>
<tr>
<td>2 月 26 日</td>
<td>[KubeDNS 自動擴充](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/)</td>
<td>KubeDNS 現在會隨著叢集成長而擴充。您可以使用下列指令來調整擴充量：`kubectl -n kube-system edit cm kube-dns-autoscaler`。</td>
</tr>
<tr>
<td>2 月 23 日</td>
<td>檢視 Web 使用者介面中的[記載](cs_health.html#view_logs)及[度量值](cs_health.html#view_metrics)</td>
<td>使用改良的 Web 使用者介面，輕鬆檢視叢集及其元件的日誌及度量值資料。請參閱叢集詳細資料頁面來進行存取。</td>
</tr>
<tr>
<td>2 月 20 日</td>
<td>已加密的映像檔及[已簽署的信任內容](../services/Registry/registry_trusted_content.html#registry_trustedcontent)</td>
<td>在 {{site.data.keyword.registryshort_notm}} 中，您可以簽署及加密映像檔，以確保您在登錄名稱空間中儲存之映像檔的完整性。只使用信任的內容來執行您的容器實例。</td>
</tr>
<tr>
<td>2 月 19 日</td>
<td>[設定 strongSwan IPSec VPN](cs_vpn.html#vpn-setup)</td>
<td>快速部署 strongSwan IPSec VPN Helm 圖表，以將 {{site.data.keyword.containershort_notm}} 叢集安全地連接至內部部署的資料中心，而不使用 Virtual Router Appliance。</td>
</tr>
<tr>
<td>2 月 14 日</td>
<td>[首爾現在為可用位置](cs_regions.html)</td>
<td>剛好來得及趕上奧林匹克運動會，將 Kubernetes 叢集部署至位於亞太地區北部的首爾。如果您有防火牆，務必針對此位置以及您叢集所在地區內的其他位置[開啟防火牆埠](cs_firewall.html#firewall)。</td>
</tr>
<tr>
<td>2 月 8 日</td>
<td>[更新 Kubernetes 1.9](cs_versions.html#cs_v19)</td>
<td>更新至 Kubernetes 1.9 之前，請先檢閱對叢集所做的變更。</td>
</tr>
</tbody></table>

## 2018 年 1 月的熱門主題
{: #jan18}

<table summary="此表格顯示熱門主題。列應該從左到右閱讀，第一欄為日期，第二欄為特性的標題，第三欄為說明。">
<caption>2018 年 1 月關於容器及 Kubernetes 叢集的熱門主題</caption>
<thead>
<th>日期</th>
<th>標題</th>
<th>說明</th>
</thead>
<tbody>
<td>1 月 25 日</td>
<td>[現已推出廣域登錄](../services/Registry/registry_overview.html#registry_regions)</td>
<td>透過 {{site.data.keyword.registryshort_notm}}，您可以使用廣域 `registry.bluemix.net` 來取回 IBM 提供的公用映像檔。</td>
</tr>
<tr>
<td>1 月 23 日</td>
<td>[新加坡及加拿大蒙特婁現在為可用位置](cs_regions.html)</td>
<td>新加坡及蒙特婁是位於 {{site.data.keyword.containershort_notm}} 亞太地區北部及美國東部地區的可用位置。如果您有防火牆，務必針對這些位置以及您叢集所在地區內的其他位置[開啟防火牆埠](cs_firewall.html#firewall)。</td>
</tr>
<tr>
<td>1 月 8 日</td>
<td>[已加強的可用特性](cs_cli_reference.html#cs_machine_types)</td>
<td>系列 2 虛擬機型包括本端 SSD 儲存空間及磁碟加密。[將工作負載移至](cs_cluster_update.html#machine_type)這些特性，以提高效能及穩定性。</td>
</tr>
</tbody></table>

## 與 Slack 上志同道合的開發人員會談
{: #slack}

您可以看到別人談論的內容，也可以在 [{{site.data.keyword.containershort_notm}} Slack ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://ibm-container-service.slack.com) 中提出自己的問題。
{:shortdesc}

如果您的 {{site.data.keyword.Bluemix_notm}} 帳戶未使用 IBM ID，請[要求邀請](https://bxcs-slack-invite.mybluemix.net/)以加入此 Slack。
{: tip}
