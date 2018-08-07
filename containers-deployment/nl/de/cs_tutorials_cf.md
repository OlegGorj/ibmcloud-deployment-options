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



# Lernprogramm: App aus Cloud Foundry in einen Cluster migrieren
{: #cf_tutorial}

Sie können eine App nehmen, die Sie zuvor mithilfe von Cloud Foundry bereitgestellt haben, und denselben Code in einen Container auf einem Kubernetes-Cluster in {{site.data.keyword.containershort_notm}} bereitstellen.
{: shortdesc}


## Ziele

- Den allgemeinen Prozess der Bereitstellung von Apps in Containern auf einem Kubernetes-Cluster kennenlernen.
- Eine Dockerfile aus Ihrem App-Code generieren, um ein Container-Image zu erstellen.
- Einen Container aus diesem Image in einem Kubernetes-Cluster bereitstellen.

## Erforderlicher Zeitaufwand
30 Minuten

## Zielgruppe
Dieses Lernprogramm richtet sich an Entwickler von Cloud Foundry-Apps.

## Voraussetzungen

- [Erstellen Sie eine private Image-Registry in {{site.data.keyword.registrylong_notm}}](../services/Registry/index.html).
- [Erstellen Sie einen Cluster](cs_clusters.html#clusters_ui).
- [Geben Sie als Ziel Ihrer CLI den Cluster an](cs_cli_install.html#cs_cli_configure).
- [Lernen Sie die Docker- und Kubernetes-Terminologie kennen](cs_tech.html).


<br />



## Lerneinheit 1: App-Code herunterladen

Bereiten Sie Ihren Code vor. Sie haben noch keinen Code? Sie können den in diesem Lernprogramm verwendeten Starter-Code herunterladen.
{: shortdesc}

1. Erstellen Sie ein Verzeichnis namens `cf-py` und navigieren Sie zu diesem Verzeichnis. In diesem Verzeichnis speichern Sie alle Dateien, die zum Erstellen des Docker-Images und zum Ausführen Ihrer App erforderlich sind.

  ```
  mkdir cf-py && cd cf-py
  ```
  {: pre}

2. Kopieren Sie den App-Code und alle zugehörigen Dateien in das Verzeichnis. Sie können Ihren eigenen App-Code verwenden oder die Boilerplate aus dem Katalog herunterladen. In diesem Lernprogramm wird die Python Flask-Boilerplate verwendet. Sie können dieselben grundlegenden Schritte aber auch in einer Node.js-, Java- oder [Kitura](https://github.com/IBM-Cloud/Kitura-Starter)-App ausführen.

    Gehen Sie wie folgt vor, um den Python Flask-App-Code herunterzuladen:

    a. Klicken Sie im Katalog unter **Boilerplates** auf **Python Flask**. Diese Boilerplate enthält eine Laufzeitumgebung für Python 2- und Python 3-Anwendungen.

    b. Geben Sie den App-Namen `cf-py-<name>` ein und klicken Sie auf **ERSTELLEN**. Um auf den App-Code für die Boilerplate zuzugreifen, müssen Sie die CF-App zunächst in der Cloud bereitstellen. Sie können einen beliebigen Namen für die App verwenden. Wenn Sie den Namen aus dem Beispiel verwenden, ersetzen Sie `.<name>` durch eine eindeutige ID wie `cf-py-msx`.
    
    **Achtung**: Verwenden Sie keine persönlichen Informationen in Namen von Apps, Container-Images oder Kubernetes-Ressourcen.

    Während die App bereitgestellt wird, werden Anweisungen für das Herunterladen, Ändern und erneute Bereitstellen Ihrer App mithilfe der Befehlszeilenschnittstelle angezeigt.

    c. Klicken Sie in Schritt 1 in den GUI-Anweisungen auf **STARTER-CODE HERUNTERLADEN**.

    d. Extrahieren Sie die ZIP-Datei und speichern Sie ihren Inhalt in Ihrem Verzeichnis `cf-py`.

Ihr App-Code kann jetzt containerisiert werden.


<br />



## Lerneinheit 2: Docker-Image mit Ihrem App-Code erstellen

Erstellen Sie eine Dockerfile, die Ihren App-Code und die notwendigen Konfigurationen für Ihren Container enthält. Erstellen Sie anschließend ein Docker-Image aus dieser Dockerfile, und übertragen Sie es per Push-Operation in Ihre private Image-Registry.
{: shortdesc}

1. Erstellen Sie im Verzeichnis `cf-py`, das sie in der vorherigen Lerneinheit erstellt haben, eine `Dockerfile`, die als Basis für das Erstellen eines Container-Image dienen wird. Sie können die Dockerfile mithilfe Ihres bevorzugten CLI-Editors oder eines Texteditors auf Ihrem Computer erstellen. Im folgenden Beispiel wird veranschaulicht, wie eine Dockerfile mit dem Nano-Editor erstellt wird.

  ```
  nano Dockerfile
  ```
  {: pre}

2. Kopieren Sie das folgende Script in die Dockerfile. Diese Dockerfile ist speziell für eine Python-App bestimmt. Wenn Sie einen anderen Typ von Code verwenden, muss Ihre Dockerfile ein anderes Basisimage enthalten und es müssen unter Umständen andere Felder definiert werden.

  ```
  #Python-Image aus DockerHub als Basisimage verwenden
  FROM python:3-slim

  #Port für Ihre Python-App zugänglich machen
  EXPOSE 5000

  #Alle App-Dateien aus dem aktuellen Verzeichnis in das App-
  #Verzeichnis in Ihrem Container kopieren. App-Verzeichnis
  #als Arbeitsverzeichnis festlegen.
  WORKDIR /cf-py/
  COPY .  .

  #Alle definierten Voraussetzungen installieren.
  RUN pip install --no-cache-dir -r requirements.txt

  #OpenSSL-Paket aktualisieren.
  RUN apt-get update && apt-get install -y \
  openssl

  #App starten.
  CMD ["python", "welcome.py"]
  ```
  {: codeblock}

3. Speichern Sie Ihre Änderungen im Nano-Editor, indem Sie `Strg + o` drücken. Bestätigen Sie Ihre Änderungen, indem Sie die `Eingabetaste` drücken. Beenden Sie den Nano-Editor, indem Sie `Strg + x` drücken.

4. Erstellen Sie ein Docker-Image, das Ihren App-Code enthält und übertragen Sie es per Push-Operation in Ihre private Registry.

  ```
  bx cr build -t registry.<region>.bluemix.net/namespace/cf-py .
  ```
  {: pre}

  <table>
  <caption>Erklärung der Bestandteile dieses Befehls</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Dieses Symbol weist daraufhin, dass weitere Informationen zu diesen Befehlskomponenten verfügbar sind."/> Erklärung der Komponenten dieses Befehls</th>
  </thead>
  <tbody>
  <tr>
  <td>Parameter</td>
  <td>Beschreibung</td>
  </tr>
  <tr>
  <td><code>build</code></td>
  <td>Der Befehl 'build'.</td>
  </tr>
  <tr>
  <td><code>-t registry.&lt;region&gt;.bluemix.net/namespace/cf-py</code></td>
  <td>Der Pfad zu Ihrer privaten Registry, mit Ihrem eindeutigen Namensbereich und dem Namen des Image. In diesem Beispiel wird derselbe Name für das Image verwendet wie für das App-Verzeichnis, aber Sie können einen beliebigen Namen für das Image in Ihrer privaten Registry wählen. Wenn Sie nicht sicher sind, wie Ihr Namensbereich lautet, führen Sie den Befehl `bx cr namespaces` aus, um ihn abzurufen.</td>
  </tr>
  <tr>
  <td><code>.</code></td>
  <td>Der Standort der Dockerfile. Wenn Sie den Befehl 'build' aus dem Verzeichnis mit der Dockerfile ausführen, geben Sie einen Punkt (.) ein. Verwenden Sie andernfalls den relativen Pfad zu der Dockerfile.</td>
  </tr>
  </tbody>
  </table>

  Das Image wird in Ihrer privaten Registry erstellt. Sie können den Befehl `bx cr images` ausführen, um zu überprüfen, dass das Image erstellt wurde.

  ```
  REPOSITORY                                     NAMESPACE   TAG      DIGEST         CREATED         SIZE     VULNERABILITY STATUS   
  registry.ng.bluemix.net/namespace/cf-py        namespace   latest   cb03170b2cb2   3 minutes ago   271 MB   OK
  ```
  {: screen}


<br />



## Lerneinheit 3: Container aus Ihrem Image bereitstellen

Stellen Sie Ihre App als Container in einem Kubernetes-Cluster bereit.
{: shortdesc}

1. Erstellen Sie eine YAML-Konfigurationsdatei namens `cf-py.yaml` und aktualisieren Sie `<registry_namespace>` mit dem Namen Ihrer privaten Image-Registry. Diese Konfigurationsdatei definiert eine Containerbereitstellung aus dem Image, das Sie in der vorherigen Lerneinheit erstellt haben, und einen Service, um die App der Öffentlichkeit zugänglich zu machen.

  ```
  apiVersion: extensions/v1beta1
  kind: Deployment
  metadata:
    labels:
      app: cf-py
    name: cf-py
    namespace: default
  spec:
    selector:
      matchLabels:
        app: cf-py
    replicas: 1
    template:
      metadata:
        labels:
          app: cf-py
      spec:
        containers:
        - image: registry.ng.bluemix.net/<registry-namensbereich>/cf-py:latest
          name: cf-py
  ---
  apiVersion: v1
  kind: Service
  metadata:
    name: cf-py-nodeport
    labels:
      app: cf-py
  spec:
    selector:
      app: cf-py
    type: NodePort
    ports:
     - port: 5000
       nodePort: 30872
  ```
  {: codeblock}

  <table>
  <caption>Erklärung der Komponenten der YAML-Datei</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der YAML-Dateikomponenten</th>
  </thead>
  <tbody>
  <tr>
  <td><code>image</code></td>
  <td>Ersetzen Sie in `registry.ng.bluemix.net/<registry_namespace>/cf-py:latest` den Eintrag &lt;registry-namensbereich&gt; durch den Namensbereich Ihrer privaten Image-Registry. Wenn Sie nicht sicher sind, wie Ihr Namensbereich lautet, führen Sie den Befehl `bx cr namespaces` aus, um ihn abzurufen.</td>
  </tr>
  <tr>
  <td><code>nodePort</code></td>
  <td>Machen Sie Ihre App verfügbar, indem Sie einen Kubernetes-Service vom Typ 'NodePort' erstellen. Knotenports (NodePorts) liegen im Bereich zwischen 30000 und 32767. Sie verwenden diesen Port später, um Ihre App in einem Browser zu testen.</td>
  </tr>
  </tbody></table>

2. Wenden Sie die Konfigurationsdatei an, um die Bereitstellung und den Service in Ihrem Cluster zu erstellen.

  ```
  kubectl apply -f <dateipfad>/cf-py.yaml
  ```
  {: pre}

  Ausgabe:

  ```
  deployment "cf-py" configured
  service "cf-py-nodeport" configured
  ```
  {: screen}

3. Nachdem alle Schritte für die Bereitstellung ausgeführt worden sind, können Sie Ihre App in einem Browser testen. Rufen Sie die Details ab, um die zugehörige URL zu bilden.

    a.  Rufen Sie die öffentliche IP-Adresse für den Workerknoten im Cluster ab.

    ```
    bx cs workers <clustername>
    ```
    {: pre}

    Ausgabe:

    ```
    ID                                                 Public IP        Private IP     Machine Type        State    Status   Zone    Version   
    kube-dal10-cr18e61e63c6e94b658596ca93d087eed9-w1   169.xx.xxx.xxx   10.xxx.xx.xxx   u2c.2x4.encrypted   normal   Ready    dal10   1.9.7
    ```
    {: screen}

    b. Öffnen Sie einen Browser und überprüfen Sie die App mit der folgenden URL: `http://<public_IP_address>:<NodePort>`. Anhand der Werte im Beispiel lautet die URL wie folgt: `http://169.xx.xxx.xxx:30872`. Geben Sie diese URL an eine Kollegin oder einen Kollegen weiter und fordern Sie diese bzw. diesen auf, die URL selbst auszuprobieren, oder geben Sie sie auf Ihrem Mobiltelefon in den Browser ein, um sich davon zu überzeugen, dass die App auch tatsächlich öffentlich verfügbar ist.

    <img src="images/python_flask.png" alt="Screenshot der bereitgestellten Python Flask-Boilerplate-App." />

5.  [Starten Sie das Kubernetes-Dashboard](cs_app.html#cli_dashboard).

    Wenn Sie den Cluster in der [{{site.data.keyword.Bluemix_notm}}-GUI](https://console.bluemix.net/) auswählen, können Sie über die Schaltfläche **Kubernetes-Dashboard** das Dashboard mit einem einzigen Klick starten.
    {: tip}

6. Auf der Registerkarte **Workloads** werden die von Ihnen erstellten Ressourcen angezeigt.

Glückwunsch! Ihre App ist jetzt in einem Container bereitgestellt.
