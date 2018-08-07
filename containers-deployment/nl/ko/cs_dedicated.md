---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-24"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}



# {{site.data.keyword.Bluemix_dedicated_notm}}에서 클러스터 시작하기
{: #dedicated}

{{site.data.keyword.Bluemix_dedicated}} 계정이 있는 경우 데디케이티드 클라우드 환경(`https://<my-dedicated-cloud-instance>.bluemix.net`)에서 Kubernetes 클러스터를 배치하고 이 환경에서 실행 중인 사전 선택된 {{site.data.keyword.Bluemix_notm}} 서비스와 연결할 수 있습니다.
{:shortdesc}

{{site.data.keyword.Bluemix_dedicated_notm}} 계정이 없는 경우 퍼블릭 {{site.data.keyword.Bluemix_notm}} 계정에서 [{{site.data.keyword.containershort_notm}}를 시작](container_index.html)할 수 있습니다.

##  데디케이티드 클라우드 환경 정보
{: #dedicated_environment}

{{site.data.keyword.Bluemix_dedicated_notm}} 계정을 사용하는 경우 사용 가능한 실제 리소스는 사용자의 클러스터에만 데디케이티드되며, 기타 {{site.data.keyword.IBM_notm}} 고객의 클러스터와는 공유되지 않습니다. 클러스터에 대한 격리를 원할 때 {{site.data.keyword.Bluemix_dedicated_notm}} 환경을 설정하도록 선택할 수 있으며, 사용하는 다른 {{site.data.keyword.Bluemix_notm}} 서비스에 대한 격리가 필요합니다. 전용 계정이 없는 경우, [{{site.data.keyword.Bluemix_notm}} 퍼블릭에서 전용 하드웨어로 클러스터를 작성](cs_clusters.html#clusters_ui)할 수 있습니다.

{{site.data.keyword.Bluemix_dedicated_notm}}를 사용하여, 전용 콘솔의 카탈로그에서 또는 {{site.data.keyword.containershort_notm}} CLI를 사용하여 클러스터를 작성할 수 있습니다. 전용 콘솔을 사용하려면 IBM ID를 사용하여 데디케이티드 및 퍼블릭 계정에 동시에 로그인합니다. 이중 로그인을 사용하면 데디케이티드 콘솔을 통해 퍼블릭 클러스터에 액세스할 수 있습니다. CLI를 사용하려면 데디케이티드 엔드포인트(`api.<my-dedicated-cloud-instance>.bluemix.net.`)를 사용하여 로그인합니다. 그런 다음 데디케이티드 환경과 연관된 공용 지역의 {{site.data.keyword.containershort_notm}} API 엔드포인트를 대상으로 지정합니다.

{{site.data.keyword.Bluemix_notm}} 퍼블릭과 Dediicated의 가장 중요한 차이점은 다음과 같습니다.

*   {{site.data.keyword.Bluemix_dedicated_notm}}에서 {{site.data.keyword.IBM_notm}}은 작업자 노드, VLAN 및 서브넷이 배치된 IBM Cloud 인프라(SoftLayer) 계정을 소유하고 관리합니다. {{site.data.keyword.Bluemix_notm}} 퍼블릭에서 IBM Cloud 인프라(SoftLayer) 계정을 소유합니다.
*   {{site.data.keyword.Bluemix_dedicated_notm}}에서 데디케이티드 환경이 사용으로 설정될 때 {{site.data.keyword.IBM_notm}} 관리 IBM Cloud 인프라(SoftLayer) 계정에서 VLAN 및 서브넷의 스펙이 판별됩니다. {{site.data.keyword.Bluemix_notm}} 퍼블릭에서 VLAN 및 서브넷의 스펙은 클러스터가 작성될 때 판별됩니다.

### 클라우드 환경 간에 클러스터 관리의 차이점
{: #dedicated_env_differences}

<table>
<caption>클러스터 관리의 차이점</caption>
<col width="20%">
<col width="40%">
<col width="40%">
 <thead>
 <th>영역</th>
 <th>{{site.data.keyword.Bluemix_notm}} 퍼블릭</th>
 <th>{{site.data.keyword.Bluemix_dedicated_notm}}</th>
 </thead>
 <tbody>
 <tr>
 <td>클러스터 작성</td>
 <td>무료 클러스터 또는 표준 클러스터를 작성합니다.</td>
 <td>표준 클러스터를 작성하십시오.</td>
 </tr>
 <tr>
 <td>클러스터 하드웨어 및 소유권</td>
 <td>표준 클러스터에서 하드웨어는 기타 {{site.data.keyword.IBM_notm}} 고객과 공유되거나 사용자에게만 전용될 수 있습니다. 사용자가 IBM Cloud 인프라(SoftLayer) 계정으로 공용 및 사설 VLAN을 소유하고 관리합니다.</td>
 <td>{{site.data.keyword.Bluemix_dedicated_notm}}의 클러스터에서 하드웨어는 항상 전용입니다. 클러스터 작성에 사용할 수 있는 공인 및 사설 VLAN은 {{site.data.keyword.Bluemix_dedicated_notm}} 환경이 설정될 때 사전 정의되며, IBM은 사용자 대신 이들을 소유하고 관리합니다. 클러스터 작성 중에 사용할 수 있는 위치 또한 {{site.data.keyword.Bluemix_notm}} 환경에 대해 사전 정의됩니다.</td>
 </tr>
 <tr>
 <td>로드 밸런서 및 Ingress 네트워킹</td>
 <td>표준 클러스터를 프로비저닝하는 동안 다음 조치가 자동으로 수행됩니다.<ul><li>하나의 포터블 공용 및 하나의 포터블 사설 서브넷이 클러스터에 바인딩되고 IBM Cloud 인프라(SoftLayer) 계정에 지정됩니다. IBM Cloud 인프라(SoftLayer) 계정을 통해 추가 서브넷을 요청할 수 있습니다.</li></li><li>Ingress 고가용성 애플리케이션 로드 밸런서(ALB)에 대해 하나의 포터블 공인 IP 주소가 사용되고 고유한 공용 라우트가 <code>&lt;cluster_name&gt;. containers.appdomain.cloud</code> 형식으로 지정됩니다. 이 라우트를 사용하여 공용에 여러 앱을 노출할 수 있습니다. 하나의 포터블 사설 IP 주소가 사설 ALB에 대해 사용됩니다.</li><li>네 개의 포터블 공인 IP 주소와 네 개의 사설 IP 주소가 로드 밸런서 서비스에 사용될 수 있는 클러스터에 지정됩니다.</ul></td>
 <td>데디케이티드 계정을 작성할 때 사용자는 클러스터 서비스를 노출하고 액세스하는 방법에 대한 연결 의사결정을 작성합니다. 고유의 엔터프라이즈 IP 범위(사용자 관리 IP)를 사용하려면 [{{site.data.keyword.Bluemix_dedicated_notm}} 환경을 설정](/docs/dedicated/index.html#setupdedicated)할 때 이 범위를 제공해야 합니다. <ul><li>기본적으로 포터블 공인 서브넷은 사용자가 데디케이티드 계정에 작성하는 클러스터에 바인딩되지 않습니다. 대신 사용자의 엔터프라이즈에 가장 잘 맞는 연결 모델을 선택할 수 있는 유연성이 있습니다.</li><li>클러스터를 작성한 후에는 로드 밸런서 또는 Ingress 연결성을 위해 클러스터에 바인딩하고 사용할 서브넷의 유형을 선택합니다.<ul><li>공용 또는 사설 포터블 서브넷의 경우 [클러스터에 서브넷을 추가](cs_subnets.html#subnets)할 수 있습니다.</li><li>전용 온보딩에서 IBM에 제공한 사용자 관리 IP 주소의 경우 [클러스터에 사용자 관리 서브넷을 추가](#dedicated_byoip_subnets)할 수 있습니다.</li></ul></li><li>서브넷을 클러스터에 바인딩하고 나면 Ingress ALB가 작성됩니다. 포터블 공인 서브넷을 사용하는 경우에만 공용 Ingress 라우트가 작성됩니다.</li></ul></td>
 </tr>
 <tr>
 <td>NodePort 네트워킹</td>
 <td>작업자 노드에서 공용 포트를 노출하고, 작업자 노드의 공인 IP 주소를 사용하여 클러스터의 서비스에 공용으로 액세스합니다.</td>
 <td>작업자 노드의 모든 공인 IP 주소는 방화벽을 통해 차단합니다. 그러나 클러스터에 추가된 {{site.data.keyword.Bluemix_notm}} 서비스의 경우 공인 IP 주소 또는 사설 IP 주소를 통해 NodePort에 액세스할 수 있습니다.</td>
 </tr>
 <tr>
 <td>지속적 스토리지</td>
 <td>볼륨의 [동적 프로비저닝](cs_storage.html#create) 또는 [정적 프로비저닝](cs_storage.html#existing)을 사용합니다.</td>
 <td>볼륨의 [동적 프로비저닝](cs_storage.html#create)을 사용합니다. 볼륨의 백업을 요청하고 볼륨에서 복원을 요청하고 기타 스토리지 기능을 수행하려면 [지원 티켓을 여십시오](/docs/get-support/howtogetsupport.html#getting-customer-support).</li></ul></td>
 </tr>
 <tr>
 <td>{{site.data.keyword.registryshort_notm}}의 이미지 레지스트리 URL</td>
 <td><ul><li>미국 남부 및 미국 동부: <code>registry.ng bluemix.net</code></li><li>영국 남부: <code>registry.eu-gb.bluemix.net</code></li><li>중앙 유럽(프랑크푸르트): <code>registry.eu-de.bluemix.net</code></li><li>호주(시드니): <code>registry.au-syd.bluemix.net</code></li></ul></td>
 <td><ul><li>새 네임스페이스의 경우에는 {{site.data.keyword.Bluemix_notm}} 퍼블릭에 대해 정의된 동일한 지역 기반 레지스트리를 사용하십시오.</li><li>{{site.data.keyword.Bluemix_dedicated_notm}}의 단일 및 확장 가능 컨테이너에 설정된 네임스페이스의 경우 <code>registry.&lt;dedicated_domain&gt;</code>을 사용하십시오.</li></ul></td>
 </tr>
 <tr>
 <td>레지스트리에 액세스</td>
 <td>[{{site.data.keyword.containershort_notm}}에서 개인용 및 공용 이미지 레지스트리 사용](cs_images.html)의 옵션을 참조하십시오.</td>
 <td><ul><li>새로운 네임스페이스의 경우 [{{site.data.keyword.containershort_notm}}에서 개인용 및 공용 이미지 레지스트리 사용](cs_images.html)의 옵션을 참조하십시오.</li><li>단일 및 확장 가능 그룹에 대해 설정된 네임스페이스의 경우에는 인증을 위해
[토큰을 사용하고 Kubernetes 시크릿을 작성](cs_dedicated_tokens.html#cs_dedicated_tokens)하십시오.</li></ul></td>
 </tr>
</tbody></table>
{: caption="{{site.data.keyword.Bluemix_notm}} 퍼블릭과 {{site.data.keyword.Bluemix_dedicated_notm}} 간의 기능 차이점" caption-side="top"}

<br />


### 서비스 아키텍처
{: #dedicated_ov_architecture}

각 작업자 노드는 {{site.data.keyword.IBM_notm}} 관리 Docker 엔진, 별도의 컴퓨팅 리소스, 네트워킹 및 볼륨 서비스로 설정됩니다.
{:shortdesc}

기본 제공 보안 기능은 격리, 리소스 관리 기능 및 작업자 노드 보안 준수를 제공합니다. 작업자 노드는 보안 TLS 인증서 및 openVPN 연결을 사용하여 마스터와 통신합니다.


*{{site.data.keyword.Bluemix_dedicated_notm}}의 Kubernetes 아키텍처 및 네트워킹*

![{{site.data.keyword.Bluemix_dedicated_notm}}의 {{site.data.keyword.containershort_notm}} Kubernetes 아키텍처](images/cs_dedicated_arch.png)

<br />


## 데디케이티드에서 {{site.data.keyword.containershort_notm}} 설정
{: #dedicated_setup}

각 {{site.data.keyword.Bluemix_dedicated_notm}} 환경에는 {{site.data.keyword.Bluemix_notm}}에 퍼블릭, 클라이언트 소유 및 회사 계정이 있습니다. 데디케이티드 환경에서 사용자가 클러스터를 작성하려면 관리자가 해당 사용자를 데디케이티드 환경의 퍼블릭 회사 계정에 추가해야 합니다.
{:shortdesc}

시작하기 전에:
  * [{{site.data.keyword.Bluemix_dedicated_notm}} 환경을 설정](/docs/dedicated/index.html#setupdedicated)하십시오.
  * 로컬 시스템 또는 회사 네트워크에서 프록시 또는 방화벽을 사용하여 공용 인터넷 엔드포인트를 제어하는 경우, [방화벽에서 필수 포트 및 IP 주소를 열어야](cs_firewall.html#firewall) 합니다.
  * [Cloud Foundry CLI 다운로드 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/cloudfoundry/cli/releases) 및 [IBM Cloud 관리 CLI 플러그인 추가](/docs/cli/plugins/bluemix_admin/index.html#adding-the-ibm-cloud-admin-cli-plug-in)를 수행하십시오.

{{site.data.keyword.Bluemix_dedicated_notm}} 사용자가 클러스터에 액세스할 수 있도록 하려면 다음을 수행하십시오.

1.  퍼블릭 {{site.data.keyword.Bluemix_notm}} 계정의 소유자는 API 키를 생성해야 합니다.
    1.  {{site.data.keyword.Bluemix_dedicated_notm}} 인스턴스의 엔드포인트에 로그인하십시오. 퍼블릭 계정 소유자의 {{site.data.keyword.Bluemix_notm}} 신임 정보를 입력하고 프롬프트가 표시되면 사용자의 계정을 선택하십시오.

        ```
        bx login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net
        ```
        {: pre}

        **참고:** 연합 ID가 있는 경우에는 `bx login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net --sso`를 사용하여 {{site.data.keyword.Bluemix_notm}} CLI에 로그인하십시오. 사용자 이름을 입력하고 CLI 출력에서 제공된 URL을 사용하여 일회성 패스코드를 검색하십시오. `--sso` 옵션을 사용하지 않으면 로그인에 실패하고 `--sso` 옵션을 사용하면 성공하는 경우에는 연합 ID를 보유하고 있다는 것입니다.

    2.  사용자를 퍼블릭 계정으로 초대하기 위해 API 키를 생성하십시오. API 키 값을 기록해 두십시오. 데디케이티드 계정 관리자가 다음 단계에서 이 값을 사용해야 합니다.

        ```
        bx iam api-key-create <key_name> -d "Key to invite users to <dedicated_account_name>"
        ```
        {: pre}

    3.  사용자를 초대할 퍼블릭 계정 조직의 GUID를 기록해 두십시오. 데디케이티드 계정 관리자가 다음 단계에서 이 GUID를 사용해야 합니다.

        ```
        bx account orgs
        ```
        {: pre}

2.  {{site.data.keyword.Bluemix_dedicated_notm}} 계정의 소유자는 단일 또는 여러 사용자를 퍼블릭 계정으로 초대할 수 있습니다.
    1.  {{site.data.keyword.Bluemix_dedicated_notm}} 인스턴스의 엔드포인트에 로그인하십시오. 데디케이티드 계정 소유자의 {{site.data.keyword.Bluemix_notm}} 신임 정보를 입력하고 프롬프트가 표시되면 사용자의 계정을 선택하십시오.

        ```
        bx login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net
        ```
        {: pre}

        **참고:** 연합 ID가 있는 경우에는 `bx login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net --sso`를 사용하여 {{site.data.keyword.Bluemix_notm}} CLI에 로그인하십시오. 사용자 이름을 입력하고 CLI 출력에서 제공된 URL을 사용하여 일회성 패스코드를 검색하십시오. `--sso` 옵션을 사용하지 않으면 로그인에 실패하고 `--sso` 옵션을 사용하면 성공하는 경우에는 연합 ID를 보유하고 있다는 것입니다.

    2.  사용자를 퍼블릭 계정으로 초대하십시오.
        * 단일 사용자를 초대하려면 다음을 수행하십시오.

            ```
            bx cf bluemix-admin invite-users-to-public -userid=<user_email> -apikey=<public_API_key> -public_org_id=<public_org_ID>
            ```
            {: pre}

            <em>&lt;user_IBMid&gt;</em>를 초대할 사용자의 이메일로 대체하고, <em>&lt;public_API_key&gt;</em>를 이전 단계에서 생성된 API 키로 대체하고, <em>&lt;public_org_ID&gt;</em>를 퍼블릭 계정 조직의 GUID로 대체하십시오. 이 명령에 대한 자세한 정보는 [IBM Cloud 데디케이티드에서 사용자 초대](/docs/cli/plugins/bluemix_admin/index.html#admin_dedicated_invite_public)를 참조하십시오.

        * 현재 데디케이티드 계정 조직에 있는 모든 사용자를 초대하려면 다음을 수행하십시오.

            ```
            bx cf bluemix-admin invite-users-to-public -organization=<dedicated_org_ID> -apikey=<public_API_key> -public_org_id=<public_org_ID>
            ```

            <em>&lt;dedicated_org_ID&gt;</em>를 데디케이티드 계정 조직 ID로 대체하고, <em>&lt;public_API_key&gt;</em>를 이전 단계에서 생성된 API 키로 대체하고, <em>&lt;public_org_ID&gt;</em>를 퍼블릭 계정 조직의 GUID로 대체하십시오. 이 명령에 대한 자세한 정보는 [IBM Cloud 데디케이티드에서 사용자 초대](/docs/cli/plugins/bluemix_admin/index.html#admin_dedicated_invite_public)를 참조하십시오.

    3.  사용자의 IBM ID가 존재하는 경우, 해당 사용자는 퍼블릭 계정의 지정된 조직에 자동으로 추가됩니다. 사용자의 IBM ID가 존재하지 않는 경우 해당 사용자의 이메일 주소로 초대가 전송됩니다. 사용자가 초대를 수락하면 해당 사용자의 IBM ID가 작성되고 사용자가 퍼블릭 계정의 지정된 조직에 추가됩니다.

    4.  사용자가 계정에 추가되었는지 확인하십시오.

        ```
        bx cf bluemix-admin invite-users-status -apikey=<public_API_key>
        ```
        {: pre}

        기존 IBM ID가 있는 초대된 사용자는 `ACTIVE` 상태입니다. 기존 IBM ID가 없는 초대된 사용자는 초대를 수락하기 전에는 `PENDING` 상태이며 초대를 수락한 후 `ACTIVE` 상태가 됩니다.

3.  클러스터 작성 권한이 필요한 사용자가 있는 경우, 해당 사용자에게 관리자 역할을 부여해야 합니다.

    1.  공용 콘솔의 메뉴 표시줄에서 **관리 > 보안 > ID 및 액세스**를 클릭한 다음 **사용자**를 클릭하십시오.

    2.  액세스를 지정할 사용자의 행에서 **조치** 메뉴를 선택한 다음 **액세스 지정**을 클릭하십시오.

    3.  **리소스에 액세스 지정**을 선택하십시오.

    4.  **서비스** 목록에서 **{{site.data.keyword.containerlong}}**를 선택하십시오.

    5.  프롬프트가 표시되면 **지역** 목록에서 **모든 현재 지역** 또는 특정 지역을 선택하십시오.

    6. **역할 선택**에서 관리자를 선택하십시오.

    7. **지정**을 클릭하십시오.

4.  사용자는 이제 데디케이티드 계정 엔드포인트에 로그인하여 클러스터 작성을 시작할 수 있습니다.

    1.  {{site.data.keyword.Bluemix_dedicated_notm}} 인스턴스의 엔드포인트에 로그인하십시오. 프롬프트가 표시되면 IBM ID를 입력하십시오.

        ```
        bx login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net
        ```
        {: pre}

        **참고:** 연합 ID가 있는 경우에는 `bx login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net --sso`를 사용하여 {{site.data.keyword.Bluemix_notm}} CLI에 로그인하십시오. 사용자 이름을 입력하고 CLI 출력에서 제공된 URL을 사용하여 일회성 패스코드를 검색하십시오. `--sso` 옵션을 사용하지 않으면 로그인에 실패하고 `--sso` 옵션을 사용하면 성공하는 경우에는 연합 ID를 보유하고 있다는 것입니다.

    2.  처음 로그인하는 경우에는 프롬프트가 표시될 때 데디케이티드 사용자 ID 및 비밀번호를 제공하십시오. 데디케이티드 계정이 인증되고 데디케이티드 및 퍼블릭 계정이 함께 연결됩니다. 첫 번째 로그인 후에는 로그인할 때마다 IBM ID만 사용하여 로그인합니다. 자세한 정보는 [데디케이티드 ID를 공용 IBM ID에 연결](/docs/cli/connect_dedicated_id.html#connect_dedicated_id)을 확인하십시오.

        **참고**: 클러스터를 작성하려면 데디케이티드 계정과 퍼블릭 계정 둘 다에 로그인해야 합니다. 데디케이티드 계정에만 로그인하려면 데디케이티드 엔드포인트에 로그인할 때 `--no-iam` 플래그를 사용하십시오.

    3.  데디케이티드 환경에서 클러스터를 작성하거나 액세스하려면 해당 환경과 연관된 지역을 설정해야 합니다.

        ```
        bx cs region-set
        ```
        {: pre}

5.  계정의 연결을 끊으려면 데디케이티드 사용자 ID에서 IBM ID의 연결을 끊을 수 있습니다. 자세한 정보는 [공용 IBM ID에서 데디케이티드 ID 연결 끊기](/docs/cli/connect_dedicated_id.html#disconnect-your-dedicated-id-from-the-public-ibmid)를 참조하십시오.

    ```
    bx iam dedicated-id-disconnect
    ```
    {: pre}

<br />


## 클러스터 작성
{: #dedicated_administering}

최대 가용성 및 용량을 위해 {{site.data.keyword.Bluemix_dedicated_notm}} 클러스터 설정을 디자인하십시오.
{:shortdesc}

### GUI에서 클러스터 작성
{: #dedicated_creating_ui}

1. 데디케이티드 콘솔 `https://<my-dedicated-cloud-instance>.bluemix.net`를 여십시오.

2. **{{site.data.keyword.Bluemix_notm}} 퍼블릭에도 로그인** 선택란을 선택하고 **로그인**을 클릭하십시오.

3. 프롬프트에 따라 IBM ID로 로그인하십시오. 처음으로 데디케이티드 계정에 로그인하는 경우에는 프롬프트에 따라 {{site.data.keyword.Bluemix_dedicated_notm}}에 로그인하십시오.

4. 카탈로그에서 **컨테이너**를 선택하고 **Kubernetes 클러스터**를 클릭하십시오.

5. 클러스터 세부사항을 구성하십시오.

    1. **클러스터 이름**을 입력하십시오. 이름은 문자로 시작해야 하며 35자 이하의 문자, 숫자 및 하이픈(-)을 포함할 수 있습니다. 클러스터 이름과 클러스터가 배치된 지역이 Ingress 하위 도메인의 완전한 이름을 형성합니다. 특정 Ingress 하위 도메인이 지역 내에서 고유하도록 하기 위해 클러스터 이름을 자르고 Ingress 도메인 이름 내의 무작위 값을 추가할 수 있습니다.

    2. 클러스터를 배치할 **위치**를 선택하십시오. 사용 가능한 위치는 {{site.data.keyword.Bluemix_dedicated_notm}} 환경이 설정될 때 사전 정의되었습니다.

    3. 클러스터 마스터 노드의 Kubernetes API 서버 버전을 선택하십시오.

    4. 하드웨어 격리의 유형을 선택하십시오. 가상은 시간별로 비용이 청구되고 베어메탈은 월별로 비용이 청구됩니다.

        - **가상 - 데디케이티드**: 작업자 노드가 사용자 계정에 전념하는 인프라에서 호스팅됩니다. 실제 리소스가 완전히 격리됩니다.

        - **베어메탈**: 월별로 비용이 청구되는 베어메탈 서버는 IBM Cloud 인프라(SoftLayer)의 수동 상호작용으로 프로비저닝되며 완료하는 데 영업일 기준으로 이틀 이상 걸릴 수 있습니다. 베어메탈은 추가 리소스 및 호스트 제어가 필요한 고성능 애플리케이션에 가장 적합합니다. 

        베어메탈 머신을 프로비저닝하려는지 확인하십시오. 월별로 비용이 청구되므로 실수로 주문한 후 즉시 취소하는 경우 한 달의 요금이 청구됩니다.
        {:tip}

    5. **머신 유형**을 선택하십시오. 머신 유형은 각 작업자 노드에서 설정되고 컨테이너에 사용 가능한 가상 CPU, 메모리 및 디스크 공간의 양을 정의합니다. 사용 가능한 베어메탈 및 가상 머신 유형은 클러스터를 배치하는 위치에 따라 다릅니다. 자세한 정보는 `bx cs machine-type` [명령](cs_cli_reference.html#cs_machine_types)에 대한 문서를 참조하십시오. 클러스터를 작성한 후 작업자 노드를 클러스터에 추가하여 다른 머신 유형을 추가할 수 있습니다.

    6. 필요한 **작업자 노드의 수**를 선택하십시오. 클러스터의 고가용성을 보장하려면 `3`을 선택하십시오.

    7. **공인 VLAN**(선택사항) 및 **사설 VLAN**(필수)을 선택하십시오. 사용 가능한 공인 및 사설 VLAN은 {{site.data.keyword.Bluemix_dedicated_notm}} 환경이 설정될 때 사전 정의되었습니다. 두 VLAN 모두 작업자 노드 간에 통신하지만 공용 VLAN은 IBM 관리 Kubernetes 마스터와도 통신합니다. 다중 클러스터에 대해 동일한 VLAN을 사용할 수 있습니다.
        **참고**: 작업자 노드가 사설 VLAN 전용으로 설정된 경우에는 네트워크 연결에 대해 대체 솔루션을 구성해야 합니다. 자세한 정보는 [작업자 노드에 대한 VLAN 연결](cs_clusters.html#worker_vlan_connection)을 참조하십시오.

    8. 기본적으로 **로컬 디스크 암호화**가 선택되어 있습니다. 이 선택란을 선택 취소하도록 선택하는 경우 호스트의 Docker 데이터가 암호화되지 않습니다. [암호화에 대해 자세히 알아보십시오.](cs_secure.html#encrypted_disks)

6. **클러스터 작성**을 클릭하십시오. **작업자 노드** 탭에서 작업자 노드 배치의 진행상태를 볼 수 있습니다. 배치가 완료되면 **개요** 탭에서 클러스터가 준비되었음을 확인할 수 있습니다. **참고:** 모든 작업자 노드에는 클러스터가 작성된 이후 수동으로 변경될 수 없는 고유 작업자 노드 ID 및 도메인 이름이 지정됩니다. 
ID 또는 도메인 이름을 변경하면 Kubernetes 마스터가 클러스터를 관리할 수 없습니다.

### CLI에서 클러스터 작성
{: #dedicated_creating_cli}

1.  {{site.data.keyword.Bluemix_notm}} CLI 및 [{{site.data.keyword.containershort_notm}} 플러그인](cs_cli_install.html#cs_cli_install)을 설치하십시오.
2.  {{site.data.keyword.Bluemix_dedicated_notm}} 인스턴스의 엔드포인트에 로그인하십시오. {{site.data.keyword.Bluemix_notm}} 신임 정보를 입력하고, 프롬프트가 표시되면 계정을 선택하십시오.

    ```
    bx login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net
    ```
    {: pre}

    **참고:** 연합 ID가 있는 경우에는 `bx login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net --sso`를 사용하여 {{site.data.keyword.Bluemix_notm}} CLI에 로그인하십시오. 사용자 이름을 입력하고 CLI 출력에서 제공된 URL을 사용하여 일회성 패스코드를 검색하십시오. `--sso` 옵션을 사용하지 않으면 로그인에 실패하고 `--sso` 옵션을 사용하면 성공하는 경우에는 연합 ID를 보유하고 있다는 것입니다.

3.  지역을 대상으로 지정하려면 `bx cs region-set`를 실행하십시오.

4.  `cluster-create` 명령을 사용하여 클러스터를 작성하십시오. 표준 클러스터를 작성하는 경우, 작업자 노드의 하드웨어는 사용 시간을 기준으로 비용이 청구됩니다.

    예:

    ```
    bx cs cluster-create --location <location> --machine-type <machine_type> --name <cluster_name> --workers <number>
    ```
    {: pre}

    <table>
    <caption>이 명령의 컴포넌트 이해</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 이 명령의 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-create</code></td>
    <td>{{site.data.keyword.Bluemix_notm}} 조직에 클러스터를 작성하는 명령입니다.</td>
    </tr>
    <tr>
    <td><code>--location <em>&lt;location&gt;</em></code></td>
    <td>데디케이티드 환경이 사용하도록 구성된 {{site.data.keyword.Bluemix_notm}} 위치 ID를 입력하십시오.</td>
    </tr>
    <tr>
    <td><code>--machine-type <em>&lt;machine_type&gt;</em></code></td>
    <td>머신 유형을 입력하십시오. 전용 하드웨어에 가상 머신으로서, 또는 베어메탈에 실제 머신으로서 작업자 노드를 배치할 수 있습니다. 사용 가능한 실제 및 가상 머신 유형은 클러스터를 배치하는 위치에 따라 다릅니다. 자세한 정보는 `bx cs machine-type` [명령](cs_cli_reference.html#cs_machine_types)에 대한 문서를 참조하십시오.</td>
    </tr>
    <tr>
    <td><code>--public-vlan <em>&lt;machine_type&gt;</em></code></td>
    <td>데디케이티드 환경이 사용하도록 구성된 공인 VLAN의 ID를 입력하십시오. 작업자 노드가 사설 VLAN에만 연결하려는 경우 이 옵션을 지정하지 마십시오. **참고**: 작업자 노드가 사설 VLAN 전용으로 설정된 경우에는 네트워크 연결에 대해 대체 솔루션을 구성해야 합니다. 자세한 정보는 [작업자 노드에 대한 VLAN 연결](cs_clusters.html#worker_vlan_connection)을 참조하십시오.</td>
    </tr>
    <tr>
    <td><code>--private-vlan <em>&lt;machine_type&gt;</em></code></td>
    <td>데디케이티드 환경이 사용하도록 구성된 사설 VLAN의 ID를 입력하십시오.</td>
    </tr>  
    <tr>
    <td><code>--name <em>&lt;name&gt;</em></code></td>
    <td>클러스터 이름을 입력하십시오. 이름은 문자로 시작해야 하며 35자 이하의 문자, 숫자 및 하이픈(-)을 포함할 수 있습니다. 클러스터 이름과 클러스터가 배치된 지역이 Ingress 하위 도메인의 완전한 이름을 형성합니다. 특정 Ingress 하위 도메인이 지역 내에서 고유하도록 하기 위해 클러스터 이름을 자르고 Ingress 도메인 이름 내의 무작위 값을 추가할 수 있습니다.
</td>
    </tr>
    <tr>
    <td><code>--workers <em>&lt;number&gt;</em></code></td>
    <td>클러스터에 포함시킬 작업자 노드의 수를 입력하십시오. <code>--workers</code> 옵션이 지정되지 않은 경우 하나의 작업자 노드가 작성됩니다.</td>
    </tr>
    <tr>
    <td><code>--kube-version <em>&lt;major.minor.patch&gt;</em></code></td>
    <td>클러스터 마스터 노드를 위한 Kubernetes 버전입니다. 이 값은 선택사항입니다. 버전이 지정되지 않은 경우에는 클러스터가 지원되는 Kubernetes 버전의 기본값으로 작성됩니다. 사용 가능한 버전을 보려면 <code>bx cs kube-versions</code>를 실행하십시오.
</td>
    </tr>
    <tr>
    <td><code>--disable-disk-encrypt</code></td>
    <td>작업자 노드는 기본적으로 [디스크 암호화](cs_secure.html#encrypted_disks)를 사용합니다. 암호화를 사용 안함으로 설정하려면 이 옵션을 포함하십시오.</td>
    </tr>
    <tr>
    <td><code>--trusted</code></td>
    <td>[신뢰할 수 있는 컴퓨팅](cs_secure.html#trusted_compute)을 사용으로 설정하여 베어메탈 작업자 노드의 변조 여부를 확인합니다. 클러스터 작성 중에 신뢰를 사용하도록 설정하지 않았으나 나중에 사용하도록 설정하기를 원하는 경우 `bx cs feature-enable` [명령](cs_cli_reference.html#cs_cluster_feature_enable)을 사용할 수 있습니다. 신뢰를 사용하도록 설정한 후에는 나중에 사용하지 않도록 설정할 수 없습니다.</td>
    </tr>
    </tbody></table>

5.  클러스터 작성이 요청되었는지 확인하십시오.

    ```
    bx cs clusters
    ```
    {: pre}

    **참고:**
    * 가상 머신의 경우 작업자 노드 머신을 정렬하며 클러스터를 설정하고 계정에 프로비저닝하는 데 몇 분이 걸릴 수 있습니다. 베어메탈 실제 머신은 IBM Cloud 인프라(SoftLayer)의 수동 상호작용으로 프로비저닝되며 완료하는 데 영업일 기준으로 이틀 이상 걸릴 수 있습니다.
    * 다음과 같은 오류 메시지가 표시되면 [지원 티켓을 여십시오](/docs/get-support/howtogetsupport.html#getting-customer-support).
        ```
        {{site.data.keyword.Bluemix_notm}} 인프라 예외: 주문할 수 없습니다. 'router_name' 라우터의 리소스가 충분하지 않아 'worker_id' 게스트의 요청을 이행할 수 없습니다.
        ```

    클러스터의 프로비저닝이 완료되면 클러스터의 상태가 **배치됨(deployed)**으로 변경됩니다.

    ```
    Name         ID                                   State      Created          Workers   Location   Version
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1         mil01      1.9.7
    ```
    {: screen}

6.  작업자 노드의 상태를 확인하십시오.

    ```
    bx cs workers <cluster_name_or_ID>
    ```
    {: pre}

    작업자 노드가 준비되면 상태(state)가 **정상(normal)**으로 변경되며, 상태(status)는 **준비(Ready)**입니다. 노드 상태(status)가 **준비(Ready)**인 경우에는 클러스터에 액세스할 수 있습니다.

    **참고:** 모든 작업자 노드에는 클러스터가 작성된 이후 수동으로 변경될 수 없는 고유 작업자 노드 ID 및 도메인 이름이 지정됩니다. 
ID 또는 도메인 이름을 변경하면 Kubernetes 마스터가 클러스터를 관리할 수 없습니다.

    ```
    ID                                                 Public IP       Private IP       Machine Type   State    Status   Location   Version
    kube-mil01-paf97e8843e29941b49c598f516de72101-w1   169.xx.xxx.xxx  10.xxx.xx.xxx    free           normal   Ready    mil01      1.9.7
    ```
    {: screen}

7.  작성한 클러스터를 이 세션에 대한 컨텍스트로 설정하십시오. 클러스터 관련 작업을 수행할 때마다 다음 구성 단계를 완료하십시오.

    1.  환경 변수를 설정하기 위한 명령을 가져오고 Kubernetes 구성 파일을 다운로드하십시오.

        ```
        bx cs cluster-config <cluster_name_or_ID>
        ```
        {: pre}

        구성 파일 다운로드가 완료되면 환경 변수로서 경로를 로컬 Kubernetes 구성 파일로 설정하는 데 사용할 수 있는 명령이 표시됩니다.

        OS X에 대한 예:

        ```
         export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
        ```
        {: screen}

    2.  `KUBECONFIG` 환경 변수를 설정하려면 출력의 명령을 복사하여 붙여넣으십시오.
    3.  `KUBECONFIG` 환경 변수가 올바르게 설정되었는지 확인하십시오.

        OS X에 대한 예:

        ```
        echo $KUBECONFIG
        ```
        {: pre}

        출력:

        ```
         /Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml

        ```
        {: screen}

8.  기본 포트 8001로 Kubernetes 대시보드에 액세스하십시오.
    1.  기본 포트 번호로 프록시를 설정하십시오.

        ```
        kubectl proxy
        ```
        {: pre}

        ```
        Starting to serve on 127.0.0.1:8001
        ```
        {: screen}

    2.  웹 브라우저에서 다음 URL을 열어서 Kubernetes 대시보드를 보십시오.

        ```
        http://localhost:8001/ui
        ```
        {: codeblock}

### 개인용 및 공용 이미지 레지스트리 사용
{: #dedicated_images}

컨테이너 이미지에 대해 작업하는 경우 [개인 정보 보호](cs_secure.html#pi)에 대해 자세히 알아보십시오.

새로운 네임스페이스의 경우 [{{site.data.keyword.containershort_notm}}에서 개인용 및 공용 이미지 레지스트리 사용](cs_images.html)의 옵션을 참조하십시오. 단일 및 확장 가능 그룹에 대해 설정된 네임스페이스의 경우에는 인증을 위해
[토큰을 사용하고 Kubernetes 시크릿을 작성](cs_dedicated_tokens.html#cs_dedicated_tokens)하십시오.

### 클러스터에 서브넷 추가
{: #dedicated_cluster_subnet}

클러스터에 서브넷을 추가하여 사용 가능한 포터블 공인 IP 주소의 풀을 변경합니다. 자세한 정보는 [클러스터에 서브넷 추가](cs_subnets.html#subnets)를 참조하십시오. 데디케이티드 클러스터에 서브넷을 추가하려면 다음 차이점을 검토하십시오.

#### Kubernetes 클러스터에 추가적인 사용자 관리 서브넷 및 IP 주소 추가
{: #dedicated_byoip_subnets}

{{site.data.keyword.containershort_notm}}에 액세스하는 데 사용할 온프레미스 네트워크에서 더 많은 고유 서브넷을 제공하십시오. 해당 서브넷의 사설 IP 주소를 Kubernetes 클러스터의 Ingress 및 로드 밸런스 서비스에 추가할 수 있습니다. 사용자 관리 서브넷은 사용할 서브넷의 형식에 따라 두 가지 방법 중 하나로 구성됩니다.

요구사항:
- 사용자가 관리하는 서브넷은 사설 VLAN에만 추가할 수 있습니다.
- 서브넷 접두부 길이 한계는 /24 - /30입니다. 예를 들어, `203.0.113.0/24`는 253개의 사용 가능한 사설 IP 주소를 지정하지만 `203.0.113.0/30`은 1개의 사용 가능한 사설 IP 주소를 지정합니다.
- 서브넷의 첫 번째 IP 주소는 서브넷에 대한 게이트웨이로 사용되어야 합니다.

시작하기 전에: 사용자의 엔터프라이즈 네트워크에서 들어오고 나가는 네트워크 트래픽을 사용자 관리 서브넷을 사용할 {{site.data.keyword.Bluemix_dedicated_notm}} 네트워크로 라우팅하도록 구성하십시오.

1. 고유의 서브넷을 사용하려면 [지원 티켓을 열고](/docs/get-support/howtogetsupport.html#getting-customer-support) 사용할 서브넷 CIDR의 목록을 제공하십시오.
    **참고**: 온프레미스와 내부 계정 연결에 대해 ALB 및 로드 밸런서가 관리되는 방식은 서브넷 CIDR의 형식에 따라 다릅니다. 구성의 차이점에 대해서는 최종 단계를 참조하십시오.

2. {{site.data.keyword.IBM_notm}}에서 사용자 관리 서브넷을 프로비저닝한 후에는 Kubernetes 클러스터에 대해 해당 서브넷을 사용 가능하게 만드십시오.

    ```
   bx cs cluster-user-subnet-add <cluster_name> <subnet_CIDR> <private_VLAN>
    ```
    {: pre}
    <em>&lt;cluster_name&gt;</em>을 클러스터의 이름 또는 ID로 대체하고, <em>&lt;subnet_CIDR&gt;</em>을 지원 티켓에서 제공되는 서브넷 CIDR 중 하나로 대체하고, <em>&lt;private_VLAN&gt;</em>을 사용 가능한 사설 VLAN ID로 대체하십시오. `bx cs vlans`를 실행하여 사용 가능한 사설 VLAN의 ID를 찾을 수 있습니다.

3. 서브넷이 클러스터에 추가되었는지 확인하십시오. 사용자 제공 서브넷의 **사용자 관리** 필드가 _`true`_입니다.

    ```
    bx cs cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
    VLANs
    VLAN ID   Subnet CIDR         Public       User-managed
    1555503   169.xx.xxx.xxx/24   true         false
    1555505   10.xxx.xx.xxx/24    false        false
    1555505   10.xxx.xx.xxx/24    false        true
    ```
    {: screen}

4. 선택사항: [동일한 VLAN의 서브넷 간에 라우팅 사용](cs_subnets.html#vlan-spanning)을 수행하십시오.

5. 온프레미스 및 내부 계정 연결을 구성하려면 다음 옵션 중에서 선택하십시오.
  - 서브넷에 대해 10.x.x.x 사설 IP 주소 범위를 사용한 경우에는 해당 범위에서 유효한 IP를 사용하여 Ingress 및 로드 밸런서로 온프레미스 및 내부 계정 연결을 구성하십시오. 자세한 정보는 [NodePort, LoadBalancer 또는 Ingress 서비스와의 네트워킹 계획](cs_network_planning.html#planning)을 참조하십시오.
  - 서브넷에 대해 10.x.x.x 사설 IP 주소 범위를 사용하지 않은 경우에는 해당 범위에서 유효한 IP를 사용하여 Ingress 및 로드 밸런서로 온프레미스 연결을 구성하십시오. 자세한 정보는 [NodePort, LoadBalancer 또는 Ingress 서비스와의 네트워킹 계획](cs_network_planning.html#planning)을 참조하십시오. 그러나 사용자의 클러스터와 기타 Cloud Foundry 기반 서비스 간에 내부 계정 연결을 구성하려면 IBM Cloud 인프라(SoftLayer) 포터블 사설 서브넷을 사용해야 합니다. [`bx cs cluster-subnet-add`](cs_cli_reference.html#cs_cluster_subnet_add) 명령으로 포터블 사설 서브넷을 작성할 수 있습니다. 이 시나리오의 경우 클러스터에는 온프레미스 연결을 위한 사용자 관리 서브넷과 내부 계정 연결을 위한 IBM Cloud 인프라(SoftLayer) 포터블 사설 서브넷이 둘 다 있습니다.

### 기타 클러스터 구성
{: #dedicated_other}

기타 클러스터 구성을 위해 다음 옵션을 검토하십시오.
  * [클러스터 액세스 관리](cs_users.html#access_policies)
  * [Kubernetes 마스터 업데이트](cs_cluster_update.html#master)
  * [작업자 노드 업데이트](cs_cluster_update.html#worker_node)
  * [클러스터 로깅 구성](cs_health.html#logging)
      * **참고**: 로그 인에이블먼트는 데디케이티드 엔드포인트에서 지원되지 않습니다. 로그 전달을 사용으로 설정하려면 퍼블릭 {{site.data.keyword.cloud_notm}} 엔드포인트에 로그인하여 퍼블릭 조직 및 영역을 대상으로 지정해야 합니다.
  * [클러스터 모니터링 구성](cs_health.html#view_metrics)
      * **참고**: `ibm-monitoring` 클러스터는 각 {{site.data.keyword.Bluemix_dedicated_notm}} 계정 내에 존재합니다. 이 클러스터는 데디케이티드 환경에서 {{site.data.keyword.containerlong_notm}}의 상태를 계속 모니터하고 환경의 안정성 및 연결성을 확인합니다. 환경에서 이 클러스터를 제거하지 마십시오.
  * [Kubernetes 클러스터 리소스 시각화](cs_integrations.html#weavescope)
  * [클러스터 제거](cs_clusters.html#remove)

<br />


## 클러스터에 앱 배치
{: #dedicated_apps}

Kubernetes 기술을 사용하여 {{site.data.keyword.Bluemix_dedicated_notm}} 클러스터에 앱을 배치하고, 앱이 항상 시작되고 실행되도록 할 수 있습니다.
{:shortdesc}

클러스터에 앱을 배치하려면 [{{site.data.keyword.Bluemix_notm}} 퍼블릭 클러스터에 앱 배치](cs_app.html#app)의 지시사항에 따르십시오. {{site.data.keyword.Bluemix_dedicated_notm}} 클러스터의 경우 다음 차이점을 검토하십시오.

Kubernetes 리소스에 대해 작업할 때 [개인 정보 보호](cs_secure.html#pi)에 대해 자세히 알아보십시오.

### 앱에 대한 공용 액세스 허용
{: #dedicated_apps_public}

{{site.data.keyword.Bluemix_dedicated_notm}} 환경의 경우 방화벽에서 공인 기본 IP 주소를 차단합니다. 앱을 공용으로 사용 가능하게 하려면 NodePort 서비스 대신 [LoadBalancer 서비스](#dedicated_apps_public_load_balancer) 또는 [Ingress](#dedicated_apps_public_ingress)를 사용하십시오. 포터블 공인 IP 주소인 LoadBalancer 서비스 또는 Ingress에 대한 액세스가 필요한 경우, 서비스 온보딩 시간에 엔터프라이즈 방화벽 화이트리스트를 IBM에 제공하십시오.

#### 로드 밸런서 서비스 유형을 사용하여 앱에 대한 액세스 구성
{: #dedicated_apps_public_load_balancer}

로드 밸런서에 대해 공인 IP 주소를 사용하려면 엔터프라이즈 방화벽 화이트리스트가 IBM에 제공되었는지 확인하거나 [지원 티켓을 열어서](/docs/get-support/howtogetsupport.html#getting-customer-support) 방화벽 화이트리스트를 구성하십시오. 그런 다음 [LoadBalancer를 사용한 앱 노출](cs_loadbalancer.html)의 단계를 따르십시오.

#### Ingress를 사용하여 앱에 대한 공용 액세스 구성
{: #dedicated_apps_public_ingress}

Ingress ALB에 대해 공인 IP 주소를 사용하려면 엔터프라이즈 방화벽 화이트리스트가 IBM에 제공되었는지 확인하거나 [지원 티켓을 열어서](/docs/get-support/howtogetsupport.html#getting-customer-support) 방화벽 화이트리스트를 구성하십시오. 그런 다음 [공용으로 앱 노출](cs_ingress.html#ingress_expose_public)의 단계를 따르십시오.

### 지속적 스토리지 작성
{: #dedicated_apps_volume_claim}

지속적 스토리지 작성을 위한 옵션을 검토하려면 [지속적 데이터 스토리지](cs_storage.html#planning)를 참조하십시오. 볼륨의 백업, 볼륨에서 복원 및 기타 스토리지 기능을 요청하려면 [지원 티켓을 여십시오](/docs/get-support/howtogetsupport.html#getting-customer-support).
