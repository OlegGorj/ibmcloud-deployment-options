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



# 버전 정보 및 업데이트 조치
{: #cs_versions}

## Kubernetes 버전 유형
{: #version_types}

{{site.data.keyword.containerlong}}는 동시에 여러 Kubernetes 버전을 지원합니다. 최신 버전이 릴리스될 때 최대 두 개의 이전 버전이(n-2)이 지원됩니다. 최신 버전보다 세 개 이상의 이전 버전(n-3)이 먼저 더 이상 사용되지 않게 되고 지원되지 않습니다.
{:shortdesc}

현재 지원되는 Kubernetes 버전은 다음과 같습니다.

- 최신 버전: 1.10.1
- 기본 버전: 1.9.7
- 지원되는 버전: 1.8.11

**더 이상 사용되지 않는 버전**: 클러스터가 더 이상 사용되지 않는 Kubernetes에서 실행되는 경우 버전 지원이 중단되기 전에 지원되는 Kubernetes 버전을 검토하고 업데이트하기 위한 30일의 기간이 주어집니다. 지원 중단 중에 클러스터에서 제한된 명령을 실행하여 작업자를 추가하고 작업자를 다시 로드하며 클러스터를 업데이트할 수 있습니다. 더 이상 사용되지 않는 버전에 새 클러스터를 작성할 수 없습니다.

**지원되지 않는 버전**: 지원되지 않는 Kubernetes 버전에서 클러스터를 실행 중인 경우 업데이트에 대한 [잠재적인 영향을 검토](#version_types)한 후 즉각적으로 [클러스터를 업데이트](cs_cluster_update.html#update)하여 중요한 보안 업데이트 및 지원을 계속 받으십시오.

클러스터의 서버 버전을 확인하려면 다음 명령을 실행하십시오.

```
kubectl version  --short | grep -i server
```
{: pre}

출력 예:

```
Server Version: v1.9.7+9d6e0610086578
```
{: screen}


## 업데이트 유형
{: #update_types}

Kubernetes 클러스터에 대한 업데이트에는 세 가지 유형(주 버전, 부 버전 및 패치)가 있습니다.
{:shortdesc}

|업데이트 유형|버전 레이블의 예|업데이트 수행자|영향
|-----|-----|-----|-----|
|주 버전|1.x.x|사용자|스크립트 또는 배치를 포함한 클러스터의 오퍼레이션 변경.|
|부 버전|x.9.x|사용자|스크립트 또는 배치를 포함한 클러스터의 오퍼레이션 변경.|
|패치|x.x.4_1510|IBM 및 사용자|Kubernetes 패치 및 기타 {{site.data.keyword.Bluemix_notm}} Provider 컴포넌트 업데이트(보안 및 운영 체제 패치 등)입니다. IBM은 자동으로 마스터를 업데이트하지만 사용자는 작업자 노드에 패치를 적용합니다.|
{: caption="Kubernetes 업데이트의 영향" caption-side="top"}

업데이트가 사용 가능해지면 `bx cs workers <cluster>` 또는 `bx cs worker-get <cluster> <worker>` 명령 등을 사용하여 작업자 노드에 대한 정보를 볼 때 알림을 받습니다.
-  **주 버전 및 부 버전 업데이트**: 먼저 [마스터 노드를 업데이트](cs_cluster_update.html#master)한 후 [작업자 노드를 업데이트](cs_cluster_update.html#worker_node)하십시오.  
   - 기본적으로 Kubernetes 마스터를 3개 이상의 부 버전 이후로 업데이트할 수 없습니다. 예를 들어, 현재 마스터가 버전 1.5이고 1.8로 업데이트하려면 먼저 1.7로 업데이트해야 합니다. 업데이트를 강제로 계속할 수 있지만 2를 넘는 부 버전 업데이트로 인해 예상치 못한 결과가 발생할 수 있습니다.
   - 클러스터의 최소 `major.minor` CLI 버전과 일치하는 `kubectl` CLI 버전을 사용하는 경우 예상치 못한 결과가 발생할 수 있습니다. Kubernetes 클러스터 및 [CLI 버전](cs_cli_install.html#kubectl)을 최신 상태로 유지해야 합니다.
-  **패치 업데이트**: 매월 업데이트가 사용 가능한지 확인하고 `bx cs worker-update` [명령](cs_cli_reference.html#cs_worker_update) 또는 `bx cs worker-reload` [명령](cs_cli_reference.html#cs_worker_reload)을 사용하여 이러한 보안 및 운영 체제 패치를 적용하십시오. 자세한 정보는 [버전 변경 로그](cs_versions_changelog.html)를 참조하십시오.

<br/>

이 정보에는 클러스터를 이전 버전에서 새 버전으로 업데이트할 때 배치된 앱에 영향을 미칠 수 있는 업데이트가 요약되어 있습니다. 
-  버전 1.10 [마이그레이션 조치](#cs_v110).
-  버전 1.9 [마이그레이션 조치](#cs_v19).
-  버전 1.8 [마이그레이션 조치](#cs_v18).
-  더 이상 사용되지 않거나 지원되지 않는 버전의 [아카이브](#k8s_version_archive).

<br/>

전체 변경사항 목록은 다음 정보를 검토하십시오. 
* [Kubernetes 변경 로그 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md).
* [IBM 버전 변경 로그](cs_versions_changelog.html).

## 버전 1.10
{: #cs_v110}

<p><img src="images/certified_kubernetes_1x10.png" style="padding-right: 10px;" align="left" alt="이 배지는 IBM Cloud 컨테이너 서비스에 대한 Kubernetes 버전 1.10 인증을 표시합니다."/> {{site.data.keyword.containerlong_notm}}는 CNCF Kubernetes Software Conformance Certification 프로그램에서 버전 1.10에 대해 인증된 Kubernetes 제품입니다. _Kubernetes®는 미국 또는 기타 국가에서 사용되는 Linux Foundation의 등록상표이며, Linux Foundation의 라이센스에 따라 사용됩니다. _</p>

이전 Kubernetes 버전에서 1.10으로 업데이트 중일 때 작성해야 할 변경사항을 검토하십시오.

**중요**: Kubernetes 1.10으로 업데이트하려면 먼저 [Calico v3으로 업데이트 준비](#110_calicov3)에 나열된 단계를 따라야 합니다.

<br/>

### 마스터 이전 업데이트
{: #110_before}

<table summary="버전 1.10에 대한 Kubernetes 업데이트">
<caption>마스터를 Kubernetes 1.10으로 업데이트하기 전에 작성할 변경사항</caption>
<thead>
<tr>
<th>유형</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico v3</td>
<td>Kubernetes 버전 1.10으로 업데이트하면 Calico도 v2.6.5에서 v3.1.1로 업데이트됩니다. <strong>중요</strong>: Kubernetes v1.10으로 업데이트하려면 먼저 [Calico v3으로 업데이트 준비](#110_calicov3)에 나열된 단계를 따라야 합니다.</td>
</tr>
<tr>
<td>Kubernetes 대시보드 네트워크 정책</td>
<td>Kubernetes 1.10에서는 <code>kube-system</code> 네임스페이스의 <code>kubernetes-dashboard</code> 네트워크 정책이 모든 팟(Pod)이 Kubernetes 대시보드에 액세스하지 못하도록 차단합니다. 그러나 {{site.data.keyword.Bluemix_notm}} 콘솔에서 또는 <code>kubectl proxy</code>를 사용하여 대시보드에 액세스하는 기능에는 영향을 미치지 <strong>않습니다</strong>. 팟(Pod)에 대시보드에 대한 액세스 권한이 필요한 경우 네임스페이스에 <code>kubernetes-dashboard-policy: allow</code> 레이블을 추가한 후 팟(Pod)을 네임스페이스에 배치할 수 있습니다.</td>
</tr>
<tr>
<td>Kubelet API 액세스</td>
<td>이제 Kubelet API 권한이 <code>Kubernetes API server</code>에 위임됩니다. Kubelet API에 대한 액세스는 <strong>노드</strong> 하위 리소스에 액세스하기 위한 권한을 부여하는 <code>ClusterRoles</code>를 기반으로 합니다. 기본적으로 Kubernetes Heapster에는 <code>ClusterRole</code> 및 <code>ClusterRoleBinding</code>이 있습니다. 그러나 다른 사용자 또는 앱에서 Kubelet API를 사용하는 경우 해당 사용자 또는 앱에 API를 사용할 수 있는 권한을 부여해야 합니다. [Kubelet 권한 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/admin/kubelet-authentication-authorization/#kubelet-authorization)에 대한 Kubernetes 문서를 참조하십시오.</td>
</tr>
<tr>
<td>암호 스위트</td>
<td><code>Kubernetes API server</code> 및 Kubelet API에 대해 지원되는 암호 스위트가 이제 높은 강도의 암호화(128비트 이상)를 사용하는 서브세트로 제한됩니다. 강도가 낮은 암호를 사용하고 <code>Kubernetes API server</code> 또는 Kubelet API와의 통신에 의존하는 기존 자동화 또는 리소스가 있는 경우 마스터를 업데이트하기 전에 더 강력한 암호 지원을 사용으로 설정하십시오.</td>
</tr>
</tbody>
</table>

### 마스터 이후 업데이트
{: #110_after}

<table summary="버전 1.10에 대한 Kubernetes 업데이트">
<caption>마스터를 Kubernetes 1.10으로 업데이트한 후 작성할 변경사항</caption>
<thead>
<tr>
<th>유형</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico v3</td>
<td>클러스터가 업데이트되면 클러스터에 적용된 기존의 모든 Calico 데이터가 Calico v3 구문을 사용하도록 자동으로 마이그레이션됩니다. Calico v3 구문을 사용하여 Calico 리소스를 보거나 추가하거나 수정하려면 [Calico CLI 구성을 버전 3.1.1로](#110_calicov3) 업데이트하십시오.</td>
</tr>
<tr>
<td>노드 <code>ExternalIP</code> 주소</td>
<td>이제 노드의 <code>ExternalIP</code> 필드가 노드의 공인 IP 주소 값으로 설정됩니다. 이 값이 종속되는 모든 리소스를 검토하고 업데이트하십시오.</td>
</tr>
<tr>
<td><code>kubectl port-forward</code></td>
<td>이제 <code>kubectl port-forward</code> 명령을 사용할 때 <code>-p</code> 플래그를 더 이상 지원하지 않습니다. 스크립트가 이전 동작에 의존하는 경우 <code>-p</code> 플래그를 팟(Pod) 이름으로 대체하도록 스크립트를 업데이트하십시오.</td>
</tr>
<tr>
<td>읽기 전용 API 데이터 볼륨</td>
<td>이제 `secret`, `configMap`, `downwardAPI` 및 투영 볼륨은 읽기 전용으로 마운트됩니다.
이전에는 시스템이 자동으로 되돌릴 수 있는 이러한 볼륨에 앱이 데이터를
기록할 수 있었습니다. 이 마이그레이션 조치는 보안 취약성 [CVE-2017-1002102![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2017-1002102)을 수정하는 데 필요합니다.
앱이 이전의 안전하지 않은 작동에 의존하는 경우에는 이를 적절히 수정하십시오.</td>
</tr>
</tbody>
</table>

### Calico v3으로 업데이트 준비
{: #110_calicov3}

시작하기 전에 클러스터 마스터 및 모든 작업자 노드가 Kubernetes 버전 1.8 이상을 실행 중이어야 하며 하나 이상의 작업자 노드를 보유해야 합니다.

**중요**: 마스터를 업데이트하기 전에 Calico v3 업데이트를 준비하십시오. 마스터를 Kubernetes v1.10으로 업그레이드하는 동안 새 팟(Pod) 및 새 Kubernetes 또는 Calico 네트워크 정책이 스케줄되지 않습니다. 업데이트로 인해 새로 스케줄링되지 못하는 시간은 경우에 따라 다릅니다. 소규모 클러스터에서는 몇 분이 소요되며 10개의 노드마다 몇 분이 더 추가될 수 있습니다. 기존 네트워크 정책 및 팟(Pod)은 계속 실행됩니다.

1.  Calico 팟(Pod)이 정상 상태인지 확인하십시오.
    ```
    kubectl get pods -n kube-system -l k8s-app=calico-node -o wide
    ```
    {: pre}
    
2.  팟(Pod)이 **실행 중** 상태가 아니면 해당 팟(Pod)을 삭제하고 계속하기 전에 **실행 중** 상태가 될 때까지 기다리십시오.

3.  Calico 정책 또는 Calico 리소스를 자동 생성하는 경우 [Calico v3 구문 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/)을 사용하여 이러한 리소스를 생성하도록 자동화 도구를 업데이트하십시오.

4.  VPN 연결을 위해 [strongSwan](cs_vpn.html#vpn-setup)을 사용하는 경우 strongSwan 2.0.0 Helm 차트는 Calico v3 또는 Kubernetes 1.10에서 작동하지 않습니다. Calico 2.6 및 Kubernetes 1.7, 1.8, 1.9와 역호환 가능한 2.1.0 Helm 차트로 [strongSwan을 업데이트](cs_vpn.html#vpn_upgrade)하십시오.

5.  [클러스터 마스터를 Kubernetes v1.10으로 업데이트](cs_cluster_update.html#master)하십시오.

<br />


## 버전 1.9
{: #cs_v19}

<p><img src="images/certified_kubernetes_1x9.png" style="padding-right: 10px;" align="left" alt="이 배지는 IBM Cloud 컨테이너 서비스에 대한 Kubernetes 버전 1.9 인증을 표시합니다."/> {{site.data.keyword.containerlong_notm}}는 CNCF Kubernetes Software Conformance Certification 프로그램에서 버전 1.9에 대해 인증된 Kubernetes 제품입니다. _Kubernetes®는 미국 또는 기타 국가에서 사용되는 Linux Foundation의 등록상표이며, Linux Foundation의 라이센스에 따라 사용됩니다. _</p>

이전 Kubernetes 버전에서 1.9로 업데이트 중일 때 작성해야 할 변경사항을 검토하십시오.

<br/>

### 마스터 이전 업데이트
{: #19_before}

<table summary="버전 1.9에 대한 Kubernetes 업데이트">
<caption>마스터를 Kubernetes 1.9로 업데이트하기 전에 작성할 변경사항</caption>
<thead>
<tr>
<th>유형</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>웹훅 허가 API</td>
<td>API 서버가 허가 제어 웹훅을 호출할 때 사용되는 허가 API가 <code>admission.v1alpha1</code>에서 <code>admission.v1beta1</code>로 이동되었습니다. <em>클러스터를 업그레이드하기 전에 기존 웹훅을 삭제</em>하고 최신 API를 사용하도록 웹훅 구성 파일을 업데이트해야 합니다. 이 변경사항은 이전 버전과 호환 가능하지 않습니다.</td>
</tr>
</tbody>
</table>

### 마스터 이후 업데이트
{: #19_after}

<table summary="버전 1.9에 대한 Kubernetes 업데이트">
<caption>마스터를 Kubernetes 1.9로 업데이트한 후 작성할 변경사항</caption>
<thead>
<tr>
<th>유형</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>`kubectl` 출력</td>
<td>이제 `kubectl` 명령을 사용하여 `-o custom-columns`를 지정할 때 열이 오브젝트에 없는 경우 `<none>` 출력이 표시됩니다.<br>
이전에는 오퍼레이션이 실패하고 `xxx is not found`라는 오류 메시지가 표시되었습니다. 스크립트가 이전 동작에 의존하는 경우 스크립트를 업데이트하십시오.</td>
</tr>
<tr>
<td>`kubectl patch`</td>
<td>이제 패치된 리소스가 변경되지 않으면 `kubectl patch` 명령이 `exit code 1`과 함께 실패합니다. 스크립트가 이전 동작에 의존하는 경우 스크립트를 업데이트하십시오.</td>
</tr>
<tr>
<td>Kubernetes 대시보드 권한</td>
<td>사용자가 클러스터 리소스를 보려면 해당 신임 정보를 사용하여 Kubernetes 대시보드에 로그인해야 합니다. 기본 Kubernetes 대시보드 `ClusterRoleBinding` RBAC 권한이 제거되었습니다. 지시사항은 [Kubernetes 대시보드 실행](cs_app.html#cli_dashboard)을 참조하십시오.</td>
</tr>
<tr>
<td>읽기 전용 API 데이터 볼륨</td>
<td>이제 `secret`, `configMap`, `downwardAPI` 및 투영 볼륨은 읽기 전용으로 마운트됩니다.
이전에는 시스템이 자동으로 되돌릴 수 있는 이러한 볼륨에 앱이 데이터를
기록할 수 있었습니다. 이 마이그레이션 조치는 보안 취약성
[CVE-2017-1002102](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2017-1002102)를 수정하는 데 필요합니다.
앱이 이전의 안전하지 않은 작동에 의존하는 경우에는 이를 적절히 수정하십시오.</td>
</tr>
<tr>
<td>오염 및 결함 허용</td>
<td>`node.alpha.kubernetes.io/notReady` 및 `node.alpha.kubernetes.io/unreachable` 오염이 각각 `node.kubernetes.io/not-ready` 및 `node.kubernetes.io/unreachable`로 변경되었습니다.<br>
오염이 자동으로 업데이트되지만 이러한 오염에 대한 결함 허용을 수동으로 업데이트해야 합니다. `ibm-system` 및 `kube-system`을 제외한 각 네임스페이스에서 결함 허용을 변경할지 여부를 판별하십시오.<br>
<ul><li><code>kubectl get pods -n &lt;namespace&gt; -o yaml | grep "node.alpha.kubernetes.io/notReady" && echo "Action required"</code></li><li>
<code>kubectl get pods -n &lt;namespace&gt; -o yaml | grep "node.alpha.kubernetes.io/unreachable" && echo "Action required"</code></li></ul><br>
`Action required`가 리턴되면 팟(Pod) 결함 허용을 적절히 수정하십시오.</td>
</tr>
<tr>
<td>웹훅 허가 API</td>
<td>클러스터를 업데이트하기 전에 기존 웹훅을 삭제한 경우 새 웹훅을 작성하십시오.</td>
</tr>
</tbody>
</table>

<br />



## 버전 1.8
{: #cs_v18}

<p><img src="images/certified_kubernetes_1x8.png" style="padding-right: 10px;" align="left" alt="이 배지는 IBM Cloud 컨테이너 서비스에 대한 Kubernetes 버전 1.8 인증을 표시합니다."/> {{site.data.keyword.containerlong_notm}}는 CNCF Kubernetes Software Conformance Certification 프로그램에서 버전 1.8에 대해 인증된 Kubernetes 제품입니다. _Kubernetes®는 미국 또는 기타 국가에서 사용되는 Linux Foundation의 등록상표이며, Linux Foundation의 라이센스에 따라 사용됩니다. _</p>

이전 Kubernetes 버전에서 1.8로 업데이트 중일 때 작성해야 할 변경사항을 검토하십시오.

<br/>

### 마스터 이전 업데이트
{: #18_before}

<table summary="버전 1.8의 Kubernetes 업데이트">
<caption>마스터를 Kubernetes 1.8로 업데이트하기 전에 작성할 변경사항</caption>
<thead>
<tr>
<th>유형</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>없음</td>
<td>마스터를 업데이트하기 전에 필요한 변경사항 없음</td>
</tr>
</tbody>
</table>

### 마스터 이후 업데이트
{: #18_after}

<table summary="버전 1.8의 Kubernetes 업데이트">
<caption>마스터를 Kubernetes 1.8로 업데이트한 후 작성할 변경사항</caption>
<thead>
<tr>
<th>유형</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes 대시보드 로그인</td>
<td>버전 1.8에서 Kubernetes 대시보드에 액세스하기 위한 URL이 변경되었고 로그인 프로세스에 새 인증 단계가 포함됩니다. 예를 들어, [Kubernetes 대시보드에 액세스](cs_app.html#cli_dashboard)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes 대시보드 권한</td>
<td>사용자가 신임 정보로 로그인하여 버전 1.8에서 클러스터 리소스를 보도록 하려면 1.7 ClusterRoleBinding RBAC 권한을 제거하십시오. `kubectl delete clusterrolebinding -n kube-system kubernetes-dashboard`를 실행하십시오.</td>
</tr>
<tr>
<td>`kubectl delete`</td>
<td>`kubectl delete` 명령은 오브젝트가 삭제되기 전에 팟(Pod)과 같은 워크로드 API 오브젝트를 더 이상 축소하지 않습니다. 오브젝트를 축소해야 하는 경우, 오브젝트를 삭제하기 전에 [`kubectl 스케일 ` ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/reference/kubectl/overview/#scale)을 사용하십시오.</td>
</tr>
<tr>
<td>`kubectl run`</td>
<td>`kubectl run` 명령은 쉼표로 구분된 인수 대신 `--env`의 다중 플래그를 사용해야 합니다. 예를 들어, <code>kubectl run --env &lt;x&gt;=&lt;y&gt;,&lt;z&gt;=&lt;a&gt;</code>가 아니라 <code>kubectl run --env &lt;x&gt;=&lt;y&gt; --env &lt;z&gt;=&lt;a&gt;</code>를 실행하십시오. </td>
</tr>
<tr>
<td>`kubectl stop`</td>
<td>`kubectl stop` 명령은 더 이상 사용 가능하지 않습니다.</td>
</tr>
<tr>
<td>읽기 전용 API 데이터 볼륨</td>
<td>이제 `secret`, `configMap`, `downwardAPI` 및 투영 볼륨은 읽기 전용으로 마운트됩니다.
이전에는 시스템이 자동으로 되돌릴 수 있는 이러한 볼륨에 앱이 데이터를
기록할 수 있었습니다. 이 마이그레이션 조치는 보안 취약성
[CVE-2017-1002102](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2017-1002102)를 수정하는 데 필요합니다.
앱이 이전의 안전하지 않은 작동에 의존하는 경우에는 이를 적절히 수정하십시오.</td>
</tr>
</tbody>
</table>

<br />



## 아카이브
{: #k8s_version_archive}

### 버전 1.7(더 이상 사용되지 않음)
{: #cs_v17}

**2018년 5월 22일부터 Kubernetes 버전 1.7을 실행하는 {{site.data.keyword.containershort_notm}} 클러스터는 더 이상 시용되지 않습니다**. 2018년 6월 21일 이후 버전 1.7 클러스터는 다음 최신 버전([Kubernetes 1.8](#cs_v18))으로 업데이트되지 않는 한 보안 업데이트 또는 지원을 받을 수 없습니다.

각 Kubernetes 버전 업데이트의 [잠재적인 영향을 검토](cs_versions.html#cs_versions)한 후 즉시 [클러스터를 업데이트](cs_cluster_update.html#update)하십시오.

여전히 Kubernetes 버전 1.5를 실행 중입니까? 다음 정보를 검토하여 클러스터를 v1.5에서 v1.7로 업데이트할 때의 영향을 평가하십시오. [클러스터를 v1.7로 업데이트](cs_cluster_update.html#update)한 후 즉시 v1.8 이상으로 업데이트하십시오.
{: tip}

<p><img src="images/certified_kubernetes_1x7.png" style="padding-right: 10px;" align="left" alt="이 배지는 IBM Cloud 컨테이너 서비스에 대한 Kubernetes 버전 1.7 인증을 표시합니다."/> {{site.data.keyword.containerlong_notm}}는 CNCF Kubernetes Software Conformance Certification 프로그램에서 버전 1.7에 대해 인증된 Kubernetes 제품입니다.</p>

이전 Kubernetes 버전에서 1.7로 업데이트 중일 때 작성해야 할 변경사항을 검토하십시오.

<br/>

#### 마스터 이전 업데이트
{: #17_before}

<table summary="버전 1.7 및 1.6에 대한 Kubernetes 업데이트">
<caption>마스터를 Kubernetes 1.7로 업데이트하기 전에 작성할 변경사항</caption>
<thead>
<tr>
<th>유형</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>스토리지</td>
<td>`../to/dir`과 같은 상위 디렉토리 참조를 포함하는 `hostPath` 및 `mountPath`가 있는 구성 스크립트는 허용되지 않습니다. 경로를 단순 절대 경로(예: `/path/to/dir`)로 변경하십시오.
<ol>
  <li>스토리지 경로를 변경할지 여부를 판별하십시오.</br>
  ```
  kubectl get pods --all-namespaces -o yaml | grep "\.\." && echo "Action required"
  ```
  </br>

  <li>`Action required`가 리턴되면 작업자 노드를 모두 업데이트하기 전에 절대 경로를 참조하도록 팟(Pod)을 변경하십시오. 다른 리소스(예: 배치)에서 팟(Pod)을 소유하는 경우 해당 리소스 내에서 [_PodSpec_ ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/api-reference/v1.7/#podspec-v1-core)을 변경하십시오.
</ol>
</td>
</tr>
</tbody>
</table>

#### 마스터 이후 업데이트
{: #17_after}

<table summary="버전 1.7 및 1.6에 대한 Kubernetes 업데이트">
<caption>마스터를 Kubernetes 1.7로 업데이트한 후 작성할 변경사항</caption>
<thead>
<tr>
<th>유형</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>배치 `apiVersion`</td>
<td>Kubernetes 1.5에서 클러스터를 업데이트한 후 새 `Deployment` YAML 파일에 있는 `apiVersion` 필드에 `apps/v1beta1`을 사용합니다. `Ingress`와 같은 기타 리소스에 `extensions/v1beta1`을 계속 사용합니다.</td>
</tr>
<tr>
<td>'kubectl'</td>
<td>`kubectl` CLI 업데이트 후 이러한 `kubectl create` 명령은 쉼표로 구분된 인수 대신 다중 플래그를 사용해야 합니다.<ul>
 <li>`role`
 <li>`clusterrole`
 <li>`rolebinding`
 <li>`clusterrolebinding`
 <li>`secret`
 </ul>
</br>  예를 들어, `kubectl create role --resource-name <x> , <y>`이 아닌 `kubectl create role --resource-name <x>,<y>`.</td>
</tr>
<tr>
<td>네트워크 정책</td>
<td>`net.beta.kubernetes.io/network-policy` 어노테이션은 더 이상 사용 가능하지 않습니다.
<ol>
  <li>정책을 변경할지 여부를 판별하십시오.</br>
  ```
  kubectl get ns -o yaml | grep "net.beta.kubernetes.io/network-policy" | grep "DefaultDeny" && echo "Action required"
  ```
  <li>`"Action required"`가 리턴되는 경우 나열된 각 Kubernetes 네임스페이스에 다음 네트워크 정책을 추가하십시오.</br>

  <pre class="codeblock">
  <code>
  kubectl create -n &lt;namespace&gt; -f - &lt;&lt;EOF
  kind: NetworkPolicy
  apiVersion: networking.k8s.io/v1
metadata:
    name: default-deny
    namespace: &lt;namespace&gt;
spec:
    podSelector: {}
  EOF
  </code>
  </pre>

  <li> 네트워크 정책을 추가한 후 `net.beta.kubernetes.io/network-policy` 어노테이션을 제거하십시오.
  ```
  kubectl annotate ns <namespace> --overwrite "net.beta.kubernetes.io/network-policy-"
  ```
  </li></ol>
</td></tr>
<tr>
<td>팟(Pod) 친화성 스케줄링</td>
<td> `scheduler.alpha.kubernetes.io/affinity` 어노테이션은 더 이상 사용되지 않습니다.
<ol>
  <li>`ibm-system` 및 `kube-system`을 제외한 각 네임스페이스에서 팟(Pod) 친화성 스케줄링을 업데이트할지 여부를 판별하십시오.</br>
  ```
  kubectl get pods -n <namespace> -o yaml | grep "scheduler.alpha.kubernetes.io/affinity" && echo "Action required"
  ```
  </br></li>
  <li>`"Action required"`가 리턴되는 경우 `scheduler.alpha.kubernetes.io/affinity` 어노테이션 대신 [_PodSpec_ ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/api-reference/v1.7/#podspec-v1-core) _친화성_ 필드를 사용하십시오.</li>
</ol>
</td></tr>
<tr>
<td>`default` `ServiceAccount`에 대한 RBAC</td>
<td><p>`default` 네임스페이스의 `default` `ServiceAccount`에 대한 관리자 `ClusterRoleBinding`이 클러스터 보안 개선을 위해 제거되었습니다. `default` 네임스페이스에서 실행되는 애플리케이션에는 더 이상 Kubernetes API에 대한 클러스터 관리자 권한이 없으며 `RBAC DENY` 권한 오류가 발생할 수 있습니다. 앱 및 해당 `.yaml` 파일을 확인하여 앱이 `default` 네임스페이스에서 실행되는지, `default ServiceAccount`를 사용하는지, Kubernetes API에 액세스하는지 확인하십시오.</p>
<p>애플리케이션이 이러한 권한에 의존하는 경우에는 앱에 대한 [RBAC 권한 리소스를 작성 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview)하십시오.</p>
  <p>앱 RBAC 정책을 업데이트할 때 이전 `default`로 임시로 되돌리려고 할 수 있습니다. `kubectl apply -f FILENAME` 명령을 사용하여 다음 파일을 복사, 저장 및 적용하십시오. <strong>참고</strong>: 장기적인 솔루션이 아닌 모든 애플리케이션 리소스를 업데이트할 시간을 자신에게 부여하도록 되돌리십시오.</p>

<p><pre class="codeblock">
<code>
  kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
   name: admin-binding-nonResourceURLSs-default
  subjects:
  - kind: ServiceAccount
    name: default
    namespace: default
roleRef:
 kind: ClusterRole
 name: admin-role-nonResourceURLSs
 apiGroup: rbac.authorization.k8s.io
---
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
 name: admin-binding-resourceURLSs-default
subjects:
  - kind: ServiceAccount
      name: default
      namespace: default
  roleRef:
   kind: ClusterRole
   name: admin-role-resourceURLSs
   apiGroup: rbac.authorization.k8s.io
</code>
</pre></p>
</td>
</tr>
<tr>
<td>읽기 전용 API 데이터 볼륨</td>
<td>이제 `secret`, `configMap`, `downwardAPI` 및 투영 볼륨은 읽기 전용으로 마운트됩니다.
이전에는 시스템이 자동으로 되돌릴 수 있는 이러한 볼륨에 앱이 데이터를
기록할 수 있었습니다. 이 마이그레이션 조치는 보안 취약성
[CVE-2017-1002102](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2017-1002102)를 수정하는 데 필요합니다.
앱이 이전의 안전하지 않은 작동에 의존하는 경우에는 이를 적절히 수정하십시오.</td>
</tr>
<tr>
<td>StatefulSet 팟(Pod) DNS</td>
<td>마스터가 업데이트된 후 StatefulSet 팟(Pod)이 해당 Kubernetes DNS 항목을 유실합니다. DNS 항목을 복원하려면 StatefulSet 팟(Pod)을 삭제하십시오. Kubernetes는 팟(Pod)을 다시 작성하고 자동으로 DNS 항목을 복원합니다. 자세한 정보는 [Kubernetes 문제 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/issues/48327)를 참조하십시오.</td>
</tr>
<tr>
<td>결함 허용</td>
<td>`scheduler.alpha.kubernetes.io/tolerations` 어노테이션은 더 이상 사용 가능하지 않습니다.
<ol>
  <li>`ibm-system` 및 `kube-system`을 제외한 각 네임스페이스에서 결함 허용을 변경할지 여부를 판별하십시오.</br>
  ```
  kubectl get pods -n <namespace> -o yaml | grep "scheduler.alpha.kubernetes.io/tolerations" && echo "Action required"
  ```
  </br>

  <li>`"Action required"`가 리턴되는 경우 `scheduler.alpha.kubernetes.io/tolerations` 어노테이션 대신 [_PodSpec_ ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/api-reference/v1.7/#podspec-v1-core) _결함 허용_ 필드를 사용하십시오.
</ol>
</td></tr>
<tr>
<td>오염</td>
<td>`scheduler.alpha.kubernetes.io/taints` 어노테이션은 더 이상 사용 가능하지 않습니다.
<ol>
  <li>오염을 변경할지 여부를 판별하십시오.</br>
  ```
  kubectl get nodes -o yaml | grep "scheduler.alpha.kubernetes.io/taints" && echo "Action required"
  ```
  <li>`"Action required"`가 리턴되는 경우 각 노드에 대해 `scheduler.alpha.kubernetes.io/taints` 어노테이션을 제거하십시오.</br>
  `kubectl annotate nodes <node> scheduler.alpha.kubernetes.io/taints-`
  <li>각 노드에 오염을 추가하십시오.</br>
  `kubectl taint node <node> <taint>`
  </li></ol>
</td></tr>
</tbody>
</table>

<br />


### 버전 1.5(지원되지 않음)
{: #cs_v1-5}

2018년 4월 4일부터, [Kubernetes 버전 1.5](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.5.md)를 실행하는 {{site.data.keyword.containershort_notm}} 클러스터는 지원되지 않습니다. 버전 1.5 클러스터는 그 다음 최신 버전([Kubernetes 1.7](#cs_v17))으로 업데이트되지 않는 한 보안 업데이트 또는 지원을 받을 수 없습니다.

각 Kubernetes 버전 업데이트의 [잠재적인 영향을 검토](cs_versions.html#cs_versions)한 후 즉시 [클러스터를 업데이트](cs_cluster_update.html#update)하십시오. 버전 1.5에서 1.7 또는 1.8에서 1.9와 같이 하나의 버전에서 그 다음 최신 버전으로 업데이트해야 합니다.
