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



# 教程：将应用程序从 Cloud Foundry 迁移到集群
{: #cf_tutorial}

您可以采用先前通过 Cloud Foundry 部署的应用程序，并将容器中的相同代码部署到 {{site.data.keyword.containershort_notm}} 中的 Kubernetes 集群。
{: shortdesc}


## 目标

- 了解将容器中的应用程序部署到 Kubernetes 集群的一般过程。
- 根据应用程序代码创建 Dockerfile 以构建容器映像。
- 将该映像中的容器部署到 Kubernetes 集群中。

## 所需时间
30 分钟

## 受众
本教程适用于 Cloud Foundry 应用程序开发者。

## 先决条件

- [在 {{site.data.keyword.registrylong_notm}} 中创建专用映像注册表](../services/Registry/index.html)。
- [创建集群](cs_clusters.html#clusters_ui)。
- [设定 CLI 的目标为集群](cs_cli_install.html#cs_cli_configure)。
- [了解有关 Docker 和 Kubernetes 术语的信息](cs_tech.html)。


<br />



## 第 1 课：下载应用程序代码

准备好您的代码。还没有任何代码？您可以下载要在本教程中使用的入门模板代码。
{: shortdesc}

1. 创建名为 `cf-py` 的目录并浏览到此目录。在此目录中，将保存构建 Docker 映像以及运行应用程序所需的所有文件。

  ```
  mkdir cf-py && cd cf-py
  ```
  {: pre}

2. 将应用程序代码以及所有相关文件复制到该目录中。可以使用自己的应用程序代码，也可以从目录下载样板。本教程使用 Python Fldask 样板。但是，您也可以将相同的基本步骤用于 Node.js、Java 或 [Kitura](https://github.com/IBM-Cloud/Kitura-Starter) 应用程序。

    要下载 Python Flask 应用程序代码，请执行以下操作：

    a. 在目录的**样板**中，单击 **Python Flask**。此样板包含适用于 Python 2 和 Python 3 应用程序的运行时环境。

    b. 输入应用程序名称 `cf-py-<name>`，然后单击**创建**。要访问样板的应用程序代码，必须首先将 CF 应用程序部署到云。可以对应用程序使用任何名称。如果使用示例中的名称，请将 `<name>` 替换为唯一标识，例如 `cf-py-msx`。
    
    **注意**：不要在任何应用程序、容器映像或 Kubernetes 资源名称中使用个人信息。

    部署应用程序后，将显示“使用命令行界面下载、修改和重新部署应用程序”的指示信息。

    c. 在 GUI 指示信息的步骤 1 中，单击**下载入门模板代码**。

    d. 解压缩 .zip 文件，并将其内容保存到 `cf-py` 目录。

应用程序代码可随时进行容器化！


<br />



## 第 2 课：使用应用程序代码创建 Docker 映像

创建 Dockerfile 以包含应用程序代码以及容器的必要配置。然后，通过该 Dockerfile 构建 Docker 映像，并将其推送到专用映像注册表。
{: shortdesc}

1. 在上一课中创建的 `cf-p.py` 目录中创建 `Dockerfile`，这是创建容器映像的基础。可以使用您计算机上的首选 CLI 编辑器或文本编辑器来创建 Dockerfile。以下示例显示了如何使用 nano 编辑器来创建 Dockerfile 文件。

  ```
  nano Dockerfile
  ```
  {: pre}

2. 将以下脚本复制到 Dockerfile 中。此 Dockerfile 专门适用于 Python 应用程序。如果使用的是其他类型的代码，Dockerfile 必须包含其他基本映像，并且可能需要定义其他字段。

  ```
  #Use the Python image from DockerHub as a base image
  FROM python:3-slim

  #Expose the port for your python app
  EXPOSE 5000

  #Copy all app files from the current directory into the app
  #directory in your container. Set the app directory
  #as the working directory
  WORKDIR /cf-py/
  COPY .  .

  #Install any requirements that are defined
  RUN pip install --no-cache-dir -r requirements.txt

  #Update the openssl package
  RUN apt-get update && apt-get install -y \
  openssl

  #Start the app.
  CMD ["python", "welcome.py"]
  ```
  {: codeblock}

3. 按 `ctrl + o` 在 nano 编辑器中保存更改。按 `enter` 键确认更改。按 `ctrl + x` 退出 nano 编辑器。

4. 构建包含应用程序代码的 Docker 映像，并将其推送到专用注册表。

  ```
  bx cr build -t registry.<region>.bluemix.net/namespace/cf-py .
  ```
  {: pre}

  <table>
  <caption>了解此命令的组成部分</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="此图标指示可了解有关此命令组成部分的更多信息。"/> 了解此命令的组成部分</th>
  </thead>
  <tbody>
  <tr>
  <td>参数</td>
  <td>描述</td>
  </tr>
  <tr>
  <td><code>build</code></td>
  <td>build 命令。</td>
  </tr>
  <tr>
  <td><code>-t registry.&lt;region&gt;.bluemix.net/namespace/cf-py</code></td>
  <td>专用注册表路径，其中包含唯一名称空间以及映像的名称。对于本示例，用于映像的名称与应用程序目录相同，但您可以为专用注册表中的映像选择任何名称。如果不确定哪个是您的名称空间，请运行 `bx cr namespaces` 命令进行查找。</td>
  </tr>
  <tr>
  <td><code>.</code></td>
  <td>Dockerfile 的位置。如果要从包含 Dockerfile 的目录运行 build 命令，请输入句点 (.). 否则，请使用 Dockerfile 的相对路径。</td>
  </tr>
  </tbody>
  </table>

  映像已在注册表中创建。您可以运行 `bx cr images` 命令来验证是否已创建映像。

  ```
  REPOSITORY                                     NAMESPACE   TAG      DIGEST         CREATED         SIZE     VULNERABILITY STATUS   
  registry.ng.bluemix.net/namespace/cf-py        namespace   latest   cb03170b2cb2   3 minutes ago   271 MB   OK
  ```
  {: screen}


<br />



## 第 3 课：通过映像部署容器

将应用程序部署为 Kubernetes 集群中的容器。
{: shortdesc}

1. 创建名为 `cf-py.yaml` 的配置 YAML 文件，并使用专用映像注册表的名称更新 `<registry_namespace>`。此配置文件根据您在上一课中创建的映像以及用于向公众公开应用程序的服务来定义容器部署。

  ```
  apiVersion: extensions/v1beta1
  kind: Deployment
  metadata:
    labels:
      app: cf-py
    name: cf-py
    namespace: default
  spec:
    selector:
      matchLabels:
        app: cf-py
    replicas: 1
    template:
      metadata:
        labels:
          app: cf-py
      spec:
        containers:
        - image: registry.ng.bluemix.net/<registry_namespace>/cf-py:latest
          name: cf-py
  ---
  apiVersion: v1
  kind: Service
  metadata:
    name: cf-py-nodeport
    labels:
      app: cf-py
  spec:
    selector:
      app: cf-py
    type: NodePort
    ports:
     - port: 5000
       nodePort: 30872
  ```
  {: codeblock}

  <table>
  <caption>了解 YAML 文件的组成部分</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
  </thead>
  <tbody>
  <tr>
  <td><code>image</code></td>
  <td>在 `registry.ng.bluemix.net/<registry_namespace>/cf-py:latest` 中，将 &lt;registry_namespace&gt; 替换为专用映像注册表的名称空间。如果不确定哪个是您的名称空间，请运行 `bx cr namespaces` 命令进行查找。</td>
  </tr>
  <tr>
  <td><code>nodePort</code></td>
  <td>通过创建类型为 NodePort 的 Kubernetes 服务来公开应用程序。NodePort 的范围是 30000 - 32767。日后，可使用此端口在浏览器中测试应用程序。</td>
  </tr>
  </tbody></table>

2. 应用配置文件，以在集群中创建部署和服务。

  ```
  kubectl apply -f <filepath>/cf-py.yaml
  ```
  {: pre}

  输出：

  ```
  deployment "cf-py" configured
  service "cf-py-nodeport" configured
  ```
  {: screen}

3. 现在所有的部署工作均已完成，您可以在浏览器中测试应用程序。获取详细信息以构成 URL。

    a.  获取集群中工作程序节点的公共 IP 地址。

    ```
        bx cs workers <cluster_name>
    ```
    {: pre}

    输出：

    ```
    ID                                                 Public IP        Private IP     Machine Type        State    Status   Zone    Version   
    kube-dal10-cr18e61e63c6e94b658596ca93d087eed9-w1   169.xx.xxx.xxx   10.xxx.xx.xxx   u2c.2x4.encrypted   normal   Ready    dal10   1.9.7
    ```
    {: screen}

    b. 打开浏览器并通过以下 URL 检查应用程序：`http://<public_IP_address>:<NodePort>`. 使用示例值时，URL 为 `http://169.xx.xxx.xxx:30872`。您可以将此 URL 提供给同事试用，或者在您的手机浏览器中输入此 URL，从而可以查看该应用程序是否确实已公共可用。

    <img src="images/python_flask.png" alt="已部署样板 Python Flask 应用程序的截屏。" />

5.  [启动 Kubernetes 仪表板](cs_app.html#cli_dashboard)。

    如果在 [{{site.data.keyword.Bluemix_notm}}GUI](https://console.bluemix.net/) 中选择了集群，那么可以使用 **Kubernetes 仪表板**按钮来通过一次单击启动仪表板。
    {: tip}

6. 在**工作负载**选项卡中，可以查看已创建的资源。

祝贺您！您的应用程序已部署在容器中！
