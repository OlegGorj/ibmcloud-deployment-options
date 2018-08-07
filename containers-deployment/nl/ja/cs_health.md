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


# ロギングとモニタリング
{: #health}

{{site.data.keyword.containerlong}} でロギングとモニタリングをセットアップすると、問題のトラブルシューティングや、Kubernetes クラスターとアプリの正常性とパフォーマンスの改善に役立ちます。
{: shortdesc}


## クラスターとアプリのログ転送の構成
{: #logging}

{{site.data.keyword.containershort_notm}} の標準 Kubernetes クラスターでは、さまざまなソースから {{site.data.keyword.loganalysislong_notm}}、外部 syslog サーバー、またはその両方にログを転送できます。
{: shortdesc}

あるソースから両方のコレクター・サーバーにログを転送する場合は、2 つのロギング構成を作成する必要があります。
{: tip}

さまざまなログ・ソースについては、以下の表を参照してください。

<table>
<caption>ログ・ソース</caption>
  <thead>
    <tr>
      <th>ログ・ソース</th>
      <th>特性</th>
      <th>ログ・パス</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>container</code></td>
      <td>Kubernetes クラスターで実行されるコンテナーのログ。 STDOUT または STDERR に記録されるものすべて。</td>
      <td> </td>
    </tr>
    <tr>
      <td><code>application</code></td>
      <td>Kubernetes クラスターで実行される独自のアプリケーションのログ。</td>
      <td><p>パスを設定できます。 ログが送信されるようにするには、ロギング構成で絶対パスを使用する必要があります。そうしないと、ログを読み取れません。 パスがワーカー・ノードにマウントされている場合は、シンボリック・リンクが作成されている可能性があります。</p>
      <p>例: 指定されたパスが <code>/usr/local/<b>spark</b>/work/app-0546/0/stderr</code> であるのに、実際にはログが <code>/usr/local/<b>spark-1.0-hadoop-1.2</b>/work/app-0546/0/stderr</code> に送信されている場合、ログは読み取れません。</p></td>
    </tr>
    <tr>
      <td><code>worker</code></td>
      <td>Kubernetes クラスター内の仮想マシン・ワーカー・ノードのログ。</td>
      <td><code>/var/log/syslog</code>、<code>/var/log/auth.log</code></td>
    </tr>
    <tr>
      <td><code>kubernetes</code></td>
      <td>Kubernetes システム構成要素のログ。</td>
      <td><code>/var/log/kubelet.log</code>、<code>/var/log/kube-proxy.log</code>、<code>/var/log/event-exporter/&ast;.log</code></td>
    </tr>
    <tr>
      <td><code>ingress</code></td>
      <td>クラスターへのネットワーク・トラフィックを管理する Ingress アプリケーション・ロード・バランサーのログ。</td>
      <td><code>/var/log/alb/ids/&ast;.log</code>、<code>/var/log/alb/ids/&ast;.err</code>、<code>/var/log/alb/customerlogs/&ast;.log</code>、 <code>/var/log/alb/customerlogs/&ast;.err</code></td>
    </tr>
    <tr>
      <td><code>kube-audit</code></td>
      <td>Kubernetes API サーバーのログ。</td>
      <td> </td>
    </tr>
  </tbody>
</table>

アカウント・レベルでのロギングを有効にしたり、アプリケーション・ロギングを構成したりするには、CLI を使用します。
{: tip}


### GUI を使用したログ転送の有効化
{: #enable-forwarding-ui}

{{site.data.keyword.containershort_notm}} ダッシュボードでロギング構成をセットアップできます。この処理が完了するまで数分かかることがあるため、すぐにログが表示されない場合は、数分待ってから再度確認してください。

1. ダッシュボードの**「概要」**タブにナビゲートします。
2. ログの転送元となる Cloud Foundry 組織とスペースを選択します。ダッシュボードでログ転送を構成すると、クラスターのデフォルトの {{site.data.keyword.loganalysisshort_notm}} エンドポイントにログが送信されます。外部サーバーまたは別の {{site.data.keyword.loganalysisshort_notm}} エンドポイントにログを転送するには、CLI を使用してロギングを構成します。
3. ログの転送元となるログ・ソースを選択します。

    アプリケーション・ロギングまたは特定のコンテナー名前空間を構成するには、CLI を使用してロギング構成をセットアップします。
    {: tip}
4. **「作成」**をクリックします。

### CLI を使用したログ転送の有効化
{: #enable-forwarding}

クラスター・ロギングの構成を作成できます。 さまざまなログ・ソースを、フラグを使用して区別できます。 構成オプションの完全なリストについては、[CLI リファレンス](cs_cli_reference.html#logging_commands)を参照してください。

**始める前に**

1. 権限を確認してください。 クラスターまたはロギング構成の作成時にスペースを指定した場合は、アカウント所有者と {{site.data.keyword.containershort_notm}} 鍵所有者の両方に、そのスペースの管理者権限、開発者権限、または監査員権限が必要です。
  * {{site.data.keyword.containershort_notm}} 鍵所有者が不明な場合は、以下のコマンドを実行します。
      ```
      bx cs api-key-info <cluster_name>
      ```
      {: pre}
  * 権限に対して行った変更を即時に適用するには、以下のコマンドを実行します。
      ```
      bx cs logging-config-refresh <cluster_name>
      ```
      {: pre}

  {{site.data.keyword.containershort_notm}} のアクセス・ポリシーとアクセス権限の変更について詳しくは、[クラスター・アクセス権限の管理](cs_users.html#access_policies)を参照してください。
  {: tip}

2. [CLI の宛先](cs_cli_install.html#cs_cli_configure)を、ログ・ソースがあるクラスターにします。

  専用アカウントを使用している場合は、パブリックの {{site.data.keyword.cloud_notm}} エンドポイントにログインし、ログ転送を有効にするために、パブリックの組織とスペースをターゲットにする必要があります。
  {: tip}

3. ログを syslog に転送するには、以下の 2 つの方法のいずれかで、syslog プロトコルを受け入れるサーバーをセットアップします。
  * 独自のサーバーをセットアップして管理するか、プロバイダーが管理するサーバーを使用します。 プロバイダーがサーバーを管理する場合は、ロギング・プロバイダーからロギング・エンドポイントを取得します。 syslog サーバーが UDP プロトコルを受け入れる必要があります。
  * コンテナーから syslog を実行します。 例えば、この[デプロイメント .yaml ファイル ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/IBM-Cloud/kube-samples/blob/master/deploy-apps-clusters/deploy-syslog-from-kube.yaml) を使用して、Kubernetes クラスター内のコンテナーで実行されている Docker パブリック・イメージをフェッチできます。 このイメージは、パブリック・クラスター IP アドレスのポート `514` を公開し、このパブリック・クラスター IP アドレスを使用して syslog ホストを構成します。

    rsyslog サーバーが実行されている `etc/rsyslog.conf` ファイルの先頭に次のコードを追加することによって、syslog 接頭部を削除してログを有効な JSON として表示できます。</br>
    ```$template customFormat,"%msg%\n"
    $ActionFileDefaultTemplate customFormat
    ```
    {: tip}


**ログの転送**

1. ログ転送構成を作成します。
  ```
  bx cs logging-config-create <cluster_name_or_ID> --logsource <log_source> --namespace <kubernetes_namespace> --hostname <log_server_hostname_or_IP> --port <log_server_port> --type <type> --app-containers <containers> --app-paths <paths_to_logs> --syslog-protocol <protocol> --skip-validation
  ```
  {: pre}

    * デフォルトの名前空間および出力用のコンテナー・ロギング構成例
      ```
      bx cs logging-config-create cluster1 --namespace default
      Creating logging configuration for container logs in cluster cluster1...
      OK
      Id                                     Source      Namespace   Host                                 Port    Org   Space   Protocol   Application Containers   Paths
      af7d1ff4-33e6-4275-8167-b52eb3c5f0ee   container   default     ingest-au-syd.logging.bluemix.net✣  9091✣   -     -       ibm        -                        -

      ✣ Indicates the default endpoint for the {{site.data.keyword.loganalysislong_notm}} service.

      ```
      {: screen}

    * アプリケーション・ロギングの構成と出力の例:
      ```
      bx cs logging-config-create cluster2 --logsource application --app-paths '/var/log/apps.log' --app-containers 'container1,container2,container3'
      Creating logging configuration for application logs in cluster cluster2...
      OK
      Id                                     Source        Namespace   Host                                    Port    Org   Space   Protocol   Application   Containers   Paths
      aa2b415e-3158-48c9-94cf-f8b298a5ae39   application   -           ingest.logging.stage1.ng.bluemix.net✣  9091✣   -     -       ibm        container1,container2,container3   /var/log/apps.log
      ```
      {: screen}

      コンテナー内で実行するアプリを、STDOUT または STDERR にログを書き込むように構成できない場合は、アプリ・ログ・ファイルからログを転送するログ構成を作成できます。
      {: tip}

  <table>
  <caption>このコマンドの構成要素について</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> このコマンドの構成要素について</th>
    </thead>
    <tbody>
      <tr>
        <td><code><em>&lt;cluster_name_or_ID&gt;</em></code></td>
        <td>クラスターの名前または ID。</td>
      </tr>
      <tr>
        <td><code><em>&lt;log_source&gt;</em></code></td>
        <td>ログの転送元になるソース。 指定可能な値は、<code>container</code>、<code>application</code>、<code>worker</code>、<code>kubernetes</code>、<code>ingress</code>、および <code>kube-audit</code> です。</td>
      </tr>
      <tr>
        <td><code><em>&lt;kubernetes_namespace&gt;</em></code></td>
        <td>オプション: ログの転送元になる Kubernetes 名前空間。 ログ転送は、Kubernetes 名前空間 <code>ibm-system</code> と <code>kube-system</code> ではサポートされていません。 この値は、<code>container</code> ログ・ソースについてのみ有効です。 名前空間を指定しないと、クラスター内のすべての名前空間でこの構成が使用されます。</td>
      </tr>
      <tr>
        <td><code><em>&lt;hostname_or_ingestion_URL&gt;</em></code></td>
        <td><p>{{site.data.keyword.loganalysisshort_notm}} の場合は、[取り込み URL](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls)を使用します。 取り込み URL を指定しない場合、クラスターを作成した地域のエンドポイントが使用されます。</p>
        <p>syslog の場合は、ログ・コレクター・サービスのホスト名または IP アドレスを指定します。</p></td>
      </tr>
      <tr>
        <td><code><em>&lt;port&gt;</em></code></td>
        <td>取り込みポート。 ポートを指定しないと、標準ポート <code>9091</code> が使用されます。
        <p>syslog の場合は、ログ・コレクター・サーバーのポートを指定します。 ポートを指定しないと、標準ポート <code>514</code> が使用されます。</td>
      </tr>
      <tr>
        <td><code><em>&lt;cluster_space&gt;</em></code></td>
        <td>オプション: ログの送信先となる Cloud Foundry スペースの名前。 ログを {{site.data.keyword.loganalysisshort_notm}} に転送するとき、スペースと組織は取り込みポイントで指定されます。 スペースを指定しない場合、ログはアカウント・レベルに送信されます。</td>
      </tr>
      <tr>
        <td><code><em>&lt;cluster_org&gt;</em></code></td>
        <td>このスペースが属する Cloud Foundry 組織の名前。 この値は、スペースを指定した場合には必須です。</td>
      </tr>
      <tr>
        <td><code><em>&lt;type&gt;</em></code></td>
        <td>ログの転送先。 オプションは、ログを {{site.data.keyword.loganalysisshort_notm}} に転送する <code>ibm</code> と、ログを外部サーバーに転送する <code>syslog</code> です。</td>
      </tr>
      <tr>
        <td><code><em>&lt;paths_to_logs&gt;</em></code></td>
        <td>アプリがログを書き込むコンテナー上のパス。 ソース・タイプが <code>application</code> のログを転送するには、パスを指定する必要があります。 複数のパスを指定するには、コンマ区切りリストを使用します。 例: <code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></td>
      </tr>
      <tr>
        <td><code><em>&lt;containers&gt;</em></code></td>
        <td>オプション: アプリのログを転送するには、アプリが含まれているコンテナーの名前を指定します。 コンマ区切りのリストを使用して複数のコンテナーを指定できます。 コンテナーを指定しない場合は、指定したパスが入っているすべてのコンテナーのログが転送されます。</td>
      </tr>
      <tr>
        <td><code><em>&lt;protocol&gt;</em></code></td>
        <td>ロギング・タイプが <code>syslog</code> の場合は、トランスポート層プロトコル。サポートされる値は、<code>TCP</code> とデフォルトの <code>UDP</code> です。<code>udp</code> プロトコルを使用して rsyslog サーバーに転送する場合、1KB を超えるログは切り捨てられます。</td>
      </tr>
      <tr>
        <td><code><em>--skip-validation</em></code></td>
        <td>オプション: 組織名とスペース名が指定されている場合にそれらの検証をスキップします。 検証をスキップすると処理時間は短縮されますが、ロギング構成が無効な場合、ログは正しく転送されません。</td>
      </tr>
    </tbody>
  </table>

2. 以下の 2 つの方法のいずれかで、構成が正しいことを確認します。

    * クラスター内のすべてのロギング構成をリスト表示する場合には、以下のようにします。
      ```
      bx cs logging-config-get <cluster_name_or_ID>
      ```
      {: pre}

      出力例:

      ```
      Id                                    Source       Namespace     Host                          Port   Org      Space      Protocol     Paths
      f4bc77c0-ee7d-422d-aabf-a4e6b977264e  kubernetes   -             172.30.xxx.xxx                5514   -        -          syslog       /var/log/kubelet.log,/var/log/kube-proxy.log
      5bd9c609-13c8-4c48-9d6e-3a6664c825a9  application  -             ingest.logging.ng.bluemix.net 9091   my_org   my_space   ibm          /var/log/apps/**/*.log,/var/log/apps/**/*.err
      8a284f1a-451c-4c48-b1b4-a4e6b977264e  containers   my-namespace  myhostname.common             5514   -        -          syslog       -
      ```
      {: screen}

    * 1 つのタイプのログ・ソースのロギング構成をリストする場合には、以下のようにします。
      ```
      bx cs logging-config-get <cluster_name_or_ID> --logsource worker
      ```
      {: pre}

      出力例:

      ```
      Id                                    Source    Namespace   Host                            Port   Org    Space     Protocol    Paths
      f4bc77c0-ee7d-422d-aabf-a4e6b977264e  worker    -           ingest.logging.ng.bluemix.net   9091   -      -         ibm         /var/log/syslog,/var/log/auth.log
      5bd9c609-13c8-4c48-9d6e-3a6664c825a9  worker    -           172.30.xxx.xxx                  5514   -      -         syslog      /var/log/syslog,/var/log/auth.log
      ```
      {: screen}

## ログ転送の更新
{: #updating-forwarding}

1. ログ転送構成を更新します。
    ```
    bx cs logging-config-update <cluster_name_or_ID> <log_config_id> --namespace <namespace> --type <log_type> --logsource <source> --hostname <hostname_or_ingestion_URL> --port <port> --space <cluster_space> --org <cluster_org> --app-containers <containers> --app-paths <paths_to_logs>
    ```
    {: pre}

  <table>
  <caption>このコマンドの構成要素について</caption>
  <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> このコマンドの構成要素について</th>
  </thead>
  <tbody>
    <tr>
      <td><code><em>&lt;cluster_name_or_ID&gt;</em></code></td>
      <td>クラスターの名前または ID。</td>
    </tr>
    <tr>
      <td><code><em>&lt;log_config_id&gt;</em></code></td>
      <td>更新する構成の ID。</td>
    </tr>
    <tr>
      <td><code><em>&lt;namespace&gt;</em></code></td>
      <td>オプション: ログの転送元の Kubernetes 名前空間。ログ転送は、Kubernetes 名前空間 <code>ibm-system</code> と <code>kube-system</code> ではサポートされていません。 この値は、<code>container</code> ログ・ソースについてのみ有効です。 名前空間を指定しないと、クラスター内のすべての名前空間で構成が使用されます。</td>
    </tr>
    <tr>
      <td><code><em>&lt;log_type&gt;</em></code></td>
      <td>ログの転送先。 オプションは、ログを {{site.data.keyword.loganalysisshort_notm}} に転送する <code>ibm</code> と、ログを外部サーバーに転送する <code>syslog</code> です。</td>
    </tr>
    <tr>
      <td><code><em>&lt;hostname_or_ingestion_URL&gt;</em></code></td>
      <td><p>{{site.data.keyword.loganalysisshort_notm}} の場合は、[取り込み URL](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls)を使用します。 取り込み URL を指定しない場合、クラスターを作成した地域のエンドポイントが使用されます。</p>
      <p>syslog の場合は、ログ・コレクター・サービスのホスト名または IP アドレスを指定します。</p></td>
    </tr>
    <tr>
      <td><code><em>&lt;port&gt;</em></code></td>
      <td>取り込みポート。 ポートを指定しないと、標準ポート <code>9091</code> が使用されます。
      <p>syslog の場合は、ログ・コレクター・サーバーのポートを指定します。 ポートを指定しないと、標準ポート <code>514</code> が使用されます。</td>
    </tr>
    <tr>
      <td><code><em>&lt;cluster_space&gt;</em></code></td>
      <td>オプション: ログの送信先となる Cloud Foundry スペースの名前。 ログを {{site.data.keyword.loganalysisshort_notm}} に転送するとき、スペースと組織は取り込みポイントで指定されます。 スペースを指定しない場合、ログはアカウント・レベルに送信されます。</td>
    </tr>
    <tr>
      <td><code><em>&lt;cluster_org&gt;</em></code></td>
      <td>このスペースが属する Cloud Foundry 組織の名前。 この値は、スペースを指定した場合には必須です。</td>
    </tr>
    <tr>
      <td><code><em>&lt;paths_to_logs&gt;</em></code></td>
      <td>アプリがログを書き込むコンテナー上のパス。 ソース・タイプが <code>application</code> のログを転送するには、パスを指定する必要があります。 複数のパスを指定するには、コンマ区切りリストを使用します。 例: <code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></td>
    </tr>
    <tr>
      <td><code><em>&lt;containers&gt;</em></code></td>
      <td>オプション: アプリのログを転送するには、アプリが含まれているコンテナーの名前を指定します。 コンマ区切りのリストを使用して複数のコンテナーを指定できます。 コンテナーを指定しない場合は、指定したパスが入っているすべてのコンテナーのログが転送されます。</td>
    </tr>
  </tbody>
  </table>

<br />



## ログのフィルタリング
{: #filter-logs}

一定期間における特定のログをフィルターで除外して、転送するログを選択することができます。

1. ロギング・フィルターを作成します。
  ```
  bx cs logging-filter-create <cluster_name_or_ID> --type <log_type> --logging-configs <configs> --namespace <kubernetes_namespace> --container <container_name> --level <logging_level> --regex-message <message>
  ```
  {: pre}
  <table>
  <caption>このコマンドの構成要素について</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> このコマンドの構成要素について</th>
    </thead>
    <tbody>
      <tr>
        <td>&lt;cluster_name_or_ID&gt;</td>
        <td>必須: ロギング・フィルターを作成するクラスターの名前または ID。</td>
      </tr>
      <tr>
        <td><code>&lt;log_type&gt;</code></td>
        <td>フィルターを適用するログのタイプ。 現在、<code>all</code>、<code>container</code>、<code>host</code> がサポートされています。</td>
      </tr>
      <tr>
        <td><code>&lt;configs&gt;</code></td>
        <td>オプション: ロギング構成 ID のコンマ区切りのリスト。 指定しない場合、フィルターに渡されたすべてのクラスター・ロギング構成にフィルターが適用されます。 フィルターと一致するログ構成を表示するには、コマンドに <code>--show-matching-configs</code> フラグを使用します。</td>
      </tr>
      <tr>
        <td><code>&lt;kubernetes_namespace&gt;</code></td>
        <td>オプション: ログの転送元になる Kubernetes 名前空間。 このフラグは、ログ・タイプ <code>container</code> を使用している場合にのみ、適用されます。</td>
      </tr>
      <tr>
        <td><code>&lt;container_name&gt;</code></td>
        <td>オプション: ログをフィルタリングするコンテナーの名前。</td>
      </tr>
      <tr>
        <td><code>&lt;logging_level&gt;</code></td>
        <td>オプション: 指定したレベル以下のログをフィルターで除外します。 指定できる値は、規定の順に <code>fatal</code>、<code>error</code>、<code>warn/warning</code>、<code>info</code>、<code>debug</code>、<code>trace</code> です。 例えば、<code>info</code> レベルのログをフィルタリングした場合、<code>debug</code> と <code>trace</code> もフィルタリングされます。 **注**: このフラグは、ログ・メッセージが JSON 形式で、レベル・フィールドを含んでいる場合にのみ使用できます。 JSON 形式でメッセージを表示するには、<code>--json</code> フラグをコマンドに付加します。</td>
      </tr>
      <tr>
        <td><code>&lt;メッセージ&gt;</code></td>
        <td>オプション: 正規表現で記述された指定メッセージが含まれるログをフィルターで除外します。</td>
      </tr>
    </tbody>
  </table>

2. 作成したログ・フィルターを表示します。

  ```
  bx cs logging-filter-get <cluster_name_or_ID> --id <filter_ID> --show-matching-configs
  ```
  {: pre}
  <table>
  <caption>このコマンドの構成要素について</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> このコマンドの構成要素について</th>
    </thead>
    <tbody>
      <tr>
        <td>&lt;cluster_name_or_ID&gt;</td>
        <td>必須: ロギング・フィルターを作成するクラスターの名前または ID。</td>
      </tr>
      <tr>
        <td><code>&lt;filter_ID&gt;</code></td>
        <td>オプション: 表示するログ・フィルターの ID。</td>
      </tr>
      <tr>
        <td><code>--show-matching-configs</code></td>
        <td>各フィルターが適用されるロギング構成を表示します。</td>
      </tr>
    </tbody>
  </table>

3. 作成したログ・フィルターを更新します。
  ```
  bx cs logging-filter-update <cluster_name_or_ID> --id <filter_ID> --type <log_type> --logging-configs <configs> --namespace <kubernetes_namespace --container <container_name> --level <logging_level> --regex-message <message>
  ```
  {: pre}
  <table>
  <caption>このコマンドの構成要素について</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> このコマンドの構成要素について</th>
    </thead>
    <tbody>
      <tr>
        <td>&lt;cluster_name_or_ID&gt;</td>
        <td>必須: ロギング・フィルターを更新するクラスターの名前または ID。</td>
      </tr>
      <tr>
        <td><code>&lt;filter_ID&gt;</code></td>
        <td>更新するログ・フィルターの ID。</td>
      </tr>
      <tr>
        <td><code><&lt;log_type&gt;</code></td>
        <td>フィルターを適用するログのタイプ。 現在、<code>all</code>、<code>container</code>、<code>host</code> がサポートされています。</td>
      </tr>
      <tr>
        <td><code>&lt;configs&gt;</code></td>
        <td>オプション: フィルターを適用するすべてのロギング構成 ID のコンマ区切りリスト。 指定しない場合、フィルターに渡されたすべてのクラスター・ロギング構成にフィルターが適用されます。 フィルターと一致するログ構成を表示するには、<code>bx cs logging-filter-get</code> コマンドに <code>--show-matching-configs</code> フラグを使用します。</td>
      </tr>
      <tr>
        <td><code>&lt;kubernetes_namespace&gt;</code></td>
        <td>オプション: ログの転送元になる Kubernetes 名前空間。 このフラグは、ログ・タイプ <code>container</code> を使用している場合にのみ、適用されます。</td>
      </tr>
      <tr>
        <td><code>&lt;container_name&gt;</code></td>
        <td>オプション: ログをフィルタリングするコンテナーの名前。 このフラグは、ログ・タイプ <code>container</code> を使用している場合にのみ、適用されます。</td>
      </tr>
      <tr>
        <td><code>&lt;logging_level&gt;</code></td>
        <td>オプション: 指定したレベル以下のログをフィルターで除外します。 指定できる値は、規定の順に <code>fatal</code>、<code>error</code>、<code>warn/warning</code>、<code>info</code>、<code>debug</code>、<code>trace</code> です。 例えば、<code>info</code> レベルのログをフィルタリングした場合、<code>debug</code> と <code>trace</code> もフィルタリングされます。 **注**: このフラグは、ログ・メッセージが JSON 形式で、レベル・フィールドを含んでいる場合にのみ使用できます。</td>
      </tr>
      <tr>
        <td><code>&lt;メッセージ&gt;</code></td>
        <td>オプション: 正規表現で記述された指定メッセージが含まれるログをフィルターで除外します。</td>
      </tr>
    </tbody>
  </table>

4. 作成したログ・フィルターを削除します。

  ```
  bx cs logging-filter-rm <cluster_name_or_ID> --id <filter_ID> [--all]
  ```
  {: pre}
  <table>
  <caption>このコマンドの構成要素について</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> このコマンドの構成要素について</th>
    </thead>
    <tbody>
      <tr>
        <td><code>&lt;cluster_name_or_ID&gt;</code></td>
        <td>必須: ロギング・フィルターを削除するクラスターの名前または ID。</td>
      </tr>
      <tr>
        <td><code>&lt;filter_ID&gt;</code></td>
        <td>オプション: 削除するログ・フィルターの ID。</td>
      </tr>
      <tr>
        <td><code>--all</code></td>
        <td>オプション: すべてのログ転送フィルターを削除します。</td>
      </tr>
    </tbody>
  </table>

<br />




## ログの表示
{: #view_logs}

クラスターとコンテナーのログを表示するには、Kubernetes と Docker の標準的なロギング機能を使用します。
{:shortdesc}

### {{site.data.keyword.loganalysislong_notm}}
{: #view_logs_k8s}

Kibana ダッシュボードで、{{site.data.keyword.loganalysislong_notm}} に転送したログを参照できます。
{: shortdesc}

デフォルト値を使用して構成ファイルを作成した場合は、クラスターが作成されたアカウント (または組織とスペース) にログがあります。 構成ファイルに組織とスペースを指定した場合は、そのスペースにログがあります。 ロギングについて詳しくは、[{{site.data.keyword.containershort_notm}} のロギング](/docs/services/CloudLogAnalysis/containers/containers_kubernetes.html#containers_kubernetes)を参照してください。

Kibana ダッシュボードにアクセスするには、以下のいずれかの URL にアクセスし、クラスターのログ転送を構成した {{site.data.keyword.Bluemix_notm}} アカウントまたはスペースを選択します。
- 米国南部および米国東部: https://logging.ng.bluemix.net
- 英国南部: https://logging.eu-gb.bluemix.net
- EU 中央: https://logging.eu-fra.bluemix.net
- 南アジア太平洋地域: https://logging.au-syd.bluemix.net

ログの表示について詳しくは、[Web ブラウザーから Kibana へのナビゲート](/docs/services/CloudLogAnalysis/kibana/launch.html#launch_Kibana_from_browser)を参照してください。

### Docker ログ
{: #view_logs_docker}

組み込みの Docker ロギング機能を活用して、標準の STDOUT と STDERR 出力ストリームのアクティビティーを検討することができます。 詳しくは、[Kubernetes クラスターで実行されるコンテナーのコンテナー・ログの表示](/docs/services/CloudLogAnalysis/containers/containers_kubernetes.html#containers_kubernetes)を参照してください。

<br />



## ログ転送の停止
{: #log_sources_delete}

クラスターの 1 つまたはすべてのロギング構成によるログの転送を停止できます。
{: shortdesc}

1. [CLI の宛先](cs_cli_install.html#cs_cli_configure)を、ログ・ソースがあるクラスターにします。

2. ロギング構成を削除します。
<ul>
<li>1 つのロギング構成を削除するには、以下のようにします。</br>
  <pre><code>bx cs logging-config-rm &lt;cluster_name_or_ID&gt; --id &lt;log_config_ID&gt;</pre></code>
  <table>
  <caption>このコマンドの構成要素について</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> このコマンドの構成要素について</th>
    </thead>
    <tbody>
      <tr>
        <td><code><em>&lt;cluster_name_or_ID&gt;</em></code></td>
        <td>ロギング構成が含まれているクラスターの名前。</td>
      </tr>
      <tr>
        <td><code><em>&lt;log_config_ID&gt;</em></code></td>
        <td>ログ・ソース構成の ID。</td>
      </tr>
    </tbody>
  </table></li>
<li>すべてのロギング構成を削除するには、以下のようにします。</br>
  <pre><code>bx cs logging-config-rm <my_cluster> --all</pre></code></li>
</ul>

<br />



## Kubernetes API 監査ログのログ転送の構成
{: #api_forward}

Kubernetes は、apiserver を通るすべてのイベントを自動的に監査します。{{site.data.keyword.loganalysisshort_notm}} または外部サーバーにイベントを転送できます。
{: shortdesc}


Kubernetes 監査ログについて詳しくは、Kubernetes 資料の<a href="https://kubernetes.io/docs/tasks/debug-application-cluster/audit/" target="blank">監査トピック <img src="../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を参照してください。

* Kubernetes API 監査ログの転送は、Kubernetes バージョン 1.7 以降でのみサポートされています。
* 現在は、このロギング構成のすべてのクラスターで、デフォルトの監査ポリシーが使用されます。
* 現在、フィルターはサポートされていません。
* クラスターごとに 1 つの `kube-audit` 構成しか設定できませんが、ロギング構成と Webhook を作成することで、{{site.data.keyword.loganalysisshort_notm}} と外部サーバーにログを転送できます。
{: tip}


### {{site.data.keyword.loganalysisshort_notm}} への監査ログの送信
{: #audit_enable_loganalysis}

Kubernetes API サーバーの監査ログを {{site.data.keyword.loganalysisshort_notm}} に転送できます。

**始める前に**

1. 権限を確認してください。 クラスターまたはロギング構成の作成時にスペースを指定した場合は、アカウント所有者と {{site.data.keyword.containershort_notm}} 鍵所有者の両方に、そのスペースの管理者権限、開発者権限、または監査員権限が必要です。

2. [CLI のターゲット](cs_cli_install.html#cs_cli_configure)を、API サーバー監査ログの収集対象となるクラスターに設定します。 **注**: 専用アカウントを使用している場合は、パブリックの {{site.data.keyword.cloud_notm}} エンドポイントにログインし、ログ転送を有効にするために、パブリックの組織とスペースをターゲットにする必要があります。

**ログの転送**

1. ロギング構成を作成します。

    ```
    bx cs logging-config-create <cluster_name_or_ID> --logsource kube-audit --space <cluster_space> --org <cluster_org> --hostname <ingestion_URL> --type ibm
    ```
    {: pre}

    コマンドと出力の例:

    ```
    bx cs logging-config-create myCluster --logsource kube-audit
    Creating logging configuration for kube-audit logs in cluster myCluster...
    OK
    Id                                     Source      Namespace   Host                                 Port    Org   Space   Protocol   Application Containers   Paths
    14ca6a0c-5bc8-499a-b1bd-cedcf40ab850   kube-audit  -           ingest-au-syd.logging.bluemix.net✣   9091✣   -     -       ibm        -                        -

    ✣ Indicates the default endpoint for the {{site.data.keyword.loganalysisshort_notm}} service.

    ```
    {: screen}

    <table>
    <caption>このコマンドの構成要素について</caption>
      <thead>
        <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> このコマンドの構成要素について</th>
      </thead>
      <tbody>
        <tr>
          <td><code><em>&lt;cluster_name_or_ID&gt;</em></code></td>
          <td>クラスターの名前または ID。</td>
        </tr>
        <tr>
          <td><code><em>&lt;ingestion_URL&gt;</em></code></td>
          <td>ログの転送先となるエンドポイント。[取り込み URL](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls) を指定しない場合、クラスターを作成した地域のエンドポイントが使用されます。</td>
        </tr>
        <tr>
          <td><code><em>&lt;cluster_space&gt;</em></code></td>
          <td>オプション: ログの送信先となる Cloud Foundry スペースの名前。 ログを {{site.data.keyword.loganalysisshort_notm}} に転送するとき、スペースと組織は取り込みポイントで指定されます。 スペースを指定しない場合、ログはアカウント・レベルに送信されます。</td>
        </tr>
        <tr>
          <td><code><em>&lt;cluster_org&gt;</em></code></td>
          <td>このスペースが属する Cloud Foundry 組織の名前。 この値は、スペースを指定した場合には必須です。</td>
        </tr>
      </tbody>
    </table>

2. クラスター・ロギング構成を表示して、意図したとおりに実装されたことを確認します。

    ```
    bx cs logging-config-get <cluster_name_or_ID>
    ```
    {: pre}

    コマンドと出力の例:
    ```
    bx cs logging-config-get myCluster
    Retrieving cluster myCluster logging configurations...
    OK
    Id                                     Source        Namespace   Host                                 Port    Org   Space   Protocol   Application Containers   Paths
    a550d2ba-6a02-4d4d-83ef-68f7a113325c   container     *           ingest-au-syd.logging.bluemix.net✣   9091✣   -     -       ibm        -                        -
    14ca6a0c-5bc8-499a-b1bd-cedcf40ab850   kube-audit    -           ingest-au-syd.logging.bluemix.net✣   9091✣   -     -       ibm        -                    
    ```
    {: screen}

  <table>
  <caption>このコマンドの構成要素について</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> このコマンドの構成要素について</th>
    </thead>
    <tbody>
      <tr>
        <td><code><em>&lt;cluster_name_or_ID&gt;</em></code></td>
        <td>クラスターの名前または ID。</td>
      </tr>
    </tbody>
  </table>

3. オプション: 監査ログの転送を停止するには、[構成を削除](#log_sources_delete)します。

<br />



### 外部サーバーへの監査ログの送信
{: #audit_enable}

**始める前に**

1. ログを転送できるリモート・ロギング・サーバーをセットアップします。 例えば、[Logstash と Kubernetes を使用して ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/tasks/debug-application-cluster/audit/#use-logstash-to-collect-and-distribute-audit-events-from-webhook-backend)、監査イベントを収集できます。

2. [CLI のターゲット](cs_cli_install.html#cs_cli_configure)を、API サーバー監査ログの収集対象となるクラスターに設定します。 **注**: 専用アカウントを使用している場合は、パブリックの {{site.data.keyword.cloud_notm}} エンドポイントにログインし、ログ転送を有効にするために、パブリックの組織とスペースをターゲットにする必要があります。

Kubernetes API 監査ログを転送するには、以下のようにします。

1. Webhook をセットアップします。どのフラグにも情報を指定しない場合、デフォルトの構成が使用されます。

    ```
    bx cs apiserver-config-set audit-webhook <cluster_name_or_ID> --remoteServer <server_URL_or_IP> --caCert <CA_cert_path> --clientCert <client_cert_path> --clientKey <client_key_path>
    ```
    {: pre}

  <table>
  <caption>このコマンドの構成要素について</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> このコマンドの構成要素について</th>
    </thead>
    <tbody>
      <tr>
        <td><code><em>&lt;cluster_name_or_ID&gt;</em></code></td>
        <td>クラスターの名前または ID。</td>
      </tr>
      <tr>
        <td><code><em>&lt;server_URL&gt;</em></code></td>
        <td>ログの送信先となるリモート・ロギング・サービスの URL または IP アドレス。 非セキュアなサーバー URL を指定すると、証明書は無視されます。</td>
      </tr>
      <tr>
        <td><code><em>&lt;CA_cert_path&gt;</em></code></td>
        <td>リモート・ロギング・サービスの検証に使用される CA 証明書のファイル・パス。</td>
      </tr>
      <tr>
        <td><code><em>&lt;client_cert_path&gt;</em></code></td>
        <td>リモート・ロギング・サービスに対する認証に使用されるクライアント証明書のファイル・パス。</td>
      </tr>
      <tr>
        <td><code><em>&lt;client_key_path&gt;</em></code></td>
        <td>リモート・ロギング・サービスへの接続に使用される、対応するクライアント・キーのファイル・パス。</td>
      </tr>
    </tbody>
  </table>

2. リモート・ロギング・サービスの URL を参照して、ログ転送が有効化されていることを確認します。

    ```
    bx cs apiserver-config-get audit-webhook <cluster_name_or_ID>
    ```
    {: pre}

    出力例:
    ```
    OK
    Server:			https://8.8.8.8
    ```
    {: screen}

3. Kubernetes マスターを再始動して、構成の更新を適用します。

    ```
    bx cs apiserver-refresh <cluster_name_or_ID>
    ```
    {: pre}

4. オプション: 監査ログの転送を停止するには、構成を無効にします。
    1. [CLI のターゲット](cs_cli_install.html#cs_cli_configure)を、API サーバー監査ログの収集停止対象となるクラスターにします。
    2. クラスターの API サーバーの Web フック・バックエンド構成を無効にします。

        ```
        bx cs apiserver-config-unset audit-webhook <cluster_name_or_ID>
        ```
        {: pre}

    3. Kubernetes マスターを再始動して、構成の更新を適用します。

        ```
        bx cs apiserver-refresh <cluster_name_or_ID>
        ```
        {: pre}

## メトリックの表示
{: #view_metrics}

メトリックは、クラスターの正常性とパフォーマンスをモニターするのに役立ちます。 Kubernetes と Docker の標準機能を使用して、クラスターとアプリの正常性をモニターできます。 **注**: モニタリングは標準クラスターでのみサポートされています。
{:shortdesc}

<dl>
  <dt>{{site.data.keyword.Bluemix_notm}} のクラスター詳細ページ</dt>
    <dd>{{site.data.keyword.containershort_notm}} には、クラスターの正常性と能力、そしてクラスター・リソースの使用方法に関する情報が表示されます。 この GUI を使用して、クラスターのスケールアウト、永続ストレージの作業、{{site.data.keyword.Bluemix_notm}} サービス・バインディングによるクラスターへの機能の追加を行うことができます。 クラスターの詳細ページを表示するには、**{{site.data.keyword.Bluemix_notm}} の「ダッシュボード」**に移動して、クラスターを選択します。</dd>
  <dt>Kubernetes ダッシュボード</dt>
    <dd>Kubernetes ダッシュボードは、ワーカー・ノードの正常性の確認、Kubernetes リソースの検索、コンテナー化アプリのデプロイ、ロギングとモニタリング情報を使用したアプリのトラブルシューティングを行える管理 Web インターフェースです。 Kubernetes ダッシュボードにアクセスする方法について詳しくは、[{{site.data.keyword.containershort_notm}} での Kubernetes ダッシュボードの起動](cs_app.html#cli_dashboard)を参照してください。</dd>
  <dt>{{site.data.keyword.monitoringlong_notm}}</dt>
    <dd><p>標準クラスターのメトリックは、Kubernetes クラスターを作成したときにログインした {{site.data.keyword.Bluemix_notm}} アカウントにあります。 クラスターの作成時に {{site.data.keyword.Bluemix_notm}} スペースを指定した場合、メトリックはそのスペースに配置されます。 コンテナーのメトリックはクラスターにデプロイされたすべてのコンテナーについて自動的に収集されます。 これらのメトリックが送信され、Grafana で使用できるようになります。 メトリックについて詳しくは、[{{site.data.keyword.containershort_notm}} のモニター](/docs/services/cloud-monitoring/containers/monitoring_containers_ov.html#monitoring_bmx_containers_ov)を参照してください。</p>
    <p>Grafana ダッシュボードにアクセスするには、以下のいずれかの URL にアクセスし、クラスターを作成した {{site.data.keyword.Bluemix_notm}} アカウントまたはスペースを選択します。
      <ul>
        <li>米国南部と米国東部: https://metrics.ng.bluemix.net</li>
        <li>英国南部: https://metrics.eu-gb.bluemix.net</li>
        <li>EU 中央: https://metrics.eu-de.bluemix.net</li>
      </ul></p></dd>
</dl>

### その他のヘルス・モニター・ツール
{: #health_tools}

他のツールを構成してモニター機能を追加することができます。
<dl>
  <dt>Prometheus</dt>
    <dd>Prometheus は、Kubernetes のために設計されたモニタリング、ロギング、アラートのためのオープン・ソースのツールです。 このツールは、Kubernetes のロギング情報に基づいてクラスター、ワーカー・ノード、デプロイメントの正常性に関する詳細情報を取得します。 セットアップ方法について詳しくは、[{{site.data.keyword.containershort_notm}} とのサービスの統合](cs_integrations.html#integrations)を参照してください。</dd>
</dl>

<br />


## Autorecovery を使用したワーカー・ノードの正常性モニタリングの構成
{: #autorecovery}

{{site.data.keyword.containerlong_notm}} Autorecovery システムは、Kubernetes バージョン 1.7 以降の既存のクラスターにデプロイできます。
{: shortdesc}

Autorecovery システムは、さまざまな検査機能を使用してワーカー・ノードの正常性状況を照会します。 Autorecovery は、構成された検査に基づいて正常でないワーカー・ノードを検出すると、ワーカー・ノードの OS の再ロードのような修正アクションをトリガーします。 修正アクションは、一度に 1 つのワーカー・ノードに対してのみ実行されます。 1 つのワーカー・ノードの修正アクションが正常に完了してからでないと、別のワーカー・ノードの修正アクションは実行されません。 詳しくは、[Autorecovery に関するブログ投稿 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/) を参照してください。</br> </br>
**注**: Autorecovery が正常に機能するためには、1 つ以上の正常なノードが必要です。 Autorecovery でのアクティブな検査は、複数のワーカー・ノードが存在するクラスターでのみ構成します。

始めに、[CLI のターゲット](cs_cli_install.html#cs_cli_configure)を、ワーカー・ノードの状況を検査するクラスターに設定してください。

1. [クラスター用の Helm をインストールし、Helm インスタンスに {{site.data.keyword.Bluemix_notm}} リポジトリーを追加します](cs_integrations.html#helm)。

2. 検査を JSON 形式で定義する構成マップ・ファイルを作成します。 例えば、次の YAML ファイルでは、3 つの検査 (1 つの HTTP 検査と 2 つの Kubernetes API サーバー検査) を定義しています。 この 3 種類の検査とそれらの検査の個々の構成要素については、YAML ファイルの例に続く表を参照してください。
</br>
   **ヒント:** 構成マップの `data` セクションに、各検査を固有キーとして定義します。

   ```
   kind: ConfigMap
   apiVersion: v1
   metadata:
     name: ibm-worker-recovery-checks
     namespace: kube-system
   data:
     checknode.json: |
       {
         "Check":"KUBEAPI",
          "Resource":"NODE",
          "FailureThreshold":3,
          "CorrectiveAction":"RELOAD",
          "CooloffSeconds":1800,
          "IntervalSeconds":180,
          "TimeoutSeconds":10,
          "Enabled":true
        }
      checkpod.json: |
        {
         "Check":"KUBEAPI",
         "Resource":"POD",
         "PodFailureThresholdPercent":50,
         "FailureThreshold":3,
         "CorrectiveAction":"RELOAD",
         "CooloffSeconds":1800,
         "IntervalSeconds":180,
         "TimeoutSeconds":10,
         "Enabled":true
       }
     checkhttp.json: |
       {
         "Check":"HTTP",
         "FailureThreshold":3,
         "CorrectiveAction":"REBOOT",
         "CooloffSeconds":1800,
         "IntervalSeconds":180,
         "TimeoutSeconds":10,
         "Port":80,
         "ExpectedStatus":200,
         "Route":"/myhealth",
         "Enabled":false
       }
   ```
   {:codeblock}

   <table summary="構成マップの構成要素について">
   <caption>構成マップの構成要素について</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>構成マップの構成要素について</th>
   </thead>
   <tbody>
   <tr>
   <td><code>name</code></td>
   <td>構成名 <code>ibm-worker-recovery-checks</code> は定数であり、変更することはできません。</td>
   </tr>
   <tr>
   <td><code>namespace</code></td>
   <td><code>kube-system</code> 名前空間は定数であり、変更することはできません。</td>
   </tr>
   <tr>
   <td><code>checknode.json</code></td>
   <td>各ワーカー・ノードが <code>Ready</code> 状態であるかどうかを検査する Kubernetes API ノード検査を定義します。 特定のワーカー・ノードが<code>Ready</code> 状態でない場合、そのワーカー・ノードの検査結果は失敗となります。 サンプル YAML では、検査が 3 分ごとに実行されます。 連続して 3 回失敗すると、ワーカー・ノードは再ロードされます。 この操作は、<code>bx cs worker-reload</code> を実行することと同等です。<br></br><b>「有効」</b>フィールドを <code>false</code> に設定するか、または検査を除去するまで、ノード検査は有効になります。</td>
   </tr>
   <tr>
   <td><code>checkpod.json</code></td>
   <td>
   ワーカー・ノード上の <code>NotReady</code> ポッドの合計パーセンテージを、そのワーカー・ノードに割り当てられた合計ポッド数に基づいて検査する、Kubernetes API ポッド検査を定義します。 <code>NotReady</code> ポッドの合計パーセンテージが、定義済みの <code>PodFailureThresholdPercent</code> より大きい場合、そのワーカー・ノードの検査結果は失敗となります。 サンプル YAML では、検査が 3 分ごとに実行されます。 連続して 3 回失敗すると、ワーカー・ノードは再ロードされます。 この操作は、<code>bx cs worker-reload</code> を実行することと同等です。 例えば、デフォルトの <code>PodFailureThresholdPercent</code> は 50% です。<code>NotReady</code> ポッドの割合が連続 3 回 50% を超えると、ワーカー・ノードが再ロードされます。<br></br>デフォルトでは、すべての名前空間のポッドが検査されます。 指定した名前空間内のポッドのみに検査を限定するには、<code>「名前空間」</code>フィールドを検査に追加します。 <b>「有効」</b>フィールドを <code>false</code> に設定するか、または検査を除去するまで、ポッド検査は有効になります。
   </td>
   </tr>
   <tr>
   <td><code>checkhttp.json</code></td>
   <td>ワーカー・ノードで稼働している HTTP サーバーが正常かどうかを検査する HTTP 検査を定義します。 この検査を使用するには、[DaemonSet ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) を使用して、クラスター内のすべてのワーカー・ノードに HTTP サーバーをデプロイする必要があります。 HTTP サーバーが正常かどうかを検証できるヘルス・チェックを、<code>/myhealth</code> パスで利用できるように実装する必要があります。 他のパスを定義するには、<strong>Route</strong> パラメーターを変更します。 HTTP サーバーが正常である場合は、<strong>ExpectedStatus</strong> で定義されている HTTP 応答コードを返す必要があります。 HTTP サーバーは、ワーカー・ノードのプライベート IP アドレスで listen するように構成する必要があります。 プライベート IP アドレスを調べるには、<code>kubectl get nodes</code> ノードを実行します。<br></br>
   例えば、プライベート IP アドレスが 10.10.10.1 および 10.10.10.2 の 2 つのノードがクラスターにあるとします。 この例では、<code>http://10.10.10.1:80/myhealth</code> および <code>http://10.10.10.2:80/myhealth</code> の 2 つのルートの 200 HTTP 応答を検査します。
   サンプル YAML では、検査が 3 分ごとに実行されます。 連続して 3 回失敗すると、ワーカー・ノードはリブートされます。 この操作は、<code>bx cs worker-reboot</code> を実行することと同等です。<br></br><b>「有効」</b>フィールドを <code>true</code> に設定するまで、HTTP 検査は無効になります。</td>
   </tr>
   </tbody>
   </table>

   <table summary="検査の個々の構成要素について">
   <caption>検査の個々の構成要素について</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>検査の個々の構成要素について</th>
   </thead>
   <tbody>
   <tr>
   <td><code>Check</code></td>
   <td>Autorecovery で使用する検査のタイプを入力します。 <ul><li><code>HTTP</code>: Autorecovery は、各ノードで稼働する HTTP サーバーを呼び出して、ノードが正常に稼働しているかどうかを判別します。</li><li><code>KUBEAPI</code>: Autorecovery は、Kubernetes API サーバーを呼び出し、ワーカー・ノードによって報告された正常性状況データを読み取ります。</li></ul></td>
   </tr>
   <tr>
   <td><code>リソース</code></td>
   <td>検査タイプが <code>KUBEAPI</code> である場合は、Autorecovery で検査するリソースのタイプを入力します。 受け入れられる値は <code>NODE</code> または <code>POD</code> です。</td>
   </tr>
   <tr>
   <td><code>FailureThreshold</code></td>
   <td>検査の連続失敗数のしきい値を入力します。 このしきい値に達すると、Autorecovery が、指定された修正アクションをトリガーします。 例えば、値が 3 である場合に、Autorecovery で構成された検査が 3 回連続して失敗すると、Autorecovery は検査に関連付けられている修正アクションをトリガーします。</td>
   </tr>
   <tr>
   <td><code>PodFailureThresholdPercent</code></td>
   <td>リソース・タイプが <code>POD</code> である場合は、[NotReady ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/#define-readiness-probes) 状態になることができるワーカー・ノード上のポッドのしきい値 (パーセンテージ) を入力します。 このパーセンテージは、ワーカー・ノードにスケジュールされているポッドの合計数に基づきます。 検査の結果、正常でないポッドのパーセンテージがしきい値を超えていることが判別されると、1 回の失敗としてカウントされます。</td>
   </tr>
   <tr>
   <td><code>CorrectiveAction</code></td>
   <td>失敗しきい値に達したときに実行するアクションを入力します。 修正アクションは、他のワーカーが修復されておらず、しかもこのワーカー・ノードが以前のアクション実行時のクールオフ期間にないときにのみ実行されます。 <ul><li><code>REBOOT</code>: ワーカー・ノードをリブートします。</li><li><code>RELOAD</code>: ワーカー・ノードに必要な構成をすべて、クリーンな OS から再ロードします。</li></ul></td>
   </tr>
   <tr>
   <td><code>CooloffSeconds</code></td>
   <td>既に修正アクションが実行されたノードに対して別の修正アクションを実行するために Autorecovery が待機する必要がある秒数を入力します。 クールオフ期間は、修正アクションが実行された時点から始まります。</td>
   </tr>
   <tr>
   <td><code>IntervalSeconds</code></td>
   <td>連続する検査の間隔の秒数を入力します。 例えば、値が 180 である場合、Autorecovery は 3 分ごとに各ノードで検査を実行します。</td>
   </tr>
   <tr>
   <td><code>TimeoutSeconds</code></td>
   <td>データベースに対する検査の呼び出しの最大秒数を入力します。この秒数が経過すると、Autorecovery は呼び出し動作を終了します。 <code>TimeoutSeconds</code> の値は、<code>IntervalSeconds</code> の値より小さくする必要があります。</td>
   </tr>
   <tr>
   <td><code>ポート</code></td>
   <td>検査タイプが <code>HTTP</code> である場合は、HTTP サーバーがワーカー・ノードにバインドする必要があるポートを入力します。 このポートは、クラスター内のすべてのワーカー・ノードの IP で公開されている必要があります。 Autorecovery がサーバーを検査するためには、すべてのノードで一定のポート番号を使用する必要があります。 カスタム・サーバーをクラスターにデプロイする場合は、[DaemonSets ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) を使用します。</td>
   </tr>
   <tr>
   <td><code>ExpectedStatus</code></td>
   <td>検査タイプが <code>HTTP</code> である場合は、検査で返されることが予期される HTTP サーバー状況を入力します。 例えば、値 200 は、サーバーからの応答として <code>OK</code> を予期していることを示します。</td>
   </tr>
   <tr>
   <td><code>Route</code></td>
   <td>検査タイプが <code>HTTP</code> である場合は、HTTP サーバーから要求されたパスを入力します。 この値は通常、すべてのワーカー・ノードで実行されているサーバーのメトリック・パスです。</td>
   </tr>
   <tr>
   <td><code>Enabled</code></td>
   <td>検査を有効にするには <code>true</code>、検査を無効にするには <code>false</code> を入力します。</td>
   </tr>
   <tr>
   <td><code>Namespace</code></td>
   <td> オプション: ある名前空間内のポッドのみを検査するように <code>checkpod.json</code> を制限するには、<code>Namespace</code> フィールドを追加してその名前空間を入力します。</td>
   </tr>
   </tbody>
   </table>

3. クラスター内に構成マップを作成します。

    ```
    kubectl apply -f ibm-worker-recovery-checks.yaml
    ```
    {: pre}

3. 適切な検査を実行して、`kube-system` 名前空間内に `ibm-worker-recovery-checks` という名前の構成マップが作成されていることを確認します。

    ```
    kubectl -n kube-system get cm ibm-worker-recovery-checks -o yaml
    ```
    {: pre}

4. `ibm-worker-recovery` Helm チャートをインストールして、クラスターに Autorecovery をデプロイします。

    ```
    helm install --name ibm-worker-recovery ibm/ibm-worker-recovery  --namespace kube-system
    ```
    {: pre}

5. 数分後に、次のコマンドの出力にある `Events` セクションを確認して、Autorecovery のデプロイメントのアクティビティーを確認できます。

    ```
    kubectl -n kube-system describe deployment ibm-worker-recovery
    ```
    {: pre}

6. Autorecovery デプロイメントでアクティビティーが表示されない場合は、Autorecovery チャート定義に含まれているテストを実行して、Helm デプロイメントを確認することができます。

    ```
    helm test ibm-worker-recovery
    ```
    {: pre}
