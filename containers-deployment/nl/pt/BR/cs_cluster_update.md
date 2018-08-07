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





# Atualizando clusters e nós do trabalhador
{: #update}

É possível instalar atualizações para manter seus clusters do Kubernetes atualizados no {{site.data.keyword.containerlong}}.
{:shortdesc}

## Atualizando o mestre do Kubernetes
{: #master}

Periodicamente, o Kubernetes libera [atualizações principais, secundárias ou de correção](cs_versions.html#version_types). Dependendo do tipo de atualização, você pode ser responsável por atualizar os componentes do mestre do Kubernetes.
{:shortdesc}

As atualizações podem afetar a versão do servidor da API do Kubernetes ou outros componentes em seu mestre do Kubernetes.  Você é sempre responsável por manter seus nós do trabalhador atualizados. Ao fazer atualizações, o mestre do Kubernetes é atualizado antes dos nós do trabalhador.

Por padrão, sua capacidade de atualizar o servidor da API do Kubernetes é limitada em seu mestre do Kubernetes para mais de duas versões secundárias à frente de sua versão atual. Por exemplo, se a sua versão atual do servidor da API do Kubernetes é 1.7 e você deseja atualizar para a 1.10, deve-se primeiro atualizar para a 1.8 ou 1.9. É possível forçar a ocorrência da atualização, mas atualizar três ou mais versões secundárias pode causar resultados inesperados. Se o seu cluster está executando uma versão do Kubernetes não suportada, você pode ter que forçar a atualização.

O diagrama a seguir mostra o processo que você pode usar para atualizar seu mestre.

![Melhor prática de atualização de mestre](/images/update-tree.png)

Figura 1. Atualizando o diagrama do processo de mestre do Kubernetes

**Atenção**: não é possível recuperar um cluster para uma versão anterior depois que o processo de atualização ocorre. Certifique-se de usar um cluster de teste e siga as instruções para direcionar problemas potenciais antes de atualizar o mestre de produção.

Para atualizações _principal_ ou _menor_, conclua as etapas a seguir:

1. Revise as [mudanças do Kubernetes](cs_versions.html) e faça as atualizações marcadas como _Atualizar antes do mestre_.
2. Atualize o seu servidor da API do Kubernetes e os componentes do mestre do Kubernetes associados usando a GUI ou executando o [comando da CLI](cs_cli_reference.html#cs_cluster_update). Quando você atualiza o servidor da API do Kubernetes, o servidor da API fica inativo por cerca de 5 a 10 minutos. Durante a atualização, não é possível acessar nem mudar o cluster. No entanto, os nós do trabalhador, apps e recursos que os usuários do cluster implementaram não serão modificados e continuarão a executar.
3. Confirme que a atualização foi concluída. Revise a versão do servidor da API do Kubernetes no Painel do {{site.data.keyword.Bluemix_notm}} ou execute `bx cs clusters`.
4. Instale a versão do [`kubectl cli`](cs_cli_install.html#kubectl) que corresponde à versão do servidor da API do Kubernetes que é executada no mestre do Kubernetes.

Quando a atualização do servidor da API do Kubernetes for concluída, será possível atualizar seus nós do trabalhador.

<br />


## Atualizando nós do trabalhador
{: #worker_node}


Você recebeu uma notificação para atualizar seus nós do trabalhador. O que isso significa? Conforme as atualizações de segurança e as correções são colocadas no local para o servidor da API do Kubernetes e outros componentes do mestre do Kubernetes, deve-se ter certeza de que os nós do trabalhador permanecem em sincronização.
{: shortdesc}

A versão do Kubernetes do nó do trabalhador não pode ser maior que a versão do servidor da API do Kubernetes que é executada em seu mestre do Kubernetes. Antes de iniciar, [atualize o mestre do Kubernetes](#master).

**Atenção**:
<ul><li>As atualizações para os nós do trabalhador podem causar tempo de inatividade para seus apps e serviços.</li>
<li>Os dados são excluídos se não armazenados fora do pod.</li>
<li>Use [réplicas ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#replicas) em suas implementações para reagendar os pods em nós disponíveis.</li></ul>

Mas e se eu não puder ter o tempo de inatividade?

Como parte do processo de atualização, nós específicos vão ficar inativos por um período de tempo. Para ajudar a evitar o tempo de inatividade para seu aplicativo, é possível definir chaves exclusivas em um mapa de configuração que especifica as porcentagens de limite para tipos específicos de nós durante o processo de upgrade. Definindo regras baseadas em rótulos padrão do Kubernetes e dando uma porcentagem da quantia máxima de nós que são permitidos estar indisponíveis, é possível assegurar que seu app continua funcionando. Um nó é considerado indisponível se ele ainda está para concluir o processo de implementação.

Como as chaves são definidas?

Na seção de informações de dados do mapa de configuração, é possível definir até 10 regras separadas para executar a qualquer momento. Para serem submetidos a upgrade, os nós do trabalhador devem passar por todas as regras definidas.

As chaves estão definidas. E agora?

Depois de definir suas regras, você executa o comando `bx cs worker-update`. Se uma resposta bem-sucedida for retornada, os nós do trabalhador serão enfileirados para serem atualizados. No entanto, os nós não são submetidos ao processo de atualização até que todas as regras estejam satisfeitas. Enquanto são enfileiradas, as regras são verificadas em um intervalo para ver se algum dos nós é capaz de ser atualizado.

E se eu escolhi não definir um mapa de configuração?

Quando o mapa de configuração não está definido, o padrão é usado. Por padrão, um máximo de 20% de todos os nós do trabalhador em cada cluster ficam indisponíveis durante o processo de atualização.

Para atualizar seus nós do trabalhador:

1. Faça quaisquer mudanças que estejam marcadas como _Atualizar após o mestre_ em [Mudanças do Kubernetes](cs_versions.html).

2. Opcional: defina seu mapa de configuração.
    Exemplo:

    ```
    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: ibm-cluster-update-configuration
      namespace: kube-system
    data:
     drain_timeout_seconds: "120"
     zonecheck.json: |
       {
         "MaxUnavailablePercentage": 70,
         "NodeSelectorKey": "failure-domain.beta.kubernetes.io/zone",
         "NodeSelectorValue": "dal13"
       }
     regioncheck.json: |
       {
         "MaxUnavailablePercentage": 80,
         "NodeSelectorKey": "failure-domain.beta.kubernetes.io/region",
         "NodeSelectorValue": "us-south"
       }
     defaultcheck.json: |
       {
         "MaxUnavailablePercentage": 100
       }
    ```
    {:pre}
  <table summary="A primeira linha na tabela abrange ambas as colunas. O resto das linhas deve ser lido da esquerda para a direita, com o parâmetro na coluna um e a descrição que corresponde na coluna dois.">
  <caption>Componentes ConfigMap</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> Entendendo os componentes </th>
    </thead>
    <tbody>
      <tr>
        <td><code>drain_timeout_seconds</code></td>
        <td> Opcional: o tempo limite em segundos do dreno que ocorre durante a atualização do nó do trabalhador. O dreno configura o nó como `unschedulable`, que evita que os novos pods sejam implementados nesse nó. O dreno também exclui os pods do nó. Os valores aceitos são números inteiros de 1 a 180. O valor padrão é 30.</td>
      </tr>
      <tr>
        <td><code>zonecheck.json</code></br><code>regioncheck.json</code></td>
        <td> Exemplos de chaves exclusivas para as quais você deseja configurar regras. Os nomes das chaves pode ser qualquer coisa que você desejar; as informações são analisadas pelas configurações definidas na chave. Para cada chave definida, é possível configurar somente um valor para <code>NodeSelectorKey</code> e <code>NodeSelectorValue</code>. Se desejar configurar regras para mais de uma região ou local (data center), crie uma nova entrada de chave. </td>
      </tr>
      <tr>
        <td><code>defaultcheck.json</code></td>
        <td> Como padrão, se o mapa <code>ibm-cluster-update-configuration</code> não for definido de uma maneira válida, somente 20% de seus clusters poderão ficar indisponíveis de cada vez. Se uma ou mais regras válidas são definidas sem um padrão global, o novo padrão é para permitir que 100% dos trabalhadores fiquem indisponíveis por vez. É possível controlar isso criando uma porcentagem padrão. </td>
      </tr>
      <tr>
        <td><code>MaxUnavailablePercentage</code></td>
        <td> A quantia máxima, especificada em porcentagem, de nós que são permitidos estar indisponíveis para uma chave especificada. Um nó está indisponível quando no processo de implementação, recarregamento ou fornecimento. O upgrade dos nós do trabalhador enfileirados é bloqueado se isso excede quaisquer porcentagens máximas definidas indisponíveis. </td>
      </tr>
      <tr>
        <td><code>NodeSelectorKey</code></td>
        <td> O tipo de rótulo para o qual você deseja configurar uma regra para uma chave especificada. É possível configurar regras para os rótulos padrão fornecidos pela IBM, bem como sobre os rótulos que você criou. </td>
      </tr>
      <tr>
        <td><code>NodeSelectorValue</code></td>
        <td> O subconjunto de nós em uma chave especificada que a regra está configurada para avaliar. </td>
      </tr>
    </tbody>
  </table>

    **Nota**: um máximo de 10 regras podem ser definidas. Se você inclui mais de 10 chaves em um arquivo, somente um subconjunto das informações é analisado.

3. Atualize seus nós do trabalhador na GUI ou executando o comando da CLI.
  * Para atualizar do Painel do {{site.data.keyword.Bluemix_notm}}, navegue para a seção `Nós do trabalhador` de seu cluster e clique em `Atualizar trabalhador`.
  * Para obter IDs de nó do trabalhador, execute `bx cs workers <cluster_name_or_ID>`. Se você seleciona múltiplos nós do trabalhador, os nós do trabalhador são colocados em uma fila para avaliação de atualização. Se eles forem considerados prontos após a avaliação, eles serão atualizados de acordo com as regras definidas nas configurações

    ```
    bx cs worker-update <cluster_name_or_ID> <worker_node1_ID> <worker_node2_ID>
    ```
    {: pre}

4. Opcional: verifique os eventos que são acionados pelo mapa de configuração e quaisquer erros de validação que ocorrem executando o comando a seguir e examinando **Eventos**.
    ```
    kubectl describe -n kube-system cm ibm-cluster-update-configuration
    ```
    {: pre}

5. Confirme se a atualização foi concluída:
  * Revise a versão do Kubernetes no Painel do {{site.data.keyword.Bluemix_notm}} ou execute `bx cs workers <cluster_name_or_ID>`.
  * Revise a versão do Kubernets dos nós do trabalhador executando `kubectl get nodes`.
  * Em alguns casos, clusters mais velhos podem listar nós do trabalhador duplicados com um status de **NotReady** após uma atualização. Para remover duplicatas, consulte [Resolução de problemas](cs_troubleshoot_clusters.html#cs_duplicate_nodes).

Próximas etapas:
  - Repita o processo de atualização com outros clusters.
  - Informe aos desenvolvedores que trabalham no cluster para atualizar sua CLI `kubectl` para a versão do mestre do Kubernetes.
  - Se o painel do Kubernetes não exibir gráficos de utilização, [exclua o pod `kube-dashboard`](cs_troubleshoot_health.html#cs_dashboard_graphs).
  





<br />



## Atualizando tipos de máquina
{: #machine_type}

É possível atualizar os tipos de máquina de seus nós do trabalhador, incluindo novos nós do trabalhador e removendo os antigos. Por exemplo, se você têm nós do trabalhador virtual em tipos de máquina descontinuada com `u1c` ou `b1c` nos nomes, crie nós do trabalhador que usam tipos de máquina com `u2c` ou `b2c` nos nomes.
{: shortdesc}

Antes de iniciar:
- [Destine sua CLI](cs_cli_install.html#cs_cli_configure) para seu cluster.
- Se você armazenar dados em seu nó do trabalhador, os dados serão excluídos se não [armazenados fora do nó do trabalhador](cs_storage.html#storage).


**Atenção**: as atualizações para os nós do trabalhador podem causar tempo de inatividade para seus apps e serviços. Os dados serão excluídos se não [armazenados fora do pod](cs_storage.html#storage).



1. Anote os nomes e locais dos nós do trabalhador para atualizar.
    ```
    bx cs workers <cluster_name>
    ```
    {: pre}

2. Visualize os tipos de máquina disponíveis.
    ```
    bx cs machine-types <location>
    ```
    {: pre}

3. Inclua nós do trabalhador usando o comando [bx cs worker-add](cs_cli_reference.html#cs_worker_add). Especifique um tipo de máquina.

    ```
    bx cs worker-add --cluster <cluster_name> --machine-type <machine_type> --number <number_of_worker_nodes> --private-vlan <private_VLAN_ID> --public-vlan <public_VLAN_ID>
    ```
    {: pre}

4. Verifique se os nós do trabalhador foram incluídos.

    ```
    bx cs workers <cluster_name>
    ```
    {: pre}

5. Quando os nós do trabalhador incluídos estiverem no estado `Normal`, será possível remover o nó do trabalhador desatualizado. **Nota**: se estiver removendo um tipo de máquina que seja faturado mensalmente (como bare metal), você será cobrado pelo mês inteiro.

    ```
    bx cs worker-rm <cluster_name> <worker_node>
    ```
    {: pre}

6. Repita essas etapas para atualizar outros nós do trabalhador para tipos de máquina diferentes.










