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



# 튜토리얼: Cloud Foundry에서 클러스터로 앱 마이그레이션
{: #cf_tutorial}

Cloud Foundry를 사용하여 이전에 배치한 앱과 동일한 컨테이너 내 코드를 {{site.data.keyword.containershort_notm}}의 Kubernetes 클러스터에 배치할 수 있습니다.
{: shortdesc}


## 목표

- 컨테이너 내 앱을 Kubernetes 클러스터에 배치하는 일반 프로세스를 학습하십시오.
- 앱 코드로부터 컨테이너 이미지를 빌드하는 데 필요한 Dockerfile을 작성하십시오.
- 이 이미지로부터 컨테이너를 Kubernetes 클러스터에 배치하십시오.

## 소요 시간
30분

## 대상
이 튜토리얼은 Cloud Foundry 앱 개발자를 위한 것입니다.

## 전제조건

- [{{site.data.keyword.registrylong_notm}}에서 개인용 이미지 레지스트리를 작성하십시오](../services/Registry/index.html).
- [클러스터를 작성](cs_clusters.html#clusters_ui)하십시오.
- [CLI에서 클러스터를 대상으로 지정하십시오](cs_cli_install.html#cs_cli_configure).
- [Docker 및 Kubernetes 용어에 대해 학습하십시오](cs_tech.html).


<br />



## 학습 1: 앱 코드 다운로드

이동할 수 있도록 코드를 준비하십시오. 코드가 아직 없습니까? 이 경우에는 이 튜토리얼에서 사용할 수 있는 스타터 코드를 다운로드할 수 있습니다.
{: shortdesc}

1. `cf-py`라는 디렉토리를 작성하고 이 디렉토리로 이동하십시오. 이 디렉토리에는 Docker 이미지를 빌드하고 앱을 실행하는 데 필요한 모든 파일을 저장할 수 있습니다.

  ```
  mkdir cf-py && cd cf-py
  ```
  {: pre}

2. 앱 코드 및 모든 관련 파일을 이 디렉토리에 복사하십시오. 자신의 고유 앱 코드를 사용하거나 카탈로그에서 표준 유형을 다운로드하십시오. 이 튜토리얼에서는 Python Flask 표준 유형을 사용합니다. 그러나 Node.js, Java 또는 [Kitura](https://github.com/IBM-Cloud/Kitura-Starter) 앱에 대해서도 동일한 기본 단계를 사용할 수 있습니다.

    Python Flask 앱 코드를 다운로드하려면 다음 작업을 수행하십시오.

    a. 카탈로그의 **표준 유형**에서 **Python Flask**를 클릭하십시오. 이 표준 유형에는 Python 2 앱 및 Python 3 앱 모두를 위한 런타임 환경이 포함되어 있습니다.

    b. 앱 이름 `cf-py-<name>`을 입력하고 **작성**을 클릭하십시오. 표준 유형의 앱 코드에 액세스하려면 먼저 CF 앱을 클라우드에 배치해야 합니다. 앱에는 어떤 이름이든 사용할 수 있습니다. 이 예의 이름을 사용하거나, `<name>`을 `cf-py-msx`와 같이 고유 ID로 대체할 수도 있습니다.
    
    **주의**: 개인 정보를 앱, 컨테이너 이미지 또는 Kubernetes 리소스 이름으로 사용하지 마십시오.

    앱이 배치되면 "명령 인터페이스를 사용한 앱 다운로드, 수정 및 재배치"에 대한 지시사항이 표시됩니다.

    c. GUI 지시사항의 1단계에서 **스타터 코드 다운로드**를 클릭하십시오.

    d. .zip 파일의 압축을 풀어 컨텐츠를 `cf-py` 디렉토리에 저장하십시오.

앱 코드를 컨테이너화할 준비가 되었습니다.


<br />



## 학습 2: 앱 코드를 포함하는 Docker 이미지 작성

앱 코드 및 컨테이너에 필요한 구성을 포함하는 Dockerfile을 작성하십시오. 그 후 이 Dockerfile로부터 Docker 이미지를 빌드하고 이 이미지를 개인용 이미지 레지스트리에 푸시하십시오.
{: shortdesc}

1. 이전 학습에서 작성한 `cf-py` 디렉토리에서 컨테이너 이미지 작성의 기초인 `Dockerfile`을 작성하십시오. 컴퓨터의 선호하는 CLI 편집기 또는 텍스트 편집기를 사용하여 Dockerfile을 작성할 수 있습니다. 다음 예는 nano 편집기를 사용하여 Dockerfile을 작성하는 방법을 보여줍니다.

  ```
  nano Dockerfile
  ```
  {: pre}

2. 다음 스크립트를 Dockerfile에 복사하십시오. 이 Dockerfile은 Python 앱에 고유하게 적용됩니다. 다른 유형의 코드를 사용하고 있는 경우에는 Dockerfile이 다른 기초 이미지를 포함해야 하며 다른 필드를 정의해야 할 수 있습니다.

  ```
  #Use the Python image from DockerHub as a base image
  FROM python:3-slim

  #Expose the port for your python app
  EXPOSE 5000

  #Copy all app files from the current directory into the app
  #directory in your container. Set the app directory
  #as the working directory
  WORKDIR /cf-py/
  COPY .  .

  #Install any requirements that are defined
  RUN pip install --no-cache-dir -r requirements.txt

  #Update the openssl package
  RUN apt-get update && apt-get install -y \
  openssl

  #Start the app.
  CMD ["python", "welcome.py"]
  ```
  {: codeblock}

3. `ctrl + o`를 눌러 nano 편집기에서 변경사항을 저장하십시오. `enter`를 눌러 변경사항을 확인하십시오. `ctrl + x`를 눌러 nano 편집기를 종료하십시오.

4. 앱 코드를 포함하는 Docker 이미지를 빌드하고 이 이미지를 개인용 레지스트리에 푸시하십시오.

  ```
  bx cr build -t registry.<region>.bluemix.net/namespace/cf-py .
  ```
  {: pre}

  <table>
  <caption>이 명령의 컴포넌트 이해</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="이 아이콘은 이 명령의 컴포넌트에 대해 학습할 추가 정보가 있음을 나타냅니다. "/> 이 명령의 컴포넌트 이해</th>
  </thead>
  <tbody>
  <tr>
  <td>매개변수</td>
  <td>설명</td>
  </tr>
  <tr>
  <td><code>build</code></td>
  <td>빌드 명령입니다.</td>
  </tr>
  <tr>
  <td><code>-t registry.&lt;region&gt;.bluemix.net/namespace/cf-py</code></td>
  <td>고유 네임스페이스 및 이미지 이름을 포함하는 개인용 레지스트리 경로입니다. 이 예에서는 이미지에 대해 앱 디렉토리와 동일한 이름이 사용되었으나, 개인용 레지스트리의 이미지에 대해서는 어떤 이름이든 선택할 수 있습니다. 자신의 네임스페이스가 무엇인지 모르는 경우에는 `bx cr namespaces` 명령을 실행하여 찾아보십시오.</td>
  </tr>
  <tr>
  <td><code>.</code></td>
  <td>Dockerfile의 위치입니다. Dockerfile을 포함하는 디렉토리로부터 build 명령을 실행하는 경우에는 마침표(.)를 입력하십시오. 그렇지 않은 경우에는 Dockerfile에 대한 상대 경로를 사용하십시오.</td>
  </tr>
  </tbody>
  </table>

  이미지가 개인용 레지스트리에 작성됩니다. `bx cr images` 명령을 실행하여 이미지가 작성되었는지 확인할 수 있습니다.

  ```
  REPOSITORY                                     NAMESPACE   TAG      DIGEST         CREATED         SIZE     VULNERABILITY STATUS   
  registry.ng.bluemix.net/namespace/cf-py        namespace   latest   cb03170b2cb2   3 minutes ago   271 MB   OK
  ```
  {: screen}


<br />



## 학습 3: 이미지로부터 컨테이너 배치

앱을 컨테이너로서 Kubernetes 클러스터에 배치하십시오.
{: shortdesc}

1. `cf-py.yaml`이라는 구성 YAML 파일을 작성하고 `<registry_namespace>`를 개인용 이미지 레지스트리의 이름으로 업데이트하십시오. 이 구성 파일은 이전 학습에서 작성한 이미지로부터의 컨테이너 배치와 앱을 공용으로 노출시키는 서비스를 정의합니다.

  ```
  apiVersion: extensions/v1beta1
  kind: Deployment
  metadata:
    labels:
      app: cf-py
    name: cf-py
    namespace: default
  spec:
    selector:
      matchLabels:
        app: cf-py
    replicas: 1
    template:
      metadata:
        labels:
          app: cf-py
      spec:
        containers:
        - image: registry.ng.bluemix.net/<registry_namespace>/cf-py:latest
          name: cf-py
  ---
  apiVersion: v1
  kind: Service
  metadata:
    name: cf-py-nodeport
    labels:
      app: cf-py
  spec:
    selector:
      app: cf-py
    type: NodePort
    ports:
     - port: 5000
       nodePort: 30872
  ```
  {: codeblock}

  <table>
  <caption>YAML 파일 컴포넌트 이해</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
  </thead>
  <tbody>
  <tr>
  <td><code>image</code></td>
  <td>`registry.ng.bluemix.net/<registry_namespace>/cf-py:latest`에서 &lt;registry_namespace&gt;를 개인용 이미지 레지스트리의 네임스페이스로 대체하십시오. 자신의 네임스페이스가 무엇인지 모르는 경우에는 `bx cr namespaces` 명령을 실행하여 찾아보십시오.</td>
  </tr>
  <tr>
  <td><code>nodePort</code></td>
  <td>NodePort 유형의 Kubernetes 서비스를 작성하여 앱을 노출시킵니다. NodePort의 범위는 30000 - 32767입니다. 이 포트는 나중에 브라우저에서 앱을 테스트하는 데 사용됩니다.</td>
  </tr>
  </tbody></table>

2. 구성 파일을 적용하여 클러스터에서 배치 및 서비스를 작성하십시오.

  ```
  kubectl apply -f <filepath>/cf-py.yaml
  ```
  {: pre}

  출력:

  ```
  deployment "cf-py" configured
  service "cf-py-nodeport" configured
  ```
  {: screen}

3. 이제 모든 배치 작업이 완료되었으므로 브라우저에서 앱을 테스트할 수 있습니다. URL을 형성하기 위한 세부사항을 가져오십시오.

    a.  클러스터의 작업자 노드에 대한 공인 IP 주소를 가져오십시오.

    ```
    bx cs workers <cluster_name>
    ```
    {: pre}

    출력:

    ```
    ID                                                 Public IP        Private IP     Machine Type        State    Status   Zone    Version   
    kube-dal10-cr18e61e63c6e94b658596ca93d087eed9-w1   169.xx.xxx.xxx   10.xxx.xx.xxx   u2c.2x4.encrypted   normal   Ready    dal10   1.9.7
    ```
    {: screen}

    b. 브라우저를 열고 `http://<public_IP_address>:<NodePort>` URL로 앱을 확인하십시오. 예의 값을 사용하면 이 URL은 `http://169.xx.xxx.xxx:30872`입니다. 앱이 실제로 공용으로 사용 가능한지 확인하기 위해 동료에게 이 URL에 접속해 보도록 요청하거나 자신의 휴대전화 브라우저에 이 URL을 입력해 볼 수 있습니다.

    <img src="images/python_flask.png" alt="배치된 표준 유형 Python Flask 앱의 화면 캡처입니다. " />

5.  [Kubernetes 대시보드를 실행](cs_app.html#cli_dashboard)하십시오.

    [{{site.data.keyword.Bluemix_notm}} GUI](https://console.bluemix.net/)에서 클러스터를 선택하는 경우 **Kubernetes 대시보드** 단추를 사용하여 한 번의 클릭으로 대시보드를 실행할 수 있습니다.
    {: tip}

6. **워크로드** 탭에서, 작성된 리소스를 볼 수 있습니다.

축하합니다! 앱이 컨테이너에 배치되었습니다.
