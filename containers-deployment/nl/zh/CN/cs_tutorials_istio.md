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



# 教程：在 {{site.data.keyword.containerlong_notm}} 上安装 Istio
{: #istio_tutorial}

[Istio](https://www.ibm.com/cloud/info/istio) 是一个开放式平台，用于在云平台上（如 {{site.data.keyword.containerlong}} 中的 Kubernetes）连接、保护和管理微服务网络（也称为服务网）。通过 Istio，可管理网络流量，在微服务之间进行负载均衡，强制实施访问策略，验证服务身份，等等。
{:shortdesc}

在本教程中，您可以查看如何为简单的模拟书店应用程序“BookInfo”安装 Istio 以及四个微服务。微服务包括产品 Web 页面、书籍详细信息、评论和评级。将 BookInfo 的微服务部署到安装了 Istio 的 {{site.data.keyword.containershort}} 集群时，会在每个微服务的 pod 中注入 Istio Envoecar sidecar 代理。

**注**：Istio 平台的一些配置和功能仍在开发中，可能会根据用户反馈而更改。请等待几个月，稳定后再在生产中使用 Istio。 

## 目标

-   在集群中下载并安装 Istio
-   部署 BookInfo 样本应用程序
-   将 Envoy sidecar 代理注入到应用程序四个微型服务的 pod 中，以连接服务网中的微服务
-   通过评级服务的三个版本来验证 BookInfo 应用程序部署和循环

## 所需时间

30 分钟

## 受众

本教程适用于首次使用 Istio 的软件开发者和网络管理员。

## 先决条件

-  [安装 CLI](cs_cli_install.html#cs_cli_install_steps)。Istio 需要 Kubernetes V1.9 或更高版本。确保安装与集群的 Kubernetes 版本相匹配的 `kubectl` CLI 版本。
-  使用 Kubernetes V1.9 或更高版本来[创建集群](cs_clusters.html#clusters_cli)。
-  [设定 CLI 的目标为集群](cs_cli_install.html#cs_cli_configure)。

## 第 1 课：下载并安装 Istio
{: #istio_tutorial1}

在集群中下载并安装 Istio。
{:shortdesc}

1. 直接从 [https://github.com/istio/istio/releases ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/istio/istio/releases) 下载 Istio，或使用 curl 获取最新版本：

   ```
   curl -L https://git.io/getLatestIstio | sh -
   ```
   {: pre}

2. 解压缩安装文件。

3. 向 PATH 添加 `istioctl` 客户机。例如，在 MacOS 或 Linux 系统上运行以下命令：

   ```
   export PATH=$PWD/istio-0.4.0/bin:$PATH
   ```
   {: pre}

4. 将目录切换到 Istio 文件位置。

   ```
   cd filepath/istio-0.4.0
   ```
   {: pre}

5. 在 Kubernetes 集群上安装 Istio。Istio 部署在 Kubernetes 名称空间 `istio-system` 中。

   ```
   kubectl apply -f install/kubernetes/istio.yaml
   ```
   {: pre}

   **注**：如果需要在 sidecar 之间启用相互 TLS 认证，那么可以改为安装 `istio-auth` 文件：`kubectl apply -f install/kubernetes/istio-auth.yaml`

6. 在继续之前，请确保已完全部署 Kubernetes 服务 `istio-pilot`、`istio-mixer` 和 `istio-ingress`。

   ```
   kubectl get svc -n istio-system
   ```
   {: pre}

   ```
   NAME            TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)                                                            AGE
   istio-ingress   LoadBalancer   172.21.xxx.xxx   169.xx.xxx.xxx   80:31176/TCP,443:30288/TCP                                         2m
   istio-mixer     ClusterIP      172.21.xxx.xxx     <none>           9091/TCP,15004/TCP,9093/TCP,9094/TCP,9102/TCP,9125/UDP,42422/TCP   2m
   istio-pilot     ClusterIP      172.21.xxx.xxx    <none>           15003/TCP,443/TCP                                                  2m
   ```
   {: screen}

7. 在继续之前，确保相应的 pod `istio-pilo-*`、`istio-mixer-*`、`istio-ingress-*` 和 `istio-ca-*` 也已完全部署。

   ```
   kubectl get pods -n istio-system
   ```
   {: pre}

   ```
   istio-ca-3657790228-j21b9           1/1       Running   0          5m
   istio-ingress-1842462111-j3vcs      1/1       Running   0          5m
   istio-pilot-2275554717-93c43        1/1       Running   0          5m
   istio-mixer-2104784889-20rm8        2/2       Running   0          5m
   ```
   {: screen}


非常好！您已成功将 Istio 安装到集群中。接下来，将 BookInfo 样本应用程序部署到集群中。


## 第 2 课：部署 BookInfo 应用程序
{: #istio_tutorial2}

将 BookInfo 样本应用程序的微服务部署到 Kubernetes 集群。
{:shortdesc}

这四个微服务包括产品 Web 页面、书籍详细信息、评论（具有多个版本的评论微服务）和评级。您可以在 Istio 安装的 `samples/bookinfo` 目录中找到本示例中使用的所有文件。

部署 BookInfo 时，在部署微服务 pod 之前，Envoy sidecar 代理会作为容器注入到应用程序微服务的 pod 中。Istio 使用 Envoy 代理的扩展版本来调解服务网中所有微服务的所有入站和出站流量。有关 Envoy 的更多信息，请参阅 [Istio 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://istio.io/docs/concepts/what-is-istio/overview.html#envoy)。

1. 部署 BookInfo 应用程序。`kube-inject` 命令将 Envoy 添加到 `bookinfo.yaml` 文件，并使用此更新的文件来部署应用程序。当应用程序微服务部署时，每个微服务 pod 中也会部署 Envoy sidecar。

   ```
   kubectl apply -f <(istioctl kube-inject -f samples/bookinfo/kube/bookinfo.yaml)
   ```
   {: pre}

2. 确保已部署微服务及其相应的 pod：

   ```
    kubectl get svc
    ```
   {: pre}

   ```
   NAME                       CLUSTER-IP   EXTERNAL-IP   PORT(S)              AGE
   details                    10.xxx.xx.xxx    <none>        9080/TCP             6m
   kubernetes                 10.xxx.xx.xxx     <none>        443/TCP              30m
   productpage                10.xxx.xx.xxx   <none>        9080/TCP             6m
   ratings                    10.xxx.xx.xxx    <none>        9080/TCP             6m
   reviews                    10.xxx.xx.xxx   <none>        9080/TCP             6m
   ```
   {: screen}

   ```
            kubectl get pods
            ```
   {: pre}

   ```
   NAME                                        READY     STATUS    RESTARTS   AGE
   details-v1-1520924117-48z17                 2/2       Running   0          6m
   productpage-v1-560495357-jk1lz              2/2       Running   0          6m
   ratings-v1-734492171-rnr5l                  2/2       Running   0          6m
   reviews-v1-874083890-f0qf0                  2/2       Running   0          6m
   reviews-v2-1343845940-b34q5                 2/2       Running   0          6m
   reviews-v3-1813607990-8ch52                 2/2       Running   0          6m
   ```
   {: screen}

3. 要验证应用程序部署，请获取集群的公共地址。

    * 如果您正在使用标准集群，请运行以下命令以获取集群的 Ingress IP 和端口：

       ```
              kubectl get ingress
       ```
       {: pre}

       输出示例：

       ```
              NAME      HOSTS     ADDRESS          PORTS     AGE
       gateway   *         169.xx.xxx.xxx   80        3m
       ```
       {: screen}

       此示例所产生的 Ingress 地址为 `169.48.221.218:80`。使用以下命令将地址导出为网关 URL。您将在下一步中使用该网关 URL 来访问 BookInfo 产品页面。

       ```
              export GATEWAY_URL=169.xx.xxx.xxx:80
       ```
       {: pre}

    * 如果您使用的是免费集群，那么必须使用工作程序节点的公共 IP 和 NodePort。运行以下命令，以获取工作程序节点的公共 IP：

       ```
              bx cs workers <cluster_name_or_ID>
       ```
       {: pre}

       使用以下命令将工作程序节点的公共 IP 导出为网关 URL。您将在下一步中使用该网关 URL 来访问 BookInfo 产品页面。

       ```
              export GATEWAY_URL=<worker_node_public_IP>:$(kubectl get svc istio-ingress -n istio-system -o jsonpath='{.spec.ports[0].nodePort}')
       ```
       {: pre}

4. 对 `GATEWAY_URL` 变量进行 curl，以检查 BookInfo 是否正在运行。`200` 响应表示 BookInfo 与 Istio 一起正常运行。

   ```
   curl -I http://$GATEWAY_URL/productpage
   ```
   {: pre}

5. 在浏览器中，转至 `http://$GATEWAY_URL/productpage` 以查看 BookInfo Web 页面。

6. 尝试多次刷新该页面。不同版本的评论部分会以红色星星、黑色星星和无星星进行循环。

非常好！您已成功部署了使用 Istio Envoy sidecar 的 BookInfo 样本应用程序。接下来，您可以清除资源或继续使用更多教程来进一步探索 Istio。

## 清除
{: #istio_tutorial_cleanup}

如果您已完成使用 Istio，并且不希望[继续探索](#istio_tutorial_whatsnext)，那么可以清除集群中的 Istio 资源。
{:shortdesc}

1. 删除集群中的所有 BookInfo 服务、pod 和部署。

   ```
   samples/bookinfo/kube/cleanup.sh
   ```
   {: pre}

2. 卸载 Istio。

   ```
   kubectl delete -f install/kubernetes/istio.yaml
   ```
   {: pre}

## 接下来要做什么？
{: #istio_tutorial_whatsnext}

要进一步探索 Istio，您可以在 [Istio 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://istio.io/) 中找到更多指南。

* [智能路由 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://istio.io/docs/guides/intelligent-routing.html)：此示例显示如何使用 Istio 的各种流量管理功能，将流量路由到特定版本 BookInfo 的评论和评级微服务中。

* [深入遥测 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://istio.io/docs/guides/telemetry.html)：此示例包含如何使用 Istio Mixer 和 Envoy 代理跨 BookInfo 微服务获取统一度量值、日志和跟踪。
