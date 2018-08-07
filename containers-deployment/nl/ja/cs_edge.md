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



# ネットワーク・トラフィックをエッジ・ワーカー・ノードに制限する
{: #edge}

エッジ・ワーカー・ノードを使用すると、外部からアクセスされるワーカー・ノードの数を減らし、{{site.data.keyword.containerlong}} のネットワーキングのワークロードを分離することができるので、Kubernetes クラスターのセキュリティーが改善されます。
{:shortdesc}

これらのワーカー・ノードがネットワーキング専用としてマーク付けされると、他のワークロードはワーカー・ノードの CPU やメモリーを消費してネットワーキングに干渉することがなくなります。




## ワーカー・ノードにエッジ・ノードとしてラベル付けする
{: #edge_nodes}

クラスター内の各パブリック VLAN 上の複数のワーカー・ノードに `dedicated=edge` ラベルを追加して、Ingress とロード・バランサーがそれらのワーカー・ノードにだけデプロイされるようにします。
{:shortdesc}

開始前に、以下のことを行います。

- [標準クラスターを作成します。](cs_clusters.html#clusters_cli)
- クラスターに 1 つ以上のパブリック VLAN があることを確認してください。 エッジ・ワーカー・ノードは、プライベート VLAN だけがあるクラスターには使用できません。
- [クラスターを Kubernetes CLI のターゲットとして設定](cs_cli_install.html#cs_cli_configure)します。

ワーカー・ノードにエッジ・ノードとしてラベル付けするには、以下のようにします。

1. クラスター内のすべてのワーカー・ノードをリストします。 **NAME** 列からプライベート IP アドレスを使用して、ノードを識別します。 各パブリック VLAN で少なくとも 2 つのワーカー・ノードをエッジ・ワーカー・ノードとして選択します。 Ingress は、高可用性を提供するために、各ゾーンに少なくとも 2 つのワーカー・ノードを必要とします。 

  ```
  kubectl get nodes -L publicVLAN,privateVLAN,dedicated
  ```
  {: pre}

2. `dedicated=edge` により、ワーカー・ノードにラベルを付けます。 `dedicated=edge` によりワーカー・ノードにマークが付けられると、すべての後続の Ingress とロード・バランサーは、エッジ・ワーカー・ノードにデプロイされます。

  ```
  kubectl label nodes <node1_name> <node2_name> dedicated=edge
  ```
  {: pre}

3. クラスター内の既存のロード・バランサー・サービスをすべて検索します。

  ```
  kubectl get services --all-namespaces -o jsonpath='{range .items[*]}kubectl get service -n {.metadata.namespace} {.metadata.name} -o yaml | kubectl apply -f - :{.spec.type},{end}' | tr "," "\n" | grep "LoadBalancer" | cut -d':' -f1
  ```
  {: pre}

  出力例:

  ```
  kubectl get service -n <namespace> <service_name> -o yaml | kubectl apply -f
  ```
  {: screen}

4. 直前のステップからの出力を使用して、それぞれの `kubectl get service` 行をコピーして貼り付けます。 このコマンドは、ロード・バランサーをエッジ・ワーカー・ノードに再デプロイします。 再デプロイする必要があるのは、パブリック・ロード・バランサーだけです。

  出力例:

  ```
  service "my_loadbalancer" configured
  ```
  {: screen}

ワーカー・ノードに `dedicated=edge` のラベルを付け、既存のロード・バランサーのすべてと Ingress をエッジ・ワーカー・ノードに再デプロイしました。次に、他の[ワークロードがエッジ・ワーカー・ノード上で実行されないようにして](#edge_workloads)、[ワーカー・ノード上の NodePort へのインバウンド・トラフィックをブロックします](cs_network_policy.html#block_ingress)。

<br />


## ワークロードがエッジ・ワーカー・ノード上で実行されないようにする
{: #edge_workloads}

エッジ・ワーカー・ノードの利点の 1 つは、それらがネットワーク・サービスだけを実行するように指定できることです。
{:shortdesc}

`dedicated=edge` 耐障害性の使用は、すべてのロード・バランサーと Ingress サービスが、ラベルの付けられたワーカー・ノードにのみデプロイされることを意味します。 ただし、他のワークロードがエッジ・ワーカー・ノード上で実行されてワーカー・ノードのリソースを消費することがないようにするため、[Kubernetes テイント ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/) を使用する必要があります。


1. `dedicated=edge` ラベルの付いたワーカー・ノードをすべてリストします。

  ```
  kubectl get nodes -L publicVLAN,privateVLAN,dedicated -l dedicated=edge
  ```
  {: pre}

2. テイントを各ワーカー・ノードに適用します。このテイントは、ポッドがワーカー・ノード上で実行されるのを防ぎ、`dedicated=edge` ラベルのないポッドをワーカー・ノードから削除します。 削除されるポッドは、容量のある他のワーカー・ノードに再デプロイされます。

  ```
  kubectl taint node <node_name> dedicated=edge:NoSchedule dedicated=edge:NoExecute
  ```
  これで、`dedicated=edge` 耐障害性のあるポッドだけがエッジ・ワーカー・ノードにデプロイされます。

3. [ロード・バランサー・サービスのソース IP 保持を有効にする ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-typeloadbalancer) 場合は必ず、[アプリ・ポッドにエッジ・ノード・アフィニティーを追加](cs_loadbalancer.html#edge_nodes)して、エッジ・ワーカー・ノードにアプリ・ポッドをスケジュールするようにしてください。着信要求を受信するには、アプリ・ポッドをエッジ・ノードにスケジュールする必要があります。
