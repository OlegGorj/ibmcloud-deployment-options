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




# {{site.data.keyword.containerlong_notm}}의 고가용성
{: #ha}

내장 Kubernetes 및 {{site.data.keyword.containerlong}} 기능을 사용하여 클러스터의 가용성을 더 높이고 클러스터의 컴포넌트가 실패할 때 앱의 작동 중단을 방지하십시오.
{: shortdesc}

고가용성은 부분 또는 전체 사이트 실패 후에도 앱의 시작 및 실행을 유지시키는 IT 인프라의 핵심 원칙입니다. 고가용성의 주요 목적은 IT 인프라의 잠재적 실패 지점을 제거하는 것입니다. 예를 들어, 중복성을 추가하고 장애 복구 메커니즘을 설정하여 한 시스템의 실패에 대비할 수 있습니다.

IT 인프라의 다른 레벨 및 클러스터의 다른 컴포넌트에서 고가용성을 구현할 수 있습니다. 사용자에게 적합한 고가용성의 레벨은 비즈니스 요구사항, 고객과 약정한 SLA(Service Level Agreements), 지출할 비용과 같은 여러 요인에 따라 달라집니다.

## {{site.data.keyword.containerlong_notm}}의 잠재적 실패 지점에 대한 개요
{: #fault_domains} 

{{site.data.keyword.containerlong_notm}} 아키텍처 및 인프라는 신뢰성, 짧은 처리 대기 시간 및 최대 가동 시간을 보장하도록 설계되었습니다. 그러나 실패가 발생할 수 있습니다. {{site.data.keyword.Bluemix_notm}}에서 호스팅하는 서비스에 따라 실패가 몇 분 동안만 지속되는 경우에도 실패를 허용하지 못할 수 있습니다.
{: shortdesc}

{{site.data.keyword.containershort_notm}}는 중복성 및 반친화성을 추가하여 클러스터에 더 많은 가용성을 추가하는 여러 방법을 제공합니다. 잠재적 실패 지점과 이를 제거하는 방법에 대해 알아보려면 다음 이미지를 검토하십시오.

<img src="images/cs_failure_ov.png" alt="{{site.data.keyword.containershort_notm}} 지역 내 고가용성 클러스터의 결함 도메인에 대한 개요" width="250" style="width:250px; border-style: none"/>


<table summary="이 표는 {{site.data.keyword.containershort_notm}}의 실패 지점을 보여줍니다. 행은 왼쪽에서 오른쪽 방향으로 읽어야 하며 1열에는 실패 지점 번호, 2열에는 실패 지점 제목, 3열에는 설명, 4열에는 문서에 대한 링크가 있습니다.">
<caption>실패 지점</caption>
<col width="3%">
<col width="10%">
<col width="70%">
<col width="17%">
  <thead>
  <th>#</th>
  <th>실패 지점</th>
  <th>설명</th>
  <th>문서에 대한 링크</th>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>컨테이너 또는 팟(Pod) 실패</td>
      <td>컨테이너 및 팟(Pod)은 단기적으로만 지속되도록 디자인되었으며 예기치 않게 실패할 수 있습니다. 예를 들어, 앱에서 오류가 발생하는 경우 컨테이너 또는 팟(Pod)이 손상될 수 있습니다. 앱의 가용성을 높이려면 실패가 발생한 경우 워크로드 및 추가 인스턴스를 처리할 수 있도록 앱의 인스턴스가 충분히 있는지 확인해야 합니다. 이상적으로, 작업자 노드 실패로부터 앱을 보호하도록 이러한 인스턴스가 다중 작업자 노드에 분배됩니다.</td>
      <td>[고가용성 앱 배치](cs_app.html#highly_available_apps)</td>
  </tr>
  <tr>
    <td>2</td>
    <td>작업자 노드 실패</td>
    <td>작업자 노드는 실제 하드웨어의 상위에서 실행되는 VM입니다. 작업자 노드 실패에는 전원, 냉각 또는 네트워킹과 같은 하드웨어 가동 중단 및 VM 자체 문제가 포함됩니다. 클러스터에 다중 작업자 노드를 설정하여 작업자 노드 실패에 대해 설명할 수 있습니다. <br/><br/><strong>참고:</strong> 하나의 위치에 있는 작업자 노드가 별도의 실제 컴퓨팅 호스트에 있다고 보장되지 않습니다. 예를 들어, 세 개의 작업자 노드가 있는 클러스터를 보유할 수 있으나 세 개의 모든 작업자 노드가 IBM 위치의 동일한 실제 컴퓨팅 호스트에 작성되었습니다. 이 실제 컴퓨팅 호스트의 작동이 중지되면 모든 작업자 노드의 작동이 중지됩니다. 이 실패로부터 보호하려면 다른 위치에 두 번째 클러스터를 설정해야 합니다.</td>
    <td>[다중 작업자 노드가 있는 클러스터 작성](cs_cli_reference.html#cs_cluster_create)</td>
  </tr>
  <tr>
    <td>3</td>
    <td>클러스터 실패</td>
    <td>Kubernetes 마스터는 클러스터의 시작 및 실행을 유지하는 기본 컴포넌트입니다. 마스터는 클러스터에 대한 실제 단일 지점의 역할을 하는 etcd 데이터베이스에 모든 클러스터 데이터를 저장합니다. 클러스터 실패는 마스터가 네트워킹 실패로 인해 도달할 수 없거나 etcd 데이터베이스의 데이터가 손상될 때 발생합니다. 하나의 위치에 다중 클러스터를 작성하여 Kubernetes 마스터 또는 etcd 실패로부터 앱을 보호할 수 있습니다. 클러스터 간에 로드 밸런싱하려면 외부 로드 밸런서를 설정해야 합니다. <br/><br/><strong>참고:</strong> 하나의 위치에 다중 클러스터를 설정한다고 해서 작업자 노드가 별도의 실제 컴퓨팅 호스트에 배치되는 것은 아닙니다. 이 실패로부터 보호하려면 다른 위치에 두 번째 클러스터를 설정해야 합니다.</td>
    <td>[고가용성 클러스터 설정](cs_clusters.html#planning_clusters)</td>
  </tr>
  <tr>
    <td>4</td>
    <td>위치 실패</td>
    <td>위치 실패는 모든 실제 컴퓨팅 호스트 및 NFS 스토리지에 영향을 줍니다. 실패에는 전원, 냉각, 네트워킹 또는 가동 중단 및 자연 재해(예: 홍수, 지진 및 허리케인)가 포함됩니다. 위치 실패로부터 보호하려면 클러스터가 외부 로드 밸런서로 로드 밸런싱된 두 개의 다른 위치에 있어야 합니다.</td>
    <td>[고가용성 클러스터 설정](cs_clusters.html#planning_clusters)</td>
  </tr>
  <tr>
    <td>5</td>
    <td>지역 실패</td>
    <td>모든 지역은 지역별 API 엔드포인트에서 액세스할 수 있는 고가용성 로드 밸런서로 설정됩니다. 로드 밸런서는 수신 및 송신 요청을 지역 위치의 클러스터에 라우팅합니다. 전체 지역 실패의 가능성은 낮습니다. 그러나 이 실패에 대해 설명하려는 경우 여러 지역에 다중 클러스터를 설정하고 외부 로드 밸런서를 사용하여 이를 연결할 수 있습니다. 전체 지역이 실패하는 경우 다른 지역의 클러스터가 작업 로드를 인계할 수 있습니다. <br/><br/><strong>참고:</strong> 다중 지역 클러스터에 여러 클라우드 리소스가 필요하며 앱에 따라 복잡해지고 비용이 증가할 수 있습니다. 다중 지역 설정이 필요하거나 잠재적인 서비스 중단을 수용할 수 있는지 확인하십시오. 다중 지역 클러스터를 설정하려는 경우 앱 및 데이터를 다른 지역에 호스팅할 수 있고 앱에서 글로벌 데이터 복제를 처리할 수 있는지 확인하십시오.</td>
    <td>[고가용성 클러스터 설정](cs_clusters.html#planning_clusters)</td>
  </tr>
  <tr>
    <td>6a, 6b</td>
    <td>스토리지 실패</td>
    <td>상태 저장 앱에서 데이터는 앱의 시작 및 실행을 유지하는 중요한 역할을 수행합니다. 데이터가 잠재적인 실패로부터 복구할 수 있을 정도로 가용성이 높은지 확인하려고 합니다. {{site.data.keyword.containershort_notm}}에서 데이터를 지속하기 위한 여러 옵션을 선택할 수 있습니다. 예를 들어, Kubernetes 고유의 지속적 볼륨을 사용하여 NFS 스토리지를 프로비저닝하거나 {{site.data.keyword.Bluemix_notm}} 데이터베이스 서비스를 사용하여 데이터를 저장할 수 있습니다.</td>
    <td>[고가용성 데이터 계획](cs_storage.html#planning)</td>
  </tr>
  </tbody>
  </table>



