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



# 튜토리얼: {{site.data.keyword.containerlong_notm}}에 Istio 설치
{: #istio_tutorial}

[Istio](https://www.ibm.com/cloud/info/istio)는 {{site.data.keyword.containerlong}}에서 Kubernetes와 같은 클라우드 플랫폼에서 서비스 메시(service mesh)로도 알려진 마이크로서비스의 네트워크에 연결하고, 보안, 관리 및 모니터하기 위한 오픈 플랫폼입니다. Istio를 사용하여 네트워크 트래픽 관리하고, 마이크로서비스에서 로드 밸런싱하고, 액세스 정책을 적용하고, 서비스 ID를 확인할 수 있습니다.
{:shortdesc}

이 튜토리얼에서는 BookInfo라는 간단한 모의 서점 앱을 위해 네 개의 마이크로서비스에 나란히 Istio를 설치하는 방법을 살펴봅니다. 마이크로서비스는 제품 웹 페이지, 책 세부사항, 검토 및 평가를 포함합니다. Istio가 설치된 {{site.data.keyword.containershort}} 클러스터에 BookInfo의 마이크로서비스를 배치하는 경우, 각 마이크로서비스의 팟(Pod)에 Istio Envoy 사이드카 프록시를 삽입합니다.

**참고**: Istio 플랫폼의 일부 구성 및 기능은 여전히 개발 중이며 사용자 피드백을 기반으로 변경됩니다. 프로덕션에서 Istio를 사용하기 전에 안정화를 위해 몇 개월을 허용하십시오. 

## 목표

-   Istio를 클러스터에 다운로드하고 설치합니다.
-   BookInfo 샘플 앱을 배치합니다.
-   Envoy 사이드카 프록시를 앱의 네 개 마이크로서비스 팟(Pod)에 삽입하여 마이크로서비스를 서비스 메쉬에 연결합니다.
-   평가 서비스의 세 버전을 통해 BookInfo 앱 배치와 라운드 로빈을 확인하십시오.

## 소요 시간

30분

## 대상

이 튜토리얼은 처음으로 Istio를 사용하는 소프트웨어 개발자와 네트워크 관리자를 대상으로 합니다.

## 전제조건

-  [CLI를 설치](cs_cli_install.html#cs_cli_install_steps)하십시오. Istio에는 Kubernetes 버전 1.9 이상이 필요합니다. 클러스터의 Kubernetes 버전과 일치하는 `kubectl` CLI 버전을 설치해야 합니다.
-  Kubernetes 버전 1.9 이상을 사용하는 [클러스터를 작성](cs_clusters.html#clusters_cli)하십시오.
-  [CLI를 클러스터에 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오.

## 학습 1: Istio 다운로드 및 설치
{: #istio_tutorial1}

Istio를 클러스터에 다운로드하고 설치하십시오.
{:shortdesc}

1. [https://github.com/istio/istio/releases ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/istio/istio/releases)에서 Istio를 직접 다운로드할 수도 있고 curl을 사용하여 최신 버전을 가져올 수도 있습니다.

   ```
   curl -L https://git.io/getLatestIstio | sh -
   ```
   {: pre}

2. 설치 파일을 추출하십시오.

3. `istioctl` 클라이언트를 사용자의 PATH에 추가하십시오. 예를 들어, MacOS 또는 Linux 시스템에서 다음 명령을 실행하십시오.

   ```
   export PATH=$PWD/istio-0.4.0/bin:$PATH
   ```
   {: pre}

4. 디렉토리를 Istio 파일 위치로 변경하십시오.

   ```
   cd filepath/istio-0.4.0
   ```
   {: pre}

5. Istio를 Kubernetes 클러스터에 설치하십시오. Istio는 Kubernetes 네임스페이스 `istio-system`에 배치됩니다.

   ```
   kubectl apply -f install/kubernetes/istio.yaml
   ```
   {: pre}

   **참고**: 사이드카 간에 상호 TLS 인증을 사용해야 하는 경우, `kubectl apply -f install/kubernetes/istio-auth.yaml` 대신 `istio-auth` 파일을 설치할 수 있습니다.

6. 계속하기 전에 Kubernetes 서비스 `istio-pilot`, `istio-mixer` 및 `istio-ingress`가 완전히 배치되었는지 확인하십시오.

   ```
   kubectl get svc -n istio-system
   ```
   {: pre}

   ```
   NAME            TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)                                                            AGE
   istio-ingress   LoadBalancer   172.21.xxx.xxx   169.xx.xxx.xxx   80:31176/TCP,443:30288/TCP                                         2m
   istio-mixer     ClusterIP      172.21.xxx.xxx     <none>           9091/TCP,15004/TCP,9093/TCP,9094/TCP,9102/TCP,9125/UDP,42422/TCP   2m
   istio-pilot     ClusterIP      172.21.xxx.xxx    <none>           15003/TCP,443/TCP                                                  2m
   ```
   {: screen}

7. 계속하기 전에 해당 팟(Pod) `istio-pilot-*`, `istio-mixer-*`, `istio-ingress-*` 및 `istio-ca-*`도 완전히 배치되었는지 확인하십시오.

   ```
   kubectl get pods -n istio-system
   ```
   {: pre}

   ```
   istio-ca-3657790228-j21b9           1/1       Running   0          5m
   istio-ingress-1842462111-j3vcs      1/1       Running   0          5m
   istio-pilot-2275554717-93c43        1/1       Running   0          5m
   istio-mixer-2104784889-20rm8        2/2       Running   0          5m
   ```
   {: screen}


잘하셨습니다! Istio를 클러스터에 설치했습니다. 다음, BookInfo 샘플 앱을 클러스터에 배치하십시오.


## 학습 2: BookInfo 앱 배치
{: #istio_tutorial2}

BookInfo 샘플 앱의 마이크로서비스를 Kubernetes 클러스터에 배치하십시오.
{:shortdesc}

이러한 네 개의 마이크로서비스는 제품 웹 페이지, 책 세부사항, 검토(검토 마이크로서비스의 여러 버전으로) 및 평가를 포함합니다. 이 예제에서 사용된 모든 파일은 Istio 설치의 `samples/bookinfo` 디렉토리에서 찾을 수 있습니다.

BookInfo를 배치하는 경우 마이크로서비스 팟(Pod)이 배치되기 전에 Envoy 사이드카 프록시가 컨테이너로서 앱 마이크로서비스의 팟(Pod)에 삽입됩니다. Istio는 Envoy 프록시의 확장 버전을 사용하여 서비스 메쉬에서 모든 마이크로서비스에 대한 모든 인바운드 및 아웃바운드 트래픽을 중개합니다. Envoy에 대한 자세한 정보는 [Istio 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://istio.io/docs/concepts/what-is-istio/overview.html#envoy)를 참조하십시오.

1. BookInfo 앱을 배치하십시오. `kube-inject` 명령은 Envoy를 `bookinfo.yaml` 파일에 추가하고 이 업데이트된 파일을 사용하여 앱을 배치합니다. 앱 마이크로서비스가 배치될 때 Envoy 사이드카도 각 마이크로서비스 팟(Pod)에 배치됩니다.

   ```
   kubectl apply -f <(istioctl kube-inject -f samples/bookinfo/kube/bookinfo.yaml)
   ```
   {: pre}

2. 마이크로서비스와 해당 팟(Pod)이 배치되었는지 확인하십시오.

   ```
    kubectl get svc
   ```
   {: pre}

   ```
   NAME                       CLUSTER-IP   EXTERNAL-IP   PORT(S)              AGE
   details                    10.xxx.xx.xxx    <none>        9080/TCP             6m
   kubernetes                 10.xxx.xx.xxx     <none>        443/TCP              30m
   productpage                10.xxx.xx.xxx   <none>        9080/TCP             6m
   ratings                    10.xxx.xx.xxx    <none>        9080/TCP             6m
   reviews                    10.xxx.xx.xxx   <none>        9080/TCP             6m
   ```
   {: screen}

   ```
            kubectl get pods
   ```
   {: pre}

   ```
   NAME                                        READY     STATUS    RESTARTS   AGE
   details-v1-1520924117-48z17                 2/2       Running   0          6m
   productpage-v1-560495357-jk1lz              2/2       Running   0          6m
   ratings-v1-734492171-rnr5l                  2/2       Running   0          6m
   reviews-v1-874083890-f0qf0                  2/2       Running   0          6m
   reviews-v2-1343845940-b34q5                 2/2       Running   0          6m
   reviews-v3-1813607990-8ch52                 2/2       Running   0          6m
   ```
   {: screen}

3. 애플리케이션 배치를 확인하려면 클러스터의 공인 주소를 가져오십시오.

    * 표준 클러스터에 대해 작업 중인 경우, 다음 명령을 실행하여 클러스터의 Ingress IP와 포트를 가져오십시오.

       ```
       kubectl get ingress
       ```
       {: pre}

       출력 예:

       ```
       NAME      HOSTS     ADDRESS          PORTS     AGE
       gateway   *         169.xx.xxx.xxx   80        3m
       ```
       {: screen}

       이 예의 결과 Ingress 주소는 `169.48.221.218:80`입니다. 다음 명령을 사용하여 게이트웨이 URL로서 주소를 내보내십시오. 다음 단계에서는 게이트웨이 URL을 사용하여 BookInfo 제품 페이지에 액세스합니다.

       ```
       export GATEWAY_URL=169.xx.xxx.xxx:80
       ```
       {: pre}

    * 무료 클러스터에 대해 작업 중인 경우 작업자 노드와 NodePort의 공인 IP를 사용해야 합니다. 작업자 노드의 공인 IP를 얻으려면 다음 명령을 실행하십시오.

       ```
       bx cs workers <cluster_name_or_ID>
       ```
       {: pre}

       다음 명령을 사용하여 게이트웨이 URL로서 작업자 노드의 공인 IP를 내보내십시오. 다음 단계에서는 게이트웨이 URL을 사용하여 BookInfo 제품 페이지에 액세스합니다.

       ```
       export GATEWAY_URL=<worker_node_public_IP>:$(kubectl get svc istio-ingress -n istio-system -o jsonpath='{.spec.ports[0].nodePort}')
       ```
       {: pre}

4. `GATEWAY_URL` 변수를 curl하여 BookInfo가 실행 중인지 확인하십시오. `200` 응답은 BookInfo가 Istio와 함께 적절하게 실행 중임을 의미합니다.

   ```
   curl -I http://$GATEWAY_URL/productpage
   ```
   {: pre}

5. 브라우저에서 `http://$GATEWAY_URL/productpage`로 이동하여 BookInfo 웹 페이지를 보십시오.

6. 페이지를 여러 번 새로 고쳐보십시오. 검토 섹션의 다른 버전은 빨간색 별, 검은색 별, 별 없음을 통해 라운드 로빈됩니다.

잘하셨습니다! Istio Envoy 사이드카로 BookInfo 샘플 앱을 배치했습니다. 다음으로 리소스를 정리하거나 더 많은 튜토리얼을 학습하여 Istio를 추가로 탐색할 수도 있습니다.

## 정리
{: #istio_tutorial_cleanup}

Istio에 대한 작업을 완료했으며 [계속해서 탐색](#istio_tutorial_whatsnext)하지 않으려는 경우 클러스터에서 Istio 리소스를 정리할 수 있습니다.
{:shortdesc}

1. 클러스터에서 모든 BookInfo 서비스, 팟(Pod) 및 배치를 삭제하십시오.

   ```
   samples/bookinfo/kube/cleanup.sh
   ```
   {: pre}

2. Istio를 설치 제거하십시오.

   ```
   kubectl delete -f install/kubernetes/istio.yaml
   ```
   {: pre}

## 다음 단계
{: #istio_tutorial_whatsnext}

Istio를 추가로 탐색하려면 [Istio 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://istio.io/)에서 더 많은 안내서를 찾을 수 있습니다.

* [Intelligent Routing ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://istio.io/docs/guides/intelligent-routing.html): 이 예제는 Istio의 트래픽 관리 기능을 사용하여 특정 BookInfo 버전의 검토 및 등급 마이크로서비스로 트래픽을 라우팅하는 방법을 보여줍니다.

* [In-Depth Telemetry ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://istio.io/docs/guides/telemetry.html): 이 예에는 Istio Mixer와 Envoy 프록시를 사용하여 BookInfo의 전체 마이크로서비스에서 균일한 메트릭, 로그 및 추적을 얻는 방법이 포함되어 있습니다.
