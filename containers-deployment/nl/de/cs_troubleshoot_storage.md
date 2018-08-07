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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# Fehlerbehebung für Clusterspeicher
{: #cs_troubleshoot_storage}

Ziehen Sie bei der Verwendung von {{site.data.keyword.containerlong}} die folgenden Verfahren für die Fehlerbehebung für Clusterspeicher in Betracht.
{: shortdesc}

Wenn Sie ein allgemeineres Problem haben, testen Sie das [Cluster-Debugging](cs_troubleshoot.html).
{: tip}



## Dateisysteme für Workerknoten werden schreibgeschützt
{: #readonly_nodes}

{: tsSymptoms}
{: #stuck_creating_state}
Sie bemerken möglicherweise eines der folgenden Symptome:
- Wenn Sie den Befehl `kubectl get pods -o wide` ausführen, dann können Sie erkennen, dass mehrere Pods, die auf demselben Workerknoten ausgeführt werden, im Zustand `ContainerCreating` blockiert sind.
- Wenn Sie den Befehl `kubectl describe` ausführen, sehen Sie den folgenden Fehler im Abschnitt **Ereignisse** `MountVolume.SetUp failed for volume ... read-only file system`.

{: tsCauses}
Das Dateisystem auf dem Workerknoten ist schreibgeschützt.

{: tsResolve}
1.  Erstellen Sie eine Sicherungskopie aller Daten, die möglicherweise auf dem Workerknoten oder in Ihren Containern gespeichert werden.
2.  Laden Sie den Workerknoten erneut, um eine kurzfristige Programmkorrektur für den vorhandenen Workerknoten zu erreichen.
    <pre class="pre"><code>bx cs worker-reload &lt;clustername&gt; &lt;worker-id&gt;</code></pre>

Für eine langfristige Programmkorrektur müssen Sie [den Maschinentyp aktualisieren, indem Sie einen anderen Workerknoten hinzufügen](cs_cluster_update.html#machine_type).

<br />



## App schlägt fehl, wenn ein Benutzer ohne Rootberechtigung Eigner des NFS-Dateispeicher-Mountpfads ist
{: #nonroot}

{: tsSymptoms}
Nach dem [Hinzufügen von NFS-Speicher](cs_storage.html#app_volume_mount) zu Ihrer Bereitstellung schlägt die Bereitstellung Ihres Containers fehl. Wenn Sie die Protokolle für Ihren Container abrufen, werden möglicherweise Fehler wie "write-permission" oder "do not have required permission" angezeigt. Der Pod schlägt fehl und bleibt in einer Neuladeschleife stecken.

{: tsCauses}
Standardmäßig haben Benutzer ohne Rootberechtigung keinen Schreibzugriff auf den Datenträgermountpfad für NFS-gesicherte Speicher. Einige allgemeine App-Images, wie z. B. Jenkins und Nexus3, geben einen Benutzer ohne Rootberechtigung an, der Eigner des Mountpfads in der Dockerfile ist. Wenn Sie einen Container aus dieser Dockerfile erstellen, schlägt die Erstellung des Containers aufgrund unzureichender Berechtigungen für den Benutzer ohne Rootberechtigung auf dem Mountpfad fehl. Um Schreibberechtigung zu erteilen, können Sie die Dockerfile so ändern, dass der Benutzer ohne Rootberechtigung temporär zur Stammbenutzergruppe hinzugefügt wird, bevor die Mountpfadberechtigungen geändert werden, oder Sie verwenden einen Init-Container.

Wenn Sie ein Helm-Diagramm verwenden, um das Image bereitzustellen, bearbeiten Sie die Helm-Bereitstellung, um einen Init-Container zu verwenden.
{:tip}



{: tsResolve}
Wenn Sie einen [Init-Container![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) in Ihre Bereitstellung einschließen, können Sie einem Benutzer ohne Rootberechtigung, der in Ihrer Dockerfile angegeben ist, Schreibberechtigungen für den Datenträgermountpfad innerhalb des Containers erteilen. Der Init-Container startet, bevor Ihr App-Container startet. Der Init-Container erstellt den Datenträgermountpfad innerhalb des Containers, ändert den Mountpfad, sodass der richtige Benutzer (ohne Rootberechtigung) Eigner ist, und schließt den Pfad wieder. Anschließend wird Ihr App-Container mit dem Benutzer ohne Rootberechtigung gestartet, der in den Mountpfad schreiben muss. Da der Benutzer ohne Rootberechtigung bereits Eigner des Pfads ist, ist das Schreiben in den Mountpfad erfolgreich. Wenn Sie keinen Init-Container verwenden möchten, können Sie die Dockerfile ändern, um Benutzern ohne Rootberechtigung Zugriff auf den NFS-Dateispeicher hinzuzufügen.


Führen Sie zunächst den folgenden Schritt aus: [Richten Sie Ihre CLI](cs_cli_install.html#cs_cli_configure) auf Ihren Cluster aus.

1.  Öffnen Sie die Dockerfile für Ihre App und rufen Sie die Benutzer-ID (UID) und die Benutzer-ID (GID) von dem Benutzer ab, dem Sie Schreibzugriff auf den Datenträgermountpfad erteilen möchten. Im Beispiel einer Jenkins-Dockerfile lautet die Information:
    - UID: `1000`
    - GID: `1000`

    **Beispiel**:

    ```
    FROM openjdk:8-jdk

    RUN apt-get update &&apt-get install -y git curl &&rm -rf /var/lib/apt/lists/*

    ARG user=jenkins
    ARG group=jenkins
    ARG uid=1000
    ARG gid=1000
    ARG http_port=8080
    ARG agent_port=50000

    ENV JENKINS_HOME /var/jenkins_home
    ENV JENKINS_SLAVE_AGENT_PORT ${agent_port}
    ...
    ```
    {:screen}

2.  Fügen Sie persistenten Speicher zu Ihrer App hinzu, indem Sie einen PVC (Persistent Volume Claim) erstellen. Dieses Beispiel verwendet die Speicherklasse `ibmc-file-bronze`. Um verfügbare Speicherklassen zu überprüfen, führen Sie den folgenden Befehl aus: `kubectl get storageclasses`.

    ```
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: mypvc
      annotations:
        volume.beta.kubernetes.io/storage-class: "ibmc-file-bronze"
    spec:
      accessModes:
        - ReadWriteMany
      resources:
        requests:
          storage: 20Gi
    ```
    {: codeblock}

3.  Erstellen Sie den PVC.

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

4.  Fügen Sie in Ihrer `.yaml`-Bereitstellungsdatei den Init-Container hinzu. Beziehen Sie die UID und GID ein, die Sie zuvor abgerufen haben.

    ```
    initContainers:
    - name: initContainer # Kann durch einen beliebigen Namen ersetzt werden
      image: alpine:latest
      command: ["/bin/sh", "-c"]
      args:
        - chown <UID>:<GID> /mount; # Ersetzen Sie UID und GID durch Werte aus der Dockerfile
      volumeMounts:
      - name: volume # Kann durch einen beliebigen Namen ersetzt werden
        mountPath: /mount # Muss mit dem Mountpfad in der args-Zeile übereinstimmen
    ```
    {: codeblock}

    **Beispiel** für eine Jenkins-Bereitstellung

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: mein_pod
    spec:
      replicas: 1
      template:
        metadata:
          labels:
            app: jenkins
        spec:
          containers:
          - name: jenkins
            image: jenkins
            volumeMounts:
            - mountPath: /var/jenkins_home
              name: volume
          volumes:
          - name: volume
            persistentVolumeClaim:
              claimName: mypvc
          initContainers:
          - name: permissionsfix
            image: alpine:latest
            command: ["/bin/sh", "-c"]
            args:
              - chown 1000:1000 /mount;
            volumeMounts:
            - name: volume
              mountPath: /mount
    ```
    {: codeblock}

5.  Erstellen Sie den Pod und hängen Sie den PVC an Ihren Pod an.

    ```
    kubectl apply -f mein_pod.yaml
    ```
    {: pre}

6. Überprüfen Sie, ob der Datenträger erfolgreich an Ihren Pod angehängt wurde. Notieren Sie den Namen des Pod und den **Containers/Mounts**-Pfad.

    ```
    kubectl describe pod <mein_pod>
    ```
    {: pre}

    **Beispielausgabe**:

    ```
    Name:		    mypod-123456789
    Namespace:	default
    ...
    Init Containers:
    ...
    Mounts:
      /mount from volume (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-cp9f0 (ro)
    ...
    Containers:
      jenkins:
        Container ID:
        Image:		jenkins
        Image ID:
        Port:		  <none>
        State:		Waiting
          Reason:		PodInitializing
        Ready:		False
        Restart Count:	0
        Environment:	<none>
        Mounts:
          /var/jenkins_home from volume (rw)
          /var/run/secrets/kubernetes.io/serviceaccount from default-token-cp9f0 (ro)
    ...
    Volumes:
      myvol:
        Type: PersistentVolumeClaim (Referenz auf einen PersistentVolumeClaim im gleichen Namensbereich)
        ClaimName: mypvc
        ReadOnly: false

    ```
    {: screen}

7.  Melden Sie sich beim Pod an, indem Sie den Podnamen verwenden, den Sie zuvor notiert haben.

    ```
    kubectl exec -it <mein_pod-123456789> /bin/bash
    ```
    {: pre}

8. Überprüfen Sie die Berechtigungen des Mountpfads Ihres Containers. In dem Beispiel ist der Mountpfad `/var/jenkins_home`.

    ```
    ls -ln /var/jenkins_home
    ```
    {: pre}

    **Beispielausgabe**:

    ```
    jenkins@mypod-123456789:/$ ls -ln /var/jenkins_home
    total 12
    -rw-r--r-- 1 1000 1000  102 Mar  9 19:58 copy_reference_file.log
    drwxr-xr-x 2 1000 1000 4096 Mar  9 19:58 init.groovy.d
    drwxr-xr-x 9 1000 1000 4096 Mar  9 20:16 war
    ```
    {: screen}

    Diese Ausgabe zeigt, dass die GID und UID von Ihrer Dockerfile (in diesem Beispiel `1000` und `1000`) Eigner des Mountpfads innerhalb des Containers sind.

<br />


## Hinzufügen von Zugriff für Benutzer ohne Rootberechtigung auf persistente Speicher ist fehlgeschlagen
{: #cs_storage_nonroot}

{: tsSymptoms}
Nach dem [Hinzufügen von Zugriff für Benutzer ohne Rootberechtigung auf persistenten Speicher](#nonroot) oder nach dem Bereitstellen eines Helm-Diagramms, in dem ein Benutzer ohne Rootberechtigung angegeben ist, kann der Benutzer nicht in den angehängten Speicher schreiben.

{: tsCauses}
Die Bereitstellungs- oder Helm-Diagrammkonfiguration gibt den [Sicherheitskontext](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/) für `fsGroup` (Gruppen-ID) und `runAsUser` (Benutzer-ID) des Pod an. Aktuell unterstützt {{site.data.keyword.containershort_notm}} die Spezifikation `fsGroup` nicht, sondern nur `runAsUser` mit der Festlegung als `0` (Rootberechtigungen).

{: tsResolve}
Entfernen Sie in der Konfiguration die `securityContext`-Felder für `fsGroup` und `runAsUser` aus dem Image, der Bereitstellungs- oder Helm-Diagrammkonfigurationsdatei und nehmen Sie die Bereitstellung erneut vor. Wenn Sie die Eigentumsrechte des Mountpfads von `nobody` ändern müssen, [fügen Sie Zugriff für Benutzer ohne Rootberechtigung hinzu](#nonroot). Nachdem Sie den [non-root initContainer](#nonroot) hinzugefügt haben, legen Sie `runAsUser` auf Containerebene fest, nicht Podebene.

<br />




## Das Anhängen eines vorhandenen Blockspeichers an einen Pod schlägt aufgrund des falschen Dateisystems fehl
{: #block_filesystem}

{: tsSymptoms}
Bei der Ausführung des Befehls `kubectl describe pod <pod_name>` wird der folgende Fehler angezeigt:
```
failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32
```
{: screen}

{: tsCauses}
Sie verfügen über eine vorhandene Blockspeichereinheit, die für ein `XFS`-Dateisystem konfiguriert ist. Um diese Einheit an Ihren Pod anzuhängen, haben Sie [einen persistenten Datenträger (PV) erstellt](cs_storage.html#existing_block), der `ext4` als Ihr Dateisystem oder kein Dateisystem im Abschnitt `spec/flexVolume/fsType` angegeben hat. Wenn kein Dateisystem definiert ist, nimmt der persistente Datenträger standardmäßig den Wert `ext4` ein.
Der persistente Datenträger wurde erfolgreich erstellt und mit der vorhandenen Blockspeicherinstanz verknüpft. Wenn Sie jedoch versuchen, den persistenten Datenträger mithilfe eines PVC an den Cluster anzuhängen, schlägt dieser Vorgang fehl. Sie können die `XFS`-Blockspeicherinstanz nicht mit einem `ext4`-Dateisystem an den Pod anhängen.

{: tsResolve}
Aktualisieren Sie das Dateisystem im vorhandenen persistenten Datenträger von `ext4` auf `XFS`.

1. Listen Sie die vorhandenen persistenten Datenträger in Ihrem Cluster auf und notieren Sie sich den Namen des persistenten Datenträgers, den Sie für die vorhandene Blockspeicherinstanz verwendet haben.
   ```
   kubectl get pv
   ```
   {: pre}

2. Speichern Sie die YALM-Datei des persistenten Datenträgers auf der lokalen Maschine.
   ```
   kubectl get pv <pv-name> -o yaml > <dateipfad/xfs_pv.yaml>
   ```
   {: pre}

3. Öffnen Sie die YALM-Datei und ändern Sie den Wert für `fsType` von `ext4` in `xfs`.
4. Ersetzen Sie den persistenten Datenträger im Cluster.
   ```
   kubectl replace --force -f <dateipfad/xfs_pv.yaml>
   ```
   {: pre}

5. Melden Sie sich bei dem Pod an, an den Sie den persistenten Datenträger angehängt haben.
   ```
   kubectl exec -it <pod-name> sh
   ```
   {: pre}

6. Stellen Sie sicher, dass das Dateisystem in `XFS` geändert wurde.
   ```
   df -Th
   ```
   {: pre}

   Beispielausgabe:
   ```
   Filesystem Type Size Used Avail Use% Mounted on /dev/mapper/3600a098031234546d5d4c9876654e35 xfs 20G 33M 20G 1% /myvolumepath
   ```
   {: screen}

<br />



## Hilfe und Unterstützung anfordern
{: #ts_getting_help}

Haben Sie noch immer Probleme mit Ihrem Cluster?
{: shortdesc}

-   [Überprüfen Sie auf der {{site.data.keyword.Bluemix_notm}}-Statusseite ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/bluemix/support/#status), ob {{site.data.keyword.Bluemix_notm}} verfügbar ist.
-   Veröffentlichen Sie eine Frage im [{{site.data.keyword.containershort_notm}}-Slack ![External link icon](../icons/launch-glyph.svg "Symbol für externen Link")](https://ibm-container-service.slack.com).

    Wenn Sie keine IBM ID für Ihr {{site.data.keyword.Bluemix_notm}}-Konto verwenden, [fordern Sie eine Einladung](https://bxcs-slack-invite.mybluemix.net/) zu diesem Slack an.
    {: tip}
-   Suchen Sie in entsprechenden Foren, ob andere Benutzer auf das gleiche Problem
gestoßen sind. Versehen Sie Ihre Fragen in den Foren mit Tags, um sie für das Entwicklungsteam
von {{site.data.keyword.Bluemix_notm}} erkennbar zu machen.

    -   Wenn Sie technische Fragen zur Entwicklung oder Bereitstellung von Clustern oder Apps mit {{site.data.keyword.containershort_notm}} haben, veröffentlichen Sie Ihre Frage auf [Stack Overflow ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) und versehen Sie sie mit den Tags `ibm-cloud`, `kubernetes` und `containers`.
    -   Verwenden Sie für Fragen zum Service und zu ersten Schritten das Forum [IBM developerWorks dW Answers ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix). Geben Sie die Tags `ibm-cloud` und `containers` an.
    Weitere Details zur Verwendung der Foren
finden Sie unter [Hilfe anfordern](/docs/get-support/howtogetsupport.html#using-avatar).

-   Wenden Sie sich an den IBM Support, indem Sie ein Ticket öffnen. Informationen zum Öffnen eines IBM Support-Tickets oder zu Supportstufen und zu Prioritätsstufen von Tickets finden Sie unter [Support kontaktieren](/docs/get-support/howtogetsupport.html#getting-customer-support).

{: tip}
Geben Sie beim Melden eines Problems Ihre Cluster-ID an. Führen Sie den Befehl `bx cs clusters` aus, um Ihre Cluster-ID abzurufen.

