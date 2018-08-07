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





# 为 {{site.data.keyword.Bluemix_dedicated_notm}} 映像注册表创建 {{site.data.keyword.registryshort_notm}} 令牌
{: #cs_dedicated_tokens}

针对用于 {{site.data.keyword.containerlong}} 集群的单个组和可扩展组的映像注册表，创建不到期的令牌。
{:shortdesc}

1.  登录到 {{site.data.keyword.Bluemix_dedicated_notm}} 环境。

    ```
        bx login -a api.<dedicated_domain>
    ```
    {: pre}

2.  为当前会话请求 `oauth-token` 并将其保存为变量。

    ```
    OAUTH_TOKEN=`bx iam oauth-tokens | awk 'FNR == 2 {print $3 " " $4}'`
    ```
    {: pre}

3.  为当前会话请求组织标识，并将其保存为变量。

    ```
    ORG_GUID=`bx iam org <org_name> --guid`
    ```
    {: pre}

4.  为当前会话请求永久注册表标记。将 <dedicated_domain> 替换为 {{site.data.keyword.Bluemix_dedicated_notm}} 环境的域。此令牌将授予对当前名称空间中的映像的访问权。

    ```
        curl -XPOST -H "Authorization: ${OAUTH_TOKEN}" -H "Organization: ${ORG_GUID}" https://registry.<dedicated_domain>/api/v1/tokens?permanent=true
    ```
    {: pre}

    输出：

    ```
    {
        "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI2MzdiM2Q4Yy1hMDg3LTVhZjktYTYzNi0xNmU3ZWZjNzA5NjciLCJpc3MiOiJyZWdpc3RyeS5jZnNkZWRpY2F0ZWQxLnVzLXNvdXRoLmJsdWVtaXgubmV0"
    }
    ```
    {: screen}

5.  验证 Kubernetes 私钥。

    ```
        kubectl describe secrets
    ```
    {: pre}

    您可以使用此私钥来处理 {{site.data.keyword.containerlong}}。

6.  创建 Kubernetes 私钥以用于存储令牌信息。

    ```
        kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_url> --docker-username=token --docker-password=<token_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>了解此命令的组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解此命令的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace &lt;kubernetes_namespace&gt;</code></td>
    <td>必需。要使用私钥并将容器部署到的集群的 Kubernetes 名称空间。运行 <code>kubectl get namespaces</code> 可列出集群中的所有名称空间。</td>
    </tr>
    <tr>
    <td><code>&lt;secret_name&gt;</code></td>
    <td>必需。要用于 imagePullSecret 的名称。</td>
    </tr>
    <tr>
    <td><code>--docker-server &lt;registry_url&gt;</code></td>
    <td>必需。指向用于设置名称空间的映像注册表的 URL：registry.&lt;dedicated_domain&gt;</li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username &lt;docker_username&gt;</code></td>
    <td>必需。用于登录到专用注册表的用户名。</td>
    </tr>
    <tr>
    <td><code>--docker-password &lt;token_value&gt;</code></td>
    <td>必需。先前检索到的注册表令牌的值。</td>
    </tr>
    <tr>
    <td><code>--docker-email &lt;docker-email&gt;</code></td>
    <td>必需。如果您有 Docker 电子邮件地址，请输入该地址。如果没有，请输入虚构的电子邮件地址，例如 a@b.c。此电子邮件对于创建 Kubernetes 私钥是必需的，但在创建后不会再使用此电子邮件。</td>
    </tr>
    </tbody></table>

7.  创建引用 imagePullSecret 的 pod。

    1.  打开首选文本编辑器，并创建名为 mypod.yaml 的 pod 配置脚本。
    2.  定义要用于访问注册表的 pod 和 imagePullSecret。要使用名称空间中的专用映像，请使用以下内容：


        ```
        apiVersion: v1
        kind: Pod
        metadata:
          name: <pod_name>
        spec:
          containers:
            - name: <container_name>
              image: registry.<dedicated_domain>/<my_namespace>/<my_image>:<tag>
          imagePullSecrets:
            - name: <secret_name>
        ```
        {: codeblock}

        <table>
        <caption>了解 YAML 文件的组成部分</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
        </thead>
        <tbody>
        <tr>
        <td><code>&lt;pod_name&gt;</code></td>
        <td>要创建的 pod 的名称。</td>
        </tr>
        <tr>
        <td><code>&lt;container_name&gt;</code></td>
        <td>要部署到集群的容器的名称。</td>
        </tr>
        <tr>
        <td><code>&lt;my_namespace&gt;</code></td>
        <td>存储映像的名称空间。要列出可用名称空间，请运行 `bx cr namespace-list`。</td>
        </tr>
        <td><code>&lt;my_image&gt;</code></td>
        <td>要使用的映像的名称。要列出 {{site.data.keyword.Bluemix_notm}} 帐户中的可用映像，请运行 <code>bx cr image-list</code>。</td>
        </tr>
        <tr>
        <td><code>&lt;tag&gt;</code></td>
        <td>要使用的映像的版本。如果未指定标记，那么缺省情况下会使用标记为 <strong>latest</strong> 的映像。</td>
        </tr>
        <tr>
        <td><code>&lt;secret_name&gt;</code></td>
        <td>先前创建的 imagePullSecret 的名称。</td>
        </tr>
        </tbody></table>

    3.  保存更改。

    4.  在集群中创建部署。

          ```
                    kubectl apply -f mypod.yaml
          ```
          {: pre}

