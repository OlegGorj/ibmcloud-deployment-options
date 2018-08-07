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





# {{site.data.keyword.registryshort_notm}}-Token für eine {{site.data.keyword.Bluemix_dedicated_notm}}-Image-Registry erstellen
{: #cs_dedicated_tokens}

Erstellen Sie ein Token ohne Ablaufdatum für eine Image-Registry, die Sie für einzelne und skalierbare Gruppen mit Clustern in {{site.data.keyword.containerlong}} verwendet haben.
{:shortdesc}

1.  Melden Sie sich bei der {{site.data.keyword.Bluemix_dedicated_notm}}-Umgebung an.

    ```
    bx login -a api.<dedizierte_domäne>
    ```
    {: pre}

2.  Fordern Sie ein `oauth-token` für die aktuelle Sitzung an und speichern Sie es als Variable.

    ```
    OAUTH_TOKEN=`bx iam oauth-tokens | awk 'FNR == 2 {print $3 " " $4}'`
    ```
    {: pre}

3.  Fordern Sie die ID der Organisation für die aktuelle Sitzung an und speichern Sie sie als Variable.

    ```
    ORG_GUID=`bx iam org <org_name> --guid`
    ```
    {: pre}

4.  Fordern Sie ein permanentes Registry-Token für die aktuelle Sitzung an. Ersetzen Sie <dedizierte_domäne> durch die Domäne für Ihre {{site.data.keyword.Bluemix_dedicated_notm}}-Umgebung. Dieses Token gewährt Zugriff auf die Images im aktuellen Namensbereich.

    ```
    curl -XPOST -H "Authorization: ${OAUTH_TOKEN}" -H "Organization: ${ORG_GUID}" https://registry.<dedizierte_domäne>/api/v1/tokens?permanent=true
    ```
    {: pre}

    Ausgabe:

    ```
    {
        "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI2MzdiM2Q4Yy1hMDg3LTVhZjktYTYzNi0xNmU3ZWZjNzA5NjciLCJpc3MiOiJyZWdpc3RyeS5jZnNkZWRpY2F0ZWQxLnVzLXNvdXRoLmJsdWVtaXgubmV0"
    }
    ```
    {: screen}

5.  Überprüfen Sie den geheimen Kubernetes-Schlüssel.

    ```
    kubectl describe secrets
    ```
    {: pre}

    Sie können diesen geheimen Schlüssel verwenden, um mit {{site.data.keyword.containerlong}} zu arbeiten.

6.  Erstellen Sie den geheimen Kubernetes-Schlüssel, um Ihre Tokeninformationen zu speichern.

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
    <td><code>--namespace &lt;kubernetes-namensbereich&gt;</code></td>
    <td>Erforderlich. Der Kubernetes-Namensbereich Ihres Clusters, in dem Sie den geheimen Schlüssel verwenden und Container bereitstellen möchten. Führen Sie <code>kubectl get namespaces</code>, um alle Namensbereiche in Ihrem Cluster aufzulisten.</td>
    </tr>
    <tr>
    <td><code>&lt;name_des_geheimen_schlüssels&gt;</code></td>
    <td>Erforderlich. Der Name, den Sie für Ihr imagePullSecret verwenden möchten.</td>
    </tr>
    <tr>
    <td><code>--docker-server &lt;registry-url&gt;</code></td>
    <td>Erforderlich. Die URL der Image-Registry, in der Ihr Namensbereich eingerichtet ist: Registry.&lt;dedizierte_domäne&gt;</li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username &lt;docker-benutzername&gt;</code></td>
    <td>Erforderlich. Der Benutzername für die Anmeldung bei Ihrer privaten Registry.</td>
    </tr>
    <tr>
    <td><code>--docker-password &lt;tokenwert&gt;</code></td>
    <td>Erforderlich. Der Wert des zuvor abgerufenen Registry-Tokens.</td>
    </tr>
    <tr>
    <td><code>--docker-email &lt;docker-e-mail&gt;</code></td>
    <td>Erforderlich. Falls Sie über eine Docker-E-Mail-Adresse verfügen, geben Sie diese ein. Ist dies nicht der Fall, geben Sie eine fiktive E-Mail-Adresse ein, z. B. a@b.c. Die Angabe der E-Mail-Adresse ist obligatorisch, um einen geheimen Kubernetes-Schlüssel zu erstellen, wird aber nach der Erstellung nicht weiter genutzt.</td>
    </tr>
    </tbody></table>

7.  Erstellen Sie einen Pod, der das imagePullSecret referenziert.

    1.  Öffnen Sie Ihren bevorzugten Texteditor und erstellen Sie ein Podkonfigurationsscript namens 'mypod.yaml'.
    2.  Definieren Sie den Pod und das imagePullSecret, das Sie für den Zugriff auf die Registry verwenden möchten. Gehen Sie wie folgt vor, um ein privates Image aus einem Namensbereich zu verwenden:

        ```
        apiVersion: v1
        kind: Pod
        metadata:
          name: <podname>
        spec:
          containers:
            - name: <containername>
              image: registry.<dedizierte_domäne>/<mein_namensbereich>/<mein_image>:<tag>
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
        <td><code>&lt;podname&gt;</code></td>
        <td>Der Name des Pod, den Sie erstellen möchten.</td>
        </tr>
        <tr>
        <td><code>&lt;containername&gt;</code></td>
        <td>Der Name des Containers, den Sie in Ihrem Cluster bereitstellen möchten.</td>
        </tr>
        <tr>
        <td><code>&lt;mein_namensbereich&gt;</code></td>
        <td>Der Namensbereich, in dem das Image gespeichert ist. Führen Sie den Befehl `bx cr namespace-list` aus, um die verfügbaren Namensbereiche aufzulisten.</td>
        </tr>
        <td><code>&lt;mein_image&gt;</code></td>
        <td>Der Name des Images, das Sie verwenden möchten. Führen Sie den Befehl <code>bx cr image-list</code> aus, um die verfügbaren Images in einem {{site.data.keyword.Bluemix_notm}} aufzulisten.</td>
        </tr>
        <tr>
        <td><code>&lt;tag&gt;</code></td>
        <td>Die Version des Images, das Sie verwenden möchten. Ist kein Tag angegeben, wird standardmäßig das Image mit dem Tag <strong>latest</strong> verwendet.</td>
        </tr>
        <tr>
        <td><code>&lt;name_des_geheimen_schlüssels&gt;</code></td>
        <td>Der Name des imagePullSecret, das Sie zuvor erstellt haben.</td>
        </tr>
        </tbody></table>

    3.  Speichern Sie Ihre Änderungen.

    4.  Erstellen Sie die Bereitstellung in Ihrem Cluster.

          ```
          kubectl apply -f mypod.yaml
          ```
          {: pre}

