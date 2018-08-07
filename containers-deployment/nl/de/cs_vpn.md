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


# VPN-Konnektivität einrichten
{: #vpn}

Mit der VPN-Konnektivität können Sie sichere Verbindungen von Apps in einem Kubernetes-Cluster auf {{site.data.keyword.containerlong}} zu einem lokalen Netz herstellen. Sie können auch Apps, die nicht in Ihrem Cluster enthalten sind, mit Apps verbinden, die Teil Ihres Clusters sind.
{:shortdesc}

Um eine Verbindung Ihrer Workerknoten und Apps mit einem lokalen Rechenzentrum einzurichten, können Sie eine der folgenden Optionen konfigurieren.

- **StrongSwan-IPSec-VPN-Service**: Sie können einen [StrongSwan-IPSec-VPN-Service ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.strongswan.org/) konfigurieren, der Ihren Kubernetes-Cluster sicher mit einem lokalen Netz verbindet. Der StrongSwan-IPSec-VPN-Service stellt einen sicheren End-to-End-Kommunikationskanal über das Internet bereit, der auf der standardisierten IPsec-Protokollsuite (IPsec - Internet Protocol Security) basiert. Um eine sichere Verbindung zwischen Ihrem Cluster und einem lokalen Netz einzurichten, [konfigurieren und implementieren Sie den StrongSwan-IPSec-VPN-Service](#vpn-setup) direkt in einem Pod in Ihrem Cluster.

- **Virtual Router Appliance (VRA) oder Fortigate Security Appliance (FSA)**: Sie können zum Konfigurieren eines IPSec-VPN-Endpunkts entweder eine [VRA](/docs/infrastructure/virtual-router-appliance/about.html) oder eine [FSA](/docs/infrastructure/fortigate-10g/about.html) einrichten. Diese Option ist hilfreich, wenn der Cluster größer ist, Sie über das VPN auf Nicht-Kubernetes-Ressourcen oder über ein einzelnes VPN auf mehrere Cluster zugreifen möchten. Informationen zum Konfigurieren einer VRA finden Sie unter [VPN-Konnektivität mit VRA konfigurieren](#vyatta). 

## Helm-Diagramm zur Einrichtung von VPN-Konnektivität mit dem StrongSwan-IPSec-VPN-Service
{: #vpn-setup}

Verwenden Sie ein Helm-Diagramm, um den StrongSwan-IPSec-VPN-Service innerhalb eines Kubernetes-Pods zu konfigurieren und bereitzustellen.
{:shortdesc}

Da strongSwan in Ihren Cluster integriert ist, benötigen Sie keine externe Gateway-Einheit. Beim Einrichten der VPN-Konnektivität werden automatisch Routen auf allen Workerknoten im Cluster konfiguriert. Diese Routen ermöglichen eine bidirektionale Konnektivität über den VPN-Tunnel zwischen Pods auf allen Workerknoten und dem fernen System. Das folgende Diagramm zeigt beispielsweise, wie eine App in {{site.data.keyword.containershort_notm}} mit einem lokalen Server über eine strongSwan-VPN-Verbindung kommunizieren kann:

<img src="images/cs_vpn_strongswan.png" width="700" alt="App in {{site.data.keyword.containershort_notm}} mithilfe einer Lastausgleichsfunktion zugänglich machen" style="width:700px; border-style: none"/>

1. Eine App in Ihrem Cluster, `myapp`, empfängt eine Anforderung von einem Ingress- oder LoadBalancer-Service und muss eine sichere Verbindung mit Daten in Ihrem lokalen Netz herstellen.

2. Die Anforderung an das lokale Rechenzentrum wird an den IPSec-strongSwan-VPN-Pod weitergeleitet. Die IP-Zieladresse wird verwendet, um zu bestimmen, welche Netzpakete an den IPSec-strongSwan-VPN-Pod gesendet werden sollen.

3. Die Anforderung wird verschlüsselt und über den VPN-Tunnel an das lokale Rechenzentrum gesendet.

4. Die eingehende Anforderung passiert die lokale Firewall und wird an den VPN-Tunnelendpunkt (Router) zugestellt, wo sie entschlüsselt wird.

5. Der VPN-Tunnelendpunkt (Router) leitet die Anforderung abhängig von der in Schritt 2 angegebenen Ziel-IP-Adresse an den lokalen Server oder den Mainframe weiter. Die erforderlichen Daten werden auf dieselbe Weise über die VPN-Verbindung zurück an `myapp` gesendet.

### StrongSwan-Helm-Diagramm konfigurieren
{: #vpn_configure}

Vorbemerkungen:
* [Installieren Sie ein IPsec-VPN-Gateway in Ihrem lokalen Rechenzentrum](/docs/infrastructure/iaas-vpn/set-up-ipsec-vpn.html#setting-up-an-ipsec-connection).
* Entweder [erstellen Sie einen Standardcluster](cs_clusters.html#clusters_cli) oder Sie [aktualisieren einen vorhandenen Cluster auf Version 1.7.16 oder eine höhere Version](cs_cluster_update.html#master).
* Das Cluster muss mindestens eine verfügbare öffentliche Load Balancer-IP-Adresse haben. [Sie können Ihre verfügbaren öffentlichen IP-Adressen zur Überprüfung anzeigen](cs_subnets.html#manage) oder [eine bereits verwendete IP-Adresse freigeben](cs_subnets.html#free).
* [Richten Sie die Kubernetes-CLI auf den Cluster aus](cs_cli_install.html#cs_cli_configure).

Weitere Informationen zu den Helm-Befehlen zum Konfigurieren des StrongSwan-Diagramms finden Sie in der <a href="https://docs.helm.sh/helm/" target="_blank">Helm-Dokumentation <img src="../icons/launch-glyph.svg" alt="Symbol für externen Link"></a>.

Gehen Sie wie folgt vor, um das Helm-Diagramm zu konfigurieren:

1. [Installieren Sie Helm für Ihr Cluster und fügen Sie das {{site.data.keyword.Bluemix_notm}}-Repository zu Ihrer Helm-Instanz hinzu](cs_integrations.html#helm).

2. Speichern Sie die Standardkonfigurationseinstellungseinstellungen für das StrongSwan-Helm-Diagramm in einer lokalen YAML-Datei.

    ```
    helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. Öffnen Sie die Datei `config.yaml` und nehmen Sie die folgenden Änderungen an den Standardwerten entsprechend der gewünschten VPN-Konfiguration vor. Beschreibungen für erweiterte Einstellungen finden Sie in den Kommentaren zu den Konfigurationsdateien.

    **Wichtig**: Wenn Sie keine Eigenschaft ändern müssen, kommentieren Sie diese Eigenschaft aus, indem Sie jeweils ein `#` davorsetzen.

    <table>
    <caption>Erklärung dieser YAML-Komponenten</caption>
    <col width="22%">
    <col width="78%">
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der YAML-Dateikomponenten</th>
    </thead>
    <tbody>
    <tr>
    <td><code>localSubnetNAT</code></td>
    <td>Network Address Translation (NAT) für Teilnetze bietet eine Ausweichlösung für Teilnetzkonflikte zwischen lokalen Netzen und den lokalen Netzen im Unternehmen. Sie können NAT verwenden, um die privaten lokalen IP-Teilnetze des Clusters, das Pod-Teilnetz (172.30.0.0/16) oder das Teilnetz des Pod-Service (172.21.0.0/16) zu einem anderen privaten Teilnetz zuzuordnen. Der VPN-Tunnel erkennt erneut zugeordnete IP-Teilnetze, die an Stelle der ursprünglichen Teilnetze treten. Die erneute Zuordnung erfolgt vor dem Senden der Pakete über den VPN-Tunnel sowie nach dem Eintreffen der Pakete aus dem VPN-Tunnel. Sie können sowohl erneut zugeordnete als auch nicht erneut zugeordnete Teilnetze gleichzeitig über VPN bereitstellen.<br><br>Um NAT aktivieren zu können, können Sie entweder ein vollständiges Teilnetz oder einzelne IP-Adressen hinzufügen. Wenn Sie ein vollständiges Teilnetz hinzufügen (im Format <code>10.171.42.0/24=10.10.10.0/24</code>), erfolgt die erneute Zuordnung 1-zu-1: Alle IP-Adressen im internen Teilnetz werden einem externen Teilnetz zugeordnet und umgekehrt. Wenn Sie einzelne IP-Adressen (im Format <code>10.171.42.17/32=10.10.10.2/32, 10.171.42.29/32=10.10.10.3/32</code>) zuordnen, werden nur diese internen IP-Adressen zu den angegebenen externen IP-Adressen zugeordnet.<br><br>Wenn Sie diese Option verwenden, ist das lokale Teilnetz, das über die VPN-Verbindung bereitgestellt wird, das Teilnetz "außerhalb", dem das "interne" Teilnetz zugeordnet wird.
    </td>
    </tr>
    <tr>
    <td><code>loadBalancerIP</code></td>
    <td>Wenn Sie eine portierbare öffentliche IP-Adresse für den strongSwan-VPN-Service für eingehende VPN-Verbindungen angeben möchten, fügen Sie diese IP-Adresse hinzu. Die Angabe einer IP-Adresse ist nützlich, wenn Sie eine fixe IP-Adresse benötigen, z. B. wenn Sie festlegen müssen, welche IP-Adressen von einer lokalen Firewall zugelassen werden.<br><br>Informationen zum Anzeigen verfügbarer portierbarer öffentlicher IP-Adressen, die diesem Cluster zugewiesen sind, finden Sie unter [IP-Adressen und Teilnetze verwalten](cs_subnets.html#manage). Wenn Sie diese Einstellung leer lassen, wird eine freie portierbare öffentliche IP-Adresse verwendet. Wenn die VPN-Verbindung vom lokalen Gateway im Unternehmen initialisiert wird (für <code>ipsec.auto</code> ist <code>add</code> festgelegt), können Sie diese Eigenschaft verwenden, um eine persistente, öffentliche IP-Adresse auf dem lokalen Gateway im Unternehmen für den Cluster zu konfigurieren.</td>
    </tr>
    <tr>
    <td><code>connectUsingLoadBalancerIP</code></td>
    <td>Verwenden Sie die IP-Adresse der Lastausgleichsfunktion, die Sie in <code>loadBalancerIP</code> hinzugefügt haben, um auch die ausgehende VPN-Verbindung herzustellen. Ist diese Option aktiviert, müssen sich alle Workerknoten des Clusters in demselben öffentlichen VLAN befinden. Andernfalls müssen Sie die Einstellung <code>nodeSelector</code> verwenden, um sicherzustellen, dass der VPN-Pod auf einem Workerknoten in demselben öffentliche VLAN wie <code>loadBalancerIP</code> bereitgestellt wird. Diese Option wird ignoriert, wenn <code>ipsec.auto</code> auf <code>add</code> gesetzt ist.<p>Gültige Werte:</p><ul><li><code>"false"</code>: Die Verbindung des VPN wird nicht mithilfe der IP der Lastausgleichsfunktion hergestellt. Stattdessen wird die öffentliche IP-Adresse des Workerknotens, auf dem der VPN-Pod ausgeführt wird, verwendet.</li><li><code>"true"</code>: Die Verbindung des VPN wird mithilfe der IP der Lastausgleichsfunktion als lokaler Quellen-IP hergestellt. Wenn <code>loadBalancerIP</code> nicht festgelegt ist, wird die externe IP-Adresse, die dem Lastausgleichsservice zugewiesen ist, verwendet.</li><li><code>"auto"</code>: Wenn <code>ipsec.auto</code> auf <code>start</code> gesetzt und <code>loadBalancerIP</code> festgelegt ist, wird das VPN mithilfe der IP der Lastausgleichsfunktion als lokaler Quellen-IP eingerichtet.</li></ul></td>
    </tr>
    <tr>
    <td><code>nodeSelector</code></td>
    <td>Um zu beschränken, auf welchen Knoten der StrongSwan-VPN-Pod bereitgestellt wird, fügen Sie die IP-Adresse eines bestimmten Workerknotens oder eine Workerknotenbezeichnung hinzu. Der Wert <code>kubernetes.io/hostname: 10.xxx.xx.xxx</code> beschränkt zum Beispiel den VPN-Pod auf die Ausführung auf diesem bestimmten Workerknoten. Der Wert <code>strongswan: vpn</code> beschränkt die Ausführung des VPN-Pods auf alle Workerknoten mit dieser Bezeichnung. Sie können jede beliebige Workerknotenbezeichnung verwenden. Es wird jedoch empfohlen, dass Sie <code>strongswan: &lt;releasename&gt;</code> verwenden, damit unterschiedliche Workerknoten mit verschiedenen Bereitstellungen dieses Diagramms verwendet werden können.<br><br>Falls die VPN-Verbindung vom Cluster initialisiert wird (für <code>ipsec.auto</code> ist <code>start</code> festgelegt), können Sie diese Eigenschaft verwenden, um die Quellen-IP-Adressen der VPN-Verbindung zu beschränken, die auf dem lokalen Gateway im Unternehmen bereitgestellt werden. Dieser Wert ist optional.</td>
    </tr>
    <tr>
    <td><code>ipsec.keyexchange</code></td>
    <td>Wenn Ihr lokaler VPN-Tunnelendpunkt im Unternehmen <code>ikev2</code> nicht als Protokoll für die Initialisierung der Verbindung unterstützt, ändern Sie diesen Wert in <code>ikev1</code> oder <code>ike</code>.</td>
    </tr>
    <tr>
    <td><code>ipsec.esp</code></td>
    <td>Fügen Sie die Liste von ESP-Verschlüsselungs-/Authentifizierungsalgorithmen hinzu, die Ihr lokaler VPN-Tunnelendpunkt für die Verbindung verwendet.<ul><li>Wenn <code>ipsec.keyexchange</code> auf <code>ikev1</code> festgelegt ist, muss diese Einstellung angegeben werden.</li><li>Wenn <code>ipsec.keyexchange</code> auf <code>ikev2</code> festgelegt ist, ist diese Einstellung optional. Wenn Sie diese Einstellung leer lassen, wird der Standardwert für strongSwan-Algorithmen <code>aes128-sha1,3des-sha1</code> für die Verbindung verwendet.</li></ul></td>
    </tr>
    <tr>
    <td><code>ipsec.ike</code></td>
    <td>Fügen Sie die Liste von IKE/ISAKMP-SA-Verschlüsselungs-/Authentifizierungsalgorithmen hinzu, die Ihr lokaler VPN-Tunnelendpunkt für die Verbindung verwendet.<ul><li>Wenn <code>ipsec.keyexchange</code> auf <code>ikev1</code> festgelegt ist, muss diese Einstellung angegeben werden.</li><li>Wenn <code>ipsec.keyexchange</code> auf <code>ikev2</code> festgelegt ist, ist diese Einstellung optional. Wenn Sie diese Einstellung leer lassen, wird der Standardwert für strongSwan-Algorithmen <code>aes128-sha1-modp2048,3des-sha1-modp1536</code> für die Verbindung verwendet.</li></ul></td>
    </tr>
    <tr>
    <td><code>ipsec.auto</code></td>
    <td>Wenn Sie möchten, dass der Cluster die VPN-Verbindung initialisiert, ändern Sie diesen Wert in <code>start</code>.</td>
    </tr>
    <tr>
    <td><code>local.subnet</code></td>
    <td>Ändern Sie diesen Wert in die Liste von Clusterteilnetz-CIDRs, die über die VPN-Verbindung für das lokale Netz zugänglich sein sollen. Diese Liste kann die folgenden Teilnetze enthalten: <ul><li>Die Teilnetz-CIDR des Kubernetes-Pods: <code>172.30.0.0/16</code></li><li>Die Teilnetz-CIDR des Kubernetes-Service: <code>172.21.0.0/16</code></li><li>Wenn Ihre Apps über einen NodePort-Service im privaten Netz zugänglich gemacht werden, die private Teilnetz-CIDR des Workerknotens. Rufen Sie die ersten drei Oktette der privaten IP-Adresse Ihres Workers ab, indem Sie <code>bx cs worker &lt;clustername&gt;</code> ausführen. Beispiel: Bei <code>&lt;10.176.48.xx&gt;</code> notieren Sie sich <code>&lt;10.176.48&gt;</code>. Rufen Sie als Nächstes die private Teilnetz-CIDR Ihre Workers ab, indem Sie den folgenden Befehl ausführen. Ersetzen Sie dabei <code>&lt;xxx.yyy.zz&gt;</code> durch das eben abgerufene Oktett: <code>bx cs subnets | grep &lt;xxx.yyy.zzz&gt;</code>.</li><li>Wenn Sie Apps über LoadBalancer-Services im privaten Netz zugänglich machen, die private oder benutzerverwaltete Teilnetz-CIDR des Clusters. Sie finden diese Werte, indem Sie <code>bx cs cluster-get &lt;clustername&gt; --showResources</code> abrufen. Suchen Sie im Abschnitt **VLANS** nach CIDRs, deren **Public**-Wert <code>false</code> lautet.</li></ul>**Hinweis**: Wenn <code>ipsec.keyexchange</code> auf <code>ikev1</code> festgelegt ist, können Sie nur ein Teilnetz angeben.</td>
    </tr>
    <tr>
    <td><code>local.id</code></td>
    <td>Ändern Sie diesen Wert in die Zeichenfolge-ID für die lokale Kubernetes-Clusterseite, die Ihr VPN-Tunnelendpunkt für die Verbindung verwendet.</td>
    </tr>
    <tr>
    <td><code>remote.gateway</code></td>
    <td>Ändern Sie diesen Wert in die öffentliche IP-Adresse für das lokale VPN-Gateway. Wenn für <code>ipsec.auto</code> die Einstellung <code>start</code> festgelegt ist, ist dieser Wert erforderlich.</td>
    </tr>
    <td><code>remote.subnet</code></td>
    <td>Ändern Sie diesen Wert in die Liste von lokalen privaten Teilnetz-CIDRs, die für die Kubernetes-Cluster zugänglich sind. **Hinweis**: Wenn <code>ipsec.keyexchange</code> auf <code>ikev1</code> festgelegt ist, können Sie nur ein Teilnetz angeben.</td>
    </tr>
    <tr>
    <td><code>remote.id</code></td>
    <td>Ändern Sie diesen Wert in die Zeichenfolge-ID für die ferne lokale Seite, die Ihr VPN-Tunnelendpunkt für die Verbindung verwendet.</td>
    </tr>
    <tr>
    <td><code>remote.privateIPtoPing</code></td>
    <td>Fügen Sie die private IP-Adresse im fernen Teilnetz hinzu, damit diese von den Helm-Testprüfprogrammen für VPN-Verbindungstests mit dem Pingsignal verwendet wird. Dieser Wert ist optional.</td>
    </tr>
    <tr>
    <td><code>preshared.secret</code></td>
    <td>Ändern Sie diesen Wert in den vorab geteilten geheimen Schlüssel, den das Gateway Ihres lokalen VPN-Tunnelendpunkts für die Verbindung verwendet. Dieser Wert wird in <code>ipsec.secrets</code> gespeichert.</td>
    </tr>
    </tbody></table>

4. Speichern Sie die aktualisierte Datei `config.yaml`.

5. Installieren Sie das Helm-Diagramm auf Ihrem Cluster mit der aktualisierten Datei `config.yaml`. Die aktualisierten Eigenschaften werden in einer Konfigurationszuordnung für Ihr Diagramm gespeichert.

    **Hinweis**: Wenn Sie über mehrere VPN-Bereitstellungen in einem einzelnen Cluster verfügen, können Sie Namenskonflikte vermeiden und zwischen Ihren Bereitstellungen unterscheiden, indem Sie aussagekräftigere Releasenamen als `vpn` verwenden. Um das Abschneiden des Releasenamens zu vermeiden, begrenzen Sie den Releasenamen auf maximal 35 Zeichen.

    ```
    helm install -f config.yaml --name=vpn ibm/strongswan
    ```
    {: pre}

6. Prüfen Sie den Status der Diagrammbereitstellung. Sobald das Diagramm bereit ist, hat das Feld **STATUS** oben in der Ausgabe den Wert `DEPLOYED`.

    ```
    helm status vpn
    ```
    {: pre}

7. Sobald das Diagramm bereitgestellt ist, überprüfen Sie, dass die aktualisierten Einstellungen in der Datei `config.yaml` verwendet wurden.

    ```
    helm get values vpn
    ```
    {: pre}


### VPN-Konnektivität testen und überprüfen
{: #vpn_test}

Nachdem Sie Ihr Helm-Diagramm bereitgestellt haben, testen Sie die VPN-Konnektivität.
{:shortdesc}

1. Wenn das VPN im lokalen Gateway nicht aktiv ist, starten Sie das VPN.

2. Legen Sie die Umgebungsvariable `STRONGSWAN_POD` fest.

    ```
    export STRONGSWAN_POD=$(kubectl get pod -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
    ```
    {: pre}

3. Prüfen Sie den Status des VPN. Der Status `ESTABLISHED` bedeutet, dass die VPN-Verbindung erfolgreich war.

    ```
    kubectl exec $STRONGSWAN_POD -- ipsec status
    ```
    {: pre}

    Beispielausgabe:

    ```
    Security Associations (1 up, 0 connecting):
    k8s-conn[1]: ESTABLISHED 17 minutes ago, 172.30.xxx.xxx[ibm-cloud]...192.xxx.xxx.xxx[on-premises]
    k8s-conn{2}: INSTALLED, TUNNEL, reqid 12, ESP in UDP SPIs: c78cb6b1_i c5d0d1c3_o
    k8s-conn{2}: 172.21.0.0/16 172.30.0.0/16 === 10.91.152.xxx/26
    ```
    {: screen}

    **Hinweis**:

    <ul>
    <li>Wenn Sie versuchen, die VPN-Konnektivität mit dem StrongSwan-Helm-Diagramm zu erstellen, ist es wahrscheinlich, dass das VPN beim ersten Mal nicht den Status `ESTABLISHED` aufweist. Unter Umständen müssen Sie die Einstellungen des lokalen VPN-Endpunkts prüfen und die Konfigurationsdatei mehrmals ändern, bevor die Verbindung erfolgreich ist: <ol><li>Führen Sie folgenden Befehl aus: `helm delete --purge <release_name>`</li><li>Korrigieren Sie die falschen Werte in der Konfigurationsdatei.</li><li>Führen Sie `helm install -f config.yaml --name=<release_name> ibm/strongswan` aus.</li></ol>Sie können im nächsten Schritt noch weitere Prüfungen ausführen.</li>
    <li>Falls der VPN-Pod den Status `ERROR` aufweist oder immer wieder ausfällt und neu startet, kann dies an der Parametervalidierung der `ipsec.conf`-Einstellungen in der Konfigurationszuordnung des Diagramms liegen.<ol><li>Prüfen Sie, ob dies der Fall ist, indem Sie mithilfe des Befehls `kubectl logs -n $STRONGSWAN_POD` in den Protokollen des StrongSwan-Pods nach Validierungsfehlern suchen.</li><li>Wenn Gültigkeitsfehler vorhanden sind, führen Sie den folgenden Befehl aus: `helm delete --purge <release_name>`<li>Korrigieren Sie die falschen Werte in der Konfigurationsdatei.</li><li>Führen Sie `helm install -f config.yaml --name=<release_name> ibm/strongswan` aus.</li></ol>Wenn Ihr Cluster viele Workerknoten umfasst, können Sie mit `helm upgrade` Ihre Änderungen schneller anwenden, anstatt erst `helm delete` und `helm install` auszuführen.</li>
    </ul>

4. Sie können die VPN-Konnektivität weiter testen, indem Sie die fünf Helm-Tests ausführen, die in der StrongSwan-Diagrammdefinition enthalten sind.

    ```
    helm test vpn
    ```
    {: pre}

    * Wenn alle Tests erfolgreich sind, wurde Ihre StrongSwan-VPN-Verbindung erfolgreich konfiguriert.

    * Wenn ein Test fehlschlägt, fahren Sie mit dem nächsten Schritt fort.

5. Zeigen Sie die Ausgabe eines fehlgeschlagenen Tests an, indem Sie die Protokolle des Testpods anzeigen.

    ```
    kubectl logs <testprogramm>
    ```
    {: pre}

    **Hinweis**: Einige der Tests haben als Voraussetzungen optionale Einstellungen in der VPN-Konfiguration. Wenn einige der Tests fehlschlagen, kann dies abhängig davon, ob Sie diese optionalen Einstellungen angegeben haben, zulässig sein. Suchen Sie in der folgenden Tabelle nach Informationen zu den einzelnen Tests und warum ein Test fehlschlagen könnte.

    {: #vpn_tests_table}
    <table>
    <caption>Erklärung der Helm-VPN-Konnektivitätstests</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Helm-VPN-Konnektivitätstests</th>
    </thead>
    <tbody>
    <tr>
    <td><code>vpn-strongswan-check-config</code></td>
    <td>Überprüft die Syntax der Datei <code>ipsec.conf</code>, die von der Datei <code>config.yaml</code> generiert wird. Dieser Test kann aufgrund von falschen Werten in der Datei <code>config.yaml</code> fehlschlagen.</td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-check-state</code></td>
    <td>Überprüft, ob die VPN-Verbindung den Status <code>ESTABLISHED</code> aufweist. Dieser Test kann aus folgenden Gründen fehlschlagen:<ul><li>Unterschiede zwischen den Werten in der Datei <code>config.yaml</code> und den lokalen VPN-Endpunkteinstellungen im Unternehmen.</li><li>Wenn der Cluster empfangsbereit ist (im Modus "listen") (für <code>ipsec.auto</code> ist <code>add</code> festgelegt), ist die Verbindung auf der lokalen Unternehmensseite nicht eingerichtet.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-gw</code></td>
    <td>Überprüfen Sie die öffentliche IP-Adresse <code>remote.gateway</code>, die Sie in der Datei <code>config.yaml</code> konfiguriert haben mit einem Pingsignal, Dieser Test kann aus folgenden Gründen fehlschlagen:<ul><li>Sie haben keine IP-Adresse für das lokalen VPN-Gateway im Unternehmen angegeben. Falls für <code>ipsec.auto</code> die Einstellung <code>start</code> festgelegt ist, ist die IP-Adresse <code>remote.gateway</code> erforderlich.</li><li>Die VPN-Verbindung weist nicht den Status <code>ESTABLISHED</code> auf. Weitere Informationen finden Sie unter <code>vpn-strongswan-check-state</code>.</li><li>Die VPN-Konnektivität weist den Status <code>ESTABLISHED</code> auf aber ICMP-Pakete werden von der Firewall geblockt.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-ip-1</code></td>
    <td>Die private IP-Adresse <code>remote.privateIPtoPing</code> des lokalen VPN-Gateways im Unternehmen wird vom VPN-Pod im Cluster mit einem Pingsignal überprüft. Dieser Test kann aus folgenden Gründen fehlschlagen:<ul><li>Sie haben keine IP-Adresse <code>remote.privateIPtoPing</code> angegeben. Wenn Sie absichtlich keine IP-Adresse angegeben haben, ist dieser Fehler akzeptabel.</li><li>Sie haben das Teilnetz-CIDR für den Cluster-Pod, <code>172.30.0.0/16</code>, nicht in der Liste <code>local.subnet</code> angegeben.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-ip-2</code></td>
    <td>Die private IP-Adresse <code>remote.privateIPtoPing</code> des lokalen VPN-Gateways im Unternehmen wird vom Workerknoten im Cluster mit einem Pingsignal überprüft. Dieser Test kann aus folgenden Gründen fehlschlagen:<ul><li>Sie haben keine IP-Adresse <code>remote.privateIPtoPing</code> angegeben. Wenn Sie absichtlich keine IP-Adresse angegeben haben, ist dieser Fehler akzeptabel.</li><li>Sie haben das private Teilnetz-CIDR für den Workernoten im Cluster nicht in der Liste <code>local.subnet</code> angegeben.</li></ul></td>
    </tr>
    </tbody></table>

6. Löschen Sie das Helm-Diagramm.

    ```
    helm delete --purge vpn
    ```
    {: pre}

7. Öffnen Sie die Datei `config.yaml` und korrigieren Sie die falschen Werte.

8. Speichern Sie die aktualisierte Datei `config.yaml`.

9. Installieren Sie das Helm-Diagramm auf Ihrem Cluster mit der aktualisierten Datei `config.yaml`. Die aktualisierten Eigenschaften werden in einer Konfigurationszuordnung für Ihr Diagramm gespeichert.

    ```
    helm install -f config.yaml --name=<releasename> ibm/strongswan
    ```
    {: pre}

10. Prüfen Sie den Status der Diagrammbereitstellung. Sobald das Diagramm bereit ist, hat das Feld **STATUS** oben in der Ausgabe den Wert `DEPLOYED`.

    ```
    helm status vpn
    ```
    {: pre}

11. Sobald das Diagramm bereitgestellt ist, überprüfen Sie, dass die aktualisierten Einstellungen in der Datei `config.yaml` verwendet wurden.

    ```
    helm get values vpn
    ```
    {: pre}

12. Bereinigen Sie die aktuellen Testpods.

    ```
    kubectl get pods -a -l app=strongswan-test
    ```
    {: pre}

    ```
    kubectl delete pods -l app=strongswan-test
    ```
    {: pre}

13. Führen Sie die Tests erneut aus.

    ```
    helm test vpn
    ```
    {: pre}

<br />


## Aktualisieren Sie das StrongSwan-Helm-Diagramm
{: #vpn_upgrade}

Stellen Sie sicher, dass Ihr StrongSwan-Helm-Diagramm aktuell ist, indem Sie es aktualisieren.
{:shortdesc}

Gehen Sie wie folge vor, um das StrongSwan-Helm-Diagramm auf die neueste Version zu aktualisieren.

  ```
  helm upgrade -f config.yaml <releasename> ibm/strongswan
  ```
  {: pre}

**Wichtig**: Das strongSwan 2.0.0-Helm-Diagramm ist nicht mit Calico Version 3 oder Kubernetes Version 1.10 kompatibel. Bevor Sie [den Cluster auf Version 1.10 aktualisieren](cs_versions.html#cs_v110), aktualisieren Sie strongSwan auf das 2.1.0-Helm-Diagramm, das mit Calico Version 2.6 und Kubernetes Version 1.7, 1.8 und 1.9 abwärtskompatibel ist.


### Upgrade von Version 1.0.0 durchführen
{: #vpn_upgrade_1.0.0}

Aufgrund einiger Einstellungen, die in der Version 1.0.0 des Helm-Diagramms verwendet werden, können Sie `helm upgrade` nicht für eine Aktualisierung ´von Version 1.0.0 auf die neueste Version verwenden.
{:shortdesc}

Um ein Upgrade von Version 1.0.0 auszuführen, müssen Sie das Diagramm der Version 1.0.0 löschen und die neueste Version installieren.

1. Löschen Sie das Helm-Diagramm der Version 1.0.0

    ```
    helm delete --purge <releasename>
    ```
    {: pre}

2. Speichern Sie die Standardkonfigurationseinstellungen für das StrongSwan-Helm-Diagramm in einer lokalen YAML-Datei.

    ```
    helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. Aktualisieren Sie die Konfigurationsdatei, und speichern Sie die Datei mit den Änderungen.

4. Installieren Sie das Helm-Diagramm auf Ihrem Cluster mit der aktualisierten Datei `config.yaml`.

    ```
    helm install -f config.yaml --name=<releasename> ibm/strongswan
    ```
    {: pre}

Zusätzlich werden bestimmte `ipsec.conf`-Einstellungen für die Zeitlimitüberschreitung, die in Version 1.0.0 fest codiert waren, in nachfolgenden Versionen als konfigurierbare Eigenschaften bereitgestellt. Die Namen und Standardwerte einiger dieser konfigurierbaren `ipsec.conf`-Einstellungen für die Zeitlimitüberschreitung wurden ferner geändert, damit Sie mit den Standardwerten von StrongSwan besser harmonieren. Wenn Sie ein Upgrade für Ihr Helm-Diagramm von Version 1.0.0 durchführen und die Standardwerte der Version 1.0.0 für die Zeitlimitüberschreitung beibehalten möchten, fügen Sie die alten Standardwerte als neue Einstellungen zu Ihrer Diagrammkonfigurationsdatei hinzu.



  <table>
  <caption>Unterschiede der ipsec.conf-Einstellungen zwischen Version 1.0.0 und der neuesten Version</caption>
  <thead>
  <th>Einstellungsnamen in Version 1.0.0</th>
  <th>Standardwert in Version 1.0.0</th>
  <th>Einstellungsname in der neusten Version</th>
  <th>Standardwert in der neuesten Version</th>
  </thead>
  <tbody>
  <tr>
  <td><code>ikelifetime</code></td>
  <td>60m</td>
  <td><code>ikelifetime</code></td>
  <td>3h</td>
  </tr>
  <tr>
  <td><code>keylife</code></td>
  <td>20m</td>
  <td><code>lifetime</code></td>
  <td>1h</td>
  </tr>
  <tr>
  <td><code>rekeymargin</code></td>
  <td>3m</td>
  <td><code>margintime</code></td>
  <td>9m</td>
  </tr>
  </tbody></table>


## StrongSwan-IPSec-VPN-Service inaktivieren
{: vpn_disable}

Sie können die VPN-Verbindung inaktivieren, indem Sie das Helm-Diagramm löschen .
{:shortdesc}

  ```
  helm delete --purge <releasename>
  ```
  {: pre}

<br />


## VPN-Konnektivität mit einer Virtual Router Appliance (VRA) einrichten
{: #vyatta}

[Virtual Router Appliance (VRA)](/docs/infrastructure/virtual-router-appliance/about.html) stellt das aktuelle Vyatta 5600-Betriebssystem für x86-Bare-Metal-Server bereit. Sie können eine VRA-Instanz als VPN-Gateway verwenden, um eine sichere Verbindung zu einem lokalen Netz herzustellen.{:shortdesc}

Der gesamte öffentliche und private Netzverkehr, der in die Cluster-VLANs eintritt oder sie verlässt, wird über eine VRA geleitet. Sie können die VRA als VPN-Endpunkt einsetzen, um einen verschlüsselten IPSec-Tunnel zwischen Servern in IBM Cloud Infrastructure- (SoftLayer) und lokalen Ressourcen zu erstellen. Das folgende Diagramm zeigt beispielsweise, wie eine App auf einem privaten Workerknoten in {{site.data.keyword.containershort_notm}} mit einem lokalen Server über eine VRA-VPN-Verbindung kommunizieren kann: 

<img src="images/cs_vpn_vyatta.png" width="725" alt="App in {{site.data.keyword.containershort_notm}} mithilfe einer Lastausgleichsfunktion zugänglich machen" style="width:725px; border-style: none"/>

1. Eine App in Ihrem Cluster, `myapp2`, empfängt eine Anforderung von einem Ingress- oder LoadBalancer-Service und muss eine sichere Verbindung mit Daten in Ihrem lokalen Netz herstellen.

2. Da `myapp2` sich auf einem Workerknoten in einem rein privaten VLAN befindet, fungiert die VRA als sichere Verbindung zwischen den Workerknoten und dem lokalen Netz. Die VRA verwendet die IP-Zieladresse, um zu bestimmen, welche Netzpakete an das lokale Netz gesendet werden sollen.

3. Die Anforderung wird verschlüsselt und über den VPN-Tunnel an das lokale Rechenzentrum gesendet.

4. Die eingehende Anforderung passiert die lokale Firewall und wird an den VPN-Tunnelendpunkt (Router) zugestellt, wo sie entschlüsselt wird.

5. Der VPN-Tunnelendpunkt (Router) leitet die Anforderung abhängig von der in Schritt 2 angegebenen Ziel-IP-Adresse an den lokalen Server oder den Mainframe weiter. Die erforderlichen Daten werden auf dieselbe Weise über die VPN-Verbindung zurück an `myapp2` gesendet.

Führen Sie die folgenden Schritte aus, um eine Virtual Router Appliance einzurichten:

1. [Bestellen Sie eine Virtual Router Appliance (VRA)](/docs/infrastructure/virtual-router-appliance/getting-started.html).

2. [Konfigurieren Sie das private VLAN auf der VRA](/docs/infrastructure/virtual-router-appliance/manage-vlans.html).

3. Um eine VPN-Verbindung mithilfe der VRA zu aktivieren, [konfigurieren Sie VRRP auf der VRA](/docs/infrastructure/virtual-router-appliance/vrrp.html#high-availability-vpn-with-vrrp).
