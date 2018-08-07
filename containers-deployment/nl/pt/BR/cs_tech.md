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



# Tecnologia do {{site.data.keyword.containerlong_notm}}

Saiba mais sobre a tecnologia por trás do {{site.data.keyword.containerlong}}.
{:shortdesc}

## Contêineres do Docker
{: #docker_containers}

Construído na tecnologia de contêiner Linux (LXC), o projeto de software livre chamado Docker se tornou uma plataforma de software para construir, testar, implementar e escalar apps rapidamente. O Docker empacota o software em unidades padronizadas, chamadas de contêineres, que incluem todos os elementos que um app precisa para ser executado.
{:shortdesc}

Aprenda alguns conceitos básicos do Docker:

<dl>
<dt>Image</dt>
<dd>Uma imagem do Docker é construída usando um Dockerfile, um arquivo de texto que define como construir a imagem e quais os artefatos de construção a serem incluídos nela, tal como o app, a configuração do app e suas dependências. As imagens sempre são construídas usando outras imagens, tornando-as rápidas de serem configuradas. Deixe alguém fazer a maior parte do trabalho em uma imagem e, em seguida, ajuste-a para seu uso.</dd>
<dt>Registro</dt>
<dd>Um registro de imagem é um lugar para armazenar, recuperar e compartilhar imagens do Docker. As imagens que são armazenadas em um registro podem estar publicamente disponíveis (registro público) ou acessíveis por um pequeno grupo de usuários (registro privado). O {{site.data.keyword.containershort_notm}} oferece imagens públicas, como o ibmliberty, que podem ser usadas para criar seu primeiro app conteinerizado. Quando se trata de aplicativos corporativos, use um registro privado como aquele que é fornecido no {{site.data.keyword.Bluemix_notm}} para proteger suas imagens de serem usadas por usuários não autorizados.
</dd>
<dt>Contêiner</dt>
<dd>Cada contêiner é criado de uma imagem. Um contêiner é um app empacotado com todas as suas dependências, para que o app possa ser movido entre ambientes e ser executado sem mudanças. Ao contrário de máquinas virtuais, os contêineres não virtualizam um dispositivo, seu sistema operacional e o hardware subjacente. Somente o código de app, o tempo de execução,
as ferramentas de sistema, as bibliotecas e as configurações são empacotadas dentro do contêiner. Os contêineres são executados como processos isolados em hosts de cálculo do Ubuntu e compartilham o sistema operacional do host e seus recursos de hardware. Essa abordagem torna um contêiner mais leve, móvel e eficiente do que
uma máquina virtual.</dd>
</dl>



### Benefícios chave do uso de contêineres
{: #container_benefits}

<dl>
<dt>Contêineres são ágeis</dt>
<dd>Os contêineres simplificam a administração do sistema, fornecendo ambientes padronizados para as implementações de desenvolvimento e de produção. Um tempo de execução leve permite aumentar e diminuir rapidamente a capacidade de implementações. Remova a complexidade do gerenciamento de diferentes plataformas de sistema operacional e suas infraestruturas subjacentes, usando contêineres para ajudá-lo a implementar e executar qualquer app em qualquer infraestrutura, de modo rápido e confiável.</dd>
<dt>Contêineres são pequenos</dt>
<dd>É possível ajustar vários contêineres na quantia de espaço que uma única máquina virtual requer.</dd>
<dt>Contêineres são móveis</dt>
<dd>
<ul>
  <li>Reutilize fragmentos de imagens para construir contêineres. </li>
  <li>Mova o código do app rapidamente do ambiente de preparação para o de produção.</li>
  <li>Automatize seus processos com ferramentas de entrega contínua.</li>
  </ul>
  </dd>

<p>Saiba mais sobre [como proteger suas informações pessoais](cs_secure.html#pi) quando trabalhar com imagens de contêiner.</p>

<p>Pronto para obter um conhecimento mais profundo do Docker? <a href="https://developer.ibm.com/courses/all/docker-essentials-extend-your-apps-with-containers/" target="_blank">Saiba como o Docker e o {{site.data.keyword.containershort_notm}} trabalham juntos concluindo este curso.</a></p>

</dl>

<br />


## Clusters do Kubernetes
{: #kubernetes_basics}

<img src="images/certified-kubernetes-resized.png" style="padding-right: 10px;" align="left" alt="Este badge indica a certificação do Kubernetes para o IBM Cloud Container Service."/>O projeto de software livre chamado Kubernetes combina a execução de uma infraestrutura conteinerizada com cargas de trabalho de produção, contribuições de software livre e ferramentas de gerenciamento de contêiner do Docker. A infraestrutura do Kubernetes fornece uma plataforma de app isolado e seguro para gerenciar contêineres que são móveis, extensíveis e com capacidade de recuperação automática em caso de failovers.
{:shortdesc}

Aprenda alguns conceitos básicos do Kubernetes conforme mostrado no diagrama a seguir.

![Configuração de implementação](images/cs_app_tutorial_components1.png)

<dl>
<dt>Conta</dt>
<dd>Sua conta refere-se à sua conta do {{site.data.keyword.Bluemix_notm}}.</dd>

<dt>Grupo</dt>
<dd>Um cluster do Kubernetes consiste em uma ou mais hosts de cálculo que são chamadas nós do trabalhador. Os nós do trabalhador são gerenciados por um mestre do Kubernetes que controla e monitora centralmente todos os recursos
do Kubernetes no cluster. Então quando você implementa os recursos para um app conteinerizado, o mestre do Kubernetes decide em qual nó do trabalhador implementar esses recursos, levando em consideração os requisitos de implementação e a capacidade disponível no cluster. Os recursos do Kubernetes incluem serviços, implementações e pods.</dd>

<dt>Serviço</dt>
<dd>Um serviço é um recurso do Kubernetes que agrupa um conjunto de pods e fornece conectividade de rede a esses pods, sem expor o endereço IP privado real de cada pod. É possível
usar um serviço para tornar seu app disponível no cluster ou na Internet pública.
</dd>

<dt>Implementação</dt>
<dd>Uma implementação é um recurso do Kubernetes em que se pode especificar informações sobre outros recursos ou habilidades que são necessários para executar seu app, tais como serviços, armazenamento persistente ou anotações. Você documenta uma implementação em um arquivo YAML de configuração e, depois, aplica ao cluster. O mestre do Kubernetes configura os recursos e implementa os contêineres em pods nos nós do trabalhador com capacidade disponível.
</br></br>
Defina as estratégias de atualização para seu app, incluindo o número de pods que você deseja incluir durante uma atualização contínua e o número de pods que podem estar indisponíveis por vez. Quando você executar a uma atualização contínua, a implementação verificará se a atualização está funcionando e parará o lançamento quando falhas forem detectadas.</dd>

<dt>Pod</dt>
<dd>Cada app conteinerizado que é implementado em um cluster é implementado, executado e gerenciado por um recurso do Kubernetes que é chamado de pod. Os pods representam pequenas unidades implementáveis em um cluster do Kubernetes e são usados para agrupar os contêineres que devem ser tratados como uma unidade única. Na maioria dos casos, cada contêiner é implementado em seu próprio pod. No entanto, um app pode requerer que um contêiner e outros contêineres auxiliares sejam implementados em um pod para que esses contêineres possam ser direcionados usando o mesmo endereço IP privado.</dd>

<dt>App</dt>
<dd>Um app pode se referir a um app completo ou um componente de um app. Você pode implementar componentes de um app em pods separados ou nós do trabalhador separados.</dd>

<p>Saiba mais sobre [como proteger suas informações pessoais](cs_secure.html#pi) quando trabalhar com recursos do Kubernetes.</p>

<p>Pronto para obter um conhecimento mais profundo do Kubernetes?</p>
<ul><li><a href="cs_tutorials.html#cs_cluster_tutorial" target="_blank">Expanda seu conhecimento de terminologia com o tutorial Criando clusters</a>.</li>
<li><a href="https://developer.ibm.com/courses/all/get-started-kubernetes-ibm-cloud-container-service/" target="_blank">Saiba como o Kubernetes e o {{site.data.keyword.containershort_notm}} trabalham juntos concluindo este curso.</a></li></ul>


</dl>

<br />


## Arquitetura de serviço
{: #architecture}

Em um cluster do Kubernetes que é executado no {{site.data.keyword.containershort_notm}}, seus apps conteinerizados são hospedados em hosts de cálculo que são chamados de nós do trabalhador. Bem, para ser mais específico, os apps são executados em pods e os pods são hospedados em nós do trabalhador. Os nós do trabalhador são gerenciados pelo mestre do Kubernetes. O mestre do Kubernetes e os nós do trabalhador se comunicam entre si por meio de certificados TLS seguros e uma conexão openVPN para orquestrar suas configurações de cluster.
{: shortdesc}

Qual é a diferença entre o mestre do Kubernetes e um nó do trabalhador? Feliz por perguntar.

<dl>
  <dt>Mestre do Kubernetes</dt>
    <dd>O mestre do Kubernetes é encarregado de gerenciar todos os recursos de cálculo, rede e armazenamento no cluster. O mestre do Kubernetes assegura que seus apps e serviços conteinerizados sejam igualmente implementados nos nós do trabalhador no cluster. Dependendo de como você configura seu app e serviços, o mestre determina o nó do trabalhador que tem recursos suficientes para preencher os requisitos do app.</dd>
  <dt>Nó do trabalhador</dt>
    <dd>Cada nó do trabalhador é uma máquina física (bare metal) ou uma máquina virtual que é executada em hardware físico no ambiente de nuvem. Ao provisionar um nó do trabalhador, você determina os recursos que estão disponíveis para os contêineres hospedados nesse nó do trabalhador. Prontos para utilização, os nós do trabalhador são configurados com um mecanismo de Docker gerenciado pela {{site.data.keyword.IBM_notm}}, recursos de cálculo separados, rede e um serviço de volume. Os recursos de segurança integrada fornecem isolamento, capacidades de gerenciamento de recurso e conformidade de segurança do nó do trabalhador.</dd>
</dl>

<p>
<figure>
 <img src="images/cs_org_ov.png" alt="{{site.data.keyword.containerlong_notm}} Arquitetura do Kubernetes">
 <figcaption>Arquitetura do {{site.data.keyword.containershort_notm}}</figcaption>
</figure>
</p>

Quer ver como o {{site.data.keyword.containerlong_notm}} pode ser usado com outros produtos e serviços? Confira algumas das [integrações](cs_integrations.html#integrations).


<br />

