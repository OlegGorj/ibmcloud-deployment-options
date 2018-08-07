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





# Referência de CLI para gerenciar clusters
{: #cs_cli_reference}

Consulte esses comandos para criar e gerenciar clusters no {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

## Comandos bx cs
{: #cs_commands}

**Dica:** para ver a versão do plug-in do {{site.data.keyword.containershort_notm}}, execute o comando a seguir.

```
bx plugin list
```
{: pre}



<table summary="Comandos de API">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Comandos de API</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs api-key-info](#cs_api_key_info)</td>
    <td>[bx cs api-key-reset](#cs_api_key_reset)</td>
    <td>[bx cs apiserver-config-get](#cs_apiserver_config_get)</td>
    <td>[bx cs apiserver-config-set](#cs_apiserver_config_set)</td>
  </tr>
  <tr>
    <td>[bx cs apiserver-config-unset](#cs_apiserver_config_unset)</td>
    <td>[bx cs apiserver-refresh](#cs_apiserver_refresh)</td>
    <td></td>
    <td></td>
 </tr>
</tbody>
</table>

<br>

<table summary="Comandos de uso de plug-in da CLI">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Comandos de uso do plug-in da CLI</th>
 </thead>
 <tbody>
  <tr>
    <td>[  bx cs help
  ](#cs_help)</td>
    <td>[        bx cs init
        ](#cs_init)</td>
    <td>[bx cs messages](#cs_messages)</td>
    <td></td>
  </tr>
</tbody>
</table>

<br>

<table summary="Comandos do cluster: gerenciamento">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Comandos do cluster: gerenciamento</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs cluster-config](#cs_cluster_config)</td>
    <td>[bx cs cluster-create](#cs_cluster_create)</td>
    <td>[bx cs cluster-feature-enable](#cs_cluster_feature_enable)</td>
    <td>[bx cs cluster-get](#cs_cluster_get)</td>
  </tr>
  <tr>
    <td>[bx cs cluster-rm](#cs_cluster_rm)</td>
    <td>[bx cs cluster-update](#cs_cluster_update)</td>
    <td>[        bx cs clusters
        ](#cs_clusters)</td>
    <td>[bx cs kube-versions](#cs_kube_versions)</td>
  </tr>
</tbody>
</table>

<br>

<table summary="Comandos do cluster: serviços e integrações">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Comandos do cluster: serviços e integrações</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs cluster-service-bind](#cs_cluster_service_bind)</td>
    <td>[bx cs cluster-service-unbind](#cs_cluster_service_unbind)</td>
    <td>[bx cs cluster-services](#cs_cluster_services)</td>
    <td>[bx cs webhook-create](#cs_webhook_create)</td>
  </tr>
</tbody>
</table>

</br>

<table summary="Comandos de cluster: Subnets">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Comandos do cluster: Subnets</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs cluster-subnet-add](#cs_cluster_subnet_add)</td>
    <td>[bx cs cluster-subnet-create](#cs_cluster_subnet_create)</td>
    <td>[bx cs cluster-user-subnet-add](#cs_cluster_user_subnet_add)</td>
    <td>[bx cs cluster-user-subnet-rm](#cs_cluster_user_subnet_rm)</td>
  </tr>
  <tr>
    <td>[    bx cs subnets
    ](#cs_subnets)</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</tbody>
</table>

</br>

<table summary="Comandos de infraestrutura">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Comandos de infraestrutura</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs credentials-set](#cs_credentials_set)</td>
    <td>[  bx cs credentials-unset
  ](#cs_credentials_unset)</td>
    <td>[bx cs machine-types](#cs_machine_types)</td>
    <td>[bx cs vlans](#cs_vlans)</td>
  </tr>
</tbody>
</table>

</br>

<table summary="Comandos do balanceador de carga do aplicativo (ALB) do Ingress">
<col width = 25%>
<col width = 25%>
<col width = 25%>
  <thead>
    <tr>
      <th colspan=4>Comandos do balanceador de carga do aplicativo (ALB) Ingress</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>[bx cs alb-cert-deploy](#cs_alb_cert_deploy)</td>
      <td>[bx cs alb-cert-get](#cs_alb_cert_get)</td>
      <td>[bx cs alb-cert-rm](#cs_alb_cert_rm)</td>
      <td>[bx cs alb-certs](#cs_alb_certs)</td>
    </tr>
    <tr>
      <td>[bx cs alb-configure](#cs_alb_configure)</td>
      <td>[bx cs alb-get](#cs_alb_get)</td>
      <td>[bx cs alb-types](#cs_alb_types)</td>
      <td>[bx cs albs](#cs_albs)</td>
    </tr>
  </tbody>
</table>

</br>

<table summary="Comandos de criação de log">
<col width = 25%>
<col width = 25%>
<col width = 25%>
  <thead>
    <tr>
      <th colspan=4>Comandos de criação de log</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>[bx cs logging-config-create](#cs_logging_create)</td>
      <td>[bx cs logging-config-get](#cs_logging_get)</td>
      <td>[bx cs logging-config-refresh](#cs_logging_refresh)</td>
      <td>[bx cs logging-config-rm](#cs_logging_rm)</td>
    </tr>
    <tr>
      <td>[bx cs logging-config-update](#cs_logging_update)</td>
      <td>[bx cs logging-filter-create](#cs_log_filter_create)</td>
      <td>[bx cs logging-filter-update](#cs_log_filter_update)</td>
      <td>[bx cs logging-filter-get](#cs_log_filter_view)</td>
    </tr>
    <tr>
      <td>[bx cs logging-filter-rm](#cs_log_filter_delete)</td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>

</br>

<table summary="Comandos de região">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Comandos de região</th>
 </thead>
 <tbody>
  <tr>
    <td>[        bx cs locations
        ](#cs_datacenters)</td>
    <td>[bx cs region](#cs_region)</td>
    <td>[bx cs region-set](#cs_region-set)</td>
    <td>[bx cs regions](#cs_regions)</td>
  </tr>
</tbody>
</table>

</br>

<table summary="Comandos de nó do trabalhador">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Comandos de nó do trabalhador</th>
 </thead>
 <tbody>
    <tr>
      <td>[bx cs worker-add](#cs_worker_add)</td>
      <td>[bx cs worker-get](#cs_worker_get)</td>
      <td>[bx cs worker-reboot](#cs_worker_reboot)</td>
      <td>[bx cs worker-reload](#cs_worker_reload)</td></staging>
    </tr>
    <tr>
      <td>[bx cs worker-rm](#cs_worker_rm)</td>
      <td>[bx cs worker-update](#cs_worker_update)</td>
      <td>[bx cs workers](#cs_workers)</td>
      <td></td>
    </tr>
  </tbody>
</table>

## Comandos de API
{: #api_commands}

### bx cs api-key-info CLUSTER
{: #cs_api_key_info}

Visualize o nome e o endereço de e-mail para o proprietário da chave API do IAM em uma região do {{site.data.keyword.containershort_notm}}.

A chave API do Identity and Access Management (IAM) é configurada automaticamente para uma região quando a primeira ação que requer a política de acesso de administrador do {{site.data.keyword.containershort_notm}} é executada. Por exemplo, um dos seus usuários administradores cria o primeiro cluster na região `us-south`. Ao fazer isso, a chave API do IAM para esse usuário é armazenada na conta para essa região. A chave API é usada para pedir recursos na infraestrutura do IBM Cloud (SoftLayer), como nós do trabalhador novo ou VLANs.

Quando um usuário diferente executa uma ação nessa região que requer interação com o portfólio da infraestrutura do IBM Cloud (SoftLayer), como a criação de um novo cluster ou o recarregamento de um nó do trabalhador, a chave API armazenada é usada para determinar se existem permissões suficientes para executar essa ação. Para certificar-se de que as ações relacionadas à infraestrutura em seu cluster possam ser executadas com sucesso, designe a seus usuários administradores do {{site.data.keyword.containershort_notm}} a política de acesso de infraestrutura do **Superusuário**. Para obter mais informações, veja [Gerenciando o acesso de usuário](cs_users.html#infra_access).

Se você acha que precisa atualizar a chave API que é armazenada para uma região, é possível fazer isso executando o comando [bx cs api-key-reset](#cs_api_key_reset). Esse comando requer a política de acesso de administrador do {{site.data.keyword.containershort_notm}} e armazena a chave API do usuário que executa esse comando na conta.

**Dica:** a chave API retornada nesse comando pode não ser usada se as credenciais de infraestrutura do IBM Cloud (SoftLayer) foram configuradas manualmente usando o comando [bx cs credentials-set](#cs_credentials_set).

<strong>Opções de comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>
   </dl>

**Exemplo**:

  ```
  bx cs api-key-info my_cluster
  ```
  {: pre}


### bx cs api-key-reset
{: #cs_api_key_reset}

Substitua a chave API do IAM atual em uma região do {{site.data.keyword.containershort_notm}}.

Esse comando requer a política de acesso de administrador do {{site.data.keyword.containershort_notm}} e armazena a chave API do usuário que executa esse comando na conta. A chave API do IAM é necessária para pedir infraestrutura do portfólio da infraestrutura do IBM Cloud (SoftLayer). Quando armazenada, a chave API é usada para cada ação em uma região que requer permissões de infraestrutura independentemente do usuário que executa esse comando. Para obter mais informações sobre como as chaves API do IAM funcionam, veja o [comando `bx cs api-key-info`](#cs_api_key_info).

**Importante** antes de usar esse comando, certifique-se de que o usuário que executa esse comando tenha as [permissões necessárias do {{site.data.keyword.containershort_notm}} e da infraestrutura do IBM Cloud (SoftLayer)](cs_users.html#users).

**Exemplo**:

  ```
  bx cs api-key-reset
  ```
  {: pre}


### bx cs apiserver-config-get
{: #cs_apiserver_config_get}

Obtenha informações sobre uma opção para a configuração do servidor de API do Kubernetes de um cluster. Esse comando deve ser combinado com um dos subcomandos a seguir para a opção de configuração sobre a qual você deseja informações.

#### bx cs apiserver-config-get audit-webhook CLUSTER
{: #cs_apiserver_api_webhook_get}

Visualize a URL para o serviço de criação de log remoto para o qual você está enviando logs de auditoria do servidor de API. A URL foi especificada quando você criou o backend de webhook para a configuração do servidor de API.

<strong>Opções de comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>
   </dl>

**Exemplo**:

  ```
  bx cs apiserver-config-get audit-webhook my_cluster
  ```
  {: pre}

### bx cs apiserver-config-set
{: #cs_apiserver_config_set}

Defina uma opção para a configuração do servidor da API do Kubernetes de um cluster. Esse comando deve ser combinado com um dos seguintes subcomandos para a opção de configuração que você deseja definir.

#### bx cs apiserver-config-set audit-webhook CLUSTER [--remoteServer SERVER_URL_OR_IP][--caCert CA_CERT_PATH] [--clientCert CLIENT_CERT_PATH][--clientKey CLIENT_KEY_PATH]
{: #cs_apiserver_api_webhook_set}

Configure o backend de webhook para a configuração do servidor de API. O backend de webhook encaminha os logs de auditoria do servidor de API para um servidor remoto. Uma configuração de webhook é criada com base nas informações fornecidas nas sinalizações desse comando. Se você não fornecer nenhuma informação nas sinalizações, uma configuração de webhook padrão será usada.

<strong>Opções de comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code>--remoteServer <em>SERVER_URL</em></code></dt>
   <dd>A URL ou o endereço IP para o serviço de criação de log remoto para o qual deseja enviar logs de auditoria. Se você fornecer uma URL insegura do servidor, todos os certificados serão ignorados. Esse valor é opcional.</dd>

   <dt><code>--caCert <em>CA_CERT_PATH</em></code></dt>
   <dd>O caminho de arquivo para o certificado de autoridade de certificação que é usado para verificar o serviço de criação de log remoto. Esse valor é opcional.</dd>

   <dt><code>--clientCert <em>CLIENT_CERT_PATH</em></code></dt>
   <dd>O caminho de arquivo para o certificado de cliente que é usado para autenticar com relação ao serviço de criação de log remoto. Esse valor é opcional.</dd>

   <dt><code>--clientKey <em> CLIENT_KEY_PATH</em></code></dt>
   <dd>O caminho de arquivo para a chave do cliente correspondente que é usada para se conectar ao serviço de registro remoto. Esse valor é opcional.</dd>
   </dl>

**Exemplo**:

  ```
  bx cs apiserver-config-set audit-webhook my_cluster --remoteServer https://audit.example.com/audit --caCert /mnt/etc/kubernetes/apiserver-audit/ca.pem --clientCert /mnt/etc/kubernetes/apiserver-audit/cert.pem --clientKey /mnt/etc/kubernetes/apiserver-audit/key.pem
  ```
  {: pre}


### bx cs apiserver-config-unset
{: #cs_apiserver_config_unset}

Desative uma opção para a configuração do servidor de API do Kubernetes de um cluster. Esse comando deve ser combinado com um dos subcomandos a seguir para a opção de configuração que você deseja desconfigurar.

#### bx cs apiserver-config-unset audit-webhook CLUSTER
{: #cs_apiserver_api_webhook_unset}

Desativar a configuração de backend do webhook para o servidor de API do cluster. Desativar o backend de webhook para o encaminhamento dos logs de auditoria do servidor da API para um servidor remoto.

<strong>Opções de comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>
   </dl>

**Exemplo**:

  ```
  bx cs apiserver-config-unset audit-webhook my_cluster
  ```
  {: pre}

### bx cs apiserver-refresh CLUSTER
{: #cs_apiserver_refresh}

Reinicie o mestre do Kubernetes no cluster para aplicar as mudanças na configuração do servidor de API.

<strong>Opções de comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>
   </dl>

**Exemplo**:

  ```
  bx cs apiserver-refresh my_cluster
  ```
  {: pre}


<br />


## Comandos de uso do plug-in da CLI
{: #cli_plug-in_commands}

### bx cs help
{: #cs_help}

Visualizar uma lista de comandos e parâmetros suportados.

<strong>Opções de comando</strong>:

   Nenhuma

**Exemplo**:

  ```
  bx cs help
  ```
  {: pre}


### bx cs init [--host HOST]
{: #cs_init}

Inicialize o plug-in do {{site.data.keyword.containershort_notm}} ou especifique a região em que você deseja criar ou acessar clusters do Kubernetes.

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--host <em>HOST</em></code></dt>
   <dd>O terminal de API do {{site.data.keyword.containershort_notm}} a ser usado.  Esse valor é opcional. [Visualize os valores de terminal de API disponíveis.](cs_regions.html#container_regions)</dd>
   </dl>

**Exemplo**:


```
bx cs init --host https://uk-south.containers.bluemix.net
```
{: pre}


### bx cs messages
{: #cs_messages}

Visualize as mensagens atuais para o usuário do IBMid.

**Exemplo**:

```
bx cs messages
```
{: pre}


<br />


## Comandos do cluster: gerenciamento
{: #cluster_mgmt_commands}


### bx cs cluster-config CLUSTER [--admin][--export]
{: #cs_cluster_config}

Após efetuar login, faça download dos dados de configuração e certificados do Kubernetes para se conectar ao seu cluster e execute comandos `kubectl`. Os arquivos são transferidos por download em `user_home_directory/.bluemix/plugins/container-service/clusters/<cluster_name>`.

**Opções de comando**:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code>--admin</code></dt>
   <dd>Faça download dos certificados TLS e dos arquivos de permissão para a função de Super usuário. É possível usar os certificados para automatizar tarefas em um cluster sem precisar autenticar novamente. Os arquivos são transferidos por download para `<user_home_directory>/.bluemix/plugins/container-service/clusters/<cluster_name>-admin`. Esse valor é opcional.</dd>

   <dt><code>--export</code></dt>
   <dd>Faça download dos dados de configuração e certificados do Kubernetes sem nenhuma mensagem diferente do comando de exportação. Como nenhuma mensagem é exibida, é possível usar essa sinalização quando você cria scripts automatizados. Esse valor é opcional.</dd>
   </dl>

**Exemplo**:

```
bx cs cluster-config my_cluster
```
{: pre}


### bx cs cluster-create [--file FILE_LOCATION][--hardware HARDWARE] --location LOCATION --machine-type MACHINE_TYPE --name NAME [--kube-version MAJOR.MINOR.PATCH][--no-subnet] [--private-vlan PRIVATE_VLAN][--public-vlan PUBLIC_VLAN] [--workers WORKER][--disable-disk-encrypt] [--trusted]
{: #cs_cluster_create}

Crie um cluster em sua organização. Para clusters gratuitos, você especifica o nome do cluster; tudo o mais é configurado com um valor padrão. Um cluster grátis é excluído automaticamente após 21 dias. É possível ter um cluster grátis de cada vez. Para aproveitar os recursos integrais do Kubernetes, crie um cluster padrão.

<strong>Opções de comandos</strong>

<dl>
<dt><code>--file <em>FILE_LOCATION</em></code></dt>

<dd>O caminho para o arquivo YAML para criar seu cluster padrão. Em vez de definir as características de seu cluster usando as opções fornecidas nesse comando, será possível usar um arquivo YAML.  Esse valor é opcional para clusters padrão e não está disponível para clusters livres.

<p><strong>Nota:</strong> se você fornecer a mesma opção no comando como parâmetro no arquivo YAML, o valor no comando terá precedência sobre o valor no YAML. Por exemplo, você define um local em seu arquivo YAML e usa a opção <code>--location</code> no comando, o valor inserido na opção de comando substituirá o valor no arquivo YAML.

<pre class="codeblock">
<code>name: <em>&lt;cluster_name&gt;</em>
location: <em>&lt;location&gt;</em>
no-subnet: <em>&lt;no-subnet&gt;</em>
machine-type: <em>&lt;machine_type&gt;</em>
private-vlan: <em>&lt;private_VLAN&gt;</em>
public-vlan: <em>&lt;public_VLAN&gt;</em>
hardware: <em>&lt;shared_or_dedicated&gt;</em>
workerNum: <em>&lt;number_workers&gt;</em>
kube-version: <em>&lt;kube-version&gt;</em>
diskEncryption: <em>false</em>
trusted: <em>true</em>
</code></pre>


<table>
    <caption>Tabela. Entendendo os componentes de arquivo YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes de arquivo do YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>name</em></code></td>
    <td>Substitua <code><em>&lt;cluster_name&gt;</em></code> por um nome para seu cluster. O nome deve iniciar com uma letra, pode conter letras, números e hífen (-) e deve ter 35 caracteres ou menos. O nome do cluster e a região na qual o cluster está implementado formam o nome completo do domínio para o subdomínio do Ingress. Para assegurar que o subdomínio do Ingress seja exclusivo dentro de uma região, o nome do cluster pode ser truncado e anexado com um valor aleatório dentro do nome de domínio do Ingress.
</td>
    </tr>
    <tr>
    <td><code><em>location</em></code></td>
    <td>Substitua <code><em>&lt;location&gt;</em></code> pelo local no qual você deseja criar seu cluster. Os locais disponíveis dependem da região a que você está conectado. Para listar os locais disponíveis, execute <code>bx cs locations</code>. </td>
     </tr>
     <tr>
     <td><code><em>no-subnet</em></code></td>
     <td>Por padrão, uma sub-rede móvel pública e uma privada são criadas na VLAN associada ao cluster. Substitua <code><em>&lt;no-subnet&gt;</em></code> por <code><em>true</em></code> para evitar a criação de sub-redes com o cluster. É possível [criar](#cs_cluster_subnet_create) ou [incluir](#cs_cluster_subnet_add) sub-redes em um cluster posteriormente.</td>
      </tr>
     <tr>
     <td><code><em>machine-type</em></code></td>
     <td>Substitua <code><em>&lt;machine_type&gt;</em></code> pelo tipo de máquina na qual você deseja implementar os nós do trabalhador. É possível implementar os nós do trabalhador como máquinas virtuais em hardware compartilhado ou dedicado ou como máquinas físicas no bare metal. Os tipos de máquinas físicas e virtuais disponíveis variam pelo local no qual o cluster é implementado. Para obter mais informações, veja a documentação do [comando](cs_cli_reference.html#cs_machine_types) `bx cs machine-type`.</td>
     </tr>
     <tr>
     <td><code><em>private-vlan</em></code></td>
     <td>Substitua <code><em>&lt;private_VLAN&gt;</em></code> pelo ID da VLAN privada que você deseja usar para os seus nós do trabalhador. Para listar as VLANs disponíveis, execute <code>bx cs vlans <em>&lt;location&gt;</em></code> e procure roteadores de VLAN iniciados com <code>bcr</code> (roteador de backend).</td>
     </tr>
     <tr>
     <td><code><em>public-vlan</em></code></td>
     <td>Substitua <code><em>&lt;public_VLAN&gt;</em></code> pelo ID da VLAN pública que você deseja usar para os seus nós do trabalhador. Para listar as VLANs disponíveis, execute <code>bx cs vlans <em>&lt;location&gt;</em></code> e procure roteadores de VLAN iniciados com <code>fcr</code> (roteador de front-end).</td>
     </tr>
     <tr>
     <td><code><em>hardware</em></code></td>
     <td>Para tipos de máquinas virtuais: o nível de isolamento de hardware para seu nó do trabalhador. Use dedicado se desejar que os recursos físicos disponíveis sejam dedicados somente a você ou compartilhado para permitir que os recursos físicos sejam compartilhados com outros clientes IBM. O padrão é <code>shared</code>.</td>
     </tr>
     <tr>
     <td><code><em>workerNum</em></code></td>
     <td>Substitua <code><em>&lt;number_workers&gt;</em></code> pelo número de nós do trabalhador que você deseja implementar.</td>
     </tr>
     <tr>
      <td><code><em>kube-version</em></code></td>
      <td>A versão do Kubernetes para o nó principal do cluster. Esse valor é opcional. Quando a versão não for especificada, o cluster será criado com o padrão de versões do Kubernetes suportadas. Para ver versões disponíveis, execute <code>bx cs kube-versions</code>.
</td></tr>
      <tr>
      <td><code>diskEncryption: <em>false</em></code></td>
      <td>Nós do trabalhador apresentam criptografia de disco por padrão; [saiba
mais](cs_secure.html#worker). Para desativar a criptografia, inclua essa opção e configure o valor para <code>false</code>.</td></tr>
      <tr>
      <td><code>trusted: <em>true</em></code></td>
      <td>**Somente bare metal**: ative [Cálculo confiável](cs_secure.html#trusted_compute) para verificar os nós do trabalhador do bare metal com relação à violação. Se você não ativar a confiança durante a criação do cluster, mas quiser fazer isso mais tarde, será possível usar o [comando](cs_cli_reference.html#cs_cluster_feature_enable) `bx cs feature-enable`. Depois de ativar a confiança, não é possível desativá-la posteriormente.</td></tr>
     </tbody></table>
    </p></dd>

<dt><code>--hardware <em>HARDWARE</em></code></dt>
<dd>O nível de isolamento de hardware para seu nó do trabalhador. Use dedicado para disponibilizar recursos físicos disponíveis apenas para você ou compartilhado para permitir que os recursos físicos sejam compartilhados com outros clientes da IBM. O padrão é shared.  Esse valor é opcional para clusters padrão e não está disponível para clusters livres.</dd>

<dt><code>--location <em>LOCATION</em></code></dt>
<dd>O local no qual você deseja criar o cluster. Os locais que estão disponíveis para você dependem da região do {{site.data.keyword.Bluemix_notm}} em que o login foi efetuado. Selecione a região fisicamente mais próxima de você para melhor desempenho.  Esse valor é necessário para clusters padrão e opcional para clusters livres.

<p>Revise [os locais disponíveis](cs_regions.html#locations).
</p>

<p><strong>Nota:</strong> ao selecionar um local fora de seu país, lembre-se de que você pode precisar de autorização legal para que os dados possam ser armazenados fisicamente em um país estrangeiro.</p>
</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>Escolha um tipo de máquina. É possível implementar os nós do trabalhador como máquinas virtuais em hardware compartilhado ou dedicado ou como máquinas físicas no bare metal. Os tipos de máquinas físicas e virtuais disponíveis variam pelo local no qual o cluster é implementado. Para obter mais informações, veja a documentação para o comando `bx cs machine-types` [](cs_cli_reference.html#cs_machine_types). Esse valor é necessário para clusters padrão e não está disponível para clusters livres.</dd>

<dt><code>--name <em>NAME</em></code></dt>
<dd>O nome para o cluster.  Este valor é necessário. O nome deve iniciar com uma letra, pode conter letras, números e hífen (-) e deve ter 35 caracteres ou menos. O nome do cluster e a região na qual o cluster está implementado formam o nome completo do domínio para o subdomínio do Ingress. Para assegurar que o subdomínio do Ingress seja exclusivo dentro de uma região, o nome do cluster pode ser truncado e anexado com um valor aleatório dentro do nome de domínio do Ingress.
</dd>

<dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
<dd>A versão do Kubernetes para o nó principal do cluster. Esse valor é opcional. Quando a versão não for especificada, o cluster será criado com o padrão de versões do Kubernetes suportadas. Para ver versões disponíveis, execute <code>bx cs kube-versions</code>.
</dd>

<dt><code>--no-subnet</code></dt>
<dd>Por padrão, uma sub-rede móvel pública e uma privada são criadas na VLAN associada ao cluster. Inclua a sinalização <code>--no-subnet</code> para evitar a criação de sub-redes com o cluster. É possível [criar](#cs_cluster_subnet_create) ou [incluir](#cs_cluster_subnet_add) sub-redes em um cluster posteriormente.</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>

<ul>
<li>Esse parâmetro não está disponível para clusters livres.</li>
<li>Se esse cluster padrão for o primeiro cluster padrão que você criar nesse local, não inclua essa sinalização. Uma VLAN privada é criada para você quando o cluster é criado.</li>
<li>Se você criou um cluster padrão antes neste local ou criou uma VLAN privada em infraestrutura do IBM Cloud (SoftLayer) antes, deve-se especificar essa VLAN privada.

<p><strong>Nota:</strong> os roteadores de VLAN privada sempre iniciam com <code>bcr</code> (roteador de backend) e os roteadores de VLAN pública sempre iniciam com <code>fcr</code> (roteador de front-end). Ao criar um cluster e especificar as VLANs públicas e privadas, o número e a combinação de letras após esses prefixos devem corresponder.</p></li>
</ul>

<p>Para descobrir se você já tem uma VLAN privada para um local específico ou para localizar o nome de uma VLAN privada existente, execute <code>bx cs vlans <em>&lt;location&gt;</em></code>.</p></dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>
<ul>
<li>Esse parâmetro não está disponível para clusters livres.</li>
<li>Se esse cluster padrão for o primeiro cluster padrão que você criar nesse local, não use essa sinalização. Uma VLAN pública é criada para você quando o cluster é criado.</li>
<li>Se você criou um cluster padrão antes neste local ou criou uma VLAN pública em infraestrutura do IBM Cloud (SoftLayer) antes, deve-se especificar essa VLAN pública.

<p><strong>Nota:</strong> os roteadores de VLAN privada sempre iniciam com <code>bcr</code> (roteador de backend) e os roteadores de VLAN pública sempre iniciam com <code>fcr</code> (roteador de front-end). Ao criar um cluster e especificar as VLANs públicas e privadas, o número e a combinação de letras após esses prefixos devem corresponder.</p></li>
</ul>

<p>Para descobrir se você já tem uma VLAN pública para um local específico ou para localizar o nome de uma VLAN pública existente, execute <code>bx cs vlans <em>&lt;location&gt;</em></code>.</p></dd>

<dt><code>--workers WORKER</code></dt>
<dd>O número de nós do trabalhador que
você deseja implementar em seu cluster. Se não
especificar essa opção, um cluster com 1 nó do trabalhador será criado. Esse valor é opcional para clusters padrão e não está disponível para clusters livres.

<p><strong>Nota:</strong> a cada nó do trabalhador é designado um ID de nó do trabalhador e um nome de domínio exclusivos que não devem ser mudados manualmente após a criação do cluster. Mudar o ID ou o nome do domínio evita que o mestre do Kubernetes gerencie o cluster.</p></dd>

<dt><code>--disable-disk-encrypt</code></dt>
<dd>Nós do trabalhador apresentam criptografia de disco por padrão; [saiba
mais](cs_secure.html#worker). Para desativar a criptografia, inclua essa opção.</dd>

<dt><code>--trusted</code></dt>
<dd><p>**Somente bare metal**: ative [Cálculo confiável](cs_secure.html#trusted_compute) para verificar os nós do trabalhador do bare metal com relação à violação. Se você não ativar a confiança durante a criação do cluster, mas quiser fazer isso mais tarde, será possível usar o [comando](cs_cli_reference.html#cs_cluster_feature_enable) `bx cs feature-enable`. Depois de ativar a confiança, não é possível desativá-la posteriormente.</p>
<p>Para verificar se o tipo de máquina bare metal suporta confiança, verifique o campo `Trustable` na saída de `bx cs machine-types <location>` [Comando](#cs_machine_types). Para verificar se um cluster está com a confiança ativada, visualize o campo **Confiança pronta** na saída do [comando](#cs_cluster_get) `bx cs cluster-get`. Para verificar se um nó do trabalhador bare metal está com a confiança ativada, visualize o campo **Confiança** na saída do [comando](#cs_worker_get) `bx cs worker-get`.</p></dd>
</dl>

**Exemplos**:

  

  Exemplo para um cluster padrão:
  {: #example_cluster_create}

  ```
  bx cs cluster-create --location dal10 --public-vlan my_public_VLAN_ID --private-vlan my_private_VLAN_ID --machine-type u2c.2x4 --name my_cluster --hardware shared --workers 2
  ```
  {: pre}

  Exemplo para um cluster grátis:

  ```
  bx cs cluster-create --name my_cluster
  ```
  {: pre}

  Exemplo para um ambiente do {{site.data.keyword.Bluemix_dedicated_notm}}:

  ```
  bx cs cluster-create --machine-type machine-type --workers number --name cluster_name
  ```
  {: pre}

### bx cs cluster-feature-enable CLUSTER [--trusted]
{: #cs_cluster_feature_enable}

Ative um recurso em um cluster existente.

<strong>Opções de comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code><em>--trusted</em></code></dt>
   <dd><p>Inclua a sinalização para ativar o [Cálculo confiável](cs_secure.html#trusted_compute) para todos os nós do trabalhador bare metal suportados que estiverem no cluster. Depois de ativar a confiança, não é possível desativá-la posteriormente para o cluster.</p>
   <p>Para verificar se o tipo de máquina bare metal suporta confiança, verifique o campo **Trustable** na saída de `bx cs machine-types <location>` [Comando](#cs_machine_types). Para verificar se um cluster está com a confiança ativada, visualize o campo **Confiança pronta** na saída do [comando](#cs_cluster_get) `bx cs cluster-get`. Para verificar se um nó do trabalhador bare metal está com a confiança ativada, visualize o campo **Confiança** na saída do [comando](#cs_worker_get) `bx cs worker-get`.</p></dd>
   </dl>

**Comando de exemplo**:

  ```
  bx cs cluster-feature-enable my_cluster --trusted=true
  ```
  {: pre}

### bx cs cluster-get CLUSTER [--showResources]
{: #cs_cluster_get}

Visualizar informações sobre um cluster em sua organização.

<strong>Opções de comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code><em>--showResources</em></code></dt>
   <dd>Mostre mais recursos de cluster, como complementos, VLANs, sub-redes e armazenamento.</dd>
   </dl>

**Comando de exemplo**:

  ```
  bx cs cluster-get my_cluster --showResources
  ```
  {: pre}

**Saída de exemplo**:

  ```
  Name: my_cluster ID: abc1234567 State: normal Trust ready: false Created: 2018-01-01T17:19:28+0000 Location: dal10 Master URL: https://169.xx.xxx.xxx:xxxxx Ingress subdomain: my_cluster.us-south.containers.appdomain.cloud Ingress secret: my_cluster Workers: 3 Version: 1.7.16_1511* (1.8.11_1509 latest) Owner Email: name@example.com Monitoring dashboard: https://metrics.ng.bluemix.net/app/#/grafana4/dashboard/db/link

  Addons
  Name                   Enabled
  customer-storage-pod   true
  basic-ingress-v2       true
  storage-watcher-pod    true

  Subnet VLANs VLAN ID Subnet CIDR Public User-managed 2234947 10.xxx.xx.xxx/29 false false 2234945 169.xx.xxx.xxx/29 true false

  ```
  {: screen}

### bx cs cluster-rm [-f] CLUSTER
{: #cs_cluster_rm}

Remover um cluster de sua organização.

<strong>Opções de comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code>-f</code></dt>
   <dd>Use esta opção para forçar a remoção de um cluster sem avisos do usuário. Esse valor é opcional.</dd>
   </dl>

**Exemplo**:

  ```
  bx cs cluster-rm my_cluster
  ```
  {: pre}


### bx cs cluster-update [-f] CLUSTER [--kube-version MAJOR.MINOR.PATCH][--force-update]
{: #cs_cluster_update}

Atualize o mestre do Kubernetes para a versão de API padrão. Durante a atualização, não é possível acessar nem mudar o cluster. Nós do trabalhador, apps e recursos que foram implementados pelo usuário não são modificados e continuam a ser executados.

Pode ser necessário mudar seus arquivos YAML para implementações futuras. Revise essa [nota sobre a liberação](cs_versions.html) para obter detalhes.

<strong>Opções de comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
   <dd>A versão do Kubernetes do cluster. Se você não especificar uma versão, o mestre do Kubernetes será atualizado para a versão de API padrão. Para ver versões disponíveis, execute [bx cs kube-versions](#cs_kube_versions). Esse valor é opcional.</dd>

   <dt><code>-f</code></dt>
   <dd>Use esta opção para forçar a atualização do mestre sem avisos do usuário. Esse valor é opcional.</dd>

   <dt><code>--force-update</code></dt>
   <dd>Tente a atualização mesmo se a mudança for maior que duas versões secundárias. Esse valor é opcional.</dd>
   </dl>

**Exemplo**:

  ```
  bx cs cluster-update my_cluster
  ```
  {: pre}


### bx cs clusters
{: #cs_clusters}

Visualizar uma lista de clusters em sua organização.

<strong>Opções de comando</strong>:

  Nenhuma

**Exemplo**:

  ```
  bx cs clusters
  ```
  {: pre}


### bx cs kube-versions
{: #cs_kube_versions}

Visualize uma lista de versões do Kubernetes suportadas em {{site.data.keyword.containershort_notm}}. Atualize o seu [cluster mestre](#cs_cluster_update) e [nós do trabalhador](cs_cli_reference.html#cs_worker_update) para a versão padrão para os recursos mais recentes, estáveis.

**Opções de comando**:

  Nenhuma

**Exemplo**:

  ```
  bx cs kube-versions
  ```
  {: pre}



<br />



## Comandos do cluster: serviços e integrações
{: #cluster_services_commands}


### bx cs cluster-service-bind CLUSTER KUBERNETES_NAMESPACE SERVICE_INSTANCE_NAME
{: #cs_cluster_service_bind}

Inclua um serviço do {{site.data.keyword.Bluemix_notm}} em um cluster. Para visualizar os serviços do {{site.data.keyword.Bluemix_notm}} disponíveis no catálogo do {{site.data.keyword.Bluemix_notm}}, execute `bx service offerings`. **Nota**: é possível incluir somente serviços do {{site.data.keyword.Bluemix_notm}} que suportam chaves de serviço.

<strong>Opções de comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code><em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>O nome do namespace do Kubernetes. Este valor é obrigatório.</dd>

   <dt><code><em>SERVICE_INSTANCE_NAME</em></code></dt>
   <dd>O nome da instância de serviço do {{site.data.keyword.Bluemix_notm}} que você deseja ligar. Para localizar o nome de sua instância de serviço, execute <code>bx service list</code>. Se mais de uma instância tiver o mesmo nome na conta, use o ID da instância de serviço no lugar do nome. Para localizar o ID, execute <code>bx service show <service instance name> --guid</code>. Um desses valores é necessário.</dd>
   </dl>

**Exemplo**:

  ```
  bx cs cluster-service-bind my_cluster my_namespace my_service_instance
  ```
  {: pre}


### bx cs cluster-service-unbind CLUSTER KUBERNETES_NAMESPACE SERVICE_INSTANCE_GUID
{: #cs_cluster_service_unbind}

Remova um serviço do {{site.data.keyword.Bluemix_notm}} de um cluster.

**Nota:** ao remover um serviço do {{site.data.keyword.Bluemix_notm}}, as credenciais de serviço serão removidas do cluster. Se um pod ainda está usando o serviço,
ele falha porque as credenciais de serviço não podem ser localizadas.

<strong>Opções de comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code><em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>O nome do namespace do Kubernetes. Este valor é obrigatório.</dd>

   <dt><code><em>SERVICE_INSTANCE_GUID</em></code></dt>
   <dd>O ID da instância de serviço do {{site.data.keyword.Bluemix_notm}} que você deseja remover. Para localizar o ID da instância de serviço, execute `bx cs cluster-services <cluster_name_or_ID>`.Esse valor é necessário.</dd>
   </dl>

**Exemplo**:

  ```
  bx cs cluster-service-unbind my_cluster my_namespace 8567221
  ```
  {: pre}


### bx cs cluster-services CLUSTER [--namespace KUBERNETES_NAMESPACE][--all-namespaces]
{: #cs_cluster_services}

Listar os serviços que estão ligados a um ou todos os namespaces do Kubernetes em um cluster. Se nenhuma
opção é especificada, os serviços para o namespace padrão são exibidos.

<strong>Opções de comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code>, <code>-n <em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>Inclua os serviços que forem limitados a um namespace específico em um cluster. Esse valor é opcional.</dd>

   <dt><code>--all-namespaces</code></dt>
    <dd>Inclua os serviços que forem limitados a todos os namespaces em um cluster. Esse valor é opcional.</dd>
    </dl>

**Exemplo**:

  ```
  bx cs cluster-services my_cluster --namespace my_namespace
  ```
  {: pre}



### bx cs webhook-create --cluster CLUSTER --level LEVEL --type slack --url URL
{: #cs_webhook_create}

Registre um webhook.

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code>--level <em>LEVEL</em></code></dt>
   <dd>O nível de notificação, como <code>Normal</code> ou <code>Warning</code>. <code>Warning</code> é o valor padrão. Esse valor é opcional.</dd>

   <dt><code>--type <em>slack</em></code></dt>
   <dd>O tipo de webhook. A folga é atualmente suportada. Este valor é obrigatório.</dd>

   <dt><code>--url <em>URL</em></code></dt>
   <dd>A URL para o webhook. Este valor é obrigatório.</dd>
   </dl>

**Exemplo**:

  ```
  bx cs webhook-create --cluster my_cluster --level Normal --type slack --url http://github.com/mywebhook
  ```
  {: pre}


<br />


## Comandos do cluster: Subnets
{: #cluster_subnets_commands}

### bx cs cluster-subnet-add CLUSTER SUBNET
{: #cs_cluster_subnet_add}

Faça uma sub-rede em uma conta de infraestrutura do IBM Cloud (SoftLayer) disponível para um cluster especificado.

**Nota:**
* Quando você torna uma sub-rede disponível para um cluster, os endereços IP
dessa sub-rede são usados para propósitos de rede do cluster. Para evitar conflitos de endereço IP, certifique-se de usar uma sub-rede com somente um cluster. Não use uma sub-rede para múltiplos clusters ou para outros
propósitos fora do {{site.data.keyword.containershort_notm}} ao mesmo
tempo.
* Para rotear entre sub-redes na mesma VLAN, deve-se [ativar a ampliação de VLAN](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning).

<strong>Opções de comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code><em>SUBNET</em></code></dt>
   <dd>O ID da sub-rede. Este valor é obrigatório.</dd>
   </dl>

**Exemplo**:

  ```
  bx cs cluster-subnet-add my_cluster 1643389
  ```
  {: pre}


### bx cs cluster-subnet-create CLUSTER SIZE VLAN_ID
{: #cs_cluster_subnet_create}

Crie uma sub-rede em uma conta de infraestrutura do IBM Cloud (SoftLayer) e torne-a disponível para um cluster especificado em {{site.data.keyword.containershort_notm}}.

**Nota:**
* Quando você torna uma sub-rede disponível para um cluster, os endereços IP
dessa sub-rede são usados para propósitos de rede do cluster. Para evitar conflitos de endereço IP, certifique-se de usar uma sub-rede com somente um cluster. Não use uma sub-rede para múltiplos clusters ou para outros
propósitos fora do {{site.data.keyword.containershort_notm}} ao mesmo
tempo.
* Para rotear entre sub-redes na mesma VLAN, deve-se [ativar a ampliação de VLAN](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning).

<strong>Opções de comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é necessário. Para listar seus clusters, use o [comando](#cs_clusters) `bx cs clusters`.</dd>

   <dt><code><em>SIZE</em></code></dt>
   <dd>O número de endereços IP de sub-rede. Este valor é necessário. Os valores possíveis são 8, 16, 32 ou 64.</dd>

   <dt><code><em>VLAN_ID</em></code></dt>
   <dd>A VLAN na qual criar a sub-rede. Este valor é necessário. Para listar VLANS disponíveis, use o comando `bx cs vlans <location>` [](#cs_vlans). </dd>
   </dl>

**Exemplo**:

  ```
  bx cs cluster-subnet-create my_cluster 8 1764905
  ```
  {: pre}


### bx cs cluster-user-subnet-add CLUSTER SUBNET_CIDR PRIVATE_VLAN
{: #cs_cluster_user_subnet_add}

Traga a sua própria sub-rede privada para os seus clusters do {{site.data.keyword.containershort_notm}}.

Essa sub-rede privada não é uma fornecida pela infraestrutura do IBM Cloud (SoftLayer). Como tal, deve-se configurar qualquer roteamento de tráfego de rede de entrada e saída para a sub-rede. Para incluir uma sub-rede de infraestrutura do IBM Cloud (SoftLayer), use o comando `bx cs cluster-subnet-add` [](#cs_cluster_subnet_add).

**Nota**:
* Quando você inclui uma sub-rede privada de usuário em um cluster, os endereços IP dessa sub-rede são usados para Load Balancers privados no cluster. Para evitar conflitos de endereço IP, certifique-se de usar uma sub-rede com somente um cluster. Não use uma sub-rede para múltiplos clusters ou para outros
propósitos fora do {{site.data.keyword.containershort_notm}} ao mesmo
tempo.
* Para rotear entre sub-redes na mesma VLAN, deve-se [ativar a ampliação de VLAN](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning).

<strong>Opções de comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code><em>SUBNET_CIDR</em></code></dt>
   <dd>O Classless InterDomain Routing (CIDR) de sub-rede. Esse valor é obrigatório e não deverá entrar em conflito com qualquer sub-rede que for usada pela infraestrutura do IBM Cloud (SoftLayer).

   Os prefixos suportados variam de `/30` (1 endereço IP) a `/24` (253 endereços IP). Se você definir o CIDR com um comprimento de prefixo e posteriormente precisar mudá-lo, primeiro inclua o novo CIDR, então, [remova o CIDR antigo](#cs_cluster_user_subnet_rm).</dd>

   <dt><code><em>PRIVATE_VLAN</em></code></dt>
   <dd>O ID da VLAN privada. Este valor é necessário. Ele deverá corresponder ao ID de VLAN privada de um ou mais nós do trabalhador no cluster.</dd>
   </dl>

**Exemplo**:

  ```
  bx cs cluster-user-subnet-add my_cluster 169.xx.xxx.xxx/29 1502175
  ```
  {: pre}


### bx cs cluster-user-subnet-rm CLUSTER SUBNET_CIDR PRIVATE_VLAN
{: #cs_cluster_user_subnet_rm}

Remova a sua própria sub-rede privada de um cluster especificado.

**Nota:** qualquer serviço que foi implementado em um endereço IP por meio de sua própria sub-rede privada permanecerá ativo após a sub-rede ser removida.

<strong>Opções de comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code><em>SUBNET_CIDR</em></code></dt>
   <dd>O Classless InterDomain Routing (CIDR) de sub-rede. Esse valor é obrigatório e deverá corresponder ao CIDR que foi configurado pelo comando `bx cs cluster-user-subnet-add` [](#cs_cluster_user_subnet_add).</dd>

   <dt><code><em>PRIVATE_VLAN</em></code></dt>
   <dd>O ID da VLAN privada. Esse valor é obrigatório e deverá corresponder ao ID de VLAN que foi configurado pelo comando `bx cs cluster-user-subnet-add` [](#cs_cluster_user_subnet_add).</dd>
   </dl>

**Exemplo**:

  ```
  bx cs cluster-user-subnet-rm my_cluster 169.xx.xxx.xxx/29 1502175
  ```
  {: pre}

### bx cs subnets
{: #cs_subnets}

Visualize uma lista de sub-redes que estão disponíveis em uma conta de infraestrutura do IBM Cloud (SoftLayer).

<strong>Opções de comando</strong>:

   Nenhuma

**Exemplo**:

  ```
  bx cs subnets
  ```
  {: pre}


<br />


## Comandos do balanceador de carga do aplicativo (ALB) Ingress
{: #alb_commands}

### bx cs alb-cert-deploy [--update] --cluster CLUSTER --secret-name SECRET_NAME --cert-crn CERTIFICATE_CRN
{: #cs_alb_cert_deploy}

Implemente ou atualize um certificado de sua instância do {{site.data.keyword.cloudcerts_long_notm}} para o ALB em um cluster.

**Nota:**
* Somente um usuário com a função de acesso do Administrador pode executar esse comando.
* É possível atualizar somente certificados que são importados da mesma instância do {{site.data.keyword.cloudcerts_long_notm}}.

<strong>Opções de comandos</strong>

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code>--update</code></dt>
   <dd>Atualize o certificado para um segredo do ALB em um cluster. Esse valor é opcional.</dd>

   <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
   <dd>O nome do segredo do ALB. Este valor é obrigatório.</dd>

   <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
   <dd>O CRN do certificado. Este valor é obrigatório.</dd>
   </dl>

**Exemplos**:

Exemplo para implementar um segredo do ALB:

   ```
   bx cs alb-cert-deploy --secret-name my_alb_secret --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
   ```
   {: pre}

Exemplo para atualizar um segredo do ALB existente:

 ```
 bx cs alb-cert-deploy --update --secret-name my_alb_secret --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:7e21fde8ee84a96d29240327daee3eb2
 ```
 {: pre}


### bx cs alb-cert-get --cluster CLUSTER [--secret-name SECRET_NAME][--cert-crn CERTIFICATE_CRN]
{: #cs_alb_cert_get}

Visualize informações sobre um segredo do ALB em um cluster.

**Observação:** somente um usuário com a função de acesso do administrador pode executar esse comando.

<strong>Opções de comandos</strong>

  <dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
  <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

  <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
  <dd>O nome do segredo do ALB. Esse valor é necessário para obter informações sobre um segredo do ALB específico no cluster.</dd>

  <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
  <dd>O CRN do certificado. Esse valor é necessário para obter informações sobre todos os segredos do ALB correspondentes a um CRN de certificado específico no cluster.</dd>
  </dl>

**Exemplos**:

 Exemplo para buscar informações sobre um segredo do ALB:

 ```
 bx cs alb-cert-get --cluster my_cluster --secret-name my_alb_secret
 ```
 {: pre}

 Exemplo para buscar informações sobre todos os segredos do ALB que correspondem a um CRN de certificado especificado:

 ```
 bx cs alb-cert-get --cluster my_cluster --cert-crn  crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
 ```
 {: pre}


### bx cs alb-cert-rm --cluster CLUSTER [--secret-name SECRET_NAME][--cert-crn CERTIFICATE_CRN]
{: #cs_alb_cert_rm}

Remova um segredo do ALB em um cluster.

**Observação:** somente um usuário com a função de acesso do administrador pode executar esse comando.

<strong>Opções de comandos</strong>

  <dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
  <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

  <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
  <dd>O nome do segredo do ALB. Esse valor é necessário para remover um segredo do ALB específico no cluster.</dd>

  <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
  <dd>O CRN do certificado. Esse valor é necessário para remover todos os segredos do ALB correspondentes a um CRN de certificado específico no cluster.</dd>
  </dl>

**Exemplos**:

 Exemplo para remover um segredo do ALB:

 ```
 bx cs alb-cert-rm --cluster my_cluster --secret-name my_alb_secret
 ```
 {: pre}

 Exemplo para remover todos os segredos do ALB que correspondem a um CRN de certificado especificado:

 ```
 bx cs alb-cert-rm --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
 ```
 {: pre}


### bx cs alb-certs --cluster CLUSTER
{: #cs_alb_certs}

Visualize uma lista de segredos do ALB em um cluster.

**Nota:** apenas usuários com a função de acesso do Administrador podem executar esse comando.

<strong>Opções de comandos</strong>

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>
   </dl>

**Exemplo**:

 ```
 bx cs alb-certs --cluster my_cluster
 ```
 {: pre}

### bx cs alb-configure --albID ALB_ID [--enable][--disable][--user-ip USERIP]
{: #cs_alb_configure}

Ative ou desative um ALB em seu cluster padrão. O ALB público é ativado por padrão.

**Opções de comando**:

   <dl>
   <dt><code><em>--albID </em>ALB_ID</code></dt>
   <dd>O ID para um ALB. Execute <code>bx cs albs <em>--cluster </em>CLUSTER</code> para visualizar os IDs para os ALBs em um cluster. Este valor é obrigatório.</dd>

   <dt><code>--enable</code></dt>
   <dd>Inclua essa sinalização para ativar um ALB em um cluster.</dd>

   <dt><code>--disable</code></dt>
   <dd>Inclua essa sinalização para desativar um ALB em um cluster.</dd>

   <dt><code>--user-ip <em>USER_IP</em></code></dt>
   <dd>

   <ul>
    <li>Esse parâmetro está disponível para ativar um ALB privado apenas.</li>
    <li>O ALB privado é implementado com um endereço IP de uma sub-rede privada fornecida pelo usuário. Se nenhum endereço IP for fornecido, o ALB será implementado com um endereço IP privado da sub-rede privada móvel que foi provisionada automaticamente quando você criou o cluster.</li>
   </ul>
   </dd>
   </dl>

**Exemplos**:

  Exemplo para ativar um ALB:

  ```
  bx cs alb-configure --albID private-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --enable
  ```
  {: pre}

  Exemplo para ativar um ALB com um endereço IP fornecido pelo usuário:

  ```
  bx cs alb-configure --albID private-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --enable --user-ip user_ip
  ```
  {: pre}

  Exemplo para desativar um ALB:

  ```
  bx cs alb-configure --albID public-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --disable
  ```
  {: pre}

### bx cs alb-get --albID ALB_ID
{: #cs_alb_get}

Visualize os detalhes de um ALB.

<strong>Opções de comando</strong>:

   <dl>
   <dt><code><em>--albID </em>ALB_ID</code></dt>
   <dd>O ID para um ALB. Execute <code>bx cs albs --cluster <em>CLUSTER</em></code> para visualizar os IDs para os ALBs em um cluster. Este valor é obrigatório.</dd>
   </dl>

**Exemplo**:

  ```
  bx cs alb-get --albID public-cr18a61a63a6a94b658596aa93a087aaa9-alb1
  ```
  {: pre}

### bx cs alb-types
{: #cs_alb_types}

Visualize os tipos de ALB que são suportados na região.

<strong>Opções de comando</strong>:

   Nenhuma

**Exemplo**:

  ```
  bx cs alb-types
  ```
  {: pre}


### bx cs albs --cluster CLUSTER
{: #cs_albs}

Visualize o status de todos os ALBs em um cluster. Se nenhum ID de ALB for retornado, então, o cluster não terá uma sub-rede portátil. É possível [criar](#cs_cluster_subnet_create) ou [incluir](#cs_cluster_subnet_add) sub-redes em um cluster.

<strong>Opções de comando</strong>:

   <dl>
   <dt><code><em>--cluster </em>CLUSTER</code></dt>
   <dd>O nome ou ID do cluster no qual você lista os ALBs disponíveis. Este valor é obrigatório.</dd>
   </dl>

**Exemplo**:

  ```
  bx cs albs --cluster my_cluster
  ```
  {: pre}


<br />


## Comandos de infraestrutura
{: #infrastructure_commands}

### bx cs credentials-set --infrastructure-api-key API_KEY --infrastructure-username USERNAME
{: #cs_credentials_set}

Configure as credenciais de conta de infraestrutura do IBM Cloud (SoftLayer) para a sua conta do {{site.data.keyword.containershort_notm}}.

Se você tiver uma {{site.data.keyword.Bluemix_notm}}conta pré-paga, terá acesso ao portfólio de infraestrutura do IBM Cloud (SoftLayer) por padrão. No entanto, talvez você queira usar uma conta de infraestrutura do IBM Cloud (SoftLayer) diferente da que você já tem para pedir a infraestrutura. É possível vincular essa conta de infraestrutura à sua conta do {{site.data.keyword.Bluemix_notm}} conta usando este comando.

Se as credenciais de infraestrutura do IBM Cloud (SoftLayer) são configuradas manualmente, essas credenciais são usadas para pedir infraestrutura, mesmo se uma [chave API do IAM](#cs_api_key_info) já existe para a conta. Se o usuário cujas credenciais estão armazenadas não tiver as permissões necessárias para pedir a infraestrutura, as ações relacionadas à infraestrutura, como a criação de um cluster ou o recarregamento de um nó do trabalhador, poderão falhar.

Não é possível configurar múltiplas credenciais para uma conta do {{site.data.keyword.containershort_notm}}. Cada conta do {{site.data.keyword.containershort_notm}} é vinculada a um portfólio de infraestrutura do IBM Cloud (SoftLayer) apenas.

**Importante:** antes de usar esse comando, certifique-se de que o usuário cujas credenciais são usadas tem as [permissões necessárias do {{site.data.keyword.containershort_notm}} e da infraestrutura do IBM Cloud (SoftLayer)](cs_users.html#users).

<strong>Opções de comando</strong>:

   <dl>
   <dt><code>--infrastructure-username <em>USERNAME</em></code></dt>
   <dd>Nome do usuário de infraestrutura do IBM Cloud (SoftLayer). Este valor é obrigatório.</dd>


   <dt><code>--infrastructure-api-key <em>API_KEY</em></code></dt>
   <dd>Chave API de infraestrutura do IBM Cloud infrastructure (SoftLayer). Este valor é obrigatório.

 <p>
  Para gerar uma chave API:

  <ol>
  <li>Efetue login no portal de infraestrutura do [IBM Cloud (SoftLayer)![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/).</li>
  <li>Selecione <strong>Conta</strong> e, em seguida, <strong>Usuários</strong>.</li>
  <li>Clique em <strong>Gerar</strong> para gerar uma chave API de infraestrutura do IBM Cloud (SoftLayer) para a sua conta.</li>
  <li>Copie a chave API para usar nesse comando.</li>
  </ol>

  Para visualizar sua chave API existente:
  <ol>
  <li>Efetue login no portal de infraestrutura do [IBM Cloud (SoftLayer)![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/).</li>
  <li>Selecione <strong>Conta</strong> e, em seguida, <strong>Usuários</strong>.</li>
  <li>Clique em <strong>Visualizar</strong> para ver sua chave API existente.</li>
  <li>Copie a chave API para usar nesse comando.</li>
  </ol>
  </p></dd>
  </dl>

**Exemplo**:

  ```
  bx cs credentials-set --infrastructure-api-key <api_key> --infrastructure-username dbmanager
  ```
  {: pre}


### bx cs credentials-unset
{: #cs_credentials_unset}

Remova as credenciais da conta de infraestrutura do IBM Cloud (SoftLayer) de sua conta do {{site.data.keyword.containershort_notm}}.

Depois de remover as credenciais, a [chave API do IAM](#cs_api_key_info) é usada para pedir recursos em infraestrutura do IBM Cloud (SoftLayer).

<strong>Opções de comando</strong>:

   Nenhuma

**Exemplo**:

  ```
  bx cs credentials-unset
  ```
  {: pre}


### bx cs machine-types LOCATION
{: #cs_machine_types}

Visualizar uma lista de tipos de máquina disponíveis para seus nós do trabalhador. Cada tipo de máquina inclui a
quantia de CPU, memória e espaço em disco virtual para cada nó do trabalhador no cluster. Por padrão, o diretório `/var/lib/docker`, no qual todos os dados de contêiner são armazenados, é criptografado com criptografia LUKS. Se a opção `disable-disk-encrypt` for incluída durante a criação do cluster, os dados do Docker do host não estarão criptografados. [Saiba mais sobre a criptografia.](cs_secure.html#encrypted_disks)
{:shortdesc}

É possível provisionar o nó do trabalhador como uma máquina virtual em hardware compartilhado ou dedicado ou como uma máquina física em bare metal.

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


<strong>Opções de comando</strong>:

   <dl>
   <dt><code><em>LOCATION</em></code></dt>
   <dd>Insira o local no qual você deseja listar tipos de máquina disponíveis. Este valor é necessário. Revise [os locais disponíveis](cs_regions.html#locations).</dd></dl>

**Comando de exemplo**:

  ```
  bx cs machine-types dal10
  ```
  {: pre}

**Saída de exemplo**:

  ```
  Getting machine types list...
  OK
  Machine Types
  Name                 Cores   Memory   Network Speed   OS             Server Type   Storage   Secondary Storage   Trustable
  u2c.2x4              2       4GB      1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  b2c.4x16             4       16GB     1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  b2c.16x64            16      64GB     1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  b2c.32x128           32      128GB    1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  b2c.56x242           56      242GB    1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  mb1c.4x32            4       32GB     10000Mbps       UBUNTU_16_64   physical      1000GB    2000GB              False
  mb1c.16x64           16      64GB     10000Mbps       UBUNTU_16_64   physical      1000GB    1700GB              False
  mr1c.28x512          28      512GB    10000Mbps       UBUNTU_16_64   physical      1000GB    1700GB              False
  md1c.16x64.4x4tb     16      64GB     10000Mbps       UBUNTU_16_64   physical      1000GB    8000GB              False
  md1c.28x512.4x4tb    28      512GB    10000Mbps       UBUNTU_16_64   physical      1000GB    8000GB              False
  
  ```
  {: screen}


### bx cs vlans LOCATION [--all]
{: #cs_vlans}

Liste as VLANs públicas e privadas que estão disponíveis para um local em sua conta de infraestrutura do IBM Cloud (SoftLayer). Para listar as VLANs disponíveis, deve-se
ter uma conta paga.

<strong>Opções de comando</strong>:

   <dl>
   <dt><code><em>LOCATION</em></code></dt>
   <dd>Insira o local no qual você deseja listar as suas VLANs públicas e privadas. Este valor é necessário. Revise [os locais disponíveis](cs_regions.html#locations).</dd>
   <dt><code>--all</code></dt>
   <dd>Lista todas as VLANs disponíveis. Por padrão, as VLANs são filtradas para mostrar somente aquelas VLANs que são válidas. Para ser válida, uma VLAN deve ser associada à infraestrutura que pode hospedar um trabalhador com armazenamento em disco local.</dd>
   </dl>

**Exemplo**:

  ```
  bx cs vlans dal10
  ```
  {: pre}


<br />


## Comandos de criação de log
{: #logging_commands}

### bx cs logging-config-create CLUSTER --logsource LOG_SOURCE [--namespace KUBERNETES_NAMESPACE][--hostname LOG_SERVER_HOSTNAME_OR_IP] [--port LOG_SERVER_PORT][--space CLUSTER_SPACE] [--org CLUSTER_ORG][--app-containers CONTAINERS] [--app-paths PATHS_TO_LOGS] --type LOG_TYPE [--json][--skip-validation]
{: #cs_logging_create}

Crie uma configuração de criação de log. É possível usar esse comando para encaminhar logs para contêineres, aplicativos, nós do trabalhador, clusters do Kubernetes e balanceadores de carga do aplicativo Ingress para o {{site.data.keyword.loganalysisshort_notm}} ou para um servidor syslog externo.

<strong>Opções de comando</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>O nome ou ID do cluster.</dd>
  <dt><code>--logsource <em>LOG_SOURCE</em></code></dt>
    <dd>A origem do log para o qual ativar o encaminhamento de log. Esse argumento suporta uma lista separada por vírgula de origens de log para aplicar a configuração. Os valores aceitos são <code>container</code>, <code>application</code>, <code>worker</code>,
<code>kubernetes</code> e <code>ingress</code>. Se você não fornecer uma origem de log, configurações de criação de log serão criadas para as origens de log <code>container</code> e <code>ingress</code>.</dd>
  <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
    <dd>O namespace do Kubernetes do qual você deseja encaminhar logs. O encaminhamento de log não é suportado para os namespaces do Kubernetes <code>ibm-system</code> e <code>kube-system</code>. Esse valor é válido somente para a origem de log do contêiner e é opcional. Se você não especificar um namespace, todos os namespaces no cluster usarão essa configuração.</dd>
  <dt><code>--hostname <em>LOG_SERVER_HOSTNAME</em></code></dt>
    <dd>Quando o tipo de criação de log for <code>syslog</code>, o nome do host ou endereço IP do servidor do coletor do log. Esse valor é necessário para <code>syslog</code>. Quando o tipo de criação de log for <code>ibm</code>, a URL de ingestão {{site.data.keyword.loganalysislong_notm}}. É possível localizar a lista de URLs de ingestão disponíveis [aqui](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls). Se você não especificar uma URL de ingestão, o endpoint para a região na qual seu cluster foi criado será usado.</dd>
  <dt><code>--port <em>LOG_SERVER_PORT</em></code></dt>
    <dd>A porta do servidor coletor do log. Esse valor é opcional. Se você não especificar uma porta, a porta padrão <code>514</code> será usada para <code>syslog</code> e a porta padrão <code>9091</code> será usada para <code>ibm</code>.</dd>
  <dt><code>--space <em>CLUSTER_SPACE</em></code></dt>
    <dd>O nome do espaço do Cloud Foundry para o qual você deseja enviar logs. Esse valor é válido somente para o tipo de log <code>ibm</code> e é opcional. Se você não especificar um espaço, os logs serão enviados para o nível de conta.</dd>
  <dt><code>--org <em>CLUSTER_ORG</em></code></dt>
    <dd>O nome da organização do Cloud Foundry em que o espaço está. Esse valor é válido somente para o tipo de log <code>ibm</code> e é necessário se você especificou um espaço.</dd>
  <dt><code>--app-paths</code></dt>
    <dd>O caminho no contêiner no qual os apps estão efetuando login. Para encaminhar logs com tipo de origem <code>application</code>, deve-se fornecer um caminho. Para especificar mais de um caminho, use uma lista separada por vírgula. Esse valor é necessário para origem de log <code>application</code>. Exemplo: <code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></dd>
  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>Onde você deseja encaminhar os logs. As opções são <code>ibm</code>, que encaminha os logs para o {{site.data.keyword.loganalysisshort_notm}} e <code>syslog</code>, que encaminha os logs para um servidor externo.</dd>
  <dt><code>--app-containers</code></dt>
    <dd>Opcional: para encaminhar logs por meio de apps, é possível especificar o nome do contêiner que contém o seu app. É possível especificar mais de um contêiner usando uma lista separada por vírgula. Se nenhum contêiner é especificado, os logs são encaminhados de todos os contêineres que contêm os caminhos que você forneceu. Essa opção é válida apenas para origem de log <code>application</code></dt>
  <dt><code>--json</code></dt>
    <dd>Imprima a saída de comando em formato JSON. Esse valor é opcional.</dd>
  <dt><code>--skip-validation</code></dt>
    <dd>Ignore a validação dos nomes da organização e do espaço quando são especificados. Ignorar a validação diminui o tempo de processamento, mas uma configuração de criação de log inválida não encaminhará os logs corretamente. Esse valor é opcional.</dd>
</dl>

**Exemplos**:

Exemplo para o tipo de log `ibm` que encaminha de uma origem de log `container` na porta padrão 9091:

  ```
  bx cs logging-config-create my_cluster --logsource container --namespace my_namespace --hostname ingest.logging.ng.bluemix.net --type ibm
  ```
  {: pre}

Exemplo para o tipo de log `syslog` que encaminha de uma origem de log `container` na porta padrão 514:

  ```
  bx cs logging-config-create my_cluster --logsource container --namespace my_namespace --hostname 169.xx.xxx.xxx --type syslog
  ```
  {: pre}

Exemplo para o tipo de log `syslog` que encaminha logs de uma origem `ingress` em uma porta diferente do padrão:

  ```
  bx cs logging-config-create my_cluster --logsource container --hostname 169.xx.xxx.xxx --port 5514 --type syslog
  ```
  {: pre}

### bx cs logging-config-get CLUSTER [--logsource LOG_SOURCE][--json]
{: #cs_logging_get}

Visualize todas as configurações de encaminhamento de log para um cluster ou filtre configurações de criação de log com base em origem de log.

<strong>Opções de comando</strong>:

 <dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>
  <dt><code>--logsource <em>LOG_SOURCE</em></code></dt>
    <dd>O tipo de origem de log para a qual você deseja filtrar. Apenas as configurações de criação de log dessa origem de log no cluster são retornadas. Os valores aceitos são <code>container</code>, <code>application</code>, <code>worker</code>,
<code>kubernetes</code> e <code>ingress</code>. Esse valor é opcional.</dd>
  <dt><code>--json</code></dt>
    <dd>Opcionalmente imprime a saída de comando no formato JSON.</dd>
 </dl>

**Exemplo**:

  ```
  bx cs logging-config-get my_cluster --logsource worker
  ```
  {: pre}


### bx cs logging-config-refresh CLUSTER
{: #cs_logging_refresh}

Atualize a configuração de criação de log para o cluster. Isso atualiza o token de criação de log para qualquer configuração de criação de log que está encaminhando para o nível de espaço em seu cluster.

<strong>Opções de comando</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>
</dl>

**Exemplo**:

  ```
  bx cs logging-config-refresh my_cluster
  ```
  {: pre}


### bx cs logging-config-rm CLUSTER [--id LOG_CONFIG_ID][--all]
{: #cs_logging_rm}

Excluir uma configuração de encaminhamento de log ou todas as configurações de criação de log para um cluster. Isso para o encaminhamento de log para um servidor syslog remoto ou {{site.data.keyword.loganalysisshort_notm}}.

<strong>Opções de comando</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>
  <dt><code>--id <em>LOG_CONFIG_ID</em></code></dt>
   <dd>Se você deseja remover uma configuração de criação de log única, o ID de configuração de criação de log.</dd>
  <dt><code>--all</code></dt>
   <dd>A sinalização para remover todas as configurações de criação de log em um cluster.</dd>
</dl>

**Exemplo**:

  ```
  bx cs logging-config-rm my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e
  ```
  {: pre}


### bx cs logging-config-update CLUSTER --id LOG_CONFIG_ID [--namespace NAMESPACE][--hostname LOG_SERVER_HOSTNAME_OR_IP] [--port LOG_SERVER_PORT][--space CLUSTER_SPACE] [--org CLUSTER_ORG] --type LOG_TYPE [--json][--skipValidation]
{: #cs_logging_update}

Atualize os detalhes de uma configuração de encaminhamento de log.

<strong>Opções de comando</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>
  <dt><code>--id <em>LOG_CONFIG_ID</em></code></dt>
   <dd>O ID de configuração de criação de log que você deseja atualizar. Este valor é obrigatório.</dd>
  <dt><code>--namespace <em>NAMESPACE</em></code>
    <dd>O namespace do Kubernetes do qual você deseja encaminhar logs. O encaminhamento de log não é suportado para os namespaces do Kubernetes <code>ibm-system</code> e <code>kube-system</code>. Esse valor é válido somente para a origem de log do <code>container</code>. Se você não especificar um namespace, todos os namespaces no cluster usarão essa configuração.</dd>
  <dt><code>--hostname <em>LOG_SERVER_HOSTNAME</em></code></dt>
   <dd>Quando o tipo de criação de log for <code>syslog</code>, o nome do host ou endereço IP do servidor do coletor do log. Esse valor é necessário para <code>syslog</code>. Quando o tipo de criação de log for <code>ibm</code>, a URL de ingestão {{site.data.keyword.loganalysislong_notm}}. É possível localizar a lista de URLs de ingestão disponíveis [aqui](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls). Se você não especificar uma URL de ingestão, o endpoint para a região na qual seu cluster foi criado será usado.</dd>
   <dt><code>--port <em>LOG_SERVER_PORT</em></code></dt>
   <dd>A porta do servidor coletor do log. Esse valor será opcional quando o tipo de criação de log for <code>syslog</code>. Se você não especificar uma porta, a porta padrão <code>514</code> será usada para <code>syslog</code> e <code>9091</code> será usada para <code>ibm</code>.</dd>
   <dt><code>--space <em>CLUSTER_SPACE</em></code></dt>
   <dd>O nome do espaço para o qual deseja enviar logs. Esse valor é válido somente para o tipo de log <code>ibm</code> e é opcional. Se você não especificar um espaço, os logs serão enviados para o nível de conta.</dd>
   <dt><code>--org <em>CLUSTER_ORG</em></code></dt>
   <dd>O nome da organização na qual está o espaço. Esse valor é válido somente para o tipo de log <code>ibm</code> e é necessário se você especificou um espaço.</dd>
   <dt><code>--app-paths</code></dt>
     <dd>Ignore a validação dos nomes da organização e do espaço quando são especificados. Ignorar a validação diminui o tempo de processamento, mas uma configuração de criação de log inválida não encaminhará os logs corretamente. Esse valor é opcional.</dd>
   <dt><code>--app-containers</code></dt>
     <dd>O caminho nos contêineres nos quais os apps estão registrando. Para encaminhar logs com tipo de origem <code>application</code>, deve-se fornecer um caminho. Para especificar mais de um caminho, use uma lista separada por vírgula. Exemplo: <code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></dd>
   <dt><code>--type <em>LOG_TYPE</em></code></dt>
   <dd>O protocolo de encaminhamento de log que você deseja usar. Atualmente, <code>syslog</code> e <code>ibm</code> são suportados. Este valor é obrigatório.</dd>
   <dt><code>--json</code></dt>
   <dd>Opcionalmente imprime a saída de comando no formato JSON.</dd>
   <dt><code>--skipValidation</code></dt>
   <dd>Ignore a validação dos nomes da organização e do espaço quando são especificados. Ignorar a validação diminui o tempo de processamento, mas uma configuração de criação de log inválida não encaminhará os logs corretamente. Esse valor é opcional.</dd>
   </dl>

**Exemplo para o tipo de log `ibm`**:

  ```
  bx cs logging-config-update my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e --type ibm
  ```
  {: pre}

**Exemplo para o tipo de log `syslog`**:

  ```
  bx cs logging-config-update my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e --hostname localhost --port 5514 --type syslog
  ```
  {: pre}


### bx cs logging-filter-create CLUSTER --type LOG_TYPE [--logging-configs CONFIGS][--namespace KUBERNETES_NAMESPACE] [--container CONTAINER_NAME][--level LOGGING_LEVEL] [--message MESSAGE][--s] [--json]
{: #cs_log_filter_create}

Crie um filtro de criação de log. É possível usar esse comando para filtrar logs que são encaminhados por sua configuração de criação de log.

<strong>Opções de comando</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>Necessário: o nome ou ID do cluster para o qual você deseja criar um filtro de criação de log.</dd>
  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>O tipo de logs nos quais você deseja aplicar o filtro. Atualmente <code>all</code>, <code>container</code> e <code>host</code> são suportados.</dd>
  <dt><code>--logging-configs <em>CONFIGS</em></code></dt>
    <dd>Opcional: uma lista separada por vírgula de seus IDs de configuração de criação de log. Se não fornecido, o filtro será aplicado a todas as configurações de criação de log de cluster que forem passadas para o filtro. É possível visualizar as configurações de log que correspondem ao filtro usando a sinalização <code>--show-matching-configs</code> com o comando.</dd>
  <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
    <dd>Opcional: o espaço de nomes do Kubernetes por meio do qual você deseja filtrar os logs.</dd>
  <dt><code>--container <em>CONTAINER_NAME</em></code></dt>
    <dd>Opcional: o nome do contêiner por meio do qual você deseja filtrar os logs. Essa sinalização se aplicará apenas quando você estiver usando o tipo de log <code>container</code>.</dd>
  <dt><code>--level <em>LOGGING_LEVEL</em></code></dt>
    <dd>Opcional: filtrará os logs que estiverem no nível especificado e menos. Os valores aceitáveis na ordem canônica são <code>fatal</code>, <code>error</code>, <code>warn/warning</code>, <code>info</code>, <code>debug</code> e <code>trace</code>. Como um exemplo, se você filtrou logs no nível <code>info</code>, <code>debug</code> e <code>trace</code> também serão filtrados. **Nota**: é possível usar essa sinalização apenas quando as mensagens de log estiverem em formato JSON e contiverem um campo de nível. Saída de exemplo: <code>{"log": "hello", "level": "info"}</code></dd>
  <dt><code>--message <em>MESSAGE</em></code></dt>
    <dd>Opcional: filtrará qualquer log que contenha uma mensagem especificada em qualquer lugar no log. A mensagem é correspondida literalmente e não como uma expressão. Exemplo: As mensagens “Hello”, “!”e “Hello, World!”se aplicaria ao log “Hello, World!”.</dd>
  <dt><code>--json</code></dt>
    <dd>Opcional: imprimirá a saída de comando em formato JSON.</dd>
</dl>

**Exemplos**:

Este exemplo filtra todos os logs que são encaminhados de contêineres com o nome `test-container` no namespace padrão que estão no nível de depuração ou menos e têm uma mensagem de log que contém "solicitação GET".

  ```
  bx cs logging-filter-create example-cluster --type container --namespace default --container test-container --level debug --message "GET request"
  ```
  {: pre}

Este exemplo filtra todos os logs que são encaminhados, em um nível de informações ou menos, por meio de um cluster específico. A saída é retornada como JSON.

  ```
  bx cs logging-filter-create example-cluster --type all --level info --json
  ```
  {: pre}

### bx cs logging-filter-update CLUSTER --type LOG_TYPE [--logging-configs CONFIGS][--namespace KUBERNETES_NAMESPACE] [--container CONTAINER_NAME][--level LOGGING_LEVEL] [--message MESSAGE][--s] [--json]
{: #cs_log_filter_update}

Atualize um filtro de criação de log. É possível usar esse comando para atualizar um filtro de criação de log que você criou.

<strong>Opções de comando</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>Necessário: o nome ou ID do cluster para o qual você deseja atualizar um filtro de criação de log.</dd>
  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>O tipo de logs nos quais você deseja aplicar o filtro. Atualmente <code>all</code>, <code>container</code> e <code>host</code> são suportados.</dd>
  <dt><code>--logging-configs <em>CONFIGS</em></code></dt>
    <dd>Opcional: uma lista separada por vírgula de seus IDs de configuração de criação de log. Se não fornecido, o filtro será aplicado a todas as configurações de criação de log de cluster que forem passadas para o filtro. É possível visualizar as configurações de log que correspondem ao filtro usando a sinalização <code>--show-matching-configs</code> com o comando.</dd>
  <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
    <dd>Opcional: o espaço de nomes do Kubernetes por meio do qual você deseja filtrar os logs.</dd>
  <dt><code>--container <em>CONTAINER_NAME</em></code></dt>
    <dd>Opcional: o nome do contêiner por meio do qual você deseja filtrar os logs. Essa sinalização se aplicará apenas quando você estiver usando o tipo de log <code>container</code>.</dd>
  <dt><code>--level <em>LOGGING_LEVEL</em></code></dt>
    <dd>Opcional: filtrará os logs que estiverem no nível especificado e menos. Os valores aceitáveis na ordem canônica são <code>fatal</code>, <code>error</code>, <code>warn/warning</code>, <code>info</code>, <code>debug</code> e <code>trace</code>. Como um exemplo, se você filtrou logs no nível <code>info</code>, <code>debug</code> e <code>trace</code> também serão filtrados. **Nota**: é possível usar essa sinalização apenas quando as mensagens de log estiverem em formato JSON e contiverem um campo de nível. Saída de exemplo: <code>{"log": "hello", "level": "info"}</code></dd>
  <dt><code>--message <em>MESSAGE</em></code></dt>
    <dd>Opcional: filtrará qualquer log que contenha uma mensagem especificada em qualquer lugar no log. A mensagem é correspondida literalmente e não como uma expressão. Exemplo: As mensagens “Hello”, “!”e “Hello, World!”se aplicaria ao log “Hello, World!”.</dd>
  <dt><code>--json</code></dt>
    <dd>Opcional: imprimirá a saída de comando em formato JSON.</dd>
</dl>


### bx cs logging-filter-get CLUSTER [--id FILTER_ID][--show-matching-configs] [--json]
{: #cs_log_filter_view}

Visualize uma configuração de filtro de criação de log. É possível usar esse comando para visualizar os filtros de criação de log que você criou.

<strong>Opções de comando</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>Necessário: o nome ou ID do cluster por meio do qual você deseja visualizar filtros.</dd>
  <dt><code>--id <em>FILTER_ID</em></code></dt>
    <dd>O ID do filtro de log que você deseja visualizar.</dd>
  <dt><code>--show-matching-configs</code></dt>
    <dd>Opcional: mostrará as configurações de criação de log que corresponderem à configuração que você estiver visualizando.</dd>
  <dt><code>--json</code></dt>
    <dd>Opcional: imprimirá a saída de comando em formato JSON.</dd>
</dl>


### bx cs logging-filter-rm CLUSTER [--id FILTER_ID][--json] [--all]
{: #cs_log_filter_delete}

Exclua um filtro de criação de log É possível usar esse comando para remover um filtro de criação de log que você criou.

<strong>Opções de comando</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>O nome ou ID do cluster por meio do qual você deseja excluir um filtro.</dd>
  <dt><code>--id <em>FILTER_ID</em></code></dt>
    <dd>O ID do filtro de log a ser excluído.</dd>
  <dt><code>--all</code></dt>
    <dd>Opcional: exclua todos os seus filtros de encaminhamento de log.</dd>
  <dt><code>--json</code></dt>
    <dd>Opcional: imprimirá a saída de comando em formato JSON.</dd>
</dl>

<br />


## Comandos de região
{: #region_commands}

### bx cs locations
{: #cs_datacenters}

Visualizar uma lista de locais disponíveis para você criar um cluster.

<strong>Opções de comando</strong>:

   Nenhuma

**Exemplo**:

  ```
  bx cs locations
  ```
  {: pre}


### bx cs region
{: #cs_region}

Localize a região do {{site.data.keyword.containershort_notm}} na qual você está atualmente. É possível criar e gerenciar clusters específicos para a região. Use o comando `bx cs region-set` para mudar regiões.

**Exemplo**:

```
bx cs region
```
{: pre}

**Saída**:
```
Região: us-south
```
{: screen}

### bx cs region-set [REGION]
{: #cs_region-set}

Configure a região para o {{site.data.keyword.containershort_notm}}. É possível criar e gerenciar clusters específicos para a região e você pode querer clusters em múltiplas regiões para alta disponibilidade.

Por exemplo, é possível efetuar login no {{site.data.keyword.Bluemix_notm}} na região sul dos EUA e criar um cluster. Em seguida, é possível usar `bx cs region-set eu-central` para destinar a região central da UE e criar um outro cluster. Finalmente, é possível usar o `bx cs region-set us-south` para retornar ao Sul dos EUA para gerenciar seu cluster na região.

**Opções de comando**:

<dl>
<dt><code><em>REGION</em></code></dt>
<dd>Insira a região que você deseja destinar. Esse valor é opcional. Se você não fornecer a região, será possível selecionar uma na lista na saída.

Para obter uma lista de regiões disponíveis, revise [regiões e locais](cs_regions.html) ou use o comando `bx cs regions` [](#cs_regions).</dd></dl>

**Exemplo**:

```
bx cs region-set eu-central
```
{: pre}

```
bx cs region-set
```
{: pre}

**Saída**:
```
Choose a region:
1. ap-north
2. ap-south
3. eu-central
4. uk-south
5. us-east
6. us-south
Enter a number> 3
OK
```
{: screen}

### bx cs regions
{: #cs_regions}

Lista as regiões disponíveis. O `Region Name` é o nome do {{site.data.keyword.containershort_notm}} e o `Region Alias` é o nome geral do {{site.data.keyword.Bluemix_notm}} para a região.

**Exemplo**:

```
bx cs regions
```
{: pre}

**Saída**:
```
Region Name   Region Alias
ap-north      jp-tok
ap-south      au-syd
eu-central    eu-de
uk-south      eu-gb
us-east       us-east
us-south      us-south
```
{: screen}


<br />


## Comandos de nó do trabalhador
{: worker_node_commands}



### bx cs worker-add --cluster CLUSTER [--file FILE_LOCATION][--hardware HARDWARE] --machine-type MACHINE_TYPE --number NUMBER --private-vlan PRIVATE_VLAN --public-vlan PUBLIC_VLAN [--disable-disk-encrypt]
{: #cs_worker_add}

Incluir nós do trabalhador no cluster padrão.

<strong>Opções de comando</strong>:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

<dt><code>--file <em>FILE_LOCATION</em></code></dt>
<dd>O caminho para o arquivo YAML incluir nós do trabalhador no cluster. Em vez de definir nós do trabalhador adicionais usando as opções fornecidas nesse comando, será possível usar um arquivo do YAML. Esse valor é opcional.

<p><strong>Nota:</strong> se você fornecer a mesma opção no comando que o parâmetro no arquivo do YAML, o valor no comando terá precedência sobre o valor no YAML. Por exemplo, você define um tipo de máquina em seu arquivo YAML e usa a opção --machine-type no comando, o valor inserido na opção de comando substituirá o valor no arquivo YAML.

<pre class="codeblock">
<code>name: <em>&lt;cluster_name_or_ID&gt;</em>
location: <em>&lt;location&gt;</em>
machine-type: <em>&lt;machine_type&gt;</em>
private-vlan: <em>&lt;private_VLAN&gt;</em>
public-vlan: <em>&lt;public_VLAN&gt;</em>
hardware: <em>&lt;shared_or_dedicated&gt;</em>
workerNum: <em>&lt;number_workers&gt;</em>
diskEncryption: <em>false</em></code></pre>

<table>
<caption>Tabela 2. Entendendo os componentes de arquivo YAML</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes de arquivo do YAML</th>
</thead>
<tbody>
<tr>
<td><code><em>name</em></code></td>
<td>Substitua <code><em>&lt;cluster_name_or_ID&gt;</em></code> pelo nome ou ID do cluster no qual você deseja incluir nós do trabalhador.</td>
</tr>
<tr>
<td><code><em>location</em></code></td>
<td>Substitua <code><em>&lt;location&gt;</em></code> pelo local para implementar os seus nós do trabalhador. Os locais disponíveis dependem da região a que você está conectado. Para listar os locais disponíveis, execute <code>bx cs locations</code>.</td>
</tr>
<tr>
<td><code><em>machine-type</em></code></td>
<td>Substitua <code><em>&lt;machine_type&gt;</em></code> pelo tipo de máquina na qual você deseja implementar os nós do trabalhador. É possível implementar os nós do trabalhador como máquinas virtuais em hardware compartilhado ou dedicado ou como máquinas físicas no bare metal. Os tipos de máquinas físicas e virtuais disponíveis variam pelo local no qual o cluster é implementado. Para obter mais informações, veja o comando `bx cs machine-types` [](cs_cli_reference.html#cs_machine_types).</td>
</tr>
<tr>
<td><code><em>private-vlan</em></code></td>
<td>Substitua <code><em>&lt;private_VLAN&gt;</em></code> pelo ID da VLAN privada que você deseja usar para os seus nós do trabalhador. Para listar as VLANs disponíveis, execute <code>bx cs vlans <em>&lt;location&gt;</em></code> e procure roteadores de VLAN iniciados com <code>bcr</code> (roteador de backend).</td>
</tr>
<tr>
<td><code>public-vlan</code></td>
<td>Substitua <code>&lt;public_VLAN&gt;</code> pelo ID da VLAN pública que você deseja usar para os seus nós do trabalhador. Para listar as VLANs disponíveis, execute <code>bx cs vlans &lt;location&gt;</code> e procure roteadores de VLAN iniciados com <code>fcr</code> (roteador de front-end). <br><strong>Nota</strong>: {[private_VLAN_vyatta]}</td>
</tr>
<tr>
<td><code>Hardware</code></td>
<td>Para tipos de máquinas virtuais: o nível de isolamento de hardware para seu nó do trabalhador. Use dedicado se desejar que os recursos físicos disponíveis sejam dedicados somente a você ou compartilhado para permitir que os recursos físicos sejam compartilhados com outros clientes IBM. O padrão é shared.</td>
</tr>
<tr>
<td><code>workerNum</code></td>
<td>Substitua <code><em>&lt;number_workers&gt;</em></code> pelo número de nós do trabalhador que você deseja implementar.</td>
</tr>
<tr>
<td><code>diskEncryption: <em>false</em></code></td>
<td>Nós do trabalhador apresentam criptografia de disco por padrão; [saiba
mais](cs_secure.html#worker). Para desativar a criptografia, inclua essa opção e configure o valor para <code>false</code>.</td></tr>
</tbody></table></p></dd>

<dt><code>--hardware <em>HARDWARE</em></code></dt>
<dd>O nível de isolamento de hardware para seu nó do trabalhador. Use dedicado se desejar que os recursos físicos disponíveis sejam dedicados somente a você ou compartilhado para permitir que os recursos físicos sejam compartilhados com outros clientes IBM. O padrão é shared. Esse valor é opcional.</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>Escolha um tipo de máquina. É possível implementar os nós do trabalhador como máquinas virtuais em hardware compartilhado ou dedicado ou como máquinas físicas no bare metal. Os tipos de máquinas físicas e virtuais disponíveis variam pelo local no qual o cluster é implementado. Para obter mais informações, veja a documentação para o comando `bx cs machine-types` [](cs_cli_reference.html#cs_machine_types). Esse valor é necessário para clusters padrão e não está disponível para clusters livres.</dd>

<dt><code>--number <em>NUMBER</em></code></dt>
<dd>Um número inteiro que representa o número de nós do trabalhador a serem criados no cluster. O valor padrão é 1. Esse valor é opcional.</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>A VLAN privada que foi especificada quando o cluster foi criado. Este valor é obrigatório.

<p><strong>Nota:</strong> os roteadores de VLAN privada sempre iniciam com <code>bcr</code> (roteador de backend) e os roteadores de VLAN pública sempre iniciam com <code>fcr</code> (roteador de front-end). Ao criar um cluster e especificar as VLANs públicas e privadas, o número e a combinação de letras após esses prefixos devem corresponder.</p></dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>A VLAN pública que foi especificada quando o cluster foi criado. Esse valor é opcional. Se você deseja que os nós do trabalhador existam somente em uma VLAN privada, não forneça um ID de VLAN pública. <strong>Nota</strong>: {[private_VLAN_vyatta]}

<p><strong>Nota:</strong> os roteadores de VLAN privada sempre iniciam com <code>bcr</code> (roteador de backend) e os roteadores de VLAN pública sempre iniciam com <code>fcr</code> (roteador de front-end). Ao criar um cluster e especificar as VLANs públicas e privadas, o número e a combinação de letras após esses prefixos devem corresponder.</p></dd>

<dt><code>--disable-disk-encrypt</code></dt>
<dd>Nós do trabalhador apresentam criptografia de disco por padrão; [saiba
mais](cs_secure.html#worker). Para desativar a criptografia, inclua essa opção.</dd>
</dl>

**Exemplos**:

  ```
  bx cs worker-add --cluster my_cluster --number 3 --public-vlan my_public_VLAN_ID --private-vlan my_private_VLAN_ID --machine-type u2c.2x4 --hardware shared
  ```
  {: pre}

  Exemplo para o {{site.data.keyword.Bluemix_dedicated_notm}}:

  ```
  bx cs worker-add --cluster my_cluster --number 3 --machine-type u2c.2x4
  ```
  {: pre}




### bx cs worker-get [CLUSTER_NAME_OR_ID] WORKER_NODE_ID
{: #cs_worker_get}

Visualize os detalhes de um nó do trabalhador.

<strong>Opções de comando</strong>:

   <dl>
   <dt><code><em>CLUSTER_NAME_OR_ID</em></code></dt>
   <dd>O nome ou o ID do cluster do nó do trabalhador. Esse valor é opcional.</dd>
   <dt><code><em>WORKER_NODE_ID</em></code></dt>
   <dd>O nome do seu nó do trabalhador. Execute <code>bx cs workers <em>CLUSTER</em></code> para visualizar os IDs para os nós do trabalhador em um cluster. Este valor é obrigatório.</dd>
   </dl>

**Comando de exemplo**:

  ```
  bx cs worker-get my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1
  ```
  {: pre}

**Saída de exemplo**:

  ```
  ID: kube-dal10-123456789-w1 State: normal Status: Ready Trust: disabled Private VLAN: 223xxxx Public VLAN: 223xxxx Private IP: 10.xxx.xx.xxx Public IP: 169.xx.xxx.xxx Hardware: shared Zone: dal10 Version: 1.8.11_1509
  ```
  {: screen}

### bx cs worker-reboot [-f][--hard] CLUSTER WORKER [WORKER]
{: #cs_worker_reboot}

Reinicialize um nó do trabalhador em um cluster. Durante a reinicialização, o estado do nó do trabalhador não muda.

**Atenção:** reinicializando um nó do trabalhador pode causar distorção de dados no nó do trabalhador. Use esse comando com cuidado e quando souber que uma reinicialização pode ajudar a recuperar seu nó do trabalhador. Em todos os outros casos, [recarregue seu nó do trabalhador](#cs_worker_reload).

Antes de reinicializar o nó do trabalhador, certifique-se de que os pods estão reprogramados em outros nós do trabalhador para ajudar a evitar um tempo de inatividade para seu app ou distorção de dados em seu nó do trabalhador.

1. Liste todos os nós do trabalhador em seu cluster e anote o **nome** do nó do trabalhador que você deseja reinicializar.
   ```
   kubectl get nodes
   ```
   O **nome** retornado nesse comando é o endereço IP privado designado ao nó do trabalhador. É possível localizar mais informações sobre o do trabalhador ao executar `bx cs workers <cluster_name_or_ID>` e procura o nó do trabalhador com o mesmo endereço **IP privado**.
2. Marque o nó do trabalhador como não programável em um processo conhecido como bloqueio. Ao bloquear um nó do trabalhador, ele fica indisponível para planejamento futuro do pod. Use o **nome** do nó do trabalhador recuperado na etapa anterior.
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. Verifique se o planejamento do pod está desativado para seu nó do trabalhador.
   ```
   kubectl get nodes
   ```
   {: pre}
   O nó do trabalhador ficará desativado para planejamento do pod se o status exibir **SchedulingDisabled**.
 4. Force os pods para que sejam removidos do nó do trabalhador e reprogramados nos nós do trabalhador restantes no cluster.
    ```
    kubectl drain <worker_name>
    ```
    {: pre}
    Esse processo pode levar alguns minutos.
 5. Reinicialize o nó do trabalhador. Use o ID do trabalhador retornado do comando `bx cs workers <cluster_name_or_ID>`.
    ```
    bx cs worker-reboot <cluster_name_or_ID> <worker_name_or_ID>
    ```
    {: pre}
 6. Espere cerca de 5 minutos antes de disponibilizar o seu nó do trabalhador para planejamento de pod para assegurar que a reinicialização esteja concluída. Durante a reinicialização, o estado do nó do trabalhador não muda. A reinicialização de um nó do trabalhador é geralmente concluída em alguns segundos.
 7. Disponibilize o nó do trabalhador para planejamento do pod. Use o **nome** do nó do trabalhador retornado do comando `kubectl get nodes`.
    ```
    kubectl uncordon <worker_name>
    ```
    {: pre}
    </br>

<strong>Opções de comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code>-f</code></dt>
   <dd>Use esta opção para forçar a reinicialização do nó do trabalhador sem avisos do usuário. Esse valor é opcional.</dd>

   <dt><code>--hard</code></dt>
   <dd>Use esta opção para forçar uma reinicialização forçada de um nó do trabalhador cortando a energia para o nó do trabalhador. Use essa opção se o nó do trabalhador não responder ou se ele tiver uma interrupção do
Docker. Esse valor é opcional.</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>O nome ou ID de um ou mais nós do trabalhador. Use um espaço para listar múltiplos nós do
trabalhador. Este valor é obrigatório.</dd>
   </dl>

**Exemplo**:

  ```
  bx cs worker-reboot my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}


### bx cs worker-reload [-f] CLUSTER WORKER [WORKER]
{: #cs_worker_reload}

Recarregue todas as configurações necessárias para um nó do trabalhador. Um recarregamento poderá ser útil se seu nó do trabalhador tiver problemas, como desempenho lento ou se o nó do trabalhador estiver preso em um estado inoperante.

O recarregamento de um nó do trabalhador aplica atualizações da versão de correção ao nó do trabalhador, mas não atualizações principais ou secundárias. Para ver as mudanças de uma versão de correção para a próxima, revise a documentação de [Log de mudanças da versão](cs_versions_changelog.html#changelog).
{: tip}

Antes de recarregar seu nó do trabalhador, certifique-se de que os pods estejam reprogramados em outros nós do trabalhador para ajudar a evitar um tempo de inatividade para seu app ou distorção de dados em seu nó do trabalhador.

1. Liste todos os nós do trabalhador em seu cluster e anote o **nome** do nó do trabalhador que você deseja recarregar.
   ```
   kubectl get nodes
   ```
   O **nome** retornado nesse comando é o endereço IP privado designado ao nó do trabalhador. É possível localizar mais informações sobre o do trabalhador ao executar `bx cs workers <cluster_name_or_ID>` e procura o nó do trabalhador com o mesmo endereço **IP privado**.
2. Marque o nó do trabalhador como não programável em um processo conhecido como bloqueio. Ao bloquear um nó do trabalhador, ele fica indisponível para planejamento futuro do pod. Use o **nome** do nó do trabalhador recuperado na etapa anterior.
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. Verifique se o planejamento do pod está desativado para seu nó do trabalhador.
   ```
   kubectl get nodes
   ```
   {: pre}
   O nó do trabalhador ficará desativado para planejamento do pod se o status exibir **SchedulingDisabled**.
 4. Force os pods para que sejam removidos do nó do trabalhador e reprogramados nos nós do trabalhador restantes no cluster.
    ```
    kubectl drain <worker_name>
    ```
    {: pre}
    Esse processo pode levar alguns minutos.
 5. Recarregue o nó do trabalhador. Use o ID do trabalhador retornado do comando `bx cs workers <cluster_name_or_ID>`.
    ```
    bx cs worker-reload <cluster_name_or_ID> <worker_name_or_ID>
    ```
    {: pre}
 6. Aguarde o recarregamento ser concluído.
 7. Disponibilize o nó do trabalhador para planejamento do pod. Use o **nome** do nó do trabalhador retornado do comando `kubectl get nodes`.
    ```
    kubectl uncordon <worker_name>
    ```
</br>
<strong>Opções de comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code>-f</code></dt>
   <dd>Use esta opção para forçar a recarga de um nó do trabalhador sem avisos do usuário. Esse valor é opcional.</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>O nome ou ID de um ou mais nós do trabalhador. Use um espaço para listar múltiplos nós do
trabalhador. Este valor é obrigatório.</dd>
   </dl>

**Exemplo**:

  ```
  bx cs worker-reload my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}


### bx cs worker-rm [-f] CLUSTER WORKER [WORKER]
{: #cs_worker_rm}

Remover um ou mais nós do trabalhador de um cluster. Se você remover um nó do trabalhador, o seu cluster se tornará desbalanceado. 

Antes de remover o seu nó do trabalhador, certifique-se de que os pods estejam reprogramados em outros nós do trabalhador para ajudar a evitar um tempo de inatividade para o seu app ou a distorção de dados em seu nó do trabalhador.
{: tip}

1. Liste todos os nós do trabalhador em seu cluster e anote o **nome** do nó do trabalhador que você deseja remover.
   ```
   kubectl get nodes
   ```
   O **nome** retornado nesse comando é o endereço IP privado designado ao nó do trabalhador. É possível localizar mais informações sobre o do trabalhador ao executar `bx cs workers <cluster_name_or_ID>` e procura o nó do trabalhador com o mesmo endereço **IP privado**.
2. Marque o nó do trabalhador como não programável em um processo conhecido como bloqueio. Ao bloquear um nó do trabalhador, ele fica indisponível para planejamento futuro do pod. Use o **nome** do nó do trabalhador recuperado na etapa anterior.
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. Verifique se o planejamento do pod está desativado para seu nó do trabalhador.
   ```
   kubectl get nodes
   ```
   {: pre}
   O nó do trabalhador ficará desativado para planejamento do pod se o status exibir **SchedulingDisabled**.
4. Force os pods para que sejam removidos do nó do trabalhador e reprogramados nos nós do trabalhador restantes no cluster.
   ```
   kubectl drain <worker_name>
   ```
   {: pre}
   Esse processo pode levar alguns minutos.
5. Remova o nó do trabalhador. Use o ID do trabalhador retornado do comando `bx cs workers <cluster_name_or_ID>`.
   ```
   bx cs worker-rm <cluster_name_or_ID> <worker_name_or_ID>
   ```
   {: pre}

6. Verifique se o nó do trabalhador foi removido.
   ```
   bx cs workers <cluster_name_or_ID>
   ```
</br>
<strong>Opções de comando</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>O nome ou ID do cluster. Este valor é obrigatório.</dd>

   <dt><code>-f</code></dt>
   <dd>Use esta opção para forçar a remoção de um nó do trabalhador sem avisos do usuário. Esse valor é opcional.</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>O nome ou ID de um ou mais nós do trabalhador. Use um espaço para listar múltiplos nós do
trabalhador. Este valor é obrigatório.</dd>
   </dl>

**Exemplo**:

  ```
  bx cs worker-rm my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}




### bx cs worker-update [-f] CLUSTER WORKER [WORKER][--kube-version MAJOR.MINOR.PATCH] [--force-update]
{: #cs_worker_update}

Atualize os nós do trabalhador para aplicar as atualizações e correções de segurança mais recentes no sistema operacional e para atualizar a versão do Kubernetes para corresponder à versão do nó principal. É possível atualizar a versão do Kubernetes do nó principal com o comando `bx cs cluster-update` [](cs_cli_reference.html#cs_cluster_update).

**Importante**: executar `bx cs worker-update` pode causar tempo de inatividade para os seus apps e serviços. Durante a atualização, todos os pods serão reprogramados sobre outros nós do trabalhador e os dados serão excluídos, se não forem armazenados fora do pod. Para evitar tempo de inatividade, [assegure-se de que você tenha nós do trabalhador suficientes para manipular a carga de trabalho enquanto os nós do trabalhador selecionados estão atualizando](cs_cluster_update.html#worker_node).

Pode ser necessário mudar seus arquivos YAML para implementações antes de atualizar. Revise essa [nota sobre a liberação](cs_versions.html) para obter detalhes.

<strong>Opções de comando</strong>:

   <dl>

   <dt><em>CLUSTER</em></dt>
   <dd>O nome ou ID do cluster no qual você lista nós do trabalhador disponíveis. Este valor é obrigatório.</dd>

   <dt><code>-f</code></dt>
   <dd>Use esta opção para forçar a atualização do mestre sem avisos do usuário. Esse valor é opcional.</dd>

   <dt><code>--force-update</code></dt>
   <dd>Tente a atualização mesmo se a mudança for maior que duas versões secundárias. Esse valor é opcional.</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>O ID de um ou mais nós do trabalhador. Use um espaço para listar múltiplos nós do
trabalhador. Este valor é obrigatório.</dd>
   </dl>

**Exemplo**:

  ```
  bx cs worker-update my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}



### bx cs workers CLUSTER [--show-deleted]
{: #cs_workers}

Visualizar uma lista de nós do trabalhador e o status de cada um deles em um cluster.

<strong>Opções de comando</strong>:

   <dl>
   <dt><em>CLUSTER</em></dt>
   <dd>O nome ou o ID do cluster para os nós disponíveis do trabalhador. Este valor é obrigatório.</dd>
   <dt><em>--show-deleted</em></dt>
   <dd>Visualize nós do trabalhador que foram excluídos do cluster, incluindo o motivo para a exclusão. Esse valor é opcional.</dd>
   </dl>

**Exemplo**:

  ```
  bx cs workers my_cluster
  ```
  {: pre}
