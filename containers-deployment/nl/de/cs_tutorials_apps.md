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


# Lernprogramm: Apps in Clustern bereitstellen
{: #cs_apps_tutorial}

Hier erfahren Sie, wie Sie mit {{site.data.keyword.containerlong}} eine containerisierte App bereitstellen, die den {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} nutzt.
{: shortdesc}

In diesem Szenario nutzt ein fiktives PR-Unternehmen den {{site.data.keyword.Bluemix_notm}}-Service, um Pressemitteilungen zu analysieren und Feedback zum Tenor in ihren Nachrichten zu erhalten.

Der App-Entwickler des PR-Unternehmens verwendet den im vorherigen Lernprogramm erstellten Kubernetes-Cluster, um eine 'Hello World'-Version der App bereitzustellen. In den aufeinander aufbauenden Lerneinheiten dieses Lernprogramms stellt der App-Entwickler zunehmend komplexere Versionen derselben App bereit. Das folgende Diagramm zeigt die Komponenten der Bereitstellungen in den einzelnen Lerneinheiten.


![Komponenten der Lerneinheit](images/cs_app_tutorial_roadmap.png)

Wie im Diagramm dargestellt, verwendet Kubernetes verschiedene Typen von Ressourcen, um Ihre Apps betriebsbereit für die Ausführung in Clustern zu gestalten. In Kubernetes arbeiten Bereitstellungen und Services zusammen. Bereitstellungen enthalten die Definitionen für die App. Dazu zählen beispielsweise das Image, das für den Container verwendet werden soll, oder die Angabe, welcher Port für die App zugänglich gemacht werden muss. Wenn Sie eine Bereitstellung erstellen, wird für jeden Container, den Sie in der Bereitstellung definiert haben, ein Kubernetes-Pod erstellt. Um Ihre App widerstandsfähiger zu machen, können Sie in Ihrer Bereitstellung mehrere Instanzen derselben App definieren und von Kubernetes automatisch eine Replikatgruppe erstellen lassen. Die Replikatgruppe überwacht die Pods und stellt sicher, dass die angegebene Anzahl von Pods immer betriebsbereit ist. Reagiert einer der Pods nicht mehr, so wird dieser automatisch neu erstellt.

Services fassen eine Gruppe von Pods zusammen und stellen diesen Pods eine Netzverbindung für andere Services im Cluster bereit, ohne hierbei die tatsächlichen privaten IP-Adressen der einzelnen Pods preiszugeben. Mit Kubernetes-Services können Sie eine App anderen Pods im Cluster zur Verfügung stellen oder über das Internet verfügbar machen. Im vorliegenden Lernprogramm verwenden Sie einen Kubernetes-Service, um über eine öffentliche IP-Adresse, die automatisch einem Workerknoten zugewiesen wird, und einen öffentlichen Port vom Internet aus auf Ihre aktive App zuzugreifen.

Um die Verfügbarkeit Ihrer App noch weiter zu steigern, können Sie bei Standardclustern mehrere Workerknoten erstellen, die eine noch größere Anzahl an Replikaten der App ausführen. Obwohl diese Task im vorliegenden Lernprogramm nicht behandelt wird, sollten Sie dieses Konzept im Hinterkopf behalten, wenn Sie zu einem späteren Zeitpunkt die Verfügbarkeit einer App weiter verbessern möchten.

Die Integration eines {{site.data.keyword.Bluemix_notm}}-Service in eine App wird nur in einer Lerneinheit behandelt. Sie können Services jedoch mit so einfach oder so komplex konzipierten Apps verwenden, wie überhaupt vorstellbar.

## Ziele

* Grundlegende Kubernetes-Terminologie verstehen
* Image mit Push-Operation in Registry-Namensbereich in {{site.data.keyword.registryshort_notm}} übertragen
* App öffentlich zugänglich machen
* Einzelne Instanz einer App mit einem Kubernetes-Befehl und eine Script in einem Cluster bereitstellen
* Mehrere Instanzen einer App in Containern bereitstellen, die bei Statusprüfungen neu erstellt werden
* Eine App implementieren, die Funktionalität eines {{site.data.keyword.Bluemix_notm}}-Service verwendet

## Erforderlicher Zeitaufwand

40 Minuten

## Zielgruppen

Softwareentwickler und Netzadministratoren, die erstmalig eine App in einem Kubernetes-Cluster bereitstellen.

## Voraussetzungen

* [Lernprogramm: Kubernetes-Cluster in {{site.data.keyword.containershort_notm}}](cs_tutorials.html#cs_cluster_tutorial) erstellen.


## Lerneinheit 1: Einzelinstanz-Apps auf Kubernetes-Clustern bereitstellen
{: #cs_apps_tutorial_lesson1}

Im vorherigen Lernprogramm haben Sie einen Cluster mit einem Workerknoten erstellt. In dieser Lerneinheit konfigurieren Sie eine Bereitstellung und stellen eine einzelne Instanz der App in einem Kubernetes-Pod im Workerknoten bereit.
{:shortdesc}

Das folgende Diagramm zeigt die Komponenten, die Sie im Rahmen dieser Lerneinheit bereitstellen.


![Konfiguration für die Bereitstellung](images/cs_app_tutorial_components1.png)

Gehen Sie wie folgt vor, um die App bereitzustellen:

1.  Klonen Sie den Quellcode für die [App 'Hello World' ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/IBM/container-service-getting-started-wt) in Ihr Benutzerausgangsverzeichnis. Das Repository enthält verschiedene Versionen einer ähnlichen App in Ordnern, deren Namen jeweils mit `Lab` beginnen. Jede Version enthält die folgenden Dateien:
    * `Dockerfile`: Die Builddefinitionen für das Image.
    * `app.js`: Die App 'Hello World'.
    * `package.json`: Metadaten zur App.

    ```
    git clone https://github.com/IBM/container-service-getting-started-wt.git
    ```
    {: pre}

2.  Navigieren Sie zum Verzeichnis `Lab 1`.

    ```
    cd 'container-service-getting-started-wt/Lab 1'
    ```
    {: pre}

3.  Melden Sie sich an der {{site.data.keyword.Bluemix_notm}}-CLI an. Geben Sie Ihre {{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise ein, wenn Sie dazu aufgefordert werden. Um eine {{site.data.keyword.Bluemix_notm}}-Region anzugeben, verwenden Sie den Befehl `bx cs region-set`.

    ```
    bx login [--sso]
    ```
    {: pre}

    **Hinweis**: Wenn der Anmeldebefehl fehlschlägt, verwenden Sie möglicherweise eine eingebundene ID. Versuchen Sie, das Flag `--sso` an den Befehl anzufügen. Verwenden Sie die bereitgestellte URL in Ihrer CLI-Ausgabe, um einen einmaligen Kenncode abzurufen.

4.  Legen Sie in Ihrer CLI (Befehlszeilenschnittstelle) den Kontext für den Cluster fest.
    1.  Ermitteln Sie den Befehl zum Festlegen der Umgebungsvariablen und laden Sie die Kubernetes-Konfigurationsdateien herunter.

        ```
        bx cs cluster-config <clustername_oder_-id>
        ```
        {: pre}

        Wenn
der Download der Konfigurationsdateien abgeschlossen ist, wird ein Befehl angezeigt, den Sie verwenden können,
um den Pfad zu der lokalen Kubernetes-Konfigurationsdatei als Umgebungsvariable festzulegen.
    2.  Kopieren Sie die Ausgabe und fügen Sie sie ein, um die Umgebungsvariable `KUBECONFIG` festzulegen.

        Beispiel für OS X:

        ```
        export KUBECONFIG=/Users/<benutzername>/.bluemix/plugins/container-service/clusters/<pr-unternehmenscluster>/kube-config-prod-dal10-pr_firm_cluster.yml
        ```
        {: screen}

5.  Melden Sie sich an der {{site.data.keyword.registryshort_notm}}-CLI an. **Hinweis**: Stellen Sie sicher, dass das Plug-in 'container-registry' [installiert](/docs/services/Registry/index.html#registry_cli_install) ist.

    ```
    bx cr login
    ```
    {: pre}
    -   Wenn Sie Ihren Namensbereich in {{site.data.keyword.registryshort_notm}} vergessen haben, führen Sie den folgenden Befehl aus.

        ```
        bx cr namespace-list
        ```
        {: pre}

6.  Starten Sie Docker.
    * Wenn Sie Docker Community Edition verwenden, sind keine weiteren Maßnahmen erforderlich.
    * Wenn Sie Linux verwenden, durchsuchen Sie die [Docker-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.docker.com/engine/admin/) nach Anweisungen dazu, wie Docker abhängig von der verwendeten Linux-Distribution gestartet wird.
    * Wenn Sie Docker Toolbox unter Windows oder OSX verwenden, können Sie das Programm 'Docker Quickstart Terminal' verwenden, das Docker für Sie startet. Verwenden Sie 'Docker Quickstart Terminal' für die nächsten Schritte, um die Docker-Befehle auszuführen. Wechseln Sie anschließend zurück zur CLI (Befehlszeilenschnittstelle) und legen Sie dort die Sitzungsvariable `KUBECONFIG` fest.

7.  Erstellen Sie ein Docker-Image, das die App-Dateien aus dem Verzeichnis `Lab 1` enthält. Falls zu einem späteren Zeitpunkt Änderungen an der App vorgenommen werden sollen, wiederholen Sie diese Schritte, um eine weitere Version des Image zu erstellen.

    Erfahren Sie mehr über das [Sichern der persönliche Daten](cs_secure.html#pi) bei der Arbeit mit Container-Images.

    1.  Erstellen Sie das Image lokal. Geben Sie den Namen und den Tag an, den Sie verwenden möchten. Verwenden Sie unbedingt den Namensbereich, den Sie in {{site.data.keyword.registryshort_notm}} im vorherigen Lernprogramm erstellt haben. Durch die Kennzeichnung (das Tagging) des Image mit den Namensbereichsinformationen weiß Docker, wohin das Image in einem späteren Schritt per Push-Operation übertragen werden soll. Verwenden Sie im Imagenamen nur alphanumerische Zeichen in Kleinschreibung oder Unterstreichungszeichen (`_`). Vergessen Sie nicht den Punkt (`.`) am Ende des Befehls. Der Punkt signalisiert Docker, im aktuellen Verzeichnis nach der Dockerfile zu suchen und Artefakte zum Erstellen des Image zu erstellen.

        ```
        docker build -t registry.<region>.bluemix.net/<namensbereich>/hello-world:1 .
        ```
        {: pre}

        Überprüfen Sie nach der Beendigung des Buildprozesses, dass die folgende Nachricht über die erfolgreiche Ausführung angezeigt wird:
        ```
        Successfully built <image-id>
        Successfully tagged <image_tag>
        ```
        {: screen}

    2.  Übertragen Sie das Image mit einer Push-Operation in den Registry-Namensbereich.

        ```
        docker push registry.<region>.bluemix.net/<namensbereich>/hello-world:1
        ```
        {: pre}

        Beispielausgabe:

        ```
        The push refers to a repository [registry.ng.bluemix.net/pr_firm/hello-world]
  ea2ded433ac8: Pushed
  894eb973f4d3: Pushed
  788906ca2c7e: Pushed
  381c97ba7dc3: Pushed
  604c78617f34: Pushed
  fa18e5ffd316: Pushed
  0a5e2b2ddeaa: Pushed
  53c779688d06: Pushed
  60a0858edcd5: Pushed
  b6ca02dfe5e6: Pushed
  1: digest: sha256:0d90cb73288113bde441ae9b8901204c212c8980d6283fbc2ae5d7cf652405
  43 size: 2398
        ```
        {: screen}

8.  Bereitstellungen werden zum Verwalten von Pods verwendet, die containerisierte Instanzen einer App enthalten. Der folgende Befehl stellt die App in einem einzelnen Pod bereit. Im vorliegenden Lernprogramm wird der Bereitstellung der Name **hello-world-deployment** zugeordnet. Sie können jedoch einen eigenen Namen verwenden. Wenn Sie das Docker Quickstart-Terminal verwendet haben, um Docker-Befehle auszuführen, wechseln Sie unbedingt zurück zu der CLI (Befehlszeilenschnittstelle), die Sie zum Festlegen der Sitzungsvariablen `KUBECONFIG` verwendet haben.

    ```
    kubectl run hello-world-deployment --image=registry.<region>.bluemix.net/<namensbereich>/hello-world:1
    ```
    {: pre}

    Beispielausgabe:

    ```
    deployment "hello-world-deployment" created
    ```
    {: screen}

    Erfahren Sie mehr über das [Sichern der persönliche Daten](cs_secure.html#pi) bei der Arbeit mit Kubernetes-Ressourcen.

9.  Machen Sie die App zugänglich, indem Sie die Bereitstellung als Service vom Typ 'NodePort' zur Verfügung stellen. Genau wie beim Zugänglichmachen eines Ports für eine Cloud Foundry-App machen Sie als Knotenport (NodePort) den Port zugänglich, an dem der Workerknoten für Datenverkehr empfangsbereit ist.

    ```
    kubectl expose deployment/hello-world-deployment --type=NodePort --port=8080 --name=hello-world-service --target-port=8080
    ```
    {: pre}

    Beispielausgabe:

    ```
    service "hello-world-service" exposed
    ```
    {: screen}

    <table summary=“Information about the expose command parameters.”>
    <caption>Weitere Informationen zu den Parametern von 'expose'</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Weitere Informationen zu den Parametern von 'expose'</th>
    </thead>
    <tbody>
    <tr>
    <td><code>expose</code></td>
    <td>Macht eine Ressource als Kubernetes-Service verfügbar und macht sie den Benutzern öffentlich zugänglich.</td>
    </tr>
    <tr>
    <td><code>deployment/<em>&lt;hello-world-deployment&gt;</em></code></td>
    <td>Der Ressourcentyp und der Name der Ressource, die mit diesem Service verfügbar gemacht werden soll.</td>
    </tr>
    <tr>
    <td><code>--name=<em>&lt;hello-world-service&gt;</em></code></td>
    <td>Der Name des Service.</td>
    </tr>
    <tr>
    <td><code>--port=<em>&lt;8080&gt;</em></code></td>
    <td>Der Port, an dem der Service bereitgestellt wird.</td>
    </tr>
    <tr>
    <td><code>--type=NodePort</code></td>
    <td>Der Typ des Service, der erstellt werden soll.</td>
    </tr>
    <tr>
    <td><code>--target-port=<em>&lt;8080&gt;</em></code></td>
    <td>Der Port, an den der Service den Datenverkehr leitet. In dieser Instanz ist der Zielport mit dem Port identisch, andere von Ihnen erstellte Apps können hiervon jedoch abweichen.</td>
    </tr>
    </tbody></table>

10. Nachdem alle Schritte für die Bereitstellung ausgeführt worden sind, können Sie Ihre App in einem Browser testen. Rufen Sie die Details ab, um die zugehörige URL zu bilden.
    1.  Rufen Sie Informationen zum Service ab, um zu erfahren, welcher Knotenport (NodePort) zugewiesen wurde.

        ```
        kubectl describe service hello-world-service
        ```
        {: pre}

        Beispielausgabe:

        ```
        Name:                   hello-world-service
        Namespace:              default
        Labels:                 run=hello-world-deployment
        Selector:               run=hello-world-deployment
        Type:                   NodePort
        IP:                     10.xxx.xx.xxx
        Port:                   <unset> 8080/TCP
        NodePort:               <unset> 30872/TCP
        Endpoints:              172.30.xxx.xxx:8080
        Session Affinity:       None
        No events.
        ```
        {: screen}

        Knotenportnummern (NodePort) werden nach dem Zufallsprinzip zugewiesen, wenn sie mit dem Befehl `expose` generiert werden, bewegen sich aber im Bereich 30000-32767. Im vorliegenden Beispiel lautet die Portnummer für den Service vom Typ 'NodePort' 30872.

    2.  Rufen Sie die öffentliche IP-Adresse für den Workerknoten im Cluster ab.

        ```
        bx cs workers <clustername_oder_-id>
        ```
        {: pre}

        Beispielausgabe:

        ```
        bx cs workers pr_firm_cluster
        Listing cluster workers...
        OK
        ID                                                 Public IP       Private IP       Machine Type   State    Status   Location   Version
        kube-mil01-pa10c8f571c84d4ac3b52acbf50fd11788-w1   169.xx.xxx.xxx  10.xxx.xx.xxx    free           normal   Ready    mil01      1.9.7
        ```
        {: screen}

11. Öffnen Sie einen Browser und überprüfen Sie die App mit der folgenden URL: `http://<IP_address>:<NodePort>`. Anhand der Werte im Beispiel lautet die URL wie folgt: `http://169.xx.xxx.xxx:30872`. Bei Eingabe dieser URL in einen Browser wird folgender Text angezeigt.

    ```
    Hello world! Your app is up and running in a cluster!
(Hallo Welt! Ihre App steht jetzt in einem Cluster für den Betrieb bereit!)
    ```
    {: screen}

    Um zu überprüfen, ob die App öffentlich verfügbar ist, versuchen Sie, sie im Browser Ihres Mobiltelefons einzugeben.
    {: tip}

12. [Starten Sie das Kubernetes-Dashboard](cs_app.html#cli_dashboard).

    Wenn Sie den Cluster in der [{{site.data.keyword.Bluemix_notm}}-GUI](https://console.bluemix.net/) auswählen, können Sie über die Schaltfläche **Kubernetes-Dashboard** das Dashboard mit einem einzigen Klick starten.
    {: tip}

13. Auf der Registerkarte **Workloads** werden die von Ihnen erstellten Ressourcen angezeigt.

Glückwunsch! Sie haben die erste Version der App bereitgestellt.

Sie sind der Ansicht, dass diese Lerneinheit zu viele Befehle enthält? Ganz Ihrer Meinung. Warum also verwenden Sie nicht ein Konfigurationsscript, das Ihnen einen Teil der Arbeit abnimmt? Um für die zweite Version der App ein Konfigurationsscript zu verwenden und durch die Bereitstellung mehrerer Instanzen dieser App ein höheres Maß an Verfügbarkeit erreichen, fahren Sie mit der nächsten Lerneinheit fort.

<br />


## Lerneinheit 2: Apps mit höherer Verfügbarkeit bereitstellen und aktualisieren
{: #cs_apps_tutorial_lesson2}

In dieser Lerneinheit stellen Sie drei Instanzen der App 'Hello World' in einem Cluster mit höherer Verfügbarkeit als in der ersten Version der App bereit.
{:shortdesc}

Höhere Verfügbarkeit bedeutet, dass der Benutzerzugriff auf drei Instanzen aufgeteilt ist. Versuchen zu viele Benutzer, auf dieselbe Instanz der App zuzugreifen, so können schleppende Reaktionszeiten auftreten. Eine höhere Anzahl von Instanzen kann für Ihre Benutzer gleichbedeutend mit geringeren Reaktionszeiten sein. In der vorliegenden Lerneinheit erfahren Sie außerdem, wie Statusprüfungen und Bereitstellungsaktualisierungen mit Kubernetes funktionieren können. Das folgende Diagramm enthält die von Ihnen im Rahmen der Lerneinheit bereitgestellten Komponenten.


![Konfiguration für die Bereitstellung](images/cs_app_tutorial_components2.png)

Im vorherigen Lernprogramm haben Sie ein eigenes Konto und einen Cluster mit einem Workerknoten erstellt. In dieser Lerneinheit konfigurieren Sie eine Bereitstellung und stellen drei Instanzen der App 'Hello World' bereit. Jede dieser Instanzen wird in einem Kubernetes-Pod als Teil einer Replikatgruppe im Workerknoten implementiert. Um die App öffentlich zu machen, erstellen Sie zudem einen Kubernetes-Service.

Wie im Konfigurationsscript definiert kann Kubernetes anhand einer Verfügbarkeitsprüfung feststellen, ob ein Container in einem Pod aktiv oder inaktiv ist. Mit diesen Prüfungen können gegebenenfalls Deadlocks abgefangen werden, bei denen eine App zwar aktiv, nicht aber in der Lage ist, Verarbeitungsfortschritt zu machen. Durch den Neustart eines Containers, der sich in einem solchen Zustand befindet, ist es möglich, die Verfügbarkeit der App trotz Fehlern zu erhöhen. Anhand einer Bereitschaftsprüfung ermittelt Kubernetes dann, wann ein Container wieder für die Entgegennahme von Datenverkehr ist. Ein Pod gilt als bereit, wenn sein Container bereit ist. Wenn der Pod bereit ist, wird er wieder gestartet. In dieser Version der App wird das Zeitlimit alle 15 Sekunden überschritten. Da im Konfigurationsscript eine Statusprüfung konfiguriert ist, werden Container erneut  erstellt, falls bei der Statusprüfung ein Problem im Zusammenhang mit der App festgestellt wird.

1.  Navigieren Sie in einer CLI (Befehlszeilenschnittstelle) zum Verzeichnis `Lab 2`.

  ```
  cd 'container-service-getting-started-wt/Lab 2'
  ```
  {: pre}

2.  Wenn Sie eine CLI-Sitzung gestartet haben, melden Sie sich an und legen Sie den Clusterkontext fest.

3.  Erstellen und kennzeichnen (taggen) Sie die zweite Version der App lokal als Image. Vergessen Sie auch hier nicht den Punkt (`.`) am Ende des Befehls.

  ```
  docker build -t registry.<region>.bluemix.net/<namensbereich>/hello-world:2 .
  ```
  {: pre}

  Stellen Sie sicher, dass die Nachricht über die erfolgreiche Ausführung angezeigt wird.

  ```
  Successfully built <image-id>
  ```
  {: screen}

4.  Übertragen Sie die zweite Version des Image mit einer Push-Operation an Ihren Registry-Namensbereich. Fahren Sie erst dann mit dem nächsten Schritt fort, wenn das Image per Push-Operation übertragen worden ist.

  ```
  docker push registry.<region>.bluemix.net/<namensbereich>/hello-world:2
  ```
  {: pre}

  Beispielausgabe:

  ```
  The push refers to a repository [registry.ng.bluemix.net/pr_firm/hello-world]
  ea2ded433ac8: Pushed
  894eb973f4d3: Pushed
  788906ca2c7e: Pushed
  381c97ba7dc3: Pushed
  604c78617f34: Pushed
  fa18e5ffd316: Pushed
  0a5e2b2ddeaa: Pushed
  53c779688d06: Pushed
  60a0858edcd5: Pushed
  b6ca02dfe5e6: Pushed
  1: digest: sha256:0d90cb73288113bde441ae9b8901204c212c8980d6283fbc2ae5d7cf652405
  43 size: 2398
  ```
  {: screen}

5.  Öffnen Sie die Datei `healthcheck.yml` im Verzeichnis `Lab 2` mit einem Texteditor. Dieses Konfigurationsscript verbindet einige Schritte aus der vorherigen Lerneinheit, um zur gleichen Zeit eine Bereitstellung sowie einen Service zu erstellen. Die App-Entwickler des PR-Unternehmens können diese Scripts verwenden, wenn Aktualisierungen durchgeführt werden. Sie können sie auch im Rahmen der Fehlerbehebung einsetzen, indem Sie die Pods neu erstellen.
    1. Aktualisieren Sie die Details für das Image in Ihrem privaten Registry-Namensbereich.

        ```
        image: "registry.<region>.bluemix.net/<namensbereich>/hello-world:2"
        ```
        {: codeblock}

    2.  Beachten Sie im Abschnitt **Deployment** für die Bereitstellung die Angabe für die Anzahl der Replikate (`replicas`). Replikate sind gleichbedeutend mit der Anzahl von Instanzen Ihrer App. Durch Ausführen von drei Instanzen ist die Verfügbarkeit der App sehr viel höher als dies bei Ausführung von nur einer Instanz der Fall wäre.

        ```
        replicas: 3
        ```
        {: codeblock}

    3.  Beachten Sie die HTTP-Aktivitätsprüfung (livenessProbe), die den Zustand des Containers alle 5 Sekunden überprüft.

        ```
        livenessProbe:
                    httpGet:
                      path: /healthz
                      port: 8080
                    initialDelaySeconds: 5
                    periodSeconds: 5
        ```
        {: codeblock}

    4.  Beachten Sie im Abschnitt **Service** die Angabe für die Knotenportnummern (`NodePort`). Anstatt einen Knotenport nach dem Zufallsprinzip generieren zu lassen, wie das der Fall in der vorherigen Lerneinheit war, können Sie eine Portnummer im Bereich 30000-32767 angeben. Das vorliegende Beispiel verwendet die Nummer 30072.

6.  Wechseln Sie zurück zu der CLI, die Sie zum Festlegen des Clusterkontexts verwendet haben, und führen Sie das Konfigurationsscript aus. Wenn die Bereitstellung und der Service erstellt worden sind, kann die App gegenüber den Benutzern des PR-Unternehmens verfügbar gemacht werden.

  ```
  kubectl apply -f healthcheck.yml
  ```
  {: pre}

  Beispielausgabe:

  ```
  deployment "hw-demo-deployment" created
  service "hw-demo-service" created
  ```
  {: screen}

7.  Nachdem die Bereitstellung nun abgeschlossen ist, können Sie einen Browser öffnen und die App überprüfen. Um die URL zu bilden, kombinieren Sie die öffentliche IP-Adresse, die Sie in der vorherigen Lerneinheit für Ihren Workerknoten verwendet haben, mit der Knotenportnummer (NodePort), die im Konfigurationsscript angegeben war. So rufen Sie die öffentliche IP-Adresse für den Workerknoten ab:

  ```
  bx cs workers <clustername_oder_-id>
  ```
  {: pre}

  Anhand der Werte im Beispiel lautet die URL wie folgt: `http://169.xx.xxx.xxx:30072`. Bei Eingabe dieser URL in einen Browser wird gegebenenfalls der folgende Text angezeigt. Falls dieser Text nicht angezeigt wird, machen Sie sich keine Gedanken. Diese App wurde für alternierende Intervalle von Aktivität und Inaktivität konzipiert.

  ```
  Hello world! Great job getting the second stage up and running! (Hallo Welt! Sie haben Stage2 hervorragend umgesetzt.)
  ```
  {: screen}

  Sie können den Status auch unter `http://169.xx.xxx.xxx:30072/healthz` überprüfen.

  Während der ersten 10-15 Sekunden wird eine Nachricht vom Typ 200 zurückgegeben. Dadurch wissen Sie, dass die App erfolgreich ausgeführt wird. Nach Verstreichen dieser 15 Sekunden wird eine Zeitlimitnachricht angezeigt. Dies entspricht dem erwarteten Verhalten.

  ```
  {
    "error": "Timeout, Health check error!" ("Fehler": "Zeitlimitüberschreitung, Fehler bei Statusprüfung!")
  }
  ```
  {: screen}

8.  [Starten Sie das Kubernetes-Dashboard](cs_app.html#cli_dashboard).

9. Auf der Registerkarte **Workloads** werden die von Ihnen erstellten Ressourcen angezeigt. Von dieser Registerkarte aus können Sie die Anzeige fortlaufend aktualisieren lassen und sicherstellen, dass die Statusprüfung ordnungsgemäß funktioniert. Im Abschnitt **Pods** wird angegeben, wie häufig die Pods erneut gestartet werden, wenn die in ihnen enthaltenen Container erneut erstellt werden. Falls Sie per Zufall den folgenden Fehler im Dashboard abfangen, so beachten Sie, das diese Nachricht darauf hinweist, dass bei der Statusprüfung ein Problem festgestellt wurde. Warten Sie einige Minuten ab und aktualisieren Sie dann die Anzeige erneut. Sie können erkennen, dass die Anzahl von Neustarts für jeden Pod variiert.

    ```
    Liveness probe failed: HTTP probe failed with statuscode: 500
Back-off restarting failed docker container
Error syncing pod, skipping: failed to "StartContainer" for "hw-container" with CrashLoopBackOff: "Back-off 1m20s restarting failed container=hw-container pod=hw-demo-deployment-3090568676-3s8v1_default(458320e7-059b-11e7-8941-56171be20503)"
(Aktivitätsprüfung fehlgeschlagen: HTTP-Prüfung fehlgeschlagen mit Statuscode: 500
Fehlgeschlagener Container wird mit 'Back-off' neu gestartet
Fehler bei Synchronisierung von Pod, wird übersprungen: "StartContainer" für "hw-container" mit CrashLoopBackOff fehlgeschlagen: "Back-off 1m20s restarting failed container=hw-container pod=hw-demo-deployment-3090568676-3s8v1_default(458320e7-059b-11e7-8941-56171be20503)"
    ```
    {: screen}

Glückwunsch! Sie haben die zweite Version der App bereitgestellt. Hierzu haben Sie eine geringere Anzahl von Befehlen verwenden müssen, konnten erfahren, wie Statusprüfungen funktionieren, und haben eine Bereitstellung bearbeitet, was hervorragend ist. Die App 'Hello World' hat den Test für das PR-Unternehmen bestanden. Nun können Sie eine App mit höherem Nutzwert für das PR-Unternehmen bereitstellen, um mit der Analyse von Pressemitteilungen zu beginnen.

Sind Sie bereit, die von Ihnen erstellten Elemente zu löschen, bevor Sie fortfahren? Dieses Mal können Sie dasselbe Konfigurationsscript verwenden, um die beiden von Ihnen erstellten Ressourcen zu löschen.

  ```
  kubectl delete -f healthcheck.yml
  ```
  {: pre}

  Beispielausgabe:

  ```
  deployment "hw-demo-deployment" deleted
service "hw-demo-service" deleted
  ```
  {: screen}

<br />


## Lerneinheit 3: App 'Watson Tone Analyzer' bereitstellen und aktualisieren
{: #cs_apps_tutorial_lesson3}

In den vorherigen Lerneinheiten wurden die Apps als einzelne Komponenten in einem Workerknoten bereitgestellt. In dieser Lerneinheit stellen Sie zwei Komponenten einer App in einem Cluster bereit, die den Service {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} verwenden.
{:shortdesc}

Durch das Aufteilen der Komponenten auf verschiedene Container stellen Sie sicher, dass Sie ein Element aktualisieren können, ohne dass sich dies nachteilig auf die übrigen auswirkt. Dann aktualisieren Sie die App, um sie mit einer größeren Anzahl von Replikaten vertikal zu skalieren und so ihre Verfügbarkeit weiter zu steigern. Das folgende Diagramm enthält die von Ihnen im Rahmen der Lerneinheit bereitgestellten Komponenten.

![Konfiguration für die Bereitstellung](images/cs_app_tutorial_components3.png)


Aus dem vorherigen Lernprogramm verfügen Sie bereits über ein Konto und einen Cluster mit einem Workerknoten. In dieser Lerneinheit erstellen Sie eine Instanz des {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}}-Service in Ihrem {{site.data.keyword.Bluemix_notm}}-Konto und konfigurieren zwei Bereitstellungen (eine Bereitstellung für jede Komponente der App). Jede Komponente wird in einem Kubernetes-Pod im Workerknoten implementiert. Um beide Komponenten öffentlich zu machen, erstellen Sie zudem für jede Komponente einen Kubernetes-Service.


### Lerneinheit 3a: App '{{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}}' bereitstellen
{: #lesson3a}

1.  Navigieren Sie in einer CLI (Befehlszeilenschnittstelle) zum Verzeichnis `Lab 3`.

  ```
  cd 'container-service-getting-started-wt/Lab 3'
  ```
  {: pre}

2.  Wenn Sie eine CLI-Sitzung gestartet haben, melden Sie sich an und legen Sie den Clusterkontext fest.

3.  Erstellen Sie das erste {{site.data.keyword.watson}}-Image.

    1.  Navigieren Sie zum Verzeichnis `watson`.

        ```
        cd watson
        ```
        {: pre}

    2.  Erstellen und kennzeichnen (taggen) Sie den ersten Teil der App lokal als Image. Vergessen Sie auch hier nicht den Punkt (`.`) am Ende des Befehls. Wenn Sie das Docker Quickstart-Terminal verwenden, um Docker-Befehle auszuführen, wechseln Sie unbedingt die CLI.

        ```
        docker build -t registry.<region>.bluemix.net/<namensbereich>/watson .
        ```
        {: pre}

        Stellen Sie sicher, dass die Nachricht über die erfolgreiche Ausführung angezeigt wird.

        ```
        Successfully built <image-id>
        ```
        {: screen}

    3.  Übertragen Sie den ersten Teil der App mit einer Push-Operation als Image an Ihren private Registry-Namensbereich. Fahren Sie erst dann mit dem nächsten Schritt fort, wenn das Image per Push-Operation übertragen worden ist.

        ```
        docker push registry.<region>.bluemix.net/<namensbereich>/watson
        ```
        {: pre}

4.  Erstellen Sie das '{{site.data.keyword.watson}}-talk'-Image.

    1.  Navigieren Sie zum Verzeichnis `watson-talk`.

        ```
        cd 'container-service-getting-started-wt/Lab 3/watson-talk'
        ```
        {: pre}

    2.  Erstellen und kennzeichnen (taggen) Sie den zweiten Teil der App lokal als Image. Vergessen Sie auch hier nicht den Punkt (`.`) am Ende des Befehls.

        ```
        docker build -t registry.<region>.bluemix.net/<namensbereich>/watson-talk .
        ```
        {: pre}

        Stellen Sie sicher, dass die Nachricht über die erfolgreiche Ausführung angezeigt wird.

        ```
        Successfully built <image-id>
        ```
        {: screen}

    3.  Übertragen Sie den zweiten Teil der App mit einer Push-Operation an Ihren privaten Registry-Namensbereich. Fahren Sie erst dann mit dem nächsten Schritt fort, wenn das Image per Push-Operation übertragen worden ist.

        ```
        docker push registry.<region>.bluemix.net/<namensbereich>/watson-talk
        ```
        {: pre}

5.  Überprüfen Sie, ob die Images erfolgreich zu Ihrem Registry-Namensbereich hinzugefügt wurden. Wenn Sie das Docker Quickstart-Terminal verwendet haben, um Docker-Befehle auszuführen, wechseln Sie unbedingt zurück zu der CLI (Befehlszeilenschnittstelle), die Sie zum Festlegen der Sitzungsvariablen `KUBECONFIG` verwendet haben.

    ```
    bx cr images
    ```
    {: pre}

    Beispielausgabe:

    ```
    Listing images...

    REPOSITORY                                      NAMESPACE  TAG      DIGEST         CREATED         SIZE     VULNERABILITY STATUS
    registry.ng.bluemix.net/namespace/hello-world   namespace  1        0d90cb732881   40 minutes ago  264 MB   OK
    registry.ng.bluemix.net/namespace/hello-world   namespace  2        c3b506bdf33e   20 minutes ago  264 MB   OK
    registry.ng.bluemix.net/namespace/watson        namespace  latest   fedbe587e174   3 minutes ago   274 MB   OK
    registry.ng.bluemix.net/namespace/watson-talk   namespace  latest   fedbe587e174   2 minutes ago   274 MB   OK
    ```
    {: screen}

6.  Öffnen Sie die Datei `watson-deployment.yml` im Verzeichnis `Lab 3` mit einem Texteditor. Dieses Konfigurationsscript enthält sowohl für die `watson`- als auch die `watson-talk`-Komponente der App eine Bereitstellung und einen Service.

    1.  Aktualisieren Sie für beide Bereitstellungen die Details für das Image in Ihrem Registry-Namensbereich.

        watson:

        ```
        image: "registry.<region>.bluemix.net/namespace/watson"
        ```
        {: codeblock}

        watson-talk:

        ```
        image: "registry.<region>.bluemix.net/namespace/watson-talk"
        ```
        {: codeblock}

    2.  Aktualisieren Sie im Datenträgerabschnitt der Watson-Bereitstellung den Namen des geheimen Schlüssels
für {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}}, den Sie im vorherigen Lernprogramm erstellt haben. Wenn Sie den geheimen Kubernetes-Schlüssel als Datenträger an Ihre Bereitstellung anhängen, stellen Sie die
{{site.data.keyword.Bluemix_notm}}-Serviceberechtigungsnachweise
dem Container zur Verfügung, der in Ihrem Pod ausgeführt wird. Die {{site.data.keyword.watson}}-App-Komponenten in diesem Lernprogramm sind so konfiguriert, dass sie die
Serviceberechtigungsnachweise unter Verwendung des Datenträgermountpfads suchen.

        ```
        volumes:
                - name: service-bind-volume
                  secret:
                    defaultMode: 420
                    secretName: binding-mytoneanalyzer
        ```
        {: codeblock}

        Falls Sie vergessen haben, welchen Namen Sie dem geheimen Schlüssel zugeordnet haben, führen Sie den folgenden Befehl aus.

        ```
        kubectl get secrets --namespace=default
        ```
        {: pre}

    3.  Beachten Sie im Abschnitt für den Service 'watson-talk' die Knotenportnummer, die für `NodePort` festgelegt ist. Das vorliegende Beispiel verwendet die Nummer 30080.

7.  Führen Sie das Konfigurationsscript aus.

  ```
  kubectl apply -f watson-deployment.yml
  ```
  {: pre}

8.  Optional: Überprüfen Sie, ob der geheime Schlüssel für {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} als Datenträger an den Pod angehängt wurde.

    1.  Wenn Sie den Namen eines Watson-Pods abrufen wollen, führen Sie den folgenden Befehl aus:

        ```
        kubectl get pods
        ```
        {: pre}

        Beispielausgabe:

        ```
        NAME                                 READY     STATUS    RESTARTS  AGE
        watson-pod-4255222204-rdl2f          1/1       Running   0         13h
        watson-talk-pod-956939399-zlx5t      1/1       Running   0         13h
        ```
        {: screen}

    2.  Rufen Sie die Details zum Pod ab und suchen Sie den Namen des geheimen Schlüssels.

        ```
        kubectl describe pod <podname>
        ```
        {: pre}

        Beispielausgabe:

        ```
        Volumes:
          service-bind-volume:
            Type:       Secret (Datenträger, der mit einem geheimen Schlüssel belegt ist)
            SecretName: binding-mytoneanalyzer
          default-token-j9mgd:
            Type:       Secret (Datenträger, der mit einem geheimen Schlüssel belegt ist)
            SecretName: default-token-j9mgd
        ```
        {: codeblock}

9.  Öffnen Sie einen Browser und analysieren Sie Text. Die URL weist das folgende Format auf: `http://<worker_node_IP_address>:<watson-talk-nodeport>/analyze/"<text_to_analyze>"`.

    Beispiel:

    ```
    http://169.xx.xxx.xxx:30080/analyze/"Heute ist ein herrlicher Tag"
    ```
    {: screen}

    In einem Browser wird die JSON-Antwort auf den von Ihnen eingegebenen Text angezeigt.

10. [Starten Sie das Kubernetes-Dashboard](cs_app.html#cli_dashboard).

11. Auf der Registerkarte **Workloads** werden die von Ihnen erstellten Ressourcen angezeigt.

### Lerneinheit 3b. Aktive Bereitstellung von 'Watson Tone Analyzer' aktualisieren
{: #lesson3b}

Während der Ausführung einer Bereitstellung können Sie die Bereitstellung durch Änderung von Werten in der Pod-Vorlage bearbeiten. In dieser Lerneinheit aktualisieren Sie das verwendete Image. Die PR-Firma möchte die App in der Bereitstellung ändern.

Ändern Sie den Namen des Image:

1.  Öffnen Sie die Konfigurationsdetails für die aktive Bereitstellung.

    ```
    kubectl edit deployment/watson-talk-pod
    ```
    {: pre}

    Abhängig vom verwendeten Betriebssystem wird ein Editor vom Typ 'vi' oder ein Texteditor geöffnet.

2.  Ändern Sie den Namen des Image in das 'ibmliberty'-Image.

    ```
    spec:
          containers:
          - image: registry.<region>.bluemix.net/ibmliberty:latest
    ```
    {: codeblock}

3.  Speichern Sie die Änderungen und beenden Sie den Editor.

4.  Wenden Sie die Änderungen auf die aktive Bereitstellung an.

    ```
    kubectl rollout status deployment/watson-talk-pod
    ```
    {: pre}

    Warten Sie die Bestätigung ab, dass der Rollout vollständig abgeschlossen wurde.

    ```
    deployment "watson-talk-pod" successfully rolled out
    ```
    {: screen}

    Wenn Sie eine Änderung implementieren, wird ein weiterer Pod von Kubernetes erstellt getestet. Wenn der Test erfolgreich ist, wird der alte Pod entfernt.

[Testen Sie Ihr Wissen in einem Quiz! ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://ibmcloud-quizzes.mybluemix.net/containers/apps_tutorial/quiz.php)

Glückwunsch! Sie haben die App '{{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}}' bereitgestellt. Das PR-Unternehmen kann diese Bereitstellung künftig einsetzen, um mit der Analyse seiner Pressemitteilungen zu beginnen.

Sind Sie bereit, die von Ihnen erstellten Elemente zu löschen? Zum Löschen der von Ihnen erstellten Ressourcen können Sie das Konfigurationsscript verwenden.

  ```
  kubectl delete -f watson-deployment.yml
  ```
  {: pre}

  Beispielausgabe:

  ```
  deployment "watson-pod" deleted
deployment "watson-talk-pod" deleted
service "watson-service" deleted
service "watson-talk-service" deleted
  ```
  {: screen}

  Falls Sie den Cluster nicht beibehalten wollen, können Sie diesen ebenfalls löschen.

  ```
  bx cs cluster-rm <clustername_oder_-id>
  ```
  {: pre}

## Womit möchten Sie fortfahren?
{: #next}

Nachdem Sie sich mit den grundlegenden Informationen vertraut gemacht haben, können Sie nun komplexere Aufgaben in Angriff nehmen. Ziehen Sie eine der folgenden Aufgaben in Betracht:

- Ein [kompliziertes Lab ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/IBM/container-service-getting-started-wt#lab-overview) im Repository vervollständigen
- [Apps automatisch skalieren](cs_app.html#app_scaling) mit {{site.data.keyword.containershort_notm}}
- Lernprogramme für die Containerorchestrierung unter [developerWorks ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/code/journey/category/container-orchestration/) erkunden
