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





# 设置 CLI 和 API
{: #cs_cli_install}

可以使用 {{site.data.keyword.containerlong}} CLI 或 API 来创建和管理 Kubernetes 集群。
{:shortdesc}

<br />


## 安装 CLI
{: #cs_cli_install_steps}

安装必需的 CLI 以在 {{site.data.keyword.containershort_notm}} 中创建和管理 Kubernetes 集群，并将容器化应用程序部署到集群。
{:shortdesc}

此任务包含用于安装这些 CLI 和插件的信息：

-   {{site.data.keyword.Bluemix_notm}} CLI V0.5.0 或更高版本
-   {{site.data.keyword.containershort_notm}} 插件
-   与集群的 `major.minor` 版本相匹配的 Kubernetes CLI 版本
-   可选：{{site.data.keyword.registryshort_notm}} 插件
-   可选：Docker V1.9 或更高版本

<br>
要安装 CLI，请执行以下操作：



1.  作为 {{site.data.keyword.containershort_notm}} 插件的必备软件，请安装 [{{site.data.keyword.Bluemix_notm}} CLI ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://clis.ng.bluemix.net/ui/home.html)。用于通过 {{site.data.keyword.Bluemix_notm}} CLI 运行命令的前缀是 `bx`。

2.  登录到 {{site.data.keyword.Bluemix_notm}} CLI。根据提示，输入您的 {{site.data.keyword.Bluemix_notm}} 凭证。

    ```
        bx login
    ```
    {: pre}

    **注**：如果您有联合标识，请使用 `bx login --sso` 登录到 {{site.data.keyword.Bluemix_notm}} CLI。输入您的用户名，并使用 CLI 输出中提供的 URL 来检索一次性密码。如果不使用 `--sso` 时登录失败，而使用 `--sso` 选项时登录成功，说明您拥有的是联合标识。

3.  要创建 Kubernetes 集群以及管理工作程序节点，请安装 {{site.data.keyword.containershort_notm}} 插件。用于通过 {{site.data.keyword.containershort_notm}} 插件运行命令的前缀是 `bx cs`。

    ```
        bx plugin install container-service -r Bluemix
    ```
    {: pre}

    要验证是否已正确安装该插件，请运行以下命令：

    ```
        bx plugin list
    ```
    {: pre}

    {{site.data.keyword.containershort_notm}} 插件会在结果中显示为 container-service。

4.  {: #kubectl}要查看 Kubernetes 仪表板的本地版本以及将应用程序部署到集群，请[安装 Kubernetes CLI ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/tasks/tools/install-kubectl/)。用于通过 Kubernetes CLI 来运行命令的前缀是 `kubectl`。

    1.  下载与您计划使用的 Kubernetes 集群 `major.minor` 版本相匹配的 Kubernetes CLI `major.minor` 版本。当前 {{site.data.keyword.containershort_notm}} 缺省 Kubernetes 版本是 1.9.7。**注**：如果使用的 `kubectl` CLI 版本并未至少与集群的 `major.minor` 版本相匹配，那么可能会遇到意外的结果。请确保 Kubernetes 集群版本和 CLI 版本保持最新。


        - **OS X**：[https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/darwin/amd64/kubectl ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/darwin/amd64/kubectl)
        - **Linux**：[https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/linux/amd64/kubectl ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/linux/amd64/kubectl)
        - **Windows**：[https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/windows/amd64/kubectl.exe ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/windows/amd64/kubectl.exe)

    2.  **对于 OSX 和 Linux**：完成以下步骤。
        1.  将可执行文件移至 `/usr/local/bin` 目录。

            ```
                        mv /filepath/kubectl /usr/local/bin/kubectl
            ```
            {: pre}

        2.  确保 `/usr/local/bin` 列在 `PATH` 系统变量中。`PATH` 变量包含操作系统可以在其中找到可执行文件的所有目录。列在 `PATH` 变量中的目录用于不同的用途。`/usr/local/bin` 用于为不属于操作系统的一部分，而是由系统管理员手动安装的软件存储其可执行文件。

            ```
                        echo $PATH
            ```
            {: pre}

            示例 CLI 输出：

            ```
                        /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
            ```
            {: screen}

        3.  使文件可执行。

            ```
                        chmod +x /usr/local/bin/kubectl
            ```
            {: pre}

    3.  **对于 Windows：**将 Kubernetes CLI 安装在 {{site.data.keyword.Bluemix_notm}} CLI 所在的目录中。此安装将在您以后运行命令时减少一些文件路径更改操作。

5.  要管理专用映像存储库，请安装 {{site.data.keyword.registryshort_notm}} 插件。使用此插件可在 IBM 托管的具备高可用性和高可扩展性的多租户专用映像注册表中设置自己的名称空间，存储 Docker 映像并与其他用户共享这些映像。要将容器部署到集群中，Docker 映像是必需的。用于运行注册表命令的前缀是 `bx cr`。

    ```
        bx plugin install container-registry -r Bluemix
    ```
    {: pre}

    要验证是否已正确安装该插件，请运行以下命令：

    ```
        bx plugin list
    ```
    {: pre}

    该插件会在结果中显示为 container-registry。

6.  要在本地构建映像并将其推送到注册表名称空间，请[安装 Docker ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.docker.com/community-edition#/download)。如果使用的是 Windows 8 或更低版本，可以改为安装 [Docker Toolbox ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.docker.com/toolbox/toolbox_install_windows/)。Docker CLI 用于将应用程序构建成映像。用于通过 Docker CLI 运行命令的前缀是 `docker`。

接下来，开始[使用 {{site.data.keyword.containershort_notm}} 通过 CLI 创建 Kubernetes 集群](cs_clusters.html#clusters_cli)。

有关这些 CLI 的参考信息，请参阅那些工具的文档。

-   [`bx` 命令](/docs/cli/reference/bluemix_cli/bx_cli.html)
-   [`bx cs` 命令](cs_cli_reference.html#cs_cli_reference)
-   [`kubectl` 命令 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/reference/kubectl/overview/)
-   [`bx cr` 命令](/docs/cli/plugins/registry/index.html)

<br />




## 在计算机上的容器中运行 CLI
{: #cs_cli_container}

您可以将多个 CLI 安装到计算机上运行的一个容器中，而不必分别将每个 CLI 安装在计算机上。
{:shortdesc}

1. 基于提供的 Dockerfile 创建映像。

    ```
    docker build -t <image_name> https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/install-clis-container/Dockerfile
    ```
    {: pre}

2. 将映像本地部署为容器，并安装卷以访问本地文件。
  
    ```
    docker run -it -v /local/path:/container/volume <image_name>
    ```
    {: pre}
  
3. 在交互式 shell 中，开始运行 `bx cs` 和 `kubectl` 命令。如果创建了要保存的数据，请将这些数据保存到安装的卷中。退出 shell 时，容器会停止。 

<br />



## 将 CLI 配置为运行 `kubectl`
{: #cs_cli_configure}

您可以使用 Kubernetes CLI 随附的命令，在 {{site.data.keyword.Bluemix_notm}} 中管理集群。
{:shortdesc}

支持 Kubernetes 1.9.7 中可用的所有 `kubectl` 命令用于 {{site.data.keyword.Bluemix_notm}} 中的集群。创建集群后，使用环境变量，将本地 CLI 的上下文设置到该集群。
然后，您可以运行 Kubernetes `kubectl` 命令，以在 {{site.data.keyword.Bluemix_notm}} 中使用集群。


[安装必需的 CLI](#cs_cli_install) 并[创建集群](cs_clusters.html#clusters_cli)后，才能运行 `kubectl` 命令。

1.  登录到 {{site.data.keyword.Bluemix_notm}} CLI。根据提示，输入您的 {{site.data.keyword.Bluemix_notm}} 凭证。要指定 {{site.data.keyword.Bluemix_notm}} 区域，请[包含 API 端点](cs_regions.html#bluemix_regions)。

    ```
        bx login
    ```
    {: pre}

    **注**：如果您有联合标识，请使用 `bx login --sso` 登录到 {{site.data.keyword.Bluemix_notm}} CLI。输入您的用户名，并使用 CLI 输出中提供的 URL 来检索一次性密码。如果不使用 `--sso` 时登录失败，而使用 `--sso` 选项时登录成功，说明您拥有的是联合标识。

2.  选择 {{site.data.keyword.Bluemix_notm}} 帐户。如果您分配有多个 {{site.data.keyword.Bluemix_notm}} 组织，请选择在其中创建集群的组织。集群是特定于组织的，但又独立于 {{site.data.keyword.Bluemix_notm}} 空间。因此，您无需选择空间。

3.  要在先前选择的 {{site.data.keyword.Bluemix_notm}} 区域以外的区域中创建或访问 Kubernetes 集群，请运行 `bx cs region-set`。

4.  列出帐户中的所有集群以获取集群的名称。

    ```
            bx cs clusters
        ```
    {: pre}

5.  将所创建的集群设置为此会话的上下文。每次使用集群时都完成这些配置步骤。
    1.  获取命令以设置环境变量并下载 Kubernetes 配置文件。

        ```
            bx cs cluster-config <cluster_name_or_ID>
    ```
        {: pre}

        下载配置文件后，会显示一个命令，您可以使用该命令将本地 Kubernetes 配置文件的路径设置为环境变量。

        示例：

        ```
            export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
    ```
        {: screen}

    2.  复制并粘贴终端中显示的命令，以设置 `KUBECONFIG` 环境变量。

    3.  验证是否已正确设置 `KUBECONFIG` 环境变量。

        示例：

        ```
                echo $KUBECONFIG
        ```
        {: pre}

        输出：
        ```
                /Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

6.  通过检查 Kubernetes CLI 服务器版本，验证 `kubectl` 命令是否针对您的集群正常运行。

    ```
            kubectl version  --short
        ```
    {: pre}

    输出示例：

    ```
    Client Version: v1.9.7
    Server Version: v1.9.7
    ```
    {: screen}

现在，可以运行 `kubectl` 命令，在 {{site.data.keyword.Bluemix_notm}} 中管理集群。有关完整的命令列表，请参阅 [Kubernetes 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/reference/kubectl/overview/)。

**提示：**如果使用的是 Windows，而 Kubernetes CLI 未安装在 {{site.data.keyword.Bluemix_notm}} CLI 所在的目录中，那么必须将目录更改为 Kubernetes CLI 的安装路径才能成功运行 `kubectl` 命令。


<br />


## 更新 CLI
{: #cs_cli_upgrade}

您可能想要定期更新 CLI 以使用新功能。
{:shortdesc}

此任务包含用于更新这些 CLI 的信息。

-   {{site.data.keyword.Bluemix_notm}} CLI V0.5.0 或更高版本
-   {{site.data.keyword.containershort_notm}} 插件
-   Kubernetes CLI V1.9.7 或更高版本
-   {{site.data.keyword.registryshort_notm}} 插件
-   Docker V1.9 或更高版本

<br>
要更新 CLI，请执行以下操作：


1.  更新 {{site.data.keyword.Bluemix_notm}} CLI。下载[最新版本 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://clis.ng.bluemix.net/ui/home.html) 并运行安装程序。

2. 登录到 {{site.data.keyword.Bluemix_notm}} CLI。根据提示，输入您的 {{site.data.keyword.Bluemix_notm}} 凭证。要指定 {{site.data.keyword.Bluemix_notm}} 区域，请[包含 API 端点](cs_regions.html#bluemix_regions)。

    ```
        bx login
    ```
    {: pre}

     **注**：如果您有联合标识，请使用 `bx login --sso` 登录到 {{site.data.keyword.Bluemix_notm}} CLI。输入您的用户名，并使用 CLI 输出中提供的 URL 来检索一次性密码。如果不使用 `--sso` 时登录失败，而使用 `--sso` 选项时登录成功，说明您拥有的是联合标识。

3.  更新 {{site.data.keyword.containershort_notm}} 插件。
    1.  通过 {{site.data.keyword.Bluemix_notm}} 插件存储库安装更新。

        ```
                bx plugin update container-service -r Bluemix
        ```
        {: pre}

    2.  通过运行以下命令并检查已安装的插件的列表来验证插件安装。

        ```
            bx plugin list
    ```
        {: pre}

        {{site.data.keyword.containershort_notm}} 插件会在结果中显示为 container-service。

    3.  初始化 CLI。

        ```
                bx cs init
        ```
        {: pre}

4.  [更新 Kubernetes CLI](#kubectl)。

5.  更新 {{site.data.keyword.registryshort_notm}} 插件。
    1.  通过 {{site.data.keyword.Bluemix_notm}} 插件存储库安装更新。

        ```
                bx plugin update container-registry -r Bluemix
        ```
        {: pre}

    2.  通过运行以下命令并检查已安装的插件的列表来验证插件安装。

        ```
            bx plugin list
    ```
        {: pre}

        Registry 插件会在结果中显示为 container-registry。

6.  更新 Docker。
    -   如果您使用 Docker Community Edition，请启动 Docker，单击 **Docker** 图标并单击**检查更新**。
    -   如果使用的是 Docker Toolbox，请下载[最新版本 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.docker.com/toolbox/toolbox_install_windows/) 并运行安装程序。

<br />


## 卸载 CLI
{: #cs_cli_uninstall}

如果不再需要 CLI，可以将其卸载。
{:shortdesc}

此任务包含用于除去以下 CLI 的信息：


-   {{site.data.keyword.containershort_notm}} 插件
-   Kubernetes CLI
-   {{site.data.keyword.registryshort_notm}} 插件
-   Docker V1.9 或更高版本

<br>
要卸载 CLI，请执行以下操作：



1.  卸载 {{site.data.keyword.containershort_notm}} 插件。

    ```
        bx plugin uninstall container-service
    ```
    {: pre}

2.  卸载 {{site.data.keyword.registryshort_notm}} 插件。

    ```
        bx plugin uninstall container-registry
    ```
    {: pre}

3.  通过运行以下命令并检查已安装的插件的列表，验证插件是否已卸载。

    ```
        bx plugin list
    ```
    {: pre}

    container-service 和 container-registry 插件未显示在结果中。

6.  卸载 Docker。有关卸载 Docker 的指示信息因您使用的操作系统不同而有所不同。

    - [OSX ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.docker.com/docker-for-mac/#uninstall-or-reset)
    - [Linux ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/#uninstall-docker-ce)
    - [Windows ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://docs.docker.com/toolbox/toolbox_install_windows/#how-to-uninstall-toolbox)

<br />


## 使用 API 自动部署集群
{: #cs_api}

可以使用 {{site.data.keyword.containershort_notm}} API 来自动创建、部署和管理 Kubernetes 集群。
{:shortdesc}

{{site.data.keyword.containershort_notm}} API 需要您必须在 API 请求中提供的头信息，根据您要使用的 API，这些头信息可以有所不同。
要确定 API 需要哪些头信息，请参阅 [{{site.data.keyword.containershort_notm}} API 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://us-south.containers.bluemix.net/swagger-api)。

**注：**要向 {{site.data.keyword.containershort_notm}} 进行认证，必须提供使用 {{site.data.keyword.Bluemix_notm}} 凭证生成的 Identity and Access Management (IAM) 令牌，并包括创建集群的 {{site.data.keyword.Bluemix_notm}} 帐户标识。根据您向 {{site.data.keyword.Bluemix_notm}} 进行认证的方式，可以在下列选项之间进行选择，以自动创建 IAM 令牌。

<table>
<caption>标识类型和选项</caption>
<thead>
<th>{{site.data.keyword.Bluemix_notm}} 标识</th>
<th>我的选项</th>
</thead>
<tbody>
<tr>
<td>未联合的标识</td>
<td><ul><li><strong>{{site.data.keyword.Bluemix_notm}} 用户名和密码：</strong>您可以遵循本主题中的步骤来完全自动化 IAM 访问令牌的创建。</li>
<li><strong>生成 {{site.data.keyword.Bluemix_notm}} API 密钥：</strong>作为使用 {{site.data.keyword.Bluemix_notm}} 用户名和密码的替代方法，您可以<a href="../iam/apikeys.html#manapikey" target="_blank">使用 {{site.data.keyword.Bluemix_notm}} API 密钥</a>。{{site.data.keyword.Bluemix_notm}} API 密钥依赖于为其生成这些密钥的 {{site.data.keyword.Bluemix_notm}} 帐户。您不能将 {{site.data.keyword.Bluemix_notm}} API 密钥与同一 IAM 令牌中的不同帐户标识组合使用。要访问使用非 {{site.data.keyword.Bluemix_notm}} API 密钥所基于的帐户创建的集群，必须登录该帐户以生成新的 API 密钥。</li></ul></tr>
<tr>
<td>联合标识</td>
<td><ul><li><strong>生成 {{site.data.keyword.Bluemix_notm}} API 密钥：</strong><a href="../iam/apikeys.html#manapikey" target="_blank">{{site.data.keyword.Bluemix_notm}} API 密钥</a>依赖于为其生成这些密钥的 {{site.data.keyword.Bluemix_notm}} 帐户。您不能将 {{site.data.keyword.Bluemix_notm}} API 密钥与同一 IAM 令牌中的不同帐户标识组合使用。要访问使用非 {{site.data.keyword.Bluemix_notm}} API 密钥所基于的帐户创建的集群，必须登录该帐户以生成新的 API 密钥。</li><li><strong>使用一次性密码：</strong>如果使用一次性密码向 {{site.data.keyword.Bluemix_notm}} 进行认证，那么无法完全自动化 IAM 令牌的创建，因为检索一次性密码需要与 Web 浏览器进行手动交互。要完全自动化 IAM 令牌的创建，必须改为创建 {{site.data.keyword.Bluemix_notm}} API 密钥。</ul></td>
</tr>
</tbody>
</table>

1.  创建 IAM (Identity and Access Management) 访问令牌。包含在请求中的主体信息根据您使用的 {{site.data.keyword.Bluemix_notm}} 认证方法而有所不同。替换以下值：
  - _&lt;username&gt;_：您的 {{site.data.keyword.Bluemix_notm}} 用户名。
  - _&lt;password&gt;_：您的 {{site.data.keyword.Bluemix_notm}} 密码。
  - _&lt;api_key&gt;_：您的 {{site.data.keyword.Bluemix_notm}} API 密钥。
  - _&lt;passcode&gt;_：您的 {{site.data.keyword.Bluemix_notm}} 一次性密码。运行 `bx login --sso`，并按照 CLI 输出中的指示信息，使用 Web 浏览器检索一次性密码。

    ```
        POST https://iam.<region>.bluemix.net/oidc/token
    ```
    {: codeblock}

    示例：
    ```
        POST https://iam.ng.bluemix.net/oidc/token
    ```
    {: codeblock}

    要指定 {{site.data.keyword.Bluemix_notm}} 区域，请[复查 API 端点中使用的区域缩写](cs_regions.html#bluemix_regions)。

    <table summary-"Input parameters to retrieve tokens">
    <caption>用于获取令牌的输入参数</caption>
    <thead>
        <th>输入参数</th>
        <th>值</th>
    </thead>
    <tbody>
    <tr>
    <td>头</td>
    <td><ul><li>Content-Type:application/x-www-form-urlencoded</li> <li>Authorization: Basic Yng6Yng=<p><strong>注</strong>：<code>Yng6Yng=</code> 是针对用户名 <strong>bx</strong> 和密码 <strong>bx</strong> 的 URL 编码授权。</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>{{site.data.keyword.Bluemix_notm}} 用户名和密码的主体</td>
    <td><ul><li>grant_type: password</li>
    <li>response_type: cloud_iam uaa</li>
    <li>username: <em>&lt;username&gt;</em></li>
    <li>password: <em>&lt;password&gt;</em></li>
    <li>uaa_client_ID: cf</li>
    <li>uaa_client_secret: </li></ul>
    <strong>注</strong>：添加不指定值的 <code>uaa_client_secret</code> 键。</td>
    </tr>
    <tr>
    <td>{{site.data.keyword.Bluemix_notm}} API 密钥的主体</td>
    <td><ul><li>grant_type: urn:ibm:params:oauth:grant-type:apikey</li>
    <li>response_type: cloud_iam uaa</li>
    <li>apikey: <em>&lt;api_key&gt;</em></li>
    <li>uaa_client_ID: cf</li>
    <li>uaa_client_secret: </li></ul>
    <strong>注</strong>：添加不指定值的 <code>uaa_client_secret</code> 键。</td>
    </tr>
    <tr>
    <td>{{site.data.keyword.Bluemix_notm}} 一次性密码的主体</td>
    <td><ul><li>grant_type: urn:ibm:params:oauth:grant-type:passcode</li>
    <li>response_type: cloud_iam uaa</li>
    <li>passcode: <em>&lt;passcode&gt;</em></li>
    <li>uaa_client_ID: cf</li>
    <li>uaa_client_secret: </li></ul>
    <strong>注</strong>：添加不指定值的 <code>uaa_client_secret</code> 键。</td>
    </tr>
    </tbody>
    </table>

    示例 API 输出：

    ```
    {
    "access_token": "<iam_token>",
      "refresh_token": "<iam_refresh_token>",
      "uaa_token": "<uaa_token>",
      "uaa_refresh_token": "<uaa_refresh_token>",
      "token_type": "Bearer",
      "expires_in": 3600,
      "expiration": 1493747503
    }

    

    ```
    {: screen}

    您可以在 API 输出的 **access_token** 字段中找到 IAM 令牌。请记下 IAM 令牌以在后续步骤中用于检索更多头信息。

2.  检索创建集群的 {{site.data.keyword.Bluemix_notm}} 帐户的标识。将 _&lt;iam_token&gt;_ 替换为在上一步中检索到的 IAM 令牌。

    ```
        GET https://accountmanagement.<region>.bluemix.net/v1/accounts
    ```
    {: codeblock}

    <table summary="用于获取 {{site.data.keyword.Bluemix_notm}} 帐户标识的输入参数">
    <caption>用于获取 {{site.data.keyword.Bluemix_notm}} 帐户标识的输入参数</caption>
    <thead>
  	<th>输入参数</th>
  	<th>值</th>
    </thead>
    <tbody>
  	<tr>
  		<td>头</td>
  		<td><ul><li>Content-Type: application/json</li>
      <li>Authorization: bearer &lt;iam_token&gt;</li>
      <li>Accept: application/json
</li></ul></td>
  	</tr>
    </tbody>
    </table>

    示例 API 输出：

    ```
    {
      "total_results": 3,
      "total_pages": 1,
      "prev_url": null,
      "next_url": null,
      "resources":
        {
          "metadata": {
"guid": "<account_ID>",
            "url": "/v1/accounts/<account_ID>",
            "created_at": "2016-01-07T18:55:09.726Z",
            "updated_at": "2017-04-28T23:46:03.739Z",
            "origin": "BSS"
    ...
    ```
    {: screen}

    可以在 API 输出的 **resources/metadata/guid** 字段中找到您的 {{site.data.keyword.Bluemix_notm}} 帐户的标识。

3.  生成新的 IAM 令牌，该令牌包含您的 {{site.data.keyword.Bluemix_notm}} 凭证和创建集群的帐户标识。将 _&lt;account_ID&gt;_ 替换为在上一步中检索到的 {{site.data.keyword.Bluemix_notm}} 帐户的标识。

    **注：**如果使用的是 {{site.data.keyword.Bluemix_notm}} API 密钥，那么必须使用为其创建 API 密钥的 {{site.data.keyword.Bluemix_notm}} 帐户标识。要访问其他帐户中的集群，请登录此帐户并创建基于此帐户的 {{site.data.keyword.Bluemix_notm}} API 密钥。

    ```
        POST https://iam.<region>.bluemix.net/oidc/token
    ```
    {: codeblock}

    示例：
    ```
        POST https://iam.ng.bluemix.net/oidc/token
    ```
    {: codeblock}

    要指定 {{site.data.keyword.Bluemix_notm}} 区域，请[复查 API 端点中使用的区域缩写](cs_regions.html#bluemix_regions)。

    <table summary-"Input parameters to retrieve tokens">
    <caption>用于获取令牌的输入参数</caption>
    <thead>
        <th>输入参数</th>
        <th>值</th>
    </thead>
    <tbody>
    <tr>
    <td>头</td>
    <td><ul><li>Content-Type:application/x-www-form-urlencoded</li> <li>Authorization: Basic Yng6Yng=<p><strong>注</strong>：<code>Yng6Yng=</code> 是针对用户名 <strong>bx</strong> 和密码 <strong>bx</strong> 的 URL 编码授权。</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>{{site.data.keyword.Bluemix_notm}} 用户名和密码的主体</td>
    <td><ul><li>grant_type: password</li>
    <li>response_type: cloud_iam uaa</li>
    <li>username: <em>&lt;username&gt;</em></li>
    <li>password: <em>&lt;password&gt;</em></li>
    <li>uaa_client_ID: cf</li>
    <li>uaa_client_secret: </li>
    <li>bss_account: <em>&lt;account_ID&gt;</em></li></ul>
    <strong>注</strong>：添加不指定值的 <code>uaa_client_secret</code> 键。</td>
    </tr>
    <tr>
    <td>{{site.data.keyword.Bluemix_notm}} API 密钥的主体</td>
    <td><ul><li>grant_type: urn:ibm:params:oauth:grant-type:apikey</li>
    <li>response_type: cloud_iam uaa</li>
    <li>apikey: <em>&lt;api_key&gt;</em></li>
    <li>uaa_client_ID: cf</li>
    <li>uaa_client_secret: </li>
    <li>bss_account: <em>&lt;account_ID&gt;</em></li></ul>
      <strong>注</strong>：添加不指定值的 <code>uaa_client_secret</code> 键。</td>
    </tr>
    <tr>
    <td>{{site.data.keyword.Bluemix_notm}} 一次性密码的主体</td>
    <td><ul><li>grant_type: urn:ibm:params:oauth:grant-type:passcode</li>
    <li>response_type: cloud_iam uaa</li>
    <li>passcode: <em>&lt;passcode&gt;</em></li>
    <li>uaa_client_ID: cf</li>
    <li>uaa_client_secret: </li>
    <li>bss_account: <em>&lt;account_ID&gt;</em></li></ul><strong>注</strong>：添加不指定值的 <code>uaa_client_secret</code> 键。</td>
    </tr>
    </tbody>
    </table>

    示例 API 输出：

    ```
    {
      "access_token": "<iam_token>",
      "refresh_token": "<iam_refresh_token>",
      "uaa_token": "<uaa_token>",
      "uaa_refresh_token": "<uaa_refresh_token>",
      "token_type": "Bearer",
      "expires_in": 3600,
      "expiration": 1493747503
    }

    

    ```
    {: screen}

    您可以在 **access_token** 中找到 IAM 令牌，在 **refresh_token** 中找到 IAM 刷新令牌。

4.  列出您帐户中的所有 Kubernetes 集群。使用在先前步骤中检索到的信息来构建头信息。


     ```
             GET https://containers.bluemix.net/v1/clusters
        ```
     {: codeblock}

     <table summary="用于使用 API 的输入参数">
         <caption>用于使用 API 的输入参数</caption>
     <thead>
     <th>输入参数</th>
     <th>值</th>
     </thead>
     <tbody>
     <tr>
     <td>头</td>
     <td><ul><li>Authorization: bearer <em>&lt;iam_token&gt;</em></li>
     <li>X-Auth-Refresh-Token: <em>&lt;refresh_token&gt;</em></li></ul></td>
     </tr>
     </tbody>
     </table>

5.  请查看 [{{site.data.keyword.containershort_notm}} API 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://containers.bluemix.net/swagger-api)，以查找受支持 API 的列表。

<br />


## 刷新 IAM 访问令牌并获取新的刷新令牌
{: #cs_api_refresh}

通过 API 发出的每个 IAM（身份和访问权管理）访问令牌都会在 1 小时后到期。必须定期刷新访问令牌才可确保对 {{site.data.keyword.Bluemix_notm}} API 的访问权。
您可以使用相同的步骤来获取新的刷新令牌。
{:shortdesc}

开始之前，请确保您拥有可用于请求新访问令牌的 IAM 刷新令牌或 {{site.data.keyword.Bluemix_notm}} API 密钥。
- **刷新令牌：**遵循[使用 {{site.data.keyword.Bluemix_notm}} API 自动执行集群创建和管理过程](#cs_api)中的指示信息。
- **API 密钥：**如下所示检索您的 [{{site.data.keyword.Bluemix_notm}} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/) API 密钥。
   1. 在菜单栏中，单击**管理** > **安全性** > **平台 API 密钥**。
   2. 单击**创建**。
   3. 输入 API 密钥的**名称**和**描述**，然后单击 **创建**。
   4. 单击**显示**以查看生成的 API 密钥。
   5. 复制 API 密钥，以便可以将其用于检索新的 IAM 访问令牌。

如果要创建 IAM 令牌或要获取新的刷新令牌，请执行以下步骤。

1.  使用刷新令牌或 {{site.data.keyword.Bluemix_notm}} API 密钥来生成新的 IAM 访问令牌。
    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="用于新 IAM 令牌的输入参数">
         <caption>用于新 IAM 令牌的输入参数</caption>
    <thead>
    <th>输入参数</th>
    <th>值</th>
    </thead>
    <tbody>
    <tr>
    <td>头</td>
    <td><ul><li>Content-Type: application/x-www-form-urlencoded</li>
      <li>Authorization: Basic Yng6Yng=</br></br><strong>注</strong>：<code>Yng6Yng=</code> 是针对用户名 <strong>bx</strong> 和密码 <strong>bx</strong> 的 URL 编码授权。</li></ul></td>
    </tr>
    <tr>
    <td>使用刷新令牌时的主体</td>
    <td><ul><li>grant_type: refresh_token</li>
    <li>response_type: cloud_iam uaa</li>
    <li>refresh_token: <em>&lt;iam_refresh_token&gt;</em></li>
    <li>uaa_client_ID: cf</li>
    <li>uaa_client_secret: </li>
    <li>bss_account: <em>&lt;account_ID&gt;</em></li></ul><strong>注</strong>：添加不指定值的 <code>uaa_client_secret</code> 键。</td>
    </tr>
    <tr>
      <td>使用 {{site.data.keyword.Bluemix_notm}} API 密钥时的主体</td>
      <td><ul><li>grant_type: <code>urn:ibm:params:oauth:grant-type:apikey</code></li>
    <li>response_type: cloud_iam uaa</li>
    <li>apikey: <em>&lt;api_key&gt;</em></li>
    <li>uaa_client_ID: cf</li>
        <li>uaa_client_secret: </li></ul><strong>注</strong>：添加不指定值的 <code>uaa_client_secret</code> 键。</td>
    </tr>
    </tbody>
    </table>

    示例 API 输出：

    ```
    {
      "access_token": "<iam_token>",
      "refresh_token": "<iam_refresh_token>",
      "uaa_token": "<uaa_token>",
      "uaa_refresh_token": "<uaa_refresh_token>",
      "token_type": "Bearer",
      "expires_in": 3600,
      "expiration": 1493747503
    }

    

    ```
    {: screen}

    您可以在 API 输出的 **access_token** 字段中找到新的 IAM 令牌，在 **refresh_token** 字段中找到 IAM 刷新令牌。

2.  使用上一步中的令牌，继续执行 [{{site.data.keyword.containershort_notm}} API 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://us-south.containers.bluemix.net/swagger-api) 中的操作。

