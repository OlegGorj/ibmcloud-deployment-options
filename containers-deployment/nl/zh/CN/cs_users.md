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



# 分配集群访问权
{: #users}

作为集群管理员，您可以为 Kubernetes 集群定义访问策略，以便为不同用户创建不同级别的访问权。例如，您可以授权某些用户使用集群资源，而其他用户只能部署容器。
{: shortdesc}


## 规划访问请求
{: #planning_access}

作为集群管理员，可能很难跟踪访问请求。因此，建立访问请求的通信模式对于维护集群的安全性至关重要。
{: shortdesc}

为了确保正确的人员具有正确的访问权，请在策略上明确规定谁可以请求访问权或获取有关常见任务的帮助。

您可能已经有一个适用于您团队的方法，那很好！如果您还不知道从何着手，请考虑尝试下列其中一种方法。

*  创建凭单系统
*  创建表单模板
*  创建 Wiki 页面
*  需要电子邮件请求
*  使用您已用于跟踪团队日常工作的问题跟踪系统

有些不知所措？请尝试了解有关[组织用户、团队和应用程序的最佳实践](/docs/tutorials/users-teams-applications.html)的这一教程。
{: tip}

## 访问策略和许可权
{: #access_policies}

访问策略的作用域基于用户定义的一个或多个角色，这些角色确定允许用户执行的操作。可以设置特定于集群、基础架构、服务实例或 Cloud Foundry 角色的策略。
{: shortdesc}

必须为使用 {{site.data.keyword.containershort_notm}} 的每个用户定义访问策略。某些策略是预定义策略，但其他策略可以进行定制。查看下图及定义，以了解哪些角色适用于常见用户任务，并确定您可能希望在哪里定制策略。

![{{site.data.keyword.containershort_notm}} 访问角色](/images/user-policies.png)

图. {{site.data.keyword.containershort_notm}} 访问角色

<dl>
  <dt>身份和访问管理 (IAM) 策略</dt>
    <dd><p><strong>平台</strong>：可以确定个人可在 {{site.data.keyword.containershort_notm}} 集群上执行的操作。可以按区域设置这些策略。操作示例包括创建或除去集群，或者添加额外的工作程序节点。这些策略必须与基础架构策略一起设置。</p>
    <p><strong>基础架构</strong>：可以确定基础架构的访问级别，例如集群节点机器、联网或存储资源。无论用户是通过 {{site.data.keyword.containershort_notm}} GUI 还是通过 CLI 发出请求，都将强制执行同一策略；即便在 IBM Cloud Infrastructure (SoftLayer) 中完成这些操作也是如此。必须将此类型的策略与 {{site.data.keyword.containershort_notm}} 平台访问策略一起设置。要了解可用角色的信息，请查看[基础架构许可权](/docs/iam/infrastructureaccess.html#infrapermission)。</p> </br></br><strong>注：</strong>确保您的 {{site.data.keyword.Bluemix_notm}} 帐户已[设置为有权访问 IBM Cloud Infrastructure (SoftLayer) 产品服务组合](cs_troubleshoot_clusters.html#cs_credentials)，以便授权用户可以根据分配的许可权在 IBM Cloud Infrastructure (SoftLayer) 帐户中执行操作。</dd>
  <dt>Kubernetes 基于资源的访问控制 (RBAC) 角色</dt>
    <dd>分配了平台访问策略的每个用户都会自动分配有 Kubernetes 角色。在 Kubernetes 中，[基于角色的访问控制 (RBAC) ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview) 将确定用户可以对集群中的资源执行的操作。<strong>注</strong>：对于 <code>default</code> 名称空间，会自动为其配置 RBAC 角色，但作为集群管理员，您可以为其他名称空间分配角色。</dd>
  <dt>Cloud Foundry</dt>
    <dd>并非所有服务都可以通过 Cloud IAM 进行管理。如果使用的是无法通过 Cloud IAM 管理的其中一个服务，那么可以继续使用 [Cloud Foundry 用户角色](/docs/iam/cfaccess.html#cfaccess)来控制对服务的访问。</dd>
</dl>


要降级许可权？完成此操作可能需要几分钟时间。
{: tip}

### 平台角色
{: #platform_roles}

{{site.data.keyword.containershort_notm}} 配置为使用 {{site.data.keyword.Bluemix_notm}} 平台角色。角色许可权基于彼此进行构建，这意味着`编辑者`角色具有与`查看者`角色相同的许可权，外加授予编辑者的许可权。下表说明了每个角色可以执行的操作的类型。

您分配平台角色时，会将相应的 RBAC 角色自动分配给缺省名称空间。如果您更改用户的平台角色，那么还会更新 RBAC 角色。
{: tip}

<table>
<caption>平台角色和操作</caption>
  <tr>
    <th>平台角色</th>
    <th>操作示例</th>
    <th>相应的 RBAC 角色</th>
  </tr>
  <tr>
      <td>查看者</td>
      <td>查看集群或其他服务实例的详细信息。</td>
      <td>查看</td>
  </tr>
  <tr>
    <td>编辑者</td>
    <td>可以将 IBM Cloud 服务绑定到集群或从集群取消绑定，也可以创建 Webhook。<strong>注</strong>：要绑定服务，您还必须分配有 Cloud Foundry 开发者角色。</td>
    <td>编辑</td>
  </tr>
  <tr>
    <td>操作员</td>
    <td>可以创建、除去、重新引导或重新装入工作程序节点。可以将子网添加到集群。</td>
    <td>管理</td>
  </tr>
  <tr>
    <td>管理员</td>
    <td>可以创建和除去集群。可以在服务和基础架构的帐户级别编辑其他用户的访问策略。<strong>注</strong>：可以将管理访问权分配给特定集群，也可以分配给您帐户的所有服务实例。要删除集群，您必须具有对要删除的集群的管理访问权。要创建集群，您必须具有对该服务所有实例的管理角色。</td>
    <td>集群管理</td>
  </tr>
</table>

有关在 UI 中分配用户角色的更多信息，请参阅[管理 IAM 访问权](/docs/iam/mngiam.html#iammanidaccser)。


### 基础架构角色
{: #infrastructure_roles}

基础架构角色支持用户对基础架构级别的资源执行任务。下表说明了每个角色可以执行的操作的类型。基础架构角色可定制；请务必仅向用户授予执行其作业所需的访问权。

除了授予特定的基础架构角色外，您还必须将设备访问权授予使用基础架构的用户。
{: tip}

<table>
<caption>基础架构角色和操作</caption>
  <tr>
    <th>基础架构角色</th>
    <th>操作示例</th>
  </tr>
  <tr>
    <td><i>仅查看</i></td>
    <td>可以查看基础架构详细信息和帐户摘要，包括发票和付款情况</td>
  </tr>
  <tr>
    <td><i>基本用户</i></td>
    <td>可以编辑服务配置（包括 IP 地址），添加或编辑 DNS 记录，以及添加有权访问基础架构的新用户</td>
  </tr>
  <tr>
    <td><i>超级用户</i></td>
    <td>可以执行与基础架构相关的所有操作</td>
  </tr>
</table>

要开始分配角色，请执行[定制用户的基础架构许可权](#infra_access)中的步骤。

### RBAC 角色
{: #rbac_roles}

基于资源的访问控制 (RBAC) 是一种保护集群内部资源并决定谁可以执行哪些 Kubernetes 操作的方法。在下表中，可以查看 RBAC 角色的类型以及具有该角色的用户可以执行的操作的类型。许可权基于彼此进行构建，这意味着`管理`同时具有`查看`和`编辑`角色随附的所有策略。请务必仅授予用户所需的访问权。

对于缺省名称空间，RBAC 角色会与平台角色一起自动设置。[您可以更新角色，也可以为其他名称空间分配角色](#rbac)。
{: tip}

<table>
<caption>RBAC 角色和操作</caption>
  <tr>
    <th>RBAC 角色</th>
    <th>操作示例</th>
  </tr>
  <tr>
    <td>查看</td>
    <td>可以查看缺省名称空间内部的资源。查看者无法查看 Kubernetes 的私钥。</td>
  </tr>
  <tr>
    <td>编辑</td>
    <td>对缺省名称空间内部资源执行读写操作。</td>
  </tr>
  <tr>
    <td>管理</td>
    <td>对缺省名称空间内的资源执行读写操作，但不能对名称空间本身执行读写操作。可以在名称空间内创建角色。</td>
  </tr>
  <tr>
    <td>集群管理</td>
    <td>可以对每个名称空间中的资源执行读写操作。可以在名称空间内创建角色。可以访问 Kubernetes 仪表板。可以创建使应用程序公开可用的 Ingress 资源。</td>
  </tr>
</table>

<br />


## 向 {{site.data.keyword.Bluemix_notm}} 帐户添加用户
{: #add_users}

可以将用户添加到 {{site.data.keyword.Bluemix_notm}} 帐户以授予对集群的访问权。
{:shortdesc}

开始之前，请验证您是否已分配有对 {{site.data.keyword.Bluemix_notm}} 帐户的`管理员` Cloud Foundry 角色。

1.  [向帐户添加用户](../iam/iamuserinv.html#iamuserinv).
2.  在**访问权**部分中，展开**服务**。
3.  为用户分配平台角色以设置对 {{site.data.keyword.containershort_notm}} 的访问权。
      1. 从**服务**下拉列表中，选择 **{{site.data.keyword.containershort_notm}}**。
      2. 从**区域**下拉列表中，选择要邀请用户加入的区域。
      3. 从**服务实例**下拉列表中，选择要邀请用户加入的集群。要找到特定集群的标识，请运行 `bx cs clusters`。
      4. 在**选择角色**部分中，选择角色。要查找每个角色的受支持操作列表，请参阅[访问策略和许可权](#access_policies)。
4. [可选：分配 Cloud Foundry 角色](/docs/iam/mngcf.html#mngcf)。
5. [可选：分配基础架构角色](/docs/iam/infrastructureaccess.html#infrapermission)。
6. 单击**邀请用户**。

<br />


## 了解 IAM API 密钥和 `bx cs credentials-set` 命令
{: #api_key}

要成功供应和使用帐户中的集群，必须确保帐户已正确设置为访问 IBM Cloud Infrastructure (SoftLayer) 产品服务组合。根据帐户设置，可以使用通过 `bx cs credentials-set` 命令手动设置的 IAM API 密钥或基础架构凭证。

<dl>
  <dt>IAM API 密钥</dt>
    <dd><p>执行需要 {{site.data.keyword.containershort_notm}} 管理员访问策略的第一个操作时，会自动针对区域设置 Identity and Access Management (IAM) API 密钥。例如，某个管理用户在 <code>us-south</code> 区域中创建了第一个集群。通过执行此操作，此用户的 IAM API 密钥将存储在此区域的帐户中。API 密钥用于订购 IBM Cloud Infrastructure (SoftLayer)，例如新的工作程序节点或 VLAN。</p> <p>其他用户在此区域中执行需要与 IBM Cloud Infrastructure (SoftLayer) 产品服务组合进行交互的操作（例如，创建新集群或重新装入工作程序节点）时，将使用存储的 API 密钥来确定是否存在执行该操作的足够许可权。要确保可以成功执行集群中与基础架构相关的操作，请为 {{site.data.keyword.containershort_notm}} 管理用户分配<strong>超级用户</strong>基础架构访问策略。</p>
    <p>您可以通过运行 [<code>bx cs api-key-info</code>](cs_cli_reference.html#cs_api_key_info) 来查找当前 API 密钥所有者。如果发现需要更新为区域存储的 API 密钥，那么可以通过运行 [<code>bx cs api-key-reset</code>](cs_cli_reference.html#cs_api_key_reset) 命令来执行此操作。此命令需要 {{site.data.keyword.containershort_notm}} 管理员访问策略，并在帐户中存储执行此命令的用户的 API 密钥。</p>
    <p><strong>注：</strong>如果使用 <code>bx cs credentials-set</code> 命令手动设置了 IBM Cloud Infrastructure (SoftLayer) 凭证，那么可能不会使用为该区域存储的 API 密钥。</p></dd>
  <dt>通过 <code>bx cs credentials-set</code> 设置的 IBM Cloud Infrastructure (SoftLayer) 凭证</dt>
    <dd><p>如果您有 {{site.data.keyword.Bluemix_notm}} 现买现付帐户，那么缺省情况下您可以访问 IBM Cloud Infrastructure (SoftLayer) 产品服务组合。但是，您可能希望使用已经拥有的其他 IBM Cloud Infrastructure (SoftLayer) 帐户来订购基础架构。您可以使用 [<code>bx cs credentials-set</code>](cs_cli_reference.html#cs_credentials_set) 命令将此基础架构帐户链接到 {{site.data.keyword.Bluemix_notm}} 帐户。</p>
    <p>如果手动设置了 IBM Cloud Infrastructure (SoftLayer) 凭证，那么这些凭证会用于订购基础架构，即使已存在帐户的 IAM API 密钥也不例外。如果存储了其凭证的用户没有必需的许可权来订购基础架构，那么与基础架构相关的操作（例如，创建集群或重新装入工作程序节点）可能会失败。</p>
    <p>要除去手动设置的 IBM Cloud Infrastructure (SoftLayer) 凭证，可以使用 [<code>bx cs credentials-unset</code>](cs_cli_reference.html#cs_credentials_unset) 命令。除去凭证后，将使用 IAM API 密钥来订购基础架构。</p></dd>
</dl>

<br />


## 定制用户的基础架构许可权
{: #infra_access}

在 Identity and Access Management 中设置基础架构策略时，会向用户授予与角色相关联的许可权。要定制这些许可权，必须登录到 IBM Cloud Infrastructure (SoftLayer) 并在其中调整许可权。
{: #view_access}

例如，**基本用户**可以重新引导工作程序节点，但无法重新装入工作程序节点。在不授予此人**超级用户**许可权的情况下，可以调整 IBM Cloud Infrastructure (SoftLayer) 许可权，并添加许可权以运行重新装入命令。

1.  登录到 [{{site.data.keyword.Bluemix_notm}} 帐户](https://console.bluemix.net/)，然后从菜单中选择**基础架构**。

2.  转至**帐户** > **用户** > **用户列表**。

3.  要修改许可权，请选择用户概要文件的名称或**设备访问权**列。

4.  在**门户网站许可权**选项卡中，定制用户的访问权。用户所需的许可权取决于需要使用的基础架构资源：

    * 使用**快速许可权**下拉列表来分配**超级用户**角色，此角色将授予用户所有许可权。
    * 使用**快速许可权**下拉列表来分配**基本用户**角色，此角色将授予用户部分（而不是全部）所需的许可权。
    * 如果不想授予**超级用户**角色的所有许可权，或者不需要添加除**基本用户**角色以外的许可权，请查看下表中对 {{site.data.keyword.containershort_notm}} 中执行常见任务所需许可权的描述。

    <table summary="常见 {{site.data.keyword.containershort_notm}} 场景的基础架构许可权。">
     <caption>{{site.data.keyword.containershort_notm}} 通常必需的基础架构许可权</caption>
     <thead>
      <th>{{site.data.keyword.containershort_notm}} 中的常见任务</th>
      <th>必需的基础架构许可权（按选项卡）</th>
     </thead>
     <tbody>
       <tr>
         <td><strong>最低许可权</strong>：<ul><li>创建集群。</li></ul></td>
         <td><strong>设备</strong>：<ul><li>查看虚拟服务器详细信息</li><li>重新引导服务器并查看 IPMI 系统信息</li><li>发出操作系统重装并启动急救内核</li></ul><strong>帐户</strong>：<ul><li>添加/升级云实例</li><li>添加服务器</li></ul></td>
       </tr>
       <tr>
         <td><strong>集群管理</strong>：<ul><li>创建、更新和删除集群。</li><li>添加、重新装入和重新引导工作程序节点。</li><li>查看 VLAN。</li><li>创建子网。</li><li>部署 pod 和 LoadBalancer 服务。</li></ul></td>
         <td><strong>支持</strong>：<ul><li>查看凭单</li><li>添加凭单</li><li>编辑凭单</li></ul>
         <strong>设备</strong>：<ul><li>查看虚拟服务器详细信息</li><li>重新引导服务器并查看 IPMI 系统信息</li><li>升级服务器</li><li>发出操作系统重装并启动急救内核</li></ul>
         <strong>服务</strong>：<ul><li>管理 SSH 密钥</li></ul>
         <strong>帐户</strong>：<ul><li>查看帐户摘要</li><li>添加/升级云实例</li><li>取消服务器</li><li>添加服务器</li></ul></td>
       </tr>
       <tr>
         <td><strong>存储器</strong>：<ul><li>创建持久性卷申领以供应持久卷。</li><li>创建和管理存储器基础架构资源。</li></ul></td>
         <td><strong>服务</strong>：<ul><li>管理存储器</li></ul><strong>帐户</strong>：<ul><li>添加存储器</li></ul></td>
       </tr>
       <tr>
         <td><strong>专用联网</strong>：<ul><li>管理用于集群内联网的专用 VLAN。</li><li>设置与专用网络的 VPN 连接。</li></ul></td>
         <td><strong>网络</strong>：<ul><li>管理网络子网路径</li><li>管理 IPSEC 网络隧道</li><li>管理网络网关</li><li>VPN 管理</li></ul></td>
       </tr>
       <tr>
         <td><strong>公用网络</strong>：<ul><li>设置公共负载均衡器或 Ingress 联网以公开应用程序。</li></ul></td>
         <td><strong>设备</strong>：<ul><li>管理负载均衡器</li><li>编辑主机名/域</li><li>管理端口控制</li></ul>
         <strong>网络</strong>：<ul><li>使用公用网络端口添加计算</li><li>管理网络子网路径</li><li>添加 IP 地址</li></ul>
         <strong>服务</strong>：<ul><li>管理 DNS、逆向 DNS 和 WHOIS</li><li>查看证书 (SSL)</li><li>管理证书 (SSL)</li></ul></td>
       </tr>
     </tbody>
    </table>

5.  要保存更改，请单击**编辑门户网站许可权**。

6.  在**设备访问权**选项卡中，选择要向其授予访问权的设备。

    * 在**设备类型**下拉列表中，可以授予对**所有虚拟服务器**的访问权。
    * 要允许用户访问所创建的新设备，请选择**添加新设备时自动授予访问权**。
    * 要保存更改，请单击**更新设备访问权**。

<br />


## 授予用户定制 Kubernetes RBAC 角色
{: #rbac}

{{site.data.keyword.containershort_notm}} 访问策略与特定 Kubernetes 基于角色的访问控制 (RBAC) 角色相对应，如[访问策略和许可权](#access_policies)中所述。要授权不同于相应访问策略的其他 Kubernetes 角色，您可以定制 RBAC 角色，然后将这些角色分配给个人或用户组。
{: shortdesc}

例如，您可能希望授予开发者团队许可权，以对特定 API 组或集群中某个 Kubernetes 名称空间内的资源（而不是整个集群）进行操作。使用对 {{site.data.keyword.containershort_notm}} 唯一的用户名创建角色，然后将该角色绑定到用户。有关更多信息，请参阅 Kubernetes 文档中的[使用 RBAC 授权 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview)。

开始之前，请[设定 Kubernetes CLI 的目标为集群](cs_cli_install.html#cs_cli_configure)

1.  创建具有要分配的访问权的角色。

    1. 创建 `.yaml` 文件以定义具有要分配的访问权的角色。

        ```
        kind: Role
        apiVersion: rbac.authorization.k8s.io/v1
        metadata:
          namespace: default
          name: my_role
        rules:
        - apiGroups: [""]
          resources: ["pods"]
          verbs: ["get", "watch", "list"]
        - apiGroups: ["apps", "extensions"]
          resources: ["daemonsets", "deployments"]
          verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
        ```
        {: codeblock}

        <table>
        <caption>了解此 YAML 的组成部分</caption>
          <thead>
            <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解此 YAML 的组成部分</th>
          </thead>
          <tbody>
            <tr>
              <td><code>kind</code></td>
              <td>使用 `Role` 来授予对单个名称空间内资源的访问权，或使用 `ClusterRole` 授予对集群范围内资源的访问权。</td>
            </tr>
            <tr>
              <td><code>apiVersion</code></td>
              <td><ul><li>对于运行 Kubernetes 1.8 或更高版本的集群，请使用 `rbac.authorization.k8s.io/v1`。</li><li>对于更早的版本，请使用 `apiVersion: rbac.authorization.k8s.io/v1beta1`。</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/namespace</code></td>
              <td><ul><li>对于 `Role` 类型：指定授予其访问权的 Kubernetes 名称空间。</li><li>如果要创建在集群级别应用的 `ClusterRole`，请不要使用 `namespace` 字段。</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/name</code></td>
              <td>对角色命名，并在稍后绑定角色时使用该名称。</td>
            </tr>
            <tr>
              <td><code>rules/apiGroups</code></td>
              <td><ul><li>指定您希望用户能够与之进行交互的 Kubernetes API 组，例如 `"apps"`、`"batch"` 或 `"extensions"`。</li><li>要访问 REST 路径 `api/v1` 上的核心 API 组，请将该组保留为空：`[""]`。</li><li>有关更多信息，请参阅 Kubernetes 文档中的 [API 组 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://v1-9.docs.kubernetes.io/docs/reference/api-overview/#api-groups)。</li></ul></td>
            </tr>
            <tr>
              <td><code>rules/resources</code></td>
              <td><ul><li>指定要授予其访问权的 Kubernetes 资源，例如 `"daemonsets"`、`"deployments"`、`"events"` 或 `"ingresses"`。</li><li>如果指定 `"nodes"`，那么角色类型必须为 `ClusterRole`。</li><li>有关资源列表，请参阅 Kubernetes 备忘单中的[资源类型 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/reference/kubectl/cheatsheet/) 表。</li></ul></td>
            </tr>
            <tr>
              <td><code>rules/verbs</code></td>
              <td><ul><li>指定希望用户能够执行的操作的类型，例如 `"get"`、`"list"`、`"describe"`、`"create"` 或 `"delete"`。</li><li>有关完整的动词列表，请参阅 [`kubectl` 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/reference/kubectl/overview/)。</li></ul></td>
            </tr>
          </tbody>
        </table>

    2.  在集群中创建角色。

        ```
                kubectl apply -f <path_to_yaml_file>
        ```
        {: pre}

    3.  验证角色是否已创建。

        ```
                kubectl get roles
        ```
        {: pre}

2.  将用户绑定到角色。

    1. 创建 `.yaml` 文件以将用户绑定到角色。记下用于每个主体名称的唯一 URL。

        ```
        kind: RoleBinding
        apiVersion: rbac.authorization.k8s.io/v1
        metadata:
          name: my_role_team1
          namespace: default
        subjects:
        - kind: User
          name: https://iam.ng.bluemix.net/kubernetes#user1@example.com
          apiGroup: rbac.authorization.k8s.io
        - kind: User
          name: https://iam.ng.bluemix.net/kubernetes#user2@example.com
          apiGroup: rbac.authorization.k8s.io
        roleRef:
          kind: Role
          name: custom-rbac-test
          apiGroup: rbac.authorization.k8s.io
        ```
        {: codeblock}

        <table>
        <caption>了解此 YAML 的组成部分</caption>
          <thead>
            <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解此 YAML 的组成部分</th>
          </thead>
          <tbody>
            <tr>
              <td><code>kind</code></td>
              <td>对于以下两种类型的角色 `.yaml` 文件，将 `kind` 指定为 `RoleBinding`：名称空间 `Role` 和集群范围的 `ClusterRole`。</td>
            </tr>
            <tr>
              <td><code>apiVersion</code></td>
              <td><ul><li>对于运行 Kubernetes 1.8 或更高版本的集群，请使用 `rbac.authorization.k8s.io/v1`。</li><li>对于更早的版本，请使用 `apiVersion: rbac.authorization.k8s.io/v1beta1`。</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/namespace</code></td>
              <td><ul><li>对于 `Role` 类型：指定授予其访问权的 Kubernetes 名称空间。</li><li>如果要创建在集群级别应用的 `ClusterRole`，请不要使用 `namespace` 字段。</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/name</code></td>
              <td>对角色绑定命名。</td>
            </tr>
            <tr>
              <td><code>subjects/kind</code></td>
              <td>将 kind 指定为 `User`。</td>
            </tr>
            <tr>
              <td><code>subjects/name</code></td>
              <td><ul><li>将用户的电子邮件地址附加到以下 URL：`https://iam.ng.bluemix.net/kubernetes#`。</li><li>例如，`https://iam.ng.bluemix.net/kubernetes#user1@example.com`</li></ul></td>
            </tr>
            <tr>
              <td><code>subjects/apiGroup</code></td>
              <td>使用 `rbac.authorization.k8s.io`。</td>
            </tr>
            <tr>
              <td><code>roleRef/kind</code></td>
              <td>在角色 `.yaml` 文件中输入与 `kind` 相同的值：`Role` 或 `ClusterRole`。</td>
            </tr>
            <tr>
              <td><code>roleRef/name</code></td>
              <td>输入角色 `.yaml` 文件的名称。</td>
            </tr>
            <tr>
              <td><code>roleRef/apiGroup</code></td>
              <td>使用 `rbac.authorization.k8s.io`。</td>
            </tr>
          </tbody>
        </table>

    2. 在集群中创建角色绑定资源。

        ```
                kubectl apply -f filepath/my_role_team1.yaml
        ```
        {: pre}

    3.  验证绑定是否已创建。

        ```
                kubectl get rolebinding
        ```
        {: pre}

现在您已经创建并绑定了定制 Kubernetes RBAC 角色，接下来该由用户进行操作。请要求用户测试根据其角色有权完成的操作，例如删除 pod。
