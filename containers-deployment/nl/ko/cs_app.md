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




# 클러스터에 앱 배치
{: #app}

{{site.data.keyword.containerlong}}에서 Kubernetes 기술을 사용하여 컨테이너에 앱을 배치하고 앱이 시작되어 실행 중인지 항상 확인할 수 있습니다. 예를 들어, 사용자를 위해 작동 중단 시간 없이 롤링 업데이트 및 롤백을 수행할 수 있습니다.
{: shortdesc}

다음 이미지의 영역을 클릭하여 앱을 배치하기 위한 일반 단계를 자세히 보십시오.

<img usemap="#d62e18" border="0" class="image" id="basic_deployment_process" src="images/basic_deployment_process.png" width="780" style="width:780px;" alt="기본 배치 프로세스"/>
<map name="d62e18" id="d62e18">
<area href="cs_cli_install.html" target="_blank" alt="CLI를 설치하십시오." title="CLI를 설치하십시오." shape="rect" coords="30, 69, 179, 209" />
<area href="https://kubernetes.io/docs/concepts/configuration/overview/" target="_blank" alt="앱의 구성 파일을 작성하십시오. Kubernetes의 우수 사례를 검토하십시오." title="앱의 구성 파일을 작성하십시오. Kubernetes의 우수 사례를 검토하십시오." shape="rect" coords="254, 64, 486, 231" />
<area href="#app_cli" target="_blank" alt="옵션 1: Kubernetes CLI에서 구성 파일을 실행하십시오." title="옵션 1: Kubernetes CLI에서 구성 파일을 실행하십시오." shape="rect" coords="544, 67, 730, 124" />
<area href="#cli_dashboard" target="_blank" alt="옵션 2: Kubernetes 대시보드를 로컬로 시작하고 구성 파일을 실행하십시오." title="옵션 2: Kubernetes 대시보드를 로컬로 시작하고 구성 파일을 실행하십시오." shape="rect" coords="544, 141, 728, 204" />
</map>


<br />


## 고가용성 배치 계획
{: #highly_available_apps}

다중 작업자 노드 및 클러스터에
보다 광범위하게 설정을 분배할수록 사용자가 앱에서 작동 중단을 겪을 가능성이 보다 줄어듭니다.
{: shortdesc}

가용성의 정도가 증가하는 순서로 정렬된 다음의 잠재적 앱 설정을 검토하십시오.

![앱의 고가용성 단계](images/cs_app_ha_roadmap.png)

1.  복제본 세트에 의해 관리되는 n+2 팟(Pod)의 배치.
2.  복제본 세트에 의해 관리되며 같은 위치의 여러 노드 간에 전개된(반친화성) n+2 팟(Pod)의 배치.
3.  복제본 세트에 의해 관리되며 서로 다른 위치의 여러 노드 간에 전개된(반친화성) n+2 팟(Pod)의 배치.
4.  복제본 세트에 의해 관리되며 서로 다른 지역의 여러 노드 간에 전개된(반친화성) n+2 팟(Pod)의 배치.




### 앱의 가용성 향상
{: #increase_availability}

<dl>
  <dt>배치 및 복제본 세트를 사용하여 앱과 해당 종속 항목 배치</dt>
    <dd><p>배치는 앱과 해당 종속 항목의 모든 컴포넌트를 선언하는 데 사용할 수 있는 Kubernetes 리소스입니다. 배치를 사용하면 모든 단계를 기록할 필요 없이 앱에 집중할 수 있습니다.</p>
    <p>둘 이상의 팟(Pod)을 배치하는 경우, 팟(Pod)을 모니터하며 항상 원하는 수의 팟(Pod)이 시작되고 실행되도록 보장하는 복제본 세트가 배치에 대해 자동으로 작성됩니다. 팟(Pod)이 중단되는 경우, 복제본 세트는 응답하지 않는 팟(Pod)을 새 팟(Pod)으로 대체합니다.</p>
    <p>배치를 사용하면 롤링 업데이트 중에 추가할 팟(Pod)의 수와 한 번에 사용 불가능한 팟(Pod)의 수를 포함하여 앱에 대한 업데이트 전략을 정의할 수 있습니다. 롤링 업데이트를 수행할 때 배치는 개정이 작동 중인지 여부를 확인하며, 장애가 발견되면 롤아웃을 중지합니다.</p>
    <p>배치를 사용하면 서로 다른 플래그를 사용하여 여러 개정판을 동시에 배치할 수 있습니다. 예를 들면, 배치를 프로덕션으로 푸시하기로 결정하기 전에 먼저 이를 테스트할 수 있습니다.</p>
    <p>배치는 배치된 개정판을 추적할 수 있게 해 줍니다. 사용자는 이 히스토리를 사용하여 업데이트가 예상한 대로 작동하지 않는 경우 이전 버전으로 롤백할 수 있습니다.</p></dd>
  <dt>앱의 워크로드를 위한 충분한 복제본과 두 개의 추가 복제본 포함</dt>
    <dd>앱의 보다 높은 가용성과 장애에 대한 복원성을 더욱 높이려면, 예상된 워크로드를 처리할 수 있도록 최소량 이상의 추가 복제본을 포함할 것을 고려하십시오. 팟(Pod)이 중단되었으며 복제본 세트가 아직 중단된 팟(Pod)을 복구하지 않은 경우, 추가 복제본은 워크로드를 처리할 수 있습니다. 두 건의 장애가 동시에 발생하지 않도록 방지하려면, 두 개의 추가 복제본을 포함하십시오. 이 설정은 N + 2 패턴으로, 여기서 N은 수신 워크로드를 처리하기 위한 복제본의 수이며 + 2는 두 개의 추가 복제본입니다. 팟(Pod)은 클러스터에 충분한 공간이 있는 한 원하는 만큼 보유할 수 있습니다.</dd>
  <dt>여러 노드 간에 팟(Pod) 전개(반친화성)</dt>
    <dd><p>배치를 작성할 때 각 팟(Pod)을 동일한 작업자 노드에 배치할 수 있습니다. 이를 친화성(affinity) 또는 동일 위치에 배치라고 합니다. 작업자 노드 장애로부터 앱을 보호하기 위해, 표준 클러스터에 <em>podAntiAffinity</em> 옵션을 사용하여 여러 작업자 노드에 팟(Pod)이 분산되도록 배치를 구성할 수 있습니다. 팟(Pod) 반친화성에는 선호 및 필수의 두 가지 유형이 있습니다. 자세한 정보는 <a href="https://kubernetes.io/docs/concepts/configuration/assign-pod-node/" rel="external" target="_blank" title="(새 탭 또는 창에서 열림)">노드에 팟(Pod) 지정</a>에 대한 Kubernetes 문서를 참조하십시오.</p>
    <p><strong>참고</strong>: 필수 반친화성을 사용하면 보유한 작업자 노드 수만큼만 복제본을 배치할 수 있습니다. 예를 들어, 클러스터에 3개의 작업자 노드가 있으나 YAML 파일에 5개의 복제본이 있는 경우에는 3개의 복제본만 배치됩니다. 각 복제본은 서로 다른 작업자 노드에 상주합니다. 남은 2개의 복제본은 보류 상태로 유지됩니다. 클러스터에 다른 작업자 노드를 추가하면 남은 복제본 중 하나가 자동으로 새 작업자 노드에 배치됩니다.<p>
    <p><strong>배치 YAML 파일 예</strong>:<ul>
    <li><a href="https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/nginx_preferredAntiAffinity.yaml" rel="external" target="_blank" title="(새 탭 또는 창에서 열림)">팟(Pod) 반친화성 선호를 사용하는 Nginx 앱</a></li>
    <li><a href="https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/liberty_requiredAntiAffinity.yaml" rel="external" target="_blank" title="(새 탭 또는 창에서 열림)">팟(Pod) 반친화성 필수를 사용하는 IBM® WebSphere® Application Server Liberty 앱</a></li></ul></p>
    </dd>
<dt>여러 위치 또는 지역 간에 팟(Pod) 분배</dt>
  <dd>위치 또는 지역 장애로부터 앱을 보호하기 위해 다른 위치 또는 지역에 두 번째 클러스터를 작성하고 배치 YAML을 사용하여 앱에 대한 중복 복제본 세트를 배치할 수 있습니다. 클러스터의 앞에 공유 라우트 및 로드 밸런서를 추가함으로써 위치 및 지역 간에 워크로드를 분배할 수 있습니다. 자세한 정보는 [클러스터의 고가용성](cs_clusters.html#clusters)을 참조하십시오.
  </dd>
</dl>


### 최소 앱 배치
{: #minimal_app_deployment}

무료 또는 표준 클러스터의 기본 앱 배치에는 다음 컴포넌트가 포함될 수 있습니다.
{: shortdesc}

![배치 설정](images/cs_app_tutorial_components1.png)

다이어그램에 표시된 대로 최소 앱을 위한 컴포넌트를 배치하려면 다음 예와 유사한 구성 파일을 사용합니다.
```
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: ibmliberty
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: ibmliberty
    spec:
      containers:
      - name: ibmliberty
        image: registry.bluemix.net/ibmliberty:latest
        ports:
        - containerPort: 9080        
---
apiVersion: v1
kind: Service
metadata:
  name: ibmliberty-service
  labels:
    app: ibmliberty
spec:
  selector:
    app: ibmliberty
  type: NodePort
  ports:
   - protocol: TCP
     port: 9080
```
{: codeblock}

**참고:** 서비스를 노출시키려면 서비스의 `spec.selector` 섹션에 사용하는 키/값 쌍이 배치 yaml의 `spec.template.metadata.labels` 섹션에 사용하는 키/값 쌍과 동일하도록 하십시오.
각 컴포넌트에 대해 자세히 보려면 [Kubernetes 기본](cs_tech.html#kubernetes_basics)을 검토하십시오.

<br />




## Kubernetes 대시보드 실행
{: #cli_dashboard}

로컬 시스템의 Kubernetes 대시보드를 열어서 클러스터 및 해당 작업자 노드에 대한 정보를 봅니다. [GUI에서](#db_gui) 편리한 한 번 클릭 단추를 사용하여 대시보드에 액세스할 수 있습니다. [CLI를 사용하여](#db_cli) 대시보드에 액세스하거나 CI/CD 파이프라인의 경우와 같은 자동화 프로세스의 단계를 사용할 수 있습니다.
{:shortdesc}

시작하기 전에 클러스터를 [CLI의 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오. 이 태스크에는 [관리자 액세스 정책](cs_users.html#access_policies)이 필요합니다. 현재 [액세스 정책](cs_users.html#infra_access)을 확인하십시오.

기본 포트를 사용하거나 자체 포트를 설정하여 클러스터에 대한 Kubernetes 대시보드를 실행할 수 있습니다.

**GUI에서 Kubernetes 대시보드 실행**
{: #db_gui}

1.  [{{site.data.keyword.Bluemix_notm}} GUI](https://console.bluemix.net/)에 로그인하십시오.
2.  메뉴 표시줄의 프로파일에서 사용할 계정을 선택하십시오.
3.  메뉴에서 **컨테이너**를 클릭하십시오.
4.  **클러스터** 페이지에서 액세스하려는 클러스터를 클릭하십시오.
5.  클러스터 세부사항 페이지에서 **Kubernetes 대시보드** 단추를 클릭하십시오.

**CLI에서 Kubernetes 대시보드 실행**
{: #db_cli}

*  Kubernetes 마스터 버전 1.7.16 이하가 있는 클러스터의 경우:

    1.  기본 포트 번호로 프록시를 설정하십시오.

        ```
        kubectl proxy
        ```
        {: pre}

        출력:

        ```
        Starting to serve on 127.0.0.1:8001
        ```
        {: screen}

    2.  웹 브라우저에서 Kubernetes 대시보드를 여십시오.

        ```
        http://localhost:8001/ui
        ```
        {: codeblock}

*  Kubernetes 마스터 버전 1.8.2 이상이 있는 클러스터의 경우:

    1.  Kubernetes 신임 정보를 가져오십시오.

        ```
        kubectl config view -o jsonpath='{.users[0].user.auth-provider.config.id-token}'
        ```
        {: pre}

    2.  출력에 표시되는 **id-token** 값을 복사하십시오.

    3.  기본 포트 번호로 프록시를 설정하십시오.

        ```
        kubectl proxy
        ```
        {: pre}

        출력 예:

        ```
        Starting to serve on 127.0.0.1:8001
        ```
        {: screen}

    4.  대시보드에 로그인하십시오.

      1.  브라우저에서 다음 URL로 이동하십시오.

          ```
            http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/
          ```
          {: codeblock}

      2.  사인온 페이지에서 **토큰** 인증 방법을 선택하십시오.

      3.  그런 다음 이전에 복사한 **id-token** 값을 **토큰** 필드에 붙여넣고 **로그인**을 클릭하십시오.

Kubernetes 대시보드에서 작업이 완료되면 `CTRL+C`를 사용하여 `proxy` 명령을 종료하십시오. 종료한 후에는 Kubernetes 대시보드를 더 이상 사용할 수 없습니다. `proxy` 명령을 실행하여 Kubernetes 대시보드를 다시 시작하십시오.

[그런 다음 대시보드에서 구성 파일을 실행할 수 있습니다.](#app_ui)


<br />




## 시크릿 작성
{: #secrets}

Kubernetes 시크릿은 사용자 이름, 비밀번호 또는 키와 같은 기밀 정보를 저장하는 안전한 방법입니다.
{:shortdesc}

<table>
<caption>태스크별 시크릿에 저장할 필수 파일</caption>
<thead>
<th>태스크</th>
<th>시크릿에 저장할 필수 파일</th>
</thead>
<tbody>
<tr>
<td>클러스터에 서비스 추가</td>
<td>없음. 클러스터에 서비스를 바인딩할 때 시크릿이 작성됩니다.</td>
</tr>
<tr>
<td>선택사항: Ingress 시크릿을 사용하지 않는 경우 TLS를 사용하는 Ingress 서비스를 구성하십시오. <p><b>참고</b>: TLS는 기본적으로 이미 사용 가능하며 시크릿도 TLS 연결을 위해 이미 작성되어 있습니다.

기본 TLS 시크릿을 보려면 다음을 수행하십시오.
<pre>
bx cs cluster-get &lt;cluster_name_or_ID&gt; | grep "Ingress secret"
</pre>
</p>
대신 고유 시크릿을 작성하려면 이 주제의 단계를 완료하십시오.</td>
<td>서버 인증서 및 키: <code>server.crt</code> 및 <code>server.key</code></td>
<tr>
<td>상호 인증 어노테이션을 작성하십시오.</td>
<td>CA 인증서: <code>ca.crt</code></td>
</tr>
</tbody>
</table>

시크릿에 저장할 수 있는 항목에 대한 자세한 정보는 [Kubernetes 문서](https://kubernetes.io/docs/concepts/configuration/secret/)를 참조하십시오.



인증서로 시크릿을 작성하려면 다음을 수행하십시오.

1. 인증 제공자로부터 인증 기관(CA) 인증서 및 키를 생성하십시오. 고유 도메인이 있는 경우 도메인의 공식적 TLS 인증서를 구매하십시오. 테스트용으로 자체 서명 인증서를 생성할 수 있습니다.

 **중요**: [CN](https://support.dnsimple.com/articles/what-is-common-name/)이 인증서마다 다른지 확인하십시오.

 클라이언트 인증서 및 클라이언트 키는 신뢰할 수 있는 루트 인증서(이 경우 CA 인증서)까지 확인해야 합니다. 예:

 ```
 Client Certificate: issued by Intermediate Certificate
 Intermediate Certificate: issued by Root Certificate
 Root Certificate: issued by itself
 ```
 {: codeblock}

2. 인증서를 Kubernetes 시크릿으로 작성하십시오.

   ```
   kubectl create secret generic <secret_name> --from-file=<cert_file>=<cert_file>
   ```
   {: pre}

   예:
   - TLS 연결:

     ```
     kubectl create secret tls <secret_name> --from-file=tls.crt=server.crt --from-file=tls.key=server.key
     ```
     {: pre}

   - 상호 인증 어노테이션:

     ```
     kubectl create secret generic <secret_name> --from-file=ca.crt=ca.crt
     ```
     {: pre}

<br />





## GUI로 앱 배치
{: #app_ui}

Kubernetes 대시보드를 사용하여 클러스터에 앱을 배치하는 경우, 배치 리소스가 자동으로 클러스터에서 팟(Pod)을 작성하고 업데이트 및 관리합니다.
{:shortdesc}

시작하기 전에:

-   필수 [CLI](cs_cli_install.html#cs_cli_install)를 설치하십시오.
-   클러스터를 [CLI의 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오.

앱을 배치하려면 다음을 수행하십시오.

1.  Kubernetes [대시보드](#cli_dashboard)를 열고 **+ 작성**을 클릭하십시오.
2.  다음 두 방법 중 하나로 앱 세부사항을 입력하십시오.
  * **아래의 앱 세부사항 지정**을 선택하고 세부사항을 입력하십시오.
  * **YAML 또는 JSON 파일 업로드**를 선택하여 앱 [구성 파일 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/)을 업로드하십시오.

  구성 파일에 대해 도움이 필요하십니까? 이 [YAML 파일 예 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/IBM-Cloud/kube-samples/blob/master/deploy-apps-clusters/deploy-ibmliberty.yaml)를 참조하십시오. 이 예에서는 미국 남부 지역의 **ibmliberty** 이미지로부터 컨테이너가 배치됩니다. Kubernetes 리소스에 대해 작업할 때 [개인 정보 보호](cs_secure.html#pi)에 대해 자세히 알아보십시오.
  {: tip}

3.  다음 방법 중 하나를 사용하여 앱이 배치되었는지 확인하십시오.
  * Kubernetes 대시보드에서 **배치**를 클릭하십시오. 성공한 배치의 목록이 표시됩니다.
  * 앱이 [공용으로 사용 가능](cs_network_planning.html#public_access)한 경우에는 {{site.data.keyword.containerlong}} 대시보드의 클러스터 개요 페이지로 이동하십시오. 클러스터 요약 섹션에 있는 하위 도메인을 복사한 후 브라우저에 붙여넣어 앱을 보십시오.

<br />


## CLI로 앱 배치
{: #app_cli}

클러스터가 작성된 후에 Kubernetes CLI를 사용하여 해당 클러스터에 앱을 배치할 수 있습니다.
{:shortdesc}

시작하기 전에:

-   필수 [CLI](cs_cli_install.html#cs_cli_install)를 설치하십시오.
-   클러스터를 [CLI의 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오.

앱을 배치하려면 다음을 수행하십시오.

1.  [Kubernetes 우수 사례 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/configuration/overview/)를 기반으로 구성 파일을 작성하십시오. 일반적으로, 구성 파일에는 Kubernetes에서 작성 중인 각각의 리소스에 대한 구성 세부사항이 포함되어 있습니다. 스크립트에는 하나 이상의 다음 섹션이 포함될 수 있습니다.

    -   [배치 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/): 팟(Pod)과 복제본 세트 작성을 정의합니다. 팟(Pod)에는 개별 컨테이너화된 앱이 포함되며, 복제본 세트는 팟(Pod)의 다중 인스턴스를 제어합니다.

    -   [서비스 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/services-networking/service/): 작업자 노드나 로드 밸런서 공인 IP 주소 또는 공용 Ingress 라우트를 사용하여 팟(Pod)에 프론트 엔드 액세스를 제공합니다.

    -   [Ingress ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/services-networking/ingress/): 공개적으로 앱에 액세스하는 라우트를 제공하는 로드 밸런서 유형을 지정합니다.

    Kubernetes 리소스에 대해 작업할 때 [개인 정보 보호](cs_secure.html#pi)에 대해 자세히 알아보십시오.

2.  클러스터의 컨텍스트에서 구성 파일을 실행하십시오.

    ```
    kubectl apply -f config.yaml
    ```
    {: pre}

3.  노드 포트 서비스, 로드 밸런서 서비스 또는 Ingress를 사용하여 앱을 공용으로 사용 가능하게 한 경우 앱에 액세스할 수 있는지 확인하십시오.

<br />




## GPU 머신에 앱 배치
{: #gpu_app}

[베어메탈 그래픽 처리 장치(GPU) 머신 유형](cs_clusters.html#shared_dedicated_node)이 있는 경우 작업자 노드에 수학적으로 집약적인 워크로드를 스케줄할 수 있습니다. 예를 들어, 성능 향상을 위해 CUDA(Compute Unified Device Architecture) 플랫폼을 사용하여 GPU 및 CPU에서 처리 로드를 공유하는 3D 앱을 실행할 수 있습니다.
{:shortdesc}

다음 단계에서는 GPU가 필요한 워크로드를 배치하는 방법에 대해 알아봅니다. 또한 GPU 및 CPU 모두에서 워크로드를 처리할 필요가 없는 [앱을 배치](#app_ui)할 수도 있습니다. 이후 [이 Kubernetes 데모 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/pachyderm/pachyderm/tree/master/doc/examples/ml/tensorflow)를 통해 [TensorFlow ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.tensorflow.org/) 기계 학습 프레임워크와 같은 수학적으로 집약적인 워크로드를 살펴보는 것이 유용할 수 있습니다.

시작하기 전에:
* [베어메탈 GPU 머신 유형을 작성](cs_clusters.html#clusters_cli)하십시오. 이 프로세스를 완료하는 데 1영업일 이상이 걸릴 수 있습니다.
* 클러스터 마스터와 GPU 작업자 노드가 Kubernetes 버전 1.10 이상에서 실행되어야 합니다.

GPU 머신에서 워크로드를 실행하려면 다음을 수행하십시오.
1.  YAML 파일을 작성하십시오. 이 예에서는 `Job` YAML이 완료하도록 스케줄된 명령이 성공적으로 종료될 때까지 실행되는 일시적인 포드를 작성하여 일괄처리와 유사한 워크로드를 관리합니다. 

    **중요**: GPU 워크로드의 경우 항상 YAML 스펙에 `resources: limits: nvidia.com/gpu` 필드를 제공해야 합니다.

    ```yaml
    apiVersion: batch/v1
    kind: Job
    metadata:
      name: nvidia-smi
      labels:
        name: nvidia-smi
    spec:
      template:
        metadata:
          labels:
            name: nvidia-smi
        spec:
          containers:
          - name: nvidia-smi
            image: nvidia/cuda:9.1-base-ubuntu16.04
            command: [ "/usr/test/nvidia-smi" ]
            imagePullPolicy: IfNotPresent
            resources:
              limits:
                nvidia.com/gpu: 2
            volumeMounts:
            - mountPath: /usr/test
              name: nvidia0
          volumes:
            - name: nvidia0
              hostPath:
                path: /usr/bin
          restartPolicy: Never
    ```
    {: codeblock}

    <table>
    <caption>YAML 컴포넌트</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td>메타데이터 및 레이블 이름</td>
    <td>작업의 이름과 레이블을 지정하고 파일의 메타데이터 및 `spec template` 메타데이터 모두에서 동일한 이름을 사용하십시오(예: `nvidia-smi`).</td>
    </tr>
    <tr>
    <td><code>containers/image</code></td>
    <td>컨테이너가 실행 중인 인스턴스인 이미지를 제공하십시오. 이 예에서는 이 값이 DockerHub CUDA 이미지를 사용하도록 설정됩니다(<code>nvidia/cuda:9.1-base-ubuntu16.04</code>).</td>
    </tr>
    <tr>
    <td><code>containers/command</code></td>
    <td>컨테이너에서 실행할 명령을 지정하십시오. 이 예에서는 <code>[ "/usr/test/nvidia-smi" ]</code> 명령이 GPU 머신에 있는 바이너리를 참조하므로 볼륨 마운트도 설정해야 합니다.</td>
    </tr>
    <tr>
    <td><code>containers/imagePullPolicy</code></td>
    <td>이미지가 현재 작업자 노드에 있지 않은 경우에만 새 이미지를 가져오려면 <code>IfNotPresent</code>를 지정하십시오.</td>
    </tr>
    <tr>
    <td><code>resources/limits</code></td>
    <td>GPU 머신의 경우 자원 한계를 지정해야 합니다. Kubernetes [디바이스 플러그인 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/cluster-administration/device-plugins/)이 한계와 일치하도록 기본 리소스 요청을 설정합니다.
    <ul><li>키를 <code>nvidia.com/gpu</code>로 지정해야 합니다.</li>
    <li>요청하는 GPU의 정수(예: <code>2</code>)를 입력하십시오. <strong>참고</strong>: 컨테이너 팟(Pod)은 GPU를 공유하지 않으며 GPU가 과다 할당될 수 없습니다. 예를 들어, 1개의 `mg1c.16x128` 머신만 있는 경우 해당 머신에는 2개의 GPU만 있으며 최대 `2`를 지정할 수 있습니다.</li></ul></td>
    </tr>
    <tr>
    <td><code>volumeMounts</code></td>
    <td>컨테이너에 마운트된 볼륨의 이름을 지정하십시오(예: <code>nvidia0</code>). 컨테이너에 볼륨의 <code>mountPath</code>를 지정하십시오. 이 예에서는 <code>/usr/test</code> 경로가 작업 컨테이너 명령에서 사용되는 경로와 일치합니다.</td>
    </tr>
    <tr>
    <td><code>volumes</code></td>
    <td>작업 볼륨의 이름을 지정하십시오(예: <code>nvidia0</code>). GPU 작업자 노드의 <code>hostPath</code>에서 호스트에 있는 볼륨의 <code>path</code>를 지정하십시오(이 예에서는 <code>/usr/bin</code>). <code>mountPath</code> 컨테이너가 호스트 볼륨 <code>path</code>에 맵핑되며, 이를 통해 이 작업에 컨테이너 명령이 실행될 GPU 작업자 노드의 NVIDIA 바이너리에 대한 액세스 권한이 부여됩니다.</td>
    </tr>
    </tbody></table>

2.  YAML 파일을 적용하십시오. 예를 들어, 다음과 같습니다.

    ```
    kubectl apply -f nvidia-smi.yaml
    ```
    {: pre}

3.  팟(Pod)을 `nvidia-sim` 레이블별로 필터링하여 작업 팟(Pod)을 확인하십시오. **STATUS**가 **Completed**인지 확인하십시오.

    ```
    kubectl get pod -a -l 'name in (nvidia-sim)'
    ```
    {: pre}

    출력 예:
    ```
    NAME                  READY     STATUS      RESTARTS   AGE
    nvidia-smi-ppkd4      0/1       Completed   0          36s
    ```
    {: screen}

4.  GPU 디바이스 플러그인이 팟(Pod)을 스케줄한 방법을 알 수 있도록 팟(Pod)에 대해 설명하십시오.
    * `Limits` 및 `Requests` 필드에서 사용자가 지정한 리소스 한계가 디바이스 플러그인에서 자동으로 설정한 요청과 일치하는지 확인하십시오. 
    * 이벤트에서 팟(Pod)이 GPU 작업자 노드에 지정되었는지 확인하십시오.

    ```
    kubectl describe pod nvidia-smi-ppkd4
    ```
    {: pre}

    출력 예:
    ```
    Name:           nvidia-smi-ppkd4
    Namespace:      default
    ...
    Limits:
     nvidia.com/gpu:  2
    Requests:
     nvidia.com/gpu:  2
    ...
    Events:
    Type    Reason                 Age   From                     Message
    ----    ------                 ----  ----                     -------
    Normal  Scheduled              1m    default-scheduler        Successfully assigned nvidia-smi-ppkd4 to 10.xxx.xx.xxx
    ...
    ```
    {: screen}

5.  로그를 확인하여 작업이 GPU를 사용하여 해당 워크로드를 계산했는지 확인할 수 있습니다. 작업의 `[ "/usr/test/nvidia-smi" ]` 명령이 GPU 작업자 노드의 GPU 디바이스 상태를 조회했습니다.

    ```
    kubectl logs nvidia-sim-ppkd4
    ```
    {: pre}

    출력 예:
    ```
    +-----------------------------------------------------------------------------+
    | NVIDIA-SMI 390.12                 Driver Version: 390.12                    |
    |-------------------------------+----------------------+----------------------+
    | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
    | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
    |===============================+======================+======================|
    |   0  Tesla K80           Off  | 00000000:83:00.0 Off |                  Off |
    | N/A   37C    P0    57W / 149W |      0MiB / 12206MiB |      0%      Default |
    +-------------------------------+----------------------+----------------------+
    |   1  Tesla K80           Off  | 00000000:84:00.0 Off |                  Off |
    | N/A   32C    P0    63W / 149W |      0MiB / 12206MiB |      1%      Default |
    +-------------------------------+----------------------+----------------------+

    +-----------------------------------------------------------------------------+
    | Processes:                                                       GPU Memory |
    |  GPU       PID   Type   Process name                             Usage      |
    |=============================================================================|
    |  No running processes found                                                 |
    +-----------------------------------------------------------------------------+
    ```
    {: screen}

    이 예에서는 두 개의 GPU가 모두 작업자 노드에서 스케줄되었으므로 두 개의 GPU가 작업을 실행하는 데 사용되었음을 알 수 있습니다. 한계가 1로 설정된 경우 하나의 GPU만 표시됩니다.

## 앱 스케일링 
{: #app_scaling}

Kubernetes에서는 [수평 팟(Pod) Auto-Scaling ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/)을 사용하여 CPU를 기반으로 앱의 인스턴스 수를 늘리거나 줄일 수 있습니다.
{:shortdesc}

Cloud Foundry 애플리케이션 스케일링에 대한 정보를 찾으십니까? [{{site.data.keyword.Bluemix_notm}}에 대한 IBM Auto-Scaling](/docs/services/Auto-Scaling/index.html)을 확인하십시오. 
{: tip}

시작하기 전에:
- 클러스터를 [CLI의 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오.
- heapster 모니터링은 Auto-Scaling을 수행할 클러스터에 배치되어야 합니다.

단계:

1.  CLI에서 클러스터에 앱을 배치하십시오. 앱을 배치할 때는 CPU를 요청해야 합니다.

    ```
    kubectl run <app_name> --image=<image> --requests=cpu=<cpu> --expose --port=<port_number>
    ```
    {: pre}

    <table>
    <caption>kubectl run에 대한 명령 컴포넌트</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 이 명령의 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--image</code></td>
    <td>배치하려는 애플리케이션입니다.</td>
    </tr>
    <tr>
    <td><code>--request=cpu</code></td>
    <td>컨테이너의 필수 CPU(밀리 코어 단위로 지정됨)입니다. 예: <code>--requests=200m</code>.</td>
    </tr>
    <tr>
    <td><code>--expose</code></td>
    <td>true인 경우, 외부 서비스를 작성합니다.</td>
    </tr>
    <tr>
    <td><code>--port</code></td>
    <td>외부에서 앱이 사용 가능한 포트입니다.</td>
    </tr></tbody></table>

    보다 복잡한 배치를 위해서는 [구성 파일](#app_cli)을 작성해야 할 수 있습니다.
    {: tip}

2.  Autoscaler를 작성하고 정책을 정의하십시오. `kubectl autoscale` 명령을 사용한 작업에 대한 자세한 정보는 [Kubernetes 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://v1-8.docs.kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#autoscale)를 참조하십시오.

    ```
    kubectl autoscale deployment <deployment_name> --cpu-percent=<percentage> --min=<min_value> --max=<max_value>
    ```
    {: pre}

    <table>
    <caption>kubectl autoscale에 대한 명령 컴포넌트</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 이 명령의 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--cpu-percent</code></td>
    <td>Horizontal Pod Autoscaler에서 유지하는 평균 CPU 사용률(백분율로 지정됨)입니다.</td>
    </tr>
    <tr>
    <td><code>--min</code></td>
    <td>지정된 CPU 사용 백분율을 유지하기 위해 사용되는 배치된 팟(Pod)의 최소 수입니다.</td>
    </tr>
    <tr>
    <td><code>--max</code></td>
    <td>지정된 CPU 사용 백분율을 유지하기 위해 사용되는 배치된 팟(Pod)의 최대 수입니다.</td>
    </tr>
    </tbody></table>


<br />


## 롤링 배치 관리
{: #app_rolling}

자동화되고 제어 가능한 방식으로 변경사항의 롤아웃을 관리할 수 있습니다. 롤아웃이 계획대로 진행되지 않으면 배치를 이전 개정으로 롤백할 수 있습니다.
{:shortdesc}

시작하기 전에 [배치](#app_cli)를 작성하십시오.

1.  변경사항을 [롤아웃 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#updating-a-deployment)하십시오. 예를 들어, 초기 배치에 사용된 이미지를 변경할 수 있습니다.

    1.  배치 이름을 가져오십시오.

        ```
         kubectl get deployments
        ```
        {: pre}

    2.  팟(Pod) 이름을 가져오십시오.

        ```
            kubectl get pods
        ```
        {: pre}

    3.  팟(Pod)에서 실행 중인 컨테이너의 이름을 가져오십시오.

        ```
        kubectl describe pod <pod_name>
        ```
        {: pre}

    4.  사용할 배치에 대해 새 이미지를 설정하십시오.

        ```
        kubectl set image deployment/<deployment_name><container_name>=<image_name>
        ```
        {: pre}

    명령을 실행하면 변경사항이 즉시 적용되며 롤아웃 히스토리에 로깅됩니다.

2.  배치의 상태를 확인하십시오.

    ```
    kubectl rollout status deployments/<deployment_name>
    ```
    {: pre}

3.  변경사항을 롤백하십시오.
    1.  배치의 롤아웃 히스토리를 보고 마지막 배치의 개정 번호를 식별하십시오.

        ```
        kubectl rollout history deployment/<deployment_name>
        ```
        {: pre}

        **팁:** 특정 개정에 대한 세부사항을 보려면 개정 번호를 포함하십시오.

        ```
        kubectl rollout history deployment/<deployment_name> --revision=<number>
        ```
        {: pre}

    2.  이전 버전으로 롤백하거나 개정을 지정하십시오. 이전 버전으로 롤백하려면 다음 명령을 사용하십시오.

        ```
        kubectl rollout undo deployment/<depoyment_name> --to-revision=<number>
        ```
        {: pre}

<br />

