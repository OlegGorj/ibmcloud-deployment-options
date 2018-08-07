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



# 日志记录和监视故障诊断
{: #cs_troubleshoot_health}

在使用 {{site.data.keyword.containerlong}} 时，请考虑对日志记录和监视问题进行故障诊断的以下方法。
{: shortdesc}

如果您有更常规的问题，请尝试[集群调试](cs_troubleshoot.html)。
{: tip}

## 日志不显示
{: #cs_no_logs}

{: tsSymptoms}
访问 Kibana 仪表板时，日志不显示。

{: tsResolve}
查看导致集群日志不显示的以下原因以及对应的故障诊断步骤：

<table>
<caption>对不显示的日志进行故障诊断</caption>
  <col width="40%">
  <col width="60%">
  <thead>
    <tr>
      <th>问题原因</th>
      <th>解决方法</th>
    </tr>
 </thead>
 <tbody>
  <tr>
    <td>未设置任何日志记录配置。</td>
    <td>要发送日志，必须先创建日志记录配置。为此，请参阅<a href="cs_health.html#logging">配置集群日志记录</a>。</td>
  </tr>
  <tr>
    <td>集群不处于 <code>Normal</code> 状态。</td>
    <td>要检查集群的状态，请参阅<a href="cs_troubleshoot.html#debug_clusters">调试集群</a>。</td>
  </tr>
  <tr>
    <td>已达到日志存储配额。</td>
    <td>要提高日志存储限制，请参阅 <a href="/docs/services/CloudLogAnalysis/troubleshooting/error_msgs.html">{{site.data.keyword.loganalysislong_notm}} 文档</a>。</td>
  </tr>
  <tr>
    <td>如果您在创建集群时指定了空间，帐户所有者没有对该空间的“管理员”、“开发者”或“审计员”许可权。</td>
      <td>要更改帐户所有者的访问许可权，请执行以下操作：<ol><li>要找出集群的帐户所有者，请运行 <code>bx cs api-key-info &lt;cluster_name_or_ID&gt;</code>。</li>
      <li>要授予帐户所有者对空间的 {{site.data.keyword.containershort_notm}}“管理员”、“开发者”或“审计员”访问许可权，请参阅<a href="cs_users.html">管理集群访问权</a>。</li>
      <li>要在更改许可权后刷新日志记录令牌，请运行 <code>bx cs logging-config-refresh &lt;cluster_name_or_ID&gt;</code>。</li></ol></td>
    </tr>
    <tr>
      <td>您的应用程序日志记录配置的应用程序路径中有符号链接。</td>
      <td><p>要发送日志，必须在日志记录配置中使用绝对路径，否则无法读取日志。如果路径安装到工作程序节点上，那么可能已创建符号链接。</p> <p>示例：如果指定的路径为 <code>/usr/local/<b>spark</b>/work/app-0546/0/stderr</code>，但日志却转至 <code>/usr/local/<b>spark-1.0-hadoop-1.2</b>/work/app-0546/0/stderr</code>，那么无法读取日志。</p></td>
    </tr>
  </tbody>
</table>

要测试在故障诊断期间进行的更改，可以在集群中的一个工作程序节点上部署 *Noisy*（用于生成多个日志事件的样本 pod）。

  1. [设定 CLI 的目标](cs_cli_install.html#cs_cli_configure)，将目标设置为要在其中开始生成日志的集群。

  2. 创建 `deploy-noisy.yaml` 配置文件。

      ```
      apiVersion: v1
      kind: Pod
      metadata:
        name: noisy
      spec:
        containers:
        - name: noisy
          image: ubuntu:16.04
          command: ["/bin/sh"]
          args: ["-c", "while true; do sleep 10; echo 'Hello world!'; done"]
          imagePullPolicy: "Always"
        ```
        {: codeblock}

  3. 在集群上下文中运行该配置文件。

        ```
                kubectl apply -f noisy.yaml
        ```
        {:pre}

  4. 几分钟后，可以在 Kibana 仪表板中查看日志。要访问 Kibana 仪表板，请转至以下某个 URL，然后选择在其中创建了集群的 {{site.data.keyword.Bluemix_notm}} 帐户。如果在创建集群时指定了空间，请改为转至该空间。
      - 美国南部和美国东部：https://logging.ng.bluemix.net
      - 英国南部：https://logging.eu-gb.bluemix.net
      - 欧洲中部：https://logging.eu-fra.bluemix.net
      - 亚太南部：https://logging.au-syd.bluemix.net

<br />


## Kubernetes 仪表板不显示利用率图形
{: #cs_dashboard_graphs}

{: tsSymptoms}
访问 Kibana 仪表板时，利用率图形不显示。

{: tsCauses}
有时，在集群更新或工作程序节点重新引导后，`kube-dashboard` pod 不会更新。

{: tsResolve}
删除 `kube-dashboard` pod 以强制重新启动。该 pod 会通过 RBAC 策略重新创建，以用于访问 heapster 来获取利用率信息。

  ```
    kubectl delete pod -n kube-system $(kubectl get pod -n kube-system --selector=k8s-app=kubernetes-dashboard -o jsonpath='{.items..metadata.name}')
    ```
  {: pre}

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

