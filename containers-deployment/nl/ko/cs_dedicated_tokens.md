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





# {{site.data.keyword.Bluemix_dedicated_notm}} 이미지 레지스트리에 대한 {{site.data.keyword.registryshort_notm}} 토큰 작성
{: #cs_dedicated_tokens}

{{site.data.keyword.containerlong}}에서 클러스터가 있는 단일 및 확장 가능 그룹에 대해 사용한 이미지 레지스트리에 대한 만료되지 않는 토큰을 작성합니다.
{:shortdesc}

1.  {{site.data.keyword.Bluemix_dedicated_notm}} 환경에 로그인하십시오.

    ```
    bx login -a api.<dedicated_domain>
    ```
    {: pre}

2.  현재 세션에 대한 `oauth-token`을 요청하고 이를 변수로서 저장하십시오.

    ```
    OAUTH_TOKEN=`bx iam oauth-tokens | awk 'FNR == 2 {print $3 " " $4}'`
    ```
    {: pre}

3.  현재 세션에 대한 조직의 ID를 요청하고 이를 변수로 저장하십시오.

    ```
    ORG_GUID=`bx iam org <org_name> --guid`
    ```
    {: pre}

4.  현재 세션에 대한 영구 레지스트리 토큰을 요청하십시오. `<dedicated_domain>`을 {{site.data.keyword.Bluemix_dedicated_notm}} 환경에 대한 도메인으로 대체하십시오. 이 토큰은 현재 네임스페이스의 이미지에 대한 액세스 권한을 부여합니다.

    ```
    curl -XPOST -H "Authorization: ${OAUTH_TOKEN}" -H "Organization: ${ORG_GUID}" https://registry.<dedicated_domain>/api/v1/tokens?permanent=true
    ```
    {: pre}

    출력:

    ```
    {
        "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI2MzdiM2Q4Yy1hMDg3LTVhZjktYTYzNi0xNmU3ZWZjNzA5NjciLCJpc3MiOiJyZWdpc3RyeS5jZnNkZWRpY2F0ZWQxLnVzLXNvdXRoLmJsdWVtaXgubmV0"
    }
    ```
    {: screen}

5.  Kubernetes 시크릿을 확인하십시오.

    ```
    kubectl describe secrets
    ```
    {: pre}

    이 시크릿을 사용하여 {{site.data.keyword.containerlong}}에 대해 작업할 수 있습니다.

6.  토큰 정보를 저장하기 위한 Kubernetes 시크릿을 작성하십시오.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_url> --docker-username=token --docker-password=<token_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>이 명령의 컴포넌트 이해</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 이 명령의 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace &lt;kubernetes_namespace&gt;</code></td>
    <td>필수. 시크릿을 사용하고 컨테이너를 배치하려는 클러스터의 Kubernetes 네임스페이스입니다. 클러스터의 모든 네임스페이스를 나열하려면 <code>kubectl get namespaces</code>를 실행하십시오.</td>
    </tr>
    <tr>
    <td><code>&lt;secret_name&gt;</code></td>
    <td>필수. imagePullSecret에 사용하려는 이름입니다.</td>
    </tr>
    <tr>
    <td><code>--docker-server &lt;registry_url&gt;</code></td>
    <td>필수. 네임스페이스가 설정된 이미지 레지스트리에 대한 URL: registry.&lt;dedicated_domain&gt;입니다.</li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username &lt;docker_username&gt;</code></td>
    <td>필수. 개인용 레지스트리에 로그인하기 위한 사용자 이름입니다.</td>
    </tr>
    <tr>
    <td><code>--docker-password &lt;token_value&gt;</code></td>
    <td>필수. 이전에 검색한 레지스트리 토큰의 값입니다.</td>
    </tr>
    <tr>
    <td><code>--docker-email &lt;docker-email&gt;</code></td>
    <td>필수. Docker 이메일 주소가 있는 경우 입력하십시오. 없는 경우에는 가상의 이메일 주소(예: example a@b.c)를 입력하십시오. 이 이메일은 Kubernetes 시크릿을 작성하기 위해서는 반드시 필요하지만 작성 후에는 사용되지 않습니다.</td>
    </tr>
    </tbody></table>

7.  imagePullSecret을 참조하는 팟(Pod)을 작성하십시오.

    1.  선호하는 텍스트 편집기를 열고 mypod.yaml이라는 이름의 팟(Pod) 구성 스크립트를 작성하십시오.
    2.  레지스트리에 액세스하기 위해 사용할 imagePullSecret 및 팟(Pod)을 정의하십시오. 네임스페이스에서 개인용 이미지를 사용하려면 다음을 작성하십시오.

        ```
        apiVersion: v1
        kind: Pod
        metadata:
          name: <pod_name>
        spec:
          containers:
            - name: <container_name>
              image: registry.<dedicated_domain>/<my_namespace>/<my_image>:<tag>  
          imagePullSecrets:
            - name: <secret_name>
        ```
        {: codeblock}

        <table>
        <caption>YAML 파일 컴포넌트 이해</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
        </thead>
        <tbody>
        <tr>
        <td><code>&lt;pod_name&gt;</code></td>
        <td>작성하려는 팟(Pod)의 이름입니다.</td>
        </tr>
        <tr>
        <td><code>&lt;container_name&gt;</code></td>
        <td>클러스터에 배치하려는 컨테이너의 이름입니다.</td>
        </tr>
        <tr>
        <td><code>&lt;my_namespace&gt;</code></td>
        <td>이미지가 저장된 네임스페이스입니다. 사용 가능한 네임스페이스를 나열하려면 `bx cr namespace-list`를 실행하십시오.</td>
        </tr>
        <td><code>&lt;my_image&gt;</code></td>
        <td>사용하려는 이미지의 이름입니다. {{site.data.keyword.Bluemix_notm}} 계정에서 사용 가능한 이미지를 나열하려면 <code>bx cr image-list</code>을 실행하십시오.</td>
        </tr>
        <tr>
        <td><code>&lt;tag&gt;</code></td>
        <td>사용하려는 이미지의 버전입니다. 태그가 지정되지 않은 경우, 기본적으로 <strong>latest</strong>로 태그가 지정된 이미지가 사용됩니다.</td>
        </tr>
        <tr>
        <td><code>&lt;secret_name&gt;</code></td>
        <td>이전에 작성한 imagePullSecret의 이름입니다.</td>
        </tr>
        </tbody></table>

    3.  변경사항을 저장하십시오.

    4.  클러스터에 배치를 작성하십시오.

          ```
          kubectl apply -f mypod.yaml
          ```
          {: pre}

