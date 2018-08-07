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



# Annotazioni di Ingress
{: #ingress_annotation}

Per aggiungere funzionalità al tuo programma di bilanciamento del carico dell'applicazione (o ALB, application load balancer) Ingress, puoi specificare delle annotazioni sotto forma di metadati in una risorsa Ingress.
{: shortdesc}

Per informazioni generali sui servizi Ingress e su come iniziare ad usarli, vedi [Gestione del traffico di rete utilizzando Ingress](cs_ingress.html#planning).

<table>
<caption>Annotazioni generali</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>Annotazioni generali</th>
 <th>Nome</th>
 <th>Descrizione</th>
 </thead>
 <tbody>
 <tr>
 <td><a href="#proxy-external-service">Servizi esterni</a></td>
 <td><code>proxy-external-service</code></td>
 <td>Aggiungi definizioni di percorso a servizi esterni, ad esempio il servizio ospitato in {{site.data.keyword.Bluemix_notm}}.</td>
 </tr>
 <tr>
 <td><a href="#location-modifier">Modificatore ubicazione</a></td>
 <td><code>location-modifier</code></td>
 <td>Modifica il modo in cui l'ALB mette in corrispondenza l'URI della richiesta con il percorso dell'applicazione.</td>
 </tr>
 <tr>
 <td><a href="#alb-id">Instradamento ALB privato</a></td>
 <td><code>ALB-ID</code></td>
 <td>Instrada le richieste in entrata alle tue applicazioni con un ALB privato.</td>
 </tr>
 <tr>
 <td><a href="#rewrite-path">Percorsi di riscrittura</a></td>
 <td><code>rewrite-path</code></td>
 <td>Instrada il traffico di rete in entrata a un percorso diverso su cui è in ascolto la tua applicazione di back-end.</td>
 </tr>
 <tr>
 <td><a href="#tcp-ports">Porte TCP</a></td>
 <td><code>tcp-ports</code></td>
 <td>Accedi a un'applicazione tramite una porta TCP non standard.</td>
 </tr>
 </tbody></table>

<br>

<table>
<caption>Annotazioni di connessione</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>Annotazioni di connessione</th>
 <th>Nome</th>
 <th>Descrizione</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#proxy-connect-timeout">Timeout di lettura e connessione personalizzati</a></td>
  <td><code>proxy-connect-timeout, proxy-read-timeout</code></td>
  <td>Imposta il tempo in cui l'ALB attende di connettersi e leggere dall'applicazione di back-end prima che questa venga considerata non disponibile.</td>
  </tr>
  <tr>
  <td><a href="#keepalive-requests">Richieste di keepalive</a></td>
  <td><code>keepalive-requests</code></td>
  <td>Imposta il numero massimo di richieste che possono essere offerte attraverso una connessione keepalive.</td>
  </tr>
  <tr>
  <td><a href="#keepalive-timeout">Timeout di keepalive</a></td>
  <td><code>keepalive-timeout</code></td>
  <td>Imposta il tempo massimo per cui una connessione keepalive rimane aperta sul server.</td>
  </tr>
  <tr>
  <td><a href="#proxy-next-upstream-config">Upstream successivo del proxy</a></td>
  <td><code>proxy-next-upstream-config</code></td>
  <td>Imposta quando l'ALB può passare una richiesta al server upstream successivo.</td>
  </tr>
  <tr>
  <td><a href="#sticky-cookie-services">Affinità di sessione con i cookie</a></td>
  <td><code>sticky-cookie-services</code></td>
  <td>Instrada sempre il tuo traffico di rete in entrata allo stesso server upstream utilizzando un cookie permanente.</td>
  </tr>
  <tr>
  <td><a href="#upstream-keepalive">Keepalive upstream</a></td>
  <td><code>upstream-keepalive</code></td>
  <td>Imposta il numero massimo di connessioni keepalive inattive per un server upstream.</td>
  </tr>
  </tbody></table>

<br>

  <table>
  <caption>Annotazioni autenticazione HTTPS e TLS/SSL</caption>
  <col width="20%">
  <col width="20%">
  <col width="60%">
  <thead>
  <th>Annotazioni autenticazione HTTPS e TLS/SSL</th>
  <th>Nome</th>
  <th>Descrizione</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#appid-auth">Autenticazione {{site.data.keyword.appid_short}}</a></td>
  <td><code>appid-auth</code></td>
  <td>Utilizza {{site.data.keyword.appid_full_notm}} per eseguire l'autenticazione con la tua applicazione.</td>
  </tr>
  <tr>
  <td><a href="#custom-port">Porte HTTP e HTTPS personalizzate</a></td>
  <td><code>custom-port</code></td>
  <td>Modifica le porte predefinite per il traffico di rete HTTP (porta 80) e HTTPS (porta 443).</td>
  </tr>
  <tr>
  <td><a href="#redirect-to-https">Reindirizzamenti HTTP a HTTPS</a></td>
  <td><code>redirect-to-https</code></td>
  <td>Reindirizza le richieste HTTP nel tuo dominio a HTTPS.</td>
  </tr>
  <tr>
  <td><a href="#hsts">HSTS (HTTP Strict Transport Security)</a></td>
  <td><code>hsts</code></td>
  <td>Imposta il browser per accedere al dominio solo tramite HTTPS.</td>
  </tr>
  <tr>
  <td><a href="#mutual-auth">Autenticazione reciproca</a></td>
  <td><code>mutual-auth</code></td>
  <td>Configura l'autenticazione reciproca per l'ALB.</td>
  </tr>
  <tr>
  <td><a href="#ssl-services">Supporto dei servizi SSL</a></td>
  <td><code>ssl-services</code></td>
  <td>Consenti il supporto dei servizi SSL per crittografare il traffico verso le tue applicazioni upstream che richiedono HTTPS.</td>
  </tr>
  </tbody></table>

<br>

<table>
<caption>Annotazioni Istio</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Annotazioni Istio</th>
<th>Nome</th>
<th>Descrizione</th>
</thead>
<tbody>
<tr>
<td><a href="#istio-services">Servizi Istio</a></td>
<td><code>istio-services</code></td>
<td>Instrada il traffico ai servizi gestiti da Istio.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Annotazioni buffer proxy</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>Annotazioni buffer proxy</th>
 <th>Nome</th>
 <th>Descrizione</th>
 </thead>
 <tbody>
 <tr>
 <td><a href="#proxy-buffering">Buffer dei dati della risposta client</a></td>
 <td><code>proxy-buffering</code></td>
 <td>Disabilita il buffer di una risposta client sull'ALB durante l'invio della risposta al client.</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffers">Buffer proxy</a></td>
 <td><code>proxy-buffers</code></td>
 <td>Imposta il numero e la dimensione dei buffer che leggono una risposta per una singola connessione dal server proxy.</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffer-size">Dimensione buffer proxy</a></td>
 <td><code>proxy-buffer-size</code></td>
 <td>Imposta la dimensione del buffer che legge la prima parte della risposta ricevuta dal server proxy.</td>
 </tr>
 <tr>
 <td><a href="#proxy-busy-buffers-size">Dimensione buffer occupati del proxy</a></td>
 <td><code>proxy-busy-buffers-size</code></td>
 <td>Imposta la dimensione dei buffer del proxy che può essere occupata.</td>
 </tr>
 </tbody></table>

<br>

<table>
<caption>Annotazioni richiesta e risposta</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Annotazioni richiesta e risposta</th>
<th>Nome</th>
<th>Descrizione</th>
</thead>
<tbody>
<tr>
<td><a href="#proxy-add-headers">Intestazione della risposta o della richiesta del client aggiuntivo</a></td>
<td><code>proxy-add-headers, response-add-headers</code></td>
<td>Aggiungi le informazioni di intestazione a una richiesta client prima di inoltrare la richiesta alla tua applicazione di back-end o a una risposta client prima di inviare la risposta al client.</td>
</tr>
<tr>
<td><a href="#response-remove-headers">Rimozione intestazione risposta client</a></td>
<td><code>response-remove-headers</code></td>
<td>Rimuovi le informazioni sull'intestazione da una risposta client prima di inoltrarla al client.</td>
</tr>
<tr>
<td><a href="#client-max-body-size">Dimensione corpo richiesta client</a></td>
<td><code>client-max-body-size</code></td>
<td>Imposta la dimensione massima del corpo che il client può inviare come parte di una richiesta.</td>
</tr>
<tr>
<td><a href="#large-client-header-buffers">Buffer di intestazione client di grandi dimensioni</a></td>
<td><code>large-client-header-buffers</code></td>
<td>Imposta il numero e la dimensione massima dei buffer che leggono intestazioni delle richieste client di grandi dimensioni.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Annotazioni limite del servizio</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Annotazioni limite del servizio</th>
<th>Nome</th>
<th>Descrizione</th>
</thead>
<tbody>
<tr>
<td><a href="#global-rate-limit">Limiti di velocità globali</a></td>
<td><code>global-rate-limit</code></td>
<td>Limita la velocità di elaborazione delle richieste e il numero di connessioni per una chiave definita per tutti i servizi.</td>
</tr>
<tr>
<td><a href="#service-rate-limit">Limiti di velocità del servizio</a></td>
<td><code>service-rate-limit</code></td>
<td>Limita la velocità di elaborazione delle richieste e il numero di connessioni per una chiave definita per servizi specifici.</td>
</tr>
</tbody></table>

<br>



## Annotazioni generali
{: #general}

### Servizi esterni (proxy-external-service)
{: #proxy-external-service}

Aggiungi definizioni di percorso a servizi esterni, ad esempio i servizi ospitati in {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

<dl>
<dt>Descrizione</dt>
<dd>Aggiungi le definizioni di percorso ai servizi esterni. Utilizza questa annotazione solo quando la tua applicazione opera su un servizio esterno invece che su un servizio di back-end. Quando utilizzi questa annotazione per creare una rotta al servizio esterno, sono supportate insieme solo le annotazioni `client-max-body-size`, `proxy-read-timeout`, `proxy-connect-timeout` e `proxy-buffering`. Tutte le altre annotazioni non sono supportate insieme a `proxy-external-service`.
</dd>
<dt>YAML risorsa Ingress di esempio</dt>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
 </thead>
 <tbody>
 <tr>
 <td><code>path</code></td>
 <td>Sostituisci <code>&lt;<em>mypath</em>&gt;</code> con il percorso su cui è in ascolto il servizio esterno.</td>
 </tr>
 <tr>
 <td><code>external-svc</code></td>
 <td>Sostituisci <code>&lt;<em>external_service</em>&gt;</code> con il servizio esterno da richiamare. Ad esempio, <code>https://&lt;myservice&gt;.&lt;region&gt;.mybluemix.net</code>.</td>
 </tr>
 <tr>
 <td><code>host</code></td>
 <td>Sostituisci <code>&lt;<em>mydomain</em>&gt;</code> con il dominio host del servizio esterno.</td>
 </tr>
 </tbody></table>

 </dd></dl>

<br />


### Modificatore ubicazione (location-modifier)
{: #location-modifier}

Modifica il modo in cui l'ALB mette in corrispondenza l'URI della richiesta con il percorso dell'applicazione.
{:shortdesc}

<dl>
<dt>Descrizione</dt>
<dd>Per impostazione predefinita, gli ALB elaborano i percorsi che le applicazioni ascoltano sotto forma di prefissi. Alla ricezione di una richiesta a un'applicazione, l'ALB controlla la risorsa Ingress per rilevare un percorso (come prefisso) che corrisponda all'inizio dell'URI della richiesta. Se viene trovata una corrispondenza, la richiesta viene inoltrata all'indirizzo IP del pod in cui viene distribuita l'applicazione.<br><br>L'annotazione `location-modifier` cambia il modo in cui l'ALB cerca le corrispondenze modificando la configurazione del blocco di ubicazione. Il blocco di ubicazione determina come vengono gestite le richieste per il percorso dell'applicazione.<br><br><strong>Nota</strong>: per gestire i percorsi di espressioni regolari (regex), questa annotazione è obbligatoria.</dd>

<dt>Modificatori supportati</dt>
<dd>

<table>
<caption>Modificatori supportati</caption>
 <col width="10%">
 <col width="90%">
 <thead>
 <th>Modificatore</th>
 <th>Descrizione</th>
 </thead>
 <tbody>
 <tr>
 <td><code>=</code></td>
 <td>Il modificatore con segno di uguale fa sì che l'ALB selezioni solo le corrispondenze esatte. Quando viene trovata una corrispondenza esatta, la ricerca si ferma e viene selezionato il percorso corrispondente.<br>Ad esempio, se la tua applicazione è in ascolto su <code>/tea</code>, l'ALB seleziona solo i percorsi <code>/tea</code> esatti quando fa corrispondere una richiesta alla tua applicazione.</td>
 </tr>
 <tr>
 <td><code>~</code></td>
 <td>il modificatore con tilde fa sì che l'ALB elabori i percorsi come percorsi regex sensibili al maiuscolo/minuscolo durante la corrispondenza.<br>Ad esempio, se la tua applicazione è in ascolto su <code>/coffee</code>, l'ALB può selezionare i percorsi <code>/ab/coffee</code> o <code>/123/coffee</code> quando fa corrispondere una richiesta alla tua applicazione anche se i percorsi non sono esplicitamente impostati per la tua applicazione.</td>
 </tr>
 <tr>
 <td><code>~\*</code></td>
 <td>Il modificatore con tilde seguita da un asterisco fa sì che l'ALB elabori i percorsi come percorsi regex non sensibili al maiuscolo/minuscolo durante la corrispondenza.<br>Ad esempio, se la tua applicazione è in ascolto su <code>/coffee</code>, l'ALB può selezionare i percorsi <code>/ab/Coffee</code> o <code>/123/COFFEE</code> quando fa corrispondere una richiesta alla tua applicazione anche se i percorsi non sono esplicitamente impostati per la tua applicazione.</td>
 </tr>
 <tr>
 <td><code>^~</code></td>
 <td>Il modificatore con accento circonflesso seguito da una tilde fa sì che l'ALB selezioni la migliore corrispondenza non regex anziché un percorso regex.</td>
 </tr>
 </tbody>
</table>

</dd>

<dt>YAML risorsa Ingress di esempio</dt>
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
 <caption>Descrizione dei componenti dell'annotazione</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
  </thead>
  <tbody>
  <tr>
  <td><code>modifier</code></td>
  <td>Sostituisci <code>&lt;<em>location_modifier</em>&gt;</code> con il modificatore ubicazione che vuoi utilizzare per il percorso. I modificatori supportati sono <code>'='</code>, <code>'~'</code>, <code>'~\*'</code> e <code>'^~'</code>. Devi racchiudere i modificatori tra virgolette singole.</td>
  </tr>
  <tr>
  <td><code>serviceName</code></td>
  <td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome del servizio Kubernetes che hai creato per la tua applicazione.</td>
  </tr>
  </tbody></table>
  </dd>
  </dl>

<br />


### Instradamento ALB privato (ALB-ID)
{: #alb-id}

Instrada le richieste in entrata alle tue applicazioni con un ALB privato.
{:shortdesc}

<dl>
<dt>Descrizione</dt>
<dd>
Scegli un ALB privato con cui instradare le richieste in entrata al posto dell'ALB pubblico.</dd>


<dt>YAML risorsa Ingress di esempio</dt>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>&lt;private_ALB_ID&gt;</code></td>
<td>L'ID per il tuo ALB privato. Per trovare l'ID ALB privato, esegui <code>bx cs albs --cluster &lt;my_cluster&gt;</code>.
</td>
</tr>
</tbody></table>
</dd>
</dl>

<br />


### Percorsi di riscrittura (rewrite-path)
{: #rewrite-path}

Instrada il traffico di rete in entrata in un percorso di dominio dell'ALB a un percorso diverso su cui è in ascolto la tua applicazione di back-end.
{:shortdesc}

<dl>
<dt>Descrizione</dt>
<dd>Il tuo dominio dell'ALB Ingress instrada il traffico di rete in entrata in <code>mykubecluster.us-south.containers.appdomain.cloud/beans</code> alla tua applicazione. La tua applicazione è in ascolto su <code>/coffee</code>, invece di <code>/beans</code>. Per inoltrare il traffico di rete in entrata alla tua applicazione, aggiungi l'annotazione di riscrittura al file di configurazione della tua risorsa Ingress. L'annotazione di riscrittura garantisce che il traffico di rete in entrata su <code>/beans</code> venga inoltrato alla tua applicazione utilizzando il percorso <code>/coffee</code>. Quando includi più servizi, utilizza solo un punto e virgola (;) per
separarli.</dd>
<dt>YAML risorsa Ingress di esempio</dt>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome del servizio Kubernetes che hai creato per la tua applicazione.</td>
</tr>
<tr>
<td><code>rewrite</code></td>
<td>Sostituisci <code>&lt;<em>target_path</em>&gt;</code> con il percorso su cui è in ascolto la tua applicazione. Il traffico di rete in entrata nel dominio dell'ALB viene inoltrato al servizio Kubernetes utilizzando tale percorso. La maggior parte delle applicazioni non è
in ascolto su uno specifico percorso, ma utilizza il percorso root e una porta specifica. Nell'esempio precedente, il percorso di riscrittura è stato definito come <code>/coffee</code>.</td>
</tr>
</tbody></table>

</dd></dl>

<br />


### Porte TCP per i programmi di bilanciamento del carico dell'applicazione (tcp-ports)
{: #tcp-ports}

Accedi a un'applicazione tramite una porta TCP non standard.
{:shortdesc}

<dl>
<dt>Descrizione</dt>
<dd>
Utilizza questa annotazione per un'applicazione che esegue un carico di lavoro di flussi TCP.

<p>**Nota**: l'ALB funziona in modalità pass-through e inoltra il traffico alle applicazioni di back-end. In questo caso, la terminazione SSL non è supportata.</p>
</dd>


<dt>YAML risorsa Ingress di esempio</dt>
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
 <caption>Descrizione dei componenti dell'annotazione</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome del servizio Kubernetes a cui accedere tramite una porta TCP non standard.</td>
  </tr>
  <tr>
  <td><code>ingressPort</code></td>
  <td>Sostituisci <code>&lt;<em>ingress_port</em>&gt;</code> con la porta TCP con cui vuoi accedere alla tua applicazione.</td>
  </tr>
  <tr>
  <td><code>servicePort</code></td>
  <td>Questo parametro è facoltativo. Se fornito, la porta viene sostituita con questo valore prima che il traffico venga inviato all'applicazione di back-end. Altrimenti, la porta rimane uguale alla porta Ingress.</td>
  </tr>
  </tbody></table>

 </dd>
 <dt>Utilizzo</dt>
 <dd><ol><li>Controlla le porte aperte per il tuo ALB.
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
L'output della CLI sarà simile al seguente:
<pre class="screen">
<code>NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx 80:30416/TCP,443:32668/TCP   109d</code></pre></li>
<li>Apri la mappa di configurazione di ALB.
<pre class="pre">
<code>kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system</code></pre></li>
<li>Aggiungi le porte TCP alla mappa di configurazione. Sostituisci <code>&lt;port&gt;</code> con le porte TCP che desideri aprire. <b>Nota</b>: per impostazione predefinita, le porte 80 e 443 sono aperte. Se vuoi mantenere le porte 80 e 443 aperte, devi includerle in aggiunta a tutte le altre porte TCP che specifichi nel campo `public-ports`. Se hai abilitato un ALB privato, devi inoltre specificare tutte le porte che vuoi mantenere aperte nel campo `private-ports`. Per ulteriori informazioni, consulta <a href="cs_ingress.html#opening_ingress_ports">Apertura delle porte nell'ALB Ingress</a>.
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
 <li>Verifica che il tuo ALB sia stato riconfigurato con le porte TCP.
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
L'output della CLI sarà simile al seguente:
<pre class="screen">
<code>NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx  169.xx.xxx.xxx &lt;port1&gt;:30776/TCP,&lt;port2&gt;:30412/TCP   109d</code></pre></li>
<li>Configura Ingress per accedere alla tua applicazione tramite una porta TCP non standard. Utilizza il file YAML di esempio in questo riferimento. </li>
<li>Aggiorna la configurazione del tuo ALB.
<pre class="pre">
<code>        kubectl apply -f myingress.yaml
        </code></pre>
</li>
<li>Apri il tuo browser web preferito per accedere alla tua applicazione. Esempio: <code>https://&lt;ibmdomain&gt;:&lt;ingressPort&gt;/</code></li></ol></dd></dl>

<br />


## Annotazioni di connessione
{: #connection}

### Timeout di lettura e connessione personalizzati (proxy-connect-timeout, proxy-read-timeout)
{: #proxy-connect-timeout}

Imposta il tempo in cui l'ALB attende di connettersi e leggere dall'applicazione di back-end prima che questa venga considerata non disponibile.
{:shortdesc}

<dl>
<dt>Descrizione</dt>
<dd>Quando una richiesta client viene inviata all'ALB Ingress, l'ALB apre una connessione all'applicazione di back-end. Per impostazione predefinita, l'ALB attende 60 secondi per ricevere una risposta dall'applicazione di back-end. Se l'applicazione di back-end non risponde entro 60 secondi, la richiesta di connessione viene annullata e l'applicazione di back-end viene considerata non disponibile.

</br></br>
Una volta connesso all'applicazione di back-end, l'ALB legge i dati della risposta da tale applicazione. Durante questa operazione di lettura, l'ALB attende un massimo di 60 secondi tra due operazioni di lettura per ricevere i dati dall'applicazione di back-end. Se l'applicazione di back-end non invia i dati entro 60 secondi, il collegamento all'applicazione di back-end viene chiuso e l'applicazione non viene considerata disponibile.
</br></br>
Un timeout di connessione e timeout di lettura di 60 secondi è il timeout predefinito su un proxy e, in genere, non deve essere modificato.
</br></br>
Se la disponibilità della tua applicazione non è stazionaria o la tua applicazione è lenta a rispondere a causa di elevati carichi di lavoro, potresti voler aumentare i timeout di collegamento o di lettura. Tieni presente che l'aumento del timeout influisce sulle prestazioni dell'ALB in quanto la connessione all'applicazione di back-end deve rimanere aperta fino al raggiungimento del timeout.
</br></br>
D'altra parte, puoi ridurre il timeout per migliorare le prestazioni sull'ALB. Assicurati che la tua applicazione di back-end possa gestire le richieste nel timeout specificato, anche durante elevati carichi di lavoro.</dd>
<dt>YAML risorsa Ingress di esempio</dt>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;connect_timeout&gt;</code></td>
 <td>Il numero di secondi o di minuti da attendere per il collegamento all'applicazione di back-end, ad esempio <code>65s</code> o <code>2m</code>. <strong>Nota:</strong> un timeout di collegamento non può superare 75 secondi.</td>
 </tr>
 <tr>
 <td><code>&lt;read_timeout&gt;</code></td>
 <td>Il numero di secondi o di minuti da attendere prima che venga letta l'applicazione di back-end, ad esempio <code>65s</code> o <code>2m</code>.
 </tr>
 </tbody></table>

 </dd></dl>

<br />



### Richieste di keepalive (keepalive-requests)
{: #keepalive-requests}

Imposta il numero massimo di richieste che possono essere offerte attraverso una connessione keepalive.
{:shortdesc}

<dl>
<dt>Descrizione</dt>
<dd>
Imposta il numero massimo di richieste che possono essere offerte attraverso una connessione keepalive.
</dd>


<dt>YAML risorsa Ingress di esempio</dt>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome del servizio Kubernetes che hai creato per la tua applicazione. Questo parametro è facoltativo. La configurazione viene applicata a tutti i servizi nell'host Ingress, a meno che non venga specificato un servizio. Se il parametro viene fornito, le richieste di keepalive vengono impostate per il servizio specificato. Se il parametro non viene fornito, le richieste di keepalive vengono impostate al livello server del <code>nginx.conf</code> per tutti i servizi per i quali non sono configurate le richieste di keepalive.</td>
</tr>
<tr>
<td><code>requests</code></td>
<td>Sostituisci <code>&lt;<em>max_requests</em>&gt;</code> con il numero massimo di richieste che possono essere offerte attraverso una connessione keepalive.</td>
</tr>
</tbody></table>

</dd>
</dl>

<br />



### Timeout di keepalive (keepalive-timeout)
{: #keepalive-timeout}

Imposta il tempo massimo per cui una connessione keepalive rimane aperta sul lato server.
{:shortdesc}

<dl>
<dt>Descrizione</dt>
<dd>
Imposta il tempo massimo per cui una connessione keepalive rimane aperta sul server.
</dd>


<dt>YAML risorsa Ingress di esempio</dt>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
 </thead>
 <tbody>
 <tr>
 <td><code>serviceName</code></td>
 <td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome del servizio Kubernetes che hai creato per la tua applicazione. Questo parametro è facoltativo. Se il parametro viene fornito, il timeout di keepalive viene impostato per il servizio specificato. Se il parametro non viene fornito, il timeout di keepalive viene impostato al livello server del <code>nginx.conf</code> per tutti i servizi per i quali non è configurato il timeout di keepalive.</td>
 </tr>
 <tr>
 <td><code>timeout</code></td>
 <td>Sostituisci <code>&lt;<em>time</em>&gt;</code> con una quantità di tempo in secondi. Esempio: <code>timeout=20s</code>. Un valore zero disabilita le connessioni client keepalive.</td>
 </tr>
 </tbody></table>

 </dd>
 </dl>

<br />


### Upstream successivo del proxy (proxy-next-upstream-config)
{: #proxy-next-upstream-config}

Imposta quando l'ALB può passare una richiesta al server upstream successivo.
{:shortdesc}

<dl>
<dt>Descrizione</dt>
<dd>
L'ALB Ingress funge da proxy tra l'applicazione client e la tua applicazione. Alcune configurazioni di applicazione richiedono più server upstream che gestiscono le richieste client in entrata dall'ALB. A volte, il server proxy utilizzato dall'ALB non può stabilire una connessione con un server upstream utilizzato dall'applicazione L'ALB può quindi tentare di stabilire una connessione con il server upstream successivo per passare ad esso la richiesta. Puoi utilizzare l'annotazione `proxy-next-upstream-config` per impostare in quali casi, per quanto tempo e per quante volte l'ALB può tentare di passare una richiesta al server upstream successivo.<br><br><strong>Nota</strong>: il timeout viene sempre configurato quando utilizzi `proxy-next-upstream-config`, quindi non aggiungere `timeout=true` a questa annotazione.
</dd>
<dt>YAML risorsa Ingress di esempio</dt>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome del servizio Kubernetes che hai creato per la tua applicazione.</td>
</tr>
<tr>
<td><code>retries</code></td>
<td>Sostituisci <code>&lt;<em>tries</em>&gt;</code> con il numero massimo di volte in cui l'ALB tenterà di passare una richiesta al server upstream successivo. Questo numero include la richiesta originale. Per disattivare questa limitazione, utilizza <code>0</code>. Se non specifichi un valore, verrà utilizzato il valore predefinito <code>0</code>.
</td>
</tr>
<tr>
<td><code>timeout</code></td>
<td>Sostituisci <code>&lt;<em>time</em>&gt;</code> con la quantità di tempo massima, in secondi, in cui l'ALB tenta di passare una richiesta al server upstream successivo. Ad esempio, per impostare un tempo di 30 secondi, immetti <code>30s</code>. Per disattivare questa limitazione, utilizza <code>0</code>. Se non specifichi un valore, verrà utilizzato il valore predefinito <code>0</code>.
</td>
</tr>
<tr>
<td><code>error</code></td>
<td>Se impostato su <code>true</code>, l'ALB passa una richiesta al server upstream successivo quando si verifica un errore mentre si stabilisce una connessione con il primo server upstream, si passa una richiesta ad esso oppure si legge l'intestazione della risposta.
</td>
</tr>
<tr>
<td><code>invalid_header</code></td>
<td>Se impostato su <code>true</code>, l'ALB passa una richiesta al server upstream successivo quando il primo server upstream restituisce una risposta vuota o non valida.
</td>
</tr>
<tr>
<td><code>http_502</code></td>
<td>Se impostato su <code>true</code>, l'ALB passa una richiesta al server upstream successivo quando il primo server upstream restituisce una risposta con il codice 502. Puoi designare i seguenti codici di risposta HTTP: <code>500</code>, <code>502</code>, <code>503</code>, <code>504</code>, <code>403</code>, <code>404</code>, <code>429</code>.
</td>
</tr>
<tr>
<td><code>non_idempotent</code></td>
<td>Se impostato su <code>true</code>, l'ALB può passare le richieste con un metodo non-idempotent al server upstream successivo. Per impostazione predefinita, l'ALB non passa queste richieste al server upstream successivo.
</td>
</tr>
<tr>
<td><code>off</code></td>
<td>Per impedire che l'ALB passi le richieste al server upstream successivo, deve essere impostato su <code>true</code>.
</td>
</tr>
</tbody></table>
</dd>
</dl>

<br />


### Affinità di sessione con i cookie (sticky-cookie-services)
{: #sticky-cookie-services}

Utilizza l'annotazione cookie permanente per aggiungere l'affinità di sessione al tuo ALB e instradare sempre il traffico di rete in entrata allo stesso server upstream.
{:shortdesc}

<dl>
<dt>Descrizione</dt>
<dd>Per l'alta disponibilità, alcune configurazioni di applicazione richiedono di distribuire più server upstream che gestiscono le richieste client in entrata. Quando un client si collega alla tua applicazione di back-end, puoi utilizzare l'affinità di sessione in modo che un client sia servito dallo stesso server upstream per la durata di una sessione o per il tempo necessario per completare un'attività. Puoi configurare il tuo ALB per garantire l'affinità di sessione indirizzando sempre il traffico di rete in entrata allo stesso server upstream.

</br></br>
Ad ogni client che si collega alla tua applicazione di back-end, l'ALB assegna uno dei server upstream disponibili. L'ALB crea un cookie di sessione che viene memorizzato nell'applicazione del client e che viene incluso nelle informazioni di intestazione di ogni richiesta tra l'ALB e il client. Le informazioni nel cookie garantiscono che tutte le richieste vengano gestite dallo stesso server upstream nella sessione.

</br></br>
Quando includi più servizi, utilizza un punto e virgola (;) per separarli.</dd>
<dt>YAML risorsa Ingress di esempio</dt>
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
  <caption>Descrizione dei componenti dell'annotazione</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome del servizio Kubernetes che hai creato per la tua applicazione.</td>
  </tr>
  <tr>
  <td><code>name</code></td>
  <td>Sostituisci <code>&lt;<em>cookie_name</em>&gt;</code> con il nome di un cookie permanente creato durante una sessione.</td>
  </tr>
  <tr>
  <td><code>expires</code></td>
  <td>Sostituisci <code>&lt;<em>expiration_time</em>&gt;</code> con il tempo in secondi (s), minuti (m) o ore (h) prima che scada il cookie permanente. Il tempo non è dipendente dall'attività utente. Una volta scaduto, il cookie viene eliminato dal browser web del client e non viene più inviato all'ALB. Ad esempio, per impostare un orario di scadenza di 1 secondo, 1 minuto o 1 ora, immetti <code>1s</code>, <code>1m</code> o <code>1h</code>.</td>
  </tr>
  <tr>
  <td><code>path</code></td>
  <td>Sostituisci <code>&lt;<em>cookie_path</em>&gt;</code> con il percorso che viene aggiunto al dominio secondario Ingress e che indica per quali domini e domini secondari il cookie viene inviato all'ALB. Ad esempio, se il tuo dominio Ingress è <code>www.myingress.com</code> e desideri inviare il cookie in ogni richiesta client, devi impostare <code>path=/</code>. Se desideri inviare il cookie solo per <code>www.myingress.com/myapp</code> e a tutti i relativi domini secondari, devi impostare <code>path=/myapp</code>.</td>
  </tr>
  <tr>
  <td><code>hash</code></td>
  <td>Sostituisci <code>&lt;<em>hash_algorithm</em>&gt;</code> con l'algoritmo hash che protegge le informazioni nel cookie. È
supportato solo <code>sha1</code>. SHA1 crea un riepilogo hash in base alle informazioni nel cookie e lo accoda ad esso. Il server può decrittografare le informazioni nel cookie e verificare l'integrità dei dati.</td>
  </tr>
  </tbody></table>

 </dd></dl>

<br />




### Keepalive upstream (upstream-keepalive)
{: #upstream-keepalive}

Imposta il numero massimo di connessioni keepalive inattive per un server upstream.
{:shortdesc}

<dl>
<dt>Descrizione</dt>
<dd>
Imposta il numero massimo di connessioni keepalive inattive al server upstream di un determinato servizio. Il server upstream ha 64 connessioni keepalive inattive per impostazione predefinita.
</dd>


 <dt>YAML risorsa Ingress di esempio</dt>
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
 <caption>Descrizione dei componenti dell'annotazione</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome del servizio Kubernetes che hai creato per la tua applicazione.</td>
  </tr>
  <tr>
  <td><code>keepalive</code></td>
  <td>Sostituisci <code>&lt;<em>max_connections</em>&gt;</code> con il numero massimo di connessioni keepalive inattive a un server upstream. Il valore predefinito è <code>64</code>. Un valore <code>0</code> disabilita le connessioni keepalive upstream per il servizio specificato.</td>
  </tr>
  </tbody></table>
  </dd>
  </dl>

<br />




## Annotazioni autenticazione HTTPS e TLS/SSL
{: #https-auth}

### Autenticazione {{site.data.keyword.appid_short_notm}} (appid-auth)
{: #appid-auth}

  Utilizza {{site.data.keyword.appid_full_notm}} per eseguire l'autenticazione con la tua applicazione.
  {:shortdesc}

  <dl>
  <dt>Descrizione</dt>
  <dd>
  Esegui l'autenticazione di richieste HTTP/HTTPS web o API con {{site.data.keyword.appid_short_notm}}.

  <p>Se imposti il tipo di richiesta su <code>web</code>, viene convalidata una richiesta web che contiene un token di accesso {{site.data.keyword.appid_short_notm}}. Se la convalida del token non riesce, la richiesta web viene rifiutata. Se la richiesta non contiene un token di accesso, verrà reindirizzata alla pagina di accesso di {{site.data.keyword.appid_short_notm}}. <strong>Nota</strong>: perché l'autenticazione web {{site.data.keyword.appid_short_notm}} funzioni, è necessario abilitare i cookie nel browser dell'utente.</p>

  <p>Se imposti il tipo di richiesta su <code>api</code>, viene convalidata una richiesta API che contiene un token di accesso {{site.data.keyword.appid_short_notm}}. Se la richiesta non contiene un token di accesso, l'utente riceverà il messaggio di errore <code>401: Unauthorized</code>.</p>

  <p>**Nota**: per motivi di sicurezza, l'autenticazione {{site.data.keyword.appid_short_notm}} supporta solo i backend con TLS/SSL abilitato.</p>
  </dd>
   <dt>YAML risorsa Ingress di esempio</dt>
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
   <caption>Descrizione dei componenti dell'annotazione</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
    </thead>
    <tbody>
    <tr>
    <td><code>bindSecret</code></td>
    <td>Sostituisci <em><code>&lt;bind_secret&gt;</code></em> con il segreto Kubernetes che memorizza il segreto di bind.</td>
    </tr>
    <tr>
    <td><code>namespace</code></td>
    <td>Sostituisci <em><code>&lt;namespace&gt;</code></em> con lo spazio dei nomi del segreto di bind. Il valore predefinito di questo campo è lo spazio dei nomi `default`.</td>
    </tr>
    <tr>
    <td><code>requestType</code></td>
    <td>Sostituisci <code><em>&lt;request_type&gt;</em></code> con il tipo di richiesta che vuoi inviare a {{site.data.keyword.appid_short_notm}}. I valori accettati sono `web` o `api`. Il valore predefinito è `api`.</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td>Sostituisci <code><em>&lt;myservice&gt;</em></code> con il nome del servizio Kubernetes che hai creato per la tua applicazione. Questo campo è facoltativo. Se non si include un nome di servizio, l'annotazione viene abilitata per tutti i servizi.  Se si include un nome di servizio, l'annotazione viene abilitata solo per tale servizio. Separa più servizi con un punto e virgola (;).</td>
    </tr>
    </tbody></table>
    </dd>
    <dt>Utilizzo</dt>
    <dd>Poiché l'applicazione utilizza {{site.data.keyword.appid_short_notm}} per l'autenticazione, devi eseguire il provisioning di un'istanza {{site.data.keyword.appid_short_notm}}, configurare l'istanza con URI di reindirizzamento validi e generare un segreto di bind.
    <ol>
    <li>Esegui il provisioning di un'[istanza {{site.data.keyword.appid_short_notm}}](https://console.bluemix.net/catalog/services/app-id).</li>
    <li>Nella console di gestione di {{site.data.keyword.appid_short_notm}}, aggiungi gli URI di reindirizzamento per la tua applicazione.</li>
    <li>Crea un segreto di bind.
    <pre class="pre"><code>bx cs cluster-service-bind &lt;my_cluster&gt; &lt;my_namespace&gt; &lt;my_service_instance_GUID&gt;</code></pre> </li>
    <li>Configura l'annotazione <code>appid-auth</code>.</li>
    </ol></dd>
    </dl>

<br />



### Porte HTTP e HTTPS personalizzate (custom-port)
{: #custom-port}

Modifica le porte predefinite per il traffico di rete HTTP (porta 80) e HTTPS (porta 443).
{:shortdesc}

<dl>
<dt>Descrizione</dt>
<dd>Per impostazione predefinita, l'ALB Ingress è configurato per l'ascolto del traffico di rete HTTP in entrata sulla porta 80 e del traffico di rete HTTPS in entrata sulla porta 443. Puoi modificare le porte predefinite per aggiungere sicurezza al tuo dominio ALB o per abilitare solo una porta HTTPS.
</dd>


<dt>YAML risorsa Ingress di esempio</dt>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;protocol&gt;</code></td>
 <td>Immetti <code>http</code> o <code>https</code> per modificare la porta predefinita per il traffico di rete HTTP o HTTPS in entrata.</td>
 </tr>
 <tr>
 <td><code>&lt; port&gt;</code></td>
 <td>Immetti il numero di porta che desideri utilizzare per il traffico di rete HTTP o HTTPS in entrata.  <p><strong>Nota:</strong> quando viene specificata una porta personalizzata per HTTP o HTTPS, le porte predefinite non sono più valide. Ad esempio, per modificare la porta predefinita per HTTPS in 8443, ma utilizzare la porta predefinita per HTTP, devi impostare le porte personalizzate per entrambe: <code>custom-port: "protocol=http port=80; protocol=https port=8443"</code>.</p></td>
 </tr>
 </tbody></table>

 </dd>
 <dt>Utilizzo</dt>
 <dd><ol><li>Controlla le porte aperte per il tuo ALB.
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
L'output della CLI sarà simile al seguente:
<pre class="screen">
<code>NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx 80:30416/TCP,443:32668/TCP   109d</code></pre></li>
<li>Apri la mappa di configurazione di ALB.
<pre class="pre">
<code>kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system</code></pre></li>
<li>Aggiungi le porte HTTP e HTTPS non predefinite alla mappa di configurazione. Sostituisci &lt;port&gt; con la porta HTTP o HTTPS che desideri aprire. <b>Nota</b>: per impostazione predefinita, le porte 80 e 443 sono aperte. Se vuoi mantenere le porte 80 e 443 aperte, devi includerle in aggiunta a tutte le altre porte TCP che specifichi nel campo `public-ports`. Se hai abilitato un ALB privato, devi inoltre specificare tutte le porte che vuoi mantenere aperte nel campo `private-ports`. Per ulteriori informazioni, consulta <a href="cs_ingress.html#opening_ingress_ports">Apertura delle porte nell'ALB Ingress</a>.
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
 <li>Verifica che il tuo ALB sia stato riconfigurato con le porte non predefinite.
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
L'output della CLI sarà simile al seguente:
<pre class="screen">
<code>NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx  169.xx.xxx.xxx &lt;port1&gt;:30776/TCP,&lt;port2&gt;:30412/TCP   109d</code></pre></li>
<li>Configura il tuo Ingress in modo che utilizzi le porte non predefinite quando instrada il traffico di rete ai tuoi servizi. Utilizza il file YAML di esempio in questo riferimento. </li>
<li>Aggiorna la configurazione del tuo ALB.
<pre class="pre">
<code>        kubectl apply -f myingress.yaml
        </code></pre>
</li>
<li>Apri il tuo browser web preferito per accedere alla tua applicazione. Esempio: <code>https://&lt;ibmdomain&gt;:&lt;port&gt;/&lt;service_path&gt;/</code></li></ol></dd></dl>

<br />



### Reindirizzamenti HTTP a HTTPS (redirect-to-https)
{: #redirect-to-https}

Converti le richieste client http non sicure in https.
{:shortdesc}

<dl>
<dt>Descrizione</dt>
<dd>Configura il tuo ALB Ingress per proteggere il tuo dominio con il certificato TLS fornito da IBM o con il tuo certificato TLS personalizzato. Alcuni utenti potrebbero tentare di accedere alle tue applicazioni utilizzando una richiesta <code>http</code> non sicura al tuo dominio ALB, ad esempio <code>http://www.myingress.com</code>, invece di utilizzare <code>https</code>. Puoi utilizzare l'annotazione di reindirizzamento per convertire sempre le richieste HTTP non sicure in HTTPS. Se non utilizzi questa annotazione, le richieste HTTP non sicure non vengono convertite in richieste HTTPS per impostazione predefinita e potrebbero esporre al pubblico informazioni riservate non crittografate.

</br></br>
Il reindirizzamento delle richieste HTTP in HTTPS è disabilitato per impostazione predefinita.</dd>

<dt>YAML risorsa Ingress di esempio</dt>
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


### HSTS (HTTP Strict Transport Security)
{: #hsts}

<dl>
<dt>Descrizione</dt>
<dd>
HSTS indica al browser di accedere a un dominio solo tramite HTTPS. Anche se l'utente inserisce o segue un link HTTP semplice, il browser aggiorna rigorosamente la connessione a HTTPS.
</dd>


<dt>YAML risorsa Ingress di esempio</dt>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
  </thead>
  <tbody>
  <tr>
  <td><code>enabled</code></td>
  <td>Utilizza <code>true</code> per abilitare HSTS.</td>
  </tr>
    <tr>
  <td><code>maxAge</code></td>
  <td>Sostituisci <code>&lt;<em>31536000</em>&gt;</code> con un numero intero che rappresenta il numero di secondi in cui un browser memorizzerà nella cache l'invio di richieste direttamente a HTTPS. Il valore predefinito è <code>31536000</code>, che equivale a 1 anno.</td>
  </tr>
  <tr>
  <td><code>includeSubdomains</code></td>
  <td>Utilizza <code>true</code> per indicare al browser che la politica HSTS si applica anche a tutti i domini secondari del dominio corrente. Il valore predefinito è <code>true</code>. </td>
  </tr>
  </tbody></table>

  </dd>
  </dl>


<br />


### Autenticazione reciproca (mutual-auth)
{: #mutual-auth}

Configura l'autenticazione reciproca per l'ALB.
{:shortdesc}

<dl>
<dt>Descrizione</dt>
<dd>
Configura l'autenticazione reciproca del traffico di downstream per l'ALB Ingress. Il client esterno autentica il server e anche il server autentica il client utilizzando i certificati. L'autenticazione reciproca è nota anche come autenticazione basata su certificato o autenticazione bidirezionale.
</dd>

<dt>Prerequisiti</dt>
<dd>
<ul>
<li>[Devi disporre di un segreto valido che contenga l'autorità di certificazione (CA) richiesta](cs_app.html#secrets). Per l'autenticazione reciproca sono necessari anche <code>client.key</code> e <code>client.crt</code>.</li>
<li>Per abilitare l'autenticazione reciproca su una porta diversa da 443, [configura l'ALB per aprire la porta valida](cs_ingress.html#opening_ingress_ports).</li>
</ul>
</dd>


<dt>YAML risorsa Ingress di esempio</dt>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>secretName</code></td>
<td>Sostituisci <code>&lt;<em>mysecret</em>&gt;</code> con un nome per la risorsa del segreto.</td>
</tr>
<tr>
<td><code> port</code></td>
<td>Sostituisci <code>&lt;<em>port</em>&gt;</code> con il numero di porta ALB.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Sostituisci <code>&lt;<em>servicename</em>&gt;</code> con il nome di una o più risorse Ingress. Questo parametro è facoltativo.</td>
</tr>
</tbody></table>

</dd>
</dl>

<br />



### Supporto dei servizi SSL (ssl-services)
{: #ssl-services}

Consenti le richieste HTTPS e crittografa il traffico alle tue applicazioni upstream.
{:shortdesc}

<dl>
<dt>Descrizione</dt>
<dd>
Crittografa il traffico che Ingress invia alle applicazioni upstream che richiedono HTTPS. Se le tue applicazioni upstream possono gestire TLS, puoi facoltativamente fornire un certificato contenuto in un segreto TLS.<br></br>**Facoltativo**: a questa annotazione, puoi aggiungere [l'autenticazione unidirezionale o l'autenticazione reciproca](#ssl-services-auth).</dd>


<dt>YAML risorsa Ingress di esempio</dt>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
  </thead>
  <tbody>
  <tr>
  <td><code>ssl-service</code></td>
  <td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome del servizio che richiede HTTPS. Il traffico viene crittografato dall'ALB a questo servizio dell'applicazione.</td>
  </tr>
  <tr>
  <td><code>ssl-secret</code></td>
  <td>Facoltativo: se vuoi utilizzare un segreto TLS e la tua applicazione upstream può gestire TLS, sostituisci <code>&lt;<em>service-ssl-secret</em>&gt;</code> con il segreto del servizio. Se fornisci un segreto, il valore deve contenere i <code>trusted.crt</code>, <code>client.crt</code> e <code>client.key</code> che la tua applicazione si aspetta dal client. Per creare un segreto TLS, per prima cosa [converti i certificati e la chiave in base-64 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.base64encode.org/). Poi consulta [Creazione dei segreti](cs_app.html#secrets).</td>
  </tr>
  </tbody></table>

  </dd>
</dl>

<br />


#### Supporto dei servizi SSL con l'autenticazione
{: #ssl-services-auth}

<dl>
<dt>Descrizione</dt>
<dd>
Consenti le richieste HTTPS e crittografa il traffico alle tue applicazioni upstream con l'autorizzazione unidirezionale o reciproca per garantire ulteriore sicurezza.
</dd>


<dt>YAML risorsa Ingress di esempio</dt>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
  </thead>
  <tbody>
  <tr>
  <td><code>ssl-service</code></td>
  <td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome del servizio che richiede HTTPS. Il traffico viene crittografato dall'ALB a questo servizio dell'applicazione.</td>
  </tr>
  <tr>
  <td><code>ssl-secret</code></td>
  <td>Sostituisci <code>&lt;<em>service-ssl-secret</em>&gt;</code> con il segreto dell'autenticazione reciproca per il servizio. Il valore deve contenere il certificato CA che la tua applicazione si aspetta dal client. Per creare un segreto di autenticazione reciproca, per prima cosa [converti il certificato e la chiave in base-64 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.base64encode.org/). Poi consulta [Creazione dei segreti](cs_app.html#secrets).</td>
  </tr>
  </tbody></table>

  </dd>
  </dl>

<br />


## Annotazioni Istio
{: #istio-annotations}

### Servizi Istio (istio-services)
{: #istio-services}

  Instrada il traffico ai servizi gestiti da Istio.
  {:shortdesc}

  <dl>
  <dt>Descrizione</dt>
  <dd>
  Se hai servizi gestiti da Istio, puoi utilizzare un ALB cluster per instradare le richieste HTTP/HTTPS al controller Ingress Istio. Il controller Ingress Istio instrada quindi le richieste ai servizi dell'applicazione. Al fine di instradare il traffico, devi apportare modifiche alle risorse Ingress sia per l'ALB cluster che per il controller Ingress Istio.
    <br><br>Nella risorsa Ingress per l'ALB cluster, devi:
      <ul>
        <li>specificare l'annotazione `istio-services`</li>
        <li>definire il percorso di servizio come il percorso effettivo su cui è in ascolto l'applicazione</li>
        <li>definire la porta di servizio come la porta del controller Ingress Istio</li>
      </ul>
    <br>Nella risorsa Ingress per il controller Ingress Istio, devi:
      <ul>
        <li>definire il percorso di servizio come il percorso effettivo su cui è in ascolto l'applicazione</li>
        <li>definire la porta di servizio come la porta HTTP/HTTPS del servizio dell'applicazione esposto dal controller Ingress Istio</li>
    </ul>
  </dd>

   <dt>YAML risorsa Ingress di esempio per l'ALB cluster</dt>
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
   <caption>Descrizione dei componenti del file YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>enabled</code></td>
      <td>Per abilitare l'instradamento del traffico ai servizi gestiti da Istio, deve essere impostato su <code>True</code>.</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td>Sostituisci <code><em>&lt;myservice1&gt;</em></code> con il nome del servizio Kubernetes che hai creato per la tua applicazione gestita da Istio. Separa più servizi con un punto e virgola (;). Questo campo è facoltativo. Se non specifichi un nome di servizio, tutti i servizi gestiti da Istio verranno abilitati per l'instradamento del traffico.</td>
    </tr>
    <tr>
    <td><code>istioServiceNamespace</code></td>
    <td>Sostituisci <code><em>&lt;istio-namespace&gt;</em></code> con lo spazio dei nomi Kubernetes in cui è installato Istio. Questo campo è facoltativo. Se non specifichi uno spazio dei nomi, verrà utilizzato lo spazio dei nomi <code>istio-system</code>.</td>
    </tr>
    <tr>
    <td><code>istioServiceName</code></td>
    <td>Sostituisci <code><em>&lt;istio-ingress-service&gt;</em></code> con il nome del servizio Ingress Istio. Questo campo è facoltativo. Se non specifichi il nome di servizio Ingress Istio, verrà utilizzato il nome di servizio <code>istio-ingress</code>.</td>
    </tr>
    <tr>
    <td><code>path</code></td>
      <td>Per ciascun servizio gestito da Istio a cui desideri instradare il traffico, sostituisci <code><em>&lt;/myapp1&gt;</em></code> con il percorso di backend su cui è in ascolto il servizio gestito da Istio. Il percorso deve corrispondere a quello che hai definito nella risorsa Ingress Istio.</td>
    </tr>
    <tr>
    <td><code>servicePort</code></td>
    <td>Per ciascun servizio gestito da Istio a cui desideri instradare il traffico, sostituisci <code><em>&lt;istio_ingress_port&gt;</em></code> con la porta del controller Ingress Istio.</td>
    </tr>
    </tbody></table>
    </dd>
    </dl>

## Annotazioni buffer proxy
{: #proxy-buffer}


### Buffer dei dati della risposta client (proxy-buffering)
{: #proxy-buffering}

Utilizza l'annotazione del buffer per disabilitare l'archiviazione dei dati della risposta sull'ALB mentre i dati vengono inviati al client.
{:shortdesc}

<dl>
<dt>Descrizione</dt>
<dd>L'ALB Ingress funge da proxy tra la tua applicazione di back-end e il browser web del client. Quando una risposta viene inviata dall'applicazione di back-end al client, i dati della risposta vengono memorizzati nel buffer sull'ALB per impostazione predefinita. L'ALB trasmette tramite proxy la risposta client e inizia a inviare la risposta al client al ritmo del client. Una volta che l'ALB ha ricevuto tutti i dati dall'applicazione di back-end, la connessione all'applicazione di back-end viene chiusa. La connessione dall'ALB al client rimane aperta finché il client non riceve tutti i dati.

</br></br>
Se il buffer dei dati della risposta sull'ALB è disabilitato, i dati vengono immediatamente inviati dall'ALB al client. Il client deve essere in grado di gestire i dati in entrata al ritmo dell'ALB. Se il client è troppo lento, i dati potrebbero andare persi.
</br></br>
Il buffer dei dati della risposta sull'ALB è abilitato per impostazione predefinita.</dd>
<dt>YAML risorsa Ingress di esempio</dt>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
 </thead>
 <tbody>
 <tr>
 <td><code>enabled</code></td>
   <td>Per disabilitare il buffer dei dati della risposta nell'ALB, imposta su <code>false</code>.</td>
 </tr>
 <tr>
 <td><code>serviceName</code></td>
 <td>Sostituisci <code><em>&lt;myservice1&gt;</em></code> con il nome del servizio Kubernetes che hai creato per la tua applicazione. Separa più servizi con un punto e virgola (;). Questo campo è facoltativo. Se non specifichi un nome di servizio, tutti i servizi utilizzano questa annotazione. </td>
 </tr>
 </tbody></table>
 </dd>
 </dl>

<br />



### Buffer proxy (proxy-buffers)
{: #proxy-buffers}

Configura il numero e la dimensione dei buffer del proxy per l'ALB.
{:shortdesc}

<dl>
<dt>Descrizione</dt>
<dd>
Imposta il numero e la dimensione dei buffer che leggono una risposta per una singola connessione dal server proxy. La configurazione viene applicata a tutti i servizi nell'host Ingress, a meno che non venga specificato un servizio. Ad esempio, se si specifica una configurazione come <code>serviceName=SERVICE number=2 size=1k</code>, al servizio viene applicato 1k. Se si specifica una configurazione come <code>number=2 size=1k</code>, 1k viene applicato a tutti i servizi nell'host Ingress.
</dd>
<dt>YAML risorsa Ingress di esempio</dt>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
 </thead>
 <tbody>
 <tr>
 <td><code>serviceName</code></td>
 <td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome di un servizio a cui applicare proxy-buffers.</td>
 </tr>
 <tr>
 <td><code>number</code></td>
 <td>Sostituisci <code>&lt;<em>number_of_buffers</em>&gt;</code> con un numero, ad esempio <em>2</em>.</td>
 </tr>
 <tr>
 <td><code>size</code></td>
 <td>Sostituisci <code>&lt;<em>size</em>&gt;</code> con la dimensione di ciascun buffer in kilobyte (k o K), ad esempio <em>1K</em>.</td>
 </tr>
 </tbody>
 </table>
 </dd></dl>

<br />


### Dimensione buffer proxy (proxy-buffer-size)
{: #proxy-buffer-size}

Configura la dimensione del buffer del proxy che legge la prima parte della risposta.
{:shortdesc}

<dl>
<dt>Descrizione</dt>
<dd>
Imposta la dimensione del buffer che legge la prima parte della risposta ricevuta dal server proxy. Questa parte della risposta di solito contiene una piccola intestazione di risposta. La configurazione viene applicata a tutti i servizi nell'host Ingress, a meno che non venga specificato un servizio. Ad esempio, se si specifica una configurazione come <code>serviceName=SERVICE size=1k</code>, al servizio viene applicato 1k. Se si specifica una configurazione come <code>size=1k</code>, 1k viene applicato a tutti i servizi nell'host Ingress.
</dd>


<dt>YAML risorsa Ingress di esempio</dt>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
 </thead>
 <tbody>
 <tr>
 <td><code>serviceName</code></td>
 <td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome di un servizio a cui applicare proxy-buffers-size.</td>
 </tr>
 <tr>
 <td><code>size</code></td>
 <td>Sostituisci <code>&lt;<em>size</em>&gt;</code> con la dimensione di ciascun buffer in kilobyte (k o K), ad esempio <em>1K</em>.</td>
 </tr>
 </tbody></table>

 </dd>
 </dl>

<br />



### Dimensione buffer occupati del proxy (proxy-busy-buffers-size)
{: #proxy-busy-buffers-size}

Configura la dimensione dei buffer del proxy che può essere occupata.
{:shortdesc}

<dl>
<dt>Descrizione</dt>
<dd>
Limita la dimensione di tutti i buffer che stanno inviando una risposta al client mentre la risposta non è stata ancora completamente letta. Nel frattempo, il resto dei buffer può leggere la risposta e, se necessario, eseguire il buffer di una parte della risposta in un file temporaneo. La configurazione viene applicata a tutti i servizi nell'host Ingress, a meno che non venga specificato un servizio. Ad esempio, se si specifica una configurazione come <code>serviceName=SERVICE size=1k</code>, al servizio viene applicato 1k. Se si specifica una configurazione come <code>size=1k</code>, 1k viene applicato a tutti i servizi nell'host Ingress.
</dd>


<dt>YAML risorsa Ingress di esempio</dt>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome di un servizio a cui applicare proxy-busy-buffers-size.</td>
</tr>
<tr>
<td><code>size</code></td>
<td>Sostituisci <code>&lt;<em>size</em>&gt;</code> con la dimensione di ciascun buffer in kilobyte (k o K), ad esempio <em>1K</em>.</td>
</tr>
</tbody></table>

 </dd>
 </dl>

<br />



## Annotazioni richiesta e risposta
{: #request-response}


### Intestazione della risposta o della richiesta del client aggiuntivo (proxy-add-headers, response-add-headers)
{: #proxy-add-headers}

Aggiungi ulteriori informazioni di intestazione a una richiesta client prima che venga inviata all'applicazione di back-end o a una risposta client prima che venga inviata al client.
{:shortdesc}

<dl>
<dt>Descrizione</dt>
<dd>L'ALB Ingress funge da proxy tra l'applicazione client e la tua applicazione di back-end. Le richieste client inviate all'ALB vengono elaborate (tramite proxy) e inserite in una nuova richiesta che viene quindi inviata all'applicazione di back-end. Allo stesso modo, le risposte dell'applicazione di back-end inviate all'ALB vengono elaborate (tramite proxy) e inserite in una nuova richiesta che viene quindi inviata al client. La trasmissione tramite proxy di una richiesta o una risposta rimuove le informazioni dell'intestazione HTTP, come il nome utente, che erano state inizialmente inviate dal client o dall'applicazione di back-end.

<br><br>
Se la tua applicazione di back-end richiede le informazioni dell'intestazione HTTP, puoi utilizzare l'annotazione <code>proxy-add-headers</code> per aggiungere le informazioni di intestazione alla richiesta client prima che venga inoltrata dall'ALB all'applicazione di back-end.

<br>
<ul><li>Ad esempio, potresti dover aggiungere le seguenti informazioni dell'intestazione X-Forward alla richiesta prima che venga inoltrata alla tua applicazione:

<pre class="screen">
<code>proxy_set_header Host $host;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;</code></pre>

</li>

<li>Per aggiungere le informazioni dell'intestazione X-Forward alla richiesta inviata alla tua applicazione, utilizza l'annotazione `proxy-add-headers` nel seguente modo:

<pre class="screen">
<code>ingress.bluemix.net/proxy-add-headers: |
  serviceName=<myservice1> {
  Host $host;
  X-Forwarded-Proto $scheme;
  X-Forwarded-For $proxy_add_x_forwarded_for;
  }</code></pre>

</li></ul><br>

Se l'applicazione web client richiede le informazioni dell'intestazione HTTP, puoi utilizzare l'annotazione <code>response-add-headers</code> per aggiungere le informazioni di intestazione alla risposta prima che venga inoltrata dall'ALB all'applicazione web client.</dd>

<dt>YAML risorsa Ingress di esempio</dt>
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
 <caption>Descrizione dei componenti dell'annotazione</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
  </thead>
  <tbody>
  <tr>
  <td><code>service_name</code></td>
  <td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome del servizio Kubernetes che hai creato per la tua applicazione.</td>
  </tr>
  <tr>
  <td><code>&lt;header&gt;</code></td>
  <td>La chiave delle informazioni sull'intestazione da aggiungere alla richiesta o alla risposta client.</td>
  </tr>
  <tr>
  <td><code>&lt;value&gt;</code></td>
  <td>Il valore delle informazioni sull'intestazione da aggiungere alla richiesta o alla risposta client.</td>
  </tr>
  </tbody></table>
 </dd></dl>

<br />



### Rimozione intestazione risposta client (response-remove-headers)
{: #response-remove-headers}

Rimuovi le informazioni di intestazione incluse nella risposta client dall'applicazione di back-end prima che la risposta venga inviata al client.
 {:shortdesc}

 <dl>
 <dt>Descrizione</dt>
 <dd>L'ALB Ingress funge da proxy tra la tua applicazione di back-end e il browser web del client. Le risposte client dell'applicazione di back-end inviate all'ALB vengono elaborate (tramite proxy) e inserite in una nuova risposta che viene quindi inviata dall'ALB al browser web del client. Sebbene la trasmissione tramite proxy di una risposta rimuova le informazioni sull'intestazione http che erano state inizialmente inviate dall'applicazione di back-end, questo processo potrebbe non rimuovere tutte le intestazione specifiche dell'applicazione di back-end. Rimuovi le informazioni di intestazione da una risposta client prima che la risposta venga inoltrata dall'ALB al browser web del client.</dd>
 <dt>YAML risorsa Ingress di esempio</dt>
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
  <caption>Descrizione dei componenti dell'annotazione</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
   </thead>
   <tbody>
   <tr>
   <td><code>service_name</code></td>
   <td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome del servizio Kubernetes che hai creato per la tua applicazione.</td>
   </tr>
   <tr>
   <td><code>&lt;header&gt;</code></td>
   <td>La chiave dell'intestazione da rimuovere dalla risposta client.</td>
   </tr>
   </tbody></table>
   </dd></dl>

<br />


### Dimensione corpo richiesta client (client-max-body-size)
{: #client-max-body-size}

Imposta la dimensione massima del corpo che il client può inviare come parte di una richiesta.
{:shortdesc}

<dl>
<dt>Descrizione</dt>
<dd>Per mantenere le prestazioni previste, la dimensione del corpo della richiesta client massima è impostata su 1 megabyte. Quando all'ALB Ingress viene inviata una richiesta client con una dimensione del corpo superiore al limite e il client non consente la divisione dei dati, l'ALB restituisce una risposta HTTP 413 (Request Entity Too Large) al client. Una connessione tra il client e l'ALB non è possibile finché non viene ridotta la dimensione del corpo della richiesta. Quando il client consente di suddividere i dati in più blocchi, i dati vengono suddivisi in pacchetti da 1 megabyte e inviati all'ALB.

</br></br>
Potresti voler ridurre la dimensione del corpo massima perché ti attendi richieste client con una dimensione del corpo maggiore di 1 megabyte. Ad esempio, vuoi che il tuo client sia in grado di caricare file di grandi dimensioni. L'aumento della dimensione massima del corpo della richiesta potrebbe influire sulle prestazioni del tuo ALB poiché la connessione al client deve rimanere aperta fino a quando la richiesta non viene ricevuta.
</br></br>
<strong>Nota:</strong> alcuni browser web del client non possono visualizzare il messaggio di risposta HTTP 413 correttamente.</dd>
<dt>YAML risorsa Ingress di esempio</dt>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;size&gt;</code></td>
 <td>La dimensione massima del corpo della risposta client. Ad esempio, per impostare la dimensione massima su 200 megabyte, definisci <code>200m</code>. <strong>Nota:</strong> puoi impostare la dimensione su 0 per disabilitare il controllo della dimensione del corpo della richiesta client.</td>
 </tr>
 </tbody></table>

 </dd></dl>

<br />


### Buffer di intestazione client di grandi dimensioni (large-client-header-buffers)
{: #large-client-header-buffers}

Imposta il numero e la dimensione massima dei buffer che leggono intestazioni delle richieste client di grandi dimensioni.
{:shortdesc}

<dl>
<dt>Descrizione</dt>
<dd>I buffer che leggono le intestazioni delle richieste client di grandi dimensioni vengono assegnati solo in base alla domanda: se una connessione viene trasferita nello stato keepalive dopo l'elaborazione di fine richiesta, questi buffer vengono rilasciati. Per impostazione predefinita, la dimensione del buffer è uguale a <code>8K</code>. Se una riga di richiesta supera la dimensione massima impostata di un buffer, al client viene restituito l'errore <code>414 Request-URI Too Large</code>. Inoltre, se un campo di intestazione della richiesta supera la dimensione massima impostata di un buffer, al client viene restituito l'errore <code>400 Bad Request</code>. Puoi regolare il numero e la dimensione massima dei buffer utilizzati per la lettura di intestazioni di richieste client di grandi dimensioni.

<dt>YAML risorsa Ingress di esempio</dt>
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
<caption>Descrizione dei componenti dell'annotazione</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;number&gt;</code></td>
 <td>Il numero massimo di buffer che devono essere assegnati per leggere l'intestazione della richiesta client di grandi dimensioni. Ad esempio, per impostarlo su 4, definisci <code>4</code>.</td>
 </tr>
 <tr>
 <td><code>&lt;size&gt;</code></td>
 <td>La dimensione massima dei buffer che leggono l'intestazione della richiesta client di grandi dimensioni. Ad esempio, per impostarla su 16 kilobyte, definisci <code>16k</code>.
   <strong>Nota:</strong> la dimensione deve terminare con <code>k</code> per kilobyte o con <code>m</code> per megabyte.</td>
 </tr>
</tbody></table>
</dd>
</dl>

<br />


## Annotazioni limite del servizio
{: #service-limit}


### Limiti di velocità globali
{: #global-rate-limit}

Limita la velocità di elaborazione delle richieste e il numero di connessioni per una chiave definita per tutti i servizi.
{:shortdesc}

<dl>
<dt>Descrizione</dt>
<dd>
Per tutti i servizi, limita la velocità di elaborazione delle richieste e il numero di connessioni per una chiave definita provenienti da un singolo indirizzo IP per tutti i percorsi dei back-end selezionati.
</dd>


 <dt>YAML risorsa Ingress di esempio</dt>
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
 <caption>Descrizione dei componenti dell'annotazione</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
  </thead>
  <tbody>
  <tr>
  <td><code>key</code></td>
  <td>Per impostare un limite globale per le richieste in entrata in base alla posizione o al servizio, utilizza `key=location`. Per impostare un limite globale per le richieste in entrata in base all'intestazione, utilizza `X-USER-ID key=$http_x_user_id`.</td>
  </tr>
  <tr>
  <td><code>rate</code></td>
  <td>Sostituisci <code>&lt;<em>rate</em>&gt;</code> con la velocità di elaborazione. Immetti un valore come una velocità al secondo (r/s) o al minuto (r/m). Esempio: <code>50r/m</code>.</td>
  </tr>
  <tr>
  <td><code>number-of_connections</code></td>
  <td>Sostituisci <code>&lt;<em>conn</em>&gt;</code> con il numero di connessioni.</td>
  </tr>
  </tbody></table>

  </dd>
  </dl>

<br />



### Limiti di velocità del servizio (service-rate-limit)
{: #service-rate-limit}

Limita la velocità di elaborazione delle richieste e il numero di connessioni per i servizi specifici.
{:shortdesc}

<dl>
<dt>Descrizione</dt>
<dd>Per servizi specifici, limita la velocità di elaborazione delle richieste e il numero di connessioni per una chiave definita provenienti da un singolo indirizzo IP per tutti i percorsi dei back-end selezionati.
</dd>


 <dt>YAML risorsa Ingress di esempio</dt>
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
 <caption>Descrizione dei componenti dell'annotazione</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti dell'annotazione</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td>Sostituisci <code>&lt;<em>myservice</em>&gt;</code> con il nome del servizio per cui vuoi limitare la velocità di elaborazione.</li>
  </tr>
  <tr>
  <td><code>key</code></td>
  <td>Per impostare un limite globale per le richieste in entrata in base alla posizione o al servizio, utilizza `key=location`. Per impostare un limite globale per le richieste in entrata in base all'intestazione, utilizza `X-USER-ID key=$http_x_user_id`.</td>
  </tr>
  <tr>
  <td><code>rate</code></td>
  <td>Sostituisci <code>&lt;<em>rate</em>&gt;</code> con la velocità di elaborazione. Per definire una velocità al secondo, utilizza r/s: <code>10r/s</code>. Per definire una velocità al minuto, utilizza r/m: <code>50r/m</code>.</td>
  </tr>
  <tr>
  <td><code>number-of_connections</code></td>
  <td>Sostituisci <code>&lt;<em>conn</em>&gt;</code> con il numero di connessioni.</td>
  </tr>
  </tbody></table>
  </dd>
  </dl>

  <br />



