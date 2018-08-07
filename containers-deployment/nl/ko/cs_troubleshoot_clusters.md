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



# 클러스터 및 작업자 노드 문제점 해결
{: #cs_troubleshoot_clusters}

{{site.data.keyword.containerlong}}를 사용할 때 클러스터 및 작업자 노드 관련 문제점을 해결하려면 이러한 방법을 고려하십시오.
{: shortdesc}

더 일반적인 문제점이 있는 경우에는 [클러스터 디버깅](cs_troubleshoot.html)을 시도해 보십시오.
{: tip}

## 인프라 계정에 연결할 수 없음
{: #cs_credentials}

{: tsSymptoms}
새 Kubernetes 클러스터를 작성할 때 다음과 같은 메시지를 수신합니다.

```
We were unable to connect to your IBM Cloud infrastructure (SoftLayer) account.
Creating a standard cluster requires that you have either a Pay-As-You-Go account
that is linked to an IBM Cloud infrastructure (SoftLayer) account term or that you have used the {{site.data.keyword.containerlong}} CLI to set your {{site.data.keyword.Bluemix_notm}} Infrastructure API keys.
```
{: screen}

{: tsCauses}
자동 계정 링크가 사용 가능해진 후에 작성된 {{site.data.keyword.Bluemix_notm}} 종량과금제 계정은 IBM Cloud 인프라(SoftLayer) 포트폴리오에 대한 액세스 권한으로 이미 설정되어 있습니다. 추가 구성 없이 클러스터의 인프라 리소스를 구매할 수 있습니다.

기타 {{site.data.keyword.Bluemix_notm}} 계정 유형을 사용하는 사용자 또는 해당 {{site.data.keyword.Bluemix_notm}} 계정에 연결되지 않은 기존 IBM Cloud 인프라(SoftLayer) 계정을 보유한 사용자는 표준 클러스터를 작성하도록 해당 계정을 구성해야 합니다.


{: tsResolve}
IBM Cloud 인프라(SoftLayer) 포트폴리오에 액세스하도록 계정을 구성하는 방법은 보유한 계정의 유형에 따라 다릅니다. 다음 표를 검토하여 각 계정 유형에 사용 가능한 옵션을 찾으십시오.

|계정 유형|설명|표준 클러스터를 작성하기 위해 사용 가능한 옵션|
|------------|-----------|----------------------------------------------|
|라이트 계정|라이트 계정은 클러스터를 프로비저닝할 수 없습니다.|[라이트 계정을 IBM Cloud 인프라(SoftLayer) 포트폴리오에 대한 액세스 권한으로 설정된 {{site.data.keyword.Bluemix_notm}} 종량과금제 계정](/docs/account/index.html#paygo)으로 업그레이드하십시오.|
|이전 종량과금제 계정|자동 계정 링크가 사용 가능하기 전에 작성된 종량과금제 계정에서는 IBM Cloud 인프라(SoftLayer) 포트폴리오에 대한 액세스 권한을 제공하지 않았습니다.<p>기존 IBM Cloud 인프라(SoftLayer) 계정이 있으면 이 계정을 이전 종량과금제 계정에 연결할 수 없습니다.</p>|<strong>옵션 1:</strong> IBM Cloud 인프라(SoftLayer) 포트폴리오에 대한 액세스 권한으로 설정된 [새 종량과금제 계정을 작성](/docs/account/index.html#paygo)합니다. 이 옵션을 선택하는 경우에는 두 개의 별도 {{site.data.keyword.Bluemix_notm}} 계정과 비용 청구가 있습니다.<p>이전 종량과금제 계정을 계속 사용하려면 새 종량과금제 계정을 사용하여 IBM Cloud 인프라(SoftLayer) 포트폴리오에 액세스하기 위한 API 키를 생성할 수 있습니다. 그런 다음 [이전 종량과금제 계정에 대한 IBM Cloud 인프라(SoftLayer) API 키를 설정](cs_cli_reference.html#cs_credentials_set)해야 합니다. </p><p><strong>옵션 2:</strong> 사용할 기존 IBM Cloud 인프라(SoftLayer) 계정이 이미 있는 경우에는 {{site.data.keyword.Bluemix_notm}} 계정의 [신임 정보를 설정](cs_cli_reference.html#cs_credentials_set)할 수 있습니다.</p><p>**참고:** IBM Cloud 인프라(SoftLayer) 계정에 수동으로 연결하는 경우 신임 정보가 {{site.data.keyword.Bluemix_notm}} 계정의 모든 IBM Cloud 인프라(SoftLayer) 특정 조치에 대해 사용됩니다. 사용자가 클러스터를 작성하고 작업할 수 있도록 설정한 API 키에 [충분한 인프라 권한](cs_users.html#infra_access)이 있는지 확인해야 합니다.</p>|
|구독 계정|구독 계정은 IBM Cloud 인프라(SoftLayer) 포트폴리오에 대한 액세스 권한으로 설정되지 않습니다.|<strong>옵션 1:</strong> IBM Cloud 인프라(SoftLayer) 포트폴리오에 대한 액세스 권한으로 설정된 [새 종량과금제 계정을 작성](/docs/account/index.html#paygo)합니다. 이 옵션을 선택하는 경우에는 두 개의 별도 {{site.data.keyword.Bluemix_notm}} 계정과 비용 청구가 있습니다.<p>계속해서 구독 계정을 사용하려는 경우 새 종량과금제 계정을 사용하여 IBM Cloud 인프라(SoftLayer)에 API 키를 생성할 수 있습니다. 그런 다음, 수동으로 [구독 계정에 대한 IBM Cloud 인프라(SoftLayer) API 키를 설정](cs_cli_reference.html#cs_credentials_set)해야 합니다. IBM Cloud 인프라(SoftLayer) 리소스가 새 종량과금제 계정을 통해 비용 청구된다는 점을 유념하십시오.</p><p><strong>옵션 2:</strong> 사용할 기존 IBM Cloud 인프라(SoftLayer) 계정이 이미 있는 경우에는 수동으로 {{site.data.keyword.Bluemix_notm}} 계정의 [IBM Cloud 인프라(SoftLayer) 신임 정보를 설정](cs_cli_reference.html#cs_credentials_set)할 수 있습니다.<p>**참고:** IBM Cloud 인프라(SoftLayer) 계정에 수동으로 연결하는 경우 신임 정보가 {{site.data.keyword.Bluemix_notm}} 계정의 모든 IBM Cloud 인프라(SoftLayer) 특정 조치에 대해 사용됩니다. 사용자가 클러스터를 작성하고 작업할 수 있도록 설정한 API 키에 [충분한 인프라 권한](cs_users.html#infra_access)이 있는지 확인해야 합니다.</p>|
|{{site.data.keyword.Bluemix_notm}} 계정이 아닌 IBM Cloud 인프라(SoftLayer) 계정|표준 클러스터를 작성하려면 {{site.data.keyword.Bluemix_notm}} 계정이 있어야 합니다.|<p>IBM Cloud 인프라(SoftLayer) 포트폴리오에 대한 액세스 권한으로 설정된 [종량과금제 계정을 작성](/docs/account/index.html#paygo)합니다. 이 옵션을 선택하면 IBM Cloud 인프라(SoftLayer) 계정이 작성됩니다. 두 개의 별도 IBM Cloud 인프라(SoftLayer) 계정과 비용 청구가 있습니다.</p>|
{: caption="계정 유형별 표준 클러스터 작성 옵션" caption-side="top"}


<br />


## 방화벽으로 인해 CLI 명령을 실행할 수 없음
{: #ts_firewall_clis}

{: tsSymptoms}
CLI에서 `bx`, `kubectl` 또는 `calicoctl` 명령을 실행하면 실패합니다.

{: tsCauses}
로컬 시스템에서 프록시 또는 방화벽을 통해 공용 엔드포인트에 액세스하지 못하도록 방지하는 회사 네트워크 정책이 있을 수 있습니다.

{: tsResolve}
[CLI 명령에 대한 TCP 액세스가 작동하도록 허용](cs_firewall.html#firewall)하십시오. 이 태스크에는 [관리자 액세스 정책](cs_users.html#access_policies)이 필요합니다. 현재 [액세스 정책](cs_users.html#infra_access)을 확인하십시오.


## 방화벽으로 인해 클러스터를 리소스에 연결할 수 없음
{: #cs_firewall}

{: tsSymptoms}
작업자 노드를 연결할 수 없는 경우 다양한 증상이 나타날 수 있습니다. kubectl proxy가 실패하거나 클러스터의 서비스에 액세스하려고 할 때 다음 메시지 중 하나가 표시되면서 연결에 실패합니다.

  ```
  Connection refused
  ```
  {: screen}

  ```
  Connection timed out
  ```
  {: screen}

  ```
  Unable to connect to the server: net/http: TLS handshake timeout
  ```
  {: screen}

kubectl exec, attach 또는 logs를 실행하는 경우 다음 메시지가 표시될 수 있습니다.

  ```
  Error from server: error dialing backend: dial tcp XXX.XXX.XXX:10250: getsockopt: connection timed out
  ```
  {: screen}

kubectl proxy가 성공하지만 대시보드를 사용할 수 없는 경우 다음 메시지가 표시될 수 있습니다.

  ```
  timeout on 172.xxx.xxx.xxx
  ```
  {: screen}



{: tsCauses}
IBM Cloud 인프라(SoftLayer) 계정에 다른 방화벽 설정이 있거나 기존 방화벽 설정을 사용자 정의했을 수 있습니다. {{site.data.keyword.containershort_notm}}에서는 작업자 노드와 Kubernetes 마스터 간의 양방향 통신을 허용하기 위해 특정 IP 주소와 포트를 열도록 요구합니다. 다른 이유는 작업자 노드가 다시 로드 루프에서 고착 상태에 빠졌기 때문일 수 있습니다.

{: tsResolve}
[클러스터가 인프라 리소스 및 기타 서비스에 액세스하도록 허용](cs_firewall.html#firewall_outbound)하십시오. 이 태스크에는 [관리자 액세스 정책](cs_users.html#access_policies)이 필요합니다. 현재 [액세스 정책](cs_users.html#infra_access)을 확인하십시오.

<br />



## SSH로 작업자 노드에 액세스하는 데 실패
{: #cs_ssh_worker}

{: tsSymptoms}
SSH 연결을 사용하여 작업자 노드에 액세스할 수 없습니다.

{: tsCauses}
비밀번호를 통한 SSH가 작업자 노드에서 사용되지 않습니다.

{: tsResolve}
실행해야 하는 일회성 조치에 대한 모든 노드 또는 작업에 대해 실행해야 하는 전체 항목에 대해 [DaemonSets![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)를 사용하십시오.

<br />


## `kubectl exec` 및 `kubectl logs`가 작동하지 않음
{: #exec_logs_fail}

{: tsSymptoms}
`kubectl exec` 또는 `kubectl logs`를 실행하면 다음 메시지가 표시됩니다.

  ```
  <workerIP>:10250: getsockopt: connection timed out
  ```
  {: screen}

{: tsCauses}
마스터 노드와 작업자 노드 간의 OpenVPN 연결이 제대로 작동하지 않습니다.

{: tsResolve}
1. IBM Cloud 인프라(SoftLayer) 계정에 대한 [VLAN Spanning](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning)을 사용으로 설정하십시오.
2. OpenVPN 클라이언트 팟(Pod)을 다시 시작하십시오.
  ```
  kubectl delete pod -n kube-system -l app=vpn
  ```
  {: pre}
3. 동일한 오류 메시지가 계속 표시되는 경우 VPN 팟(Pod)이 있는 작업자 노드가 비정상일 수 있습니다. VPN 팟(Pod)을 다시 시작하고 다른 작업자 노드로 다시 스케줄하려면 VPN 팟(Pod)이 있는 [작업자 노드를 유출, 드레인 및 다시 부팅](cs_cli_reference.html#cs_worker_reboot)하십시오.

<br />


## 서비스를 클러스터에 바인딩하면 동일한 이름 오류가 발생함
{: #cs_duplicate_services}

{: tsSymptoms}
`bx cs cluster-service-bind <cluster_name> <namespace> <service_instance_name>`을 실행하면 다음 메시지가 표시됩니다.

```
Multiple services with the same name were found.
Run 'bx service list' to view available Bluemix service instances...
```
{: screen}

{: tsCauses}
서로 다른 지역에 동일한 이름의 서비스 인스턴스가 여러 개 있습니다.

{: tsResolve}
`bx cs cluster-service-bind` 명령에서 서비스 인스턴스 이름 대신 서비스 GUID를 사용하십시오.

1. [바인딩할 서비스 인스턴스가 포함된 지역에 로그인하십시오.](cs_regions.html#bluemix_regions)

2. 서비스 인스턴스의 GUID를 가져오십시오.
  ```
  bx service show <service_instance_name> --guid
  ```
  {: pre}

  출력:
  ```
  Invoking 'cf service <service_instance_name> --guid'...
  <service_instance_GUID>
  ```
  {: screen}
3. 서비스를 다시 클러스터에 바인딩하십시오.
  ```
  bx cs cluster-service-bind <cluster_name> <namespace> <service_instance_GUID>
  ```
  {: pre}

<br />


## 서비스를 클러스터에 바인딩하면 서비스를 찾을 수 없음 오류가 발생함
{: #cs_not_found_services}

{: tsSymptoms}
`bx cs cluster-service-bind <cluster_name> <namespace> <service_instance_name>`을 실행하면 다음 메시지가 표시됩니다.

```
Binding service to a namespace...
FAILED

The specified IBM Cloud service could not be found. If you just created the service, wait a little and then try to bind it again. To view available IBM Cloud service instances, run 'bx service list'. (E0023)
```
{: screen}

{: tsCauses}
서비스를 클러스터에 바인드하려면 서비스 인스턴스가 프로비저닝된 영역에 대한 Cloud Foundry 개발자 사용자 역할이 있어야 합니다. 또한 {{site.data.keyword.containerlong}}에 대한 IAM 편집자 액세스 권한이 있어야 합니다. 서비스 인스턴스에 액세스하려면 서비스 인스턴스가 프로비저닝된 영역에 로그인되어 있어야 합니다. 

{: tsResolve}

**사용자로서 다음을 수행하십시오.**

1. {{site.data.keyword.Bluemix_notm}}에 로그인하십시오. 
   ```
           bx login
   ```
   {: pre}
   
2. 서비스 인스턴스가 프로비저닝된 조직 및 영역을 대상으로 지정하십시오. 
   ```
   bx target -o <org> -s <space>
   ```
   {: pre}
   
3. 서비스 인스턴스를 나열하여 올바른 영역에 있는지 확인하십시오. 
   ```
     bx service list 
   ```
   {: pre}
   
4. 서비스를 다시 바인딩하십시오. 일부 오류가 발생하는 경우 계정 관리자에게 문의하여 서비스를 바인딩하기에 충분한 권한이 있는지 확인하십시오(다음 계정 관리자 단계 참조). 

**계정 관리자로서 다음을 수행하십시오.**

1. 이 문제점이 발생한 사용자에게 [{{site.data.keyword.containerlong}}에 대한 편집자 권한](/docs/iam/mngiam.html#editing-existing-access)이 있는지 확인하십시오. 

2. 이 문제점이 발생한 사용자에게 서비스가 프로비저닝되는 [영역에 대한 Cloud Foundry 개발자 역할](/docs/iam/mngcf.html#updating-cloud-foundry-access)이 있는지 확인하십시오. 

3. 올바른 권한이 있는 경우 다른 권한을 지정한 후 필요한 권한을 다시 지정해 보십시오. 

4. 몇 분 동안 기다린 후 사용자가 서비스 바인드를 다시 시도하도록 하십시오. 

5. 그래도 문제점이 해결되지 않으면 IAM 권한이 동기화되지 않고 직접 문제를 해결할 수 없습니다. 지원 티켓을 열어 [IBM 지원에 문의](/docs/get-support/howtogetsupport.html#getting-customer-support)하십시오. 클러스터 ID, 사용자 ID 및 서비스 인스턴스 ID를 제공해야 합니다. 
   1. 클러스터 ID를 검색하십시오. 
      ```
      bx cs clusters
      ```
      {: pre}
      
   2. 서비스 인스턴스 ID를 검색하십시오. 
      ```
      bx service show <service_name> --guid
      ```
      {: pre}


<br />



## 작업자 노드가 업데이트되거나 다시 로드된 후 중복 노드 및 팟(Pod)이 표시됨
{: #cs_duplicate_nodes}

{: tsSymptoms}
`kubectl get nodes`를 실행할 때 **NotReady** 상태의 중복된 작업자 노드가 표시됩니다. **NotReady** 상태의 작업자 노드에는 공인 IP 주소가 있지만 **Ready** 상태의 작업자 노드에는 사설 IP 주소가 있습니다.

{: tsCauses}
이전 클러스터는 클러스터의 공인 IP 주소별로 작업자 노드를 나열했습니다. 이제 작업자 노드가 클러스터의 사설 IP 주소별로 나열됩니다. 노드를 다시 로드하거나 업데이트할 때 IP 주소가 변경되지만 공인 IP 주소에 대한 참조는 그대로 유지됩니다.

{: tsResolve}
이러한 중복 항목으로 인해 서비스가 중단되지는 않지만 API 서버에서 이전 작업자 노드 참조를 제거할 수 있습니다.

  ```
  kubectl delete node <node_name1> <node_name2>
  ```
  {: pre}

<br />


## 작업자 노드가 업데이트되거나 다시 로드된 후 애플리케이션이 RBAC DENY 오류를 수신함
{: #cs_rbac_deny}

{: tsSymptoms}
Kubernetes 버전 1.7로 업데이트한 후 애플리케이션이 `RBAC DENY` 오류를 수신합니다. 

{: tsCauses}
보안 강화를 위해 [Kubernetes 버전 1.7](cs_versions.html#cs_v17)부터는 `default` 네임스페이스에서 실행되는 애플리케이션에 더 이상 Kubernetes API에 대한 클러스터 관리자 권한이 부여되지 않습니다.

앱이 `default` 네임스페이스에서 실행되며, `default ServiceAccount`를 사용하고, Kubernetes API에 액세스하는 경우에는 이 Kubernetes 변경사항의 영향을 받습니다. 자세한 정보는 [Kubernetes 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/admin/authorization/rbac/#upgrading-from-15)를 참조하십시오.

{: tsResolve}
시작하기 전에 클러스터를 [CLI의 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오.

1.  **임치 조치**: 앱 RBAC 정책을 업데이트하면서 `default` 네임스페이스의 `default ServiceAccount`를 이전 `ClusterRoleBinding`으로 되돌릴 수 있습니다.

    1.  다음 `.yaml` 파일을 복사하십시오.

        ```yaml
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
        ```

    2.  이 `.yaml` 파일을 클러스터에 적용하십시오.

        ```
        kubectl apply -f FILENAME
        ```
        {: pre}

2.  [RBAC 권한 리소스를 작성 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview)하여 `ClusterRoleBinding` 관리자 액세스 권한을 업데이트하십시오.

3.  임시 클러스터 역할 바인딩을 작성한 경우에는 이를 제거하십시오.

<br />


## 제한시간 초과로 인해 새 작업자 노드에서 팟(Pod)에 액세스하는 데 실패
{: #cs_nodes_duplicate_ip}

{: tsSymptoms}
클러스터에서 작업자 노드를 삭제한 다음 작업자 노드를 추가했습니다. 팟(Pod) 또는 Kubernetes 서비스를 배치한 경우 리소스에서 새로 작성한 작업자 노드에 액세스할 수 없으며 연결 제한시간이 초과됩니다.

{: tsCauses}
클러스터에서 작업자 노드를 삭제한 다음 작업자 노드를 추가하는 경우 삭제된 작업자 노드의 사설 IP 주소에 새 작업자 노드를 지정할 수 있습니다. Calico에서는 이 사설 IP 주소를 태그로 사용하고 계속하여 삭제된 노드에 연결하려고 합니다.

{: tsResolve}
정확한 노드를 가리키도록 사설 IP 주소의 참조를 수동으로 업데이트하십시오.

1.  **사설 IP** 주소가 동일한 작업자 노드가 두 개 있는지 확인하십시오. 삭제된 작업자의 **사설 IP** 및 **ID**를 참고하십시오.

  ```
  bx cs workers <CLUSTER_NAME>
  ```
  {: pre}

  ```
  ID                                                 Public IP       Private IP       Machine Type   State     Status   Location   Version
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w1   169.xx.xxx.xxx  10.xxx.xx.xxx    b2c.4x16       normal    Ready    dal10      1.9.7
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w2   169.xx.xxx.xxx  10.xxx.xx.xxx    b2c.4x16       deleted    -       dal10      1.9.7
  ```
  {: screen}

2.  [Calico CLI](cs_network_policy.html#adding_network_policies)를 설치하십시오.
3.  Calico에서 사용 가능한 작업자 노드를 나열하십시오. <path_to_file>을 Calico 구성 파일의 로컬 경로로 대체하십시오.

  ```
  calicoctl get nodes --config=filepath/calicoctl.cfg
  ```
  {: pre}

  ```
  NAME
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w1
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w2
  ```
  {: screen}

4.  Calico에서 중복 작업자 노드를 삭제하십시오. NODE_ID를 작업자 노드 ID로 대체하십시오.

  ```
  calicoctl delete node NODE_ID --config=<path_to_file>/calicoctl.cfg
  ```
  {: pre}

5.  삭제되지 않은 작업자 노드를 다시 부팅하십시오.

  ```
  bx cs worker-reboot CLUSTER_ID NODE_ID
  ```
  {: pre}


삭제된 노드는 더 이상 Calico에 나열되지 않습니다.

<br />


## 클러스터가 보류 상태를 유지함
{: #cs_cluster_pending}

{: tsSymptoms}
클러스터를 배치하는 경우 보류 상태를 유지하고 시작하지 않습니다.

{: tsCauses}
클러스터를 방금 작성한 경우, 작업자 노드가 여전히 구성 중일 수 있습니다. 이미 일정한 기간 동안 대기한 경우 올바르지 않은 VLAN이 있을 수 있습니다.

{: tsResolve}

다음 솔루션 중 하나를 수행할 수 있습니다.
  - `bx cs clusters`를 실행하여 클러스터의 상태를 확인하십시오. 그런 다음 `bx cs workers <cluster_name>`을 실행하여 작업자 노드가 배치되었는지 확인하십시오.
  - VLAN이 올바른지 확인하십시오. 올바른 상태가 되려면 VLAN은 로컬 디스크 스토리지로 작업자를 호스팅할 수 있는 인프라와 연관되어야 합니다. 목록에 VLAN이 표시되지 않으며 유효하지 않은 경우 `bx cs vlans <location>`을 실행하여 [VLAN을 나열](/docs/containers/cs_cli_reference.html#cs_vlans)할 수 있습니다. 다른 VALN을 선택하십시오.

<br />


## 팟(Pod)이 보류 상태를 유지함
{: #cs_pods_pending}

{: tsSymptoms}
`kubectl get pods`를 실행할 때 **보류** 상태를 유지하는 팟(Pod)을 볼 수 있습니다.

{: tsCauses}
Kubernetes 클러스터를 방금 작성한 경우, 작업자 노드가 여전히 구성 중일 수 있습니다. 이 클러스터가 기존 클러스터인 경우, 클러스터에 팟(Pod)을 배치할 만큼 충분한 용량이 없을 수 있습니다.

{: tsResolve}
이 태스크에는 [관리자 액세스 정책](cs_users.html#access_policies)이 필요합니다. 현재 [액세스 정책](cs_users.html#infra_access)을 확인하십시오.

방금 Kubernetes 클러스터를 작성한 경우에는 다음 명령을 실행하고 작업자 노드가 초기화될 때까지 대기하십시오.

```
kubectl get nodes
```
{: pre}

이 클러스터가 기존 클러스터인 경우, 클러스터 용량을 확인하십시오.

1.  기본 포트 번호로 프록시를 설정하십시오.

  ```
   kubectl proxy
  ```
   {: pre}

2.  Kubernetes 대시보드를 여십시오.

  ```
   http://localhost:8001/ui
  ```
  {: pre}

3.  팟(Pod)을 배치하기에 충분한 용량이 클러스터에 있는지 확인하십시오.

4.  클러스터에 충분한 용량이 없으면 클러스터에 다른 작업자 노드를 추가하십시오.

    ```
    bx cs worker-add <cluster_name_or_ID> 1
    ```
    {: pre}

5.  작업자 노드를 완전히 배치한 후에도 팟(Pod)이 계속 **보류** 상태를 유지하면 [Kubernetes 문서![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-pod-replication-controller/#my-pod-stays-pending)를 검토하여 보류 상태인 팟(Pod)의 추가 문제점을 해결할 수 있습니다.

<br />




## 컨테이너가 시작되지 않음
{: #containers_do_not_start}

{: tsSymptoms}
팟(Pod)이 클러스터에 배치되지만 컨테이너가 시작되지 않습니다.

{: tsCauses}
레지스트리 할당량에 도달하면 컨테이너가 시작되지 않을 수 있습니다.

{: tsResolve}
[{{site.data.keyword.registryshort_notm}}의 스토리지를 비우십시오.](../services/Registry/registry_quota.html#registry_quota_freeup)

<br />



## 업데이트된 구성 값을 사용하여 Helm 차트를 설치할 수 없음
{: #cs_helm_install}

{: tsSymptoms}
`helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>`을 실행하여 업데이트된 Helm 차트를 설치하려는 경우 `Error: failed to download "ibm/<chart_name>"` 오류 메시지가 표시됩니다.

{: tsCauses}
Helm 인스턴스의 {{site.data.keyword.Bluemix_notm}} 저장소에 대한 URL이 올바르지 않을 수 있습니다.

{: tsResolve}
Helm 차트의 문제점을 해결하려면 다음을 수행하십시오.

1. Helm 인스턴스에서 현재 사용 가능한 저장소를 나열하십시오.

    ```
    helm repo list
    ```
    {: pre}

2. 출력에서 {{site.data.keyword.Bluemix_notm}} 저장소인 `ibm`에 대한 URL이 `https://registry.bluemix.net/helm/ibm`인지 확인하십시오.

    ```
    NAME    URL
    stable  https://kubernetes-charts.storage.googleapis.com
    local   http://127.0.0.1:8888/charts
    ibm     https://registry.bluemix.net/helm/ibm
    ```
    {: screen}

    * URL이 올바르지 않은 경우:

        1. {{site.data.keyword.Bluemix_notm}} 저장소를 제거하십시오.

            ```
            helm repo remove ibm
            ```
            {: pre}

        2. {{site.data.keyword.Bluemix_notm}} 저장소를 다시 추가하십시오.

            ```
            helm repo add ibm  https://registry.bluemix.net/helm/ibm
            ```
            {: pre}

    * URL이 올바르지 않은 경우 저장소에서 최신 업데이트를 가져오십시오.

        ```
        helm repo update
        ```
        {: pre}

3. 업데이트를 사용하여 Helm 차트를 설치하십시오.

    ```
    helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>
    ```
    {: pre}

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

