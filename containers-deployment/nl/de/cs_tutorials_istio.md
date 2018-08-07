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



# Lernprogramm: Istio in einer {{site.data.keyword.containerlong_notm}} bereitstellen
{: #istio_tutorial}

[Istio](https://www.ibm.com/cloud/info/istio) ist eine offene Plattform zum Verbinden, Sichern und Verwalten eines Netzes von Mikroservices (auch als Servicenetz bezeichnet) auf Cloudplattformen wie Kubernetes in {{site.data.keyword.containerlong}}. Mit Istio können Sie den Netzverkehr und den Lastausgleich zwischen Mikroservices verwalten, Zugriffsrichtlinien durchsetzen, Serviceidentitäten überprüfen und vieles mehr.
{:shortdesc}

In diesem Lernprogramm sehen Sie, wie Istio zusammen mit vier Mikroservices für eine einfache Beispiel-App für Buchhandlungen namens BookInfo installiert wird. Die Mikroservices umfassen eine Produktwebseite, Buchdetails, Rezensionen und Bewertungen. Wenn Sie die Mikroservices von BookInfo in einem {{site.data.keyword.containershort}}-Cluster bereitstellen, in dem Istio installiert ist, fügen Sie Istio Envoy-Sidecar-Proxys in die Pods für jeden Mikroservice ein.

**Hinweis**: Manche Konfigurationen und Features der Istio-Plattform sind noch in Entwicklung begriffen und ändern sich basierend auf dem Feedback von Benutzern. Warten Sie noch ein paar Monate, bis sich die Lage stabilisiert hat, bevor Sie Istio in der Produktion einsetzen. 

## Ziele

-   Istio herunterladen und in Ihrem Cluster installieren
-   Beispiel-App BookInfo implementieren
-   Envoy Sidecar Proxys in die Pods der vier Mikroservices der App einfügen, um die Mikroservices mit und im Servicenetz zu verbinden
-   Implementierung der App BookInfo überprüfen und einen Blick auf die drei Versionen der Bewertungsservices werfen

## Erforderlicher Zeitaufwand

30 Minuten

## Zielgruppe

Dieses Lernprogramm ist für Softwareentwickler und Netzadministratoren konzipiert, die Istio zum ersten Mal verwenden.

## Voraussetzungen

-  [Installieren Sie die CLIs](cs_cli_install.html#cs_cli_install_steps). Für Istio ist Kubernetes Version 1.9 oder höher erforderlich. Stellen Sie sicher, dass Sie die CLI-Version von `kubectl` installieren, die der Kubernetes-Version Ihres Clusters entspricht.
-  [Erstellen Sie einen Cluster](cs_clusters.html#clusters_cli) mit Kubernetes Version 1.9 oder höher.
-  [Richten Sie die CLI auf Ihren Cluster aus](cs_cli_install.html#cs_cli_configure).

## Lerneinheit 1: Istio herunterladen und installieren
{: #istio_tutorial1}

Istio in Ihren Cluster herunterladen und installieren
{:shortdesc}

1. Laden Sie Istio entweder direkt von [https://github.com/istio/istio/releases ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/istio/istio/releases) herunter oder rufen Sie die aktuelle Version mithilfe von 'curl' ab:

   ```
   curl -L https://git.io/getLatestIstio | sh -
   ```
   {: pre}

2. Extrahieren Sie die Installationsdateien.

3. Fügen Sie den Client `istioctl` zu Ihrer PATH-Variablen hinzu. Führen Sie beispielsweise den folgenden Befehl auf einem MacOS- oder Linux-System aus:

   ```
   export PATH=$PWD/istio-0.4.0/bin:$PATH
   ```
   {: pre}

4. Ändern Sie das Verzeichnis in die Istio-Dateiadresse.

   ```
   cd filepath/istio-0.4.0
   ```
   {: pre}

5. Installieren Sie Istio auf dem Kubernetes-Cluster. Istio wird im Kubernetes-Namensbereich `istio-system` implementiert.

   ```
   kubectl apply -f install/kubernetes/istio.yaml
   ```
   {: pre}

   **Hinweis**: Wenn Sie gegenseitige TLS-Authentifizierung zwischen Sidecars aktivieren müssen, können Sie stattdessen die Datei `istio-auth` installieren: `kubectl apply -f install/kubernetes/istio-auth.yaml`

6. Stellen Sie sicher, dass die Kubernetes-Services `istio-pilot`, `istio-mixer` und `istio-ingress` vollständig bereitgestellt sind, bevor Sie fortfahren.

   ```
   kubectl get svc -n istio-system
   ```
   {: pre}

   ```
   NAME            TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)                                                            AGE
   istio-ingress   LoadBalancer   172.21.xxx.xxx   169.xx.xxx.xxx   80:31176/TCP,443:30288/TCP                                         2m
   istio-mixer     ClusterIP      172.21.xxx.xxx     <none>           9091/TCP,15004/TCP,9093/TCP,9094/TCP,9102/TCP,9125/UDP,42422/TCP   2m
   istio-pilot     ClusterIP      172.21.xxx.xxx    <none>           15003/TCP,443/TCP                                                  2m
   ```
   {: screen}

7. Stellen Sie sicher, dass die entsprechenden Pods `istio-pilot-*`, `istio-mixer-*`, `istio-ingress-*` und `istio-ca-*` ebenfalls vollständig bereitgestellt sind, bevor Sie fortfahren.

   ```
   kubectl get pods -n istio-system
   ```
   {: pre}

   ```
   istio-ca-3657790228-j21b9           1/1       Running   0          5m
   istio-ingress-1842462111-j3vcs      1/1       Running   0          5m
   istio-pilot-2275554717-93c43        1/1       Running   0          5m
   istio-mixer-2104784889-20rm8        2/2       Running   0          5m
   ```
   {: screen}


Hervorragend! Sie haben Istio erfolgreich in Ihrem Cluster installiert. Stellen Sie im nächsten Schritt die Beispiel-App BookInfo in Ihrem Cluster bereit.


## Lerneinheit 2: App BookInfo bereitstellen
{: #istio_tutorial2}

Stellen Sie die Mikroservices der Beispiel-App BookInfo in Ihrem Kubernetes-Cluster bereit.
{:shortdesc}

Diese vier Mikroservices umfassen eine Produktwebseite, Buchdetails, Rezensionen (mit verschiedenen Versionen des Rezensionsmikroservice) sowie Bewertungen. Sie finden alle in diesem Beispiel verwendeten Dateien im Verzeichnis `samples/bookinfo` Ihrer Istio-Installation.

Wenn Sie BookInfo bereitstellen, werden Envoy Sidecar Proxys als Container in die Pods der Mikroservices Ihrer App eingefügt, bevor die Mikroservice-Pods bereitgestellt werden. Istio verwendet eine erweiterte Version des Envoy-Proxys, um den gesamten eingehenden und ausgehenden Datenverkehr für alle Mikroservices im Servicenetz auszugleichen. Weitere Informationen zu Envoy finden Sie in der [Istio-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://istio.io/docs/concepts/what-is-istio/overview.html#envoy).

1. Stellen Sie die App BookInfo bereit. Der Befehl `kube-inject` fügt Envoy zur Datei `bookinfo.yaml` hinzu und verwendet diese aktualisierte Datei, um die App zu implementieren. Wenn die Mikroservices der App bereitgestellt werden, wird auch der Envoy Sidecar in den einzelnen Mikroservice-Pods bereitgestellt.

   ```
   kubectl apply -f <(istioctl kube-inject -f samples/bookinfo/kube/bookinfo.yaml)
   ```
   {: pre}

2. Stellen Sie sicher, dass die Mikroservices und ihre zugehörigen Pods bereitgestellt werden:

   ```
   kubectl get svc
   ```
   {: pre}

   ```
   NAME                       CLUSTER-IP   EXTERNAL-IP   PORT(S)              AGE
   details                    10.xxx.xx.xxx    <none>        9080/TCP             6m
   kubernetes                 10.xxx.xx.xxx     <none>        443/TCP              30m
   productpage                10.xxx.xx.xxx   <none>        9080/TCP             6m
   ratings                    10.xxx.xx.xxx    <none>        9080/TCP             6m
   reviews                    10.xxx.xx.xxx   <none>        9080/TCP             6m
   ```
   {: screen}

   ```
   kubectl get pods
   ```
   {: pre}

   ```
   NAME                                        READY     STATUS    RESTARTS   AGE
   details-v1-1520924117-48z17                 2/2       Running   0          6m
   productpage-v1-560495357-jk1lz              2/2       Running   0          6m
   ratings-v1-734492171-rnr5l                  2/2       Running   0          6m
   reviews-v1-874083890-f0qf0                  2/2       Running   0          6m
   reviews-v2-1343845940-b34q5                 2/2       Running   0          6m
   reviews-v3-1813607990-8ch52                 2/2       Running   0          6m
   ```
   {: screen}

3. Rufen Sie die öffentliche IP-Adresse für Ihren Cluster ab, um die Anwendungsbereitstellung zu überprüfen.

    * Wenn Sie mit einem Standardcluster arbeiten, führen Sie den folgenden Befehl aus, um die Ingress-IP-Adresse und den Port Ihres Clusters abzurufen:

       ```
       kubectl get ingress
       ```
       {: pre}

       Beispielausgabe:

       ```
       NAME      HOSTS     ADDRESS          PORTS     AGE
       gateway   *         169.xx.xxx.xxx   80        3m
       ```
       {: screen}

       Die resultierende Ingress-Adresse für dieses Beispiel ist `169.48.221.218:80`. Exportieren Sie die Adresse als Gateway-URL mit dem folgenden Befehl. Sie werden die Gateway-URL im nächsten Schritt für den Zugriff auf die BookInfo-Produktseite verwenden.

       ```
       export GATEWAY_URL=169.xx.xxx.xxx:80
       ```
       {: pre}

    * Wenn Sie mit einem kostenlosen Cluster arbeiten, müssen sie die öffentliche IP-Adresse des Workerknotens und des Knotenports (NodePort) verwenden. Führen Sie den folgenden Befehl aus, um die öffentliche IP-Adresse des Workerknotens abzurufen:

       ```
       bx cs workers <clustername_oder_-id>
       ```
       {: pre}

       Exportieren Sie die öffentliche IP-Adresse des Workerknotens als Gateway-URL mit dem folgenden Befehl. Sie werden die Gateway-URL im nächsten Schritt für den Zugriff auf die BookInfo-Produktseite verwenden.

       ```
       export GATEWAY_URL=<öffentliche_ip_des_workerknotens>:$(kubectl get svc istio-ingress -n istio-system -o jsonpath='{.spec.ports[0].nodePort}')
       ```
       {: pre}

4. Führen Sie den Befehl 'curl' für die Variable `GATEWAY_URL` aus, um zu prüfen, dass BookInfo aktiv ist. Die Antwort `200` bedeutet, dass BookInfo ordnungsgemäß mit Istio ausgeführt wird.

   ```
   curl -I http://$GATEWAY_URL/productpage
   ```
   {: pre}

5. Navigieren Sie in einem Browser zu `http://$GATEWAY_URL/productpage`, um die BookInfo-Webseite anzuzeigen.

6. Versuchen Sie mehrfach, die Seite zu aktualisieren. Verschiedene Versionen des Abschnitts mit den Rezensionen zeigen abwechselnd rote, schwarze oder gar keine Sterne an.

Hervorragend! Sie haben die Beispiel-App BookInfo erfolgreich  mit Istio Envoy-Sidecars bereitgestellt. Im nächsten Schritt können Sie Ihre Ressourcen bereinigen oder weitere Lernprogramme durchführen, um Istio näher zu erkunden.

## Bereinigen
{: #istio_tutorial_cleanup}

Wenn Sie die Arbeit mit Istio abgeschlossen haben und Istio nicht [weiter erkunden](#istio_tutorial_whatsnext) möchten, können Sie Ihre Istio-Ressourcen in Ihrem Cluster bereinigen.
{:shortdesc}

1. Löschen Sie alle BookInfo-Services, -Pods und -Bereitstellungen im Cluster.

   ```
   samples/bookinfo/kube/cleanup.sh
   ```
   {: pre}

2. Deinstallieren Sie Istio.

   ```
   kubectl delete -f install/kubernetes/istio.yaml
   ```
   {: pre}

## Womit möchten Sie fortfahren?
{: #istio_tutorial_whatsnext}

Weitere Anleitungen zur Verwendung von Istio finden Sie in der [Istio-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://istio.io/).

* [Intelligentes Routing ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://istio.io/docs/guides/intelligent-routing.html): In diesem Beispiel wird die Verwendung der verschiedenen Funktionen zur Verwaltung des Datenverkehrs in Istio aufgezeigt, um in BookInfo Datenverkehr an eine bestimmte Version der Mikroservices für Rezensionen und Bewertungen weiterzuleiten.

* [Intelligentes Routing ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://istio.io/docs/guides/telemetry.html): In diesem Beispiel wird die Verwendung der verschiedenen Funktionen zur Verwaltung des Datenverkehrs in Istio aufgezeigt, um in BookInfo Datenverkehr an eine bestimmte Version der Mikroservices für Rezensionen und Bewertungen weiterzuleiten.
