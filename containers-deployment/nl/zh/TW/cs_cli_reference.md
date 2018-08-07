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





# {{site.data.keyword.containerlong_notm}} CLI 參考資料
{: #cs_cli_reference}

請參閱這些指令，以在 {{site.data.keyword.containerlong}} 中建立及管理 Kubernetes 叢集。
{:shortdesc}

若要安裝 CLI 外掛程式，請參閱[安裝 CLI](cs_cli_install.html#cs_cli_install_steps)。

要尋找 `bx cr` 指令嗎？請參閱 [{{site.data.keyword.registryshort_notm}} CLI 參考資料](/docs/cli/plugins/registry/index.html)。要尋找 `kubectl` 指令嗎？請參閱 [Kubernetes 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/reference/kubectl/overview/)。
{:tip}

## bx cs 指令
{: #cs_commands}

**提示：**若要查看 {{site.data.keyword.containershort_notm}} 外掛程式的版本，請執行下列指令。

```
bx plugin list
```
{: pre}



<table summary="API 指令表格">
<caption>API 指令</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>API 指令</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs api](#cs_api)</td>
    <td>[bx cs api-key-info](#cs_api_key_info)</td>
    <td>[bx cs api-key-reset](#cs_api_key_reset)</td>
    <td>[bx cs apiserver-config-get](#cs_apiserver_config_get)</td>
  </tr>
  <tr>
    <td>[bx cs apiserver-config-set](#cs_apiserver_config_set)</td>
    <td>[bx cs apiserver-config-unset](#cs_apiserver_config_unset)</td>
    <td>[bx cs apiserver-refresh](#cs_apiserver_refresh)</td>
    <td></td>
 </tr>
</tbody>
</table>

<br>

<table summary="CLI 外掛程式用法指令表格">
<caption>CLI 外掛程式用法指令</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>CLI 外掛程式用法指令</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs help](#cs_help)</td>
    <td>[bx cs init](#cs_init)</td>
    <td>[bx cs messages](#cs_messages)</td>
    <td></td>
  </tr>
</tbody>
</table>

<br>

<table summary="叢集指令：管理表格">
<caption>叢集指令：管理指令</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>叢集指令：管理</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs cluster-config](#cs_cluster_config)</td>
    <td>[bx cs cluster-create](#cs_cluster_create)</td>
    <td>[bx cs cluster-feature-enable](#cs_cluster_feature_enable)</td>
    <td>[bx cs cluster-get](#cs_cluster_get)</td>
  </tr>
  <tr>
    <td>[bx cs cluster-rm](#cs_cluster_rm)</td>
    <td>[bx cs cluster-update](#cs_cluster_update)</td>
    <td>[bx cs clusters](#cs_clusters)</td>
    <td>[bx cs kube-versions](#cs_kube_versions)</td>
  </tr>
</tbody>
</table>

<br>

<table summary="叢集指令：服務與整合表格">
<caption>叢集指令：服務與整合指令</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>叢集指令：服務與整合</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs cluster-service-bind](#cs_cluster_service_bind)</td>
    <td>[bx cs cluster-service-unbind](#cs_cluster_service_unbind)</td>
    <td>[bx cs cluster-services](#cs_cluster_services)</td>
    <td>[bx cs webhook-create](#cs_webhook_create)</td>
  </tr>
</tbody>
</table>

</br>

<table summary="叢集指令：子網路表格">
<caption>叢集指令：子網路指令</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>叢集指令：子網路</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs cluster-subnet-add](#cs_cluster_subnet_add)</td>
    <td>[bx cs cluster-subnet-create](#cs_cluster_subnet_create)</td>
    <td>[bx cs cluster-user-subnet-add](#cs_cluster_user_subnet_add)</td>
    <td>[bx cs cluster-user-subnet-rm](#cs_cluster_user_subnet_rm)</td>
  </tr>
  <tr>
    <td>[bx cs subnets](#cs_subnets)</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</tbody>
</table>

</br>

<table summary="基礎架構指令表格">
<caption>叢集指令：基礎架構指令</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>基礎架構指令</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs credentials-set](#cs_credentials_set)</td>
    <td>[bx cs credentials-unset](#cs_credentials_unset)</td>
    <td>[bx cs machine-types](#cs_machine_types)</td>
    <td>[bx cs vlans](#cs_vlans)</td>
  </tr>
</tbody>
</table>

</br>

<table summary="Ingress 應用程式負載平衡器 (ALB) 指令表格">
<caption>Ingress 應用程式負載平衡器 (ALB) 指令</caption>
<col width = 25%>
<col width = 25%>
<col width = 25%>
  <thead>
    <tr>
      <th colspan=4>Ingress 應用程式負載平衡器 (ALB) 指令</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>[bx cs alb-cert-deploy](#cs_alb_cert_deploy)</td>
      <td>[bx cs alb-cert-get](#cs_alb_cert_get)</td>
      <td>[bx cs alb-cert-rm](#cs_alb_cert_rm)</td>
      <td>[bx cs alb-certs](#cs_alb_certs)</td>
    </tr>
    <tr>
      <td>[bx cs alb-configure](#cs_alb_configure)</td>
      <td>[bx cs alb-get](#cs_alb_get)</td>
      <td>[bx cs alb-types](#cs_alb_types)</td>
      <td>[bx cs albs](#cs_albs)</td>
    </tr>
  </tbody>
</table>

</br>

<table summary="記載指令表格">
<caption>記載指令</caption>
<col width = 25%>
<col width = 25%>
<col width = 25%>
  <thead>
    <tr>
      <th colspan=4>記載指令</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>[bx cs logging-config-create](#cs_logging_create)</td>
      <td>[bx cs logging-config-get](#cs_logging_get)</td>
      <td>[bx cs logging-config-refresh](#cs_logging_refresh)</td>
      <td>[bx cs logging-config-rm](#cs_logging_rm)</td>
    </tr>
    <tr>
      <td>[bx cs logging-config-update](#cs_logging_update)</td>
      <td>[bx cs logging-filter-create](#cs_log_filter_create)</td>
      <td>[bx cs logging-filter-update](#cs_log_filter_update)</td>
      <td>[bx cs logging-filter-get](#cs_log_filter_view)</td>
    </tr>
    <tr>
      <td>[bx cs logging-filter-rm](#cs_log_filter_delete)</td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>

</br>

<table summary="地區指令表格">
<caption>地區指令</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>地區指令</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs locations](#cs_datacenters)</td>
    <td>[bx cs region](#cs_region)</td>
    <td>[bx cs region-set](#cs_region-set)</td>
    <td>[bx cs regions](#cs_regions)</td>
  </tr>
</tbody>
</table>

</br>

<table summary="工作者節點指令表格">
<caption>工作者節點指令</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>工作者節點指令</th>
 </thead>
 <tbody>
    <tr>
      <td>[bx cs worker-add](#cs_worker_add)</td>
      <td>[bx cs worker-get](#cs_worker_get)</td>
      <td>[bx cs worker-reboot](#cs_worker_reboot)</td>
      <td>[bx cs worker-reload](#cs_worker_reload)</td></staging>
    </tr>
    <tr>
      <td>[bx cs worker-rm](#cs_worker_rm)</td>
      <td>[bx cs worker-update](#cs_worker_update)</td>
      <td>[bx cs workers](#cs_workers)</td>
      <td></td>
    </tr>
  </tbody>
</table>

## API 指令
{: #api_commands}

### bx cs api ENDPOINT [--insecure][--skip-ssl-validation] [--api-version VALUE][-s]
{: #cs_api}

將目標設為 {{site.data.keyword.containershort_notm}} 的 API 端點。如果您未指定端點，則可以檢視已設定目標之現行端點的相關資訊。

要切換地區嗎？請改用 `bx cs region-set` [指令](#cs_region-set)。
{: tip}

<strong>指令選項</strong>：

   <dl>
   <dt><code><em>ENDPOINT</em></code></dt>
   <dd>{{site.data.keyword.containershort_notm}} API 端點。請注意，此端點與 {{site.data.keyword.Bluemix_notm}} 端點不同。設定 API 端點需要此值。接受值如下：<ul>
   <li>全球端點：https://containers.bluemix.net</li>
   <li>亞太地區北部端點：https://ap-north.containers.bluemix.net</li>
   <li>亞太地區南部端點：https://ap-south.containers.bluemix.net</li>
   <li>歐盟中部端點：https://eu-central.containers.bluemix.net</li>
   <li>英國南部端點：https://uk-south.containers.bluemix.net</li>
   <li>美國東部端點：https://us-east.containers.bluemix.net</li>
   <li>美國南部端點：https://us-south.containers.bluemix.net</li></ul>
   </dd>

   <dt><code>--insecure</code></dt>
   <dd>容許不安全的 HTTP 連線。這是選用旗標。</dd>

   <dt><code>--skip-ssl-validation</code></dt>
   <dd>容許不安全的 SSL 憑證。這是選用旗標。</dd>

   <dt><code>--api-version VALUE</code></dt>
   <dd>指定您要使用之服務的 API 版本。這是選用值。</dd>

   <dt><code>-s</code></dt>
   <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>

   </dl>

**範例**：檢視已設定目標之現行 API 端點的相關資訊。
```
bx cs api
```
{: pre}

```
API Endpoint:          https://containers.bluemix.net   
API Version:           v1   
Skip SSL Validation:   false   
Region:                us-south
```
{: screen}


### bx cs api-key-info CLUSTER [--json][-s]
{: #cs_api_key_info}

檢視 {{site.data.keyword.containershort_notm}} 地區中 IAM API 金鑰的擁有者的名稱及電子郵件位址。

當執行第一個需要 {{site.data.keyword.containershort_notm}} 管理存取原則的動作時，會針對地區自動設定「身分及存取管理 (IAM)」API 金鑰。例如，其中一位管理使用者會在 `us-south` 地區中建立第一個叢集。如此一來，這位使用者的 IAM API 金鑰就會儲存在這個地區的帳戶中。此 API 金鑰是用來訂購 IBM Cloud 基礎架構 (SoftLayer) 中的資源，例如新的工作者節點或 VLAN。

當另一位使用者在此地區中執行需要與 IBM Cloud 基礎架構 (SoftLayer) 組合互動的動作（例如建立新叢集或重新載入工作者節點）時，會使用儲存的 API 金鑰來判斷是否有足夠的許可權可執行該動作。若要確保可以順利執行叢集中的基礎架構相關動作，請對 {{site.data.keyword.containershort_notm}} 管理使用者指派**超級使用者**基礎架構存取原則。如需相關資訊，請參閱[管理使用者存取](cs_users.html#infra_access)。

如果您發現需要更新針對某地區所儲存的 API 金鑰，您可以執行 [bx cs api-key-reset](#cs_api_key_reset) 指令來達成此目的。此指令需要 {{site.data.keyword.containershort_notm}} 管理存取原則，它會將執行此指令的使用者的 API 金鑰儲存在帳戶中。

**提示：**如果 IBM Cloud 基礎架構 (SoftLayer) 認證是使用 [bx cs credentials-set](#cs_credentials_set) 指令手動設定，則可能無法使用此指令所傳回的 API 金鑰。

<strong>指令選項</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>叢集的名稱或 ID。這是必要值。</dd>

   <dt><code>--json</code></dt>
   <dd>以 JSON 格式列印指令輸出。這是選用值。</dd>

   <dt><code>-s</code></dt>
   <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>

   </dl>

**範例**：

  ```
  bx cs api-key-info my_cluster
  ```
  {: pre}


### bx cs api-key-reset [-s]
{: #cs_api_key_reset}

取代 {{site.data.keyword.containershort_notm}} 地區中的現行 IAM API 金鑰。

此指令需要 {{site.data.keyword.containershort_notm}} 管理存取原則，它會將執行此指令的使用者的 API 金鑰儲存在帳戶中。需要有 IAM API 金鑰，才能訂購 IBM Cloud 基礎架構 (SoftLayer) 組合中的基礎架構。儲存之後，API 金鑰即會用於地區中的每個動作，這些動作需要基礎架構許可權（與執行此指令的使用者無關）。如需 IAM API 金鑰運作方式的相關資訊，請參閱 [`bx cs api-key-info` 指令](#cs_api_key_info)。

**重要事項**：在使用此指令之前，請確定執行此指令的使用者具有必要的 [{{site.data.keyword.containershort_notm}} 及 IBM Cloud 基礎架構 (SoftLayer) 許可權](cs_users.html#users)。

<strong>指令選項</strong>：

   <dl>
   <dt><code>-s</code></dt>
   <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
   </dl>


**範例**：

  ```
  bx cs api-key-reset
  ```
  {: pre}


### bx cs apiserver-config-get
{: #cs_apiserver_config_get}

取得叢集 Kubernetes API 伺服器配置之選項的相關資訊。此指令必須與您要取得其相關資訊之配置選項的下列其中一個次指令一起使用。

#### bx cs apiserver-config-get audit-webhook CLUSTER
{: #cs_apiserver_api_webhook_get}

檢視您要將 API 伺服器審核日誌傳送至其中的遠端記載服務的 URL。URL 是在建立 API 伺服器配置的 Webhook 後端時指定。

<strong>指令選項</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>叢集的名稱或 ID。這是必要值。</dd>
   </dl>

**範例**：

  ```
  bx cs apiserver-config-get audit-webhook my_cluster
  ```
  {: pre}

### bx cs apiserver-config-set
{: #cs_apiserver_config_set}

設定叢集 Kubernetes API 伺服器配置的選項。此指令必須與您要設定之配置選項的下列其中一個次指令一起使用。

#### bx cs apiserver-config-set audit-webhook CLUSTER [--remoteServer SERVER_URL_OR_IP][--caCert CA_CERT_PATH] [--clientCert CLIENT_CERT_PATH][--clientKey CLIENT_KEY_PATH]
{: #cs_apiserver_api_webhook_set}

設定 API 伺服器配置的 Webhook 後端。Webhook 後端會將 API 伺服器審核日誌轉遞至遠端伺服器。將根據您在此指令旗標中提供的資訊，來建立 Webhook 配置。如果您未在旗標中提供任何資訊，則會使用預設 Webhook 配置。

<strong>指令選項</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>叢集的名稱或 ID。這是必要值。</dd>

   <dt><code>--remoteServer <em>SERVER_URL</em></code></dt>
   <dd>您要將審核日誌傳送至其中的遠端記載服務的 URL 或 IP 位址。如果您提供不安全的伺服器 URL，則會忽略任何憑證。這是選用值。</dd>

   <dt><code>--caCert <em>CA_CERT_PATH</em></code></dt>
   <dd>用來驗證遠端記載服務之 CA 憑證的檔案路徑。這是選用值。</dd>

   <dt><code>--clientCert <em>CLIENT_CERT_PATH</em></code></dt>
   <dd>用來對遠端記載服務進行鑑別之用戶端憑證的檔案路徑。這是選用值。</dd>

   <dt><code>--clientKey <em> CLIENT_KEY_PATH</em></code></dt>
   <dd>用來連接至遠端記載服務之對應用戶端金鑰的檔案路徑。這是選用值。</dd>
   </dl>

**範例**：

  ```
  bx cs apiserver-config-set audit-webhook my_cluster --remoteServer https://audit.example.com/audit --caCert /mnt/etc/kubernetes/apiserver-audit/ca.pem --clientCert /mnt/etc/kubernetes/apiserver-audit/cert.pem --clientKey /mnt/etc/kubernetes/apiserver-audit/key.pem
  ```
  {: pre}


### bx cs apiserver-config-unset
{: #cs_apiserver_config_unset}

停用叢集 Kubernetes API 伺服器配置的選項。此指令必須與您要取消設定之配置選項的下列其中一個次指令一起使用。

#### bx cs apiserver-config-unset audit-webhook CLUSTER
{: #cs_apiserver_api_webhook_unset}

停用叢集 API 伺服器的 Webhook 後端配置。停用 Webhook 後端會停止將 API 伺服器審核日誌轉遞至遠端伺服器。

<strong>指令選項</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>叢集的名稱或 ID。這是必要值。</dd>
   </dl>

**範例**：

  ```
  bx cs apiserver-config-unset audit-webhook my_cluster
  ```
  {: pre}

### bx cs apiserver-refresh CLUSTER [-s]
{: #cs_apiserver_refresh}

重新啟動叢集中的 Kubernetes 主節點，以將變更套用至 API 伺服器配置。

<strong>指令選項</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>叢集的名稱或 ID。這是必要值。</dd>

   <dt><code>-s</code></dt>
   <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>

   </dl>

**範例**：

  ```
  bx cs apiserver-refresh my_cluster
  ```
  {: pre}


<br />


## CLI 外掛程式用法指令
{: #cli_plug-in_commands}

### bx cs help
{: #cs_help}

檢視所支援指令及參數的清單。

<strong>指令選項</strong>：

   無

**範例**：

  ```
  bx cs help
  ```
  {: pre}


### bx cs init [--host HOST][--insecure] [-p][-u] [-s]
{: #cs_init}

起始設定 {{site.data.keyword.containershort_notm}} 外掛程式，或指定您要建立或存取 Kubernetes 叢集的地區。

<strong>指令選項</strong>：

   <dl>
   <dt><code>--host <em>HOST</em></code></dt>
   <dd>要使用的 {{site.data.keyword.containershort_notm}} API 端點。這是選用值。[檢視可用的 API 端點值。](cs_regions.html#container_regions)</dd>

   <dt><code>--insecure</code></dt>
   <dd>容許不安全的 HTTP 連線。</dd>

   <dt><code>-p</code></dt>
   <dd>您的 IBM Cloud 密碼。</dd>

   <dt><code>-u</code></dt>
   <dd>您的 IBM Cloud 使用者名稱。</dd>

   <dt><code>-s</code></dt>
   <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>

   </dl>

**範例**：


```
    bx cs init --host https://uk-south.containers.bluemix.net
    ```
{: pre}


### bx cs messages
{: #cs_messages}

檢視 IBMid 使用者的現行訊息。

**範例**：

```
bx cs messages
```
{: pre}


<br />


## 叢集指令：管理
{: #cluster_mgmt_commands}


### bx cs cluster-config CLUSTER [--admin][--export] [-s][--yaml]
{: #cs_cluster_config}

登入之後，請下載 Kubernetes 配置資料及憑證來連接至叢集，以及執行 `kubectl` 指令。檔案會下載至 `user_home_directory/.bluemix/plugins/container-service/clusters/<cluster_name>`。

**指令選項**：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>叢集的名稱或 ID。這是必要值。</dd>

   <dt><code>--admin</code></dt>
   <dd>下載「超級使用者」角色的 TLS 憑證及許可權檔案。您可以使用憑證，在叢集中自動執行作業，而無需重新鑑別。檔案會下載至 `<user_home_directory>/.bluemix/plugins/container-service/clusters/<cluster_name>-admin`。這是選用值。</dd>

   <dt><code>--export</code></dt>
   <dd>下載 Kubernets 配置資料及憑證，但不包含 export 指令以外的任何訊息。因為沒有顯示任何訊息，所以可以在建立自動化 Script 時使用此旗標。這是選用值。</dd>

  <dt><code>-s</code></dt>
  <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>

  <dt><code>--yaml</code></dt>
  <dd>以 YAML 格式列印指令輸出。這是選用值。</dd>

   </dl>

**範例**：

```
bx cs cluster-config my_cluster
```
{: pre}


### bx cs cluster-create [--file FILE_LOCATION][--hardware HARDWARE] --location LOCATION --machine-type MACHINE_TYPE --name NAME [--kube-version MAJOR.MINOR.PATCH][--no-subnet] [--private-vlan PRIVATE_VLAN][--public-vlan PUBLIC_VLAN] [--workers WORKER][--disable-disk-encrypt] [--trusted][-s]
{: #cs_cluster_create}

在您的組織中建立叢集。對於免費叢集，您只要指定叢集名稱；其他所有項目都設為預設值。在 21 天後會自動刪除可用的叢集。您一次可以有一個免費叢集。若要充分運用 Kubernetes 的完整功能，請建立標準叢集。

<strong>指令選項</strong>

<dl>
<dt><code>--file <em>FILE_LOCATION</em></code></dt>

<dd>建立標準叢集之 YAML 檔案的路徑。您可以使用 YAML 檔案，而不是使用此指令中所提供的選項來定義叢集的特徵。此值對於標準叢集是選用的，不適用於免費叢集。

<p><strong>附註：</strong>如果您在指令中提供與 YAML 檔案中的參數相同的選項，則指令中的值優先順序會高於 YAML 中的值。例如，您在 YAML 檔案中定義了位置，並在指令中使用 <code>--location</code> 選項，則您在指令選項中輸入的值會置換 YAML 檔案中的值。

<pre class="codeblock">
<code>name: <em>&lt;cluster_name&gt;</em>
location: <em>&lt;location&gt;</em>
no-subnet: <em>&lt;no-subnet&gt;</em>
machine-type: <em>&lt;machine_type&gt;</em>
private-vlan: <em>&lt;private_VLAN&gt;</em>
public-vlan: <em>&lt;public_VLAN&gt;</em>
hardware: <em>&lt;shared_or_dedicated&gt;</em>
workerNum: <em>&lt;number_workers&gt;</em>
kube-version: <em>&lt;kube-version&gt;</em>
diskEncryption: <em>false</em>
trusted: <em>true</em>
</code></pre>


<table>
    <caption>瞭解 YAML 檔案元件</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解 YAML 檔案元件</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>name</em></code></td>
    <td>將 <code><em>&lt;cluster_name&gt;</em></code> 取代為叢集的名稱。此名稱必須以字母開頭，可以包含字母、數字及連字號 (-)，且長度不得超過 35 個字元。叢集名稱和叢集部署所在的地區會形成 Ingress 子網域的完整網域名稱。為了確保 Ingress 子網域在地區內是唯一的，叢集名稱可能會被截斷，並在 Ingress 網域名稱內附加一個隨機值。
</td>
    </tr>
    <tr>
    <td><code><em>location</em></code></td>
    <td>將 <code><em>&lt;location&gt;</em></code> 取代為您要建立叢集的位置。可用的位置取決於您所登入的地區。若要列出可用的位置，請執行 <code>bx cs locations</code>。</td>
     </tr>
     <tr>
     <td><code><em>no-subnet</em></code></td>
     <td>依預設，公用及專用可攜式子網路都是建立在與叢集相關聯的 VLAN 上。將 <code><em>&lt;no-subnet&gt;</em></code> 取代為 <code><em>true</em></code>，以避免建立具有叢集的子網路。稍後，您可以[建立](#cs_cluster_subnet_create)或[新增](#cs_cluster_subnet_add)子網路至叢集。</td>
      </tr>
     <tr>
     <td><code><em>machine-type</em></code></td>
     <td>將 <code><em>&lt;machine_type&gt;</em></code> 取代為您要將工作者節點部署至其中的機型。您可以將工作者節點部署為共用或專用硬體上的虛擬機器，或部署為裸機上的實體機器。可用的實體及虛擬機器類型會因您部署叢集所在的位置而不同。如需相關資訊，請參閱 `bx cs machine-type` [指令](cs_cli_reference.html#cs_machine_types) 的文件。</td>
     </tr>
     <tr>
     <td><code><em>private-vlan</em></code></td>
     <td>將 <code><em>&lt;private_VLAN&gt;</em></code> 取代為您要用於工作者節點的專用 VLAN ID。若要列出可用的 VLAN，請執行 <code>bx cs vlans <em>&lt;location&gt;</em></code>，並尋找開頭為 <code>bcr</code>（後端路由器）的 VLAN 路由器。</td>
     </tr>
     <tr>
     <td><code><em>public-vlan</em></code></td>
     <td>將 <code><em>&lt;public_VLAN&gt;</em></code> 取代為您要用於工作者節點的公用 VLAN ID。若要列出可用的 VLAN，請執行 <code>bx cs vlans <em>&lt;location&gt;</em></code>，並尋找開頭為 <code>fcr</code>（前端路由器）的 VLAN 路由器。</td>
     </tr>
     <tr>
     <td><code><em>hardware</em></code></td>
     <td>若為虛擬機器類型：您的工作者節點的硬體隔離層次。如果您希望可用的實體資源只供您專用，請使用 dedicated，或者，若要容許與其他 IBM 客戶共用實體資源，請使用 shared。預設值為 <code>shared</code>。</td>
     </tr>
     <tr>
     <td><code><em>workerNum</em></code></td>
     <td>將 <code><em>&lt;number_workers&gt;</em></code> 取代為您要部署的工作者節點數目。</td>
     </tr>
     <tr>
      <td><code><em>kube-version</em></code></td>
      <td>叢集主節點的 Kubernetes 版本。這是選用值。未指定版本時，會使用支援的 Kubernetes 版本的預設值來建立叢集。若要查看可用的版本，請執行 <code>bx cs kube-versions</code>。</td></tr>
      <tr>
      <td><code>diskEncryption: <em>false</em></code></td>
      <td>依預設，工作者節點會具備磁碟加密；[進一步瞭解](cs_secure.html#worker)。若要停用加密，請包含此選項，並將值設為 <code>false</code>。</td></tr>
      <tr>
      <td><code>trusted: <em>true</em></code></td>
      <td>**僅限裸機**：啟用[授信運算](cs_secure.html#trusted_compute)，以驗證裸機工作者節點是否遭到竄改。如果您在叢集建立期間未啟用信任，但後來想要啟用，您可以使用 `bx cs feature-enable` [指令](cs_cli_reference.html#cs_cluster_feature_enable)。啟用信任之後，以後您就無法再予以停用。</td></tr>
     </tbody></table>
    </p></dd>

<dt><code>--hardware <em>HARDWARE</em></code></dt>
<dd>工作者節點的硬體隔離層次。若要讓可用的實體資源只供您專用，請使用 dedicated，或者，若要容許與其他 IBM 客戶共用實體資源，請使用 shared。預設值為 shared。此值對於標準叢集是選用的，不適用於免費叢集。</dd>

<dt><code>--location <em>LOCATION</em></code></dt>
<dd>您要建立叢集的位置。可用的位置取決於您所登入的 {{site.data.keyword.Bluemix_notm}} 地區。選取實際上與您最接近的地區以獲得最佳效能。此值對於標準叢集是必要的，對於免費叢集則是選用性。

<p>檢閱[可用位置](cs_regions.html#locations)。</p>

<p><strong>附註：</strong>若您選取的位置不在您的國家/地區境內，請謹記，您可能需要合法授權，才能實際將資料儲存在國外。</p>
</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>選擇機型。您可以將工作者節點部署為共用或專用硬體上的虛擬機器，或部署為裸機上的實體機器。可用的實體及虛擬機器類型會因您部署叢集所在的位置而不同。如需相關資訊，請參閱 `bx cs machine-types` [指令](cs_cli_reference.html#cs_machine_types) 的文件。此值對於標準叢集是必要的，不適用於免費叢集。</dd>

<dt><code>--name <em>NAME</em></code></dt>
<dd>叢集的名稱。這是必要值。此名稱必須以字母開頭，可以包含字母、數字及連字號 (-)，且長度不得超過 35 個字元。叢集名稱和叢集部署所在的地區會形成 Ingress 子網域的完整網域名稱。為了確保 Ingress 子網域在地區內是唯一的，叢集名稱可能會被截斷，並在 Ingress 網域名稱內附加一個隨機值。
</dd>

<dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
<dd>叢集主節點的 Kubernetes 版本。這是選用值。未指定版本時，會使用支援的 Kubernetes 版本的預設值來建立叢集。若要查看可用的版本，請執行 <code>bx cs kube-versions</code>。</dd>

<dt><code>--no-subnet</code></dt>
<dd>依預設，公用及專用可攜式子網路都是建立在與叢集相關聯的 VLAN 上。請包括 <code>--no-subnet</code> 旗標，以避免建立具有叢集的子網路。稍後，您可以[建立](#cs_cluster_subnet_create)或[新增](#cs_cluster_subnet_add)子網路至叢集。</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>

<ul>
<li>此參數不適用於免費叢集。</li>
<li>如果此標準叢集是您在這個位置所建立的第一個標準叢集，請不要包括此旗標。建立叢集時，會自動建立一個專用 VLAN。</li>
<li>如果您之前已在此位置中建立標準叢集，或之前已在 IBM Cloud 基礎架構 (SoftLayer) 中建立專用 VLAN，則必須指定該專用 VLAN。



<p><strong>附註：</strong>專用 VLAN 路由器的開頭一律為 <code>bcr</code>（後端路由器），而公用 VLAN 路由器的開頭一律為 <code>fcr</code>（前端路由器）。當建立叢集並指定公用和專用 VLAN 時，那些字首之後的數字和字母組合必須相符。</p></li>
</ul>

<p>若要找出您是否已有特定位置的專用 VLAN，或尋找現有專用 VLAN 的名稱，請執行 <code>bx cs vlans <em>&lt;location&gt;</em></code>。</p></dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>
<ul>
<li>此參數不適用於免費叢集。</li>
<li>如果此標準叢集是您在這個位置所建立的第一個標準叢集，請不要使用此旗標。建立叢集時，會自動建立一個公用 VLAN。</li>
<li>如果您之前已在此位置中建立標準叢集，或之前已在 IBM Cloud 基礎架構 (SoftLayer) 中建立公用 VLAN，請指定該公用 VLAN。如果您只要將工作者節點連接至專用 VLAN，請不要指定此選項。

<p><strong>附註：</strong>專用 VLAN 路由器的開頭一律為 <code>bcr</code>（後端路由器），而公用 VLAN 路由器的開頭一律為 <code>fcr</code>（前端路由器）。當建立叢集並指定公用和專用 VLAN 時，那些字首之後的數字和字母組合必須相符。</p></li>
</ul>

<p>若要找出您是否已有特定位置的公用 VLAN，或尋找現有公用 VLAN 的名稱，請執行 <code>bx cs vlans <em>&lt;location&gt;</em></code>。</p></dd>

<dt><code>--workers WORKER</code></dt>
<dd>您要在叢集中部署的工作者節點數目。如果您未指定此選項，則會建立具有 1 個工作者節點的叢集。此值對於標準叢集是選用的，不適用於免費叢集。

<p><strong>附註：</strong>每個工作者節點都會獲指派唯一的工作者節點 ID 及網域名稱，在叢集建立之後即不得手動予以變更。變更 ID 或網域名稱會讓 Kubernetes 主節點無法管理叢集。</p></dd>

<dt><code>--disable-disk-encrypt</code></dt>
<dd>依預設，工作者節點會具備磁碟加密；[進一步瞭解](cs_secure.html#worker)。若要停用加密，請包含此選項。</dd>

<dt><code>--trusted</code></dt>
<dd><p>**僅限裸機**：啟用[授信運算](cs_secure.html#trusted_compute)，以驗證裸機工作者節點是否遭到竄改。如果您在叢集建立期間未啟用信任，但後來想要啟用，您可以使用 `bx cs feature-enable` [指令](cs_cli_reference.html#cs_cluster_feature_enable)。啟用信任之後，以後您就無法再予以停用。</p>
<p>若要檢查裸機機型是否支援信任，請檢查 `bx cs machine-types <location>` [指令](#cs_machine_types)輸出中的 `Trustable` 欄位。若要驗證叢集已啟用信任，請檢視 `bx cluster-get` [指令](#cs_cluster_get)輸出中的 **Trust ready** 欄位。若要驗證裸機工作者節點已啟用信任，請檢視 `bx cs worker-get` [指令](#cs_worker_get)輸出中的 **Trust** 欄位。</p></dd>

<dt><code>-s</code></dt>
<dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
</dl>

**範例**：

  

  **建立免費叢集**：只指定叢集名稱；其他所有項目都設為預設值。在 21 天後會自動刪除可用的叢集。您一次可以有一個免費叢集。若要充分運用 Kubernetes 的完整功能，請建立標準叢集。

  ```
  bx cs cluster-create --name my_cluster
  ```
  {: pre}

  **建立您的第一個標準叢集**：在位置中建立的第一個標準叢集也會建立專用 VLAN。因此，請不要包括 `--public-vlan` 旗標。
  {: #example_cluster_create}

  ```
  bx cs cluster-create --location dal10 --private-vlan my_private_VLAN_ID --machine-type u2c.2x4 --name my_cluster --hardware shared --workers 2
  ```
  {: pre}

  **建立後續標準叢集**：如果您已在此位置中建立標準叢集，或之前已在 IBM Cloud 基礎架構 (SoftLayer) 中建立公用 VLAN，請使用 `--public-vlan` 旗標來指定公用 VLAN。若要找出您是否已有特定位置的公用 VLAN，或尋找現有公用 VLAN 的名稱，請執行 `bx cs vlans <location>`。

  ```
  bx cs cluster-create --location dal10 --public-vlan my_public_VLAN_ID --private-vlan my_private_VLAN_ID --machine-type u2c.2x4 --name my_cluster --hardware shared --workers 2
  ```
  {: pre}

  **在 {{site.data.keyword.Bluemix_dedicated_notm}} 環境中建立叢集**：

  ```
  bx cs cluster-create --machine-type machine-type --workers number --name cluster_name
  ```
  {: pre}

### bx cs cluster-feature-enable [-f] CLUSTER [--trusted][-s]
{: #cs_cluster_feature_enable}

在現有叢集上啟用特性。

<strong>指令選項</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>叢集的名稱或 ID。這是必要值。</dd>

   <dt><code>-f</code></dt>
   <dd>使用此選項，以強制執行 <code>--trusted</code> 選項，而不出現任何使用者提示。這是選用值。</dd>

   <dt><code><em>--trusted</em></code></dt>
   <dd><p>包括此旗標，以對叢集中所有受支援的裸機工作者節點啟用[授信運算](cs_secure.html#trusted_compute)。啟用信任之後，以後您就無法針對叢集予以停用。</p>
   <p>若要檢查裸機機型是否支援信任，請檢查 **bx cs machine-types <location>` [指令](#cs_machine_types)輸出中的 `Trustable** 欄位。若要驗證叢集已啟用信任，請檢視 `bx cluster-get` [指令](#cs_cluster_get)輸出中的 **Trust ready** 欄位。若要驗證裸機工作者節點已啟用信任，請檢視 `bx cs worker-get` [指令](#cs_worker_get)輸出中的 **Trust** 欄位。</p></dd>

  <dt><code>-s</code></dt>
   <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
</dl>

**指令範例**：

  ```
  bx cs cluster-feature-enable my_cluster --trusted=true
  ```
  {: pre}

### bx cs cluster-get CLUSTER [--json][--showResources] [-s]
{: #cs_cluster_get}

檢視組織中叢集的相關資訊。

<strong>指令選項</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>叢集的名稱或 ID。這是必要值。</dd>

   <dt><code>--json</code></dt>
   <dd>以 JSON 格式列印指令輸出。這是選用值。</dd>

   <dt><code><em>--showResources</em></code></dt>
   <dd>顯示其他叢集資源，例如附加程式、VLAN、子網路及儲存空間。</dd>


  <dt><code>-s</code></dt>
  <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
  </dl>



**指令範例**：

  ```
  bx cs cluster-get my_cluster --showResources
  ```
  {: pre}

**輸出範例**：

  ```
  Name:        my_cluster
  ID:          abc1234567
  State:       normal
  Trust ready: false
  Created:     2018-01-01T17:19:28+0000
  Location:    dal10
  Master URL:  https://169.xx.xxx.xxx:xxxxx
  Ingress subdomain: my_cluster.us-south.containers.appdomain.cloud
  Ingress secret:    my_cluster
  Workers:     3
  Version:     1.7.16_1511* (1.8.11_1509 latest)
  Owner Email: name@example.com
  Monitoring dashboard: https://metrics.ng.bluemix.net/app/#/grafana4/dashboard/db/link

  Addons
  Name                   Enabled
  customer-storage-pod   true
  basic-ingress-v2       true
  storage-watcher-pod    true

  Subnet VLANs
  VLAN ID   Subnet CIDR         Public   User-managed
  2234947   10.xxx.xx.xxx/29    false    false
  2234945   169.xx.xxx.xxx/29  true    false

  ```
  {: screen}

### bx cs cluster-rm [-f] CLUSTER [-s]
{: #cs_cluster_rm}

從組織中移除叢集。

<strong>指令選項</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>叢集的名稱或 ID。這是必要值。</dd>

   <dt><code>-f</code></dt>
   <dd>使用此選項，以強制移除叢集，而不出現任何使用者提示。這是選用值。</dd>

   <dt><code>-s</code></dt>
   <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>

   </dl>

**範例**：

  ```
  bx cs cluster-rm my_cluster
  ```
  {: pre}


### bx cs cluster-update [-f] CLUSTER [--kube-version MAJOR.MINOR.PATCH][--force-update] [-s]
{: #cs_cluster_update}

將 Kubernetes 主節點更新為預設 API 版本。在更新期間，您無法存取或變更叢集。由使用者部署的工作者節點、應用程式及資源不會遭到修改，並會繼續執行。

您可能需要變更 YAML 檔案以進行未來的部署。如需詳細資料，請檢閱此[版本注意事項](cs_versions.html)。

<strong>指令選項</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>叢集的名稱或 ID。這是必要值。</dd>

   <dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
   <dd>叢集的 Kubernedes 版本。如果您未指定版本，則 Kubernetes 主節點會更新為預設 API 版本。若要查看可用的版本，請執行 [bx cs kube-versions](#cs_kube_versions)。這是選用值。</dd>

   <dt><code>-f</code></dt>
   <dd>使用此選項，以強制更新主節點，而不出現任何使用者提示。這是選用值。</dd>

   <dt><code>--force-update</code></dt>
   <dd>即使變更大於兩個次要版本，也要嘗試更新。這是選用值。</dd>

   <dt><code>-s</code></dt>
   <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
   </dl>

**範例**：

  ```
  bx cs cluster-update my_cluster
  ```
  {: pre}


### bx cs clusters [--json][-s]
{: #cs_clusters}

檢視組織中的叢集清單。

<strong>指令選項</strong>：

  <dl>
  <dt><code>--json</code></dt>
  <dd>以 JSON 格式列印指令輸出。這是選用值。</dd>

  <dt><code>-s</code></dt>
  <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
  </dl>

**範例**：

  ```
  bx cs clusters
  ```
  {: pre}


### bx cs kube-versions [--json][-s]
{: #cs_kube_versions}

檢視 {{site.data.keyword.containershort_notm}} 中支援的 Kubernetes 版本清單。將您的[主要叢集](#cs_cluster_update)及[工作者節點](cs_cli_reference.html#cs_worker_update)更新為最新且功能穩定的預設版本。

**指令選項**：

  <dl>
  <dt><code>--json</code></dt>
  <dd>以 JSON 格式列印指令輸出。這是選用值。</dd>

  <dt><code>-s</code></dt>
  <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
  </dl>

**範例**：

  ```
  bx cs kube-versions
  ```
  {: pre}



<br />



## 叢集指令：服務與整合
{: #cluster_services_commands}


### bx cs cluster-service-bind CLUSTER KUBERNETES_NAMESPACE SERVICE_INSTANCE_NAME [-s]
{: #cs_cluster_service_bind}

將 {{site.data.keyword.Bluemix_notm}} 服務新增至叢集。若要從 {{site.data.keyword.Bluemix_notm}} 型錄檢視可用的 {{site.data.keyword.Bluemix_notm}} 服務，請執行 `bx service offerings`。**附註**：您只能新增支援服務金鑰的 {{site.data.keyword.Bluemix_notm}} 服務。

<strong>指令選項</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>叢集的名稱或 ID。這是必要值。</dd>

   <dt><code><em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>Kubernetes 名稱空間的名稱。這是必要值。</dd>

   <dt><code><em>SERVICE_INSTANCE_NAME</em></code></dt>
   <dd>您要連結的 {{site.data.keyword.Bluemix_notm}} 服務實例的名稱。若要尋找服務實例的名稱，請執行 <code>bx service list</code>。如果帳戶中有多個實例具有相同名稱，請使用服務實例 ID 而非名稱。若要尋找 ID，請執行 <code>bx service show <service instance name> --guid</code>。其中一個值是必要的。</dd>

   <dt><code>-s</code></dt>
   <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>

   </dl>

**範例**：

  ```
  bx cs cluster-service-bind my_cluster my_namespace my_service_instance
  ```
  {: pre}


### bx cs cluster-service-unbind CLUSTER KUBERNETES_NAMESPACE SERVICE_INSTANCE_GUID [-s]
{: #cs_cluster_service_unbind}

從叢集中移除 {{site.data.keyword.Bluemix_notm}} 服務。

**附註：**移除 {{site.data.keyword.Bluemix_notm}} 服務時，會從叢集中移除服務認證。如果 pod 仍在使用服務，會因為找不到服務認證而失敗。

<strong>指令選項</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>叢集的名稱或 ID。這是必要值。</dd>

   <dt><code><em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>Kubernetes 名稱空間的名稱。這是必要值。</dd>

   <dt><code><em>SERVICE_INSTANCE_GUID</em></code></dt>
   <dd>您要移除的 {{site.data.keyword.Bluemix_notm}} 服務實例的 ID。若要尋找服務實例的 ID，請執行 `bx cs cluster-services <cluster_name_or_ID>`。這是必要值。</dd>

   <dt><code>-s</code></dt>
   <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>

   </dl>

**範例**：

  ```
  bx cs cluster-service-unbind my_cluster my_namespace 8567221
  ```
  {: pre}


### bx cs cluster-services CLUSTER [--namespace KUBERNETES_NAMESPACE][--all-namespaces] [--json][-s]
{: #cs_cluster_services}

列出連結至叢集中一個或所有 Kubernetes 名稱空間的服務。如果未指定任何選項，則會顯示 default 名稱空間的服務。

<strong>指令選項</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>叢集的名稱或 ID。這是必要值。</dd>

   <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code>, <code>-n <em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>包括連結至叢集中特定名稱空間的服務。這是選用值。</dd>

   <dt><code>--all-namespaces</code></dt>
    <dd>包括連結至叢集中所有名稱空間的服務。這是選用值。</dd>

    <dt><code>--json</code></dt>
    <dd>以 JSON 格式列印指令輸出。這是選用值。</dd>

    <dt><code>-s</code></dt>
    <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>

    </dl>

**範例**：

  ```
  bx cs cluster-services my_cluster --namespace my_namespace
  ```
  {: pre}



### bx cs webhook-create --cluster CLUSTER --level LEVEL --type slack --url URL  [-s]
{: #cs_webhook_create}

登錄 Webhook。

<strong>指令選項</strong>：

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>叢集的名稱或 ID。這是必要值。</dd>

   <dt><code>--level <em>LEVEL</em></code></dt>
   <dd>通知層次（例如 <code>Normal</code> 或 <code>Warning</code>）。<code>Warning</code> 是預設值。這是選用值。</dd>

   <dt><code>--type <em>slack</em></code></dt>
   <dd>Webhook 類型。目前支援 Slack。這是必要值。</dd>

   <dt><code>--url <em>URL</em></code></dt>
   <dd>Webhook 的 URL。這是必要值。</dd>

   <dt><code>-s</code></dt>
   <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
   </dl>

**範例**：

  ```
  bx cs webhook-create --cluster my_cluster --level Normal --type slack --url http://github.com/mywebhook
  ```
  {: pre}


<br />


## 叢集指令：子網路
{: #cluster_subnets_commands}

### bx cs cluster-subnet-add CLUSTER SUBNET [-s]
{: #cs_cluster_subnet_add}

將 IBM Cloud 基礎架構 (SoftLayer) 帳戶中的子網路設為可供指定的叢集使用。

**附註：**
* 當您讓子網路可供叢集使用時，會使用這個子網路的 IP 位址來進行叢集網路連線。若要避免 IP 位址衝突，請確定一個子網路只搭配使用一個叢集。請不要同時將子網路用於多個叢集或 {{site.data.keyword.containershort_notm}} 以外的其他用途。
* 若要在相同 VLAN 上的子網路之間遞送，您必須[開啟 VLAN 跨越](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning)。

<strong>指令選項</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>叢集的名稱或 ID。這是必要值。</dd>

   <dt><code><em>SUBNET</em></code></dt>
   <dd>子網路的 ID。這是必要值。</dd>

   <dt><code>-s</code></dt>
   <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>

   </dl>

**範例**：

  ```
  bx cs cluster-subnet-add my_cluster 1643389
  ```
  {: pre}


### bx cs cluster-subnet-create CLUSTER SIZE VLAN_ID [-s]
{: #cs_cluster_subnet_create}

在 IBM Cloud 基礎架構 (SoftLayer) 帳戶中建立子網路，並將它設為可供 {{site.data.keyword.containershort_notm}} 中指定的叢集使用。

**附註：**
* 當您讓子網路可供叢集使用時，會使用這個子網路的 IP 位址來進行叢集網路連線。若要避免 IP 位址衝突，請確定一個子網路只搭配使用一個叢集。請不要同時將子網路用於多個叢集或 {{site.data.keyword.containershort_notm}} 以外的其他用途。
* 若要在相同 VLAN 上的子網路之間遞送，您必須[開啟 VLAN 跨越](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning)。

<strong>指令選項</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>叢集的名稱或 ID。這是必要值。若要列出叢集，請使用 `bx cs clusters` [指令](#cs_clusters)。</dd>

   <dt><code><em>SIZE</em></code></dt>
   <dd>子網路 IP 位址的數目。這是必要值。可能的值為 8、16、32 或 64。</dd>

   <dt><code><em>VLAN_ID</em></code></dt>
   <dd>要在其中建立子網路的 VLAN。這是必要值。如果要列出可用的 VLAN，請使用 `bx cs vlans<location>` [指令](#cs_vlans)。</dd>

   <dt><code>-s</code></dt>
   <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>

   </dl>

**範例**：

  ```
  bx cs cluster-subnet-create my_cluster 8 1764905
  ```
  {: pre}


### bx cs cluster-user-subnet-add CLUSTER SUBNET_CIDR PRIVATE_VLAN
{: #cs_cluster_user_subnet_add}

將您自己的專用子網路帶到 {{site.data.keyword.containershort_notm}} 叢集。

這個專用子網路不是 IBM Cloud 基礎架構 (SoftLayer) 所提供的專用子網路。因此，您必須配置子網路的任何入埠及出埠網路資料流量遞送。若要新增 IBM Cloud 基礎架構 (SoftLayer) 子網路，請使用 `bx cs cluster-subnet-add` [指令](#cs_cluster_subnet_add)。

**附註**：
* 當您將專用使用者子網路新增至叢集時，這個子網路的 IP 位址會用於叢集中的專用「負載平衡器」。若要避免 IP 位址衝突，請確定一個子網路只搭配使用一個叢集。請不要同時將子網路用於多個叢集或 {{site.data.keyword.containershort_notm}} 以外的其他用途。
* 若要在相同 VLAN 上的子網路之間遞送，您必須[開啟 VLAN 跨越](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning)。

<strong>指令選項</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>叢集的名稱或 ID。這是必要值。</dd>

   <dt><code><em>SUBNET_CIDR</em></code></dt>
   <dd>子網路無類別跨網域遞送 (CIDR)。這是必要值，且不得與 IBM Cloud 基礎架構 (SoftLayer) 所使用的任何子網路衝突。

   支援的字首範圍從 `/30`（1 個 IP 位址）一直到 `/24`（253 個 IP 位址）。如果您將 CIDR 設在一個字首長度，之後又需要變更它，則請先新增新的 CIDR，然後[移除舊的 CIDR](#cs_cluster_user_subnet_rm)。</dd>

   <dt><code><em>PRIVATE_VLAN</em></code></dt>
   <dd>專用 VLAN 的 ID。這是必要值。它必須符合叢集中一個以上工作者節點的專用 VLAN ID。</dd>
   </dl>

**範例**：

  ```
  bx cs cluster-user-subnet-add my_cluster 169.xx.xxx.xxx/29 1502175
  ```
  {: pre}


### bx cs cluster-user-subnet-rm CLUSTER SUBNET_CIDR PRIVATE_VLAN
{: #cs_cluster_user_subnet_rm}

從指定的叢集移除您自己的專用子網路。

**附註：**從您自己的專用子網路部署至 IP 位址的任何服務，都會在移除子網路之後保持作用中。

<strong>指令選項</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>叢集的名稱或 ID。這是必要值。</dd>

   <dt><code><em>SUBNET_CIDR</em></code></dt>
   <dd>子網路無類別跨網域遞送 (CIDR)。這是必要值，且必須符合 `bx cs cluster-user-subnet-add` [指令](#cs_cluster_user_subnet_add)設定的 CIDR。</dd>

   <dt><code><em>PRIVATE_VLAN</em></code></dt>
   <dd>專用 VLAN 的 ID。這是必要值，且必須符合 `bx cs cluster-user-subnet-add` [指令](#cs_cluster_user_subnet_add)設定的 VLAN ID。</dd>
   </dl>

**範例**：

  ```
  bx cs cluster-user-subnet-rm my_cluster 169.xx.xxx.xxx/29 1502175
  ```
  {: pre}

### bx cs subnets [--json][-s]
{: #cs_subnets}

檢視 IBM Cloud 基礎架構 (SoftLayer) 帳戶中可用的子網路清單。

<strong>指令選項</strong>：

  <dl>
  <dt><code>--json</code></dt>
  <dd>以 JSON 格式列印指令輸出。這是選用值。</dd>

  <dt><code>-s</code></dt>
  <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
  </dl>

**範例**：

  ```
  bx cs subnets
  ```
  {: pre}


<br />


## Ingress 應用程式負載平衡器 (ALB) 指令
{: #alb_commands}

### bx cs alb-cert-deploy [--update] --cluster CLUSTER --secret-name SECRET_NAME --cert-crn CERTIFICATE_CRN [-s]
{: #cs_alb_cert_deploy}

將 {{site.data.keyword.cloudcerts_long_notm}} 實例中的憑證部署或更新至叢集中的 ALB。

**附註：**
* 只有具有管理者存取角色的使用者能執行這個指令。
* 您只能更新從相同 {{site.data.keyword.cloudcerts_long_notm}} 實例匯入的憑證。

<strong>指令選項</strong>

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>叢集的名稱或 ID。這是必要值。</dd>

   <dt><code>--update</code></dt>
   <dd>更新叢集中 ALB 密碼的憑證。這是選用值。</dd>

   <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
   <dd>ALB 密碼的名稱。這是必要值。</dd>

   <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
   <dd>憑證 CRN。這是必要值。</dd>

   <dt><code>-s</code></dt>
   <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
   </dl>

**範例**：

部署 ALB 密碼的範例：

   ```
   bx cs alb-cert-deploy --secret-name my_alb_secret --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
   ```
   {: pre}

更新現有 ALB 密碼的範例：

 ```
 bx cs alb-cert-deploy --update --secret-name my_alb_secret --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:7e21fde8ee84a96d29240327daee3eb2
 ```
 {: pre}


### bx cs alb-cert-get --cluster CLUSTER [--secret-name SECRET_NAME][--cert-crn CERTIFICATE_CRN] [--json][-s]
{: #cs_alb_cert_get}

檢視叢集中的 ALB 密碼的相關資訊。

**附註：**只有具有管理者存取角色的使用者能執行這個指令。

<strong>指令選項</strong>

  <dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
  <dd>叢集的名稱或 ID。這是必要值。</dd>

  <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
  <dd>ALB 密碼的名稱。需要有這個值，才能取得叢集中特定 ALB 密碼的相關資訊。</dd>

  <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
  <dd>憑證 CRN。需要有這個值，才能取得符合叢集中特定憑證 CRN 的所有 ALB 密碼的相關資訊。</dd>

  <dt><code>--json</code></dt>
  <dd>以 JSON 格式列印指令輸出。這是選用值。</dd>

  <dt><code>-s</code></dt>
  <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
  </dl>

**範例**：

 提取 ALB 密碼相關資訊的範例：

 ```
 bx cs alb-cert-get --cluster my_cluster --secret-name my_alb_secret
 ```
 {: pre}

 提取符合指定憑證 CRN 的所有 ALB 密碼的相關資訊的範例：

 ```
 bx cs alb-cert-get --cluster my_cluster --cert-crn  crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
 ```
 {: pre}


### bx cs alb-cert-rm --cluster CLUSTER [--secret-name SECRET_NAME][--cert-crn CERTIFICATE_CRN] [-s]
{: #cs_alb_cert_rm}

移除叢集中的 ALB 密碼。

**附註：**只有具有管理者存取角色的使用者能執行這個指令。

<strong>指令選項</strong>

  <dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
  <dd>叢集的名稱或 ID。這是必要值。</dd>

  <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
  <dd>ALB 密碼的名稱。需要有這個值，才能移除叢集中的特定 ALB 密碼。</dd>

  <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
  <dd>憑證 CRN。需要有這個值，才能移除符合叢集中特定憑證 CRN 的所有 ALB 密碼。</dd>

  <dt><code>-s</code></dt>
  <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>

  </dl>

**範例**：

 移除 ALB 密碼的範例：

 ```
 bx cs alb-cert-rm --cluster my_cluster --secret-name my_alb_secret
 ```
 {: pre}

 移除符合指定憑證 CRN 的所有 ALB 密碼的範例：

 ```
 bx cs alb-cert-rm --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
 ```
 {: pre}


### bx cs alb-certs --cluster CLUSTER [--json][-s]
{: #cs_alb_certs}

檢視叢集中的 ALB 密碼清單。

**附註：**只有具有管理者存取角色的使用者能執行這個指令。

<strong>指令選項</strong>

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>叢集的名稱或 ID。這是必要值。</dd>
   <dt><code>--json</code></dt>
   <dd>以 JSON 格式列印指令輸出。這是選用值。</dd>
   <dt><code>-s</code></dt>
   <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
   </dl>

**範例**：

 ```
 bx cs alb-certs --cluster my_cluster
 ```
 {: pre}

### bx cs alb-configure --albID ALB_ID [--enable][--disable][--user-ip USERIP][-s]
{: #cs_alb_configure}

在標準叢集中啟用或停用 ALB。依預設，會啟用公用 ALB。

**指令選項**：

   <dl>
   <dt><code><em>--albID </em>ALB_ID</code></dt>
   <dd>ALB 的 ID。執行 <code>bx cs albs <em>--cluster </em>CLUSTER</code>，以檢視叢集中 ALB 的 ID。這是必要值。</dd>

   <dt><code>--enable</code></dt>
   <dd>包括此旗標，以在叢集中啟用 ALB。</dd>

   <dt><code>--disable</code></dt>
   <dd>包括此旗標，以在叢集中停用 ALB。</dd>

   <dt><code>--user-ip <em>USER_IP</em></code></dt>
   <dd>

   <ul>
    <li>此參數僅適用於啟用專用 ALB。</li>
    <li>專用 ALB 是使用來自使用者提供的專用子網路的 IP 位址進行部署。如果未提供 IP 位址，則會使用您建立叢集時自動佈建的可攜式專用子網路中的專用 IP 位址進行部署。</li>
   </ul>
   </dd>

   <dt><code>-s</code></dt>
   <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>

   </dl>

**範例**：

  啟用 ALB 的範例：

  ```
  bx cs alb-configure --albID private-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --enable
  ```
  {: pre}

  以使用者提供的 IP 位址啟用 ALB 的範例：

  ```
  bx cs alb-configure --albID private-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --enable --user-ip user_ip
  ```
  {: pre}

  停用 ALB 的範例：

  ```
  bx cs alb-configure --albID public-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --disable
  ```
  {: pre}

### bx cs alb-get --albID ALB_ID [--json][-s]
{: #cs_alb_get}

檢視 ALB 的詳細資料。

<strong>指令選項</strong>：

   <dl>
   <dt><code><em>--albID </em>ALB_ID</code></dt>
   <dd>ALB 的 ID。執行 <code>bx cs albs --cluster <em>CLUSTER</em></code>，以檢視叢集中 ALB 的 ID。這是必要值。</dd>

   <dt><code>--json</code></dt>
   <dd>以 JSON 格式列印指令輸出。這是選用值。</dd>

  <dt><code>-s</code></dt>
  <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>

   </dl>

**範例**：

  ```
  bx cs alb-get --albID public-cr18a61a63a6a94b658596aa93a087aaa9-alb1
  ```
  {: pre}

### bx cs alb-types [--json][-s]
{: #cs_alb_types}

檢視地區中支援的 ALB 類型。

<strong>指令選項</strong>：

  <dl>
  <dt><code>--json</code></dt>
  <dd>以 JSON 格式列印指令輸出。這是選用值。</dd>

  <dt><code>-s</code></dt>
  <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
  </dl>

**範例**：

  ```
  bx cs alb-types
  ```
  {: pre}


### bx cs albs --cluster CLUSTER [--json][-s]
{: #cs_albs}

檢視叢集中所有 ALB 的狀態。如果未傳回任何 ALB ID，則叢集沒有可攜式子網路。您可以[建立](#cs_cluster_subnet_create)或[新增](#cs_cluster_subnet_add)子網路至叢集。

<strong>指令選項</strong>：

   <dl>
   <dt><code><em>--cluster </em>CLUSTER</code></dt>
   <dd>您列出可用 ALB 之叢集的名稱或 ID。這是必要值。</dd>

   <dt><code>--json</code></dt>
   <dd>以 JSON 格式列印指令輸出。這是選用值。</dd>

   <dt><code>-s</code></dt>
   <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>

   </dl>

**範例**：

  ```
  bx cs albs --cluster my_cluster
  ```
  {: pre}


<br />


## 基礎架構指令
{: #infrastructure_commands}

### bx cs credentials-set --infrastructure-api-key API_KEY --infrastructure-username USERNAME [-s]
{: #cs_credentials_set}

針對 {{site.data.keyword.containershort_notm}} 帳戶設定 IBM Cloud 基礎架構 (SoftLayer) 帳戶認證。

如果您有 {{site.data.keyword.Bluemix_notm}}「隨收隨付制」帳戶，依預設，您可以存取 IBM Cloud 基礎架構 (SoftLayer) 組合。不過，您可能想要使用您已具有的不同 IBM Cloud 基礎架構 (SoftLayer) 帳戶來訂購基礎架構。您可以使用此指令，將此基礎架構帳戶鏈結至 {{site.data.keyword.Bluemix_notm}} 帳戶。

如果手動設定了 IBM Cloud 基礎架構 (SoftLayer) 認證，則即使帳戶已有 [IAM API 金鑰](#cs_api_key_info)，也會使用這些認證來訂購基礎架構。如果已儲存其認證的使用者沒有訂購基礎架構的必要許可權，則基礎架構相關動作（例如建立叢集或重新載入工作者節點）可能會失敗。

您無法為一個 {{site.data.keyword.containershort_notm}} 帳戶設定多個認證。每個 {{site.data.keyword.containershort_notm}} 帳戶僅會鏈結至一個 IBM Cloud 基礎架構 (SoftLayer) 組合。

**重要事項：**在使用此指令之前，請確定使用其認證的使用者具有必要的 [{{site.data.keyword.containershort_notm}} 及 IBM Cloud 基礎架構 (SoftLayer) 許可權](cs_users.html#users)。

<strong>指令選項</strong>：

   <dl>
   <dt><code>--infrastructure-username <em>USERNAME</em></code></dt>
   <dd>IBM Cloud 基礎架構 (SoftLayer) 帳戶使用者名稱。這是必要值。</dd>


   <dt><code>--infrastructure-api-key <em>API_KEY</em></code></dt>
   <dd>IBM Cloud 基礎架構 (SoftLayer) 帳戶 API 金鑰。這是必要值。

 <p>
  若要產生 API 金鑰，請執行下列動作：

  <ol>
  <li>登入 [IBM Cloud 基礎架構 (SoftLayer) 入口網站 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/)。</li>
  <li>選取<strong>帳戶</strong>，然後選取<strong>使用者</strong>。</li>
  <li>按一下<strong>產生</strong>，以產生帳戶的 IBM Cloud 基礎架構 (SoftLayer) API 金鑰。</li>
  <li>複製 API 金鑰以便在這個指令中使用。</li>
  </ol>

  若要檢視現有的 API 金鑰，請執行下列動作：
  <ol>
  <li>登入 [IBM Cloud 基礎架構 (SoftLayer) 入口網站 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/)。</li>
  <li>選取<strong>帳戶</strong>，然後選取<strong>使用者</strong>。</li>
  <li>按一下<strong>檢視</strong>，以查看現有的 API 金鑰。</li>
  <li>複製 API 金鑰以便在這個指令中使用。</li>
  </ol>
  </p></dd>

  <dt><code>-s</code></dt>
  <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>

  </dl>

**範例**：

  ```
  bx cs credentials-set --infrastructure-api-key <api_key> --infrastructure-username dbmanager
  ```
  {: pre}


### bx cs credentials-unset
{: #cs_credentials_unset}

從您的 {{site.data.keyword.containershort_notm}} 帳戶中移除 IBM Cloud 基礎架構 (SoftLayer) 帳戶認證。

移除認證之後，會使用 [IAM API 金鑰](#cs_api_key_info)來訂購 IBM Cloud 基礎架構 (SoftLayer) 中的資源。

<strong>指令選項</strong>：

  <dl>
  <dt><code>-s</code></dt>
  <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
  </dl>

**範例**：

  ```
  bx cs credentials-unset
  ```
  {: pre}


### bx cs machine-types LOCATION [--json][-s]
{: #cs_machine_types}

檢視工作者節點的可用機型清單。機型因位置而異。每一個機型都包括叢集中每一個工作者節點的虛擬 CPU、記憶體及磁碟空間量。依預設，儲存所有容器資料的 `/var/lib/docker` 目錄是使用 LUKS 加密來進行加密。如果在建立叢集的期間包含了 `disable-disk-encrypt` 選項，則主機的 Docker 資料不會加密。[進一步瞭解加密。](cs_secure.html#encrypted_disks)
{:shortdesc}

您可以將工作者節點佈建為共用或專用硬體上的虛擬機器，或佈建為裸機上的實體機器。

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


<strong>指令選項</strong>：

   <dl>
   <dt><code><em>LOCATION</em></code></dt>
   <dd>輸入您要列出可用機型的位置。這是必要值。檢閱[可用位置](cs_regions.html#locations)。</dd>

   <dt><code>--json</code></dt>
  <dd>以 JSON 格式列印指令輸出。這是選用值。</dd>

  <dt><code>-s</code></dt>
  <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
  </dl>

**指令範例**：

  ```
  bx cs machine-types dal10
  ```
  {: pre}

**輸出範例**：

  ```
  Getting machine types list...
  OK
  Machine Types
  Name                 Cores   Memory   Network Speed   OS             Server Type   Storage   Secondary Storage   Trustable
  u2c.2x4              2       4GB      1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  b2c.4x16             4       16GB     1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  b2c.16x64            16      64GB     1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  b2c.32x128           32      128GB    1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  b2c.56x242           56      242GB    1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  mb1c.4x32            4       32GB     10000Mbps       UBUNTU_16_64   physical      1000GB    2000GB              False
  mb1c.16x64           16      64GB     10000Mbps       UBUNTU_16_64   physical      1000GB    1700GB              False
  mr1c.28x512          28      512GB    10000Mbps       UBUNTU_16_64   physical      1000GB    1700GB              False
  md1c.16x64.4x4tb     16      64GB     10000Mbps       UBUNTU_16_64   physical      1000GB    8000GB              False
  md1c.28x512.4x4tb    28      512GB    10000Mbps       UBUNTU_16_64   physical      1000GB    8000GB              False
  
  ```
  {: screen}


### bx cs vlans LOCATION [--all][--json] [-s]
{: #cs_vlans}

列出 IBM Cloud 基礎架構 (SoftLayer) 帳戶中的位置可用的公用及專用 VLAN。若要列出可用的 VLAN，您必須具有付費帳戶。

<strong>指令選項</strong>：

   <dl>
   <dt><code><em>LOCATION</em></code></dt>
   <dd>輸入您要列出專用及公用 VLAN 的位置。這是必要值。檢閱[可用位置](cs_regions.html#locations)。</dd>

   <dt><code>--all</code></dt>
   <dd>列出所有可用的 VLAN。依預設會過濾 VLAN，只顯示那些有效的 VLAN。VLAN 若要有效，則必須與基礎架構相關聯，且該基礎架構可以管理具有本端磁碟儲存空間的工作者節點。</dd>

   <dt><code>--json</code></dt>
  <dd>以 JSON 格式列印指令輸出。這是選用值。</dd>

  <dt><code>-s</code></dt>
  <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
   </dl>

**範例**：

  ```
  bx cs vlans dal10
  ```
  {: pre}


<br />


## 記載指令
{: #logging_commands}

### bx cs logging-config-create CLUSTER --logsource LOG_SOURCE [--namespace KUBERNETES_NAMESPACE][--hostname LOG_SERVER_HOSTNAME_OR_IP] [--port LOG_SERVER_PORT][--space CLUSTER_SPACE] [--org CLUSTER_ORG][--app-containers CONTAINERS] [--app-paths PATHS_TO_LOGS][--syslog-protocol PROTOCOL] --type LOG_TYPE [--json][--skip-validation] [-s]
{: #cs_logging_create}

建立記載配置。您可以使用這個指令，將容器、應用程式、工作者節點、Kubernetes 叢集及 Ingress 應用程式負載平衡器的日誌轉遞至 {{site.data.keyword.loganalysisshort_notm}} 或外部 syslog 伺服器。

<strong>指令選項</strong>：

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>叢集的名稱或 ID。</dd>

  <dt><code>--logsource <em>LOG_SOURCE</em></code></dt>    
    <dd>要為其啟用日誌轉遞的日誌來源。此引數支援以逗點區隔的日誌來源（要套用其配置）清單。接受值為 <code>container</code>、<code>application</code>、<code>worker</code>、<code>kubernetes</code>、<code>ingress</code> 及 <code>kube-audit</code>。如果您未提供日誌來源，則會建立 <code>container</code> 及 <code>ingress</code> 日誌來源的記載配置。</dd>

  <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
    <dd>您要從該處轉遞日誌的 Kubernetes 名稱空間。<code>ibm-system</code> 及 <code>kube-system</code> Kubernetes 名稱空間不支援日誌轉遞。此值僅對容器日誌來源有效且為選用。如果未指定名稱空間，則叢集中的所有名稱空間都會使用此配置。</dd>

  <dt><code>--hostname <em>LOG_SERVER_HOSTNAME</em></code></dt>
    <dd>記載類型是 <code>syslog</code> 時，則為日誌收集器伺服器的主機名稱或 IP 位址。這對於 <code>syslog</code> 是必要值。記載類型是 <code>ibm</code> 時，則為 {{site.data.keyword.loganalysislong_notm}} 汲取 URL。您可以在[這裡](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls)尋找可用的汲取 URL 清單。如果您未指定汲取 URL，則會使用您在其中建立叢集之地區的端點。</dd>

  <dt><code>--port <em>LOG_SERVER_PORT</em></code></dt>
    <dd>日誌收集器伺服器的埠。這是選用值。如果您未指定埠，則會將標準埠 <code>514</code> 用於 <code>syslog</code>，而將標準埠 <code>9091</code> 用於 <code>ibm</code>。</dd>

  <dt><code>--space <em>CLUSTER_SPACE</em></code></dt>
    <dd>您要將日誌傳送至其中的 Cloud Foundry 空間的名稱。此值僅對日誌類型 <code>ibm</code> 有效且為選用。如果您未指定空間，則會將日誌傳送至帳戶層次。</dd>

  <dt><code>--org <em>CLUSTER_ORG</em></code></dt>
    <dd>空間所在的 Cloud Foundry 組織的名稱。此值僅對日誌類型 <code>ibm</code> 有效，而且如果您已指定空間，則這是必要值。</dd>

  <dt><code>--app-paths</code></dt>
    <dd>應用程式記載至的容器上的路徑。若要使用來源類型 <code>application</code> 轉遞日誌，您必須提供路徑。若要指定多個路徑，請使用逗點區隔清單。這對於日誌來源 <code>application</code> 是必要值。範例：<code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></dd>

  <dt><code>--syslog-protocol</code></dt>
    <dd>記載類型為 <code>syslog</code> 時，所使用的傳送層通訊協定。支援值為 <code>TCP</code> 及預設 <code>UDP</code>。使用 <code>udp</code> 通訊協定轉遞至 rsyslog 伺服器時，會截斷超過 1KB 的日誌。</dd>

  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>您要轉遞日誌的位置。選項為 <code>ibm</code>，它會將日誌轉遞至 {{site.data.keyword.loganalysisshort_notm}}，還有 <code>syslog</code>，它會將日誌轉遞至外部伺服器。</dd>

  <dt><code>--app-containers</code></dt>
    <dd>若要從應用程式中轉遞日誌，可以指定包含您應用程式之容器的名稱。您可以使用逗點區隔清單來指定多個容器。如果未指定任何容器，則會從包含您所提供路徑的所有容器轉遞日誌。此選項僅適用於日誌來源 <code>application</code>。</dd>

  <dt><code>--json</code></dt>
    <dd>以 JSON 格式列印指令輸出。這是選用值。</dd>

  <dt><code>--skip-validation</code></dt>
    <dd>在指定組織及空間名稱時跳過其驗證。跳過驗證可減少處理時間，但是無效的記載配置不會正確轉遞日誌。這是選用值。</dd>

    <dt><code>-s</code></dt>
    <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
</dl>

**範例**：

在預設埠 9091 上從 `container` 日誌來源轉遞之日誌類型 `ibm` 的範例：

  ```
  bx cs logging-config-create my_cluster --logsource container --namespace my_namespace --hostname ingest.logging.ng.bluemix.net --type ibm
  ```
  {: pre}

在預設埠 514 上從 `container` 日誌來源轉遞之日誌類型 `syslog` 的範例：

  ```
  bx cs logging-config-create my_cluster --logsource container --namespace my_namespace  --hostname 169.xx.xxx.xxx --type syslog
  ```
  {: pre}

在與預設值不同的埠上從 `ingress` 來源轉遞日誌之日誌類型 `syslog` 的範例：

  ```
  bx cs logging-config-create my_cluster --logsource container --hostname 169.xx.xxx.xxx --port 5514 --type syslog
  ```
  {: pre}

### bx cs logging-config-get CLUSTER [--logsource LOG_SOURCE][--json] [-s]
{: #cs_logging_get}

檢視叢集的所有日誌轉遞配置，或根據日誌來源來過濾記載配置。

<strong>指令選項</strong>：

 <dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>叢集的名稱或 ID。這是必要值。</dd>

  <dt><code>--logsource <em>LOG_SOURCE</em></code></dt>
    <dd>您要過濾的日誌來源種類。只會傳回叢集中這個日誌來源的記載配置。接受值為 <code>container</code>、<code>application</code>、<code>worker</code>、<code>kubernetes</code>、<code>ingress</code> 及 <code>kube-audit</code>。這是選用值。</dd>

  <dt><code>--show-covering-filters</code></dt>
    <dd>顯示將先前過濾器呈現為已作廢的記載過濾器。</dd>

  <dt><code>--json</code></dt>
    <dd>以 JSON 格式列印指令輸出。這是選用值。</dd>

  <dt><code>-s</code></dt>
    <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
 </dl>

**範例**：

  ```
  bx cs logging-config-get my_cluster --logsource worker
  ```
  {: pre}


### bx cs logging-config-refresh CLUSTER [-s]
{: #cs_logging_refresh}

重新整理叢集的記載配置。這會為轉遞至您叢集空間層次的任何記載配置，重新整理其記載記號。

<strong>指令選項</strong>：

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
   <dd>叢集的名稱或 ID。這是必要值。</dd>

   <dt><code>-s</code></dt>
     <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
</dl>

**範例**：

  ```
  bx cs logging-config-refresh my_cluster
  ```
  {: pre}


### bx cs logging-config-rm CLUSTER [--id LOG_CONFIG_ID][--all] [-s]
{: #cs_logging_rm}

刪除叢集的一個日誌轉遞配置或所有記載配置。這會停止將日誌轉遞至遠端 syslog 伺服器或 {{site.data.keyword.loganalysisshort_notm}}。

<strong>指令選項</strong>：

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
   <dd>叢集的名稱或 ID。這是必要值。</dd>

  <dt><code>--id <em>LOG_CONFIG_ID</em></code></dt>
   <dd>如果您要移除單一記載配置，則為記載配置 ID。</dd>

  <dt><code>--all</code></dt>
   <dd>此旗標會移除叢集中的所有記載配置。</dd>

   <dt><code>-s</code></dt>
     <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
</dl>

**範例**：

  ```
  bx cs logging-config-rm my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e
  ```
  {: pre}


### bx cs logging-config-update CLUSTER --id LOG_CONFIG_ID [--namespace NAMESPACE][--hostname LOG_SERVER_HOSTNAME_OR_IP] [--port LOG_SERVER_PORT][--space CLUSTER_SPACE] [--org CLUSTER_ORG][--app-paths PATH] [--app-containers PATH] --type LOG_TYPE [--json][--skipValidation] [-s]
{: #cs_logging_update}

更新日誌轉遞配置的詳細資料。

<strong>指令選項</strong>：

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
   <dd>叢集的名稱或 ID。這是必要值。</dd>

  <dt><code>--id <em>LOG_CONFIG_ID</em></code></dt>
   <dd>您要更新的記載配置 ID。這是必要值。</dd>

  <dt><code>--namespace <em>NAMESPACE</em></code>
    <dd>您要從該處轉遞日誌的 Kubernetes 名稱空間。<code>ibm-system</code> 及 <code>kube-system</code> Kubernetes 名稱空間不支援日誌轉遞。此值僅適用於 <code>container</code> 日誌來源。如果未指定名稱空間，則叢集中的所有名稱空間都會使用此配置。</dd>

  <dt><code>--hostname <em>LOG_SERVER_HOSTNAME</em></code></dt>
   <dd>記載類型是 <code>syslog</code> 時，則為日誌收集器伺服器的主機名稱或 IP 位址。這對於 <code>syslog</code> 是必要值。記載類型是 <code>ibm</code> 時，則為 {{site.data.keyword.loganalysislong_notm}} 汲取 URL。您可以在[這裡](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls)尋找可用的汲取 URL 清單。如果您未指定汲取 URL，則會使用您在其中建立叢集之地區的端點。</dd>

   <dt><code>--port <em>LOG_SERVER_PORT</em></code></dt>
   <dd>日誌收集器伺服器的埠。當記載類型是 <code>syslog</code> 時，這是選用值。如果您未指定埠，則會將標準埠 <code>514</code> 用於 <code>syslog</code>，而將 <code>9091</code> 用於 <code>ibm</code>。</dd>

   <dt><code>--space <em>CLUSTER_SPACE</em></code></dt>
   <dd>您要將日誌傳送至其中的空間的名稱。此值僅對日誌類型 <code>ibm</code> 有效且為選用。如果您未指定空間，則會將日誌傳送至帳戶層次。</dd>

   <dt><code>--org <em>CLUSTER_ORG</em></code></dt>
   <dd>空間所在組織的名稱。此值僅對日誌類型 <code>ibm</code> 有效，而且如果您已指定空間，則這是必要值。</dd>

   <dt><code>--app-paths <em>PATH</em>,<em>PATH</em></code></dt>
     <dd>容器中要從中收集日誌的絕對檔案路徑。可以使用萬用字元（例如 '/var/log/*.log'），但無法使用遞迴 Glob（例如 '/var/log/**/test.log'）。若要指定多個路徑，請使用逗點區隔清單。當您針對日誌來源指定 'application' 時，這是必要值。</dd>

   <dt><code>--app-containers <em>PATH</em>,<em>PATH</em></code></dt>
     <dd>應用程式記載至的容器上的路徑。若要使用來源類型 <code>application</code> 轉遞日誌，您必須提供路徑。若要指定多個路徑，請使用逗點區隔清單。範例：<code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></dd>

   <dt><code>--type <em>LOG_TYPE</em></code></dt>
   <dd>您要使用的日誌轉遞通訊協定。目前支援 <code>syslog</code> 和 <code>ibm</code>。這是必要值。</dd>

   <dt><code>--json</code></dt>
   <dd>以 JSON 格式列印指令輸出。這是選用值。</dd>

   <dt><code>--skipValidation</code></dt>
   <dd>在指定組織及空間名稱時跳過其驗證。跳過驗證可減少處理時間，但是無效的記載配置不會正確轉遞日誌。這是選用值。</dd>

   <dt><code>-s</code></dt>
     <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
     </dl>

**日誌類型 `ibm`** 的範例：

  ```
  bx cs logging-config-update my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e --type ibm
  ```
  {: pre}

**日誌類型 `syslog`** 的範例：

  ```
  bx cs logging-config-update my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e --hostname localhost --port 5514 --type syslog
  ```
  {: pre}


### bx cs logging-filter-create CLUSTER --type LOG_TYPE [--logging-configs CONFIGS][--namespace KUBERNETES_NAMESPACE] [--container CONTAINER_NAME][--level LOGGING_LEVEL] [--regex-message MESSAGE][--json] [-s]
{: #cs_log_filter_create}

建立記載過濾器。您可以使用這個指令來過濾掉記載配置所轉遞的日誌。

<strong>指令選項</strong>：

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>您要為其建立記載過濾器之叢集的名稱或 ID。這是必要值。</dd>

  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>您要將過濾器套用至其中的日誌類型。目前支援 <code>all</code>、<code>container</code> 及 <code>host</code>。</dd>

  <dt><code>--logging-configs <em>CONFIGS</em></code></dt>
    <dd>以逗點區隔的記載配置 ID 清單。如果未提供，過濾器會套用至傳遞給過濾器的所有叢集記載配置。您可以使用 <code>--show-matching-configs</code> 旗標與指令搭配，來檢視符合過濾器的日誌配置。這是選用值。</dd>

  <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
    <dd>您要從中過濾日誌的 Kubernetes 名稱空間。這是選用值。</dd>

  <dt><code>--container <em>CONTAINER_NAME</em></code></dt>
    <dd>您要從中過濾掉日誌的容器名稱。僅在您使用日誌類型 <code>container</code> 時，此旗標才適用。這是選用值。</dd>

  <dt><code>--level <em>LOGGING_LEVEL</em></code></dt>
    <dd>過濾掉指定層次以下的日誌。可接受的值依其標準順序為 <code>fatal</code>、<code>error</code>、<code>warn/warning</code>、<code>info</code>、<code>debug</code> 及 <code>trace</code>。這是選用值。舉例來說，如果您過濾掉 <code>info</code> 層次的日誌，則也會過濾掉 <code>debug</code> 及 <code>trace</code>。**附註**：只有在日誌訊息是 JSON 格式且包含 level 欄位時，才能使用此旗標。輸出範例：<code>{"log": "hello", "level": "info"}</code></dd>

  <dt><code>--regex-message <em>MESSAGE</em></code></dt>
    <dd>過濾掉日誌中任何位置包含撰寫為正規表示式之指定訊息的任何日誌。這是選用值。</dd>

  <dt><code>--json</code></dt>
    <dd>以 JSON 格式列印指令輸出。這是選用值。</dd>

  <dt><code>-s</code></dt>
    <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
</dl>

**範例**：

這個範例會過濾掉從 default 名稱空間中名為 `test-container` 的容器轉遞而來、在 debug 層次以下，並且具有包含 "GET request" 之日誌訊息的所有日誌。

  ```
  bx cs logging-filter-create example-cluster --type container --namespace default --container test-container --level debug --message "GET request"
  ```
  {: pre}

這個範例會過濾掉從特定叢集轉遞且在 info 層次以下的所有日誌。輸出會以 JSON 傳回。

  ```
  bx cs logging-filter-create example-cluster --type all --level info --json
  ```
  {: pre}



### bx cs logging-filter-get CLUSTER [--id FILTER_ID][--show-matching-configs] [--show-covering-filters][--json] [-s]
{: #cs_log_filter_view}

檢視記載過濾器配置。您可以使用這個指令來檢視您所建立的記載過濾器。

<strong>指令選項</strong>：

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>您要檢視其過濾器之叢集的名稱或 ID。這是必要值。</dd>

  <dt><code>--id <em>FILTER_ID</em></code></dt>
    <dd>您要檢視的日誌過濾器 ID。</dd>

  <dt><code>--show-matching-configs</code></dt>
    <dd>顯示符合您正在檢視之配置的記載配置。這是選用值。</dd>

  <dt><code>--show-covering-filters</code></dt>
    <dd>顯示將先前過濾器呈現為已作廢的記載過濾器。這是選用值。</dd>

  <dt><code>--json</code></dt>
    <dd>以 JSON 格式列印指令輸出。這是選用值。</dd>

   <dt><code>-s</code></dt>
     <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
</dl>


### bx cs logging-filter-rm CLUSTER [--id FILTER_ID][--all] [-s]
{: #cs_log_filter_delete}

刪除記載過濾器。您可以使用這個指令來移除您所建立的記載過濾器。

<strong>指令選項</strong>：

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>您要從中刪除過濾器之叢集的名稱或 ID。</dd>

  <dt><code>--id <em>FILTER_ID</em></code></dt>
    <dd>要刪除的日誌過濾器 ID。</dd>

  <dt><code>--all</code></dt>
    <dd>刪除所有日誌轉遞過濾器。這是選用值。</dd>

  <dt><code>-s</code></dt>
    <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
</dl>


### bx cs logging-filter-update CLUSTER --id FILTER_ID --type LOG_TYPE [--logging-configs CONFIGS][--namespace KUBERNETES_NAMESPACE] [--container CONTAINER_NAME][--level LOGGING_LEVEL] [--message MESSAGE][--json] [-s]
{: #cs_log_filter_update}

更新記載過濾器。您可以使用這個指令來更新您所建立的記載過濾器。

<strong>指令選項</strong>：

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>您要更新其記載過濾器之叢集的名稱或 ID。這是必要值。</dd>

 <dt><code>--id <em>FILTER_ID</em></code></dt>
    <dd>要更新的日誌過濾器 ID。</dd>

  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>您要將過濾器套用至其中的日誌類型。目前支援 <code>all</code>、<code>container</code> 及 <code>host</code>。</dd>

  <dt><code>--logging-configs <em>CONFIGS</em></code></dt>
    <dd>以逗點區隔的記載配置 ID 清單。如果未提供，過濾器會套用至傳遞給過濾器的所有叢集記載配置。您可以使用 <code>--show-matching-configs</code> 旗標與指令搭配，來檢視符合過濾器的日誌配置。這是選用值。</dd>

  <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
    <dd>您要從中過濾日誌的 Kubernetes 名稱空間。這是選用值。</dd>

  <dt><code>--container <em>CONTAINER_NAME</em></code></dt>
    <dd>您要從中過濾掉日誌的容器名稱。只有在您使用日誌類型 <code>container</code> 時，此旗標才適用。這是選用值。</dd>

  <dt><code>--level <em>LOGGING_LEVEL</em></code></dt>
    <dd>過濾掉指定層次以下的日誌。可接受的值依其標準順序為 <code>fatal</code>、<code>error</code>、<code>warn/warning</code>、<code>info</code>、<code>debug</code> 及 <code>trace</code>。這是選用值。舉例來說，如果您過濾掉 <code>info</code> 層次的日誌，則也會過濾掉 <code>debug</code> 及 <code>trace</code>。**附註**：只有在日誌訊息是 JSON 格式且包含 level 欄位時，才能使用此旗標。輸出範例：<code>{"log": "hello", "level": "info"}</code></dd>

  <dt><code>--message <em>MESSAGE</em></code></dt>
    <dd>過濾掉日誌中任何位置包含指定訊息的任何日誌。訊息會逐字比對，而不是以表示式的方式比對。範例：訊息 "Hello"、"!" 及 "Hello, World!" 會適用於日誌 "Hello, World!"。這是選用值。</dd>

  <dt><code>--json</code></dt>
    <dd>以 JSON 格式列印指令輸出。這是選用值。</dd>

  <dt><code>-s</code></dt>
    <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
</dl>




<br />


## 地區指令
{: #region_commands}

### bx cs locations [--json][-s]
{: #cs_datacenters}

檢視可讓您在其中建立叢集的可用位置清單。可用位置因您登入的地區而異。若要切換地區，請執行 `bx cs region-set`。

<strong>指令選項</strong>：

   <dl>
   <dt><code>--json</code></dt>
   <dd>以 JSON 格式列印指令輸出。這是選用值。</dd>

   <dt><code>-s</code></dt>
   <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
   </dl>

**範例**：

  ```
  bx cs locations
  ```
  {: pre}


### bx cs region
{: #cs_region}

尋找您目前所在的 {{site.data.keyword.containershort_notm}} 地區。您可以建立及管理地區特定的叢集。使用 `bx cs region-set` 指令來變更地區。

**範例**：

```
bx cs region
```
{: pre}

**輸出**：
```
Region: us-south
```
{: screen}

### bx cs region-set [REGION]
{: #cs_region-set}

設定 {{site.data.keyword.containershort_notm}} 的地區。您可以建立及管理地區特定的叢集，而且可能要多個地區中都有叢集，才能獲得高可用性。

例如，您可以在美國南部地區登入 {{site.data.keyword.Bluemix_notm}}，並建立叢集。接下來，您可以使用 `bx cs region-set eu-central` 以將目標設為歐盟中部地區，並建立另一個叢集。最後，您可以使用 `bx cs region-set us-south` 回到美國南部，以管理該地區中的叢集。

**指令選項**：

<dl>
<dt><code><em>REGION</em></code></dt>
<dd>輸入您要設為目標的地區。這是選用值。如果您未提供地區，則可以從輸出的清單中選取該地區。



如需可用地區清單，請檢閱[地區及位置](cs_regions.html)，或使用 `bx cs regions` [指令](#cs_regions)。</dd></dl>

**範例**：

```
bx cs region-set eu-central
```
{: pre}

```
bx cs region-set
```
{: pre}

**輸出**：
```
Choose a region:
1. ap-north
2. ap-south
3. eu-central
4. uk-south
5. us-east
6. us-south
Enter a number> 3
OK
```
{: screen}

### bx cs regions
{: #cs_regions}

列出可用的地區。`Region Name` 是 {{site.data.keyword.containershort_notm}} 名稱，而 `Region Alias` 是地區的一般 {{site.data.keyword.Bluemix_notm}} 名稱。

**範例**：

```
bx cs regions
```
{: pre}

**輸出**：
```
Region Name   Region Alias
ap-north      jp-tok
ap-south      au-syd
eu-central    eu-de
uk-south      eu-gb
us-east       us-east
us-south      us-south
```
{: screen}


<br />


## 工作者節點指令
{: worker_node_commands}


### bx cs worker-add --cluster CLUSTER [--file FILE_LOCATION][--hardware HARDWARE] --machine-type MACHINE_TYPE --number NUMBER --private-vlan PRIVATE_VLAN --public-vlan PUBLIC_VLAN [--disable-disk-encrypt][-s]
{: #cs_worker_add}

將工作者節點新增至標準叢集。



<strong>指令選項</strong>：

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>叢集的名稱或 ID。這是必要值。</dd>

<dt><code>--file <em>FILE_LOCATION</em></code></dt>
<dd>將工作者節點新增至叢集之 YAML 檔案的路徑。您可以使用 YAML 檔案，而不是使用此指令中所提供的選項來定義其他工作者節點。這是選用值。

<p><strong>附註：</strong>如果您在指令中提供與 YAML 檔案中的參數相同的選項，則指令中的值優先順序會高於 YAML 中的值。例如，您在 YAML 檔案中定義了機型，並在指令中使用 --machine-type 選項，則您在指令選項中輸入的值會置換 YAML 檔案中的值。



<pre class="codeblock">
<code>name: <em>&lt;cluster_name_or_ID&gt;</em>
location: <em>&lt;location&gt;</em>
machine-type: <em>&lt;machine_type&gt;</em>
private-vlan: <em>&lt;private_VLAN&gt;</em>
public-vlan: <em>&lt;public_VLAN&gt;</em>
hardware: <em>&lt;shared_or_dedicated&gt;</em>
workerNum: <em>&lt;number_workers&gt;</em>
diskEncryption: <em>false</em></code></pre>

<table>
<caption>瞭解 YAML 檔案元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解 YAML 檔案元件</th>
</thead>
<tbody>
<tr>
<td><code><em>name</em></code></td>
<td>將 <code><em>&lt;cluster_name_or_ID&gt;</em></code> 取代為您要新增工作者節點之叢集的名稱或 ID。</td>
</tr>
<tr>
<td><code><em>location</em></code></td>
<td>將 <code><em>&lt;location&gt;</em></code> 取代為您要部署工作者節點的位置。可用的位置取決於您所登入的地區。若要列出可用的位置，請執行 <code>bx cs locations</code>。</td>
</tr>
<tr>
<td><code><em>machine-type</em></code></td>
<td>將 <code><em>&lt;machine_type&gt;</em></code> 取代為您要將工作者節點部署至其中的機型。您可以將工作者節點部署為共用或專用硬體上的虛擬機器，或部署為裸機上的實體機器。可用的實體及虛擬機器類型會因您部署叢集所在的位置而不同。如需相關資訊，請參閱 `bx cs machine-types` [指令](cs_cli_reference.html#cs_machine_types)。</td>
</tr>
<tr>
<td><code><em>private-vlan</em></code></td>
<td>將 <code><em>&lt;private_VLAN&gt;</em></code> 取代為您要用於工作者節點的專用 VLAN ID。若要列出可用的 VLAN，請執行 <code>bx cs vlans <em>&lt;location&gt;</em></code>，並尋找開頭為 <code>bcr</code>（後端路由器）的 VLAN 路由器。</td>
</tr>
<tr>
<td><code>public-vlan</code></td>
<td>將 <code>&lt;public_VLAN&gt;</code> 取代為您要用於工作者節點的公用 VLAN ID。若要列出可用的 VLAN，請執行 <code>bx cs vlans &lt;location&gt;</code>，並尋找開頭為 <code>fcr</code>（前端路由器）的 VLAN 路由器。<br><strong>附註</strong>：{[private_VLAN_vyatta]}</td>
</tr>
<tr>
<td><code>hardware</code></td>
<td>若為虛擬機器類型：您的工作者節點的硬體隔離層次。如果您希望可用的實體資源只供您專用，請使用 dedicated，或者，若要容許與其他 IBM 客戶共用實體資源，請使用 shared。預設值為 shared。</td>
</tr>
<tr>
<td><code>workerNum</code></td>
<td>將 <code><em>&lt;number_workers&gt;</em></code> 取代為您要部署的工作者節點數目。</td>
</tr>
<tr>
<td><code>diskEncryption: <em>false</em></code></td>
<td>依預設，工作者節點會具備磁碟加密；[進一步瞭解](cs_secure.html#worker)。若要停用加密，請包含此選項，並將值設為 <code>false</code>。</td></tr>
</tbody></table></p></dd>

<dt><code>--hardware <em>HARDWARE</em></code></dt>
<dd>工作者節點的硬體隔離層次。如果您希望可用的實體資源只供您專用，請使用 dedicated，或者，若要容許與其他 IBM 客戶共用實體資源，請使用 shared。預設值為 shared。這是選用值。</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>選擇機型。您可以將工作者節點部署為共用或專用硬體上的虛擬機器，或部署為裸機上的實體機器。可用的實體及虛擬機器類型會因您部署叢集所在的位置而不同。如需相關資訊，請參閱 `bx cs machine-types` [指令](cs_cli_reference.html#cs_machine_types) 的文件。此值對於標準叢集是必要的，不適用於免費叢集。</dd>

<dt><code>--number <em>NUMBER</em></code></dt>
<dd>整數，代表要在叢集中建立的工作者節點數目。預設值為 1。這是選用值。</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>建立叢集時所指定的專用 VLAN。這是必要值。

<p><strong>附註：</strong>專用 VLAN 路由器的開頭一律為 <code>bcr</code>（後端路由器），而公用 VLAN 路由器的開頭一律為 <code>fcr</code>（前端路由器）。當建立叢集並指定公用和專用 VLAN 時，那些字首之後的數字和字母組合必須相符。</p></dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>建立叢集時所指定的公用 VLAN。這是選用值。如果想要工作者節點僅存在於專用 VLAN 上，請不要提供公用 VLAN ID。<strong>附註</strong>：{[private_VLAN_vyatta]}

<p><strong>附註：</strong>專用 VLAN 路由器的開頭一律為 <code>bcr</code>（後端路由器），而公用 VLAN 路由器的開頭一律為 <code>fcr</code>（前端路由器）。當建立叢集並指定公用和專用 VLAN 時，那些字首之後的數字和字母組合必須相符。</p></dd>

<dt><code>--disable-disk-encrypt</code></dt>
<dd>依預設，工作者節點會具備磁碟加密；[進一步瞭解](cs_secure.html#worker)。若要停用加密，請包含此選項。</dd>

<dt><code>-s</code></dt>
<dd>不要顯示日或更新提示的訊息。這是選用值。</dd>

</dl>

**範例**：

  ```
  bx cs worker-add --cluster my_cluster --number 3 --public-vlan my_public_VLAN_ID --private-vlan my_private_VLAN_ID --machine-type u2c.2x4 --hardware shared
  ```
  {: pre}

  {{site.data.keyword.Bluemix_dedicated_notm}} 的範例：

  ```
  bx cs worker-add --cluster my_cluster --number 3 --machine-type u2c.2x4
  ```
  {: pre}

 部署叢集的位置。如需相關資訊，請參閱 `bx cs machine-types` [指令](cs_cli_reference.html#cs_machine_types) 的文件。此值對於標準叢集是必要的，不適用於免費叢集。</dd>

  <dt><code>--size-per-zone <em>WORKERS_PER_ZONE</em></code></dt>
    <dd>要在每一個區域中建立的工作者數目。這是必要值。</dd>

  <dt><code>--kube-version <em>VERSION</em></code></dt>
    <dd>您要用來建立工作者節點的 Kubernetes 版本。如果未指定此值，則會使用預設版本。</dd>

  <dt><code>--hardware <em>HARDWARE</em></code></dt>
    <dd>工作者節點的硬體隔離層次。如果您希望可用的實體資源只供您專用，請使用 dedicated，或者，若要容許與其他 IBM 客戶共用實體資源，請使用 shared。預設值為 shared。這是選用值。</dd>

  <dt><code>--labels <em>LABELS</em></code></dt>
    <dd>您要在儲存區中指派給工作者的標籤。範例：<key1>=<val1>,<key2>=<val2></dd>

  <dt><code>--private-only </code></dt>
    <dd>指定工作者儲存區上沒有公用 VLAN。預設值為 <code>false</code>。</dd>

  <dt><code>--diable-disk-encrpyt</code></dt>
    <dd>指定磁碟未加密。預設值為 <code>false</code>。</dd>

</dl>

**指令範例**：

  ```
  bx cs worker-pool-add my_cluster --machine-type u2c.2x4 --size-per-zone 6
  ```
  {: pre}

### bx cs worker-pools --cluster CLUSTER
{: #cs_worker_pools}

檢視您在叢集中具有的工作者儲存區。

<strong>指令選項</strong>：

<dl>
  <dt><code>--cluster <em>CLUSTER_NAME_OR_ID</em></code></dt>
    <dd>您要列出其工作者儲存區之叢集的名稱或 ID。這是必要值。</dd>
</dl>

**指令範例**：

  ```
  bx cs worker-pools --cluster my_cluster
  ```
  {: pre}

### bx cs worker-pool-get --worker-pool WORKER_POOL --cluster CLUSTER
{: #cs_worker_pool_get}

檢視工作者儲存區的詳細資料。

<strong>指令選項</strong>：

<dl>
  <dt><code>--worker-pool <em>WORKER_POOL</em></code></dt>
    <dd>您要檢視其詳細資料之工作者節點儲存區的名稱。這是必要值。</dd>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>工作者儲存區所在之叢集的名稱或 ID。這是必要值。</dd>
</dl>

**指令範例**：

  ```
  bx cs worker-pool-get --worker-pool pool1 --cluster my_cluster
  ```
  {: pre}

### bx cs worker-pool-update --worker-pool WORKER_POOL --cluster CLUSTER
{: #cs_worker_pool_update}

將您儲存區中的所有工作者節點更新為符合所指定主節點的最新 Kubernet 版本。

<strong>指令選項</strong>：

<dl>
  <dt><code>--worker-pool <em>WORKER_POOL</em></code></dt>
    <dd>您要更新之工作者節點儲存區的名稱。這是必要值。</dd>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>您要更新其工作者儲存區之叢集的名稱或 ID。這是必要值。</dd>
</dl>

**指令範例**：

  ```
  bx cs worker-pool-update --worker-pool pool1 --cluster my_cluster
  ```
  {: pre}



### bx cs worker-pool-resize --worker-pool WORKER_POOL --cluster CLUSTER --size-per-zone WORKERS_PER_ZONE
{: #cs_worker_pool_resize}

調整工作者儲存區的大小，以增加或減少叢集之每一個區域中的工作者節點數目。

<strong>指令選項</strong>：

<dl>
  <dt><code>--worker-pool <em>WORKER_POOL</em></code></dt>
    <dd>您要更新之工作者節點儲存區的名稱。這是必要值。</dd>

  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>您要調整其工作者儲存區大小之叢集的名稱或 ID。這是必要值。</dd>

  <dt><code>--size-per-zone <em>WORKERS_PER_ZONE</em></code></dt>
    <dd>您要在每一個區域中建立的工作者數目。這是必要值。</dd>
</dl>

**指令範例**：

  ```
  bx cs worker-pool-update --cluster my_cluster --worker-pool pool1,pool2 --size-per-zone 3
  ```
  {: pre}

### bx cs worker-pool-rm --worker-pool WORKER_POOL --cluster CLUSTER
{: #cs_worker_pool_rm}

從叢集中移除工作者儲存區。這會刪除儲存區中的所有工作者節點。刪除時會重新排定您的 Pod。為了避免當機，請確定您有足夠的工作者來執行工作負載。

<strong>指令選項</strong>：

<dl>
  <dt><code>--worker-pool <em>WORKER_POOL</em></code></dt>
    <dd>您要移除之工作者節點儲存區的名稱。這是必要值。</dd>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>您要從中移除工作者儲存區之叢集的名稱或 ID。這是必要值。</dd>
</dl>

**指令範例**：

  ```
  bx cs worker-pool-rm --cluster my_cluster --worker-pool pool1
  ```
  {: pre}

</staging>
    ### bx cs worker-get [CLUSTER_NAME_OR_ID] WORKER_NODE_ID [--json][-s]
{: #cs_worker_get}

檢視工作者節點的詳細資料。

<strong>指令選項</strong>：

   <dl>
   <dt><code><em>CLUSTER_NAME_OR_ID</em></code></dt>
   <dd>工作者節點叢集的名稱或 ID。這是選用值。</dd>

   <dt><code><em>WORKER_NODE_ID</em></code></dt>
   <dd>工作者節點的名稱。執行 <code>bx cs workers <em>CLUSTER</em></code>，以檢視叢集中工作者節點的 ID。這是必要值。</dd>

   <dt><code>--json</code></dt>
   <dd>以 JSON 格式列印指令輸出。這是選用值。</dd>

   <dt><code>-s</code></dt>
   <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
   </dl>

**指令範例**：

  ```
  bx cs worker-get my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1
  ```
  {: pre}

**輸出範例**：

  ```
  ID:           kube-dal10-123456789-w1
  State:        normal
  Status:       Ready
  Trust:        disabled
  Private VLAN: 223xxxx
  Public VLAN:  223xxxx
  Private IP:   10.xxx.xx.xxx
  Public IP:    169.xx.xxx.xxx
  Hardware:     shared
  Zone:         dal10
  Version:      1.8.11_1509
  ```
  {: screen}

### bx cs worker-reboot [-f][--hard] CLUSTER WORKER [WORKER][-s]
{: #cs_worker_reboot}

重新啟動叢集中的工作者節點。在重新啟動期間，工作者節點的狀態不會變更。

**注意：**重新啟動工作者節點，可能會造成工作者節點上的資料毀損。當您知道重新啟動可以協助回復工作者節點時，請小心使用此指令。除此之外，請改為[重新載入工作者節點](#cs_worker_reload)。

在您重新啟動工作者節點之前，請確定在其他工作者節點上的 Pod 已重新排程，這有助於避免應用程式關閉，或工作者節點上的資料毀損。

1. 列出叢集中的所有工作者節點，並記下您要重新啟動的工作者節點的**名稱**。
   ```
   kubectl get nodes
   ```
   此指令傳回的**名稱**是指派給工作者節點的專用 IP 位址。您可以執行 `bx cs Workers <cluster_name_or_ID>` 指令並尋找具有相同**專用 IP** 位址的工作者節點，來找到工作者節點的相關資訊。
2. 在一個稱為隔離的處理程序中，將工作者節點標示為無法排程。當您隔離工作者節點時，會使它無法用於未來 Pod 排程。請使用您在前一個步驟中擷取之工作者節點的**名稱**。
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. 驗證已停用工作者節點的 Pod 排程。
   ```
   kubectl get nodes
   ```
   {: pre}
   如果狀態顯示 **SchedulingDisabled**，表示工作者節點已停用 Pod 排程。
 4. 強制從工作者節點中移除 Pod，並將其重新排程至叢集中的其餘工作者節點。
    ```
    kubectl drain <worker_name>
    ```
    {: pre}
    此處理程序可能需要幾分鐘的時間。
 5. 重新啟動工作者節點。請使用從 `bx cs workers <cluster_name_or_ID>` 指令傳回的工作者節點 ID。
    ```
    bx cs worker-reboot <cluster_name_or_ID> <worker_name_or_ID>
    ```
    {: pre}
 6. 在讓工作者節點可用於 pod 排程之前等待大約 5 分鐘，以確定重新啟動已完成。在重新啟動期間，工作者節點的狀態不會變更。工作者節點的重新啟動通常會在幾秒鐘內完成。
 7. 讓工作者節點可用於 Pod 排程。請使用從 `kubectl get node` 指令傳回的工作者節點的**名稱**。
    ```
    kubectl uncordon <worker_name>
    ```
    {: pre}
    </br>

<strong>指令選項</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>叢集的名稱或 ID。這是必要值。</dd>

   <dt><code>-f</code></dt>
   <dd>使用此選項，以強制重新啟動工作者節點，而不出現任何使用者提示。這是選用值。</dd>

   <dt><code>--hard</code></dt>
   <dd>使用此選項，透過切斷工作者節點的電源來強制執行強迫重新啟動工作者節點。如果工作者節點無回應，或工作者節點的 Docker 當掉，請使用此選項。這是選用值。</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>一個以上工作者節點的名稱或 ID。請使用空格來列出多個工作者節點。這是必要值。</dd>

   <dt><code>-s</code></dt>
   <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
   </dl>

**範例**：

  ```
  bx cs worker-reboot my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}


### bx cs worker-reload [-f] CLUSTER WORKER [WORKER][-s]
{: #cs_worker_reload}

重新載入工作者節點的所有必要配置。如果您的工作者節點遇到問題，例如效能變慢，或工作者節點停留在不健全的狀態下，則重新載入可能會很有幫助。

重新載入工作者節點會將修補程式版本更新套用至您的工作者節點，但不會套用主要或次要更新。若要查看從某個修補程式版本到下一個修補程式版本的變更，請檢閱[版本變更日誌](cs_versions_changelog.html#changelog)文件。
{: tip}

在您重新載入工作者節點之前，請確定在其他工作者節點上的 Pod 已重新排程，這有助於避免應用程式關閉，或工作者節點上的資料毀損。

1. 列出叢集中的所有工作者節點，並記下您要重新載入的工作者節點的**名稱**。
   ```
   kubectl get nodes
   ```
   此指令傳回的**名稱**是指派給工作者節點的專用 IP 位址。您可以執行 `bx cs Workers <cluster_name_or_ID>` 指令並尋找具有相同**專用 IP** 位址的工作者節點，來找到工作者節點的相關資訊。
2. 在一個稱為隔離的處理程序中，將工作者節點標示為無法排程。當您隔離工作者節點時，會使它無法用於未來 Pod 排程。請使用您在前一個步驟中擷取之工作者節點的**名稱**。
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. 驗證已停用工作者節點的 Pod 排程。
   ```
   kubectl get nodes
   ```
   {: pre}
   如果狀態顯示 **SchedulingDisabled**，表示工作者節點已停用 Pod 排程。
 4. 強制從工作者節點中移除 Pod，並將其重新排程至叢集中的其餘工作者節點。
    ```
    kubectl drain <worker_name>
    ```
    {: pre}
    此處理程序可能需要幾分鐘的時間。
 5. 重新載入工作者節點。請使用從 `bx cs workers <cluster_name_or_ID>` 指令傳回的工作者節點 ID。
    ```
    bx cs worker-reload <cluster_name_or_ID> <worker_name_or_ID>
    ```
    {: pre}
 6. 等待重新載入完成。
 7. 讓工作者節點可用於 Pod 排程。請使用從 `kubectl get node` 指令傳回的工作者節點的**名稱**。
    ```
    kubectl uncordon <worker_name>
    ```
</br>
<strong>指令選項</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>叢集的名稱或 ID。這是必要值。</dd>

   <dt><code>-f</code></dt>
   <dd>使用此選項，以強制重新載入工作者節點，而不出現任何使用者提示。這是選用值。</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>一個以上工作者節點的名稱或 ID。請使用空格來列出多個工作者節點。這是必要值。</dd>

   <dt><code>-s</code></dt>
   <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
   </dl>

**範例**：

  ```
  bx cs worker-reload my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}


### bx cs worker-rm [-f] CLUSTER WORKER [WORKER][-s]
{: #cs_worker_rm}

從叢集中移除一個以上的工作者節點。如果您移除工作者節點，叢集會變成不平衡。 

在您移除工作者節點之前，請確定在其他工作者節點上的 Pod 已重新排程，這有助於避免應用程式關閉，或工作者節點上的資料毀損。
{: tip}

1. 列出叢集中的所有工作者節點，並記下您要移除的工作者節點的**名稱**。
   ```
   kubectl get nodes
   ```
   此指令傳回的**名稱**是指派給工作者節點的專用 IP 位址。您可以執行 `bx cs Workers <cluster_name_or_ID>` 指令並尋找具有相同**專用 IP** 位址的工作者節點，來找到工作者節點的相關資訊。
2. 在一個稱為隔離的處理程序中，將工作者節點標示為無法排程。當您隔離工作者節點時，會使它無法用於未來 Pod 排程。請使用您在前一個步驟中擷取之工作者節點的**名稱**。
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. 驗證已停用工作者節點的 Pod 排程。
   ```
   kubectl get nodes
   ```
   {: pre}
   如果狀態顯示 **SchedulingDisabled**，表示工作者節點已停用 Pod 排程。
4. 強制從工作者節點中移除 Pod，並將其重新排程至叢集中的其餘工作者節點。
   ```
   kubectl drain <worker_name>
   ```
   {: pre}
   此處理程序可能需要幾分鐘的時間。
5. 移除工作者節點。請使用從 `bx cs workers <cluster_name_or_ID>` 指令傳回的工作者節點 ID。
   ```
   bx cs worker-rm <cluster_name_or_ID> <worker_name_or_ID>
   ```
   {: pre}

6. 驗證已移除工作者節點。
   ```
       bx cs workers <cluster_name_or_ID>
       ```
</br>
<strong>指令選項</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>叢集的名稱或 ID。這是必要值。</dd>

   <dt><code>-f</code></dt>
   <dd>使用此選項，以強制移除工作者節點，而不出現任何使用者提示。這是選用值。</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>一個以上工作者節點的名稱或 ID。請使用空格來列出多個工作者節點。這是必要值。</dd>

   <dt><code>-s</code></dt>
   <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
   </dl>

**範例**：

  ```
  bx cs worker-rm my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}


###bx cs worker-update [-f] CLUSTER WORKER [WORKER][--kube-version MAJOR.MINOR.PATCH] [--force-update][-s]
{: #cs_worker_update}

更新工作者節點以將最新的安全更新及修補程式套用至作業系統，並更新 Kubernetes 版本以符合主要節點的版本。您可以使用 `bx cs cluster-update` [指令](cs_cli_reference.html#cs_cluster_update)來更新主要節點 Kubernetes 版本。



**重要事項**：執行 `bx cs worker-update` 可能會導致應用程式及服務的關閉時間。在更新期間，所有 Pod 會重新排定到其他工作者節點，且資料若未儲存在 Pod 之外便會刪除。若要避免關閉時間，[請確定您有足夠的工作者節點，可以處理所選取工作者節點在更新時的工作負載](cs_cluster_update.html#worker_node)。

您可能需要變更 YAML 檔案以進行部署，然後才更新。如需詳細資料，請檢閱此[版本注意事項](cs_versions.html)。

<strong>指令選項</strong>：

   <dl>

   <dt><em>CLUSTER</em></dt>
   <dd>列出可用工作者節點的叢集的名稱或 ID。這是必要值。</dd>

   <dt><code>-f</code></dt>
   <dd>使用此選項，以強制更新主節點，而不出現任何使用者提示。這是選用值。</dd>

   <dt><code>--force-update</code></dt>
   <dd>即使變更大於兩個次要版本，也要嘗試更新。這是選用值。</dd>

   <dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
     <dd>您要用來更新工作者節點的 Kubernetes 版本。如果未指定此值，則會使用預設版本。</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>一個以上工作者節點的 ID。請使用空格來列出多個工作者節點。這是必要值。</dd>

   <dt><code>-s</code></dt>
   <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>

   </dl>

**範例**：

  ```
  bx cs worker-update my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}


### bx cs workers CLUSTER [--show-deleted][--json] [-s]
{: #cs_workers}

檢視工作者節點清單以及叢集中各工作者節點的狀態。

<strong>指令選項</strong>：

   <dl>
   <dt><em>CLUSTER</em></dt>
   <dd>可用工作者節點之叢集的名稱或 ID。這是必要值。</dd>

   <dt><em>--show-deleted</em></dt>
   <dd>檢視已從叢集中刪除的工作者節點，包括刪除的原因。這是選用值。</dd>

   <dt><code>--json</code></dt>
   <dd>以 JSON 格式列印指令輸出。這是選用值。</dd>

  <dt><code>-s</code></dt>
  <dd>不要顯示日或更新提示的訊息。這是選用值。</dd>
   </dl>

**範例**：

  ```
  bx cs workers my_cluster
  ```
  {: pre}
