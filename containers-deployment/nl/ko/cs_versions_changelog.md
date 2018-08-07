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



# 버전 변경 로그
{: #changelog}

{{site.data.keyword.containerlong}} Kubernetes 클러스터에 대해 사용 가능한 주 버전, 부 버전 및 패치 업데이트의 버전 변경 정보를 보십시오. 변경사항에는 Kubernetes 및 {{site.data.keyword.Bluemix_notm}} Provider 컴포넌트에 대한 업데이트가 포함됩니다. 
{:shortdesc}

IBM은 마스터에 자동으로 패치 레벨 업데이트를 적용하지만, [작업자 노드 업데이트](cs_cluster_update.html#worker_node)는 수동으로 수행해야 합니다. 사용 가능한 업데이트를 매월 확인하십시오. 업데이트가 사용 가능해지면 `bx cs workers <cluster>` 또는 `bx cs worker-get <cluster> <worker>` 명령 등을 사용하여 작업자 노드에 대한 정보를 볼 때 알림을 받습니다.

마이그레이션 조치에 대한 요약은 [Kubernetes 버전](cs_versions.html)을 참조하십시오.
{: tip}

이전 버전에서 변경된 사항에 대한 정보는 다음 변경 로그를 보십시오.
-  버전 1.10 [변경 로그](#110_changelog).
-  버전 1.9 [변경 로그](#19_changelog).
-  버전 1.8 [변경 로그](#18_changelog).
-  **더 이상 사용되지 않음**: 버전 1.7 [변경 로그](#17_changelog).

## 버전 1.10 변경 로그
{: #110_changelog}

다음 변경사항을 검토하십시오.

### 2018년 5월 18일에 릴리스된 작업자 노드 수정팩 1.10.1_1510에 대한 변경 로그
{: #1101_1510}

<table summary="버전 1.10.1_1509 이후에 작성된 변경사항">
<caption>버전 1.10.1_1509 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>블록 스토리지 플러그인을 사용한 경우 발생한 버그를 해결하기 위한 수정사항입니다.</td>
</tr>
</tbody>
</table>

### 2018년 5월 16일에 릴리스된 작업자 노드 수정팩 1.10.1_1509에 대한 변경 로그
{: #1101_1509}

<table summary="버전 1.10.1_1508 이후에 작성된 변경사항">
<caption>버전 1.10.1_1508 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>`kubelet` 루트 디렉토리에 저장하는 데이터가 이제 작업자 노드 머신의 더 큰 보조 디스크에 저장됩니다.</td>
</tr>
</tbody>
</table>

### 2018년 5월 1일에 릴리스된 1.10.1_1508에 대한 변경 로그
{: #1101_1508}

<table summary="버전 1.9.7_1510 이후에 작성된 변경사항">
<caption>버전 1.9.7_1510 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v2.6.5</td>
<td>v3.1.1</td>
<td>Calico [릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.projectcalico.org/v3.1/releases/#v311)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes Heapster</td>
<td>v1.5.0</td>
<td>v1.5.2</td>
<td>Kubernetes Heapster [릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/heapster/releases/tag/v1.5.2)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.7</td>
<td>v1.10.1</td>
<td>Kubernetes [릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.1)를 참조하십시오.</td>
</tr>
<tr>
<td>Kubernetes 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>클러스터의 Kubernetes API 서버에 대한 <code>--enable-admission-plugins</code> 옵션에 <code>StorageObjectInUseProtection</code>이 추가되었습니다.</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>1.14.8</td>
<td>1.14.10</td>
<td>Kubernetes DNS [릴리스 정보 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/dns/releases/tag/1.14.10)를 참조하십시오.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.9.7-102</td>
<td>v1.10.1-52</td>
<td>Kubernetes 1.10 릴리스를 지원하도록 업데이트되었습니다.</td>
</tr>
<tr>
<td>GPU 지원</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>이제 스케줄링 및 실행을 위해 [그래픽 처리 장치(GPU) 컨테이너 워크로드](cs_app.html#gpu_app)에 대한 지원을 사용할 수 있습니다. 사용 가능한 GPU 머신 유형의 목록은 [작업자 노드에 대한 하드웨어](cs_clusters.html#shared_dedicated_node)를 참조하십시오. 자세한 정보는 [GPU 스케줄 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/)에 대한 Kubernetes 문서를 참조하십시오.</td>
</tr>
</tbody>
</table>

## 버전 1.9 변경 로그
{: #19_changelog}

다음 변경사항을 검토하십시오.

### 2018년 5월 18일에 릴리스된 작업자 노드 수정팩 1.9.7_1512에 대한 변경 로그
{: #197_1512}

<table summary="버전 1.9.7_1511 이후에 작성된 변경사항">
<caption>버전 1.9.7_1511 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>블록 스토리지 플러그인을 사용한 경우 발생한 버그를 해결하기 위한 수정사항입니다.</td>
</tr>
</tbody>
</table>

### 2018년 5월 16일에 릴리스된 작업자 노드 수정팩 1.9.7_1511에 대한 변경 로그
{: #197_1511}

<table summary="버전 1.9.7_1510 이후에 작성된 변경사항">
<caption>버전 1.9.7_1510 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>`kubelet` 루트 디렉토리에 저장하는 데이터가 이제 작업자 노드 머신의 더 큰 보조 디스크에 저장됩니다.</td>
</tr>
</tbody>
</table>

### 2018년 4월 30일에 릴리스된 1.9.7_1510에 대한 변경 로그
{: #197_1510}

<table summary="버전 1.9.3_1506 이후에 작성된 변경사항">
<caption>버전 1.9.3_1506 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.9.3</td>
<td>v1.9.7	</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.7)를 참조하십시오. 이 릴리스에서는 [CVE-2017-1002101 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) 및 [CVE-2017-1002102 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102) 취약성이 해결되었습니다.</td>
</tr>
<tr>
<td>Kubernetes 구성</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>클러스터의 Kubernetes API 서버에 대한 `--runtime-config` 옵션에 <code>admissionregistration.k8s.io/v1alpha1=true</code>가 추가되었습니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.9.3-71</td>
<td>v1.9.7-102</td>
<td>`NodePort` 및 `LoadBalancer` 서비스는 이제 `service.spec.externalTrafficPolicy`를 `Local`로 설정하여 [클라이언스 소스 IP를 유지](cs_loadbalancer.html#node_affinity_tolerations)하는 것을 지원합니다.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>이전 클러스터의 [에지 노드](cs_edge.html#edge) 결함 허용 설정을 수정합니다.</td>
</tr>
</tbody>
</table>

## 버전 1.8 변경 로그
{: #18_changelog}

다음 변경사항을 검토하십시오.

### 2018년 5월 18일에 릴리스된 작업자 노드 수정팩 1.8.11_1511에 대한 변경 로그
{: #1811_1511}

<table summary="버전 1.8.11_1510 이후에 작성된 변경사항">
<caption>버전 1.8.11_1510 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>블록 스토리지 플러그인을 사용한 경우 발생한 버그를 해결하기 위한 수정사항입니다.</td>
</tr>
</tbody>
</table>

### 2018년 5월 16일에 릴리스된 작업자 노드 수정팩 1.8.11_1510에 대한 변경 로그
{: #1811_1510}

<table summary="버전 1.8.11_1509 이후에 작성된 변경사항">
<caption>버전 1.8.11_1509 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>`kubelet` 루트 디렉토리에 저장하는 데이터가 이제 작업자 노드 머신의 더 큰 보조 디스크에 저장됩니다.</td>
</tr>
</tbody>
</table>


### 2018년 4월 19일에 릴리스된 1.8.11_1509에 대한 변경 로그
{: #1811_1509}

<table summary="버전 1.8.8_1507 이후에 작성된 변경사항">
<caption>버전 1.8.8_1507 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.8.8</td>
<td>v1.8.11	</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.11)를 참조하십시오. 이 릴리스에서는 [CVE-2017-1002101 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) 및 [CVE-2017-1002102 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102) 취약성이 해결되었습니다.</td>
</tr>
<tr>
<td>컨테이너 이미지 일시정지</td>
<td>3.0</td>
<td>3.1</td>
<td>상속된 고아 좀비 프로세스를 제거합니다.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.8.8-86</td>
<td>v1.8.11-126</td>
<td>`NodePort` 및 `LoadBalancer` 서비스는 이제 `service.spec.externalTrafficPolicy`를 `Local`로 설정하여 [클라이언스 소스 IP를 유지](cs_loadbalancer.html#node_affinity_tolerations)하는 것을 지원합니다.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>이전 클러스터의 [에지 노드](cs_edge.html#edge) 결함 허용 설정을 수정합니다.</td>
</tr>
</tbody>
</table>

## 아카이브
{: #changelog_archive}

### 버전 1.7 변경 로그(더 이상 사용되지 않음)
{: #17_changelog}

다음 변경사항을 검토하십시오.

#### 2018년 5월 18일에 릴리스된 작업자 노드 수정팩 1.7.16_1513에 대한 변경 로그
{: #1716_1513}

<table summary="버전 1.7.16_1512 이후에 작성된 변경사항">
<caption>버전 1.7.16_1512 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>블록 스토리지 플러그인을 사용한 경우 발생한 버그를 해결하기 위한 수정사항입니다.</td>
</tr>
</tbody>
</table>

#### 2018년 5월 16일에 릴리스된 작업자 노드 수정팩 1.7.16_1512에 대한 변경 로그
{: #1716_1512}

<table summary="버전 1.7.16_1511 이후에 작성된 변경사항">
<caption>버전 1.7.16_1511 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>해당사항 없음</td>
<td>해당사항 없음</td>
<td>`kubelet` 루트 디렉토리에 저장하는 데이터가 이제 작업자 노드 머신의 더 큰 보조 디스크에 저장됩니다.</td>
</tr>
</tbody>
</table>

#### 2018년 4월 19일에 릴리스된 1.7.16_1511에 대한 변경 로그
{: #1716_1511}

<table summary="버전 1.7.4_1509 이후에 작성된 변경사항">
<caption>버전 1.7.4_1509 이후의 변경사항</caption>
<thead>
<tr>
<th>컴포넌트</th>
<th>이전</th>
<th>현재</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.7.4</td>
<td>v1.7.16	</td>
<td>[Kubernetes 릴리스 정보![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/kubernetes/kubernetes/releases/tag/v1.7.16)를 참조하십시오. 이 릴리스에서는 [CVE-2017-1002101 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) 및 [CVE-2017-1002102 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102) 취약성이 해결되었습니다.</td>
</tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.7.4-133</td>
<td>v1.7.16-17</td>
<td>`NodePort` 및 `LoadBalancer` 서비스는 이제 `service.spec.externalTrafficPolicy`를 `Local`로 설정하여 [클라이언스 소스 IP를 유지](cs_loadbalancer.html#node_affinity_tolerations)하는 것을 지원합니다.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>이전 클러스터의 [에지 노드](cs_edge.html#edge) 결함 허용 설정을 수정합니다.</td>
</tr>
</tbody>
</table>
