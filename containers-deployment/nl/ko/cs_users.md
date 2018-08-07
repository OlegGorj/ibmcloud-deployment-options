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



# 클러스터 액세스 지정
{: #users}

클러스터 관리자는 Kubernetes 클러스터에 대한 액세스 정책을 정의하여 다양한 사용자에 대해 서로 다른 액세스 레벨을 작성할 수 있습니다. 예를 들어, 특정 사용자는 클러스터 리소스에 대해 작업할 수 있도록 하고 다른 사용자는 컨테이너 배치만 수행할 수 있도록 권한 부여할 수 있습니다.
{: shortdesc}


## 액세스 요청 계획
{: #planning_access}

클러스터 관리자가 액세스 요청을 추적하는 것은 어려울 수 있습니다. 액세스 요청을 위한 통신 패턴을 설정하는 것은 클러스터 보안을 유지하는 데 있어서 필수적입니다.
{: shortdesc}

올바른 사용자가 적절한 액세스 권한을 보유하도록 하려면 액세스 요청 또는 공통 태스크 관련 도움 받기에 대한 정책을 명확하게 하십시오.

사용자에게 이미 자신의 팀에서 잘 작동하고 있는 방법이 있을 수도 있으며, 이는 좋은 일입니다. 어디서부터 시작해야 할지 잘 모르겠는 경우에는 다음 방법 중 하나를 시도해 보십시오.

*  티켓 시스템 작성
*  양식 템플리트 작성
*  위키 페이지 작성
*  이메일 요청 필요
*  팀의 일일 작업을 추적하는 데 이미 사용하고 있는 문제 추적 시스템 사용

무엇을 해야 할지 잘 모르시겠습니까? [사용자, 팀 및 애플리케이션 구성에 대한 우수 사례](/docs/tutorials/users-teams-applications.html)와 관련된 튜토리얼을 수행해 보십시오.
{: tip}

## 액세스 정책 및 권한
{: #access_policies}

액세스 정책의 범위는 사용자가 수행할 수 있도록 허용된 조치를 결정하는 사용자 정의 역할을 기반으로 합니다. 사용자는 클러스터, 인프라, 서비스 인스턴스 또는 Cloud Foundry 역할에 대해 고유한 정책을 설정할 수 있습니다.
{: shortdesc}

{{site.data.keyword.containershort_notm}}에 대해 작업하는 모든 사용자에 대해 액세스 정책을 정의해야 합니다. 일부 정책은 사전 정의되어 있으나, 그 외의 정책은 사용자 정의할 수 있습니다. 어느 역할이 일반적인 사용자 태스크와 연결되어 있는지 보고 정책을 사용자 정의할 부분을 식별하려면 다음 이미지 및 정의를 참조하십시오.

![{{site.data.keyword.containershort_notm}} 액세스 역할](/images/user-policies.png)

그림. {{site.data.keyword.containershort_notm}} 액세스 역할

<dl>
  <dt>IAM(Identity and Access Management) 정책</dt>
    <dd><p><strong>플랫폼</strong>: 개인이 {{site.data.keyword.containershort_notm}} 클러스터에 대해 수행할 수 있는 조치를 결정할 수 있습니다. 이러한 정책은 지역별로 설정할 수 있습니다. 조치의 예를 들면 클러스터 작성 또는 제거, 작업자 노드 추가 등이 있습니다. 이러한 정책은 인프라 정책과 함께 설정되어야 합니다.</p>
    <p><strong>인프라</strong>: 클러스터 노드 머신, 네트워킹 또는 스토리지 리소스와 같은 인프라에 대한 액세스 레벨을 결정할 수 있습니다. IBM Cloud 인프라(SoftLayer)에서 조치가 완료되는 경우에도 사용자가 {{site.data.keyword.containershort_notm}} GUI에서나 CLI를 통해 요청을 수행하는지에 관계없이 동일한 정책이 적용됩니다. 이 유형의 정책은 {{site.data.keyword.containershort_notm}} 플랫폼 액세스 정책과 함께 설정해야 합니다. 사용 가능한 역할에 대해 알아보려면 [인프라 권한](/docs/iam/infrastructureaccess.html#infrapermission)을 참조하십시오.</p> </br></br><strong>참고:</strong> 권한이 부여된 사용자가 지정된 권한에 따라 IBM Cloud 인프라(SoftLayer) 계정에서 조치를 수행할 수 있도록 {{site.data.keyword.Bluemix_notm}} 계정이 [IBM Cloud 인프라(SoftLayer) 포트폴리오에 대한 액세스 권한으로 설정](cs_troubleshoot_clusters.html#cs_credentials)됩니다. </dd>
  <dt>Kubernetes 역할 기반 액세스 제어(RBAC) 역할</dt>
    <dd>플랫폼 액세스 정책이 지정된 모든 사용자에게는 자동으로 Kubernetes 역할이 지정됩니다. Kubernetes에서 [역할 기반 액세스 제어(RBAC) ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview)는 사용자가 클러스터 내의 리소스에 대해 수행할 수 있는 조치를 결정합니다. <strong>참고</strong>: RBAC 역할은 <code>default</code> 네임스페이스에 대해 자동으로 구성되지만 클러스터 관리자는 다른 네임스페이스에 대한 역할을 지정할 수 있습니다. </dd>
  <dt>Cloud Foundry</dt>
    <dd>Cloud IAM으로 관리할 수 없는 서비스도 있습니다. 이러한 서비스 중 하나를 사용하고 있는 경우에는 [Cloud Foundry 사용자 역할](/docs/iam/cfaccess.html#cfaccess)을 계속 사용하여 서비스에 대한 액세스를 제어할 수 있습니다.</dd>
</dl>


권한을 다운그레이드하고 계십니까? 이 조치가 완료되려면 몇 분 정도 소요됩니다.
{: tip}

### 플랫폼 역할
{: #platform_roles}

{{site.data.keyword.containershort_notm}}는 {{site.data.keyword.Bluemix_notm}} 플랫폼 역할을 사용하도록 구성되었습니다. 역할 권한은 누적 방식으로 형성되며, 이는 `Editor` 역할이 `Viewer` 역할과 동일한 권한 외에 편집자에게 부여되는 권한을 추가로 보유함을 의미합니다. 다음 표에는 각 역할이 수행할 수 있는 조치의 유형이 설명되어 있습니다.

플랫폼 역할을 지정할 때 해당 RBAC 역할이 기본 네임스페이스에 자동으로 지정됩니다. 사용자의 플랫폼 역할을 변경하면 RBAC 역할도 업데이트됩니다.
{: tip}

<table>
<caption>플랫폼 역할 및 조치</caption>
  <tr>
    <th>플랫폼 역할</th>
    <th>조치 예</th>
    <th>해당 RBAC 역할</th>
  </tr>
  <tr>
      <td>뷰어</td>
      <td>클러스터 또는 기타 서비스 인스턴스의 세부사항을 봅니다.</td>
      <td>보기</td>
  </tr>
  <tr>
    <td>편집자</td>
    <td>IBM Cloud 서비스를 클러스터에 바인드 또는 바인드 해제할 수 있으며, 웹훅을 작성할 수 있습니다. <strong>참고</strong>: 서비스를 바인드하려면 개발자 Cloud Foundry 역할에도 지정되어야 합니다.</td>
    <td>편집</td>
  </tr>
  <tr>
    <td>운영자</td>
    <td>작업자 노드를 작성, 제거, 다시 부팅 또는 다시 로드할 수 있습니다. 클러스터에 서브넷을 추가할 수 있습니다.</td>
    <td>관리자</td>
  </tr>
  <tr>
    <td>관리자</td>
    <td>클러스터를 작성하고 제거할 수 있습니다. 서비스 및 인프라에 대해, 계정 레벨에서 다른 사용자에 대한 액세스 정책을 편집할 수 있습니다. <strong>참고</strong>: 특정 클러스터 또는 계정의 모든 서비스 인스턴스에 관리자 액세스 권한을 지정할 수 있습니다. 클러스터를 삭제하려면 삭제할 클러스터에 대한 관리자 액세스 권한이 있어야 합니다. 클러스터를 작성하려면 모든 서비스 인스턴스에 대한 관리자 역할이 있어야 합니다.</td>
    <td>클러스터 관리자</td>
  </tr>
</table>

UI에서의 사용자 역할 지정에 대한 자세한 정보는 [IAM 액세스 관리](/docs/iam/mngiam.html#iammanidaccser)를 참조하십시오.


### 인프라 역할
{: #infrastructure_roles}

인프라 역할은 사용자가 인프라 레벨에서 리소스에 대해 태스크를 수행할 수 있게 해 줍니다. 다음 표에는 각 역할이 수행할 수 있는 조치의 유형이 설명되어 있습니다. 인프라 역할은 사용자 정의할 수 있지만 사용자에게 자신의 작업을 수행하는 데 필요한 액세스 권한만 부여해야 합니다.

특정 인프라 역할을 부여하는 것 이외에 인프라에 대해 작업하는 사용자에게 디바이스 액세스 권한도 부여해야 합니다.
{: tip}

<table>
<caption>인프라 역할 및 조치</caption>
  <tr>
    <th>인프라 역할</th>
    <th>조치 예</th>
  </tr>
  <tr>
    <td><i>보기 전용</i></td>
    <td>인프라 세부사항을 보고 송장 및 지불 내역을 포함한 계정 요약을 확인할 수 있습니다.</td>
  </tr>
  <tr>
    <td><i>기본 사용자</i></td>
    <td>IP 주소를 포함한 서비스 구성을 편집하고, DNS 레코드를 추가하거나 편집하고, 인프라에 대한 액세스 권한이 있는 새 사용자를 추가할 수 있습니다.</td>
  </tr>
  <tr>
    <td><i>수퍼유저</i></td>
    <td>인프라와 관련된 모든 조치를 수행할 수 있습니다.</td>
  </tr>
</table>

역할 지정을 시작하려면 [사용자의 인프라 권한 사용자 정의](#infra_access)의 단계를 따르십시오.

### RBAC 역할
{: #rbac_roles}

리소스 기반 액세스 제어(RBAC)는 클러스터 내의 리소스를 보안하고 어느 사용자가 어떤 Kubernetes 조치를 수행할 수 있는지 결정하는 방법입니다. 다음 표에서는 RBAC 역할의 유형과 사용자가 해당 역할로 수행할 수 있는 조치의 유형을 볼 수 있습니다. 권한은 누적 방식으로 빌드되며, 이는 `Admin`이 `View` 및 `Edit` 역할과 함께 제공되는 정책을 모두 보유함을 의미합니다. 사용자에게는 필요한 액세스 권한만 부여하십시오.

RBAC 역할은 기본 네임스페이스에 대한 플랫폼 역할과 함께 자동으로 설정됩니다. [역할을 업데이트하거나 다른 네임스페이스에 대한 역할을 지정할 수 있습니다.](#rbac)
{: tip}

<table>
<caption>RBAC 역할 및 조치</caption>
  <tr>
    <th>RBAC 역할</th>
    <th>조치 예</th>
  </tr>
  <tr>
    <td>보기</td>
    <td>기본 네임스페이스 내의 리소스를 볼 수 있습니다. 뷰어는 Kubernetes 시크릿을 볼 수 없습니다. </td>
  </tr>
  <tr>
    <td>편집</td>
    <td>기본 네임스페이스 내의 리소스에 대해 읽기 및 쓰기를 수행할 수 있습니다.</td>
  </tr>
  <tr>
    <td>관리자</td>
    <td>기본 네임스페이스 내의 리소스에 대해 읽기 및 쓰기를 수행할 수 있으나, 네임스페이스 자체에 대해서는 이러한 조치를 수행할 수 없습니다. 네임스페이스 내에 역할을 작성할 수 있습니다.</td>
  </tr>
  <tr>
    <td>클러스터 관리자</td>
    <td>모든 네임스페이스 내의 리소스에 대해 읽기 및 쓰기를 수행할 수 있습니다. 네임스페이스 내에 역할을 작성할 수 있습니다. Kubernetes 대시보드에 액세스할 수 있습니다. 앱을 공용으로 사용할 수 있도록 하는 Ingress 리소스를 작성할 수 있습니다.</td>
  </tr>
</table>

<br />


## {{site.data.keyword.Bluemix_notm}} 계정에 사용자 추가
{: #add_users}

{{site.data.keyword.Bluemix_notm}} 계정에 사용자를 추가하여 클러스터에 액세스 권한을 부여할 수 있습니다.
{:shortdesc}

시작하기 전에 {{site.data.keyword.Bluemix_notm}} 계정에 대한 `Manager` Cloud Foundry 역할이 지정되어 있는지 확인하십시오. 

1.  [사용자를 계정에 추가](../iam/iamuserinv.html#iamuserinv)하십시오.
2.  **액세스** 섹션에서 **서비스**를 펼치십시오.
3.  사용자에게 플랫폼 역할을 지정하여 {{site.data.keyword.containershort_notm}}에 대한 액세스를 설정하십시오.
      1. **서비스** 드롭 다운 목록에서 **{{site.data.keyword.containershort_notm}}**를 선택하십시오.
      2. **지역** 드롭 다운 목록에서 사용자를 초대할 지역을 선택하십시오.
      3. **서비스 인스턴스** 드롭 다운 목록에서 사용자를 초대할 클러스터를 선택하십시오. 특정 클러스터의 ID를 찾으려면 `bx cs clusters`를 실행하십시오.
      4. **역할 선택** 섹션에서 역할을 선택하십시오. 역할에 대해 지원되는 조치 목록을 찾으려면 [액세스 정책 및 권한](#access_policies)을 참조하십시오.
4. [선택사항: Cloud Foundry 역할을 지정](/docs/iam/mngcf.html#mngcf)하십시오.
5. [선택사항: 인프라 역할을 지정](/docs/iam/infrastructureaccess.html#infrapermission)하십시오.
6. **사용자 초대**를 클릭하십시오.

<br />


## IAM API 키 및 `bx cs credentials-set` 명령 이해
{: #api_key}

계정의 클러스터를 프로비저닝하고 관련 작업을 수행하려면 계정이 IBM Cloud 인프라(SoftLayer) 포트폴리오에 액세스하도록 올바르게 설정되었는지 확인해야 합니다. 사용자의 계정 설정에 따라 IAM API 키 또는 `bx cs credentials-set` 명령을 사용하여 수동으로 설정한 인프라 신임 정보를 사용합니다.

<dl>
  <dt>IAM API 키</dt>
    <dd><p>IAM(Identity and Access Management) API 키는 {{site.data.keyword.containershort_notm}} 관리자 액세스 권한이 필요한 첫 번째 조치가 수행될 때 지역에 대해 자동으로 설정됩니다. 예를 들어, 관리 사용자 중 한 명이 <code>us-south</code> 지역에서 첫 번째 클러스터를 작성합니다. 이를 수행하면 이 사용자의 IAM API 키가 이 지역의 계정에 저장됩니다. API 키는 새 작업자 노드 또는 VLAN과 같은 IBM Cloud 인프라(SoftLayer)를 정렬하는 데 사용됩니다.</p> <p>다른 사용자가 새 클러스터 작성 또는 작업자 노드 다시 로드와 같이 IBM Cloud 인프라(SoftLayer) 포트폴리오와의 상호작용이 필요한 조치를 이 지역에서 수행하는 경우 저장된 API 키를 사용하여 해당 조치를 수행하는 데 충분한 권한이 있는지 여부를 판별합니다. 클러스터의 인프라 관련 조치를 수행할 수 있는지 확인하려면 {{site.data.keyword.containershort_notm}} 관리 사용자에게 <strong>수퍼유저</strong> 인프라 액세스 정책을 지정하십시오.</p>
    <p>[<code>bx cs api-key-info</code>](cs_cli_reference.html#cs_api_key_info)를 실행하여 현재 API 키 소유자를 찾을 수 있습니다. 지역에 대해 저장된 API 키를 업데이트해야 하는 경우 [<code>bx cs api-key-reset</code>](cs_cli_reference.html#cs_api_key_reset) 명령을 사용하여 이를 수행할 수 있습니다. 이 명령은 {{site.data.keyword.containershort_notm}} 관리자 액세스 정책이 필요하고 계정에서 이 명령을 실행하는 사용자의 API 키를 저장합니다.</p>
    <p><strong>참고:</strong> IBM Cloud 인프라(SoftLayer) 신임 정보가 <code>bx cs credentials-set</code> 명령을 사용하여 수동으로 설정된 경우 지역에 대해 저장된 API 키가 사용되지 않을 수 있습니다.</p></dd>
  <dt><code>bx cs credentials-set</code>를 통한 IBM Cloud 인프라(SoftLayer) 신임 정보</dt>
    <dd><p>{{site.data.keyword.Bluemix_notm}} 종량과금제 계정이 있는 경우 기본적으로 IBM Cloud 인프라(SoftLayer) 포트폴리오에 대한 액세스 권한이 제공됩니다. 그러나 인프라를 정렬하기 위해 이미 보유하고 있는 다른 IBM Cloud 인프라(SoftLayer) 계정을 사용하려고 할 수 있습니다. [<code>bx cs credentials-set</code>](cs_cli_reference.html#cs_credentials_set) 명령을 사용하여 이 인프라 계정을 {{site.data.keyword.Bluemix_notm}} 계정에 연결할 수 있습니다.</p>
    <p>IBM Cloud 인프라(SoftLayer) 신임 정보가 수동으로 설정되면 계정에 대한 IAM API 키가 존재하는 경우에도 이러한 신임 정보를 사용하여 인프라를 정렬합니다. 신임 정보를 저장하는 사용자에게 인프라를 정렬하기 위한 필수 권한이 없는 경우 클러스터 작성 또는 작업자 노드 다시 로드와 같은 인프라 관련 조치에 실패할 수 있습니다.</p>
    <p>수동으로 설정된 IBM Cloud 인프라(SoftLayer) 신임 정보를 제거하기 위해 [<code>bx cs credentials-unset</code>](cs_cli_reference.html#cs_credentials_unset) 명령을 사용할 수 있습니다. 신임 정보가 제거된 후 인프라를 정렬하는 데 IAM API 키를 사용합니다.</p></dd>
</dl>

<br />


## 사용자의 인프라 권한 사용자 정의
{: #infra_access}

Identity and Access Management에서 인프라 정책을 설정할 때 사용자에게 역할과 연관된 권한이 부여됩니다. 해당 권한을 사용자 정의하려면 IBM Cloud 인프라(SoftLayer)에 로그인하고 거기에서 권한을 조정해야 합니다.
{: #view_access}

예를 들어, **기본 사용자**는 작업자 노드를 다시 부팅할 수 있지만 작업자 노드를 다시 로드할 수는 없습니다. 사용자에게 **수퍼유저** 권한을 부여하지 않고 IBM Cloud 인프라(SoftLayer) 권한을 조정하여 다시 로드 명령을 실행할 권한을 추가할 수 있습니다.

1.  [{{site.data.keyword.Bluemix_notm}} 계정](https://console.bluemix.net/)에 로그인한 후 메뉴에서 **인프라**를 선택하십시오.

2.  **계정** > **사용자** > **사용자 목록**으로 이동하십시오.

3.  권한을 수정하려면 사용자 프로파일의 이름 또는 **디바이스 액세스** 열을 선택하십시오.

4.  **포털 권한** 탭에서 사용자의 액세스 권한을 사용자 정의하십시오. 사용자에게 필요한 권한은 사용하는 데 필요한 인프라 리소스에 따라 달라집니다.

    * **빠른 권한** 드롭 다운 목록을 사용하여 사용자에게 모든 권한을 제공하는 **수퍼유저** 역할을 지정합니다.
    * **빠른 권한** 드롭 다운 목록을 사용하여 사용자에게 일부 필요한 권한(모든 권한은 아님)을 제공하는 **기본 사용자** 역할을 지정합니다.
    * **수퍼유저** 역할이 포함된 모든 권한을 부여하지 않거나 **기본 사용자** 역할 이상의 권한을 추가해야 하는 경우 {{site.data.keyword.containershort_notm}}에서 공통 태스크를 수행하는 데 필요한 권한에 대해 설명하는 다음 표를 검토하십시오.

    <table summary="공통 {{site.data.keyword.containershort_notm}} 시나리오에 대한 인프라 권한">
     <caption>{{site.data.keyword.containershort_notm}}에 공통으로 필요한 인프라 권한</caption>
     <thead>
      <th>{{site.data.keyword.containershort_notm}}의 공통 태스크</th>
      <th>탭에서 필요한 인프라 권한</th>
     </thead>
     <tbody>
       <tr>
         <td><strong>최소 권한</strong>: <ul><li>클러스터를 작성하십시오.</li></ul></td>
         <td><strong>디바이스</strong>:<ul><li>Virtual Server 세부사항 보기</li><li>서버 다시 부팅 및 IPMI 시스템 정보 보기</li><li>OS 다시 로드 실행 및 복구 커널 시작</li></ul><strong>계정</strong>: <ul><li>클라우드 인스턴스 추가/업그레이드</li><li>서버 추가</li></ul></td>
       </tr>
       <tr>
         <td><strong>클러스터 관리</strong>: <ul><li>클러스터 작성, 업데이트 및 삭제</li><li>작업자 노드 추가, 다시 로드 및 다시 부팅</li><li>VLAN 보기</li><li>서브넷 작성</li><li>팟(Pod) 및 로드 밸런서 서비스 배치</li></ul></td>
         <td><strong>지원</strong>:<ul><li>티켓 보기</li><li>티켓 추가</li><li>티켓 편집</li></ul>
         <strong>디바이스</strong>:<ul><li>Virtual Server 세부사항 보기</li><li>서버 다시 부팅 및 IPMI 시스템 정보 보기</li><li>서버 업그레이드</li><li>OS 다시 로드 실행 및 복구 커널 시작</li></ul>
         <strong>서비스</strong>:<ul><li>SSH 키 관리</li></ul>
         <strong>계정</strong>:<ul><li>계정 요약 보기</li><li>클라우드 인스턴스 추가/업그레이드</li><li>서버 취소</li><li>서버 추가</li></ul></td>
       </tr>
       <tr>
         <td><strong>스토리지</strong>: <ul><li>지속적 볼륨 클레임을 작성하여 지속적 볼륨 프로비저닝</li><li>스토리지 인프라 리소스 작성 및 관리</li></ul></td>
         <td><strong>서비스</strong>:<ul><li>스토리지 관리</li></ul><strong>계정</strong>:<ul><li>스토리지 추가</li></ul></td>
       </tr>
       <tr>
         <td><strong>사설 네트워크</strong>: <ul><li>클러스터 내부 네트워킹을 위한 사설 VLAN 관리</li><li>VPN 연결을 사설 네트워크에 설정</li></ul></td>
         <td><strong>네트워크</strong>:<ul><li>네트워크 서브넷 라우트 관리</li><li>IPSEC 네트워크 터널 관리</li><li>네트워크 게이트웨이 관리</li><li>VPN 관리</li></ul></td>
       </tr>
       <tr>
         <td><strong>사설 네트워킹</strong>:<ul><li>공용 로드 밸런서 또는 Ingress 네트워킹을 설정하여 앱 노출</li></ul></td>
         <td><strong>디바이스</strong>:<ul><li>Load Balancers 관리</li><li>호스트 이름/도메인 편집</li><li>포트 제어 관리</li></ul>
         <strong>네트워크</strong>:<ul><li>공용 네트워크 포트로 컴퓨팅 추가</li><li>네트워크 서브넷 라우트 관리</li><li>IP 주소 추가</li></ul>
         <strong>서비스</strong>:<ul><li>DNS, 리버스 DNS 및 WHOIS 관리</li><li>인증서(SSL) 보기</li><li>인증서(SSL) 관리</li></ul></td>
       </tr>
     </tbody>
    </table>

5.  변경사항을 저장하려면 **포털 권한 편집**을 클릭하십시오.

6.  **디바이스 액세스** 탭에서 액세스 권한을 부여할 디바이스를 선택하십시오.

    * **디바이스 유형** 드롭 다운 목록에서 **모든 Virtual Server**에 대한 액세스 권한을 부여할 수 있습니다.
    * 사용자가 작성된 새 디바이스에 액세스할 수 있도록 허용하려면 **새 디바이스가 추가될 때 자동으로 액세스 권한 부여**를 선택하십시오.
    * 변경사항을 저장하려면 **디바이스 액세스 업데이트**를 클릭하십시오.

<br />


## 사용자 정의 Kubernetes RBAC 역할이 포함된 사용자 권한 부여
{: #rbac}

{{site.data.keyword.containershort_notm}} 액세스 정책은 [액세스 정책 및 권한](#access_policies)에 설명된 대로 특정 Kubernetes 역할 기반 액세스 제어(RBAC)와 일치합니다. 해당 액세스 정책과 다른 기타 Kubernetes 역할에 권한을 부여하기 위해 RBAC 역할을 사용자 정의한 후 개인 또는 사용자의 그룹에 역할을 지정할 수 있습니다.
{: shortdesc}

예를 들어, 개발자의 팀에 특정 API 그룹 또는 클러스터의 특정 Kubernetes 네임스페이스 내의(전체 클러스터 전체는 아님) 리소스에 대해 작업할 수 있는 권한을 부여하고자 할 수 있습니다. 역할을 작성한 후 {{site.data.keyword.containershort_notm}}에 고유한 사용자 이름을 사용하여 해당 역할을 사용자에 바인딩합니다. 자세한 정보는 Kubernetes 문서에 있는 [RBAC 권한 사용 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview)을 참조하십시오.

시작하기 전에 클러스터를 [Kubernetes CLI를 클러스터에 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오.

1.  지정할 액세스 권한으로 역할을 작성하십시오.

    1. `.yaml` 파일을 작성하여 지정할 액세스 권한으로 역할을 정의하십시오.

        ```
        kind: Role
        apiVersion: rbac.authorization.k8s.io/v1
        metadata:
          namespace: default
          name: my_role
        rules:
        - apiGroups: [""]
          resources: ["pods"]
          verbs: ["get", "watch", "list"]
        - apiGroups: ["apps", "extensions"]
          resources: ["daemonsets", "deployments"]
          verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
        ```
        {: codeblock}

        <table>
        <caption>이 YAML 컴포넌트 이해</caption>
          <thead>
            <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 이 YAML 컴포넌트 이해</th>
          </thead>
          <tbody>
            <tr>
              <td><code>kind</code></td>
              <td>`Role`을 사용하여 단일 네임스페이스 내 리소스에 액세스 권한을 부여하거나 클러스터 전반의 리소스에 `ClusterRole`을 사용하십시오.</td>
            </tr>
            <tr>
              <td><code>apiVersion</code></td>
              <td><ul><li>Kubernetes 1.8 이상을 실행하는 클러스터의 경우 `rbac.authorization.k8s.io/v1`을 사용하십시오. </li><li>이전 버전의 경우 `apiVersion: rbac.authorization.k8s.io/v1beta1`을 사용하십시오.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/namespace</code></td>
              <td><ul><li>`Role` 유형의 경우: 액세스 권한이 부여되는 Kubernetes 네임스페이스를 지정하십시오.</li><li>클러스터 레벨에 적용하는 `ClusterRole`을 작성하는 경우 `namespace` 필드를 사용하지 마십시오.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/name</code></td>
              <td>나중에 역할을 바인딩할 때 역할의 이름을 지정하고 이름을 사용하십시오.</td>
            </tr>
            <tr>
              <td><code>rules/apiGroups</code></td>
              <td><ul><li>`"apps"`, `"batch"` 또는 `"extensions"`와 같이 사용자가 상호작용할 수 있도록 Kubernetes API 그룹을 지정하십시오. </li><li>REST 경로 `api/v1`의 코어 API 그룹에 액세스하려면 그룹을 `[""]`와 같이 공백으로 두십시오.</li><li>자세한 정보는 Kubernetes 문서의 [API 그룹![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://v1-9.docs.kubernetes.io/docs/reference/api-overview/#api-groups)을 참조하십시오.</li></ul></td>
            </tr>
            <tr>
              <td><code>rules/resources</code></td>
              <td><ul><li>`"daemonsets"`, `"deployments"`, `"events"` 또는 `"ingresses"`와 같이 액세스 권한을 부여할 Kubernetes 리소스를 지정하십시오.</li><li>`"nodes"`를 지정하는 경우 역할 유형은 `ClusterRole`이어야 합니다.</li><li>리소스 목록은 Kubernetes 치트 시트에서 [리소스 유형 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)의 표를 참조하십시오.</li></ul></td>
            </tr>
            <tr>
              <td><code>rules/verbs</code></td>
              <td><ul><li>`"get"`, `"list"`, `"describe"`, `"create"` 또는 `"delete"`와 같이 사용자가 수행할 수 있는 조치의 유형을 지정하십시오. </li><li>verb에 대한 전체 목록은 [`kubectl` 문서![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/reference/kubectl/overview/)를 참조하십시오.</li></ul></td>
            </tr>
          </tbody>
        </table>

    2.  클러스터에 역할을 작성하십시오.

        ```
        kubectl apply -f <path_to_yaml_file>
        ```
        {: pre}

    3.  역할이 작성되었는지 확인하십시오.

        ```
        kubectl get roles
        ```
        {: pre}

2.  역할을 사용자에 바인딩하십시오.

    1. `.yaml` 파일을 작성하여 역할에 사용자를 바인딩하십시오. 각 주제의 이름에 사용할 고유한 URL을 기록해 두십시오.

        ```
        kind: RoleBinding
        apiVersion: rbac.authorization.k8s.io/v1
        metadata:
          name: my_role_team1
          namespace: default
        subjects:
        - kind: User
          name: https://iam.ng.bluemix.net/kubernetes#user1@example.com
          apiGroup: rbac.authorization.k8s.io
        - kind: User
          name: https://iam.ng.bluemix.net/kubernetes#user2@example.com
          apiGroup: rbac.authorization.k8s.io
        roleRef:
          kind: Role
          name: custom-rbac-test
          apiGroup: rbac.authorization.k8s.io
        ```
        {: codeblock}

        <table>
        <caption>이 YAML 컴포넌트 이해</caption>
          <thead>
            <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 이 YAML 컴포넌트 이해</th>
          </thead>
          <tbody>
            <tr>
              <td><code>kind</code></td>
              <td>두 가지 유형의 역할 `.yaml` 파일(네임스페이스 `Role` 및 클러스터 전반의 `ClusterRole`)에 대해 `kind`를 `RoleBinding`으로 지정하십시오.</td>
            </tr>
            <tr>
              <td><code>apiVersion</code></td>
              <td><ul><li>Kubernetes 1.8 이상을 실행하는 클러스터의 경우 `rbac.authorization.k8s.io/v1`을 사용하십시오. </li><li>이전 버전의 경우 `apiVersion: rbac.authorization.k8s.io/v1beta1`을 사용하십시오.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/namespace</code></td>
              <td><ul><li>`Role` 유형의 경우: 액세스 권한이 부여되는 Kubernetes 네임스페이스를 지정하십시오.</li><li>클러스터 레벨에 적용하는 `ClusterRole`을 작성하는 경우 `namespace` 필드를 사용하지 마십시오.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/name</code></td>
              <td>역할 바인딩의 이름을 지정하십시오.</td>
            </tr>
            <tr>
              <td><code>subjects/kind</code></td>
              <td>`User`로 유형을 지정하십시오.</td>
            </tr>
            <tr>
              <td><code>subjects/name</code></td>
              <td><ul><li>사용자의 이메일 주소를 `https://iam.ng.bluemix.net/kubernetes#` URL에 추가하십시오.</li><li>예를 들면, `https://iam.ng.bluemix.net/kubernetes#user1@example.com`입니다.</li></ul></td>
            </tr>
            <tr>
              <td><code>subjects/apiGroup</code></td>
              <td>`rbac.authorization.k8s.io`를 사용하십시오.</td>
            </tr>
            <tr>
              <td><code>roleRef/kind</code></td>
              <td>역할 `.yaml` 파일(`Role` 또는 `ClusterRole`)에서 `kind`와 동일한 값을 입력하십시오.</td>
            </tr>
            <tr>
              <td><code>roleRef/name</code></td>
              <td>역할 `.yaml` 파일의 이름을 입력하십시오.</td>
            </tr>
            <tr>
              <td><code>roleRef/apiGroup</code></td>
              <td>`rbac.authorization.k8s.io`를 사용하십시오.</td>
            </tr>
          </tbody>
        </table>

    2. 클러스터에 역할 바인딩 리소스를 작성하십시오.

        ```
        kubectl apply -f filepath/my_role_team1.yaml
        ```
        {: pre}

    3.  바인딩이 작성되었는지 확인하십시오.

        ```
        kubectl get rolebinding
        ```
        {: pre}

이제 사용자 정의 Kubernetes RBAC 역할을 작성하고 바인딩했습니다. 사용자에게 후속 조치를 취하십시오. 팟(Pod) 삭제와 같은 역할로 인해 완료할 권한이 있는 조치를 테스트하도록 사용자에게 요청하십시오.
