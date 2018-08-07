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



# 集群存储故障诊断
{: #cs_troubleshoot_storage}

在使用 {{site.data.keyword.containerlong}} 时，请考虑对集群存储进行故障诊断的以下方法。
{: shortdesc}

如果您有更常规的问题，请尝试[集群调试](cs_troubleshoot.html)。
{: tip}



## 工作程序节点的文件系统更改为只读
{: #readonly_nodes}

{: tsSymptoms}
{: #stuck_creating_state}
您可能会看到下列其中一个症状：
- 当您运行 `kubectl get pods -o wide` 时，您会看到在同一工作程序节点上运行的多个 pod 陷入 `ContainerCreating` 状态。
- 运行 `kubectl describe` 命令时，在 **Events** 部分中看到以下错误：`MountVolume.SetUp failed for volume ... read-only file system`。

{: tsCauses}
工作程序节点上的文件系统是只读的。

{: tsResolve}
1.  备份可能存储在工作程序节点或容器中的任何数据。
2.  要对现有工作程序节点进行短时间修订，请重新装入工作程序节点。<pre class="pre"><code>bx cs worker-reload &lt;cluster_name&gt; &lt;worker_ID&gt;</code></pre>

对于长时间修订，请[通过添加其他工作程序节点来更新机器类型](cs_cluster_update.html#machine_type)。

<br />



## 非 root 用户拥有 NFS 文件存储器安装路径时，应用程序发生故障
{: #nonroot}

{: tsSymptoms}
向部署[添加 NFS 存储器](cs_storage.html#app_volume_mount)后，容器的部署失败。检索容器的日志时，您可能会看到“write-permission”或“do not have required permission”之类的错误。pod 发生故障，并且卡在重新装入循环中。

{: tsCauses}
缺省情况下，非 root 用户在支持 NFS 的存储器的卷安装路径上没有写许可权。一些公共应用程序映像（例如，Jenkins 和 Nexus3）会在 Dockerfile 中指定拥有安装路径的非 root 用户。通过此 Dockerfile 创建容器时，由于安装路径上非 root 用户的许可权不足，创建容器会失败。要授予写许可权，可以修改 Dockerfile，以在更改安装路径许可权之前将非 root 用户临时添加到 root 用户组，或者使用 init 容器。

如果使用 Helm 图表来部署映像，请编辑 Helm 部署以使用 init 容器。
{:tip}



{: tsResolve}
在部署中包含 [init 容器 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) 时，可以向 Dockerfile 中指定的非 root 用户授予对容器内卷安装路径的写许可权。init 容器会在应用程序容器启动之前启动。init 容器在容器内创建卷安装路径，将安装路径更改为由正确的（非 root）用户拥有，然后关闭。随后，应用程序容器将以必须写入安装路径的非 root 用户身份启动。由于该路径已由非 root 用户拥有，因此写入安装路径成功。如果您不想使用 init 容器，那么可以修改 Dockerfile 以添加对 NFS 文件存储器的非 root 用户访问权。


开始之前，请[设定 CLI 的目标](cs_cli_install.html#cs_cli_configure)为集群。

1.  打开应用程序的 Dockerfile，并从要向其授予卷安装路径的作者许可权的用户获取用户标识 (UID) 和组标识 (GID)。在来自 Jenkins Dockerfile 的示例中，信息为：
    - UID：`1000`
    - GID：`1000`

    **示例**：

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

2.  通过创建持久性卷申领 (PVC) 将持久性存储器添加到应用程序。此示例使用 `ibmc-file-bronze` 存储类。要查看可用的存储类，请运行 `kubectl get storageclasses`。

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

3.  创建 PVC。

    ```
        kubectl apply -f mypvc.yaml
    ```
    {: pre}

4.  在部署 `.yaml` 文件中，添加 init 容器。包含您先前检索到的 UID 和 GID。

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

    Jenkins 部署的**示例**：

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

5.  创建 pod 并将 PVC 安装到该 pod。

    ```
        kubectl apply -f my_pod.yaml
    ```
    {: pre}

6. 验证卷是否已成功安装到 pod。记下 pod 名称和 **Containers/Mounts** 路径。

    ```
        kubectl describe pod <my_pod>
    ```
    {: pre}

    **示例输出**：

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

7.  使用先前记下的 pod 名称登录到 pod。

    ```
        kubectl exec -it <my_pod-123456789> /bin/bash
    ```
    {: pre}

8. 验证容器安装路径的许可权。在示例中，安装路径为 `/var/jenkins_home`。

    ```
        ls -ln /var/jenkins_home
    ```
    {: pre}

    **示例输出**：

    ```
        jenkins@mypod-123456789:/$ ls -ln /var/jenkins_home
    total 12
    -rw-r--r-- 1 1000 1000  102 Mar  9 19:58 copy_reference_file.log
    drwxr-xr-x 2 1000 1000 4096 Mar  9 19:58 init.groovy.d
    drwxr-xr-x 9 1000 1000 4096 Mar  9 20:16 war
    ```
    {: screen}

    此输出显示 Dockerfile 中的 GID 和 UID（在此示例中为 `1000` 和 `1000`）拥有容器内的安装路径。

<br />


## 添加持久性存储器的非 root 用户访问权失败
{: #cs_storage_nonroot}

{: tsSymptoms}
在[添加持久性存储器的非 root 用户访问权](#nonroot)或使用指定的非 root 用户标识部署 Helm 图表后，用户无法写入安装的存储器。

{: tsCauses}
部署或 Helm 图表配置为 pod 的 `fsGroup`（组标识）和 `runAsUser`（用户标识）指定了[安全上下文](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)。目前，{{site.data.keyword.containershort_notm}} 不支持 `fsGroup` 规范，而仅支持将 `runAsUser` 设置为 `0`（root 用户许可权）。

{: tsResolve}
从映像、部署或 Helm 图表配置文件中除去配置的 `fsGroup` 和 `runAsUser` 的 `securityContext` 字段，然后重新部署。如果需要将安装路径的所有权从 `nobody` 更改为其他值，请[添加非 root 用户访问权](#nonroot)。添加[非 root 用户 initContainer](#nonroot) 后，请在容器级别（而非 pod 级别）设置 `runAsUser`。

<br />




## 由于文件系统不正确，将现有块存储器安装到 pod 失败
{: #block_filesystem}

{: tsSymptoms}
运行 `kubectl describe pod <pod_name>` 时，会看到以下错误：
```
failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32
```
{: screen}

{: tsCauses}
您具有设置为使用 `XFS` 文件系统的现有块存储设备。为了将此设备安装到 pod，您已[创建 PV](cs_storage.html#existing_block)，其中在 `spec/flexVolume/fsType` 部分中指定 `ext4` 作为文件系统，或者未指定任何文件系统。如果未定义任何文件系统，PV 将缺省为 `ext4`。PV 已成功创建，并且已链接到现有块存储器实例。但是，尝试使用匹配的 PVC 将 PV 安装到集群时，卷安装失败。无法将使用 `ext4` 文件系统的 `XFS` 块存储器实例安装到 pod。

{: tsResolve}
将现有 PV 中的文件系统从 `ext4` 更新为 `XFS`。

1. 列出集群中的现有 PV，并记下用于现有块存储器实例的 PV 的名称。
   ```
    kubectl get pv
    ```
   {: pre}

2. 将 PV YAML 保存到本地计算机。
   ```
   kubectl get pv <pv_name> -o yaml > <filepath/xfs_pv.yaml>
   ```
   {: pre}

3. 打开该 YAML 文件，并将 `fsType` 从 `ext4` 更改为 `xfs`。
4. 替换集群中的 PV。
   ```
   kubectl replace --force -f <filepath/xfs_pv.yaml>
   ```
   {: pre}

5. 登录到安装了 PV 的 pod。
   ```
   kubectl exec -it <pod_name> sh
   ```
   {: pre}

6. 验证文件系统是否已更改为 `XFS`。
   ```
   df -Th
   ```
   {: pre}

   输出示例：
   ```
   Filesystem Type Size Used Avail Use% Mounted on /dev/mapper/3600a098031234546d5d4c9876654e35 xfs 20G 33M 20G 1% /myvolumepath
   ```
   {: screen}

<br />



## 获取帮助和支持
{: #ts_getting_help}

集群仍然有问题吗？
{: shortdesc}

-   要查看 {{site.data.keyword.Bluemix_notm}} 是否可用，请[检查 {{site.data.keyword.Bluemix_notm}} 状态页面 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/bluemix/support/#status)。
-   在 [{{site.data.keyword.containershort_notm}} Slack ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://ibm-container-service.slack.com) 中发布问题。

如果未将 IBM 标识用于 {{site.data.keyword.Bluemix_notm}} 帐户，请针对此 Slack [请求邀请](https://bxcs-slack-invite.mybluemix.net/)。
    {: tip}
-   请复查论坛，以查看是否有其他用户遇到相同的问题。使用论坛进行提问时，请使用适当的标记来标注您的问题，以方便 {{site.data.keyword.Bluemix_notm}} 开发团队识别。

    -   如果您有关于使用 {{site.data.keyword.containershort_notm}} 开发或部署集群或应用程序的技术问题，请在 [Stack Overflow ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) 上发布您的问题，并使用 `ibm-cloud`、`kubernetes` 和 `containers` 标记您的问题。
    -   有关服务的问题和入门指示信息，请使用 [IBM developerWorks dW Answers ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) 论坛。请加上 `ibm-cloud` 和 `containers` 标记。
    有关使用论坛的更多详细信息，请参阅[获取帮助](/docs/get-support/howtogetsupport.html#using-avatar)。

-   通过开具凭单，与 IBM 支持联系。要了解有关开具 IBM 支持凭单或有关支持级别和凭单严重性的信息，请参阅[联系支持人员](/docs/get-support/howtogetsupport.html#getting-customer-support)。

{: tip}
报告问题时，请包含集群标识。要获取集群标识，请运行 `bx cs clusters`。

