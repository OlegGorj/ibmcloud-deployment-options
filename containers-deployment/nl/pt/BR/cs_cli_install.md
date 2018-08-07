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





# Configurando a CLI e API
{: #cs_cli_install}

É possível usar a CLI ou API do {{site.data.keyword.containerlong}} para criar e gerenciar seus clusters do Kubernetes.
{:shortdesc}

<br />


## Instalando a CLI
{: #cs_cli_install_steps}

Instale as CLIs necessárias para criar e gerenciar seus clusters do Kubernetes no {{site.data.keyword.containershort_notm}} e implementar apps conteinerizados em seu cluster.
{:shortdesc}

Esta tarefa inclui as informações para instalar essas CLIs e plug-ins:

-   CLI do {{site.data.keyword.Bluemix_notm}} versão 0.5.0 ou mais recente
-   Plug-in do {{site.data.keyword.containershort_notm}}
-   A versão da CLI do Kubernetes que corresponde à versão `major.minor` de seu cluster
-   Opcional: plug-in do {{site.data.keyword.registryshort_notm}}
-   Opcional: Docker versão 1.9 ou mais recente

<br>
Para instalar as CLIs:

1.  Como um pré-requisito para o plug-in do {{site.data.keyword.containershort_notm}}, instale a CLI do [{{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://clis.ng.bluemix.net/ui/home.html). O prefixo para executar comandos usando a CLI do {{site.data.keyword.Bluemix_notm}} é `bx`.

2.  Efetue login na CLI do {{site.data.keyword.Bluemix_notm}}. Insira suas credenciais do {{site.data.keyword.Bluemix_notm}} quando solicitadas.

    ```
    bx login
    ```
    {: pre}

    **Nota:** se você tiver um ID federado, use `bx login --sso` para efetuar login na CLI do {{site.data.keyword.Bluemix_notm}}. Insira seu nome do usuário e use a URL fornecida na saída da CLI para recuperar sua senha descartável. Você sabe que tem um ID federado quando o login falha sem a opção `--sso` e é bem-sucedido com a opção `--sso`.

3.  Para criar clusters do Kubernetes e gerenciar nós do trabalhador, instale o plug-in do {{site.data.keyword.containershort_notm}}. O prefixo para executar comandos usando o plug-in do {{site.data.keyword.containershort_notm}} é `bx cs`.

    ```
    bx plugin install container-service -r Bluemix
    ```
    {: pre}

    Para verificar se o plug-in está instalado adequadamente, execute o comando a seguir:

    ```
    bx plugin list
    ```
    {: pre}

    O plug-in do {{site.data.keyword.containershort_notm}} é exibido nos resultados como um serviço de contêiner.

4.  {: #kubectl}Para visualizar uma versão local do painel do Kubernetes e implementar apps em seus clusters, [instale a CLI do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/tasks/tools/install-kubectl/). O prefixo para executar comandos usando o Kubernetes CLI é `kubectl`.

    1.  Faça download da versão `major.minor` da CLI do Kubernetes que corresponda à versão `major.minor` do cluster do Kubernetes que você planeja usar. A versão atual do Kubernetes padrão do {{site.data.keyword.containershort_notm}} é 1.9.7. **Nota**: se você usar uma versão da CLI `kubectl` que não corresponda pelo menos à versão `major.minor` de seus clusters, poderá ter resultados inesperados. Certifique-se de manter as versões de cluster e de CLI do Kubernetes atualizadas.

        - **OS X**:   [https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/darwin/amd64/kubectl ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/darwin/amd64/kubectl)
        - **Linux**:   [https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/linux/amd64/kubectl ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/linux/amd64/kubectl)
        - **Windows**:    [https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/windows/amd64/kubectl.exe ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/windows/amd64/kubectl.exe)

    2.  **Para OSX e Linux**: conclua as etapas a seguir.
        1.  Mova o arquivo executável para o diretório `/usr/local/bin`.

            ```
            Mv /filepath/kubectl /usr/local/bin/kubectl
            ```
            {: pre}

        2.  Certifique-se de que `/usr/local/bin` esteja listado na variável do sistema `PATH`. A variável `PATH` contém todos os diretórios nos quais o sistema operacional pode localizar arquivos executáveis. Os diretórios que estão listados na variável `PATH` servem propósitos diferentes. `/usr/local/bin` é usado para armazenar arquivos executáveis para o software que não faz parte do sistema operacional e que foi instalado manualmente pelo administrador do sistema.

            ```
            echo $PATH
            ```
            {: pre}

            Exemplo de saída da CLI:

            ```
            /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
            ```
            {: screen}

        3.  Torne o arquivo executável.

            ```
            chmod +x /usr/local/bin/kubectl
            ```
            {: pre}

    3.  **Para Windows**: instale a CLI do Kubernetes no mesmo diretório que a CLI do {{site.data.keyword.Bluemix_notm}}. Essa configuração economiza algumas
mudanças de caminho de arquivo ao executar comandos posteriormente.

5.  Para gerenciar um repositório de imagem privada, instale o plug-in do {{site.data.keyword.registryshort_notm}}. Use esse plug-in para configurar o seu próprio namespace em um registro de imagem privada de múltiplos locatários, altamente disponível e escalável que é hospedado pela IBM e para armazenar e compartilhar imagens do Docker com outros usuários. As imagens do Docker são necessárias para implementar contêineres em um cluster. O prefixo para executar comandos de registro é `bx cr`.

    ```
    bx plugin install container-registry -r Bluemix
    ```
    {: pre}

    Para verificar se o plug-in está instalado adequadamente, execute o comando a seguir:

    ```
    bx plugin list
    ```
    {: pre}

    O plug-in é exibido nos resultados como registro do contêiner.

6.  Para construir imagens localmente e enviá-las por push para o seu namespace de registro, [instale o Docker ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.docker.com/community-edition#/download). Se você estiver usando o Windows 8 ou anterior, será possível instalar o [Docker Toolbox ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.docker.com/toolbox/toolbox_install_windows/) como alternativa. A CLI do Docker é usada para construir apps em imagens. O prefixo para executar comandos usando o Docker CLI é `docker`.

Em seguida, inicie [Criando clusters do Kubernetes por meio da CLI com o {{site.data.keyword.containershort_notm}}](cs_clusters.html#clusters_cli).

Para obter informações de referência sobre essas CLIs, veja a documentação para essas ferramentas.

-   [Comandos `bx`](/docs/cli/reference/bluemix_cli/bx_cli.html)
-   [Comandos `bx cs`](cs_cli_reference.html#cs_cli_reference)
-   [Comandos `kubectl` ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/reference/kubectl/overview/)
-   [Comandos `bx cr`](/docs/cli/plugins/registry/index.html)

<br />




## Executando a CLI em um contêiner em seu computador
{: #cs_cli_container}

Em vez de instalar cada uma das CLIs individualmente em seu computador, é possível instalar as CLIs em um contêiner que é executado em seu computador.
{:shortdesc}

1. Crie uma imagem do Dockerfile fornecido.

    ```
    Docker build -t < image_name> https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/install-clis-container/Dockerfile
    ```
    {: pre}

2. Implemente a imagem localmente como um contêiner e monte um volume para acessar arquivos locais.
  
    ```
    docker run -it -v /local/path:/container/volume <image_name>
    ```
    {: pre}
  
3. Inicie executando os comandos `bx cs` e `kubectl` no shell interativo. Se você criar dados que deseja salvar, salve esses dados no volume montado. Quando você sair do shell, o contêiner será parado. 

<br />



## Configurando a CLI para executar `kubectl`
{: #cs_cli_configure}

É possível usar os comandos que são fornecidos com a CLI do Kubernetes para gerenciar clusters no {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

Todos os comandos `kubectl` que estão disponíveis no Kubernetes 1.9.7 são suportados para uso com clusters no {{site.data.keyword.Bluemix_notm}}. Após criar um cluster, configure o contexto para sua CLI local para esse cluster com uma variável de ambiente. Então, é possível executar os comandos `kubectl` do Kubernetes para trabalhar com o seu cluster no {{site.data.keyword.Bluemix_notm}}.

Antes de poder executar comandos `kubectl`, [instale as CLIs necessárias](#cs_cli_install) e [crie um cluster](cs_clusters.html#clusters_cli).

1.  Efetue login na CLI do {{site.data.keyword.Bluemix_notm}}. Insira suas credenciais do {{site.data.keyword.Bluemix_notm}} quando solicitadas. Para especificar uma região do {{site.data.keyword.Bluemix_notm}}, [inclua o terminal de API](cs_regions.html#bluemix_regions).

    ```
    bx login
    ```
    {: pre}

    **Nota:** se você tiver um ID federado, use `bx login --sso` para efetuar login na CLI do {{site.data.keyword.Bluemix_notm}}. Insira seu nome do usuário e use a URL fornecida na saída da CLI para recuperar sua senha descartável. Você sabe que tem um ID federado quando o login falha sem a opção `--sso` e é bem-sucedido com a opção `--sso`.

2.  Selecione uma conta do {{site.data.keyword.Bluemix_notm}}. Se você estiver designado para múltiplas organizações do {{site.data.keyword.Bluemix_notm}}, selecione a organização na qual o cluster foi criado. Os clusters são específicos para uma organização, mas são independentes de um espaço do {{site.data.keyword.Bluemix_notm}}. Portanto, não é necessário selecionar um espaço.

3.  Para criar ou acessar clusters do Kubernetes em uma região diferente da região do {{site.data.keyword.Bluemix_notm}} que você selecionou anteriormente, execute `bx cs region-set`.

4.  Liste todos os clusters na conta para obter o nome do cluster.

    ```
    bx cs clusters
    ```
    {: pre}

5.  Configure o cluster criado como o contexto para esta sessão. Conclua estas etapas de configuração toda vez que você trabalhar com o seu cluster.
    1.  Obtenha o comando para configurar a variável de ambiente e fazer download dos arquivos de configuração do Kubernetes.

        ```
        bx cs cluster-config <cluster_name_or_ID>
        ```
        {: pre}

        Depois de fazer download dos arquivos de configuração, será exibido um comando que poderá ser usado para configurar o caminho para o arquivo de configuração local do Kubernetes como uma variável de ambiente.

        Exemplo:

        ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

    2.  Copie e cole o comando que é exibido em seu terminal para configurar a variável de ambiente `KUBECONFIG`.

    3.  Verifique se a variável de ambiente `KUBECONFIG` está configurada corretamente.

        Exemplo:

        ```
        echo $KUBECONFIG
        ```
        {: pre}

        Saída:
        ```
        /Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

6.  Verifique se os comandos `kubectl` são executados adequadamente com seu cluster verificando a versão do servidor de CLI do Kubernetes.

    ```
    kubectl version  --short
    ```
    {: pre}

    Saída de exemplo:

    ```
    Versão do cliente: v1.9.7 Versão do servidor: v1.9.7
    ```
    {: screen}

Agora, é possível executar comandos `kubectl` para gerenciar seus clusters no {{site.data.keyword.Bluemix_notm}}. Para obter uma lista completa de comandos, veja a [documentação do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/reference/kubectl/overview/).

**Dica:** se estiver usando o Windows e a CLI do Kubernetes não estiver instalada no mesmo diretório que a CLI do {{site.data.keyword.Bluemix_notm}}, você deverá mudar os diretórios para o caminho no qual a CLI do Kubernetes está instalada para executar comandos `kubectl` com êxito.


<br />


## Atualizando a CLI
{: #cs_cli_upgrade}

Talvez você queira atualizar as CLIs periodicamente para usar novos recursos.
{:shortdesc}

Esta tarefa inclui as informações para atualizar essas CLIs.

-   CLI do {{site.data.keyword.Bluemix_notm}} versão 0.5.0 ou mais recente
-   Plug-in do {{site.data.keyword.containershort_notm}}
-   Kubernetes CLI versão version 1.9.7 ou posterior
-   Plug-in do {{site.data.keyword.registryshort_notm}}
-   Docker versão 1.9. ou posterior

<br>
Para atualizar as CLIs:

1.  Atualize a CLI do {{site.data.keyword.Bluemix_notm}}. Faça download da [versão mais recente ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://clis.ng.bluemix.net/ui/home.html) e execute o instalador.

2. Efetue login na CLI do {{site.data.keyword.Bluemix_notm}}. Insira suas credenciais do {{site.data.keyword.Bluemix_notm}} quando solicitadas. Para especificar uma região do {{site.data.keyword.Bluemix_notm}}, [inclua o terminal de API](cs_regions.html#bluemix_regions).

    ```
    bx login
    ```
    {: pre}

     **Nota:** se você tiver um ID federado, use `bx login --sso` para efetuar login na CLI do {{site.data.keyword.Bluemix_notm}}. Insira seu nome do usuário e use a URL fornecida na saída da CLI para recuperar sua senha descartável. Você sabe que tem um ID federado quando o login falha sem a opção `--sso` e é bem-sucedido com a opção `--sso`.

3.  Atualize o plug-in {{site.data.keyword.containershort_notm}}.
    1.  Instale a atualização do repositório do plug-in do {{site.data.keyword.Bluemix_notm}}.

        ```
        bx plugin update container-service -r Bluemix
        ```
        {: pre}

    2.  Verifique a instalação do plug-in executando o comando a seguir e verificando a lista dos plug-ins instalados.

        ```
        bx plugin list
        ```
        {: pre}

        O plug-in do {{site.data.keyword.containershort_notm}} é exibido nos resultados como um serviço de contêiner.

    3.  Inicialize a CLI.

        ```
        bx cs init
        ```
        {: pre}

4.  [Atualize a CLI do Kubernetes](#kubectl).

5.  Atualize o plug-in {{site.data.keyword.registryshort_notm}}.
    1.  Instale a atualização do repositório do plug-in do {{site.data.keyword.Bluemix_notm}}.

        ```
        bx plugin update container-registry -r Bluemix
        ```
        {: pre}

    2.  Verifique a instalação do plug-in executando o comando a seguir e verificando a lista dos plug-ins instalados.

        ```
        bx plugin list
        ```
        {: pre}

        O plug-in de registro é exibido nos resultados como registro do contêiner.

6.  Atualize o Docker.
    -   Se você estiver usando o Docker Community Edition, inicie o Docker, clique no ícone **Docker** e clique em **Verificar atualizações**.
    -   Se você estiver usando o Docker Toolbox, faça download da [versão mais recente ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.docker.com/toolbox/toolbox_install_windows/) e execute o instalador.

<br />


## Desinstalando a CLI
{: #cs_cli_uninstall}

Se você não precisar mais da CLI, será possível desinstalá-la.
{:shortdesc}

Esta tarefa inclui as informações para remover estas CLIs:


-   Plug-in do {{site.data.keyword.containershort_notm}}
-   Kubernetes CLI
-   Plug-in do {{site.data.keyword.registryshort_notm}}
-   Docker versão 1.9. ou posterior

<br>
Para desinstalar as CLIs:

1.  Desinstale o plug-in do {{site.data.keyword.containershort_notm}}.

    ```
    bx plugin uninstall container-service
    ```
    {: pre}

2.  Desinstale o plug-in do {{site.data.keyword.registryshort_notm}}.

    ```
    bx plugin uninstall container-registry
    ```
    {: pre}

3.  Verifique se os plug-ins foram desinstalados executando o comando a seguir e verificando a lista dos plug-ins instalados.

    ```
    bx plugin list
    ```
    {: pre}

    Os plug-ins container-service e container-registry não são exibidos nos resultados.

6.  Desinstale o Docker. As instruções para desinstalar o Docker variam com base no sistema operacional usado.

    - [OSX ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.docker.com/docker-for-mac/#uninstall-or-reset)
    - [Linux ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/#uninstall-docker-ce)
    - [Windows ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.docker.com/toolbox/toolbox_install_windows/#how-to-uninstall-toolbox)

<br />


## Automatizando implementações de cluster com a API
{: #cs_api}

É possível usar a API do {{site.data.keyword.containershort_notm}} para automatizar a criação, a implementação e o gerenciamento de seus clusters do Kubernetes.
{:shortdesc}

A API do {{site.data.keyword.containershort_notm}} requer informações do cabeçalho que devem ser fornecidas na solicitação de API e que podem variar, dependendo da API que você deseja usar. Para determinar quais informações do cabeçalho são necessárias para sua API, veja a [{{site.data.keyword.containershort_notm}}documentação da API ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://us-south.containers.bluemix.net/swagger-api).

**Nota:** para autenticar com o {{site.data.keyword.containershort_notm}}, deve-se fornecer um token Identity and Access Management (IAM) que é gerado com suas credenciais do {{site.data.keyword.Bluemix_notm}} e que inclui o ID da conta do {{site.data.keyword.Bluemix_notm}} na qual o cluster foi criado. Dependendo da maneira que se autenticar com o {{site.data.keyword.Bluemix_notm}}, será possível escolher entre as opções a seguir para automatizar a criação de seu token IAM.

<table>
<caption>Tipos de ID e opções</caption>
<thead>
<th>ID do {{site.data.keyword.Bluemix_notm}}</th>
<th>Minhas opções</th>
</thead>
<tbody>
<tr>
<td>ID não federado</td>
<td><ul><li><strong>Nome do usuário e senha do {{site.data.keyword.Bluemix_notm}}:</strong> é possível seguir as etapas neste tópico para automatizar totalmente a criação de seu token de acesso IAM.</li>
<li><strong>Gerar uma chave API do {{site.data.keyword.Bluemix_notm}}:</strong> como uma alternativa ao uso de nome do usuário e senha do {{site.data.keyword.Bluemix_notm}}, é possível <a href="../iam/apikeys.html#manapikey" target="_blank">usar chaves API do {{site.data.keyword.Bluemix_notm}}</a>. As chaves API do {{site.data.keyword.Bluemix_notm}} são dependentes da conta do {{site.data.keyword.Bluemix_notm}} para a qual elas são geradas. Não é possível combinar a sua chave API do {{site.data.keyword.Bluemix_notm}} com um ID da conta diferente no mesmo token do IAM. Para acessar os clusters que foram criados com uma conta diferente daquela na qual a sua chave API do {{site.data.keyword.Bluemix_notm}} é baseada; deve-se efetuar login na conta para gerar uma nova chave API. </li></ul></tr>
<tr>
<td>ID federado</td>
<td><ul><li><strong>Gerar uma chave API do {{site.data.keyword.Bluemix_notm}}:</strong> <a href="../iam/apikeys.html#manapikey" target="_blank">as chaves API do {{site.data.keyword.Bluemix_notm}}</a> são dependentes da conta do {{site.data.keyword.Bluemix_notm}} para a qual elas são geradas. Não é possível combinar a sua chave API do {{site.data.keyword.Bluemix_notm}} com um ID da conta diferente no mesmo token do IAM. Para acessar os clusters que foram criados com uma conta diferente daquela na qual a sua chave API do {{site.data.keyword.Bluemix_notm}} é baseada; deve-se efetuar login na conta para gerar uma nova chave API. </li><li><strong>Usar uma senha única: </strong> se você autenticar com o {{site.data.keyword.Bluemix_notm}} usando uma senha única, não será possível automatizar totalmente a criação de seu token IAM porque a recuperação de sua senha única requer uma interação manual com seu navegador da web. Para automatizar totalmente a criação de seu token do IAM, deve-se criar uma chave API do {{site.data.keyword.Bluemix_notm}}. </ul></td>
</tr>
</tbody>
</table>

1.  Crie seu token de acesso do IAM (Identity and Access Management). As informações do corpo incluídas em sua solicitação variam com base no método de autenticação do {{site.data.keyword.Bluemix_notm}} usado. Substitua os seguintes valores:
  - _&lt;username&gt;_: seu nome de usuário do {{site.data.keyword.Bluemix_notm}}.
  - _&lt;password&gt;_: sua senha do {{site.data.keyword.Bluemix_notm}}.
  - _&lt;api_key&gt;_: sua chave API do {{site.data.keyword.Bluemix_notm}}.
  - _&lt;passcode&gt;_: O {{site.data.keyword.Bluemix_notm}} senha descartável. Execute `bx login --sso` e siga as instruções em sua saída da CLI para recuperar sua senha única usando seu navegador da web.

    ```
    POST https://iam.<region>.bluemix.net/oidc/token
    ```
    {: codeblock}

    Exemplo:
    ```
    POST https://iam.ng.bluemix.net/oidc/token
    ```
    {: codeblock}

    Para especificar uma região {{site.data.keyword.Bluemix_notm}}, [revise as
abreviações de região conforme elas são usadas nos endpoints de API](cs_regions.html#bluemix_regions).

    <table summary-"Input parameters to retrieve tokens">
    <caption>Os parâmetros de entrada para obter tokens</caption>
    <thead>
        <th>Parâmetros de Entrada</th>
        <th>Values</th>
    </thead>
    <tbody>
    <tr>
    <td>Cabeçalho (Header)</td>
    <td><ul><li>Content-Type:application/x-www-form-urlencoded</li> <li>Authorization: Basic Yng6Yng=<p><strong>Nota</strong>: <code>Yng6Yng=</code> é igual à autorização codificada por URL para o nome do usuário <strong>bx</strong> e a senha <strong>bx</strong>.</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>Corpo para nome do usuário e senha do {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>grant_type: password</li>
    <li>response_type: cloud_iam uaa</li>
    <li>username: <em>&lt;username&gt;</em></li>
    <li>password: <em>&lt;password&gt;</em></li>
    <li>uaa_client_ID: cf</li>
    <li>uaa_client_secret:</li></ul>
    <strong>Nota</strong>: inclua a chave <code>uaa_client_secret</code> sem valor especificado.</td>
    </tr>
    <tr>
    <td>Corpo para chaves API do {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>grant_type: urn:ibm:params:oauth:grant-type:apikey</li>
    <li>response_type: cloud_iam uaa</li>
    <li>apikey: <em>&lt;api_key&gt;</em></li>
    <li>uaa_client_ID: cf</li>
    <li>uaa_client_secret:</li></ul>
    <strong>Nota</strong>: inclua a chave <code>uaa_client_secret</code> sem valor especificado.</td>
    </tr>
    <tr>
    <td>Corpo para senha única do {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>grant_type: urn:ibm:params:oauth:grant-type:passcode</li>
    <li>response_type: cloud_iam uaa</li>
    <li>passcode: <em>&lt;passcode&gt;</em></li>
    <li>uaa_client_ID: cf</li>
    <li>uaa_client_secret:</li></ul>
    <strong>Nota</strong>: inclua a chave <code>uaa_client_secret</code> sem valor especificado.</td>
    </tr>
    </tbody>
    </table>

    Exemplo de saída da API:

    ```
    {
    "access_token": "<iam_token>",
    "refresh_token": "<iam_refresh_token>",
    "uaa_token": "<uaa_token>",
    "uaa_refresh_token": "<uaa_refresh_token>",
    "token_type": "Bearer",
    "expires_in": 3600,
    "expiration": 1493747503
    }

    ```
    {: screen}

    É possível localizar o token IAM no campo **access_token** de sua saída da API. Observe o token IAM para recuperar informações adicionais do cabeçalho nas próximas etapas.

2.  Recupere o ID da conta do {{site.data.keyword.Bluemix_notm}} em que o cluster foi criado. Substitua _&lt;iam_token&gt;_ pelo token IAM recuperado na etapa anterior.

    ```
    GET https://accountmanagement.<region>.bluemix.net/v1/accounts
    ```
    {: codeblock}

    <table summary="Parâmetros de entrada para obter o ID da conta {{site.data.keyword.Bluemix_notm}}">
    <caption>Parâmetros de entrada para obter um ID da conta do {{site.data.keyword.Bluemix_notm}}</caption>
    <thead>
  	<th>Parâmetros de entrada</th>
  	<th>Values</th>
    </thead>
    <tbody>
  	<tr>
  		<td>Cabeçalhos</td>
  		<td><ul><li>Content-Type: application/json</li>
      <li>Authorization: bearer &lt;iam_token&gt;</li>
      <li>Accept: application/json</li></ul></td>
  	</tr>
    </tbody>
    </table>

    Exemplo de saída da API:

    ```
    {
      "total_results": 3,
      "total_pages": 1,
      "prev_url": null,
      "next_url": null,
      "resources":
        {
          "metadata": {
            "guid": "<account_ID>",
            "url": "/v1/accounts/<account_ID>",
            "created_at": "2016-01-07T18:55:09.726Z",
            "updated_at": "2017-04-28T23:46:03.739Z",
            "origin": "BSS"
    ...
    ```
    {: screen}

    É possível localizar o ID de sua conta do {{site.data.keyword.Bluemix_notm}} no campo **resources/metadata/guid** de sua saída da API.

3.  Gere um novo token IAM que inclua as credenciais do {{site.data.keyword.Bluemix_notm}} e o ID da conta no qual o cluster foi criado. Substitua _&lt;account_ID&gt;_ pelo ID da conta do {{site.data.keyword.Bluemix_notm}} recuperada na etapa anterior.

    **Nota:** se você está usando uma chave API do {{site.data.keyword.Bluemix_notm}}, deve-se usar o ID da conta do {{site.data.keyword.Bluemix_notm}} para a qual a chave API foi criada. Para acessar clusters em outras contas, efetue login nessa conta e crie uma chave API do {{site.data.keyword.Bluemix_notm}} que seja baseada nessa conta.

    ```
    POST https://iam.<region>.bluemix.net/oidc/token
    ```
    {: codeblock}

    Exemplo:
    ```
    POST https://iam.ng.bluemix.net/oidc/token
    ```
    {: codeblock}

    Para especificar uma região {{site.data.keyword.Bluemix_notm}}, [revise as
abreviações de região conforme elas são usadas nos endpoints de API](cs_regions.html#bluemix_regions).

    <table summary-"Input parameters to retrieve tokens">
    <caption>Os parâmetros de entrada para obter tokens</caption>
    <thead>
        <th>Parâmetros de Entrada</th>
        <th>Values</th>
    </thead>
    <tbody>
    <tr>
    <td>Cabeçalho (Header)</td>
    <td><ul><li>Content-Type:application/x-www-form-urlencoded</li> <li>Authorization: Basic Yng6Yng=<p><strong>Nota</strong>: <code>Yng6Yng=</code> é igual à autorização codificada por URL para o nome do usuário <strong>bx</strong> e a senha <strong>bx</strong>.</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>Corpo para nome do usuário e senha do {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>grant_type: password</li>
    <li>response_type: cloud_iam uaa</li>
    <li>username: <em>&lt;username&gt;</em></li>
    <li>password: <em>&lt;password&gt;</em></li>
    <li>uaa_client_ID: cf</li>
    <li>uaa_client_secret:</li>
    <li>bss_account: <em>&lt;account_ID&gt;</em></li></ul>
    <strong>Nota</strong>: inclua a chave <code>uaa_client_secret</code> sem valor especificado.</td>
    </tr>
    <tr>
    <td>Corpo para chaves API do {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>grant_type: urn:ibm:params:oauth:grant-type:apikey</li>
    <li>response_type: cloud_iam uaa</li>
    <li>apikey: <em>&lt;api_key&gt;</em></li>
    <li>uaa_client_ID: cf</li>
    <li>uaa_client_secret:</li>
    <li>bss_account: <em>&lt;account_ID&gt;</em></li></ul>
      <strong>Nota</strong>: inclua a chave <code>uaa_client_secret</code> sem valor especificado.</td>
    </tr>
    <tr>
    <td>Corpo para senha única do {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>grant_type: urn:ibm:params:oauth:grant-type:passcode</li>
    <li>response_type: cloud_iam uaa</li>
    <li>passcode: <em>&lt;passcode&gt;</em></li>
    <li>uaa_client_ID: cf</li>
    <li>uaa_client_secret:</li>
    <li>bss_account: <em>&lt;account_ID&gt;</em></li></ul><strong>Nota</strong>: inclua a chave <code>uaa_client_secret</code> sem valor especificado.</td>
    </tr>
    </tbody>
    </table>

    Exemplo de saída da API:

    ```
    {
      "access_token": "<iam_token>",
    "refresh_token": "<iam_refresh_token>",
    "uaa_token": "<uaa_token>",
    "uaa_refresh_token": "<uaa_refresh_token>",
    "token_type": "Bearer",
    "expires_in": 3600,
    "expiration": 1493747503
    }

    ```
    {: screen}

    É possível localizar o token do IAM no **access_token** e o token de atualização do IAM no **refresh_token**.

4.  Liste todos os clusters do Kubernetes em sua conta. Use as informações que você recuperou nas etapas anteriores para construir as informações do cabeçalho.

     ```
     GET https://containers.bluemix.net/v1/clusters
     ```
     {: codeblock}

     <table summary="Parâmetros de entrada para trabalhar com a API">
     <caption>Parâmetros de entrada para trabalhar com a API</caption>
     <thead>
     <th>Parâmetros de entrada</th>
     <th>Values</th>
     </thead>
     <tbody>
     <tr>
     <td>Cabeçalho (Header)</td>
     <td><ul><li>Authorization: bearer <em>&lt;iam_token&gt;</em></li>
     <li>X-Auth-Refresh-Token: <em>&lt;refresh_token&gt;</em></li></ul></td>
     </tr>
     </tbody>
     </table>

5.  Revise a [{{site.data.keyword.containershort_notm}} Documentação da API ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://containers.bluemix.net/swagger-api) para localizar uma lista de APIs suportadas.

<br />


## Atualizando tokens de acesso IAM e obtendo novos tokens de atualização
{: #cs_api_refresh}

Cada token de acesso IAM (Identity and Access Management) emitido por meio da API expira depois de uma hora. Deve-se atualizar seu token de acesso regularmente para assegurar acesso à
API do {{site.data.keyword.Bluemix_notm}}. É possível usar as mesmas etapas para obter um novo token de atualização.
{:shortdesc}

Antes de iniciar, certifique-se de que tenha um token de atualização IAM ou uma chave API do {{site.data.keyword.Bluemix_notm}} que possa ser usada para solicitar um novo token de acesso.
- **Token de atualização:** siga as instruções em [Automatizando a criação do cluster e o processo de gerenciamento com a {{site.data.keyword.Bluemix_notm}}API do ](#cs_api).
- **Chave API:** Recupere sua chave API do [{{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/) conforme a seguir.
   1. Na barra de menus, clique em **Gerenciar** > **Segurança** > **Chaves API da plataforma**.
   2. Clique em
**Criar**.
   3. Insira um **Nome** e uma **Descrição** para sua chave API e clique em **Criar**.
   4. Clique em **Mostrar** para ver a chave API que foi gerado para você.
   5. Copie a chave API para que possa ser usada para recuperar seu novo token de acesso IAM.

Use as etapas a seguir se desejar criar um token IAM ou se desejar obter um novo token de atualização.

1.  Gere um novo token de acesso IAM usando o token de atualização ou a chave API do {{site.data.keyword.Bluemix_notm}}.
    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="Parâmetros de entrada para o novo token IAM">
    <caption>Parâmetros de entrada para um novo token do IAM</caption>
    <thead>
    <th>Parâmetros de entrada</th>
    <th>Values</th>
    </thead>
    <tbody>
    <tr>
    <td>Cabeçalho (Header)</td>
    <td><ul><li>Content-Type: application/x-www-form-urlencoded</li>
      <li>Authorization: Basic Yng6Yng=</br></br><strong>Nota:</strong> <code>Yng6Yng=</code> é igual à autorização codificada por URL para o nome do usuário <strong>bx</strong> e a senha <strong>bx</strong>.</li></ul></td>
    </tr>
    <tr>
    <td>Corpo ao usar o token de atualização</td>
    <td><ul><li>grant_type: refresh_token</li>
    <li>response_type: cloud_iam uaa</li>
    <li>refresh_token: <em>&lt;iam_refresh_token&gt;</em></li>
    <li>uaa_client_ID: cf</li>
    <li>uaa_client_secret:</li>
    <li>bss_account: <em>&lt;account_ID&gt;</em></li></ul><strong>Nota</strong>: inclua a chave <code>uaa_client_secret</code> sem valor especificado.</td>
    </tr>
    <tr>
      <td>Corpo ao usar a chave API do {{site.data.keyword.Bluemix_notm}}</td>
      <td><ul><li>grant_type: <code>urn:ibm:params:oauth:grant-type:apikey</code></li>
    <li>response_type: cloud_iam uaa</li>
    <li>apikey: <em>&lt;api_key&gt;</em></li>
    <li>uaa_client_ID: cf</li>
        <li>uaa_client_secret:</li></ul><strong>Nota:</strong> inclua a chave <code>uaa_client_secret</code> sem valor especificado.</td>
    </tr>
    </tbody>
    </table>

    Exemplo de saída da API:

    ```
    {
      "access_token": "<iam_token>",
    "refresh_token": "<iam_refresh_token>",
    "uaa_token": "<uaa_token>",
    "uaa_refresh_token": "<uaa_refresh_token>",
    "token_type": "Bearer",
    "expires_in": 3600,
    "expiration": 1493747503
    }

    ```
    {: screen}

    É possível localizar seu novo token IAM no **access_token** e o token de atualização IAM no campo **refresh_token** da saída de API.

2.  Continue trabalhando com a [{{site.data.keyword.containershort_notm}}documentação da API ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://us-south.containers.bluemix.net/swagger-api) usando o token da etapa anterior.

