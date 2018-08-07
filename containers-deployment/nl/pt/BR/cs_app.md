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




# Implementando apps em clusters
{: #app}

É possível usar técnicas do Kubernetes no {{site.data.keyword.containerlong}} para implementar apps em contêineres e assegurar que os apps estejam funcionando sempre. Por exemplo, é possível executar atualizações e recuperações contínuas sem tempo de inatividade para seus usuários.
{: shortdesc}

Aprenda as etapas gerais para implementar apps clicando em uma área da imagem a seguir.

<img usemap="#d62e18" border="0" class="image" id="basic_deployment_process" src="images/basic_deployment_process.png" width="780" style="width:780px;" alt="Processo de implementação básica"/>
<map name="d62e18" id="d62e18">
<area href="cs_cli_install.html" target="_blank" alt="Instalar as CLIs." title="Instalar as CLIs." shape="rect" coords="30, 69, 179, 209" />
<area href="https://kubernetes.io/docs/concepts/configuration/overview/" target="_blank" alt="Crie um arquivo de configuração para o seu app. Revise as melhores práticas do Kubernetes." title="Crie um arquivo de configuração para o seu app. Revise as melhores práticas do Kubernetes." shape="rect" coords="254, 64, 486, 231" />
<area href="#app_cli" target="_blank" alt="Opção 1: execute os arquivos de configuração da CLI do Kubernetes." title="Opção 1: execute os arquivos de configuração da CLI do Kubernetes." shape="rect" coords="544, 67, 730, 124" />
<area href="#cli_dashboard" target="_blank" alt="Opção 2: inicie o painel do Kubernetes localmente e execute os arquivos de configuração." title="Opção 2: inicie o painel do Kubernetes localmente e execute os arquivos de configuração." shape="rect" coords="544, 141, 728, 204" />
</map>


<br />


## Planejando implementações altamente disponíveis
{: #highly_available_apps}

Quanto mais amplamente você distribui a configuração entre múltiplos nós do trabalhador e clusters,
menos provável que os usuários tenham que experimentar tempo de inatividade com seu app.
{: shortdesc}

Revise as potenciais configurações de app a seguir que são ordenadas com graus crescentes de disponibilidade.

![Estágios de alta disponibilidade para um app](images/cs_app_ha_roadmap.png)

1.  Uma implementação com n + 2 pods que são gerenciados por um conjunto de réplicas.
2.  Uma implementação com n + 2 pods que são gerenciados por um conjunto de réplicas e difundidos em múltiplos nós
(antiafinidade) no mesmo local.
3.  Uma implementação com n+2 pods que são gerenciados por um conjunto de réplicas e difundidos em
múltiplos nós (antiafinidade) em diferentes locais.
4.  Uma implementação com n + 2 pods que são gerenciados por um conjunto de réplicas e difundidos em
múltiplos nós (antiafinidade) em diferentes regiões.




### Aumentando a disponibilidade de seu app
{: #increase_availability}

<dl>
  <dt>Usar implementações e conjuntos de réplicas para implementar seu app e suas dependências</dt>
    <dd><p>Uma implementação é um recurso do Kubernetes que pode ser usado para declarar todos os componentes de seu app e suas dependências. Com as implementações, não é necessário anotar todas as etapas e, em vez disso, é possível se concentrar no app.</p>
    <p>Ao implementar mais de um pod, um conjunto de réplicas é criado automaticamente para as suas
implementações e monitora os pods e assegura que o número desejado de pods esteja ativo e em execução
sempre. Quando um pod fica inativo, o conjunto de réplicas substitui o pod não responsivo por um novo.</p>
    <p>É possível usar uma implementação para definir estratégias de atualização para seu app incluindo o número de
módulos que você deseja incluir durante uma atualização contínua e o número de pods que podem estar indisponíveis
por vez. Ao executar uma atualização contínua, a implementação verifica se a revisão está ou não
funcionando e para o lançamento quando falhas são detectadas.</p>
    <p>Com as implementações, é possível implementar simultaneamente múltiplas revisões com diferentes sinalizações. Por exemplo, é possível testar uma implementação primeiro antes de decidir enviá-la por push para a produção.</p>
    <p>As implementações permitem manter o controle de qualquer revisão implementada. Será possível usar esse histórico para recuperar uma versão anterior se você descobrir que as suas atualizações não estão funcionando conforme o esperado.</p></dd>
  <dt>Incluir réplicas suficientes para a carga de trabalho de seu app, mais duas</dt>
    <dd>Para tornar seu app ainda mais altamente disponível e mais resiliente à falha, considere a inclusão
de réplicas extras, além do mínimo, para manipular a carga de trabalho esperada. As réplicas extras podem manipular a
carga de trabalho no caso de um pod travar e o conjunto de réplicas ainda não tiver recuperado o pod travado. Para
proteção contra duas falhas simultâneas, inclua duas réplicas extras. Essa configuração é um padrão N + 2, em que N é o número de réplicas para manipular a carga de trabalho recebida e + 2 são duas réplicas extras. Desde que seu cluster tenha espaço suficiente, será possível ter tantos pods quantos você quiser.</dd>
  <dt>Difundir pods em múltiplos nós (antiafinidade)</dt>
    <dd><p>Quando você cria a sua implementação, cada pod pode ser implementado no mesmo nó do trabalhador. Isso é conhecido como afinidade ou colocação. Para proteger seu app contra falha do nó do trabalhador, será possível configurar sua implementação para difundir os pods em múltiplos nós do trabalhador usando a opção <em>podAntiAffinity</em> com seus clusters padrão. É possível definir dois tipos de antiafinidade do pod: preferencial ou necessário. Para obter mais informações, veja a documentação do Kubernetes no <a href="https://kubernetes.io/docs/concepts/configuration/assign-pod-node/" rel="external" target="_blank" title="(Abre em uma nova guia ou janela)">Designando pods aos nós</a>.</p>
    <p><strong>Nota</strong>: com a antiafinidade necessária, só será possível implementar a quantia de réplicas para as quais você tiver nós do trabalhador. Por exemplo, se você tiver 3 nós do trabalhador no cluster, mas definir 5 réplicas no arquivo YAML, apenas 3 réplicas serão implementadas. Cada réplica mora em um nó de trabalhador diferente. Os restantes 2 réplicas continuam pendentes. Se você incluir outro nó do trabalhador no cluster, uma das réplicas restantes será implementada no novo nó do trabalhador automaticamente.<p>
    <p><strong>Exemplo de arquivos YAML de implementação</strong>:<ul>
    <li><a href="https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/nginx_preferredAntiAffinity.yaml" rel="external" target="_blank" title="(Abre em uma nova guia ou janela)">App Nginx com antiafinidade preferencial de pod.</a></li>
    <li><a href="https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/liberty_requiredAntiAffinity.yaml" rel="external" target="_blank" title="(Abre em uma nova guia ou janela)">App IBM® WebSphere® Application Server Liberty com antiafinidade preferencial de pod.</a></li></ul></p>
    </dd>
<dt>Distribuir pods em múltiplas zonas ou regiões</dt>
  <dd>Para proteger o app contra uma falha de local ou de região, será possível criar um segundo cluster em outro local ou região e usar um YAML de implementação para implementar um conjunto de réplicas duplicadas para o seu app. Incluindo uma rota e um balanceador de carga compartilhados na frente de seus clusters, é possível difundir a
carga de trabalho entre os locais e regiões. Para obter mais informações, veja [Alta disponibilidade de clusters](cs_clusters.html#clusters).
  </dd>
</dl>


### Implementação de app mínimo
{: #minimal_app_deployment}

Uma implementação básica de app em um cluster grátis ou padrão pode incluir os componentes a seguir.
{: shortdesc}

![Configuração de implementação](images/cs_app_tutorial_components1.png)

Para implementar os componentes para um app mínimo conforme descrito no diagrama, você usa um arquivo de configuração semelhante ao exemplo a seguir:
```
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: ibmliberty
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: ibmliberty
    spec:
      containers:
      - name: ibmliberty
        image: registry.bluemix.net/ibmliberty:latest
        ports:
        - containerPort: 9080        
---
apiVersion: v1
kind: Service
metadata:
  name: ibmliberty-service
  labels:
    app: ibmliberty
spec:
  selector:
    app: ibmliberty
  type: NodePort
  ports:
   - protocol: TCP
     port: 9080
```
{: codeblock}

**Nota:** para expor seu serviço, certifique-se de que o par de chave/valor usado na seção `spec.selector` do serviço é o mesmo par de chave/valor usado na seção `spec.template.metadata.labels` do yaml de sua implementação.
Para aprender mais sobre cada componente, revise os [Conceitos básicos do Kubernetes](cs_tech.html#kubernetes_basics).

<br />




## Ativando o painel do Kubernetes
{: #cli_dashboard}

Abra um painel do Kubernetes em seu sistema local para visualizar informações sobre um cluster e seus nós do trabalhador. [Na GUI](#db_gui), é possível acessar o painel com um conveniente botão de um clique. [Com a CLI](#db_cli), é possível acessar o painel ou usar as etapas em um processo de automação, como para um pipeline CI/CD.
{:shortdesc}

Antes de iniciar, [destine sua CLI](cs_cli_install.html#cs_cli_configure) para seu cluster. Essa tarefa requer a [política de acesso de Administrador](cs_users.html#access_policies). Verifique sua [política de acesso](cs_users.html#infra_access) atual.

É possível usar a porta padrão ou configurar sua própria porta para ativar o painel do Kubernetes para um cluster.

**Ativando o painel do Kubernetes por meio da GUI**
{: #db_gui}

1.  Efetue login no [{{site.data.keyword.Bluemix_notm}} GUI](https://console.bluemix.net/).
2.  No seu perfil na barra de menus, selecione a conta que você deseja usar.
3.  No menu, clique em **Contêineres**.
4.  Na página **Clusters**, clique no cluster que você deseja acessar.
5.  Na página de detalhes do cluster, clique no botão **Painel do Kubernetes**.

**Ativando o painel do Kubernetes por meio da CLI**
{: #db_cli}

*  Para clusters com uma versão mestre do Kubernetes de 1.7.16 ou anterior:

    1.  Configure o proxy com o número da porta padrão.

        ```
        kubectl proxy
        ```
        {: pre}

        Saída:

        ```
        Iniciando a entrega em 127.0.0.1:8001
        ```
        {: screen}

    2.  Abra o painel do Kubernetes em um navegador da web.

        ```
        http://localhost:8001/ui
        ```
        {: codeblock}

*  Para clusters com uma versão mestre do Kubernetes de 1.8.2 ou mais recente:

    1.  Obtenha suas credenciais para o Kubernetes.

        ```
        kubectl config view -o jsonpath='{.users[0].user.auth-provider.config.id-token}'
        ```
        {: pre}

    2.  Copie o valor **id-token** que é mostrado na saída.

    3.  Configure o proxy com o número da porta padrão.

        ```
        kubectl proxy
        ```
        {: pre}

        Saída de exemplo:

        ```
        Iniciando a entrega em 127.0.0.1:8001
        ```
        {: screen}

    4.  Conecte-se ao painel.

      1.  Em seu navegador, navegue para a URL a seguir:

          ```
          http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/
          ```
          {: codeblock}

      2.  Na página de conexão, selecione o método de autenticação **Token**.

      3.  Em seguida, cole o valor **id-token** que você copiou anteriormente no campo **Token** e clique em **CONECTAR**.

Quando estiver pronto com o painel do Kubernetes, use `CTRL+C` para sair do comando `proxy`. Depois de sair, o painel do Kubernetes não estará mais disponível. Execute o comando `proxy` para reiniciar o painel do Kubernetes.

[Em seguida, é possível executar um arquivo de configuração do painel.](#app_ui)


<br />




## Criando segredos
{: #secrets}

Segredos do Kubernetes são uma maneira segura para armazenar informação confidencial, como nomes de usuário,
senhas ou chaves.
{:shortdesc}

<table>
<caption>Arquivos necessários para armazenar em segredos por tarefa</caption>
<thead>
<th>Tarefas</th>
<th>Os arquivos necessários para armazenar em segredos</th>
</thead>
<tbody>
<tr>
<td>Incluir um serviço em um cluster</td>
<td>Nenhuma. Um segredo é criado quando você liga um serviço a um cluster.</td>
</tr>
<tr>
<td>Opcional: configure o serviço de Ingresso com TLS, se você não estiver usando o segredo do ingresso. <p><b>Nota</b>: o TLS já está ativado por padrão e um segredo já está criado pela Conexão TLS.

Para visualizar o segredo do TLS padrão:
<pre>
bx cs cluster-get &lt;cluster_name_or_ID&gt; | grep "Ingress secret"
</pre>
</p>
Para criar o seu próprio, conclua as etapas neste tópico.</td>
<td>Certificado e chave do servidor: <code>server.crt</code> e <code>server.key</code></td>
<tr>
<td>Crie a anotação de autenticação mútua.</td>
<td>Certificado de CA: <code>ca.crt</code></td>
</tr>
</tbody>
</table>

Para obter mais informações sobre o que é possível armazenar em segredos, veja a [documentação do Kubernetes](https://kubernetes.io/docs/concepts/configuration/secret/).



Para criar um segredo com um certificado:

1. Gere o certificado e a chave da autoridade de certificação (CA) de seu provedor de certificado. Se você tiver seu próprio domínio, compre um certificado TLS oficial para seu domínio. Para propósitos de teste, é possível gerar um certificado autoassinado.

 **Importante**: certifique-se de que o [CN](https://support.dnsimple.com/articles/what-is-common-name/) seja diferente para cada certificado.

 O certificado do cliente e a chave do cliente devem ser verificados até o certificado de raiz confiável que, neste caso, é o certificado de CA. Exemplo:

 ```
 Certificado do cliente: emitido pelo Certificado intermediário
 Certificado intermediário: emitido pelo Certificado raiz
 Certificado raiz: emitido por si mesmo
 ```
 {: codeblock}

2. Crie o certificado como um segredo do Kubernetes.

   ```
   kubectl create secret generic <secret_name> --from-file=<cert_file>=<cert_file>
   ```
   {: pre}

   Exemplos:
   - Conexão TLS:

     ```
     kubectl create secret tls <secret_name> --from-file=tls.crt=server.crt --from-file=tls.key=server.key
     ```
     {: pre}

   - Anotação de autenticação mútua:

     ```
     Kubectl create secret generic < secret_name> -- from-file=ca.crt=ca.crt
     ```
     {: pre}

<br />





## Implementando apps com a GUI
{: #app_ui}

Ao implementar um app em seu cluster usando o painel do Kubernetes, um recurso de implementação cria, atualiza e gerencia automaticamente os pods em seu cluster.
{:shortdesc}

Antes de iniciar:

-   Instale as [CLIs](cs_cli_install.html#cs_cli_install) necessárias.
-   [Destine sua CLI](cs_cli_install.html#cs_cli_configure) para seu cluster.

Para implementar seu app:

1.  Abra o [painel](#cli_dashboard) do Kubernetes e clique em **+ Criar**.
2.  Insira os detalhes do app em uma de duas maneiras.
  * Selecione **Especificar detalhes do app abaixo** e insira os detalhes.
  * Selecione **Fazer upload de um arquivo YAML ou JSON** para fazer upload do [arquivo de configuração de seu app ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/).

  Precisa de ajuda com seu arquivo de configuração. Verifique este [arquivo YAML de exemplo ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/IBM-Cloud/kube-samples/blob/master/deploy-apps-clusters/deploy-ibmliberty.yaml). Neste exemplo, um contêiner é implementado por meio da imagem **ibmliberty** na região sul dos EUA. Saiba mais sobre [como proteger suas informações pessoais](cs_secure.html#pi) quando trabalhar com recursos do Kubernetes.
  {: tip}

3.  Verifique se você implementou com sucesso o seu app em uma das maneiras a seguir.
  * No painel do Kubernetes, clique em **Implementações**. Uma lista de implementações bem-sucedidas é exibida.
  * Se o seu app estiver [publicamente disponível](cs_network_planning.html#public_access), navegue para a página de visão geral do cluster no painel do {{site.data.keyword.containerlong}}. Copie o subdomínio, que está localizado na seção de resumo do cluster e cole-o em um navegador para visualizar seu app.

<br />


## Implementando apps com a CLI
{: #app_cli}

Após um cluster ser criado, é possível implementar um app nesse cluster usando a CLI do Kubernetes.
{:shortdesc}

Antes de iniciar:

-   Instale as [CLIs](cs_cli_install.html#cs_cli_install) necessárias.
-   [Destine sua CLI](cs_cli_install.html#cs_cli_configure) para seu cluster.

Para implementar seu app:

1.  Crie um arquivo de configuração com base nas [melhores práticas do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/configuration/overview/). Geralmente, um arquivo de configuração contém detalhes de configuração para cada um dos recursos que você estiver criando no Kubernetes. Seu script pode incluir uma ou mais das seções a seguir:

    -   [Implementação ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/): define a criação de pods e conjuntos de réplicas. Um pod inclui um app conteinerizado individual e os conjuntos de réplicas controlam múltiplas instâncias de pods.

    -   [Serviço ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/services-networking/service/): fornece acesso de front-end para os pods usando um nó do trabalhador ou um endereço IP público do balanceador de carga ou uma rota pública do Ingresso.

    -   [Ingresso ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/services-networking/ingress/): especifica um tipo de balanceador de carga que fornece rotas para acessar seu app publicamente.

    Saiba mais sobre [como proteger suas informações pessoais](cs_secure.html#pi) quando trabalhar com recursos do Kubernetes.

2.  Execute o arquivo de configuração no contexto de um cluster.

    ```
    Kubectl apply -f config.yaml
    ```
    {: pre}

3.  Se você disponibilizou o seu app publicamente usando um serviço de porta de nó, um serviço de balanceador de carga ou Ingresso, verifique se é possível acessar o app.

<br />




## Implementando um app em uma máquina de GPU
{: #gpu_app}

Se você tem um [tipo de máquina de unidade de processamento de gráfico (GPU) bare metal](cs_clusters.html#shared_dedicated_node), é possível planejar cargas de trabalho matematicamente intensivas no nó do trabalhador. Por exemplo, você pode executar um app 3D que usa a plataforma Compute Unified Device Architecture (CUDA) para compartilhar a carga de processamento da GPU e CPU para aumentar o desempenho.
{:shortdesc}

Nas etapas a seguir, você aprenderá como implementar cargas de trabalho que requerem a GPU. Também é possível [implementar apps](#app_ui) que não precisam processar suas cargas de trabalho na GPU e CPU. Depois, você pode achar útil experimentar cargas de trabalho matematicamente intensivas, como a estrutura de aprendizado de máquina [TensorFlow ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.tensorflow.org/) com [esta demo do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/pachyderm/pachyderm/tree/master/doc/examples/ml/tensorflow).

Antes de iniciar:
* [Crie um tipo de máquina de GPU bare metal](cs_clusters.html#clusters_cli). Observe que esse processo pode levar mais de 1 dia útil para ser concluído.
* Seu cluster mestre e o nó do trabalhador de GPU devem executar o Kubernetes versão 1.10 ou mais recente.

Para executar uma carga de trabalho em uma máquina de GPU:
1.  Crie um arquivo YAML. Neste exemplo, um `Job` YAML gerencia cargas de trabalho em lote, fazendo um pod de curta duração que é executado até que o comando planejado para ser concluído com êxito seja finalizado.

    **Importante**: para cargas de trabalho de GPU, deve-se sempre fornecer o campo `resources: limits: nvidia.com/gpu` na especificação YAML.

    ```yaml
    apiVersion: batch/v1
    kind: Job
    metadata:
      name: nvidia-smi
      labels:
        name: nvidia-smi
    spec:
      template:
        metadata:
          labels:
            name: nvidia-smi
        spec:
          containers:
          - name: nvidia-smi
            image: nvidia/cuda:9.1-base-ubuntu16.04
            command: [ "/usr/test/nvidia-smi" ]
            imagePullPolicy: IfNotPresent
            resources:
              limits:
                nvidia.com/gpu: 2
            volumeMounts:
            - mountPath: /usr/test
              name: nvidia0
          volumes:
            - name: nvidia0
              hostPath:
                path: /usr/bin
          restartPolicy: Never
    ```
    {: codeblock}

    <table>
    <caption>Componentes do YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes de arquivo do YAML</th>
    </thead>
    <tbody>
    <tr>
    <td>Metadados e nomes de rótulo</td>
    <td>Dê um nome e um rótulo para a tarefa e use o mesmo nome nos metadados do arquivo e nos metadados de `spec template`. Por exemplo, `nvidia-smi`.</td>
    </tr>
    <tr>
    <td><code>contêineres / imagem</code></td>
    <td>Forneça a imagem da qual o contêiner é uma instância em execução. Neste exemplo, o valor é configurado para usar a imagem do DockerHub CUDA:<code>nvidia/cuda:9.1-base-ubuntu16.04</code></td>
    </tr>
    <tr>
    <td><code>contêineres / comando</code></td>
    <td>Especifique um comando para ser executado no contêiner. Neste exemplo, o comando <code>[ "/usr/test/nvidia-smi" ]</code>refere-se a um binário que está na máquina de GPU; portanto, deve-se também configurar uma montagem do volume.</td>
    </tr>
    <tr>
    <td><code>contêineres / imagePullPolicy</code></td>
    <td>Para puxar uma nova imagem somente se a imagem não estiver atualmente no nó do trabalhador, especifique <code>IfNotPresent</code>.</td>
    </tr>
    <tr>
    <td><code>resources / limites</code></td>
    <td>Para máquinas de GPU, deve-se especificar o limite de recurso. O [Plug-in do dispositivo![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/cluster-administration/device-plugins/) do Kubernetes configura a solicitação de recurso padrão para corresponder ao limite.
    <ul><li>Deve-se especificar a chave como <code>nvidia.com/gpu</code>.</li>
    <li>Insira o número inteiro de GPUs que você solicitar, como <code>2</code>. <strong>Nota</strong>: os pods do contêiner não compartilham GPUs e as GPUs não podem estar supercomprometidas. Por exemplo, se você tem somente 1 máquina `mg1c.16x128`, então tem apenas 2 GPUs nessa máquina e pode especificar um máximo de `2`.</li></ul></td>
    </tr>
    <tr>
    <td><code>volumeMounts</code></td>
    <td>Nomeie o volume que está montado no contêiner, como <code>nvidia0</code>. Especifique o <code>mountPath</code> no contêiner para o volume. Neste exemplo, o caminho <code>/usr/test</code> corresponde ao caminho usado no comando de contêiner de tarefa.</td>
    </tr>
    <tr>
    <td><code>volumes</code></td>
    <td>Nomeie o volume de tarefa, como <code>nvidia0</code>. No <code>hostPath</code> do nó do trabalhador da GPU, especifique o <code>path</code> do volume no host, neste exemplo, <code>/usr/bin</code>. O contêiner <code>mountPath</code> é mapeado para o <code>path</code> do volume do host, que fornece esse acesso de tarefa aos binários do NVIDIA no nó do trabalhador da GPU para o comando de contêiner a ser executado.</td>
    </tr>
    </tbody></table>

2.  Aplique o arquivo YAML. Por exemplo:

    ```
    Kubectl apply -f nvidia-smi.yaml
    ```
    {: pre}

3.  Verifique o pod de tarefa filtrando os pods pelo rótulo `nvidia-sim`. Verifique se o **STATUS** é **Concluído**.

    ```
    kubectl get pod -a -l 'name in (nvidia-sim)'
    ```
    {: pre}

    Saída de exemplo:
    ```
    NAME                  READY     STATUS      RESTARTS   AGE
    nvidia-smi-ppkd4      0/1       Completed   0          36s
    ```
    {: screen}

4.  Descreva o pod para ver como o plug-in do dispositivo de GPU planejou o pod.
    * Nos campos `Limits` e `Requests`, veja se o limite de recurso que você especificou corresponde à solicitação que o plug-in do dispositivo configurou automaticamente.
    * Nos eventos, verifique se o pod está designado ao seu nó do trabalhador da GPU.

    ```
    Kubectl describe pod nvidia-smi-ppkd4
    ```
    {: pre}

    Saída de exemplo:
    ```
    Name:           nvidia-smi-ppkd4
    Namespace:      default
    ...
    Limits:
     nvidia.com/gpu:  2
    Requests:
     nvidia.com/gpu:  2
    ...
    Events:
    Type    Reason                 Age   From                     Message
    ----    ------                 ----  ----                     -------
    Normal  Scheduled              1m    default-scheduler        Successfully assigned nvidia-smi-ppkd4 to 10.xxx.xx.xxx
    ...
    ```
    {: screen}

5.  Para verificar se a tarefa usou a GPU para calcular sua carga de trabalho, é possível verificar os logs. O comando `[ "/usr/test/nvidia-smi" ]` da tarefa consultou o estado do dispositivo GPU no nó do trabalhador da GPU.

    ```
    Kubectl logs nvidia a simulação ppkd4
    ```
    {: pre}

    Saída de exemplo:
    ```
    +-----------------------------------------------------------------------------+
    | NVIDIA-SMI 390.12                 Driver Version: 390.12                    |
    |-------------------------------+----------------------+----------------------+
    | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
    | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
    |===============================+======================+======================|
    |   0  Tesla K80           Off  | 00000000:83:00.0 Off |                  Off |
    | N/A   37C    P0    57W / 149W |      0MiB / 12206MiB |      0%      Default |
    +-------------------------------+----------------------+----------------------+
    |   1  Tesla K80           Off  | 00000000:84:00.0 Off |                  Off |
    | N/A   32C    P0    63W / 149W |      0MiB / 12206MiB |      1%      Default |
    +-------------------------------+----------------------+----------------------+

    +-----------------------------------------------------------------------------+
    | Processes:                                                       GPU Memory |
    |  GPU       PID   Type   Process name                             Usage      |
    |=============================================================================|
    |  No running processes found                                                 |
    +-----------------------------------------------------------------------------+
    ```
    {: screen}

    Neste exemplo, você vê que ambas as GPUs foram usadas para executar a tarefa porque foram planejadas no nó do trabalhador. Se o limite for configurado como 1, somente 1 GPU será mostrada.

## Ajuste de escala de apps 
{: #app_scaling}

Com o Kubernetes, é possível ativar o [ajuste automático de escala de pod horizontal ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) para aumentar ou diminuir automaticamente o número de instâncias de seus apps com base na CPU.
{:shortdesc}

Procurando informações sobre ajuste de escala de aplicativos Cloud Foundry? Confira [IBM Auto-Scaling for {{site.data.keyword.Bluemix_notm}}](/docs/services/Auto-Scaling/index.html). 
{: tip}

Antes de iniciar:
- [Destine sua CLI](cs_cli_install.html#cs_cli_configure) para seu cluster.
- O monitoramento Heapster deve ser implementado no cluster em que você deseja ajustar a escala automaticamente.

Etapas:

1.  Implemente seu app em um cluster por meio da CLI. Ao implementar seu app, deve-se solicitar a CPU.

    ```
    kubectl run <app_name> --image=<image> --requests=cpu=<cpu> --expose --port=<port_number>
    ```
    {: pre}

    <table>
    <caption>Componentes de comando para executar kubectl</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes desse comando</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--image</code></td>
    <td>O aplicativo que você deseja implementar.</td>
    </tr>
    <tr>
    <td><code>--request=cpu</code></td>
    <td>A CPU necessária para o contêiner, que é especificada em milinúcleos. Como um exemplo, <code>--requests=200m</code>.</td>
    </tr>
    <tr>
    <td><code>--expose</code></td>
    <td>Quando true, cria um serviço externo.</td>
    </tr>
    <tr>
    <td><code>--port</code></td>
    <td>A porta em que seu app está disponível externamente.</td>
    </tr></tbody></table>

    Para implementações mais complexas, você pode precisar criar um [arquivo de configuração](#app_cli).
    {: tip}

2.  Crie um ajustador automático de escala e defina sua política. Para obter mais informações sobre como trabalhar com o comando `kubectl autoscale`, veja [a documentação do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://v1-8.docs.kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#autoscale).

    ```
    kubectl autoscale deployment <deployment_name> --cpu-percent=<percentage> --min=<min_value> --max=<max_value>
    ```
    {: pre}

    <table>
    <caption>Componentes de comando para kubectl autoscale</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes desse comando</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--cpu-percent</code></td>
    <td>A utilização média da CPU que é mantida pelo Escalador automático de ajuste de pod horizontal, que é especificada em porcentagem.</td>
    </tr>
    <tr>
    <td><code>--min</code></td>
    <td>O número mínimo de pods implementados que são usados para manter a porcentagem de utilização da CPU especificada.</td>
    </tr>
    <tr>
    <td><code>--max</code></td>
    <td>O número máximo de pods implementados que são usados para manter a porcentagem de utilização da CPU especificada.</td>
    </tr>
    </tbody></table>


<br />


## Gerenciando implementações de rolagem
{: #app_rolling}

É possível gerenciar o lançamento de suas mudanças de forma automatizada e controlada. Se a sua apresentação não estiver indo de acordo com o plano, será possível recuperar a sua implementação para a revisão anterior.
{:shortdesc}

Antes de iniciar, crie uma [implementação](#app_cli).

1.  [Apresente ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#updating-a-deployment) uma mudança. Por exemplo, talvez você queira mudar a imagem usada na implementação inicial.

    1.  Obtenha o nome da implementação.

        ```
        kubectl get deployments
        ```
        {: pre}

    2.  Obtenha o nome do pod.

        ```
        kubectl get pods
        ```
        {: pre}

    3.  Obtenha o nome do contêiner que está em execução no pod.

        ```
        kubectl describe pod <pod_name>
        ```
        {: pre}

    4.  Configure a nova imagem para a implementação para usar.

        ```
        kubectl set image deployment/<deployment_name><container_name>=<image_name>
        ```
        {: pre}

    Ao executar os comandos, a mudança é imediatamente aplicada e registrada no histórico de apresentação.

2.  Verifique o status de sua implementação.

    ```
    kubectl rollout status deployments/<deployment_name>
    ```
    {: pre}

3.  Recupere uma mudança.
    1.  Visualize o histórico de apresentação da implementação e identifique o número da revisão de sua última implementação.

        ```
        kubectl rollout history deployment/<deployment_name>
        ```
        {: pre}

        **Dica:** para ver os detalhes de uma revisão específica, inclua o número da revisão.

        ```
        kubectl rollout history deployment/<deployment_name> --revision=<number>
        ```
        {: pre}

    2.  Recupere para a versão anterior ou especifique uma revisão. Para recuperar para a versão anterior, use o comando a seguir.

        ```
        kubectl rollout undo deployment/<depoyment_name> --to-revision=<number>
        ```
        {: pre}

<br />

