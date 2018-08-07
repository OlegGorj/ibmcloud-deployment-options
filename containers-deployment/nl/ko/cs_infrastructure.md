---

copyright:
  years: 2014, 2018
lastupdated: "2018-4-20"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# IBM Cloud 인프라(SoftLayer) 포트폴리오 액세스
{: #infrastructure}

표준 Kubernetes 클러스터를 작성하려면 IBM Cloud 인프라(SoftLayer) 포트폴리오에 대한 액세스 권한이 있어야 합니다. {{site.data.keyword.containerlong}}에서 이러한 액세스 권한은 작업자 노드, 포터블 공인 IP 주소 또는 Kubernetes 클러스터에 대한 지속적 스토리지 등과 같은 유료 인프라 리소스를 요청하는 데 필요합니다.
{:shortdesc}

## IBM Cloud 인프라(SoftLayer) 포트폴리오 액세스
{: #unify_accounts}

자동 계정 링크가 사용 가능해진 후에 작성된 {{site.data.keyword.Bluemix_notm}} 종량과금제 계정은 IBM Cloud 인프라(SoftLayer) 포트폴리오에 대한 액세스 권한으로 이미 설정되어 있습니다. 추가 구성 없이 클러스터의 인프라 리소스를 구매할 수 있습니다.
{:shortdesc}

기타 {{site.data.keyword.Bluemix_notm}} 계정 유형의 사용자나 자체 {{site.data.keyword.Bluemix_notm}} 계정에 연결되지 않은 기존 IBM Cloud 인프라(SoftLayer) 계정을 보유한 사용자는 표준 클러스터를 작성하기 위한 자체 계정을 구성해야 합니다.
{:shortdesc}

각 계정 유형에 대해 사용 가능한 옵션을 찾으려면 다음 표를 검토하십시오.

|계정 유형|설명|표준 클러스터를 작성하기 위해 사용 가능한 옵션|
|------------|-----------|----------------------------------------------|
|라이트 계정|라이트 계정은 클러스터를 프로비저닝할 수 없습니다.|[라이트 계정을 IBM Cloud 인프라(SoftLayer) 포트폴리오에 대한 액세스 권한으로 설정된 {{site.data.keyword.Bluemix_notm}} 종량과금제 계정](/docs/account/index.html#billableacts)으로 업그레이드하십시오.|
|이전 종량과금제 계정|자동 계정 링크가 사용 가능하기 전에 작성된 종량과금제 계정에서는 IBM Cloud 인프라(SoftLayer) 포트폴리오에 대한 액세스 권한을 제공하지 않았습니다.<p>기존 IBM Cloud 인프라(SoftLayer) 계정이 있으면 이 계정을 이전 종량과금제 계정에 연결할 수 없습니다.</p>|옵션 1: IBM Cloud 인프라(SoftLayer) 포트폴리오에 대한 액세스 권한으로 설정된 [새 종량과금제 계정을 작성](/docs/account/index.html#billableacts)합니다. 이 옵션을 선택하는 경우에는 두 개의 별도 {{site.data.keyword.Bluemix_notm}} 계정과 비용 청구가 있습니다.<p>계속해서 이전 종량과금제 계정을 사용하여 표준 클러스터를 작성하려는 경우에는 새 종량과금제 계정을 사용하여 IBM Cloud 인프라(SoftLayer) 포트폴리오에 액세스하기 위한 API 키를 생성할 수 있습니다. 그리고 이전 종량과금제 계정에 대해 API 키를 설정해야 합니다. 자세한 정보는 [이전 종량과금제 및 구독 계정에 대한 API 키 생성](#old_account)을 참조하십시오. IBM Cloud 인프라(SoftLayer) 리소스가 새 종량과금제 계정을 통해 비용 청구된다는 점을 유념하십시오.</p></br><p>옵션 2: 사용할 기존 IBM Cloud 인프라(SoftLayer) 계정이 이미 있는 경우에는 {{site.data.keyword.Bluemix_notm}} 계정에 대해 [신임 정보를 설정](cs_cli_reference.html#cs_credentials_set)할 수 있습니다.</p><p>**참고:** {{site.data.keyword.Bluemix_notm}} 계정과 함께 사용하는 IBM Cloud 인프라(SoftLayer) 계정은 수퍼유저 권한으로 설정되어야 합니다.</p>|
|구독 계정|구독 계정은 IBM Cloud 인프라(SoftLayer) 포트폴리오에 대한 액세스 권한으로 설정되지 않습니다.|옵션 1: IBM Cloud 인프라(SoftLayer) 포트폴리오에 대한 액세스 권한으로 설정된 [새 종량과금제 계정을 작성](/docs/account/index.html#billableacts)합니다. 이 옵션을 선택하는 경우에는 두 개의 별도 {{site.data.keyword.Bluemix_notm}} 계정과 비용 청구가 있습니다.<p>계속해서 구독 계정을 사용하여 표준 클러스터를 작성하려는 경우 새 종량과금제 계정을 사용하여 IBM Cloud 인프라(SoftLayer) 포트폴리오에 액세스하기 위한 API 키를 생성할 수 있습니다. 그리고 사용자의 구독 계정에 대해 API 키를 설정해야 합니다. 자세한 정보는 [이전 종량과금제 및 구독 계정에 대한 API 키 생성](#old_account)을 참조하십시오. IBM Cloud 인프라(SoftLayer) 리소스가 새 종량과금제 계정을 통해 비용 청구된다는 점을 유념하십시오.</p></br><p>옵션 2: 사용할 기존 IBM Cloud 인프라(SoftLayer) 계정이 이미 있는 경우에는 {{site.data.keyword.Bluemix_notm}} 계정에 대해 [신임 정보를 설정](cs_cli_reference.html#cs_credentials_set)할 수 있습니다.<p>**참고:** {{site.data.keyword.Bluemix_notm}} 계정과 함께 사용하는 IBM Cloud 인프라(SoftLayer) 계정은 수퍼유저 권한으로 설정되어야 합니다.</p>|
|{{site.data.keyword.Bluemix_notm}} 계정이 아닌 IBM Cloud 인프라(SoftLayer) 계정|표준 클러스터를 작성하려면 {{site.data.keyword.Bluemix_notm}} 계정이 있어야 합니다.|<p>IBM Cloud 인프라(SoftLayer) 포트폴리오에 대한 액세스 권한으로 설정된 [새 종량과금제 계정을 작성](/docs/account/index.html#billableacts)하십시오. 이 옵션을 선택하는 경우, IBM Cloud 인프라(SoftLayer)가 작성됩니다. 두 개의 별도 IBM Cloud 인프라(SoftLayer) 계정과 비용 청구가 있습니다.</p>|

<br />


## {{site.data.keyword.Bluemix_notm}} 계정에서 사용할 IBM Cloud 인프라(SoftLayer) API 키 생성
{: #old_account}

계속해서 이전 종량과금제 또는 구독 계정을 사용하여 표준 클러스터를 계속 작성하려면 새 종량과금제 계정으로 API 키를 생성하고 이전 계정에 대해 API 키를 설정하십시오.
{:shortdesc}

시작하기 전에 IBM Cloud 인프라(SoftLayer) 포트폴리오에 대한 액세스 권한으로 자동 설정된 {{site.data.keyword.Bluemix_notm}} 종량과금제 계정을 작성하십시오.

1.  새 종량과금제 계정을 위해 작성된 {{site.data.keyword.ibmid}} 및 비밀번호를 사용하여 [IBM Cloud 인프라(SoftLayer) 포털 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/)에 로그인하십시오.
2.  **계정**을 선택한 후에 **사용자**를 선택하십시오.
3.  **생성**을 클릭하여 새 종량과금제 계정에 대한 IBM Cloud 인프라(SoftLayer) API 키를 생성하십시오.
4.  API 키를 복사하십시오.
5.  CLI에서 이전 종량과금제 또는 구독 계정의 {{site.data.keyword.ibmid}} 및 비밀번호를 사용하여 {{site.data.keyword.Bluemix_notm}}에 로그인하십시오.

  ```
   bx login
  ```
  {: pre}

6.  IBM Cloud 인프라(SoftLayer) 포트폴리오에 액세스할 수 있도록 이전에 생성된 API 키를 설정하십시오. `<API_key>`를 API 키로 대체하고 `<username>`을 새 종량과금제 계정의 {{site.data.keyword.ibmid}}로 대체하십시오.

  ```
  bx cs credentials-set --infrastructure-api-key <API_key> --infrastructure-username <username>
  ```
  {: pre}

7.  [표준 클러스터 작성](cs_clusters.html#clusters_cli)을 시작하십시오.

**참고:** API 키를 생성한 후에 이를 검토하려면 1단계와 2단계를 수행한 후에 **API 키** 섹션에서 **보기**를 클릭하여 사용자 ID에 대한 API 키를 보십시오.

