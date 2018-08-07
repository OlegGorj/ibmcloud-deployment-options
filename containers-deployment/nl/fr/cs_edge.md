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



# Restriction du trafic réseau aux noeuds worker de périphérie
{: #edge}

Les noeuds worker de périphérie peuvent améliorer la sécurité de votre cluster en limitant les accès aux noeuds worker depuis l'extérieur et en isolant la charge de travail du réseau dans {{site.data.keyword.containerlong}}.
{:shortdesc}

Lorsque ces noeuds worker sont marqués pour mise en réseau uniquement, les autres charges de travail ne peuvent pas consommer d'unité centrale ou de mémoire ni interférer avec le réseau.




## Etiquetage de noeuds worker en tant que noeuds de périphérie
{: #edge_nodes}

Ajoutez l'étiquette `dedicated=edge` à au moins deux noeuds worker sur chaque VLAN public de votre cluster par garantir qu'Ingress et les équilibreurs de charge sont déployés uniquement sur ces noeuds worker.
{:shortdesc}

Avant de commencer :

- [Créez un cluster standard.
](cs_clusters.html#clusters_cli)
- Vérifiez que votre cluster dispose d'au moins un VLAN public. Les noeuds worker de périphérie ne sont pas disponibles pour les clusters avec VLAN privés uniquement.
- [Ciblez l'interface CLI de Kubernetes sur le cluster](cs_cli_install.html#cs_cli_configure).

Pour étiqueter des noeuds worker en tant que noeuds de périphérie :

1. Répertoriez tous les noeuds worker présents dans votre cluster. Utilisez l'adresse IP privée de la colonne **NAME** pour identifier les noeuds. Sélectionnez au moins deux noeuds worker sur chaque VLAN public comme noeuds worker de périphérie. Ingress nécessite au moins deux noeuds worker dans chaque zone pour fournir la haute disponibilité. 

  ```
  kubectl get nodes -L publicVLAN,privateVLAN,dedicated
  ```
  {: pre}

2. Etiquetez les noeuds worker `dedicated=edge`. Une fois qu'un noeud worker est marqué avec `dedicated=edge`, tous les équilibreurs de charge et Ingress suivants sont déployés sur un noeud worker de périphérie.

  ```
  kubectl label nodes <node1_name> <node2_name> dedicated=edge
  ```
  {: pre}

3. Extrayez tous les services d'équilibreur de charge dans le cluster.

  ```
  kubectl get services --all-namespaces -o jsonpath='{range .items[*]}kubectl get service -n {.metadata.namespace} {.metadata.name} -o yaml | kubectl apply -f - :{.spec.type},{end}' | tr "," "\n" | grep "LoadBalancer" | cut -d':' -f1
  ```
  {: pre}

  Exemple de sortie :

  ```
  kubectl get service -n <namespace> <service_name> -o yaml | kubectl apply -f
  ```
  {: screen}

4. A partir de la sortie de l'étape précédente, copiez et collez chaque ligne `kubectl get service`. Cette commande redéploie l'équilibreur de charge sur un noeud worker de périphérie. Seuls les équilibreurs de charge publics doivent être redéployés.

  Exemple de sortie :

  ```
  service "my_loadbalancer" configured
  ```
  {: screen}

Vous avez étiqueté des noeuds worker avec `dedicated=edge` et redéployé tous les équilibreurs de charge et Ingress existants sur les noeuds worker de périphérie. Ensuite, empêchez d'autres [charges de travail de s'exécuter sur des noeuds worker de périphérie](#edge_workloads) et [bloquez le trafic entrant vers des ports de noeud (NodePort) sur des noeuds worker](cs_network_policy.html#block_ingress).

<br />


## Empêcher l'exécution de charges de travail sur des noeuds worker de périphérie
{: #edge_workloads}

Un avantage des noeuds worker de périphérie est le fait qu'ils peuvent être définis pour n'exécuter que des services de mise en réseau.
{:shortdesc}

La tolérance `dedicated=edge` implique que tous les services d'équilibreur de charge et Ingress sont déployés uniquement sur les noeuds worker étiquetés. Toutefois, pour empêcher d'autres charges de travail de s'exécuter sur des noeuds worker de périphérie et de consommer des ressources de noeud worker, vous devez utiliser une [annotation Kubernetes taints![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/).


1. Répertoriez tous les noeuds worker avec l'étiquette `dedicated=edge`.

  ```
  kubectl get nodes -L publicVLAN,privateVLAN,dedicated -l dedicated=edge
  ```
  {: pre}

2. Appliquez à chaque noeud worker une annotation taint qui empêche l'exécution des pods sur le noeud worker et qui supprime du noeud worker ceux qui n'ont pas l'étiquette `dedicated=edge`. Les pods supprimés sont redéployés sur d'autres noeuds worker dont la capacité le permet.

  ```
  kubectl taint node <node_name> dedicated=edge:NoSchedule dedicated=edge:NoExecute
  ```
  Maintenant, seuls les pods ayant la tolérance `dedicated=edge` sont déployés sur vos noeuds worker de périphérie.

3. Si vous choisissez d'[activer la conservation de l'adresse IP source pour un service d'équilibreur de charge ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-typeloadbalancer), assurez-vous que tous les pods d'application sont planifiés sur les noeuds worker de périphérie en [ajoutant l'affinité avec les noeuds de périphérie aux pods d'application](cs_loadbalancer.html#edge_nodes). Les pods d'application doivent être planifiés sur des noeuds de périphérie pour recevoir des demandes entrantes.
