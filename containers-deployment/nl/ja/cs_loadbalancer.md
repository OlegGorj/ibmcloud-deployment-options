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



# LoadBalancer を使用してアプリを公開する
{: #loadbalancer}

ポートを公開し、ロード・バランサーのポータブル IP アドレスを使用してコンテナー化アプリにアクセスします。
{:shortdesc}

## LoadBalancer を使用してネットワーク・トラフィックを管理する
{: #planning}

標準クラスターを作成すると、{{site.data.keyword.containershort_notm}} は自動的に以下のサブネットをプロビジョンします。
* クラスター作成中にワーカー・ノードのパブリック IP アドレスを決定する 1 次パブリック・サブネット
* クラスター作成中にワーカー・ノードのプライベート IP アドレスを決定する 1 次プライベート・サブネット
* Ingress およびロード・バランサーのネットワーク・サービス用に 5 つのパブリック IP アドレスを提供するポータブル・パブリック・サブネット
* Ingress およびロード・バランサーのネットワーク・サービス用に 5 つのプライベート IP アドレスを提供するポータブル・プライベート・サブネット

ポータブル・パブリック IP アドレスとポータブル・プライベート IP アドレスは静的で、ワーカー・ノードが削除されても変更されません。 サブネットごとに、1 つのポータブル・パブリック IP アドレスと 1 つのポータブル・プライベート IP アドレスがデフォルトの [Ingress アプリケーション・ロード・バランサー](cs_ingress.html)に使用されます。残りの 4 つのポータブル・パブリック IP アドレスと 4 つのポータブル・プライベート IP アドレスは、ロード・バランサー・サービスを作成して単一アプリをパブリック・ネットワークまたはプライベート・ネットワークに公開するために使用できます。

パブリック VLAN のクラスター内に Kubernetes LoadBalancer サービスを作成する時、外部ロード・バランサーが 1 つ作成されます。 LoadBalancer サービスを作成するときには、IP アドレスに関して以下のオプションがあります。

- クラスターがパブリック VLAN 上にある場合、使用可能な 4 つのポータブル・パブリック IP アドレスのいずれかが使用されます。
- クラスターがプライベート VLAN 上にしかない場合、使用可能な 4 つのポータブル・プライベート IP アドレスのいずれかが使用されます。
- 構成ファイルに次のアノテーションを追加することにより、LoadBalancer サービス用にポータブル・パブリック IP アドレスまたはポータブル・プライベート IP アドレスを要求できます: `service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: <public_or_private>`.

LoadBalancer サービスに割り当てられるポータブル・パブリック IP アドレスは永続的なものであり、ワーカー・ノードが削除されたり再作成されたりしても変更されません。 したがって、LoadBalancer サービスの可用性は NodePort サービスより高くなります。 NodePort サービスの場合と異なり、任意のポートをロード・バランサーに割り当て可能で、特定のポート範囲に縛られません。 LoadBalancer サービスを使用する場合は、任意のワーカー・ノードの各 IP アドレスで NodePort を使用することもできます。LoadBalancer サービスを使用しているときに NodePort へのアクセスをブロックするには、[着信トラフィックのブロック](cs_network_policy.html#block_ingress)を参照してください。

LoadBalancer サービスは、アプリに対する着信要求のための外部エントリー・ポイントとして機能します。 インターネットから LoadBalancer サービスにアクセスするには、ロード・バランサーのパブリック IP アドレスと割り当てられたポートを、`<IP_address>:<port>` という形式で使用します。 次の図は、ロード・バランサーがインターネットからアプリへの通信をどのように誘導するかを示しています。

<img src="images/cs_loadbalancer_planning.png" width="550" alt="ロード・バランサーを使用した {{site.data.keyword.containershort_notm}} でのアプリの公開" style="width:550px; border-style: none"/>

1. ロード・バランサーのパブリック IP アドレスとワーカー・ノード上の割り当てられたポートを使用して、要求がアプリに送信されます。

2. 要求が、ロード・バランサー・サービスの内部クラスター IP アドレスとポートに自動的に転送されます。 内部クラスター IP アドレスはクラスター内でのみアクセス可能です。

3. `kube-proxy` が、要求をアプリの Kubernetes ロード・バランサー・サービスにルーティングします。

4. 要求が、アプリがデプロイされたポッドのプライベート IP アドレスに転送されます。 複数のアプリ・インスタンスがクラスターにデプロイされている場合、ロード・バランサーは、アプリ・ポッド間で要求をルーティングします。




<br />


   </td>
        </tr>
        <tr>
          <td><code>selector</code></td>
          <td>アプリが実行されるポッドをターゲットにするために使用する、ラベル・キー (<em>&lt;selector_key&gt;</em>) と値 (<em>&lt;selector_value&gt;</em>) のペアを入力します。ポッドをターゲットにしてサービス・ロード・バランシングに含めるには、<em>&lt;selectorkey&gt;</em> と <em>&lt;selectorvalue&gt;</em> の値を確認します。これらが、デプロイメント yaml の <code>spec.template.metadata.labels</code> セクションで使用した<em>キーと値</em> のペアと同じであることを確認してください。</td>
        </tr>
        <tr>
          <td><code>port</code></td>
          <td>サービスが listen するポート。</td>
        </tr>
        <tr>
          <td><code>loadBalancerIP</code></td>
          <td>プライベート LoadBalancer を作成するには、またはパブリック LoadBalancer 用に特定のポータブル IP アドレスを使用するには、<em>&lt;IP_address&gt;</em> を、使用する IP アドレスに置き換えます。 詳しくは、[Kubernetes の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer) を参照してください。</td>
        </tr>
        </tbody></table>

      3. オプション: **spec** セクションに `loadBalancerSourceRanges` を指定してファイアウォールを構成します。 詳しくは、[Kubernetes の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/tasks/access-application-cluster/configure-cloud-provider-firewall/) を参照してください。

      4. クラスター内にサービスを作成します。

          ```
          kubectl apply -f myloadbalancer.yaml
          ```
          {: pre}

          ロード・バランサー・サービスが作成される際に、ロード・バランサーにポータブル IP アドレスが自動的に割り当てられます。 使用可能なポータブル IP アドレスがなければ、ロード・バランサー・サービスは作成できません。

3.  ロード・バランサー・サービスが正常に作成されたことを確認します。_&lt;myservice&gt;_ を、前のステップで作成したロード・バランサー・サービスの名前に置き換えます。

    ```
    kubectl describe service myloadbalancer
    ```
    {: pre}

    **注:** ロード・バランサー・サービスが適切に作成され、アプリが利用可能になるまでに数分かかることがあります。

    CLI 出力例:

    ```
    Name:                   myloadbalancer
    Namespace:              default
    Labels:                 <none>
    Selector:               app=liberty
    Type:                   LoadBalancer
    Location:               dal10
    IP:                     172.21.xxx.xxx
    LoadBalancer Ingress:   169.xx.xxx.xxx
    Port:                   <unset> 8080/TCP
    NodePort:               <unset> 32040/TCP
    Endpoints:              172.30.xxx.xxx:8080
    Session Affinity:       None
    Events:
      FirstSeen	LastSeen	Count	From			SubObjectPath	Type	 Reason			          Message
      ---------	--------	-----	----			-------------	----	 ------			          -------
      10s		    10s		    1	    {service-controller }	  Normal CreatingLoadBalancer	Creating load balancer
      10s		    10s		    1	    {service-controller }		Normal CreatedLoadBalancer	Created load balancer
    ```
    {: screen}

    **LoadBalancer Ingress** IP アドレスは、ロード・バランサー・サービスに割り当てられたポータブル・パブリック IP アドレスです。

4.  パブリック・ロード・バランサーを作成した場合、インターネットからアプリにアクセスします。
    1.  任意の Web ブラウザーを開きます。
    2.  ロード・バランサーのポータブル・パブリック IP アドレスとポートを入力します。

        ```
        http://169.xx.xxx.xxx:8080
        ```
        {: codeblock}        

5. [ロード・バランサー・サービスのソース IP 保持を有効にする ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-typeloadbalancer) 場合は必ず、[アプリ・ポッドにエッジ・ノード・アフィニティーを追加](cs_loadbalancer.html#edge_nodes)して、エッジ・ワーカー・ノードにアプリ・ポッドをスケジュールするようにしてください。着信要求を受信するには、アプリ・ポッドをエッジ・ノードにスケジュールする必要があります。

6. オプション: 他のゾーンからのアプリへの着信要求を処理するには、各ゾーンでこれらのステップを繰り返してロード・バランサーを追加します。

</staging>

## LoadBalancer サービスを使用してアプリへのパブリック・アクセスまたはプライベート・アクセスを有効にする
{: #config}

開始前に、以下のことを行います。

-   このフィーチャーを使用できるのは、標準クラスターの場合に限られます。
-   ロード・バランサー・サービスに割り当てることのできるポータブル・パブリック IP アドレスまたはポータブル・プライベート IP アドレスが必要です。
-   ポータブル・プライベート IP アドレスを持つロード・バランサー・サービスでは、すべてのワーカー・ノードでパブリック NodePort がまだ開いています。パブリック・トラフィックを回避するためのネットワーク・ポリシーを追加する方法については、[着信トラフィックのブロック](cs_network_policy.html#block_ingress)を参照してください。

ロード・バランサー・サービスを作成するには、以下のようにします。

1.  [アプリをクラスターにデプロイします](cs_app.html#app_cli)。 アプリをクラスターにデプロイする際に、コンテナー内のアプリを実行するポッドが 1 つ以上自動的に作成されます。 構成ファイルの metadata セクションで、デプロイメントにラベルを追加しておく必要があります。 このラベルは、アプリが実行されるすべてのポッドをロード・バランシングに含めるためにそれらのポッドを識別する上で必要です。
2.  公開するアプリのロード・バランサー・サービスを作成します。 アプリを公共のインターネットまたはプライベート・ネットワーク上で利用可能にするには、アプリの Kubernetes サービスを作成します。 アプリを構成しているすべてのポッドをロード・バランシングに含めるようにサービスを構成します。
    1.  `myloadbalancer.yaml` などの名前のサービス構成ファイルを作成します。

    2.  公開するアプリのロード・バランサー・サービスを定義します。
        - クラスターがパブリック VLAN 上にある場合、ポータブル・パブリック IP アドレスが使用されます。 ほとんどのクラスターはパブリック VLAN 上にあります。
        - クラスターがプライベート VLAN 上でのみ使用可能な場合は、ポータブル・プライベート IP アドレスが使用されます。
        - 構成ファイルにアノテーションを追加することにより、LoadBalancer サービス用にポータブル・パブリック IP アドレスまたはポータブル・プライベート IP アドレスを要求できます。

        デフォルトの IP アドレスを使用する LoadBalancer サービスの場合は次のようにします。

        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: myloadbalancer
        spec:
          type: LoadBalancer
          selector:
            <selector_key>:<selector_value>
          ports:
           - protocol: TCP
         port: 8080
        ```
        {: codeblock}

        アノテーションを使用してプライベート IP アドレスまたはパブリックの IP アドレスを指定する LoadBalancer サービスの場合は次のようにします。

        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: myloadbalancer
          annotations:
            service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: <public_or_private>
        spec:
          type: LoadBalancer
          selector:
            <selector_key>: <selector_value>
          ports:
           - protocol: TCP
             port: 8080
          loadBalancerIP: <IP_address>
        ```
        {: codeblock}

        <table>
        <caption>YAML ファイルの構成要素について</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
        </thead>
        <tbody>
        <tr>
          <td><code>selector</code></td>
          <td>アプリが実行されるポッドをターゲットにするために使用する、ラベル・キー (<em>&lt;selector_key&gt;</em>) と値 (<em>&lt;selector_value&gt;</em>) のペアを入力します。ポッドをターゲットにしてサービス・ロード・バランシングに含めるには、<em>&lt;selector_key&gt;</em> と <em>&lt;selector_value&gt;</em> の値を確認します。これらが、デプロイメント yaml の <code>spec.template.metadata.labels</code> セクションで使用した<em>キーと値</em> のペアと同じであることを確認してください。</td>
        </tr>
        <tr>
          <td><code>port</code></td>
          <td>サービスが listen するポート。</td>
        </tr>
        <tr>
          <td>`service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type:`
          <td>LoadBalancer のタイプを指定するアノテーション。 指定可能な値は `private` と `public` です。 パブリック VLAN 上のクラスターにパブリック LoadBalancer を作成する場合には、このアノテーションは必要ありません。</td>
        </tr>
        <tr>
          <td><code>loadBalancerIP</code></td>
          <td>プライベート LoadBalancer を作成するには、またはパブリック LoadBalancer 用に特定のポータブル IP アドレスを使用するには、<em>&lt;IP_address&gt;</em> を、使用する IP アドレスに置き換えます。 詳しくは、[Kubernetes の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer) を参照してください。</td>
        </tr>
        </tbody></table>

    3.  オプション: **spec** セクションに `loadBalancerSourceRanges` を指定してファイアウォールを構成します。 詳しくは、[Kubernetes の資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/tasks/access-application-cluster/configure-cloud-provider-firewall/) を参照してください。

    4.  クラスター内にサービスを作成します。

        ```
        kubectl apply -f myloadbalancer.yaml
        ```
        {: pre}

        ロード・バランサー・サービスが作成される際に、ロード・バランサーにポータブル IP アドレスが自動的に割り当てられます。 使用可能なポータブル IP アドレスがなければ、ロード・バランサー・サービスは作成できません。

3.  ロード・バランサー・サービスが正常に作成されたことを確認します。

    ```
    kubectl describe service myloadbalancer
    ```
    {: pre}

    **注:** ロード・バランサー・サービスが適切に作成され、アプリが利用可能になるまでに数分かかることがあります。

    CLI 出力例:

    ```
    Name:                   myloadbalancer
    Namespace:              default
    Labels:                 <none>
    Selector:               app=liberty
    Type:                   LoadBalancer
    Location:               dal10
    IP:                     172.21.xxx.xxx
    LoadBalancer Ingress:   169.xx.xxx.xxx
    Port:                   <unset> 8080/TCP
    NodePort:               <unset> 32040/TCP
    Endpoints:              172.30.xxx.xxx:8080
    Session Affinity:       None
    Events:
      FirstSeen	LastSeen	Count	From			SubObjectPath	Type	 Reason			          Message
      ---------	--------	-----	----			-------------	----	 ------			          -------
      10s		    10s		    1	    {service-controller }	  Normal CreatingLoadBalancer	Creating load balancer
      10s		    10s		    1	    {service-controller }		Normal CreatedLoadBalancer	Created load balancer
    ```
    {: screen}

    **LoadBalancer Ingress** IP アドレスは、ロード・バランサー・サービスに割り当てられたポータブル・パブリック IP アドレスです。

4.  パブリック・ロード・バランサーを作成した場合、インターネットからアプリにアクセスします。
    1.  任意の Web ブラウザーを開きます。
    2.  ロード・バランサーのポータブル・パブリック IP アドレスとポートを入力します。

        ```
        http://169.xx.xxx.xxx:8080
        ```
        {: codeblock}

5. [ロード・バランサー・サービスのソース IP 保持を有効にする ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-typeloadbalancer) 場合は必ず、[アプリ・ポッドにエッジ・ノード・アフィニティーを追加](cs_loadbalancer.html#edge_nodes)して、エッジ・ワーカー・ノードにアプリ・ポッドをスケジュールするようにしてください。着信要求を受信するには、アプリ・ポッドをエッジ・ノードにスケジュールする必要があります。

<br />


## ソース IP のためにノード・アフィニティーと耐障害性をアプリ・ポッドに追加する
{: #node_affinity_tolerations}

アプリ・ポッドをデプロイすると、必ず、ロード・バランサー・サービス・ポッドも、アプリ・ポッドをデプロイしたワーカー・ノードにデプロイされます。 しかし、次のように、ロード・バランサー・ポッドとアプリ・ポッドが同じワーカー・ノードにスケジュールされない状況もあります。
{: shortdesc}

* エッジ・ノードにテイントがあるため、これらのエッジ・ノードにデプロイできるのはロード・バランサー・サービス・ポッドのみです。このようなノードにはアプリ・ポッドをデプロイできません。
* 複数のパブリック VLAN またはプライベート VLAN にクラスターが接続されている場合、1 つの VLAN のみに接続されているワーカー・ノードにアプリ・ポッドはデプロイされます。 ロード・バランサーの IP アドレスがワーカー・ノードとは別の VLAN に接続されているという理由で、このようなワーカー・ノードにはロード・バランサー・サービス・ポッドはデプロイされない可能性があります。

アプリへのクライアント要求がクラスターに送信されると、その要求は、アプリを公開している Kubernetes ロード・バランサー・サービス・ポッドに転送されます。 ロード・バランサー・サービス・ポッドと同じワーカー・ノードにアプリ・ポッドが存在しない場合、ロード・バランサーは異なるワーカー・ノード上のアプリ・ポッドに要求を転送します。パッケージのソース IP アドレスは、アプリ・ポッドが実行されているワーカー・ノードのパブリック IP アドレスに変更されます。

クライアント要求の元のソース IP アドレスを保持するには、ロード・バランサー・サービスの[ソース IP を有効化 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-typeloadbalancer) します。クライアントの IP を保持すると、例えば、アプリ・サーバーがセキュリティーやアクセス制御ポリシーを適用する必要がある場合などに役に立ちます。 ソース IP を有効にした場合は、ロード・バランサー・サービス・ポッドは、同じワーカー・ノードにデプロイされているアプリ・ポッドにのみ要求を転送しなければなりません。 ロード・バランサー・サービス・ポッドもデプロイできる特定のワーカー・ノードに強制的にアプリをデプロイするには、アフィニティー・ルールと耐障害性をアプリのデプロイメントに追加する必要があります。

### エッジ・ノードのアフィニティー・ルールと耐障害性を追加する
{: #edge_nodes}

[ワーカー・ノードにエッジ・ノードのラベルを付け](cs_edge.html#edge_nodes)、さらに[エッジ・ノードにテイントを適用する](cs_edge.html#edge_workloads)場合、ロード・バランサー・サービス・ポッドはそれらのエッジ・ノードにのみデプロイされ、アプリ・ポッドはエッジ・ノードにデプロイできません。ロード・バランサー・サービスでソース IP が有効になっている場合、エッジ・ノード上のロード・バランサー・ポッドは、着信要求を他のワーカー・ノード上のアプリ・ポッドに転送できません。
{:shortdesc}

アプリ・ポッドをエッジ・ノードに強制的にデプロイするには、エッジ・ノードの[アフィニティー・ルール ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#node-affinity-beta-feature) と[耐障害性 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/#concepts) をアプリのデプロイメントに追加します。

エッジ・ノード・アフィニティーとエッジ・ノード耐障害性を追加したデプロイメント yaml のサンプル

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: with-node-affinity
spec:
  template:
    spec:
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: dedicated
                operator: In
                values:
                - edge
      tolerations:
        - key: dedicated
          value: edge
...
```
{: codeblock}

**affinity** と **tolerations** の両方のセクションで、`key` として `dedicated` が、`value` として `edge` が指定されています。

### 複数のパブリック VLAN またはプライベート VLAN にアフィニティー・ルールを追加する
{: #edge_nodes_multiple_vlans}

複数のパブリック VLAN またはプライベート VLAN にクラスターが接続されている場合は、1 つの VLAN のみに接続されているワーカー・ノードにアプリ・ポッドはデプロイされる可能性があります。 ロード・バランサーの IP アドレスが、そのようなワーカー・ノードとは別の VLAN に接続されていると、ロード・バランサー・サービス・ポッドはそのようなワーカー・ノードにデプロイされません。
{:shortdesc}

ソース IP を有効にした場合は、アフィニティー・ルールをアプリのデプロイメントに追加して、ロード・バランサーの IP アドレスと同じ VLAN にあるワーカー・ノードにアプリ・ポッドをスケジュールしてください。

始めに、[CLI のターゲット](cs_cli_install.html#cs_cli_configure)を自分のクラスターに設定してください。

1. ロード・バランサー・サービスのパブリック IP アドレスを取得します。**LoadBalancer Ingress** フィールドの IP アドレスを確認してください。
    ```
    kubectl describe service <loadbalancer_service_name>
    ```
    {: pre}

2. ロード・バランサー・サービスが接続する VLAN ID を取得します。

    1. クラスターのポータブル・パブリック VLAN をリストします。
        ```
        bx cs cluster-get <cluster_name_or_ID> --showResources
        ```
        {: pre}

        出力例:
        ```
        ...

        Subnet VLANs
        VLAN ID   Subnet CIDR       Public   User-managed
        2234947   10.xxx.xx.xxx/29  false    false
        2234945   169.36.5.xxx/29   true     false
        ```
        {: screen}

    2. **Subnet VLANs** の下にある出力で、先ほど取得したロード・バランサーの IP アドレスと一致するサブネット CIDR を見つけ、その VLAN ID をメモします。

        例えば、ロード・バランサー・サービスの IP アドレスが `169.36.5.xxx` の場合、前のステップの出力例で一致するサブネットは `169.36.5.xxx/29` です。そのサブネットが接続する VLAN ID は、`2234945` です。

3. アプリのデプロイメントに、前の手順でメモした VLAN ID に対する[アフィニティー・ルールを追加 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#node-affinity-beta-feature) します。

    例えば、複数の VLAN がある場合に、`2234945` のパブリック VLAN にあるワーカー・ノードにのみアプリ・ポッドをデプロイするには、以下のようにします。

    ```
    apiVersion: extensions/v1beta1
    kind: Deployment
    metadata:
      name: with-node-affinity
    spec:
      template:
        spec:
          affinity:
            nodeAffinity:
              requiredDuringSchedulingIgnoredDuringExecution:
                nodeSelectorTerms:
                - matchExpressions:
                  - key: publicVLAN
                    operator: In
                    values:
                    - "2234945"
    ...
    ```
    {: codeblock}

    サンプル YAML では、**affinity** セクションの `key` は `publicVLAN`、`value` は `"2234945"` です。

4. 更新したデプロイメント構成ファイルを適用します。
    ```
    kubectl apply -f with-node-affinity.yaml
    ```
    {: pre}

5. アプリ・ポッドが、指定した VLAN に接続されたワーカー・ノードにデプロイされたことを確認します。

    1. クラスター内のポッドをリストします。 `<selector>` を、アプリに使用したラベルに置き換えます。
        ```
        kubectl get pods -o wide app=<selector>
        ```
        {: pre}

        出力例:
        ```
        NAME                   READY     STATUS              RESTARTS   AGE       IP               NODE
        cf-py-d7b7d94db-vp8pq  1/1       Running             0          15d       172.30.xxx.xxx   10.176.48.78
        ```
        {: screen}

    2. 出力で、アプリのポッドを確認します。 ポッドがあるワーカー・ノードの **NODE** ID をメモします。

        前のステップの出力例では、アプリ・ポッド `cf-py-d7b7d94db-vp8pq` がワーカー・ノード `10.176.48.78` にあります。

    3. ワーカー・ノードの詳細情報をリストします。

        ```
        kubectl describe node <worker_node_ID>
        ```
        {: pre}

        出力例:

        ```
        Name:                   10.xxx.xx.xxx
        Role:
        Labels:                 arch=amd64
                                beta.kubernetes.io/arch=amd64
                                beta.kubernetes.io/os=linux
                                failure-domain.beta.kubernetes.io/region=us-south
                                failure-domain.beta.kubernetes.io/zone=dal10
                                ibm-cloud.kubernetes.io/encrypted-docker-data=true
                                kubernetes.io/hostname=10.xxx.xx.xxx
                                privateVLAN=2234945
                                publicVLAN=2234967
        ...
        ```
        {: screen}

    4. 出力の **Labels** セクションのパブリック VLAN またはプライベート VLAN が、前の手順で指定した VLAN であることを確認します。
