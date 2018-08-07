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




# 在集群中部署应用程序
{: #app}

您可以在 {{site.data.keyword.containerlong}} 中使用 Kubernetes 方法来部署容器中的应用程序，并确保这些应用程序始终保持启动并正常运行。例如，可以执行滚动更新以及回滚，而不给用户造成任何停机时间。
{: shortdesc}

通过单击下图中的某个区域可了解用于部署应用程序的常规步骤。

<img usemap="#d62e18" border="0" class="image" id="basic_deployment_process" src="images/basic_deployment_process.png" width="780" style="width:780px;" alt="基本部署过程"/>
<map name="d62e18" id="d62e18">
<area href="cs_cli_install.html" target="_blank" alt="安装 CLI。" title="安装 CLI。" shape="rect" coords="30, 69, 179, 209" />
<area href="https://kubernetes.io/docs/concepts/configuration/overview/" target="_blank" alt="为应用程序创建配置文件。请查看 Kubernetes 中的最佳实践。" title="为应用程序创建配置文件。请查看 Kubernetes 中的最佳实践。" shape="rect" coords="254, 64, 486, 231" />
<area href="#app_cli" target="_blank" alt="选项 1：通过 Kubernetes CLI 运行配置文件。" title="选项 1：通过 Kubernetes CLI 运行配置文件。" shape="rect" coords="544, 67, 730, 124" />
<area href="#cli_dashboard" target="_blank" alt="选项 2：在本地启动 Kubernetes 仪表板，然后运行配置文件。" title="选项 2：在本地启动 Kubernetes 仪表板，然后运行配置文件。" shape="rect" coords="544, 141, 728, 204" />
</map>


<br />


## 规划高可用性部署
{: #highly_available_apps}

设置在多个工作程序节点和集群上分发得越广泛，用户使用应用程序时遭遇停机时间的可能性就越低。
{: shortdesc}

查看以下潜在的应用程序设置（按可用性程度从低到高排序）。


![应用程序的高可用性阶段](images/cs_app_ha_roadmap.png)

1.  部署具有 n+2 个 pod，这些 pod 由副本集管理。
2.  部署具有 n+2 个 pod，这些 pod 由副本集管理并跨同一位置的多个节点分布（反亲缘关系）。
3.  部署具有 n+2 个 pod，这些 pod 由副本集管理并跨不同位置的多个节点分布（反亲缘关系）。
4.  部署具有 n+2 个 pod，这些 pod 由副本集管理并跨不同区域的多个节点分布（反亲缘关系）。




### 提高应用程序的可用性
{: #increase_availability}

<dl>
  <dt>使用部署和副本集来部署应用程序及其依赖项</dt>
    <dd><p>部署是一种 Kubernetes 资源，可用于声明应用程序的所有组件以及应用程序的依赖项。通过部署，您不必记下所有这些步骤，而可以将重点放在应用程序上。</p>
    <p>部署多个 pod 时，会自动为部署创建副本集；副本集用于监视这些 pod，并确保始终有所需数量的 pod 正常运行。pod 发生故障时，副本集会将无响应的 pod 替换为新的 pod。</p>
    <p>您可以使用部署来定义应用程序的更新策略，包括在滚动更新期间要添加的 pod 数，以及允许同时不可用的 pod 数。执行滚动更新时，部署将检查修订版是否有效，并在检测到故障时停止应用。</p>
    <p>通过部署，可以同时部署多个具有不同标志的修订版。例如，您可以先测试部署，然后再决定是否将其推送到生产环境。</p>
    <p>部署允许您跟踪任何已部署的修订版。如果遇到更新无法按预期运行的情况，可以使用此历史记录回滚到上一个版本。</p></dd>
  <dt>包含足够多的副本用于应用程序的工作负载，在此基础上再额外增加两个副本</dt>
    <dd>要使应用程序具有更高可用性且在出现故障时能够更快恢复，请考虑在处理预期工作负载所需最低要求的副本数基础上，再包含额外的副本。在某个 pod 崩溃且副本集尚未恢复已崩溃 pod 的情况下，额外的副本可处理工作负载。要针对同时发生两个故障的情况进行防护，请包含两个额外的副本。此设置是 N+2 模式，其中 N 是处理入局工作负载的副本数，+2 是额外两个副本。只要集群具有足够的空间，就可以拥有任意数量的 pod。</dd>
  <dt>跨多个节点分布 pod（反亲缘关系）</dt>
    <dd><p>创建部署时，各个 pod 可部署到同一工作程序节点。这称为亲缘关系或共存。为了保护应用程序不受工作程序节点故障的影响，可以在标准集群中使用 <em>podAntiAffinity</em> 选项将部署配置为跨多个工作程序节点分布 pod。可以定义两种类型的 pod 反亲缘关系：首选或必需。有关更多信息，请参阅有关<a href="https://kubernetes.io/docs/concepts/configuration/assign-pod-node/" rel="external" target="_blank" title="（在新选项卡或窗口中打开）">为节点分配 pod</a> 的 Kubernetes 文档。</p>
    <p><strong>注</strong>：在需要反亲缘关系的情况下，可以部署的副本数不超过您拥有的工作程序节点数。例如，如果集群中有 3 个工作程序节点，但在 YAML 文件中定义了 5 个副本，那么仅部署 3 个副本。每个副本位于不同的工作程序节点上。剩余的 2 个副本保持暂挂状态。如果将另一个工作程序节点添加到集群，那么其中一个剩余副本将自动部署到这一新的工作程序节点。<p>
    <p><strong>部署 YAML 文件示例</strong>：<ul>
    <li><a href="https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/nginx_preferredAntiAffinity.yaml" rel="external" target="_blank" title="（在新选项卡或窗口中打开）">具有首选 pod 反亲缘关系的 Nginx 应用程序。</a></li>
    <li><a href="https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/liberty_requiredAntiAffinity.yaml" rel="external" target="_blank" title="（在新选项卡或窗口中打开）">具有必需 pod 反亲缘关系的 IBM® WebSphere® Application Server Liberty 应用程序。</a></li></ul></p>
    </dd>
<dt>跨多个专区或区域分布 pod</dt>
  <dd>为了保护应用程序不受位置或区域故障的影响，可以在另一个位置或区域中创建第二个集群，并使用部署 YAML 来部署应用程序的重复副本集。通过在集群前端添加共享路径和负载均衡器，可以跨位置和区域分布工作负载。有关更多信息，请参阅[集群的高可用性](cs_clusters.html#clusters)。</dd>
</dl>


### 最低应用程序部署
{: #minimal_app_deployment}

免费或标准集群中的基本应用程序部署可能包含以下组件。
{: shortdesc}

![部署设置](images/cs_app_tutorial_components1.png)

要部署图中所示的最简应用程序的组件，请使用类似于以下示例的配置文件：
```
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: ibmliberty
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: ibmliberty
    spec:
      containers:
      - name: ibmliberty
        image: registry.bluemix.net/ibmliberty:latest
        ports:
        - containerPort: 9080        
---
apiVersion: v1
kind: Service
metadata:
  name: ibmliberty-service
  labels:
    app: ibmliberty
spec:
  selector:
    app: ibmliberty
  type: NodePort
  ports:
   - protocol: TCP
     port: 9080
```
{: codeblock}

**注**：要公开服务，请确保在服务的 `spec.selector` 部分中使用的键/值对与部署 YAML 的 `spec.template.metadata.labels` 部分中使用的键/值对相同。要了解有关每个组件的更多信息，请查看 [Kubernetes 基础知识](cs_tech.html#kubernetes_basics)。

<br />




## 启动 Kubernetes 仪表板
{: #cli_dashboard}

在本地系统上打开 Kubernetes 仪表板，以查看有关集群及其工作程序节点的信息。
[在 GUI 中](#db_gui)，可以使用方便的一次单击按钮来访问该仪表板。[通过 CLI](#db_cli)，可以访问该仪表板或使用自动化过程中的步骤，例如针对 CI/CD 管道的步骤。
{:shortdesc}

开始之前，请[设定 CLI 的目标](cs_cli_install.html#cs_cli_configure)为集群。此任务需要[管理员访问策略](cs_users.html#access_policies)。验证您当前的[访问策略](cs_users.html#infra_access)。

可以使用缺省端口或设置自己的端口来启动集群的 Kubernetes 仪表板。

**通过 GUI 启动 Kubernetes 仪表板**
{: #db_gui}

1.  登录到 [{{site.data.keyword.Bluemix_notm}} GUI](https://console.bluemix.net/)。
2.  从菜单栏的概要文件中，选择要使用的帐户。
3.  在菜单中，单击**容器**。
4.  在**集群**页面上，单击要访问的集群。
5.  在集群详细信息页面中，单击 **Kubernetes 仪表板**按钮。

**通过 CLI 启动 Kubernetes 仪表板**
{: #db_cli}

*  对于带有 Kubernetes V1.7.16 或更低版本主节点的集群：

    1.  使用缺省端口号设置代理。

        ```
                kubectl proxy
        ```
        {: pre}

        输出：

        ```
                Starting to serve on 127.0.0.1:8001
        ```
        {: screen}

    2.  在 Web 浏览器中打开 Kubernetes 仪表板。

        ```
                http://localhost:8001/ui
        ```
        {: codeblock}

*  对于带有 Kubernetes V1.8.2 或更高版本主节点的集群：

    1.  获取 Kubernetes 的凭证。

        ```
                kubectl config view -o jsonpath='{.users[0].user.auth-provider.config.id-token}'
        ```
        {: pre}

    2.  复制输出中显示的 **id-token** 值。

    3.  使用缺省端口号设置代理。

        ```
                kubectl proxy
        ```
        {: pre}

        输出示例：

        ```
                Starting to serve on 127.0.0.1:8001
        ```
        {: screen}

    4.  登录到仪表板。

      1.  在浏览器中，浏览至以下 URL：

          ```
                      http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/
            ```
          {: codeblock}

      2.  在登录页面中，选择**令牌**认证方法。

      3.  接下来，将先前复制的 **id-token** 值粘贴到**令牌**字段中，然后单击**登录**。

对 Kubernetes 仪表板操作完毕后，使用 `CTRL+C` 以退出 `proxy` 命令。退出后，Kubernetes 仪表板不再可用。运行 `proxy` 命令以重新启动 Kubernetes 仪表板。

[接下来，可以通过仪表板来运行配置文件。](#app_ui)


<br />




## 创建私钥
{: #secrets}

Kubernetes 私钥是一种存储保密信息（如用户名、密码或密钥）的安全方法。
{:shortdesc}

<table>
<caption>要通过任务以私钥形式存储的必需文件</caption>
<thead>
<th>任务</th>
<th>要以私钥形式存储的必需文件</th>
</thead>
<tbody>
<tr>
<td>向集群添加服务</td>
<td>无。将服务绑定到集群时，会创建私钥。</td>
</tr>
<tr>
<td>可选：如果不打算使用 ingress-secret，请将 Ingress 服务配置为使用 TLS。<p><b>注</b>：缺省情况下已启用 TLS，并且已经为 TLS 连接创建私钥。


要查看缺省 TLS 私钥：
<pre>
bx cs cluster-get &lt;cluster_name_or_ID&gt; | grep "Ingress secret"
</pre>
</p>
要改为创建自己的私钥，请完成本主题中的步骤。</td>
<td>服务器证书和密钥：<code>server.crt</code> 和 <code>server.key</code></td>
<tr>
<td>创建相互认证注释。</td>
<td>CA 证书：<code>ca.crt</code></td>
</tr>
</tbody>
</table>

有关在私钥中可以存储哪些内容的更多信息，请参阅 [Kubernetes 文档](https://kubernetes.io/docs/concepts/configuration/secret/)。



要使用证书创建私钥，请执行以下操作：

1. 通过证书提供者生成认证中心 (CA) 证书和密钥。如果您有自己的域，请为您的域购买正式的 TLS 证书。如果是为了进行测试，您可以生成自签名证书。

 **重要信息**：请确保每个证书的 [CN](https://support.dnsimple.com/articles/what-is-common-name/) 都是不同的。

 必须验证客户机证书和客户机密钥，一直验证到可信根证书（在本例中为 CA 证书）。示例：

 ```
 客户机证书：由中间证书签发
 中间证书：由根证书签发
 根证书：由其自身签发
 ```
 {: codeblock}

2. 将证书创建为 Kubernetes 私钥。

   ```
   kubectl create secret generic <secret_name> --from-file=<cert_file>=<cert_file>
   ```
   {: pre}

     示例：
   - TLS 连接：

     ```
          kubectl create secret tls <secret_name> --from-file=tls.crt=server.crt --from-file=tls.key=server.key
     ```
     {: pre}

   - 相互认证注释：

     ```
          kubectl create secret generic <secret_name> --from-file=ca.crt=ca.crt
     ```
     {: pre}

<br />





## 使用 GUI 部署应用程序
{: #app_ui}

使用 Kubernetes 仪表板将应用程序部署到集群时，部署资源会在集群中自动创建、更新和管理 pod。
{:shortdesc}

开始之前：

-   安装必需的 [CLI](cs_cli_install.html#cs_cli_install)。
-   [设定 CLI 的目标](cs_cli_install.html#cs_cli_configure)为集群。

要部署应用程序，请执行以下操作：

1.  在 Kubernetes [仪表板](#cli_dashboard)中，单击 **+ 创建**。
2.  通过下面两种方式中的一种来输入应用程序详细信息。
  * 选择**在下面指定应用程序详细信息**，然后输入详细信息。
  * 选择**上传 YAML 或 JSON 文件**以上传应用程序[配置文件 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/)。

  需要配置文件的相关帮助？请查看此 [YAML 文件示例 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/IBM-Cloud/kube-samples/blob/master/deploy-apps-clusters/deploy-ibmliberty.yaml)。在此示例中，将从美国南部区域中的 **ibmliberty** 映像部署容器。
  使用 Kubernetes 资源时，请了解有关[确保个人信息安全](cs_secure.html#pi)的更多信息。
  {: tip}

3.  通过下列其中一种方式验证是否已成功部署应用程序。
  * 在 Kubernetes 仪表板中，单击**部署**。这将显示成功部署的列表。
  * 如果应用程序[公开可用](cs_network_planning.html#public_access)，请浏览至 {{site.data.keyword.containerlong}} 仪表板中的集群概述页面。复制位于“集群摘要”部分中的子域，并将其粘贴到浏览器以查看应用程序。

<br />


## 使用 CLI 部署应用程序
{: #app_cli}

创建集群后，可以使用 Kubernetes CLI 将应用程序部署到该集群。
{:shortdesc}

开始之前：

-   安装必需的 [CLI](cs_cli_install.html#cs_cli_install)。
-   [设定 CLI 的目标](cs_cli_install.html#cs_cli_configure)为集群。

要部署应用程序，请执行以下操作：

1.  根据 [Kubernetes 最佳实践 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/configuration/overview/) 创建配置文件。通常，配置文件包含要在 Kubernetes 中创建的每个资源的配置详细信息。脚本可能包含以下一个或多个部分：

    -   [部署 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)：定义 pod 和副本集的创建。pod 包含单个容器化应用程序，而副本集用于控制多个 pod 实例。

    -   [服务 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/services-networking/service/)：使用工作程序节点或负载均衡器公共 IP 地址或公共 Ingress 路径，提供对 pod 的前端访问。

    -   [Ingress ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/services-networking/ingress/)：指定一种类型的负载均衡器，以提供用于公开访问应用程序的路径。

    使用 Kubernetes 资源时，请了解有关[确保个人信息安全](cs_secure.html#pi)的更多信息。

2.  在集群上下文中运行配置文件。

    ```
        kubectl apply -f config.yaml
    ```
    {: pre}

3.  如果使用 NodePort 服务、LoadBalancer 服务或 Ingress 使应用程序公共可用，请验证您是否可以访问该应用程序。

<br />




## 在 GPU 机器上部署应用程序
{: #gpu_app}

如果您有[裸机图形处理单元 (GPU) 机器类型](cs_clusters.html#shared_dedicated_node)，那么可以在工作程序节点上安排数学密集型工作负载。例如，您可以运行使用计算统一设备体系结构 (CUDA) 平台的 3D 应用程序，使处理负载在 GPU 和 CPU 上共享以提高性能。
{:shortdesc}

在以下步骤中，您将了解如何部署需要 GPU 的工作负载。您还可以[部署应用程序](#app_ui)，这些应用程序无需处理 GPU 和 CPU 上的工作负载。之后，您可能会发现使用数学密集型工作负载（例如，[此 Kubernetes 演示 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/pachyderm/pachyderm/tree/master/doc/examples/ml/tensorflow) 中的 [TensorFlow ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.tensorflow.org/) 机器学习框架）非常有用。

开始之前：
* [创建裸机 GPU 机器类型](cs_clusters.html#clusters_cli)。请注意，完成此过程可能需要超过 1 个工作日的时间。
* 集群主节点和 GPU 工作程序节点必须运行 Kubernetes V1.10 或更高版本。

要在 GPU 机器上执行工作负载，请执行以下操作：
1.  创建 YAML 文件。在此示例中，`Job` YAML 管理类似批处理的工作负载的方式是，生成一个运行时间很短的 pod，该 pod 一直运行到将其安排为完成的命令成功终止。

    **重要信息**：对于 GPU 工作负载，必须始终在 YAML 规范中提供 `resources: limits: nvidia.com/gpu` 字段。

    ```yaml
    apiVersion: batch/v1
    kind: Job
    metadata:
      name: nvidia-smi
      labels:
        name: nvidia-smi
    spec:
      template:
        metadata:
          labels:
            name: nvidia-smi
        spec:
          containers:
          - name: nvidia-smi
            image: nvidia/cuda:9.1-base-ubuntu16.04
            command: [ "/usr/test/nvidia-smi" ]
            imagePullPolicy: IfNotPresent
            resources:
              limits:
                nvidia.com/gpu: 2
            volumeMounts:
            - mountPath: /usr/test
              name: nvidia0
          volumes:
            - name: nvidia0
              hostPath:
                path: /usr/bin
          restartPolicy: Never
    ```
    {: codeblock}

    <table>
    <caption>YAML 的组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td>元数据和标签名称</td>
    <td>为作业提供名称和标签，并在文件的元数据和 `spec template` 元数据中使用相同的名称。例如，`nvidi-smi`。</td>
    </tr>
    <tr>
    <td><code>containers/image</code></td>
    <td>提供容器是其运行实例的映像。在此示例中，该值设置为使用 DockerHub CUDA 映像：<code>nvidia/cuda:9.1-base-ubuntu16.04</code></td>
    </tr>
    <tr>
    <td><code>containers/command</code></td>
    <td>指定要在容器中运行的命令。在此示例中，<code>[ "/usr/test/nvidia-smi" ]</code> 命令引用 GPU 机器上的二进制文件，因此您还必须设置卷安装。</td>
    </tr>
    <tr>
    <td><code>containers/imagePullPolicy</code></td>
    <td>要仅在映像当前不在工作程序节点上时才拉取新映像，请指定 <code>IfNotPresent</code>。</td>
    </tr>
    <tr>
    <td><code>resources/limits</code></td>
    <td>对于 GPU 机器，必须指定资源限制。Kubernetes [设备插件 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/cluster-administration/device-plugins/) 会设置缺省资源请求以与该限制相匹配。
    <ul><li>必须将键指定为 <code>nvidia.com/gpu</code>。</li>
    <li>输入整数表示请求的 GPU 数，例如 <code>2</code>。<strong>注</strong>：容器 pod 不会共享 GPU，并且 GPU 也无法超量使用。例如，如果只有 1 个 `mg1c.16x128` 机器，那么该机器中只有 2 个 GPU，因此可以指定的最大值为 `2`。</li></ul></td>
    </tr>
    <tr>
    <td><code>volumeMounts</code></td>
    <td>对安装到容器上的卷命名，例如 <code>nvipi0</code>。指定该卷在容器上的 <code>mountPath</code>。在此示例中，路径 <code>/usr/test</code> 与作业容器命令中使用的路径相匹配。</td>
    </tr>
    <tr>
    <td><code>volumes</code></td>
    <td>对作业卷命名，例如 <code>nvia0</code>。在 GPU 工作程序节点的 <code>hostPath</code> 中，指定卷在主机上的 <code>path</code>，在此示例中为 <code>/usr/bin</code>。容器 <code>mountPath</code> 会映射到主机卷 <code>path</code>，这使此作业能够访问 GPU 工作程序节点上供容器命令运行的 NVIDIA 二进制文件。</td>
    </tr>
    </tbody></table>

2.  应用 YAML 文件。例如：

    ```
    kubectl apply -f nvidia-smi.yaml
    ```
    {: pre}

3.  通过按 `nvipia-sim` 标签过滤 pod 来检查作业 pod。验证 **STATUS** 是否为 **Completed**。

    ```
    kubectl get pod -a -l 'name in (nvidia-sim)'
    ```
    {: pre}

    输出示例：
    ```
    NAME                  READY     STATUS      RESTARTS   AGE
    nvidia-smi-ppkd4      0/1       Completed   0          36s
    ```
    {: screen}

4.  对 pod 执行 describe 命令，以查看 GPU 设备插件是如何安排 pod 的。
    * 在 `Limits` 和 `Requests` 字段中，确保指定的资源限制与设备插件自动设置的请求相匹配。
    * 在 Events 中，验证是否已将 pod 分配给 GPU 工作程序节点。

    ```
    kubectl describe pod nvidia-smi-ppkd4
    ```
    {: pre}

    输出示例：
    ```
    Name:           nvidia-smi-ppkd4
    Namespace:      default
    ...
    Limits:
     nvidia.com/gpu:  2
    Requests:
     nvidia.com/gpu:  2
    ...
    Events:
    Type    Reason                 Age   From                     Message
    ----    ------                 ----  ----                     -------
    Normal  Scheduled              1m    default-scheduler        Successfully assigned nvidia-smi-ppkd4 to 10.xxx.xx.xxx
    ...
    ```
    {: screen}

5.  要验证作业是否使用了 GPU 来计算其工作负载，可以检查日志。从作业发出的 `[ "/usr/test/nvidia-smi" ]` 命令查询 GPU 工作程序节点上的 GPU 设备状态。

    ```
    kubectl logs nvidia-sim-ppkd4
    ```
    {: pre}

    输出示例：
    ```
    +-----------------------------------------------------------------------------+
    | NVIDIA-SMI 390.12                 Driver Version: 390.12                    |
    |-------------------------------+----------------------+----------------------+
    | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
    | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
    |===============================+======================+======================|
    |   0  Tesla K80           Off  | 00000000:83:00.0 Off |                  Off |
    | N/A   37C    P0    57W / 149W |      0MiB / 12206MiB |      0%      Default |
    +-------------------------------+----------------------+----------------------+
    |   1  Tesla K80           Off  | 00000000:84:00.0 Off |                  Off |
    | N/A   32C    P0    63W / 149W |      0MiB / 12206MiB |      1%      Default |
    +-------------------------------+----------------------+----------------------+

    +-----------------------------------------------------------------------------+
    | Processes:                                                       GPU Memory |
    |  GPU       PID   Type   Process name                             Usage      |
    |=============================================================================|
    |  No running processes found                                                 |
    +-----------------------------------------------------------------------------+
    ```
    {: screen}

    在此示例中，您看到两个 GPU 都用于执行作业，因为这两个 GPU 均已安排在工作程序节点中。如果限制设置为 1，那么仅显示 1 个 GPU。

## 扩展应用程序 
{: #app_scaling}

使用 Kubernetes，可以启用[水平 pod 自动扩展 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/)，以根据 CPU 来自动增加或减少应用程序的实例数。
{:shortdesc}

需要有关扩展 Cloud Foundry 应用程序的信息？请查看 [IBM Auto-Scaling for {{site.data.keyword.Bluemix_notm}}](/docs/services/Auto-Scaling/index.html)。
{: tip}

开始之前：
- [设定 CLI 的目标](cs_cli_install.html#cs_cli_configure)为集群。
- 必须在要自动扩展的集群中部署 heapster 监视。

步骤：

1.  通过 CLI 将应用程序部署到集群。部署应用程序时，必须请求 CPU。


    ```
        kubectl run <app_name> --image=<image> --requests=cpu=<cpu> --expose --port=<port_number>
    ```
    {: pre}

    <table>
    <caption>kubectl run 的命令组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解此命令的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--image</code></td>
    <td>要部署的应用程序。</td>
    </tr>
    <tr>
    <td><code>--request=cpu</code></td>
    <td>容器的必需 CPU，以千分之一核心数为单位指定。例如，<code>--requests=200m</code>。</td>
    </tr>
    <tr>
    <td><code>--expose</code></td>
    <td>为 true 时，创建外部服务。</td>
    </tr>
    <tr>
    <td><code>--port</code></td>
    <td>应用程序对外部可用的端口。</td>
    </tr></tbody></table>

    对于更复杂的部署，您可能需要创建[配置文件](#app_cli)。
    {: tip}

2.  创建自动扩展程序并定义策略。有关使用 `kubectl autoscale` 命令的更多信息，请参阅 [Kubernetes 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://v1-8.docs.kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#autoscale)。

    ```
        kubectl autoscale deployment <deployment_name> --cpu-percent=<percentage> --min=<min_value> --max=<max_value>
    ```
    {: pre}

    <table>
    <caption>kubectl autoscale 的命令组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解此命令的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--cpu-percent</code></td>
    <td>Horizontal Pod Autoscaler 保持的平均 CPU 使用率，以百分比为单位指定。</td>
    </tr>
    <tr>
    <td><code>--min</code></td>
    <td>用于保持指定 CPU 使用率百分比的最小部署 Pod 数。</td>
    </tr>
    <tr>
    <td><code>--max</code></td>
    <td>用于保持指定 CPU 使用率百分比的最大部署 Pod 数。</td>
    </tr>
    </tbody></table>


<br />


## 管理滚动部署
{: #app_rolling}

可以通过自动和受控方式来管理如何应用您的更改。如果应用未按计划开展，那么可以将部署回滚到先前的修订版。
{:shortdesc}

开始之前，请创建[部署](#app_cli)。

1.  [应用 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#updating-a-deployment) 更改。例如，您可能希望更改初始部署中使用的映像。

    1.  获取部署名称。

        ```
                kubectl get deployments
        ```
        {: pre}

    2.  获取 pod 名称。

        ```
                    kubectl get pods
            ```
        {: pre}

    3.  获取在 pod 中运行的容器的名称。

        ```
                kubectl describe pod <pod_name>
        ```
        {: pre}

    4.  设置新映像以供部署使用。

        ```
                kubectl set image deployment/<deployment_name><container_name>=<image_name>
        ```
        {: pre}

    运行命令时，更改会立即应用并会记录在应用历史记录中。

2.  检查部署的状态。

    ```
        kubectl rollout status deployments/<deployment_name>
    ```
    {: pre}

3.  回滚更改。
    1.  查看部署的应用历史记录，并确定上次部署的修订版号。

        ```
                kubectl rollout history deployment/<deployment_name>
        ```
        {: pre}

        **提示**：要查看特定修订版的详细信息，请包含相应的修订版号。

        ```
                kubectl rollout history deployment/<deployment_name> --revision=<number>
        ```
        {: pre}

    2.  回滚到先前的版本或指定修订版。要回滚到先前的版本，请使用以下命令。

        ```
                kubectl rollout undo deployment/<depoyment_name> --to-revision=<number>
        ```
        {: pre}

<br />

