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



# Resolução de problemas de clusters e nós do trabalhador
{: #cs_troubleshoot_clusters}

Ao usar o {{site.data.keyword.containerlong}}, considere estas técnicas para resolução de problemas de seus clusters e nós do trabalhador.
{: shortdesc}

Se você tiver um problema mais geral, tente a [depuração do cluster](cs_troubleshoot.html).
{: tip}

## Não é possível se conectar à sua conta de infraestrutura
{: #cs_credentials}

{: tsSymptoms}
Ao criar um novo cluster do Kubernetes, você receberá a mensagem a seguir.

```
Não foi possível se conectar à sua conta de infraestrutura do IBM Cloud (SoftLayer).
Criar um cluster padrão requer que você tenha uma conta pré-paga vinculada a um termo da conta de infraestrutura do IBM Cloud (SoftLayer) ou que tenha usado a CLI do {{site.data.keyword.containerlong}} para configurar as suas chaves API de Infraestrutura do {{site.data.keyword.Bluemix_notm}}.
```
{: screen}

{: tsCauses}
As contas pré-pagas do {{site.data.keyword.Bluemix_notm}} que foram criadas após a vinculação de conta automática ter sido ativada já estão configuradas com acesso ao portfólio da infraestrutura do IBM Cloud (SoftLayer). É possível comprar recursos de infraestrutura para seu cluster sem configuração adicional.

Os usuários com outros tipos de conta do {{site.data.keyword.Bluemix_notm}} que têm uma conta de infraestrutura do IBM Cloud existente (SoftLayer) que não está vinculada à sua conta do {{site.data.keyword.Bluemix_notm}} devem configurar suas contas para criar clusters padrão.

{: tsResolve}
Configurar sua conta para acessar o portfólio de infraestrutura do IBM Cloud (SoftLayer) depende do tipo de conta que você tem. Revise a tabela para localizar opções disponíveis para cada tipo de conta.

|Tipo de conta|Descrição|Opções disponíveis para criar um cluster padrão|
|------------|-----------|----------------------------------------------|
|Contas Lite|Contas Lite não podem provisionar clusters.|[Faça upgrade de sua conta Lite para uma {{site.data.keyword.Bluemix_notm}}conta pré-paga](/docs/account/index.html#paygo) que está configurada com acesso ao portfólio de infraestrutura do IBM Cloud (SoftLayer).|
|Contas pré-pagas|As contas pré-paga que foram criadas antes de a vinculação de conta automática estar disponível não vieram com acesso ao portfólio de infraestrutura do IBM Cloud (SoftLayer).<p>Se você tiver uma conta de infraestrutura do IBM Cloud existente (SoftLayer), não será possível vincular essa conta a uma conta pré-paga mais antiga.</p>|<strong>Opção 1:</strong> [Criar uma nova conta pré-paga](/docs/account/index.html#paygo) que é configurada com acesso ao portfólio de infraestrutura do IBM Cloud (SoftLayer). Ao escolher essa opção,
você tem duas contas e faturamentos separados do
{{site.data.keyword.Bluemix_notm}}.<p>Para continuar usando a sua antiga conta pré-paga, é possível usar sua nova conta pré-paga para gerar uma chave API para acessar o portfólio de infraestrutura do IBM Cloud (SoftLayer). Em seguida, deve-se [configurar a chave API de infraestrutura do IBM Cloud (SoftLayer) para sua antiga conta pré-paga](cs_cli_reference.html#cs_credentials_set). </p><p><strong>Opção 2:</strong> se você já tem uma conta de infraestrutura do IBM Cloud existente (SoftLayer) que deseja usar, é possível [configurar suas credenciais](cs_cli_reference.html#cs_credentials_set) em sua conta do {{site.data.keyword.Bluemix_notm}}.</p><p>**Nota:** quando você se vincula manualmente a uma conta de infraestrutura do IBM Cloud (SoftLayer), as credenciais são usadas para cada ação específica da infraestrutura do IBM Cloud (SoftLayer) em sua conta do {{site.data.keyword.Bluemix_notm}}. Deve-se assegurar que a chave API configurada possua [permissões de infraestrutura suficientes](cs_users.html#infra_access) para que os usuários possam criar e trabalhar com clusters.</p>|
|Contas de assinatura|As contas de assinatura não são configuradas com acesso ao portfólio de infraestrutura do IBM Cloud (SoftLayer).|<strong>Opção 1:</strong> [Criar uma nova conta pré-paga](/docs/account/index.html#paygo) que é configurada com acesso ao portfólio de infraestrutura do IBM Cloud (SoftLayer). Ao escolher essa opção,
você tem duas contas e faturamentos separados do
{{site.data.keyword.Bluemix_notm}}.<p>Se você deseja continuar usando a sua conta de Assinatura, é possível usar sua nova conta pré-paga para gerar uma chave API na infraestrutura do IBM Cloud (SoftLayer). Em seguida, deve-se [configurar manualmente a chave API de infraestrutura do IBM Cloud (SoftLayer) para sua conta de Assinatura](cs_cli_reference.html#cs_credentials_set). Mantenha em mente que os recursos de infraestrutura do IBM Cloud (SoftLayer) são cobrados através de sua nova conta pré-paga.</p><p><strong>Opção 2:</strong> se você já tem uma conta de infraestrutura do IBM Cloud existente (SoftLayer) que deseja usar, é possível [configurar manualmente as credenciais de infraestrutura do IBM Cloud (SoftLayer)](cs_cli_reference.html#cs_credentials_set) para sua conta do {{site.data.keyword.Bluemix_notm}}.<p>**Nota:** quando você se vincula manualmente a uma conta de infraestrutura do IBM Cloud (SoftLayer), as credenciais são usadas para cada ação específica da infraestrutura do IBM Cloud (SoftLayer) em sua conta do {{site.data.keyword.Bluemix_notm}}. Deve-se assegurar que a chave API configurada possua [permissões de infraestrutura suficientes](cs_users.html#infra_access) para que os usuários possam criar e trabalhar com clusters.</p>|
|Contas de infraestrutura do IBM Cloud (SoftLayer), nenhuma conta do {{site.data.keyword.Bluemix_notm}}|Para criar um cluster padrão, deve-se ter uma conta do {{site.data.keyword.Bluemix_notm}}.|<p>[Crie uma conta pré-paga](/docs/account/index.html#paygo) que esteja configurada com acesso ao portfólio de infraestrutura do IBM Cloud (SoftLayer). Ao escolher essa opção, uma conta de infraestrutura do IBM Cloud (SoftLayer) é criada para você. Você tem duas contas de infraestrutura do IBM Cloud (SoftLayer) separadas e faturamento.</p>|
{: caption="Opções de criação de cluster padrão por tipo de conta" caption-side="top"}


<br />


## O firewall evita a execução de comandos da CLI
{: #ts_firewall_clis}

{: tsSymptoms}
Quando você executa os comandos `bx`, `kubectl` ou `calicoctl` na CLI, eles falham.

{: tsCauses}
Você pode ter políticas de rede corporativa que impedem o acesso de seu sistema local a terminais públicos por proxies ou firewalls.

{: tsResolve}
[Permita acesso TCP para os comandos da CLI funcionarem](cs_firewall.html#firewall). Essa tarefa requer uma [Política de acesso de administrador](cs_users.html#access_policies). Verifique sua [política de acesso](cs_users.html#infra_access) atual.


## O firewall evita que o cluster se conecte a recursos
{: #cs_firewall}

{: tsSymptoms}
Quando os nós do trabalhador não podem se conectar, você pode ver vários sintomas diferentes. É possível que você veja uma das mensagens a seguir quando o proxy kubectl falhar ou você tentar acessar um serviço em seu cluster e a conexão falhar.

  ```
  Conexão recusada
  ```
  {: screen}

  ```
  Conexão expirada
  ```
  {: screen}

  ```
  Não é possível se conectar ao servidor: net/http: tempo limite do handshake do TLS
  ```
  {: screen}

Se você executar kubectl exec, attach ou logs, é possível ver a mensagem a seguir.

  ```
  Error from server: error dialing backend: dial tcp XXX.XXX.XXX:10250: getsockopt: connection timed out
  ```
  {: screen}

Se o proxy kubectl for bem-sucedido, mas o painel não estiver disponível, será possível que você veja a mensagem a seguir.

  ```
  timeout on 172.xxx.xxx.xxx
  ```
  {: screen}



{: tsCauses}
Você pode ter outro firewall configurado ou pode ter customizado suas configurações de firewall existentes em sua conta de infraestrutura do IBM Cloud (SoftLayer). O {{site.data.keyword.containershort_notm}} requer que determinados endereços IP e portas sejam abertos para permitir a comunicação do nó do trabalhador com o mestre do Kubernetes e vice-versa. Outro motivo talvez seja que os nós do trabalhador estejam presos em um loop de recarregamento.

{: tsResolve}
[Permita que o cluster acesse os recursos de infraestrutura e outros serviços](cs_firewall.html#firewall_outbound). Essa tarefa requer uma [Política de acesso de administrador](cs_users.html#access_policies). Verifique sua [política de acesso](cs_users.html#infra_access) atual.

<br />



## O acesso a seu nó do trabalhador com SSH falha
{: #cs_ssh_worker}

{: tsSymptoms}
Não é possível acessar seu nó do trabalhador usando uma conexão SSH.

{: tsCauses}
O SSH via senha está desativado nos nós do trabalhador.

{: tsResolve}
Use [DaemonSets ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) para qualquer coisa que precisar ser executada em cada nó ou tarefas para qualquer ação única que precisar ser executada.

<br />


## `kubectl exec` e `kubectl logs` não funcionam
{: #exec_logs_fail}

{: tsSymptoms}
Se você executar `kubectl exec` ou `kubectl logs`, você verá a mensagem a seguir.

  ```
  WorkerIP> <: 10250: getsockopt: connection timed out
  ```
  {: screen}

{: tsCauses}
A conexão OpenVPN entre o nó principal e os nós do trabalhador não está funcionando corretamente.

{: tsResolve}
1. Ative [VLAN Spanning](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning) para sua conta de infraestrutura do IBM Cloud (SoftLayer).
2. Reinicie o pod cliente OpenVPN.
  ```
  kubectl delete pod -n kube-system -l app=vpn
  ```
  {: pre}
3. Se você ainda vê a mesma mensagem de erro, então o nó do trabalhador em que o pod de VPN está pode não estar saudável. Para reiniciar o pod de VPN e reagendá-lo para um nó do trabalhador diferente, [bloqueie, drene e reinicialize o nó do trabalhador](cs_cli_reference.html#cs_worker_reboot) em que o pod de VPN está.

<br />


## Ligar um serviço a um cluster resulta no erro de mesmo nome
{: #cs_duplicate_services}

{: tsSymptoms}
Ao executar `bx cs cluster-service-bind <cluster_name> <namespace> <service_instance_name>`, você vê a mensagem a seguir.

```
Múltiplos serviços com o mesmo nome foram localizados.
Execute 'bx service list' para visualizar as instâncias de serviço disponíveis do Bluemix...
```
{: screen}

{: tsCauses}
Múltiplas instâncias de serviço podem ter o mesmo nome em regiões diferentes.

{: tsResolve}
Use o GUID de serviço em vez do nome da instância de serviço no comando `bx cs cluster-service-bind`.

1. [Efetue login na região que inclui a instância de serviço para ligação.](cs_regions.html#bluemix_regions)

2. Obtenha o GUID para a instância de serviço.
  ```
  bx service show <service_instance_name> --guid
  ```
  {: pre}

  Saída:
  ```
  Invoking 'cf service <service_instance_name> --guid'...
  <service_instance_GUID>
  ```
  {: screen}
3. Ligue o serviço ao cluster novamente.
  ```
  bx cs cluster-service-bind <cluster_name> <namespace> <service_instance_GUID>
  ```
  {: pre}

<br />


## Ligar um serviço a um cluster resulta no erro de serviço não localizado
{: #cs_not_found_services}

{: tsSymptoms}
Ao executar `bx cs cluster-service-bind <cluster_name> <namespace> <service_instance_name>`, você vê a mensagem a seguir.

```
Ligando o serviço a um namespace...
COM FALHA

O serviço IBM Cloud especificado não pôde ser localizado. Se você acabou de criar o serviço, espere um pouco e, em seguida, tente ligá-lo novamente. Para visualizar as instâncias de serviço disponíveis do IBM Cloud, execute 'bx service list'. (E0023)
```
{: screen}

{: tsCauses}
Para ligar serviços a um cluster, deve-se ter a função de usuário de desenvolvedor do Cloud Foundry para o espaço no qual a instância de serviço é provisionada. Além disso, deve-se ter o acesso de Editor do IAM para o {{site.data.keyword.containerlong}}. Para acessar a instância de serviço, deve-se ter efetuado login no espaço no qual a instância de serviço é provisionada. 

{: tsResolve}

**Como o usuário:**

1. Efetue login no {{site.data.keyword.Bluemix_notm}}. 
   ```
   bx login
   ```
   {: pre}
   
2. Destine a organização e o espaço nos quais a instância de serviço é provisionada. 
   ```
   bx target -o <org> -s <space>
   ```
   {: pre}
   
3. Verifique se você está no espaço certo listando suas instâncias de serviço. 
   ```
   bx service list 
   ```
   {: pre}
   
4. Tente ligar o serviço novamente. Se obtiver o mesmo erro, entre em contato com o administrador de conta e verifique se você tem permissões suficientes para ligar serviços (veja as etapas de administrador de conta). 

**Como o administrador de conta:**

1. Verifique se o usuário que experiencia esse problema tem [permissões de Editor para o {{site.data.keyword.containerlong}}](/docs/iam/mngiam.html#editing-existing-access). 

2. Verifique se o usuário que experiencia esse problema tem a [função de desenvolvedor do Cloud Foundry para o espaço](/docs/iam/mngcf.html#updating-cloud-foundry-access) no qual o serviço é provisionado. 

3. Se as permissões corretas existirem, tente designar uma permissão diferente e, em seguida, redesignar a permissão necessária. 

4. Aguarde alguns minutos, em seguida, permita que o usuário tente ligar o serviço novamente. 

5. Se isso não resolver o problema, as permissões do IAM estão fora de sincronização e não é possível resolver o problema sozinho. [Entre em contato com o suporte IBM](/docs/get-support/howtogetsupport.html#getting-customer-support) abrindo um chamado de suporte. Certifique-se de fornecer o ID do cluster, o ID do usuário e o ID da instância de serviço. 
   1. Recupere o ID do cluster.
      ```
      bx cs clusters
      ```
      {: pre}
      
   2. Recupere o ID da instância de serviço. 
      ```
      Bx service show < service_name> -- guid
      ```
      {: pre}


<br />



## Após um nó do trabalhador ser atualizado ou recarregado, nós e pods duplicados aparecem
{: #cs_duplicate_nodes}

{: tsSymptoms}
Ao executar `kubectl get nodes`, você vê nós do trabalhador duplicados com o status **NotReady**. Os nós do trabalhador com **NotReady** têm endereços IP públicos, enquanto os nós do trabalhador com **Ready** possuem endereços IP privados.

{: tsCauses}
Os clusters mais antigos listam os nós do trabalhador pelo endereço IP público do cluster. Agora, os nós do trabalhador são listados pelo endereço IP privado do cluster. Ao recarregar ou atualizar um nó, o endereço IP é mudado, mas a referência ao endereço IP público permanece.

{: tsResolve}
O serviço não é interrompido devido a essas duplicatas, mas é possível remover as referências do nó do trabalhador antigo do servidor de API.

  ```
  kubectl delete node <node_name1> <node_name2>
  ```
  {: pre}

<br />


## Após um nó do trabalhador ser atualizado ou recarregado, os aplicativos recebem erros RBAC DENY
{: #cs_rbac_deny}

{: tsSymptoms}
Depois de atualizar para o Kubernetes versão 1.7, os aplicativos recebem erros `RBAC DENY`.

{: tsCauses}
A partir do [Kubernetes versão 1.7](cs_versions.html#cs_v17), os aplicativos executados no namespace `padrão` não têm mais privilégios de administrador de cluster para a API do Kubernetes para segurança aprimorada.

Se o seu app for executado no namespace `padrão`, use o `default ServiceAccount` e acesse a API do Kubernetes, ele é afetado por esta mudança do Kubernetes. Para obter mais informações, veja [a documentação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/admin/authorization/rbac/#upgrading-from-15).

{: tsResolve}
Antes de iniciar, [destine sua CLI](cs_cli_install.html#cs_cli_configure) para seu cluster.

1.  **Ação provisória**: ao atualizar as políticas de RBAC do app, talvez você queira reverter provisoriamente para o `ClusterRoleBinding` anterior do `default ServiceAccount` no namespace `default`.

    1.  Copie os seguintes `.yaml` arquivo.

        ```yaml
        kind: ClusterRoleBinding
        apiVersion: rbac.authorization.k8s.io/v1beta1
        metadata:
         name: admin-binding-nonResourceURLSs-default
        subjects:
          - kind: ServiceAccount
      name: default
      namespace: default
  roleRef:
   kind: ClusterRole
   name: admin-role-nonResourceURLSs
   apiGroup: rbac.authorization.k8s.io
        ---
        kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
 name: admin-binding-resourceURLSs-default
subjects:
          - kind: ServiceAccount
      name: default
      namespace: default
  roleRef:
   kind: ClusterRole
   name: admin-role-resourceURLSs
   apiGroup: rbac.authorization.k8s.io
        ```

    2.  Aplique os arquivos `.yaml` em seu cluster.

        ```
        Kubectl apply -f FILENAME
        ```
        {: pre}

2.  [Crie recursos de autorização RBAC![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview) para atualizar o acesso de administrador `ClusterRoleBinding`.

3.  Se você criou uma ligação de função de cluster provisória, remova-a.

<br />


## Acessar um pod em um novo nó do trabalhador falha com um tempo limite
{: #cs_nodes_duplicate_ip}

{: tsSymptoms}
Você excluiu um nó do trabalhador no cluster e, em seguida, incluiu um nó do trabalhador. Ao implementar um pod ou um serviço do Kubernetes, o recurso não pode acessar o nó do trabalhador recém-criado e os tempos de conexão se esgotam.

{: tsCauses}
Se você exclui um nó do trabalhador de seu cluster e, em seguida, inclui um nó do trabalhador, o novo nó do trabalhador pode ser designado a endereço IP privado do nó do trabalhador excluído. O Calico usa esse endereço IP privado como uma tag e continua tentando acessar o nó excluído.

{: tsResolve}
Atualize manualmente a referência do endereço IP privado para apontar para o nó correto.

1.  Confirme que você tem dois nós do trabalhador com o mesmo endereço **IP privado**. Observe o **IP privado** e o **ID** do trabalhador excluído.

  ```
  bx cs workers <CLUSTER_NAME>
  ```
  {: pre}

  ```
  ID                                                 Public IP       Private IP       Machine Type   State     Status   Location   Version
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w1   169.xx.xxx.xxx  10.xxx.xx.xxx    b2c.4x16       normal    Ready    dal10      1.9.7
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w2   169.xx.xxx.xxx  10.xxx.xx.xxx    b2c.4x16       deleted    -       dal10      1.9.7
  ```
  {: screen}

2.  Instale a [CLI do Calico](cs_network_policy.html#adding_network_policies).
3.  Liste os nós do trabalhador disponíveis no Calico. Substitua <path_to_file> pelo caminho local para o arquivo de configuração do Calico.

  ```
  calicoctl get nodes --config=filepath/calicoctl.cfg
  ```
  {: pre}

  ```
  NAME
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w1
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w2
  ```
  {: screen}

4.  Exclua o nó do trabalhador duplicado no Calico. Substitua NODE_ID pelo ID do nó do trabalhador.

  ```
  calicoctl delete node NODE_ID --config=<path_to_file>/calicoctl.cfg
  ```
  {: pre}

5.  Reinicialize o nó do trabalhador que não foi excluído.

  ```
  bx cs worker-reboot CLUSTER_ID NODE_ID
  ```
  {: pre}


O nó excluído não é mais listado no Calico.

<br />


## O cluster permanece em um estado pendente
{: #cs_cluster_pending}

{: tsSymptoms}
Quando você implementa o seu cluster, ele permanece em um estado pendente e não é iniciado.

{: tsCauses}
Se você acabou de criar o cluster, os nós do trabalhador podem ainda estar sendo configurados. Se já espera por um tempo, você pode ter uma VLAN inválida.

{: tsResolve}

É possível tentar uma das soluções a seguir:
  - Verifique o status de seu cluster executando `bx cs clusters`. Em seguida, verifique se os nós do trabalhador estão implementados executando `bx cs workers <cluster_name>`.
  - Verifique se a sua VLAN é válida. Para ser válida, uma VLAN deve ser associada à infraestrutura que pode hospedar um trabalhador com armazenamento em disco local. É possível [listar suas VLANs](/docs/containers/cs_cli_reference.html#cs_vlans) executando `bx cs vlans<location>`, se a VLAN não é mostrada na lista, então ela não é válida. Escolha uma VLAN diferente.

<br />


## Os pods permanecem no estado pendente
{: #cs_pods_pending}

{: tsSymptoms}
Ao executar `kubectl get pods`, será possível ver pods que permanecem em um estado **Pendente**.

{: tsCauses}
Se você acabou de criar o cluster do Kubernetes, os nós do trabalhador podem ainda estar configurando. Se esse cluster for um existente, você talvez não tenha capacidade suficiente no cluster para implementar o pod.

{: tsResolve}
Essa tarefa requer uma [Política de acesso de administrador](cs_users.html#access_policies). Verifique sua [política de acesso](cs_users.html#infra_access) atual.

Se você acabou de criar o cluster do Kubernetes, execute o comando a seguir e aguarde a inicialização dos nós do trabalhador.

```
kubectl get nodes
```
{: pre}

Se esse cluster for um existente, verifique a capacidade dele.

1.  Configure o proxy com o número da porta padrão.

  ```
  kubectl proxy
  ```
   {: pre}

2.  Abra o painel do Kubernetes.

  ```
  http://localhost:8001/ui
  ```
  {: pre}

3.  Verifique se há capacidade suficiente no cluster para implementar o pod.

4.  Se não houver capacidade suficiente no cluster, inclua outro nó do trabalhador no cluster.

    ```
    Bx cs worker-add < cluster_name_or_ID> 1
    ```
    {: pre}

5.  Se os pods ainda estiverem em um estado **pendente** depois que o nó do trabalhador for totalmente implementado, revise a [documentação do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-pod-replication-controller/#my-pod-stays-pending) para solucionar posteriormente os problemas no estado pendente do pod.

<br />




## Os contêineres não iniciam
{: #containers_do_not_start}

{: tsSymptoms}
Os pods são implementados com êxito em clusters, mas os contêineres não iniciam.

{: tsCauses}
Os contêineres não podem ser iniciados quando a cota de registro é atingido.

{: tsResolve}
[Liberar armazenamento em {{site.data.keyword.registryshort_notm}}.](../services/Registry/registry_quota.html#registry_quota_freeup)

<br />



## Não é possível instalar um gráfico Helm com valores de configuração atualizados
{: #cs_helm_install}

{: tsSymptoms}
Ao tentar instalar um gráfico Helm atualizado executando `helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>`, você obtém a mensagem de erro `Error: failed to download "ibm/<chart_name>"`.

{: tsCauses}
A URL para o repositório do {{site.data.keyword.Bluemix_notm}} em sua instância do Helm pode estar incorreta.

{: tsResolve}
Para solucionar problemas de seu gráfico Helm:

1. Liste os repositórios disponíveis atualmente em sua instância do Helm.

    ```
    helm repo list
    ```
    {: pre}

2. Na saída, verifique se a URL para o repositório do {{site.data.keyword.Bluemix_notm}}, `ibm`, é `https://registry.bluemix.net/helm/ibm`.

    ```
    NAME    URL
    stable  https://kubernetes-charts.storage.googleapis.com
    local   http://127.0.0.1:8888/charts
    ibm     https://registry.bluemix.net/helm/ibm
    ```
    {: screen}

    * Se a URL estiver incorreta:

        1. Remova o repositório do {{site.data.keyword.Bluemix_notm}}.

            ```
            helm repo remove ibm
            ```
            {: pre}

        2. Inclua o repositório do {{site.data.keyword.Bluemix_notm}} novamente.

            ```
            helm repo add ibm https://registry.bluemix.net/helm/ibm
            ```
            {: pre}

    * Se a URL estiver correta, obtenha as atualizações mais recentes do repositório.

        ```
        helm repo update
        ```
        {: pre}

3. Instale o gráfico Helm com suas atualizações.

    ```
    helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>
    ```
    {: pre}

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

