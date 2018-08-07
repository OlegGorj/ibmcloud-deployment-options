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



# Ingress 어노테이션
{: #ingress_annotation}

Ingress 애플리케이션 로드 밸런서(ALB)에 기능을 추가하려면 Ingress 리소스에서 어노테이션을 메타데이터로 지정할 수 있습니다.
{: shortdesc}

Ingress 서비스 및 이 서비스의 사용을 시작하는 방법에 대한 일반 정보는 [Ingress를 사용한 네트워크 트래픽 관리](cs_ingress.html#planning)를 참조하십시오.

<table>
<caption>일반 어노테이션</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>일반 어노테이션</th>
 <th>이름</th>
 <th>설명</th>
 </thead>
 <tbody>
 <tr>
 <td><a href="#proxy-external-service">외부 서비스</a></td>
 <td><code>proxy-external-service</code></td>
 <td>외부 서비스(예: {{site.data.keyword.Bluemix_notm}}에서 호스팅되는 서비스)에 경로 정의를 추가합니다.</td>
 </tr>
 <tr>
 <td><a href="#location-modifier">위치 수정자</a></td>
 <td><code>location-modifier</code></td>
 <td>ALB가 앱 경로에 대해 요청 URI를 일치시키는 방법을 수정합니다.</td>
 </tr>
 <tr>
 <td><a href="#alb-id">사설 ALB 라우팅</a></td>
 <td><code>ALB-ID</code></td>
 <td>사설 ALB를 사용하여 수신 요청을 앱으로 라우팅합니다.</td>
 </tr>
 <tr>
 <td><a href="#rewrite-path">경로 재작성</a></td>
 <td><code>rewrite-path</code></td>
 <td>백엔드 앱이 청취하는 다른 경로로 수신 네트워크 트래픽을 라우팅합니다.</td>
 </tr>
 <tr>
 <td><a href="#tcp-ports">TCP 포트</a></td>
 <td><code>tcp-ports</code></td>
 <td>비표준 TCP 포트를 통해 앱에 액세스합니다.</td>
 </tr>
 </tbody></table>

<br>

<table>
<caption>연결 어노테이션</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>연결 어노테이션</th>
 <th>이름</th>
 <th>설명</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#proxy-connect-timeout">사용자 정의 연결 제한시간 및 읽기 제한시간</a></td>
  <td><code>proxy-connect-timeout, proxy-read-timeout</code></td>
  <td>백엔드 앱이 사용할 수 없는 것으로 간주되기 전에 백엔드 앱에 연결하고 백엔드 앱에서 읽기 위해 ALB가 대기하는 시간을 설정합니다.</td>
  </tr>
  <tr>
  <td><a href="#keepalive-requests">Keepalive 요청</a></td>
  <td><code>keepalive-requests</code></td>
  <td>하나의 Keepalive 연결을 통해 서비스할 수 있는 최대 요청 수를 설정합니다.</td>
  </tr>
  <tr>
  <td><a href="#keepalive-timeout">Keepalive 제한시간</a></td>
  <td><code>keepalive-timeout</code></td>
  <td>서버에서 Keepalive 연결이 열려 있는 최대 시간을 설정합니다.</td>
  </tr>
  <tr>
  <td><a href="#proxy-next-upstream-config">다음 업스트림 프록시</a></td>
  <td><code>proxy-next-upstream-config</code></td>
  <td>ALB가 다음 업스트림 서버로 요청을 전달할 수 있는 시점을 설정합니다.</td>
  </tr>
  <tr>
  <td><a href="#sticky-cookie-services">쿠키를 사용하는 세션 선호도</a></td>
  <td><code>sticky-cookie-services</code></td>
  <td>스티키 쿠키(sticky cookie)를 사용하여 동일한 업스트림 서버에 수신 네트워크 트래픽을 항상 라우팅합니다.</td>
  </tr>
  <tr>
  <td><a href="#upstream-keepalive">업스트림 Keepalive</a></td>
  <td><code>upstream-keepalive</code></td>
  <td>업스트림 서버에 대한 최대 유휴 Keepalive 연결 수를 설정합니다.</td>
  </tr>
  </tbody></table>

<br>

  <table>
  <caption>HTTPS 및 TLS/SSL 인증 어노테이션</caption>
  <col width="20%">
  <col width="20%">
  <col width="60%">
  <thead>
  <th>HTTPS 및 TLS/SSL 인증 어노테이션</th>
  <th>이름</th>
  <th>설명</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#appid-auth">{{site.data.keyword.appid_short}} 인증</a></td>
  <td><code>appid-auth</code></td>
  <td>{{site.data.keyword.appid_full_notm}}를 사용하여 앱으로 인증합니다.</td>
  </tr>
  <tr>
  <td><a href="#custom-port">사용자 정의 HTTP 및 HTTPS 포트</a></td>
  <td><code>custom-port</code></td>
  <td>HTTP(포트 80) 및 HTTPS(포트 443) 네트워크 트래픽에 대한 기본 포트를 변경합니다.</td>
  </tr>
  <tr>
  <td><a href="#redirect-to-https">HTTP의 경로를 HTTPS로 재지정</a></td>
  <td><code>redirect-to-https</code></td>
  <td>도메인에서 비보안 HTTP 요청의 경로를 HTTPS로 재지정합니다.</td>
  </tr>
  <tr>
  <td><a href="#hsts">HSTS(HTTP Strict Transport Security)</a></td>
  <td><code>hsts</code></td>
  <td>HTTPS를 통해서만 도메인에 액세스하도록 브라우저를 설정합니다.</td>
  </tr>
  <tr>
  <td><a href="#mutual-auth">상호 인증</a></td>
  <td><code>mutual-auth</code></td>
  <td>ALB에 대한 상호 인증을 구성합니다.</td>
  </tr>
  <tr>
  <td><a href="#ssl-services">SSL 서비스 지원</a></td>
  <td><code>ssl-services</code></td>
  <td>SSL 서비스 지원이 HTTPS가 필요한 업스트림 앱에 트래픽을 암호화하도록 허용합니다.</td>
  </tr>
  </tbody></table>

<br>

<table>
<caption>Istio 어노테이션</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Istio 어노테이션</th>
<th>이름</th>
<th>설명</th>
</thead>
<tbody>
<tr>
<td><a href="#istio-services">Istio 서비스</a></td>
<td><code>istio-services</code></td>
<td>Istio 관리 서비스로 트래픽을 라우팅합니다.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>프록시 버퍼 어노테이션</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>프록시 버퍼 어노테이션</th>
 <th>이름</th>
 <th>설명</th>
 </thead>
 <tbody>
 <tr>
 <td><a href="#proxy-buffering">클라이언트 응답 데이터 버퍼링</a></td>
 <td><code>proxy-buffering</code></td>
 <td>클라이언트로 응답을 보내는 동안 ALB에서 클라이언트 응답의 버퍼링을 사용하지 않도록 설정합니다.</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffers">프록시 버퍼</a></td>
 <td><code>proxy-buffers</code></td>
 <td>프록시된 서버에서 단일 연결에 대한 응답을 읽는 데 사용되는 버퍼의 수와 크기를 설정합니다.</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffer-size">프록시 버퍼 크기</a></td>
 <td><code>proxy-buffer-size</code></td>
 <td>프록시된 서버에서 수신되는 응답의 첫 번째 파트를 읽는 데 사용되는 버퍼의 크기를 설정합니다.</td>
 </tr>
 <tr>
 <td><a href="#proxy-busy-buffers-size">프록시의 사용 중인 버퍼 크기</a></td>
 <td><code>proxy-busy-buffers-size</code></td>
 <td>사용 중인 프록시 버퍼 크기를 설정합니다.</td>
 </tr>
 </tbody></table>

<br>

<table>
<caption>요청 및 응답 어노테이션</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>요청 및 응답 어노테이션</th>
<th>이름</th>
<th>설명</th>
</thead>
<tbody>
<tr>
<td><a href="#proxy-add-headers">추가 클라이언트 요청 또는 응답 헤더</a></td>
<td><code>proxy-add-headers, response-add-headers</code></td>
<td>백엔드 앱에 요청을 전달하기 전에 클라이언트 요청에 헤더 정보를 추가하거나 클라이언트에 응답을 전송하기 전에 클라이언트 응답에 헤더 정보를 추가합니다.</td>
</tr>
<tr>
<td><a href="#response-remove-headers">클라이언트 응답 헤더 제거</a></td>
<td><code>response-remove-headers</code></td>
<td>클라이언트에 응답을 전달하기 전에 클라이언트 응답에서 헤더 정보를 제거합니다.</td>
</tr>
<tr>
<td><a href="#client-max-body-size">클라이언트 요청 본문 크기</a></td>
<td><code>client-max-body-size</code></td>
<td>클라이언트가 요청의 일부로 전송할 수 있는 최대 본문 크기를 설정합니다.</td>
</tr>
<tr>
<td><a href="#large-client-header-buffers">대형 클라이언트 헤더 버퍼</a></td>
<td><code>large-client-header-buffers</code></td>
<td>대형 클라이언트 요청 헤더를 읽는 최대 버퍼의 수 및 크기를 설정합니다.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>서비스 제한 어노테이션</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>서비스 제한 어노테이션</th>
<th>이름</th>
<th>설명</th>
</thead>
<tbody>
<tr>
<td><a href="#global-rate-limit">글로벌 속도 제한</a></td>
<td><code>global-rate-limit</code></td>
<td>모든 서비스의 정의된 키별 연결 수와 요청 처리 속도를 제한합니다.</td>
</tr>
<tr>
<td><a href="#service-rate-limit">서비스 속도 제한</a></td>
<td><code>service-rate-limit</code></td>
<td>특정 서비스의 정의된 키별 연결 수와 요청 처리 속도를 제한합니다.</td>
</tr>
</tbody></table>

<br>



## 일반 어노테이션
{: #general}

### 외부 서비스(proxy-external-service)
{: #proxy-external-service}

외부 서비스(예: {{site.data.keyword.Bluemix_notm}}에서 호스팅되는 서비스)에 경로 정의를 추가합니다.
{:shortdesc}

<dl>
<dt>설명</dt>
<dd>외부 서비스에 경로 정의를 추가합니다. 이 어노테이션은 앱이 백엔드 서비스가 아닌 외부 서비스에서 작동할 때만 사용하십시오. 이 어노테이션을 사용하여 외부 서비스 라우트를 작성하는 경우, `client-max-body-size`, `proxy-read-timeout`, `proxy-connect-timeout`, `proxy-buffering` 어노테이션만 함께 지원됩니다. 기타 어노테이션은 `proxy-external-service`와 함께 지원되지 않습니다.
</dd>
<dt>샘플 Ingress 리소스 YAML</dt>
<dd>

<pre class="codeblock">
<code>
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: cafe-ingress
annotations:
    ingress.bluemix.net/proxy-external-service: "path=&lt;mypath&gt; external-svc=https:&lt;external_service&gt; host=&lt;mydomain&gt;"
spec:
tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 80
</code></pre>

<table>
<caption>어노테이션 컴포넌트 이해</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
 </thead>
 <tbody>
 <tr>
 <td><code>path</code></td>
 <td><code>&lt;<em>mypath</em>&gt;</code>를 외부 서비스에서 청취하는 경로로 대체합니다.</td>
 </tr>
 <tr>
 <td><code>external-svc</code></td>
 <td><code>&lt;<em>external_service</em>&gt;</code>를 호출하려는 외부 서비스로 대체합니다. 예: <code>https://&lt;myservice&gt;.&lt;region&gt;.mybluemix.net</code>.</td>
 </tr>
 <tr>
 <td><code>host</code></td>
 <td><code>&lt;<em>mydomain</em>&gt;</code>을 외부 서비스의 호스트 도메인으로 대체합니다.</td>
 </tr>
 </tbody></table>

 </dd></dl>

<br />


### 위치 수정자(location-modifier)
{: #location-modifier}

ALB가 앱 경로에 대해 요청 URI를 일치시키는 방법을 수정합니다.
{:shortdesc}

<dl>
<dt>설명</dt>
<dd>기본적으로, ALB는 앱이 청취하는 경로를 접두부로 처리합니다. ALB가 앱에 대한 요청을 수신하는 경우 ALB는 요청 URI의 시작과 일치하는 경로(접두부로)의 Ingress 리소스를 처리합니다. 일치 항목이 있으면 요청은 앱이 배치된 팟(Pod)의 IP 주소로 전달됩니다.<br><br>`location-modifier` 어노테이션은 ALB가 위치 블록 구성을 수정하여 일치 항목을 검색하는 방법을 변경합니다. 위치 블록은 앱 경로에 대한 요청을 처리하는 방법을 결정합니다.<br><br><strong>참고</strong>: 정규식(regex) 경로를 처리하기 위해 이 어노테이션이 필요합니다.</dd>

<dt>지원되는 수정자</dt>
<dd>

<table>
<caption>지원되는 수정자</caption>
 <col width="10%">
 <col width="90%">
 <thead>
 <th>수정자</th>
 <th>설명</th>
 </thead>
 <tbody>
 <tr>
 <td><code>=</code></td>
 <td>등호 수정자는 ALB가 정확히 일치하는 항목만 선택하도록 합니다. 정확하게 일치하는 항목을 찾으면 검색을 중지하고 일치하는 경로가 선택됩니다.<br>예를 들어, 앱이 <code>/tea</code>에서 청취하는 경우 ALB가 앱에 대한 요청을 일치시킬 때 정확한 <code>/tea</code> 경로만 선택합니다.</td>
 </tr>
 <tr>
 <td><code>~</code></td>
 <td>물결 기호 수정자는 ALB가 비교 중에 경로를 대소문자를 구분하는 정규식 경로로 처리하도록 합니다.<br>예를 들어, 앱이 <code>/coffee</code>에서 청취하면 경로가 앱에 대해 명시적으로 설정되지 않은 경우에도 ALB가 앱에 대한 요청을 일치시킬 때 <code>/ab/coffee</code> 또는 <code>/123/coffee</code> 경로를 선택할 수 있습니다.</td>
 </tr>
 <tr>
 <td><code>~\*</code></td>
 <td>물결 기호와 그 뒤의 별표는 ALB가 비교 중에 경로를 대소문자를 구분하지 않는 정규식 경로로 처리하도록 합니다.<br>예를 들어, 앱이 <code>/coffee</code>에서 청취하면 경로가 앱에 대해 명시적으로 설정되지 않은 경우에도 ALB가 앱에 대한 요청을 일치시킬 때 <code>/ab/Coffee</code> 또는 <code>/123/COFFEE</code> 경로를 선택할 수 있습니다.</td>
 </tr>
 <tr>
 <td><code>^~</code></td>
 <td>캐럿과 그 뒤의 물결 기호는 ALB가 정규식 경로 대신 정규식이 아닌 최적 일치 항목을 선택하도록 합니다.</td>
 </tr>
 </tbody>
</table>

</dd>

<dt>샘플 Ingress 리소스 YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
annotations:
  ingress.bluemix.net/location-modifier: "modifier='&lt;location_modifier&gt;' serviceName=&lt;myservice1&gt;;modifier='&lt;location_modifier&gt;' serviceName=&lt;myservice2&gt;"
spec:
tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: &lt;myservice&gt;
          servicePort: 80</code></pre>

 <table>
 <caption>어노테이션 컴포넌트 이해</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
  </thead>
  <tbody>
  <tr>
  <td><code>modifier</code></td>
  <td><code>&lt;<em>location_modifier</em>&gt;</code>를 경로에 사용할 위치 수정자로 대체합니다. 지원되는 수정자는 <code>'='</code>, <code>'~'</code>, <code>'~\*'</code> 및 <code>'^~'</code>입니다. 작은따옴표로 수정자를 묶어야 합니다.</td>
  </tr>
  <tr>
  <td><code>serviceName</code></td>
  <td><code>&lt;<em>myservice</em>&gt;</code>를 앱에 대해 작성된 Kubernetes 서비스의 이름으로 대체합니다.</td>
  </tr>
  </tbody></table>
  </dd>
  </dl>

<br />


### 사설 ALB 라우팅(ALB-ID)
{: #alb-id}

사설 ALB를 사용하여 수신 요청을 앱으로 라우팅합니다.
{:shortdesc}

<dl>
<dt>설명</dt>
<dd>
공용 ALB 대신 수신 요청을 라우팅할 개인용 ALB를 선택합니다.</dd>


<dt>샘플 Ingress 리소스 YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
annotations:
    ingress.bluemix.net/ALB-ID: "&lt;private_ALB_ID&gt;"
spec:
tls:
- hosts:
  - mydomain
    secretName: mytlssecret
  rules:
- host: mydomain
  http:
paths:
    - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>&lt;private_ALB_ID&gt;</code></td>
<td>사설 ALB의 ID입니다. 개인용 ALB ID를 찾으려면 <code>bx cs albs --cluster &lt;my_cluster&gt;</code>를 실행하십시오.
</td>
</tr>
</tbody></table>
</dd>
</dl>

<br />


### 경로 재작성(rewrite-path)
{: #rewrite-path}

ALB 도메인 경로의 수신 네트워크 트래픽을 백엔드 애플리케이션이 청취하는 다른 경로로 라우팅합니다.
{:shortdesc}

<dl>
<dt>설명</dt>
<dd>Ingress ALB 도메인은 <code>mykubecluster.us-south.containers.appdomain.cloud/beans</code>의 수신 네트워크 트래픽을 사용자의 앱으로 라우팅합니다. 사용자 앱은 <code>/beans</code> 대신 <code>/coffee</code>를 청취합니다. 수신 네트워크 트래픽을 사용자 앱에 전달하려면 재작성 어노테이션을 Ingress 리소스 구성 파일에 추가하십시오. 재작성 어노테이션에서는 <code>/coffee</code> 경로를 사용하여 <code>/beans</code>의 수신 네트워크 트래픽을 사용자 앱으로 전달할 수 있습니다. 여러 서비스가 포함된 경우에는 세미콜론(;)만 사용하여 이를 구분하십시오.</dd>
<dt>샘플 Ingress 리소스 YAML</dt>
<dd>
<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
annotations:
    ingress.bluemix.net/rewrite-path: "serviceName=&lt;myservice1&gt; rewrite=&lt;target_path1&gt;;serviceName=&lt;myservice2&gt; rewrite=&lt;target_path2&gt;"
spec:
tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /beans
        backend:
          serviceName: myservice1
          servicePort: 80
</code></pre>

<table>
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code>를 앱에 대해 작성된 Kubernetes 서비스의 이름으로 대체합니다.</td>
</tr>
<tr>
<td><code>rewrite</code></td>
<td><code>&lt;<em>target_path</em>&gt;</code>를 앱에서 청취하는 경로로 대체합니다. ALB 도메인의 수신 네트워크 트래픽은 이 경로를 사용하여 Kubernetes 서비스로 전달됩니다. 대부분의 앱은 특정 경로에서 청취하지 않지만 루트 경로와 특정 포트를 사용합니다. 위 예에서 재작성 경로는 <code>/coffee</code>로 정의되었습니다.</td>
</tr>
</tbody></table>

</dd></dl>

<br />


### 애플리케이션 로드 밸런서의 TCP 포트(tcp-ports)
{: #tcp-ports}

비표준 TCP 포트를 통해 앱에 액세스합니다.
{:shortdesc}

<dl>
<dt>설명</dt>
<dd>
TCP 스트림 워크로드를 실행 중인 앱에 이 어노테이션을 사용합니다.

<p>**참고**: ALB는 패스 스루 모드에서 작동하고 백엔드 앱으로 트래픽을 전달합니다. 이 경우에 SSL 종료는 지원되지 않습니다.</p>
</dd>


<dt>샘플 Ingress 리소스 YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
annotations:
  ingress.bluemix.net/tcp-ports: "serviceName=&lt;myservice&gt; ingressPort=&lt;ingress_port&gt; [servicePort=&lt;service_port&gt;]"
spec:
tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: &lt;myservice&gt;
          servicePort: 80</code></pre>

 <table>
 <caption>어노테이션 컴포넌트 이해</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td><code>&lt;<em>myservice</em>&gt;</code>를 비표준 TCP 포트를 통해 액세스할 Kubernetes 서비스의 이름으로 대체합니다.</td>
  </tr>
  <tr>
  <td><code>ingressPort</code></td>
  <td><code>&lt;<em>ingress_port</em>&gt;</code>를 앱에 액세스하려는 TCP 포트로 대체합니다.</td>
  </tr>
  <tr>
  <td><code>servicePort</code></td>
  <td>이 매개변수는 선택사항입니다. 제공되는 경우, 트래픽이 백엔드 앱으로 전송되기 전에 포트를 이 값으로 대체합니다. 제공되지 않으면 포트가 Ingress 포트와 동일하게 유지됩니다.</td>
  </tr>
  </tbody></table>

 </dd>
 <dt>사용</dt>
 <dd><ol><li>ALB를 위한 열린 포트를 검토하십시오.
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
CLI 출력이 다음과 유사하게 나타납니다.
<pre class="screen">
<code>NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx 80:30416/TCP,443:32668/TCP   109d</code></pre></li>
<li>ALB 구성 맵을 여십시오.
<pre class="pre">
<code>kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system</code></pre></li>
<li>TCP 포트를 구성 맵에 추가하십시오. 열려는 TCP 포트로 <code>&lt;port&gt;</code>를 대체하십시오. <b>참고</b>: 기본적으로 포트 80 및 443이 열립니다. 80 및 443을 열린 상태로 유지하려면 `public-ports` 필드에 지정하는 다른 TCP 포트 이외에 이러한 포트를 포함해야 합니다. 사설 ALB를 사용으로 설정한 경우 `private-ports` 필드에도 열린 상태로 유지할 포트를 지정해야 합니다. 자세한 정보는 <a href="cs_ingress.html#opening_ingress_ports">Ingress ALB에서 포트 열기</a>를 참조하십시오.
<pre class="codeblock">
<code>apiVersion: v1
kind: ConfigMap
data:
 public-ports: 80;443;&lt;port1&gt;;&lt;port2&gt;
metadata:
  creationTimestamp: 2017-08-22T19:06:51Z
  name: ibm-cloud-provider-ingress-cm
  namespace: kube-system
  resourceVersion: "1320"
  selfLink: /api/v1/namespaces/kube-system/configmaps/ibm-cloud-provider-ingress-cm
  uid: &lt;uid&gt;</code></pre></li>
 <li>ALB가 TCP 포트로 다시 구성되었는지 확인하십시오.
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
CLI 출력이 다음과 유사하게 나타납니다.
<pre class="screen">
<code>NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx  169.xx.xxx.xxx &lt;port1&gt;:30776/TCP,&lt;port2&gt;:30412/TCP   109d</code></pre></li>
<li>비표준 TCP 포트를 통해 앱에 액세스하도록 Ingress를 구성하십시오. 이 참조서에 있는 샘플 YAML 파일을 사용하십시오. </li>
<li>ALB 구성을 업데이트하십시오.
<pre class="pre">
<code>         kubectl apply -f myingress.yaml
        </code></pre>
</li>
<li>선호하는 웹 브라우저를 열어 앱에 액세스하십시오. 예를 들면, <code>https://&lt;ibmdomain&gt;:&lt;ingressPort&gt;/</code>입니다.</li></ol></dd></dl>

<br />


## 연결 어노테이션
{: #connection}

### 사용자 정의 연결 제한시간 및 읽기 제한시간(proxy-connect-timeout, proxy-read-timeout)
{: #proxy-connect-timeout}

백엔드 앱이 사용할 수 없는 것으로 간주되기 전에 백엔드 앱에 연결하고 백엔드 앱에서 읽기 위해 ALB가 대기하는 시간을 설정합니다.
{:shortdesc}

<dl>
<dt>설명</dt>
<dd>클라이언트 요청이 Ingress ALB로 전송될 때 ALB가 백엔드 앱에 대한 연결을 엽니다. 기본적으로 ALB는 백엔드 앱에서 응답을 수신하기 위해 60초간 대기합니다. 백엔드 앱에서 60초 내에 응답하지 않으면 연결 요청이 중단되며 백엔드 앱이 사용 불가능한 것으로 간주됩니다.

</br></br>
ALB가 백엔드 앱에 연결되고 나면 ALB에서 백엔드 앱의 응답 데이터를 읽습니다. 이 읽기 조작 중에는 ALB가 백엔드 앱에서 데이터를 받기 위한 두 읽기 작업 사이에 최대 60초간 대기합니다. 백엔드 앱에서 60초 내에 데이터를 보내지 않으면 백엔드 앱에 대한 연결이 닫히며 앱을 사용할 수 없는 것으로 간주합니다.
</br></br>
프록시에서는 60초 연결 제한시간 및 읽기 제한시간이 기본 제한시간이며 변경하지 않아야 합니다.
</br></br>
앱의 가용성이 안정되지 않거나 높은 워크로드 때문에 앱이 느리게 응답하는 경우 연결 제한시간 또는 읽기 제한시간을 늘릴 수 있습니다. 제한시간에 도달할 때까지 백엔드 앱에 대한 연결이 열린 상태로 있어야 하므로 제한시간을 늘리면 ALB의 성능에 영향을 미칠 수 있습니다.
</br></br>
반면에 ALB의 성능을 높이기 위해 제한시간을 줄일 수도 있습니다. 워크로드가 높은 경우에도 백엔드 앱에서 지정된 제한시간 내에 요청을 처리할 수 있는지 확인하십시오.</dd>
<dt>샘플 Ingress 리소스 YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
annotations:
   ingress.bluemix.net/proxy-connect-timeout: "serviceName=&lt;myservice&gt; timeout=&lt;connect_timeout&gt;"
   ingress.bluemix.net/proxy-read-timeout: "serviceName=&lt;myservice&gt; timeout=&lt;read_timeout&gt;"
spec:
tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>어노테이션 컴포넌트 이해</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;connect_timeout&gt;</code></td>
 <td>백엔드 앱에 연결하기 위해 대기하는 시간(초 또는 분)입니다(예: <code>65s</code> 또는 <code>2m</code>). <strong>참고:</strong> 연결 제한시간은 75초를 초과할 수 없습니다.</td>
 </tr>
 <tr>
 <td><code>&lt;read_timeout&gt;</code></td>
 <td>백엔드 앱을 읽기 전에 대기하는 시간(초 또는 분)입니다(예: <code>65s</code> 또는 <code>2m</code>). </tr>
 </tbody></table>

 </dd></dl>

<br />



### Keepalive 요청(keepalive-requests)
{: #keepalive-requests}

하나의 Keepalive 연결을 통해 서비스할 수 있는 최대 요청 수를 설정합니다.
{:shortdesc}

<dl>
<dt>설명</dt>
<dd>
 하나의 Keepalive 연결을 통해 제공할 수 있는 최대 요청 수를 설정합니다.
</dd>


<dt>샘플 Ingress 리소스 YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
annotations:
  ingress.bluemix.net/keepalive-requests: "serviceName=&lt;myservice&gt; requests=&lt;max_requests&gt;"
spec:
tls:
- hosts:
  - mydomain
    secretName: mytlssecret
  rules:
- host: mydomain
  http:
paths:
    - path: /
      backend:
        serviceName: &lt;myservice&gt;
        servicePort: 8080</code></pre>

<table>
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code>를 앱에 대해 작성된 Kubernetes 서비스의 이름으로 대체합니다. 이 매개변수는 선택사항입니다. 서비스가 지정되지 않으면 구성은 Ingress 호스트의 모든 서비스에 적용됩니다. 매개변수가 제공되면 Keepalive 요청이 지정된 서비스에 설정됩니다. 매개변수가 제공되지 않으면 Keepalive 요청이 구성되지 않은 모든 서비스에 대해 <code>nginx.conf</code>의 서버 레벨에서 Keepalive 요청이 설정됩니다.</td>
</tr>
<tr>
<td><code>requests</code></td>
<td><code>&lt;<em>max_requests</em>&gt;</code>를 Keepalive 연결을 통해 제공할 수 있는 최대 요청 수로 대체합니다.</td>
</tr>
</tbody></table>

</dd>
</dl>

<br />



### Keepalive 제한시간(keepalive-timeout)
{: #keepalive-timeout}

서버에서 Keepalive 연결이 열려 있는 최대 시간을 설정합니다.
{:shortdesc}

<dl>
<dt>설명</dt>
<dd>
서버에서 Keepalive 연결이 열려 있는 최대 시간을 설정합니다.
</dd>


<dt>샘플 Ingress 리소스 YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
annotations:
   ingress.bluemix.net/keepalive-timeout: "serviceName=&lt;myservice&gt; timeout=&lt;time&gt;s"
spec:
tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>어노테이션 컴포넌트 이해</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
 </thead>
 <tbody>
 <tr>
 <td><code>serviceName</code></td>
 <td><code>&lt;<em>myservice</em>&gt;</code>를 앱에 대해 작성된 Kubernetes 서비스의 이름으로 대체합니다. 이 매개변수는 선택사항입니다. 매개변수가 제공되면 Keepalive 제한시간이 지정된 서비스에 설정됩니다. 매개변수가 제공되지 않으면 Keepalive 제한시간이 구성되지 않은 모든 서비스에 대해 <code>nginx.conf</code>의 서버 레벨에서 Keepalive 제한시간이 설정됩니다.</td>
 </tr>
 <tr>
 <td><code>timeout</code></td>
 <td><code>&lt;<em>time</em>&gt;</code>을 시간(초)으로 대체합니다. 예: <code>timeout=20s</code>. 0 값을 사용하면 Keepalive 클라이언트 연결을 사용할 수 없습니다.</td>
 </tr>
 </tbody></table>

 </dd>
 </dl>

<br />


### 다음 업스트림 프록시(proxy-next-upstream-config)
{: #proxy-next-upstream-config}

ALB가 다음 업스트림 서버로 요청을 전달할 수 있는 시점을 설정합니다.
{:shortdesc}

<dl>
<dt>설명</dt>
<dd>
Ingress ALB가 클라이언트 앱과 사용자의 앱 간의 프록시 역할을 수행합니다. 일부 앱 설정은 ALB로부터 수신되는 클라이언트 요청을 처리하는 여러 업스트림 서버를 필요로 합니다. ALB가 사용하는 프록시 서버가 앱이 사용하는 업스트림 서버와의 연결을 설정하지 못하는 경우가 있습니다. 이 경우 ALB는 다음 업스트림 서버와 연결을 설정하여 여기에 대신 요청을 전달하려 할 수 있습니다. 사용자는 `proxy-next-upstream-config` 어노테이션을 사용하여 ALB가 다음 업스트림 서버로 요청을 전달할 수 있는 경우, 시간 및 횟수를 설정할 수 있습니다.<br><br><strong>참고</strong>: `proxy-next-upstream-config`를 사용할 때는 제한시간이 항상 구성되므로 이 어노테이션에 `timeout=true`를 추가하지 마십시오.
</dd>
<dt>샘플 Ingress 리소스 YAML</dt>
<dd>
<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
annotations:
    ingress.bluemix.net/proxy-next-upstream-config: "serviceName=&lt;myservice1&gt; retries=&lt;tries&gt; timeout=&lt;time&gt; error=true http_502=true; serviceName=&lt;myservice2&gt; http_403=true non_idempotent=true"
spec:
tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice1
          servicePort: 80
</code></pre>

<table>
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code>를 앱에 대해 작성된 Kubernetes 서비스의 이름으로 대체합니다.</td>
</tr>
<tr>
<td><code>retries</code></td>
<td><code>&lt;<em>tries</em>&gt;</code>를 ALB가 다음 업스트림 서버로 요청을 전달하려 시도하는 최대 횟수로 대체하십시오. 이 숫자는 원래 요청을 포함합니다. 이 한계를 해제하려면 <code>0</code>을 사용하십시오. 값을 지정하지 않으면 기본값 <code>0</code>이 사용됩니다.
</td>
</tr>
<tr>
<td><code>timeout</code></td>
<td><code>&lt;<em>time</em>&gt;</code>을 ALB가 다음 업스트림 서버로 요청을 전달하려 시도하는 최대 시간(초)으로 대체하십시오. 예를 들어, 30초를 설정하려면 <code>30s</code>를 입력하십시오. 이 한계를 해제하려면 <code>0</code>을 사용하십시오. 값을 지정하지 않으면 기본값 <code>0</code>이 사용됩니다.
</td>
</tr>
<tr>
<td><code>error</code></td>
<td><code>true</code>로 설정되면 첫 번째 업스트림 서버와 연결을 설정하거나, 여기에 요청을 전달하거나, 응답 헤더를 읽는 중에 오류가 발생하는 경우 ALB가 요청을 다음 업스트림 서버로 전달합니다.
</td>
</tr>
<tr>
<td><code>invalid_header</code></td>
<td><code>true</code>로 설정되면 첫 번째 업스트림 서버가 비어 있거나 올바르지 않은 응답을 리턴하는 경우 ALB가 요청을 다음 업스트림 서버로 전달합니다.
</td>
</tr>
<tr>
<td><code>http_502</code></td>
<td><code>true</code>로 설정되면 첫 번째 업스트림 서버가 코드 502를 포함하는 응답을 리턴하는 경우 ALB가 요청을 다음 업스트림 서버로 전달합니다. HTTP 응답 코드 <code>500</code>, <code>502</code>, <code>503</code>, <code>504</code>, <code>403</code>, <code>404</code>, <code>429</code>를 지정할 수 있습니다.
</td>
</tr>
<tr>
<td><code>non_idempotent</code></td>
<td><code>true</code>로 설정되면 ALB가 멱등이 아닌 메소드를 사용한 요청을 다음 업스트림 서버로 전달할 수 있습니다. 기본적으로 ALB는 이러한 요청을 다음 업스트림 서버로 전달하지 않습니다.
</td>
</tr>
<tr>
<td><code>off</code></td>
<td>ALB가 다음 업스트림 서버로 요청을 전달하지 않도록 하려면 <code>true</code>로 설정하십시오.
</td>
</tr>
</tbody></table>
</dd>
</dl>

<br />


### 쿠키에 대한 세션 선호도(sticky-cookie-services)
{: #sticky-cookie-services}

ALB에 세션 선호도를 추가하고 수신 네트워크 트래픽을 항상 동일한 업스트림 서버로 라우팅하려면 스티키 쿠키 어노테이션을 사용하십시오.
{:shortdesc}

<dl>
<dt>설명</dt>
<dd>고가용성을 위해 앱 설정에 따라 수신 클라이언트 요청을 처리하는 여러 업스트림 서버를 배치해야 합니다. 클라이언트가 백엔드 앱에 연결할 때 세션 지속 기간 동안 또는 태스크를 완료할 때까지 소요되는 시간 동안 동일한 업스트림 서버에서 클라이언트에 서비스를 제공하도록 세션 선호도를 사용할 수 있습니다. 수신 네트워크 트래픽이 항상 동일한 업스트림 서버로 라우팅되게 하여 세션 선호도를 보장하도록 ALB를 구성할 수 있습니다.

</br></br>
백엔드 앱에 연결하는 모든 클라이언트는 ALB를 통해 사용 가능한 업스트림 서버 중 하나에 지정됩니다. ALB는 클라이언트 앱에 저장되는 세션 쿠키를 작성하며, 이 쿠키는 애플리케이션 로드 밸런서와 클라이언트 간 모든 요청의 헤더 정보에 포함됩니다. 쿠키의 정보를 사용하면 세션 전체에서 동일한 업스트림 서버가 모든 요청을 처리할 수 있습니다.

</br></br>
여러 서비스가 포함된 경우에는 세미콜론(;)을 사용하여 구분하십시오.</dd>
<dt>샘플 Ingress 리소스 YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
annotations:
    ingress.bluemix.net/sticky-cookie-services: "serviceName=&lt;myservice1&gt; name=&lt;cookie_name1&gt; expires=&lt;expiration_time1&gt; path=&lt;cookie_path1&gt; hash=&lt;hash_algorithm1&gt;;serviceName=&lt;myservice2&gt; name=&lt;cookie_name2&gt; expires=&lt;expiration_time2&gt; path=&lt;cookie_path2&gt; hash=&lt;hash_algorithm2&gt;"
spec:
tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: &lt;myservice1&gt;
          servicePort: 8080
      - path: /service2_path
        backend:
          serviceName: &lt;myservice2&gt;
          servicePort: 80</code></pre>

  <table>
  <caption>어노테이션 컴포넌트 이해</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td><code>&lt;<em>myservice</em>&gt;</code>를 앱에 대해 작성된 Kubernetes 서비스의 이름으로 대체합니다.</td>
  </tr>
  <tr>
  <td><code>name</code></td>
  <td><code>&lt;<em>cookie_name</em>&gt;</code>을 세션 중 작성된 스티키 쿠키의 이름으로 대체합니다.</td>
  </tr>
  <tr>
  <td><code>expires</code></td>
  <td>스티키 쿠키가 만료되기 전에 <code>&lt;<em>expiration_time</em>&gt;</code>을 초, 분 또는 시간 단위의 시간으로 대체합니다. 이 시간은 사용자 활동과 무관합니다. 쿠키가 만료되고 나면 클라이언트 웹 브라우저에서 쿠키가 삭제되어 더 이상 ALB로 전송되지 않습니다. 예를 들어, 만기 시간을 1초, 1분 또는 1시간으로 설정하려면 <code>1s</code>, <code>1m</code> 또는 <code>1h</code>를 입력하십시오.</td>
  </tr>
  <tr>
  <td><code>path</code></td>
  <td><code>&lt;<em>cookie_path</em>&gt;</code>를 Ingress 하위 도메인에 추가되는 경로로 대체합니다. 이 경로는 ALB로 쿠키가 전송되는 도메인과 하위 도메인을 표시합니다. 예를 들어, Ingress 도메인이 <code>www.myingress.com</code>이고 모든 클라이언트 요청에 해당 쿠키를 전송하려는 경우 <code>path=/</code>를 설정해야 합니다. <code>www.myingress.com/myapp</code> 및 해당 하위 도메인 모두에 대해서만 쿠키를 전송하려는 경우 <code>path=/myapp</code>으로 설정해야 합니다.</td>
  </tr>
  <tr>
  <td><code>hash</code></td>
  <td><code>&lt;<em>hash_algorithm</em>&gt;</code>를 쿠키의 정보를 보호하는 해시 알고리즘으로 대체합니다. <code>sha1</code>만 지원됩니다. SHA1은 쿠키의 정보를 기반으로 해시 합계를 작성하고 이 해시 합계를 쿠키에 추가합니다. 서버에서 쿠키의 정보를 복호화하고 데이터 무결성을 확인할 수 있습니다.</td>
  </tr>
  </tbody></table>

 </dd></dl>

<br />




### 업스트림 Keepalive(upstream-keepalive)
{: #upstream-keepalive}

업스트림 서버에 대한 최대 유휴 Keepalive 연결 수를 설정합니다.
{:shortdesc}

<dl>
<dt>설명</dt>
<dd>
지정된 서비스의 업스트림 서버에 대한 유휴 Keepalive 연결의 최대 수를 설정합니다. 업스트림 서버에는 기본적으로 64개의 유휴 Keepalive 연결이 있습니다.
</dd>


 <dt>샘플 Ingress 리소스 YAML</dt>
 <dd>

 <pre class="codeblock">
 <code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
annotations:
    ingress.bluemix.net/upstream-keepalive: "serviceName=&lt;myservice&gt; keepalive=&lt;max_connections&gt;"
spec:
tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

 <table>
 <caption>어노테이션 컴포넌트 이해</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td><code>&lt;<em>myservice</em>&gt;</code>를 앱에 대해 작성된 Kubernetes 서비스의 이름으로 대체합니다.</td>
  </tr>
  <tr>
  <td><code>keepalive</code></td>
  <td><code>&lt;<em>max_connections</em>&gt;</code>를 업스트림 서버에 대한 유휴 Keepalive 연결의 최대 수로 대체합니다. 기본값은 <code>64</code>입니다. <code>0</code> 값을 사용하면 지정된 서비스의 업스트림 Keepalive 연결을 사용할 수 없습니다.</td>
  </tr>
  </tbody></table>
  </dd>
  </dl>

<br />




## HTTPS 및 TLS/SSL 인증 어노테이션
{: #https-auth}

### {{site.data.keyword.appid_short_notm}} 인증(appid-auth)
{: #appid-auth}

  {{site.data.keyword.appid_full_notm}}를 사용하여 애플리케이션으로 인증합니다.
  {:shortdesc}

  <dl>
  <dt>설명</dt>
  <dd>
  {{site.data.keyword.appid_short_notm}}를 사용하여 웹 또는 API HTTP/HTTPS 요청을 인증합니다.

  <p>요청 유형을 <code>web</code>으로 설정하면 {{site.data.keyword.appid_short_notm}} 액세스 토큰이 포함된 웹 요청이 유효성 검증됩니다. 토큰 유효성 검증에 실패하는 경우 웹 요청이 거부됩니다. 요청에 액세스 토큰이 포함되지 않으면 요청이 {{site.data.keyword.appid_short_notm}} 로그인 페이지로 경로 재지정됩니다. <strong>참고</strong>: {{site.data.keyword.appid_short_notm}} 웹 인증이 작동하려면 사용자의 브라우저에서 쿠키가 사용으로 설정되어야 합니다.</p>

  <p>요청 유형을 <code>api</code>로 설정하면 {{site.data.keyword.appid_short_notm}} 액세스 토큰이 포함된 API 요청이 유효성 검증됩니다. 요청에 액세스 토큰이 포함되지 않으면 <code>401: Unauthorized</code> 오류 메시지가 사용자에게 리턴됩니다.</p>

  <p>**참고**: 보안상의 이유로, {{site.data.keyword.appid_short_notm}} 인증은 TLS/SSL을 사용하는 백엔드만 지원합니다.</p>
  </dd>
   <dt>샘플 Ingress 리소스 YAML</dt>
   <dd>

   <pre class="codeblock">
   <code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
annotations:
      ingress.bluemix.net/appid-auth: "bindSecret=&lt;bind_secret&gt; namespace=&lt;namespace&gt; requestType=&lt;request_type&gt; serviceName=&lt;myservice&gt;"
spec:
tls:
    - hosts:
      - mydomain
    secretName: mytlssecret
  rules:
    - host: mydomain
    http:
      paths:
        - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

   <table>
   <caption>어노테이션 컴포넌트 이해</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code>bindSecret</code></td>
    <td><em><code>&lt;bind_secret&gt;</code></em>을 바인드 시크릿을 저장하는 Kubernetes 시크릿으로 대체합니다.</td>
    </tr>
    <tr>
    <td><code>namespace</code></td>
    <td><em><code>&lt;namespace&gt;</code></em>를 바인드 시크릿으로 대체합니다. 이 필드의 기본값은 `default` 네임스페이스입니다.</td>
    </tr>
    <tr>
    <td><code>requestType</code></td>
    <td><code><em>&lt;request_type&gt;</em></code>을 {{site.data.keyword.appid_short_notm}}에 전송할 요청의 유형으로 대체합니다. 허용되는 값은 `web` 또는 `api`입니다. 기본값은 `api`입니다.</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td><code><em>&lt;myservice&gt;</em></code>를 앱에 대해 작성된 Kubernetes 서비스의 이름으로 대체합니다. 이 필드는 선택사항입니다. 서비스 이름이 포함되지 않으면 어노테이션은 모든 서비스에 대해 사용으로 설정됩니다.  서비스 이름이 포함되면 어노테이션은 해당 서비스에 대해서만 사용으로 설정됩니다. 여러 서비스는 세미콜론(;)으로 구분하십시오.</td>
    </tr>
    </tbody></table>
    </dd>
    <dt>사용</dt>
    <dd>애플리케이션이 인증을 위해 {{site.data.keyword.appid_short_notm}}를 사용하므로 {{site.data.keyword.appid_short_notm}} 인스턴스를 프로비저닝하고 유효한 경로 재지정 URI로 인스턴스를 구성하며 바인드 시크릿을 생성해야 합니다.
    <ol>
    <li>[{{site.data.keyword.appid_short_notm}} 인스턴스](https://console.bluemix.net/catalog/services/app-id)를 프로비저닝하십시오.</li>
    <li>{{site.data.keyword.appid_short_notm}} 관리 콘솔에서 앱의 redirectURI를 추가하십시오.</li>
    <li>바인드 시크릿을 작성하십시오.
    <pre class="pre"><code>bx cs cluster-service-bind &lt;my_cluster&gt; &lt;my_namespace&gt; &lt;my_service_instance_GUID&gt;</code></pre> </li>
    <li><code>appid-auth</code> 어노테이션을 구성하십시오.</li>
    </ol></dd>
    </dl>

<br />



### 사용자 정의 HTTP 및 HTTPS 포트(custom-port)
{: #custom-port}

HTTP(포트 80) 및 HTTPS(포트 443) 네트워크 트래픽에 대한 기본 포트를 변경합니다.
{:shortdesc}

<dl>
<dt>설명</dt>
<dd>기본적으로 Ingress ALB는 수신 HTTP 네트워크 트래픽을 포트 80에서 청취하고 수신 HTTPS 네트워크 트래픽은 포트 443에서 청취하도록 구성됩니다. ALB 도메인에 보안을 추가하거나 HTTPS 포트만 사용하도록 기본 포트를 변경할 수 있습니다.
</dd>


<dt>샘플 Ingress 리소스 YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
annotations:
    ingress.bluemix.net/custom-port: "protocol=&lt;protocol1&gt; port=&lt;port1&gt;;protocol=&lt;protocol2&gt;port=&lt;port2&gt;"
spec:
tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>어노테이션 컴포넌트 이해</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;protocol&gt;</code></td>
 <td>수신 HTTP 또는 HTTPS 네트워크 트래픽의 기본 포트를 변경하려면 <code>http</code> 또는 <code>https</code>를 입력하십시오.</td>
 </tr>
 <tr>
 <td><code>&lt;port&gt;</code></td>
 <td>수신 HTTP 또는 HTTPS 네트워크 트래픽에 사용할 포트 번호를 입력하십시오.  <p><strong>참고:</strong> HTTP 또는 HTTPS에 대한 사용자 정의 포트가 지정된 경우 HTTP 및 HTTPS에 대한 기본 포트가 더 이상 유효하지 않습니다. 예를 들어, HTTPS에 대한 기본 포트를 8443으로 변경하지만 HTTP에는 기본 포트를 사용하려면 <code>custom-port: "protocol=http port=80; protocol=https port=8443"</code>과 같이 둘 다에 대한 사용자 정의 포트를 설정해야 합니다.</p></td>
 </tr>
 </tbody></table>

 </dd>
 <dt>사용</dt>
 <dd><ol><li>ALB를 위한 열린 포트를 검토하십시오.
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
CLI 출력이 다음과 유사하게 나타납니다.
<pre class="screen">
<code>NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx 80:30416/TCP,443:32668/TCP   109d</code></pre></li>
<li>ALB 구성 맵을 여십시오.
<pre class="pre">
<code>kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system</code></pre></li>
<li>기본이 아닌 HTTP 및 HTTPS 포트를 구성 맵에 추가하십시오. 열려는 HTTP 또는 HTTPS 포트로 &lt;port&gt;를 대체하십시오. <b>참고</b>: 기본적으로 포트 80 및 443이 열립니다. 80 및 443을 열린 상태로 유지하려면 `public-ports` 필드에 지정하는 다른 TCP 포트 이외에 이러한 포트를 포함해야 합니다. 사설 ALB를 사용으로 설정한 경우 `private-ports` 필드에도 열린 상태로 유지할 포트를 지정해야 합니다. 자세한 정보는 <a href="cs_ingress.html#opening_ingress_ports">Ingress ALB에서 포트 열기</a>를 참조하십시오.
<pre class="codeblock">
<code>apiVersion: v1
kind: ConfigMap
data:
  public-ports: &lt;port1&gt;;&lt;port2&gt;
metadata:
  creationTimestamp: 2017-08-22T19:06:51Z
  name: ibm-cloud-provider-ingress-cm
  namespace: kube-system
  resourceVersion: "1320"
  selfLink: /api/v1/namespaces/kube-system/configmaps/ibm-cloud-provider-ingress-cm
  uid: &lt;uid&gt;</code></pre></li>
 <li>ALB가 기본이 아닌 포트로 다시 구성되었는지 확인하십시오.
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
CLI 출력이 다음과 유사하게 나타납니다.
<pre class="screen">
<code>NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx  169.xx.xxx.xxx &lt;port1&gt;:30776/TCP,&lt;port2&gt;:30412/TCP   109d</code></pre></li>
<li>수신 네트워크 트래픽을 서버로 라우팅할 때 기본이 아닌 포트를 사용하도록 Ingress를 구성하십시오. 이 참조서에 있는 샘플 YAML 파일을 사용하십시오. </li>
<li>ALB 구성을 업데이트하십시오.
<pre class="pre">
<code>         kubectl apply -f myingress.yaml
        </code></pre>
</li>
<li>선호하는 웹 브라우저를 열어 앱에 액세스하십시오. 예: <code>https://&lt;ibmdomain&gt;:&lt;port&gt;/&lt;service_path&gt;/</code></li></ol></dd></dl>

<br />



### HTTP가 HTTPS로 경로 재지정됨(redirect-to-https)
{: #redirect-to-https}

비보안 HTTP 클라이언트 요청을 HTTPS로 변환합니다.
{:shortdesc}

<dl>
<dt>설명</dt>
<dd>IBM 제공 TLS 인증서 또는 사용자 정의 TLS 인증서로 도메인을 보호하도록 Ingress ALB를 설정합니다. 일부 사용자는 <code>https</code>를 사용하지 않고 ALB 도메인에 대한 비보안 <code>http</code> 요청을 사용하여 앱에 액세스하려고 할 수 있습니다(예: <code>http://www.myingress.com</code>). 경로 재지정 어노테이션을 사용하여 항상 비보안 HTTP 요청을 HTTPS로 변환할 수 있습니다. 이 어노테이션을 사용하지 않으면 기본적으로 비보안 HTTP 요청이 HTTPS 요청으로 변환되지 않으며 암호화되지 않은 기밀 정보를 공용으로 노출할 수 있습니다.

</br></br>
HTTP 요청의 경로를 HTTPS로 재지정하는 기능은 기본적으로 사용되지 않습니다.</dd>

<dt>샘플 Ingress 리소스 YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
annotations:
    ingress.bluemix.net/redirect-to-https: "True"
spec:
tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

</dd>

</dl>

<br />


### HSTS(HTTP Strict Transport Security)
{: #hsts}

<dl>
<dt>설명</dt>
<dd>
HSTS는 HTTPS를 사용해서만 도메인에 액세스하도록 브라우저에 지시합니다. 사용자가 일반 HTTP 링크를 입력하거나 따르는 경우에도 브라우저는 연결을 HTTPS로 엄격하게 업그레이드합니다.
</dd>


<dt>샘플 Ingress 리소스 YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
annotations:
    ingress.bluemix.net/hsts: enabled=true maxAge=&lt;31536000&gt; includeSubdomains=true
spec:
tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: myservice1
          servicePort: 8443
      - path: /service2_path
        backend:
          serviceName: myservice2
          servicePort: 8444
          </code></pre>

<table>
<caption>어노테이션 컴포넌트 이해</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
  </thead>
  <tbody>
  <tr>
  <td><code>enabled</code></td>
  <td><code>true</code>를 사용하면 HSTS를 사용으로 설정합니다.</td>
  </tr>
    <tr>
  <td><code>maxAge</code></td>
  <td><code>&lt;<em>31536000</em>&gt;</code>을 브라우저가 바로 HTTP에 전송 요청을 캐시하는 기간(초)을 나타내는 정수로 대체합니다. 기본값은 <code>31536000</code>로, 1년과 같습니다.</td>
  </tr>
  <tr>
  <td><code>includeSubdomains</code></td>
  <td><code>true</code>를 사용하면 HSTS 정책이 현재 도메인의 모든 하위 도메인에도 적용됨을 브라우저에게 알려줍니다. 기본값은 <code>true</code>입니다. </td>
  </tr>
  </tbody></table>

  </dd>
  </dl>


<br />


### 상호 인증(mutual-auth)
{: #mutual-auth}

ALB에 대한 상호 인증을 구성합니다.
{:shortdesc}

<dl>
<dt>설명</dt>
<dd>
Ingress ALB에 대한 다운스트림 트래픽의 상호 인증을 구성합니다. 외부 클라이언트가 서버를 인증하고 또한 서버는 인증서를 사용하여 클라이언트를 인증합니다. 상호 인증은 인증서 기반 인증 또는 양방향 인증이라고도 합니다.
</dd>

<dt>전제조건</dt>
<dd>
<ul>
<li>[필수 인증 기관(CA)이 포함된 올바른 시크릿을 가져야 합니다](cs_app.html#secrets). <code>client.key</code> 및 <code>client.crt</code>는 상호 인증으로 인증해야 합니다.</li>
<li>443 외 포트에서의 상호 인증을 가능하게 하려면 [유효한 포트를 열도록 ALB를 구성](cs_ingress.html#opening_ingress_ports)하십시오.</li>
</ul>
</dd>


<dt>샘플 Ingress 리소스 YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
annotations:
  ingress.bluemix.net/mutual-auth: "secretName=&lt;mysecret&gt; port=&lt;port&gt; serviceName=&lt;servicename1&gt;,&lt;servicename2&gt;"
spec:
tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>secretName</code></td>
<td><code>&lt;<em>mysecret</em>&gt;</code>을 시크릿 리소스의 이름으로 대체합니다.</td>
</tr>
<tr>
<td><code>port</code></td>
<td><code>&lt;<em>port</em>&gt;</code>를 ALB 포트 번호로 대체하십시오.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>servicename</em>&gt;</code>을 하나 이상의 Ingress 리소스의 이름으로 대체하십시오. 이 매개변수는 선택사항입니다.</td>
</tr>
</tbody></table>

</dd>
</dl>

<br />



### SSL 서비스 지원(ssl-services)
{: #ssl-services}

HTTPS 요청을 허용하고 업스트림 앱에 대한 트래픽을 암호화합니다.
{:shortdesc}

<dl>
<dt>설명</dt>
<dd>
Ingress에서 HTTPS가 필요한 업스트림 앱에 전송하는 트래픽을 암호화합니다. 업스트림 앱이 TLS를 처리할 수 있는 경우 선택적으로 TLS 시크릿에 포함된 인증서를 제공할 수 있습니다.<br></br>**선택사항**: 이 어노테이션에 [단방향 인증 또는 상호 인증](#ssl-services-auth)을 추가할 수 있습니다.</dd>


<dt>샘플 Ingress 리소스 YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
name: &lt;myingressname&gt;
annotations:
    ingress.bluemix.net/ssl-services: "ssl-service=&lt;myservice1&gt; [ssl-secret=&lt;service1-ssl-secret&gt;];ssl-service=&lt;myservice2&gt; [ssl-secret=&lt;service2-ssl-secret&gt;]
spec:
rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: myservice1
          servicePort: 8443
      - path: /service2_path
        backend:
          serviceName: myservice2
          servicePort: 8444</code></pre>

<table>
<caption>어노테이션 컴포넌트 이해</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
  </thead>
  <tbody>
  <tr>
  <td><code>ssl-service</code></td>
  <td><code>&lt;<em>myservice</em>&gt;</code>를 HTTPS를 필요로 하는 서비스의 이름으로 대체하십시오. ALB에서 이 앱의 서비스로의 트래픽이 암호화됩니다.</td>
  </tr>
  <tr>
  <td><code>ssl-secret</code></td>
  <td>선택사항: TLS 시크릿을 사용하려고 하고 업스트림 앱이 TLS를 처리할 수 있는 경우 <code>&lt;<em>service-ssl-secret</em>&gt;</code>을 서비스에 대한 시크릿으로 대체하십시오. 시크릿을 제공하는 경우 앱이 클라이언트로부터 예상하는 <code>trusted.crt</code>, <code>client.crt</code> 및 <code>client.key</code>가 값에 포함되어야 합니다. TLS 시크릿을 작성하려면 먼저 [인증서와 키를 base-64로 변환 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.base64encode.org/)을 수행하십시오. 그런 다음 [시크릿 작성](cs_app.html#secrets)을 참조하십시오.</td>
  </tr>
  </tbody></table>

  </dd>
</dl>

<br />


#### 인증을 통한 SSL 서비스 지원
{: #ssl-services-auth}

<dl>
<dt>설명</dt>
<dd>
HTTPS 요청을 허용하고 추가 보안을 위해 단방향 또는 상호 인증을 사용하여 업스트림 앱에 대한 트래픽을 암호화합니다.
</dd>


<dt>샘플 Ingress 리소스 YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
name: &lt;myingressname&gt;
annotations:
    ingress.bluemix.net/ssl-services: |
      ssl-service=&lt;myservice1&gt; ssl-secret=&lt;service1-ssl-secret&gt;;
      ssl-service=&lt;myservice2&gt; ssl-secret=&lt;service2-ssl-secret&gt;
spec:
tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: myservice1
          servicePort: 8443
      - path: /service2_path
        backend:
          serviceName: myservice2
          servicePort: 8444
          </code></pre>

<table>
<caption>어노테이션 컴포넌트 이해</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
  </thead>
  <tbody>
  <tr>
  <td><code>ssl-service</code></td>
  <td><code>&lt;<em>myservice</em>&gt;</code>를 HTTPS를 필요로 하는 서비스의 이름으로 대체하십시오. ALB에서 이 앱의 서비스로의 트래픽이 암호화됩니다.</td>
  </tr>
  <tr>
  <td><code>ssl-secret</code></td>
  <td><code>&lt;<em>service-ssl-secret</em>&gt;</code>을 서비스에 대한 상호 인증 시크릿으로 대체하십시오. 앱이 클라이언트로부터 예상하는 CA 인증서가 값에 포함되어야 합니다. 상호 인증 시크릿을 작성하려면 먼저 [인증서와 키를 base-64로 변환 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.base64encode.org/)을 수행하십시오. 그런 다음 [시크릿 작성](cs_app.html#secrets)을 참조하십시오.</td>
  </tr>
  </tbody></table>

  </dd>
  </dl>

<br />


## Istio 어노테이션
{: #istio-annotations}

### Istio 서비스(istio-services)
{: #istio-services}

  Istio 관리 서비스로 트래픽을 라우팅합니다.
  {:shortdesc}

  <dl>
  <dt>설명</dt>
  <dd>
  Istio 관리 서비스가 있는 경우에는 클러스터 ALB를 사용하여 HTTP/HTTPS 요청을 Istio Ingress 제어기로 라우팅할 수 있습니다. 그 후 Istio Ingress 제어기는 요청을 앱 서비스로 라우팅합니다. 트래픽을 라우팅하려면 클러스터 ALB 및 Istio Ingress 제어기에 대한 Ingress 리소스를 모두 변경해야 합니다.
    <br><br>클러스터 ALB에 대한 Ingress 리소스에서는 다음 작업을 수행해야 합니다.
      <ul>
        <li>`istio-services` 어노테이션 지정</li>
        <li>서비스 경로를 앱이 청취하는 실제 경로로 정의</li>
        <li>서비스 포트를 Istio Ingress 제어기의 포트로 정의</li>
      </ul>
    <br>Istio Ingress 제어기에 대한 Ingress 리소스에서는 다음 작업을 수행해야 합니다.
      <ul>
        <li>서비스 경로를 앱이 청취하는 실제 경로로 정의</li>
        <li>서비스 포트를 Istio Ingress 제어기에 의해 노출된 앱 서비스의 HTTP/HTTPS 포트로 정의</li>
    </ul>
  </dd>

   <dt>클러스터 ALB에 대한 Ingress 리소스 YAML 예</dt>
   <dd>

   <pre class="codeblock">
   <code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
annotations:
      ingress.bluemix.net/istio-services: "enabled=true serviceName=&lt;myservice1&gt; istioServiceNamespace=&lt;istio-namespace&gt; istioServiceName=&lt;istio-ingress-service&gt;"
spec:
tls:
    - hosts:
      - mydomain
    secretName: mytlssecret
  rules:
    - host: mydomain
    http:
      paths:
        - path: &lt;/myapp1&gt;
          backend:
            serviceName: &lt;myservice1&gt;
            servicePort: &lt;istio_ingress_port&gt;
        - path: &lt;/myapp2&gt;
          backend:
            serviceName: &lt;myservice2&gt;
            servicePort: &lt;istio_ingress_port&gt;</code></pre>

   <table>
   <caption>YAML 파일 컴포넌트 이해</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> YAML 파일 컴포넌트 이해</th>
    </thead>
    <tbody>
    <tr>
    <td><code>enabled</code></td>
      <td>Istio 관리 서비스에 대한 트래픽 라우팅을 사용하려면 <code>True</code>로 설정하십시오.</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td><code><em>&lt;myservice1&gt;</em></code>을 Istio 관리 앱을 위해 작성한 Kubernetes 서비스의 이름으로 대체하십시오. 여러 서비스는 세미콜론(;)으로 구분하십시오. 이 필드는 선택사항입니다. 서비스 이름을 지정하지 않으면 모든 Istio 관리 서비스에서 트래픽 라우팅을 사용할 수 있게 됩니다.</td>
    </tr>
    <tr>
    <td><code>istioServiceNamespace</code></td>
    <td><code><em>&lt;istio-namespace&gt;</em></code>를 Istio가 설치된 Kubernetes 네임스페이스로 대체하십시오. 이 필드는 선택사항입니다. 네임스페이스를 지정하지 않으면 <code>istio-system</code> 네임스페이스가 사용됩니다.</td>
    </tr>
    <tr>
    <td><code>istioServiceName</code></td>
    <td><code><em>&lt;istio-ingress-service&gt;</em></code>를 Istio Ingress 서비스의 이름으로 대체하십시오. 이 필드는 선택사항입니다. Istio Ingress 서비스 이름을 지정하지 않으면 서비스 이름 <code>istio-ingress</code>가 사용됩니다.</td>
    </tr>
    <tr>
    <td><code>path</code></td>
      <td>트래픽을 라우팅할 각 Istio 관리 서비스에 대해, <code><em>&lt;/myapp1&gt;</em></code>을 Istio 관리 서비스가 청취하는 백엔드 경로로 대체하십시오. 이 경로는 Istio Ingress 리소스에 정의한 경로와 대응해야 합니다.</td>
    </tr>
    <tr>
    <td><code>servicePort</code></td>
    <td>트래픽을 라우팅할 각 Istio 관리 서비스에 대해, <code><em>&lt;istio_ingress_port&gt;</em></code>를 Istio Ingress 제어기의 포트로 대체하십시오.</td>
    </tr>
    </tbody></table>
    </dd>
    </dl>

## 프록시 버퍼 어노테이션
{: #proxy-buffer}


### 클라이언트 응답 데이터 버퍼링(proxy-buffering)
{: #proxy-buffering}

데이터를 클라이언트로 전송하는 동안 ALB에서 응답 데이터의 스토리지를 사용하지 않도록 설정하려면 버퍼 어노테이션을 사용하십시오.
{:shortdesc}

<dl>
<dt>설명</dt>
<dd>Ingress ALB는 백엔드 앱과 클라이언트 웹 브라우저 사이의 프록시 역할을 합니다. 백엔드 앱에서 클라이언트로 응답을 전송하면 기본적으로 응답 데이터가 ALB에서 버퍼링됩니다. ALB는 클라이언트 응답을 프록시하고 클라이언트의 속도에 맞춰 클라이언트로 응답을 전송하기 시작합니다. ALB가 백엔드 앱으로부터 모든 데이터를 수신하면 백엔드 앱에 대한 연결이 종료됩니다. 클라이언트가 모든 데이터를 수신할 때까지 ALB에서 클라이언트로의 연결은 열린 상태로 남아 있습니다.

</br></br>
ALB에서 응답 데이터 버퍼링을 사용하지 않으면 ALB에서 클라이언트로 데이터가 즉시 전송됩니다. 클라이언트는 ALB의 속도에 따라 수신되는 데이터를 처리할 수 있어야 합니다. 클라이언트가 너무 느리면 데이터가 유실될 수 있습니다.
</br></br>
ALB에서 응답 데이터 버퍼링은 기본적으로 사용으로 설정됩니다.</dd>
<dt>샘플 Ingress 리소스 YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
annotations:
   ingress.bluemix.net/proxy-buffering: "enabled=&lt;false&gt; serviceName=&lt;myservice1&gt;"
spec:
tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>어노테이션 컴포넌트 이해</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
 </thead>
 <tbody>
 <tr>
 <td><code>enabled</code></td>
   <td>ALB에서 응답 데이터 버퍼링을 사용하지 않도록 설정하려면 <code>false</code>로 설정하십시오.</td>
 </tr>
 <tr>
 <td><code>serviceName</code></td>
 <td><code><em>&lt;myservice1&gt;</em></code>을 앱을 대해 작성한 Kubernetes 서비스의 이름으로 대체하십시오. 여러 서비스는 세미콜론(;)으로 구분하십시오. 이 필드는 선택사항입니다. 서비스 이름을 지정하지 않으면 모든 서비스에서 이 어노테이션을 사용합니다.</td>
 </tr>
 </tbody></table>
 </dd>
 </dl>

<br />



### 프록시 버퍼(proxy-buffers)
{: #proxy-buffers}

ALB를 위한 프록시 버퍼 수와 크기를 구성하십시오.
{:shortdesc}

<dl>
<dt>설명</dt>
<dd>
프록시된 서버에서 단일 연결에 대한 응답을 읽는 데 사용되는 버퍼의 수와 크기를 설정합니다. 서비스가 지정되지 않으면 구성은 Ingress 호스트의 모든 서비스에 적용됩니다. 예를 들어, <code>serviceName=SERVICE number=2 size=1k</code>와 같은 구성이 지정되면 1k가 서비스에 적용됩니다. <code>number=2 size=1k</code>와 같은 구성이 지정되면 1k가 Ingress 호스트의 모든 서비스에 적용됩니다.
</dd>
<dt>샘플 Ingress 리소스 YAML</dt>
<dd>
<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: proxy-ingress
annotations:
   ingress.bluemix.net/proxy-buffers: "serviceName=&lt;myservice&gt; number=&lt;number_of_buffers&gt; size=&lt;size&gt;"
spec:
tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>어노테이션 컴포넌트 이해</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
 </thead>
 <tbody>
 <tr>
 <td><code>serviceName</code></td>
 <td><code>&lt;<em>myservice</em>&gt;</code>를 프록시-버퍼를 적용할 서비스 이름으로 대체합니다.</td>
 </tr>
 <tr>
 <td><code>number</code></td>
 <td><code>&lt;<em>number_of_buffers</em>&gt;</code>를 숫자(예: <em>2</em>)로 대체합니다.</td>
 </tr>
 <tr>
 <td><code>size</code></td>
 <td><code>&lt;<em>size</em>&gt;</code>를 각 버퍼의 크기(킬로바이트, k 또는 K)로 대체합니다(예: <em>1K</em>).</td>
 </tr>
 </tbody>
 </table>
 </dd></dl>

<br />


### 프록시 버퍼 크기(proxy-buffer-size)
{: #proxy-buffer-size}

응답의 첫 번째 파트를 읽는 프록시 버퍼의 크기를 구성합니다.
{:shortdesc}

<dl>
<dt>설명</dt>
<dd>
프록시된 서버에서 수신되는 응답의 첫 번째 파트를 읽는 데 사용되는 버퍼의 크기를 설정합니다. 응답의 이 파트는 대개 작은 응답 헤더를 포함합니다. 서비스가 지정되지 않으면 구성은 Ingress 호스트의 모든 서비스에 적용됩니다. 예를 들어, <code>serviceName=SERVICE size=1k</code>와 같은 구성이 지정되면 1k가 서비스에 적용됩니다. <code>size=1k</code>와 같은 구성이 지정되면 1k가 Ingress 호스트의 모든 서비스에 적용됩니다.
</dd>


<dt>샘플 Ingress 리소스 YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: proxy-ingress
annotations:
   ingress.bluemix.net/proxy-buffer-size: "serviceName=&lt;myservice&gt; size=&lt;size&gt;"
spec:
tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
 </code></pre>

<table>
<caption>어노테이션 컴포넌트 이해</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
 </thead>
 <tbody>
 <tr>
 <td><code>serviceName</code></td>
 <td><code>&lt;<em>myservice</em>&gt;</code>를 프록시-버퍼-크기를 적용할 서비스 이름으로 대체합니다.</td>
 </tr>
 <tr>
 <td><code>size</code></td>
 <td><code>&lt;<em>size</em>&gt;</code>를 각 버퍼의 크기(킬로바이트, k 또는 K)로 대체합니다(예: <em>1K</em>).</td>
 </tr>
 </tbody></table>

 </dd>
 </dl>

<br />



### 프록시의 사용 중인 버퍼 크기(proxy-busy-buffers-size)
{: #proxy-busy-buffers-size}

사용 중일 수 있는 프록시 버퍼 크기를 구성합니다.
{:shortdesc}

<dl>
<dt>설명</dt>
<dd>
응답을 완전히 읽기 전에 클라이언트에 응답을 보내는 모든 버퍼의 크기를 제한합니다. 한편 나머지 버퍼는 응답을 읽을 수 있으며 필요한 경우 응답의 일부를 임시 파일로 버퍼링할 수 있습니다. 서비스가 지정되지 않으면 구성은 Ingress 호스트의 모든 서비스에 적용됩니다. 예를 들어, <code>serviceName=SERVICE size=1k</code>와 같은 구성이 지정되면 1k가 서비스에 적용됩니다. <code>size=1k</code>와 같은 구성이 지정되면 1k가 Ingress 호스트의 모든 서비스에 적용됩니다.
</dd>


<dt>샘플 Ingress 리소스 YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: proxy-ingress
annotations:
   ingress.bluemix.net/proxy-busy-buffers-size: "serviceName=&lt;myservice&gt; size=&lt;size&gt;"
spec:
tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>어노테이션 컴포넌트 이해</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code>를 프록시-사용-버퍼-크기를 적용할 서비스 이름으로 대체합니다.</td>
</tr>
<tr>
<td><code>size</code></td>
<td><code>&lt;<em>size</em>&gt;</code>를 각 버퍼의 크기(킬로바이트, k 또는 K)로 대체합니다(예: <em>1K</em>).</td>
</tr>
</tbody></table>

 </dd>
 </dl>

<br />



## 요청 및 응답 어노테이션
{: #request-response}


### 추가 클라이언트 요청 또는 응답 헤더(proxy-add-headers, response-add-headers)
{: #proxy-add-headers}

백엔드 앱에 요청을 전송하기 전에 클라이언트 요청에 헤더 정보를 추가하거나 클라이언트에 응답을 전송하기 전에 클라이언트 응답에 헤더 정보를 추가합니다.
{:shortdesc}

<dl>
<dt>설명</dt>
<dd>Ingress ALB는 클라이언트 앱과 백엔드 앱 사이의 프록시 역할을 합니다. ALB로 전송된 클라이언트 요청은 처리(프록시)되어 이후에 백엔드 앱으로 전송되는 새 요청에 넣어집니다. 이와 유사하게, ALB로 전송된 백엔드 요청은 처리(프록시)되어 이후에 클라이언트로 전송되는 새 요청에 넣어집니다. 요청 또는 응답을 프록시하면 클라이언트 또는 백엔드 앱에서 처음에 전송된 HTTP 헤더 정보(예: 사용자 이름)가 제거됩니다.

<br><br>
백엔드 앱에서 HTTP 헤더 정보가 필요하면 ALB가 백엔드 앱으로 요청을 전달하기 전에 <code>proxy-add-headers</code> 어노테이션을 사용하여 클라이언트 요청에 헤더 정보를 추가할 수 있습니다.

<br>
<ul><li>예를 들어, 앱에 요청을 전송하기 전에 다음 X-Forward 헤더 정보를 요청에 추가해야 할 수 있습니다.

<pre class="screen">
<code>proxy_set_header Host $host;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;</code></pre>

</li>

<li>X-Forward 헤더 정보를 앱에 전송된 요청에 추가하려면 다음과 같은 방법으로 `proxy-add-headers` 어노테이션을 사용하십시오.

<pre class="screen">
<code>ingress.bluemix.net/proxy-add-headers: |
serviceName=<myservice1> {
  Host $host;
  X-Forwarded-Proto $scheme;
  X-Forwarded-For $proxy_add_x_forwarded_for;
  }</code></pre>

</li></ul><br>

클라이언트 웹 앱에서 HTTP 헤더 정보가 필요하면 ALB가 클라이언트 웹 앱으로 응답을 전달하기 전에 <code>response-add-headers</code> 어노테이션을 사용하여 응답에 헤더 정보를 추가할 수 있습니다.</dd>

<dt>샘플 Ingress 리소스 YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
annotations:
    ingress.bluemix.net/proxy-add-headers: |
      serviceName=&lt;myservice1&gt; {
      &lt;header1&gt; &lt;value1&gt;;
      &lt;header2&gt; &lt;value2&gt;;
      }
      serviceName=&lt;myservice2&gt; {
      &lt;header3&gt; &lt;value3&gt;;
      }
    ingress.bluemix.net/response-add-headers: |
      serviceName=&lt;myservice1&gt; {
      &lt;header1&gt;: &lt;value1&gt;;
      &lt;header2&gt;: &lt;value2&gt;;
      }
      serviceName=&lt;myservice2&gt; {
      &lt;header3&gt;: &lt;value3&gt;;
      }
spec:
tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: &lt;myservice1&gt;
          servicePort: 8080
      - path: /service2_path
        backend:
          serviceName: &lt;myservice2&gt;
          servicePort: 80</code></pre>

 <table>
 <caption>어노테이션 컴포넌트 이해</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
  </thead>
  <tbody>
  <tr>
  <td><code>service_name</code></td>
  <td><code>&lt;<em>myservice</em>&gt;</code>를 앱에 대해 작성된 Kubernetes 서비스의 이름으로 대체합니다.</td>
  </tr>
  <tr>
  <td><code>&lt;header&gt;</code></td>
  <td>클라이언트 요청 또는 클라이언트 응답에 추가할 헤더 정보의 키입니다.</td>
  </tr>
  <tr>
  <td><code>&lt;value&gt;</code></td>
  <td>클라이언트 요청 또는 클라이언트 응답에 추가할 헤더 정보의 값입니다.</td>
  </tr>
  </tbody></table>
 </dd></dl>

<br />



### 클라이언트 응답 헤더 제거(response-remove-headers)
{: #response-remove-headers}

응답을 클라이언트에 전송하기 전에 백엔드 앱에서 클라이언트 응답에 포함되는 헤더 정보를 제거합니다.
 {:shortdesc}

 <dl>
 <dt>설명</dt>
 <dd>Ingress ALB는 백엔드 앱과 클라이언트 웹 브라우저 사이의 프록시 역할을 합니다. ALB로 전송된 백엔드 앱의 클라이언트 응답은 처리(프록시)되어 새 응답에 넣어진 후 새 응답이 ALB에서 클라이언트 웹 브라우저로 전송됩니다. 응답을 프록시 처리하면 백엔드 앱에서 처음에 전송된 http 헤더 정보가 제거되지만 이 프로세스에서는 모든 백엔드 앱 고유 헤더를 제거하지 않을 수 있습니다. ALB에서 클라이언트 웹 브라우저로 응답을 전달하기 전에 클라이언트 응답에서 헤더 정보를 제거하십시오.</dd>
 <dt>샘플 Ingress 리소스 YAML</dt>
 <dd>
 <pre class="codeblock">
 <code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
annotations:
    ingress.bluemix.net/response-remove-headers: |
       serviceName=&lt;myservice1&gt; {
      "&lt;header1&gt;";
      "&lt;header2&gt;";
       }
       serviceName=&lt;myservice2&gt; {
      "&lt;header3&gt;";
       }
spec:
tls:
   - hosts:
     - mydomain
    secretName: mytlssecret
  rules:
   - host: mydomain
    http:
      paths:
       - path: /service1_path
         backend:
           serviceName: &lt;myservice1&gt;
           servicePort: 8080
       - path: /service2_path
         backend:
           serviceName: &lt;myservice2&gt;
           servicePort: 80</code></pre>

  <table>
  <caption>어노테이션 컴포넌트 이해</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
   </thead>
   <tbody>
   <tr>
   <td><code>service_name</code></td>
   <td><code>&lt;<em>myservice</em>&gt;</code>를 앱에 대해 작성된 Kubernetes 서비스의 이름으로 대체합니다.</td>
   </tr>
   <tr>
   <td><code>&lt;header&gt;</code></td>
   <td>클라이언트 응답에서 제거할 헤더의 키입니다.</td>
   </tr>
   </tbody></table>
   </dd></dl>

<br />


### 클라이언트 요청 본문 크기(client-max-body-size)
{: #client-max-body-size}

클라이언트가 요청의 일부로 전송할 수 있는 최대 본문 크기를 설정합니다.
{:shortdesc}

<dl>
<dt>설명</dt>
<dd>예상 성능을 유지보수하려면 최대 클라이언트 요청 본문 크기를 1MB로 설정하십시오. 본문 크기가 한계를 초과하는 클라이언트 요청을 Ingress ALB로 전송한 경우, 클라이언트가 데이터를 분할하지 못하게 하면 ALB는 413(요청 엔티티가 너무 큼) HTTP 응답을 클라이언트로 리턴합니다. 요청 본문 크기를 줄일 때까지 클라이언트와 ALB 사이에 연결이 불가능합니다. 클라이언트에서 데이터를 여러 청크로 분할하도록 허용하는 경우, 데이터는 1MB의 패키지로 나뉘어 ALB로 전송됩니다.

</br></br>
본문 크기가 1MB보다 큰 클라이언트 요청을 예상하므로 최대 본문 크기를 늘릴 수 있습니다. 예를 들어, 클라이언트에서 큰 파일을 업로드할 수 있게 합니다. 요청을 받을 때까지 클라이언트에 대한 연결이 열린 상태를 유지해야 하므로 최대 요청 본문 크기를 늘리면 ALB의 성능에 영향을 미칠 수 있습니다.
</br></br>
<strong>참고:</strong> 일부 클라이언트 웹 브라우저에서는 413 HTTP 응답 메시지를 제대로 표시할 수 없습니다.</dd>
<dt>샘플 Ingress 리소스 YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
annotations:
   ingress.bluemix.net/client-max-body-size: "size=&lt;size&gt;"
spec:
tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>어노테이션 컴포넌트 이해</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;size&gt;</code></td>
 <td>클라이언트 응답 본문의 최대 크기입니다. 예를 들어, 최대 크기를 200MB로 설정하려면 <code>200m</code>을 정의하십시오. <strong>참고:</strong> 클라이언트 요청 본문 크기 검사를 사용하지 않게 크기를 0으로 설정할 수 있습니다.</td>
 </tr>
 </tbody></table>

 </dd></dl>

<br />


### 대형 클라이언트 헤더 버퍼(large-client-header-buffers)
{: #large-client-header-buffers}

대형 클라이언트 요청 헤더를 읽는 최대 버퍼의 수 및 크기를 설정합니다.
{:shortdesc}

<dl>
<dt>설명</dt>
<dd>대형 클라이언트 요청 헤더를 읽는 버퍼는 요청 시에만 할당됩니다. 요청 처리가 끝난 후 연결이 keepalive 상태로 전이되는 경우 이 버퍼가 해제됩니다. 기본적으로 버퍼 크기는 <code>8K</code>바이트와 같습니다. 요청 행이 한 개 버퍼의 설정 최대 크기를 초과하는 경우 <code>414 Request-URI Too Large</code> 오류가 클라이언트에 리턴됩니다. 또한 요청 헤더 필드가 한 개 버퍼의 설정 최대 크기를 초과하는 경우 <code>400 Bad Request Large</code> 오류가 클라이언트에 리턴됩니다. 대형 클라이언트 요청 헤더를 읽는 데 사용된 버퍼의 최대 수 및 크기를 조정할 수 있습니다.

<dt>샘플 Ingress 리소스 YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
annotations:
   ingress.bluemix.net/large-client-header-buffers: "number=&lt;number&gt; size=&lt;size&gt;"
spec:
tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>어노테이션 컴포넌트 이해</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;number&gt;</code></td>
 <td>대형 클라이언트 요청 헤더를 읽는 데 할당되어야 하는 최대 버퍼의 수입니다. 예를 들어, 4로 설정하려면 <code>4</code>를 정의하십시오.</td>
 </tr>
 <tr>
 <td><code>&lt;size&gt;</code></td>
 <td>대형 클라이언트 요청 헤더를 읽는 최대 버퍼의 크기입니다. 예를 들어, 16KB로 설정하려면 <code>16k</code>를 정의하십시오.
   <strong>참고:</strong> 크기는 KB의 경우 <code>k</code>로 끝나고 MB의 경우 <code>m</code>으로 끝나야 합니다.</td>
 </tr>
</tbody></table>
</dd>
</dl>

<br />


## 서비스 제한 어노테이션
{: #service-limit}


### 글로벌 속도 제한(global-rate-limit)
{: #global-rate-limit}

모든 서비스의 정의된 키별 연결 수와 요청 처리 속도를 제한합니다.
{:shortdesc}

<dl>
<dt>설명</dt>
<dd>
모든 서비스에서, 선택된 백엔드의 모든 경로에 대한 단일 IP 주소에서 수신되는 정의된 키마다 연결 수와 요청 처리 속도를 제한합니다.
</dd>


 <dt>샘플 Ingress 리소스 YAML</dt>
 <dd>

 <pre class="codeblock">
 <code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
annotations:
    ingress.bluemix.net/global-rate-limit: "key=&lt;key&gt; rate=&lt;rate&gt; conn=&lt;number_of_connections&gt;"
spec:
tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

 <table>
 <caption>어노테이션 컴포넌트 이해</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
  </thead>
  <tbody>
  <tr>
  <td><code>key</code></td>
  <td>위치 또는 서비스를 기반으로 수신 요청의 글로벌 한계를 설정하려면 `key=location`을 사용하십시오. 헤더를 기반으로 수신 요청의 글로벌 한계를 설정하려면 `X-USER-ID key=$http_x_user_id`를 사용하십시오.</td>
  </tr>
  <tr>
  <td><code>rate</code></td>
  <td><code>&lt;<em>rate</em>&gt;</code>를 처리 속도로 대체합니다. 값을 초당 비율(r/s) 또는 분당 비율(r/m)로 입력하십시오. 예: <code>50r/m</code>.</td>
  </tr>
  <tr>
  <td><code>number-of_connections</code></td>
  <td><code>&lt;<em>conn</em>&gt;</code>을 연결 수로 대체합니다.</td>
  </tr>
  </tbody></table>

  </dd>
  </dl>

<br />



### 서비스 속도 한계(service-rate-limit)
{: #service-rate-limit}

특정 서비스의 연결 수와 요청 처리 속도를 제한합니다.
{:shortdesc}

<dl>
<dt>설명</dt>
<dd>특정 서비스의 경우 선택된 백엔드의 모든 경로에 대한 단일 IP 주소에서 수신되는 정의된 키마다 연결 수와 요청 처리 속도를 제한합니다.
</dd>


 <dt>샘플 Ingress 리소스 YAML</dt>
 <dd>

 <pre class="codeblock">
 <code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
annotations:
    ingress.bluemix.net/service-rate-limit: "serviceName=&lt;myservice&gt; key=&lt;key&gt; rate=&lt;rate&gt; conn=&lt;number_of_connections&gt;"
spec:
tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

 <table>
 <caption>어노테이션 컴포넌트 이해</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 어노테이션 컴포넌트 이해</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td><code>&lt;<em>myservice</em>&gt;</code>를 처리 속도를 제한하려는 서비스 이름으로 대체합니다.</li>
  </tr>
  <tr>
  <td><code>key</code></td>
  <td>위치 또는 서비스를 기반으로 수신 요청의 글로벌 한계를 설정하려면 `key=location`을 사용하십시오. 헤더를 기반으로 수신 요청의 글로벌 한계를 설정하려면 `X-USER-ID key=$http_x_user_id`를 사용하십시오.</td>
  </tr>
  <tr>
  <td><code>rate</code></td>
  <td><code>&lt;<em>rate</em>&gt;</code>를 처리 속도로 대체합니다. 초당 비율을 정의하려면 r/s를 사용합니다(<code>10r/s</code>). 분당 비율을 정의할 때는 r/m을 사용합니다(<code>50r/m</code>).</td>
  </tr>
  <tr>
  <td><code>number-of_connections</code></td>
  <td><code>&lt;<em>conn</em>&gt;</code>을 연결 수로 대체합니다.</td>
  </tr>
  </tbody></table>
  </dd>
  </dl>

  <br />



