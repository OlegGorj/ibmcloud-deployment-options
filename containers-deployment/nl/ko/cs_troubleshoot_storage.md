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



# 클러스터 스토리지 문제점 해결
{: #cs_troubleshoot_storage}

{{site.data.keyword.containerlong}}를 사용할 때 클러스터 스토리지 관련 문제점을 해결하려면 이러한 방법을 고려하십시오.
{: shortdesc}

더 일반적인 문제점이 있는 경우에는 [클러스터 디버깅](cs_troubleshoot.html)을 시도해 보십시오.
{: tip}



## 작업자 노드의 파일 시스템을 읽기 전용으로 변경
{: #readonly_nodes}

{: tsSymptoms}
{: #stuck_creating_state}
다음 증상 중 하나가 표시될 수 있습니다.
- `kubectl get pods -o wide`를 실행할 때 동일한 작업자 노드에서 실행 중인 여러 팟(Pod)이 계속 `ContainerCreating` 상태에 있음을 알게 됩니다.
- `kubectl describe` 명령을 실행할 때 **Events** 섹션에 `MountVolume.SetUp failed for volume ... read-only file system` 오류가 표시됩니다.

{: tsCauses}
작업자 노드의 파일 시스템은 읽기 전용입니다.

{: tsResolve}
1.  작업자 노드 또는 컨테이너에 저장될 수 있는 데이터를 백업하십시오.
2.  기존 작업자 노드에 대한 단기 수정사항의 경우 작업자 노드를 다시 로드하십시오.
    <pre class="pre"><code>bx cs worker-reload &lt;cluster_name&gt; &lt;worker_ID&gt;</code></pre>

장기 수정사항의 경우 [다른 작업자 노드를 추가하여 머신 유형을 업데이트](cs_cluster_update.html#machine_type)하십시오.

<br />



## 루트가 아닌 사용자가 NFS 파일 스토리지 마운트 경로를 소유하는 경우 앱에 장애가 발생함
{: #nonroot}

{: tsSymptoms}
배치에 [NFS 스토리지를 추가](cs_storage.html#app_volume_mount)한 후 컨테이너의 배치가 실패합니다. 컨테이너의 로그를 검색하면 "write-permission" 또는 "do not have required permission"과 같은 오류가 기록되어 있습니다. 팟(Pod)에 장애가 발생하여 다시 로드 순환에서 벗어나지 못합니다.

{: tsCauses}
기본적으로, 루트가 아닌 사용자에게는 NFS 지원 스토리지의 볼륨 마운트 경로에 대한 쓰기 권한이 없습니다. Jenkins 및 Nexus3과 같은 일부 일반적인 앱 이미지에서는 Dockerfile에 마운트 경로를 소유하는 루트가 아닌 사용자를 지정합니다. 이 Dockerfile에서 컨테이너를 작성하는 경우 마운트 경로에 루트가 아닌 사용자에 대한 권한이 충분하지 않기 때문에 컨테이너의 작성에 실패합니다. 쓰기 권한을 부여하기 위해 마운트 경로 권한을 변경하기 전에 Dockerfile 파일을 수정하여 임시로 루트가 아닌 사용자를 루트 사용자 그룹에 추가하거나 init 컨테이너를 사용할 수 있습니다.

Helm 차트를 사용하여 이미지를 배치하는 경우 init 컨테이너를 사용하도록 Helm 배치를 편집하십시오.
{:tip}



{: tsResolve}
배치에 [init 컨테이너 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/)를 포함하는 경우 컨테이너 내부의 볼륨 마운트 경로에 대해 Dockerfile 쓰기 권한에 지정된 루트가 아닌 사용자를 제공할 수 있습니다. 앱 컨테이너가 시작되기 전에 init 컨테이너가 시작됩니다. init 컨테이너는 컨테이너 내부에 볼륨 마운트 경로를 작성하고 올바른(루트가 아닌) 사용자가 소유하도록 마운트 경로를 변경한 후 닫습니다. 그런 다음, 마운트 경로에 쓰기를 수행해야 하는 루트가 아닌 사용자가 포함된 앱 컨테이너가 시작됩니다. 루트가 아닌 사용자가 경로를 이미 소유하고 있기 때문에 마운트 경로 쓰기에 성공합니다. init 컨테이너를 사용하지 않을 경우 Dockerfile을 수정하여 루트가 아닌 사용자 액세스를 NFS 파일 스토리지에 추가할 수 있습니다.


시작하기 전에 클러스터를 [CLI의 대상으로 지정](cs_cli_install.html#cs_cli_configure)하십시오.

1.  앱의 Dockerfile을 열고 볼륨 마운트 경로에 대한 쓰기 권한을 제공하려는 사용자로부터 사용자 ID(UID) 및 그룹 ID(GID)를 가져오십시오. Jenkins Dockerfile의 예에서 정보는 다음과 같습니다.
    - UID: `1000`
    - GID: `1000`

    **예제**:

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

2.  지속적 볼륨 클레임(PVC)을 작성하여 지속적 스토리지를 앱에 추가하십시오. 이 예에서는 `ibmc-file-bronze` 스토리지 클래스를 사용합니다. 사용 가능한 스토리지 클래스를 검토하려면 `kubectl get storageclasses`를 실행하십시오.

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

3.  PVC를 작성하십시오.

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

4.  배치 `.yaml` 파일에서 init 컨테이너를 추가하십시오. 이전에 검색한 UID 및 GID를 포함하십시오.

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

    Jenkins 배치의 **예**:

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

5.  팟(Pod)을 작성하고 PVC를 팟(Pod)에 마운트하십시오.

    ```
    kubectl apply -f my_pod.yaml
    ```
    {: pre}

6. 볼륨이 팟(Pod)에 정상적으로 마운트되었는지 확인하십시오. 팟(Pod) 이름 및 **컨테이너/마운트** 경로를 기록해 두십시오.

    ```
    kubectl describe pod <my_pod>
    ```
    {: pre}

    **출력 예**:

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

7.  이전에 기록해 놓은 팟(Pod) 이름을 사용하여 팟(Pod)에 로그인하십시오.

    ```
    kubectl exec -it <my_pod-123456789> /bin/bash
    ```
    {: pre}

8. 컨테이너의 마운트 경로에 대한 권한을 확인하십시오. 예를 들어, 마운트 경로는 `/var/jenkins_home`입니다.

    ```
    ls -ln /var/jenkins_home
    ```
    {: pre}

    **출력 예**:

    ```
    jenkins@mypod-123456789:/$ ls -ln /var/jenkins_home
    total 12
    -rw-r--r-- 1 1000 1000  102 Mar  9 19:58 copy_reference_file.log
    drwxr-xr-x 2 1000 1000 4096 Mar  9 19:58 init.groovy.d
    drwxr-xr-x 9 1000 1000 4096 Mar  9 20:16 war
    ```
    {: screen}

    이 출력은 Dockerfile의 GID 및 UID(이 예에서 `1000` 및 `1000`)가 컨테이너 내부의 마운트 경로를 소유함을 표시합니다.

<br />


## 지속적 스토리지에 루트가 아닌 사용자 액세스 추가 실패
{: #cs_storage_nonroot}

{: tsSymptoms}
[지속적 스토리지에 루트가 아닌 사용자 액세스를 추가](#nonroot)하거나 지정된 루트가 아닌 사용자 ID로 Helm 차트를 배치한 후 사용자가 마운트된 스토리지에 쓸 수 없습니다.

{: tsCauses}
배치 또는 Helm 차트 구성은 팟(Pod)의 `fsGroup`(그룹 ID) 및 `runAsUser`(사용자 ID)에 대한 [보안 컨텍스트](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)를 지정합니다. 현재 {{site.data.keyword.containershort_notm}}는 `fsGroup` 스펙을 지원하지 않으며 `0`으로 설정된 `runAsUser`(루트 권한)만 지원합니다.

{: tsResolve}
이미지, 배치 또는 Helm 차트 구성 파일 및 다시 배치에서 `fsGroup` 및 `runAsUser`에 대한 구성의 `securityContext` 필드를 제거하십시오. `nobody`에서 마운트 경로의 소유권을 변경해야 하는 경우 [루트가 아닌 사용자 액세스 추가](#nonroot)를 수행하십시오. [루트가 아닌 initContainer](#nonroot)를 추가한 후에는 팟(Pod) 레벨이 아니라 컨테이너 레벨에서 `runAsUser`를 설정하십시오. 

<br />




## 잘못된 파일 시스템으로 인해 기존 블록 스토리지를 팟(Pod)에 마운트하는 데 실패함
{: #block_filesystem}

{: tsSymptoms}
`kubectl describe pod <pod_name>`을 실행할 때 다음 오류가 표시됩니다.
```
failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32
```
{: screen}

{: tsCauses}
`XFS` 파일 시스템을 사용하여 설정된 기존 블록 스토리지 디바이스가 있습니다. 이 디바이스를 팟(Pod)에 마운트하기 위해 `spec/flexVolume/fsType`에서 `ext4`를 파일 시스템으로 지정하거나 파일 시스템을 지정하지 않은 [PV를 작성](cs_storage.html#existing_block)했습니다. 파일 시스템이 정의되지 않으면 기본적으로 PV가 `ext4`로 설정됩니다.
PV가 성공적으로 작성되고 기존 블록 스토리지 인스턴스에 링크되었습니다. 그러나 일치하는 PVC를 사용하여 PV를 클러스터에 마운트하려고 하면 볼륨 마운트에 실패합니다.`ext4` 파일 시스템을 사용하는 `XFS` 블록 스토리지 인스턴스를 팟(Pod)에 마운트할 수 없습니다.

{: tsResolve}
기존 PV의 파일 시스템을 `ext4`에서 `XFS`로 업데이트하십시오.

1. 클러스터에 있는 기존 PV를 나열하고 기존 블록 스토리지 인스턴스에 사용한 PV의 이름을 기록해 두십시오.
   ```
    kubectl get pv
   ```
   {: pre}

2. PV yaml을 로컬 시스템에 저장하십시오.
   ```
   kubectl get pv <pv_name> -o yaml > <filepath/xfs_pv.yaml>
   ```
   {: pre}

3. yaml 파일을 열고 `fsType`을 `ext4`에서 `xfs`로 변경하십시오.
4. 클러스터의 PV를 대체하십시오.
   ```
   kubectl replace --force -f <filepath/xfs_pv.yaml>
   ```
   {: pre}

5. PV를 마운트한 팟(Pod)에 로그인하십시오.
   ```
   kubectl exec -it <pod_name> sh
   ```
   {: pre}

6. 파일 시스템이 `XFS`로 변경되었는지 확인하십시오.
   ```
   df -Th
   ```
   {: pre}

   출력 예:
   ```
   Filesystem Type Size Used Avail Use% Mounted on /dev/mapper/3600a098031234546d5d4c9876654e35 xfs 20G 33M 20G 1% /myvolumepath
   ```
   {: screen}

<br />



## 도움 및 지원 받기
{: #ts_getting_help}

클러스터에 여전히 문제점이 있습니까?
{: shortdesc}

-   {{site.data.keyword.Bluemix_notm}}가 사용 가능한지 확인하려면 [{{site.data.keyword.Bluemix_notm}} 상태 페이지![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")를 참조](https://developer.ibm.com/bluemix/support/#status)하십시오.
-   [{{site.data.keyword.containershort_notm}} Slack ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://ibm-container-service.slack.com)에 질문을 게시하십시오.

{{site.data.keyword.Bluemix_notm}} 계정에 대해 IBM ID를 사용 중이 아닌 경우에는 이 Slack에 대한 [초대를 요청](https://bxcs-slack-invite.mybluemix.net/)하십시오.
    {: tip}
-   포럼을 검토하여 다른 사용자에게도 동일한 문제가 발생하는지 여부를 확인하십시오. 포럼을 사용하여 질문을 할 때는 {{site.data.keyword.Bluemix_notm}} 개발 팀이 볼 수 있도록 질문에 태그를 지정하십시오.

    -   {{site.data.keyword.containershort_notm}}로 클러스터 또는 앱을 개발하거나 배치하는 데 대한 기술적 질문이 있으면 [스택 오버플로우![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers)에 질문을 게시하고 질문에 `ibm-cloud`, `kubernetes` 및 `containers` 태그를 지정하십시오.
    -   시작하기 지시사항과 서비스에 대한 질문은 [IBM developerWorks dW 응답![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) 포럼을 사용하십시오. `ibm-cloud` 및 `containers` 태그를 포함하십시오.
    포럼 사용에 대한 세부사항은 [도움 받기](/docs/get-support/howtogetsupport.html#using-avatar)를 참조하십시오.

-   티켓을 열어 IBM 지원 센터에 문의하십시오. IBM 지원 티켓 열기 또는 지원 레벨 및 티켓 심각도에 대해 알아보려면 [지원 문의](/docs/get-support/howtogetsupport.html#getting-customer-support)를 참조하십시오.

{: tip}
문제를 보고할 때 클러스터 ID를 포함시키십시오. 클러스터 ID를 가져오려면 `bx cs clusters`를 실행하십시오.

