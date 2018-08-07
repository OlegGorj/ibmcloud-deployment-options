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



# 集群联网故障诊断
{: #cs_troubleshoot_network}

在使用 {{site.data.keyword.containerlong}} 时，请考虑用于对集群联网进行故障诊断的以下方法。
{: shortdesc}

如果您有更常规的问题，请尝试[集群调试](cs_troubleshoot.html)。
{: tip}


## 无法通过 LoadBalancer 服务连接到应用程序
{: #cs_loadbalancer_fails}

{: tsSymptoms}
您已通过在集群中创建 LoadBalancer 服务来向公众公开应用程序。但尝试使用负载均衡器的公共 IP 地址连接到应用程序时，连接失败或超时。

{: tsCauses}
由于以下某种原因，LoadBalancer 服务可能未正常运行：

-   集群为免费集群，或者为仅具有一个工作程序节点的标准集群。
-   集群尚未完全部署。
-   LoadBalancer 服务的配置脚本包含错误。

{: tsResolve}
要对 LoadBalancer 服务进行故障诊断，请执行以下操作：

1.  检查是否设置了完全部署的标准集群，以及该集群是否至少有两个工作程序节点，以确保 LoadBalancer 服务具有高可用性。

  ```
       bx cs workers <cluster_name_or_ID>
       ```
  {: pre}

    在 CLI 输出中，确保工作程序节点的 **Status** 显示 **Ready**，并且 **Machine Type** 显示除了 **free** 之外的机器类型。

2.  检查 LoadBalancer 服务的配置文件是否准确。

    ```
    apiVersion: v1
    kind: Service
    metadata:
      name: myservice
    spec:
      type: LoadBalancer
      selector:
        <selector_key>:<selector_value>
      ports:
       - protocol: TCP
         port: 8080
    ```
    {: pre}

    1.  检查是否已将 **LoadBalancer** 定义为服务类型。
    2.  在 LoadBalancer 服务的 `spec.selector` 部分中，确保 `<selector_key>` 和 `<selector_value>` 与部署 YAML 的 `spec.template.metadata.labels` 部分中使用的键/值对相同。如果标签不匹配，LoadBalancer 服务中的 **Endpoints** 部分将显示 **<none>**，并且无法从因特网访问应用程序。
    3.  检查是否使用的是应用程序侦听的**端口**。

3.  检查 LoadBalancer 服务，并查看 **Events** 部分以查找潜在错误。

    ```
        kubectl describe service <myservice>
    ```
    {: pre}

    查找以下错误消息：
    

    <ul><li><pre class="screen"><code>Clusters with one node must use services of type NodePort</code></pre></br>要使用 LoadBalancer 服务，您必须有至少包含两个工作程序节点的标准集群。
    </li>
    <li><pre class="screen"><code>No cloud provider IPs are available to fulfill the load balancer service request. Add a portable subnet to the cluster and try again</code></pre></br>此错误消息指示没有任何可移植公共 IP 地址可供分配给 LoadBalancer 服务。请参阅<a href="cs_subnets.html#subnets">向集群添加子网</a>，以了解有关如何为集群请求可移植公共 IP 地址的信息。有可移植的公共 IP 地址可供集群使用后，将自动创建 LoadBalancer 服务。
    </li>
    <li><pre class="screen"><code>Requested cloud provider IP <cloud-provider-ip> is not available. The following cloud provider IPs are available: <available-cloud-provider-ips></code></pre></br>您使用 **loadBalancerIP** 部分为 LoadBalancer 服务定义了可移植公共 IP 地址，但此可移植公共 IP 地址在可移植公共子网中不可用。在配置脚本的 **loadBalancerIP** 部分中，除去现有 IP 地址，然后添加其中一个可用的可移植公共 IP 地址。您还可以从脚本中除去 **loadBalancerIP** 部分，以便可以自动分配可用的可移植公共 IP 地址。</li>
    <li><pre class="screen"><code>No available nodes for load balancer services</code></pre>您没有足够的工作程序节点可部署 LoadBalancer 服务。一个原因可能是您已部署了包含多个工作程序节点的标准集群，但供应这些工作程序节点失败。
    </li>
    <ol><li>列出可用的工作程序节点。</br><pre class="codeblock"><code>kubectl get nodes</code></pre></li>
    <li>如果找到了至少两个可用的工作程序节点，请列出工作程序节点详细信息。</br><pre class="codeblock"><code>bx cs worker-get [&lt;cluster_name_or_ID&gt;] &lt;worker_ID&gt;</code></pre></li>
    <li>确保分别由 <code>kubectl get nodes</code> 和 <code>bx cs [&lt;cluster_name_or_ID&gt;] worker-get</code> 命令返回的工作程序节点的公用和专用 VLAN 标识相匹配。</li></ol></li></ul>

4.  如果使用定制域来连接到 LoadBalancer 服务，请确保定制域已映射到 LoadBalancer 服务的公共 IP 地址。
    1.  找到 LoadBalancer 服务的公共 IP 地址。

        ```
        kubectl describe service <service_name> | grep "LoadBalancer Ingress"
        ```
        {: pre}

    2.  检查定制域是否已映射到指针记录 (PTR) 中 LoadBalancer 服务的可移植公共 IP 地址。

<br />




## 无法通过 Ingress 连接到应用程序
{: #cs_ingress_fails}

{: tsSymptoms}
您已通过为集群中的应用程序创建 Ingress 资源来向公众公开应用程序。但尝试使用 Ingress 应用程序负载均衡器 (ALB) 的公共 IP 地址或子域连接到应用程序时，连接失败或超时。

{: tsCauses}
由于以下原因，Ingress 可能未正常运行：
<ul><ul>
<li>集群尚未完全部署。
<li>集群设置为免费集群，或设置为仅具有一个工作程序节点的标准集群。
<li>Ingress 配置脚本包含错误。
</ul></ul>

{: tsResolve}
要对 Ingress 进行故障诊断，请执行以下操作：

1.  检查是否设置了完全部署的标准集群，以及该集群是否至少有两个工作程序节点，以确保 ALB 具有高可用性。

  ```
       bx cs workers <cluster_name_or_ID>
       ```
  {: pre}

    在 CLI 输出中，确保工作程序节点的 **Status** 显示 **Ready**，并且 **Machine Type** 显示除了 **free** 之外的机器类型。

2.  检索 ALB 子域和公共 IP 地址，然后对每一项执行 ping 操作。

    1.  检索 ALB 子域。

      ```
            bx cs cluster-get <cluster_name_or_ID> | grep "Ingress subdomain"
      ```
      {: pre}

    2.  对 ALB 子域执行 ping 操作。

      ```
      ping <ingress_subdomain>
      ```
      {: pre}

    3.  检索 ALB 的公共 IP 地址。

      ```
      nslookup <ingress_subdomain>
      ```
      {: pre}

    4.  对 ALB 公共 IP 地址执行 ping 操作。

      ```
      ping <ALB_IP>
      ```
      {: pre}

    如果对于 ALB 的公共 IP 地址或子域，CLI 返回超时，并且您已设置定制防火墙来保护工作程序节点，请在[防火墙](cs_troubleshoot_clusters.html#cs_firewall)中打开更多端口和联网组。

3.  如果使用的是定制域，请确保定制域已通过 DNS 提供者映射到 IBM 提供的 ALB 的公共 IP 地址或子域。
    1.  如果使用的是 ALB 子域，请检查规范名称记录 (CNAME)。
    2.  如果使用的是 ALB 公共 IP 地址，请检查定制域是否已映射到指针记录 (PTR) 中的可移植公共 IP 地址。
4.  检查 Ingress 资源配置文件。

    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingress
    spec:
      tls:
      - hosts:
        - <ingress_subdomain>
        secretName: <ingress_tls_secret>
      rules:
      - host: <ingress_subdomain>
        http:
          paths:
          - path: /
            backend:
              serviceName: myservice
              servicePort: 80
    ```
    {: codeblock}

    1.  检查 ALB 子域和 TLS 证书是否正确。要查找 IBM 提供的子域和 TLS 证书，请运行 `bx cs cluster-get <cluster_name_or_ID>`.
    2.  确保应用程序侦听的是在 Ingress 的 **path** 部分中配置的路径。如果应用程序设置为侦听根路径，请包含 **/** 以作为路径。
5.  检查 Ingress 部署，并查找潜在的警告或错误消息。

    ```
      kubectl describe ingress <myingress>
  ```
    {: pre}

    例如，在输出的 **Events** 部分中，您可能会看到警告消息，提醒您所使用的 Ingress 资源或某些注释中有无效的值。

    ```
    Name:             myingress
    Namespace:        default
    Address:          169.xx.xxx.xxx,169.xx.xxx.xxx
    Default backend:  default-http-backend:80 (<none>)
    Rules:
      Host                                             Path  Backends
      ----                                             ----  --------
      mycluster.us-south.containers.appdomain.cloud
                                                       /tea      myservice1:80 (<none>)
                                                       /coffee   myservice2:80 (<none>)
    Annotations:
      custom-port:        protocol=http port=7490; protocol=https port=4431
      location-modifier:  modifier='~' serviceName=myservice1;modifier='^~' serviceName=myservice2
    Events:
      Type     Reason             Age   From                                                            Message
      ----     ------             ----  ----                                                            -------
      Normal   Success            1m    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  TLSSecretNotFound  1m    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress resource.
      Normal   Success            59s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  AnnotationError    40s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress.bluemix.net/custom-port annotation. Error annotation format error : One of the mandatory fields not valid/missing for annotation ingress.bluemix.net/custom-port
      Normal   Success            40s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  AnnotationError    2s    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress.bluemix.net/custom-port annotation. Invalid port 7490. Annotation cannot use ports 7481 - 7490
      Normal   Success            2s    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
    ```
    {: screen}

6.  检查 ALB 的日志。
    1.  检索正在集群中运行的 Ingress pod 的标识。

      ```
      kubectl get pods -n kube-system | grep alb
      ```
      {: pre}

    2.  检索每个 Ingress pod 的日志。

      ```
            kubectl logs <ingress_pod_ID> nginx-ingress -n kube-system
      ```
      {: pre}

    3.  在 ALB 日志中查找错误消息。

<br />


## Ingress 应用程序负载均衡器私钥问题
{: #cs_albsecret_fails}

{: tsSymptoms}
将 Ingress 应用程序负载均衡器 (ALB) 私钥部署到集群后，在 {{site.data.keyword.cloudcerts_full_notm}} 中查看证书时，`Description` 字段未使用该私钥名称进行更新。

列出有关 ALB 私钥的信息时，阶段状态为 `*_failed`。例如，`create_failed`、`update_failed` 或 `delete_failed`。

{: tsResolve}
查看以下导致 ALB 私钥可能失败的原因以及对应的故障诊断步骤：

<table>
<caption>对 Ingress 应用程序负载均衡器私钥进行故障诊断</caption>
 <thead>
 <th>问题原因</th>
 <th>解决方法</th>
 </thead>
 <tbody>
 <tr>
 <td>您没有下载和更新证书数据所需的访问角色。</td>
 <td>请咨询帐户管理员，要求为您分配对 {{site.data.keyword.cloudcerts_full_notm}} 实例的**操作员**和**编辑者**角色。有关更多信息，请参阅 {{site.data.keyword.cloudcerts_short}} 的<a href="/docs/services/certificate-manager/access-management.html#managing-service-access-roles">管理服务访问</a>。</td>
 </tr>
 <tr>
 <td>创建、更新或除去时提供的证书 CRN 所属的帐户与集群不同。</td>
 <td>检查提供的证书 CRN 导入到的 {{site.data.keyword.cloudcerts_short}} 服务实例是否与集群部署在同一帐户中。</td>
 </tr>
 <tr>
 <td>创建时提供的证书 CRN 不正确。</td>
 <td><ol><li>检查提供的证书 CRN 字符串的准确性。</li><li>如果发现证书 CRN 是准确的，请尝试更新私钥：<code>bx cs alb-cert-deploy --update --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li><li>如果此命令生成 <code>update_failed</code> 阶段状态，请除去私钥：<code>bx cs alb-cert-rm --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt;</code></li><li>重新部署私钥：<code>bx cs alb-cert-deploy --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li></ol></td>
 </tr>
 <tr>
 <td>更新时提供的证书 CRN 不正确。</td>
 <td><ol><li>检查提供的证书 CRN 字符串的准确性。</li><li>如果发现证书 CRN 是准确的，请除去私钥：<code>bx cs alb-cert-rm --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt;</code></li><li>重新部署私钥：<code>bx cs alb-cert-deploy --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li><li>尝试更新私钥：<code>bx cs alb-cert-deploy --update --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li></ol></td>
 </tr>
 <tr>
 <td>{{site.data.keyword.cloudcerts_long_notm}} 服务遭遇停机时间。</td>
 <td>检查 {{site.data.keyword.cloudcerts_short}} 服务是否已启动并在运行。</td>
 </tr>
 </tbody></table>

<br />


## 无法获取 Ingress ALB 的子域
{: #cs_subnet_limit}

{: tsSymptoms}
运行 `bx cs cluster-get <cluster>` 时，集群处于 `normal` 状态，但没有 **Ingress 子域**可用。

您可能会看到类似于以下内容的错误消息。

```
There are already the maximum number of subnets permitted in this VLAN.
```
{: screen}

{: tsCauses}
创建集群时，将在指定的 VLAN 上请求 8 个公用和 8 个专用可移植子网。对于 {{site.data.keyword.containershort_notm}}，VLAN 限制为 40 个子网。如果集群的 VLAN 已达到该限制，那么供应 **Ingress 子域**会失败。

要查看 VLAN 的子网数，请执行以下操作：
1.  在 [IBM Cloud Infrastructure (SoftLayer) 控制台](https://control.bluemix.net/)中，选择**网络** > **IP 管理** > **VLAN**。
2.  单击用于创建集群的 VLAN 的 **VLAN 编号**。查看**子网**部分以了解是否存在 40 个或更多子网。

{: tsResolve}
如果需要新的 VLAN，请通过[联系 {{site.data.keyword.Bluemix_notm}} 支持](/docs/get-support/howtogetsupport.html#getting-customer-support)进行订购。然后，[创建集群](cs_cli_reference.html#cs_cluster_create)以使用这一新的 VLAN。

如果有其他 VLAN 可用，那么可以在现有集群中[设置 VLAN 生成](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning)。在此之后，即可将新的工作程序节点添加到集群，这些节点将使用具有可用子网的其他 VLAN。

如果并未使用 VLAN 中的所有子网，那么可以在集群中复用子网。
1.  检查要使用的子网是否可用。**注**：使用的 Infrastructure 帐户可能在多个 {{site.data.keyword.Bluemix_notm}} 帐户之间共享。在这种情况下，即便运行 `bx cs subnets` 命令来查看 **Bound Cluster** 的子网，也只能看到您的集群的信息。请与 Infrastructure 帐户所有者核实以确保这些子网可用，并且未由其他任何帐户或团队使用。

2.  使用 `--no-subnet` 选项[创建集群](cs_cli_reference.html#cs_cluster_create)，以便该服务不会尝试创建新的子网。指定位置和具有可供复用的子网的 VLAN。

3.  使用 `bx cs cluster-subnet-add` [命令](cs_cli_reference.html#cs_cluster_subnet_add)将现有子网添加到集群。有关更多信息，请参阅[在 Kubernetes 集群中添加或复用定制和现有子网](cs_subnets.html#custom)。

<br />


## 无法与 strongSwan Helm 图表建立 VPN 连接
{: #cs_vpn_fails}

{: tsSymptoms}
通过运行 `kubectl exec -n kube-system  $STRONGSWAN_POD -- ipsec status` 来检查 VPN 连接时，未看到阶段状态 `ESTABLISHED`，或者 VPN pod 处于 `ERROR` 状态或继续崩溃并重新启动。

{: tsCauses}
Helm 图表配置文件具有不正确的值、缺少值或有语法错误。

{: tsResolve}
尝试使用 strongSwan Helm 图表建立 VPN 连接时，很有可能 VPN 阶段状态一开始不是 `ESTABLISHED`。您可能需要检查多种类型的问题，并相应地更改配置文件。要对 strongSwan VPN 连接进行故障诊断，请执行以下操作：

1. 针对配置文件中的设置检查内部部署 VPN 端点设置。如果设置不匹配，请执行以下操作：

    <ol>
    <li>删除现有的 Helm 图表。</br><pre class="codeblock"><code>helm delete --purge <release_name></code></pre></li>
    <li>修正 <code>config.yaml</code> 文件中不正确的值，并保存更新的文件。</li>
    <li>安装新的 Helm 图表。</br><pre class="codeblock"><code>helm install -f config.yaml --namespace=kube-system --name=<release_name> bluemix/strongswan</code></pre></li>
    </ol>

2. 如果 VPN pod 处于 `ERROR` 状态或继续崩溃并重新启动，那么可能是由于在图表的配置映射中对 `ipsec.conf` 设置进行了参数验证。

    <ol>
    <li>检查 strongSwan pod 日志中是否有任何验证错误。</br><pre class="codeblock"><code>kubectl logs -n kube-system $STRONGSWAN_POD</code></pre></li>
    <li>如果日志包含验证错误，请删除现有 Helm 图表。</br><pre class="codeblock"><code>helm delete --purge <release_name></code></pre></li>
    <li>修正 `config.yaml` 文件中不正确的值，并保存更新的文件。</li>
    <li>安装新的 Helm 图表。</br><pre class="codeblock"><code>helm install -f config.yaml --namespace=kube-system --name=<release_name> bluemix/strongswan</code></pre></li>
    </ol>

3. 运行 strongSwan 图表定义中包含的 5 个 Helm 测试。

    <ol>
    <li>运行 Helm 测试。</br><pre class="codeblock"><code>helm test vpn</code></pre></li>
    <li>如果任何测试失败，请参阅[了解 Helm VPN 连接测试](cs_vpn.html#vpn_tests_table)，以获取有关每个测试的信息以及测试可能失败的原因。<b>注</b>：某些测试有一些要求，而这些要求在 VPN 配置中是可选设置。如果某些测试失败，失败可能是可接受的，具体取决于您是否指定了这些可选设置。</li>
    <li>通过查看测试 pod 的日志来查看失败测试的输出。<br><pre class="codeblock"><code>kubectl logs -n kube-system <test_program></code></pre></li>
    <li>删除现有的 Helm 图表。</br><pre class="codeblock"><code>helm delete --purge <release_name></code></pre></li>
    <li>修正 <code>config.yaml</code> 文件中不正确的值，并保存更新的文件。</li>
    <li>安装新的 Helm 图表。</br><pre class="codeblock"><code>helm install -f config.yaml --namespace=kube-system --name=<release_name> bluemix/strongswan</code></pre></li>
    <li>要检查更改，请执行以下操作：<ol><li>获取当前测试 pod。</br><pre class="codeblock"><code>kubectl get pods -a -n kube-system -l app=strongswan-test</code></pre></li><li>清除当前测试 pod。</br><pre class="codeblock"><code>kubectl delete pods -n kube-system -l app=strongswan-test</code></pre></li><li>重新运行测试。</br><pre class="codeblock"><code>helm test vpn</code></pre></li>
    </ol></ol>

4. 运行在 VPN pod 映像内打包的 VPN 调试工具。

    1. 设置 `STRONGSWAN_POD` 环境变量。

        ```
                export STRONGSWAN_POD=$(kubectl get pod -n kube-system -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
        ```
        {: pre}

    2. 运行调试工具。

        ```
                kubectl exec -n kube-system  $STRONGSWAN_POD -- vpnDebug
        ```
        {: pre}

        该工具在对常见联网问题运行各种测试时，会输出多页信息。以 `ERROR`、`WARNING`、`VERIFY` 或 `CHECK` 开头的输出行指示 VPN 连接可能存在错误。

    <br />


## 添加或删除工作程序节点后，strongSwan VPN 连接失败
{: #cs_vpn_fails_worker_add}

{: tsSymptoms}
先前已使用 strongSwan IPSec VPN 服务建立了有效的 VPN 连接。但是，在集群上添加或删除工作程序节点后，遇到了下列一种或多种症状：

* VPN 的阶段状态不为 `ESTABLISHED`
* 无法从内部部署网络访问新工作程序节点
* 无法从新工作程序节点上运行的 pod 访问远程网络

{: tsCauses}
如果添加了工作程序节点：

* 工作程序节点在新的专用子网上供应，该子网未由现有 `localSubnetNAT` 或 `local.subnet` 设置通过 VPN 连接公开
* 无法将 VPN 路径添加到工作程序节点，因为工作程序具有未包含在现有 `tolerations` 或 `nodeSelector` 设置中的污点或标签
* VPN pod 在新的工作程序节点上运行，但该工作程序节点的公共 IP 地址不允许通过内部部署防火墙访问

如果删除了工作程序节点：

* 由于对现有 `tolerations` 或 `nodeSelector` 设置中的特定污点或标签存在限制，因此该工作程序节点是唯一在运行 VPN pod 的节点

{: tsResolve}
更新 Helm 图表值以反映工作程序节点更改：

1. 删除现有的 Helm 图表。

    ```
        helm delete --purge <release_name>
    ```
    {: pre}

2. 打开 strongSwan VPN 服务的配置文件。

    ```
        helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. 检查以下设置并根据需要进行更改，以反映出已删除或已添加的工作程序节点。

    如果添加了工作程序节点：

    <table>
    <caption>工作程序节点设置</caption?
     <thead>
     <th>设置</th>
     <th>描述</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>添加的工作程序可能部署在新的专用子网上，该子网不同于其他工作程序节点所在的其他现有子网。如果是使用子网 NAT 来重新映射集群的专用本地 IP 地址，并且在新子网上添加了工作程序，请将新的子网 CIDR 添加到此设置。</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>如果先前将 VPN pod 部署仅限于具有特定标签的工作程序，请确保添加的工作程序节点也具有该标签。</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>如果添加的工作程序节点已有污点，请更改此设置以允许 VPN pod 在具有任何污点或特定污点的所有有污点的工作程序上运行。</td>
     </tr>
     <tr>
     <td><code>local.subnet</code></td>
     <td>添加的工作程序可能部署在新的专用子网上，该子网不同于其他工作程序所在的现有子网。如果应用程序是由专用网络上的 NodePort 或 LoadBalancer 服务公开的，并且应用程序位于添加的工作程序上，请将新的子网 CIDR 添加到此设置。**注**：如果将值添加到 `local.subnet`，请检查内部部署子网的 VPN 设置，以确定是否还必须更新这些设置。</td>
     </tr>
     </tbody></table>

    如果删除了工作程序节点：

    <table>
    <caption>工作程序节点设置</caption>
     <thead>
     <th>设置</th>
     <th>描述</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>如果是使用子网 NAT 来重新映射特定专用本地 IP 地址，请从此设置中除去来自旧工作程序的任何 IP 地址。如果是使用子网 NAT 来重新映射整个子网，并且子网上没有任何工作程序存在，请从此设置中除去该子网 CIDR。</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>如果先前将 VPN pod 部署仅限于单个工作程序，并且删除了该工作程序，请将此设置更改为允许 VPN pod 在其他工作程序上运行。</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>如果删除的工作程序没有污点，而唯一保留的工作程序有污点，请将此设置更改为允许 VPN pod 在具有任何污点或特定污点的工作程序上运行。</td>
     </tr>
     </tbody></table>

4. 使用更新的值安装新 Helm 图表。

    ```
        helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/strongswan
    ```
    {: pre}

5. 检查图表部署状态。当图表就绪时，输出顶部附近的 **STATUS** 字段的值为 `DEPLOYED`。

    ```
        helm status <release_name>
    ```
    {: pre}

6. 在某些情况下，可能需要更改内部部署设置和防火墙设置，以匹配对 VPN 配置文件的更改。

7. 启动 VPN。
    * 如果 VPN 连接是由集群启动的（`ipsec.auto` 设置为 `start`），请先在内部部署网关上启动 VPN，然后在集群上启动 VPN。
    * 如果 VPN 连接是由内部部署网关启动的（`ipsec.auto` 设置为 `auto`），请先在集群上启动 VPN，然后在内部部署网关上启动 VPN。

8. 设置 `STRONGSWAN_POD` 环境变量。

    ```
        export STRONGSWAN_POD=$(kubectl get pod -n kube-system -l app=strongswan,release=<release_name> -o jsonpath='{ .items[0].metadata.name }')
    ```
    {: pre}

9. 检查 VPN 的状态。

    ```
            kubectl exec -n kube-system  $STRONGSWAN_POD -- ipsec status
        ```
    {: pre}

    * 如果 VPN 连接的阶段状态为 `ESTABLISHED`，说明 VPN 连接成功。无需进一步操作。

    * 如果仍存在连接问题，请参阅[无法建立与 stronSwan Helm 图表的 VPN 连接](#cs_vpn_fails)，以进一步对 VPN 连接进行故障诊断。

<br />



## 无法检索 Calico 网络策略
{: #cs_calico_fails}

{: tsSymptoms}
尝试通过运行 `calicoctl get policy` 来查看集群中的 Calico 网络策略时，会收到下列其中一个意外结果或错误消息：
- 空列表
- 旧 Calico V2 策略的列表，而不是 V3 策略的列表
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'projectcalico.org/v3'`（创建 Calico API 客户机失败：calicoctl.cfg 中存在语法错误：配置文件无效：未知的 APIVersion“projectcalico.org/v3”）

尝试通过运行 `calicoctl get GlobalNetworkPolicy` 来查看集群中的 Calico 网络策略时，会收到下列其中一个意外结果或错误消息：
- 空列表
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'v1'`（创建 Calico API 客户机失败：calicoctl.cfg 中存在语法错误：配置文件无效：未知的 APIVersion“v1”）
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'projectcalico.org/v3'`（创建 Calico API 客户机失败：calicoctl.cfg 中存在语法错误：配置文件无效：未知的 APIVersion“projectcalico.org/v3”）
- `Failed to get resources: Resource type 'GlobalNetworkPolicy' is not supported`（获取资源失败：不支持资源类型“GlobalNetworkPolicy”）

{: tsCauses}
要使用 Calico 策略，下面四个因素必须全部符合要求：集群 Kubernetes 版本、Calico CLI 版本、Calico 配置文件语法和查看策略命令。其中一个或多个因素的版本不正确。

{: tsResolve}
如果集群处于 [Kubernetes V1.10 或更高版本](cs_versions.html)，那么必须使用 Calico CLI V3.1、`calicoctl.cfg` V3 配置文件语法以及 `calicoctl get GlobalNetworkPolicy` 和 `calicoctl get NetworkPolicy` 命令。

如果集群处于 [Kubernetes V1.9 或更低版本](cs_versions.html)，那么必须使用 Calico CLI V1.6.3、`calicoctl.cfg` V2 配置文件语法以及 `calicoctl get policy` 命令。

要确保所有 Calico 因素都符合要求，请执行以下操作：

1. 查看集群 Kubernetes 版本。
    ```
    bx cs cluster-get <cluster_name>
    ```
    {: pre}

    * 如果集群处于 Kubernetes V1.10 或更高版本：
        1. [安装和配置 Calico CLI V3.1.1](cs_network_policy.html#1.10_install)。配置包括手动更新 `calicoctl.cfg` 文件以使用 Calico V3 语法。
        2. 确保您创建并要应用于集群的任何策略都使用 [Calico V3 语法 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/networkpolicy)。如果在 Calico V2 语法中具有现有策略 `.yaml` 或 `.json` 文件，那么可以使用 [`calicoctl convert` 命令 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.projectcalico.org/v3.1/reference/calicoctl/commands/convert) 将其转换为 Calico V3 语法。
        3. 要[查看策略](cs_network_policy.html#1.10_examine_policies)，请确保对于全局策略，使用 `calicoctl get GlobalNetworkPolicy`，对于作用域限定为特定名称空间的策略，使用 `calicoctl get NetworkPolicy --namespace<policy_namespace>`。

    * 如果集群处于 Kubernetes V1.9 或更低版本：
        1. [安装和配置 Calico CLI V1.6.3](cs_network_policy.html#1.9_install)。确保 `calicoctl.cfg` 文件使用 Calico V2 语法。
        2. 确保您创建并要应用于集群的任何策略都使用 [Calico V2 语法 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.projectcalico.org/v2.6/reference/calicoctl/resources/policy)。
        3. 要[查看策略](cs_network_policy.html#1.9_examine_policies)，请确保使用的是 `calicoctl get policy`。

在将集群从 Kubernetes V1.9 或更低版本更新到 V1.10 或更高版本之前，请查看[准备更新到 Calico V3](cs_versions.html#110_calicov3)。
{: tip}

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

