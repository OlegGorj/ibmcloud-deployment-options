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




# Abrindo portas e endereços IP necessários em seu firewall
{: #firewall}

Revise estas situações nas quais pode ser necessário abrir portas e endereços IP específicos em seus firewalls para o {{site.data.keyword.containerlong}}.
{:shortdesc}

* [Para executar comandos `bx` ](#firewall_bx) de seu sistema local quando as políticas de rede corporativa impedem o acesso aos terminais de Internet pública por meio de proxies ou de firewalls.
* [Para executar comandos `kubectl`](#firewall_kubectl) em seu sistema local quando políticas de rede corporativas evitam acesso a terminais de Internet pública por meio de proxies ou de firewalls.
* [Para executar comandos `calicoctl` ](#firewall_calicoctl)de seu sistema local quando as políticas de rede corporativa impedem o acesso aos terminais de Internet pública por meio de proxies ou de firewalls.
* [Para permitir a comunicação entre o mestre do Kubernetes e os nós do trabalhador](#firewall_outbound) quando um firewall for configurado para os nós do trabalhador ou as configurações de firewall forem customizadas em sua conta de infraestrutura do IBM Cloud (SoftLayer).
* [Para acessar o serviço NodePort, o serviço LoadBalancer ou o Ingress de fora do cluster](#firewall_inbound).

<br />


## Executando comandos `bx cs` por trás de um firewall
{: #firewall_bx}

Se as políticas de rede corporativa impedirem o acesso de seu sistema local a terminais públicos por proxies ou firewalls, para executar os comandos `bx cs`, deve-se permitir acesso TCP ao {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

1. Permita o acesso a `containers.bluemix.net` na porta 443.
2. Verifique sua conexão. Se o acesso for configurado corretamente, navios serão exibidos na saída.
   ```
   curl https://containers.bluemix.net/v1/
   ```
   {: pre}

   Saída de exemplo:
   ```
                                     )___(
                              _______/__/_
                     ___     /===========|   ___
    ____       __   [\\\]___/____________|__[///]   __
    \   \_____[\\]__/___________________________\__[//]___
     \                                                    |
      \                                                  /
   ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ```
   {: screen}

<br />


## Executando comandos `kubectl` por trás de um firewall
{: #firewall_kubectl}

Se as políticas de rede corporativa impedirem o acesso de seu sistema local a terminais públicos por proxies ou firewalls, para executar os comandos `kubectl`, deve-se permitir o acesso TCP para o cluster.
{:shortdesc}

Quando um cluster é criado, a porta na URL principal é designada aleatoriamente de 20000 a 32767. É possível escolher abrir o intervalo de portas 20000 a 32767 para qualquer cluster que pode ser criado ou é possível escolher permitir o acesso a um cluster existente específico.

Antes de iniciar, permita acesso aos comandos [executar `bx cs`](#firewall_bx).

Para permitir acesso para um cluster específico:

1. Efetue login na CLI do {{site.data.keyword.Bluemix_notm}}. Insira suas credenciais do {{site.data.keyword.Bluemix_notm}} quando solicitadas. Se você tiver uma conta federada, inclua a opção `--sso`.

   ```
   bx login [--sso]
   ```
   {: pre}

2. Selecione a região em que seu cluster está dentro.

   ```
   bx cs region-set
   ```
   {: pre}

3. Obtenha o nome do cluster.

   ```
   bx cs clusters
   ```
   {: pre}

4. Recupere a **URL principal** para seu cluster.

   ```
   bx cs cluster-get <cluster_name_or_ID>
   ```
   {: pre}

   Saída de exemplo:
   ```
   ...
   Master URL:		https://169.xx.xxx.xxx:31142
   ...
   ```
   {: screen}

5. Permita acesso ao **Master URL** na porta, como a porta `31142` do exemplo anterior.

6. Verifique sua conexão.

   ```
   curl --insecure <master_URL>/version
   ```
   {: pre}

   Exemplo de comando:
   ```
   Curl -- inseguro https://169.xx.xxx.xxx:31142/version
   ```
   {: pre}

   Saída de exemplo:
   ```
   {
     "major": "1",
     "minor": "7+",
     "gitVersion": "v1.7.4-2+eb9172c211dc41",
     "gitCommit": "eb9172c211dc4108341c0fd5340ee5200f0ec534",
     "gitTreeState": "clean",
     "buildDate": "2017-11-16T08:13:08Z",
     "goVersion": "go1.8.3",
     "compiler": "gc",
     "platform": "linux/amd64"
   }
   ```
   {: screen}

7. Opcional: repita essas etapas para cada cluster que você precisa expor.

<br />


## Executando comandos `calicoctl` por trás de um firewall
{: #firewall_calicoctl}

Se as políticas de rede corporativa impedem o acesso de seu sistema local a terminais públicos por proxies ou firewalls, para executar comandos `calicoctl`, deve-se permitir acesso TCP aos comandos do Calico.
{:shortdesc}

Antes de iniciar, permita acesso aos comandos [`bx` ](#firewall_bx) e aos comandos [`kubectl`](#firewall_kubectl).

1. Recupere o endereço IP da URL principal que você usou para permitir os comandos [`kubectl`](#firewall_kubectl).

2. Obtenha a porta para ETCD.

  ```
  kubectl get cm -n kube-system calico-config -o yaml | grep etcd_endpoints
  ```
  {: pre}

3. Permita acesso para as políticas do Calico por meio do endereço IP principal da URL e a porta ETCD.

<br />


## Permitindo o cluster para acessar recursos de infraestrutura e outros serviços
{: #firewall_outbound}

Deixe seus recursos e serviços de infraestrutura de acesso ao cluster atrás de um firewall, como para regiões do {{site.data.keyword.containershort_notm}}, {{site.data.keyword.registrylong_notm}}, {{site.data.keyword.monitoringlong_notm}}, {{site.data.keyword.loganalysislong_notm}}, IPs privados de infraestrutura do IBM Cloud (SoftLayer) e egresso para solicitações de volume persistente.
{:shortdesc}

  1.  Anote o endereço IP público para todos os nós do trabalhador no cluster.

      ```
      bx cs workers <cluster_name_or_ID>
      ```
      {: pre}

  2.  Permita tráfego de rede de saída da origem _<each_worker_node_publicIP>_ para o intervalo de portas TCP/UDP de destino 20000 a 32767 e a porta 443 e os seguintes endereços IP e grupos de rede. Se você tiver um firewall corporativo que impede sua máquina local de acessar terminais de Internet pública, execute essa etapa para os nós do trabalhador de origem e sua máquina local.
      - **Importante**: deve-se permitir o tráfego de saída para a porta 443 para todos os locais dentro da região, para equilibrar a carga durante o processo de autoinicialização. Por exemplo, se o seu cluster estiver no Sul dos EUA, deve-se permitir o tráfego da porta 443 para os endereços IP para todos os locais (dal10, dal12 e dal13).
      <p>
  <table summary="A primeira linha na tabela abrange ambas as colunas. O resto das linhas deve ser lido da esquerda para a direita, com o local do servidor na coluna um e os endereços IP a serem correspondidos na coluna dois.">
  <caption>Endereços IP a serem abertos para o tráfego de saída</caption>
      <thead>
      <th>Região</th>
      <th>Localização</th>
      <th>Endereço IP</th>
      </thead>
    <tbody>
      <tr>
        <td>AP Norte</td>
        <td>hkg02<br>seo01<br>sng01<br>tok02</td>
        <td><code>169.56.132.234</code><br><code>169.56.69.242</code><br><code>161.202.186.226</code><br><code>161.202.126.210</code></td>
       </tr>
      <tr>
         <td>AP Sul</td>
         <td>mel01<br>syd01<br>syd04</td>
         <td><code>168.1.97.67</code><br><code>168.1.8.195</code><br><code>130.198.64.19, 130.198.66.34</code></td>
      </tr>
      <tr>
         <td>União Europeia Central</td>
         <td>ams03<br>fra02<br>mil01<br>par01</td>
         <td><code>169.50.169.110, 169.50.154.194</code><br><code>169.50.56.174</code><br><code>159.122.190.98</code><br><code>159.8.86.149, 159.8.98.170</code></td>
        </tr>
      <tr>
        <td>Sul do Reino Unido</td>
        <td>lon02<br>lon04</td>
        <td><code>159.122.242.78</code><br><code>158.175.65.170, 158.175.74.170, 158.175.76.2</code></td>
      </tr>
      <tr>
        <td>Leste dos EUA</td>
         <td>mon01<br>tor01<br>wdc06<br>wdc07</td>
         <td><code>169.54.126.219</code><br><code>169.53.167.50</code><br><code>169.60.73.142</code><br><code>169.61.83.62</code></td>
      </tr>
      <tr>
        <td>SUL dos EUA</td>
        <td>dal10<br>dal12<br>dal13<br>hou02<br>sao01</td>
        <td><code>169.47.234.18, 169.46.7.238</code><br><code>169.47.70.10</code><br><code>169.60.128.2</code><br><code>184.173.44.62</code><br><code>169.57.151.10</code></td>
      </tr>
      </tbody>
    </table>
</p>

  3.  Permita o tráfego de rede de saída dos nós do trabalhador para [regiões do {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/registry_overview.html#registry_regions):
      - `TCP port 443 FROM <each_worker_node_publicIP> TO <registry_publicIP>`
      - Substitua <em>&lt;registry_publicIP&gt;</em> pelos endereços IP de registro para os quais você deseja permitir o tráfego. O registro global armazena imagens públicas fornecidas pela IBM e os registros regionais armazenam suas próprias imagens privadas ou públicas.
        <p>
<table summary="A primeira linha na tabela abrange ambas as colunas. O resto das linhas deve ser lido da esquerda para a direita, com o local do servidor na coluna um e os endereços IP a serem correspondidos na coluna dois.">
  <caption>Endereços IP a serem abertos para o tráfego de Registro</caption>
      <thead>
        <th>Região do {{site.data.keyword.containershort_notm}}</th>
        <th>Endereço de registro</th>
        <th>Endereço IP de registro</th>
      </thead>
      <tbody>
        <tr>
          <td>Registro global em {{site.data.keyword.containershort_notm}} regiões</td>
          <td>registry.bluemix.net</td>
          <td><code>169.60.72.144/28</code><br><code>169.61.76.176/28</code></td>
        </tr>
        <tr>
          <td>AP Norte, AP Sul</td>
          <td>registry.au-syd.bluemix.net</td>
          <td><code>168.1.45.160/27</code></br><code>168.1.139.32/27</code></td>
        </tr>
        <tr>
          <td>União Europeia Central</td>
          <td>registry.eu-de.bluemix.net</td>
          <td><code>169.50.56.144/28</code></br><code>159.8.73.80/28</code></td>
         </tr>
         <tr>
          <td>Sul do Reino Unido</td>
          <td>registry.eu-gb.bluemix.net</td>
          <td><code>159.8.188.160/27</code></br><code>169.50.153.64/27</code></td>
         </tr>
         <tr>
          <td>Leste dos EUA, Sul dos EUA</td>
          <td>registry.ng.bluemix.net</td>
          <td><code>169.55.39.112/28</code></br><code>169.46.9.0/27</code></br><code>169.55.211.0/27</code></td>
         </tr>
        </tbody>
      </table>
</p>

  4.  Opcional: permita o tráfego de rede de saída dos nós do trabalhador para os serviços do {{site.data.keyword.monitoringlong_notm}} e do {{site.data.keyword.loganalysislong_notm}}:
      - `TCP port 443, port 9095 FROM <each_worker_node_public_IP> TO <monitoring_public_IP>`
      - Substitua <em>&lt;monitoring_public_IP&gt;</em> por todos os endereços das regiões de monitoramento para as quais você deseja permitir o tráfego:
        <p><table summary="A primeira linha na tabela abrange ambas as colunas. O resto das linhas deve ser lido da esquerda para a direita, com o local do servidor na coluna um e os endereços IP a serem correspondidos na coluna dois.">
  <caption>Endereços IP a serem abertos para o tráfego de monitoramento</caption>
        <thead>
        <th>Região do {{site.data.keyword.containershort_notm}}</th>
        <th>Endereço de monitoramento</th>
        <th>Endereços IP de monitoramento</th>
        </thead>
      <tbody>
        <tr>
         <td>União Europeia Central</td>
         <td>Metrics.eu-de.bluemix.net</td>
         <td><code>159.122.78.136/29</code></td>
        </tr>
        <tr>
         <td>Sul do Reino Unido</td>
         <td>Metrics.eu-gb.bluemix.net</td>
         <td><code>169.50.196.136/29</code></td>
        </tr>
        <tr>
          <td>Leste dos EUA, Sul dos EUA, AP Norte</td>
          <td>Metrics.ng.bluemix.net</td>
          <td><code>169.47.204.128/29</code></td>
         </tr>
         
        </tbody>
      </table>
</p>
      - `TCP port 443, port 9091 FROM <each_worker_node_public_IP> TO <logging_public_IP>`
      - Substitua <em>&lt;logging_public_IP&gt;</em> por todos os endereços das regiões de criação de log para as quais você deseja permitir o tráfego:
        <p><table summary="A primeira linha na tabela abrange ambas as colunas. O resto das linhas deve ser lido da esquerda para a direita, com o local do servidor na coluna um e os endereços IP a serem correspondidos na coluna dois.">
<caption>Endereços IP a serem abertos para o tráfego de criação de log</caption>
        <thead>
        <th>Região do {{site.data.keyword.containershort_notm}}</th>
        <th>Endereço de criação de log</th>
        <th>Endereços IP de log</th>
        </thead>
        <tbody>
          <tr>
            <td>Leste dos EUA, Sul dos EUA</td>
            <td>ingest.logging.ng.bluemix.net</td>
            <td><code>169.48.79.236</code><br><code>169.46.186.113</code></td>
           </tr>
          <tr>
           <td>Sul do Reino Unido</td>
           <td>ingest.logging.eu-gb.bluemix.net</td>
           <td><code>169.50.115.113</code></td>
          </tr>
          <tr>
           <td>União Europeia Central</td>
           <td>ingest-eu-fra.logging.bluemix.net</td>
           <td><code>158.177.88.43</code><br><code>159.122.87.107</code></td>
          </tr>
          <tr>
           <td>AP Sul, AP Norte</td>
           <td>ingest-au-syd.logging.bluemix.net</td>
           <td><code>130.198.76.125</code><br><code>168.1.209.20</code></td>
          </tr>
         </tbody>
       </table>
</p>

  5. Para firewalls privados, permita os intervalos de IP privado da infraestrutura apropriada do IBM Cloud (SoftLayer). Consulte [este link](https://knowledgelayer.softlayer.com/faq/what-ip-ranges-do-i-allow-through-firewall) iniciando com a seção **Rede de backend (privada)**.
      - Inclua todos os [locais dentro das regiões](cs_regions.html#locations) que você está usando.
      - Observe que se deve incluir o local de dal01 (data center).
      - Abra as portas 80 e 443 para permitir o processo de autoinicialização do cluster.

  6. {: #pvc}Para criar solicitações de volume persistentes para armazenamento de dados, permita acesso de egresso por meio de seu firewall para [endereços IP da infraestrutura do IBM Cloud (SoftLayer)](https://knowledgelayer.softlayer.com/faq/what-ip-ranges-do-i-allow-through-firewall) do local (data center) em que seu cluster está.
      - Para localizar o local (data center) de seu cluster, execute `bx cs clusters`.
      - Permita acesso ao intervalo de IP para a **Rede frontend (pública)** e a **Rede backend (privada)**.
      - Observe que se deve incluir o local de dal01 (data center) para a **Rede backend (privada)**.

<br />


## Acessando o NodePort, o balanceador de carga e os serviços do Ingress de fora do cluster
{: #firewall_inbound}

É possível permitir acesso de entrada para o NodePort, o balanceador de carga e os serviços do Ingress.
{:shortdesc}

<dl>
  <dt>Serviço NodePort</dt>
  <dd>Abra a porta que você configurou quando implementou o serviço aos endereços IP públicos para todos os nós do trabalhador para permitir o tráfego. Para localizar a porta, execute `kubectl get svc`. A porta está no intervalo de 20000 a 32000.<dd>
  <dt>Serviço LoadBalancer</dt>
  <dd>Abra a porta que você configurou quando implementou o serviço para o endereço IP público do serviço do balanceador de carga.</dd>
  <dt>Entrada</dt>
  <dd>Abra a porta 80 para HTTP ou a porta 443 para HTTPS para o endereço IP para o balanceador de carga do aplicativo Ingress.</dd>
</dl>
