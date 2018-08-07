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




# 방화벽에서 필수 포트와 IP 주소 열기
{: #firewall}

{{site.data.keyword.containerlong}}를 위해 방화벽에서 특정 포트 및 IP 주소를 열어야 할 수 있는 다음 상황을 검토하십시오.
{:shortdesc}

* 회사 네트워크 정책으로 인해 프록시 또는 방화벽을 통해 공용 인터넷 엔드포인트에 액세스하지 못할 때 로컬 시스템에서 [`bx` 명령을 실행](#firewall_bx)하려는 경우
* 회사 네트워크 정책으로 인해 프록시 또는 방화벽을 통해 공용 인터넷 엔드포인트에 액세스하지 못할 때 로컬 시스템에서 [`kubectl` 명령을 실행](#firewall_kubectl)하려는 경우
* 회사 네트워크 정책으로 인해 프록시 또는 방화벽을 통해 공용 인터넷 엔드포인트에 액세스하지 못할 때 로컬 시스템에서 [`calicoctl` 명령을 실행](#firewall_calicoctl)하려는 경우
* 작업자 노드에 대한 방화벽이 설정되었거나 IBM Cloud 인프라(SoftLayer) 계정에서 방화벽 설정이 사용자 정의되었을 때 [Kubernetes 마스터와 작업자 노드 간의 통신을 허용](#firewall_outbound)하려는 경우
* [클러스터의 외부에서 NodePort 서비스, LoadBalancer 서비스 또는 Ingress에 액세스](#firewall_inbound)하려는 경우

<br />


## 방화벽 뒤에서 `bx cs` 명령 실행
{: #firewall_bx}

회사 네트워크 정책이 로컬 시스템에서 프록시 또는 방화벽을 통해 공용 엔드포인트에 액세스하지 못하도록 방지하는 경우 `bx cs` 명령을 실행하려면 {{site.data.keyword.containerlong_notm}}에 대한 TCP 액세스를 허용해야 합니다.
{:shortdesc}

1. 포트 443에서 `containers.bluemix.net`에 대한 액세스를 허용하십시오.
2. 연결을 확인하십시오. 액세스가 올바르게 구성되면 출력에 배가 표시됩니다.
   ```
   curl https://containers.bluemix.net/v1/
   ```
   {: pre}

   출력 예:
   ```
                                     )___(
                              _______/__/_
                     ___     /===========|   ___
    ____       __   [\\\]___/____________|__[///]   __
    \   \_____[\\]__/___________________________\__[//]___
     \                                                    |
      \                                                  /
   ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ```
   {: screen}

<br />


## 방화벽 뒤에서 `kubectl` 명령 실행
{: #firewall_kubectl}

회사 네트워크 정책이 로컬 시스템에서 프록시 또는 방화벽을 통해 공용 엔드포인트에 액세스하지 못하도록 방지하는 경우 `kubectl` 명령을 실행하려면 클러스터에 대한 TCP 액세스를 허용해야 합니다.
{:shortdesc}

클러스터가 작성될 때 마스터 URL의 포트는 20000 - 32767 내에서 무작위로 지정됩니다. 작성될 수 있는 모든 클러스터에 포트 범위 20000 - 32767을 공개할 수도 있고 기존의 특정 클러스터에만 액세스를 허용할 수도 있습니다.

시작하기 전에 [`bx cs` 명령 실행](#firewall_bx)에 대한 액세스를 허용하십시오.

특정 클러스터에 대한 액세스를 허용하려면 다음을 수행하십시오.

1. {{site.data.keyword.Bluemix_notm}} CLI에 로그인하십시오. 프롬프트가 표시되면
{{site.data.keyword.Bluemix_notm}} 신임 정보를 입력하십시오. 연합 계정이 있는 경우, `--sso` 옵션을 포함하십시오.

   ```
    bx login [--sso]
   ```
   {: pre}

2. 클러스터가 있는 지역을 선택하십시오.

   ```
   bx cs region-set
   ```
   {: pre}

3. 클러스터의 이름을 가져오십시오.

   ```
   bx cs clusters
   ```
   {: pre}

4. 클러스터의 **Master URL**을 검색하십시오.

   ```
   bx cs cluster-get <cluster_name_or_ID>
   ```
   {: pre}

   출력 예:
   ```
   ...
   Master URL:		https://169.xx.xxx.xxx:31142
   ...
   ```
   {: screen}

5. 포트(예: 이전 예제의 `31142` 포트)에서 **마스터 URL**에 대한 액세스를 허용하십시오.

6. 연결을 확인하십시오.

   ```
   curl --insecure <master_URL>/version
   ```
   {: pre}

   명령 예:
   ```
   curl --insecure https://169.xx.xxx.xxx:31142/version
   ```
   {: pre}

   출력 예:
   ```
   {
     "major": "1",
     "minor": "7+",
     "gitVersion": "v1.7.4-2+eb9172c211dc41",
     "gitCommit": "eb9172c211dc4108341c0fd5340ee5200f0ec534",
     "gitTreeState": "clean",
     "buildDate": "2017-11-16T08:13:08Z",
     "goVersion": "go1.8.3",
     "compiler": "gc",
     "platform": "linux/amd64"
   }
   ```
   {: screen}

7. 선택사항: 노출해야 하는 각 클러스터에 대해 이러한 단계를 반복하십시오.

<br />


## 방화벽 뒤에서 `calicoctl` 명령 실행
{: #firewall_calicoctl}

회사 네트워크 정책이 로컬 시스템에서 프록시 또는 방화벽을 통해 공용 엔드포인트에 액세스하지 못하도록 방지하는 경우 `calicoctl` 명령을 실행하려면 Calico 명령에 대한 TCP 액세스를 허용해야 합니다.
{:shortdesc}

시작하기 전에 [`bx` commands](#firewall_bx) 및 [`kubectl` 명령](#firewall_kubectl)을 실행할 수 있도록 액세스를 허용하십시오.

1. [`kubectl` 명령](#firewall_kubectl)을 허용하는 데 사용한 마스터 URL에서 IP 주소를 검색하십시오.

2. ETCD의 포트를 가져오십시오.

  ```
  kubectl get cm -n kube-system calico-config -o yaml | grep etcd_endpoints
  ```
  {: pre}

3. 마스터 URL IP 주소와 ETCD 포트를 통해 Calico 정책에 대한 액세스를 허용하십시오.

<br />


## 클러스터가 인프라 리소스 및 기타 서비스에 액세스하도록 허용
{: #firewall_outbound}

클러스터가 방화벽 뒤에서 인프라 리소스와 서비스(예: {{site.data.keyword.containershort_notm}} 지역, {{site.data.keyword.registrylong_notm}}, {{site.data.keyword.monitoringlong_notm}}, {{site.data.keyword.loganalysislong_notm}}, IBM Cloud 인프라(SoftLayer) 사설 IP 및 지속적 볼륨 클레임을 위한 egress)에 액세스할 수 있게 하십시오.
{:shortdesc}

  1.  클러스터의 모든 작업자 노드에 대한 공인 IP 주소를 기록해 두십시오.

      ```
      bx cs workers <cluster_name_or_ID>
      ```
      {: pre}

  2.  소스 _<each_worker_node_publicIP>_에서 대상 TCP/UDP 포트 범위 20000-32767 및 포트 443으로의 발신 네트워크 트래픽과 다음 IP 주소 및 네트워크 그룹을 허용하십시오. 로컬 머신이 공용 인터넷 엔드포인트에 액세스하지 못하도록 방지하는 회사 방화벽이 있는 경우, 소스 작업자 노드와 로컬 머신 둘 다에 대해 이 단계를 수행하십시오.
      - **중요**: 부트스트랩 프로세스 중에 로드를 밸런싱하려면 지역 내 모든 위치에 대해 포트 443으로의 발신 트래픽을 허용해야 합니다. 예를 들어, 클러스터가 미국 남부에 있는 경우 포트 443에서 모든 위치(dal10, dal12 및 dal13)의 IP 주소로의 트래픽을 허용해야 합니다.
      <p>
  <table summary="테이블의 첫 번째 행에는 두 개의 열이 있습니다. 나머지 행은 왼쪽에서 오른쪽으로 읽어야 하며 1열에는 서버 위치, 2열에는 일치시킬 IP 주소가 있습니다.">
  <caption>발신 트래픽을 위해 열리는 IP 주소</caption>
      <thead>
      <th>지역</th>
      <th>위치</th>
      <th>IP 주소</th>
      </thead>
    <tbody>
      <tr>
        <td>AP 북부</td>
        <td>hkg02<br>seo01<br>sng01<br>tok02</td>
        <td><code>169.56.132.234</code><br><code>169.56.69.242</code><br><code>161.202.186.226</code><br><code>161.202.126.210</code></td>
       </tr>
      <tr>
         <td>AP 남부</td>
         <td>mel01<br>syd01<br>syd04</td>
         <td><code>168.1.97.67</code><br><code>168.1.8.195</code><br><code>130.198.64.19, 130.198.66.34</code></td>
      </tr>
      <tr>
         <td>중앙 유럽</td>
         <td>ams03<br>fra02<br>mil01<br>par01</td>
         <td><code>169.50.169.110, 169.50.154.194</code><br><code>169.50.56.174</code><br><code>159.122.190.98</code><br><code>159.8.86.149, 159.8.98.170</code></td>
        </tr>
      <tr>
        <td>영국 남부</td>
        <td>lon02<br>lon04</td>
        <td><code>159.122.242.78</code><br><code>158.175.65.170, 158.175.74.170, 158.175.76.2</code></td>
      </tr>
      <tr>
        <td>미국 동부</td>
         <td>mon01<br>tor01<br>wdc06<br>wdc07</td>
         <td><code>169.54.126.219</code><br><code>169.53.167.50</code><br><code>169.60.73.142</code><br><code>169.61.83.62</code></td>
      </tr>
      <tr>
        <td>미국 남부</td>
        <td>dal10<br>dal12<br>dal13<br>hou02<br>sao01</td>
        <td><code>169.47.234.18, 169.46.7.238</code><br><code>169.47.70.10</code><br><code>169.60.128.2</code><br><code>184.173.44.62</code><br><code>169.57.151.10</code></td>
      </tr>
      </tbody>
    </table>
</p>

  3.  작업자 노드에서 [{{site.data.keyword.registrylong_notm}} 지역](/docs/services/Registry/registry_overview.html#registry_regions)으로의 발신 네트워크 트래픽을 허용하십시오.
      - `TCP port 443 FROM <each_worker_node_publicIP> TO <registry_publicIP>`
      - <em>&lt;registry_publicIP&gt;</em>를 트래픽을 허용하려는 레지스트리 IP 주소로 대체하십시오. 글로벌 레지스트리는 IBM 제공 공용 이미지를 저장하고 지역 레지스트리는 사용자의 개인용 이미지 또는 공용 이미지를 저장합니다.
        <p>
<table summary="테이블의 첫 번째 행에는 두 개의 열이 있습니다. 나머지 행은 왼쪽에서 오른쪽으로 읽어야 하며 1열에는 서버 위치, 2열에는 일치시킬 IP 주소가 있습니다.">
  <caption>레지스트리 트래픽을 위해 열리는 IP 주소</caption>
      <thead>
        <th>{{site.data.keyword.containershort_notm}} 지역</th>
        <th>레지스트리 주소</th>
        <th>레지스트리 IP 주소</th>
      </thead>
      <tbody>
        <tr>
          <td>{{site.data.keyword.containershort_notm}} 지역 전체의 글로벌 레지스트리</td>
          <td>registry.bluemix.net</td>
          <td><code>169.60.72.144/28</code><br><code>169.61.76.176/28</code></td>
        </tr>
        <tr>
          <td>AP 북부, AP 남부</td>
          <td>registry.au-syd.bluemix.net</td>
          <td><code>168.1.45.160/27</code></br><code>168.1.139.32/27</code></td>
        </tr>
        <tr>
          <td>중앙 유럽</td>
          <td>registry.eu-de.bluemix.net</td>
          <td><code>169.50.56.144/28</code></br><code>159.8.73.80/28</code></td>
         </tr>
         <tr>
          <td>영국 남부</td>
          <td>registry.eu-gb.bluemix.net</td>
          <td><code>159.8.188.160/27</code></br><code>169.50.153.64/27</code></td>
         </tr>
         <tr>
          <td>미국 동부, 미국 남부</td>
          <td>registry.ng.bluemix.net</td>
          <td><code>169.55.39.112/28</code></br><code>169.46.9.0/27</code></br><code>169.55.211.0/27</code></td>
         </tr>
        </tbody>
      </table>
</p>

  4.  선택사항: 작업자 노드에서 {{site.data.keyword.monitoringlong_notm}} 및 {{site.data.keyword.loganalysislong_notm}} 서비스로의 발신 네트워크 트래픽을 허용하십시오.
      - `TCP port 443, port 9095 FROM <each_worker_node_public_IP> TO <monitoring_public_IP>`
      - <em>&lt;monitoring_public_IP&gt;</em>를 트래픽을 허용할 모니터링 지역에 대한 모든 주소로 대체하십시오.
        <p><table summary="테이블의 첫 번째 행에는 두 개의 열이 있습니다. 나머지 행은 왼쪽에서 오른쪽으로 읽어야 하며 1열에는 서버 위치, 2열에는 일치시킬 IP 주소가 있습니다.">
  <caption>모니터링 트래픽을 위해 열리는 IP 주소</caption>
        <thead>
        <th>{{site.data.keyword.containershort_notm}} 지역</th>
        <th>모니터링 주소</th>
        <th>모니터링 IP 주소</th>
        </thead>
      <tbody>
        <tr>
         <td>중앙 유럽</td>
         <td>metrics.eu-de.bluemix.net</td>
         <td><code>159.122.78.136/29</code></td>
        </tr>
        <tr>
         <td>영국 남부</td>
         <td>metrics.eu-gb.bluemix.net</td>
         <td><code>169.50.196.136/29</code></td>
        </tr>
        <tr>
          <td>미국 동부, 미국 남부, AP 북부</td>
          <td>metrics.ng.bluemix.net</td>
          <td><code>169.47.204.128/29</code></td>
         </tr>
         
        </tbody>
      </table>
</p>
      - `TCP port 443, port 9091 FROM <each_worker_node_public_IP> TO <logging_public_IP>`
      - <em>&lt;logging_public_IP&gt;</em>를 트래픽을 허용할 로깅 지역에 대한 모든 주소로 대체하십시오.
        <p><table summary="테이블의 첫 번째 행에는 두 개의 열이 있습니다. 나머지 행은 왼쪽에서 오른쪽으로 읽어야 하며 1열에는 서버 위치, 2열에는 일치시킬 IP 주소가 있습니다.">
<caption>로깅 트래픽을 위해 열리는 IP 주소</caption>
        <thead>
        <th>{{site.data.keyword.containershort_notm}} 지역</th>
        <th>로깅 주소</th>
        <th>로깅 IP 주소</th>
        </thead>
        <tbody>
          <tr>
            <td>미국 동부, 미국 남부</td>
            <td>ingest.logging.ng.bluemix.net</td>
            <td><code>169.48.79.236</code><br><code>169.46.186.113</code></td>
           </tr>
          <tr>
           <td>영국 남부</td>
           <td>ingest.logging.eu-gb.bluemix.net</td>
           <td><code>169.50.115.113</code></td>
          </tr>
          <tr>
           <td>중앙 유럽</td>
           <td>ingest-eu-fra.logging.bluemix.net</td>
           <td><code>158.177.88.43</code><br><code>159.122.87.107</code></td>
          </tr>
          <tr>
           <td>AP 남부, AP 북부</td>
           <td>ingest-au-syd.logging.bluemix.net</td>
           <td><code>130.198.76.125</code><br><code>168.1.209.20</code></td>
          </tr>
         </tbody>
       </table>
</p>

  5. 사설 방화벽의 경우 적절한 IBM Cloud 인프라(SoftLayer) 사설 IP 범위를 허용하십시오. [이 링크](https://knowledgelayer.softlayer.com/faq/what-ip-ranges-do-i-allow-through-firewall)의 **백엔드(사설) 네트워크** 섹션부터 참조하십시오.
      - 사용 중인 [지역 내의 위치](cs_regions.html#locations)를 모두 추가하십시오.
      - dal01 위치(데이터센터)를 추가해야 합니다.
      - 클러스터 부트스트랩 프로세스를 허용하려면 포트 80 및 443을 여십시오.

  6. {: #pvc}데이터 스토리지에 대한 지속적 볼륨 클레임을 작성하려면 클러스터가 있는 위치(데이터센터)의 [IBM Cloud 인프라(SoftLayer) IP 주소](https://knowledgelayer.softlayer.com/faq/what-ip-ranges-do-i-allow-through-firewall)에 대해 방화벽을 통한 egress 액세스를 허용하십시오.
      - 클러스터의 위치(데이터센터)를 찾으려면 `bx cs clusters`를 실행하십시오.
      - **프론트 엔드(공용) 네트워크**와 **백엔드(사설) 네트워크** 둘 다에 대해 IP 범위에 대한 액세스를 허용하십시오.
      - **백엔드(사설) 네트워크**를 위해 dal01 위치(데이터센터)를 추가해야 합니다.

<br />


## 클러스터 외부에서 NodePort, 로드 밸런서 및 Ingress 서비스에 액세스
{: #firewall_inbound}

NodePort, 로드 밸런서 및 Ingress 서비스에 대한 수신 액세스를 허용할 수 있습니다.
{:shortdesc}

<dl>
  <dt>NodePort 서비스</dt>
  <dd>모든 작업자 노드가 트래픽을 허용할 수 있도록 서비스를 공인 IP 주소에 배치할 때 구성한 포트를 여십시오. 포트를 찾으려면 `kubectl get svc`를 실행하십시오. 포트는 20000 - 32000 범위에 있습니다.<dd>
  <dt>LoadBalancer 서비스</dt>
  <dd>서비스를 로드 밸런스 서비스의 공인 IP 주소에 배치할 때 구성한 포트를 여십시오.</dd>
  <dt>Ingress</dt>
  <dd>Ingress 애플리케이션 로드 밸런서의 IP 주소에 대해 HTTP의 경우 포트 80을 열고 HTTPS의 경우 포트 443을 여십시오.</dd>
</dl>
