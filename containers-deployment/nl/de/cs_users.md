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



# Clusterzugriff zuweisen
{: #users}

Als Clusteradministrator können Sie Zugriffsrichtlinien für Ihren Kubernetes-Cluster definieren, um verschiedene Zugriffsebenen für unterschiedliche Benutzer zu erstellen. Sie können beispielsweise bestimmte Benutzer autorisieren, mit Clusterressourcen zu arbeiten, während andere Benutzer nur Container bereitstellen können.
{: shortdesc}


## Zugriffsanforderungen planen
{: #planning_access}

Für Clusteradministratoren kann es eine Herausforderung sein, Zugriffsanforderungen zu überwachen. Die Einrichtung eines Kommunikationsmusters für Zugriffsanforderungen ist für die Aufrechterhaltung der Sicherheit Ihres Clusters von entscheidender Bedeutung.
{: shortdesc}

Um sicherzustellen, dass die richtigen Personen den richtigen Zugriff haben, müssen Sie Ihre Richtlinien für das Anfordern von Zugriff oder das Einholen von Hilfe für gängige Tasks klar kommunizieren.

Vielleicht haben Sie schon eine Strategie implementiert, die für Ihr Team funktioniert. Das ist hervorragend. Wenn Sie nocht nicht genau wissen, wo Sie beginnen sollen, können Sie eine der folgenden Methoden ausprobieren.

*  Ticketsystem erstellen
*  Formularvorlage erstellen
*  Wikiseite erstellen
*  E-Mail-Anforderung verlangen
*  Problemverfolgungssystem verwenden, das Sie bereits nutzen, um die tägliche Arbeit Ihres Teams zu überwachen

Sind Sie überfordert? Testen Sie dieses Lernprogramm zu den [Best Practices für die Organisation von Benutzern, Teams und Anwendungen](/docs/tutorials/users-teams-applications.html).
{: tip}

## Zugriffsrichtlinien und -berechtigungen
{: #access_policies}

Der Geltungsbereich einer Zugriffsrichtlinie basiert auf benutzerdefinierten Rollen, die die Aktionen bestimmen, die sie ausführen dürfen. Sie können Richtlinien festlegen, die für Ihren Cluster, Ihre Infrastruktur, Ihre Serviceinstanzen oder Cloud Foundry-Rollen spezifisch sind.
{: shortdesc}

Sie müssen Zugriffsrichtlinien für jeden Benutzer definieren, der mit {{site.data.keyword.containershort_notm}} arbeitet. Einige Richtlinien sind vordefiniert, andere können jedoch angepasst werden. Sehen Sie sich die folgende Abbildung und die Definitionen an, um herauszufinden, welche Rollen auf gängige Benutzertasks abgestimmt sind, und um zu erkennen, an welchen Stellen Sie eine Richtlinie möglicherweise anpassen möchten.

![{{site.data.keyword.containershort_notm}}-Zugriffsrollen](/images/user-policies.png)

Abbildung. {{site.data.keyword.containershort_notm}}-Zugriffsrollen

<dl>
  <dt>IAM-Richtlinien (Identity and Access Management)</dt>
    <dd><p><strong>Plattform</strong>: Sie können bestimmen, welche Aktionen Einzelpersonen in einem {{site.data.keyword.containershort_notm}}-Cluster ausführen können. Sie können diese Richtlinien nach Region festlegen. Beispiele für Aktionen sind das Erstellen oder Entfernen von Clustern oder das Hinzufügen zusätzlicher Workerknoten. Diese Richtlinien müssen in Verbindung mit Infrastrukturrichtlinien gesetzt werden.</p>
    <p><strong>Infrastruktur</strong>: Sie können die Zugriffsebenen für Ihre Infrastruktur, z. B. Clusterknotenmaschinen, Netz oder Speicherressourcen, bestimmen. Es wird dieselbe Richtlinie umgesetzt, unabhängig davon, ob der Benutzer eine Anforderung über die {{site.data.keyword.containershort_notm}}-GUI oder über die CLI startet. Dies gilt auch dann, wenn die Aktionen in IBM Cloud Infrastructure (SoftLayer) ausgeführt werden. Dieser Typ von Richtlinie muss gemeinsam mit {{site.data.keyword.containershort_notm}}-Plattformzugriffsrichtlinien gesetzt werden. Wenn Sie mehr über die verfügbaren Rollen erfahren möchten, lesen Sie die Informationen zu den [Infrastrukturberechtigungen](/docs/iam/infrastructureaccess.html#infrapermission).</p> </br></br><strong>Hinweis:</strong> Stellen Sie sicher, dass Ihr {{site.data.keyword.Bluemix_notm}}-Konto [mit Zugriff auf das Portfolio von IBM Cloud Infrastructure (SoftLayer) eingerichtet ist](cs_troubleshoot_clusters.html#cs_credentials), damit berechtigte Benutzer basierend auf den zugewiesenen Berechtigungen entsprechende Aktionen im Konto von IBM Cloud Infrastructure (SoftLayer) durchführen können. </dd>
  <dt>RBAC-Rollen (Resource Based Access Control) von Kubernetes</dt>
    <dd>Jedem Benutzer, dem eine Plattformzugriffsrichtlinie zugewiesen ist, ist automatisch auch eine Kubernetes-Rolle zugewiesen. In Kubernetes bestimmt die rollenbasierte Zugriffssteuerung ([Role Based Access Control, RBAC ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview), welche Aktionen ein Benutzer für die Ressourcen in einem Cluster ausführen kann. <strong>Hinweis</strong>: RBAC-Rollen werden automatisch für den Namensbereich <code>default</code> konfiguriert, aber als Clusteradministrator können Sie anderen Namensbereichen Rollen zuweisen.</dd>
  <dt>Cloud Foundry</dt>
    <dd>Nicht alle Services können mit Cloud IAM verwaltet werden. Wenn Sie einen dieser Services nutzen, können Sie weiterhin die [Cloud Foundry-Benutzerrollen](/docs/iam/cfaccess.html#cfaccess) verwenden, um den Zugriff auf Ihre Services zu steuern.</dd>
</dl>


Laden Sie gerade Berechtigungen herunter? Dies kann ein paar Minuten dauern.
{: tip}

### Plattformrollen
{: #platform_roles}

{{site.data.keyword.containershort_notm}} ist für die Verwendung von {{site.data.keyword.Bluemix_notm}}-Plattformrollen konfiguriert. Die Rollenberechtigungen bauen aufeinander auf, d. h. die Rolle `Editor` (Bearbeiter) verfügt über dieselben Berechtigungen wie die Rolle `Viewer` (Anzeigeberechtigter) und zusätzlich über die Berechtigungen, die einem Bearbeiter gewährt werden. In der folgenden Tabelle werden die Typen von Aktionen erläutert, die jede Rolle ausführen kann.

Beim Zuweisen einer Plattformrolle werden RBAC-Rollen automatisch dem Standardnamensbereich zugeordnet. Wenn Sie die Plattformrolle eines Benutzers ändern, wird auch die RBAC-Rolle entsprechend aktualisiert.
{: tip}

<table>
<caption>Plattformrollen und Aktionen</caption>
  <tr>
    <th>Plattformrollen</th>
    <th>Beispielaktionen</th>
    <th>Entsprechende RBAC-Rolle</th>
  </tr>
  <tr>
      <td>Viewer</td>
      <td>Anzeigen von Details für einen Cluster oder andere Serviceinstanzen</td>
      <td>Anzeigen</td>
  </tr>
  <tr>
    <td>Editor</td>
    <td>Binden eines IBM Cloud-Service an einen Cluster bzw. Aufheben der Bindung oder Erstellen eines Webhooks. <strong>Hinweis</strong>: Um Services zu binden, muss Ihnen auch die Cloud Foundry-Rolle des Entwicklers zugewiesen sein.</td>
    <td>Bearbeiten</td>
  </tr>
  <tr>
    <td>Operator</td>
    <td>Erstellen, Entfernen, Neustarten oder Neuladen eines Workerknotens. Hinzufügen eines Teilnetzes zu einem Cluster.</td>
    <td>Administrator</td>
  </tr>
  <tr>
    <td>Administrator</td>
    <td>Erstellen und Entfernen von Clustern. Bearbeiten von Zugriffsrichtlinien für andere Benutzer auf Kontoebene für den Service und die Infrastruktur. <strong>Hinweis</strong>: Der Administratorzugriff kann entweder für einen bestimmten Cluster oder für alle Instanzen des Services im gesamten Konto zugewiesen sein. Um Cluster zu löschen, müssen Sie über Administratorzugriff für den entsprechenden Cluster verfügen. Zum Erstellen von Clustern muss Ihnen die Administratorrolle für alle Instanzen des Service zugewiesen sein.</td>
    <td>Clusteradministrator</td>
  </tr>
</table>

Weitere Informationen zum Zuweisen von Benutzerrollen in der Benutzerschnittstelle finden Sie unter [IAM-Zugriff verwalten](/docs/iam/mngiam.html#iammanidaccser).


### Infrastrukturrollen
{: #infrastructure_roles}

Mit Infrastrukturrollen können Benutzer Tasks für Ressourcen auf der Infrastrukturebene ausführen. In der folgenden Tabelle werden die Typen von Aktionen erläutert, die jede Rolle ausführen kann. Infrastrukturrollen sind anpassbar. Stellen Sie sicher, dass die Benutzer nur den Zugriff haben, den sie benötigen, um ihren Job zu erledigen.

Neben bestimmten Infrastrukturrollen müssen Sie auch Benutzern Gerätezugriff erteilen, die mit der Infrastruktur arbeiten.
{: tip}

<table>
<caption>Infrastrukturrollen und Aktionen</caption>
  <tr>
    <th>Infrastrukturrolle</th>
    <th>Beispielaktionen</th>
  </tr>
  <tr>
    <td><i>Nur anzeigen</i></td>
    <td>Anzeigen von Infrastrukturdetails einer Kontozusammenfassung, einschließlich Rechnungen und Zahlungen.</td>
  </tr>
  <tr>
    <td><i>Basisbenutzer</i></td>
    <td>Bearbeiten von Servicekonfigurationen, einschließlich IP-Adressen, Hinzufügen oder Bearbeiten von DNS-Datensätzen und Hinzufügen neuer Benutzer mit Zugriff auf die Infrastruktur.</td>
  </tr>
  <tr>
    <td><i>Superuser</i></td>
    <td>Ausführen aller Aktionen im Zusammenhang mit der Infrastruktur.</td>
  </tr>
</table>

Führen Sie die folgenden Schritte unter [Infrastrukturberechtigungen für einen Benutzer anpassen](#infra_access) aus, um mit dem Zuweisen von Rollen zu beginnen.

### RBAC-Rollen
{: #rbac_roles}

Die ressourcenbasierte Zugriffsteuerung ist eine Methode, die Ressourcen in Ihrem Cluster zu sichern und zu entscheiden, welche Personen welche Kubernetes-Aktionen ausführen können. In der folgenden Tabelle sind die Typen von RBAC-Rollen und die Typen von Aktionen, die Benutzer mit der jeweiligen Rolle ausführen können, aufgeführt. Die Berechtigungen bauen aufeinander auf, d. h. ein `Administrator` verfügt über alle Berechtigungen, die mit den Rollen `Anzeigen` und `Bearbeiten` einhergehen. Stellen Sie sicher, dass Benutzer nur den Zugriff haben, den sie benötigen.

RBAC-Rollen werden automatisch in Verbindung mit den Plattformrollen für den Standardnamensbereich festgelegt. [Sie können die Rolle aktualisieren oder Rollen für andere Namensbereiche zuweisen](#rbac).
{: tip}

<table>
<caption>RBAC-Rollen und Aktionen</caption>
  <tr>
    <th>RBAC-Rolle</th>
    <th>Beispielaktionen</th>
  </tr>
  <tr>
    <td>Anzeigen</td>
    <td>Anzeigen der Ressourcen im Standardnamensbereich. Anzeigeberechtigte Personen können keine geheimen Kubernetes-Schlüssel anzeigen. </td>
  </tr>
  <tr>
    <td>Bearbeiten</td>
    <td>Schreib-/Lesezugriff auf Ressourcen im Standardnamensbereich.</td>
  </tr>
  <tr>
    <td>Administrator</td>
    <td>Schreib-/Lesezugriff auf Ressourcen im Standardnamensbereich, aber nicht auf den Namensbereich selbst. Erstellen von Rollen in einem Namensbereich.</td>
  </tr>
  <tr>
    <td>Clusteradministrator</td>
    <td>Schreib-/Lesezugriff auf Ressourcen in allen Namensbereichen. Erstellen von Rollen in einem Namensbereich. Zugriff auf das Kubernetes-Dashboard. Erstellen einer Ingress-Ressource, die Apps öffentlich zugänglich macht.</td>
  </tr>
</table>

<br />


## Benutzer zu einem {{site.data.keyword.Bluemix_notm}}-Konto hinzufügen
{: #add_users}

Sie können Benutzer zu einem {{site.data.keyword.Bluemix_notm}}-Konto hinzufügen, um ihnen Zugriff auf Ihren Cluster zu gewähren.
{:shortdesc}

Zunächst müssen Sie sicherstellen, dass Ihnen die Cloud Foundry-Rolle `Manager` für ein {{site.data.keyword.Bluemix_notm}}-Konto zugewiesen wurde.

1.  [Fügen Sie den Benutzer zum Konto hinzu](../iam/iamuserinv.html#iamuserinv).
2.  Erweitern Sie im Abschnitt **Zugriff** den Eintrag **Services**.
3.  Weisen Sie einem Benutzer eine Plattformrolle zu, um den Zugriff für {{site.data.keyword.containershort_notm}} festzulegen.
      1. Wählen Sie in der Dropdown-Liste **Services** den Eintrag **{{site.data.keyword.containershort_notm}}** aus.
      2. Wählen Sie aus der Dropdown-Liste **Region** die Region aus, der der Benutzer angehören soll.
      3. Wählen Sie aus der Dropdown-Liste **Serviceinstanz** den Cluster aus, dem der Benutzer angehören soll. Führen Sie `bx cs clusters` aus, um die ID eines bestimmten Clusters zu finden.
      4. Wählen Sie im Abschnitt **Rollen auswählen** eine Rolle aus. Eine Liste von unterstützten Aktionen pro Rolle finden Sie unter [Zugriffsrichtlinien und -berechtigungen](#access_policies).
4. [Optional: Weisen Sie eine Cloud Foundry-Rolle zu](/docs/iam/mngcf.html#mngcf).
5. [Optional: Weisen Sie eine Infrastrukturrolle zu](/docs/iam/infrastructureaccess.html#infrapermission).
6. Klicken Sie auf **Benutzer einladen**.

<br />


## Erklärung des IAM-API-Schlüssels und des Befehls `bx cs credentials-set`
{: #api_key}

Um erfolgreich Cluster bereitzustellen und mit diesen zu arbeiten, müssen Sie sicherstellen, dass Ihr Konto korrekt für den Zugriff auf das Portfolio von IBM Cloud Infrastructure (SoftLayer) konfiguriert wurde. Abhängig von der Konfiguration Ihres Kontos verwenden Sie entweder den IAM-API-Schlüssel oder die Berechtigungsnachweise der Infrastruktur, die Sie manuell mithilfe des Befehls `bx cs credentials-set` definiert haben.

<dl>
  <dt>IAM-API-Schlüssel</dt>
    <dd><p>Der IAM-API-Schlüssel (IAM, Identity and Access Management) wird automatisch für eine Region festgelegt, wenn die erste Aktion ausgeführt wird, für die die {{site.data.keyword.containershort_notm}}-Administratorzugriffsrichtlinie ausgeführt werden muss. Zum Beispiel erstellt einer Ihrer Benutzer mit Administratorberechtigung den ersten Cluster in der Region <code>Vereinigte Staaten (Süden)</code>. Dadurch wird der IAM-API-Schlüssel für diesen Benutzer in dem Konto für diese Region gespeichert. Der API-Schlüssel wird verwendet, um IBM Cloud Infrastructure (SoftLayer) wie beispielsweise neue Workerknoten oder VLANs zu bestellen.</p> <p>Wenn ein anderer Benutzer eine Aktion in dieser Region ausführt, die eine Interaktion mit dem Portfolio der IBM Cloud-Infrastruktur (SoftLayer) erfordert, wie z. B. die Erstellung eines neuen Clusters oder das erneute Laden eines Workerknotens, wird der gespeicherte API-Schlüssel verwendet, um festzustellen, ob ausreichende Berechtigungen vorhanden sind, um diese Aktion auszuführen. Um sicherzustellen, dass infrastrukturbezogene Aktionen in Ihrem Cluster erfolgreich ausgeführt werden können, weisen Sie Ihre {{site.data.keyword.containershort_notm}}-Benutzer mit Administratorberechtigung der Infrastrukturzugriffsrichtlinie <strong>Superuser</strong> zu.</p>
    <p>Sie können aktuelle Eigner von API-Schlüsseln finden, indem Sie den Befehl [<code>bx cs api-key-info</code>](cs_cli_reference.html#cs_api_key_info) ausführen. Wenn Sie feststellen, dass Sie den API-Schlüssel aktualisieren müssen, der für eine Region gespeichert ist, können Sie dies tun, indem Sie den Befehl [<code>bx cs api-key-reset</code>](cs_cli_reference.html#cs_api_key_reset) ausführen. Dieser Befehl erfordert die {{site.data.keyword.containershort_notm}}-Administratorzugriffsrichtlinie und speichert den API-Schlüssel des Benutzers, der diesen Befehl ausführt, im Konto.</p>
    <p><strong>Hinweis:</strong> Der API-Schlüssel, der für die Region gespeichert ist, wird möglicherweise nicht verwendet, falls die Berechtigungsnachweise für die IBM Cloud-Infrastruktur (SoftLayer) manuell durch Verwendung des Befehls <code>bx cs credentials-set</code> festgelegt wurden.</p></dd>
  <dt>Berechtigungsnachweise für IBM Cloud Infrastructure (SoftLayer) über <code>bx cs credentials-set</code></dt>
    <dd><p>Wenn Sie über ein nutzungsabhängiges {{site.data.keyword.Bluemix_notm}}-Konto verfügen, haben Sie standardmäßig Zugriff auf das Portfolio der IBM Cloud-Infrastruktur. Es kann jedoch sein, dass Sie ein anderes IBM Cloud-Infrastrukturkonto (SoftLayer) verwenden möchten, das Sie bereits für die Infrastrukturbestellung verwenden. Sie können dieses Infrastrukturkonto mit Ihrem {{site.data.keyword.Bluemix_notm}}-Konto verbinden, indem Sie den Befehl [<code>bx cs credentials-set</code>](cs_cli_reference.html#cs_credentials_set) verwenden.</p>
    <p>Falls Berechtigungsnachweise für IBM Cloud Infrastructure (SoftLayer) manuell definiert wurden, werden diese Berechtigungsnachweise für die Bestellung von Infrastruktur verwendet, selbst wenn ein IAM-API-Schlüssel für das Konto vorhanden ist. Wenn der Benutzer, dessen Berechtigungsnachweise gespeichert wurden, nicht über die erforderlichen Berechtigungen zum Bestellen von Infrastruktur verfügt, können infrastrukturbezogene Aktionen, wie z. B. das Erstellen eines Clusters oder das erneute Laden eines Workerknotens, fehlschlagen.</p>
    <p>Um Berechtigungsnachweise für IBM Cloud Infrastructure (SoftLayer) zu entfernen, die manuell definiert wurden, können Sie den Befehl [<code>bx cs credentials-unset</code>](cs_cli_reference.html#cs_credentials_unset) verwenden. Nachdem die Berechtigungsnachweise entfernt wurden, wird der IAM-API-Schlüssel verwendet, um Infrastruktur zu bestellen.</p></dd>
</dl>

<br />


## Infrastrukturberechtigungen für einen Benutzer anpassen
{: #infra_access}

Wenn Sie Infrastrukturrichtlinien in IAM (Identity and Access Management) konfigurieren, werden einem Benutzer mit einer Rolle verknüpfte Berechtigungen zugewiesen. Um diese Berechtigungen anzupassen, müssen Sie sich bei IBM Cloud Infrastructure (SoftLayer) anmelden und die Berechtigungen dort anpassen.
{: #view_access}

**Basisbenutzer** können beispielsweise einen Workerknoten neu starten, ihn aber nicht neu laden. Ohne diesen Benutzern **Superuser**-Berechtigungen zuzuweisen, können Sie die Berechtigungen der IBM Cloud Infrastructure (SoftLayer) anpassen und die Berechtigung zum Ausführen eines Neuladen-Befehls hinzufügen.

1.  Melden Sie sich bei Ihrem [{{site.data.keyword.Bluemix_notm}}-Konto](https://console.bluemix.net/) an und wählen Sie dann im Menü **Infrastruktur** aus.

2.  Wählen Sie **Konto** > **Benutzer** > **Benutzerliste** aus.

3.  Um die Berechtigungen zu ändern, wählen Sie den Namen des Benutzerprofils oder die Spalte **Gerätezugriff** aus.

4.  Passen Sie auf der Registerkarte **Portalberechtigungen** den Benutzerzugriff an. Die Berechtigungen, die die Benutzer benötigen, sind davon abhängig, welche Infrastrukturressourcen sie verwenden müssen:

    * Verwenden Sie die Dropdown-Liste **Schnellberechtigungen**, um die Rolle **Superuser** zuzuweisen, die dem Benutzer alle Berechtigungen erteilt.
    * Verwenden Sie die Dropdown-Liste **Schnellberechtigungen**, um die Rolle **Basisbenutzer** zuzuweisen, die dem Benutzer einige aber nicht alle benötigten Berechtigungen erteilt.
    * Wenn Sie nicht alle Berechtigungen mit der Rolle **Superuser** erteilen möchten oder wenn Sie weitere Berechtigungen über die Rolle **Basisbenutzer** hinaus erteilen möchten, lesen Sie in der folgenden Tabelle nach. Sie beschreibt die erforderlichen Berechtigungen zum Ausführen allgemeiner Tasks in {{site.data.keyword.containershort_notm}}.

    <table summary="Infrastrukturberechtigungen für allgemeine {{site.data.keyword.containershort_notm}}-Szenarios.">
     <caption>Allgemein erforderliche Infrastrukturberechtigungen für {{site.data.keyword.containershort_notm}}</caption>
     <thead>
      <th>Allgemeine Tasks in {{site.data.keyword.containershort_notm}}</th>
      <th>Erforderliche Infrastrukturberechtigungen nach Registerkarte</th>
     </thead>
     <tbody>
       <tr>
         <td><strong>Mindestberechtigungen</strong>: <ul><li>Erstellen Sie einen Cluster.</li></ul></td>
         <td><strong>Geräte</strong>:<ul><li>Details zum virtuellen Server anzeigen</li><li>Server neu starten und IPMI-Systeminformationen anzeigen</li><li>Befehle zum erneuten Laden des Betriebssystems absetzen und Rescue-Kernel initiieren</li></ul><strong>Konto</strong>: <ul><li>Cloudinstanzen hinzufügen/aktualisieren</li><li>Server hinzufügen</li></ul></td>
       </tr>
       <tr>
         <td><strong>Clusterverwaltung</strong>: <ul><li>Cluster erstellen, aktualisieren und löschen.</li><li>Workerknoten hinzufügen, erneut laden und neu starten.</li><li>VLANs anzeigen.</li><li>Teilnetze erstellen-</li><li>Pods und Lastausgleichsservices  bereitstellen.</li></ul></td>
         <td><strong>Support</strong>:<ul><li>Tickets anzeigen</li><li>Tickets hinzufügen</li><li>Tickets bearbeiten</li></ul>
         <strong>Geräte</strong>:<ul><li>Details zum virtuellen Server anzeigen</li><li>Server neu starten und IPMI-Systeminformationen anzeigen</li><li>Upgrade für Server durchführen</li><li>Befehle zum erneuten Laden des Betriebssystems absetzen und Rescue-Kernel initiieren</li></ul>
         <strong>Services</strong>:<ul><li>SSH-Schlüssel verwalten</li></ul>
         <strong>Konto</strong>:<ul><li>Kontozusammenfassung anzeigen</li><li>Cloudinstanzen hinzufügen/aktualisieren</li><li>Server abbrechen</li><li>Server hinzufügen</li></ul></td>
       </tr>
       <tr>
         <td><strong>Speicher</strong>: <ul><li>Persistente Datenträgeranforderungen zur Bereitstellung von persistenten Datenträgern erstellen</li><li>Speicherinfrastrukturressourcen erstellen und verwalten.</li></ul></td>
         <td><strong>Services</strong>:<ul><li>Speicher verwalten</li></ul><strong>Konto</strong>:<ul><li>Speicher hinzufügen</li></ul></td>
       </tr>
       <tr>
         <td><strong>Privates Netz</strong>: <ul><li>Private VLANs für Netzbetrieb im Cluster verwalten</li><li>VPN-Konnektivität für private Netze konfigurieren</li></ul></td>
         <td><strong>Netz</strong>:<ul><li>Teilnetzrouten im Netz verwalten</li><li>IPSEC-Netztunnel verwalten</li><li>Netzgateways verwalten</li><li>VPN-Verwaltung</li></ul></td>
       </tr>
       <tr>
         <td><strong>Öffentliches Netz</strong>:<ul><li>Richten Sie eine öffentliche Lastausgleichsfunktion oder das Ingress-Networking ein, um Apps bereitzustellen.</li></ul></td>
         <td><strong>Geräte</strong>:<ul><li>Lastausgleichsfunktionen verwalten</li><li>Hostname/Domäne bearbeiten</li><li>Portsteuerung verwalten</li></ul>
         <strong>Netz</strong>:<ul><li>Berechnen mit öffentlichem Netzport hinzufügen</li><li>Teilnetzrouten im Netz verwalten</li><li>IP-Adressen hinzufügen</li></ul>
         <strong>Services</strong>:<ul><li>DNS, Reverse DNS und WHOIS verwalten</li><li>Zertifikate anzeigen (SSL)</li><li>Zertifikate verwalten (SSL)</li></ul></td>
       </tr>
     </tbody>
    </table>

5.  Klicken Sie zum Speichern Ihrer Änderungen auf **Portalberechtigungen bearbeiten**.

6.  Wählen Sie auf der Registerkarte **Gerätezugriff** die Geräte aus, für die Zugriff erteilt werden soll.

    * In der Dropdown-Liste **Gerätetyp** können Sie Zugriff auf **Alle virtuellen Server** erteilen.
    * Um Benutzern Zugriff auf neue Geräte zu erteilen, die erstellt wurden, markieren Sie **Der Zugriff wird automatisch erteilt, wenn neue Geräte hinzugefügt werden**.
    * Klicken Sie zum Speichern Ihrer Änderungen auf **Gerätezugriff aktualisieren**.

<br />


## Benutzer mit angepassten Kubernetes-RBAC-Rollen berechtigen
{: #rbac}

{{site.data.keyword.containershort_notm}}-Zugriffsrichtlinien entsprechen bestimmten rollenbasierten Kubernetes-RBAC-Rollen (RBAC; Role-Based Access Control), die  in [Zugriffsrichtlinien und Berechtigungen](#access_policies) beschrieben werden. Wenn Sie andere Kubernetes-Rollen berechtigen möchten, die sich von der entsprechenden Zugriffsrichtlinie unterscheiden, können Sie RBAC-Rollen anpassen und die Rollen dann Einzelpersonen oder Benutzergruppen zuweisen.
{: shortdesc}

Beispiel: Sie möchten einem Team von Entwicklern die Berechtigung erteilen, an einer bestimmten API-Gruppe oder mit Ressourcen innerhalb eines Kubernetes-Namensbereichs im Cluster zu arbeiten, aber nicht über den gesamten Cluster. Sie erstellen eine Rolle und binden dann die Rolle an Benutzer, indem Sie einen Benutzernamen verwenden, der für {{site.data.keyword.containershort_notm}} eindeutig ist. Weitere Informationen finden Sie unter [Verwendung von RBAC-Autorisierung ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview) in der Kubernetes-Dokumentation.

Führen Sie zunächst folgende Schritte aus. [Richten Sie die Kubernetes-CLI auf Ihren Cluster aus](cs_cli_install.html#cs_cli_configure).

1.  Erstellen Sie die Rolle mit dem Zugriff, den Sie erteilen möchten.

    1. Erstellen Sie eine `.yaml`-Datei, um die Rolle mit dem Zugriff zu definieren, den Sie erteilen möchten.

        ```
        kind: Role
        apiVersion: rbac.authorization.k8s.io/v1
        metadata:
          namespace: default
          name: my_role
        rules:
        - apiGroups: [""]
          resources: ["pods"]
          verbs: ["get", "watch", "list"]
        - apiGroups: ["apps", "extensions"]
          resources: ["daemonsets", "deployments"]
          verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
        ```
        {: codeblock}

        <table>
        <caption>Erklärung dieser YAML-Komponenten</caption>
          <thead>
            <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung dieser YAML-Komponenten</th>
          </thead>
          <tbody>
            <tr>
              <td><code>kind</code></td>
              <td>Verwenden Sie `Role`, um Zugriff auf die Ressourcen in einem einzelnen Namensbereich zu erteilen, oder `ClusterRole` für clusterweite Ressourcen.</td>
            </tr>
            <tr>
              <td><code>apiVersion</code></td>
              <td><ul><li>Für Cluster, die Kubernetes 1.8 oder höher ausführen, verwenden Sie `rbac.authorization.k8s.io/v1`. </li><li>Für ältere Versionen verwenden Sie `apiVersion: rbac.authorization.k8s.io/v1beta1`.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/namespace</code></td>
              <td><ul><li>Für `Role` kind: Geben Sie den Kubernetes-Namensbereich an, für den der Zugriff erteilt wird.</li><li>Verwenden Sie das Feld `namespace` nicht, wenn Sie eine Clusterrolle erstellen (`ClusterRole`), die auf Clusterebene gilt.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/name</code></td>
              <td>Benennen Sie die Rolle und verwenden Sie den Namen später, wenn Sie die Rolle binden.</td>
            </tr>
            <tr>
              <td><code>rules/apiGroups</code></td>
              <td><ul><li>Geben Sie die Kubernetes-API-Gruppen an, mit denen Benutzer interagieren können sollen. Zum Beispiel `"apps"`, `"batch"` oder `"extensions"`. </li><li>Für den Zugriff auf die Gruppe der API-Kerndefinitionen im REST-Pfad `api/v1` lassen Sie die Angabe für die Gruppe leer `[""]`.</li><li>Weitere Informationen finden Sie unter [API-Gruppen![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://v1-9.docs.kubernetes.io/docs/reference/api-overview/#api-groups) in der Kubernetes-Dokumentation.</li></ul></td>
            </tr>
            <tr>
              <td><code>rules/resources</code></td>
              <td><ul><li>Geben Sie die Kubernetes-Ressourcen an, auf die Sie Zugriff erteilen möchten, z. B. `"daemonsets"`, `"deployments"`, `"events"` oder `"ingresses"`.</li><li>Falls Sie `"nodes"` angeben, muss als Rollentyp `ClusterRole` angegeben werden.</li><li>Eine Liste der Ressourcen finden Sie in der Tabelle der [Ressourcentypen![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/reference/kubectl/cheatsheet/) auf dem Kubernetes-Spickzettel.</li></ul></td>
            </tr>
            <tr>
              <td><code>rules/verbs</code></td>
              <td><ul><li>Geben Sie die Typen von Aktionen an, die Benutzer ausführen können sollen, z. B. `"get"`, `"list"`, `"describe"`, `"create"` oder `"delete"`. </li><li>Eine vollständige Liste der Verben finden Sie in der Dokumentation unter [`kubectl`![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/reference/kubectl/overview/).</li></ul></td>
            </tr>
          </tbody>
        </table>

    2.  Erstellen Sie die Rolle in Ihrem Cluster.

        ```
        kubectl apply -f <path_to_yaml_file>
        ```
        {: pre}

    3.  Überprüfen Sie, dass die Rolle erstellt wurde.

        ```
        kubectl get roles
        ```
        {: pre}

2.  Binden Sie Benutzer an die Rolle.

    1. Erstellen Sie eine `.yaml`-Datei, um Benutzer an Ihre Rolle zu binden. Notieren Sie die eindeutige URL, die für die einzelnen Namen des jeweiligen Subjekts verwendet werden soll

        ```
        kind: RoleBinding
        apiVersion: rbac.authorization.k8s.io/v1
        metadata:
          name: my_role_team1
          namespace: default
        subjects:
        - kind: User
          name: https://iam.ng.bluemix.net/kubernetes#user1@example.com
          apiGroup: rbac.authorization.k8s.io
        - kind: User
          name: https://iam.ng.bluemix.net/kubernetes#user2@example.com
          apiGroup: rbac.authorization.k8s.io
        roleRef:
          kind: Role
          name: custom-rbac-test
          apiGroup: rbac.authorization.k8s.io
        ```
        {: codeblock}

        <table>
        <caption>Erklärung dieser YAML-Komponenten</caption>
          <thead>
            <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung dieser YAML-Komponenten</th>
          </thead>
          <tbody>
            <tr>
              <td><code>kind</code></td>
              <td>Geben Sie für `kind` für beide Typen der `.yaml`-Rollendateien `RoleBinding` an: Namensbereich - `Role` und clusterweit - `ClusterRole`.</td>
            </tr>
            <tr>
              <td><code>apiVersion</code></td>
              <td><ul><li>Für Cluster, die Kubernetes 1.8 oder höher ausführen, verwenden Sie `rbac.authorization.k8s.io/v1`. </li><li>Für ältere Versionen verwenden Sie `apiVersion: rbac.authorization.k8s.io/v1beta1`.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/namespace</code></td>
              <td><ul><li>Für `Role` kind: Geben Sie den Kubernetes-Namensbereich an, für den der Zugriff erteilt wird.</li><li>Verwenden Sie das Feld `namespace` nicht, wenn Sie eine Clusterrolle erstellen (`ClusterRole`), die auf Clusterebene gilt.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/name</code></td>
              <td>Geben Sie der Rollenbindung einen Namen.</td>
            </tr>
            <tr>
              <td><code>subjects/kind</code></td>
              <td>Geben Sie für 'kind' `User` an.</td>
            </tr>
            <tr>
              <td><code>subjects/name</code></td>
              <td><ul><li>Hängen Sie die E-Mailadresse des Benutzers an die folgende URL an: `https://iam.ng.bluemix.net/kubernetes#`.</li><li>Beispiel: `https://iam.ng.bluemix.net/kubernetes#user1@example.com`</li></ul></td>
            </tr>
            <tr>
              <td><code>subjects/apiGroup</code></td>
              <td>Verwenden Sie `rbac.authorization.k8s.io`.</td>
            </tr>
            <tr>
              <td><code>roleRef/kind</code></td>
              <td>Geben Sie denselben Wert als `kind` in der `.yaml`-Rollendatei an: `Role` oder `ClusterRole`.</td>
            </tr>
            <tr>
              <td><code>roleRef/name</code></td>
              <td>Geben Sie den Namen der `.yaml`-Rollendatei an.</td>
            </tr>
            <tr>
              <td><code>roleRef/apiGroup</code></td>
              <td>Verwenden Sie `rbac.authorization.k8s.io`.</td>
            </tr>
          </tbody>
        </table>

    2. Erstellen Sie die Ressource für die Rollenbindung in Ihrem Cluster.

        ```
        kubectl apply -f filepath/my_role_team1.yaml
        ```
        {: pre}

    3.  Überprüfen Sie, dass die Bindung erstellt wurde.

        ```
        kubectl get rolebinding
        ```
        {: pre}

Jetzt, nachdem Sie eine angepasste Kubernetes-RBAC-Rolle erstellt und gebunden haben, fahren Sie mit den Benutzern fort. Bitten Sie sie, eine Aktion auszuführen, für deren Ausführung sie die Berechtigung aufgrund Ihrer Rolle haben. Zum Beispiel das Löschen eines Pods.
