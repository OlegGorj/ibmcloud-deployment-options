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



# 로깅 및 모니터링 문제점 해결
{: #cs_troubleshoot_health}

{{site.data.keyword.containerlong}}를 사용할 때 로깅 및 모니터링 관련 문제점을 해결하려면 이러한 방법을 고려하십시오.
{: shortdesc}

더 일반적인 문제점이 있는 경우에는 [클러스터 디버깅](cs_troubleshoot.html)을 시도해 보십시오.
{: tip}

## 로그가 나타나지 않음
{: #cs_no_logs}

{: tsSymptoms}
Kibana 대시보드에 액세스할 때 로그가 표시되지 않습니다.

{: tsResolve}
클러스터 로그가 표시되지 않는 이유와 해당 문제점 해결 단계를 검토하십시오.

<table>
<caption>표시되지 않는 로그 문제점 해결</caption>
  <col width="40%">
  <col width="60%">
  <thead>
    <tr>
      <th>발생 원인</th>
      <th>해결 방법</th>
    </tr>
 </thead>
 <tbody>
  <tr>
    <td>로깅 구성이 설정되지 않았습니다.</td>
    <td>로그가 전송되도록 하려면 로깅 구성을 작성해야 합니다. 작성하려면 <a href="cs_health.html#logging">클러스터 로깅 구성</a>을 참조하십시오.</td>
  </tr>
  <tr>
    <td>클러스터가 <code>Normal</code> 상태가 아닙니다.</td>
    <td>클러스터의 상태를 확인하려면 <a href="cs_troubleshoot.html#debug_clusters">클러스터 디버깅</a>을 참조하십시오.</td>
  </tr>
  <tr>
    <td>로그 스토리지 할당량이 충족되었습니다.</td>
    <td>로그 스토리지 한계를 늘리려면 <a href="/docs/services/CloudLogAnalysis/troubleshooting/error_msgs.html">{{site.data.keyword.loganalysislong_notm}} 문서</a>를 참조하십시오.</td>
  </tr>
  <tr>
    <td>클러스터 작성시 영역을 지정한 경우, 계정 소유자에게 해당 영역에 대한 관리자, 개발자 또는 감사자 권한이 없습니다.</td>
      <td>계정 소유자의 액세스 권한을 변경하려면 다음을 수행하십시오.
      <ol><li>클러스터의 계정 소유자를 찾으려면 <code>bx cs api-key-info &lt;cluster_name_or_ID&gt;</code>를 실행하십시오.</li>
      <li>계정 소유자에게 영역에 대한 관리자, 개발자 또는 감사자 {{site.data.keyword.containershort_notm}} 액세스 권한을 부여하려면 <a href="cs_users.html">클러스터 액세스 관리</a>를 참조하십시오.</li>
      <li>권한이 변경된 후 로깅 토큰을 새로 고치려면 <code>bx cs logging-config-refresh &lt;cluster_name_or_ID&gt;</code>를 실행하십시오.</li></ol></td>
    </tr>
    <tr>
      <td>애플리케이션 로깅 구성의 앱 경로에 symlink가 포함되어 있습니다.</td>
      <td><p>로그를 전송하려면 로깅 구성에 절대 경로를 사용해야 하며, 그렇지 않으면 로그를 읽을 수 없습니다. 경로가 작업자 노드에 마운트되어 있는 경우에는 symlink가 작성되었을 수 있습니다.</p> <p>예: 지정된 경로가 <code>/usr/local/<b>spark</b>/work/app-0546/0/stderr</code>이지만 로그가 <code>/usr/local/<b>spark-1.0-hadoop-1.2</b>/work/app-0546/0/stderr</code>로 이동하는 경우에는 이 로그를 읽을 수 없습니다. </p></td>
    </tr>
  </tbody>
</table>

문제점 해결 중에 변경한 사항을 테스트하려는 경우에는 여러 로그 이벤트를 생성하는 샘플 팟(Pod) *Noisy*를 클러스터의 작업자 노드에 배치할 수 있습니다.

  1. 로그 생성을 시작할 클러스터를 [CLI의 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오.

  2. `deploy-noisy.yaml` 구성 파일을 작성하십시오.

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

  3. 클러스터의 컨텍스트에서 구성 파일을 실행하십시오.

        ```
        kubectl apply -f noisy.yaml
        ```
        {:pre}

  4. 몇 분 뒤에 Kibana 대시보드에 로그가 표시됩니다. Kibana 대시보드에 액세스하려면 다음 URL 중 하나로 이동하여 클러스터를 작성한 {{site.data.keyword.Bluemix_notm}} 계정을 선택하십시오. 클러스터 작성 시 영역을 지정한 경우 대신 해당 영역으로 이동하십시오.
      - 미국 남부 및 미국 동부: https://logging.ng.bluemix.net
      - 영국 남부: https://logging.eu-gb.bluemix.net
      - 중앙 유럽: https://logging.eu-fra.bluemix.net
      - AP 남부: https://logging.au-syd.bluemix.net

<br />


## Kubernetes 대시보드에 사용률 그래프가 표시되지 않음
{: #cs_dashboard_graphs}

{: tsSymptoms}
Kubernetes 대시보드에 액세스할 때 사용률 그래프는 표시되지 않습니다.

{: tsCauses}
때때로 클러스터 업데이트 또는 작업자 노드 재부팅 후에 `kube-dashboard` 팟(Pod)이 업데이트되지 않습니다.

{: tsResolve}
강제로 다시 시작하려면 `kube-dashboard` 팟(Pod)을 삭제하십시오. 사용률 정보에 대한 heapster에 액세스하기 위해 RBAC 정책으로 팟(Pod)이 다시 작성됩니다.

  ```
    kubectl delete pod -n kube-system $(kubectl get pod -n kube-system --selector=k8s-app=kubernetes-dashboard -o jsonpath='{.items..metadata.name}')
  ```
  {: pre}

<br />


## 도움 및 지원 받기
{: #ts_getting_help}

클러스터에 여전히 문제점이 있습니까?
{: shortdesc}

-   {{site.data.keyword.Bluemix_notm}}가 사용 가능한지 확인하려면 [{{site.data.keyword.Bluemix_notm}} 상태 페이지![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")를 참조](https://developer.ibm.com/bluemix/support/#status)하십시오.
-   [{{site.data.keyword.containershort_notm}} Slack ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://ibm-container-service.slack.com)에 질문을 게시하십시오.

{{site.data.keyword.Bluemix_notm}} 계정에 대해 IBM ID를 사용 중이 아닌 경우에는 이 Slack에 대한 [초대를 요청](https://bxcs-slack-invite.mybluemix.net/)하십시오.
    {: tip}
-   포럼을 검토하여 다른 사용자에게도 동일한 문제가 발생하는지 여부를 확인하십시오. 포럼을 사용하여 질문을 할 때는 {{site.data.keyword.Bluemix_notm}} 개발 팀이 볼 수 있도록 질문에 태그를 지정하십시오.

    -   {{site.data.keyword.containershort_notm}}로 클러스터 또는 앱을 개발하거나 배치하는 데 대한 기술적 질문이 있으면 [스택 오버플로우![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers)에 질문을 게시하고 질문에 `ibm-cloud`, `kubernetes` 및 `containers` 태그를 지정하십시오.
    -   시작하기 지시사항과 서비스에 대한 질문은 [IBM developerWorks dW 응답![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) 포럼을 사용하십시오. `ibm-cloud` 및 `containers` 태그를 포함하십시오.
    포럼 사용에 대한 세부사항은 [도움 받기](/docs/get-support/howtogetsupport.html#using-avatar)를 참조하십시오.

-   티켓을 열어 IBM 지원 센터에 문의하십시오. IBM 지원 티켓 열기 또는 지원 레벨 및 티켓 심각도에 대해 알아보려면 [지원 문의](/docs/get-support/howtogetsupport.html#getting-customer-support)를 참조하십시오.

{: tip}
문제를 보고할 때 클러스터 ID를 포함시키십시오. 클러스터 ID를 가져오려면 `bx cs clusters`를 실행하십시오.

