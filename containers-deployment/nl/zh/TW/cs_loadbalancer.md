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



# 使用 LoadBalancer 公開應用程式
{: #loadbalancer}

公開埠並使用可攜式 IP 位址，讓負載平衡器可以存取容器化應用程式。
{:shortdesc}

## 使用 LoadBalancer 管理網路資料流量
{: #planning}

當您建立標準叢集時，{{site.data.keyword.containershort_notm}} 會自動佈建下列子網路：
    
* 主要的公用子網路，用於在建立叢集期間判定工作者節點的公用 IP 位址
* 主要的專用子網路，用於在建立叢集期間判定工作者節點的專用 IP 位址
* 可攜式公用子網路，用於提供 5 個公用 IP 位址給 Ingress 及負載平衡器網路服務
* 可攜式專用子網路，用於提供 5 個專用 IP 位址給 Ingress 及負載平衡器網路服務

      可攜式公用及專用 IP 位址是靜態的，而且不會在移除工作者節點時變更。針對每一個子網路，一個可攜式公用及一個可攜式專用 IP 位址用於預設 [Ingress 應用程式負載平衡器](cs_ingress.html)。藉由建立負載平衡器服務，即可使用其餘四個可攜式公用 IP 位址及四個可攜式專用 IP 位址，將單一應用程式公開至公用或專用網路。

當您在公用 VLAN 上的叢集建立 Kubernetes LoadBalancer 服務時，會建立外部負載平衡器。當您建立 LoadBalancer 服務時，您的 IP 位址選項如下：

- 如果您的叢集是在公用 VLAN 上，則會使用四個可用的可攜式公用 IP 位址的其中一個。
- 如果您的叢集只能在專用 VLAN 上使用，則會使用四個可用的可攜式專用 IP 位址的其中一個。
- 您可以透過將註釋新增至配置檔，要求 LoadBalancer 服務的可攜式公用或專用 IP 位址：`service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: <public_or_private>`。

指派給 LoadBalancer 服務的可攜式公用 IP 位址是永久性的，在叢集中移除或重建工作者節點時並不會變更。因此，LoadBalancer 服務的可用性比 NodePort 服務高。與 NodePort 服務不同，您可以將任何埠指派給負載平衡器，而且未連結至特定埠範圍。如果您使用 LoadBalancer 服務，則任何工作者節點的每一個 IP 位址也都有一個 NodePort 可用。若要在使用 LoadBalancer 服務時封鎖對 NodePort 的存取，請參閱[封鎖送入的資料流量](cs_network_policy.html#block_ingress)。

LoadBalancer 服務是作為應用程式送入要求的外部進入點。若要從網際網路存取 LoadBalancer 服務，請使用負載平衡器的公用 IP 位址以及 `<IP_address>:<port>` 格式的已指派埠。下圖顯示負載平衡器如何將通訊從網際網路導向應用程式。

<img src="images/cs_loadbalancer_planning.png" width="550" alt="使用負載平衡器在 {{site.data.keyword.containershort_notm}} 中公開應用程式" style="width:550px; border-style: none"/>

1. 使用負載平衡器的公用 IP 位址及工作者節點上的已指派埠，將要求傳送至應用程式。

2. 該要求會自動轉遞至負載平衡器服務的內部叢集 IP 位址及埠。內部叢集 IP 位址只能在叢集內部存取。

3. `kube-proxy` 會將要求遞送至應用程式的 Kubernetes 負載平衡器服務。

4. 要求會轉遞至應用程式部署所在 Pod 的專用 IP 位址。如果叢集中已部署多個應用程式實例，則負載平衡器會在應用程式 Pod 之間遞送要求。




<br />


s`.</td>
        </tr>
        <tr>
          <td><code>selector</code></td>
          <td>輸入標籤索引鍵 (<em>&lt;selector_key&gt;</em>) 及值 (<em>&lt;selector_value&gt;</em>) 配對，以用來將應用程式執行所在的 Pod 設為目標。若要將 Pod 設為目標，並在服務負載平衡中包括它們，請勾選 <em>&lt;selectorkey&gt;</em> 及 <em>&lt;selectorvalue&gt;</em> 值。確定它們與您在部署 yaml 的 <code>spec.template.metadata.labels</code> 區段中所使用的<em>鍵值組</em> 相同。</td>
        </tr>
        <tr>
          <td><code>port</code></td>
          <td>服務所接聽的埠。</td>
        </tr>
        <tr>
          <td><code>loadBalancerIP</code></td>
          <td>若要建立專用 LoadBalancer 或針對公用 LoadBalancer 使用特定的可攜式 IP 位址，請將 <em>&lt;IP_address&gt;</em> 取代為您要使用的 IP 位址。如需相關資訊，請參閱 [Kubernetes 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer)。</td>
        </tr>
        </tbody></table>

      3. 選用項目：在 **spec** 區段中指定 `loadBalancerSourceRanges`，以配置防火牆。如需相關資訊，請參閱 [Kubernetes 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/tasks/access-application-cluster/configure-cloud-provider-firewall/)。

      4. 在叢集中建立服務。

          ```
                  kubectl apply -f myloadbalancer.yaml
        ```
          {: pre}

                  若已建立負載平衡器服務，可攜式 IP 位址即會自動指派給負載平衡器。如果沒有可用的可攜式 IP 位址，則無法建立負載平衡器服務。


3.  驗證已順利建立負載平衡器服務。將 _&lt;myservice&gt;_ 取代為您在前一個步驟中建立之負載平衡器服務的名稱。

    ```
        kubectl describe service myloadbalancer
    ```
    {: pre}

    **附註：**這可能需要幾分鐘的時間，才能適當地建立負載平衡器服務，以及提供使用應用程式。

    CLI 輸出範例：

    ```
    Name:                   myloadbalancer
    Namespace:              default
    Labels:                 <none>
    Selector:               app=liberty
    Type:                   LoadBalancer
    Location:               dal10
    IP:                     172.21.xxx.xxx
    LoadBalancer Ingress:   169.xx.xxx.xxx
    Port:                   <unset> 8080/TCP
    NodePort:               <unset> 32040/TCP
    Endpoints:              172.30.xxx.xxx:8080
    Session Affinity:       None
    Events:
      FirstSeen	LastSeen	Count	From			SubObjectPath	Type	 Reason			          Message
      ---------	--------	-----	----			-------------	----	 ------			          -------
      10s		    10s		    1	    {service-controller }	  Normal CreatingLoadBalancer	Creating load balancer
      10s		    10s		    1	    {service-controller }		Normal CreatedLoadBalancer	Created load balancer
    ```
    {: screen}

        **LoadBalancer Ingress** IP 位址是已指派給負載平衡器服務的可攜式 IP 位址。


4.  如果您已建立公用負載平衡器，請從網際網路存取您的應用程式。
    1.  開啟偏好的 Web 瀏覽器。
    2.  輸入負載平衡器的可攜式公用 IP 位址及埠。

        ```
                http://169.xx.xxx.xxx:8080
        ```
        {: codeblock}        

5. 如果您選擇[針對負載平衡器服務啟用來源 IP 保留 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-typeloadbalancer)，請確保藉由[將邊緣節點親緣性新增至應用程式 Pod](cs_loadbalancer.html#edge_nodes)，在邊緣工作者節點上排定應用程式 Pod。必須在邊緣節點上排定應用程式 Pod，才能接收送入要求。

6. 選用項目：若要處理來自其他區域送入您應用程式的要求，請重複這些步驟以在每一個區域中新增負載平衡器。

</staging>
    ## 使用 LoadBalancer 服務來啟用應用程式的公用或專用存取
{: #config}

開始之前：

-   只有標準叢集才能使用此特性。
-   您必須要有可指派給負載平衡器服務的可攜式公用或專用 IP 位址。
-   具有可攜式專用 IP 位址的負載平衡器服務仍然會在每個工作者節點上開啟一個公用 NodePort。若要新增網路原則以防止公用資料流量，請參閱[封鎖送入的資料流量](cs_network_policy.html#block_ingress)。

若要建立負載平衡器服務，請執行下列動作：

1.  [將應用程式部署至叢集](cs_app.html#app_cli)。將應用程式部署至叢集時，會自動建立一個以上的 Pod，以在容器中執行您的應用程式。請確定您已將標籤新增至您部署中配置檔的 meta 資料區段。此標籤是識別您應用程式執行所在之所有 Pod 的必要項目，如此才能將 Pod 包含在負載平衡中。
2.  為您要公開的應用程式建立負載平衡器服務。若要讓您的應用程式可在公用網際網路或專用網路上使用，請建立應用程式的 Kubernetes 服務。請配置服務，以將所有構成應用程式的 Pod 包含在負載平衡中。
    1.  例如，建立名稱為 `myloadbalancer.yaml` 的服務配置檔。

    2.  為您要公開的應用程式定義負載平衡器服務。
        - 如果叢集是在公用 VLAN 上，則會使用可攜式公用 IP 位址。大部分叢集都在公用 VLAN 上。
        - 如果您的叢集只能在專用 VLAN 上使用，則會使用可攜式專用 IP 位址。
        - 您可以透過將註釋新增至配置檔，要求 LoadBalancer 服務的可攜式公用或專用 IP 位址。

        使用預設 IP 位址的 LoadBalancer 服務：

        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: myloadbalancer
        spec:
          type: LoadBalancer
          selector:
            <selector_key>:<selector_value>
          ports:
           - protocol: TCP
             port: 8080
        ```
        {: codeblock}

        使用註釋來指定專用或公用 IP 位址的 LoadBalancer 服務：

        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: myloadbalancer
          annotations:
            service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: <public_or_private>
        spec:
          type: LoadBalancer
          selector:
            <selector_key>: <selector_value>
          ports:
           - protocol: TCP
             port: 8080
          loadBalancerIP: <IP_address>
        ```
        {: codeblock}

        <table>
        <caption>瞭解 YAML 檔案元件</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解 YAML 檔案元件</th>
        </thead>
        <tbody>
        <tr>
          <td><code>selector</code></td>
          <td>輸入標籤索引鍵 (<em>&lt;selector_key&gt;</em>) 及值 (<em>&lt;selector_value&gt;</em>) 配對，以用來將應用程式執行所在的 Pod 設為目標。若要將 Pod 設為目標，並在服務負載平衡中包括它們，請勾選 <em>&lt;selector_key&gt;</em> 及 <em>&lt;selector_value&gt;</em> 值。確定它們與您在部署 yaml 的 <code>spec.template.metadata.labels</code> 區段中所使用的<em>鍵值組</em> 相同。</td>
        </tr>
        <tr>
          <td><code>port</code></td>
          <td>服務所接聽的埠。</td>
        </tr>
        <tr>
          <td>`service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type:`
          <td>要指定 LoadBalancer 類型的註釋。接受值為 `private` 及 `public`。如果您要在公用 VLAN 的叢集中建立公用 LoadBalancer，則不需要此註釋。</td>
        </tr>
        <tr>
          <td><code>loadBalancerIP</code></td>
          <td>若要建立專用 LoadBalancer 或針對公用 LoadBalancer 使用特定的可攜式 IP 位址，請將 <em>&lt;IP_address&gt;</em> 取代為您要使用的 IP 位址。如需相關資訊，請參閱 [Kubernetes 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer)。</td>
        </tr>
        </tbody></table>

    3.  選用項目：在 **spec** 區段中指定 `loadBalancerSourceRanges`，以配置防火牆。如需相關資訊，請參閱 [Kubernetes 文件 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/tasks/access-application-cluster/configure-cloud-provider-firewall/)。

    4.  在叢集中建立服務。

        ```
                kubectl apply -f myloadbalancer.yaml
        ```
        {: pre}

                若已建立負載平衡器服務，可攜式 IP 位址即會自動指派給負載平衡器。如果沒有可用的可攜式 IP 位址，則無法建立負載平衡器服務。


3.  驗證已順利建立負載平衡器服務。

    ```
        kubectl describe service myloadbalancer
    ```
    {: pre}

    **附註：**這可能需要幾分鐘的時間，才能適當地建立負載平衡器服務，以及提供使用應用程式。

    CLI 輸出範例：

    ```
    Name:                   myloadbalancer
    Namespace:              default
    Labels:                 <none>
    Selector:               app=liberty
    Type:                   LoadBalancer
    Location:               dal10
    IP:                     172.21.xxx.xxx
    LoadBalancer Ingress:   169.xx.xxx.xxx
    Port:                   <unset> 8080/TCP
    NodePort:               <unset> 32040/TCP
    Endpoints:              172.30.xxx.xxx:8080
    Session Affinity:       None
    Events:
      FirstSeen	LastSeen	Count	From			SubObjectPath	Type	 Reason			          Message
      ---------	--------	-----	----			-------------	----	 ------			          -------
      10s		    10s		    1	    {service-controller }	  Normal CreatingLoadBalancer	Creating load balancer
      10s		    10s		    1	    {service-controller }		Normal CreatedLoadBalancer	Created load balancer
    ```
    {: screen}

        **LoadBalancer Ingress** IP 位址是已指派給負載平衡器服務的可攜式 IP 位址。


4.  如果您已建立公用負載平衡器，請從網際網路存取您的應用程式。
    1.  開啟偏好的 Web 瀏覽器。
    2.  輸入負載平衡器的可攜式公用 IP 位址及埠。

        ```
                http://169.xx.xxx.xxx:8080
        ```
        {: codeblock}

5. 如果您選擇[針對負載平衡器服務啟用來源 IP 保留 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-typeloadbalancer)，請確保藉由[將邊緣節點親緣性新增至應用程式 Pod](cs_loadbalancer.html#edge_nodes)，在邊緣工作者節點上排定應用程式 Pod。必須在邊緣節點上排定應用程式 Pod，才能接收送入要求。

<br />


## 將節點親緣性及容忍新增至來源 IP 的應用程式 Pod
{: #node_affinity_tolerations}

每當您部署應用程式 Pod 時，負載平衡器服務 Pod 也會部署至應用程式 Pod 部署至其中的工作者節點。不過，存在某些狀況，可能未在相同的工作者節點上排定負載平衡器 Pod 及應用程式 Pod。
{: shortdesc}

* 您有已受污染、因此僅負載平衡器服務 Pod 可以部署至其中的邊緣節點。不允許將應用程式 Pod 部署至那些節點。
* 您的叢集連接至多個公用或專用 VLAN，但您的應用程式 Pod 可能部署至僅連接至某個 VLAN 的工作者節點。負載平衡器服務 Pod 可能未部署至那些工作者節點，因為負載平衡器 IP 位址連接至不同於工作者節點的 VLAN。

將應用程式的用戶端要求傳送至叢集時，會將該要求遞送至公開應用程式之 Kubernetes 負載平衡器服務的 Pod。如果沒有應用程式 Pod 存在於與負載平衡器服務 Pod 相同的工作者節點上，則負載平衡器會將要求轉遞至不同工作者節點上的應用程式 Pod。套件的來源 IP 位址會變更為應用程式 Pod 執行所在之工作者節點的公用 IP 位址。

若要保留用戶端要求的原始來源 IP 位址，您可以針對負載平衡器服務[啟用來源 IP ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-typeloadbalancer)。例如，當應用程式伺服器必須套用安全及存取控制原則時，保留用戶端的 IP 是很有用的。在啟用來源 IP 之後，負載平衡器服務 Pod 必須將要求轉遞至僅部署至相同工作者節點的應用程式 Pod。若要將您的應用程式強制部署至負載平衡器服務 Pod 也可以部署至其中的特定工作者節點，您必須將親緣性規則及容忍新增至您的應用程式部署。

### 新增邊緣節點親緣性規則及容忍
{: #edge_nodes}

當您[將工作者節點標示為邊緣節點](cs_edge.html#edge_nodes)，且同時[污染邊緣節點](cs_edge.html#edge_workloads)時，負載平衡器服務 Pod 只會部署至那些邊緣節點，而且應用程式 Pod 無法部署至邊緣節點。啟用負載平衡器服務的來源 IP 時，邊緣節點上的負載平衡器 Pod 無法將送入要求轉遞至其他工作者節點上的應用程式 Pod。
{:shortdesc}

若要強制您的應用程式 Pod 部署至邊緣節點，請將邊緣節點[親緣性規則 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#node-affinity-beta-feature) 及[容忍 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/#concepts) 新增至應用程式部署。

具有邊緣節點親緣性及邊緣節點容忍的範例部署 yaml：

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: with-node-affinity
spec:
  template:
    spec:
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: dedicated
                operator: In
                values:
                - edge
      tolerations:
        - key: dedicated
          value: edge
...
```
{: codeblock}

**affinity** 及 **tolerations** 區段同時具有 `dedicated` 作為 `key`，及具有 `edge` 作為 `value`。

### 新增多個公用或專用 VLAN 的親緣性規則
{: #edge_nodes_multiple_vlans}

當您的叢集連接至多個公用或專用 VLAN 時，您的應用程式 Pod 可能部署至僅連接至某個 VLAN 的工作者節點。如果負載平衡器 IP 位址連接至與這些工作者節點不同的 VLAN，則負載平衡器服務 Pod 不會部署至那些工作者節點。
{:shortdesc}

啟用來源 IP 時，請將親緣性規則新增至應用程式部署，以在其 VLAN 與負載平衡器的 IP 位址相同的工作者節點上排定應用程式 Pod。

開始之前，請先將 [CLI 的目標](cs_cli_install.html#cs_cli_configure)設為您的叢集。

1. 取得負載平衡器服務的 IP 位址。在 **LoadBalancer Ingress** 欄位中尋找 IP 位址。
    ```
    kubectl describe service <loadbalancer_service_name>
    ```
    {: pre}

2. 擷取負載平衡器服務連接至其中的 VLAN ID。

    1. 列出叢集的可攜式公用 VLAN。
        ```
        bx cs cluster-get <cluster_name_or_ID> --showResources
        ```
        {: pre}

        輸出範例：
        ```
        ...

        Subnet VLANs
        VLAN ID   Subnet CIDR       Public   User-managed
        2234947   10.xxx.xx.xxx/29  false    false
        2234945   169.36.5.xxx/29   true     false
        ```
        {: screen}

    2. 在 **Subnet VLANs** 下的輸出中，尋找符合您先前所擷取之負載平衡器 IP 位址的子網路 CIDR，並記下 VLAN ID。

        例如，如果負載平衡器服務 IP 位址為 `169.36.5.xxx`，則前一個步驟之輸出範例中的相符子網路為 `169.36.5.xxx/29`。子網路連接至其中的 VLAN ID 為 `22334945`。

3. 針對您在前一個步驟中記下的 VLAN ID，[新增親緣性規則 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#node-affinity-beta-feature) 至應用程式部署。

    比方說，如果您有數個 VLAN，但想要應用程式 Pod 僅部署至 `2234945` 公用 VLAN 上的工作者節點，請執行下列指令：

    ```
    apiVersion: extensions/v1beta1
    kind: Deployment
    metadata:
      name: with-node-affinity
    spec:
      template:
        spec:
          affinity:
            nodeAffinity:
              requiredDuringSchedulingIgnoredDuringExecution:
                nodeSelectorTerms:
                - matchExpressions:
                  - key: publicVLAN
                    operator: In
                    values:
                    - "2234945"
    ...
    ```
    {: codeblock}

    在範例 YAML 中，**affinity** 區段具有 `publicVLAN` 作為 `key`，以及具有 `"224945"` 作為 `value`。

4. 套用已更新的部署配置檔。
    ```
    kubectl apply -f with-node-affinity.yaml
    ```
    {: pre}

5. 驗證部署至工作者節點的應用程式 Pod 是否已連接至指定的 VLAN。

    1. 列出叢集中的 Pod。將 `<selector>` 取代為您用於應用程式的標籤。
        ```
        kubectl get pods -o wide app=<selector>
        ```
        {: pre}

        輸出範例：
        ```
        NAME                   READY     STATUS              RESTARTS   AGE       IP               NODE
        cf-py-d7b7d94db-vp8pq  1/1       Running             0          15d       172.30.xxx.xxx   10.176.48.78
        ```
        {: screen}

    2. 在輸出中，識別應用程式的 Pod。記下 Pod 所在之工作者節點的**節點** ID。

        在前一個步驟的輸出範例中，應用程式 Pod `cf-py-d7b7d94db-vp8pq` 位於工作者節點 `10.176.48.78` 上。

    3. 列出工作者節點的詳細資料。

        ```
        kubectl describe node <worker_node_ID>
        ```
        {: pre}

        輸出範例：

        ```
        Name:                   10.xxx.xx.xxx
        Role:
        Labels:                 arch=amd64
                                beta.kubernetes.io/arch=amd64
                                beta.kubernetes.io/os=linux
                                failure-domain.beta.kubernetes.io/region=us-south
                                failure-domain.beta.kubernetes.io/zone=dal10
                                ibm-cloud.kubernetes.io/encrypted-docker-data=true
                                kubernetes.io/hostname=10.xxx.xx.xxx
                                privateVLAN=2234945
                                publicVLAN=2234967
        ...
        ```
        {: screen}

    4. 在輸出的 **Labels** 區段中，驗證公用或專用 VLAN 是否為您在先前步驟中指定的 VLAN。
