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



# Tutorial: Migrando um app do Cloud Foundry para um cluster
{: #cf_tutorial}

É possível usar um app implementado anteriormente usando o Cloud Foundry e implementar o mesmo código em um contêiner para um cluster do Kubernetes no {{site.data.keyword.containershort_notm}}.
{: shortdesc}


## Objetivos

- Conhecer o processo geral de implementação de apps em contêineres para um cluster do Kubernetes.
- Criar um Dockerfile de seu app código para construir uma imagem de contêiner.
- Implementar um contêiner dessa imagem em um cluster do Kubernetes.

## Tempo Necessário
30 minutos

## Público
Este tutorial é destinado a desenvolvedores de apps Cloud Foundry.

## Pré-requisitos

- [Criar um registro de imagem privada no {{site.data.keyword.registrylong_notm}}](../services/Registry/index.html).
- [Criar um cluster](cs_clusters.html#clusters_ui).
- [Destinar sua CLI para o cluster](cs_cli_install.html#cs_cli_configure).
- [Conhecer a terminologia do Docker e do Kubernetes](cs_tech.html).


<br />



## Lição 1: Fazer download do código do app

Pegue seu código pronto para ir. Não tem nenhum código ainda? É possível fazer download do código de início para usar neste tutorial.
{: shortdesc}

1. Crie um diretório denominado `cf-py` e navegue para ele. Nesse diretório, salve todos os arquivos necessários para construir a imagem do Docker e para executar o seu app.

  ```
  mkdir cf-py && cd cf-py
  ```
  {: pre}

2. Copie o código de app e todos os arquivos relacionados para o diretório. É possível usar seu próprio código de app ou fazer download do modelo do catálogo. Esse tutorial usa o modelo Python Flask. No entanto, é possível usar as mesmas etapas básicas com um app Node.js, Java ou [Kitura](https://github.com/IBM-Cloud/Kitura-Starter).

    Para fazer download do código do app Python Flask:

    a. No catálogo, em **Modelos**, clique em **Python Flask**. Esse modelo inclui um ambiente de tempo de execução para os apps Python 2 e Python 3.

    b. Insira o nome do app `cf-py-<name>` e clique em **CREATE**. Para acessar o código do app para o modelo, deve-se implementar o app CF na nuvem primeiro. É possível usar qualquer nome para o app. Se você usar o nome do exemplo, substitua `<name>` por um identificador exclusivo, como `cf-py-msx`.
    
    **Atenção**: não use informações pessoais em nenhum app, imagem de contêiner ou nomes de recursos do Kubernetes.

    Conforme o app é implementado, instruções para "Fazer download, modificar e reimplementar seu app com a interface da linha de comandos" são exibidas.

    c. Na etapa 1 nas instruções da GUI, clique em **FAZER DOWNLOAD DO CÓDIGO DE INÍCIO**.

    d. Extraia o arquivo .zip e salve seu conteúdo no diretório `cf-py`.

Seu código de app está pronto para ser conteinerizado!


<br />



## Lição 2: Criando uma imagem do Docker com seu código de app

Crie um Dockerfile que inclua seu código de app e as configurações necessárias para seu contêiner. Em seguida, construa uma imagem do Docker desse Dockerfile e envie-a por push para seu registro de imagem privada.
{: shortdesc}

1. No diretório `cf-py` criado na lição anterior, crie um `Dockerfile`, que é a base para criar uma imagem de contêiner. É possível criar o Dockerfile usando seu editor de CLI preferencial ou um editor de texto no
computador. O exemplo a seguir mostra como criar um arquivo Dockerfile com o editor nano.

  ```
  nano Dockerfile
  ```
  {: pre}

2. Copie o script a seguir para o Dockerfile. Esse Dockerfile se aplica especificamente a um app Python. Se você estiver usando outro tipo de código, seu Dockerfile deverá incluir uma imagem base diferente e poderá requerer que outros campos sejam definidos.

  ```
  #Use the Python image from DockerHub as a base image
  FROM python:3-slim

  #Expose the port for your python app
  EXPOSE 5000

  #Copy all app files from the current directory into the app
  #directory in your container. Set the app directory
  #as the working directory
  WORKDIR /cf-py/
  COPY .  .

  #Install any requirements that are defined
  RUN pip install --no-cache-dir -r requirements.txt

  #Update the openssl package
  RUN apt-get update && apt-get install -y \
  openssl

  #Start o app.
  CMD [ "python", "welcome.py " ]
  ```
  {: codeblock}

3. Salve suas mudanças no editor nano pressionando `ctrl + o`. Confirme suas mudanças pressionando `Enter`. Saia do editor nano pressionando `ctrl + x`.

4. Construa uma imagem do Docker que inclua seu código de app e envie-o por push para seu registro privado.

  ```
  bx cr build -t registry.<region>.bluemix.net/namespace/cf-py .
  ```
  {: pre}

  <table>
  <caption>Entendendo os componentes deste comando</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Este ícone indica que há mais informações para conhecer sobre os componentes desse comando."/> Entendendo os componentes desse comando</th>
  </thead>
  <tbody>
  <tr>
  <td>Parâmetro</td>
  <td>Descrição</td>
  </tr>
  <tr>
  <td><code>build</code></td>
  <td>O comando de construção.</td>
  </tr>
  <tr>
  <td><code>-t registry.&lt;region&gt;.bluemix.net/namespace/cf-py</code></td>
  <td>O seu caminho do registro privado, que inclui seu namespace exclusivo e o nome da imagem. Para este exemplo, o mesmo nome é usado para a imagem como o diretório do app, mas é possível escolher qualquer nome para a imagem em seu registro privado. Se você não tiver certeza de qual é o seu espaço de nomes, execute o comando `bx cr namespaces` para localizá-lo.</td>
  </tr>
  <tr>
  <td><code>.</code></td>
  <td>A localização do Dockerfile. Se você estiver executando o comando de construção a partir do diretório que
inclui o Dockerfile, insira um ponto (.). Caso contrário, use o caminho relativo para o Dockerfile.</td>
  </tr>
  </tbody>
  </table>

  A imagem é criada em seu registro privado. É possível executar o comando `bx cr images` para verificar se a imagem foi criada.

  ```
  REPOSITORY                                     NAMESPACE   TAG      DIGEST         CREATED         SIZE     VULNERABILITY STATUS   
  registry.ng.bluemix.net/namespace/cf-py        namespace   latest   cb03170b2cb2   3 minutes ago   271 MB   OK
  ```
  {: screen}


<br />



## Lição 3: Implementar um contêiner a partir de sua imagem

Implemente seu app como um contêiner em um cluster do Kubernetes.
{: shortdesc}

1. Crie um arquivo YAML de configuração denominado `cf-py.yaml` e atualize `<registry_namespace>` com o nome de seu registro de imagem privada. Esse arquivo de configuração define uma implementação de contêiner da imagem criada na lição anterior e um serviço para expor o app ao público.

  ```
  apiVersion: extensions/v1beta1
  kind: Deployment
  metadata:
    labels:
      app: cf-py
    name: cf-py
    namespace: default
  spec:
    selector:
      matchLabels:
        app: cf-py
    replicas: 1
    template:
      metadata:
        labels:
          app: cf-py
      spec:
        containers:
        - image: registry.ng.bluemix.net/<registry_namespace>/cf-py:latest
          name: cf-py
  ---
  apiVersion: v1
  kind: Service
  metadata:
    name: cf-py-nodeport
    labels:
      app: cf-py
  spec:
    selector:
      app: cf-py
    type: NodePort
    ports:
     - port: 5000
       nodePort: 30872
  ```
  {: codeblock}

  <table>
  <caption>Entendendo os componentes de arquivo YAML</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes de arquivo do YAML</th>
  </thead>
  <tbody>
  <tr>
  <td><code>imagem</code></td>
  <td>Em `registry.ng.bluemix.net/<registry_namespace>/cf-py:latest`, substitua &lt;registry_namespace&gt; pelo namespace de seu registro de imagem privada. Se você não tiver certeza de qual é o seu espaço de nomes, execute o comando `bx cr namespaces` para localizá-lo.</td>
  </tr>
  <tr>
  <td><code>nodePort</code></td>
  <td>Exponha seu app criando um serviço do Kubernetes do tipo NodePort. Os NodePorts estão no intervalo de 30.000 - 32.767. Use essa porta para testar seu app em um navegador posteriormente.</td>
  </tr>
  </tbody></table>

2. Aplique o arquivo de configuração para criar a implementação e o serviço em seu cluster.

  ```
  Kubectl apply filepath> -f < /cf-py.yaml
  ```
  {: pre}

  Saída:

  ```
  deployment "cf-py" configured
  service "cf-py-nodeport" configured
  ```
  {: screen}

3. Agora que todo o trabalho de implementação está pronto, é possível testar seu app em um navegador. Obtenha os detalhes para formar a URL.

    a.  Obtenha o endereço IP público para o nó do trabalhador no cluster.

    ```
    bx cs workers <cluster_name>
    ```
    {: pre}

    Saída:

    ```
    ID                                                 Public IP        Private IP     Machine Type        State    Status   Zone    Version   
    kube-dal10-cr18e61e63c6e94b658596ca93d087eed9-w1   169.xx.xxx.xxx   10.xxx.xx.xxx   u2c.2x4.encrypted   normal   Ready    dal10   1.9.7
    ```
    {: screen}

    b. Abra um navegador e verifique o app com a URL a seguir: `http://<public_IP_address>:<NodePort>`. Com os valores de exemplo, a URL é `http://169.xx.xxx.xxx:30872`. É possível fornecer essa URL a um colega de trabalho para experimentá-la ou inseri-la no navegador do seu telefone celular, para que seja possível ver se o app está realmente disponível publicamente.

    <img src="images/python_flask.png" alt="Uma captura de tela do app Python Flask de modelo implementado." />

5.  [Ativar o painel do Kubernetes](cs_app.html#cli_dashboard).

    Se selecionar seu cluster na GUI do [{{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/), será possível usar o botão **Painel do Kubernetes** para ativar seu painel com um clique.
    {: tip}

6. Na guia **Cargas de trabalho**, é possível ver os recursos que você criou.

Parabéns! Seu app está implementado em um contêiner!
