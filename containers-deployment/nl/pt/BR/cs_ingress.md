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




# Expondo apps com o Ingress
{: #ingress}

Exponha múltiplos apps em seu cluster do Kubernetes criando recursos de Ingresso que são gerenciados pelo balanceador de carga de aplicativo fornecido pela IBM no {{site.data.keyword.containerlong}}.
{:shortdesc}

## Gerenciando o tráfego de rede usando o Ingress
{: #planning}

Ingress é um serviço do Kubernetes que equilibra cargas de trabalho do tráfego de rede em seu cluster encaminhando solicitações públicas ou privadas para seus apps. É possível usar o Ingress para expor múltiplos serviços de app ao público ou a uma rede privada usando uma rota público ou privada exclusiva.
{:shortdesc}



O Ingress consiste em dois componentes:
<dl>
<dt>Balanceador de carga de aplicativo</dt>
<dd>O balanceador de carga de aplicativo (ALB) é um balanceador de carga externo que atende solicitações de HTTP, HTTPS, TCP ou UDP recebidas e encaminha as solicitações para o pod de app apropriado. Ao criar um cluster padrão, o {{site.data.keyword.containershort_notm}} cria automaticamente um ALB altamente disponível para seu cluster e designa uma rota pública exclusiva para ele. A rota pública está vinculada a um endereço IP público móvel que é provisionado em sua conta de infraestrutura do IBM Cloud (SoftLayer) durante a criação do cluster. Um ALB privado padrão também é criado automaticamente, mas não é ativado automaticamente.</dd>
<dt>Recurso do Ingress</dt>
<dd>Para expor um app usando o Ingress, deve-se criar um serviço do Kubernetes para seu app e registrar esse serviço com o ALB definindo um recurso do Ingress. O recurso do Ingress é um recurso do Kubernetes que define as regras de como rotear solicitações recebidas para um app. O recurso de Ingresso também especifica o caminho para seu serviço de app, que é anexado à rota pública para formar uma URL exclusiva do app, como `mycluster.us-south.containers.appdomain.cloud/myapp`.
<br></br><strong>Nota</strong>: desde 24 de maio de 2018, o formato de subdomínio de Ingresso mudou para os novos clusters.<ul><li>Os clusters criada após 24 de maio de 2018 são designados a um subdomínio no novo formato, <code>&lt;cluster_name&gt;.&lt;region&gt;.containers.appdomain.cloud</code>.</li><li>Os clusters criados antes de 24 de maio de 2018 continuam a usar o subdomínio designado no formato antigo, <code>&lt;cluster_name&gt;.&lt;region&gt;.containers.mybluemix.net</code>.</li></ul></dd>
</dl>

O diagrama a seguir mostra como o Ingresso direciona a comunicação da internet para um app:

<img src="images/cs_ingress.png" width="550" alt="Expor um app no {{site.data.keyword.containershort_notm}} usando o Ingresso" style="width:550px; border-style: none"/>

1. Um usuário envia uma solicitação para seu app acessando a URL do app. Essa URL é a URL pública para o seu app exposto anexada ao caminho de recurso de Ingresso, como `mycluster.us-south.containers.appdomain.cloud/myapp`.

2. Um serviço do sistema DNS que age como o balanceador de carga global resolve a URL para o endereço IP público móvel do ALB público padrão no cluster. A solicitação é roteada para o serviço ALB do Kubernetes para o app.

3. O serviço Kubernetes roteia a solicitação para o ALB.

4. O ALB verifica se uma regra de roteamento para o caminho `myapp` existe no cluster. Se uma regra de correspondência é localizada, a solicitação é encaminhada de acordo com as regras que você definiu no recurso de Ingresso para o pod no qual o app está implementado. Se múltiplas instâncias do app são implementadas no cluster, o ALB balanceia a carga de solicitações entre os pods de app.



<br />


## Pré-requisitos
{: #config_prereqs}

Antes de começar com o Ingresso, revise os pré-requisitos a seguir.
{:shortdesc}

**Pré-requisitos para todas as configurações de Ingresso:**
- O Ingresso está disponível somente para clusters padrão e requer pelo menos dois nós do trabalhador no cluster para assegurar alta disponibilidade e que atualizações periódicas sejam aplicadas.
- Configurar o Ingresso requer uma [política de acesso de Administrador](cs_users.html#access_policies). Verifique sua [política de acesso](cs_users.html#infra_access) atual.



<br />


## Planejando a rede para um único ou múltiplos namespaces
{: #multiple_namespaces}

Pelo menos um recurso de Ingresso é necessário por namespace no qual você tem aplicativos que deseja expor.
{:shortdesc}

<dl>
<dt>Todos os apps estão em um namespace</dt>
<dd>Se os apps em seu cluster estão no mesmo namespace, pelo menos um recurso de Ingresso é necessário para definir regras de roteamento para os apps expostos lá. Por exemplo, se você tem o `app1` e o `app2` expostos por serviços em um espaço de desenvolvimento, é possível criar um recurso de Ingresso no namespace. O recurso especifica `domain.net` como o host e registra os caminhos em que cada app atende com `domain.net`.
<br></br><img src="images/cs_ingress_single_ns.png" width="300" alt="Pelo menos um recurso é necessário por namespace." style="width:300px; border-style: none"/>
</dd>
<dt>Os apps estão em múltiplos namespaces</dt>
<dd>Se os apps em seu cluster estão em namespaces diferentes, deve-se criar pelo menos um recurso por namespace para definir regras para os apps expostos lá. Para registrar múltiplos recursos de Ingresso com o ALB de Ingresso do cluster, deve-se usar um domínio curinga. Quando um domínio curinga como `*.mycluster.us-south.containers.appdomain.cloud` for registrado, múltiplos subdomínios serão todos resolvidos no mesmo host. Em seguida, é possível criar um recurso de Ingresso em cada namespace e especificar um subdomínio diferente em cada recurso de Ingresso.
<br><br>
Por exemplo, considere o seguinte cenário:<ul>
<li>Você tem duas versões do mesmo app, `app1` e `app3`, para propósitos de teste.</li>
<li>Você implementa os apps em dois namespaces diferentes dentro do mesmo cluster: `app1` no namespace de desenvolvimento e `app3` no namespace temporário.</li></ul>
Para usar o mesmo ALB do cluster para gerenciar o tráfego para esses apps, você cria os serviços e recursos a seguir:<ul>
<li>Um serviço do Kubernetes no namespace de desenvolvimento para expor `app1`.</li>
<li>Um recurso de Ingresso no namespace de desenvolvimento que especifica o host como `dev.mycluster.us-south.containers.appdomain.cloud`.</li>
<li>Um serviço do Kubernetes no namespace temporário para expor `app3`.</li>
<li>Um recurso de Ingresso no namespace temporário que especifica o host como `stage.mycluster.us-south.containers.appdomain.cloud`.</li></ul></br>
<img src="images/cs_ingress_multi_ns.png" alt="Dentro de um namespace, use subdomínios em um ou múltiplos recursos" style="border-style: none"/>
Agora, ambas as URLs são resolvidas para o mesmo domínio e são, portanto, ambas atendidas pelo mesmo ALB. No entanto, como o recurso no namespace temporário é registrado com o subdomínio `stage`, o ALB de Ingresso roteia corretamente as solicitações da URL `stage.mycluster.us-south.containers.appdomain.cloud/app3` somente para `app3`.</dd>
</dl>

**Nota**:
* O IBM fornecido pelo subdomínio de Ingresso curinga, `*.<cluster_name>.<region>.containers.appdomain.cloud` é registrado por padrão para seu cluster. No entanto, o TLS não é suportado para o curinga do subdomínio do Ingress fornecido pela IBM.
* Se deseja usar um domínio customizado, deve-se registrá-lo como um domínio curinga, como `*.custom_domain.net`. Para usar o TLS, deve-se obter um certificado curinga.

### Vários domínios dentro de um namespace

Em um namespace individual, é possível usar um domínio para acessar todos os apps no namespace. Se você deseja usar domínios diferentes para os apps dentro de um namespace individual, use um domínio curinga. Quando um domínio curinga como `*.mycluster.us-south.containers.appdomain.cloud` for registrado, múltiplos subdomínios serão todos resolvidos no mesmo host. Em seguida, é possível usar um recurso para especificar múltiplos hosts de subdomínio dentro desse recurso. Como alternativa, é possível criar múltiplos recursos de Ingresso no namespace e especificar um subdomínio diferente em cada recurso de Ingresso.

<img src="images/cs_ingress_single_ns_multi_subs.png" alt="Pelo menos um recurso é necessário por namespace." style="border-style: none"/>

**Nota**:
* O IBM fornecido pelo subdomínio de Ingresso curinga, `*.<cluster_name>.<region>.containers.appdomain.cloud` é registrado por padrão para seu cluster. No entanto, o TLS não é suportado para o curinga do subdomínio do Ingress fornecido pela IBM.
* Se deseja usar um domínio customizado, deve-se registrá-lo como um domínio curinga, como `*.custom_domain.net`. Para usar o TLS, deve-se obter um certificado curinga.

<br />


## Expondo apps que estão dentro de seu cluster para o público
{: #ingress_expose_public}

Exponha apps que estão dentro de seu cluster para o público usando o ALB de Ingresso público.
{:shortdesc}

Antes de iniciar:

-   Revise o Ingresso [pré-requisitos](#config_prereqs).
-   Se você não tiver nenhum ainda, [crie um cluster padrão](cs_clusters.html#clusters_ui).
-   [Destine sua CLI](cs_cli_install.html#cs_cli_configure) para seu cluster para executar comandos `kubectl`.

### Etapa 1: Implementar apps e criar serviços de app
{: #public_inside_1}

Inicie implementando seus apps e criando serviços do Kubernetes para expô-los.
{: shortdesc}

1.  [Implemente o seu app no cluster](cs_app.html#app_cli). Assegure-se de incluir um rótulo em sua implementação na seção de metadados de seu arquivo de configuração, como `app: code`. Esse rótulo é necessário para identificar todos os pods nos quais o seu app está em execução para que os pods possam ser incluídos no balanceamento de carga do Ingress.

2.   Crie um serviço do Kubernetes para cada app que se deseja expor. O seu app deve ser exposto por um serviço do Kubernetes a ser incluído pelo ALB de cluster no balanceamento de carga do Ingress.
      1.  Abra seu editor preferencial e crie um arquivo de configuração de serviço que tenha o nome, por exemplo, `myapp_service.yaml`.
      2.  Defina um serviço para o app que o ALB exporá.

          ```
          apiVersion: v1 kind: Service metadata: name: myapp_service spec: selector: <selector_key>: <selector_value> ports:
             - protocol: TCP port: 8080
          ```
          {: codeblock}

          <table>
          <thead>
          <th colspan=2><img src="images/idea.png" alt="Ícone Ideia"/> Entendendo os componentes do arquivo YAML do serviço ALB</th>
          </thead>
          <tbody>
          <tr>
          <td><code>seletor</code></td>
          <td>Insira a chave de etiqueta (<em>&lt;selector_key&gt;</em>) e o par de valores (<em>&lt;selector_value&gt;</em>) que você deseja usar para destinar os pods nos quais o seu app é executado. Para direcionar seus pods e incluí-los no balanceamento de carga do serviço, assegure-se de que o <em>&lt;selector_key&gt;</em> e o <em>&lt;selector_value&gt;</em> sejam iguais ao par de chave/valor na seção <code>spec.template.metadata.labels</code> do yaml de sua implementação.</td>
           </tr>
           <tr>
           <td><code>port</code></td>
           <td>A porta na qual o serviço atende.</td>
           </tr>
           </tbody></table>
      3.  Salve as suas mudanças.
      4.  Crie o serviço em seu cluster. Se os apps forem implementados em múltiplos namespaces no cluster, assegure-se de que o serviço seja implementado no mesmo namespace que o app que se deseja expor.

          ```
          kubectl apply -f myapp_service.yaml [-n <namespace>]
          ```
          {: pre}
      5.  Repita essas etapas para cada app que desejar expor.


### Etapa 2: Selecionar um domínio de app e uma finalização de TLS
{: #public_inside_2}

Ao configurar o ALB público, você escolhe o domínio pelo qual seus apps são acessíveis e se usa a finalização do TLS.
{: shortdesc}

<dl>
<dt>Domínio</dt>
<dd>É possível usar o domínio fornecido pela IBM, como <code>mycluster-12345.us-south.containers.appdomain.cloud/myapp</code>, para acessar seu app na Internet. Para usar um domínio customizado como alternativa, será possível mapear seu domínio customizado para o domínio fornecido pela IBM ou para o endereço IP público do ALB.</dd>
<dt>Finalização TLS</dt>
<dd>O ALB faz o balanceamento de carga do tráfego de rede HTTP para os apps no cluster. Para também balancear a carga de conexões HTTPS recebidas, será possível configurar o ALB para decriptografar o tráfego de rede e encaminhar a solicitação decriptografada para os apps expostos no cluster. Se estiver usando o subdomínio do Ingress fornecido pela IBM, será possível usar o certificado do TLS fornecido pela IBM. O TLS não é suportado atualmente para subdomínios curinga fornecidos pela IBM. Se estiver usando um domínio customizado, será possível usar seu próprio certificado TLS para gerenciar a finalização do TLS.</dd>
</dl>

Para usar o domínio de Ingresso fornecido pela IBM:
1. Obtenha os detalhes para seu cluster. Substitua _&lt;cluster_name_or_ID&gt;_ pelo nome do cluster no qual os apps que se deseja expor estão implementados.

    ```
    bx cs cluster-get <cluster_name_or_ID>
    ```
    {: pre}

    Saída de exemplo:

    ```
    Name:                   mycluster
    ID:                     18a61a63c6a94b658596ca93d087aad9
    State:                  normal
    Created:                2018-01-12T18:33:35+0000
    Location:               dal10
    Master URL:             https://169.xx.xxx.xxx:26268
    Ingress Subdomain:      mycluster-12345.us-south.containers.appdomain.cloud
    Ingress Secret:         <tls_secret>
    Workers:                3
    Version:                1.9.7
    Owner Email:            owner@email.com
    Monitoring Dashboard:   <dashboard_URL>
    ```
    {: screen}
2. Obtenha o domínio fornecido pela IBM no campo **Subdomínio do Ingress**. Se desejar usar o TLS, obtenha também o segredo do TLS fornecido pela IBM no campo **Segredo do Ingress**.
    **Nota**: se você estiver usando um subdomínio curinga, o TLS não será suportado.

Para usar um domínio customizado:
1.    Crie um domínio customizado. Para registrar seu domínio customizado, trabalhe com o provedor Domain Name Service (DNS) ou com o [DNS do {{site.data.keyword.Bluemix_notm}}](/docs/infrastructure/dns/getting-started.html#getting-started-with-dns).
      * Se os apps que você deseja que o Ingress exponha estiverem em namespaces diferentes em um cluster, registre o domínio customizado como um domínio curinga, como `*.custom_domain.net`.

2.  Configure seu domínio para rotear o tráfego de rede recebido para o ALB fornecido pela IBM. Escolha entre estas opções:
    -   Defina um alias para seu domínio customizado especificando o domínio fornecido pela IBM como um registro de Nome Canônico (CNAME). Para localizar o domínio de Ingresso fornecido pela IBM, execute `bx cs cluster-get <cluster_name>` e procure o campo **Subdomínio do Ingresso**.
    -   Mapeie o seu domínio customizado para o endereço IP público móvel do ALB fornecido pela IBM incluindo o endereço IP como um registro. Para localizar o endereço IP público móvel do ALB, execute `bx cs alb-get <public_alb_ID>`.
3.   Opcional: se desejar usar o TLS, importe ou crie um certificado TLS e o segredo da chave. Se estiver usando um domínio curinga, assegure-se de importar ou criar um certificado curinga.
      * Se um certificado TLS é armazenado no {{site.data.keyword.cloudcerts_long_notm}} que você deseja usar, é possível importar seu segredo associado para o cluster executando o comando a seguir:
        ```
        bx cs alb-cert-deploy --secret-name <secret_name> --cluster <cluster_name_or_ID> --cert-crn <certificate_crn>
        ```
        {: pre}
      * Se você não tiver um certificado TLS pronto, siga estas etapas:
        1. Crie um certificado e chave TLS para seu domínio que é codificado no formato PEM.
        2. Crie um segredo que use seu certificado e chave do TLS. Substitua <em>&lt;tls_secret_name&gt;</em> pelo nome para o seu segredo do Kubernetes, <em>&lt;tls_key_filepath&gt;</em> pelo caminho para o seu arquivo-chave do TLS customizado e <em>&lt;tls_cert_filepath&gt;</em> pelo caminho para o seu arquivo de certificado do TLS customizado.
          ```
          kubectl create secret tls <tls_secret_name> --key <tls_key_filepath> --cert <tls_cert_filepath>
          ```
          {: pre}


### Etapa 3: Criar o recurso de Ingresso
{: #public_inside_3}

Os recursos do Ingress definem as regras de roteamento que o ALB usa para rotear tráfego para seu serviço de app.
{: shortdesc}

**Nota:** se o seu cluster tiver múltiplos namespaces nos quais os apps serão expostos, pelo menos um recurso do Ingress será necessário por namespace. No entanto, cada namespace deve usar um host diferente. Deve-se registrar um domínio curinga e especificar um subdomínio diferente em cada recurso. Para obter mais informações, veja [Planejando a rede para namespaces únicos ou múltiplos](#multiple_namespaces).

1. Abra o seu editor preferencial e crie um arquivo de configuração do Ingress que seja denominado, por exemplo, `myingressresource.yaml`.

2. Defina um recurso do Ingress em seu arquivo de configuração que use o domínio fornecido pela IBM ou seu domínio customizado para rotear o tráfego de rede recebido para os serviços criados anteriormente.

    YAML de exemplo que não usa TLS:
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingressresource
    spec:
      rules:
      - host: <domain> http: paths:
          - path: /<app1_path> backend: serviceName: <app1_service> servicePort: 80
          - path: /<app2_path> backend: serviceName: <app2_service> servicePort: 80
    ```
    {: codeblock}

    Exemplo YAML que usa TLS:
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingressresource
    spec:
      tls:
      - System z:
        - <domain> secretName: <tls_secret_name> rules:
      - host: <domain> http: paths:
          - path: /<app1_path> backend: serviceName: <app1_service> servicePort: 80
          - path: /<app2_path> backend: serviceName: <app2_service> servicePort: 80
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes de arquivo do YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>tls/hosts</code></td>
    <td>Para usar o TLS, substitua <em>&lt;domain&gt;</em> pelo subdomínio do Ingress fornecido pela IBM ou seu domínio customizado.

    </br></br>
    <strong>Nota:</strong><ul><li>Se os seus apps forem expostos por serviços em namespaces diferentes em um cluster, anexe um subdomínio curinga ao início do domínio, como `subdomain1.custom_domain.net` ou `subdomain1.mycluster.us-south.containers.appdomain.cloud`. Use um subdomínio exclusivo para cada recurso criado no cluster.</li><li>Não usar &ast; para o host ou deixar a propriedade do host vazia para evitar falhas durante a criação do Ingress.</li></ul></td>
    </tr>
    <tr>
    <td><code>tls/secretName</code></td>
    <td><ul><li>Se estiver usando o domínio do Ingress fornecido pela IBM, substitua <em>&lt;tls_secret_name&gt;</em> pelo nome do segredo do Ingress fornecido pela IBM.</li><li>Se estiver usando um domínio customizado, substitua <em>&lt;tls_secret_name&gt;</em> pelo segredo criado anteriormente que contém seu certificado e chave TLS customizados. Se você tiver importado um certificado do {{site.data.keyword.cloudcerts_short}}, será possível executar <code>bx cs alb-cert-get --cluster <cluster_name_or_ID> --cert-crn <certificate_crn></code> para ver os segredos associados a um certificado TLS.</li><ul><td>
    </tr>
    <tr>
    <td><code>host</code></td>
    <td>Substitua <em>&lt;domain&gt;</em> pelo subdomínio do Ingress fornecido pela IBM ou por seu domínio customizado.

    </br></br>
    <strong>Nota:</strong><ul><li>Se os seus apps forem expostos por serviços em namespaces diferentes em um cluster, anexe um subdomínio curinga ao início do domínio, como `subdomain1.custom_domain.net` ou `subdomain1.mycluster.us-south.containers.appdomain.cloud`. Use um subdomínio exclusivo para cada recurso criado no cluster.</li><li>Não usar &ast; para o host ou deixar a propriedade do host vazia para evitar falhas durante a criação do Ingress.</li></ul></td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td>Substitua <em>&lt;app_path&gt;</em> por uma barra ou pelo caminho em que seu app está atendendo. O caminho é anexado ao domínio fornecido pela IBM ou ao seu domínio customizado para criar uma rota exclusiva para o app. Quando você insere essa rota em um navegador da web, o tráfego de rede é roteado para o ALB. O ALB consulta o serviço associado e envia o tráfego de rede para o serviço. O serviço, então, encaminha o tráfego para os pods nos quais o app está em execução.
    </br></br>
    Muitos apps não atendem em um caminho específico, mas usam o caminho raiz e uma porta específica. Nesse caso, defina o caminho raiz como <code>/</code> e não especifique um caminho individual para seu app. Exemplos: <ul><li>Para <code>http://domain/</code>, insira <code>/</code> como o caminho.</li><li>Para <code>http://domain/app1_path</code>, insira <code>/app1_path</code> como o caminho.</li></ul>
    </br>
    <strong>Dica:</strong> para configurar o Ingress para atender em um caminho diferente do caminho no qual seu app atende, será possível usar [gravar novamente a anotação](cs_annotations.html#rewrite-path).</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td>Substitua <em>&lt;app1_service&gt;</em> e <em>&lt;app2_service&gt;</em> e assim por diante, pelo nome dos serviços criados para expor seus apps. Se os seus apps forem expostos por serviços em namespaces diferentes no cluster, inclua apenas serviços de app que estejam no mesmo namespace. Deve-se criar um recurso do Ingress para cada namespace que contenha apps que se deseja expor.</td>
    </tr>
    <tr>
    <td><code>servicePort</code></td>
    <td>A porta na qual o serviço atende. Use a mesma porta que você definiu quando criou o serviço do Kubernetes para seu app.</td>
    </tr>
    </tbody></table>

3.  Crie o recurso de Ingresso para seu cluster. Assegure-se de que o recurso seja implementado no mesmo namespace que os serviços de app especificados no recurso.

    ```
    kubectl apply -f myingressresource.yaml -n <namespace>
    ```
    {: pre}
4.   Verifique se o recurso de Ingresso foi criado com êxito.

      ```
      kubectl describe ingress myingressresource
      ```
      {: pre}

      1. Se as mensagens nos eventos descreverem um erro na configuração do recurso, mude os valores no arquivo de recursos e reaplique o arquivo para o recurso.


O recurso do Ingress é criado no mesmo namespace que os serviços de app. Seus apps nesse namespace são registrados com o ALB do Ingress do cluster.

### Etapa 4: Acessar seu app na Internet
{: #public_inside_4}

Em um navegador da web, insira a URL do serviço de app a ser acessado.

```
https://<domain>/<app1_path>
```
{: pre}

Se tiver exposto múltiplos apps, acesse-os mudando o caminho anexado à URL.

```
https://<domain>/<app2_path>
```
{: pre}

Se usar um domínio curinga para expor apps em diferentes namespaces, acesse esses apps com seus respectivos subdomínios.

```
http://<subdomain1>.<domain>/<app1_path>
```
{: pre}

```
http://<subdomain2>.<domain>/<app1_path>
```
{: pre}


<br />


## Expondo apps que estão fora de seu cluster para o público
{: #external_endpoint}

Expor apps que estão fora de seu cluster para o público, incluindo-os no balanceamento de carga do ALB de Ingresso público. As solicitações públicas recebidas no domínio customizado ou fornecido pela IBM são encaminhadas automaticamente para o app externo.
{:shortdesc}

Antes de iniciar:

-   Revise o Ingresso [pré-requisitos](#config_prereqs).
-   Se você não tiver nenhum ainda, [crie um cluster padrão](cs_clusters.html#clusters_ui).
-   [Destine sua CLI](cs_cli_install.html#cs_cli_configure) para seu cluster para executar comandos `kubectl`.
-   Assegure-se de que o app externo que você deseja incluir no balanceamento de carga do cluster possa ser acessado usando um endereço IP público.

### Etapa 1: criar um serviço de app e terminal externo
{: #public_outside_1}

Inicie criando um serviço do Kubernetes para expor o app externo e configurando um terminal externo do Kubernetes para o app.
{: shortdesc}

1.  Crie um serviço do Kubernetes para o seu cluster que encaminhará as solicitações recebidas para um terminal externo que você criará.
    1.  Abra seu editor preferencial e crie um arquivo de configuração de serviço que seja chamado, por exemplo, de `myexternalservice.yaml`.
    2.  Defina um serviço para o app que o ALB exporá.

        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: myexternalservice
        spec:
          ports:
           - protocol: TCP port: 8080
        ```
        {: codeblock}

        <table>
        <caption>Entendendo os componentes de arquivo de serviço do ALB</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes de arquivo do YAML</th>
        </thead>
        <tbody>
        <tr>
        <td><code>metadata/name</code></td>
        <td>Substitua <em>&lt;myexternalservice&gt;</em> por um nome de seu serviço.<p>Saiba mais sobre [como proteger suas informações pessoais](cs_secure.html#pi) quando trabalhar com recursos do Kubernetes.</p></td>
        </tr>
        <tr>
        <td><code>port</code></td>
        <td>A porta na qual o serviço atende.</td>
        </tr></tbody></table>

    3.  Salve as suas mudanças.
    4.  Crie o serviço do Kubernetes para seu cluster.

        ```
        kubectl apply -f myexternalservice.yaml
        ```
        {: pre}
2.  Configure um terminal do Kubernetes que defina o local externo do app que você deseja incluir no balanceamento de carga do cluster.
    1.  Abra seu editor preferencial e crie um arquivo de configuração do terminal que é chamado, por exemplo, de `myexternalendpoint.yaml`.
    2.  Defina seu terminal externo. Inclua todos os endereços IP públicos e portas que podem ser usados para acessar seu app externo.

        ```
        kind: Endpoints
        apiVersion: v1
        metadata:
          name: myexternalendpoint
        subsets:
          - addresses:
              - ip: <external_IP1>
              - ip: <external_IP2>
            ports:
              - port: <external_port>
        ```
        {: codeblock}

        <table>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes de arquivo do YAML</th>
        </thead>
        <tbody>
        <tr>
        <td><code>name</code></td>
        <td>Substitua <em>&lt;myexternalendpoint&gt;</em> pelo nome do serviço do Kubernetes que você criou anteriormente.</td>
        </tr>
        <tr>
        <td><code>IP</code></td>
        <td>Substitua <em>&lt;external_IP&gt;</em> pelos endereços IP públicos para se conectar ao app externo.</td>
         </tr>
         <td><code>port</code></td>
         <td>Substitua <em>&lt;external_port&gt;</em> pela porta em que seu app externo atende.</td>
         </tbody></table>

    3.  Salve as suas mudanças.
    4.  Crie o terminal do Kubernetes para seu cluster.

        ```
        kubectl apply -f myexternalendpoint.yaml
        ```
        {: pre}

### Etapa 2: Selecionar um domínio de app e uma finalização de TLS
{: #public_outside_2}

Ao configurar o ALB público, você escolhe o domínio pelo qual seus apps são acessíveis e se usa a finalização do TLS.
{: shortdesc}

<dl>
<dt>Domínio</dt>
<dd>É possível usar o domínio fornecido pela IBM, como <code>mycluster-12345.us-south.containers.appdomain.cloud/myapp</code>, para acessar seu app na Internet. Para usar um domínio customizado como alternativa, será possível mapear seu domínio customizado para o domínio fornecido pela IBM ou para o endereço IP público do ALB.</dd>
<dt>Finalização TLS</dt>
<dd>O ALB faz o balanceamento de carga do tráfego de rede HTTP para os apps no cluster. Para também balancear a carga de conexões HTTPS recebidas, será possível configurar o ALB para decriptografar o tráfego de rede e encaminhar a solicitação decriptografada para os apps expostos no cluster. Se estiver usando o subdomínio do Ingress fornecido pela IBM, será possível usar o certificado do TLS fornecido pela IBM. O TLS não é suportado atualmente para subdomínios curinga fornecidos pela IBM. Se estiver usando um domínio customizado, será possível usar seu próprio certificado TLS para gerenciar a finalização do TLS.</dd>
</dl>

Para usar o domínio de Ingresso fornecido pela IBM:
1. Obtenha os detalhes para seu cluster. Substitua _&lt;cluster_name_or_ID&gt;_ pelo nome do cluster no qual os apps que se deseja expor estão implementados.

    ```
    bx cs cluster-get <cluster_name_or_ID>
    ```
    {: pre}

    Saída de exemplo:

    ```
    Name:                   mycluster
    ID:                     18a61a63c6a94b658596ca93d087aad9
    State:                  normal
    Created:                2018-01-12T18:33:35+0000
    Location:               dal10
    Master URL:             https://169.xx.xxx.xxx:26268
    Ingress Subdomain:      mycluster-12345.us-south.containers.appdomain.cloud
    Ingress Secret:         <tls_secret>
    Workers:                3
    Version:                1.9.7
    Owner Email:            owner@email.com
    Monitoring Dashboard:   <dashboard_URL>
    ```
    {: screen}
2. Obtenha o domínio fornecido pela IBM no campo **Subdomínio do Ingress**. Se desejar usar o TLS, obtenha também o segredo do TLS fornecido pela IBM no campo **Segredo do Ingress**. **Nota**: se você estiver usando um subdomínio curinga, o TLS não será suportado.

Para usar um domínio customizado:
1.    Crie um domínio customizado. Para registrar seu domínio customizado, trabalhe com o provedor Domain Name Service (DNS) ou com o [DNS do {{site.data.keyword.Bluemix_notm}}](/docs/infrastructure/dns/getting-started.html#getting-started-with-dns).
      * Se os apps que você deseja que o Ingress exponha estiverem em namespaces diferentes em um cluster, registre o domínio customizado como um domínio curinga, como `*.custom_domain.net`.

2.  Configure seu domínio para rotear o tráfego de rede recebido para o ALB fornecido pela IBM. Escolha entre estas opções:
    -   Defina um alias para seu domínio customizado especificando o domínio fornecido pela IBM como um registro de Nome Canônico (CNAME). Para localizar o domínio de Ingresso fornecido pela IBM, execute `bx cs cluster-get <cluster_name>` e procure o campo **Subdomínio do Ingresso**.
    -   Mapeie o seu domínio customizado para o endereço IP público móvel do ALB fornecido pela IBM incluindo o endereço IP como um registro. Para localizar o endereço IP público móvel do ALB, execute `bx cs alb-get <public_alb_ID>`.
3.   Opcional: se desejar usar o TLS, importe ou crie um certificado TLS e o segredo da chave. Se estiver usando um domínio curinga, assegure-se de importar ou criar um certificado curinga.
      * Se um certificado TLS é armazenado no {{site.data.keyword.cloudcerts_long_notm}} que você deseja usar, é possível importar seu segredo associado para o cluster executando o comando a seguir:
        ```
        bx cs alb-cert-deploy --secret-name <secret_name> --cluster <cluster_name_or_ID> --cert-crn <certificate_crn>
        ```
        {: pre}
      * Se você não tiver um certificado TLS pronto, siga estas etapas:
        1. Crie um certificado e chave TLS para seu domínio que é codificado no formato PEM.
        2. Crie um segredo que use seu certificado e chave do TLS. Substitua <em>&lt;tls_secret_name&gt;</em> pelo nome para o seu segredo do Kubernetes, <em>&lt;tls_key_filepath&gt;</em> pelo caminho para o seu arquivo-chave do TLS customizado e <em>&lt;tls_cert_filepath&gt;</em> pelo caminho para o seu arquivo de certificado do TLS customizado.
          ```
          kubectl create secret tls <tls_secret_name> --key <tls_key_filepath> --cert <tls_cert_filepath>
          ```
          {: pre}


### Etapa 3: Criar o recurso de Ingresso
{: #public_outside_3}

Os recursos do Ingress definem as regras de roteamento que o ALB usa para rotear tráfego para seu serviço de app.
{: shortdesc}

**Nota:** se você está expondo múltiplos apps externos e os serviços criados para os apps na [Etapa 1](#public_outside_1) estão em namespaces diferentes, pelo menos um recurso de Ingresso é necessário por namespace. No entanto, cada namespace deve usar um host diferente. Deve-se registrar um domínio curinga e especificar um subdomínio diferente em cada recurso. Para obter mais informações, veja [Planejando a rede para namespaces únicos ou múltiplos](#multiple_namespaces).

1. Abra seu editor preferencial e crie um arquivo de configuração do Ingresso que seja chamado, por exemplo, de `myexternalingress.yaml`.

2. Defina um recurso do Ingress em seu arquivo de configuração que use o domínio fornecido pela IBM ou seu domínio customizado para rotear o tráfego de rede recebido para os serviços criados anteriormente.

    YAML de exemplo que não usa TLS:
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myexternalingress
    spec:
      rules:
      - host: <domain> http: paths:
          - path: /<external_app1_path> backend: serviceName: <app1_service> servicePort: 80
          - path: /<external_app2_path> backend: serviceName: <app2_service> servicePort: 80
    ```
    {: codeblock}

    Exemplo YAML que usa TLS:
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myexternalingress
    spec:
      tls:
      - System z:
        - <domain> secretName: <tls_secret_name> rules:
      - host: <domain> http: paths:
          - path: /<external_app1_path> backend: serviceName: <app1_service> servicePort: 80
          - path: /<external_app2_path> backend: serviceName: <app2_service> servicePort: 80
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes de arquivo do YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>tls/hosts</code></td>
    <td>Para usar o TLS, substitua <em>&lt;domain&gt;</em> pelo subdomínio do Ingress fornecido pela IBM ou seu domínio customizado.

    </br></br>
    <strong>Nota:</strong><ul><li>Se o seu app atende em diferentes namespaces no cluster, anexe um subdomínio curinga ao início do domínio, como `subdomain1.custom_domain.net` ou `subdomain1.mycluster.us-south.containers.appdomain.cloud`. Use um subdomínio exclusivo para cada recurso criado no cluster.</li><li>Não usar &ast; para o host ou deixar a propriedade do host vazia para evitar falhas durante a criação do Ingress.</li></ul></td>
    </tr>
    <tr>
    <td><code>tls/secretName</code></td>
    <td><ul><li>Se estiver usando o domínio do Ingress fornecido pela IBM, substitua <em>&lt;tls_secret_name&gt;</em> pelo nome do segredo do Ingress fornecido pela IBM.</li><li>Se estiver usando um domínio customizado, substitua <em>&lt;tls_secret_name&gt;</em> pelo segredo criado anteriormente que contém seu certificado e chave TLS customizados. Se você tiver importado um certificado do {{site.data.keyword.cloudcerts_short}}, será possível executar <code>bx cs alb-cert-get --cluster <cluster_name_or_ID> --cert-crn <certificate_crn></code> para ver os segredos associados a um certificado TLS.</li><ul><td>
    </tr>
    <tr>
    <td><code>rules/host</code></td>
    <td>Substitua <em>&lt;domain&gt;</em> pelo subdomínio do Ingress fornecido pela IBM ou por seu domínio customizado.

    </br></br>
    <strong>Nota:</strong><ul><li>Se os seus apps forem expostos por serviços em namespaces diferentes em um cluster, anexe um subdomínio curinga ao início do domínio, como `subdomain1.custom_domain.net` ou `subdomain1.mycluster.us-south.containers.appdomain.cloud`. Use um subdomínio exclusivo para cada recurso criado no cluster.</li><li>Não usar &ast; para o host ou deixar a propriedade do host vazia para evitar falhas durante a criação do Ingress.</li></ul></td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td>Substitua <em>&lt;external_app_path&gt;</em> por uma barra ou o caminho em que seu app está atendendo. O caminho é anexado ao domínio fornecido pela IBM ou ao seu domínio customizado para criar uma rota exclusiva para o app. Quando você insere essa rota em um navegador da web, o tráfego de rede é roteado para o ALB. O ALB consulta o serviço associado e envia o tráfego de rede para o serviço. O serviço encaminha o tráfego para o app externo. O app deve ser configurado para atender nesse caminho para receber o tráfego de rede recebido.
    </br></br>
    Muitos apps não atendem em um caminho específico, mas usam o caminho raiz e uma porta específica. Nesse caso, defina o caminho raiz como <code>/</code> e não especifique um caminho individual para seu app. Exemplos: <ul><li>Para <code>http://domain/</code>, insira <code>/</code> como o caminho.</li><li>Para <code>http://domain/app1_path</code>, insira <code>/app1_path</code> como o caminho.</li></ul>
    </br></br>
    <strong>Dica:</strong> para configurar o Ingress para atender em um caminho diferente do caminho no qual seu app atende, será possível usar [gravar novamente a anotação](cs_annotations.html#rewrite-path).</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td>Substitua <em>&lt;app1_service&gt;</em> e <em>&lt;app2_service&gt;</em>. pelo nome dos serviços criados para expor seus apps externos. Se os seus apps forem expostos por serviços em namespaces diferentes no cluster, inclua apenas serviços de app que estejam no mesmo namespace. Deve-se criar um recurso do Ingress para cada namespace que contenha apps que se deseja expor.</td>
    </tr>
    <tr>
    <td><code>servicePort</code></td>
    <td>A porta na qual o serviço atende. Use a mesma porta que você definiu quando criou o serviço do Kubernetes para seu app.</td>
    </tr>
    </tbody></table>

3.  Crie o recurso de Ingresso para seu cluster. Assegure-se de que o recurso seja implementado no mesmo namespace que os serviços de app especificados no recurso.

    ```
    kubectl apply -f myexternalingress.yaml -n <namespace>
    ```
    {: pre}
4.   Verifique se o recurso de Ingresso foi criado com êxito.

      ```
      kubectl describe ingress myingressresource
      ```
      {: pre}

      1. Se as mensagens nos eventos descreverem um erro na configuração do recurso, mude os valores no arquivo de recursos e reaplique o arquivo para o recurso.


O recurso do Ingress é criado no mesmo namespace que os serviços de app. Seus apps nesse namespace são registrados com o ALB do Ingress do cluster.

### Etapa 4: Acessar seu app na Internet
{: #public_outside_4}

Em um navegador da web, insira a URL do serviço de app a ser acessado.

```
https://<domain>/<app1_path>
```
{: pre}

Se tiver exposto múltiplos apps, acesse-os mudando o caminho anexado à URL.

```
https://<domain>/<app2_path>
```
{: pre}

Se usar um domínio curinga para expor apps em diferentes namespaces, acesse esses apps com seus respectivos subdomínios.

```
http://<subdomain1>.<domain>/<app1_path>
```
{: pre}

```
http://<subdomain2>.<domain>/<app1_path>
```
{: pre}


<br />


## Ativando o ALB privado padrão
{: #private_ingress}

Ao criar um cluster padrão, um balanceador de carga do aplicativo (ALB) privado fornecido pela IBM é criado e designado a um endereço IP privado móvel e uma rota privada. No entanto, o ALB privado padrão não é ativado automaticamente. Para usar o ALB privado para balanceamento de carga do tráfego de rede privada para seus apps, deve-se primeiro ativá-lo com o endereço IP privado móvel fornecido pela IBM ou seu próprio endereço IP privado móvel.
{:shortdesc}

**Nota**: se você usou a sinalização `--no-subnet` quando criou o cluster, deve-se incluir uma sub-rede privada móvel ou uma sub-rede gerenciada pelo usuário antes de poder ativar o ALB privado. Para obter mais informações, veja [Solicitando sub-redes adicionais para seu cluster](cs_subnets.html#request).

Antes de iniciar:

-   Se você não tiver nenhum ainda, [crie um cluster padrão](cs_clusters.html#clusters_ui).
-   [Destine sua CLI](cs_cli_install.html#cs_cli_configure) para seu cluster.

Para ativar o ALB privado usando o endereço IP privado móvel, fornecido pela IBM, pré-designado:

1. Liste os ALBs disponíveis em seu cluster para obter o ID do ALB privado. Substitua <em>&lt;cluser_name&gt;</em> pelo nome do cluster no qual o app que você deseja expor está implementado.

    ```
    bx cs albs --cluster <cluser_name>
    ```
    {: pre}

    O campo **Status** para o ALB privado está _desativado_.
    ```
    ALB ID Enabled Status Type ALB IP private-cr6d779503319d419ea3b4ab171d12c3b8-alb1 false disabled private - public-cr6d779503319d419ea3b4ab171d12c3b8-alb1 true enabled public 169.xx.xxx.xxx
    ```
    {: screen}

2. Ative o ALB privado. Substitua <em>&lt;private_ALB_ID&gt;</em> pelo ID de ALB privado da saída na etapa anterior.

   ```
   bx cs alb-configure --albID <private_ALB_ID> --enable
   ```
   {: pre}

<br>
Para ativar o ALB privado usando seu próprio endereço IP privado móvel:

1. Configure a sub-rede gerenciada pelo usuário de seu endereço IP escolhido para rotear o tráfego na VLAN privada do seu cluster.

   ```
   bx cs cluster-user-subnet-add <cluster_name> <subnet_CIDR> <private_VLAN_ID>
   ```
   {: pre}

   <table>
   <thead>
   <th colspan=2>Entendendo os componentes do comando<img src="images/idea.png" alt="Idea icon"/></th>
   </thead>
   <tbody>
   <tr>
   <td><code>&lt;cluser_name&gt;</code></td>
   <td>O nome ou ID do cluster no qual o app que você deseja expor está implementado.</td>
   </tr>
   <tr>
   <td><code>&lt;subnet_CIDR&gt;</code></td>
   <td>O CIDR de sua sub-rede gerenciada pelo usuário.</td>
   </tr>
   <tr>
   <td><code>&lt;private_VLAN_ID&gt;</code></td>
   <td>Um ID de VLAN privada disponível. É possível localizar o ID de uma VLAN privada disponível executando `bx cs vlans`.</td>
   </tr>
   </tbody></table>

2. Liste os ALBs disponíveis em seu cluster para obter o ID de ALB privado.

    ```
    bx cs albs --cluster <cluster_name>
    ```
    {: pre}

    O campo **Status** para o ALB privado está _desativado_.
    ```
    ALB ID Enabled Status Type ALB IP private-cr6d779503319d419ea3b4ab171d12c3b8-alb1 false disabled private - public-cr6d779503319d419ea3b4ab171d12c3b8-alb1 true enabled public 169.xx.xxx.xxx
    ```
    {: screen}

3. Ative o ALB privado. Substitua <em>&lt;private_ALB_ID&gt;</em> pelo ID do ALB privado da saída na etapa anterior e <em>&lt;user_IP&gt;</em> pelo endereço IP de sua sub-rede gerenciada pelo usuário que você deseja usar.

   ```
   bx cs alb-configure --albID <private_ALB_ID> --enable --user-ip <user_IP>
   ```
   {: pre}

<br />


## Expondo apps para uma rede privada
{: #ingress_expose_private}

Exponha apps para uma rede privada usando o ALB de Ingresso privado.
{:shortdesc}

Antes de iniciar:
* Revise o Ingresso [pré-requisitos](#config_prereqs).
* [Ative o balanceador de carga do aplicativo privado](#private_ingress).
* Se você tem nós do trabalhador privados e deseja usar um provedor DNS externo, deve-se [configurar os nós de borda com acesso público](cs_edge.html#edge) e [configurar um Virtual Router Appliance ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/blogs/bluemix/2017/07/kubernetes-and-bluemix-container-based-workloads-part4/).
* Se você tem nós do trabalhador privados e deseja permanecer somente em uma rede privada, deve-se [configurar um serviço DNS privado no local ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/) para resolver solicitações de URL para seus apps.

### Etapa 1: Implementar apps e criar serviços de app
{: #private_1}

Inicie implementando seus apps e criando serviços do Kubernetes para expô-los.
{: shortdesc}

1.  [Implemente o seu app no cluster](cs_app.html#app_cli). Assegure-se de incluir um rótulo em sua implementação na seção de metadados de seu arquivo de configuração, como `app: code`. Esse rótulo é necessário para identificar todos os pods nos quais o seu app está em execução para que os pods possam ser incluídos no balanceamento de carga do Ingress.

2.   Crie um serviço do Kubernetes para cada app que se deseja expor. O seu app deve ser exposto por um serviço do Kubernetes a ser incluído pelo ALB de cluster no balanceamento de carga do Ingress.
      1.  Abra seu editor preferencial e crie um arquivo de configuração de serviço que tenha o nome, por exemplo, `myapp_service.yaml`.
      2.  Defina um serviço para o app que o ALB exporá.

          ```
          apiVersion: v1 kind: Service metadata: name: myapp_service spec: selector: <selector_key>: <selector_value> ports:
             - protocol: TCP port: 8080
          ```
          {: codeblock}

          <table>
          <thead>
          <th colspan=2><img src="images/idea.png" alt="Ícone Ideia"/> Entendendo os componentes do arquivo YAML do serviço ALB</th>
          </thead>
          <tbody>
          <tr>
          <td><code>seletor</code></td>
          <td>Insira a chave de etiqueta (<em>&lt;selector_key&gt;</em>) e o par de valores (<em>&lt;selector_value&gt;</em>) que você deseja usar para destinar os pods nos quais o seu app é executado. Para direcionar seus pods e incluí-los no balanceamento de carga do serviço, assegure-se de que o <em>&lt;selector_key&gt;</em> e o <em>&lt;selector_value&gt;</em> sejam iguais ao par de chave/valor na seção <code>spec.template.metadata.labels</code> do yaml de sua implementação.</td>
           </tr>
           <tr>
           <td><code>port</code></td>
           <td>A porta na qual o serviço atende.</td>
           </tr>
           </tbody></table>
      3.  Salve as suas mudanças.
      4.  Crie o serviço em seu cluster. Se os apps forem implementados em múltiplos namespaces no cluster, assegure-se de que o serviço seja implementado no mesmo namespace que o app que se deseja expor.

          ```
          kubectl apply -f myapp_service.yaml [-n <namespace>]
          ```
          {: pre}
      5.  Repita essas etapas para cada app que desejar expor.


### Etapa 2: mapear seu domínio customizado e selecionar a finalização do TLS
{: #private_2}

Ao configurar o ALB privado, você usa um domínio customizado por meio do qual seus apps serão acessíveis e escolhe se deseja usar a finalização do TLS.
{: shortdesc}

O ALB faz o balanceamento de carga do tráfego de rede HTTP para seus apps. Para também fazer o balanceamento de carga de conexões HTTPS recebidas, é possível configurar o ALB para usar seu próprio certificado TLS para decriptografar o tráfego de rede. O ALB então encaminha a solicitação decriptografada para os apps expostos em seu cluster.
1.   Crie um domínio customizado. Para registrar seu domínio customizado, trabalhe com o provedor Domain Name Service (DNS) ou com o [DNS do {{site.data.keyword.Bluemix_notm}}](/docs/infrastructure/dns/getting-started.html#getting-started-with-dns).
      * Se os apps que você deseja que o Ingress exponha estiverem em namespaces diferentes em um cluster, registre o domínio customizado como um domínio curinga, como `*.custom_domain.net`.

2. Mapeie o seu domínio customizado para o endereço IP privado móvel do ALB privado fornecido pela IBM, incluindo o endereço IP como um registro. Para localizar o endereço IP privado móvel do ALB privado, execute `bx cs albs --cluster <cluster_name>`.
3.   Opcional: se desejar usar o TLS, importe ou crie um certificado TLS e o segredo da chave. Se estiver usando um domínio curinga, assegure-se de importar ou criar um certificado curinga.
      * Se um certificado TLS é armazenado no {{site.data.keyword.cloudcerts_long_notm}} que você deseja usar, é possível importar seu segredo associado para o cluster executando o comando a seguir:
        ```
        bx cs alb-cert-deploy --secret-name <secret_name> --cluster <cluster_name_or_ID> --cert-crn <certificate_crn>
        ```
        {: pre}
      * Se você não tiver um certificado TLS pronto, siga estas etapas:
        1. Crie um certificado e chave TLS para seu domínio que é codificado no formato PEM.
        2. Crie um segredo que use seu certificado e chave do TLS. Substitua <em>&lt;tls_secret_name&gt;</em> pelo nome para o seu segredo do Kubernetes, <em>&lt;tls_key_filepath&gt;</em> pelo caminho para o seu arquivo-chave do TLS customizado e <em>&lt;tls_cert_filepath&gt;</em> pelo caminho para o seu arquivo de certificado do TLS customizado.
          ```
          kubectl create secret tls <tls_secret_name> --key <tls_key_filepath> --cert <tls_cert_filepath>
          ```
          {: pre}


### Etapa 3: Criar o recurso de Ingresso
{: #pivate_3}

Os recursos do Ingress definem as regras de roteamento que o ALB usa para rotear tráfego para seu serviço de app.
{: shortdesc}

**Nota:** se o seu cluster tiver múltiplos namespaces nos quais os apps serão expostos, pelo menos um recurso do Ingress será necessário por namespace. No entanto, cada namespace deve usar um host diferente. Deve-se registrar um domínio curinga e especificar um subdomínio diferente em cada recurso. Para obter mais informações, veja [Planejando a rede para namespaces únicos ou múltiplos](#multiple_namespaces).

1. Abra o seu editor preferencial e crie um arquivo de configuração do Ingress que seja denominado, por exemplo, `myingressresource.yaml`.

2.  Defina um recurso de Ingresso em seu arquivo de configuração que use o seu domínio customizado para rotear o tráfego de rede recebido para os serviços que você criou anteriormente.

    YAML de exemplo que não usa TLS:
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingressresource
      annotations:
        ingress.bluemix.net/ALB-ID: "<private_ALB_ID>"
    spec:
      rules:
      - host: <domain> http: paths:
          - path: /<app1_path> backend: serviceName: <app1_service> servicePort: 80
          - path: /<app2_path> backend: serviceName: <app2_service> servicePort: 80
    ```
    {: codeblock}

    Exemplo YAML que usa TLS:
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingressresource
      annotations:
        ingress.bluemix.net/ALB-ID: "<private_ALB_ID>"
    spec:
      tls:
      - System z:
        - <domain> secretName: <tls_secret_name> rules:
      - host: <domain> http: paths:
          - path: /<app1_path> backend: serviceName: <app1_service> servicePort: 80
          - path: /<app2_path> backend: serviceName: <app2_service> servicePort: 80
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes de arquivo do YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>ingress.bluemix.net/ALB-ID</code></td>
    <td>Substitua <em>&lt;private_ALB_ID&gt;</em> pelo ID de seu ALB privado. Execute <code>bx cs albs --cluster <my_cluster></code> para localizar o ID do ALB. Para obter mais informações sobre essa anotação de Ingresso, veja [Roteamento do balanceador de carga de aplicativo privado](cs_annotations.html#alb-id).</td>
    </tr>
    <tr>
    <td><code>tls/hosts</code></td>
    <td>Para usar TLS, substitua <em>&lt;domain&gt;</em> com seu domínio customizado.

    </br></br>
    <strong>Nota:</strong><ul><li>Se os seus apps forem expostos por serviços em namespaces diferentes em um cluster, anexe um subdomínio curinga ao início do domínio, como `subdomain1.custom_domain.net`. Use um subdomínio exclusivo para cada recurso criado no cluster.</li><li>Não usar &ast; para o host ou deixar a propriedade do host vazia para evitar falhas durante a criação do Ingress.</li></ul></td>
    </tr>
    <tr>
    <td><code>tls/secretName</code></td>
    <td>Substitua <em>&lt;tls_secret_name&gt;</em> pelo nome do segredo que você criou anteriormente e que contém o seu certificado e chave TLS customizados. Se você tiver importado um certificado do {{site.data.keyword.cloudcerts_short}}, será possível executar <code>bx cs alb-cert-get --cluster <cluster_name_or_ID> --cert-crn <certificate_crn></code> para ver os segredos associados a um certificado TLS.
    </tr>
    <tr>
    <td><code>host</code></td>
    <td>Substitua <em>&lt;domain&gt;</em> com seu domínio customizado.
    </br></br>
    <strong>Nota:</strong><ul><li>Se os seus apps forem expostos por serviços em namespaces diferentes em um cluster, anexe um subdomínio curinga ao início do domínio, como `subdomain1.custom_domain.net`. Use um subdomínio exclusivo para cada recurso criado no cluster.</li><li>Não usar &ast; para o host ou deixar a propriedade do host vazia para evitar falhas durante a criação do Ingress.</li></ul></td>
    </td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td>Substitua <em>&lt;app_path&gt;</em> por uma barra ou pelo caminho em que seu app está atendendo. O caminho é anexado ao seu domínio customizado para criar uma rota exclusiva para seu app. Quando você insere essa rota em um navegador da web, o tráfego de rede é roteado para o ALB. O ALB consulta o serviço associado e envia o tráfego de rede para o serviço. O serviço, então, encaminha o tráfego para os pods nos quais o app está em execução.
    </br></br>
    Muitos apps não atendem em um caminho específico, mas usam o caminho raiz e uma porta específica. Nesse caso, defina o caminho raiz como <code>/</code> e não especifique um caminho individual para seu app. Exemplos: <ul><li>Para <code>http://domain/</code>, insira <code>/</code> como o caminho.</li><li>Para <code>http://domain/app1_path</code>, insira <code>/app1_path</code> como o caminho.</li></ul>
    </br>
    <strong>Dica:</strong> para configurar o Ingress para atender em um caminho diferente do caminho no qual seu app atende, será possível usar [gravar novamente a anotação](cs_annotations.html#rewrite-path).</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td>Substitua <em>&lt;app1_service&gt;</em> e <em>&lt;app2_service&gt;</em> e assim por diante, pelo nome dos serviços criados para expor seus apps. Se os seus apps forem expostos por serviços em namespaces diferentes no cluster, inclua apenas serviços de app que estejam no mesmo namespace. Deve-se criar um recurso do Ingress para cada namespace que contenha apps que se deseja expor.</td>
    </tr>
    <tr>
    <td><code>servicePort</code></td>
    <td>A porta na qual o serviço atende. Use a mesma porta que você definiu quando criou o serviço do Kubernetes para seu app.</td>
    </tr>
    </tbody></table>

3.  Crie o recurso de Ingresso para seu cluster. Assegure-se de que o recurso seja implementado no mesmo namespace que os serviços de app especificados no recurso.

    ```
    kubectl apply -f myingressresource.yaml -n <namespace>
    ```
    {: pre}
4.   Verifique se o recurso de Ingresso foi criado com êxito.

      ```
      kubectl describe ingress myingressresource
      ```
      {: pre}

      1. Se as mensagens nos eventos descreverem um erro na configuração do recurso, mude os valores no arquivo de recursos e reaplique o arquivo para o recurso.


O recurso do Ingress é criado no mesmo namespace que os serviços de app. Seus apps nesse namespace são registrados com o ALB do Ingress do cluster.

### Etapa 4: acessar seu app de sua rede privada
{: #private_4}

De dentro de seu firewall de rede privada, insira a URL do serviço de app em um navegador da web.

```
https://<domain>/<app1_path>
```
{: pre}

Se tiver exposto múltiplos apps, acesse-os mudando o caminho anexado à URL.

```
https://<domain>/<app2_path>
```
{: pre}

Se usar um domínio curinga para expor apps em diferentes namespaces, acesse esses apps com seus respectivos subdomínios.

```
http://<subdomain1>.<domain>/<app1_path>
```
{: pre}

```
http://<subdomain2>.<domain>/<app1_path>
```
{: pre}


Para obter um tutorial abrangente sobre como assegurar a comunicação de microsserviço-para-microsserviço em seus clusters usando o ALB privado com TLS, veja [esta postagem do blog ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://medium.com/ibm-cloud/secure-microservice-to-microservice-communication-across-kubernetes-clusters-using-a-private-ecbe2a8d4fe2).

<br />


## Configurações opcionais do balanceador de carga de aplicativo
{: #configure_alb}

É possível configurar ainda mais um balanceador de carga de aplicativo com as opções a seguir.

-   [Abrindo portas no balanceador de carga de aplicativo de Ingresso](#opening_ingress_ports)
-   [Configurar protocolos SSL e cifras SSL no nível de HTTP](#ssl_protocols_ciphers)
-   [Customizando o formato de log do Ingress](#ingress_log_format)
-   [Customizando o seu balanceador de carga de aplicativo com anotações](cs_annotations.html)
{: #ingress_annotation}


### Abrindo portas no balanceador de carga de aplicativo de Ingresso
{: #opening_ingress_ports}

Por padrão, somente as portas 80 e 443 são expostas no ALB de Ingresso. Para expor outras portas, é possível editar o recurso configmpa `ibm-cloud-provider-ingress-cm`.
{:shortdesc}

1. Crie e abra uma versão local do arquivo de configuração para o recurso configmap `ibm-cloud-provider-ingress-cm`.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Inclua uma seção <code>dados</code> e especifique as portas públicas `80`, `443` e quaisquer outras portas que você deseja expor separados por um ponto e vírgula (;).

    **Importante**: por padrão, as portas 80 e 443 estão abertas. Se você deseja manter a 80 e a 443 abertas, deve-se também incluí-las além de quaisquer outras portas especificadas no campo `public-ports`. Qualquer porta que não esteja especificada é encerrada. Se você ativou um ALB privado, deve-se também especificar quaisquer portas que você deseja manter abertas no campo `private-ports`.

    ```
    apiVersion: v1
    data:
      public-ports: "80;443;<port3>"
      private-ports: "80;443;<port4>"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

    Exemplo que mantém as portas `80`, `443` e `9443` abertas:
    ```
    apiVersion: v1
 data:
   public-ports: "80;443;9443"
 kind: ConfigMap
 metadata:
   name: ibm-cloud-provider-ingress-cm
   namespace: kube-system
    ```
    {: screen}

3. Salve o arquivo de configuração.

4. Verifique se as mudanças de configmap foram aplicadas.

 ```
 kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
 ```
 {: pre}

 Saída:
 ```
 Name:        ibm-cloud-provider-ingress-cm
 Namespace:    kube-system
 Labels:        <none>
 Annotations:    <none>

 Dados ====

  Public-ports: "80; 443; 9443"
 ```
 {: screen}

Para obter mais informações sobre os recursos configmap, veja a [documentação do Kubernetes](https://kubernetes-v1-4.github.io/docs/user-guide/configmap/).

### Configurando protocolos SSL e cifras SSL no nível de HTTP
{: #ssl_protocols_ciphers}

Ative os protocolos e cifras SSL no nível HTTP global editando o configmap `ibm-cloud-provider-ingress-cm`.
{:shortdesc}

Por padrão, o protocolo TLS 1.2 é usado para todas as configurações de Ingress que usam o domínio fornecido pela IBM. É possível substituir o padrão para usar os protocolos TLS 1.1 ou 1.0 seguindo estas etapas.

**Nota**: quando você especifica os protocolos ativados para todos os hosts, os parâmetros TLSv1.1 e TLSv1.2 (1.1.13, 1.0.12) funcionam somente quando o OpenSSL 1.0.1 ou superior é usado. O parâmetro TLSv1.3 (1.13.0) funciona somente quando o OpenSSL 1.1.1 construído com o suporte TLSv1.3 é usado.

Para editar o configmap para ativar protocolos e cifras SSL:

1. Crie e abra uma versão local do arquivo de configuração para o recurso configmap `ibm-cloud-provider-ingress-cm`.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Inclua os protocolos e cifras SSL. Formate as cifras de acordo com o [Formato de lista de cifras da biblioteca OpenSSL ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.openssl.org/docs/man1.0.2/apps/ciphers.html).

   ```
   apiVersion: v1
 data:
   ssl-protocols: "TLSv1 TLSv1.1 TLSv1.2"
   ssl-ciphers: "HIGH:!aNULL:!MD5"
 kind: ConfigMap
 metadata:
   name: ibm-cloud-provider-ingress-cm
   namespace: kube-system
   ```
   {: codeblock}

3. Salve o arquivo de configuração.

4. Verifique se as mudanças de configmap foram aplicadas.

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}

   Saída:
   ```
   Name:        ibm-cloud-provider-ingress-cm
 Namespace:    kube-system
 Labels:        <none>
 Annotations:    <none>

   Dados ====

    ssl-protocols: "TLSv1 TLSv1.1 TLSv1.2"
  ssl-ciphers: "HIGH:!aNULL:!MD5"
   ```
   {: screen}

### Customizando o conteúdo e o formato de log do Ingress
{: #ingress_log_format}

É possível customizar o conteúdo e o formato de logs que são coletados para o ALB do Ingress.
{:shortdesc}

Por padrão, os logs do Ingress são formatados em JSON e exibem campos de log comuns. No entanto, também é possível criar um formato de log customizado. Para escolher quais componentes de log são encaminhados e como os componentes são organizados na saída de log:

1. Crie e abra uma versão local do arquivo de configuração para o recurso configmap `ibm-cloud-provider-ingress-cm`.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Inclua um <code>dados</code> seção. Inclua o campo `log-format` e, opcionalmente, o campo `log-format-escape-json`.

    ```
    apiVersion: v1
    data:
      log-format: '{<key1>: <log_variable1>, <key2>: <log_variable2>, <key3>: <log_variable3>}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: pre}

    <table>
    <caption>Componentes de arquivo YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ícone ideia"/> Entendendo a configuração de formato de log</th>
    </thead>
    <tbody>
    <tr>
    <td><code>log-format</code></td>
    <td>Substitua <code>&lt;key&gt;</code> pelo nome do componente de log e <code>&lt;log_variable&gt;</code> por uma variável para o componente de log que você deseja coletar em entradas de log. É possível incluir texto e pontuação que você deseja que a entrada de log contenha, como aspas em torno de valores de sequência e vírgulas para separar os componentes de log. Por exemplo, formatar um componente como <code>request: "$request",</code> gera o seguinte em uma entrada de log: <code>request: "GET / HTTP/1.1",</code>. Para obter uma lista de todas as variáveis que você pode usar, veja o <a href="http://nginx.org/en/docs/varindex.html">Índice de variável Nginx</a>.<br><br>Para registrar um cabeçalho adicional como <em>x-custom-ID</em>, inclua o par chave-valor a seguir no conteúdo de log customizado: <br><pre class="pre"><code>customID: $http_x_custom_id</code></pre> <br>Os hifens (<code>-</code>) são convertidos em sublinhados (<code>_</code>) e <code>$http_</code> deve ser pré-anexado ao nome do cabeçalho customizado.</td>
    </tr>
    <tr>
    <td><code>log-format-escape-json</code></td>
    <td>Opcional: por padrão, os logs são gerados em formato de texto. Para gerar logs no formato JSON, inclua o campo <code>log-format-escape-json</code> e use o valor <code>true</code>.</td>
    </tr>
    </tbody></table>
    </dd>
    </dl>

    Por exemplo, seu formato de log pode conter as variáveis a seguir:
    ```
    apiVersion: v1
    data:
      log-format: '{remote_address: $remote_addr, remote_user: "$remote_user",
                    time_date: [$time_local], request: "$request",
                    status: $status, http_referer: "$http_referer",
                    http_user_agent: "$http_user_agent",
                    request_id: $request_id}'
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: screen}

    Uma entrada de log de acordo com esse formato é semelhante ao exemplo a seguir:
    ```
    remote_address: 127.0.0.1, remote_user: "dbmanager", time_date: [30/Mar/2018:18:52:17 +0000], request: "GET / HTTP/1.1", status: 401, http_referer: "-", http_user_agent: "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:47.0) Gecko/20100101 Firefox/47.0", request_id: a02b2dea9cf06344a25611c1d7ad72db
    ```
    {: screen}

    Para criar um formato de log customizado que é baseado no formato padrão para logs do ALB, modifique a seção a seguir conforme necessário e inclua-a em seu configmap:
    ```
    apiVersion: v1
    data:
      log-format: '{"time_date": "$time_iso8601", "client": "$remote_addr",
                    "host": "$http_host", "scheme": "$scheme",
                    "request_method": "$request_method", "request_uri": "$uri",
                    "request_id": "$request_id", "status": $status,
                    "upstream_addr": "$upstream_addr", "upstream_status":
                    $upstream_status, "request_time": $request_time,
                    "upstream_response_time": $upstream_response_time,
                    "upstream_connect_time": $upstream_connect_time,
                    "upstream_header_time": $upstream_header_time}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

4. Salve o arquivo de configuração.

5. Verifique se as mudanças de configmap foram aplicadas.

 ```
 kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
 ```
 {: pre}

 Saída de exemplo:
 ```
 Name:        ibm-cloud-provider-ingress-cm
 Namespace:    kube-system
 Labels:        <none>
 Annotations:    <none>

 Dados ====

  log-format: '{remote_address: $remote_addr, remote_user: "$remote_user", time_date: [$time_local], request: "$request", status: $status, http_referer: "$http_referer", http_user_agent: "$http_user_agent", request_id: $request_id}'
  log-format-escape-json: "true"
 ```
 {: screen}

4. Para visualizar os logs de ALB do Ingress, [crie uma configuração de criação de log para o serviço Ingress](cs_health.html#logging) em seu cluster.

<br />



