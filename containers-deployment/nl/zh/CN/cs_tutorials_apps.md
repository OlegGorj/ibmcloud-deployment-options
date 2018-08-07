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


# 教程：将应用程序部署到集群
{: #cs_apps_tutorial}

您可以了解如何使用 {{site.data.keyword.containerlong}} 来部署利用 {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} 的容器化应用程序。
{: shortdesc}

在此场景中，一家虚构的公关公司使用 {{site.data.keyword.Bluemix_notm}} 服务来分析其新闻稿，并在消息中收到了关于语气的反馈。


公关公司的应用程序开发者使用最后一个教程中创建的 Kubernetes 集群，部署 Hello World 版本的应用程序。通过在此教程中的每一课上进行构建，应用程序开发者可通过渐进方式部署同一应用程序的更复杂版本。下图按课程显示每个部署的组件。


![课程组成部分](images/cs_app_tutorial_roadmap.png)

如图中所示，Kubernetes 使用多种不同类型的资源使应用程序在集群中启动并开始运行。在 Kubernetes 中，部署与服务一起工作。部署包含应用程序的定义。例如，要用于容器的映像以及必须为应用程序公开的端口。创建部署时，会为部署中定义的每个容器创建一个 Kubernetes pod。要使应用程序更具弹性，可以在部署中定义同一应用程序的多个实例，并允许 Kubernetes 自动为您创建副本集。副本集用于监视 pod，并确保始终有指定数量的 pod 启动并在运行。如果其中一个 pod 无响应，那么会自动重新创建该 pod。

服务会将一些 pod 分组在一起，并提供与这些 pod 的网络连接，以供集群中的其他服务使用，而无需公开每个 pod 的实际专用 IP 地址。可以使用 Kubernetes 服务来使应用程序可供集群内的其他 pod 使用，也可以将应用程序公开到因特网。在本教程中，您将通过一个自动分配给工作程序节点的公共 IP 地址和一个公共端口，使用 Kubernetes 服务从因特网访问正在运行的应用程序。

要使应用程序具有更高可用性，可以在标准集群中创建多个工作程序节点以运行应用程序的更多副本。本教程中未涉及此任务，但请记住这一概念，以便将来改进应用程序可用性时加以运用。

虽然只有其中一课涉及将 {{site.data.keyword.Bluemix_notm}} 服务集成到应用程序中，但您可以将这些服务用于任何复杂程度的应用程序。

## 目标

* 了解基本 Kubernetes 术语
* 将映像推送到 {{site.data.keyword.registryshort_notm}} 中的注册表名称空间
* 使应用程序可公共访问
* 通过使用 Kubernetes 命令和脚本，在集群中部署应用程序的单个实例
* 在运行状况检查期间重新创建的容器中，部署应用程序的多个实例
* 部署使用 {{site.data.keyword.Bluemix_notm}} 服务中功能的应用程序

## 所需时间

40 分钟

## 受众

首次将应用程序部署到 Kubernetes 集群的软件开发者和网络管理员。

## 先决条件

* [教程：在 {{site.data.keyword.containershort_notm}} 中创建 Kubernetes 集群](cs_tutorials.html#cs_cluster_tutorial)。


## 第 1 课：将单实例应用程序部署到 Kubernetes 集群
{: #cs_apps_tutorial_lesson1}

在上一个教程中，您已创建含一个工作程序节点的集群。在本课中，您将配置部署并将应用程序的单个实例部署到工作程序节点内的 Kubernetes pod 中。
{:shortdesc}

下图显示通过完成本课部署的各组件。



![部署设置](images/cs_app_tutorial_components1.png)

要部署应用程序，请执行以下操作：

1.  将 [Hello world 应用程序 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/IBM/container-service-getting-started-wt) 的源代码克隆到用户主目录。存储库在以 `Lab` 开头的各个文件夹中分别包含类似应用程序的不同版本。每个版本包含以下文件：

    * `Dockerfile`：映像的构建定义。
    * `app.js`：Hello World 应用程序。
    * `package.json`：有关应用程序的元数据。

    ```
        git clone https://github.com/IBM/container-service-getting-started-wt.git
    ```
    {: pre}

2.  浏览到 `Lab 1` 目录。

    ```
        cd 'container-service-getting-started-wt/Lab 1'
    ```
    {: pre}

3.  登录到 {{site.data.keyword.Bluemix_notm}} CLI。根据提示，输入您的 {{site.data.keyword.Bluemix_notm}} 凭证。要指定 {{site.data.keyword.Bluemix_notm}} 区域，请使用 `bx cs region-set` 命令。

    ```
        bx login [--sso]
    ```
    {: pre}

    **注**：如果 login 命令失败，说明您可能拥有的是联合标识。请尝试向命令附加 `--sso` 标志。使用 CLI 输出中提供的 URL 来检索一次性密码。

4.  在 CLI 中设置集群的上下文。
    1.  获取命令以设置环境变量并下载 Kubernetes 配置文件。

        ```
                bx cs cluster-config <cluster_name_or_ID>
        ```
        {: pre}

        配置文件下载完成后，会显示一个命令，您可以使用该命令将本地 Kubernetes 配置文件的路径设置为环境变量。
    2.  复制并粘贴输出，以设置 `KUBECONFIG` 环境变量。

        OS X 的示例：

        ```
                export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<pr_firm_cluster>/kube-config-prod-dal10-pr_firm_cluster.yml
        ```
        {: screen}

5.  登录到 {{site.data.keyword.registryshort_notm}} CLI。**注**：请确保已[安装](/docs/services/Registry/index.html#registry_cli_install) container-registry 插件。

    ```
        bx cr login
    ```
    {: pre}
    -   如果忘记了 {{site.data.keyword.registryshort_notm}} 中的名称空间，请运行以下命令。

        ```
                bx cr namespace-list
        ```
        {: pre}

6.  启动 Docker。
    * 如果使用的是 Docker Community Edition，那么无需任何操作。
    * 如果使用的是 Linux，请访问 [Docker 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.docker.com/engine/admin/)，以查找有关如何启动 Docker 的指示信息，具体取决于使用的 Linux 分发版。
    * 如果在 Windows 或 OSX 上使用的是 Docker Toolbox，那么可以使用 Docker Quickstart Terminal，该程序将为您启动 Docker。将 Docker Quickstart Terminal 用于后面的几个步骤以运行 Docker 命令，然后切换回在其中设置 `KUBECONFIG` 会话变量的 CLI。

7.  构建包含 `Lab 1` 目录中应用程序文件的 Docker 映像。如果未来需要对应用程序进行更改，请重复这些步骤以创建映像的另一个版本。

    使用容器映像时，请了解有关[确保个人信息安全](cs_secure.html#pi)的更多信息。

    1.  在本地构建映像。指定要使用的名称和标记。确保使用上一个教程中在 {{site.data.keyword.registryshort_notm}} 中创建的名称空间。使用名称空间信息来标记映像，可让 Docker 知道在后续步骤中应将映像推送到何处。在映像名称中仅使用小写字母数字字符或下划线 (`_`)。不要忘记在命令末尾输入句点 (`.`)。句点将通知 Docker 在当前目录内查找用于构建映像的 Dockerfile 和构建工件。

        ```
                docker build -t registry.<region>.bluemix.net/<namespace>/hello-world:1 .
        ```
        {: pre}

        构建完成后，请验证是否看到以下成功消息：
        ```
        Successfully built <image_id>
        Successfully tagged <image_tag>
        ```
        {: screen}

    2.  将映像推送到注册表名称空间。

        ```
        docker push registry.<region>.bluemix.net/<namespace>/hello-world:1
        ```
        {: pre}

        输出示例：

        ```
        The push refers to a repository [registry.ng.bluemix.net/pr_firm/hello-world]
        ea2ded433ac8: Pushed
        894eb973f4d3: Pushed
        788906ca2c7e: Pushed
        381c97ba7dc3: Pushed
        604c78617f34: Pushed
        fa18e5ffd316: Pushed
        0a5e2b2ddeaa: Pushed
        53c779688d06: Pushed
        60a0858edcd5: Pushed
        b6ca02dfe5e6: Pushed
        1: digest: sha256:0d90cb73288113bde441ae9b8901204c212c8980d6283fbc2ae5d7cf652405
        43 size: 2398
        ```
        {: screen}

8.  部署用于管理 pod；pod 包含应用程序的容器化实例。以下命令会将应用程序部署在单个 pod 中。对于本教程，部署名为 **hello-world-deployment**，但您可以根据需要为其指定任何名称。如果使用了 Docker Quickstart 终端来运行 Docker 命令，请确保切换回用于设置 `KUBECONFIG` 会话变量的 CLI。

    ```
    kubectl run hello-world-deployment --image=registry.<region>.bluemix.net/<namespace>/hello-world:1
    ```
    {: pre}

    输出示例：

    ```
    deployment "hello-world-deployment" created
    ```
    {: screen}

    使用 Kubernetes 资源时，请了解有关[确保个人信息安全](cs_secure.html#pi)的更多信息。

9.  通过将部署公开为 NodePort 服务，使应用程序可供公共访问。正如您可能会公开 Cloud Foundry 应用程序的端口，您公开的 NodePort 就是工作程序节点用于侦听流量的端口。

    ```
    kubectl expose deployment/hello-world-deployment --type=NodePort --port=8080 --name=hello-world-service --target-port=8080
    ```
    {: pre}

    输出示例：

    ```
    service "hello-world-service" exposed
    ```
    {: screen}

    <table summary=“Information about the expose command parameters.”>
    <caption>关于 expose 参数的更多信息</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 关于 expose 参数的更多信息</th>
    </thead>
    <tbody>
    <tr>
    <td><code>expose</code></td>
    <td>将资源作为 Kubernetes 服务公开，并使其可供用户公共使用。</td>
    </tr>
    <tr>
    <td><code>deployment/<em>&lt;hello-world-deployment&gt;</em></code></td>
    <td>要使用此服务公开的资源的资源类型和名称。</td>
    </tr>
    <tr>
    <td><code>--name=<em>&lt;hello-world-service&gt;</em></code></td>
    <td>服务的名称。</td>
    </tr>
    <tr>
    <td><code>--port=<em>&lt;8080&gt;</em></code></td>
    <td>服务发挥作用的端口。</td>
    </tr>
    <tr>
    <td><code>--type=NodePort</code></td>
    <td>要创建的服务类型。</td>
    </tr>
    <tr>
    <td><code>--target-port=<em>&lt;8080&gt;</em></code></td>
    <td>服务将流量定向到的目标端口。在本例中，target-port 与 port 相同，但您创建的其他应用程序可能不同。</td>
    </tr>
    </tbody></table>

10. 现在所有的部署工作均已完成，您可以在浏览器中测试应用程序。获取详细信息以构成 URL。
    1.  获取有关服务的信息以查看分配的 NodePort。

        ```
        kubectl describe service hello-world-service
        ```
        {: pre}

        输出示例：

        ```
        Name:                   hello-world-service
        Namespace:              default
        Labels:                 run=hello-world-deployment
        Selector:               run=hello-world-deployment
        Type:                   NodePort
        IP:                     10.xxx.xx.xxx
        Port:                   <unset> 8080/TCP
        NodePort:               <unset> 30872/TCP
        Endpoints:              172.30.xxx.xxx:8080
        Session Affinity:       None
        No events.
        ```
        {: screen}

        使用 `expose` 命令生成 NodePort 时，会随机分配这些 NodePort，但范围为 30000-32767。在此示例中，NodePort 为 30872。

    2.  获取集群中工作程序节点的公共 IP 地址。

        ```
        bx cs workers <cluster_name_or_ID>
        ```
        {: pre}

        输出示例：

        ```
        bx cs workers pr_firm_cluster
        Listing cluster workers...
        OK
        ID                                                 Public IP       Private IP       Machine Type   State    Status   Location   Version
        kube-mil01-pa10c8f571c84d4ac3b52acbf50fd11788-w1   169.xx.xxx.xxx  10.xxx.xx.xxx    free           normal   Ready    mil01      1.9.7
        ```
        {: screen}

11. 打开浏览器并通过以下 URL 检查应用程序：`http://<IP_address>:<NodePort>`. 使用示例值时，URL 为 `http://169.xx.xxx.xxx:30872`。在浏览器中输入该 URL 时，可以看到以下文本。


    ```
    Hello world! Your app is up and running in a cluster!
    ```
    {: screen}

    要查看该应用程序是否公开可用，请尝试在手机上的浏览器中输入该应用程序。
    {: tip}

12. [启动 Kubernetes 仪表板](cs_app.html#cli_dashboard)。

    如果在 [{{site.data.keyword.Bluemix_notm}}GUI](https://console.bluemix.net/) 中选择了集群，那么可以使用 **Kubernetes 仪表板**按钮来通过一次单击启动仪表板。
    {: tip}

13. 在**工作负载**选项卡中，可以查看已创建的资源。

祝贺您！您已部署了应用程序的第一个版本。

本课中的命令太多？没错。那么使用配置脚本为您执行其中一些工作怎么样？要为应用程序的第二个版本使用配置脚本，并要通过部署该应用程序的多个实例来创建更高可用性，请继续学习下一课。

<br />


## 第 2 课：部署和更新更高可用性的应用程序
{: #cs_apps_tutorial_lesson2}

在本课中，您要将 Hello World 应用程序的三个实例部署到集群中，以实现比应用程序的第一个版本更高的可用性。
{:shortdesc}

更高可用性意味着用户访问会在这三个实例之间分布。如果有过多用户尝试访问同一应用程序实例，他们可能会发现响应缓慢。而多个实例可提高对用户的响应速度。在本课中，您还将学习运行状况检查和部署更新可以如何用于 Kubernetes。下图包含通过完成本课进行部署的组件。


![部署设置](images/cs_app_tutorial_components2.png)

在上一个教程中，您已创建帐户以及含一个工作程序节点的集群。在本课中，您将配置部署并部署 Hello World 应用程序的三个实例。每个实例都会部署在一个 Kubernetes pod 中，作为工作程序节点中副本集的一部分。要使实例公开可用，也请创建 Kubernetes 服务。

如配置脚本中所定义，Kubernetes 可以使用可用性检查来查看 pod 中的容器是否在运行。例如，这些检查可能发现死锁情况，即应用程序在运行，但无法取得进展。重新启动处于这种状况的容器，有助于使应用程序在有错误的情况下仍能有更高可用性。然后，Kubernetes 会使用就绪性检查来确定容器何时已准备好开始重新接受流量。在 pod 的容器准备就绪时，该 pod 即视为准备就绪。pod 准备就绪后，即会再次启动。在此版本的应用程序中，每 15 秒超时一次。通过在配置脚本中配置的运行状况检查，如果运行状况检查发现应用程序有问题，会重新创建容器。

1.  在 CLI 中，浏览到 `Lab 2` 目录。

  ```
  cd 'container-service-getting-started-wt/Lab 2'
  ```
  {: pre}

2.  如果启动了新的 CLI 会话，请登录并设置集群上下文。

3.  将应用程序的第二个版本作为映像在本地进行构建和标记。同样，不要忘记在命令末尾输入句点 (`.`)。

  ```
  docker build -t registry.<region>.bluemix.net/<namespace>/hello-world:2 .
  ```
  {: pre}

  验证是否看到成功消息。


  ```
  Successfully built <image_id>
  ```
  {: screen}

4.  将映像的第二个版本推送到注册表名称空间中。等待映像推送完后，再继续执行下一步。

  ```
  docker push registry.<region>.bluemix.net/<namespace>/hello-world:2
  ```
  {: pre}

  输出示例：

  ```
  The push refers to a repository [registry.ng.bluemix.net/pr_firm/hello-world]
  ea2ded433ac8: Pushed
  894eb973f4d3: Pushed
  788906ca2c7e: Pushed
  381c97ba7dc3: Pushed
  604c78617f34: Pushed
  fa18e5ffd316: Pushed
  0a5e2b2ddeaa: Pushed
  53c779688d06: Pushed
  60a0858edcd5: Pushed
  b6ca02dfe5e6: Pushed
  1: digest: sha256:0d90cb73288113bde441ae9b8901204c212c8980d6283fbc2ae5d7cf652405
  43 size: 2398
  ```
  {: screen}

5.  使用文本编辑器打开 `Lab 2` 目录中的 `healthcheck.yml` 文件。此配置脚本包含上一课中的若干步骤，用于同时创建部署和服务。公关公司的应用程序开发者在进行更新或要通过重新创建 pod 对问题进行故障诊断时，可以使用这些脚本。
    1. 在专用注册表名称空间中更新映像的详细信息。

        ```
        image: "registry.<region>.bluemix.net/<namespace>/hello-world:2"
        ```
        {: codeblock}

    2.  在**部署**部分中，记下 `replicas`。Replicas 是应用程序的实例数。应用程序的可用性在运行三个实例时高于仅运行一个实例时。

        ```
        replicas: 3
        ```
        {: codeblock}

    3.  记下 HTTP 活性探测器，此探测器每 5 秒检查一次容器的运行状况。

        ```
        livenessProbe:
                    httpGet:
                      path: /healthz
                      port: 8080
                    initialDelaySeconds: 5
                    periodSeconds: 5
        ```
        {: codeblock}

    4.  在**服务**部分中，记下 `NodePort`。与上一课中生成随机 NodePort 不同，您可以指定 30000-32767 范围内的端口。此示例使用 30072。

6.  切换回用于设置集群上下文和运行配置脚本的 CLI。创建了部署和服务后，应用程序可供公关公司用户查看。

  ```
  kubectl apply -f healthcheck.yml
  ```
  {: pre}

  输出示例：

  ```
  deployment "hw-demo-deployment" created
  service "hw-demo-service" created
  ```
  {: screen}

7.  现在，部署工作已完成，您可以打开浏览器并检查应用程序。要构成 URL，请采用上一课中用于工作程序节点的公共 IP 地址，并将其与配置脚本中指定的 NodePort 组合在一起。要获取工作程序节点的公共 IP 地址，请执行以下操作：

  ```
  bx cs workers <cluster_name_or_ID>
  ```
  {: pre}

  使用示例值时，URL 为 `http://169.xx.xxx.xxx:30072`。在浏览器中，可能会看到以下文本。如果未看到此文本，也不必担心。此应用程序本来就是要让文本时而显示时而隐藏的。


  ```
  Hello world! Great job getting the second stage up and running!
  ```
  {: screen}

  您还可以检查 `http://169.xx.xxx.xxx:30072/healthz` 以了解状态。

  在前 10-15 秒内返回了 200 消息，这表明应用程序在成功运行。超过这 15 秒后，将显示超时消息。这是预期的行为。

  ```
  {
    "error": "Timeout, Health check error!"
  }
  ```
  {: screen}

8.  [启动 Kubernetes 仪表板](cs_app.html#cli_dashboard)。

9. 在**工作负载**选项卡中，可以查看已创建的资源。在此选项卡中，可以持续刷新并查看运行状况检查是否在运行。在 **Pod** 部分中，可以查看在重新创建 pod 中的容器时，pod 重新启动的次数。如果在仪表板中偶然遇到以下错误，此消息指示运行状况检查遇到问题。请等待几分钟，然后重新刷新。您会看到每个 pod 的重新启动次数发生变化。


    ```
    Liveness probe failed: HTTP probe failed with statuscode: 500
    Back-off restarting failed docker container
    Error syncing pod, skipping: failed to "StartContainer" for "hw-container" with CrashLoopBackOff: "Back-off 1m20s restarting failed container=hw-container pod=hw-demo-deployment-3090568676-3s8v1_default(458320e7-059b-11e7-8941-56171be20503)"
    ```
    {: screen}

祝贺您！您已部署了应用程序的第二个版本。您在此过程中必须使用更少的命令，学习了运行状况检查如何运行，并编辑了部署，非常不错！Hello World 应用程序已通过公关公司的测试。现在，您可以为公关公司部署更有用的应用程序，以开始分析新闻稿。

继续之前，准备好删除已创建的内容了吗？现在，您可以使用相同的配置脚本来删除已创建的两个资源。

  ```
  kubectl delete -f healthcheck.yml
  ```
  {: pre}

  输出示例：

  ```
  deployment "hw-demo-deployment" deleted
  service "hw-demo-service" deleted
  ```
  {: screen}

<br />


## 第 3 课：部署和更新 Watson Tone Analyzer 应用程序
{: #cs_apps_tutorial_lesson3}

在前几课中，应用程序部署为一个工作程序节点中的单独组件。在本课中，您可以将使用 {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} 服务的两个应用程序组件部署到集群中。
{:shortdesc}

将组件分隔到不同的容器中可确保更新一个组件时不会影响其他组件。然后，您可更新应用程序以使用更多副本将其向上扩展，使其可用性更高。下图包含通过完成本课进行部署的组件。

![部署设置](images/cs_app_tutorial_components3.png)


在上一个教程中，您已具有帐户以及含一个工作程序节点的集群。在本课中，您将在 {{site.data.keyword.Bluemix_notm}} 帐户中创建 {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} 服务的实例，并配置两个部署，其中应用程序的每个组件对应一个部署。每个组件都会部署在工作程序节点的一个 Kubernetes pod 中。要使这两个组件公开可用，也请为每个组件创建一个 Kubernetes 服务。


### 第 3a 课：部署 {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} 应用程序
{: #lesson3a}

1.  在 CLI 中，浏览到 `Lab 3` 目录。

  ```
  cd 'container-service-getting-started-wt/Lab 3'
  ```
  {: pre}

2.  如果启动了新的 CLI 会话，请登录并设置集群上下文。

3.  构建第一个 {{site.data.keyword.watson}} 映像。

    1.  浏览到 `watson` 目录。

        ```
        cd watson
        ```
        {: pre}

    2.  将应用程序的第一部分作为映像在本地进行构建和标记。同样，不要忘记在命令末尾输入句点 (`.`)。如果要使用 Docker Quickstart 终端来运行 Docker 命令，请确保切换 CLI。

        ```
        docker build -t registry.<region>.bluemix.net/<namespace>/watson .
        ```
        {: pre}

        验证是否看到成功消息。


        ```
        Successfully built <image_id>
        ```
        {: screen}

    3.  将应用程序的第一部分作为映像推送到专用注册表名称空间中。等待映像推送完后，再继续执行下一步。

        ```
        docker push registry.<region>.bluemix.net/<namespace>/watson
        ```
        {: pre}

4.  构建 {{site.data.keyword.watson}}-talk 映像。

    1.  浏览到 `watson-talk` 目录。

        ```
        cd 'container-service-getting-started-wt/Lab 3/watson-talk'
        ```
        {: pre}

    2.  将应用程序的第二部分作为映像在本地进行构建和标记。同样，不要忘记在命令末尾输入句点 (`.`)。

        ```
        docker build -t registry.<region>.bluemix.net/<namespace>/watson-talk .
        ```
        {: pre}

        验证是否看到成功消息。


        ```
        Successfully built <image_id>
        ```
        {: screen}

    3.  将应用程序的第二部分推送到专用注册表名称空间中。等待映像推送完后，再继续执行下一步。

        ```
        docker push registry.<region>.bluemix.net/<namespace>/watson-talk
        ```
        {: pre}

5.  验证映像是否已成功添加到注册表名称空间。如果使用了 Docker Quickstart 终端来运行 Docker 命令，请确保切换回用于设置 `KUBECONFIG` 会话变量的 CLI。

    ```
    bx cr images
    ```
    {: pre}

    输出示例：

    ```
    Listing images...

    REPOSITORY                                      NAMESPACE  TAG      DIGEST         CREATED         SIZE     VULNERABILITY STATUS
    registry.ng.bluemix.net/namespace/hello-world   namespace  1        0d90cb732881   40 minutes ago  264 MB   OK
    registry.ng.bluemix.net/namespace/hello-world   namespace  2        c3b506bdf33e   20 minutes ago  264 MB   OK
    registry.ng.bluemix.net/namespace/watson        namespace  latest   fedbe587e174   3 minutes ago   274 MB   OK
    registry.ng.bluemix.net/namespace/watson-talk   namespace  latest   fedbe587e174   2 minutes ago   274 MB   OK
    ```
    {: screen}

6.  使用文本编辑器打开 `Lab 3` 目录中的 `watson-deployment.yml` 文件。此配置脚本包含同时用于应用程序的 `watson` 和 `watson-talk` 组件的部署和服务。

    1.  在注册表名称空间中为两个部署更新该映像的详细信息。

        watson:

        ```
        image: "registry.<region>.bluemix.net/namespace/watson"
        ```
        {: codeblock}

        watson-talk:

        ```
        image: "registry.<region>.bluemix.net/namespace/watson-talk"
        ```
        {: codeblock}

    2.  在 watson 部署的 volumes 部分中，更新在先前教程中创建的 {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} 私钥的名称。通过将 Kubernetes 私钥作为卷安装到您的部署，可使 {{site.data.keyword.Bluemix_notm}} 服务凭证可用于在 pod 中运行的容器。
本教程中的 {{site.data.keyword.watson}} 应用程序组件配置为使用卷安装路径来查找服务凭证。


        ```
        volumes:
                - name: service-bind-volume
                  secret:
                    defaultMode: 420
                    secretName: binding-mytoneanalyzer
        ```
        {: codeblock}

        如果忘记了私钥的名称，请运行以下命令。

        ```
        kubectl get secrets --namespace=default
        ```
        {: pre}

    3.  在 watson-talk 服务部分中，记下为 `NodePort` 设置的值。此示例使用 30080。

7.  运行配置脚本。

  ```
  kubectl apply -f watson-deployment.yml
  ```
  {: pre}

8.  可选：验证 {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} 私钥是否已作为卷安装到 pod。

    1.  要获取 watson pod 的名称，请运行以下命令。

        ```
        kubectl get pods
        ```
        {: pre}

        输出示例：

        ```
        NAME                                 READY     STATUS    RESTARTS  AGE
        watson-pod-4255222204-rdl2f          1/1       Running   0         13h
        watson-talk-pod-956939399-zlx5t      1/1       Running   0         13h
        ```
        {: screen}

    2.  获取有关 pod 的详细信息，并查找私钥名称。

        ```
        kubectl describe pod <pod_name>
        ```
        {: pre}

        输出示例：

        ```
        Volumes:
          service-bind-volume:
            Type:       Secret (a volume populated by a Secret)
            SecretName: binding-mytoneanalyzer
          default-token-j9mgd:
            Type:       Secret (a volume populated by a Secret)
            SecretName: default-token-j9mgd
        ```
        {: codeblock}

9.  打开浏览器并分析一些文本。URL 的格式为 `http://<worker_node_IP_address>:<watson-talk-nodeport>/analyze/"<text_to_analyze>"`.

    示例：

    ```
    http://169.xx.xxx.xxx:30080/analyze/"Today is a beautiful day"
    ```
    {: screen}

    在浏览器中，可以看到对所输入文本的 JSON 响应。

10. [启动 Kubernetes 仪表板](cs_app.html#cli_dashboard)。

11. 在**工作负载**选项卡中，可以查看已创建的资源。

### 第 3b 课：更新正在运行的 Watson Tone Analyzer 部署
{: #lesson3b}

部署正在运行时，可以编辑部署以更改 pod 模板中的值。本课程包含如何更新使用的映像。公关公司希望在部署中更改应用程序。

更改映像的名称：

1.  打开正在运行的部署的配置详细信息。

    ```
    kubectl edit deployment/watson-talk-pod
    ```
    {: pre}

    根据操作系统，将打开 vi 编辑器或文本编辑器。

2.  将映像的名称更改为 ibmliberty 映像。

    ```
    spec:
          containers:
          - image: registry.<region>.bluemix.net/ibmliberty:latest
    ```
    {: codeblock}

3.  保存更改并退出编辑器。

4.  将更改应用于正在运行的部署。

    ```
    kubectl rollout status deployment/watson-talk-pod
    ```
    {: pre}

    等待有关应用完成的确认。


    ```
    deployment "watson-talk-pod" successfully rolled out
    ```
    {: screen}

    推广更改时，Kubernetes 会创建并测试另一个 pod。测试成功后，将除去原先的 pod。

[测试您的掌握情况并进行测验！![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://ibmcloud-quizzes.mybluemix.net/containers/apps_tutorial/quiz.php)

祝贺您！您已部署了 {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} 应用程序。公关公司可以开始使用这一部署来着手分析其新闻稿。

准备好删除已创建的内容了吗？您可以使用配置脚本删除已创建的资源。


  ```
  kubectl delete -f watson-deployment.yml
  ```
  {: pre}

  输出示例：

  ```
  deployment "watson-pod" deleted
  deployment "watson-talk-pod" deleted
  service "watson-service" deleted
  service "watson-talk-service" deleted
  ```
  {: screen}

  如果不希望保留集群，还可以删除该集群。

  ```
  bx cs cluster-rm <cluster_name_or_ID>
  ```
  {: pre}

## 接下来要做什么？
{: #next}

现在，您已掌握了基础知识，可以移至更高级的活动。请考虑尝试下列其中一项：

- 完成存储库中[更复杂的实验 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/IBM/container-service-getting-started-wt#lab-overview)
- 使用 {{site.data.keyword.containershort_notm}} [自动扩展应用程序](cs_app.html#app_scaling)
- 在 [developerWorks ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/code/journey/category/container-orchestration/) 上浏览容器编排过程
