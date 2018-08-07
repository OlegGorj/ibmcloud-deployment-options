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


# 日志记录和监视
{: #health}

在 {{site.data.keyword.containerlong}} 中设置日志记录和监视可帮助您对问题进行故障诊断，并提高 Kubernetes 集群和应用程序的运行状况和性能。
{: shortdesc}


## 配置集群和应用程序日志转发
{: #logging}

通过 {{site.data.keyword.containershort_notm}} 中的标准 Kubernetes 集群，可以将来自不同源的日志转发到 {{site.data.keyword.loganalysislong_notm}} 和/或外部 syslog 服务器。
{: shortdesc}

如果要将来自一个源的日志同时转发到这两个收集器服务器，那么必须创建两个日志记录配置。
{: tip}

查看下表以获取有关不同日志源的信息。

<table>
<caption>日志源</caption>
  <thead>
    <tr>
      <th>日志源</th>
      <th>特征</th>
      <th>日志路径</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>container</code></td>
      <td>在 Kubernetes 集群中运行的容器的日志。记录到 STDOUT 或 STDERR 的任何内容。</td>
      <td> </td>
    </tr>
    <tr>
      <td><code>application</code></td>
      <td>在 Kubernetes 集群中运行的自己应用程序的日志。</td>
      <td><p>可以设置路径。要发送日志，必须在日志记录配置中使用绝对路径，否则无法读取日志。如果路径安装到工作程序节点上，那么可能已创建符号链接。</p>
      <p>示例：如果指定的路径为 <code>/usr/local/<b>spark</b>/work/app-0546/0/stderr</code>，但日志实际上会转至 <code>/usr/local/<b>spark-1.0-hadoop-1.2</b>/work/app-0546/0/stderr</code>，那么无法读取日志。</p></td>
    </tr>
    <tr>
      <td><code>worker</code></td>
      <td>Kubernetes 集群中虚拟机工作程序节点的日志。</td>
      <td><code>/var/log/syslog</code>、<code>/var/log/auth.log</code></td>
    </tr>
    <tr>
      <td><code>Kubernetes</code></td>
      <td>Kubernetes 系统组件的日志。</td>
      <td><code>/var/log/kubelet.log</code>、<code>/var/log/kube-proxy.log</code>、<code>/var/log/event-exporter/&ast;.log</code></td>
    </tr>
    <tr>
      <td><code>ingress</code></td>
      <td>用于管理进入集群的网络流量的 Ingress 应用程序负载均衡器的日志。</td>
      <td><code>/var/log/alb/ids/&ast;.log</code>、<code>/var/log/alb/ids/&ast;.err</code>、<code>/var/log/alb/customerlogs/&ast;.log</code>、<code>/var/log/alb/customerlogs/&ast;.err</code></td>
    </tr>
    <tr>
      <td><code>kube-audit</code></td>
      <td>Kubernetes API 服务器的日志。</td>
      <td> </td>
    </tr>
  </tbody>
</table>

要在帐户级别启用日志记录或配置应用程序日志记录，请使用 CLI。
{: tip}


### 使用 GUI 启用日志转发
{: #enable-forwarding-ui}

可以在 {{site.data.keyword.containershort_notm}}“仪表板”中设置日志记录配置。完成此过程可能需要几分钟时间，因此，如果您未立即看到日志，请尝试等待几分钟，然后再检查。

1. 浏览到仪表板的**概述**选项卡。
2. 选择要从中转发日志的 Cloud Foundry 组织和空间。在仪表板中配置日志转发时，日志将发送到集群的缺省 {{site.data.keyword.loganalysisshort_notm}} 端点。要将日志转发到外部服务器或其他 {{site.data.keyword.loganalysisshort_notm}} 端点，可以使用 CLI 来配置日志记录。
3. 选择要从其转发日志的日志源。

    要配置应用程序日志记录或特定容器名称空间，请使用 CLI 来设置日志记录配置。
    {: tip}
4. 单击**创建**。

### 使用 CLI 启用日志转发
{: #enable-forwarding}

可以针对集群日志记录创建配置。您可以使用标志来区分不同的日志源。可以在 [CLI 参考](cs_cli_reference.html#logging_commands)中查看配置选项的完整列表。

**开始之前**

1. 验证许可权。如果在创建集群或日志记录配置时指定了空间，那么帐户所有者和 {{site.data.keyword.containershort_notm}} 密钥所有者需要该空间的“管理员”、“开发者”或“审计员”许可权。
  * 如果您不知道谁是 {{site.data.keyword.containershort_notm}} 密钥所有者，请运行以下命令。
      ```
      bx cs api-key-info <cluster_name>
      ```
      {: pre}
  * 要立即应用您对许可权所做的任何更改，请运行以下命令。
      ```
      bx cs logging-config-refresh <cluster_name>
      ```
      {: pre}

  有关更改 {{site.data.keyword.containershort_notm}} 访问策略和许可权的更多信息，请参阅[管理集群访问权](cs_users.html#access_policies)。
  {: tip}

2. [设定 CLI 的目标](cs_cli_install.html#cs_cli_configure)为日志源所在的集群。

  如果使用的是 Dedicated 帐户，那么必须登录到公共 {{site.data.keyword.cloud_notm}} 端点并将公共组织和空间设定为目标，才能启用日志转发。
  {: tip}

3. 要将日志转发到 syslog，请通过以下两种方式之一来设置接受 syslog 协议的服务器：
  * 设置和管理自己的服务器，或者让提供者为您管理服务器。如果提供者为您管理服务器，请从日志记录提供者获取日志记录端点。syslog 服务器必须接受 UDP 协议。
  * 从容器运行 syslog。例如，可以使用此[部署 .yaml 文件 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/IBM-Cloud/kube-samples/blob/master/deploy-apps-clusters/deploy-syslog-from-kube.yaml) 来访存在 Kubernetes 集群中运行容器的 Docker 公共映像。该映像在公共集群 IP 地址上发布端口 `514`，并使用此公共集群 IP 地址来配置 syslog 主机。

    通过在运行 rsyslog 服务器的 `etc/rsyslog.conf` 文件的顶部添加以下代码，可以除去 syslog 前缀而将日志作为有效 JSON 进行查看。</br>
    ```$template customFormat,"%msg%\n"
    $ActionFileDefaultTemplate customFormat
    ```
    {: tip}


**转发日志**

1. 创建日志转发配置。
  ```
  bx cs logging-config-create <cluster_name_or_ID> --logsource <log_source> --namespace <kubernetes_namespace> --hostname <log_server_hostname_or_IP> --port <log_server_port> --type <type> --app-containers <containers> --app-paths <paths_to_logs> --syslog-protocol <protocol> --skip-validation
  ```
  {: pre}

    * 缺省名称空间的容器日志记录配置以及输出示例：
      ```
            bx cs logging-config-create cluster1 --namespace default
      Creating logging configuration for container logs in cluster cluster1...
      OK
      Id                                     Source      Namespace   Host                                 Port    Org   Space   Protocol   Application Containers   Paths
      af7d1ff4-33e6-4275-8167-b52eb3c5f0ee   container   default     ingest-au-syd.logging.bluemix.net✣  9091✣   -     -       ibm        -                        -

      

      ✣ Indicates the default endpoint for the {{site.data.keyword.loganalysislong_notm}} service.

      

      ```
      {: screen}

    * 应用程序日志记录配置和输出示例：
      ```
      bx cs logging-config-create cluster2 --logsource application --app-paths '/var/log/apps.log' --app-containers 'container1,container2,container3'
      Creating logging configuration for application logs in cluster cluster2...
      OK
      Id                                     Source        Namespace   Host                                    Port    Org   Space   Protocol   Application   Containers   Paths
      aa2b415e-3158-48c9-94cf-f8b298a5ae39   application   -           ingest.logging.stage1.ng.bluemix.net✣  9091✣   -     -       ibm        container1,container2,container3   /var/log/apps.log
      ```
      {: screen}

      如果在容器中运行的应用程序无法配置为将日志写入到 STDOUT 或 STDERR，那么可以创建日志记录配置以从应用程序日志文件转发日志。
      {: tip}

  <table>
  <caption>了解此命令的组成部分</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解此命令的组成部分</th>
    </thead>
    <tbody>
      <tr>
        <td><code><em>&lt;cluster_name_or_ID&gt;</em></code></td>
        <td>集群的名称或标识。</td>
      </tr>
      <tr>
        <td><code><em>&lt;log_source&gt;</em></code></td>
        <td>要从中转发日志的源。接受的值为 <code>container</code>、<code>application</code>、<code>worker</code>、<code>kubernetes</code>、<code>ingress</code> 和 <code>kube-audit</code>。</td>
      </tr>
      <tr>
        <td><code><em>&lt;kubernetes_namespace&gt;</em></code></td>
        <td>可选：要从中转发日志的 Kubernetes 名称空间。<code>ibm-system</code> 和 <code>kube-system</code> Kubernetes 名称空间不支持日志转发。此值仅对 <code>container</code> 日志源有效。如果未指定名称空间，那么集群中的所有名称空间都将使用此配置。</td>
      </tr>
      <tr>
        <td><code><em>&lt;hostname_or_ingestion_URL&gt;</em></code></td>
        <td><p>对于 {{site.data.keyword.loganalysisshort_notm}}，请使用[数据获取 URL](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls)。如果未指定数据获取 URL，那么将使用在其中创建集群的区域的端点。</p>
        <p>对于 syslog，请指定日志收集器服务的主机名和 IP 地址。</p></td>
      </tr>
      <tr>
        <td><code><em>&lt;port&gt;</em></code></td>
        <td>数据获取端口。如果未指定端口，那么将使用标准端口 <code>9091</code>。<p>对于 syslog，请指定日志收集器服务器的端口。如果未指定端口，那么将使用标准端口 <code>514</code>。</td>
      </tr>
      <tr>
        <td><code><em>&lt;cluster_space&gt;</em></code></td>
        <td>可选：要向其发送日志的 Cloud Foundry 空间的名称。将日志转发到 {{site.data.keyword.loganalysisshort_notm}} 时，会在数据获取点中指定空间和组织。如果未指定空间，日志将发送到帐户级别。</td>
      </tr>
      <tr>
        <td><code><em>&lt;cluster_org&gt;</em></code></td>
        <td>该空间所在 Cloud Foundry 组织的名称。如果指定了空间，那么此值是必需的。</td>
      </tr>
      <tr>
        <td><code><em>&lt;type&gt;</em></code></td>
        <td>要转发日志的位置。选项为 <code>ibm</code>（将日志转发到 {{site.data.keyword.loganalysisshort_notm}}）和 <code>syslog</code>（将日志转发到外部服务器）。</td>
      </tr>
      <tr>
        <td><code><em>&lt;paths_to_logs&gt;</em></code></td>
        <td>容器上应用程序要将日志记录到的路径。要转发源类型为 <code>application</code> 的日志，必须提供路径。要指定多个路径，请使用逗号分隔列表。示例：<code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></td>
      </tr>
      <tr>
        <td><code><em>&lt;containers&gt;</em></code></td>
        <td>可选：要转发来自应用程序的日志，可以指定包含应用程序的容器的名称。可以使用逗号分隔列表来指定多个容器。如果未指定任何容器，那么会转发来自包含所提供路径的所有容器中的日志。</td>
      </tr>
      <tr>
        <td><code><em>&lt;protocol&gt;</em></code></td>
        <td>日志记录类型为 <code>syslog</code> 时，为传输层协议。支持的值为 <code>TCP</code>，缺省值为 <code>UDP</code>。使用 <code>udp</code> 协议转发到 rsyslog 服务器时，将截断超过 1 KB 的日志。</td>
      </tr>
      <tr>
        <td><code><em>--skip-validation</em></code></td>
        <td>可选：跳过对指定组织和空间名称的验证。跳过验证可减少处理时间，但无效的日志记录配置将无法正确转发日志。</td>
      </tr>
    </tbody>
  </table>

2. 通过以下两种方式中的一种来验证配置是否正确：

    * 列出集群中的所有日志记录配置：
      ```
      bx cs logging-config-get <cluster_name_or_ID>
      ```
      {: pre}

      输出示例：

      ```
      Id                                    Source       Namespace     Host                          Port   Org      Space      Protocol     Paths
      f4bc77c0-ee7d-422d-aabf-a4e6b977264e  kubernetes   -             172.30.xxx.xxx                5514   -        -          syslog       /var/log/kubelet.log,/var/log/kube-proxy.log
      5bd9c609-13c8-4c48-9d6e-3a6664c825a9  application  -             ingest.logging.ng.bluemix.net 9091   my_org   my_space   ibm          /var/log/apps/**/*.log,/var/log/apps/**/*.err
      8a284f1a-451c-4c48-b1b4-a4e6b977264e  containers   my-namespace  myhostname.common             5514   -        -          syslog       -
      ```
      {: screen}

    * 列出一种类型日志源的日志记录配置：
      ```
      bx cs logging-config-get <cluster_name_or_ID> --logsource worker
      ```
      {: pre}

      输出示例：

      ```
            Id                                    Source    Namespace   Host                            Port   Org    Space     Protocol    Paths
      f4bc77c0-ee7d-422d-aabf-a4e6b977264e  worker    -           ingest.logging.ng.bluemix.net   9091   -      -         ibm         /var/log/syslog,/var/log/auth.log
      5bd9c609-13c8-4c48-9d6e-3a6664c825a9  worker    -           172.30.xxx.xxx                  5514   -      -         syslog      /var/log/syslog,/var/log/auth.log
      ```
      {: screen}

## 更新日志转发
{: #updating-forwarding}

1. 更新日志转发配置。
    ```
    bx cs logging-config-update <cluster_name_or_ID> <log_config_id> --namespace <namespace> --type <log_type> --logsource <source> --hostname <hostname_or_ingestion_URL> --port <port> --space <cluster_space> --org <cluster_org> --app-containers <containers> --app-paths <paths_to_logs>
    ```
    {: pre}

  <table>
  <caption>了解此命令的组成部分</caption>
  <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解此命令的组成部分</th>
  </thead>
  <tbody>
    <tr>
      <td><code><em>&lt;cluster_name_or_ID&gt;</em></code></td>
      <td>集群的名称或标识。</td>
    </tr>
    <tr>
      <td><code><em>&lt;log_config_id&gt;</em></code></td>
      <td>要更新的配置的标识。</td>
    </tr>
    <tr>
      <td><code><em>&lt;namespace&gt;</em></code></td>
      <td>可选：要从中转发日志的 Kubernetes 名称空间。<code>ibm-system</code> 和 <code>kube-system</code> Kubernetes 名称空间不支持日志转发。此值仅对 <code>container</code> 日志源有效。如果未指定名称空间，那么集群中的所有名称空间都将使用此配置。</td>
    </tr>
    <tr>
      <td><code><em>&lt;log_type&gt;</em></code></td>
      <td>要转发日志的位置。选项为 <code>ibm</code>（将日志转发到 {{site.data.keyword.loganalysisshort_notm}}）和 <code>syslog</code>（将日志转发到外部服务器）。</td>
    </tr>
    <tr>
      <td><code><em>&lt;hostname_or_ingestion_URL&gt;</em></code></td>
      <td><p>对于 {{site.data.keyword.loganalysisshort_notm}}，请使用[数据获取 URL](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls)。如果未指定数据获取 URL，那么将使用在其中创建集群的区域的端点。</p>
      <p>对于 syslog，请指定日志收集器服务的主机名和 IP 地址。</p></td>
    </tr>
    <tr>
      <td><code><em>&lt;port&gt;</em></code></td>
      <td>数据获取端口。如果未指定端口，那么将使用标准端口 <code>9091</code>。<p>对于 syslog，请指定日志收集器服务器的端口。如果未指定端口，那么将使用标准端口 <code>514</code>。</td>
    </tr>
    <tr>
      <td><code><em>&lt;cluster_space&gt;</em></code></td>
      <td>可选：要向其发送日志的 Cloud Foundry 空间的名称。将日志转发到 {{site.data.keyword.loganalysisshort_notm}} 时，会在数据获取点中指定空间和组织。如果未指定空间，日志将发送到帐户级别。</td>
    </tr>
    <tr>
      <td><code><em>&lt;cluster_org&gt;</em></code></td>
      <td>该空间所在 Cloud Foundry 组织的名称。如果指定了空间，那么此值是必需的。</td>
    </tr>
    <tr>
      <td><code><em>&lt;paths_to_logs&gt;</em></code></td>
      <td>容器上应用程序要将日志记录到的路径。要转发源类型为 <code>application</code> 的日志，必须提供路径。要指定多个路径，请使用逗号分隔列表。示例：<code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></td>
    </tr>
    <tr>
      <td><code><em>&lt;containers&gt;</em></code></td>
      <td>可选：要转发来自应用程序的日志，可以指定包含应用程序的容器的名称。可以使用逗号分隔列表来指定多个容器。如果未指定任何容器，那么会转发来自包含所提供路径的所有容器中的日志。</td>
    </tr>
  </tbody>
  </table>

<br />



## 过滤日志
{: #filter-logs}

可以通过过滤掉某段时间内的特定日志来选择转发的日志。

1. 创建日志记录过滤器。
  ```
  bx cs logging-filter-create <cluster_name_or_ID> --type <log_type> --logging-configs <configs> --namespace <kubernetes_namespace> --container <container_name> --level <logging_level> --regex-message <message>
  ```
  {: pre}
  <table>
  <caption>了解此命令的组成部分</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解此命令的组成部分</th>
    </thead>
    <tbody>
      <tr>
        <td>&lt;cluster_name_or_ID&gt;</td>
        <td>必需：要为其创建日志记录过滤器的集群的名称或标识。</td>
      </tr>
      <tr>
        <td><code>&lt;log_type&gt;</code></td>
        <td>要应用过滤器的日志的类型。目前支持 <code>all</code>、<code>container</code> 和 <code>host</code>。</td>
      </tr>
      <tr>
        <td><code>&lt;configs&gt;</code></td>
        <td>可选：日志记录配置标识的逗号分隔列表。如果未提供，过滤器将应用于传递到过滤器的所有集群日志记录配置。可以通过将 <code>--show-matching-configs</code> 标志用于命令来查看与过滤器相匹配的日志配置。</td>
      </tr>
      <tr>
        <td><code>&lt;kubernetes_namespace&gt;</code></td>
        <td>可选：要从中转发日志的 Kubernetes 名称空间。仅当使用日志类型 <code>container</code> 时，此标志才适用。</td>
      </tr>
      <tr>
        <td><code>&lt;container_name&gt;</code></td>
        <td>可选：要从中过滤日志的容器的名称。</td>
      </tr>
      <tr>
        <td><code>&lt;logging_level&gt;</code></td>
        <td>可选：过滤掉处于指定级别及更低级别的日志。规范顺序的可接受值为 <code>fatal</code>、<code>error</code>、<code>warn/warning</code>、<code>info</code>、<code>debug</code> 和 <code>trace</code>。例如，如果过滤掉 <code>info</code> 级别的日志，那么还会过滤掉 <code>debug</code> 和 <code>trace</code>。**注**：仅当日志消息为 JSON 格式且包含 level 字段时，才能使用此标志。要以 JSON 格式显示消息，请向命令附加 <code>--json</code> 标志。</td>
      </tr>
      <tr>
        <td><code>&lt;message&gt;</code></td>
        <td>可选：过滤掉包含编写为正则表达式的指定消息的日志。</td>
      </tr>
    </tbody>
  </table>

2. 查看创建的日志过滤器。

  ```
  bx cs logging-filter-get <cluster_name_or_ID> --id <filter_ID> --show-matching-configs
  ```
  {: pre}
  <table>
  <caption>了解此命令的组成部分</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解此命令的组成部分</th>
    </thead>
    <tbody>
      <tr>
        <td>&lt;cluster_name_or_ID&gt;</td>
        <td>必需：要为其创建日志记录过滤器的集群的名称或标识。</td>
      </tr>
      <tr>
        <td><code>&lt;filter_ID&gt;</code></td>
        <td>可选：要查看的日志过滤器的标识。</td>
      </tr>
      <tr>
        <td><code>--show-matching-configs</code></td>
        <td>显示每个过滤器应用于的日志记录配置。</td>
      </tr>
    </tbody>
  </table>

3. 更新创建的日志过滤器。
  ```
  bx cs logging-filter-update <cluster_name_or_ID> --id <filter_ID> --type <log_type> --logging-configs <configs> --namespace <kubernetes_namespace --container <container_name> --level <logging_level> --regex-message <message>
  ```
  {: pre}
  <table>
  <caption>了解此命令的组成部分</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解此命令的组成部分</th>
    </thead>
    <tbody>
      <tr>
        <td>&lt;cluster_name_or_ID&gt;</td>
        <td>必需：要更新其日志记录过滤器的集群的名称或标识。</td>
      </tr>
      <tr>
        <td><code>&lt;filter_ID&gt;</code></td>
        <td>要更新的日志过滤器的标识。</td>
      </tr>
      <tr>
        <td><code><&lt;log_type&gt;</code></td>
        <td>要应用过滤器的日志的类型。目前支持 <code>all</code>、<code>container</code> 和 <code>host</code>。</td>
      </tr>
      <tr>
        <td><code>&lt;configs&gt;</code></td>
        <td>可选：要应用过滤器的所有日志记录配置标识的逗号分隔列表。如果未提供，过滤器将应用于传递到过滤器的所有集群日志记录配置。可以通过将 <code>--show-matching-configs</code> 标志用于 <code>bx cs logging-filter-get</code> 命令来查看与过滤器相匹配的日志配置。</td>
      </tr>
      <tr>
        <td><code>&lt;kubernetes_namespace&gt;</code></td>
        <td>可选：要从中转发日志的 Kubernetes 名称空间。仅当使用日志类型 <code>container</code> 时，此标志才适用。</td>
      </tr>
      <tr>
        <td><code>&lt;container_name&gt;</code></td>
        <td>可选：要从中过滤日志的容器的名称。仅当使用日志类型 <code>container</code> 时，此标志才适用。</td>
      </tr>
      <tr>
        <td><code>&lt;logging_level&gt;</code></td>
        <td>可选：过滤掉处于指定级别及更低级别的日志。规范顺序的可接受值为 <code>fatal</code>、<code>error</code>、<code>warn/warning</code>、<code>info</code>、<code>debug</code> 和 <code>trace</code>。例如，如果过滤掉 <code>info</code> 级别的日志，那么还会过滤掉 <code>debug</code> 和 <code>trace</code>。**注**：仅当日志消息为 JSON 格式且包含 level 字段时，才能使用此标志。</td>
      </tr>
      <tr>
        <td><code>&lt;message&gt;</code></td>
        <td>可选：过滤掉包含编写为正则表达式的指定消息的日志。</td>
      </tr>
    </tbody>
  </table>

4. 删除创建的日志过滤器。

  ```
  bx cs logging-filter-rm <cluster_name_or_ID> --id <filter_ID> [--all]
  ```
  {: pre}
  <table>
  <caption>了解此命令的组成部分</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解此命令的组成部分</th>
    </thead>
    <tbody>
      <tr>
        <td><code>&lt;cluster_name_or_ID&gt;</code></td>
        <td>必需：要删除其日志记录过滤器的集群的名称或标识。</td>
      </tr>
      <tr>
        <td><code>&lt;filter_ID&gt;</code></td>
        <td>可选：要除去的日志过滤器的标识。</td>
      </tr>
      <tr>
        <td><code>--all</code></td>
        <td>可选：删除所有日志转发过滤器。</td>
      </tr>
    </tbody>
  </table>

<br />




## 查看日志
{: #view_logs}

要查看集群和容器的日志，可以使用标准的 Kubernetes 和 Docker 日志记录功能。
{:shortdesc}

### {{site.data.keyword.loganalysislong_notm}}
{: #view_logs_k8s}

可以通过 Kibana 仪表板查看转发到 {{site.data.keyword.loganalysislong_notm}} 的日志。
{: shortdesc}

如果使用缺省值来创建配置文件，那么可以在创建集群的帐户或者组织和空间中找到日志。如果在配置文件中指定了组织和空间，那么可以在该空间中找到日志。有关日志记录的更多信息，请参阅 [{{site.data.keyword.containershort_notm}} 的日志记录](/docs/services/CloudLogAnalysis/containers/containers_kubernetes.html#containers_kubernetes)。

要访问 Kibana 仪表板，请转至下列其中一个 URL，然后选择在其中配置了集群日志转发的 {{site.data.keyword.Bluemix_notm}} 帐户或空间。
- 美国南部和美国东部：https://logging.ng.bluemix.net
- 英国南部：https://logging.eu-gb.bluemix.net
- 欧洲中部：https://logging.eu-fra.bluemix.net
- 亚太南部：https://logging.au-syd.bluemix.net

有关查看日志的更多信息，请参阅[通过 Web 浏览器导航至 Kibana](/docs/services/CloudLogAnalysis/kibana/launch.html#launch_Kibana_from_browser)。

### Docker 日志
{: #view_logs_docker}

可以利用内置 Docker 日志记录功能来查看标准 STDOUT 和 STDERR 输出流上的活动。有关更多信息，请参阅[查看在 Kubernetes 集群中运行的容器的容器日志](/docs/services/CloudLogAnalysis/containers/containers_kubernetes.html#containers_kubernetes)。

<br />



## 停止日志转发
{: #log_sources_delete}

可以停止集群的一个或全部日志记录配置的日志转发。
{: shortdesc}

1. [设定 CLI 的目标](cs_cli_install.html#cs_cli_configure)为日志源所在的集群。

2. 删除日志记录配置。
<ul>
<li>删除一个日志记录配置：</br>
  <pre><code>bx cs logging-config-rm &lt;cluster_name_or_ID&gt; --id &lt;log_config_ID&gt;</pre></code>
  <table>
  <caption>了解此命令的组成部分</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解此命令的组成部分</th>
    </thead>
    <tbody>
      <tr>
        <td><code><em>&lt;cluster_name_or_ID&gt;</em></code></td>
        <td>日志记录配置所在的集群的名称。</td>
      </tr>
      <tr>
        <td><code><em>&lt;log_config_ID&gt;</em></code></td>
        <td>日志源配置的标识。</td>
      </tr>
    </tbody>
  </table></li>
<li>删除所有日志记录配置：</br>
  <pre><code>bx cs logging-config-rm <my_cluster> --all</pre></code></li>
</ul>

<br />



## 为 Kubernetes API 审计日志配置日志转发
{: #api_forward}

Kubernetes 会自动审计通过 API 服务器传递的任何事件。可以将日志转发到 {{site.data.keyword.loganalysisshort_notm}} 或外部服务器。
{: shortdesc}


有关 Kubernetes 审计日志的更多信息，请参阅 Kubernetes 文档中的<a href="https://kubernetes.io/docs/tasks/debug-application-cluster/audit/" target="blank">审计主题 <img src="../icons/launch-glyph.svg" alt="外部链接图标"></a>。

* 仅 Kubernetes V1.7 和更高版本支持 Kubernetes API 审计日志的转发。
* 目前，缺省审计策略用于具有此日志记录配置的所有集群。
* 目前不支持过滤器。
* 每个集群只能有一个 `kube-audit` 配置，但可以通过创建日志记录配置和 Webhook，将日志转发到 {{site.data.keyword.loganalysisshort_notm}} 和外部服务器。
{: tip}


### 将审计日志发送到 {{site.data.keyword.loganalysisshort_notm}}
{: #audit_enable_loganalysis}

可以将 Kubernetes API 服务器审计日志转发到 {{site.data.keyword.loganalysisshort_notm}}

**开始之前**

1. 验证许可权。如果在创建集群或日志记录配置时指定了空间，那么帐户所有者和 {{site.data.keyword.containershort_notm}} 密钥所有者需要该空间的“管理员”、“开发者”或“审计员”许可权。

2. [设定 CLI 的目标](cs_cli_install.html#cs_cli_configure)为要从中收集 API 服务器审计日志的集群。**注**：如果使用的是 Dedicated 帐户，那么必须登录到公共 {{site.data.keyword.cloud_notm}} 端点并将公共组织和空间设定为目标，才能启用日志转发。


**转发日志**

1. 创建日志记录配置。

    ```
    bx cs logging-config-create <cluster_name_or_ID> --logsource kube-audit --space <cluster_space> --org <cluster_org> --hostname <ingestion_URL> --type ibm
    ```
    {: pre}

    示例命令和输出：

    ```
    bx cs logging-config-create myCluster --logsource kube-audit
    Creating logging configuration for kube-audit logs in cluster myCluster...
    OK
    Id                                     Source      Namespace   Host                                 Port    Org   Space   Protocol   Application Containers   Paths
    14ca6a0c-5bc8-499a-b1bd-cedcf40ab850   kube-audit  -           ingest-au-syd.logging.bluemix.net✣   9091✣   -     -       ibm        -                        -

    ✣ Indicates the default endpoint for the {{site.data.keyword.loganalysisshort_notm}} service.

      

    ```
    {: screen}

    <table>
    <caption>了解此命令的组成部分</caption>
      <thead>
        <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解此命令的组成部分</th>
      </thead>
      <tbody>
        <tr>
          <td><code><em>&lt;cluster_name_or_ID&gt;</em></code></td>
          <td>集群的名称或标识。</td>
        </tr>
        <tr>
          <td><code><em>&lt;ingestion_URL&gt;</em></code></td>
          <td>要从中转发日志的端点。如果未指定[数据获取 URL](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls)，那么将使用在其中创建集群的区域的端点。</td>
        </tr>
        <tr>
          <td><code><em>&lt;cluster_space&gt;</em></code></td>
          <td>可选：要向其发送日志的 Cloud Foundry 空间的名称。将日志转发到 {{site.data.keyword.loganalysisshort_notm}} 时，会在数据获取点中指定空间和组织。如果未指定空间，日志将发送到帐户级别。</td>
        </tr>
        <tr>
          <td><code><em>&lt;cluster_org&gt;</em></code></td>
          <td>该空间所在 Cloud Foundry 组织的名称。如果指定了空间，那么此值是必需的。</td>
        </tr>
      </tbody>
    </table>

2. 查看集群日志记录配置，以验证它是否已按预期方式实现。

    ```
          bx cs logging-config-get <cluster_name_or_ID>
      ```
    {: pre}

    示例命令和输出：
    ```
    bx cs logging-config-get myCluster
    Retrieving cluster myCluster logging configurations...
    OK
    Id                                     Source        Namespace   Host                                 Port    Org   Space   Protocol   Application Containers   Paths
    a550d2ba-6a02-4d4d-83ef-68f7a113325c   container     *           ingest-au-syd.logging.bluemix.net✣   9091✣   -     -       ibm        -                        -
    14ca6a0c-5bc8-499a-b1bd-cedcf40ab850   kube-audit    -           ingest-au-syd.logging.bluemix.net✣   9091✣   -     -       ibm        -                    
    ```
    {: screen}

  <table>
  <caption>了解此命令的组成部分</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解此命令的组成部分</th>
    </thead>
    <tbody>
      <tr>
        <td><code><em>&lt;cluster_name_or_ID&gt;</em></code></td>
        <td>集群的名称或标识。</td>
      </tr>
    </tbody>
  </table>

3. 可选：如果要停止转发审计日志，可以[删除配置](#log_sources_delete)。

<br />



### 将审计日志发送到外部服务器
{: #audit_enable}

**开始之前**

1. 设置可以转发日志的远程日志记录服务器。例如，您可以[将 Logstash 用于 Kubernetes ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/tasks/debug-application-cluster/audit/#use-logstash-to-collect-and-distribute-audit-events-from-webhook-backend) 以收集审计事件。

2. [设定 CLI 的目标](cs_cli_install.html#cs_cli_configure)为要从中收集 API 服务器审计日志的集群。**注**：如果使用的是 Dedicated 帐户，那么必须登录到公共 {{site.data.keyword.cloud_notm}} 端点并将公共组织和空间设定为目标，才能启用日志转发。


转发 Kubernetes API 审计日志：

1. 设置 Webhook。如果未在标志中提供任何信息，那么将使用缺省配置。

    ```
        bx cs apiserver-config-set audit-webhook <cluster_name_or_ID> --remoteServer <server_URL_or_IP> --caCert <CA_cert_path> --clientCert <client_cert_path> --clientKey <client_key_path>
    ```
    {: pre}

  <table>
  <caption>了解此命令的组成部分</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解此命令的组成部分</th>
    </thead>
    <tbody>
      <tr>
        <td><code><em>&lt;cluster_name_or_ID&gt;</em></code></td>
        <td>集群的名称或标识。</td>
      </tr>
      <tr>
        <td><code><em>&lt;server_URL&gt;</em></code></td>
        <td>要向其发送日志的远程日志记录服务的 URL 或 IP 地址。如果提供了不安全的服务器 URL，那么将忽略证书。</td>
      </tr>
      <tr>
        <td><code><em>&lt;CA_cert_path&gt;</em></code></td>
        <td>用于验证远程日志记录服务的 CA 证书的文件路径。</td>
      </tr>
      <tr>
        <td><code><em>&lt;client_cert_path&gt;</em></code></td>
        <td>用于向远程日志记录服务进行认证的客户机证书的文件路径。</td>
      </tr>
      <tr>
        <td><code><em>&lt;client_key_path&gt;</em></code></td>
        <td>用于连接到远程日志记录服务的相应客户机密钥的文件路径。</td>
      </tr>
    </tbody>
  </table>

2. 通过查看远程日志记录服务的 URL 来验证是否已启用日志转发。

    ```
        bx cs apiserver-config-get audit-webhook <cluster_name_or_ID>
    ```
    {: pre}

    输出示例：
    ```
        OK
    Server:			https://8.8.8.8
    ```
    {: screen}

3. 通过重新启动 Kubernetes 主节点来应用配置更新。

    ```
        bx cs apiserver-refresh <cluster_name_or_ID>
    ```
    {: pre}

4. 可选：如果要停止转发审计日志，可以禁用配置。
    1. [设定 CLI 的目标](cs_cli_install.html#cs_cli_configure)为要停止从中收集 API 服务器审计日志的集群。
    2. 禁用集群 API 服务器的 Webhook 后端配置。

        ```
            bx cs apiserver-config-unset audit-webhook <cluster_name_or_ID>
    ```
        {: pre}

    3. 通过重新启动 Kubernetes 主节点来应用配置更新。

        ```
            bx cs apiserver-refresh <cluster_name_or_ID>
    ```
        {: pre}

## 查看度量值
{: #view_metrics}

度量值可帮助您监视集群的运行状况和性能。您可以使用标准 Kubernetes 和 Docker 功能来监视集群和应用程序的运行状况。
**注**：仅标准集群支持监视功能。
{:shortdesc}

<dl>
  <dt>{{site.data.keyword.Bluemix_notm}} 中的集群详细信息页面</dt>
    <dd>{{site.data.keyword.containershort_notm}} 提供了有关集群的运行状况和容量以及集群资源使用情况的信息。可以使用此 GUI 通过 {{site.data.keyword.Bluemix_notm}} 服务绑定来向外扩展集群、使用持久性存储器以及向集群添加更多功能。要查看集群详细信息页面，请转至 **{{site.data.keyword.Bluemix_notm}} 仪表板**，然后选择集群。</dd>
  <dt>Kubernetes 仪表板</dt>
    <dd>Kubernetes 仪表板是一个管理 Web 界面，可以在其中查看工作程序节点的运行状况，查找 Kubernetes 资源，部署容器化应用程序，以及使用日志记录和监视信息对应用程序进行故障诊断。有关如何访问 Kubernetes 仪表板的更多信息，请参阅[启动 {{site.data.keyword.containershort_notm}} 的 Kubernetes 仪表板](cs_app.html#cli_dashboard)。</dd>
  <dt>{{site.data.keyword.monitoringlong_notm}}</dt>
    <dd><p>标准集群的度量值位于创建 Kubernetes 集群时登录到的 {{site.data.keyword.Bluemix_notm}} 帐户中。如果在创建集群时指定了 {{site.data.keyword.Bluemix_notm}} 空间，那么度量值将位于该空间中。将为集群中部署的所有容器自动收集容器度量值。这些度量值会通过 Grafana 发送并使其可用。有关度量值的更多信息，请参阅[监视 {{site.data.keyword.containershort_notm}}](/docs/services/cloud-monitoring/containers/monitoring_containers_ov.html#monitoring_bmx_containers_ov)。</p>
    <p>要访问 Grafana 仪表板，请转至以下某个 URL，然后选择在其中已创建集群的 {{site.data.keyword.Bluemix_notm}} 帐户或空间。<ul>
        <li>美国南部和美国东部：https://metrics.ng.bluemix.net
</li>
        <li>英国南部：https://metrics.eu-gb.bluemix.net</li>
        <li>欧洲中部：https://metrics.eu-de.bluemix.net</li>
      </ul></p></dd>
</dl>

### 其他运行状况监视工具
{: #health_tools}

可以配置其他工具来执行更多监视功能。
<dl>
  <dt>Prometheus</dt>
    <dd>Prometheus 是一个开放式源代码监视、日志记录和警报工具，专为 Kubernetes 而设计。该工具基于 Kubernetes 日志记录信息来检索有关集群、工作程序节点和部署运行状况的详细信息。有关设置信息，请参阅[将服务与 {{site.data.keyword.containershort_notm}} 集成](cs_integrations.html#integrations)。</dd>
</dl>

<br />


## 使用自动恢复为工作程序节点配置运行状况监视
{: #autorecovery}

可以将 {{site.data.keyword.containerlong_notm}} 自动恢复系统部署到 Kubernetes V1.7 或更高版本的现有集群中。
{: shortdesc}

自动恢复系统会使用各种检查来查询工作程序节点的运行状态。如果自动恢复根据配置的检查，检测到运行状况欠佳的工作程序节点，那么自动恢复会触发更正操作，例如在工作程序节点上重装操作系统。一次只会对一个工作程序节点执行更正操作。该工作程序节点必须成功完成更正操作后，才能对其他任何工作程序节点执行更正操作。
有关更多信息，请参阅此[自动恢复博客帖子 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/)。</br> </br>
**注**：自动恢复至少需要一个正常运行的节点才能有效运行。仅在具有两个或两个以上工作程序节点的集群中，将自动恢复配置为执行主动检查。

开始之前，请[设定 CLI 的目标](cs_cli_install.html#cs_cli_configure)为要检查其工作程序节点状态的集群。

1. [为集群安装 Helm，然后将 {{site.data.keyword.Bluemix_notm}} 存储库添加到 Helm 实例](cs_integrations.html#helm)。

2. 创建配置映射文件以通过 JSON 格式定义检查。例如，以下 YAML 文件定义了三项检查：一项 HTTP 检查和两项 Kubernetes API 服务器检查。请参阅示例 YAML 文件后面的表，以获取有关三种检查的信息以及有关这些检查的各个组成部分的信息。
</br>
   **提示**：将每项检查定义为配置映射中 `data` 部分中的唯一键。

   ```
   kind: ConfigMap
   apiVersion: v1
   metadata:
     name: ibm-worker-recovery-checks
     namespace: kube-system
   data:
     checknode.json: |
       {
         "Check":"KUBEAPI",
          "Resource":"NODE",
          "FailureThreshold":3,
          "CorrectiveAction":"RELOAD",
          "CooloffSeconds":1800,
          "IntervalSeconds":180,
          "TimeoutSeconds":10,
          "Enabled":true
        }
      checkpod.json: |
        {
          "Check":"KUBEAPI",
         "Resource":"POD",
         "PodFailureThresholdPercent":50,
         "FailureThreshold":3,
         "CorrectiveAction":"RELOAD",
         "CooloffSeconds":1800,
         "IntervalSeconds":180,
         "TimeoutSeconds":10,
         "Enabled":true
       }
     checkhttp.json: |
       {
         "Check":"HTTP",
         "FailureThreshold":3,
         "CorrectiveAction":"REBOOT",
         "CooloffSeconds":1800,
         "IntervalSeconds":180,
         "TimeoutSeconds":10,
         "Port":80,
         "ExpectedStatus":200,
         "Route":"/myhealth",
         "Enabled":false
       }
   ```
   {:codeblock}

   <table summary="了解配置映射的组成部分">
   <caption>了解配置映射的组成部分</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解配置映射的组成部分</th>
   </thead>
   <tbody>
   <tr>
   <td><code>name</code></td>
   <td>配置名称 <code>ibm-worker-recovery-checks</code> 是常量，无法更改。</td>
   </tr>
   <tr>
   <td><code>namespace</code></td>
   <td><code>kube-system</code> 名称空间是常量，无法更改。</td>
   </tr>
   <tr>
   <td><code>checknode.json</code></td>
   <td>定义 Kubernetes API 节点检查，用于检查是否每个工作程序节点都处于 <code>Ready</code> 状态。如果特定工作程序节点不处于 <code>Ready</code> 状态，那么对该工作程序节点的检查计为一次失败。示例 YAML 中的检查每 3 分钟运行一次。如果连续失败三次，将重新装入该工作程序节点。此操作相当于运行 <code>bx cs worker-reload</code>。<br></br>节点检查会一直处于启用状态，直到您将 <b>Enabled</b> 字段设置为 <code>false</code> 或除去检查。</td>
   </tr>
   <tr>
   <td><code>checkpod.json</code></td>
   <td>
定义 Kubernetes API pod 检查，用于根据分配给工作程序节点的 pod 总数来检查该工作程序节点上 <code>NotReady</code> pod 的总百分比。如果特定工作程序节点的 <code>NotReady</code> pod 的总百分比大于定义的 <code>PodFailureThresholdPercent</code>，那么对该工作程序节点的检查计为一次失败。示例 YAML 中的检查每 3 分钟运行一次。如果连续失败三次，将重新装入该工作程序节点。此操作相当于运行 <code>bx cs worker-reload</code>。例如，缺省 <code>PodFailureThresholdPercent</code>为 50%。如果 <code>NotReady</code> pod 的百分比连续三次超过 50%，那么将重新装入工作程序节点。<br></br>缺省情况下，将检查所有名称空间中的 pod。要将检查限制为仅检查指定名称空间中的 pod，请将 <code>Namespace</code> 字段添加到检查。pod 检查会一直处于启用状态，直到您将 <b>Enabled</b> 字段设置为 <code>false</code> 或除去检查。</td>
   </tr>
   <tr>
   <td><code>checkhttp.json</code></td>
   <td>定义 HTTP 检查，用于检查在工作程序节点上运行的 HTTP 服务器是否运行正常。要使用此检查，必须使用 [DaemonSet ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) 在集群中的每个工作程序节点上部署 HTTP 服务器。必须实现在 <code>/myhealth</code> 路径中可用并且可以验证 HTTP 服务器是否运行正常的运行状况检查。您可以通过更改 <strong>Route</strong> 参数来定义其他路径。如果 HTTP 服务器运行正常，那么必须返回 <strong>ExpectedStatus</strong> 中定义的 HTTP 响应代码。必须将 HTTP 服务器配置为侦听工作程序节点的专用 IP 地址。可以通过运行 <code>kubectl get nodes</code> 来查找专用 IP 地址。<br></br>
   例如，假设集群中有两个节点，其专用 IP 地址分别为 10.10.10.1 和 10.10.10.2。在此示例中，会对下面两个路径检查是否返回 200 HTTP 响应：<code>http://10.10.10.1:80/myhealth</code> 和 <code>http://10.10.10.2:80/myhealth</code>。
   示例 YAML 中的检查每 3 分钟运行一次。如果连续失败三次，将重新引导该工作程序节点。此操作相当于运行 <code>bx cs worker-reboot</code>。<br></br>HTTP 检查会一直处于禁用状态，直到您将 <b>Enabled</b> 字段设置为 <code>true</code>。</td>
   </tr>
   </tbody>
   </table>

   <table summary="了解检查的各个组成部分">
   <caption>了解检查的各个组成部分</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解检查的各个组成部分</th>
   </thead>
   <tbody>
   <tr>
   <td><code>Check</code></td>
   <td>输入希望自动恢复使用的检查类型。<ul><li><code>HTTP</code>：自动恢复调用在每个节点上运行的 HTTP 服务器，以确定节点是否在正常运行。</li><li><code>KUBEAPI</code>：自动恢复调用 Kubernetes API 服务器并读取工作程序节点报告的运行状态数据。</li></ul></td>
   </tr>
   <tr>
   <td><code>资源</code></td>
   <td>检查类型为 <code>KUBEAPI</code> 时，输入希望自动恢复检查的资源类型。接受的值为 <code>NODE</code> 或 <code>POD</code>。</td>
   </tr>
   <tr>
   <td><code>FailureThreshold</code></td>
   <td>输入检查连续失败次数的阈值。达到此阈值时，自动恢复会触发指定的更正操作。例如，如果值为 3 且自动恢复连续三次执行配置的检查失败，那么自动恢复会触发与检查关联的更正操作。</td>
   </tr>
   <tr>
   <td><code>PodFailureThresholdPercent</code></td>
   <td>资源类型为 <code>POD</code> 时，请输入工作程序节点上可以处于 [NotReady ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/#define-readiness-probes) 状态的 pod 的百分比阈值。此百分比基于安排到一个工作程序节点的 pod 总数。检查确定运行状况欠佳的 pod 百分比大于阈值时，该检查计为一次失败。</td>
   </tr>
   <tr>
   <td><code>CorrectiveAction</code></td>
   <td>输入在达到失败阈值时要运行的操作。仅当没有其他工作程序在进行修复，并且此工作程序节点距离执行上一个更正操作的时间已超过等待期时，才会运行更正操作。<ul><li><code>REBOOT</code>：重新引导工作程序节点。</li><li><code>RELOAD</code>：从干净的操作系统重新装入工作程序节点的所有必需配置。</li></ul></td>
   </tr>
   <tr>
   <td><code>CooloffSeconds</code></td>
   <td>输入自动恢复在对节点发出更正操作后，必须等待多长时间才能再次向该节点发出其他更正操作（以秒为单位）。等待期自发出更正操作时开始计算。</td>
   </tr>
   <tr>
   <td><code>IntervalSeconds</code></td>
   <td>输入连续检查的间隔秒数。例如，如果值为 180，那么自动恢复会每 3 分钟对每个节点运行一次检查。</td>
   </tr>
   <tr>
   <td><code>TimeoutSeconds</code></td>
   <td>输入在自动恢复终止调用操作之前，对数据库执行检查调用所用的最长时间（以秒为单位）。<code>TimeoutSeconds</code> 的值必须小于 <code>IntervalSeconds</code> 的值。</td>
   </tr>
   <tr>
   <td><code>端口</code></td>
   <td>检查类型为 <code>HTTP</code> 时，请输入工作程序节点上 HTTP 服务器必须绑定到的端口。此端口必须在集群中每个工作程序节点的 IP 上公开。自动恢复需要一个跨所有节点的常量端口号以用于检查服务器。将定制服务器部署到集群中时，请使用 [DaemonSets ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)。</td>
   </tr>
   <tr>
   <td><code>ExpectedStatus</code></td>
   <td>检查类型为 <code>HTTP</code> 时，请输入期望从检查返回的 HTTP 服务器状态。例如，值为 200 表示期望服务器返回 <code>OK</code> 响应。</td>
   </tr>
   <tr>
   <td><code>Route</code></td>
   <td>检查类型为 <code>HTTP</code> 时，请输入从 HTTP 服务器请求的路径。此值通常是在所有工作程序节点上运行的服务器的度量值路径。</td>
   </tr>
   <tr>
   <td><code>Enabled</code></td>
   <td>输入 <code>true</code> 以启用检查，或输入 <code>false</code> 以禁用检查。</td>
   </tr>
   <tr>
   <td><code>Namespace</code></td>
   <td> 可选：要将 <code>checkpod.json</code> 限制为仅检查一个名称空间中的 pod，请添加 <code>Namespace</code> 字段并输入名称空间。</td>
   </tr>
   </tbody>
   </table>

3. 在集群中创建配置映射。

    ```
        kubectl apply -f ibm-worker-recovery-checks.yaml
    ```
    {: pre}

3. 使用相应检查来验证是否已在 `kube-system` 名称空间中创建名称为 `ibm-worker-recovery-checks` 的配置映射。

    ```
        kubectl -n kube-system get cm ibm-worker-recovery-checks -o yaml
    ```
    {: pre}

4. 通过安装 `ibm-worker-recovery` Helm 图表，将自动恢复部署到集群中。

    ```
    helm install --name ibm-worker-recovery ibm/ibm-worker-recovery  --namespace kube-system
    ```
    {: pre}

5. 过几分钟后，可以检查以下命令的输出中的 `Events` 部分，以查看自动恢复部署上的活动。

    ```
        kubectl -n kube-system describe deployment ibm-worker-recovery
    ```
    {: pre}

6. 如果未看到自动恢复部署上有活动，可以通过运行自动恢复图表定义中包含的测试来检查 Helm 部署。

    ```
    helm test ibm-worker-recovery
    ```
    {: pre}
