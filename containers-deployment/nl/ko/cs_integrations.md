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




# 서비스 통합
{: #integrations}

{{site.data.keyword.containerlong}}의 표준 Kubernetes 클러스터에서 다양한 외부 서비스와 카탈로그 서비스를 사용할 수 있습니다.
{:shortdesc}


## 애플리케이션 서비스
<table summary="접근성 요약">
<caption>애플리케이션 서비스</caption>
<thead>
<tr>
<th>서비스</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.blockchainfull}}</td>
<td>IBM Blockchain에 공용으로 사용 가능한 배치 환경을 {{site.data.keyword.containerlong_notm}}의 Kubernetes 클러스터에 배치하십시오. 트랜잭션 히스토리를 기록하기 위해 불변 원장을 공유하는 앱을 배치하려면 이 환경을 사용하여 고유 블록체인 네트워크를 개발하여 사용자 정의하십시오. 자세한 정보는 <a href="https://ibm-blockchain.github.io" target="_blank"> 클라우드 샌드박스
IBM Blockchain Platform에서 개발 <img src="../icons/launch-glyph.svg" alt="외부 링크 문서"></a>을 참조하십시오. </td>
</tr>
</tbody>
</table>

<br />



## DevOps 서비스
<table summary="접근성 요약">
<caption>DevOps 서비스</caption>
<thead>
<tr>
<th>서비스</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Codeship</td>
<td>컨테이너의 지속적인 통합 및 전달을 위해 <a href="https://codeship.com" target="_blank">Codeship <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>을 사용할 수 있습니다. 자세한 정보는 <a href="https://www.ibm.com/blogs/bluemix/2017/10/using-codeship-pro-deploy-workloads-ibm-container-service/" target="_blank">Codeship Pro를 사용하여 {{site.data.keyword.containershort_notm}}에 워크로드 배치 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 참조하십시오. </td>
</tr>
<tr>
<td>Helm</td>
<td> <a href="https://helm.sh/" target="_blank">Helm <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>은 Kubernetes 패키지 관리자입니다. {{site.data.keyword.containerlong_notm}} 클러스터에서 실행되는 복잡한 Kubernetes 애플리케이션을 정의, 설치 및 업그레이드하기 위해 새 Helm 차트를 작성하거나 기존 Helm 차트를 사용할 수 있습니다. <p>자세한 정보는 [{{site.data.keyword.containershort_notm}}에서 Helm 설정](cs_integrations.html#helm)을 참조하십시오.</p></td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>도구 체인을 사용하여 Kubernetes 클러스터에 대한 컨테이너 배치 및 앱 빌드를 자동화합니다. 설정 정보는 <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">DevOps 파이프라인을 사용하여 {{site.data.keyword.containerlong_notm}}에 Kubernetes 팟(Pod) 배치 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a> 블로그를 참조하십시오. </td>
</tr>
<tr>
<td>Istio</td>
<td><a href="https://www.ibm.com/cloud/info/istio" target="_blank">Istio<img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>는 Kubernetes와 같은 클라우드 오케스트레이션 플랫폼에서 서비스 메시(service mesh)로도 알려진 마이크로서비스의 네트워크에 연결하고, 보안, 관리 및 모니터 방법을 개발자에게 제공하는 오픈 소스 서비스입니다. 오픈 소스 프로젝트에 대한 자세한 내용은 <a href="https://developer.ibm.com/dwblog/2017/istio/" target="_blank">IBM이 공동 설립하고 시작한 Istio<img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>에 대한 블로그 게시물을 확인하십시오. {{site.data.keyword.containershort_notm}}에서 Kubernetes 클러스터에 Istio를 설치하고 샘플 앱을 시작하려면 [튜토리얼: Istio로 마이크로서비스 관리](cs_tutorials_istio.html#istio_tutorial)를 참조하십시오.</td>
</tr>
</tbody>
</table>

<br />



## 서비스 로깅 및 모니터링
<table summary="접근성 요약">
<caption>서비스 로깅 및 모니터링</caption>
<thead>
<tr>
<th>서비스</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>CoScale</td>
<td><a href="https://www.coscale.com/" target="_blank">CoScale <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>을 사용하여 작업자 노드, 컨테이너, 복제본 세트 및 서비스를 모니터합니다. 자세한 정보는 <a href="https://www.ibm.com/blogs/bluemix/2017/06/monitoring-ibm-bluemix-container-service-coscale/" target="_blank">CoScale을 사용하여 {{site.data.keyword.containershort_notm}} 모니터링 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>을 참조하십시오. </td>
</tr>
<tr>
<td>Datadog</td>
<td><a href="https://www.datadoghq.com/" target="_blank">Datadog <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 사용하여 클러스터를 모니터하고 인프라 및 애플리케이션 성능 메트릭을 봅니다. 자세한 정보는 <a href="https://www.ibm.com/blogs/bluemix/2017/07/monitoring-ibm-bluemix-container-service-datadog/" target="_blank">Datadog를 사용하여 {{site.data.keyword.containershort_notm}} 모니터링 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>을 참조하십시오. </td>
</tr>
<tr>
<td>{{site.data.keyword.loganalysisfull}}</td>
<td>{{site.data.keyword.loganalysisfull_notm}}를 사용하여 로그 콜렉션, 보존 및 검색 기능을 확장합니다. 자세한 정보는 <a href="../services/CloudLogAnalysis/containers/containers_kube_other_logs.html" target="_blank">클러스터 로그의 자동 콜렉션 사용 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>을 참조하십시오. </td>
</tr>
<tr>
<td>{{site.data.keyword.monitoringlong}}</td>
<td>{{site.data.keyword.monitoringlong_notm}}으로 규칙 및 경보를 정의하여 메트릭 콜렉션 및 보존 기능을 확장합니다. 자세한 정보는 <a href="../services/cloud-monitoring/tutorials/container_service_metrics.html" target="_blank">Kubernetes 클러스터에 배치된 앱에 대한 Grafana의 메트릭 분석 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>을 참조하십시오. </td>
</tr>
<tr>
<td>Instana</td>
<td> <a href="https://www.instana.com/" target="_blank">Instana <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>는 인프라 및 앱 성능 모니터링에 자동으로 앱을 발견하고 맵핑하는 GUI를 제공합니다. Istana는 앱에 대한 모든 요청을 캡처하며 문제점을 해결하고 근본 원인 분석을 수행하여 문제점이 다시 발생하지 않도록 방지하는 데 사용할 수 있습니다. 자세한 내용은 <a href="https://www.instana.com/blog/precise-visibility-applications-ibm-bluemix-container-service/" target="_blank">{{site.data.keyword.containershort_notm}}에 Istana 배치 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>에 대한 블로그 게시물을 확인하십시오.</td>
</tr>
<tr>
<td>Prometheus</td>
<td>Prometheus는 특별히 Kubernetes용으로 설계된 오픈 소스 모니터링, 로깅 및 경보 도구입니다. Prometheus는 Kubernetes 로깅 정보를 기반으로 클러스터, 작업자 노드 및 배치 상태에 대한 자세한 정보를 검색합니다. 클러스터에서 실행 중인 각 컨테이너에 대한 CPU, 메모리, I/O 및 네트워크 활동이 수집됩니다. 사용자 정의 조회 또는 경보에서 수집된 데이터를 사용하여 클러스터의 성능 및 워크로드를 모니터할 수 있습니다.

<p>Prometheus를 사용하려면 <a href="https://github.com/coreos/prometheus-operator/tree/master/contrib/kube-prometheus" target="_blank">CoreOS 지시사항 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>을 따르십시오.</p>
</td>
</tr>
<tr>
<td>Sematext</td>
<td><a href="https://sematext.com/" target="_blank">Sematext <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 사용하여 컨테이너화된 애플리케이션에 대한 메트릭 및 로그를 봅니다. 자세한 정보는 <a href="https://www.ibm.com/blogs/bluemix/2017/09/monitoring-logging-ibm-bluemix-container-service-sematext/" target="_blank">Sematext를 사용하여 컨테이너에 대해 모니터링 및 로깅 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>을 참조하십시오. </td>
</tr>
<tr>
<td>Sysdig</td>
<td><a href="https://sysdig.com/" target="_blank">Sysdig <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 사용하여 단일 인스트루먼테이션 지점으로 앱, 컨테이너, statsd 및 호스트 메트릭을 캡처합니다. 자세한 정보는 <a href="https://www.ibm.com/blogs/bluemix/2017/08/monitoring-ibm-bluemix-container-service-sysdig-container-intelligence/" target="_blank">Sysdig Container Intelligence를 사용하여 {{site.data.keyword.containershort_notm}} 모니터링 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>을 참조하십시오. </td>
</tr>
<tr>
<td>Weave Scope</td>
<td>Weave Scope는 서비스, 팟(Pod), 컨테이너, 프로세스, 노드 등을 포함하여 Kubernetes 클러스터 내의 리소스에 대한 시각적 다이어그램을 제공합니다. Weave Scope는 CPU 및 메모리에 대한 대화식 메트릭을 제공하며 컨테이너로 tail 및 exec를 실행하기 위한 도구도 제공합니다.<p>자세한 정보는 [Weave Scope 및 {{site.data.keyword.containershort_notm}}를 사용하여 Kubernetes 클러스터 리소스 시각화](cs_integrations.html#weavescope)를 참조하십시오.</p></li></ol>
</td>
</tr>
</tbody>
</table>

<br />



## 보안 서비스
<table summary="접근성 요약">
<caption>보안 서비스</caption>
<thead>
<tr>
<th>서비스</th>
<th>설명</th>
</tr>
</thead>
<tbody>
  <tr id="appid">
    <td>{{site.data.keyword.appid_full}}</td>
    <td>사용자가 로그인하도록 하여 [{{site.data.keyword.appid_short}}](/docs/services/appid/index.html#gettingstarted)를 통해 앱에 보안 레벨을 추가합니다. 앱에 웹 또는 API HTTP/HTTPS 요청을 인증하기 위해 [{{site.data.keyword.appid_short_notm}} 인증 Ingress 어노테이션](cs_annotations.html#appid-auth)을 사용하여 {{site.data.keyword.appid_short_notm}}를 Ingress 서비스와 통합할 수 있습니다.</td>
  </tr>
<tr>
<td>Aqua Security</td>
  <td><a href="/docs/services/va/va_index.html" target="_blank">Vulnerability Advisor</a>를 보완하여 <a href="https://www.aquasec.com/" target="_blank">Aqua Security <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 사용하면 앱에서 수행하도록 허용되는 항목을 줄여서 컨테이너 배치의 보안을 향상시킬 수 있습니다. 자세한 정보는 <a href="https://www.ibm.com/blogs/bluemix/2017/06/protecting-container-deployments-bluemix-aqua-security/" target="_blank">Aqua Security를 사용하여 {{site.data.keyword.Bluemix_notm}}의 컨테이너 배치 보호 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 참조하십시오. </td>
</tr>
<tr>
<td>{{site.data.keyword.cloudcerts_full}}</td>
<td><a href="../services/certificate-manager/index.html" target="_blank">{{site.data.keyword.cloudcerts_long}} <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 사용하여 앱에 대한 SSL 인증서를 저장하고 관리할 수 있습니다. 자세한 정보는 <a href="https://www.ibm.com/blogs/bluemix/2018/01/use-ibm-cloud-certificate-manager-ibm-cloud-container-service-deploy-custom-domain-tls-certificates/" target="_blank">{{site.data.keyword.cloudcerts_long_notm}}를 {{site.data.keyword.containershort_notm}}와 함께 사용하여 사용자 정의 도메인 TLS 인증서 배치 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 참조하십시오. </td>
</tr>
<tr>
  <td>{{site.data.keyword.registrylong}}</td>
  <td>클러스터 사용자 간에 이미지를 안전하게 저장하고 공유할 수 있는 보안된 Docker 이미지 저장소를 설정하십시오. 자세한 정보는 <a href="/docs/services/Registry/index.html" target="_blank">{{site.data.keyword.registrylong}} 문서 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 참조하십시오.</td>
</tr>
<tr>
<td>NeuVector</td>
<td><a href="https://neuvector.com/" target="_blank">NeuVector <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 사용하여 클라우드 고유 방화벽으로 컨테이너를 보호합니다. 자세한 정보는 <a href="https://www.ibm.com/us-en/marketplace/neuvector-container-security" target="_blank">NeuVector Container Security <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 참조하십시오. </td>
</tr>
<tr>
<td>Twistlock</td>
<td><a href="/docs/services/va/va_index.html" target="_blank">Vulnerability Advisor</a>를 보완하여 <a href="https://www.twistlock.com/" target="_blank">Twistlock <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>을 사용하여 방화벽, 위협 방지 및 인시던트 응답을 관리할 수 있습니다. 자세한 정보는 <a href="https://www.ibm.com/blogs/bluemix/2017/07/twistlock-ibm-bluemix-container-service/" target="_blank">{{site.data.keyword.containershort_notm}}의 Twistlock <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>을 참조하십시오. </td>
</tr>
</tbody>
</table>

<br />



## 스토리지 서비스
<table summary="접근성 요약">
<caption>스토리지 서비스</caption>
<thead>
<tr>
<th>서비스</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Heptio Ark</td>
  <td><a href="https://github.com/heptio/ark" target="_blank">Heptio Ark <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 사용하여 클러스터 리소스 및 지속적 볼륨을 백업하고 복원할 수 있습니다. 자세한 정보는 Heptio Ark의 <a href="https://github.com/heptio/ark/blob/master/docs/use-cases.md#use-cases" target="_blank">재해 복구 및 클러스터 마이그레이션을 위한 유스 케이스 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 참조하십시오.</td>
</tr>
<tr>
  <td>{{site.data.keyword.cos_full}}</td>
  <td>{{site.data.keyword.cos_short}}에 저장된 데이터는 암호화되어 여러 지리적 위치에 분산되며, REST API를 사용하여 HTTP를 통해 액세스할 수 있습니다. [ibm-backup-restore 이미지](/docs/services/RegistryImages/ibm-backup-restore/index.html) 를 사용하여 클러스터 내의 데이터에 대해 일회성 또는 스케줄된 백업을 작성하도록 서비스를 구성할 수 있습니다. 이 서비스에 대한 일반 정보는 <a href="/docs/services/cloud-object-storage/about-cos.html" target="_blank">{{site.data.keyword.cos_short}} 문서 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 참조하십시오.</td>
</tr>
  <tr>
    <td>{{site.data.keyword.cloudantfull}}</td>
    <td>{{site.data.keyword.cloudant_short_notm}}는  데이터를 JSON 형식의 문서로 저장하는 문서 지향 DBaaS(DataBase as a Service)입니다. 이 서비스는 확장성, 고가용성 및 내구성을 제공하기 위해 제작되었습니다. 자세한 정보는 <a href="/docs/services/Cloudant/getting-started.html" target="_blank">{{site.data.keyword.cloudant_short_notm}} 문서 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 참조하십시오.</td>
  </tr>
  <tr>
    <td>{{site.data.keyword.composeForMongoDB_full}}</td>
    <td>{{site.data.keyword.composeForMongoDB}}는 고가용성 및 중복성, On-Demand 방식의 자동화된 비중지 백업, 모니터링 도구, 경보 시스템으로의 통합, 성능 분석 보기 등을 제공합니다. 자세한 정보는 <a href="/docs/services/ComposeForMongoDB/index.html" target="_blank">{{site.data.keyword.composeForMongoDB}} 문서 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 참조하십시오.</td>
  </tr>
</tbody>
</table>


<br />



## 클러스터에 Cloud Foundry 서비스 추가
{: #adding_cluster}

기존 Cloud Foundry 서비스 인스턴스를 클러스터에 추가하여 클러스터 사용자가 앱을 클러스터에 배치할 때 서비스에 액세스하고 이를 사용할 수 있게 합니다.
{:shortdesc}

시작하기 전에:

1. 클러스터를 [CLI의 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오.
2. [{{site.data.keyword.Bluemix_notm}} 서비스의 인스턴스를 요청](/docs/apps/reqnsi.html#req_instance)하십시오.
   **참고:** 워싱턴 DC 위치에 서비스 인스턴스를 작성하려면 CLI를 사용해야 합니다.
3. Cloud Foundry 서비스는 클러스터와 바인딩하는 데 지원되지만 기타 서비스는 지원되지 않습니다. 서비스 인스턴스를 작성하고 서비스가 **Cloud Foundry 서비스** 및 **서비스**로 대시보드에 그룹화된 후 다른 서비스 유형을 볼 수 있습니다. **서비스** 섹션의 서비스를 클러스터와 바인딩하려면 [먼저 Cloud Foundry 별명을 작성](#adding_resource_cluster)하십시오.

**참고:**
<ul><ul>
<li>서비스 키를 지원하는 {{site.data.keyword.Bluemix_notm}} 서비스만 추가할 수 있습니다. 서비스가 서비스 키를 지원하지 않는 경우 [외부 앱이 {{site.data.keyword.Bluemix_notm}} 서비스를 사용하도록 설정](/docs/apps/reqnsi.html#accser_external)을 참조하십시오.</li>
<li>서비스를 추가하기 전에 클러스터와 작업자 노드가 완전히 배치되어야 합니다.</li>
</ul></ul>


서비스를 추가하려면 다음을 수행하십시오.
2.  사용 가능한 {{site.data.keyword.Bluemix_notm}} 서비스를 나열하십시오.

    ```
     bx service list
    ```
    {: pre}

    CLI 출력 예:

    ```
    name                      service           plan    bound apps   last operation
    <service_instance_name>   <service_name>    spark                create succeeded
    ```
    {: screen}

3.  클러스터에 추가할 서비스 인스턴스의 **이름**을 기록해 두십시오.
4.  서비스를 추가하는 데 사용할 클러스터 네임스페이스를 식별하십시오. 다음 옵션 중에 선택하십시오.
    -   기존 네임스페이스를 나열하고 사용할 네임스페이스를 선택하십시오.

        ```
         kubectl get namespaces
        ```
        {: pre}

    -   클러스터에 네임스페이스를 작성하십시오.

        ```
        kubectl create namespace <namespace_name>
        ```
        {: pre}

5.  서비스를 클러스터에 추가하십시오.

    ```
    bx cs cluster-service-bind <cluster_name_or_ID> <namespace> <service_instance_name>
    ```
    {: pre}

    서비스가 클러스터에 정상적으로 추가되면 서비스 인스턴스의 신임 정보를 보유하는 클러스터 시크릿이 작성됩니다. CLI 출력 예:

    ```
    bx cs cluster-service-bind mycluster mynamespace cleardb 
    Binding service instance to namespace...
    OK
    Namespace: mynamespace
    Secret name:     binding-<service_instance_name>
    ```
    {: screen}

6.  시크릿이 클러스터 네임스페이스에서 작성되었는지 확인하십시오.

    ```
    kubectl get secrets --namespace=<namespace>
    ```
    {: pre}

팟(Pod)에서 클러스터에 배치된 서비스를 사용하려면 클러스터 사용자가 서비스 신임 정보에 액세스해야 합니다. 사용자는 [Kubernetes 시크릿을 시크릿 볼륨으로 팟(Pod)에 마운트하여](#adding_app) {{site.data.keyword.Bluemix_notm}} 서비스의 서비스 신임 정보에 액세스할 수 있습니다.

<br />


## 기타 {{site.data.keyword.Bluemix_notm}} 서비스 리소스에 대한 Cloud Foundry 별명 작성
{: #adding_resource_cluster}

Cloud Foundry 서비스는 클러스터와의 바인딩을 위해 지원됩니다. Cloud Foundry 서비스가 아닌 {{site.data.keyword.Bluemix_notm}} 서비스를 클러스터에 바인딩하려면 서비스 인스턴스에 대한 Cloud Foundry 별명을 작성하십시오.
{:shortdesc}

시작하기 전에 [{{site.data.keyword.Bluemix_notm}} 서비스의 인스턴스를 요청](/docs/apps/reqnsi.html#req_instance)하십시오.

서비스 인스턴스에 대한 Cloud Foundry 별명을 작성하려면 다음을 수행하십시오.

1. 서비스 인스턴스가 작성된 조직 및 영역을 대상으로 지정하십시오.

    ```
    bx target -o <org_name> -s <space_name>
    ```
    {: pre}

2. 서비스 인스턴스 이름을 기록해 두십시오.
    ```
    bx resource service-instances
    ```
    {: pre}

3. 서비스 인스턴스에 대한 Cloud Foundry 별명을 작성하십시오.
    ```
    bx resource service-alias-create <service_alias_name> --instance-name <service_instance>
    ```
    {: pre}

4. 서비스 별명이 작성되었는지 확인하십시오.

    ```
     bx service list
    ```
    {: pre}

5. [Cloud Foundry 별명을 클러스터에 바인딩](#adding_cluster)하십시오.



<br />


## 앱에 서비스 추가
{: #adding_app}

암호화된 Kubernetes 시크릿을 사용하여 {{site.data.keyword.Bluemix_notm}} 서비스 세부사항과 신임 정보를 저장하고 서비스와 클러스터 간의 보안 통신을 허용합니다.
{:shortdesc}

Kubernetes 시크릿은 사용자 이름, 비밀번호 또는 키와 같은 기밀 정보를 저장하는 안전한 방법입니다. 환경 변수를 통하거나 Dockerfile에서 직접 기밀 정보를 노출하는 대신 시크릿을 팟(Pod)에 마운트할 수 있습니다. 그런 다음, 팟(Pod)의 컨테이너를 실행하여 이 시크릿에 액세스할 수 있습니다.

시크릿 볼륨을 팟(Pod)에 마운트하면 `binding`이라는 파일이 볼륨 마운트 디렉토리에 저장됩니다. `binding` 파일에는 {{site.data.keyword.Bluemix_notm}} 서비스에 액세스하는 데 필요한 모든 정보와 신임 정보가 포함되어 있습니다.

시작하기 전에 클러스터를 [CLI의 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오. 앱에서 사용하려는 {{site.data.keyword.Bluemix_notm}} 서비스를 클러스터 관리자가 [클러스터에 추가](cs_integrations.html#adding_cluster)하게 하십시오.

1.  클러스터 네임스페이스에서 사용 가능한 시크릿을 나열하십시오.

    ```
    kubectl get secrets --namespace=<my_namespace>
    ```
    {: pre}

    출력 예:

    ```
    NAME                                    TYPE                                  DATA      AGE
    binding-<service_instance_name>         Opaque                                1         3m

    ```
    {: screen}

2.  **오파크** 유형의 시크릿을 찾아서 시크릿의 **이름**을 기록해 두십시오. 다수의 시크릿이 존재하면 클러스터 관리자에게 문의하여 올바른 서비스 시크릿을 식별하십시오.

3.  선호하는 편집기를 여십시오.

4.  시크릿 볼륨을 통해 서비스 세부사항에 액세스할 수 있는 팟(Pod)을 구성하기 위한 YAML 파일을 작성하십시오. 둘 이상의 서비스를 바인딩한 경우 각 시크릿이 올바른 서비스와 연관되었는지 확인하십시오.

    ```
    apiVersion: apps/v1beta1
    kind: Deployment
    metadata:
      labels:
        app: secret-test
      name: secret-test
      namespace: <my_namespace>
    spec:
      selector:
        matchLabels:
          app: secret-test
      replicas: 1
      template:
        metadata:
          labels:
            app: secret-test
        spec:
          containers:
          - image: nginx
            name: secret-test
            volumeMounts:
            - mountPath: /opt/service-bind
              name: service-bind-volume
          volumes:
          - name: service-bind-volume
            secret:
              defaultMode: 420
              secretName: binding-<service_instance_name>
    ```
    {: codeblock}

    <table>
    <caption>YAML 파일 컴포넌트 이해</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code>volumeMounts/mountPath</code></td>
    <td>컨테이너에 마운트할 시크릿 볼륨의 이름입니다.</td>
    </tr>
    <tr>
    <td><code>volumes/name</code></td>
    <td>컨테이너에 마운트할 시크릿 볼륨의 이름을 입력하십시오.</td>
    </tr>
    <tr>
    <td><code>secret/defaultMode</code></td>
    <td>서비스 시크릿에 대한 읽기 전용 권한을 설정하십시오.</td>
    </tr>
    <tr>
    <td><code>secret/secretName</code></td>
    <td>이전에 기록한 시크릿의 이름을 입력하십시오.</td>
    </tr></tbody></table>

5.  팟(Pod)을 작성하고 시크릿 볼륨을 마운트하십시오.

    ```
    kubectl apply -f secret-test.yaml
    ```
    {: pre}

6.  팟(Pod)이 작성되었는지 확인하십시오.

    ```
    kubectl get pods --namespace=<my_namespace>
    ```
    {: pre}

    CLI 출력 예:

    ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
    {: screen}

7.  팟(Pod)의 **이름**을 기록해 두십시오.
8.  팟(Pod)에 대한 세부사항을 가져오고 시크릿 이름을 찾으십시오.

    ```
    kubectl describe pod <pod_name>
    ```
    {: pre}

    출력:

    ```
    ...
    Volumes:
      service-bind-volume:
        Type:       Secret (a volume populated by a Secret)
        SecretName: binding-<service_instance_name>
    ...
    ```
    {: screen}

    

9.  마운트 디렉토리에서 `binding` 시크릿 파일을 찾고 JSON 컨텐츠를 구문 분석하며 {{site.data.keyword.Bluemix_notm}} 서비스에 액세스하기 위한 URL 및 서비스 신임 정보를 판별하도록 앱을 구성하십시오.

이제 {{site.data.keyword.Bluemix_notm}} 서비스 세부사항과 신임 정보에 액세스할 수 있습니다. {{site.data.keyword.Bluemix_notm}} 서비스에 대해 작업하려면 마운트 디렉토리에서 서비스 시크릿 파일을 찾아서 JSON 컨텐츠를 구문 분석하고 서비스 세부사항을 판별하도록 앱이 구성되어 있는지 확인하십시오.

<br />


## {{site.data.keyword.containershort_notm}}에서 Helm 설정
{: #helm}

[Helm ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://helm.sh/)은 Kubernetes 패키지 관리자입니다. {{site.data.keyword.containerlong_notm}} 클러스터에서 실행되는 복잡한 Kubernetes 애플리케이션을 정의, 설치 및 업그레이드하기 위해 Helm 차트를 작성하거나 기존 Helm 차트를 사용할 수 있습니다.
{:shortdesc}

{{site.data.keyword.containershort_notm}}에 Helm 차트를 사용하려면 먼저 클러스터에 Helm 인스턴스를 설치하고 초기화해야 합니다. 그런 다음, {{site.data.keyword.Bluemix_notm}} Helm 저장소를 Helm 인스턴스에 추가할 수 있습니다.

시작하기 전에 Helm 차트를 사용할 클러스터를 [CLI의 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오.

1. <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 설치하십시오.

2. **중요**: 클러스터 보안을 유지하려면 `kube-system` 네임스페이스에 tiller에 대한 서비스 계정을 작성하고 `tiller-deploy` 팟(Pod)에 대한 Kubernetes RBAC 클러스터 역할 바인딩을 작성하십시오.

    1. 선호하는 편집기에서 다음 파일을 작성하고 `rbac-config.yaml`로 저장하십시오.
      **참고**:
        * `cluster-admin` 클러스터 역할은 Kubernetes 클러스터에서 기본적으로 작성되므로 이를 명시적으로 정의할 필요는 없습니다.
        * 버전 1.7.x 클러스터를 사용하고 있는 경우에는 `apiVersion`을 `rbac.authorization.k8s.io/v1beta1`로 변경하십시오.

      ```
      apiVersion: v1
      kind: ServiceAccount
      metadata:
        name: tiller
        namespace: kube-system
      ---
      apiVersion: rbac.authorization.k8s.io/v1
      kind: ClusterRoleBinding
      metadata:
        name: tiller
      roleRef:
        apiGroup: rbac.authorization.k8s.io
        kind: ClusterRole
        name: cluster-admin
      subjects:
        - kind: ServiceAccount
          name: tiller
          namespace: kube-system
      ```
      {: codeblock}

    2. 서비스 계정 및 클러스터 역할 바인딩을 작성하십시오.

        ```
        kubectl create -f rbac-config.yaml
        ```
        {: pre}

3. Helm을 초기화하고 작성한 서비스 계정을 사용하여 `tiller`를 설치하십시오.

    ```
    helm init --service-account tiller
    ```
    {: pre}

4. `tiller-deploy` 팟(Pod)이 클러스터에서 `Running` **상태**인지 확인하십시오.

    ```
        kubectl get pods -n kube-system -l app=helm
    ```
    {: pre}

    출력 예:

    ```
    NAME                            READY     STATUS    RESTARTS   AGE
    tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
    ```
    {: screen}

5. {{site.data.keyword.Bluemix_notm}} Helm 저장소를 Helm 인스턴스에 추가하십시오.

    ```
    helm repo add ibm  https://registry.bluemix.net/helm/ibm
    ```
    {: pre}

6. 현재 {{site.data.keyword.Bluemix_notm}} 저장소에서 사용 가능한 Helm 차트를 나열하십시오.

    ```
    helm search ibm
    ```
    {: pre}

7. 차트에 대해 자세히 알아보려면 해당 설정 및 기본값을 나열하십시오.

    예를 들어, strongSwan IPSec VPN 서비스 Helm 차트의 설정, 문서 및 기본값을 보려면 다음과 같이 입력하십시오.

    ```
    helm inspect ibm/strongswan
    ```
    {: pre}


### 관련된 Helm 링크
{: #helm_links}

* strongSwan Helm 차트를 사용하려면 [strongSwan IPSec VPN 서비스 Helm 차트와 VPN 연결 설정](cs_vpn.html#vpn-setup)을 참조하십시오.
* [Helm 차트 카탈로그 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.bluemix.net/containers-kubernetes/solutions/helm-charts) GUI에서 {{site.data.keyword.Bluemix_notm}}와 함께 사용할 수 있는 사용 가능한 Helm 차트를 보십시오.
* <a href="https://docs.helm.sh/helm/" target="_blank">Helm 문서 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>에서 Helm 차트를 설정하고 관리하는 데 사용되는 Helm 명령에 대해 자세히 알아보십시오.
* [Kubernetes Helm 차트를 사용하여 배치 속도를 향상시키는 방법 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.ibm.com/recipes/tutorials/increase-deployment-velocity-with-kubernetes-helm-charts/)에 대해 자세히 알아보십시오.

## Kubernetes 클러스터 리소스 시각화
{: #weavescope}

Weave Scope는 서비스, 팟(Pod), 컨테이너, 프로세스 등을 포함하여 Kubernetes 클러스터 내의 리소스에 대한 시각적 다이어그램을 제공합니다. Weave Scope는 CPU 및 메모리 및 도구에 대한 대화식 메트릭을 제공하여 컨테이너로 tail 및 exec를 실행합니다.
{:shortdesc}

시작하기 전에:

-   공용 인터넷에 클러스터 정보를 노출하지 않도록 유념하십시오. 이러한 단계를 완료하여 안전하게 Weave Scope를 배치하고 웹 브라우저에서 로컬로 이에 액세스하십시오.
-   클러스터가 아직 없으면 [표준 클러스터를 작성](cs_clusters.html#clusters_ui)하십시오. Weave Scope는 CPU를 많이 사용할 수 있습니다(특히, 앱의 경우). 무료 클러스터가 아닌 더 큰 표준 클러스터에서 Weave Scope를 실행하십시오.
-   클러스터에 [CLI를 대상으로 지정](cs_cli_install.html#cs_cli_configure)하고 `kubectl` 명령을 실행하십시오.


클러스터에서 Weave Scope를 사용하려면 다음을 수행하십시오.
2.  클러스터에서 제공된 RBAC 권한 구성 파일 중 하나를 배치하십시오.

    읽기/쓰기 권한을 사용하려면 다음을 실행하십시오.

    ```
    kubectl apply -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac.yaml"
    ```
    {: pre}

    읽기 전용 권한을 사용하려면 다음을 실행하십시오.

    ```
    kubectl apply --namespace weave -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac-readonly.yaml"
    ```
    {: pre}

    출력:

    ```
    clusterrole "weave-scope-mgr" created
    clusterrolebinding "weave-scope-mgr-role-binding" created
    ```
    {: screen}

3.  클러스터 IP 주소로 개별적 액세스가 가능한 Weave Scope 서비스를 배치하십시오.

    <pre class="pre">
    <code>kubectl apply --namespace weave -f "https://cloud.weave.works/k8s/scope.yaml?k8s-version=$(kubectl version | base64 | tr -d '&bsol;n')"</code>
    </pre>

    출력:

    ```
    serviceaccount "weave-scope" created
    deployment "weave-scope-app" created
    service "weave-scope-app" created
    daemonset "weave-scope-agent" created
    ```
    {: screen}

4.  포트 전달 명령을 실행하여 컴퓨터에서 서비스를 여십시오. 다음에 Weave Scope에 액세스할 때 이전 구성 단계를 다시 완료하지 않고도 이 포트 전달 명령을 실행할 수 있습니다.

    ```
    kubectl port-forward -n weave "$(kubectl get -n weave pod --selector=weave-scope-component=app -o jsonpath='{.items..metadata.name}')" 4040
    ```
    {: pre}

    출력:

    ```
    Forwarding from 127.0.0.1:4040 -> 4040
    Forwarding from [::1]:4040 -> 4040
    Handling connection for 4040
    ```
    {: screen}

5.  `http://localhost:4040` 주소로 웹 브라우저를 여십시오. 기본 컴포넌트가 배치되지 않은 경우 다음 다이어그램이 표시됩니다. 클러스터에서 Kubernetes 리소스의 토폴로지 다이어그램이나 테이블을 보도록 선택할 수 있습니다.

     <img src="images/weave_scope.png" alt="Weave Scope의 토폴로지 예" style="width:357px;" />


[Weave Scope 기능에 대해 자세히 알아보십시오 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.weave.works/docs/scope/latest/features/).

<br />

