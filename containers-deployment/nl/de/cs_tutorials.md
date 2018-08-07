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



# Lernprogramm: Cluster erstellen
{: #cs_cluster_tutorial}

Stellen Sie einen Kubernetes-Cluster in {{site.data.keyword.containerlong}} bereit und verwalten Sie ihn darin. Sie können die Bereitstellung, den Betrieb, die Skalierung und Überwachung containerisierter Apps in einem Cluster automatisieren.
{:shortdesc}

In der vorliegenden Reihe von Lernprogrammen werden Sie erfahren, wie eine fiktive Public-Relations-Firma Kubernetes-Funktionalität verwendet, um eine containerisierte App in {{site.data.keyword.Bluemix_notm}} bereitzustellen. Diese PR-Firma nutzt {{site.data.keyword.toneanalyzerfull}} zur Analyse ihrer Pressemitteilungen und zum Empfang von Feedback.


## Ziele

Im ersten Lernprogramm fungieren Sie als Netzadministrator der PR-Firma. Sie konfigurieren einen angepassten Kubernetes-Cluster, der zur Bereitstellung und zum Testen einer Hello World-Version der App verwendet wird.

Führen Sie die folgenden Schritte aus, um die Infrastruktur einzurichten:

-   Erstellen Sie einen Cluster mit einem Workerknoten.
-   Installieren Sie die CLIs zum Ausführen von Kubernetes-Befehlen und Verwalten von Docker-Images.
-   Erstellen Sie ein privates Image-Repository in {{site.data.keyword.registrylong_notm}} zum Speichern der Images.
-   Fügen Sie den {{site.data.keyword.toneanalyzershort}}-Service zum Cluster hinzu, sodass alle Apps im Cluster diesen Service verwenden können.


## Erforderlicher Zeitaufwand

40 Minuten


## Zielgruppe

Dieses Lernprogramm ist für Softwareentwickler und Netzadministratoren konzipiert, die zum ersten Mal einen Kubernetes-Cluster erstellen.


## Voraussetzungen

-  Ein nutzungsabhängiges [{{site.data.keyword.Bluemix_notm}}-Konto oder ein Abonnementkonto ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/registration/)
-  Die [Cloud Foundry-Entwicklerrolle](/docs/iam/mngcf.html#mngcf) in dem Clusterbereich, in dem Sie arbeiten möchten.


## Lerneinheit 1: Cluster erstellen und CLI einrichten
{: #cs_cluster_tutorial_lesson1}

Erstellen Sie Ihren Cluster in der GUI und installieren Sie die erforderlichen CLIs.
{: shortdesc}

**Gehen Sie wie folgt vor, um den Cluster zu erstellen:**

Da die Bereitstellung des Clusters einige Minuten dauern kann, erstellen Sie den Cluster, bevor Sie die CLIs installieren.

1.  [Erstellen Sie in der GUI ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/containers-kubernetes/catalog/cluster/create) einen kostenlosen oder Standardcluster mit einem Workerknoten.

    Sie können auch einen [Cluster in der CLI](cs_clusters.html#clusters_cli) erstellen.
    {: tip}

Installieren Sie bei der Bereitstellung des Clusters die folgenden CLIs, die für die Verwaltung von Clustern verwendet werden:
-   {{site.data.keyword.Bluemix_notm}}-CLI
-   {{site.data.keyword.containershort_notm}}-Plug-in
-   Kubernetes-CLI
-   {{site.data.keyword.registryshort_notm}}-Plug-in
-   Docker-CLI

</br>
**Gehen Sie wie folgt vor, um die CLIs (Befehlszeilenschnittstellen) und die zugehörigen Voraussetzungen zu installieren:**

1.  Als Voraussetzung für das {{site.data.keyword.containershort_notm}}-Plug-in müssen Sie die [{{site.data.keyword.Bluemix_notm}}-CLI ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://clis.ng.bluemix.net/ui/home.html) installieren. Verwenden Sie zur Ausführung von {{site.data.keyword.Bluemix_notm}}-CLI-Befehlen das Präfix `bx`.
2.  Folgen Sie den Eingabeaufforderungen, um ein Konto und eine {{site.data.keyword.Bluemix_notm}}-Organisation auszuwählen. Cluster sind zwar kontospezifisch, besitzen jedoch keine Abhängigkeit zu einer {{site.data.keyword.Bluemix_notm}}-Organisation oder einem Bluemix-Bereich.

4.  Installieren Sie das {{site.data.keyword.containershort_notm}}-Plug-in, um Kubernetes-Cluster zu erstellen und Workerknoten zu verwalten. Verwenden Sie zur Ausführung von {{site.data.keyword.containershort_notm}}-Plug-in-Befehlen das Präfix `bx cs`.

    ```
    bx plugin install container-service -r Bluemix
    ```
    {: pre}

5.  Um die Apps in Ihren Clustern bereitzustellen, müssen Sie die [Kubernetes-CLI installieren ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/tasks/tools/install-kubectl/). Um Befehle über die Kubernetes-CLI auszuführen, müssen Sie das Präfix `kubectl` verwenden.
    1.  Zur Erreichung der vollständigen funktionalen Kompatibilität müssen Sie die Kubernetes-CLI-Version herunterladen, die mit der Version des Kubernetes-Clusters übereinstimmt, die verwendet werden soll. Die aktuelle standardmäßige Kubernetes-Version für {{site.data.keyword.containershort_notm}} ist die Version 1.9.7.

        OS X:   [https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/darwin/amd64/kubectl ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/darwin/amd64/kubectl)

        Linux:   [https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/linux/amd64/kubectl ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/linux/amd64/kubectl)

        Windows:   [https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/windows/amd64/kubectl.exe ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/windows/amd64/kubectl.exe)

          **Tipp:** Wenn Sie Windows verwenden, installieren Sie die Kubernetes-CLI in demselben Verzeichnis wie die {{site.data.keyword.Bluemix_notm}}-CLI. Diese Konstellation erspart Ihnen einige Dateipfadänderungen, wenn Sie spätere Befehle ausführen.

    2.  Wenn Sie mit OS X oder Linux arbeiten, dann führen Sie die folgenden Schritte aus.
        1.  Verschieben Sie die ausführbare Datei in das Verzeichnis `/usr/local/bin`.

            ```
            mv filepath/kubectl /usr/local/bin/kubectl
            ```
            {: pre}

        2.  Stellen Sie sicher, dass `/usr/local/bin` in der Variablen `PATH` Ihres Systems aufgelistet wird. Die Variable `PATH` enthält alle Verzeichnisse, in denen Ihr Betriebssystem ausführbare Dateien finden kann. Die Verzeichnisse, die in der Variablen `PATH` aufgelistet sind, erfüllen jedoch eine andere Funktion. Im Verzeichnis `/usr/local/bin` werden ausführbare Dateien für Software gespeichert, die nicht Bestandteil des Betriebssystem ist und vom Systemadministrator manuell installiert wurde.

            ```
            echo $PATH
            ```
            {: pre}

            Die Ausgabe in der Befehlszeilenschnittstelle (CLI) ähnelt der folgenden:

            ```
            /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
            ```
            {: screen}

        3.  Konvertieren Sie die Datei in eine ausführbare Datei.

            ```
            chmod +x /usr/local/bin/kubectl
            ```
            {: pre}

6. Installieren Sie zum Einrichten und Verwalten eines privaten Image-Repositorys in {{site.data.keyword.registryshort_notm}} das {{site.data.keyword.registryshort_notm}}-Plug-in. Verwenden Sie zur Ausführung von Registry-Befehlen das Präfix `bx cr`.

    ```
    bx plugin install container-registry -r Bluemix
    ```
    {: pre}

    Überprüfen Sie durch Ausführen des folgenden Befehls, ob die Plug-ins 'container-service' und 'container-registry' ordnungsgemäß installiert wurden:

    ```
    bx plugin list
    ```
    {: pre}

7. Um Images lokal zu erstellen und per Push-Operation an das private Image-Repository zu übertragen, müssen Sie die [Docker Community Edition-CLI installieren ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.docker.com/community-edition#/download). Wenn Sie Windows 8 oder älter verwenden, können Sie stattdessen [Docker Toolbox ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.docker.com/toolbox/toolbox_install_windows/) installieren.

Glückwunsch! Sie haben die CLIs für die folgenden Lerneinheiten und Lernprogramme erfolgreich installiert. Als Nächstes werden Sie die Clusterumgebung einrichten und den {{site.data.keyword.toneanalyzershort}}-Service hinzufügen.


## Lerneinheit 2: Private Registry einrichten
{: #cs_cluster_tutorial_lesson2}

Richten Sie ein privates Image-Repository in {{site.data.keyword.registryshort_notm}} ein und fügen Sie geheime Schlüssel zu Ihrem Cluster hinzu, sodass die App auf den {{site.data.keyword.toneanalyzershort}}-Service zugreifen kann.
{: shortdesc}

1.  Melden Sie sich bei der {{site.data.keyword.Bluemix_notm}}-CLI mithilfe Ihrer {{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise an, wenn Sie dazu aufgefordert werden.

    ```
    bx login [--sso]
    ```
    {: pre}

    **Hinweis:** Wenn Sie über eine eingebundene ID verfügen, dann geben Sie das Flag `--sso` an, um sich anzumelden. Geben Sie Ihren Benutzernamen ein und verwenden Sie die bereitgestellte URL in Ihrer CLI-Ausgabe, um Ihren einmaligen Kenncode abzurufen.

2.  Richten Sie Ihr eigenes privates Image-Repository in {{site.data.keyword.registryshort_notm}} ein, um Docker-Images sicher zu speichern und mit allen Benutzern des Clusters gemeinsam nutzen zu können. In {{site.data.keyword.Bluemix_notm}} ist ein privates Image-Repository durch einen Namensbereich gekennzeichnet. Der Namensbereich dient der Erstellung einer eindeutigen URL zu Ihrem Image-Repository, das Entwickler für den Zugriff auf private Docker-Images verwenden können.

    Erfahren Sie mehr über das [Sichern der persönliche Daten](cs_secure.html#pi) bei der Arbeit mit Container-Images.

    Im vorliegenden Beispiel will das PR-Unternehmen lediglich ein Image-Repository in {{site.data.keyword.registryshort_notm}} erstellen und wählt daher _pr-unternehmen_ als Namensbereich aus, um alle Images im Konto zu gruppieren. Ersetzen Sie _&lt;namensbereich&gt;_ durch einen Namensbereich Ihrer Wahl, der sich nicht auf das Lernprogramm bezieht.

    ```
    bx cr namespace-add <namensbereich>
    ```
    {: pre}

3.  Vergewissern Sie sich zuerst, dass die Bereitstellung Ihres Workerknotens abgeschlossen ist, bevor Sie mit dem nächsten Schritt fortfahren.

    ```
    bx cs workers <clustername_oder_-id>
    ```
    {: pre}

    Wenn die Bereitstellung Ihres Workerknotens abgeschlossen ist, wechselt der Status zu **Bereit**. Sie können nun mit dem Binden von {{site.data.keyword.Bluemix_notm}}-Services beginnen.

    ```
    ID                                                 Public IP       Private IP       Machine Type   State    Status   Location   Version
    kube-mil01-pafe24f557f070463caf9e31ecf2d96625-w1   169.xx.xxx.xxx   10.xxx.xx.xxx   free           normal   Ready    mil01      1.9.7
    ```
    {: screen}

## Lerneinheit 3: Clusterumgebung einrichten
{: #cs_cluster_tutorial_lesson3}

Legen Sie in Ihrer CLI (Befehlszeilenschnittstelle) den Kontext für Ihren Cluster fest.
{: shortdesc}

Jedes Mal, wenn Sie sich an der {{site.data.keyword.containerlong}}-CLI anmelden, um mit Clustern zu arbeiten, müssen Sie diese Befehle ausführen, um den Pfad zu der Konfigurationsdatei des Clusters als Sitzungsvariable festzulegen. Anhand dieser Variablen sucht die Kubernetes-CLI eine lokale Konfigurationsdatei und Zertifikate, die zum Herstellen der Verbindung zum Cluster in {{site.data.keyword.Bluemix_notm}} erforderlich sind.

1.  Ermitteln Sie den Befehl zum Festlegen der Umgebungsvariablen und laden Sie die Kubernetes-Konfigurationsdateien herunter.

    ```
    bx cs cluster-config <clustername_oder_-id>
    ```
    {: pre}

    Wenn
der Download der Konfigurationsdateien abgeschlossen ist, wird ein Befehl angezeigt, den Sie verwenden können,
um den Pfad zu der lokalen Kubernetes-Konfigurationsdatei als Umgebungsvariable festzulegen.

    Beispiel für OS X:

    ```
    export KUBECONFIG=/Users/<benutzername>/.bluemix/plugins/container-service/clusters/pr_firm_cluster/kube-config-prod-par02-pr_firm_cluster.yml
    ```
    {: screen}

2.  Kopieren Sie den Befehl, der in Ihrem Terminal angezeigt wird, um die Umgebungsvariable `KUBECONFIG` festzulegen.

3.  Stellen Sie sicher, dass die Umgebungsvariable `KUBECONFIG` richtig eingestellt ist.

    Beispiel für OS X:

    ```
    echo $KUBECONFIG
    ```
    {: pre}

    Ausgabe:

    ```
    /Users/<benutzername>/.bluemix/plugins/container-service/clusters/pr_firm_cluster/kube-config-prod-par02-pr_firm_cluster.yml
    ```
    {: screen}

4.  Stellen Sie sicher, dass die `kubectl`-Befehle mit Ihrem Cluster ordnungsgemäß ausgeführt werden. Überprüfen Sie dazu die Serverversion der Kubernetes-CLI wie folgt.

    ```
    kubectl version  --short
    ```
    {: pre}

    Beispielausgabe:

    ```
    Client Version: v1.9.7
    Server Version: v1.9.7
    ```
    {: screen}

## Lerneinheit 4: Einen Service zum Cluster hinzufügen
{: #cs_cluster_tutorial_lesson4}

Mit den {{site.data.keyword.Bluemix_notm}}-Services können Sie bereits entwickelte Funktionen in Ihren Apps nutzen. Jeder an den Cluster gebundene {{site.data.keyword.Bluemix_notm}}-Service kann von jeder App genutzt werden, die in diesem Cluster bereitgestellt ist. Wiederholen Sie die folgenden Schritte für jeden {{site.data.keyword.Bluemix_notm}}-Service, den Sie in Verbindung mit Ihren Apps verwenden wollen.

1.  Fügen Sie den {{site.data.keyword.toneanalyzershort}}-Service zu Ihrem {{site.data.keyword.Bluemix_notm}}-Konto hinzu. Ersetzen Sie <servicename> durch einen Namen für Ihre Serviceinstanz.

    **Hinweis:** Wenn Sie den {{site.data.keyword.toneanalyzershort}}-Service zu Ihrem Konto hinzufügen, wird eine Nachricht mit dem Inhalt angezeigt, dass der Service nicht kostenlos ist. Wenn Sie Ihren API-Aufruf einschränken, entstehen im Rahmen dieses Lernprogramms keine Kosten durch den {{site.data.keyword.watson}}-Service. [Informieren Sie sich über die Preisinformationen für den {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}}-Service ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/watson/developercloud/tone-analyzer.html#pricing-block).

    ```
    bx service create tone_analyzer standard <servicename>
    ```
    {: pre}

2.  Binden Sie die {{site.data.keyword.toneanalyzershort}}-Instanz an den Kubernetes-Standardnamensbereich (`default`) für den Cluster. Später können Sie Ihren eigenen Namensbereich erstellen, um Benutzerzugriff auf Kubernetes-Ressourcen zu verwalten. Aber für den Moment müssen Sie den Namensbereich `default` verwenden. Kubernetes-Namensbereiche unterscheiden sich von dem Registry-Namensbereich, den Sie davor erstellt haben.

    ```
    bx cs cluster-service-bind <clustername> default <servicename>
    ```
    {: pre}

    Ausgabe:

    ```
    bx cs cluster-service-bind pr_firm_cluster default mytoneanalyzer
    Binding service instance to namespace...
    OK
    Namespace:	default
    Secret name:	binding-mytoneanalyzer
    ```
    {: screen}

3.  Stellen Sie sicher, dass der geheime Kubernetes-Schlüssel im Namensbereich Ihres Clusters erstellt wurde. Jeder {{site.data.keyword.Bluemix_notm}}-Service ist durch eine JSON-Datei definiert, die vertrauliche Informationen, wie zum Beispiel den Benutzernamen, das Kennwort und die URL enthält, über die der Container Zugriff erhält. Kubernetes verwendet geheime Schlüssel, um diese Informationen sicher zu speichern. Im vorliegenden Beispiel enthält der geheime Schlüssel die Berechtigungsnachweise für den Zugriff auf die {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}}-Instanz, die in Ihrem Konto bereitgestellt wird.

    ```
    kubectl get secrets --namespace=default
    ```
    {: pre}

    Ausgabe:

    ```
    NAME                       TYPE                                  DATA      AGE
    binding-mytoneanalyzer     Opaque                                1         1m
    bluemix-default-secret     kubernetes.io/dockercfg               1         1h
    default-token-kf97z        kubernetes.io/service-account-token   3         1h
    ```
    {: screen}

</br>
Ganz hervorragend! Der Cluster ist konfiguriert und Ihre lokale Umgebung ist jetzt so vorbereitet, dass Sie mit der Bereitstellung von Apps im Cluster beginnen können.

## Womit möchten Sie fortfahren?
{: #next}

* Testen Sie Ihr Wissen [in einem Quiz ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://ibmcloud-quizzes.mybluemix.net/containers/cluster_tutorial/quiz.php)!

* Machen Sie sich mit dem [Lernprogramm: Apps in Kubernetes-Clustern in {{site.data.keyword.containershort_notm}} bereitstellen](cs_tutorials_apps.html#cs_apps_tutorial) vertraut, um die App der PR-Firma in dem von Ihnen erstellten Cluster bereitzustellen.
