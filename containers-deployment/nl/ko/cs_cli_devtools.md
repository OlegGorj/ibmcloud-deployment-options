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





# 클러스터 관리를 위한 CLI 참조
{: #cs_cli_reference}

{{site.data.keyword.Bluemix_notm}}에서 클러스터를 작성하고 관리하려면 다음 명령을 참조하십시오.
{:shortdesc}

## bx cs 명령
{: #cs_commands}

**팁:** {{site.data.keyword.containershort_notm}} 플러그인의 버전을 보려면 다음 명령을 실행하십시오.

```
bx plugin list
```
{: pre}



<table summary="API 명령">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>API 명령</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs api-key-info](#cs_api_key_info)</td>
    <td>[bx cs api-key-reset](#cs_api_key_reset)</td>
    <td>[bx cs apiserver-config-get](#cs_apiserver_config_get)</td>
    <td>[bx cs apiserver-config-set](#cs_apiserver_config_set)</td>
  </tr>
  <tr>
    <td>[bx cs apiserver-config-unset](#cs_apiserver_config_unset)</td>
    <td>[bx cs apiserver-refresh](#cs_apiserver_refresh)</td>
    <td></td>
    <td></td>
 </tr>
</tbody>
</table>

<br>

<table summary="CLI 플러그인 사용법 명령">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>CLI 플러그인 사용법 명령</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs help](#cs_help)</td>
    <td>[bx cs init](#cs_init)</td>
    <td>[bx cs messages](#cs_messages)</td>
    <td></td>
  </tr>
</tbody>
</table>

<br>

<table summary="클러스터 명령: 관리">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>클러스터 명령: 관리</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs cluster-config](#cs_cluster_config)</td>
    <td>[bx cs cluster-create](#cs_cluster_create)</td>
    <td>[bx cs cluster-feature-enable](#cs_cluster_feature_enable)</td>
    <td>[bx cs cluster-get](#cs_cluster_get)</td>
  </tr>
  <tr>
    <td>[bx cs cluster-rm](#cs_cluster_rm)</td>
    <td>[bx cs cluster-update](#cs_cluster_update)</td>
    <td>[     bx cs clusters
    ](#cs_clusters)</td>
    <td>[bx cs kube-versions](#cs_kube_versions)</td>
  </tr>
</tbody>
</table>

<br>

<table summary="클러스터 명령: 서비스 및 통합">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>클러스터 명령: 서비스 및 통합</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs cluster-service-bind](#cs_cluster_service_bind)</td>
    <td>[bx cs cluster-service-unbind](#cs_cluster_service_unbind)</td>
    <td>[bx cs cluster-services](#cs_cluster_services)</td>
    <td>[bx cs webhook-create](#cs_webhook_create)</td>
  </tr>
</tbody>
</table>

</br>

<table summary="클러스터 명령: 서브넷">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>클러스터 명령: 서브넷</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs cluster-subnet-add](#cs_cluster_subnet_add)</td>
    <td>[bx cs cluster-subnet-create](#cs_cluster_subnet_create)</td>
    <td>[bx cs cluster-user-subnet-add](#cs_cluster_user_subnet_add)</td>
    <td>[bx cs cluster-user-subnet-rm](#cs_cluster_user_subnet_rm)</td>
  </tr>
  <tr>
    <td>[     bx cs subnets
    ](#cs_subnets)</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</tbody>
</table>

</br>

<table summary="인프라 명령">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>인프라 명령</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs credentials-set](#cs_credentials_set)</td>
    <td>[bx cs credentials-unset](#cs_credentials_unset)</td>
    <td>[bx cs machine-types](#cs_machine_types)</td>
    <td>[bx cs vlans](#cs_vlans)</td>
  </tr>
</tbody>
</table>

</br>

<table summary="Ingress 애플리케이션 로드 밸런서(ALB) 명령">
<col width = 25%>
<col width = 25%>
<col width = 25%>
  <thead>
    <tr>
      <th colspan=4>Ingress 애플리케이션 로드 밸런서(ALB) 명령</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>[bx cs alb-cert-deploy](#cs_alb_cert_deploy)</td>
      <td>[bx cs alb-cert-get](#cs_alb_cert_get)</td>
      <td>[bx cs alb-cert-rm](#cs_alb_cert_rm)</td>
      <td>[bx cs alb-certs](#cs_alb_certs)</td>
    </tr>
    <tr>
      <td>[bx cs alb-configure](#cs_alb_configure)</td>
      <td>[bx cs alb-get](#cs_alb_get)</td>
      <td>[bx cs alb-types](#cs_alb_types)</td>
      <td>[bx cs albs](#cs_albs)</td>
    </tr>
  </tbody>
</table>

</br>

<table summary="로깅 명령">
<col width = 25%>
<col width = 25%>
<col width = 25%>
  <thead>
    <tr>
      <th colspan=4>로깅 명령</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>[bx cs logging-config-create](#cs_logging_create)</td>
      <td>[bx cs logging-config-get](#cs_logging_get)</td>
      <td>[bx cs logging-config-refresh](#cs_logging_refresh)</td>
      <td>[bx cs logging-config-rm](#cs_logging_rm)</td>
    </tr>
    <tr>
      <td>[bx cs logging-config-update](#cs_logging_update)</td>
      <td>[bx cs logging-filter-create](#cs_log_filter_create)</td>
      <td>[bx cs logging-filter-update](#cs_log_filter_update)</td>
      <td>[bx cs logging-filter-get](#cs_log_filter_view)</td>
    </tr>
    <tr>
      <td>[bx cs logging-filter-rm](#cs_log_filter_delete)</td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>

</br>

<table summary="지역 명령">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>지역 명령</th>
 </thead>
 <tbody>
  <tr>
    <td>[        bx cs locations
        ](#cs_datacenters)</td>
    <td>[bx cs region](#cs_region)</td>
    <td>[bx cs region-set](#cs_region-set)</td>
    <td>[bx cs regions](#cs_regions)</td>
  </tr>
</tbody>
</table>

</br>

<table summary="작업자 노드 명령">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>작업자 노드 명령</th>
 </thead>
 <tbody>
    <tr>
      <td>[bx cs worker-add](#cs_worker_add)</td>
      <td>[bx cs worker-get](#cs_worker_get)</td>
      <td>[bx cs worker-reboot](#cs_worker_reboot)</td>
      <td>[bx cs worker-reload](#cs_worker_reload)</td></staging>
    </tr>
    <tr>
      <td>[bx cs worker-rm](#cs_worker_rm)</td>
      <td>[bx cs worker-update](#cs_worker_update)</td>
      <td>[bx cs workers](#cs_workers)</td>
      <td></td>
    </tr>
  </tbody>
</table>

## API 명령
{: #api_commands}

### bx cs api-key-info CLUSTER
{: #cs_api_key_info}

{{site.data.keyword.containershort_notm}} 지역에서 IAM API 키 소유자의 이름과 이메일 주소를 봅니다.

IAM(Identity and Access Management) API 키는 {{site.data.keyword.containershort_notm}} 관리자 액세스 권한이 필요한 첫 번째 조치가 수행될 때 지역에 대해 자동으로 설정됩니다. 예를 들어, 관리 사용자 중 한 명이 `us-south` 지역에서 첫 번째 클러스터를 작성합니다. 이를 수행하면 이 사용자의 IAM API 키가 이 지역의 계정에 저장됩니다. API 키는 새 작업자 노드 또는 VLAN과 같은 IBM Cloud 인프라(SoftLayer)에서 리소스를 정렬하는 데 사용됩니다.

다른 사용자가 새 클러스터 작성 또는 작업자 노드 다시 로드와 같이 IBM Cloud 인프라(SoftLayer) 포트폴리오와의 상호작용이 필요한 이 지역의 조치를 수행하는 경우 저장된 API 키는 해당 조치를 수행하는 데 충분한 권한이 있는지 판별하는 데 사용됩니다. 클러스터의 인프라 관련 조치를 수행할 수 있는지 확인하려면 {{site.data.keyword.containershort_notm}} 관리 사용자에게 **수퍼유저** 인프라 액세스 정책을 지정하십시오. 자세한 정보는 [사용자 액세스 관리](cs_users.html#infra_access)를 참조하십시오.

지역에 대해 저장된 API 키를 업데이트해야 하는 경우 [bx cs api-key-reset](#cs_api_key_reset) 명령을 사용하여 이를 수행할 수 있습니다. 이 명령은 {{site.data.keyword.containershort_notm}} 관리자 액세스 정책이 필요하고 계정에서 이 명령을 실행하는 사용자의 API 키를 저장합니다.

**팁:** IBM Cloud 인프라(SoftLayer) 신임 정보가 [bx cs credentials-set](#cs_credentials_set) 명령을 사용하여 수동으로 설정된 경우 이 명령에서 리턴되는 API 키가 사용되지 않을 수 있습니다.

<strong>명령 옵션</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>
   </dl>

**예제**:

  ```
  bx cs api-key-info my_cluster
  ```
  {: pre}


### bx cs api-key-reset
{: #cs_api_key_reset}

{{site.data.keyword.containershort_notm}} 지역에서 현재 IAM API 키를 대체합니다.

이 명령은 {{site.data.keyword.containershort_notm}} 관리자 액세스 정책이 필요하고 계정에서 이 명령을 실행하는 사용자의 API 키를 저장합니다. IBM Cloud 인프라(SoftLayer) 포트폴리오에서 인프라를 정렬하는 데 IAM API 키가 필요합니다. 저장되면, API 키는 이 명령을 실행하는 사용자와 무관하게 인프라 권한이 필요한 지역의 모든 조치에 사용됩니다. IAM API 키 작동 방법에 대한 자세한 정보는 [`bx cs api-key-info` 명령](#cs_api_key_info)을 참조하십시오.

**중요** 이 명령을 시작하기 전에 이 명령을 실행하는 사용자가 필수 [{{site.data.keyword.containershort_notm}} 및 IBM Cloud 인프라(SoftLayer) 권한](cs_users.html#users)을 보유하고 있는지 확인하십시오.

**예제**:

  ```
  bx cs api-key-reset
  ```
  {: pre}


### bx cs apiserver-config-get
{: #cs_apiserver_config_get}

클러스터의 Kubernetes API 서버 구성 옵션에 대한 정보를 가져옵니다. 이 명령은 정보를 원하는 구성 옵션에 대한 다음 하위 명령 중 하나와 결합되어야 합니다.

#### bx cs apiserver-config-get audit-webhook CLUSTER
{: #cs_apiserver_api_webhook_get}

API 서버 감사 로그를 전송 중인 원격 로깅 서비스의 URL을 봅니다. URL은 API 서버 구성을 위한 웹훅 백엔드를 작성할 때 지정되었습니다.

<strong>명령 옵션</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>
   </dl>

**예제**:

  ```
  bx cs apiserver-config-get audit-webhook my_cluster
  ```
  {: pre}

### bx cs apiserver-config-set
{: #cs_apiserver_config_set}

클러스터의 Kubernetes API 서버 구성에 대한 한 옵션을 설정합니다. 이 명령은 설정할 구성 옵션에 대한 다음 하위 명령 중 하나와 결합되어야 합니다.

#### bx cs apiserver-config-set audit-webhook CLUSTER [--remoteServer SERVER_URL_OR_IP][--caCert CA_CERT_PATH] [--clientCert CLIENT_CERT_PATH][--clientKey CLIENT_KEY_PATH]
{: #cs_apiserver_api_webhook_set}

API 서버 구성을 위한 웹훅 백엔드를 설정합니다. 웹훅 백엔드는 API 서버 감사 로그를 원격 서버로 전달합니다. 웹훅 구성은 사용자가 이 명령의 플래그에 제공하는 정보를 기반으로 작성됩니다. 이 플래그에 정보를 제공하지 않은 경우에는 기본 웹훅 구성이 사용됩니다.

<strong>명령 옵션</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>--remoteServer <em>SERVER_URL</em></code></dt>
   <dd>감사 로그를 전송할 원격 로깅 서비스의 URL 또는 IP 주소입니다. 비보안 서버 URL을 제공하는 경우에는 모든 인증서가 무시됩니다. 이 값은 선택사항입니다.</dd>

   <dt><code>--caCert <em>CA_CERT_PATH</em></code></dt>
   <dd>원격 로깅 서비스를 확인하는 데 사용되는 CA 인증서의 파일 경로입니다. 이 값은 선택사항입니다.</dd>

   <dt><code>--clientCert <em>CLIENT_CERT_PATH</em></code></dt>
   <dd>원격 로깅 서비스에 대해 인증하는 데 사용되는 클라이언트 인증서의 파일 경로입니다. 이 값은 선택사항입니다.</dd>

   <dt><code>--clientKey <em> CLIENT_KEY_PATH</em></code></dt>
   <dd>원격 로깅 서비스에 연결하는 데 사용되는 해당 클라이언트 키의 파일 경로입니다. 이 값은 선택사항입니다.</dd>
   </dl>

**예제**:

  ```
  bx cs apiserver-config-set audit-webhook my_cluster --remoteServer https://audit.example.com/audit --caCert /mnt/etc/kubernetes/apiserver-audit/ca.pem --clientCert /mnt/etc/kubernetes/apiserver-audit/cert.pem --clientKey /mnt/etc/kubernetes/apiserver-audit/key.pem
  ```
  {: pre}


### bx cs apiserver-config-unset
{: #cs_apiserver_config_unset}

클러스터의 Kubernetes API 서버 구성에 대한 옵션을 사용 안함으로 설정합니다. 이 명령은 설정 해제할 구성 옵션에 대한 다음 하위 명령 중 하나와 결합되어야 합니다.

#### bx cs apiserver-config-unset audit-webhook CLUSTER
{: #cs_apiserver_api_webhook_unset}

클러스터의 API 서버를 위한 웹훅 백엔드 구성을 사용 안함으로 설정합니다. 웹훅 백엔드를 사용 안함으로 설정하면 원격 서버로의 API 서버 감사 로그 전달이 중지됩니다.

<strong>명령 옵션</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>
   </dl>

**예제**:

  ```
  bx cs apiserver-config-unset audit-webhook my_cluster
  ```
  {: pre}

### bx cs apiserver-refresh CLUSTER
{: #cs_apiserver_refresh}

클러스터에서 Kubernetes 마스터를 다시 시작하여 변경사항을 API 서버 구성에 적용합니다.

<strong>명령 옵션</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>
   </dl>

**예제**:

  ```
  bx cs apiserver-refresh my_cluster
  ```
  {: pre}


<br />


## CLI 플러그인 사용법 명령
{: #cli_plug-in_commands}

###   bx cs help
{: #cs_help}

지원되는 명령 및 매개변수의 목록을 봅니다.

<strong>명령 옵션</strong>:

   없음

**예제**:

  ```
  bx cs help
  ```
  {: pre}


### bx cs init [--host HOST]
{: #cs_init}

{{site.data.keyword.containershort_notm}} 플러그인을 초기화하거나 Kubernetes 클러스터를 작성 또는 액세스할 지역을 지정합니다.

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--host <em>HOST</em></code></dt>
   <dd>사용할 {{site.data.keyword.containershort_notm}} API 엔드포인트입니다.  이 값은 선택사항입니다. [사용 가능한 API 엔드포인트 값을 보십시오.](cs_regions.html#container_regions)</dd>
   </dl>

**예제**:


```
        bx cs init --host https://uk-south.containers.bluemix.net
```
{: pre}


### bx cs messages
{: #cs_messages}

IBM ID 사용자에 대한 현재 메시지를 봅니다.

**예제**:

```
bx cs messages
```
{: pre}


<br />


## 클러스터 명령: 관리
{: #cluster_mgmt_commands}


### bx cs cluster-config CLUSTER [--admin][--export]
{: #cs_cluster_config}

로그인한 후 클러스터에 연결하는 데 필요한 Kubernetes 구성 데이터 및 인증서를 다운로드하고 `kubectl` 명령을 실행합니다. 파일은 `user_home_directory/.bluemix/plugins/container-service/clusters/<cluster_name>`에 다운로드됩니다.

**명령 옵션**:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>--admin</code></dt>
   <dd>수퍼유저 역할을 위한 TLS 인증서와 권한 파일을 다운로드합니다. 재인증할 필요 없이 인증서를 사용하여 클러스터의 태스크를 자동화할 수 있습니다. 파일은 `<user_home_directory>/.bluemix/plugins/container-service/clusters/<cluster_name>-admin`에 다운로드됩니다. 이 값은 선택사항입니다.</dd>

   <dt><code>--export</code></dt>
   <dd>내보내기 명령 이외의 다른 메시지 없이 Kubernetes 구성 데이터와 인증서를 다운로드합니다. 메시지가 표시되지 않으므로 자동화된 스크립트를 작성할 때 이 플래그를 사용할 수 있습니다. 이 값은 선택사항입니다.</dd>
   </dl>

**예제**:

```
bx cs cluster-config my_cluster
```
{: pre}


### bx cs cluster-create [--file FILE_LOCATION][--hardware HARDWARE] --location LOCATION --machine-type MACHINE_TYPE --name NAME [--kube-version MAJOR.MINOR.PATCH][--no-subnet] [--private-vlan PRIVATE_VLAN][--public-vlan PUBLIC_VLAN] [--workers WORKER][--disable-disk-encrypt] [--trusted]
{: #cs_cluster_create}

조직에 클러스터를 작성합니다. 무료 클러스터의 경우 클러스터 이름을 지정합니다. 그 외에는 모두 기본값으로 설정됩니다. 무료 클러스터는 21일 후 자동으로 삭제됩니다. 한 번에 하나의 무료 클러스터가 제공됩니다. Kubernetes의 전체 기능을 활용하려면 표준 클러스터를 작성하십시오.

<strong>명령 옵션</strong>

<dl>
<dt><code>--file <em>FILE_LOCATION</em></code></dt>

<dd>표준 클러스터를 작성하기 위한 YAML 파일의 경로입니다. 이 명령에 제공된 옵션을 사용하여 클러스터의 특징을 정의하지 않고 YAML 파일을 사용할 수 있습니다.  이 값은 표준 클러스터의 경우 선택사항이며 무료 클러스터에는 사용할 수 없습니다.

<p><strong>참고:</strong> 명령에서 YAML 파일의 매개변수와 동일한 옵션을 제공하면 명령의 값이 YAML의 값보다 우선합니다. 예를 들어, YAML 파일의 위치를 정의하고 명령에서 <code>--location</code> 옵션을 사용하십시오. 그러면 명령 옵션에 입력한 값이 YAML 파일의 값을 대체합니다.

<pre class="codeblock">
<code>name: <em>&lt;cluster_name&gt;</em>
location: <em>&lt;location&gt;</em>
no-subnet: <em>&lt;no-subnet&gt;</em>
machine-type: <em>&lt;machine_type&gt;</em>
private-vlan: <em>&lt;private_VLAN&gt;</em>
public-vlan: <em>&lt;public_VLAN&gt;</em>
hardware: <em>&lt;shared_or_dedicated&gt;</em>
workerNum: <em>&lt;number_workers&gt;</em>
kube-version: <em>&lt;kube-version&gt;</em>
diskEncryption: <em>false</em>
trusted: <em>true</em>
</code></pre>


<table>
    <caption>표. YAML 파일 컴포넌트 이해</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>name</em></code></td>
    <td><code><em>&lt;cluster_name&gt;</em></code>을 클러스터의 이름으로 대체합니다. 이름은 문자로 시작해야 하며 35자 이하의 문자, 숫자 및 하이픈(-)을 포함할 수 있습니다. 클러스터 이름과 클러스터가 배치된 지역이 Ingress 하위 도메인의 완전한 이름을 형성합니다. 특정 Ingress 하위 도메인이 지역 내에서 고유하도록 하기 위해 클러스터 이름을 자르고 Ingress 도메인 이름 내의 무작위 값을 추가할 수 있습니다.
</td>
    </tr>
    <tr>
    <td><code><em>location</em></code></td>
    <td><code><em>&lt;location&gt;</em></code>을 클러스터를 작성할 위치로 대체합니다. 사용 가능한 위치는 사용자가 로그인한 지역에 따라 다릅니다. 사용 가능한 위치를 나열하려면 <code>bx cs locations</code>를 실행하십시오. </td>
     </tr>
     <tr>
     <td><code><em>no-subnet</em></code></td>
     <td>기본적으로 공용 및 사설 포터블 서브넷이 클러스터와 연관된 VLAN에서 작성됩니다. 클러스터에서 서브넷을 작성하지 않도록 하려면 <code><em>&lt;no-subnet&gt;</em></code>을 <code><em>true</em></code>로 대체하십시오. 나중에 서브넷을 [작성](#cs_cluster_subnet_create)하거나 클러스터에 [추가](#cs_cluster_subnet_add)할 수 있습니다.</td>
      </tr>
     <tr>
     <td><code><em>machine-type</em></code></td>
     <td><code><em>&lt;machine_type&gt;</em></code>을 작업자 노드를 배치하려는 머신 유형으로 대체합니다. 공유 또는 전용 하드웨어에서 가상 머신으로서 또는 베어메탈에서 실제 머신으로서 작업자 노드를 배치할 수 있습니다. 사용 가능한 실제 및 가상 머신 유형은 클러스터를 배치하는 위치에 따라 다릅니다. 자세한 정보는 `bx cs machine-type` [명령](cs_cli_reference.html#cs_machine_types)에 대한 문서를 참조하십시오.</td>
     </tr>
     <tr>
     <td><code><em>private-vlan</em></code></td>
     <td><code><em>&lt;private_VLAN&gt;</em></code>을 작업자 노드에 사용할 사설 VLAN의 ID로 대체합니다. 사용 가능한 VLAN을 나열하려면 <code>bx cs vlans <em>&lt;location&gt;</em></code>을 실행하고 <code>bcr</code>(백엔드 라우터)로 시작하는 VLAN 라우터를 찾으십시오.</td>
     </tr>
     <tr>
     <td><code><em>public-vlan</em></code></td>
     <td><code><em>&lt;public_VLAN&gt;</em></code>을 작업자 노드에 사용할 공인 VLAN의 ID로 대체합니다. 사용 가능한 VLAN을 나열하려면 <code>bx cs vlans <em>&lt;location&gt;</em></code>을 실행하고 <code>fcr</code>(프론트 엔드 라우터)로 시작하는 VLAN 라우터를 찾으십시오.</td>
     </tr>
     <tr>
     <td><code><em>hardware</em></code></td>
     <td>가상 머신 유형의 경우: 작업자 노드에 대한 하드웨어 격리의 레벨입니다. 사용자 전용으로만 실제 리소스를 사용 가능하게 하려면 dedicated를 사용하고, 실제 리소스를 다른 IBM 고객과 공유하도록 허용하려면 shared를 사용하십시오. 기본값은 <code>shared</code>입니다.</td>
     </tr>
     <tr>
     <td><code><em>workerNum</em></code></td>
     <td><code><em>&lt;number_workers&gt;</em></code>를 배치할 작업자 노드 수로 대체합니다.</td>
     </tr>
     <tr>
      <td><code><em>kube-version</em></code></td>
      <td>클러스터 마스터 노드를 위한 Kubernetes 버전입니다. 이 값은 선택사항입니다. 버전이 지정되지 않은 경우에는 클러스터가 지원되는 Kubernetes 버전의 기본값으로 작성됩니다. 사용 가능한 버전을 보려면 <code>bx cs kube-versions</code>를 실행하십시오.
</td></tr>
      <tr>
      <td><code>diskEncryption: <em>false</em></code></td>
      <td>작업자 노드는 기본적으로 디스크 암호화 기능을 합니다. [자세히 보기](cs_secure.html#worker). 암호화를 사용 안함으로 설정하려면 이 옵션을 포함하고 값을 <code>false</code>로 설정하십시오.</td></tr>
      <tr>
      <td><code>trusted: <em>true</em></code></td>
      <td>**베어메탈 전용**: [신뢰할 수 있는 컴퓨팅](cs_secure.html#trusted_compute)을 사용하여 베어메탈 작업자 노드를 변조와 비교하여 확인합니다. 클러스터 작성 중에 신뢰를 사용하도록 설정하지 않았으나 나중에 사용하도록 설정하기를 원하는 경우 `bx cs feature-enable` [명령](cs_cli_reference.html#cs_cluster_feature_enable)을 사용할 수 있습니다. 신뢰를 사용하도록 설정한 후에는 나중에 사용하지 않도록 설정할 수 없습니다.</td></tr>
     </tbody></table>
    </p></dd>

<dt><code>--hardware <em>HARDWARE</em></code></dt>
<dd>작업자 노드에 대한 하드웨어 격리의 레벨입니다. 사용자 전용으로만 실제 리소스를 사용 가능하게 하려면 dedicated를 사용하고, 실제 리소스를 다른 IBM 고객과 공유하도록 허용하려면 shared를 사용하십시오. 기본값은 shared입니다.  이 값은 표준 클러스터의 경우 선택사항이며 무료 클러스터에는 사용할 수 없습니다.</dd>

<dt><code>--location <em>LOCATION</em></code></dt>
<dd>클러스터를 작성하려는 위치입니다. 사용 가능한 위치는 사용자가 로그인한 {{site.data.keyword.Bluemix_notm}} 지역에 따라 다릅니다. 
최고의 성능을 위해서는 실제로 사용자와 가장 가까운 지역을 선택하십시오.  이 값은 표준 클러스터의 경우 필수이며 무료 클러스터의 경우 선택사항입니다.

<p>[사용 가능한 위치](cs_regions.html#locations)를 검토하십시오.
</p>

<p><strong>참고:</strong> 자국 외에 있는 위치를 선택하는 경우에는 외국에서 데이터를 물리적으로 저장하기 전에 법적 인가를 받아야 할 수 있음을 유념하십시오.</p>
</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>머신 유형을 선택합니다. 공유 또는 전용 하드웨어에서 가상 머신으로서 또는 베어메탈에서 실제 머신으로서 작업자 노드를 배치할 수 있습니다. 사용 가능한 실제 및 가상 머신 유형은 클러스터를 배치하는 위치에 따라 다릅니다. 자세한 정보는 `bx cs machine-types` [명령](cs_cli_reference.html#cs_machine_types)에 대한 문서를 참조하십시오. 이 값은 표준 클러스터의 경우 필수이며 무료 클러스터에는 사용할 수 없습니다.</dd>

<dt><code>--name <em>NAME</em></code></dt>
<dd>클러스터의 이름입니다.  이 값은 필수입니다. 이름은 문자로 시작해야 하며 35자 이하의 문자, 숫자 및 하이픈(-)을 포함할 수 있습니다. 클러스터 이름과 클러스터가 배치된 지역이 Ingress 하위 도메인의 완전한 이름을 형성합니다. 특정 Ingress 하위 도메인이 지역 내에서 고유하도록 하기 위해 클러스터 이름을 자르고 Ingress 도메인 이름 내의 무작위 값을 추가할 수 있습니다.
</dd>

<dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
<dd>클러스터 마스터 노드를 위한 Kubernetes 버전입니다. 이 값은 선택사항입니다. 버전이 지정되지 않은 경우에는 클러스터가 지원되는 Kubernetes 버전의 기본값으로 작성됩니다. 사용 가능한 버전을 보려면 <code>bx cs kube-versions</code>를 실행하십시오.
</dd>

<dt><code>--no-subnet</code></dt>
<dd>기본적으로 공용 및 사설 포터블 서브넷이 클러스터와 연관된 VLAN에서 작성됩니다. 클러스터에서 서브넷을 작성하지 않도록 하려면 <code>--no-subnet</code> 플래그를 포함하십시오. 나중에 서브넷을 [작성](#cs_cluster_subnet_create)하거나 클러스터에 [추가](#cs_cluster_subnet_add)할 수 있습니다.</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>

<ul>
<li>무료 클러스터에는 이 매개변수를 사용할 수 없습니다.</li>
<li>이 표준 클러스터가 이 위치에서 작성하는 첫 번째 표준 클러스터인 경우 이 플래그를 포함하지 마십시오. 클러스터가 작성되면 사설 VLAN이 작성됩니다.</li>
<li>이 위치에서 이전에 표준 클러스터를 작성했거나 IBM Cloud 인프라(SoftLayer)에서 이전에 사설 VLAN을 작성한 경우 이 사설 VLAN을 지정해야 합니다.

<p><strong>참고:</strong> 사설 VLAN 라우터는 항상 <code>bcr</code>(벡엔드 라우터)로 시작하고 공용 VLAN 라우터는 항상 <code>fcr</code>(프론트 엔드 라우터)로 시작합니다. 클러스터를 작성하고 공인 및 사설 VLAN을 지정할 때는 이러한 접두부 뒤의 숫자 및 문자 조합이 일치해야 합니다.</p></li>
</ul>

<p>특정 위치에 대한 사설 VLAN이 이미 있는지 찾거나 기존 사설 VLAN의 이름을 찾으려면 <code>bx cs vlans <em>&lt;location&gt;</em></code>을 실행하십시오.</p></dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>
<ul>
<li>무료 클러스터에는 이 매개변수를 사용할 수 없습니다.</li>
<li>이 표준 클러스터가 이 위치에서 작성하는 첫 번째 표준 클러스터인 경우 이 플래그를 사용하지 마십시오. 클러스터가 작성되면 공용 VLAN이 작성됩니다.</li>
<li>이 위치에서 이전에 표준 클러스터를 작성했거나 IBM Cloud 인프라(SoftLayer)에서 이전에 공용 VLAN을 작성한 경우 해당 공용 VLAN을 지정해야 합니다.

<p><strong>참고:</strong> 사설 VLAN 라우터는 항상 <code>bcr</code>(벡엔드 라우터)로 시작하고 공용 VLAN 라우터는 항상 <code>fcr</code>(프론트 엔드 라우터)로 시작합니다. 클러스터를 작성하고 공인 및 사설 VLAN을 지정할 때는 이러한 접두부 뒤의 숫자 및 문자 조합이 일치해야 합니다.</p></li>
</ul>

<p>특정 위치에 대한 공용 VLAN이 이미 있는지 찾거나 기존 공용 VLAN의 이름을 찾으려면 <code>bx cs vlans <em>&lt;location&gt;</em></code>을 실행하십시오.</p></dd>

<dt><code>--workers WORKER</code></dt>
<dd>클러스터에 배치하려는 작업자 노드의 수입니다. 이 옵션을 지정하지 않으면 1개의 작업자 노드가 있는 클러스터가 작성됩니다. 이 값은 표준 클러스터의 경우 선택사항이며 무료 클러스터에는 사용할 수 없습니다.

<p><strong>참고:</strong> 모든 작업자 노드에는 클러스터가 작성된 이후 수동으로 변경될 수 없는 고유 작업자 노드 ID 및 도메인 이름이 지정됩니다. ID 또는 도메인 이름을 변경하면 Kubernetes 마스터가 클러스터를 관리할 수 없습니다.</p></dd>

<dt><code>--disable-disk-encrypt</code></dt>
<dd>작업자 노드는 기본적으로 디스크 암호화 기능을 합니다. [자세히 보기](cs_secure.html#worker). 암호화를 사용 안함으로 설정하려면 이 옵션을 포함하십시오.</dd>

<dt><code>--trusted</code></dt>
<dd><p>**베어메탈 전용**: [신뢰할 수 있는 컴퓨팅](cs_secure.html#trusted_compute)을 사용하여 베어메탈 작업자 노드를 변조와 비교하여 확인합니다. 클러스터 작성 중에 신뢰를 사용하도록 설정하지 않았으나 나중에 사용하도록 설정하기를 원하는 경우 `bx cs feature-enable` [명령](cs_cli_reference.html#cs_cluster_feature_enable)을 사용할 수 있습니다. 신뢰를 사용하도록 설정한 후에는 나중에 사용하지 않도록 설정할 수 없습니다.</p>
<p>베어메탈 머신 유형이 신뢰를 지원하는지 여부를 확인하려면 `bx cs machine-types <location>` [명령](#cs_machine_types)의 출력에서 `Trustable` 필드를 확인하십시오. 클러스터에서 신뢰가 사용 가능한지 확인하려면 `bx cs cluster-get` [명령](#cs_cluster_get)의 출력에서 **신뢰 준비** 필드를 보십시오. 베어메탈 작업자 노드에서 신뢰가 사용 가능한지 확인하려면 `bx cs worker-get` [명령](#cs_worker_get)의 출력에서 **신뢰** 필드를 보십시오.</p></dd>
</dl>

**예제**:

  

  표준 클러스터의 예:
  {: #example_cluster_create}

  ```
  bx cs cluster-create --location dal10 --public-vlan my_public_VLAN_ID --private-vlan my_private_VLAN_ID --machine-type u2c.2x4 --name my_cluster --hardware shared --workers 2
  ```
  {: pre}

  무료 클러스터의 예:

  ```
  bx cs cluster-create --name my_cluster
  ```
  {: pre}

  {{site.data.keyword.Bluemix_dedicated_notm}} 환경의 예:

  ```
  bx cs cluster-create --machine-type machine-type --workers number --name cluster_name
  ```
  {: pre}

### bx cs cluster-feature-enable CLUSTER [--trusted]
{: #cs_cluster_feature_enable}

기존 클러스터에서 기능을 사용으로 설정합니다.

<strong>명령 옵션</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code><em>--trusted</em></code></dt>
   <dd><p>클러스터에 있는 모든 지원되는 베어메탈 작업자 노드에서 [신뢰할 수 있는 컴퓨팅](cs_secure.html#trusted_compute)을 사용으로 설정하기 위한 플래그를 포함시킵니다. 신뢰를 사용하도록 설정한 후에는 나중에 클러스터에 대해 이를 사용하지 않도록 설정할 수 없습니다.</p>
   <p>베어메탈 머신 유형이 신뢰를 지원하는지 여부를 확인하려면 `bx cs machine-types <location>` [명령](#cs_machine_types)의 출력에서 **Trustable** 필드를 확인하십시오. 클러스터에서 신뢰가 사용 가능한지 확인하려면 `bx cs cluster-get` [명령](#cs_cluster_get)의 출력에서 **신뢰 준비** 필드를 보십시오. 베어메탈 작업자 노드에서 신뢰가 사용 가능한지 확인하려면 `bx cs worker-get` [명령](#cs_worker_get)의 출력에서 **신뢰** 필드를 보십시오.</p></dd>
   </dl>

**명령 예**:

  ```
  bx cs cluster-feature-enable my_cluster --trusted=true
  ```
  {: pre}

### bx cs cluster-get CLUSTER [--showResources]
{: #cs_cluster_get}

조직의 클러스터에 대한 정보를 봅니다.

<strong>명령 옵션</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code><em>--showResources</em></code></dt>
   <dd>추가 기능, VLAN, 서브넷 및 스토리지와 같은 추가 클러스터 리소스를 표시합니다.</dd>
   </dl>

**명령 예**:

  ```
  bx cs cluster-get my_cluster --showResources
  ```
  {: pre}

**출력 예**:

  ```
  Name:        my_cluster
  ID:          abc1234567
  State:       normal
  Trust ready: false
  Created:     2018-01-01T17:19:28+0000
  Location:    dal10
  Master URL:  https://169.xx.xxx.xxx:xxxxx
  Ingress subdomain: my_cluster.us-south.containers.appdomain.cloud
  Ingress secret:    my_cluster
  Workers:     3
  Version:     1.7.16_1511* (1.8.11_1509 latest)
  Owner Email: name@example.com
  Monitoring dashboard: https://metrics.ng.bluemix.net/app/#/grafana4/dashboard/db/link

  Addons
  Name                   Enabled
  customer-storage-pod   true
  basic-ingress-v2       true
  storage-watcher-pod    true

  Subnet VLANs
  VLAN ID   Subnet CIDR         Public   User-managed
  2234947   10.xxx.xx.xxx/29    false    false
  2234945   169.xx.xxx.xxx/29  true    false

  ```
  {: screen}

### bx cs cluster-rm [-f] CLUSTER
{: #cs_cluster_rm}

조직에서 클러스터를 제거합니다.

<strong>명령 옵션</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>-f</code></dt>
   <dd>사용자 프롬프트를 표시하지 않고 클러스터의 제거를 강제 실행하려면 이 옵션을 사용하십시오. 이 값은 선택사항입니다.</dd>
   </dl>

**예제**:

  ```
  bx cs cluster-rm my_cluster
  ```
  {: pre}


### bx cs cluster-update [-f] CLUSTER [--kube-version MAJOR.MINOR.PATCH][--force-update]
{: #cs_cluster_update}

Kubernetes 마스터를 기본 API 버전으로 업데이트합니다. 업데이트 중에는 클러스터에 액세스하거나 클러스터를 변경할 수 없습니다. 사용자가 배치한 작업자 노드, 앱 및 리소스는 수정되지 않고 계속 실행됩니다.

차후 배치를 위해 YAML 파일을 변경해야 할 수도 있습니다. 세부사항은 이 [릴리스 정보](cs_versions.html)를 검토하십시오.

<strong>명령 옵션</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
   <dd>클러스터의 Kubernetes 버전입니다. 버전을 지정하지 않으면 Kubernetes 마스터가 기본 API 버전으로 업데이트됩니다. 사용 가능한 버전을 보려면 [bx cs kube-versions](#cs_kube_versions)를 실행하십시오. 이 값은 선택사항입니다.</dd>

   <dt><code>-f</code></dt>
   <dd>사용자 프롬프트를 표시하지 않고 마스터 업데이트를 강제 실행하려면 이 옵션을 사용하십시오. 이 값은 선택사항입니다.</dd>

   <dt><code>--force-update</code></dt>
   <dd>변경 시 부 버전의 차이가 2보다 큰 경우에도 업데이트를 시도합니다. 이 값은 선택사항입니다.</dd>
   </dl>

**예제**:

  ```
  bx cs cluster-update my_cluster
  ```
  {: pre}


### bx cs clusters
{: #cs_clusters}

조직에서 클러스터의 목록을 봅니다.

<strong>명령 옵션</strong>:

  없음

**예제**:

  ```
  bx cs clusters
  ```
  {: pre}


### bx cs kube-versions
{: #cs_kube_versions}

{{site.data.keyword.containershort_notm}}에서 지원되는 Kubernetes 버전 목록을 봅니다. [클러스터 마스터](#cs_cluster_update) 및 [작업자 노드](cs_cli_reference.html#cs_worker_update)를 안정적인 최신 기능을 위한 기본 버전으로 업데이트합니다.

**명령 옵션**:

  없음

**예제**:

  ```
  bx cs kube-versions
  ```
  {: pre}



<br />



## 클러스터 명령: 서비스 및 통합
{: #cluster_services_commands}


### bx cs cluster-service-bind CLUSTER KUBERNETES_NAMESPACE SERVICE_INSTANCE_NAME
{: #cs_cluster_service_bind}

클러스터에 {{site.data.keyword.Bluemix_notm}} 서비스를 추가합니다. {{site.data.keyword.Bluemix_notm}} 카탈로그에서 사용 가능한 {{site.data.keyword.Bluemix_notm}} 서비스를 보려면 `bx service offerings`를 실행하십시오. **참고**: 서비스 키를 지원하는 {{site.data.keyword.Bluemix_notm}} 서비스만 추가할 수 있습니다.

<strong>명령 옵션</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code><em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>Kubernetes 네임스페이스의 이름입니다. 이 값은 필수입니다.</dd>

   <dt><code><em>SERVICE_INSTANCE_NAME</em></code></dt>
   <dd>바인딩하려는 {{site.data.keyword.Bluemix_notm}} 서비스 인스턴스의 이름입니다. 서비스 인스턴스의 이름을 찾으려면 <code>bx service list</code>를 실행하십시오. 계정에서 두 개 이상의 인스턴스에 동일한 이름이 있는 경우 이름 대신 서비스 인스턴스 ID를 사용하십시오. ID를 찾으려면 <code>bx service show <service instance name> --guid</code>를 실행하십시오. 이러한 값 중 하나가 필수입니다.</dd>
   </dl>

**예제**:

  ```
  bx cs cluster-service-bind my_cluster my_namespace my_service_instance
  ```
  {: pre}


### bx cs cluster-service-unbind CLUSTER KUBERNETES_NAMESPACE SERVICE_INSTANCE_GUID
{: #cs_cluster_service_unbind}

클러스터에서 {{site.data.keyword.Bluemix_notm}} 서비스를 제거합니다.

**참고:** {{site.data.keyword.Bluemix_notm}} 서비스를 제거하면 서비스 신임 정보도 클러스터에서 제거됩니다. 팟(Pod)에서 서비스를 계속 사용 중인 경우,
서비스 신임 정보를 찾을 수 없기 때문에 실패합니다.

<strong>명령 옵션</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code><em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>Kubernetes 네임스페이스의 이름입니다. 이 값은 필수입니다.</dd>

   <dt><code><em>SERVICE_INSTANCE_GUID</em></code></dt>
   <dd>제거하려는 {{site.data.keyword.Bluemix_notm}} 서비스 인스턴스의 ID입니다. 서비스 인스턴스의 ID를 찾으려면 `bx cs cluster-services <cluster_name_or_ID>`를 실행하십시오. 이 값은 필수입니다.</dd>
   </dl>

**예제**:

  ```
  bx cs cluster-service-unbind my_cluster my_namespace 8567221
  ```
  {: pre}


### bx cs cluster-services CLUSTER [--namespace KUBERNETES_NAMESPACE][--all-namespaces]
{: #cs_cluster_services}

클러스터에서 한 개 또는 모든 Kubernetes 네임스페이스에 바인딩된 서비스를 나열합니다. 옵션이 지정되지 않은 경우 기본 네임스페이스를 위한 서비스가 표시됩니다.

<strong>명령 옵션</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code>, <code>-n
<em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>클러스터에서 특정 네임스페이스에 바인딩된 서비스를 포함합니다. 이 값은 선택사항입니다.</dd>

   <dt><code>--all-namespaces</code></dt>
    <dd>클러스터에서 모든 네임스페이스에 바인딩된 서비스를 포함합니다. 이 값은 선택사항입니다.</dd>
    </dl>

**예제**:

  ```
  bx cs cluster-services my_cluster --namespace my_namespace
  ```
  {: pre}



### bx cs webhook-create --cluster CLUSTER --level LEVEL --type slack --url URL
{: #cs_webhook_create}

웹훅을 등록합니다.

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>--level <em>LEVEL</em></code></dt>
   <dd>알림 레벨(예: <code>Normal</code> 또는 <code>Warning</code>)입니다. 기본값은 <code>Warning</code>입니다. 이 값은 선택사항입니다.</dd>

   <dt><code>--type <em>slack</em></code></dt>
   <dd>웹훅 유형입니다. 현재 slack이 지원됩니다. 이 값은 필수입니다.</dd>

   <dt><code>--url <em>URL</em></code></dt>
   <dd>웹훅의 URL입니다. 이 값은 필수입니다.</dd>
   </dl>

**예제**:

  ```
  bx cs webhook-create --cluster my_cluster --level Normal --type slack --url http://github.com/mywebhook
  ```
  {: pre}


<br />


## 클러스터 명령: 서브넷
{: #cluster_subnets_commands}

### bx cs cluster-subnet-add CLUSTER SUBNET
{: #cs_cluster_subnet_add}

IBM Cloud 인프라(SoftLayer) 계정의 서브넷을 지정된 클러스터에서 사용 가능하도록 설정합니다.

**참고:**
* 클러스터에 서브넷을 사용 가능하게 하면 이 서브넷의 IP 주소가 클러스터 네트워킹 목적으로 사용됩니다. IP 주소 충돌을 피하려면 한 개의 클러스터만 있는 서브넷을 사용해야 합니다. 동시에
{{site.data.keyword.containershort_notm}}의 외부에서
다른 목적으로 또는 다중 클러스터에 대한 서브넷으로 사용하지 마십시오.
* 동일한 VLAN의 서브넷 간에 라우팅하려면 [VLAN Spanning을 켜야](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning) 합니다.

<strong>명령 옵션</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code><em>SUBNET</em></code></dt>
   <dd>서브넷의 ID입니다. 이 값은 필수입니다.</dd>
   </dl>

**예제**:

  ```
  bx cs cluster-subnet-add my_cluster 1643389
  ```
  {: pre}


### bx cs cluster-subnet-create CLUSTER SIZE VLAN_ID
{: #cs_cluster_subnet_create}

IBM Cloud 인프라(SoftLayer) 계정에서 서브넷을 작성하고 {{site.data.keyword.containershort_notm}}의 지정된 클러스터에 사용 가능하도록 설정합니다.

**참고:**
* 클러스터에 서브넷을 사용 가능하게 하면 이 서브넷의 IP 주소가 클러스터 네트워킹 목적으로 사용됩니다. IP 주소 충돌을 피하려면 한 개의 클러스터만 있는 서브넷을 사용해야 합니다. 동시에
{{site.data.keyword.containershort_notm}}의 외부에서
다른 목적으로 또는 다중 클러스터에 대한 서브넷으로 사용하지 마십시오.
* 동일한 VLAN의 서브넷 간에 라우팅하려면 [VLAN Spanning을 켜야](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning) 합니다.

<strong>명령 옵션</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다. 클러스터를 나열하려면 `bx cs clusters` [명령](#cs_clusters)을 사용하십시오.</dd>

   <dt><code><em>SIZE</em></code></dt>
   <dd>서브넷 IP 주소의 수입니다. 이 값은 필수입니다. 가능한 값은 8, 16, 32 또는 64입니다.</dd>

   <dt><code><em>VLAN_ID</em></code></dt>
   <dd>서브넷을 작성할 VLAN입니다. 이 값은 필수입니다. 사용 가능한 VLAN을 나열하려면 `bx cs vlans <location>` [명령](#cs_vlans)을 사용하십시오. </dd>
   </dl>

**예제**:

  ```
  bx cs cluster-subnet-create my_cluster 8 1764905
  ```
  {: pre}


### bx cs cluster-user-subnet-add CLUSTER SUBNET_CIDR PRIVATE_VLAN
{: #cs_cluster_user_subnet_add}

고유한 사설 서브넷을 {{site.data.keyword.containershort_notm}} 클러스터로 가져옵니다.

이 사설 서브넷은 IBM Cloud 인프라(SoftLayer)에서 제공되는 서브넷이 아닙니다. 따라서 서브넷에 대한 인바운드 및 아웃바운드 네트워크 트래픽 라우팅을 구성해야 합니다. IBM Cloud 인프라(SoftLayer) 서브넷을 추가하려면 `bx cs cluster-subnet-add` [명령](#cs_cluster_subnet_add)을 사용하십시오.

**참고**:
* 클러스터에 사설 사용자 서브넷을 추가하면 이 서브넷의 IP 주소가 클러스터의 사설 Load Balancers로 사용됩니다. IP 주소 충돌을 피하려면 한 개의 클러스터만 있는 서브넷을 사용해야 합니다. 동시에
{{site.data.keyword.containershort_notm}}의 외부에서
다른 목적으로 또는 다중 클러스터에 대한 서브넷으로 사용하지 마십시오.
* 동일한 VLAN의 서브넷 간에 라우팅하려면 [VLAN Spanning을 켜야](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning) 합니다.

<strong>명령 옵션</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code><em>SUBNET_CIDR</em></code></dt>
   <dd>서브넷 CIDR(Classless InterDomain Routing)입니다. 이 값은 필수이며 IBM Cloud 인프라(SoftLayer)에서 사용되는 서브넷과 충돌하지 않아야 합니다.

   지원되는 접두부의 범위는 `/30`(1개의 IP 주소) - `/24`(253개의 IP 주소)입니다. 하나의 접두부 길이에 CIDR을 설정하고 나중에 이를 변경해야 하는 경우 먼저 새 CIDR을 추가한 후 [이전 CIDR을 제거](#cs_cluster_user_subnet_rm)하십시오.</dd>

   <dt><code><em>PRIVATE_VLAN</em></code></dt>
   <dd>사설 VLAN의 ID입니다. 이 값은 필수입니다. 클러스터에 있는 하나 이상의 작업자 노드의 사설 VLAN ID와 일치해야 합니다.</dd>
   </dl>

**예제**:

  ```
  bx cs cluster-user-subnet-add my_cluster 169.xx.xxx.xxx/29 1502175
  ```
  {: pre}


### bx cs cluster-user-subnet-rm CLUSTER SUBNET_CIDR PRIVATE_VLAN
{: #cs_cluster_user_subnet_rm}

지정된 클러스터에서 고유한 사설 서브넷을 제거합니다.

**참고:** 고유한 사설 서브넷에서 IP 주소에 배치된 서비스는 서브넷이 제거된 후에도 활성 상태로 남아 있습니다.

<strong>명령 옵션</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code><em>SUBNET_CIDR</em></code></dt>
   <dd>서브넷 CIDR(Classless InterDomain Routing)입니다. 이 값은 필수이며 `bx cs cluster-user-subnet-add` [명령](#cs_cluster_user_subnet_add)을 사용하여 설정된 CIDR과 일치해야 합니다.</dd>

   <dt><code><em>PRIVATE_VLAN</em></code></dt>
   <dd>사설 VLAN의 ID입니다. 이 값은 필수이며 `bx cs cluster-user-subnet-add` [명령](#cs_cluster_user_subnet_add)을 사용하여 설정된 VLAN ID와 일치해야 합니다.</dd>
   </dl>

**예제**:

  ```
  bx cs cluster-user-subnet-rm my_cluster 169.xx.xxx.xxx/29 1502175
  ```
  {: pre}

### bx cs subnets
{: #cs_subnets}

IBM Cloud 인프라(SoftLayer) 계정에서 사용 가능한 서브넷의 목록을 봅니다.

<strong>명령 옵션</strong>:

   없음

**예제**:

  ```
   bx cs subnets
  ```
  {: pre}


<br />


## Ingress 애플리케이션 로드 밸런서(ALB) 명령
{: #alb_commands}

### bx cs alb-cert-deploy [--update] --cluster CLUSTER --secret-name SECRET_NAME --cert-crn CERTIFICATE_CRN
{: #cs_alb_cert_deploy}

{{site.data.keyword.cloudcerts_long_notm}} 인스턴스의 인증서를 클러스터의 ALB에 배치하거나 업데이트합니다.

**참고:**
* 관리자 액세스 역할이 있는 사용자만 이 명령을 실행할 수 있습니다.
* 같은 {{site.data.keyword.cloudcerts_long_notm}} 인스턴스에서 가져온 인증서만 업데이트할 수 있습니다.

<strong>명령 옵션</strong>

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>--update</code></dt>
   <dd>클러스터의 ALB 시크릿에 대한 인증서를 업데이트합니다. 이 값은 선택사항입니다.</dd>

   <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
   <dd>ALB 시크릿의 이름입니다. 이 값은 필수입니다.</dd>

   <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
   <dd>인증서 CRN입니다. 이 값은 필수입니다.</dd>
   </dl>

**예제**:

ALB 시크릿 배치 예:

   ```
   bx cs alb-cert-deploy --secret-name my_alb_secret --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
   ```
   {: pre}

기존 ALB 시크릿 업데이트 예:

 ```
 bx cs alb-cert-deploy --update --secret-name my_alb_secret --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:7e21fde8ee84a96d29240327daee3eb2
 ```
 {: pre}


### bx cs alb-cert-get --cluster CLUSTER [--secret-name SECRET_NAME][--cert-crn CERTIFICATE_CRN]
{: #cs_alb_cert_get}

클러스터의 ALB 시크릿에 관한 정보를 봅니다.

**참고:** 관리자 액세스 역할이 있는 사용자만 이 명령을 실행할 수 있습니다.

<strong>명령 옵션</strong>

  <dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
  <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

  <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
  <dd>ALB 시크릿의 이름입니다. 이 값은 클러스터의 특정 ALB 시크릿에 관한 정보를 가져오는 데 필요합니다.</dd>

  <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
  <dd>인증서 CRN입니다. 이 값은 클러스터의 특정 인증서 CRN과 일치하는 모든 ALB 시크릿에 관한 정보를 가져오는 데 필요합니다.</dd>
  </dl>

**예제**:

 ALB 시크릿에 관한 정보 가져오기 예:

 ```
 bx cs alb-cert-get --cluster my_cluster --secret-name my_alb_secret
 ```
 {: pre}

 지정된 인증서 CRN과 일치하는 모든 ALB 시크릿에 관한 정보 가져오기 예:

 ```
 bx cs alb-cert-get --cluster my_cluster --cert-crn  crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
 ```
 {: pre}


### bx cs alb-cert-rm --cluster CLUSTER [--secret-name SECRET_NAME][--cert-crn CERTIFICATE_CRN]
{: #cs_alb_cert_rm}

클러스터의 ALB 시크릿을 제거합니다.

**참고:** 관리자 액세스 역할이 있는 사용자만 이 명령을 실행할 수 있습니다.

<strong>명령 옵션</strong>

  <dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
  <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

  <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
  <dd>ALB 시크릿의 이름입니다. 이 값은 클러스터의 특정 ALB 시크릿을 제거하는 데 필요합니다.</dd>

  <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
  <dd>인증서 CRN입니다. 이 값은 클러스터의 특정 인증서 CRN과 일치하는 모든 ALB 시크릿을 제거하는 데 필요합니다.</dd>
  </dl>

**예제**:

 ALB 시크릿 제거 예:

 ```
 bx cs alb-cert-rm --cluster my_cluster --secret-name my_alb_secret
 ```
 {: pre}

 지정된 인증서 CRN과 일치하는 모든 ALB 시크릿 제거 예:

 ```
 bx cs alb-cert-rm --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
 ```
 {: pre}


### bx cs alb-certs --cluster CLUSTER
{: #cs_alb_certs}

클러스터의 ALB 시크릿 목록을 봅니다.

**참고:** 관리자 액세스 역할이 있는 사용자만 이 명령을 실행할 수 있습니다.

<strong>명령 옵션</strong>

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>
   </dl>

**예제**:

 ```
 bx cs alb-certs --cluster my_cluster
 ```
 {: pre}

### bx cs alb-configure --albID ALB_ID [--enable][--disable][--user-ip USERIP]
{: #cs_alb_configure}

표준 클러스터에서 ALB를 사용 또는 사용 안함으로 설정합니다. 기본적으로 공용 ALB는 사용 가능합니다.

**명령 옵션**:

   <dl>
   <dt><code><em>--albID </em>ALB_ID</code></dt>
   <dd>ALB의 ID입니다. 클러스터에서 ALB의 ID를 보려면 <code>bx cs albs <em>--cluster </em>CLUSTER</code>를 실행하십시오. 이 값은 필수입니다.</dd>

   <dt><code>--enable</code></dt>
   <dd>클러스터에서 ALB를 사용으로 설정하려면 이 플래그를 포함하십시오.</dd>

   <dt><code>--disable</code></dt>
   <dd>클러스터에서 ALB를 사용 안함으로 설정하려면 이 플래그를 포함하십시오.</dd>

   <dt><code>--user-ip <em>USER_IP</em></code></dt>
   <dd>

   <ul>
    <li>이 매개변수는 사설 ALB를 사용으로 설정하는 데만 사용할 수 있습니다.</li>
    <li>사설 ALB는 사용자 제공 사설 서브넷의 IP 주소로 배치됩니다. IP 주소가 제공되지 않으면 ALB는 클러스터를 작성할 때 자동으로 프로비저닝된 포터블 사설 서브넷의 사설 IP 주소로 배치됩니다.</li>
   </ul>
   </dd>
   </dl>

**예제**:

  ALB 사용 예:

  ```
  bx cs alb-configure --albID private-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --enable
  ```
  {: pre}

  사용자 제공 IP 주소로 ALB를 사용으로 설정하는 예:

  ```
  bx cs alb-configure --albID private-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --enable --user-ip user_ip
  ```
  {: pre}

  ALB 사용 안함 예:

  ```
  bx cs alb-configure --albID public-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --disable
  ```
  {: pre}

### bx cs alb-get --albID ALB_ID
{: #cs_alb_get}

ALB의 세부사항을 봅니다.

<strong>명령 옵션</strong>:

   <dl>
   <dt><code><em>--albID </em>ALB_ID</code></dt>
   <dd>ALB의 ID입니다. 클러스터에서 ALB의 ID를 보려면 <code>bx cs albs <em>--cluster </em>CLUSTER</code>를 실행하십시오. 이 값은 필수입니다.</dd>
   </dl>

**예제**:

  ```
  bx cs alb-get --albID public-cr18a61a63a6a94b658596aa93a087aaa9-alb1
  ```
  {: pre}

### bx cs alb-types
{: #cs_alb_types}

지역에서 지원되는 ALB 유형을 봅니다.

<strong>명령 옵션</strong>:

   없음

**예제**:

  ```
  bx cs alb-types
  ```
  {: pre}


### bx cs albs --cluster CLUSTER
{: #cs_albs}

클러스터에서 모든 ALB의 상태를 봅니다. ALB ID가 리턴되지 않으면 클러스터에 포터블 서브넷이 없습니다. 서브넷을 [작성](#cs_cluster_subnet_create)하거나 클러스터에 [추가](#cs_cluster_subnet_add)할 수 있습니다.

<strong>명령 옵션</strong>:

   <dl>
   <dt><code><em>--cluster </em>CLUSTER</code></dt>
   <dd>사용 가능한 ALB를 나열하는 클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>
   </dl>

**예제**:

  ```
  bx cs albs --cluster my_cluster
  ```
  {: pre}


<br />


## 인프라 명령
{: #infrastructure_commands}

### bx cs credentials-set --infrastructure-api-key API_KEY --infrastructure-username USERNAME
{: #cs_credentials_set}

{{site.data.keyword.containershort_notm}} 계정에 대한 IBM Cloud 인프라(SoftLayer) 계정 신임 정보를 설정합니다.

{{site.data.keyword.Bluemix_notm}} 종량과금제 계정이 있는 경우 기본적으로 IBM Cloud 인프라(SoftLayer) 포트폴리오에 대한 액세스 권한이 제공됩니다. 그러나 인프라를 정렬하기 위해 이미 보유하고 있는 다른 IBM Cloud 인프라(SoftLayer) 계정을 사용하려고 할 수 있습니다. 이 명령을 사용하여 인프라 계정을 {{site.data.keyword.Bluemix_notm}} 계정에 연결할 수 있습니다.

IBM Cloud 인프라(SoftLayer) 신임 정보가 수동으로 설정되면 [IAM API 키](#cs_api_key_info)가 이미 계정에 대해 존재하는 경우에도 인프라를 정렬하는 데 이 신임 정보가 사용됩니다. 신임 정보를 저장하는 사용자에게 인프라를 정렬하기 위한 필수 권한이 없는 경우 클러스터 작성 또는 작업자 노드 다시 로드와 같은 인프라 관련 조치에 실패할 수 있습니다.

하나의 {{site.data.keyword.containershort_notm}} 계정에 여러 신임 정보를 설정할 수 없습니다. 모든 {{site.data.keyword.containershort_notm}} 계정이 하나의 IBM Cloud 인프라(SoftLayer) 포트폴리오에만 연결됩니다.

**중요:** 이 명령을 시작하기 전에 신임 정보를 사용하는 사용자가 필수 [{{site.data.keyword.containershort_notm}} 및 IBM Cloud 인프라(SoftLayer) 권한](cs_users.html#users)을 보유하고 있는지 확인하십시오.

<strong>명령 옵션</strong>:

   <dl>
   <dt><code>--infrastructure-username <em>USERNAME</em></code></dt>
   <dd>IBM Cloud 인프라(SoftLayer) 계정 사용자 이름입니다. 이 값은 필수입니다.</dd>


   <dt><code>--infrastructure-api-key <em>API_KEY</em></code></dt>
   <dd>IBM Cloud 인프라(SoftLayer) 계정 API 키입니다. 이 값은 필수입니다.

 <p>

API 키를 생성하려면 다음을 수행하십시오.

  <ol>
  <li>[IBM Cloud 인프라(SoftLayer) 포털 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/)에 로그인하십시오.</li>
  <li><strong>계정</strong>을 선택한 후에 <strong>사용자</strong>를 선택하십시오.</li>
  <li><strong>생성</strong>을 클릭하여 사용자 계정에 대한 IBM Cloud 인프라(SoftLayer) API 키를 생성하십시오.</li>
  <li>이 명령에서 사용할 API 키를 복사하십시오.</li>
  </ol>

  기존 API 키를 보려면 다음을 수행하십시오.
  <ol>
  <li>[IBM Cloud 인프라(SoftLayer) 포털 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/)에 로그인하십시오.</li>
  <li><strong>계정</strong>을 선택한 후에 <strong>사용자</strong>를 선택하십시오.</li>
  <li><strong>보기</strong>를 클릭하여 기존 API 키를 확인하십시오.</li>
  <li>이 명령에서 사용할 API 키를 복사하십시오.</li>
  </ol>
  </p></dd>
  </dl>

**예제**:

  ```
  bx cs credentials-set --infrastructure-api-key <api_key> --infrastructure-username dbmanager
  ```
  {: pre}


###   bx cs credentials-unset
{: #cs_credentials_unset}

{{site.data.keyword.containershort_notm}} 계정에서 IBM Cloud 인프라(SoftLayer) 계정 신임 정보를 제거합니다.

신임 정보를 제거한 후에는 [IAM API 키](#cs_api_key_info)가 IBM Cloud 인프라(SoftLayer)에서 리소스를 정렬하는 데 사용됩니다.

<strong>명령 옵션</strong>:

   없음

**예제**:

  ```
  bx cs credentials-unset
  ```
  {: pre}


###   bx cs machine-types LOCATION
{: #cs_machine_types}

작업자 노드에 대해 사용 가능한 머신 유형의 목록을 봅니다. 각각의 머신 유형에는
클러스터의 각 작업자 노드에 대한 가상 CPU, 메모리 및 디스크 공간의 양이 포함됩니다. 기본적으로, 모든 컨테이너 데이터가 저장된 `/var/lib/docker` 디렉토리는 LUKS 암호화를 통해 암호화됩니다. 클러스터 작성 중 `disable-disk-encrypt` 옵션이 포함된 경우, 호스트의 Docker 데이터가 암호화되지 않습니다. [암호화에 대해 자세히 알아보십시오.](cs_secure.html#encrypted_disks)
{:shortdesc}

공유 또는 전용 하드웨어에서 가상 머신으로서 또는 베어메탈에서 실제 머신으로서 작업자 노드를 프로비저닝할 수 있습니다.

<dl>
<dt>실제 머신(베어메탈)을 사용해야 하는 이유는 무엇입니까?</dt>
<dd><p><strong>더 많은 컴퓨팅 리소스</strong>: 베어메탈이라고도 하는 싱글 테넌트 실제 서버로 작업자 노드를 프로비저닝할 수 있습니다. 베어메탈은 메모리 또는 CPU와 같이 머신의 실제 리소스에 직접 액세스를 제공합니다. 이 설정은 호스트에서 실행되는 가상 머신에 실제 리소스를 할당하는 가상 머신 하이퍼바이저를 제거합니다. 대신, 모든 베어메탈 머신의 리소스가 작업자 전용으로만 사용되므로 리소스를 공유하거나 성능을 저하시키는 "시끄러운 이웃(noisy neighbors)" 문제를 신경쓰지 않아도 됩니다. 실제 머신 유형에는 가상 머신 유형보다 더 많은 로컬 스토리지가 있으며, 일부에는 로컬 데이터를 백업할 수 있는 RAID가 있습니다.</p>
<p><strong>월별 비용 청구</strong>: 베어메탈 서버는 가상 서버보다 더 비싸며, 추가적인 리소스 및 호스트 제어 기능이 필요한 고성능 앱에 가장 적합합니다. 베어메탈 서버는 월별로 비용이 청구됩니다. 월말 전에 베어메탈 서버를 취소하는 경우 해당 월말까지 비용이 청구됩니다. 베어메탈 서버 주문 및 취소는 IBM Cloud 인프라(SoftLayer) 계정을 통해 이뤄지는 수동 프로세스입니다. 완료하는 데 1영업일 이상이 소요될 수 있습니다.</p>
<p><strong>신뢰할 수 있는 컴퓨팅을 사용하기 위한 옵션</strong>: 신뢰할 수 있는 컴퓨팅을 사용으로 설정하여 작업자 노드의 변조 여부를 확인하십시오. 클러스터 작성 중에 신뢰를 사용하도록 설정하지 않았으나 나중에 사용하도록 설정하기를 원하는 경우 `bx cs feature-enable` [명령](cs_cli_reference.html#cs_cluster_feature_enable)을 사용할 수 있습니다. 신뢰를 사용하도록 설정한 후에는 나중에 사용하지 않도록 설정할 수 없습니다. 신뢰가 없는 새 클러스터를 작성할 수 있습니다. 노드 시작 프로세스 중에 신뢰가 작동하는 방법에 대한 정보는 [신뢰할 수 있는 컴퓨팅을 사용하는 {{site.data.keyword.containershort_notm}}](cs_secure.html#trusted_compute)를 참조하십시오. 신뢰할 수 있는 컴퓨팅은 Kubernetes 버전 1.9 이상을 실행하며 특정 베어메탈 머신 유형을 포함하는 클러스터에서 사용 가능합니다. `bx cs machine-types <location>` [명령](cs_cli_reference.html#cs_machine_types)을 실행하는 경우 **Trustable** 필드를 검토하여 신뢰를 지원하는 머신을 확인할 수 있습니다. 예를 들어, `mgXc` GPU 특성(flavor)은 신뢰할 수 있는 컴퓨팅을 지원하지 않습니다.</p></dd>
<dt>가상 머신을 사용해야 하는 이유는 무엇입니까?</dt>
<dd><p>VM을 사용하면 더 비용 효율적인 가격으로 베어메탈보다 더 뛰어난 유연성, 빠른 프로비저닝 시간 및 자동화된 확장성 기능을 얻을 수 있습니다. 테스트 및 개발 환경, 스테이징 및 프로덕션 환경, 마이크로서비스 및 비즈니스 앱과 같은 가장 일반적인 용도의 유스 케이스에 VM을 사용할 수 있습니다. 그러나 성능에는 트레이드오프가 있을 수 있습니다. RAM, 데이터 및 GPU 집약적인 워크로드에 고성능 컴퓨팅이 필요한 경우 베어메탈을 사용하십시오.</p>
<p><strong>단일 또는 멀티 테넌시 간에 결정</strong>:표준 가상 클러스터를 작성하는 경우 기본 하드웨어를 여러 {{site.data.keyword.IBM_notm}} 고객이 공유할 것인지(멀티 테넌시) 또는 사용자만 전용으로 사용할 것인지(단일 테넌시)를 선택해야 합니다.</p>
<p>멀티 테넌트 설정에서 실제 리소스(예: CPU 및 메모리)는 동일한 실제 하드웨어에 배치된 모든 가상 머신 간에 공유됩니다. 모든 가상 머신이 독립적으로 실행될 수 있도록 보장하기 위해, 가상 머신 모니터(하이퍼바이저라고도 함)는 실제 리소스를 격리된 엔티티로 세그먼트화하고 이를 전용 리소스로서 가상 머신에 할당합니다(하이퍼바이저 격리).</p>
<p>싱글 테넌트 설정에서 모든 실제 리소스는 사용자에게만 전용으로 제공됩니다. 동일한 실제 호스트에서 가상 머신으로서 여러 작업자 노드를 배치할 수 있습니다. 멀티 테넌트 설정과 유사하게, 하이퍼바이저는 모든 작업자 노드가 사용 가능한 실제 리소스의 해당 공유를 가져오도록 보장합니다.</p>
<p>기반 하드웨어의 비용이 여러 고객 간에 공유되므로, 공유 노드는 일반적으로 전용 노드보다 비용이 저렴합니다. 그러나 공유 및 전용 노드 간에 결정하는 경우, 사용자는 자체 법률 부서에 문의하여 앱 환경에서 요구하는 인프라 격리 및 준수의 레벨을 논의하고자 할 수 있습니다.</p>
<p><strong>가상 `u2c` 또는 `b2c` 머신 특성</strong>: 이러한 머신은 신뢰성을 위해 SAN(Storage Area Networing) 대신 로컬 디스크를 사용합니다. 신뢰성을 갖게 되면 로컬 디스크에 바이트를 직렬화하는 경우 처리량이 많아지고 네트워크 장애로 인한 파일 시스템 성능 저하를 줄일 수 있습니다. 이러한 머신 유형에는 OS 파일 시스템을 위한 25GB 기본 로컬 디스크 스토리지 및 모든 컨테이너 데이터가 기록되는 디렉토리 `/var/lib/docker`를 위한 100GB 보조 로컬 디스크 스토리지가 포함됩니다.</p>
<p><strong>더 이상 사용되지 않는 `u1c` 또는 `b1c` 머신 유형이 있으면 어떻게 됩니까?</strong> `u2c` 및 `b2c` 머신 유형의 사용을 시작하려면 [작업자 노드를 추가하여 머신 유형을 업데이트](cs_cluster_update.html#machine_type)하십시오.</p></dd>
<dt>선택할 수 있는 가상 및 실제 머신 특성은 무엇입니까?</dt>
<dd><p>많습니다! 유스 케이스에 가장 적합한 머신의 유형을 선택하십시오. 작업자 풀은 특성이 동일한 머신으로 구성된다는 점에 유의하십시오. 클러스터에서 머신 유형을 혼합하려면 각 특성마다 별도의 작업자 풀을 작성하십시오.</p>
<p>머신 유형은 지역에 따라 다릅니다. 해당 지역에서 사용 가능한 머신 유형을 보려면 `bx cs machine-types <zone_name>`을 실행하십시오.</p>
<p><table>
<caption>{{site.data.keyword.containershort_notm}}에서 사용 가능한 실제(베어메탈) 및 가상 머신 유형</caption>
<thead>
<th>이름 및 유스 케이스</th>
<th>코어 수 / 메모리</th>
<th>기본 / 보조 디스크</th>
<th>네트워크 속도</th>
</thead>
<tbody>
<tr>
<td><strong>가상, u2c.2x4</strong>: 빠른 테스트, 개념 증명 및 기타 경량 워크로드에는 이 가장 작은 크기의 VM을 사용하십시오.</td>
<td>2 / 4GB</td>
<td>25GB / 100GB</td>
<td>1000Mbps</td>
</tr>
<tr>
<td><strong>가상, b2c.4x16</strong>: 테스트, 개발 및 기타 경량 워크로드의 경우 이 균형 VM을 선택하십시오.</td>
<td>4 / 16GB</td>
<td>25GB / 100GB</td>
<td>1000Mbps</td>
</tr>
<tr>
<td><strong>가상, b2c.16x64</strong>: 중간 규모의 워크로드의 경우 이 균형 VM을 선택하십시오.</td></td>
<td>16 / 64GB</td>
<td>25GB / 100GB</td>
<td>1000Mbps</td>
</tr>
<tr>
<td><strong>가상, b2c.32x128</strong>: 동시 사용자가 많은 데이터베이스 및 동적 웹 사이트와 같은 중간 규모에서 대규모 워크로드의 경우 이 균형 VM을 선택하십시오.</td></td>
<td>32 / 128GB</td>
<td>25GB / 100GB</td>
<td>1000Mbps</td>
</tr>
<tr>
<td><strong>가상, b2c.56x242</strong>: 동시 사용자가 많은 데이터베이스 및 다중 앱과 같은 대규모 워크로드의 경우 이 균형 VM을 선택하십시오.</td></td>
<td>56 / 242GB</td>
<td>25GB / 100GB</td>
<td>1000Mbps</td>
</tr>
<tr>
<td><strong>RAM 집약적인 베어메탈, mr1c.28x512</strong>: 작업자 노드에 사용 가능한 RAM을 최대화하십시오.</td>
<td>28 / 512GB</td>
<td>2TB SATA / 960GB SSD</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>GPU 베어메탈, mg1c.16x128</strong>: 고성능 컴퓨팅, 기계 학습 또는 3D 애플리케이션과 같은 수학적으로 집약적인 워크로드의 경우 이 유형을 선택하십시오. 이 특성에는 1개의 Tesla K80 실제 카드가 있으며 카드당 2개씩 총 2개의 그래픽 처리 장치(GPU)가 포함되어 있습니다.</td>
<td>16 / 128GB</td>
<td>2TB SATA / 960GB SSD</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>GPU 베어메탈, mg1c.28x256</strong>: 고성능 컴퓨팅, 기계 학습 또는 3D 애플리케이션과 같은 수학적으로 집약적인 워크로드의 경우 이 유형을 선택하십시오. 이 특성에는 2개의 Tesla K80 실제 카드가 있으며 카드당 2개씩 총 4개의 GPU가 포함되어 있습니다.</td>
<td>28 / 256GB</td>
<td>2TB SATA / 960GB SSD</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>데이터 집약적인 베어메탈, md1c.16x64.4x4tb</strong>: 머신에 로컬로 저장된 데이터를 백업하기 위한 RAID를 포함하여 상당한 크기의 로컬 디스크 스토리지의 경우. 분산 파일 시스템, 대형 데이터베이스 및 빅데이터 분석 워크로드와 같은 경우에 사용하십시오.</td>
<td>16 / 64GB</td>
<td>2x2TB RAID1 / 4x4TB SATA RAID10</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>데이터 집약적인 베어메탈, md1c.28x512.4x4tb</strong>: 머신에 로컬로 저장된 데이터를 백업하기 위한 RAID를 포함하여 상당한 크기의 로컬 디스크 스토리지의 경우. 분산 파일 시스템, 대형 데이터베이스 및 빅데이터 분석 워크로드와 같은 경우에 사용하십시오.</td>
<td>28 / 512GB</td>
<td>2x2TB RAID1 / 4x4TB SATA RAID10</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>균형 베어메탈, mb1c.4x32</strong>: 가상 머신에서 제공하는 것보다 더 많은 컴퓨팅 리소스가 필요한 균형 워크로드에 사용하십시오.</td>
<td>4 / 32GB</td>
<td>2TB SATA / 2TB SATA</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>균형 베어메탈, mb1c.16x64</strong>: 가상 머신에서 제공하는 것보다 더 많은 컴퓨팅 리소스가 필요한 균형 워크로드에 사용하십시오.</td>
<td>16 / 64GB</td>
<td>2TB SATA / 960GB SSD</td>
<td>10000Mbps</td>
</tr>
</tbody>
</table>
</p>
</dd>
</dl>


<strong>명령 옵션</strong>:

   <dl>
   <dt><code><em>LOCATION</em></code></dt>
   <dd>사용 가능한 머신 유형을 나열하려는 위치를 입력하십시오. 이 값은 필수입니다. [사용 가능한 위치](cs_regions.html#locations)를 검토하십시오.</dd></dl>

**명령 예**:

  ```
  bx cs machine-types dal10
  ```
  {: pre}

**출력 예**:

  ```
  Getting machine types list...
  OK
  Machine Types
  Name                 Cores   Memory   Network Speed   OS             Server Type   Storage   Secondary Storage   Trustable
  u2c.2x4              2       4GB      1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  b2c.4x16             4       16GB     1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  b2c.16x64            16      64GB     1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  b2c.32x128           32      128GB    1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  b2c.56x242           56      242GB    1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  mb1c.4x32            4       32GB     10000Mbps       UBUNTU_16_64   physical      1000GB    2000GB              False
  mb1c.16x64           16      64GB     10000Mbps       UBUNTU_16_64   physical      1000GB    1700GB              False
  mr1c.28x512          28      512GB    10000Mbps       UBUNTU_16_64   physical      1000GB    1700GB              False
  md1c.16x64.4x4tb     16      64GB     10000Mbps       UBUNTU_16_64   physical      1000GB    8000GB              False
  md1c.28x512.4x4tb    28      512GB    10000Mbps       UBUNTU_16_64   physical      1000GB    8000GB              False
  
  ```
  {: screen}


### bx cs vlans LOCATION [--all]
{: #cs_vlans}

IBM Cloud 인프라(SoftLayer) 계정의 위치에 사용 가능한 공용 및 사설 VLAN을 나열합니다. 사용 가능한 VLAN을 나열하려면 유료 계정이 있어야 합니다.

<strong>명령 옵션</strong>:

   <dl>
   <dt><code><em>LOCATION</em></code></dt>
   <dd>사설 및 공용 VLAN을 나열하려는 위치를 입력하십시오. 이 값은 필수입니다. [사용 가능한 위치](cs_regions.html#locations)를 검토하십시오.</dd>
   <dt><code>--all</code></dt>
   <dd>사용 가능한 모든 VLAN을 나열합니다. 기본적으로 VLAN은 유효한 VLAN만 표시되도록 필터링됩니다. 올바른 상태가 되려면 VLAN은 로컬 디스크 스토리지로 작업자를 호스팅할 수 있는 인프라와 연관되어야 합니다.</dd>
   </dl>

**예제**:

  ```
  bx cs vlans dal10
  ```
  {: pre}


<br />


## 로깅 명령
{: #logging_commands}

### bx cs logging-config-create CLUSTER --logsource LOG_SOURCE [--namespace KUBERNETES_NAMESPACE][--hostname LOG_SERVER_HOSTNAME_OR_IP] [--port LOG_SERVER_PORT][--space CLUSTER_SPACE] [--org CLUSTER_ORG][--app-containers CONTAINERS] [--app-paths PATHS_TO_LOGS] --type LOG_TYPE [--json][--skip-validation]
{: #cs_logging_create}

로깅 구성을 작성합니다. 이 명령을 사용하여 컨테이너, 애플리케이션, 작업자 노드, Kubernetes 클러스터 및 Ingress 애플리케이션 로드 밸런서에 대한 로그를 {{site.data.keyword.loganalysisshort_notm}} 또는 외부 syslog 서버로 전달할 수 있습니다.

<strong>명령 옵션</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>클러스터의 이름 또는 ID입니다.</dd>
  <dt><code>--logsource <em>LOG_SOURCE</em></code></dt>
    <dd>로그 전달을 사용할 로그 소스입니다. 이 인수는 구성을 적용할 로그 소스의 쉼표로 구분된 목록을 지원합니다. 허용되는 값은 <code>container</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code> 및 <code>ingress</code>입니다. 로그 소스를 제공하지 않는 경우 로깅 구성은 <code>container</code> 및 <code>ingress</code> 로그 소스에 작성됩니다.</dd>
  <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
    <dd>로그를 전달할 Kubernetes 네임스페이스입니다. <code>ibm-system</code> 및 <code>kube-system</code> Kubernetes 네임스페이스의 경우 로그 전달이 지원되지 않습니다. 이 값은 container 로그 소스에 대해서만 유효하며 선택사항입니다. 네임스페이스를 지정하지 않으면 컨테이너의 모든 네임스페이스가 이 구성을 사용합니다.</dd>
  <dt><code>--hostname <em>LOG_SERVER_HOSTNAME</em></code></dt>
    <dd>로깅 유형이 <code>syslog</code>인 경우 로그 콜렉터 서버의 호스트 이름 또는 IP 주소입니다. 이 값은 <code>syslog</code>에 필수입니다. 로깅 유형이 <code>ibm</code>인 경우 {{site.data.keyword.loganalysislong_notm}} 유입 URL입니다. 사용 가능한 유입 URL 목록은 [여기](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls)에서 찾을 수 있습니다. 유입 URL을 지정하지 않으면 클러스터가 작성된 지역의 엔드포인트가 사용됩니다.</dd>
  <dt><code>--port <em>LOG_SERVER_PORT</em></code></dt>
    <dd>로그 콜렉터 서버의 포트입니다. 이 값은 선택사항입니다. 포트를 지정하지 않으면 표준 포트 <code>514</code>가 <code>syslog</code>에 사용되고 표준 포트 <code>9091</code>이 <code>ibm</code>에 사용됩니다.</dd>
  <dt><code>--space <em>CLUSTER_SPACE</em></code></dt>
    <dd>로그를 전송할 Cloud Foundry 영역의 이름입니다. 이 값은 <code>ibm</code> 로그 유형에 대해서만 유효하며 선택사항입니다. 영역을 지정하지 않으면 로그는 계정 레벨로 전송됩니다.</dd>
  <dt><code>--org <em>CLUSTER_ORG</em></code></dt>
    <dd>영역이 있는 Cloud Foundry 조직의 이름입니다. 이 값은 <code>ibm</code> 로그 유형에 대해서만 유효하며, 영역을 지정한 경우 필수입니다.</dd>
  <dt><code>--app-paths</code></dt>
    <dd>앱이 로그를 기록하는 컨테이너 상의 경로입니다. 소스 유형이 <code>application</code>인 로그를 전달하려면 경로를 제공해야 합니다. 두 개 이상의 경로를 지정하려면 쉼표로 구분된 목록을 사용하십시오. 이 값은 <code>application</code> 로그 소스의 경우 필수입니다. 예: <code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></dd>
  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>로그를 전달할 위치입니다. 옵션은 <code>ibm</code>(로그를 {{site.data.keyword.loganalysisshort_notm}}로 전달) 및 <code>syslog</code>(로그를 외부 서버로 전달)입니다.</dd>
  <dt><code>--app-containers</code></dt>
    <dd>선택사항: 앱에서 로그를 전달하기 위해 앱이 포함된 컨테이너의 이름을 지정할 수 있습니다. 쉼표로 구분된 목록을 사용하여 두 개 이상의 컨테이너를 지정할 수 있습니다. 컨테이너가 지정되지 않은 경우 로그는 사용자가 제공한 경로가 포함된 모든 컨테이너에서 전달됩니다. 이 옵션은 <code>application</code> 로그 소스에 대해서만 유효합니다.</dt>
  <dt><code>--json</code></dt>
    <dd>명령 출력을 JSON 형식으로 인쇄합니다. 이 값은 선택사항입니다.</dd>
  <dt><code>--skip-validation</code></dt>
    <dd>조직 및 영역 이름을 지정할 때 이러한 항목의 유효성 검증을 건너뜁니다. 유효성 검증을 건너뛰면 처리 시간이 줄어들지만 올바르지 않은 로깅 구성은 로그를 올바르게 전달하지 않습니다. 이 값은 선택사항입니다.</dd>
</dl>

**예제**:

기본 포트 9091에서 `container` 로그 소스로부터 전달하는 로그 유형 `ibm`의 예:

  ```
  bx cs logging-config-create my_cluster --logsource container --namespace my_namespace --hostname ingest.logging.ng.bluemix.net --type ibm
  ```
  {: pre}

기본 포트 514에서 `container` 로그 소스로부터 전달하는 로그 유형 `syslog`의 예:

  ```
  bx cs logging-config-create my_cluster --logsource container --namespace my_namespace  --hostname 169.xx.xxx.xxx --type syslog
  ```
  {: pre}

기본 포트가 아닌 다른 포트에서 `ingress` 소스로부터 로그를 전달하는 로그 유형 `syslog`의 예:

  ```
  bx cs logging-config-create my_cluster --logsource container --hostname 169.xx.xxx.xxx --port 5514 --type syslog
  ```
  {: pre}

### bx cs logging-config-get CLUSTER [--logsource LOG_SOURCE][--json]
{: #cs_logging_get}

클러스터에 대한 모든 로그 전달 구성을 보거나 로그 소스를 기반으로 로깅 구성을 필터링합니다.

<strong>명령 옵션</strong>:

 <dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>
  <dt><code>--logsource <em>LOG_SOURCE</em></code></dt>
    <dd>필터링하려는 로그 소스의 유형입니다. 클러스터에 있는 이 로그 소스의 로깅 구성만 리턴됩니다. 허용되는 값은 <code>container</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code> 및 <code>ingress</code>입니다. 이 값은 선택사항입니다.</dd>
  <dt><code>--json</code></dt>
    <dd>선택적으로 명령 출력을 JSON 형식으로 인쇄합니다.</dd>
 </dl>

**예제**:

  ```
  bx cs logging-config-get my_cluster --logsource worker
  ```
  {: pre}


### bx cs logging-config-refresh CLUSTER
{: #cs_logging_refresh}

클러스터의 로깅 구성을 새로 고칩니다. 이렇게 하면 클러스터의 영역 레벨에 전달되는 모든 로깅 구성의 로깅 토큰이 새로 고쳐집니다.

<strong>명령 옵션</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>
</dl>

**예제**:

  ```
  bx cs logging-config-refresh my_cluster
  ```
  {: pre}


### bx cs logging-config-rm CLUSTER [--id LOG_CONFIG_ID][--all]
{: #cs_logging_rm}

클러스터에 대한 한 개의 로그 전달 구성을 삭제하거나 모든 로깅 구성을 삭제합니다. 이렇게 하면 원격 syslog 서버 또는 {{site.data.keyword.loganalysisshort_notm}}로의 로그 전달이 중지됩니다.

<strong>명령 옵션</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>
  <dt><code>--id <em>LOG_CONFIG_ID</em></code></dt>
   <dd>로깅 구성 ID입니다(단일 로깅 구성을 제거하려는 경우).</dd>
  <dt><code>--all</code></dt>
   <dd>클러스터에서 모든 로깅 구성을 제거하는 플래그입니다.</dd>
</dl>

**예제**:

  ```
  bx cs logging-config-rm my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e
  ```
  {: pre}


### bx cs logging-config-update CLUSTER --id LOG_CONFIG_ID [--namespace NAMESPACE][--hostname LOG_SERVER_HOSTNAME_OR_IP] [--port LOG_SERVER_PORT][--space CLUSTER_SPACE] [--org CLUSTER_ORG] --type LOG_TYPE [--json][--skipValidation]
{: #cs_logging_update}

로그 전달 구성의 세부사항을 업데이트합니다.

<strong>명령 옵션</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>
  <dt><code>--id <em>LOG_CONFIG_ID</em></code></dt>
   <dd>업데이트하려는 로깅 구성 ID입니다. 이 값은 필수입니다.</dd>
  <dt><code>--namespace <em>NAMESPACE</em></code>
    <dd>로그를 전달할 Kubernetes 네임스페이스입니다. <code>ibm-system</code> 및 <code>kube-system</code> Kubernetes 네임스페이스의 경우 로그 전달이 지원되지 않습니다. 이 값은 <code>container</code> 로그 소스에 대해서만 유효합니다. 네임스페이스를 지정하지 않으면 컨테이너의 모든 네임스페이스가 이 구성을 사용합니다.</dd>
  <dt><code>--hostname <em>LOG_SERVER_HOSTNAME</em></code></dt>
   <dd>로깅 유형이 <code>syslog</code>인 경우 로그 콜렉터 서버의 호스트 이름 또는 IP 주소입니다. 이 값은 <code>syslog</code>에 필수입니다. 로깅 유형이 <code>ibm</code>인 경우 {{site.data.keyword.loganalysislong_notm}} 유입 URL입니다. 사용 가능한 유입 URL 목록은 [여기](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls)에서 찾을 수 있습니다. 유입 URL을 지정하지 않으면 클러스터가 작성된 지역의 엔드포인트가 사용됩니다.</dd>
   <dt><code>--port <em>LOG_SERVER_PORT</em></code></dt>
   <dd>로그 콜렉터 서버의 포트입니다. 로깅 유형이 <code>syslog</code>인 경우 이 값은 선택사항입니다. 포트를 지정하지 않으면 표준 포트 <code>514</code>가 <code>syslog</code>에 사용되고 <code>9091</code>이 <code>ibm</code>에 사용됩니다.</dd>
   <dt><code>--space <em>CLUSTER_SPACE</em></code></dt>
   <dd>로그를 전송할 영역의 이름입니다. 이 값은 <code>ibm</code> 로그 유형에 대해서만 유효하며 선택사항입니다. 영역을 지정하지 않으면 로그는 계정 레벨로 전송됩니다.</dd>
   <dt><code>--org <em>CLUSTER_ORG</em></code></dt>
   <dd>영역이 있는 조직의 이름입니다. 이 값은 <code>ibm</code> 로그 유형에 대해서만 유효하며, 영역을 지정한 경우 필수입니다.</dd>
   <dt><code>--app-paths</code></dt>
     <dd>조직 및 영역 이름을 지정할 때 이러한 항목의 유효성 검증을 건너뜁니다. 유효성 검증을 건너뛰면 처리 시간이 줄어들지만 올바르지 않은 로깅 구성은 로그를 올바르게 전달하지 않습니다. 이 값은 선택사항입니다.</dd>
   <dt><code>--app-containers</code></dt>
     <dd>앱이 로깅되는 컨테이너의 경로입니다. 소스 유형이 <code>application</code>인 로그를 전달하려면 경로를 제공해야 합니다. 두 개 이상의 경로를 지정하려면 쉼표로 구분된 목록을 사용하십시오. 예: <code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></dd>
   <dt><code>--type <em>LOG_TYPE</em></code></dt>
   <dd>사용하려는 로그 전달 프로토콜입니다. 현재 <code>syslog</code> 및 <code>ibm</code>이 지원됩니다. 이 값은 필수입니다.</dd>
   <dt><code>--json</code></dt>
   <dd>선택적으로 명령 출력을 JSON 형식으로 인쇄합니다.</dd>
   <dt><code>--skipValidation</code></dt>
   <dd>조직 및 영역 이름을 지정할 때 이러한 항목의 유효성 검증을 건너뜁니다. 유효성 검증을 건너뛰면 처리 시간이 줄어들지만 올바르지 않은 로깅 구성은 로그를 올바르게 전달하지 않습니다. 이 값은 선택사항입니다.</dd>
   </dl>

**로그 유형 `ibm`**에 대한 예제:

  ```
  bx cs logging-config-update my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e --type ibm
  ```
  {: pre}

**로그 유형 `syslog`**에 대한 예제:

  ```
  bx cs logging-config-update my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e --hostname localhost --port 5514 --type syslog
  ```
  {: pre}


### bx cs logging-filter-create CLUSTER --type LOG_TYPE [--logging-configs CONFIGS][--namespace KUBERNETES_NAMESPACE] [--container CONTAINER_NAME][--level LOGGING_LEVEL] [--message MESSAGE][--s] [--json]
{: #cs_log_filter_create}

로깅 필터를 작성합니다. 이 명령을 사용하여 로깅 구성에 의해 전달되는 로그를 필터링할 수 있습니다.

<strong>명령 옵션</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>필수: 로깅 필터를 작성할 클러스터의 이름 또는 ID입니다.</dd>
  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>필터를 적용할 로그의 유형입니다. 현재는 <code>all</code>, <code>container</code> 및 <code>host</code>가 지원됩니다.</dd>
  <dt><code>--logging-configs <em>CONFIGS</em></code></dt>
    <dd>선택사항: 로깅 구성 ID의 쉼표로 구분된 목록입니다. 제공되지 않으면 필터에 전달된 모든 클러스터 로깅 구성에 필터가 적용됩니다. 명령과 함께 <code>--show-matching-configs</code> 플래그를 사용하여 필터와 일치하는 로그 구성을 볼 수 있습니다.</dd>
  <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
    <dd>선택사항: 로그를 필터링할 Kubernetes 네임스페이스입니다.</dd>
  <dt><code>--container <em>CONTAINER_NAME</em></code></dt>
    <dd>선택사항: 로그를 필터링할 컨테이너의 이름입니다. 이 플래그는 로그 유형 <code>container</code>를 사용하는 경우에만 적용됩니다.</dd>
  <dt><code>--level <em>LOGGING_LEVEL</em></code></dt>
    <dd>선택사항: 지정된 레벨 이하의 로그를 필터링합니다. 허용 가능한 값은 규범적 순서대로 <code>fatal</code>, <code>error</code>, <code>warn/warning</code>, <code>info</code>, <code>debug</code> 및 <code>trace</code>입니다. 예를 들어, <code>info</code> 레벨에서 로그를 필터링한 경우에는 <code>debug</code> 및 <code>trace</code> 또한 필터링됩니다. **참고**: 로그 메시지가 JSON 형식이며 level 필드를 포함하는 경우에만 이 플래그를 사용할 수 있습니다. 출력 예: <code>{"log": "hello", "level": "info"}</code></dd>
  <dt><code>--message <em>MESSAGE</em></code></dt>
    <dd>선택사항: 지정된 메시지를 포함하는 로그를 필터링합니다. 이 메시지는 표현식으로 비교되지 않고 글자 그대로 비교됩니다. 예: 메시지 “Hello”, “!” 및 “Hello, World!”는 로그 “Hello, World!”에 적용됩니다.</dd>
  <dt><code>--json</code></dt>
    <dd>선택사항: 명령 출력을 JSON 형식으로 인쇄합니다.</dd>
</dl>

**예제**:

이 예는 기본 네임스페이스에 있는 `test-container`라는 이름의 컨테이너에서 전달되는, debug 레벨 이하이며 "GET request"를 포함하는 로그 메시지가 있는 모든 로그를 필터링합니다.

  ```
  bx cs logging-filter-create example-cluster --type container --namespace default --container test-container --level debug --message "GET request"
  ```
  {: pre}

이 예는 특정 클러스터에서 전달되는, info 레벨 이하의 모든 로그를 필터링합니다. 출력은 JSON으로 리턴됩니다.

  ```
  bx cs logging-filter-create example-cluster --type all --level info --json
  ```
  {: pre}

### bx cs logging-filter-update CLUSTER --type LOG_TYPE [--logging-configs CONFIGS][--namespace KUBERNETES_NAMESPACE] [--container CONTAINER_NAME][--level LOGGING_LEVEL] [--message MESSAGE][--s] [--json]
{: #cs_log_filter_update}

로깅 필터를 업데이트합니다. 이 명령을 사용하여 작성한 로깅 필터를 업데이트할 수 있습니다.

<strong>명령 옵션</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>필수: 로깅 필터를 업데이트할 클러스터의 이름 또는 ID입니다.</dd>
  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>필터를 적용할 로그의 유형입니다. 현재는 <code>all</code>, <code>container</code> 및 <code>host</code>가 지원됩니다.</dd>
  <dt><code>--logging-configs <em>CONFIGS</em></code></dt>
    <dd>선택사항: 로깅 구성 ID의 쉼표로 구분된 목록입니다. 제공되지 않으면 필터에 전달된 모든 클러스터 로깅 구성에 필터가 적용됩니다. 명령과 함께 <code>--show-matching-configs</code> 플래그를 사용하여 필터와 일치하는 로그 구성을 볼 수 있습니다.</dd>
  <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
    <dd>선택사항: 로그를 필터링할 Kubernetes 네임스페이스입니다.</dd>
  <dt><code>--container <em>CONTAINER_NAME</em></code></dt>
    <dd>선택사항: 로그를 필터링할 컨테이너의 이름입니다. 이 플래그는 로그 유형 <code>container</code>를 사용하는 경우에만 적용됩니다.</dd>
  <dt><code>--level <em>LOGGING_LEVEL</em></code></dt>
    <dd>선택사항: 지정된 레벨 이하의 로그를 필터링합니다. 허용 가능한 값은 규범적 순서대로 <code>fatal</code>, <code>error</code>, <code>warn/warning</code>, <code>info</code>, <code>debug</code> 및 <code>trace</code>입니다. 예를 들어, <code>info</code> 레벨에서 로그를 필터링한 경우에는 <code>debug</code> 및 <code>trace</code> 또한 필터링됩니다. **참고**: 로그 메시지가 JSON 형식이며 level 필드를 포함하는 경우에만 이 플래그를 사용할 수 있습니다. 출력 예: <code>{"log": "hello", "level": "info"}</code></dd>
  <dt><code>--message <em>MESSAGE</em></code></dt>
    <dd>선택사항: 지정된 메시지를 포함하는 로그를 필터링합니다. 이 메시지는 표현식으로 비교되지 않고 글자 그대로 비교됩니다. 예: 메시지 “Hello”, “!” 및 “Hello, World!”는 로그 “Hello, World!”에 적용됩니다.</dd>
  <dt><code>--json</code></dt>
    <dd>선택사항: 명령 출력을 JSON 형식으로 인쇄합니다.</dd>
</dl>


### bx cs logging-filter-get CLUSTER [--id FILTER_ID][--show-matching-configs] [--json]
{: #cs_log_filter_view}

로깅 필터 구성을 봅니다. 이 명령을 사용하여 작성한 로깅 필터를 볼 수 있습니다.

<strong>명령 옵션</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>필수: 필터를 볼 클러스터의 이름 또는 ID입니다.</dd>
  <dt><code>--id <em>FILTER_ID</em></code></dt>
    <dd>보려는 로그 필터의 ID입니다.</dd>
  <dt><code>--show-matching-configs</code></dt>
    <dd>선택사항: 보고 있는 구성과 일치하는 로깅 구성을 표시합니다.</dd>
  <dt><code>--json</code></dt>
    <dd>선택사항: 명령 출력을 JSON 형식으로 인쇄합니다.</dd>
</dl>


### bx cs logging-filter-rm CLUSTER [--id FILTER_ID][--json] [--all]
{: #cs_log_filter_delete}

로깅 필터를 삭제합니다. 이 명령을 사용하여 작성한 로깅 필터를 제거할 수 있습니다.

<strong>명령 옵션</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>필터를 삭제할 클러스터의 이름 또는 ID입니다.</dd>
  <dt><code>--id <em>FILTER_ID</em></code></dt>
    <dd>삭제할 로그 필터의 ID입니다.</dd>
  <dt><code>--all</code></dt>
    <dd>선택사항: 로그 전달 필터를 모두 삭제합니다.</dd>
  <dt><code>--json</code></dt>
    <dd>선택사항: 명령 출력을 JSON 형식으로 인쇄합니다.</dd>
</dl>

<br />


## 지역 명령
{: #region_commands}

###   bx cs locations
{: #cs_datacenters}

사용자가 클러스터를 작성하기 위해 사용할 수 있는 위치의 목록을 봅니다.

<strong>명령 옵션</strong>:

   없음

**예제**:

  ```
  bx cs locations
  ```
  {: pre}


### bx cs region
{: #cs_region}

사용자가 현재 있는 {{site.data.keyword.containershort_notm}} 지역을 찾습니다. 사용자는 영역에 특정한 클러스터를 작성하고 관리합니다. 지역을 변경하려면 `bx cs region-set` 명령을 사용하십시오.

**예제**:

```
bx cs region
```
{: pre}

**출력**:
```
Region: us-south
```
{: screen}

### bx cs region-set [REGION]
{: #cs_region-set}

{{site.data.keyword.containershort_notm}}의 지역을 설정합니다. 사용자는 지역에 특정한 클러스터를 작성 및 관리하며, 고가용성을 위해 여러 지역에서 클러스터를 원할 수 있습니다.

예를 들어, 미국 남부 지역에서 {{site.data.keyword.Bluemix_notm}}에 로그인하고 클러스터를 작성할 수 있습니다. 그런 다음, EU 중심부를 대상으로 하기 위해 `bx cs region-set eu-central`을 사용하고 다른 클러스터를 작성할 수 있습니다. 마지막으로, `bx cs region-set us-south`를 사용하여 미국 남부로 돌아가서 해당 지역의 클러스터를 관리할 수 있습니다.

**명령 옵션**:

<dl>
<dt><code><em>REGION</em></code></dt>
<dd>대상으로 할 지역을 입력하십시오. 이 값은 선택사항입니다. 지역을 제공하지 않은 경우 출력의 목록에서 선택할 수 있습니다.

사용 가능한 지역의 목록을 보려면 [지역 및 위치](cs_regions.html)를 검토하거나 `bx cs regions` [명령](#cs_regions)을 사용하십시오.</dd></dl>

**예제**:

```
bx cs region-set eu-central
```
{: pre}

```
bx cs region-set
```
{: pre}

**출력**:
```
Choose a region:
1. ap-north
2. ap-south
3. eu-central
4. uk-south
5. us-east
6. us-south
Enter a number> 3
OK
```
{: screen}

### bx cs regions
{: #cs_regions}

사용 가능한 지역을 나열합니다. `Region Name`은 {{site.data.keyword.containershort_notm}} 이름이고 `Region Alias`는 해당 지역의 일반 {{site.data.keyword.Bluemix_notm}} 이름입니다.

**예제**:

```
bx cs regions
```
{: pre}

**출력**:
```
Region Name   Region Alias
ap-north      jp-tok
ap-south      au-syd
eu-central    eu-de
uk-south      eu-gb
us-east       us-east
us-south      us-south
```
{: screen}


<br />


## 작업자 노드 명령
{: worker_node_commands}



### bx cs worker-add --cluster CLUSTER [--file FILE_LOCATION][--hardware HARDWARE] --machine-type MACHINE_TYPE --number NUMBER --private-vlan PRIVATE_VLAN --public-vlan PUBLIC_VLAN [--disable-disk-encrypt]
{: #cs_worker_add}

표준 클러스터에 작업자 노드를 추가합니다.

<strong>명령 옵션</strong>:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

<dt><code>--file <em>FILE_LOCATION</em></code></dt>
<dd>클러스터에 작업자 노드를 추가하기 위한 YAML 파일의 경로입니다. 이 명령에 제공된 옵션을 사용하여 추가 작업자 노드를 정의하는 대신 YAML 파일을 사용할 수 있습니다. 이 값은 선택사항입니다.

<p><strong>참고:</strong> 이 명령에 YAML 파일의 매개변수와 동일한 옵션을 제공하는 경우 명령의 값이 YAML의 값보다 우선합니다. 예를 들어, YAML 파일에 머신 유형을 정의하고 명령에서 --machine-type 옵션을 사용하십시오. 명령 옵션에 입력한 값이 YAML 파일의 값을 대체합니다.

<pre class="codeblock">
<code>name: <em>&lt;cluster_name_or_ID&gt;</em>
location: <em>&lt;location&gt;</em>
machine-type: <em>&lt;machine_type&gt;</em>
private-vlan: <em>&lt;private_VLAN&gt;</em>
public-vlan: <em>&lt;public_VLAN&gt;</em>
hardware: <em>&lt;shared_or_dedicated&gt;</em>
workerNum: <em>&lt;number_workers&gt;</em>
diskEncryption: <em>false</em></code></pre>

<table>
<caption>표 2. YAML 파일 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code><em>name</em></code></td>
<td><code><em>&lt;cluster_name_or_ID&gt;</em></code>를 작업자 노드를 추가할 클러스터의 이름 또는 ID로 대체합니다.</td>
</tr>
<tr>
<td><code><em>location</em></code></td>
<td><code><em>&lt;location&gt;</em></code>을 작업자 노드를 배치할 위치로 대체합니다. 사용 가능한 위치는 사용자가 로그인한 지역에 따라 다릅니다. 사용 가능한 위치를 나열하려면 <code>bx cs locations</code>를 실행하십시오.</td>
</tr>
<tr>
<td><code><em>machine-type</em></code></td>
<td><code><em>&lt;machine_type&gt;</em></code>을 작업자 노드를 배치하려는 머신 유형으로 대체합니다. 공유 또는 전용 하드웨어에서 가상 머신으로서 또는 베어메탈에서 실제 머신으로서 작업자 노드를 배치할 수 있습니다. 사용 가능한 실제 및 가상 머신 유형은 클러스터를 배치하는 위치에 따라 다릅니다. 자세한 정보는 `bx cs machine-types` [명령](cs_cli_reference.html#cs_machine_types)을 참조하십시오.</td>
</tr>
<tr>
<td><code><em>private-vlan</em></code></td>
<td><code><em>&lt;private_VLAN&gt;</em></code>을 작업자 노드에 사용할 사설 VLAN의 ID로 대체합니다. 사용 가능한 VLAN을 나열하려면 <code>bx cs vlans <em>&lt;location&gt;</em></code>을 실행하고 <code>bcr</code>(백엔드 라우터)로 시작하는 VLAN 라우터를 찾으십시오.</td>
</tr>
<tr>
<td><code>public-vlan</code></td>
<td><code>&lt;public_VLAN&gt;</code>을 작업자 노드에 사용할 공인 VLAN의 ID로 대체합니다. 사용 가능한 VLAN을 나열하려면 <code>bx cs vlans &lt;location&gt;</code>을 실행하고 <code>fcr</code>(프론트 엔드 라우터)로 시작하는 VLAN 라우터를 찾으십시오. <br><strong>참고</strong>: {[private_VLAN_vyatta]}</td>
</tr>
<tr>
<td><code>hardware</code></td>
<td>가상 머신 유형의 경우: 작업자 노드에 대한 하드웨어 격리의 레벨입니다. 사용자 전용으로만 실제 리소스를 사용 가능하게 하려면 dedicated를 사용하고, 실제 리소스를 다른 IBM 고객과 공유하도록 허용하려면 shared를 사용하십시오. 기본값은 shared입니다.</td>
</tr>
<tr>
<td><code>workerNum</code></td>
<td><code><em>&lt;number_workers&gt;</em></code>를 배치할 작업자 노드 수로 대체합니다.</td>
</tr>
<tr>
<td><code>diskEncryption: <em>false</em></code></td>
<td>작업자 노드는 기본적으로 디스크 암호화 기능을 합니다. [자세히 보기](cs_secure.html#worker). 암호화를 사용 안함으로 설정하려면 이 옵션을 포함하고 값을 <code>false</code>로 설정하십시오.</td></tr>
</tbody></table></p></dd>

<dt><code>--hardware <em>HARDWARE</em></code></dt>
<dd>작업자 노드에 대한 하드웨어 격리의 레벨입니다. 사용자 전용으로만 실제 리소스를 사용 가능하게 하려면 dedicated를 사용하고, 실제 리소스를 다른 IBM 고객과 공유하도록 허용하려면 shared를 사용하십시오. 기본값은 shared입니다. 이 값은 선택사항입니다.</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>머신 유형을 선택합니다. 공유 또는 전용 하드웨어에서 가상 머신으로서 또는 베어메탈에서 실제 머신으로서 작업자 노드를 배치할 수 있습니다. 사용 가능한 실제 및 가상 머신 유형은 클러스터를 배치하는 위치에 따라 다릅니다. 자세한 정보는 `bx cs machine-types` [명령](cs_cli_reference.html#cs_machine_types)에 대한 문서를 참조하십시오. 이 값은 표준 클러스터의 경우 필수이며 무료 클러스터에는 사용할 수 없습니다.</dd>

<dt><code>--number <em>NUMBER</em></code></dt>
<dd>클러스터에서 작성할 작업자 노드의 수를 표시하는 정수입니다. 기본값은 1입니다. 이 값은 선택사항입니다.</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>클러스터가 작성될 때 지정된 사설 VLAN입니다. 이 값은 필수입니다.

<p><strong>참고:</strong> 사설 VLAN 라우터는 항상 <code>bcr</code>(벡엔드 라우터)로 시작하고 공용 VLAN 라우터는 항상 <code>fcr</code>(프론트 엔드 라우터)로 시작합니다. 클러스터를 작성하고 공인 및 사설 VLAN을 지정할 때는 이러한 접두부 뒤의 숫자 및 문자 조합이 일치해야 합니다.</p></dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>클러스터가 작성될 때 지정된 공용 VLAN입니다. 이 값은 선택사항입니다. 작업자 노드가 사설 VLAN에만 존재하도록 하려는 경우 공용 VLAN ID를 제공하지 마십시오. <strong>참고</strong>: {[private_VLAN_vyatta]}

<p><strong>참고:</strong> 사설 VLAN 라우터는 항상 <code>bcr</code>(벡엔드 라우터)로 시작하고 공용 VLAN 라우터는 항상 <code>fcr</code>(프론트 엔드 라우터)로 시작합니다. 클러스터를 작성하고 공인 및 사설 VLAN을 지정할 때는 이러한 접두부 뒤의 숫자 및 문자 조합이 일치해야 합니다.</p></dd>

<dt><code>--disable-disk-encrypt</code></dt>
<dd>작업자 노드는 기본적으로 디스크 암호화 기능을 합니다. [자세히 보기](cs_secure.html#worker). 암호화를 사용 안함으로 설정하려면 이 옵션을 포함하십시오.</dd>
</dl>

**예제**:

  ```
  bx cs worker-add --cluster my_cluster --number 3 --public-vlan my_public_VLAN_ID --private-vlan my_private_VLAN_ID --machine-type u2c.2x4 --hardware shared
  ```
  {: pre}

  {{site.data.keyword.Bluemix_dedicated_notm}}의 예:

  ```
  bx cs worker-add --cluster my_cluster --number 3 --machine-type u2c.2x4
  ```
  {: pre}




### bx cs worker-get [CLUSTER_NAME_OR_ID] WORKER_NODE_ID
{: #cs_worker_get}

작업자 노드의 세부사항을 봅니다.

<strong>명령 옵션</strong>:

   <dl>
   <dt><code><em>CLUSTER_NAME_OR_ID</em></code></dt>
   <dd>작업자 노드 클러스터의 이름 또는 ID입니다. 이 값은 선택사항입니다.</dd>
   <dt><code><em>WORKER_NODE_ID</em></code></dt>
   <dd>작업자 노드의 이름입니다. 클러스터로 작업자 노드를 위한 ID를 보려면 <code>bx cs workers <em>CLUSTER</em></code>를 실행하십시오. 이 값은 필수입니다.</dd>
   </dl>

**명령 예**:

  ```
  bx cs worker-get my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1
  ```
  {: pre}

**출력 예**:

  ```
  ID:           kube-dal10-123456789-w1
  State:        normal
  Status:       Ready
  Trust:        disabled
  Private VLAN: 223xxxx
  Public VLAN:  223xxxx
  Private IP:   10.xxx.xx.xxx
  Public IP:    169.xx.xxx.xxx
  Hardware:     shared
  Zone:         dal10
  Version:      1.8.11_1509
  ```
  {: screen}

### bx cs worker-reboot [-f][--hard] CLUSTER WORKER [WORKER]
{: #cs_worker_reboot}

클러스터에서 작업자 노드를 다시 부팅합니다. 다시 부팅 중에 작업자 노드의 상태가 변경되지 않습니다.

**주의:** 작업자 노드를 다시 부팅하면 작업자 노드에서 데이터 손상이 발생할 수 있습니다. 이 명령은 다시 부팅하면 작업자 노드를 복구하는 데 도움이 된다고 알고 있는 경우에 주의하여 사용하십시오. 다른 모든 경우에는 대신 [작업자 노드를 다시 로드](#cs_worker_reload)하십시오.

작업자 노드를 다시 부팅하기 전에 다른 작업자 노드에서 팟(Pod)을 다시 스케줄하여 앱의 작동 중단 또는 작업자 노드의 데이터 손상을 방지할 수 있는지 확인하십시오.

1. 클러스터의 모든 작업자 노드를 나열하고 다시 부팅할 작업자 노드의 **이름**을 기록해 두십시오.
   ```
kubectl get nodes
   ```
   이 명령에서 리턴되는 **이름**은 작업자 노드에 지정된 사설 IP 주소입니다. `bx cs workers <cluster_name_or_ID>` 명령을 실행하고 동일한 **사설 IP** 주소로 작업자 노드를 검색할 때 작업자 노드에 대한 자세한 정보를 찾을 수 있습니다.
2. 유출(cordoning)이라고 알려진 프로세스에서 작업자 노드를 스케줄 불가능으로 표시하십시오. 작업자 노드를 유출할 때 이후 팟(Pod) 스케줄링에서 사용할 수 없도록 합니다. 이전 단계에서 검색한 작업자 노드의 **이름**을 사용하십시오.
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. 팟(Pod) 스케줄링이 작업자 노드에 사용 불가능한지 확인하십시오.
   ```
kubectl get nodes
   ```
   {: pre}
   상태가 **SchedulingDisabled**로 표시되는 경우 작업자 노드가 팟(Pod) 스케줄링에 사용 불가능합니다.
 4. 작업자 노드에서 팟(Pod)을 제거하고 클러스터에 남아 있는 작업자 노드로 다시 스케줄하도록 강제 실행하십시오.
    ```
    kubectl drain <worker_name>
    ```
    {: pre}
이 프로세스는 몇 분 정도 소요됩니다.
 5. 작업자 노드를 다시 부팅하십시오. `bx cs workers <cluster_name_or_ID>` 명령에서 리턴된 작업자 ID를 사용하십시오.
    ```
    bx cs worker-reboot <cluster_name_or_ID> <worker_name_or_ID>
    ```
    {: pre}
 6. 다시 부팅이 완료되었는지 확인하려면 작업자 노드를 팟(Pod) 스케줄링에서 사용할 수 있도록 하기 전에 약 5분 간 기다리십시오. 다시 부팅 중에 작업자 노드의 상태가 변경되지 않습니다. 일반적으로 작업자 노드의 다시 부팅은 몇 초 후에 완료됩니다.
 7. 작업자 노드를 팟(Pod) 스케줄링에서 사용할 수 있도록 하십시오. `kubectl get nodes` 명령에서 리턴되는 작업자 노드의 **이름**을 사용하십시오.
    ```
    kubectl uncordon <worker_name>
    ```
    {: pre}
    </br>

<strong>명령 옵션</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>-f</code></dt>
   <dd>사용자 프롬프트를 표시하지 않고 작업자 노드의 다시 시작을 강제 실행하려면 이 옵션을 사용하십시오. 이 값은 선택사항입니다.</dd>

   <dt><code>--hard</code></dt>
   <dd>작업자 노드의 전원을 끊어서 작업자 노드의 하드 다시 시작을 강제 실행하려면 이 옵션을 사용하십시오. 작업자 노드의 반응이 늦거나 작업자 노드에 Docker
정지가 된 경우 이 옵션을 사용하십시오. 이 값은 선택사항입니다.</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>하나 이상의 작업자 노드의 이름 또는 ID입니다. 여러 작업자 노드를 나열하려면 공백을 사용하십시오. 이 값은 필수입니다.</dd>
   </dl>

**예제**:

  ```
  bx cs worker-reboot my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}


### bx cs worker-reload [-f] CLUSTER WORKER [WORKER]
{: #cs_worker_reload}

작업자 노드에 필요한 모든 구성을 다시 로드하십시오. 다시 로드는 작업자 노드에 성능 저하와 같은 문제점이 발생하거나 작업자 노드가 비정상적인 상태인 경우 유용할 수 있습니다.

작업자 노드를 다시 로드하면 작업자 노드에 패치 버전 업데이트가 적용되지만 주 버전 또는 부 버전 업데이트는 적용되지 않습니다. 한 패치 버전에서 다음 패치 버전으로의 변경사항을 보려면 [버전 변경 로그](cs_versions_changelog.html#changelog) 문서를 검토하십시오.
{: tip}

작업자 노드를 다시 로드하기 전에 다른 작업자 노드에서 팟(Pod)을 다시 스케줄하여 앱의 작동 중단 또는 작업자 노드의 데이터 손상을 방지할 수 있는지 확인하십시오.

1. 클러스터의 모든 작업자 노드를 나열하고 다시 로드할 작업자 노드의 **이름**을 기록해 두십시오.
   ```
kubectl get nodes
   ```
   이 명령에서 리턴되는 **이름**은 작업자 노드에 지정된 사설 IP 주소입니다. `bx cs workers <cluster_name_or_ID>` 명령을 실행하고 동일한 **사설 IP** 주소로 작업자 노드를 검색할 때 작업자 노드에 대한 자세한 정보를 찾을 수 있습니다.
2. 유출(cordoning)이라고 알려진 프로세스에서 작업자 노드를 스케줄 불가능으로 표시하십시오. 작업자 노드를 유출할 때 이후 팟(Pod) 스케줄링에서 사용할 수 없도록 합니다. 이전 단계에서 검색한 작업자 노드의 **이름**을 사용하십시오.
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. 팟(Pod) 스케줄링이 작업자 노드에 사용 불가능한지 확인하십시오.
   ```
kubectl get nodes
   ```
   {: pre}
   상태가 **SchedulingDisabled**로 표시되는 경우 작업자 노드가 팟(Pod) 스케줄링에 사용 불가능합니다.
 4. 작업자 노드에서 팟(Pod)을 제거하고 클러스터에 남아 있는 작업자 노드로 다시 스케줄하도록 강제 실행하십시오.
    ```
    kubectl drain <worker_name>
    ```
    {: pre}
이 프로세스는 몇 분 정도 소요됩니다.
 5. 작업자 노드를 다시 로드하십시오. `bx cs workers <cluster_name_or_ID>` 명령에서 리턴된 작업자 ID를 사용하십시오.
    ```
    bx cs worker-reload <cluster_name_or_ID> <worker_name_or_ID>
    ```
    {: pre}
 6. 다시 로드가 완료될 때까지 기다리십시오.
 7. 작업자 노드를 팟(Pod) 스케줄링에서 사용할 수 있도록 하십시오. `kubectl get nodes` 명령에서 리턴되는 작업자 노드의 **이름**을 사용하십시오.
    ```
    kubectl uncordon <worker_name>
    ```
</br>
<strong>명령 옵션</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>-f</code></dt>
   <dd>사용자 프롬프트를 표시하지 않고 작업자 노드의 다시 로드를 강제 실행하려면 이 옵션을 사용하십시오. 이 값은 선택사항입니다.</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>하나 이상의 작업자 노드의 이름 또는 ID입니다. 여러 작업자 노드를 나열하려면 공백을 사용하십시오. 이 값은 필수입니다.</dd>
   </dl>

**예제**:

  ```
  bx cs worker-reload my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}


### bx cs worker-rm [-f] CLUSTER WORKER [WORKER]
{: #cs_worker_rm}

클러스터에서 하나 이상의 작업자 노드를 제거합니다. 작업자 노드를 제거하면 클러스터가 불균형 상태가 됩니다. 

작업자 노드를 제거하기 전에 다른 작업자 노드에서 팟(Pod)을 다시 스케줄하여 앱의 작동 중단 또는 작업자 노드의 데이터 손상을 방지할 수 있는지 확인하십시오.
{: tip}

1. 클러스터의 모든 작업자 노드를 나열하고 제거할 작업자 노드의 **이름**을 기록해 두십시오.
   ```
kubectl get nodes
   ```
   이 명령에서 리턴되는 **이름**은 작업자 노드에 지정된 사설 IP 주소입니다. `bx cs workers <cluster_name_or_ID>` 명령을 실행하고 동일한 **사설 IP** 주소로 작업자 노드를 검색할 때 작업자 노드에 대한 자세한 정보를 찾을 수 있습니다.
2. 유출(cordoning)이라고 알려진 프로세스에서 작업자 노드를 스케줄 불가능으로 표시하십시오. 작업자 노드를 유출할 때 이후 팟(Pod) 스케줄링에서 사용할 수 없도록 합니다. 이전 단계에서 검색한 작업자 노드의 **이름**을 사용하십시오.
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. 팟(Pod) 스케줄링이 작업자 노드에 사용 불가능한지 확인하십시오.
   ```
kubectl get nodes
   ```
   {: pre}
   상태가 **SchedulingDisabled**로 표시되는 경우 작업자 노드가 팟(Pod) 스케줄링에 사용 불가능합니다.
4. 작업자 노드에서 팟(Pod)을 제거하고 클러스터에 남아 있는 작업자 노드로 다시 스케줄하도록 강제 실행하십시오.
   ```
   kubectl drain <worker_name>
   ```
   {: pre}
이 프로세스는 몇 분 정도 소요됩니다.
5. 작업자 노드를 제거하십시오. `bx cs workers <cluster_name_or_ID>` 명령에서 리턴된 작업자 ID를 사용하십시오.
   ```
   bx cs worker-rm <cluster_name_or_ID> <worker_name_or_ID>
   ```
   {: pre}

6. 작업자 노드가 제거되었는지 확인하십시오.
   ```
   bx cs workers <cluster_name_or_ID>
   ```
</br>
<strong>명령 옵션</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>-f</code></dt>
   <dd>사용자 프롬프트를 표시하지 않고 작업자 노드의 제거를 강제 실행하려면 이 옵션을 사용하십시오. 이 값은 선택사항입니다.</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>하나 이상의 작업자 노드의 이름 또는 ID입니다. 여러 작업자 노드를 나열하려면 공백을 사용하십시오. 이 값은 필수입니다.</dd>
   </dl>

**예제**:

  ```
  bx cs worker-rm my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}




### bx cs worker-update [-f] CLUSTER WORKER [WORKER][--kube-version MAJOR.MINOR.PATCH] [--force-update]
{: #cs_worker_update}

작업자 노드를 업데이트하여 운영 체제에 최신 보안 업데이트 및 패치를 적용하고, 마스터 노드의 버전과 일치하도록 Kubernetes 버전을 업데이트합니다. 마스터 노드 Kubernetes 버전은 `bx cs cluster-update` [명령](cs_cli_reference.html#cs_cluster_update)을 사용하여 업데이트할 수 있습니다.

**중요**: `bx cs worker-update`를 실행하면 앱과 서비스의 가동이 중단될 수 있습니다. 업데이트 중에 모든 팟(Pod)이 다른 작업자 노드로 재스케줄되고 팟(Pod) 외부에 저장되지 않은 경우 데이터가 삭제됩니다. 가동 중단을 방지하려면 [선택한 작업자 노드가 업데이트되는 동안 워크로드를 처리하기에 충분한 작업자 노드가 있는지 확인](cs_cluster_update.html#worker_node)하십시오.

업데이트하기 전에 배치를 위해 YAML 파일을 변경해야 할 수도 있습니다. 세부사항은 이 [릴리스 정보](cs_versions.html)를 검토하십시오.

<strong>명령 옵션</strong>:

   <dl>

   <dt><em>CLUSTER</em></dt>
   <dd>사용 가능한 작업자 노드를 나열하는 클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt><code>-f</code></dt>
   <dd>사용자 프롬프트를 표시하지 않고 마스터 업데이트를 강제 실행하려면 이 옵션을 사용하십시오. 이 값은 선택사항입니다.</dd>

   <dt><code>--force-update</code></dt>
   <dd>변경 시 부 버전의 차이가 2보다 큰 경우에도 업데이트를 시도합니다. 이 값은 선택사항입니다.</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>하나 이상의 작업자 노드의 ID입니다. 여러 작업자 노드를 나열하려면 공백을 사용하십시오. 이 값은 필수입니다.</dd>
   </dl>

**예제**:

  ```
  bx cs worker-update my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}



### bx cs workers CLUSTER [--show-deleted]
{: #cs_workers}

작업자 노드의 목록과 클러스터에서 각각의 상태(status)를 봅니다.

<strong>명령 옵션</strong>:

   <dl>
   <dt><em>CLUSTER</em></dt>
   <dd>사용 가능한 작업자 노드에 대한 클러스터의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>
   <dt><em>--show-deleted</em></dt>
   <dd>삭제 이유를 포함, 클러스터에서 삭제된 작업자 노드를 봅니다. 이 값은 선택사항입니다.</dd>
   </dl>

**예제**:

  ```
  bx cs workers my_cluster
  ```
  {: pre}
