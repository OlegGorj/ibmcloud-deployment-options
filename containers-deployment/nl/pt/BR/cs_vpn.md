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


# Configurando a conectividade de VPN
{: #vpn}

Com a conectividade VPN, é possível conectar apps com segurança em um cluster do Kubernetes no {{site.data.keyword.containerlong}} a uma rede no local. Também é possível conectar apps que são externos ao seu cluster para um app que está em execução dentro de seu cluster.
{:shortdesc}

Para conectar seus nós do trabalhador e apps a um data center no local, é possível configurar uma das opções a seguir.

- **Serviço de VPN strongSwan IPSec**: é possível configurar um [Serviço de VPN strongSwan IPSec ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.strongswan.org/) que conecte de forma segura seu cluster do Kubernetes a uma rede no local. O serviço strongSwan IPSec VPN fornece um canal de comunicação seguro de ponta a ponta na Internet, que se baseia no conjunto de protocolo Internet Protocol Security (IPsec) padrão de mercado. Para configurar uma conexão segura entre seu cluster e uma rede no local, [configure e implemente o serviço VPN IPSec do strongSwan](#vpn-setup) diretamente em um pod no cluster.

- **Virtual Router Appliance (VRA) ou Fortigate Security Appliance (FSA)**: você pode escolher configurar um [VRA](/docs/infrastructure/virtual-router-appliance/about.html) ou [FSA](/docs/infrastructure/fortigate-10g/about.html) para configurar um terminal de VPN IPSec. Essa opção é útil quando se tem um cluster maior e se deseja acessar recursos não Kubernetes por meio da VPN ou acessar múltiplos clusters por meio de uma única VPN. Para configurar um VRA, veja [Configurando a conectividade VPN com o VRA](#vyatta).

## Configurando a conectividade de VPN com o gráfico Helm do serviço de VPN strongSwan IPSec
{: #vpn-setup}

Use um gráfico Helm para configurar e implementar o serviço de VPN strongSwan IPSec dentro de um pod do Kubernetes.
{:shortdesc}

Como o strongSwan está integrado ao cluster, não é necessário um dispositivo de gateway externo. Quando a conectividade de VPN é estabelecida, as rotas são configuradas automaticamente em todos os nós do trabalhador no cluster. Essas rotas permitem a conectividade bidirecional por meio do túnel VPN entre pods em qualquer nó do trabalhador e o sistema remoto. Por exemplo, o diagrama a seguir mostra como um app no {{site.data.keyword.containershort_notm}} pode se comunicar com um servidor local por meio de uma conexão VPN do strongSwan:

<img src="images/cs_vpn_strongswan.png" width="700" alt="Expor um app no {{site.data.keyword.containershort_notm}} usando um balanceador de carga" style="width:700px; border-style: none"/>

1. Um app em seu cluster, `myapp`, recebe uma solicitação de um serviço Ingress ou LoadBalancer e precisa conectar-se com segurança a dados em sua rede local.

2. A solicitação para o data center local é encaminhada para o pod VPN do strongSwan do IPSec. O endereço IP de destino é usado para determinar quais pacotes de rede enviar para o pod VPN do strongSwan do IPSec.

3. A solicitação é criptografada e enviada pelo túnel VPN para o data center no local.

4. A solicitação recebida passa pelo firewall no local e é entregue ao terminal de túnel VPN (roteador) no qual ela é decriptografada.

5. O terminal de túnel VPN (roteador) encaminha a solicitação para o servidor ou mainframe no local, dependendo do endereço IP de destino que foi especificado na etapa 2. Os dados necessários são enviados de volta na conexão VPN para `myapp` por meio do mesmo processo.

### Configurar o gráfico Helm do strongSwan
{: #vpn_configure}

Antes de iniciar:
* [Instale um gateway de VPN IPsec no data center local](/docs/infrastructure/iaas-vpn/set-up-ipsec-vpn.html#setting-up-an-ipsec-connection).
* [Crie um cluster padrão](cs_clusters.html#clusters_cli) ou [atualize um cluster existente para a versão 1.7.16 ou mais recente](cs_cluster_update.html#master).
* O cluster deve ter pelo menos um endereço IP público disponível do balanceador de carga. [É possível verificar seus endereços IP públicos disponíveis](cs_subnets.html#manage) ou [liberar um endereço IP usado](cs_subnets.html#free).
* [Destine a CLI do Kubernetes para o cluster](cs_cli_install.html#cs_cli_configure).

Para obter mais informações sobre os comandos do Helm que são usados para configurar o gráfico strongSwan, veja a <a href="https://docs.helm.sh/helm/" target="_blank">documentação do Helm <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>.

Para configurar o gráfico Helm:

1. [Instale o Helm para seu cluster e inclua o repositório do {{site.data.keyword.Bluemix_notm}} em sua instância do Helm](cs_integrations.html#helm).

2. Salve as definições de configuração padrão para o gráfico Helm do strongSwan em um arquivo YAML local.

    ```
    helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. Abra o arquivo `config.yaml` e faça as mudanças a seguir para os valores padrão de acordo com a configuração de VPN que você desejar. É possível localizar descrições para configurações mais avançadas nos comentários do arquivo de configuração.

    **Importante**: se não for necessário mudar uma propriedade, deixe um comentário nessa propriedade, colocando um `#` na frente dela.

    <table>
    <caption>Entendendo os componentes deste YAML</caption>
    <col width="22%">
    <col width="78%">
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes de arquivo do YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>localSubnetNAT</code></td>
    <td>A Conversão de Endereço de Rede (NAT) para sub-redes fornece uma solução alternativa para conflitos de sub-rede entre as redes locais e no local. É possível usar o NAT para remapear as sub-redes de IP local privado, a sub-rede do pod (172.30.0.0/16) ou a sub-rede do serviço de pod (172.21.0.0/16) para uma sub-rede privada diferente. O túnel VPN vê as sub-redes de IP remapeadas em vez das sub-redes originais. O remapeamento acontece antes de os pacotes serem enviados pelo túnel VPN, bem como após os pacotes chegarem do túnel VPN. É possível expor as sub-redes remapeada e não remapeada ao mesmo tempo pela VPN.<br><br>Para ativar o NAT, é possível incluir uma sub-rede inteira ou endereços IP individuais. Se você inclui uma sub-rede inteira (no formato <code>10.171.42.0/24=10.10.10.0/24</code>), o remapeamento é 1 para 1: todos os endereços IP na sub-rede de rede interna são mapeados para a sub-rede de rede externa e vice-versa. Se você incluir endereços IP individuais (no formato <code>10.171.42.17/32=10.10.10.2/32, 10.171.42.29/32=10.10.10.3/32</code>), somente esses endereços IP internos serão mapeados para os endereços IP externos especificados.<br><br>Se você usa essa opção, a sub-rede local exposta pela conexão VPN é a sub-rede "externa" para a qual a sub-rede "interna" está sendo mapeada.
    </td>
    </tr>
    <tr>
    <td><code>loadBalancerIP</code></td>
    <td>Se você quiser especificar um endereço IP público móvel para o serviço VPN do strongSwan para conexões VPN de entrada, inclua esse endereço IP. Especificar um endereço IP é útil quando você precisa de um endereço IP estável, como quando se deve designar quais endereços IP são permitidos por meio de um firewall no local.<br><br>Para visualizar endereços IP públicos móveis disponíveis designados a esse cluster, veja [Gerenciando endereços IP e sub-redes](cs_subnets.html#manage). Se você deixar essa configuração em branco, um endereço IP público móvel gratuito será usado. Se a conexão VPN for iniciada no gateway local (<code>ipsec.auto</code> é configurado como <code>add</code>), será possível usar essa propriedade para configurar um endereço IP público persistente no gateway local para o cluster.</td>
    </tr>
    <tr>
    <td><code>connectUsingLoadBalancerIP</code></td>
    <td>Use o endereço IP do balanceador de carga que foi incluído no <code>loadBalancerIP</code> para também estabelecer a conexão VPN de saída. Se essa opção estiver ativada, todos os nós do trabalhador do cluster deverão estar na mesma VLAN pública. Caso contrário, deve-se usar a configuração <code>nodeSelector</code> para assegurar que o pod de VPN seja implementado em um nó do trabalhador na mesma VLAN pública que a do <code>loadBalancerIP</code>. Essa opção será ignorada se <code>ipsec.auto</code> estiver configurado como <code>add</code>.<p>Valores aceitos:</p><ul><li><code>"false"</code>: não conecte a VPN usando o IP do balanceador de carga. O endereço IP público do nó do trabalhador em que o pod de VPN está em execução é usado no lugar.</li><li><code>"true"</code>: estabeleça a VPN usando o IP do balanceador de carga como o IP de origem local. Se <code>loadBalancerIP</code> não estiver configurado, o endereço IP externo designado ao serviço de balanceador de carga será usado.</li><li><code>"auto"</code>: quando <code>ipsec.auto</code> é configurado como <code>start</code> e <code>loadBalancerIP</code> é configurado, estabeleça a VPN usando o IP do balanceador de carga como o IP de origem local.</li></ul></td>
    </tr>
    <tr>
    <td><code>nodeSelector</code></td>
    <td>Para limitar em quais nós o pod de VPN do strongSwan é implementado, inclua o endereço IP de um nó do trabalhador específico ou um rótulo de nó do trabalhador. Por exemplo, o valor <code>kubernetes.io/hostname: 10.xxx.xx.xxx</code> restringe o pod de VPN para execução nesse nó do trabalhador apenas. O valor <code>strongswan: vpn</code> restringe o pod de VPN à execução em quaisquer nós do trabalhador com esse rótulo. É possível usar qualquer rótulo de nó do trabalhador, mas é recomendado usar: <code>strongswan: &lt;release_name&gt;</code> para que diferentes nós do trabalhador possam ser usado com diferentes implementações deste gráfico.<br><br>Se a conexão VPN é iniciada pelo cluster (<code>ipsec.auto</code> é configurado para <code>start</code>), é possível usar essa propriedade para limitar os endereços IP de origem da conexão VPN que são expostos para o gateway no local. Esse valor é opcional.</td>
    </tr>
    <tr>
    <td><code>ipsec.keyexchange</code></td>
    <td>Se seu terminal de túnel VPN no local não suporta o <code>ikev2</code> como um protocolo para inicializar a conexão, mude esse valor para <code>ikev1</code> ou <code>ike</code>.</td>
    </tr>
    <tr>
    <td><code>ipsec.esp</code></td>
    <td>Inclua a lista de algoritmos ESP de criptografia e autenticação que seu terminal de túnel VPN local usa para a conexão.<ul><li>Se <code>ipsec.keyexchange</code> estiver configurado como <code>ikev1</code>, esta configuração deverá ser especificada.</li><li>Se <code>ipsec.keyexchange</code> estiver configurado como <code>ikev2</code>, esta configuração será opcional. Se você deixar essa configuração em branco, os algoritmos padrão do strongSwan <code>aes128-sha1,3des-sha1</code> serão usados para a conexão.</li></ul></td>
    </tr>
    <tr>
    <td><code>ipsec.ike</code></td>
    <td>Inclua a lista de algoritmos IKE/ISAKMP SA de criptografia e autenticação que seu terminal de túnel VPN local usa para a conexão.<ul><li>Se <code>ipsec.keyexchange</code> estiver configurado como <code>ikev1</code>, esta configuração deverá ser especificada.</li><li>Se <code>ipsec.keyexchange</code> estiver configurado como <code>ikev2</code>, esta configuração será opcional. Se você deixar essa configuração em branco, os algoritmos padrão do strongSwan <code>aes128-sha1-modp2048,3des-sha1-modp1536</code> serão usados para a conexão.</li></ul></td>
    </tr>
    <tr>
    <td><code>ipsec.auto</code></td>
    <td>Se você deseja que o cluster inicie a conexão VPN, mude esse valor para <code>start</code>.</td>
    </tr>
    <tr>
    <td><code>local.subnet</code></td>
    <td>Mude esse valor para a lista de CIDRs de sub-rede de cluster para expor ao longo da conexão VPN à rede no local. Essa lista pode incluir as seguintes sub-redes: <ul><li>O CIDR de sub-rede de pod do Kubernetes: <code>172.30.0.0/16</code></li><li>O CIDR de sub-rede do serviço do Kubernetes: <code>172.21.0.0/16</code></li><li>Se seus apps são expostos por um serviço NodePort na rede privada, o CIDR de sub-rede privada do nó do trabalhador. Recupere os três primeiros octetos do endereço IP privado de seu trabalhador executando <code>bx cs worker &lt;cluster_name&gt;</code>. Por exemplo, se for <code>&lt;10.176.48.xx&gt;</code>, anote <code>&lt;10.176.48&gt;</code>. Em seguida, obtenha o CIDR da sub-rede privada do trabalhador executando o comando a seguir, substituindo <code>&lt;xxx.yyy.zz&gt;</code> pelo octeto recuperado anteriormente: <code>bx cs subnets | grep &lt;xxx.yyy.zzz&gt;</code>.</li><li>Se você tem aplicativos que são expostos por serviços LoadBalancer na rede privada, os CIDRs de sub-rede privada ou gerenciada pelo usuário do cluster. Para encontrar esses valores, execute <code>bx cs cluster-get &lt;cluster_name&gt; --showResources</code>. Na seção **VLANS**, procure os CIDRs que possuem um valor **público** de <code>false</code>.</li></ul>**Nota**: se <code>ipsec.keyexchange</code> estiver configurado como <code>ikev1</code>, será possível especificar apenas uma sub-rede.</td>
    </tr>
    <tr>
    <td><code>local.id</code></td>
    <td>Mude esse valor para o identificador de sequência para o lado cluster do Kubernetes local que o seu terminal de túnel da VPN usa para a conexão.</td>
    </tr>
    <tr>
    <td><code>remote.gateway</code></td>
    <td>Mude esse valor para o endereço IP público para o gateway da VPN no local. Quando <code>ipsec.auto</code> é configurado para <code>start</code>, esse valor é necessário.</td>
    </tr>
    <td><code>remote.subnet</code></td>
    <td>Mude esse valor para a lista de CIDRs de sub-rede privada no local que os clusters do Kubernetes podem acessar. **Nota**: se <code>ipsec.keyexchange</code> estiver configurado como <code>ikev1</code>, será possível especificar apenas uma sub-rede.</td>
    </tr>
    <tr>
    <td><code>remote.id</code></td>
    <td>Mude esse valor para o identificador de sequência para o lado local remoto que o seu terminal de túnel da VPN usa para a conexão.</td>
    </tr>
    <tr>
    <td><code>remote.privateIPtoPing</code></td>
    <td>Inclua o endereço IP privado na sub-rede remota a ser usada pelos programas de validação de teste do Helm para testes de conectividade de ping de VPN. Esse valor é opcional.</td>
    </tr>
    <tr>
    <td><code>preshared.secret</code></td>
    <td>Mude esse valor para o segredo pré-compartilhado que o gateway do terminal do túnel da VPN local usa para a conexão. Esse valor é armazenado em <code>ipsec.secrets</code>.</td>
    </tr>
    </tbody></table>

4. Salve o arquivo `config.yaml` atualizado.

5. Instale o gráfico do Helm em seu cluster com o arquivo `config.yaml` atualizado. As propriedades atualizadas são armazenadas em um configmap para seu gráfico.

    **Nota**: se você tem múltiplas implementações de VPN em um único cluster, é possível evitar conflitos de nomenclatura e diferenciar entre suas implementações escolhendo nomes de liberação mais descritivos que `vpn`. Para evitar o truncamento do nome de liberação, limite o nome de liberação para 35 caracteres ou menos.

    ```
    -f config.yaml helm install -- name=vpn ibm/strongswan
    ```
    {: pre}

6. Verifique o status de implementação do gráfico. Quando o gráfico estiver pronto, o campo **STATUS** perto da parte superior da saída terá um valor de `DEPLOYED`.

    ```
    helm status vpn
    ```
    {: pre}

7. Quando o gráfico for implementado, verifique se as configurações atualizadas no arquivo `config.yaml` foram usadas.

    ```
    helm get values vpn
    ```
    {: pre}


### Testar e verificar a conectividade VPN
{: #vpn_test}

Depois de implementar seu gráfico Helm, teste a conectividade VPN.
{:shortdesc}

1. Se a VPN no gateway local não estiver ativo, inicie a VPN.

2. Configure a variável de ambiente `STRONGSWAN_POD`.

    ```
    export STRONGSWAN_POD=$(kubectl get pod -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
    ```
    {: pre}

3. Verifique o status da VPN. Um status de `ESTABLISHED` significa que a conexão de VPN foi bem-sucedida.

    ```
    Kubectl exec $STRONGSWAN_POD -- ipsec status
    ```
    {: pre}

    Saída de exemplo:

    ```
    Security Associations (1 up, 0 connecting):
    k8s-conn[1]: ESTABLISHED 17 minutes ago, 172.30.xxx.xxx[ibm-cloud]...192.xxx.xxx.xxx[on-premises]
    k8s-conn{2}: INSTALLED, TUNNEL, reqid 12, ESP in UDP SPIs: c78cb6b1_i c5d0d1c3_o
    k8s-conn{2}: 172.21.0.0/16 172.30.0.0/16 === 10.91.152.xxx/26
    ```
    {: screen}

    **Nota**:

    <ul>
    <li>Quando você tenta estabelecer a conectividade VPN com o gráfico Helm do strongSwan, é provável que o status da VPN não seja `ESTABLISHED` na primeira vez. Você pode precisar verificar as configurações do terminal de VPN e mudar o arquivo de configuração várias vezes antes que a conexão seja bem-sucedida: <ol><li>Execute `helm delete --purge <release_name>`</li><li>Corrija os valores incorretos no arquivo de configuração.</li><li>Execute `helm install -f config.yaml -- name=<release_name> ibm/strongswan`</li></ol>Também é possível executar mais verificações na próxima etapa.</li>
    <li>Se o pod de VPN está em um estado `ERROR` ou continua travando e reiniciando, isso pode ser devido à validação do parâmetro das configurações do `ipsec.conf` no configmap do gráfico.<ol><li>Verifique quaisquer erros de validação nos logs do pod do strongSwan executando `kubectl logs -n $STRONGSWAN_POD`.</li><li>Se erros de validação existirem, execute `helm delete --purge <release_name>`<li>Corrija os valores incorretos no arquivo de configuração.</li><li>Execute `helm install -f config.yaml -- name=<release_name> ibm/strongswan`</li></ol>Se o seu cluster tem um alto número de nós do trabalhador, também é possível usar `helm upgrade` para aplicar mais rapidamente suas mudanças em vez de executar `helm delete` e `helm install`.</li>
    </ul>

4. É possível testar ainda mais a conectividade VPN executando os cinco testes do Helm que são incluídos na definição de gráfico do strongSwan.

    ```
    helm test vpn
    ```
    {: pre}

    * Se todos os testes passarem, sua conexão VPN do strongSwan foi configurada com êxito.

    * Se algum dos testes falhar, continue com a próxima etapa.

5. Visualize a saída de um teste com falha consultando os logs do pod de teste.

    ```
    kubectl logs <test_program>
    ```
    {: pre}

    **Nota**: alguns dos testes têm requisitos que são configurações opcionais na configuração de VPN. Se alguns dos testes falharem, as falhas poderão ser aceitáveis, dependendo de se você especificou essas configurações opcionais. Consulte a tabela a seguir para obter informações sobre cada teste e por que ele pode falhar.

    {: #vpn_tests_table}
    <table>
    <caption>Entendendo os testes de conectividade VPN do Helm</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> Entendendo os testes de conectividade VPN do Helm</th>
    </thead>
    <tbody>
    <tr>
    <td><code>vpn-strongswan-check-config</code></td>
    <td>Valida a sintaxe do arquivo <code>ipsec.conf</code> que é gerado por meio do arquivo <code>config.yaml</code>. Esse teste pode falhar devido a valores incorretos no arquivo <code>config.yaml</code>.</td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-check-state</code></td>
    <td>Verifica se a conexão VPN tem um status de <code>ESTABLISHED</code>. Esse teste poderá falhar pelos motivos a seguir:<ul><li>Diferenças entre os valores no arquivo <code>config.yaml</code> e as configurações do terminal de VPN no local.</li><li>Se o cluster está em modo de "atendimento" (<code>ipsec.auto</code> está configurado para <code>add</code>), a conexão não é estabelecida no lado no local.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-gw</code></td>
    <td>Efetua ping do endereço IP público <code>remote.gateway</code> que você configurou no arquivo <code>config.yaml</code>. Esse teste poderá falhar pelos motivos a seguir:<ul><li>Você não especificou um endereço IP do gateway de VPN no local. Se <code>ipsec.auto</code> está configurado para <code>start</code>, o endereço IP <code>remote.gateway</code> é necessário.</li><li>A conexão VPN não tem o status <code>ESTABLISHED</code>. Veja <code>vpn-strongswan-check-state</code> para obter mais informações.</li><li>A conectividade VPN é <code>ESTABLISHED</code>, mas os pacotes do ICMP estão sendo bloqueados por um firewall.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-ip-1</code></td>
    <td>Efetua ping do endereço IP privado <code>remote.privateIPtoPing</code> do gateway de VPN no local por meio do pod de VPN no cluster. Esse teste poderá falhar pelos motivos a seguir:<ul><li>Você não especificou um endereço IP <code>remote.privateIPtoPing</code>. Se você intencionalmente não tiver especificado um endereço IP, essa falha será aceitável.</li><li>Você não especificou o CIDR de sub-rede de pod do cluster, <code>172.30.0.0/16</code>, na lista <code>local.subnet</code>.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-ip-2</code></td>
    <td>Efetua ping do endereço IP privado <code>remote.privateIPtoPing</code> do gateway de VPN no local por meio do nó do trabalhador no cluster. Esse teste poderá falhar pelos motivos a seguir:<ul><li>Você não especificou um endereço IP <code>remote.privateIPtoPing</code>. Se você intencionalmente não tiver especificado um endereço IP, essa falha será aceitável.</li><li>Você não especificou o CIDR de sub-rede privada do nó do trabalhador na lista <code>local.subnet</code>.</li></ul></td>
    </tr>
    </tbody></table>

6. Exclua o gráfico Helm atual.

    ```
    helm delete --purge vpn
    ```
    {: pre}

7. Abra o arquivo `config.yaml` e corrija os valores incorretos.

8. Salve o arquivo `config.yaml` atualizado.

9. Instale o gráfico do Helm em seu cluster com o arquivo `config.yaml` atualizado. As propriedades atualizadas são armazenadas em um configmap para seu gráfico.

    ```
    helm install -f config.yaml --name=<release_name> ibm/strongswan
    ```
    {: pre}

10. Verifique o status de implementação do gráfico. Quando o gráfico estiver pronto, o campo **STATUS** perto da parte superior da saída terá um valor de `DEPLOYED`.

    ```
    helm status vpn
    ```
    {: pre}

11. Quando o gráfico for implementado, verifique se as configurações atualizadas no arquivo `config.yaml` foram usadas.

    ```
    helm get values vpn
    ```
    {: pre}

12. Limpe os pods de teste atuais.

    ```
    Kubectl get pods -a -l app=strongswan-test
    ```
    {: pre}

    ```
    kubectl delete pods -l app=strongswan-test
    ```
    {: pre}

13. Execute os testes novamente.

    ```
    helm test vpn
    ```
    {: pre}

<br />


## Fazendo upgrade do gráfico Helm do strongSwan
{: #vpn_upgrade}

Certifique-se de que o gráfico Helm do strongSwan esteja atualizado fazendo upgrade dele.
{:shortdesc}

Para fazer upgrade de seu gráfico Helm do strongSwan para a versão mais recente:

  ```
  Helm upgrade -f config.yaml < release_name> ibm/strongswan
  ```
  {: pre}

**Importante**: o gráfico Helm do strongSwan 2.0.0 não funciona com o Calico v3 ou Kubernetes 1.10. Antes de [atualizar seu cluster para 1.10](cs_versions.html#cs_v110), atualize o strongSwan para o gráfico Helm 2.1.0, que é compatível com versões anteriores do Calico 2.6 e Kubernetes 1.7, 1.8 e 1.9.


### Fazendo upgrade da versão 1.0.0
{: #vpn_upgrade_1.0.0}

Devido a algumas das configurações que são usadas no gráfico Helm da versão 1.0.0, não é possível usar `helm upgrade` para atualizar da 1.0.0 para a versão mais recente.
{:shortdesc}

Para fazer upgrade da versão 1.0.0, deve-se excluir o gráfico 1.0.0 e instalar a versão mais recente:

1. Exclua o gráfico Helm da 1.0.0.

    ```
    helm delete --purge <release_name>
    ```
    {: pre}

2. Salve as definições de configuração padrão para a versão mais recente do gráfico Helm do strongSwan em um arquivo YAML local.

    ```
    helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. Atualize o arquivo de configuração e salve o arquivo com suas mudanças.

4. Instale o gráfico do Helm em seu cluster com o arquivo `config.yaml` atualizado.

    ```
    helm install -f config.yaml --name=<release_name> ibm/strongswan
    ```
    {: pre}

Além disso, algumas configurações de tempo limite do `ipsec.conf` que foram codificadas permanentemente na 1.0.0 são expostas como propriedades configuráveis em versões mais recentes. Os nomes e os padrões de algumas dessas configurações de tempo limite do `ipsec.conf` configuráveis também foram mudados para serem mais consistentes com os padrões do strongSwan. Se você está fazendo upgrade de seu gráfico Helm da 1.0.0 e deseja reter os padrões da versão 1.0.0 para as configurações de tempo limite, inclua as novas configurações em seu arquivo de configuração do gráfico com os valores padrão antigos.



  <table>
  <caption>As diferenças de configurações do ipsec.conf entre a versão 1.0.0 e a versão mais recente</caption>
  <thead>
  <th>Nome da configuração da 1.0.0</th>
  <th>Padrão da 1.0.0</th>
  <th>Nome da configuração de versão mais recente</th>
  <th>Padrão de versão mais recente</th>
  </thead>
  <tbody>
  <tr>
  <td><code>ikelifetime</code></td>
  <td>60m</td>
  <td><code>ikelifetime</code></td>
  <td>3h</td>
  </tr>
  <tr>
  <td><code>keylife</code></td>
  <td>20m</td>
  <td><code>lifetime</code></td>
  <td>1h</td>
  </tr>
  <tr>
  <td><code>rekeymargin</code></td>
  <td>3m</td>
  <td><code>margintime</code></td>
  <td>9m</td>
  </tr>
  </tbody></table>


## Desativando o serviço de VPN strongSwan IPSec
{: vpn_disable}

É possível desativar a conexão VPN excluindo o gráfico Helm.
{:shortdesc}

  ```
  helm delete --purge <release_name>
  ```
  {: pre}

<br />


## Configurando a conectividade VPN com um Virtual Router Appliance
{: #vyatta}

O [Virtual Router Appliance (VRA)](/docs/infrastructure/virtual-router-appliance/about.html) fornece o sistema operacional Vyatta 5600 mais recente para servidores bare metal x86. É possível usar um VRA como um gateway de VPN para se conectar com segurança a uma rede no local.
{:shortdesc}

Todo o tráfego de rede pública e privada que entra ou sai das VLANs do cluster é roteado por meio de um VRA. É possível usar o VRA como um terminal de VPN para criar um túnel IPSec criptografado entre servidores na infraestrutura do IBM Cloud (SoftLayer) e recursos no local. Por exemplo, o diagrama a seguir mostra como um app em um nó do trabalhador somente privado no {{site.data.keyword.containershort_notm}} pode se comunicar com um servidor no local por meio de uma conexão VPN do VRA:

<img src="images/cs_vpn_vyatta.png" width="725" alt="Expor um app no {{site.data.keyword.containershort_notm}} usando um balanceador de carga" style="width:725px; border-style: none"/>

1. Um app em seu cluster, `myapp2`, recebe uma solicitação de um serviço Ingress ou LoadBalancer e precisa conectar-se com segurança a dados na rede local.

2. Como o `myapp2` está em um nó do trabalhador que está somente em uma VLAN privada, o VRA age como uma conexão segura entre os nós do trabalhador e a rede no local. O VRA usa o endereço IP de destino para determinar quais pacotes de rede enviar para a rede no local.

3. A solicitação é criptografada e enviada pelo túnel VPN para o data center no local.

4. A solicitação recebida passa pelo firewall no local e é entregue ao terminal de túnel VPN (roteador) no qual ela é decriptografada.

5. O terminal de túnel VPN (roteador) encaminha a solicitação para o servidor ou mainframe no local, dependendo do endereço IP de destino que foi especificado na etapa 2. Os dados necessários são enviados de volta na conexão VPN para `myapp2` por meio do mesmo processo.

Para configurar um Virtual Router Appliance:

1. [Pedir um VRA](/docs/infrastructure/virtual-router-appliance/getting-started.html).

2. [Configure a VLAN privada no VRA](/docs/infrastructure/virtual-router-appliance/manage-vlans.html).

3. Para ativar uma conexão VPN usando o VRA, [configure VRRP no VRA](/docs/infrastructure/virtual-router-appliance/vrrp.html#high-availability-vpn-with-vrrp).
