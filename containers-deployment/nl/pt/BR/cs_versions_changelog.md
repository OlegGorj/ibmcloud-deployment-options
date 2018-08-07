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



# Log de mudanças da versão
{: #changelog}

Visualize informações de mudanças de versão para atualizações principais, secundárias e de correção que estão disponíveis para os clusters do Kubernetes do {{site.data.keyword.containerlong}}. As mudanças incluem atualizações para o Kubernetes e componentes do Provedor do {{site.data.keyword.Bluemix_notm}}. 
{:shortdesc}

A IBM aplica atualizações no nível da correção ao seu nó principal automaticamente, mas deve-se [atualizar seus nós do trabalhador](cs_cluster_update.html#worker_node). Verifique mensal para atualizações disponíveis. Conforme as atualizações se tornam disponíveis, você será notificado ao visualizar informações sobre os nós do trabalhador, como com os `bx cs workers <cluster>` ou `bx cs worker-get <cluster> <worker>`.

Para obter um resumo das ações de migração, veja [Versões do Kubernetes](cs_versions.html).
{: tip}

Para obter informações sobre mudanças desde a versão anterior, veja os logs de mudanças a seguir.
-  Version 1.10 [changelog](#110_changelog).
-  Version 1.9 [changelog](#19_changelog).
-  [Log de mudanças](#18_changelog) da Versão 1.8.
-  **Descontinuado**: Version 1.7 [changelog](#17_changelog).

## Version 1.10 log
{: #110_changelog}

Revise as mudanças a seguir.

### Log de mudanças para o fix pack 1.10.1_1510 do nó do trabalhador, liberado em 18 de maio de 2018
{: #1101_1510}

<table summary="Mudanças que foram feitas desde a versão 1.10.1_1509">
<caption>Mudanças desde a versão 1.10.1_1509</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Correção para tratar de um erro ocorrido se você tiver usado o plug-in de armazenamento de bloco.</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack 1.10.1_1509 do nó do trabalhador, liberado em 16 de maio de 2018
{: #1101_1509}

<table summary="Mudanças que foram feitas desde a versão 1.10.1_1508">
<caption>Mudanças desde a versão 1.10.1_1508</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Os dados armazenados no diretório-raiz `kubelet` agora são salvos no disco secundário maior da máquina do nó do trabalhador.</td>
</tr>
</tbody>
</table>

### Log de mudanças para 1.10.1_1508, liberado em 1º de maio de 2018
{: #1101_1508}

<table summary="Mudanças que foram feitas desde a versão 1.9.7_1510">
<caption>Mudanças desde a versão 1.9.7_1510</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v2.6.5</td>
<td>V3.1.1</td>
<td>Veja as [notas sobre a liberação do Calico ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.projectcalico.org/v3.1/releases/#v311).</td>
</tr>
<tr>
<td>Kubernetes Heapster</td>
<td>V1.5.0</td>
<td>V1.5.2</td>
<td>Veja as [notas sobre a liberação do Kubernetes Heapster ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/heapster/releases/tag/v1.5.2).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.7</td>
<td>v1.10.1</td>
<td>Veja as [notas sobre a liberação do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.1).</td>
</tr>
<tr>
<td>Configuração do Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Incluído <code>StorageObjectInUseProtection</code> na opção <code>--enable-admission-plugins</code> para o servidor de API do Kubernetes do cluster.</td>
</tr>
<tr>
<td>DNS do Kubernetes</td>
<td>1.14.8</td>
<td>14.1.2010</td>
<td>Veja as [notas sobre a liberação do DNS do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/dns/releases/tag/1.14.10).</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.9.7-102</td>
<td>v1.10.1-52</td>
<td>Atualizado para suportar Kubernetes 1,10 release.</td>
</tr>
<tr>
<td>Suporte de GPU</td>
<td>N/A</td>
<td>N/A</td>
<td>O suporte para as [cargas de trabalho do contêiner de unidade de processamento de gráfico (GPU)](cs_app.html#gpu_app) está agora disponível para planejamento e execução. Para obter uma lista de tipos de máquina de GPU disponíveis, veja [Hardware para nós do trabalhador](cs_clusters.html#shared_dedicated_node). Para obter mais informações, veja a documentação do Kubernetes para [Planejar GPUs ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/).</td>
</tr>
</tbody>
</table>

## Version 1.9 log
{: #19_changelog}

Revise as mudanças a seguir.

### Log de mudanças para o fix pack 1.9.7_1512 do nó do trabalhador, liberado em 18 de maio de 2018
{: #197_1512}

<table summary="Mudanças que foram feitas desde a versão 1.9.7_1511">
<caption>Mudanças desde a versão 1.9.7_1511</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Correção para tratar de um erro ocorrido se você tiver usado o plug-in de armazenamento de bloco.</td>
</tr>
</tbody>
</table>

### Log de mudanças para o fix pack 1.9.7_1511 nó do trabalhador, liberado em 16 de maio de 2018
{: #197_1511}

<table summary="Mudanças que foram feitas desde a versão 1.9.7_1510">
<caption>Mudanças desde a versão 1.9.7_1510</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Os dados armazenados no diretório-raiz `kubelet` agora são salvos no disco secundário maior da máquina do nó do trabalhador.</td>
</tr>
</tbody>
</table>

### Log de mudanças para 1.9.7_1510, liberado em 30 de abril de 2018
{: #197_1510}

<table summary="Mudanças que foram feitas desde a versão 1.9.3_1506">
<caption>Mudanças desde a versão 1.9.3_1506</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>V1.9.3</td>
<td>v1.9.7	</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.7). Essa liberação trata das vulnerabilidades [CVE-2017-1002101 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) e [CVE-2017-1002102 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</td>
</tr>
<tr>
<td>Configuração do Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Incluído `admissionregistration.k8s.io/v1alpha1=true` na opção `--runtime-config` para o servidor de API do Kubernetes do cluster.</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.9.3-71</td>
<td>v1.9.7-102</td>
<td>Os serviços `NodePort` e `LoadBalancer` agora suportam [preservar o IP de origem do cliente](cs_loadbalancer.html#node_affinity_tolerations) configurando `service.spec.externalTrafficPolicy` como `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Corrija a configuração de tolerância do [nó de borda](cs_edge.html#edge) para clusters mais velhos.</td>
</tr>
</tbody>
</table>

## Log de mudanças da Versão 1.8
{: #18_changelog}

Revise as mudanças a seguir.

### Log de mudanças para o fix pack 1.8.11_1511 do nó do trabalhador, liberado em 18 de maio de 2018
{: #1811_1511}

<table summary="Mudanças que foram feitas desde a versão 1.8.11_1510">
<caption>Mudanças desde a versão 1.8.11_1510</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Correção para tratar de um erro ocorrido se você tiver usado o plug-in de armazenamento de bloco.</td>
</tr>
</tbody>
</table>

### Mudanças de log para o fix pack 1.8.11_1510 do nó do trabalhador, liberado em 16 de maio de 2018
{: #1811_1510}

<table summary="Mudanças que foram feitas desde a versão 1.8.11_1509">
<caption>Mudanças desde a versão 1.8.11_1509</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Os dados armazenados no diretório-raiz `kubelet` agora são salvos no disco secundário maior da máquina do nó do trabalhador.</td>
</tr>
</tbody>
</table>


### Log de mudanças para 1.8.11_1509, liberado em 19 de abril de 2018
{: #1811_1509}

<table summary="Mudanças que foram feitas desde a versão 1.8.8_1507">
<caption>Mudanças desde a versão 1.8.8_1507</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.8.8</td>
<td>v1.8.11	</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.11). Essa liberação trata das vulnerabilidades [CVE-2017-1002101 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) e [CVE-2017-1002102 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</td>
</tr>
<tr>
<td>Pause a imagem do contêiner</td>
<td>3.0</td>
<td>3.1</td>
<td>Remove processos zumbi órfãos herdados.</td>
</tr>
<tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.8.8-86</td>
<td>v1.8.11-126</td>
<td>Os serviços `NodePort` e `LoadBalancer` agora suportam [preservar o IP de origem do cliente](cs_loadbalancer.html#node_affinity_tolerations) configurando `service.spec.externalTrafficPolicy` como `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Corrija a configuração de tolerância do [nó de borda](cs_edge.html#edge) para clusters mais velhos.</td>
</tr>
</tbody>
</table>

## Archive
{: #changelog_archive}

### Version 1.7 log (Descontinuado)
{: #17_changelog}

Revise as mudanças a seguir.

#### Log de mudanças para o fix pack 1.7.16_1513 do nó do trabalhador, liberado em 18 de maio de 2018
{: #1716_1513}

<table summary="Mudanças que foram feitas desde a versão 1.7.16_1512">
<caption>Mudanças desde a versão 1.7.16_1512</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Correção para tratar de um erro ocorrido se você tiver usado o plug-in de armazenamento de bloco.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para o fix pack 1.7.16_1512 do nó do trabalhador, liberado em 16 de maio de 2018
{: #1716_1512}

<table summary="Mudanças que foram feitas desde a versão 1.7.16_1511">
<caption>Mudanças desde a versão 1.7.16_1511</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Os dados armazenados no diretório-raiz `kubelet` agora são salvos no disco secundário maior da máquina do nó do trabalhador.</td>
</tr>
</tbody>
</table>

#### Log de mudanças para 1.7.16_1511, liberado em 19 de abril de 2018
{: #1716_1511}

<table summary="Mudanças que foram feitas desde a versão 1.7.4_1509">
<caption>Mudanças desde a versão 1.7.4_1509</caption>
<thead>
<tr>
<th>Componente</th>
<th>Prévio</th>
<th>Atual</th>
<th>Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.7.4</td>
<td>v1.7.16	</td>
<td>Veja as [Notas sobre a liberação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.7.16). Essa liberação trata das vulnerabilidades [CVE-2017-1002101 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) e [CVE-2017-1002102 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</td>
</tr>
<td>Provedor do {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.7.4-133</td>
<td>v1.7.16-17</td>
<td>Os serviços `NodePort` e `LoadBalancer` agora suportam [preservar o IP de origem do cliente](cs_loadbalancer.html#node_affinity_tolerations) configurando `service.spec.externalTrafficPolicy` como `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Corrija a configuração de tolerância do [nó de borda](cs_edge.html#edge) para clusters mais velhos.</td>
</tr>
</tbody>
</table>
