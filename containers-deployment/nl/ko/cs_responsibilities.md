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



#  {{site.data.keyword.containerlong_notm}} 사용 시 사용자의 책임
{{site.data.keyword.containerlong}} 사용 시 지켜야 하는 클러스터 관리 책임 및 이용 약관에 대해 알아보십시오.
{:shortdesc}

## 클러스터 관리 책임
{: #responsibilities}

클러스터를 관리하기 위해 IBM과 공유하는 책임을 검토하십시오.
{:shortdesc}

**IBM은 다음을 담당합니다.**

- 클러스터 작성 시 마스터, 작업자 노드 및 클러스터 내의 관리 컴포넌트(예: Ingress 애플리케이션 로드 밸런서) 배치
- 클러스터에 대한 Kubernetes 마스터의 보안 업데이트, 모니터링, 격리 및 복구 관리
- 작업자 노드의 상태를 모니터하고 해당 작업자 노드의 업데이트 및 복구 자동화 제공
- 작업자 노드 추가, 작업자 노드 제거 및 기본 서브넷 작성을 포함하여 인프라 계정에 대한 자동화 태스크 수행
- 클러스터 내의 운영 컴포넌트(예: Ingress 애플리케이션 로드 밸런서 및 스토리지 플러그인) 관리, 업데이트 및 복구
- 지속적 볼륨 클레임에서 요청될 때 스토리지 볼륨 프로비저닝
- 모든 작업자 노드에 대한 보안 설정 제공

</br>

**사용자는 다음을 담당합니다.**

- [IBM Cloud 인프라(SoftLayer) 포트폴리오에 액세스하도록 {{site.data.keyword.Bluemix_notm}} 계정 구성](cs_troubleshoot_clusters.html#cs_credentials)
- [클러스터 내의 Kubernetes 리소스(예: 팟(Pod), 서비스 및 배치) 배치 및 관리](cs_app.html#app_cli)
- [서비스 및 Kubernetes의 기능을 활용하여 앱의 고가용성 보장](cs_app.html#highly_available_apps)
- [CLI를 통해 작업자 노드를 추가하거나 제거하여 클러스터 기능 추가 또는 제거](cs_cli_reference.html#cs_worker_add)
- [ 클러스터의 네트워크 격리를 위해 IBM Cloud 인프라(SoftLayer)에서 공용 및 사설 VLAN 작성](/docs/infrastructure/vlans/getting-started.html#getting-started-with-vlans)
- [모든 작업자 노드에서 Kubernetes 마스터 URL에 네트워크로 연결할 수 있는지 확인](cs_firewall.html#firewall) <p>**참고**: 작업자 노드에 공용 및 사설 VLAN이 모두 있는 경우 네트워크 연결이 구성됩니다. 작업자 노드가 사설 VLAN 전용으로 설정된 경우에는 네트워크 연결에 대해 대체 솔루션을 구성해야 합니다. 자세한 정보는 [작업자 노드에 대한 VLAN 연결](cs_clusters.html#worker_vlan_connection)을 참조하십시오. </p>
- [Kubernetes 버전 업데이트가 사용 가능한 경우 마스터 kube-apiserver 업데이트](cs_cluster_update.html#master)
- [작업자 노드를 주, 부 및 패치 버전에 대해 최신 상태로 유지](cs_cluster_update.html#worker_node)
- [`kubectl` 명령(예: `cordon` 또는 `drain`)을 실행하고 `bx cs` 명령(예: `reboot`, `reload` 또는 `delete`)을 실행하여 문제가 발생한 작업자 노드 복구](cs_cli_reference.html#cs_worker_reboot)
- [필요에 따라 IBM Cloud 인프라(SoftLayer)에서 서브넷 추가 또는 제거](cs_subnets.html#subnets)
- [IBM Cloud 인프라(SoftLayer)의 지속적 스토리지에 데이터 백업 및 복원 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](../services/RegistryImages/ibm-backup-restore/index.html)
- [자동 복구를 통해 작업자 노드의 상태 모니터링 구성](cs_health.html#autorecovery)

<br />


## {{site.data.keyword.containerlong_notm}}의 오용
{: #terms}

클라이언트는 {{site.data.keyword.containershort_notm}}를 오용할 수 없습니다.
{:shortdesc}

오용에는 다음이 포함됩니다.

*   불법적 활동
*   맬웨어의 배포 또는 실행
*   {{site.data.keyword.containershort_notm}}에 손상을 주거나 사용자의 {{site.data.keyword.containershort_notm}} 사용을 방해함
*   기타 서비스 또는 시스템에 손상을 주거나 사용자의 사용을 방해함
*   서비스 또는 시스템에 대한 비인가된 액세스
*   서비스 또는 시스템의 비인가된 수정
*   기타 사용자의 권한 위반


전체 이용 약관은 [클라우드 서비스 이용 약관](https://console.bluemix.net/docs/overview/terms-of-use/notices.html#terms)을 참조하십시오.
