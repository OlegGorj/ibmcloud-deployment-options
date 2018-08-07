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



# Ingress 註釋
{: #ingress_annotation}

若要將功能新增至 Ingress 應用程式負載平衡器 (ALB)，您可以將註釋指定為 Ingress 資源中的 meta 資料。
{: shortdesc}

如需 Ingress 服務及如何開始使用它們的一般資訊，請參閱[使用 Ingress 來管理網路資料流量](cs_ingress.html#planning)。

<table>
<caption>一般註釋</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>一般註釋</th>
 <th>名稱</th>
 <th>說明</th>
 </thead>
 <tbody>
 <tr>
 <td><a href="#proxy-external-service">外部服務</a></td>
 <td><code>proxy-external-service</code></td>
 <td>將路徑定義新增至外部服務，例如 {{site.data.keyword.Bluemix_notm}} 中管理的服務。</td>
 </tr>
 <tr>
 <td><a href="#location-modifier">位置修飾元</a></td>
 <td><code>location-modifier</code></td>
 <td>修改 ALB 比對要求 URI 與應用程式路徑的方式。</td>
 </tr>
 <tr>
 <td><a href="#alb-id">專用 ALB 遞送</a></td>
 <td><code>ALB-ID</code></td>
 <td>使用專用 ALB，將送入要求遞送至應用程式。</td>
 </tr>
 <tr>
 <td><a href="#rewrite-path">重寫路徑</a></td>
 <td><code>rewrite-path</code></td>
 <td>將送入的網路資料流量遞送至後端應用程式接聽的不同路徑。</td>
 </tr>
 <tr>
 <td><a href="#tcp-ports">TCP 埠</a></td>
 <td><code>tcp-ports</code></td>
 <td>透過非標準 TCP 埠存取應用程式。</td>
 </tr>
 </tbody></table>

<br>

<table>
<caption>連線註釋</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>連線註釋</th>
 <th>名稱</th>
 <th>說明</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#proxy-connect-timeout">自訂連接逾時和讀取逾時</a></td>
  <td><code>proxy-connect-timeout、proxy-read-timeout</code></td>
  <td>設定 ALB 等待連接及讀取後端應用程式的時間，在此時間之後，後端應用程式將被視為無法使用。</td>
  </tr>
  <tr>
  <td><a href="#keepalive-requests">保留作用中要求</a></td>
  <td><code>keepalive-requests</code></td>
  <td>設定可以透過一個保留作用中連線提供的要求數目上限。</td>
  </tr>
  <tr>
  <td><a href="#keepalive-timeout">保留作用中逾時</a></td>
  <td><code>keepalive-timeout</code></td>
  <td>設定保留作用中連線在伺服器上保持開啟的時間上限。</td>
  </tr>
  <tr>
  <td><a href="#proxy-next-upstream-config">Proxy 下一個上游</a></td>
  <td><code>proxy-next-upstream-config</code></td>
  <td>設定 ALB 可以將要求傳遞給下一個上游伺服器的時間。</td>
  </tr>
  <tr>
  <td><a href="#sticky-cookie-services">Cookie 的階段作業親緣性</a></td>
  <td><code>sticky-cookie-services</code></td>
  <td>使用 Sticky Cookie 一律將送入的網路資料流量遞送至相同的上游伺服器。</td>
  </tr>
  <tr>
  <td><a href="#upstream-keepalive">上游保留作用中</a></td>
  <td><code>upstream-keepalive</code></td>
  <td>設定上游伺服器之閒置保留作用中連線的數目上限。</td>
  </tr>
  </tbody></table>

<br>

  <table>
  <caption>HTTPS 及 TLS/SSL 鑑別註釋</caption>
  <col width="20%">
  <col width="20%">
  <col width="60%">
  <thead>
  <th>HTTPS 及 TLS/SSL 鑑別註釋</th>
  <th>名稱</th>
  <th>說明</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#appid-auth">{{site.data.keyword.appid_short}} 鑑別</a></td>
  <td><code>appid-auth</code></td>
  <td>使用 {{site.data.keyword.appid_full_notm}} 對您的應用程式進行鑑別。</td>
  </tr>
  <tr>
  <td><a href="#custom-port">自訂 HTTP 及 HTTPS 埠</a></td>
  <td><code>custom-port</code></td>
  <td>變更 HTTP 及 HTTPS 網路資料流量的預設埠，HTTP 為埠 80，HTTPS 為埠 443。
</td>
  </tr>
  <tr>
  <td><a href="#redirect-to-https">HTTP 重新導向至 HTTPS</a></td>
  <td><code>redirect-to-https</code></td>
  <td>將您網域上不安全的 HTTP 要求重新導向 HTTPS。</td>
  </tr>
  <tr>
  <td><a href="#hsts">HTTP 強制安全傳輸技術 (HSTS)</a></td>
  <td><code>hsts</code></td>
  <td>將瀏覽器設為僅使用 HTTPS 存取網域。</td>
  </tr>
  <tr>
  <td><a href="#mutual-auth">交互鑑別</a></td>
  <td><code>mutual-auth</code></td>
  <td>配置 ALB 的交互鑑別。</td>
  </tr>
  <tr>
  <td><a href="#ssl-services">SSL 服務支援</a></td>
  <td><code>ssl-services</code></td>
  <td>容許 SSL 服務支援加密要送至需要 HTTPS 的上游應用程式的資料流量。</td>
  </tr>
  </tbody></table>

<br>

<table>
<caption>Istio 註釋</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Istio 註釋</th>
<th>名稱</th>
<th>說明</th>
</thead>
<tbody>
<tr>
<td><a href="#istio-services">Istio 服務</a></td>
<td><code>istio-services</code></td>
<td>將資料流量遞送至 Istio 受管理服務。</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Proxy 緩衝區註釋</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>Proxy 緩衝區註釋</th>
 <th>名稱</th>
 <th>說明</th>
 </thead>
 <tbody>
 <tr>
 <td><a href="#proxy-buffering">用戶端回應資料緩衝</a></td>
 <td><code>proxy-buffering</code></td>
 <td>將回應傳送至用戶端時，停用 ALB 上的用戶端回應緩衝處理。</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffers">Proxy 緩衝區</a></td>
 <td><code>proxy-buffers</code></td>
 <td>針對來自 Proxy 伺服器的單一連線，設定讀取回應的緩衝區大小及數目。</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffer-size">Proxy 緩衝區大小</a></td>
 <td><code>proxy-buffer-size</code></td>
 <td>設定緩衝區大小，以讀取從 Proxy 伺服器收到之回應的第一部分。</td>
 </tr>
 <tr>
 <td><a href="#proxy-busy-buffers-size">Proxy 工作中緩衝區大小</a></td>
 <td><code>proxy-busy-buffers-size</code></td>
 <td>設定可以在忙碌中的 Proxy 緩衝區大小。</td>
 </tr>
 </tbody></table>

<br>

<table>
<caption>要求及回應註釋</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>要求及回應註釋</th>
<th>名稱</th>
<th>說明</th>
</thead>
<tbody>
<tr>
<td><a href="#proxy-add-headers">額外的用戶端要求或回應標頭</a></td>
<td><code>proxy-add-headers、response-add-headers</code></td>
<td>將標頭資訊新增至用戶端要求，然後再將要求轉遞至後端應用程式，或新增至用戶端回應，然後再將回應傳送至用戶端。</td>
</tr>
<tr>
<td><a href="#response-remove-headers">移除用戶端回應標頭</a></td>
<td><code>response-remove-headers</code></td>
<td>從用戶端回應移除標頭資訊，然後才將回應轉遞至用戶端。</td>
</tr>
<tr>
<td><a href="#client-max-body-size">用戶端要求內文大小</a></td>
<td><code>client-max-body-size</code></td>
<td>設定用戶端在要求中所能傳送的內文大小上限。</td>
</tr>
<tr>
<td><a href="#large-client-header-buffers">大型用戶端標頭緩衝區</a></td>
<td><code>large-client-header-buffers</code></td>
<td>設定讀取大型用戶端要求標頭的緩衝區數目上限及大小。</td>
</tr>
</tbody></table>

<br>

<table>
<caption>服務限制註釋</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>服務限制註釋</th>
<th>名稱</th>
<th>說明</th>
</thead>
<tbody>
<tr>
<td><a href="#global-rate-limit">廣域速率限制</a></td>
<td><code>global-rate-limit</code></td>
<td>針對所有服務的已定義金鑰，限制要求處理速率及連線數。</td>
</tr>
<tr>
<td><a href="#service-rate-limit">服務速率限制</a></td>
<td><code>service-rate-limit</code></td>
<td>針對特定服務的已定義金鑰，限制要求處理速率及連線數。</td>
</tr>
</tbody></table>

<br>



## 一般註釋
{: #general}

### 外部服務 (proxy-external-service)
{: #proxy-external-service}

將路徑定義新增至外部服務，例如 {{site.data.keyword.Bluemix_notm}} 中管理的服務。
{:shortdesc}

<dl>
<dt>說明</dt>
<dd>將路徑定義新增至外部服務。只有在您的應用程式在外部服務上作業，而非在後端服務上作業時才使用此註釋。當您使用此註釋建立外部服務路徑時，只會一起支援 `client-max-body-size`、`proxy-read-timeout`、`proxy-connect-timeout` 及 `proxy-buffering` 註釋。任何其他註釋都不支援與 `proxy-external-service` 一起使用。
</dd>
<dt>Ingress 資源範例 YAML</dt>
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
<caption>瞭解註釋元件</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
 </thead>
 <tbody>
 <tr>
 <td><code>path</code></td>
 <td>將 <code>&lt;<em>mypath</em>&gt;</code> 取代為外部服務接聽的路徑。</td>
 </tr>
 <tr>
 <td><code>external-svc</code></td>
 <td>將 <code>&lt;<em>external_service</em>&gt;</code> 取代為要呼叫的外部服務。例如 <code>https://&lt;myservice&gt;.&lt;region&gt;.mybluemix.net</code>。</td>
 </tr>
 <tr>
 <td><code>host</code></td>
 <td>將 <code>&lt;<em>mydomain</em>&gt;</code> 取代為外部服務的主機網域。</td>
 </tr>
 </tbody></table>

 </dd></dl>

<br />


### 位置修飾元 (location-modifier)
{: #location-modifier}

修改 ALB 比對要求 URI 與應用程式路徑的方式。
{:shortdesc}

<dl>
<dt>說明</dt>
<dd>依預設，ALB 會處理應用程式接聽的路徑（作為字首）。當 ALB 收到對應用程式的要求時，ALB 會檢查 Ingress 資源以尋找符合此要求 URI 開頭的路徑（作為字首）。如果找到相符項，要求會轉遞至應用程式部署所在 Pod 的 IP 位址。<br><br>`location-modifier` 註釋可藉由修改位置區塊配置來變更 ALB 搜尋相符項的方式。位置區塊決定如何處理對應用程式路徑的要求。<br><br><strong>附註</strong>：若要處理正規表示式 (regex) 路徑，則需要此註釋。</dd>

<dt>支援的修飾元</dt>
<dd>

<table>
<caption>支援的修飾元</caption>
 <col width="10%">
 <col width="90%">
 <thead>
 <th>修飾元</th>
 <th>說明</th>
 </thead>
 <tbody>
 <tr>
 <td><code>=</code></td>
 <td>等號修飾元讓 ALB 只選取完全相符項。找到完全相符項時，搜尋即會停止，並選取相符的路徑。<br>例如，如果您的應用程式在 <code>/tea</code> 上接聽，則在將要求與您的應用程式進行比對時，ALB 只會選取確切的 <code>/tea</code> 路徑。</td>
 </tr>
 <tr>
 <td><code>~</code></td>
 <td>波狀符號修飾元讓 ALB 在比對期間將路徑當作區分大小寫的 regex 路徑來處理。<br>例如，如果您的應用程式在 <code>/coffee</code> 上接聽，則在將要求與您的應用程式進行比對時，即使未針對您的應用程式明確地指定路徑，ALB 也可以選取 <code>/ab/coffee</code> 或 <code>/123/coffee</code> 路徑。</td>
 </tr>
 <tr>
 <td><code>~\*</code></td>
 <td>波狀符號後面接一個星號修飾元，讓 ALB 在比對期間將路徑當作不區分大小寫的 regex 路徑來處理。<br>例如，如果您的應用程式在 <code>/coffee</code> 上接聽，則在將要求與您的應用程式進行或時，即使未針對您的應用程式明確地指定路徑，ALB 也可以選取 <code>/ab/Coffee</code> 或 <code>/123/COFFEE</code> 路徑。</td>
 </tr>
 <tr>
 <td><code>^~</code></td>
 <td>^ 符號後面接一個波狀符號修飾元，讓 ALB 選取最佳的非 regex 比對，而非 regex 路徑。</td>
 </tr>
 </tbody>
</table>

</dd>

<dt>Ingress 資源範例 YAML</dt>
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
 <caption>瞭解註釋元件</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
  </thead>
  <tbody>
  <tr>
  <td><code>modifier</code></td>
  <td>將 <code>&lt;<em>location_modifier</em>&gt;</code> 取代為您要用於路徑的位置修飾元。支援的修飾元為 <code>'='</code>、<code>'~'</code>、<code>'~\*'</code> 及 <code>'^~'</code>。您必須以單引號括住這些修飾元。</td>
  </tr>
  <tr>
  <td><code>serviceName</code></td>
  <td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為您為應用程式所建立之 Kubernetes 服務的名稱。</td>
  </tr>
  </tbody></table>
  </dd>
  </dl>

<br />


### 專用 ALB 遞送 (ALB-ID)
{: #alb-id}

使用專用 ALB，將送入要求遞送至應用程式。
{:shortdesc}

<dl>
<dt>說明</dt>
<dd>
選擇專用 ALB 來遞送送入要求，而非公用 ALB。</dd>


<dt>Ingress 資源範例 YAML</dt>
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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>&lt;private_ALB_ID&gt;</code></td>
<td>專用 ALB 的 ID。若要尋找專用 ALB ID，請執行 <code>bx cs albs --cluster &lt;my_cluster&gt;</code>。
</td>
</tr>
</tbody></table>
</dd>
</dl>

<br />


### 重寫路徑 (rewrite-path)
{: #rewrite-path}

將 ALB 網域路徑上的送入網路資料流量，遞送至與後端應用程式所接聽路徑不同的路徑。
{:shortdesc}

<dl>
<dt>說明</dt>
<dd>Ingress ALB 網域會將 <code>mykubecluster.us-south.containers.appdomain.cloud/beans</code> 上的送入網路資料流量遞送至您的應用程式。您的應用程式會接聽 <code>/coffee</code>，而非 <code>/beans</code>。若要將送入的網路資料流量轉遞至您的應用程式，請將重寫註釋新增至您的 Ingress 資源配置檔。重寫註釋確保 <code>/beans</code> 上的送入網路資料流量會使用 <code>/coffee</code> 路徑轉遞至您的應用程式。包含多個服務時，只使用分號 (;) 來區隔它們。</dd>
<dt>Ingress 資源範例 YAML</dt>
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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為您為應用程式所建立之 Kubernetes 服務的名稱。</td>
</tr>
<tr>
<td><code>rewrite</code></td>
<td>將 <code>&lt;<em>target_path</em>&gt;</code> 取代為應用程式所接聽的路徑。ALB 網域上的送入網路資料流量會使用此路徑轉遞至 Kubernetes 服務。大部分的應用程式不會接聽特定路徑，但使用根路徑及特定埠。在上述範例中，重寫路徑定義為 <code>/coffee</code>。</td>
</tr>
</tbody></table>

</dd></dl>

<br />


### 應用程式負載平衡器的 TCP 埠 (tcp-ports)
{: #tcp-ports}

透過非標準 TCP 埠存取應用程式。
{:shortdesc}

<dl>
<dt>說明</dt>
<dd>
對正在執行 TCP 串流工作負載的應用程式使用此註釋。



<p>**附註**：ALB 係以透通模式運作，並將資料流量轉遞至後端應用程式。在此情況下，不支援 SSL 終止。</p>
</dd>


<dt>Ingress 資源範例 YAML</dt>
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
 <caption>瞭解註釋元件</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為您為要透過非標準 TCP 埠存取之 Kubernetes 服務的名稱。</td>
  </tr>
  <tr>
  <td><code>ingressPort</code></td>
  <td>將 <code>&lt;<em>ingress_port</em>&gt;</code> 取代為您要在其上存取應用程式的 TCP 埠。</td>
  </tr>
  <tr>
  <td><code>servicePort</code></td>
  <td>這是選用參數。若已提供，埠會替換為此值，然後再將資料流量傳送至後端應用程式。否則，埠會保持與 Ingress 埠相同。</td>
  </tr>
  </tbody></table>

 </dd>
 <dt>用法</dt>
 <dd><ol><li>檢閱 ALB 的開啟埠。
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
CLI 輸出會與下列內容類似：
<pre class="screen">
<code>NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx 80:30416/TCP,443:32668/TCP   109d</code></pre></li>
<li>開啟 ALB 配置對映。
<pre class="pre">
<code>kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system</code></pre></li>
<li>將 TCP 埠新增至配置對映。將 <code>&lt;port&gt;</code> 取代為您要開啟的 TCP 埠。
<b>附註</b>：依預設，會開啟埠 80 及 443。如果您要將 80 及 443 保留為開啟狀態，則除了您在 `public-ports` 欄位中指定的任何其他 TCP 埠之外，還必須包括它們。如果已啟用專用 ALB，則也必須在 `private-ports` 欄位中指定您要保留開啟狀態的任何埠。如需相關資訊，請參閱<a href="cs_ingress.html#opening_ingress_ports">在 Ingress ALB 中開啟埠</a>。
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
 <li>驗證已使用 TCP 埠重新配置 ALB。
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
CLI 輸出會與下列內容類似：
<pre class="screen">
<code>NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx  169.xx.xxx.xxx &lt;port1&gt;:30776/TCP,&lt;port2&gt;:30412/TCP   109d</code></pre></li>
<li>配置 Ingress 透過非標準 TCP 埠來存取您的應用程式。請在此參照中使用範例 YAML。</li>
<li>更新 ALB 配置。
<pre class="pre">
<code>        kubectl apply -f myingress.yaml
        </code></pre>
</li>
<li>開啟偏好的 Web 瀏覽器以存取您的應用程式。範例：<code>https://&lt;ibmdomain&gt;:&lt;ingressPort&gt;/</code></li></ol></dd></dl>

<br />


## 連線註釋
{: #connection}

### 自訂連接逾時及讀取逾時（proxy-connect-timeout、proxy-read-timeout）
{: #proxy-connect-timeout}

設定 ALB 等待連接及讀取後端應用程式的時間，在此時間之後，後端應用程式將被視為無法使用。
{:shortdesc}

<dl>
<dt>說明</dt>
<dd>當用戶端要求傳送至 Ingress ALB 時，ALB 會開啟與後端應用程式的連線。依預設，ALB 會等待 60 秒，以接收來自後端應用程式的回覆。如果後端應用程式未在 60 秒內回覆，則會中斷連線要求，且後端應用程式將被視為無法使用。



</br></br>
在 ALB 連接至後端應用程式之後，ALB 會讀取來自後端應用程式的回應資料。在此讀取作業期間，ALB 在兩次讀取作業之間最多等待 60 秒，以接收來自後端應用程式的資料。如果後端應用程式未在 60 秒內傳送資料，則會關閉與後端應用程式的連線，且應用程式將被視為無法使用。
</br></br>
60 秒的連接逾時及讀取逾時是 Proxy 上的預設逾時，通常不應該加以變更。
</br></br>
如果應用程式的可用性不穩定，或是您的應用程式因為高工作負載而回應太慢，您或許會想增加連接逾時或讀取逾時。請記住，增加逾時值會影響 ALB 的效能，因為與後端應用程式的連線必須維持開啟，直到達到逾時值為止。
</br></br>
另一方面，您可以減少逾時值，以提高 ALB 的效能。請確保您的後端應用程式能夠在指定的逾時內處理要求，即使是在較高工作負載的時段。</dd>
<dt>Ingress 資源範例 YAML</dt>
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
<caption>瞭解註釋元件</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;connect_timeout&gt;</code></td>
 <td>要等待連接至後端應用程式的秒數或分鐘數，例如 <code>65s</code> 或 <code>2m</code>。<strong>附註：</strong>連接逾時不可超過 75 秒。</td>
 </tr>
 <tr>
 <td><code>&lt;read_timeout&gt;</code></td>
 <td>要在讀取後端應用程式之前等待的秒數或分鐘數，例如 <code>65s</code> 或 <code>2m</code>。
 </tr>
 </tbody></table>

 </dd></dl>

<br />



### 保留作用中要求 (keepalive-requests)
{: #keepalive-requests}

設定可以透過一個保留作用中連線提供的要求數目上限。
{:shortdesc}

<dl>
<dt>說明</dt>
<dd>
 設定可以透過一個保留作用中連線提供的要求數目上限。
 </dd>


<dt>Ingress 資源範例 YAML</dt>
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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為您為應用程式所建立之 Kubernetes 服務的名稱。這是選用參數。除非已指定服務，否則配置會套用至 Ingress 主機中的所有服務。如果提供參數，則會針對給定的服務設定保留作用中要求。如果未提供參數，則會針對未配置保留作用中要求的所有服務，在 <code>nginx.conf</code> 的伺服器層次設定保留作用中要求。</td>
</tr>
<tr>
<td><code>requests</code></td>
<td>將 <code>&lt;<em>max_requests</em>&gt;</code> 取代為可以透過一個保留作用中連線提供的要求數目上限。</td>
</tr>
</tbody></table>

</dd>
</dl>

<br />



### 保留作用中逾時（keepalive-timeout）
{: #keepalive-timeout}

設定保留作用中連線在伺服器端上保持開啟的時間上限。
{:shortdesc}

<dl>
<dt>說明</dt>
<dd>
設定保留作用中連線在伺服器上保持開啟的時間上限。
</dd>


<dt>Ingress 資源範例 YAML</dt>
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
<caption>瞭解註釋元件</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
 </thead>
 <tbody>
 <tr>
 <td><code>serviceName</code></td>
 <td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為您為應用程式所建立之 Kubernetes 服務的名稱。這是選用參數。如果提供參數，則會針對給定的服務設定保留作用中逾時。如果未提供參數，則會針對未配置保留作用中逾時的所有服務，在 <code>nginx.conf</code> 的伺服器層次設定保留作用中逾時。</td>
 </tr>
 <tr>
 <td><code>timeout</code></td>
 <td>將 <code>&lt;<em>time</em>&gt;</code> 取代為時間量（以秒為單位）。範例：<code>timeout=20s</code>。零值會停用保留作用中用戶端連線。</td>
 </tr>
 </tbody></table>

 </dd>
 </dl>

<br />


### Proxy 下一個上游 (proxy-next-upstream-config)
{: #proxy-next-upstream-config}

設定 ALB 可以將要求傳遞給下一個上游伺服器的時間。
{:shortdesc}

<dl>
<dt>說明</dt>
<dd>
Ingress ALB 會充當用戶端應用程式與您的應用程式之間的 Proxy。部分應用程式設定需要多個上游伺服器，從 ALB 處理送入的用戶端要求。有時，ALB 所使用的 Proxy 伺服器無法與應用程式所使用的上游伺服器建立連線。然後，ALB 可以嘗試與下一個上游伺服器建立連線，以改為將要求傳遞給它。您可以使用 `proxy-next-upstream-config` 註釋，來設定 ALB 可在哪些情況、多久時間及多少次嘗試將要求傳遞給下一個上游伺服器。<br><br><strong>附註</strong>：當您使用 `proxy-next-upstream-config` 時，一律會配置逾時，因此不要將 `timeout=true` 新增至此註釋。</dd>
<dt>Ingress 資源範例 YAML</dt>
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
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為您為應用程式所建立之 Kubernetes 服務的名稱。</td>
</tr>
<tr>
<td><code>retries</code></td>
<td>將 <code>&lt;<em>tries</em>&gt;</code> 取代為 ALB 嘗試將要求傳遞給下一個上游伺服器的次數上限。此數字包含原始要求。若要關閉此限制，請使用 <code>0</code>。如果未指定一值，則會使用預設值 <code>0</code>。</td>
</tr>
<tr>
<td><code>timeout</code></td>
<td>將 <code>&lt;<em>time</em>&gt;</code> 取代為 ALB 嘗試將要求傳遞給下一個上游伺服器的時間量上限（以秒為單位）。例如，若要設定 30 秒的時間，請輸入 <code>30s</code>。若要關閉此限制，請使用 <code>0</code>。如果未指定一值，則會使用預設值 <code>0</code>。</td>
</tr>
<tr>
<td><code>error</code></td>
<td>如果設為 <code>true</code>，則在與第一個上游伺服器建立連線、將要求傳遞至其中，或讀取回應標頭時，若發生錯誤，ALB 會將要求傳遞給下一個上游伺服器。
</td>
</tr>
<tr>
<td><code>invalid_header</code></td>
<td>如果設為 <code>true</code>，則當第一個上游伺服器傳回空的或無效的回應時， ALB 會將要求傳遞給下一個上游伺服器。</td>
</tr>
<tr>
<td><code>http_502</code></td>
<td>如果設為 <code>true</code>，則當第一個上游伺服器傳回代碼為 502 的回應時， ALB 會將要求傳遞給下一個上游伺服器。您可以指定下列 HTTP 回應碼：<code>500</code>、<code>502</code>、<code>503</code>、<code>504</code>、<code>403</code>、<code>404</code>、<code>429</code>。</td>
</tr>
<tr>
<td><code>non_idempotent</code></td>
<td>如果設為 <code>true</code>，則 ALB 可以將具有非冪等方法的要求傳遞給下一個上游伺服器。依預設，ALB 不會將這些要求傳遞給下一個上游伺服器。</td>
</tr>
<tr>
<td><code>off</code></td>
<td>若要防止 ALB 將要求傳遞給下一個上游伺服器，請設為 <code>true</code>。
</td>
</tr>
</tbody></table>
</dd>
</dl>

<br />


### 使用 Cookie 的階段作業親緣性 (sticky-cookie-services)
{: #sticky-cookie-services}

使用 Sticky Cookie 註釋，可為 ALB 增加階段作業親緣性，並一律將送入的網路資料流量遞送至相同的上游伺服器。
{:shortdesc}

<dl>
<dt>說明</dt>
<dd>針當用戶端連接至您的後端應用程式時，您可以使用階段作業親緣性，以在階段作業時間，或是在完成作業所需的時間內，由相同的上游伺服器服務某一用戶端。您可以配置 ALB，一律將送入的網路資料流量遞送至相同的上游伺服器，以確保階段作業親緣性。

</br></br>
每一個連接至後端應用程式的用戶端都會由 ALB 指派給其中一台可用的上游伺服器。ALB 會建立一個儲存在用戶端應用程式中的階段作業 Cookie，該 Cookie 內含在 ALB 與用戶端之間每個要求的標頭資訊中。Cookie 中的資訊確保所有要求在整個階段作業中，都由相同的上游伺服器處理。



</br></br>
當您包含多個服務時，請使用分號 (;) 來區隔它們。</dd>
<dt>Ingress 資源範例 YAML</dt>
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
  <caption>瞭解註釋元件</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為您為應用程式所建立之 Kubernetes 服務的名稱。</td>
  </tr>
  <tr>
  <td><code>name</code></td>
  <td>將 <code>&lt;<em>cookie_name</em>&gt;</code> 取代為在階段作業期間建立的 Sticky Cookie 名稱。</td>
  </tr>
  <tr>
  <td><code>expires</code></td>
  <td>將 <code>&lt;<em>expiration_time</em>&gt;</code> 取代為 Sticky Cookie 到期之前的時間，以秒 (s)、分鐘 (m) 或小時 (h) 為單位。此時間與使用者活動無關。Cookie 過期之後，用戶端 Web 瀏覽器就會刪除 Cookie，且不會再傳送至 ALB。例如，若要設定 1 秒、1 分鐘或 1 小時的有效期限，請輸入 <code>1s</code>、<code>1m</code> 或 <code>1h</code>。</td>
  </tr>
  <tr>
  <td><code>path</code></td>
  <td>將 <code>&lt;<em>cookie_path</em>&gt;</code> 取代為附加至 Ingress 子網域的路徑，且該路徑指出針對哪些網域和子網域而將 Cookie 傳送至 ALB。例如，如果您的 Ingress 網域是 <code>www.myingress.com</code> 且想要在每個用戶端要求中傳送 Cookie，您必須設定 <code>path=/</code>。如果您只想針對 <code>www.myingress.com/myapp</code> 及其所有子網域傳送 Cookie，則必須設定 <code>path=/myapp</code>。</td>
  </tr>
  <tr>
  <td><code>hash</code></td>
  <td>將 <code>&lt;<em>hash_algorithm</em>&gt;</code> 取代為保護 Cookie 中資訊的雜湊演算法。僅支援 <code>sha1</code>。SHA1 會根據 Cookie 中的資訊建立雜湊總和，並將此雜湊總和附加到 Cookie。伺服器可以解密 Cookie 中的資訊，然後驗證資料完整性。</td>
  </tr>
  </tbody></table>

 </dd></dl>

<br />




### 上游保留作用中 (upstream-keepalive)
{: #upstream-keepalive}

設定上游伺服器之閒置保留作用中連線的數目上限。
{:shortdesc}

<dl>
<dt>說明</dt>
<dd>
設定所給定服務的上游伺服器的閒置保留作用中連線數目上限。依預設，上游伺服器具有 64 個閒置保留作用中連線。
</dd>


 <dt>Ingress 資源範例 YAML</dt>
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
 <caption>瞭解註釋元件</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為您為應用程式所建立之 Kubernetes 服務的名稱。</td>
  </tr>
  <tr>
  <td><code>keepalive</code></td>
  <td>將 <code>&lt;<em>max_connections</em>&gt;</code> 取代為上游伺服器之閒置保留作用中連線的數目上限。預設值為 <code>64</code>。<code>0</code> 值會停用給定服務的上游保留作用中連線。</td>
  </tr>
  </tbody></table>
  </dd>
  </dl>

<br />




## HTTPS 及 TLS/SSL 鑑別註釋
{: #https-auth}

### {{site.data.keyword.appid_short_notm}} 鑑別 (appid-auth)
{: #appid-auth}

  使用 {{site.data.keyword.appid_full_notm}} 對您的應用程式進行鑑別。
  {:shortdesc}

  <dl>
  <dt>說明</dt>
  <dd>
使用 {{site.data.keyword.appid_short_notm}} 鑑別 Web 或 API HTTP/HTTPS 要求。

  <p>如果您將要求類型設為 <code>web</code>，則會驗證包含 {{site.data.keyword.appid_short_notm}} 存取記號的 Web 要求。如果記號驗證失敗，則會拒絕 Web 要求。如果要求不包含存取記號，則會將該要求重新導向至 {{site.data.keyword.appid_short_notm}} 登入頁面。<strong>附註</strong>：若要讓 {{site.data.keyword.appid_short_notm}} Web 鑑別能夠運作，必須在使用者的瀏覽器中啟用 Cookie。</p>

  <p>如果您將要求類型設為 <code>api</code>，則會驗證包含 {{site.data.keyword.appid_short_notm}} 存取記號的 API 要求。如果要求不包含存取記號，則會向使用者傳回 <code>401: Unauthorized</code> 錯誤訊息。</p>

  <p>**附註**：基於安全理由，{{site.data.keyword.appid_short_notm}} 鑑別僅支援已啟用 TLS/SSL 的後端。</p>
  </dd>
   <dt>Ingress 資源範例 YAML</dt>
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
   <caption>瞭解註釋元件</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
    </thead>
    <tbody>
    <tr>
    <td><code>bindSecret</code></td>
    <td>將 <em><code>&lt;bind_secret&gt;</code></em> 取代為儲存連結密碼的 Kubernetes 密碼。</td>
    </tr>
    <tr>
    <td><code>namespace</code></td>
    <td>將 <em><code>&lt;namespace&gt;</code></em> 取代為連結密碼的名稱空間。這個欄位預設為 `default` 名稱空間。</td>
    </tr>
    <tr>
    <td><code>requestType</code></td>
    <td>將 <code><em>&lt;request_type&gt;</em></code> 取代為您要傳送至 {{site.data.keyword.appid_short_notm}} 的要求類型。接受的值包含 `web` 或 `api`。預設值為 `api`。</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td>將 <code><em>&lt;myservice&gt;</em></code> 取代為您為應用程式所建立之 Kubernetes 服務的名稱。這是選用欄位。如果不包括服務名稱，則會啟用所有服務的註釋。如果包括服務名稱，則只會啟用該服務的註釋。以分號 (;) 區隔多項服務。</td>
    </tr>
    </tbody></table>
    </dd>
    <dt>用法</dt>
    <dd>因為應用程式使用 {{site.data.keyword.appid_short_notm}} 進行鑑別，所以您必須佈建 {{site.data.keyword.appid_short_notm}} 實例，以有效的重新導向 URI 配置該實例，並產生連結密碼。
    <ol>
    <li>佈建 [{{site.data.keyword.appid_short_notm}} 實例](https://console.bluemix.net/catalog/services/app-id)。</li>
    <li>在 {{site.data.keyword.appid_short_notm}} 管理主控台中，新增應用程式的 redirectURI。</li>
    <li>建立連結密碼。
    <pre class="pre"><code>bx cs cluster-service-bind &lt;my_cluster&gt; &lt;my_namespace&gt; &lt;my_service_instance_GUID&gt;</code></pre> </li>
    <li>配置 <code>appid-auth</code> 註釋。</li>
    </ol></dd>
    </dl>

<br />



### 自訂 HTTP 及 HTTPS 埠 (custom-port)
{: #custom-port}

變更 HTTP 及 HTTPS 網路資料流量的預設埠，HTTP 為埠 80，HTTPS 為埠 443。
{:shortdesc}

<dl>
<dt>說明</dt>
<dd>依預設，Ingress ALB 是配置為在埠 80 接聽送入的 HTTP 網路資料流量，並在埠 443 接聽送入的 HTTPS 網路資料流量。您可以變更預設埠來增加 ALB 網域的安全，或只啟用 HTTPS 埠。
</dd>


<dt>Ingress 資源範例 YAML</dt>
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
<caption>瞭解註釋元件</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;protocol&gt;</code></td>
 <td>輸入 <code>http</code> 或 <code>https</code> 來變更送入之 HTTP 或 HTTPS 網路資料流量的預設埠。</td>
 </tr>
 <tr>
 <td><code>&lt;port&gt;</code></td>
 <td>輸入要用於送入之 HTTP 或 HTTPS 網路資料流量的埠號。<p><strong>附註：</strong>當為 HTTP 或 HTTPS 指定自訂埠時，預設埠對於 HTTP 和 HTTPS 便不再有效。例如，若要將 HTTPS 的預設埠變更為 8443，但 HTTP 使用預設埠，您必須兩者都設定自訂埠：<code>custom-port: "protocol=http port=80; protocol=https port=8443"</code>。</p></td>
 </tr>
 </tbody></table>

 </dd>
 <dt>用法</dt>
 <dd><ol><li>檢閱 ALB 的開啟埠。
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
CLI 輸出會與下列內容類似：
<pre class="screen">
<code>NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx 80:30416/TCP,443:32668/TCP   109d</code></pre></li>
<li>開啟 ALB 配置對映。
<pre class="pre">
<code>kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system</code></pre></li>
<li>將非預設的 HTTP 及 HTTPS 埠新增至配置對映。將 &lt;port&gt; 取代為您要開啟的 HTTP 或 HTTPS 埠。
<b>附註</b>：依預設，會開啟埠 80 及 443。如果您要將 80 及 443 保留為開啟狀態，則除了您在 `public-ports` 欄位中指定的任何其他 TCP 埠之外，還必須包括它們。如果已啟用專用 ALB，則也必須在 `private-ports` 欄位中指定您要保留開啟狀態的任何埠。如需相關資訊，請參閱<a href="cs_ingress.html#opening_ingress_ports">在 Ingress ALB 中開啟埠</a>。
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
 <li>驗證已使用非預設埠重新配置 ALB。
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
CLI 輸出會與下列內容類似：
<pre class="screen">
<code>NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx  169.xx.xxx.xxx &lt;port1&gt;:30776/TCP,&lt;port2&gt;:30412/TCP   109d</code></pre></li>
<li>配置您的 Ingress，以在將送入的網路資料流量遞送至服務時使用非預設埠。請在此參照中使用範例 YAML。</li>
<li>更新 ALB 配置。
<pre class="pre">
<code>        kubectl apply -f myingress.yaml
        </code></pre>
</li>
<li>開啟偏好的 Web 瀏覽器以存取您的應用程式。範例：<code>https://&lt;ibmdomain&gt;:&lt;port&gt;/&lt;service_path&gt;/</code></li></ol></dd></dl>

<br />



### HTTP 重新導向至 HTTPS (redirect-to-https)
{: #redirect-to-https}

將不安全的 HTTP 用戶端要求轉換成 HTTPS。
 {:shortdesc}

<dl>
<dt>說明</dt>
<dd>請設定 Ingress ALB，以 IBM 提供的 TLS 憑證或您的自訂 TLS 憑證來保護網域安全。部分使用者可能會嘗試對 ALB 網域使用不安全的 <code>http</code> 要求（例如 <code>http://www.myingress.com</code>）來存取您的應用程式，而非使用 <code>https</code>。您可以使用重新導向註釋，一律將不安全的 HTTP 用戶端要求重新導向至 HTTPS。如果您未使用此註釋，依預設，不安全的 HTTP 要求不會轉換成 HTTPS 要求，且可能會將未加密的機密資訊公開給大眾使用。

</br></br>
依預設，會停用將 HTTP 要求重新導向至 HTTPS。</dd>

<dt>Ingress 資源範例 YAML</dt>
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


### HTTP 強制安全傳輸技術 (hsts)
{: #hsts}

<dl>
<dt>說明</dt>
<dd>
HSTS 指示瀏覽器僅使用 HTTPS 來存取網域。即使使用者輸入或遵循一般 HTTP 鏈結，瀏覽器也會強制升級至 HTTPS 的連線。
</dd>


<dt>Ingress 資源範例 YAML</dt>
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
<caption>瞭解註釋元件</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
  </thead>
  <tbody>
  <tr>
  <td><code>enabled</code></td>
  <td>使用 <code>true</code> 來啟用 HSTS。</td>
  </tr>
    <tr>
  <td><code>maxAge</code></td>
  <td>將 <code>&lt;<em>31536000</em>&gt;</code> 取代為整數，代表瀏覽器將傳送中要求直接快取至 HTTPS 的秒數。預設值為 <code>31536000</code>，相當於 1 年。</td>
  </tr>
  <tr>
  <td><code>includeSubdomains</code></td>
  <td>使用 <code>true</code> 來告知瀏覽器，HSTS 原則也適用於現行網域的所有子網域。預設值為 <code>true</code>。</td>
  </tr>
  </tbody></table>

  </dd>
  </dl>


<br />


### 交互鑑別 (mutual-auth)
{: #mutual-auth}

配置 ALB 的交互鑑別。
{:shortdesc}

<dl>
<dt>說明</dt>
<dd>
針對 Ingress ALB 配置下游資料流量的交互鑑別。外部用戶端會鑑別伺服器，而伺服器也會使用憑證來鑑別用戶端。交互鑑別也稱為憑證型鑑別或雙向鑑別。
 </dd>

<dt>必要條件</dt>
<dd>
<ul>
<li>[您必須具有包含必要憑證管理中心 (CA) 的有效密碼](cs_app.html#secrets)。同時需要 <code>client.key</code> 和 <code>client.crt</code> 來進行交互鑑別。</li>
<li>若要在 443 以外的埠上啟用交互鑑別，請[配置 ALB 以開啟有效的埠](cs_ingress.html#opening_ingress_ports)。</li>
</ul>
</dd>


<dt>Ingress 資源範例 YAML</dt>
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
          servicePort: 8080
          </code></pre>

<table>
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>secretName</code></td>
<td>將 <code>&lt;<em>mysecret</em>&gt;</code> 取代為密碼資源的名稱。</td>
</tr>
<tr>
<td><code>port</code></td>
<td>將 <code>&lt;<em>port</em>&gt;</code> 取代為 ALB 埠號。</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>將 <code>&lt;<em>servicename</em>&gt;</code> 取代為一個以上 Ingress 資源的名稱。這是選用參數。</td>
</tr>
</tbody></table>

</dd>
</dl>

<br />



### SSL 服務支援 (ssl-services)
{: #ssl-services}

容許 HTTPS 要求並加密要送至上游應用程式的資料流量。
{:shortdesc}

<dl>
<dt>說明</dt>
<dd>
加密 Ingress 傳送至需要 HTTPS 的上游應用程式的資料流量。如果您的上游應用程式可以處理 TLS，則您可以選擇性地提供 TLS 密碼中包含的憑證。<br></br>**選用**：您可以將[單向鑑別或交互鑑別](#ssl-services-auth)新增至此註釋。</dd>


<dt>Ingress 資源範例 YAML</dt>
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
<caption>瞭解註釋元件</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
  </thead>
  <tbody>
  <tr>
  <td><code>ssl-service</code></td>
  <td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為需要 HTTPS 的服務名稱。系統會加密從 ALB 到此應用程式之服務的資料流量。</td>
  </tr>
  <tr>
  <td><code>ssl-secret</code></td>
  <td>選用項目：如果您要使用 TLS 密碼，且您的上游應用程式可以處理 TLS，請將 <code>&lt;<em>service-ssl-secret</em>&gt;</code> 取代為服務的密碼。如果您提供密碼，則此值必須包含您的應用程式預期來自用戶端的 <code>trusted.crt</code>、<code>client.crt</code> 及 <code>client.key</code>。若要建立 TLS 密碼，請先[將憑證及金鑰轉換為 base-64 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.base64encode.org/)。然後，請參閱[建立密碼](cs_app.html#secrets)。</td>
  </tr>
  </tbody></table>

  </dd>
</dl>

<br />


#### SSL 服務支援與鑑別
{: #ssl-services-auth}

<dl>
<dt>說明</dt>
<dd>
容許 HTTPS 要求，並使用單向或交互鑑別，加密要送至上游應用程式的資料流量，以取得額外的安全。
</dd>


<dt>Ingress 資源範例 YAML</dt>
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
<caption>瞭解註釋元件</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
  </thead>
  <tbody>
  <tr>
  <td><code>ssl-service</code></td>
  <td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為需要 HTTPS 的服務名稱。系統會加密從 ALB 到此應用程式之服務的資料流量。</td>
  </tr>
  <tr>
  <td><code>ssl-secret</code></td>
  <td>將 <code>&lt;<em>service-ssl-secret</em>&gt;</code> 取代為服務的交互鑑別密碼。此值必須包含您的應用程式預期來自用戶端的 CA 憑證。若要建立交互鑑別密碼，請先[將憑證及金鑰轉換為 base-64 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.base64encode.org/)。然後，請參閱[建立密碼](cs_app.html#secrets)。</td>
  </tr>
  </tbody></table>

  </dd>
  </dl>

<br />


## Istio 註釋
{: #istio-annotations}

### Istio 服務（istio-services）
{: #istio-services}

  將資料流量遞送至 Istio 受管理服務。
  {:shortdesc}

  <dl>
  <dt>說明</dt>
  <dd>
  如果您具有 Istio 受管理服務，則可以使用叢集 ALB，將 HTTP/HTTPS 要求遞送至 Istio Ingress 控制器。Istio Ingress 控制器接著會將要求遞送至應用程式服務。為了遞送資料流量，您必須對叢集 ALB 及 Istio Ingress 控制器兩者的 Ingress 資源進行變更。<br><br>在叢集 ALB 的 Ingress 資源中，您必須：
      <ul>
        <li>指定 `istio-services` 註釋</li>
        <li>將服務路徑定義為應用程式接聽所在的實際路徑</li>
        <li>將服務埠定義為 Istio Ingress 控制器的埠</li>
      </ul>
    <br>在 Istio Ingress 控制器的 Ingress 資源中，您必須：
      <ul>
        <li>將服務路徑定義為應用程式接聽所在的實際路徑</li>
        <li>將服務埠定義為 Istio Ingress 控制器所公開之應用程式服務的 HTTP/HTTPS 埠</li>
    </ul>
  </dd>

   <dt>叢集 ALB 的範例 Ingress 資源 YAML</dt>
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
   <caption>瞭解 YAML 檔案元件</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解 YAML 檔案元件</th>
    </thead>
    <tbody>
    <tr>
    <td><code>enabled</code></td>
      <td>若要讓資料流量可以遞送至 Istio 管理服務，請設為 <code>True</code>。</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td>將 <code><em>&lt;myservice1&gt;</em></code> 取代為您為 Istio 受管理應用程式所建立之 Kubernetes 服務的名稱。以分號 (;) 區隔多項服務。這是選用欄位。如果未指定服務名稱，則會啟用所有 Istio 受管理服務進行資料流量遞送。</td>
    </tr>
    <tr>
    <td><code>istioServiceNamespace</code></td>
    <td>將 <code><em>&lt;istio-namespace&gt;</em></code> 取代為已安裝 Istio 的 Kubernees 名稱空間。這是選用欄位。如果未指定名稱空間，則會使用 <code>istio-system</code> 名稱空間。</td>
    </tr>
    <tr>
    <td><code>istioServiceName</code></td>
    <td>將 <code><em>&lt;istio-ingress-service&gt;</em></code> 取代為 Istio Ingress 服務的名稱。這是選用欄位。如果未指定 Itio Ingress 服務名稱，則會使用服務名稱 <code>istio-ingress</code>。</td>
    </tr>
    <tr>
    <td><code>path</code></td>
      <td>對於每一個您要將資料流量遞送至其中的 Istio 受管理服務，請將 <code><em>&lt;/myapp1&gt;</em></code> 取代為 Istio 受管理服務接聽所在的後端路徑。此路徑必須對應於您已在 Istio Ingress 資源中定義的路徑。</td>
    </tr>
    <tr>
    <td><code>servicePort</code></td>
    <td>對於每一個您要將資料流量遞送至其中的 Istio 受管理服務，請將 <code><em>&lt;istio_ingress_port&gt;</em></code> 取代為 Istio Ingress 控制器的埠。</td>
    </tr>
    </tbody></table>
    </dd>
    </dl>

## Proxy 緩衝區註釋
{: #proxy-buffer}


### 用戶端回應資料緩衝 (proxy-buffering)
{: #proxy-buffering}

使用緩衝區註釋，可以在資料傳送至用戶端時，停用在 ALB 上儲存回應資料的功能。
{:shortdesc}

<dl>
<dt>說明</dt>
<dd>Ingress ALB 會充當後端應用程式與用戶端 Web 瀏覽器之間的 Proxy。當回應從後端應用程式傳送至用戶端時，依預設，回應資料會在 ALB 上進行緩衝處理。ALB 會代理用戶端回應，並開始按照用戶端的速度將回應傳送至用戶端。ALB 收到來自後端應用程式的所有資料之後，會關閉與後端應用程式的連線。從 ALB 到用戶端的連線會維持開啟，直到用戶端收到所有資料為止。

</br></br>
如果停用在 ALB 上緩衝回應資料的功能，資料會立即從 ALB 傳送至用戶端。用戶端必須能夠按照 ALB 的速度處理送入的資料。如果用戶端太慢，資料可能會遺失。
</br></br>
依預設，會啟用 ALB 上的回應資料緩衝功能。</dd>
<dt>Ingress 資源範例 YAML</dt>
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
<caption>瞭解註釋元件</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
 </thead>
 <tbody>
 <tr>
 <td><code>enabled</code></td>
   <td>若要在 ALB 上停用回應資料緩衝，請設為 <code>false</code>。</td>
 </tr>
 <tr>
 <td><code>serviceName</code></td>
 <td>將 <code><em>&lt;myservice1&gt;</em></code> 取代為您為應用程式所建立之 Kubernetes 服務的名稱。以分號 (;) 區隔多項服務。這是選用欄位。如果您未指定服務名稱，則所有服務都會使用此註釋。</td>
 </tr>
 </tbody></table>
 </dd>
 </dl>

<br />



### Proxy 緩衝區 (proxy-buffers)
{: #proxy-buffers}

配置 ALB 的 Proxy 緩衝區數目及大小。
{:shortdesc}

<dl>
<dt>說明</dt>
<dd>
針對來自 Proxy 伺服器的單一連線，設定讀取回應的緩衝區大小及數目。除非已指定服務，否則配置會套用至 Ingress 主機中的所有服務。例如，如果指定 <code>serviceName=SERVICE number=2 size=1k</code> 之類的配置，則會將 1k 套用至服務。如果指定 <code>number=2 size=1k</code> 之類的配置，則會將 1k 套用至 Ingress 主機中的所有服務。
</dd>
<dt>Ingress 資源範例 YAML</dt>
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
<caption>瞭解註釋元件</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
 </thead>
 <tbody>
 <tr>
 <td><code>serviceName</code></td>
 <td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為要套用 proxy-buffers 的服務名稱。</td>
 </tr>
 <tr>
 <td><code>number</code></td>
 <td>將 <code>&lt;<em>number_of_buffers</em>&gt;</code> 取代為數字，例如 <em>2</em>。</td>
 </tr>
 <tr>
 <td><code>size</code></td>
 <td>將 <code>&lt;<em>size</em>&gt;</code> 取代為每一個緩衝區的大小，並以千位元組（k 或 K）為單位，例如 <em>1K</em>。</td>
 </tr>
 </tbody>
 </table>
 </dd></dl>

<br />


### Proxy 緩衝區大小 (proxy-buffer-size)
{: #proxy-buffer-size}

配置從回應的第一部分讀取的 Proxy 緩衝區大小。
{:shortdesc}

<dl>
<dt>說明</dt>
<dd>
設定緩衝區大小，以讀取從 Proxy 伺服器收到之回應的第一部分。此部分的回應通常包含小型回應標頭。除非已指定服務，否則配置會套用至 Ingress 主機中的所有服務。例如，如果指定 <code>serviceName=SERVICE size=1k</code> 之類的配置，則會將 1k 套用至服務。如果指定 <code>size=1k</code> 之類的配置，則會將 1k 套用至 Ingress 主機中的所有服務。
 </dd>


<dt>Ingress 資源範例 YAML</dt>
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
<caption>瞭解註釋元件</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
 </thead>
 <tbody>
 <tr>
 <td><code>serviceName</code></td>
 <td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為要套用 proxy-buffer-size 的服務名稱。</td>
 </tr>
 <tr>
 <td><code>size</code></td>
 <td>將 <code>&lt;<em>size</em>&gt;</code> 取代為每一個緩衝區的大小，並以千位元組（k 或 K）為單位，例如 <em>1K</em>。</td>
 </tr>
 </tbody></table>

 </dd>
 </dl>

<br />



### Proxy 工作中緩衝區大小 (proxy-busy-buffers-size)
{: #proxy-busy-buffers-size}

配置可以在忙碌中的 Proxy 緩衝區大小。
{:shortdesc}

<dl>
<dt>說明</dt>
<dd>
限制在尚未完全讀取回應時，正在將回應傳送至用戶端的任何緩衝區大小。同時，其餘的緩衝區可讀取回應，並在必要時，將回應的一部分緩衝至暫存檔。除非已指定服務，否則配置會套用至 Ingress 主機中的所有服務。例如，如果指定 <code>serviceName=SERVICE size=1k</code> 之類的配置，則會將 1k 套用至服務。如果指定 <code>size=1k</code> 之類的配置，則會將 1k 套用至 Ingress 主機中的所有服務。
 </dd>


<dt>Ingress 資源範例 YAML</dt>
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
         servicePort: 8080
         </code></pre>

<table>
<caption>瞭解註釋元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為要套用 proxy-busy-buffers-size 的服務名稱。</td>
</tr>
<tr>
<td><code>size</code></td>
<td>將 <code>&lt;<em>size</em>&gt;</code> 取代為每一個緩衝區的大小，並以千位元組（k 或 K）為單位，例如 <em>1K</em>。</td>
</tr>
</tbody></table>

 </dd>
 </dl>

<br />



## 要求及回應註釋
{: #request-response}


### 其他用戶端要求或回應標頭（proxy-add-headers、response-add-headers）
{: #proxy-add-headers}

將額外的標頭資訊新增至用戶端要求，然後再將要求傳送至後端應用程式，或新增至用戶端回應，然後再將回應傳送至用戶端。
{:shortdesc}

<dl>
<dt>說明</dt>
<dd>Ingress ALB 會充當用戶端應用程式與後端應用程式之間的 Proxy。傳送至 ALB 的用戶端要求會經過處理（代理），然後放入新的要求，再傳送到您的後端應用程式。同樣地，傳送至 ALB 的後端應用程式回應會經過處理（代理），然後放入新的要求，再傳送到用戶端。代理 (Proxy) 要求或回應會移除一開始從用戶端或後端應用程式傳送的 HTTP 標頭資訊，例如使用者名稱。

<br><br>
如果您的後端應用程式需要 HTTP 標頭資訊，在 ALB 將用戶端要求轉遞至後端應用程式之前，您可以使用 <code>proxy-add-headers</code> 註釋將標頭資訊新增至該用戶端要求。

<br>
<ul><li>例如，您可能需要在要求轉遞至應用程式之前，將下列 X-Forward 標頭資訊新增至該要求：

<pre class="screen">
<code>proxy_set_header Host $host;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;</code></pre>

</li>

<li>若要將 X-Forward 標頭資訊新增至要傳送至應用程式的要求，請以下列方式使用 `proxy-add-headers` 註釋：

<pre class="screen">
<code>ingress.bluemix.net/proxy-add-headers: |
serviceName=<myservice1> {
  Host $host;
  X-Forwarded-Proto $scheme;
  X-Forwarded-For $proxy_add_x_forwarded_for;
  }</code></pre>

</li></ul><br>

如果用戶端 Web 應用程式需要 HTTP 標頭資訊，在 ALB 將回應轉遞至用戶端 Web 應用程式之前，您可以使用 <code>response-add-headers</code> 註釋將標頭資訊新增至該回應。</dd>

<dt>Ingress 資源範例 YAML</dt>
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
 <caption>瞭解註釋元件</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
  </thead>
  <tbody>
  <tr>
  <td><code>service_name</code></td>
  <td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為您為應用程式所建立之 Kubernetes 服務的名稱。</td>
  </tr>
  <tr>
  <td><code>&lt;header&gt;</code></td>
  <td>要新增至用戶端要求或用戶端回應的標頭資訊索引鍵。</td>
  </tr>
  <tr>
  <td><code>&lt;value&gt;</code></td>
  <td>要新增至用戶端要求或用戶端回應的標頭資訊值。</td>
  </tr>
  </tbody></table>
 </dd></dl>

<br />



### 移除用戶端回應標頭 (response-remove-headers)
{: #response-remove-headers}

移除來自後端應用程式之用戶端回應中內含的標頭資訊，再將回應傳送至用戶端。
{:shortdesc}

 <dl>
 <dt>說明</dt>
 <dd>Ingress ALB 會充當後端應用程式與用戶端 Web 瀏覽器之間的 Proxy。從後端應用程式傳送到 ALB 的用戶端回應會經過處理（代理），然後放入新的回應，再從 ALB 傳送到用戶端 Web 瀏覽器。雖然代理回應會移除一開始從後端應用程式傳送的 HTTP 標頭資訊，但這個程序不一定會移除所有後端應用程式特有的標頭。從用戶端回應移除標頭資訊，然後再將回應從 ALB 轉遞到用戶端 Web 瀏覽器。</dd>
 <dt>Ingress 資源範例 YAML</dt>
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
  <caption>瞭解註釋元件</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
   </thead>
   <tbody>
   <tr>
   <td><code>service_name</code></td>
   <td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為您為應用程式所建立之 Kubernetes 服務的名稱。</td>
   </tr>
   <tr>
   <td><code>&lt;header&gt;</code></td>
   <td>要從用戶端回應移除的標頭索引鍵。</td>
   </tr>
   </tbody></table>
   </dd></dl>

<br />


### 用戶端要求內文大小 (client-max-body-size)
{: #client-max-body-size}

設定用戶端在要求中所能傳送的內文大小上限。
{:shortdesc}

<dl>
<dt>說明</dt>
<dd>為了維護預期的效能，用戶端要求內文大小上限設為 1 MB。當內文大小超過限制的用戶端要求傳送至 Ingress ALB，且用戶端不容許分割資料時，ALB 會對該用戶端傳回 413（要求實體太大）的 HTTP 回應。在要求內文的大小減少之前，用戶端與 ALB 之間無法進行連線。當用戶端容許資料分割成多個片段時，資料會分成多個 1 MB 的套件，然後傳送至 ALB。

</br></br>
您可能會想要增加內文大小上限，因為您預期用戶端要求的內文大小會大於 1 MB。例如，您要讓用戶端能上傳大型檔案。增加要求內文大小上限可能會影響 ALB 的效能，因為與用戶端的連線必須保持開啟，直到收到要求為止。
</br></br>
<strong>附註：</strong>部分用戶端 Web 瀏覽器無法適當地顯示 413 HTTP 回應訊息。</dd>
<dt>Ingress 資源範例 YAML</dt>
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
<caption>瞭解註釋元件</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;size&gt;</code></td>
 <td>用戶端回應內文的大小上限。例如，若要將大小上限設為 200 MB，請定義 <code>200m</code>。<strong>附註：</strong>您可以將大小設為 0，以停用用戶端要求內文大小的檢查。</td>
 </tr>
 </tbody></table>

 </dd></dl>

<br />


### 大型用戶端標頭緩衝區 (large-client-header-buffers)
{: #large-client-header-buffers}

設定讀取大型用戶端要求標頭的緩衝區數目上限及大小。
{:shortdesc}

<dl>
<dt>說明</dt>
<dd>讀取大型用戶端要求標頭的緩衝區僅依需求配置：如果在結束要求處理之後連線轉移至保留作用中狀態，則會釋放這些緩衝區。依預設，緩衝區大小等於 <code>8K</code> 位元組。如果要求行超出設定的大小上限，即一個緩衝區，則會對用戶端傳回 <code>414 Request-URI Too Large</code> 錯誤。此外，如果要求標頭欄位超出設定的大小上限，即一個緩衝區，則會對用戶端傳回 <code>400 Bad Request</code> 錯誤。您可以調整用於讀取大型用戶端要求標頭的緩衝區數目上限及大小。

<dt>Ingress 資源範例 YAML</dt>
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
<caption>瞭解註釋元件</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;number&gt;</code></td>
 <td>應配置用於讀取大型用戶端要求標頭的緩衝區數目上限。例如，若要將它設為 4，請定義 <code>4</code>。</td>
 </tr>
 <tr>
 <td><code>&lt;size&gt;</code></td>
 <td>讀取大型用戶端要求標頭的緩衝區大小上限。例如，若要將它設為 16 KB，請定義 <code>16k</code>。
   <strong>附註：</strong>大小結尾必須為 <code>k</code> 代表 KB，或 <code>m</code> 代表 MB。</td>
 </tr>
</tbody></table>
</dd>
</dl>

<br />


## 服務限制註釋
{: #service-limit}


### 廣域速率限制 (global-rate-limit)
{: #global-rate-limit}

針對所有服務的已定義金鑰，限制要求處理速率及連線數。
{:shortdesc}

<dl>
<dt>說明</dt>
<dd>
針對所有服務，根據來自所選取後端的所有路徑之單一 IP 位址的已定義金鑰，限制要求處理速率及連線數目。
</dd>


 <dt>Ingress 資源範例 YAML</dt>
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
 <caption>瞭解註釋元件</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
  </thead>
  <tbody>
  <tr>
  <td><code>key</code></td>
  <td>若要根據位置或服務來設定送入要求的廣域限制，請使用 `key=location`。若要根據標頭來設定送入要求的廣域限制，請使用 `X-USER-ID key=$http_x_user_id`。</td>
  </tr>
  <tr>
  <td><code>rate</code></td>
  <td>將 <code>&lt;<em>rate</em>&gt;</code> 取代為處理速率。輸入值作為每秒速率 (r/s) 或每分鐘速率 (r/m)。範例：<code>50r/m</code>。</td>
  </tr>
  <tr>
  <td><code>number-of_connections</code></td>
  <td>將 <code>&lt;<em>conn</em>&gt;</code> 取代為連線數目。</td>
  </tr>
  </tbody></table>

  </dd>
  </dl>

<br />



### 服務速率限制 (service-rate-limit)
{: #service-rate-limit}

針對特定服務，限制要求處理速率及連線數。
{:shortdesc}

<dl>
<dt>說明</dt>
<dd>針對特定服務，根據來自所選取後端的所有路徑之單一 IP 位址的已定義金鑰，限制要求處理速率及連線數目。</dd>


 <dt>Ingress 資源範例 YAML</dt>
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
 <caption>瞭解註釋元件</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="構想圖示"/> 瞭解註釋元件</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td>將 <code>&lt;<em>myservice</em>&gt;</code> 取代為您要建立處理速率的服務名稱。</li>
  </tr>
  <tr>
  <td><code>key</code></td>
  <td>若要根據位置或服務來設定送入要求的廣域限制，請使用 `key=location`。若要根據標頭來設定送入要求的廣域限制，請使用 `X-USER-ID key=$http_x_user_id`。</td>
  </tr>
  <tr>
  <td><code>rate</code></td>
  <td>將 <code>&lt;<em>rate</em>&gt;</code> 取代為處理速率。若要定義每秒速率，請使用 r/s：<code>10r/s</code>。若要定義每分鐘速率，請使用 r/m：<code>50r/m</code>。</td>
  </tr>
  <tr>
  <td><code>number-of_connections</code></td>
  <td>將 <code>&lt;<em>conn</em>&gt;</code> 取代為連線數目。</td>
  </tr>
  </tbody></table>
  </dd>
  </dl>

  <br />



