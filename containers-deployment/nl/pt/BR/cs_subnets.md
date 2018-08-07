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




# Configurando sub-redes para clusters
{: #subnets}

Mude o conjunto de endereços IP públicos ou privados móveis disponíveis, incluindo sub-redes em seu cluster do Kubernetes no {{site.data.keyword.containerlong}}.
{:shortdesc}

No {{site.data.keyword.containershort_notm}}, é possível incluir endereços IP móveis e estáveis para serviços do Kubernetes, incluindo sub-redes de rede para o cluster. Nesse caso, as sub-redes não estão sendo usadas com netmasking para criar a conectividade entre um ou mais clusters. Em vez disso, as sub-redes são usadas para fornecer endereços IP fixos permanentes para um serviço de um cluster que pode ser usado para acessar esse serviço.

<dl>
  <dt>A criação de um cluster inclui a criação de uma sub-rede por padrão</dt>
  <dd>Quando você cria um cluster padrão, o {{site.data.keyword.containershort_notm}} provisiona automaticamente as redes a seguir:
    <ul><li>Uma sub-rede pública primária que determina os endereços IP públicos para nós do trabalhador durante a criação de cluster</li>
    <li>Uma sub-rede privada primária que determina endereços IP privados para nós do trabalhador durante a criação do cluster</li>
    <li>Uma sub-rede pública móvel que fornece 5 endereços IP públicos para os serviços de rede do Ingress e do balanceador de carga</li>
    <li>Uma sub-rede privada móvel que fornece 5 endereços IP privados para os serviços de rede do Ingress e do balanceador de carga</li></ul>
      Os endereços IP públicos e privados móveis são estáticos e não mudam quando um nó do trabalhador é removido. Para cada sub-rede, um endereço IP público móvel e um endereço IP privado móvel serão usados para os [balanceadores de carga do aplicativo Ingress](cs_ingress.html) padrão. É possível usar o balanceador de carga do aplicativo de Ingresso para expor múltiplos apps em seu cluster. Os outros quatro endereços IP públicos e privados móveis podem ser usados para expor apps únicos à rede pública ou privada [criando um serviço de balanceador de carga](cs_loadbalancer.html).</dd>
  <dt>[Solicitando e gerenciando suas próprias sub-redes existentes](#custom)</dt>
  <dd>É possível solicitar e gerenciar sub-redes móveis existentes em sua conta de infraestrutura do IBM Cloud (SoftLayer) em vez de usar as sub-redes provisionadas automaticamente. Use essa opção para reter endereços IP estáticos estáveis em remoções e criações de cluster ou para pedir blocos maiores de endereços IP. Primeiro, crie um cluster sem sub-redes usando o comando `cluster-create --no-subnet` e, em seguida, inclua a sub-rede no cluster com o comando `cluster-subnet-add`. </dd>
</dl>

**Nota:** os endereços IP públicos móveis são cobrados mensalmente. Se você remover os endereços IP públicos móveis depois que o cluster for provisionado, ainda terá que pagar o encargo mensal, mesmo se os tiver usado apenas por um curto período de tempo.

## Solicitando sub-redes adicionais para seu cluster
{: #request}

É possível incluir endereços IP públicos móveis, privados ou estáveis no cluster designando sub-redes ao cluster.
{:shortdesc}

**Nota:** quando você disponibiliza uma sub-rede para um cluster, os endereços IP dessa sub-rede são usados para propósitos de rede do cluster. Para evitar conflitos de endereço IP, certifique-se de usar uma sub-rede com somente um cluster. Não use uma sub-rede para múltiplos clusters ou para outros
propósitos fora do {{site.data.keyword.containershort_notm}} ao mesmo
tempo.

Antes de iniciar, [destine sua CLI](cs_cli_install.html#cs_cli_configure) para seu cluster.

Para criar uma sub-rede em uma conta de infraestrutura do IBM Cloud (SoftLayer) e torná-la disponível para um cluster especificado:

1. Provisione uma nova sub-rede.

    ```
    bx cs cluster-subnet-create <cluster_name_or_id> <subnet_size> <VLAN_ID>
    ```
    {: pre}

    <table>
    <caption>Entendendo os componentes deste comando</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes desse comando</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-subnet-create</code></td>
    <td>O comando para provisionar uma sub-rede para seu cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;cluster_name_or_id&gt;</em></code></td>
    <td>Substitua <code>&lt;cluster_name_or_id&gt;</code> pelo nome ou ID do cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;subnet_size&gt;</em></code></td>
    <td>Substitua <code>&lt;subnet_size&gt;</code> pelo número de endereços IP que você deseja incluir de sua sub-rede móvel. Os valores aceitos são 8, 16, 32 ou 64. <p>**Nota:** quando você inclui endereços IP móveis para sua sub-rede, três endereços IP são usados para estabelecer a rede interna do cluster. Não é possível usar esses três endereços IP para seu balanceador de carga do aplicativo ou para criar um serviço de balanceador de carga. Por exemplo, se você solicitar oito endereços IP públicos móveis, será possível usar cinco deles para expor os seus apps ao público.</p> </td>
    </tr>
    <tr>
    <td><code><em>&lt;VLAN_ID&gt;</em></code></td>
    <td>Substitua <code>&lt;VLAN_ID&gt;</code> pelo ID da VLAN pública ou privada na qual você deseja alocar os endereços IP móveis públicos ou privados. Deve-se selecionar a VLAN pública ou privada à qual um nó do trabalhador existente está conectado. Para revisar a VLAN pública ou a VLAN privada para um nó do trabalhador, execute o comando <code>bx cs worker-get &lt;worker_id&gt;</code>. </td>
    </tr>
    </tbody></table>

2.  Verifique se a sub-rede foi criada com sucesso e se foi incluída em seu cluster. A sub-rede CIDR é listada na seção **VLANs**.

    ```
    bx cs cluster-get --showResources <cluster_name_or_ID>
    ```
    {: pre}

3. Opcional: [Ative o roteamento entre as sub-redes na mesma VLAN](#vlan-spanning).

<br />


## Incluindo ou reutilizando sub-redes customizadas e existentes em clusters do Kubernetes
{: #custom}

É possível incluir sub-redes públicas ou privadas móveis existentes no cluster do Kubernetes ou reutilizar sub-redes de um cluster excluído.
{:shortdesc}

Antes de iniciar,
- [Destine sua CLI](cs_cli_install.html#cs_cli_configure) para seu cluster.
- Para reutilizar sub-redes de um cluster que você não precisa mais, exclua o cluster desnecessário. As sub-redes são excluídas dentro de 24 horas.

   ```
   Bx cs cluster-rm < cluster_name_or_ID
   ```
   {: pre}

Para usar uma sub-rede existente no portfólio da infraestrutura do IBM Cloud (SoftLayer) com regras de firewall customizado ou endereços IP disponíveis:

1.  Identifique a sub-rede a ser usada. Observe o ID da sub-rede e o ID da VLAN. Neste exemplo, o ID da sub-rede é `1602829` e o ID da VLAN é `2234945`.

    ```
    bx cs subnets
    ```
    {: pre}

    ```
    Getting subnet list...
    OK
    ID        Network             Gateway          VLAN ID   Type      Bound Cluster
    1550165   10.xxx.xx.xxx/26    10.xxx.xx.xxx    2234947   private
    1602829   169.xx.xxx.xxx/28   169.xx.xxx.xxx   2234945   public

    ```
    {: screen}

2.  Confirme a localização da VLAN. Neste exemplo, o local é dal10.

    ```
    bx cs vlans dal10
    ```
    {: pre}

    ```
    Getting VLAN list...
    OK
    ID        Name   Number   Type      Router         Supports Virtual Workers
    2234947          1813     private   bcr01a.dal10   true
    2234945          1618     public    fcr01a.dal10   true
    ```
    {: screen}

3.  Crie um cluster usando o local e o ID da VLAN identificados. Para reutilizar uma sub-rede existente, inclua a sinalização `--no-subnet` para evitar que uma nova sub-rede IP pública móvel e uma nova sub-rede IP privada móvel seja criada automaticamente.

    ```
    bx cs cluster-create --location dal10 --machine-type u2c.2x4 --no-subnet --public-vlan 2234945 --private-vlan 2234947 --workers 3 --name my_cluster
    ```
    {: pre}

4.  Verifique se a criação do cluster foi solicitada.

    ```
    bx cs clusters
    ```
    {: pre}

    **Nota:** pode levar até 15 minutos para que as máquinas do nó do trabalhador sejam ordenadas e para que o cluster seja configurado e provisionado em sua conta.

    Quando o fornecimento do cluster é concluído, o status do cluster muda para **implementado**.

    ```
    Name         ID                                   State      Created          Workers   Location   Version
    mycluster    aaf97a8843a29941b49a598f516da72101   deployed   20170201162433   3         dal10      1.9.7
    ```
    {: screen}

5.  Verifique o status dos nós do trabalhador.

    ```
    bx cs workers <cluster>
    ```
    {: pre}

    Quando os nós do trabalhador estiverem prontos, o estado mudará para **normal** e o status será **Pronto**. Quando o status do nó for **Pronto**, será possível, então, acessar o cluster.

    ```
    ID                                                  Public IP        Private IP     Machine Type   State      Status   Location   Version
    prod-dal10-pa8dfcc5223804439c87489886dbbc9c07-w1    169.xx.xxx.xxx   10.xxx.xx.xxx  free           normal     Ready    dal10      1.9.7
    ```
    {: screen}

6.  Inclua a sub-rede em seu cluster especificando o ID da sub-rede. Quando você disponibiliza uma sub-rede para um cluster, um configmap do Kubernetes é criado incluindo todos os endereços IP públicos móveis disponíveis que podem ser usados. Se nenhum balanceador de carga de aplicativo existe para seu cluster, um endereço IP público móvel e um privado móvel são usados automaticamente para criar os balanceadores de carga de aplicativo público e privado. Todos os outros endereços IP públicos e privados móveis podem ser usados para criar serviços de balanceador de carga para seus apps.

    ```
    bx cs cluster-subnet-add mycluster 807861
    ```
    {: pre}

7. Opcional: [Ative o roteamento entre as sub-redes na mesma VLAN](#vlan-spanning).

<br />


## Incluindo sub-redes gerenciadas por usuário e endereços IP para clusters do Kubernetes
{: #user_managed}

Forneça uma sub-rede de uma rede no local que você deseja que o {{site.data.keyword.containershort_notm}} acesse. Em seguida, é possível incluir endereços IP privados dessa sub-rede para serviços de balanceador de carga em seu cluster do Kubernetes.
{:shortdesc}

Requisitos:
- Sub-redes gerenciadas pelo usuário podem ser incluídas em VLANs privadas apenas.
- O limite de comprimento de prefixo de sub-rede é /24 para /30. Por exemplo, `169.xx.xxx.xxx/24` especifica 253 endereços IP privados utilizáveis, enquanto `169.xx.xxx.xxx/30` especifica 1 endereço IP privado utilizável.
- O primeiro endereço IP na sub-rede deve ser usado como o gateway para a sub-rede.

Antes de iniciar:
- Configure o roteamento de tráfego de rede dentro e fora da sub-rede externa.
- Confirme se você tem conectividade VPN entre o gateway de rede do data center no local e o Virtual Router Appliance de rede privada ou o serviço VPN do strongSwan que é executado em seu cluster. Para obter mais informações, veja [Configurando a conectividade VPN](cs_vpn.html).

Para incluir uma sub-rede de uma rede no local:

1. Visualize o ID de VLAN privada do seu cluster. Localize a seção **VLANs**. No campo **Gerenciado pelo usuário**, identifique o ID da VLAN com _false_.

    ```
    bx cs cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
    VLANs
    VLAN ID   Subnet CIDR       Public   User-managed
    2234947   10.xxx.xx.xxx/29  false    false
    2234945   169.xx.xxx.xxx/29 true     false
    ```
    {: screen}

2. Inclua a sub-rede externa em sua VLAN privada. Os endereços IP privados móveis são incluídos no configmap do cluster.

    ```
    bx cs cluster-user-subnet-add <cluster_name> <subnet_CIDR> <VLAN_ID>
    ```
    {: pre}

    Exemplo:

    ```
    Bx cs cluster-user-subnet-add mycluster 10.xxx.xx.xxx/ 24 2234947
    ```
    {: pre}

3. Verifique se a sub-rede fornecida pelo usuário foi incluída. O campo **Gerenciado pelo usuário** é _true_.

    ```
    bx cs cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
    VLANs
    VLAN ID   Subnet CIDR       Public   User-managed
    2234947   10.xxx.xx.xxx/29  false    false
    2234945   169.xx.xxx.xxx/29 true   false
    2234947   10.xxx.xx.xxx/24  false    true
    ```
    {: screen}

4. Opcional: [Ative o roteamento entre as sub-redes na mesma VLAN](#vlan-spanning).

5. Inclua um serviço de balanceador de carga privado ou um balanceador de carga privado do aplicativo Ingress para acessar seu app na rede privada. Para usar um endereço IP privado da sub-rede que você incluiu, deve-se especificar um endereço IP. Caso contrário, um endereço IP será escolhido aleatoriamente das sub-redes de infraestrutura do IBM Cloud (SoftLayer) ou sub-redes fornecidas pelo usuário na VLAN privada. Para obter mais informações, veja [Ativando o acesso público ou privado a um app usando um serviço LoadBalancer](cs_loadbalancer.html#config) ou [Ativando o balanceador de carga de aplicativo privado](cs_ingress.html#private_ingress).

<br />


## Gerenciando endereços IP e sub-redes
{: #manage}

Revise as opções a seguir para listar endereços IP públicos disponíveis, liberando os endereços IP usados e roteando entre múltiplas sub-redes na mesma VLAN.
{:shortdesc}

### Visualizando endereços IP públicos móveis disponíveis
{: #review_ip}

Para listar todos os endereços IP em seu cluster, usados e disponíveis, é possível executar:

  ```
  kubectl get cm ibm-cloud-provider-vlan-ip-config -n kube-system -o yaml
  ```
  {: pre}

Para listar somente endereços IP públicos disponíveis para o seu cluster, é possível usar as etapas a seguir:

Antes de iniciar, [configure o contexto para o cluster que você deseja usar.](cs_cli_install.html#cs_cli_configure)

1.  Crie um arquivo de configuração de serviço do Kubernetes que seja chamado `myservice.yaml` e defina um serviço do tipo `LoadBalancer` com um endereço IP do balanceador de carga simulado. O exemplo a seguir usa o endereço IP 1.1.1.1 como o endereço IP do balanceador de carga.

    ```
    apiVersion: v1
    kind: Service
    metadata:
      labels:
        run: myservice
      name: myservice
      namespace: default
    spec:
      ports:
      - port: 80
        protocol: TCP
        targetPort: 80
      selector:
        run: myservice
      sessionAffinity: None
      type: LoadBalancer
      loadBalancerIP: 1.1.1.1
    ```
    {: codeblock}

2.  Crie o serviço em seu cluster.

    ```
    kubectl apply -f myservice.yaml
    ```
    {: pre}

3.  Inspecione o serviço.

    ```
    kubectl describe service myservice
    ```
    {: pre}

    **Nota:** a criação desse serviço falha porque o mestre do Kubernetes não pode localizar o endereço IP do balanceador de carga especificado no configmap do Kubernetes. Quando você executa esse comando, é possível ver a mensagem de erro e a lista de endereços IP públicos disponíveis para o cluster.

    ```
    Error on cloud load balancer a8bfa26552e8511e7bee4324285f6a4a for service default/myservice with UID 8bfa2655-2e85-11e7-bee4-324285f6a4af: Requested cloud provider IP 1.1.1.1 is not available. Os endereços IP do provedor em nuvem a seguir estão disponíveis: <list_of_IP_addresses>
    ```
    {: screen}

### Liberando os endereços IP usados
{: #free}

É possível liberar um endereço IP móvel usado excluindo o serviço de balanceador de carga que está usando o endereço IP móvel.
{:shortdesc}

Antes de iniciar, [configure o contexto para o cluster que você deseja usar.](cs_cli_install.html#cs_cli_configure)

1.  Liste os serviços disponíveis em seu cluster.

    ```
    kubectl get services
    ```
    {: pre}

2.  Remova o serviço de balanceador de carga que usa um endereço IP público ou privado.

    ```
    kubectl delete service <service_name>
    ```
    {: pre}

### Ativando o roteamento entre sub-redes na mesma VLAN
{: #vlan-spanning}

Ao criar um cluster, uma sub-rede que termina em `/26` é provisionada na mesma VLAN em que o cluster está. Essa sub-rede primária pode conter até 62 nós do trabalhador.
{:shortdesc}

Esse limite de 62 nós do trabalhador pode ser excedido por um cluster grande ou por vários clusters menores em uma única região que estão na mesma VLAN. Quando o limite de 62 nós do trabalhador é atingido, uma segunda sub-rede primária na mesma VLAN é solicitada.

Para rotear entre sub-redes na mesma VLAN, deve-se ativar a ampliação de VLAN. Para obter instruções, veja [Ativar ou desativar a ampliação de VLAN](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning).
