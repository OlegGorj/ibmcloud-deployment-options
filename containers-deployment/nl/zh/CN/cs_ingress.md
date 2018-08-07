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




# 使用 Ingress 公开应用程序
{: #ingress}

通过创建在 {{site.data.keyword.containerlong}} 中由 IBM 提供的应用程序负载均衡器管理的 Ingress 资源，公开 Kubernetes 集群中的多个应用程序。
{:shortdesc}

## 使用 Ingress 管理网络流量
{: #planning}

Ingress 是一种 Kubernetes 服务，通过将公共或专用请求转发到应用程序，均衡集群中的网络流量工作负载。可以使用 Ingress 通过唯一的公共或专用路径，向公用或专用网络公开多个应用程序服务。
{:shortdesc}



Ingress 由两个组件组成：
<dl>
<dt>应用程序负载均衡器</dt>
<dd>应用程序负载均衡器 (ALB) 是一种外部负载均衡器，用于侦听入局 HTTP、HTTPS、TCP 或 UDP 服务请求，并将这些请求转发到相应的应用程序 pod。创建标准集群时，{{site.data.keyword.containershort_notm}} 会自动为集群创建高可用性 ALB，并为其分配唯一公共路径。该公共路径链接到在集群创建期间供应到 IBM Cloud infrastructure (SoftLayer) 帐户中的可移植公共 IP 地址。另外，还会自动创建缺省专用 ALB，但不会自动启用该 ALB。</dd>
<dt>Ingress 资源</dt>
<dd>要使用 Ingress 公开应用程序，必须为应用程序创建 Kubernetes 服务，并通过定义 Ingress 资源向 ALB 注册此服务。Ingress 资源是一种 Kubernetes 资源，定义了有关如何对应用程序的入局请求进行路由的规则。Ingress 资源还指定应用程序服务的路径，该路径附加到公共路径，以构成唯一的应用程序 URL，例如 `mycluster.us-south.containers.appdomain.cloud/myapp`。<br></br><strong>注</strong>：从 2018 年 5 月 24 日开始，更改了新集群的 Ingress 子域格式。<ul><li>对于 2018 年 5 月 24 日之后创建的集群，将为其分配新格式的子域：<code>&lt;cluster_name&gt;.&lt;region&gt;.containers.appdomain.cloud</code>。</li><li>2018 年 5 月 24 日之前创建的集群将继续使用分配的旧格式子域：<code>&lt;cluster_name&gt;.&lt;region&gt;.containers.mybluemix.net</code>。</li></ul></dd>
</dl>

下图显示 Ingress 如何将通信从因特网定向到应用程序：

<img src="images/cs_ingress.png" width="550" alt="使用 Ingress 在 {{site.data.keyword.containershort_notm}} 中公开应用程序" style="width:550px; border-style: none"/>

1. 用户通过访问应用程序的 URL 向应用程序发送请求。此 URL 是已公开应用程序的公共 URL，并附加有 Ingress 资源路径，例如 `mycluster.us-south.containers.appdomain.cloud/myapp`。

2. 充当全局负载均衡器的 DNS 系统服务会将该 URL 解析为集群中缺省公共 ALB 的可移植公共 IP 地址。请求将路由到应用程序的 Kubernetes ALB 服务。

3. Kubernetes 服务将请求路由到 ALB。

4. ALB 会检查集群中 `myapp` 路径的路由规则是否存在。如果找到匹配的规则，那么会根据在 Ingress 资源中定义的规则，将请求转发到部署了应用程序的 pod。如果集群中部署了多个应用程序实例，那么 ALB 会在应用程序 pod 之间对请求进行负载均衡。



<br />


## 先决条件
{: #config_prereqs}

开始使用 Ingress 之前，请查看以下先决条件。
{:shortdesc}

**所有 Ingress 配置的先决条件：**
- Ingress 仅可用于标准集群，并要求集群中至少有两个工作程序节点以确保高可用性，同时要求定期进行更新。
- 设置 Ingress 需要[管理员访问策略](cs_users.html#access_policies)。验证您当前的[访问策略](cs_users.html#infra_access)。



<br />


## 规划单个或多个名称空间的联网
{: #multiple_namespaces}

在有要公开的应用程序的每个名称空间中，至少需要一个 Ingress 资源。
{:shortdesc}

<dl>
<dt>所有应用程序位于一个名称空间中</dt>
<dd>如果集群中的应用程序全部位于同一名称空间中，那么至少需要一个 Ingress 资源，以定义在其中公开的应用程序的路由规则。例如，如果您有由服务在开发名称空间中公开的 `app1` 和 `app2`，那么可以在该名称空间中创建 Ingress 资源。该资源将 `domain.net` 指定为主机，并向 `domain.net` 注册每个应用程序侦听的路径。
<br></br><img src="images/cs_ingress_single_ns.png" width="300" alt="每个名称空间至少需要一个资源。" style="width:300px; border-style: none"/>
</dd>
<dt>应用程序位于多个名称空间中</dt>
<dd>如果集群中的应用程序位于不同的名称空间中，那么必须为每个名称空间至少创建一个资源，以定义在其中公开的应用程序的规则。要向集群的 Ingress ALB 注册多个 Ingress 资源，必须使用通配符域。如果已注册通配符域（例如 `*.mycluster.us-sous.containers.appdomain.cloud`），那么多个子域将全部解析为同一主机。然后，可以在每个名称空间中创建 Ingress 资源，并在每个 Ingress 资源中指定不同的子域。
<br><br>
例如，假设有以下场景：<ul>
<li>您有同一应用程序的两个版本 `app1` 和 `app3` 用于测试。</li>
<li>您将应用程序部署在同一集群的两个不同名称空间中：`app1` 部署在开发名称空间中，`app3` 部署在编译打包名称空间中。</li></ul>
要使用同一集群 ALB 来管理这两个应用程序的流量，请创建以下服务和资源：<ul>
<li>在开发名称空间中创建 Kubernetes 服务，用于公开 `app1`。</li>
<li>在开发名称空间中创建 Ingress 资源，用于将主机指定为 `dev.mycluster.us-south.containers.appdomain.cloud`。</li>
<li>在编译打包名称空间中创建 Kubernetes 服务，用于公开 `app3`。</li>
<li>在编译打包名称空间中创建 Ingress 资源，用于将主机指定为 `stage.mycluster.us-south.containers.appdomain.cloud`。</li></ul></br>
<img src="images/cs_ingress_multi_ns.png" alt="在名称空间内，使用一个或多个资源中的子域。" style="border-style: none"/>
现在，这两个 URL 会解析为同一个域，因此都由同一 ALB 进行维护。但是，由于编译打包名称空间中的资源已向 `stage` 子域注册，因此 Ingress ALB 会将来自 `stage.mycluster.us-south.containers.appdomain.cloud/app3` URL 的请求正确路由到仅 `app3`。</dd>
</dl>

**注**：
* 缺省情况下，已为集群注册 IBM 提供的 Ingress 子域通配符 `*.<cluster_name>.<region>.containers.appdomain.cloud`。但是，IBM 提供的 Ingress 子域通配符不支持 TLS。
* 如果要使用定制域，您必须将定制域注册为通配符域，例如 `*.custom_domain.net`。要使用 TLS，必须获取通配符证书。

### 一个名称空间内多个域

在单个名称空间中，可以使用一个域来访问该名称空间中的所有应用程序。如果要对单个名称空间中的应用程序使用不同的域，请使用通配符域。如果已注册通配符域（例如 `*.mycluster.us-sous.containers.appdomain.cloud`），那么多个子域将全部解析为同一主机。然后，可以使用一个资源来指定该资源内的多个子域主机。或者，可以在该名称空间中创建多个 Ingress 资源，并在每个 Ingress 资源中指定不同的子域。


<img src="images/cs_ingress_single_ns_multi_subs.png" alt="每个名称空间至少需要一个资源。" style="border-style: none"/>

**注**：
* 缺省情况下，已为集群注册 IBM 提供的 Ingress 子域通配符 `*.<cluster_name>.<region>.containers.appdomain.cloud`。但是，IBM 提供的 Ingress 子域通配符不支持 TLS。
* 如果要使用定制域，您必须将定制域注册为通配符域，例如 `*.custom_domain.net`。要使用 TLS，必须获取通配符证书。

<br />


## 向公众公开集群内部应用程序
{: #ingress_expose_public}

使用公共 Ingress ALB 可向公众公开集群内部应用程序。
{:shortdesc}

开始之前：

-   查看 Ingress [先决条件](#config_prereqs)。
-   如果还没有标准集群，请[创建标准集群](cs_clusters.html#clusters_ui)。
-   [设定 CLI 的目标](cs_cli_install.html#cs_cli_configure)为集群以运行 `kubectl` 命令。

### 步骤 1：部署应用程序并创建应用程序服务
{: #public_inside_1}

首先部署应用程序，并创建 Kubernetes 服务来公开这些应用程序。
{: shortdesc}

1.  [将应用程序部署到集群](cs_app.html#app_cli)。确保在配置文件的 metadata 部分中向部署添加标签，例如 `app: code`。需要此标签才能识别运行应用程序的所有 pod，以便可以将这些 pod 包含在 Ingress 负载均衡中。

2.   针对要公开的每个应用程序创建 Kubernetes 服务。应用程序必须由 Kubernetes 服务公开才能被集群 ALB 包含在 Ingress 负载均衡中。
      1.  打开首选编辑器，并创建服务配置文件，例如名为 `myapp_service.yaml`。
      2.  针对 ALB 将公开的应用程序定义服务。

          ```
          apiVersion: v1
          kind: Service
          metadata:
            name: myapp_service
          spec:
            selector:
              <selector_key>: <selector_value>
            ports:
             - protocol: TCP
               port: 8080
          ```
          {: codeblock}

          <table>
          <thead>
          <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 ALB 服务 YAML 文件的组成部分</th>
          </thead>
          <tbody>
          <tr>
          <td><code>selector</code></td>
          <td>输入要用于将运行应用程序的 pod 设定为目标的标签键 (<em>&lt;selector_key&gt;</em>) 和值 (<em>&lt;selector_value&gt;</em>) 对。要将 pod 设定为目标并将其包含在服务负载均衡中，请确保 <em>&lt;selector_key&gt;</em> 和 <em>&lt;selector_value&gt;</em> 与部署 YAML 的 <code>spec.template.metadata.labels</code> 部分中的键/值对相同。</td>
           </tr>
           <tr>
           <td><code>port</code></td>
           <td>服务侦听的端口。</td>
           </tr>
           </tbody></table>
      3.  保存更改。
      4.  在集群中创建服务。如果应用程序部署在集群中的多个名称空间中，请确保该服务部署到要公开的应用程序所在的名称空间中。

          ```
          kubectl apply -f myapp_service.yaml [-n <namespace>]
          ```
          {: pre}
      5.  针对要公开的每一个应用程序，重复上述步骤。


### 步骤 2：选择应用程序域和 TLS 终止
{: #public_inside_2}

配置公共 ALB 时，可以选择可用于访问应用程序的域，并选择是否使用 TLS 终止。
{: shortdesc}

<dl>
<dt>域</dt>
<dd>可以使用 IBM 提供的域（例如 <code>mycluster-12345.us-south.containers.appdomain.cloud/myapp</code>）通过因特网访问应用程序。要改为使用定制域，可以将定制域映射到 IBM 提供的域或 ALB 的公共 IP 地址。</dd>
<dt>TLS 终止</dt>
<dd>ALB 会对流至集群中应用程序的 HTTP 网络流量进行负载均衡。要同时对入局 HTTPS 连接进行负载均衡，可以配置 ALB 来解密网络流量，然后将已解密的请求转发到集群中公开的应用程序。如果使用的是 IBM 提供的 Ingress 子域，那么可以使用 IBM 提供的 TLS 证书。目前，IBM 提供的通配符子域不支持 TLS。如果使用的是定制域，那么可以使用您自己的 TLS 证书来管理 TLS 终止。</dd>
</dl>

要使用 IBM 提供的 Ingress 域，请执行以下操作：
1. 获取集群的详细信息。将 _&lt;cluster_name_or_ID&gt;_ 替换为要公开的应用程序部署所在集群的名称。

    ```
        bx cs cluster-get <cluster_name_or_ID>
    ```
    {: pre}

    输出示例：

    ```
    Name:                   mycluster
    ID:                     18a61a63c6a94b658596ca93d087aad9
    State:                  normal
    Created:                2018-01-12T18:33:35+0000
    Location:               dal10
    Master URL:             https://169.xx.xxx.xxx:26268
    Ingress Subdomain:      mycluster-12345.us-south.containers.appdomain.cloud
    Ingress Secret:         <tls_secret>
    Workers:                3
    Version:                1.9.7
    Owner Email:            owner@email.com
    Monitoring Dashboard:   <dashboard_URL>
    ```
    {: screen}
2. 获取 **Ingress subdomain** 字段中 IBM 提供的域。如果要使用 TLS，还请获取 **Ingress Secret** 字段中 IBM 提供的 TLS 私钥。
    **注**：如果使用的是通配符子域，那么不支持 TLS。

要使用定制域，请执行以下操作：
1.    创建定制域。要注册定制域，请使用您的域名服务 (DNS) 提供者或 [{{site.data.keyword.Bluemix_notm}} DNS](/docs/infrastructure/dns/getting-started.html#getting-started-with-dns)。
      * 如果希望 Ingress 公开的应用程序位于一个集群的不同名称空间中，请将定制域注册为通配符域，例如 `*.custom_domain.net`。

2.  配置域以将入局网络流量路由到 IBM 提供的 ALB。在以下选项之间进行选择：
    -   通过将 IBM 提供的域指定为规范名称记录 (CNAME)，定义定制域的别名。要找到 IBM 提供的 Ingress 域，请运行 `bx cs cluster-get <cluster_name>` 并查找 **Ingress subdomain** 字段。
    -   通过将 IBM 提供的 ALB 的可移植公共 IP 地址添加为记录，将定制域映射到该 IP 地址。要查找 ALB 的可移植公共 IP 地址，请运行 `bx cs alb-get <public_alb_ID>`.
3.   可选：如果要使用 TLS，请导入或创建 TLS 证书和私钥。如果要使用通配符域，请确保导入或创建通配符证书。
      * 如果要使用存储在 {{site.data.keyword.cloudcerts_long_notm}} 中的 TLS 证书，那么可以通过运行以下命令，将其关联的私钥导入到集群：
        ```
          bx cs alb-cert-deploy --secret-name <secret_name> --cluster <cluster_name_or_ID> --cert-crn <certificate_crn>
          ```
        {: pre}
      * 如果还没有 TLS 证书，请执行以下步骤：
        1. 为域创建以 PEM 格式编码的 TLS 证书和密钥。
        2. 定义使用您的 TLS 证书和密钥的私钥。将 <em>&lt;tls_secret_name&gt;</em> 替换为您的 Kubernetes 私钥的名称，将 <em>&lt;tls_key_filepath&gt;</em> 替换为定制 TLS 密钥文件的路径，并将 <em>&lt;tls_cert_filepath&gt;</em> 替换为定制 TLS 证书文件的路径。
          ```
            kubectl create secret tls <tls_secret_name> --key <tls_key_filepath> --cert <tls_cert_filepath>
            ```
          {: pre}


### 步骤 3：创建 Ingress 资源
{: #public_inside_3}

Ingress 资源定义 ALB 用于将流量路由到应用程序服务的路由规则。
{: shortdesc}

**注：**如果集群的多个名称空间中公开了应用程序，那么每个名称空间至少需要一个 Ingress 资源。但是，每个名称空间都必须使用不同的主机。必须注册通配符域并在每个资源中指定一个不同的子域。有关更多信息，请参阅[规划单个或多个名称空间的联网](#multiple_namespaces)。

1. 打开首选编辑器，并创建 Ingress 配置文件，例如名为 `myingressresource.yaml`。

2. 在配置文件中定义 Ingress 资源，该资源使用 IBM 提供的域或定制域将入局网络流量路由到先前创建的服务。

    不使用 TLS 的示例 YAML：
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingressresource
    spec:
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    使用 TLS 的示例 YAML：
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingressresource
    spec:
      tls:
      - hosts:
        - <domain>
        secretName: <tls_secret_name>
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td><code>tls/hosts</code></td>
    <td>要使用 TLS，请将 <em>&lt;domain&gt;</em> 替换为 IBM 提供的 Ingress 子域或定制域。

    </br></br>
    <strong>注：</strong><ul><li>如果应用程序由服务在一个集群的不同名称空间中公开，请将通配符子域附加到该域的开头，例如 `subdomain1.custom_domain.net` 或 `subdomain1.mycluster.us-south.containers.appdomain.cloud`。请对在集群中创建的每个资源使用唯一子域。</li><li>不要使用 &ast; 表示主机，也不要将主机属性保留为空，以避免创建 Ingress 期间发生失败。</li></ul></td>
    </tr>
    <tr>
    <td><code>tls/secretName</code></td>
    <td><ul><li>如果要使用 IBM 提供的 Ingress 域，请将 <em>&lt;tls_secret_name&gt;</em> 替换为 IBM 提供的 Ingress 私钥的名称。</li><li>如果要使用定制域，请将 <em>&lt;tls_secret_name&gt;</em> 替换为您先前创建用于保存定制 TLS 证书和密钥的私钥。如果已从 {{site.data.keyword.cloudcerts_short}} 导入证书，那么可以运行 <code>bx cs alb-cert-get --cluster <cluster_name_or_ID> --cert-crn <certificate_crn></code> 来查看与 TLS 证书关联的私钥。
        </li><ul><td>
    </tr>
    <tr>
    <td><code>host</code></td>
    <td>将 <em>&lt;domain&gt;</em> 替换为 IBM 提供的 Ingress 子域或定制域。

    </br></br>
    <strong>注：</strong><ul><li>如果应用程序由服务在一个集群的不同名称空间中公开，请将通配符子域附加到该域的开头，例如 `subdomain1.custom_domain.net` 或 `subdomain1.mycluster.us-south.containers.appdomain.cloud`。请对在集群中创建的每个资源使用唯一子域。</li><li>不要使用 &ast; 表示主机，也不要将主机属性保留为空，以避免创建 Ingress 期间发生失败。</li></ul></td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td>将 <em>&lt;app_path&gt;</em> 替换为斜杠或应用程序正在侦听的路径。该路径将附加到 IBM 提供的域或定制域，以创建应用程序的唯一路径。在 Web 浏览器中输入此路径时，网络流量会路由到 ALB。ALB 会查找关联的服务，并将网络流量发送到该服务。然后，该服务将流量转发到在运行应用程序的 pod。</br></br>
        许多应用程序不会侦听特定路径，而是使用根路径和特定端口。在这种情况下，请将根路径定义为 <code>/</code>，并且不要为应用程序指定单独的路径。
                示例：<ul><li>对于 <code>http://domain/</code>，请输入 <code>/</code> 作为路径。</li><li>对于 <code>http://domain/app1_path</code>，请输入 <code>/app1_path</code> 作为路径。</li></ul>
    </br>
    <strong>提示：</strong>要将 Ingress 配置为侦听与应用程序所侦听的路径不同的路径，可以使用 [rewrite 注释](cs_annotations.html#rewrite-path)。</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td>将 <em>&lt;app1_service&gt;</em> 和 <em>&lt;app2_service&gt;</em> 等替换为已创建用于公开应用程序的服务的名称。如果应用程序由服务在一个集群的不同名称空间中公开，请仅包含位于同一名称空间中的应用程序服务。必须为要在其中公开应用程序的每个名称空间创建一个 Ingress 资源。</td>
    </tr>
    <tr>
    <td><code>servicePort</code></td>
    <td>服务侦听的端口。使用针对应用程序创建 Kubernetes 服务时定义的端口。</td>
    </tr>
    </tbody></table>

3.  为集群创建 Ingress 资源。确保资源部署到在资源中指定的应用程序服务所在的名称空间中。

    ```
    kubectl apply -f myingressresource.yaml -n <namespace>
    ```
    {: pre}
4.   验证 Ingress 资源是否已成功创建。

      ```
            kubectl describe ingress myingressresource
      ```
      {: pre}

      1. 如果 events 中的消息描述了资源配置中的错误，请更改资源文件中的值，然后将该文件重新应用于资源。


Ingress 资源已在应用程序服务所在的名称空间中创建。此名称空间中的应用程序已向集群的 Ingress ALB 注册。

### 步骤 4：通过因特网访问应用程序
{: #public_inside_4}

在 Web 浏览器中，输入要访问的应用程序服务的 URL。

```
https://<domain>/<app1_path>
```
{: pre}

如果公开了多个应用程序，请通过更改附加到该 URL 的路径来访问这些应用程序。

```
https://<domain>/<app2_path>
```
{: pre}

如果使用通配符域在不同名称空间中公开应用程序，请使用其各自的子域来访问这些应用程序。

```
http://<subdomain1>.<domain>/<app1_path>
```
{: pre}

```
http://<subdomain2>.<domain>/<app1_path>
```
{: pre}


<br />


## 向公众公开集群外部应用程序
{: #external_endpoint}

通过在公共 Ingress ALB 负载均衡中包含集群外部应用程序，可向公众公开这些应用程序。IBM 提供的域或定制域上的入局公共请求会自动转发到外部应用程序。
{:shortdesc}

开始之前：

-   查看 Ingress [先决条件](#config_prereqs)。
-   如果还没有标准集群，请[创建标准集群](cs_clusters.html#clusters_ui)。
-   [设定 CLI 的目标](cs_cli_install.html#cs_cli_configure)为集群以运行 `kubectl` 命令。
-   确保要包含在集群负载均衡中的外部应用程序可以使用公共 IP 地址进行访问。

### 步骤 1：创建应用程序服务和外部端点
{: #public_outside_1}

首先创建 Kubernetes 服务以公开外部应用程序，然后配置应用程序的 Kubernetes 外部端点。
{: shortdesc}

1.  为集群创建 Kubernetes 服务，以用于将入局请求转发到将创建的外部端点。
    1.  打开首选编辑器，并创建服务配置文件，例如名为 `myexternalservice.yaml`。
    2.  针对 ALB 将公开的应用程序定义服务。

        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: myexternalservice
        spec:
          ports:
           - protocol: TCP
             port: 8080
        ```
        {: codeblock}

        <table>
        <caption>了解 ALB 服务文件的组成部分</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
        </thead>
        <tbody>
        <tr>
        <td><code>metadata/name</code></td>
        <td>将 <em>&lt;myexternalservice&gt;</em> 替换为服务的名称。<p>使用 Kubernetes 资源时，请了解有关[确保个人信息安全](cs_secure.html#pi)的更多信息。</p></td>
        </tr>
        <tr>
        <td><code>port</code></td>
        <td>服务侦听的端口。</td>
        </tr></tbody></table>

    3.  保存更改。
    4.  为集群创建 Kubernetes 服务。

        ```
                kubectl apply -f myexternalservice.yaml
        ```
        {: pre}
2.  配置 Kubernetes 端点以定义要包含在集群负载均衡中的应用程序的外部位置。
    1.  打开首选编辑器，并创建端点配置文件，例如名为 `myexternalendpoint.yaml`。
    2.  定义外部端点。包括所有可用于访问外部应用程序的公共 IP 地址和端口。


        ```
        kind: Endpoints
        apiVersion: v1
        metadata:
          name: myexternalendpoint
        subsets:
          - addresses:
              - ip: <external_IP1>
              - ip: <external_IP2>
            ports:
              - port: <external_port>
        ```
        {: codeblock}

        <table>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
        </thead>
        <tbody>
        <tr>
        <td><code>name</code></td>
        <td>将 <em>&lt;myexternalendpoint&gt;</em> 替换为先前创建的 Kubernetes 服务的名称。</td>
        </tr>
        <tr>
        <td><code>ip</code></td>
        <td>将 <em>&lt;external_IP&gt;</em> 替换为用于连接到外部应用程序的公共 IP 地址。</td>
         </tr>
         <td><code>port</code></td>
         <td>将 <em>&lt;external_port&gt;</em> 替换为外部应用程序侦听的端口。</td>
         </tbody></table>

    3.  保存更改。
    4.  为集群创建 Kubernetes 端点。

        ```
                kubectl apply -f myexternalendpoint.yaml
        ```
        {: pre}

### 步骤 2：选择应用程序域和 TLS 终止
{: #public_outside_2}

配置公共 ALB 时，可以选择可用于访问应用程序的域，并选择是否使用 TLS 终止。
{: shortdesc}

<dl>
<dt>域</dt>
<dd>可以使用 IBM 提供的域（例如 <code>mycluster-12345.us-south.containers.appdomain.cloud/myapp</code>）通过因特网访问应用程序。要改为使用定制域，可以将定制域映射到 IBM 提供的域或 ALB 的公共 IP 地址。</dd>
<dt>TLS 终止</dt>
<dd>ALB 会对流至集群中应用程序的 HTTP 网络流量进行负载均衡。要同时对入局 HTTPS 连接进行负载均衡，可以配置 ALB 来解密网络流量，然后将已解密的请求转发到集群中公开的应用程序。如果使用的是 IBM 提供的 Ingress 子域，那么可以使用 IBM 提供的 TLS 证书。目前，IBM 提供的通配符子域不支持 TLS。如果使用的是定制域，那么可以使用您自己的 TLS 证书来管理 TLS 终止。</dd>
</dl>

要使用 IBM 提供的 Ingress 域，请执行以下操作：
1. 获取集群的详细信息。将 _&lt;cluster_name_or_ID&gt;_ 替换为要公开的应用程序部署所在集群的名称。

    ```
        bx cs cluster-get <cluster_name_or_ID>
    ```
    {: pre}

    输出示例：

    ```
    Name:                   mycluster
    ID:                     18a61a63c6a94b658596ca93d087aad9
    State:                  normal
    Created:                2018-01-12T18:33:35+0000
    Location:               dal10
    Master URL:             https://169.xx.xxx.xxx:26268
    Ingress Subdomain:      mycluster-12345.us-south.containers.appdomain.cloud
    Ingress Secret:         <tls_secret>
    Workers:                3
    Version:                1.9.7
    Owner Email:            owner@email.com
    Monitoring Dashboard:   <dashboard_URL>
    ```
    {: screen}
2. 获取 **Ingress subdomain** 字段中 IBM 提供的域。如果要使用 TLS，还请获取 **Ingress Secret** 字段中 IBM 提供的 TLS 私钥。
    **注**：如果使用的是通配符子域，那么不支持 TLS。

要使用定制域，请执行以下操作：
1.    创建定制域。要注册定制域，请使用您的域名服务 (DNS) 提供者或 [{{site.data.keyword.Bluemix_notm}} DNS](/docs/infrastructure/dns/getting-started.html#getting-started-with-dns)。
      * 如果希望 Ingress 公开的应用程序位于一个集群的不同名称空间中，请将定制域注册为通配符域，例如 `*.custom_domain.net`。

2.  配置域以将入局网络流量路由到 IBM 提供的 ALB。在以下选项之间进行选择：
    -   通过将 IBM 提供的域指定为规范名称记录 (CNAME)，定义定制域的别名。要找到 IBM 提供的 Ingress 域，请运行 `bx cs cluster-get <cluster_name>` 并查找 **Ingress subdomain** 字段。
    -   通过将 IBM 提供的 ALB 的可移植公共 IP 地址添加为记录，将定制域映射到该 IP 地址。要查找 ALB 的可移植公共 IP 地址，请运行 `bx cs alb-get <public_alb_ID>`.
3.   可选：如果要使用 TLS，请导入或创建 TLS 证书和私钥。如果要使用通配符域，请确保导入或创建通配符证书。
      * 如果要使用存储在 {{site.data.keyword.cloudcerts_long_notm}} 中的 TLS 证书，那么可以通过运行以下命令，将其关联的私钥导入到集群：
        ```
          bx cs alb-cert-deploy --secret-name <secret_name> --cluster <cluster_name_or_ID> --cert-crn <certificate_crn>
          ```
        {: pre}
      * 如果还没有 TLS 证书，请执行以下步骤：
        1. 为域创建以 PEM 格式编码的 TLS 证书和密钥。
        2. 定义使用您的 TLS 证书和密钥的私钥。将 <em>&lt;tls_secret_name&gt;</em> 替换为您的 Kubernetes 私钥的名称，将 <em>&lt;tls_key_filepath&gt;</em> 替换为定制 TLS 密钥文件的路径，并将 <em>&lt;tls_cert_filepath&gt;</em> 替换为定制 TLS 证书文件的路径。
          ```
            kubectl create secret tls <tls_secret_name> --key <tls_key_filepath> --cert <tls_cert_filepath>
            ```
          {: pre}


### 步骤 3：创建 Ingress 资源
{: #public_outside_3}

Ingress 资源定义 ALB 用于将流量路由到应用程序服务的路由规则。
{: shortdesc}

**注：**如果要公开多个外部应用程序，并且在[步骤 1](#public_outside_1) 中为应用程序创建的服务位于不同的名称空间中，那么每个名称空间至少需要一个 Ingress 资源。但是，每个名称空间都必须使用不同的主机。必须注册通配符域并在每个资源中指定一个不同的子域。有关更多信息，请参阅[规划单个或多个名称空间的联网](#multiple_namespaces)。

1. 打开首选编辑器，并创建 Ingress 配置文件，例如名为 `myexternalingress.yaml`。

2. 在配置文件中定义 Ingress 资源，该资源使用 IBM 提供的域或定制域将入局网络流量路由到先前创建的服务。

    不使用 TLS 的示例 YAML：
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myexternalingress
    spec:
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<external_app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<external_app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    使用 TLS 的示例 YAML：
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myexternalingress
    spec:
      tls:
      - hosts:
        - <domain>
        secretName: <tls_secret_name>
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<external_app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<external_app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td><code>tls/hosts</code></td>
    <td>要使用 TLS，请将 <em>&lt;domain&gt;</em> 替换为 IBM 提供的 Ingress 子域或定制域。

    </br></br>
    <strong>注：</strong><ul><li>如果应用程序由服务在一个集群的不同名称空间中公开，请将通配符子域附加到该域的开头，例如 `subdomain1.custom_domain.net` 或 `subdomain1.mycluster.us-south.containers.appdomain.cloud`。请对在集群中创建的每个资源使用唯一子域。</li><li>不要使用 &ast; 表示主机，也不要将主机属性保留为空，以避免创建 Ingress 期间发生失败。</li></ul></td>
    </tr>
    <tr>
    <td><code>tls/secretName</code></td>
    <td><ul><li>如果要使用 IBM 提供的 Ingress 域，请将 <em>&lt;tls_secret_name&gt;</em> 替换为 IBM 提供的 Ingress 私钥的名称。</li><li>如果要使用定制域，请将 <em>&lt;tls_secret_name&gt;</em> 替换为您先前创建用于保存定制 TLS 证书和密钥的私钥。如果已从 {{site.data.keyword.cloudcerts_short}} 导入证书，那么可以运行 <code>bx cs alb-cert-get --cluster <cluster_name_or_ID> --cert-crn <certificate_crn></code> 来查看与 TLS 证书关联的私钥。
        </li><ul><td>
    </tr>
    <tr>
    <td><code>rules/host</code></td>
    <td>将 <em>&lt;domain&gt;</em> 替换为 IBM 提供的 Ingress 子域或定制域。

    </br></br>
    <strong>注：</strong><ul><li>如果应用程序由服务在一个集群的不同名称空间中公开，请将通配符子域附加到该域的开头，例如 `subdomain1.custom_domain.net` 或 `subdomain1.mycluster.us-south.containers.appdomain.cloud`。请对在集群中创建的每个资源使用唯一子域。</li><li>不要使用 &ast; 表示主机，也不要将主机属性保留为空，以避免创建 Ingress 期间发生失败。</li></ul></td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td>将 <em>&lt;external_app_path&gt;</em> 替换为斜杠或应用程序正在侦听的路径。该路径将附加到 IBM 提供的域或定制域，以创建应用程序的唯一路径。在 Web 浏览器中输入此路径时，网络流量会路由到 ALB。ALB 会查找关联的服务，并将网络流量发送到该服务。该服务会将流量转发到外部应用程序。应用程序必须设置为侦听此路径，才能接收入局网络流量。</br></br>
        许多应用程序不会侦听特定路径，而是使用根路径和特定端口。在这种情况下，请将根路径定义为 <code>/</code>，并且不要为应用程序指定单独的路径。
                示例：<ul><li>对于 <code>http://domain/</code>，请输入 <code>/</code> 作为路径。</li><li>对于 <code>http://domain/app1_path</code>，请输入 <code>/app1_path</code> 作为路径。</li></ul>
    </br></br>
    <strong>提示：</strong>要将 Ingress 配置为侦听与应用程序所侦听的路径不同的路径，可以使用 [rewrite 注释](cs_annotations.html#rewrite-path)。</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td>将 <em>&lt;app1_service&gt;</em> 和 <em>&lt;app2_service&gt;</em> 等替换为已创建用于公开外部应用程序的服务的名称。如果应用程序由服务在一个集群的不同名称空间中公开，请仅包含位于同一名称空间中的应用程序服务。必须为要在其中公开应用程序的每个名称空间创建一个 Ingress 资源。</td>
    </tr>
    <tr>
    <td><code>servicePort</code></td>
    <td>服务侦听的端口。使用针对应用程序创建 Kubernetes 服务时定义的端口。</td>
    </tr>
    </tbody></table>

3.  为集群创建 Ingress 资源。确保资源部署到在资源中指定的应用程序服务所在的名称空间中。

    ```
    kubectl apply -f myexternalingress.yaml -n <namespace>
    ```
    {: pre}
4.   验证 Ingress 资源是否已成功创建。

      ```
            kubectl describe ingress myingressresource
      ```
      {: pre}

      1. 如果 events 中的消息描述了资源配置中的错误，请更改资源文件中的值，然后将该文件重新应用于资源。


Ingress 资源已在应用程序服务所在的名称空间中创建。此名称空间中的应用程序已向集群的 Ingress ALB 注册。

### 步骤 4：通过因特网访问应用程序
{: #public_outside_4}

在 Web 浏览器中，输入要访问的应用程序服务的 URL。

```
https://<domain>/<app1_path>
```
{: pre}

如果公开了多个应用程序，请通过更改附加到该 URL 的路径来访问这些应用程序。

```
https://<domain>/<app2_path>
```
{: pre}

如果使用通配符域在不同名称空间中公开应用程序，请使用其各自的子域来访问这些应用程序。

```
http://<subdomain1>.<domain>/<app1_path>
```
{: pre}

```
http://<subdomain2>.<domain>/<app1_path>
```
{: pre}


<br />


## 启用缺省专用 ALB
{: #private_ingress}

创建标准集群时，会创建 IBM 提供的专用应用程序负载均衡器 (ALB)，并为其分配可移植专用 IP 地址和专用路径。但是，不会自动启用缺省专用 ALB。要使用专用 ALB 对流至应用程序的专用网络流量进行负载均衡，必须先使用 IBM 提供的可移植专用 IP 地址或您自己的可移植专用 IP 地址来启用此 ALB。
{:shortdesc}

**注**：如果在创建集群时使用了 `--no-subnet` 标志，那么必须先添加可移植专用子网或用户管理的子网，然后才能启用专用 ALB。有关更多信息，请参阅[为集群请求更多子网](cs_subnets.html#request)。

开始之前：

-   如果还没有标准集群，请[创建标准集群](cs_clusters.html#clusters_ui)。
-   [设定 CLI 的目标](cs_cli_install.html#cs_cli_configure)为集群。

要使用预先分配的 IBM 提供的可移植专用 IP 地址来启用专用 ALB，请执行以下操作：

1. 列出集群中的可用 ALB，以获取专用 ALB 的标识。将 <em>&lt;cluser_name&gt;</em> 替换为部署了要公开的应用程序的集群的名称。

    ```
        bx cs albs --cluster <cluser_name>
    ```
    {: pre}

    专用 ALB 的 **Status** 字段为 _disabled_。
    ```
        ALB ID                                            Enabled   Status     Type      ALB IP
    private-cr6d779503319d419ea3b4ab171d12c3b8-alb1   false     disabled   private   -
    public-cr6d779503319d419ea3b4ab171d12c3b8-alb1    true      enabled    public    169.xx.xxx.xxx
    ```
    {: screen}

2. 启用专用 ALB。将 <em>&lt;private_ALB_ID&gt;</em> 替换为上一步的输出中专用 ALB 的标识。

   ```
   bx cs alb-configure --albID <private_ALB_ID> --enable
   ```
   {: pre}

<br>
要使用您自己的可移植专用 IP 地址来启用专用 ALB，请执行以下操作：

1. 为所选 IP 地址配置用户管理的子网，以在集群的专用 VLAN 上路由流量。

   ```
   bx cs cluster-user-subnet-add <cluster_name> <subnet_CIDR> <private_VLAN_ID>
   ```
   {: pre}

   <table>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解命令的组成部分</th>
   </thead>
   <tbody>
   <tr>
   <td><code>&lt;cluser_name&gt;</code></td>
   <td>要公开的应用程序部署所在集群的名称或标识。</td>
   </tr>
   <tr>
   <td><code>&lt;subnet_CIDR&gt;</code></td>
   <td>用户管理的子网的 CIDR。</td>
   </tr>
   <tr>
   <td><code>&lt;private_VLAN_ID&gt;</code></td>
   <td>可用的专用 VLAN 标识。可以通过运行 `bx cs vlans` 来查找可用专用 VLAN 的标识。</td>
   </tr>
   </tbody></table>

2. 列出集群中的可用 ALB，以获取专用 ALB 的标识。

    ```
        bx cs albs --cluster <cluster_name>
    ```
    {: pre}

    专用 ALB 的 **Status** 字段为 _disabled_。
    ```
        ALB ID                                            Enabled   Status     Type      ALB IP
    private-cr6d779503319d419ea3b4ab171d12c3b8-alb1   false     disabled   private   -
    public-cr6d779503319d419ea3b4ab171d12c3b8-alb1    true      enabled    public    169.xx.xxx.xxx
    ```
    {: screen}

3. 启用专用 ALB。将 <em>&lt;private_ALB_ID&gt;</em> 替换为上一步的输出中专用 ALB 的标识，并将 <em>&lt;user_IP&gt;</em> 替换为要使用的用户管理子网中的 IP 地址。

   ```
   bx cs alb-configure --albID <private_ALB_ID> --enable --user-ip <user_IP>
   ```
   {: pre}

<br />


## 向专用网络公开应用程序
{: #ingress_expose_private}

使用专用 Ingress ALB 可将应用程序公开到专用网络。
{:shortdesc}

开始之前：
* 查看 Ingress [先决条件](#config_prereqs)。
* [启用专用应用程序负载均衡器](#private_ingress)。
* 如果您有专用工作程序节点并且要使用外部 DNS 提供者，那么必须[配置具有公用访问权的边缘节点](cs_edge.html#edge)，然后[配置虚拟路由器设备 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/blogs/bluemix/2017/07/kubernetes-and-bluemix-container-based-workloads-part4/)。
* 如果您有专用工作程序节点并且希望仅保持位于专用网络上，那么必须[配置专用内部部署 DNS 服务 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/)，以解析对应用程序的 URL 请求。


### 步骤 1：部署应用程序并创建应用程序服务
{: #private_1}

首先部署应用程序，并创建 Kubernetes 服务来公开这些应用程序。
{: shortdesc}

1.  [将应用程序部署到集群](cs_app.html#app_cli)。确保在配置文件的 metadata 部分中向部署添加标签，例如 `app: code`。需要此标签才能识别运行应用程序的所有 pod，以便可以将这些 pod 包含在 Ingress 负载均衡中。

2.   针对要公开的每个应用程序创建 Kubernetes 服务。应用程序必须由 Kubernetes 服务公开才能被集群 ALB 包含在 Ingress 负载均衡中。
      1.  打开首选编辑器，并创建服务配置文件，例如名为 `myapp_service.yaml`。
      2.  针对 ALB 将公开的应用程序定义服务。

          ```
          apiVersion: v1
          kind: Service
          metadata:
            name: myapp_service
          spec:
            selector:
              <selector_key>: <selector_value>
            ports:
             - protocol: TCP
               port: 8080
          ```
          {: codeblock}

          <table>
          <thead>
          <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 ALB 服务 YAML 文件的组成部分</th>
          </thead>
          <tbody>
          <tr>
          <td><code>selector</code></td>
          <td>输入要用于将运行应用程序的 pod 设定为目标的标签键 (<em>&lt;selector_key&gt;</em>) 和值 (<em>&lt;selector_value&gt;</em>) 对。要将 pod 设定为目标并将其包含在服务负载均衡中，请确保 <em>&lt;selector_key&gt;</em> 和 <em>&lt;selector_value&gt;</em> 与部署 YAML 的 <code>spec.template.metadata.labels</code> 部分中的键/值对相同。</td>
           </tr>
           <tr>
           <td><code>port</code></td>
           <td>服务侦听的端口。</td>
           </tr>
           </tbody></table>
      3.  保存更改。
      4.  在集群中创建服务。如果应用程序部署在集群中的多个名称空间中，请确保该服务部署到要公开的应用程序所在的名称空间中。

          ```
          kubectl apply -f myapp_service.yaml [-n <namespace>]
          ```
          {: pre}
      5.  针对要公开的每一个应用程序，重复上述步骤。


### 步骤 2：映射定制域并选择 TLS 终止
{: #private_2}

配置专用 ALB 时，可以使用可用于访问应用程序的定制域，并选择是否使用 TLS 终止。
{: shortdesc}

ALB 会对流至应用程序的 HTTP 网络流量进行负载均衡。要同时对入局 HTTPS 连接进行负载均衡，可以配置 ALB，以便可以使用您自己的 TLS 证书来解密网络流量。然后，ALB 会将已解密的请求转发到集群中公开的应用程序。
1.   创建定制域。要注册定制域，请使用您的域名服务 (DNS) 提供者或 [{{site.data.keyword.Bluemix_notm}} DNS](/docs/infrastructure/dns/getting-started.html#getting-started-with-dns)。
      * 如果希望 Ingress 公开的应用程序位于一个集群的不同名称空间中，请将定制域注册为通配符域，例如 `*.custom_domain.net`。

2. 通过将 IBM 提供的专用 ALB 的可移植专用 IP 地址添加为记录，将定制域映射到该 IP 地址。要查找专用 ALB 的可移植专用 IP 地址，请运行 `bx cs albs --cluster <cluster_name>`.
3.   可选：如果要使用 TLS，请导入或创建 TLS 证书和私钥。如果要使用通配符域，请确保导入或创建通配符证书。
      * 如果要使用存储在 {{site.data.keyword.cloudcerts_long_notm}} 中的 TLS 证书，那么可以通过运行以下命令，将其关联的私钥导入到集群：
        ```
          bx cs alb-cert-deploy --secret-name <secret_name> --cluster <cluster_name_or_ID> --cert-crn <certificate_crn>
          ```
        {: pre}
      * 如果还没有 TLS 证书，请执行以下步骤：
        1. 为域创建以 PEM 格式编码的 TLS 证书和密钥。
        2. 定义使用您的 TLS 证书和密钥的私钥。将 <em>&lt;tls_secret_name&gt;</em> 替换为您的 Kubernetes 私钥的名称，将 <em>&lt;tls_key_filepath&gt;</em> 替换为定制 TLS 密钥文件的路径，并将 <em>&lt;tls_cert_filepath&gt;</em> 替换为定制 TLS 证书文件的路径。
          ```
            kubectl create secret tls <tls_secret_name> --key <tls_key_filepath> --cert <tls_cert_filepath>
            ```
          {: pre}


### 步骤 3：创建 Ingress 资源
{: #pivate_3}

Ingress 资源定义 ALB 用于将流量路由到应用程序服务的路由规则。
{: shortdesc}

**注：**如果集群的多个名称空间中公开了应用程序，那么每个名称空间至少需要一个 Ingress 资源。但是，每个名称空间都必须使用不同的主机。必须注册通配符域并在每个资源中指定一个不同的子域。有关更多信息，请参阅[规划单个或多个名称空间的联网](#multiple_namespaces)。

1. 打开首选编辑器，并创建 Ingress 配置文件，例如名为 `myingressresource.yaml`。

2.  在配置文件中定义 Ingress 资源，该资源使用定制域将入局网络流量路由到先前创建的服务。

    不使用 TLS 的示例 YAML：
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingressresource
      annotations:
        ingress.bluemix.net/ALB-ID: "<private_ALB_ID>"
    spec:
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    使用 TLS 的示例 YAML：
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingressresource
      annotations:
        ingress.bluemix.net/ALB-ID: "<private_ALB_ID>"
    spec:
      tls:
      - hosts:
        - <domain>
        secretName: <tls_secret_name>
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td><code>ingress.bluemix.net/ALB-ID</code></td>
    <td>将 <em>&lt;private_ALB_ID&gt;</em> 替换为专用 ALB 的标识。运行 <code>bx cs albs --cluster <my_cluster></code> 以查找 ALB 标识。有关此 Ingress 注释的更多信息，请参阅[专用应用程序负载均衡器路由](cs_annotations.html#alb-id)。</td>
    </tr>
    <tr>
    <td><code>tls/hosts</code></td>
    <td>要使用 TLS，请将 <em>&lt;domain&gt;</em> 替换为定制域。

    </br></br>
    <strong>注：</strong><ul><li>如果应用程序由服务在一个集群的不同名称空间中公开，请将通配符子域附加到该域的开头，例如 `subdomain1.custom_domain.net`。请对在集群中创建的每个资源使用唯一子域。</li><li>不要使用 &ast; 表示主机，也不要将主机属性保留为空，以避免创建 Ingress 期间发生失败。</li></ul></td>
    </tr>
    <tr>
    <td><code>tls/secretName</code></td>
    <td>将 <em>&lt;tls_secret_name&gt;</em> 替换为先前创建的私钥的名称，此私钥用于保存定制 TLS 证书和密钥。如果已从 {{site.data.keyword.cloudcerts_short}} 导入证书，那么可以运行 <code>bx cs alb-cert-get --cluster <cluster_name_or_ID> --cert-crn <certificate_crn></code> 来查看与 TLS 证书关联的私钥。
        </tr>
    <tr>
    <td><code>host</code></td>
    <td>将 <em>&lt;domain&gt;</em> 替换为定制域。
    </br></br>
    <strong>注：</strong><ul><li>如果应用程序由服务在一个集群的不同名称空间中公开，请将通配符子域附加到该域的开头，例如 `subdomain1.custom_domain.net`。请对在集群中创建的每个资源使用唯一子域。</li><li>不要使用 &ast; 表示主机，也不要将主机属性保留为空，以避免创建 Ingress 期间发生失败。</li></ul></td>
    </td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td>将 <em>&lt;app_path&gt;</em> 替换为斜杠或应用程序正在侦听的路径。该路径将附加到定制域，以创建应用程序的唯一路径。在 Web 浏览器中输入此路径时，网络流量会路由到 ALB。ALB 会查找关联的服务，并将网络流量发送到该服务。然后，该服务将流量转发到在运行应用程序的 pod。</br></br>
        许多应用程序不会侦听特定路径，而是使用根路径和特定端口。在这种情况下，请将根路径定义为 <code>/</code>，并且不要为应用程序指定单独的路径。
                示例：<ul><li>对于 <code>http://domain/</code>，请输入 <code>/</code> 作为路径。</li><li>对于 <code>http://domain/app1_path</code>，请输入 <code>/app1_path</code> 作为路径。</li></ul>
    </br>
    <strong>提示：</strong>要将 Ingress 配置为侦听与应用程序所侦听的路径不同的路径，可以使用 [rewrite 注释](cs_annotations.html#rewrite-path)。</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td>将 <em>&lt;app1_service&gt;</em> 和 <em>&lt;app2_service&gt;</em> 等替换为已创建用于公开应用程序的服务的名称。如果应用程序由服务在一个集群的不同名称空间中公开，请仅包含位于同一名称空间中的应用程序服务。必须为要在其中公开应用程序的每个名称空间创建一个 Ingress 资源。</td>
    </tr>
    <tr>
    <td><code>servicePort</code></td>
    <td>服务侦听的端口。使用针对应用程序创建 Kubernetes 服务时定义的端口。</td>
    </tr>
    </tbody></table>

3.  为集群创建 Ingress 资源。确保资源部署到在资源中指定的应用程序服务所在的名称空间中。

    ```
    kubectl apply -f myingressresource.yaml -n <namespace>
    ```
    {: pre}
4.   验证 Ingress 资源是否已成功创建。

      ```
            kubectl describe ingress myingressresource
      ```
      {: pre}

      1. 如果 events 中的消息描述了资源配置中的错误，请更改资源文件中的值，然后将该文件重新应用于资源。


Ingress 资源已在应用程序服务所在的名称空间中创建。此名称空间中的应用程序已向集群的 Ingress ALB 注册。

### 步骤 4：通过专用网络访问应用程序
{: #private_4}

在专用网络防火墙内，在 Web 浏览器中输入应用程序服务的 URL。

```
https://<domain>/<app1_path>
```
{: pre}

如果公开了多个应用程序，请通过更改附加到该 URL 的路径来访问这些应用程序。

```
https://<domain>/<app2_path>
```
{: pre}

如果使用通配符域在不同名称空间中公开应用程序，请使用其各自的子域来访问这些应用程序。

```
http://<subdomain1>.<domain>/<app1_path>
```
{: pre}

```
http://<subdomain2>.<domain>/<app1_path>
```
{: pre}


有关如何使用专用 ALB（带 TLS）来保护跨集群的微服务对微服务通信的全面教程，请查看[此博客帖子 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://medium.com/ibm-cloud/secure-microservice-to-microservice-communication-across-kubernetes-clusters-using-a-private-ecbe2a8d4fe2)。

<br />


## 可选的应用程序负载均衡器配置
{: #configure_alb}

您可以使用以下选项进一步配置应用程序负载均衡器。

-   [在 Ingress 应用程序负载均衡器中打开端口](#opening_ingress_ports)
-   [在 HTTP 级别配置 SSL 协议和 SSL 密码](#ssl_protocols_ciphers)
-   [定制 Ingress 日志格式](#ingress_log_format)
-   [使用注释定制应用程序负载均衡器](cs_annotations.html)
{: #ingress_annotation}


### 在 Ingress 应用程序负载均衡器中打开端口
{: #opening_ingress_ports}

缺省情况下，Ingress ALB 中仅公开端口 80 和 443。要公开其他端口，可以编辑 `ibm-cloud-provider-ingress-cm` 配置映射资源。
{:shortdesc}

1. 为 `ibm-cloud-provider-ingress-cm` 配置映射资源创建并打开配置文件的本地版本。

    ```
        kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. 添加 <code>data</code> 部分，并指定公共端口 `80`、`443` 以及要公开的其他任何端口，端口之间以分号 (;) 分隔。

    **重要信息**：缺省情况下，端口 80 和 443 已打开。如果要使 80 和 443 保持打开，那么必须在 `public-ports` 字段中包含这两个端口以及您指定的其他任何端口。未指定的任何端口都处于关闭状态。如果启用了专用 ALB，那么还必须在 `private-ports` 字段中指定要保持打开的任何端口。

    ```
    apiVersion: v1
    data:
      public-ports: "80;443;<port3>"
      private-ports: "80;443;<port4>"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

    使端口 `80`、`443` 和 `9443` 保持打开的示例：
    ```
     apiVersion: v1
 data:
   public-ports: "80;443;9443"
 kind: ConfigMap
 metadata:
   name: ibm-cloud-provider-ingress-cm
   namespace: kube-system
 ```
    {: screen}

3. 保存配置文件。

4. 验证是否已应用配置映射更改。

 ```
 kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
 ```
 {: pre}

 输出：
 ```
 Name:        ibm-cloud-provider-ingress-cm
 Namespace:    kube-system
 Labels:        <none>
 Annotations:    <none>

 Data
 ====

  public-ports: "80;443;9443"
 ```
 {: screen}

有关配置映射资源的更多信息，请参阅 [Kubernetes 文档](https://kubernetes-v1-4.github.io/docs/user-guide/configmap/)。

### 在 HTTP 级别配置 SSL 协议和 SSL 密码
{: #ssl_protocols_ciphers}

通过编辑 `ibm-cloud-provider-ingress-cm` 配置映射，在全局 HTTP 级别启用 SSL 协议和密码。
{:shortdesc}

缺省情况下，TLS 1.2 协议用于使用 IBM 提供的域的所有 Ingress 配置。通过执行以下步骤，可以覆盖缺省值以改为使用 TLS 1.1 或 1.0 协议。

**注**：如果指定启用的协议用于所有主机，那么仅当使用 OpenSSL 1.0.1 或更高版本时，TLSv1.1 和 TLSv1.2 参数（1.1.13 和 1.0.12）才有效。仅当使用通过 TLSv1.3 支持构建的 OpenSSL 1.1.1 时，TLSv1.3 参数 (1.13.0) 才有效。

要编辑配置映射以启用 SSL 协议和密码，请执行以下操作：

1. 为 `ibm-cloud-provider-ingress-cm` 配置映射资源创建并打开配置文件的本地版本。

    ```
        kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. 添加 SSL 协议和密码。根据 [OpenSSL 库密码列表格式 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.openssl.org/docs/man1.0.2/apps/ciphers.html) 设置密码格式。

   ```
 apiVersion: v1
 data:
   ssl-protocols: "TLSv1 TLSv1.1 TLSv1.2"
   ssl-ciphers: "HIGH:!aNULL:!MD5"
 kind: ConfigMap
 metadata:
   name: ibm-cloud-provider-ingress-cm
   namespace: kube-system
 ```
   {: codeblock}

3. 保存配置文件。

4. 验证是否已应用配置映射更改。

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}

   输出：
   ```
 Name:        ibm-cloud-provider-ingress-cm
 Namespace:    kube-system
 Labels:        <none>
 Annotations:    <none>

 Data
 ====

  ssl-protocols: "TLSv1 TLSv1.1 TLSv1.2"
  ssl-ciphers: "HIGH:!aNULL:!MD5"
 ```
   {: screen}

### 定制 Ingress 日志内容和格式
{: #ingress_log_format}

可以定制为 Ingress ALB 收集的日志的内容和格式。
{:shortdesc}

缺省情况下，Ingress 日志设置为 JSON 格式并显示公共日志字段。但是，您还可以创建定制日志格式。要选择转发哪些日志组成部分以及这些组成部分在日志输出中的排列方式，请执行以下操作：

1. 为 `ibm-cloud-provider-ingress-cm` 配置映射资源创建并打开配置文件的本地版本。

    ```
        kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. 添加 <code>data</code> 部分。添加 `log-format` 字段以及（可选）`log-format-escape-json` 字段。

    ```
        apiVersion: v1
    data:
      log-format: '{<key1>: <log_variable1>, <key2>: <log_variable2>, <key3>: <log_variable3>}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: pre}

    <table>
    <caption>YAML 文件的组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 log-format 配置</th>
    </thead>
    <tbody>
    <tr>
    <td><code>log-format</code></td>
    <td>将 <code>&lt;key&gt;</code> 替换为日志组成部分的名称，并将 <code>&lt;log_variable&gt;</code> 替换为希望在日志条目中收集的日志组成部分的变量。可以包含希望日志条目包含的文本和标点符号，如用于将字符串值括起的引号，以及用于分隔日志组成部分的逗号。例如，将组成部分的格式设置为诸如 <code>request: "$request",</code> 会在日志条目中生成以下内容：<code>request: "GET / HTTP/1.1",</code>。有关可以使用的所有变量的列表，请参阅 <a href="http://nginx.org/en/docs/varindex.html">Nginx 变量索引</a>。<br><br>要记录其他头（例如 <em>x-custom-ID</em>），请将以下键/值对添加到定制日志内容：<br><pre class="pre"><code>customID: $http_x_custom_id</code></pre> <br>连字符 (<code>-</code>) 会转换为下划线 (<code>_</code>)，并且必须在定制头名称的前面附加 <code>$http_</code>。</td>
    </tr>
    <tr>
    <td><code>log-format-escape-json</code></td>
    <td>可选：缺省情况下，将生成文本格式的日志。要生成 JSON 格式的日志，请添加 <code>log-format-escape-json</code> 字段并使用值 <code>true</code>。</td>
    </tr>
    </tbody></table>
    </dd>
    </dl>

    例如，日志格式可能包含以下变量：
    ```
        apiVersion: v1
    data:
      log-format: '{remote_address: $remote_addr, remote_user: "$remote_user",
                    time_date: [$time_local], request: "$request",
                    status: $status, http_referer: "$http_referer",
                    http_user_agent: "$http_user_agent",
                    request_id: $request_id}'
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: screen}

    符合此格式的日志条目类似于以下示例：
    ```
        remote_address: 127.0.0.1, remote_user: "dbmanager", time_date: [30/Mar/2018:18:52:17 +0000], request: "GET / HTTP/1.1", status: 401, http_referer: "-", http_user_agent: "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:47.0) Gecko/20100101 Firefox/47.0", request_id: a02b2dea9cf06344a25611c1d7ad72db
    ```
    {: screen}

    要创建基于 ALB 日志的缺省格式的定制日志格式，请根据需要修改以下部分并将其添加到 configmap：
    ```
        apiVersion: v1
    data:
      log-format: '{"time_date": "$time_iso8601", "client": "$remote_addr",
                    "host": "$http_host", "scheme": "$scheme",
                    "request_method": "$request_method", "request_uri": "$uri",
                    "request_id": "$request_id", "status": $status,
                    "upstream_addr": "$upstream_addr", "upstream_status":
                    $upstream_status, "request_time": $request_time,
                    "upstream_response_time": $upstream_response_time,
                    "upstream_connect_time": $upstream_connect_time,
                    "upstream_header_time": $upstream_header_time}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

4. 保存配置文件。

5. 验证是否已应用配置映射更改。

 ```
 kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
 ```
 {: pre}

 输出示例：
 ```
 Name:        ibm-cloud-provider-ingress-cm
 Namespace:    kube-system
 Labels:        <none>
 Annotations:    <none>

 Data
 ====

  log-format: '{remote_address: $remote_addr, remote_user: "$remote_user", time_date: [$time_local], request: "$request", status: $status, http_referer: "$http_referer", http_user_agent: "$http_user_agent", request_id: $request_id}'
  log-format-escape-json: "true"
 ```
 {: screen}

4. 要查看 Ingress ALB 日志，请在集群中[为 Ingress 服务创建日志记录配置](cs_health.html#logging)。

<br />



