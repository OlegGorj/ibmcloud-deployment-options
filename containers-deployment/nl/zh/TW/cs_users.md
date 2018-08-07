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



# 指派叢集存取
{: #users}

身為叢集管理者，您可以為 Kubernetes 叢集定義存取原則，以針對不同使用者建立不同的存取層次。例如，您可以授權某些使用者使用叢集資源，而其他使用者則只能部署容器。
{: shortdesc}


## 規劃存取要求
{: #planning_access}

身為叢集管理者，可能很難追蹤存取要求。建立用於存取要求的通訊型樣是維護叢集安全的必要動作。
{: shortdesc}

若要確保正確的人員具有正確的存取權，請清楚您的原則可要求存取，或取得一般作業的說明。

您可能已有一個適合您團隊的方法，這是很棒的！如果您要尋找一個開始的位置，請考量嘗試下列其中一種方法。

*  建立問題單系統
*  建立表單範本
*  建立 Wiki 頁面
*  需要電子郵件要求
*  使用您已使用的問題追蹤系統，來追蹤團隊的日常工作

感覺不知所措嗎？請試用本指導教學，其關於[組織使用者、團隊及應用程式的最佳作法](/docs/tutorials/users-teams-applications.html)。
{: tip}

## 存取原則及許可權
{: #access_policies}

存取原則的範圍是根據使用者定義的角色來決定容許他們執行的動作。您可以設定叢集、基礎架構、服務實例或 Cloud Foundry 角色所特有的原則。
{: shortdesc}

您必須針對使用 {{site.data.keyword.containershort_notm}} 的每一個使用者定義存取原則。部分原則是預先定義的，但可以自訂其他原則。請查看下列映像檔及定義，以查看哪些角色與一般使用者作業一致，並識別您可能想要自訂原則的位置。

![{{site.data.keyword.containershort_notm}} 存取角色](/images/user-policies.png)

圖. {{site.data.keyword.containershort_notm}} 存取角色

<dl>
  <dt>身分與存取管理 (IAM) 原則</dt>
    <dd><p><strong>平台</strong>：您可以決定個體可以在 {{site.data.keyword.containershort_notm}} 叢集上執行的動作。您可以依地區設定這些原則。範例動作是建立或移除叢集，或新增額外的工作者節點。這些原則必須與基礎架構原則一起設定。</p>
    <p><strong>基礎架構</strong>：您可以判斷基礎架構的存取層次，例如叢集節點機器、網路或儲存空間資源。不論使用者是從 {{site.data.keyword.containershort_notm}} GUI 或透過 CLI 提出要求，都會強制執行相同的原則，即使在 IBM Cloud 基礎架構 (SoftLayer) 中完成動作也一樣。您必須同時設定此類型的原則與 {{site.data.keyword.containershort_notm}} 平台存取原則。若要瞭解可用角色，請查看[基礎架構許可權](/docs/iam/infrastructureaccess.html#infrapermission)。</p> </br></br><strong>附註：</strong>請確定 {{site.data.keyword.Bluemix_notm}} 帳戶已[設定 IBM Cloud 基礎架構 (SoftLayer) 組合存取權](cs_troubleshoot_clusters.html#cs_credentials)，讓授權使用者可以根據指派的權限在 IBM Cloud 基礎架構 (SoftLayer) 帳戶中執行動作。</dd>
  <dt>Kubernetes 資源型存取控制 (RBAC) 角色</dt>
    <dd>每個獲指派平台存取原則的使用者都會自動獲指派 Kubernetes 角色。在 Kubernetes 中，[角色型存取控制 (RBAC) ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview) 決定使用者可對叢集內的資源執行的動作。<strong>附註</strong>：RBAC 角色會自動配置給 <code>default</code> 名稱空間，但身為叢集管理者，您可以指派其他名稱空間的角色。</dd>
  <dt>Cloud Foundry</dt>
    <dd>無法使用 Cloud IAM 來管理所有服務。如果您是使用其中一個服務，則可以繼續使用 [Cloud Foundry 使用者角色](/docs/iam/cfaccess.html#cfaccess)，控制服務的存取。</dd>
</dl>


要降級許可權嗎？可能需要幾分鐘的時間才能完成此動作。
{: tip}

### 平台角色
{: #platform_roles}

{{site.data.keyword.containershort_notm}} 會配置為使用 {{site.data.keyword.Bluemix_notm}} 平台角色。角色許可權是根據彼此來建置，這表示 `Editor` 角色具有與 `Viewer` 角色相同的許可權，加上授與編輯者的許可權。下表說明每一個角色可以執行的動作類型。

當您指派平台角色時，會將對應的 RBAC 角色自動指派給預設名稱空間。如果您變更使用者的平台角色，則也會更新 RBAC 角色。
{: tip}

<table>
<caption>平台角色及動作</caption>
  <tr>
    <th>平台角色</th>
    <th>範例動作</th>
    <th>對應 RBAC 角色</th>
  </tr>
  <tr>
      <td>檢視者</td>
      <td>檢視叢集或其他服務實例的詳細資料。</td>
      <td>檢視</td>
  </tr>
  <tr>
    <td>編輯者</td>
    <td>可以連結或取消連結 IBM 雲端服務至叢集，或建立 Webhook。<strong>附註</strong>：若要連結服務，您也必須獲指派 Cloud Foundry 開發人員角色。</td>
    <td>Edit</td>
  </tr>
  <tr>
    <td>操作員</td>
    <td>可以建立、移除、重新啟動或重新載入工作者節點。可以將子網路新增至叢集。</td>
    <td>Admin</td>
  </tr>
  <tr>
    <td>管理者</td>
    <td>可以建立及移除叢集。可以在服務及基礎架構的帳戶層次編輯其他人的存取原則。<strong>附註</strong>：透過帳戶，可以將管理者存取權指派給特定叢集或所有服務實例。若要刪除叢集，您必須具有要刪除之叢集的管理者存取權。若要建立叢集，您必須具有所有服務實例的 admin 角色。</td>
    <td>Cluster-admin</td>
  </tr>
</table>

如需在使用者介面中指派使用者角色的相關資訊，請參閱[管理 IAM 存取](/docs/iam/mngiam.html#iammanidaccser)。


### 基礎架構角色
{: #infrastructure_roles}

基礎架構角色可讓使用者在基礎架構層次的資源上執行作業。下表說明每一個角色可以執行的動作類型。基礎架構角色是可自訂的；請務必只給與使用者執行其工作所需的存取權。

除了授與特定基礎架構角色之外，您還必須將裝置存取權授與使用基礎架構的使用者。
{: tip}

<table>
<caption>基礎架構角色及動作</caption>
  <tr>
    <th>基礎架構角色</th>
    <th>範例動作</th>
  </tr>
  <tr>
    <td><i>僅限檢視</i></td>
    <td>可以檢視基礎架構詳細資料，以及查看帳戶摘要（包括發票及付款）</td>
  </tr>
  <tr>
    <td><i>基本使用者</i></td>
    <td>可以編輯服務配置（包括 IP 位址）、新增或編輯 DNS 記錄，以及新增對基礎架構具有存取權的使用者</td>
  </tr>
  <tr>
    <td><i>超級使用者</i></td>
    <td>可以執行與基礎架構相關的所有動作</td>
  </tr>
</table>

若要開始指派角色，請遵循[自訂使用者的基礎架構許可權](#infra_access)中的步驟。

### RBAC 角色
{: #rbac_roles}

資源型存取控制 (RBAC) 是一種保護叢集內資源，以及決定誰可以執行哪些 Kubernetes 動作的方式。在下表中，您可以看到 RBAC 角色類型，以及使用者可以利用該角色執行的動作類型。許可權是根據彼此來建置，這表示 `Admin` 也具有 `View` 及 `Edit` 角色隨附的所有原則。務必只給與使用者他們所需的存取權。

會自動設定 RBAC 角色與預設名稱空間的平台角色。[您可以更新角色，或指派其他名稱空間的角色](#rbac)。
{: tip}

<table>
<caption>RBAC 角色及動作</caption>
  <tr>
    <th>RBAC 角色</th>
    <th>範例動作</th>
  </tr>
  <tr>
    <td>檢視</td>
    <td>可以檢視預設名稱空間內的資源。檢視者無法檢視 Kubernetes 密碼。</td>
  </tr>
  <tr>
    <td>Edit</td>
    <td>可以讀取及寫入至預設名稱空間內的資源。</td>
  </tr>
  <tr>
    <td>Admin</td>
    <td>可以讀取及寫入至預設名稱空間內的資源，但無法寫入至名稱空間本身。可以建立名稱空間內的角色。</td>
  </tr>
  <tr>
    <td>叢集管理者</td>
    <td>可以讀取及寫入至每一個名稱空間中的資源。可以建立名稱空間內的角色。可以存取 Kubernetes 儀表板。可以建立使應用程式公開可用的 Ingress 資源。</td>
  </tr>
</table>

<br />


## 將使用者新增至 {{site.data.keyword.Bluemix_notm}} 帳戶
{: #add_users}

您可以將使用者新增至 {{site.data.keyword.Bluemix_notm}} 帳戶，以授與對叢集的存取權。
{:shortdesc}

開始之前，請驗證您已獲指派 {{site.data.keyword.Bluemix_notm}} 帳戶的 `Manager` Cloud Foundry 角色。

1.  [將使用者新增至帳戶](../iam/iamuserinv.html#iamuserinv)。
2.  在**存取**區段中，展開**服務**。
3.  將平台角色指派給使用者，以設定 {{site.data.keyword.containershort_notm}} 的存取權。
      1. 從**服務**下拉清單中，選取 **{{site.data.keyword.containershort_notm}}**。
      2. 從**地區**下拉清單中，選取要邀請使用者加入的地區。
      3. 從**服務實例**下拉清單中，選取要邀請使用者加入的叢集。若要尋找特定叢集的 ID，請執行 `bx cs clusters`。
      4. 在**選取角色**區段中，選擇角色。若要尋找每個角色的支援動作清單，請參閱[存取原則及許可權](#access_policies)。
4. [選用項目：指派 Cloud Foundry 角色](/docs/iam/mngcf.html#mngcf)。
5. [選用項目：指派基礎架構角色](/docs/iam/infrastructureaccess.html#infrapermission)。
6. 按一下**邀請使用者**。

<br />


## 瞭解 IAM API 金鑰及 `bx cs credentials-set` 指令
{: #api_key}

若要順利在帳戶中佈建及使用叢集，您必須確定已正確設定您的帳戶，可存取 IBM Cloud 基礎架構 (SoftLayer) 組合。根據您的帳戶設定，您可以使用 IAM API 金鑰或使用 `bx cs credentials-set` 指令手動設定的基礎架構認證。

<dl>
  <dt>IAM API 金鑰</dt>
    <dd><p>當執行第一個需要 {{site.data.keyword.containershort_notm}} 管理存取原則的動作時，會針對地區自動設定「身分及存取管理 (IAM)」API 金鑰。例如，其中一位管理使用者會在 <code>us-south</code> 地區中建立第一個叢集。如此一來，這位使用者的 IAM API 金鑰就會儲存在這個地區的帳戶中。API 金鑰會用來訂購 IBM Cloud 基礎架構 (SoftLayer)，例如新的工作者節點或 VLAN。</p> <p>當另一位使用者在此地區中執行需要與 IBM Cloud 基礎架構 (SoftLayer) 組合互動的動作（例如建立新叢集或重新載入工作者節點）時，會使用儲存的 API 金鑰來判斷是否有足夠的許可權可執行該動作。為了確保可以順利執行叢集中的基礎架構相關動作，請對 {{site.data.keyword.containershort_notm}} 管理使用者指派<strong>超級使用者</strong>基礎架構存取原則。</p>
    <p>您可以執行 [<code>bx cs api-key-info</code>](cs_cli_reference.html#cs_api_key_info) 來尋找現行 API 金鑰擁有者。如果您發現需要更新針對某地區所儲存的 API 金鑰，您可以執行 [<code>bx cs api-key-reset</code>](cs_cli_reference.html#cs_api_key_reset) 指令來達成此目的。此指令需要 {{site.data.keyword.containershort_notm}} 管理存取原則，它會將執行此指令的使用者的 API 金鑰儲存在帳戶中。</p>
    <p><strong>附註：</strong>如果 IBM Cloud 基礎架構 (SoftLayer) 認證是使用 <code>bx cs credentials-set</code> 指令手動設定的，則可能不會使用針對該地區所儲存的 API 金鑰。</p></dd>
  <dt>透過 <code>bx cs credentials-set</code> 設定的 IBM Cloud infrastructure (SoftLayer) 認證</dt>
    <dd><p>如果您有 {{site.data.keyword.Bluemix_notm}}「隨收隨付制」帳戶，依預設，您便可以存取 IBM Cloud 基礎架構 (SoftLayer) 組合。不過，您可能想要使用您已具有的不同 IBM Cloud 基礎架構 (SoftLayer) 帳戶來訂購基礎架構。您可以使用 [<code>bx cs credentials-set</code>](cs_cli_reference.html#cs_credentials_set) 指令，將此基礎架構帳戶鏈結至 {{site.data.keyword.Bluemix_notm}} 帳戶。</p>
    <p>如果已手動設定 IBM Cloud 基礎架構 (SoftLayer) 認證，則即使帳戶已有 IAM API 金鑰，也會使用這些認證來訂購基礎架構。如果已儲存其認證的使用者沒有訂購基礎架構的必要許可權，則基礎架構相關動作（例如建立叢集或重新載入工作者節點）可能會失敗。</p>
    <p>若要移除手動設定的 IBM Cloud 基礎架構 (SoftLayer) 認證，您可以使用 [<code>bx cs credentials-unset</code>](cs_cli_reference.html#cs_credentials_unset) 指令。移除認證之後，會使用 IAM API 金鑰來訂購基礎架構。</p></dd>
</dl>

<br />


## 自訂使用者的基礎架構許可權
{: #infra_access}

當您在「身分及存取管理」中設定基礎架構原則時，使用者會獲得與角色相關聯的許可權。若要自訂這些許可權，您必須登入 IBM Cloud 基礎架構 (SoftLayer)，並在該處調整許可權。
{: #view_access}

例如，**基本使用者**可以重新啟動工作者節點，但無法重新載入工作者節點。不必授與該人員**超級使用者**許可權，您可以調整 IBM Cloud 基礎架構 (SoftLayer) 許可權，並新增許可權來執行重新載入指令。

1.  登入 [{{site.data.keyword.Bluemix_notm}} 帳戶](https://console.bluemix.net/)，然後從功能表中選取**基礎架構**。

2.  移至**帳戶** > **使用者** > **使用者清單**。

3.  若要修改許可權，請選取使用者設定檔的名稱或**裝置存取權**直欄。

4.  在**入口網站許可權**標籤中，自訂使用者的存取權。使用者需要的許可權，取決於他們需要使用的基礎架構資源：

    * 使用**快速許可權**下拉清單來指派**超級使用者**角色，這會將所有許可權提供給使用者。
    * 使用**快速許可權**下拉清單來指派**基本使用者**角色，這會將部分（但非全部）所需許可權提供給使用者。
    * 如果您不要使用**超級使用者**角色來授與所有許可權，或需要新增**基本使用者**角色以外的許可權，請檢閱下表，其中會說明在 {{site.data.keyword.containershort_notm}} 中執行一般作業所需的許可權。

    <table summary="一般 {{site.data.keyword.containershort_notm}} 情境的基礎架構許可權。">
     <caption>{{site.data.keyword.containershort_notm}} 一般需要的基礎架構許可權</caption>
     <thead>
      <th>{{site.data.keyword.containershort_notm}} 的一般作業</th>
      <th>依標籤列出必要的基礎架構許可權</th>
     </thead>
     <tbody>
       <tr>
         <td><strong>最少許可權</strong>：<ul><li>建立叢集。</li></ul></td>
         <td><strong>裝置</strong>：<ul><li>檢視虛擬伺服器詳細資料</li><li>重新啟動伺服器並檢視 IPMI 系統資訊</li><li>發出 OS 重新載入及起始救援核心</li></ul><strong>帳戶</strong>：<ul><li>新增/升級雲端實例</li><li>新增伺服器</li></ul></td>
       </tr>
       <tr>
         <td><strong>叢集管理</strong>：<ul><li>建立、更新及刪除叢集。</li><li>新增、重新載入及重新啟動工作者節點。</li><li>檢視 VLAN。</li><li>建立子網路。</li><li>部署 Pod 及負載平衡器服務。</li></ul></td>
         <td><strong>支援</strong>：<ul><li>檢視問題單</li><li>新增問題單</li><li>編輯問題單</li></ul>
         <strong>裝置</strong>：<ul><li>檢視虛擬伺服器詳細資料</li><li>重新啟動伺服器並檢視 IPMI 系統資訊</li><li>升級伺服器</li><li>發出 OS 重新載入及起始救援核心</li></ul>
         <strong>服務</strong>：<ul><li>管理 SSH 金鑰</li></ul>
         <strong>帳戶</strong>：<ul><li>檢視帳戶摘要</li><li>新增/升級雲端實例</li><li>取消伺服器</li><li>新增伺服器</li></ul></td>
       </tr>
       <tr>
         <td><strong>儲存空間</strong>：<ul><li>建立持續性磁區宣告，以佈建持續性磁區。</li><li>建立及管理儲存空間基礎架構資源。</li></ul></td>
         <td><strong>服務</strong>：<ul><li>管理儲存空間</li></ul><strong>帳戶</strong>：<ul><li>新增儲存空間</li></ul></td>
       </tr>
       <tr>
         <td><strong>專用網路</strong>：<ul><li>管理專用 VLAN 來建立叢集內網路連線。</li><li>設定專用網路的 VPN 連線功能。</li></ul></td>
         <td><strong>網路</strong>：<ul><li>管理網路子網路路徑</li><li>管理 IPSEC 網路通道</li><li>管理網路閘道</li><li>VPN 管理</li></ul></td>
       </tr>
       <tr>
         <td><strong>公用網路</strong>：<ul><li>設定公用負載平衡器或 Ingress 網路來公開應用程式。</li></ul></td>
         <td><strong>裝置</strong>：<ul><li>管理負載平衡器</li><li>編輯主機名稱/網域</li><li>管理埠控制</li></ul>
         <strong>網路</strong>：<ul><li>新增運算與公用網路埠</li><li>管理網路子網路路徑</li><li>新增 IP 位址</li></ul>
         <strong>服務</strong>：<ul><li>管理 DNS、反向 DNS 及 WHOIS</li><li>檢視憑證 (SSL)</li><li>管理憑證 (SSL)</li></ul></td>
       </tr>
     </tbody>
    </table>

5.  若要儲存您的變更，請按一下**編輯入口網站許可權**。

6.  在**裝置存取權**標籤中，選取要授與存取權的裝置。

    * 在**裝置類型**下拉清單中，您可以授與對**所有虛擬伺服器**的存取權。
    * 若要容許使用者存取所建立的新裝置，請選取**新增裝置時自動授與存取權**。
    * 若要儲存您的變更，請按一下**更新裝置存取權**。

<br />


## 授權具有自訂 Kubernetes RBAC 角色的使用者
{: #rbac}

{{site.data.keyword.containershort_notm}} 存取原則對應於某些 Kubernetes 角色型存取控制 (RBAC) 角色，如[存取原則及許可權](#access_policies)所述。若要授權不同於對應存取原則的其他 Kubernetes 角色，您可以自訂 RBAC 角色，然後將角色指派給個人或使用者群組。
{: shortdesc}

例如，建議您授與許可權給開發人員團隊，以使用特定 API 群組或叢集中 Kubernetes 名稱空間內的資源，而不是整個叢集。您要使用對 {{site.data.keyword.containershort_notm}} 而言唯一的使用者名稱來建立角色，然後將角色連結至使用者。如需相關資訊，請參閱 Kubernetes 文件中的[使用 RBAC 授權 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview)。

開始之前，請[將 Kubernetes CLI 的目標設為叢集](cs_cli_install.html#cs_cli_configure)。

1.  建立具有您要指派之存取權的角色。

    1. 建立 `.yaml` 檔案，以定義具有您要指派之存取權的角色。

        ```
        kind: Role
        apiVersion: rbac.authorization.k8s.io/v1
        metadata:
          namespace: default
          name: my_role
        rules:
        - apiGroups: [""]
          resources: ["pods"]
          verbs: ["get", "watch", "list"]
        - apiGroups: ["apps", "extensions"]
          resources: ["daemonsets", "deployments"]
          verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
        ```
        {: codeblock}

        <table>
        <caption>瞭解這個 YAML 元件</caption>
          <thead>
            <th colspan=2><img src="images/idea.png" alt="構想圖示"/>瞭解這個 YAML 元件</th>
          </thead>
          <tbody>
            <tr>
              <td><code>kind</code></td>
              <td>使用 `Role` 來授與對單一名稱空間內的資源的存取權，或使用 `ClusterRole` 來授與對全叢集的資源的存取權。</td>
            </tr>
            <tr>
              <td><code>apiVersion</code></td>
              <td><ul><li>對於執行 Kubernetes 1.8 或更新版本的叢集，請使用 `rbac.authorization.k8s.io/v1`。</li><li>對於較舊版本，請使用 `apiVersion: rbac.authorization.k8s.io/v1beta1`。</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/namespace</code></td>
              <td><ul><li>對於 `Role` 類型：指定授與對其具有存取權的 Kubernetes 名稱空間。</li><li>如果您要建立在叢集層次上套用的 `ClusterRole`，請不要使用 `namespace` 欄位。</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/name</code></td>
              <td>命名角色，並在稍後連結該角色時使用此名稱。</td>
            </tr>
            <tr>
              <td><code>rules/apiGroups</code></td>
              <td><ul><li>指定您要使用者能夠與其互動的 Kubernetes API 群組，例如 `"apps"`、`"batch"` 或 `"extensions"`。</li><li>若要存取位於 REST 路徑 `apiv1` 的核心 API 群組，請將群組保留空白：`[""]`。</li><li>如需相關資訊，請參閱 Kubernetes 文件中的 [API 群組 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://v1-9.docs.kubernetes.io/docs/reference/api-overview/#api-groups)。</li></ul></td>
            </tr>
            <tr>
              <td><code>rules/resources</code></td>
              <td><ul><li>指定您要授與對其之存取權的 Kubernetes 資源，例如 `"daemonset"`、`"deployments"`、`"events"` 或 `"ingresses"`。</li><li>如果您指定 `"nodes"`，則角色類型必須是 `ClusterRole`。</li><li>如需資源清單，請參閱 Kubernetes 提要中的[資源類型 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/reference/kubectl/cheatsheet/) 表格。</li></ul></td>
            </tr>
            <tr>
              <td><code>rules/verbs</code></td>
              <td><ul><li>指定您要使用者能夠執行的動作類型，例如 `"get"`、`"list"`、`"describe"`、`"create"` 或 `"delete"`。</li><li>如需動詞的完整清單，請參閱 [`kubectl` 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/reference/kubectl/overview/)。</li></ul></td>
            </tr>
          </tbody>
        </table>

    2.  在叢集中建立角色。

        ```
                kubectl apply -f <path_to_yaml_file>
        ```
        {: pre}

    3.  驗證已建立角色。

        ```
                kubectl get roles
        ```
        {: pre}

2.  將使用者連結至角色。

    1. 建立 `.yaml` 檔案，將使用者連結至您的角色。請注意要用於每一個主題名稱的唯一 URL。

        ```
        kind: RoleBinding
        apiVersion: rbac.authorization.k8s.io/v1
        metadata:
          name: my_role_team1
          namespace: default
        subjects:
        - kind: User
          name: https://iam.ng.bluemix.net/kubernetes#user1@example.com
          apiGroup: rbac.authorization.k8s.io
        - kind: User
          name: https://iam.ng.bluemix.net/kubernetes#user2@example.com
          apiGroup: rbac.authorization.k8s.io
        roleRef:
          kind: Role
          name: custom-rbac-test
          apiGroup: rbac.authorization.k8s.io
        ```
        {: codeblock}

        <table>
        <caption>瞭解這個 YAML 元件</caption>
          <thead>
            <th colspan=2><img src="images/idea.png" alt="構想圖示"/>瞭解這個 YAML 元件</th>
          </thead>
          <tbody>
            <tr>
              <td><code>kind</code></td>
              <td>針對這兩種類型的角色 `.yaml` 檔案，將 `kind` 指定為 `RoleBinding`：名稱空間 `role` 及全叢集 `ClusterRole`。</td>
            </tr>
            <tr>
              <td><code>apiVersion</code></td>
              <td><ul><li>對於執行 Kubernetes 1.8 或更新版本的叢集，請使用 `rbac.authorization.k8s.io/v1`。</li><li>對於較舊版本，請使用 `apiVersion: rbac.authorization.k8s.io/v1beta1`。</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/namespace</code></td>
              <td><ul><li>對於 `Role` 類型：指定授與對其具有存取權的 Kubernetes 名稱空間。</li><li>如果您要建立在叢集層次上套用的 `ClusterRole`，請不要使用 `namespace` 欄位。</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/name</code></td>
              <td>命名角色連結。</td>
            </tr>
            <tr>
              <td><code>subjects/kind</code></td>
              <td>將類型指定為 `User`。</td>
            </tr>
            <tr>
              <td><code>subjects/name</code></td>
              <td><ul><li>將使用者的電子郵件位址附加到下列 URL 後面：`https://iam.ng.bluemix.net/kubernetes#`。</li><li>例如，`https://iam.ng.bluemix.net/kubernetes#user1@example.com`</li></ul></td>
            </tr>
            <tr>
              <td><code>subjects/apiGroup</code></td>
              <td>使用 `rbac.authorization.k8s.io`。</td>
            </tr>
            <tr>
              <td><code>roleRef/kind</code></td>
              <td>在角色 `.yaml` 檔案中輸入與 `kind` 相同的值：`Role` 或 `ClusterRole`。</td>
            </tr>
            <tr>
              <td><code>roleRef/name</code></td>
              <td>輸入角色 `.yaml` 檔案的名稱。</td>
            </tr>
            <tr>
              <td><code>roleRef/apiGroup</code></td>
              <td>使用 `rbac.authorization.k8s.io`。</td>
            </tr>
          </tbody>
        </table>

    2. 在叢集中建立角色連結資源。

        ```
                kubectl apply -f filepath/my_role_team1.yaml
        ```
        {: pre}

    3.  驗證已建立連結。

        ```
                kubectl get rolebinding
        ```
        {: pre}

既然您已建立並連結自訂 Kubernetes RBAC 角色，請對使用者採取後續行動。要求他們測試由於該角色而具有許可權可以完成的動作，例如刪除 Pod。
