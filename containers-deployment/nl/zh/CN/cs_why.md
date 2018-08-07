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



# 为什么使用 {{site.data.keyword.containerlong_notm}}
{: #cs_ov}

{{site.data.keyword.containerlong}} 通过组合 Docker 和 Kubernetes 技术、直观的用户体验以及内置安全性和隔离，提供功能强大的工具来自动对计算主机集群中的容器化应用程序进行部署、操作、扩展和监视。
{:shortdesc}


## 使用服务的优点
{: #benefits}

集群会部署在提供本机 Kubernetes 和特定于 {{site.data.keyword.IBM_notm}} 的功能的计算主机上。
{:shortdesc}

|优点|描述|
|-------|-----------|
|隔离了计算、网络和存储基础架构的单租户 Kubernetes 集群|<ul><li>创建自己的定制基础架构，以满足组织的需求。</li><li>使用 IBM Cloud infrastructure (SoftLayer) 提供的资源来供应专用而安全的 Kubernetes 主节点、工作程序节点、虚拟网络和存储器。</li><li>由 {{site.data.keyword.IBM_notm}} 持续监视和更新的完全受管 Kubernetes 主节点，使您的集群可用。</li><li>用于将工作程序节点作为具有可信计算的裸机服务器供应的选项。</li><li>存储持久数据，在 Kubernetes pod 之间共享数据，以及在需要时使用集成和安全卷服务复原数据。</li><li>受益于对所有本机 Kubernetes API 的完全支持。</li></ul>|
|使用漏洞顾问程序确保映像安全合规性|<ul><li>设置自己的安全 Docker 专用映像注册表，映像会存储在该注册表中，并供组织中的所有用户共享。</li><li>受益于自动扫描专用 {{site.data.keyword.Bluemix_notm}} 注册表中的映像。</li><li>查看特定于映像中所用操作系统的建议，以修复潜在漏洞。</li></ul>|
|持续监视集群运行状况|<ul><li>使用集群仪表板可快速查看和管理集群、工作程序节点和容器部署的运行状况。</li><li>使用 {{site.data.keyword.monitoringlong}}，找到详细的使用量度量值，并快速扩展集群以满足工作负载需求。</li><li>使用 {{site.data.keyword.loganalysislong}} 复查日志记录信息，以查看详细的集群活动。</li></ul>|
|安全地向公众公开应用程序|<ul><li>在公共 IP 地址、{{site.data.keyword.IBM_notm}} 提供的路径或自己的定制域之间进行选择，以通过因特网访问集群中的服务。</li></ul>|
|{{site.data.keyword.Bluemix_notm}} 服务集成|<ul><li>通过集成 {{site.data.keyword.Bluemix_notm}} 服务（例如，Watson API、Blockchain、数据服务或 Internet of Things）向应用程序添加额外的功能。</li></ul>|
{: caption="{{site.data.keyword.containerlong_notm}} 的优点" caption-side="top"}



<br />


## 比较产品及其组合
{: #differentiation}

可以在 {{site.data.keyword.Bluemix_notm}} Public 或 Dedicated 中、在 {{site.data.keyword.Bluemix_notm}} Private 中或者在混合设置中运行 {{site.data.keyword.containershort_notm}}。
{:shortdesc}


<table>
<caption>{{site.data.keyword.containershort_notm}} 设置之间的差异</caption>
<col width="22%">
<col width="78%">
 <thead>
 <th>{{site.data.keyword.containershort_notm}} 设置</th>
 <th>描述</th>
 </thead>
 <tbody>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Public
 </td>
 <td>使用位于[共享或专用硬件或者位于裸机机器上](cs_clusters.html#shared_dedicated_node)的 {{site.data.keyword.Bluemix_notm}} Public 时，可以使用 {{site.data.keyword.containershort_notm}} 在云上的集群中托管应用程序。{{site.data.keyword.Bluemix_notm}} Public 上的 {{site.data.keyword.containershort_notm}} 通过将 Docker 和 Kubernetes 技术、直观的用户体验以及内置安全和隔离功能组合使用，交付了功能强大的工具，可在计算主机的集群中对容器化应用程序自动执行部署、操作、扩展和监视。<br><br>有关更多信息，请参阅 [{{site.data.keyword.containershort_notm}} 技术](cs_tech.html)。
 </td>
 </tr>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Dedicated
 </td>
 <td>{{site.data.keyword.Bluemix_notm}} Dedicated 在云上提供的 {{site.data.keyword.containershort_notm}} 功能与 {{site.data.keyword.Bluemix_notm}} Public 相同。但是，使用 {{site.data.keyword.Bluemix_notm}} Dedicated 帐户，可用[物理资源仅供您的集群专用](cs_clusters.html#shared_dedicated_node)，而不会与其他 {{site.data.keyword.IBM_notm}} 客户的集群共享。需要对集群和使用的其他 {{site.data.keyword.Bluemix_notm}} 服务进行隔离时，您可能会选择设置 {{site.data.keyword.Bluemix_notm}} Dedicated 环境。<br><br>有关更多信息，请参阅[开始使用 {{site.data.keyword.Bluemix_notm}} Dedicated 中的集群](cs_dedicated.html#dedicated)。
 </td>
 </tr>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Private
 </td>
 <td>{{site.data.keyword.Bluemix_notm}} Private 是可以在自己的机器上本地安装的应用程序平台。如果需要在防火墙后您自己的受控环境中开发和管理内部部署容器化应用程序，那么可以选择在 {{site.data.keyword.Bluemix_notm}} Private 中使用 {{site.data.keyword.containershort_notm}}。<br><br>有关更多信息，请参阅 [{{site.data.keyword.Bluemix_notm}} Private 产品信息 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud/private) 和[文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_1.2.0/kc_welcome_containers.html)。
 </td>
 </tr>
 <tr>
 <td>混合设置
 </td>
 <td>混合是将 {{site.data.keyword.Bluemix_notm}} Public 或 Dedicated 中运行的服务与内部部署运行的其他服务（例如，{{site.data.keyword.Bluemix_notm}} Private 中的应用程序）组合使用。混合设置的示例：<ul><li>在 {{site.data.keyword.Bluemix_notm}} Public 中使用 {{site.data.keyword.containershort_notm}} 供应集群，但将该集群连接到内部部署数据库。</li><li>在 {{site.data.keyword.Bluemix_notm}} Private 中使用 {{site.data.keyword.containershort_notm}} 供应集群，并将应用程序部署到该集群中。但是，此应用程序可能会使用 {{site.data.keyword.Bluemix_notm}} Public 中的 {{site.data.keyword.ibmwatson}} 服务，例如 {{site.data.keyword.toneanalyzershort}}。</li></ul><br>要启用在 {{site.data.keyword.Bluemix_notm}} Public 或 Dedicated 中运行的服务与内部部署运行的服务之间的通信，必须[设置 VPN 连接](cs_vpn.html)。
 </td>
 </tr>
 </tbody>
</table>

<br />


## 比较免费和标准集群
{: #cluster_types}

您可以创建一个免费集群或创建任意数量的标准集群。试用免费集群以熟悉和测试一些 Kubernetes 功能，或者创建标准集群以使用完整的 Kubernetes 功能来部署应用程序。
{:shortdesc}

|特征|免费集群|标准集群|
|---------------|-------------|-----------------|
|[集群内联网](cs_secure.html#in_cluster_network)
|[基于 NFS 文件的持久存储器（带有卷）](cs_storage.html#planning)| |<img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|
|[LoadBalancer 服务对稳定 IP 地址的公用或专用网络应用程序访问权](cs_loadbalancer.html#planning)| |<img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|
|[Ingress 服务对稳定 IP 地址和可定制 URL 的公用网络应用程序访问权](cs_ingress.html#planning)| |<img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|
|[可移植公共 IP 地址](cs_subnets.html#manage)| |<img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|
|[日志记录和监视](cs_health.html#logging)| |<img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|
|[用于在物理（裸机服务器）服务器上供应工作程序节点的选项](cs_clusters.html#shared_dedicated_node)| |<img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|
|[用于供应具有可信计算的裸机工作程序的选项](cs_clusters.html#shared_dedicated_node)| |<img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|
|[在 {{site.data.keyword.Bluemix_dedicated_notm}} 中可用](cs_dedicated.html#dedicated_environment)| |<img src="images/confirm.svg" width="32" alt="功能可用" style="width:32px;" />|
{: caption="免费和标准集群的特征" caption-side="top"}

<br />




{: #responsibilities}
**注**：使用服务时，想查找您的责任和 {{site.data.keyword.containerlong}} 条款？

{: #terms}
请参阅 [{{site.data.keyword.containershort_notm}} 责任](cs_responsibilities.html)。
