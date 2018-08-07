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



# Planejando a rede com os serviços NodePort, LoadBalancer ou Ingress
{: #planning}

Ao criar um cluster do Kubernetes no {{site.data.keyword.containerlong}}, cada cluster deve ser conectado a uma VLAN pública. A VLAN pública
determina o endereço IP público que é designado a um nó do trabalhador durante a criação de cluster.
{:shortdesc}

A interface de rede pública para os nós do trabalhador nos clusters grátis e padrão é protegida por políticas de rede do Calico. Essas políticas bloqueiam a maior parte do tráfego de entrada por padrão. No entanto, o tráfego de entrada que é necessário para o Kubernetes funcionar é permitido, assim como conexões para os serviços NodePort, LoadBalancer e Ingresso. Para obter mais informações sobre essas políticas, incluindo como modificá-las, veja [Políticas de rede](cs_network_policy.html#network_policies).

|Tipo de cluster|Gerenciador da VLAN pública para o cluster|
|------------|------------------------------------------|
|Clusters grátis|{{site.data.keyword.IBM_notm}}|
|Clusters padrão|Você em sua conta de infraestrutura do IBM Cloud (SoftLayer)|
{: caption="Gerenciadores da VLANs públicas por tipo de cluster" caption-side="top"}

Para obter mais informações sobre a comunicação de rede em cluster entre os nós do trabalhador e os pods, veja [Rede em cluster](cs_secure.html#in_cluster_network). Para obter mais informações sobre como conectar com segurança apps que são executados em um cluster do Kubernetes a uma rede no local ou a apps que são externos ao seu cluster, veja [Configurando a conectividade da VPN](cs_vpn.html).

## Permitindo o acesso público a apps
{: #public_access}

Para tornar um app publicamente disponível na Internet, deve-se atualizar seu arquivo de configuração antes de implementar o app em um cluster.
{:shortdesc}

*Plano de dados do Kubernetes no {{site.data.keyword.containershort_notm}}*

![{{site.data.keyword.containerlong_notm}} Arquitetura do Kubernetes](images/networking.png)

O diagrama mostra como o Kubernetes transporta o tráfego de rede do usuário em {{site.data.keyword.containershort_notm}}. Para tornar o seu app acessível na Internet, as maneiras variam dependendo se você criou um cluster grátis ou padrão.

<dl>
<dt><a href="cs_nodeport.html#planning" target="_blank">Serviço NodePort</a> (clusters grátis e padrão)</dt>
<dd>
 <ul>
  <li>Exponha uma porta pública em cada nó do trabalhador e use o endereço IP público de qualquer nó do trabalhador para acessar publicamente seu serviço no cluster.</li>
  <li>Iptables é um recurso de kernel Linux que balanceia a carga de solicitações nos pods do app, fornece roteamento de rede de alto desempenho e fornece controle de acesso à rede.</li>
  <li>O endereço IP público do nó do trabalhador não é permanente. Quando um nó do trabalhador é removido ou recriado, um novo endereço IP público é designado ao
nó do trabalhador.</li>
  <li>O serviço NodePort é ótimo para testar o acesso público. Ele também poderá ser usado se você precisar de acesso público somente por um curto período de tempo.</li>
 </ul>
</dd>
<dt><a href="cs_loadbalancer.html#planning" target="_blank">Serviço LoadBalancer</a> (somente clusters padrão)</dt>
<dd>
 <ul>
  <li>Cada cluster padrão é provisionado com quatro endereços IP públicos móveis e quatro privados móveis que podem ser usados para criar um balanceador de carga TCP/UDP externo para seu app.</li>
  <li>Iptables é um recurso de kernel Linux que balanceia a carga de solicitações nos pods do app, fornece roteamento de rede de alto desempenho e fornece controle de acesso à rede.</li>
  <li>O endereço IP público móvel que é designado para o balanceador de carga é permanente e não muda quando um nó do trabalhador é recriado no cluster.</li>
  <li>É possível customizar seu balanceador de carga expondo qualquer porta que seu app requer.</li></ul>
</dd>
<dt><a href="cs_ingress.html#planning" target="_blank">Ingresso</a> (somente clusters padrão)</dt>
<dd>
 <ul>
  <li>Exponha múltiplos apps em um cluster criando um balanceador de carga HTTP ou HTTPS, TCP ou UDP externo. O balanceador de carga usa um ponto de entrada público protegido e exclusivo para rotear solicitações recebidas para seus apps.</li>
  <li>É possível usar uma rota pública para expor múltiplos apps em seu cluster como serviços.</li>
  <li>O Ingress consiste em dois componentes:
   <ul>
    <li>O recurso de Ingresso define as regras de como rotear e balancear a carga de solicitações recebidas para um app.</li>
    <li>O balanceador de carga do aplicativo (ALB) atende às solicitações de serviço HTTP ou HTTPS, TCP ou UDP recebidas. Ele encaminha as solicitações em pods dos apps com base nas regras que você definiu no recurso de Ingresso.</li>
   </ul>
  <li>Use o Ingresso para implementar seu próprio ALB com regras de roteamento customizado e se precisar de finalização SSL para seus apps.</li>
 </ul>
</dd></dl>

Para escolher a melhor opção de rede para seu aplicativo, é possível seguir esta árvore de decisão. Para obter informações de planejamento e instruções de configuração, clique na opção de serviço de rede que você escolher.

<img usemap="#networking_map" border="0" class="image" src="images/networkingdt.png" width="500px" alt="Esta imagem orienta na escolha da melhor opção de rede para seu aplicativo. Se esta imagem não estiver sendo exibida, a informação ainda poderá ser encontrada na documentação." style="width:500px;" />
<map name="networking_map" id="networking_map">
<area href="/docs/containers/cs_nodeport.html" alt="Serviço Nodeport" shape="circle" coords="52, 283, 45"/>
<area href="/docs/containers/cs_loadbalancer.html" alt="Serviço LoadBalancer" shape="circle" coords="247, 419, 44"/>
<area href="/docs/containers/cs_ingress.html" alt="Serviço Ingress" shape="circle" coords="445, 420, 45"/>
</map>
