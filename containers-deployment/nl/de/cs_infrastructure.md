---

copyright:
  years: 2014, 2018
lastupdated: "2018-4-20"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Zugriff auf das Portfolio von IBM Cloud Infrastructure (SoftLayer)
{: #infrastructure}

Um einen Kubernetes-Standardcluster zu erstellen, benötigen Sie Zugriff auf das Portfolio von IBM Cloud Infrastructure (SoftLayer). Dieser Zugriff wird benötigt, um bezahlte Infrastrukturressourcen, wie z. B. Workerknoten, portierbare öffentliche
IP-Adressen oder permanenten Speicher für Ihren Kubernetes-Cluster in {{site.data.keyword.containerlong}} anzufordern.
{:shortdesc}

## Auf das Portfolio von IBM Cloud Infrastructure (SoftLayer) zugreifen
{: #unify_accounts}

Nutzungsabhängige {{site.data.keyword.Bluemix_notm}}-Konten, die erstellt wurden, nachdem die automatische Kontoverknüpfung aktiviert wurde, haben bereits Zugriff auf das Portfolio von IBM Cloud Infrastructure (SoftLayer). Somit können Sie Infrastrukturressourcen für Ihren Cluster ohne weitere Konfiguration kaufen.
{:shortdesc}

Benutzer mit anderen {{site.data.keyword.Bluemix_notm}}-Kontotypen oder Benutzer, die bereits ein Konto von IBM Cloud Infrastructure (SoftLayer) haben, das mit dem {{site.data.keyword.Bluemix_notm}}-Konto noch nicht verknüpft ist, müssen ihre Konten konfigurieren, um Standardcluster zu erstellen.
{:shortdesc}

Sehen Sie sich die folgende Tabelle an, um die verfügbaren Optionen für die einzelnen Kontotypen zu finden.

|Kontotyp|Beschreibung|Verfügbare Optionen zum Erstellen eines Standardclusters|
|------------|-----------|----------------------------------------------|
|Lite-Konten|Lite-Konten können keine Cluster bereitstellen.|[Aktualisieren Sie Ihr Lite-Konto auf ein nutzungsabhängiges {{site.data.keyword.Bluemix_notm}}-Konto](/docs/account/index.html#billableacts), das mit Zugriff auf das Portfolio von IBM Cloud Infrastructure (SoftLayer) eingerichtet ist.|
|Ältere nutzungsabhängige Konten|Nutzungsabhängige Konten, die erstellt wurden, bevor die automatische Kontoverknüpfung verfügbar war, haben keinen Zugriff auf das Portfolio von IBM Cloud Infrastructure (SoftLayer).<p>Wenn Sie ein Konto von IBM Cloud Infrastructure (SoftLayer) haben, können Sie dieses Konto nicht mit einem älteren nutzungsabhängigen Konto verknüpfen.</p>|Option 1: [Erstellen Sie ein neues nutzungsabhängiges Konto](/docs/account/index.html#billableacts), das mit Zugriff auf das Portfolio von IBM Cloud Infrastructure (SoftLayer) eingerichtet wird. Wenn Sie diese Option auswählen, erhalten Sie zwei separate {{site.data.keyword.Bluemix_notm}}-Konten und -Rechnungen.<p>Wenn Sie Ihr altes nutzungsabhängiges Konto weiterhin verwenden möchten, um Standardcluster zu erstellen, können Sie mit Ihrem neuen nutzungsabhängigen Konto einen API-Schlüssel generieren. Mit diesem API-Schlüssel greifen Sie auf das Portfolio von IBM Cloud Infrastructure (SoftLayer) zu. Sie müssen nur den API-Schlüssel für Ihr altes nutzungsabhängiges Konto einrichten. Weitere Informationen finden Sie unter
[Einen API-Schlüssel für alte nutzungsabhängige Konten und Abonnementkonten generieren](#old_account). Beachten Sie, dass Ressourcen von IBM Cloud Infrastructure (SoftLayer) über Ihr neues nutzungsabhängiges Konto in Rechnung gestellt werden.</p></br><p>Option 2: Wenn Sie bereits ein Konto von IBM Cloud Infrastructure (SoftLayer) haben, das Sie verwenden möchten, können Sie [Ihre Berechtigungsnachweise](cs_cli_reference.html#cs_credentials_set) für Ihr {{site.data.keyword.Bluemix_notm}}-Konto festlegen.</p><p>**Hinweis:** Das Konto von IBM Cloud Infrastructure (SoftLayer), das Sie mit Ihrem {{site.data.keyword.Bluemix_notm}}-Konto verwenden, muss mit Superuser-Berechtigungen konfiguriert sein.</p>|
|Abonnementkonten|Abonnementkonten werden ohne Zugriff auf das Portfolio von IBM Cloud Infrastructure (SoftLayer) eingerichtet.|Option 1: [Erstellen Sie ein neues nutzungsabhängiges Konto](/docs/account/index.html#billableacts), das mit Zugriff auf das Portfolio von IBM Cloud Infrastructure (SoftLayer) eingerichtet wird. Wenn Sie diese Option auswählen, erhalten Sie zwei separate {{site.data.keyword.Bluemix_notm}}-Konten und -Rechnungen.<p>Wenn Sie Ihr Abonnementkonto weiterhin verwenden möchten, um Standardcluster zu erstellen, können Sie mit Ihrem neuen nutzungsabhängigen Konto einen API-Schlüssel generieren. Mit diesem API-Schlüssel greifen Sie auf das Portfolio von IBM Cloud Infrastructure (SoftLayer) zu. Sie müssen nur den API-Schlüssel für Ihr Abonnementkonto einrichten. Weitere Informationen finden Sie unter
[Einen API-Schlüssel für alte nutzungsabhängige Konten und Abonnementkonten generieren](#old_account). Beachten Sie, dass Ressourcen von IBM Cloud Infrastructure (SoftLayer) über Ihr neues nutzungsabhängiges Konto in Rechnung gestellt werden.</p></br><p>Option 2: Wenn Sie bereits ein Konto von IBM Cloud Infrastructure (SoftLayer) haben, das Sie verwenden möchten, können Sie [Ihre Berechtigungsnachweise](cs_cli_reference.html#cs_credentials_set) für Ihr {{site.data.keyword.Bluemix_notm}}-Konto festlegen.<p>**Hinweis:** Das Konto von IBM Cloud Infrastructure (SoftLayer), das Sie mit Ihrem {{site.data.keyword.Bluemix_notm}}-Konto verwenden, muss mit Superuser-Berechtigungen konfiguriert sein.</p>|
|Konten von IBM Cloud Infrastructure (SoftLayer), kein {{site.data.keyword.Bluemix_notm}}-Konto|Um einen Standardcluster zu erstellen, müssen Sie ein {{site.data.keyword.Bluemix_notm}}-Konto haben.|<p>[Erstellen Sie ein neues nutzungsabhängiges Konto](/docs/account/index.html#billableacts), das mit Zugriff auf das Portfolio von IBM Cloud Infrastructure (SoftLayer) eingerichtet wird. Wenn Sie diese Option auswählen, wird ein Konto von IBM Cloud Infrastructure (SoftLayer) für Sie erstellt. Sie haben zwei separate Konten von IBM Cloud Infrastructure (SoftLayer) und eine jeweils separate Abrechnung.</p>|

<br />


## API-Schlüssel für IBM Cloud Infrastructure (SoftLayer) zur Verwendung mit {{site.data.keyword.Bluemix_notm}}-Konten generieren
{: #old_account}

Um Ihr altes nutzungsabhängiges Konto oder Ihr Abonnementkonto weiterhin zu verwenden, um Standardcluster zu erstellen, generieren Sie
einen API-Schlüssel für Ihr neues nutzungsabhängiges Konto. Legen Sie dann den API-Schlüssel für Ihr altes Konto fest.
{:shortdesc}

Erstellen Sie zunächst ein nutzungsabhängiges {{site.data.keyword.Bluemix_notm}}-Konto, das automatisch mit Zugriff auf das Portfolio vom IBM Cloud Infrastructure (SoftLayer) eingerichtet wird.

1.  Melden Sie sich beim [Portal von IBM Cloud Infrastructure (SoftLayer) ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/) an, indem Sie die {{site.data.keyword.ibmid}} und das Kennwort benutzen, die Sie für Ihr neues nutzungsabhängiges Konto erstellt haben.
2.  Wählen Sie **Konto** und dann **Benutzer** aus.
3.  Klicken Sie auf **Generieren**, um einen API-Schlüssel für IBM Cloud Infrastructure (SoftLayer) für Ihr neues nutzungsabhängiges Konto zu generieren.
4.  Kopieren Sie den API-Schlüssel.
5.  Melden Sie sich über die Befehlszeilenschnittstelle (CLI) bei {{site.data.keyword.Bluemix_notm}} an, indem
Sie die {{site.data.keyword.ibmid}} und das Kennwort für Ihr altes nutzungsabhängiges Konto oder Abonnementkonto verwenden.

  ```
  bx login
  ```
  {: pre}

6.  Legen Sie den API-Schlüssel fest, den Sie zuvor generiert haben, um auf das Portfolio von IBM Cloud Infrastructure (SoftLayer) zuzugreifen. Ersetzen Sie `<API_key>` durch den API-Schlüssel und `<username>` durch die {{site.data.keyword.ibmid}} Ihres neuen nutzungsabhängigen Kontos.

  ```
  bx cs credentials-set --infrastructure-api-key <api-schlüssel> --infrastructure-username <benutzername>
  ```
  {: pre}

7.  [Erstellen Sie jetzt Standardcluster](cs_clusters.html#clusters_cli).

**Hinweis:** Nachdem Sie Ihren API-Schlüssel generiert haben, überprüfen Sie ihn. Befolgen Sie dazu Schritt 1 und 2. Klicken Sie dann im Abschnitt
**API-Schlüssel** auf **Anzeigen**, um den API-Schlüssel für Ihre Benutzer-ID anzuzeigen.

