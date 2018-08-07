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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# 集群和工作程序节点故障诊断
{: #cs_troubleshoot_clusters}

在使用 {{site.data.keyword.containerlong}} 时，请考虑对集群和工作程序节点进行故障诊断的以下方法。
{: shortdesc}

如果您有更常规的问题，请尝试[集群调试](cs_troubleshoot.html)。
{: tip}

## 无法连接到 Infrastructure 帐户
{: #cs_credentials}

{: tsSymptoms}
创建新的 Kubernetes 集群时，收到以下消息。

```
我们无法连接到您的 IBM Cloud infrastructure (SoftLayer) 帐户。创建标准集群要求您有链接到 IBM Cloud Infrastructure (SoftLayer) 帐户条款的现买现付帐户，或者您已使用 {{site.data.keyword.containerlong}} CLI 设置 {{site.data.keyword.Bluemix_notm}} 基础架构 API 密钥。
```
{: screen}

{: tsCauses}
在自动帐户链接启用后创建的 {{site.data.keyword.Bluemix_notm}} 现买现付帐户已设置有对 IBM Cloud Infrastructure (SoftLayer) 产品服务组合的访问权。您可以为集群购买基础架构资源，而无需额外配置。


具有其他 {{site.data.keyword.Bluemix_notm}} 帐户类型的用户或具有未链接到其 {{site.data.keyword.Bluemix_notm}} 帐户的现有 IBM Cloud Infrastructure (SoftLayer) 帐户的用户必须对其帐户进行配置才能创建标准集群。


{: tsResolve}
配置帐户以访问 IBM Cloud Infrastructure (SoftLayer) 产品服务组合取决于您拥有的帐户类型。请查看下表以查找每种帐户类型的可用选项。

|帐户类型|描述|用于创建标准集群的可用选项|
|------------|-----------|----------------------------------------------|
|Lite 帐户|Lite 帐户无法供应集群。|[将 Lite 帐户升级到 {{site.data.keyword.Bluemix_notm}} 现买现付帐户](/docs/account/index.html#paygo)，该帐户设置为具有对 IBM Cloud infrastructure (SoftLayer) 产品服务组合的访问权。|
|较旧的现买现付帐户|在自动帐户链接可用之前创建的现买现付帐户没有对 IBM Cloud infrastructure (SoftLayer) 产品服务组合的访问权。<p>如果您有现有的 IBM Cloud infrastructure (SoftLayer) 帐户，那么无法将此帐户链接到较旧的现买现付帐户。</p>|<strong>选项 1：</strong>[创建新的现买现付帐户](/docs/account/index.html#paygo)，该帐户设置为具有对 IBM Cloud Infrastructure (SoftLayer) 产品服务组合的访问权。选择此选项时，您有两个单独的 {{site.data.keyword.Bluemix_notm}} 帐户和帐单。<p>要继续使用旧的现买现付帐户，可以使用新的现买现付帐户生成 API 密钥，以用于访问 IBM Cloud Infrastructure (SoftLayer) 产品服务组合。然后，必须[为旧的现买现付帐户设置 IBM Cloud Infrastructure (SoftLayer) API 密钥](cs_cli_reference.html#cs_credentials_set)。</p><p><strong>选项 2：</strong>如果您已经拥有要使用的现有 IBM Cloud Infrastructure (SoftLayer) 帐户，那么可以在 {{site.data.keyword.Bluemix_notm}} 帐户中[设置凭证](cs_cli_reference.html#cs_credentials_set)。</p><p>**注：**手动链接到 IBM Cloud Infrastructure (SoftLayer) 帐户时，凭证用于 {{site.data.keyword.Bluemix_notm}} 帐户中每个特定于 IBM Cloud Infrastructure (SoftLayer) 的操作。您必须确保设置的 API 密钥具有[足够的基础架构许可权](cs_users.html#infra_access)，以便用户可以创建和使用集群。</p>|
|预订帐户|预订帐户未设置为具有对 IBM Cloud infrastructure (SoftLayer) 产品服务组合的访问权。|<strong>选项 1：</strong>[创建新的现买现付帐户](/docs/account/index.html#paygo)，该帐户设置为具有对 IBM Cloud Infrastructure (SoftLayer) 产品服务组合的访问权。选择此选项时，您有两个单独的 {{site.data.keyword.Bluemix_notm}} 帐户和帐单。<p>如果要继续使用预订帐户，那么可以使用新的现买现付帐户在 IBM Cloud Infrastructure (SoftLayer) 中生成 API 密钥。然后，必须手动[为预订帐户设置 IBM Cloud Infrastructure (SoftLayer) API 密钥](cs_cli_reference.html#cs_credentials_set)。请记住，IBM Cloud infrastructure (SoftLayer) 资源将通过新的现买现付帐户进行计费。</p><p><strong>选项 2：</strong>如果您已经拥有要使用的现有 IBM Cloud Infrastructure (SoftLayer) 帐户，那么可以为 {{site.data.keyword.Bluemix_notm}} 帐户手动[设置 IBM Cloud Infrastructure (SoftLayer) 凭证](cs_cli_reference.html#cs_credentials_set)。<p>**注：**手动链接到 IBM Cloud Infrastructure (SoftLayer) 帐户时，凭证用于 {{site.data.keyword.Bluemix_notm}} 帐户中每个特定于 IBM Cloud Infrastructure (SoftLayer) 的操作。您必须确保设置的 API 密钥具有[足够的基础架构许可权](cs_users.html#infra_access)，以便用户可以创建和使用集群。</p>|
|IBM Cloud infrastructure (SoftLayer) 帐户，无 {{site.data.keyword.Bluemix_notm}} 帐户|要创建标准集群，您必须具有 {{site.data.keyword.Bluemix_notm}} 帐户。|<p>[创建现买现付帐户](/docs/account/index.html#paygo)，该帐户设置为具有对 IBM Cloud infrastructure (SoftLayer) 产品服务组合的访问权。选择此选项时，将为您创建 IBM Cloud Infrastructure (SoftLayer) 帐户。您有两个独立的 IBM Cloud infrastructure (SoftLayer) 帐户，两者单独进行计费。</p>|
{: caption="按帐户类型列出的标准集群创建选项" caption-side="top"}


<br />


## 防火墙阻止 CLI 命令运行
{: #ts_firewall_clis}

{: tsSymptoms}
从 CLI 运行 `bx`、`kubectl` 或 `calicoctl` 命令时，它们会失败。

{: tsCauses}
您可能具有企业网络策略，这些策略灰阻止通过代理或防火墙从本地系统访问公共端点。

{: tsResolve}
[允许 TCP 访问以使 CLI 命令能够运作](cs_firewall.html#firewall)。此任务需要[管理员访问策略](cs_users.html#access_policies)。验证您当前的[访问策略](cs_users.html#infra_access)。


## 防火墙阻止集群连接到资源
{: #cs_firewall}

{: tsSymptoms}
当工作程序节点无法连接时，您可能会看到各种不同的症状。当 kubectl 代理失败时，或者您尝试访问集群中的某个服务并且连接失败时，您可能会看到以下消息之一。

  ```
  Connection refused
  ```
  {: screen}

  ```
  Connection timed out
  ```
  {: screen}

  ```
  Unable to connect to the server: net/http: TLS handshake timeout
  ```
  {: screen}

如果运行 kubectl exec、attach 或 log，那么可能会看到以下消息。

  ```
  Error from server: error dialing backend: dial tcp XXX.XXX.XXX:10250: getsockopt: connection timed out
  ```
  {: screen}

如果 kubectl 代理成功，但仪表板不可用，那么您可能会看到以下消息。

  ```
  timeout on 172.xxx.xxx.xxx
  ```
  {: screen}



{: tsCauses}
您可能已在 IBM Cloud Infrastructure (SoftLayer) 帐户中设置了其他防火墙或定制了现有防火墙设置。{{site.data.keyword.containershort_notm}} 需要打开特定 IP 地址和端口，以允许工作程序节点与 Kubernetes 主节点之间进行通信。另一个原因可能是工作程序节点陷入重新装入循环。

{: tsResolve}
[允许集群访问基础架构资源和其他服务](cs_firewall.html#firewall_outbound)。此任务需要[管理员访问策略](cs_users.html#access_policies)。验证您当前的[访问策略](cs_users.html#infra_access)。

<br />



## 使用 SSH 访问工作程序节点失败
{: #cs_ssh_worker}

{: tsSymptoms}
使用 SSH 连接无法访问工作程序节点。

{: tsCauses}
工作程序节点上禁用了通过密码进行的 SSH。

{: tsResolve}
将 [DaemonSets ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) 用于必须在每个节点上运行的任何操作，或者将作业用于必须执行的任何一次性操作。

<br />


## `kubectl exec` 和 `kubectl logs` 不起作用
{: #exec_logs_fail}

{: tsSymptoms}
如果运行 `kubectl exec` 或 `kubectl logs`，会看到以下消息。

  ```
  <workerIP>:10250: getsockopt: connection timed out
  ```
  {: screen}

{: tsCauses}
主节点与工作程序节点之间的 OpenVPN 连接工作不正常。

{: tsResolve}
1. 为 IBM Cloud Infrastructure (SoftLayer) 帐户启用 [VLAN 生成](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning)。
2. 重新启动 OpenVPN 客户机 pod。
  ```
  kubectl delete pod -n kube-system -l app=vpn
  ```
  {: pre}
3. 如果仍看到相同的错误消息，说明 VPN pod 所在的工作程序节点可能运行状况欠佳。要重新启动 VPN pod 并将其重新安排到其他工作程序节点，请[对工作程序节点执行 cordon、drain 和 reboot](cs_cli_reference.html#cs_worker_reboot)（即，对 VPN pod 所在的工作程序节点执行这些命令）。

<br />


## 将服务绑定到集群导致同名错误
{: #cs_duplicate_services}

{: tsSymptoms}
运行 `bx cs cluster-service-bind <cluster_name> <namespace> <service_instance_name>` 时，会看到以下消息。

```
Multiple services with the same name were found.
Run 'bx service list' to view available Bluemix service instances...
```
{: screen}

{: tsCauses}
多个服务实例可能在不同区域中具有相同名称。

{: tsResolve}
在 `bx cs cluster-service-bind` 命令中，请使用服务 GUID，而不要使用服务实例名称。

1. [登录到包含要绑定的服务实例的区域](cs_regions.html#bluemix_regions)。

2. 获取服务实例的 GUID。
  ```
  bx service show <service_instance_name> --guid
  ```
  {: pre}

  输出：
  ```
  Invoking 'cf service <service_instance_name> --guid'...
  <service_instance_GUID>
  ```
  {: screen}
3. 再次将服务绑定到集群。
  ```
  bx cs cluster-service-bind <cluster_name> <namespace> <service_instance_GUID>
  ```
  {: pre}

<br />


## 将服务绑定到集群导致找不到服务错误
{: #cs_not_found_services}

{: tsSymptoms}
运行 `bx cs cluster-service-bind <cluster_name> <namespace> <service_instance_name>` 时，会看到以下消息。

```
Binding service to a namespace...
FAILED

The specified IBM Cloud service could not be found. If you just created the service, wait a little and then try to bind it again. To view available IBM Cloud service instances, run 'bx service list'. (E0023)
```
{: screen}

{: tsCauses}
要将服务绑定到集群，您必须具有在其中供应服务实例的空间的 Cloud Foundry 开发者用户角色。此外，您必须具有对 {{site.data.keyword.containerlong}} 的 IAM 编辑者访问权。要访问服务实例，您必须登录到在其中供应该服务实例的空间。 

{: tsResolve}

**以用户身份执行以下操作：**

1. 登录到 {{site.data.keyword.Bluemix_notm}}。 
   ```
    bx login
    ```
   {: pre}
   
2. 将在其中供应服务实例的组织和空间设定为目标。 
   ```
   bx target -o <org> -s <space>
   ```
   {: pre}
   
3. 通过列出服务实例来验证您是否位于正确的空间中。 
   ```
    bx service list
    ```
   {: pre}
   
4. 重试绑定服务。如果遇到相同的错误，请联系帐户管理员，并验证您是否具有足够的许可权来绑定服务（请参阅以下帐户管理员步骤）。 

**以帐户管理员身份执行以下操作：**

1. 验证遇到此问题的用户是否具有[对 {{site.data.keyword.containerlong}} 的编辑者许可权](/docs/iam/mngiam.html#editing-existing-access)。 

2. 验证遇到此问题的用户是否具有对在其中供应该服务的[空间的 Cloud Foundry 开发者角色](/docs/iam/mngcf.html#updating-cloud-foundry-access)。 

3. 如果存在正确的许可权，请尝试分配其他许可权，然后重新分配所需的许可权。 

4. 稍等几分钟，然后让用户重试绑定服务。 

5. 如果这无法解决此问题，说明 IAM 许可权不同步，您无法自行解决此问题。请通过开具支持凭单，[与 IBM 支持联系](/docs/get-support/howtogetsupport.html#getting-customer-support)。确保提供集群标识、用户标识和服务实例标识。 
   1. 检索集群标识。
      ```
        bx cs clusters
        ```
      {: pre}
      
   2. 检索服务实例标识。
      ```
      bx service show <service_name> --guid
      ```
      {: pre}


<br />



## 工作程序节点更新或重新装入后，出现重复的节点和 pod
{: #cs_duplicate_nodes}

{: tsSymptoms}
运行 `kubectl get nodes` 时，看到状态为 **NotReady** 的重复工作程序节点。状态为 **NotReady** 的工作程序节点具有公共 IP 地址，而状态为 **Ready** 的工作程序节点具有专用 IP 地址。

{: tsCauses}
较旧的集群按集群的公共 IP 地址列出工作程序节点。现在，工作程序节点按集群的专用 IP 地址列出。当您重新装入或更新节点时，IP 地址将更改，但对公共 IP 地址的引用将保持不变。

{: tsResolve}
由于存在这些重复项，因此服务不会中断，但您可以从 API 服务器中除去旧的工作程序节点引用。

  ```
  kubectl delete node <node_name1> <node_name2>
  ```
  {: pre}

<br />


## 工作程序节点更新或重新装入后，应用程序收到 RBAC DENY 错误
{: #cs_rbac_deny}

{: tsSymptoms}
更新到 Kubernetes V1.7 后，应用程序收到 `RBAC DENY` 错误。

{: tsCauses}
从 [Kubernetes V1.7](cs_versions.html#cs_v17) 开始，在 `default` 名称空间中运行的应用程序不再具有对 Kubernetes API 的集群管理员特权，以提高安全性。

如果应用程序在 `default` 名称空间中运行，使用 `default ServiceAccount`，并且访问 Kubernetes API，那么此 Kubernetes 更改会对其产生影响。有关更多信息，请参阅 [Kubernetes 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/admin/authorization/rbac/#upgrading-from-15)。

{: tsResolve}
开始之前，请[设定 CLI 的目标](cs_cli_install.html#cs_cli_configure)为集群。

1.  **临时操作**：更新应用程序 RBAC 策略时，您可能希望临时还原到 `default` 名称空间中 `default ServiceAccount` 的先前 `ClusterRoleBinding`。

    1.  复制以下 `.yaml` 文件。

        ```yaml
        kind: ClusterRoleBinding
        apiVersion: rbac.authorization.k8s.io/v1beta1
        metadata:
         name: admin-binding-nonResourceURLSs-default
        subjects:
          - kind: ServiceAccount
      name: default
      namespace: default
  roleRef:
   kind: ClusterRole
   name: admin-role-nonResourceURLSs
   apiGroup: rbac.authorization.k8s.io
        ---
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
 name: admin-binding-resourceURLSs-default
subjects:
          - kind: ServiceAccount
            name: default
            namespace: default
        roleRef:
         kind: ClusterRole
         name: admin-role-resourceURLSs
         apiGroup: rbac.authorization.k8s.io
        ```

    2.  将 `.yaml` 文件应用于集群。

        ```
                kubectl apply -f FILENAME
        ```
        {: pre}

2.  [创建 RBAC 授权资源 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview) 以更新 `ClusterRoleBinding` 管理访问权。

3.  如果创建了临时集群角色绑定，请将其除去。

<br />


## 访问新工作程序节点上的 pod 失败，并返回超时错误
{: #cs_nodes_duplicate_ip}

{: tsSymptoms}
您删除了集群中的工作程序节点，然后添加了工作程序节点。部署 pod 或 Kubernetes 服务时，资源无法访问新创建的工作程序节点，并且连接超时。

{: tsCauses}
如果从集群中删除工作程序节点，然后添加工作程序节点，那么可能会将已删除工作程序节点的专用 IP 地址分配给新工作程序节点。Calico 使用此专用 IP 地址作为标记，并继续尝试访问已删除的节点。

{: tsResolve}
手动更新专用 IP 地址的引用以指向正确的节点。

1.  确认您是否有两个工作程序节点使用相同的**专用 IP** 地址。记下已删除的工作程序的**专用 IP** 和**标识**。

  ```
  bx cs workers <CLUSTER_NAME>
  ```
  {: pre}

  ```
  ID                                                 Public IP       Private IP       Machine Type   State     Status   Location   Version
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w1   169.xx.xxx.xxx  10.xxx.xx.xxx    b2c.4x16       normal    Ready    dal10      1.9.7
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w2   169.xx.xxx.xxx  10.xxx.xx.xxx    b2c.4x16       deleted    -       dal10      1.9.7
  ```
  {: screen}

2.  安装 [Calico CLI](cs_network_policy.html#adding_network_policies)。
3.  列出 Calico 中的可用工作程序节点。将 <path_to_file> 替换为 Calico 配置文件的本地路径。

  ```
  calicoctl get nodes --config=filepath/calicoctl.cfg
  ```
  {: pre}

  ```
  NAME
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w1
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w2
  ```
  {: screen}

4.  删除 Calico 中的重复工作程序节点。将 NODE_ID 替换为工作程序节点标识。

  ```
  calicoctl delete node NODE_ID --config=<path_to_file>/calicoctl.cfg
  ```
  {: pre}

5.  重新引导未删除的工作程序节点。

  ```
  bx cs worker-reboot CLUSTER_ID NODE_ID
  ```
  {: pre}


已删除的节点不会再在 Calico 中列出。

<br />


## 集群保持暂挂状态
{: #cs_cluster_pending}

{: tsSymptoms}
部署集群时，集群保持暂挂状态，而不启动。

{: tsCauses}
如果刚刚创建了集群，那么工作程序节点可能仍在配置中。如果您已经等待一段时间，那么可能是 VLAN 无效。

{: tsResolve}

可以尝试下列其中一个解决方案：
  - 通过运行 `bx cs cluster` 来检查集群的阶段状态。然后，通过运行 `bx cs workers <cluster_name>` 来检查以确保工作程序节点已部署。
  - 检查以确定 VLAN 是否有效。要使 VLAN 有效，必须将 VLAN 与可使用本地磁盘存储来托管工作程序的基础架构相关联。可以通过运行 `bx cs vlans <location>` 来[列出 VLAN](/docs/containers/cs_cli_reference.html#cs_vlans)，如果 VLAN 未显示在列表中，说明该 VLAN 无效。请选择其他 VLAN。

<br />


## Pod 保持暂挂状态
{: #cs_pods_pending}

{: tsSymptoms}
运行 `kubectl get pods` 时，可能看到 pod 保持**暂挂**状态。

{: tsCauses}
如果刚刚创建了 Kubernetes 集群，那么工作程序节点可能仍在配置中。如果这是现有集群，那么可能是集群中没有足够的容量来部署 pod。

{: tsResolve}
此任务需要[管理员访问策略](cs_users.html#access_policies)。验证您当前的[访问策略](cs_users.html#infra_access)。

如果刚创建了 Kubernetes 集群，请运行以下命令并等待工作程序节点初始化。

```
kubectl get nodes
```
{: pre}

如果这是现有集群，请检查集群容量。

1.  使用缺省端口号设置代理。

  ```
  kubectl proxy
  ```
   {: pre}

2.  打开 Kubernetes 仪表板。

  ```
  http://localhost:8001/ui
  ```
  {: pre}

3.  检查集群中是否有足够的容量来部署 pod。

4.  如果集群中没有足够的容量，请向集群另外添加一个工作程序节点。

    ```
      bx cs worker-add <cluster_name_or_ID> 1
  ```
    {: pre}

5.  如果在完全部署工作程序节点后，pod 仍然保持 **pending** 状态，请查看 [Kubernetes 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-pod-replication-controller/#my-pod-stays-pending) 以进一步对 pod 的暂挂状态进行故障诊断。

<br />




## 容器不启动
{: #containers_do_not_start}

{: tsSymptoms}
pod 会成功部署到集群，但容器不启动。

{: tsCauses}
当达到注册表限额时，容器可能不会启动。

{: tsResolve}
[在 {{site.data.keyword.registryshort_notm}} 中释放存储器。](../services/Registry/registry_quota.html#registry_quota_freeup)

<br />



## 无法使用已更新的配置值安装 Helm 图表
{: #cs_helm_install}

{: tsSymptoms}
尝试通过运行 `helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>` 来安装更新的 Helm 图表时，将获得 `Error: failed to download "ibm/<chart_name>"` 错误消息。

{: tsCauses}
Helm 实例中 {{site.data.keyword.Bluemix_notm}} 存储库的 URL 可能不正确。

{: tsResolve}
要对 Helm 图表进行故障诊断，请执行以下操作：

1. 列出 Helm 实例中当前可用的存储库。

    ```
        helm repo list
    ```
    {: pre}

2. 在输出中，验证 {{site.data.keyword.Bluemix_notm}} 存储库 `ibm` 的 URL 是否为 `https://registry.bluemix.net/helm/ibm`。

    ```
        NAME    URL
    stable  https://kubernetes-charts.storage.googleapis.com
    local   http://127.0.0.1:8888/charts
    ibm     https://registry.bluemix.net/helm/ibm
    ```
    {: screen}

    * 如果该 URL 不正确，请执行以下操作：

        1. 除去 {{site.data.keyword.Bluemix_notm}} 存储库。

            ```
                        helm repo remove ibm
            ```
            {: pre}

        2. 重新添加 {{site.data.keyword.Bluemix_notm}} 存储库。

            ```
                        helm repo add ibm  https://registry.bluemix.net/helm/ibm
            ```
            {: pre}

    * 如果该 URL 正确，请从相应存储库中获取最新更新。

        ```
                helm repo update
        ```
        {: pre}

3. 使用更新安装 Helm 图表。

    ```
        helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>
    ```
    {: pre}

<br />


## 获取帮助和支持
{: #ts_getting_help}

集群仍然有问题吗？
{: shortdesc}

-   要查看 {{site.data.keyword.Bluemix_notm}} 是否可用，请[检查 {{site.data.keyword.Bluemix_notm}} 状态页面 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/bluemix/support/#status)。
-   在 [{{site.data.keyword.containershort_notm}} Slack ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://ibm-container-service.slack.com) 中发布问题。

如果未将 IBM 标识用于 {{site.data.keyword.Bluemix_notm}} 帐户，请针对此 Slack [请求邀请](https://bxcs-slack-invite.mybluemix.net/)。
    {: tip}
-   请复查论坛，以查看是否有其他用户遇到相同的问题。使用论坛进行提问时，请使用适当的标记来标注您的问题，以方便 {{site.data.keyword.Bluemix_notm}} 开发团队识别。

    -   如果您有关于使用 {{site.data.keyword.containershort_notm}} 开发或部署集群或应用程序的技术问题，请在 [Stack Overflow ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) 上发布您的问题，并使用 `ibm-cloud`、`kubernetes` 和 `containers` 标记您的问题。
    -   有关服务的问题和入门指示信息，请使用 [IBM developerWorks dW Answers ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) 论坛。请加上 `ibm-cloud` 和 `containers` 标记。
    有关使用论坛的更多详细信息，请参阅[获取帮助](/docs/get-support/howtogetsupport.html#using-avatar)。

-   通过开具凭单，与 IBM 支持联系。要了解有关开具 IBM 支持凭单或有关支持级别和凭单严重性的信息，请参阅[联系支持人员](/docs/get-support/howtogetsupport.html#getting-customer-support)。

{: tip}
报告问题时，请包含集群标识。要获取集群标识，请运行 `bx cs clusters`。

