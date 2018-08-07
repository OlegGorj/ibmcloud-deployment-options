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



# 地區及位置
{{site.data.keyword.Bluemix}} 是在全球各地管理。地區是端點所存取的地理區域。位置是地區內的資料中心。{{site.data.keyword.Bluemix_notm}} 內的服務可能在全球提供，或在特定地區內提供。當您在 {{site.data.keyword.containerlong}} 中建立 Kubernetes 叢集時，其資源會保留在您已部署叢集的地區。
{:shortdesc}

[{{site.data.keyword.Bluemix_notm}} 地區](#bluemix_regions)與 [{{site.data.keyword.containershort_notm}} 地區](#container_regions)不同。

![{{site.data.keyword.containershort_notm}} 地區及位置](/images/regions.png)

_{{site.data.keyword.containershort_notm}} 地區及位置_

支援的 {{site.data.keyword.containershort_notm}} 地區如下：
  * 亞太地區北部
  * 亞太地區南部
  * 歐盟中部
  * 英國南部
  * 美國東部
  * 美國南部


## {{site.data.keyword.Bluemix_notm}} 地區 API 端點
{: #bluemix_regions}

您可以使用 {{site.data.keyword.Bluemix_notm}} 地區，跨 {{site.data.keyword.Bluemix_notm}} 服務來組織資源。例如，您可以使用儲存在相同地區的 {{site.data.keyword.registryshort_notm}} 中的專用 Docker 映像檔，來建立 Kubernetes 叢集。
{:shortdesc}

若要檢查您目前所在的 {{site.data.keyword.Bluemix_notm}} 地區，請執行 `bx info`，並檢閱**地區**欄位。

{{site.data.keyword.Bluemix_notm}} 地區可以藉由在登入時指定 API 端點來存取。如果您未指定地區，則會將您自動登入最接近的地區。

例如，您可以使用下列指令來登入 {{site.data.keyword.Bluemix_notm}} 地區 API 端點：

  * 美國南部及美國東部
      ```
      bx login -a api.ng.bluemix.net
      ```
      {: pre}

  * 雪梨及亞太地區北部
      ```
      bx login -a api.au-syd.bluemix.net
      ```
      {: pre}

  * 德國
      ```
      bx login -a api.eu-de.bluemix.net
      ```
      {: pre}

  * 英國
      ```
      bx login -a api.eu-gb.bluemix.net
      ```
      {: pre}



<br />


## {{site.data.keyword.containershort_notm}} 地區 API 端點及位置
{: #container_regions}

透過使用 {{site.data.keyword.containershort_notm}} 地區，您可以在您登入的 {{site.data.keyword.Bluemix_notm}} 地區以外的地區建立或存取 Kubernetes 叢集。{{site.data.keyword.containershort_notm}} 地區端點特指 {{site.data.keyword.containershort_notm}}，而不是 {{site.data.keyword.Bluemix_notm}} 整體。
{:shortdesc}

您可以透過一個廣域端點 `https://containers.bluemix.net/` 來存取 {{site.data.keyword.containershort_notm}}。
* 若要檢查您目前所在的 {{site.data.keyword.containershort_notm}} 地區，請執行 `bx cs region`。
* 若要擷取可用地區及其端點的清單，請執行 `bx cs regions`。

若要使用 API 搭配廣域端點，請在您的所有要求中，在 `X-Region` 標頭傳遞地區名稱。
{: tip}

### 登入不同的 {{site.data.keyword.containerlong}_notm} 地區
{: #container_login_endpoints}

您可以使用 {{site.data.keyword.containershort_notm}} CLI 來變更位置。
{:shortdesc}

您可能因為下列原因而想要登入另一個 {{site.data.keyword.containershort_notm}} 地區：
  * 您已在其中一個地區中建立 {{site.data.keyword.Bluemix_notm}} 服務或專用 Docker 映像檔，並且想要在另一個地區中將它們與 {{site.data.keyword.containershort_notm}} 搭配使用。
  * 您要在與您登入的預設 {{site.data.keyword.Bluemix_notm}} 地區不同的地區中存取叢集。

</br>

若要快速切換地區，請執行 `bx cs region-set`。

### 使用 {{site.data.keyword.containerlong_notm}} API 指令
{: #containers_api}

若要與 {{site.data.keyword.containershort_notm}} API 互動，請輸入指令類型，並在廣域端點附加 `/v1/command`。
{:shortdesc}

`GET /clusters` API 的範例：
  ```
  GET https://containers.bluemix.net/v1/clusters
  ```
  {: codeblock}

</br>

若要使用 API 搭配廣域端點，請在您的所有要求中，在 `X-Region` 標頭傳遞地區名稱。若要列出可用的地區，請執行 `bx cs regions`。
{: tip}

若要檢視 API 指令的文件，請檢視 [https://containers.bluemix.net/swagger-api/](https://containers.bluemix.net/swagger-api/)。

## {{site.data.keyword.containershort_notm}} 可用的位置
{: #locations}

位置是指 {{site.data.keyword.Bluemix_notm}} 地區內可用的實體資料中心。地區是組織位置的概念工具，可包括不同國家/地區中的位置（資料中心）。下表依地區顯示可用的位置。
{:shortdesc}

|地區|位置|城市|
|--------|----------|------|
|亞太地區北部|hkg02、seo01、sng01、tok02 |中華人民共和國香港特別行政區、首爾、新加坡、東京|
|亞太地區南部|mel01、syd01、syd04|墨爾本、雪梨|
|歐盟中部|ams03、fra02、par01        |阿姆斯特丹、法蘭克福、巴黎|
|英國南部|lon02、lon04|倫敦|
|美國東部|mon01、tor01、wdc06、wdc07|蒙特利爾、多倫多、華盛頓特區|
|美國南部|dal10、dal12、dal13、sao01|達拉斯、聖保羅|
{: caption="可用的地區及位置" caption-side="top"}

叢集的資源會保留在已部署叢集的位置（資料中心）。下列影像強調顯示叢集在美國東部地區範例內的關係：

1.  叢集的資源（包括主節點及工作者節點）位於您已部署叢集的相同位置。當您建立本端容器編排動作（例如 `kubectl` 指令）時，會在相同位置內的主節點與工作者節點之間交換資訊。

2.  如果您已設定其他叢集資源（例如，儲存空間、網路、運算或在 Pod 執行的應用程式），則資源及其資料會保留在您已部署叢集的位置。

3.  當您建立叢集管理動作（例如，使用 `bx cs` 指令）時，會將有關叢集的基本資訊（例如，名稱、ID、使用者、指令）遞送至地區端點。

![瞭解叢集資源所在的位置](/images/region-cluster-resources.png)

_瞭解叢集資源所在的位置。_



