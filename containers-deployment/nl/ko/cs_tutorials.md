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



# 튜토리얼: 클러스터 작성
{: #cs_cluster_tutorial}

{{site.data.keyword.containerlong}}에서 Kubernetes 클러스터를 배치하고 관리하십시오. 클러스터에서 컨테이너화된 앱의 배치, 오퍼레이션, 스케일링 및 모니터링을 자동화할 수 있습니다.
{:shortdesc}

이 튜토리얼 시리즈에서는 가상의 홍보(PR) 회사가 Kubernetes 기능을 사용하여 {{site.data.keyword.Bluemix_notm}}의 컨테이너화된 앱을 배치하는 방법을 보여줍니다. PR 회사에서는 Leveraging {{site.data.keyword.toneanalyzerfull}}를 활용하여 보도 자료를 분석하고 피드백을 받습니다.


## 목표

이 첫 번째 튜토리얼에서는 사용자가 PR 회사의 네트워킹 관리자 역할을 합니다. 앱의 Hello World 버전을 배치하고 테스트하는 데 사용되는 사용자 정의 Kubernetes 클러스터를 구성합니다.

인프라를 설정하려면 다음을 수행하십시오.

-   하나의 작업자 노드가 있는 클러스터를 작성하십시오.
-   Kubernetes 명령을 실행하고 Docker 이미지를 관리하기 위한 CLI를 설치하십시오.
-   {{site.data.keyword.registrylong_notm}}에 이미지를 저장하기 위한 개인용 이미지 저장소를 작성하십시오.
-   클러스터의 앱이 해당 서비스를 사용할 수 있도록 {{site.data.keyword.toneanalyzershort}} 서비스를 클러스터에 추가하십시오.


## 소요 시간

40분


## 대상

이 튜토리얼은 처음으로 Kubernetes 클러스터를 작성하는 소프트웨어 개발자와 네트워크 관리자를 대상으로 합니다.


## 전제조건

-  종량과금제 또는 구독 [{{site.data.keyword.Bluemix_notm}} 계정 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.bluemix.net/registration/)
-  작업하려는 클러스터 영역의 [Cloud Foundry 개발자 역할](/docs/iam/mngcf.html#mngcf)


## 학습 1: 클러스터 작성 및 CLI 설정
{: #cs_cluster_tutorial_lesson1}

GUI에서 클러스터를 작성하고 필수 CLI를 설치합니다.
{: shortdesc}

**클러스터를 작성하려면 다음을 수행하십시오.**

프로비저닝하는 데 몇 분이 걸릴 수 있으므로 CLI를 설치하기 전에 클러스터를 작성하십시오.

1.  [ GUI ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.bluemix.net/containers-kubernetes/catalog/cluster/create)에서 하나의 작업자 노드가 포함된 무료 또는 표준 클러스터를 작성하십시오. 

    [CLI에서 클러스터](cs_clusters.html#clusters_cli)를 작성할 수도 있습니다.
    {: tip}

클러스터가 프로비저닝될 때 클러스터를 관리하는 데 사용되는 다음 CLI를 설치하십시오.
-   {{site.data.keyword.Bluemix_notm}} CLI
-   {{site.data.keyword.containershort_notm}} 플러그인
-   Kubernetes CLI
-   {{site.data.keyword.registryshort_notm}} 플러그인
-   Docker CLI

</br>
**CLI 및 해당 필수 소프트웨어를 설치하려면 다음을 수행하십시오.**

1.  {{site.data.keyword.containershort_notm}} 플러그인의 필수 소프트웨어로서 [{{site.data.keyword.Bluemix_notm}} CLI ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://clis.ng.bluemix.net/ui/home.html)를 설치하십시오. {{site.data.keyword.Bluemix_notm}} CLI 명령을 실행하려면 `bx` 접두부를 사용하십시오.
2.  프롬프트에 따라 계정 및 {{site.data.keyword.Bluemix_notm}} 조직을 선택하십시오. 클러스터는 계정에 특정하지만, {{site.data.keyword.Bluemix_notm}} 조직이나 영역에는 독립적입니다.

4.  Kubernetes 클러스터를 작성하고 작업자 노드를 관리하려면 {{site.data.keyword.containershort_notm}} 플러그인을 설치하십시오. {{site.data.keyword.containershort_notm}} 플러그인 명령을 실행하려면 `bx cs` 접두부를 사용하십시오.

    ```
    bx plugin install container-service -r Bluemix
    ```
    {: pre}

5.  클러스터에 앱을 배치하려면 [Kubernetes CLI를 설치 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/tools/install-kubectl/)하십시오. Kubernetes CLI를 사용하여 명령을 실행하려면 `kubectl` 접두부를 사용하십시오.
    1.  전체 기능 호환성을 위해 사용하려는 Kubernetes 클러스터 버전과 일치하는 Kubernetes CLI 버전을 다운로드하십시오. 현재 {{site.data.keyword.containershort_notm}} 기본 Kubernetes 버전은 1.9.7입니다. 

        OS X: [https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/darwin/amd64/kubectl ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/darwin/amd64/kubectl)

        Linux:   [https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/linux/amd64/kubectl ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/linux/amd64/kubectl)

        Windows:   [https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/windows/amd64/kubectl.exe ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/windows/amd64/kubectl.exe)

          **팁:** Windows를 사용하는 경우, {{site.data.keyword.Bluemix_notm}} CLI와 동일한 디렉토리에 Kubernetes CLI를 설치하십시오. 이 설정을 사용하면 나중에 명령을 실행할 때 일부 파일 경로 변경이 필요하지 않습니다.

    2.  OS X 또는 Linux를 사용 중인 경우 다음 단계를 완료하십시오.
        1.  실행 파일을 `/usr/local/bin` 디렉토리로 이동하십시오.

            ```
            mv filepath/kubectl /usr/local/bin/kubectl
            ```
            {: pre}

        2.  `/usr/local/bin`이 `PATH` 시스템 변수에 나열되어 있는지 확인하십시오. `PATH` 변수에는 운영 체제가 실행 파일을 찾을 수 있는 모든 디렉토리가 포함되어 있습니다. `PATH` 변수에 나열된 디렉토리는 서로 다른 용도로 사용됩니다. `/usr/local/bin`은 시스템 관리자가 수동으로 설치했으며 운영 체제의 일부가 아닌 소프트웨어의 실행 파일을 저장하는 데 사용됩니다.

            ```
             echo $PATH
            ```
            {: pre}

            CLI 출력이 다음과 유사하게 나타납니다.

            ```
            /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
            ```
            {: screen}

        3.  파일을 실행 가능하도록 설정하십시오.

            ```
            chmod +x /usr/local/bin/kubectl
            ```
            {: pre}

6. {{site.data.keyword.registryshort_notm}}에서 개인용 이미지 저장소를 설정하여 관리하려면 {{site.data.keyword.registryshort_notm}} 플러그인을 설치하십시오. 레지스트리 명령을 실행하려면 `bx cr` 접두부를 사용하십시오.

    ```
    bx plugin install container-registry -r Bluemix
    ```
    {: pre}

    container-service 및 container-registry 플러그인이 올바르게 설치되었는지 확인하려면 다음 명령을 실행하십시오.

    ```
    bx plugin list
    ```
    {: pre}

7. 이미지를 로컬로 빌드하여 개인용 이미지 저장소에 푸시하려면 [Docker Community Edition CLI를 설치 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.docker.com/community-edition#/download)하십시오. Windows 8 이하를 사용 중인 경우 [Docker Toolbox ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.docker.com/toolbox/toolbox_install_windows/)를 대신 설치할 수 있습니다.

축하합니다! 다음 학습과 튜토리얼을 위한 CLI를 설치했습니다. 다음으로 클러스터 환경을 설정하고 {{site.data.keyword.toneanalyzershort}} 서비스를 추가하십시오.


## 학습 2: 개인용 레지스트리 설정
{: #cs_cluster_tutorial_lesson2}

{{site.data.keyword.registryshort_notm}}에서 개인용 이미지 저장소를 설정하고 {{site.data.keyword.toneanalyzershort}} 서비스에 액세스할 수 있도록 클러스터에 시크릿을 추가하십시오.
{: shortdesc}

1.  프롬프트가 표시되면 {{site.data.keyword.Bluemix_notm}} 신임 정보를 사용하여 {{site.data.keyword.Bluemix_notm}} CLI에 로그인하십시오.

    ```
    bx login [--sso]
    ```
    {: pre}

    **참고:** 연합 ID가 있는 경우 `--sso` 플래그를 사용하여 로그인하십시오. 사용자 이름을 입력하고
CLI 출력에서 제공된 URL을 사용하여 일회성 패스코드를 검색하십시오.

2.  Docker 이미지를 안전하게 저장하고 모든 클러스터 사용자와 공유할 수 있도록 {{site.data.keyword.registryshort_notm}}에서 사용자 고유의 개인용 이미지 저장소를 설정하십시오. {{site.data.keyword.Bluemix_notm}}의 개인용 저장소는 네임스페이스로 식별됩니다. 네임스페이스를 사용하여 개발자가 개인용 Docker 이미지에 액세스하기 위해 사용할 수 있는 개인용 저장소에 대한 고유 URL을 작성할 수 있습니다.

    컨테이너 이미지에 대해 작업하는 경우 [개인 정보 보호](cs_secure.html#pi)에 대해 자세히 알아보십시오.

    이 예에서 PR 회사는 _pr_firm_을 자체 네임스페이스로서 선택하여 자체 계정의 모든 이미지를 그룹화할 수 있도록 {{site.data.keyword.registryshort_notm}}에서 하나의 이미지 저장소만 작성하고자 합니다. _&lt;namespace&gt;_를 튜토리얼과 관련이 없는, 선택한 네임스페이스로 대체하십시오.

    ```
    bx cr namespace-add <namespace>
    ```
    {: pre}

3.  다음 단계를 계속하기 전에 작업자 노드의 배치가 완료되었는지 확인하십시오.

    ```
    bx cs workers <cluster_name_or_ID>
    ```
    {: pre}

    작업자 노드의 프로비저닝이 완료되면 상태가 **준비**로 변경되며 {{site.data.keyword.Bluemix_notm}} 서비스 바인딩을 시작할 수 있습니다.

    ```
    ID                                                 Public IP       Private IP       Machine Type   State    Status   Location   Version
    kube-mil01-pafe24f557f070463caf9e31ecf2d96625-w1   169.xx.xxx.xxx   10.xxx.xx.xxx   free           normal   Ready    mil01      1.9.7
    ```
    {: screen}

## 학습 3: 클러스터 환경 설정
{: #cs_cluster_tutorial_lesson3}

CLI에서 클러스터의 컨텍스트를 설정하십시오.
{: shortdesc}

클러스터 관련 작업을 위해 {{site.data.keyword.containerlong}} CLI에 로그인할 때마다 사용자는 이러한 명령을 실행하여 클러스터의 구성 파일에 대한 경로를 세션 변수로 설정해야 합니다. Kubernetes CLI는 이 변수를 사용하여 {{site.data.keyword.Bluemix_notm}}에서 클러스터와 연결하는 데 필요한 로컬 구성 파일과 인증서를 찾습니다.

1.  환경 변수를 설정하기 위한 명령을 가져오고 Kubernetes 구성 파일을 다운로드하십시오.

    ```
    bx cs cluster-config <cluster_name_or_ID>
    ```
    {: pre}

    구성 파일 다운로드가 완료되면 환경 변수로서 경로를 로컬 Kubernetes 구성 파일로 설정하는 데 사용할 수 있는 명령이 표시됩니다.

    OS X에 대한 예:

    ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/pr_firm_cluster/kube-config-prod-par02-pr_firm_cluster.yml
    ```
    {: screen}

2.  `KUBECONFIG` 환경 변수를 설정하려면 터미널에 표시되는 명령을 복사하여 붙여넣기하십시오.

3.  `KUBECONFIG` 환경 변수가 올바르게 설정되었는지 확인하십시오.

    OS X에 대한 예:

    ```
        echo $KUBECONFIG
    ```
    {: pre}

    출력:

    ```
        /Users/<user_name>/.bluemix/plugins/container-service/clusters/pr_firm_cluster/kube-config-prod-par02-pr_firm_cluster.yml
    ```
    {: screen}

4.  Kubernetes CLI 서버 버전을 확인하여 `kubectl` 명령이 올바르게 실행되는지 확인하십시오.

    ```
        kubectl version  --short
    ```
    {: pre}

    출력 예:

    ```
    Client Version: v1.9.7
    Server Version: v1.9.7
    ```
    {: screen}

## 학습 4: 클러스터에 서비스 추가
{: #cs_cluster_tutorial_lesson4}

{{site.data.keyword.Bluemix_notm}} 서비스를 사용하면 앱에서 이미 개발된 앱을 활용할 수 있습니다. 클러스터에 바인딩된 {{site.data.keyword.Bluemix_notm}} 서비스는 해당 클러스터에 배치된 앱에 의해 사용될 수 있습니다. 앱에서 사용할 모든 {{site.data.keyword.Bluemix_notm}} 서비스에 대해 다음 단계를 반복하십시오.

1.  {{site.data.keyword.toneanalyzershort}} 서비스를 {{site.data.keyword.Bluemix_notm}} 계정에 추가하십시오. <service_name>을 서비스 인스턴스의 이름으로 대체하십시오.

    **참고:** {{site.data.keyword.toneanalyzershort}} 서비스를 계정에 추가하면 서비스가 무료가 아님을 알리는 메시지가 표시됩니다. API 호출을 제한하는 경우, 이 튜토리얼에서는 {{site.data.keyword.watson}} 서비스에 대한 비용을 발생시키지 않습니다. [{{site.data.keyword.watson}}{{site.data.keyword.toneanalyzershort}} 서비스의 가격 책정 정보를 검토 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/watson/developercloud/tone-analyzer.html#pricing-block)하십시오.

    ```
    bx service create tone_analyzer standard <service_name>
    ```
    {: pre}

2.  {{site.data.keyword.toneanalyzershort}} 인스턴스를 클러스터의 `default` Kubernetes 네임스페이스에 바인딩하십시오. 나중에 Kubernetes 리소스에 대한 사용자 액세스 권한을 관리하는 고유 네임스페이스를 작성할 수 있지만 지금은 `default` 네임스페이스를 사용하십시오. Kubernetes 네임스페이스는 이전에 작성한 레지스트리 네임스페이스와는 다릅니다.

    ```
    bx cs cluster-service-bind <cluster_name> default <service_name>
    ```
    {: pre}

    출력:

    ```
    bx cs cluster-service-bind pr_firm_cluster default mytoneanalyzer
    Binding service instance to namespace...
    OK
    Namespace:	default
    Secret name:	binding-mytoneanalyzer
    ```
    {: screen}

3.  Kubernetes 시크릿이 클러스터 네임스페이스에서 작성되었는지 확인하십시오. 모든 {{site.data.keyword.Bluemix_notm}} 서비스는 컨테이너가 액세스하는 데 사용하는 기밀 정보(예: 사용자 이름, 비밀번호 및 URL)가 포함된 JSON 파일로 정의됩니다. 이 정보를 안전하게 저장하기 위해 Kubernetes 시크릿이 사용됩니다. 이 예에서는 사용자 계정에서 프로비저닝된 {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} 인스턴스에 액세스하기 위한 신임 정보가 시크릿에 포함됩니다.

    ```
     kubectl get secrets --namespace=default
    ```
    {: pre}

    출력:

    ```
    NAME                       TYPE                                  DATA      AGE
    binding-mytoneanalyzer     Opaque                                1         1m
    bluemix-default-secret     kubernetes.io/dockercfg               1         1h
    default-token-kf97z        kubernetes.io/service-account-token   3         1h
    ```
    {: screen}

</br>
수고하셨습니다! 클러스터가 구성되었으며 클러스터에 앱을 배치하기 시작할 수 있도록 로컬 환경이 준비되었습니다.

## 다음 단계
{: #next}

* 배운 내용을 테스트하고 [퀴즈를 풀어보십시오! ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://ibmcloud-quizzes.mybluemix.net/containers/cluster_tutorial/quiz.php)

* [튜토리얼: {{site.data.keyword.containershort_notm}}의 Kubernetes 클러스터에 앱 배치](cs_tutorials_apps.html#cs_apps_tutorial)를 시도하여 작성한 클러스터에 PR 회사의 앱을 배치하십시오.
