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



# Ingress 注释
{: #ingress_annotation}

要向 Ingress 应用程序负载均衡器 (ALB) 添加功能，您可以将注释指定为 Ingress 资源中的元数据。
{: shortdesc}

有关 Ingress 服务以及有关如何开始使用这些服务的常规信息，请参阅[使用 Ingress 管理网络流量](cs_ingress.html#planning)。

<table>
<caption>一般注释</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>一般注释</th>
 <th>名称</th>
 <th>描述</th>
 </thead>
 <tbody>
 <tr>
 <td><a href="#proxy-external-service">外部服务</a></td>
 <td><code>proxy-external-service</code></td>
 <td>添加外部服务（例如，{{site.data.keyword.Bluemix_notm}} 中托管的服务）的路径定义。
</td>
 </tr>
 <tr>
 <td><a href="#location-modifier">位置修饰符</a></td>
 <td><code>location-modifier</code></td>
 <td>修改 ALB 将请求 URI 与应用程序路径相匹配的方式。</td>
 </tr>
 <tr>
 <td><a href="#alb-id">专用 ALB 路由</a></td>
 <td><code>ALB-ID</code></td>
 <td>使用专用 ALB 将入局请求路由到应用程序。</td>
 </tr>
 <tr>
 <td><a href="#rewrite-path">重写路径</a></td>
 <td><code>rewrite-path</code></td>
 <td>将入局网络流量路由到后端应用程序侦听的其他路径。</td>
 </tr>
 <tr>
 <td><a href="#tcp-ports">TCP 端口</a></td>
 <td><code>tcp-ports</code></td>
 <td>通过非标准 TCP 端口访问应用程序。</td>
 </tr>
 </tbody></table>

<br>

<table>
<caption>连接注释</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>连接注释</th>
 <th>名称</th>
 <th>描述</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#proxy-connect-timeout">定制连接超时和读取超时</a></td>
  <td><code>proxy-connect-timeout，proxy-read-timeout</code></td>
  <td>设置 ALB 等待连接到后端应用程序以及从后端应用程序进行读取的时间，超过该时间后，后端应用程序将视为不可用。</td>
  </tr>
  <tr>
  <td><a href="#keepalive-requests">保持活动请求数</a></td>
  <td><code>keepalive-requests</code></td>
  <td>设置可通过一个保持活动连接处理的最大请求数。</td>
  </tr>
  <tr>
  <td><a href="#keepalive-timeout">保持活动超时</a></td>
  <td><code>keepalive-timeout</code></td>
  <td>设置保持活动连接在服务器上保持打开状态的最长时间。</td>
  </tr>
  <tr>
  <td><a href="#proxy-next-upstream-config">作为代理传递到下一个上游</a></td>
  <td><code>proxy-next-upstream-config</code></td>
  <td>设置 ALB 何时可以向下一个上游服务器传递请求。</td>
  </tr>
  <tr>
  <td><a href="#sticky-cookie-services">使用 cookie 确保会话亲缘关系</a></td>
  <td><code>sticky-cookie-services</code></td>
  <td>使用粘性 cookie 始终将入局网络流量路由到同一个上游服务器。</td>
  </tr>
  <tr>
  <td><a href="#upstream-keepalive">上游保持活动连接数</a></td>
  <td><code>upstream-keepalive</code></td>
  <td>设置上游服务器的最大空闲保持活动连接数。</td>
  </tr>
  </tbody></table>

<br>

  <table>
  <caption>HTTPS 和 TLS/SSL 认证注释</caption>
  <col width="20%">
  <col width="20%">
  <col width="60%">
  <thead>
  <th>HTTPS 和 TLS/SSL 认证注释</th>
  <th>名称</th>
  <th>描述</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#appid-auth">{{site.data.keyword.appid_short}} 认证</a></td>
  <td><code>appid-auth</code></td>
  <td>使用 {{site.data.keyword.appid_full_notm}} 向应用程序进行认证。</td>
  </tr>
  <tr>
  <td><a href="#custom-port">定制 HTTP 和 HTTPS 端口</a></td>
  <td><code>custom-port</code></td>
  <td>更改 HTTP（端口 80）和 HTTPS（端口 443）网络流量的缺省端口。
</td>
  </tr>
  <tr>
  <td><a href="#redirect-to-https">HTTP 重定向到 HTTPS</a></td>
  <td><code>redirect-to-https</code></td>
  <td>将域上的不安全的 HTTP 请求重定向到 HTTPS。</td>
  </tr>
  <tr>
  <td><a href="#hsts">HTTP 严格传输安全性 (HSTS)</a></td>
  <td><code>hsts</code></td>
  <td>将浏览器设置为仅使用 HTTPS 访问域。</td>
  </tr>
  <tr>
  <td><a href="#mutual-auth">相互认证</a></td>
  <td><code>mutual-auth</code></td>
  <td>为 ALB 配置相互认证。</td>
  </tr>
  <tr>
  <td><a href="#ssl-services">SSL 服务支持</a></td>
  <td><code>ssl-services</code></td>
  <td>允许 SSL 服务支持加密流至需要 HTTPS 的上游应用程序的流量。</td>
  </tr>
  </tbody></table>

<br>

<table>
<caption>Istio 注释</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Istio 注释</th>
<th>名称</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td><a href="#istio-services">Istio 服务</a></td>
<td><code>istio-services</code></td>
<td>将流量路由到 Istio 管理的服务。</td>
</tr>
</tbody></table>

<br>

<table>
<caption>代理缓冲区注释</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>代理缓冲区注释</th>
 <th>名称</th>
 <th>描述</th>
 </thead>
 <tbody>
 <tr>
 <td><a href="#proxy-buffering">客户机响应数据缓冲</a></td>
 <td><code>proxy-buffering</code></td>
 <td>禁止在向客户机发送响应时，在 ALB 上对客户机响应进行缓冲。</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffers">代理缓冲区数</a></td>
 <td><code>proxy-buffers</code></td>
 <td>设置缓冲区的数目和大小，这些缓冲区用于读取来自通过代理传递的服务器的单个连接的响应。</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffer-size">代理缓冲区大小</a></td>
 <td><code>proxy-buffer-size</code></td>
 <td>设置缓冲区的大小，该缓冲区用于读取从通过代理传递的服务器收到的响应的第一部分。</td>
 </tr>
 <tr>
 <td><a href="#proxy-busy-buffers-size">代理繁忙缓冲区大小</a></td>
 <td><code>proxy-busy-buffers-size</code></td>
 <td>设置可以处于繁忙状态的代理缓冲区的大小。</td>
 </tr>
 </tbody></table>

<br>

<table>
<caption>请求和响应注释</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>请求和响应注释</th>
<th>名称</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td><a href="#proxy-add-headers">额外的客户机请求或响应头</a></td>
<td><code>proxy-add-headers，response-add-headers</code></td>
<td>向客户机请求添加头信息，然后将该请求转发到后端应用程序，或者向客户机响应添加头信息，然后将该响应发送到客户机。</td>
</tr>
<tr>
<td><a href="#response-remove-headers">除去客户机响应头</a></td>
<td><code>response-remove-headers</code></td>
<td>从客户机响应中除去头信息，然后将该响应转发到客户机。</td>
</tr>
<tr>
<td><a href="#client-max-body-size">客户机请求主体大小</a></td>
<td><code>client-max-body-size</code></td>
<td>设置客户机可以作为请求一部分发送的主体的最大大小。
</td>
</tr>
<tr>
<td><a href="#large-client-header-buffers">大型客户机头缓冲区</a></td>
<td><code>large-client-header-buffers</code></td>
<td>设置读取大型客户机请求头的缓冲区的最大数目和大小。</td>
</tr>
</tbody></table>

<br>

<table>
<caption>服务限制注释</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>服务限制注释</th>
<th>名称</th>
<th>描述</th>
</thead>
<tbody>
<tr>
<td><a href="#global-rate-limit">全局速率限制</a></td>
<td><code>global-rate-limit</code></td>
<td>对于所有服务，按定义的键限制请求处理速率和连接数。</td>
</tr>
<tr>
<td><a href="#service-rate-limit">服务速率限制</a></td>
<td><code>service-rate-limit</code></td>
<td>对于特定服务，按定义的键限制请求处理速率和连接数。</td>
</tr>
</tbody></table>

<br>



## 一般注释
{: #general}

### 外部服务 (proxy-external-service)
{: #proxy-external-service}

添加外部服务（例如，{{site.data.keyword.Bluemix_notm}} 中托管的服务）的路径定义。
{:shortdesc}

<dl>
<dt>描述</dt>
<dd>添加外部服务的路径定义。仅当应用程序在外部服务（而不是后端服务）上运行时，才可使用此注释。使用此注释来创建外部服务路径时，仅支持 `client-max-body-size`、`proxy-read-timeout`、`proxy-connect-timeout` 和 `proxy-buffering` 注释一起使用。不支持其他任何注释与 `proxy-external-service` 一起使用。
</dd>
<dt>样本 Ingress 资源 YAML</dt>
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
<caption>了解注释的组成部分</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
 </thead>
 <tbody>
 <tr>
 <td><code>path</code></td>
 <td>将 <code>&lt;<em>mypath</em>&gt;</code> 替换为外部服务侦听的路径。</td>
 </tr>
 <tr>
 <td><code>external-svc</code></td>
 <td>将 <code>&lt;<em>external_service</em>&gt;</code> 替换为要调用的外部服务。例如，<code>https://&lt;myservice&gt;.&lt;region&gt;.mybluemix.net</code>。</td>
 </tr>
 <tr>
 <td><code>host</code></td>
 <td>将 <code>&lt;<em>mydomain</em>&gt;</code> 替换为外部服务的主机域。</td>
 </tr>
 </tbody></table>

 </dd></dl>

<br />


### 位置修饰符 (location-modifier)
{: #location-modifier}

修改 ALB 将请求 URI 与应用程序路径相匹配的方式。
{:shortdesc}

<dl>
<dt>描述</dt>
<dd>缺省情况下，ALB 会将应用程序侦听的路径作为前缀进行处理。ALB 接收到对应用程序的请求时，ALB 会检查 Ingress 资源以查找与请求 URI 的开头相匹配的路径（作为前缀）。如果找到匹配项，那么会将请求转发到部署了应用程序的 pod 的 IP 地址。<br><br>`location-modifier` 注释通过修改位置块配置来更改 ALB 搜索匹配项的方式。位置块用于确定如何处理请求中的应用程序路径。<br><br><strong>注</strong>：要处理正则表达式 (regex) 路径，此注释是必需的。</dd>

<dt>支持的修饰符</dt>
<dd>

<table>
<caption>支持的修饰符</caption>
 <col width="10%">
 <col width="90%">
 <thead>
 <th>修饰符</th>
 <th>描述</th>
 </thead>
 <tbody>
 <tr>
 <td><code>=</code></td>
 <td>等号修饰符使 ALB 仅选择完全匹配项。找到完全匹配项时，搜索将停止，并选择匹配路径。<br>例如，如果应用程序侦听的是 <code>/tea</code>，那么 ALB 在匹配对应用程序的请求时，仅选择完全匹配的 <code>/tea</code> 路径。</td>
 </tr>
 <tr>
 <td><code>~</code></td>
 <td>波浪号修饰符使 ALB 在匹配期间将路径作为区分大小写的 regex 路径进行处理。<br>例如，如果应用程序侦听的是 <code>/coffee</code>，那么 ALB 在匹配对应用程序的请求时，可以选择 <code>/ab/coffee</code> 或 <code>/123/coffee</code> 路径，即便并未为应用程序显式设置这些路径也不例外。</td>
 </tr>
 <tr>
 <td><code>~\*</code></td>
 <td>波浪号后跟星号修饰符使 ALB 在匹配期间将路径作为不区分大小写的 regex 路径进行处理。<br>例如，如果应用程序侦听的是 <code>/coffee</code>，那么 ALB 在匹配对应用程序的请求时，可以选择 <code>/ab/Coffee</code> 或 <code>/123/COFFEE</code> 路径，即便并未为应用程序显式设置这些路径也不例外。</td>
 </tr>
 <tr>
 <td><code>^~</code></td>
 <td>重音符后跟波浪号修饰符使 ALB 选择最佳的非 regex 匹配项，而不选择 regex 路径。</td>
 </tr>
 </tbody>
</table>

</dd>

<dt>样本 Ingress 资源 YAML</dt>
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
 <caption>了解注释的组成部分</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
  </thead>
  <tbody>
  <tr>
  <td><code>modifier</code></td>
  <td>将 <code>&lt;<em>location_modifier</em>&gt;</code> 替换为要用于路径的位置修饰符。支持的修饰符为 <code>'='</code>、<code>'~'</code>、<code>'~\*'</code> 和 <code>'^~'</code>。必须用单引号将修饰符括起。</td>
  </tr>
  <tr>
  <td><code>serviceName</code></td>
  <td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为针对应用程序创建的 Kubernetes 服务的名称。</td>
  </tr>
  </tbody></table>
  </dd>
  </dl>

<br />


### 专用 ALB 路由 (ALB-ID)
{: #alb-id}

使用专用 ALB 将入局请求路由到应用程序。
{:shortdesc}

<dl>
<dt>描述</dt>
<dd>
选择专用 ALB 来路由入局请求，而不选择公共 ALB。</dd>


<dt>样本 Ingress 资源 YAML</dt>
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
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>&lt;private_ALB_ID&gt;</code></td>
<td>专用 ALB 的标识。要查找专用 ALB 标识，请运行 <code>bx cs albs --cluster &lt;my_cluster&gt;</code>。
</td>
</tr>
</tbody></table>
</dd>
</dl>

<br />


### 重写路径 (rewrite-path)
{: #rewrite-path}

将 ALB 域路径上的入局网络流量路由到后端应用程序侦听的其他路径。
{:shortdesc}

<dl>
<dt>描述</dt>
<dd>Ingress ALB 域将 <code>mykubecluster.us-south.containers.appdomain.cloud/beans</code> 上的入局网络流量路由到应用程序。应用程序侦听的是 <code>/coffee</code>，而不是 <code>/beans</code>。要将入局网络流量转发到应用程序，请将 rewrite 注释添加到 Ingress 资源配置文件。rewrite 注释可确保使用 <code>/coffee</code> 路径将 <code>/beans</code> 上的入局网络流量转发到应用程序。当包含多个服务时，请仅使用分号 (;) 来分隔这些服务。</dd>
<dt>样本 Ingress 资源 YAML</dt>
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
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为您针对应用程序创建的 Kubernetes 服务的名称。</td>
</tr>
<tr>
<td><code>rewrite</code></td>
<td>将 <code>&lt;<em>target_path</em>&gt;</code> 替换为应用程序侦听的路径。ALB 域上的入局网络流量会使用此路径转发到 Kubernetes 服务。大多数应用程序不会侦听特定路径，而是使用根路径和特定端口。在上例中，rewrite 路径定义为 <code>/coffee</code>。</td>
</tr>
</tbody></table>

</dd></dl>

<br />


### 用于应用程序负载均衡器的 TCP 端口 (tcp-ports)
{: #tcp-ports}

通过非标准 TCP 端口访问应用程序。
{:shortdesc}

<dl>
<dt>描述</dt>
<dd>
将此注释用于在运行 TCP 流工作负载的应用程序。



<p>**注**：ALB 以通过方式运行，并将流量转发到后端应用程序。在此情况下，不支持 SSL 终止。</p>
</dd>


<dt>样本 Ingress 资源 YAML</dt>
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
 <caption>了解注释的组成部分</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为要通过非标准 TCP 端口访问的 Kubernetes 服务的名称。</td>
  </tr>
  <tr>
  <td><code>ingressPort</code></td>
  <td>将 <code><em>&lt;ingress_port&gt;</em></code> 替换为要用于访问应用程序的 TCP 端口。</td>
  </tr>
  <tr>
  <td><code>servicePort</code></td>
  <td>此参数是可选的。如果提供了此参数，那么在将流量发送到后端应用程序之前，会将端口替换为此值。否则，该端口与 Ingress 端口保持相同。</td>
  </tr>
  </tbody></table>

 </dd>
 <dt>用法</dt>
 <dd><ol><li>查看 ALB 的打开端口。
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
CLI 输出类似于以下内容：
<pre class="screen">
<code>NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx 80:30416/TCP,443:32668/TCP   109d</code></pre></li>
<li>打开 ALB 配置映射。
<pre class="pre">
<code>kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system</code></pre></li>
<li>将 TCP 端口添加到配置映射。将 <code>&lt;port&gt;</code> 替换为要打开的 TCP 端口。
<b>注</b>：缺省情况下，端口 80 和 443 已打开。如果要使 80 和 443 保持打开，那么必须在 `public-ports` 字段中包含这两个端口以及您指定的其他任何 TCP 端口。如果启用了专用 ALB，那么还必须在 `private-ports` 字段中指定要保持打开的任何端口。有关更多信息，请参阅<a href="cs_ingress.html#opening_ingress_ports">在 Ingress ALB 中打开端口</a>。
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
 <li>验证是否将 ALB 重新配置为使用 TCP 端口。
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
CLI 输出类似于以下内容：
<pre class="screen">
<code>NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx  169.xx.xxx.xxx &lt;port1&gt;:30776/TCP,&lt;port2&gt;:30412/TCP   109d</code></pre></li>
<li>将 Ingress 配置为通过非标准 TCP 端口访问应用程序。在此引用中使用样本 YAML 文件。</li>
<li>更新 ALB 配置。
<pre class="pre">
<code>        kubectl apply -f myingress.yaml
        </code></pre>
</li>
<li>打开首选 Web 浏览器以访问您的应用程序。示例：<code>https://&lt;ibmdomain&gt;:&lt;ingressPort&gt;/</code></li></ol></dd></dl>

<br />


## 连接注释
{: #connection}

### 定制连接超时和读取超时（proxy-connect-timeout 和 proxy-read-timeout）
{: #proxy-connect-timeout}

设置 ALB 等待连接到后端应用程序以及从后端应用程序进行读取的时间，超过该时间后，后端应用程序将视为不可用。
{:shortdesc}

<dl>
<dt>描述</dt>
<dd>将客户机请求发送到 Ingress ALB 时，ALB 会打开与后端应用程序的连接。缺省情况下，ALB 会等待 60 秒以接收来自后端应用程序的应答。如果后端应用程序在 60 秒内未应答，那么连接请求将异常中止，并且后端应用程序将视为不可用。



</br></br>
ALB 连接到后端应用程序后，ALB 会从后端应用程序读取响应数据。在此读操作期间，ALB 在两次读操作之间等待最长 60 秒以接收来自后端应用程序的数据。如果后端应用程序在 60 秒内未发送数据，那么与后端应用程序的连接请求将关闭，并且后端应用程序将视为不可用。
</br></br>
60 秒连接超时和读取超时是代理上的缺省超时，通常不应进行更改。
</br></br>
如果由于工作负载过高而导致应用程序的可用性不稳定或应用程序响应速度缓慢，那么您可能会希望增大连接超时或读取超时。请记住，增大超时会影响 ALB 的性能，因为与后端应用程序的连接必须保持打开状态，直至达到超时为止。
</br></br>
另一方面，您可以减小超时以提高 ALB 的性能。确保后端应用程序能够在指定的超时内处理请求，即使在更高的工作负载期间。</dd>
<dt>样本 Ingress 资源 YAML</dt>
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
<caption>了解注释的组成部分</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;connect_timeout&gt;</code></td>
 <td>等待连接到后端应用程序的秒数或分钟数，例如 <code>65s</code> 或 <code>2m</code>。<strong>注</strong>：连接超时不能超过 75 秒。</td>
 </tr>
 <tr>
 <td><code>&lt;read_timeout&gt;</code></td>
 <td>读取后端应用程序之前等待的秒数或分钟数，例如 <code>65s</code> 或 <code>2m</code>。
 </tr>
 </tbody></table>

 </dd></dl>

<br />



### 保持活动请求数 (keepalive-requests)
{: #keepalive-requests}

设置可通过一个保持活动连接处理的最大请求数。
{:shortdesc}

<dl>
<dt>描述</dt>
<dd>
 设置可通过一个保持活动连接处理的最大请求数。
 </dd>


<dt>样本 Ingress 资源 YAML</dt>
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
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为您针对应用程序创建的 Kubernetes 服务的名称。此参数是可选的。除非指定了服务，否则配置将应用于 Ingress 主机中的所有服务。如果提供了此参数，那么将为给定的服务设置保持活动请求数。如果未提供此参数，那么将在 <code>nginx.conf</code> 的服务器级别为所有未配置保持活动请求数的服务设置保持活动请求数。</td>
</tr>
<tr>
<td><code>requests</code></td>
<td>将 <code>&lt;<em>max_requests</em>&gt;</code> 替换为可通过一个保持活动连接处理的最大请求数。</td>
</tr>
</tbody></table>

</dd>
</dl>

<br />



### 保持活动超时 (keepalive-timeout)
{: #keepalive-timeout}

设置保持活动连接在服务器端保持打开状态的最长时间。
{:shortdesc}

<dl>
<dt>描述</dt>
<dd>
设置保持活动连接在服务器上保持打开状态的最长时间。
</dd>


<dt>样本 Ingress 资源 YAML</dt>
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
<caption>了解注释的组成部分</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
 </thead>
 <tbody>
 <tr>
 <td><code>serviceName</code></td>
 <td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为您针对应用程序创建的 Kubernetes 服务的名称。此参数是可选的。如果提供了此参数，那么将为给定的服务设置保持活动超时。如果未提供此参数，那么将在 <code>nginx.conf</code> 的服务器级别为所有未配置保持活动超时的服务设置保持活动超时。</td>
 </tr>
 <tr>
 <td><code>timeout</code></td>
 <td>将 <code>&lt;<em>time</em>&gt;</code> 替换为时间长度（以秒为单位）。示例：<code>timeout=20s</code>。值为零将禁用保持活动客户机连接。</td>
 </tr>
 </tbody></table>

 </dd>
 </dl>

<br />


### 作为代理传递到下一个上游 (proxy-next-upstream-config)
{: #proxy-next-upstream-config}

设置 ALB 何时可以向下一个上游服务器传递请求。
{:shortdesc}

<dl>
<dt>描述</dt>
<dd>
Ingress ALB 充当客户机应用程序和您的应用程序之间的代理。某些应用程序设置需要多个上游服务器来处理来自 ALB 的入局客户机请求。有时，ALB 使用的代理服务器无法与应用程序使用的上游服务器建立连接。于是 ALB 可以尝试与下一个上游服务器建立连接，以改为将请求传递到这一个上游服务器。可以使用 `proxy-next-upstream-config` 注释来设置在哪些情况下 ALB 可以尝试将请求传递到下一个上游服务器，以及尝试的时间长度和次数。<br><br><strong>注</strong>：使用 `proxy-next-upstream-config` 时，始终会配置超时，所以不要将 `timeout=true` 添加到此注释。</dd>
<dt>样本 Ingress 资源 YAML</dt>
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
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为您针对应用程序创建的 Kubernetes 服务的名称。</td>
</tr>
<tr>
<td><code>retries</code></td>
<td>将 <code>&lt;<em>tries</em>&gt;</code> 替换为 ALB 尝试将请求传递到下一个上游服务器的最大次数。此数字包括原始请求。要关闭此限制，请使用 <code>0</code>。如果不指定值，将使用缺省值 <code>0</code>。
</td>
</tr>
<tr>
<td><code>timeout</code></td>
<td>将 <code>&lt;<em>time</em>&gt;</code> 替换为 ALB 尝试将请求传递到下一个上游服务器的最长时间（以秒为单位）。例如，要设置 30 秒时间，请输入 <code>30s</code>。要关闭此限制，请使用 <code>0</code>。如果不指定值，将使用缺省值 <code>0</code>。
</td>
</tr>
<tr>
<td><code>error</code></td>
<td>如果设置为 <code>true</code>，那么在与第一个上游服务器建立连接，向其传递请求或读取响应头时发生错误的情况下，ALB 会将请求传递到下一个上游服务器。</td>
</tr>
<tr>
<td><code>invalid_header</code></td>
<td>如果设置为 <code>true</code>，那么在第一个上游服务器返回空响应或无效响应时，ALB 会将请求传递到下一个上游服务器。</td>
</tr>
<tr>
<td><code>http_502</code></td>
<td>如果设置为 <code>true</code>，那么在第一个上游服务器返回代码为 502 的响应时，ALB 会将请求传递到下一个上游服务器。可以指定以下 HTTP 响应代码：<code>500</code>、<code>502</code>、<code>503</code>、<code>504</code>、<code>403</code>、<code>404</code> 或 <code>429</code>。
</td>
</tr>
<tr>
<td><code>non_idempotent</code></td>
<td>如果设置为 <code>true</code>，那么 ALB 可以使用非幂等方法将请求传递到下一个上游服务器。缺省情况下，ALB 不会将这些请求传递到下一个上游服务器。
</td>
</tr>
<tr>
<td><code>off</code></td>
<td>要阻止 ALB 将请求传递到下一个上游服务器，请设置为 <code>true</code>。
</td>
</tr>
</tbody></table>
</dd>
</dl>

<br />


### 使用 cookie 确保会话亲缘关系 (sticky-cookie-services)
{: #sticky-cookie-services}

使用粘性 cookie 注释向 ALB 添加会话亲缘关系，并始终将入局网络流量路由到同一个上游服务器。
{:shortdesc}

<dl>
<dt>描述</dt>
<dd>为了实现高可用性，某些应用程序设置要求您部署多个上游服务器来处理入局客户机请求。客户机连接到后端应用程序后，可以使用会话亲缘关系，使得在会话期间或完成任务所需的时间内，客户机由同一个上游服务器处理。您可以将 ALB 配置为始终将入局网络流量路由到同一个上游服务器来确保会话亲缘关系。


</br></br>
连接到后端应用程序的每个客户机都会由 ALB 分配其中一个可用的上游服务器。ALB 会创建会话 cookie，该会话 cookie 存储在客户机的应用程序中，并且包含在 ALB 和客户机之间每个请求的头信息中。该 cookie 中的信息将确保在整个会话中的所有请求都由同一个上游服务器进行处理。


</br></br>
当包含多个服务时，请使用分号 (;) 来分隔这些服务。</dd>
<dt>样本 Ingress 资源 YAML</dt>
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
  <caption>了解注释的组成部分</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为您针对应用程序创建的 Kubernetes 服务的名称。</td>
  </tr>
  <tr>
  <td><code>name</code></td>
  <td>将 <code><em>&lt;cookie_name&gt;</em></code> 替换为在会话期间创建的粘性 cookie 的名称。</td>
  </tr>
  <tr>
  <td><code>expires</code></td>
  <td>将 <code>&lt;<em>expiration_time</em>&gt;</code> 替换为时间，以秒 (s)、分钟 (m) 或小时 (h) 为单位；超过该时间后，粘性 cookie 到期。此时间与用户活动无关。该 cookie 到期后，会被客户机 Web 浏览器删除，并且不会再发送到 ALB。例如，要将到期时间设置为 1 秒、1 分钟或 1 小时，请输入 <code>1s</code>、<code>1m</code> 或 <code>1h</code>。</td>
  </tr>
  <tr>
  <td><code>path</code></td>
  <td>将 <code><em>&lt;cookie_path&gt;</em></code> 替换为附加到 Ingress 子域的路径，该路径指示针对哪些域和子域将 cookie 发送到 ALB。例如，如果 Ingress 域为 <code>www.myingress.com</code>，并且您希望在每个客户机请求中都发送 cookie，那么必须设置 <code>path=/</code>。如果希望仅针对 <code>www.myingress.com/myapp</code> 及其所有子域发送 cookie，那么必须设置 <code>path=/myapp</code>。</td>
  </tr>
  <tr>
  <td><code>hash</code></td>
  <td>将 <code><em>&lt;hash_algorithm&gt;</em></code> 替换为用于保护 cookie 中信息的散列算法。仅支持 <code>sha1</code>。SHA1 基于 cookie 中的信息创建散列总和，并将此散列总和附加到 cookie。服务器可以解密 cookie 中的信息并验证数据完整性。</td>
  </tr>
  </tbody></table>

 </dd></dl>

<br />




### 上游保持活动连接数 (upstream-keepalive)
{: #upstream-keepalive}

设置上游服务器的最大空闲保持活动连接数。
{:shortdesc}

<dl>
<dt>描述</dt>
<dd>
设置给定服务的上游服务器的最大空闲保持活动连接数。缺省情况下，上游服务器具有 64 个空闲保持活动连接。
  </dd>


 <dt>样本 Ingress 资源 YAML</dt>
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
 <caption>了解注释的组成部分</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为您针对应用程序创建的 Kubernetes 服务的名称。</td>
  </tr>
  <tr>
  <td><code>keepalive</code></td>
  <td>将 <code>&lt;<em>max_connections</em>&gt;</code> 替换为上游服务器的最大空闲保持活动连接数。缺省值为 <code>64</code>。值为 <code>0</code> 将禁用给定服务的上游保持活动连接。</td>
  </tr>
  </tbody></table>
  </dd>
  </dl>

<br />




## HTTPS 和 TLS/SSL 认证注释
{: #https-auth}

### {{site.data.keyword.appid_short_notm}} 认证 (appid-auth)
{: #appid-auth}

  使用 {{site.data.keyword.appid_full_notm}} 向应用程序进行认证。
{:shortdesc}

  <dl>
  <dt>描述</dt>
  <dd>
  使用 {{site.data.keyword.appid_short_notm}} 对 Web 或 API HTTP/HTTPS 请求进行认证。

  <p>如果将请求类型设置为 <code>web</code>，那么将验证包含 {{site.data.keyword.appid_short_notm}} 访问令牌的 Web 请求。如果令牌验证失败，将拒绝 Web 请求。如果请求不包含访问令牌，那么会将请求重定向到 {{site.data.keyword.appid_short_notm}} 登录页面。<strong>注</strong>：要使 {{site.data.keyword.appid_short_notm}} Web 认证能够正常运作，必须在用户的浏览器中启用 cookie。</p>

  <p>如果将请求类型设置为 <code>api</code>，那么将验证包含 {{site.data.keyword.appid_short_notm}} 访问令牌的 API 请求。如果请求不包含访问令牌，将向用户返回 <code>401: Unauthorized</code> 错误消息。</p>

  <p>**注**：出于安全原因，{{site.data.keyword.appid_short_notm}} 认证仅支持启用了 TLS/SSL 的后端。</p>
  </dd>
   <dt>样本 Ingress 资源 YAML</dt>
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
   <caption>了解注释的组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td><code>bindSecret</code></td>
    <td>将 <em><code>&lt;bind_secret&gt;</code></em> 替换为存储绑定私钥的 Kubernetes 私钥。</td>
    </tr>
    <tr>
    <td><code>namespace</code></td>
    <td>将 <em><code>&lt;namespace&gt;</code></em> 替换为绑定私钥的名称空间。此字段缺省为 `default` 名称空间。</td>
    </tr>
    <tr>
    <td><code>requestType</code></td>
    <td>将 <code><em>&lt;request_type&gt;</em></code> 替换为要发送到 {{site.data.keyword.appid_short_notm}} 的请求的类型。接受的值为 `web` 或 `api`。缺省值为 `api`。</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td>将 <code><em>&lt;myservice&gt;</em></code> 替换为您针对应用程序创建的 Kubernetes 服务的名称。此字段是可选的。如果未包含服务名称，那么将对所有服务启用注释。如果包含服务名称，那么仅对该服务启用注释。使用分号 (;) 分隔多个服务。</td>
    </tr>
    </tbody></table>
    </dd>
    <dt>用法</dt>
    <dd>由于应用程序使用 {{site.data.keyword.appid_short_notm}} 进行认证，因此必须供应 {{site.data.keyword.appid_short_notm}} 实例，使用有效的重定向 URI 来配置实例，并生成绑定密钥。
    <ol>
    <li>供应 [{{site.data.keyword.appid_short_notm}} 实例](https://console.bluemix.net/catalog/services/app-id)。</li>
    <li>在 {{site.data.keyword.appid_short_notm}} 管理控制台中，为应用程序添加 redirectURI。</li>
    <li>创建绑定密钥。
    <pre class="pre"><code>bx cs cluster-service-bind &lt;my_cluster&gt; &lt;my_namespace&gt; &lt;my_service_instance_GUID&gt;</code></pre> </li>
    <li>配置 <code>appid-auth</code> 注释。</li>
    </ol></dd>
    </dl>

<br />



### 定制 HTTP 和 HTTPS 端口 (custom-port)
{: #custom-port}

更改 HTTP（端口 80）和 HTTPS（端口 443）网络流量的缺省端口。
{:shortdesc}

<dl>
<dt>描述</dt>
<dd>缺省情况下，Ingress ALB 配置为在端口 80 上侦听入局 HTTP 网络流量，在端口 443 上侦听入局 HTTPS 网络流量。您可以更改缺省端口以向 ALB 域添加安全性，或仅启用 HTTPS 端口。

</dd>


<dt>样本 Ingress 资源 YAML</dt>
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
<caption>了解注释的组成部分</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;protocol&gt;</code></td>
 <td>输入 <code>http</code> 或 <code>https</code> 以更改入局 HTTP 或 HTTPS 网络流量的缺省端口。</td>
 </tr>
 <tr>
 <td><code>&lt;port&gt;</code></td>
 <td>输入要用于入局 HTTP 或 HTTPS 网络流量的端口号。<p><strong>注：</strong>当为 HTTP 或 HTTPS 指定定制端口时，缺省端口对于 HTTP 和 HTTPS 都不再有效。例如，要将 HTTPS 的缺省端口更改为 8443，而对 HTTP 使用缺省端口，那么必须为两者设置定制端口：<code>custom-port: "protocol=http port=80; protocol=https port=8443"</code>。</p></td>
 </tr>
 </tbody></table>

 </dd>
 <dt>用法</dt>
 <dd><ol><li>查看 ALB 的打开端口。
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
CLI 输出类似于以下内容：
<pre class="screen">
<code>NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx 80:30416/TCP,443:32668/TCP   109d</code></pre></li>
<li>打开 ALB 配置映射。
<pre class="pre">
<code>kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system</code></pre></li>
<li>将非缺省 HTTP 和 HTTPS 端口添加到配置映射。将 &lt;port&gt; 替换为要打开的 HTTP 或 HTTPS 端口。
<b>注</b>：缺省情况下，端口 80 和 443 已打开。如果要使 80 和 443 保持打开，那么必须在 `public-ports` 字段中包含这两个端口以及您指定的其他任何 TCP 端口。如果启用了专用 ALB，那么还必须在 `private-ports` 字段中指定要保持打开的任何端口。有关更多信息，请参阅<a href="cs_ingress.html#opening_ingress_ports">在 Ingress ALB 中打开端口</a>。
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
 <li>验证是否已将 ALB 重新配置为使用非缺省端口。
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
CLI 输出类似于以下内容：
<pre class="screen">
<code>NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx  169.xx.xxx.xxx &lt;port1&gt;:30776/TCP,&lt;port2&gt;:30412/TCP   109d</code></pre></li>
<li>配置 Ingress 以在将入局网络流量路由到服务时使用非缺省端口。在此引用中使用样本 YAML 文件。</li>
<li>更新 ALB 配置。
<pre class="pre">
<code>        kubectl apply -f myingress.yaml
        </code></pre>
</li>
<li>打开首选 Web 浏览器以访问您的应用程序。示例：<code>https://&lt;ibmdomain&gt;:&lt;port&gt;/&lt;service_path&gt;/</code></li></ol></dd></dl>

<br />



### HTTP 重定向到 HTTPS (redirect-to-https)
{: #redirect-to-https}

将不安全的 HTTP 客户机请求转换为 HTTPS。
{:shortdesc}

<dl>
<dt>描述</dt>
<dd>将 Ingress ALB 设置为通过 IBM 提供的 TLS 证书或定制 TLS 证书来保护域。某些用户可能会尝试向 ALB 域发出不安全的 <code>http</code> 请求（例如 <code>http://www.myingress.com</code>，而不是使用 <code>https</code>）来访问应用程序。可以使用重定向注释始终将不安全的 HTTP 请求转换为 HTTPS。如果不使用此注释，缺省情况下，不安全的 HTTP 请求不会转换为 HTTPS 请求，并且可能会向公众公开未加密的保密信息。

 

</br></br>
缺省情况下，HTTP 请求到 HTTPS 的重定向是禁用的。</dd>

<dt>样本 Ingress 资源 YAML</dt>
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


### HTTP 严格传输安全性 (hsts)
{: #hsts}

<dl>
<dt>描述</dt>
<dd>
HSTS 指示浏览器仅使用 HTTPS 访问域。即使用户输入或访问普通 HTTP 链接，浏览器也会严格地将连接升级到 HTTPS。
</dd>


<dt>样本 Ingress 资源 YAML</dt>
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
<caption>了解注释的组成部分</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
  </thead>
  <tbody>
  <tr>
  <td><code>enabled</code></td>
  <td>使用 <code>true</code> 以启用 HSTS。</td>
  </tr>
    <tr>
  <td><code>maxAge</code></td>
  <td>将 <code>&lt;<em>31536000</em>&gt;</code> 替换为整数，该整数表示浏览器将发送的请求直接高速缓存到 HTTPS 的秒数。缺省值为 <code>31536000</code>，等于 1 年。</td>
  </tr>
  <tr>
  <td><code>includeSubdomains</code></td>
  <td>使用 <code>true</code> 以指示浏览器 HSTS 策略也适用于当前域的所有子域。缺省值为 <code>true</code>。</td>
  </tr>
  </tbody></table>

  </dd>
  </dl>


<br />


### 相互认证 (mutual-auth)
{: #mutual-auth}

为 ALB 配置相互认证。
{:shortdesc}

<dl>
<dt>描述</dt>
<dd>
为 Ingress ALB 配置下游流量的相互认证。外部客户机会认证服务器，而服务器也会使用证书来认证客户机。相互认证也称为基于证书的认证或双向认证。
 </dd>

<dt>先决条件</dt>
<dd>
<ul>
<li>[您必须具有包含所需认证中心 (CA) 的有效私钥](cs_app.html#secrets)。此外，还需要 <code>client.key</code> 和 <code>client.crt</code> 才能通过相互认证进行认证。</li>
<li>要在 443 之外的端口上启用相互认证，请[配置 ALB 以打开有效端口](cs_ingress.html#opening_ingress_ports)。</li>
</ul>
</dd>


<dt>样本 Ingress 资源 YAML</dt>
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
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>secretName</code></td>
<td>将 <code>&lt;<em>mysecret</em>&gt;</code> 替换为私钥资源的名称。</td>
</tr>
<tr>
<td><code>port</code></td>
<td>将 <code>&lt;<em>port</em>&gt;</code> 替换为 ALB 端口号。</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>将 <code><em>&lt;serviceName&gt;</em></code> 替换为一个或多个 Ingress 资源的名称。此参数是可选的。</td>
</tr>
</tbody></table>

</dd>
</dl>

<br />



### SSL 服务支持 (ssl-services)
{: #ssl-services}

允许对上游应用程序进行 HTTPS 请求，并对流入上游应用程序的流量加密。
{:shortdesc}

<dl>
<dt>描述</dt>
<dd>
加密 Ingress 发送到需要 HTTPS 的上游应用程序的流量。如果上游应用程序可以处理 TLS，那么可以选择提供 TLS 私钥中包含的证书。<br></br>**可选**：可以向此注释添加[单向认证或相互认证](#ssl-services-auth)。</dd>


<dt>样本 Ingress 资源 YAML</dt>
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
<caption>了解注释的组成部分</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
  </thead>
  <tbody>
  <tr>
  <td><code>ssl-service</code></td>
  <td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为需要 HTTPS 的服务的名称。将加密从 ALB 到此应用程序服务的流量。</td>
  </tr>
  <tr>
  <td><code>ssl-secret</code></td>
  <td>可选：如果要使用 TLS 私钥，并且上游应用程序可以处理 TLS，请将 <code>&lt;<em>service-ssl-secret</em>&gt;</code> 替换为该服务的私钥。如果提供了私钥，那么值必须包含应用程序期望从客户机收到的 <code>trusted.crt</code>、<code>client.crt</code> 和 <code>client.key</code>。要创建 TLS 私钥，请先[将证书和密钥转换为 Base64 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.base64encode.org/)。然后，请参阅[创建私钥](cs_app.html#secrets)。</td>
  </tr>
  </tbody></table>

  </dd>
</dl>

<br />


#### 使用认证的 SSL 服务支持
{: #ssl-services-auth}

<dl>
<dt>描述</dt>
<dd>
允许向上游应用程序发出 HTTPS 请求，以及使用单向认证或相互认证加密流入上游应用程序的流量，以实现额外的安全性。
</dd>


<dt>样本 Ingress 资源 YAML</dt>
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
<caption>了解注释的组成部分</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
  </thead>
  <tbody>
  <tr>
  <td><code>ssl-service</code></td>
  <td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为需要 HTTPS 的服务的名称。将加密从 ALB 到此应用程序服务的流量。</td>
  </tr>
  <tr>
  <td><code>ssl-secret</code></td>
  <td>将 <code>&lt;<em>service-ssl-secret</em>&gt;</code> 替换为服务的相互认证私钥。值必须包含应用程序期望从客户机收到的 CA 证书。要创建相互认证私钥，请先[将证书和密钥转换为 Base64 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://www.base64encode.org/)。然后，请参阅[创建私钥](cs_app.html#secrets)。</td>
  </tr>
  </tbody></table>

  </dd>
  </dl>

<br />


## Istio 注释
{: #istio-annotations}

### Istio 服务 (istio-services)
{: #istio-services}

  将流量路由到 Istio 管理的服务。
  {:shortdesc}

  <dl>
  <dt>描述</dt>
  <dd>
  如果您有 Istio 管理的服务，那么可以使用集群 ALB 将 HTTP/HTTPS 请求路由到 Istio Ingress 控制器。然后，Istio Ingress 控制器会将请求路由到应用程序服务。为了路由流量，必须对集群 ALB 和 Istio Ingress 控制器的 Ingress 资源进行更改。
    <br><br>在集群 ALB 的 Ingress 资源中，必须：
      <ul>
        <li>指定 `istio-services` 注释</li>
        <li>将服务路径定义为应用程序侦听的实际路径</li>
        <li>将服务端口定义为 Istio Ingress 控制器的端口</li>
      </ul>
    <br>在 Istio Ingress 控制器的 Ingress 资源中，必须：
      <ul>
        <li>将服务路径定义为应用程序侦听的实际路径</li>
        <li>将服务端口定义为 Istio Ingress 控制器公开的应用程序服务的 HTTP/HTTPS 端口</li>
    </ul>
  </dd>

   <dt>集群 ALB 的样本 Ingress 资源 YAML</dt>
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
   <caption>了解 YAML 文件的组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 YAML 文件的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td><code>enabled</code></td>
      <td>要允许将流量路由到 Istio 管理的服务，请设置为 <code>True</code>。</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td>将 <code><em>&lt;myservice1&gt;</em></code> 替换为您针对 Istio 管理的应用程序创建的 Kubernetes 服务的名称。使用分号 (;) 分隔多个服务。此字段是可选的。如果未指定服务名称，那么将针对流量路由启用所有 Istio 管理的服务。</td>
    </tr>
    <tr>
    <td><code>istioServiceNamespace</code></td>
    <td>将 <code><em>&lt;istio-namespace&gt;</em></code> 替换为安装了 Istio 的 Kubernetes 名称空间。此字段是可选的。如果未指定名称空间，那么将使用 <code>istio-system</code> 名称空间。</td>
    </tr>
    <tr>
    <td><code>istioServiceName</code></td>
    <td>将 <code><em>&lt;istio-ingress-service&gt;</em></code> 替换为 Istio Ingress 服务的名称。此字段是可选的。如果未指定 Istio Ingress 服务名称，那么将使用服务名称 <code>istio-ingress</code>。</td>
    </tr>
    <tr>
    <td><code>path</code></td>
      <td>对于要将流量路由到的每个 Istio 管理的服务，请将 <code><em>&lt;/myapp1&gt;</em></code> 替换为该 Istio 管理的服务所侦听的后端路径。该路径必须对应于 Istio Ingress 资源中定义的路径。</td>
    </tr>
    <tr>
    <td><code>servicePort</code></td>
    <td>对于要将流量路由到的每个 Istio 管理的服务，请将 <code><em>&lt;istio_ingress_port&gt;</em></code> 替换为 Istio Ingress 控制器的端口。</td>
    </tr>
    </tbody></table>
    </dd>
    </dl>

## 代理缓冲区注释
{: #proxy-buffer}


### 客户机响应数据缓冲 (proxy-buffering)
{: #proxy-buffering}

使用缓冲注释可禁止在将数据发送到客户机期间，在 ALB 上存储响应数据。
{:shortdesc}

<dl>
<dt>描述</dt>
<dd>Ingress ALB 充当后端应用程序和客户机 Web 浏览器之间的代理。当响应从后端应用程序发送到客户机时，缺省情况下会在 ALB 上对响应数据进行缓冲。ALB 作为代理传递客户机响应，并开始按客户机的速度将响应发送到客户机。ALB 接收到来自后端应用程序的所有数据后，会关闭与后端应用程序的连接。ALB 与客户机的连接会保持打开状态，直到客户机接收完所有数据为止。

</br></br>
如果在 ALB 上禁用响应数据缓冲，那么数据会立即从 ALB 发送到客户机。客户机必须能够按 ALB 的速度来处理入局数据。如果客户机速度太慢，数据可能会丢失。
</br></br>
缺省情况下，ALB 上启用了响应数据缓冲。</dd>
<dt>样本 Ingress 资源 YAML</dt>
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
<caption>了解注释的组成部分</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
 </thead>
 <tbody>
 <tr>
 <td><code>enabled</code></td>
   <td>要禁用 ALB 上的响应数据缓冲，请设置为 <code>false</code>。</td>
 </tr>
 <tr>
 <td><code>serviceName</code></td>
 <td>将 <code><em>&lt;myservice1&gt;</em></code> 替换为您针对应用程序创建的 Kubernetes 服务的名称。使用分号 (;) 分隔多个服务。此字段是可选的。如果未指定服务名称，那么所有服务都将使用此注释。</td>
 </tr>
 </tbody></table>
 </dd>
 </dl>

<br />



### 代理缓冲区 (proxy-buffers)
{: #proxy-buffers}

为 ALB 配置代理缓冲区的数目和大小。
{:shortdesc}

<dl>
<dt>描述</dt>
<dd>
设置缓冲区的数目和大小，这些缓冲区用于读取来自通过代理传递的服务器的单个连接的响应。除非指定了服务，否则配置将应用于 Ingress 主机中的所有服务。例如，如果指定如 <code>serviceName=SERVICE number=2 size=1k</code> 这样的配置，那么会对该服务应用 1k。如果指定如 <code>number=2 size=1k</code> 这样的配置，那么会对 Ingress 主机中的所有服务应用 1k。</dd>
<dt>样本 Ingress 资源 YAML</dt>
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
<caption>了解注释的组成部分</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
 </thead>
 <tbody>
 <tr>
 <td><code>serviceName</code></td>
 <td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为要应用 proxy-buffers 的服务的名称。</td>
 </tr>
 <tr>
 <td><code>number</code></td>
 <td>将 <code>&lt;<em>number_of_buffers</em>&gt;</code> 替换为数字，例如 <em>2</em>。</td>
 </tr>
 <tr>
 <td><code>size</code></td>
 <td>将 <code>&lt;<em>size</em>&gt;</code> 替换为每个缓冲区的大小（以千字节（k 或 K）为单位），例如 <em>1K</em>。</td>
 </tr>
 </tbody>
 </table>
 </dd></dl>

<br />


### 代理缓冲区大小 (proxy-buffer-size)
{: #proxy-buffer-size}

配置代理缓冲区的大小，该缓冲区用于读取响应的第一部分。
{:shortdesc}

<dl>
<dt>描述</dt>
<dd>
设置缓冲区的大小，该缓冲区用于读取从通过代理传递的服务器收到的响应的第一部分。响应的这一部分通常包含小型响应头。除非指定了服务，否则配置将应用于 Ingress 主机中的所有服务。例如，如果指定如 <code>serviceName=SERVICE size=1k</code> 这样的配置，那么会对该服务应用 1k。如果指定如 <code>size=1k</code> 这样的配置，那么会对 Ingress 主机中的所有服务应用 1k。</dd>


<dt>样本 Ingress 资源 YAML</dt>
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
<caption>了解注释的组成部分</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
 </thead>
 <tbody>
 <tr>
 <td><code>serviceName</code></td>
 <td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为要应用 proxy-buffers-size 的服务的名称。</td>
 </tr>
 <tr>
 <td><code>size</code></td>
 <td>将 <code>&lt;<em>size</em>&gt;</code> 替换为每个缓冲区的大小（以千字节（k 或 K）为单位），例如 <em>1K</em>。</td>
 </tr>
 </tbody></table>

 </dd>
 </dl>

<br />



### 代理繁忙缓冲区大小 (proxy-busy-buffers-size)
{: #proxy-busy-buffers-size}

配置可以处于繁忙状态的代理缓冲区的大小。
{:shortdesc}

<dl>
<dt>描述</dt>
<dd>
限制在尚未完全读取响应期间，向客户机发送响应的任何缓冲区的大小。在此期间，其余缓冲区可用于读取响应，并可以根据需要将响应的一部分缓冲到临时文件。除非指定了服务，否则配置将应用于 Ingress 主机中的所有服务。例如，如果指定如 <code>serviceName=SERVICE size=1k</code> 这样的配置，那么会对该服务应用 1k。如果指定如 <code>size=1k</code> 这样的配置，那么会对 Ingress 主机中的所有服务应用 1k。</dd>


<dt>样本 Ingress 资源 YAML</dt>
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
<caption>了解注释的组成部分</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为要应用 proxy-busy-buffers-size 的服务的名称。</td>
</tr>
<tr>
<td><code>size</code></td>
<td>将 <code>&lt;<em>size</em>&gt;</code> 替换为每个缓冲区的大小（以千字节（k 或 K）为单位），例如 <em>1K</em>。</td>
</tr>
</tbody></table>

 </dd>
 </dl>

<br />



## 请求和响应注释
{: #request-response}


### 额外的客户机请求或响应头 (proxy-add-headers, response-add-headers)
{: #proxy-add-headers}

向客户机请求添加额外的头信息，然后将该请求发送到后端应用程序，或者向客户机响应添加额外的头信息，然后将该响应发送到客户机。
{:shortdesc}

<dl>
<dt>描述</dt>
<dd>Ingress ALB 充当客户机应用程序和后端应用程序之间的代理。发送到 ALB 的客户机请求经过处理（通过代理传递）后放入新的请求中，然后将该新请求发送到后端应用程序。与此类似，发送到 ALB 的后端应用程序响应经过处理（通过代理传递）后放入新的响应中，然后将该新响应发送到客户机。通过代理传递请求或响应会除去初始从客户机或后端应用程序发送的 HTTP 头信息，例如用户名。

<br><br>
如果后端应用程序需要 HTTP 头信息，那么可以使用 <code>proxy-add-headers</code> 注释向客户机请求添加头信息，然后由 ALB 将该请求转发到后端应用程序。

<br>
<ul><li>例如，在将请求转发到应用程序之前，可能需要将以下 X-Forward 头信息添加到请求中：

<pre class="screen">
<code>proxy_set_header Host $host;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;</code></pre>

</li>

<li>要将 X-Forward 头信息添加到发送到应用程序的请求中，请按以下方式使用 `proxy-add-headers` 注释：

<pre class="screen">
<code>ingress.bluemix.net/proxy-add-headers: |
serviceName=<myservice1> {
  Host $host;
  X-Forwarded-Proto $scheme;
  X-Forwarded-For $proxy_add_x_forwarded_for;
  }</code></pre>

</li></ul><br>

如果客户机 Web 应用程序需要 HTTP 头信息，那么可以使用 <code>response-add-headers</code> 注释将头信息添加到响应，然后由 ALB 将响应转发到客户机 Web 应用程序。</dd>

<dt>样本 Ingress 资源 YAML</dt>
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
 <caption>了解注释的组成部分</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
  </thead>
  <tbody>
  <tr>
  <td><code>service_name</code></td>
  <td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为您针对应用程序创建的 Kubernetes 服务的名称。</td>
  </tr>
  <tr>
  <td><code>&lt;header&gt;</code></td>
  <td>要添加到客户机请求或客户机响应的头信息的键。</td>
  </tr>
  <tr>
  <td><code>&lt;value&gt;</code></td>
  <td>要添加到客户机请求或客户机响应的头信息的值。</td>
  </tr>
  </tbody></table>
 </dd></dl>

<br />



### 除去客户机响应头 (response-remove-headers)
{: #response-remove-headers}

除去后端应用程序的客户机响应中包含的头信息，然后将该响应发送到客户机。
{:shortdesc}

 <dl>
 <dt>描述</dt>
 <dd>Ingress ALB 充当后端应用程序和客户机 Web 浏览器之间的代理。发送到 ALB 的来自后端应用程序的客户机响应经过处理（通过代理传递）后放入新的响应中，然后将该新响应从 ALB 发送到客户机 Web 浏览器。尽管通过代理传递响应会除去初始从后端应用程序发送的 HTTP 头信息，但此过程可能不会除去所有特定于后端应用程序的头。从客户机响应中除去头信息，然后将该响应从 ALB 转发到客户机 Web 浏览器。</dd>
 <dt>样本 Ingress 资源 YAML</dt>
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
  <caption>了解注释的组成部分</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
   </thead>
   <tbody>
   <tr>
   <td><code>service_name</code></td>
   <td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为您针对应用程序创建的 Kubernetes 服务的名称。</td>
   </tr>
   <tr>
   <td><code>&lt;header&gt;</code></td>
   <td>要从客户机响应中除去的头的键。</td>
   </tr>
   </tbody></table>
   </dd></dl>

<br />


### 客户机请求主体大小 (client-max-body-size)
{: #client-max-body-size}

设置客户机可以作为请求一部分发送的主体的最大大小。
{:shortdesc}

<dl>
<dt>描述</dt>
<dd>为了保持期望的性能，最大客户机请求主体大小设置为 1 兆字节。将主体大小超出此限制的客户机请求发送到 Ingress ALB，并且客户机不允许拆分数据时，ALB 会向客户机返回 413（请求实体太大）HTTP 响应。除非减小请求主体的大小，否则无法在客户机和 ALB 之间建立连接。客户机允许数据拆分为多个区块时，数据将分成 1 兆字节的包并发送到 ALB。

</br></br>
您可能希望增大最大主体大小，因为您预期客户机请求的主体大小大于 1 兆字节。例如，您希望客户机能够上传大型文件。增大最大请求主体大小可能会影响 ALB 的性能，因为与客户机的连接必须保持打开状态，直到接收完请求为止。
</br></br>
<strong>注</strong>：某些客户机 Web 浏览器无法正确显示 413 HTTP 响应消息。</dd>
<dt>样本 Ingress 资源 YAML</dt>
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
<caption>了解注释的组成部分</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;size&gt;</code></td>
 <td>客户机响应主体的最大大小。例如，要将最大大小设置为 200 兆字节，请定义 <code>200m</code>。<strong>注</strong>：可以将大小设置为 0 以禁止检查客户机请求主体大小。</td>
 </tr>
 </tbody></table>

 </dd></dl>

<br />


### 大型客户机头缓冲区 (large-client-header-buffers)
{: #large-client-header-buffers}

设置读取大型客户机请求头的缓冲区的最大数目和大小。
{:shortdesc}

<dl>
<dt>描述</dt>
<dd>读取大型客户机请求头的缓冲区仅按需进行分配：如果在请求结尾处理后连接已转换为保持活动状态，那么将释放这些缓冲区。缺省情况下，缓冲区大小等于 <code>8K</code> 字节。如果请求行超过一个缓冲区的设定最大大小，那么会向客户机返回 <code>414 Request-URI Too Large</code> 错误。此外，如果请求头字段超过一个缓冲区的设定最大大小，那么会向客户机返回 <code>400 Bad Request</code> 错误。可以调整用于读取大型客户机请求头的缓冲区的最大数目和大小。

<dt>样本 Ingress 资源 YAML</dt>
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
<caption>了解注释的组成部分</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;number&gt;</code></td>
 <td>应该为读取大型客户机请求头而分配的最大缓冲区数。例如，要将其设置为 4，请定义 <code>4</code>。</td>
 </tr>
 <tr>
 <td><code>&lt;size&gt;</code></td>
 <td>读取大型客户机请求头的缓冲区的最大大小。例如，要将其设置为 16 千字节，请定义 <code>16k</code>。
   <strong>注：</strong>对于千字节，大小必须以 <code>k</code> 结尾，对于兆字节，必须以 <code>m</code> 结尾。</td>
 </tr>
</tbody></table>
</dd>
</dl>

<br />


## 服务限制注释
{: #service-limit}


### 全局速率限制 (global-rate-limit)
{: #global-rate-limit}

对于所有服务，按定义的键限制请求处理速率和连接数。
{:shortdesc}

<dl>
<dt>描述</dt>
<dd>
对于所有服务，按定义的键限制来自所选后端中所有路径的单个 IP 地址的请求处理速率和连接数。
</dd>


 <dt>样本 Ingress 资源 YAML</dt>
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
 <caption>了解注释的组成部分</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
  </thead>
  <tbody>
  <tr>
  <td><code>key</code></td>
  <td>要基于位置或服务来设置入局请求的全局限制，请使用 `key=location`。要基于头来设置入局请求的全局限制，请使用 `X-USER-ID key=$http_x_user_id`。</td>
  </tr>
  <tr>
  <td><code>rate</code></td>
  <td>将 <code>&lt;<em>rate</em>&gt;</code> 替换为处理速率。输入以每秒速率 (r/s) 或每分钟速率 (r/m) 为单位的值。示例：<code>50r/m</code>。</td>
  </tr>
  <tr>
  <td><code>number-of_connections</code></td>
  <td>将 <code>&lt;<em>conn</em>&gt;</code> 替换为连接数。</td>
  </tr>
  </tbody></table>

  </dd>
  </dl>

<br />



### 服务速率限制 (service-rate-limit)
{: #service-rate-limit}

限制特定服务的请求处理速率和连接数。
{:shortdesc}

<dl>
<dt>描述</dt>
<dd>对于特定服务，按定义的键限制来自所选后端中所有路径的单个 IP 地址的请求处理速率和连接数。
</dd>


 <dt>样本 Ingress 资源 YAML</dt>
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
 <caption>了解注释的组成部分</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解注释的组成部分</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td>将 <code>&lt;<em>myservice</em>&gt;</code> 替换为要限制其处理速率的服务的名称。</li>
  </tr>
  <tr>
  <td><code>key</code></td>
  <td>要基于位置或服务来设置入局请求的全局限制，请使用 `key=location`。要基于头来设置入局请求的全局限制，请使用 `X-USER-ID key=$http_x_user_id`。</td>
  </tr>
  <tr>
  <td><code>rate</code></td>
  <td>将 <code>&lt;<em>rate</em>&gt;</code> 替换为处理速率。要定义每秒速率，请使用 r/s：<code>10r/s</code>。要定义每分钟速率，请使用 r/m：<code>50r/m</code>。</td>
  </tr>
  <tr>
  <td><code>number-of_connections</code></td>
  <td>将 <code>&lt;<em>conn</em>&gt;</code> 替换为连接数。</td>
  </tr>
  </tbody></table>
  </dd>
  </dl>

  <br />



