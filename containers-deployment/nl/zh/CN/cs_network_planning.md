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



# 使用 NodePort、LoadBalancer 或 Ingress 服务规划联网
{: #planning}

在 {{site.data.keyword.containerlong}} 中创建 Kubernetes 集群时，每个集群都必须连接到一个公共 VLAN。公共 VLAN 用于确定在集群创建期间分配给工作程序节点的公共 IP 地址。
{:shortdesc}

免费集群和标准集群中的工作程序节点的公用网络接口受 Calico 网络策略保护。缺省情况下，这些策略将阻止大多数入站流量。但是，允许 Kubernetes 正常运行所需的入站流量，正如与 NodePort、LoadBalancer 和 Ingress 服务的连接一样。有关这些策略的更多信息（包括如何修改这些策略），请参阅[网络策略](cs_network_policy.html#network_policies)。

|集群类型|集群的公共 VLAN 的管理方|
|------------|------------------------------------------|
|免费集群|{{site.data.keyword.IBM_notm}}|
|标准集群|您通过您的 IBM Cloud infrastructure (SoftLayer) 帐户|
{: caption="按集群类型列出的公用 VLAN 的管理器" caption-side="top"}

有关工作程序节点与 pod 之间的集群内网络通信的更多信息，请参阅[集群内联网](cs_secure.html#in_cluster_network)。有关将 Kubernetes 集群中运行的应用程序安全连接到内部部署网络或连接到集群外部的应用程序的更多信息，请参阅[设置 VPN 连接](cs_vpn.html)。

## 允许对应用程序进行公共访问
{: #public_access}

要使应用程序在因特网上公开可用，必须先更新配置文件，然后再将应用程序部署到集群中。
{:shortdesc}

*{{site.data.keyword.containershort_notm}} 中的 Kubernetes 数据平面*

![{{site.data.keyword.containerlong_notm}} Kubernetes 体系结构](images/networking.png)

此图显示了 {{site.data.keyword.containershort_notm}} 中 Kubernetes 如何携带用户网络流量。要使应用程序可从因特网进行访问，具体方法根据您创建的是免费集群还是标准集群而有所不同。

<dl>
<dt><a href="cs_nodeport.html#planning" target="_blank">NodePort 服务</a>（免费和标准集群）</dt>
<dd>
 <ul>
  <li>在每个工作程序节点上公开一个公共端口，并使用任一工作程序节点的公共 IP 地址来公共访问集群中的服务。</li>
  <li>Iptables 是一个 Linux 内核功能，用于在应用程序的 pod 之间对请求进行负载均衡，提供高性能的网络路由，并提供网络访问控制。</li>
  <li>工作程序节点的公共 IP 地址不是永久固定的。除去或重新创建工作程序节点时，将为该工作程序节点分配新的公共 IP 地址。</li>
  <li>NodePort 服务适合于测试公共访问。如果您只需要短时间的公共访问，那么也可以使用此服务。</li>
 </ul>
</dd>
<dt><a href="cs_loadbalancer.html#planning" target="_blank">LoadBalancer 服务</a>（仅限标准集群）</dt>
<dd>
 <ul>
  <li>每个标准集群供应有四个可移植的公共 IP 地址和四个可移植的专用 IP 地址，这些 IP 地址可以用于为应用程序创建外部 TCP/UDP 负载均衡器。</li>
  <li>Iptables 是一个 Linux 内核功能，用于在应用程序的 pod 之间对请求进行负载均衡，提供高性能的网络路由，并提供网络访问控制。</li>
  <li>分配给负载均衡器的可移植公共 IP 地址是永久固定的，在集群中重新创建工作程序节点时不会更改。</li>
  <li>您可以通过公开应用程序需要的任何端口来定制负载均衡器。</li></ul>
</dd>
<dt><a href="cs_ingress.html#planning" target="_blank">Ingress</a>（仅限标准集群）</dt>
<dd>
 <ul>
  <li>通过创建一个外部 HTTP 或 HTTPS、TCP 或 UDP 负载均衡器，公开集群中的多个应用程序。负载均衡器使用安全的唯一公共入口点将入局请求路由到应用程序。</li>
  <li>您可以使用一个公用路径，将集群中的多个应用程序显示为服务。</li>
  <li>Ingress 由两个组件组成：
   <ul>
    <li>Ingress 资源用于定义如何对应用程序的入局请求进行路由和负载均衡的规则。</li>
    <li>应用程序负载均衡器 (ALB) 侦听入局 HTTP 或 HTTPS、TCP 或 UDP 服务请求。ALB 根据在 Ingress 资源中定义的规则，在各个应用程序 pod 之间转发请求。</li>
   </ul>
  <li>如果要通过定制路由规则来实施自己的 ALB，并需要对应用程序进行 SSL 终止，可使用 Ingress。</li>
 </ul>
</dd></dl>

要为应用程序选择最佳联网选项，可以遵循以下决策树。有关规划信息和配置指示信息，请单击所选择的联网服务选项。

<img usemap="#networking_map" border="0" class="image" src="images/networkingdt.png" width="500px" alt="此图像指导您选择应用程序的最佳联网选项。如果此图像未显示，仍可在文档这找到此信息。" style="width:500px;" />
<map name="networking_map" id="networking_map">
<area href="/docs/containers/cs_nodeport.html" alt="Nodeport 服务" shape="circle" coords="52, 283, 45"/>
<area href="/docs/containers/cs_loadbalancer.html" alt="LoadBalancer 服务" shape="circle" coords="247, 419, 44"/>
<area href="/docs/containers/cs_ingress.html" alt="Ingress 服务" shape="circle" coords="445, 420, 45"/>
</map>
