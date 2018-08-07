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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# 叢集網路的疑難排解
{: #cs_troubleshoot_network}

在您使用 {{site.data.keyword.containerlong}} 時，請考慮使用這些技術來進行叢集網路的疑難排解。
{: shortdesc}

如果您有更一般性的問題，請嘗試[叢集除錯](cs_troubleshoot.html)。
{: tip}


## 無法透過負載平衡器服務連接至應用程式
{: #cs_loadbalancer_fails}

{: tsSymptoms}
您已透過在叢集中建立負載平衡器服務，來公開應用程式。當您嘗試使用負載平衡器的公用 IP 位址連接至應用程式時，連線失敗或逾時。

{: tsCauses}
負載平衡器服務可能因下列其中一個原因而未正常運作：

-   叢集是免費叢集或只有一個工作者節點的標準叢集。
-   尚未完整部署叢集。
-   負載平衡器服務的配置 Script 包含錯誤。

{: tsResolve}
若要對負載平衡器服務進行疑難排解，請執行下列動作：

1.  確認您所設定的標準叢集已完整部署並且至少有兩個工作者節點，以確保負載平衡器服務的高可用性。

  ```
  bx cs workers <cluster_name_or_ID>
  ```
  {: pre}

    在 CLI 輸出中，確定工作者節點的 **Status** 顯示 **Ready**，而且 **Machine Type** 顯示 **free** 以外的機型。

2.  檢查負載平衡器服務配置檔的正確性。

    ```
    apiVersion: v1
    kind: Service
    metadata:
      name: myservice
    spec:
      type: LoadBalancer
      selector:
        <selector_key>:<selector_value>
      ports:
       - protocol: TCP
         port: 8080
    ```
    {: pre}

    1.  確認您已將 **LoadBalancer** 定義為服務的類型。
    2.  在 LoadBalancer 服務的 `spec.selector` 區段中，確定 `<selector_key>` 及 `<selector_value>` 與您在部署 yaml 的 `spec.template.metadata.labels` 區段中使用的鍵值組相同。如果標籤不符，則 LoadBalancer 服務中的 **Endpoints** 區段會顯示 **<none>**，且無法從網際網路存取您的應用程式。
    3.  確認您已使用應用程式所接聽的**埠**。

3.  檢查負載平衡器服務，並檢閱 **Events** 區段來尋找可能的錯誤。

    ```
    kubectl describe service <myservice>
    ```
    {: pre}

    尋找下列錯誤訊息：

    <ul><li><pre class="screen"><code>Clusters with one node must use services of type NodePort</code></pre></br>若要使用負載平衡器服務，您必須有至少包含兩個工作者節點的標準叢集。</li>
    <li><pre class="screen"><code>No cloud provider IPs are available to fulfill the load balancer service request. Add a portable subnet to the cluster and try again</code></pre></br>此錯誤訊息指出未將可攜式公用 IP 位址配置給負載平衡器服務。請參閱<a href="cs_subnets.html#subnets">將子網路新增至叢集</a>，以尋找如何要求叢集之可攜式公用 IP 位址的相關資訊。叢集可以使用可攜式公用 IP 位址之後，會自動建立負載平衡器服務。
    </li>
    <li><pre class="screen"><code>Requested cloud provider IP <cloud-provider-ip> is not available. The following cloud provider IPs are available: <available-cloud-provider-ips></code></pre></br>您已使用 **loadBalancerIP** 區段定義負載平衡器服務的可攜式公用 IP 位址，但在可攜式公用子網路中無法使用此可攜式公用 IP 位址。在配置 Script 的 **loadBalancerIP** 區段中，移除現有 IP 位址，並新增其中一個可用的可攜式公用 IP 位址。您也可以移除 Script 中的 **loadBalancerIP** 區段，以自動配置可用的可攜式公用 IP 位址。</li>
    <li><pre class="screen"><code>No available nodes for load balancer services</code></pre>您沒有足夠的工作者節點可部署負載平衡器服務。其中一個原因可能是您所部署的標準叢集有多個工作者節點，但佈建工作者節點失敗。
    </li>
    <ol><li>列出可用的工作者節點。</br><pre class="codeblock"><code>kubectl get nodes</code></pre></li>
    <li>如果找到至少兩個可用的工作者節點，則會列出工作者節點詳細資料。</br><pre class="codeblock"><code>bx cs worker-get [&lt;cluster_name_or_ID&gt;] &lt;worker_ID&gt;</code></pre></li>
    <li>確定 <code>kubectl get nodes</code> 及 <code>bx cs [&lt;cluster_name_or_ID&gt;] worker-get</code> 指令所傳回的工作者節點的公用及專用 VLAN ID 相符。</li></ol></li></ul>

4.  如果您要使用自訂網域連接至負載平衡器服務，請確定已將自訂網域對映至負載平衡器服務的公用 IP 位址。
    1.  尋找負載平衡器服務的公用 IP 位址。

        ```
        kubectl describe service <service_name> | grep "LoadBalancer Ingress"
        ```
        {: pre}

    2.  確認在「指標記錄 (PTR)」中已將自訂網域對映至負載平衡器服務的可攜式公用 IP 位址。

<br />




## 無法透過 Ingress 連接至應用程式
{: #cs_ingress_fails}

{: tsSymptoms}
您已透過在叢集中建立應用程式的 Ingress 資源，來公開應用程式。當您嘗試使用 Ingress 應用程式負載平衡器 (ALB) 的公用 IP 位址或子網域連接至應用程式時，連線失敗或逾時。

{: tsCauses}
Ingress 可能未正常運作，原因如下：
<ul><ul>
<li>尚未完整部署叢集。
<li>叢集已設定為免費叢集或只有一個工作者節點的標準叢集。
<li>Ingress 配置 Script 包含錯誤。
</ul></ul>

{: tsResolve}
若要對 Ingress 進行疑難排解，請執行下列動作：

1.  確認您所設定的標準叢集已完整部署並且至少有兩個工作者節點，以確保 ALB 的高可用性。

  ```
  bx cs workers <cluster_name_or_ID>
  ```
  {: pre}

    在 CLI 輸出中，確定工作者節點的 **Status** 顯示 **Ready**，而且 **Machine Type** 顯示 **free** 以外的機型。

2.  擷取 ALB 子網域及公用 IP 位址，然後對其進行連線測試。

    1.  擷取 ALB 子網域。

      ```
      bx cs cluster-get <cluster_name_or_ID> | grep "Ingress subdomain"
      ```
      {: pre}

    2.  對 ALB 子網域進行連線測試。

      ```
      ping <ingress_subdomain>
      ```
      {: pre}

    3.  擷取 ALB 的公用 IP 位址。

      ```
      nslookup <ingress_subdomain>
      ```
      {: pre}

    4.  對 ALB 公用 IP 位址進行連線測試。

      ```
      ping <ALB_IP>
      ```
      {: pre}

    如果 CLI 傳回 ALB 公用 IP 位址或子網域的逾時，並且已設定保護工作者節點的自訂防火牆，請開啟[防火牆](cs_troubleshoot_clusters.html#cs_firewall)中的其他埠及網路群組。

3.  如果您要使用自訂網域，請確定使用 DNS 提供者將自訂網域對映至 IBM 所提供 ALB 的公用 IP 位址或子網域。
    1.  如果您已使用 ALB 子網域，請檢查「標準名稱記錄 (CNAME)」。
    2.  如果您已使用 ALB 公用 IP 位址，請確認已在「指標記錄 (PTR)」中將自訂網域對映至可攜式公用 IP 位址。
4.  檢查 Ingress 資源配置檔。

    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingress
    spec:
      tls:
      - hosts:
        - <ingress_subdomain>
        secretName: <ingress_tls_secret>
      rules:
      - host: <ingress_subdomain>
        http:
          paths:
          - path: /
            backend:
              serviceName: myservice
              servicePort: 80
    ```
    {: codeblock}

    1.  確認 ALB 子網域及 TLS 憑證正確無誤。若要尋找 IBM 提供的子網域及 TLS 憑證，請執行 `bx cs cluster-get<cluster_name_or_ID>`。
    2.  確定應用程式接聽與 Ingress 之 **path** 區段中配置相同的路徑。如果您的應用程式設定成接聽根路徑，請包含 **/** 作為路徑。
5.  檢查 Ingress 部署，並尋找可能的警告或錯誤訊息。

    ```
    kubectl describe ingress <myingress>
    ```
    {: pre}

    例如，在輸出的 **Events** 區段中，您可能會看到關於您使用之 Ingress 資源或某些註釋中含有無效值的警告訊息。

    ```
    Name:             myingress
    Namespace:        default
    Address:          169.xx.xxx.xxx,169.xx.xxx.xxx
    Default backend:  default-http-backend:80 (<none>)
    Rules:
      Host                                             Path  Backends
      ----                                             ----  --------
      mycluster.us-south.containers.appdomain.cloud
                                                       /tea      myservice1:80 (<none>)
                                                       /coffee   myservice2:80 (<none>)
    Annotations:
      custom-port:        protocol=http port=7490; protocol=https port=4431
      location-modifier:  modifier='~' serviceName=myservice1;modifier='^~' serviceName=myservice2
    Events:
      Type     Reason             Age   From                                                            Message
      ----     ------             ----  ----                                                            -------
      Normal   Success            1m    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  TLSSecretNotFound  1m    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress resource.
      Normal   Success            59s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  AnnotationError    40s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress.bluemix.net/custom-port annotation. Error annotation format error : One of the mandatory fields not valid/missing for annotation ingress.bluemix.net/custom-port
      Normal   Success            40s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  AnnotationError    2s    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress.bluemix.net/custom-port annotation. Invalid port 7490. Annotation cannot use ports 7481 - 7490
      Normal   Success            2s    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
    ```
    {: screen}

6.  檢查 ALB 的日誌。
    1.  擷取在叢集中執行的 Ingress Pod 的 ID。

      ```
      kubectl get pods -n kube-system | grep alb
      ```
      {: pre}

    2.  擷取每一個 Ingress Pod 的日誌。

      ```
      kubectl logs <ingress_pod_ID> nginx-ingress -n kube-system
      ```
      {: pre}

    3.  尋找 ALB 日誌中的錯誤訊息。

<br />


## Ingress 應用程式負載平衡器密碼問題
{: #cs_albsecret_fails}

{: tsSymptoms}
在您將 Ingress 應用程式負載平衡器 (ALB) 密碼部署到叢集之後，於 {{site.data.keyword.cloudcerts_full_notm}} 檢視憑證時，`Description` 欄位不會更新密碼名稱。

當您列出 ALB 密碼的相關資訊時，狀態顯示為 `*_failed`。例如，`create_failed`、`update_failed`、`delete_failed`。

{: tsResolve}
請檢閱下列原因，以了解 ALB 密碼為何失敗以及對應的疑難排解步驟：

<table>
<caption>疑難排解 Ingress 應用程式負載平衡器密碼</caption>
 <thead>
 <th>發生原因</th>
 <th>修正方式</th>
 </thead>
 <tbody>
 <tr>
 <td>您沒有必要的存取角色，無法下載及更新憑證資料。</td>
 <td>請洽詢帳戶管理者，指派您 {{site.data.keyword.cloudcerts_full_notm}} 實例的**操作員**和**編輯者**角色。如需相關資訊，請參閱 {{site.data.keyword.cloudcerts_short}} 的<a href="/docs/services/certificate-manager/access-management.html#managing-service-access-roles">管理服務存取</a>。</td>
 </tr>
 <tr>
 <td>建立、更新或移除時提供的憑證 CRN 與叢集不屬於相同的帳戶。</td>
 <td>請確定您提供的憑證 CRN 已匯入到部署在與叢集相同帳戶中的 {{site.data.keyword.cloudcerts_short}} 服務實例。</td>
 </tr>
 <tr>
 <td>建立時提供的憑證 CRN 不正確。</td>
 <td><ol><li>檢查您提供之憑證 CRN 字串的正確性。</li><li>如果發現憑證 CRN 是正確的，請嘗試更新密碼：<code>bx cs alb-cert-deploy --update --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li><li>如果這個指令導致 <code>update_failed</code> 狀態，則移除密碼：<code>bx cs alb-cert-rm --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt;</code></li><li>重新部署密碼：<code>bx cs alb-cert-deploy --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li></ol></td>
 </tr>
 <tr>
 <td>更新時提供的憑證 CRN 不正確。</td>
 <td><ol><li>檢查您提供之憑證 CRN 字串的正確性。</li><li>如果發現憑證 CRN 是正確的，則移除密碼：<code>bx cs alb-cert-rm --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt;</code></li><li>重新部署密碼：<code>bx cs alb-cert-deploy --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li><li>嘗試更新密碼：<code>bx cs alb-cert-deploy --update --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li></ol></td>
 </tr>
 <tr>
 <td>{{site.data.keyword.cloudcerts_long_notm}} 服務遭遇關閉時間。</td>
 <td>確定您的 {{site.data.keyword.cloudcerts_short}} 服務已啟動並執行。</td>
 </tr>
 </tbody></table>

<br />


## 無法取得 Ingress ALB 的子網域
{: #cs_subnet_limit}

{: tsSymptoms}
當您執行 `bx cs cluster-get <cluster>` 時，您的叢集處於 `normal` 狀態，但沒有 **Ingress 子網域**可用。

您可能會看到類似下列內容的錯誤訊息。

```
There are already the maximum number of subnets permitted in this VLAN.
```
{: screen}

{: tsCauses}
當您建立叢集時，會在您指定的 VLAN 上要求 8 個公用及 8 個專用可攜式子網路。對於 {{site.data.keyword.containershort_notm}}，VLAN 的限制為 40 個子網路。如果叢集的 VLAN 已達到該限制，則無法佈建 **Ingress 子網域**。

若要檢視 VLAN 有多少子網路，請執行下列動作：
1.  從 [IBM Cloud 基礎架構 (SoftLayer) 主控台](https://control.bluemix.net/)，選取**網路** > **IP 管理** > **VLAN**。
2.  按一下您用來建立叢集之 VLAN 的 **VLAN 號碼**。檢閱 **Subnets** 區段，以查看是否有 40 個以上的子網路。

{: tsResolve}
如果您需要新的 VLAN，請[與 {{site.data.keyword.Bluemix_notm}} 支援中心聯絡](/docs/get-support/howtogetsupport.html#getting-customer-support)來訂購。然後，[建立叢集](cs_cli_reference.html#cs_cluster_create)，而叢集使用這個新的 VLAN。

如果您有另一個可用的 VLAN，可以在現有叢集中[設定 VLAN 跨越](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning)。之後，您便可以將新的工作者節點新增至使用具有可用子網路之另一個 VLAN 的叢集。

如果您未使用 VLAN 中的所有子網路，則可以在叢集中重複使用子網路。
1.  檢查您要使用的子網路可供使用。**附註**：您所使用的基礎架構帳戶可能會在多個 {{site.data.keyword.Bluemix_notm}} 帳戶之間共用。若是如此，即使您執行 `bx cs subnets` 指令來查看具有**連結叢集**的子網路，您也只能看到您的叢集的資訊。請洽詢基礎架構帳戶擁有者，以確定子網路可供使用，且其他任何帳戶或團隊不在使用中。

2.  使用 `--no-subnet` 選項[建立叢集](cs_cli_reference.html#cs_cluster_create)，以便服務不會嘗試建立新的子網路。請指定位置以及有子網路可供重複使用的 VLAN。

3.  使用 `bx cs cluster-subnet-add` [指令](cs_cli_reference.html#cs_cluster_subnet_add)，將現有子網路新增至叢集。如需相關資訊，請參閱[在 Kubernetes 叢集中新增或重複使用自訂及現有的子網路](cs_subnets.html#custom)。

<br />


## 無法使用 strongSwan Helm 圖表建立 VPN 連線功能
{: #cs_vpn_fails}

{: tsSymptoms}
當您執行 `kubectl exec -n kube-system  $STRONGSWAN_POD -- ipsec status` 來檢查 VPN 連線功能時，並未看到 `ESTABLISHED` 狀態，或是 VPN Pod 處於 `ERROR` 狀態，或持續當機及重新啟動。

{: tsCauses}
您的 Helm 圖表配置檔有不正確的值、遺漏值或語法錯誤。

{: tsResolve}
當您嘗試使用 strongSwan Helm 圖表建立 VPN 連線功能時，第一次 VPN 狀態可能不是 `ESTABLISHED`。您可能需要檢查數種問題，並據此變更配置檔。若要對 strongSwan VPN 連線功能進行疑難排解，請執行下列動作：

1. 根據配置檔中的設定檢查內部部署 VPN 端點設定。如果設定不符合，請執行下列動作：

    <ol>
    <li>刪除現有 Helm 圖表。</br><pre class="codeblock"><code>helm delete --purge <release_name></code></pre></li>
    <li>修正 <code>config.yaml</code> 檔案中不正確的值，然後儲存更新的檔案。</li>
    <li>安裝新的 Helm 圖表。</br><pre class="codeblock"><code>helm install -f config.yaml --namespace=kube-system --name=<release_name> bluemix/strongswan</code></pre></li>
    </ol>

2. 如果 VPN Pod 處於 `ERROR` 狀態，或持續損毀並重新啟動，則可能是圖表配置對映中 `ipsec.conf` 設定的參數驗證所造成。

    <ol>
    <li>檢查 strongSwan Pod 日誌中的所有驗證錯誤。</br><pre class="codeblock"><code>kubectl logs -n kube-system $STRONGSWAN_POD</code></pre></li>
    <li>如果日誌包含驗證錯誤，請刪除現有 Helm 圖表。</br><pre class="codeblock"><code>helm delete --purge <release_name></code></pre></li>
    <li>修正 `config.yaml` 檔案中不正確的值，然後儲存更新的檔案。</li>
    <li>安裝新的 Helm 圖表。</br><pre class="codeblock"><code>helm install -f config.yaml --namespace=kube-system --name=<release_name> bluemix/strongswan</code></pre></li>
    </ol>

3. 執行 strongSwan 圖表定義所包括的 5 個 Helm 測試。

    <ol>
    <li>執行 Helm 測試。</br><pre class="codeblock"><code>helm test vpn</code></pre></li>
    <li>如果有任何測試失敗，請參閱[瞭解 Helm VPN 連線功能測試](cs_vpn.html#vpn_tests_table)，以取得每一個測試的相關資訊，以及可能失敗的原因。<b>附註</b>：部分測試的需求是 VPN 配置中的選用設定。如果某些測試失敗，則根據您是否指定這些選用設定，失敗也許是可接受的。</li>
    <li>查看測試 Pod 的日誌，以檢視失敗測試的輸出。<br><pre class="codeblock"><code>kubectl logs -n kube-system <test_program></code></pre></li>
    <li>刪除現有 Helm 圖表。</br><pre class="codeblock"><code>helm delete --purge <release_name></code></pre></li>
    <li>修正 <code>config.yaml</code> 檔案中不正確的值，然後儲存更新的檔案。</li>
    <li>安裝新的 Helm 圖表。</br><pre class="codeblock"><code>helm install -f config.yaml --namespace=kube-system --name=<release_name> bluemix/strongswan</code></pre></li>
    <li>若要檢查變更，請執行下列動作：<ol><li>取得現行測試 Pod。</br><pre class="codeblock"><code>kubectl get pods -a -n kube-system -l app=strongswan-test</code></pre></li><li>清除現行測試 Pod。</br><pre class="codeblock"><code>kubectl delete pods -n kube-system -l app=strongswan-test</code></pre></li><li>重新執行測試。</br><pre class="codeblock"><code>helm test vpn</code></pre></li>
    </ol></ol>

4. 執行包裝在 VPN Pod 映像檔內的 VPN 除錯工具。

    1. 設定 `STRONGSWAN_POD` 環境變數。

        ```
        export STRONGSWAN_POD=$(kubectl get pod -n kube-system -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
        ```
        {: pre}

    2. 執行除錯工具。

        ```
        kubectl exec -n kube-system  $STRONGSWAN_POD -- vpnDebug
        ```
        {: pre}

        此工具會輸出好幾頁的資訊，因為它會針對一般網路問題執行各種不同的測試。開頭為 `ERROR`、`WARNING`、`VERIFY` 或 `CHECK` 的輸出行，指出 VPN 連線功能可能發生的錯誤。

    <br />


## 在工作者節點新增或刪除之後，strongSwan VPN 連線功能失敗
{: #cs_vpn_fails_worker_add}

{: tsSymptoms}
您先前已使用 strongSwan IPSec VPN 服務建立了工作中 VPN 連線。不過，您在叢集上新增或刪除工作者節點之後，發生下列一個以上的症狀：

* 您沒有 `ESTABLISHED` 的 VPN 狀態
* 您無法從內部部署網路中存取新的工作者節點
* 您無法從在新的工作者節點上執行的 Pod 中存取遠端網路

{: tsCauses}
如果您已新增工作者節點：

* 工作者節點是佈建在新的專用子網路上，現有 `localSubnetNAT` 或 `local.subnet` 設定並未透過 VPN 連線公開該子網路
* VPN 路徑無法新增至工作者節點，因為工作者節點有污點或標籤未內含在現有 `tolerations` 或 `nodeSelector` 設定中
* VPN Pod 在新的工作者節點上執行，但不容許該工作者節點的公用 IP 位址通過內部部署防火牆

如果您已刪除工作者節點：

* 該工作者節點是執行 VPN Pod 的唯一節點，這是因為現有 `tolerations` 或 `nodeSelector` 設定中對特定污點或標籤有限制

{: tsResolve}
更新 Helm 圖表值，以反映工作者節點變更：

1. 刪除現有 Helm 圖表。

    ```
    helm delete --purge <release_name>
    ```
    {: pre}

2. 開啟 strongSwan VPN 服務的配置檔。

    ```
    helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. 檢查下列設定，並視需要變更設定，以反映已刪除或新增的工作者節點。

    如果您已新增工作者節點：

    <table>
    <caption>工作者節點設定</caption?
     <thead>
     <th>設定</th>
     <th>說明</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>新增的工作者可能部署在另一個新的專用子網路上，而不是其他工作者節點所在的其他現有子網路。如果您使用子網路 NAT 來重新對映叢集的專用本端 IP 位址，且工作者已新增在新的子網路上，請將新的子網路 CIDR 新增至此設定。</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>如果您先前將 VPN Pod 部署限制為具有特定標籤的工作者，請確定新增的工作者節點也具有該標籤。</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>如果新增的工作者節點有污點，則變更此設定，以容許 VPN Pod 在具有任何污點或特定污點的所有有污點的工作者上執行。</td>
     </tr>
     <tr>
     <td><code>local.subnet</code></td>
     <td>新增的工作者可能部署在另一個新的專用子網路上，而不是其他工作者所在的現有子網路。如果您的應用程式是由專用網路上的 NodePort 或 LoadBalancer 服務公開，且應用程式位於新增的工作者上，請將新的子網路 CIDR 新增至此設定。**附註**：如果您將值新增至 `local.subnet`，請檢查內部部署子網路的 VPN 設定，以查看它們是否也必須進行更新。</td>
     </tr>
     </tbody></table>

    如果您已刪除工作者節點：

    <table>
    <caption>工作者節點設定</caption>
     <thead>
     <th>設定</th>
     <th>說明</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>如果您使用子網路 NAT 來重新對映特定的專用本端 IP 位址，請從這個設定移除舊工作者中的任何 IP 位址。如果您使用子網路 NAT 來重新對映整個子網路，且子網路上未剩餘任何工作者，請從此設定中移除該子網路 CIDR。</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>如果您先前將 VPN Pod 部署限制為單一工作者，且已刪除該工作者，請變更此設定，以容許 VPN Pod 在其他工作者上執行。</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>如果您所刪除的工作者沒有污點，但唯一剩下的工作者有污點，請變更此設定，以容許 VPN Pod 在具有任何污點或特定污點的工作者上執行。
     </td>
     </tr>
     </tbody></table>

4. 安裝含有更新值的新 Helm 圖表。

    ```
    helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/strongswan
    ```
    {: pre}

5. 檢查圖表部署狀態。圖表就緒時，輸出頂端附近的 **STATUS** 欄位值為 `DEPLOYED`。

    ```
    helm status <release_name>
    ```
    {: pre}

6. 在某些情況下，您可能需要變更內部部署設定及防火牆設定，以符合您對 VPN 配置檔所做的變更。

7. 啟動 VPN。
    * 如果由叢集起始 VPN 連線（`ipsec.auto` 設為 `start`），請在內部部署閘道上啟動 VPN，然後在叢集上啟動 VPN。
    * 如果由內部部署閘道起始 VPN 連線（`ipsec.auto` 設為 `auto`），請在叢集上啟動 VPN，然後在內部部署閘道上啟動 VPN。

8. 設定 `STRONGSWAN_POD` 環境變數。

    ```
    export STRONGSWAN_POD=$(kubectl get pod -n kube-system -l app=strongswan,release=<release_name> -o jsonpath='{ .items[0].metadata.name }')
    ```
    {: pre}

9. 檢查 VPN 的狀態。

    ```
    kubectl exec -n kube-system  $STRONGSWAN_POD -- ipsec status
    ```
    {: pre}

    * 如果 VPN 連線的狀態為 `ESTABLISHED`，則表示 VPN 連線成功。不需執行進一步的動作。

    * 如果您仍有連線問題，請參閱[無法使用 strongSwan Helm 圖表建立 VPN 連線功能](#cs_vpn_fails)，進一步對您的 VPN 連線進行疑難排解。

<br />



## 無法擷取 Calico 網路原則
{: #cs_calico_fails}

{: tsSymptoms}
當您執行 `calicoctl get policy` 嘗試檢視叢集中的 Calico 網路原則時，會收到下列其中一個非預期的結果或錯誤訊息：
- 空清單
- 舊 Calico 第 2 版原則的清單，而非第 3 版原則
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'projectcalico.org/v3'`

當您執行 `calicoctl get GlobalNetworkPolicy` 嘗試檢視叢集中的 Calico 網路原則時，會收到下列其中一個非預期的結果或錯誤訊息：
- 空清單
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'v1'`
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'projectcalico.org/v3'`
- `Failed to get resources: Resource type 'GlobalNetworkPolicy' is not supported`

{: tsCauses}
若要使用 Calico 原則，則必須全部符合四個因素：您叢集的 Kubernetes 版本、Calico CLI 版本、Calico 配置檔語法，以及檢視原則指令。其中有一個以上的因素的版本不正確。

{: tsResolve}
當您的叢集是 [Kubernetes 1.10 版或更新版本](cs_versions.html)時，您必須使用 Calico CLI 3.1 版、`calicoctl.cfg` 第 3 版配置檔語法，以及 `calicoctl get GlobalNetworkPolicy` 和 `calicoctl get NetworkPolicy` 指令。

當您的叢集是 [Kubernetes 1.9 版或更早版本](cs_versions.html)時，您必須使用 Calico CLI 1.6.3 版、`calicoctl.cfg` 第 2 版配置檔語法，以及 `calicoctl get policy` 指令。

若要確保符合所有 Calico 因素，請執行下列動作：

1. 檢視叢集 Kubernetes 版本。
    ```
    bx cs cluster-get <cluster_name>
    ```
    {: pre}

    * 如果您的叢集是 Kubernetes 1.10 版或更新版本，請執行下列動作：
        1. [安裝及配置 3.1.1 版 Calico CLI](cs_network_policy.html#1.10_install)。配置包括手動更新 `calicoctl.cfg` 檔案，以使用 Calico 第 3 版語法。
        2. 確定您建立且要套用至您叢集的任何原則都會使用 [Calico 第 3 版語法 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/networkpolicy)。如果您在 Calico 第 2 版語法中有現有原則 `.yaml` 或 `.json` 檔案，則可以使用 [`calicoctl convert` 指令 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.projectcalico.org/v3.1/reference/calicoctl/commands/convert) 將它轉換為 Calico 第 3 版語法。
        3. 若要[檢視原則](cs_network_policy.html#1.10_examine_policies)，請確定您針對廣域原則使用 `calicoctl get GlobalNetworkPolicy`，並針對範圍設為特定名稱空間的原則使用 `calicoctl get NetworkPolicy --namespace <policy_namespace>`。

    * 如果您的叢集是 Kubernetes 1.9 版或更早版本，請執行下列動作：
        1. [安裝及配置 1.6.3 版 Calico CLI](cs_network_policy.html#1.9_install)。請確定 `calicoctl.cfg` 檔案使用 Calico 第 2 版語法。
        2. 確定您建立且要套用至您叢集的任何原則都會使用 [Calico 第 2 版語法 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.projectcalico.org/v2.6/reference/calicoctl/resources/policy)。
        3. 若要[檢視原則](cs_network_policy.html#1.9_examine_policies)，請確定您使用 `calicoctl get policy`。

在您將叢集從 Kubernetes 1.9 版或更早版本更新至 1.10 版或更新版本之前，請檢閱[準備更新至 Calico 第 3 版](cs_versions.html#110_calicov3)。
{: tip}

<br />


## 取得協助及支援
{: #ts_getting_help}

叢集仍有問題？
{: shortdesc}

-   若要查看 {{site.data.keyword.Bluemix_notm}} 是否可用，請[檢查 {{site.data.keyword.Bluemix_notm}} 狀態頁面 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/bluemix/support/#status)。
-   將問題張貼到 [{{site.data.keyword.containershort_notm}} Slack ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://ibm-container-service.slack.com)。

    如果您的 {{site.data.keyword.Bluemix_notm}} 帳戶未使用 IBM ID，請[要求邀請](https://bxcs-slack-invite.mybluemix.net/)以加入此 Slack。
    {: tip}
-   檢閱討論區，以查看其他使用者是否發生過相同的問題。使用討論區提問時，請標記您的問題，以便 {{site.data.keyword.Bluemix_notm}} 開發團隊能看到它。

    -   如果您在使用 {{site.data.keyword.containershort_notm}} 開發或部署叢集或應用程式時有技術方面的問題，請將問題張貼到 [Stack Overflow ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers)，並使用 `ibm-cloud`、`kubernetes` 及 `containers` 來標記問題。
    -   若為服務及開始使用指示的相關問題，請使用 [IBM developerWorks dW Answers ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) 討論區。請包含 `ibm-cloud` 及 `containers` 標籤。如需使用討論區的詳細資料，請參閱[取得協助](/docs/get-support/howtogetsupport.html#using-avatar)。

-   開立問題單以與 IBM 支援中心聯絡。若要瞭解開立 IBM 支援問題單或是支援層次與問題單嚴重性，請參閱[與支援中心聯絡](/docs/get-support/howtogetsupport.html#getting-customer-support)。

{: tip}
當您報告問題時，請包含您的叢集 ID。若要取得叢集 ID，請執行 `bx cs clusters`。

