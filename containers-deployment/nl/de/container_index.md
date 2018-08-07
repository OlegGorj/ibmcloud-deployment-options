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



# Einführung in {{site.data.keyword.containerlong_notm}}
{: #container_index}

Seien Sie sofort einsatzbereit mit {{site.data.keyword.containerlong}} durch die Bereitstellung hoch verfügbarer Apps in Docker-Containern, die in Kubernetes-Clustern ausgeführt werden.
{:shortdesc}

Ein Container ist ein Standardverfahren zum Packen von Apps mit den zugehörigen Abhängigkeiten, sodass die Apps nahtlos zwischen Umgebungen verlagert werden können. Im Unterschied zu virtuellen Maschinen packen Container das Betriebssystem nicht. Nur App-Code, Laufzeit, Systemtools, Bibliotheken und Einstellungen werden in Container gepackt. Containers sind schlanker, leichter portierbar und effizienter als virtuelle Maschinen.


Klicken Sie auf eine Option für den Start:

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/cs_public_dedicated_options.png" width="440" alt="Klicken Sie auf ein Symbol, um schnell Ihre ersten Schritte mit {{site.data.keyword.containershort_notm}} zu machen. Klicken Sie in {{site.data.keyword.Bluemix_dedicated_notm}} auf dieses Symbol, um Ihre Optionen anzuzeigen." style="width:440px;" />
<map name="home_map" id="home_map">
<area href="#clusters" alt="Einführung zu Kubernetes-Clustern in {{site.data.keyword.Bluemix_notm}}" title="Einführung zu Kubernetes-Clustern in {{site.data.keyword.Bluemix_notm}}" shape="rect" coords="-7, -8, 108, 211" />
<area href="cs_cli_install.html" alt="Installieren Sie die CLIs." title="Installieren Sie die CLIs." shape="rect" coords="155, -1, 289, 210" />
<area href="cs_dedicated.html#dedicated_environment" alt="{{site.data.keyword.Bluemix_dedicated_notm}}-Cloudumgebung" title="{{site.data.keyword.Bluemix_notm}}-Cloudumgebung" shape="rect" coords="326, -10, 448, 218" />
</map>


## Einführung in Cluster
{: #clusters}

Sie möchten also eine App in einem Container bereitstellen? Dann müssen Sie als Erstes einen Kubernetes-Cluster erstellen. Kubernetes ist ein Orchestrierungstool für Container. Mit Kubernetes können Entwickler hoch verfügbare Apps im Handumdrehen bereitstellen, indem sie sich die Leistungsstärke und Flexibilität von Clustern zunutze machen.
{:shortdesc}

Und was ist ein Cluster? Ein Cluster ist eine Gruppe von Ressourcen, Knoten, Netzen und Speichereinheiten, die die Hochverfügbarkeit von Apps sicherstellen. Nachdem Sie den Cluster erstellt haben, können Sie Ihre Apps in Containern bereitstellen.

**Vorbereitende Schritte**

Sie müssen über ein Testkonto, ein nutzungsabhängiges Konto oder ein Abonnementkonto als [{{site.data.keyword.Bluemix_notm}}-Konto](https://console.bluemix.net/registration/) verfügen.

Mit einem Testkonto können Sie einen kostenlosen Cluster erstellen, den Sie 21 Tage lang verwenden können, um sich mit dem Service vertraut zu machen. Mit einem nutzungsabhängigen Konto oder einem Abonnementkonto können Sie auch einen kostenlosen Testcluster erstellen, jedoch auch IBM Cloud Infrastructure-Ressourcen (SoftLayer) für die Verwendung in Standardclustern bereitstellen.
{:tip}

Gehen Sie wie folgt vor, um einen kostenlosen Cluster zu erstellen:

1.  Wählen Sie im [{{site.data.keyword.Bluemix_notm}}-**Katalog** ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/catalog/?category=containers) den Eintrag **Container in Kubernetes-Clustern** aus und klicken Sie auf **Erstellen**. Es wird eine Seite für die Clusterkonfiguration geöffnet. Standardmäßig ist **Kostenloser Cluster** ausgewählt.

2. Geben Sie dem Cluster einen eindeutigen Namen.

3.  Klicken Sie auf **Cluster erstellen**. Es wird ein Workerknoten erstellt. Die Bereitstellung kann einige Minuten dauern, der Fortschritt wird jedoch auf der Registerkarte **Workerknoten** angezeigt. Wenn der Status `Bereit` lautet, können Sie anfangen, mit dem Cluster zu arbeiten!

Hervorragend! Sie haben Ihren ersten Kubernetes-Cluster erstellt. Im Folgenden finden Sie einige Details zu Ihrem kostenlosen Cluster:

*   **Maschinentyp**: Der kostenlose Cluster stellt Ihren Apps einen virtuellen Workerknoten mit 2 CPUs und 4 GB Speicherplatz zur Verfügung. Wenn Sie einen Standardcluster erstellen, können Sie zwischen physischen (Bare-Metal-Maschinen) oder virtuellen Maschinen sowie verschiedenen Maschinengrößen wählen.
*   **Verwalteter Master**: Der Workerknoten wird durch einen dedizierten und hoch verfügbaren {{site.data.keyword.IBM_notm}}-eigenen Kubernetes-Master zentral überwacht und verwaltet, der alle Kubernetes-Ressourcen im Cluster steuert und überwacht. Sie können sich auf Ihren Workerknoten und die darin bereitgestellten Apps konzentrieren, ohne sich Gedanken über die Verwaltung des Masters zu machen.
*   **Infrastrukturressourcen**: Die für die Ausführung des Clusters erforderlichen Ressourcen, z. B. VLANs und IP-Adressen, werden in einem {{site.data.keyword.IBM_notm}}-eigenen Konto von IBM Cloud Infrastructure (SoftLayer) verwaltet. Wenn Sie einen Standardcluster erstellen, verwalten Sie diese Ressourcen in Ihrem eigenen Konto von IBM Cloud Infrastructure (SoftLayer). Weitere Informationen zu diesen Ressourcen und den [erforderlichen Berechtigungen](cs_users.html#infra_access) erhalten Sie, wenn Sie einen Standardcluster erstellen.
*   **Weitere Optionen**: Kostenlose Cluster werden innerhalb der von Ihnen ausgewählten Region bereitgestellt. Sie können jedoch nicht auswählen, an welcher Position (in welchem Rechenzentrum) sie sich befinden werden. Erstellen Sie einen Standardcluster, um Kontrolle über Position, Vernetzung und persistenten Speicher zu haben. [Weitere Informationen zu den Vorteilen von kostenlosen und Standardclustern](cs_why.html#cluster_types).


**Womit möchten Sie fortfahren?**
In den nächsten 21 Tagen haben Sie die Gelegenheit, verschiedene Dinge mit Ihrem kostenlosen Cluster auszuprobieren.

* [Installieren Sie die Befehlszeilenschnittstellen und nehmen Sie die Arbeit mit dem Cluster auf.](cs_cli_install.html#cs_cli_install)
* [Stellen Sie eine App in Ihrem Cluster bereit.](cs_app.html#app_cli)
* [Erstellen Sie einen Standardcluster mit mehreren Knoten zwecks höherer Verfügbarkeit.](cs_clusters.html#clusters_ui)
* [Richten Sie eine private Registry in {{site.data.keyword.Bluemix_notm}} ein, um Docker-Images zu speichern und gemeinsam mit anderen Benutzern zu verwenden.](/docs/services/Registry/index.html)
