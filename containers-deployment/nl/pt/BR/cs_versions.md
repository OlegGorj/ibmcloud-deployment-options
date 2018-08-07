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



# Informações de Versão e ações de atualização
{: #cs_versions}

## Tipos de versão do Kubernetes
{: #version_types}

O {{site.data.keyword.containerlong}} suporta simultaneamente múltiplas versões do Kubernetes. Quando uma versão mais recente (n) é liberada, as versões até 2 atrás (n-2) são suportadas. As versões com mais de 2 atrás do mais recente (n-3) são descontinuadas primeiro e depois não suportadas.
{:shortdesc}

As versões atuais do Kubernetes suportadas são:

- Últimas: 1.10.1
- Padrão: 1.9.7
- Suportada: 8.1.2011

**Versões descontinuadas**: quando os clusters estão em execução em um Kubernetes descontinuado, você tem 30 dias para revisar e atualizar para uma versão suportada do Kubernetes antes que a versão se torne não suportada. Durante o período de descontinuação, é possível executar comandos limitados em seus clusters para incluir trabalhadores, recarregar trabalhadores e atualizar o cluster. Não é possível criar novos clusters na versão descontinuada.

**Versões não suportadas**: se você está executando clusters em uma versão do Kubernetes que não é suportada, [revise potenciais impactos](#version_types) para atualizações e então [atualize imediatamente o cluster](cs_cluster_update.html#update) para continuar recebendo importantes atualizações de segurança e suporte.

Para verificar a versão do servidor de um cluster, execute o comando a seguir.

```
kubectl version  --short | grep -i server
```
{: pre}

Saída de exemplo:

```
Versão do Servidor: v1.9.7+9d6e0610086578
```
{: screen}


## Tipos de atualização
{: #update_types}

Seu cluster do Kubernetes possui três tipos de atualizações: principais, secundárias e de correção.
{:shortdesc}

|Tipo de atualização|Exemplos de rótulos de versão|Atualizado por|Impacto
|-----|-----|-----|-----|
|Principal|1.x.x|Você|Mudanças de operação para clusters, incluindo scripts ou implementações.|
|Menor|x.9.x|Você|Mudanças de operação para clusters, incluindo scripts ou implementações.|
|Correção|x.x.4_1510|IBM e você|Correções do Kubernetes, bem como outras atualizações de componentes do Provedor {{site.data.keyword.Bluemix_notm}}, como correções de segurança e do sistema operacional. A IBM atualiza os mestres automaticamente, mas você aplica correções a nós do trabalhador.|
{: caption="Impactos de atualizações do Kubernetes" caption-side="top"}

Conforme as atualizações se tornam disponíveis, você será notificado ao visualizar informações sobre os nós do trabalhador, como com os `bx cs workers <cluster>` ou `bx cs worker-get <cluster> <worker>`.
-  **Atualizações principais e secundárias**: primeiro, [atualize seu nó principal](cs_cluster_update.html#master) e, em seguida, [atualize o nós do trabalhador](cs_cluster_update.html#worker_node). 
   - Por padrão, não é possível atualizar um mestre do Kubernetes três ou mais versões secundárias à frente. Por exemplo, se o seu mestre atual for versão 1.5 e você desejar atualizar para 1.8, deve-se atualizar para 1.7 primeiro. É possível forçar a atualização para continuar, mas atualizar mais de duas versões secundárias poderá causar resultados inesperados.
   - Se você usa uma versão CLI do `kubectl` que corresponde pelo menos à versão `major.minor` de seus clusters, você pode ter resultados inesperados. Certifique-se de manter seu cluster do Kubernetes e as [versões da CLI](cs_cli_install.html#kubectl) atualizados.
-  **Atualizações de correção**: verifique mensalmente se uma atualização está disponível e use o [comando](cs_cli_reference.html#cs_worker_update) `bx cs worker-update` ou o [comando](cs_cli_reference.html#cs_worker_reload) `bx cs worker-reload` para aplicar essas correções de segurança e do sistema operacional. Para obter mais informações, consulte [Versão do log](cs_versions_changelog.html).

<br/>

Essas informações resumem as atualizações que provavelmente têm impacto em apps implementados ao atualizar um cluster da versão anterior para uma nova versão.
-  Version 1.10 [Ações de migração](#cs_v110).
-  [Ações de migração](#cs_v19) da versão 1.9.
-  [Ações de migração](#cs_v18) da versão 1.8.
-  [Archive](#k8s_version_archive) de versões descontinuadas ou não suportadas.

<br/>

Para obter uma lista completa de mudanças, revise as informações a seguir:
* [Log de mudanças do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md).
* [Log de mudanças de versão da IBM](cs_versions_changelog.html).

## Version 1.10
{: #cs_v110}

<p><img src="images/certified_kubernetes_1x10.png" style="padding-right: 10px;" align="left" alt="Este badge indica a certificação do Kubernetes versão 1.10 para o IBM Cloud Container Service."/> {{site.data.keyword.containerlong_notm}} é um produto Kubernetes certificado para a versão 1.10 sob o programa CNCF Kubernetes Software Conformance Certification. _Kubernetes® é uma marca registrada da Linux Foundation nos Estados Unidos e em outros países e é usada nos termos de uma licença da Linux Foundation._</p>

Revise as mudanças que você poderá precisar fazer quando estiver atualizando da versão anterior do Kubernetes para 1.10.

**Importante**: antes de poder atualizar com êxito para o Kubernetes 1.10, deve-se seguir as etapas listadas em [Preparando para atualizar para o Calico v3](#110_calicov3).

<br/>

### Atualizar antes do mestre
{: #110_before}

<table summary="Kubernetes updates for version 1.10">
<caption>Mudanças para fazer antes de atualizar o mestre para o Kubernetes 1.10</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico v3</td>
<td>Atualizar para o Kubernetes versão 1.10 também atualiza o Calico da v2.6.5 para a v3.1.1. <strong>Importante</strong>: antes de poder atualizar com êxito para o Kubernetes v1.10, deve-se seguir as etapas listadas em [Preparando para atualizar para o Calico v3](#110_calicov3).</td>
</tr>
<tr>
<td>Painel do Kubernetes política de rede</td>
<td>No Kubernetes 1.10, a política de rede <code>kubernetes-dashboard</code> no namespace <code>kube-system</code> bloqueia o acesso de todos os pods ao painel do Kubernetes. No entanto, isso <strong>não</strong> afeta a capacidade de acessar o painel por meio do console do {{site.data.keyword.Bluemix_notm}} ou usando <code>kubectl proxy</code>. Se um pod requer acesso ao painel, é possível incluir um rótulo <code>kubernetes-dashboard-policy: allow</code> em um namespace e, em seguida, implementar o pod no namespace.</td>
</tr>
<tr>
<td>Acesso à API do kubelet</td>
<td>A autorização da API do kubelet agora é delegada para o <code>servidor de API do Kubernetes</code>. O acesso à API do Kubelet é baseado em <code>ClusterRoles</code> que concedem permissão para acessar sub-recursos do <strong>nó</strong>. Por padrão, o Kubernetes Heapster tem <code>ClusterRole</code> e <code>ClusterRoleBinding</code>. No entanto, se a API do Kubelet é usada por outros usuários ou apps, deve-se conceder a eles permissão para usar a API. Consulte a documentação do Kubernetes em [Autorização do Kubelet![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/admin/kubelet-authentication-authorization/#kubelet-authorization).</td>
</tr>
<tr>
<td>Conjuntos de Cifras</td>
<td>Os conjuntos de cifras suportados para o <code>servidor de API do Kubernetes</code> e a API do Kubelet agora são restritos a um subconjunto com criptografia de alta intensidade (128 bits ou mais). Se você tem automação existente ou recursos que usam cifras mais fracas e dependem de se comunicar com o <code>servidor de API do Kubernetes</code> ou a API do Kubelet, ative o suporte de cifra mais forte antes de atualizar o mestre.</td>
</tr>
</tbody>
</table>

### Atualizar depois do mestre
{: #110_after}

<table summary="Kubernetes updates for version 1.10">
<caption>Mudanças para fazer depois de atualizar o mestre para o Kubernetes 1.10</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico v3</td>
<td>Quando o cluster é atualizado, todos os dados existentes do Calico aplicados ao cluster são migrados automaticamente para usar a sintaxe do Calico v3. Para visualizar, incluir ou modificar recursos do Calico com a sintaxe do Calico v3, atualize sua [configuração da CLI do Calico para a versão 3.1.1](#110_calicov3).</td>
</tr>
<tr>
<td>O nó <code>ExternalIP</code> endereço</td>
<td>O campo <code>ExternalIP</code> de um nó é agora configurado para o valor do endereço IP público do nó. Revise e atualize quaisquer recursos que dependem desse valor.</td>
</tr>
<tr>
<td><code>kubectl port-forward</code></td>
<td>Agora, quando você usa o comando <code>kubectl port-forward</code>, ele não suporta mais a sinalização <code>-p</code>. Se seus scripts dependem do comportamento anterior, atualize-os para substituir a sinalização <code>-p</code> pelo nome do pod.</td>
</tr>
<tr>
<td>Volumes de dados de API somente leitura</td>
<td>Agora `secret`, `configMap`, `downwardAPI` e volumes projetados são montados como somente leitura.
Anteriormente, os apps eram autorizados a gravar dados nesses volumes que podiam ser revertidos automaticamente pelo sistema. Essa ação de migração é necessária para corrigir
a vulnerabilidade de segurança [CVE-2017-1002102![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2017-1002102).
Se os seus apps dependerem do comportamento inseguro prévio, modifique-os de modo correspondente.</td>
</tr>
</tbody>
</table>

### Preparando para atualizar para o Calico v3
{: #110_calicov3}

Antes de iniciar, seu cluster mestre e todos os nós do trabalhador devem estar executando o Kubernetes versão 1.8 ou mais recente e devem ter pelo menos um nó do trabalhador.

**Importante**: prepare-se para a atualização do Calico v3 antes de atualizar o mestre. Durante o upgrade do mestre para o Kubernetes v1.10, novos pods e novas políticas de rede do Kubernetes ou Calico não são planejados. A quantia de tempo que a atualização evita novo planejamento varia. Clusters pequenos podem levar alguns minutos, com alguns minutos extras para cada 10 nós. As políticas de rede e os pods existentes continuam a ser executados.

1.  Verifique se os seus pods do Calico estão saudáveis.
    ```
    kubectl get pods -n kube-system -l k8s-app=calico-node -o wide
    ```
    {: pre}
    
2.  Se nenhum pod estiver em um estado **Executando**, exclua o pod e aguarde até que ele esteja em um estado **Executando** antes de continuar.

3.  Se você gerar automaticamente políticas do Calico ou outros recursos do Calico, atualize seu conjunto de ferramentas de automação para gerar esses recursos com a [sintaxe do Calico v3 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/).

4.  Se você usar o [strongSwan](cs_vpn.html#vpn-setup) para a conectividade VPN, o gráfico Helm do strongSwan 2.0.0 não funcionará com o Calico v3 ou Kubernetes 1.10. [Atualize o strongSwan](cs_vpn.html#vpn_upgrade) para o gráfico Helm 2.1.0, que é compatível com versões anteriores do Calico 2.6 e Kubernetes 1.7, 1.8 e 1.9.

5.  [Atualize seu cluster mestre para o Kubernetes v1.10](cs_cluster_update.html#master).

<br />


## Versão 1.9
{: #cs_v19}

<p><img src="images/certified_kubernetes_1x9.png" style="padding-right: 10px;" align="left" alt="Esse badge indica a certificação do Kubernetes versão 1.9 para o IBM Cloud Container Service."/> O {{site.data.keyword.containerlong_notm}} é um produto Kubernetes certificado para a versão 1.9 sob o programa CNCF Kubernetes Software Conformance Certification. _Kubernetes® é uma marca registrada da Linux Foundation nos Estados Unidos e em outros países e é usada nos termos de uma licença da Linux Foundation._</p>

Revise as mudanças que você poderá precisar fazer quando estiver atualizando da versão anterior do Kubernetes para 1.9.

<br/>

### Atualizar antes do mestre
{: #19_before}

<table summary="Atualizações do Kubernetes para a versão 1.9">
<caption>Mudanças a serem feitas antes de atualizar o mestre para o Kubernetes 1.9</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>API de admissão do webhook</td>
<td>A API de admissão, que é usada quando o servidor de API chama webhooks de controle de admissão, é movida de <code>admission.v1alpha1</code> para <code>admission.v1beta1</code>. <em>Deve-se excluir quaisquer webhooks existentes antes de fazer upgrade de seu cluster</em> e atualizar os arquivos de configuração de webhook para usar a API mais recente. Essa mudança não é compatível com versões anteriores.</td>
</tr>
</tbody>
</table>

### Atualizar depois do mestre
{: #19_after}

<table summary="Atualizações do Kubernetes para a versão 1.9">
<caption>Mudanças a serem feitas depois de atualizar o mestre para o Kubernetes 1.9</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Saída `kubectl`</td>
<td>Agora, quando o comando `kubectl` é usado para especificar `-o custom-columns` e a coluna não é localizada no objeto, você vê uma saída de `<none>`.<br>
Anteriormente, a operação falhava e você via a mensagem de erro `xxx is not found`. Se seus scripts dependem do comportamento anterior, atualize-os.</td>
</tr>
<tr>
<td>`kubectl patch`</td>
<td>Agora, quando nenhuma mudança é feita no recurso que é corrigido, o comando `kubectl patch` falha com `exit code 1`. Se seus scripts dependem do comportamento anterior, atualize-os.</td>
</tr>
<tr>
<td>Permissões de painel do Kubernetes</td>
<td>Os usuários precisam efetuar login no painel do Kubernetes com suas credenciais para visualizar recursos de cluster. A autorização RBAC `ClusterRoleBinding` do painel padrão do Kubernetes foi removida. Para obter instruções, veja [Ativando o painel do Kubernetes](cs_app.html#cli_dashboard).</td>
</tr>
<tr>
<td>Volumes de dados de API somente leitura</td>
<td>Agora `secret`, `configMap`, `downwardAPI` e volumes projetados são montados como somente leitura.
Anteriormente, os apps eram autorizados a gravar dados nesses volumes que podiam ser revertidos automaticamente pelo sistema. Esta ação de migração é necessária para corrigir
a vulnerabilidade de segurança [CVE-2017-1002102](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2017-1002102).
Se os seus apps dependerem do comportamento inseguro prévio, modifique-os de modo correspondente.</td>
</tr>
<tr>
<td>Contaminações e tolerâncias</td>
<td>As contaminações `node.alpha.kubernetes.io/notReady` e `node.alpha.kubernetes.io/unreachable` foram mudadas para `node.kubernetes.io/not-ready` e `node.kubernetes.io/unreachable`, respectivamente.<br>
Embora as contaminações sejam atualizadas automaticamente, deve-se atualizar manualmente as tolerâncias para essas contaminações. Para cada namespace, exceto `ibm-system` e `kube-system`, determine se é necessário mudar as tolerâncias:<br>
<ul><li><code>kubectl get pods -n &lt;namespace&gt; -o yaml | grep "node.alpha.kubernetes.io/notReady" && echo "Action required"</code></li><li>
<code>kubectl get pods -n &lt;namespace&gt; -o yaml | grep "node.alpha.kubernetes.io/unreachable" && echo "Action required"</code></li></ul><br>
Se `Action required` for retornado, modifique as tolerâncias de pod de forma adequada.</td>
</tr>
<tr>
<td>API de admissão do webhook</td>
<td>Se você excluiu webhooks existente antes de atualizar o cluster, crie novos webhooks.</td>
</tr>
</tbody>
</table>

<br />



## Versão 1.8
{: #cs_v18}

<p><img src="images/certified_kubernetes_1x8.png" style="padding-right: 10px;" align="left" alt="Esse badge indica a certificação do Kubernetes versão 1.8 para o IBM Cloud Container Service."/> O {{site.data.keyword.containerlong_notm}} é um produto Kubernetes certificado para a versão 1.8 sob o programa CNCF Kubernetes Software Conformance Certification. _Kubernetes® é uma marca registrada da Linux Foundation nos Estados Unidos e em outros países e é usada nos termos de uma licença da Linux Foundation._</p>

Revise as mudanças que você poderá precisar fazer quando estiver atualizando da versão anterior do Kubernetes para 1.8.

<br/>

### Atualizar antes do mestre
{: #18_before}

<table summary="Atualizações do Kubernetes para versões 1.8">
<caption>Mudanças para fazer antes de atualizar o mestre para o Kubernetes 1.8</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Nenhuma</td>
<td>Nenhuma mudança é necessária antes de atualizar o mestre</td>
</tr>
</tbody>
</table>

### Atualizar depois do mestre
{: #18_after}

<table summary="Atualizações do Kubernetes para versões 1.8">
<caption>Mudanças para fazer depois de atualizar o mestre para o Kubernetes 1.8</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Login do painel do Kubernetes</td>
<td>A URL para acessar o painel do Kubernetes na versão 1.8 mudou e o processo de login inclui uma nova etapa de autenticação. Para obter mais informações, veja [acessando o painel do Kubernetes](cs_app.html#cli_dashboard).</td>
</tr>
<tr>
<td>Permissões de painel do Kubernetes</td>
<td>Para forçar os usuários a efetuar login com suas credenciais para visualizar recursos de cluster na versão 1.8, remova a autorização RBAC do ClusterRoleBinding 1.7. Execute `kubectl delete clusterrolebinding -n kube-system kubernetes-dashboard`.</td>
</tr>
<tr>
<td>`kubectl delete`</td>
<td>O comando `kubectl delete` não reduz mais os objetos da API de carga de trabalho, como pods, antes de o objeto ser excluído. Se você requer a redução de escala do objeto, use [`kubectl scale` ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/reference/kubectl/overview/#scale) antes de excluir o objeto.</td>
</tr>
<tr>
<td>`kubectl run`</td>
<td>O comando `kubectl run` deve usar múltiplas sinalizações para `--env` em vez de argumentos separados por vírgula. Por exemplo, execute <code>kubectl run --env &lt;x&gt;=&lt;y&gt; --env &lt;z&gt;=&lt;a&gt;</code> e não <code>kubectl run --env &lt;x&gt;=&lt;y&gt;,&lt;z&gt;=&lt;a&gt;</code>. </td>
</tr>
<tr>
<td>`kubectl stop`</td>
<td>O comando `kubectl stop` não está mais disponível.</td>
</tr>
<tr>
<td>Volumes de dados de API somente leitura</td>
<td>Agora `secret`, `configMap`, `downwardAPI` e volumes projetados são montados como somente leitura.
Anteriormente, os apps eram autorizados a gravar dados nesses volumes que podiam ser revertidos automaticamente pelo sistema. Esta ação de migração é necessária para corrigir
a vulnerabilidade de segurança [CVE-2017-1002102](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2017-1002102).
Se os seus apps dependerem do comportamento inseguro prévio, modifique-os de modo correspondente.</td>
</tr>
</tbody>
</table>

<br />



## Archive
{: #k8s_version_archive}

### Version 1.7 (Reprovado)
{: #cs_v17}

**Desde 22 de maio de 2018, os clusters do {{site.data.keyword.containershort_notm}} que executam o Kubernetes versão 1.7 estão descontinuados**. Depois de 21 de junho de 2018, os clusters da Versão 1.7 não podem receber atualizações de segurança ou suporte, a menos que sejam atualizados para a próxima versão mais recente ([Kubernetes 1.8](#cs_v18)).

[Revise o potencial impacto](cs_versions.html#cs_versions) de cada atualização de versão do Kubernetes e depois [atualize seus clusters](cs_cluster_update.html#update) imediatamente.

Você ainda está executando o Kubernetes versão 1.5? Revise as informações a seguir para avaliar o impacto de atualizar seu cluster da v1.5 para a v1.7. [Atualize seus clusters](cs_cluster_update.html#update) para a v1.7, em seguida, atualize-os imediatamente para pelo menos v1.8.
{: tip}

<p><img src="images/certified_kubernetes_1x7.png" style="padding-right: 10px;" align="left" alt="Esse badge indica a certificação do Kubernetes versão 1.7 para o IBM Cloud Container Service."/> O {{site.data.keyword.containerlong_notm}} é um produto Kubernetes certificado para a versão 1.7 sob o programa CNCF Kubernetes Software Conformance Certification.</p>

Revise as mudanças que você poderá precisar fazer quando estiver atualizando da versão anterior do Kubernetes para 1.7.

<br/>

#### Atualizar antes do mestre
{: #17_before}

<table summary="Atualizações do Kubernetes para as versões 1.7 e 1.6">
<caption>Mudanças para fazer antes de atualizar o mestre para o Kubernetes 1.7</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Armazenamento</td>
<td>Os scripts de configuração com `hostPath` e `mountPath` com o referência de diretório-pai como `../to/dir` não são permitidos. Mude os caminhos para caminhos absolutos simples, por exemplo, `/path/to/dir`.
<ol>
  <li>Determine se você precisa mudar os caminhos de armazenamento:</br>
  ```
  kubectl get pods --all-namespaces -o yaml | grep "\.\." && echo "Action required"
  ```
  </br>

  <li>Se `Ação necessária` é retornado, mude o pods para referenciar o caminho absoluto antes de atualizar todos os nós do trabalhador. Se o pod é de propriedade de outro recurso, como uma implementação, mude o [_PodSpec_ ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/api-reference/v1.7/#podspec-v1-core) dentro desse recurso.
</ol>
</td>
</tr>
</tbody>
</table>

#### Atualizar depois do mestre
{: #17_after}

<table summary="Atualizações do Kubernetes para as versões 1.7 e 1.6">
<caption>Mudanças para fazer depois de atualizar o mestre para o Kubernetes 1.7</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>`apiVersion` de implementação</td>
<td>Depois de atualizar o cluster do Kubernetes 1.5, use `apps/v1beta1` para o campo `apiVersion` em novos arquivos YAML de `Deployment`. Continue a usar `extensions/v1beta1` para outros recursos, como `Ingress`.</td>
</tr>
<tr>
<td>'kubectl'</td>
<td>Após a atualização da CLI `kubectl`, estes comandos `kubectl create` devem usar múltiplas sinalizações em vez de argumentos separados por vírgula:<ul>
 <li>`Função`
 <li>`clusterrole`
 <li>`rolebinding`
 <li>`clusterrolebinding`
 <li>`secret`
 </ul>
</br>  Por exemplo, execute `kubectl create role --resource-name <x> --resource-name <y>` e não `kubectl create role --resource-name <x>,<y>`.</td>
</tr>
<tr>
<td>Network Policy</td>
<td>A anotação `net.beta.kubernetes.io/network-policy` não está mais disponível.
<ol>
  <li>Determine se você precisa mudar as políticas:</br>
  ```
  kubectl get ns -o yaml | grep "net.beta.kubernetes.io/network-policy" | grep "DefaultDeny" && echo "Action required"
  ```
  <li>Se `"Action required"` for retornado, inclua a política de rede a seguir em cada namespace do Kubernetes que foi listado:</br>

  <pre class="codeblock">
  <code>
  kubectl create -n &lt;namespace&gt; -f - &lt;&lt;EOF
  kind: NetworkPolicy
  apiVersion: networking.k8s.io/v1
  metadados:
    name: default-deny
    namespace: &lt;namespace&gt;
  spec:
    podSelector: {}
  EOF
  </code>
  </pre>

  <li> Depois de incluir a política de rede, remova a anotação `net.beta.kubernetes.io/network-policy`:
  ```
  kubectl annotate ns <namespace> --overwrite "net.beta.kubernetes.io/network-policy-"
  ```
  </li></ol>
</td></tr>
<tr>
<td>Planejamento de afinidade de Pod</td>
<td> A anotação `scheduler.alpha.kubernetes.io/affinity` está descontinuada.
<ol>
  <li>Para cada namespace, exceto `ibm-system` e `kube-system`, determine se você precisa atualizar o planejamento de afinidade de pod:</br>
  ```
  kubectl get pods -n <namespace> -o yaml | grep "scheduler.alpha.kubernetes.io/affinity" && echo "Action required"
  ```
  </br></li>
  <li>Se `"Action required"` for retornado, use o campo [_PodSpec_ ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/api-reference/v1.7/#podspec-v1-core) _affinity_ em vez da anotação `scheduler.alpha.kubernetes.io/affinity`.</li>
</ol>
</td></tr>
<tr>
<td>RBAC para `padrão` `ServiceAccount`</td>
<td><p>O `ClusterRoleBinding` administrador para a `default` `ServiceAccount` no namespace `default` é removido para melhorar a segurança do cluster. Os aplicativos executados no namespace `default` não têm mais privilégios de administrador de cluster para a API do Kubernetes e podem encontrar erros de permissão `RBAC DENY`. Verifique seu app e seu arquivo `.yaml` para ver se ele é executado no namespace `default`, se usa a `default ServiceAccount` e acessa a API do Kubernetes.</p>
<p>Se os seus aplicativos dependerem desses privilégios, [crie os recursos de autorização RBAC![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview) para seus apps.</p>
  <p>Conforme atualiza suas políticas de RBAC do app, você pode desejar reverter temporariamente para o `padrão` anterior. Copie, salve e aplique os arquivos a seguir com o comando `kubectl apply -f FILENAME`. <strong>Nota</strong>: reverter para se dar tempo para atualizar todos os recursos de aplicativos e não como uma solução a longo prazo.</p>

<p><pre class="codeblock">
<code>
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1beta1
metadados:
 name: admin-binding-nonResourceURLSs-default
subjects:
  - kind: ServiceAccount
    name: default
    namespace: default
roleRef:
 kind: ClusterRole
 name: admin-role-nonResourceURLSs
 apiGroup: rbac.authorization.k8s.io
---
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
 name: admin-binding-resourceURLSs-default
subjects:
  - kind: ServiceAccount
      name: default
      namespace: default
  roleRef:
   kind: ClusterRole
   name: admin-role-resourceURLSs
   apiGroup: rbac.authorization.k8s.io
</code>
</pre></p>
</td>
</tr>
<tr>
<td>Volumes de dados de API somente leitura</td>
<td>Agora `secret`, `configMap`, `downwardAPI` e volumes projetados são montados como somente leitura.
Anteriormente, os apps eram autorizados a gravar dados nesses volumes que podiam ser revertidos automaticamente pelo sistema. Esta ação de migração é necessária para corrigir
a vulnerabilidade de segurança [CVE-2017-1002102](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2017-1002102).
Se os seus apps dependerem do comportamento inseguro prévio, modifique-os de modo correspondente.</td>
</tr>
<tr>
<td>DNS do pod StatefulSet</td>
<td>Os pods StatefulSet perdem suas entradas DNS do Kubernetes após o mestre ser atualizado. Para restaurar as entradas DNS, exclua os pods StatefulSet. O Kubernetes recria os pods e restaura automaticamente as entradas DNS. Para obter mais informações, veja [Problema do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/issues/48327).</td>
</tr>
<tr>
<td>Tolerations</td>
<td>A anotação `scheduler.alpha.kubernetes.io/tolerations` não está mais disponível.
<ol>
  <li>Para cada namespace, exceto `ibm-system` e `kube-system`, determine se é necessário mudar as tolerâncias:</br>
  ```
  kubectl get pods -n <namespace> -o yaml | grep "scheduler.alpha.kubernetes.io/tolerations" && echo "Action required"
  ```
  </br>

  <li>Se `"Action required"` for retornado, use o campo [_PodSpec_ ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/api-reference/v1.7/#podspec-v1-core) _tolerations_ em vez da anotação `scheduler.alpha.kubernetes.io/tolerations`
</ol>
</td></tr>
<tr>
<td>Taints</td>
<td>A anotação `scheduler.alpha.kubernetes.io/taints` não está mais disponível.
<ol>
  <li>Determine se você precisa mudar as contaminações:</br>
  ```
  kubectl get nodes -o yaml | grep "scheduler.alpha.kubernetes.io/taints" && echo "Action required"
  ```
  <li>Se `"Action required"` é retornado, remova a anotação `scheduler.alpha.kubernetes.io/taints` para cada nó:</br>
  `kubectl annotate nodes <node> scheduler.alpha.kubernetes.io/taints-`
  <li>Inclua uma contaminação em cada nó:</br>
  `kubectl taint node <node> <taint>`
  </li></ol>
</td></tr>
</tbody>
</table>

<br />


### Versão 1.5 (Não suportada)
{: #cs_v1-5}

A partir de 4 de abril de 2018, os clusters do {{site.data.keyword.containershort_notm}} que executam o [Kubernetes versão 1.5](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.5.md) são não suportados. Os clusters da versão 1.5 não podem receber atualizações de segurança ou suporte, a menos que sejam atualizados para a próxima versão mais recente ([Kubernetes 1.7](#cs_v17)).

[Revise o potencial impacto](cs_versions.html#cs_versions) de cada atualização de versão do Kubernetes e depois [atualize seus clusters](cs_cluster_update.html#update) imediatamente. Deve-se atualizar de uma versão para a próxima mais recente, como 1.5 para 1.7 ou 1.8 para 1.9.
