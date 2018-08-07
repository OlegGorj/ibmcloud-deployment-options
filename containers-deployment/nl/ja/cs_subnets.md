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




# クラスター用のサブネットの構成
{: #subnets}

{{site.data.keyword.containerlong}} で Kubernetes クラスターにサブネットを追加して、使用可能なポータブル・パブリック IP アドレスまたはポータブル・プライベート IP アドレスのプールを変更します。
{:shortdesc}

{{site.data.keyword.containershort_notm}} では、クラスターにネットワーク・サブネットを追加して、Kubernetes サービス用の安定したポータブル IP アドレスを追加できます。 このケースでは、1 つ以上のクラスター間に接続を作成するための、ネットマスキングを指定するサブネットは使用されません。 代わりに、サービスへのアクセスに使用できるクラスターからそのサービスの永続的な固定 IP アドレスを利用できるように、サブネットが使用されます。

<dl>
  <dt>クラスターの作成には、デフォルトでサブネットの作成が含まれる</dt>
  <dd>標準クラスターを作成すると、{{site.data.keyword.containershort_notm}} は自動的に以下のサブネットをプロビジョンします。
    <ul><li>クラスター作成中にワーカー・ノードのパブリック IP アドレスを決定する 1 次パブリック・サブネット</li>
    <li>クラスター作成中にワーカー・ノードのプライベート IP アドレスを決定する 1 次プライベート・サブネット</li>
    <li>Ingress およびロード・バランサーのネットワーク・サービス用に 5 つのパブリック IP アドレスを提供するポータブル・パブリック・サブネット</li>
    <li>Ingress およびロード・バランサーのネットワーク・サービス用に 5 つのプライベート IP アドレスを提供するポータブル・プライベート・サブネット</li></ul>
      ポータブル・パブリック IP アドレスとポータブル・プライベート IP アドレスは静的で、ワーカー・ノードが削除されても変更されません。 サブネットごとに、1 つのポータブル・パブリック IP アドレスと 1 つのポータブル・プライベート IP アドレスがデフォルトの [Ingress アプリケーション・ロード・バランサー](cs_ingress.html)に使用されます。Ingress アプリケーション・ロード・バランサーを使用して、クラスター内の複数のアプリを公開できます。残りの 4 つのポータブル・パブリック IP アドレスと 4 つのポータブル・プライベート IP アドレスは、[ロード・バランサー・サービスを作成して](cs_loadbalancer.html)単一アプリをパブリック・ネットワークまたはプライベート・ネットワークに公開するために使用できます。</dd>
  <dt>[独自の既存のサブネットを注文して管理する](#custom)</dt>
  <dd>自動的にプロビジョンされるサブネットを使用する代わりに、IBM Cloud インフラストラクチャー (SoftLayer) アカウントで既存のポータブル・サブネットを注文して管理することができます。 このオプションは、クラスターの削除や作成が行われても変わらない静的 IP アドレスを保持する場合や、より大きな IP アドレス・ブロックを注文する場合に利用してください。まず、`cluster-create --no-subnet` コマンドを使用して、サブネットなしでクラスターを作成します。それから、`cluster-subnet-add` コマンドを使用して、サブネットをクラスターに追加します。 </dd>
</dl>

**注:** ポータブル・パブリック IP アドレスは、月単位で課金されます。 クラスターをプロビジョンした後にポータブル・パブリック IP アドレスを削除した場合、短時間しか使用していなくても月額料金を支払う必要があります。

## クラスターのその他のサブネットの要求
{: #request}

クラスターにサブネットを割り当て、安定したポータブル・パブリック IP アドレス、またはポータブル・プライベート IP アドレスをクラスターに追加できます。
{:shortdesc}

**注:** クラスターでサブネットを使用できるようにすると、このサブネットの IP アドレスは、クラスターのネットワーキングの目的で使用されるようになります。 IP アドレスの競合を回避するため、1 つのサブネットは必ず 1 つのクラスターでのみ使用してください。あるサブネットを複数のクラスターで使用したり、同時に他の目的で {{site.data.keyword.containershort_notm}}の外部で使用したりしないでください。

始めに、[CLI のターゲット](cs_cli_install.html#cs_cli_configure)を自分のクラスターに設定してください。

IBM Cloud インフラストラクチャー (SoftLayer) アカウントでサブネットを作成し、指定されたクラスターでそのサブネットを使用できるようにするには、以下のようにします。

1. 新しいサブネットをプロビジョンします。

    ```
    bx cs cluster-subnet-create <cluster_name_or_id> <subnet_size> <VLAN_ID>
    ```
    {: pre}

    <table>
    <caption>このコマンドの構成要素について</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> このコマンドの構成要素について</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-subnet-create</code></td>
    <td>クラスターのためにサブネットをプロビジョンするコマンド。</td>
    </tr>
    <tr>
    <td><code><em>&lt;cluster_name_or_id&gt;</em></code></td>
    <td><code>&lt;cluster_name_or_id&gt;</code> をクラスターの名前または ID に置き換えます。</td>
    </tr>
    <tr>
    <td><code><em>&lt;subnet_size&gt;</em></code></td>
    <td><code>&lt;subnet_size&gt;</code> を、ポータブル・サブネットから追加する IP アドレスの数に置き換えます。 受け入れられる値は 8、16、32、64 です。 <p>**注:**  サブネットのポータブル IP アドレスを追加する場合、3 つの IP アドレスはクラスター内ネットワークの確立のために使用されます。 これらの 3 つの IP アドレスは、アプリケーション・ロード・バランサーでは、あるいはロード・バランサー・サービスの作成には使用できません。 例えば、8 個のポータブル・パブリック IP アドレスを要求する場合は、そのうちの 5 個を、アプリをパブリックに公開するために使用できます。</p> </td>
    </tr>
    <tr>
    <td><code><em>&lt;VLAN_ID&gt;</em></code></td>
    <td><code>&lt;VLAN_ID&gt;</code> を、ポータブル・パブリック IP アドレスまたはポータブル・プライベート IP アドレスの割り振り先となるパブリック VLAN またはプライベート VLAN の ID に置き換えます。 既存のワーカー・ノードが接続されているパブリック VLAN またはプライベート VLAN を選択する必要があります。 ワーカー・ノードのパブリック VLAN またはプライベート VLAN を確認するには、<code>bx cs worker-get &lt;worker_id&gt;</code> コマンドを実行します。 </td>
    </tr>
    </tbody></table>

2.  クラスターにサブネットが正常に作成されて追加されたことを確認します。 サブネットの CIDR は **VLANs** セクションにリストされます。

    ```
    bx cs cluster-get --showResources <cluster_name_or_ID>
    ```
    {: pre}

3. オプション: [同一 VLAN 上のサブネット間のルーティングを有効に](#vlan-spanning)します。

<br />


## Kubernetes クラスターでカスタム・サブネットおよび既存のサブネットを追加または再使用する
{: #custom}

既存のポータブル・パブリック・サブネットまたはポータブル・プライベート・サブネットを Kubernetes クラスターに追加したり、削除したクラスターのサブネットを再使用したりできます。
{:shortdesc}

始める前に
- [CLI のターゲットを](cs_cli_install.html#cs_cli_configure)自分のクラスターに設定します。
- 不要になったクラスターのサブネットを再使用する場合は、不要なクラスターを削除します。 サブネットは 24 時間以内に削除されます。

   ```
   bx cs cluster-rm <cluster_name_or_ID
   ```
   {: pre}

IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオにある、カスタム・ファイアウォール・ルールまたは使用可能な IP アドレスが設定された既存のサブネットを使用するには、以下のようにします。

1.  使用するサブネットを確認します。 サブネットの ID と VLAN ID をメモしてください。 この例では、サブネット ID は `1602829`、VLAN ID は `2234945` です。

    ```
    bx cs subnets
    ```
    {: pre}

    ```
    Getting subnet list...
    OK
    ID        Network             Gateway          VLAN ID   Type      Bound Cluster
    1550165   10.xxx.xx.xxx/26    10.xxx.xx.xxx    2234947   private
    1602829   169.xx.xxx.xxx/28   169.xx.xxx.xxx   2234945   public

    ```
    {: screen}

2.  VLAN のロケーションを確認します。 この例では、ロケーションは dal10 です。

    ```
    bx cs vlans dal10
    ```
    {: pre}

    ```
    Getting VLAN list...
    OK
    ID        Name   Number   Type      Router         Supports Virtual Workers
    2234947          1813     private   bcr01a.dal10   true
    2234945          1618     public    fcr01a.dal10   true
    ```
    {: screen}

3.  確認したロケーションと VLAN ID を使用して、クラスターを作成します。 既存のサブネットを再使用する場合は、新しいポータブル・パブリック IP サブネットと新しいポータブル・プライベート IP サブネットが自動的に作成されないように、`--no-subnet` フラグを指定します。

    ```
    bx cs cluster-create --location dal10 --machine-type u2c.2x4 --no-subnet --public-vlan 2234945 --private-vlan 2234947 --workers 3 --name my_cluster
    ```
    {: pre}

4.  クラスターの作成が要求されたことを確認します。

    ```
    bx cs clusters
    ```
    {: pre}

    **注:** ワーカー・ノード・マシンが配列され、クラスターがセットアップされて自分のアカウントにプロビジョンされるまでに、最大 15 分かかります。

    クラスターのプロビジョニングが完了すると、クラスターの状況が **deployed** に変わります。

    ```
    Name         ID                                   State      Created          Workers   Location   Version
    mycluster    aaf97a8843a29941b49a598f516da72101   deployed   20170201162433   3         dal10      1.9.7
    ```
    {: screen}

5.  ワーカー・ノードの状況を確認します。

    ```
    bx cs workers <cluster>
    ```
    {: pre}

    ワーカー・ノードの準備が完了すると、状態が **normal** に変わり、状況が **Ready** になります。 ノードの状況が**「Ready」**になったら、クラスターにアクセスできます。

    ```
    ID                                                  Public IP        Private IP     Machine Type   State      Status   Location   Version
    prod-dal10-pa8dfcc5223804439c87489886dbbc9c07-w1    169.xx.xxx.xxx   10.xxx.xx.xxx  free           normal     Ready    dal10      1.9.7
    ```
    {: screen}

6.  サブネット ID を指定してクラスターにサブネットを追加します。 クラスターでサブネットを使用できるようにすると、使用できるすべての使用可能なポータブル・パブリック IP アドレスが含まれた Kubernetes の構成マップが作成されます。 アプリケーション・ロード・バランサーがクラスターに存在しない場合は、パブリック・アプリケーション・ロード・バランサーとプライベート・アプリケーション・ロード・バランサーを作成するために、1 つのポータブル・パブリック IP アドレスと 1 つのポータブル・プライベート IP アドレスが自動的に使用されます。 その他のすべてのポータブル・パブリック IP アドレスおよびポータブル・プライベート IP アドレスは、アプリのロード・バランサー・サービスの作成に使用できます。

    ```
    bx cs cluster-subnet-add mycluster 807861
    ```
    {: pre}

7. オプション: [同一 VLAN 上のサブネット間のルーティングを有効に](#vlan-spanning)します。

<br />


## Kubernetes クラスターへのユーザー管理のサブネットと IP アドレスの追加
{: #user_managed}

{{site.data.keyword.containershort_notm}} のアクセス先となるオンプレミス・ネットワークのサブネットを指定します。 その後、そのサブネットからのプライベート IP アドレスを、Kubernetes クラスターのロード・バランサー・サービスに追加できます。
{:shortdesc}

要件:
- ユーザー管理のサブネットを追加できるのは、プライベート VLAN のみです。
- サブネットの接頭部の長さの限度は /24 から /30 です。 例えば、`169.xx.xxx.xxx/24` は 253 個の使用可能なプライベート IP アドレスを示し、`169.xx.xxx.xxx/30` は 1 つの使用可能なプライベート IP アドレスを示します。
- サブネットの最初の IP アドレスは、サブネットのゲートウェイとして使用する必要があります。

開始前に、以下のことを行います。
- 外部サブネットとのネットワーク・トラフィックの出入りのルーティングを構成します。
- プライベート・ネットワーク Virtual Router Appliance またはクラスター内で実行されている strongSwan VPN サービスのいずれかと、オンプレミス・データ・センター・ネットワーク・ゲートウェイとの間に VPN 接続があることを確認してください。詳しくは、[VPN 接続のセットアップ](cs_vpn.html)を参照してください。

オンプレミス・ネットワークからサブネットを追加するには、以下のようにします。

1. クラスターのプライベート VLAN の ID を表示します。 **VLANs** セクションを見つけます。 **User-managed** フィールドで、_false_ となっている VLAN ID を見つけます。

    ```
    bx cs cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
    VLANs
    VLAN ID   Subnet CIDR       Public   User-managed
    2234947   10.xxx.xx.xxx/29  false    false
    2234945   169.xx.xxx.xxx/29 true     false
    ```
    {: screen}

2. プライベート VLAN に外部サブネットを追加します。 ポータブル・プライベート IP アドレスがクラスターの構成マップに追加されます。

    ```
    bx cs cluster-user-subnet-add <cluster_name> <subnet_CIDR> <VLAN_ID>
    ```
    {: pre}

    例:

    ```
    bx cs cluster-user-subnet-add mycluster 10.xxx.xx.xxx/24 2234947
    ```
    {: pre}

3. ユーザー提供のサブネットが追加されていることを確認します。 **User-managed** フィールドは _true_ です。

    ```
    bx cs cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
    VLANs
    VLAN ID   Subnet CIDR       Public   User-managed
    2234947   10.xxx.xx.xxx/29  false    false
    2234945   169.xx.xxx.xxx/29 true     false
    2234947   10.xxx.xx.xxx/24  false    true
    ```
    {: screen}

4. オプション: [同一 VLAN 上のサブネット間のルーティングを有効に](#vlan-spanning)します。

5. プライベート・ネットワークを介してアプリにアクセスするために、プライベート・ロード・バランサー・サービスまたはプライベート Ingress アプリケーション・ロード・バランサーを追加します。 追加したサブネットのプライベート IP アドレスを使用するには、IP アドレスを指定する必要があります。 使用しない場合は、IP アドレスは IBM Cloud インフラストラクチャー (SoftLayer) サブネット、またはプライベート VLAN 上のユーザー提供のサブネットからランダムに選択されます。 詳しくは、[LoadBalancer サービスを使用してアプリへのパブリック・アクセスまたはプライベート・アクセスを有効にする](cs_loadbalancer.html#config)または[プライベートのアプリケーション・ロード・バランサーを有効にする](cs_ingress.html#private_ingress)を参照してください。

<br />


## IP アドレスとサブネットの管理
{: #manage}

使用可能なパブリック IP アドレスのリスト表示、使用されている IP アドレスの解放、同一 VLAN 上の複数のサブネット間のルーティングについては、以下のオプションを確認してください。
{:shortdesc}

### 使用可能なポータブル・パブリック IP アドレスの表示
{: #review_ip}

クラスター内のすべての IP アドレス (使用されているものと使用可能なものの両方) をリスト表示するには、以下を実行します。

  ```
  kubectl get cm ibm-cloud-provider-vlan-ip-config -n kube-system -o yaml
  ```
  {: pre}

クラスターで使用可能なパブリック IP アドレスのみリスト表示するには、以下の手順を使用します。

始めに、[使用するクラスターのコンテキストを設定してください。](cs_cli_install.html#cs_cli_configure)

1.  `myservice.yaml` という Kubernetes サービス構成ファイルを作成します。このファイルでは、ダミーのロード・バランサー IP アドレスを使用して `LoadBalancer` タイプのサービスを定義します。 以下の例では、ロード・バランサー IP アドレスとして IP アドレス 1.1.1.1 を使用します。

    ```
    apiVersion: v1
    kind: Service
    metadata:
      labels:
        run: myservice
      name: myservice
      namespace: default
    spec:
      ports:
      - port: 80
        protocol: TCP
        targetPort: 80
      selector:
        run: myservice
      sessionAffinity: None
      type: LoadBalancer
      loadBalancerIP: 1.1.1.1
    ```
    {: codeblock}

2.  クラスター内にサービスを作成します。

    ```
    kubectl apply -f myservice.yaml
    ```
    {: pre}

3.  サービスを検査します。

    ```
    kubectl describe service myservice
    ```
    {: pre}

    **注:** Kubernetes マスターが、指定されたロード・バランサー IP アドレスを Kubernetes 構成マップで見つけることができないため、このサービスの作成は失敗します。 このコマンドを実行すると、エラー・メッセージと、クラスターで使用可能なパブリック IP アドレスのリストが表示されます。

    ```
    Error on cloud load balancer a8bfa26552e8511e7bee4324285f6a4a for service default/myservice with UID 8bfa2655-2e85-11e7-bee4-324285f6a4af: Requested cloud provider IP 1.1.1.1 is not available. The following cloud provider IP addresses are available: <list_of_IP_addresses>
    ```
    {: screen}

### 使用されている IP アドレスの解放
{: #free}

ポータブル IP アドレスを使用しているロード・バランサー・サービスを削除することによって、使用されているポータブル IP アドレスを解放できます。
{:shortdesc}

始めに、[使用するクラスターのコンテキストを設定してください。](cs_cli_install.html#cs_cli_configure)

1.  クラスターで使用可能なサービスをリスト表示します。

    ```
    kubectl get services
    ```
    {: pre}

2.  パブリック IP アドレスまたはプライベート IP アドレスを使用しているロード・バランサー・サービスを削除します。

    ```
    kubectl delete service <service_name>
    ```
    {: pre}

### 同一 VLAN 上のサブネット間のルーティングを有効にする
{: #vlan-spanning}

クラスターを作成すると、末尾が `/26` のサブネットが、そのクラスターがある同一の VLAN 内にプロビジョンされます。 この 1 次サブネットは、最大 62 個までワーカー・ノードを保持できます。
{:shortdesc}

同一 VLAN 上の単一地域内に大規模なクラスターがあったり、小さくても複数あったりすると、この 62 個のワーカー・ノードの制限を超過する可能性があります。 62 個のワーカー・ノードの制限に達すると、同一 VLAN 内で 2 つ目の 1 次サブネットが配列されます。

同一 VLAN 上のサブネット間をルーティングするには、VLAN スパンをオンにする必要があります。 手順については、[VLAN スパンの有効化または無効化](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning)を参照してください。
