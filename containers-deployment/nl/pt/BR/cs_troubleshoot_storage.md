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



# Resolução de problemas de armazenamento do cluster
{: #cs_troubleshoot_storage}

Ao usar o {{site.data.keyword.containerlong}}, considere estas técnicas para resolução de problemas de armazenamento em cluster.
{: shortdesc}

Se você tiver um problema mais geral, tente a [depuração do cluster](cs_troubleshoot.html).
{: tip}



## Os sistemas de arquivos para nós do trabalhador mudam para somente leitura
{: #readonly_nodes}

{: tsSymptoms}
{: #stuck_creating_state}
Você pode ver um dos sintomas a seguir:
- Quando você executa `kubectl get pods -o wide`, você vê que vários pods que estão em execução no mesmo nó do trabalhador estão presos no estado `ContainerCreating`.
- Ao executar um comando `kubectl describe`, você vê o erro a seguir na seção **Eventos**: `MountVolume.SetUp failed for volume ... read-only file system`.

{: tsCauses}
O sistema de arquivos no nó do trabalhador é somente leitura.

{: tsResolve}
1.  Faça backup de quaisquer dados que podem ser armazenados no nó do trabalhador ou em seus contêineres.
2.  Para uma correção de curto prazo para o nó do trabalhador existente, recarregue o nó do trabalhador.
    <pre class="pre"><code>bx cs worker-reload &lt;cluster_name&gt; &lt;worker_ID&gt;</code></pre>

Para uma correção de longo prazo, [atualize o tipo de máquina incluindo outro nó do trabalhador](cs_cluster_update.html#machine_type).

<br />



## O app falha quando um usuário não raiz possui o caminho de montagem de armazenamento de arquivo NFS
{: #nonroot}

{: tsSymptoms}
Depois de [incluir armazenamento NFS](cs_storage.html#app_volume_mount) em sua implementação, a implementação de seu contêiner falha. Ao recuperar os logs para seu contêiner, talvez você veja erros como "permissão de gravação" ou "não tem a permissão necessária". O pod falha e fica preso em um ciclo de recarregamento.

{: tsCauses}
Por padrão, os usuários não raiz não tem permissão de gravação no caminho de montagem do volume para armazenamento suportado por NFS. Algumas imagens comuns do app, como Jenkins e Nexus3, especificam um usuário não raiz que possui o caminho de montagem no Dockerfile. Quando você cria um contêiner por meio desse Dockerfile, a criação do contêiner falha devido a permissões insuficientes do usuário não raiz no caminho de montagem. Para conceder permissão de gravação, é possível modificar o Dockerfile para incluir temporariamente o usuário não raiz no grupo de usuários raiz antes que ele mude as permissões de caminho de montagem ou usar um contêiner de inicialização.

Se você usar um gráfico Helm para implementar a imagem, edite a implementação do Helm para usar um contêiner de inicialização.
{:tip}



{: tsResolve}
Quando você inclui um [contêiner de inicialização![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) em sua implementação, é possível fornecer a um usuário não raiz que está especificado em seu Dockerfile as permissões de gravação para o caminho de montagem do volume dentro do contêiner. O contêiner de inicialização é iniciado antes de seu contêiner de app ser iniciado. O contêiner de inicialização cria o caminho de montagem do volume dentro do contêiner, muda o caminho de montagem para ser de propriedade do usuário correto (não raiz) e fecha. Em seguida, seu contêiner de app é iniciado com o usuário não raiz que deve gravar no caminho de montagem. Como o caminho já pertence ao usuário não raiz, a gravação no caminho de montagem é bem-sucedida. Se você não deseja usar um contêiner de inicialização, é possível modificar o Dockerfile para incluir acesso de usuário não raiz no armazenamento de arquivo NFS.


Antes de iniciar, [destine sua CLI](cs_cli_install.html#cs_cli_configure) para seu cluster.

1.  Abra o Dockerfile para seu app e obtenha o ID do usuário (UID) e ID do grupo (GID) do usuário que você deseja fornecer permissão de gravação no caminho de montagem do volume. No exemplo de um Dockerfile Jenkins, as informações são:
    - UID: `1000`
    - GID: `1000`

    **Exemplo**:

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

2.  Inclua armazenamento persistente para seu app criando um persistent volume claim (PVC). Este exemplo usa a classe de armazenamento `ibmc-file-bronze`. Para revisar as classes de armazenamento disponíveis, execute `kubectl get storageclasses`.

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

3.  Crie o PVC.

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

4.  Em seu arquivo de implementação `.yaml`, inclua o contêiner de inicialização. Inclua o UID e o GID que você recuperou anteriormente.

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

    **Exemplo** para uma implementação Jenkins:

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

5.  Crie o pod e monte o PVC em seu pod.

    ```
    kubectl apply -f my_pod.yaml
    ```
    {: pre}

6. Verifique se o volume foi montado com êxito no pod. Anote o nome do pod e o caminho **Contêineres/Montagens**.

    ```
    kubectl describe pod <my_pod>
    ```
    {: pre}

    **Saída de exemplo**:

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

7.  Efetue login no pod usando o nome do pod que você anotou anteriormente.

    ```
    kubectl exec -it <my_pod-123456789> /bin/bash
    ```
    {: pre}

8. Verifique as permissões do caminho de montagem do seu contêiner. No exemplo, o caminho de montagem é `/var/jenkins_home`.

    ```
    ls -ln /var/jenkins_home
    ```
    {: pre}

    **Saída de exemplo**:

    ```
    jenkins@mypod-123456789:/$ ls -ln /var/jenkins_home
    total 12
    -rw-r--r-- 1 1000 1000  102 Mar  9 19:58 copy_reference_file.log
    drwxr-xr-x 2 1000 1000 4096 Mar  9 19:58 init.groovy.d
    drwxr-xr-x 9 1000 1000 4096 Mar  9 20:16 war
    ```
    {: screen}

    Essa saída mostra que o GID e o UID de seu Dockerfile (neste exemplo, `1000` e `1000`) possuem o caminho de montagem dentro do contêiner.

<br />


## A inclusão do acesso de usuário não raiz no armazenamento persistente falha
{: #cs_storage_nonroot}

{: tsSymptoms}
Depois de [incluir acesso de usuário não raiz no armazenamento persistente](#nonroot) ou implementar um gráfico Helm com um ID do usuário não raiz especificado, o usuário não pode gravar no armazenamento montado.

{: tsCauses}
A configuração da implementação ou do gráfico Helm especifica o [contexto de segurança](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/) para o `fsGroup` (ID do grupo) e `runAsUser` (ID do usuário) do pod. Atualmente, o {{site.data.keyword.containershort_notm}} não suporta a especificação `fsGroup` e suporta somente `runAsUser` configurado como `0` (permissões raiz).

{: tsResolve}
Remova os campos `securityContext` da configuração para `fsGroup` e `runAsUser` do arquivo de configuração de imagem, implementação ou gráfico Helm e reimplemente. Se você precisa mudar a propriedade do caminho de montagem de `nobody`, [inclua o acesso de usuário não raiz](#nonroot). Depois de incluir o [initContainer não raiz](#nonroot), configure `runAsUser` no nível de contêiner, não no nível de pod.

<br />




## A montagem do armazenamento de bloco existente em um pod falha devido ao sistema de arquivos errado
{: #block_filesystem}

{: tsSymptoms}
Quando você executa `kubectl describe pod <pod_name>`, você vê o erro a seguir:
```
failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32
```
{: screen}

{: tsCauses}
Você tem um dispositivo de armazenamento de bloco existente que é configurado com um sistema de arquivos `XFS`. Para montar esse dispositivo em seu pod, você [criou um PV](cs_storage.html#existing_block) que especificou `ext4` como seu sistema de arquivos ou nenhum sistema de arquivos na seção `spec/flexVolume/fsType`. Se nenhum sistema de arquivos está definido, o PV é padronizado para `ext4`.
O PV foi criado com êxito e foi vinculado à sua instância de armazenamento de bloco existente. No entanto, quando você tenta montar o PV em seu cluster usando um PVC correspondente, o volume falha ao ser montado. Não é possível montar sua instância de armazenamento de bloco `XFS` com um sistema de arquivos `ext4` no pod.

{: tsResolve}
Atualize o sistema de arquivos no PV existente de `ext4` para `XFS`.

1. Liste os PVs existentes em seu cluster e anote o nome do PV que você usou para a sua instância de armazenamento de bloco existente.
   ```
   kubectl get pv
   ```
   {: pre}

2. Salve o yaml do PV em sua máquina local.
   ```
   kubectl get pv <pv_name> -o yaml > <filepath/xfs_pv.yaml>
   ```
   {: pre}

3. Abra o arquivo yaml e mude o `fsType` de `ext4` para `xfs`.
4. Substitua o PV em seu cluster.
   ```
   kubectl replace --force -f <filepath/xfs_pv.yaml>
   ```
   {: pre}

5. Efetue login no pod no qual você montou o PV.
   ```
   Kubectl exec -it < pod_name> sh
   ```
   {: pre}

6. Verifique se o sistema de arquivos mudou para `XFS`.
   ```
   -th df
   ```
   {: pre}

   Saída de exemplo:
   ```
   Filesystem Type Size Used Avail Use% Mounted on /dev/mapper/3600a098031234546d5d4c9876654e35 xfs 20G 33M 20G 1% /myvolumepath
   ```
   {: screen}

<br />



## Obtendo ajuda e suporte
{: #ts_getting_help}

Ainda está tendo problemas com o seu cluster?
{: shortdesc}

-   Para ver se o {{site.data.keyword.Bluemix_notm}} está disponível, [verifique a página de status do {{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/bluemix/support/#status).
-   Poste uma pergunta no [{{site.data.keyword.containershort_notm}} Slack ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://ibm-container-service.slack.com).

    Se você não estiver usando um IBMid para a sua conta do {{site.data.keyword.Bluemix_notm}}, [solicite um convite](https://bxcs-slack-invite.mybluemix.net/) para essa Folga.
    {: tip}
-   Revise os fóruns para ver se outros usuários tiveram o mesmo problema. Ao usar os fóruns para fazer uma pergunta, marque sua pergunta para que ela seja vista pelas equipes de desenvolvimento do {{site.data.keyword.Bluemix_notm}}.

    -   Se você tiver questões técnicas sobre como desenvolver ou implementar clusters ou apps com o {{site.data.keyword.containershort_notm}}, poste sua pergunta no [Stack Overflow ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) e identifique a sua pergunta com `ibm-cloud`, `kubernetes` e `containers`.
    -   Para perguntas sobre o serviço e instruções de introdução, use o fórum [IBM developerWorks dW Answers ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix). Inclua as tags `ibm-cloud` e `containers`.
    Consulte [Obtendo ajuda](/docs/get-support/howtogetsupport.html#using-avatar) para obter mais detalhes sobre o uso dos fóruns.

-   Entre em contato com o Suporte IBM abrindo um chamado. Para saber como abrir um chamado de suporte IBM ou sobre os níveis de suporte e as severidades de chamado, veja [Entrando em contato com o suporte](/docs/get-support/howtogetsupport.html#getting-customer-support).

{: tip}
Ao relatar um problema, inclua o ID do cluster. Para obter o ID do cluster, execute `bx cs clusters`.

