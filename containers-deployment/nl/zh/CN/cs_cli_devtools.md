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





# 用于管理集群的 CLI 参考
{: #cs_cli_reference}

请参阅以下命令以在 {{site.data.keyword.Bluemix_notm}} 上创建和管理集群。
{:shortdesc}

## bx cs 命令
{: #cs_commands}

**提示：**要查看 {{site.data.keyword.containershort_notm}} 插件的版本，请运行以下命令。

```
    bx plugin list
    ```
{: pre}



<table summary="API 命令">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>API 命令</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs api-key-info](#cs_api_key_info)</td>
    <td>[bx cs api-key-reset](#cs_api_key_reset)</td>
    <td>[bx cs apiserver-config-get](#cs_apiserver_config_get)</td>
    <td>[bx cs apiserver-config-set](#cs_apiserver_config_set)</td>
  </tr>
  <tr>
    <td>[bx cs apiserver-config-unset](#cs_apiserver_config_unset)</td>
    <td>[bx cs apiserver-refresh](#cs_apiserver_refresh)</td>
    <td></td>
    <td></td>
 </tr>
</tbody>
</table>

<br>

<table summary="CLI 插件用法命令">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>CLI 插件用法命令</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs help](#cs_help)</td>
    <td>[bx cs init](#cs_init)</td>
    <td>[bx cs messages](#cs_messages)</td>
    <td></td>
  </tr>
</tbody>
</table>

<br>

<table summary="集群命令：管理">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>集群命令：管理</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs cluster-config](#cs_cluster_config)</td>
    <td>[bx cs cluster-create](#cs_cluster_create)</td>
    <td>[bx cs cluster-feature-enable](#cs_cluster_feature_enable)</td>
    <td>[bx cs cluster-get](#cs_cluster_get)</td>
  </tr>
  <tr>
    <td>[bx cs cluster-rm](#cs_cluster_rm)</td>
    <td>[bx cs cluster-update](#cs_cluster_update)</td>
    <td>[bx cs clusters](#cs_clusters)</td>
    <td>[bx cs kube-versions](#cs_kube_versions)</td>
  </tr>
</tbody>
</table>

<br>

<table summary="集群命令：服务和集成">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>集群命令：服务和集成</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs cluster-service-bind](#cs_cluster_service_bind)</td>
    <td>[bx cs cluster-service-unbind](#cs_cluster_service_unbind)</td>
    <td>[bx cs cluster-services](#cs_cluster_services)</td>
    <td>[bx cs webhook-create](#cs_webhook_create)</td>
  </tr>
</tbody>
</table>

</br>

<table summary="集群命令：子网">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>集群命令：子网</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs cluster-subnet-add](#cs_cluster_subnet_add)</td>
    <td>[bx cs cluster-subnet-create](#cs_cluster_subnet_create)</td>
    <td>[bx cs cluster-user-subnet-add](#cs_cluster_user_subnet_add)</td>
    <td>[bx cs cluster-user-subnet-rm](#cs_cluster_user_subnet_rm)</td>
  </tr>
  <tr>
    <td>[bx cs subnets](#cs_subnets)</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</tbody>
</table>

</br>

<table summary="基础架构命令">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>基础架构命令</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs credentials-set](#cs_credentials_set)</td>
    <td>[bx cs credentials-unset](#cs_credentials_unset)</td>
    <td>[bx cs machine-types](#cs_machine_types)</td>
    <td>[bx cs vlans](#cs_vlans)</td>
  </tr>
</tbody>
</table>

</br>

<table summary="Ingress 应用程序负载均衡器 (ALB) 命令">
<col width = 25%>
<col width = 25%>
<col width = 25%>
  <thead>
    <tr>
      <th colspan=4>Ingress 应用程序负载均衡器 (ALB) 命令</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>[bx cs alb-cert-deploy](#cs_alb_cert_deploy)</td>
      <td>[bx cs alb-cert-get](#cs_alb_cert_get)</td>
      <td>[bx cs alb-cert-rm](#cs_alb_cert_rm)</td>
      <td>[bx cs alb-certs](#cs_alb_certs)</td>
    </tr>
    <tr>
      <td>[bx cs alb-configure](#cs_alb_configure)</td>
      <td>[bx cs alb-get](#cs_alb_get)</td>
      <td>[bx cs alb-types](#cs_alb_types)</td>
      <td>[bx cs albs](#cs_albs)</td>
    </tr>
  </tbody>
</table>

</br>

<table summary="日志记录命令">
<col width = 25%>
<col width = 25%>
<col width = 25%>
  <thead>
    <tr>
      <th colspan=4>日志记录命令</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>[bx cs logging-config-create](#cs_logging_create)</td>
      <td>[bx cs logging-config-get](#cs_logging_get)</td>
      <td>[bx cs logging-config-refresh](#cs_logging_refresh)</td>
      <td>[bx cs logging-config-rm](#cs_logging_rm)</td>
    </tr>
    <tr>
      <td>[bx cs logging-config-update](#cs_logging_update)</td>
      <td>[bx cs logging-filter-create](#cs_log_filter_create)</td>
      <td>[bx cs logging-filter-update](#cs_log_filter_update)</td>
      <td>[bx cs logging-filter-get](#cs_log_filter_view)</td>
    </tr>
    <tr>
      <td>[bx cs logging-filter-rm](#cs_log_filter_delete)</td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>

</br>

<table summary="区域命令">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>区域命令</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs locations](#cs_datacenters)</td>
    <td>[bx cs region](#cs_region)</td>
    <td>[bx cs region-set](#cs_region-set)</td>
    <td>[bx cs regions](#cs_regions)</td>
  </tr>
</tbody>
</table>

</br>

<table summary="工作程序节点命令">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>工作程序节点命令</th>
 </thead>
 <tbody>
    <tr>
      <td>[bx cs worker-add](#cs_worker_add)</td>
      <td>[bx cs worker-get](#cs_worker_get)</td>
      <td>[bx cs worker-reboot](#cs_worker_reboot)</td>
      <td>[bx cs worker-reload](#cs_worker_reload)</td></staging>
    </tr>
    <tr>
      <td>[bx cs worker-rm](#cs_worker_rm)</td>
      <td>[bx cs worker-update](#cs_worker_update)</td>
      <td>[bx cs workers](#cs_workers)</td>
      <td></td>
    </tr>
  </tbody>
</table>

## API 命令
{: #api_commands}

### bx cs api-key-info CLUSTER
{: #cs_api_key_info}

查看 {{site.data.keyword.containershort_notm}} 区域中 IAM API 密钥所有者的姓名和电子邮件地址。

执行需要 {{site.data.keyword.containershort_notm}} 管理员访问策略的第一个操作时，会自动针对区域设置 Identity and Access Management (IAM) API 密钥。例如，某个管理用户在 `us-south` 区域中创建了第一个集群。通过执行此操作，此用户的 IAM API 密钥将存储在此区域的帐户中。API 密钥用于订购 IBM Cloud Infrastructure (SoftLayer) 中的资源，例如新的工作程序节点或 VLAN。

其他用户在此区域中执行需要与 IBM Cloud Infrastructure (SoftLayer) 产品服务组合进行交互的操作（例如，创建新集群或重新装入工作程序节点）时，将使用存储的 API 密钥来确定是否存在执行该操作的足够许可权。要确保可以成功执行集群中与基础架构相关的操作，请为 {{site.data.keyword.containershort_notm}} 管理用户分配**超级用户**基础架构访问策略。有关更多信息，请参阅[管理用户访问权](cs_users.html#infra_access)。

如果发现需要更新为区域存储的 API 密钥，那么可以通过运行 [bx cs api-key-reset](#cs_api_key_reset) 命令来执行此操作。此命令需要 {{site.data.keyword.containershort_notm}} 管理员访问策略，并在帐户中存储执行此命令的用户的 API 密钥。

**提示：**如果使用 [bx cs credentials-set](#cs_credentials_set) 命令手动设置了 IBM Cloud Infrastructure (SoftLayer) 凭证，那么可能不会使用在此命令中返回的 API 密钥。

<strong>命令选项</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>集群的名称或标识。此值是必需的。</dd>
   </dl>

**示例**：

  ```
  bx cs api-key-info my_cluster
  ```
  {: pre}


### bx cs api-key-reset
{: #cs_api_key_reset}

替换 {{site.data.keyword.containershort_notm}} 区域中的当前 IAM API 密钥。

此命令需要 {{site.data.keyword.containershort_notm}} 管理员访问策略，并在帐户中存储执行此命令的用户的 API 密钥。要从 IBM Cloud Infrastructure (SoftLayer) 产品服务组合订购基础架构，需要 IAM API 密钥。存储后，该 API 密钥会用于区域中需要基础架构许可权（与执行此命令的用户无关）的每个操作。有关 IAM API 密钥的工作方式的更多信息，请参阅 [`bx cs api-key-info` 命令](#cs_api_key_info)。

**重要信息**：使用此命令之前，请确保执行此命令的用户具有必需的 [{{site.data.keyword.containershort_notm}} 和 IBM Cloud Infrastructure (SoftLayer) 许可权](cs_users.html#users)。

**示例**：

  ```
  bx cs api-key-reset
  ```
  {: pre}


### bx cs apiserver-config-get
{: #cs_apiserver_config_get}

获取有关集群的 Kubernetes API 服务器配置选项的信息。对于要获取其信息的配置选项，此命令必须与下列其中一个子命令组合在一起。

#### bx cs apiserver-config-get audit-webhook CLUSTER
{: #cs_apiserver_api_webhook_get}

查看要向其发送 API 服务器审计日志的远程日志记录服务的 URL。URL 是在您为 API 服务器配置创建 Webhook 后端时指定的。

<strong>命令选项</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>集群的名称或标识。此值是必需的。</dd>
   </dl>

**示例**：

  ```
  bx cs apiserver-config-get audit-webhook my_cluster
  ```
  {: pre}

### bx cs apiserver-config-set
{: #cs_apiserver_config_set}

为集群的 Kubernetes API 服务器配置设置选项。对于要设置的配置选项，此命令必须与下列其中一个子命令组合在一起。

#### bx cs apiserver-config-set audit-webhook CLUSTER [--remoteServer SERVER_URL_OR_IP][--caCert CA_CERT_PATH] [--clientCert CLIENT_CERT_PATH][--clientKey CLIENT_KEY_PATH]
{: #cs_apiserver_api_webhook_set}

设置 API 服务器配置的 Webhook 后端。Webhook 后端将 API 服务器审计日志转发到远程服务器。根据您在此命令标志中提供的信息来创建 Webhook 配置。如果未在标志中提供任何信息，那么将使用缺省的 Webhook 配置。

<strong>命令选项</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>集群的名称或标识。此值是必需的。</dd>

   <dt><code>--remoteServer <em>SERVER_URL</em></code></dt>
   <dd>要向其发送审计日志的远程日志记录服务的 URL 或 IP 地址。如果提供不安全的服务器 URL，那么将忽略任何证书。此值是可选的。</dd>

   <dt><code>--caCert <em>CA_CERT_PATH</em></code></dt>
   <dd>用于验证远程日志记录服务的 CA 证书的文件路径。此值是可选的。</dd>

   <dt><code>--clientCert <em>CLIENT_CERT_PATH</em></code></dt>
   <dd>用于针对远程日志记录服务进行认证的客户机证书的文件路径。此值是可选的。</dd>

   <dt><code>--clientKey <em> CLIENT_KEY_PATH</em></code></dt>
   <dd>用于连接到远程日志记录服务的相应客户机密钥的文件路径。此值是可选的。</dd>
   </dl>

**示例**：

  ```
  bx cs apiserver-config-set audit-webhook my_cluster --remoteServer https://audit.example.com/audit --caCert /mnt/etc/kubernetes/apiserver-audit/ca.pem --clientCert /mnt/etc/kubernetes/apiserver-audit/cert.pem --clientKey /mnt/etc/kubernetes/apiserver-audit/key.pem
  ```
  {: pre}


### bx cs apiserver-config-unset
{: #cs_apiserver_config_unset}

禁用集群的 Kubernetes API 服务器配置的选项。对于要取消设置的配置选项，此命令必须与下列其中一个子命令组合在一起。

#### bx cs apiserver-config-unset audit-webhook CLUSTER
{: #cs_apiserver_api_webhook_unset}

禁用集群 API 服务器的 Webhook 后端配置。对 Webhook 后端进行拨号将停止将 API 服务器审计日志转发到远程服务器。

<strong>命令选项</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>集群的名称或标识。此值是必需的。</dd>
   </dl>

**示例**：

  ```
  bx cs apiserver-config-unset audit-webhook my_cluster
  ```
  {: pre}

### bx cs apiserver-refresh CLUSTER
{: #cs_apiserver_refresh}

在集群中重新启动 Kubernetes 主节点以将更改应用于 API 服务器配置。

<strong>命令选项</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>集群的名称或标识。此值是必需的。</dd>
   </dl>

**示例**：

  ```
  bx cs apiserver-refresh my_cluster
  ```
  {: pre}


<br />


## CLI 插件用法命令
{: #cli_plug-in_commands}

### bx cs help
{: #cs_help}

查看支持的命令和参数的列表。

<strong>命令选项</strong>：

   无

**示例**：

  ```
  bx cs help
  ```
  {: pre}


### bx cs init [--host HOST]
{: #cs_init}

初始化 {{site.data.keyword.containershort_notm}} 插件或指定要在其中创建或访问 Kubernetes 集群的区域。

<strong>命令选项</strong>：

   <dl>
   <dt><code>--host <em>HOST</em></code></dt>
   <dd>要使用的 {{site.data.keyword.containershort_notm}} API 端点。此值是可选的。[查看可用的 API 端点值。](cs_regions.html#container_regions)</dd>
   </dl>

**示例**：


```
          bx cs init --host https://uk-south.containers.bluemix.net
          ```
{: pre}


### bx cs messages
{: #cs_messages}

查看 IBM 标识用户的当前消息。

**示例**：

```
bx cs messages
```
{: pre}


<br />


## 集群命令：管理
{: #cluster_mgmt_commands}


### bx cs cluster-config CLUSTER [--admin][--export]
{: #cs_cluster_config}

登录后，下载 Kubernetes 配置数据和证书，以连接到集群并运行 `kubectl` 命令。这些文件会下载到 `user_home_directory/.bluemix/plugins/container-service/clusters/<cluster_name>`.

**命令选项**：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>集群的名称或标识。此值是必需的。</dd>

   <dt><code>--admin</code></dt>
   <dd>下载超级用户角色的 TLS 证书和许可权文件。您可以使用证书来自动执行集群中的任务，而无需重新认证。这些文件会下载到 `<user_home_directory>/.bluemix/plugins/container-service/clusters/<cluster_name>-admin`。此值是可选的。</dd>

   <dt><code>--export</code></dt>
   <dd>下载 Kubernetes 配置数据和证书，而不包含导出命令以外的任何消息。由于未显示任何消息，因此您可以在创建自动化脚本时使用此标志。此值是可选的。</dd>
   </dl>

**示例**：

```
bx cs cluster-config my_cluster
```
{: pre}


### bx cs cluster-create [--file FILE_LOCATION][--hardware HARDWARE] --location LOCATION --machine-type MACHINE_TYPE --name NAME [--kube-version MAJOR.MINOR.PATCH][--no-subnet] [--private-vlan PRIVATE_VLAN][--public-vlan PUBLIC_VLAN] [--workers WORKER][--disable-disk-encrypt] [--trusted]
{: #cs_cluster_create}

在组织中创建集群。对于免费集群，请指定集群名称；其他所有项都设置为缺省值。免费集群在 21 天后会被自动删除。一次只能有一个免费集群。要利用 Kubernetes 的全部功能，请创建标准集群。

<strong>命令选项</strong>

<dl>
<dt><code>--file <em>FILE_LOCATION</em></code></dt>

<dd>用于创建标准集群的 YAML 文件的路径。您可以使用 YAML 文件，而不使用此命令中提供的选项来定义集群的特征。
此值对于标准集群是可选的，且不可用于免费集群。

<p><strong>注</strong>：如果在命令中提供的选项与 YAML 文件中的参数相同，那么命令中的值将优先于 YAML 中的值。例如，您在 YAML 文件中定义了位置，并在命令中使用了 <code>--location</code> 选项，那么在该命令选项中输入的值会覆盖 YAML 文件中的相应值。

<pre class="codeblock">
<code>name: <em>&lt;cluster_name&gt;</em>
location: <em>&lt;location&gt;</em>
no-subnet: <em>&lt;no-subnet&gt;</em>
machine-type: <em>&lt;machine_type&gt;</em>
private-vlan: <em>&lt;private_VLAN&gt;</em>
public-vlan: <em>&lt;public_VLAN&gt;</em>
hardware: <em>&lt;shared_or_dedicated&gt;</em>
workerNum: <em>&lt;number_workers&gt;</em>
kube-version: <em>&lt;kube-version&gt;</em>
diskEncryption: <em>false</em>
trusted: <em>true</em>
</code></pre>


<table>
    <caption>表. 了解 YAML 文件的组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>name</em></code></td>
    <td>将 <code><em>&lt;cluster_name&gt;</em></code> 替换为集群的名称。名称必须以字母开头，可以包含字母、数字和连字符 (-)，并且不能超过 35 个字符。集群名称和部署集群的区域构成了 Ingress 子域的标准域名。为了确保 Ingress 子域在区域内是唯一的，可能会截断 Ingress 域名中的集群名称并附加随机值。</td>
    </tr>
    <tr>
    <td><code><em>location</em></code></td>
    <td>将 <code><em>&lt;location&gt;</em></code> 替换为要在其中创建集群的位置。可用的位置取决于您登录到的区域。要列出可用位置，请运行 <code>bx cs locations</code>。</td>
     </tr>
     <tr>
     <td><code><em>no-subnet</em></code></td>
     <td>缺省情况下，将在与集群关联的 VLAN 上创建公用和专用可移植子网。将 <code><em>&lt;no-subnet&gt;</em></code> 替换为 <code><em>true</em></code> 可避免为集群创建子网。您可以日后为集群[创建](#cs_cluster_subnet_create)或[添加](#cs_cluster_subnet_add)子网。</td>
      </tr>
     <tr>
     <td><code><em>machine-type</em></code></td>
     <td>将 <code><em>&lt;machine_type&gt;</em></code> 替换为要将工作程序节点部署到的机器类型。可以将工作程序节点作为虚拟机部署在共享或专用硬件上，也可以作为物理机器部署在裸机上。可用的物理和虚拟机类型随集群的部署位置而变化。有关更多信息，请参阅 `bx cs machine-type` [命令](cs_cli_reference.html#cs_machine_types)的文档。</td>
     </tr>
     <tr>
     <td><code><em>private-vlan</em></code></td>
     <td>将 <code><em>&lt;private_VLAN&gt;</em></code> 替换为要用于工作程序节点的专用 VLAN 的标识。要列出可用的 VLAN，请运行 <code>bx cs vlans <em>&lt;location&gt;</em></code> 并查找以 <code>bcr</code>（后端路由器）开头的 VLAN 路由器。</td>
     </tr>
     <tr>
     <td><code><em>public-vlan</em></code></td>
     <td>将 <code><em>&lt;public_VLAN&gt;</em></code> 替换为要用于工作程序节点的公用 VLAN 的标识。要列出可用的 VLAN，请运行 <code>bx cs vlans <em>&lt;location&gt;</em></code> 并查找以 <code>fcr</code>（前端路由器）开头的 VLAN 路由器。</td>
     </tr>
     <tr>
     <td><code><em>hardware</em></code></td>
     <td>对于虚拟机类型：工作程序节点的硬件隔离级别。如果希望可用的物理资源仅供您专用，请使用 dedicated，或者要允许物理资源与其他 IBM 客户共享，请使用 shared。缺省值为 <code>shared</code>。</td>
     </tr>
     <tr>
     <td><code><em>workerNum</em></code></td>
     <td>将 <code><em>&lt;number_workers&gt;</em></code> 替换为要部署的工作程序节点数。</td>
     </tr>
     <tr>
      <td><code><em>kube-version</em></code></td>
      <td>集群主节点的 Kubernetes 版本。此值是可选的。未指定版本时，会使用受支持 Kubernetes 版本的缺省值来创建集群。要查看可用版本，请运行 <code>bx cs kube-versions</code>。</td></tr>
      <tr>
      <td><code>diskEncryption: <em>false</em></code></td>
      <td>工作程序节点缺省情况下具有磁盘加密功能：[了解更多](cs_secure.html#worker)。要禁用加密，请包括此选项并将值设置为 <code>false</code>。</td></tr>
      <tr>
      <td><code>trusted: <em>true</em></code></td>
      <td>**仅限裸机**：启用[可信计算](cs_secure.html#trusted_compute)以验证裸机工作程序节点是否被篡改。如果在创建集群期间未启用信任，但希望日后启用，那么可以使用 `bx cs feature-enable` [命令](cs_cli_reference.html#cs_cluster_feature_enable)。启用信任后，日后无法将其禁用。</td></tr>
     </tbody></table>
    </p></dd>

<dt><code>--hardware <em>HARDWARE</em></code></dt>
<dd>工作程序节点的硬件隔离级别。如果希望可用的物理资源仅供您专用，请使用 dedicated，或者要允许物理资源与其他 IBM 客户共享，请使用 shared。缺省值为 shared。此值对于标准集群是可选的，且不可用于免费集群。</dd>

<dt><code>--location <em>LOCATION</em></code></dt>
<dd>要在其中创建集群的位置。可用的位置取决于您登录到的 {{site.data.keyword.Bluemix_notm}} 区域。请选择实际离您最近的区域，以获得最佳性能。
此值对于标准集群是必需的，对于免费集群是可选的。

<p>复查[可用位置](cs_regions.html#locations)。
</p>

<p><strong>注</strong>：选择您所在国家或地区以外的位置时，请记住，您可能需要法律授权才能将数据实际存储在国外。</p>
</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>选择机器类型。可以将工作程序节点作为虚拟机部署在共享或专用硬件上，也可以作为物理机器部署在裸机上。可用的物理和虚拟机类型随集群的部署位置而变化。有关更多信息，请参阅 `bx cs machine-types` [命令](cs_cli_reference.html#cs_machine_types)的文档。此值对于标准集群是必需的，且不可用于免费集群。</dd>

<dt><code>--name <em>NAME</em></code></dt>
<dd>集群的名称。此值是必需的。名称必须以字母开头，可以包含字母、数字和连字符 (-)，并且不能超过 35 个字符。集群名称和部署集群的区域构成了 Ingress 子域的标准域名。为了确保 Ingress 子域在区域内是唯一的，可能会截断 Ingress 域名中的集群名称并附加随机值。</dd>

<dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
<dd>集群主节点的 Kubernetes 版本。此值是可选的。未指定版本时，会使用受支持 Kubernetes 版本的缺省值来创建集群。要查看可用版本，请运行 <code>bx cs kube-versions</code>。</dd>

<dt><code>--no-subnet</code></dt>
<dd>缺省情况下，将在与集群关联的 VLAN 上创建公用和专用可移植子网。包含 <code>--no-subnets</code> 标志可避免为集群创建子网。您可以日后为集群[创建](#cs_cluster_subnet_create)或[添加](#cs_cluster_subnet_add)子网。</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>

<ul>
<li>此参数不可用于免费集群。</li>
<li>如果此标准集群是您在此位置中创建的第一个标准集群，请勿包含此标志。创建集群时，将为您创建专用 VLAN。</li>
<li>如果之前在此位置中已创建标准集群，或者之前在 IBM Cloud infrastructure (SoftLayer) 中已创建专用 VLAN，那么必须指定该专用 VLAN。



<p><strong>注：</strong>专用 VLAN 路由器始终以 <code>bcr</code>（后端路由器）开头，而公用 VLAN 路由器始终以 <code>fcr</code>（前端路由器）开头。创建集群并指定公用和专用 VLAN 时，在这些前缀之后的数字和字母组合必须匹配。</p></li>
</ul>

<p>要了解您是否已具有用于特定位置的专用 VLAN，或要找到现有专用 VLAN 的名称，请运行 <code>bx cs vlans <em>&lt;location&gt;</em></code>。</p></dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>
<ul>
<li>此参数不可用于免费集群。</li>
<li>如果此标准集群是您在此位置中创建的第一个标准集群，请勿使用此标志。创建集群时，将为您创建公用 VLAN。</li>
<li>如果之前在此位置中已创建标准集群，或者之前在 IBM Cloud infrastructure (SoftLayer) 中已创建公用 VLAN，那么必须指定该公用 VLAN。



<p><strong>注：</strong>专用 VLAN 路由器始终以 <code>bcr</code>（后端路由器）开头，而公用 VLAN 路由器始终以 <code>fcr</code>（前端路由器）开头。创建集群并指定公用和专用 VLAN 时，在这些前缀之后的数字和字母组合必须匹配。</p></li>
</ul>

<p>要了解您是否已具有用于特定位置的公用 VLAN，或要找到现有公用 VLAN 的名称，请运行 <code>bx cs vlans <em>&lt;location&gt;</em></code>。</p></dd>

<dt><code>--workers WORKER</code></dt>
<dd>要在集群中部署的工作程序节点数。如果未指定此选项，将创建具有 1 个工作程序节点的集群。此值对于标准集群是可选的，且不可用于免费集群。

<p><strong>注</strong>：为每个工作程序节点分配了唯一的工作程序节点标识和域名，在创建集群后，不得手动更改该标识和域名。更改标识或域名会阻止 Kubernetes 主节点管理集群。</p></dd>

<dt><code>--disable-disk-encrypt</code></dt>
<dd>工作程序节点缺省情况下具有磁盘加密功能：[了解更多](cs_secure.html#worker)。要禁用加密，请包括此选项。</dd>

<dt><code>--trusted</code></dt>
<dd><p>**仅限裸机**：启用[可信计算](cs_secure.html#trusted_compute)以验证裸机工作程序节点是否被篡改。如果在创建集群期间未启用信任，但希望日后启用，那么可以使用 `bx cs feature-enable` [命令](cs_cli_reference.html#cs_cluster_feature_enable)。启用信任后，日后无法将其禁用。</p>
<p>要检查裸机机器类型是否支持信任，请检查 `bx cs machine-types <location>` [命令](#cs_machine_types)输出中的 `Trustable` 字段。要验证集群是否已启用信任，请查看 `bx cs cluster-get` [命令](#cs_cluster_get)输出中的 **Trust ready** 字段。要验证裸机工作程序节点是否已启用信任，请查看 `bx cs worker-get` [ 命令](#cs_worker_get)输出中的 **Trust** 字段。</p></dd>
</dl>

**示例**：

  

  标准集群的示例：
  {: #example_cluster_create}

  ```
  bx cs cluster-create --location dal10 --public-vlan my_public_VLAN_ID --private-vlan my_private_VLAN_ID --machine-type u2c.2x4 --name my_cluster --hardware shared --workers 2
  ```
  {: pre}

  免费集群的示例：

  ```
  bx cs cluster-create --name my_cluster
  ```
  {: pre}

  {{site.data.keyword.Bluemix_dedicated_notm}} 环境的示例：

  ```
  bx cs cluster-create --machine-type machine-type --workers number --name cluster_name
  ```
  {: pre}

### bx cs cluster-feature-enable CLUSTER [--trusted]
{: #cs_cluster_feature_enable}

在现有集群上启用功能。

<strong>命令选项</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>集群的名称或标识。此值是必需的。</dd>

   <dt><code><em>--trusted</em></code></dt>
   <dd><p>包含此标志可对集群中所有支持的裸机工作程序节点启用[可信计算](cs_secure.html#trusted_compute)。启用信任后，日后无法对集群禁用信任。</p>
   <p>要检查裸机机器类型是否支持信任，请检查 `bx cs machine-types <location>` [命令](#cs_machine_types)输出中的 `Trustable` 字段。要验证集群是否已启用信任，请查看 `bx cs cluster-get` [命令](#cs_cluster_get)输出中的 **Trust ready** 字段。要验证裸机工作程序节点是否已启用信任，请查看 `bx cs worker-get` [ 命令](#cs_worker_get)输出中的 **Trust** 字段。</p></dd>
   </dl>

**示例命令**：

  ```
  bx cs cluster-feature-enable my_cluster --trusted=true
  ```
  {: pre}

### bx cs cluster-get CLUSTER [--showResources]
{: #cs_cluster_get}

查看有关组织中集群的信息。

<strong>命令选项</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>集群的名称或标识。此值是必需的。</dd>

   <dt><code><em>--showResources</em></code></dt>
   <dd>显示更多集群资源，例如附加组件、VLAN、子网和存储器。</dd>
   </dl>

**示例命令**：

  ```
  bx cs cluster-get my_cluster --showResources
  ```
  {: pre}

**示例输出**：

  ```
  Name:        my_cluster
  ID:          abc1234567
  State:       normal
  Trust ready: false
  Created:     2018-01-01T17:19:28+0000
  Location:    dal10
  Master URL:  https://169.xx.xxx.xxx:xxxxx
  Ingress subdomain: my_cluster.us-south.containers.appdomain.cloud
  Ingress secret:    my_cluster
  Workers:     3
  Version:     1.7.16_1511* (1.8.11_1509 latest)
  Owner Email: name@example.com
  Monitoring dashboard: https://metrics.ng.bluemix.net/app/#/grafana4/dashboard/db/link

  Addons
  Name                   Enabled
  customer-storage-pod   true
  basic-ingress-v2       true
  storage-watcher-pod    true

  Subnet VLANs
  VLAN ID   Subnet CIDR         Public   User-managed
  2234947   10.xxx.xx.xxx/29    false    false
  2234945   169.xx.xxx.xxx/29  true    false

  ```
  {: screen}

### bx cs cluster-rm [-f] CLUSTER
{: #cs_cluster_rm}

从组织中除去集群。

<strong>命令选项</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>集群的名称或标识。此值是必需的。</dd>

   <dt><code>-f</code></dt>
   <dd>使用此选项可强制除去集群，而不显示用户提示。此值是可选的。</dd>
   </dl>

**示例**：

  ```
  bx cs cluster-rm my_cluster
  ```
  {: pre}


### bx cs cluster-update [-f] CLUSTER [--kube-version MAJOR.MINOR.PATCH][--force-update]
{: #cs_cluster_update}

将 Kubernetes 主节点更新到缺省 API 版本。在更新期间，您无法访问或更改集群。用户已部署的工作程序节点、应用程序和资源不会被修改，并且将继续运行。

您可能需要更改 YAML 文件以供未来部署。请查看此[发行说明](cs_versions.html)以了解详细信息。

<strong>命令选项</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>集群的名称或标识。此值是必需的。</dd>

   <dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
   <dd>集群的 Kubernetes 版本。如果未指定版本，那么 Kubernetes 主节点将更新到缺省 API 版本。要查看可用版本，请运行 [bx cs kube-versions](#cs_kube_versions)。此值是可选的。</dd>

   <dt><code>-f</code></dt>
   <dd>使用此选项可强制更新主节点，而不显示用户提示。此值是可选的。</dd>

   <dt><code>--force-update</code></dt>
   <dd>即便更改是跨 2 个以上的次版本，也仍尝试更新。此值是可选的。</dd>
   </dl>

**示例**：

  ```
  bx cs cluster-update my_cluster
  ```
  {: pre}


### bx cs clusters
{: #cs_clusters}

查看组织中集群的列表。

<strong>命令选项</strong>：

  无

**示例**：

  ```
        bx cs clusters
        ```
  {: pre}


### bx cs kube-versions
{: #cs_kube_versions}

查看 {{site.data.keyword.containershort_notm}} 中支持的 Kubernetes 版本列表。将[集群主节点](#cs_cluster_update)和[工作程序节点](cs_cli_reference.html#cs_worker_update)更新到缺省版本以获取最新的稳定功能。

**命令选项**：

  无

**示例**：

  ```
  bx cs kube-versions
  ```
  {: pre}



<br />



## 集群命令：服务和集成
{: #cluster_services_commands}


### bx cs cluster-service-bind CLUSTER KUBERNETES_NAMESPACE SERVICE_INSTANCE_NAME
{: #cs_cluster_service_bind}

向集群添加 {{site.data.keyword.Bluemix_notm}} 服务。要查看 {{site.data.keyword.Bluemix_notm}}“目录”中的可用 {{site.data.keyword.Bluemix_notm}} 服务，请运行 `bx service offerings`。**注**：只能添加支持服务密钥的 {{site.data.keyword.Bluemix_notm}} 服务。

<strong>命令选项</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>集群的名称或标识。此值是必需的。</dd>

   <dt><code><em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>Kubernetes 名称空间的名称。此值是必需的。</dd>

   <dt><code><em>SERVICE_INSTANCE_NAME</em></code></dt>
   <dd>要绑定的 {{site.data.keyword.Bluemix_notm}} 服务实例的名称。要查找服务实例的名称，请运行 <code>bx service list</code>。如果多个实例在帐户中具有相同名称，请使用服务实例标识来代替名称。要查找标识，请运行 <code>bx service show <service instance name> --guid</code>。其中一个值是必需的。</dd>
   </dl>

**示例**：

  ```
  bx cs cluster-service-bind my_cluster my_namespace my_service_instance
  ```
  {: pre}


### bx cs cluster-service-unbind CLUSTER KUBERNETES_NAMESPACE SERVICE_INSTANCE_GUID
{: #cs_cluster_service_unbind}

从集群中除去 {{site.data.keyword.Bluemix_notm}} 服务。

**注：**除去 {{site.data.keyword.Bluemix_notm}} 服务时，会从集群中除去服务凭证。如果 pod 仍在使用该服务，那么除去操作会因为找不到服务凭证而失败。

<strong>命令选项</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>集群的名称或标识。此值是必需的。</dd>

   <dt><code><em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>Kubernetes 名称空间的名称。此值是必需的。</dd>

   <dt><code><em>SERVICE_INSTANCE_GUID</em></code></dt>
   <dd>要除去的 {{site.data.keyword.Bluemix_notm}} 服务实例的标识。要找到服务实例的标识，请运行 `bx cs cluster-services <cluster_name_or_ID>`。此值是必需的。</dd>
   </dl>

**示例**：

  ```
  bx cs cluster-service-unbind my_cluster my_namespace 8567221
  ```
  {: pre}


### bx cs cluster-services CLUSTER [--namespace KUBERNETES_NAMESPACE][--all-namespaces]
{: #cs_cluster_services}

列出绑定到集群中一个或全部 Kubernetes 名称空间的服务。如果未指定任何选项，那么将显示缺省名称空间的服务。

<strong>命令选项</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>集群的名称或标识。此值是必需的。</dd>

   <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code>, <code>-n
<em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>包含绑定到集群中特定名称空间的服务。此值是可选的。</dd>

   <dt><code>--all-namespaces</code></dt>
    <dd>包含绑定到集群中所有名称空间的服务。此值是可选的。</dd>
    </dl>

**示例**：

  ```
  bx cs cluster-services my_cluster --namespace my_namespace
  ```
  {: pre}



### bx cs webhook-create --cluster CLUSTER --level LEVEL --type slack --url URL
{: #cs_webhook_create}

注册 Webhook。

<strong>命令选项</strong>：

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>集群的名称或标识。此值是必需的。</dd>

   <dt><code>--level <em>LEVEL</em></code></dt>
   <dd>通知级别，例如 <code>Normal</code> 或 <code>Warning</code>。<code>Warning</code> 是缺省值。此值是可选的。</dd>

   <dt><code>--type <em>slack</em></code></dt>
   <dd>Webhook 类型。当前支持 slack。此值是必需的。</dd>

   <dt><code>--url <em>URL</em></code></dt>
   <dd>Webhook 的 URL。此值是必需的。</dd>
   </dl>

**示例**：

  ```
  bx cs webhook-create --cluster my_cluster --level Normal --type slack --url http://github.com/mywebhook
  ```
  {: pre}


<br />


## 集群命令：子网
{: #cluster_subnets_commands}

### bx cs cluster-subnet-add CLUSTER SUBNET
{: #cs_cluster_subnet_add}

使 IBM Cloud infrastructure (SoftLayer) 帐户中的子网可供指定集群使用。

**注：**
* 使子网可供集群使用时，此子网的 IP 地址会用于集群联网。为了避免 IP 地址冲突，请确保一个子网只用于一个集群。不要同时将一个子网用于多个集群或用于 {{site.data.keyword.containershort_notm}} 外部的其他用途。
* 要在同一 VLAN 上的子网之间进行路由，必须[开启 VLAN 生成](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning)。

<strong>命令选项</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>集群的名称或标识。此值是必需的。</dd>

   <dt><code><em>SUBNET</em></code></dt>
   <dd>这是子网的标识。此值是必需的。</dd>
   </dl>

**示例**：

  ```
  bx cs cluster-subnet-add my_cluster 1643389
  ```
  {: pre}


### bx cs cluster-subnet-create CLUSTER SIZE VLAN_ID
{: #cs_cluster_subnet_create}

在 IBM Cloud infrastructure (SoftLayer) 帐户中创建子网，并使其可供 {{site.data.keyword.containershort_notm}} 中的指定集群使用。

**注：**
* 使子网可供集群使用时，此子网的 IP 地址会用于集群联网。为了避免 IP 地址冲突，请确保一个子网只用于一个集群。不要同时将一个子网用于多个集群或用于 {{site.data.keyword.containershort_notm}} 外部的其他用途。
* 要在同一 VLAN 上的子网之间进行路由，必须[开启 VLAN 生成](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning)。

<strong>命令选项</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>集群的名称或标识。此值是必需的。要列出集群，请使用 `bx cs clusters` [命令](#cs_clusters)。</dd>

   <dt><code><em>SIZE</em></code></dt>
   <dd>子网 IP 地址数。此值是必需的。可能的值为 8、16、32 或 64。</dd>

   <dt><code><em>VLAN_ID</em></code></dt>
   <dd>要在其中创建子网的 VLAN。此值是必需的。要列出可用的 VLAN，请使用 `bx cs vlans<location>` [命令](#cs_vlans)。</dd>
   </dl>

**示例**：

  ```
  bx cs cluster-subnet-create my_cluster 8 1764905
  ```
  {: pre}


### bx cs cluster-user-subnet-add CLUSTER SUBNET_CIDR PRIVATE_VLAN
{: #cs_cluster_user_subnet_add}

将您自己的专用子网置于 {{site.data.keyword.containershort_notm}} 集群中。

此专用子网不是 IBM Cloud infrastructure (SoftLayer) 提供的子网。因此，您必须为子网配置任何入站和出站网络流量路由。要添加 IBM Cloud infrastructure (SoftLayer) 子网，请使用 `bx cs cluster-subnet-add` [命令](#cs_cluster_subnet_add)。

**注**：
* 将专用用户子网添加到集群时，此子网的 IP 地址将用于集群中的专用负载均衡器。为了避免 IP 地址冲突，请确保一个子网只用于一个集群。不要同时将一个子网用于多个集群或用于 {{site.data.keyword.containershort_notm}} 外部的其他用途。
* 要在同一 VLAN 上的子网之间进行路由，必须[开启 VLAN 生成](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning)。

<strong>命令选项</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>集群的名称或标识。此值是必需的。</dd>

   <dt><code><em>SUBNET_CIDR</em></code></dt>
   <dd>子网无类域间路由 (CIDR)。此值是必需的，且不得与 IBM Cloud infrastructure (SoftLayer) 使用的任何子网相冲突。



   支持的前缀范围从 `/30`（1 个 IP 地址）到 `/24`（253 个 IP 地址）。如果将 CIDR 设置为一个前缀长度，而稍后需要对其进行更改，请先添加新的 CIDR，然后[除去旧 CIDR](#cs_cluster_user_subnet_rm)。</dd>

   <dt><code><em>PRIVATE_VLAN</em></code></dt>
   <dd>专用 VLAN 的标识。此值是必需的。它必须与集群中一个或多个工作程序节点的专用 VLAN 标识相匹配。</dd>
   </dl>

**示例**：

  ```
  bx cs cluster-user-subnet-add my_cluster 169.xx.xxx.xxx/29 1502175
  ```
  {: pre}


### bx cs cluster-user-subnet-rm CLUSTER SUBNET_CIDR PRIVATE_VLAN
{: #cs_cluster_user_subnet_rm}

从指定集群除去自己的专用子网。

**注：**除去子网后，部署到您自己专用子网的 IP 地址的任何服务都将保持活动状态。

<strong>命令选项</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>集群的名称或标识。此值是必需的。</dd>

   <dt><code><em>SUBNET_CIDR</em></code></dt>
   <dd>子网无类域间路由 (CIDR)。此值是必需的，并且必须与 `bx cs cluster-user-subnet-add` [命令](#cs_cluster_user_subnet_add)设置的 CIDR 匹配。</dd>

   <dt><code><em>PRIVATE_VLAN</em></code></dt>
   <dd>专用 VLAN 的标识。此值是必需的，并且必须与 `bx cs cluster-user-subnet-add` [命令](#cs_cluster_user_subnet_add)设置的 VLAN 标识匹配。</dd>
   </dl>

**示例**：

  ```
  bx cs cluster-user-subnet-rm my_cluster 169.xx.xxx.xxx/29 1502175
  ```
  {: pre}

### bx cs subnets
{: #cs_subnets}

查看 IBM Cloud infrastructure (SoftLayer) 帐户中可用的子网列表。

<strong>命令选项</strong>：

   无

**示例**：

  ```
  bx cs subnets
  ```
  {: pre}


<br />


## Ingress 应用程序负载均衡器 (ALB) 命令
{: #alb_commands}

### bx cs alb-cert-deploy [--update] --cluster CLUSTER --secret-name SECRET_NAME --cert-crn CERTIFICATE_CRN
{: #cs_alb_cert_deploy}

将 {{site.data.keyword.cloudcerts_long_notm}} 实例中的证书部署或更新到集群中的 ALB。

**注：**
* 只有具有管理员访问权角色的用户才能执行此命令。
* 只能更新从同一 {{site.data.keyword.cloudcerts_long_notm}} 实例导入的证书。

<strong>命令选项</strong>

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>集群的名称或标识。此值是必需的。</dd>

   <dt><code>--update</code></dt>
   <dd>更新集群中 ALB 私钥的证书。此值是可选的。</dd>

   <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
   <dd>ALB 私钥的名称。此值是必需的。</dd>

   <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
   <dd>证书 CRN。此值是必需的。</dd>
   </dl>

**示例**：

部署 ALB 私钥的示例：

   ```
   bx cs alb-cert-deploy --secret-name my_alb_secret --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
   ```
   {: pre}

更新现有 ALB 私钥的示例：

 ```
 bx cs alb-cert-deploy --update --secret-name my_alb_secret --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:7e21fde8ee84a96d29240327daee3eb2
 ```
 {: pre}


### bx cs alb-cert-get --cluster CLUSTER [--secret-name SECRET_NAME][--cert-crn CERTIFICATE_CRN]
{: #cs_alb_cert_get}

查看有关集群中 ALB 私钥的信息。

**注：**只有具有管理员访问权角色的用户才能执行此命令。

<strong>命令选项</strong>

  <dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
  <dd>集群的名称或标识。此值是必需的。</dd>

  <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
  <dd>ALB 私钥的名称。要获取有关集群中特定 ALB 私钥的信息，此值是必需的。</dd>

  <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
  <dd>证书 CRN。要获取有关集群中与特定证书 CRN 匹配的所有 ALB 私钥的信息，此值是必需的。</dd>
  </dl>

**示例**：

 访存 ALB 私钥相关信息的示例：

 ```
 bx cs alb-cert-get --cluster my_cluster --secret-name my_alb_secret
 ```
 {: pre}

 访存与指定证书 CRN 匹配的所有 ALB 私钥相关信息的示例：

 ```
 bx cs alb-cert-get --cluster my_cluster --cert-crn  crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
 ```
 {: pre}


### bx cs alb-cert-rm --cluster CLUSTER [--secret-name SECRET_NAME][--cert-crn CERTIFICATE_CRN]
{: #cs_alb_cert_rm}

除去集群中的 ALB 私钥。

**注：**只有具有管理员访问权角色的用户才能执行此命令。

<strong>命令选项</strong>

  <dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
  <dd>集群的名称或标识。此值是必需的。</dd>

  <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
  <dd>ALB 私钥的名称。要除去集群中的特定 ALB 私钥，此值是必需的。</dd>

  <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
  <dd>证书 CRN。要除去集群中与特定证书 CRN 匹配的所有 ALB 私钥，此值是必需的。</dd>
  </dl>

**示例**：

 除去 ALB 私钥的示例：

 ```
 bx cs alb-cert-rm --cluster my_cluster --secret-name my_alb_secret
 ```
 {: pre}

 除去与指定证书 CRN 匹配的所有 ALB 私钥的示例：

 ```
 bx cs alb-cert-rm --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
 ```
 {: pre}


### bx cs alb-certs --cluster CLUSTER
{: #cs_alb_certs}

查看集群中 ALB 私钥的列表。

**注**：只有具有管理员访问权角色的用户才能执行此命令。

<strong>命令选项</strong>

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>集群的名称或标识。此值是必需的。</dd>
   </dl>

**示例**：

 ```
 bx cs alb-certs --cluster my_cluster
 ```
 {: pre}

### bx cs alb-configure --albID ALB_ID [--enable][--disable][--user-ip USERIP]
{: #cs_alb_configure}

在标准集群中启用或禁用 ALB。缺省情况下，已启用公共 ALB。

**命令选项**：

   <dl>
   <dt><code><em>--albID </em>ALB_ID</code></dt>
   <dd>ALB 的标识。运行 <code>bx cs albs <em>--cluster </em>CLUSTER</code> 可查看集群中 ALB 的标识。此值是必需的。</dd>

   <dt><code>--enable</code></dt>
   <dd>包含此标志可在集群中启用 ALB。</dd>

   <dt><code>--disable</code></dt>
   <dd>包含此标志可在集群中禁用 ALB。</dd>

   <dt><code>--user-ip <em>USER_IP</em></code></dt>
   <dd>

   <ul>
    <li>此参数仅可用于启用专用 ALB。</li>
    <li>专用 ALB 将使用用户提供的专用子网中的 IP 地址进行部署。如果未提供任何 IP 地址，那么该 ALB 将使用创建集群时自动供应的可移植专用子网中的专用 IP 地址进行部署。</li>
   </ul>
   </dd>
   </dl>

**示例**：

  启用 ALB 的示例：

  ```
  bx cs alb-configure --albID private-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --enable
  ```
  {: pre}

  使用用户提供的 IP 地址启用 ALB 的示例：

  ```
  bx cs alb-configure --albID private-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --enable --user-ip user_ip
  ```
  {: pre}

  禁用 ALB 的示例：

  ```
  bx cs alb-configure --albID public-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --disable
  ```
  {: pre}

### bx cs alb-get --albID ALB_ID
{: #cs_alb_get}

查看 ALB 的详细信息。

<strong>命令选项</strong>：

   <dl>
   <dt><code><em>--albID </em>ALB_ID</code></dt>
   <dd>ALB 的标识。运行 <code>bx cs albs --cluster <em>CLUSTER</em></code> 可查看集群中 ALB 的标识。此值是必需的。</dd>
   </dl>

**示例**：

  ```
  bx cs alb-get --albID public-cr18a61a63a6a94b658596aa93a087aaa9-alb1
  ```
  {: pre}

### bx cs alb-types
{: #cs_alb_types}

查看区域中支持的 ALB 类型。

<strong>命令选项</strong>：

   无

**示例**：

  ```
  bx cs alb-types
  ```
  {: pre}


### bx cs albs --cluster CLUSTER
{: #cs_albs}

查看集群中所有 ALB 的阶段状态。如果未返回任何 ALB 标识，说明集群没有可移植子网。您可以为集群[创建](#cs_cluster_subnet_create)或[添加](#cs_cluster_subnet_add)子网。

<strong>命令选项</strong>：

   <dl>
   <dt><code><em>--cluster </em>CLUSTER</code></dt>
   <dd>列出其中可用 ALB 的集群的名称或标识。此值是必需的。</dd>
   </dl>

**示例**：

  ```
  bx cs albs --cluster my_cluster
  ```
  {: pre}


<br />


## 基础架构命令
{: #infrastructure_commands}

### bx cs credentials-set --infrastructure-api-key API_KEY --infrastructure-username USERNAME
{: #cs_credentials_set}

为 {{site.data.keyword.containershort_notm}} 帐户设置 IBM Cloud infrastructure (SoftLayer) 帐户凭证。

如果您有 {{site.data.keyword.Bluemix_notm}} 现买现付帐户，那么缺省情况下您可以访问 IBM Cloud Infrastructure (SoftLayer) 产品服务组合。但是，您可能希望使用已经拥有的其他 IBM Cloud Infrastructure (SoftLayer) 帐户来订购基础架构。您可以使用此命令将此基础架构帐户链接到 {{site.data.keyword.Bluemix_notm}} 帐户。

如果手动设置了 IBM Cloud Infrastructure (SoftLayer) 凭证，那么这些凭证会用于订购基础架构，即使已存在帐户的 [IAM API 密钥](#cs_api_key_info)也不例外。如果存储了其凭证的用户没有必需的许可权来订购基础架构，那么与基础架构相关的操作（例如，创建集群或重新装入工作程序节点）可能会失败。

不能为一个 {{site.data.keyword.containershort_notm}} 帐户设置多个凭证。每个 {{site.data.keyword.containershort_notm}} 帐户仅链接到一个 IBM Cloud infrastructure (SoftLayer) 产品服务组合。

**重要信息**：使用此命令之前，请确保使用其凭证的用户具有必需的 [{{site.data.keyword.containershort_notm}} 和 IBM Cloud Infrastructure (SoftLayer) 许可权](cs_users.html#users)。

<strong>命令选项</strong>：

   <dl>
   <dt><code>--infrastructure-username <em>USERNAME</em></code></dt>
   <dd>IBM Cloud infrastructure (SoftLayer) 帐户用户名。此值是必需的。</dd>


   <dt><code>--infrastructure-api-key <em>API_KEY</em></code></dt>
   <dd>IBM Cloud infrastructure (SoftLayer) 帐户 API 密钥。此值是必需的。

 <p>
要生成 API 密钥：

  <ol>
  <li>登录到 [IBM Cloud infrastructure (SoftLayer) 门户网站 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/)。</li>
  <li>选择<strong>帐户</strong>，然后选择<strong>用户</strong>。</li>
  <li>单击<strong>生成</strong>，为帐户生成 IBM Cloud infrastructure (SoftLayer) API 密钥。</li>
  <li>复制 API 密钥以在此命令中使用。</li>
  </ol>

  要查看现有 API 密钥：
  <ol>
  <li>登录到 [IBM Cloud infrastructure (SoftLayer) 门户网站 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/)。</li>
  <li>选择<strong>帐户</strong>，然后选择<strong>用户</strong>。</li>
  <li>单击<strong>查看</strong>以查看现有 API 密钥。</li>
  <li>复制 API 密钥以在此命令中使用。</li>
  </ol>
  </p></dd>
  </dl>

**示例**：

  ```
  bx cs credentials-set --infrastructure-api-key <api_key> --infrastructure-username dbmanager
  ```
  {: pre}


### bx cs credentials-unset
{: #cs_credentials_unset}

从 {{site.data.keyword.containershort_notm}} 帐户中除去 IBM Cloud infrastructure (SoftLayer) 帐户凭证。

除去凭证后，将使用 [IAM API 密钥](#cs_api_key_info)来订购 IBM Cloud infrastructure (SoftLayer) 中的资源。

<strong>命令选项</strong>：

   无

**示例**：

  ```
  bx cs credentials-unset
  ```
  {: pre}


### bx cs machine-types LOCATION
{: #cs_machine_types}

查看可用于工作程序节点的机器类型的列表。每种机器类型都包含集群中每个工作程序节点的虚拟 CPU 量、内存量和磁盘空间量。缺省情况下，存储所有容器数据的 `/var/lib/docker` 目录将使用 LUKS 加密进行加密。如果在创建集群期间包含了 `disable-disk-encrypt` 选项，那么不会加密主机的 Docker 数据。[了解有关加密的更多信息](cs_secure.html#encrypted_disks)。
{:shortdesc}

可以将工作程序节点作为虚拟机在共享或专用硬件上进行供应，也可以作为物理机器在裸机上进行供应。

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
<p>机器类型因专区而变化。要查看专区中可用的机器类型，请运行 `bx cs machine-types<zone_name>`。</p>
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


<strong>命令选项</strong>：

   <dl>
   <dt><code><em>LOCATION</em></code></dt>
   <dd>输入要列出其中可用机器类型的位置。此值是必需的。复查[可用位置](cs_regions.html#locations)。
</dd></dl>

**示例命令**：

  ```
  bx cs machine-types dal10
  ```
  {: pre}

**示例输出**：

  ```
  Getting machine types list...
  OK
  Machine Types
  Name                 Cores   Memory   Network Speed   OS             Server Type   Storage   Secondary Storage   Trustable
  u2c.2x4              2       4GB      1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  b2c.4x16             4       16GB     1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  b2c.16x64            16      64GB     1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  b2c.32x128           32      128GB    1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  b2c.56x242           56      242GB    1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  mb1c.4x32            4       32GB     10000Mbps       UBUNTU_16_64   physical      1000GB    2000GB              False
  mb1c.16x64           16      64GB     10000Mbps       UBUNTU_16_64   physical      1000GB    1700GB              False
  mr1c.28x512          28      512GB    10000Mbps       UBUNTU_16_64   physical      1000GB    1700GB              False
  md1c.16x64.4x4tb     16      64GB     10000Mbps       UBUNTU_16_64   physical      1000GB    8000GB              False
  md1c.28x512.4x4tb    28      512GB    10000Mbps       UBUNTU_16_64   physical      1000GB    8000GB              False
  
  ```
  {: screen}


### bx cs vlans LOCATION [--all]
{: #cs_vlans}

列出可用于 IBM Cloud infrastructure (SoftLayer) 帐户中位置的公用和专用 VLAN。要列出可用 VLAN，您必须具有付费帐户。

<strong>命令选项</strong>：

   <dl>
   <dt><code><em>LOCATION</em></code></dt>
   <dd>输入要列出其中专用和公用 VLAN 的位置。此值是必需的。复查[可用位置](cs_regions.html#locations)。
</dd>
   <dt><code>--all</code></dt>
   <dd>列出所有可用的 VLAN。缺省情况下，会对 VLAN 进行过滤，以仅显示有效的 VLAN。要使 VLAN 有效，必须将 VLAN 与可使用本地磁盘存储来托管工作程序的基础架构相关联。</dd>
   </dl>

**示例**：

  ```
    bx cs vlans dal10
    ```
  {: pre}


<br />


## 日志记录命令
{: #logging_commands}

### bx cs logging-config-create CLUSTER --logsource LOG_SOURCE [--namespace KUBERNETES_NAMESPACE][--hostname LOG_SERVER_HOSTNAME_OR_IP] [--port LOG_SERVER_PORT][--space CLUSTER_SPACE] [--org CLUSTER_ORG][--app-containers CONTAINERS] [--app-paths PATHS_TO_LOGS] --type LOG_TYPE [--json][--skip-validation]
{: #cs_logging_create}

创建日志记录配置。您可以使用此命令将容器、应用程序、工作程序节点、Kubernetes 集群以及 Ingress 应用程序负载均衡器的日志转发到 {{site.data.keyword.loganalysisshort_notm}} 或外部 syslog 服务器。

<strong>命令选项</strong>：

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>集群的名称或标识。</dd>
  <dt><code>--logsource <em>LOG_SOURCE</em></code></dt>
    <dd>要对其启用日志转发的日志源。此自变量支持要应用其配置的日志源的逗号分隔列表。接受的值为 <code>container</code>、<code>application</code>、<code>worker</code>、<code>kubernetes</code> 和 <code>ingress</code>。如果未提供日志源，那么会为 <code>container</code> 和 <code>ingress</code> 日志源创建日志记录配置。</dd>
  <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
    <dd>要从中转发日志的 Kubernetes 名称空间。<code>ibm-system</code> 和 <code>kube-system</code> Kubernetes 名称空间不支持日志转发。此值仅对容器日志源有效，并且是可选的。如果未指定名称空间，那么集群中的所有名称空间都将使用此配置。</dd>
  <dt><code>--hostname <em>LOG_SERVER_HOSTNAME</em></code></dt>
    <dd>日志记录类型为 <code>syslog</code> 时，日志收集器服务器的主机名或 IP 地址。此值对于 <code>syslog</code> 是必需的。日志记录类型为 <code>ibm</code> 时，{{site.data.keyword.loganalysislong_notm}} 数据获取 URL。您可以在[此处](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls)找到可用数据获取 URL 的列表。如果未指定数据获取 URL，那么将使用创建集群所在区域的端点。</dd>
  <dt><code>--port <em>LOG_SERVER_PORT</em></code></dt>
    <dd>日志收集器服务器的端口。此值是可选的。如果未指定端口，那么标准端口 <code>514</code> 将用于 <code>syslog</code>，并且标准端口 <code>9091</code> 将用于 <code>ibm</code>。</dd>
  <dt><code>--space <em>CLUSTER_SPACE</em></code></dt>
    <dd>要向其发送日志的 Cloud Foundry 空间的名称。此值仅对日志类型 <code>ibm</code> 有效，并且是可选的。如果未指定空间，日志将发送到帐户级别。</dd>
  <dt><code>--org <em>CLUSTER_ORG</em></code></dt>
    <dd>该空间所在 Cloud Foundry 组织的名称。此值仅对日志类型 <code>ibm</code> 有效，如果指定了空间，那么此值是必需的。</dd>
  <dt><code>--app-paths</code></dt>
    <dd>容器上应用程序要将日志记录到的路径。要转发源类型为 <code>application</code> 的日志，必须提供路径。要指定多个路径，请使用逗号分隔列表。此值对于日志源 <code>application</code> 是必需的。示例：<code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></dd>
  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>要转发日志的位置。选项为 <code>ibm</code>（将日志转发到 {{site.data.keyword.loganalysisshort_notm}}）和 <code>syslog</code>（将日志转发到外部服务器）。</dd>
  <dt><code>--app-containers</code></dt>
    <dd>可选：要转发来自应用程序的日志，可以指定包含应用程序的容器的名称。可以使用逗号分隔列表来指定多个容器。如果未指定任何容器，那么会转发来自包含所提供路径的所有容器中的日志。此选项仅对日志源 <code>application</code> 有效。</dt>
  <dt><code>--json</code></dt>
    <dd>以 JSON 格式打印命令输出。此值是可选的。</dd>
  <dt><code>--skip-validation</code></dt>
    <dd>跳过对指定组织和空间名称的验证。跳过验证可减少处理时间，但无效的日志记录配置将无法正确转发日志。此值是可选的。</dd>
</dl>

**示例**：

缺省端口 9091 上从 `container` 日志源转发的日志类型 `ibm` 的示例：

  ```
  bx cs logging-config-create my_cluster --logsource container --namespace my_namespace --hostname ingest.logging.ng.bluemix.net --type ibm
  ```
  {: pre}

缺省端口 514 上从 `container` 日志源转发的日志类型 `syslog` 的示例：

  ```
  bx cs logging-config-create my_cluster --logsource container --namespace my_namespace  --hostname 169.xx.xxx.xxx --type syslog
  ```
  {: pre}

非缺省端口上从 `ingress` 源转发日志的日志类型 `syslog` 的实例：

  ```
  bx cs logging-config-create my_cluster --logsource container --hostname 169.xx.xxx.xxx --port 5514 --type syslog
  ```
  {: pre}

### bx cs logging-config-get CLUSTER [--logsource LOG_SOURCE][--json]
{: #cs_logging_get}

查看集群的所有日志转发配置，或基于日志源过滤日志记录配置。

<strong>命令选项</strong>：

 <dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>集群的名称或标识。此值是必需的。</dd>
  <dt><code>--logsource <em>LOG_SOURCE</em></code></dt>
    <dd>要过滤的日志源的类型。仅会返回集群中此日志源的日志记录配置。接受的值为 <code>container</code>、<code>application</code>、<code>worker</code>、<code>kubernetes</code> 和 <code>ingress</code>。此值是可选的。</dd>
  <dt><code>--json</code></dt>
    <dd>（可选）以 JSON 格式打印命令输出。</dd>
 </dl>

**示例**：

  ```
  bx cs logging-config-get my_cluster --logsource worker
  ```
  {: pre}


### bx cs logging-config-refresh CLUSTER
{: #cs_logging_refresh}

刷新集群的日志记录配置。这将刷新转发到集群中空间级别的任何日志记录配置的日志记录令牌。

<strong>命令选项</strong>：

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
   <dd>集群的名称或标识。此值是必需的。</dd>
</dl>

**示例**：

  ```
  bx cs logging-config-refresh my_cluster
  ```
  {: pre}


### bx cs logging-config-rm CLUSTER [--id LOG_CONFIG_ID][--all]
{: #cs_logging_rm}

删除集群的一个日志转发配置或所有日志记录配置。这会停止将日志转发到远程 syslog 服务器或 {{site.data.keyword.loganalysisshort_notm}}。

<strong>命令选项</strong>：

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
   <dd>集群的名称或标识。此值是必需的。</dd>
  <dt><code>--id <em>LOG_CONFIG_ID</em></code></dt>
   <dd>如果要除去单个日志记录配置，此值为日志记录配置标识。</dd>
  <dt><code>--all</code></dt>
   <dd>用于除去集群中所有日志记录配置的标志。</dd>
</dl>

**示例**：

  ```
  bx cs logging-config-rm my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e
  ```
  {: pre}


### bx cs logging-config-update CLUSTER --id LOG_CONFIG_ID [--namespace NAMESPACE][--hostname LOG_SERVER_HOSTNAME_OR_IP] [--port LOG_SERVER_PORT][--space CLUSTER_SPACE] [--org CLUSTER_ORG] --type LOG_TYPE [--json][--skipValidation]
{: #cs_logging_update}

更新日志转发配置的详细信息。

<strong>命令选项</strong>：

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
   <dd>集群的名称或标识。此值是必需的。</dd>
  <dt><code>--id <em>LOG_CONFIG_ID</em></code></dt>
   <dd>要更新的日志记录配置标识。此值是必需的。</dd>
  <dt><code>--namespace <em>NAMESPACE</em></code>
    <dd>要从中转发日志的 Kubernetes 名称空间。<code>ibm-system</code> 和 <code>kube-system</code> Kubernetes 名称空间不支持日志转发。此值仅对 <code>container</code> 日志源有效。如果未指定名称空间，那么集群中的所有名称空间都将使用此配置。</dd>
  <dt><code>--hostname <em>LOG_SERVER_HOSTNAME</em></code></dt>
   <dd>日志记录类型为 <code>syslog</code> 时，日志收集器服务器的主机名或 IP 地址。此值对于 <code>syslog</code> 是必需的。日志记录类型为 <code>ibm</code> 时，{{site.data.keyword.loganalysislong_notm}} 数据获取 URL。您可以在[此处](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls)找到可用数据获取 URL 的列表。如果未指定数据获取 URL，那么将使用创建集群所在区域的端点。</dd>
   <dt><code>--port <em>LOG_SERVER_PORT</em></code></dt>
   <dd>日志收集器服务器的端口。当记录类型为 <code>syslog</code> 时，此值是可选的。如果未指定端口，那么标准端口 <code>514</code> 将用于 <code>syslog</code>，并且 <code>9091</code> 将用于 <code>ibm</code>。</dd>
   <dt><code>--space <em>CLUSTER_SPACE</em></code></dt>
   <dd>要向其发送日志的空间的名称。此值仅对日志类型 <code>ibm</code> 有效，并且是可选的。如果未指定空间，日志将发送到帐户级别。</dd>
   <dt><code>--org <em>CLUSTER_ORG</em></code></dt>
   <dd>空间所在组织的名称。此值仅对日志类型 <code>ibm</code> 有效，如果指定了空间，那么此值是必需的。</dd>
   <dt><code>--app-paths</code></dt>
     <dd>跳过对指定组织和空间名称的验证。跳过验证可减少处理时间，但无效的日志记录配置将无法正确转发日志。此值是可选的。</dd>
   <dt><code>--app-containers</code></dt>
     <dd>容器上应用程序要将日志记录到的路径。要转发源类型为 <code>application</code> 的日志，必须提供路径。要指定多个路径，请使用逗号分隔列表。示例：<code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></dd>
   <dt><code>--type <em>LOG_TYPE</em></code></dt>
   <dd>您要使用的日志转发协议。目前支持 <code>syslog</code> 和 <code>ibm</code>。此值是必需的。</dd>
   <dt><code>--json</code></dt>
   <dd>（可选）以 JSON 格式打印命令输出。</dd>
   <dt><code>--skipValidation</code></dt>
   <dd>跳过对指定组织和空间名称的验证。跳过验证可减少处理时间，但无效的日志记录配置将无法正确转发日志。此值是可选的。</dd>
   </dl>

**日志类型 `ibm` 的示例**：

  ```
  bx cs logging-config-update my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e --type ibm
  ```
  {: pre}

**日志类型 `syslog` 的示例**：

  ```
  bx cs logging-config-update my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e --hostname localhost --port 5514 --type syslog
  ```
  {: pre}


### bx cs logging-filter-create CLUSTER --type LOG_TYPE [--logging-configs CONFIGS][--namespace KUBERNETES_NAMESPACE] [--container CONTAINER_NAME][--level LOGGING_LEVEL] [--message MESSAGE][--s] [--json]
{: #cs_log_filter_create}

创建日志记录过滤器。可以使用此命令来过滤掉根据日志记录配置转发的日志。

<strong>命令选项</strong>：

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>必需：要为其创建日志记录过滤器的集群的名称或标识。</dd>
  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>要应用过滤器的日志的类型。目前支持 <code>all</code>、<code>container</code> 和 <code>host</code>。</dd>
  <dt><code>--logging-configs <em>CONFIGS</em></code></dt>
    <dd>可选：日志记录配置标识的逗号分隔列表。如果未提供，过滤器将应用于传递到过滤器的所有集群日志记录配置。可以通过将 <code>--show-matching-configs</code> 标志用于命令来查看与过滤器相匹配的日志配置。</dd>
  <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
    <dd>可选：要过滤其中日志的 Kubernetes 名称空间。</dd>
  <dt><code>--container <em>CONTAINER_NAME</em></code></dt>
    <dd>可选：要从中过滤掉日志的容器的名称。仅当使用日志类型 <code>container</code> 时，此标志才适用。</dd>
  <dt><code>--level <em>LOGGING_LEVEL</em></code></dt>
    <dd>可选：过滤掉处于指定级别及更低级别的日志。规范顺序的可接受值为 <code>fatal</code>、<code>error</code>、<code>warn/warning</code>、<code>info</code>、<code>debug</code> 和 <code>trace</code>。例如，如果过滤掉 <code>info</code> 级别的日志，那么还会过滤掉 <code>debug</code> 和 <code>trace</code>。**注**：仅当日志消息为 JSON 格式且包含 level 字段时，才能使用此标志。示例输出：<code>{"log": "hello", "level": "info"}</code></dd>
  <dt><code>--message <em>MESSAGE</em></code></dt>
    <dd>可选：过滤掉在日志中任何位置包含指定消息的任何日志。消息按字面值进行匹配，而不作为表达式进行匹配。示例：消息“Hello”、“!”和“Hello, World!”都将应用于日志“Hello, World!”。</dd>
  <dt><code>--json</code></dt>
    <dd>可选：以 JSON 格式打印命令输出。</dd>
</dl>

**示例**：

此示例过滤掉满足以下条件的所有日志：从缺省名称空间中名称为 `test-container` 的容器转发，处于 debug 级别或更低级别，并且具有包含“GET request”的日志消息。

  ```
  bx cs logging-filter-create example-cluster --type container --namespace default --container test-container --level debug --message "GET request"
  ```
  {: pre}

此示例过滤掉处于 info 级别或更低级别且从特定集群转发的所有日志。输出会作为 JSON 返回。

  ```
  bx cs logging-filter-create example-cluster --type all --level info --json
  ```
  {: pre}

### bx cs logging-filter-update CLUSTER --type LOG_TYPE [--logging-configs CONFIGS][--namespace KUBERNETES_NAMESPACE] [--container CONTAINER_NAME][--level LOGGING_LEVEL] [--message MESSAGE][--s] [--json]
{: #cs_log_filter_update}

更新日志记录过滤器。可以使用此命令来更新已创建的日志记录过滤器。

<strong>命令选项</strong>：

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>必需：要更新其日志记录过滤器的集群的名称或标识。</dd>
  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>要应用过滤器的日志的类型。目前支持 <code>all</code>、<code>container</code> 和 <code>host</code>。</dd>
  <dt><code>--logging-configs <em>CONFIGS</em></code></dt>
    <dd>可选：日志记录配置标识的逗号分隔列表。如果未提供，过滤器将应用于传递到过滤器的所有集群日志记录配置。可以通过将 <code>--show-matching-configs</code> 标志用于命令来查看与过滤器相匹配的日志配置。</dd>
  <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
    <dd>可选：要过滤其中日志的 Kubernetes 名称空间。</dd>
  <dt><code>--container <em>CONTAINER_NAME</em></code></dt>
    <dd>可选：要从中过滤掉日志的容器的名称。仅当使用日志类型 <code>container</code> 时，此标志才适用。</dd>
  <dt><code>--level <em>LOGGING_LEVEL</em></code></dt>
    <dd>可选：过滤掉处于指定级别及更低级别的日志。规范顺序的可接受值为 <code>fatal</code>、<code>error</code>、<code>warn/warning</code>、<code>info</code>、<code>debug</code> 和 <code>trace</code>。例如，如果过滤掉 <code>info</code> 级别的日志，那么还会过滤掉 <code>debug</code> 和 <code>trace</code>。**注**：仅当日志消息为 JSON 格式且包含 level 字段时，才能使用此标志。示例输出：<code>{"log": "hello", "level": "info"}</code></dd>
  <dt><code>--message <em>MESSAGE</em></code></dt>
    <dd>可选：过滤掉在日志中任何位置包含指定消息的任何日志。消息按字面值进行匹配，而不作为表达式进行匹配。示例：消息“Hello”、“!”和“Hello, World!”都将应用于日志“Hello, World!”。</dd>
  <dt><code>--json</code></dt>
    <dd>可选：以 JSON 格式打印命令输出。</dd>
</dl>


### bx cs logging-filter-get CLUSTER [--id FILTER_ID][--show-matching-configs] [--json]
{: #cs_log_filter_view}

查看日志记录过滤器配置。可以使用此命令来查看已创建的日志记录过滤器。

<strong>命令选项</strong>：

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>必需：要查看其中过滤器的集群的名称或标识。</dd>
  <dt><code>--id <em>FILTER_ID</em></code></dt>
    <dd>要查看的日志过滤器的标识。</dd>
  <dt><code>--show-matching-configs</code></dt>
    <dd>可选：显示与要查看的配置相匹配的日志记录配置。</dd>
  <dt><code>--json</code></dt>
    <dd>可选：以 JSON 格式打印命令输出。</dd>
</dl>


### bx cs logging-filter-rm CLUSTER [--id FILTER_ID][--json] [--all]
{: #cs_log_filter_delete}

删除日志记录过滤器。可以使用此命令来除去已创建的日志记录过滤器。

<strong>命令选项</strong>：

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>要删除其中过滤器的集群的名称或标识。</dd>
  <dt><code>--id <em>FILTER_ID</em></code></dt>
    <dd>要删除的日志过滤器的标识。</dd>
  <dt><code>--all</code></dt>
    <dd>可选：删除所有日志转发过滤器。</dd>
  <dt><code>--json</code></dt>
    <dd>可选：以 JSON 格式打印命令输出。</dd>
</dl>

<br />


## 区域命令
{: #region_commands}

### bx cs locations
{: #cs_datacenters}

查看可用于在其中创建集群的位置的列表。

<strong>命令选项</strong>：

   无

**示例**：

  ```
  bx cs locations
  ```
  {: pre}


### bx cs region
{: #cs_region}

查找当前所在的 {{site.data.keyword.containershort_notm}} 区域。您可以创建和管理特定于该区域的集群。使用 `bx cs region-set` 命令可更改区域。

**示例**：

```
bx cs region
```
{: pre}

**输出**：
```
Region: us-south
```
{: screen}

### bx cs region-set [REGION]
{: #cs_region-set}

设置 {{site.data.keyword.containershort_notm}} 的区域。您可以创建和管理特定于该区域的集群，并且您可能希望在多个区域中创建集群以实现高可用性。

例如，您可以登录到美国南部区域的 {{site.data.keyword.Bluemix_notm}} 并创建集群。接下来，您可以使用 `bx cs region-set eu-central` 将欧洲中部区域作为目标，并创建另一个集群。最后，您可以使用 `bx cs region-set us-south` 返回到美国南部以管理该区域中的集群。

**命令选项**：

<dl>
<dt><code><em>REGION</em></code></dt>
<dd>输入要作为目标的区域。此值是可选的。如果您未提供区域，那么可以从输出中的列表中选择区域。



要获取可用区域的列表，请查看[区域和位置](cs_regions.html)或使用 `bx cs regions` [命令](#cs_regions)。</dd></dl>

**示例**：

```
bx cs region-set eu-central
```
{: pre}

```
bx cs region-set
```
{: pre}

**输出**：
```
Choose a region:
1. ap-north
2. ap-south
3. eu-central
4. uk-south
5. us-east
6. us-south
Enter a number> 3
OK
```
{: screen}

### bx cs regions
{: #cs_regions}

列出可用的区域。`Region Name` 是 {{site.data.keyword.containershort_notm}} 名称，`Region Alias` 是区域的常规 {{site.data.keyword.Bluemix_notm}} 名称。

**示例**：

```
bx cs regions
```
{: pre}

**输出**：
```
Region Name   Region Alias
ap-north      jp-tok
ap-south      au-syd
eu-central    eu-de
uk-south      eu-gb
us-east       us-east
us-south      us-south
```
{: screen}


<br />


## 工作程序节点命令
{: worker_node_commands}



### bx cs worker-add --cluster CLUSTER [--file FILE_LOCATION][--hardware HARDWARE] --machine-type MACHINE_TYPE --number NUMBER --private-vlan PRIVATE_VLAN --public-vlan PUBLIC_VLAN [--disable-disk-encrypt]
{: #cs_worker_add}

将工作程序节点添加到标准集群。

<strong>命令选项</strong>：

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>集群的名称或标识。此值是必需的。</dd>

<dt><code>--file <em>FILE_LOCATION</em></code></dt>
<dd>用于将工作程序节点添加到集群的 YAML 文件的路径。您可以使用 YAML 文件，而不使用此命令中提供的选项来定义其他工作程序节点。此值是可选的。

<p><strong>注</strong>：如果在命令中提供的选项与 YAML 文件中的参数相同，那么命令中的值将优先于 YAML 中的值。例如，您在 YAML 文件中定义了机器类型，并在命令中使用了 --machine-type 选项，那么在该命令选项中输入的值会覆盖 YAML 文件中的相应值。

      

<pre class="codeblock">
<code>name: <em>&lt;cluster_name_or_ID&gt;</em>
location: <em>&lt;location&gt;</em>
machine-type: <em>&lt;machine_type&gt;</em>
private-vlan: <em>&lt;private_VLAN&gt;</em>
public-vlan: <em>&lt;public_VLAN&gt;</em>
hardware: <em>&lt;shared_or_dedicated&gt;</em>
workerNum: <em>&lt;number_workers&gt;</em>
diskEncryption: <em>false</em></code></pre>

<table>
<caption>表 2. 了解 YAML 文件的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
</thead>
<tbody>
<tr>
<td><code><em>name</em></code></td>
<td>将 <code><em>&lt;cluster_name_or_ID&gt;</em></code> 替换为要在其中添加工作程序节点的集群的名称或标识。</td>
</tr>
<tr>
<td><code><em>location</em></code></td>
<td>将 <code><em>&lt;location&gt;</em></code> 替换为要部署工作程序节点的位置。可用的位置取决于您登录到的区域。要列出可用位置，请运行 <code>bx cs locations</code>。</td>
</tr>
<tr>
<td><code><em>machine-type</em></code></td>
<td>将 <code><em>&lt;machine_type&gt;</em></code> 替换为要将工作程序节点部署到的机器类型。可以将工作程序节点作为虚拟机部署在共享或专用硬件上，也可以作为物理机器部署在裸机上。可用的物理和虚拟机类型随集群的部署位置而变化。有关更多信息，请参阅 `bx cs machine-types` [命令](cs_cli_reference.html#cs_machine_types)。</td>
</tr>
<tr>
<td><code><em>private-vlan</em></code></td>
<td>将 <code><em>&lt;private_VLAN&gt;</em></code> 替换为要用于工作程序节点的专用 VLAN 的标识。要列出可用的 VLAN，请运行 <code>bx cs vlans <em>&lt;location&gt;</em></code> 并查找以 <code>bcr</code>（后端路由器）开头的 VLAN 路由器。</td>
</tr>
<tr>
<td><code>public-vlan</code></td>
<td>将 <code>&lt;public_VLAN&gt;</code> 替换为要用于工作程序节点的公用 VLAN 的标识。要列出可用的 VLAN，请运行 <code>bx cs vlans &lt;location&gt;</code> 并查找以 <code>fcr</code>（前端路由器）开头的 VLAN 路由器。<br><strong>注</strong>：{[private_VLAN_vyatta]}</td>
</tr>
<tr>
<td><code>hardware</code></td>
<td>对于虚拟机类型：工作程序节点的硬件隔离级别。如果希望可用的物理资源仅供您专用，请使用 dedicated，或者要允许物理资源与其他 IBM 客户共享，请使用 shared。缺省值为 shared。</td>
</tr>
<tr>
<td><code>workerNum</code></td>
<td>将 <code><em>&lt;number_workers&gt;</em></code> 替换为要部署的工作程序节点数。</td>
</tr>
<tr>
<td><code>diskEncryption: <em>false</em></code></td>
<td>工作程序节点缺省情况下具有磁盘加密功能：[了解更多](cs_secure.html#worker)。要禁用加密，请包括此选项并将值设置为 <code>false</code>。</td></tr>
</tbody></table></p></dd>

<dt><code>--hardware <em>HARDWARE</em></code></dt>
<dd>工作程序节点的硬件隔离级别。如果希望可用的物理资源仅供您专用，请使用 dedicated，或者要允许物理资源与其他 IBM 客户共享，请使用 shared。缺省值为 shared。此值是可选的。</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>选择机器类型。可以将工作程序节点作为虚拟机部署在共享或专用硬件上，也可以作为物理机器部署在裸机上。可用的物理和虚拟机类型随集群的部署位置而变化。有关更多信息，请参阅 `bx cs machine-types` [命令](cs_cli_reference.html#cs_machine_types)的文档。此值对于标准集群是必需的，且不可用于免费集群。</dd>

<dt><code>--number <em>NUMBER</em></code></dt>
<dd>整数，表示要在集群中创建的工作程序节点数。缺省值为 1。此值是可选的。</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>创建集群时指定的专用 VLAN。此值是必需的。

<p><strong>注：</strong>专用 VLAN 路由器始终以 <code>bcr</code>（后端路由器）开头，而公用 VLAN 路由器始终以 <code>fcr</code>（前端路由器）开头。创建集群并指定公用和专用 VLAN 时，在这些前缀之后的数字和字母组合必须匹配。</p></dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>创建集群时指定的公用 VLAN。此值是可选的。如果希望工作程序节点仅存在于专用 VLAN 上，请不要提供公用 VLAN 标识。<strong>注</strong>：{[private_VLAN_vyatta]}

<p><strong>注：</strong>专用 VLAN 路由器始终以 <code>bcr</code>（后端路由器）开头，而公用 VLAN 路由器始终以 <code>fcr</code>（前端路由器）开头。创建集群并指定公用和专用 VLAN 时，在这些前缀之后的数字和字母组合必须匹配。</p></dd>

<dt><code>--disable-disk-encrypt</code></dt>
<dd>工作程序节点缺省情况下具有磁盘加密功能：[了解更多](cs_secure.html#worker)。要禁用加密，请包括此选项。</dd>
</dl>

**示例**：

  ```
  bx cs worker-add --cluster my_cluster --number 3 --public-vlan my_public_VLAN_ID --private-vlan my_private_VLAN_ID --machine-type u2c.2x4 --hardware shared
  ```
  {: pre}

  {{site.data.keyword.Bluemix_dedicated_notm}} 的示例：

  ```
  bx cs worker-add --cluster my_cluster --number 3 --machine-type u2c.2x4
  ```
  {: pre}




### bx cs worker-get [CLUSTER_NAME_OR_ID] WORKER_NODE_ID
{: #cs_worker_get}

查看工作程序节点的详细信息。

<strong>命令选项</strong>：

   <dl>
   <dt><code><em>CLUSTER_NAME_OR_ID</em></code></dt>
   <dd>工作程序节点的集群的名称或标识。此值是可选的。</dd>
   <dt><code><em>WORKER_NODE_ID</em></code></dt>
   <dd>工作程序节点的名称。运行 <code>bx cs workers <em>CLUSTER</em></code> 可查看集群中工作程序节点的标识。此值是必需的。</dd>
   </dl>

**示例命令**：

  ```
  bx cs worker-get my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1
  ```
  {: pre}

**示例输出**：

  ```
  ID:           kube-dal10-123456789-w1
  State:        normal
  Status:       Ready
  Trust:        disabled
  Private VLAN: 223xxxx
  Public VLAN:  223xxxx
  Private IP:   10.xxx.xx.xxx
  Public IP:    169.xx.xxx.xxx
  Hardware:     shared
  Zone:         dal10
  Version:      1.8.11_1509
  ```
  {: screen}

### bx cs worker-reboot [-f][--hard] CLUSTER WORKER [WORKER]
{: #cs_worker_reboot}

重新引导集群中的工作程序节点。在重新引导期间，工作程序节点的状态不会更改。

**注意：**重新引导工作程序节点可能导致工作程序节点上发生数据损坏。请谨慎使用此命令，仅在确信重新引导可以帮助恢复工作程序节点时使用。在其他所有情况下，请改为[重新装入工作程序节点](#cs_worker_reload)。

在重新引导工作程序节点之前，请确保将 pod 重新安排到其他工作程序节点上，以帮助避免因工作程序节点上的应用程序或数据损坏而产生的停机时间。

1. 列出集群中的所有工作程序节点，并记下要重新引导的工作程序节点的 **name**。
   ```
kubectl get nodes
```
   此命令中返回的 **name** 是分配给工作程序节点的专用 IP 地址。运行 `bx cs workers <cluster_name_or_ID>` 命令，并查找具有相同 **Private IP** 地址的工作程序节点时，可以找到有关工作程序节点的更多信息。
2. 在称为“封锁”的过程中将工作程序节点标记为不可安排。封锁工作程序节点后，该节点即不可用于未来的 pod 安排。使用在上一步中检索到的工作程序节点的 **name**。
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. 验证是否已对工作程序节点禁用了 pod 安排。
   ```
kubectl get nodes
```
   {: pre}
   如果阶段状态显示为 **SchedulingDisabled**，说明已禁止工作程序节点用于 pod 安排。
 4. 强制从工作程序节点中除去 pod，并将其重新安排到集群中的剩余工作程序节点上。
    ```
    kubectl drain <worker_name>
    ```
    {: pre}
此过程可能需要几分钟时间。
 5. 重新引导工作程序节点。使用从 `bx cs workers <cluster_name_or_ID>` 命令返回的工作程序标识。
    ```
    bx cs worker-reboot <cluster_name_or_ID> <worker_name_or_ID>
    ```
    {: pre}
 6. 等待大约 5 分钟后，才能使工作程序节点可用于 pod 安排，以确保重新引导完成。在重新引导期间，工作程序节点的状态不会更改。工作程序节点的重新引导通常在几秒后完成。
 7. 使工作程序节点可用于 pod 安排。请使用从 `kubectl get nodes` 命令返回的工作程序节点的 **name**。
    ```
    kubectl uncordon <worker_name>
    ```
    {: pre}
    </br>

<strong>命令选项</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>集群的名称或标识。此值是必需的。</dd>

   <dt><code>-f</code></dt>
   <dd>使用此选项可强制重新启动工作程序节点，而不显示用户提示。此值是可选的。</dd>

   <dt><code>--hard</code></dt>
   <dd>使用此选项可通过切断工作程序节点的电源来强制硬重新启动该工作程序节点。如果工作程序节点无响应或工作程序节点有一个 Docker 挂起，请使用此选项。此值是可选的。</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>一个或多个工作程序节点的名称或标识。列出多个工作程序节点时使用空格分隔。此值是必需的。</dd>
   </dl>

**示例**：

  ```
  bx cs worker-reboot my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}


### bx cs worker-reload [-f] CLUSTER WORKER [WORKER]
{: #cs_worker_reload}

重新装入工作程序节点的所有必需配置。如果工作程序节点遇到问题（例如，性能降低），或者如果工作程序节点卡在非正常运行状态，那么重新装入会非常有用。

重新装入工作程序节点会将补丁版本更新应用于工作程序节点，但不会应用主要或次要更新。要查看从一个补丁版本到下一个补丁版本的更改，请查看[版本更改日志](cs_versions_changelog.html#changelog)文档。
{: tip}

在重新装入工作程序节点之前，请确保将 pod 重新安排到其他工作程序节点上，以帮助避免因工作程序节点上的应用程序或数据损坏而产生的停机时间。

1. 列出集群中的所有工作程序节点，并记下要重新装入的工作程序节点的 **name**。
   ```
kubectl get nodes
```
   此命令中返回的 **name** 是分配给工作程序节点的专用 IP 地址。运行 `bx cs workers <cluster_name_or_ID>` 命令，并查找具有相同 **Private IP** 地址的工作程序节点时，可以找到有关工作程序节点的更多信息。
2. 在称为“封锁”的过程中将工作程序节点标记为不可安排。封锁工作程序节点后，该节点即不可用于未来的 pod 安排。使用在上一步中检索到的工作程序节点的 **name**。
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. 验证是否已对工作程序节点禁用了 pod 安排。
   ```
kubectl get nodes
```
   {: pre}
   如果阶段状态显示为 **SchedulingDisabled**，说明已禁止工作程序节点用于 pod 安排。
 4. 强制从工作程序节点中除去 pod，并将其重新安排到集群中的剩余工作程序节点上。
    ```
    kubectl drain <worker_name>
    ```
    {: pre}
此过程可能需要几分钟时间。
 5. 重新装入工作程序节点。使用从 `bx cs workers <cluster_name_or_ID>` 命令返回的工作程序标识。
    ```
    bx cs worker-reload <cluster_name_or_ID> <worker_name_or_ID>
    ```
    {: pre}
 6. 等待重新装入完成。
 7. 使工作程序节点可用于 pod 安排。请使用从 `kubectl get nodes` 命令返回的工作程序节点的 **name**。         
    ```
    kubectl uncordon <worker_name>
    ```
</br>
<strong>命令选项</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>集群的名称或标识。此值是必需的。</dd>

   <dt><code>-f</code></dt>
   <dd>使用此选项可强制重新装入工作程序节点，而不显示用户提示。此值是可选的。</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>一个或多个工作程序节点的名称或标识。列出多个工作程序节点时使用空格分隔。此值是必需的。</dd>
   </dl>

**示例**：

  ```
  bx cs worker-reload my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}


### bx cs worker-rm [-f] CLUSTER WORKER [WORKER]
{: #cs_worker_rm}

从集群中除去一个或多个工作程序节点。如果除去工作程序节点，那么集群将变得不均衡。 

在除去工作程序节点之前，请确保将 pod 重新安排到其他工作程序节点上，以帮助避免因工作程序节点上的应用程序或数据损坏而产生的停机时间。
{: tip}

1. 列出集群中的所有工作程序节点，并记下要除去的工作程序节点的 **name**。
   ```
kubectl get nodes
```
   此命令中返回的 **name** 是分配给工作程序节点的专用 IP 地址。运行 `bx cs workers <cluster_name_or_ID>` 命令，并查找具有相同 **Private IP** 地址的工作程序节点时，可以找到有关工作程序节点的更多信息。
2. 在称为“封锁”的过程中将工作程序节点标记为不可安排。封锁工作程序节点后，该节点即不可用于未来的 pod 安排。使用在上一步中检索到的工作程序节点的 **name**。
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. 验证是否已对工作程序节点禁用了 pod 安排。
   ```
kubectl get nodes
```
   {: pre}
   如果阶段状态显示为 **SchedulingDisabled**，说明已禁止工作程序节点用于 pod 安排。
4. 强制从工作程序节点中除去 pod，并将其重新安排到集群中的剩余工作程序节点上。
   ```
    kubectl drain <worker_name>
    ```
   {: pre}
   此过程可能需要几分钟时间。
5. 除去工作程序节点。使用从 `bx cs workers <cluster_name_or_ID>` 命令返回的工作程序标识。
   ```
   bx cs worker-rm <cluster_name_or_ID> <worker_name_or_ID>
   ```
   {: pre}

6. 验证工作程序节点是否已除去。
   ```
       bx cs workers <cluster_name_or_ID>
       ```
</br>
<strong>命令选项</strong>：

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>集群的名称或标识。此值是必需的。</dd>

   <dt><code>-f</code></dt>
   <dd>使用此选项可强制除去工作程序节点，而不显示用户提示。此值是可选的。</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>一个或多个工作程序节点的名称或标识。列出多个工作程序节点时使用空格分隔。此值是必需的。</dd>
   </dl>

**示例**：

  ```
  bx cs worker-rm my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}




### bx cs worker-update [-f] CLUSTER WORKER [WORKER][--kube-version MAJOR.MINOR.PATCH] [--force-update]
{: #cs_worker_update}

更新工作程序节点以将最新的安全性更新和补丁应用于操作系统，并更新 Kubernetes 版本以与主节点的版本相匹配。可以使用 `bx cs cluster-update` [命令](cs_cli_reference.html#cs_cluster_update)来更新主节点 Kubernetes 版本。

**重要信息**：运行 `bx cs worker-update` 可能会导致应用程序和服务产生停机时间。更新期间，所有 pod 都将重新安排到其他工作程序节点，如果数据未存储在 pod 外部，那么将删除数据。为避免停机时间，请[确保在所选工作程序节点更新时有足够的工作程序节点来处理工作负载](cs_cluster_update.html#worker_node)。

在更新前，您可能需要更改 YAML 文件以进行部署。请查看此[发行说明](cs_versions.html)以了解详细信息。

<strong>命令选项</strong>：

   <dl>

   <dt><em>CLUSTER</em></dt>
   <dd>列出了其中可用工作程序节点的集群的名称或标识。此值是必需的。</dd>

   <dt><code>-f</code></dt>
   <dd>使用此选项可强制更新主节点，而不显示用户提示。此值是可选的。</dd>

   <dt><code>--force-update</code></dt>
   <dd>即便更改是跨 2 个以上的次版本，也仍尝试更新。此值是可选的。</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>一个或多个工作程序节点的标识。列出多个工作程序节点时使用空格分隔。此值是必需的。</dd>
   </dl>

**示例**：

  ```
  bx cs worker-update my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}



### bx cs workers CLUSTER [--show-deleted]
{: #cs_workers}

查看集群中工作程序节点的列表以及每个工作程序节点的状态。

<strong>命令选项</strong>：

   <dl>
   <dt><em>CLUSTER</em></dt>
   <dd>可用工作程序节点的集群的名称或标识。此值是必需的。</dd>
   <dt><em>--show-deleted</em></dt>
   <dd>查看从集群中删除的工作程序节点，包括删除原因。此值是可选的。</dd>
   </dl>

**示例**：

  ```
  bx cs workers my_cluster
  ```
  {: pre}
