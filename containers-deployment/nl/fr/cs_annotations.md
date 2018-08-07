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



# Annotations Ingress
{: #ingress_annotation}

Pour ajouter des fonctionnalités à l'équilibreur de charge d'application (ALB) Ingress, vous pouvez indiquer des annotations sous forme de métadonnées dans une ressource Ingress.
{: shortdesc}

Pour obtenir des informations générales sur les services Ingress et commencer à les utiliser, voir [Gestion de trafic réseau à l'aide d'Ingress](cs_ingress.html#planning).

<table>
<caption>Annotations générales</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>Annotations générales</th>
 <th>Nom</th>
 <th>Description</th>
 </thead>
 <tbody>
 <tr>
 <td><a href="#proxy-external-service">Services externes</a></td>
 <td><code>proxy-external-service</code></td>
 <td>Ajouter des définitions de chemin à des services externes, tels qu'un service hébergé dans {{site.data.keyword.Bluemix_notm}}.</td>
 </tr>
 <tr>
 <td><a href="#location-modifier">Modificateur d'emplacement</a></td>
 <td><code>location-modifier</code></td>
 <td>Modifier le mode utilisé par l'équilibreur de charge ALB pour faire correspondre l'URI de la demande au chemin de l'application.</td>
 </tr>
 <tr>
 <td><a href="#alb-id">Routage de l'équilibreur de charge d'application (ALB) privé</a></td>
 <td><code>ALB-ID</code></td>
 <td>Router les demandes entrantes vers vos applications avec un équilibreur de charge ALB privé.</td>
 </tr>
 <tr>
 <td><a href="#rewrite-path">Chemins de redirection</a></td>
 <td><code>rewrite-path</code></td>
 <td>Router le trafic réseau entrant vers un autre chemin sur lequel votre application de back end est à l'écoute.</td>
 </tr>
 <tr>
 <td><a href="#tcp-ports">Ports TCP</a></td>
 <td><code>tcp-ports</code></td>
 <td>Accéder à une application via un port TCP non standard.</td>
 </tr>
 </tbody></table>

<br>

<table>
<caption>Annotations de connexion</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>Annotations de connexion</th>
 <th>Nom</th>
 <th>Description</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#proxy-connect-timeout">Personnalisation des paramètres connect-timeouts et read-timeouts</a></td>
  <td><code>proxy-connect-timeout, proxy-read-timeout</code></td>
  <td>Définir la durée d'attente de connexion de l'équilibreur de charge ALB à l'application de back end et la durée de lecture avant que l'application soit considérée comme indisponible.</td>
  </tr>
  <tr>
  <td><a href="#keepalive-requests">Demandes keepalive</a></td>
  <td><code>keepalive-requests</code></td>
  <td>Définir le nombre maximum de demandes pouvant être traitées via une connexion persistante.</td>
  </tr>
  <tr>
  <td><a href="#keepalive-timeout">Délai d'expiration de connexion persistante (keepalive)</a></td>
  <td><code>keepalive-timeout</code></td>
  <td>Définir la durée maximale d'ouverture d'une connexion persistante sur le serveur.</td>
  </tr>
  <tr>
  <td><a href="#proxy-next-upstream-config">Proxy vers le prochain serveur en amont</a></td>
  <td><code>proxy-next-upstream-config</code></td>
  <td>Définir à quel moment l'équilibreur de charge ALB transmet une demande au prochain serveur en amont.</td>
  </tr>
  <tr>
  <td><a href="#sticky-cookie-services">Affinité de session avec des cookies</a></td>
  <td><code>sticky-cookie-services</code></td>
  <td>Acheminer systématiquement le trafic réseau entrant vers le même serveur en amont à l'aide d'un cookie d'association (sticky cookie).</td>
  </tr>
  <tr>
  <td><a href="#upstream-keepalive">Connexions persistantes en amont</a></td>
  <td><code>upstream-keepalive</code></td>
  <td>Définir le nombre maximum de connexions persistantes inactives pour un serveur en amont.</td>
  </tr>
  </tbody></table>

<br>

  <table>
  <caption>Annotations d'authentification HTTPS et TLS/SSL</caption>
  <col width="20%">
  <col width="20%">
  <col width="60%">
  <thead>
  <th>Annotations d'authentification HTTPS et TLS/SSL</th>
  <th>Nom</th>
  <th>Description</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#appid-auth">Authentification {{site.data.keyword.appid_short}}</a></td>
  <td><code>appid-auth</code></td>
  <td>Utiliser {{site.data.keyword.appid_full_notm}} pour l'authentification auprès de votre application.</td>
  </tr>
  <tr>
  <td><a href="#custom-port">Ports HTTP et HTTPS personnalisés</a></td>
  <td><code>custom-port</code></td>
  <td>Modifier les ports par défaut pour le trafic réseau HTTP (port 80) et HTTPS (port 443).</td>
  </tr>
  <tr>
  <td><a href="#redirect-to-https">Redirection HTTP vers HTTPS</a></td>
  <td><code>redirect-to-https</code></td>
  <td>Rediriger les demandes HTTP non sécurisées sur votre domaine vers HTTPS.</td>
  </tr>
  <tr>
  <td><a href="#hsts">Dispositif de sécurité HSTS (HTTP Strict Transport Security)</a></td>
  <td><code>hsts</code></td>
  <td>Définir le navigateur pour accéder au domaine uniquement à l'aide de HTTPS.</td>
  </tr>
  <tr>
  <td><a href="#mutual-auth">Authentification mutuelle</a></td>
  <td><code>mutual-auth</code></td>
  <td>Configurer l'authentification mutuelle pour l'équilibreur de charge ALB.</td>
  </tr>
  <tr>
  <td><a href="#ssl-services">Support des services SSL</a></td>
  <td><code>ssl-services</code></td>
  <td>Autoriser le support des services SSL à chiffrer le trafic vers vos applications en amont nécessitant HTTPS.</td>
  </tr>
  </tbody></table>

<br>

<table>
<caption>Annotations Istio</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Annotations Istio</th>
<th>Nom</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td><a href="#istio-services">Services Istio</a></td>
<td><code>istio-services</code></td>
<td>Acheminer le trafic vers des services gérés par Istio.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Annotations de mémoire tampon de proxy</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>Annotations de mémoire tampon de proxy</th>
 <th>Nom</th>
 <th>Description</th>
 </thead>
 <tbody>
 <tr>
 <td><a href="#proxy-buffering">Mise en mémoire tampon des données de réponse client</a></td>
 <td><code>proxy-buffering</code></td>
 <td>Désactiver la mise en mémoire tampon d'une réponse client sur l'équilibreur de charge ALB lors de l'envoi de la réponse au client.</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffers">Tampons de proxy</a></td>
 <td><code>proxy-buffers</code></td>
 <td>Définir le nombre et la taille des mémoires tampons qui lisent une réponse pour une connexion unique provenant du serveur relayé via un proxy.</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffer-size">Taille de mémoire tampon de proxy</a></td>
 <td><code>proxy-buffer-size</code></td>
 <td>Définir la taille de la mémoire tampon qui lit la première partie de la réponse reçue du serveur relayé via un proxy.</td>
 </tr>
 <tr>
 <td><a href="#proxy-busy-buffers-size">Taille des mémoires tampons occupées de proxy</a></td>
 <td><code>proxy-busy-buffers-size</code></td>
 <td>Définir la taille des mémoires tampons de proxy pouvant être occupées.</td>
 </tr>
 </tbody></table>

<br>

<table>
<caption>Annotations de demande et de réponse</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Annotations de demande et de réponse</th>
<th>Nom</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td><a href="#proxy-add-headers">En-tête de réponse ou de demande client supplémentaire</a></td>
<td><code>proxy-add-headers, response-add-headers</code></td>
<td>Ajouter des informations d'en-tête à une demande client avant d'acheminer la demande vers votre application de back end ou à une réponse client avant d'envoyer la réponse au client.</td>
</tr>
<tr>
<td><a href="#response-remove-headers">Retrait d'en-tête de réponse client</a></td>
<td><code>response-remove-headers</code></td>
<td>Retirer les informations d'en-tête d'une réponse client avant d'acheminer la réponse vers le client.</td>
</tr>
<tr>
<td><a href="#client-max-body-size">Taille du corps de demande client</a></td>
<td><code>client-max-body-size</code></td>
<td>Définir la taille maximale du corps que le client peut envoyer dans le cadre d'une demande.</td>
</tr>
<tr>
<td><a href="#large-client-header-buffers">Mémoires tampon d'en-têtes de demande client volumineux</a></td>
<td><code>large-client-header-buffers</code></td>
<td>Définir le nombre et la taille maximum des mémoires tampons qui lisent des en-têtes de demande client volumineux</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Annotations de limites de service</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Annotations de limites de service</th>
<th>Nom</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td><a href="#global-rate-limit">Limites de débit globales</a></td>
<td><code>global-rate-limit</code></td>
<td>Limiter le débit de traitement des demandes et le nombre de connexions par une clé définie pour tous les services.</td>
</tr>
<tr>
<td><a href="#service-rate-limit">Limites de débit de service</a></td>
<td><code>service-rate-limit</code></td>
<td>Limiter le débit de traitement des demandes et le nombre de connexions par une clé définie pour des services spécifiques.</td>
</tr>
</tbody></table>

<br>



## Annotations générales
{: #general}

### Services externes (proxy-external-service)
{: #proxy-external-service}

Ajoutez des définitions de chemin d'accès à des services externes, tels des services hébergés dans {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>Ajouter des définitions de chemin d'accès à des services externes. N'utilisez cette annotation que si votre application opère sur un service externe au lieu d'un service de back end. Lorsque vous utilisez cette annotation pour créer une route de service externe, seules les annotations `client-max-body-size`, `proxy-read-timeout`, `proxy-connect-timeout` et `proxy-buffering` sont prises en charge conjointement. Aucune autre annotation n'est prise en charge en conjonction avec `proxy-external-service`.
</dd>
<dt>Exemple de fichier YAML de ressource Ingress</dt>
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
<caption>Description des composants de l'annotation</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
 </thead>
 <tbody>
 <tr>
 <td><code>path</code></td>
 <td>Remplacez <code>&lt;<em>mypath</em>&gt;</code> par le chemin sur lequel le service externe est à l'écoute.</td>
 </tr>
 <tr>
 <td><code>external-svc</code></td>
 <td>Remplacez <code>&lt;<em>external_service</em>&gt;</code> par le service externe à appeler. Par exemple, <code>https://&lt;myservice&gt;.&lt;region&gt;.mybluemix.net</code>.</td>
 </tr>
 <tr>
 <td><code>host</code></td>
 <td>Remplacez <code>&lt;<em>mydomain</em>&gt;</code> par le domaine hôte du service externe.</td>
 </tr>
 </tbody></table>

 </dd></dl>

<br />


### Modificateur d'emplacement (location-modifier)
{: #location-modifier}

Modifiez le mode utilisé par l'équilibreur de charge d'application (ALB) pour faire correspondre l'URI de la demande avec le chemin de l'application.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>Par défaut, les équilibreurs de charge ALB traitent les chemins sur lesquels les applications sont à l'écoute en tant que préfixes. Lorsqu'un équilibreur de charge ALB reçoit une demande destinée à une application, il recherche un chemin (sous forme de préfixe) dans la ressource Ingress qui correspond au début de l'URI de la demande. Si une correspondance est trouvée, la demande est transmise à l'adresse IP du pod sur lequel est déployée l'application.<br><br>L'annotation `location-modifier` modifie le mode utilisé par l'équilibreur de charge ALB pour rechercher des correspondances en modifiant l'emplacement de configuration des blocs. Le bloc d'emplacement détermine comment sont traitées les demandes correspondant au chemin de l'application.<br><br><strong>Remarque</strong> : cette annotation est obligatoire pour traiter les chemins d'expression régulière (regex).</dd>

<dt>Modificateurs pris en charge</dt>
<dd>

<table>
<caption>Modificateurs pris en charge</caption>
 <col width="10%">
 <col width="90%">
 <thead>
 <th>Modificateur</th>
 <th>Description</th>
 </thead>
 <tbody>
 <tr>
 <td><code>=</code></td>
 <td>Avec le modificateur représenté par un signe égal, l'équilibreur de charge ALB sélectionne uniquement les correspondances exactes. Lorsqu'une correspondance exacte est trouvée, la recherche s'arrête et le chemin correspondant est sélectionné.<br>Par exemple, si votre application est à l'écoute sur <code>/tea</code>, l'équilibreur de charge ALB sélectionne uniquement les chemins <code>/tea</code> exacts lorsqu'il établit une correspondance entre une demande et votre application.</td>
 </tr>
 <tr>
 <td><code>~</code></td>
 <td>Avec le modificateur représenté par un tilde, l'équilibreur de charge ALB traite les chemins comme des chemins d'expression régulière sensibles à la casse lors de la correspondance.<br>Par exemple, si votre application est à l'écoute sur <code>/coffee</code>, l'équilibreur de charge ALB peut sélectionner les chemins <code>/ab/coffee</code> ou <code>/123/coffee</code> lorsqu'il établit une correspondance entre une demande et votre application même si ces chemins ne sont pas explicitement définis pour votre application.</td>
 </tr>
 <tr>
 <td><code>~\*</code></td>
 <td>Avec le modificateur représenté par un tilde suivi d'un astérisque, l'équilibreur de charge ALB traite les chemins comme des chemins d'expression régulière insensibles à la casse lors de la correspondance.<br>Par exemple, si votre application est à l'écoute sur <code>/coffee</code>, l'équilibreur de charge ALB peut sélectionner les chemins <code>/ab/Coffee</code> ou <code>/123/COFFEE</code> lorsqu'il établit une correspondance entre une demande et votre application même si ces chemins ne sont pas explicitement définis pour votre application.</td>
 </tr>
 <tr>
 <td><code>^~</code></td>
 <td>Avec le modificateur représenté par un caret suivi d'un tilde, l'équilibreur de charge ALB sélectionne la meilleure correspondance (hors expression régulière) au lieu d'un chemin d'accès d'expression régulière.</td>
 </tr>
 </tbody>
</table>

</dd>

<dt>Exemple de fichier YAML de ressource Ingress</dt>
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
 <caption>Description des composants de l'annotation</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
  </thead>
  <tbody>
  <tr>
  <td><code>modifier</code></td>
  <td>Remplacez <code>&lt;<em>location_modifier</em>&gt;</code> par le modificateur d'emplacement à utiliser pour le chemin. Les modificateurs pris en charge sont : <code>'='</code>, <code>'~'</code>, <code>'~\*'</code> et <code>'^~'</code>. Vous devez mettre les modificateurs entre guillemets simples.</td>
  </tr>
  <tr>
  <td><code>serviceName</code></td>
  <td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service Kubernetes que vous avez créé pour votre application.</td>
  </tr>
  </tbody></table>
  </dd>
  </dl>

<br />


### Routage d'équilibreur de charge ALB privé (ALB-ID)
{: #alb-id}

Routez les demandes entrantes vers vos applications avec un équilibreur de charge ALB privé.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>
Sélectionner un équilibreur de charge ALB privé pour acheminer les demandes entrantes au lieu de l'équilibreur de charge ALB public.</dd>


<dt>Exemple de fichier YAML de ressource Ingress</dt>
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
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>&lt;private_ALB_ID&gt;</code></td>
<td>ID de votre équilibreur de charge d'application (ALB) privé. Pour identifier l'ID de l'ALB privé, exécutez la commande <code>bx cs albs --cluster &lt;my_cluster&gt;</code>.
</td>
</tr>
</tbody></table>
</dd>
</dl>

<br />


### Chemins de redirection (rewrite-path)
{: #rewrite-path}

Routez le trafic réseau entrant sur un chemin de domaine d'un équilibreur de charge ALB vers un autre chemin utilisé par votre application de back end en mode écoute.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>Votre domaine d'équilibreur de charge d'application (ALB) Ingress achemine le trafic réseau entrant sur <code>mykubecluster.us-south.containers.appdomain.cloud/beans</code> vers votre application. Votre application est en mode écoute sur <code>/coffee</code> au lieu de <code>/beans</code>. Pour acheminer le trafic réseau entrant vers votre application, ajoutez l'annotation rewrite au fichier de configuration de votre ressource Ingress. L'annotation rewrite garantit que le trafic réseau entrant sur <code>/beans</code> est réacheminé vers votre application en utilisant le chemin <code>/coffee</code>. Lorsque vous incluez plusieurs services, utilisez uniquement un point-virgule (;) pour les séparer.</dd>
<dt>Exemple de fichier YAML de ressource Ingress</dt>
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
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service Kubernetes que vous avez créé pour votre application.</td>
</tr>
<tr>
<td><code>rewrite</code></td>
<td>Remplacez <code>&lt;<em>target_path</em>&gt;</code> par le chemin sur lequel votre application est à l'écoute. Le trafic réseau entrant sur le domaine ALB est acheminé au service Kubernetes en utilisant ce chemin. La plupart des applications ne sont pas à l'écoute sur un chemin spécifique, mais utilisent le chemin racine et un port spécifique. Dans l'exemple ci-dessus, le chemin de redirection a été défini en tant que <code>/coffee</code>.</td>
</tr>
</tbody></table>

</dd></dl>

<br />


### Ports TCP pour les équilibreurs de charge d'application (tcp-ports)
{: #tcp-ports}

Accédez à une application via un port TCP non standard.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>
Utiliser cette annotation pour une application qui s'exécute sur une charge de travail de flux TCP.

<p>**Remarque** : l'équilibreur de charge ALB opère en mode passe-système et achemine le trafic aux applications de back end. La terminaison SSL n'est pas prise en charge dans ce cas.</p>
</dd>


<dt>Exemple de fichier YAML de ressource Ingress</dt>
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
 <caption>Description des composants de l'annotation</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service Kubernetes auquel accéder via un port TCP non standard.</td>
  </tr>
  <tr>
  <td><code>ingressPort</code></td>
  <td>Remplacez <code>&lt;<em>ingress_port</em>&gt;</code> par le port TCP sur lequel accéder à votre application.</td>
  </tr>
  <tr>
  <td><code>servicePort</code></td>
  <td>Ce paramètre est facultatif. Lorsqu'il est fourni, le port est remplacé par cette valeur avant l'envoi du trafic à l'application de back end. Sinon, le port est identique au port Ingress.</td>
  </tr>
  </tbody></table>

 </dd>
 <dt>Utilisation</dt>
 <dd><ol><li>Passez en revue les ports ouverts pour votre équilibreur de charge ALB.
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
La sortie de votre interface CLI sera similaire à ceci :
<pre class="screen">
<code>NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx 80:30416/TCP,443:32668/TCP   109d</code></pre></li>
<li>Ouvrez la mappe de configuration ALB.
<pre class="pre">
<code>kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system</code></pre></li>
<li>Ajoutez les ports TCP à la mappe de configuration. Remplacez <code>&lt;port&gt;</code> par les ports TCP que vous souhaitez ouvrir. <b>Remarque</b> : par défaut, les ports 80 et 443 sont ouverts. Pour conserver les ports 80 et 443 ouverts, vous devez également les inclure en plus des autres ports TCP que vous indiquez dans la zone `public-ports`. Si vous avez activé un équilibreur de charge ALB privé, vous devez également spécifier tous les ports que vous souhaitez garder ouverts dans la zone `private-ports`. Pour plus d'informations, voir <a href="cs_ingress.html#opening_ingress_ports">Ouverture de ports dans l'équilibreur de charge d'application (ALB) Ingress</a>.
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
 <li>Vérifiez que votre équilibreur de charge ALB est reconfiguré avec les ports TCP.
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
La sortie de votre interface CLI sera similaire à ceci :
<pre class="screen">
<code>NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx  169.xx.xxx.xxx &lt;port1&gt;:30776/TCP,&lt;port2&gt;:30412/TCP   109d</code></pre></li>
<li>Configurez Ingress pour accéder à votre application via un port TCP non standard. Utilisez l'exemple de fichier YAML dans cette référence. </li>
<li>Mettez à jour la configuration de votre équilibreur de charge ALB.
<pre class="pre">
<code>        kubectl apply -f myingress.yaml
        </code></pre>
</li>
<li>Ouvrez le navigateur Web de votre choix pour accéder à votre application. Exemple : <code>https://&lt;ibmdomain&gt;:&lt;ingressPort&gt;/</code></li></ol></dd></dl>

<br />


## Annotations de connexion
{: #connection}

### Personnalisation des paramètres connect-timeout et read-timeout (proxy-connect-timeout, proxy-read-timeout)
{: #proxy-connect-timeout}

Définissez la durée d'attente de connexion de l'équilibreur de charge ALB à l'application de back end et la durée de lecture avant que l'application soit considérée comme indisponible.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>Lorsqu'une demande client est envoyée à l'équilibreur de charge d'application (ALB) Ingress, une connexion à l'application de back end est ouverte par l'ALB. Par défaut, l'ALB patiente pendant 60 secondes pour la réception d'une réponse de l'application de back end. Si l'application de back end ne répond pas au cours de ce délai de 60 secondes, la demande de connexion est abandonnée et l'application de back end est considérée comme indisponible.

</br></br>
Une fois que l'ALB est connecté à l'application de back end, les données de réponse sont lues depuis cette application par l'ALB. Au cours de cette opération de lecture, l'ALB patiente jusqu'à 60 secondes entre deux opérations de lecture pour recevoir des données de l'application de back end. Si l'application de back end n'envoie pas les données au cours de ce délai de 60 secondes, la connexion avec l'application de back end est fermée et cette dernière est considérée comme indisponible.
</br></br>
Sur un proxy, les paramètres connect-timeout et read-timeout sont définis par défaut sur 60 secondes et, en principe, ne doivent pas être modifiés.
</br></br>
Si la disponibilité de votre application est imprévisible ou si celle-ci est lente à répondre en raison de charges de travail élevées, vous envisagerez peut-être d'augmenter la valeur du paramètre connect-timeout ou read-timeout. Notez que l'augmentation du délai d'attente a une incidence sur les performances de l'équilibreur de charge ALB puisque la connexion à l'application de back end doit rester ouverte jusqu'à expiration de ce délai.
</br></br>
D'un autre côté, vous pouvez réduire le délai d'attente afin d'améliorer les performances sur l'équilibreur de charge ALB. Assurez-vous que votre application de back end est capable de traiter les demandes dans le délai d'attente imparti, même lorsque les charges de travail sont plus importantes.</dd>
<dt>Exemple de fichier YAML de ressource Ingress</dt>
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
<caption>Description des composants de l'annotation</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;connect_timeout&gt;</code></td>
 <td>Délai d'attente en secondes ou en minutes pour la connexion à l'application de back end, par exemple <code>65s</code> ou <code>2m</code>. <strong>Remarque :</strong> la valeur d'un paramètre connect-timeout ne doit pas excéder 75 secondes.</td>
 </tr>
 <tr>
 <td><code>&lt;read_timeout&gt;</code></td>
 <td>Délai d'attente en secondes ou en minutes avant la lecture de l'application de back end, par exemple <code>65s</code> ou <code>2m</code>.
 </tr>
 </tbody></table>

 </dd></dl>

<br />



### Demandes keepalive (keepalive-requests)
{: #keepalive-requests}

Définissez le nombre maximum de demandes pouvant être traitées via une connexion persistante.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>
Définir le nombre maximum de demandes pouvant être traitées via une connexion persistante.
</dd>


<dt>Exemple de fichier YAML de ressource Ingress</dt>
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
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service Kubernetes que vous avez créé pour votre application. Ce paramètre est facultatif. Si aucun service n'est spécifié, la configuration est appliquée à tous les services de l'hôte Ingress. Si le paramètre est fourni, les demandes keepalive sont définies pour le service indiqué. Si le paramètre n'est pas fourni, les demandes keepalive sont définies au niveau serveur de <code>nginx.conf</code> pour tous les services pour lesquels aucune demande keepalive n'est configurée.</td>
</tr>
<tr>
<td><code>requests</code></td>
<td>Remplacez <code>&lt;<em>max_requests</em>&gt;</code> par le nombre maximal de demandes pouvant être traitées dans le cadre d'une connexion persistante (keepalive).</td>
</tr>
</tbody></table>

</dd>
</dl>

<br />



### Délai d'expiration d'une connexion persistante (keepalive-timeout)
{: #keepalive-timeout}

Définissez la durée maximale d'ouverture d'une connexion persistante côté serveur.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>
Définir la durée maximale d'ouverture d'une connexion persistante sur le serveur.
</dd>


<dt>Exemple de fichier YAML de ressource Ingress</dt>
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
<caption>Description des composants de l'annotation</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
 </thead>
 <tbody>
 <tr>
 <td><code>serviceName</code></td>
 <td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service Kubernetes que vous avez créé pour votre application. Ce paramètre est facultatif. Si le paramètre est fourni, le délai d'expiration de la connexion persistante (keepalive-timeout) est défini pour le service indiqué. Si le paramètre n'est pas fourni, le délai d'expiration de la connexion persistante est défini au niveau serveur de <code>nginx.conf</code> pour tous les services pour lesquels aucun délai d'expiration de connexion persistante n'est configuré.</td>
 </tr>
 <tr>
 <td><code>timeout</code></td>
 <td>Remplacez <code>&lt;<em>time</em>&gt;</code> par la durée, en secondes. Exemple :<code>timeout=20s</code>. Une valeur zéro désactive les connexions client persistantes.</td>
 </tr>
 </tbody></table>

 </dd>
 </dl>

<br />


### Proxy vers le prochain serveur en amont (proxy-next-upstream-config)
{: #proxy-next-upstream-config}

Définissez à quel moment l'équilibreur de charge ALB transmet une demande au prochain serveur en amont.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>
L'équilibreur de charge d'application (ALB) Ingress fait office de proxy entre l'application client et votre application. Certaines configurations d'application nécessitent plusieurs serveurs en amont qui traitent les demandes client entrantes à partir de l'équilibreur de charge ALB. Parfois, le serveur proxy utilisé par l'équilibreur de charge ALB ne parvient pas à établir une connexion à un serveur en amont utilisé par l'application. L'équilibreur de charge ALB peut alors essayer d'établir une connexion avec le prochain serveur en amont pour lui transmettre la demande à la place. Vous pouvez utiliser l'annotation `proxy-next-upstream-config` pour définir dans quels cas, pour combien de temps et combien de fois l'équilibreur de charge ALB peut tenter de transmettre une demande au prochain serveur en amont.<br><br><strong>Remarque</strong> : un délai d'attente est toujours configuré lorsque vous utilisez l'annotation `proxy-next-upstream-config`, par conséquent, n'ajoutez pas le paramètre `timeout=true` dans cette annotation.
</dd>
<dt>Exemple de fichier YAML de ressource Ingress</dt>
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
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service Kubernetes que vous avez créé pour votre application.</td>
</tr>
<tr>
<td><code>retries</code></td>
<td>Remplacez <code>&lt;<em>tries</em>&gt;</code> par le nombre maximal de tentatives de l'équilibreur de charge ALB pour transmettre une demande au prochain serveur en amont. Ce nombre inclut la demande d'origine. Pour désactiver cette limitation, utilisez <code>0</code>. Si vous n'indiquez pas de valeur, la valeur par défaut <code>0</code> est utilisée.
</td>
</tr>
<tr>
<td><code>timeout</code></td>
<td>Remplacez <code>&lt;<em>time</em>&gt;</code> par la durée maximale (en secondes) allouée à l'équilibreur de charge ALB pour tenter de transmettre une demande au prochain serveur en amont. Par exemple, pour définir une durée de 30 secondes, entrez <code>30s</code>. Pour désactiver cette limitation, utilisez <code>0</code>. Si vous n'indiquez pas de valeur, la valeur par défaut <code>0</code> est utilisée.
</td>
</tr>
<tr>
<td><code>error</code></td>
<td>Si la valeur est <code>true</code>, l'équilibreur de charge ALB transmet une demande au prochain serveur en amont en cas d'erreur lors de l'établissement d'une connexion avec le premier serveur en amont, en lui transférant la demande ou en lisant l'en-tête de réponse.
</td>
</tr>
<tr>
<td><code>invalid_header</code></td>
<td>Si la valeur est <code>true</code>, l'équilibreur de charge ALB transmet une demande au prochain serveur en amont lorsque le premier serveur en amont renvoie une réponse vide ou non valide.
</td>
</tr>
<tr>
<td><code>http_502</code></td>
<td>Si la valeur est <code>true</code>, l'équilibreur de charge ALB transmet une demande au prochain serveur en amont lorsque le premier serveur en amont renvoie une réponse avec le code 502. Vous pouvez considérer les codes de réponse HTTP suivants : <code>500</code>, <code>502</code>, <code>503</code>, <code>504</code>, <code>403</code>, <code>404</code>, <code>429</code>.
</td>
</tr>
<tr>
<td><code>non_idempotent</code></td>
<td>Si la valeur est <code>true</code>, l'équilibreur de charge ALB peut transmettre des demandes avec une méthode 'non-idempotent' au prochain serveur en amont. Par défaut, l'équilibreur de charge ALB ne transmet pas ces demandes au prochain serveur en amont.
</td>
</tr>
<tr>
<td><code>off</code></td>
<td>Pour empêcher l'équilibreur de charge ALB de transmettre les demandes au prochain serveur en amont, définissez ce paramètre avec la valeur <code>true</code>.
</td>
</tr>
</tbody></table>
</dd>
</dl>

<br />


### Affinité de session avec des cookies (sticky-cookie-services)
{: #sticky-cookie-services}

Utilisez l'annotation sticky cookie pour ajouter une affinité de session à votre équilibreur de charge ALB et toujours acheminer le trafic réseau entrant au même serveur en amont.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>Pour la haute disponibilité, certaines configurations d'application nécessitent de déployer plusieurs serveurs en amont qui prennent en charge les demandes client entrantes. Lorsqu'un client se connecte à votre application de back end, vous pouvez utiliser l'affinité de session pour qu'un client soit servi par le même serveur en amont pendant toute la durée d'une session ou pendant la durée d'exécution d'une tâche. Vous pouvez configurer votre équilibreur de charge ALB pour assurer une affinité de session en acheminant toujours le trafic réseau entrant au même serveur en amont.

</br></br>
Chaque client qui se connecte à votre application de back end est affecté par l'équilibreur de charge d'application (ALB) à l'un des serveurs en amont disponibles. L'équilibreur de charge ALB crée un cookie de session stocké dans l'application du client et inclus dans les informations d'en-tête de chaque demande entre l'équilibreur de charge ALB et le client. Les informations contenues dans le cookie garantissent la prise en charge de toutes les demandes par le même serveur en amont pendant toute la session.

</br></br>
Lorsque vous incluez plusieurs services, utilisez un point-virgule (;) pour les séparer.</dd>
<dt>Exemple de fichier YAML de ressource Ingress</dt>
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
  <caption>Description des composants de l'annotation</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service Kubernetes que vous avez créé pour votre application.</td>
  </tr>
  <tr>
  <td><code>name</code></td>
  <td>Remplacez <code>&lt;<em>cookie_name</em>&gt;</code> par le nom d'un cookie d'association (sticky cookie) créé lors d'une session.</td>
  </tr>
  <tr>
  <td><code>expires</code></td>
  <td>Remplacez <code>&lt;<em>expiration_time</em>&gt;</code> par le délai en secondes (s), minutes (m) ou heures (h) avant expiration du cookie d'association. Ce délai est indépendant de l'activité de l'utilisateur. Une fois le cookie arrivé à expiration, il est supprimé par le navigateur Web du client et n'est plus envoyé à l'équilibreur de charge ALB. Par exemple, pour définir un délai d'expiration d'1 seconde, d'1 minute ou d'1 heure, entrez <code>1s</code>, <code>1m</code> ou <code>1h</code>.</td>
  </tr>
  <tr>
  <td><code>path</code></td>
  <td>Remplacez <code>&lt;<em>cookie_path</em>&gt;</code> par le chemin ajouté au sous-domaine Ingress et qui indique pour quels domaines et sous-domaines le cookie est envoyé à l'équilibreur de charge ALB. Par exemple, si votre domaine Ingress est <code>www.myingress.com</code> et que vous souhaitez envoyer le cookie dans chaque demande client, vous devez définir <code>path=/</code>. Si vous souhaitez envoyer le cookie uniquement pour <code>www.myingress.com/myapp</code> et tous ses sous-domaines, vous devez définir <code>path=/myapp</code>.</td>
  </tr>
  <tr>
  <td><code>hash</code></td>
  <td>Remplacez <code>&lt;<em>hash_algorithm</em>&gt;</code> par l'algorithme de hachage qui protège les informations dans le cookie. Seul le type <code>sha1</code> est pris en charge. SHA1 crée une somme hachée basée sur les informations contenues dans le cookie et l'ajoute au cookie. Le serveur peut déchiffrer les informations contenues dans le cookie et vérifier l'intégrité des données.</td>
  </tr>
  </tbody></table>

 </dd></dl>

<br />




### Connexions persistantes en amont (upstream-keepalive)
{: #upstream-keepalive}

Définissez le nombre maximum de connexions persistantes inactives pour un serveur en amont.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>
Définir le nombre maximum de connexions persistantes inactives à un serveur en amont d'un service donné. Le serveur en amont accepte, par défaut, de 64 connexions persistantes inactives.
</dd>


 <dt>Exemple de fichier YAML de ressource Ingress</dt>
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
 <caption>Description des composants de l'annotation</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service Kubernetes que vous avez créé pour votre application.</td>
  </tr>
  <tr>
  <td><code>keepalive</code></td>
  <td>Remplacez <code>&lt;<em>max_connections</em>&gt;</code> par le nombre maximal de connexions persistantes inactives dans le serveur en amont. Valeur par défaut : <code>64</code>. La valeur <code>0</code> désactive les connexions persistantes en amont pour le service spécifié.</td>
  </tr>
  </tbody></table>
  </dd>
  </dl>

<br />




## Annotations d'authentification HTTPS et TLS/SSL
{: #https-auth}

### Authentification {{site.data.keyword.appid_short_notm}} (appid-auth)
{: #appid-auth}

  Utilisez {{site.data.keyword.appid_full_notm}} pour l'authentification auprès de votre application.
  {:shortdesc}

  <dl>
  <dt>Description</dt>
  <dd>
  Authentifier les demandes Web ou HTTP/HTTPS d'API avec {{site.data.keyword.appid_short_notm}}.

  <p>Si vous définissez le type de demande sur <code>web</code>, une demande Web qui contient un jeton d'accès {{site.data.keyword.appid_short_notm}} est validée. En cas d'échec de la validation de jeton, la demande Web est rejetée. Si la demande ne contient pas de jeton d'accès, elle est redirigée vers la page de connexion {{site.data.keyword.appid_short_notm}}. <strong>Remarque</strong> : pour que l'authentification Web {{site.data.keyword.appid_short_notm}} fonctionne, les cookies doivent être activés dans le navigateur de l'utilisateur.</p>

  <p>Si vous définissez le type de requête sur <code>api</code>, une demande API qui contient un jeton d'accès {{site.data.keyword.appid_short_notm}} est validée. Si la demande ne contient pas de jeton d'accès, le message d'erreur <code>401: Unauthorized</code> est renvoyé à l'utilisateur.</p>

  <p>**Remarque** : pour des raisons de sécurité, l'authentification {{site.data.keyword.appid_short_notm}} ne prend en charge que les systèmes de back end avec TLS/SSL activé.</p>
  </dd>
   <dt>Exemple de fichier YAML de ressource Ingress</dt>
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
   <caption>Description des composants de l'annotation</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
    </thead>
    <tbody>
    <tr>
    <td><code>bindSecret</code></td>
    <td>Remplacez <em><code>&lt;bind_secret&gt;</code></em> par la valeur confidentielle (secret) Kubernetes qui stocke la valeur confidentielle de liaison.</td>
    </tr>
    <tr>
    <td><code>espace de nom</code></td>
    <td>Remplacez <em><code>&lt;namespace&gt;</code></em> par l'espace de nom de la valeur confidentielle de liaison. Par défaut, cette zone correspond à l'espace de nom `default`.</td>
    </tr>
    <tr>
    <td><code>requestType</code></td>
    <td>Remplacez <code><em>&lt;request_type&gt;</em></code> par le type de demande que vous voulez envoyer à {{site.data.keyword.appid_short_notm}}. Les valeurs admises sont `web` ou `api`. La valeur par défaut est `api`.</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td>Remplacez <code><em>&lt;myservice&gt;</em></code> par le nom du service Kubernetes que vous avez créé pour votre application. Cette zone est facultative. Si aucun nom de service n'est inclus, l'annotation est activée pour tous les services.  Si un nom de service est inclus, l'annotation est activée uniquement pour ce service. S'il y a plusieurs services, séparez-les par un point-virgule (;).</td>
    </tr>
    </tbody></table>
    </dd>
    <dt>Utilisation</dt>
    <dd>Comme l'application utilise {{site.data.keyword.appid_short_notm}} pour l'authentification, vous devez mettre à disposition une instance {{site.data.keyword.appid_short_notm}}, configurer cette instance avec des URI de redirection valides et générer une valeur confidentielle de liaison.
    <ol>
    <li>Mettez à disposition une instance [{{site.data.keyword.appid_short_notm}}](https://console.bluemix.net/catalog/services/app-id).</li>
    <li>Dans la console de gestion {{site.data.keyword.appid_short_notm}}, ajoutez des URI de redirection pour votre application.</li>
    <li>Créez une valeur confidentielle de liaison.
    <pre class="pre"><code>bx cs cluster-service-bind &lt;my_cluster&gt; &lt;my_namespace&gt; &lt;my_service_instance_GUID&gt;</code></pre> </li>
    <li>Configurez l'annotation <code>appid-auth</code>.</li>
    </ol></dd>
    </dl>

<br />



### Ports HTTP et HTTPS personnalisés (custom-port)
{: #custom-port}

Modifiez les ports par défaut pour le trafic réseau HTTP (port 80) et HTTPS (port 443).
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>Par défaut, l'équilibreur de charge d'application (ALB) Ingress est configuré pour écouter le trafic réseau HTTP entrant sur le port 80 et pour le trafic réseau HTTPS entrant sur le port 443. Vous pouvez modifier les ports par défaut pour renforcer la sécurité de votre domaine ALB ou pour activer uniquement un port HTTPS.
</dd>


<dt>Exemple de fichier YAML de ressource Ingress</dt>
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
<caption>Description des composants de l'annotation</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;protocol&gt;</code></td>
 <td>Entrez <code>http</code> ou <code>https</code> pour changer le port par défaut pour le trafic réseau HTTP ou HTTPS entrant.</td>
 </tr>
 <tr>
 <td><code>&lt;port&gt;</code></td>
 <td>Entrez le numéro de port que vous désirez utiliser pour le trafic réseau HTTP ou HTTPS entrant.  <p><strong>Remarque :</strong> lorsqu'un port personnalisé est indiqué pour HTTP ou HTTPS, les ports par défaut ne sont plus valides à la fois pour HTTP et HTTPS. Par exemple, pour remplacer le port par défaut pour HTTPS par 8443, mais utiliser le port par défaut pour HTTP, vous devez définir des ports personnalisés pour les deux ports : <code>custom-port: "protocol=http port=80; protocol=https port=8443"</code>.</p></td>
 </tr>
 </tbody></table>

 </dd>
 <dt>Utilisation</dt>
 <dd><ol><li>Passez en revue les ports ouverts pour votre équilibreur de charge ALB.
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
La sortie de votre interface CLI sera similaire à ceci :
<pre class="screen">
<code>NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx 80:30416/TCP,443:32668/TCP   109d</code></pre></li>
<li>Ouvrez la mappe de configuration ALB.
<pre class="pre">
<code>kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system</code></pre></li>
<li>Ajoutez les ports HTTP et HTTPS différents des valeurs par défaut à la mappe de configuration (ConfigMap). Remplacez &lt;port&gt; par le port HTTP ou HTTPS que vous souhaitez ouvrir. <b>Remarque</b> : par défaut, les ports 80 et 443 sont ouverts. Pour conserver les ports 80 et 443 ouverts, vous devez également les inclure en plus des autres ports TCP que vous indiquez dans la zone `public-ports`. Si vous avez activé un équilibreur de charge ALB privé, vous devez également spécifier tous les ports que vous souhaitez garder ouverts dans la zone `private-ports`. Pour plus d'informations, voir <a href="cs_ingress.html#opening_ingress_ports">Ouverture de ports dans l'équilibreur de charge d'application (ALB) Ingress</a>.
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
 <li>Vérifiez que votre équilibreur de charge ALB est reconfiguré avec d'autres ports que les ports par défaut.
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
La sortie de votre interface CLI sera similaire à ceci :
<pre class="screen">
<code>NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx  169.xx.xxx.xxx &lt;port1&gt;:30776/TCP,&lt;port2&gt;:30412/TCP   109d</code></pre></li>
<li>Configurez Ingress pour utiliser les ports différents des ports par défaut lorsque vous routez le trafic réseau entrant vers vos services. Utilisez l'exemple de fichier YAML dans cette référence. </li>
<li>Mettez à jour la configuration de votre équilibreur de charge ALB.
<pre class="pre">
<code>        kubectl apply -f myingress.yaml
        </code></pre>
</li>
<li>Ouvrez le navigateur Web de votre choix pour accéder à votre application. Exemple : <code>https://&lt;ibmdomain&gt;:&lt;port&gt;/&lt;service_path&gt;/</code></li></ol></dd></dl>

<br />



### Redirection HTTP vers HTTPS (redirect-to-https)
{: #redirect-to-https}

Convertissez les demandes HTTP non sécurisées en demandes HTTPS.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>Configurer votre équilibreur de charge d'application (ALB) Ingress pour sécuriser votre domaine avec le certificat TLS fourni par IBM ou votre certificat TLS personnalisé. Certains utilisateurs peuvent tenter d'accéder à vos applications en utilisant une demande <code>http</code> non sécurisée vers votre domaine ALB (par exemple, <code>http://www.myingress.com</code> au lieu de d'utiliser <code>https</code>). Vous pouvez utiliser l'annotation redirect pour convertir systématiquement en HTTPS les demandes HTTP non sécurisées. Si vous n'utilisez pas cette annotation, les demandes HTTP non sécurisées ne sont pas converties par défaut en demandes HTTPS et peuvent exposer au public des informations confidentielles non chiffrées.

</br></br>
La redirection de demandes HTTP en demandes HTTPS est désactivée par défaut.</dd>

<dt>Exemple de fichier YAML de ressource Ingress</dt>
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


### Dispositif de sécurité HSTS (HTTP Strict Transport Security)
{: #hsts}

<dl>
<dt>Description</dt>
<dd>
Le dispositif de sécurité HSTS oblige le navigateur à n'accéder à un domaine qu'en utilisant HTTPS. Même si l'utilisateur saisit ou suit un lien HTTP normal, le navigateur met à niveau la connexion systématiquement vers HTTPS.
</dd>


<dt>Exemple de fichier YAML de ressource Ingress</dt>
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
<caption>Description des composants de l'annotation</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
  </thead>
  <tbody>
  <tr>
  <td><code>enabled</code></td>
  <td>Utilisez <code>true</code> pour activer HSTS.</td>
  </tr>
    <tr>
  <td><code>maxAge</code></td>
  <td>Remplacez <code>&lt;<em>31536000</em>&gt;</code> par un nombre entier représentant le nombre de secondes nécessaire à un navigateur pour mettre en cache les demandes d'envoi directement sur HTTPS. La valeur par défaut est <code>31536000</code>, ce qui équivaut à 1 an.</td>
  </tr>
  <tr>
  <td><code>includeSubdomains</code></td>
  <td>Utilisez <code>true</code> pour demander au navigateur que la politique HSTS s'applique également à tous les sous-domaines du domaine en cours. La valeur par défaut est <code>true</code>. </td>
  </tr>
  </tbody></table>

  </dd>
  </dl>


<br />


### Authentification mutuelle (mutual-auth)
{: #mutual-auth}

Configurez l'authentification mutuelle pour l'équilibreur de charge d'application (ALB).
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>
Configurer l'authentification mutuelle du trafic en aval pour l'équilibreur de charge d'application (ALB) Ingress. Le client externe authentifie le serveur et le serveur authentifie également le client à l'aide de certificats. L'authentification mutuelle est également appelée authentification basée sur des certificats ou authentification bidirectionnelle.
</dd>

<dt>Conditions prérequises</dt>
<dd>
<ul>
<li>[Vous devez disposer d'une valeur confidentielle valide contenant l'autorité de certification (CA) requise](cs_app.html#secrets). Les valeurs <code>client.key</code> et <code>client.crt</code> sont également nécessaires pour une authentification via l'authentification mutuelle.</li>
<li>Pour activer l'authentification mutuelle sur un autre port que le port 443, [configurez l'équilibreur de charge ALB de manière à ouvrir un port valide](cs_ingress.html#opening_ingress_ports).</li>
</ul>
</dd>


<dt>Exemple de fichier YAML de ressource Ingress</dt>
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
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>secretName</code></td>
<td>Remplacez <code>&lt;<em>mysecret</em>&gt;</code> par le nom de la ressource confidentielle.</td>
</tr>
<tr>
<td><code>port</code></td>
<td>Remplacez <code>&lt;<em>port</em>&gt;</code> par le numéro de port de l'équilibreur de charge ALB.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Remplacez <code>&lt;<em>servicename</em>&gt;</code> par le nom d'une ou de plusieurs ressources Ingress. Ce paramètre est facultatif.</td>
</tr>
</tbody></table>

</dd>
</dl>

<br />



### Support des services SSL (ssl-services)
{: #ssl-services}

Autorisez les demandes HTTPS et chiffrez le trafic vers vos applications en amont.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>
Chiffrer le trafic envoyé par Ingress aux applications en amont qui nécessitent HTTPS. Si vos applications en amont sont compatibles avec TLS, vous pouvez éventuellement fournir un certificat inclus dans une valeur confidentielle TLS.<br></br>**Facultatif** : vous pouvez ajouter [l'authentification unidirectionnelle ou l'authentification mutuelle](#ssl-services-auth) à cette annotation.</dd>


<dt>Exemple de fichier YAML de ressource Ingress</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: &lt;nom_ingress&gt;
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
<caption>Description des composants de l'annotation</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
  </thead>
  <tbody>
  <tr>
  <td><code>ssl-service</code></td>
  <td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service qui nécessite HTTPS. Le trafic est chiffré de l'équilibreur de charge ALB jusqu'au service de cette application.</td>
  </tr>
  <tr>
  <td><code>ssl-secret</code></td>
  <td>Facultatif : si vous voulez utiliser une valeur confidentielle TLS et que votre application en amont est compatible avec TLS, remplacez <code>&lt;<em>service-ssl-secret</em>&gt;</code> par la valeur confidentielle utilisée pour le service. Si vous fournissez une valeur confidentielle, cette valeur doit contenir les éléments <code>trusted.crt</code>, <code>client.crt</code> et <code>client.key</code> que votre application attend du client. Pour créer une valeur confidentielle TLS, commencez par [convertir les certificats et la clé en base 64 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.base64encode.org/). Puis consultez la rubrique [Création de valeurs confidentielles](cs_app.html#secrets).</td>
  </tr>
  </tbody></table>

  </dd>
</dl>

<br />


#### Support des services SSL avec authentification
{: #ssl-services-auth}

<dl>
<dt>Description</dt>
<dd>
Autoriser les demandes HTTPS et chiffrer le trafic vers vos applications en amont avec authentification unidirectionnelle ou mutuelle pour une sécurité renforcée.
</dd>


<dt>Exemple de fichier YAML de ressource Ingress</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: &lt;nom_ingress&gt;
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
<caption>Description des composants de l'annotation</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
  </thead>
  <tbody>
  <tr>
  <td><code>ssl-service</code></td>
  <td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service qui nécessite HTTPS. Le trafic est chiffré de l'équilibreur de charge ALB jusqu'au service de cette application.</td>
  </tr>
  <tr>
  <td><code>ssl-secret</code></td>
  <td>Remplacez <code>&lt;<em>service-ssl-secret</em>&gt;</code> par la valeur confidentielle d'authentification mutuelle pour le service. Cette valeur doit contenir le certificat de l'autorité de certification que votre application attend du client. Pour créer une valeur confidentielle d'authentification mutuelle, commencez par [convertir le certificat et la clé en base 64 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.base64encode.org/). Puis consultez la rubrique [Création de valeurs confidentielles](cs_app.html#secrets).</td>
  </tr>
  </tbody></table>

  </dd>
  </dl>

<br />


## Annotations Istio
{: #istio-annotations}

### Services Istio (istio-services)
{: #istio-services}

  Acheminez le trafic vers des services gérés par Istio.
  {:shortdesc}

  <dl>
  <dt>Description</dt>
  <dd>
  Si vous disposez de services gérés par Istio, vous pouvez utiliser un équilibreur de charge ALB de cluster pour acheminer les demandes HTTP/HTTPS vers le contrôleur Ingress Istio. Ce contrôleur achemine ensuite les demandes aux services d'application. Pour acheminer le trafic, vous devez modifier les ressources Ingress pour l'équilibreur de charge ALB de cluster et le contrôleur Ingress Istio.
    <br><br>Dans la ressource Ingress de l'équilibreur de charge ALB de cluster, vous devez :
      <ul>
        <li>spécifier l'annotation `istio-services`</li>
        <li>définir le chemin d'accès au service comme chemin réel sur lequel l'application est à l'écoute</li>
        <li>définir le port du service comme port du contrôleur Ingress Istio</li>
      </ul>
    <br>Dans la ressource Ingress du contrôleur Ingress Istio, vous devez :
      <ul>
        <li>définir le chemin d'accès au service comme chemin réel sur lequel l'application est à l'écoute</li>
        <li>définir le port du service comme port HTTP/HTTPS du service d'application exposé par le contrôleur Ingress Istio</li>
    </ul>
  </dd>

   <dt>Exemple de fichier YAML de ressource Ingress pour l'équilibreur de charge ALB de cluster</dt>
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
   <caption>Description des composants du fichier YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>enabled</code></td>
      <td>Pour activer le routage du trafic vers des services gérés par Istio, définissez la valeur <code>True</code>.</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td>Remplacez <code><em>&lt;myservice1&gt;</em></code> par le nom du service Kubernetes que vous avez créé pour votre application gérée par Istio. S'il y a plusieurs services, séparez-les par un point-virgule (;). Cette zone est facultative. Si vous n'indiquez pas de nom de service, tous les services gérés par Istio sont activés pour le routage du trafic.</td>
    </tr>
    <tr>
    <td><code>istioServiceNamespace</code></td>
    <td>Remplacez <code><em>&lt;istio-namespace&gt;</em></code> par l'espace de nom Kubernetes sur lequel est installé Istio. Cette zone est facultative. Si vous n'indiquez pas d'espace de nom, l'espace de nom <code>istio-system</code> est utilisé.</td>
    </tr>
    <tr>
    <td><code>istioServiceName</code></td>
    <td>Remplacez <code><em>&lt;istio-ingress-service&gt;</em></code> par le nom du service Ingress Istio. Cette zone est facultative. Si vous n'indiquez pas de nom de service Istio, le nom de service <code>istio-ingress</code> est utilisé.</td>
    </tr>
    <tr>
    <td><code>path</code></td>
      <td>Pour chaque service géré par Istio vers lequel vous souhaitez acheminer le trafic, remplacez <code><em>&lt;/myapp1&gt;</em></code> par le chemin de l'application de back end sur lequel le service géré par Istio est à l'écoute. Le chemin doit correspondre au chemin que vous avez défini dans la ressource Ingress Istio.</td>
    </tr>
    <tr>
    <td><code>servicePort</code></td>
    <td>Pour chaque service géré par Istio vers lequel vous souhaitez acheminer le trafic, remplacez <code><em>&lt;istio_ingress_port&gt;</em></code> par le port du contrôleur Ingress Istio.</td>
    </tr>
    </tbody></table>
    </dd>
    </dl>

## Annotations de mémoire tampon de proxy
{: #proxy-buffer}


### Mise en mémoire tampon des données de réponse client (proxy-buffering)
{: #proxy-buffering}

Utilisez l'annotation de mémoire tampon pour désactiver le stockage de données de réponse sur l'équilibreur de charge d'application (ALB) tandis que les données sont envoyées au client.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>L'équilibreur de charge d'application (ALB) Ingress fait office de proxy entre votre application de back end et le navigateur Web du client. Lorsqu'une réponse est envoyée de l'application de back end au client, les données de réponse sont placées par défaut en mémoire tampon sur l'ALB par défaut. L'équilibreur de charge ALB met en proxy la réponse client et commence à envoyer la réponse au client à la cadence du client. Une fois que toutes les données de l'application de back end ont été reçues par l'équilibreur de charge ALB, la connexion à l'application de back end est clôturée. La connexion de l'équilibreur de charge ALB au client reste ouverte jusqu'à ce que le client ait reçu toutes les données.

</br></br>
Si la mise en mémoire tampon des données de réponse sur l'équilibreur de charge d'application (ALB) est désactivée, les données sont envoyées immédiatement de l'ALB au client. Le client doit être capable de traiter les données entrantes au rythme de l'ALB. Si le client est trop lent, les données risquent d'être perdues.
</br></br>
La mise en mémoire tampon des données de réponse sur l'équilibreur de charge ALB est activée par défaut.</dd>
<dt>Exemple de fichier YAML de ressource Ingress</dt>
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
<caption>Description des composants de l'annotation</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
 </thead>
 <tbody>
 <tr>
 <td><code>enabled</code></td>
   <td>Pour désactiver la mise en mémoire tampon des données de réponse sur l'équilibreur de charge ALB, définissez cette option avec <code>false</code>.</td>
 </tr>
 <tr>
 <td><code>serviceName</code></td>
 <td>Remplacez <code><em>&lt;myservice1&gt;</em></code> par le nom du service Kubernetes que vous avez créé pour votre application. S'il y a plusieurs services, séparez-les par un point-virgule (;). Cette zone est facultative. Si vous n'indiquez pas de nom de service, cette annotation est utilisée par tous les services.</td>
 </tr>
 </tbody></table>
 </dd>
 </dl>

<br />



### Mémoires tampons de proxy (proxy-buffers)
{: #proxy-buffers}

Configurez le nombre et la taille des mémoires tampons de proxy pour l'équilibreur de charge d'application (ALB).
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>
Définir le nombre et la taille des mémoires tampons qui lisent une réponse pour une connexion unique provenant du serveur relayé via un proxy. Si aucun service n'est spécifié, la configuration est appliquée à tous les services de l'hôte Ingress. Par exemple, si une configuration telle que <code>serviceName=SERVICE number=2 size=1k</code> est spécifiée, 1k est appliqué au service. Si une configuration telle que <code>number=2 size=1k</code> est spécifiée, 1k est appliqué à tous les services de l'hôte Ingress.
</dd>
<dt>Exemple de fichier YAML de ressource Ingress</dt>
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
<caption>Description des composants de l'annotation</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
 </thead>
 <tbody>
 <tr>
 <td><code>serviceName</code></td>
 <td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service auquel appliquer proxy-buffers.</td>
 </tr>
 <tr>
 <td><code>number</code></td>
 <td>Remplacez <code>&lt;<em>number_of_buffers</em>&gt;</code> par un nombre. Par exemple, <em>2</em>.</td>
 </tr>
 <tr>
 <td><code>size</code></td>
 <td>Remplacez <code>&lt;<em>size</em>&gt;</code> par la taille de chaque mémoire tampon, en kilooctets (k ou K). Par exemple, <em>1K</em>.</td>
 </tr>
 </tbody>
 </table>
 </dd></dl>

<br />


### Taille de mémoire tampon de proxy (proxy-buffer-size)
{: #proxy-buffer-size}

Définissez la taille de la mémoire tampon de proxy qui lit la première partie de la réponse.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>
Définir la taille de la mémoire tampon qui lit la première partie de la réponse reçue du serveur relayé via un proxy. Cette partie de la réponse contient généralement un petit en-tête de réponse. Si aucun service n'est spécifié, la configuration est appliquée à tous les services de l'hôte Ingress. Par exemple, si une configuration telle que <code>serviceName=SERVICE size=1k</code> est spécifiée, 1k est appliqué au service. Si une configuration telle que <code>size=1k</code> est spécifiée, 1k est appliqué à tous les services de l'hôte Ingress.
</dd>


<dt>Exemple de fichier YAML de ressource Ingress</dt>
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
<caption>Description des composants de l'annotation</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
 </thead>
 <tbody>
 <tr>
 <td><code>serviceName</code></td>
 <td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service auquel appliquer proxy-buffers-size.</td>
 </tr>
 <tr>
 <td><code>size</code></td>
 <td>Remplacez <code>&lt;<em>size</em>&gt;</code> par la taille de chaque mémoire tampon, en kilooctets (k ou K). Par exemple, <em>1K</em>.</td>
 </tr>
 </tbody></table>

 </dd>
 </dl>

<br />



### Taille des mémoires tampons occupées de proxy (proxy-busy-buffers-size)
{: #proxy-busy-buffers-size}

Configurez la taille des mémoires tampons de proxy pouvant être occupées.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>
Limiter la taille des mémoires tampons pouvant envoyer une réponse au client alors que la réponse n'a pas été encore complètement lue. Dans l'intervalle, le reste des mémoires tampons peut lire la réponse et, au besoin, placer en mémoire tampon une partie de la réponse dans un fichier temporaire. Si aucun service n'est spécifié, la configuration est appliquée à tous les services de l'hôte Ingress. Par exemple, si une configuration telle que <code>serviceName=SERVICE size=1k</code> est spécifiée, 1k est appliqué au service. Si une configuration telle que <code>size=1k</code> est spécifiée, 1k est appliqué à tous les services de l'hôte Ingress.
</dd>


<dt>Exemple de fichier YAML de ressource Ingress</dt>
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
<caption>Description des composants de l'annotation</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service auquel appliquer proxy-busy-buffers-size.</td>
</tr>
<tr>
<td><code>size</code></td>
<td>Remplacez <code>&lt;<em>size</em>&gt;</code> par la taille de chaque mémoire tampon, en kilooctets (k ou K). Par exemple, <em>1K</em>.</td>
</tr>
</tbody></table>

 </dd>
 </dl>

<br />



## Annotations de demande et de réponse
{: #request-response}


### En-tête de réponse ou de demande client supplémentaire (proxy-add-headers, response-add-headers)
{: #proxy-add-headers}

Ajoutez des informations d'en-tête à une demande client avant d'envoyer la demande vers l'application de back end ou à une réponse client avant d'envoyer la réponse au client.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>L'équilibreur de charge d'application (ALB) Ingress fait office de proxy entre l'application client et l'application de back end. Les demandes client envoyées à l'équilibreur de charge ALB sont traitées (relayées via un proxy) et placées dans une nouvelle demande, envoyée ensuite à votre application de back end. De la même manière, les réponses de l'application de back end sont envoyées à l'équilibreur de charge ALB sont traitées (relayées via un proxy) et placées dans une nouvelle réponse qui est ensuite envoyée au client. Lorsqu'une demande ou une réponse passe par un proxy, les informations d'en-tête HTTP initialement envoyées par le client ou l'application de back end, par exemple, le nom d'utilisateur, sont retirées.

<br><br>
Si votre application de back end a besoin des informations d'en-tête HTTP, vous pouvez utiliser l'annotation <code>proxy-add-headers</code> pour ajouter des informations d'en-tête à la demande client avant son envoi par l'équilibreur de charge ALB à l'application de back end.

<br>
<ul><li>Par exemple, vous devrez peut-être ajouter les informations d'en-tête X-Forward suivantes à la demande avant son transfert vers votre application :

<pre class="screen">
<code>proxy_set_header Host $host;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;</code></pre>

</li>

<li>Pour ajouter les informations d'en-tête X-Forward à la demande envoyée à votre application, utilisez l'annotation `proxy-add-headers` comme suit :

<pre class="screen">
<code>ingress.bluemix.net/proxy-add-headers: |
  serviceName=<myservice1> {
  Host $host;
  X-Forwarded-Proto $scheme;
  X-Forwarded-For $proxy_add_x_forwarded_for;
  }</code></pre>

</li></ul><br>

Si l'application Web du client nécessite des informations d'en-tête HTTP, vous pouvez utiliser l'annotation <code>response-add-headers</code> pour ajouter les informations d'en-tête à la réponse avant son envoi par l'équilibreur de charge ALB à l'application Web du client.</dd>

<dt>Exemple de fichier YAML de ressource Ingress</dt>
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
 <caption>Description des composants de l'annotation</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
  </thead>
  <tbody>
  <tr>
  <td><code>service_name</code></td>
  <td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service Kubernetes que vous avez créé pour votre application.</td>
  </tr>
  <tr>
  <td><code>&lt;header&gt;</code></td>
  <td>Clé des informations d'en-tête à ajouter à la demande ou à la réponse du client.</td>
  </tr>
  <tr>
  <td><code>&lt;value&gt;</code></td>
  <td>Valeur des informations d'en-tête à ajouter à la demande ou à la réponse du client.</td>
  </tr>
  </tbody></table>
 </dd></dl>

<br />



### Retrait d'en-tête de réponse client (response-remove-headers)
{: #response-remove-headers}

Retirez de l'application de back end les informations d'en-tête incluses dans la réponse client, avant l'envoi de la réponse au client.
 {:shortdesc}

 <dl>
 <dt>Description</dt>
 <dd>L'équilibreur de charge d'application (ALB) Ingress fait office de proxy entre votre application de back end et le navigateur Web du client. Les réponses client de l'application de back end qui sont envoyées à l'équilibreur de charge ALB (relayées via proxy) sont traitées et placées dans une nouvelle réponse qui est alors envoyée de l'ALB au navigateur Web du client. Même si le fait de relayer une réponse via un proxy permet de retirer les informations d'en-tête HTTP initialement envoyées à partir de l'application de back end, il se peut que les en-têtes propres à l'application de back end ne soient pas tous retirés par ce processus. Supprimez les informations d'en-tête d'une réponse client avant que celle-ci ne soit envoyée de l'équilibreur de charge ALB au navigateur Web du client.</dd>
 <dt>Exemple de fichier YAML de ressource Ingress</dt>
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
  <caption>Description des composants de l'annotation</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
   </thead>
   <tbody>
   <tr>
   <td><code>service_name</code></td>
   <td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service Kubernetes que vous avez créé pour votre application.</td>
   </tr>
   <tr>
   <td><code>&lt;header&gt;</code></td>
   <td>Clé de l'en-tête à retirer de la réponse du client.</td>
   </tr>
   </tbody></table>
   </dd></dl>

<br />


### Taille du corps de demande client (client-max-body-size)
{: #client-max-body-size}

Définissez la taille maximale du corps que le client peut envoyer dans le cadre d'une demande.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>Dans le but de maintenir les performances attendues, la valeur définie pour la taille maximale du corps de la demande client est 1 mégaoctet. Lorsqu'une demande client dont la taille du corps dépasse la limite est envoyée à l'équilibreur de charge ALB Ingress et que le client n'autorise pas la scission des données, l'équilibreur de charge ALB renvoie une réponse HTTP 413 (Entité de demande trop volumineuse) au client. Une connexion entre le client et l'équilibreur de charge ALB n'est pas possible tant que le corps de la demande n'a pas été réduit. Lorsque le client autorise le fractionnement des données en plusieurs blocs, les données sont divisées en packages de 1 mégaoctet et envoyées à l'équilibreur de charge ALB.

</br></br>
Vous souhaiterez peut-être augmenter la taille maximale du corps car vous attendez des demandes client avec une taille de corps supérieure à 1 mégaoctet. Par exemple, vous voulez que votre client puisse télécharger des fichiers volumineux. L'augmentation de la taille de corps de demande maximale peut avoir une incidence sur les performances de votre équilibreur de charge ALB puisque la connexion au client doit rester ouverte jusqu'à ce que la demande ait été reçue.
</br></br>
<strong>Remarque :</strong> certains navigateurs Web client ne peuvent pas afficher correctement le message de réponse HTTP 413.</dd>
<dt>Exemple de fichier YAML de ressource Ingress</dt>
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
<caption>Description des composants de l'annotation</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;size&gt;</code></td>
 <td>Taille maximale du corps de réponse client. Par exemple, pour définir la taille maximale à 200 mégaoctets, indiquez <code>200m</code>.  <strong>Remarque :</strong> vous pouvez définir la taille 0 pour désactiver la vérification de la taille du corps de demande client.</td>
 </tr>
 </tbody></table>

 </dd></dl>

<br />


### Mémoires tampon d'en-têtes de demande client volumineux (large-client-header-buffers)
{: #large-client-header-buffers}

Définissez le nombre et la taille maximum des mémoires tampons qui lisent des en-têtes de demande client volumineux.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>Les mémoires tampons qui lisent des en-têtes de demande client volumineux sont attribuées uniquement à la demande : si une connexion passe à l'état keepalive après le traitement de la fin de la demande (end-of-request), ces mémoires tampons sont libérées. Par défaut, la taille de la mémoire tampon est égale à <code>8K</code> octets. Si une ligne de demande dépasse la taille maximale d'une mémoire tampon, le message d'erreur <code>414 Request-URI Too Large</code> est renvoyé au client. Par ailleurs, si la zone d'en-tête de demande dépasse la taille maximale d'une mémoire tampon, l'erreur <code>400 Bad Request</code> est renvoyée au client. Vous pouvez ajuster le nombre et la taille maximum des mémoires tampons utilisées pour la lecture des en-têtes de demande client volumineux.

<dt>Exemple de fichier YAML de ressource Ingress</dt>
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
<caption>Description des composants de l'annotation</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;number&gt;</code></td>
 <td>Nombre maximal de mémoires tampons à attribuer pour lire les en-têtes de demande client volumineux. Par exemple, pour le définir à 4, définissez <code>4</code>.</td>
 </tr>
 <tr>
 <td><code>&lt;size&gt;</code></td>
 <td>Taille maximale des mémoires tampons utilisées pour la lecture des en-têtes de demande client volumineux. Par exemple, pour la définir à 16 kilooctets, indiquez <code>16k</code>.
   <strong>Remarque :</strong> la taille doit se terminer par <code>k</code> pour kilooctet ou <code>m</code> pour mégaoctet.</td>
 </tr>
</tbody></table>
</dd>
</dl>

<br />


## Annotations de limites de service
{: #service-limit}


### Limites de débit globales (global-rate-limit)
{: #global-rate-limit}

Limitez le débit de traitement des demandes et le nombre de connexions par une clé définie pour tous les services.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>
Pour tous les services, permet par une clé définie de limiter le débit de traitement des demandes et le nombre de connexions émanant d'une même adresse IP pour tous les chemins de back end sélectionnés.
</dd>


 <dt>Exemple de fichier YAML de ressource Ingress</dt>
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
 <caption>Description des composants de l'annotation</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
  </thead>
  <tbody>
  <tr>
  <td><code>key</code></td>
  <td>Pour définir une limite globale pour les demandes entrantes en fonction de l'emplacement ou du service, utilisez `key=location`. Pour définir une limite globale pour les demandes entrantes en fonction de l'en-tête, utilisez `X-USER-ID key=$http_x_user_id`.</td>
  </tr>
  <tr>
  <td><code>rate</code></td>
  <td>Remplacez <code>&lt;<em>rate</em>&gt;</code> par le débit de traitement. Entrez une valeur correspondant à un débit par seconde (r/s) ou à un débit par minute (r/m). Exemple : <code>50r/m</code>.</td>
  </tr>
  <tr>
  <td><code>number-of_connections</code></td>
  <td>Remplacez <code>&lt;<em>conn</em>&gt;</code> par le nombre de connexions.</td>
  </tr>
  </tbody></table>

  </dd>
  </dl>

<br />



### Limites de débit de service (service-rate-limit)
{: #service-rate-limit}

Limitez le débit de traitement des demandes et le nombre de connexions pour des services spécifiques.
{:shortdesc}

<dl>
<dt>Description</dt>
<dd>Pour des services spécifiques, permet de limiter le débit de traitement des demandes et le nombre de connexions par une clé définie émanant d'une même adresse IP pour tous les chemins des back end sélectionnés.
</dd>


 <dt>Exemple de fichier YAML de ressource Ingress</dt>
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
 <caption>Description des composants de l'annotation</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de l'annotation</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td>Remplacez <code>&lt;<em>myservice</em>&gt;</code> par le nom du service pour lequel vous désirez limiter le débit de traitement.</li>
  </tr>
  <tr>
  <td><code>key</code></td>
  <td>Pour définir une limite globale pour les demandes entrantes en fonction de l'emplacement ou du service, utilisez `key=location`. Pour définir une limite globale pour les demandes entrantes en fonction de l'en-tête, utilisez `X-USER-ID key=$http_x_user_id`.</td>
  </tr>
  <tr>
  <td><code>rate</code></td>
  <td>Remplacez <code>&lt;<em>rate</em>&gt;</code> par le débit de traitement. Pour définir un débit par seconde, utilisez r/s: <code>10r/s</code>. Pour définir un débit par minute, utilisez r/m: <code>50r/m</code>.</td>
  </tr>
  <tr>
  <td><code>number-of_connections</code></td>
  <td>Remplacez <code>&lt;<em>conn</em>&gt;</code> par le nombre de connexions.</td>
  </tr>
  </tbody></table>
  </dd>
  </dl>

  <br />



