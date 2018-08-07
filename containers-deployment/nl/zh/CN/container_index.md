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



# {{site.data.keyword.containerlong_notm}} 入门
{: #container_index}

通过在 Kubernetes 集群中运行的 Docker 容器中部署高可用性应用程序，可迅速开始使用 {{site.data.keyword.containerlong}}。
{:shortdesc}

容器是打包应用程序及其所有依赖项的标准方法，以便您可以无缝地在环境之间移动应用程序。与虚拟机不同，容器不会捆绑操作系统。在容器中只会打包应用程序代码、运行时、系统工具、库和设置。因此容器比虚拟机更轻便、可移植性更高且更高效。



单击以下选项以开始：

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/cs_public_dedicated_options.png" width="440" alt="单击图标以快速开始使用 {{site.data.keyword.containershort_notm}}。使用 {{site.data.keyword.Bluemix_dedicated_notm}}，单击此图标以查看选项。" style="width:440px;" />
<map name="home_map" id="home_map">
<area href="#clusters" alt="在 {{site.data.keyword.Bluemix_notm}} 中开始使用 Kubernetes 集群" title="在 {{site.data.keyword.Bluemix_notm}} 中开始使用 Kubernetes 集群" shape="rect" coords="-7, -8, 108, 211" />
<area href="cs_cli_install.html" alt="安装 CLI。" title="安装 CLI。" shape="rect" coords="155, -1, 289, 210" />
<area href="cs_dedicated.html#dedicated_environment" alt="{{site.data.keyword.Bluemix_dedicated_notm}} 云环境" title="{{site.data.keyword.Bluemix_notm}} 云环境" shape="rect" coords="326, -10, 448, 218" />
</map>


## 集群入门
{: #clusters}

想在容器中部署应用程序？稍等一下！请首先创建 Kubernetes 集群。Kubernetes 是一种用于容器的编排工具。使用 Kubernetes，开发者可以通过利用集群的强大功能和灵活性，快速部署高可用性应用程序。
{:shortdesc}

那么什么是集群呢？集群是一组资源、工作程序节点、网络和存储设备，用于使应用程序保持高可用性。拥有集群后，可以将应用程序部署到容器中。

**开始之前**

您必须具有试用、现买现付或预订 [{{site.data.keyword.Bluemix_notm}} 帐户](https://console.bluemix.net/registration/)。

通过试用帐户，您可以创建一个免费集群并可以使用 21 天，以便熟悉该服务。通过现买现付或预订帐户，您仍可以创建免费试用的集群，但也可以供应 IBM Cloud Infrastructure (SoftLayer) 资源以在标准集群中使用。
{:tip}

要创建免费集群，请执行以下操作：

1.  在 [{{site.data.keyword.Bluemix_notm}} **目录** ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/catalog/?category=containers) 中，选择 **Kubernetes 集群中的容器**，然后单击**创建**。这将打开集群配置页面。缺省情况下，已选择**免费集群**。

2. 为集群提供唯一名称。

3.  单击**创建集群**。创建工作程序节点可能需要几分钟的时间进行供应，但您可以在**工作程序节点**选项卡中查看进度。阶段状态达到 `Ready` 时，即可以开始使用集群！

非常好！您已创建第一个 Kubernetes 集群。下面是有关免费集群的一些详细信息：

*   **机器类型**：免费集群有一个虚拟工作程序节点，其中有 2 个 CPU 和 4 GB 内存可供应用程序使用。创建标准集群时，可以选择物理（裸机）或虚拟机，以及各种机器大小。
*   **受管主节点**：工作程序节点由 {{site.data.keyword.IBM_notm}} 拥有的一个专用高可用性 Kubernetes 主节点进行集中监视和管理，该主节点控制和监视集群中的所有 Kubernetes 资源。此外，您可以专注于工作程序节点以及该工作程序节点中部署的应用程序，而不必担心如何管理此主节点。
*   **基础架构资源**：运行集群所需的资源（例如 VLAN 和 IP 地址）在 {{site.data.keyword.IBM_notm}} 拥有的 IBM Cloud Infrastructure (SoftLayer) 帐户中进行管理。创建标准集群时，可在您自己的 IBM Cloud infrastructure (SoftLayer) 帐户中管理这些资源。创建标准集群时，可以了解有关这些资源及[所需许可权](cs_users.html#infra_access)的更多信息。
*   **其他选项**：免费集群部署在您选择的区域内，但您无法选择部署在哪个位置（数据中心）。要对位置、联网和持久性存储器拥有控制权，请创建标准集群。[了解有关免费和标准集群优点的更多信息](cs_why.html#cluster_types)。


**接下来要做什么？**
在接下来的 21 天内，请使用免费集群试用一些功能。

* [安装 CLI 以开始使用集群。](cs_cli_install.html#cs_cli_install)
* [在集群中部署应用程序。](cs_app.html#app_cli)
* [创建具有多个节点的标准集群以获得更高可用性。](cs_clusters.html#clusters_ui)
* [在 {{site.data.keyword.Bluemix_notm}} 中设置专用注册表，以存储 Docker 映像并与其他用户共享这些映像。](/docs/services/Registry/index.html)
