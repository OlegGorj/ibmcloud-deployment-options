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



# Netzverkehr zu Edge-Workerknoten beschränken
{: #edge}

Mit Edge-Workerknoten kann die Sicherheit des Kubernetes-Clusters verbessert werden, indem der externe Zugriff auf Workerknoten beschränkt und die Netzarbeitslast in {{site.data.keyword.containerlong}} isoliert wird.
{:shortdesc}

Wenn diese Workerknoten nur für den Netzbetrieb markiert sind, können andere Arbeitslasten nicht die CPU oder den Speicher des entsprechenden Workerknotens nutzen und somit Auswirkungen auf den Netzbetrieb haben.




## Bezeichnung für Edge-Knoten zu Workerknoten hinzufügen
{: #edge_nodes}

Fügen Sie die Bezeichnung `dedicated=edge` zu mindestens zwei Workerknoten auf jedem öffentlichen VLAN in Ihrem Cluster hinzu, um sicherzustellen, dass Ingress- und  Lastausgleichsservices nur für diese Workerknoten bereitgestellt werden.
{:shortdesc}

Vorbemerkungen:

- [Erstellen Sie einen Standardcluster.](cs_clusters.html#clusters_cli)
- Stellen Sie sicher, dass der Cluster mindestens über ein öffentliches VLAN verfügt. Edge-Workerknoten stehen nicht für Cluster mit ausschließlich privaten VLANs zur Verfügung.
- [Richten Sie die Kubernetes-CLI auf den Cluster aus](cs_cli_install.html#cs_cli_configure).

Gehen Sie wie folgt vor, um eine Bezeichnung für Edge-Knoten zu Workerknoten hinzuzufügen:

1. Listen Sie alle Workerknoten im Cluster auf. Verwenden Sie die private IP-Adresse aus der Spalte **NAME**, um die Knoten anzugeben. Wählen Sie mindestens zwei Workerknoten in jedem öffentlichen VLAN als Edge-Workerknoten aus. Für Ingress sind mindestens zwei Workerknoten in jeder Zone zur Bereitstellung von Hochverfügbarkeit erforderlich. 

  ```
  kubectl get nodes -L publicVLAN,privateVLAN,dedicated
  ```
  {: pre}

2. Ordnen Sie den Workerknoten die Bezeichnung `dedicated=edge` zu. Nachdem ein Workerknoten mit der Bezeichnung `dedicated=edge` markiert wurde, werden alle Ingress- und LoadBalancer-Services auf einem Edge-Workerknoten bereitgestellt.

  ```
  kubectl label nodes <knoten1_name> <knoten2_name> dedicated=edge
  ```
  {: pre}

3. Rufen Sie alle vorhandenen LoadBalancer-Services in Ihrem Cluster auf.

  ```
  kubectl get services --all-namespaces -o jsonpath='{range .items[*]}kubectl get service -n {.metadata.namespace} {.metadata.name} -o yaml | kubectl apply -f - :{.spec.type},{end}' | tr "," "\n" | grep "LoadBalancer" | cut -d':' -f1
  ```
  {: pre}

  Beispielausgabe:

  ```
  kubectl get service -n <namensbereich> <servicename> -o yaml | kubectl apply -f
  ```
  {: screen}

4. Kopieren Sie unter Verwendung der Ausgabe aus dem vorherigen Schritt jede Zeile `kubectl get service` und fügen Sie sie ein. Mit diesem Befehl wird die Lastausgleichsfunktion erneut auf einem Edge-Workerknoten bereitgestellt. Nur öffentliche Lastausgleichsfunktionen müssen erneut bereitgestellt werden.

  Beispielausgabe:

  ```
  service "my_loadbalancer" configured
  ```
  {: screen}

Sie haben Workerknoten mit der Bezeichnung `dedicated=edge` markiert und alle vorhandenen LoadBalancer- sowie Ingress-Services erneut auf den Workerknoten bereitgestellt. Verhindern Sie nun als nächsten Schritt, dass andere [Arbeitslasten auf Edge-Workerknoten ausgeführt werden](#edge_workloads) und [blockieren Sie eingehenden Datenverkehr an Knotenports auf Workerknoten](cs_network_policy.html#block_ingress).

<br />


## Verhindern, dass Workloads auf Edge-Workerknoten ausgeführt werden
{: #edge_workloads}

Ein Vorteil von Edge-Workerknoten besteht darin, dass sie so konfiguriert werden können, dass sie nur Netzservices (Networking Services) ausführen.
{:shortdesc}

Die Verwendung der Tolerierung `dedicated=edge` bedeutet, dass alle LoadBalancer- und Ingress-Services nur auf den markierten Workerknoten ausgeführt werden. Um jedoch zu verhindern, dass andere Workloads auf Edge-Workerknoten ausgeführt werden und deren Ressourcen verbrauchen, müssen Sie [Kubernetes-Taints ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/) verwenden.


1. Listen Sie alle Workerknoten mit der Bezeichnung `dedicated=edge` auf.

  ```
  kubectl get nodes -L publicVLAN,privateVLAN,dedicated -l dedicated=edge
  ```
  {: pre}

2. Wenden Sie einen Taint auf alle Workerknoten an, der verhindert, dass Pods auf den entsprechenden Workerknoten ausgeführt werden und der Pods von den Workerknoten entfernt, die nicht die Bezeichnung `dedicated=edge` aufweisen. Die entfernten Pods werden auf anderen Workerknoten mit entsprechender Kapazität erneut bereitgestellt.

  ```
  kubectl taint node <knotenname> dedicated=edge:NoSchedule dedicated=edge:NoExecute
  ```
  Nun werden nur Pods mit der Tolerierung `dedicated=edge` auf Ihren Edge-Workerknoten bereitgestellt.

3. Wenn Sie die [Beibehaltung der Quellen-IP für einen LoadBalancer-Service ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") aktivieren](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-typeloadbalancer), stellen Sie sicher, dass App-Pods auf den Edge-Workerknoten geplant sind, indem Sie [Edge-Knoten-Affinität zu App-Pods hinzufügen](cs_loadbalancer.html#edge_nodes). App-Pods müssen auf Edge-Knoten geplant werden, um eingehende Anforderungen empfangen zu können.
