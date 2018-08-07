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





# 更新集群和工作程序节点
{: #update}

可以通过安装更新，使 {{site.data.keyword.containerlong}} 中的 Kubernetes 集群保持最新。
{:shortdesc}

## 更新 Kubernetes 主节点
{: #master}

Kubernetes 会定期发布[主要更新、次要更新或补丁更新](cs_versions.html#version_types)。根据更新类型，您可能要负责更新 Kubernetes 主节点组件。
{:shortdesc}

更新会影响 Kubernetes 主节点中的 Kubernetes API 服务器版本或其他组件。使工作程序节点保持最新始终是您的责任。进行更新时，会先更新 Kubernetes 主节点，后更新工作程序节点。


缺省情况下，您最多只能跨当前版本 Kubernetes 主节点的两个次版本来更新 Kubernetes API 服务器。例如，如果当前 Kubernetes API 服务器的版本是 1.7，而您要更新到 1.10，那么必须先更新到 1.8 或 1.9。可以强制更新执行，但跨三个或三个以上的次版本更新可能会导致意外结果。如果集群运行的是不支持的 Kubernetes 版本，那么可能必须强制执行此更新。

下图显示更新主节点时可以执行的流程。

![主节点更新最佳实践](/images/update-tree.png)

图 1. Kubernetes 主节点更新流程图

**注意**：执行更新过程后，即不能将集群回滚到先前版本。请确保使用测试集群并遵循指示信息来解决潜在问题，然后再更新生产主节点。

对于_主要_或_次要_更新，请完成以下步骤：

1. 查看 [Kubernetes 更改](cs_versions.html)，并对任何更新标记为_在更新主节点之前更新_。
2. 使用 GUI 或运行 [CLI 命令](cs_cli_reference.html#cs_cluster_update)来更新 Kubernetes API 服务器和关联的 Kubernetes 主节点组件。更新 Kubernetes API 服务器时，该 API 服务器将关闭约 5 到 10 分钟。在更新期间，您无法访问或更改集群。但是，不会修改集群用户已部署的工作程序节点、应用程序和资源，并继续运行。
3. 确认更新已完成。在 {{site.data.keyword.Bluemix_notm}}“仪表板”上查看 Kubernetes API 服务器版本，或运行 `bx cs clusters`。
4. 安装与 Kubernetes 主节点中运行的 Kubernetes API 服务器版本相匹配的 [`kubectl cli`](cs_cli_install.html#kubectl) 版本。

Kubernetes API 服务器更新完成后，可以更新工作程序节点。

<br />


## 更新工作程序节点
{: #worker_node}


您收到了工作程序节点更新通知。这意味着什么呢？由于 Kubernetes API 服务器和其他 Kubernetes 主节点组件的安全性更新和补丁已到位，因此您必须确保工作程序节点保持同步。
{: shortdesc}

工作程序节点 Kubernetes 版本不能高于在 Kubernetes 主节点中运行的 Kubernetes API 服务器版本。开始之前，请[更新 Kubernetes 主节点](#master)。

**注意**：
<ul><li>更新工作程序节点可能会导致应用程序和服务产生停机时间。</li>
<li>如果数据未存储在 pod 外，那么将删除数据。</li>
<li>在部署中使用 [replicas ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#replicas) 以在可用节点上重新安排 pod。</li></ul>

但如果我不能有停机时间该怎么办？

作为更新过程的一部分，特定的节点将停止运行一段时间。为了帮助避免应用程序产生停机时间，您可以在配置映射中定义唯一键，用于指定升级过程中特定类型节点的阈值百分比。通过定义基于标准 Kubernetes 标签的规则并给出允许不可用的最大节点数的百分比，可以确保应用程序保持开启和运行。如果节点尚未完成部署过程，那么该节点会视为不可用。

如何定义键？

在配置映射的“数据信息”部分中，您最多可以定义 10 个单独规则以在任意给定时间运行。要进行升级，工作程序节点必须通过每条定义的规则。

键已定义。现在，我该怎样做？

定义规则后，请运行 `bx cs worker-update` 命令。如果返回成功的响应，说明工作程序节点在排队等待更新。但是，节点须满足所有规则后，才会执行更新过程。节点排队时，会定期检查规则，确定是否有任何节点可以更新。

如果选择不定义配置映射会怎样？

未定义配置映射时，将使用缺省值。缺省情况下，在更新过程中，每个集群的所有工作程序节点中最多有 20% 的节点不可用。

要更新工作程序节点，请执行以下操作：

1. 落实在 [Kubernetes 更改](cs_versions.html)中标记为_在主节点后更新_的任何更改。

2. 可选：定义配置映射。
    示例：

    ```
        apiVersion: v1
    kind: ConfigMap
    metadata:
      name: ibm-cluster-update-configuration
      namespace: kube-system
    data:
     drain_timeout_seconds: "120"
     zonecheck.json: |
       {
         "MaxUnavailablePercentage": 70,
         "NodeSelectorKey": "failure-domain.beta.kubernetes.io/zone",
         "NodeSelectorValue": "dal13"
       }
     regioncheck.json: |
       {
         "MaxUnavailablePercentage": 80,
         "NodeSelectorKey": "failure-domain.beta.kubernetes.io/region",
         "NodeSelectorValue": "us-south"
       }
    defaultcheck.json: |
       {
         "MaxUnavailablePercentage": 100
       }
    ```
    {:pre}
  <table summary="表中的第一行跨两列。其余行应从左到右阅读，其中第一列是参数，第二列是匹配的描述。">
    <caption>ConfigMap 组成部分</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解组成部分</th>
    </thead>
    <tbody>
      <tr>
        <td><code>drain_timeout_seconds</code></td>
        <td> 可选：工作程序节点更新期间发生的漏出的超时（以秒为单位）。漏出会将节点设置为 `unschedulable`，这将阻止新 pod 部署到该节点。漏出还会删除该节点的 pod。接受的值为 1 到 180 之间的整数。缺省值为 30。</td>
      </tr>
      <tr>
        <td><code>zonecheck.json</code></br><code>regioncheck.json</code></td>
        <td> 要为其设置规则的唯一键的示例。键名可以根据需要设置；信息由键内设置的配置进行解析。对于定义的每个键，只能为 <code>NodeSelectorKey</code> 和 <code>NodeSelectorValue</code> 设置一个值。如果要为多个区域或位置（数据中心）设置规则，请创建新的键条目。</td>
      </tr>
      <tr>
        <td><code>defaultcheck.json</code></td>
        <td> 缺省情况下，如果未以有效方式定义 <code>ibm-cluster-update-configuration</code> 映射，那么集群中只能有 20% 的节点同时不可用。如果在未使用全局缺省值的情况下定义了一个或多个有效规则，那么新的缺省值是允许 100% 的工作程序同时不可用。您可以通过创建缺省百分比来控制此比例。</td>
      </tr>
      <tr>
        <td><code>MaxUnavailablePercentage</code></td>
        <td> 对于指定键，允许不可用的最大节点数，指定为百分比。节点在部署、重新装入或供应过程中会不可用。如果排队的工作程序节点超过任何已定义的最大不可用百分比，那么会阻止这些节点升级。</td>
      </tr>
      <tr>
        <td><code>NodeSelectorKey</code></td>
        <td> 要为其设置规则的指定键标签的类型。您可以为 IBM 提供的缺省标签以及您创建的标签设置规则。</td>
      </tr>
      <tr>
        <td><code>NodeSelectorValue</code></td>
        <td> 指定键内规则设置为评估的节点子集。</td>
      </tr>
    </tbody>
  </table>

    **注**：最多可以定义 10 个规则。如果向一个文件添加的键超过 10 个，那么只会解析其中一部分信息。

3. 通过 GUI 或运行 CLI 命令来更新工作程序节点。
  * 要从 {{site.data.keyword.Bluemix_notm}} 仪表板进行更新，请浏览到集群的 `Worker Nodes` 部分，然后单击 `Update Worker`。
  * 要获取工作程序节点标识，请运行 `bx cs workers <cluster_name_or_ID>`. 如果选择多个工作程序节点，那么会将这些工作程序节点放入队列等待更新评估。如果在评估后认为工作程序节点已经准备就绪，那么将根据配置中设置的规则对其进行更新。

    ```
        bx cs worker-update <cluster_name_or_ID> <worker_node1_ID> <worker_node2_ID>
    ```
    {: pre}

4. 可选：通过运行以下命令并查看**事件**，验证由配置映射触发的事件以及发生的任何验证错误。
    ```
    kubectl describe -n kube-system cm ibm-cluster-update-configuration
    ```
    {: pre}

5. 确认更新是否已完成：
  * 在 {{site.data.keyword.Bluemix_notm}}“仪表板”上查看 Kubernetes 版本，或运行 `bx cs workers <cluster_name_or_ID>`.
  * 通过运行 `kubectl get nodes`，查看工作程序节点的 Kubernetes 版本。
  * 在某些情况下，较旧的集群可能会在更新后列出具有 **NotReady** 状态的重复工作程序节点。要除去重复项，请参阅[故障诊断](cs_troubleshoot_clusters.html#cs_duplicate_nodes)。

后续步骤：
  - 对其他集群重复更新过程。
  - 通知在集群中工作的开发者将其 `kubectl` CLI 更新到 Kubernetes 主节点的版本。
  - 如果 Kubernetes 仪表板未显示利用率图形，请[删除 `kube-dashboard` pod](cs_troubleshoot_health.html#cs_dashboard_graphs)。
  





<br />



## 更新机器类型
{: #machine_type}

可以通过添加新工作程序节点并除去旧工作程序节点来更新工作程序节点的机器类型。例如，如果在其名称中含有 `u1c` 或 `b1c` 的不推荐机器类型上具有虚拟工作程序节点，请创建使用其名称中含有 `u2c` 或 `b2c` 的机器类型的工作程序节点。
{: shortdesc}

开始之前：
- [设定 CLI 的目标](cs_cli_install.html#cs_cli_configure)为集群。
- 如果是将数据存储在工作程序节点上，而没有[存储在工作程序节点外部](cs_storage.html#storage)，那么将删除数据。


**注意**：更新工作程序节点可能会导致应用程序和服务产生停机时间。如果数据未[存储在 pod 外部](cs_storage.html#storage)，那么将删除数据。



1. 记下要更新的工作程序节点的名称和位置。
    ```
    bx cs workers <cluster_name>
    ```
    {: pre}

2. 查看可用的机器类型。
    ```
    bx cs machine-types <location>
    ```
    {: pre}

3. 使用 [bx cs worker-add](cs_cli_reference.html#cs_worker_add) 命令来添加工作程序节点。请指定机器类型。

    ```
        bx cs worker-add --cluster <cluster_name> --machine-type <machine_type> --number <number_of_worker_nodes> --private-vlan <private_VLAN_ID> --public-vlan <public_VLAN_ID>
    ```
    {: pre}

4. 验证工作程序节点是否已添加。

    ```
        bx cs workers <cluster_name>
    ```
    {: pre}

5. 添加的工作程序节点处于 `Normal` 状态时，可以除去过时的工作程序节点。**注**：如果要除去按月计费的机器类型（如裸机），那么仍将对整个月收费。

    ```
        bx cs worker-rm <cluster_name> <worker_node>
    ```
    {: pre}

6. 重复这些步骤以将其他工作程序节点升级到不同的机器类型。










