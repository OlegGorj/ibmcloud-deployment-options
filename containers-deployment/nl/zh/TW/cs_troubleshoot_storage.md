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



# 叢集儲存空間的疑難排解
{: #cs_troubleshoot_storage}

在您使用 {{site.data.keyword.containerlong}} 時，請考慮使用這些技術來進行叢集儲存空間的疑難排解。
{: shortdesc}

如果您有更一般性的問題，請嘗試[叢集除錯](cs_troubleshoot.html)。
{: tip}



## 工作者節點的檔案系統變更為唯讀
{: #readonly_nodes}

{: tsSymptoms}
{: #stuck_creating_state}
您可能會看到下列其中一個症狀：
- 當您執行 `kubectl get pods -o wide` 時，您會看到在同一個工作者節點上執行的多個位置停留在 `ContainerCreating` 狀態中。
- 當您執行 `kubectl describe` 指令時，會在 **Events** 區段中看到下列錯誤：`MountVolume.SetUp failed for volume ... read-only file system`。

{: tsCauses}
工作者節點上的檔案系統是唯讀的。

{: tsResolve}
1.  備份可能儲存在工作者節點或容器上的任何資料。
2.  若要對現有工作者節點進行短期修正，請重新載入工作者節點。<pre class="pre"><code>bx cs worker-reload &lt;cluster_name&gt; &lt;worker_ID&gt;</code></pre>

如需長期修正，請[藉由新增另一個工作者節點來更新機型](cs_cluster_update.html#machine_type)。

<br />



## 當非 root 使用者擁有 NFS 檔案儲存空間裝載路徑時，應用程式會失敗
{: #nonroot}

{: tsSymptoms}
[新增 NFS 儲存空間](cs_storage.html#app_volume_mount)至您的部署之後，您的容器部署失敗。擷取容器的日誌時，您可能會看到諸如「撰寫許可權」或「沒有必要許可權」等錯誤。Pod 失敗，並停留在重新載入的循環中。

{: tsCauses}
依預設，非 root 使用者對於支援 NFS 的儲存空間的磁區裝載路徑沒有寫入權。部分一般應用程式映像檔（例如 Jenkins 及 Nexus3）會在 Dockerfile 中指定擁有裝載路徑的非 root 使用者。當您從這個 Dockerfile 建立容器時，會由於非 root 使用者對裝載路徑的權限不足，而造成建立容器失敗。若要授與寫入權，您可以修改 Dockerfile，以在變更裝載路徑許可權之前，暫時將非 root 使用者新增至 root 使用者群組，或使用 init 容器。

如果您使用 Helm 圖表來部署映像檔，則請編輯 Helm 部署以使用 init 容器。
{:tip}



{: tsResolve}
當您在部署中包括 [init 容器 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) 時，可以授與 Dockerfile 中所指定的非 root 使用者對於容器內的磁區裝載路徑的寫入權。init 容器會在您的應用程式容器啟動之前啟動。init 容器會在容器內建立磁區裝載路徑，將裝載路徑變更為由正確的（非 root）使用者所擁有，然後關閉。然後，會使用必須寫入至裝載路徑的非 root 使用者來啟動您的應用程式容器。因為路徑已由非 root 使用者擁有，所以會成功寫入至裝載路徑。如果您不想要使用 init 容器，則可以修改 Dockerfile，以新增非 root 使用者對 NFS 檔案儲存空間的存取權。


開始之前，請先將 [CLI 的目標](cs_cli_install.html#cs_cli_configure)設為您的叢集。

1.  開啟應用程式的 Dockerfile，然後取得您要授與磁區裝載路徑寫入權的使用者的使用者 ID (UID) 及群組 ID (GID)。在 Jenkins Dockerfile 的範例中，其資訊如下：
    - UID：`1000`
    - GID：`1000`

    **範例**：

    ```
        FROM openjdk:8-jdk

    

    RUN apt-get update &&apt-get install -y git curl &&rm -rf /var/lib/apt/lists/*

    

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

2.  藉由建立持續性磁區宣告 (PVC)，將持續性儲存空間新增至應用程式。這個範例使用 `ibmc-file-bronze` 儲存空間類別。若要檢閱可用的儲存空間類別，請執行 `kubectl get storageclasses`。

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

3.  建立 PVC。

    ```
        kubectl apply -f mypvc.yaml
    ```
    {: pre}

4.  在您的部署 `.yaml` 檔案中，新增 init 容器。包括您先前擷取的 UID 及 GID。

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

    Jenkins 部署的**範例**：

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

5.  建立 Pod，並將 PVC 裝載至 Pod。

    ```
        kubectl apply -f my_pod.yaml
    ```
    {: pre}

6. 驗證磁區已順利裝載至 Pod。請記下 Pod 名稱及 **Containers/Mounts** 路徑。

    ```
        kubectl describe pod <my_pod>
    ```
    {: pre}

    **範例輸出**：

    ```
        Name:		    mypod-123456789
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
        Type: PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
        ClaimName: mypvc
        ReadOnly: false

    

    ```
    {: screen}

7.  使用您先前記下的 Pod 名稱來登入 Pod。

    ```
        kubectl exec -it <my_pod-123456789> /bin/bash
    ```
    {: pre}

8. 驗證容器裝載路徑的許可權。在此範例中，裝載路徑為 `/var/jenkins_home`。

    ```
        ls -ln /var/jenkins_home
    ```
    {: pre}

    **範例輸出**：

    ```
        jenkins@mypod-123456789:/$ ls -ln /var/jenkins_home
    total 12
    -rw-r--r-- 1 1000 1000  102 Mar  9 19:58 copy_reference_file.log
    drwxr-xr-x 2 1000 1000 4096 Mar  9 19:58 init.groovy.d
    drwxr-xr-x 9 1000 1000 4096 Mar  9 20:16 war
    ```
    {: screen}

    此輸出顯示 Dockerfile 中的 GID 及 UID（在此範例中是 `1000` 及 `1000`）擁有容器內的裝載路徑。

<br />


## 新增非 root 使用者對持續性儲存空間的存取權失敗
{: #cs_storage_nonroot}

{: tsSymptoms}
在您[新增非 root 使用者對持續性儲存空間的存取權](#nonroot)或在指定非 root 使用者 ID 的情況下部署 Helm 圖表之後，使用者無法寫入已裝載的儲存空間。

{: tsCauses}
部署或 Helm 圖表配置為 Pod 的 `fsGroup`（群組 ID）及 `runAsUser`（使用者 ID）指定[安全環境定義](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)。目前，{{site.data.keyword.containershort_notm}} 不支援 `fsGroup` 規格，僅支援 `runAsUser` 設為 `0`（root 許可權）。

{: tsResolve}
請從映像檔、部署或 Helm 圖表配置檔中，移除 `fsGroup` 及 `runAsUser` 的配置的 `securityContext` 欄位，然後重新部署。如果您需要變更 `nobody` 的裝載路徑的所有權，請[新增非 root 使用者存取權](#nonroot)。在您新增[非 root initContainer](#nonroot) 之後，請在容器層次設定 `runAsUser`，而不是 Pod 層次。

<br />




## 將現有區塊儲存空間裝載至 Pod 失敗，因為檔案系統錯誤
{: #block_filesystem}

{: tsSymptoms}
當您執行 `kubectl describe pod <pod_name>` 時，會看到下列錯誤：
```
failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32
```
{: screen}

{: tsCauses}
您的現有區塊儲存裝置已設定 `XFS` 檔案系統。若要將此裝置裝載至您的 Pod，您已[建立 PV](cs_storage.html#existing_block)，其將 `ext4` 指定為檔案系統或 `spec/flexVolume/fsType` 區段中未指定檔案系統。如果未定義任何檔案系統，則 PV 預設為 `ext4`。
PV 已順利建立並鏈結至現有區塊儲存空間實例。不過，當您嘗試使用相符的 PVC 將 PV 裝載到叢集時，會無法裝載磁區。您無法將具有 `ext4` 檔案系統的 `XFS` 區塊儲存空間實例裝載到 Pod。

{: tsResolve}
將現有 PV 中的檔案系統從 `ext4` 更新至 `XFS`。

1. 列出叢集中的現有 PV，並記下用於現有區塊儲存空間實例的 PV 名稱。
   ```
    kubectl get pv
    ```
   {: pre}

2. 將 PV yaml 儲存在您的本端機器上。
   ```
   kubectl get pv <pv_name> -o yaml > <filepath/xfs_pv.yaml>
   ```
   {: pre}

3. 開啟 yaml 檔案，並將 `fsType` 從 `ext4` 變更為 `xfs`。
4. 取代叢集中的 PV。
   ```
   kubectl replace --force -f <filepath/xfs_pv.yaml>
   ```
   {: pre}

5. 登入已裝載 PV 的 Pod。
   ```
   kubectl exec -it <pod_name> sh
   ```
   {: pre}

6. 驗證檔案系統已變更為 `XFS`。
   ```
   df -Th
   ```
   {: pre}

   輸出範例：
   ```
   Filesystem Type Size Used Avail Use% Mounted on /dev/mapper/3600a098031234546d5d4c9876654e35 xfs 20G 33M 20G 1% /myvolumepath
   ```
   {: screen}

<br />



## 取得協助及支援
{: #ts_getting_help}

叢集仍有問題？
{: shortdesc}

-   若要查看 {{site.data.keyword.Bluemix_notm}} 是否可用，請[檢查 {{site.data.keyword.Bluemix_notm}} 狀態頁面 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/bluemix/support/#status)。
-   將問題張貼到 [{{site.data.keyword.containershort_notm}} Slack ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://ibm-container-service.slack.com)。

如果您的 {{site.data.keyword.Bluemix_notm}} 帳戶未使用 IBM ID，請[要求邀請](https://bxcs-slack-invite.mybluemix.net/)以加入此 Slack。
    {: tip}
-   檢閱討論區，以查看其他使用者是否發生過相同的問題。使用討論區提問時，請標記您的問題，以便 {{site.data.keyword.Bluemix_notm}} 開發團隊能看到它。

    -   如果您在使用 {{site.data.keyword.containershort_notm}} 開發或部署叢集或應用程式時有技術方面的問題，請將問題張貼到 [Stack Overflow ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers)，並使用 `ibm-cloud`、`kubernetes` 及 `containers` 來標記問題。
    -   若為服務及開始使用指示的相關問題，請使用 [IBM developerWorks dW Answers ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) 討論區。請包含 `ibm-cloud` 及 `containers` 標籤。如需使用討論區的詳細資料，請參閱[取得協助](/docs/get-support/howtogetsupport.html#using-avatar)。

-   開立問題單以與 IBM 支援中心聯絡。若要瞭解開立 IBM 支援問題單或是支援層次與問題單嚴重性，請參閱[與支援中心聯絡](/docs/get-support/howtogetsupport.html#getting-customer-support)。

{: tip}
當您報告問題時，請包含您的叢集 ID。若要取得叢集 ID，請執行 `bx cs clusters`。

