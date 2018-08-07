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



# 使用 {{site.data.keyword.containerlong_notm}} 的责任
了解使用 {{site.data.keyword.containerlong}} 时，您的集群管理责任以及相关条款和条件。
{:shortdesc}

## 集群管理责任
{: #responsibilities}

查看您与 IBM 共享的用于管理集群的责任。
{:shortdesc}

**IBM 负责：**

- 在集群中部署主节点、工作程序节点和管理组件，例如在集群创建时部署 Ingress 应用程序负载均衡器
- 管理集群的 Kubernetes 主节点的安全性更新、监视、隔离和恢复
- 监视工作程序节点的运行状况并为这些工作程序节点的更新和恢复提供自动化
- 对基础架构帐户执行自动化任务，包括添加工作程序节点、除去工作程序节点以及创建缺省子网
- 管理、更新和恢复集群内的操作组件，例如 Ingress 应用程序负载均衡器和存储插件
- 在持久性卷申领请求时供应存储卷
- 在所有工作程序节点上提供安全性设置

</br>

**您负责：**

- [配置 {{site.data.keyword.Bluemix_notm}} 帐户以访问 IBM Cloud Infrastructure (SoftLayer) 产品服务组合](cs_troubleshoot_clusters.html#cs_credentials)
- [在集群中部署和管理 Kubernetes 资源，例如，Pod、服务和部署](cs_app.html#app_cli)
- [利用服务和 Kubernetes 的功能以确保应用程序的高可用性](cs_app.html#highly_available_apps)
- [通过使用 CLI 添加或除去工作程序节点来添加或除去集群容量](cs_cli_reference.html#cs_worker_add)
- [在 IBM Cloud infrastructure (SoftLayer) 中创建公用和专用 VLAN 以针对集群进行网络隔离](/docs/infrastructure/vlans/getting-started.html#getting-started-with-vlans)
- [确保所有工作程序节点都具有到 Kibernetes 主节点 URL 的网络连接](cs_firewall.html#firewall)<p>**注**：如果工作程序节点同时具有公用和专用 VLAN，那么已配置网络连接。如果工作程序节点设置为仅使用专用 VLAN，那么必须为网络连接配置备用解决方案。有关更多信息，请参阅[工作程序节点的 VLAN 连接](cs_clusters.html#worker_vlan_connection)。</p>
- [Kubernetes 版本更新可用时更新主 kube-apiserver](cs_cluster_update.html#master)
- [使主版本、次版本和补丁版本上的工作程序节点保持最新](cs_cluster_update.html#worker_node)
- [通过运行 `kubectl` 命令（如 `cordon` 或 `drain`）以及运行 `bx cs` 命令（如 `reboot`、`reload` 或 `delete`](cs_cli_reference.html#cs_worker_reboot)）来恢复有故障的工作程序节点。
- [根据需要在 IBM Cloud infrastructure (SoftLayer) 中添加或除去子网](cs_subnets.html#subnets)
- [在 IBM Cloud infrastructure (SoftLayer) 中备份和复原持久性存储器中的数据 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](../services/RegistryImages/ibm-backup-restore/index.html)
- [使用自动恢复为工作程序节点配置运行状况监视](cs_health.html#autorecovery)

<br />


## 滥用 {{site.data.keyword.containerlong_notm}}
{: #terms}

客户机不能滥用 {{site.data.keyword.containershort_notm}}。
{:shortdesc}

滥用包括：

*   任何非法活动
*   分发或执行恶意软件
*   损害 {{site.data.keyword.containershort_notm}} 或干扰任何人使用 {{site.data.keyword.containershort_notm}}
*   损害或干扰任何人使用任何其他服务或系统
*   对任何服务或系统进行未经授权的访问
*   对任何服务或系统进行未经授权的修改
*   侵犯他人的权利


请参阅 [Cloud Services 条款](https://console.bluemix.net/docs/overview/terms-of-use/notices.html#terms)，以获取总体使用条款。
