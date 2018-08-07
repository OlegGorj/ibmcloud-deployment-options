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




# Integration von Services
{: #integrations}

Sie können verschiedene externe Services und Katalogservices mit einem Kubernetes-Standardcluster in {{site.data.keyword.containerlong}} verwenden.
{:shortdesc}


## Anwendungsservices
<table summary="Zusammenfassung der Zugriffsmöglichkeiten">
<caption>Anwendungsservices</caption>
<thead>
<tr>
<th>Service</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.blockchainfull}}</td>
<td>Implementieren Sie eine öffentlich verfügbare Entwicklungsumgebung für IBM Blockchain in einem Kubernetes-Cluster in {{site.data.keyword.containerlong_notm}}. Verwenden Sie diese Umgebung für die Entwicklung und Anpassung Ihres eigenen Blockchain-Netzes, um Apps bereitzustellen, die ein nicht veränderbares Hauptbuch zur Aufzeichnung des Transaktionsprotokolls gemeinsam nutzen. Weitere Informationen finden Sie unter <a href="https://ibm-blockchain.github.io" target="_blank">In einer Cloud-Sandbox entwickeln - IBM Blockchain Platform <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>. </td>
</tr>
</tbody>
</table>

<br />



## DevOps-Services
<table summary="Zusammenfassung der Zugriffsmöglichkeiten">
<caption>DevOps-Services</caption>
<thead>
<tr>
<th>Service</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Codeship</td>
<td>Mit <a href="https://codeship.com" target="_blank">Codeship <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a> können Sie die kontinuierliche Integration und Bereitstellung von Containern vorantreiben. Weitere Informationen finden Sie unter <a href="https://www.ibm.com/blogs/bluemix/2017/10/using-codeship-pro-deploy-workloads-ibm-container-service/" target="_blank">Using Codeship Pro To Deploy Workloads to {{site.data.keyword.containershort_notm}} <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>. </td>
</tr>
<tr>
<td>Helm</td>
<td> <a href="https://helm.sh/" target="_blank">Helm <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a> ist ein Kubernetes-Paketmanager. Sie können neue Helm-Diagramme zum Definieren, Installieren und Durchführen von Upgrades für komplexe Kubernetes-Anwendungen, die in {{site.data.keyword.containerlong_notm}}-Clustern ausgeführt werden, erstellen. <p>Weitere Informationen finden Sie unter [Helm in {{site.data.keyword.containershort_notm}} konfigurieren](cs_integrations.html#helm).</p></td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>Automatisieren Sie die App-Builds und die Containerbereitstellungen in Kubernetes-Clustern mithilfe einer Toolchain. Informationen zur Konfiguration finden Sie in dem Blog <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Deploy Kubernetes pods to the {{site.data.keyword.containerlong_notm}} using DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>. </td>
</tr>
<tr>
<td>Istio</td>
<td><a href="https://www.ibm.com/cloud/info/istio" target="_blank">Istio <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a> ist ein Open-Source-Service, der Entwicklern eine Möglichkeit zum Verbinden, Sichern, Verwalten und Überwachen eines Netzes von Mikroservices (auch als Servicenetz bezeichnet) auf Cloudorchestrierungsplattformen wie Kubernetes bietet. Lesen Sie den Blogbeitrag darüber, <a href="https://developer.ibm.com/dwblog/2017/istio/" target="_blank">wie IBM Istio mitgegründet und auf den Markt gebracht hat <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>, um weitere Informationen zu dem Open-Source-Projekt zu erhalten. Weitere Informationen zum Installieren von Istio aud Ihrem Kubernetes-Cluster in {{site.data.keyword.containershort_notm}} und zu den ersten Schritten mit einer Beispiel-App erhalten Sie im [Lernprogramm: Mikroservices mit Istio verwalten](cs_tutorials_istio.html#istio_tutorial).</td>
</tr>
</tbody>
</table>

<br />



## Protokollierungs- und Überwachungsservices
<table summary="Zusammenfassung der Zugriffsmöglichkeiten">
<caption>Protokollierungs- und Überwachungsservices</caption>
<thead>
<tr>
<th>Service</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>CoScale</td>
<td>Überwachen Sie Workerknoten, Container, Replikatgruppen, Replikationscontroller und Services mit <a href="https://www.coscale.com/" target="_blank">CoScale <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>. Weitere Informationen finden Sie unter <a href="https://www.ibm.com/blogs/bluemix/2017/06/monitoring-ibm-bluemix-container-service-coscale/" target="_blank">Monitoring {{site.data.keyword.containershort_notm}} with CoScale <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>. </td>
</tr>
<tr>
<td>Datadog</td>
<td>Überwachen Sie Ihren Cluster und zeigen Sie Metriken für die Infrastruktur- und Anwendungsleistung mit <a href="https://www.datadoghq.com/" target="_blank">Datadog <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a> an. Weitere Informationen finden Sie unter <a href="https://www.ibm.com/blogs/bluemix/2017/07/monitoring-ibm-bluemix-container-service-datadog/" target="_blank">Monitoring {{site.data.keyword.containershort_notm}} with Datadog <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.loganalysisfull}}</td>
<td>Erweitern Sie Ihre Protokollerfassungs-, Aufbewahrungs- und Suchmöglichkeiten mit {{site.data.keyword.loganalysisfull_notm}}. Weitere Informationen finden Sie unter <a href="../services/CloudLogAnalysis/containers/containers_kube_other_logs.html" target="_blank">Automatische Erfassung von Clusterprotokollen aktivieren<img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.monitoringlong}}</td>
<td>Erweitern Sie Ihre Erfassungs- und Aufbewahrungsmöglichkeiten für Metriken, indem Sie Regeln und Alerts mit {{site.data.keyword.monitoringlong_notm}} definieren. Weitere Informationen finden Sie unter <a href="../services/cloud-monitoring/tutorials/container_service_metrics.html" target="_blank">Metriken in Grafana für eine App analysieren, die in einem Kubernetes-Cluster bereitgestellt wurde<img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>. </td>
</tr>
<tr>
<td>Instana</td>
<td> <a href="https://www.instana.com/" target="_blank">Instana <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a> bietet eine Leistungsüberwachung von Infrastrukturen und Apps über eine grafische Benutzerschnittstelle, die automatisch Apps erkennt und zuordnet. Istana erfasst alle Anforderungen an Ihre Apps und ermöglicht Ihnen damit die Durchführung von Fehler- und Ursachenanalysen, um zu vermeiden, dass Probleme erneut auftreten. Lesen Sie dazu den Blogeintrag zur <a href="https://www.instana.com/blog/precise-visibility-applications-ibm-bluemix-container-service/" target="_blank">Bereitstellung von Istana in {{site.data.keyword.containershort_notm}} <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>, um weitere Informationen zu erhalten.</td>
</tr>
<tr>
<td>Prometheus</td>
<td>Prometheus ist ein Open-Source-Tool für die Überwachung, Protokollierung und Benachrichtigung bei Alerts, das speziell für Kubernetes entwickelt wurde. Prometheus ruft auf Grundlage der Kubernetes-Protokollierungsinformationen detaillierte Informationen zu Cluster, Workerknoten und Bereitstellungszustand ab. Informationen zu CPU, Speicher, Ein-/Ausgabe und Netzaktivität werden für alle Container erfasst, die in einem Cluster ausgeführt werden. Sie können die erfassten Daten in angepassten Abfragen oder Alerts verwenden, um die Leistung und Workloads im Cluster zu überwachen.

<p>Um Prometheus zu verwenden, befolgen Sie die <a href="https://github.com/coreos/prometheus-operator/tree/master/contrib/kube-prometheus" target="_blank">CoreOS-Anweisungen <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>.</p>
</td>
</tr>
<tr>
<td>Sematext</td>
<td>Zeigen Sie Metriken und Protokolle für Ihre containerisierten Anwendungen mithilfe von <a href="https://sematext.com/" target="_blank">Sematext <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a> an. Weitere Informationen finden Sie unter <a href="https://www.ibm.com/blogs/bluemix/2017/09/monitoring-logging-ibm-bluemix-container-service-sematext/" target="_blank">Monitoring and logging for containers with Sematext <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>.</td>
</tr>
<tr>
<td>Sysdig</td>
<td>Erfassen Sie App-, Container-, statsd- und Hostmetriken über einen einzigen Instrumentierungspunkt mithilfe von <a href="https://sysdig.com/" target="_blank">Sysdig <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>. Weitere Informationen finden Sie unter <a href="https://www.ibm.com/blogs/bluemix/2017/08/monitoring-ibm-bluemix-container-service-sysdig-container-intelligence/" target="_blank">Monitoring {{site.data.keyword.containershort_notm}} with Sysdig Container Intelligence <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>. </td>
</tr>
<tr>
<td>Weave Scope</td>
<td>Weave Scope liefert eine grafisch orientierte Diagrammdarstellung Ihrer Ressourcen in einem Kubernetes-Cluster unter Einbeziehung von Services, Pods, Containern, Prozessen, Knoten und vielem mehr. Weave Scope stellt interaktive Metriken für CPU und Speicher bereit und bietet Tools, um 'tail'- und 'exec'-Aufrufe in einem Container durchzuführen.<p>Weitere Informationen finden Sie unter [Kubernetes-Clusterressourcen mit Weave Scope und {{site.data.keyword.containershort_notm}} grafisch darstellen](cs_integrations.html#weavescope).</p></li></ol>
</td>
</tr>
</tbody>
</table>

<br />



## Sicherheitsservices
<table summary="Zusammenfassung der Zugriffsmöglichkeiten">
<caption>Sicherheitsservices</caption>
<thead>
<tr>
<th>Service</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
  <tr id="appid">
    <td>{{site.data.keyword.appid_full}}</td>
    <td>Fügen Sie Ihren Apps mit [{{site.data.keyword.appid_short}}](/docs/services/appid/index.html#gettingstarted) eine Sicherheitsebene hinzu, indem Sie Benutzer dazu verpflichten, sich anzumelden. Um Web- oder API-HTTP/HTTPS-Anforderungen in Ihrer App zu authentifizieren, können Sie {{site.data.keyword.appid_short_notm}} in Ihren Ingress-Service integrieren, indem Sie die [{{site.data.keyword.appid_short_notm}}Annotation zur Ingress-Authentifizierung](cs_annotations.html#appid-auth) verwenden.</td>
  </tr>
<tr>
<td>Aqua Security</td>
  <td>Sie können <a href="/docs/services/va/va_index.html" target="_blank">Vulnerability Advisor</a> durch <a href="https://www.aquasec.com/" target="_blank">Aqua Security <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a> ergänzen, um die Sicherheit von Containerbereitstellungen zu optimieren, indem Sie die zulässigen Funktionen für Ihre Apps einschränken. Weitere Informationen finden Sie unter <a href="https://www.ibm.com/blogs/bluemix/2017/06/protecting-container-deployments-bluemix-aqua-security/" target="_blank">Protecting container deployments on {{site.data.keyword.Bluemix_notm}} with Aqua Security <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.cloudcerts_full}}</td>
<td>Sie können <a href="../services/certificate-manager/index.html" target="_blank">{{site.data.keyword.cloudcerts_long}} <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a> verwenden, um SSL-Zertifikate für Ihre Apps zu speichern und zu verwalten. Weitere Informationen finden Sie unter <a href="https://www.ibm.com/blogs/bluemix/2018/01/use-ibm-cloud-certificate-manager-ibm-cloud-container-service-deploy-custom-domain-tls-certificates/" target="_blank">Use {{site.data.keyword.cloudcerts_long_notm}} with {{site.data.keyword.containershort_notm}} to deploy custom domain TLS Certificates <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>. </td>
</tr>
<tr>
  <td>{{site.data.keyword.registrylong}}</td>
  <td>Richten Sie Ihr eigenes gesichertes Docker-Image-Repository ein, in dem Sie Images sicher speichern und mit Clusterbenutzern teilen können. Weitere Informationen finden Sie in der <a href="/docs/services/Registry/index.html" target="_blank">{{site.data.keyword.registrylong}}-Dokumentation <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>.</td>
</tr>
<tr>
<td>NeuVector</td>
<td>Schützen Sie Container durch eine native Cloud-Firewall mithilfe von <a href="https://neuvector.com/" target="_blank">NeuVector <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>. Weitere Informationen finden Sie unter <a href="https://www.ibm.com/us-en/marketplace/neuvector-container-security" target="_blank">NeuVector Container Security <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>. </td>
</tr>
<tr>
<td>Twistlock</td>
<td>Sie können <a href="/docs/services/va/va_index.html" target="_blank">Vulnerability Advisor</a> durch <a href="https://www.twistlock.com/" target="_blank">Twistlock <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a> ergänzen, um Firewalls, den Schutz vor Bedrohungen und die Behebung von Störfällen zu verwalten. Weitere Informationen finden Sie unter <a href="https://www.ibm.com/blogs/bluemix/2017/07/twistlock-ibm-bluemix-container-service/" target="_blank">Twistlock on {{site.data.keyword.containershort_notm}} <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>. </td>
</tr>
</tbody>
</table>

<br />



## Speicherservices
<table summary="Zusammenfassung der Zugriffsmöglichkeiten">
<caption>Speicherservices</caption>
<thead>
<tr>
<th>Service</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Heptio Ark</td>
  <td>Sie können <a href="https://github.com/heptio/ark" target="_blank">Heptio Ark <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a> verwenden, um Clusterressourcen und PVs (Persistent Volumes) zu sichern und wiederherzustellen. Weitere Informationen finden Sie in der Heptio Ark-Veröffentlichung zu <a href="https://github.com/heptio/ark/blob/master/docs/use-cases.md#use-cases" target="_blank">Anwendungsfällen für Disaster-Recovery und Clustermigration <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>.</td>
</tr>
<tr>
  <td>{{site.data.keyword.cos_full}}</td>
  <td>In {{site.data.keyword.cos_short}} gespeicherte Daten werden verschlüsselt und über mehrere geografische Regionen verteilt. Auf sie kann über HTTP mithilfe einer REST-API zugegriffen werden. Sie können den Befehl [ibm-backup-restore image](/docs/services/RegistryImages/ibm-backup-restore/index.html) verwenden, um den Service so zu konfigurieren, dass er einmalige oder geplante Sicherungen von Daten in Ihrem Cluster ausführt. Allgemeine Informationen zu dem Service finden Sie in der <a href="/docs/services/cloud-object-storage/about-cos.html" target="_blank">{{site.data.keyword.cos_short}}-Dokumentation <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>.</td>
</tr>
  <tr>
    <td>{{site.data.keyword.cloudantfull}}</td>
    <td>{{site.data.keyword.cloudant_short_notm}} ist eine dokumentorientierte Database as a Service (DBaaS), die Daten als Dokumente im JSON-Format speichert. Der Service ist auf Skalierbarkeit, Hochverfügbarkeit und Langlebigkeit ausgelegt. Weitere Informationen finden Sie in der <a href="/docs/services/Cloudant/getting-started.html" target="_blank">{{site.data.keyword.cloudant_short_notm}}-Dokumentation <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>.</td>
  </tr>
  <tr>
    <td>{{site.data.keyword.composeForMongoDB_full}}</td>
    <td>{{site.data.keyword.composeForMongoDB}} bietet Hochverfügbarkeit und Redundanz, automatisierte und bedarfsgesteuerte, unterbrechungsfreie Sicherungen, Überwachungstools, Integration in Alertsysteme, Leistungsanalyseansichten und vieles mehr. Weitere Informationen finden Sie in der <a href="/docs/services/ComposeForMongoDB/index.html" target="_blank">{{site.data.keyword.composeForMongoDB}}-Dokumentation <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>.</td>
  </tr>
</tbody>
</table>


<br />



## Cloud Foundry-Services zu Clustern hinzufügen
{: #adding_cluster}

Sie können eine vorhandene Cloud Foundry-Serviceinstanz
zu Ihrem Cluster hinzufügen, um den Benutzern Ihres Clusters den Zugriff auf den
Service sowie seine Verwendung zu ermöglichen, wenn sie eine App auf dem Cluster bereitstellen.
{:shortdesc}

Vorbemerkungen:

1. [Richten Sie Ihre CLI](cs_cli_install.html#cs_cli_configure) (Befehlszeilenschnittstelle) auf Ihren Cluster aus.
2. [Fordern Sie eine Instanz des {{site.data.keyword.Bluemix_notm}}-Service](/docs/apps/reqnsi.html#req_instance) an.
   **Hinweis:** Zur Erstellung einer Instanz eines Service am Standort 'Washington DC' müssen Sie die CLI verwenden.
3. Cloud Foundry-Services werden für die Bindung mit Clustern unterstützt, andere Services jedoch nicht. Sie können die verschiedenen Servicetypen sehen, nachdem Sie die Serviceinstanz erstellt haben und die Services im Dashboard als **Cloud Foundry-Services** und **Services** gruppiert sind. Um die Services im Abschnitt **Services** mit Clustern zu binden, [erstellen Sie zuerst Cloud Foundry-Aliasnamen](#adding_resource_cluster).

**Hinweis:**
<ul><ul>
<li>Es können nur {{site.data.keyword.Bluemix_notm}}-Services hinzugefügt werden, die Serviceschlüssel unterstützen. Wenn der Service keine Serviceschlüssel unterstützt, dann sollten Sie die Informationen zum Thema [Externen Apps die Verwendung von {{site.data.keyword.Bluemix_notm}}-Services ermöglichen](/docs/apps/reqnsi.html#accser_external) lesen.</li>
<li>Der Cluster und die Workerknoten müssen vollständig bereitgestellt werden, bevor Sie einen Service hinzufügen können.</li>
</ul></ul>


Gehen Sie wie folgt vor, um einen Service hinzuzufügen:
2.  Listen Sie verfügbare {{site.data.keyword.Bluemix_notm}}-Services auf.

    ```
    bx service list
    ```
    {: pre}

    CLI-Beispielausgabe:

    ```
    name                      service           plan    bound apps   last operation
    <serviceinstanzname>   <servicename>    spark                create succeeded
    ```
    {: screen}

3.  Notieren Sie unter **name** den Namen der Serviceinstanz, die Sie zu Ihrem Cluster hinzufügen wollen.
4.  Geben Sie den Clusternamensbereich an, den Sie verwenden wollen, um Ihren Service hinzuzufügen. Wählen Sie eine der folgenden Optionen aus.
    -   Lassen Sie eine Liste der vorhandenen Namensbereiche anzeigen und wählen Sie einen Namensbereich aus, den Sie verwenden wollen.

        ```
        kubectl get namespaces
        ```
        {: pre}

    -   Erstellen Sie einen Namensbereich in Ihrem Cluster.

        ```
        kubectl create namespace <name_des_namensbereichs>
        ```
        {: pre}

5.  Fügen Sie den Service zu Ihrem Cluster hinzu.

    ```
    bx cs cluster-service-bind <clustername_oder_-id> <namensbereich> <serviceinstanzname>
    ```
    {: pre}

    Wenn der Service erfolgreich zu Ihrem Cluster hinzugefügt worden ist, wird ein geheimer Schlüssel für den Cluster erstellt, der die Berechtigungsnachweise Ihrer Serviceinstanz enthält. CLI-Beispielausgabe:

    ```
    bx cs cluster-service-bind mein_cluster mein_namensbereich cleardb
    Binding service instance to namespace...
    OK
    Namespace: mein_namensbereich
    Secret name:     binding-<serviceinstanzname>
    ```
    {: screen}

6.  Stellen Sie sicher, dass der geheime Schlüssel im Namensbereich Ihres Clusters erstellt wurde.

    ```
    kubectl get secrets --namespace=<namensbereich>
    ```
    {: pre}

Um den Service in einem Pod zu verwenden, der im Cluster bereitgestellt ist, müssen Clusterbenutzer auf die Serviceberechtigungsnachweise zugreifen. Benutzer können auf die Serviceberechtigungsnachweise des {{site.data.keyword.Bluemix_notm}}-Service zugreifen, indem sie [den geheimen Kubernetes-Schlüssel als Datenträger für geheime Schlüssel an einen Pod anhängen](#adding_app).

<br />


## Cloud Foundry-Aliasnamen für andere Ressourcen des {{site.data.keyword.Bluemix_notm}}-Service erstellen
{: #adding_resource_cluster}

Cloud Foundry-Services werden für die Bindung mit Clustern unterstützt. Um einen {{site.data.keyword.Bluemix_notm}}-Service an Ihr Cluster zu binden, der kein Cloud Foundry-Service ist, erstellen Sie einen Cloud Foundry-Aliasnamen für die Serviceinstanz.
{:shortdesc}

[Fordern Sie zunächst eine Instanz des {{site.data.keyword.Bluemix_notm}}-Service an](/docs/apps/reqnsi.html#req_instance).

Gehen Sie wie folgt vor, um einen Cloud Foundry-Aliasnamen zu erstellen:

1. Geben Sie als Ziel die Organisation und den Bereich an, wo die Serviceinstanz erstellt wurde.

    ```
    bx target -o <organisationsname> -s <bereichsname>
    ```
    {: pre}

2. Notieren Sie den Namen der Serviceinstanz.
    ```
    bx resource service-instances
    ```
    {: pre}

3. Erstellen Sie einen Cloud Foundry-Aliasnamen für die Serviceinstanz.
    ```
    bx resource service-alias-create <servicealiasname> --instance-name <serviceinstanz>
    ```
    {: pre}

4. Überprüfen Sie, dass der Servicealiasname erstellt wurde.

    ```
    bx service list
    ```
    {: pre}

5. [Binden Sie den Cloud Foundry-Aliasnamen an den Cluster](#adding_cluster).



<br />


## Services zu Apps hinzufügen
{: #adding_app}

Zum Speichern der Detailinformationen und Berechtigungsnachweise für {{site.data.keyword.Bluemix_notm}}-Services und zur Sicherstellung der sicheren Kommunikation zwischen dem Service und dem Cluster werden verschlüsselte Kubernetes-Schlüssel verwendet.
{:shortdesc}

Geheime Kubernetes-Schlüssel stellen eine sichere Methode zum Speichern vertraulicher Informationen wie Benutzernamen, Kennwörter oder Schlüssel dar. Statt vertrauliche Informationen über Umgebungsvariablen oder direkt in der Dockerfile selbst offenzulegen, können Sie geheime Schlüssel an einen Pod anhängen. Auf diese geheimen Schlüssel kann anschließend über einen aktiven Container in einem Pod zugegriffen werden.

Wenn Sie einen Datenträger für geheime Schlüssel an Ihren Pod anhängen, wird im Mountverzeichnis des Datenträgers eine Datei namens `binding` gespeichert. Die Datei `binding` enthält sämtliche Informationen und Berechtigungsnachweise, die Sie benötigen, um auf den {{site.data.keyword.Bluemix_notm}}-Service zuzugreifen.

Führen Sie zunächst den folgenden Schritt aus: [Richten Sie Ihre CLI](cs_cli_install.html#cs_cli_configure) auf Ihren Cluster aus. Stellen Sie sicher, dass der {{site.data.keyword.Bluemix_notm}}-Service, den Sie in Ihrer App verwenden wollen, vom Clusteradministrator [zu dem Cluster hinzugefügt](cs_integrations.html#adding_cluster) wurde.

1.  Listen Sie die verfügbaren geheimen Schlüssel im Namensbereich Ihres Clusters auf.

    ```
    kubectl get secrets --namespace=<mein_namensbereich>
    ```
    {: pre}

    Beispielausgabe:

    ```
    NAME                                    TYPE                                  DATA      AGE
    binding-<serviceinstanzname>         Opaque                                1         3m

    ```
    {: screen}

2.  Suchen Sie nach einem Schlüssel des Typs **Opaque** und notieren Sie den **Namen** des geheimen Schlüssels. Sollten mehrere geheime Schlüssel vorhanden sein, wenden Sie sich an Ihren Clusteradministrator, damit dieser den geheimen Schlüssel für den gewünschten Service ermittelt.

3.  Öffnen Sie Ihren bevorzugten Editor.

4.  Erstellen Sie eine YAML-Datei, um einen Pod zu konfigurieren, der in der Lage ist, über einen Datenträger für geheime Schlüssel auf die Servicedetails zuzugreifen. Wenn Sie mehr als einen Service gebunden haben, stellen Sie sicher, dass jeder geheime Schlüssel dem richtigen Service zugeordnet ist.

    ```
    apiVersion: apps/v1beta1
    kind: Deployment
    metadata:
      labels:
        app: secret-test
      name: secret-test
      namespace: <mein_namensbereich>
    spec:
      selector:
        matchLabels:
          app: secret-test
      replicas: 1
      template:
        metadata:
          labels:
            app: secret-test
        spec:
          containers:
          - image: nginx
            name: secret-test
            volumeMounts:
            - mountPath: /opt/service-bind
              name: service-bind-volume
          volumes:
          - name: service-bind-volume
            secret:
              defaultMode: 420
              secretName: binding-<name_der_serviceinstanz>
    ```
    {: codeblock}

    <table>
    <caption>Erklärung der Komponenten der YAML-Datei</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der YAML-Dateikomponenten</th>
    </thead>
    <tbody>
    <tr>
    <td><code>volumeMounts/mountPath</code></td>
    <td>Der Name des Datenträgers für geheime Schlüssel, der an den Container angehängt werden soll.</td>
    </tr>
    <tr>
    <td><code>volumes/name</code></td>
    <td>Geben Sie einen Namen für den Datenträger für geheime Schlüssel ein, der an den Container angehängt werden soll.</td>
    </tr>
    <tr>
    <td><code>secret/defaultMode</code></td>
    <td>Legen Sie Schreibschutz für den geheimen Schlüssel des Service fest.</td>
    </tr>
    <tr>
    <td><code>secret/secretName</code></td>
    <td>Geben Sie den Namen des geheimen Schlüssels ein, den Sie zuvor notiert haben.</td>
    </tr></tbody></table>

5.  Erstellen Sie den Pod und hängen Sie den Datenträger für geheime Schlüssel an.

    ```
    kubectl apply -f secret-test.yaml
    ```
    {: pre}

6.  Stellen Sie sicher, dass der Pod erstellt wurde.

    ```
    kubectl get pods --namespace=<mein_namensbereich>
    ```
    {: pre}

    CLI-Beispielausgabe:

    ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
    {: screen}

7.  Notieren Sie die Namensangabe für Ihren Pod unter **NAME**.
8.  Rufen Sie die Details zum Pod ab und suchen Sie den Namen des geheimen Schlüssels.

    ```
    kubectl describe pod <podname>
    ```
    {: pre}

    Ausgabe:

    ```
    ...
    Volumes:
      service-bind-volume:
        Type:       Secret (Datenträger, der mit einem geheimen Schlüssel belegt ist)
        SecretName: binding-<serviceinstanzname>
    ...
    ```
    {: screen}

    

9.  Konfigurieren Sie Ihre App beim Implementieren so, dass sie die Datei `binding` mit dem geheimen Schlüssel im Mountverzeichnis finden, den JSON-Inhalt parsen und die URL sowie die Berechtigungsnachweise für den Service ermitteln kann, um auf den {{site.data.keyword.Bluemix_notm}}-Service zuzugreifen.

Sie können nun auf die Details für den {{site.data.keyword.Bluemix_notm}}-Service und die zugehörigen Berechtigungsnachweise zugreifen. Um mit Ihrem {{site.data.keyword.Bluemix_notm}}-Service arbeiten zu können, stellen Sie sicher, dass Ihre App so konfiguriert ist, dass sie die Datei mit dem geheimen Schlüssel für den Service im Mountverzeichnis finden, den JSON-Inhalt parsen und die Servicedetails ermitteln kann.

<br />


## Helm in {{site.data.keyword.containershort_notm}} konfigurieren
{: #helm}

[Helm ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://helm.sh/) ist ein Kubernetes-Paketmanager. Sie können Helm-Diagramme zum Definieren, Installieren und Durchführen von Upgrades für komplexe Kubernetes-Anwendungen, die in {{site.data.keyword.containerlong_notm}}-Clustern ausgeführt werden, erstellen.
{:shortdesc}

Vor der Verwendung von Helm-Diagrammen mit {{site.data.keyword.containershort_notm}} müssen Sie eine Helm-Instanz in Ihrem Cluster installieren und initialisieren. Sie können anschließend das {{site.data.keyword.Bluemix_notm}}-Helm-Repository zu Ihrer Helm-Instanz hinzufügen.

Führen Sie zunächst den folgenden Schritt aus: [Geben Sie als Ziel der CLI](cs_cli_install.html#cs_cli_configure) den Cluster an, auf dem Sie ein Helm-Diagramm verwenden möchten.

1. Installieren Sie die <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm-CLI <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>.

2. **Wichtig**: Um Clustersicherheit zu bewahren, erstellen Sie ein Servicekonto für Tiller im Namensbereich `kube-system` und eine RBAC-Clusterrollenbindung für Kubernetes für den Pod `tiller-deploy`.

    1. Erstellen Sie in Ihrem bevorzugten Editor die folgende Datei und speichern Sie sie als `rbac-config.yaml`.
      **Hinweis**:
        * Die Clusterrolle `cluster-admin` wird standardmäßig in Kubernetes-Clustern erstellt, Sie müssen sie also nicht explizit definieren.
        * Wenn Sie ein Cluster der Version 1.7.x verwenden, ändern Sie `apiVersion` in `rbac.authorization.k8s.io/v1beta1`.

      ```
      apiVersion: v1
      kind: ServiceAccount
      metadata:
        name: tiller
        namespace: kube-system
      ---
      apiVersion: rbac.authorization.k8s.io/v1
      kind: ClusterRoleBinding
      metadata:
        name: tiller
      roleRef:
        apiGroup: rbac.authorization.k8s.io
        kind: ClusterRole
        name: cluster-admin
      subjects:
        - kind: ServiceAccount
          name: tiller
          namespace: kube-system
      ```
      {: codeblock}

    2. Erstellen Sie das Servicekonto und die Clusterrollenbindung.

        ```
        kubectl create -f rbac-config.yaml
        ```
        {: pre}

3. Initialisieren Sie Helm und installieren Sie `tiller` mit dem erstellten Servicekonto.

    ```
    helm init --service-account tiller
    ```
    {: pre}

4. Überprüfen Sie, dass der Pod `tiller-deploy` in Ihrem Cluster den **Status** `Running` aufweist.

    ```
    kubectl get pods -n kube-system -l app=helm
    ```
    {: pre}

    Beispielausgabe:

    ```
    NAME                            READY     STATUS    RESTARTS   AGE
    tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
    ```
    {: screen}

5. Fügen Sie das {{site.data.keyword.Bluemix_notm}}-Helm-Repository zu Ihrer Helm-Instanz hinzu.

    ```
    helm repo add ibm  https://registry.bluemix.net/helm/ibm
    ```
    {: pre}

6. Listen Sie die aktuell im {{site.data.keyword.Bluemix_notm}}-Repository verfügbaren Helm-Diagramme auf.

    ```
    helm search ibm
    ```
    {: pre}

7. Wenn Sie mehr über ein Diagramm erfahren möchten, listen Sie die zugehörigen Einstellungen und Standardwerte auf.

    So können Sie beispielsweise die Einstellungen, Dokumentation und Standardwerte für das Helm-Diagramm des strongSwan-IPSec-VPN-Service anzeigen:

    ```
    helm inspect ibm/strongswan
    ```
    {: pre}


### Zugehörige Helm-Links
{: #helm_links}

* Informationen zur Verwendung von StrongSwan finden Sie unter [Einrichtung von VPN-Konnektivität mit dem Helm-Diagramm des StrongSwan-IPSec-VPN-Service](cs_vpn.html#vpn-setup).
* Zeigen Sie verfügbare Helm-Diagramme an, die Sie mit {{site.data.keyword.Bluemix_notm}} in der GUI des [Helm-Diagrammkatalogs![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/containers-kubernetes/solutions/helm-charts) verwenden können.
* Weitere Informationen zu den Helm-Befehlen zum Konfigurieren und Verwalten von Helm-Diagrammen finden Sie in der <a href="https://docs.helm.sh/helm/" target="_blank">Helm-Dokumentation <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>.
* Über den folgenden Link finden Sie weitere Informationen zur Vorgehensweise bei der [Erhöhung der Bereitstellungsgeschwindigkeit mit Kubernetes-Helm-Diagrammen![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/recipes/tutorials/increase-deployment-velocity-with-kubernetes-helm-charts/).

## Kubernetes-Clusterressourcen grafisch darstellen
{: #weavescope}

Weave Scope liefert eine grafisch orientierte Diagrammdarstellung Ihrer Ressourcen in einem Kubernetes-Cluster unter Einbeziehung von Services, Pods, Containern und vielem mehr. Weave Scope stellt interaktive Metriken für CPU und Speicher bereit und bietet Tools, um 'tail'- und 'exec'-Aufrufe in einem Container durchzuführen.
{:shortdesc}

Vorbemerkungen:

-   Denken Sie daran, dass Sie keine Clusterinformationen im öffentlichen Internet offenlegen dürfen. Führen Sie diese Schritte aus, um Weave Scope sicher bereitzustellen und dann lokal über einen Web-Browser auf den Service zuzugreifen.
-   Wenn Sie nicht bereits über einen verfügen, [erstellen Sie einen Standardcluster](cs_clusters.html#clusters_ui). Weave Scope kann die CPU stark belasten, insbesondere die App. Führen Sie Weave Scope mit den größeren Standardclustern aus und nicht mit kostenlosen Clustern.
-   [Richten Sie Ihre CLI](cs_cli_install.html#cs_cli_configure) auf Ihren Cluster aus, `kubectl`-Befehle auszuführen.


Gehen Sie wie folgt vor, um Weave Scope mit einem Cluster zu verwenden:
2.  Stellen Sie eine der zur Verfügung gestellten Konfigurationsdatei für die RBAC-Berechtigungen im Cluster bereit.

    Gehen Sie wie folgt vor, um die Schreib-/Leseberechtigung zu aktivieren:

    ```
    kubectl apply -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac.yaml"
    ```
    {: pre}

    Gehen Sie wie folgt vor, um die Schreibberechtigung zu aktivieren:

    ```
    kubectl apply --namespace weave -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac-readonly.yaml"
    ```
    {: pre}

    Ausgabe:

    ```
    clusterrole "weave-scope-mgr" created
    clusterrolebinding "weave-scope-mgr-role-binding" created
    ```
    {: screen}

3.  Stellen Sie den Weave Scope-Service bereit, auf den über die IP-Adresse des Clusters privat zugegriffen werden kann.

    <pre class="pre">
    <code>kubectl apply --namespace weave -f "https://cloud.weave.works/k8s/scope.yaml?k8s-version=$(kubectl version | base64 | tr -d '&bsol;n')"</code>
    </pre>

    Ausgabe:

    ```
    serviceaccount "weave-scope" created
    deployment "weave-scope-app" created
    service "weave-scope-app" created
    daemonset "weave-scope-agent" created
    ```
    {: screen}

4.  Führen Sie einen Befehl zur Portweiterleitung aus, um den Service auf dem Computer zu öffnen. Beim nächsten Zugriff auf Weave Scope können Sie diesen Befehl zur Portweiterleitung ausführen, ohne wieder die vorherigen Konfigurationsschritte durchführen zu müssen.

    ```
    kubectl port-forward -n weave "$(kubectl get -n weave pod --selector=weave-scope-component=app -o jsonpath='{.items..metadata.name}')" 4040
    ```
    {: pre}

    Ausgabe:

    ```
    Forwarding from 127.0.0.1:4040 -> 4040
    Forwarding from [::1]: :1]:4040 -> 4040
    Handling connection for 4040
    ```
    {: screen}

5.  Öffnen Sie Ihren Web-Browser mit der Adresse `http://localhost:4040`. Ohne die Bereitstellung der Standardkomponenten wird das folgende Diagramm angezeigt. Sie können entweder Topologiediagramme oder Tabellen in den Kubernetes-Ressourcen im Cluster anzeigen.

     <img src="images/weave_scope.png" alt="Beispieltopologie von Weave Scope" style="width:357px;" />


[Weitere Informationen zu den Weave Scope-Funktionen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.weave.works/docs/scope/latest/features/).

<br />

