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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# Resolução de problemas de criação de log e monitoramento
{: #cs_troubleshoot_health}

Ao usar o {{site.data.keyword.containerlong}}, considere estas técnicas para resolução de problemas com a criação de log e o monitoramento.
{: shortdesc}

Se você tiver um problema mais geral, tente a [depuração do cluster](cs_troubleshoot.html).
{: tip}

## Os logs não aparecem
{: #cs_no_logs}

{: tsSymptoms}
Quando você acessa o painel do Kibana, seus logs não são exibidos.

{: tsResolve}
Revise as razões a seguir para saber o motivo de os logs do cluster não aparecerem e as etapas de resolução de problemas correspondentes:

<table>
<caption>Resolução de logs que não exibem</caption>
  <col width="40%">
  <col width="60%">
  <thead>
    <tr>
      <th>Por que isso está acontecendo?</th>
      <th>Como corrigi-lo</th>
    </tr>
 </thead>
 <tbody>
  <tr>
    <td>Nenhuma configuração de criação de log está definida.</td>
    <td>Para que os logs sejam enviados, deve-se criar uma configuração de criação de log. Para fazer isso, veja <a href="cs_health.html#logging">Configurando a criação de log do cluster</a>.</td>
  </tr>
  <tr>
    <td>O cluster não está em um estado <code>Normal</code>.</td>
    <td>Para verificar o estado do seu cluster, veja <a href="cs_troubleshoot.html#debug_clusters">Depurando clusters</a>.</td>
  </tr>
  <tr>
    <td>A cota de armazenamento do log foi atendida.</td>
    <td>Para aumentar os seus limites de armazenamento de log, veja a <a href="/docs/services/CloudLogAnalysis/troubleshooting/error_msgs.html"> documentação do {{site.data.keyword.loganalysislong_notm}}</a>.</td>
  </tr>
  <tr>
    <td>Se você especificou um espaço na criação do cluster, o proprietário da conta não tem permissões de Gerenciador, Desenvolvedor ou Auditor para esse espaço.</td>
      <td>Para mudar permissões de acesso para o proprietário da conta:
      <ol><li>Para descobrir quem é o proprietário da conta para o cluster, execute <code>bx cs api-key-info &lt;cluster_name_or_ID&gt;</code>.</li>
      <li>Para conceder a esse proprietário da conta as permissões de acesso de Gerenciador, Desenvolvedor ou Auditor do {{site.data.keyword.containershort_notm}} para o espaço, veja <a href="cs_users.html">Gerenciando o acesso ao cluster</a>.</li>
      <li>Para atualizar o token de criação de log após as permissões serem mudadas, execute <code>bx cs logging-config-refresh &lt;cluster_name_or_ID&gt;</code>.</li></ol></td>
    </tr>
    <tr>
      <td>Você tem uma configuração de criação de log do aplicativo com um link simbólico no caminho do app.</td>
      <td><p>Para que os logs sejam enviados, deve-se usar um caminho absoluto na configuração de criação de log ou os logs não poderão ser lidos. Se o seu caminho estiver montado em seu nó do trabalhador, ele poderá ter criado um link simbólico.</p> <p>Exemplo: se o caminho especificado é <code>/usr/local/<b>spark</b>/work/app-0546/0/stderr</code>, mas os logs vão para <code>/usr/local/<b>spark-1.0-hadoop-1.2</b>/work/app-0546/0/stderr</code>, os logs não podem ser lidos.</p></td>
    </tr>
  </tbody>
</table>

Para testar as mudanças feitas durante a resolução de problemas, é possível implementar *Noisy*, um pod de amostra que produz vários eventos de log, em um nó do trabalhador em seu cluster.

  1. [Direcione sua CLI](cs_cli_install.html#cs_cli_configure) para um cluster no qual você deseja começar a produzir logs.

  2. Crie o arquivo de configuração `deploy-noisy.yaml`.

      ```
      apiVersion: v1
      kind: Pod
      metadata:
        name: noisy
      spec:
        containers:
        - name: noisy
          image: ubuntu:16.04
          command: ["/bin/sh"]
          args: ["-c", "while true; do sleep 10; echo 'Hello world!'; done"]
          imagePullPolicy: "Always"
        ```
        {: codeblock}

  3. Execute o arquivo de configuração no contexto do cluster.

        ```
        Kubectl apply -f ``deploy-noisy.yaml
        ```
        {:pre}

  4. Após alguns minutos, é possível visualizar seus logs no painel do Kibana. Para acessar o painel do Kibana, acesse uma das URLs a seguir e selecione a conta do {{site.data.keyword.Bluemix_notm}} na qual você criou o cluster. Se você especificou um espaço na criação do cluster, acesse esse espaço.
      - Sul e Leste dos EUA: https://logging.ng.bluemix.net
      - Sul do Reino Unido: https://logging.eu-gb.bluemix.net
      - UE Central: https://logging.eu-fra.bluemix.net
      - AP-South: https://logging.au-syd.bluemix.net

<br />


## O painel do Kubernetes não exibe gráficos de utilização
{: #cs_dashboard_graphs}

{: tsSymptoms}
Quando você acessa o painel do Kubernetes, os gráficos de utilização não são exibidos.

{: tsCauses}
Às vezes, após uma atualização de cluster ou reinicialização do nó do trabalhador, o pod `kube-dashboard` não é atualizado.

{: tsResolve}
Exclua o pod `kube-painel` para forçar uma reinicialização. O pod é recriado com políticas RBAC para acessar o heapster para obter informações de utilização.

  ```
  kubectl delete pod -n kube-system $(kubectl get pod -n kube-system --selector=k8s-app=kubernetes-dashboard -o jsonpath='{.items..metadata.name}')
  ```
  {: pre}

<br />


## Obtendo ajuda e suporte
{: #ts_getting_help}

Ainda está tendo problemas com o seu cluster?
{: shortdesc}

-   Para ver se o {{site.data.keyword.Bluemix_notm}} está disponível, [verifique a página de status do {{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/bluemix/support/#status).
-   Poste uma pergunta no [{{site.data.keyword.containershort_notm}} Slack ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://ibm-container-service.slack.com).

    Se você não estiver usando um IBMid para a sua conta do {{site.data.keyword.Bluemix_notm}}, [solicite um convite](https://bxcs-slack-invite.mybluemix.net/) para essa Folga.
    {: tip}
-   Revise os fóruns para ver se outros usuários tiveram o mesmo problema. Ao usar os fóruns para fazer uma pergunta, marque sua pergunta para que ela seja vista pelas equipes de desenvolvimento do {{site.data.keyword.Bluemix_notm}}.

    -   Se você tiver questões técnicas sobre como desenvolver ou implementar clusters ou apps com o {{site.data.keyword.containershort_notm}}, poste sua pergunta no [Stack Overflow ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) e identifique a sua pergunta com `ibm-cloud`, `kubernetes` e `containers`.
    -   Para perguntas sobre o serviço e instruções de introdução, use o fórum [IBM developerWorks dW Answers ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix). Inclua as tags `ibm-cloud` e `containers`.
    Consulte [Obtendo ajuda](/docs/get-support/howtogetsupport.html#using-avatar) para obter mais detalhes sobre o uso dos fóruns.

-   Entre em contato com o Suporte IBM abrindo um chamado. Para saber como abrir um chamado de suporte IBM ou sobre os níveis de suporte e as severidades de chamado, veja [Entrando em contato com o suporte](/docs/get-support/howtogetsupport.html#getting-customer-support).

{: tip}
Ao relatar um problema, inclua o ID do cluster. Para obter o ID do cluster, execute `bx cs clusters`.

