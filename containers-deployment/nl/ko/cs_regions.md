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



# 지역 및 위치
{{site.data.keyword.Bluemix}}는 전세계적으로 호스팅됩니다. 지역은 엔드포인트에서 액세스하는 지리적 영역입니다. 위치는 지역 내의 데이터센터입니다. {{site.data.keyword.Bluemix_notm}} 내의 서비스는 글로벌로 사용 가능하거나 특정 지역 내에서 사용 가능할 수 있습니다. {{site.data.keyword.containerlong}}에서 Kubernetes 클러스터를 작성할 때 리소스는 클러스터를 배치하는 지역에 남아 있습니다.
{:shortdesc}

[{{site.data.keyword.Bluemix_notm}} 지역](#bluemix_regions)은 [{{site.data.keyword.containershort_notm}} 지역](#container_regions)과 다릅니다.

![{{site.data.keyword.containershort_notm}} 지역 및 위치](/images/regions.png)

_{{site.data.keyword.containershort_notm}} 지역 및 위치_

지원되는 {{site.data.keyword.containershort_notm}} 지역은 다음과 같습니다.
  * AP 북부
  * AP 남부
  * 중앙 유럽
  * 영국 남부
  * 미국 동부
  * 미국 남부


## {{site.data.keyword.Bluemix_notm}} 지역 API 엔드포인트
{: #bluemix_regions}

{{site.data.keyword.Bluemix_notm}} 지역을 사용하여 {{site.data.keyword.Bluemix_notm}} 서비스에 걸쳐 리소스를 구성할 수 있습니다. 예를 들어, 동일한 지역의 {{site.data.keyword.registryshort_notm}}에 저장된 개인용 Docker 이미지를 사용하여 Kubernetes 클러스터를 작성할 수 있습니다.
{:shortdesc}

현재 사용자가 속해 있는 {{site.data.keyword.Bluemix_notm}} 지역을 확인하려면 `bx info`를 실행하고 **지역** 필드를 검토하십시오.

로그인할 때 API 엔드포인트를 지정하여 {{site.data.keyword.Bluemix_notm}} 지역에 액세스할 수 있습니다. 지역을 지정하지 않으면 가장 근접한 지역에 자동으로 로그인됩니다.

예를 들어, 다음 명령을 사용하여 {{site.data.keyword.Bluemix_notm}} 지역 API 엔드포인트에 로그인할 수 있습니다.

  * 미국 남부 및 미국 동부
      ```
           bx login -a api.ng.bluemix.net
      ```
      {: pre}

  * 시드니 및 AP 북부
      ```
        bx login -a api.au-syd.bluemix.net
      ```
      {: pre}

  * 독일
      ```
           bx login -a api.eu-de.bluemix.net
      ```
      {: pre}

  * 영국
      ```
           bx login -a api.eu-gb.bluemix.net
      ```
      {: pre}



<br />


## {{site.data.keyword.containershort_notm}} 지역 API 엔드포인트 및 위치
{: #container_regions}

{{site.data.keyword.containershort_notm}} 지역을 사용하여 사용자가 로그인한 {{site.data.keyword.Bluemix_notm}} 지역 이외의 지역에 Kubernetes 클러스터를 작성하거나 액세스할 수 있습니다. {{site.data.keyword.containershort_notm}} 지역 엔드포인트는 특별히 {{site.data.keyword.Bluemix_notm}}가 아니라 {{site.data.keyword.containershort_notm}}를 전체적으로 참조합니다.
{:shortdesc}

단일 글로벌 엔드포인트 `https://containers.bluemix.net/`를 통해 {{site.data.keyword.containershort_notm}}에 액세스할 수 있습니다.
* 현재 사용자가 있는 {{site.data.keyword.containershort_notm}} 지역을 확인하려면 `bx cs region`을 실행하십시오.
* 사용 가능한 지역과 해당 엔드포인트 목록을 검색하려면 `bx cs regions`를 실행하십시오.

글로벌 엔드포인트가 있는 API를 사용하려면 모든 요청에서 `X-Region` 헤더에 지역 이름을 전달하십시오.
{: tip}

### 다른 {{site.data.keyword.containerlong}_notm} 지역에 로그인
{: #container_login_endpoints}

{{site.data.keyword.containershort_notm}} CLI를 사용하여 위치를 변경할 수 있습니다.
{:shortdesc}

다음과 같은 이유로 다른 {{site.data.keyword.containershort_notm}} 지역에 로그인할 수 있습니다.
  * 한 지역에 {{site.data.keyword.Bluemix_notm}} 서비스 또는 사설 Docker를 작성한 후 다른 지역의 {{site.data.keyword.containershort_notm}}에서 사용하려고 합니다.
  * 로그인한 기본 {{site.data.keyword.Bluemix_notm}} 지역과 다른 지역에 있는 클러스터에 액세스하려고 합니다.

</br>

지역을 신속하게 전환하려면 `bx cs region-set`를 실행하십시오.

### {{site.data.keyword.containerlong_notm}} API 명령 사용
{: #containers_api}

{{site.data.keyword.containershort_notm}} API와 상호작용하려면 명령 유형을 입력하고 `/v1/command`를 글로벌 엔드포인트에 추가하십시오.
{:shortdesc}

`GET /clusters` API 예제:
  ```
  GET https://containers.bluemix.net/v1/clusters
  ```
  {: codeblock}

</br>

글로벌 엔드포인트가 있는 API를 사용하려면 모든 요청에서 `X-Region` 헤더에 지역 이름을 전달하십시오. 사용 가능한 지역을 나열하려면 `bx cs regions`를 실행하십시오.
{: tip}

API 명령에 대한 문서는 [https://containers.bluemix.net/swagger-api/](https://containers.bluemix.net/swagger-api/)를 보십시오.

## {{site.data.keyword.containershort_notm}}에 사용 가능한 위치
{: #locations}

위치는 {{site.data.keyword.Bluemix_notm}} 지역 내에서 사용 가능한 실제 데이터센터입니다. 지역은 위치를 구성하기 위한 개념적인 도구이며, 다른 나라의 위치(데이터센터)에 포함시킬 수 있습니다. 다음 표는 지역별로 사용 가능한 위치를 표시합니다.
{:shortdesc}

| 지역 | 위치 | 도시 |
|--------|----------|------|
| AP 북부 | hkg02, seo01, sng01, tok02 | 홍콩 특별행정구, 서울, 싱가포르, 도쿄 |
| AP 남부     | mel01, syd01, syd04        | 멜버른, 시드니 |
| 중앙 유럽     | ams03, fra02, par01        | 암스테르담, 프랑크푸르트, 파리 |
| 영국 남부      | lon02, lon04         | 런던 |
| 미국 동부      | mon01, tor01, wdc06, wdc07        | 몬트리올, 토론토, 워싱턴 DC |
| 미국 남부     | dal10, dal12, dal13, sao01       | 댈러스, 상파울루 |
{: caption="사용 가능한 지역 및 위치" caption-side="top"}

클러스터의 리소스는 클러스터가 배치된 위치(데이터센터)에 남아 있습니다. 다음 이미지는 미국 동부의 지역 예제 내에서 클러스터의 관계를 강조표시합니다.

1.  마스터 및 작업자 노드를 포함한 클러스터의 리소스는 클러스터를 배치한 같은 위치에 있습니다. `kubectl` 명령과 같은 로컬 컨테이너 오케스트레이션 조치를 작성할 때 동일한 위치 내의 마스터와 작업자 노드 간에 정보가 교환됩니다.

2.  스토리지, 네트워킹, 컴퓨팅 또는 팟(Pod)에서 실행되는 앱과 같은 기타 클러스터 리소스를 설정하면 리소스 및 해당 데이터가 클러스터를 배치한 위치에 남아 있습니다.

3.  `bx cs` 명령 사용과 같은 클러스터 관리 조치를 작성할 때 클러스터에 대한 기본 정보(예: 이름, ID, 사용자, 명령)가 지역 엔드포인트에 라우팅됩니다.

![클러스터 리소스가 있는 위치 이해](/images/region-cluster-resources.png)

_클러스터 리소스가 있는 위치 이해_



