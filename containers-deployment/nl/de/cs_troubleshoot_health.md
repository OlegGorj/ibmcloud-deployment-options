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



# Fehlerbehebung für die Protokollierung und Überwachung
{: #cs_troubleshoot_health}

Ziehen Sie bei der Verwendung von {{site.data.keyword.containerlong}} die folgenden Verfahren für die Fehlerbehebung bei der Verwendung von Protokollierung und Überwachung in Betracht.
{: shortdesc}

Wenn Sie ein allgemeineres Problem haben, testen Sie das [Cluster-Debugging](cs_troubleshoot.html).
{: tip}

## Protokolle werden nicht angezeigt
{: #cs_no_logs}

{: tsSymptoms}
Beim Zugriff auf das Kibana-Dashboard werden Ihre Protokolle nicht angezeigt.

{: tsResolve}
Überprüfen Sie die folgenden möglichen Ursachen für nicht angezeigte Clusterprotokolle sowie die entsprechenden Fehlerbehebungsschritte:

<table>
<caption>Nicht angezeigte Protokolle zur Fehlerbehebung</caption>
  <col width="40%">
  <col width="60%">
  <thead>
    <tr>
      <th>Mögliche Ursache</th>
      <th>Fehlerbehebungsmaßnahme</th>
    </tr>
 </thead>
 <tbody>
  <tr>
    <td>Es ist keine Protokollierungskonfiguration eingerichtet.</td>
    <td>Damit Protokolle gesendet werden, müssen Sie eine Protokollierungskonfiguration erstellen. Informationen dazu finden Sie unter <a href="cs_health.html#logging">Clusterprotokollierung konfigurieren</a>.</td>
  </tr>
  <tr>
    <td>Der Cluster weist nicht den Status <code>Normal</code> auf.</td>
    <td>Informationen darüber, wie Sie den Status des Clusters überprüfen können, finden Sie unter <a href="cs_troubleshoot.html#debug_clusters">Cluster debuggen</a>.</td>
  </tr>
  <tr>
    <td>Das Kontingent für den Protokollspeicher ist erschöpft.</td>
    <td>Informationen darüber, wie Sie die Protokollspeichergrenze erhöhen können, finden Sie in der <a href="/docs/services/CloudLogAnalysis/troubleshooting/error_msgs.html">{{site.data.keyword.loganalysislong_notm}}-Dokumentation</a>.</td>
  </tr>
  <tr>
    <td>Wenn Sie beim Erstellen des Clusters einen Bereich angegeben haben, verfügt der Kontoeigner nicht über die Berechtigungen eines Managers, Entwicklers oder Prüfers für diesen Bereich.</td>
      <td>Gehen Sie wie folgt vor, um die Zugriffsberechtigungen für den Kontoeigner zu ändern:
      <ol><li>Führen Sie den folgenden Befehl aus, um den Kontoeigner für den Cluster zu ermitteln: <code>bx cs api-key-info &lt;clustername_oder_-id&gt;</code>.</li>
      <li>Informationen darüber, wie dem betreffenden Kontoeigner die {{site.data.keyword.containershort_notm}}-Zugriffsberechtigung eines Managers, Entwicklers oder Prüfers für den Bereich zugeordnet werden kann, finden Sie unter <a href="cs_users.html">Clusterzugriff verwalten</a>.</li>
      <li>Führen Sie den folgenden Befehl aus, um das Protokollierungstoken nach Änderung der Berechtigungen zu aktualisieren: <code>bx cs logging-config-refresh &lt;clustername_oder_-id&gt;</code>.</li></ol></td>
    </tr>
    <tr>
      <td>Sie haben eine Anwendungsprotokollierungskonfiguration mit einer symbolischen Verbindung in Ihrem App-Pfad.</td>
      <td><p>Damit Protokolle gesendet werden, müssen Sie einen absoluten Pfad in Ihrer Protokollierungskonfiguration verwenden. Andernfalls können die Protokolle nicht gelesen werden. Falls Ihr Pfad an Ihren Workerknoten angehängt ist, wurde dadurch möglicherweise die symbolische Verbindung erstellt.</p> <p>Beispiel: Falls der angegebene Pfad <code>/usr/local/<b>spark</b>/work/app-0546/0/stderr</code> lautet, aber die Protokolle an <code>/usr/local/<b>spark-1.0-hadoop-1.2</b>/work/app-0546/0/stderr</code> gesendet werden, können sie nicht gelesen werden.</p></td>
    </tr>
  </tbody>
</table>

Um Änderungen zu testen, die Sie während der Fehlerbehebung vorgenommen haben, können Sie *Noisy* auf einem Workerknoten in Ihrem Cluster bereitstellen. Dabei handelt es sich um einen Beispiel-Pod, der mehrere Protokollereignisse generiert.

  1. [Geben Sie als Ziel der CLI](cs_cli_install.html#cs_cli_configure) einen Cluster an, auf dem Protokolle generiert werden sollen.

  2. Erstellen Sie die Konfigurationsdatei `deploy-noisy.yaml`.

      ```
      apiVersion: v1
      kind: Pod
      metadata:
        name: noisy
      spec:
        containers:
        - name: noisy
          image: ubuntu:16.04
          command: ["/bin/sh"]
          args: ["-c", "while true; do sleep 10; echo 'Hello world!'; done"]
          imagePullPolicy: "Always"
        ```
        {: codeblock}

  3. Führen Sie die Konfigurationsdatei in dem Kontext des Clusters aus.

        ```
        kubectl apply -f noisy.yaml
        ```
        {:pre}

  4. Nach einigen Minuten werden die Protokolle im Kibana-Dashboard angezeigt. Zum Zugriff auf das Kibana-Dashboard müssen Sie eine der folgenden URLs aufrufen und dann das {{site.data.keyword.Bluemix_notm}}-Konto, in dem Sie den Cluster erstellt haben, auswählen. Wenn Sie beim Erstellen des Clusters einen Bereich angegeben haben, wechseln Sie stattdessen zu diesem Bereich.
      - Vereinigte Staaten (Süden) und Vereinigte Staaten (Osten): https://logging.ng.bluemix.net
      - Großbritannien (Süden): https://logging.eu-gb.bluemix.net
      - Zentraleuropa: https://logging.eu-fra.bluemix.net
      - Asiatisch-pazifischer Raum (Süden): https://logging.au-syd.bluemix.net

<br />


## Im Kubernetes-Dashboard werden keine Nutzungsdiagramme angezeigt
{: #cs_dashboard_graphs}

{: tsSymptoms}
Beim Zugriff auf das Kubernetes-Dashboard werden keine Nutzungsdiagramme angezeigt.

{: tsCauses}
Nach einer Clusteraktualisierung oder dem Neustart eines Workerknotens kann es vorkommen, dass der Pod `kube-dashboard` nicht aktualisiert wird.

{: tsResolve}
Löschen Sie den Pod `kube-dashboard`, um einen Neustart zu erzwingen. Der Pod wird mit RBAC-Richtlinien erneut erstellt, um die Informationen zur Auslastung von Heapster abzurufen.

  ```
  kubectl delete pod -n kube-system $(kubectl get pod -n kube-system --selector=k8s-app=kubernetes-dashboard -o jsonpath='{.items..metadata.name}')
  ```
  {: pre}

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

