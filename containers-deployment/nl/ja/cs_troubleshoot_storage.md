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



# クラスターのストレージのトラブルシューティング
{: #cs_troubleshoot_storage}

{{site.data.keyword.containerlong}} を使用する際は、ここに示すクラスターのストレージのトラブルシューティング手法を検討してください。
{: shortdesc}

より一般的な問題が起きている場合は、[クラスターのデバッグ](cs_troubleshoot.html)を試してください。
{: tip}



## ワーカー・ノードのファイル・システムが読み取り専用に変更される
{: #readonly_nodes}

{: tsSymptoms}
{: #stuck_creating_state}
以下のいずれかの症状が見られることがあります。
- `kubectl get pods -o wide` を実行すると、同じワーカー・ノードで稼働している複数のポッドが `ContainerCreating` 状態で停滞していることが判明します。
- `kubectl describe` コマンドを実行すると、**Events** セクションに `MountVolume.SetUp failed for volume ... read-only file system` というエラーが表示されます。

{: tsCauses}
ワーカー・ノード上のファイル・システムは読み取り専用です。

{: tsResolve}
1.  ワーカー・ノード上またはコンテナー内に保管されている可能性のあるデータをバックアップします。
2.  既存のワーカー・ノードに対する短期的な解決策としては、ワーカー・ノードを再ロードします。
    <pre class="pre"><code>bx cs worker-reload &lt;cluster_name&gt; &lt;worker_ID&gt;</code></pre>

長期的な解決策としては、[別のワーカー・ノードを追加して、マシン・タイプを更新します](cs_cluster_update.html#machine_type)。

<br />



## 非 root ユーザーが NFS ファイル・ストレージのマウント・パスを所有しているとアプリが失敗する
{: #nonroot}

{: tsSymptoms}
デプロイメントに [NFS ストレージを追加](cs_storage.html#app_volume_mount)した後に、コンテナーのデプロイメントが失敗します。 コンテナーのログを取得すると、「書き込み権限」や「必要な権限がない」などのエラーが表示されることがあります。 ポッドが失敗し、再ロードが繰り返されます。

{: tsCauses}
デフォルトでは、非 root ユーザーには、NFS ベースのストレージのボリューム・マウント・パスに対する書き込み権限がありません。 一部の一般的なアプリのイメージ (Jenkins や Nexus3 など) は、マウント・パスを所有する非 root ユーザーを Dockerfile に指定しています。 この Dockerfile からコンテナーを作成すると、マウント・パスに対する非 root ユーザーの権限が不十分なために、コンテナーの作成は失敗します。 書き込み権限を付与するには、Dockerfile を変更して非 root ユーザーを root ユーザー・グループに一時的に追加してから、マウント・パスの権限を変更するか、または init コンテナーを使用します。

Helm チャートを使用してイメージをデプロイする場合は、init コンテナーを使用するように Helm デプロイメントを編集します。
{:tip}



{: tsResolve}
デプロイメントに [init コンテナー ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) を指定すると、Dockerfile に指定された非 root ユーザーに、コンテナー内のボリューム・マウント・パスに対する書き込み権限を付与できます。init コンテナーは、アプリ・コンテナーが開始される前に開始されます。 init コンテナーは、コンテナーの内部にボリューム・マウント・パスを作成し、そのマウント・パスを正しい (非 root) ユーザーが所有するように変更してから、クローズします。 その後、マウント・パスに書き込む必要がある非 root ユーザーでアプリ・コンテナーが開始されます。パスは既に非 root ユーザーによって所有されているため、マウント・パスへの書き込みは成功します。 init コンテナーを使用したくない場合は、Dockerfile を変更して、非 root ユーザーに NFS ファイル・ストレージへのアクセス権限を追加できます。


始めに、[CLI のターゲット](cs_cli_install.html#cs_cli_configure)を自分のクラスターに設定してください。

1.  アプリの Dockerfile を開き、ボリューム・マウント・パスに対する書き込み権限を付与するユーザーのユーザー ID (UID) とグループ ID (GID) を取得します。 Jenkins Dockerfile の例では、情報は以下のとおりです。
    - UID: `1000`
    - GID: `1000`

    **例**:

    ```
    FROM openjdk:8-jdk

    RUN apt-get update && apt-get install -y git curl && rm -rf /var/lib/apt/lists/*

    ARG user=jenkins
    ARG group=jenkins
    ARG uid=1000
    ARG gid=1000
    ARG http_port=8080
    ARG agent_port=50000

    ENV JENKINS_HOME /var/jenkins_home
    ENV JENKINS_SLAVE_AGENT_PORT ${agent_port}
    ...
    ```
    {:screen}

2.  永続ボリューム請求 (PVC) を作成して、アプリに永続ストレージを追加します。 この例では、`ibmc-file-bronze` ストレージ・クラスを使用します。 使用可能なストレージ・クラスを確認するには、`kubectl get storageclasses` を実行します。

    ```
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: mypvc
      annotations:
        volume.beta.kubernetes.io/storage-class: "ibmc-file-bronze"
    spec:
      accessModes:
        - ReadWriteMany
      resources:
        requests:
          storage: 20Gi
    ```
    {: codeblock}

3.  PVC を作成します。

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

4.  デプロイメント `.yaml` ファイルに、init コンテナーを追加します。 先ほど取得した UID と GID を指定します。

    ```
    initContainers:
    - name: initContainer # Or you can replace with any name
      image: alpine:latest
      command: ["/bin/sh", "-c"]
      args:
        - chown <UID>:<GID> /mount; # Replace UID and GID with values from the Dockerfile
      volumeMounts:
      - name: volume # Or you can replace with any name
        mountPath: /mount # Must match the mount path in the args line
    ```
    {: codeblock}

    Jenkins デプロイメントの**例**:

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: my_pod
    spec:
      replicas: 1
      template:
        metadata:
          labels:
            app: jenkins
        spec:
          containers:
          - name: jenkins
            image: jenkins
            volumeMounts:
            - mountPath: /var/jenkins_home
              name: volume
          volumes:
          - name: volume
            persistentVolumeClaim:
              claimName: mypvc
          initContainers:
          - name: permissionsfix
            image: alpine:latest
            command: ["/bin/sh", "-c"]
            args:
              - chown 1000:1000 /mount;
            volumeMounts:
            - name: volume
              mountPath: /mount
    ```
    {: codeblock}

5.  ポッドを作成して、PVC をポッドにマウントします。

    ```
    kubectl apply -f my_pod.yaml
    ```
    {: pre}

6. ボリュームがポッドに正常にマウントされたことを確認します。 ポッド名と **Containers/Mounts** パスをメモします。

    ```
    kubectl describe pod <my_pod>
    ```
    {: pre}

    **出力例**:

    ```
    Name:       mypod-123456789
    Namespace:	default
    ...
    Init Containers:
    ...
    Mounts:
      /mount from volume (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-cp9f0 (ro)
    ...
    Containers:
      jenkins:
        Container ID:
        Image:		jenkins
        Image ID:
        Port:		  <none>
        State:		Waiting
          Reason:		PodInitializing
        Ready:		False
        Restart Count:	0
        Environment:	<none>
        Mounts:
          /var/jenkins_home from volume (rw)
          /var/run/secrets/kubernetes.io/serviceaccount from default-token-cp9f0 (ro)
    ...
    Volumes:
      myvol:
        Type:	PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
        ClaimName:	mypvc
        ReadOnly:	  false

    ```
    {: screen}

7.  前にメモしたポッド名を使用して、ポッドにログインします。

    ```
    kubectl exec -it <my_pod-123456789> /bin/bash
    ```
    {: pre}

8. コンテナーのマウント・パスの権限を確認します。 この例では、マウント・パスは `/var/jenkins_home` です。

    ```
    ls -ln /var/jenkins_home
    ```
    {: pre}

    **出力例**:

    ```
    jenkins@mypod-123456789:/$ ls -ln /var/jenkins_home
    total 12
    -rw-r--r-- 1 1000 1000  102 Mar  9 19:58 copy_reference_file.log
    drwxr-xr-x 2 1000 1000 4096 Mar  9 19:58 init.groovy.d
    drwxr-xr-x 9 1000 1000 4096 Mar  9 20:16 war
    ```
    {: screen}

    この出力は、Dockerfile の GID と UID (この例では `1000` と `1000`) がコンテナー内部のマウント・パスを所有していることを示しています。

<br />


## 永続ストレージに対する非 root ユーザー・アクセスの追加が失敗する
{: #cs_storage_nonroot}

{: tsSymptoms}
[永続ストレージに対する非 root ユーザーのアクセスを追加した](#nonroot)後、または、非 root ユーザーの ID を指定して Helm チャートをデプロイした後に、そのユーザーがマウントされたストレージに書き込めません。

{: tsCauses}
デプロイメント構成または Helm チャートの構成で、ポッドの `fsGroup` (グループ ID) と `runAsUser` (ユーザー ID) に対して[セキュリティー・コンテキスト](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)が指定されています。 現在、{{site.data.keyword.containershort_notm}} では `fsGroup` の指定をサポートしていません。`0` (ルート権限) に設定された `runAsUser` だけをサポートしています。

{: tsResolve}
イメージ、デプロイメント、または Helm チャートの構成ファイルから構成の `fsGroup` と `runAsUser` の `securityContext` フィールドを削除してから、再デプロイします。 マウント・パスの所有権を `nobody` から変更する必要がある場合は、[非 root ユーザー・アクセスを追加します](#nonroot)。 [非 root の initContainer](#nonroot) を追加した後、ポッド・レベルではなくコンテナー・レベルで `runAsUser` を設定します。

<br />




## ファイル・システムが正しくないため、既存のブロック・ストレージをポッドにマウントできない
{: #block_filesystem}

{: tsSymptoms}
`kubectl describe pod<pod_name>` を実行すると、以下のエラーが表示されます。
```
failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32
```
{: screen}

{: tsCauses}
`XFS` ファイル・システムでセットアップされている、既存のブロック・ストレージ・デバイスがあります。このデバイスをポッドにマウントするために、`spec/flexVolume/fsType` セクションでファイル・システムとして `ext4` を指定しているか、またはファイル・システムを指定していない [PV を作成](cs_storage.html#existing_block)しました。ファイル・システムが定義されていない場合、PV はデフォルトで `ext4` に設定されます。
PV は正常に作成され、既存のブロック・ストレージ・インスタンスにリンクされました。ところが、対応する PVC を使用してクラスターに PV をマウントしようとすると、ボリュームのマウントが失敗します。`ext4` ファイル・システムが指定された `XFS` ブロック・ストレージ・インスタンスをポッドにマウントすることができません。

{: tsResolve}
既存の PV のファイル・システムを `ext4` から `XFS` に更新します。

1. クラスター内の既存の PV をリストし、既存のブロック・ストレージ・インスタンスに使用している PV の名前をメモします。
   ```
   kubectl get pv
   ```
   {: pre}

2. ローカル・マシンに PV yaml を保存します。
   ```
   kubectl get pv <pv_name> -o yaml > <filepath/xfs_pv.yaml>
   ```
   {: pre}

3. yaml ファイルを開き、`fsType` を `ext4` から `xfs` に変更します。
4. クラスター内の PV を置き換えます。
   ```
   kubectl replace --force -f <filepath/xfs_pv.yaml>
   ```
   {: pre}

5. PV をマウントしたポッドにログインします。
   ```
   kubectl exec -it <pod_name> sh
   ```
   {: pre}

6. ファイル・システムが `XFS` に変更されたことを確認します。
   ```
   df -Th
   ```
   {: pre}

   出力例:
   ```
   Filesystem Type Size Used Avail Use% Mounted on /dev/mapper/3600a098031234546d5d4c9876654e35 xfs 20G 33M 20G 1% /myvolumepath
   ```
   {: screen}

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

