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




# 保存集群中的数据
{: #storage}
您可以将数据持久存储在 {{site.data.keyword.containerlong}} 中，以在应用程序实例之间共享数据，并保护数据以免在 Kubernetes 集群中的某个组件发生故障时丢失。


## 规划高可用性存储
{: #planning}

在 {{site.data.keyword.containerlong_notm}} 中，可以从多个选项中进行选择，以存储应用程序数据并在集群中的各个 pod 之间共享数据。但是，对于集群中的组件或整个站点发生故障的情况，并非所有存储选项都提供相同级别的持久性和可用性。
{: shortdesc}


### 非持久性数据存储选项
{: #non_persistent}

如果数据不需要持久存储，或者数据无需在应用程序实例间共享，那么可以使用非持久性存储选项。非持久性存储选项还可用于对应用程序组件进行单元测试或试用新功能。
{: shortdesc}

下图显示 {{site.data.keyword.containerlong_notm}} 中可用的非持久性数据存储选项。这些选项可用于免费和标准集群。
<p>
<img src="images/cs_storage_nonpersistent.png" alt="非持久性数据存储选项" width="500" style="width: 500px; border-style: none"/></p>

<table summary="该表显示非持久性存储选项。每行从左到右阅读，其中第一列是选项编号，第二列是选项标题，第三列是描述。" style="width: 100%">
<caption>非持久性存储选项</caption>
  <thead>
  <th>选项</th>
  <th>描述</th>
  </thead>
  <tbody>
    <tr>
      <td>1. 在容器或 pod 内</td>
      <td>根据设计，容器和 pod 的生存时间短，并且可能会意外发生故障。但是，您可以将数据写入容器的本地文件系统，以存储容器整个生命周期内的数据。容器内的数据不能与其他容器或 pod 共享，并且在容器崩溃或被除去时会丢失。有关更多信息，请参阅[存储容器中的数据](https://docs.docker.com/storage/)。</td>
    </tr>
  <tr>
    <td>2. 在工作程序节点上
</td>
    <td>每个工作程序节点都设置有主存储器和辅助存储器，这由您为工作程序节点选择的机器类型确定。主存储器用于存储来自操作系统的数据，并且无法使用 [Kubernetes <code>hostPath</code> 卷 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/storage/volumes/#hostpath) 对其进行访问。辅助存储器用于存储来自 `kubelet` 和容器运行时引擎的数据。可以使用 [Kubernetes <code>emptyDir</code> 卷 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/storage/volumes/#emptydir) 来访问辅助存储器。<br/><br/>虽然 <code>hostPath</code> 卷用于将文件从工作程序节点文件系统安装到 pod，但 <code>emptyDir</code> 会创建一个空目录，此目录将分配给集群中的 pod。该 pod 中的所有容器可以对该卷执行读写操作。由于卷会分配给一个特定 pod，因此数据无法与副本集内的其他 pod 共享。
<br/><br/><p>对于以下情况，会除去 <code>hostPath</code> 或 <code>emptyDir</code> 卷及其数据：<ul><li>工作程序节点已删除。</li><li>工作程序节点已重新装入或更新。</li><li>集群已删除。</li><li>{{site.data.keyword.Bluemix_notm}} 帐户进入暂挂状态。</li></ul></p><p>此外，在以下情况下，也将除去 <code>emptyDir</code> 卷中的数据：<ul><li>从工作程序节点中永久删除分配的 pod。</li><li>在其他工作程序节点上安排了分配的 pod。</li></ul></p><p><strong>注</strong>：如果 pod 内的容器崩溃，该卷中的数据在工作程序节点上仍可用。</p></td>
    </tr>
    </tbody>
    </table>


### 用于高可用性的持久性数据存储选项
{: #persistent}

创建高可用性有状态应用程序时，主要困难是如何在多个位置的多个应用程序实例中持久存储数据，并使数据始终保持同步。对于高可用性数据，您希望确保有一个包含多个实例的主数据库，这些实例分布在多个数据中心甚或多个区域中。此主数据库必须被持续复制以保持单个事实源。集群中的所有实例都必须对此主数据库执行读写操作。如果主数据库的一个实例停止运行，其他实例会接管工作负载，避免您的应用程序发生停机时间。
{: shortdesc}

下图显示您在 {{site.data.keyword.containerlong_notm}} 中具有的可使数据在标准集群中实现高可用性的选项。适合您的选项取决于以下因素：
  * **拥有的应用程序的类型：**例如，您可能有一个应用程序必须基于文件存储数据，而不是在数据库内存储数据。
  * **数据存储和路由的法律要求：**例如，您可能只能在美国存储和路由数据，而不能使用位于欧洲的服务。
  * **备份和复原选项：**每个存储选项都随附数据备份和复原功能。检查可用的备份和复原选项是否满足灾难恢复计划的需求，例如备份频率或在主数据中心外部存储数据的功能。
  * **全局复制：**为了实现高可用性，您可能希望设置多个存储器实例以分布在全球各数据中心并进行复制。

<br/>
<img src="images/cs_storage_ha.png" alt="持久性存储的高可用性选项"/>

<table summary="此表显示持久性存储选项。各行都应从左到右阅读，其中第一列是选项编号，第二列是选项标题，第三列是描述。">
  <caption>持久性数据存储选项</caption>
  <thead>
  <th>选项</th>
  <th>描述</th>
  </thead>
  <tbody>
  <tr>
  <td>1. NFS 文件存储器或块存储器</td>
  <td>使用此选项时，可以利用 Kubernetes 持久性卷来持久存储应用程序和容器数据。卷在“耐久性”和“性能”[基于 NFS 的文件存储器 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud/file-storage/details) 或 [块存储器 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud/block-storage) 上进行托管，可供存储基于文件的数据的应用程序使用，也可以用作块，而不在数据库中使用。保存在文件和块存储器中的数据以静态方式加密。<p>{{site.data.keyword.containershort_notm}} 会提供预定义存储类，用于定义存储器的大小范围、IOPS、删除策略以及卷的读写许可权。要对文件存储器或块存储器发起请求，必须创建[持久性卷申领 (PVC)](cs_storage.html#create)。提交 PVC 后，{{site.data.keyword.containershort_notm}} 会以动态方式供应在基于 NFS 的文件存储器或块存储器上托管的持久性卷。可以[将 PVC 作为卷安装到部署](cs_storage.html#app_volume_mount)，以允许容器对该卷执行读写操作。</p><p>持久性卷在工作程序节点所在的数据中心进行供应。您可以跨同一副本集共享数据，也可以与同一集群中的其他部署共享数据。集群位于不同数据中心或区域中时，不能在集群间共享数据。</p><p>缺省情况下，不会自动备份 NFS 存储器和块存储器。您可以使用提供的[备份和复原机制](cs_storage.html#backup_restore)为集群设置定期备份。容器崩溃或从工作程序节点中除去 pod 时，数据不会除去，而是仍可由安装该卷的其他部署访问。</p><p><strong>注：</strong>您可以选择按每小时或每月对持久性 NFS 文件共享存储器和块存储器进行计费。如果选择“每月”，那么除去持久性存储器时，即使只用了很短的时间，您也仍需支付该持久性存储器一个月的费用。</p></td>
  </tr>
  <tr id="cloud-db-service">
    <td>2. Cloud 数据库服务</td>
    <td>使用此选项时，可以利用 {{site.data.keyword.Bluemix_notm}} 数据库云服务（例如 [IBM Cloudant NoSQL DB](/docs/services/Cloudant/getting-started.html#getting-started-with-cloudant)）来持久存储数据。通过此选项存储的数据可以跨集群、位置和区域进行访问。<p> 您可以选择配置单个数据库实例供所有应用程序访问，或[跨数据中心和复制设置多个实例](/docs/services/Cloudant/guides/active-active.html#configuring-cloudant-nosql-db-for-cross-region-disaster-recovery)以在实例之间实现更高可用性。在 IBM Cloudant NoSQL 数据库中，不会自动备份数据。您可以使用提供的[备份和复原机制](/docs/services/Cloudant/guides/backup-cookbook.html#cloudant-nosql-db-backup-and-recovery)来保护数据不受站点故障影响。</p> <p> 要在集群中使用服务，必须[绑定 {{site.data.keyword.Bluemix_notm}} 服务](cs_integrations.html#adding_app)到集群中的名称空间。将该服务绑定到集群时，将创建 Kubernetes 私钥。Kubernetes 私钥会保存有关该服务的保密信息，例如服务的 URL、用户名和密码。可以将私钥作为私钥卷安装到 pod，并使用该私钥中的凭证来访问该服务。通过将私钥卷安装到其他 pod，还可以在 pod 之间共享数据。容器崩溃或从工作程序节点中除去 pod 时，数据不会除去，而是仍可由安装该私钥卷的其他 pod 访问。<p>大多数 {{site.data.keyword.Bluemix_notm}} 数据库服务都免费对较小的数据量提供磁盘空间，因此您可以测试其功能。
</p></td>
  </tr>
  <tr>
    <td>3. 内部部署数据库</td>
    <td>如果由于法律原因必须在现场存储数据，请[设置 VPN 连接](cs_vpn.html#vpn)以连接到内部部署数据库，并使用数据中心内的现有存储、备份和复制机制。</td>
  </tr>
  </tbody>
  </table>

{: caption="表. 用于在 Kubernetes 集群中进行部署的持久数据存储选项" caption-side="top"}

<br />



## 在集群中使用 NFS 文件共享
{: #existing}

如果在 IBM Cloud Infrastructure (SoftLayer) 帐户中已经有要使用的现有 NFS 文件共享，那么可以通过为现有存储器创建持久性卷 (PV) 来使用这些 NFS 文件共享。
{:shortdesc}

持久性卷 (PV) 是一种 Kubernetes 资源，用于表示在数据中心内供应的实际存储设备。持久性卷会提取 {{site.data.keyword.Bluemix_notm}} Storage 如何供应特定存储类型的详细信息。要将 PV 安装到集群，必须通过创建持久性卷申领 (PVC) 来为 pod 请求持久性存储器。下图说明 PV 与 PVC 之间的关系。

![创建持久性卷和持久性卷申领](images/cs_cluster_pv_pvc.png)

如图所示，要启用现有 NFS 文件共享，必须创建具有特定大小和访问方式的 PV，并创建与 PV 规范相匹配的 PVC。如果 PV 和 PVC 相匹配，那么会将它们相互绑定。只有绑定的 PVC 才能由集群用户用于将相应卷安装到部署。此过程称为持久性存储器的静态供应。

开始之前，请确保您拥有可用于创建 PV 的现有 NFS 文件共享。例如，如果先前已[使用 `retain` 存储类策略创建 PVC](#create)，那么可以将现有 NFS 文件共享中的相应保留数据用于此新 PVC。

**注：**持久性存储器的静态供应仅适用于现有 NFS 文件共享。如果没有现有 NFS 文件共享，那么集群用户可以使用[动态供应](cs_storage.html#create)过程来添加 PV。

要创建 PV 和匹配的 PVC，请执行以下步骤。

1.  在 IBM Cloud Infrastructure (SoftLayer) 帐户中，查找要在其中创建 PV 对象的 NFS 文件共享的标识和路径。此外，将文件存储器授权给集群中的子网。此授权可授予您对存储器的集群访问权。
    1.  登录到 IBM Cloud infrastructure (SoftLayer) 帐户。
    2.  单击**存储器**。
    3.  单击**文件存储器**并从**操作**菜单中，选择**授权主机**。
    4.  选择**子网**。
    5.  从下拉列表中，选择工作程序节点连接到的专用 VLAN 子网。要查找工作程序节点的子网，请运行 `bx cs workers <cluster_name>`，并将工作程序节点的`专用 IP` 与下拉列表中找到的子网进行比较。
    6.  单击**提交**。
    6.  单击文件存储器的名称。
    7.  记下**安装点**字段。该字段显示为 `<server>:/<path>`.
2.  为 PV 创建存储器配置文件。在文件存储器**安装点**字段中包含服务器和路径。

    ```
    apiVersion: v1
    kind: PersistentVolume
    metadata:
     name: mypv
    spec:
     capacity:
       storage: "20Gi"
     accessModes:
       - ReadWriteMany
     nfs:
       server: "nfslon0410b-fz.service.networklayer.com"
       path: "/IBM01SEV8491247_0908/data01"
    ```
    {: codeblock}

    <table>
    <caption>了解 YAML 文件的组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td><code>name</code></td>
    <td>输入要创建的 PV 对象的名称。</td>
    </tr>
    <tr>
    <td><code>spec/capacity/storage</code></td>
    <td>输入现有 NFS 文件共享的存储器大小。输入的存储器大小必须以千兆字节为单位，例如 20Gi (20 GB) 或 1000Gi (1 TB)，并且大小必须与现有文件共享的大小相匹配。</td>
    </tr>
    <tr>
    <td><code>accessMode</code></td>
    <td>访问方式定义了 PVC 可以安装到工作程序节点的方式。<ul><li>ReadWriteOnce (RWO)：此 PV 只能安装到单个工作程序节点中的部署。安装到此 PV 的部署中的容器可以对该卷执行读写操作。</li><li>ReadOnlyMany (ROX)：此 PV 可以安装到在多个工作程序节点上托管的部署。安装到此 PV 的部署只能对该卷执行读操作。</li><li>ReadWriteMany (RWX)：此 PV 可以安装到在多个工作程序节点上托管的部署。安装到此 PV 的部署可以对该卷执行读写操作。</li></ul></td>
    </tr>
    <tr>
    <td><code>spec/nfs/server</code></td>
    <td>输入 NFS 文件共享服务器标识。</td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td>输入要在其中创建 PV 对象的 NFS 文件共享的路径。</td>
    </tr>
    </tbody></table>

3.  在集群中创建 PV 对象。

    ```
        kubectl apply -f deploy/kube-config/mypv.yaml
    ```
    {: pre}

4.  验证 PV 是否已创建。

    ```
        kubectl get pv
    ```
    {: pre}

5.  创建另一个配置文件以创建 PVC。为了使 PVC 与先前创建的 PV 对象相匹配，必须为 `storage` 和 `accessMode` 选择相同的值。`storage-class` 字段必须为空。如果其中任何字段与 PV 不匹配，那么会改为自动创建新的 PV。

    ```
        kind: PersistentVolumeClaim
    apiVersion: v1
    metadata:
     name: mypvc
     annotations:
       volume.beta.kubernetes.io/storage-class: ""
    spec:
     accessModes:
       - ReadWriteMany
     resources:
       requests:
         storage: "20Gi"
    ```
    {: codeblock}

6.  创建 PVC。

    ```
        kubectl apply -f deploy/kube-config/mypvc.yaml
    ```
    {: pre}

7.  验证 PVC 是否已创建并与 PV 对象绑定。此过程可能需要几分钟时间。

    ```
        kubectl describe pvc mypvc
    ```
    {: pre}

    输出示例：

    ```
        Name: mypvc
    Namespace: default
    StorageClass: ""
    Status: Bound
    Volume: pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    Labels: <none>
    Capacity: 20Gi
    Access Modes: RWX
    Events:
      FirstSeen LastSeen Count From        SubObjectPath Type Reason Message
      --------- -------- ----- ----        ------------- -------- ------ -------
      3m 3m 1 {ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 } Normal Provisioning External provisioner is provisioning volume for claim "default/my-persistent-volume-claim"
      3m 1m	 10 {persistentvolume-controller } Normal ExternalProvisioning cannot find provisioner "ibm.io/ibmc-file", expecting that a volume for the claim is provisioned either manually or via external software
      1m 1m 1 {ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 } Normal ProvisioningSucceeded	Successfully provisioned volume pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    ```
    {: screen}


您已成功创建 PV 对象，并将其绑定到 PVC。现在，集群用户可以[将 PVC 安装到其部署](#app_volume_mount)，并开始对 PV 对象执行读写操作。

<br />



## 使用集群中的现有块存储器
{: #existing_block}

开始之前，请确保您拥有可用于创建 PV 的现有块存储器实例。例如，如果先前已[使用 `retain` 存储类策略创建 PVC](#create)，那么可以将现有块存储器中的相应保留数据用于这一新的 PVC。

**注**：块存储器是 `ReadWriteOnce` 访问方式设备。一次只能将其安装到集群中一个工作程序节点上的一个 pod。

要创建 PV 和匹配的 PVC，请执行以下步骤。

1.  检索或生成 IBM Cloud Infrastructure (SoftLayer) 帐户的 API 密钥。
    1. 登录到 [IBM Cloud infrastructure (SoftLayer) 门户网站 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/)。
    2. 选择**帐户**，再选择**用户**，然后选择**用户列表**。
    3. 找到您的用户标识。
    4. 在 **API 密钥**列中，单击**生成**以生成 API 密钥，或者单击**查看**以查看现有 API 密钥。
2.  检索 IBM Cloud Infrastructure (SoftLayer) 帐户的 API 用户名。
    1. 从**用户列表**菜单中，选择您的用户标识。
    2. 在 **API 访问信息**部分中，找到您的 **API 用户名**。
3.  登录到 IBM Cloud Infrastructure CLI 插件。
    ```
    bx sl init
    ```
    {: pre}

4.  选择使用 IBM Cloud Infrastructure (SoftLayer) 帐户的用户名和 API 密钥进行认证。
5.  输入在先前步骤中检索到的用户名和 API 密钥。
6.  列出可用的块存储设备。
    ```
    bx sl block volume-list
    ```
    {: pre}

    输出示例：
    ```
    id         username            datacenter   storage_type              capacity_gb   bytes_used   ip_addr         lunId   active_transactions
    38642141   IBM02SEL1543159-1   dal10        endurance_block_storage   20            -            169.xx.xxx.xxx   170     0
    ```
    {: screen}

7.  记下要安装到集群的块存储设备的 `id`、`ip_addr`、`capacity_gb` 和 `lunId`。
8.  为 PV 创建配置文件。包含您在先前步骤中检索到的块存储器标识、IP 地址、大小和 LUN 标识。

    ```
    apiVersion: v1
    kind: PersistentVolume
    metadata:
      name: mypv
    spec:
      capacity:
        storage: "<storage_size>"
      accessModes:
        - ReadWriteOnce
      flexVolume:
        driver: "ibm/ibmc-block"
        fsType: "<fs_type>"
        options:
          "Lun": "<lun_ID>"
          "TargetPortal": "<IP_address>"
          "VolumeID": "<volume_ID>"
          "volumeName": "<volume_name>"
      ```
      {: codeblock}

    <table>
    <caption>了解 YAML 文件的组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td><code>metadata/name</code></td>
    <td>输入要创建的 PV 的名称。</td>
    </tr>
    <tr>
    <td><code>spec/flexVolume/fsType</code></td> 
    <td>输入为现有块存储器配置的文件系统类型。选择 <code>ext4</code> 或 <code>xfs</code>。如果未指定此选项，PV 将缺省为 <code>ext4</code>。如果定义了错误的 fsType，那么 PV 创建会成功，但将 PV 安装到 pod 会失败。</td></tr>	    
    <tr>
    <td><code>spec/capacity/storage</code></td>
    <td>输入在先前步骤中检索到的现有块存储器的存储器大小作为 <code>capacity-gb</code>。存储器大小必须以千兆字节为单位，例如 20Gi (20 GB) 或 1000Gi (1 TB)。</td>
    </tr>
    <tr>
    <td><code>flexVolume/options/Lun</code></td>
    <td>输入在先前步骤中检索到的块存储器的 LUN 标识作为 <code>lunId</code>。</td>
    </tr>
    <tr>
    <td><code>flexVolume/options/TargetPortal</code></td>
    <td>输入在先前步骤中检索到的块存储器的 IP 地址作为 <code>ip_addr</code>。</td>
    </tr>
    <tr>
	    <td><code>flexVolume/options/VolumeId</code></td>
	    <td>输入在先前步骤中检索到的块存储器的标识作为 <code>id</code>。</td>
	    </tr>
	    <tr>
		    <td><code>flexVolume/options/volumeName</code></td>
		    <td>输入卷的名称。</td>
	    </tr>
    </tbody></table>

9.  在集群中创建 PV。
    ```
    kubectl apply -f mypv.yaml
    ```
    {: pre}

10. 验证 PV 是否已创建。
    ```
    kubectl get pv
    ```
    {: pre}

11. 创建另一个配置文件以创建 PVC。为了使 PVC 与先前创建的 PV 相匹配，必须为 `storage` 和 `accessMode` 选择相同的值。`storage-class` 字段必须为空。如果其中任何字段与 PV 不匹配，那么会改为自动创建新的 PV。

     ```
         kind: PersistentVolumeClaim
    apiVersion: v1
    metadata:
     name: mypvc
     annotations:
       volume.beta.kubernetes.io/storage-class: ""
    spec:
      accessModes:
        - ReadWriteOnce
      resources:
        requests:
          storage: "20Gi"
     ```
     {: codeblock}

12.  创建 PVC。
     ```
     kubectl apply -f mypvc.yaml
     ```
     {: pre}

13.  验证 PVC 是否已创建并与先前创建的 PV 绑定。此过程可能需要几分钟时间。
     ```
    kubectl describe pvc mypvc
    ```
     {: pre}

     输出示例：

     ```
         Name: mypvc
    Namespace: default
    StorageClass: ""
    Status: Bound
     Volume: pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
     Labels: <none>
     Capacity: 20Gi
     Access Modes: RWO
     Events:
       FirstSeen LastSeen Count From        SubObjectPath Type Reason Message
       --------- -------- ----- ----        ------------- -------- ------ -------
       3m 3m 1 {ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 } Normal Provisioning External provisioner is provisioning volume  for claim "default/my-persistent-volume-claim"
       3m 1m	 10 {persistentvolume-controller } Normal ExternalProvisioning cannot find provisioner "ibm.io/ibmc-block", expecting that  a volume for the claim is provisioned either manually or via external software
       1m 1m 1 {ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 } Normal ProvisioningSucceeded	Successfully provisioned volume  pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
     ```
     {: screen}

您已成功创建 PV，并将其绑定到 PVC。现在，集群用户可以向自己的部署[安装 PVC](#app_volume_mount)，并开始对 PV 执行读写操作。

<br />



## 向应用程序添加 NFS 文件存储器或块存储器
{: #create}

创建持久性卷申领 (PVC) 以便为集群供应 NFS 文件存储器或块存储器。然后，将此申领安装到持久性卷 (PV) 可确保即便 pod 崩溃或关闭，数据也仍然可用。
{:shortdesc}

支持 PV 的 NFS 文件存储器和块存储器由 IBM 建立集群，以便为数据提供高可用性。存储类描述可用的存储产品类型，并在创建 PV 时定义数据保留策略、大小（以千兆字节计）和 IOPS 等方面。

**注**：块存储器是 `ReadWriteOnce` 访问方式设备。一次只能将其安装到集群中一个工作程序节点上的一个 pod。NFS 文件存储器是 `ReadWriteMany` 访问方式，因此可以将其安装到集群内多个工作程序上的多个 pod。

开始之前：
- 如果您有防火墙，请针对集群所在位置的 IBM Cloud Infrastructure (SoftLayer) IP 范围[允许流出访问](cs_firewall.html#pvc)，以便您可以创建 PVC。
- 如果要将块存储器安装到应用程序，必须首先安装[块存储器的 {{site.data.keyword.Bluemix_notm}} Storage 插件](#install_block)。

要添加持久性存储器，请执行以下操作：

1.  查看可用的存储类。{{site.data.keyword.containerlong}} 为 NFS 文件存储器和块存储器提供了预定义的存储类，因此集群管理员不必创建任何存储类。`ibmc-file-bronze` 存储类与 `default` 存储类相同。缺省情况下，文件存储器使用 `nfs` 文件系统进行供应，块存储器使用 `ext4` 文件系统进行供应。如果要使用 `XFS` 文件系统来供应块存储器，请[创建您自己的定制存储类](#custom_storageclass)。 

    ```
        kubectl get storageclasses
    ```
    {: pre}

    ```
        $ kubectl get storageclasses
    NAME                         TYPE
    default                      ibm.io/ibmc-file
    ibmc-file-bronze (default)   ibm.io/ibmc-file
    ibmc-file-custom             ibm.io/ibmc-file
    ibmc-file-gold               ibm.io/ibmc-file
    ibmc-file-retain-bronze      ibm.io/ibmc-file
    ibmc-file-retain-custom      ibm.io/ibmc-file
    ibmc-file-retain-gold        ibm.io/ibmc-file
    ibmc-file-retain-silver      ibm.io/ibmc-file
    ibmc-file-silver             ibm.io/ibmc-file
    ibmc-block-custom            ibm.io/ibmc-block
    ibmc-block-bronze            ibm.io/ibmc-block
    ibmc-block-gold              ibm.io/ibmc-block
    ibmc-block-silver            ibm.io/ibmc-block
    ibmc-block-retain-bronze     ibm.io/ibmc-block
    ibmc-block-retain-silver     ibm.io/ibmc-block
    ibmc-block-retain-gold       ibm.io/ibmc-block
    ibmc-block-retain-custom     ibm.io/ibmc-block
    ```
    {: screen}

    **提示：**如果要更改缺省存储类，请运行 `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'`，并将 `<storageclass>` 替换为存储类的名称。

2.  决定在删除 PVC 之后是否要保留数据和 NFS 文件共享或块存储器。
    - 如果要保留数据，请选择 `retain` 存储类。删除 PVC 时，仅会删除 PVC。PV 仍存在于集群中，并且会保存其中的数据，但不能将其复用于其他 PVC。此外，IBM Cloud Infrastructure (SoftLayer) 帐户中仍存在 NFS 文件或块存储器以及数据。日后，要访问集群中的这些数据，请创建一个 PVC 以及一个引用现有 [NFS 文件](#existing)或[块](#existing_block)存储器的匹配 PV。 
    - 如果要在删除 PVC 时删除 PV、数据和 NFS 文件共享或块存储器，请选择不带 `retain` 的存储类。

3.  **如果使用铜牌级、银牌级或金牌级存储类**：您将获得[耐久性存储器 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://knowledgelayer.softlayer.com/topic/endurance-storage)，用于为每个类定义 IOPS/GB。但是，您可以通过在可用范围内选择大小来确定总 IOPS。您可以在允许的大小范围内选择任意整数的千兆字节大小（例如，20 Gi、256 Gi、11854 Gi）。例如，如果在 4 IOPS/GB 银牌级存储类中选择 1000Gi 文件共享或块存储器大小，那么您的卷总计有 4000 IOPS。PV 的 IOPS 越高，处理输入和输出操作的速度越快。下表描述每个存储类的 IOPS/千兆字节和大小范围。

    <table>
         <caption>存储类大小范围和 IOPS/千兆字节表</caption>
         <thead>
         <th>存储类</th>
         <th>IOPS/千兆字节</th>
         <th>大小范围（千兆字节）</th>
         </thead>
         <tbody>
         <tr>
         <td>铜牌级（缺省值）</td>
         <td>2 IOPS/GB</td>
         <td>20-12000 Gi</td>
         </tr>
         <tr>
         <td>银牌级</td>
         <td>4 IOPS/GB</td>
         <td>20-12000 Gi</td>
         </tr>
         <tr>
         <td>金牌级</td>
         <td>10 IOPS/GB</td>
         <td>20-4000 Gi</td>
         </tr>
         </tbody></table>

    <p>**显示存储类详细信息的示例命令**：</p>

    <pre class="pre"><code>    kubectl describe storageclasses ibmc-file-silver
    </code></pre>

4.  **如果选择定制存储类**：您将获得[性能存储器 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://knowledgelayer.softlayer.com/topic/performance-storage)，并在选择 IOPS 和大小的组合时具有更多控制权。例如，如果为 PVC 选择大小 40 Gi，那么可以选择在 100 - 2000 IOPS 范围内且为 100 倍数的 IOPS。您选择的 IOPS 是静态的，不会随存储器大小进行扩展。如果选择了 40 Gi 和 100 IOPS，那么总 IOPS 仍为 100。下表显示根据所选大小，您可以选择的 IOPS 范围。

    <table>
         <caption>定制存储类大小范围和 IOPS 表</caption>
         <thead>
         <th>大小范围（千兆字节）</th>
         <th>IOPS 范围（100 的倍数）</th>
         </thead>
         <tbody>
         <tr>
         <td>20-39 Gi</td>
         <td>100-1000 IOPS</td>
         </tr>
         <tr>
         <td>40-79 Gi</td>
         <td>100-2000 IOPS</td>
         </tr>
         <tr>
         <td>80-99 Gi</td>
         <td>100-4000 IOPS</td>
         </tr>
         <tr>
         <td>100-499 Gi</td>
         <td>100-6000 IOPS</td>
         </tr>
         <tr>
         <td>500-999 Gi</td>
         <td>100-10000 IOPS</td>
         </tr>
         <tr>
         <td>1000-1999 Gi</td>
         <td>100-20000 IOPS</td>
         </tr>
         <tr>
         <td>2000-2999 Gi</td>
         <td>200-40000 IOPS</td>
         </tr>
         <tr>
         <td>3000-3999 Gi</td>
         <td>200-48000 IOPS</td>
         </tr>
         <tr>
         <td>4000-7999 Gi</td>
         <td>300-48000 IOPS</td>
         </tr>
         <tr>
         <td>8000-9999 Gi</td>
         <td>500-48000 IOPS</td>
         </tr>
         <tr>
         <td>10000-12000 Gi</td>
         <td>1000-48000 IOPS</td>
         </tr>
         </tbody></table>

    <p>**显示定制存储类详细信息的示例命令**：</p>

    <pre class="pre"><code>    kubectl describe storageclasses ibmc-file-retain-custom 
    </code></pre>

5.  决定是要按小时还是按月计费。缺省情况下，会按月计费。

6.  创建配置文件以定义 PVC，并将配置保存为 `.yaml` 文件。

    -  **铜牌级、银牌级或金牌级存储类的示例**：
       以下 `.yaml` 文件创建名为 `mypvc` 的申领，其中存储类为 `"ibmc-file-silver"`，计费类型为 `"hourly"`，千兆字节大小为 `24Gi`。如果要创建 PVC 以将块存储器安装到集群，请确保在 `accessModes` 部分中输入 `ReadWriteOnce`。

       ```
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: mypvc
         annotations:
           volume.beta.kubernetes.io/storage-class: "ibmc-file-silver"
         labels:
           billingType: "hourly"
       spec:
         accessModes:
           - ReadWriteMany
         resources:
           requests:
             storage: 24Gi
        ```
        {: codeblock}

    -  **定制存储类的示例**：
       以下 `.yaml` 文件创建名为 `mypvc` 的申领，其中存储类为 `ibmc-file-retain-custom`，计费类型为缺省值 `"monthly"`，千兆字节大小为 `45Gi`，IOPS 为 `"300"`。如果要创建 PVC 以将块存储器安装到集群，请确保在 `accessModes` 部分中输入 `ReadWriteOnce`。

       ```
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: mypvc
         annotations:
           volume.beta.kubernetes.io/storage-class: "ibmc-file-retain-custom"
         labels:
           billingType: "monthly"
       spec:
         accessModes:
           - ReadWriteMany
         resources:
           requests:
             storage: 45Gi
             iops: "300"
        ```
        {: codeblock}

        <table>
        <caption>了解 YAML 文件的组成部分</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
        </thead>
        <tbody>
        <tr>
        <td><code>metadata/name</code></td>
        <td>输入 PVC 的名称。</td>
        </tr>
        <tr>
        <td><code>metadata/annotations</code></td>
        <td>指定 PV 的存储类：
          <ul>
          <li>ibmc-file-bronze / ibmc-file-retain-bronze：2 IOPS/GB。</li>
          <li>ibmc-file-silver / ibmc-file-retain-silver：4 IOPS/GB。</li>
          <li>ibmc-file-gold / ibmc-file-retain-gold：10 IOPS/GB。</li>
          <li>ibmc-file-custom / ibmc-file-retain-custom：多个 IOPS 值可用。</li>
          <li>ibmc-block-bronze / ibmc-block-retain-bronze：2 IOPS/GB。</li>
          <li>ibmc-block-silver / ibmc-block-retain-silver：4 IOPS/GB。</li>
          <li>ibmc-block-gold / ibmc-block-retain-gold：10 IOPS/GB。</li>
          <li>ibmc-block-custom / ibmc-block-retain-custom：多个 IOPS 值可用。</li></ul>
          <p>如果未指定存储类，那么会使用缺省存储类来创建 PV。</p><p>**提示：**如果要更改缺省存储类，请运行 <code>kubectl patch storageclass &lt;storageclass&gt; -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'</code>，并将 <code>&lt;storageclass&gt;</code> 替换为存储类的名称。</p></td>
        </tr>
        <tr>
          <td><code>metadata/labels/billingType</code></td>
          <td>指定用于计算存储器帐单的频率："monthly" 或 "hourly"。缺省值为 "monthly"。</td>
        </tr>
        <tr>
        <td><code>spec/resources/requests/storage</code></td>
        <td>输入文件存储器的大小，以千兆字节 (Gi) 为单位。选择在允许大小范围内的整数。</br></br><strong>注</strong>：供应存储器后，即不能更改 NFS 文件共享或块存储器的大小。因此，请确保指定与要存储的数据量相匹配的大小。</td>
        </tr>
        <tr>
        <td><code>spec/resources/requests/iops</code></td>
        <td>此选项仅适用于定制存储类 (`ibmc-file-custom / ibmc-file-retain-custom / ibmc-block-custom / ibmc-block-retain-custom`)。请指定存储器的总 IOPS，在允许范围内选择 100 的倍数。要查看所有选项，请运行 `kubectl describe storageclasses <storageclass>`. 如果选择的 IOPS 不同于列出的 IOPS，那么该 IOPS 会向上舍入。</td>
        </tr>
        </tbody></table>
	
    如果要使用定制存储类，请使用相应的存储类名称、有效的 IOPS 和大小来创建 PVC。   
    {: tip}

7.  创建 PVC。

    ```
        kubectl apply -f mypvc.yaml
    ```
    {: pre}

8.  验证 PVC 是否已创建并与 PV 绑定。此过程可能需要几分钟时间。

    ```
        kubectl describe pvc mypvc
    ```
    {: pre}

    输出示例：

    ```
        Name: mypvc
    Namespace: default
    StorageClass: ""
    Status:		Bound
    Volume:		pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    Labels:		<none>
    Capacity:	20Gi
    Access Modes:	RWX
    Events:
      FirstSeen	LastSeen	Count	From								SubObjectPath	Type		Reason			Message
      ---------	--------	-----	----								-------------	--------	------			-------
      3m		3m		1	{ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 }			Normal		Provisioning		External provisioner is provisioning volume for claim "default/my-persistent-volume-claim"
      3m		1m		10	{persistentvolume-controller }							Normal		ExternalProvisioning	cannot find provisioner "ibm.io/ibmc-file", expecting that a volume for the claim is provisioned either manually or via external software
      1m		1m		1	{ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 }			Normal		ProvisioningSucceeded	Successfully provisioned volume pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2

    ```
    {: screen}

9.  {: #app_volume_mount}要将 PVC 安装到部署，请创建配置 `.yaml` 文件。

    ```
    apiVersion: apps/v1beta1
    kind: Deployment
    metadata:
      name: <deployment_name>
      labels:
        app: <deployment_label>
    spec:
      selector:
        matchLabels:
          app: <app_name>
      template:
        metadata:
          labels:
            app: <app_name>
        spec:
          containers:
          - image: <image_name>
            name: <container_name>
            volumeMounts:
            - name: <volume_name>
              mountPath: /<file_path>
          volumes:
          - name: <volume_name>
            persistentVolumeClaim:
              claimName: <pvc_name>
    ```
    {: codeblock}

    <table>
    <caption>了解 YAML 文件的组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
    </thead>
    <tbody>
        <tr>
    <td><code>metadata/labels/app</code></td>
    <td>部署的标签。</td>
      </tr>
      <tr>
        <td><code>spec/selector/matchLabels/app</code> <br/> <code>spec/template/metadata/labels/app</code></td>
        <td>应用程序的标签。</td>
      </tr>
    <tr>
    <td><code>template/metadata/labels/app</code></td>
    <td>部署的标签。</td>
      </tr>
    <tr>
    <td><code>spec/containers/image</code></td>
    <td>要使用的映像的名称。要列出 {{site.data.keyword.registryshort_notm}} 帐户中的可用映像，请运行 `bx cr image-list`。</td>
    </tr>
    <tr>
    <td><code>spec/containers/name</code></td>
    <td>要部署到集群的容器的名称。</td>
    </tr>
    <tr>
    <td><code>spec/containers/volumeMounts/mountPath</code></td>
    <td>在容器中安装卷的目录的绝对路径。</td>
    </tr>
    <tr>
    <td><code>spec/containers/volumeMounts/name</code></td>
    <td>要安装到 pod 中的卷的名称。</td>
    </tr>
    <tr>
    <td><code>volumes/name</code></td>
    <td>要安装到 pod 中的卷的名称。通常此名称与 <code>volumeMounts/name</code> 相同。</td>
    </tr>
    <tr>
    <td><code>volumes/persistentVolumeClaim/claimName</code></td>
    <td>要用作卷的 PVC 的名称。将卷安装到 pod 时，Kubernetes 会识别绑定到该 PVC 的 PV，并支持用户对 PV 执行读写操作。</td>
    </tr>
    </tbody></table>

10.  创建部署并安装 PVC。
     ```
    kubectl apply -f <local_yaml_path>
    ```
     {: pre}

11.  验证是否已成功安装卷。

     ```
         kubectl describe deployment <deployment_name>
    ```
     {: pre}

     安装点位于 **Volume Mounts** 字段中，卷位于 **Volumes** 字段中。

     ```
           Volume Mounts:
          /var/run/secrets/kubernetes.io/serviceaccount from default-token-tqp61 (ro)
          /volumemount from myvol (rw)
    ...
     Volumes:
      myvol:
        Type: PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
        ClaimName: mypvc
        ReadOnly: false

    ```
     {: screen}

{: #nonroot}
{: #enabling_root_permission}

**NFS 许可权**：在找有关启用 NFS 非 root 用户许可权的文档吗？请参阅[添加对 NFS 文件存储器的非 root 用户访问权](cs_troubleshoot_storage.html#nonroot)。

<br />

	
## 定制 XFS 块存储器的存储类
{: #custom_storageclass}

缺省情况下，由 {{site.data.keyword.containerlong}} 预定义的存储类会使用 `ext4` 文件系统来供应块存储器。您可以创建定制存储类以使用 `XFS` 文件系统来供应块存储器。
{: shortdesc}

开始之前： 
- [设定 Kubernetes CLI 的目标为集群](cs_cli_install.html#cs_cli_configure)。
- 安装[块存储器的 {{site.data.keyword.Bluemix_notm}} Storage 插件](#install_block)。

要创建定制存储类，请执行以下操作： 
1. 为定制存储类创建 yaml 文件。 
   ```
   apiVersion: storage.k8s.io/v1
   kind: StorageClass
   metadata:
     name: ibmc-block-custom-xfs
     labels:
       addonmanager.kubernetes.io/mode: Reconcile
   provisioner: ibm.io/ibmc-block
   parameters:
     type: "Performance"
     sizeIOPSRange: |-
       [20-39]Gi:[100-1000]
       [40-79]Gi:[100-2000]
       [80-99]Gi:[100-4000]
       [100-499]Gi:[100-6000]
       [500-999]Gi:[100-10000]
       [1000-1999]Gi:[100-20000]
       [2000-2999]Gi:[200-40000]
       [3000-3999]Gi:[200-48000]
       [4000-7999]Gi:[300-48000]
       [8000-9999]Gi:[500-48000]
       [10000-12000]Gi:[1000-48000]
     fsType: "xfs"
     reclaimPolicy: "Delete"
     classVersion: "2"
   ```
   {: codeblock}
   
   如果要在从集群中除去块存储器后保留数据，请将 `reclaimPolicy` 更改为 `Retain`。
   {: tip}
   
2. 在集群中创建存储类。 
   ```
   kubectl apply -f <filepath/xfs_storageclass.yaml>
   ```
   {: pre}
       
3. 验证是否已创建定制存储类。 
   ```
    kubectl get storageclasses
    ```
   {: pre}

4. 向 [XFS 块存储器](#create)供应定制存储类。 

<br />


## 更改缺省 NFS 文件存储器版本
{: #nfs_version}

NFS 文件存储器的版本确定了用于与 NFS 文件存储服务器通信的协议。缺省情况下，所有文件存储器实例都设置为使用 NFS V4。如果应用程序需要特定版本才能正常运行，那么可以将现有 PV 更改为较旧的 NFS 版本。
{: shortdesc}

要更改缺省 NFS 版本，您可以创建新的存储类以在集群中动态供应文件存储器，或者选择更改已安装到 pod 的现有 PV。 

要应用最新的安全性更新并提高性能，请使用缺省 NFS 版本，而不要更改为较旧的 NFS 版本。
{: tip}

**要使用所需的 NFS 版本来创建定制存储类，请执行以下操作：**
1. 为定制存储类创建 yaml 文件。将 <nfs_version> 替换为要使用的 NFS 版本。例如，要供应 NFS V3.0，请输入 **3.0**。
   ```
   apiVersion: storage.k8s.io/v1
   kind: StorageClass
   metadata:
     name: ibmc-file-mount
     #annotations:
     #  storageclass.beta.kubernetes.io/is-default-class: "true"
     labels:
       kubernetes.io/cluster-service: "true"
   provisioner: ibm.io/ibmc-file
   parameters:
     type: "Endurance"
     iopsPerGB: "2"
     sizeRange: "[1-12000]Gi"
     reclaimPolicy: "Delete"
     classVersion: "2"
     mountOptions: nfsvers=<nfs_version>
   ```
   {: codeblock}
   
   如果要在从集群中除去块存储器后保留数据，请将 `reclaimPolicy` 更改为 `Retain`。
   {: tip}
   
2. 在集群中创建存储类。 
   ```
   kubectl apply -f <filepath/nfsversion_storageclass.yaml>
   ```
   {: pre}
       
3. 验证是否已创建定制存储类。 
   ```
    kubectl get storageclasses
    ```
   {: pre}

4. 向[文件存储器](#create)供应定制存储类。 

**要将现有 PV 更改为使用其他 NFS 版本，请执行以下操作：**

1. 获取要在其中更改 NFS 版本的文件存储器的 PV，并记下该 PV 的名称。
   ```
    kubectl get pv
    ```
   {: pre}

2. 向 PV 添加注释。将 `<version_number>` 替换为要使用的 NFS 版本。例如，要更改为 NFS V3.0，请输入 **3**。  
   ```
   kubectl patch pv <pv_name> -p '{"metadata": {"annotations":{"volume.beta.kubernetes.io/mount-options":"vers=<version_number>"}}}'
   ```
   {: pre}
   
3. 删除使用文件存储器的 pod，然后重新创建 pod。 
   1. 将 pod yaml 保存到本地计算机。
      ```
      kubect get pod <pod_name> -o yaml > <filepath/pod.yaml>
      ```
      {: pre}
      
   2. 删除 pod。
      ```
      kubectl deleted pod <pod_name>
      ```
      {: pre}
   
   3. 重新创建 pod。
      ```
      kubectl apply -f <filepath/pod.yaml>
      ```
      {: pre}

4. 等待 pod 部署。 
   ```
            kubectl get pods
            ```
   {: pre}
   
   状态更改为 `Running` 时，说明 pod 已完全部署。 

5. 登录到 pod。 
   ```
   kubectl exec -it <pod_name> sh
   ```
   {: pre}
   
6. 验证文件存储器是否已安装了您先前指定的 NFS 版本。 
   ```
   mount | grep "nfs" | awk -F" |," '{ print $5, $8 }'
   ```
   {: pre}
   
   输出示例： 
   ```
   nfs vers=3.0
   ```
   {: screen}
   
<br />




## 安装 IBM Cloud Infrastructure (SoftLayer) CLI
{: #slcli}

安装 IBM Cloud Infrastructure (SoftLayer) CLI 以与基础架构资源（如 NFS 文件和块存储器实例）进行交互。
{: shortdesc}

开始之前，请[安装 Python 3.6](https://www.python.org/downloads/)。

1.  查看[安装文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://softlayer-api-python-client.readthedocs.io/en/latest/install/)。

    1.  单击**下载 tarball** 或**下载 zipball** 步骤中的链接。不要使用 `curl` 命令。
    2.  找到下载的软件包，将其解压缩，然后浏览至相应目录。
    3.  安装 CLI。
    
        ```
        python3 setup.py install
        ```
        {: pre}
    
2.  获取 IBM Cloud Infrastructure (SoftLayer) API 用户名和 API 密钥。

    1.  在 [{{site.data.keyword.Bluemix_notm}} 控制台](https://console.bluemix.net/)中，展开菜单并选择**基础架构**。
    2.  从菜单栏的概要文件中，选择要使用的基础架构帐户。
    3.  选择**帐户** > **用户** > **用户列表**。
    4.  在**用户**表的 **API 密钥**列中，单击**查看**。如果看不到 API 密钥，请单击**生成**。
    5.  复制弹出窗口中的用户名和 API 密钥。

3.  配置 CLI 以连接到 IBM Cloud Infrastructure (SoftLayer) 帐户。

    1.  配置 IBM Cloud infrastructure (SoftLayer) CLI。
        ```
        slcli setup
        ```
        {: pre}

    2.  填写必填信息。
    
        * **Username**：输入您先前检索到的 IBM Cloud Infrastructure (SoftLayer) API 用户名。
        * **API Key or Password**：输入您先前检索到的 IBM Cloud Infrastructure (SoftLayer) API 密钥。
        * **Endpoint (public|private|custom) [public]**：输入 `https://api.softlayer.com/rest/v3.1`。
        * **Timeout [0]**：输入 CLI 等待来自 API 的响应的时间值（以秒为单位）。值 `0` 将 CLI 设置为永久等待。
        
        **示例**：
        
        ```
        $ slcli setup
        Username []: 1234567_user.name@example.com
        API Key or Password []:         Endpoint (public|private|custom) [public]: https://api.softlayer.com/rest/v3.1
        Timeout [0]: 6000
        :..............:..................................................................:
        :         name : value                                                            :
        :..............:..................................................................:
        :     Username : 1234567_user.name@example.com                                    :
        :      API Key : 1111aa1111bbb22222b2b3c33333c3c3cc44d4444444444dd4444eee55e5e5e5 :
        : Endpoint URL : https://api.softlayer.com/xmlrpc/v3.1/                           :
        :      Timeout : 6000                                                             :
        :..............:..................................................................:
        Are you sure you want to write settings to "/Users/name/.softlayer"? [Y/n]: Y
        Configuration Updated Successfully
        ```
        {: screen}

您现在已准备就绪，可以使用 IBM Cloud Infrastructure (SoftLayer) CLI。

## 在集群上安装 {{site.data.keyword.Bluemix_notm}} Block Storage 插件
{: #install_block}

安装 {{site.data.keyword.Bluemix_notm}} Block Storage 插件和 Helm 图表，以便为块存储器设置预定义的存储类。可以使用这些存储类来创建用于为应用程序供应块存储器的 PVC。
{: shortdesc}

开始之前，请[设定 CLI 的目标](cs_cli_install.html#cs_cli_configure)为要在其中安装 {{site.data.keyword.Bluemix_notm}} Block Storage 插件的集群。

1. 在要使用 {{site.data.keyword.Bluemix_notm}} Block Storage 插件的集群上安装 [Helm](cs_integrations.html#helm)。
2. 更新 Helm 存储库以检索此存储库中最新版本的所有 Helm 图表。
   ```
        helm repo update
        ```
   {: pre}

3. 安装 {{site.data.keyword.Bluemix_notm}} Block Storage 插件。安装该插件时，会将预定义的块存储类添加到集群中。
   ```
   helm install ibm/ibmcloud-block-storage-plugin
   ```
   {: pre}

   输出示例：
   ```
   NAME:   bald-olm
   LAST DEPLOYED: Wed Apr 18 10:02:55 2018
   NAMESPACE: default
   STATUS: DEPLOYED

   RESOURCES:
   ==> v1beta1/DaemonSet
   NAME                           DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE SELECTOR  AGE
   ibmcloud-block-storage-driver  0        0        0      0           0          <none>         0s

   ==> v1beta1/Deployment
   NAME                           DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
   ibmcloud-block-storage-plugin  1        0        0           0          0s

   ==> v1/StorageClass
   NAME                      PROVISIONER        AGE
   ibmc-block-bronze         ibm.io/ibmc-block  0s
   ibmc-block-custom         ibm.io/ibmc-block  0s
   ibmc-block-gold           ibm.io/ibmc-block  0s
   ibmc-block-retain-bronze  ibm.io/ibmc-block  0s
   ibmc-block-retain-custom  ibm.io/ibmc-block  0s
   ibmc-block-retain-gold    ibm.io/ibmc-block  0s
   ibmc-block-retain-silver  ibm.io/ibmc-block  0s
   ibmc-block-silver         ibm.io/ibmc-block  0s

   ==> v1/ServiceAccount
   NAME                           SECRETS  AGE
   ibmcloud-block-storage-plugin  1        0s

   ==> v1beta1/ClusterRole
   NAME                           AGE
   ibmcloud-block-storage-plugin  0s

   ==> v1beta1/ClusterRoleBinding
   NAME                           AGE
   ibmcloud-block-storage-plugin  0s

   NOTES:
   Thank you for installing: ibmcloud-block-storage-plugin.   Your release is named: bald-olm
   ```
   {: screen}

4. 验证安装是否已成功。
   ```
   kubectl get pod -n kube-system | grep block
   ```
   {: pre}

   输出示例：
   ```
   ibmcloud-block-storage-driver-kh4mt                              1/1       Running   0          27d       10.118.98.19   10.118.98.19
   ibmcloud-block-storage-plugin-58c5f9dc86-pbl4t                   1/1       Running   0          14d       172.21.0.204   10.118.98.19
   ```
   {: screen}
   
   看到一个 `ibmcloud-block-storage-plugin` pod 以及一个或多个 `ibmcloud-block-storage-driver` pod 时，说明安装成功。`ibmcloud-block-storage-driver` 的数量等于集群中的工作程序节点数。所有 pod 都必须处于 **Running** 状态。 

5. 验证块存储器的存储类是否已添加到集群。
   ```
   kubectl get storageclasses | grep block
   ```
   {: pre}

   输出示例：
   ```
   ibmc-block-bronze            ibm.io/ibmc-block
   ibmc-block-custom            ibm.io/ibmc-block
   ibmc-block-gold              ibm.io/ibmc-block
   ibmc-block-retain-bronze     ibm.io/ibmc-block
   ibmc-block-retain-custom     ibm.io/ibmc-block
   ibmc-block-retain-gold       ibm.io/ibmc-block
   ibmc-block-retain-silver     ibm.io/ibmc-block
   ibmc-block-silver            ibm.io/ibmc-block
   ```
   {: screen}

6. 对要供应块存储器的每个集群，重复这些步骤。

现在可以继续[创建 PVC](#create)，以用于为应用程序供应块存储器。

<br />


### 更新 {{site.data.keyword.Bluemix_notm}} Block Storage 插件
现在，可以将现有 {{site.data.keyword.Bluemix_notm}} Block Storage 插件升级到最新版本。
{: shortdesc}

开始之前，请[设定 CLI 的目标](cs_cli_install.html#cs_cli_configure)为集群。

1. 找到已安装在集群中的块存储器 Helm 图表的名称。
   ```
   helm ls | grep ibmcloud-block-storage-plugin
   ```
   {: pre}

   输出示例：
   ```
   myhelmchart 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-block-storage-plugin-0.1.0	default
   ```
   {: screen}

2. 将 {{site.data.keyword.Bluemix_notm}} Block Storage 插件升级到最新版本。
   ```
   helm upgrade --force --recreate-pods <helm_chart_name>  ibm/ibmcloud-block-storage-plugin
   ```
   {: pre}

<br />


### 除去 {{site.data.keyword.Bluemix_notm}} Block Storage 插件
如果不想对集群供应和使用 {{site.data.keyword.Bluemix_notm}} Block Storage，那么可以卸载 Helm 图表。
{: shortdesc}

**注**：除去该插件不会除去现有 PVC、PV 或数据。除去该插件时，将从集群中除去所有相关的 pod 和守护程序集。除去该插件后，无法为集群供应新的块存储器，也无法使用现有块存储器 PVC 和 PV。

开始之前，请[设定 CLI 的目标](cs_cli_install.html#cs_cli_configure)为集群，并确保该集群中没有任何使用块存储器的 PVC 或 PV。

1. 找到已安装在集群中的块存储器 Helm 图表的名称。
   ```
   helm ls | grep ibmcloud-block-storage-plugin
   ```
   {: pre}

   输出示例：
   ```
   myhelmchart 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-block-storage-plugin-0.1.0	default
   ```
   {: screen}

2. 删除 {{site.data.keyword.Bluemix_notm}} Block Storage 插件。
   ```
   helm delete <helm_chart_name>
   ```
   {: pre}

3. 验证块存储器 pod 是否已除去。
   ```
   kubectl get pod -n kube-system | grep ibmcloud-block-storage-plugin
   ```
   {: pre}
   如果 CLI 输出中未显示任何 pod，说明 pod 除去操作成功。

4. 验证块存储器的存储类是否已除去。
   ```
   kubectl get storageclasses | grep block
   ```
   {: pre}
   如果 CLI 输出中未显示任何存储类，说明存储类除去操作成功。

<br />



## 为 NFS 文件共享和块存储器设置备份和复原解决方案
{: #backup_restore}

文件共享和块存储器会供应到集群所在的位置。存储器由 {{site.data.keyword.IBM_notm}} 在集群服务器上托管，以在服务器停止运行时提供可用性。但是，文件共享和块存储器不会自动进行备份，因此它们在整个位置发生故障时可能无法进行访问。为了防止数据丢失或损坏，可以设置定期备份，以便在需要时可用于复原数据。
{: shortdesc}

查看 NFS 文件共享和块存储器的以下备份和复原选项：

<dl>
  <dt>设置定期快照</dt>
  <dd><p>可以为 NFS 文件共享或块存储器设置定期快照，这是捕获某个时间点的实例状态的只读图像。要存储快照，必须在 NFS 文件共享或块存储器上请求快照空间。快照会存储在同一位置的现有存储器实例上。如果用户意外地从卷中除去了重要数据，那么可以通过快照来复原数据。</br></br> <strong>要为卷创建快照，请执行以下操作：</strong><ol><li>列出集群中的现有 PV。<pre class="pre"><code>    kubectl get pv
    </code></pre></li><li>获取要为其创建快照空间的 PV 的详细信息，并记下卷标识、大小和 IOPS。<pre class="pre"><code>kubectl describe pv &lt;pv_name&gt;</code></pre> 对于文件存储器，可以在 CLI 输出的 <strong>Labels</strong> 部分中找到卷标识、大小和 IOPS。对于块存储器，大小和 IOPS 会显示在 CLI 输出的 <strong>Labels</strong> 部分中。要查找卷标识，请查看 CLI 输出的 <code>ibm.io/network-storage-id</code> 注释。</li><li>使用您在先前步骤中检索到的参数为现有卷创建快照大小。<pre class="pre"><code>slcli file snapshot-order --capacity &lt;size&gt; --tier &lt;iops&gt; &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-order --capacity &lt;size&gt; --tier &lt;iops&gt; &lt;volume_id&gt;</code></pre></li><li>等待快照大小创建。<pre class="pre"><code>slcli file volume-detail &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block volume-detail &lt;volume_id&gt;</code></pre>CLI 输出中的 <strong>Snapshot Capacity (GB)</strong> 从 0 更改为您所订购的大小时，说明已成功供应快照大小。</li><li>为卷创建快照，并记下创建的快照的标识。<pre class="pre"><code>slcli file snapshot-create &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-create &lt;volume_id&gt;</code></pre></li><li>验证快照是否已成功创建。<pre class="pre"><code>slcli file volume-detail &lt;snapshot_id&gt;</code></pre><pre class="pre"><code>slcli block volume-detail &lt;snapshot_id&gt;</code></pre></li></ol></br><strong>要将数据从快照复原到现有卷，请运行以下命令：</strong><pre class="pre"><code>slcli file snapshot-restore -s &lt;snapshot_id&gt; &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-restore -s &lt;snapshot_id&gt; &lt;volume_id&gt;</code></pre></br>有关更多信息，请参阅：<ul><li>[NFS 定期快照](/docs/infrastructure/FileStorage/snapshots.html)</li><li>[块定期快照](/docs/infrastructure/BlockStorage/snapshots.html#snapshots)</li></ul></p></dd>
  <dt>将快照复制到其他位置</dt>
 <dd><p>为了保护数据不受位置故障的影响，可以[复制快照](/docs/infrastructure/FileStorage/replication.html#working-with-replication)到安装在其他位置的 NFS 文件共享或块存储器实例。数据只能从主存储器复制到备份存储器。不能将复制的 NFS 文件共享或块存储器实例安装到集群。主存储器发生故障时，可以手动将复制的备份存储器设置为主存储器。然后，可以将其安装到集群。复原主存储器后，可以从备份存储器复原数据。</p>
 <p>有关更多信息，请参阅：<ul><li>[复制 NFS 的快照](/docs/infrastructure/FileStorage/replication.html#working-with-replication)</li><li>[复制块的快照](/docs/infrastructure/BlockStorage/replication.html#working-with-replication)</li></ul></p></dd>
 <dt>复制存储器</dt>
 <dd><p>可以在原始存储器实例所在的位置中，复制 NFS 文件共享或块存储器实例。复制项采用原始存储器实例在创建该复制项的时间点的数据。与副本不同，复制项用作独立于原始项的存储器实例。要进行复制，请首先为该卷设置快照。</p>
 <p>有关更多信息，请参阅：<ul><li>[NFS 复制快照](/docs/infrastructure/FileStorage/how-to-create-duplicate-volume.html#creating-a-duplicate-file-storage)</li><li>[块复制快照](/docs/infrastructure/BlockStorage/how-to-create-duplicate-volume.html#creating-a-duplicate-block-volume)</li></ul></p></dd>
  <dt>将数据备份到 Object Storage</dt>
  <dd><p>可以使用 [**ibm-backup-restore 映像**](/docs/services/RegistryImages/ibm-backup-restore/index.html#ibmbackup_restore_starter)来加快集群中的备份和复原 pod。此 pod 包含一个脚本，用于对集群中的任何持久性卷申领 (PVC) 运行一次性或定期备份。数据存储在您在某个位置设置的 {{site.data.keyword.objectstoragefull}} 实例中。</p>
  <p>要使数据具有更高可用性，并保护应用程序不受位置故障的影响，请设置第二个 {{site.data.keyword.objectstoragefull}} 实例，并在各个位置之间复制数据。如果需要从 {{site.data.keyword.objectstoragefull}} 实例复原数据，请使用随映像一起提供的复原脚本。</p></dd>
<dt>将数据复制到 pod 和容器，以及从 pod 和容器中复制数据</dt>
<dd><p>可以使用 `kubectl cp` [命令 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/reference/kubectl/overview/#cp) 将文件和目录复制到集群中的 pod 或特定容器，或者从集群中的 pod 或特定容器中复制文件和目录。</p>
<p>开始之前，请[设定 Kubernetes CLI 的目标](cs_cli_install.html#cs_cli_configure)为要使用的集群。如果未使用 <code>-c</code> 来指定容器，那么此命令将使用 pod 中的第一个可用容器。</p>
<p>可以通过以下各种方法来使用此命令：</p>
<ul>
<li>将本地机器中的数据复制到集群中的 pod：<pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var></code></pre></li>
<li>将集群的 pod 中的数据复制到本地机器：<pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;/&lt;filename&gt;</var> <var>&lt;local_filepath&gt;/&lt;filename&gt;</var></code></pre></li>
<li>将集群的 pod 中的数据复制到其他集群的其他 pod 中的特定容器：<pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var> <var>&lt;namespace&gt;/&lt;other_pod&gt;:&lt;pod_filepath&gt;</var> -c <var>&lt;container></var></code></pre></li>
</ul></dd>
  </dl>


