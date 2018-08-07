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



# 使用 LoadBalancer 公开应用程序
{: #loadbalancer}

公开一个端口，并使用负载均衡器的可移植 IP 地址来访问容器化应用程序。
{:shortdesc}

## 使用 LoadBalancer 管理网络流量
{: #planning}

创建标准集群时，{{site.data.keyword.containershort_notm}} 将自动供应以下子网：
    
* 在集群创建期间，用于确定工作程序节点的公共 IP 地址的主要公用子网
* 在集群创建期间，用于确定工作程序节点的专用 IP 地址的主要专用子网
* 用于为 Ingress 和负载均衡器联网服务提供 5 个公共 IP 地址的可移植公用子网
* 用于为 Ingress 和负载均衡器联网服务提供 5 个专用 IP 地址的可移植专用子网

      可移植公共和专用 IP 地址是静态的，不会在除去工作程序节点时更改。对于每个子网，都会有一个可移植公共 IP 地址和一个可移植专用 IP 地址用于缺省 [Ingress 应用程序负载均衡器](cs_ingress.html)。剩余 4 个可移植公共 IP 地址和 4 个可移植专用 IP 地址可用于通过创建 LoadBalancer 服务向公用或专用网络公开单个应用程序。

在公用 VLAN 上于集群中创建 Kubernetes LoadBalancer 服务时，会创建一个外部负载均衡器。在创建 LoadBalancer 服务时，IP 地址的选项如下所示：

- 如果集群位于公用 VLAN 上，那么将使用 4 个可用的可移植公共 IP 地址中的一个地址。
- 如果集群仅在专用 VLAN 上可用，那么将使用 4 个可用的可移植专用 IP 地址中的一个地址。
- 通过向配置文件添加注释，可请求用于 LoadBalancer 服务的可移植公共或专用 IP 地址：`service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type
<public_or_private>`.

分配给 LoadBalancer 服务的可移植公共 IP 地址是永久固定的，在除去或重新创建工作程序节点时不会更改。因此，LoadBalancer 服务的可用性比 NodePort 服务更高。与 NodePort 服务不同，您可以为负载均衡器分配任何端口，而不限于特定端口范围。如果使用 LoadBalancer 服务，那么任何工作程序节点的每个 IP 地址上还提供一个 NodePort。要在使用 LoadBalancer 服务时阻止访问 NodePort，请参阅[阻止入局流量](cs_network_policy.html#block_ingress)。

LoadBalancer 服务充当应用程序入局请求的外部入口点。要从因特网访问 LoadBalancer 服务，请使用负载均衡器的公共 IP 地址以及分配的端口，格式为 `<IP_address>:<port>`. 下图显示负载均衡器如何将通信从因特网定向到应用程序。

<img src="images/cs_loadbalancer_planning.png" width="550" alt="使用负载均衡器公开 {{site.data.keyword.containershort_notm}} 中的应用程序" style="width:550px; border-style: none"/>

1. 使用负载均衡器的公共 IP 地址和工作程序节点上分配的端口，将请求发送到应用程序。

2. 请求会自动转发到 LoadBalancer 服务的内部集群 IP 地址和端口。该内部集群 IP 地址只能在集群内部访问。

3. `kube-proxy` 将请求路由到应用程序的 Kubernetes LoadBalancer 服务。

4. 该请求会转发到部署了应用程序的 pod 的专用 IP 地址。如果集群中部署了多个应用程序实例，那么负载均衡器会在应用程序 pod 之间路由请求。




<br />


s`.</td>
        </tr>
        <tr>
          <td><code>selector</code></td>
          <td>输入要用于将运行应用程序的 pod 设定为目标的标签键 (<em>&lt;selector_key&gt;</em>) 和值 (<em>&lt;selector_value&gt;</em>) 对。要将 pod 设定为目标并将其包含在服务负载均衡中，请检查 <em>&lt;selectorkey&gt;</em> 和 <em>&lt;selectorvalue&gt;</em> 值。确保这些值与部署 YAML 的 <code>spec.template.metadata.labels</code> 部分中使用的<em>键/值</em>对相同。</td>
        </tr>
        <tr>
          <td><code>port</code></td>
          <td>服务侦听的端口。</td>
        </tr>
        <tr>
          <td><code>loadBalancerIP</code></td>
          <td>要创建专用 LoadBalancer 或要将特定可移植 IP 地址用于公共 LoadBalancer，请将 <em>&lt;IP_address&gt;</em> 替换为要使用的 IP 地址。有关更多信息，请参阅 [Kubernetes 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer)。</td>
        </tr>
        </tbody></table>

      3. 可选：通过在 **spec** 部分中指定 `loadBalancerSourceRanges` 来配置防火墙。有关更多信息，请参阅 [Kubernetes 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/tasks/access-application-cluster/configure-cloud-provider-firewall/)。

      4. 在集群中创建服务。

          ```
                  kubectl apply -f myloadbalancer.yaml
        ```
          {: pre}

          创建 LoadBalancer 服务时，会自动为负载均衡器分配一个可移植 IP 地址。如果没有可移植 IP 地址可用，那么无法创建 LoadBalancer 服务。


3.  验证 LoadBalancer 服务是否已成功创建。将 _&lt;myservice&gt;_ 替换为在上一步中创建的 LoadBalancer 服务的名称。

    ```
        kubectl describe service myloadbalancer
    ```
    {: pre}

    **注**：可能需要几分钟时间，才能正确创建 LoadBalancer 服务并使应用程序可用。

    示例 CLI 输出：

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

    **LoadBalancer Ingress** IP 地址是分配给 LoadBalancer 服务的可移植 IP 地址。


4.  如果创建了公共负载均衡器，请从因特网访问该应用程序。
    1.  打开首选的 Web 浏览器。
    2.  输入负载均衡器的可移植公共 IP 地址和端口。

        ```
                http://169.xx.xxx.xxx:8080
        ```
        {: codeblock}        

5. 如果选择[为 LoadBalancer 服务启用源 IP 保留 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-typeloadbalancer)，请确保通过[向应用程序 pod 添加边缘节点亲缘关系](cs_loadbalancer.html#edge_nodes)，将应用程序 pod 安排到边缘工作程序节点。应用程序 pod 必须安排到边缘节点才能接收入局请求。

6. 可选：要处理从其他专区到应用程序的入局请求，请重复上述步骤以在每个专区中添加一个负载均衡器。

</staging>
    ## 使用 LoadBalancer 服务启用对应用程序的公共或专用访问权
{: #config}

开始之前：

-   此功能仅可用于标准集群。
-   必须有可移植公共或专用 IP 地址可用于分配给 LoadBalancer 服务。
-   具有可移植专用 IP 地址的 LoadBalancer 服务仍会在每个工作程序节点上打开一个公共 NodePort。要添加网络策略以防止公共流量，请参阅[阻止入局流量](cs_network_policy.html#block_ingress)。

要创建 LoadBalancer 服务，请执行以下操作：

1.  [将应用程序部署到集群](cs_app.html#app_cli)。将应用程序部署到集群时，将创建一个或多个 pod 以用于在容器中运行应用程序。确保在配置文件的 metadata 部分中添加针对您的部署的标签。需要此标签才能识别运行应用程序的所有 pod，以便可以将这些 pod 包含在负载均衡中。
2.  针对要公开的应用程序创建 LoadBalancer 服务。要使应用程序在公共因特网或专用网络上可用，请为您的应用程序创建 Kubernetes 服务。配置该服务以在负载均衡中包含构成该应用程序的所有 pod。
    1.  创建名为 `myloadbalancer.yaml`（举例来说）的服务配置文件。

    2.  针对要公开的应用程序定义 LoadBalancer 服务。
        - 如果集群位于公用 VLAN 上，那么将使用可移植的公共 IP 地址。大多数集群都位于公用 VLAN 上。
        - 如果集群仅在专用 VLAN 上可用，那么将使用可移植的专用 IP 地址。
        - 通过向配置文件添加注释，可请求用于 LoadBalancer 服务的可移植公共或专用 IP 地址。

        使用缺省 IP 地址的 LoadBalancer 服务：

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

        使用注释来指定专用或公共 IP 地址的 LoadBalancer 服务：

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
        <caption>了解 YAML 文件的组成部分</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
        </thead>
        <tbody>
        <tr>
          <td><code>selector</code></td>
          <td>输入要用于将运行应用程序的 pod 设定为目标的标签键 (<em>&lt;selector_key&gt;</em>) 和值 (<em>&lt;selector_value&gt;</em>) 对。要将 pod 设定为目标并将其包含在服务负载均衡中，请检查 <em>&lt;selector_key&gt;</em> 和 <em>&lt;selector_value&gt;</em> 值。确保这些值与部署 YAML 的 <code>spec.template.metadata.labels</code> 部分中使用的<em>键/值</em>对相同。</td>
        </tr>
        <tr>
          <td><code>port</code></td>
          <td>服务侦听的端口。</td>
        </tr>
        <tr>
          <td>`service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type:`
          <td>指定 LoadBalancer 类型的注释。接受的值为 `private` 和 `public`。如果要在公用 VLAN 上的集群中创建公共 LoadBalancer，那么不需要此注释。</td>
        </tr>
        <tr>
          <td><code>loadBalancerIP</code></td>
          <td>要创建专用 LoadBalancer 或要将特定可移植 IP 地址用于公共 LoadBalancer，请将 <em>&lt;IP_address&gt;</em> 替换为要使用的 IP 地址。有关更多信息，请参阅 [Kubernetes 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer)。</td>
        </tr>
        </tbody></table>

    3.  可选：通过在 **spec** 部分中指定 `loadBalancerSourceRanges` 来配置防火墙。有关更多信息，请参阅 [Kubernetes 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/tasks/access-application-cluster/configure-cloud-provider-firewall/)。

    4.  在集群中创建服务。

        ```
                kubectl apply -f myloadbalancer.yaml
        ```
        {: pre}

        创建 LoadBalancer 服务时，会自动为负载均衡器分配一个可移植 IP 地址。如果没有可移植 IP 地址可用，那么无法创建 LoadBalancer 服务。


3.  验证 LoadBalancer 服务是否已成功创建。

    ```
        kubectl describe service myloadbalancer
    ```
    {: pre}

    **注**：可能需要几分钟时间，才能正确创建 LoadBalancer 服务并使应用程序可用。

    示例 CLI 输出：

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

    **LoadBalancer Ingress** IP 地址是分配给 LoadBalancer 服务的可移植 IP 地址。


4.  如果创建了公共负载均衡器，请从因特网访问该应用程序。
    1.  打开首选的 Web 浏览器。
    2.  输入负载均衡器的可移植公共 IP 地址和端口。

        ```
                http://169.xx.xxx.xxx:8080
        ```
        {: codeblock}

5. 如果选择[为 LoadBalancer 服务启用源 IP 保留 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-typeloadbalancer)，请确保通过[向应用程序 pod 添加边缘节点亲缘关系](cs_loadbalancer.html#edge_nodes)，将应用程序 pod 安排到边缘工作程序节点。应用程序 pod 必须安排到边缘节点才能接收入局请求。

<br />


## 向源 IP 的应用程序 pod 添加节点亲缘关系和容忍度
{: #node_affinity_tolerations}

每当部署应用程序 pod 时，LoadBalancer 服务 pod 也会部署到应用程序 pod 所部署到的工作程序节点上。但是，在某些情况下，负载均衡器 pod 和应用程序 pod 可能未安排到同一工作程序节点上：
{: shortdesc}

* 您具有有污点的边缘节点，所以只有 LoadBalancer 服务 pod 才能部署到这些节点。不允许应用程序 pod 部署到这些节点。
* 集群连接到多个公用或专用 VLAN，但应用程序 pod 可能会部署到仅连接到一个 VLAN 的工作程序节点。LoadBalancer 服务 pod 可能不会部署到这些工作程序节点，因为负载均衡器 IP 地址连接到的 VLAN 与工作程序节点连接到的不同。

对应用程序的客户机请求发送到集群时，该请求会路由到用于公开该应用程序的 Kubernetes LoadBalancer 服务的 pod。如果在 LoadBalancer 服务 pod 所在的工作程序节点上不存在应用程序 pod，那么负载均衡器会将该请求转发到其他工作程序节点上的应用程序 pod。软件包的源 IP 地址将更改为运行应用程序 pod 的工作程序节点的公共 IP 地址。

要保留客户机请求的原始源 IP 地址，可以为 LoadBalancer 服务[启用源 IP ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-typeloadbalancer)。例如，在应用程序服务器必须应用安全性和访问控制策略的情况下，保留客户机的 IP 非常有用。启用源 IP 后，LoadBalancer 服务 pod 必须将请求转发到仅部署到同一工作程序节点的应用程序 pod。要强制将应用程序部署到 LoadBalancer 服务 pod 也可以部署到的特定工作程序节点，必须将亲缘关系规则和容忍度添加到应用程序部署。

### 添加边缘节点亲缘关系规则和容忍度
{: #edge_nodes}

[将工作程序节点标记为边缘节点](cs_edge.html#edge_nodes)并且还[污染边缘节点](cs_edge.html#edge_workloads)后，LoadBalancer 服务 pod 仅部署到这些边缘节点，应用程序 pod 无法部署到边缘节点。为 LoadBalancer 服务启用源 IP 后，边缘节点上的负载均衡器 pod 无法将入局请求转发到其他工作程序节点上的应用程序 pod。
{:shortdesc}

要强制应用程序 pod 部署到边缘节点，请将边缘节点[亲缘关系规则 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#node-affinity-beta-feature) 和[容忍度 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/#concepts) 添加到应用程序部署。

具有边缘节点亲缘关系和边缘节点容忍度的部署 YAML 示例：

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

**affinity** 和 **tolerations** 部分都将 `dedicated` 作为 `key`，将 `edge` 作为 `value`。

### 为多个公用或专用 VLAN 添加亲缘关系规则
{: #edge_nodes_multiple_vlans}

集群连接到多个公用或专用 VLAN 时，应用程序 pod 可能会部署到仅连接到一个 VLAN 的工作程序节点。如果负载均衡器 IP 地址连接到的 VLAN 与这些工作程序节点连接到的不同，那么 LoadBalancer 服务 pod 不会部署到这些工作程序节点。
{:shortdesc}

启用源 IP 后，通过将亲缘关系规则添加到应用程序部署，将应用程序 pod 安排在负载均衡器的 IP 地址所在的 VLAN 中的工作程序节点上。

开始之前，请[设定 CLI 的目标](cs_cli_install.html#cs_cli_configure)为集群。

1. 获取 LoadBalancer 服务的 IP 地址。在 **LoadBalancer Ingress** 字段中查找该 IP 地址。
    ```
    kubectl describe service <loadbalancer_service_name>
    ```
    {: pre}

2. 检索 LoadBalancer 服务连接到的 VLAN 标识。

    1. 列出集群的可移植公用 VLAN。
        ```
        bx cs cluster-get <cluster_name_or_ID> --showResources
        ```
        {: pre}

        输出示例：
        ```
        ...

        Subnet VLANs
        VLAN ID   Subnet CIDR       Public   User-managed
        2234947   10.xxx.xx.xxx/29  false    false
        2234945   169.36.5.xxx/29   true     false
        ```
        {: screen}

    2. 在输出的 **Subnet VLANs** 下，查找与先前检索到的负载均衡器 IP 地址相匹配的子网 CIDR，并记下相应的 VLAN 标识。

        例如，如果 LoadBalancer 服务 IP 地址为 `169.36.5.xxx`，那么上一步的示例输出中的匹配子网为 `169.36.5.xxx/29`。该子网连接到的 VLAN 标识为 `2234945`。

3. 向上一步中记录的 VLAN 标识的应用程序部署[添加亲缘关系规则 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#node-affinity-beta-feature)。

    例如，如果您有多个 VLAN，但希望应用程序 pod 仅部署到 `2234945` 公用 VLAN 上的工作程序节点：

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

    在示例 YAML 中，**affinity** 部分将 `publicVLAN` 作为 `key`，将 `"2234945"` 作为 `value`。

4. 应用已更新的部署配置文件。
    ```
    kubectl apply -f with-node-affinity.yaml
    ```
    {: pre}

5. 验证应用程序 pod 是否已部署到连接至指定 VLAN 的工作程序节点。

    1. 列出集群中的 pod。将 `<selector>` 替换为用于应用程序的标签。
        ```
        kubectl get pods -o wide app=<selector>
        ```
        {: pre}

        输出示例：
        ```
                NAME                   READY     STATUS              RESTARTS   AGE       IP               NODE
        cf-py-d7b7d94db-vp8pq  1/1       Running             0          15d       172.30.xxx.xxx   10.176.48.78
        ```
        {: screen}

    2. 在输出中，确定应用程序的 pod。记下该 pod 所在的工作程序节点的 **NODE** 标识。

        在上一步的示例输出中，应用程序 pod `cf-py-d7b7d94db-vp8pq` 位于工作程序节点 `10.176.48.78` 上。

    3. 列出工作程序节点的详细信息。

        ```
                kubectl describe node <worker_node_ID>
        ```
        {: pre}

        输出示例：

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

    4. 在输出的 **Labels** 部分中，验证公用或专用 VLAN 是否为在先前步骤中指定的 VLAN。
