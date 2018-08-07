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



# 限制送至邊緣工作者節點的網路資料流量
{: #edge}

邊緣工作者節點可以藉由容許較少的工作者節點可在外部進行存取，以及隔離 {{site.data.keyword.containerlong}} 中的網路工作負載，來增進 Kubernetes 叢集的安全。
{:shortdesc}

僅在標示這些工作者節點可以進行網路連線時，其他工作負載才無法取用工作者節點的記憶體，並干擾網路連線。





## 將工作者節點標示為邊緣節點
{: #edge_nodes}

將 `dedicated=edge` 標籤新增至叢集中每一個公用 VLAN 的兩個以上工作者節點，以確保 Ingress 及負載平衡器只會部署至那些工作者節點。
{:shortdesc}

開始之前：

- [建立標準叢集。](cs_clusters.html#clusters_cli)
- 確保您的叢集至少具有一個公用 VLAN。僅具有專用 VLAN 的叢集無法使用邊緣工作者節點。
- [將 Kubernetes CLI 的目標設為叢集](cs_cli_install.html#cs_cli_configure)。

若要將工作者節點標示為邊緣節點，請執行下列動作：

1. 列出叢集中的所有工作者節點。請使用來自 **NAME** 直欄的專用 IP 位址來識別節點。在每一個公用 VLAN 上至少選取兩個工作者節點作為邊緣工作者節點。Ingress 需要每一個區域中至少有兩個工作者節點來提供高可用性。 

  ```
  kubectl get nodes -L publicVLAN,privateVLAN,dedicated
  ```
  {: pre}

2. 使用 `dedicated=edge` 來標示工作者節點。在使用 `dedicated=edge` 標示工作者節點之後，所有後續的 Ingress 及負載平衡器都會部署至邊緣工作者節點。

  ```
  kubectl label nodes <node1_name> <node2_name> dedicated=edge
  ```
  {: pre}

3. 擷取叢集中所有現有的負載平衡器服務。

  ```
  kubectl get services --all-namespaces -o jsonpath='{range .items[*]}kubectl get service -n {.metadata.namespace} {.metadata.name} -o yaml | kubectl apply -f - :{.spec.type},{end}' | tr "," "\n" | grep "LoadBalancer" | cut -d':' -f1
  ```
  {: pre}

  輸出範例：

  ```
  kubectl get service -n <namespace> <service_name> -o yaml | kubectl apply -f
  ```
  {: screen}

4. 使用來自前一個步驟的輸出，複製並貼到每一個 `kubectl get service` 指令行。此指令會將負載平衡器重新部署至邊緣工作者節點。只有公用負載平衡器必須重新部署。

  輸出範例：

  ```
  service "my_loadbalancer" configured
  ```
  {: screen}

您已使用 `dedicated=edge` 來標示工作者節點，並已將所有現有的負載平衡器及 Ingress 重新部署至邊緣工作者節點。接下來，請避免其他[工作負載在邊緣工作者節點上執行](#edge_workloads)，以及[封鎖對工作者節點上 NodePort 的入埠資料流量](cs_network_policy.html#block_ingress)。

<br />


## 避免工作負載在邊緣工作者節點上執行
{: #edge_workloads}

邊緣工作者節點的一項好處是它們可以指定為僅執行網路服務。
{:shortdesc}

使用 `dedicated=edge` 容忍，表示所有負載平衡器及 Ingress 服務都只會部署至已標示的工作者節點。不過，為了避免其他工作負載在邊緣工作者節點上執行，以及取用工作者節點資源，您必須使用 [Kubernetes 污點 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)。



1. 列出所有具有 `dedicated=edge` 標籤的工作者節點。

  ```
  kubectl get nodes -L publicVLAN,privateVLAN,dedicated -l dedicated=edge
  ```
  {: pre}

2. 將污點套用至每一個工作者節點，以避免 Pod 在工作者節點上執行，並從工作者節點中移除沒有 `dedicated=edge` 標籤的 Pod。移除的 Pod 會重新部署在有容量的其他工作者節點上。

  ```
  kubectl taint node <node_name> dedicated=edge:NoSchedule dedicated=edge:NoExecute
  ```
現在，僅具有 `dedicated=edge` 容忍的 Pod 才會部署至您的邊緣工作者節點。

3. 如果您選擇[針對負載平衡器服務啟用來源 IP 保留 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-typeloadbalancer)，請確保藉由[將邊緣節點親緣性新增至應用程式 Pod](cs_loadbalancer.html#edge_nodes)，在邊緣工作者節點上排定應用程式 Pod。必須在邊緣節點上排定應用程式 Pod，才能接收送入要求。
