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



# Suas responsabilidades usando {{site.data.keyword.containerlong_notm}}
Saiba mais sobre as responsabilidades de gerenciamento de cluster e termos e condições que você tem ao usar o {{site.data.keyword.containerlong}}.
{:shortdesc}

## Responsabilidades de gerenciamento de cluster
{: #responsibilities}

Revise as responsabilidades que você compartilha com a IBM para gerenciar seus clusters.
{:shortdesc}

**A IBM é responsável por:**

- Implementar o mestre, nós do trabalhador e componentes de gerenciamento dentro do cluster, como balanceador de carga de aplicativo de Ingresso, no tempo de criação do cluster
- Gerenciando as atualizações de segurança, monitoramento, isolamento e recuperação do mestre do Kubernetes para o cluster
- Monitorar o funcionamento dos nós do trabalhador e fornecer automação para a atualização e a recuperação dos nós do trabalhador
- Executar tarefas de automação em sua conta de infraestrutura, incluindo adicionar nós do trabalhador, remover nós do trabalhador e criar uma sub-rede padrão
- Gerenciar, atualizar e recuperar componentes operacionais dentro do cluster, tais como o balanceador de carga de aplicativo de Ingresso e o plug-in de armazenamento
- Provisionar volumes de armazenamento quando solicitado pelas solicitações de volume persistente
- Fornecer configurações de segurança em todos os nós do trabalhador

</br>

**Você é responsável por:**

- [Configurando sua conta do {{site.data.keyword.Bluemix_notm}} para acessar o portfólio de infraestrutura do IBM Cloud (SoftLayer)](cs_troubleshoot_clusters.html#cs_credentials)
- [Implementar e gerenciar recursos do Kubernetes, como pods, serviços e implementações no cluster](cs_app.html#app_cli)
- [Usar os recursos do serviço e o Kubernetes para assegurar a alta disponibilidade de apps](cs_app.html#highly_available_apps)
- [Incluindo ou removendo a capacidade do cluster usando a CLI para incluir ou remover nós do trabalhador](cs_cli_reference.html#cs_worker_add)
- [Criar VLANs públicas e privadas na infraestrutura do IBM Cloud (SoftLayer) para isolamento da rede de seu cluster](/docs/infrastructure/vlans/getting-started.html#getting-started-with-vlans)
- [Assegurar que todos os nós do trabalhador tenham conectividade de rede com a URL do mestre](cs_firewall.html#firewall) <p>**Nota**: se um nó do trabalhador possuir duas VLANs públicas e privadas, a conectividade de rede será configurada. Se nós do trabalhador forem configurados com uma VLAN privada apenas, será necessário configurar uma solução alternativa para conectividade de rede. Para obter mais informações, veja [Conexão da VLAN para nós do trabalhador](cs_clusters.html#worker_vlan_connection). </p>
- [Atualizando o kube-apiserver mestre quando atualizações de versão do Kubernetes estão disponíveis](cs_cluster_update.html#master)
- [Mantendo os nós do trabalhador atualizados em versões principais, secundárias e de correção](cs_cluster_update.html#worker_node)
- [Recuperando nós do trabalhador problemáticos executando comandos `kubectl`, como `cordon` ou `drain`, e executando comandos `bx cs`, como `reboot`, `reload` ou `delete`](cs_cli_reference.html#cs_worker_reboot)
- [Incluindo ou removendo sub-redes na infraestrutura do IBM Cloud (SoftLayer) conforme necessário](cs_subnets.html#subnets)
- [Fazer backup e restaurar dados no armazenamento persistente na infraestrutura do IBM Cloud (SoftLayer) ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](../services/RegistryImages/ibm-backup-restore/index.html)
- [Configurando o monitoramento de funcionamento para os nós do trabalhador com Recuperação automática](cs_health.html#autorecovery)

<br />


## Abuso do {{site.data.keyword.containerlong_notm}}
{: #terms}

Os clientes não podem fazer uso inadequado do {{site.data.keyword.containershort_notm}}.
{:shortdesc}

Uso inadequado inclui:

*   Qualquer atividade ilegal
*   Distribuição ou execução de malware
*   Prejudicar o {{site.data.keyword.containershort_notm}} ou interferir no
uso de alguém do {{site.data.keyword.containershort_notm}}
*   Prejudicar ou interferir no uso de alguém de qualquer outro serviço ou sistema
*   Acesso não autorizado a qualquer serviço ou sistema
*   Modificação não autorizada de qualquer serviço ou sistema
*   Violação dos direitos de outros


Veja [Termos dos serviços
de nuvem](https://console.bluemix.net/docs/overview/terms-of-use/notices.html#terms) para obter os termos gerais de uso.
