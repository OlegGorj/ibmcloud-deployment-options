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



# クラスター・アクセス権限の割り当て
{: #users}

クラスター管理者は、Kubernetes クラスターのアクセス・ポリシーを定義して、さまざまなユーザーに対してさまざまなレベルのアクセス権限を作成することができます。 例えば、特定のユーザーにはクラスター・リソースを使用する権限を与える一方で、他のユーザーにはコンテナーをデプロイする権限だけを与えることができます。
{: shortdesc}


## アクセス要求の計画
{: #planning_access}

クラスター管理者にとって、アクセス要求を追跡することが難しいことがあります。クラスターのセキュリティーを維持するためには、アクセス要求を伝えるためのコミュニケーション・パターンを確立することが不可欠です。
{: shortdesc}

適切なユーザーが適切なアクセス権限を持つように、アクセス権限の要求や、一般的な作業の支援の依頼に関するポリシーを明確に定義してください。

チームで使用している方式が既にある場合、それは素晴らしいことです。 どこから始めるべきか検討している場合は、以下のいずれかの方式を試してみてください。

*  チケット・システムを作成する
*  フォーム・テンプレートを作成する
*  Wiki ページを作成する
*  E メールによる要求を義務付ける
*  チームの日常業務を追跡するために既に使用している問題追跡システムを使用する

難しく感じますか? [ユーザー、チーム、アプリケーションを編成するためのベスト・プラクティス](/docs/tutorials/users-teams-applications.html)に関するこのチュートリアルを試してみてください。
{: tip}

## アクセス・ポリシーと許可
{: #access_policies}

アクセス・ポリシーの適用範囲は、実行を許可する操作を定義するためのユーザー定義役割に基づきます。 クラスター、インフラストラクチャー、サービスのインスタンス、または Cloud Foundry の役割に固有のポリシーを設定できます。
{: shortdesc}

{{site.data.keyword.containershort_notm}} を使用するすべてのユーザー用にアクセス・ポリシーを定義する必要があります。 事前定義されているポリシーもあれば、カスタマイズ可能なポリシーもあります。 以下の画像と定義を参照して、一般的なユーザー・タスクに合う役割を見つけ、ポリシーをカスタマイズする必要がある箇所を特定してください。

![{{site.data.keyword.containershort_notm}} アクセス役割](/images/user-policies.png)

図. {{site.data.keyword.containershort_notm}} アクセス役割

<dl>
  <dt>IAM (ID およびアクセス管理) のポリシー</dt>
    <dd><p><strong>プラットフォーム</strong>: 個々のユーザーが {{site.data.keyword.containershort_notm}} クラスターに対して実行できる操作を定義できます。 これらのポリシーは地域ごとに設定できます。 操作の例として、クラスターの作成や削除、ワーカー・ノードの追加などがあります。 これらのポリシーは、インフラストラクチャー・ポリシーと一緒に設定する必要があります。</p>
    <p><strong>インフラストラクチャー</strong>: クラスター・ノード・マシン、ネットワーキング、ストレージ・リソースなどのインフラストラクチャーに対するアクセス・レベルを定義できます。 ユーザーが要求の実行に {{site.data.keyword.containershort_notm}} GUI を使用しようと CLI を使用しようと、また、IBM Cloud インフラストラクチャー (SoftLayer) で操作を実行しようと、同じポリシーが適用されます。 このタイプのポリシーは、{{site.data.keyword.containershort_notm}} プラットフォーム・アクセス・ポリシーと一緒に設定する必要があります。選択可能な役割について詳しくは、[インフラストラクチャーの許可](/docs/iam/infrastructureaccess.html#infrapermission)を参照してください。</p> </br></br><strong>注:</strong> 権限のあるユーザーが、割り当てられている権限に基づいて IBM Cloud インフラストラクチャー (SoftLayer) アカウントでアクションを実行できるようにするために、ご使用の {{site.data.keyword.Bluemix_notm}} アカウントに [IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオへのアクセス権限がセットアップされている](cs_troubleshoot_clusters.html#cs_credentials)ことを確認してください。</dd>
  <dt>Kubernetes Resource Based Access Control (RBAC) の役割</dt>
    <dd>プラットフォームのアクセス・ポリシーが割り当てられているすべてのユーザーに、Kubernetes 役割が自動的に割り当てられます。 Kubernetes では、[Role Based Access Control (RBAC) ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview) によって、ユーザーがクラスター内部のリソースに対して実行できる操作が決まります。 <strong>注</strong>: <code>default</code> 名前空間では RBAC 役割が自動的に構成されますが、他の名前空間では、クラスター管理者が役割を割り当てることができます。</dd>
  <dt>Cloud Foundry</dt>
    <dd>すべてのサービスを Cloud IAM で管理できるわけではありません。管理できないサービスを使用している場合は、今後も [Cloud Foundry ユーザー役割](/docs/iam/cfaccess.html#cfaccess)を使用してサービスへのアクセス権限を管理できます。</dd>
</dl>


アクセス権をダウングレードしていますか? 処理が完了するまで、数分かかることがあります。
{: tip}

### プラットフォームの役割
{: #platform_roles}

{{site.data.keyword.containershort_notm}} は {{site.data.keyword.Bluemix_notm}} プラットフォームの役割を使用するように構成されています。 役割の権限は、互いの権限を基に構築されています。つまり、`Editor` 役割には、`Viewer` 役割と同じ権限に加えて、エディターに付与される権限があります。 以下の表に、各役割で実行できる操作のタイプを示します。

プラットフォームの役割を割り当てると、対応する RBAC 役割が自動的にデフォルトの名前空間に割り当てられます。ユーザーのプラットフォームの役割を変更すると、RBAC 役割も更新されます。
{: tip}

<table>
<caption>プラットフォームの役割と操作</caption>
  <tr>
    <th>プラットフォームの役割</th>
    <th>操作の例</th>
    <th>対応する RBAC 役割</th>
  </tr>
  <tr>
      <td>Viewer</td>
      <td>クラスターまたは他のサービス・インスタンスの詳細を表示します。</td>
      <td>View</td>
  </tr>
  <tr>
    <td>Editor</td>
    <td>IBM Cloud サービスをクラスターに対してバインドまたはアンバインドしたり、Web フックを作成したりできます。 <strong>注</strong>: サービスをバインドするには、Cloud Foundry の開発者の役割も割り当てられている必要があります。</td>
    <td>Edit</td>
  </tr>
  <tr>
    <td>Operator</td>
    <td>ワーカー・ノードを作成、削除、リブート、または再ロードできます。 サブネットをクラスターに追加できます。</td>
    <td>Admin</td>
  </tr>
  <tr>
    <td>Administrator</td>
    <td>クラスターを作成および削除できます。 サービスおよびインフラストラクチャーに対する他のユーザーのアクセス・ポリシーをアカウント・レベルで編集できます。 <strong>注</strong>: 管理者アクセス権限は、特定のクラスターまたはご使用のアカウントのすべてのサービス・インスタンスに割り当てることができます。クラスターを削除するには、削除するクラスターに対する管理者アクセス権限が必要です。クラスターを作成するには、すべてのサービス・インスタンスに対する管理者役割が必要です。</td>
    <td>Cluster-admin</td>
  </tr>
</table>

UI でユーザー役割を割り当てる方法について詳しくは、[IAM アクセスの管理](/docs/iam/mngiam.html#iammanidaccser)を参照してください。


### インフラストラクチャーの役割
{: #infrastructure_roles}

インフラストラクチャーの役割により、ユーザーはインフラストラクチャー・レベルでリソースに対してタスクを実行できます。 以下の表に、各役割で実行できる操作のタイプを示します。 インフラストラクチャーの役割はカスタマイズ可能です。ユーザーには各ユーザーのジョブの実行に必要なアクセス権限だけを付与してください。

特定のインフラストラクチャーの役割を付与することに加え、インフラストラクチャーを操作するユーザーにデバイス・アクセス権限を付与する必要もあります。
{: tip}

<table>
<caption>インフラストラクチャーの役割と操作</caption>
  <tr>
    <th>インフラストラクチャー役割</th>
    <th>操作の例</th>
  </tr>
  <tr>
    <td><i>表示のみ</i></td>
    <td>インフラストラクチャーの詳細を表示して、請求や支払いなどのアカウント・サマリーを参照できます。</td>
  </tr>
  <tr>
    <td><i>基本ユーザー</i></td>
    <td>IP アドレスなどのサービス構成の編集、DNS レコードの追加または編集、インフラストラクチャーに対するアクセス権限を持つ新しいユーザーの追加を行えます。</td>
  </tr>
  <tr>
    <td><i>スーパーユーザー</i></td>
    <td>インフラストラクチャーに関するすべての操作を実行できます。</td>
  </tr>
</table>

役割の割り当てを開始するには、[ユーザーのインフラストラクチャー許可のカスタマイズ](#infra_access)の手順に従ってください。

### RBAC 役割
{: #rbac_roles}

リソース・ベース・アクセス制御 (RBAC) は、クラスター内部のリソースを保護し、どのユーザーがどの Kubernetes 操作を実行できるかを決めるための方法です。 以下の表に、RBAC 役割のタイプと、その役割でユーザーが実行できる操作のタイプを示しています。 権限は互いの権限を基に構築されます。つまり、`Admin` には、`View` 役割と `Edit` 役割に属するすべてのポリシーもあります。 必ず、必要なアクセス権限だけをユーザーに付与するようにしてください。

RBAC 役割は、デフォルト名前空間のプラットフォーム役割とともに自動的に設定されます。[役割は更新できます。また、他の名前空間に役割を割り当てることもできます](#rbac)。
{: tip}

<table>
<caption>RBAC の役割と操作</caption>
  <tr>
    <th>RBAC 役割</th>
    <th>操作の例</th>
  </tr>
  <tr>
    <td>View</td>
    <td>デフォルトの名前空間内のリソースを参照できます。 Viewer が Kubernetes シークレットを表示することはできません。</td>
  </tr>
  <tr>
    <td>Edit</td>
    <td>デフォルトの名前空間内のリソースの読み取りと書き込みを行えます。</td>
  </tr>
  <tr>
    <td>Admin</td>
    <td>デフォルトの名前空間自体ではなく、その名前空間内のリソースの読み取りと書き込みを行えます。 名前空間内に役割を作成できます。</td>
  </tr>
  <tr>
    <td>Cluster admin</td>
    <td>すべての名前空間内のリソースの読み取りと書き込みを行えます。 名前空間内に役割を作成できます。 Kubernetes ダッシュボードにアクセスできます。 アプリを公開する Ingress リソースを作成できます。</td>
  </tr>
</table>

<br />


## {{site.data.keyword.Bluemix_notm}} アカウントへのユーザーの追加
{: #add_users}

{{site.data.keyword.Bluemix_notm}} アカウントにユーザーを追加して、クラスターへのアクセス権限を付与できます。
{:shortdesc}

始める前に、{{site.data.keyword.Bluemix_notm}} アカウントに対する Cloud Foundry の`管理者`役割が自分に割り当てられていることを確認してください。

1.  [ユーザーをアカウントに追加します](../iam/iamuserinv.html#iamuserinv)。
2.  **「アクセス」**セクションで**「サービス」**を展開します。
3.  {{site.data.keyword.containershort_notm}} のアクセス権限を設定するために、ユーザーにプラットフォーム役割を割り当てます。
      1. **「サービス」**ドロップダウン・リストで、**「{{site.data.keyword.containershort_notm}}」**を選択します。
      2. **「地域」**ドロップダウン・リストから、ユーザーを招待する地域を選択します。
      3. **「サービス・インスタンス」**ドロップダウン・リストから、ユーザーを招待するクラスターを選択します。 特定のクラスターの ID を確認するには、`bx cs clusters` を実行します。
      4. **「役割の選択」**セクションで、役割を選択します。 役割別のサポート対象アクションの一覧については、[アクセス・ポリシーと許可](#access_policies)を参照してください。
4. [オプション: Cloud Foundry の役割を割り当てます](/docs/iam/mngcf.html#mngcf)。
5. [オプション: インフラストラクチャーの役割を割り当てます](/docs/iam/infrastructureaccess.html#infrapermission)。
6. **「ユーザーの招待」**をクリックします。

<br />


## IAM API キーと `bx cs credentials-set` コマンドについて
{: #api_key}

アカウントでクラスターを正常にプロビジョンして使用するには、IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオにアクセスできるようにアカウントを正しくセットアップしておく必要があります。 アカウントのセットアップに応じて、IAM API キーまたはインフラストラクチャー資格情報 (`bx cs credentials-set` コマンドを使用して手動で設定) のいずれかを使用します。

<dl>
  <dt>IAM API キー</dt>
    <dd><p>IAM (ID およびアクセス管理) の API キーは、{{site.data.keyword.containershort_notm}} 管理アクセス・ポリシーを必要とする最初のアクションを実行したときに、地域に対して自動的に設定されます。 例えば、管理ユーザーの 1 人が <code>us-south</code> 地域に最初のクラスターを作成したとします。 これにより、その地域に対してこのユーザーの IAM API キーがアカウントに保管されます。 新しいワーカー・ノードや VLAN などの IBM Cloud インフラストラクチャー (SoftLayer) を注文する際には、この API キーが使用されます。</p> <p>IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオとのやりとりが必要なアクション (例えば、新規クラスターの作成やワーカー・ノードの再ロードなど) を別のユーザーがこの地域で実行すると、保管されている API キーを基に、そのアクションを実行できるだけの権限があるかどうかが判断されます。 インフラストラクチャー関連のアクションをクラスター内で正常に実行するためには、{{site.data.keyword.containershort_notm}} 管理ユーザーにインフラストラクチャー・アクセス・ポリシーの<strong>スーパーユーザー</strong>を割り当ててください。</p>
    <p>現在の API キー所有者を調べるには、[<code>bx cs api-key-info</code>](cs_cli_reference.html#cs_api_key_info) を実行します。地域に対して保管されている API キーを更新する必要があることがわかった場合は、[<code>bx cs api-key-reset</code>](cs_cli_reference.html#cs_api_key_reset) コマンドを実行して更新できます。 このコマンドには {{site.data.keyword.containershort_notm}} 管理アクセス・ポリシーが必要です。このコマンドを実行すると、実行したユーザーの API キーがアカウントに保管されます。</p>
    <p><strong>注:</strong> <code>bx cs credentials-set</code> コマンドを使用して手動で IBM Cloud インフラストラクチャー (SoftLayer) 資格情報を設定した場合、地域に対して保管されている API キーは使用されない可能性があります。</p></dd>
  <dt><code>bx cs credentials-set</code> による IBM Cloud インフラストラクチャー (SoftLayer) の資格情報</dt>
    <dd><p>{{site.data.keyword.Bluemix_notm}} 従量制課金アカウントがあれば、IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオにデフォルトでアクセスできます。 しかし、既に所有している別の IBM Cloud インフラストラクチャー (SoftLayer) アカウントを使用して、インフラストラクチャーを注文したい場合もあるでしょう。 [<code>bx cs credentials-set</code>](cs_cli_reference.html#cs_credentials_set) コマンドを使用すると、そのようなインフラストラクチャー・アカウントを {{site.data.keyword.Bluemix_notm}} アカウントにリンクできます。</p>
    <p>IBM Cloud インフラストラクチャー (SoftLayer) の資格情報を手動で設定した場合は、アカウントに IAM API キーが存在していても、それらの資格情報がインフラストラクチャーの注文に使用されます。 資格情報が保管されているユーザーにインフラストラクチャーを注文するために必要な権限がない場合、クラスターを作成したりワーカー・ノードを再ロードしたりするインフラストラクチャー関連のアクションは失敗する可能性があります。</p>
    <p>手動で設定した IBM Cloud インフラストラクチャー (SoftLayer) の資格情報を削除するには、[<code>bx cs credentials-unset</code>](cs_cli_reference.html#cs_credentials_unset) コマンドを使用します。 資格情報が削除されると、IAM API キーがインフラストラクチャーの注文に使用されます。</p></dd>
</dl>

<br />


## ユーザーのインフラストラクチャー許可のカスタマイズ
{: #infra_access}

ID およびアクセス管理でインフラストラクチャー・ポリシーを設定すると、ユーザーには役割に関連付けられた許可が付与されます。 それらの許可をカスタマイズするには、IBM Cloud インフラストラクチャー (SoftLayer) にログインし、そこで許可を調整する必要があります。
{: #view_access}

例えば、**基本ユーザー**はワーカー・ノードをリブートできますが、ワーカー・ノードを再ロードすることはできません。 そのユーザーに**スーパーユーザー**権限を付与しなくても、IBM Cloud インフラストラクチャー (SoftLayer) の許可を調整して、再ロード・コマンドを実行する許可を追加できます。

1.  [{{site.data.keyword.Bluemix_notm}} アカウント](https://console.bluemix.net/)にログインし、メニューから**「インフラストラクチャー」**を選択します。

2.  **「アカウント」** > **「ユーザー」** > **「ユーザー・リスト」**と進みます。

3.  許可を変更するには、ユーザー・プロファイルの名前または**「デバイス・アクセス」**列を選択します。

4.  **「ポータルの許可」**タブで、ユーザーのアクセス権限をカスタマイズします。 ユーザーが必要とする許可は、ユーザーが使用する必要があるインフラストラクチャー・リソースによって異なります。

    * **「クイック許可」**ドロップダウン・リストを使用して、**「スーパーユーザー」**役割を割り当てます。これにより、すべての許可がユーザーに付与されます。
    * **「クイック許可」**ドロップダウン・リストを使用して、**「基本ユーザー」**役割を割り当てます。これにより、すべての許可ではなくいくつかの必要な許可がユーザーに付与されます。
    * **「スーパーユーザー」**役割を指定してすべての許可を付与したくない場合や、**「基本ユーザー」**役割を超える許可を追加する必要がある場合は、次の表を参照してください。この表は、{{site.data.keyword.containershort_notm}} で一般的な作業を行うために必要な許可を示しています。

    <table summary="一般的な {{site.data.keyword.containershort_notm}} シナリオに必要なインフラストラクチャー許可。">
     <caption>一般的に必要な {{site.data.keyword.containershort_notm}} のインフラストラクチャー許可</caption>
     <thead>
      <th>{{site.data.keyword.containershort_notm}} の一般的な作業</th>
      <th>必要なインフラストラクチャー許可 (タブ別)</th>
     </thead>
     <tbody>
       <tr>
         <td><strong>最小許可</strong>: <ul><li>クラスターを作成します。</li></ul></td>
         <td><strong>デバイス</strong>:<ul><li>Virtual Server の詳細の表示</li><li>サーバーのリブートと IPMI システム情報の表示</li><li>OS 再ロードの発行とレスキュー・カーネルの開始</li></ul><strong>アカウント</strong>: <ul><li>クラウド・インスタンスの追加/アップグレード</li><li>サーバーの追加</li></ul></td>
       </tr>
       <tr>
         <td><strong>クラスター管理</strong>: <ul><li>クラスターを作成、更新、削除する。</li><li>ワーカー・ノードを追加、再ロード、リブートする。</li><li>VLAN を表示する。</li><li>サブネットを作成する。</li><li>ポッドとロード・バランサー・サービスをデプロイする。</li></ul></td>
         <td><strong>サポート</strong>:<ul><li>チケットの表示</li><li>チケットの追加</li><li>チケットの編集</li></ul>
         <strong>デバイス</strong>:<ul><li>Virtual Server の詳細の表示</li><li>サーバーのリブートと IPMI システム情報の表示</li><li>サーバーのアップグレード</li><li>OS 再ロードの発行とレスキュー・カーネルの開始</li></ul>
         <strong>サービス</strong>:<ul><li>SSH 鍵の管理</li></ul>
         <strong>アカウント</strong>:<ul><li>アカウント要約の表示</li><li>クラウド・インスタンスの追加/アップグレード</li><li>サーバーのキャンセル</li><li>サーバーの追加</li></ul></td>
       </tr>
       <tr>
         <td><strong>ストレージ</strong>: <ul><li>永続ボリューム請求を作成して永続ボリュームをプロビジョンする。</li><li>ストレージ・インフラストラクチャー・リソースを作成および管理する。</li></ul></td>
         <td><strong>サービス</strong>:<ul><li>ストレージの管理</li></ul><strong>アカウント</strong>:<ul><li>ストレージの追加</li></ul></td>
       </tr>
       <tr>
         <td><strong>プライベート・ネットワーキング</strong>: <ul><li>クラスター内ネットワーキング用プライベート VLAN を管理する。</li><li>プライベート・ネットワークへの VPN 接続をセットアップする。</li></ul></td>
         <td><strong>ネットワーク</strong>:<ul><li>ネットワーク・サブネット経路の管理</li><li>IPSEC ネットワーク・トンネルの管理</li><li>ネットワーク・ゲートウェイの管理</li><li>VPN 管理</li></ul></td>
       </tr>
       <tr>
         <td><strong>パブリック・ネットワーキング</strong>:<ul><li>アプリを公開するためにパブリック・ロード・バランサーまたは Ingress ネットワーキングをセットアップする。</li></ul></td>
         <td><strong>デバイス</strong>:<ul><li>Load Balancers の管理</li><li>ホスト名/ドメインの編集</li><li>ポート・コントロールの管理</li></ul>
         <strong>ネットワーク</strong>:<ul><li>パブリック・ネットワーク・ポートのコンピュートを追加</li><li>ネットワーク・サブネット経路の管理</li><li>IP アドレスの追加</li></ul>
         <strong>サービス</strong>:<ul><li>DNS、リバース DNS、WHOIS の管理</li><li>証明書 (SSL) の表示</li><li>証明書 (SSL) の管理</li></ul></td>
       </tr>
     </tbody>
    </table>

5.  変更を保存するには、**「ポータル許可の編集」**をクリックします。

6.  **「デバイス・アクセス」**タブで、アクセス権限を付与するデバイスを選択します。

    * **「デバイス・タイプ」**ドロップダウン・リストで、**「すべての仮想サーバー」**に対するアクセス権限を付与できます。
    * 作成される新しいデバイスへのアクセスをユーザーに許可するには、**「新規デバイスが追加されたときに自動的にアクセス権限を付与します」**を選択します。
    * 変更を保存するには、**「デバイスへのアクセス権限の更新」**をクリックします。

<br />


## カスタム Kubernetes RBAC 役割によるユーザーの許可
{: #rbac}

[アクセス・ポリシーと許可](#access_policies)で説明したように、{{site.data.keyword.containershort_notm}} アクセス・ポリシーは、Kubernetes の役割ベース・アクセス制御 (RBAC) の特定の役割に対応しています。 対応するアクセス・ポリシーとは異なる他の Kubernetes 役割を許可するには、RBAC 役割をカスタマイズしたものを個人またはユーザー・グループに割り当てます。
{: shortdesc}

例えば、開発者チームに、特定の API グループを使用する許可や、クラスター全体ではなくクラスター内の特定の Kubernetes 名前空間内のリソースを使用する許可を付与したい場合があります。 その場合は、役割を作成した後、{{site.data.keyword.containershort_notm}} に固有のユーザー名を使用して、ユーザーに役割をバインドします。 詳しくは、Kubernetes 資料の [Using RBAC Authorization ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview) を参照してください。

始めに、[Kubernetes CLI のターゲットをクラスターにします](cs_cli_install.html#cs_cli_configure)。

1.  割り当てるアクセス権限を含む役割を作成します。

    1. 割り当てるアクセス権限を含む役割を定義するための `.yaml` ファイルを作成します。

        ```
        kind: Role
        apiVersion: rbac.authorization.k8s.io/v1
        metadata:
          namespace: default
          name: my_role
        rules:
        - apiGroups: [""]
          resources: ["pods"]
          verbs: ["get", "watch", "list"]
        - apiGroups: ["apps", "extensions"]
          resources: ["daemonsets", "deployments"]
          verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
        ```
        {: codeblock}

        <table>
        <caption>この YAML の構成要素について</caption>
          <thead>
            <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> この YAML の構成要素について</th>
          </thead>
          <tbody>
            <tr>
              <td><code>kind</code></td>
              <td>単一の名前空間に含まれているリソースへのアクセス権限を付与する場合は `Role` を使用し、クラスター全体のリソースの場合は `ClusterRole` を使用します。</td>
            </tr>
            <tr>
              <td><code>apiVersion</code></td>
              <td><ul><li>Kubernetes 1.8 以降を実行するクラスターの場合は、`rbac.authorization.k8s.io/v1` を使用します。 </li><li>それより前のバージョンの場合は、`apiVersion: rbac.authorization.k8s.io/v1beta1` を使用します。</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/namespace</code></td>
              <td><ul><li>kind が `Role` の場合: アクセス権限を付与する Kubernetes 名前空間を指定します。</li><li>クラスター・レベルで適用される `ClusterRole` を作成する場合、`namespace` フィールドは使用しないでください。</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/name</code></td>
              <td>役割に名前を付けます。この名前を、後で役割をバインドするときに使用します。</td>
            </tr>
            <tr>
              <td><code>rules/apiGroups</code></td>
              <td><ul><li>ユーザーに対話を許可する Kubernetes API グループ (`"apps"`、`"batch"`、`"extensions"` など) を指定します。 </li><li>REST パス `api/v1` にあるコア API グループに対するアクセス権限については、`[""]` のようにグループをブランクにします。</li><li>詳しくは、Kubernetes 資料の [API groups ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://v1-9.docs.kubernetes.io/docs/reference/api-overview/#api-groups) を参照してください。</li></ul></td>
            </tr>
            <tr>
              <td><code>rules/resources</code></td>
              <td><ul><li>アクセス権限を付与する Kubernetes リソース (`"daemonsets"`、`"deployments"`、`"events"`、`"ingresses"` など) を指定します。</li><li>`"nodes"` を指定する場合、役割の kind は `ClusterRole` でなければなりません。</li><li>リソースのリストについては、Kubernetes チートシート [Resource types ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/reference/kubectl/cheatsheet/) にある表を参照してください。</li></ul></td>
            </tr>
            <tr>
              <td><code>rules/verbs</code></td>
              <td><ul><li>ユーザーに実行を許可するアクションのタイプ (`"get"`、`"list"`、`"describe"`、`"create"`、`"delete"` など) を指定します。 </li><li>verb の完全なリストについては、[`kubectl` 資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/reference/kubectl/overview/) を参照してください。</li></ul></td>
            </tr>
          </tbody>
        </table>

    2.  クラスター内に役割を作成します。

        ```
        kubectl apply -f <path_to_yaml_file>
        ```
        {: pre}

    3.  役割が作成されたことを確認します。

        ```
        kubectl get roles
        ```
        {: pre}

2.  ユーザーを役割にバインドします。

    1. ユーザーを役割にバインドするための `.yaml` ファイルを作成します。 各件名 (subject) の name に使用している固有の URL に注目してください。

        ```
        kind: RoleBinding
        apiVersion: rbac.authorization.k8s.io/v1
        metadata:
          name: my_role_team1
          namespace: default
        subjects:
        - kind: User
          name: https://iam.ng.bluemix.net/kubernetes#user1@example.com
          apiGroup: rbac.authorization.k8s.io
        - kind: User
          name: https://iam.ng.bluemix.net/kubernetes#user2@example.com
          apiGroup: rbac.authorization.k8s.io
        roleRef:
          kind: Role
          name: custom-rbac-test
          apiGroup: rbac.authorization.k8s.io
        ```
        {: codeblock}

        <table>
        <caption>この YAML の構成要素について</caption>
          <thead>
            <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> この YAML の構成要素について</th>
          </thead>
          <tbody>
            <tr>
              <td><code>kind</code></td>
              <td>役割用の `.yaml` ファイルのタイプが `Role` (名前空間) でも `ClusterRole` (クラスター全体) でも、`kind` には `RoleBinding` を指定します。</td>
            </tr>
            <tr>
              <td><code>apiVersion</code></td>
              <td><ul><li>Kubernetes 1.8 以降を実行するクラスターの場合は、`rbac.authorization.k8s.io/v1` を使用します。 </li><li>それより前のバージョンの場合は、`apiVersion: rbac.authorization.k8s.io/v1beta1` を使用します。</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/namespace</code></td>
              <td><ul><li>kind が `Role` の場合: アクセス権限を付与する Kubernetes 名前空間を指定します。</li><li>クラスター・レベルで適用される `ClusterRole` を作成する場合、`namespace` フィールドは使用しないでください。</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/name</code></td>
              <td>この役割バインディングの名前を指定します。</td>
            </tr>
            <tr>
              <td><code>subjects/kind</code></td>
              <td>kind には `User` を指定します。</td>
            </tr>
            <tr>
              <td><code>subjects/name</code></td>
              <td><ul><li>ユーザーの E メール・アドレスを URL `https://iam.ng.bluemix.net/kubernetes#` に付加します。</li><li>例えば、`https://iam.ng.bluemix.net/kubernetes#user1@example.com` のようにします。</li></ul></td>
            </tr>
            <tr>
              <td><code>subjects/apiGroup</code></td>
              <td>`rbac.authorization.k8s.io` を使用します。</td>
            </tr>
            <tr>
              <td><code>roleRef/kind</code></td>
              <td>役割用の `.yaml` ファイルの `kind` と同じ値 (`Role` または `ClusterRole`) を入力します。</td>
            </tr>
            <tr>
              <td><code>roleRef/name</code></td>
              <td>役割用の `.yaml` ファイルの名前を入力します。</td>
            </tr>
            <tr>
              <td><code>roleRef/apiGroup</code></td>
              <td>`rbac.authorization.k8s.io` を使用します。</td>
            </tr>
          </tbody>
        </table>

    2. クラスター内に役割バインディング・リソースを作成します。

        ```
        kubectl apply -f filepath/my_role_team1.yaml
        ```
        {: pre}

    3.  バインディングが作成されたことを確認します。

        ```
        kubectl get rolebinding
        ```
        {: pre}

カスタム Kubernetes RBAC 役割を作成してバインドしたので、ユーザーをフォローアップします。 役割を介して実行許可を持っているアクション (ポッドを削除するなど) をテストするようにユーザーに依頼してください。
