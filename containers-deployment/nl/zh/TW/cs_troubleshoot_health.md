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



# 記載及監視的疑難排解
{: #cs_troubleshoot_health}

在您使用 {{site.data.keyword.containerlong}} 時，請考慮使用這些技術來進行記載及監視問題的疑難排解。
{: shortdesc}

如果您有更一般性的問題，請嘗試[叢集除錯](cs_troubleshoot.html)。
{: tip}

## 日誌未出現
{: #cs_no_logs}

{: tsSymptoms}
存取 Kibana 儀表板時，未顯示任何日誌。

{: tsResolve}
請檢閱下列原因，以了解對應日誌為何未出現以及對應的疑難排解步驟：

<table>
<caption>疑難排解未顯示的日誌</caption>
  <col width="40%">
  <col width="60%">
  <thead>
    <tr>
      <th>發生原因</th>
      <th>修正方式</th>
    </tr>
 </thead>
 <tbody>
  <tr>
    <td>未設定任何記載配置。</td>
    <td>若要傳送日誌，您必須建立記載配置。如果要這麼做，請參閱<a href="cs_health.html#logging">配置叢集記載</a>。</td>
  </tr>
  <tr>
    <td>叢集未處於 <code>Normal</code> 狀態。</td>
    <td>若要檢查叢集的狀態，請參閱<a href="cs_troubleshoot.html#debug_clusters">叢集除錯</a>。</td>
  </tr>
  <tr>
    <td>已符合日誌儲存空間配額。</td>
    <td>若要增加日誌儲存空間限制，請參閱 <a href="/docs/services/CloudLogAnalysis/troubleshooting/error_msgs.html">{{site.data.keyword.loganalysislong_notm}} 文件</a>。</td>
  </tr>
  <tr>
    <td>如果您在建立叢集時指定了空間，則帳戶擁有者對該空間沒有「管理員」、「開發人員」或「審核員」許可權。</td>
      <td>若要變更帳戶擁有者的存取權，請執行下列動作：<ol><li>若要找出叢集的帳戶擁有者是誰，請執行 <code>bx cs api-key-info &lt;cluster_name_or_ID&gt;</code>。</li>
      <li>若要授與該帳戶擁有者對空間的「管理員」、「開發人員」或「審核員」等 {{site.data.keyword.containershort_notm}} 存取許可權，請參閱<a href="cs_users.html">管理叢集存取</a>。</li>
      <li>若要在許可權變更之後重新整理記載記號，請執行 <code>bx cs logging-config-refresh &lt;cluster_name_or_ID&gt;</code>。</li></ol></td>
    </tr>
    <tr>
      <td>您有應用程式記載配置，且在應用程式路徑中有符號鏈結。</td>
      <td><p>若要傳送日誌，您必須在記載配置中使用絕對路徑，否則無法讀取日誌。如果您的路徑已裝載至工作者節點，則可能已建立一個符號鏈結。</p> <p>範例：如果指定的路徑是 <code>/usr/local/<b>spark</b>/work/app-0546/0/stderr</code>，但日誌進入 <code>/usr/local/<b>spark-1.0-hadoop-1.2</b>/work/app-0546/0/stderr</code>，則無法讀取日誌。</p></td>
    </tr>
  </tbody>
</table>

若要測試您在疑難排解期間所做的變更，您可以在叢集中的工作者節點上部署 *Noisy*，這是可以產生數個日誌事件的範例 Pod。

  1. 將 [CLI 的目標](cs_cli_install.html#cs_cli_configure)設為您要在其中開始產生日誌的叢集。

  2. 建立 `deploy-noisy.yaml` 配置檔。

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

  3. 在叢集的環境定義中執行配置檔。

        ```
                kubectl apply -f noisy.yaml
        ```
        {:pre}

  4. 幾分鐘之後，您可以在 Kibana 儀表板中檢視日誌。若要存取 Kibana 儀表板，請移至下列其中一個 URL，然後選取您建立叢集所在的 {{site.data.keyword.Bluemix_notm}} 帳戶。如果您在建立叢集時指定了空間，請改為移至該空間。
      - 美國南部及美國東部：https://logging.ng.bluemix.net
      - 英國南部：https://logging.eu-gb.bluemix.net
      - 歐盟中部：https://logging.eu-fra.bluemix.net
      - 亞太地區南部：https://logging.au-syd.bluemix.net

<br />


## Kubernetes 儀表板未顯示使用率圖形
{: #cs_dashboard_graphs}

{: tsSymptoms}
存取 Kubernetes 儀表板時，未顯示任何使用率圖形。

{: tsCauses}
有時，在叢集更新或工作者節點重新啟動之後，`kube-dashboard` Pod 並未更新。

{: tsResolve}
刪除 `kube-dashboard` Pod，以強制重新啟動。系統會以 RBAC 原則重建 Pod，存取 heapster 以取得使用率資訊。

  ```
  kubectl delete pod -n kube-system $(kubectl get pod -n kube-system --selector=k8s-app=kubernetes-dashboard -o jsonpath='{.items..metadata.name}')
  ```
  {: pre}

<br />


## 取得協助及支援
{: #ts_getting_help}

叢集仍有問題？
{: shortdesc}

-   若要查看 {{site.data.keyword.Bluemix_notm}} 是否可用，請[檢查 {{site.data.keyword.Bluemix_notm}} 狀態頁面 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/bluemix/support/#status)。
-   將問題張貼到 [{{site.data.keyword.containershort_notm}} Slack ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://ibm-container-service.slack.com)。

如果您的 {{site.data.keyword.Bluemix_notm}} 帳戶未使用 IBM ID，請[要求邀請](https://bxcs-slack-invite.mybluemix.net/)以加入此 Slack。
    {: tip}
-   檢閱討論區，以查看其他使用者是否發生過相同的問題。使用討論區提問時，請標記您的問題，以便 {{site.data.keyword.Bluemix_notm}} 開發團隊能看到它。

    -   如果您在使用 {{site.data.keyword.containershort_notm}} 開發或部署叢集或應用程式時有技術方面的問題，請將問題張貼到 [Stack Overflow ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers)，並使用 `ibm-cloud`、`kubernetes` 及 `containers` 來標記問題。
    -   若為服務及開始使用指示的相關問題，請使用 [IBM developerWorks dW Answers ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) 討論區。請包含 `ibm-cloud` 及 `containers` 標籤。如需使用討論區的詳細資料，請參閱[取得協助](/docs/get-support/howtogetsupport.html#using-avatar)。

-   開立問題單以與 IBM 支援中心聯絡。若要瞭解開立 IBM 支援問題單或是支援層次與問題單嚴重性，請參閱[與支援中心聯絡](/docs/get-support/howtogetsupport.html#getting-customer-support)。

{: tip}
當您報告問題時，請包含您的叢集 ID。若要取得叢集 ID，請執行 `bx cs clusters`。

