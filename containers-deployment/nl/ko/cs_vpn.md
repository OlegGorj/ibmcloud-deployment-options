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


# VPN 연결 설정
{: #vpn}

VPN 연결을 통해 {{site.data.keyword.containerlong}}에서 Kubernetes 클러스터의 앱을 온프레미스 네트워크에 안전하게 연결할 수 있습니다. 클러스터 내에서 실행 중인 앱에 클러스터 외부의 앱을 연결할 수도 있습니다.
{:shortdesc}

작업자 노드 및 앱을 온프레미스 데이터센터에 연결하려면 다음 옵션 중 하나를 구성할 수 있습니다.

- **strongSwan IPSec VPN 서비스**: Kubernetes 클러스터를 온프레미스 네트워크와 안전하게 연결하는 [strongSwan IPSec VPN 서비스 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.strongswan.org/)를 설정할 수 있습니다. strongSwan IPSec VPN 서비스는 IPsec(Industry-standard Protocol Security) 프로토콜 스위트를 기반으로 하는 인터넷을 통해 안전한 엔드-투-엔드 통신 채널을 제공합니다. 클러스터와 온프레미스 네트워크 간의 보안 연결을 설정하려면 클러스터 내의 팟(Pod)에 직접 [strongSwan IPSec VPN 서비스를 구성하고 배치](#vpn-setup)하십시오.

- **가상 라우터 어플라이언스(VRA) 또는 Fortigate 보안 어플라이언스(FSA)**: [VRA](/docs/infrastructure/virtual-router-appliance/about.html) 또는 [FSA](/docs/infrastructure/fortigate-10g/about.html)를 설정하여 IPSec VPN 엔드포인트를 구성하도록 선택할 수 있습니다. 이 옵션은 대규모 클러스터가 있으며 VPN을 통해 비Kubernetes 리소스에 액세스하거나 단일 VPN을 통해 다중 클러스터에 액세스하려는 경우에 유용합니다. VRA를 구성하려면 [VRA를 사용하여 VPN 연결 설정](#vyatta)을 참조하십시오.

## strongSwan IPSec VPN 서비스 Helm 차트로 VPN 연결 설정
{: #vpn-setup}

Helm 차트를 사용하여 Kubernetes 팟(Pod) 내부에 strongSwan IPSec VPN 서비스를 구성 및 배치하십시오.
{:shortdesc}

strongSwan은 클러스터와 통합되어 있으므로 외부 게이트웨이 디바이스가 필요하지 않습니다. VPN 연결이 설정되면 클러스터 내의 모든 작업자 노드에서 라우트가 자동으로 구성됩니다. 이러한 라우트는 작업자 노드와 원격 시스템 간에 VPN 터널을 통한 양방향 연결을 설정할 수 있게 해 줍니다. 예를 들면, 다음 다이어그램은 {{site.data.keyword.containershort_notm}}의 앱이 strongSwan VPN 연결을 통해 온프레미스 서버와 통신할 수 있는 방법을 보여줍니다.

<img src="images/cs_vpn_strongswan.png" width="700" alt="로드 밸런서를 사용하여 {{site.data.keyword.containershort_notm}}의 앱 노출" style="width:700px; border-style: none"/>

1. 클러스터 내의 앱 `myapp`이 Ingress 또는 LoadBalancer 서비스로부터 요청을 수신하여 온프레미스 네트워크의 데이터와 안전하게 연결해야 합니다.

2. 온프레미스 데이터센터에 대한 요청이 IPSec strongSwan VPN 팟(Pod)에 전달됩니다. 대상 IP 주소는 IPSec strongSwan VPN 팟(Pod)에 전송할 네트워크 패킷을 판별하는 데 사용됩니다. 

3. 요청이 암호화되어 VPN 터널을 통해 온프레미스 데이터 센터에 전송됩니다.

4. 수신 요청이 온프레미스 방화벽을 통과해 VPN 터널 엔드포인트(라우터)에 전달되어 여기서 복호화됩니다.

5. VPN 터널 엔드포인트(라우터)는 2단계에서 지정된 대상 IP 주소에 따라 요청을 온프레미스 서버 또는 메인프레임에 전달합니다. 필요한 데이터는 동일한 프로세스를 거쳐 VPN 연결을 통해 `myapp`에 다시 전송됩니다. 

### strongSwan Helm 차트 구성
{: #vpn_configure}

시작하기 전에:
* [온프레미스 데이터 센터에 IPsec VPN 게이트웨이를 설치하십시오](/docs/infrastructure/iaas-vpn/set-up-ipsec-vpn.html#setting-up-an-ipsec-connection).
* [표준 클러스터를 작성](cs_clusters.html#clusters_cli)하거나 [기존 클러스터를 버전 1.7.16 이상으로 업데이트](cs_cluster_update.html#master)하십시오.
* 클러스터에는 하나 이상의 사용 가능한 공인 로드 밸런서 IP 주소가 있어야 합니다. [사용 가능한 공인 IP 주소를 볼 수 있는지 확인](cs_subnets.html#manage)하거나 [사용된 IP 주소를 해제](cs_subnets.html#free)할 수 있습니다.
* [Kubernetes CLI를 클러스터에 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오.

strongSwan 차트를 설정하는 데 사용되는 Helm 명령에 대한 자세한 정보는 <a href="https://docs.helm.sh/helm/" target="_blank">Helm 문서 <img src="../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>를 참조하십시오.

Helm 차트를 구성하려면 다음을 수행하십시오.

1. [클러스터를 위해 Helm을 설치하고 {{site.data.keyword.Bluemix_notm}} 저장소를 Helm 인스턴스에 추가](cs_integrations.html#helm)하십시오.

2. strongSwan Helm 차트를 위한 기본 구성 설정을 로컬 YAML 파일에 저장하십시오.

    ```
    helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. `config.yaml` 파일을 열고 원하는 VPN 구성에 따라 기본값을 다음과 같이 변경하십시오. 구성 파일 주석에서 더 자세한 고급 설정에 대한 설명을 찾을 수 있습니다.

    **중요**: 특성을 변경할 필요가 없는 경우 해당 특성 앞에 `#`를 놓아서 주석 처리하십시오.

    <table>
    <caption>이 YAML 컴포넌트 이해</caption>
    <col width="22%">
    <col width="78%">
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code>localSubnetNAT</code></td>
    <td>서브넷의 NAT(Network Address Translation)은 로컬과 온프레미스 네트워크 간의 서브넷 충돌에 대한 임시 해결책을 제공합니다. NAT를 사용하여 클러스터의 사설 로컬 IP 서브넷, 팟(Pod) 서브넷(172.30.0.0/16) 또는 팟(Pod) 서비스 서브넷(172.21.0.0/16)을 다른 사설 서브넷에 다시 맵핑할 수 있습니다. VPN 터널에서 원래의 서브넷 대신 다시 맵핑된 IP 서브넷을 확인합니다. VPN을 통해 패킷을 전송하기 전과 VPN 터널에서 패킷이 도달된 후에 다시 맵핑이 발생합니다. VPN을 통해 동시에 다시 맵핑되고 다시 맵핑되지 않은 서브넷을 모두 노출할 수 있습니다.<br><br>NAT를 사용하려면 전체 서브넷 또는 개별 IP 주소를 추가할 수 있습니다. 전체 서브넷을 추가하는 경우(<code>10.171.42.0/24=10.10.10.0/24</code> 형식으로), 다시 맵핑은 일대일입니다. 내부 네트워크 서브넷의 모든 IP 주소가 외부 네트워크 서브넷에 맵핑되고 반대의 경우도 마찬가지입니다. 개별 IP 주소를 추가하는 경우(<code>10.171.42.17/32=10.10.10.2/32, 10.171.42.29/32=10.10.10.3/32</code> 형식으로)에는 이러한 내부 IP 주소만 지정된 외부 IP 주소에 맵핑됩니다.<br><br>이 옵션을 사용하는 경우 VPN 연결을 통해 노출된 로컬 서브넷은 "내부" 서브넷이 맵핑된 "외부" 서브넷입니다.
    </td>
    </tr>
    <tr>
    <td><code>loadBalancerIP</code></td>
    <td>인바운드 VPN 연결에 대해 strongSwan VPN 서비스의 포터블 공인 IP 주소를 지정하려는 경우에는 해당 IP 주소를 추가하십시오. 온프레미스 방화벽을 통과할 수 있는 IP 주소를 지정해야 하는 경우와 같이 고정 IP 주소가 필요한 경우에는 IP 주소를 지정하는 것이 유용합니다.<br><br>이 클러스터에 지정된 사용 가능한 포터블 공인 IP 주소를 보려면 [IP 주소 및 서브넷 관리](cs_subnets.html#manage)를 참조하십시오. 이 설정을 공백으로 두면 사용 가능한 포터블 공인 IP 주소가 사용됩니다. VPN 연결이 온프레미스 게이트웨이에서 시작된 경우(<code>ipsec.auto</code>가 <code>add</code>로 설정됨)에는 이 특성을 사용하여 클러스터의 온프레미스 게이트웨이에 지속적 공인 IP 주소를 구성할 수 있습니다.</td>
    </tr>
    <tr>
    <td><code>connectUsingLoadBalancerIP</code></td>
    <td>아웃바운드 VPN 연결 또한 설정하려는 경우에는 <code>loadBalancerIP</code>에 추가한 로드 밸런서 IP 주소를 사용하십시오. 이 옵션이 사용으로 설정되면 모든 클러스터 작업자 노드가 동일한 사설 VLAN에 있어야 합니다. 그렇지 않은 경우에는 <code>nodeSelector</code> 설정을 사용하여 VPN 팟(Pod)이 <code>loadBalancerIP</code>와 동일한 사설 VLAN의 작업자 노드에 배치되도록 해야 합니다. <code>ipsec.auto</code>가 <code>add</code>로 설정된 경우에는 이 옵션이 무시됩니다.<p>허용되는 값:</p><ul><li><code>"false"</code>: 로드 밸런서 IP를 사용하여 VPN에 연결하지 않습니다. 대신 VPN 팟(Pod)이 실행 중인 작업자 노드의 공인 IP 주소가 사용됩니다.</li><li><code>"true"</code>: 로드 밸런서 IP를 로컬 소스 IP로 사용하여 VPN을 설정합니다. <code>loadBalancerIP</code>가 설정되지 않은 경우에는 로드 밸런서 서비스에 지정된 외부 IP 주소가 사용됩니다. </li><li><code>"auto"</code>: <code>ipsec.auto</code>가 <code>start</code>로 설정되었으며 <code>loadBalancerIP</code>가 설정된 경우 로드 밸런서 IP를 로컬 소스 IP로 사용하여 VPN을 설정합니다. </li></ul></td>
    </tr>
    <tr>
    <td><code>nodeSelector</code></td>
    <td>strongSwan VPN 팟(Pod)이 배치하는 노드를 제한하려면 특정 작업자 노드의 IP 주소 또는 작업자 노드 레이블을 추가하십시오. 예를 들면, 값 <code>kubernetes.io/hostname: 10.xxx.xx.xxx</code>는 해당 작업자 노드에서만 실행되도록 VPN 팟(Pod)을 제한합니다. 값 <code>strongswan: vpn</code>은 VPN 팟(Pod)을 해당 레이블과 함께 모든 작업자 노드에서 실행하도록 제한합니다. 모든 작업자 노드 레이블을 사용할 수 있으나 이 차트의 다른 배치를 통해 다른 작업자 노드를 사용할 수 있도록 <code>strongswan: &lt;release_name&gt;</code>을 사용하는 것이 좋습니다.<br><br>VPN 연결이 클러스터로 시작되는 경우(<code>ipsec.auto</code>가 <code>start</code>로 설정됨) 이 특성을 사용하여 온프레미스 게이트웨이에 노출된 VPN 연결의 소스 IP 주소를 제한할 수 있습니다. 이 값은 선택사항입니다.</td>
    </tr>
    <tr>
    <td><code>ipsec.keyexchange</code></td>
    <td>온프레미스 VPN 터널 엔드포인트가 연결 초기화를 위한 프로토콜로 <code>ikev2</code>를 지원하지 않는 경우, 이 값을 <code>ikev1</code> 또는 <code>ike</code>로 변경하십시오.</td>
    </tr>
    <tr>
    <td><code>ipsec.esp</code></td>
    <td>온프레미스 VPN 터널 엔드포인트가 연결에 사용하는 ESP 암호화 및 인증 알고리즘의 목록을 추가하십시오.<ul><li><code>ipsec.keyexchange</code>가 <code>ikev1</code>로 설정된 경우에는 이 설정을 지정해야 합니다.</li><li><code>ipsec.keyexchange</code>가 <code>ikev2</code>로 설정된 경우 이 설정은 선택사항입니다. 이 설정을 공백으로 두면 기본 strongSwan 알고리즘 <code>aes128-sha1,3des-sha1</code>이 연결에 사용됩니다.</li></ul></td>
    </tr>
    <tr>
    <td><code>ipsec.ike</code></td>
    <td>온프레미스 VPN 터널 엔드포인트가 연결에 사용하는 IKE/ISAKMP SA 암호화 및 인증 알고리즘의 목록을 추가하십시오.<ul><li><code>ipsec.keyexchange</code>가 <code>ikev1</code>로 설정된 경우에는 이 설정을 지정해야 합니다.</li><li><code>ipsec.keyexchange</code>가 <code>ikev2</code>로 설정된 경우 이 설정은 선택사항입니다. 이 설정을 공백으로 두면 기본 strongSwan 알고리즘 <code>aes128-sha1-modp2048,3des-sha1-modp1536</code>이 연결에 사용됩니다.</li></ul></td>
    </tr>
    <tr>
    <td><code>ipsec.auto</code></td>
    <td>클러스터가 VPN 연결을 시작하도록 하려면 이 값을 <code>start</code>로 변경하십시오.</td>
    </tr>
    <tr>
    <td><code>local.subnet</code></td>
    <td>VPN 연결을 통해 온프레미스 네트워크에 노출할 클러스터 서브넷 CIDR의 목록으로 이 값을 변경하십시오. 이 목록은 다음 서브넷을 포함할 수 있습니다. <ul><li>Kubernetes 팟(Pod) 서브넷 CIDR: <code>172.30.0.0/16</code></li><li>Kubernetes 서비스 서브넷 CIDR: <code>172.21.0.0/16</code></li><li>사설 네트워크의 NodePort 서비스에서 앱을 노출한 경우 작업자 노드의 사설 서브넷 CIDR입니다. <code>bx cs worker &lt;cluster_name&gt;</code>을 실행하여 작업자 사설 IP 주소의 처음 세 옥텟을 검색하십시오. 예를 들어, 주소가 <code>&lt;10.176.48.xx&gt;</code>인 경우에는 <code>&lt;10.176.48&gt;</code>을 기록하십시오. 그 다음에는 명령 <code>bx cs subnets | grep &lt;xxx.yyy.zzz&gt;</code>를 실행(<code>&lt;xxx.yyy.zz&gt;</code>는 이전에 검색한 옥텟으로 대체)하여 작업자 사설 서브넷 CIDR을 가져오십시오.</li><li>사설 네트워크에서 LoadBalancer 서비스가 노출한 앱이 있는 경우, 클러스터의 사설 또는 사용자 관리 서브넷 CIDR입니다. 이러한 값을 찾으려면 <code>bx cs cluster-get &lt;cluster_name&gt; --showResources</code>를 실행하십시오. **VLANS** 섹션에서 **Public** 값이 <code>false</code>인 CIDR을 찾으십시오.</li></ul>**참고**: <code>ipsec.keyexchange</code>가 <code>ikev1</code>로 설정된 경우에는 하나의 서브넷만 지정할 수 있습니다.</td>
    </tr>
    <tr>
    <td><code>local.id</code></td>
    <td>VPN 터널 엔드포인트가 연결에 사용하는 로컬 Kubernetes 클러스터 측 문자열 ID로 이 값을 변경하십시오.</td>
    </tr>
    <tr>
    <td><code>remote.gateway</code></td>
    <td>온프레미스 VPN 게이트웨이의 공인 IP 주소로 이 값을 변경하십시오. <code>ipsec.auto</code>가 <code>start</code>로 설정되면 이 값은 필수입니다.</td>
    </tr>
    <td><code>remote.subnet</code></td>
    <td>Kubernetes 클러스터가 액세스하도록 허용된 온프레미스 사설 서브넷 CIDR의 목록으로 이 값을 변경하십시오. **참고**: <code>ipsec.keyexchange</code>가 <code>ikev1</code>로 설정된 경우에는 하나의 서브넷만 지정할 수 있습니다.</td>
    </tr>
    <tr>
    <td><code>remote.id</code></td>
    <td>VPN 터널 엔드포인트가 연결에 사용하는 원격 온프레미스 측 문자열 ID로 이 값을 변경하십시오.</td>
    </tr>
    <tr>
    <td><code>remote.privateIPtoPing</code></td>
    <td>VPN Ping 연결 테스트를 위해 Helm 테스트 유효성 검증 프로그램으로 사용되도록 원격 서브넷의 사설 IP 주소를 추가하십시오. 이 값은 선택사항입니다.</td>
    </tr>
    <tr>
    <td><code>preshared.secret</code></td>
    <td>온프레미스 VPN 터널 엔드포인트 게이트웨이가 연결에 사용하는 사전 공유된 시크릿으로 이 값을 변경하십시오. 이 값은 <code>ipsec.secrets</code>에 저장됩니다.</td>
    </tr>
    </tbody></table>

4. 업데이트된 `config.yaml` 파일을 저장하십시오.

5. 업데이트된 `config.yaml` 파일로 Helm 차트를 클러스터에 설치하십시오. 업데이트된 특성은 차트의 configmap에 저장됩니다.

    **참고**: 단일 클러스터에 다중 VPN 배치가 있는 경우 이름 지정 충돌을 방지할 수 있고 `vpn` 이외의 좀 더 구체적인 릴리스 이름을 선택하여 배치를 구별할 수 있습니다. 릴리스 이름이 잘리지 않으려면 릴리스 이름을 35자 미만으로 제한하십시오.

    ```
    helm install -f config.yaml --name=vpn ibm/strongswan
    ```
    {: pre}

6. 차트 배치 상태를 확인하십시오. 차트가 준비 상태인 경우, 출력의 맨 위 근처에 있는 **STATUS** 필드의 값은 `DEPLOYED`입니다.

    ```
    helm status vpn
    ```
    {: pre}

7. 차트가 배치된 후 `config.yaml` 파일에서 업데이트된 설정이 사용되었는지 확인하십시오.

    ```
    helm get values vpn
    ```
    {: pre}


### VPN 연결 테스트 및 확인
{: #vpn_test}

Helm 차트를 배치한 후 VPN 연결을 테스트하십시오.
{:shortdesc}

1. 온프레미스 게이트웨이에서 VPN이 활성이 아닌 경우 VPN을 시작하십시오.

2. `STRONGSWAN_POD` 환경 변수를 설정하십시오.

    ```
    export STRONGSWAN_POD=$(kubectl get pod -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
    ```
    {: pre}

3. VPN의 상태를 확인하십시오. `ESTABLISHED` 상태는 VPN 연결이 성공임을 의미합니다.

    ```
    kubectl exec $STRONGSWAN_POD -- ipsec status
    ```
    {: pre}

    출력 예:

    ```
    Security Associations (1 up, 0 connecting):
    k8s-conn[1]: ESTABLISHED 17 minutes ago, 172.30.xxx.xxx[ibm-cloud]...192.xxx.xxx.xxx[on-premises]
    k8s-conn{2}: INSTALLED, TUNNEL, reqid 12, ESP in UDP SPIs: c78cb6b1_i c5d0d1c3_o
    k8s-conn{2}: 172.21.0.0/16 172.30.0.0/16 === 10.91.152.xxx/26
    ```
    {: screen}

    **참고**:

    <ul>
    <li>strongSwan Helm 차트를 사용하여 VPN 연결을 설정하려는 경우 처음에 VPN 상태는 `ESTABLISHED`가 아닐 수 있습니다. 연결이 성공하기 전에 온프레미스 VPN 엔드포인트 설정을 확인하고 구성 파일을 여러 번 변경해야 할 수도 있습니다. <ol><li>`helm delete --purge <release_name>을 실행하십시오.`</li><li>구성 파일에서 올바르지 않은 값을 수정하십시오.</li><li>`helm install -f config.yaml --name=<release_name> ibm/strongswan`을 실행하십시오.</li></ol>다음 단계에서 추가 확인을 실행할 수도 있습니다.</li>
    <li>VPN 팟(Pod)이 `ERROR` 상태이거나 계속 충돌하고 다시 시작되는 경우, 차트의 configmap에 있는 `ipsec.conf` 설정의 매개변수 유효성 검증 때문일 수 있습니다.<ol><li>`kubectl logs -n $STRONGSWAN_POD`를 실행하여 strongSwan 팟(Pod) 로그에서 유효성 검증 오류가 있는지 확인하십시오.</li><li>유효성 검증 오류가 있는 경우 `helm delete --purge <release_name>`을 실행하십시오.<li>구성 파일에서 올바르지 않은 값을 수정하십시오.</li><li>`helm install -f config.yaml --name=<release_name> ibm/strongswan`을 실행하십시오.</li></ol>클러스터에 많은 수의 작업자 로드가 있는 경우에는 `helm delete` 및 `helm install`을 실행하는 대신 `helm upgrade`를 사용하여 변경사항을 더 빠르게 적용할 수도 있습니다.</li>
    </ul>

4. strongSwan 차트 정의에 포함된 5회의 Helm 테스트를 실행하여 VPN 연결을 추가로 테스트할 수 있습니다.

    ```
    helm test vpn
    ```
    {: pre}

    * 모든 테스트에 통과하면 strongSwan VPN 연결이 설정됩니다.

    * 테스트에 실패한 경우 다음 단계로 진행하십시오.

5. 테스트 팟(Pod)의 로그를 확인하여 실패한 테스트의 출력을 보십시오.

    ```
    kubectl logs <test_program>
    ```
    {: pre}

    **참고**: 일부 테스트에는 VPN 구성에서 선택적 설정인 요구사항이 포함됩니다. 일부 테스트가 실패하는 경우 실패는 선택적 설정의 지정 여부에 따라 허용될 수 있습니다. 각 테스트에 대한 자세한 정보와 실패 이유는 다음 표를 참조하십시오.

    {: #vpn_tests_table}
    <table>
    <caption>Helm VPN 연결 테스트 이해</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> Helm VPN 연결 테스트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code>vpn-strongswan-check-config</code></td>
    <td><code>config.yaml</code> 파일에서 생성된 <code>ipsec.conf</code> 파일의 구문을 유효성 검증하십시오. 이 테스트는 <code>config.yaml</code> 파일의 올바르지 않은 값으로 인해 실패할 수 있습니다.</td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-check-state</code></td>
    <td>VPN 연결이 <code>ESTABLISHED</code> 상태인지 확인합니다. 다음과 같은 이유로 인해 이 테스트가 실패할 수 있습니다.<ul><li><code>config.yaml</code> 파일의 값 및 온프레미스 VPN 엔드포인트 설정 간에 차이점이 있습니다.</li><li>클러스터가 "청취" 모드에 있는 경우(<code>ipsec.auto</code>가 <code>add</code>로 설정됨) 연결이 온프레미스 측에 설정되지 않습니다.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-gw</code></td>
    <td><code>config.yaml</code> 파일에 구성한 <code>remote.gateway</code> 공인 IP 주소에 대해 ping을 실행합니다. 다음과 같은 이유로 인해 이 테스트가 실패할 수 있습니다.<ul><li>온프레미스 VPN 게이트웨이 IP 주소를 지정하지 않았습니다. <code>ipsec.auto</code>가 <code>start</code>로 설정된 경우 <code>remote.gateway</code> IP 주소가 필수입니다.</li><li>VPN 연결이 <code>ESTABLISHED</code> 상태가 아닙니다. 자세한 정보는 <code>vpn-strongswan-check-state</code>를 참조하십시오.</li><li>VPN 연결 상태는 <code>ESTABLISHED</code>이지만 ICMP 패킷이 방화벽으로 차단됩니다.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-ip-1</code></td>
    <td>클러스터의 VPN 팟(Pod)에서 온프레미스 VPN 게이트웨이의 <code>remote.privateIPtoPing</code> 게이트웨이 IP 주소에 대해 ping을 실행합니다. 다음과 같은 이유로 인해 이 테스트가 실패할 수 있습니다.<ul><li><code>remote.privateIPtoPing</code> IP 주소를 지정하지 않았습니다. 의도적으로 IP 주소를 지정하지 않은 경우 이 실패는 허용됩니다.</li><li><code>local.subnet</code> 목록에 있는 클러스터 팟(Pod) 서브넷 CIDR인 <code>172.30.0.0/16</code>을 지정하지 않았습니다.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-ip-2</code></td>
    <td>클러스터의 작업자 노드에서 온프레미스 VPN 게이트웨이의 <code>remote.privateIPtoPing</code> 게이트웨이 IP 주소에 대해 ping을 실행합니다. 다음과 같은 이유로 인해 이 테스트가 실패할 수 있습니다.<ul><li><code>remote.privateIPtoPing</code> IP 주소를 지정하지 않았습니다. 의도적으로 IP 주소를 지정하지 않은 경우 이 실패는 허용됩니다.</li><li><code>local.subnet</code> 목록에 있는 클러스터 작업자 노드 사설 서브넷 CIDR을 지정하지 않았습니다.</li></ul></td>
    </tr>
    </tbody></table>

6. 현재 Helm 차트를 삭제하십시오.

    ```
    helm delete --purge vpn
    ```
    {: pre}

7. `config.yaml` 파일을 열고 올바르지 않은 값을 수정하십시오.

8. 업데이트된 `config.yaml` 파일을 저장하십시오.

9. 업데이트된 `config.yaml` 파일로 Helm 차트를 클러스터에 설치하십시오. 업데이트된 특성은 차트의 configmap에 저장됩니다.

    ```
    helm install -f config.yaml --name=<release_name> ibm/strongswan
    ```
    {: pre}

10. 차트 배치 상태를 확인하십시오. 차트가 준비 상태인 경우, 출력의 맨 위 근처에 있는 **STATUS** 필드의 값은 `DEPLOYED`입니다.

    ```
    helm status vpn
    ```
    {: pre}

11. 차트가 배치된 후 `config.yaml` 파일에서 업데이트된 설정이 사용되었는지 확인하십시오.

    ```
    helm get values vpn
    ```
    {: pre}

12. 현재 테스트 팟(Pod)을 정리하십시오.

    ```
    kubectl get pods -a -l app=strongswan-test
    ```
    {: pre}

    ```
    kubectl delete pods -l app=strongswan-test
    ```
    {: pre}

13. 테스트를 다시 실행하십시오.

    ```
    helm test vpn
    ```
    {: pre}

<br />


## strongSwan Helm 차트 업그레이드
{: #vpn_upgrade}

strongSwan Helm 차트를 업그레이드하여 최신 상태인지 확인하십시오.
{:shortdesc}

strongSwan Helm 차트를 최신 버전으로 업그레이드하려면 다음을 수행하십시오.

  ```
  helm upgrade -f config.yaml <release_name> ibm/strongswan
  ```
  {: pre}

**중요**: strongSwan 2.0.0 Helm 차트는 Calico v3 또는 Kubernetes 1.10에서 작동하지 않습니다. [클러스터를 1.10으로 업데이트](cs_versions.html#cs_v110)하기 전에 Calico 2.6 및 Kubernetes 1.7, 1.8, 1.9와 역호환 가능한 2.1.0 Helm 차트로 strongSwan을 업데이트하십시오.


### 버전 1.0.0에서 업그레이드
{: #vpn_upgrade_1.0.0}

버전 1.0.0 Helm 차트에 사용된 일부 설정으로 인해 `helm upgrade`를 사용하여 1.0.0에서 최신 버전으로 업데이트할 수 없습니다.
{:shortdesc}

버전 1.0.0에서 업그레이드하려면 1.0.0 차트를 삭제하고 최신 버전을 설치해야 합니다.

1. 1.0.0 Helm 차트를 삭제하십시오.

    ```
    helm delete --purge <release_name>
    ```
    {: pre}

2. 최신 버전의 strongSwan Helm 차트를 위한 기본 구성 설정을 로컬 YAML 파일에 저장하십시오.

    ```
    helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. 구성 파일을 업데이트하고 변경사항을 포함하여 파일을 저장하십시오.

4. 업데이트된 `config.yaml` 파일로 Helm 차트를 클러스터에 설치하십시오.

    ```
    helm install -f config.yaml --name=<release_name> ibm/strongswan
    ```
    {: pre}

또한 1.0.0에서 하드 코딩된 특정 `ipsec.conf` 제한시간 설정은 이후 버전에서 구성 가능한 특성으로 노출됩니다. 일부 구성 가능한 `ipsec.conf` 제한시간 설정의 이름 및 기본값도 strongSwan 표준과 좀 더 일치하도록 변경되었습니다. 1.0.0에서 Helm 차트를 업그레이드하는 중이고 제한시간 설정의 1.0.0 버전 기본값을 보존하려면 이전 기본값을 사용하여 새 설정을 차트 구성 파일에 추가하십시오.



  <table>
  <caption>버전 1.0.0과 최신 버전 간의 ipsec.conf 설정 차이점</caption>
  <thead>
  <th>1.0.0 설정 이름</th>
  <th>1.0.0 기본값</th>
  <th>최신 버전 설정 이름</th>
  <th>최신 버전 기본값</th>
  </thead>
  <tbody>
  <tr>
  <td><code>ikelifetime</code></td>
  <td>60m</td>
  <td><code>ikelifetime</code></td>
  <td>3h</td>
  </tr>
  <tr>
  <td><code>keylife</code></td>
  <td>20m</td>
  <td><code>lifetime</code></td>
  <td>1h</td>
  </tr>
  <tr>
  <td><code>rekeymargin</code></td>
  <td>3m</td>
  <td><code>margintime</code></td>
  <td>9m</td>
  </tr>
  </tbody></table>


## strongSwan IPSec VPN 서비스 사용 안함
{: vpn_disable}

Helm 차트를 삭제하여 VPN 연결을 사용 안함으로 설정할 수 있습니다.
{:shortdesc}

  ```
  helm delete --purge <release_name>
  ```
  {: pre}

<br />


## 가상 라우터 어플라이언스를 사용하여 VPN 연결 설정
{: #vyatta}

[가상 라우터 어플라이언스(VRA)](/docs/infrastructure/virtual-router-appliance/about.html)는 x86 베어메탈 서버에 대한 최신 Vyatta 5600 운영 체제를 제공합니다. VRA를 VPN 게이트웨이로 사용하여 온프레미스 네트워크에 안전하게 연결할 수 있습니다.
{:shortdesc}

클러스터 VLAN에 들어오거나 나가는 모든 공용 및 사설 네트워크 트래픽은 VRA를 통해 라우팅됩니다. VRA를 VPN 엔드포인트로 사용하여 IBM Cloud 인프라(SoftLayer)의 서버와 온프레미스 리소스 간에 암호화된 IPSec 터널을 작성할 수 있습니다. 예를 들어, 다음 다이어그램은 {{site.data.keyword.containershort_notm}}에 있는 개인 전용 작업자 노드의 앱이 VRA VPN 연결을 통해 온프레미스 서버와 통신할 수 있는 방법을 보여줍니다. 

<img src="images/cs_vpn_vyatta.png" width="725" alt="로드 밸런서를 사용하여 {{site.data.keyword.containershort_notm}}의 앱 노출" style="width:725px; border-style: none"/>

1. 클러스터 내의 앱 `myapp2`가 Ingress 또는 LoadBalancer 서비스로부터 요청을 수신하여 온프레미스 네트워크의 데이터와 안전하게 연결해야 합니다.

2. `myapp2`는 사설 VLAN 전용의 작업자 노드에 있으므로 VRA는 작업자 노드와 온프레미스 네트워크 간의 보안 연결 역할을 수행합니다. VRA는 대상 IP 주소를 사용하여 온프레미스 네트워크에 전송할 네트워크 패킷을 판별합니다. 

3. 요청이 암호화되어 VPN 터널을 통해 온프레미스 데이터 센터에 전송됩니다.

4. 수신 요청이 온프레미스 방화벽을 통과해 VPN 터널 엔드포인트(라우터)에 전달되어 여기서 복호화됩니다.

5. VPN 터널 엔드포인트(라우터)는 2단계에서 지정된 대상 IP 주소에 따라 요청을 온프레미스 서버 또는 메인프레임에 전달합니다. 필요한 데이터는 동일한 프로세스를 거쳐 VPN 연결을 통해 `myapp2`에 다시 전송됩니다. 

가상 라우터 어플라이언스를 설정하려면 다음을 수행하시시오.

1. [VRA를 주문](/docs/infrastructure/virtual-router-appliance/getting-started.html)하십시오.

2. [VRA에서 사설 VLAN을 구성](/docs/infrastructure/virtual-router-appliance/manage-vlans.html)하십시오.

3. VRA를 사용하여 VPN 연결을 사용으로 설정하려면 [VRA에서 VRRP를 구성](/docs/infrastructure/virtual-router-appliance/vrrp.html#high-availability-vpn-with-vrrp)하십시오.
