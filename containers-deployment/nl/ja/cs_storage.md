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




# クラスター内でのデータの保存
{: #storage}
{{site.data.keyword.containerlong}} 内のデータを永続化して、アプリ・インスタンス間でデータを共有し、Kubernetes クラスター内のコンポーネントに障害が発生した場合にデータが失われないようにすることができます。


## 可用性の高いストレージの計画
{: #planning}

{{site.data.keyword.containerlong_notm}} では、アプリ・データを保管してクラスター内のポッド間でデータを共有する方法として、複数の選択肢 (オプション) の中から選択することができます。 ただし、どのストレージ・オプションでも、クラスター内のコンポーネントの障害またはサイト全体の障害が起きた場合に同じレベルの永続性と可用性が得られるというわけではありません。
{: shortdesc}


### 非永続のデータ・ストレージ・オプション
{: #non_persistent}

データを永続的に保管する必要がない場合や、データをアプリ・インスタンス間で共有する必要がない場合は、非永続ストレージ・オプションを使用できます。アプリ・コンポーネントの単体テストを行う場合や新機能を試す場合にも非永続ストレージ・オプションを使用できます。
{: shortdesc}

以下のイメージは、{{site.data.keyword.containerlong_notm}} で使用可能な非永続データ・ストレージ・オプションを示しています。 これらの方法は、フリー・クラスターと標準クラスターで使用可能です。
<p>
<img src="images/cs_storage_nonpersistent.png" alt="非永続データ・ストレージ・オプション" width="500" style="width: 500px; border-style: none"/></p>

<table summary="この表は非永続ストレージ・オプションを示しています。行は左から右に読みます。1 列目はオプションの番号、2 列目はオプションの名称、3 列目は説明です。" style="width: 100%">
<caption>非永続のストレージ・オプション</caption>
  <thead>
  <th>オプション</th>
  <th>説明</th>
  </thead>
  <tbody>
    <tr>
      <td>1. コンテナーまたはポッドの内部</td>
      <td>設計上、コンテナーやポッドの存続期間は短く、予期せぬ障害が起こることがあります。 ただし、コンテナーのローカル・ファイル・システムにデータを書き込み、コンテナーのライフサイクルにわたってデータを保管することはできます。 コンテナー内部のデータは、他のコンテナーやポッドと共有できず、コンテナーがクラッシュしたり削除されたりすると失われます。 詳しくは、[Storing data in a container](https://docs.docker.com/storage/) を参照してください。</td>
    </tr>
  <tr>
    <td>2. ワーカー・ノード上</td>
    <td>すべてのワーカー・ノードには 1 次ストレージと 2 次ストレージがセットアップされています。これはワーカー・ノードとして選択したマシン・タイプによって決まります。 1 次ストレージは、オペレーティング・システムのデータの保管に使用され、[Kubernetes <code>hostPath</code> ボリューム ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/storage/volumes/#hostpath) を使用してアクセスできます。 2 次ストレージは、`kubelet` およびコンテナー・ランタイム・エンジンからのデータを保管するために使用されます。2 次ストレージには、[Kubernetes <code>emptyDir</code> ボリューム ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/storage/volumes/#emptydir) を使用してアクセスできます。<br/><br/><code>hostPath</code> ボリュームは、ワーカー・ノード・ファイル・システムからポッドにファイルをマウントするために使用されますが、<code>emptyDir</code> は、クラスター内のポッドに割り当てられる空のディレクトリーを作成します。 そのポッド内のすべてのコンテナーは、そのボリュームに対する読み取りと書き込みを行うことができます。 ボリュームは特定の 1 つのポッドに割り当てられるので、レプリカ・セット内の他のポッドとデータを共有することはできません。<br/><br/><p><code>hostPath</code> または <code>emptyDir</code> ボリュームとそのデータは、以下の場合に削除されます。 <ul><li>ワーカー・ノードが削除された場合。</li><li>ワーカー・ノードが再ロードされた、または更新された場合。</li><li>クラスターが削除された場合。</li><li>{{site.data.keyword.Bluemix_notm}} アカウントが一時停止状態になった場合。 </li></ul></p><p>さらに、<code>emptyDir</code> ボリューム内のデータは以下の場合に削除されます。 <ul><li>割り当てられたポッドはワーカー・ノードから永久に削除されます。</li><li>割り当てられたポッドが別のワーカー・ノード上にスケジュールされた場合。</li></ul></p><p><strong>注:</strong> ポッド内のコンテナーがクラッシュした場合でも、ボリューム内のデータはワーカー・ノードで引き続き使用できます。</p></td>
    </tr>
    </tbody>
    </table>


### 高可用性のための永続データ・ストレージ・オプション
{: #persistent}

可用性の高いステートフル・アプリを作成するうえでの主要な課題は、複数の場所にある複数のアプリ・インスタンスの間でデータを保持し、常にデータの同期を保つことです。データの高可用性を実現するためには、マスター・データベースに複数のインスタンスがあり、それらが複数のデータ・センターや複数の地域にも分散している必要があります。このマスター・データベースは、正しいソースを 1 つのみにするために、継続的に複製される必要があります。クラスター内のすべてのインスタンスが、このマスター・データベースに対して読み取りと書き込みを行う必要があります。 マスターのいずれかのインスタンスがダウンした場合は、その他のインスタンスがワークロードを引き継ぐため、アプリのダウン時間は発生しません。
{: shortdesc}

以下のイメージは、標準クラスター内のデータの可用性を高めるために {{site.data.keyword.containerlong_notm}} で選択できるオプションを示しています。 どのオプションが適切かは、以下の要因に応じて決まります。
  * **所有しているアプリのタイプ:** 例えば、データをデータベース内ではなくファイル・ベースで保管しなければならないアプリがある場合があります。
  * **データの保管と転送の場所に関する法的要件:** 例えば、法律によってデータの保管と転送が米国内だけに制限されているために、欧州にあるサービスを使用できない場合があります。
  * **バックアップとリストアのオプション:** すべてのストレージ・オプションに、データをバックアップ/リストアするための機能が用意されています。 利用可能なバックアップとリストアのオプションが、バックアップの頻度や、1 次データ・センター外にデータを保管できることなどの、お客様の災害復旧計画の要件を満たしていることを確認してください。
  * **グローバルな複製:** 高可用性を実現するためには、世界中のデータ・センター間に分散され、複製されている複数のストレージ・インスタンスをセットアップすることが推奨されます。

<br/>
<img src="images/cs_storage_ha.png" alt="永続ストレージに関する高可用性オプション"/>

<table summary="この表は永続ストレージ・オプションを示しています。行は左から右に読みます。1 列目はオプションの番号、2 列目はオプションの名称、3 列目は説明です。">
<caption>永続ストレージ・オプション</caption>
  <thead>
  <th>オプション</th>
  <th>説明</th>
  </thead>
  <tbody>
  <tr>
  <td>1. NFS ファイル・ストレージまたはブロック・ストレージ</td>
  <td>このオプションでは、Kubernetes 永続ボリュームを使用して、アプリやコンテナーのデータを保持できます。 ボリュームは、エンデュランスおよびパフォーマンスの [NFS ベースのファイル・ストレージ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud/file-storage/details) または[ブロック・ストレージ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud/block-storage) でホストされます。これらは、データをデータベース内ではなくファイル・ベースまたはブロックで保管するアプリに使用できます。 ファイルおよびブロック・ストレージに保存されるデータは、保存時に暗号化されます。<p>{{site.data.keyword.containershort_notm}} には、ストレージのサイズ範囲、IOPS、削除ポリシー、ボリュームに対する読み取りと書き込みの許可を定義する、事前定義されたストレージ・クラスが用意されています。 ファイル・ストレージまたはブロック・ストレージに関する要求を開始するには、[永続ボリューム請求 (PVC)](cs_storage.html#create) を作成する必要があります。 PVC をサブミットすると、NFS ベースのファイル・ストレージまたはブロック・ストレージでホストされる永続ボリュームが、{{site.data.keyword.containershort_notm}} によって動的にプロビジョンされます。 [PVC をデプロイメントのボリュームとしてマウント](cs_storage.html#app_volume_mount)すると、コンテナーがそのボリュームに対して読み取りと書き込みを行えるようになります。 </p><p>永続ボリュームは、ワーカー・ノードがあるデータ・センター内でプロビジョンされます。 同じレプリカ・セットと、または同じクラスター内の他のデプロイメントとデータを共有できます。 別のデータ・センターまたは地域にあるクラスターとデータを共有することはできません。 </p><p>デフォルトでは、NFS ストレージおよびブロック・ストレージは自動的にバックアップされません。 用意されている[バックアップとリストアのメカニズム](cs_storage.html#backup_restore)を使用して、クラスターの定期的なバックアップをセットアップできます。 コンテナーがクラッシュしたとき、またはポッドがワーカー・ノードから削除されたときでも、データは削除されないので、ボリュームをマウントした他のデプロイメントから引き続きアクセスできます。 </p><p><strong>注:</strong> 永続 NFS ファイル共有ストレージおよびブロック・ストレージに対する課金は、時間単位または月単位から選択できます。月単位を選択すると、永続ストレージを削除した場合、短期間しか使用していないとしても、月額料金を支払うことになります。</p></td>
  </tr>
  <tr id="cloud-db-service">
    <td>2. Cloud データベース・サービス</td>
    <td>このオプションでは、[IBM Cloudant NoSQL DB](/docs/services/Cloudant/getting-started.html#getting-started-with-cloudant) などの {{site.data.keyword.Bluemix_notm}} データベース・クラウド・サービスを使用して、データを保持できます。 このオプションを使用して保管されたデータには、すべてのクラスター、場所、地域からアクセスできます。 <p> すべてのアプリから単一のデータベース・インスタンスにアクセスするように構成するか、[複数のインスタンスを複数のデータ・センターに分散させ、インスタンス間の複製をセットアップ](/docs/services/Cloudant/guides/active-active.html#configuring-cloudant-nosql-db-for-cross-region-disaster-recovery)して可用性を高めるかを選択できます。 IBM Cloudant NoSQL データベースでは、データは自動的にバックアップされません。 用意されている[バックアップとリストアのメカニズム](/docs/services/Cloudant/guides/backup-cookbook.html#cloudant-nosql-db-backup-and-recovery)を使用して、サイトの障害からデータを保護できます。</p> <p> クラスター内のサービスを使用するには、クラスター内の名前空間に [{{site.data.keyword.Bluemix_notm}} サービスをバインド](cs_integrations.html#adding_app)する必要があります。 サービスをクラスターにバインドすると、Kubernetes シークレットが作成されます。 Kubernetes シークレットは、サービスへの URL、ユーザー名、およびパスワードなど、サービスに関する機密情報を保持します。 シークレットをシークレット・ボリュームとしてポッドにマウントして、シークレット内の資格情報を使用することによりサービスにアクセスできます。 シークレット・ボリュームを他のポッドにマウントすることにより、ポッド間でデータを共有することもできます。 コンテナーがクラッシュしたとき、またはポッドがワーカー・ノードから削除されたときでも、データは削除されないので、シークレット・ボリュームをマウントした他のポッドから引き続きアクセスできます。 <p>{{site.data.keyword.Bluemix_notm}} のデータベース・サービスのほとんどは、少量のデータ用のディスク・スペースを無料で提供しているため、機能をテストすることができます。</p></td>
  </tr>
  <tr>
    <td>3. 社内データベース</td>
    <td>法的な理由でデータをオンサイトに保管しなければならない場合は、オンプレミス・データベースへの [VPN 接続をセットアップ](cs_vpn.html#vpn)し、データ・センター内の既存のストレージ、バックアップと複製のメカニズムを使用できます。</td>
  </tr>
  </tbody>
  </table>

{: caption="表。 Kubernetes クラスターでのデプロイメントのための永続データ・ストレージのオプション" caption-side="top"}

<br />



## クラスターでの既存の NFS ファイル共有の使用
{: #existing}

IBM Cloud インフラストラクチャー (SoftLayer) アカウントにある既存の NFS ファイル共有を使用するには、既存のストレージの永続ボリューム (PV) を作成します。
{:shortdesc}

永続ボリューム (PV) は、データ・センター内にプロビジョンされている実際のストレージ・デバイスを表す Kubernetes リソースです。 永続ボリュームは、特定のストレージ・タイプを {{site.data.keyword.Bluemix_notm}} Storage がプロビジョンする詳細な方法を概念化したものです。 PV をクラスターにマウントするには、永続ボリューム請求 (PVC) を作成して、ポッドの永続ストレージを要求する必要があります。 次の図は、PV と PVC の間の関係を示しています。

![永続ボリュームと永続ボリューム請求の作成](images/cs_cluster_pv_pvc.png)

図に示すように、既存の NFS ストレージを使用できるようにするには、特定のサイズとアクセス・モードを持つ PV を作成し、その PV の仕様と一致する PVC を作成する必要があります。PV と PVC が一致すると、それらは相互にバインドされます。 クラスター・ユーザーがデプロイメントへのボリュームのマウントに使用できるのは、バインドされた PVC だけです。 この処理は永続ストレージの静的プロビジョニングと呼ばれます。

始める前に、PV の作成に使用できる既存の NFS ファイル共有があることを確認します。 例えば、過去に [`retain` ストレージ・クラス・ポリシーを使用して PVC を作成](#create)していた場合は、既存の NFS ファイル共有に保存されているデータを、この新しい PVC に使用できます。

**注:** 永続ストレージの静的プロビジョニングは、既存の NFS ファイル共有にのみ適用されます。既存の NFS ファイル共有がない場合、クラスター・ユーザーは[動的プロビジョニング](cs_storage.html#create)処理を使用して PV を追加できます。

PV および一致する PVC を作成するには、以下の手順を実行します。

1.  IBM Cloud インフラストラクチャー (SoftLayer) アカウントで、PV オブジェクトを作成する NFS ファイル共有の ID とパスを検索します。 さらに、クラスター内のサブネットに対する許可をファイル・ストレージに付与します。 この許可により、クラスターにストレージへのアクセス権限が付与されます。
    1.  IBM Cloud インフラストラクチャー (SoftLayer) アカウントにログインします。
    2.  **「ストレージ」**をクリックします。
    3.  **「File Storage」**をクリックして、**「アクション」**メニューから**「ホストの許可」**を選択します。
    4.  **「サブネット」**を選択します。
    5.  ドロップダウン・リストから、ワーカー・ノードが接続されているプライベート VLAN サブネットを選択します。 ワーカー・ノードのサブネットを見つけるには、`bx cs workers <cluster_name>` を実行して、ワーカー・ノードの `Private IP` をドロップダウン・リストにあるサブネットと比較します。
    6.  **「送信」**をクリックします。
    6.  ファイル・ストレージの名前をクリックします。
    7.  **「マウント・ポイント (Mount Point)」**フィールドをメモします。フィールドは `<server>:/<path>` の形式で表示されます。
2.  PV のストレージ構成ファイルを作成します。 ファイル・ストレージの**「マウント・ポイント」**フィールドにあるサーバーとパスを含めます。

    ```
    apiVersion: v1
    kind: PersistentVolume
    metadata:
     name: mypv
    spec:
     capacity:
       storage: "20Gi"
     accessModes:
       - ReadWriteMany
     nfs:
       server: "nfslon0410b-fz.service.networklayer.com"
       path: "/IBM01SEV8491247_0908/data01"
    ```
    {: codeblock}

    <table>
    <caption>YAML ファイルの構成要素について</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
    </thead>
    <tbody>
    <tr>
    <td><code>name</code></td>
    <td>作成する PV オブジェクトの名前を入力します。</td>
    </tr>
    <tr>
    <td><code>spec/capacity/storage</code></td>
    <td>既存の NFS ファイル共有のストレージ・サイズを入力します。 このストレージ・サイズはギガバイト単位 (例えば、20Gi (20 GB) や 1000Gi (1 TB) など) で入力する必要があり、そのサイズは既存のファイル共有のサイズと一致している必要があります。</td>
    </tr>
    <tr>
    <td><code>accessMode</code></td>
    <td>アクセス・モードは、PVC をワーカー・ノードにマウントする方法を定義します。<ul><li>ReadWriteOnce (RWO): PV は、単一のワーカー・ノードのデプロイメントにのみマウントできます。 この PV にマウントされているデプロイメントのコンテナーは、当該ボリュームに対する読み取り/書き込みを行うことができます。</li><li>ReadOnlyMany (ROX): PV は、複数のワーカー・ノードでホストされているデプロイメントにマウントできます。 この PV にマウントされているデプロイメントは、ボリュームからのみ読み取ることができます。</li><li>ReadWriteMany (RWX): この PV は、複数のワーカー・ノードでホストされているデプロイメントにマウントできます。 この PV にマウントされているデプロイメントは、当該ボリュームに対する読み取り/書き込みを行うことができます。</li></ul></td>
    </tr>
    <tr>
    <td><code>spec/nfs/server</code></td>
    <td>NFS ファイル共有サーバーの ID を入力します。</td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td>PV オブジェクトを作成する NFS ファイル共有のパスを入力します。</td>
    </tr>
    </tbody></table>

3.  クラスターに PV オブジェクトを作成します。

    ```
    kubectl apply -f deploy/kube-config/mypv.yaml
    ```
    {: pre}

4.  PV が作成されたことを確認します。

    ```
    kubectl get pv
    ```
    {: pre}

5.  PVC を作成するために、別の構成ファイルを作成します。 PVC が、以前に作成した PV オブジェクトと一致するようにするには、`storage` および `accessMode` に同じ値を選択する必要があります。`storage-class` フィールドは空である必要があります。 これらのいずれかのフィールドが PV と一致しない場合、代わりに新しい PV が自動的に作成されます。

    ```
    kind: PersistentVolumeClaim
    apiVersion: v1
    metadata:
     name: mypvc
     annotations:
       volume.beta.kubernetes.io/storage-class: ""
    spec:
     accessModes:
       - ReadWriteMany
     resources:
       requests:
         storage: "20Gi"
    ```
    {: codeblock}

6.  PVC を作成します。

    ```
    kubectl apply -f deploy/kube-config/mypvc.yaml
    ```
    {: pre}

7.  PVC が作成され、PV オブジェクトにバインドされたことを確認します。 この処理には数分かかる場合があります。

    ```
    kubectl describe pvc mypvc
    ```
    {: pre}

    出力例:

    ```
    Name: mypvc
    Namespace: default
    StorageClass:	""
    Status: Bound
    Volume: pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    Labels: <none>
    Capacity: 20Gi
    Access Modes: RWX
    Events:
      FirstSeen LastSeen Count From        SubObjectPath Type Reason Message
      --------- -------- ----- ----        ------------- -------- ------ -------
      3m 3m 1 {ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 } Normal Provisioning External provisioner is provisioning volume for claim "default/my-persistent-volume-claim"
      3m 1m	 10 {persistentvolume-controller } Normal ExternalProvisioning cannot find provisioner "ibm.io/ibmc-file", expecting that a volume for the claim is provisioned either manually or via external software
      1m 1m 1 {ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 } Normal ProvisioningSucceeded	Successfully provisioned volume pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    ```
    {: screen}


PV オブジェクトが正常に作成され、PVC にバインドされました。 これで、クラスター・ユーザーがデプロイメントに[PVC をマウント](#app_volume_mount)して、PV オブジェクトへの読み書きを開始できるようになりました。

<br />



## クラスターでの既存のブロック・ストレージの使用
{: #existing_block}

まず始めに、PV の作成に使用できる既存のブロック・ストレージ・インスタンスがあることを確認します。 例えば、過去に [`retain` ストレージ・クラス・ポリシーを使用して PVC を作成](#create)していた場合は、既存のブロック・ストレージに保存されているデータを、この新しい PVC に使用できます。

**注**: ブロック・ストレージは、`ReadWriteOnce` アクセス・モード・デバイスです。これは、一度にクラスター内の 1 つのワーカー・ノードの 1 つのポッドにのみマウントできます。

PV および一致する PVC を作成するには、以下の手順を実行します。

1.  IBM Cloud インフラストラクチャー (SoftLayer) アカウントの API キーを取得または生成します。
    1. [IBM Cloud インフラストラクチャー (SoftLayer) ポータル ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://control.softlayer.com/) にログインします。
    2. **「アカウント」**、**「ユーザー」**、**「ユーザー・リスト」**の順に選択します。
    3. 自分のユーザー ID を見つけます。
    4. **「API キー」**列で、**「生成」**をクリックして API キーを生成するか、または**「表示」**をクリックして既存の API キーを表示します。
2.  IBM Cloud インフラストラクチャー (SoftLayer) アカウントの API ユーザー名を取得します。
    1. **「ユーザー・リスト」**メニューで、自分のユーザー ID を選択します。
    2. **「API アクセス情報」**セクションで、自分の**「API ユーザー名」**を見つけます。
3.  IBM Cloud インフラストラクチャー CLI プラグインにログインします。
    ```
    bx sl init
    ```
    {: pre}

4.  IBM Cloud インフラストラクチャー (SoftLayer) アカウントのユーザー名と API キーを使用して認証を受けることを選択します。
5.  前の手順で取得したユーザー名と API キーを入力します。
6.  使用可能なブロック・ストレージ・デバイスをリストします。
    ```
    bx sl block volume-list
    ```
    {: pre}

    出力例:
    ```
    id         username            datacenter   storage_type              capacity_gb   bytes_used   ip_addr         lunId   active_transactions
    38642141   IBM02SEL1543159-1   dal10        endurance_block_storage   20            -            169.xx.xxx.xxx   170     0
    ```
    {: screen}

7.  クラスターにマウントするブロック・ストレージ・デバイスの `id`、`ip_addr`、`capacity_gb`、`lunId` をメモします。
8.  PV の構成ファイルを作成します。 前の手順で取得したブロック・ストレージの ID、IP アドレス、サイズ、lun ID を指定してください。

    ```
    apiVersion: v1
    kind: PersistentVolume
    metadata:
      name: mypv
    spec:
      capacity:
        storage: "<storage_size>"
      accessModes:
        - ReadWriteOnce
      flexVolume:
        driver: "ibm/ibmc-block"
        fsType: "<fs_type>"
        options:
          "Lun": "<lun_ID>"
          "TargetPortal": "<IP_address>"
          "VolumeID": "<volume_ID>"
          "volumeName": "<volume_name>"
      ```
      {: codeblock}

    <table>
    <caption>YAML ファイルの構成要素について</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
    </thead>
    <tbody>
    <tr>
    <td><code>metadata/name</code></td>
    <td>作成する PV の名前を入力します。</td>
    </tr>
    <tr>
    <td><code>spec/flexVolume/fsType</code></td> 
    <td>既存のブロック・ストレージ用に構成されているファイル・システム・タイプを入力します。<code>ext4</code> または <code>xfs</code> のいずれかを選択します。このオプションを指定しない場合、PV はデフォルトで <code>ext4</code> に設定されます。誤った fsType が定義された場合、PV の作成は成功しますが、PV のポッドへのマウントは失敗します。</td></tr>	    
    <tr>
    <td><code>spec/capacity/storage</code></td>
    <td>前の手順で取得した既存のブロック・ストレージのストレージ・サイズ (<code>capacity-gb</code>) を入力します。 このストレージ・サイズはギガバイト単位 (例えば、20Gi (20 GB) や 1000Gi (1 TB) など) で入力する必要があります。</td>
    </tr>
    <tr>
    <td><code>flexVolume/options/Lun</code></td>
    <td>前の手順で取得したブロック・ストレージの lun ID (<code>lunId</code>) を入力します。</td>
    </tr>
    <tr>
    <td><code>flexVolume/options/TargetPortal</code></td>
    <td>前の手順で取得したブロック・ストレージの IP アドレス (<code>ip_addr</code>) を入力します。 </td>
    </tr>
    <tr>
	    <td><code>flexVolume/options/VolumeId</code></td>
	    <td>前の手順で取得したブロック・ストレージの ID (<code>id</code>) を入力します。</td>
	    </tr>
	    <tr>
		    <td><code>flexVolume/options/volumeName</code></td>
		    <td>ボリュームの名前を入力します。</td>
	    </tr>
    </tbody></table>

9.  クラスターに PV を作成します。
    ```
    kubectl apply -f mypv.yaml
    ```
    {: pre}

10. PV が作成されたことを確認します。
    ```
    kubectl get pv
    ```
    {: pre}

11. PVC を作成するために、別の構成ファイルを作成します。 PVC が、前の手順で作成した PV と一致するようにするには、`storage` および `accessMode` に同じ値を選択する必要があります。 `storage-class` フィールドは空である必要があります。 これらのいずれかのフィールドが PV と一致しない場合、代わりに新しい PV が自動的に作成されます。

     ```
     kind: PersistentVolumeClaim
     apiVersion: v1
     metadata:
      name: mypvc
      annotations:
        volume.beta.kubernetes.io/storage-class: ""
     spec:
      accessModes:
        - ReadWriteOnce
      resources:
        requests:
          storage: "20Gi"
     ```
     {: codeblock}

12.  PVC を作成します。
     ```
     kubectl apply -f mypvc.yaml
     ```
     {: pre}

13.  PVC が作成され、先ほど作成した PV にバインドされたことを確認します。 この処理には数分かかる場合があります。
     ```
     kubectl describe pvc mypvc
     ```
     {: pre}

     出力例:

     ```
     Name: mypvc
     Namespace: default
     StorageClass:	""
     Status: Bound
     Volume: pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
     Labels: <none>
     Capacity: 20Gi
     Access Modes: RWO
     Events:
       FirstSeen LastSeen Count From        SubObjectPath Type Reason Message
       --------- -------- ----- ----        ------------- -------- ------ -------
       3m 3m 1 {ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 } Normal Provisioning External provisioner is provisioning volume  for claim "default/my-persistent-volume-claim"
       3m 1m	 10 {persistentvolume-controller } Normal ExternalProvisioning cannot find provisioner "ibm.io/ibmc-block", expecting that  a volume for the claim is provisioned either manually or via external software
       1m 1m 1 {ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 } Normal ProvisioningSucceeded	Successfully provisioned volume  pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
     ```
     {: screen}

PV が正常に作成され、PVC にバインドされました。 これで、クラスター・ユーザーがデプロイメントに[PVC をマウント](#app_volume_mount)して、PV への読み書きを開始できるようになりました。

<br />



## アプリへの NFS ファイル・ストレージまたはブロック・ストレージの追加
{: #create}

NFS ファイル・ストレージまたはブロック・ストレージをクラスターにプロビジョンするために、永続ボリューム請求 (PVC) を作成します。 その後、この請求を永続ボリューム (PV) にマウントすることで、ポッドがクラッシュしたりシャットダウンしたりしてもデータを利用できるようにします。
{:shortdesc}

PV の基礎となる NFS ファイル・ストレージおよびブロック・ストレージは、データの高可用性を実現するために IBM がクラスター化しています。 ストレージ・クラスとは、提供されているストレージ・オファリングのタイプを表し、PV の作成時にデータ保存ポリシー、サイズ (GB)、IOPS などの特性を定義するものです。

**注**: ブロック・ストレージは、`ReadWriteOnce` アクセス・モード・デバイスです。これは、一度にクラスター内の 1 つのワーカー・ノードの 1 つのポッドにのみマウントできます。NFS ファイル・ストレージは `ReadWriteMany` アクセス・モードであるため、クラスター内の複数のワーカーにわたり、複数のポッドにマウントできます。

開始前に、以下のことを行います。
- ファイアウォールがある場合は、クラスターのあるロケーションの IBM Cloud インフラストラクチャー (SoftLayer) の IP 範囲に[発信アクセスを許可](cs_firewall.html#pvc)し、PVC を作成できるようにします。
- ブロック・ストレージをアプリにマウントする場合は、まず[ブロック・ストレージ用の {{site.data.keyword.Bluemix_notm}} Storage プラグイン](#install_block)をインストールする必要があります。

永続ストレージを追加するには、以下のようにします。

1.  使用可能なストレージ・クラスを確認します。 {{site.data.keyword.containerlong}} には NFS ファイル・ストレージおよびブロック・ストレージ用に事前定義されたストレージ・クラスがあるので、クラスター管理者がストレージ・クラスを作成する必要はありません。 `ibmc-file-bronze` ストレージ・クラスは `default` ストレージ・クラスと同じです。 デフォルトでは、ファイル・ストレージは `nfs` ファイル・システムでプロビジョンされ、ブロック・ストレージは `ext4` ファイル・システムでプロビジョンされます。`XFS` ファイル・システムでブロック・ストレージをプロビジョンするには、[独自のカスタム・ストレージ・クラスを作成します](#custom_storageclass)。 

    ```
    kubectl get storageclasses
    ```
    {: pre}

    ```
    $ kubectl get storageclasses
    NAME                         TYPE
    default                      ibm.io/ibmc-file
    ibmc-file-bronze (default)   ibm.io/ibmc-file
    ibmc-file-custom             ibm.io/ibmc-file
    ibmc-file-gold               ibm.io/ibmc-file
    ibmc-file-retain-bronze      ibm.io/ibmc-file
    ibmc-file-retain-custom      ibm.io/ibmc-file
    ibmc-file-retain-gold        ibm.io/ibmc-file
    ibmc-file-retain-silver      ibm.io/ibmc-file
    ibmc-file-silver             ibm.io/ibmc-file
    ibmc-block-custom            ibm.io/ibmc-block
    ibmc-block-bronze            ibm.io/ibmc-block
    ibmc-block-gold              ibm.io/ibmc-block
    ibmc-block-silver            ibm.io/ibmc-block
    ibmc-block-retain-bronze     ibm.io/ibmc-block
    ibmc-block-retain-silver     ibm.io/ibmc-block
    ibmc-block-retain-gold       ibm.io/ibmc-block
    ibmc-block-retain-custom     ibm.io/ibmc-block
    ```
    {: screen}

    **ヒント:** デフォルトのストレージ・クラスを変更する場合は、`kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` を実行して、`<storageclass>` をストレージ・クラスの名前に置き換えます。

2.  PVC を削除した後にデータと NFS ファイル共有またはブロック・ストレージを保持するかどうかを決めます。
    - データを保持する場合、`retain` ストレージ・クラスを選択します。 PVC を削除すると、PVC のみが削除されます。PV は引き続きクラスター内に存在し、その中のデータは保存されますが、別の PVC で再使用することはできません。また、NFS ファイルまたはブロック・ストレージ、およびデータが IBM Cloud インフラストラクチャー (SoftLayer) アカウントに引き続き存在します。後でクラスターでこのデータにアクセスするには、PVC を作成し、既存の [NFS ファイル](#existing)または[ブロック](#existing_block)・ストレージを指定して、対応する PV を作成します。 
    - PVC を削除するときに PV、データ、および NFS ファイル共有またはブロック・ストレージも削除するには、`retain` のないストレージ・クラスを選択します。

3.  **ブロンズ、シルバー、ゴールドのストレージ・クラスを選択した場合**: クラスごとに IOPS/GB が定義された[エンデュランス・ストレージ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://knowledgelayer.softlayer.com/topic/endurance-storage) が割り当てられます。 しかし、使用可能な範囲内のサイズを選択して、合計 IOPS を決めることができます。 使用可能なサイズの範囲内で、任意の整数のギガバイト・サイズ (20 Gi、256 Gi、11854 Gi など) を選択できます。 例えば、4 IOPS/GB のシルバー・ストレージ・クラスで 1000Gi のファイル共有またはブロック・ストレージのサイズを選択すると、ボリュームの合計 IOPS は 4000 になります。 PV の IOPS が多いほど、入出力操作の処理が高速になります。 次の表に、各ストレージ・クラスの IOPS/GB とサイズの範囲を示します。

    <table>
         <caption>ストレージ・クラスのサイズの範囲と IOPS/GB を示す表</caption>
         <thead>
         <th>ストレージ・クラス</th>
         <th>IOPS/GB</th>
         <th>サイズの範囲 (ギガバイト単位)</th>
         </thead>
         <tbody>
         <tr>
         <td>ブロンズ (デフォルト)</td>
         <td>2 IOPS/GB</td>
         <td>20 から 12000 Gi</td>
         </tr>
         <tr>
         <td>シルバー</td>
         <td>4 IOPS/GB</td>
         <td>20 から 12000 Gi</td>
         </tr>
         <tr>
         <td>ゴールド</td>
         <td>10 IOPS/GB</td>
         <td>20 から 4000 Gi</td>
         </tr>
         </tbody></table>

    <p>**ストレージ・クラスの詳細を表示するコマンドの例**:</p>

    <pre class="pre"><code>kubectl describe storageclasses ibmc-file-silver</code></pre>

4.  ** カスタム・ストレージ・クラスを選択した場合**: [パフォーマンス・ストレージ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://knowledgelayer.softlayer.com/topic/performance-storage) が割り当てられるので、IOPS とサイズの組み合わせを、より細かく選択できます。 例えば、PVC で 40Gi のサイズを選択した場合、100 から 2000 までの範囲にある 100 の倍数を IOPS として選択できます。 選択する IOPS は静的であり、ストレージのサイズに合わせてスケーリングされません。40Gi と 100 IOPS を選択した場合、合計 IOPS は 100 のままです。次の表に、選択したサイズに応じて選択可能な IOPS の範囲を示します。

    <table>
         <caption>カスタム・ストレージ・クラスのサイズ範囲と IOPS を示す表</caption>
         <thead>
         <th>サイズの範囲 (ギガバイト単位)</th>
         <th>IOPS の範囲 (100 の倍数)</th>
         </thead>
         <tbody>
         <tr>
         <td>20 から 39 Gi</td>
         <td>100 から 1000 IOPS</td>
         </tr>
         <tr>
         <td>40 から 79 Gi</td>
         <td>100 から 2000 IOPS</td>
         </tr>
         <tr>
         <td>80 から 99 Gi</td>
         <td>100 から 4000 IOPS</td>
         </tr>
         <tr>
         <td>100 から 499 Gi</td>
         <td>100 から 6000 IOPS</td>
         </tr>
         <tr>
         <td>500 から 999 Gi</td>
         <td>100 から 10000 IOPS</td>
         </tr>
         <tr>
         <td>1000 から 1999 Gi</td>
         <td>100 から 20000 IOPS</td>
         </tr>
         <tr>
         <td>2000 から 2999 Gi</td>
         <td>200 から 40000 IOPS</td>
         </tr>
         <tr>
         <td>3000 から 3999 Gi</td>
         <td>200 から 48000 IOPS</td>
         </tr>
         <tr>
         <td>4000 から 7999 Gi</td>
         <td>300 から 48000 IOPS</td>
         </tr>
         <tr>
         <td>8000 から 9999 Gi</td>
         <td>500 から 48000 IOPS</td>
         </tr>
         <tr>
         <td>10000 から 12000 Gi</td>
         <td>1000 から 48000 IOPS</td>
         </tr>
         </tbody></table>

    <p>**カスタム・ストレージ・クラスの詳細を表示するコマンドの例**:</p>

    <pre class="pre"><code>kubectl describe storageclasses ibmc-file-retain-custom</code></pre>

5.  時間単位と月単位のどちらの課金方法にするかを決定します。デフォルトでは、月単位で課金されます。

6.  PVC を定義した構成ファイルを作成し、`.yaml` ファイルとして構成を保存します。

    -  **ブロンズ、シルバー、ゴールドのストレージ・クラスの例**:
       以下の `.yaml` ファイルは、名前が `mypvc`、ストレージ・クラスが `"ibmc-file-silver"`、課金が `"hourly"`、ギガバイト・サイズが `24Gi` の請求を作成します。 ブロック・ストレージをクラスターにマウントするために PVC を作成する場合は、必ず、`accessModes` セクションに `ReadWriteOnce` と入力してください。

       ```
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: mypvc
         annotations:
           volume.beta.kubernetes.io/storage-class: "ibmc-file-silver"
         labels:
           billingType: "hourly"
       spec:
         accessModes:
           - ReadWriteMany
         resources:
           requests:
             storage: 24Gi
        ```
        {: codeblock}

    -  **カスタム・ストレージ・クラスの例**:
       以下の `.yaml` ファイルは、名前が `mypvc`、ストレージ・クラスが `ibmc-file-retain-custom`、課金がデフォルトの `"monthly"`、ギガバイト・サイズが `45Gi`、IOPS が `"300"` の請求を作成します。 ブロック・ストレージをクラスターにマウントするために PVC を作成する場合は、必ず、`accessModes` セクションに `ReadWriteOnce` と入力してください。

       ```
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: mypvc
         annotations:
           volume.beta.kubernetes.io/storage-class: "ibmc-file-retain-custom"
         labels:
           billingType: "monthly"
       spec:
         accessModes:
           - ReadWriteMany
         resources:
           requests:
             storage: 45Gi
             iops: "300"
        ```
        {: codeblock}

        <table>
        <caption>YAML ファイルの構成要素について</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
        </thead>
        <tbody>
        <tr>
        <td><code>metadata/name</code></td>
        <td>PVC の名前を入力します。</td>
        </tr>
        <tr>
        <td><code>metadata/annotations</code></td>
        <td>PV のためのストレージ・クラスを指定します。
          <ul>
          <li>ibmc-file-bronze / ibmc-file-retain-bronze: 2 IOPS/GB。</li>
          <li>ibmc-file-silver / ibmc-file-retain-silver: 4 IOPS/GB。</li>
          <li>ibmc-file-gold / ibmc-file-retain-gold: 10 IOPS/GB。</li>
          <li>ibmc-file-custom / ibmc-file-retain-custom: 複数の IOPS の値を使用できます。</li>
          <li>ibmc-block-bronze / ibmc-block-retain-bronze : 2 IOPS/GB。</li>
          <li>ibmc-block-silver / ibmc-block-retain-silver: 4 IOPS/GB。</li>
          <li>ibmc-block-gold / ibmc-block-retain-gold: 10 IOPS/GB。</li>
          <li>ibmc-block-custom / ibmc-block-retain-custom: 複数の IOPS の値を使用できます。</li></ul>
          <p>ストレージ・クラスを指定しなかった場合は、デフォルト・ストレージ・クラスを使用して PV が作成されます。</p><p>**ヒント:** デフォルトのストレージ・クラスを変更する場合は、<code>kubectl patch storageclass &lt;storageclass&gt; -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'</code> を実行して、<code>&lt;storageclass&gt;</code> をストレージ・クラスの名前に置き換えます。</p></td>
        </tr>
        <tr>
          <td><code>metadata/labels/billingType</code></td>
          <td>ストレージの課金額を計算する頻度として「monthly」または「hourly」を指定します。 デフォルトは「monthly」です。</td>
        </tr>
        <tr>
        <td><code>spec/resources/requests/storage</code></td>
        <td>ファイル・ストレージのサイズをギガバイト (Gi) 単位で入力します。 使用可能なサイズ範囲内の整数を選択してください。 </br></br><strong>注:</strong> ストレージがプロビジョンされた後は、NFS ファイル共有またはブロック・ストレージのサイズを変更できません。 保管するデータの量に一致するサイズを指定してください。 </td>
        </tr>
        <tr>
        <td><code>spec/resources/requests/iops</code></td>
        <td>このオプションは、カスタム・ストレージ・クラス (`ibmc-file-custom / ibmc-file-retain-custom / ibmc-block-custom / ibmc-block-retain-custom`)でのみ使用できます。 許容範囲内で 100 の倍数を選択して、ストレージの合計 IOPS を指定します。 すべてのオプションを表示するには、`kubectl describe storageclasses <storageclass>` を実行します。 リストされているもの以外の IOPS を選択した場合、その IOPS は切り上げられます。</td>
        </tr>
        </tbody></table>
	
    カスタマイズされたストレージ・クラスを使用する場合は、対応するストレージ・クラス名、有効な IOPS、およびサイズを指定して PVC を作成します。   
    {: tip}

7.  PVC を作成します。

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

8.  PVC が作成され、PV にバインドされたことを確認します。 この処理には数分かかる場合があります。

    ```
    kubectl describe pvc mypvc
    ```
    {: pre}

    出力例:

    ```
    Name:		mypvc
    Namespace:	default
    StorageClass:	""
    Status:		Bound
    Volume:		pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    Labels:		<none>
    Capacity:	20Gi
    Access Modes:	RWX
    Events:
      FirstSeen	LastSeen	Count	From								SubObjectPath	Type		Reason			Message
      ---------	--------	-----	----								-------------	--------	------			-------
      3m		3m		1	{ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 }			Normal		Provisioning		External provisioner is provisioning volume for claim "default/my-persistent-volume-claim"
      3m		1m		10	{persistentvolume-controller }							Normal		ExternalProvisioning	cannot find provisioner "ibm.io/ibmc-file", expecting that a volume for the claim is provisioned either manually or via external software
      1m		1m		1	{ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 }			Normal		ProvisioningSucceeded	Successfully provisioned volume pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2

    ```
    {: screen}

9.  {: #app_volume_mount}PVC をデプロイメントにマウントするには、構成 `.yaml` ファイルを作成します。

    ```
    apiVersion: apps/v1beta1
    kind: Deployment
    metadata:
      name: <deployment_name>
      labels:
        app: <deployment_label>
    spec:
      selector:
        matchLabels:
          app: <app_name>
      template:
        metadata:
          labels:
            app: <app_name>
        spec:
          containers:
          - image: <image_name>
            name: <container_name>
            volumeMounts:
            - name: <volume_name>
              mountPath: /<file_path>
          volumes:
          - name: <volume_name>
            persistentVolumeClaim:
              claimName: <pvc_name>
    ```
    {: codeblock}

    <table>
    <caption>YAML ファイルの構成要素について</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
    </thead>
    <tbody>
        <tr>
    <td><code>metadata/labels/app</code></td>
    <td>デプロイメントのラベル。</td>
      </tr>
      <tr>
        <td><code>spec/selector/matchLabels/app</code> <br/> <code>spec/template/metadata/labels/app</code></td>
        <td>アプリのラベル。</td>
      </tr>
    <tr>
    <td><code>template/metadata/labels/app</code></td>
    <td>デプロイメントのラベル。</td>
      </tr>
    <tr>
    <td><code>spec/containers/image</code></td>
    <td>使用するイメージの名前。 {{site.data.keyword.registryshort_notm}} アカウント内の使用可能なイメージをリストするには、`bx cr image-list` を実行します。</td>
    </tr>
    <tr>
    <td><code>spec/containers/name</code></td>
    <td>クラスターにデプロイするコンテナーの名前。</td>
    </tr>
    <tr>
    <td><code>spec/containers/volumeMounts/mountPath</code></td>
    <td>コンテナー内でボリュームがマウントされるディレクトリーの絶対パス。</td>
    </tr>
    <tr>
    <td><code>spec/containers/volumeMounts/name</code></td>
    <td>ポッドにマウントするボリュームの名前。</td>
    </tr>
    <tr>
    <td><code>volumes/name</code></td>
    <td>ポッドにマウントするボリュームの名前。 通常、この名前は <code>volumeMounts/name</code> と同じです。</td>
    </tr>
    <tr>
    <td><code>volumes/persistentVolumeClaim/claimName</code></td>
    <td>ボリュームとして使用する PVC の名前。 ボリュームをポッドにマウントすると、Kubernetes は PVC にバインドされた PV を識別して、その PV でユーザーが読み取り/書き込みを行えるようにします。</td>
    </tr>
    </tbody></table>

10.  デプロイメントを作成して、PVC をマウントします。
     ```
     kubectl apply -f <local_yaml_path>
     ```
     {: pre}

11.  ボリュームが正常にマウントされたことを確認します。

     ```
     kubectl describe deployment <deployment_name>
     ```
     {: pre}

     マウント・ポイントは **Volume Mounts** フィールドにあり、ボリュームは **Volumes** フィールドにあります。

     ```
      Volume Mounts:
           /var/run/secrets/kubernetes.io/serviceaccount from default-token-tqp61 (ro)
           /volumemount from myvol (rw)
     ...
     Volumes:
       myvol:
         Type:	PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
         ClaimName:	mypvc
         ReadOnly:	false
     ```
     {: screen}

{: #nonroot}
{: #enabling_root_permission}

**NFS アクセス権限**: NFS の非 root アクセス権限の有効化方法についての資料をお探しですか? [NFS ファイル・ストレージに対する非 root ユーザーのアクセス権限の追加](cs_troubleshoot_storage.html#nonroot)を参照してください。

<br />

	
## XFS ブロック・ストレージのストレージ・クラスのカスタマイズ
{: #custom_storageclass}

{{site.data.keyword.containerlong}} によって事前定義されているストレージ・クラスは、デフォルトで、`ext4` ファイル・システムによりブロック・ストレージをプロビジョンします。カスタマイズされたストレージ・クラスを作成すると、`XFS` ファイル・システムでブロック・ストレージをプロビジョンできます。
{: shortdesc}

開始前に、以下のことを行います。 
- [クラスターを Kubernetes CLI のターゲットとして設定](cs_cli_install.html#cs_cli_configure)します。
- [ブロック・ストレージ用の {{site.data.keyword.Bluemix_notm}} Storage プラグイン](#install_block)をインストールします。

カスタマイズしたストレージ・クラスを作成するには、以下のようにします。 
1. カスタマイズしたストレージ・クラスの yaml ファイルを作成します。 
   ```
   apiVersion: storage.k8s.io/v1
   kind: StorageClass
   metadata:
     name: ibmc-block-custom-xfs
     labels:
       addonmanager.kubernetes.io/mode: Reconcile
   provisioner: ibm.io/ibmc-block
   parameters:
     type: "Performance"
     sizeIOPSRange: |-
       [20-39]Gi:[100-1000]
       [40-79]Gi:[100-2000]
       [80-99]Gi:[100-4000]
       [100-499]Gi:[100-6000]
       [500-999]Gi:[100-10000]
       [1000-1999]Gi:[100-20000]
       [2000-2999]Gi:[200-40000]
       [3000-3999]Gi:[200-48000]
       [4000-7999]Gi:[300-48000]
       [8000-9999]Gi:[500-48000]
       [10000-12000]Gi:[1000-48000]
     fsType: "xfs"
     reclaimPolicy: "Delete"
     classVersion: "2"
   ```
   {: codeblock}
   
   ブロック・ストレージをクラスターから削除した後にデータを保持する場合は、`reclaimPolicy` を `Retain` に変更します。
   {: tip}
   
2. クラスター内にストレージ・クラスを作成します。 
   ```
   kubectl apply -f <filepath/xfs_storageclass.yaml>
   ```
   {: pre}
       
3. カスタマイズしたストレージ・クラスが作成されたことを確認します。 
   ```
   kubectl get storageclasses
   ```
   {: pre}

4. カスタマイズしたストレージ・クラスで [XFS ブロック・ストレージ](#create)をプロビジョンします。 

<br />


## デフォルトの NFS ファイル・ストレージ・バージョンの変更
{: #nfs_version}

NFS ファイル・ストレージのバージョンによって、NFS ファイル・ストレージ・サーバーとの通信に使用されるプロトコルが決まります。デフォルトでは、すべてのファイル・ストレージ・インスタンスは NFS バージョン 4 でセットアップされます。アプリが正しく機能するために特定のバージョンが必要な場合は、既存の PV を古い NFS バージョンに変更できます。
{: shortdesc}

デフォルトの NFS バージョンを変更するには、新規ストレージ・クラスを作成してクラスター内のファイル・ストレージを動的にプロビジョンするか、ポッドにマウントされている既存の PV を変更します。 

最新のセキュリティー更新を適用し、パフォーマンスを向上させるには、デフォルトの NFS バージョンを使用し、古い NFS バージョンに変更しないでください。
{: tip}

**必要な NFS バージョンでカスタマイズしたストレージ・クラスを作成するには、以下のようにします。**
1. カスタマイズしたストレージ・クラスの yaml ファイルを作成します。<nfs_version> を、使用する NFS バージョンで置き換えます。例えば、NFS バージョン 3.0 をプロビジョンする場合は、**3.0** と入力します。
   ```
   apiVersion: storage.k8s.io/v1
   kind: StorageClass
   metadata:
     name: ibmc-file-mount
     #annotations:
     #  storageclass.beta.kubernetes.io/is-default-class: "true"
     labels:
       kubernetes.io/cluster-service: "true"
   provisioner: ibm.io/ibmc-file
   parameters:
     type: "Endurance"
     iopsPerGB: "2"
     sizeRange: "[1-12000]Gi"
     reclaimPolicy: "Delete"
     classVersion: "2"
     mountOptions: nfsvers=<nfs_version>
   ```
   {: codeblock}
   
   ブロック・ストレージをクラスターから削除した後にデータを保持する場合は、`reclaimPolicy` を `Retain` に変更します。
   {: tip}
   
2. クラスター内にストレージ・クラスを作成します。 
   ```
   kubectl apply -f <filepath/nfsversion_storageclass.yaml>
   ```
   {: pre}
       
3. カスタマイズしたストレージ・クラスが作成されたことを確認します。 
   ```
   kubectl get storageclasses
   ```
   {: pre}

4. カスタマイズしたストレージ・クラスで[ファイル・ストレージ](#create)をプロビジョンします。 

**別の NFS バージョンを使用するように既存の PV を変更するには、以下のようにします。**

1. NFS バージョンを変更するファイル・ストレージの PV を取得し、PV の名前をメモします。
   ```
   kubectl get pv
   ```
   {: pre}

2. PV にアノテーションを追加します。`<version_number>` を、使用する NFS バージョンで置き換えます。例えば、NFS バージョン 3.0 に変更するには、**3** を入力します。  
   ```
   kubectl patch pv <pv_name> -p '{"metadata": {"annotations":{"volume.beta.kubernetes.io/mount-options":"vers=<version_number>"}}}'
   ```
   {: pre}
   
3. ファイル・ストレージを使用するポッドを削除し、ポッドを再作成します。 
   1. ポッド yaml をローカル・マシンに保存します。
      ```
      kubect get pod <pod_name> -o yaml > <filepath/pod.yaml>
      ```
      {: pre}
      
   2. ポッドを削除します。
      ```
      kubectl deleted pod <pod_name>
      ```
      {: pre}
   
   3. ポッドを再作成します。
      ```
      kubectl apply -f <filepath/pod.yaml>
      ```
      {: pre}

4. ポッドがデプロイされるまで待ちます。 
   ```
   kubectl get pods
   ```
   {: pre}
   
   status が `Running` に変更されたら、ポッドは完全にデプロイされています。 

5. ポッドにログインします。 
   ```
   kubectl exec -it <pod_name> sh
   ```
   {: pre}
   
6. 以前指定した NFS バージョンでファイル・ストレージがマウントされたことを確認します。 
   ```
   mount | grep "nfs" | awk -F" |," '{ print $5, $8 }'
   ```
   {: pre}
   
   出力例: 
   ```
   nfs vers=3.0
   ```
   {: screen}
   
<br />




## IBM Cloud インフラストラクチャー (SoftLayer) CLI のインストール
{: #slcli}

IBM Cloud インフラストラクチャー (SoftLayer) CLI をインストールして、NFS ファイル・インスタンスやブロック・ストレージ・インスタンスなどのインフラストラクチャー・リソースと対話します。
{: shortdesc}

始める前に、[Python 3.6 をインストール](https://www.python.org/downloads/)します。

1.  [インストール資料 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://softlayer-api-python-client.readthedocs.io/en/latest/install/) を表示します。

    1.  **tarball のダウンロード**・ステップまたは **zipball のダウンロード**・ステップのリンクをクリックします。`curl` コマンドは使用しないでください。
    2.  ダウンロードしたパッケージを見つけて unzip し、そのディレクトリーにナビゲートします。
    3.  CLI をインストールします。
    
        ```
        python3 setup.py install
        ```
        {: pre}
    
2.  IBM Cloud インフラストラクチャー (SoftLayer) API のユーザー名と API キーを取得します。

    1.  [{{site.data.keyword.Bluemix_notm}} コンソール](https://console.bluemix.net/)から、メニューを展開し、**「インフラストラクチャー」**を選択します。
    2.  メニュー・バーの自分のプロファイルから、使用するインフラストラクチャー・アカウントを選択します。
    3.  **「アカウント」** > **「ユーザー」** > **「ユーザー・リスト」**を選択します。
    4.  **「ユーザー」**テーブルの**「API キー」**列で、**「表示」**をクリックします。API キーが表示されない場合は、**「生成」**をクリックします。
    5.  ポップアップ・ウィンドウでユーザー名と API キーをコピーします。

3.  IBM Cloud インフラストラクチャー (SoftLayer) アカウントに接続するように CLI を構成します。

    1.  IBM Cloud インフラストラクチャー (SoftLayer) CLI を構成します。
        ```
        slcli setup
        ```
        {: pre}

    2.  必要な情報を入力します。
    
        * **Username**: 先ほど取得した IBM Cloud インフラストラクチャー (SoftLayer) API のユーザー名を入力します。
        * **API Key or Password**: 先ほど取得した IBM Cloud インフラストラクチャー (SoftLayer) API キーを入力します。
        * **Endpoint (public|private|custom) [public]**: `https://api.softlayer.com/rest/v3.1` と入力します。
        * **Timeout [0]**: CLI が API からの応答を待機する秒数の値を入力します。値 `0` を入力すると、CLI が永久に待機するように設定されます。
        
        **例**:
        
        ```
        $ slcli setup
        Username []: 1234567_user.name@example.com
        API Key or Password []:         Endpoint (public|private|custom) [public]: https://api.softlayer.com/rest/v3.1 
        Timeout [0]: 6000
        :..............:..................................................................:
        :         name : value                                                            :
        :..............:..................................................................:
        :     Username : 1234567_user.name@example.com                                    :
        :      API Key : 1111aa1111bbb22222b2b3c33333c3c3cc44d4444444444dd4444eee55e5e5e5 :
        : Endpoint URL : https://api.softlayer.com/xmlrpc/v3.1/                           :
        :      Timeout : 6000                                                             :
        :..............:..................................................................:
        Are you sure you want to write settings to "/Users/name/.softlayer"? [Y/n]: Y
        Configuration Updated Successfully
        ```
        {: screen}

これで、IBM Cloud インフラストラクチャー (SoftLayer) CLI を使用する準備ができました。

## クラスターへの {{site.data.keyword.Bluemix_notm}} Block Storage プラグインのインストール
{: #install_block}

ブロック・ストレージ用に事前定義されたストレージ・クラスをセットアップするには、{{site.data.keyword.Bluemix_notm}} Block Storage プラグインと Helm チャートをインストールします。 これらのストレージ・クラスを使用すると、アプリ用にブロック・ストレージをプロビジョンするための PVC を作成できます。
{: shortdesc}

まず始めに、[CLI のターゲット](cs_cli_install.html#cs_cli_configure)を、{{site.data.keyword.Bluemix_notm}} Block Storage プラグインをインストールするクラスターに設定してください。

1. {{site.data.keyword.Bluemix_notm}} Block Storage プラグインを使用したいクラスターに、[Helm](cs_integrations.html#helm) をインストールします。
2. Helm リポジトリーを更新して、このリポジトリーにあるすべての Helm チャートの最新バージョンを取得します。
   ```
   helm repo update
   ```
   {: pre}

3. {{site.data.keyword.Bluemix_notm}} Block Storage プラグインをインストールします。 このプラグインをインストールすると、事前定義されたブロック・ストレージ・クラスがクラスターに追加されます。
   ```
   helm install ibm/ibmcloud-block-storage-plugin
   ```
   {: pre}

   出力例:
   ```
   NAME:   bald-olm
   LAST DEPLOYED: Wed Apr 18 10:02:55 2018
   NAMESPACE: default
   STATUS: DEPLOYED

   RESOURCES:
   ==> v1beta1/DaemonSet
   NAME                           DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE SELECTOR  AGE
   ibmcloud-block-storage-driver  0        0        0      0           0          <none>         0s

   ==> v1beta1/Deployment
   NAME                           DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
   ibmcloud-block-storage-plugin  1        0        0           0          0s

   ==> v1/StorageClass
   NAME                      PROVISIONER        AGE
   ibmc-block-bronze         ibm.io/ibmc-block  0s
   ibmc-block-custom         ibm.io/ibmc-block  0s
   ibmc-block-gold           ibm.io/ibmc-block  0s
   ibmc-block-retain-bronze  ibm.io/ibmc-block  0s
   ibmc-block-retain-custom  ibm.io/ibmc-block  0s
   ibmc-block-retain-gold    ibm.io/ibmc-block  0s
   ibmc-block-retain-silver  ibm.io/ibmc-block  0s
   ibmc-block-silver         ibm.io/ibmc-block  0s

   ==> v1/ServiceAccount
   NAME                           SECRETS  AGE
   ibmcloud-block-storage-plugin  1        0s

   ==> v1beta1/ClusterRole
   NAME                           AGE
   ibmcloud-block-storage-plugin  0s

   ==> v1beta1/ClusterRoleBinding
   NAME                           AGE
   ibmcloud-block-storage-plugin  0s

   NOTES:
   Thank you for installing: ibmcloud-block-storage-plugin.   Your release is named: bald-olm
   ```
   {: screen}

4. 正常にインストールされたことを確認します。
   ```
   kubectl get pod -n kube-system | grep block
   ```
   {: pre}

   出力例:
   ```
   ibmcloud-block-storage-driver-kh4mt                              1/1       Running   0          27d       10.118.98.19   10.118.98.19
   ibmcloud-block-storage-plugin-58c5f9dc86-pbl4t                   1/1       Running   0          14d       172.21.0.204   10.118.98.19
   ```
   {: screen}
   
   1 つの `ibmcloud-block-storage-plugin` ポッドと 1 つ以上の `ibmcloud-block-storage-driver` ポッドが表示されたら、インストールは成功しています。`ibmcloud-block-storage-driver` の数は、クラスター内のワーカー・ノードの数と等しくなります。すべてのポッドが **Running** 状態である必要があります。 

5. ブロック・ストレージのストレージ・クラスがクラスターに追加されたことを確認します。
   ```
   kubectl get storageclasses | grep block
   ```
   {: pre}

   出力例:
   ```
   ibmc-block-bronze            ibm.io/ibmc-block
   ibmc-block-custom            ibm.io/ibmc-block
   ibmc-block-gold              ibm.io/ibmc-block
   ibmc-block-retain-bronze     ibm.io/ibmc-block
   ibmc-block-retain-custom     ibm.io/ibmc-block
   ibmc-block-retain-gold       ibm.io/ibmc-block
   ibmc-block-retain-silver     ibm.io/ibmc-block
   ibmc-block-silver            ibm.io/ibmc-block
   ```
   {: screen}

6. ブロック・ストレージをプロビジョンするすべてのクラスターに対して、上記の手順を繰り返します。

これで、アプリ用にブロック・ストレージをプロビジョンするための [PVC の作成](#create)に進むことができます。

<br />


### {{site.data.keyword.Bluemix_notm}} Block Storage プラグインの更新
既存の {{site.data.keyword.Bluemix_notm}} Block Storage プラグインを最新バージョンにアップグレードできます。
{: shortdesc}

始めに、[CLI のターゲット](cs_cli_install.html#cs_cli_configure)をクラスターに設定してください。

1. クラスターにインストールしたブロック・ストレージ Helm チャートの名前を調べます。
   ```
   helm ls | grep ibmcloud-block-storage-plugin
   ```
   {: pre}

   出力例:
   ```
   myhelmchart 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-block-storage-plugin-0.1.0	default
   ```
   {: screen}

2. {{site.data.keyword.Bluemix_notm}} Block Storage プラグインを最新バージョンにアップグレードします。
   ```
   helm upgrade --force --recreate-pods <helm_chart_name>  ibm/ibmcloud-block-storage-plugin
   ```
   {: pre}

<br />


### {{site.data.keyword.Bluemix_notm}} Block Storage プラグインの削除
クラスターで {{site.data.keyword.Bluemix_notm}} Block Storage をプロビジョンして使用する必要がない場合は、Helm チャートをアンインストールできます。
{: shortdesc}

**注:** このプラグインを削除しても、既存の PVC、PV、データは削除されません。 このプラグインを削除すると、関連するすべてのポッドとデーモン・セットがクラスターから削除されます。 このプラグインを削除した後は、新しいブロック・ストレージをクラスターにプロビジョンしたり、既存のブロック・ストレージの PVC と PV を使用したりできません。

まず始めに、[CLI のターゲット](cs_cli_install.html#cs_cli_configure)をクラスターに設定し、ブロック・ストレージを使用する PVC と PV がクラスターにないことを確認してください。

1. クラスターにインストールしたブロック・ストレージ Helm チャートの名前を調べます。
   ```
   helm ls | grep ibmcloud-block-storage-plugin
   ```
   {: pre}

   出力例:
   ```
   myhelmchart 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-block-storage-plugin-0.1.0	default
   ```
   {: screen}

2. {{site.data.keyword.Bluemix_notm}} Block Storage プラグインを削除します。
   ```
   helm delete <helm_chart_name>
   ```
   {: pre}

3. ブロック・ストレージ・ポッドが削除されたことを確認します。
   ```
   kubectl get pod -n kube-system | grep ibmcloud-block-storage-plugin
   ```
   {: pre}
   CLI 出力にポッドが表示されていなければ、ポッドは正常に削除されています。

4. ブロック・ストレージのストレージ・クラスが削除されたことを確認します。
   ```
   kubectl get storageclasses | grep block
   ```
   {: pre}
   CLI 出力にストレージ・クラスが表示されていなければ、ストレージ・クラスは正常に削除されています。

<br />



## NFS ファイル共有およびブロック・ストレージのためのバックアップとリストアのソリューションのセットアップ
{: #backup_restore}

ファイル共有およびブロック・ストレージは、クラスターと同じロケーションにプロビジョンされます。 サーバーがダウンした場合の可用性を確保するために、{{site.data.keyword.IBM_notm}} は、クラスター化したサーバーでストレージをホストしています。 ただし、ファイル共有およびブロック・ストレージは自動的にはバックアップされないため、ロケーション全体で障害が発生した場合はアクセス不能になる可能性があります。 データの損失や損傷を防止するために、定期バックアップをセットアップすると、必要時にバックアップを使用してデータをリストアできます。
{: shortdesc}

NFS ファイル共有およびブロック・ストレージのバックアップとリストアのための以下のオプションを検討してください。

<dl>
  <dt>定期的なスナップショットをセットアップする</dt>
  <dd><p>NFS ファイル共有またはブロック・ストレージの定期的なスナップショットをセットアップできます。スナップショットとは、特定の時点のインスタンスの状態をキャプチャーした読み取り専用のイメージです。 スナップショットを保管するには、NFS ファイル共有またはブロック・ストレージでスナップショット・スペースを要求する必要があります。スナップショットは、同じロケーション内の既存のストレージ・インスタンスに保管されます。ユーザーが誤って重要なデータをボリュームから削除した場合に、スナップショットからデータをリストアできます。</br></br> <strong>ボリュームのスナップショットを作成するには、以下のようにします。</strong><ol><li>クラスター内の既存の PV をリストします。<pre class="pre"><code>kubectl get pv</code></pre></li><li>スナップショット・スペースを作成する PV の詳細を取得し、ボリューム ID、サイズ、および IOPS をメモします。<pre class="pre"><code>kubectl describe pv &lt;pv_name&gt;</code></pre> ファイル・ストレージの場合、ボリューム ID、サイズ、および IOPS は CLI 出力の <strong>Labels</strong> セクションにあります。ブロック・ストレージの場合、サイズと IOPS は CLI 出力の <strong>Labels</strong> セクションに表示されます。ボリューム ID は、CLI 出力の <code>ibm.io/network-storage-id</code> アノテーションで確認します。</li><li>前のステップで取得したパラメーターを使用して、既存のボリュームのスナップショット・サイズを作成します。<pre class="pre"><code>slcli file snapshot-order --capacity &lt;size&gt; --tier &lt;iops&gt; &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-order --capacity &lt;size&gt; --tier &lt;iops&gt; &lt;volume_id&gt;</code></pre></li><li>スナップショット・サイズが作成されるまで待ちます。<pre class="pre"><code>slcli file volume-detail &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block volume-detail &lt;volume_id&gt;</code></pre>CLI 出力の <strong>Snapshot Capacity (GB)</strong> が 0 から注文したサイズに変更されていれば、スナップショット・サイズは正常にプロビジョンされています。</li><li>ボリュームのスナップショットを作成し、作成されたスナップショットの ID をメモします。<pre class="pre"><code>slcli file snapshot-create &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-create &lt;volume_id&gt;</code></pre></li><li>スナップショットが正常に作成されたことを確認します。<pre class="pre"><code>slcli file volume-detail &lt;snapshot_id&gt;</code></pre><pre class="pre"><code>slcli block volume-detail &lt;snapshot_id&gt;</code></pre></li></ol></br><strong>スナップショットから既存のボリュームにデータをリストアするには、以下のようにします。</strong><pre class="pre"><code>slcli file snapshot-restore -s &lt;snapshot_id&gt; &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-restore -s &lt;snapshot_id&gt; &lt;volume_id&gt;</code></pre></br>詳しくは、以下を参照してください。<ul><li>[NFS の定期的なスナップショット](/docs/infrastructure/FileStorage/snapshots.html)</li><li>[ブロックの定期的なスナップショット](/docs/infrastructure/BlockStorage/snapshots.html#snapshots)</li></ul></p></dd>
  <dt>スナップショットを別のロケーションにレプリケーションする</dt>
 <dd><p>ロケーションの障害からデータを保護するために、別のロケーションにセットアップした NFS ファイル共有またはブロック・ストレージのインスタンスに[スナップショットをレプリケーション](/docs/infrastructure/FileStorage/replication.html#working-with-replication)することができます。 データは、1 次ストレージからバックアップ・ストレージにのみレプリケーションできます。 レプリケーションされた NFS ファイル共有またはブロック・ストレージのインスタンスを、クラスターにマウントすることはできません。 1 次ストレージに障害が発生した場合には、レプリケーションされたバックアップ・ストレージを 1 次ストレージに手動で設定できます。 すると、そのファイル共有をクラスターにマウントできます。 1 次ストレージがリストアされたら、バックアップ・ストレージからデータをリストアできます。</p>
 <p>詳しくは、以下を参照してください。<ul><li>[NFS のスナップショットのレプリケーション](/docs/infrastructure/FileStorage/replication.html#working-with-replication)</li><li>[ブロックのスナップショットのレプリケーション](/docs/infrastructure/BlockStorage/replication.html#working-with-replication)</li></ul></p></dd>
 <dt>ストレージを複製する</dt>
 <dd><p>NFS ファイル共有またはブロック・ストレージのインスタンスを、元のストレージ・インスタンスと同じロケーションに複製できます。 複製インスタンスのデータは、それを作成した時点の元のストレージ・インスタンスと同じです。 レプリカとは異なり、複製インスタンスは、元のインスタンスから独立したストレージ・インスタンスとして使用します。 複製するには、まず、ボリュームのスナップショットをセットアップします。</p>
 <p>詳しくは、以下を参照してください。<ul><li>[NFS の複製スナップショット](/docs/infrastructure/FileStorage/how-to-create-duplicate-volume.html#creating-a-duplicate-file-storage)</li><li>[ブロックの複製スナップショット](/docs/infrastructure/BlockStorage/how-to-create-duplicate-volume.html#creating-a-duplicate-block-volume)</li></ul></p></dd>
  <dt>Object Storage にデータをバックアップする</dt>
  <dd><p>[**ibm-backup-restore image**](/docs/services/RegistryImages/ibm-backup-restore/index.html#ibmbackup_restore_starter) を使用して、クラスター内にバックアップとリストアのポッドをスピンアップできます。 このポッドには、クラスター内の任意の永続ボリューム請求 (PVC) のために 1 回限りのバックアップまたは定期バックアップを実行するスクリプトが含まれています。 データは、ロケーションにセットアップした {{site.data.keyword.objectstoragefull}} インスタンスに保管されます。</p>
  <p>データを可用性をさらに高め、アプリをロケーション障害から保護するには、2 つ目の {{site.data.keyword.objectstoragefull}} インスタンスをセットアップして、ロケーション間でデータを複製します。 {{site.data.keyword.objectstoragefull}} インスタンスからデータをリストアする必要がある場合は、イメージに付属するリストア・スクリプトを使用します。</p></dd>
<dt>ポッドおよびコンテナーとの間でデータをコピーする</dt>
<dd><p>`kubectl cp` [コマンド ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/reference/kubectl/overview/#cp) を使用して、クラスター内のポッドまたは特定のコンテナーとの間でファイルとディレクトリーをコピーできます。</p>
<p>まず始めに、[Kubernetes CLI のターゲット](cs_cli_install.html#cs_cli_configure)を、使用するクラスターに設定してください。 <code>-c</code> を使用してコンテナーを指定しない場合、コマンドはポッド内で最初に使用可能なコンテナーを使用します。</p>
<p>このコマンドは、以下のようにさまざまな方法で使用できます。</p>
<ul>
<li>ローカル・マシンからクラスター内のポッドにデータをコピーする: <pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var></code></pre></li>
<li>クラスター内のポッドからローカル・マシンにデータをコピーする: <pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;/&lt;filename&gt;</var> <var>&lt;local_filepath&gt;/&lt;filename&gt;</var></code></pre></li>
<li>クラスター内のあるポッドから別のポッドの特定のコンテナーにデータをコピーする: <pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var> <var>&lt;namespace&gt;/&lt;other_pod&gt;:&lt;pod_filepath&gt;</var> -c <var>&lt;container></var></code></pre></li>
</ul></dd>
  </dl>


