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


# 로깅 및 모니터링
{: #health}

{{site.data.keyword.containerlong}}의 로깅 및 모니터링을 설정하면 문제를 해결하고 Kubernetes 클러스터 및 앱의 상태와 성능을 향상시킬 수 있습니다.
{: shortdesc}


## 클러스터 및 앱 로그 전달 구성
{: #logging}

{{site.data.keyword.containershort_notm}}의 표준 Kubernetes 클러스터를 사용하면 다른 소스의 로그를 {{site.data.keyword.loganalysislong_notm}}, 외부 syslog 서버(또는 둘 다)로 전달할 수 있습니다.
{: shortdesc}

하나의 소스의 로그를 두 콜렉터 서버로 전달하려면 두 개의 로깅 구성을 작성해야 합니다.
{: tip}

여러 가지 로그 소스에 대한 정보를 보려면 다음 표를 확인하십시오.

<table>
<caption>로그 소스</caption>
  <thead>
    <tr>
      <th>로그 소스</th>
      <th>특성</th>
      <th>로그 경로</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>container</code></td>
      <td>Kubernetes 클러스터에서 실행되는 사용자의 컨테이너에 대한 로그입니다. STDOUT 또는 STDERR에 로깅된 모든 항목입니다.</td>
      <td> </td>
    </tr>
    <tr>
      <td><code>application</code></td>
      <td>Kubernetes 클러스터에서 실행되는 고유 애플리케이션의 로그입니다.</td>
      <td><p>경로를 설정할 수 있습니다. 로그를 전송하려면 로깅 구성에 절대 경로를 사용해야 하며, 그렇지 않으면 로그를 읽을 수 없습니다. 경로가 작업자 노드에 마운트되어 있는 경우에는 symlink가 작성되었을 수 있습니다.</p>
      <p>예: 지정된 경로가 <code>/usr/local/<b>spark</b>/work/app-0546/0/stderr</code>이지만 로그가 실제로는 <code>/usr/local/<b>spark-1.0-hadoop-1.2</b>/work/app-0546/0/stderr</code>로 이동하는 경우에는 이 로그를 읽을 수 없습니다.</p></td>
    </tr>
    <tr>
      <td><code>worker</code></td>
      <td>Kubernetes 클러스터 내 가상 머신 작업자 노드의 로그입니다.</td>
      <td><code>/var/log/syslog</code>, <code>/var/log/auth.log</code></td>
    </tr>
    <tr>
      <td><code>kubernetes</code></td>
      <td>Kubernetes 시스템 컴포넌트의 로그입니다.</td>
      <td><code>/var/log/kubelet.log</code>, <code>/var/log/kube-proxy.log</code>, <code>/var/log/event-exporter/&ast;.log</code></td>
    </tr>
    <tr>
      <td><code>ingress</code></td>
      <td>클러스터로 수신되는 네트워크 트래픽을 관리하는 Ingress 애플리케이션 로드 밸런서의 로그입니다.</td>
      <td><code>/var/log/alb/ids/&ast;.log</code>, <code>/var/log/alb/ids/&ast;.err</code>, <code>/var/log/alb/customerlogs/&ast;.log</code>, <code>/var/log/alb/customerlogs/&ast;.err</code></td>
    </tr>
    <tr>
      <td><code>kube-audit</code></td>
      <td>Kubernetes API 서버의 로그입니다.</td>
      <td> </td>
    </tr>
  </tbody>
</table>

계정 레벨에서 로깅을 사용으로 설정하거나 애플리케이션 로깅을 구성하려면 CLI를 사용하십시오.
{: tip}


### GUI에서 로그 전달 사용
{: #enable-forwarding-ui}

{{site.data.keyword.containershort_notm}} 대시보드에서 로깅 구성을 설정할 수 있습니다. 이 프로세스를 완료하는 데 몇 분이 걸릴 수 있으므로 로그가 즉시 표시되지 않으면 몇 분 동안 기다린 후 다시 확인하십시오.

1. 대시보드의 **개요** 탭으로 이동하십시오.
2. 로그를 전달할 Cloud Foundry 조직 및 영역을 선택하십시오. 대시보드에서 로그 전달을 구성하면 로그가 클러스터의 기본 {{site.data.keyword.loganalysisshort_notm}} 엔드포인트에 전송됩니다. 로그를 외부 서버 또는 다른 {{site.data.keyword.loganalysisshort_notm}} 엔드포인트에 전달하려면 CLI를 사용하여 로깅을 구성하십시오.
3. 로그를 전달할 로그 소스를 선택하십시오.

    애플리케이션 로깅 또는 특정 컨테이너 네임스페이스를 구성하려면 CLI를 사용하여 로깅 구성을 설정하십시오.
    {: tip}
4. **작성**을 클릭하십시오.

### CLI에서 로그 전달 사용
{: #enable-forwarding}

클러스터 로깅에 대한 구성을 작성할 수 있습니다. 플래그를 사용하여 서로 다른 로그 소스를 구별할 수 있습니다. 구성 옵션의 전체 목록은 [CLI 참조](cs_cli_reference.html#logging_commands)에서 검토할 수 있습니다.

**시작하기 전에**

1. 권한을 확인하십시오. 클러스터 또는 로깅 구성을 작성할 때 영역을 지정한 경우 계정 소유자 및 {{site.data.keyword.containershort_notm}} 키 소유자 모두에 해당 영역의 관리자, 개발자 또는 감사자 권한이 필요합니다.
  * {{site.data.keyword.containershort_notm}} 키 소유자가 누구인지 모르는 경우 다음 명령을 실행하십시오.
      ```
      bx cs api-key-info <cluster_name>
      ```
      {: pre}
  * 권한에 대한 변경사항을 즉시 적용하려면 다음 명령을 실행하십시오.
      ```
      bx cs logging-config-refresh <cluster_name>
      ```
      {: pre}

  {{site.data.keyword.containershort_notm}} 액세스 정책 및 권한 변경에 대한 자세한 정보는 [클러스터 액세스 관리](cs_users.html#access_policies)를 참조하십시오.
  {: tip}

2. 로그 소스가 있는 클러스터를 [CLI의 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오.

  데디케이티드 계정을 사용 중인 경우 로그 전달을 사용으로 설정하려면 퍼블릭 {{site.data.keyword.cloud_notm}} 엔드포인트에 로그인하고 퍼블릭 조직 및 영역을 대상으로 지정해야 합니다.
  {: tip}

3. 로그를 syslog에 전달하려면 다음 두 가지 방법 중 하나를 사용하여 syslog 프로토콜을 허용하는 서버를 설정하십시오.
  * 자체 서버를 설정하여 관리하거나 제공자가 관리하도록 하십시오. 제공자가 사용자 대신 서버를 관리하는 경우 로깅 제공자로부터 로깅 엔드포인트를 가져오십시오. syslog 서버는 UDP 프로토콜을 허용해야 합니다.
  * 컨테이너에서 syslog를 실행하십시오. 예를 들어, 이 [배치 .yaml 파일 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/IBM-Cloud/kube-samples/blob/master/deploy-apps-clusters/deploy-syslog-from-kube.yaml)을 사용하여 Kubernetes 클러스터에서 컨테이너를 실행하는 Docker 공용 이미지를 페치할 수 있습니다. 이미지는 공용 클러스터 IP 주소에 포트 `514`를 공개하고 이 공용 클러스터 IP 주소를 사용하여 syslog 호스트를 구성합니다.

    rsyslog 서버가 실행 중인 `etc/rsyslog.conf` 파일의 맨 위에 다음 코드를 추가하여 로그를 유효한 JSON으로 표시하도록 syslog 접두부를 제거할 수 있습니다. </br>
    ```$template customFormat,"%msg%\n"
    $ActionFileDefaultTemplate customFormat
    ```
    {: tip}


**로그 전달**

1. 로그 전달 구성을 작성하십시오.
  ```
  bx cs logging-config-create <cluster_name_or_ID> --logsource <log_source> --namespace <kubernetes_namespace> --hostname <log_server_hostname_or_IP> --port <log_server_port> --type <type> --app-containers <containers> --app-paths <paths_to_logs> --syslog-protocol <protocol> --skip-validation
  ```
  {: pre}

    * 기본 네임스페이스 및 출력에 대한 컨테이너 로깅 구성 예:
      ```
      bx cs logging-config-create cluster1 --namespace default
      Creating logging configuration for container logs in cluster cluster1...
      OK
      Id                                     Source      Namespace   Host                                 Port    Org   Space   Protocol   Application Containers   Paths
      af7d1ff4-33e6-4275-8167-b52eb3c5f0ee   container   default     ingest-au-syd.logging.bluemix.net✣  9091✣   -     -       ibm        -                        -

      ✣ Indicates the default endpoint for the {{site.data.keyword.loganalysislong_notm}} service.

      ```
      {: screen}

    * 애플리케이션 로깅 구성 및 출력 예:
      ```
      bx cs logging-config-create cluster2 --logsource application --app-paths '/var/log/apps.log' --app-containers 'container1,container2,container3'
      Creating logging configuration for application logs in cluster cluster2...
      OK
      Id                                     Source        Namespace   Host                                    Port    Org   Space   Protocol   Application   Containers   Paths
      aa2b415e-3158-48c9-94cf-f8b298a5ae39   application   -           ingest.logging.stage1.ng.bluemix.net✣  9091✣   -     -       ibm        container1,container2,container3   /var/log/apps.log
      ```
      {: screen}

      컨테이너에서 실행되는 앱 중에서 STDOUT 또는 STDERR에 로그를 기록하도록 구성할 수 없는 앱이 있는 경우에는 앱 로그 파일로부터 로그를 전달하도록 하는 로깅 구성을 작성할 수 있습니다.
      {: tip}

  <table>
  <caption>이 명령의 컴포넌트 이해</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 이 명령의 컴포넌트 이해</th>
    </thead>
    <tbody>
      <tr>
        <td><code><em>&lt;cluster_name_or_ID&gt;</em></code></td>
        <td>클러스터의 이름 또는 ID입니다.</td>
      </tr>
      <tr>
        <td><code><em>&lt;log_source&gt;</em></code></td>
        <td>로그를 전달할 소스입니다. 허용되는 값은 <code>container</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code>, <code>ingress</code> 및 <code>kube-audit</code>입니다. </td>
      </tr>
      <tr>
        <td><code><em>&lt;kubernetes_namespace&gt;</em></code></td>
        <td>선택사항: 로그를 전달할 Kubernetes 네임스페이스입니다. <code>ibm-system</code> 및 <code>kube-system</code> Kubernetes 네임스페이스의 경우 로그 전달이 지원되지 않습니다. 이 값은 <code>container</code> 로그 소스에 대해서만 유효합니다. 네임스페이스를 지정하지 않으면 컨테이너의 모든 네임스페이스가 이 구성을 사용합니다.</td>
      </tr>
      <tr>
        <td><code><em>&lt;hostname_or_ingestion_URL&gt;</em></code></td>
        <td><p>{{site.data.keyword.loganalysisshort_notm}}의 경우 [유입 URL](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls)을 사용하십시오. 유입 URL을 지정하지 않으면 클러스터가 작성된 지역의 엔드포인트가 사용됩니다.</p>
        <p>syslog의 경우 로그 콜렉터 서비스의 호스트 이름 또는 IP 주소를 지정하십시오.</p></td>
      </tr>
      <tr>
        <td><code><em>&lt;port&gt;</em></code></td>
        <td>유입 포트입니다. 포트를 지정하지 않으면 표준 포트 <code>9091</code>이 사용됩니다.
        <p>syslog의 경우 로그 콜렉터 서버의 포트를 지정하십시오. 포트를 지정하지 않으면 표준 포트 <code>514</code>가 사용됩니다.</td>
      </tr>
      <tr>
        <td><code><em>&lt;cluster_space&gt;</em></code></td>
        <td>선택사항: 로그를 전송할 Cloud Foundry 영역의 이름입니다. 로그를 {{site.data.keyword.loganalysisshort_notm}}로 전달하는 경우 영역 및 조직이 유입 지점에서 지정됩니다. 영역을 지정하지 않으면 로그는 계정 레벨로 전송됩니다.</td>
      </tr>
      <tr>
        <td><code><em>&lt;cluster_org&gt;</em></code></td>
        <td>영역이 있는 Cloud Foundry 조직의 이름입니다. 영역을 지정한 경우 이 값은 필수입니다.</td>
      </tr>
      <tr>
        <td><code><em>&lt;type&gt;</em></code></td>
        <td>로그를 전달할 위치입니다. 옵션은 <code>ibm</code>(로그를 {{site.data.keyword.loganalysisshort_notm}}로 전달) 및 <code>syslog</code>(로그를 외부 서버로 전달)입니다.</td>
      </tr>
      <tr>
        <td><code><em>&lt;paths_to_logs&gt;</em></code></td>
        <td>앱이 로그를 기록하는 컨테이너 상의 경로입니다. 소스 유형이 <code>application</code>인 로그를 전달하려면 경로를 제공해야 합니다. 두 개 이상의 경로를 지정하려면 쉼표로 구분된 목록을 사용하십시오. 예: <code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></td>
      </tr>
      <tr>
        <td><code><em>&lt;containers&gt;</em></code></td>
        <td>선택사항: 앱에서 로그를 전달하기 위해 앱이 포함된 컨테이너의 이름을 지정할 수 있습니다. 쉼표로 구분된 목록을 사용하여 두 개 이상의 컨테이너를 지정할 수 있습니다. 컨테이너가 지정되지 않은 경우 로그는 사용자가 제공한 경로가 포함된 모든 컨테이너에서 전달됩니다.</td>
      </tr>
      <tr>
        <td><code><em>&lt;protocol&gt;</em></code></td>
        <td>로깅 유형이 <code>syslog</code>인 경우 전송 계층은 프로토콜입니다. 지원되는 값은 <code>TCP</code> 및 기본값인 <code>UDP</code>입니다. <code>udp</code> 프로토콜을 사용하여 rsyslog 서버에 전달하는 경우 1KB를 초과하는 로그는 잘립니다.</td>
      </tr>
      <tr>
        <td><code><em>--skip-validation</em></code></td>
        <td>선택사항: 조직 및 영역 이름을 지정할 때 이러한 항목의 유효성 검증을 건너뜁니다. 유효성 검증을 건너뛰면 처리 시간이 줄어들지만 올바르지 않은 로깅 구성이 로그를 올바르게 전달하지 않습니다.</td>
      </tr>
    </tbody>
  </table>

2. 다음 방법 중 하나를 사용하여 구성이 올바른지 확인하십시오.

    * 클러스터의 모든 로깅 구성을 나열하려면 다음을 실행하십시오.
      ```
      bx cs logging-config-get <cluster_name_or_ID>
      ```
      {: pre}

      출력 예:

      ```
      Id                                    Source       Namespace     Host                          Port   Org      Space      Protocol     Paths
      f4bc77c0-ee7d-422d-aabf-a4e6b977264e  kubernetes   -             172.30.xxx.xxx                5514   -        -          syslog       /var/log/kubelet.log,/var/log/kube-proxy.log
      5bd9c609-13c8-4c48-9d6e-3a6664c825a9  application  -             ingest.logging.ng.bluemix.net 9091   my_org   my_space   ibm          /var/log/apps/**/*.log,/var/log/apps/**/*.err
      8a284f1a-451c-4c48-b1b4-a4e6b977264e  containers   my-namespace  myhostname.common             5514   -        -          syslog       -
      ```
      {: screen}

    * 로그 소스의 한 가지 유형에 대한 로깅 구성을 나열하려면 다음을 실행하십시오.
      ```
      bx cs logging-config-get <cluster_name_or_ID> --logsource worker
      ```
      {: pre}

      출력 예:

      ```
      Id                                    Source    Namespace   Host                            Port   Org    Space     Protocol    Paths
      f4bc77c0-ee7d-422d-aabf-a4e6b977264e  worker    -           ingest.logging.ng.bluemix.net   9091   -      -         ibm         /var/log/syslog,/var/log/auth.log
      5bd9c609-13c8-4c48-9d6e-3a6664c825a9  worker    -           172.30.xxx.xxx                  5514   -      -         syslog      /var/log/syslog,/var/log/auth.log
      ```
      {: screen}

## 로그 전달 업데이트
{: #updating-forwarding}

1. 로그 전달 구성을 업데이트하십시오.
    ```
    bx cs logging-config-update <cluster_name_or_ID> <log_config_id> --namespace <namespace> --type <log_type> --logsource <source> --hostname <hostname_or_ingestion_URL> --port <port> --space <cluster_space> --org <cluster_org> --app-containers <containers> --app-paths <paths_to_logs>
    ```
    {: pre}

  <table>
  <caption>이 명령의 컴포넌트 이해</caption>
  <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 이 명령의 컴포넌트 이해</th>
  </thead>
  <tbody>
    <tr>
      <td><code><em>&lt;cluster_name_or_ID&gt;</em></code></td>
      <td>클러스터의 이름 또는 ID입니다.</td>
    </tr>
    <tr>
      <td><code><em>&lt;log_config_id&gt;</em></code></td>
      <td>업데이트할 구성의 ID입니다.</td>
    </tr>
    <tr>
      <td><code><em>&lt;namespace&gt;</em></code></td>
      <td>선택사항: 로그를 전달할 Kubernetes 네임스페이스입니다. <code>ibm-system</code> 및 <code>kube-system</code> Kubernetes 네임스페이스의 경우 로그 전달이 지원되지 않습니다. 이 값은 <code>container</code> 로그 소스에 대해서만 유효합니다. 네임스페이스를 지정하지 않으면 컨테이너의 모든 네임스페이스가 이 구성을 사용합니다.</td>
    </tr>
    <tr>
      <td><code><em>&lt;log_type&gt;</em></code></td>
      <td>로그를 전달할 위치입니다. 옵션은 <code>ibm</code>(로그를 {{site.data.keyword.loganalysisshort_notm}}로 전달) 및 <code>syslog</code>(로그를 외부 서버로 전달)입니다.</td>
    </tr>
    <tr>
      <td><code><em>&lt;hostname_or_ingestion_URL&gt;</em></code></td>
      <td><p>{{site.data.keyword.loganalysisshort_notm}}의 경우 [유입 URL](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls)을 사용하십시오. 유입 URL을 지정하지 않으면 클러스터가 작성된 지역의 엔드포인트가 사용됩니다.</p>
      <p>syslog의 경우 로그 콜렉터 서비스의 호스트 이름 또는 IP 주소를 지정하십시오.</p></td>
    </tr>
    <tr>
      <td><code><em>&lt;port&gt;</em></code></td>
      <td>유입 포트입니다. 포트를 지정하지 않으면 표준 포트 <code>9091</code>이 사용됩니다.
      <p>syslog의 경우 로그 콜렉터 서버의 포트를 지정하십시오. 포트를 지정하지 않으면 표준 포트 <code>514</code>가 사용됩니다.</td>
    </tr>
    <tr>
      <td><code><em>&lt;cluster_space&gt;</em></code></td>
      <td>선택사항: 로그를 전송할 Cloud Foundry 영역의 이름입니다. 로그를 {{site.data.keyword.loganalysisshort_notm}}로 전달하는 경우 영역 및 조직이 유입 지점에서 지정됩니다. 영역을 지정하지 않으면 로그는 계정 레벨로 전송됩니다.</td>
    </tr>
    <tr>
      <td><code><em>&lt;cluster_org&gt;</em></code></td>
      <td>영역이 있는 Cloud Foundry 조직의 이름입니다. 영역을 지정한 경우 이 값은 필수입니다.</td>
    </tr>
    <tr>
      <td><code><em>&lt;paths_to_logs&gt;</em></code></td>
      <td>앱이 로그를 기록하는 컨테이너 상의 경로입니다. 소스 유형이 <code>application</code>인 로그를 전달하려면 경로를 제공해야 합니다. 두 개 이상의 경로를 지정하려면 쉼표로 구분된 목록을 사용하십시오. 예: <code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></td>
    </tr>
    <tr>
      <td><code><em>&lt;containers&gt;</em></code></td>
      <td>선택사항: 앱에서 로그를 전달하기 위해 앱이 포함된 컨테이너의 이름을 지정할 수 있습니다. 쉼표로 구분된 목록을 사용하여 두 개 이상의 컨테이너를 지정할 수 있습니다. 컨테이너가 지정되지 않은 경우 로그는 사용자가 제공한 경로가 포함된 모든 컨테이너에서 전달됩니다.</td>
    </tr>
  </tbody>
  </table>

<br />



## 로그 필터링
{: #filter-logs}

일정 기간 동안의 특정 로그를 필터링하여 전달하는 로그를 선택할 수 있습니다.

1. 로깅 필터를 작성하십시오.
  ```
  bx cs logging-filter-create <cluster_name_or_ID> --type <log_type> --logging-configs <configs> --namespace <kubernetes_namespace> --container <container_name> --level <logging_level> --regex-message <message>
  ```
  {: pre}
  <table>
  <caption>이 명령의 컴포넌트 이해</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 이 명령의 컴포넌트 이해</th>
    </thead>
    <tbody>
      <tr>
        <td>&lt;cluster_name_or_ID&gt;</td>
        <td>필수: 로깅 필터를 작성할 클러스터의 이름 또는 ID입니다.</td>
      </tr>
      <tr>
        <td><code>&lt;log_type&gt;</code></td>
        <td>필터를 적용할 로그의 유형입니다. 현재는 <code>all</code>, <code>container</code> 및 <code>host</code>가 지원됩니다.</td>
      </tr>
      <tr>
        <td><code>&lt;configs&gt;</code></td>
        <td>선택사항: 로깅 구성 ID의 쉼표로 구분된 목록입니다. 제공되지 않으면 필터에 전달된 모든 클러스터 로깅 구성에 필터가 적용됩니다. 명령과 함께 <code>--show-matching-configs</code> 플래그를 사용하여 필터와 일치하는 로그 구성을 볼 수 있습니다.</td>
      </tr>
      <tr>
        <td><code>&lt;kubernetes_namespace&gt;</code></td>
        <td>선택사항: 로그를 전달할 Kubernetes 네임스페이스입니다. 이 플래그는 로그 유형 <code>container</code>를 사용하는 경우에만 적용됩니다.</td>
      </tr>
      <tr>
        <td><code>&lt;container_name&gt;</code></td>
        <td>선택사항: 로그를 필터링할 컨테이너의 이름입니다.</td>
      </tr>
      <tr>
        <td><code>&lt;logging_level&gt;</code></td>
        <td>선택사항: 지정된 레벨 이하의 로그를 필터링합니다. 허용 가능한 값은 규범적 순서대로 <code>fatal</code>, <code>error</code>, <code>warn/warning</code>, <code>info</code>, <code>debug</code> 및 <code>trace</code>입니다. 예를 들어, <code>info</code> 레벨에서 로그를 필터링한 경우에는 <code>debug</code> 및 <code>trace</code> 또한 필터링됩니다. **참고**: 로그 메시지가 JSON 형식이며 level 필드를 포함하는 경우에만 이 플래그를 사용할 수 있습니다. 메시지를 JSON 형식으로 표시하려면 명령에 <code>--json</code> 플래그를 추가하십시오.</td>
      </tr>
      <tr>
        <td><code>&lt;message&gt;</code></td>
        <td>선택사항: 정규식으로 작성된 지정된 메시지가 포함되어 있는 로그를 필터링합니다.</td>
      </tr>
    </tbody>
  </table>

2. 작성한 로그 필터를 보십시오.

  ```
  bx cs logging-filter-get <cluster_name_or_ID> --id <filter_ID> --show-matching-configs
  ```
  {: pre}
  <table>
  <caption>이 명령의 컴포넌트 이해</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 이 명령의 컴포넌트 이해</th>
    </thead>
    <tbody>
      <tr>
        <td>&lt;cluster_name_or_ID&gt;</td>
        <td>필수: 로깅 필터를 작성할 클러스터의 이름 또는 ID입니다.</td>
      </tr>
      <tr>
        <td><code>&lt;filter_ID&gt;</code></td>
        <td>선택사항: 보려는 로그 필터의 ID입니다.</td>
      </tr>
      <tr>
        <td><code>--show-matching-configs</code></td>
        <td>각 필터가 적용되는 로깅 구성을 보여줍니다.</td>
      </tr>
    </tbody>
  </table>

3. 작성한 로그 필터를 업데이트하십시오.
  ```
  bx cs logging-filter-update <cluster_name_or_ID> --id <filter_ID> --type <log_type> --logging-configs <configs> --namespace <kubernetes_namespace --container <container_name> --level <logging_level> --regex-message <message>
  ```
  {: pre}
  <table>
  <caption>이 명령의 컴포넌트 이해</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 이 명령의 컴포넌트 이해</th>
    </thead>
    <tbody>
      <tr>
        <td>&lt;cluster_name_or_ID&gt;</td>
        <td>필수: 로깅 필터를 업데이트할 클러스터의 이름 또는 ID입니다.</td>
      </tr>
      <tr>
        <td><code>&lt;filter_ID&gt;</code></td>
        <td>업데이트하려는 로그 필터의 ID입니다.</td>
      </tr>
      <tr>
        <td><code><&lt;log_type&gt;</code></td>
        <td>필터를 적용할 로그의 유형입니다. 현재는 <code>all</code>, <code>container</code> 및 <code>host</code>가 지원됩니다.</td>
      </tr>
      <tr>
        <td><code>&lt;configs&gt;</code></td>
        <td>선택사항: 필터를 적용할 모든 로깅 구성 ID의 쉼표로 구분된 목록입니다. 제공되지 않으면 필터에 전달된 모든 클러스터 로깅 구성에 필터가 적용됩니다. <code>bx cs logging-filter-get</code> 명령과 함께 <code>--show-matching-configs</code> 플래그를 사용하여 필터와 일치하는 로그 구성을 볼 수 있습니다.</td>
      </tr>
      <tr>
        <td><code>&lt;kubernetes_namespace&gt;</code></td>
        <td>선택사항: 로그를 전달할 Kubernetes 네임스페이스입니다. 이 플래그는 로그 유형 <code>container</code>를 사용하는 경우에만 적용됩니다.</td>
      </tr>
      <tr>
        <td><code>&lt;container_name&gt;</code></td>
        <td>선택사항: 로그를 필터링할 컨테이너의 이름입니다. 이 플래그는 로그 유형 <code>container</code>를 사용하는 경우에만 적용됩니다.</td>
      </tr>
      <tr>
        <td><code>&lt;logging_level&gt;</code></td>
        <td>선택사항: 지정된 레벨 이하의 로그를 필터링합니다. 허용 가능한 값은 규범적 순서대로 <code>fatal</code>, <code>error</code>, <code>warn/warning</code>, <code>info</code>, <code>debug</code> 및 <code>trace</code>입니다. 예를 들어, <code>info</code> 레벨에서 로그를 필터링한 경우에는 <code>debug</code> 및 <code>trace</code> 또한 필터링됩니다. **참고**: 로그 메시지가 JSON 형식이며 level 필드를 포함하는 경우에만 이 플래그를 사용할 수 있습니다.</td>
      </tr>
      <tr>
        <td><code>&lt;message&gt;</code></td>
        <td>선택사항: 정규식으로 작성된 지정된 메시지가 포함되어 있는 로그를 필터링합니다.</td>
      </tr>
    </tbody>
  </table>

4. 작성한 로그 필터를 삭제하십시오.

  ```
  bx cs logging-filter-rm <cluster_name_or_ID> --id <filter_ID> [--all]
  ```
  {: pre}
  <table>
  <caption>이 명령의 컴포넌트 이해</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 이 명령의 컴포넌트 이해</th>
    </thead>
    <tbody>
      <tr>
        <td><code>&lt;cluster_name_or_ID&gt;</code></td>
        <td>필수: 로깅 필터를 삭제할 클러스터의 이름 또는 ID입니다.</td>
      </tr>
      <tr>
        <td><code>&lt;filter_ID&gt;</code></td>
        <td>선택사항: 제거할 로그 필터의 ID입니다.</td>
      </tr>
      <tr>
        <td><code>--all</code></td>
        <td>선택사항: 로그 전달 필터를 모두 삭제합니다.</td>
      </tr>
    </tbody>
  </table>

<br />




## 로그 보기
{: #view_logs}

클러스터 및 컨테이너에 대한 로그를 보기 위해 표준 Kubernetes 및 Docker 로깅 기능으로 사용할 수 있습니다.
{:shortdesc}

### {{site.data.keyword.loganalysislong_notm}}
{: #view_logs_k8s}

Kibana 대시보드를 통해 {{site.data.keyword.loganalysislong_notm}}에 전달한 로그를 볼 수 있습니다.
{: shortdesc}

구성 파일을 작성하는 데 기본값을 사용한 경우 클러스터가 작성된 계정 또는 조직 및 영역에서 로그를 찾을 수 있습니다. 구성 파일에서 조직 및 영역을 지정한 경우 해당 영역에서 로그를 찾을 수 있습니다. 로깅에 대한 자세한 정보는 [{{site.data.keyword.containershort_notm}}에 대한 로깅](/docs/services/CloudLogAnalysis/containers/containers_kubernetes.html#containers_kubernetes)을 참조하십시오.

Kibana 대시보드에 액세스하려면 다음 URL 중 하나로 이동하여 클러스터에 대한 로그 전달을 구성한 {{site.data.keyword.Bluemix_notm}} 계정 또는 영역을 선택하십시오.
- 미국 남부 및 미국 동부: https://logging.ng.bluemix.net
- 영국 남부: https://logging.eu-gb.bluemix.net
- 중앙 유럽: https://logging.eu-fra.bluemix.net
- AP 남부: https://logging.au-syd.bluemix.net

로그 보기에 대한 자세한 정보는 [웹 브라우저에서 Kibana로 이동](/docs/services/CloudLogAnalysis/kibana/launch.html#launch_Kibana_from_browser)을 참조하십시오.

### Docker 로그
{: #view_logs_docker}

기본 제공 Docker 로깅 기능을 활용하여 표준 STDOUT 및 STDERR 출력 스트림에서 활동을 검토할 수 있습니다. 자세한 정보는 [Kubernetes 클러스터에서 실행되는 컨테이너에 대한 컨테이너 로그 보기](/docs/services/CloudLogAnalysis/containers/containers_kubernetes.html#containers_kubernetes)를 참조하십시오.

<br />



## 로그 전달 중지
{: #log_sources_delete}

클러스터에 대한 하나 또는 모든 로깅 구성의 로그 전달을 중지할 수 있습니다.
{: shortdesc}

1. 로그 소스가 있는 클러스터를 [CLI의 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오.

2. 로깅 구성을 삭제하십시오.
<ul>
<li>하나의 로깅 구성을 삭제하려면 다음을 수행하십시오.</br>
  <pre><code>bx cs logging-config-rm &lt;cluster_name_or_ID&gt; --id &lt;log_config_ID&gt;</pre></code>
  <table>
  <caption>이 명령의 컴포넌트 이해</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 이 명령의 컴포넌트 이해</th>
    </thead>
    <tbody>
      <tr>
        <td><code><em>&lt;cluster_name_or_ID&gt;</em></code></td>
        <td>로깅 구성이 있는 클러스터의 이름입니다.</td>
      </tr>
      <tr>
        <td><code><em>&lt;log_config_ID&gt;</em></code></td>
        <td>로그 소스 구성의 ID입니다.</td>
      </tr>
    </tbody>
  </table></li>
<li>모든 로깅 구성을 삭제하려면 다음을 수행하십시오.</br>
  <pre><code>bx cs logging-config-rm <my_cluster> --all</pre></code></li>
</ul>

<br />



## Kubernetes API 감사 로그의 로그 전달 구성
{: #api_forward}

Kubernetes가 API 서버를 통해 전달된 모든 이벤트를 자동으로 감사합니다. 이벤트를 {{site.data.keyword.loganalysisshort_notm}} 또는 외부 서버로 전달할 수 있습니다.
{: shortdesc}


Kubernetes 감사 로그에 대한 자세한 정보는 Kubernetes 문서에서 <a href="https://kubernetes.io/docs/tasks/debug-application-cluster/audit/" target="blank">감사 주제<img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 참조하십시오.

* Kubernetes API 감사 로그의 전달은 Kubernetes 버전 1.7 이상에서만 지원됩니다.
* 현재 이 로깅 구성의 모든 클러스터에 대해 기본 감사 정책이 사용됩니다.
* 현재는 필터가 지원되지 않습니다.
* 클러스터당 하나의 `kube-audit` 구성만 있을 수 있지만 로깅 구성과 웹훅을 작성하여 로그를 {{site.data.keyword.loganalysisshort_notm}} 및 외부 서버로 전달할 수 있습니다.
{: tip}


### 감사 로그를 {{site.data.keyword.loganalysisshort_notm}}에 전송
{: #audit_enable_loganalysis}

Kubernetes API 서버 감사 로그를 {{site.data.keyword.loganalysisshort_notm}}로 전달할 수 있습니다.

**시작하기 전에**

1. 권한을 확인하십시오. 클러스터 또는 로깅 구성을 작성할 때 영역을 지정한 경우 계정 소유자 및 {{site.data.keyword.containershort_notm}} 키 소유자 모두에 해당 영역의 관리자, 개발자 또는 감사자 권한이 필요합니다.

2. API 서버 감사 로그를 수집할 클러스터를 [CLI의 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오. **참고**: 데디케이티드 계정을 사용 중인 경우 로그 전달을 사용으로 설정하려면 퍼블릭 {{site.data.keyword.cloud_notm}} 엔드포인트에 로그인하고 공용 조직 및 영역을 대상으로 지정해야 합니다.

**로그 전달**

1. 로깅 구성을 작성합니다.

    ```
    bx cs logging-config-create <cluster_name_or_ID> --logsource kube-audit --space <cluster_space> --org <cluster_org> --hostname <ingestion_URL> --type ibm
    ```
    {: pre}

    예제 명령 및 출력:

    ```
    bx cs logging-config-create myCluster --logsource kube-audit
    Creating logging configuration for kube-audit logs in cluster myCluster...
    OK
    Id                                     Source      Namespace   Host                                 Port    Org   Space   Protocol   Application Containers   Paths
    14ca6a0c-5bc8-499a-b1bd-cedcf40ab850   kube-audit  -           ingest-au-syd.logging.bluemix.net✣   9091✣   -     -       ibm        -                        -

    ✣ Indicates the default endpoint for the {{site.data.keyword.loganalysisshort_notm}} service.

    ```
    {: screen}

    <table>
    <caption>이 명령의 컴포넌트 이해</caption>
      <thead>
        <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 이 명령의 컴포넌트 이해</th>
      </thead>
      <tbody>
        <tr>
          <td><code><em>&lt;cluster_name_or_ID&gt;</em></code></td>
          <td>클러스터의 이름 또는 ID입니다.</td>
        </tr>
        <tr>
          <td><code><em>&lt;ingestion_URL&gt;</em></code></td>
          <td>로그를 전달할 엔드포인트입니다. [유입 URL](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls)을 지정하지 않으면 클러스터를 작성한 지역의 엔드포인트가 사용됩니다.</td>
        </tr>
        <tr>
          <td><code><em>&lt;cluster_space&gt;</em></code></td>
          <td>선택사항: 로그를 전송할 Cloud Foundry 영역의 이름입니다. 로그를 {{site.data.keyword.loganalysisshort_notm}}로 전달하는 경우 영역 및 조직이 유입 지점에서 지정됩니다. 영역을 지정하지 않으면 로그는 계정 레벨로 전송됩니다.</td>
        </tr>
        <tr>
          <td><code><em>&lt;cluster_org&gt;</em></code></td>
          <td>영역이 있는 Cloud Foundry 조직의 이름입니다. 영역을 지정한 경우 이 값은 필수입니다.</td>
        </tr>
      </tbody>
    </table>

2. 계획한 대로 구현되었는지 확인하려면 클러스터 로깅 구성을 보십시오.

    ```
    bx cs logging-config-get <cluster_name_or_ID>
    ```
    {: pre}

    예제 명령 및 출력:
    ```
    bx cs logging-config-get myCluster
    Retrieving cluster myCluster logging configurations...
    OK
    Id                                     Source        Namespace   Host                                 Port    Org   Space   Protocol   Application Containers   Paths
    a550d2ba-6a02-4d4d-83ef-68f7a113325c   container     *           ingest-au-syd.logging.bluemix.net✣   9091✣   -     -       ibm        -                        -
    14ca6a0c-5bc8-499a-b1bd-cedcf40ab850   kube-audit    -           ingest-au-syd.logging.bluemix.net✣   9091✣   -     -       ibm        -                    
    ```
    {: screen}

  <table>
  <caption>이 명령의 컴포넌트 이해</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 이 명령의 컴포넌트 이해</th>
    </thead>
    <tbody>
      <tr>
        <td><code><em>&lt;cluster_name_or_ID&gt;</em></code></td>
        <td>클러스터의 이름 또는 ID입니다.</td>
      </tr>
    </tbody>
  </table>

3. 선택사항: 감사 로그 전달을 중지하려는 경우 [구성을 삭제](#log_sources_delete)할 수 있습니다.

<br />



### 감사 로그를 외부 서버에 전송
{: #audit_enable}

**시작하기 전에**

1. 로그를 전달할 수 있는 원격 로깅 서버를 설정하십시오. 예를 들어, [Logstash를 Kubernetes와 함께 사용![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/debug-application-cluster/audit/#use-logstash-to-collect-and-distribute-audit-events-from-webhook-backend)하여 감사 이벤트를 수집할 수 있습니다.

2. API 서버 감사 로그를 수집할 클러스터를 [CLI의 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오. **참고**: 데디케이티드 계정을 사용 중인 경우 로그 전달을 사용으로 설정하려면 퍼블릭 {{site.data.keyword.cloud_notm}} 엔드포인트에 로그인하고 공용 조직 및 영역을 대상으로 지정해야 합니다.

Kubernetes API 감사 로그를 전달하려면 다음을 수행하십시오.

1. 웹훅을 설정하십시오. 이 플래그에 정보를 제공하지 않은 경우에는 기본 구성이 사용됩니다.

    ```
    bx cs apiserver-config-set audit-webhook <cluster_name_or_ID> --remoteServer <server_URL_or_IP> --caCert <CA_cert_path> --clientCert <client_cert_path> --clientKey <client_key_path>
    ```
    {: pre}

  <table>
  <caption>이 명령의 컴포넌트 이해</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 이 명령의 컴포넌트 이해</th>
    </thead>
    <tbody>
      <tr>
        <td><code><em>&lt;cluster_name_or_ID&gt;</em></code></td>
        <td>클러스터의 이름 또는 ID입니다.</td>
      </tr>
      <tr>
        <td><code><em>&lt;server_URL&gt;</em></code></td>
        <td>로그를 전송할 원격 로깅 서비스의 URL 또는 IP 주소입니다. 비보안 서버 URL을 제공한 경우 인증서가 무시됩니다.</td>
      </tr>
      <tr>
        <td><code><em>&lt;CA_cert_path&gt;</em></code></td>
        <td>원격 로깅 서비스를 확인하는 데 사용되는 CA 인증서의 파일 경로입니다.</td>
      </tr>
      <tr>
        <td><code><em>&lt;client_cert_path&gt;</em></code></td>
        <td>원격 로깅 서비스에 대해 인증하는 데 사용되는 클라이언트 인증서의 파일 경로입니다.</td>
      </tr>
      <tr>
        <td><code><em>&lt;client_key_path&gt;</em></code></td>
        <td>원격 로깅 서비스에 연결하는 데 사용되는 해당 클라이언트 키의 파일 경로입니다.</td>
      </tr>
    </tbody>
  </table>

2. 원격 로깅 서비스의 URL을 확인하여 로그 전달이 사용으로 설정되었는지 확인하십시오.

    ```
    bx cs apiserver-config-get audit-webhook <cluster_name_or_ID>
    ```
    {: pre}

    출력 예:
    ```
    OK
    Server:			https://8.8.8.8
    ```
    {: screen}

3. Kubernetes 마스터를 다시 시작하여 구성 업데이트를 적용하십시오.

    ```
    bx cs apiserver-refresh <cluster_name_or_ID>
    ```
    {: pre}

4. 선택사항: 감사 로그 전달을 중지하려는 경우 구성을 사용 안함으로 설정할 수 있습니다.
    1. API 서버 감사 로그 수집을 중지할 클러스터를 [CLI의 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오. 
    2. 클러스터의 API 서버를 위한 웹훅 백엔드 구성을 사용 안함으로 설정하십시오.

        ```
        bx cs apiserver-config-unset audit-webhook <cluster_name_or_ID>
        ```
        {: pre}

    3. Kubernetes 마스터를 다시 시작하여 구성 업데이트를 적용하십시오.

        ```
        bx cs apiserver-refresh <cluster_name_or_ID>
        ```
        {: pre}

## 메트릭 보기
{: #view_metrics}

메트릭은 클러스터의 상태와 성능을 모니터하는 데 도움이 됩니다. 표준 Kubernetes 및 Docker 기능을 사용하여 클러스터 및 앱의 상태를 모니터할 수 있습니다. **참고**: 모니터링은 표준 클러스터에서만 지원됩니다.
{:shortdesc}

<dl>
  <dt>{{site.data.keyword.Bluemix_notm}}의 클러스터 세부사항 페이지</dt>
    <dd>{{site.data.keyword.containershort_notm}}는 클러스터의 상태와 용량 및 클러스터 리소스의 사용에 대한 정보를 제공합니다. 이 GUI를 사용하면 클러스터를 확장하고 지속적 스토리지 관련 작업을 수행하며 {{site.data.keyword.Bluemix_notm}} 서비스 바인딩을 통해 클러스터에 기능을 추가할 수 있습니다. 클러스터 세부사항 페이지를 보려면 **{{site.data.keyword.Bluemix_notm}}대시보드**로 이동하고 클러스터를 선택하십시오.</dd>
  <dt>Kubernetes 대시보드</dt>
    <dd>Kubernetes 대시보드는 작업자 노드의 상태를 검토하고 Kubernetes 리소스를 찾으며 컨테이너화된 앱을 배치하고 로깅 및 모니터링 정보를 사용하여 앱의 문제점을 해결할 수 있는 관리 웹 인터페이스입니다. Kubernetes 대시보드에 액세스하는 방법에 대한 자세한 정보는 [{{site.data.keyword.containershort_notm}}의 Kubernetes 대시보드 시작](cs_app.html#cli_dashboard)을 참조하십시오.</dd>
  <dt>{{site.data.keyword.monitoringlong_notm}}</dt>
    <dd><p>표준 클러스터의 메트릭은 Kubernetes 클러스터가 작성될 때 로그인된 {{site.data.keyword.Bluemix_notm}} 계정에 위치합니다. 클러스터를 작성할 때 {{site.data.keyword.Bluemix_notm}} 영역을 지정한 경우 메트릭은 해당 영역에 위치합니다. 컨테이너 메트릭은 클러스터에 배치된 모든 컨테이너에 대해 자동으로 수집됩니다. 이러한 메트릭이 전송되고
Grafana를 통해 사용 가능하게 됩니다. 메트릭에 대한 자세한 정보는 [{{site.data.keyword.containershort_notm}}의 모니터링](/docs/services/cloud-monitoring/containers/monitoring_containers_ov.html#monitoring_bmx_containers_ov)을 참조하십시오.</p>
    <p>Grafana 대시보드에 액세스하려면 다음 URL 중 하나로 이동하여 클러스터를 작성한 {{site.data.keyword.Bluemix_notm}} 계정 또는 영역을 선택하십시오.
      <ul>
        <li>미국 남부 및 미국 동부: https://metrics.ng.bluemix.net</li>
        <li>미국 남부: https://metrics.eu-gb.bluemix.net</li>
        <li>중앙 유럽: https://metrics.eu-de.bluemix.net</li>
      </ul></p></dd>
</dl>

### 기타 상태 모니터링 도구
{: #health_tools}

추가 모니터링 기능을 위해 다른 도구를 구성할 수 있습니다.
<dl>
  <dt>Prometheus</dt>
    <dd>Prometheus는 Kubernetes에 맞게 설계된 오픈 소스 모니터링, 로깅 및 경보 도구입니다. 도구는 클러스터, 작업자 노드 및 Kubernetes 로깅 정보를 기반으로 한 배치 상태에 대한 자세한 정보를 검색합니다. 설정 정보는 [{{site.data.keyword.containershort_notm}}와 서비스 통합](cs_integrations.html#integrations)을 참조하십시오.</dd>
</dl>

<br />


## 자동 복구를 통해 작업자 노드의 상태 모니터링 구성
{: #autorecovery}

{{site.data.keyword.containerlong_notm}} 자동 복구 시스템은 Kubernetes 버전 1.7 이상의 기존 클러스터에 배치할 수 있습니다.
{: shortdesc}

자동 복구 시스템은 다양한 검사를 통해 작업자 노드 상태를 조회합니다. 자동 복구는 구성된 검사에 따라 비정상적인 작업자 노드를 발견하면 작업자 노드에서 OS 다시 로드와 같은 정정 조치를 트리거합니다. 한 번에 하나의 작업자 노드에서만 정정 조치가 이루어집니다. 다른 작업자 노드에서 정정 조치가 이루어지려면 먼저 현재 작업자 노드가 정정 조치를 완료해야 합니다. 자세한 정보는 이 [자동 복구 블로그 게시물![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/)을 참조하십시오.</br> </br>
**참고**: 자동 복구를 위해서는 하나 이상의 정상 노드가 올바르게 작동해야 합니다. 둘 이상의 작업자 노드가 있는 클러스터에서만 활성 검사를 통한 자동 복구를 구성하십시오.

시작하기 전에 작업자 노드 상태를 검사할 클러스터를 [CLI의 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오.

1. [클러스터를 위해 Helm을 설치하고 {{site.data.keyword.Bluemix_notm}} 저장소를 Helm 인스턴스에 추가](cs_integrations.html#helm)하십시오.

2. 검사를 JSON 형식으로 정의하는 구성 맵 파일을 작성하십시오. 예를 들어, 다음 YAML 파일은 세 가지 검사(하나의 HTTP 검사와 두 개의 Kubernetes API 서버 검사)를 정의합니다. 세 가지 유형의 검사에 대한 정보와 검사의 개별 컴포넌트에 대한 정보는 예제 YAML 파일 다음에 나오는 표를 참조하십시오.
</br>
   **팁**: 구성 맵의 `data` 섹션에서 각 검사를 고유 키로 정의하십시오.

   ```
   kind: ConfigMap
   apiVersion: v1
   metadata:
     name: ibm-worker-recovery-checks
     namespace: kube-system
   data:
     checknode.json: |
       {
         "Check":"KUBEAPI",
          "Resource":"NODE",
          "FailureThreshold":3,
          "CorrectiveAction":"RELOAD",
          "CooloffSeconds":1800,
          "IntervalSeconds":180,
          "TimeoutSeconds":10,
          "Enabled":true
        }
      checkpod.json: |
        {
         "Check":"KUBEAPI",
         "Resource":"POD",
         "PodFailureThresholdPercent":50,
         "FailureThreshold":3,
         "CorrectiveAction":"RELOAD",
         "CooloffSeconds":1800,
         "IntervalSeconds":180,
         "TimeoutSeconds":10,
         "Enabled":true
       }
     checkhttp.json: |
       {
         "Check":"HTTP",
         "FailureThreshold":3,
         "CorrectiveAction":"REBOOT",
         "CooloffSeconds":1800,
         "IntervalSeconds":180,
         "TimeoutSeconds":10,
         "Port":80,
         "ExpectedStatus":200,
         "Route":"/myhealth",
         "Enabled":false
       }
   ```
   {:codeblock}

   <table summary="configmap의 컴포넌트 이해">
   <caption>configmap 컴포넌트 이해</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> configmap 컴포넌트 이해</th>
   </thead>
   <tbody>
   <tr>
   <td><code>name</code></td>
   <td>구성 이름 <code>ibm-worker-recovery-checks</code>는 상수이고 변경될 수 없습니다.</td>
   </tr>
   <tr>
   <td><code>namespace</code></td>
   <td><code>kube-system</code> 네임스페이스는 상수이고 변경될 수 없습니다.</td>
   </tr>
   <tr>
   <td><code>checknode.json</code></td>
   <td>각 작업자 노드가 <code>Ready</code> 상태인지 검사하는 Kubernetes API 노드 검사를 정의합니다. 작업자 노드 상태가 <code>Ready</code> 상태가 아니면 해당 작업자 노드에 대한 검사는 실패로 간주됩니다. 예제의 검사에서는 3분 마다 YAML이 실행됩니다. 3회 연속 실패 시, 작업자 노드가 다시 로드됩니다. 이 조치는 <code>bx cs worker-reload</code>를 실행하는 것과 같습니다.<br></br>노드 검사는 <b>Enabled</b> 필드를 <code>false</code>로 설정하거나 검사를 제거할 때까지 활성화됩니다.</td>
   </tr>
   <tr>
   <td><code>checkpod.json</code></td>
   <td>
작업자 노드에 지정된 총 팟(Pod)을 기반으로 해당 작업자 노드에 있는 <code>NotReady</code> 팟(Pod)의 전체 백분율을 검사하는 Kubernetes API 팟(Pod) 검사를 정의합니다. <code>NotReady</code> 팟(Pod)의 전체 백분율이 정의된 <code>PodFailureThresholdPercent</code>보다 큰 경우 해당 작업자 노드에 대한 검사가 실패로 간주됩니다. 예제의 검사에서는 3분 마다 YAML이 실행됩니다. 3회 연속 실패 시, 작업자 노드가 다시 로드됩니다. 이 조치는 <code>bx cs worker-reload</code>를 실행하는 것과 같습니다. 예를 들어, 기본 <code>PodFailureThresholdPercent</code>는 50%입니다. 3회 연속 <code>NotReady</code> 팟(Pod)의 백분율이 50%보다 크면 작업자 노드가 다시 로드됩니다. <br></br>기본적으로는 모든 네임스페이스의 팟(Pod)이 검사됩니다. 지정된 네임스페이스의 팟(Pod)으로 검사를 제한하려면 검사에 <code>Namespace</code> 필드를 추가하십시오. 이 팟(Pod) 검사는 <b>Enabled</b> 필드를 <code>false</code>로 설정하거나 검사를 제거할 때까지 활성화됩니다.
   </td>
   </tr>
   <tr>
   <td><code>checkhttp.json</code></td>
   <td>작업자 노드에서 실행되는 HTTP 서버가 정상인지 검사하는 HTTP 검사를 정의합니다. 이 검사를 사용하려면 [DaemonSet ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)을 사용하여 클러스터 내의 모든 작업자 노드에 HTTP 서버를 배치해야 합니다. <code>/myhealth</code> 경로에서 사용 가능하며 HTTP 서버가 정상인지 확인할 수 있는 상태 검사를 구현해야 합니다. <strong>Route</strong> 매개변수를 변경하여 다른 경로를 정의할 수 있습니다. HTTP 서버가 정상인 경우에는 <strong>ExpectedStatus</strong>에 정의된 HTTP 응답 코드를 리턴해야 합니다. HTTP 서버는 작업자 노드의 사설 IP 주소를 청취하도록 구성되어야 합니다. 이 사설 IP 주소는 <code>kubectl get nodes</code>를 실행하여 찾을 수 있습니다.<br></br>
   예를 들면, 클러스터에 사설 IP 주소 10.10.10.1 및 10.10.10.2를 사용하는 두 개의 노드가 있다고 가정합니다. 이 예에서는 두 개의 라우트 <code>http://10.10.10.1:80/myhealth</code> 및 <code>http://10.10.10.2:80/myhealth</code>가 200 HTTP 응답을 리턴하는지 검사됩니다.
   예제의 검사에서는 3분 마다 YAML이 실행됩니다. 3회 연속 실패 시, 작업자 노드가 다시 부팅됩니다. 이 조치는 <code>bx cs worker-reboot</code>를 실행하는 것과 같습니다.<br></br>HTTP 검사는 <b>Enabled</b> 필드를 <code>true</code>로 설정할 때까지 비활성화됩니다.</td>
   </tr>
   </tbody>
   </table>

   <table summary="검사의 개별 컴포넌트 이해">
   <caption>검사의 개별 컴포넌트 이해</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/>검사의 개별 컴포넌트 이해</th>
   </thead>
   <tbody>
   <tr>
   <td><code>Check</code></td>
   <td>자동 복구에서 사용할 검사 유형을 입력하십시오. <ul><li><code>HTTP</code>: 자동 복구는 각 노드에서 실행되는 HTTP 서버를 호출하여 노드가 올바르게 실행 중인지 여부를 판별합니다.</li><li><code>KUBEAPI</code>: 자동 복구는 Kubernetes API 서버를 호출하고 작업자 노드에서 보고된 상태 데이터를 읽습니다.</li></ul></td>
   </tr>
   <tr>
   <td><code>Resource</code></td>
   <td>검사 유형이 <code>KUBEAPI</code>인 경우 자동 복구에서 검사할 리소스 유형을 입력하십시오. 허용되는 값은 <code>NODE</code> 또는 <code>POD</code>입니다.</td>
   </tr>
   <tr>
   <td><code>FailureThreshold</code></td>
   <td>연속 실패한 검사의 수에 대한 임계값을 입력하십시오. 이 임계값에 도달하면 자동 복구가 지정된 정정 조치를 트리거합니다. 예를 들어, 값이 3이고 자동 복구가 구성된 검사에 세 번 연속 실패하면 자동 복구가 검사와 연관된 정정 조치를 트리거합니다.</td>
   </tr>
   <tr>
   <td><code>PodFailureThresholdPercent</code></td>
   <td>리소스 유형이 <code>POD</code>인 경우 [NotReady ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/#define-readiness-probes) 상태인 작업자 노드의 팟(Pod) 백분율에 대한 임계값을 입력하십시오. 이 백분율은 작업자 노드에 대해 스케줄된 총 팟(Pod) 수를 기준으로 합니다. 검사에서 비정상 팟(Pod)의 백분율이 임계값을 초과한다고 판별되면 하나의 오류로 계수합니다.</td>
   </tr>
   <tr>
   <td><code>CorrectiveAction</code></td>
   <td>오류 임계값에 도달할 때 실행할 조치를 입력하십시오. 정정 조치는 다른 작업자가 수리되지 않는 동안에 그리고 작업자 노드가 이전 조치의 쿨오프 주기가 아닌 경우에만 실행됩니다. <ul><li><code>REBOOT</code>: 작업자 노드를 다시 부팅합니다.</li><li><code>RELOAD</code>: 클린 OS에서 작업자 노드에 필요한 모든 구성을 다시 로드합니다.</li></ul></td>
   </tr>
   <tr>
   <td><code>CooloffSeconds</code></td>
   <td>자동 복구가 이미 정정 조치가 실행된 노드로 인해 다른 정정 조치를 실행할 때까지 대기해야 하는 시간(초)을 입력하십시오. 쿨 오프 주기는 정정 조치가 실행될 때 시작됩니다.</td>
   </tr>
   <tr>
   <td><code>IntervalSeconds</code></td>
   <td>연속 검사 간 시간(초)을 입력하십시오. 예를 들어, 값이 180이면 자동 복구는 3분마다 각 노드에서 검사를 실행합니다.</td>
   </tr>
   <tr>
   <td><code>TimeoutSeconds</code></td>
   <td>자동 복구가 호출 오퍼레이션을 종료하기 전까지 데이터베이스에 대한 검사 호출에 걸리는 최대 시간(초)을 입력하십시오. <code>TimeoutSeconds</code> 값은 <code>IntervalSeconds</code> 값보다 작아야 합니다.</td>
   </tr>
   <tr>
   <td><code>Port</code></td>
   <td>검사 유형이 <code>HTTP</code>인 경우 HTTP 서버가 작업자 노드에서 바인딩해야 하는 포트를 입력하십시오. 이 포트는 클러스터에 있는 모든 작업자 노드의 IP에 노출되어야 합니다. 자동 복구에는 서버 검사를 위해 모든 노드에서 일정한 포트 번호가 필요합니다. 사용자 정의 서버를 클러스터에 배치할 때 [DaemonSets ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)를 사용하십시오.</td>
   </tr>
   <tr>
   <td><code>ExpectedStatus</code></td>
   <td>검사 유형이 <code>HTTP</code>인 경우 검사에서 리턴될 HTTP 서버 상태를 입력하십시오. 예를 들어, 값 200은 서버의 <code>OK</code> 응답을 예상함을 표시합니다.</td>
   </tr>
   <tr>
   <td><code>Route</code></td>
   <td>검사 유형이 <code>HTTP</code>인 경우 HTTP 서버에서 요청되는 경로를 입력하십시오. 이 값은 일반적으로 모든 작업자 노드에서 실행 중인 서버의 메트릭 경로입니다.</td>
   </tr>
   <tr>
   <td><code>Enabled</code></td>
   <td>검사를 사용으로 설정하려면 <code>true</code>를 입력하고 사용 안함으로 설정하려면 <code>false</code>를 입력하십시오.</td>
   </tr>
   <tr>
   <td><code>Namespace</code></td>
   <td> 선택사항: 한 네임스페이스의 팟(Pod)만을 검사하도록 <code>checkpod.json</code>을 제한하려면 <code>Namespace</code> 필드를 추가하고 네임스페이스를 입력하십시오.</td>
   </tr>
   </tbody>
   </table>

3. 클러스터에서 구성 맵을 작성하십시오.

    ```
    kubectl apply -f ibm-worker-recovery-checks.yaml
    ```
    {: pre}

3. 적절한 검사를 통해 `kube-system` 네임스페이스에서 이름이 `ibm-worker-recovery-checks`인 구성 맵을 작성했는지 확인하십시오.

    ```
    kubectl -n kube-system get cm ibm-worker-recovery-checks -o yaml
    ```
    {: pre}

4. `ibm-worker-recovery` Helm 차트를 설치하여 클러스터에 자동 복구를 배치하십시오.

    ```
    helm install --name ibm-worker-recovery ibm/ibm-worker-recovery  --namespace kube-system
    ```
    {: pre}

5. 몇 분 후에 다음 명령의 출력에서 `Events` 섹션을 확인하여 자동 복구 배치의 활동을 볼 수 있습니다.

    ```
    kubectl -n kube-system describe deployment ibm-worker-recovery
    ```
    {: pre}

6. 자동 복구 배치에 대한 활동이 표시되지 않는 경우 자동 복구 차트 정의에 포함된 테스트를 실행하여 Helm 배치를 확인할 수 있습니다.

    ```
    helm test ibm-worker-recovery
    ```
    {: pre}
