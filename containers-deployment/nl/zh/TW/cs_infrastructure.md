---

copyright:
  years: 2014, 2018
lastupdated: "2018-4-20"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# 存取 IBM Cloud 基礎架構 (SoftLayer) 組合
{: #infrastructure}

若要建立標準 Kubernetes 叢集，您必須具有 IBM Cloud 基礎架構 (SoftLayer) 組合的存取權。需要有這個存取權，才能要求付費基礎架構資源，例如，{{site.data.keyword.containerlong}} 中的 Kubernetes 叢集的工作者節點、可攜式公用 IP 位址或持續性儲存空間。
{:shortdesc}

## 存取 IBM Cloud 基礎架構 (SoftLayer) 組合
{: #unify_accounts}

在啟用自動帳戶鏈結之後所建立的 {{site.data.keyword.Bluemix_notm}}「隨收隨付制」帳戶已設定為具有 IBM Cloud 基礎架構 (SoftLayer) 組合的存取權。您可以購買叢集的基礎架構資源，而不需要進行額外配置。
{:shortdesc}

具有其他 {{site.data.keyword.Bluemix_notm}} 帳戶類型的使用者，或具有未鏈結至其 {{site.data.keyword.Bluemix_notm}} 帳戶之現有 IBM Cloud 基礎架構 (SoftLayer) 帳戶的使用者，必須配置其帳戶以建立標準叢集。
{:shortdesc}

請檢閱下表，以找出每一種帳戶類型的可用選項。

|帳戶類型|說明|建立標準叢集的可用選項|
|------------|-----------|----------------------------------------------|
|精簡帳戶|精簡帳戶無法佈建叢集。|[將「精簡」帳戶升級至 {{site.data.keyword.Bluemix_notm}} 隨收隨付制帳戶](/docs/account/index.html#billableacts)，其已設定為具有 IBM Cloud 基礎架構 (SoftLayer) 組合存取權。|
|舊隨收隨付制帳戶|在自動帳戶鏈結可供使用之前所建立的「隨收隨付制」帳戶，沒有 IBM Cloud 基礎架構 (SoftLayer) 組合的存取權。<p>如果您有現有 IBM Cloud 基礎架構 (SoftLayer) 帳戶，則無法將此帳戶鏈結至舊的「隨收隨付制」帳戶。</p>|選項 1：[建立新的隨收隨付制帳戶](/docs/account/index.html#billableacts)，其已設定為具有 IBM Cloud 基礎架構 (SoftLayer) 組合存取權。當您選擇此選項時，會有兩個不同的 {{site.data.keyword.Bluemix_notm}} 帳戶及計費。<p>如果您要繼續使用舊的「隨收隨付制」帳戶來建立標準叢集，則可以使用新的「隨收隨付制」帳戶來產生可存取 IBM Cloud 基礎架構 (SoftLayer) 組合的 API 金鑰。然後，您必須設定舊「隨收隨付制」帳戶的 API 金鑰。如需相關資訊，請參閱[產生舊隨收隨付制及訂閱帳戶的 API 金鑰](#old_account)。請記住，IBM Cloud 基礎架構 (SoftLayer) 資源是透過新的「隨收隨付制」帳戶計費。</p></br><p>選項 2：如果您已有想要使用的現有 IBM Cloud 基礎架構 (SoftLayer) 帳戶，則可以針對 {{site.data.keyword.Bluemix_notm}} 帳戶[設定認證](cs_cli_reference.html#cs_credentials_set)。</p><p>**附註：**與 {{site.data.keyword.Bluemix_notm}} 帳戶搭配使用的 IBM Cloud 基礎架構 (SoftLayer) 帳戶必須已設定「超級使用者」許可權。</p>|
|訂閱帳戶|訂閱帳戶未設定 IBM Cloud 基礎架構 (SoftLayer) 組合存取權。|選項 1：[建立新的隨收隨付制帳戶](/docs/account/index.html#billableacts)，其已設定為具有 IBM Cloud 基礎架構 (SoftLayer) 組合存取權。當您選擇此選項時，會有兩個不同的 {{site.data.keyword.Bluemix_notm}} 帳戶及計費。<p>如果您要繼續使用「訂閱」帳戶來建立標準叢集，則可以使用新的「隨收隨付制」帳戶來產生可存取 IBM Cloud 基礎架構 (SoftLayer) 組合的 API 金鑰。然後，您必須設定「訂閱」帳戶的 API 金鑰。如需相關資訊，請參閱[產生舊隨收隨付制及訂閱帳戶的 API 金鑰](#old_account)。請記住，IBM Cloud 基礎架構 (SoftLayer) 資源是透過新的「隨收隨付制」帳戶計費。</p></br><p>選項 2：如果您已有想要使用的現有 IBM Cloud 基礎架構 (SoftLayer) 帳戶，則可以針對 {{site.data.keyword.Bluemix_notm}} 帳戶[設定認證](cs_cli_reference.html#cs_credentials_set)。<p>**附註：**與 {{site.data.keyword.Bluemix_notm}} 帳戶搭配使用的 IBM Cloud 基礎架構 (SoftLayer) 帳戶必須已設定「超級使用者」許可權。</p>|
|IBM Cloud 基礎架構 (SoftLayer)，無 {{site.data.keyword.Bluemix_notm}} 帳戶|若要建立標準叢集，您必須有 {{site.data.keyword.Bluemix_notm}} 帳戶。|<p>[建立新的隨收隨付制帳戶](/docs/account/index.html#billableacts)，其已設定為具有 IBM Cloud 基礎架構 (SoftLayer) 組合存取權。當選擇此選項時，會為您建立 IBM Cloud 基礎架構 (SoftLayer)。您有兩個不同的 IBM Cloud 基礎架構 (SoftLayer) 帳戶和帳單。</p>|

<br />


## 產生 IBM Cloud 基礎架構 (SoftLayer) API 金鑰，以與 {{site.data.keyword.Bluemix_notm}} 帳戶搭配使用
{: #old_account}

若要繼續使用舊的「隨收隨付制」或「訂閱」帳戶來建立標準叢集，請使用新的「隨收隨付制」帳戶來產生 API 金鑰，並設定舊帳戶的 API 金鑰。
{:shortdesc}

開始之前，請建立已自動設定 IBM Cloud 基礎架構 (SoftLayer) 組合存取權的 {{site.data.keyword.Bluemix_notm}}「隨收隨付制」帳戶。

1.  使用針對新的「隨收隨付制」帳戶所建立的 {{site.data.keyword.ibmid}} 及密碼，登入 [IBM Cloud 基礎架構 (SoftLayer) 入口網站 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/)。
2.  選取**帳戶**，然後選取**使用者**。
3.  按一下**產生**，為新的「隨收隨付制」帳戶產生 IBM Cloud 基礎架構 (SoftLayer) API 金鑰。
4.  複製 API 金鑰。
5.  從 CLI 中，使用舊「隨收隨付制」或「訂閱」帳戶的 {{site.data.keyword.ibmid}} 及密碼來登入 {{site.data.keyword.Bluemix_notm}}。

  ```
  bx login
  ```
  {: pre}

6.  設定先前產生以存取 IBM Cloud 基礎架構 (SoftLayer) 組合的 API 金鑰。請將 `<API_key>` 取代為 API 金鑰，並將 `<username>` 取代為新「隨收隨付制」帳戶的 {{site.data.keyword.ibmid}}。

  ```
  bx cs credentials-set --infrastructure-api-key <API_key> --infrastructure-username <username>
  ```
  {: pre}

7.  開始[建立標準叢集](cs_clusters.html#clusters_cli)。

**附註：**若要在產生 API 金鑰之後進行檢閱，請遵循步驟 1 及 2，然後按一下 **API 金鑰**區段中的**檢視**，以查看使用者 ID 的 API 金鑰。

