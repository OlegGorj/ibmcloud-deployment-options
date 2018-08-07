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




# Apps in Clustern bereitstellen
{: #app}

Sie können Kubernetes-Verfahren in {{site.data.keyword.containerlong}} verwenden, um Apps in Containern bereitzustellen und um sicherzustellen, dass Ihre Apps ununterbrochen betriebsbereit sind. Sie können beispielsweise rollierende Aktualisierungen und Rollbacks ausführen, ohne dass Ihren Benutzern hierdurch Ausfallzeiten entstehen.
{: shortdesc}

Erfahren Sie mehr zu den allgemeinen Schritten zur Bereitstellung von Apps, indem Sie auf einen Bereich der folgenden Abbildung klicken.

<img usemap="#d62e18" border="0" class="image" id="basic_deployment_process" src="images/basic_deployment_process.png" width="780" style="width:780px;" alt="Basisbereitstellungsprozess"/>
<map name="d62e18" id="d62e18">
<area href="cs_cli_install.html" target="_blank" alt="Installieren Sie die CLIs." title="Installieren Sie die CLIs." shape="rect" coords="30, 69, 179, 209" />
<area href="https://kubernetes.io/docs/concepts/configuration/overview/" target="_blank" alt="Erstellen Sie eine Konfigurationsdatei für Ihre App. Verschaffen Sie sich einen Überblick über die Best Practices von Kubernetes." title="Erstellen Sie eine Konfigurationsdatei für Ihre App. Verschaffen Sie sich einen Überblick über die Best Practices von Kubernetes." shape="rect" coords="254, 64, 486, 231" />
<area href="#app_cli" target="_blank" alt="Option 1: Führen Sie Konfigurationsdateien über die Kubernetes-CLI aus." title="Option 1: Führen Sie Konfigurationsdateien über die Kubernetes-CLI aus." shape="rect" coords="544, 67, 730, 124" />
<area href="#cli_dashboard" target="_blank" alt="Option 2: Starten Sie das Kubernetes-Dashboard lokal und führen Sie Konfigurationsdateien aus." title="Option 2: Starten Sie das Kubernetes-Dashboard lokal und führen Sie Konfigurationsdateien aus." shape="rect" coords="544, 141, 728, 204" />
</map>


<br />


## Bereitstellungen mit Hochverfügbarkeit planen
{: #highly_available_apps}

Je breiter gefächert Sie Ihre Containerkonfiguration auf mehrere Workerknoten und Cluster verteilen, umso geringer ist die Wahrscheinlichkeit, dass Ihre Benutzer Ausfallzeiten mit Ihrer App verzeichnen.
{: shortdesc}

Betrachten Sie die folgenden potenziellen Appkonfigurationen, die nach zunehmendem Grad der Verfügbarkeit angeordnet sind.

![Stufen der Hochverfügbarkeit für eine App](images/cs_app_ha_roadmap.png)

1.  Bereitstellung mit n+2 Pods, deren Verwaltung durch eine Replikatgruppe erfolgt.
2.  Bereitstellung mit n+2 Pods, deren Verwaltung durch eine Replikatgruppe erfolgt und die auf mehrere Knoten
(Anti-Affinität) an demselben Standort verteilt sind.
3.  Bereitstellung mit n+2 Pods, deren Verwaltung durch eine Replikatgruppe erfolgt und die auf mehrere Knoten
(Anti-Affinität) an unterschiedlichen Standorten verteilt sind.
4.  Bereitstellung mit n+2 Pods, deren Verwaltung durch eine Replikatgruppe erfolgt und die auf mehrere Knoten (Anti-Affinität) in unterschiedlichen Regionen verteilt sind.




### Verfügbarkeit Ihrer App erhöhen
{: #increase_availability}

<dl>
  <dt>Bereitstellungen und Replikatgruppen zum Bereitstellen Ihrer App und deren Abhängigkeiten verwenden</dt>
    <dd><p>Eine Bereitstellung ist eine Kubernetes-Ressource, mit der Sie alle Komponenten Ihrer App und deren Abhängigkeiten deklarieren können. Bei Bereitstellungen müssen Sie nicht alle Schritte niederschreiben und können sich stattdessen auf Ihre App konzentrieren.</p>
    <p>Wenn Sie mehrere Pods bereitstellen, wird für Ihre Bereitstellungen automatisch eine Replikatgruppe erstellt, anhand der die Pods überwacht werden und sichergestellt wird, dass die gewünschte Anzahl von Pods jederzeit betriebsbereit ist. Wird ein Pod inaktiv, so ersetzt die Replikatgruppe den inaktiven Pod durch einen neuen Pod.</p>
    <p>Mit einer Bereitstellung können Sie Aktualisierungsstrategien für Ihre App definieren. Dabei können Sie unter Anderem die Anzahl von Pods angeben, die Sie bei einer rollierenden Aktualisierung hinzufügen wollen, und festlegen, wie viele Pods zur gleichen Zeit nicht verfügbar sein dürfen. Wenn Sie eine rollierende Aktualisierung durchführen, prüft die Bereitstellung, ob die Überarbeitung funktioniert, und stoppt den Rollout, wenn Fehler erkannt werden.</p>
    <p>Sie können mehrere Revisionen mit unterschiedlichen Flags gleichzeitig bereitstellen. Sie können beispielsweise eine Bereitstellung zuerst testen, bevor Sie sich entschließen, sie per Push-Operation an die Produktion zu übertragen.</p>
    <p>Mit Bereitstellungen können Sie alle bereitgestellten Revisionen nachverfolgen. Sie können dieses Verlaufsprotokoll verwenden, um ein Rollback auf eine vorherige Version durchzuführen, falls Sie feststellen, dass Ihre Aktualisierungen nicht wie erwartet funktionieren.</p></dd>
  <dt>Ausreichende Anzahl von Replikaten für die Arbeitslast Ihrer App plus 2 einbeziehen</dt>
    <dd>Um Ihre App noch verfügbarer zu machen und ihre Ausfallsicherheit zu steigern, sollten Sie erwägen, über die Mindestzahl hinaus zusätzliche Replikate einzubinden, damit die erwartete Arbeitslast verarbeitet werden kann. Zusätzliche Replikate sind in der Lage, die Arbeitslast abzufangen, wenn ein Pod ausfällt und der ausgefallene Pod noch nicht durch die Replikatgruppe ersetzt wurde. Zum Schutz vor zwei gleichzeitigen Ausfällen sollten Sie zwei zusätzliche Replikate einbinden. Diese Konfiguration folgt dem Muster 'N+2'. Hierbei steht 'N' für die Anzahl der Replikate, die für die Verarbeitung der eingehenden Arbeitslast zur Verfügung steht, während der Wert '+2' die beiden zusätzlich eingebundenen Replikate angibt. Vorausgesetzt, in Ihrem Cluster ist ausreichend Platz, ist die Anzahl der mögliche Pods unbegrenzt.</dd>
  <dt>Pods auf mehrere Knoten (Anti-Affinität) verteilen</dt>
    <dd><p>Wenn Sie Ihre Bereitstellung erstellen, können alle Pods auf demselben Workerknoten bereitgestellt werden. Dies wird als Affinität oder Zusammenstellung bezeichnet. Zum Schutz Ihrer App vor dem Ausfall eines Workerknotens können Sie Ihre Bereitstellung so konfigurieren, dass Ihre Pods mithilfe der Option <em>podAntiAffinity</em> in Ihren Standardclustern über mehrere Workerknoten verteilt werden. Sie können zwei Typen von Pod-Anti-Affinität definieren: bevorzugt oder erforderlich. Weitere Informationen finden Sie in der Kubernetes-Dokumentation unter <a href="https://kubernetes.io/docs/concepts/configuration/assign-pod-node/" rel="external" target="_blank" title="(Wird in einem neuen Tab oder einem neuen Fenster geöffnet)">Assigning Pods to Nodes</a>.</p>
    <p><strong>Hinweis</strong>: Mit der erforderlichen Anti-Affinität können Sie nur die Anzahl von Replikaten bereitstellen, für die es Workerknoten gibt. Wenn Sie beispielsweise drei Workerknoten haben, aber in Ihrer YAML-Datei fünf Replikate definieren, werden nur drei Replikate bereitgestellt. Jedes Replikat befindet sich auf einem anderen Workerknoten. Die zwei übrigen Replikate verbleiben ausstehend. Wenn Sie einen weiteren Workerknoten zu Ihrem Cluster hinzufügen, wird eines der restlichen Replikate automatisch auf dem Workerknoten bereitgestellt.<p>
    <p><strong>Beispiele für YAML-Bereitstellungsdateien</strong>:<ul>
    <li><a href="https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/nginx_preferredAntiAffinity.yaml" rel="external" target="_blank" title="(Wird in einem neuen Tab oder Fenster geöffnet)">Nginx-App mit bevorzugter Anti-Affinität für Pods</a></li>
    <li><a href="https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/liberty_requiredAntiAffinity.yaml" rel="external" target="_blank" title="(Wird in einem neuen Tab oder Fenster geöffnet)">IBM® WebSphere® Application Server Liberty-App mit erforderlicher Anti-Affinität für Pods</a></li></ul></p>
    </dd>
<dt>Pods auf mehrere Zonen oder Regionen verteilen</dt>
  <dd>Als Schutz Ihrer App vor einem Ausfall des Standorts oder der Region können Sie an einem anderen Standort oder in einer anderen Region
einen zweiten Cluster erstellen und mit der YAML-Datei für die Bereitstellung ein Duplikat der Replikatgruppe bereitstellen. Durch Einfügen einer gemeinsam genutzten Route und einer Lastausgleichsfunktion (Load Balancer)
vor Ihren Clustern können Sie eine Verteilung der Arbeitslast auf mehrere Standorte oder Regionen bewirken. Weitere Informationen finden Sie unter [Hochverfügbarkeit von Clustern](cs_clusters.html#clusters).
  </dd>
</dl>


### Minimale App-Bereitstellung
{: #minimal_app_deployment}

Eine grundlegende App-Bereitstellung in einem kostenlosen Cluster oder Standardcluster kann die folgenden Komponenten enthalten.
{: shortdesc}

![Konfiguration für die Bereitstellung](images/cs_app_tutorial_components1.png)

Um die Komponenten für eine minimale App wie im Diagramm dargestellt bereitzustellen, verwenden Sie eine Konfigurationsdatei wie im folgenden Beispiel:
```
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: ibmliberty
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: ibmliberty
    spec:
      containers:
      - name: ibmliberty
        image: registry.bluemix.net/ibmliberty:latest
        ports:
        - containerPort: 9080        
---
apiVersion: v1
kind: Service
metadata:
  name: ibmliberty-service
  labels:
    app: ibmliberty
spec:
  selector:
    app: ibmliberty
  type: NodePort
  ports:
   - protocol: TCP
     port: 9080
```
{: codeblock}

**Hinweis:** Um Ihren Service verfügbar zu machen, müssen Sie sicherstellen, dass das Schlüssel/Wert-Paar, das Sie im Abschnitt `spec.selector` verwendet haben, dem Schlüssel/Wert-Paar entspricht, dass Sie im Abschnitt `spec.template.metadata.labels` Ihrer YAML-Bereitstellungsdatei angegeben haben.
Um mehr über die einzelnen Komponenten zu erfahren, lesen Sie sich den Abschnitt [Grundlegende Informationen zu Kubernetes](cs_tech.html#kubernetes_basics) durch.

<br />




## Kubernetes-Dashboard starten
{: #cli_dashboard}

Öffnen Sie auf Ihrem lokalen System ein Kubernetes-Dashboard, um Informationen zu einem Cluster und seinen Workerknoten anzuzeigen. [In der GUI](#db_gui) können Sie über eine praktische Schaltfläche mit einem Mausklick auf das Dashboard zugreifen. [Bei der CLI](#db_cli) können Sie auf das Dashoard zugreifen oder die Schritte in einem Automatisierungsprozess wie für eine CI/CD-Pipeline verwenden.
{:shortdesc}

Führen Sie zunächst den folgenden Schritt aus: [Richten Sie Ihre CLI](cs_cli_install.html#cs_cli_configure) auf Ihren Cluster aus. Für diese Task ist die [Zugriffsrichtlinie 'Administrator'](cs_users.html#access_policies) erforderlich. Überprüfen Sie Ihre aktuelle [Zugriffsrichtlinie](cs_users.html#infra_access).

Sie können den Standardport verwenden oder einen eigenen Port festlegen, um das Kubernetes-Dashboard für einen Cluster zu starten.

**Kubernetes-Dashboard über die GUI starten**
{: #db_gui}

1.  Melden Sie sich an der [{{site.data.keyword.Bluemix_notm}}-GUI](https://console.bluemix.net/) an. 
2.  Wählen Sie in Ihrem Profil in der Menüleiste das Konto aus, das Sie verwenden möchten.
3.  Klicken Sie im Menü auf **Container**.
4.  Klicken Sie auf der Seite **Cluster** auf den Cluster, auf den Sie zugreifen möchten.
5.  Klicken Sie auf der Seite mit den Clusterdetails auf die Schaltfläche **Kubernetes-Dashboard**.

**Kubernetes-Dashboard über die CLI starten**
{: #db_cli}

*  Für Cluster mit der Kubernetes-Masterversion 1.7.16 oder einer früheren Version:

    1.  Legen Sie die Standardportnummer für den Proxy fest.

        ```
        kubectl proxy
        ```
        {: pre}

        Ausgabe:

        ```
        Starting to serve on 127.0.0.1:8001
        ```
        {: screen}

    2.  Öffnen Sie das Kubernetes-Dashboard in einem Web-Browser.

        ```
        http://localhost:8001/ui
        ```
        {: codeblock}

*  Für Cluster mit einer Kubernetes-Masterversion ab Version 1.8.2:

    1.  Rufen Sie Ihre Berechtigungsnachweise für Kubernetes ab.

        ```
        kubectl config view -o jsonpath='{.users[0].user.auth-provider.config.id-token}'
        ```
        {: pre}

    2.  Kopieren Sie den Wert für **id-token**, der in der Ausgabe angezeigt wird.

    3.  Legen Sie die Standardportnummer für den Proxy fest.

        ```
        kubectl proxy
        ```
        {: pre}

        Beispielausgabe:

        ```
        Starting to serve on 127.0.0.1:8001
        ```
        {: screen}

    4.  Melden Sie sich beim Dashboard an.

      1.  Navigieren Sie in Ihrem Browser zur folgenden URL:

          ```
          http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/
          ```
          {: codeblock}

      2.  Wählen Sie auf der Anmeldeseite die Authentifizierungsmethode **Token** aus.

      3.  Fügen Sie dann den zuvor kopierten Wert für **id-token** in das Feld **Token** ein und klicken Sie auf **ANMELDEN**.

Wenn Sie die Arbeit im Kubernetes-Dashboard beendet haben, beenden Sie den Befehl `proxy` mit der Tastenkombination `STRG + C`. Anschließend ist das Kubernetes-Dashboard nicht mehr verfügbar. Führen Sie den Befehl `proxy` aus, um das Kubernetes-Dashboard erneut zu starten.

[Als Nächstes können Sie eine Konfigurationsdatei über das Dashboard ausführen.](#app_ui)


<br />




## Geheime Schlüssel erstellen
{: #secrets}

Geheime Kubernetes-Schlüssel stellen eine sichere Methode zum Speichern vertraulicher Informationen wie Benutzernamen, Kennwörter oder Schlüssel dar.
{:shortdesc}

<table>
<caption>Erforderliche Dateien, die nach Task in geheimen Schlüsseln gespeichert werden müssen</caption>
<thead>
<th>Task</th>
<th>Erforderliche Dateien, die in geheimen Schlüssel gespeichert werden müssen</th>
</thead>
<tbody>
<tr>
<td>Hinzufügen eines Service zu einem Cluster</td>
<td>Keine. Es wird ein geheimer Schlüssel für Sie erstellt, wenn Sie einen Service an einen Cluster binden.</td>
</tr>
<tr>
<td>Optional: Konfigurieren Sie den Ingress-Service mit TLS, wenn 'ingress-secret' nicht verwendet wird. <p><b>Hinweis</b>: TLS ist bereits standardmäßig aktiviert und ein geheimer Schlüssel wurde bereits für die TLS-Verbindung erstellt.

Gehen Sie wie folgt vor, um den geheimen TLS-Standardschlüssel anzuzeigen:
<pre>
bx cs cluster-get &lt;clustername_oder_-id&gt; | grep "Ingress secret"
</pre>
</p>
Um stattdessen Ihren eigenen geheimen Schlüssel zu erstellen, führen Sie die Schritte im nachfolgenden Abschnitt durch.</td>
<td>Serverzertifikat und -schlüssel: <code>server.crt</code> und <code>server.key</code></td>
<tr>
<td>Erstellen Sie die Annotation 'mutual-authentication'.</td>
<td>Zertifikat der Zertifizierungsstelle: <code>ca.crt</code></td>
</tr>
</tbody>
</table>

Weitere Informationen darüber, was Sie in geheimen Schlüssel speichern können, finden Sie in der [Kubernetes-Dokumentation](https://kubernetes.io/docs/concepts/configuration/secret/).



Gehen Sie wie folgt vor, um einen geheimen Schlüssel mit einem Zertifikat zu erstellen:

1. Generieren Sie das Zertifikat und den Schlüssel der Zertifizierungsstelle über Ihren Zertifikatsanbieter. Wenn Sie über eine eigene Domäne verfügen, kaufen Sie ein offizielles TLS-Zertifikat für Ihre Domäne. Zu Testzwecken können Sie ein selbst signiertes Zertifikat generieren.

 **Wichtig**: Stellen Sie sicher, dass der [allgemeine Name](https://support.dnsimple.com/articles/what-is-common-name/) für jedes Zertifikat anders ist.

 Das Clientzertifikat und der Clientschlüssel müssen anhand des Trusted-Root-Zertifikats verifiziert werden, welches in diesem Fall das Zertifikat der Zertifizierungsstelle ist. Beispiel:

 ```
 Client Certificate: issued by Intermediate Certificate
 Intermediate Certificate: issued by Root Certificate
 Root Certificate: issued by itself
 ```
 {: codeblock}

2. Erstellen Sie das Zertifikat als einen geheimen Kubernetes-Schlüssel.

   ```
   kubectl create secret generic <name_des_geheimen_schlüssels> --from-file=<zertifikatsdatei>=<zertifikatsdatei>
   ```
   {: pre}

   Beispiel:
   - TLS-Verbindung:

     ```
     kubectl create secret tls <name_des_geheimen_schlüssels> --from-file=tls.crt=server.crt --from-file=tls.key=server.key
     ```
     {: pre}

   - Annotation für die gegenseitige Authentifizierung:

     ```
     kubectl create secret generic <name_des_geheimen_schlüssels> --from-file=ca.crt=ca.crt
     ```
     {: pre}

<br />





## Apps über die GUI bereitstellen
{: #app_ui}

Wenn Sie eine App über das Kubernetes-Dashboard in Ihrem Cluster bereitstellen, wird eine Bereitstellungsressource automatisch die Pods in Ihrem Cluster erstellen, aktualisieren und verwalten.
{:shortdesc}

Vorbemerkungen:

-   Installieren Sie die erforderlichen [CLIs](cs_cli_install.html#cs_cli_install).
-   [Richten Sie Ihre CLI](cs_cli_install.html#cs_cli_configure) (Befehlszeilenschnittstelle) auf Ihren Cluster aus.

Gehen Sie wie folgt vor, um Ihre App bereitzustellen:

1.  Öffnen Sie das Kubernetes-[Dashboard](#cli_dashboard) und klicken Sie auf **+ Erstellen**.
2.  Geben Sie Ihre App-Details auf einem der beiden folgenden Wege ein.
  * Wählen Sie **App-Details unten angeben** aus und geben Sie die Details ein.
  * Wählen Sie **YAML- oder JSON-Datei hochladen** aus, um die [Konfigurationsdatei ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/) Ihrer App hochzuladen.

  Benötigen Sie Hilfe mit Ihrer Konfigurationsdatei? Sehen Sie sich diese [YAML-Beispieldatei ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/IBM-Cloud/kube-samples/blob/master/deploy-apps-clusters/deploy-ibmliberty.yaml). In diesem Beispiel wird ein Container aus dem Image **ibmliberty** in der Region 'Vereinigte Staaten (Süden)' bereitgestellt. Erfahren Sie mehr über das [Sichern der persönliche Daten](cs_secure.html#pi) bei der Arbeit mit Kubernetes-Ressourcen.{: tip}

3.  Überprüfen Sie auf einem der beiden folgenden Wege, dass Sie Ihre App erfolgreich bereitgestellt haben.
  * Klicken Sie im Kubernetes-Dashboard auf **Bereitstellungen**. Es wird eine Liste der erfolgreichen Bereitstellungen angezeigt.
  * Wenn Ihre App [öffentlich verfügbar](cs_network_planning.html#public_access) ist, navigieren Sie zur Clusterübersichtsseite in Ihrem {{site.data.keyword.containerlong}}-Dashboard. Kopieren Sie die Unterdomäne, die sich im Abschnitt mit der Clusterzusammenfassung befindet, und fügen Sie sie in einen Browser ein, um Ihre App anzuzeigen.

<br />


## Apps über die CLI (Befehlszeilenschnittstelle) bereitstellen
{: #app_cli}

Nachdem ein Cluster erstellt worden ist, können Sie in diesem Cluster über die Kubernetes-CLI eine App bereitstellen.
{:shortdesc}

Vorbemerkungen:

-   Installieren Sie die erforderlichen [CLIs](cs_cli_install.html#cs_cli_install).
-   [Richten Sie Ihre CLI](cs_cli_install.html#cs_cli_configure) (Befehlszeilenschnittstelle) auf Ihren Cluster aus.

Gehen Sie wie folgt vor, um Ihre App bereitzustellen:

1.  Erstellen Sie eine Konfigurationsdatei auf der Grundlage der [Best Practices von Kubernetes ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/configuration/overview/). Im Allgemeinen enthält eine Konfigurationsdatei die Konfigurationsdetails für jede Ressource, die Sie in Kubernetes erstellen. Ihr Script könnte dabei einen oder mehrere der folgenden Abschnitte enthalten:

    -   [Bereitstellung ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/): Definiert die Erstellung von Pods und Replikatgruppen. Ein Pod enthält eine einzelne containerisierte App. Mehrere Instanzen von Pods werden durch Replikatgruppen gesteuert.

    -   [Service ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/services-networking/service/): Stellt Front-End-Zugriff auf Pods über eine öffentliche IP-Adresse eines Workerknotens oder einer Lastausgleichsfunktion bzw. über eine öffentliche Ingress-Route bereit.

    -   [Ingress ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/services-networking/ingress/): Gibt einen Typ von Lastausgleichsfunktion an, die Routen für den öffentlichen Zugriff auf Ihre App bereitstellt.

    Erfahren Sie mehr über das [Sichern der persönliche Daten](cs_secure.html#pi) bei der Arbeit mit Kubernetes-Ressourcen.

2.  Führen Sie die Konfigurationsdatei in dem Kontext eines Clusters aus.

    ```
    kubectl apply -f config.yaml
    ```
    {: pre}

3.  Wenn Sie Ihre App mithilfe eines Knotenportservice, eines Lastenausgleichsservice oder Ingress öffentlich zugänglich gemacht haben, überprüfen Sie, dass Sie auf die App zugreifen können.

<br />




## Eine App auf einer GPU-Maschine bereitstellen
{: #gpu_app}

Wenn Sie über einen [Bare-Metal-GPU-Maschinentyp (Graphics Processing Unit)](cs_clusters.html#shared_dedicated_node) verfügen, können Sie rechenintensive Workloads für den Workerknoten terminieren. Sie können beispielsweise eine 3D-App ausführen, die die CUDA-Plattform (CUDA, Compute Unified Device Architecture) verwendet, um zur Leistungssteigerung die Systembelastung auf die GPU und CPU aufzuteilen.
{:shortdesc}

In den nachfolgenden Abschnitten erfahren Sie, wie Sie Workloads bereitstellen, für die die GPU erforderlich ist. Sie können auch [Apps bereitstellen](#app_ui), die ihre Workloads nicht auf der GPU und der CPU verarbeiten müssen. Anschließend kann es hilfreich sein, sich mit rechenintensiven Workloads, wie dem Framework [TensorFlow ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.tensorflow.org/) für maschinelles Lernen mit [dieser Kubernetes-Demo ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/pachyderm/pachyderm/tree/master/doc/examples/ml/tensorflow) vertraut zu machen.

Vorbemerkungen:
* [Erstellen Sie einen Bare-Metal-GPU-Maschinentyp](cs_clusters.html#clusters_cli). Beachten Sie, dass dieser Prozess länger als einen Geschäftstag dauern kann.
* Für den Cluster-Master und GPU-Workerknoten muss Kubernetes Version 1.10 oder höher ausgeführt werden.

Gehen Sie wie folgt vor, um eine Workload auf einer GPU-Maschine auszuführen:
1.  Erstellen Sie eine YAML-Datei. In diesem Beispiel verwaltet eine YALM-Datei des Typs `Job` Stapelworkloads, die einen Pod mit kurzer Lebensdauer hervorbringen, der so lange ausgeführt wird, bis der Befehl, der ihn erfolgreich abschließen soll, beendet wird.

    **Wichtig**: Bei GPU-Workloads müssen Sie immer einen Wert für das Feld `resources: limits: nvidia.com/gpu` in der YAML-Spezifikation bereitstellen.

    ```yaml
    apiVersion: batch/v1
    kind: Job
    metadata:
      name: nvidia-smi
      labels:
        name: nvidia-smi
    spec:
      template:
        metadata:
          labels:
            name: nvidia-smi
        spec:
          containers:
          - name: nvidia-smi
            image: nvidia/cuda:9.1-base-ubuntu16.04
            command: [ "/usr/test/nvidia-smi" ]
            imagePullPolicy: IfNotPresent
            resources:
              limits:
                nvidia.com/gpu: 2
            volumeMounts:
            - mountPath: /usr/test
              name: nvidia0
          volumes:
            - name: nvidia0
              hostPath:
                path: /usr/bin
          restartPolicy: Never
    ```
    {: codeblock}

    <table>
    <caption>YAML-Dateikomponenten</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der YAML-Dateikomponenten</th>
    </thead>
    <tbody>
    <tr>
    <td>Namen für Metadaten und Bezeichnungen</td>
    <td>Geben Sie einen Namen und eine Bezeichnung für den Job an und verwenden Sie diesen Namen sowohl für die Metadaten der Datei als auch für die Metadaten von `spec template`. Beispiel: `nvidia-smi`.</td>
    </tr>
    <tr>
    <td><code>containers/image</code></td>
    <td>Geben Sie ein Image an, von dem der Container eine Instanz ausführt. In diesem Beispiel ist der Wert so festgelegt, dass das Docker Hub-CUDA-Image <code>nvidia/cuda:9.1-base-ubuntu16.04</code> verwendet wird.</td>
    </tr>
    <tr>
    <td><code>containers/command</code></td>
    <td>Geben Sie einen Befehl an, der im Container ausgeführt werden soll. In diesem Beispiel bezieht sich der Befehl <code>[ "/usr/test/nvidia-smi" ]</code> auf eine Binärdatei auf der GPU-Maschine; Sie müssen daher auch ein Datenträgermount einrichten.</td>
    </tr>
    <tr>
    <td><code>containers/imagePullPolicy</code></td>
    <td>Um nur ein neues Image zu extrahieren, wenn das Image sich nicht aktuell auf dem Workerknoten befindet, geben Sie <code>IfNotPresent</code> an.</td>
    </tr>
    <tr>
    <td><code>resources/limits</code></td>
    <td>Für GPU-Maschinen müssen Sie eine Ressourcengrenze angeben. Das [Einheiten-Plug-in![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/cluster-administration/device-plugins/) von Kubernetes legt die Standardressourcenanforderung entsprechend dieser Grenze fest.
    <ul><li>Sie müssen den Schlüssel als <code>nvidia.com/gpu</code> angeben.</li>
    <li>Geben Sie die Anzahl der von Ihnen angeforderten GPUs als Ganzzahl ein, beispielsweise <code>2</code>. <strong>Hinweis</strong>: Container-Pods geben GPUs nicht frei und GPUs können nicht überbelastet werden. Wenn Sie zum Beispiel nur über eine Maschine des Typs `mg1c.16x128` verfügen, stehen Ihnen nur zwei GPUs auf dieser Maschine zur Verfügung und es kann maximal der Wert `2` angegeben werden.</li></ul></td>
    </tr>
    <tr>
    <td><code>volumeMounts</code></td>
    <td>Name des Datenträgers, der an den Container angehängt wird, beispielsweise <code>nvidia0</code>. Geben Sie einen Wert für <code>mountPath</code> im Container für den Datenträger an. In diesem Beispiel entspricht der Pfad <code>/usr/test</code> dem im Containerbefehl des Jobs verwendeten Pfad.</td>
    </tr>
    <tr>
    <td><code>volumes</code></td>
    <td>Geben Sie dem Datenträger des Jobs einen Namen, beispielsweise <code>nvidia0</code>. Geben Sie als <code>hostPath</code> für den GPU-Workerknoten den Pfad (<code>path</code>) des Datenträgers auf dem Host an. In diesem Beispiel lautet dieser <code>/usr/bin</code>. Der Wert für <code>mountPath</code> des Containers wird dem Pfad (<code>path</code>) des Hostdatenträgers zugeordnet, wodurch der Job Zugriff auf die NVIDIA-Binärdateien auf dem GPU-Workerknoten erhält, damit der Containerbefehl ausgeführt werden kann.</td>
    </tr>
    </tbody></table>

2.  Wenden Sie die YAML-Datei an. Beispiel:

    ```
    kubectl apply -f nvidia-smi.yaml
    ```
    {: pre}

3.  Überprüfen Sie den Job-Pod, indem Sie die Pods anhand der Bezeichnung `nvidia-sim` filtern. Stellen Sie sicher, dass der **STATUS** den Wert **Completed** aufweist.

    ```
    kubectl get pod -a -l 'name in (nvidia-sim)'
    ```
    {: pre}

    Beispielausgabe:
    ```
    NAME                  READY     STATUS      RESTARTS   AGE
    nvidia-smi-ppkd4      0/1       Completed   0          36s
    ```
    {: screen}

4.  Beschreiben Sie den Pod, um zu sehen, wie das GPU-Einheiten-Plug-in den Pod terminiert hat.
    * In den Feldern `Limits` und `Requests` entspricht die von Ihnen angegebene Ressourcengrenze der Anforderung, die das Einheiten-Plug-in automatisch festgelegt hat.
    * Überprüfen Sie in den Ereignissen, dass der Pod dem GPU-Workerknoten zugewiesen ist.

    ```
    kubectl describe pod nvidia-smi-ppkd4
    ```
    {: pre}

    Beispielausgabe:
    ```
    Name:           nvidia-smi-ppkd4
    Namespace:      default
    ...
    Limits:
     nvidia.com/gpu:  2
    Requests:
     nvidia.com/gpu:  2
    ...
    Events:
    Type    Reason                 Age   From                     Message
    ----    ------                 ----  ----                     -------
    Normal  Scheduled              1m    default-scheduler        Successfully assigned nvidia-smi-ppkd4 to 10.xxx.xx.xxx
    ...
    ```
    {: screen}

5.  Um sicherzustellen, dass der Job die GPU zum Berechnen der Workload verwendet hat, können Sie die entsprechenden Protokolle überprüfen. Mit dem Befehl `[ "/usr/test/nvidia-smi" ]` des Jobs wurde der GPU-Einheitenstatus auf dem GPU-Workerknoten abgefragt.

    ```
    kubectl logs nvidia-sim-ppkd4
    ```
    {: pre}

    Beispielausgabe:
    ```
    +-----------------------------------------------------------------------------+
    | NVIDIA-SMI 390.12                 Driver Version: 390.12                    |
    |-------------------------------+----------------------+----------------------+
    | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
    | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
    |===============================+======================+======================|
    |   0  Tesla K80           Off  | 00000000:83:00.0 Off |                  Off |
    | N/A   37C    P0    57W / 149W |      0MiB / 12206MiB |      0%      Default |
    +-------------------------------+----------------------+----------------------+
    |   1  Tesla K80           Off  | 00000000:84:00.0 Off |                  Off |
    | N/A   32C    P0    63W / 149W |      0MiB / 12206MiB |      1%      Default |
    +-------------------------------+----------------------+----------------------+

    +-----------------------------------------------------------------------------+
    | Processes:                                                       GPU Memory |
    |  GPU       PID   Type   Process name                             Usage      |
    |=============================================================================|
    |  No running processes found                                                 |
    +-----------------------------------------------------------------------------+
    ```
    {: screen}

    In diesem Beispiel wurden beide GPUs zum Ausführen des Jobs verwendet, da beide im Workerknoten terminiert wurden. Falls der Grenzwert auf '1' gesetzt ist, wird nur eine GPU angezeigt.

## Apps skalieren 
{: #app_scaling}

Mit Kubernetes können Sie die [horizontale Autoskalierung von Pods ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) aktivieren, um die Anzahl der Instanzen Ihrer Apps CPU-basiert automatisch zu erhöhen oder zu verringern.
{:shortdesc}

Suchen Sie Informationen zum Skalieren von Cloud Foundry-Anwendungen? Lesen Sie den Abschnitt zur [IBM Autoskalierung für {{site.data.keyword.Bluemix_notm}}](/docs/services/Auto-Scaling/index.html). 
{: tip}

Vorbemerkungen:
- [Richten Sie Ihre CLI](cs_cli_install.html#cs_cli_configure) (Befehlszeilenschnittstelle) auf Ihren Cluster aus.
- Die Heapsterüberwachung muss in dem Cluster bereitgestellt werden, den Sie automatisch skalieren möchten.

Schritte:

1.  Stellen Sie Ihre App für einen Cluster über die Befehlszeilenschnittstelle (CLI) bereit. Bei der Bereitstellung Ihrer App müssen Sie CPU anfordern.

    ```
    kubectl run <appname> --image=<image> --requests=cpu=<cpu> --expose --port=<portnummer>
    ```
    {: pre}

    <table>
    <caption>Befehlskomponenten von 'kubectl run'</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Bestandteile dieses Befehls</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--image</code></td>
    <td>Die Anwendung, die bereitgestellt werden soll.</td>
    </tr>
    <tr>
    <td><code>--request=cpu</code></td>
    <td>Die erforderliche CPU für den Container. Die Angabe erfolgt in Millicore. Beispiel: <code>--requests=200m</code>.</td>
    </tr>
    <tr>
    <td><code>--expose</code></td>
    <td>Bei Angabe von 'true' wird ein externer Service erstellt.</td>
    </tr>
    <tr>
    <td><code>--port</code></td>
    <td>Der Port, an dem Ihre App extern verfügbar ist.</td>
    </tr></tbody></table>

    Für Bereitstellungen mit einem höheren Grad an Komplexität ist gegebenenfalls die Erstellung einer [Konfigurationsdatei](#app_cli) erforderlich.
    {: tip}

2.  Erstellen Sie einen automatischen Scaler und definieren Sie Ihre Richtlinie. Weitere Informationen zur Arbeit mit dem Befehl `kubectl autoscale` finden Sie in der [Dokumentation zu Kubernetes ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://v1-8.docs.kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#autoscale).

    ```
    kubectl autoscale deployment <bereitstellungsname> --cpu-percent=<prozentsatz> --min=<mindestwert> --max=<höchstwert>
    ```
    {: pre}

    <table>
    <caption>Befehlskomponenten von 'kubectl autoscale'</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Bestandteile dieses Befehls</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--cpu-percent</code></td>
    <td>Die durchschnittliche CPU-Auslastung, die durch die Funktion zur automatischen horizontalen Skalierung von Pods (Horizontal Pod Autoscaler) aufrechterhalten wird. Die Angabe erfolgt als Prozentsatz.</td>
    </tr>
    <tr>
    <td><code>--min</code></td>
    <td>Die Mindestanzahl von bereitgestellten Pods, die zur Aufrechterhaltung der angegebenen CPU-Auslastung verwendet werden.</td>
    </tr>
    <tr>
    <td><code>--max</code></td>
    <td>Die maximale Anzahl von bereitgestellten Pods, die zur Aufrechterhaltung der angegebenen CPU-Auslastung verwendet werden.</td>
    </tr>
    </tbody></table>


<br />


## Laufende Bereitstellungen verwalten
{: #app_rolling}

Sie können den Rollout Ihrer Änderungen auf eine automatisierte und gesteuerte Art verwalten. Wenn der Rollout nicht nach Plan verläuft, können Sie die Bereitstellung auf die vorherige Revision rückgängig machen.
{:shortdesc}

Erstellen Sie zunächst eine [Bereitstellung](#app_cli).

1.  [Implementieren ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#updating-a-deployment) Sie eine Änderung. Beispiel: Sie möchten das Image ändern, das Sie in Ihrer ursprünglichen Bereitstellung verwendet haben.

    1.  Rufen Sie den Namen der Bereitstellung ab.

        ```
        kubectl get deployments
        ```
        {: pre}

    2.  Rufen Sie den Namen des Pods ab.

        ```
        kubectl get pods
        ```
        {: pre}

    3.  Rufen Sie den Namen des Containers ab, der im Pod ausgeführt wird.

        ```
        kubectl describe pod <podname>
        ```
        {: pre}

    4.  Legen Sie das neue Image fest, das für die Bereitstellung verwendet werden soll.

        ```
        kubectl set image deployment/<bereitstellungsname><containername>=<imagename>
        ```
        {: pre}

    Wenn Sie die Befehle ausführen, wird die Änderung unverzüglich angewendet und im Rolloutprotokoll protokolliert.

2.  Überprüfen Sie den Status der Bereitstellung.

    ```
    kubectl rollout status deployments/<bereitstellungsname>
    ```
    {: pre}

3.  Machen Sie die Änderung rückgängig.
    1.  Zeigen Sie das Rolloutprotokoll für die Bereitstellung an und ermitteln Sie die Revisionsnummer Ihrer letzten Bereitstellung.

        ```
        kubectl rollout history deployment/<bereitstellungsname>
        ```
        {: pre}

        **Tipp:** Schließen Sie zum Anzeigen der Details für eine bestimmte Revision die Revisionsnummer ein.

        ```
        kubectl rollout history deployment/<bereitstellungsname> --revision=<nummer>
        ```
        {: pre}

    2.  Führen Sie ein Rollback auf die frühere Version durch oder geben Sie eine Revision an. Verwenden Sie für das Rollback auf die frühere Version den folgenden Befehl.

        ```
        kubectl rollout undo deployment/<bereitstellungsname> --to-revision=<nummer>
        ```
        {: pre}

<br />

