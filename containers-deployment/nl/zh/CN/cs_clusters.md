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


# 设置集群
{: #clusters}

通过 {{site.data.keyword.containerlong}} 设计 Kubernetes 集群设置以实现最大容器可用性和容量。
{:shortdesc}



## 集群配置规划
{: #planning_clusters}

使用标准集群来提高应用程序可用性。
{:shortdesc}

在多个工作程序节点和集群之间分发设置时，用户不太可能会遇到停机时间。内置功能（例如负载均衡和隔离）可在主机、网络或应用程序发生潜在故障时更快恢复。


查看以下潜在的集群设置（按可用性程度从低到高排序）：

![集群的高可用性阶段](images/cs_cluster_ha_roadmap.png)

1.  一个集群具有多个工作程序节点
2.  在同一区域的不同位置运行的两个集群，每个集群具有多个工作程序节点
3.  在不同区域运行的两个集群，每个集群具有多个工作程序节点


### 提高集群的可用性

<dl>
  <dt>跨工作程序节点分布应用程序</dt>
    <dd>允许开发者在每个集群的多个工作程序节点之间将其应用程序分布在容器中。三个工作程序节点中每个节点的应用程序实例允许有一个工作程序节点发生停机，而不会中断应用程序使用。在通过 [{{site.data.keyword.Bluemix_notm}} GUI](cs_clusters.html#clusters_ui) 或 [CLI](cs_clusters.html#clusters_cli) 创建集群时，可以指定要包含的工作程序节点数。Kubernetes 限制了可在集群中使用的最大工作程序节点数，因此请记住[工作程序节点和 pod 配额 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/admin/cluster-large/)。<pre class="codeblock"><code>bx cs cluster-create --location dal10 --workers 3 --public-vlan &lt;public_VLAN_ID&gt; --private-vlan &lt;private_VLAN_ID&gt; --machine-type &lt;u2c.2x4&gt; --name &lt;cluster_name_or_ID&gt;</code></pre></dd>
<dt>跨集群散布应用程序</dt>
    <dd>创建多个集群，每个集群具有多个工作程序节点。如果一个集群发生停机，用户仍可以访问还部署在其他集群中的应用程序。<p>集群 1：</p>
        <pre class="codeblock"><code>bx cs cluster-create --location dal10 --workers 3 --public-vlan &lt;public_VLAN_ID&gt; --private-vlan &lt;private_VLAN_ID&gt; --machine-type &lt;u2c.2x4&gt; --name &lt;cluster_name_or_ID&gt;</code></pre>
<p>集群 2：</p>
        <pre class="codeblock"><code>bx cs cluster-create --location dal12 --workers 3 --public-vlan &lt;public_VLAN_ID&gt; --private-vlan &lt;private_VLAN_ID&gt; --machine-type &lt;u2c.2x4&gt; --name &lt;cluster_name_or_ID&gt;</code></pre></dd>
  <dt>跨不同区域的集群散布应用程序</dt>
    <dd>跨不同区域的集群散布应用程序时，可以允许基于用户所在的区域进行负载均衡。如果一个区域中的集群、硬件甚至整个位置当机，那么流量会路由到另一个位置内所部署的容器。<p><strong>重要信息</strong>：配置定制域后，可使用以下命令来创建集群。</p>
      <p>位置 1：</p>
        <pre class="codeblock"><code>bx cs cluster-create --location dal10 --workers 3 --public-vlan &lt;public_VLAN_ID&gt; --private-vlan &lt;private_VLAN_ID&gt; --machine-type &lt;u2c.2x4&gt; --name &lt;cluster_name_or_ID&gt;</code></pre>
<p>位置 2：</p>
        <pre class="codeblock"><code>bx cs cluster-create --location ams03 --workers 3 --public-vlan &lt;public_VLAN_ID&gt; --private-vlan &lt;private_VLAN_ID&gt; --machine-type &lt;u2c.2x4&gt; --name &lt;cluster_name_or_ID&gt;</code></pre></dd>
</dl>

<br />







## 工作程序节点配置规划
{: #planning_worker_nodes}

Kubernetes 集群由工作程序节点组成，并由 Kubernetes 主节点机进行集中监视和管理。集群管理员决定如何设置工作程序节点的集群，以确保集群用户具备在集群中部署和运行应用程序所需的所有资源。
{:shortdesc}

创建标准集群时，会在 IBM Cloud Infrastructure (SoftLayer) 中代表您订购工作程序节点，然后将其添加到集群中的缺省工作程序节点池。为每个工作程序节点分配唯一的工作程序节点标识和域名，在创建集群后，不得更改该标识和域名。

您可以选择虚拟服务器或物理（裸机）服务器。根据选择的硬件隔离级别，可以将虚拟工作程序节点设置为共享或专用节点。您还可以选择是希望工作程序节点连接到公用 VLAN 和专用 VLAN，还是仅连接到专用 VLAN。每个工作程序节点都供应有特定机器类型，用于确定部署到该工作程序节点的容器可用的 vCPU 数、内存量和磁盘空间量。
Kubernetes 限制了在一个集群中可以拥有的最大工作程序节点数。有关更多信息，请查看[工作程序节点和 pod 配额 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/admin/cluster-large/)。



### 工作程序节点的硬件
{: #shared_dedicated_node}

在 {{site.data.keyword.Bluemix_notm}} 中创建标准集群时，可以选择将工作程序节点作为物理机器（裸机）进行供应，或作为在物理硬件上运行的虚拟机进行供应。创建免费集群时，工作程序节点会自动作为 IBM Cloud Infrastructure (SoftLayer) 帐户中的虚拟共享节点进行供应。
{:shortdesc}

![标准集群中工作程序节点的硬件选项](images/cs_clusters_hardware.png)

查看以下信息以确定所需的工作程序池的类型。规划时，请考虑将[工作程序节点限制最小阈值](#resource_limit_node)设置为总内存容量的 10%。

<dl>
<dt>为何要使用物理机器（裸机）？</dt>
<dd><p><strong>更多计算资源</strong>：可以将工作程序节点作为单租户物理服务器（也称为裸机）进行供应。通过裸机，您可以直接访问机器上的物理资源，例如内存或 CPU。此设置无需虚拟机系统管理程序将物理资源分配给在主机上运行的虚拟机。相反，裸机机器的所有资源都仅供工作程序专用，因此您无需担心“吵闹的邻居”共享资源或降低性能。物理机器类型的本地存储器大于虚拟机，并且某些类型具有用于备份本地数据的 RAID。</p>
<p><strong>按月计费</strong>：裸机服务器比虚拟服务器更昂贵，最适用于需要更多资源和主机控制的高性能应用程序。裸机服务器按月计费。如果您在月底之前取消裸机服务器，那么仍将收取该整月的费用。订购和取消裸机服务器是通过 IBM Cloud Infrastructure (SoftLayer) 帐户进行的手动过程。完成此过程可能需要超过一个工作日的时间。</p>
<p><strong>用于启用可信计算的选项</strong>：启用“可信计算”以验证工作程序节点是否被篡改。如果在创建集群期间未启用信任，但希望日后启用，那么可以使用 `bx cs feature-enable` [命令](cs_cli_reference.html#cs_cluster_feature_enable)。启用信任后，日后无法将其禁用。可以创建不含信任的新集群。有关节点启动过程中的信任工作方式的更多信息，请参阅[具有可信计算的 {{site.data.keyword.containershort_notm}}](cs_secure.html#trusted_compute)。在运行 Kubernetes V1.9 或更高版本并具有特定裸机机器类型的集群上，可信计算可用。运行 `bx cs machine-types <location>` [命令](cs_cli_reference.html#cs_machine_types)后，可以通过查看 **Trustable** 字段来了解哪些机器支持信任。例如，`mgXc` GPU 类型模板不支持可信计算。</p></dd>
<dt>为什么要使用虚拟机？</dt>
<dd><p>相对于裸机，使用虚拟机 (VM) 能以更具成本效益的价格获得更高灵活性、更短供应时间以及更多自动可扩展性功能。您可以将 VM 用于最通用的用例，例如测试和开发环境、编译打包和生产环境、微服务以及业务应用程序。但是，在性能方面会有所牺牲。如果需要针对 RAM 密集型、数据密集型或 GPU 密集型工作负载进行高性能计算，请使用裸机。</p>
<p><strong>确定是单租户还是多租户</strong>：创建标准虚拟集群时，必须选择是希望底层硬件由多个 {{site.data.keyword.IBM_notm}} 客户共享（多租户）还是仅供您专用（单租户）。</p>
<p>在多租户设置中，物理资源（如 CPU 和内存）在部署到同一物理硬件的所有虚拟机之间共享。要确保每个虚拟机都能独立运行，虚拟机监视器（也称为系统管理程序）会将物理资源分段成隔离的实体，并将其作为专用资源分配给虚拟机（系统管理程序隔离）。</p>
<p>在单租户设置中，所有物理资源都仅供您专用。您可以将多个工作程序节点作为虚拟机部署在同一物理主机上。与多租户设置类似，系统管理程序也会确保每个工作程序节点在可用物理资源中获得应有的份额。</p>
<p>共享节点通常比专用节点更便宜，因为底层硬件的开销由多个客户分担。但是，在决定是使用共享还是专用节点时，可能需要咨询您的法律部门，以讨论应用程序环境所需的基础架构隔离和合规性级别。</p>
<p><strong>虚拟 `u2c` 或 `b2c` 机器类型模板</strong>：这些机器使用本地磁盘（而不是存储区联网 (SAN)）来实现可靠性。可靠性优势包括在将字节序列化到本地磁盘时可提高吞吐量，以及减少因网络故障而导致的文件系统降级。这些机器类型包含用于操作系统文件系统的 25 GB 主本地磁盘存储和用于 `/var/lib/docker`（这是所有容器数据写入的目录）的 100 GB 辅助本地磁盘存储。</p>
<p><strong>如果我拥有不推荐使用的 `u1c` 或 `b1c` 机器类型该怎么办？</strong>要开始使用 `u2c` 和 `b2c` 机器类型，请[通过添加工作程序节点来更新机器类型](cs_cluster_update.html#machine_type)。</p></dd>
<dt>我可以选择哪些虚拟机和物理机器类型模板？</dt>
<dd><p>有很多！请选择最适合您用例的机器类型。请记住，一个工作程序池由属于相同类型模板的机器组成。如果要在集群中混合使用机器类型，请为每种类型模板创建单独的工作程序池。</p>
<p>机器类型因专区而变化。要查看专区中可用的机器类型，请运行 `bx cs machine-types<zone_name>`.</p>
<p><table>
<caption>{{site.data.keyword.containershort_notm}} 中的可用物理（裸机）和虚拟机类型。</caption>
<thead>
<th>名称和用例</th>
<th>核心数/内存</th>
<th>主/辅助磁盘</th>
<th>网络速度</th>
</thead>
<tbody>
<tr>
<td><strong>虚拟，u2c.2x4</strong>：对于快速测试、概念验证和其他轻型工作负载，请使用此最小大小的 VM。</td>
<td>2 / 4 GB</td>
<td>25 GB / 100 GB</td>
<td>1000 Mbps</td>
</tr>
<tr>
<td><strong>虚拟，b2c.4x16</strong>：对于测试和开发以及其他轻型工作负载，请选择此均衡的 VM。</td>
<td>4 / 16 GB</td>
<td>25 GB / 100 GB</td>
<td>1000 Mbps</td>
</tr>
<tr>
<td><strong>虚拟，b2c.16x64</strong>：对于中型工作负载，请选择此均衡的 VM。</td></td>
<td>16 / 64 GB</td>
<td>25 GB / 100 GB</td>
<td>1000 Mbps</td>
</tr>
<tr>
<td><strong>虚拟，b2c.32x128</strong>：对于中型到大型工作负载（例如，具有大量并发用户的数据库和动态 Web 站点），请选择此均衡的 VM。</td></td>
<td>32 / 128 GB</td>
<td>25 GB / 100 GB</td>
<td>1000 Mbps</td>
</tr>
<tr>
<td><strong>虚拟，b2c.56x242</strong>：对于大型工作负载（例如，具有大量并发用户的数据库和多个应用程序），请选择此均衡的 VM。</td></td>
<td>56 / 242 GB</td>
<td>25 GB / 100 GB</td>
<td>1000 Mbps</td>
</tr>
<tr>
<td><strong>RAM 密集型裸机，mr1c.28x512</strong>：最大限度提高可用于工作程序节点的 RAM。</td>
<td>28 / 512 GB</td>
<td>2 TB SATA / 960 GB SSD</td>
<td>10000 Mbps</td>
</tr>
<tr>
<td><strong>GPU 裸机，mg1c.16x128</strong>：对于数学密集型工作负载（例如，高性能计算、机器学习或 3D 应用程序），请选择此类型。此类型模板有 1 块 Tesla K80 物理卡，每块卡有 2 个图形处理单元 (GPU)，共有 2 个 GPU。</td>
<td>16 / 128 GB</td>
<td>2 TB SATA / 960 GB SSD</td>
<td>10000 Mbps</td>
</tr>
<tr>
<td><strong>GPU 裸机，mg1c.28x256</strong>：对于数学密集型工作负载（例如，高性能计算、机器学习或 3D 应用程序），请选择此类型。此类型模板有 2 块 Tesla K80 物理卡，每块卡有 2 个 GPU，共有 4 个 GPU。</td>
<td>28 / 256 GB</td>
<td>2 TB SATA / 960 GB SSD</td>
<td>10000 Mbps</td>
</tr>
<tr>
<td><strong>数据密集型裸机，md1c.16x64.4x4tb</strong>：适用于需要大量本地磁盘存储的情况，包括用于备份机器上本地存储的数据的 RAID。用于分布式文件系统、大型数据库和大数据分析工作负载等用例。</td>
<td>16 / 64 GB</td>
<td>2 个 2 TB RAID1 / 4 个 4 TB SATA RAID10</td>
<td>10000 Mbps</td>
</tr>
<tr>
<td><strong>数据密集型裸机，md1c.28x512.4x4tb</strong>：适用于需要大量本地磁盘存储的情况，包括用于备份机器上本地存储的数据的 RAID。用于分布式文件系统、大型数据库和大数据分析工作负载等用例。</td>
<td>28 / 512 GB</td>
<td>2 个 2 TB RAID1 / 4 个 4 TB SATA RAID10</td>
<td>10000 Mbps</td>
</tr>
<tr>
<td><strong>均衡裸机，mb1c.4x32</strong>：用于需要的计算资源比虚拟机所提供的计算资源更多的均衡工作负载。</td>
<td>4 / 32 GB</td>
<td>2 TB SATA / 2 TB SATA</td>
<td>10000 Mbps</td>
</tr>
<tr>
<td><strong>均衡裸机，mb1c.16x64</strong>：用于需要的计算资源比虚拟机所提供的计算资源更多的均衡工作负载。</td>
<td>16 / 64 GB</td>
<td>2 TB SATA / 960 GB SSD</td>
<td>10000 Mbps</td>
</tr>
</tbody>
</table>
</p>
</dd>
</dl>


可以使用[控制台 UI](#clusters_ui) 或 [CLI](#clusters_cli) 来部署集群。

### 工作程序节点的 VLAN 连接
{: #worker_vlan_connection}

创建集群时，每个集群都会自动通过 IBM Cloud infrastructure (SoftLayer) 帐户连接到 VLAN。
{:shortdesc}

VLAN 会将一组工作程序节点和 pod 视为连接到同一物理连线那样进行配置。

* 公用 VLAN 上自动供应了两个子网。主公用子网确定在集群创建期间分配给工作程序节点的公共 IP 地址，可移植公用子网为 Ingress 和负载均衡器联网服务提供公共 IP 地址。
* 专用 VLAN 上也自动供应了两个子网。主专用子网确定在集群创建期间分配给工作程序节点的专用 IP 地址，可移植专用子网为 Ingress 和负载均衡器联网服务提供专用 IP 地址。

对于免费集群，缺省情况下集群的工作程序节点会在集群创建期间连接到 IBM 拥有的公用 VLAN 和专用 VLAN。

对于标准集群，首次在某个位置创建集群时，会自动供应公用 VLAN 和专用 VLAN。对于在该位置创建的每个后续集群，可选择要使用的 VLAN。可以将工作程序节点连接到公用 VLAN 和专用 VLAN，也可以仅连接到专用 VLAN。如果要将工作程序节点仅连接到专用 VLAN，那么可以在集群创建期间使用现有专用 VLAN 的标识，或者[创建专用 VLAN](/docs/cli/reference/softlayer/index.html#sl_vlan_create) 并使用其标识。如果工作程序节点设置为仅使用专用 VLAN，那么必须为网络连接配置备用解决方案（例如，[虚拟路由器设备](cs_vpn.html#vyatta)），以便工作程序节点可以与主节点通信。

**注**：如果有多个 VLAN 用于一个集群，或者在同一 VLAN 上有多个子网，那么必须开启 VLAN 生成，以便工作程序节点可以在专用网络上相互通信。有关指示信息，请参阅[启用或禁用 VLAN 生成](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning)。

### 工作程序节点内存限制
{: #resource_limit_node}

{{site.data.keyword.containershort_notm}} 会对每个工作程序节点设置内存限制。在工作程序节点上运行的 pod 超过此内存限制时，将除去 pod。在 Kubernetes 中，此限制称为[硬逐出阈值 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/tasks/administer-cluster/out-of-resource/#hard-eviction-thresholds)。
{:shortdesc}

如果需要频繁除去 pod，请向集群添加更多工作程序节点，或者对 pod 设置[资源限制 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#resource-requests-and-limits-of-pod-and-container)。

**每台机器的最小阈值等于其总内存容量的 10%**。如果工作程序节点上的可用内存小于允许的最小阈值，那么 Kubernetes 将立即除去 pod。如果有工作程序节点可用，那么 pod 会重新安排到其他工作程序节点上。例如，如果您有一个 `b2c.4x16` 虚拟机，其总内存容量为 16 GB。如果可用内存小于 1600 MB (10%)，那么无法将新 pod 安排到此工作程序节点上，而是改为安排到其他工作程序节点上。如果没有其他工作程序节点可用，那么新 pod 将保持不安排。

要查看工作程序节点上使用的内存量，请运行 [kubectl top node ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/reference/kubectl/overview/#top)。

### 自动恢复工作程序节点
`Docker`、`kubelet`、`kube-proxy` 和 `calico` 是关键组件，这些组件必须正常运行才能拥有正常运行的 Kubernetes 工作程序节点。随着时间变化，这些组件可能会中断，这可能使工作程序节点处于非正常运行状态。非正常运行的工作节点会使集群总容量下降，并可能导致应用程序产生停机时间。

可以[为工作程序节点配置运行状况检查并启用自动恢复](cs_health.html#autorecovery)。如果自动恢复根据配置的检查，检测到运行状况欠佳的工作程序节点，那么自动恢复会触发更正操作，例如在工作程序节点上重装操作系统。有关自动恢复的工作方式的更多信息，请参阅[自动恢复博客 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/)。

<br />



## 使用 GUI 创建集群
{: #clusters_ui}

Kubernetes 集群的用途是定义一组资源、节点、网络和存储设备，以便使应用程序保持高可用性。要部署应用程序，必须先创建集群，并在该集群中设置工作程序节点的定义。
{:shortdesc}





开始之前，您必须具有现买现付或预订 [{{site.data.keyword.Bluemix_notm}} 帐户](https://console.bluemix.net/registration/)，该帐户配置为[访问 IBM Cloud Infrastructure (SoftLayer) 产品服务组合](cs_troubleshoot_clusters.html#cs_credentials)。要试用一些功能，您可以创建免费集群，该集群在 21 天后到期。一次只能有一个免费集群。

您可以随时除去免费集群，但在 21 天后，将删除免费集群及其数据，并且无法进行复原。请务必备份数据。
{: tip}

要通过选择的硬件隔离、专区、API 版本等来完全定制集群，请创建标准集群。

要创建集群，请执行以下操作：

1. 在目录中，选择 **Kubernetes 集群**。

2. 选择要在其中部署集群的区域。

3. 选择集群套餐的类型。可以选择**免费**或**标准**。通过标准集群，您有权访问多种功能，如高可用性环境的多个工作程序节点。

4. 配置集群详细信息。完成适用于要创建的集群类型的步骤。

    1. **免费和标准**：为集群提供名称。名称必须以字母开头，可以包含字母、数字和连字符 (-)，并且不能超过 35 个字符。集群名称和部署集群的区域构成了 Ingress 子域的标准域名。为了确保 Ingress 子域在区域内是唯一的，可能会截断 Ingress 域名中的集群名称并附加随机值。
 
    2. **标准**：选择要在其中部署集群的位置。要获得最佳性能，请选择实际离您最近的位置。请记住，如果选择您所在国家或地区以外的位置，那么您可能需要法律授权，才能将数据实际存储到国外。

    3. **标准**：为集群主节点选择 Kubernetes API 服务器版本。

    4. **标准**：选择硬件隔离类型。“虚拟”按小时计费，“裸机”按月计费。

        - **虚拟 - 专用**：工作程序节点在帐户专用的基础架构上托管。物理资源完全隔离。

        - **虚拟 - 共享**：基础架构资源（例如，系统管理程序和物理硬件）在您与其他 IBM 客户之间共享，但每个工作程序节点只能由您访问。虽然此选项更便宜，并且足以满足大多数情况，但您可能希望使用公司策略来验证性能和基础架构需求。

        - **裸机**：裸机服务器按月计费，通过与 IBM Cloud Infrastructure (SoftLayer) 进行手动交互来供应，可能需要一个工作日以上的时间才能完成。裸机最适用于需要更多资源和主机控制的高性能应用程序。

        确保要供应裸机机器。因为裸机机器是按月计费的，所以如果在错误下单后立即将其取消，也仍然会按整月向您收费。
        {:tip}

    5.  **标准**：选择机器类型。机器类型用于定义在每个工作程序节点中设置并可供容器使用的虚拟 CPU 量、内存量和磁盘空间量。可用的裸机和虚拟机类型随集群的部署位置而变化。创建集群后，可以通过将节点添加到集群来添加不同机器类型。

    6. **标准**：指定集群中需要的工作程序节点数。

    7. **标准**：从 IBM Cloud Infrastructure (SoftLayer) 帐户中选择公用 VLAN（可选）和专用 VLAN（必需）。两个 VLAN 在工作程序节点之间进行通信，但公用 VLAN 还与 IBM 管理的 Kubernetes 主节点进行通信。可以对多个集群使用相同的 VLAN。
**注**：如果工作程序节点设置为仅使用专用 VLAN，那么必须为网络连接配置备用解决方案。有关更多信息，请参阅[工作程序节点的 VLAN 连接](cs_clusters.html#worker_vlan_connection)。

    8. 缺省情况下，已选择**加密本地磁盘**。如果选择清除该复选框，那么不会加密主机的 Docker 数据。


4. 单击**创建集群**。您可以在**工作程序节点**选项卡中查看工作程序节点部署的进度。完成部署后，您可以在**概述**选项卡中看到集群已就绪。**注**：为每个工作程序节点分配了唯一的工作程序节点标识和域名，在创建集群后，不得手动更改该标识和域名。更改标识或域名会阻止 Kubernetes 主节点管理集群。



**接下来要做什么？**

集群启动并开始运行后，可查看以下任务：


-   [安装 CLI 以开始使用集群。](cs_cli_install.html#cs_cli_install)
-   [在集群中部署应用程序。](cs_app.html#app_cli)
-   [在 {{site.data.keyword.Bluemix_notm}} 中设置自己的专用注册表，以存储 Docker 映像并与其他用户共享这些映像。](/docs/services/Registry/index.html)
- 如果有多个 VLAN 用于一个集群，或者在同一 VLAN 上有多个子网，那么必须[开启 VLAN 生成](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning)，以便工作程序节点可以在专用网络上相互通信。
- 如果您有防火墙，那么可能需要[打开必要的端口](cs_firewall.html#firewall)才能使用 `bx`、`kubectl` 或 `calicotl` 命令，以允许来自集群的出站流量，或允许联网服务的入站流量。

<br />


## 使用 CLI 创建集群
{: #clusters_cli}

Kubernetes 集群的用途是定义一组资源、节点、网络和存储设备，以便使应用程序保持高可用性。要部署应用程序，必须先创建集群，并在该集群中设置工作程序节点的定义。
{:shortdesc}

开始之前：
- 您必须具有现买现付或预订 [{{site.data.keyword.Bluemix_notm}} 帐户](https://console.bluemix.net/registration/)，该帐户配置为[访问 IBM Cloud Infrastructure (SoftLayer) 产品服务组合](cs_troubleshoot_clusters.html#cs_credentials)。您可以创建 1 个免费集群来试用部分功能，该集群使用时间为 21 天，或者创建具有所选硬件隔离的可完全定制的标准集群。
- [确保在 IBM Cloud Infrastructure (SoftLayer) 中具有最低的必需许可权来供应标准集群](cs_users.html#infra_access)。

要创建集群，请执行以下操作：

1.  安装 {{site.data.keyword.Bluemix_notm}} CLI 和 [{{site.data.keyword.containershort_notm}} 插件](cs_cli_install.html#cs_cli_install)。

2.  登录到 {{site.data.keyword.Bluemix_notm}} CLI。根据提示，输入您的 {{site.data.keyword.Bluemix_notm}} 凭证。

    ```
        bx login
    ```
    {: pre}

    **注**：如果您有联合标识，请使用 `bx login --sso` 登录到 {{site.data.keyword.Bluemix_notm}} CLI。输入您的用户名，并使用 CLI 输出中提供的 URL 来检索一次性密码。如果不使用 `--sso` 时登录失败，而使用 `--sso` 选项时登录成功，说明您拥有的是联合标识。

3. 如果有多个 {{site.data.keyword.Bluemix_notm}} 帐户，请选择要在其中创建 Kubernetes 集群的帐户。

4.  如果要在先前选择的 {{site.data.keyword.Bluemix_notm}} 区域以外的区域中创建或访问 Kubernetes 集群，请运行 `bx cs region-set`。

6.  创建集群。

    1.  **标准集群**：查看可用的位置。显示的位置取决于您登录到的 {{site.data.keyword.containershort_notm}} 区域。

        ```
                bx cs locations
        ```
        {: pre}
        
        CLI 输出与 [{{site.data.keyword.containerlong}} 区域的位置](cs_regions.html#locations)相匹配。
        
    2.  **标准集群**：选择位置并查看该位置中可用的机器类型。机器类型指定可供每个工作程序节点使用的虚拟或物理计算主机。

        -  查看**服务器类型**字段，以选择虚拟或物理（裸机）机器。
        -  **虚拟**：虚拟机按小时计费，在共享或专用硬件上供应。
        -  **物理**：裸机服务器按月计费，通过与 IBM Cloud Infrastructure (SoftLayer) 进行手动交互来供应，可能需要一个工作日以上的时间才能完成。裸机最适用于需要更多资源和主机控制的高性能应用程序。
        - **具有可信计算的物理机器**：对于运行 Kubernetes V1.9 或更高版本的裸机集群，还可以选择启用[可信计算](cs_secure.html#trusted_compute)来验证裸机工作程序节点是否被篡改。可信计算可用于精选的裸机机器类型。例如，`mgXc` GPU 类型模板不支持可信计算。如果在创建集群期间未启用信任，但希望日后启用，那么可以使用 `bx cs feature-enable` [命令](cs_cli_reference.html#cs_cluster_feature_enable)。启用信任后，日后无法将其禁用。
        -  **机器类型**：要确定需要部署的机器类型，请查看核心、内存和存储器组合，或者查阅 `bx cs machine-types` [命令文档](cs_cli_reference.html#cs_machine_types)。创建集群后，可以使用 `bx cs worker-add` [命令](cs_cli_reference.html#cs_worker_add)来添加其他物理或虚拟机类型。

           确保要供应裸机机器。因为裸机机器是按月计费的，所以如果在错误下单后立即将其取消，也仍然会按整月向您收费。
        {:tip}

        ```
                bx cs machine-types <location>
        ```
        {: pre}

    3.  **标准集群**：检查以确定 IBM Cloud Infrastructure (SoftLayer) 中是否已存在此帐户的公用和专用 VLAN。

        ```
                bx cs vlans <location>
        ```
        {: pre}

        ```
                ID        Name                Number   Type      Router  
        1519999   vlan   1355     private   bcr02a.dal10  
        1519898   vlan   1357     private   bcr02a.dal10 
        1518787   vlan   1252     public   fcr02a.dal10 
        1518888   vlan   1254     public    fcr02a.dal10 
        ```
        {: screen}

        如果公用和专用 VLAN 已经存在，请记下匹配的路由器。专用 VLAN 路由器始终以 <code>bcr</code>（后端路由器）开头，而公用 VLAN 路由器始终以 <code>fcr</code>（前端路由器）开头。创建集群并指定公用和专用 VLAN 时，在这些前缀之后的数字和字母组合必须匹配。在示例输出中，任一专用 VLAN 都可以与任一公用 VLAN 一起使用，因为路由器全都包含 `02a.dal10`。

        必须将工作程序节点连接到专用 VLAN，还可以选择将工作程序节点连接到公用 VLAN。**注**：如果工作程序节点设置为仅使用专用 VLAN，那么必须为网络连接配置备用解决方案。有关更多信息，请参阅[工作程序节点的 VLAN 连接](cs_clusters.html#worker_vlan_connection)。

    4.  **免费和标准集群**：运行 `cluster-create` 命令。您可以选择免费集群（包含设置有 2 个 vCPU 和 4 GB 内存的一个工作程序节点），在 21 天后会自动删除该集群。创建标准集群时，缺省情况下会对工作程序节点磁盘进行加密，其硬件由多个 IBM 客户共享，并且会按使用小时数对其进行计费。</br>标准集群的示例。指定集群的选项：

        ```
        bx cs cluster-create --location dal10 --machine-type u2c.2x4 --hardware <shared_or_dedicated> --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID> --workers 3 --name <cluster_name> --kube-version <major.minor.patch> [--disable-disk-encrypt][--trusted]
        ```
        {: pre}

        免费集群的示例。指定集群名称：

        ```
                bx cs cluster-create --name my_cluster
        ```
        {: pre}

        <table>
        <caption>cluster-create 组成部分</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解此命令的组成部分</th>
        </thead>
        <tbody>
        <tr>
        <td><code>cluster-create</code></td>
        <td>此命令在 {{site.data.keyword.Bluemix_notm}} 组织中创建集群。</td>
        </tr>
        <tr>
        <td><code>--location <em>&lt;location&gt;</em></code></td>
        <td>**标准集群**：将 <em>&lt;location&gt;</em> 替换为要在其中创建集群的 {{site.data.keyword.Bluemix_notm}} 位置标识。[可用位置](cs_regions.html#locations)取决于您登录到的 {{site.data.keyword.containershort_notm}} 区域。</td>
        </tr>
        <tr>
        <td><code>--machine-type <em>&lt;machine_type&gt;</em></code></td>
        <td>**标准集群**：选择机器类型。可以将工作程序节点作为虚拟机部署在共享或专用硬件上，也可以作为物理机器部署在裸机上。可用的物理和虚拟机类型随集群的部署位置而变化。有关更多信息，请参阅 `bx cs machine-type` [命令](cs_cli_reference.html#cs_machine_types)的文档。对于免费集群，无需定义机器类型。</td>
        </tr>
        <tr>
        <td><code>--hardware <em>&lt;shared_or_dedicated&gt;</em></code></td>
        <td>**标准集群（仅虚拟）**：工作程序节点的硬件隔离级别。如果希望可用的物理资源仅供您专用，请使用 dedicated，或者要允许物理资源与其他 IBM 客户共享，请使用 shared。缺省值为 shared。此值对于标准集群是可选的，且不可用于免费集群。</td>
        </tr>
        <tr>
        <td><code>--public-vlan <em>&lt;public_vlan_id&gt;</em></code></td>
        <td><ul>
          <li>**免费集群**：无需定义公用 VLAN。免费集群会自动连接到 IBM 拥有的公用 VLAN。</li>
          <li>**标准集群**：如果已经在 IBM Cloud Infrastructure (SoftLayer) 帐户中为该位置设置了公用 VLAN，请输入该公用 VLAN 的标识。如果要将工作程序节点仅连接到专用 VLAN，请不要指定此选项。**注**：如果工作程序节点设置为仅使用专用 VLAN，那么必须为网络连接配置备用解决方案。有关更多信息，请参阅[工作程序节点的 VLAN 连接](cs_clusters.html#worker_vlan_connection)。<br/><br/>
          <strong>注</strong>：专用 VLAN 路由器始终以 <code>bcr</code>（后端路由器）开头，而公用 VLAN 路由器始终以 <code>fcr</code>（前端路由器）开头。创建集群并指定公用和专用 VLAN 时，在这些前缀之后的数字和字母组合必须匹配。</li>
        </ul></td>
        </tr>
        <tr>
        <td><code>--private-vlan <em>&lt;private_vlan_id&gt;</em></code></td>
        <td><ul><li>**免费集群**：无需定义专用 VLAN。免费集群会自动连接到 IBM 拥有的专用 VLAN。</li><li>**标准集群**：如果已经在 IBM Cloud Infrastructure (SoftLayer) 帐户中为该位置设置了专用 VLAN，请输入该专用 VLAN 的标识。如果帐户中没有专用 VLAN，请不要指定此选项。{{site.data.keyword.containershort_notm}} 会自动为您创建专用 VLAN。<br/><br/><strong>注</strong>：专用 VLAN 路由器始终以 <code>bcr</code>（后端路由器）开头，而公用 VLAN 路由器始终以 <code>fcr</code>（前端路由器）开头。创建集群并指定公用和专用 VLAN 时，在这些前缀之后的数字和字母组合必须匹配。</li></ul></td>
        </tr>
        <tr>
        <td><code>--name <em>&lt;name&gt;</em></code></td>
        <td>**免费和标准集群**：将 <em>&lt;name&gt;</em> 替换为集群的名称。名称必须以字母开头，可以包含字母、数字和连字符 (-)，并且不能超过 35 个字符。集群名称和部署集群的区域构成了 Ingress 子域的标准域名。为了确保 Ingress 子域在区域内是唯一的，可能会截断 Ingress 域名中的集群名称并附加随机值。
</td>
        </tr>
        <tr>
        <td><code>--workers <em>&lt;number&gt;</em></code></td>
        <td>**标准集群**：要包含在集群中的工作程序节点数。如果未指定 <code>--workers</code> 选项，那么会创建 1 个工作程序节点。</td>
        </tr>
        <tr>
          <td><code>--kube-version <em>&lt;major.minor.patch&gt;</em></code></td>
          <td>**标准集群**：集群主节点的 Kubernetes 版本。此值是可选的。未指定版本时，会使用受支持 Kubernetes 版本的缺省值来创建集群。要查看可用版本，请运行 <code>bx cs kube-versions</code>。
</td>
        </tr>
        <tr>
        <td><code>--disable-disk-encrypt</code></td>
        <td>**免费和标准集群**：工作程序节点缺省情况下具有磁盘加密功能；[了解更多信息](cs_secure.html#encrypted_disks)。如果要禁用加密，请包括此选项。</td>
        </tr>
        <tr>
        <td><code>--trusted</code></td>
        <td>**标准裸机集群**：启用[可信计算](cs_secure.html#trusted_compute)以验证裸机工作程序节点是否被篡改。可信计算可用于精选的裸机机器类型。例如，`mgXc` GPU 类型模板不支持可信计算。如果在创建集群期间未启用信任，但希望日后启用，那么可以使用 `bx cs feature-enable` [命令](cs_cli_reference.html#cs_cluster_feature_enable)。启用信任后，日后无法将其禁用。</td>
        </tr>
        </tbody></table>

7.  验证是否请求了创建集群。

    ```
            bx cs clusters
        ```
    {: pre}

    **注**：对于虚拟机，可能需要若干分钟才能订购好工作程序节点机器，并且在帐户中设置并供应集群。裸机物理机器通过与 IBM Cloud Infrastructure (SoftLayer) 进行手动交互来供应，可能需要一个工作日以上的时间才能完成。

    当完成集群供应时，集群的状态会更改为**已部署**。

    ```
    Name         ID                                   State      Created          Workers   Location   Version
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1         mil01      1.9.7
    ```
    {: screen}

8.  检查工作程序节点的状态。

    ```
           bx cs workers <cluster_name_or_ID>
       ```
    {: pre}

    当工作程序节点已准备就绪时，状态会更改为 **normal**，而阶段状态为 **Ready**。节点阶段状态为 **Ready** 时，可以访问集群。

    **注**：为每个工作程序节点分配了唯一的工作程序节点标识和域名，在创建集群后，不得手动更改该标识和域名。更改标识或域名会阻止 Kubernetes 主节点管理集群。

    ```
    ID                                                 Public IP       Private IP      Machine Type   State    Status   Location   Version
    kube-mil01-paf97e8843e29941b49c598f516de72101-w1   169.xx.xxx.xxx  10.xxx.xx.xxx   free           normal   Ready    mil01      1.9.7
    ```
    {: screen}

9. 将所创建的集群设置为此会话的上下文。每次使用集群时都完成这些配置步骤。
    1.  获取命令以设置环境变量并下载 Kubernetes 配置文件。

        ```
                bx cs cluster-config <cluster_name_or_ID>
        ```
        {: pre}

        配置文件下载完成后，会显示一个命令，您可以使用该命令将本地 Kubernetes 配置文件的路径设置为环境变量。

        OS X 的示例：

        ```
                export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

    2.  复制并粘贴终端中显示的命令，以设置 `KUBECONFIG` 环境变量。
    3.  验证是否已正确设置 `KUBECONFIG` 环境变量。

        OS X 的示例：

        ```
                echo $KUBECONFIG
        ```
        {: pre}

        输出：

        ```
                /Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml

        

        ```
        {: screen}

10. 使用缺省端口 `8001` 启动 Kubernetes 仪表板。
    1.  使用缺省端口号设置代理。

        ```
                kubectl proxy
        ```
        {: pre}

        ```
                Starting to serve on 127.0.0.1:8001
        ```
        {: screen}

    2.  在 Web 浏览器中打开以下 URL 以查看 Kubernetes 仪表板。

        ```
                http://localhost:8001/ui
        ```
        {: codeblock}


**接下来要做什么？**


-   [在集群中部署应用程序。](cs_app.html#app_cli)
-   [使用 `kubectl` 命令行管理集群。![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/reference/kubectl/overview/)
-   [在 {{site.data.keyword.Bluemix_notm}} 中设置自己的专用注册表，以存储 Docker 映像并与其他用户共享这些映像。](/docs/services/Registry/index.html)
- 如果有多个 VLAN 用于一个集群，或者在同一 VLAN 上有多个子网，那么必须[开启 VLAN 生成](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning)，以便工作程序节点可以在专用网络上相互通信。
- 如果您有防火墙，那么可能需要[打开必要的端口](cs_firewall.html#firewall)才能使用 `bx`、`kubectl` 或 `calicotl` 命令，以允许来自集群的出站流量，或允许联网服务的入站流量。

<br />





## 查看集群状态
{: #states}

查看 Kubernetes 集群的状态，以获取有关集群可用性和容量的信息以及可能已发生的潜在问题。
{:shortdesc}

要查看有关特定集群的信息（例如，其位置、主节点 URL、Ingress 子域、版本、所有者和监视仪表板），请使用 `bx cs cluster-get <cluster_name_or_ID>` [命令](cs_cli_reference.html#cs_cluster_get)。包含 `--showResources` 标志可查看更多集群资源，例如存储 pod 的附加组件或公共和专用 IP 的子网 VLAN。

您可以通过运行 `bx cs clusters` 命令并找到**状态**字段，查看当前集群状态。要对集群和工作程序节点进行故障诊断，请参阅[集群故障诊断](cs_troubleshoot.html#debug_clusters)。

<table summary="每个表行都应从左到右阅读，其中第一列是集群状态，第二列是描述。">
  <caption>集群状态</caption>
   <thead>
   <th>集群状态</th>
   <th>描述</th>
   </thead>
   <tbody>
<tr>
   <td>Aborted</td>
   <td>在部署 Kubernetes 主节点之前，用户请求删除集群。在集群删除完成后，将从仪表板中除去集群。如果集群长时间卡在此状态，请开具 [{{site.data.keyword.Bluemix_notm}} 支持凭单](cs_troubleshoot.html#ts_getting_help)。</td>
   </tr>
 <tr>
     <td>Critical</td>
     <td>无法访问 Kubernetes 主节点，或者集群中的所有工作程序节点都已停止运行。</td>
    </tr>
   <tr>
     <td>Delete failed</td>
     <td>Kubernetes 主节点或至少一个工作程序节点无法删除。</td>
   </tr>
   <tr>
     <td>Deleted</td>
     <td>集群已删除，但尚未从仪表板中除去。如果集群长时间卡在此状态，请开具 [{{site.data.keyword.Bluemix_notm}} 支持凭单](cs_troubleshoot.html#ts_getting_help)。</td>
   </tr>
   <tr>
   <td>Deleting</td>
   <td>正在删除集群，并且正在拆除集群基础架构。无法访问集群。</td>
   </tr>
   <tr>
     <td>Deploy failed</td>
     <td>无法完成 Kubernetes 主节点的部署。您无法解决此状态。请通过开具 [{{site.data.keyword.Bluemix_notm}} 支持凭单](cs_troubleshoot.html#ts_getting_help)来联系 IBM Cloud 支持。</td>
   </tr>
     <tr>
       <td>Deploying</td>
       <td>Kubernetes 主节点尚未完全部署。无法访问集群。请等待集群完全部署后，再复查集群的运行状况。</td>
      </tr>
      <tr>
       <td>Normal</td>
       <td>集群中的所有工作程序节点都已启动并正在运行。您可以访问集群，并将应用程序部署到集群。此状态视为正常运行，不需要您执行操作。**注**：虽然工作程序节点可能是正常的，但其他基础架构资源（例如，[联网](cs_troubleshoot_network.html)和[存储](cs_troubleshoot_storage.html)）可能仍然需要注意。</td>
    </tr>
      <tr>
       <td>Pending</td>
       <td>Kubernetes 主节点已部署。正在供应工作程序节点，这些节点在集群中尚不可用。您可以访问集群，但无法将应用程序部署到集群。</td>
     </tr>
   <tr>
     <td>Requested</td>
     <td>发送了用于创建集群并为 Kubernetes 主节点和工作程序节点订购基础架构的请求。集群部署启动后，集群状态将更改为 <code>Deploying</code>。如果集群长时间卡在 <code>Requested</code> 状态，请开具 [{{site.data.keyword.Bluemix_notm}} 支持凭单](cs_troubleshoot.html#ts_getting_help)。</td>
   </tr>
   <tr>
     <td>Updating</td>
     <td>在 Kubernetes 主节点中运行的 Kubernetes API 服务器正在更新到新的 Kubernetes API 版本。在更新期间，您无法访问或更改集群。用户已部署的工作程序节点、应用程序和资源不会被修改，并且将继续运行。等待更新完成后，再复查集群的运行状况。</td>
   </tr>
    <tr>
       <td>Warning</td>
       <td>集群中至少有一个工作程序节点不可用，但其他工作程序节点可用，并且可以接管工作负载。</td>
    </tr>
   </tbody>
 </table>


<br />


## 除去集群
{: #remove}

对于使用现买现付帐户创建的免费和标准集群，不再需要这些集群时，必须手动将其除去，以便这些集群不再耗用资源。
{:shortdesc}

**警告：**
  - 不会为持久性存储器中的集群或数据创建备份。删除集群或持久性存储器是永久的，无法撤销。
  - 除去集群时，还会除去创建集群时自动供应的任何子网，以及使用 `bx cs cluster-subnet-create` 命令创建的任何子网。但是，如果是使用 `bx cs cluster-subnet-add 命令`以手动方式将现有子网添加到集群的，那么不会从 IBM Cloud Infrastructure (SoftLayer) 帐户中除去这些子网，并且您可以在其他集群中复用这些子网。

开始之前，请记下集群标识。您可能需要集群标识来调查和除去未随集群一起自动删除的相关 IBM Cloud Infrastructure (SoftLayer) 资源，例如持久性存储器。
{: tip}

要除去集群，请执行以下操作：

-   通过 {{site.data.keyword.Bluemix_notm}} GUI
    1.  选择集群，然后单击**更多操作...** 菜单中的**删除**。

-   通过 {{site.data.keyword.Bluemix_notm}} CLI
    1.  列出可用的集群。


        ```
                bx cs clusters
        ```
        {: pre}

    2.  删除集群。

        ```
                bx cs cluster-rm <cluster_name_or_ID>
        ```
        {: pre}

    3.  遵循提示并选择是否删除集群资源，包括容器、pod、绑定的服务、持久性存储器和私钥。
      - **持久性存储器**：持久性存储器为数据提供了高可用性。如果使用[现有文件共享](cs_storage.html#existing)创建了持久性卷申领，那么在删除集群时无法删除该文件共享。必须日后从 IBM Cloud infrastructure (SoftLayer) 产品服务组合中手动删除此文件共享。

          **注**：受每月计费周期的影响，无法在一个月的最后一天删除持久性卷申领。如果在一个月的最后一天删除持续性卷申领，那么删除操作会保持暂挂，直到下个月开始再执行。

后续步骤：
- 运行 `bx cs cluster` 命令时，已除去的集群不再列在可用集群列表中之后，您可以复用该集群的名称。
- 如果保留子网，那么可以[在新集群中复用子网](cs_subnets.html#custom)，也可以日后从 IBM Cloud Infrastructure (SoftLayer) 产品服务组合中手动删除这些子网。
- 如果保留了持久性存储器，那么日后可以通过 {{site.data.keyword.Bluemix_notm}} GUI 中的 IBM Cloud Infrastructure (SoftLayer) 仪表板来删除该存储器。
