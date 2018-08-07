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



# LoadBalancer를 사용한 앱 노출
{: #loadbalancer}

포트를 노출하고 로드 밸런서의 포터블 IP 주소를 사용하여 컨테이너화된 앱에 액세스하십시오.
{:shortdesc}

## LoadBalancer를 사용한 네트워크 트래픽 관리
{: #planning}

표준 클러스터를 작성할 때 {{site.data.keyword.containershort_notm}}가 자동으로 다음 서브넷을 프로비저닝합니다.
* 클러스터 작성 중에 작업자 노드에 대한 공인 IP 주소를 판별하는 기본 공인 서브넷
* 클러스터 작성 중에 작업자 노드에 대한 사설 IP 주소를 판별하는 기본 사설 서브넷
* Ingress 및 로드 밸런서 네트워킹 서비스에 대한 5개의 공인 IP 주소를 제공하는 포터블 공인 서브넷
* Ingress 및 로드 밸런서 네트워킹 서비스에 대한 5개의 사설 IP 주소를 제공하는 포터블 사설 서브넷

      포터블 공인 및 사설 IP 주소는 정적이며 작업자 노드가 제거될 때 변경되지 않습니다. 각 서브넷마다 하나의 포터블 공인 IP 주소와 하나의 포터블 사설 IP 주소가 기본 [Ingress 애플리케이션 로드 밸런서](cs_ingress.html)에 사용됩니다. 나머지 네 개의 포터블 공인 IP 주소 및 네 개의 포터블 사설 IP 주소는 로드 밸런서 서비스를 작성하여 단일 앱을 공용 또는 사설 네트워크에 노출시키는 데 사용할 수 있습니다. 

공용 VLAN의 클러스터에 Kubernetes LoadBalancer 서비스를 작성하는 경우 외부 로드 밸런서가 작성됩니다. LoadBalancer 서비스를 작성할 때 IP 주소의 옵션은 다음과 같습니다.

- 클러스터가 공용 VLAN에 있는 경우 네 개의 사용 가능한 포터블 공인 IP 주소 중 하나가 사용됩니다.
- 클러스터가 사설 VLAN에만 있는 경우 네 개의 사용 가능한 포터블 사설 IP 주소 중 하나가 사용됩니다.
- 구성 파일에 다음과 같은 어노테이션을 추가하여 LoadBalancer 서비스의 포터블 공인 또는 사설 IP 주소를 요청할 수 있습니다. `service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: <public_or_private>`.

LoadBalancer 서비스에 지정된 포터블 공인 IP 주소는 영구적이며 작업자 노드가 제거되거나 다시 작성될 때 변경되지 않습니다. 따라서 LoadBalancer 서비스가 NodePort 서비스보다 고가용성입니다. NodePort 서비스와 다르게 로드 밸런서에 포트를 지정할 수 있으며 특정 포트 범위에 바인딩되지 않습니다. LoadBalancer 서비스를 사용하는 경우 작업자 노드의 각 IP 주소에서 NodePort를 사용할 수도 있습니다. LoadBalancer 서비스를 사용하는 동안 NodePort에 대한 액세스를 차단하려면 [수신 트래픽 차단](cs_network_policy.html#block_ingress)을 참조하십시오.

LoadBalancer 서비스는 앱의 수신 요청에 대한 외부 시작점 역할을 합니다. 인터넷에서 LoadBalancer 서비스에 액세스하려면, `<IP_address>:<port>` 형식의 지정된 포트와 로드 밸런서의 공인 IP 주소를 사용하십시오. 다음 다이어그램은 로드 밸런서가 인터넷에서 앱으로 통신하는 방식을 표시합니다.

<img src="images/cs_loadbalancer_planning.png" width="550" alt="로드 밸런서를 사용하여 {{site.data.keyword.containershort_notm}}에 앱 노출" style="width:550px; border-style: none"/>

1. 작업자 노드에서 지정된 포트와 로드 밸런서의 공인 IP 주소를 사용하여 앱에 요청을 전송합니다.

2. 요청은 자동으로 로드 밸런서 서비스의 내부 클러스터 IP 주소 및 포트로 전달됩니다. 내부 클러스터 IP 주소는 클러스터 내에서만 액세스가 가능합니다.

3. `kube-proxy`는 앱의 Kubernetes 로드 밸런서 서비스에 대한 요청을 라우팅합니다.

4. 앱이 배치된 팟(Pod)의 사설 IP 주소로 요청이 전달됩니다. 다중 앱 인스턴스가 클러스터에 배치되는 경우 로드 밸런서는 앱 팟(Pod) 간의 요청을 라우팅합니다.




<br />


 </td>
        </tr>
        <tr>
          <td><code>selector</code></td>
          <td>앱이 실행되는 팟(Pod)을 대상으로 지정하는 데 사용할 레이블 키(<em>&lt;selector_key&gt;</em>) 및 값(<em>&lt;selector_value&gt;</em>) 쌍을 입력하십시오. 팟(Pod)을 대상으로 지정하여 서비스 로드 밸런싱에 포함시키려면 <em>&lt;selectorkey&gt;</em> 및 <em>&lt;selectorvalue&gt;</em> 값을 확인하십시오. 배치 yaml의 <code>spec.template.metadata.labels</code> 섹션에서 사용한 <em>키/값</em> 쌍과 동일한지 확인하십시오.</td>
        </tr>
        <tr>
          <td><code>port</code></td>
          <td>서비스가 청취하는 포트입니다.</td>
        </tr>
        <tr>
          <td><code>loadBalancerIP</code></td>
          <td>개인용 LoadBalancer를 작성하거나 공용 LoadBalancer의 특정 포터블 IP 주소를 사용하려면 <em>&lt;IP_address&gt;</em>를 사용할 IP 주소로 대체하십시오. 자세한 정보는 [Kubernetes 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer)를 참조하십시오.</td>
        </tr>
        </tbody></table>

      3. 선택사항: **spec** 섹션에 `loadBalancerSourceRanges`를 지정하여 방화벽을 구성하십시오. 자세한 정보는 [Kubernetes 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/access-application-cluster/configure-cloud-provider-firewall/)를 참조하십시오.

      4. 클러스터에 서비스를 작성하십시오.

          ```
        kubectl apply -f myloadbalancer.yaml
          ```
          {: pre}

                  로드 밸런서 서비스가 작성되면 포터블 IP 주소가 자동으로 로드 밸런서에 지정됩니다. 사용 가능한 포터블 IP 주소가 없는 경우에는 로드 밸런서 서비스를 작성할 수 없습니다.

3.  로드 밸런서 서비스가 정상적으로 작성되었는지 확인하십시오. _&lt;myservice&gt;_를 이전 단계에서 작성한 로드 밸런서 서비스의 이름으로 대체하십시오.

    ```
    kubectl describe service myloadbalancer
    ```
    {: pre}

    **참고:** 로드 밸런서 서비스가 올바르게 작성되고 앱을 사용할 수 있으려면 몇 분이 걸릴 수 있습니다.

    CLI 출력 예:

    ```
    Name:                   myloadbalancer
    Namespace:              default
    Labels:                 <none>
    Selector:               app=liberty
    Type:                   LoadBalancer
    Location:               dal10
    IP:                     172.21.xxx.xxx
    LoadBalancer Ingress:   169.xx.xxx.xxx
    Port:                   <unset> 8080/TCP
    NodePort:               <unset> 32040/TCP
    Endpoints:              172.30.xxx.xxx:8080
    Session Affinity:       None
    Events:
      FirstSeen	LastSeen	Count	From			SubObjectPath	Type	 Reason			          Message
      ---------	--------	-----	----			-------------	----	 ------			          -------
      10s		    10s		    1	    {service-controller }	  Normal CreatingLoadBalancer	Creating load balancer
      10s		    10s		    1	    {service-controller }		Normal CreatedLoadBalancer	Created load balancer
    ```
    {: screen}

    **LoadBalancer Ingress** IP 주소는 로드 밸런서 서비스에 지정된 포터블 IP 주소입니다.

4.  공용 로드 밸런서를 작성한 경우 인터넷에서 앱에 액세스하십시오.
    1.  선호하는 웹 브라우저를 여십시오.
    2.  로드 밸런서 및 포트의 포터블 공인 IP 주소를 입력하십시오.

        ```
        http://169.xx.xxx.xxx:8080
        ```
        {: codeblock}        

5. [로드 밸런서 서비스에 대해 소스 IP 유지를 사용 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-typeloadbalancer)하도록 선택하는 경우에는 [에지 노드 친화성을 앱 팟(Pod)에 추가](cs_loadbalancer.html#edge_nodes)하여 앱 팟(Pod)이 에지 작업자 노드에 스케줄되도록 하십시오. 수신 요청을 받도록 앱 팟(Pod)이 에지 노드에 스케줄되어야 합니다.

6. 선택사항: 다른 구역에서 앱에 대한 수신 요청을 처리하려면 이러한 단계를 반복하여 각 구역에 로드 밸런서를 추가하십시오. 

</staging>

## LoadBalancer 서비스를 사용하여 앱에 대한 공용 또는 개인용 액세스 사용 설정
{: #config}

시작하기 전에:

-   이 기능은 표준 클러스터에 대해서만 사용 가능합니다.
-   로드 밸런서 서비스에 지정하는 데 사용할 수 있는 포터블 공인 또는 사설 IP 주소를 보유해야 합니다.
-   포터블 사설 IP 주소를 사용하는 로드 밸런서 서비스에는 여전히 모든 작업 노드에서 열려 있는 공용 NodePort가 있습니다. 공용 트래픽을 방지하기 위한 네트워크 정책을 추가하려면 [수신 트래픽 차단](cs_network_policy.html#block_ingress)을 참조하십시오.

로드 밸런서 서비스를 작성하려면 다음을 수행하십시오.

1.  [클러스터에 앱을 배치](cs_app.html#app_cli)하십시오. 클러스터에 앱을 배치하면 컨테이너에서 앱을 실행하는 사용자에 대한 하나 이상의 팟(Pod)이 작성됩니다. 구성 파일의 메타데이터 섹션에서 배치에 레이블을 추가했는지 확인하십시오. 이 레이블은 로드 밸런싱에 포함될 수 있도록 앱이 실행 중인 모든 팟(Pod)을 식별하는 데 필요합니다.
2.  노출시키려는 앱에 대해 로드 밸런서 서비스를 정의하십시오. 공용 인터넷 또는 사설 네트워크에서 앱을 사용할 수 있으려면 앱에 대한 Kubernetes 서비스를 작성하십시오. 앱을 구성하는 모든 팟(Pod)을 로드 밸런싱에 포함하도록 서비스를 구성하십시오.
    1.  예를 들어, `myloadbalancer.yaml`이라는 이름의 서비스 구성 파일을 작성하십시오.

    2.  노출하려는 앱에 대한 로드 밸런서 서비스를 정의하십시오.
        - 클러스터가 공용 VLAN에 있는 경우 포터블 공인 IP 주소가 사용됩니다. 대부분의 클러스터는 공용 VLAN에 있습니다.
        - 클러스터가 사설 VLAN에서만 사용 가능한 경우 포터블 사설 IP 주소가 사용됩니다.
        - 구성 파일에 어노테이션을 추가하여 LoadBalancer 서비스에 대한 포터블 공인 또는 사설 IP 주소를 요청할 수 있습니다.

        기본 IP 주소를 사용하는 LoadBalancer 서비스:

        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: myloadbalancer
        spec:
          type: LoadBalancer
          selector:
            <selector_key>:<selector_value>
          ports:
           - protocol: TCP
             port: 8080
        ```
        {: codeblock}

        어노테이션을 사용하여 사설 또는 공인 IP 주소를 지정하는 LoadBalancer 서비스:

        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: myloadbalancer
          annotations:
            service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: <public_or_private>
        spec:
          type: LoadBalancer
          selector:
            <selector_key>: <selector_value>
          ports:
           - protocol: TCP
             port: 8080
          loadBalancerIP: <IP_address>
        ```
        {: codeblock}

        <table>
        <caption>YAML 파일 컴포넌트 이해</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
        </thead>
        <tbody>
        <tr>
          <td><code>selector</code></td>
          <td>앱이 실행되는 팟(Pod)을 대상으로 지정하는 데 사용할 레이블 키(<em>&lt;selector_key&gt;</em>) 및 값(<em>&lt;selector_value&gt;</em>) 쌍을 입력하십시오. 팟(Pod)을 대상으로 지정하여 서비스 로드 밸런싱에 포함시키려면 <em>&lt;selector_key&gt;</em> 및 <em>&lt;selector_value&gt;</em> 값을 확인하십시오. 배치 yaml의 <code>spec.template.metadata.labels</code> 섹션에서 사용한 <em>키/값</em> 쌍과 동일한지 확인하십시오.</td>
        </tr>
        <tr>
          <td><code>port</code></td>
          <td>서비스가 청취하는 포트입니다.</td>
        </tr>
        <tr>
          <td>`service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type:`
          <td>LoadBalancer의 유형을 지정하기 위한 어노테이션입니다. 허용되는 값은 `private` 및 `public`입니다. 공용 VLAN의 클러스터에서 공용 LoadBalancer를 작성하는 경우 이 어노테이션은 필요하지 않습니다.</td>
        </tr>
        <tr>
          <td><code>loadBalancerIP</code></td>
          <td>개인용 LoadBalancer를 작성하거나 공용 LoadBalancer의 특정 포터블 IP 주소를 사용하려면 <em>&lt;IP_address&gt;</em>를 사용할 IP 주소로 대체하십시오. 자세한 정보는 [Kubernetes 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer)를 참조하십시오.</td>
        </tr>
        </tbody></table>

    3.  선택사항: **spec** 섹션에 `loadBalancerSourceRanges`를 지정하여 방화벽을 구성하십시오. 자세한 정보는 [Kubernetes 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tasks/access-application-cluster/configure-cloud-provider-firewall/)를 참조하십시오.

    4.  클러스터에 서비스를 작성하십시오.

        ```
        kubectl apply -f myloadbalancer.yaml
        ```
        {: pre}

                로드 밸런서 서비스가 작성되면 포터블 IP 주소가 자동으로 로드 밸런서에 지정됩니다. 사용 가능한 포터블 IP 주소가 없는 경우에는 로드 밸런서 서비스를 작성할 수 없습니다.

3.  로드 밸런서 서비스가 정상적으로 작성되었는지 확인하십시오.

    ```
    kubectl describe service myloadbalancer
    ```
    {: pre}

    **참고:** 로드 밸런서 서비스가 올바르게 작성되고 앱을 사용할 수 있으려면 몇 분이 걸릴 수 있습니다.

    CLI 출력 예:

    ```
    Name:                   myloadbalancer
    Namespace:              default
    Labels:                 <none>
    Selector:               app=liberty
    Type:                   LoadBalancer
    Location:               dal10
    IP:                     172.21.xxx.xxx
    LoadBalancer Ingress:   169.xx.xxx.xxx
    Port:                   <unset> 8080/TCP
    NodePort:               <unset> 32040/TCP
    Endpoints:              172.30.xxx.xxx:8080
    Session Affinity:       None
    Events:
      FirstSeen	LastSeen	Count	From			SubObjectPath	Type	 Reason			          Message
      ---------	--------	-----	----			-------------	----	 ------			          -------
      10s		    10s		    1	    {service-controller }	  Normal CreatingLoadBalancer	Creating load balancer
      10s		    10s		    1	    {service-controller }		Normal CreatedLoadBalancer	Created load balancer
    ```
    {: screen}

    **LoadBalancer Ingress** IP 주소는 로드 밸런서 서비스에 지정된 포터블 IP 주소입니다.

4.  공용 로드 밸런서를 작성한 경우 인터넷에서 앱에 액세스하십시오.
    1.  선호하는 웹 브라우저를 여십시오.
    2.  로드 밸런서 및 포트의 포터블 공인 IP 주소를 입력하십시오.

        ```
        http://169.xx.xxx.xxx:8080
        ```
        {: codeblock}

5. [로드 밸런서 서비스에 대해 소스 IP 유지를 사용 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-typeloadbalancer)하도록 선택하는 경우에는 [에지 노드 친화성을 앱 팟(Pod)에 추가](cs_loadbalancer.html#edge_nodes)하여 앱 팟(Pod)이 에지 작업자 노드에 스케줄되도록 하십시오. 수신 요청을 받도록 앱 팟(Pod)이 에지 노드에 스케줄되어야 합니다.

<br />


## 앱 팟(Pod)에 소스 IP에 대한 노드 친화성 및 결함 허용 추가
{: #node_affinity_tolerations}

앱 팟(Pod)을 배치할 때는 로드 밸런서 서비스 팟(Pod) 또한 앱 팟(Pod)이 배치되는 작업자 노드에 배치됩니다. 그러나 다음과 같은 일부 상황에서는 로드 밸런서 팟(Pod)과 앱 팟(Pod)이 동일한 작업자 노드에 스케줄되지 않습니다.
{: shortdesc}

* 로드 밸런서 서비스 팟(Pod)만 배치할 수 있도록 오염된 에지 노드가 있습니다. 이러한 노드에는 앱 팟(Pod)을 배치할 수 없습니다.
* 클러스터가 여러 공용 또는 개인용 VLAN에 연결되어 있으며 앱 팟(Pod)이 하나의 VLAN에만 연결된 작업자 노드에 배치될 가능성이 있습니다. 로드 밸런서 서비스 팟(Pod)이 작업자 노드와 다른 VLAN에 연결되어 있어 이러한 작업자 노드에 로드 밸런서 서비스 팟(Pod)을 배치하지 못할 수 있습니다.

앱에 대한 클라이언트 요청이 클러스터에 전송되면, 이 요청은 앱을 노출시키는 Kubernetes 로드 밸런서 서비스의 팟(Pod)으로 라우팅됩니다. 앱 팟(Pod)이 로드 밸런서 서비스 팟(Pod)과 동일한 작업자 노드에 없는 경우 로드 밸런서는 다른 작업자 노드의 앱 팟(Pod)으로 요청을 전달합니다. 패키지의 소스 IP 주소는 앱 팟(Pod)이 실행 중인 작업자 노드의 공인 IP 주소로 변경됩니다.

클라이언트 요청의 원본 소스 IP 주소를 유지하려는 경우 로드 밸런서 서비스에 대해 [소스 IP를 사용하도록 설정 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-typeloadbalancer)할 수 있습니다. 앱 서버가 보안 및 액세스 제어 정책을 적용해야 하는 경우 등에는 클라이언트의 IP를 유지하는 것이 유용합니다. 소스 IP를 사용할 수 있도록 설정하면 로드 밸런서 서비스 팟(Pod)은 동일한 작업자 노드에 배치된 앱 팟(Pod)에만 요청을 전달해야 합니다. 앱이 로드 밸런서 서비스 팟(Pod)을 함께 배치할 수 있는 특정 작업자 노드에 배치되도록 강제하려면 앱 배치에 친화성 규칙 및 결함 허용을 추가해야 합니다.

### 에지 노드 친화성 규칙 및 결함 허용 추가
{: #edge_nodes}

[작업자 노드의 레이블을 에지 노드로 지정](cs_edge.html#edge_nodes)하고 [에지 노드를 오염](cs_edge.html#edge_workloads)시키면 로드 밸런서 서비스 팟(Pod)이 해당 에지 노드에만 배치되며 앱 팟(Pod)을 에지 노드에 배치할 수 없습니다. 소스 IP가 로드 밸런서 서비스에 대해 사용으로 설정되면 에지 노드의 로드 밸런서 팟(Pod)이 다른 작업자 노드의 앱 팟(Pod)에 수신 요청을 전달할 수 없습니다.
{:shortdesc}

앱 팟(Pod)이 에지 노드에 배치되도록 강제하려면 앱 배치에 에지 노드 [친화성 규칙 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#node-affinity-beta-feature) 및 [결함 허용 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/#concepts)을 추가하십시오.

에지 노드 친화성 및 에지 노드 결함 허용을 포함하는 배치 yaml 예는 다음과 같습니다.

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: with-node-affinity
spec:
  template:
    spec:
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: dedicated
                operator: In
                values:
                - edge
      tolerations:
        - key: dedicated
          value: edge
...
```
{: codeblock}

**affinity** 및 **tolerations** 섹션 모두에서 `dedicated`가 `key`이고 `edge`가 `value`입니다. 

### 여러 공용 또는 개인용 VLAN에 대한 친화성 규칙 추가
{: #edge_nodes_multiple_vlans}

클러스터가 여러 공용 또는 개인용 VLAN에 연결되어 있으면 앱 팟(Pod)이 하나의 VLAN에만 연결된 작업자 노드에 배치될 가능성이 있습니다. 로드 밸런서 IP 주소가 작업자 노드와 다른 VLAN에 연결되어 있는 경우에는 로드 밸런서 서비스 팟(Pod)이 이러한 작업자 노드에 배치되지 않습니다.
{:shortdesc}

소스 IP를 사용할 수 있도록 설정된 경우에는 앱 배치에 친화성 규칙을 추가하여 로드 밸런서의 IP 주소와 동일한 VLAN인 작업자 노드에 앱 팟(Pod)을 스케줄하십시오.

시작하기 전에 클러스터를 [CLI의 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오.

1. 로드 밸런서 서비스의 IP 주소를 가져오십시오. **LoadBalancer Ingress** 필드에서 IP 주소를 확인하십시오.
    ```
    kubectl describe service <loadbalancer_service_name>
    ```
    {: pre}

2. 로드 밸런서 서비스가 연결된 VLAN ID를 검색하십시오.

    1. 클러스터의 포터블 공용 VLAN을 나열하십시오.
        ```
        bx cs cluster-get <cluster_name_or_ID> --showResources
        ```
        {: pre}

        출력 예:
        ```
        ...

        Subnet VLANs
        VLAN ID   Subnet CIDR       Public   User-managed
        2234947   10.xxx.xx.xxx/29  false    false
        2234945   169.36.5.xxx/29   true     false
        ```
        {: screen}

    2. **Subnet VLANs** 아래의 출력에서 이전에 검색한 로드 밸런서 IP 주소와 일치하는 서브넷 CIDR을 찾고 VLAN ID를 기록해 두십시오.

        예를 들어, 로드 밸런서 서비스 IP 주소가 `169.36.5.xxx`인 경우 이전 단계의 출력 예에서 일치하는 서브넷은 `169.36.5.xxx/29`입니다. 해당 서브넷이 연결된 VLAN ID는 `2234945`입니다.

3. 이전 단계에서 기록한 VLAN ID에 대한 앱 배치에 [친화성 규칙을 추가하십시오 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#node-affinity-beta-feature).

    예를 들어, 여러 VLAN이 있으나 앱 팟(Pod)이 `2234945` 공용 VLAN의 작업자 노드에만 배치되도록 하려면 다음과 같이 입력하십시오.

    ```
    apiVersion: extensions/v1beta1
    kind: Deployment
    metadata:
      name: with-node-affinity
    spec:
      template:
        spec:
          affinity:
            nodeAffinity:
              requiredDuringSchedulingIgnoredDuringExecution:
                nodeSelectorTerms:
                - matchExpressions:
                  - key: publicVLAN
                    operator: In
                    values:
                    - "2234945"
    ...
    ```
    {: codeblock}

    YAML 예에서는 **affinity** 섹션에서 `publicVLAN`이 `key`이며 `"2234945"`가 `value`입니다. 

4. 업데이트된 배치 구성 파일을 적용하십시오.
    ```
    kubectl apply -f with-node-affinity.yaml
    ```
    {: pre}

5. 지정된 VLAN에 연결된 작업자 노드에 앱 팟(Pod)이 배치되었는지 확인하십시오.

    1. 클러스터 내의 팟(Pod)을 나열하십시오. `<selector>`를 앱에 대해 사용한 레이블로 대체하십시오.
        ```
        kubectl get pods -o wide app=<selector>
        ```
        {: pre}

        출력 예:
        ```
        NAME                   READY     STATUS              RESTARTS   AGE       IP               NODE
        cf-py-d7b7d94db-vp8pq  1/1       Running             0          15d       172.30.xxx.xxx   10.176.48.78
        ```
        {: screen}

    2. 출력에서 앱의 팟(Pod)을 식별하십시오. 해당 팟(Pod)이 있는 작업자 노드의 **NODE** ID를 기록해 두십시오.

        이전 단계의 출력 예에서는 앱 팟(Pod) `cf-py-d7b7d94db-vp8pq`가 작업자 노드 `10.176.48.78`에 있습니다. 

    3. 작업자 노드의 세부사항을 나열하십시오.

        ```
        kubectl describe node <worker_node_ID>
        ```
        {: pre}

        출력 예:

        ```
        Name:                   10.xxx.xx.xxx
        Role:
        Labels:                 arch=amd64
                                beta.kubernetes.io/arch=amd64
                                beta.kubernetes.io/os=linux
                                failure-domain.beta.kubernetes.io/region=us-south
                                failure-domain.beta.kubernetes.io/zone=dal10
                                ibm-cloud.kubernetes.io/encrypted-docker-data=true
                                kubernetes.io/hostname=10.xxx.xx.xxx
                                privateVLAN=2234945
                                publicVLAN=2234967
        ...
        ```
        {: screen}

    4. 출력의 **Labels** 섹션에서 공용 또는 개인용 VLAN이 이전 단계에서 지정한 VLAN인지 확인하십시오.
