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



# Controlando o tráfego com políticas de rede
{: #network_policies}

Cada cluster do Kubernetes é configurado com um plug-in de rede chamado Calico. As políticas de rede padrão são configuradas para assegurar a interface de rede pública de cada nó do trabalhador no {{site.data.keyword.containerlong}}.
{: shortdesc}

Se você tem requisitos de segurança exclusivos, é possível usar o Calico e o Kubernetes para criar políticas de rede para um cluster. Com políticas de rede do Kubernetes, é possível especificar o tráfego de rede que você deseja permitir ou bloquear para/de um pod em um cluster. Para configurar políticas de rede mais avançadas, como bloquear o tráfego de entrada (ingresso) para serviços LoadBalancer, use políticas de rede do Calico.

<ul>
  <li>
  [Políticas de rede do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/services-networking/network-policies/): essas políticas especificam como os pods podem se comunicar com outros pods e com terminais externos. Desde o Kubernetes versão 1.8, ambos os tráfegos, recebido e de saída, podem ser permitidos ou bloqueados com base no protocolo, na porta e nos endereços IP de origem ou de destino. O tráfego também pode ser filtrado com base nos rótulos de pod e de namespace. As políticas de rede do Kubernetes são aplicadas usando comandos `kubectl` ou as APIs do Kubernetes. Quando essas políticas são aplicadas, elas são convertidas automaticamente em políticas de rede do Calico e o Calico cumpre essas políticas.
  </li>
  <li>
  Políticas de rede do Calico para clusters do Kubernetes versão [1.10 e mais recente ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.projectcalico.org/v3.1/getting-started/kubernetes/tutorials/advanced-policy) ou clusters do [1.9 e anterior ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.projectcalico.org/v2.6/getting-started/kubernetes/tutorials/advanced-policy): essas políticas são um superconjunto das políticas de rede do Kubernetes e são aplicadas usando comandos `calicoctl`. As políticas do Calico incluir os recursos a seguir.
    <ul>
    <li>Permitir ou bloquear tráfego de rede em interfaces de rede específicas independentemente do endereço IP de origem ou destino do pod do Kubernetes ou CIDR.</li>
    <li>Permitir ou bloquear tráfego de rede para os pods em namespaces.</li>
    <li>[Bloquear o tráfego de entrada (ingresso) para os serviços do Kubernetes LoadBalancer ou NodePort](#block_ingress).</li>
    </ul>
  </li>
  </ul>

O Calico cumpre essas políticas, incluindo quaisquer políticas de rede do Kubernetes que são convertidas automaticamente em políticas do Calico, configurando regras iptables do Linux nos nós do trabalhador do Kubernetes. As regras de Iptables servem como um firewall para o nó do trabalhador para definir as características que o tráfego de rede deve atender para ser encaminhado para o recurso de destino.

<br />


## Políticas de rede padrão do Calico e do Kubernetes
{: #default_policy}

Quando um cluster com uma VLAN pública é criado, um recurso HostEndpoint com o rótulo `ibm.role: worker_public` é criado automaticamente para cada nó do trabalhador e sua interface de rede pública. Para proteger a interface de rede pública de um nó do trabalhador, as políticas padrão do Calico são aplicadas a qualquer terminal de host com o rótulo `ibm.role: worker_public`.
{:shortdesc}

Essas políticas padrão do Calico permitem todo o tráfego de rede de saída e permitem o tráfego de entrada para componentes específicos do cluster, como os serviços do Kubernetes NodePort, LoadBalancer e Ingresso. Qualquer outro tráfego de rede de entrada da Internet para seus nós do trabalhador que não esteja especificado nas políticas padrão é bloqueado. As políticas padrão não afetam o tráfego de pod para pod.

Revise as políticas de rede padrão do Calico a seguir que são aplicadas automaticamente ao seu cluster.

**Importante:** não remova políticas que são aplicadas a um terminal de host, a menos que você entenda completamente a política. Certifique-se de que você não precise do tráfego que está sendo permitido pela política.

 <table summary="A primeira linha na tabela abrange ambas as colunas. Leia o restante das linhas da esquerda para a direita, com o local do servidor na coluna um e os endereços IP para corresponder na coluna dois.">
 <caption>Padrão do Calico políticas para cada cluster</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> Políticas padrão do Calico para cada cluster</th>
  </thead>
  <tbody>
    <tr>
      <td><code>allow-all-outbound</code></td>
      <td>Permite todo o tráfego de saída.</td>
    </tr>
    <tr>
      <td><code>allow-bixfix-port</code></td>
      <td>Permite o tráfego recebido na porta 52311 para o app BigFix para permitir as atualizações necessárias do nó do trabalhador.</td>
    </tr>
    <tr>
      <td><code>allow-icmp</code></td>
      <td>Permite pacotes ICMP recebidas (pings).</td>
     </tr>
    <tr>
      <td><code>allow-node-port-dnat</code></td>
      <td>Permite o tráfego recebido dos serviços NodePort, LoadBalancer e Ingresso para os pods que esses serviços estão expondo. <strong>Nota</strong>: você não precisa especificar as portas expostas porque o Kubernetes usa a conversão de endereço de rede de destino (DNAT) para encaminhar as solicitações de serviço para os pods corretos. Esse redirecionamento ocorre antes que as políticas de terminal de host sejam aplicadas aos iptables.</td>
   </tr>
   <tr>
      <td><code>allow-sys-mgmt</code></td>
      <td>Permite conexões recebidas para sistemas específicos da infraestrutura do IBM Cloud (SoftLayer) que são usados para gerenciar os nós do trabalhador.</td>
   </tr>
   <tr>
    <td><code>allow-vrrp</code></td>
    <td>Permitir pacotes VRRP, que são usados para monitorar e mover endereços IP virtuais entre os nós do trabalhador.</td>
   </tr>
  </tbody>
</table>

Em clusters do Kubernetes versão 1.10 e mais recente, uma política padrão do Kubernetes que limita o acesso ao Painel do Kubernetes também é criada. As políticas do Kubernetes não se aplicam ao terminal de host, mas ao pod `kube-dashboard`. Essa política se aplica a clusters conectados somente a uma VLAN privada e a clusters conectados a uma VLAN pública e privada.

<table>
<caption>Políticas padrão para cada cluster do Kubernetes</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> Políticas padrão do Kubernetes para cada cluster</th>
</thead>
<tbody>
 <tr>
  <td><code>kubernetes-painel</code></td>
  <td><b>Somente no Kubernetes v1.10</b>, fornecido no namespace <code>kube-system</code>: bloqueia todos os pods de acessarem o Painel do Kubernetes. Essa política não afeta o acesso ao painel por meio da UI do {{site.data.keyword.Bluemix_notm}} ou usando <code>kubectl proxy</code>. Se um pod requer acesso ao painel, implemente o pod em um namespace que tenha o rótulo <code>kubernetes-dashboard-policy: allow</code>.</td>
 </tr>
</tbody>
</table>

<br />


## Instalando e Configurando o CLI do Calico
{: #cli_install}

Para visualizar, gerenciar e incluir políticas do Calico, instale e configure a CLI do Calico.
{:shortdesc}

A compatibilidade de versões do Calico para configuração e políticas da CLI varia com base na versão do Kubernetes de seu cluster. Para instalar e configurar a CLI do Calico, clique em um dos links a seguir com base na versão de seu cluster:

* [Kubernetes version 1.10 ou mais clusters](#1.10_install)
* [Kubernetes version 1.9 ou anterior clusters](#1.9_install)

Antes de atualizar seu cluster do Kubernetes versão 1.9 ou anterior para a versão 1.10 ou mais recente, revise [Preparando-se para atualizar para o Calico v3](cs_versions.html#110_calicov3).
{: tip}

### Instalar e configurar o CLI do Calico versão 3.1.1 para clusters que estão executando o Kubernetes versão 1.10 ou mais recente
{: #1.10_install}

Antes de iniciar, [destine a CLI do Kubernetes para o cluster](cs_cli_install.html#cs_cli_configure). Inclua a opção `--admin` com o comando `bx cs cluster-config`, que é usado para fazer download dos certificados e arquivos de permissão. Este download também inclui as chaves para a função de Super Usuário, que você precisa para executar comandos do Calico.

  ```
  bx cs cluster-config <cluster_name> --admin
  ```
  {: pre}

Para instalar e configurar a CLI do Calico 3.1.1:

1. [Faça download da CLI do Calico ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/projectcalico/calicoctl/releases/tag/v3.1.1).

    Se estiver usando OSX, faça download da versão `-darwin-amd64`. Se estiver usando o Windows, instale a CLI do Calico no mesmo diretório que a CLI do {{site.data.keyword.Bluemix_notm}}. Essa configuração economiza algumas
mudanças de caminho de arquivo ao executar comandos posteriormente.
    {: tip}

2. Para usuários do OSX e Linux, conclua as etapas a seguir.
    1. Mova o arquivo executável para o diretório _/usr/local/bin_.
        - Linux:

          ```
          Mv filepath /calicoctl /usr/local/bin/calicoctl
          ```
          {: pre}

        - S.O.
X:

          ```
          Mv filepath/calicoctl-darwin-amd64 /usr/local/bin/calicoctl
          ```
          {: pre}

    2. Torne o arquivo um arquivo executável.

        ```
        chmod +x /usr/local/bin/calicoctl
        ```
        {: pre}

3. Verifique se os comandos `calico` são executados corretamente verificando a versão de cliente da CLI do Calico.

    ```
    calicoctl version
    ```
    {: pre}

4. Se as políticas de rede corporativa usam proxies ou firewalls para evitar o acesso de seu sistema local a terminais públicos, [permita o acesso TCP para comandos do Calico](cs_firewall.html#firewall).

5. Para S.O. Linux e X, crie o diretório `/etc/calico`. Para Windows, qualquer diretório pode ser usado.

  ```
  sudo mkdir -p /etc/calico/
  ```
  {: pre}

6. Crie um arquivo `calicoctl.cfg`.
    - Linux e OS X:

      ```
      sudo vi /etc/calico/calicoctl.cfg
      ```
      {: pre}

    - Windows: crie o arquivo com um editor de texto.

7. Digite as informações a seguir no arquivo <code>calicoctl.cfg</code>.

    ```
    apiVersion: projectcalico.org/v3
    kind: CalicoAPIConfig
    metadata:
    spec:
        datastoreType: etcdv3
        etcdEndpoints: <ETCD_URL>
        etcdKeyFile: <CERTS_DIR>/admin-key.pem
        etcdCertFile: <CERTS_DIR>/admin.pem
        etcdCACertFile: <CERTS_DIR>/<ca-*pem_file>
    ```
    {: codeblock}

    1. Recupere o `<ETCD_URL>`.

      - Linux e OS X:

          ```
          kubectl get cm -n kube-system calico-config -o yaml | grep "etcd_endpoints:" | awk '{ print $2 }'
          ```
          {: pre}

          Saída de exemplo:

          ```
          Https://169.xx.xxx.xxx:30001
          ```
          {: screen}

      - Windows:
        <ol>
        <li>Obtenha os valores de configuração do Calico do mapa de configuração. </br><pre class="codeblock"><code>kubectl get cm -n kube-system calico-config -o yaml</code></pre></br>
        <li>Na seção de `dados`, localize o valor etcd_endpoints. Exemplo: <code>https://169.xx.xxx.xxx:30001</code>
        </ol>

    2. Recupere o `<CERTS_DIR>`, o diretório no qual os certificados do Kubernetes são transferidos por download.

        - Linux e OS X:

          ```
          dirname $KUBECONFIG
          ```
          {: pre}

          Saída de exemplo:

          ```
          /home/sysadmin/.bluemix/plugins/container-service/clusters/<cluster_name>-admin/
          ```
          {: screen}

        - Windows:

          ```
          ECHO %KUBECONFIG%
          ```
          {: pre}

            Exemplo de saída:

          ```
          C: /Users/ < user>-service/mycluster-admin/kube-config-prod-dal10-mycluster.yml
          ```
          {: screen}

        **Nota**: para obter o caminho do diretório, remova o nome de arquivo `kube-config-prod-<location>-<cluster_name>.yml` do término da saída.

    3. Recupere o `ca-*pem_file`.

        - Linux e OS X:

          ```
          ls `dirname $KUBECONFIG` | grep "ca-"
          ```
          {: pre}

        - Windows:
          <ol><li>Abra o diretório recuperado na última etapa.</br><pre class="codeblock"><code>C:\Users\<user>\.bluemix\plugins\container-service\&lt;cluster_name&gt;-admin\</code></pre>
          <li> Localize o arquivo <code>ca-*pem_file</code>.</ol>

    4. Verifique se a configuração do Calico está funcionando
corretamente.

        - Linux e OS X:

          ```
          calicoctl get nodes
          ```
          {: pre}

        - Windows:

          ```
          calicoctl get nodes --config=filepath/calicoctl.cfg
          ```
          {: pre}

          Saída:

          ```
          NAME
              kube-dal10-crc21191ee3997497ca90c8173bbdaf560-w1.cloud.ibm
              kube-dal10-crc21191ee3997497ca90c8173bbdaf560-w2.cloud.ibm
              kube-dal10-crc21191ee3997497ca90c8173bbdaf560-w3.cloud.ibm
          ```
          {: screen}


### Instalando e configurando a CLI do Calico versão 1.6.3 para clusters que estão executando o Kubernetes versão 1.9 ou anterior
{: #1.9_install}

Antes de iniciar, [destine a CLI do Kubernetes para o cluster](cs_cli_install.html#cs_cli_configure). Inclua a opção `--admin` com o comando `bx cs cluster-config`, que é usado para fazer download dos certificados e arquivos de permissão. Este download também inclui as chaves para a função de Super Usuário, que você precisa para executar comandos do Calico.

  ```
  bx cs cluster-config <cluster_name> --admin
  ```
  {: pre}

Para instalar e configurar a CLI do Calico 1.6.3:

1. [Faça download da CLI do Calico ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/projectcalico/calicoctl/releases/tag/v1.6.3).

    Se estiver usando OSX, faça download da versão `-darwin-amd64`. Se estiver usando o Windows, instale a CLI do Calico no mesmo diretório que a CLI do {{site.data.keyword.Bluemix_notm}}. Essa configuração economiza algumas
mudanças de caminho de arquivo ao executar comandos posteriormente.
    {: tip}

2. Para usuários do OSX e Linux, conclua as etapas a seguir.
    1. Mova o arquivo executável para o diretório _/usr/local/bin_.
        - Linux:

          ```
          Mv filepath /calicoctl /usr/local/bin/calicoctl
          ```
          {: pre}

        - S.O.
X:

          ```
          Mv filepath/calicoctl-darwin-amd64 /usr/local/bin/calicoctl
          ```
          {: pre}

    2. Torne o arquivo um arquivo executável.

        ```
        chmod +x /usr/local/bin/calicoctl
        ```
        {: pre}

3. Verifique se os comandos `calico` são executados corretamente verificando a versão de cliente da CLI do Calico.

    ```
    calicoctl version
    ```
    {: pre}

4. Se as políticas de rede corporativa usam proxies ou firewalls para evitar o acesso de seu sistema local a terminais públicos: veja [Executando comandos `calicoctl` por trás de um firewall](cs_firewall.html#firewall) para obter instruções sobre como permitir o acesso TCP para comandos do Calico.

5. Para S.O. Linux e X, crie o diretório `/etc/calico`. Para Windows, qualquer diretório pode ser usado.
    ```
    sudo mkdir -p /etc/calico/
    ```
    {: pre}

6. Crie um arquivo `calicoctl.cfg`.
    - Linux e OS X:

      ```
      sudo vi /etc/calico/calicoctl.cfg
      ```
      {: pre}

    - Windows: crie o arquivo com um editor de texto.

7. Digite as informações a seguir no arquivo <code>calicoctl.cfg</code>.

    ```
    apiVersion: v1
      kind: calicoApiConfig
      metadata:
      spec:
          etcdEndpoints: <ETCD_URL>
          etcdKeyFile: <CERTS_DIR>/admin-key.pem
          etcdCertFile: <CERTS_DIR>/admin.pem
          etcdCACertFile: <CERTS_DIR>/<ca-*pem_file>
    ```
    {: codeblock}

    1. Recupere o `<ETCD_URL>`.

      - Linux e OS X:

          ```
          kubectl get cm -n kube-system calico-config -o yaml | grep "etcd_endpoints:" | awk '{ print $2 }'
          ```
          {: pre}

      - Exemplo de saída:

          ```
          Https://169.xx.xxx.xxx:30001
          ```
          {: screen}

      - Windows:
        <ol>
        <li>Obtenha os valores de configuração do Calico do mapa de configuração. </br><pre class="codeblock"><code>kubectl get cm -n kube-system calico-config -o yaml</code></pre></br>
        <li>Na seção de `dados`, localize o valor etcd_endpoints. Exemplo: <code>https://169.xx.xxx.xxx:30001</code>
        </ol>

    2. Recupere o `<CERTS_DIR>`, o diretório no qual os certificados do Kubernetes são transferidos por download.

        - Linux e OS X:

          ```
          dirname $KUBECONFIG
          ```
          {: pre}

          Saída de exemplo:

          ```
          /home/sysadmin/.bluemix/plugins/container-service/clusters/<cluster_name>-admin/
          ```
          {: screen}

        - Windows:

          ```
          ECHO %KUBECONFIG%
          ```
          {: pre}

          Saída de exemplo:

          ```
          C: /Users/ < user>-service/mycluster-admin/kube-config-prod-dal10-mycluster.yml
          ```
          {: screen}

        **Nota**: para obter o caminho do diretório, remova o nome de arquivo `kube-config-prod-<location>-<cluster_name>.yml` do término da saída.

    3. Recupere o `ca-*pem_file`.

        - Linux e OS X:

          ```
          ls `dirname $KUBECONFIG` | grep "ca-"
          ```
          {: pre}

        - Windows:
          <ol><li>Abra o diretório recuperado na última etapa.</br><pre class="codeblock"><code>C:\Users\<user>\.bluemix\plugins\container-service\&lt;cluster_name&gt;-admin\</code></pre>
          <li> Localize o arquivo <code>ca-*pem_file</code>.</ol>

    4. Verifique se a configuração do Calico está funcionando
corretamente.

        - Linux e OS X:

          ```
          calicoctl get nodes
          ```
          {: pre}

        - Windows:

          ```
          calicoctl get nodes --config=filepath/calicoctl.cfg
          ```
          {: pre}

          Saída:

          ```
          NAME
              kube-dal10-crc21191ee3997497ca90c8173bbdaf560-w1.cloud.ibm
              kube-dal10-crc21191ee3997497ca90c8173bbdaf560-w2.cloud.ibm
              kube-dal10-crc21191ee3997497ca90c8173bbdaf560-w3.cloud.ibm
          ```
          {: screen}

<br />


## Visualizando políticas de rede
{: #view_policies}

Visualize os detalhes para políticas padrão e quaisquer políticas de rede incluídas que são aplicadas a seu cluster.
{:shortdesc}

Antes de iniciar:
1. [Instale e configure o CLI do Calico.](#cli_install)
2. [Destine a CLI do Kubernetes para o cluster](cs_cli_install.html#cs_cli_configure). Inclua a opção `--admin` com o comando `bx cs cluster-config`, que é usado para fazer download dos certificados e arquivos de permissão. Este download também inclui as chaves para a função de Super Usuário, que você precisa para executar comandos do Calico.
    ```
    bx cs cluster-config <cluster_name> --admin
    ```
    {: pre}

A compatibilidade de versões do Calico para configuração e políticas da CLI varia com base na versão do Kubernetes de seu cluster. Para instalar e configurar a CLI do Calico, clique em um dos links a seguir com base na versão de seu cluster:

* [Kubernetes version 1.10 ou mais clusters](#1.10_examine_policies)
* [Kubernetes version 1.9 ou anterior clusters](#1.9_examine_policies)

Antes de atualizar seu cluster do Kubernetes versão 1.9 ou anterior para a versão 1.10 ou mais recente, revise [Preparando-se para atualizar para o Calico v3](cs_versions.html#110_calicov3).
{: tip}

### Visualizar políticas de rede em clusters que estão executando o Kubernetes versão 1.10 ou mais recente
{: #1.10_examine_policies}

1. Visualize o terminal de host do Calico.

    ```
    calicoctl get hostendpoint -o yaml
    ```
    {: pre}

2. Visualize todas as políticas de rede do Calico e do Kubernetes que foram criadas para o cluster. Essa lista inclui políticas que podem não estar aplicadas a quaisquer pods ou hosts ainda. Para que uma política de rede seja aplicada, deverá ser localizado um recurso do Kubernetes que corresponda ao seletor definido na política de rede do Calico.

    [As políticas de rede ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/networkpolicy) estão com escopo definido para namespaces específicos:
    ```
    Calicoctl get NetworkPolicy -- all-namespaces -o wide
    ```

    [As políticas de rede global ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/globalnetworkpolicy) não estão com escopo definido para namespaces específicos:
    ```
    Calicoctl get GlobalNetworkPolicy -o wide
    ```
    {: pre}

3. Visualize detalhes para uma política de rede.

    ```
    calicoctl get NetworkPolicy -o yaml <policy_name> --namespace <policy_namespace>
    ```
    {: pre}

4. Visualize os detalhes de todas as políticas de rede global para o cluster.

    ```
    Calicoctl get GlobalNetworkPolicy -o yaml
    ```
    {: pre}

### Visualizar políticas de rede em clusters que estão executando o Kubernetes versão 1.9 ou anterior
{: #1.9_examine_policies}

1. Visualize o terminal de host do Calico.

    ```
    calicoctl get hostendpoint -o yaml
    ```
    {: pre}

2. Visualize todas as políticas de rede do Calico e do Kubernetes que foram criadas para o cluster. Essa lista inclui políticas que podem não estar aplicadas a quaisquer pods ou hosts ainda. Para que uma política de rede seja aplicada, deverá ser localizado um recurso do Kubernetes que corresponda ao seletor definido na política de rede do Calico.

    ```
    calicoctl get policy -o wide
    ```
    {: pre}

3. Visualize detalhes para uma política de rede.

    ```
    calicoctl get policy -o yaml <policy_name>
    ```
    {: pre}

4. Visualize os detalhes de todas as políticas de rede para o cluster.

    ```
    calicoctl get policy -o yaml
    ```
    {: pre}

<br />


## Incluindo políticas de rede
{: #adding_network_policies}

Na maioria dos casos, as políticas padrão não precisam ser mudadas. Somente cenários avançados podem requerer mudanças. É possível criar suas próprias políticas de rede quando você acha necessário fazer mudanças.
{:shortdesc}

Para criar políticas de rede do Kubernetes, veja a [documentação de política de rede do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/services-networking/network-policies/).

Para criar políticas do Calico, use as etapas a seguir.

Antes de iniciar:
1. [Instale e configure o CLI do Calico.](#cli_install)
2. [Destine a CLI do Kubernetes para o cluster](cs_cli_install.html#cs_cli_configure). Inclua a opção `--admin` com o comando `bx cs cluster-config`, que é usado para fazer download dos certificados e arquivos de permissão. Este download também inclui as chaves para a função de Super Usuário, que você precisa para executar comandos do Calico.
    ```
    bx cs cluster-config <cluster_name> --admin
    ```
    {: pre}

A compatibilidade de versões do Calico para configuração e políticas da CLI varia com base na versão do Kubernetes de seu cluster. Clique em um dos links a seguir com base em sua versão do cluster:

* [Kubernetes version 1.10 ou mais clusters](#1.10_create_new)
* [Kubernetes version 1.9 ou anterior clusters](#1.9_create_new)

Antes de atualizar seu cluster do Kubernetes versão 1.9 ou anterior para a versão 1.10 ou mais recente, revise [Preparando-se para atualizar para o Calico v3](cs_versions.html#110_calicov3).
{: tip}

### Incluindo políticas do Calico em clusters que estão executando o Kubernetes versão 1.10 ou mais recente
{: #1.10_create_new}

1. Defina sua [política de rede ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/networkpolicy) do Calico ou [política de rede global ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/globalnetworkpolicy) criando um script de configuração (`.yaml`). Esses arquivos de configuração incluem os seletores que descrevem a quais pods, namespaces ou hosts essas políticas se aplicam. Consulte essas [Políticas de amostra do Calico ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://docs.projectcalico.org/v3.1/getting-started/kubernetes/tutorials/advanced-policy) para ajudá-lo a criar a sua própria.
    **Nota**: os clusters do Kubernetes versão 1.10 ou mais recente devem usar a sintaxe de política do Calico v3.

2. Aplique as políticas ao cluster.
    - Linux e OS X:

      ```
      Policy.yaml calicoctl apply -f
      ```
      {: pre}

    - Windows:

      ```
      Filepath/policy.yaml apply -f calicoctl -- config=filepath/calicoctl.cfg
      ```
      {: pre}

### Incluindo políticas do Calico em clusters que estão executando o Kubernetes versão 1.9 ou anterior
{: #1.9_create_new}

1. Defina sua [política de rede do Calico ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://docs.projectcalico.org/v2.6/reference/calicoctl/resources/policy) criando um script de configuração (`.yaml`). Esses arquivos de configuração incluem os seletores que descrevem a quais pods, namespaces ou hosts essas políticas se aplicam. Consulte essas [Políticas de amostra do Calico ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://docs.projectcalico.org/v2.6/getting-started/kubernetes/tutorials/advanced-policy) para ajudá-lo a criar a sua própria.
    **Nota**: os clusters do Kubernetes versão 1.9 ou anterior devem usar a sintaxe de política do Calico v2.


2. Aplique as políticas ao cluster.
    - Linux e OS X:

      ```
      Policy.yaml calicoctl apply -f
      ```
      {: pre}

    - Windows:

      ```
      Filepath/policy.yaml apply -f calicoctl -- config=filepath/calicoctl.cfg
      ```
      {: pre}

<br />


## Bloqueando o tráfego de entrada para os serviços LoadBalancer ou NodePort
{: #block_ingress}

[Por padrão](#default_policy), os serviços NodePort e LoadBalancer do Kubernetes são projetados para tornar seu app disponível em todas as interfaces de cluster público e privado. No entanto, é possível bloquear o tráfego recebido para seus serviços com base na origem ou no destino do tráfego.
{:shortdesc}

Um serviço Kubernetes LoadBalancer também é um serviço NodePort. Um serviço LoadBalancer disponibiliza seu app no endereço IP e porta do LoadBalancer e disponibiliza seu app em NodePorts do serviço. Os NodePorts são acessíveis em cada endereço IP (público e privado) para cada nó no cluster.

O administrador de cluster pode usar as políticas de rede Calico `preDNAT` para bloquear:

  - Tráfego para serviços NodePort. O tráfego para serviços LoadBalancer é permitido.
  - O tráfego que é baseado em um endereço de origem ou CIDR.

Alguns usos comuns para políticas de rede Calico `preDNAT`:

  - Bloquear tráfego para NodePorts públicos de um serviço LoadBalancer privado.
  - Bloquear tráfego para NodePorts públicos em clusters que estão executando os [nós do trabalhador de borda](cs_edge.html#edge). Bloquear NodePorts assegura que os nós do trabalhador de borda sejam os únicos nós do trabalhador que manipulam o tráfego recebido.

As políticas padrão do Kubernetes e Calico são difíceis de aplicar à proteção dos serviços NodePort e LoadBalancer do Kubernetes devido às regras de iptables DNAT geradas para esses serviços.

As políticas de rede `preDNAT` do Calico podem ajudar porque elas geram regras de iptables com base em um recurso de
política de rede do Calico. Os clusters do Kubernetes versão 1.10 ou mais recente usam [políticas de rede com a sintaxe do `calicoctl.cfg` v3 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/networkpolicy). Os clusters do Kubernetes versão 1.9 ou anterior usam [políticas com a sintaxe do `calicoctl.cfg` v2 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.projectcalico.org/v2.6/reference/calicoctl/resources/policy).

1. Defina uma política de rede `preDNAT` do Calico para acesso de ingresso (tráfego de entrada) para serviços do Kubernetes.

    * Os clusters do Kubernetes versão 1.10 ou mais recente devem usar a sintaxe de política do Calico v3.

        Exemplo que bloqueia todos os recursos NodePorts:

        ```
        apiVersion: projectcalico.org/v3
        kind: GlobalNetworkPolicy
        metadata:
          name: deny-kube-node-port-services
        spec:
          applyOnForward: true
          ingress:
          - action: Deny destination: ports:
              - 30000:32767
            protocol: TCP
            source: {}
          - action: Deny destination: ports:
              - 30000:32767
            protocol: UDP
            source: {}
          preDNAT: true
          selector: ibm.role in { 'worker_public', 'master_public' }
          types:
          - Entrada
        ```
        {: codeblock}

    * Os clusters do Kubernetes versão 1.9 ou anterior devem usar a sintaxe de política do Calico v2.

        Exemplo que bloqueia todos os recursos NodePorts:

        ```
        apiVersion: v1
        kind: policy
        metadata:
          name: deny-kube-node-port-services
        spec:
          preDNAT: true
          selector: ibm.role in { 'worker_public', 'master_public' }
          ingress:
          - action: deny
            protocol: tcp
            destination:
              ports:
              - 30000:32767
          - action: deny
            protocol: udp
            destination:
              ports:
              - 30000:32767
        ```
        {: codeblock}

2. Aplique a política de rede Calico preDNAT. Leva aproximadamente 1 minuto para as mudanças de política serem aplicadas em todo o cluster.

  ```
  calicoctl apply -f deny-kube-node-port-services.yaml
  ```
  {: pre}
