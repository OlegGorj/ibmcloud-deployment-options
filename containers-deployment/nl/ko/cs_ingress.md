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




# Ingress를 사용한 앱 노출
{: #ingress}

{{site.data.keyword.containerlong}}에서 IBM 제공 애플리케이션 로드 밸런서에서 관리하는 Ingress 리소스를 작성하여 Kubernetes 클러스터에서 다중 앱을 노출합니다.
{:shortdesc}

## Ingress를 사용한 네트워크 트래픽 관리
{: #planning}

Ingress는 공용 또는 개인용 요청을 앱에 전달함으로써 클러스터 내 네트워크 트래픽 워크로드의 균형을 유지합니다. Ingress를 사용하여 고유 공용 또는 개인용 라우트를 사용함으로써 공용 또는 사설 네트워크에 여러 앱 서비스를 노출시킬 수 있습니다.
{:shortdesc}



Ingress는 다음 두 가지 컴포넌트로 구성됩니다.
<dl>
<dt>애플리케이션 로드 밸런서</dt>
<dd>애플리케이션 로드 밸런서(ALB)는 수신 HTTP, HTTPS, TCP 또는 UDP 서비스 요청을 청취하고 적절한 앱 팟(Pod)으로 요청을 전달하는 외부 로드 밸런서입니다. 표준 클러스터를 작성할 때 {{site.data.keyword.containershort_notm}}에서 자동으로 클러스터용 고가용성 ALB를 작성하고 고유 공용 라우트를 ALB에 지정합니다. 공용 라우트는 클러스터 작성 중에 IBM Cloud 인프라(SoftLayer) 계정으로 프로비저닝된 포터블 공인 IP 주소에 링크됩니다. 기본 사설 ALB도 자동으로 작성되지만 자동으로 사용으로 설정되지는 않습니다.</dd>
<dt>Ingress 리소스</dt>
<dd>Ingress를 사용하여 앱을 노출시키려면 앱에 대한 Kubernetes 서비스를 작성하고 Ingress 리소스를 정의하여 ALB에 이 서비스를 등록해야 합니다. Ingress 리소스는 앱에 대한 수신 요청을 라우팅하는 방법에 대한 규칙을 정의하는 Kubernetes 리소스입니다. 또한 Ingress 리소스는 공용 라우트에 추가되어 `mycluster.us-south.containers.appdomain.cloud/myapp`과 같은 고유 앱 URL을 형성하는 앱 서비스에 대한 경로를 지정합니다. <br></br><strong>참고</strong>: 2018년 5월 24일부터 새 클라우드의 Ingress 하위 도메인 형식이 변경되었습니다.<ul><li>2018년 5월 24일 이후에 작성된 클러스터에는 새 형식(<code>&lt;cluster_name&gt;.&lt;region&gt;.containers.appdomain.cloud</code>)의 하위 도메인이 지정됩니다.</li><li>2018년 5월 24일 이전에 작성된 클러스터는 이전 형식(<code>&lt;cluster_name&gt;.&lt;region&gt;.containers.mybluemix.net</code>)의 지정된 하위 도메인을 계속 사용합니다.</li></ul></dd>
</dl>

다음 다이어그램은 Ingress가 인터넷에서 앱으로 통신하는 방식을 표시합니다.

<img src="images/cs_ingress.png" width="550" alt="Ingress를 사용하여 {{site.data.keyword.containershort_notm}}의 앱 노출" style="width:550px; border-style: none"/>

1. 사용자는 앱의 URL에 액세스하여 요청을 앱에 전송합니다. 이 URL은 Ingress 리소스 경로가 포함된 노출된 앱의 공용 URL입니다(예: `mycluster.us-south.containers.appdomain.cloud/myapp`).

2. 글로벌 로드 밸런서의 역할을 하는 DNS 시스템 서비스는 클러스터에서 기본 공용 ALB의 포터블 공인 IP 주소에 대한 URL을 분석합니다. 요청이 앱의 Kubernetes ALB 서비스로 라우팅됩니다.

3. Kubernetes 서비스는 ALB에 요청을 라우팅합니다.

4. ALB는 클러스터에서 `myapp` 경로에 대한 라우팅 규칙이 존재하는지 여부를 확인합니다. 일치하는 규칙이 발견된 경우에는 Ingress 리소스에서 정의한 규칙에 따라 요청이 앱이 배치된 팟(Pod)에 전달됩니다. 다중 앱 인스턴스가 클러스터에 배치되는 경우 ALB는 앱 팟(Pod) 간의 요청을 로드 밸런싱합니다.



<br />


## 전제조건
{: #config_prereqs}

Ingress를 시작하기 전에 다음 전제조건을 검토하십시오.
{:shortdesc}

**모든 Ingress 구성에 대한 전제조건:**
- Ingress는 표준 클러스터에만 사용 가능하며 고가용성을 보장할 수 있도록 클러스터에 두 개 이상의 작업자 노드가 필요하고 주기적 업데이트가 적용되어야 합니다.
- Ingress를 설정하려면 [관리자 액세스 정책](cs_users.html#access_policies)이 필요합니다. 현재 [액세스 정책](cs_users.html#infra_access)을 확인하십시오.



<br />


## 단일 또는 다중 네임스페이스에 대한 네트워킹 계획
{: #multiple_namespaces}

노출할 앱이 있는 네임스페이스당 하나 이상의 Ingress 리소스가 필요합니다.
{:shortdesc}

<dl>
<dt>모든 앱이 하나의 네임스페이스에 있음</dt>
<dd>클러스터의 앱이 모두 동일한 네임스페이스에 있는 경우 해당 네임스페이스에서 노출되는 앱에 대한 라우팅 규칙을 정의하려면 하나 이상의 Ingress 리소스가 필요합니다. 예를 들어, 개발 네임스페이스의 서비스에서 노출되는 `app1` 및 `app2`가 있는 경우 네임스페이스에 Ingress 리소스를 작성할 수 있습니다. 리소스는 `domain.net`을 호스트로 지정하고 각 앱이 청취하는 경로를 `domain.net`에 등록합니다.
<br></br><img src="images/cs_ingress_single_ns.png" width="300" alt="네임스페이스당 하나 이상의 리소스가 필요합니다." style="width:300px; border-style: none"/>
</dd>
<dt>앱이 다중 네임스페이스에 있음</dt>
<dd>클러스터의 앱이 여러 네임스페이스에 있는 경우 해당 네임스페이스에서 노출되는 앱에 대한 규칙을 정의하려면 네임스페이스당 하나 이상의 Ingress 리소스를 작성해야 합니다. 다중 Ingress 리소스를 클러스터의 Ingress ALB에 등록하려면 와일드카드 도메인을 사용해야 합니다. `*.mycluster.us-south.containers.appdomain.cloud`와 같은 와일드카드 도메인이 등록되면 다중 하위 도메인이 모두 동일한 호스트로 해석됩니다. 그런 다음, 각 네임스페이스에 Ingress 리소스를 작성하고 각 Ingress 리소스에 다른 하위 도메인을 지정할 수 있습니다.
<br><br>
예를 들어, 다음 시나리오를 고려하십시오.<ul>
<li>두 가지 버전의 테스트용 샘플 앱 `app1` 및 `app3`이 있습니다.</li>
<li>동일한 클러스터 내에 있는 두 개의 다른 네임스페이스에 앱을 배치합니다. 즉, `app1`을 개발 네임스페이스에 배치하고 `app3`을 스테이징 네임스페이스에 배치합니다.</li></ul>
동일한 클러스터 ALB를 사용하여 이러한 앱에 대한 트래픽을 관리하기 위해 다음 서비스와 리소스를 작성합니다.<ul>
<li>`app1`을 노출하기 위한 개발 네임스페이스의 Kubernetes 서비스</li>
<li>호스트를 `dev.mycluster.us-south.containers.appdomain.cloud`로 지정하는 개발 네임스페이스의 Ingress 리소스</li>
<li>`app3`을 노출하기 위한 스테이징 네임스페이스의 Kubernetes 서비스</li>
<li>호스트를 `stage.mycluster.us-south.containers.appdomain.cloud`로 지정하는 스테이징 네임스페이스의 Ingress 리소스</li></ul></br>
<img src="images/cs_ingress_multi_ns.png" alt="네임스페이스 내에서 하나 이상의 리소스에서 하위 도메인 사용" style="border-style: none"/>
이제 두 URL이 모두 동일한 도메인으로 해석되므로 둘 다 동일한 ALB에서 제공됩니다. 그러나 스테이징 네임스페이스의 리소스는 `stage` 하위 도메인에 등록되기 때문에 Ingress ALB가 `stage.mycluster.us-south.containers.appdomain.cloud/app3` URL의 요청을 `app3`으로만 올바르게 라우팅합니다.</dd>
</dl>

**참고**:
* IBM 제공 Ingress 하위 도메인 와일드카드, `*.<cluster_name>.<region>.containers.appdomain.cloud`가 클러스터에 기본적으로 등록됩니다. 그러나 IBM 제공 Ingress 하위 도메인 와일드카드에는 TLS가 지원되지 않습니다.
* 사용자 정의 도메인을 사용하려면 사용자 정의 도메인을 와일드카드 도메인(예: `*.custom_domain.net`)으로 등록해야 합니다. TLS를 사용하려면 와일드카드 인증서를 가져와야 합니다.

### 네임스페이스 내의 다중 도메인

개별 네임스페이스 내에서 하나의 도메인을 사용하여 네임스페이스의 모든 앱에 액세스할 수 있습니다. 개별 네임스페이스 내에서 앱에 대해 다른 도메인을 사용하려면 와일드카드 도메인을 사용하십시오. `*.mycluster.us-south.containers.appdomain.cloud`와 같은 와일드카드 도메인이 등록되면 다중 하위 도메인이 모두 동일한 호스트로 해석됩니다. 그러면 하나의 리소스를 사용하여 해당 리소스 내의 다중 하위 도메인을 지정할 수 있습니다. 또는 네임스페이스에 다중 Ingress 리소스를 작성하고 각 Ingress 리소스에 다른 하위 도메인을 지정할 수 있습니다.

<img src="images/cs_ingress_single_ns_multi_subs.png" alt="네임스페이스당 하나 이상의 리소스가 필요합니다." style="border-style: none"/>

**참고**:
* IBM 제공 Ingress 하위 도메인 와일드카드, `*.<cluster_name>.<region>.containers.appdomain.cloud`가 클러스터에 기본적으로 등록됩니다. 그러나 IBM 제공 Ingress 하위 도메인 와일드카드에는 TLS가 지원되지 않습니다.
* 사용자 정의 도메인을 사용하려면 사용자 정의 도메인을 와일드카드 도메인(예: `*.custom_domain.net`)으로 등록해야 합니다. TLS를 사용하려면 와일드카드 인증서를 가져와야 합니다.

<br />


## 클러스터 내부에 있는 앱을 공용으로 노출
{: #ingress_expose_public}

공용 Ingress ALB를 사용하여 클러스터 내부에 있는 앱을 공용으로 노출합니다.
{:shortdesc}

시작하기 전에:

-   Ingress [전제조건](#config_prereqs)을 검토하십시오.
-   클러스터가 아직 없으면 [표준 클러스터를 작성](cs_clusters.html#clusters_ui)하십시오.
-   클러스터에 [CLI를 대상으로 지정](cs_cli_install.html#cs_cli_configure)하고 `kubectl` 명령을 실행하십시오.

### 1단계: 앱 배치 및 앱 서비스 작성
{: #public_inside_1}

앱을 배치하고 앱 노출을 위한 Kubernetes 서비스를 작성하여 시작하십시오.
{: shortdesc}

1.  [클러스터에 앱을 배치](cs_app.html#app_cli)하십시오. 구성 파일의 메타데이터 섹션에서 배치에 레이블(예: `app: code`)을 추가했는지 확인하십시오. 이 레이블은 팟(Pod)을 Ingress 로드 밸런싱에 포함시킬 수 있도록 앱이 실행 중인 모든 팟(Pod)을 식별하는 데 필요합니다.

2.   노출할 각 앱에 대한 Kubernetes 서비스를 작성하십시오. Ingress 로드 밸런싱에서 클러스터 ALB에 의해 포함되려면 앱이 Kubernetes 서비스에 의해 노출되어야 합니다.
      1.  선호하는 편집기를 열고 `myapp_service.yaml`과 같은 이름의 서비스 구성 파일을 작성하십시오.
      2.  ALB가 노출할 앱에 대한 서비스를 정의하십시오.

          ```
          apiVersion: v1
          kind: Service
          metadata:
            name: myapp_service
          spec:
            selector:
              <selector_key>: <selector_value>
            ports:
             - protocol: TCP
               port: 8080
          ```
          {: codeblock}

          <table>
          <thead>
          <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> ALB 서비스 YAML 파일 컴포넌트 이해</th>
          </thead>
          <tbody>
          <tr>
          <td><code>selector</code></td>
          <td>사용자의 앱이 실행되는 팟(Pod)을 대상으로 지정하기 위해 사용할 레이블 키(<em>&lt;selector_key&gt;</em>) 및 값(<em>&lt;selector_value&gt;</em>) 쌍을 입력하십시오. 팟(Pod)을 대상으로 지정하여 서비스 로드 밸런싱에 포함시키려면 <em>&lt;selector_key&gt;</em> 및 <em>&lt;selector_value&gt;</em>가 배치 yaml의 <code>spec.template.metadata.labels</code> 섹션에 있는 키/값 쌍과 동일해야 합니다. </td>
           </tr>
           <tr>
           <td><code>port</code></td>
           <td>서비스가 청취하는 포트입니다.</td>
           </tr>
           </tbody></table>
      3.  변경사항을 저장하십시오.
      4.  클러스터에 서비스를 작성하십시오. 클러스터의 다중 네임스페이스에 앱이 배치되는 경우 서비스가 노출할 앱과 동일한 네임스페이스에 배치되는지 확인하십시오.

          ```
          kubectl apply -f myapp_service.yaml [-n <namespace>]
          ```
          {: pre}
      5.  노출할 모든 앱에 대해 이러한 단계를 반복하십시오.


### 2단계: 앱 도메인 및 TLS 종료 선택
{: #public_inside_2}

공용 ALB를 구성하는 경우 앱에 액세스하는 데 사용할 도메인과 TLS 종료를 사용할지 여부를 선택합니다.
{: shortdesc}

<dl>
<dt>도메인</dt>
<dd>IBM 제공 도메인(예: <code>mycluster-12345.us-south.containers.appdomain.cloud/myapp</code>)을 사용하여 인터넷에서 앱에 액세스할 수 있습니다. 대신 사용자 정의 도메인을 사용하려는 경우 사용자 정의 도메인을 IBM 제공 도메인 또는 ALB의 공인 IP 주소에 맵핑할 수 있습니다.</dd>
<dt>TLS 종료</dt>
<dd>ALB는 클러스터의 앱에 대한 HTTP 네트워크 트래픽을 로드 밸런싱합니다. 수신 HTTPS 연결도 로드 밸런싱하려면 네트워크 트래픽을 복호화하고 클러스터에 노출된 앱으로 복호화된 요청을 전달하도록 ALB를 구성할 수 있습니다. IBM 제공 Ingress 하위 도메인을 사용하는 경우 IBM 제공 TLS 인증서를 사용할 수 있습니다. 현재 IBM 제공 와일드카드 하위 도메인에는 TLS가 지원되지 않습니다. 사용자 정의 하위 도메인을 사용하는 경우 고유 TLS 인증서를 사용하여 TLS 종료를 관리할 수 있습니다.</dd>
</dl>

IBM 제공 Ingress 도메인을 사용하려면 다음을 수행하십시오.
1. 클러스터에 대한 세부사항을 가져오십시오. _&lt;cluster_name_or_ID&gt;_를 노출할 앱이 배치된 클러스터의 이름으로 대체하십시오.

    ```
    bx cs cluster-get <cluster_name_or_ID>
    ```
    {: pre}

    출력 예:

    ```
    Name:                   mycluster
    ID:                     18a61a63c6a94b658596ca93d087aad9
    State:                  normal
    Created:                2018-01-12T18:33:35+0000
    Location:               dal10
    Master URL:             https://169.xx.xxx.xxx:26268
    Ingress Subdomain:      mycluster-12345.us-south.containers.appdomain.cloud
    Ingress Secret:         <tls_secret>
    Workers:                3
    Version:                1.9.7
    Owner Email:            owner@email.com
    Monitoring Dashboard:   <dashboard_URL>
    ```
    {: screen}
2. **Ingress 하위 도메인** 필드에서 IBM 제공 도메인을 가져오십시오. TLS를 사용하려면 **Ingress 시크릿** 필드에서 IBM 제공 TLS 시크릿도 가져오십시오. **참고**: 와일드카드 하위 도메인을 사용하는 경우에는 TLS가 지원되지 않습니다.

사용자 정의 도메인을 사용하려면 다음을 수행하십시오.
1.    사용자 정의 도메인을 작성하십시오. 사용자 정의 도메인을 등록하려면 DNS(Domain Name Service) 제공자 또는 [{{site.data.keyword.Bluemix_notm}} DNS](/docs/infrastructure/dns/getting-started.html#getting-started-with-dns)를 통해 작업하십시오. 
      * Ingress에서 노출할 앱이 하나의 클러스터의 여러 네임스페이스에 있는 경우 사용자 정의 도메인을 와일드카드 도메인(예: `*.custom_domain.net`)으로 등록하십시오.

2.  수신 네트워크 트래픽을 IBM 제공 ALB로 라우팅하도록 사용자의 도메인을 구성하십시오. 다음 옵션 중에서 선택하십시오.
    -   표준 이름 레코드(CNAME)로서 IBM 제공 도메인을 지정하여 사용자 정의 도메인의 별명을 정의하십시오. IBM 제공 Ingress 도메인을 찾으려면 `bx cs cluster-get <cluster_name>`을 실행하고 **Ingress subdomain** 필드를 찾으십시오.
    -   IP 주소를 레코드로 추가하여 IBM 제공 ALB의 포터블 공인 IP 주소로 사용자 정의 도메인을 맵핑하십시오. ALB의 포터블 공인 IP 주소를 찾으려면 `bx cs alb-get <public_alb_ID>`를 실행하십시오.
3.   선택사항: TLS를 사용하려면 TLS 인증서와 키 시크릿을 가져오거나 작성하십시오. 와일드카드 도메인을 사용하는 경우 와일드카드 인증서를 가져오거나 작성해야 합니다.
      * TLS 인증서가 사용하려는 {{site.data.keyword.cloudcerts_long_notm}}에 저장된 경우, 다음 명령을 사용하여 클러스터에 연관 시크릿을 가져올 수 있습니다.
        ```
        bx cs alb-cert-deploy --secret-name <secret_name> --cluster <cluster_name_or_ID> --cert-crn <certificate_crn>
        ```
        {: pre}
      * 준비된 TLS 인증서가 없으면 다음 단계를 수행하십시오.
        1. PEM 형식으로 인코딩된 사용자의 도메인에 대한 TLS 인증서 및 키를 작성하십시오.
        2. TLS 인증서 및 키를 사용하는 시크릿을 작성하십시오. <em>&lt;tls_secret_name&gt;</em>을 Kubernetes 시크릿의 이름으로, <em>&lt;tls_key_filepath&gt;</em>를 사용자 정의 TLS 키 파일의 경로로, <em>&lt;tls_cert_filepath&gt;</em>를 사용자 정의 TLS 인증서 파일의 경로로 대체하십시오.
          ```
            kubectl create secret tls <tls_secret_name> --key <tls_key_filepath> --cert <tls_cert_filepath>
          ```
          {: pre}


### 3단계: Ingress 리소스 작성
{: #public_inside_3}

Ingress 리소스는 ALB에서 트래픽을 앱 서비스에 라우팅하는 데 사용하는 라우팅 규칙을 정의합니다.
{: shortdesc}

**참고:** 클러스터에 앱이 노출된 다중 네임스페이스가 있는 경우 네임스페이스당 하나 이상의 Ingress 리소스가 필요합니다.  그러나 각 네임스페이스는 다른 호스트를 사용해야 합니다. 와일드카드 도메인을 등록하고 각 리소스에 다른 하위 도메인을 지정해야 합니다. 자세한 정보는 [단일 또는 다중 네임스페이스에 대한 네트워킹 계획](#multiple_namespaces)을 참조하십시오.

1. 선호하는 편집기를 열고 `myingressresource.yaml`과 같은 이름의 Ingress 구성 파일을 작성하십시오.

2. IBM 제공 도메인 또는 사용자 정의 도메인을 사용하여 수신 네트워크 트래픽을 이전에 작성한 서비스로 라우팅하는 Ingress 리소스를 구성 파일에 정의하십시오. 

    TLS를 사용하지 않는 YAML 예:
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingressresource
    spec:
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    TLS를 사용하는 YAML 예:
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingressresource
    spec:
      tls:
      - hosts:
        - <domain>
        secretName: <tls_secret_name>
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code>tls/hosts</code></td>
    <td>TLS를 사용하려면 <em>&lt;domain&gt;</em>을 IBM 제공 Ingress 하위 도메인 또는 사용자 정의 도메인으로 대체하십시오.

    </br></br>
    <strong>참고:</strong><ul><li>하나의 클러스터에 있는 여러 네임스페이스의 서비스에서 앱을 노출한 경우 도메인의 시작 부분에 와일드카드 하위 도메인을 추가하십시오(예: `subdomain1.custom_domain.net` 또는 `subdomain1.mycluster.us-south.containers.appdomain.cloud`). 클러스터에 작성하는 각 리소스마다 고유한 하위 도메인을 사용하십시오.</li><li>Ingress 작성 중에 실패하지 않으려면 호스트에 &ast;를 사용하거나 호스트 특성을 비워 두지 마십시오.</li></ul></td>
    </tr>
    <tr>
    <td><code>tls/secretName</code></td>
    <td><ul><li>IBM 제공 Ingress 도메인을 사용하는 경우 <em>&lt;tls_secret_name&gt;</em>을 IBM 제공 Ingress 시크릿의 이름으로 대체하십시오.</li><li>사용자 정의 도메인을 사용하는 경우 <em>&lt;tls_secret_name&gt;</em>을 사용자 정의 TLS 인증서 및 키가 포함된, 이전에 작성한 시크릿으로 대체하십시오. 인증서를 {{site.data.keyword.cloudcerts_short}}에서 가져온 경우, <code>bx cs alb-cert-get --cluster <cluster_name_or_ID> --cert-crn <certificate_crn></code>을 실행하여 TLS 인증서와 연관된 시크릿을 볼 수 있습니다.</li><ul><td>
    </tr>
    <tr>
    <td><code>host</code></td>
    <td><em>&lt;domain&gt;</em>을 IBM 제공 Ingress 하위 도메인 또는 사용자 정의 도메인으로 대체하십시오.

    </br></br>
    <strong>참고:</strong><ul><li>하나의 클러스터에 있는 여러 네임스페이스의 서비스에서 앱을 노출한 경우 도메인의 시작 부분에 와일드카드 하위 도메인을 추가하십시오(예: `subdomain1.custom_domain.net` 또는 `subdomain1.mycluster.us-south.containers.appdomain.cloud`). 클러스터에 작성하는 각 리소스마다 고유한 하위 도메인을 사용하십시오.</li><li>Ingress 작성 중에 실패하지 않으려면 호스트에 &ast;를 사용하거나 호스트 특성을 비워 두지 마십시오.</li></ul></td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td><em>&lt;app_path&gt;</em>를 슬래시 또는 앱이 청취 중인 경로로 대체하십시오. 앱에 대한 고유 라우트를 작성하기 위해 경로가 IBM 제공 또는 사용자 정의 도메인에 추가됩니다. 이 라우트를 웹 브라우저에 입력하면 네트워크 트래픽이 애플리케이션 ALB로 라우팅됩니다. ALB는 연관된 서비스를 찾고 네트워크 트래픽을 이 서비스에 전송합니다. 그 후 이 서비스는 트래픽을 앱이 실행 중인 팟(Pod)에 전달합니다.
    </br></br>
         많은 앱은 특정 경로에서 청취하지는 않지만 루트 경로와 특정 포트를 사용합니다. 이 경우에는 루트 경로를 <code>/</code>로 정의하고 앱에 대한 개별 경로를 지정하지 마십시오.         예: <ul><li><code>http://domain/</code>의 경우 <code>/</code>를 경로로 입력하십시오.</li><li><code>http://domain/app1_path</code>의 경우 <code>/app1_path</code>를 경로로 입력하십시오.</li></ul>
    </br>
    <strong>팁:</strong> [재작성 어노테이션](cs_annotations.html#rewrite-path)을 사용하여 앱이 청취하는 경로와 다른 경로에서 청취하도록 Ingress를 구성할 수 있습니다.</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td><em>&lt;app1_service&gt;</em> 및 <em>&lt;app2_service&gt;</em> 등을 앱 노출을 위해 작성한 서비스의 이름으로 대체하십시오. 클러스터에 있는 여러 네임스페이스의 서비스에서 앱을 노출한 경우 동일한 네임스페이스에 있는 앱 서비스만 포함시키십시오. 노출할 앱이 있는 각 네임스페이스마다 하나의 Ingress 리소스를 작성해야 합니다.</td>
    </tr>
    <tr>
    <td><code>servicePort</code></td>
    <td>서비스가 청취하는 포트입니다. 앱에 대한 Kubernetes 서비스를 작성했을 때 정의한 동일한 포트를 사용하십시오.</td>
    </tr>
    </tbody></table>

3.  클러스터에 대한 Ingress 리소스를 작성하십시오. 리소스가 리소스에 정의한 앱 서비스와 동일한 네임스페이스에 배치되는지 확인하십시오.

    ```
    kubectl apply -f myingressresource.yaml -n <namespace>
    ```
    {: pre}
4.   Ingress 리소스가 작성되었는지 확인하십시오.

      ```
      kubectl describe ingress myingressresource
      ```
      {: pre}

      1. 이벤트의 메시지에 리소스 구성의 오류에 대한 설명이 있는 경우, 리소스 파일의 값을 변경한 후 파일을 리소스에 다시 적용하십시오.


Ingress 리소스가 앱 서비스와 동일한 네임스페이스에 작성됩니다. 이 네임스페이스의 앱이 클러스터의 Ingress ALB에 등록됩니다.

### 4단계: 인터넷에서 앱에 액세스
{: #public_inside_4}

웹 브라우저에서 액세스할 앱 서비스의 URL을 입력하십시오.

```
https://<domain>/<app1_path>
```
{: pre}

다중 앱을 노출한 경우 URL에 추가되는 경로를 변경하여 해당 앱에 액세스하십시오.

```
https://<domain>/<app2_path>
```
{: pre}

와일드카드 도메인을 사용하여 여러 네임스페이스의 앱을 노출하는 경우 각 하위 도메인을 통해 해당 앱에 액세스하십시오.

```
http://<subdomain1>.<domain>/<app1_path>
```
{: pre}

```
http://<subdomain2>.<domain>/<app1_path>
```
{: pre}


<br />


## 클러스터 외부에 있는 앱을 공용으로 노출
{: #external_endpoint}

클러스터 외부에 있는 앱을 공용 Ingress ALB 로드 밸런싱에 포함하여 공용으로 노출합니다. IBM 제공 또는 사용자 정의 도메인의 수신 공용 요청이 외부 앱으로 자동으로 전달됩니다.
{:shortdesc}

시작하기 전에:

-   Ingress [전제조건](#config_prereqs)을 검토하십시오.
-   클러스터가 아직 없으면 [표준 클러스터를 작성](cs_clusters.html#clusters_ui)하십시오.
-   클러스터에 [CLI를 대상으로 지정](cs_cli_install.html#cs_cli_configure)하고 `kubectl` 명령을 실행하십시오.
-   클러스터 로드 밸런싱에 포함시키고자 하는 외부 앱에 공인 IP 주소를 사용하여 액세스할 수 있는지 확인하십시오.

### 1단계: 앱 서비스 및 외부 엔드포인트 작성
{: #public_outside_1}

외부 앱을 노출할 Kubernetes 서비스를 작성하고 앱에 대한 Kubernetes 외부 엔드포인트를 구성하여 시작하십시오.
{: shortdesc}

1.  작성할 외부 엔드포인트로 수신 요청을 전달할 클러스터에 대한 Kubernetes 서비스를 작성하십시오.
    1.  선호하는 편집기를 열고 예를 들어, `myexternalservice.yaml`이라는 이름의 서비스 구성 파일을 작성하십시오.
    2.  ALB가 노출할 앱에 대한 서비스를 정의하십시오.

        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: myexternalservice
        spec:
          ports:
           - protocol: TCP
             port: 8080
        ```
        {: codeblock}

        <table>
        <caption>ALB 서비스 파일 컴포넌트 이해</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
        </thead>
        <tbody>
        <tr>
        <td><code>metadata/name</code></td>
        <td><em>&lt;myexternalservice&gt;</em>를 서비스의 이름으로 대체하십시오.<p>Kubernetes 리소스에 대해 작업할 때 [개인 정보 보호](cs_secure.html#pi)에 대해 자세히 알아보십시오.</p></td>
        </tr>
        <tr>
        <td><code>port</code></td>
        <td>서비스가 청취하는 포트입니다.</td>
        </tr></tbody></table>

    3.  변경사항을 저장하십시오.
    4.  클러스터에 대한 Kubernetes 서비스를 작성하십시오.

        ```
        kubectl apply -f myexternalservice.yaml
        ```
        {: pre}
2.  클러스터 로드 밸런싱에 포함시키고자 하는 앱의 외부 위치를 정의하는 Kubernetes 엔드포인트를 구성하십시오.
    1.  선호하는 편집기를 열고 예를 들어, `myexternalendpoint.yaml`이라는 이름의 엔드포인트 구성 파일을 작성하십시오.
    2.  외부 엔드포인트를 정의하십시오. 외부 앱에 액세스하기 위해 사용할 수 있는 모든 공인 IP 주소 및 포트를 포함하십시오.

        ```
        kind: Endpoints
        apiVersion: v1
        metadata:
          name: myexternalendpoint
        subsets:
          - addresses:
              - ip: <external_IP1>
              - ip: <external_IP2>
            ports:
              - port: <external_port>
        ```
        {: codeblock}

        <table>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
        </thead>
        <tbody>
        <tr>
        <td><code>name</code></td>
        <td><em>&lt;myexternalendpoint&gt;</em>를 이전에 작성한 Kubernetes 서비스의 이름으로 대체하십시오.</td>
        </tr>
        <tr>
        <td><code>ip</code></td>
        <td><em>&lt;external_IP&gt;</em>를 사용자의 외부 앱에 연결하기 위한 공인 IP 주소로 대체하십시오.</td>
         </tr>
         <td><code>port</code></td>
         <td><em>&lt;external_port&gt;</em>를 사용자의 외부 앱이 청취하는 포트로 대체하십시오.</td>
         </tbody></table>

    3.  변경사항을 저장하십시오.
    4.  클러스터에 대한 Kubernetes 엔드포인트를 작성하십시오.

        ```
        kubectl apply -f myexternalendpoint.yaml
        ```
        {: pre}

### 2단계: 앱 도메인 및 TLS 종료 선택
{: #public_outside_2}

공용 ALB를 구성하는 경우 앱에 액세스하는 데 사용할 도메인과 TLS 종료를 사용할지 여부를 선택합니다.
{: shortdesc}

<dl>
<dt>도메인</dt>
<dd>IBM 제공 도메인(예: <code>mycluster-12345.us-south.containers.appdomain.cloud/myapp</code>)을 사용하여 인터넷에서 앱에 액세스할 수 있습니다. 대신 사용자 정의 도메인을 사용하려는 경우 사용자 정의 도메인을 IBM 제공 도메인 또는 ALB의 공인 IP 주소에 맵핑할 수 있습니다.</dd>
<dt>TLS 종료</dt>
<dd>ALB는 클러스터의 앱에 대한 HTTP 네트워크 트래픽을 로드 밸런싱합니다. 수신 HTTPS 연결도 로드 밸런싱하려면 네트워크 트래픽을 복호화하고 클러스터에 노출된 앱으로 복호화된 요청을 전달하도록 ALB를 구성할 수 있습니다. IBM 제공 Ingress 하위 도메인을 사용하는 경우 IBM 제공 TLS 인증서를 사용할 수 있습니다. 현재 IBM 제공 와일드카드 하위 도메인에는 TLS가 지원되지 않습니다. 사용자 정의 하위 도메인을 사용하는 경우 고유 TLS 인증서를 사용하여 TLS 종료를 관리할 수 있습니다.</dd>
</dl>

IBM 제공 Ingress 도메인을 사용하려면 다음을 수행하십시오.
1. 클러스터에 대한 세부사항을 가져오십시오. _&lt;cluster_name_or_ID&gt;_를 노출할 앱이 배치된 클러스터의 이름으로 대체하십시오.

    ```
    bx cs cluster-get <cluster_name_or_ID>
    ```
    {: pre}

    출력 예:

    ```
    Name:                   mycluster
    ID:                     18a61a63c6a94b658596ca93d087aad9
    State:                  normal
    Created:                2018-01-12T18:33:35+0000
    Location:               dal10
    Master URL:             https://169.xx.xxx.xxx:26268
    Ingress Subdomain:      mycluster-12345.us-south.containers.appdomain.cloud
    Ingress Secret:         <tls_secret>
    Workers:                3
    Version:                1.9.7
    Owner Email:            owner@email.com
    Monitoring Dashboard:   <dashboard_URL>
    ```
    {: screen}
2. **Ingress 하위 도메인** 필드에서 IBM 제공 도메인을 가져오십시오. TLS를 사용하려면 **Ingress 시크릿** 필드에서 IBM 제공 TLS 시크릿도 가져오십시오. **참고**: 와일드카드 하위 도메인을 사용하는 경우에는 TLS가 지원되지 않습니다.

사용자 정의 도메인을 사용하려면 다음을 수행하십시오.
1.    사용자 정의 도메인을 작성하십시오. 사용자 정의 도메인을 등록하려면 DNS(Domain Name Service) 제공자 또는 [{{site.data.keyword.Bluemix_notm}} DNS](/docs/infrastructure/dns/getting-started.html#getting-started-with-dns)를 통해 작업하십시오. 
      * Ingress에서 노출할 앱이 하나의 클러스터의 여러 네임스페이스에 있는 경우 사용자 정의 도메인을 와일드카드 도메인(예: `*.custom_domain.net`)으로 등록하십시오.

2.  수신 네트워크 트래픽을 IBM 제공 ALB로 라우팅하도록 사용자의 도메인을 구성하십시오. 다음 옵션 중에서 선택하십시오.
    -   표준 이름 레코드(CNAME)로서 IBM 제공 도메인을 지정하여 사용자 정의 도메인의 별명을 정의하십시오. IBM 제공 Ingress 도메인을 찾으려면 `bx cs cluster-get <cluster_name>`을 실행하고 **Ingress subdomain** 필드를 찾으십시오.
    -   IP 주소를 레코드로 추가하여 IBM 제공 ALB의 포터블 공인 IP 주소로 사용자 정의 도메인을 맵핑하십시오. ALB의 포터블 공인 IP 주소를 찾으려면 `bx cs alb-get <public_alb_ID>`를 실행하십시오.
3.   선택사항: TLS를 사용하려면 TLS 인증서와 키 시크릿을 가져오거나 작성하십시오. 와일드카드 도메인을 사용하는 경우 와일드카드 인증서를 가져오거나 작성해야 합니다.
      * TLS 인증서가 사용하려는 {{site.data.keyword.cloudcerts_long_notm}}에 저장된 경우, 다음 명령을 사용하여 클러스터에 연관 시크릿을 가져올 수 있습니다.
        ```
          bx cs alb-cert-deploy --secret-name <secret_name> --cluster <cluster_name_or_ID> --cert-crn <certificate_crn>
        ```
        {: pre}
      * 준비된 TLS 인증서가 없으면 다음 단계를 수행하십시오.
        1. PEM 형식으로 인코딩된 사용자의 도메인에 대한 TLS 인증서 및 키를 작성하십시오.
        2. TLS 인증서 및 키를 사용하는 시크릿을 작성하십시오. <em>&lt;tls_secret_name&gt;</em>을 Kubernetes 시크릿의 이름으로, <em>&lt;tls_key_filepath&gt;</em>를 사용자 정의 TLS 키 파일의 경로로, <em>&lt;tls_cert_filepath&gt;</em>를 사용자 정의 TLS 인증서 파일의 경로로 대체하십시오.
          ```
            kubectl create secret tls <tls_secret_name> --key <tls_key_filepath> --cert <tls_cert_filepath>
          ```
          {: pre}


### 3단계: Ingress 리소스 작성
{: #public_outside_3}

Ingress 리소스는 ALB에서 트래픽을 앱 서비스에 라우팅하는 데 사용하는 라우팅 규칙을 정의합니다.
{: shortdesc}

**참고:** 다중 외부 앱을 노출하고 [1단계](#public_outside_1)에서 앱에 대해 작성한 서비스가 여러 네임스페이스에 있는 경우 네임스페이스당 하나 이상의 Ingress 리소스가 필요합니다. 그러나 각 네임스페이스는 다른 호스트를 사용해야 합니다. 와일드카드 도메인을 등록하고 각 리소스에 다른 하위 도메인을 지정해야 합니다. 자세한 정보는 [단일 또는 다중 네임스페이스에 대한 네트워킹 계획](#multiple_namespaces)을 참조하십시오.

1. 선호하는 편집기를 열고 예를 들어, `myexternalingress.yaml`이라는 이름의 Ingress 구성 파일을 작성하십시오.

2. IBM 제공 도메인 또는 사용자 정의 도메인을 사용하여 수신 네트워크 트래픽을 이전에 작성한 서비스로 라우팅하는 Ingress 리소스를 구성 파일에 정의하십시오. 

    TLS를 사용하지 않는 YAML 예:
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myexternalingress
    spec:
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<external_app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<external_app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    TLS를 사용하는 YAML 예:
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myexternalingress
    spec:
      tls:
      - hosts:
        - <domain>
        secretName: <tls_secret_name>
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<external_app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<external_app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code>tls/hosts</code></td>
    <td>TLS를 사용하려면 <em>&lt;domain&gt;</em>을 IBM 제공 Ingress 하위 도메인 또는 사용자 정의 도메인으로 대체하십시오.

    </br></br>
    <strong>참고:</strong><ul><li>앱 서비스가 클러스터의 여러 네임스페이스에 있는 경우 도메인의 시작 부분에 와일드카드 하위 도메인을 추가하십시오(예: `subdomain1.custom_domain.net` 또는 `subdomain1.mycluster.us-south.containers.appdomain.cloud`). 클러스터에 작성하는 각 리소스마다 고유한 하위 도메인을 사용하십시오.</li><li>Ingress 작성 중에 실패하지 않으려면 호스트에 &ast;를 사용하거나 호스트 특성을 비워 두지 마십시오.</li></ul></td>
    </tr>
    <tr>
    <td><code>tls/secretName</code></td>
    <td><ul><li>IBM 제공 Ingress 도메인을 사용하는 경우 <em>&lt;tls_secret_name&gt;</em>을 IBM 제공 Ingress 시크릿의 이름으로 대체하십시오.</li><li>사용자 정의 도메인을 사용하는 경우 <em>&lt;tls_secret_name&gt;</em>을 사용자 정의 TLS 인증서 및 키가 포함된, 이전에 작성한 시크릿으로 대체하십시오. 인증서를 {{site.data.keyword.cloudcerts_short}}에서 가져온 경우, <code>bx cs alb-cert-get --cluster <cluster_name_or_ID> --cert-crn <certificate_crn></code>을 실행하여 TLS 인증서와 연관된 시크릿을 볼 수 있습니다.</li><ul><td>
    </tr>
    <tr>
    <td><code>rules/host</code></td>
    <td><em>&lt;domain&gt;</em>을 IBM 제공 Ingress 하위 도메인 또는 사용자 정의 도메인으로 대체하십시오.

    </br></br>
    <strong>참고:</strong><ul><li>하나의 클러스터에 있는 여러 네임스페이스의 서비스에서 앱을 노출한 경우 도메인의 시작 부분에 와일드카드 하위 도메인을 추가하십시오(예: `subdomain1.custom_domain.net` 또는 `subdomain1.mycluster.us-south.containers.appdomain.cloud`). 클러스터에 작성하는 각 리소스마다 고유한 하위 도메인을 사용하십시오.</li><li>Ingress 작성 중에 실패하지 않으려면 호스트에 &ast;를 사용하거나 호스트 특성을 비워 두지 마십시오.</li></ul></td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td><em>&lt;external_app_path&gt;</em>를 슬래시 또는 앱이 청취 중인 경로로 대체하십시오. 앱에 대한 고유 라우트를 작성하기 위해 경로가 IBM 제공 또는 사용자 정의 도메인에 추가됩니다. 이 라우트를 웹 브라우저에 입력하면 네트워크 트래픽이 애플리케이션 ALB로 라우팅됩니다. ALB는 연관된 서비스를 찾고 네트워크 트래픽을 이 서비스에 전송합니다. 이 서비스는 트래픽을 외부 앱에 전달합니다. 앱이 수신 네트워크 트래픽을 수신하려면 이 경로를 청취하도록 설정되어야 합니다.
    </br></br>
         많은 앱은 특정 경로에서 청취하지는 않지만 루트 경로와 특정 포트를 사용합니다. 이 경우에는 루트 경로를 <code>/</code>로 정의하고 앱에 대한 개별 경로를 지정하지 마십시오.         예: <ul><li><code>http://domain/</code>의 경우 <code>/</code>를 경로로 입력하십시오.</li><li><code>http://domain/app1_path</code>의 경우 <code>/app1_path</code>를 경로로 입력하십시오.</li></ul>
    </br></br>
    <strong>팁:</strong> [재작성 어노테이션](cs_annotations.html#rewrite-path)을 사용하여 앱이 청취하는 경로와 다른 경로에서 청취하도록 Ingress를 구성할 수 있습니다.</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td><em>&lt;app1_service&gt;</em> 및 <em>&lt;app2_service&gt;</em>를 외부 앱 노출을 위해 작성한 서비스의 이름으로 대체하십시오. 클러스터에 있는 여러 네임스페이스의 서비스에서 앱을 노출한 경우 동일한 네임스페이스에 있는 앱 서비스만 포함시키십시오. 노출할 앱이 있는 각 네임스페이스마다 하나의 Ingress 리소스를 작성해야 합니다.</td>
    </tr>
    <tr>
    <td><code>servicePort</code></td>
    <td>서비스가 청취하는 포트입니다. 앱에 대한 Kubernetes 서비스를 작성했을 때 정의한 동일한 포트를 사용하십시오.</td>
    </tr>
    </tbody></table>

3.  클러스터에 대한 Ingress 리소스를 작성하십시오. 리소스가 리소스에 정의한 앱 서비스와 동일한 네임스페이스에 배치되는지 확인하십시오.

    ```
    kubectl apply -f myexternalingress.yaml -n <namespace>
    ```
    {: pre}
4.   Ingress 리소스가 작성되었는지 확인하십시오.

      ```
      kubectl describe ingress myingressresource
      ```
      {: pre}

      1. 이벤트의 메시지에 리소스 구성의 오류에 대한 설명이 있는 경우, 리소스 파일의 값을 변경한 후 파일을 리소스에 다시 적용하십시오.


Ingress 리소스가 앱 서비스와 동일한 네임스페이스에 작성됩니다. 이 네임스페이스의 앱이 클러스터의 Ingress ALB에 등록됩니다.

### 4단계: 인터넷에서 앱에 액세스
{: #public_outside_4}

웹 브라우저에서 액세스할 앱 서비스의 URL을 입력하십시오.

```
https://<domain>/<app1_path>
```
{: pre}

다중 앱을 노출한 경우 URL에 추가되는 경로를 변경하여 해당 앱에 액세스하십시오.

```
https://<domain>/<app2_path>
```
{: pre}

와일드카드 도메인을 사용하여 여러 네임스페이스의 앱을 노출하는 경우 각 하위 도메인을 통해 해당 앱에 액세스하십시오.

```
http://<subdomain1>.<domain>/<app1_path>
```
{: pre}

```
http://<subdomain2>.<domain>/<app1_path>
```
{: pre}


<br />


## 기본 사설 ALB 사용
{: #private_ingress}

표준 클러스터를 작성할 때 IBM 제공 사설 애플리케이션 로드 밸런서(ALB)가 작성되며 포터블 사설 IP 주소 및 사설 라우트가 지정됩니다. 그러나 기본 사설 ALB가 자동으로 사용으로 설정되지는 않습니다. 사설 ALB를 사용하여 앱에 대한 사설 네트워크 트래픽을 로드 밸런싱하려면 먼저 IBM 제공 포터블 사설 IP 주소 또는 고유 포터블 사설 IP 주소를 사용하여 사설 ALB를 사용으로 설정해야 합니다.
{:shortdesc}

**참고**: 클러스터를 작성했을 때 `--no-subnet` 플래그를 사용한 경우, 사설 ALB를 사용하려면 먼저 포터블 사설 서브넷 또는 사용자 관리 서브넷을 추가해야 합니다. 자세한 정보는 [클러스터의 추가 서브넷 요청](cs_subnets.html#request)을 참조하십시오.

시작하기 전에:

-   클러스터가 아직 없으면 [표준 클러스터를 작성](cs_clusters.html#clusters_ui)하십시오.
-   클러스터를 [CLI의 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오.

사전 지정된 IBM 제공 포터블 사설 IP 주소를 사용하여 사설 ALB를 사용으로 설정하려면 다음을 수행하십시오.

1. 클러스터에서 사용 가능한 ALB를 나열하여 사설 ALB의 ID를 가져오십시오. <em>&lt;cluser_name&gt;</em>을 노출할 앱이 배치된 클러스터의 이름으로 대체하십시오.

    ```
    bx cs albs --cluster <cluser_name>
    ```
    {: pre}

    사설 ALB의 **상태** 필드는 _disabled_입니다.
    ```
    ALB ID                                            Enabled   Status     Type      ALB IP
    private-cr6d779503319d419ea3b4ab171d12c3b8-alb1   false     disabled   private   -
    public-cr6d779503319d419ea3b4ab171d12c3b8-alb1    true      enabled    public    169.xx.xxx.xxx
    ```
    {: screen}

2. 사설 ALB를 사용으로 설정하십시오. 이전 단계의 출력에서 <em>&lt;private_ALB_ID&gt;</em>를 사설 ALB의 ID로 대체하십시오.

   ```
   bx cs alb-configure --albID <private_ALB_ID> --enable
   ```
   {: pre}

<br>
고유 포터블 사설 IP 주소를 사용하여 사설 ALB를 사용으로 설정하려면 다음을 수행하십시오.

1. 클러스터의 사설 VLAN에서 트래픽을 라우팅하도록 선택된 IP 주소의 사용자 관리 서브넷을 구성하십시오.

   ```
   bx cs cluster-user-subnet-add <cluster_name> <subnet_CIDR> <private_VLAN_ID>
   ```
   {: pre}

   <table>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 명령 컴포넌트 이해</th>
   </thead>
   <tbody>
   <tr>
   <td><code>&lt;cluser_name&gt;</code></td>
   <td>노출할 앱이 배치되는 클러스터의 이름 또는 ID입니다.</td>
   </tr>
   <tr>
   <td><code>&lt;subnet_CIDR&gt;</code></td>
   <td>사용자 관리 서브넷의 CIDR입니다.</td>
   </tr>
   <tr>
   <td><code>&lt;private_VLAN_ID&gt;</code></td>
   <td>사용 가능한 사설 VLAN ID입니다. `bx cs vlans`를 실행하여 사용 가능한 사설 VLAN의 ID를 찾을 수 있습니다.</td>
   </tr>
   </tbody></table>

2. 클러스터에서 사용 가능한 ALB를 나열하여 사설 ALB의 ID를 가져오십시오.

    ```
    bx cs albs --cluster <cluster_name>
    ```
    {: pre}

    사설 ALB의 **상태** 필드는 _disabled_입니다.
    ```
    ALB ID                                            Enabled   Status     Type      ALB IP
    private-cr6d779503319d419ea3b4ab171d12c3b8-alb1   false     disabled   private   -
    public-cr6d779503319d419ea3b4ab171d12c3b8-alb1    true      enabled    public    169.xx.xxx.xxx
    ```
    {: screen}

3. 사설 ALB를 사용으로 설정하십시오. <em>&lt;private_ALB_ID&gt;</em>를 이전 단계의 출력의 개인용 ALB ID로 대체하고 <em>&lt;user_IP&gt;</em>를 사용할 사용자 관리 서브넷의 IP 주소로 대체하십시오.

   ```
   bx cs alb-configure --albID <private_ALB_ID> --enable --user-ip <user_IP>
   ```
   {: pre}

<br />


## 사설 네트워크에 앱 노출
{: #ingress_expose_private}

사설 Ingress ALB를 사용하여 앱을 사설 네트워크에 노출합니다.
{:shortdesc}

시작하기 전에:
* Ingress [전제조건](#config_prereqs)을 검토하십시오.
* [개인용 애플리케이션 로드 밸런서를 사용할 수 있도록 설정하십시오](#private_ingress).
* 개인용 작업자 노드가 있으며 외부 DNS 제공자를 사용하려는 경우에는 [공용 액세스 권한을 설정하도록 에지 노드 구성](cs_edge.html#edge) 및 [가상 라우터 어플라이언스 구성 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/blogs/bluemix/2017/07/kubernetes-and-bluemix-container-based-workloads-part4/)을 수행해야 합니다. 
* 개인용 작업자 노드가 있으며 사설 네트워크에만 남아 있으려면 앱에 대한 URL 요청을 분석할 수 있도록 [사설 온프레미스 DNS 서비스를 구성 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/)해야 합니다.

### 1단계: 앱 배치 및 앱 서비스 작성
{: #private_1}

앱을 배치하고 앱 노출을 위한 Kubernetes 서비스를 작성하여 시작하십시오.
{: shortdesc}

1.  [클러스터에 앱을 배치](cs_app.html#app_cli)하십시오. 구성 파일의 메타데이터 섹션에서 배치에 레이블(예: `app: code`)을 추가했는지 확인하십시오. 이 레이블은 팟(Pod)을 Ingress 로드 밸런싱에 포함시킬 수 있도록 앱이 실행 중인 모든 팟(Pod)을 식별하는 데 필요합니다.

2.   노출할 각 앱에 대한 Kubernetes 서비스를 작성하십시오. Ingress 로드 밸런싱에서 클러스터 ALB에 의해 포함되려면 앱이 Kubernetes 서비스에 의해 노출되어야 합니다.
      1.  선호하는 편집기를 열고 `myapp_service.yaml`과 같은 이름의 서비스 구성 파일을 작성하십시오.
      2.  ALB가 노출할 앱에 대한 서비스를 정의하십시오.

          ```
          apiVersion: v1
          kind: Service
          metadata:
            name: myapp_service
          spec:
            selector:
              <selector_key>: <selector_value>
            ports:
             - protocol: TCP
               port: 8080
          ```
          {: codeblock}

          <table>
          <thead>
          <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> ALB 서비스 YAML 파일 컴포넌트 이해</th>
          </thead>
          <tbody>
          <tr>
          <td><code>selector</code></td>
          <td>사용자의 앱이 실행되는 팟(Pod)을 대상으로 지정하기 위해 사용할 레이블 키(<em>&lt;selector_key&gt;</em>) 및 값(<em>&lt;selector_value&gt;</em>) 쌍을 입력하십시오. 팟(Pod)을 대상으로 지정하여 서비스 로드 밸런싱에 포함시키려면 <em>&lt;selector_key&gt;</em> 및 <em>&lt;selector_value&gt;</em>가 배치 yaml의 <code>spec.template.metadata.labels</code> 섹션에 있는 키/값 쌍과 동일해야 합니다. </td>
           </tr>
           <tr>
           <td><code>port</code></td>
           <td>서비스가 청취하는 포트입니다.</td>
           </tr>
           </tbody></table>
      3.  변경사항을 저장하십시오.
      4.  클러스터에 서비스를 작성하십시오. 클러스터의 다중 네임스페이스에 앱이 배치되는 경우 서비스가 노출할 앱과 동일한 네임스페이스에 배치되는지 확인하십시오.

          ```
          kubectl apply -f myapp_service.yaml [-n <namespace>]
          ```
          {: pre}
      5.  노출할 모든 앱에 대해 이러한 단계를 반복하십시오.


### 2단계: 사용자 정의 도메인 맵핑 및 TLS 종료 선택
{: #private_2}

사설 ALB를 구성하는 경우 앱에 액세스할 수 있는 사용자 정의 도메인을 사용하고 TLS 종료를 사용할지 여부를 선택합니다.
{: shortdesc}

ALB는 앱에 대한 HTTP 네트워크 트래픽을 로드 밸런싱합니다. 수신 HTTPS 연결도 로드 밸런싱하려면 고유 TLS 인증서를 사용하여 네트워크 트래픽을 복호화하도록 ALB를 구성할 수 있습니다. 그런 다음 ALB가 클러스터에 노출된 앱으로 복호화된 요청을 전달합니다.
1.   사용자 정의 도메인을 작성하십시오. 사용자 정의 도메인을 등록하려면 DNS(Domain Name Service) 제공자 또는 [{{site.data.keyword.Bluemix_notm}} DNS](/docs/infrastructure/dns/getting-started.html#getting-started-with-dns)를 통해 작업하십시오. 
      * Ingress에서 노출할 앱이 하나의 클러스터의 여러 네임스페이스에 있는 경우 사용자 정의 도메인을 와일드카드 도메인(예: `*.custom_domain.net`)으로 등록하십시오.

2. IP 주소를 레코드로 추가하여 IBM 제공 사설 ALB의 포터블 사설 IP 주소로 사용자 정의 도메인을 맵핑하십시오. 사설 ALB의 포터블 사설 IP 주소를 찾으려면 `bx cs albs --cluster <cluster_name>`을 실행하십시오.
3.   선택사항: TLS를 사용하려면 TLS 인증서와 키 시크릿을 가져오거나 작성하십시오. 와일드카드 도메인을 사용하는 경우 와일드카드 인증서를 가져오거나 작성해야 합니다.
      * TLS 인증서가 사용하려는 {{site.data.keyword.cloudcerts_long_notm}}에 저장된 경우, 다음 명령을 사용하여 클러스터에 연관 시크릿을 가져올 수 있습니다.
        ```
          bx cs alb-cert-deploy --secret-name <secret_name> --cluster <cluster_name_or_ID> --cert-crn <certificate_crn>
        ```
        {: pre}
      * 준비된 TLS 인증서가 없으면 다음 단계를 수행하십시오.
        1. PEM 형식으로 인코딩된 사용자의 도메인에 대한 TLS 인증서 및 키를 작성하십시오.
        2. TLS 인증서 및 키를 사용하는 시크릿을 작성하십시오. <em>&lt;tls_secret_name&gt;</em>을 Kubernetes 시크릿의 이름으로, <em>&lt;tls_key_filepath&gt;</em>를 사용자 정의 TLS 키 파일의 경로로, <em>&lt;tls_cert_filepath&gt;</em>를 사용자 정의 TLS 인증서 파일의 경로로 대체하십시오.
          ```
            kubectl create secret tls <tls_secret_name> --key <tls_key_filepath> --cert <tls_cert_filepath>
          ```
          {: pre}


### 3단계: Ingress 리소스 작성
{: #pivate_3}

Ingress 리소스는 ALB에서 트래픽을 앱 서비스에 라우팅하는 데 사용하는 라우팅 규칙을 정의합니다.
{: shortdesc}

**참고:** 클러스터에 앱이 노출된 다중 네임스페이스가 있는 경우 네임스페이스당 하나 이상의 Ingress 리소스가 필요합니다.  그러나 각 네임스페이스는 다른 호스트를 사용해야 합니다. 와일드카드 도메인을 등록하고 각 리소스에 다른 하위 도메인을 지정해야 합니다. 자세한 정보는 [단일 또는 다중 네임스페이스에 대한 네트워킹 계획](#multiple_namespaces)을 참조하십시오.

1. 선호하는 편집기를 열고 `myingressresource.yaml`과 같은 이름의 Ingress 구성 파일을 작성하십시오.

2.  사용자 정의 도메인을 사용하여 수신 네트워크 트래픽을 이전에 작성한 서비스로 라우팅하는 Ingress 리소스를 구성 파일에 정의하십시오. 

    TLS를 사용하지 않는 YAML 예:
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingressresource
      annotations:
        ingress.bluemix.net/ALB-ID: "<private_ALB_ID>"
    spec:
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    TLS를 사용하는 YAML 예:
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingressresource
      annotations:
        ingress.bluemix.net/ALB-ID: "<private_ALB_ID>"
    spec:
      tls:
      - hosts:
        - <domain>
        secretName: <tls_secret_name>
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code>ingress.bluemix.net/ALB-ID</code></td>
    <td><em>&lt;private_ALB_ID&gt;</em>를 사설 ALB의 ID로 대체하십시오. ALB ID를 찾으려면 <code>bx cs albs --cluster <my_cluster></code>를 실행하십시오. 이 Ingress 어노테이션에 대한 자세한 정보는 [사설 애플리케이션 로드 밸런서 라우팅](cs_annotations.html#alb-id)을 참조하십시오.</td>
    </tr>
    <tr>
    <td><code>tls/hosts</code></td>
    <td>TLS를 사용하려면 <em>&lt;domain&gt;</em>을 사용자 정의 도메인으로 대체하십시오.

    </br></br>
    <strong>참고:</strong><ul><li>하나의 클러스터에 있는 여러 네임스페이스의 서비스에서 앱을 노출한 경우 도메인의 시작 부분에 와일드카드 하위 도메인을 추가하십시오(예: `subdomain1.custom_domain.net`). 클러스터에 작성하는 각 리소스마다 고유한 하위 도메인을 사용하십시오.</li><li>Ingress 작성 중에 실패하지 않으려면 호스트에 &ast;를 사용하거나 호스트 특성을 비워 두지 마십시오.</li></ul></td>
    </tr>
    <tr>
    <td><code>tls/secretName</code></td>
    <td><em>&lt;tls_secret_name&gt;</em>을 사용자 정의 TLS 인증서 및 키가 포함된, 이전에 작성한 시크릿의 이름으로 대체하십시오. 인증서를 {{site.data.keyword.cloudcerts_short}}에서 가져온 경우, <code>bx cs alb-cert-get --cluster <cluster_name_or_ID> --cert-crn <certificate_crn></code>을 실행하여 TLS 인증서와 연관된 시크릿을 볼 수 있습니다.
    </tr>
    <tr>
    <td><code>host</code></td>
    <td><em>&lt;domain&gt;</em>을 사용자 정의 도메인으로 대체하십시오.
    </br></br>
    <strong>참고:</strong><ul><li>하나의 클러스터에 있는 여러 네임스페이스의 서비스에서 앱을 노출한 경우 도메인의 시작 부분에 와일드카드 하위 도메인을 추가하십시오(예: `subdomain1.custom_domain.net`). 클러스터에 작성하는 각 리소스마다 고유한 하위 도메인을 사용하십시오.</li><li>Ingress 작성 중에 실패하지 않으려면 호스트에 &ast;를 사용하거나 호스트 특성을 비워 두지 마십시오.</li></ul></td>
    </td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td><em>&lt;app_path&gt;</em>를 슬래시 또는 앱이 청취 중인 경로로 대체하십시오. 앱에 대한 고유 라우트를 작성하기 위해 경로가 사용자 정의 도메인에 추가됩니다. 이 라우트를 웹 브라우저에 입력하면 네트워크 트래픽이 애플리케이션 ALB로 라우팅됩니다. ALB는 연관된 서비스를 찾고 네트워크 트래픽을 이 서비스에 전송합니다. 그 후 이 서비스는 트래픽을 앱이 실행 중인 팟(Pod)에 전달합니다.
    </br></br>
         많은 앱은 특정 경로에서 청취하지는 않지만 루트 경로와 특정 포트를 사용합니다. 이 경우에는 루트 경로를 <code>/</code>로 정의하고 앱에 대한 개별 경로를 지정하지 마십시오.         예: <ul><li><code>http://domain/</code>의 경우 <code>/</code>를 경로로 입력하십시오.</li><li><code>http://domain/app1_path</code>의 경우 <code>/app1_path</code>를 경로로 입력하십시오.</li></ul>
    </br>
    <strong>팁:</strong> [재작성 어노테이션](cs_annotations.html#rewrite-path)을 사용하여 앱이 청취하는 경로와 다른 경로에서 청취하도록 Ingress를 구성할 수 있습니다.</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td><em>&lt;app1_service&gt;</em> 및 <em>&lt;app2_service&gt;</em> 등을 앱 노출을 위해 작성한 서비스의 이름으로 대체하십시오. 클러스터에 있는 여러 네임스페이스의 서비스에서 앱을 노출한 경우 동일한 네임스페이스에 있는 앱 서비스만 포함시키십시오. 노출할 앱이 있는 각 네임스페이스마다 하나의 Ingress 리소스를 작성해야 합니다.</td>
    </tr>
    <tr>
    <td><code>servicePort</code></td>
    <td>서비스가 청취하는 포트입니다. 앱에 대한 Kubernetes 서비스를 작성했을 때 정의한 동일한 포트를 사용하십시오.</td>
    </tr>
    </tbody></table>

3.  클러스터에 대한 Ingress 리소스를 작성하십시오. 리소스가 리소스에 정의한 앱 서비스와 동일한 네임스페이스에 배치되는지 확인하십시오.

    ```
    kubectl apply -f myingressresource.yaml -n <namespace>
    ```
    {: pre}
4.   Ingress 리소스가 작성되었는지 확인하십시오.

      ```
      kubectl describe ingress myingressresource
      ```
      {: pre}

      1. 이벤트의 메시지에 리소스 구성의 오류에 대한 설명이 있는 경우, 리소스 파일의 값을 변경한 후 파일을 리소스에 다시 적용하십시오.


Ingress 리소스가 앱 서비스와 동일한 네임스페이스에 작성됩니다. 이 네임스페이스의 앱이 클러스터의 Ingress ALB에 등록됩니다.

### 4단계: 사설 네트워크에서 앱에 액세스
{: #private_4}

사설 네트워크 방화벽 내에서 웹 브라우저에 앱 서비스의 URL을 입력하십시오.

```
https://<domain>/<app1_path>
```
{: pre}

다중 앱을 노출한 경우 URL에 추가되는 경로를 변경하여 해당 앱에 액세스하십시오.

```
https://<domain>/<app2_path>
```
{: pre}

와일드카드 도메인을 사용하여 여러 네임스페이스의 앱을 노출하는 경우 각 하위 도메인을 통해 해당 앱에 액세스하십시오.

```
http://<subdomain1>.<domain>/<app1_path>
```
{: pre}

```
http://<subdomain2>.<domain>/<app1_path>
```
{: pre}


TLS를 사용하는 사설 ALB를 통해 클러스터에서 마이크로서비스 대 마이크로서비스 통신을 보호하는 방법에 대한 종합적인 튜토리얼은 [이 블로그 게시물 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://medium.com/ibm-cloud/secure-microservice-to-microservice-communication-across-kubernetes-clusters-using-a-private-ecbe2a8d4fe2)을 확인하십시오.

<br />


## 선택적 애플리케이션 로드 밸런서 구성
{: #configure_alb}

다음 옵션을 사용하여 애플리케이션 로드 밸런서를 추가로 구성할 수 있습니다.

-   [Ingress 애플리케이션 로드 밸런서에서 포트 열기](#opening_ingress_ports)
-   [HTTP 레벨에서 SSL 프로토콜 및 SSL 암호 구성](#ssl_protocols_ciphers)
-   [Ingress 로그 형식 사용자 정의](#ingress_log_format)
-   [어노테이션으로 애플리케이션 로드 밸런서 사용자 정의](cs_annotations.html)
{: #ingress_annotation}


### Ingress 애플리케이션 로드 밸런서에서 포트 열기
{: #opening_ingress_ports}

기본적으로 포트 80과 443만 Ingress ALB에 노출됩니다. 다른 포트를 노출하려는 경우 `ibm-cloud-provider-ingress-cm` configmap 리소스를 편집할 수 있습니다.
{:shortdesc}

1. `ibm-cloud-provider-ingress-cm` configmap 리소스에 대한 구성 파일의 로컬 버전을 작성하고 여십시오.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. <code>data</code> 섹션을 추가하고 공용 포트 `80`, `443`과 그 외에 노출시킬 기타 포트를 세미콜론(;)으로 구분하여 지정하십시오.

    **중요**: 기본적으로 포트 80 및 443이 열립니다. 80 및 443을 열린 상태로 유지하려면 `public-ports` 필드에 지정하는 다른 포트 이외에 이러한 포트도 포함해야 합니다. 지정되지 않은 포트는 닫힙니다. 사설 ALB를 사용으로 설정한 경우 `private-ports` 필드에도 열린 상태로 유지할 포트를 지정해야 합니다.

    ```
    apiVersion: v1
    data:
      public-ports: "80;443;<port3>"
      private-ports: "80;443;<port4>"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

    `80`, `443` 및 `9443`을 열린 상태로 유지하는 예:
    ```
 apiVersion: v1
 data:
   public-ports: "80;443;9443"
 kind: ConfigMap
 metadata:
   name: ibm-cloud-provider-ingress-cm
   namespace: kube-system
    ```
    {: screen}

3. 구성 파일을 저장하십시오.

4. configmap 변경사항이 적용되었는지 확인하십시오.

 ```
 kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
 ```
 {: pre}

 출력:
 ```
 Name:        ibm-cloud-provider-ingress-cm
 Namespace:    kube-system
 Labels:        <none>
 Annotations:    <none>

 Data
 ====

  public-ports: "80;443;9443"
 ```
 {: screen}

configmap 리소스에 대한 자세한 정보는 [Kubernetes 문서](https://kubernetes-v1-4.github.io/docs/user-guide/configmap/)를 참조하십시오.

### HTTP 레벨에서 SSL 프로토콜과 SSL 암호 구성
{: #ssl_protocols_ciphers}

`ibm-cloud-provider-ingress-cm` configmap을 편집하여 글로벌 HTTP 레벨에서 SSL 프로토콜과 암호를 사용으로 설정하십시오.
{:shortdesc}

기본적으로 TLS 1.2 프로토콜은 IBM 제공 도메인을 사용하는 모든 Ingress 구성에 사용됩니다. 다음 단계에 따라 대신 TLS 1.1 또는 1.0 프로토콜을 사용하도록 기본값을 대체할 수 있습니다.

**참고**: 모든 호스트에 사용으로 설정된 프로토콜을 지정할 때 TLSv1.1 및 TLSv1.2 매개변수(1.1.13, 1.0.12)는 OpenSSL 1.0.1 이상을 사용하는 경우에만 작동합니다. TLSv1.3 매개변수(1.13.0)는 TLSv1.3 지원으로 빌드된 OpenSSL 1.1.1을 사용하는 경우에만 작동합니다.

configmap을 편집하여 SSL 프로토콜 및 암호를 사용으로 설정하려면 다음을 수행하십시오.

1. `ibm-cloud-provider-ingress-cm` configmap 리소스에 대한 구성 파일의 로컬 버전을 작성하고 여십시오.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. SSL 프로토콜 및 암호를 추가하십시오. [OpenSSL 라이브러리 암호 목록 형식 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.openssl.org/docs/man1.0.2/apps/ciphers.html)에 따라 암호를 형식화하십시오.

   ```
 apiVersion: v1
 data:
   ssl-protocols: "TLSv1 TLSv1.1 TLSv1.2"
   ssl-ciphers: "HIGH:!aNULL:!MD5"
 kind: ConfigMap
 metadata:
   name: ibm-cloud-provider-ingress-cm
   namespace: kube-system
   ```
   {: codeblock}

3. 구성 파일을 저장하십시오.

4. configmap 변경사항이 적용되었는지 확인하십시오.

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}

   출력:
   ```
 Name:        ibm-cloud-provider-ingress-cm
 Namespace:    kube-system
 Labels:        <none>
 Annotations:    <none>

   Data
 ====

    ssl-protocols: "TLSv1 TLSv1.1 TLSv1.2"
  ssl-ciphers: "HIGH:!aNULL:!MD5"
   ```
   {: screen}

### Ingress 로그 컨텐츠 및 형식 사용자 정의
{: #ingress_log_format}

Ingress ALB에 대해 수집되는 로그의 컨텐츠 및 형식을 사용자 정의할 수 있습니다.
{:shortdesc}

기본적으로 Ingress 로그는 JSON으로 형식화되며 일반적인 로그 필드를 표시합니다. 그러나 사용자가 사용자 정의 로그 형식을 작성할 수도 있습니다. 전달되는 로그 컴포넌트와 컴포넌트가 로그 출력에서 배열되는 방식을 선택하려면 다음을 수행하십시오.

1. `ibm-cloud-provider-ingress-cm` configmap 리소스에 대한 구성 파일의 로컬 버전을 작성하고 여십시오.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. <code>data</code> 섹션을 추가하십시오. `log-format` 필드를 추가하고 선택적으로 `log-format-escape-json` 필드를 추가하십시오.

    ```
    apiVersion: v1
    data:
      log-format: '{<key1>: <log_variable1>, <key2>: <log_variable2>, <key3>: <log_variable3>}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: pre}

    <table>
    <caption>YAML 파일 컴포넌트</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> log-format 구성 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code>log-format</code></td>
    <td><code>&lt;key&gt;</code>를 로그 컴포넌트의 이름으로 대체하고 <code>&lt;log_variable&gt;</code>을 로그 항목에서 수집할 로그 컴포넌트에 대한 변수로 대체하십시오. 문자열 값 앞뒤의 따옴표나 로그 컴포넌트 구분을 위한 쉼표와 같이 로그 항목이 포함하도록 할 텍스트 및 구두점을 포함시킬 수 있습니다. 예를 들면, <code>request: "$request",</code>와 같은 컴포넌트의 형식은 로그 항목에 <code>request: "GET / HTTP/1.1",</code>과 같은 항목을 생성합니다. 사용할 수 있는 모든 변수의 목록은 <a href="http://nginx.org/en/docs/varindex.html">Nginx 변수 색인</a>을 참조하십시오.<br><br><em>x-custom-ID</em>와 같은 추가 헤더를 로그하려면 사용자 정의 로그 컨텐츠에 다음 키-값 쌍을 추가하십시오. <br><pre class="pre"><code>customID: $http_x_custom_id</code></pre> <br>하이픈(<code>-</code>)이 밑줄(<code>_</code>)로 변환되며 <code>$http_</code>가 사용자 정의 헤더 이름 앞에 추가되어야 합니다.</td>
    </tr>
    <tr>
    <td><code>log-format-escape-json</code></td>
    <td>선택사항: 기본적으로 로그는 텍스트 형식으로 생성됩니다. JSON 형식으로 로그를 생성하려면 <code>log-format-escape-json</code> 필드를 추가하고 값 <code>true</code>를 사용하십시오.</td>
    </tr>
    </tbody></table>
    </dd>
    </dl>

    예를 들면, 로그 형식이 다음 변수를 포함할 수 있습니다.
    ```
    apiVersion: v1
    data:
      log-format: '{remote_address: $remote_addr, remote_user: "$remote_user",
                    time_date: [$time_local], request: "$request",
                    status: $status, http_referer: "$http_referer",
                    http_user_agent: "$http_user_agent",
                    request_id: $request_id}'
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: screen}

    이 형식을 따르는 로그 항목은 다음 예와 같습니다.
    ```
    remote_address: 127.0.0.1, remote_user: "dbmanager", time_date: [30/Mar/2018:18:52:17 +0000], request: "GET / HTTP/1.1", status: 401, http_referer: "-", http_user_agent: "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:47.0) Gecko/20100101 Firefox/47.0", request_id: a02b2dea9cf06344a25611c1d7ad72db
    ```
    {: screen}

    ALB 로그의 기본 형식을 기반으로 하는 사용자 정의 로그 형식을 작성하려면 필요에 따라 다음 섹션을 수정하여 configmap에 추가하십시오.
    ```
    apiVersion: v1
    data:
      log-format: '{"time_date": "$time_iso8601", "client": "$remote_addr",
                    "host": "$http_host", "scheme": "$scheme",
                    "request_method": "$request_method", "request_uri": "$uri",
                    "request_id": "$request_id", "status": $status,
                    "upstream_addr": "$upstream_addr", "upstream_status":
                    $upstream_status, "request_time": $request_time,
                    "upstream_response_time": $upstream_response_time,
                    "upstream_connect_time": $upstream_connect_time,
                    "upstream_header_time": $upstream_header_time}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

4. 구성 파일을 저장하십시오.

5. configmap 변경사항이 적용되었는지 확인하십시오.

 ```
 kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
 ```
 {: pre}

 출력 예:
 ```
 Name:        ibm-cloud-provider-ingress-cm
 Namespace:    kube-system
 Labels:        <none>
 Annotations:    <none>

 Data
 ====

  log-format: '{remote_address: $remote_addr, remote_user: "$remote_user", time_date: [$time_local], request: "$request", status: $status, http_referer: "$http_referer", http_user_agent: "$http_user_agent", request_id: $request_id}'
  log-format-escape-json: "true"
 ```
 {: screen}

4. Ingress ALB 로그를 보려면 클러스터에서 [Ingress 서비스에 대한 로깅 구성을 작성하십시오](cs_health.html#logging).

<br />



