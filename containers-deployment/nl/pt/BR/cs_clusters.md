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


# Configurando clusters
{: #clusters}

Projete a sua configuração de cluster do Kubernetes para máxima disponibilidade e capacidade do contêiner com o {{site.data.keyword.containerlong}}.
{:shortdesc}



## Planejamento de configuração de cluster
{: #planning_clusters}

Use clusters padrão para aumentar a disponibilidade do app.
{:shortdesc}

É menos provável que seus usuários
experimentem tempo de inatividade quando você distribuir a configuração entre múltiplos nós do trabalhador e clusters. Recursos integrados, como balanceamento de carga e isolamento, aumentam a resiliência com relação a potenciais
falhas com hosts, redes ou apps.

Revise estas potenciais configurações de cluster que são ordenadas com graus crescentes de
disponibilidade:

![Estágios de alta disponibilidade para um cluster](images/cs_cluster_ha_roadmap.png)

1.  Um cluster com múltiplos nós do trabalhador
2.  Dois clusters que são executados em diferentes locais na mesma região, cada um com
múltiplos nós do trabalhador
3.  Dois clusters que são executados em diferentes regiões, cada um com múltiplos nós do trabalhador


### Aumente a disponibilidade de seu cluster

<dl>
  <dt>Difundir apps pelos nós do trabalhador</dt>
    <dd>Permita que os desenvolvedores difundam seus apps em contêineres em múltiplos nós do trabalhador por cluster. Uma instância de aplicativo em cada um dos três nós do trabalhador permite o tempo de inatividade de um nó do trabalhador, sem interromper o uso do aplicativo. É possível especificar quantos nós do trabalhador incluir ao criar um cluster por meio da [GUI do {{site.data.keyword.Bluemix_notm}}](cs_clusters.html#clusters_ui) ou da [CLI](cs_clusters.html#clusters_cli). O Kubernetes limita o número máximo de nós do trabalhador que você pode ter em um cluster, portanto, lembre-se do [nó do trabalhador e das cotas de pod ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/admin/cluster-large/).
    <pre class="codeblock"><code>bx cs cluster-create --location dal10 --workers 3 --public-vlan &lt;public_VLAN_ID&gt; --private-vlan &lt;private_VLAN_ID&gt; --machine-type &lt;u2c.2x4&gt; --name &lt;cluster_name_or_ID&gt;</code></pre></dd>
  <dt>Difundir apps entre clusters</dt>
    <dd>Crie múltiplos clusters, cada um com múltiplos nós do trabalhador. Se uma indisponibilidade ocorrer com o
cluster, ainda assim os usuários poderão acessar um app que também esteja implementado em outro cluster.
      <p>Cluster
1:</p>
        <pre class="codeblock"><code>bx cs cluster-create --location dal10 --workers 3 --public-vlan &lt;public_VLAN_ID&gt; --private-vlan &lt;private_VLAN_ID&gt; --machine-type &lt;u2c.2x4&gt; --name &lt;cluster_name_or_ID&gt;</code></pre>
      <p>Cluster
2:</p>
        <pre class="codeblock"><code>bx cs cluster-create --location dal12 --workers 3 --public-vlan &lt;public_VLAN_ID&gt; --private-vlan &lt;private_VLAN_ID&gt; --machine-type &lt;u2c.2x4&gt; --name &lt;cluster_name_or_ID&gt;</code></pre></dd>
  <dt>Difundir apps entre clusters em diferentes regiões</dt>
    <dd>Ao difundir aplicativos entre clusters em diferentes regiões, será possível permitir que o balanceamento de carga ocorra com base na região em que o usuário está. Se o cluster, hardware ou até mesmo um local inteiro em
uma região ficar inativo, o tráfego será roteado para o contêiner que estiver implementado em outro local.
      <p><strong>Importante:</strong> depois de configurar um domínio customizado, você poderá usar esses comandos para criar os clusters.</p>
      <p>Local 1:</p>
        <pre class="codeblock"><code>bx cs cluster-create --location dal10 --workers 3 --public-vlan &lt;public_VLAN_ID&gt; --private-vlan &lt;private_VLAN_ID&gt; --machine-type &lt;u2c.2x4&gt; --name &lt;cluster_name_or_ID&gt;</code></pre>
      <p>Local 2:</p>
        <pre class="codeblock"><code>bx cs cluster-create --location ams03 --workers 3 --public-vlan &lt;public_VLAN_ID&gt; --private-vlan &lt;private_VLAN_ID&gt; --machine-type &lt;u2c.2x4&gt; --name &lt;cluster_name_or_ID&gt;</code></pre></dd>
</dl>

<br />







## Planejamento de configuração de nó do trabalhador
{: #planning_worker_nodes}

Um cluster do Kubernetes consiste em nós do trabalhador e é monitorado e gerenciado centralmente pelo mestre do Kubernetes. Os administradores de cluster decidem como configurar o cluster de nós do trabalhador para assegurar que os usuários do cluster tenham todos os recursos para implementar e executar apps no cluster.
{:shortdesc}

Ao criar um cluster padrão, os nós do trabalhador são pedidos na infraestrutura do IBM Cloud (SoftLayer) em seu nome e incluídos no conjunto de nós do trabalhador padrão em seu cluster. A cada nó do trabalhador é designado
um ID de nó do trabalhador e um nome de domínio exclusivos que não devem ser mudados após a criação do cluster.

É possível escolher entre servidores virtuais ou físicos (bare metal). Dependendo do nível de isolamento de hardware escolhido, os nós do trabalhador virtual podem ser configurados como nós compartilhados ou dedicados. Também é possível escolher se você deseja que os nós do trabalhador se conectem a uma VLAN pública e VLAN privada ou somente a uma VLAN privada. Cada
nó do trabalhador é provisionado com um tipo específico de máquina que determina o número de vCPUs, memória
e espaço em disco que estão disponíveis para os contêineres que são implementados no nó do trabalhador. O Kubernetes limita o número máximo de nós do trabalhador que você pode ter em um cluster. Revise [cotas de nó do trabalhador e de pod ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/admin/cluster-large/) para obter mais informações.



### Hardware para nós do trabalhador
{: #shared_dedicated_node}

Ao criar um cluster padrão no {{site.data.keyword.Bluemix_notm}}, você escolhe provisionar seus nós do trabalhador como máquinas físicas (bare metal) ou como máquinas virtuais executadas no hardware físico. Ao criar um cluster grátis, seu nó do trabalhador é provisionado automaticamente como um nó virtual compartilhado na conta de infraestrutura do IBM Cloud (SoftLayer).
{:shortdesc}

![Opções de hardware para nós do trabalhador em um cluster padrão](images/cs_clusters_hardware.png)

Revise as informações a seguir para decidir qual tipo de conjuntos de trabalhadores você deseja. Conforme você planejar, considere o [limite mínimo do limite de nós do trabalhador](#resource_limit_node) de 10% de capacidade de memória total.

<dl>
<dt>Por que eu usaria máquinas físicas (bare metal)?</dt>
<dd><p><strong>Mais recursos de cálculo</strong>: é possível provisionar o nó do trabalhador como um servidor físico de único locatário, também referido como bare metal. O bare metal dá acesso direto aos recursos físicos na máquina, como a memória ou CPU. Essa configuração elimina o hypervisor da máquina virtual que aloca recursos físicos para máquinas virtuais executadas no host. Em vez disso, todos os recursos de uma máquina bare metal são dedicados exclusivamente ao trabalhador, portanto, você não precisará se preocupar com "vizinhos barulhentos" compartilhando recursos ou diminuindo o desempenho. Os tipos de máquina física têm mais armazenamento local do que virtual e alguns têm RAID para fazer backup de dados locais.</p>
<p><strong>Faturamento mensal</strong>: os servidores bare metal são mais caros do que servidores virtuais e são mais adequados para apps de alto desempenho que precisem de mais recursos e controle do host. Os servidores bare metal são faturados mensalmente. Se você cancelar um servidor bare metal antes do final do mês, será cobrado até o final do mês. Ordenar e cancelar servidores bare metal é um processo manual por meio da sua conta de infraestrutura (SoftLayer) do IBM Cloud. Pode levar mais de um dia útil para serem concluídos.</p>
<p><strong>Opção para ativar o Cálculo confiável</strong>: ative o Cálculo confiável para verificar os seus nós do trabalhador com relação a violações. Se você não ativar a confiança durante a criação do cluster, mas quiser fazer isso mais tarde, será possível usar o [comando](cs_cli_reference.html#cs_cluster_feature_enable) `bx cs feature-enable`. Depois de ativar a confiança, não é possível desativá-la posteriormente. É possível fazer um novo cluster sem confiança. Para obter mais informações sobre como a confiança funciona durante o processo de inicialização do nó, veja [{{site.data.keyword.containershort_notm}} com Cálculo confiável](cs_secure.html#trusted_compute). O Cálculo confiável está disponível em clusters que executam o Kubernetes versão 1.9 ou mais recente e têm determinados tipos de máquina bare metal. Quando você executa o [comando](cs_cli_reference.html#cs_machine_types) `bx cs machine-types <location>`, é possível ver quais máquinas suportam confiança, revisando o campo **Confiável**. Por exemplo, os tipos GPU `mgXc` não suportam o Cálculo confiável.</p></dd>
<dt>Por que usar máquinas virtuais.</dt>
<dd><p>Com as VMs, você tem maior flexibilidade, tempos de fornecimento mais rápidos e recursos de escalabilidade mais automáticos do que o bare metal, com custo reduzido. É possível usar as VMs para casos de uso de propósitos mais gerais, como ambientes de teste e desenvolvimento, ambientes de preparação e produção, microsserviços e apps de negócios. No entanto, há alternativas no desempenho. Se precisar de cálculo de alto desempenho para cargas de trabalho com uso intensivo de RAM, dados ou GPU, use bare metal.</p>
<p><strong>Decidir entre ocupação única ou múltipla</strong>: ao criar um cluster virtual padrão, deve-se escolher se deseja que o hardware subjacente seja compartilhado por múltiplos clientes {{site.data.keyword.IBM_notm}} (ocupação múltipla) ou seja dedicado somente a você (ocupação única).</p>
<p>Em uma configuração de diversos locatários, os recursos físicos, como CPU e memória, são compartilhados entre todas as
máquinas virtuais implementadas no mesmo hardware físico. Para assegurar que cada máquina
virtual possa ser executada independentemente, um monitor de máquina virtual, também referido como hypervisor,
segmenta os recursos físicos em entidades isoladas e aloca como recursos dedicados para
uma máquina virtual (isolamento de hypervisor).</p>
<p>Em uma configuração de locatário único, todos os recursos físicos são dedicados somente a você. É possível implementar
múltiplos nós do trabalhador como máquinas virtuais no mesmo host físico. Semelhante à configuração de diversos locatários,
o hypervisor assegura que cada nó do trabalhador obtenha seu compartilhamento dos recursos físicos
disponíveis.</p>
<p>Os nós compartilhados são geralmente menos dispendiosos que os nós dedicados porque os custos para o hardware subjacente são compartilhados entre múltiplos clientes. No entanto, ao decidir entre nós compartilhados
e dedicados, você pode desejar verificar com seu departamento jurídico para discutir o nível de isolamento
e conformidade de infraestrutura que seu ambiente de app requer.</p>
<p><strong>Tipos de máquinas virtuais `u2c` ou `b2c`</strong>: essas máquinas usam disco local, em vez de storage area networking (SAN), para confiabilidade. Os benefícios de confiabilidade incluem maior rendimento ao serializar bytes para o disco local e a degradação do sistema de arquivos reduzido devido a falhas de rede. Esses tipos de máquina contêm 25 GB de armazenamento em disco local primário para o sistema de arquivos do OS e 100 GB de armazenamento em disco local secundário para `/var/lib/docker`, o diretório em que todos os dados de contêiner são gravados.</p>
<p><strong>E se eu tiver descontinuado os tipos de máquina `u1c` ou `b1c`?</strong> Para começar a usar os tipos de máquina `u2c` e `b2c`, [atualize os tipos de máquina incluindo nós do trabalhador](cs_cluster_update.html#machine_type).</p></dd>
<dt>De quais tipos de máquinas virtuais e físicas posso escolher?</dt>
<dd><p>Muitos! Selecione o tipo de máquina que for melhor para seu caso de uso. Lembre-se de que um conjunto de trabalhadores consiste em máquinas do mesmo tipo. Se desejar uma combinação de tipos de máquina no cluster, crie conjuntos de trabalhadores separados para cada tipo.</p>
<p>Tipos de máquina variam por zona. Para ver os tipos de máquina disponíveis em sua zona, execute `bx cs machine-types <zone_name>`.</p>
<p><table>
<caption>Tipos disponíveis de máquinas físicas (bare metal) e virtuais no {{site.data.keyword.containershort_notm}}.</caption>
<thead>
<th>Caso Nome e uso</th>
<th>Núcleos / Memória</th>
<th>Disco Primário / Secundário</th>
<th>Velocidade</th>
</thead>
<tbody>
<tr>
<td><strong>Virtual, u2c.2x4</strong>: use essa VM de menor tamanho para teste rápido, provas de conceito e outras cargas de trabalho leves.</td>
<td>2 / 4GB</td>
<td>25GB / 100GB</td>
<td>1000Mbps</td>
</tr>
<tr>
<td><strong>Virtual, b2c.4x16</strong>: selecione essa VM balanceada para teste e desenvolvimento e outras cargas de trabalho leves.</td>
<td>4 / 16GB</td>
<td>25GB / 100GB</td>
<td>1000Mbps</td>
</tr>
<tr>
<td><strong>Virtual, b2c.16x64</strong>: selecione essa VM balanceada para cargas de trabalho de médio porte.</td></td>
<td>16 / 64GB</td>
<td>25GB / 100GB</td>
<td>1000Mbps</td>
</tr>
<tr>
<td><strong>Virtual, b2c.32x128</strong>: selecione essa VM balanceada para cargas de trabalho de médio a grande porte, como um banco de dados e um website dinâmico com vários usuários simultâneos.</td></td>
<td>32 / 128GB</td>
<td>25GB / 100GB</td>
<td>1000Mbps</td>
</tr>
<tr>
<td><strong>Virtual, b2c.56x242</strong>: selecione essa VM balanceada para cargas de trabalho grandes, como um banco de dados e múltiplos apps com muitos usuários simultâneos.</td></td>
<td>56 / 242GB</td>
<td>25GB / 100GB</td>
<td>1000Mbps</td>
</tr>
<tr>
<td><strong>Bare metal com uso intensivo de RAM, mr1c.28x512</strong>: maximize a RAM disponível para os nós do trabalhador.</td>
<td>28 / 512GB</td>
<td>2TB SATA / 960GB SSD</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>Bare metal GPU, mg1c.16x128</strong>: escolha esse tipo para cargas de trabalho matematicamente intensivas, como aplicativos de cálculo de alto desempenho, aprendizado de máquina ou 3D. Esse tipo tem 1 cartão físico Tesla K80 com 2 unidades de processamento de gráfico (GPUs) por cartão para um total de 2 GPUs.</td>
<td>16 / 128GB</td>
<td>2TB SATA / 960GB SSD</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>Bare metal GPU, mg1c.28x256</strong>: escolha esse tipo para cargas de trabalho matematicamente intensivas, como aplicativos de cálculo de alto desempenho, aprendizado de máquina ou 3D. Esse tipo tem 2 cartões físicos Tesla K80 com 2 GPUs por cartão para um total de 4 GPUs.</td>
<td>28 / 256GB</td>
<td>2TB SATA / 960GB SSD</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>Bare metal com uso intensivo de dados, md1c.16x64.4x4tb</strong>: para uma quantia significativa de armazenamento em disco local, incluindo RAID para fazer backup de dados que são armazenados localmente na máquina. Use para casos como sistemas de arquivo distribuído, bancos de dados grandes e cargas de trabalho de análise de Big Data.</td>
<td>16 / 64GB</td>
<td>2x2TB RAID1 / 4x4TB SATA RAID10</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>Bare metal com uso intensivo de dados, md1c.28x512.4x4tb</strong>: para uma quantia significativa de armazenamento em disco local, incluindo RAID para fazer backup de dados que são armazenados localmente na máquina. Use para casos como sistemas de arquivo distribuído, bancos de dados grandes e cargas de trabalho de análise de Big Data.</td>
<td>28 / 512 GB</td>
<td>2x2TB RAID1 / 4x4TB SATA RAID10</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>Bare metal balanceado, mb1c.4x32</strong>: use para cargas de trabalho balanceadas que requerem mais recursos de cálculo do que as máquinas virtuais oferecem.</td>
<td>4 / 32GB</td>
<td>2TB SATA / 2TB SATA</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>Bare metal balanceado, mb1c.16x64</strong>: use para cargas de trabalho balanceadas que requerem mais recursos de cálculo do que as máquinas virtuais oferecem.</td>
<td>16 / 64GB</td>
<td>2TB SATA / 960GB SSD</td>
<td>10000Mbps</td>
</tr>
</tbody>
</table>
</p>
</dd>
</dl>


É possível implementar clusters usando a [UI do console](#clusters_ui) ou a [CLI](#clusters_cli).

### Conexão VLAN para nós do trabalhador
{: #worker_vlan_connection}

Ao criar um cluster, cada cluster é conectado automaticamente a uma VLAN de sua conta de infraestrutura do IBM Cloud (SoftLayer).
{:shortdesc}

Uma VLAN configura um grupo de
nós do trabalhador e pods como se eles estivessem conectados à mesma ligação física.
* A VLAN pública tem duas sub-redes provisionadas automaticamente nela. A sub-rede pública primária determina o endereço IP público que é designado a um nó do trabalhador durante a criação de cluster e a sub-rede pública móvel fornece endereços IP públicos para os serviços de rede de Ingresso e de balanceador de carga.
* A VLAN privada também tem duas sub-redes provisionadas automaticamente nela. A sub-rede privada primária determina o endereço IP privado que é designado a um nó do trabalhador durante a criação de cluster e a sub-rede privada móvel fornece endereços IP privados para os serviços de rede de Ingresso e de balanceador de carga.

Para clusters grátis, os nós do trabalhador do cluster são conectados a uma VLAN pública e VLAN privada pertencentes à IBM por padrão durante a criação de cluster.

Para clusters padrão, na primeira vez que você cria um cluster em um local, uma VLAN pública e uma VLAN privada são provisionadas automaticamente. Para cada cluster subsequente criado nesse local, você escolhe as VLANs que deseja usar. É possível conectar seus nós do trabalhador a uma VLAN pública e à VLAN privada ou somente à VLAN privada. Se você deseja conectar seus nós do trabalhador somente a uma VLAN privada, é possível usar o ID de uma VLAN privada existente ou [crie uma VLAN privada](/docs/cli/reference/softlayer/index.html#sl_vlan_create) e usar o ID durante a criação de cluster. Se os nós do trabalhador são configurados somente com uma VLAN privada, deve-se configurar uma solução alternativa para conectividade de rede, como um [Virtual Router Appliance](cs_vpn.html#vyatta), para que os nós do trabalhador possam se comunicar com o principal.

**Nota**: se você tem múltiplas VLANs para um cluster ou múltiplas redes na mesma VLAN, deve-se ativar a ampliação da VLAN para que seus nós do trabalhador possam se comunicar uns com os outros na rede privada. Para obter instruções, veja [Ativar ou desativar a ampliação de VLAN](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning).

### Limites de memória do nó do trabalhador
{: #resource_limit_node}

O {{site.data.keyword.containershort_notm}} configura um limite de memória em cada nó do trabalhador. Quando os pods que estão em execução no nó do trabalhador excedem esse limite de memória, os pods são removidos. No Kubernetes, esse limite é chamado de [limite máximo de despejo ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/tasks/administer-cluster/out-of-resource/#hard-eviction-thresholds).
{:shortdesc}

Se os pods são removidos frequentemente, inclua mais nós do trabalhador em seu cluster ou configure [limites de recursos ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#resource-requests-and-limits-of-pod-and-container) em seu pods.

**Cada máquina tem um limite mínimo que equivale a 10% de sua capacidade de memória total**. Quando há menos memória disponível no nó do trabalhador do que o limite mínimo que é permitido, o Kubernetes remove imediatamente o pod. O pod reagenda em outro nó do trabalhador se um nó do trabalhador está disponível. Por exemplo, se você tem uma máquina virtual `b2c.4x16`, sua capacidade de memória total é 16 GB. Se menos que 1600 MB (10%) de memória estiver disponível, novos pods não poderão ser planejados nesse nó do trabalhador, mas, como alternativa, serão planejados em outro nó do trabalhador. Se nenhum outro nó do trabalhador estiver disponível, os novos pods permanecerão não planejados.

Para revisar quanta memória é usada em seu nó do trabalhador, execute [kubectl top node ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/reference/kubectl/overview/#top).

### Recuperação automática para seus nós do trabalhador
`Docker`, `kubelet`, `kube-proxy` e `calico` são componentes críticos que devem ser funcionais para ter um nó do trabalhador do Kubernetes saudável. Com o tempo, esses componentes podem se dividir e podem deixar o nó do trabalhador em um estado não funcional. Os nós do trabalhador não funcionais diminuem a capacidade total do cluster e podem resultar em tempo de inatividade para seu app.

É possível [configurar verificações de funcionamento para seu nó do trabalhador e ativar a Recuperação automática](cs_health.html#autorecovery). Se a Recuperação automática detecta um nó do trabalhador que não está saudável com base nas verificações configuradas, a Recuperação automática aciona uma ação corretiva como um recarregamento do S.O. no nó do trabalhador. Para obter mais informações sobre como a Recuperação automática funciona, veja o [blog de Recuperação automática ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/).

<br />



## Criando clusters com a GUI
{: #clusters_ui}

O propósito do cluster do Kubernetes é definir um conjunto de recursos, nós, redes e dispositivos de armazenamento que mantêm os apps altamente disponíveis. Para poder implementar um app, deve-se criar um cluster e configurar as definições para os nós do trabalhador nesse cluster.
{:shortdesc}





Antes de iniciar, deve-se ter uma [conta do {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/registration/) pré-paga ou de Assinatura que é configurada para [acessar o portfólio de infraestrutura do IBM Cloud (SoftLayer)](cs_troubleshoot_clusters.html#cs_credentials). Para experimentar alguns dos recursos, será possível criar um cluster grátis que expira após 21 dias. Você é capaz de ter 1 cluster grátis de cada vez.

É possível remover seu cluster grátis a qualquer momento, mas depois de 21 dias, um cluster grátis e seus dados são excluídos e não podem ser restaurados. Certifique-se de fazer backup dos dados.
{: tip}

Para customizar completamente os clusters com sua opção de isolamento de hardware, zona, versão da API e mais, crie um cluster padrão.

Para criar um cluster:

1. No catálogo, selecione **Kubernetes Cluster**.

2. Selecione uma região no qual implementar o cluster.

3. Selecione um tipo de plano de cluster. É possível escolher **Grátis** ou **Padrão**. Com um cluster padrão, você tem acesso a recursos como múltiplos nós do trabalhador para um ambiente altamente disponível.

4. Configure os detalhes do seu cluster. Conclua as etapas que se aplicam ao tipo de cluster que você está criando.

    1. **Grátis e padrão**: forneça um nome ao seu cluster. O nome deve iniciar com uma letra, pode conter letras, números e hífen (-) e deve ter 35 caracteres ou menos. O nome do cluster e a região na qual o cluster está implementado formam o nome completo do domínio para o subdomínio do Ingress. Para assegurar que o subdomínio do Ingress seja exclusivo dentro de uma região, o nome do cluster pode ser truncado e anexado com um valor aleatório dentro do nome de domínio do Ingress.
 
    2. **Padrão**: selecione um local no qual implementar o seu cluster. Para o melhor desempenho, selecione o local que é fisicamente mais próximo de você. Lembre-se de que uma autorização jurídica poderá ser requerida para que os dados possam ser armazenados fisicamente em um país estrangeiro se você selecionar um local que está fora de seu país.

    3. **Padrão**: escolha a versão do servidor da API do Kubernetes para o nó principal do cluster.

    4. **Padrão**: selecione um tipo de isolamento de hardware. Virtual é faturado por hora e bare metal é faturado mensalmente.

        - **Virtual - Dedicado**: os seus nós do trabalhador são hospedados na infraestrutura que é dedicada à sua conta. Os seus recursos físicos estão completamente isolados.

        - **Virtual - Compartilhado**: os recursos de infraestrutura, como o hypervisor e hardware físico, são compartilhados entre você e outros clientes IBM, mas cada nó do trabalhador é acessível somente por você. Embora essa opção seja menos cara e suficiente na maioria dos casos, você pode desejar verificar suas necessidades de desempenho e infraestrutura com suas políticas da empresa.

        - **Bare metal**: faturados mensalmente, os servidores bare metal são provisionados pela interação manual com a infraestrutura do IBM Cloud (SoftLayer) e podem levar mais de um dia útil para serem concluídos. Bare metal é mais adequado para aplicativos de alto desempenho que precisam de mais recursos e controle do host.

        Certifique-se de que deseja provisionar uma máquina bare metal. Como o faturamento é mensal, se ele for cancelado imediatamente após uma ordem por engano, você ainda será cobrado pelo mês integral.
        {:tip}

    5.  **Padrão**: Selecione um tipo de máquina. O tipo de máquina define a quantia de CPU virtual, memória e espaço em disco que é configurada em cada nó do trabalhador e disponibilizada para os contêineres. Os tipos de máquinas virtuais e bare metal disponíveis variam pelo local no qual você implementa o cluster. Depois de criar o cluster, será possível incluir tipos de máquina diferentes, incluindo um nó no cluster.

    6. **Padrão**: especifique o número de nós do trabalhador que você precisa no cluster.

    7. **Padrão**: selecione uma VLAN pública (opcional) e uma VLAN privada (necessária) por meio de sua conta de infraestrutura do IBM Cloud (SoftLayer). Ambas as VLANs se comunicam entre os nós do trabalhador, mas a VLAN pública também se comunica com o mestre do Kubernetes gerenciado pela IBM. É possível usar a mesma VLAN para múltiplos clusters.
        **Nota**: se os nós do trabalhador estiverem configurados com apenas uma VLAN privada, será necessário configurar uma solução alternativa para conectividade de rede. Para obter mais informações, veja [Conexão da VLAN para nós do trabalhador](cs_clusters.html#worker_vlan_connection).

    8. Por padrão, **Criptografar disco local** é selecionado. Se você escolher limpar a caixa de seleção, os dados do Docker do host não serão criptografados.


4. Clique em **Criar cluster**. É possível ver o progresso da implementação do nó do trabalhador na guia **Nós do trabalhador**. Quando a implementação está pronta, é possível ver que seu cluster está pronto na guia **Visão geral**.
    **Nota:** a cada nó do trabalhador é designado um ID de nó do trabalhador e um nome de domínio exclusivos que não devem ser mudados manualmente após a criação do cluster. Mudar o ID ou o nome do domínio evita que o mestre do Kubernetes gerencie o cluster.



**O que vem a seguir?**

Quando o cluster estiver funcionando, será possível verificar as tarefas a seguir:


-   [Instale as CLIs para iniciar o trabalho com seu cluster.](cs_cli_install.html#cs_cli_install)
-   [Implementar um app no cluster.](cs_app.html#app_cli)
-   [Configure seu próprio registro privado no {{site.data.keyword.Bluemix_notm}} para armazenar e compartilhar imagens do Docker com outros usuários.](/docs/services/Registry/index.html)
- Se você tem múltiplas VLANs para um cluster ou múltiplas sub-redes na mesma VLAN, deve-se [ativar a ampliação de VLAN](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning) para que os nós do trabalhador possam se comunicar entre si na rede privada.
- Se você tiver um firewall, poderá ser necessário [abrir as portas requeridas](cs_firewall.html#firewall) para usar os comandos `bx`, `kubectl` ou `calicotl`, para permitir tráfego de saída do seu cluster ou para permitir tráfego de entrada para serviços de rede.

<br />


## Criando clusters com a CLI
{: #clusters_cli}

O propósito do cluster do Kubernetes é definir um conjunto de recursos, nós, redes e dispositivos de armazenamento que mantêm os apps altamente disponíveis. Para poder implementar um app, deve-se criar um cluster e configurar as definições para os nós do trabalhador nesse cluster.
{:shortdesc}

Antes de iniciar:
- Deve-se ter uma [conta do {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/registration/) pré-paga ou de Assinatura que é configurada para [acessar o portfólio de infraestrutura do IBM Cloud (SoftLayer)](cs_troubleshoot_clusters.html#cs_credentials). É possível criar 1 cluster grátis para experimentar alguns dos recursos por 21 dias ou criar clusters padrão totalmente customizáveis com a opção de isolamento de hardware.
- [Certifique-se de que você tenha as permissões mínimas necessárias na infraestrutura do IBM Cloud (SoftLayer) para provisionar um cluster padrão](cs_users.html#infra_access).

Para criar um cluster:

1.  Instale a CLI do {{site.data.keyword.Bluemix_notm}} e o plug-in do [{{site.data.keyword.containershort_notm}}](cs_cli_install.html#cs_cli_install).

2.  Efetue login na CLI do {{site.data.keyword.Bluemix_notm}}. Insira suas credenciais do {{site.data.keyword.Bluemix_notm}} quando solicitadas.

    ```
    bx login
    ```
    {: pre}

    **Nota:** se você tiver um ID federado, use `bx login --sso` para efetuar login na CLI do {{site.data.keyword.Bluemix_notm}}. Insira seu nome do usuário e use a URL fornecida na saída da CLI para recuperar sua senha descartável. Você sabe que tem um ID federado quando o login falha sem a opção `--sso` e é bem-sucedido com a opção `--sso`.

3. Se você tiver várias contas do {{site.data.keyword.Bluemix_notm}}, selecione a conta na qual deseja criar seu cluster do Kubernetes.

4.  Se desejar criar ou acessar clusters de Kubernetes em uma região diferente da região do
{{site.data.keyword.Bluemix_notm}} que você selecionou anteriormente, execute `bx cs region-set`.

6.  Crie um cluster.

    1.  **Clusters padrão**: revise os locais que estão disponíveis. Os locais mostrados dependem da região do {{site.data.keyword.containershort_notm}} a que você está conectado.

        ```
        bx cs locations
        ```
        {: pre}
        
        Sua saída da CLI corresponde aos [locais para a região do {{site.data.keyword.containerlong}}](cs_regions.html#locations).
        
    2.  **Clusters padrão**: escolha um local e revise os tipos de máquina disponíveis nesse local. O tipo de máquina especifica os hosts de cálculo virtual ou físico que estão disponíveis para cada nó do trabalhador.

        -  Visualize o campo **Tipo de servidor** para escolher máquinas virtuais ou físicas (bare metal).
        -  **Virtual**: faturadas por hora, as máquinas virtuais são provisionadas em hardware compartilhado ou dedicado.
        -  **Físico**: faturados mensalmente, os servidores bare metal são provisionados pela interação manual com a infraestrutura do IBM Cloud (SoftLayer) e podem levar mais de um dia útil para serem concluídos. Bare metal é mais adequado para aplicativos de alto desempenho que precisam de mais recursos e controle do host.
        - **Máquinas físicas com Cálculo confiável**: para clusters bare metal que executam o Kubernetes versão 1.9 ou mais recente, também é possível escolher ativar o [Cálculo confiável](cs_secure.html#trusted_compute) para verificar seus nós do trabalhador bare metal com relação à violação. O Cálculo confiável está disponível para selecionar tipos de máquina bare metal. Por exemplo, os tipos GPU `mgXc` não suportam o Cálculo confiável. Se você não ativar a confiança durante a criação do cluster, mas quiser fazer isso mais tarde, será possível usar o [comando](cs_cli_reference.html#cs_cluster_feature_enable) `bx cs feature-enable`. Depois de ativar a confiança, não é possível desativá-la posteriormente.
        -  **Tipos de máquina**: para decidir qual tipo de máquina implementar, revise as combinações de núcleo, memória e armazenamento ou consulte a [documentação do comando](cs_cli_reference.html#cs_machine_types) `bx cs machine-types`. Depois de criar o cluster, é possível incluir diferentes tipos de máquina física ou virtual usando o [comando](cs_cli_reference.html#cs_worker_add) `bx cs worker-add`.

           Certifique-se de que deseja provisionar uma máquina bare metal. Como o faturamento é mensal, se ele for cancelado imediatamente após uma ordem por engano, você ainda será cobrado pelo mês integral.
           {:tip}

        ```
        bx cs machine-types <location>
        ```
        {: pre}

    3.  **Clusters padrão**: verifique se uma VLAN pública e privada já existe na infraestrutura do IBM Cloud (SoftLayer) para esta conta.

        ```
        bx cs vlans <location>
        ```
        {: pre}

        ```
        ID        Name                Number   Type      Router  
        1519999   vlan   1355     private   bcr02a.dal10  
        1519898   vlan   1357     private   bcr02a.dal10 
        1518787   vlan   1252     public   fcr02a.dal10 
        1518888   vlan   1254     public    fcr02a.dal10
        ```
        {: screen}

        Se uma VLAN pública e privada já existe, observe os roteadores correspondentes. Os roteadores de VLAN privada sempre iniciam com <code>bcr</code> (roteador de backend) e roteadores de VLAN pública sempre iniciam com <code>fcr</code> (roteador de front-end). Ao criar um cluster e especificar as VLANs públicas e privadas, o número e a combinação de letras após esses prefixos devem corresponder. Na saída de exemplo, quaisquer VLANs privadas podem ser usadas com quaisquer VLANs públicas porque todos os roteadores incluem `02a.dal10`.

        Deve-se conectar os nós do trabalhador a uma VLAN privada e, opcionalmente, é possível conectá-los a uma VLAN pública. **Nota**: se os nós do trabalhador estiverem configurados com apenas uma VLAN privada, será necessário configurar uma solução alternativa para conectividade de rede. Para obter mais informações, veja [Conexão da VLAN para nós do trabalhador](cs_clusters.html#worker_vlan_connection).

    4.  **Clusters grátis e padrão**: execute o comando `cluster-create`. É possível escolher entre um cluster grátis, que inclui um nó do trabalhador configurado com 2vCPU e 4 GB de memória e é excluído automaticamente após 21 dias. Ao criar um cluster padrão, por padrão, os discos do nó do trabalhador são criptografados, seu hardware é compartilhado por múltiplos clientes da IBM e são faturados por horas de uso. </br>Exemplo para um cluster padrão. Especifique as opções do cluster:

        ```
        bx cs cluster-create --location dal10 --machine-type u2c.2x4 --hardware <shared_or_dedicated> --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID> --workers 3 --name <cluster_name> --kube-version <major.minor.patch> [--disable-disk-encrypt][--trusted]
        ```
        {: pre}

        Exemplo para um cluster grátis. Especifique o nome do cluster:

        ```
        bx cs cluster-create --name my_cluster
        ```
        {: pre}

        <table>
        <caption>Os componentes de criação de cluster</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes desse comando</th>
        </thead>
        <tbody>
        <tr>
        <td><code>cluster-create</code></td>
        <td>O comando para criar um cluster na organização do {{site.data.keyword.Bluemix_notm}}.</td>
        </tr>
        <tr>
        <td><code>--location <em>&lt;location&gt;</em></code></td>
        <td>**Clusters padrão**: substitua <em>&lt;location&gt;</em> pelo ID de local do {{site.data.keyword.Bluemix_notm}} no qual você deseja criar seu cluster. [Locais disponíveis](cs_regions.html#locations) dependem da região do {{site.data.keyword.containershort_notm}} na qual você está conectado.</td>
        </tr>
        <tr>
        <td><code>--machine-type <em>&lt;machine_type&gt;</em></code></td>
        <td>**Clusters padrão**: escolha um tipo de máquina. É possível implementar os nós do trabalhador como máquinas virtuais em hardware compartilhado ou dedicado ou como máquinas físicas no bare metal. Os tipos de máquinas físicas e virtuais disponíveis variam pelo local no qual o cluster é implementado. Para obter mais informações, veja a documentação do [comando](cs_cli_reference.html#cs_machine_types) `bx cs machine-type`. Para clusters livres, não é necessário definir o tipo de máquina.</td>
        </tr>
        <tr>
        <td><code>--hardware <em>&lt;shared_or_dedicated&gt;</em></code></td>
        <td>**Somente clusters padrão virtuais**: o nível de isolamento de hardware para seu nó do trabalhador. Use dedicado para disponibilizar recursos físicos disponíveis apenas para você ou compartilhado para permitir que os recursos físicos sejam compartilhados com outros clientes da IBM. O padrão é shared. Esse valor é opcional para clusters padrão e não está disponível para clusters livres.</td>
        </tr>
        <tr>
        <td><code>--public-vlan <em>&lt;public_vlan_id&gt;</em></code></td>
        <td><ul>
          <li>**Clusters grátis**: você não precisa definir uma VLAN pública. O cluster grátis é conectado automaticamente a uma VLAN pública que é de propriedade da IBM.</li>
          <li>**clusters padrão**: se você já tem uma VLAN pública configurada em sua conta de infraestrutura do IBM Cloud (SoftLayer) para esse local, insira o ID da VLAN pública. Se você deseja conectar seus nós do trabalhador somente a uma VLAN privada, não especifique esta opção. **Nota**: se os nós do trabalhador estiverem configurados com apenas uma VLAN privada, será necessário configurar uma solução alternativa para conectividade de rede. Para obter mais informações, veja [Conexão da VLAN para nós do trabalhador](cs_clusters.html#worker_vlan_connection).<br/><br/>
          <strong>Nota</strong>: os roteadores de VLAN privada sempre iniciam com <code>bcr</code> (roteador backend) e os roteadores de VLAN pública sempre iniciam com <code>fcr</code> (roteador frontend). Ao criar um cluster e especificar as VLANs públicas e privadas, o número e a combinação de letras após esses prefixos devem corresponder.</li>
        </ul></td>
        </tr>
        <tr>
        <td><code>--private-vlan <em>&lt;private_vlan_id&gt;</em></code></td>
        <td><ul><li>**Clusters grátis**: você não precisa definir uma VLAN privada. O cluster grátis é conectado automaticamente a uma VLAN privada que é de propriedade da IBM.</li><li>**Clusters padrão**: se você já tem uma VLAN privada configurada em sua conta de infraestrutura do IBM Cloud (SoftLayer) para esse local, insira o ID da VLAN privada. Se você não tem uma VLAN privada em sua conta, não especifique esta opção. O {{site.data.keyword.containershort_notm}} cria automaticamente uma VLAN privada para você.<br/><br/><strong>Nota</strong>: os roteadores de VLAN privada sempre iniciam com <code>bcr</code> (roteador backend) e os roteadores de VLAN pública sempre iniciam com <code>fcr</code> (roteador frontend). Ao criar um cluster e especificar as VLANs públicas e privadas, o número e a combinação de letras após esses prefixos devem corresponder.</li></ul></td>
        </tr>
        <tr>
        <td><code>--name <em>&lt;name&gt;</em></code></td>
        <td>**Clusters grátis e padrão**: substitua <em>&lt;name&gt;</em> por um nome para seu cluster. O nome deve iniciar com uma letra, pode conter letras, números e hífen (-) e deve ter 35 caracteres ou menos. O nome do cluster e a região na qual o cluster está implementado formam o nome completo do domínio para o subdomínio do Ingress. Para assegurar que o subdomínio do Ingress seja exclusivo dentro de uma região, o nome do cluster pode ser truncado e anexado com um valor aleatório dentro do nome de domínio do Ingress.
</td>
        </tr>
        <tr>
        <td><code>--workers <em>&lt;number&gt;</em></code></td>
        <td>**Clusters padrão**: o número de nós do trabalhador para incluir no cluster. Se a opção <code>--workers</code> não for especificada, um nó do trabalhador será criado.</td>
        </tr>
        <tr>
          <td><code>--kube-version <em>&lt;major.minor.patch&gt;</em></code></td>
          <td>**Clusters padrão**: a versão do Kubernetes para o nó principal do cluster. Esse valor é opcional. Quando a versão não for especificada, o cluster será criado com o padrão de versões do Kubernetes suportadas. Para ver versões disponíveis, execute <code>bx cs kube-versions</code>.
</td>
        </tr>
        <tr>
        <td><code>--disable-disk-encrypt</code></td>
        <td>**Clusters grátis e padrão**: os nós do trabalhador apresentam criptografia de disco por padrão; [saiba mais](cs_secure.html#encrypted_disks). Se desejar desativar a criptografia, inclua esta opção.</td>
        </tr>
        <tr>
        <td><code>--trusted</code></td>
        <td>**Clusters bare metal padrão**: ative [Cálculo confiável](cs_secure.html#trusted_compute) para verificar seus nós do trabalhador bare metal com relação à violação. O Cálculo confiável está disponível para selecionar tipos de máquina bare metal. Por exemplo, os tipos GPU `mgXc` não suportam o Cálculo confiável. Se você não ativar a confiança durante a criação do cluster, mas quiser fazer isso mais tarde, será possível usar o [comando](cs_cli_reference.html#cs_cluster_feature_enable) `bx cs feature-enable`. Depois de ativar a confiança, não é possível desativá-la posteriormente.</td>
        </tr>
        </tbody></table>

7.  Verifique se a criação do cluster foi solicitada.

    ```
    bx cs clusters
    ```
    {: pre}

    **Nota:** para máquinas virtuais, pode levar alguns minutos para as máquinas do nó do trabalhador serem pedidas e para o cluster ser configurado e provisionado em sua conta. As máquinas físicas bare metal são provisionadas pela interação manual com a infraestrutura do IBM Cloud (SoftLayer) e podem levar mais de um dia útil para serem concluídas.

    Quando o fornecimento do cluster é concluído, o status do cluster muda para **implementado**.

    ```
    Name ID State Created Workers Location Version my_cluster paf97e8843e29941b49c598f516de72101 deployed 20170201162433 1 mil01 1.9.7
    ```
    {: screen}

8.  Verifique o status dos nós do trabalhador.

    ```
    bx cs workers <cluster_name_or_ID>
    ```
    {: pre}

    Quando os nós do trabalhador estiverem prontos, o estado mudará para **normal** e o status será **Pronto**. Quando o status do nó for **Pronto**, será possível, então, acessar o cluster.

    **Nota:** a cada nó do trabalhador é designado um ID de nó do trabalhador e um nome de domínio exclusivos que não devem ser mudados manualmente após a criação do cluster. Mudar o ID ou o nome do domínio evita que o mestre do Kubernetes gerencie o cluster.

    ```
    ID Public IP Private IP Machine Type State Status Location Version kube-mil01-paf97e8843e29941b49c598f516de72101-w1 169.xx.xxx.xxx 10.xxx.xx.xxx free normal Ready mil01 1.9.7
    ```
    {: screen}

9. Configure o cluster criado como o contexto para esta sessão. Conclua estas etapas de configuração toda vez que você trabalhar com o seu cluster.
    1.  Obtenha o comando para configurar a variável de ambiente e fazer download dos arquivos de configuração do Kubernetes.

        ```
        bx cs cluster-config <cluster_name_or_ID>
        ```
        {: pre}

        Quando o download dos arquivos de configuração estiver concluído, será exibido um comando que poderá ser usado para configurar o caminho para o seu arquivo de configuração local do Kubernetes como uma variável de ambiente.

        Exemplo para OS X:

        ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

    2.  Copie e cole o comando que é exibido em seu terminal para configurar a variável de ambiente `KUBECONFIG`.
    3.  Verifique se a variável de ambiente `KUBECONFIG` está configurada corretamente.

        Exemplo para OS X:

        ```
        echo $KUBECONFIG
        ```
        {: pre}

        Saída:

        ```
        /Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml

        ```
        {: screen}

10. Ative seu painel do Kubernetes com a porta padrão `8001`.
    1.  Configure o proxy com o número da porta padrão.

        ```
        kubectl proxy
        ```
        {: pre}

        ```
        Iniciando a entrega em 127.0.0.1:8001
        ```
        {: screen}

    2.  Abra a URL a seguir em um navegador da web para ver o painel do Kubernetes.

        ```
        http://localhost:8001/ui
        ```
        {: codeblock}


**O que vem a seguir?**


-   [Implementar um app no cluster.](cs_app.html#app_cli)
-   [Gerenciar seu cluster com a linha de comandos de `kubectl`. ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/reference/kubectl/overview/)
-   [Configure seu próprio registro privado no {{site.data.keyword.Bluemix_notm}} para armazenar e compartilhar imagens do Docker com outros usuários.](/docs/services/Registry/index.html)
- Se você tem múltiplas VLANs para um cluster ou múltiplas sub-redes na mesma VLAN, deve-se [ativar a ampliação de VLAN](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning) para que os nós do trabalhador possam se comunicar entre si na rede privada.
- Se você tiver um firewall, poderá ser necessário [abrir as portas requeridas](cs_firewall.html#firewall) para usar os comandos `bx`, `kubectl` ou `calicotl`, para permitir tráfego de saída do seu cluster ou para permitir tráfego de entrada para serviços de rede.

<br />





## Visualizando estados do cluster
{: #states}

Revise o estado de um cluster do Kubernetes para obter informações sobre a disponibilidade e a capacidade do cluster, além de problemas em potencial que possam ter ocorrido.
{:shortdesc}

Para visualizar informações sobre um cluster específico, como seus locais, URL mestre, subdomínio de Ingresso, versão, proprietário e painel de monitoramento, use o [comando](cs_cli_reference.html#cs_cluster_get) `bx cs cluster-get <cluster_name_or_ID>`. Inclua a sinalização `--showResources` para visualizar mais recursos de cluster, como complementos para os pods de armazenamento ou VLANs de sub-rede para IPs públicos e privados.

É possível visualizar o estado atual do cluster executando o comando `bx cs clusters` e localizando o campo **Estado**. Para solucionar problemas de seu cluster e nós do trabalhador, veja [Resolução de problemas de clusters](cs_troubleshoot.html#debug_clusters).

<table summary="Cada linha da tabela deve ser lida da esquerda para a direita, com o estado do cluster na coluna um e uma descrição na coluna dois.">
<caption>Estados do cluster</caption>
   <thead>
   <th>Estado do cluster</th>
   <th>Descrição</th>
   </thead>
   <tbody>
<tr>
   <td>Interrompido</td>
   <td>A exclusão do cluster é solicitada pelo usuário antes da implementação do mestre do Kubernetes. Depois que a exclusão do cluster é concluída, o cluster é removido do painel. Se o seu cluster estiver preso nesse estado por muito tempo, abra um chamado de suporte do [{{site.data.keyword.Bluemix_notm}}](cs_troubleshoot.html#ts_getting_help).</td>
   </tr>
 <tr>
     <td>Crítico</td>
     <td>O mestre do Kubernetes não pode ser atingido ou todos os nós do trabalhador no cluster estão inativos. </td>
    </tr>
   <tr>
     <td>Exclusão com falha</td>
     <td>O mestre do Kubernetes ou pelo menos um nó do trabalhador não pode ser excluído.  </td>
   </tr>
   <tr>
     <td>Excluído</td>
     <td>O cluster foi excluído, mas ainda não foi removido de seu painel. Se o seu cluster estiver preso nesse estado por muito tempo, abra um chamado de suporte do [{{site.data.keyword.Bluemix_notm}}](cs_troubleshoot.html#ts_getting_help). </td>
   </tr>
   <tr>
   <td>Exclusão</td>
   <td>O cluster está sendo excluído e a infraestrutura de cluster está sendo desmantelada. Não é possível acessar o cluster.  </td>
   </tr>
   <tr>
     <td>Implementação com falha</td>
     <td>A implementação do mestre do Kubernetes não pôde ser concluída. Não é possível resolver esse estado. Entre em contato com o suporte do IBM Cloud abrindo um chamado de suporte do [{{site.data.keyword.Bluemix_notm}}](cs_troubleshoot.html#ts_getting_help).</td>
   </tr>
     <tr>
       <td>Implementando</td>
       <td>O mestre do Kubernetes não está totalmente implementado ainda. Não é possível acessar seu cluster. Aguarde até que seu cluster seja totalmente implementado para revisar seu funcionamento.</td>
      </tr>
      <tr>
       <td>Normal</td>
       <td>Todos os nós do trabalhador em um cluster estão funcionando. É possível acessar o cluster e implementar apps no cluster. Esse estado é considerado saudável e não requer uma ação sua. **Nota**: embora os nós do trabalhador possam ser normais, outros recursos de infraestrutura, como [redes](cs_troubleshoot_network.html) e [armazenamento](cs_troubleshoot_storage.html), ainda podem precisar de atenção.</td>
    </tr>
      <tr>
       <td>Pendente</td>
       <td>O mestre do Kubernetes foi implementado. Os nós do trabalhador estão sendo provisionados e ainda não estão disponíveis no cluster. É possível acessar o cluster, mas não é possível implementar apps no cluster.  </td>
     </tr>
   <tr>
     <td>Solicitado</td>
     <td>Uma solicitação para criar o cluster e pedir a infraestrutura para os nós principal e do trabalhador do Kubernetes é enviada. Quando a implementação do cluster é iniciada, o estado do cluster muda para <code>Deploying</code>. Se o seu cluster estiver preso no estado <code>Requested</code> por muito tempo, abra um chamado de suporte do [{{site.data.keyword.Bluemix_notm}}](cs_troubleshoot.html#ts_getting_help). </td>
   </tr>
   <tr>
     <td>Atualizando</td>
     <td>O servidor da API do Kubernetes executado no mestre do Kubernetes está sendo atualizado para uma nova versão de API do Kubernetes. Durante a atualização, não é possível acessar nem mudar o cluster. Nós do trabalhador, apps e recursos implementados pelo usuário não são modificados e continuarão a ser executados. Aguarde a atualização ser concluída para revisar o funcionamento de seu cluster. </td>
   </tr>
    <tr>
       <td>Avisar</td>
       <td>Pelo menos um nó do trabalhador no cluster não está disponível, mas outros nós do trabalhador estão disponíveis e podem assumir o controle da carga de trabalho. </td>
    </tr>
   </tbody>
 </table>


<br />


## Removendo Clusters
{: #remove}

Os clusters grátis e padrão criados com uma conta pré-paga deverão ser removidos manualmente quando não forem mais necessários, para que não consumam mais recursos.
{:shortdesc}

**Aviso:
**
  - Nenhum backup é criado de seu cluster ou dos dados no armazenamento persistente. A exclusão de um cluster ou do armazenamento persistente é permanente e não pode ser desfeita.
  - Ao remover um cluster, você também remove as sub-redes que foram provisionadas automaticamente durante a criação do cluster e que você criou usando o comando `bx cs cluster-subnet-create`. No entanto, se você tiver incluído manualmente sub-redes existentes no cluster usando o comando `bx cs cluster-subnet-add`, essas sub-redes não serão removidas de sua conta de infraestrutura do IBM Cloud (SoftLayer) e poderão ser reutilizadas em outros clusters.

Antes de iniciar, anote o seu ID do cluster. Você pode precisar do ID do cluster para investigar e remover recursos relacionados à infraestrutura do IBM Cloud (SoftLayer) que não são excluídos automaticamente com seu cluster, como armazenamento persistente.
{: tip}

Para remover um cluster:

-   Por meio da GUI do {{site.data.keyword.Bluemix_notm}}
    1.  Selecione seu cluster e clique em **Excluir** no menu **Mais ações...**.

-   No {{site.data.keyword.Bluemix_notm}} CLI
    1.  Liste os clusters disponíveis.

        ```
        bx cs clusters
        ```
        {: pre}

    2.  Exclua o cluster.

        ```
        bx cs cluster-rm <cluster_name_or_ID>
        ```
        {: pre}

    3.  Siga os prompts e escolha se deseja excluir recursos de cluster, que inclui contêineres, pods, serviços de limite, armazenamento persistente e segredos.
      - **Armazenamento persistente**: o armazenamento persistente fornece alta disponibilidade para seus dados. Se você criou uma solicitação de volume persistente usando um [compartilhamento de arquivo existente](cs_storage.html#existing), não será possível excluir o compartilhamento de arquivo quando excluir o cluster. Deve-se excluir manualmente o compartilhamento de arquivo posteriormente de seu portfólio de infraestrutura do IBM Cloud (SoftLayer).

          **Nota**: devido ao ciclo de faturamento mensal, uma solicitação de volume persistente não pode ser excluída no último dia de um mês. Se você excluir a solicitação de volume persistente no último dia do mês, a exclusão permanecerá pendente até o início do mês seguinte.

Próximas etapas:
- Depois que não estiver mais listado na lista de clusters disponíveis quando você executar o comando `bx cs clusters`, será possível reutilizar o nome de um cluster removido.
- Se você tiver mantido as sub-redes, será possível [reutilizá-las em um novo cluster](cs_subnets.html#custom) ou excluí-las manualmente mais tarde de seu portfólio de infraestrutura do IBM Cloud (SoftLayer).
- Se você tiver mantido o armazenamento persistente, será possível excluir seu armazenamento mais tarde por meio do painel de infraestrutura do IBM Cloud (SoftLayer) na GUI do {{site.data.keyword.Bluemix_notm}}.
