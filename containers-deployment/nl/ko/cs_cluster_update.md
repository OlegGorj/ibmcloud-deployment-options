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





# 클러스터 및 작업자 노드 업데이트
{: #update}

{{site.data.keyword.containerlong}}에서 업데이트를 설치하여 Kubernetes 클러스터를 최신 상태로 유지할 수 있습니다.
{:shortdesc}

## Kubernetes 마스터 업데이트
{: #master}

주기적으로, Kubernetes는 [주 버전 업데이트, 부 버전 업데이트 또는 패치 업데이트](cs_versions.html#version_types)를 릴리스합니다. 업데이트 유형에 따라서 Kubernetes 마스터 컴포넌트의 업데이트를 담당할 수 있습니다.
{:shortdesc}

업데이트는 Kubernetes 마스터의 Kubernetes API 서버 버전 또는 기타 컴포넌트에 영향을 줄 수 있습니다.  작업자 노드를 항상 최신 상태로 유지해야 합니다. 업데이트 시, 작업자 노드보다 먼저 Kubernetes 마스터가 업데이트됩니다.

기본적으로 Kubernetes API 서버를 Kubernetes 마스터의 현재 버전보다 최대 2개의 부 버전까지만 업데이트할 수 있습니다. 예를 들어, 현재 Kubernetes API 서버가 버전 1.7이고 1.10으로 업데이트하려면 먼저 1.8 또는 1.9로 업데이트해야 합니다. 업데이트를 강제로 실행할 수 있지만 3개 이상의 부 버전을 업데이트하면 예상치 못한 결과가 발생할 수 있습니다. 클러스터에서 지원되지 않는 Kubernetes 버전이 실행되는 경우 업데이트를 강제 실행해야 할 수 있습니다.

다음 다이어그램은 마스터 업데이트 시 수행 가능한 프로세스를 보여줍니다.

![마스터 업데이트 우수 사례](/images/update-tree.png)

그림 1. Kubernetes 마스터 업데이트 프로세스 다이어그램

**주의**: 업데이트 프로세스가 수행된 후에는 클러스터를 이전 버전으로 롤백할 수 없습니다. 프로덕션 마스터를 업데이트하기 전에 잠재적인 문제를 처리하기 위해 테스트 클러스터를 사용하고 지시사항을 따르십시오.

_주 버전_ 또는 _부 버전_ 업데이트의 경우 다음 단계를 완료하십시오:

1. [Kubernetes 변경사항](cs_versions.html)을 검토하고 _마스터 이전 업데이트_로 표시된 변경사항을 작성하십시오.
2. GUI를 사용하거나 [CLI 명령](cs_cli_reference.html#cs_cluster_update)을 실행하여 Kubernetes API 서버 및 연관된 Kubernetes 마스터 컴포넌트를 업데이트하십시오. Kubernetes API 서버를 업데이트할 때 API 서버가 약 5 - 10분 동안 작동 중지됩니다. 업데이트 중에는 클러스터에 액세스하거나 클러스터를 변경할 수 없습니다. 그러나 클러스터 사용자가 배치한 작업자 노드, 앱 및 리소스는 수정되지 않고 계속 실행됩니다.
3. 업데이트가 완료되었는지 확인하십시오. {{site.data.keyword.Bluemix_notm}} 대시보드에서 Kubernetes API 서버 버전을 검토하거나 `bx cs clusters`를 실행하십시오.
4. Kubernetes 마스터에서 실행되는 Kubernetes API 서버 버전과 일치하는 [`kubectl cli`](cs_cli_install.html#kubectl)의 버전을 설치하십시오.

Kubernetes API 서버 업데이트가 완료되면 작업자 노드를 업데이트할 수 있습니다.

<br />


## 작업자 노드 업데이트
{: #worker_node}


작업자 노드를 업데이트하라는 알림을 수신했습니다. 무슨 의미일까요? Kubernetes API 서버 및 기타 Kubernetes 마스터 컴포넌트의 보안 업데이트와 패치가 적절히 배치되면 작업자 노드를 동기화해야 합니다.
{: shortdesc}

작업자 노드 Kubernetes 버전은 Kubernetes 마스터에서 실행되는 Kubernetes API 서버 버전보다 상위 버전일 수 없습니다. 시작하기 전에 [Kubernetes 마스터를 업데이트](#master)하십시오.

**주의**:
<ul><li>작업자 노드에 대한 업데이트로 인해 앱과 서비스의 가동이 중단될 수 있습니다.</li>
<li>팟(Pod)의 외부에 저장되지 않은 경우 데이터가 삭제됩니다.</li>
<li>배치에 [복제본 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#replicas)을 사용하여 사용 가능한 노드에서 팟(Pod)을 다시 스케줄하십시오.</li></ul>

가동 중단을 허용할 수 없는 경우 어떻게 할까요?

특정 노드는 업데이트 프로세스의 일부로 잠시 가동이 중단됩니다. 애플리케이션의 가동 중단 시간을 방지하려는 경우, 업그레이드 프로세스 중 특정 유형의 노드에 대한 임계값 백분율을 지정하는 구성 맵에서 고유 키를 정의할 수 있습니다. 표준 Kubernetes 레이블을 기반으로 규칙을 정의하고 사용 불가능으로 설정 가능한 최대 노드 수의 백분율을 제공하여 앱을 시작하고 실행할 수 있습니다. 배치 프로세스를 아직 완료하지 않은 노드는 사용 불가능한 것으로 간주됩니다.

키 정의 방법

구성 맵의 데이터 정보 섹션에서 지정된 시간에 실행할 수 있는 개별 규칙을 최대 10개까지 정의할 수 있습니다. 업그레이드되려면 작업자 노드가 정의된 모든 규칙을 통과해야 합니다.

키가 정의되었습니다. 이제 무엇을 해야 합니까?

규칙을 정의한 후에는 `bx cs worker-update` 명령을 실행하십시오. 성공 응답이 리턴되면 작업자 노드가 업데이트를 위해 큐에 대기됩니다. 그러나 모든 규칙을 만족하기 전에는 노드에서 업데이트 프로세스가 진행되지 않습니다. 노드가 큐에서 대기하는 동안에는 일정 간격마다 업데이트 가능한 노드가 있는지 확인하기 위해 규칙이 검사됩니다.

구성 맵을 정의하지 않도록 선택하면 어떻게 되나요?

구성 맵을 정의하지 않으면 기본값이 사용됩니다. 기본적으로, 업데이트 프로세스 중 각 클러스터의 전체 작업자 노드에서 최대 20%는 사용 불가능합니다.

작업자 노드 업데이트 방법:

1. [Kubernetes 변경사항](cs_versions.html)에서 _마스터 이후 업데이트_로 표시된 변경사항을 작성하십시오.

2. 선택사항: 구성 맵을 정의하십시오.
    예:

    ```
    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: ibm-cluster-update-configuration
      namespace: kube-system
    data:
     drain_timeout_seconds: "120"
     zonecheck.json: |
       {
         "MaxUnavailablePercentage": 70,
         "NodeSelectorKey": "failure-domain.beta.kubernetes.io/zone",
         "NodeSelectorValue": "dal13"
       }
     regioncheck.json: |
       {
         "MaxUnavailablePercentage": 80,
         "NodeSelectorKey": "failure-domain.beta.kubernetes.io/region",
         "NodeSelectorValue": "us-south"
       }
     defaultcheck.json: |
       {
         "MaxUnavailablePercentage": 100
       }
    ```
    {:pre}
  <table summary="테이블의 첫 번째 행에는 두 개의 열이 있습니다. 나머지 행은 왼쪽에서 오른쪽으로 읽어야 하며 1열에는 매개변수가 있고 2열에는 일치하는 설명이 있습니다.">
  <caption>ConfigMap 컴포넌트</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 컴포넌트 이해 </th>
    </thead>
    <tbody>
      <tr>
        <td><code>drain_timeout_seconds</code></td>
        <td> 선택사항: 작업자 노드 업데이트 중 발생하는 드레인에 대한 제한시간(초)입니다. 드레인은 노드에 새 팟(Pod)이 배치되지 않도록 하는 `unschedulable` 상태로 노드를 설정합니다. 또한 드레인은 노드에서 팟(Pod)을 삭제합니다. 허용되는 값은 1 - 180 사이의 정수입니다. 기본값은 30입니다.</td>
      </tr>
      <tr>
        <td><code>zonecheck.json</code></br><code>regioncheck.json</code></td>
        <td> 규칙을 설정하려는 고유 키의 예제입니다. 키 이름은 원하는 대로 작성할 수 있습니다. 해당 정보는 키 내부의 구성 세트로 구문 분석됩니다. 정의하는 각 키에 대해서 <code>NodeSelectorKey</code>와 <code>NodeSelectorValue</code>의 값을 한 개만 설정할 수 있습니다. 둘 이상의 지역이나 위치(데이터센터)에 대한 규칙을 설정하려면 키 항목을 새로 작성하십시오. </td>
      </tr>
      <tr>
        <td><code>defaultcheck.json</code></td>
        <td> <code>ibm-cluster-update-configuration</code> 맵이 올바르게 정의되지 않은 경우에는 기본값으로서 한 번에 클러스터의 20%만 사용 불가능하게 할 수 있습니다. 하나 이상의 유효한 규칙이 글로벌 기본값 없이 정의된 경우 새 기본값은 작업자의 100%가 동시에 사용 불가능하게 되도록 허용하는 것입니다. 이는 기본 백분율을 작성하여 제어할 수 있습니다. </td>
      </tr>
      <tr>
        <td><code>MaxUnavailablePercentage</code></td>
        <td> 지정된 키에 대해 사용 불가능이 될 수 있는 최대 노드 수이며 백분율로 지정됩니다. 배치, 다시 로드 또는 프로비저닝 프로세스 중에는 노드를 사용할 수 없습니다. 정의된 최대 사용 불가능 백분율을 초과하면 큐에 있는 작업자 노드가 업그레이드되지 않습니다. </td>
      </tr>
      <tr>
        <td><code>NodeSelectorKey</code></td>
        <td> 지정된 키의 규칙을 설정하려는 레이블 유형입니다. 작성한 레이블 외에 IBM에서 제공하는 기본 레이블의 규칙도 설정할 수 있습니다. </td>
      </tr>
      <tr>
        <td><code>NodeSelectorValue</code></td>
        <td> 규칙이 평가하도록 설정된 지정된 키 내부의 노드 서브세트입니다. </td>
      </tr>
    </tbody>
  </table>

    **참고**: 최대 10개의 규칙을 정의할 수 있습니다. 한 개의 파일에 10개를 넘는 키를 추가하면 정보의 서브세트만 구문 분석됩니다.

3. GUI를 사용하거나 CLI 명령을 실행하여 작업자 노드를 업데이트하십시오.
  * {{site.data.keyword.Bluemix_notm}} 대시보드에서 업데이트하려면 클러스터의 `작업자 노드` 섹션으로 이동하여 `작업자 업데이트`를 클릭하십시오.
  * 작업자 노드 ID를 가져오려면 `bx cs workers <cluster_name_or_ID>`. 여러 작업자 노드를 선택하는 경우, 작업자 노드는 업데이트 평가를 위해 큐에 대기됩니다. 평가 결과 준비된 것으로 간주되면 구성에 설정된 규칙에 따라 업데이트됩니다.

    ```
    bx cs worker-update <cluster_name_or_ID> <worker_node1_ID> <worker_node2_ID>
    ```
    {: pre}

4. 선택사항: 다음 명령을 실행하고 **이벤트**를 검토해서 구성 맵에서 트리거되는 이벤트와 발생하는 유효성 검증 오류를 확인하십시오.
    ```
    kubectl describe -n kube-system cm ibm-cluster-update-configuration
    ```
    {: pre}

5. 업데이트가 완료되었는지 확인하십시오.
  * {{site.data.keyword.Bluemix_notm}} 대시보드에서 Kubernetes 버전을 검토하거나
`bx cs workers <cluster_name_or_ID>`.
  * `kubectl get nodes`를 실행하여 작업자 노드의 Kubernets 버전을 검토하십시오.
  * 일부 경우에 업데이트 후 이전 클러스터가 **NotReady** 상태의 중복된 작업자 노드를 나열할 수 있습니다. 중복 항목을 제거하려면 [문제점 해결](cs_troubleshoot_clusters.html#cs_duplicate_nodes)을 참조하십시오.

다음 단계:
  - 다른 클러스터에서 업데이트 프로세스를 반복하십시오.
  - 클러스터에서 작업하는 개발자에게 `kubectl` CLI를 Kubernetes 마스터의 버전으로 업데이트하도록 알리십시오.
  - Kubernetes 대시보드에 사용률 그래프가 표시되지 않으면 [`kube-dashboard` 팟(Pod)을 삭제](cs_troubleshoot_health.html#cs_dashboard_graphs)하십시오.
  





<br />



## 머신 유형 업데이트
{: #machine_type}

새 작업자 노드를 추가하고 이전 작업자 노드를 제거하여 작업자 노드의 머신 유형을 업데이트할 수 있습니다. 예를 들어, 이름에 `u1c` 또는 `b1c`가 있는 더 이상 사용되지 않는 머신 유형에 가상 작업자 노드가 있는 경우 이름에 `u2c` 또는 `b2c`가 있는 머신 유형을 사용하는 작업자 노드를 작성하십시오.
{: shortdesc}

시작하기 전에:
- 클러스터를 [CLI의 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오.
- 작업자 노드에 데이터를 저장하면 [작업자 노드의 외부에 저장](cs_storage.html#storage)되지 않은 경우 데이터가 삭제됩니다.


**주의**: 작업자 노드로 업데이트하면 앱과 서비스의 작동이 중단될 수 있습니다. [팟(Pod)의 외부에 저장](cs_storage.html#storage)되지 않은 경우 데이터가 삭제됩니다.



1. 업데이트할 작업자 노드의 이름 및 위치를 기록해 두십시오.
    ```
    bx cs workers <cluster_name>
    ```
    {: pre}

2. 사용 가능한 머신 유형을 보십시오.
    ```
    bx cs machine-types <location>
    ```
    {: pre}

3. [bx cs worker-add](cs_cli_reference.html#cs_worker_add) 명령을 사용하여 작업자 노드를 추가하십시오. 머신 유형을 지정하십시오.

    ```
    bx cs worker-add --cluster <cluster_name> --machine-type <machine_type> --number <number_of_worker_nodes> --private-vlan <private_VLAN_ID> --public-vlan <public_VLAN_ID>
    ```
    {: pre}

4. 작업자 노드가 추가되었는지 확인하십시오.

    ```
    bx cs workers <cluster_name>
    ```
    {: pre}

5. 추가된 작업자 노드가 `Normal` 상태인 경우 오래된 작업자 노드를 제거할 수 있습니다. **참고**: 월별로 비용이 청구되는 머신 유형(예: 베어메탈)을 제거하는 경우 한 달의 요금이 청구됩니다.

    ```
    bx cs worker-rm <cluster_name> <worker_node>
    ```
    {: pre}

6. 기타 작업자 노드를 다른 머신 유형으로 업그레이드하려면 이 단계를 반복하십시오.










