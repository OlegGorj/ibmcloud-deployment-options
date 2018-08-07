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



# Designando acesso ao cluster
{: #users}

Como um administrador de cluster, é possível definir políticas de acesso para seu cluster do Kubernetes para criar diferentes níveis de acesso para diferentes usuários. Por exemplo, é possível autorizar determinados usuários para trabalhar com recursos de cluster enquanto outros podem somente implementar contêineres.
{: shortdesc}


## Planejando solicitações de acesso
{: #planning_access}

Como um administrador de cluster, pode ser difícil rastrear solicitações de acesso. Estabelecer um padrão de comunicação para solicitações de acesso é essencial para manter a segurança de seu cluster.
{: shortdesc}

Para assegurar que as pessoas certas tenham o acesso correto, seja claro sobre suas políticas para solicitar acesso ou obter ajuda com tarefas comuns.

Você já pode ter um método que funcione para sua equipe e isso é ótimo! Se você estiver procurando um local para começar, considere tentar um dos métodos a seguir.

*  Criar um sistema de chamado
*  Criar um modelo de formulário
*  Crie uma página da wiki
*  Requerer uma solicitação de e-mail
*  Usar o sistema de rastreamento de problemas que você já usa para controlar o trabalho diário de sua equipe

Sente-se sobrecarregado? Experimente este tutorial sobre as [melhores práticas para organizar usuários, equipes e aplicativos](/docs/tutorials/users-teams-applications.html).
{: tip}

## Políticas de acesso e permissões
{: #access_policies}

O escopo de uma política de acesso baseia-se em uma função ou funções definidas pelos usuários que determinam as ações que eles têm permissão para executar. É possível configurar políticas específicas para seu cluster, sua infraestrutura, instâncias do serviço ou funções do Cloud Foundry.
{: shortdesc}

Deve-se definir políticas de acesso para cada usuário que trabalhe com o {{site.data.keyword.containershort_notm}}. Algumas políticas são predefinidas, mas outras podem ser customizadas. Verifique a imagem e as definições a seguir para ver quais funções se alinham com tarefas comuns do usuário e identificar locais nos quais você possa querer customizar uma política.

Funções de acesso do ![{{site.data.keyword.containershort_notm}}](/images/user-policies.png)

Figura. Funções de acesso do {{site.data.keyword.containershort_notm}}

<dl>
  <dt>Políticas do Identity and Access Management (IAM)</dt>
    <dd><p><strong>Plataforma</strong>: é possível determinar as ações que os indivíduos podem executar em um cluster do {{site.data.keyword.containershort_notm}}. É possível configurar essas políticas por região. Exemplo de ações que estão criando ou removendo clusters ou incluindo nós extras do trabalhador. Essas políticas devem ser configuradas juntamente com as políticas de infraestrutura.</p>
    <p><strong>Infraestrutura</strong>: é possível determinar os níveis de acesso para sua infraestrutura, como as máquinas do nó do cluster, rede ou recursos de armazenamento. A mesma política é cumprida se o usuário faz a solicitação na GUI do {{site.data.keyword.containershort_notm}} ou por meio da CLI, mesmo quando as ações são concluídas na infraestrutura do IBM Cloud (SoftLayer). Deve-se configurar esse tipo de política junto com as políticas de acesso de plataforma do {{site.data.keyword.containershort_notm}}. Para conhecer as funções disponíveis, verifique as [permissões de infraestrutura](/docs/iam/infrastructureaccess.html#infrapermission).</p> </br></br><strong>Nota:</strong> certifique-se de que sua conta do {{site.data.keyword.Bluemix_notm}} esteja [configurada com acesso ao portfólio de infraestrutura do IBM Cloud (SoftLayer)](cs_troubleshoot_clusters.html#cs_credentials) para que os usuários autorizados possam executar ações na conta de infraestrutura do IBM Cloud (SoftLayer) com base nas permissões designadas. </dd>
  <dt>Funções do Resource Based Access Control (RBAC) do Kubernetes</dt>
    <dd>Cada usuário a quem é designada uma política de acesso de plataforma é designado automaticamente a uma função do Kubernetes. No Kubernetes, o [Role Based Access Control (RBAC) ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview) determina as ações que um usuário pode executar nos recursos dentro de um cluster. <strong>Nota</strong>: as funções RBAC são configuradas automaticamente para o namespace <code>default</code>, mas como o administrador de cluster, é possível designar funções para outros namespaces.</dd>
  <dt>Cloud Foundry</dt>
    <dd>Nem todos os serviços podem ser gerenciados com o Cloud IAM. Se você estiver usando um desses serviços, será possível continuar usando as [funções de usuário do Cloud Foundry](/docs/iam/cfaccess.html#cfaccess) para controlar o acesso a seus serviços.</dd>
</dl>


Reduzindo permissões? Pode levar alguns minutos para a ação ser concluída.
{: tip}

### Funções da plataforma
{: #platform_roles}

{{site.data.keyword.containershort_notm}} é configurado para usar o {{site.data.keyword.Bluemix_notm}} plataforma funções. As permissões de função são construídas umas sobre as outras, o que significa que a função `Editor` possui as mesmas permissões que a função `Viewer`, além das permissões concedidas a um editor. A tabela a seguir explica os tipos de ações que cada função pode executar.

As funções RBAC correspondentes são designadas automaticamente ao namespace padrão quando você designa uma função de plataforma. Se você muda a função de plataforma de um usuário, a função RBAC também é atualizada.
{: tip}

<table>
<caption>Funções e ações de plataforma</caption>
  <tr>
    <th>Funções da plataforma</th>
    <th>Exemplo de ações</th>
    <th>Função RBAC correspondente</th>
  </tr>
  <tr>
      <td>Viewer</td>
      <td>Visualiza os detalhes para um cluster ou outras instâncias de serviço.</td>
      <td>Visualizar</td>
  </tr>
  <tr>
    <td>Aplicativos</td>
    <td>Pode ligar ou desvincular um serviço IBM Cloud de um cluster ou criar um webhook. <strong>Nota</strong>: para ligar serviços, deve-se também estar designado à função de desenvolvedor do Cloud Foundry.</td>
    <td>Editar</td>
  </tr>
  <tr>
    <td>Operador</td>
    <td>Pode criar, remover, reinicializar ou recarregar um nó do trabalhador. Pode incluir uma sub-rede em um cluster.</td>
    <td>Administrador</td>
  </tr>
  <tr>
    <td>Administrator</td>
    <td>Pode criar e remover clusters. Pode editar políticas de acesso para outros no nível de conta para o serviço e a infraestrutura. <strong>Nota</strong>: o acesso de administrador pode ser designado a um cluster específico ou a todas as instâncias do serviço em sua conta. Para excluir clusters, deve-se ter acesso de administrador para o cluster que você deseja excluir. Para criar clusters, deve-se ter a função administrativa para todas as instâncias do serviço.</td>
    <td>Administrador de cluster</td>
  </tr>
</table>

Para obter mais informações sobre como designar funções de usuário na UI, veja [Gerenciando o acesso ao IAM](/docs/iam/mngiam.html#iammanidaccser).


### Funções de infraestrutura
{: #infrastructure_roles}

As funções de infraestrutura permitem que os usuários executem tarefas em recursos no nível da infraestrutura. A tabela a seguir explica os tipos de ações que cada função pode executar. As funções de infraestrutura são customizáveis; certifique-se de dar aos usuários somente o acesso que eles precisam para fazer seu trabalho.

Além de conceder funções de infraestrutura específicas, deve-se também conceder acesso ao dispositivo para usuários que trabalham com infraestrutura.
{: tip}

<table>
<caption>Funções e ações de infraestrutura</caption>
  <tr>
    <th>Função de infraestrutura</th>
    <th>Exemplo de ações</th>
  </tr>
  <tr>
    <td><i>Apenas Visualizar</i></td>
    <td>Pode visualizar os detalhes de infraestrutura e ver um resumo da conta, incluindo faturas e pagamentos</td>
  </tr>
  <tr>
    <td><i>Usuário básico</i></td>
    <td>Pode editar configurações de serviço, incluindo endereços IP, incluir ou editar registros DNS e incluir novos usuários com acesso à infraestrutura</td>
  </tr>
  <tr>
    <td><i>Superusuário</i></td>
    <td>Pode executar todas as ações que estão relacionadas à infraestrutura</td>
  </tr>
</table>

Para começar a designar funções, siga as etapas em [Customizando permissões de infraestrutura para um usuário](#infra_access).

### Funções do RBAC
{: #rbac_roles}

O Resource-based access control (RBAC) é uma maneira de proteger os recursos que estão dentro do cluster e decidir quem pode executar quais ações do Kubernetes. Na tabela a seguir, é possível ver os tipos de funções RBAC e os tipos de ações que os usuários podem executar com essa função. As permissões são construídas umas sobre as outras, o que significa que um `Admin` também tem todas as políticas que vêm com as funções `View` e `Edit`. Certifique-se de dar aos usuários apenas o acesso que eles precisam.

As funções RBAC são configuradas automaticamente em conjunto com a função de plataforma para o namespace padrão. [É possível atualizar a função ou designar funções para outros namespaces](#rbac).
{: tip}

<table>
<caption>Funções e ações RBAC</caption>
  <tr>
    <th>Função RBAC</th>
    <th>Exemplo de ações</th>
  </tr>
  <tr>
    <td>Visualizar</td>
    <td>Pode visualizar recursos dentro do namespace padrão. Os visualizadores não podem visualizar segredos do Kubernetes. </td>
  </tr>
  <tr>
    <td>Editar</td>
    <td>Pode ler e gravar recursos dentro do namespace padrão.</td>
  </tr>
  <tr>
    <td>Administrador</td>
    <td>Pode ler e gravar recursos dentro do namespace padrão, mas não no próprio namespace. Pode criar funções dentro de um namespace.</td>
  </tr>
  <tr>
    <td>Administrador de cluster</td>
    <td>Pode ler e gravar recursos em cada namespace. Pode criar funções dentro de um namespace. Pode acessar o painel do Kubernetes. Pode criar um recurso do Ingress que disponibiliza publicamente os apps.</td>
  </tr>
</table>

<br />


## Incluindo usuários em uma conta do {{site.data.keyword.Bluemix_notm}}
{: #add_users}

É possível incluir usuários em uma conta do {{site.data.keyword.Bluemix_notm}} para conceder acesso aos seus clusters.
{:shortdesc}

Antes de iniciar, verifique se você está designado à função `Manager` do Cloud Foundry para uma conta do {{site.data.keyword.Bluemix_notm}}.

1.  [Inclua o usuário na conta](../iam/iamuserinv.html#iamuserinv).
2.  Na seção **Acesso**, expanda **Serviços**.
3.  Designe uma função de plataforma a um usuário para configurar o acesso para o {{site.data.keyword.containershort_notm}}.
      1. Na lista suspensa **Serviços**, selecione **{{site.data.keyword.containershort_notm}}**.
      2. Na lista suspensa **Região**, selecione a região para a qual convidar o usuário.
      3. Na lista suspensa **Instância de serviço**, selecione o cluster para o qual convidar o usuário. Para localizar o ID de um cluster específico, execute `bx cs clusters`.
      4. Na seção **Selecionar funções**, escolha uma função. Para localizar uma lista de ações suportadas por função, consulte [Políticas e permissões de
acesso](#access_policies).
4. [Opcional: designe uma função do Cloud Foundry](/docs/iam/mngcf.html#mngcf).
5. [Opcional: designe uma função de infraestrutura](/docs/iam/infrastructureaccess.html#infrapermission).
6. Clique em **Convidar usuários**.

<br />


## Entendendo a chave API do IAM e o comando `bx cs credentials-set`
{: #api_key}

Para provisionar e trabalhar com êxito com clusters em sua conta, deve-se assegurar que sua conta esteja configurada corretamente para acessar o portfólio de infraestrutura do IBM Cloud (SoftLayer). Dependendo de sua configuração de conta, use a chave API do IAM ou as credenciais de infraestrutura que você configurou manualmente usando o comando `bx cs credentials-set`.

<dl>
  <dt>Chave API do IAM</dt>
    <dd><p>A chave API do Identity and Access Management (IAM) é configurada automaticamente para uma região quando a primeira ação que requer a política de acesso de administrador do {{site.data.keyword.containershort_notm}} é executada. Por exemplo, um dos seus usuários administradores cria o primeiro cluster na região <code>us-south</code>. Ao fazer isso, a chave API do IAM para esse usuário é armazenada na conta para essa região. A chave API é usada para pedir a infraestrutura do IBM Cloud (SoftLayer), como novos nós do trabalhador ou VLANs.</p> <p>Quando um usuário diferente executa uma ação nessa região que requer interação com o portfólio de infraestrutura do IBM Cloud (SoftLayer), como a criação de um novo cluster ou o recarregamento de um nó do trabalhador, a chave API armazenada é usada para determinar se existem permissões suficientes para executar essa ação. Para certificar-se de que as ações relacionadas à infraestrutura em seu cluster possam ser executadas com sucesso, designe a seus usuários administradores do {{site.data.keyword.containershort_notm}} a política de acesso de infraestrutura <strong>Superusuário</strong>.</p>
    <p>É possível localizar o proprietário da chave API atual executando [<code>bx cs api-key-info</code>](cs_cli_reference.html#cs_api_key_info). Se você acha que precisa atualizar a chave API armazenada para uma região, é possível fazer isso executando o comando [<code>bx cs api-key-reset</code>](cs_cli_reference.html#cs_api_key_reset). Esse comando requer a política de acesso de administrador do {{site.data.keyword.containershort_notm}} e armazena a chave API do usuário que executa esse comando na conta.</p>
    <p><strong>Nota:</strong> a chave API armazenada para a região pode não ser usada se as credenciais de infraestrutura do IBM Cloud (SoftLayer) foram configuradas manualmente usando o comando <code>bx cs credentials-set</code>.</p></dd>
  <dt>Credenciais do IBM Cloud (SoftLayer) via <code>bx cs credentials-set</code></dt>
    <dd><p>Se você tiver uma {{site.data.keyword.Bluemix_notm}}conta pré-paga, terá acesso ao portfólio de infraestrutura do IBM Cloud (SoftLayer) por padrão. No entanto, talvez você queira usar uma conta de infraestrutura do IBM Cloud (SoftLayer) diferente da que você já tem para pedir a infraestrutura. É possível vincular essa conta de infraestrutura à sua conta do {{site.data.keyword.Bluemix_notm}} usando o comando [<code>bx cs credentials-set</code>](cs_cli_reference.html#cs_credentials_set).</p>
    <p>Se as credenciais de infraestrutura do IBM Cloud (SoftLayer) são configuradas manualmente, essas credenciais são usadas para pedir infraestrutura, mesmo se uma chave API do IAM existe para a conta. Se o usuário cujas credenciais estão armazenadas não tiver as permissões necessárias para pedir a infraestrutura, as ações relacionadas à infraestrutura, como a criação de um cluster ou o recarregamento de um nó do trabalhador, poderão falhar.</p>
    <p>Para remover as credenciais de infraestrutura do IBM Cloud (SoftLayer) que foram configuradas manualmente, é possível usar o comando [<code>bx cs credentials-unset</code>](cs_cli_reference.html#cs_credentials_unset). Após as credenciais serem removidas, a chave API do IAM é usada para pedir infraestrutura.</p></dd>
</dl>

<br />


## Customizando as permissões de infraestrutura para um usuário
{: #infra_access}

Quando você configura políticas de infraestrutura no Identity and Access Management, um usuário recebe permissões que estão associadas a uma função. Para customizar essas permissões, deve-se efetuar login na infraestrutura do IBM Cloud (SoftLayer) e ajustar as permissões lá.
{: #view_access}

Por exemplo, **Usuários básicos** podem reinicializar um nó do trabalhador, mas não podem recarregar um nó do trabalhador. Sem fornecer a essa pessoa as permissões de **Superusuário**, é possível ajustar as permissões de infraestrutura do IBM Cloud (SoftLayer) e incluir a permissão para executar um comando de recarregamento.

1.  Efetue login em sua [conta do {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/), em seguida, no menu, selecione **Infraestrutura**.

2.  Acesse **Conta** > **Usuários** > **Lista de usuários**.

3.  Para modificar permissões, selecione o nome de um perfil do usuário ou a coluna **Acesso ao dispositivo**.

4.  Na guia **Permissões do portal**, customize o acesso de usuário. As permissões que os usuários precisam dependem de quais recursos de infraestrutura eles precisam usar:

    * Use a lista suspensa **Permissões rápidas** para designar a função **Superusuário**, que fornece ao usuário todas as permissões.
    * Use a lista suspensa **Permissões rápidas** para designar a função **Usuário básico**, que fornece ao usuário algumas, mas não todas, permissões necessárias.
    * Se você não desejar conceder todas as permissões com a função **Superusuário** ou precisar incluir permissões além da função **Usuário básico**, revise a tabela a seguir que descreve as permissões necessárias para executar tarefas comuns no {{site.data.keyword.containershort_notm}}.

    <table summary="Permissões de infraestrutura para cenários comuns do {{site.data.keyword.containershort_notm}}.">
     <caption>Permissões de infraestrutura normalmente necessárias para o {{site.data.keyword.containershort_notm}}</caption>
     <thead>
      <th>Tarefas comuns no {{site.data.keyword.containershort_notm}}</th>
      <th>Permissões de infraestrutura necessárias por guia</th>
     </thead>
     <tbody>
       <tr>
         <td><strong>Permissões mínimas</strong>: <ul><li>Crie um cluster.</li></ul></td>
         <td><strong>Dispositivos</strong>:<ul><li>Visualize os detalhes do Virtual Server</li><li>Reinicializar o servidor e visualizar informações do sistema IPMI</li><li>Emitir recarregamentos de OS e iniciar o kernel de resgate</li></ul><strong>Conta</strong>: <ul><li>Incluir/fazer upgrade de instâncias da nuvem</li><li>Incluir servidor</li></ul></td>
       </tr>
       <tr>
         <td><strong>Administração de cluster</strong>: <ul><li>Criar, atualizar e excluir clusters.</li><li>Incluir, recarregar e reinicializar nós do trabalhador.</li><li>Visualizar VLANs.</li><li>Criar sub-redes.</li><li>Implementar pods e serviços do balanceador de carga.</li></ul></td>
         <td><strong>Suporte</strong>:<ul><li>Visualizar chamados</li><li>Incluir chamados</li><li>Editar chamados</li></ul>
         <strong>Dispositivos</strong>:<ul><li>Visualize os detalhes do Virtual Server</li><li>Reinicializar o servidor e visualizar informações do sistema IPMI</li><li>Fazer upgrade do servidor</li><li>Emitir recarregamentos de OS e iniciar o kernel de resgate</li></ul>
         <strong>Serviços</strong>:<ul><li>Gerenciar chaves SSH</li></ul>
         <strong>Conta</strong>:<ul><li>Visualizar resumo da conta</li><li>Incluir/fazer upgrade de instâncias da nuvem</li><li>Cancelar servidor</li><li>Incluir servidor</li></ul></td>
       </tr>
       <tr>
         <td><strong>Armazenamento</strong>: <ul><li>Criar solicitações de volume persistente para provisionar volumes persistentes.</li><li>Criar e gerenciar recursos de infraestrutura de armazenamento.</li></ul></td>
         <td><strong>Serviços</strong>:<ul><li>Gerenciar armazenamento</li></ul><strong>Conta</strong>:<ul><li>Incluir armazenamento</li></ul></td>
       </tr>
       <tr>
         <td><strong>Rede privada</strong>: <ul><li>Gerenciar VLANs privadas para rede em cluster.</li><li>Configurar a conectividade VPN para redes privadas.</li></ul></td>
         <td><strong>Rede</strong>:<ul><li>Gerenciar rotas de sub-rede da rede</li><li>Gerenciar túneis de rede do IPSEC</li><li>Gerenciar gateways de rede</li><li>Administração da VPN</li></ul></td>
       </tr>
       <tr>
         <td><strong>Rede pública</strong>:<ul><li>Configurar o balanceador de carga pública ou rede de Ingresso para expor apps.</li></ul></td>
         <td><strong>Dispositivos</strong>:<ul><li>Gerenciar Load Balancers</li><li>Editar nome do host/domínio</li><li>Gerenciar controle de porta</li></ul>
         <strong>Rede</strong>:<ul><li>Incluir cálculo com porta de rede pública</li><li>Gerenciar rotas de sub-rede da rede</li><li>Incluir endereços IP</li></ul>
         <strong>Serviços</strong>:<ul><li>Gerenciar DNS, DNS reverso e WHOIS</li><li>Visualizar certificados (SSL)</li><li>Gerenciar certificados (SSL)</li></ul></td>
       </tr>
     </tbody>
    </table>

5.  Para salvar as suas mudanças, clique em **Editar permissões do portal**.

6.  Na guia **Acesso ao dispositivo**, selecione os dispositivos aos quais conceder acesso.

    * Na lista suspensa **Tipo de dispositivo**, é possível conceder acesso para **Todos os Virtual Servers**.
    * Para permitir que os usuários acessem novos dispositivos que são criados, selecione **Conceder acesso automaticamente quando novos dispositivos forem incluídos**.
    * Para salvar suas mudanças, clique em **Atualizar acesso ao dispositivo**.

<br />


## Autorizando usuários com funções RBAC customizadas do Kubernetes
{: #rbac}

As políticas de acesso do {{site.data.keyword.containershort_notm}} correspondem a determinadas funções de controle de acesso baseado na função (RBAC) do Kubernetes conforme descrito em [Políticas e permissões de acesso](#access_policies). Para autorizar outras funções do Kubernetes que diferem da política de acesso correspondente, é possível customizar as funções RBAC e, em seguida, designar as funções para indivíduos ou grupos de usuários.
{: shortdesc}

Por exemplo, você pode desejar conceder permissões a uma equipe de desenvolvedores para trabalhar em um determinado grupo de APIs ou com recursos dentro de um namespace do Kubernetes no cluster, mas não no cluster inteiro. Você cria uma função e depois liga a função aos usuários usando um nome do usuário que é exclusivo para o {{site.data.keyword.containershort_notm}}. Para obter mais informações, veja [Usando autorização RBAC ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview) na documentação do Kubernetes.

Antes de iniciar, [destine a CLI do Kubernetes para o cluster](cs_cli_install.html#cs_cli_configure).

1.  Crie a função com o acesso que você deseja designar.

    1. Faça um arquivo `.yaml` para definir a função com o acesso que você deseja designar.

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
        <caption>Entendendo os componentes deste YAML</caption>
          <thead>
            <th colspan=2><img src="images/idea.png" alt="Ícone Ideia"/> Entendendo os componentes deste YAML</th>
          </thead>
          <tbody>
            <tr>
              <td><code>kind</code></td>
              <td>Use `Função` para conceder acesso a recursos em um único namespace ou `ClusterRole` para recursos de cluster inteiro.</td>
            </tr>
            <tr>
              <td><code>apiVersion</code></td>
              <td><ul><li>Para clusters que executam o Kubernetes 1.8 ou mais recente, use `rbac.authorization.k8s.io/v1`. </li><li>Para versões anteriores, use `apiVersion: rbac.authorization.k8s.io/v1beta1`.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/namespace</code></td>
              <td><ul><li>Para o tipo `Role`: especifique o namespace do Kubernetes ao qual o acesso é concedido.</li><li>Não use o campo `namespace` se estiver criando um `ClusterRole` que se aplique ao nível do cluster.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/name</code></td>
              <td>Nomeie a função e use o nome posteriormente quando você ligar a função.</td>
            </tr>
            <tr>
              <td><code>rules/apiGroups</code></td>
              <td><ul><li>Especifique os grupos da API do Kubernetes com os quais você deseja que os usuários sejam capazes de interagir, como `"apps"`, `"batch"` ou `"extensions"`. </li><li>Para acesso ao grupo principal de APIs no caminho de REST `api/v1`, deixe o grupo em branco: `[""]`.</li><li>Para obter mais informações, veja [Grupos de APIs![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://v1-9.docs.kubernetes.io/docs/reference/api-overview/#api-groups) na documentação do Kubernetes.</li></ul></td>
            </tr>
            <tr>
              <td><code>rules/resources</code></td>
              <td><ul><li>Especifique os recursos do Kubernetes aos quais você deseja conceder acesso, como `"daemonsets"`, `"deployments"`, `"events"` ou `"ingresses"`.</li><li>Se você especificar `"nodes"`, o tipo de função deverá ser `ClusterRole`.</li><li>Para obter uma lista de recursos, veja a tabela de [Tipos de recursos![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/reference/kubectl/cheatsheet/) na folha de dicas do Kubernetes.</li></ul></td>
            </tr>
            <tr>
              <td><code>rules/verbs</code></td>
              <td><ul><li>Especifique os tipos de ações que você deseja que os usuários sejam capazes de executar, como `"get"`, `"list"`, `"describe"`, `"create"` ou `"delete"`. </li><li>Para obter uma lista integral de verbos, veja a [documentação do `kubectl`![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/reference/kubectl/overview/).</li></ul></td>
            </tr>
          </tbody>
        </table>

    2.  Crie a função em seu cluster.

        ```
        kubectl apply -f <path_to_yaml_file>
        ```
        {: pre}

    3.  Verifique se a função foi criada.

        ```
        kubectl get roles
        ```
        {: pre}

2.  Ligue os usuários à função.

    1. Faça um arquivo `.yaml` para ligar os usuários à sua função. Anote a URL exclusiva a ser usada para o nome de cada assunto.

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
        <caption>Entendendo os componentes deste YAML</caption>
          <thead>
            <th colspan=2><img src="images/idea.png" alt="Ícone Ideia"/> Entendendo os componentes deste YAML</th>
          </thead>
          <tbody>
            <tr>
              <td><code>kind</code></td>
              <td>Especifique o `kind` como `RoleBinding` para ambos os tipos de arquivos `.yaml` de função: namespace `Role` e `ClusterRole` de cluster inteiro.</td>
            </tr>
            <tr>
              <td><code>apiVersion</code></td>
              <td><ul><li>Para clusters que executam o Kubernetes 1.8 ou mais recente, use `rbac.authorization.k8s.io/v1`. </li><li>Para versões anteriores, use `apiVersion: rbac.authorization.k8s.io/v1beta1`.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/namespace</code></td>
              <td><ul><li>Para o tipo `Role`: especifique o namespace do Kubernetes ao qual o acesso é concedido.</li><li>Não use o campo `namespace` se estiver criando um `ClusterRole` que se aplique ao nível do cluster.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/name</code></td>
              <td>Nomeie a ligação de função.</td>
            </tr>
            <tr>
              <td><code>subjects/kind</code></td>
              <td>Especifique o tipo como `User`.</td>
            </tr>
            <tr>
              <td><code>subjects/name</code></td>
              <td><ul><li>Anexe o endereço de e-mail do usuário à URL a seguir: `https://iam.ng.bluemix.net/kubernetes#`.</li><li>Por exemplo, `https://iam.ng.bluemix.net/kubernetes#user1@example.com`</li></ul></td>
            </tr>
            <tr>
              <td><code>subjects/apiGroup</code></td>
              <td>Use `rbac.authorization.k8s.io`.</td>
            </tr>
            <tr>
              <td><code>roleRef/kind</code></td>
              <td>Insira o mesmo valor que o `kind` no arquivo `.yaml` de função: `Role` ou `ClusterRole`.</td>
            </tr>
            <tr>
              <td><code>roleRef/name</code></td>
              <td>Insira o nome do arquivo `.yaml` de função.</td>
            </tr>
            <tr>
              <td><code>roleRef/apiGroup</code></td>
              <td>Use `rbac.authorization.k8s.io`.</td>
            </tr>
          </tbody>
        </table>

    2. Crie o recurso de ligação de função em seu cluster.

        ```
        Kubectl apply -f filepath/my_role_team1.yaml
        ```
        {: pre}

    3.  Verifique se a ligação foi criada.

        ```
        kubectl get rolebinding
        ```
        {: pre}

Agora que você criou e ligou uma função RBAC customizada do Kubernetes, acompanhe com usuários. Peça-lhes para testar uma ação que eles tenham permissão para concluir devido à função, como excluir um pod.
