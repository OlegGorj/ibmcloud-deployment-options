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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# クラスターのネットワーキングのトラブルシューティング
{: #cs_troubleshoot_network}

{{site.data.keyword.containerlong}} を使用する際は、ここに示すクラスターのネットワーキングのトラブルシューティング手法を検討してください。
{: shortdesc}

より一般的な問題が起きている場合は、[クラスターのデバッグ](cs_troubleshoot.html)を試してください。
{: tip}


## ロード・バランサー・サービス経由でアプリに接続できない
{: #cs_loadbalancer_fails}

{: tsSymptoms}
クラスター内にロード・バランサー・サービスを作成して、アプリをパブリックに公開しました。 ロード・バランサーのパブリック IP アドレスを使用してアプリに接続しようとしたところ、接続が失敗したか、タイムアウトになりました。

{: tsCauses}
次のいずれかの理由で、ロード・バランサー・サービスが正しく機能していない可能性があります。

-   クラスターが、フリー・クラスターであるか、またはワーカー・ノードが 1 つしかない標準クラスターです。
-   クラスターがまだ完全にデプロイされていません。
-   ロード・バランサー・サービスの構成スクリプトにエラーが含まれています。

{: tsResolve}
ロード・バランサー・サービスのトラブルシューティングを行うには、以下のようにします。

1.  標準クラスターをセットアップしたこと、クラスターが完全にデプロイされていること、また、ロード・バランサー・サービスの高可用性を確保するためにクラスターに 2 つ以上のワーカー・ノードがあることを確認します。

  ```
  bx cs workers <cluster_name_or_ID>
  ```
  {: pre}

    CLI 出力で、ワーカー・ノードの **Status** に **Ready** と表示され、**Machine Type** に **free** 以外のマシン・タイプが表示されていることを確認します。

2.  ロード・バランサー・サービスの構成ファイルが正しいことを確認します。

    ```
    apiVersion: v1
    kind: Service
    metadata:
      name: myservice
    spec:
      type: LoadBalancer
      selector:
        <selector_key>:<selector_value>
      ports:
       - protocol: TCP
         port: 8080
    ```
    {: pre}

    1.  サービスのタイプとして **LoadBalancer** を定義したことを確認します。
    2.  LoadBalancer サービスの `spec.selector` セクションで、`<selector_key>` および `<selector_value>` が、デプロイメント yaml の `spec.template.metadata.labels` セクションで使用したキーと値のペアと同じであることを確認します。ラベルが一致しない場合、LoadBalancer サービスの**「エンドポイント」**セクションに **<none>** と表示され、インターネットからアプリにアクセスできません。
    3.  アプリで listen している **port** を使用していることを確認します。

3.  ロード・バランサー・サービスを確認し、**Events** セクションを参照して、エラーがないか探します。

    ```
    kubectl describe service <myservice>
    ```
    {: pre}

    次のようなエラー・メッセージを探します。

    <ul><li><pre class="screen"><code>Clusters with one node must use services of type NodePort</code></pre></br>ロード・バランサー・サービスを使用するには、2 つ以上のワーカー・ノードがある標準クラスターでなければなりません。</li>
    <li><pre class="screen"><code>No cloud provider IPs are available to fulfill the load balancer service request. Add a portable subnet to the cluster and try again</code></pre></br>このエラー・メッセージは、ロード・バランサー・サービスに割り振れるポータブル・パブリック IP アドレスが残っていないことを示しています。 クラスター用にポータブル・パブリック IP アドレスを要求する方法については、<a href="cs_subnets.html#subnets">クラスターへのサブネットの追加</a>を参照してください。 クラスターにポータブル・パブリック IP アドレスを使用できるようになると、ロード・バランサー・サービスが自動的に作成されます。</li>
    <li><pre class="screen"><code>Requested cloud provider IP <cloud-provider-ip> is not available. The following cloud provider IPs are available: <available-cloud-provider-ips></code></pre></br>**loadBalancerIP** セクションを使用してロード・バランサー・サービスのポータブル・パブリック IP アドレスを定義しましたが、そのポータブル・パブリック IP アドレスはポータブル・パブリック・サブネットに含まれていません。 構成スクリプトの **loadBalancerIP** セクションで、既存の IP アドレスを削除し、使用可能なポータブル・パブリック IP アドレスの 1 つを追加します。スクリプトから **loadBalancerIP** セクションを削除して、使用可能なポータブル・パブリック IP アドレスが自動的に割り振られるようにすることもできます。</li>
    <li><pre class="screen"><code>No available nodes for load balancer services</code></pre>ワーカー・ノードが不足しているため、ロード・バランサー・サービスをデプロイできません。 複数のワーカー・ノードを持つ標準クラスターをデプロイしましたが、ワーカー・ノードのプロビジョンが失敗した可能性があります。</li>
    <ol><li>使用可能なワーカー・ノードのリストを表示します。</br><pre class="codeblock"><code>kubectl get nodes</code></pre></li>
    <li>使用可能なワーカー・ノードが 2 つ以上ある場合は、ワーカー・ノードの詳細情報をリストします。</br><pre class="codeblock"><code>bx cs worker-get [&lt;cluster_name_or_ID&gt;] &lt;worker_ID&gt;</code></pre></li>
    <li><code>kubectl get nodes</code> コマンドと <code>bx cs [&lt;cluster_name_or_ID&gt;] worker-get</code> コマンドから返されたワーカー・ノードのパブリック VLAN ID とプライベート VLAN ID が一致していることを確認します。</li></ol></li></ul>

4.  カスタム・ドメインを使用してロード・バランサー・サービスに接続している場合は、カスタム・ドメインがロード・バランサー・サービスのパブリック IP アドレスにマップされていることを確認します。
    1.  ロード・バランサー・サービスのパブリック IP アドレスを見つけます。

        ```
        kubectl describe service <service_name> | grep "LoadBalancer Ingress"
        ```
        {: pre}

    2.  カスタム・ドメインが、ポインター・レコード (PTR) でロード・バランサー・サービスのポータブル・パブリック IP アドレスにマップされていることを確認します。

<br />




## Ingress 経由でアプリに接続できない
{: #cs_ingress_fails}

{: tsSymptoms}
クラスターでアプリ用の Ingress リソースを作成して、アプリをパブリックに公開しました。 Ingress アプリケーション・ロード・バランサー (ALB) のパブリック IP アドレスまたはサブドメインを使用してアプリに接続しようとしたところ、接続に失敗したか、タイムアウトになりました。

{: tsCauses}
次の理由で、Ingress が正しく機能していない可能性があります。
<ul><ul>
<li>クラスターがまだ完全にデプロイされていません。
<li>クラスターが、フリー・クラスターとして、またはワーカー・ノードが 1 つしかない標準クラスターとしてセットアップされました。
<li>Ingress 構成スクリプトにエラーがあります。
</ul></ul>

{: tsResolve}
Ingress のトラブルシューティングを行うには、以下のようにします。

1.  完全にデプロイされており、ALB の高可用性を確保するために 2 つ以上のワーカー・ノードがある標準クラスターをセットアップしたことを確認します。

  ```
  bx cs workers <cluster_name_or_ID>
  ```
  {: pre}

    CLI 出力で、ワーカー・ノードの **Status** に **Ready** と表示され、**Machine Type** に **free** 以外のマシン・タイプが表示されていることを確認します。

2.  ALB サブドメインおよびパブリック IP アドレスを取得し、それぞれを ping します。

    1.  ALB サブドメインを取得します。

      ```
      bx cs cluster-get <cluster_name_or_ID> | grep "Ingress subdomain"
      ```
      {: pre}

    2.  ALB サブドメインを ping します。

      ```
      ping <ingress_subdomain>
      ```
      {: pre}

    3.  ALB のパブリック IP アドレスを取得します。

      ```
      nslookup <ingress_subdomain>
      ```
      {: pre}

    4.  ALB パブリック IP アドレスを ping します。

      ```
      ping <ALB_IP>
      ```
      {: pre}

    ALB のパブリック IP アドレスまたはサブドメインに対して CLI からタイムアウトが返され、ワーカー・ノードを保護するカスタム・ファイアウォールをセットアップしている場合は、[ファイアウォール](cs_troubleshoot_clusters.html#cs_firewall)で追加のポートとネットワーキング・グループを開きます。

3.  カスタム・ドメインを使用している場合は、ご使用の DNS プロバイダーで、カスタム・ドメインが IBM 提供の ALB のパブリック IP アドレスまたはサブドメインにマップされていることを確認します。
    1.  ALB サブドメインを使用した場合は、正規名レコード (CNAME) を確認します。
    2.  ALB パブリック IP アドレスを使用した場合は、カスタム・ドメインがポインター・レコード (PTR) でポータブル・パブリック IP アドレスにマップされていることを確認します。
4.  Ingress リソース構成ファイルを確認します。

    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingress
    spec:
      tls:
      - hosts:
        - <ingress_subdomain>
        secretName: <ingress_tls_secret>
      rules:
      - host: <ingress_subdomain>
        http:
          paths:
          - path: /
            backend:
              serviceName: myservice
              servicePort: 80
    ```
    {: codeblock}

    1.  ALB のサブドメインと TLS 証明書が正しいことを確認します。 IBM 提供のサブドメインと TLS 証明書を見つけるには、`bx cs cluster-get <cluster_name_or_ID>` を実行します。
    2.  アプリが、Ingress の **path** セクションで構成されているパスを使用して listen していることを確認します。 アプリがルート・パスで listen するようにセットアップされている場合は、**/** をパスとして含めます。
5.  Ingress デプロイメントを確認して、警告メッセージやエラー・メッセージがないか探します。

    ```
    kubectl describe ingress <myingress>
    ```
    {: pre}

    例えば、出力の **Events** セクションに、Ingress リソースや使用した特定のアノテーション内の無効な値に関する警告メッセージが表示される場合があります。

    ```
    Name:             myingress
    Namespace:        default
    Address:          169.xx.xxx.xxx,169.xx.xxx.xxx
    Default backend:  default-http-backend:80 (<none>)
    Rules:
      Host                                             Path  Backends
      ----                                             ----  --------
      mycluster.us-south.containers.appdomain.cloud
                                                       /tea      myservice1:80 (<none>)
                                                       /coffee   myservice2:80 (<none>)
    Annotations:
      custom-port:        protocol=http port=7490; protocol=https port=4431
      location-modifier:  modifier='~' serviceName=myservice1;modifier='^~' serviceName=myservice2
    Events:
      Type     Reason             Age   From                                                            Message
      ----     ------             ----  ----                                                            -------
      Normal   Success            1m    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  TLSSecretNotFound  1m    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress resource.
      Normal   Success            59s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  AnnotationError    40s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress.bluemix.net/custom-port annotation. Error annotation format error : One of the mandatory fields not valid/missing for annotation ingress.bluemix.net/custom-port
      Normal   Success            40s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  AnnotationError    2s    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress.bluemix.net/custom-port annotation. Invalid port 7490. Annotation cannot use ports 7481 - 7490
      Normal   Success            2s    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
    ```
    {: screen}

6.  ALB のログを確認します。
    1.  クラスター内で稼働している Ingress ポッドの ID を取得します。

      ```
      kubectl get pods -n kube-system | grep alb
      ```
      {: pre}

    2.  Ingress ポッドごとにログを取得します。

      ```
      kubectl logs <ingress_pod_ID> nginx-ingress -n kube-system
      ```
      {: pre}

    3.  ALB ログでエラー・メッセージを確認します。

<br />


## Ingress アプリケーション・ロード・バランサーのシークレットの問題
{: #cs_albsecret_fails}

{: tsSymptoms}
Ingress アプリケーション・ロード・バランサー (ALB) のシークレットをクラスターにデプロイした後で、{{site.data.keyword.cloudcerts_full_notm}} 内の証明書を参照したところ、`Description` フィールドがシークレット名で更新されていません。

ALB シークレットに関する情報をリストすると、状況は `*_failed` となっています。例えば、`create_failed`、`update_failed`、`delete_failed` などです。

{: tsResolve}
以下に示す、ALB シークレットが失敗する理由と対応するトラブルシューティング手順を確認してください。

<table>
<caption>Ingress アプリケーション・ロード・バランサーのシークレットのトラブルシューティング</caption>
 <thead>
 <th>現象の理由</th>
 <th>修正方法</th>
 </thead>
 <tbody>
 <tr>
 <td>証明書データのダウンロードやアップデートに必要なアクセス役割を持っていない。</td>
 <td>アカウント管理者に問い合わせて、{{site.data.keyword.cloudcerts_full_notm}} インスタンスに対する**オペレーター**と**エディター**の両方の役割を割り当てるように依頼してください。 詳しくは、{{site.data.keyword.cloudcerts_short}} の <a href="/docs/services/certificate-manager/access-management.html#managing-service-access-roles">サービス・アクセスの管理</a>を参照してください。</td>
 </tr>
 <tr>
 <td>作成時、更新時、または削除時に提供された証明書 CRN が、クラスターと同じアカウントに属していない。</td>
 <td>提供した証明書 CRN が、クラスターと同じアカウントにデプロイされた、{{site.data.keyword.cloudcerts_short}} サービスのインスタンスにインポートされていることを確認します。</td>
 </tr>
 <tr>
 <td>作成時に提供された証明書 CRN が正しくない。</td>
 <td><ol><li>提供した証明書 CRN ストリングが正確であることを確認します。</li><li>証明書 CRN が正確であることがわかった場合は、<code>bx cs alb-cert-deploy --update --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code> を実行して、シークレットの更新を試行します。</li><li>このコマンドの結果が <code>update_failed</code> 状況になる場合は、<code>bx cs alb-cert-rm --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt;</code> を実行して、シークレットを削除します。</li><li><code>bx cs alb-cert-deploy --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code> を実行して、シークレットを再デプロイします。</li></ol></td>
 </tr>
 <tr>
 <td>更新時に提供された証明書 CRN が正しくない。</td>
 <td><ol><li>提供した証明書 CRN ストリングが正確であることを確認します。</li><li>証明書 CRN が正確であることを確認した場合は、<code>bx cs alb-cert-rm --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt;</code> を実行して、シークレットを削除します。</li><li><code>bx cs alb-cert-deploy --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code> を実行して、シークレットを再デプロイします。</li><li><code>bx cs alb-cert-deploy --update --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code> を実行して、シークレットの更新を試行します。</li></ol></td>
 </tr>
 <tr>
 <td>{{site.data.keyword.cloudcerts_long_notm}} サービスがダウンしている。</td>
 <td>{{site.data.keyword.cloudcerts_short}} サービスが稼働中であることを確認します。</td>
 </tr>
 </tbody></table>

<br />


## Ingress ALB のサブドメインを取得できない
{: #cs_subnet_limit}

{: tsSymptoms}
`bx cs cluster-get <cluster>` を実行すると、クラスターが `normal` 状態であるのに、**Ingress サブドメイン**が表示されません。

次のようなエラー・メッセージが表示される可能性があります。

```
この VLAN で許可されているサブネットの最大数に既に達しています。(There are already the maximum number of subnets permitted in this VLAN.)
```
{: screen}

{: tsCauses}
クラスターを作成する際、指定した VLAN で 8 つのパブリック・ポータブル・サブネットと 8 つのプライベート・ポータブル・サブネットが要求されます。 {{site.data.keyword.containershort_notm}} の場合、VLAN のサブネット数の上限は 40 個です。 クラスターの VLAN が既にその上限に達している場合、**Ingress サブドメイン**をプロビジョンできません。

VLAN のサブネット数を表示するには、以下のようにします。
1.  [IBM Cloud インフラストラクチャー (SoftLayer) コンソール](https://control.bluemix.net/)から、**「ネットワーク」**>**「IP 管理 (IP Management)」**>**「VLAN」**の順に選択します。
2.  クラスターの作成に使用した VLAN の **「VLAN の数 (VLAN Number)」**をクリックします。 **「サブネット」**セクションで、サブネットが 40 個以上存在するかどうかを確認します。

{: tsResolve}
新規 VLAN が必要な場合、[{{site.data.keyword.Bluemix_notm}} サポートに連絡して](/docs/get-support/howtogetsupport.html#getting-customer-support)注文してください。 その後、その新規 VLAN を使用する[クラスターを作成します](cs_cli_reference.html#cs_cluster_create)。

使用可能な別の VLAN がある場合は、既存のクラスターで [VLAN スパンニングをセットアップ](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning)できます。 その後、使用可能なサブネットがある他の VLAN を使用する新規ワーカー・ノードをクラスターに追加できます。

VLAN に使用していないサブネットがある場合、クラスターでサブネットを再利用できます。
1.  使用したいサブネットが使用可能であることを確認します。 **注**: 使用しているインフラストラクチャー・アカウントが、複数の {{site.data.keyword.Bluemix_notm}} アカウントで共有されている場合があります。 その場合、**バインドされたクラスター**があるサブネットを表示するために `bx cs subnets` コマンドを実行しても、自分のクラスターの情報しか表示できません。 サブネットが使用可能であり、他のアカウントやチームで使用されていないことをインフラストラクチャー・アカウント所有者に確認してください。

2.  サービスが新規サブネットの作成を試行しないように、`--no-subnet` オプションを指定して [クラスターを作成](cs_cli_reference.html#cs_cluster_create)します。 再利用できるサブネットがある場所と VLAN を指定します。

3.  `bx cs cluster-subnet-add` [コマンド](cs_cli_reference.html#cs_cluster_subnet_add)を使用して、既存のサブネットをクラスターに追加します。 詳しくは、[Kubernetes クラスターでカスタム・サブネットおよび既存のサブネットを追加または再利用する](cs_subnets.html#custom)を参照してください。

<br />


## strongSwan Helm チャートとの VPN 接続を確立できない
{: #cs_vpn_fails}

{: tsSymptoms}
`kubectl exec -n kube-system  $STRONGSWAN_POD -- ipsec status` を実行して VPN 接続を確認すると、`ESTABLISHED` 状況が表示されない、または、VPN ポッドが `ERROR` 状態になっている、または、クラッシュと再始動が繰り返されます。

{: tsCauses}
Helm チャートの構成ファイルに誤った値があるか、値が欠落しているか、構文エラーがあります。

{: tsResolve}
strongSwan Helm チャートとの VPN 接続を確立しようとすると、初回は VPN の状況が `ESTABLISHED` にならない可能性があります。 いくつかのタイプの問題を確認し、それに応じて構成ファイルを変更する必要がある場合があります。 strongSwan VPN 接続をトラブルシューティングするには、以下のようにします。

1. オンプレミス VPN エンドポイントの設定を、構成ファイル内の設定と照らして確認します。 設定が一致しない場合は、以下のようにします。

    <ol>
    <li>既存の Helm chart を削除します。</br><pre class="codeblock"><code>helm delete --purge <release_name></code></pre></li>
    <li><code>config.yaml</code> ファイル内の誤った値を修正し、更新したファイルを保存します。</li>
    <li>新しい Helm チャートをインストールします。</br><pre class="codeblock"><code>helm install -f config.yaml --namespace=kube-system --name=<release_name> bluemix/strongswan</code></pre></li>
    </ol>

2. VPN ポッドが `ERROR` 状態である場合や、クラッシュと再始動が繰り返される場合は、チャートの構成マップ内の `ipsec.conf` 設定のパラメーターの検証が原因である可能性があります。

    <ol>
    <li>strongSwan ポッドのログに検証エラーがないか確認します。</br><pre class="codeblock"><code>kubectl logs -n kube-system $STRONGSWAN_POD</code></pre></li>
    <li>ログに検証エラーがある場合は、既存の Helm チャートを削除します。</br><pre class="codeblock"><code>helm delete --purge <release_name></code></pre></li>
    <li>`config.yaml` ファイル内の誤った値を修正し、更新したファイルを保存します。</li>
    <li>新しい Helm チャートをインストールします。</br><pre class="codeblock"><code>helm install -f config.yaml --namespace=kube-system --name=<release_name> bluemix/strongswan</code></pre></li>
    </ol>

3. strongSwan チャート定義に含まれている 5 つの Helm テストを実行します。

    <ol>
    <li>Helm テストを実行します。</br><pre class="codeblock"><code>helm test vpn</code></pre></li>
    <li>いずれかのテストが失敗した場合は、[Helm VPN 接続テストについて](cs_vpn.html#vpn_tests_table)を参照して、各テストについての情報、およびテストが失敗する原因を確認してください。 <b>注</b>: テストの中には、VPN 構成ではオプションの設定を必要とするテストがあります。 一部のテストが失敗しても、そのようなオプションの設定を指定したかどうかによって、失敗を許容できる場合があります。</li>
    <li>テスト・ポッドのログを参照して、失敗したテストの出力を確認します。<br><pre class="codeblock"><code>kubectl logs -n kube-system <test_program></code></pre></li>
    <li>既存の Helm chart を削除します。</br><pre class="codeblock"><code>helm delete --purge <release_name></code></pre></li>
    <li><code>config.yaml</code> ファイル内の誤った値を修正し、更新したファイルを保存します。</li>
    <li>新しい Helm チャートをインストールします。</br><pre class="codeblock"><code>helm install -f config.yaml --namespace=kube-system --name=<release_name> bluemix/strongswan</code></pre></li>
    <li>変更を確認するには、以下の手順に従います。<ol><li>現在のテスト・ポッドを取得します。</br><pre class="codeblock"><code>kubectl get pods -a -n kube-system -l app=strongswan-test</code></pre></li><li>現在のテスト・ポッドをクリーンアップします。</br><pre class="codeblock"><code>kubectl delete pods -n kube-system -l app=strongswan-test</code></pre></li><li>テストを再度実行します。</br><pre class="codeblock"><code>helm test vpn</code></pre></li>
    </ol></ol>

4. VPN ポッド・イメージ内にパッケージされている VPN デバッグ・ツールを実行します。

    1. `STRONGSWAN_POD` 環境変数を設定します。

        ```
        export STRONGSWAN_POD=$(kubectl get pod -n kube-system -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
        ```
        {: pre}

    2. デバッグ・ツールを実行します。

        ```
        kubectl exec -n kube-system  $STRONGSWAN_POD -- vpnDebug
        ```
        {: pre}

        このツールは、一般的なネットワーキングの問題に関するさまざまなテストを実行するため、複数の情報ページを出力します。 先頭が `ERROR`、`WARNING`、`VERIFY`、または `CHECK` の出力行に、VPN 接続エラーが示されている可能性があります。

    <br />


## ワーカー・ノードの追加または削除後に strongSwan VPN 接続が失敗する
{: #cs_vpn_fails_worker_add}

{: tsSymptoms}
以前は strongSwan IPSec VPN サービスを使用して、正常に機能する VPN 接続を確立できていました。 しかし、クラスターでワーカー・ノードを追加または削除した後に、以下の症状が 1 つ以上発生します。

* VPN 状況が `ESTABLISHED` にならない
* オンプレミス・ネットワークから新しいワーカー・ノードにアクセスできない
* 新しいワーカー・ノード上で実行されているポッドからリモート・ネットワークにアクセスできない

{: tsCauses}
ワーカー・ノードを追加した場合:

* 既存の `localSubnetNAT` 設定または `local.subnet` 設定によって VPN 接続を介して公開されていない、新しいプライベート・サブネットに、ワーカー・ノードがプロビジョンされました。
* ワーカー・ノードのテイントまたはラベルが既存の `tolerations` または `nodeSelector` 設定に含まれていないため、VPN 経路をワーカー・ノードに追加できません。
* 新しいワーカー・ノードで VPN ポッドが実行されていますが、そのワーカー・ノードのパブリック IP アドレスがオンプレミス・ファイアウォールを通過することを許可されていません。

ワーカー・ノードを削除した場合:

* 既存の `tolerations` 設定または `nodeSelector` 設定による特定のテイントまたはラベルに対する制限のため、削除されたワーカー・ノードが、VPN ポッドを実行する唯一のノードでした。

{: tsResolve}
以下のように、ワーカー・ノードの変更を反映するように Helm チャートの値を更新します。

1. 既存の Helm chart を削除します。

    ```
    helm delete --purge <release_name>
    ```
    {: pre}

2. strongSwan VPN サービスの構成ファイルを開きます。

    ```
    helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. 以下の設定を確認し、必要に応じて、削除または追加したワーカー・ノードを反映するように設定を変更します。

    ワーカー・ノードを追加した場合:

    <table>
    <caption>ワーカー・ノードの設定</caption?
     <thead>
     <th>設定</th>
     <th>説明</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>追加したワーカーは、他のワーカー・ノードがある既存のサブネットとは違う新しいプライベート・サブネット上にデプロイされる可能性があります。 サブネット NAT を使用してクラスターのプライベート・ローカル IP アドレスを再マップしており、ワーカーが新しいサブネットに追加された場合は、この設定に新しいサブネット CIDR を追加してください。</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>以前に VPN ポッドのデプロイメントを特定のラベルのワーカーに制限していた場合は、追加したワーカー・ノードにもそのラベルがあることを確認してください。</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>追加したワーカー・ノードにテイントがある場合は、テイントがあるすべてのワーカーまたは特定のテイントがあるすべてのワーカーで VPN ポッドを実行できるように、この設定を変更してください。</td>
     </tr>
     <tr>
     <td><code>local.subnet</code></td>
     <td>追加したワーカーは、他のワーカーがある既存のサブネットとは異なる、新しいプライベート・サブネット上にデプロイされる可能性があります。プライベート・ネットワークの NodePort または LoadBalancer サービスでアプリが公開されており、追加したワーカー上にそれらのアプリがある場合は、この設定に新しいサブネットの CIDR を追加してください。**注**: `local.subnet` に値を追加する場合は、オンプレミス・サブネットの VPN 設定を確認して、それらの設定も更新する必要があるかどうか判断してください。</td>
     </tr>
     </tbody></table>

    ワーカー・ノードを削除した場合:

    <table>
    <caption>ワーカー・ノードの設定</caption>
     <thead>
     <th>設定</th>
     <th>説明</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>サブネット NAT を使用して特定のプライベート・ローカル IP アドレスを再マップしている場合は、その設定から古いワーカーの IP アドレスを削除してください。 サブネット NAT を使用してサブネット全体を再マップし、サブネット上にワーカーが残っていない場合は、そのサブネット CIDR をこの設定から削除してください。</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>以前に VPN ポッドのデプロイメントを単一のワーカーに制限していて、そのワーカーが削除された場合は、他のワーカーで VPN ポッドを実行できるようにこの設定を変更してください。</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>削除したワーカーにはテイントがなく、残りのワーカーにだけテイントがある場合は、テイントがあるワーカーまたは特定のテイントがあるワーカーで VPN ポッドを実行できるように、この設定を変更してください。
     </td>
     </tr>
     </tbody></table>

4. 更新した値を使用して新しい Helm チャートをインストールします。

    ```
    helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/strongswan
    ```
    {: pre}

5. Chart のデプロイメント状況を確認します。 Chart の準備ができている場合は、出力の先頭付近の **STATUS** フィールドに `DEPLOYED` の値があります。

    ```
    helm status <release_name>
    ```
    {: pre}

6. 時により、VPN 構成ファイルに加えた変更と一致するように、オンプレミス設定とファイアウォール設定を変更する必要がある場合があります。

7. VPN を開始します。
    * クラスターから VPN 接続を開始する (`ipsec.auto` を `start` に設定した) 場合は、オンプレミス・ゲートウェイの VPN を開始してからクラスターの VPN を開始します。
    * オンプレミス・ゲートウェイから VPN 接続を開始する (`ipsec.auto` を `auto` に設定した) 場合は、クラスターの VPN を開始してからオンプレミス・ゲートウェイの VPN を開始します。

8. `STRONGSWAN_POD` 環境変数を設定します。

    ```
    export STRONGSWAN_POD=$(kubectl get pod -n kube-system -l app=strongswan,release=<release_name> -o jsonpath='{ .items[0].metadata.name }')
    ```
    {: pre}

9. VPN の状況を確認します。

    ```
    kubectl exec -n kube-system  $STRONGSWAN_POD -- ipsec status
    ```
    {: pre}

    * VPN 接続の状況が `ESTABLISHED` の場合、VPN 接続は正常に行われました。 これ以上のアクションは不要です。

    * 接続の問題が解決されない場合は、[strongSwan Helm チャートとの VPN 接続を確立できない](#cs_vpn_fails)を参照して、VPN 接続のトラブルシューティングをさらに行います。

<br />



## Calico ネットワーク・ポリシーを取得できない
{: #cs_calico_fails}

{: tsSymptoms}
`calicoctl get policy` を実行してクラスター内の Calico ネットワーク・ポリシーを表示しようとすると、以下のいずれかの予期しない結果またはエラー・メッセージが表示されます。
- 空のリスト
- v3 ポリシーではなく、古い Calico v2 ポリシーのリスト
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'projectcalico.org/v3'`

`calicoctl get GlobalNetworkPolicy` を実行してクラスター内の Calico ネットワーク・ポリシーを表示しようとすると、以下のいずれかの予期しない結果またはエラー・メッセージが表示されます。
- 空のリスト
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'v1'`
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'projectcalico.org/v3'`
- `Failed to get resources: Resource type 'GlobalNetworkPolicy' is not supported`

{: tsCauses}
Calico ポリシーを使用するには、クラスターの Kubernetes バージョン、Calico CLI バージョン、Calico 構成ファイル構文、およびポリシーの表示コマンドの 4 つの要因すべてが正しい組み合わせになっている必要があります。これらの要因のうち 1 つ以上が、正しいバージョンになっていません。

{: tsResolve}
[Kubernetes バージョン 1.10 以降](cs_versions.html)のクラスターでは、Calico CLI v3.1、`calicoctl.cfg` v3 構成ファイル構文、および `calicoctl get GlobalNetworkPolicy` コマンドと `calicoctl get NetworkPolicy` コマンドを使用する必要があります。

[Kubernetes バージョン 1.9 以前](cs_versions.html)のクラスターでは、Calico CLI v1.6.3、`calicoctl.cfg` v2 構成ファイル構文、および `calicoctl get policy` コマンドを使用する必要があります。

すべての Calico 要因が正しい組み合わせになっていることを確認するには、以下のようにします。

1. クラスターの Kubernetes バージョンを表示します
    ```
    bx cs cluster-get <cluster_name>
    ```
    {: pre}

    * クラスターが Kubernetes バージョン 1.10 以降である場合は、以下のようにします。
        1. [バージョン 3.1.1 Calico CLI をインストールして構成します](cs_network_policy.html#1.10_install)。この構成には、Calico v3 構文を使用するように `calicoctl.cfg` ファイルを手動で更新することも含まれます。
        2. 作成してクラスターに適用するすべてのポリシーで、[Calico v3 構文 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/networkpolicy) が使用されていることを確認します。既存のポリシー `.yaml` または `.json` ファイルで Calico v2 構文が使用されている場合は、[`calicoctl convert` コマンド ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.projectcalico.org/v3.1/reference/calicoctl/commands/convert) を使用して、これらを Calico v3 構文に変換できます。
        3. [ポリシーを表示](cs_network_policy.html#1.10_examine_policies)するには、必ず `calicoctl get GlobalNetworkPolicy` (グローバル・ポリシーの場合) および `calicoctl get NetworkPolicy --namespace <policy_namespace>` (特定の名前空間にスコープ設定されたポリシーの場合) を使用します。

    * クラスターが Kubernetes バージョン 1.9 以前である場合は、以下のようにします。
        1. [バージョン 1.6.3 Calico CLI をインストールして構成します](cs_network_policy.html#1.9_install)。`calicoctl.cfg` ファイルで Calico v2 構文が使用されていることを確認します。
        2. 作成してクラスターに適用するすべてのポリシーで、 [Calico v2 構文 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.projectcalico.org/v2.6/reference/calicoctl/resources/policy) が使用されていることを確認します。
        3. [ポリシーを表示](cs_network_policy.html#1.9_examine_policies)するには、必ず `calicoctl get policy` を使用します。

Kubernetes バージョン 1.9 以前からバージョン 1.10 以降にクラスターを更新する前に、[Calico v3 への更新の準備](cs_versions.html#110_calicov3)を確認してください。
{: tip}

<br />


## ヘルプとサポートの取得
{: #ts_getting_help}

まだクラスターに問題がありますか?
{: shortdesc}

-   {{site.data.keyword.Bluemix_notm}} が使用可能かどうかを確認するために、[{{site.data.keyword.Bluemix_notm}} 状況ページを確認します![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.ibm.com/bluemix/support/#status)。
-   [{{site.data.keyword.containershort_notm}} Slack ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://ibm-container-service.slack.com) に質問を投稿します。

    {{site.data.keyword.Bluemix_notm}} アカウントに IBM ID を使用していない場合は、この Slack への[招待を要求](https://bxcs-slack-invite.mybluemix.net/)してください。
    {: tip}
-   フォーラムを確認して、同じ問題が他のユーザーで起こっているかどうかを調べます。 フォーラムを使用して質問するときは、{{site.data.keyword.Bluemix_notm}} 開発チームの目に止まるように、質問にタグを付けてください。

    -   {{site.data.keyword.containershort_notm}} を使用したクラスターまたはアプリの開発やデプロイに関する技術的な質問がある場合は、[Stack Overflow![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) に質問を投稿し、`ibm-cloud`、`kubernetes`、`containers` のタグを付けてください。
    -   サービスや概説の説明について質問がある場合は、[IBM developerWorks dW Answers ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) フォーラムを使用してください。 `ibm-cloud` と `containers` のタグを含めてください。
    フォーラムの使用について詳しくは、[ヘルプの取得](/docs/get-support/howtogetsupport.html#using-avatar)を参照してください。

-   チケットを開いて、IBM サポートに連絡してください。 IBM サポート・チケットを開く方法や、サポート・レベルとチケットの重大度については、[サポートへのお問い合わせ](/docs/get-support/howtogetsupport.html#getting-customer-support)を参照してください。

{: tip}
問題を報告する際に、クラスター ID も報告してください。 クラスター ID を取得するには、`bx cs clusters` を実行します。

