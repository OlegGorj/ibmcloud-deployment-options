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




# Container auf Grundlage von Images erstellen
{: #images}

Ein Docker-Image ist die Basis für jeden Container, den Sie mit {{site.data.keyword.containerlong}} erstellen.
{:shortdesc}

Ein Image wird auf der Grundlage einer Dockerfile erstellt. Hierbei handelt es sich um eine Datei, die Anweisungen zum Erstellen des Image enthält. Eine Dockerfile referenziert möglicherweise in ihren Anweisungen Buildartefakte, die separat gespeichert sind, z. B. eine App, die Konfiguration der App und ihre Abhängigkeiten.

## Image-Registrys planen
{: #planning}

Images werden in der Regel in einer Registry gespeichert, auf die der Zugriff entweder öffentlich (extern) möglich ist (öffentliche Registry) oder aber so eingerichtet ist, dass der Zugriff auf eine kleine Gruppe von Benutzern beschränkt ist (private Registry).
{:shortdesc}

Öffentliche Registrys wie Docker Hub sind gut dafür geeignet, sich mit Docker und Kubernetes vertraut zu machen und die erste containerisierte App in einem Cluster zu erstellen. Was Unternehmensanwendungen betrifft, so sollten Sie jedoch eine private Registry (wie z. B. die von {{site.data.keyword.registryshort_notm}} bereitgestellte Registry) verwenden,
um zu verhindern, dass Ihre Images durch nicht berechtigte Benutzer verwendet oder unbefugt Änderungen an ihnen vorgenommen werden. Private Registrys müssen vom Clusteradministrator eingerichtet werden, damit sichergestellt ist, dass die Berechtigungsnachweise für den Zugriff auf die private Registry den Clusterbenutzern zur Verfügung stehen.


Für die Bereitstellung Ihrer Apps im Cluster können Sie mehrere Registrys mit {{site.data.keyword.containershort_notm}} verwenden.

|Registry|Beschreibung|Vorteil|
|--------|-----------|-------|
|[{{site.data.keyword.registryshort_notm}}](/docs/services/Registry/index.html)|Mit dieser Option können Sie Ihr eigenes geschütztes Docker-Image-Repository in {{site.data.keyword.registryshort_notm}} einrichten, in dem Sie Images sicher speichern und gemeinsam mit den Clusterbenutzern nutzen können.|<ul><li>Ermöglicht die Verwaltung des Zugriffs auf Images in Ihrem Konto.</li><li>Ermöglicht die Verwendung der von {{site.data.keyword.IBM_notm}} bereitgestellten Images und Beispielapps wie zum Beispiel {{site.data.keyword.IBM_notm}} Liberty als übergeordnetes Image, zu dem eigener App-Code hinzugefügt werden kann.</li><li>Automatisches Scannen der Images durch Vulnerability Advisor auf potenzielle Sicherheitslücken und Bereitstellung betriebssystemspezifischer Empfehlungen, um diese zu schließen.</li></ul>|
|Beliebige andere private Registry|Ermöglicht die Verbindung einer beliebigen vorhandenen privaten Registry mit Ihrem Cluster durch Erstellung eines geheimen Schlüssels vom Typ [imagePullSecret ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/containers/images/). Dieser dient dazu, die URL zur Ihrer Registry und die Berechtigungsnachweise sicher in einem geheimen Kubernetes-Schlüssel zu speichern.|<ul><li>Ermöglicht die Verwendung vorhandener privater Registrys unabhängig von ihrer jeweiligen Quelle (Docker Hub, organisationseigene Registrys oder andere private Cloud-Registrys).</li></ul>|
|[Öffentlicher Docker Hub ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://hub.docker.com/){: #dockerhub}|Verwenden Sie diese Option, um vorhandene öffentliche Images von Docker Hub in Ihrer [Kubernetes-Bereitstellung![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) direkt zu verwenden, wenn keine Dockerfile-Änderungen erforderlich sind. <p>**Hinweis:** Berücksichtigen Sie, dass diese Option möglicherweise den Sicherheitsanforderungen Ihrer Organisation (wie beispielsweise bei der Zugriffsverwaltung, der Ermittlung von Sicherheitslücken oder der Vertraulichkeit von App-Daten) nicht entspricht.</p>|<ul><li>Für Ihren Cluster ist keine zusätzliche Konfiguration erforderlich.</li><li>Enthält eine Vielzahl von Open-Source-Anwendungen.</li></ul>|
{: caption="Optionen für öffentliche und für private Image-Registrys" caption-side="top"}

Nachdem Sie eine Image-Registry eingerichtet haben, können Benutzer diese Images zur Bereitstellung ihrer Apps im Cluster verwenden.

Erfahren Sie mehr über das [Sichern der persönliche Daten](cs_secure.html#pi) bei der Arbeit mit Container-Images.

<br />


## Vertrauenswürdige Inhalte für Container-Images konfigurieren
{: #trusted_images}

Sie können Container aus vertrauenswürdigen Images erstellen, die signiert und in {{site.data.keyword.registryshort_notm}} gespeichert sind, und Bereitstellungen aus nicht signierten oder gefährdeten Images verhindern.
{:shortdesc}

1.  [Images für vertrauenswürdige Inhalte signieren](/docs/services/Registry/registry_trusted_content.html#registry_trustedcontent). Nachdem Sie eine Vertrauensbeziehung für Ihre Images konfiguriert haben, können Sie vertrauenswürdige Inhalte und Unterzeichner, die Images per Push-Operation in Ihre Registry übertragen können, verwalten.
2.  Um eine Richtlinie umzusetzen, die besagt, dass nur signierte Images zum Erstellen von Containern in Ihrem Cluster verwendet werden können, [fügen Sie Container Image Security Enforcement (beta)](/docs/services/Registry/registry_security_enforce.html#security_enforce) hinzu.
3.  Stellen Sie Ihre App bereit.
    1. [Bereitstellung im Kubernetes-Standardnamensbereich (`default`)](#namespace).
    2. [Bereitstellung in einem anderen Kubernetes-Namensbereich oder aus einer anderen {{site.data.keyword.Bluemix_notm}}-Region bzw. einem anderen Konto](#other).

<br />


## Container aus einem {{site.data.keyword.registryshort_notm}}-Image im Kubernetes-Standardnamensbereich (`default`) bereitstellen
{: #namespace}

Sie können Container aus einem von IBM bereitgestellten öffentlichen Image oder einem privaten Image, das in Ihrem Namensbereich in {{site.data.keyword.registryshort_notm}} gespeichert ist, in Ihrem Cluster bereitstellen.
{:shortdesc}

Wenn Sie einen Cluster erstellen, werden automatisch nicht ablaufende Registry-Tokens und geheime Schlüssel für die [nächstgelegene regionale Registry sowie die globale Registry](/docs/services/Registry/registry_overview.html#registry_regions) erstellt. Die globale Registry dient der sicheren Speicherung öffentlicher, von IBM bereitgestellter Images, auf die Sie in allen Ihren Bereitstellungen verweisen können, sodass es nicht notwendig ist, unterschiedliche Verweise für Images zu verwenden, die in den einzelnen regionalen Registrys gespeichert sind. Die regionale Registry dient der sicheren Speicherung Ihrer eigenen privaten Docker-Images sowie derselben öffentlichen Images, die in der globalen Registry gespeichert sind. Mithilfe der Tokens wird Lesezugriff auf alle Namensbereiche autorisiert, die Sie in {{site.data.keyword.registryshort_notm}} einrichten, sodass Sie mit diesen öffentlichen Images (in der globalen Registry) und privaten Images (in den regionalen Registrys) arbeiten können.

Jedes Token muss in einem `imagePullSecret` von Kubernetes gespeichert sein, damit ein Kubernetes-Cluster darauf zugreifen kann, wenn Sie eine containerisierte App bereitstellen. Bei der Erstellung Ihres Clusters werden die Tokens für die globale Registry (mit den von IBM bereitgestellten öffentlichen Images) von {{site.data.keyword.containershort_notm}} automatisch in geheimen Schlüsseln für Image-Pulloperationen (imagePullSecrets) in Kubernetes gespeichert. Diese geheimen Schlüssel für Image-Pulloperationen werden dem `standardmäßigen` Kubernetes-Namensbereich (default), der Standardliste mit geheimen Schlüsseln im Servicekonto (`ServiceAccount`) für diesen Namensbereich und dem Namensbereich `kube-system` hinzugefügt.

**Hinweis:** Bei dieser anfänglichen Konfiguration können Sie Container aus allen Images, die in einem Namensbereich in Ihrem {{site.data.keyword.Bluemix_notm}}-Konto verfügbar sind, im **Standardnamensbereich** Ihres Clusters bereitstellen. Um einen Container in anderen Namensbereichen Ihres Clusters bereitstellen oder ein Image verwenden zu können, das in einer {{site.data.keyword.Bluemix_notm}}-Region oder in einem anderen {{site.data.keyword.Bluemix_notm}}-Konto gespeichert ist, müssen Sie [Ihr eigenes imagePullSecret für Ihren Cluster erstellen](#other).

Vorbemerkungen:
1. [Richten Sie einen Namensbereich in {{site.data.keyword.registryshort_notm}} unter {{site.data.keyword.Bluemix_notm}} Public oder {{site.data.keyword.Bluemix_dedicated_notm}} ein und übertragen Sie Images per Push-Operation an diesen Namensbereich](/docs/services/Registry/registry_setup_cli_namespace.html#registry_namespace_add).
2. [Erstellen Sie einen Cluster](cs_clusters.html#clusters_cli).
3. [Richten Sie Ihre CLI auf Ihren Cluster aus](cs_cli_install.html#cs_cli_configure).

Um einen Container im Standardnamensbereich (**default**) Ihres Clusters bereitzustellen, müssen Sie eine Konfigurationsdatei erstellen.

1.  Erstellen Sie eine Bereitstellungskonfigurationsdatei mit dem Namen `mydeployment.yaml`.
2.  Definieren Sie die Bereitstellung und das Image aus Ihrem Namensbereich, das Sie verwenden möchten, in {{site.data.keyword.registryshort_notm}}.

    Gehen Sie wie folgt vor,
um ein privates Image aus einem Namensbereich in {{site.data.keyword.registryshort_notm}} zu verwenden:

    ```
    apiVersion: apps/v1beta1
    kind: Deployment
    metadata:
      name: ibmliberty-deployment
    spec:
      replicas: 3
      template:
        metadata:
          labels:
            app: ibmliberty
        spec:
          containers:
          - name: ibmliberty
            image: registry.<region>.bluemix.net/<namensbereich>/<mein_image>:<tag>
    ```
    {: codeblock}

    **Tipp:** Führen Sie `bx cr namespace-list` aus, um Ihre Namensbereichsinformationen abzurufen.

3.  Erstellen Sie die Bereitstellung in Ihrem Cluster.

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

    **Tipp:** Sie können auch eine vorhandene Konfigurationsdatei bereitstellen, z. B. eines der von IBM bereitgestellten öffentlichen Images. In diesem Beispiel wird das Image **ibmliberty** in der Region 'Vereinigte Staaten (Süden)' verwendet.

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/deploy-ibmliberty.yaml
    ```
    {: pre}


<br />



## `imagePullSecret` erstellen, um auf {{site.data.keyword.Bluemix_notm}} oder externe private Registrys in anderen Kubernetes-Namensbereichen, {{site.data.keyword.Bluemix_notm}}-Regionen und -Konten zuzugreifen
{: #other}

Erstellen Sie Ihr eigenes `imagePullSecret`, um Container in anderen Kubernetes-Namensbereichen bereitzustellen oder um Images zu verwenden, die in anderen {{site.data.keyword.Bluemix_notm}}-Regionen oder -Konten, in {{site.data.keyword.Bluemix_dedicated_notm}} oder in externen privaten Registrys gespeichert sind.
{:shortdesc}

ImagePullSecrets sind nur für die Namensbereiche gültig, für die sie erstellt wurden. Wiederholen Sie diese Schritte für jeden Namensbereich, in dem Sie Container bereitstellen möchten. Für Images aus [DockerHub](#dockerhub) ist 'ImagePullSecrets' nicht erforderlich.
{: tip}

Vorbemerkungen:

1.  [Richten Sie einen Namensbereich in {{site.data.keyword.registryshort_notm}} unter {{site.data.keyword.Bluemix_notm}} Public oder {{site.data.keyword.Bluemix_dedicated_notm}} ein und übertragen Sie Images per Push-Operation an diesen Namensbereich](/docs/services/Registry/registry_setup_cli_namespace.html#registry_namespace_add).
2.  [Erstellen Sie einen Cluster](cs_clusters.html#clusters_cli).
3.  [Richten Sie Ihre CLI auf Ihren Cluster aus](cs_cli_install.html#cs_cli_configure).

<br/>
Sie haben folgende Optionen, wenn Sie Ihr eigenes imagePullSecret erstellen möchten:
- [Kopieren Sie das imagePullSecret aus dem Standardnamensbereich in andere Namensbereiche in Ihrem Cluster](#copy_imagePullSecret).
- [Erstellen Sie ein imagePullSecret für den Zugriff auf Images in anderen {{site.data.keyword.Bluemix_notm}}-Regionen und -Konten](#other_regions_accounts).
- [Erstellen Sie ein imagePullSecret für den Zugriff auf Images in externen privaten Registrys](#private_images).

<br/>
Wenn Sie bereits ein imagePullSecret in Ihrem Namensbereich erstellt haben, das Sie in Ihrer Bereitstellung verwenden möchten, finden Sie weitere Informationen unter [Container mithilfe des erstellten imagePullSecret bereitstellen](#use_imagePullSecret).

### imagePullSecret aus dem Standardnamensbereich in andere Namensbereiche in Ihrem Cluster kopieren
{: #copy_imagePullSecret}

Sie können das imagePullSecret, das automatisch für den Kubernetes-Standardnamensbereich (`default`) erstellt wird, in andere Namensbereiche in Ihrem Cluster kopieren.
{: shortdesc}

1. Listen Sie verfügbare Namensbereiche in Ihrem Cluster auf.
   ```
   kubectl get namespaces
   ```
   {: pre}

   Beispielausgabe:
   ```
   default          Active    79d
   ibm-cert-store   Active    79d
   ibm-system       Active    79d
   istio-system     Active    34d
   kube-public      Active    79d
   kube-system      Active    79d
   ```
   {: screen}

2. Optional: Erstellen Sie einen Namensbereich in Ihrem Cluster.
   ```
   kubectl create namespace <name_des_namensbereichs>
   ```
   {: pre}

3. Kopieren Sie das imagePullSecrets aus dem Namensbereich `default` in den Namensbereich Ihrer Wahl. Die neuen imagePullSecrets heißen `bluemix-<namespace_name>-secret-regional` und `bluemix-<namespace_name>-secret-international`.
   ```
   kubectl get secret bluemix-default-secret-regional -o yaml | sed 's/default/<name_des_namensbereichs>/g' | kubectl -n <name_des_namensbereichs> create -f -
   ```
   {: pre}
   
   ```
   kubectl get secret bluemix-default-secret-international -o yaml | sed 's/default/<name_des_namensbereichs>/g' | kubectl -n <name_des_namensbereichs> create -f -
   ```
   {: pre}

4.  Überprüfen Sie, dass der geheime Schlüssel erfolgreich erstellt wurde.
    ```
    kubectl get secrets --namespace <name_des_namensbereichs>
    ```
    {: pre}

5. [Stellen Sie mithilfe des imagePullSecret einen Container](#use_imagePullSecret) in Ihrem Namensbereich bereit.


### imagePullSecret für den Zugriff auf Images in anderen {{site.data.keyword.Bluemix_notm}}-Regionen und -Konten erstellen
{: #other_regions_accounts}

Um auf Images in anderen {{site.data.keyword.Bluemix_notm}}-Regionen oder -Konten zuzugreifen, müssen Sie ein Registry-Token erstellen und Ihre Berechtigungsnachweise in einem imagePullSecret speichern.
{: shortdesc}

1.  Falls Sie kein Token haben, [erstellen Sie ein Token für die Registry, auf die Sie zugreifen möchten.](/docs/services/Registry/registry_tokens.html#registry_tokens_create)
2.  Listen Sie die Tokens in Ihrem {{site.data.keyword.Bluemix_notm}}-Konto auf.

    ```
    bx cr token-list
    ```
    {: pre}

3.  Notieren Sie sich die Token-ID, die Sie verwenden möchten.
4.  Rufen Sie den Wert für Ihr Token ab. Ersetzen Sie <em>&lt;token-id&gt;</em> durch die ID des Tokens, das sie im vorherigen Schritt abgerufen haben.

    ```
    bx cr token-get <token-id>
    ```
    {: pre}

    Ihr Tokenwert wird im Feld **Token** Ihrer CLI-Ausgabe angezeigt.

5.  Erstellen Sie den geheimen Kubernetes-Schlüssel, um Ihre Tokeninformationen zu speichern.

    ```
    kubectl --namespace <kubernetes-namensbereich> create secret docker-registry <name_des_geheimen_schlüssels>  --docker-server=<registry-url> --docker-username=token --docker-password=<tokenwert> --docker-email=<docker-e-mail>
    ```
    {: pre}

    <table>
    <caption>Erklärung der Bestandteile dieses Befehls</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Bestandteile dieses Befehls</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes-namensbereich&gt;</em></code></td>
    <td>Erforderlich. Der Kubernetes-Namensbereich Ihres Clusters, in dem Sie den geheimen Schlüssel verwenden und Container bereitstellen möchten. Führen Sie <code>kubectl get namespaces</code>, um alle Namensbereiche in Ihrem Cluster aufzulisten.</td>
    </tr>
    <tr>
    <td><code><em>&lt;name_des_geheimen_schlüssels&gt;</em></code></td>
    <td>Erforderlich. Der Name, den Sie für Ihren geheimen Schlüssel für Image-Pulloperationen (imagePullSecret) verwenden wollen.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry-url&gt;</em></code></td>
    <td>Erforderlich. Die URL der Image-Registry, in der Ihr Namensbereich eingerichtet ist.<ul><li>Für Namensbereiche, die in den Regionen 'Vereinigte Staaten (Süden)' und 'Vereinigte Staaten (Osten)' eingerichtet sind: registry.ng.bluemix.net</li><li>Für Namensbereiche, die in der Region 'Großbritannien (Süden)' eingerichtet sind: registry.eu-gb.bluemix.net</li><li>Für Namensbereiche, die in der Region 'Zentraleuropa (Frankfurt)' eingerichtet sind: registry.eu-de.bluemix.net</li><li>Für Namensbereiche, die in der Region 'Australien (Sydney)' eingerichtet sind: registry.au-syd.bluemix.net</li><li>Für Namensbereiche, die in {{site.data.keyword.Bluemix_dedicated_notm}} eingerichtet sind: registry.<em>&lt;dedizierte_domäne&gt;</em></li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker-benutzername&gt;</em></code></td>
    <td>Erforderlich. Der Benutzername für die Anmeldung bei Ihrer privaten Registry. Für {{site.data.keyword.registryshort_notm}} wurde als Benutzername <code>token</code> festgelegt.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;tokenwert&gt;</em></code></td>
    <td>Erforderlich. Der Wert des zuvor abgerufenen Registry-Tokens.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-e-mail&gt;</em></code></td>
    <td>Erforderlich. Falls Sie über eine Docker-E-Mail-Adresse verfügen, geben Sie diese ein. Ist dies nicht der Fall, geben Sie eine fiktive E-Mail-Adresse ein, z. B. a@b.c. Die Angabe der E-Mail-Adresse ist obligatorisch, um einen geheimen Kubernetes-Schlüssel zu erstellen, wird aber nach der Erstellung nicht weiter genutzt.</td>
    </tr>
    </tbody></table>

6.  Überprüfen Sie, dass der geheime Schlüssel erfolgreich erstellt wurde. Ersetzen Sie <em>&lt;kubernetes-namensbereich&gt;</em> durch den Namensbereich, in dem Sie das imagePullSecret erstellt haben.

    ```
    kubectl get secrets --namespace <kubernetes-namensbereich>
    ```
    {: pre}

7.  [Stellen Sie mithilfe des imagePullSecret einen Container](#use_imagePullSecret) in Ihrem Namensbereich bereit.

### Zugriff auf Images in anderen privaten Registrys
{: #private_images}

Wenn bereits eine private Registry vorhanden ist, müssen Sie die Berechtigungsnachweise für die Registry in einem imagePullSecret von Kubernetes speichern und diesen geheimen Schlüssel aus Ihrer Konfigurationsdatei referenzieren.
{:shortdesc}

Vorbemerkungen:

1.  [Erstellen Sie einen Cluster](cs_clusters.html#clusters_cli).
2.  [Richten Sie Ihre CLI auf Ihren Cluster aus](cs_cli_install.html#cs_cli_configure).

Gehen Sie wie folgt vor, um ein imagePullSecret zu erstellen:

1.  Erstellen Sie den geheimen Kubernetes-Schlüssel, um Ihre Berechtigungsnachweise für die private Registry zu speichern.

    ```
    kubectl --namespace <kubernetes-namensbereich> create secret docker-registry <name_des_geheimen_schlüssels>  --docker-server=<registry-url> --docker-username=<docker-benutzername> --docker-password=<docker-kennwort> --docker-email=<docker-e-mail>
    ```
    {: pre}

    <table>
    <caption>Erklärung der Bestandteile dieses Befehls</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Bestandteile dieses Befehls</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes-namensbereich&gt;</em></code></td>
    <td>Erforderlich. Der Kubernetes-Namensbereich Ihres Clusters, in dem Sie den geheimen Schlüssel verwenden und Container bereitstellen möchten. Führen Sie <code>kubectl get namespaces</code>, um alle Namensbereiche in Ihrem Cluster aufzulisten.</td>
    </tr>
    <tr>
    <td><code><em>&lt;name_des_geheimen_schlüssels&gt;</em></code></td>
    <td>Erforderlich. Der Name, den Sie für Ihren geheimen Schlüssel für Image-Pulloperationen (imagePullSecret) verwenden wollen.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry-url&gt;</em></code></td>
    <td>Erforderlich. Die URL der Registry, in der Ihre privaten Images gespeichert sind.</td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker-benutzername&gt;</em></code></td>
    <td>Erforderlich. Der Benutzername für die Anmeldung bei Ihrer privaten Registry.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;tokenwert&gt;</em></code></td>
    <td>Erforderlich. Der Wert des zuvor abgerufenen Registry-Tokens.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-e-mail&gt;</em></code></td>
    <td>Erforderlich. Falls Sie über eine Docker-E-Mail-Adresse verfügen, geben Sie diese ein. Ist dies nicht der Fall, geben Sie eine fiktive E-Mail-Adresse ein, z. B. a@b.c. Die Angabe der E-Mail-Adresse ist obligatorisch, um einen geheimen Kubernetes-Schlüssel zu erstellen, wird aber nach der Erstellung nicht weiter genutzt.</td>
    </tr>
    </tbody></table>

2.  Überprüfen Sie, dass der geheime Schlüssel erfolgreich erstellt wurde. Ersetzen Sie
<em>&lt;kubernetes-namensbereich&gt;</em> durch den Namen des Namensbereichs, in dem Sie das
imagePullSecret erstellt haben.

    ```
    kubectl get secrets --namespace <kubernetes-namensbereich>
    ```
    {: pre}

3.  [Erstellen Sie einen Pod, der das imagePullSecret referenziert](#use_imagePullSecret).

## Container mithilfe des erstellten imagePullSecret bereitstellen
{: #use_imagePullSecret}

Sie können ein imagePullSecret in Ihrer Pod-Bereitstellung definieren oder das imagePullSecret in Ihrem Kubernetes-Servicekonto speichern, sodass es für alle Bereitstellungen, bei denen kein Servicekonto angegeben wird, verfügbar ist.
{: shortdesc}

Wählen Sie eine der beiden folgenden Optionen aus:
* [Verweis auf das imagePullSecret in Ihrer Pod-Bereitstellung](#pod_imagePullSecret): Verwenden Sie diese Option, wenn Sie nicht allen Pods in Ihrem Namensbereich standardmäßig Zugriff auf Ihre Registry geben möchten.
* [Speichern des imagePullSecret im Kubernetes-Servicekonto](#store_imagePullSecret): Verwenden Sie diese Option, um Zugriff auf Images in Ihrer Registry für Bereitstellungen im ausgewählten Kubernetes-Namensbereich zu gewähren.

Vorbemerkungen:
* [Erstellen Sie ein imagePullSecret](#other), um Zugriff auf Images in anderen Registrys, Kubernetes-Namensbereichen, {{site.data.keyword.Bluemix_notm}}-Regionen oder -Konten zu geben.
* [Richten Sie Ihre CLI auf Ihren Cluster aus](cs_cli_install.html#cs_cli_configure).

### Verweis auf das `imagePullSecret` in Ihrer Pod-Bereitstellung
{: #pod_imagePullSecret}

Wenn Sie auf das imagePullSecret in einer Pod-Bereitstellung verweisen, ist das imagePullSecret nur für diesen Pod gültig und kann nicht mit anderen Pods im Namensbereich gemeinsam verwendet werden.
{:shortdesc}

1.  Erstellen Sie eine Podkonfigurationsdatei mit dem Namen `mypod.yaml`.
2.  Definieren Sie den Pod und das imagePullSecret, um auf die private {{site.data.keyword.registrylong_notm}} zuzugreifen.

    So greifen Sie auf ein privates Image zu:
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
        - name: <containername>
          image: registry.<region>.bluemix.net/<name_des_namensbereichs>/<imagename>:<tag>
      imagePullSecrets:
        - name: <name_des_geheimen_schlüssels>
    ```
    {: codeblock}

    So greifen Sie auf ein öffentliches {{site.data.keyword.Bluemix_notm}}-Image zu:
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
        - name: <containername>
          image: registry.bluemix.net/<imagename>:<tag>
      imagePullSecrets:
        - name: <name_des_geheimen_schlüssels>
    ```
    {: codeblock}

    <table>
    <caption>Erklärung der Komponenten der YAML-Datei</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der YAML-Dateikomponenten</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;containername&gt;</em></code></td>
    <td>Der Name des Containers, den Sie in Ihrem Cluster bereitstellen möchten.</td>
    </tr>
    <tr>
    <td><code><em>&lt;name_des_namensbereichs&gt;</em></code></td>
    <td>Der Namensbereich, in dem das Image gespeichert ist. Führen Sie den Befehl `bx cr namespace-list` aus, um die verfügbaren Namensbereiche aufzulisten.</td>
    </tr>
    <tr>
    <td><code><em>&lt;imagename&gt;</em></code></td>
    <td>Der Name des Images, das Sie verwenden möchten. Führen Sie den Befehl `bx cr image-list` aus, um die verfügbaren Images in einem {{site.data.keyword.Bluemix_notm}} aufzulisten.</td>
    </tr>
    <tr>
    <td><code><em>&lt;tag&gt;</em></code></td>
    <td>Die Version des Images, das Sie verwenden möchten. Ist kein Tag angegeben, wird standardmäßig das Image mit dem Tag <strong>latest</strong> verwendet.</td>
    </tr>
    <tr>
    <td><code><em>&lt;name_des_geheimen_schlüssels&gt;</em></code></td>
    <td>Der Name des imagePullSecret, das Sie zuvor erstellt haben.</td>
    </tr>
    </tbody></table>

3.  Speichern Sie Ihre Änderungen.
4.  Erstellen Sie die Bereitstellung in Ihrem Cluster.
    ```
    kubectl apply -f mypod.yaml
    ```
    {: pre}

### imagePullSecret im Kubernetes-Servicekonto für den ausgewählten Namensbereich speichern
{:#store_imagePullSecret}

Jeder Namensbereich hat ein Kubernetes-Servicekonto namens `default`. Sie können das imagePullSecret zu diesem Servicekonto hinzufügen, um Zugriff auf Images in Ihrer Registry zu gewähren. Bereitstellungen, bei denen kein Servicekonto angegeben ist, verwenden automatisch das Servicekonto `default` für diesen Namensbereich.
{:shortdesc}

1. Prüfen Sie, ob bereits ein imagePullSecret für Ihr Standardservicekonto vorhanden ist.
   ```
   kubectl describe serviceaccount default -n <name_des_namensbereichs>
   ```
   {: pre}
   Wenn `<none>` im Eintrag **Image pull secrets** angezeigt wird, ist kein imagePullSecret vorhanden.  
2. Fügen Sie das imagePullSecret zu Ihrem Standardservicekonto hinzu.
   - **So fügen Sie das imagePullSecret hinzu, wenn kein imagePullSecret definiert ist:**
       ```
       kubectl patch -n <name_des_namensbereichs> serviceaccount/default -p '{"imagePullSecrets":[{"name": "bluemix-<name_des_namensbereichs>-secret-regional"}]}'
       ```
       {: pre}
   - **So fügen Sie das imagePullSecret hinzu, wenn bereits ein imagePullSecret definiert ist:**
       ```
       kubectl patch -n <name_des_namensbereichs> serviceaccount/default --type='json' -p='[{"op":"add","path":"/imagePullSecrets/-","value":{"name":"bluemix-<name_des_namensbereichs>-secret-regional"}}]'
       ```
       {: pre}
3. Überprüfen Sie, dass Ihr imagePullSecret zu Ihrem Standardservicekonto hinzugefügt wurde.
   ```
   kubectl describe serviceaccount default -n <name_des_namensbereichs>
   ```
   {: pre}

   Beispielausgabe:
   ```
   Name:                default
   Namespace:           <name_des_namensbereichs>
   Labels:              <none>
   Annotations:         <none>
   Image pull secrets:  bluemix-namespace_name-secret-regional
   Mountable secrets:   default-token-sh2dx
   Tokens:              default-token-sh2dx
   Events:              <none>
   ```
   {: pre}

4. Stellen Sie einen Container aus einem Image in Ihrer Registry bereit.
   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: mypod
   spec:
     containers:
       - name: <containername>
         image: registry.<region>.bluemix.net/<name_des_namensbereichs>/<imagename>:<tag>
   ```
   {: codeblock}

5. Erstellen Sie die Bereitstellung im Cluster.
   ```
   kubectl apply -f mypod.yaml
   ```
   {: pre}

<br />


