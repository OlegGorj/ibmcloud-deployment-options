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



# 将网络流量限于边缘工作程序节点
{: #edge}

边缘工作程序节点通过在 {{site.data.keyword.containerlong}} 中减少允许外部访问的工作程序节点，并隔离联网工作负载，可以提高 Kubernetes 集群的安全性。
{:shortdesc}

当这些工作程序节点标记为仅用于联网时，其他工作负载无法使用工作程序节点的 CPU 或内存，也不会干扰联网。





## 将工作程序节点标记为边缘节点
{: #edge_nodes}

将 `dedicated=edge` 标签添加到集群中每个公用 VLAN 上的两个或更多工作程序节点，以确保 Ingress 和负载均衡器仅部署到这些工作程序节点。
{:shortdesc}

开始之前：

- [创建标准集群](cs_clusters.html#clusters_cli)。
- 确保集群至少具有一个公共 VLAN。边缘工作程序节点不可用于仅具有专用 VLAN 的集群。
- [设定 Kubernetes CLI 的目标为集群](cs_cli_install.html#cs_cli_configure)。

要将工作程序节点标记为边缘节点，请执行以下操作：

1. 列出集群中的所有工作程序节点。使用 **NAME** 列中的专用 IP 地址来标识节点。请至少在每个公用 VLAN 上选择两个工作程序节点作为边缘工作程序节点。Ingress 在每个区域中至少需要两个工作程序节点，才可提供高可用性。 

  ```
  kubectl get nodes -L publicVLAN,privateVLAN,dedicated
  ```
  {: pre}

2. 使用 `dedicated=edge` 标记工作程序节点。工作程序节点标记有 `dedicated=edge` 后，所有后续 Ingress 和负载均衡器都会部署到边缘工作程序节点。

  ```
  kubectl label nodes <node1_name> <node2_name> dedicated=edge
  ```
  {: pre}

3. 检索集群中的所有现有 LoadBalancer 服务。

  ```
  kubectl get services --all-namespaces -o jsonpath='{range .items[*]}kubectl get service -n {.metadata.namespace} {.metadata.name} -o yaml | kubectl apply -f - :{.spec.type},{end}' | tr "," "\n" | grep "LoadBalancer" | cut -d':' -f1
  ```
  {: pre}

  输出示例：

  ```
  kubectl get service -n <namespace> <service_name> -o yaml | kubectl apply -f
  ```
  {: screen}

4. 使用上一步中的输出内容，复制并粘贴每个 `kubectl get service` 行。此命令会将负载均衡器重新部署到边缘工作程序节点。只有公共负载均衡器必须重新部署。

  输出示例：

  ```
  service "my_loadbalancer" configured
  ```
  {: screen}

您已使用 `dedicated=edge` 标记工作程序节点，并已将所有现有负载均衡器和 Ingress 重新部署到边缘工作程序节点。接下来，请[阻止其他工作负载在边缘工作程序节点上运行](#edge_workloads)并[阻止流至工作程序节点上 NodePort 的入站流量](cs_network_policy.html#block_ingress)。

<br />


## 阻止工作负载在边缘工作程序节点上运行
{: #edge_workloads}

边缘工作程序节点的优点是可以将其指定为仅运行联网服务。
{:shortdesc}

使用 `dedicated=edge` 容忍度意味着所有 LoadBalancer 和 Ingress 服务仅部署到已标记的工作程序节点。但是，要阻止其他工作负载在边缘工作程序节点上运行并使用工作程序节点资源，必须使用 [Kubernetes 污点 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)。



1. 列出具有 `dedicated=edge` 标签的所有工作程序节点。

  ```
  kubectl get nodes -L publicVLAN,privateVLAN,dedicated -l dedicated=edge
  ```
  {: pre}

2. 将污点应用于每个工作程序节点，这将阻止 pod 在该工作程序节点上运行，并且会从该工作程序节点中除去没有 `dedicated=edge` 标签的 pod。除去的 pod 会在具有容量的其他工作程序节点上重新部署。

  ```
  kubectl taint node <node_name> dedicated=edge:NoSchedule dedicated=edge:NoExecute
  ```
现在，仅具有 `dedicated=edge` 容忍度的 pod 会部署到边缘工作程序节点。

3. 如果选择[为 LoadBalancer 服务启用源 IP 保留 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-typeloadbalancer)，请确保通过[向应用程序 pod 添加边缘节点亲缘关系](cs_loadbalancer.html#edge_nodes)，将应用程序 pod 安排到边缘工作程序节点。应用程序 pod 必须安排到边缘节点才能接收入局请求。
