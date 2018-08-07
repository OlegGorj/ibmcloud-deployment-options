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



# 클러스터 디버깅
{: #cs_troubleshoot}

{{site.data.keyword.containerlong}}를 사용할 때 일반적인 문제점을 해결하고 클러스터를 디버깅하려면 이러한 방법을 고려하십시오. [{{site.data.keyword.Bluemix_notm}} 시스템의 상태 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.ibm.com/bluemix/support/#status)를 확인할 수도 있습니다.
{: shortdesc}

다음과 같은 일반 단계를 수행하여 클러스터가 최신 상태인지 확인할 수 있습니다.
- [작업자 노드를 업데이트](cs_cli_reference.html#cs_worker_update)하는 데 사용할 수 있는 보안 및 운영 체제 패치를 매월 확인하십시오. 
- 최신 기본 [Kubernetes 버전](cs_versions.html)으로 {{site.data.keyword.containershort_notm}}의 [클러스터를 업데이트](cs_cli_reference.html#cs_cluster_update)하십시오.

## 클러스터 디버깅
{: #debug_clusters}

클러스터를 디버그하기 위한 옵션을 검토하고 실패의 근본 원인을 찾습니다.

1.  클러스터를 나열하고 클러스터의 상태(`Status`)를 찾으십시오.

  ```
  bx cs clusters
  ```
  {: pre}

2.  클러스터의 `상태`를 검토하십시오. 클러스터가 **위험**, **삭제에 실패함** 또는 **경고** 상태이거나 오랜 기간 동안 **보류 중** 상태에 있으면 [작업자 노드 디버깅](#debug_worker_nodes)을 시작하십시오.

    <table summary="모든 테이블 행은 왼쪽에서 오른쪽 방향으로 읽어야 하며 1열에는 클러스터의 상태, 2열에는 설명이 있어야 합니다.">
<caption>클러스터 상태</caption>
   <thead>
   <th>클러스터 상태</th>
   <th>설명</th>
   </thead>
   <tbody>
<tr>
   <td>중단됨</td>
   <td>Kubernetes 마스터가 배치되기 전에 사용자가 클러스터 삭제를 요청합니다. 클러스터 삭제가 완료된 후 대시보드에서 클러스터가 제거됩니다. 클러스터가 오랜 기간 동안 이 상태인 경우 [{{site.data.keyword.Bluemix_notm}} 지원 티켓](cs_troubleshoot.html#ts_getting_help)을 여십시오.</td>
   </tr>
 <tr>
     <td>중요</td>
     <td>Kubernetes 마스터에 도달할 수 없거나 클러스터의 모든 작업자 노드가 작동 중지되었습니다. </td>
    </tr>
   <tr>
     <td>삭제에 실패함</td>
     <td>Kubernetes 마스터 또는 최소 하나의 작업자 노드를 삭제할 수 없습니다.  </td>
   </tr>
   <tr>
     <td>삭제됨</td>
     <td>클러스터가 삭제되었으나 아직 대시보드에서 제거되지 않았습니다. 클러스터가 오랜 기간 동안 이 상태인 경우 [{{site.data.keyword.Bluemix_notm}} 지원 티켓](cs_troubleshoot.html#ts_getting_help)을 여십시오. </td>
   </tr>
   <tr>
   <td>삭제 중</td>
   <td>클러스터가 삭제 중이고 클러스터 인프라가 해체되고 있습니다. 클러스터에 액세스할 수 없습니다.  </td>
   </tr>
   <tr>
     <td>배치에 실패함</td>
     <td>Kubernetes 마스터의 배치를 완료하지 못했습니다. 이 상태를 해결할 수 없습니다. [{{site.data.keyword.Bluemix_notm}} 지원 티켓](cs_troubleshoot.html#ts_getting_help)을 열어 IBM Cloud 지원에 문의하십시오.</td>
   </tr>
     <tr>
       <td>배치 중</td>
       <td>Kubernetes 마스터가 아직 완전히 배치되지 않았습니다. 클러스터에 액세스할 수 없습니다. 클러스터가 완전히 배치될 때까지 기다렸다가 클러스터의 상태를 검토하십시오.</td>
      </tr>
      <tr>
       <td>정상</td>
       <td>클러스터의 모든 작업자 노드가 시작되어 실행 중입니다. 클러스터에 액세스하고 클러스터에 앱을 배치할 수 있습니다. 이 상태는 정상으로 간주되고 사용자의 조치가 필요하지 않습니다. **참고**: 작업자 노드가 정상이더라도 [네트워킹](cs_troubleshoot_network.html) 및 [스토리지](cs_troubleshoot_storage.html)와 같은 기타 인프라 리소스에는 여전히 문제가 있을 수 있습니다.</td>
    </tr>
      <tr>
       <td>보류 중</td>
       <td>Kubernetes 마스터가 배치됩니다. 작업자 노드를 프로비저닝 중이므로 아직 클러스터에서 사용할 수 없습니다. 클러스터에 액세스할 수 있지만 클러스터에 앱을 배치할 수 없습니다.  </td>
     </tr>
   <tr>
     <td>요청됨</td>
     <td>클러스터 작성 및 Kubernetes 마스터 및 작업자 노드의 인프라 정렬에 대한 요청이 전송되었습니다. 클러스터의 배치가 시작되면 클러스터 상태가 <code>Deploying</code>으로 변경됩니다. 클러스터가 오랜 기간 동안 <code>Requested</code> 상태인 경우 [{{site.data.keyword.Bluemix_notm}} 지원 티켓](cs_troubleshoot.html#ts_getting_help)을 여십시오. </td>
   </tr>
   <tr>
     <td>업데이트 중</td>
     <td>Kubernetes 마스터에서 실행되는 Kubernetes API 서버가 새 Kubernetes API 버전으로 업데이트 중입니다. 업데이트 중에는 클러스터에 액세스하거나 클러스터를 변경할 수 없습니다. 사용자가 배치한 작업자 노드, 앱 및 리소스는 수정되지 않고 계속 실행됩니다. 업데이트가 완료될 때까지 기다렸다가 클러스터의 상태를 검토하십시오. </td>
   </tr>
    <tr>
       <td>경고</td>
       <td>클러스터에 있는 하나 이상의 작업자 노드를 사용할 수 없지만, 다른 작업자 노드가 사용 가능하며 워크로드를 인계받을 수 있습니다. </td>
    </tr>
   </tbody>
 </table>


<br />


## 작업자 노드 디버깅
{: #debug_worker_nodes}

작업자 노드를 디버그하기 위한 옵션을 검토하고 실패의 근본 원인을 찾습니다.


1.  클러스터가 **위험**, **삭제에 실패함** 또는 **경고** 상태이거나 오랜 기간 동안 **보류 중** 상태에 있으면 작업자 노드의 상태를 검토하십시오.

  ```
  bx cs workers <cluster_name_or_id>
  ```
  {: pre}

2.  CLI 출력의 모든 작업자 노드에 대한 상태(`State` 및 `Status`) 필드를 검토하십시오.

  <table summary="모든 테이블 행은 왼쪽에서 오른쪽 방향으로 읽어야 하며 1열에는 클러스터의 상태, 2열에는 설명이 있어야 합니다.">
  <caption>작업자 노드 상태</caption>
    <thead>
    <th>작업자 노드 상태</th>
    <th>설명</th>
    </thead>
    <tbody>
  <tr>
      <td>중요</td>
      <td>작업자 노드는 여러 이유로 인해 위험 상태가 될 수 있습니다. <ul><li>작업자 노드의 유출 및 드레인 없이 작업자 노드에 대한 다시 부팅을 시작했습니다. 작업자 노드를 다시 부팅하면 <code>docker</code>, <code>kubelet</code>, <code>kube-proxy</code> 및 <code>calico</code>에서 데이터 손상이 발생할 수 있습니다. </li><li>작업자 노드에 배치된 팟(Pod)은 [메모리 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/configure-pod-container/assign-memory-resource/) 및 [CPU ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/configure-pod-container/assign-cpu-resource/)에 대한 리소스 한계를 사용하지 않습니다. 리소스 한계 없이 팟(Pod)은 사용 가능한 모든 리소스를 이용할 수 있으며 이 작업자 노드에 실행할 기타 팟(Pod)에 대한 리소스를 남기지 않습니다. 워크로드의 과도한 사용으로 인해 작업자 노드가 실패하게 됩니다. </li><li>시간이 경과함에 따라 수백 개 또는 수천 개의 컨테이너를 실행한 후 <code>Docker</code>, <code>kubelet</code> 또는 <code>calico</code>가 복구할 수 없는 상태가 되었습니다.</li><li>작업자 노드 및 Kubernetes 마스터 간 통신의 성능을 저하시키거나 차단하는 작업자 노드에 대한 가상 라우터 어플라이언스를 설정합니다. </li><li> 현재 네트워킹 작업자 노드와 Kubernetes 마스터 간의 통신 실패의 원인이 되는 {{site.data.keyword.containershort_notm}} 또는 IBM Cloud 인프라(SoftLayer)의 현재 네트워킹 문제점입니다.</li><li>작업자 노드의 용량이 부족합니다. 작업자 노드의 <strong>상태</strong>를 확인하여 <strong>디스크 부족</strong> 또는 <strong>메모리 부족</strong>이 표시되는지 확인하십십시오. 작업자 노드의 용량이 부족하면 워크로드의 로드 밸런싱을 지원하기 위해 작업자 노드의 워크로드를 줄이거나 작업자 노드를 클러스터에 추가해보십시오.</li></ul> 많은 경우 작업자 노드를 [다시 로드](cs_cli_reference.html#cs_worker_reload)하면 문제점을 해결할 수 있습니다. 작업자 노드를 다시 로드하면 최신 [패치 버전](cs_versions.html#version_types)이 작업자 노드에 적용됩니다. 주 버전 및 부 버전은 변경되지 않습니다. 작업자 노드를 다시 로드하기 전에 기존 팟(Pod)이 정상적으로 종료되고 남아 있는 작업자 노드로 다시 스케줄되었는지 확인하려면 작업자 노드를 유출하고 드레인하십시오. </br></br> 작업자 노드를 다시 로드해도 문제가 해결되지 않으면 다음 단계로 이동하여 작업자 노드의 문제점 해결을 계속 진행하십시오. </br></br><strong>팁:</strong> [작업자 노드에 대한 상태 검사를 구성하고 자동 복구를 사용으로 설정](cs_health.html#autorecovery)할 수 있습니다. 자동 복구는 구성된 검사에 따라 비정상적인 작업자 노드를 발견하면 작업자 노드에서 OS 다시 로드와 같은 정정 조치를 트리거합니다. 자동 복구 작동 방식에 대한 자세한 정보는 [자동 복구 블로그 게시물![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/)을 참조하십시오.
      </td>
     </tr>
      <tr>
        <td>배치 중</td>
        <td>작업자 노드의 Kubernetes 버전을 업데이트할 때 업데이트 설치를 위해 작업자 노드가 다시 배치됩니다. 작업자 노드가 오랫동안 이 상태로 고착되어 있는 경우 다음 단계로 진행하여 배치 중에 문제점이 발생했는지 확인하십시오. </td>
     </tr>
        <tr>
        <td>정상</td>
        <td>작업자 노드가 완전히 프로비저닝되어 클러스터에서 사용할 준비가 되었습니다. 이 상태는 정상으로 간주되고 사용자의 조치가 필요하지 않습니다. **참고**: 작업자 노드가 정상이더라도 [네트워킹](cs_troubleshoot_network.html) 및 [스토리지](cs_troubleshoot_storage.html)와 같은 기타 인프라 리소스에는 여전히 문제가 있을 수 있습니다.</td>
     </tr>
   <tr>
        <td>프로비저닝</td>
        <td>작업자 노드를 프로비저닝 중이므로 아직 클러스터에서 사용할 수 없습니다. CLI 출력의 <strong>상태</strong> 열에서 프로비저닝 프로세스를 모니터할 수 있습니다. 작업자 노드가 오랫동안 이 상태로 고착되어 있는 경우 다음 단계로 진행하여 프로비저닝 중에 문제점이 발생했는지 확인하십시오. </td>
      </tr>
      <tr>
        <td>프로비저닝_실패</td>
        <td>작업자 노드를 프로비저닝할 수 없습니다. 다음 단계로 진행하여 실패에 관한 세부사항을 찾으십시오.</td>
      </tr>
   <tr>
        <td>다시 로드 중</td>
        <td>작업자 노드를 다시 로드 중이므로 클러스터에서 사용할 수 없습니다. CLI 출력의 <strong>상태</strong> 열에서 다시 로드 프로세스를 모니터할 수 있습니다. 작업자 노드가 오랫동안 이 상태로 고착되어 있는 경우 다음 단계로 진행하여 다시 로드하는 중에 문제점이 발생했는지 확인하십시오. </td>
       </tr>
       <tr>
        <td>다시 로드_실패 </td>
        <td>작업자 노드를 다시 로드할 수 없습니다. 다음 단계로 진행하여 실패에 관한 세부사항을 찾으십시오.</td>
      </tr>
      <tr>
        <td>다시 로드_보류 중 </td>
        <td>작업자 노드의 Kubernetes 버전을 다시 로드하거나 업데이트하는 요청이 전송되었습니다. 작업자 노드가 다시 로드되면 상태가 <code>다시 로드 중</code>으로 변경됩니다. </td>
      </tr>
      <tr>
       <td>알 수 없음</td>
       <td>다음 이유 중 하나로 인해 Kubernetes 마스터에 연결할 수 없습니다.<ul><li>Kubernetes 마스터의 업데이트를 요청했습니다. 업데이트 중에 작업자 노드의 상태를 검색할 수 없습니다.</li><li>작업자 노드를 보호 중이거나 최근에 방화벽 설정을 변경한 다른 방화벽이 있을 수 있습니다. {{site.data.keyword.containershort_notm}}에서는 작업자 노드와 Kubernetes 마스터 간의 양방향 통신을 허용하기 위해 특정 IP 주소와 포트를 열도록 요구합니다. 자세한 정보는 [방화벽으로 인해 작업자 노드를 연결할 수 없음](cs_troubleshoot_clusters.html#cs_firewall)을 참조하십시오.</li><li>Kubernetes 마스터가 작동 중단되었습니다. [{{site.data.keyword.Bluemix_notm}} 지원 티켓](#ts_getting_help)을 열어 {{site.data.keyword.Bluemix_notm}} 지원에 문의하십시오.</li></ul></td>
  </tr>
     <tr>
        <td>경고</td>
        <td>작업자 노드가 메모리 또는 디스크 공간의 한계에 도달하고 있습니다. 작업자 노드의 작업 로드를 줄이거나 클러스터에 작업자 노드를 추가하여 작업 로드의 로드 밸런스를 맞출 수 있습니다.</td>
  </tr>
    </tbody>
  </table>

5.  작업자 노드의 세부사항을 나열하십시오. 세부사항에 오류 메시지가 포함되는 경우 [작업자 노드에 대한 일반 오류 메시지](#common_worker_nodes_issues)의 목록을 검토하여 문제점 해결 방법을 알아보십시오.

   ```
   bx cs worker-get <worker_id>
   ```
   {: pre}

  ```
  bx cs worker-get [<cluster_name_or_id>] <worker_node_id>
  ```
  {: pre}

<br />


## 작업자 노드에 대한 일반 문제
{: #common_worker_nodes_issues}

일반 오류 메시지를 검토하고 해결 방법을 알아보십시오.

  <table>
  <caption>공통 오류 메시지</caption>
    <thead>
    <th>오류 메시지</th>
    <th>설명 및 해결
    </thead>
    <tbody>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} 인프라 예외: 현재 사용자의 계정을 통해 '컴퓨팅 인스턴스'를 주문하지 못하게 금지되어 있습니다.</td>
        <td>IBM Cloud 인프라(SoftLayer) 계정이 컴퓨팅 리소스를 주문하지 못하게 제한되었을 수 있습니다. [{{site.data.keyword.Bluemix_notm}} 지원 티켓](#ts_getting_help)을 열어 {{site.data.keyword.Bluemix_notm}} 지원에 문의하십시오.</td>
      </tr>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} 인프라 예외: 주문할 수 없습니다. 'router_name' 라우터의 리소스가 충분하지 않아 'worker_id' 게스트의 요청을 이행할 수 없습니다.</td>
        <td>선택한 VLAN이 작업자 노드를 프로비저닝하기에는 공간이 부족한 데이터센터의 팟(Pod)과 연관되어 있습니다. 선택할 수 있는 옵션은 다음과 같습니다.<ul><li>다른 데이터센터를 사용하여 작업자 노드를 프로비저닝하십시오. <code>bx cs locations</code>를 실행하여 사용 가능한 데이터센터를 나열하십시오.<li>데이터센터의 다른 팟(Pod)과 연관된 기존 공용 및 사설 VLAN 쌍이 있으면 이 VLAN 쌍을 대신 사용하십시오.<li>[{{site.data.keyword.Bluemix_notm}} 지원 티켓](#ts_getting_help)을 열어 {{site.data.keyword.Bluemix_notm}} 지원에 문의하십시오.</ul></td>
      </tr>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} 인프라 예외: ID가 &lt;vlan id&gt;인 네트워크를 얻을 수 없습니다.</td>
        <td>다음 이유 중 하나로 인해 선택된 VLAN ID를 찾을 수 없으므로 작업자 노드를 프로비저닝할 수 없습니다.<ul><li>VLAN ID가 아니라 VLAN 번호를 지정했을 수 있습니다. VLAN 번호는 3 또는 4자리 숫자이지만 VLAN ID는 7자리 숫자입니다. <code>bx cs vlans &lt;location&gt;</code>을 실행하여 VLAN ID를 검색하십시오.<li>사용하는 IBM Cloud 인프라(SoftLayer) 계정과 VLAN ID가 연관되지 않았을 수 있습니다. <code>bx cs vlans &lt;location&gt;</code>을 실행하여 계정의 사용 가능한 VLAN ID를 나열하십시오. IBM Cloud 인프라(SoftLayer) 계정을 변경하려면 [`bx cs credentials-set`](cs_cli_reference.html#cs_credentials_set)를 참조하십시오. </ul></td>
      </tr>
      <tr>
        <td>SoftLayer_Exception_Order_InvalidLocation: 이 주문을 위해 제공된 위치가 올바르지 않습니다 (HTTP 500).</td>
        <td>선택된 데이터센터에서 컴퓨팅 리소스를 주문하도록 IBM Cloud 인프라(SoftLayer)가 설정되지 않았습니다. [{{site.data.keyword.Bluemix_notm}} 지원](#ts_getting_help)에 문의하여 계정이 올바르게 설정되었는지 확인하십시오.</td>
       </tr>
       <tr>
        <td>{{site.data.keyword.Bluemix_notm}} 인프라 예외: 사용자에게 서버를 추가하는 데 필요한 {{site.data.keyword.Bluemix_notm}} 인프라 권한이 없습니다.
        </br></br>
        {{site.data.keyword.Bluemix_notm}} 인프라 예외: 'Item'은 권한을 갖고 주문해야 합니다.</td>
        <td>IBM Cloud 인프라(SoftLayer) 포트폴리오에서 작업자 노드를 프로비저닝하는 데 필요한 권한이 없을 수 있습니다. [표준 Kubernetes 클러스터를 작성하기 위해 IBM Cloud 인프라(SoftLayer) 포트폴리오에 대한 액세스 구성](cs_troubleshoot_clusters.html#cs_credentials)을 참조하십시오.</td>
      </tr>
      <tr>
       <td>작업자가 {{site.data.keyword.containershort_notm}} 서버에 연결할 수 없습니다. 방화벽 설정이 이 작업자의 트래픽을 허용하는지 확인하십시오.
       <td><ul><li>방화벽이 있으면 [방화벽 설정을 구성하여 적절한 포트 및 IP 주소로 발신 트래픽을 허용](cs_firewall.html#firewall_outbound)하십시오.</li><li>`bx cs workers <mycluster>`를 실행하여 클러스터에 공인 IP가 없는지 확인하십시오. 공인 IP가 나열되지 않으면 클러스터에 사설 VLAN만 있는 것입니다.<ul><li>클러스터가 사설 VLAN만 보유하도록 하려면 [VLAN 연결](cs_clusters.html#worker_vlan_connection) 및 [방화벽](cs_firewall.html#firewall_outbound)을 설정하십시오.</li><li>클러스터가 공인 IP를 사용하려는 경우 공용 및 사설 VLAN을 모두 사용하여 [새 작업자 노드를 추가](cs_cli_reference.html#cs_worker_add)하십시오.</li></ul></li></ul></td>
     </tr>
      <tr>
  <td>IMS 계정이 선택된 BSS 계정에 연결되지 않으므로 IMS 포털 토큰을 작성할 수 없습니다.</br></br>제공된 사용자가 없거나 활성 상태임</br></br>SoftLayer_Exception_User_Customer_InvalidUserStatus: 사용자 계정이 현재 cancel_pending입니다.</br></br>머신이 사용자에게 표시되는 것을 기다림</td>
  <td>IBM Cloud 인프라(SoftLayer) 포트폴리오에 액세스하는 데 사용되는 API 키의 소유자에 조치를 수행하는 데 필요한 권한이 없거나 삭제를 보류 중일 수 있습니다.</br></br><strong>사용자</strong>는 다음 단계를 수행하십시오. <ol><li>여러 계정에 대한 액세스 권한이 있는 경우 {{site.data.keyword.containerlong_notm}} 관련 작업을 수행하려는 계정에 로그인되어 있는지 확인하십시오. </li><li>IBM Cloud 인프라(SoftLayer) 포트폴리오에 액세스하는 데 사용되는 현재 API 키 소유자를 보려면 <code>bx cs api-key-info</code>를 실행하십시오. </li><li>현재 사용하는 {{site.data.keyword.Bluemix_notm}} 계정의 소유자를 보려면 <code>bx account list</code>를 실행하십시오. </li><li>{{site.data.keyword.Bluemix_notm}} 계정의 소유자에게 연락하여 API 키 소유자에게 IBM Cloud 인프라(SoftLayer)에 대한 충분한 권한이 없거나 삭제 보류 상태일 수 있음을 보고하십시오.</li></ol></br><strong>계정 소유자</strong>는 다음 단계를 수행하십시오. <ol><li>[IBM Cloud 인프라(SoftLayer)의 필수 권한](cs_users.html#infra_access)을 검토하여 이전에 실패한 조치를 수행하십시오. </li><li>[<code>bx cs api-key-reset</code>](cs_cli_reference.html#cs_api_key_reset) 명령을 사용하여 API 키 소유자의 권한을 수정하거나 새 API 키를 작성하십시오. </li><li>사용자 또는 다른 계정 관리자가 수동으로 계정의 IBM Cloud 인프라(SoftLayer) 신임 정보를 설정하는 경우 [<code>bx cs credentials-unset</code>](cs_cli_reference.html#cs_credentials_unset)를 실행하여 계정에서 신임 정보를 제거하십시오.</li></ol></td>
  </tr>
    </tbody>
  </table>



<br />




## 앱 배치 디버깅
{: #debug_apps}

앱 배치를 디버그해야 하는 옵션을 검토하고 실패의 근본 원인을 찾습니다.

1. `describe` 명령을 실행하여 서비스 또는 배치 리소스의 비정상 항목을 찾으십시오.

 예:
 <pre class="pre"><code>kubectl describe service &lt;service_name&gt; </code></pre>

2. [컨테이너가 ContainerCreating 상태인지 확인](cs_troubleshoot_storage.html#stuck_creating_state)하십시오.

3. 클러스터가 `Critical` 상태인지 확인하십시오. 클러스터가 `Critical` 상태이면 방화벽 규칙을 검사하고 마스터가 작업자 노드와 통신할 수 있는지 확인하십시오.

4. 서비스가 올바른 포트에서 청취하는지 확인하십시오.
   1. 팟(Pod)의 이름을 가져오십시오.
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. 컨테이너에 로그인하십시오.
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   3. 컨테이너 내에서 앱을 curl하십시오. 포트에 액세스할 수 없는 경우 서비스가 올바른 포트에서 청취 중이지 않거나 앱에 문제가 있을 수 있습니다. 서비스의 구성 파일을 올바른 포트로 업데이트하고 앱에 대한 잠재적인 문제를 재배치하거나 조사하십시오.
     <pre class="pre"><code>curl localhost: &lt;port&gt;</code></pre>

5. 서비스가 팟(Pod)에 올바르게 링크되었는지 확인하십시오.
   1. 팟(Pod)의 이름을 가져오십시오.
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. 컨테이너에 로그인하십시오.
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   3. 서비스의 클러스터 IP 주소 및 포트를 curl하십시오. IP 주소 및 포트에 액세스할 수 없는 경우 서비스의 엔드포인트를 살펴보십시오. 엔드포인트가 나열되지 않으면 서비스의 선택기가 팟(Pod)과 일치되지 않습니다. 엔드포인트가 나열된 경우 서비스의 대상 포트 필드를 살펴보고 대상 포트가 팟(Pod)에 사용된 것과 동일한지 확인하십시오.
     <pre class="pre"><code>curl &lt;cluster_IP&gt;:&lt;port&gt;</code></pre>

6. Ingress 서비스의 경우 클러스터 내에서 서비스에 액세스할 수 있는지 확인하십시오.
   1. 팟(Pod)의 이름을 가져오십시오.
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. 컨테이너에 로그인하십시오.
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   2. Ingress 서비스에 대해 지정된 URL을 curl하십시오. URL에 액세스할 수 없는 경우 클러스터와 외부 엔드포인트 간의 방화벽 문제를 확인하십시오. 
     <pre class="pre"><code>curl &lt;host_name&gt;.&lt;domain&gt;</code></pre>

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

