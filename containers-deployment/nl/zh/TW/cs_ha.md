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




# {{site.data.keyword.containerlong_notm}} 的高可用性
{: #ha}

使用內建的 Kubernetes 及 {{site.data.keyword.containerlong}} 特性，讓叢集具有更高的可用性，並在叢集中的元件失敗時避免應用程式關閉。
{: shortdesc}

高可用性是 IT 基礎架構中的核心原則，可讓您的應用程式保持運行，即使是網站局部或全部故障。高可用性的主要目的是要消除 IT 基礎架構中的潛在故障點。例如，您可以藉由新增備援並設定失效接手機制，為系統故障做好準備。

您可以在 IT 基礎架構及叢集的不同元件內，達到不同層次的高可用性。您適合的可用性層次視多個因素而定，例如您的業務需求、您與客戶之間的「服務水準合約」，以及您要花費的金額。

## {{site.data.keyword.containerlong_notm}} 中的潛在故障點的概觀
{: #fault_domains} 

{{site.data.keyword.containerlong_notm}} 架構及基礎架構的設計旨在確保可靠性、縮短處理延遲時間及服務執行時間最大化。不過，可能會發生故障。取決於您在 {{site.data.keyword.Bluemix_notm}} 中管理的服務，您可能無法容忍故障，即使故障只持續幾分鐘而已。
{: shortdesc}

{{site.data.keyword.containershort_notm}} 提供數種方法，藉由新增備援及反親緣性，以在叢集中新增更多可用性。請檢閱下列映像檔，以瞭解潛在的故障點，以及其消除方式。

<img src="images/cs_failure_ov.png" alt="{{site.data.keyword.containershort_notm}} 地區內高可用性叢集中的錯誤網域的概觀。" width="250" style="width:250px; border-style: none"/>


<table summary="此表格顯示 {{site.data.keyword.containershort_notm}} 中的故障點。列應該從左到右閱讀，第一欄為故障點數目，第二欄為故障點的標題，第三欄為說明，第四欄為文件的鏈結。">
<caption>故障點</caption>
<col width="3%">
<col width="10%">
<col width="70%">
<col width="17%">
  <thead>
  <th>#</th>
  <th>故障點</th>
  <th>說明</th>
  <th>文件的鏈結</th>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>容器或 Pod 故障</td>
      <td>依設計，容器及 Pod 為短暫存在且可能非預期地故障。例如，如果應用程式發生錯誤，則容器或 Pod 可能會當機。若要讓應用程式具有高可用性，您必須確保具有足夠的應用程式實例能夠處理工作負載，以及有更多實例來因應故障情況。理論上，這些實例會分散在多個工作者節點之間，以在發生工作者節點故障時保護您的應用程式。</td>
      <td>[部署高可用性應用程式。](cs_app.html#highly_available_apps)</td>
  </tr>
  <tr>
    <td>2</td>
    <td>工作者節點故障</td>
    <td>工作者節點是在實體硬體上執行的 VM。工作者節點故障包括硬體中斷，例如電源、散熱或網路問題，以及 VM 本身的問題。您可以藉由在叢集中設定多個工作者節點，來說明工作者節點故障情況。<br/><br/><strong>附註：</strong>不保證其中一個位置的工作者節點位於個別的實體運算主機上。例如，一個叢集可能有 3 個工作者節點，但這 3 個工作者節點全都建立在 IBM 位置的相同實體運算主機上。如果此實體運算主機關閉，則所有工作者節點都關閉。若要避免此故障狀況，您必須在另一個位置設定第二個叢集。</td>
    <td>[建立具有多個工作者節點的叢集。](cs_cli_reference.html#cs_cluster_create)</td>
  </tr>
  <tr>
    <td>3 </td>
    <td>叢集故障</td>
    <td>Kubernetes 主節點是讓叢集保持運行的主要元件。主節點將所有叢集資料儲存在 etcd 資料庫中，作為叢集的「真理單點性 (SPOT)」。若因為網路連線失敗而無法連接主節點，或 etcd 資料庫中的資料毀損，則會發生叢集故障。您可以在一個位置建立多個叢集，以在 Kubernetes 主節點或 etcd 故障時保護您的應用程式。若要達到叢集之間的負載平衡，您必須設定外部負載平衡器。<br/><br/><strong>附註：</strong>在一個位置設定多個叢集，並不保證您的工作者節點會部署在個別的實體運算主機上。若要避免此故障狀況，您必須在另一個位置設定第二個叢集。</td>
    <td>[設定高可用性叢集。](cs_clusters.html#planning_clusters)</td>
  </tr>
  <tr>
    <td>4</td>
    <td>位置故障</td>
    <td>位置故障會影響所有實體運算主機及 NFS 儲存空間。故障包括電源、散熱、網路連線或儲存空間運作中斷，以及自然災害，例如洪水、地震和颶風等。若要避免發生位置故障，您必須讓叢集位於兩個不同位置，然後由外部負載平衡器進行負載平衡。</td>
    <td>[設定高可用性叢集。](cs_clusters.html#planning_clusters)</td>
  </tr>
  <tr>
    <td>5 </td>
    <td>地區故障</td>
    <td>每一個地區都有設定高可用性的負載平衡器，可從地區專用的 API 端點進行存取。負載平衡器會將送入及送出要求遞送至地區位置中的叢集。整個地區故障的可能性很低。不過，若要說明此故障，您可以在不同地區設定多個叢集，然後使用外部負載平衡器連接它們。假設整個地區故障，其他地區的叢集就可以接管工作負載。<br/><br/><strong>附註：</strong>多地區叢集需要數個「雲端」資源，視您的應用程式而定，它們可能既複雜又昂貴。檢查您是否需要多地區設定，或您是否可以容忍潛在的服務毀壞。如果要設定多地區叢集，請確定您的應用程式和資料可在另一個地區管理，而且您的應用程式可以處理廣域資料抄寫。</td>
    <td>[設定高可用性叢集。](cs_clusters.html#planning_clusters)</td>
  </tr>
  <tr>
    <td>6a、6b</td>
    <td>儲存空間故障</td>
    <td>在有狀態的應用程式中，資料扮演能讓您應用程式保持運行的重要角色。您要確定資料具有高可用性，以在發生潛在故障時能夠回復。在 {{site.data.keyword.containershort_notm}} 中，您可以從數個選項中選擇，以持續保存您的資料。例如，您可以使用 Kubernetes 原生持續性磁區來佈建 NFS 儲存空間，或使用 {{site.data.keyword.Bluemix_notm}} 資料庫服務來儲存資料。</td>
    <td>[規劃高可用性資料。](cs_storage.html#planning)</td>
  </tr>
  </tbody>
  </table>



