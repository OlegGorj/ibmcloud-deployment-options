---

copyright:
  years: 2014, 2018
lastupdated: "2018-4-20"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Acessando o portfólio de Infraestrutura do IBM Cloud (SoftLayer)
{: #infrastructure}

Para criar um cluster do Kubernetes padrão, deve-se ter acesso ao portfólio de infraestrutura do IBM Cloud (SoftLayer). Esse acesso é necessário para solicitar recursos de infraestrutura pagos, como nós do trabalhador, endereços IP públicos móveis ou armazenamento persistente para seu cluster do Kubernetes no {{site.data.keyword.containerlong}}.
{:shortdesc}

## Acessar o portfólio de infraestrutura do IBM Cloud (SoftLayer)
{: #unify_accounts}

As contas Pay-As-You-Go do {{site.data.keyword.Bluemix_notm}} que foram criadas após a vinculação de conta automática ter sido ativada já estão configuradas com acesso ao portfólio da infraestrutura do IBM Cloud (SoftLayer). É possível comprar recursos de infraestrutura para seu cluster sem configuração adicional.
{:shortdesc}

Os usuários com outros tipos de contas do {{site.data.keyword.Bluemix_notm}} ou usuários que possuem uma conta de infraestrutura do IBM Cloud existente (SoftLayer) que não está vinculada a sua conta do {{site.data.keyword.Bluemix_notm}} devem configurar suas contas para criar clusters padrão.
{:shortdesc}

Revise a tabela a seguir para localizar opções disponíveis para cada tipo de conta.

|Tipo de conta|Descrição|Opções disponíveis para criar um cluster padrão|
|------------|-----------|----------------------------------------------|
|Contas Lite|Contas Lite não podem provisionar clusters.|[Faça upgrade de sua conta Lite para uma {{site.data.keyword.Bluemix_notm}}conta Pay-As-You-Go](/docs/account/index.html#billableacts) que está configurada com acesso ao portfólio de infraestrutura do IBM Cloud (SoftLayer).|
|Contas Pay-As-You-Go|As contas Pay-As-You-Go que foram criadas antes de a vinculação de conta automática estar disponível não vieram com acesso ao portfólio de infraestrutura do IBM Cloud (SoftLayer).<p>Se você tiver uma conta de infraestrutura do IBM Cloud existente (SoftLayer), não será possível vincular essa conta a uma conta Pay-As-You-Go mais antiga.</p>|Opção 1: [Criar uma nova conta de Pay-As-You-Go](/docs/account/index.html#billableacts) que esteja configurada com acesso ao portfólio da infraestrutura do IBM Cloud (SoftLayer). Ao escolher essa opção,
você tem duas contas e faturamentos separados do
{{site.data.keyword.Bluemix_notm}}.<p>Se você desejar continuar usando a sua antiga conta Pay-As-You-Go para criar clusters padrão, será possível usar sua nova conta Pay-As-You-Go para gerar uma chave API para acessar o portfólio de infraestrutura do IBM Cloud (SoftLayer). Em seguida, deve-se configurar a chave API
para sua conta Pay-As-You-Go antiga. Para obter mais informações, veja [Gerando
uma chave API para contas Pay-As-You-Go e de Assinatura antigas](#old_account). Mantenha em mente que os recursos de infraestrutura do IBM Cloud (SoftLayer) são cobrados através de sua nova conta Pay-As-You-Go.</p></br><p>Opção 2: se você já tiver uma conta de infraestrutura do IBM Cloud existente (SoftLayer) que você desejar usar, será possível [configurar as suas credenciais](cs_cli_reference.html#cs_credentials_set) para a sua conta do {{site.data.keyword.Bluemix_notm}}.</p><p>Nota: a conta de infraestrutura do IBM Cloud (SoftLayer) que você usar com a sua conta do {{site.data.keyword.Bluemix_notm}} deverá ser configurada com permissões de Superusuário.</p>|
|Contas de assinatura|As contas de assinatura não são configuradas com acesso ao portfólio de infraestrutura do IBM Cloud (SoftLayer).|Opção 1: [Criar uma nova conta de Pay-As-You-Go](/docs/account/index.html#billableacts) que esteja configurada com acesso ao portfólio da infraestrutura do IBM Cloud (SoftLayer). Ao escolher essa opção,
você tem duas contas e faturamentos separados do
{{site.data.keyword.Bluemix_notm}}.<p>Se você desejar continuar usando a sua conta de Assinatura para criar clusters padrão, será possível usar a sua nova conta Pay-As-You-Go para gerar uma chave API para acessar o portfólio de infraestrutura do IBM Cloud (SoftLayer). Em seguida, deve-se configurar a chave API
para sua conta de Assinatura. Para obter mais informações, veja [Gerando
uma chave API para contas Pay-As-You-Go e de Assinatura antigas](#old_account). Mantenha em mente que os recursos de infraestrutura do IBM Cloud (SoftLayer) são cobrados através de sua nova conta Pay-As-You-Go.</p></br><p>Opção 2: se você já tiver uma conta de infraestrutura do IBM Cloud existente (SoftLayer) que você desejar usar, será possível [configurar as suas credenciais](cs_cli_reference.html#cs_credentials_set) para a sua conta do {{site.data.keyword.Bluemix_notm}}.<p>Nota: a conta de infraestrutura do IBM Cloud (SoftLayer) que você usar com a sua conta do {{site.data.keyword.Bluemix_notm}} deverá ser configurada com permissões de Superusuário.</p>|
|Contas de infraestrutura do IBM Cloud (SoftLayer), nenhuma conta do {{site.data.keyword.Bluemix_notm}}|Para criar um cluster padrão, deve-se ter uma conta do {{site.data.keyword.Bluemix_notm}}.|<p>[Criar uma nova conta Pay-As-You-Go](/docs/account/index.html#billableacts) que esteja configurada com acesso ao portfólio de infraestrutura do IBM Cloud (SoftLayer). Ao escolher essa opção, uma infraestrutura IBM Cloud (SoftLayer) será criada para você. Você tem duas contas de infraestrutura do IBM Cloud (SoftLayer) separadas e faturamento.</p>|

<br />


## Gerando uma chave API da infraestrutura do IBM Cloud (SoftLayer) para usar com contas do {{site.data.keyword.Bluemix_notm}}
{: #old_account}

Para continuar usando a sua conta Pay-As-You-Go ou de Assinatura antiga para criar clusters padrão, gerar uma chave API com sua nova conta Pay-As-You-Go e configure a chave API para sua conta antiga.
{:shortdesc}

Antes de iniciar, crie uma conta {{site.data.keyword.Bluemix_notm}} Pay-As-You-Go que seja configurada automaticamente com acesso ao portfólio da infraestrutura do IBM Cloud (SoftLayer).

1.  Efetue login no [portal de infraestrutura do IBM Cloud (SoftLayer)![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/) usando o {{site.data.keyword.ibmid}} e a senha que você criou para a sua nova conta Pay-As-You-Go.
2.  Selecione **Conta** e, em seguida, **Usuários**.
3.  Clique em **Gerar** para gerar uma chave API de infraestrutura do IBM Cloud (SoftLayer) para sua nova conta Pay-As-You-Go.
4.  Copie a chave API.
5.  Na CLI, efetue login no {{site.data.keyword.Bluemix_notm}} usando o {{site.data.keyword.ibmid}} e a senha de sua conta de Pagamento por uso ou de Assinatura antiga.

  ```
  bx login
  ```
  {: pre}

6.  Configure a chave API gerada anteriormente para acessar o portfólio de infraestrutura do IBM Cloud (SoftLayer). Substitua `<API_key>` pela chave API e `<username>` pelo {{site.data.keyword.ibmid}} de sua nova conta de Pagamento por uso.

  ```
  bx cs credentials-set --infrastructure-api-key <API_key> --infrastructure-username <username>
  ```
  {: pre}

7.  Inicie [criando clusters padrão](cs_clusters.html#clusters_cli).

**Nota:** para revisar sua chave API depois de ela ter sido gerada, siga as etapas 1 e 2 e, em seguida, na seção **Chave API**, clique em **Visualizar** para ver a chave API de seu ID do usuário.

