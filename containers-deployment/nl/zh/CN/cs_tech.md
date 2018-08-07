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



# {{site.data.keyword.containerlong_notm}} 技术

了解有关 {{site.data.keyword.containerlong}} 的支持技术的更多信息。
{:shortdesc}

## Docker 容器
{: #docker_containers}

名为 Docker 的开放式源代码项目基于现有 Linux 容器技术 (LXC) 进行构建，已成为用于快速构建、测试、部署和扩展应用程序的软件平台。Docker 将软件打包成标准化单元（称为容器），其中包含应用程序运行所需的所有元素。
{:shortdesc}

了解一些基本 Docker 概念：

<dl>
<dt>映像</dt>
<dd>Docker 映像通过 Dockerfile 进行构建，此文本文件可定义如何构建映像以及要包含在其中的构建工件（例如，应用程序、应用程序配置及其依赖项）。映像始终从其他映像构建，从而使它们的配置更加快速。让其他人来做大部分的映像构建工作，您只需稍作调整即可使用。</dd>
<dt>注册表</dt>
<dd>映像注册表是用于存储、检索和共享 Docker 映像的位置。存储在注册表中的映像可以公共可用（公共注册表），也可以供一小组用户访问（专用注册表）。{{site.data.keyword.containershort_notm}} 提供了公共映像（如 ibmliberty），可用于创建第一个容器化应用程序。对于企业应用程序，请使用专用注册表（如在 {{site.data.keyword.Bluemix_notm}} 中提供的注册表）来保护映像不被未经授权的用户使用。</dd>
<dt>容器</dt>
<dd>每个容器都是通过映像创建的。容器是一个打包应用程序，其具有所有依赖项，以便可以在环境之间移动应用程序并在不进行更改的情况下运行。与虚拟机不同，容器不会对设备、其操作系统和底层硬件进行虚拟化。在容器中只打包应用程序代码、运行时、系统工具、库和设置。容器在 Ubuntu 计算主机上作为隔离进程运行，并共享主机操作系统及其硬件资源。这种方式使得容器比虚拟机更轻便、可移植性更高且更高效。</dd>
</dl>



### 使用容器的主要优点
{: #container_benefits}

<dl>
<dt>容器灵活</dt>
<dd>容器通过为开发和生产部署提供标准化环境，简化了系统管理。轻量级运行时支持快速向上扩展和向下扩展部署。通过使用容器帮助您快速可靠地在任何基础架构上部署和运行任何应用程序，以除去管理不同操作系统平台及其底层基础架构的复杂性。</dd>
<dt>容器小巧</dt>
<dd>在单个虚拟机所需的空间中，可以放入许多容器。</dd>
<dt>容器可移植</dt>
<dd>
<ul>
  <li>复用一些映像来构建容器。</li>
  <li>快速将应用程序代码从编译打包环境转移到生产环境。</li>
  <li>利用持续交付工具使流程自动化。</li>
  </ul>
  </dd>

<p>使用容器映像时，请了解有关[确保个人信息安全](cs_secure.html#pi)的更多信息。</p>

<p>准备好进一步了解 Docker 了吗？<a href="https://developer.ibm.com/courses/all/docker-essentials-extend-your-apps-with-containers/" target="_blank">通过完成本课程，了解如何一起使用 Docker 和 {{site.data.keyword.containershort_notm}}</a>。</p>

</dl>

<br />


## Kubernetes 集群
{: #kubernetes_basics}

<img src="images/certified-kubernetes-resized.png" style="padding-right: 10px;" align="left" alt="此角标指示 IBM Cloud Container Service 的 Kubernetes 证书。"/>名为 Kubernetes 的开放式源代码项目将运行容器化基础架构与生产工作负载、开放式源代码供应和 Docker 容器管理工具相结合。Kubernetes 基础架构提供了隔离、安全的应用程序平台，用于管理可移植、可扩展，并能在发生故障转移时自我复原的容器。
{:shortdesc}

了解一些基本的 Kubernetes 概念，如下图所示。

![部署设置](images/cs_app_tutorial_components1.png)

<dl>
<dt>帐户</dt>
<dd>帐户是指 {{site.data.keyword.Bluemix_notm}} 帐户。</dd>

<dt>集群</dt>
<dd>Kubernetes 集群由一个或多个计算主机组成，这些计算主机称为工作程序节点。工作程序节点由 Kubernetes 主节点管理，主节点用于集中控制和监视集群中的所有 Kubernetes 资源。因此，在部署容器化应用程序的资源时，Kubernetes 主节点会考虑部署需求和集群中的可用容量，然后决定将这些资源部署在哪个工作程序节点上。Kubernetes 资源包括服务、部署和 pod。</dd>

<dt>服务</dt>
<dd>服务是 Kubernetes 资源，可将一组 pod 分组在一起，并提供与这些 pod 的网络连接，而无需公开每个 pod 的实际专用 IP 地址。您可以通过服务使应用程序在集群或公共因特网中可用。</dd>

<dt>部署</dt>
<dd>部署是一种 Kubernetes 资源，在其中可指定运行应用程序所需的其他资源或功能的信息，例如服务、持久性存储器或注释。在配置 YAML 文件中记录部署，然后将其应用于集群。Kubernetes 主节点会配置资源，并将容器部署到具有可用容量的工作程序节点上的 pod 中。</br></br>
定义应用程序的更新策略，包括在滚动更新期间要添加的 pod 数，以及允许同时不可用的 pod 数。执行滚动更新时，部署将检查更新是否有效，并在检测到故障时停止应用。</dd>

<dt>Pod</dt>
<dd>部署到集群中的每个容器化应用程序都由称为 pod 的 Kubernetes 资源进行部署、运行和管理。pod 代表 Kubernetes 集群中的小型可部署单元，用于将必须视为单个单元的容器分组在一起。在大多数情况下，每个容器都会部署到其自己的 pod 中。但是，应用程序可能需要将一个容器和其他辅助容器部署到一个 pod 中，以便可以使用相同的专用 IP 地址寻址到这些容器。</dd>

<dt>应用程序</dt>
<dd>应用程序可能是指完整应用程序，也可能指应用程序组件。可在单独的 pod 或单独的工作程序节点中部署应用程序的组件。</dd>

<p>使用 Kubernetes 资源时，请了解有关[确保个人信息安全](cs_secure.html#pi)的更多信息。</p>

<p>准备好进一步了解 Kubernetes 了吗？</p>
<ul><li><a href="cs_tutorials.html#cs_cluster_tutorial" target="_blank">通过“创建集群”教程扩大对术语的了解</a>。</li>
<li><a href="https://developer.ibm.com/courses/all/get-started-kubernetes-ibm-cloud-container-service/" target="_blank">通过完成本课程，了解如何一起使用 Kubernetes 和 {{site.data.keyword.containershort_notm}}</a>。</li></ul>


</dl>

<br />


## 服务体系结构
{: #architecture}

在 {{site.data.keyword.containershort_notm}} 上运行的 Kubernetes 集群中，容器化应用程序在称为工作程序节点的计算主机上托管。更具体地说，应用程序在 pod 中运行，而 pod 在工作程序节点上托管。工作程序节点由 Kubernetes 主节点进行管理。Kubernetes 主节点和工作程序节点通过安全的 TLS 证书和 openVPN 连接相互通信，以编排集群配置。
{: shortdesc}

Kubernetes 主节点与工作程序节点之间有何区别？问得好。

<dl>
  <dt>Kubernetes 主节点</dt>
    <dd>Kubernetes 主节点负责的任务是管理集群中的所有计算资源、网络资源和存储资源。Kubernetes 主节点可确保将容器化应用程序和服务同等部署到集群中的工作程序节点。根据您配置应用程序和服务的方式，主节点会确定具有足够资源来满足应用程序需求的工作程序节点。</dd>
  <dt>工作程序节点</dt>
    <dd>每个工作程序节点都是物理机器（裸机）或是在云环境中的物理硬件上运行的虚拟机。供应工作程序节点时，您将确定可供该工作程序节点上托管的容器使用的资源。工作程序节点现成设置有 {{site.data.keyword.IBM_notm}} 管理的 Docker Engine 以及独立的计算资源、联网和卷服务。内置安全性功能提供了隔离、资源管理功能和工作程序节点安全合规性。</dd>
</dl>

<p>
<figure>
 <img src="images/cs_org_ov.png" alt="{{site.data.keyword.containerlong_notm}} Kubernetes 体系结构">
 <figcaption>{{site.data.keyword.containershort_notm}} 体系结构</figcaption>
</figure>
</p>

想要了解如何将 {{site.data.keyword.containerlong_notm}} 与其他产品和服务配合使用？请查看一些[集成](cs_integrations.html#integrations)。


<br />

