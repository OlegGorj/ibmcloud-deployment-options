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



# Regionen und Standorte
{{site.data.keyword.Bluemix}} wird weltweit gehostet. Eine Region ist ein geografischer Bereich, auf den über einen Endpunkt zugegriffen wird. Standorte stellen Rechenzentren innerhalb der Region dar. Die Services in {{site.data.keyword.Bluemix_notm}} sind möglicherweise global verfügbar, oder aber nur innerhalb einer bestimmten Region. Wenn Sie einen Kubernetes-Cluster in {{site.data.keyword.containerlong}} erstellen, verbleiben seine Ressourcen in der Region, in der Sie den Cluster bereitgestellt haben.
{:shortdesc}

[{{site.data.keyword.Bluemix_notm}}-Regionen](#bluemix_regions) unterscheiden sich von [{{site.data.keyword.containershort_notm}}-Regionen](#container_regions).

![{{site.data.keyword.containershort_notm}}-Regionen und -Standorte](/images/regions.png)

_{{site.data.keyword.containershort_notm}}-Regionen und -Standorte_

Folgende {{site.data.keyword.containershort_notm}}-Regionen werden unterstützt:
  * Asien-Pazifik (Norden)
  * Asien-Pazifik (Süden)
  * Zentraleuropa
  * Großbritannien (Süden)
  * Vereinigte Staaten (Osten)
  * Vereinigte Staaten (Süden)


## API-Endpunkte für {{site.data.keyword.Bluemix_notm}}-Regionen
{: #bluemix_regions}

Sie können Ihre Ressourcen übergreifend für die {{site.data.keyword.Bluemix_notm}}-Services organisieren, indem Sie die {{site.data.keyword.Bluemix_notm}}-Regionen verwenden. Sie können beispielsweise einen Kubernetes-Cluster mithilfe eines privaten Docker-Images erstellen, das in Ihrem {{site.data.keyword.registryshort_notm}} derselben Region gespeichert ist.
{:shortdesc}

Um zu überprüfen, in welcher {{site.data.keyword.Bluemix_notm}}-Region Sie sich momentan befinden, müssen Sie den Befehl `bx info` ausführen und dann den Wert im Feld **Region** überprüfen.

Auf {{site.data.keyword.Bluemix_notm}}-Regionen kann zugegriffen werden, indem bei der Anmeldung der API-Endpunkt angegeben wird. Wenn Sie keine Region angeben, werden Sie automatisch bei der nächstgelegenen Region angemeldet.

Beispielsweise können Sie die folgenden Befehle für die Anmeldung bei den API-Endpunkten der {{site.data.keyword.Bluemix_notm}}-Region verwenden:

  * Vereinigte Staaten (Süden) und Vereinigte Staaten (Osten)
      ```
      bx login -a api.ng.bluemix.net
      ```
      {: pre}

  * Sydney und Asien-Pazifik (Norden)
      ```
      bx login -a api.au-syd.bluemix.net
      ```
      {: pre}

  * Deutschland
      ```
      bx login -a api.eu-de.bluemix.net
      ```
      {: pre}

  * Großbritannien
      ```
      bx login -a api.eu-gb.bluemix.net
      ```
      {: pre}



<br />


## API-Endpunkte und Standorte für {{site.data.keyword.containershort_notm}}-Regionen
{: #container_regions}

Mithilfe von {{site.data.keyword.containershort_notm}}-Regionen können Sie Kubernetes-Cluster erstellen oder auf Kubernetes-Cluster in einer Region zugreifen, die von der {{site.data.keyword.Bluemix_notm}}-Region abweicht, in der Sie angemeldet sind. Die Endpunkte für die {{site.data.keyword.containershort_notm}}-Region verweisen speziell auf {{site.data.keyword.containershort_notm}} und nicht auf {{site.data.keyword.Bluemix_notm}} im Allgemeinen.
{:shortdesc}

Sie können auf {{site.data.keyword.containershort_notm}} über einen globalen Endpunkt zugreifen: `https://containers.bluemix.net/`.
* Um zu überprüfen, in welcher {{site.data.keyword.containershort_notm}}-Region Sie sich momentan befinden, führen Sie `bx cs region` aus.
* Um eine Liste der verfügbaren Region samt den zugehörigen Endpunkten abzurufen, führen Sie den Befehl `bx cs regions` aus.

Um die API mit dem globalen Endpunkt zu verwenden, übergeben Sie in allen Ihren Anforderungen den Regionsnamen im Header vom Typ `X-Region`.
{: tip}

### Bei einer anderen {{site.data.keyword.containerlong}_notm}-Region anmelden
{: #container_login_endpoints}

Sie können Standorte mithilfe der {{site.data.keyword.containershort_notm}}-CLI ändern.
{:shortdesc}

Sie können sich beispielsweise aus den folgenden Gründen bei einer anderen Region als der {{site.data.keyword.containershort_notm}}-Region anmelden:
  * Sie haben {{site.data.keyword.Bluemix_notm}}-Services oder private Docker-Images in einer Region erstellt und wollen sie mit {{site.data.keyword.containershort_notm}} in einer anderen Region verwenden.
  * Sie möchten auf einen Cluster in einer Region zugreifen, die sich von der {{site.data.keyword.Bluemix_notm}}-Standardregion unterscheidet, bei der Sie angemeldet sind.

</br>

Um schnell zwischen Regionen zu wechseln, führen Sie `bx cs region-set`.

### {{site.data.keyword.containerlong_notm}}-API-Befehle verwenden
{: #containers_api}

Um mit der {{site.data.keyword.containershort_notm}}-API interagieren zu können, müssen Sie den Befehlstyp eingeben und an den globalen Endpunkt die Zeichenfolge `/v1/command` anfügen.
{:shortdesc}

Beispiel für die API `GET /clusters`:
  ```
  GET https://containers.bluemix.net/v1/clusters
  ```
  {: codeblock}

</br>

Um die API mit dem globalen Endpunkt zu verwenden, übergeben Sie in allen Ihren Anforderungen den Regionsnamen im Header vom Typ `X-Region`.
Um die verfügbaren Regionen aufzulisten, führen Sie den Befehl `bx cs regions` aus.
{: tip}

Die Dokumentation zu den API-Befehlen finden Sie unter [https://containers.bluemix.net/swagger-api/](https://containers.bluemix.net/swagger-api/).

## In {{site.data.keyword.containershort_notm}} verfügbare Standorte
{: #locations}

Standorte sind physische Rechenzentren, die innerhalb einer {{site.data.keyword.Bluemix_notm}}-Region vorhanden sind. Regionen sind ein konzeptionelles Hilfsmittel, um Standorte zu organisieren. Sie können Standorte (Rechenzentren) in verschiedenen Ländern umfassen. Die folgende Tabelle zeigt die pro Region verfügbaren Standorte.
{:shortdesc}

| Region | Standort | Ort |
|--------|----------|------|
| Asien-Pazifik (Norden) | hkg02, seo01, sng01, tok02 | Hongkong (Sonderverwaltungsregion der VR China), Seoul, Singapur, Tokio |
| Asien-Pazifik (Süden)     | mel01, syd01, syd04        | Melbourne, Sydney |
| Zentraleuropa     | ams03, fra02, par01        | Amsterdam, Frankfurt, Paris |
| Großbritannien (Süden)      | lon02, lon04         | London |
| Vereinigte Staaten (Osten)      | mon01, tor01, wdc06, wdc07        | Montreal, Toronto, Washington DC |
| Vereinigte Staaten (Süden)     | dal10, dal12, dal13, sao01       | Dallas, São Paulo |
{: caption="Verfügbare Regionen und Standorte" caption-side="top"}

Die Ressourcen Ihrer Cluster bleiben an dem Standort (Rechenzentrum), an dem der Cluster bereitgestellt wird. Die folgende Abbildung stellt die Beziehung Ihres Clusters innerhalb der Beispielregion 'Vereinigte Staaten (Osten)' genauer dar.

1.  Die Ressourcen Ihres Clusters, einschließlich der Master- und Workerknoten, befinden sich an demselben Standort, an dem Sie den Cluster bereitgestellt haben. Wenn Sie lokale Container-Orchestrierungsaktionen wie z. B. `kubectl`-Befehle einleiten, werden die Informationen zwischen den Master- und Workerknoten innerhalb desselben Standorts ausgetauscht.

2.  Wenn Sie andere Clusterressourcen konfigurieren, z. B. Speicher-, Netz- und Rechenressourcen oder Anwendungen, die in Pods ausgeführt werden, verbleiben die Ressourcen und ihre Daten an dem Standort, an dem Sie den Cluster bereitgestellt haben.

3.  Wenn Sie Cluster-Management-Aktionen wie die `bx cs`-Befehle ausführen, werden Basisinformationen über die Cluster (wie z. B. Name, ID, Benutzer, Befehl) an den regionalen Endpunkt weitergeleitet.

![Erklärung, wo Ihre Clusterresourcen sich befinden](/images/region-cluster-resources.png)

_Erklärung, wo Ihre Clusterressourcen sich befinden._



