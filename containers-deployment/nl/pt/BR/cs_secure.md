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




# Segurança para o {{site.data.keyword.containerlong_notm}}
{: #security}

É possível usar recursos de segurança integrada no {{site.data.keyword.containerlong}} para análise de risco e proteção de segurança. Esses recursos ajudam você a proteger sua infraestrutura de cluster do Kubernetes e a comunicação de rede, isolar seus recursos de cálculo e a assegurar a conformidade de segurança em seus componentes de infraestrutura e implementações de contêiner.
{: shortdesc}



## Segurança por componente de cluster
{: #cluster}

Cada cluster do {{site.data.keyword.containerlong_notm}} tem recursos de segurança que são construídos em seus nós [principal](#master) e [trabalhador](#worker).
{: shortdesc}

Se você tem um firewal ou deseja executar comandos `kubectl` de seu sistema local quando as políticas de rede corporativa evitam o acesso aos terminais de Internet pública, [abra as portas em seu firewall](cs_firewall.html#firewall). Para conectar os apps em seu cluster a uma rede no local ou a outros apps externos ao seu cluster, [configure a conectividade VPN](cs_vpn.html#vpn).

No diagrama a seguir, é possível ver recursos de segurança que são agrupados por mestre do Kubernetes, nós do trabalhador e imagens de contêiner.

<img src="images/cs_security.png" width="400" alt="{{site.data.keyword.containershort_notm}} cluster security" style="width:400px; border-style: none"/>


<table summary="A primeira linha na tabela abrange ambas as colunas. As linhas restantes devem ser lidas da esquerda para a direita, com o componente de segurança na coluna um e os recursos para corresponder na coluna dois.">
<caption>Segurança por componente</caption>
  <thead>
    <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> Configurações de segurança de cluster integrado no {{site.data.keyword.containershort_notm}}</th>
  </thead>
  <tbody>
    <tr>
      <td>Mestre do Kubernetes</td>
      <td>O mestre do Kubernetes em cada cluster é gerenciado pela IBM e é altamente disponível. O mestre inclui as configurações de segurança do {{site.data.keyword.containershort_notm}} que asseguram a conformidade de segurança e comunicação segura para/de nós do trabalhador. As atualizações de segurança são executadas pela IBM conforme necessário. O mestre do Kubernetes dedicado controla e monitora centralmente todos os recursos do Kubernetes no cluster. Com base nos requisitos de implementação e na capacidade no cluster, o mestre do Kubernetes planeja automaticamente os seus apps conteinerizados para implementar ao longo de nós do trabalhador disponíveis. Para obter mais informações, veja [Segurança do
mestre do Kubernetes](#master).</td>
    </tr>
    <tr>
      <td>Nó do trabalhador</td>
      <td>Os contêineres são implementados em nós do trabalhador que são dedicados a um cluster e que asseguram o isolamento de cálculo, de rede e de armazenamento para os clientes IBM. O {{site.data.keyword.containershort_notm}} fornece recursos de segurança integrados para manter os nós do trabalhador seguros nas redes privada e pública e para assegurar a conformidade de segurança do nó do trabalhador. Para obter mais informações, veja [Segurança do nó do trabalhador](#worker). Além disso, é possível incluir [Políticas de rede do Calico](cs_network_policy.html#network_policies) para especificar melhor o tráfego de rede que é permitido ou bloqueado para/de um pod em um nó do trabalhador.</td>
    </tr>
    <tr>
      <td>Imagens</td>
      <td>Como administrador de cluster, é possível configurar o seu próprio repositório de imagem do Docker seguro no {{site.data.keyword.registryshort_notm}} no qual é possível armazenar e compartilhar imagens do Docker entre os seus usuários de cluster. Para assegurar implementações seguras de contêiner, cada imagem em seu registro privado é varrida pelo Vulnerability Advisor. O Vulnerability Advisor é um componente do {{site.data.keyword.registryshort_notm}} que varre para obter potenciais vulnerabilidades, faz recomendações de segurança e fornece instruções para resolver vulnerabilidades. Para obter mais informações, veja [Segurança da imagem no {{site.data.keyword.containershort_notm}}](#images).</td>
    </tr>
  </tbody>
</table>

<br />


## Mestre do Kubernetes
{: #master}

Revise os recursos de segurança integrados do mestre do Kubernetes para proteger o mestre do Kubernetes e para assegurar a comunicação de rede de cluster.
{: shortdesc}

<dl>
  <dt>Mestre do Kubernetes totalmente gerenciado e dedicado</dt>
    <dd>Cada cluster do Kubernetes no {{site.data.keyword.containershort_notm}} é controlado por um mestre do Kubernetes dedicado que é gerenciado pela IBM em uma conta de infraestrutura do IBM Cloud (SoftLayer) pertencente à IBM. O mestre do Kubernetes é configurado com os componentes dedicados a seguir que não são compartilhados com outros clientes IBM ou por clusters diferentes dentro da mesma conta IBM.
      <ul><li>Armazenamento de dados etcd: armazena todos os recursos do Kubernetes de um cluster, como Serviços, Implementações e Pods. Os ConfigMaps e Segredos do Kubernetes são dados do app armazenados como pares de valores de chave para que eles possam ser usados por um app que é executado em um pod. Os dados no etcd são armazenados em um disco criptografado que é gerenciado pela IBM e submetido a backup diariamente. Quando enviados para um pod, os dados são criptografados por TLS para assegurar a proteção e integridade de dados. </li>
      <li>kube-apiserver: serve como o ponto de entrada principal para todas as solicitações do nó do trabalhador para o mestre do Kubernetes. O kube-apiserver valida e processa solicitações e pode ler e gravar
no armazenamento de dados etcd.</li>
      <li>kube-scheduler: decida onde implementar pods, considerando as necessidades de capacidade e desempenho, as restrições de política de hardware e software, as especificações de antiafinidade e os requisitos de carga de trabalho. Se não puder ser localizado nenhum nó do trabalhador que corresponda aos requisitos, o pod não será implementado
no cluster.</li>
      <li>kube-controller-manager: responsável por monitorar os conjuntos de réplicas e criar pods correspondentes
para atingir o estado desejado.</li>
      <li>OpenVPN: o componente específico do {{site.data.keyword.containershort_notm}} para fornecer conectividade de rede segura para toda comunicação do mestre do Kubernetes com o nó do trabalhador.</li></ul></dd>
  <dt>Conectividade de rede assegurada TLS para toda comunicação do nó do trabalhador com o mestre do Kubernetes</dt>
    <dd>Para proteger a conexão com o mestre do Kubernetes, o {{site.data.keyword.containershort_notm}} gera certificados TLS que criptografam a comunicação para/do kube-apiserver e armazenamento de dados etcd. Esses certificados nunca são compartilhados entre clusters ou entre os componentes do mestre do Kubernetes.</dd>
  <dt>Conectividade de rede segura OpenVPN para toda comunicação do mestre do Kubernetes com o nó do trabalhador</dt>
    <dd>Embora o Kubernetes assegure a comunicação entre o mestre do Kubernetes e os nós do trabalhador usando o protocolo `https`, nenhuma autenticação é fornecida no nó do trabalhador por padrão. Para assegurar essa comunicação, o {{site.data.keyword.containershort_notm}} configura automaticamente uma conexão OpenVPN entre o mestre do Kubernetes e o nó do trabalhador quando o cluster é criado.</dd>
  <dt>Monitoramento contínuo de rede do mestre do Kubernetes</dt>
    <dd>Cada mestre do Kubernetes é monitorado continuamente pela IBM para controlar e corrigir os ataques de Negação de Serviço (DOS) de nível de processo.</dd>
  <dt>Conformidade de segurança do nó do mestre do Kubernetes</dt>
    <dd>O {{site.data.keyword.containershort_notm}} varre automaticamente cada nó em que o mestre do Kubernetes é implementado em busca de vulnerabilidades localizadas em correções de segurança específicas do Kubernetes e do S.O. Se vulnerabilidades forem localizadas, o {{site.data.keyword.containershort_notm}} aplicará automaticamente as correções e resolverá as vulnerabilidades em nome do usuário para assegurar proteção do nó principal. </dd>
</dl>

<br />


## Nós do trabalhador
{: #worker}

Revise os recursos de segurança do nó do trabalhador integrado para proteger o ambiente do nó do trabalhador e para assegurar o isolamento de recurso, rede e armazenamento.
{: shortdesc}

<dl>
  <dt>Propriedade do nó do trabalhador</dt>
    <dd>A propriedade de nós do trabalhador depende do tipo de cluster criado. <p> Os nós do trabalhador em clusters grátis são provisionados para a conta de infraestrutura do IBM Cloud (SoftLayer) que é de propriedade da IBM. Os usuários podem implementar apps nos nós do trabalhador, mas não podem mudar as configurações ou instalar software adicional no nó do trabalhador.</p>
    <p>Os nós do trabalhador em clusters padrão são provisionados para a conta de infraestrutura do IBM Cloud (SoftLayer) que está associada à conta do IBM Cloud público ou dedicado do cliente. Os nós do trabalhador são de propriedade do cliente. Os clientes podem escolher mudar as configurações de segurança ou instalar software adicional nos nós do trabalhador, conforme fornecido pelo {{site.data.keyword.containerlong}}.</p> </dd>
  <dt>Cálculo, rede e armazenamento de infraestrutura de isolamento</dt>
    <dd><p>Ao criar um cluster, os nós do trabalhador são provisionados como máquinas virtuais pela IBM. Os nós do trabalhador são dedicados a um cluster e não hospedam cargas de outros clusters.</p>
    <p> Cada conta do {{site.data.keyword.Bluemix_notm}} é configurada com VLANs de infraestrutura do IBM Cloud (SoftLayer) para assegurar o desempenho e o isolamento da rede de qualidade nos nós do trabalhador. Também é possível designar nós do trabalhador como privados conectando-os somente a uma VLAN privada.</p> <p>Para persistir dados em seu cluster, é possível provisionar armazenamento de arquivo baseado em NFS dedicado da infraestrutura do IBM Cloud (SoftLayer) e alavancar os recursos de segurança de dados integrados dessa plataforma.</p></dd>
  <dt>Configuração do nó do trabalhador seguro</dt>
    <dd><p>Cada nó do trabalhador é configurado com um sistema operacional Ubuntu que não pode ser mudado pelo proprietário do nó do trabalhador. Como o sistema operacional do nó do trabalhador é Ubuntu, todos os contêineres que são implementados no nó do trabalhador devem usar uma distribuição Linux que use o kernel do Ubuntu. As distribuições Linux que devem conversar com o kernel de uma maneira diferente não podem ser usadas. Para proteger o sistema operacional dos nós do trabalhador de potenciais ataques, cada nó do trabalhador é definido com configurações de firewall de especialista que são impingidas por regras iptable do Linux.</p>
    <p>Todos os contêineres que são executados no Kubernetes são protegidos pelas configurações de política de rede Calico que são configuradas em cada nó do trabalhador durante a criação de cluster. Essa configuração assegura a comunicação de rede segura entre os nós do trabalhador e os pods.</p>
    <p>Acesso SSH é desativado no nó do trabalhador. Se você tiver um cluster padrão e desejar instalar mais recursos em seu nó do trabalhador, será possível usar [Conjuntos de daemons do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset) para tudo o que você desejar executar em cada nó do trabalhador. Para qualquer ação única que se deve executar, use [tarefas do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/).</p></dd>
  <dt>Conformidade de segurança do nó do trabalhador do Kubernetes</dt>
    <dd>A IBM trabalha com as equipes consultivas de segurança interna e externa para resolver vulnerabilidades de conformidade de segurança em potencial. <b>Importante</b>: use o [comando](cs_cli_reference.html#cs_worker_update) `bx cs worker-update` regularmente (mensalmente, por exemplo) para implementar atualizações e correções de segurança para o sistema operacional e para atualizar a versão do Kubernetes. Quando as atualizações estiverem disponíveis, você será notificado quando visualizar informações sobre os nós do trabalhador, como com o comando `bx cs workers <cluster_name>` ou `bx cs worker-get <cluster_name> <worker_ID>`.</dd>
  <dt>Opção para implementar nós em servidores físicos (bare metal)</dt>
    <dd>Se escolher provisionar seus nós do trabalhador em servidores físicos bare metal (em vez de instâncias de servidor virtual), você terá mais controle sobre o host de cálculo, como a memória ou CPU. Essa configuração elimina o hypervisor da máquina virtual que aloca recursos físicos para máquinas virtuais executadas no host. Em vez disso, todos os recursos de uma máquina bare metal são dedicados exclusivamente ao trabalhador, portanto, você não precisará se preocupar com "vizinhos barulhentos" compartilhando recursos ou diminuindo o desempenho. Os servidores bare metal são dedicados a você, com todos os seus recursos disponíveis para uso de cluster.</dd>
  <dt id="trusted_compute">{{site.data.keyword.containershort_notm}} com Cálculo confiável</dt>
    <dd><p>Ao [implementar seu cluster em bare metal](cs_clusters.html#clusters_ui) que suporta Cálculo confiável, é possível ativar a confiança. O chip Trusted Platform Module (TPM) é ativado em cada nó do trabalhador bare metal no cluster que suporta Cálculo confiável (incluindo nós futuros que você incluir no cluster). Portanto, depois de ativar a confiança, não será possível desativá-la posteriormente para o cluster. Um servidor de confiança é implementado no nó principal e um agente de confiança é implementado como um pod no nó do trabalhador. Quando seu nó do trabalhador é inicializado, o pod de agente de confiança monitora cada estágio do processo.</p>
    <p>Por exemplo, se um usuário não autorizado obtém acesso ao seu sistema e modifica o kernel do S.O. com lógica extra para coletar dados, o agente de confiança detecta essa mudança e marca o nó como não confiável. Com cálculo confiável, é possível verificar seus nós do trabalhador com relação à violação.</p>
    <p><strong>Nota</strong>: o cálculo confiável está disponível para selecionar tipos de máquina bare metal. Por exemplo, os tipos GPU `mgXc` não suportam o Cálculo confiável.</p></dd>
  <dt id="encrypted_disks">Disco criptografado</dt>
    <dd><p>Por padrão, o {{site.data.keyword.containershort_notm}} fornece duas partições de dados criptografados de SSD local para todos os nós do trabalhador quando os nós do trabalhador são provisionados. A primeira partição não é criptografada e a segunda partição montada em _/var/lib/docker_ é desbloqueada usando chaves de criptografia LUKS. Cada trabalhador em cada cluster do Kubernetes tem sua própria chave de criptografia LUKS exclusiva, gerenciada pelo {{site.data.keyword.containershort_notm}}. Ao criar um cluster ou incluir um nó do trabalhador em um cluster existente, as chaves são obtidas de forma segura e, depois, descartadas após o disco criptografado ser desbloqueado.</p>
    <p><b>Observação</b>: a criptografia pode impactar o desempenho de E/S do disco. Para cargas de trabalho que requerem E/S de disco de alto desempenho, teste um cluster com criptografia ativada e desativada para ajudá-lo a decidir se deseja desativar a criptografia.</p></dd>
  <dt>Suporte para firewalls de rede da infraestrutura do IBM Cloud (SoftLayer)</dt>
    <dd>O {{site.data.keyword.containershort_notm}} é compatível com todas as [ofertas de firewall do IBM Cloud (SoftLayer) ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/cloud-computing/bluemix/network-security). No {{site.data.keyword.Bluemix_notm}} Public, é possível configurar um firewall com políticas de rede customizada para fornecer segurança de rede dedicada para seu cluster padrão e para detectar e corrigir intrusão de rede. Por exemplo, você pode escolher configurar um [Virtual Router Appliance](/docs/infrastructure/virtual-router-appliance/about.html) para agir como seu firewall e bloquear tráfego indesejado. Ao configurar um firewall, [deve-se também abrir as portas e os endereços IP necessários](cs_firewall.html#firewall) para cada região para que o mestre e os nós do trabalhador possam se comunicar.</dd>
  <dt>Manter os serviços privados ou expor os serviços e apps seletivamente para a Internet pública</dt>
    <dd>É possível escolher manter seus serviços e apps privados e alavancar os recursos de segurança integrados para garantir a comunicação protegida entre os nós do trabalhador e os pods. Para expor os serviços e apps para a Internet pública, é possível alavancar o suporte do Ingresso e do balanceador de carga para tornar os seus serviços publicamente disponíveis com segurança.</dd>
  <dt>Conecte com segurança seus nós do trabalhador e apps a um data center local</dt>
    <dd><p>Para conectar seus nós do trabalhador e apps a um data center no local, é possível configurar um terminal IPsec VPN com um serviço strongSwan, um Virtual Router Appliance ou um Fortigate Security Appliance.</p>
    <ul><li><b>Serviço de VPN IPsec do strongSwan</b>: é possível configurar um [serviço de VPN IPSec do strongSwan ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.strongswan.org/) que conecta de forma segura seu cluster do Kubernetes a uma rede no local. O serviço strongSwan IPSec VPN fornece um canal de comunicação seguro de ponta a ponta na Internet, que se baseia no conjunto de protocolo Internet Protocol Security (IPsec) padrão de mercado. Para configurar uma conexão segura entre seu cluster e uma rede no local, [configure e implemente o serviço VPN IPSec do strongSwan](cs_vpn.html#vpn-setup) diretamente em um pod no cluster.
    </li>
    <li><b>Virtual Router Appliance (VRA) or Fortigate Security Appliance (FSA)</b>: você pode escolher configurar um [VRA](/docs/infrastructure/virtual-router-appliance/about.html) ou [FSA](/docs/infrastructure/fortigate-10g/about.html) para configurar um terminal de VPN IPSec. Essa opção é útil quando se tem um cluster maior e se deseja acessar recursos não Kubernetes por meio da VPN ou acessar múltiplos clusters por meio de uma única VPN. Para configurar um VRA, veja [Configurando a conectividade VPN com o VRA](cs_vpn.html#vyatta).</li></ul></dd>

  <dt>Monitoramento contínuo e criação de log de atividade do cluster</dt>
    <dd>Para clusters padrão, todos os eventos relacionados ao cluster podem ser registrados e enviados ao {{site.data.keyword.loganalysislong_notm}} e ao {{site.data.keyword.monitoringlong_notm}}. Esses eventos envolvem a inclusão de um nó do trabalhador, o progresso de atualização contínua ou as informações de uso de capacidade. É possível [configurar a criação de log de cluster](/docs/containers/cs_health.html#logging) e o [monitoramento de cluster](/docs/containers/cs_health.html#view_metrics) para decidir sobre os eventos que você deseja monitorar. </dd>
</dl>

<br />


## Imagens
{: #images}

Gerencie a segurança e integridade de suas imagens com recursos de segurança integrada.
{: shortdesc}

<dl>
<dt>Repositório seguro de imagem privada do Docker no {{site.data.keyword.registryshort_notm}}</dt>
  <dd>Configure seu próprio repositório de imagem do Docker em um registro de imagem privada de diversos locatários, altamente disponível e escalável que é hospedado e gerenciado pela IBM. Usando o registro, é possível construir, armazenar com segurança e compartilhar imagens do Docker entre usuários do cluster.
  <p>Saiba mais sobre [como proteger suas informações pessoais](cs_secure.html#pi) quando trabalhar com imagens de contêiner.</p></dd>
<dt>Conformidade de segurança de imagem</dt>
  <dd>Quando você usar o {{site.data.keyword.registryshort_notm}}, será possível alavancar a varredura de segurança integrada que é fornecida pelo Vulnerability Advisor. Cada imagem enviada por push para o seu namespace é varrida automaticamente para obter vulnerabilidades com relação a um banco de dados de problemas conhecidos do CentOS, Debian, Red Hat e Ubuntu. Se vulnerabilidades forem localizadas, o Vulnerability Advisor fornecerá instruções de como resolvê-las para assegurar a integridade e segurança da imagem.</dd>
</dl>

Para visualizar a avaliação de vulnerabilidade para suas imagens, [revise a documentação do Vulnerability Advisor](/docs/services/va/va_index.html#va_registry_cli).

<br />


## Rede em cluster
{: #in_cluster_network}

A comunicação de rede segura em cluster entre os nós do trabalhador e os pods é realizada com redes locais virtuais privadas (VLANs). Uma VLAN configura um grupo de
nós do trabalhador e pods como se eles estivessem conectados à mesma ligação física.
{:shortdesc}

Ao criar um cluster, cada cluster é conectado automaticamente a uma VLAN privada. A VLAN privada
determina o endereço IP privado que é designado a um nó do trabalhador durante a criação de cluster.

|Tipo de cluster|Gerenciador da VLAN privada para o cluster|
|------------|-------------------------------------------|
|Clusters livres no {{site.data.keyword.Bluemix_notm}}|{{site.data.keyword.IBM_notm}}|
|Clusters padrão no {{site.data.keyword.Bluemix_notm}}|Você em sua conta de infraestrutura do IBM Cloud (SoftLayer) <p>**Dica:** para ter acesso a todas as VLANs em sua conta, ative a [VLAN Spanning](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning).</p>|
{: caption="Gerentes de VLANs privadas" caption-side="top"}

Todos os pods implementados em um nó do trabalhador também são designados a um endereço IP privado. Os pods são
designados a um IP na variação de endereços privados 172.30.0.0/16 e são roteados somente entre os nós do trabalhador. Para evitar conflitos, não use esse intervalo de IPs em quaisquer nós que se comunicam com seus nós do trabalhador. Os nós do trabalhador e os pods podem se comunicar com segurança na rede privada usando os endereços IP
privados. No entanto, quando um pod trava ou um nó do trabalhador precisa ser recriado, um novo endereço IP privado
é designado.

Por padrão, é difícil rastrear a mudança de endereços IP privados para apps que devem ser altamente disponíveis. Para evitar isso, é possível usar os recursos de descoberta de serviço do Kubernetes integrados e expor os apps como serviços de IP do cluster na rede privada. Um serviço do Kubernetes agrupa um conjunto de pods e fornece uma conexão de rede a esses pods para outros serviços no cluster sem expor o endereço IP privado real de cada pod. Ao criar um serviço IP do cluster, um endereço IP privado é designado a esse serviço do intervalo de endereços privados 10.10.10.0/24. Como ocorre com a variação de endereços privados do pod, não use esse intervalo de IPs em quaisquer nós que se comunicam com seus nós do trabalhador. Esse endereço IP é acessível somente dentro do cluster. Não
é possível acessar esse endereço IP na Internet. Ao mesmo tempo, uma entrada de consulta de DNS é criada para
o serviço e armazenada no componente kube-dns do cluster. A entrada DNS contém o nome do
serviço, o namespace no qual o serviço foi criado e o link para o endereço IP do cluster privado
designado.

Para acessar um pod atrás de um serviço IP do cluster, o app pode usar o endereço IP do cluster privado do serviço ou enviar uma solicitação usando o nome do serviço. Ao usar o nome do serviço, o nome é consultado no
componente kube-dns e roteado para o endereço IP do cluster privado do serviço. Quando uma solicitação
atinge o serviço, o serviço assegura que todas as solicitações sejam igualmente encaminhadas para os pods,
independentemente de seus endereços IP privados e o nó do trabalhador no qual eles estão implementados.

Para obter mais informações sobre como criar um serviço do tipo IP do cluster, veja [Serviços do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services---service-types).

Para conectar com segurança apps em um cluster do Kubernetes a uma rede no local, veja [Configurando a conectividade VPN](cs_vpn.html#vpn). Para expor seus apps para comunicação de rede externa, veja [Permitindo o acesso público a apps](cs_network_planning.html#public_access).


<br />


## Confiança no cluster
{: cs_trust}

Por padrão, o {{site.data.keyword.containerlong_notm}} fornece vários [recursos para os componentes de seu cluster](#cluster) para que seja possível implementar seus apps conteinerizados em um ambiente de segurança avançada. Amplie seu nível de confiança no cluster para melhor assegurar que o que acontece dentro de seu cluster é o que você pretende que aconteça. É possível implementar confiança no cluster de várias maneiras, conforme mostrado no diagrama a seguir.
{:shortdesc}

![Implementando contêineres com o conteúdo confiável](images/trusted_story.png)

1.  **{{site.data.keyword.containerlong_notm}} com Cálculo confiável**: em clusters bare metal, é possível ativar a confiança. O agente de confiança monitora o processo de inicialização de hardware e relata quaisquer mudanças para que seja possível verificar seus nós do trabalhador bare metal com relação à violação. Com o Cálculo confiável, é possível implementar os contêineres em hosts bare metal verificados para que as cargas de trabalho sejam executadas em hardware confiável. Observe que algumas máquinas bare metal, como GPU, não suportam Cálculo confiável. [Saiba mais sobre como o Cálculo confiável funciona](#trusted_compute).

2.  **Confiança de conteúdo para suas imagens**: assegure-se a integridade de suas imagens, ativando a confiança de conteúdo em seu {{site.data.keyword.registryshort_notm}}. Com o conteúdo confiável, é possível controlar quem pode assinar imagens como confiáveis. Depois que os assinantes confiáveis enviam uma imagem por push para seu registro, os usuários podem puxar o conteúdo assinado para que possam verificar a origem da imagem. Para obter mais informações, veja [Assinando imagens para conteúdo confiável](/docs/services/Registry/registry_trusted_content.html#registry_trustedcontent).

3.  **Contêiner Image Security Enforcement (beta)**: crie um controlador de admissão com políticas customizadas para que seja possível verificar imagens do contêiner antes de sua implementação. Com o Container Image Security Enforcement, você controla de onde as imagens são implementadas e assegura que elas atendam às políticas do [Vulnerability Advisor](/docs/services/va/va_index.html) ou aos requisitos de [confiança de conteúdo](/docs/services/Registry/registry_trusted_content.html#registry_trustedcontent). Se uma implementação não atender às políticas configuradas, a aplicação de segurança evitará modificações no cluster. Para obter mais informações, veja [Aplicando a segurança de imagem do contêiner (beta)](/docs/services/Registry/registry_security_enforce.html#security_enforce).

4.  **Scanner de vulnerabilidade do contêiner**: por padrão, o Vulnerability Advisor varre imagens que estão armazenadas no {{site.data.keyword.registryshort_notm}}. Para verificar o status de contêineres em tempo real que estão em execução no cluster, é possível instalar o scanner de contêiner. Para obter mais informações, veja [Instalando o scanner do contêiner](/docs/services/va/va_index.html#va_install_livescan).

5.  **Network analytics com o Security Advisor (visualização)**: com o {{site.data.keyword.Bluemix_notm}} Security Advisor, é possível centralizar insights de segurança por meio de serviços do {{site.data.keyword.Bluemix_notm}}, como o Vulnerability Advisor e o {{site.data.keyword.cloudcerts_short}}. Ao ativar o Security Advisor no cluster, será possível visualizar relatórios sobre tráfego de rede recebido e de saída suspeito. Para obter mais informações, consulte [Network Analytics](/docs/services/security-advisor/network-analytics.html#network-analytics). Para instalar, veja [Configurando o monitoramento de endereços IP suspeitos de clientes e de servidor para um cluster do Kubernetes](/docs/services/security-advisor/setup_cluster.html).

6.  **{{site.data.keyword.cloudcerts_long_notm}} (beta)**: se você tiver um cluster no sul dos EUA e quiser [expor o app usando um domínio customizado com o TLS](https://console.bluemix.net/docs/containers/cs_ingress.html#ingress_expose_public), será possível armazenar o seu certificado do TLS no {{site.data.keyword.cloudcerts_short}}. Os certificados expirados ou prestes a expirar também podem ser relatados no painel do Security Advisor. Para obter mais informações, consulte
[Introdução ao {{site.data.keyword.cloudcerts_short}}](/docs/services/certificate-manager/index.html#gettingstarted).

<br />


## Armazenando informações pessoais
{: #pi}

Você é responsável por assegurar a segurança de suas informações pessoais em recursos do Kubernetes e imagens de contêiner. As informações pessoais incluem seu nome, endereço, número do telefone, endereço de e-mail ou outras informações que podem identificar, contatar ou localizar você, seus clientes ou qualquer outra pessoa.
{: shortdesc}

<dl>
  <dt>Usar um segredo do Kubernetes para armazenar informações pessoais</dt>
  <dd>Somente armazene informações pessoais em recursos do Kubernetes que são projetados para conter informações pessoais. Por exemplo, não use seu nome no nome de um namespace do Kubernetes, implementação, serviço ou mapa de configuração. Para proteção e criptografia adequadas, armazenar informações pessoais em <a href="cs_app.html#secrets">Segredos do Kubernetes</a>.</dd>

  <dt>Use um `imagePullSecret` do Kubernetes para armazenar as credenciais de registro de imagem</dt>
  <dd>Não armazene informações pessoais em imagens de contêiner ou namespaces de registro. Para proteção e criptografia adequadas, armazene as credenciais de registro em <a href="cs_images.html#other">imagePullSecrets do Kubernetes</a> e outras informações pessoais em <a href="cs_app.html#secrets">Segredos do Kubernetes</a>. Lembre-se de que se as informações pessoais são armazenadas em uma camada anterior de uma imagem, excluir uma imagem pode não ser suficiente para excluir essas informações pessoais.</dd>
  </dl>

<br />






