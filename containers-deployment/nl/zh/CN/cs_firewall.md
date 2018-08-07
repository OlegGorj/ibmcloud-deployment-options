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




# 在防火墙中打开必需的端口和 IP 地址
{: #firewall}

查看以下情况，在这些情况下，您可能需要在防火墙中为 {{site.data.keyword.containerlong}} 打开特定端口和 IP 地址。
{:shortdesc}

* 在企业网络策略阻止通过代理或防火墙访问公用因特网端点时从本地系统[运行 `bx` 命令](#firewall_bx)。
* 在企业网络策略阻止通过代理或防火墙访问公用因特网端点时从本地系统[运行 `kubectl` 命令](#firewall_kubectl)。
* 在企业网络策略阻止通过代理或防火墙访问公用因特网端点时从本地系统[运行 `calicoctl` 命令](#firewall_calicoctl)。
* 为工作程序节点设置防火墙或在 IBM Cloud infrastructure (SoftLayer) 帐户中定制防火墙设置时，[允许 Kubernetes 主节点与工作程序节点之间进行通信](#firewall_outbound)。
* [从集群外部访问 NodePort 服务、LoadBalancer 服务或 Ingress](#firewall_inbound)。

<br />


## 从防火墙后运行 `bx cs` 命令
{: #firewall_bx}

如果企业网络策略阻止通过代理或防火墙从本地系统访问公共端点，那么要运行 `bx cs` 命令，必须允许 {{site.data.keyword.containerlong_notm}} 的 TCP 访问。
{:shortdesc}

1. 允许在端口 443 上访问 `containers.bluemix.net`。
2. 验证连接。如果正确配置了访问权，那么会在输出中显示船。
   ```
   curl https://containers.bluemix.net/v1/
   ```
   {: pre}

   输出示例：
   ```
                                     )___(
                              _______/__/_
                     ___     /===========|   ___
    ____       __   [\\\]___/____________|__[///]   __
    \   \_____[\\]__/___________________________\__[//]___
     \                                                    |
      \                                                  /
   ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ```
   {: screen}

<br />


## 从防火墙后运行 `kubectl` 命令
{: #firewall_kubectl}

如果企业网络策略阻止通过代理或防火墙从本地系统访问公共端点，那么要运行 `kubectl` 命令，必须允许集群的 TCP 访问。
{:shortdesc}

创建集群时，将从 20000-32767 中随机分配主 URL 中的端口。您可以选择为可能创建的任何集群打开端口范围 20000-32767，也可以选择允许对特定现有集群进行访问。

开始之前，允许访问以[运行 `bx cs` 命令](#firewall_bx)。

要允许访问特定集群：

1. 登录到 {{site.data.keyword.Bluemix_notm}} CLI。根据提示，输入您的 {{site.data.keyword.Bluemix_notm}} 凭证。如果您有联合帐户，请包括 `--sso` 选项。

   ```
    bx login [--sso]
    ```
   {: pre}

2. 选择集群所在的区域。

   ```
   bx cs region-set
   ```
   {: pre}

3. 获取集群的名称。

   ```
        bx cs clusters
        ```
   {: pre}

4. 检索集群的**主 URL**。

   ```
   bx cs cluster-get <cluster_name_or_ID>
   ```
   {: pre}

   输出示例：
   ```
   ...
   Master URL:		https://169.xx.xxx.xxx:31142
   ...
   ```
   {: screen}

5. 允许访问端口上的**主 URL**，例如，先前示例中的端口 `31142`。

6. 验证连接。

   ```
   curl --insecure <master_URL>/version
   ```
   {: pre}

   示例命令：
   ```
   curl --insecure https://169.xx.xxx.xxx:31142/version
   ```
   {: pre}

   输出示例：
   ```
   {
     "major": "1",
     "minor": "7+",
     "gitVersion": "v1.7.4-2+eb9172c211dc41",
     "gitCommit": "eb9172c211dc4108341c0fd5340ee5200f0ec534",
     "gitTreeState": "clean",
     "buildDate": "2017-11-16T08:13:08Z",
     "goVersion": "go1.8.3",
     "compiler": "gc",
     "platform": "linux/amd64"
   }
   ```
   {: screen}

7. 可选：对您需要显示的每个集群重复上述步骤。

<br />


## 从防火墙后运行 `calicoctl` 命令
{: #firewall_calicoctl}

如果企业网络策略阻止通过代理或防火墙从本地系统访问公共端点，那么要运行 `calicoctl` 命令，必须允许 Calico 命令的 TCP 访问。
{:shortdesc}

开始之前，允许访问以运行 [`bx` 命令](#firewall_bx)和 [`kubectl` 命令](#firewall_kubectl)。

1. 从用于允许 [`kubectl` 命令](#firewall_kubectl)的主 URL 中检索 IP 地址。

2. 获取 ETCD 的端口。

  ```
  kubectl get cm -n kube-system calico-config -o yaml | grep etcd_endpoints
  ```
  {: pre}

3. 允许通过主 URL IP 地址和 ETCD 端口访问 Calico 策略。

<br />


## 允许集群访问基础架构资源和其他服务
{: #firewall_outbound}

支持集群从防火墙后访问基础架构资源和服务，例如 {{site.data.keyword.containershort_notm}} 区域、{{site.data.keyword.registrylong_notm}}、{{site.data.keyword.monitoringlong_notm}}、{{site.data.keyword.loganalysislong_notm}}、IBM Cloud Infrastructure (SoftLayer) 专用 IP 以及用于持久性卷申领的流出访问。
{:shortdesc}

  1.  记下用于集群中所有工作程序节点的公共 IP 地址。

      ```
             bx cs workers <cluster_name_or_ID>
       ```
      {: pre}

  2.  允许从源 _<each_worker_node_publicIP>_ 到目标 TCP/UDP 端口范围 20000-32767 和端口 443 以及以下 IP 地址和网络组的出站网络流量。如果您拥有的公司防火墙阻止您的本地机器访问公用因特网端点，请对源工作程序节点和本地机器执行此步骤。
      - **重要事项**：针对区域内的所有位置，必须允许出站流量从端口 443 流出，以便在引导过程中均衡负载。例如，如果集群位于美国南部，那么必须允许流量从端口 443 流至所有位置（dal10、dal12 和 dal13）的 IP 地址。
      <p>
  <table summary="表中的第一行跨两列。其余行应从左到右阅读，其中第一列是服务器位置，第二列是要匹配的 IP 地址。">
  <caption>要为出局流量打开的 IP 地址</caption>
      <thead>
      <th>区域</th>
      <th>位置</th>
      <th>IP 地址</th>
      </thead>
    <tbody>
      <tr>
        <td>亚太地区北部</td>
        <td>hkg02<br>seo01<br>sng01<br>tok02</td>
        <td><code>169.56.132.234</code><br><code>169.56.69.242</code><br><code>161.202.186.226</code><br><code>161.202.126.210</code></td>
       </tr>
      <tr>
         <td>亚太地区南部</td>
         <td>mel01<br>syd01<br>syd04</td>
         <td><code>168.1.97.67</code><br><code>168.1.8.195</code><br><code>130.198.64.19, 130.198.66.34</code></td>
      </tr>
      <tr>
         <td>欧洲中部</td>
         <td>ams03<br>fra02<br>mil01<br>par01</td>
         <td><code>169.50.169.110, 169.50.154.194</code><br><code>169.50.56.174</code><br><code>159.122.190.98</code><br><code>159.8.86.149, 159.8.98.170</code></td>
        </tr>
      <tr>
        <td>英国南部</td>
        <td>lon02<br>lon04</td>
        <td><code>159.122.242.78</code><br><code>158.175.65.170, 158.175.74.170, 158.175.76.2</code></td>
      </tr>
      <tr>
        <td>美国东部</td>
         <td>mon01<br>tor01<br>wdc06<br>wdc07</td>
         <td><code>169.54.126.219</code><br><code>169.53.167.50</code><br><code>169.60.73.142</code><br><code>169.61.83.62</code></td>
      </tr>
      <tr>
        <td>美国南部</td>
        <td>dal10<br>dal12<br>dal13<br>hou02<br>sao01</td>
        <td><code>169.47.234.18, 169.46.7.238</code><br><code>169.47.70.10</code><br><code>169.60.128.2</code><br><code>184.173.44.62</code><br><code>169.57.151.10</code></td>
      </tr>
      </tbody>
    </table>
</p>

  3.  允许出站网络流量从工作程序节点流至 [{{site.data.keyword.registrylong_notm}} 区域](/docs/services/Registry/registry_overview.html#registry_regions)：
      - `TCP port 443 FROM <each_worker_node_publicIP> TO <registry_publicIP>`
      - 将 <em>&lt;registry_publicIP&gt;</em> 替换为要允许流量流至的注册表 IP 地址。全局注册表存储 IBM 提供的公共映像，区域注册表存储您自己的专用或公共映像。
        <p>
<table summary="表中的第一行跨两列。其余行应从左到右阅读，其中第一列是服务器位置，第二列是要匹配的 IP 地址。">
  <caption>要为注册表流量打开的 IP 地址</caption>
      <thead>
        <th>{{site.data.keyword.containershort_notm}} 区域</th>
        <th>注册表地址</th>
        <th>注册表 IP 地址</th>
      </thead>
      <tbody>
        <tr>
          <td>跨 {{site.data.keyword.containershort_notm}} 区域的全局注册表</td>
          <td>registry.bluemix.net</td>
          <td><code>169.60.72.144/28</code><br><code>169.61.76.176/28</code></td>
        </tr>
        <tr>
          <td>亚太地区北部和亚太地区南部</td>
          <td>registry.au-syd.bluemix.net</td>
          <td><code>168.1.45.160/27</code></br><code>168.1.139.32/27</code></td>
        </tr>
        <tr>
          <td>欧洲中部</td>
          <td>registry.eu-de.bluemix.net</td>
          <td><code>169.50.56.144/28</code></br><code>159.8.73.80/28</code></td>
         </tr>
         <tr>
          <td>英国南部</td>
          <td>registry.eu-gb.bluemix.net</td>
          <td><code>159.8.188.160/27</code></br><code>169.50.153.64/27</code></td>
         </tr>
         <tr>
          <td>美国东部和美国南部</td>
          <td>registry.ng.bluemix.net</td>
          <td><code>169.55.39.112/28</code></br><code>169.46.9.0/27</code></br><code>169.55.211.0/27</code></td>
         </tr>
        </tbody>
      </table>
</p>

  4.  可选：允许出站网络流量从工作程序节点流至 {{site.data.keyword.monitoringlong_notm}} 和 {{site.data.keyword.loganalysislong_notm}} 服务：
      - `TCP port 443, port 9095 FROM <each_worker_node_public_IP> TO <monitoring_public_IP>`
      - 将 <em>&lt;monitoring_public_IP&gt;</em> 替换为要允许流量流至的监视区域的所有地址：
        <p><table summary="表中的第一行跨两列。其余行应从左到右阅读，其中第一列是服务器位置，第二列是要匹配的 IP 地址。">
  <caption>要为监视流量打开的 IP 地址</caption>
        <thead>
        <th>{{site.data.keyword.containershort_notm}} 区域</th>
        <th>监视地址</th>
        <th>监视 IP 地址</th>
        </thead>
      <tbody>
        <tr>
         <td>欧洲中部</td>
         <td>metrics.eu-de.bluemix.net</td>
         <td><code>159.122.78.136/29</code></td>
        </tr>
        <tr>
         <td>英国南部</td>
         <td>metrics.eu-gb.bluemix.net</td>
         <td><code>169.50.196.136/29</code></td>
        </tr>
        <tr>
          <td>美国东部、美国南部和亚太地区北部</td>
          <td>metrics.ng.bluemix.net</td>
          <td><code>169.47.204.128/29</code></td>
         </tr>
         
        </tbody>
      </table>
</p>
      - `TCP port 443, port 9091 FROM <each_worker_node_public_IP> TO <logging_public_IP>`
      - 将 <em>&lt;logging_public_IP&gt;</em> 替换为要允许流量流至的日志记录区域的所有地址：
        <p><table summary="表中的第一行跨两列。其余行应从左到右阅读，其中第一列是服务器位置，第二列是要匹配的 IP 地址。">
  <caption>要对日志记录流量打开的 IP 地址</caption>
        <thead>
        <th>{{site.data.keyword.containershort_notm}} 区域</th>
        <th>日志记录地址</th>
        <th>日志记录 IP 地址</th>
        </thead>
        <tbody>
          <tr>
            <td>美国东部和美国南部</td>
            <td>ingest.logging.ng.bluemix.net</td>
            <td><code>169.48.79.236</code><br><code>169.46.186.113</code></td>
           </tr>
          <tr>
           <td>英国南部</td>
           <td>ingest.logging.eu-gb.bluemix.net</td>
           <td><code>169.50.115.113</code></td>
          </tr>
          <tr>
           <td>欧洲中部</td>
           <td>ingest-eu-fra.logging.bluemix.net</td>
           <td><code>158.177.88.43</code><br><code>159.122.87.107</code></td>
          </tr>
          <tr>
           <td>亚太南部、亚太北部</td>
           <td>ingest-au-syd.logging.bluemix.net</td>
           <td><code>130.198.76.125</code><br><code>168.1.209.20</code></td>
          </tr>
         </tbody>
       </table>
</p>

  5. 对于专用防火墙，允许适当的 IBM Cloud infrastructure (SoftLayer) 专用 IP 范围。请从**后端（专用）网络**部分开始查阅[此链接](https://knowledgelayer.softlayer.com/faq/what-ip-ranges-do-i-allow-through-firewall)。
      - 添加您正在使用的所有[区域内的位置](cs_regions.html#locations)。
      - 请注意，必须添加 dal01 位置（数据中心）。
      - 打开端口 80 和 443 以允许集群引导过程。

  6. {: #pvc}要为数据存储创建持久性卷申领，请针对集群所在位置（数据中心）的 [IBM Cloud Infrastructure (SoftLayer) IP 地址](https://knowledgelayer.softlayer.com/faq/what-ip-ranges-do-i-allow-through-firewall)允许通过防火墙进行流出访问。
      - 要找到集群的位置（数据中心），请运行 `bx cs clusters`。
      - 允许访问**前端（公共）网络**和**后端（专用）网络**的 IP 范围。
      - 请注意，必须添加**后端（专用）网络**的 dal01 位置（数据中心）。

<br />


## 从集群外部访问 NodePort、LoadBalancer 和 Ingress 服务
{: #firewall_inbound}

您可以允许入站访问 NodePort、LoadBalancer 和 Ingress 服务。
{:shortdesc}

<dl>
  <dt>NodePort 服务</dt>
  <dd>打开将服务部署到允许流量流到的所有工作程序节点的公共 IP 地址时所配置的端口。要查找该端口，请运行 `kubectl get svc`。端口在 20000-32000 范围内。<dd>
  <dt>LoadBalancer 服务</dt>
  <dd>打开将服务部署到 LoadBalancer 服务的公共 IP 地址时所配置的端口。</dd>
  <dt>Ingress</dt>
  <dd>针对 Ingress 应用程序负载均衡器的 IP 地址打开端口 80（对于 HTTP）或端口 443（对于 HTTPS）。</dd>
</dl>
