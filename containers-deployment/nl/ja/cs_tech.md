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



# {{site.data.keyword.containerlong_notm}} テクノロジー

{{site.data.keyword.containerlong}} を支えるテクノロジーについて詳しく説明します。
{:shortdesc}

## Docker コンテナー
{: #docker_containers}

既存の Linux コンテナー (LXC) テクノロジーに基づき構築された、Docker というオープン・ソース・プロジェクトは、アプリを素早く作成、テスト、デプロイ、拡張するために使用できるソフトウェア・プラットフォームとなっています。 Docker は、アプリの実行に必要なすべてのエレメントを含んだコンテナーと呼ばれる標準化されたユニットに、ソフトウェアをパッケージします。
{:shortdesc}

Docker の基本概念についての説明:

<dl>
<dt>イメージ</dt>
<dd>Docker イメージは Dockerfile から構築されます。Dockerfile は、イメージを構築する方法や、どのビルド成果物 (アプリ、アプリの構成、その従属関係など) を含めるかを定義するテキスト・ファイルです。 イメージは必ず他のイメージから構築されるので、迅速に構成できます。 イメージの処理の大部分の作業は他の人にまかせ、そのイメージを自分の用途に合わせて微調整します。</dd>
<dt>レジストリー</dt>
<dd>イメージ・レジストリーは Docker イメージを格納、取得、共有する場所です。 レジストリーに格納されたイメージは、だれでもアクセスできるものもありますし (パブリック・レジストリー)、小さなグループのユーザーにアクセスを限定したものもあります (プライベート・レジストリー)。 {{site.data.keyword.containershort_notm}} では、最初のコンテナー化アプリを作成するために使用できる ibmliberty などのパブリック・イメージを提供しています。 エンタープライズ・アプリケーションを作成する場合は、無許可ユーザーが勝手にイメージを使用することがないように、{{site.data.keyword.Bluemix_notm}} で提供されているものなどのプライベート・レジストリーを使用してください。
</dd>
<dt>コンテナー</dt>
<dd>すべてのコンテナーはイメージから作成されます。 コンテナーはアプリとそのすべての従属物をパッケージ化しているので、アプリを変更せずに環境間で移動して実行できます。 仮想マシンとは異なり、コンテナーはデバイス、そのオペレーティング・システム、およびその基礎となるハードウェアを仮想化しません。 アプリのコード、ランタイム、システム・ツール、ライブラリー、設定値のみがコンテナー内にパッケージ化されます。 コンテナーは、Ubuntu コンピュート・ホスト上で分離したプロセスとして実行され、ホストのオペレーティング・システムとそのハードウェア・リソースを共有します。 このため、コンテナーは仮想マシンより軽量で移植しやすく、効率的です。</dd>
</dl>



### コンテナーを使用する主な利点
{: #container_benefits}

<dl>
<dt>コンテナーは迅速</dt>
<dd>コンテナーは、開発デプロイメントおよび実動デプロイメントのための、標準化された環境を提供することにより、システム管理を簡素化します。 軽量ランタイムにより、デプロイメントの素早いスケールアップやスケールダウンが可能です。 コンテナーを使用すると、どのインフラストラクチャーでもすべてのアプリを素早く確実にデプロイして実行できるので、さまざまなオペレーティング・システム・プラットフォームとその基礎となるインフラストラクチャーを管理するという複雑さがなくなります。</dd>
<dt>コンテナーは小さい</dt>
<dd>1 つの仮想マシンで必要とされる量のスペースに、多数のコンテナーを入れることができます。</dd>
<dt>コンテナーはポータブル</dt>
<dd>
<ul>
  <li>イメージの断片を再利用してコンテナーを構築します。 </li>
  <li>ステージング環境から実稼働環境に素早くアプリのコードを移動します。</li>
  <li>継続的デリバリー・ツールでプロセスを自動化します。</li>
  </ul>
  </dd>

<p>コンテナー・イメージを使用する際の[個人情報の保護](cs_secure.html#pi)の詳細を確認してください。</p>

<p>Docker に関する知識をさらに深める準備ができましたか? <a href="https://developer.ibm.com/courses/all/docker-essentials-extend-your-apps-with-containers/" target="_blank">このコースを受講して、Docker と {{site.data.keyword.containershort_notm}} が連携する仕組みを学習しましょう。</a></p>

</dl>

<br />


## Kubernetes クラスター
{: #kubernetes_basics}

<img src="images/certified-kubernetes-resized.png" style="padding-right: 10px;" align="left" alt="このバッジは、IBM Cloud Container Service に対する Kubernetes 認定を示しています。"/>Kubernetes というオープン・ソース・プロジェクトでは、コンテナー化されたインフラストラクチャーの実行と、実動ワークロード、オープン・ソース・コントリビューション、Docker コンテナー管理ツールが組み合わされています。 Kubernetes インフラストラクチャーは、コンテナーを管理するための分離された安全なアプリ・プラットフォームです。ポータブルで拡張性に優れ、フェイルオーバー時の自己修復機能も備えています。
{:shortdesc}

以下の図に示されている、Kubernetes の基本概念の一部について説明します。

![デプロイメントのセットアップ](images/cs_app_tutorial_components1.png)

<dl>
<dt>アカウント</dt>
<dd>アカウントとは、ご使用の {{site.data.keyword.Bluemix_notm}} アカウントを指します。</dd>

<dt>クラスター</dt>
<dd>Kubernetes クラスターは、ワーカー・ノードと呼ばれる 1 つ以上のコンピュート・ホストからなります。 ワーカー・ノードは、クラスター内のすべての Kubernetes リソースを一元的に制御してモニターする Kubernetes マスターによって管理されます。 それで、コンテナー化アプリのリソースをデプロイする際には、Kubernetes マスターが、デプロイメント要件とクラスターで使用できるキャパシティーを考慮して、これらのリソースをデプロイするワーカー・ノードを決定します。 Kubernetes リソースには、サービス、デプロイメント、ポッドが含まれます。</dd>

<dt>サービス</dt>
<dd>サービスとは、ポッドのセットをグループ化し、各ポッドの実際のプライベート IP アドレスを公開せずにそれらのポッドへのネットワーク接続を提供する Kubernetes リソースのことです。 サービスを使用することにより、アプリをクラスター内または公開インターネットで使用可能にできます。
</dd>

<dt>デプロイメント</dt>
<dd>デプロイメントとは、アプリの実行に必要なその他のリソースや機能 (サービス、永続ストレージ、アノテーションなど) に関する情報を指定できる Kubernetes リソースです。 構成 YAML ファイルでデプロイメントを文書化し、それをクラスターに適用します。 Kubernetes マスターがリソースを構成し、使用可能な容量を持つワーカー・ノード上のポッドにコンテナーをデプロイします。
</br></br>
ローリング更新中に追加するポッドの数や、一度に使用不可にできるポッドの数など、アプリの更新戦略を定義します。 ローリング更新の実行時には、デプロイメントによって、更新が動作しているかどうかが確認され、障害が検出されるとロールアウトが停止されます。</dd>

<dt>ポッド</dt>
<dd>クラスターにデプロイされるすべてのコンテナー化アプリは、ポッドと呼ばれる Kubernetes リソースによってデプロイ、実行、管理されます。 ポッドは Kubernetes クラスター内のデプロイ可能な小さいユニットを表し、単一のユニットとして処理される必要があるコンテナーをグループ化するために使用します。 ほとんどの場合、各コンテナーはその独自のポッドにデプロイされます。 ただしアプリでは、コンテナーとその他のヘルパー・コンテナーを 1 つのポッドにデプロイすることによって、同じプライベート IP アドレスを使用してそれらのコンテナーをアドレス指定できるようにする必要がある場合もあります。</dd>

<dt>アプリ</dt>
<dd>アプリとは、完全に機能する 1 つのアプリ全体を指す場合もありますし、アプリのコンポーネントを指す場合もあります。 アプリのコンポーネントは、別々のポッドまたは別々のワーカー・ノードにデプロイできます。</dd>

<p>Kubernetes リソースを処理する際の[個人情報の保護](cs_secure.html#pi)の詳細を確認してください。</p>

<p>Kubernetes に関する知識をさらに深める準備ができましたか?</p>
<ul><li><a href="cs_tutorials.html#cs_cluster_tutorial" target="_blank">クラスターの作成チュートリアルを利用して、用語の理解をさらに深めてください</a>。</li>
<li><a href="https://developer.ibm.com/courses/all/get-started-kubernetes-ibm-cloud-container-service/" target="_blank">このコースを受講して、Kubernetes と {{site.data.keyword.containershort_notm}} が連携する仕組みを学習しましょう。</a></li></ul>


</dl>

<br />


## サービス・アーキテクチャー
{: #architecture}

{{site.data.keyword.containershort_notm}} 上で実行される Kubernetes クラスターでは、コンテナー化アプリは、ワーカー・ノードと呼ばれるコンピュート・ホスト上でホストされます。 具体的には、これらのアプリはポッド内で実行され、ポッドがワーカー・ノード上でホストされます。 ワーカー・ノードは Kubernetes マスターによって管理されます。 Kubernetes マスターとワーカー・ノードは、安全な TLS 証明書と openVPN 接続を使用して相互に通信し、クラスター構成を調整します。
{: shortdesc}

Kubernetes マスターとワーカー・ノードの違いは何ですか? これは良い質問です。

<dl>
  <dt>Kubernetes マスター</dt>
    <dd>Kubernetes マスターは、クラスター内のすべてのコンピュート・リソース、ネットワーク・リソース、ストレージ・リソースを管理します。 Kubernetes マスターは、コンテナー化されたアプリとサービスがクラスター内のワーカー・ノードに均等にデプロイされるようにします。 マスターは、アプリとサービスの構成方法に応じて、アプリの要件を満たせるだけのリソースがあるワーカー・ノードを決定します。</dd>
  <dt>ワーカー・ノード</dt>
    <dd>各ワーカー・ノードは、物理マシン (ベア・メタル) であるか、クラウド環境内の物理ハードウェアで実行される仮想マシンです。 ワーカー・ノードをプロビジョンする際に、そのワーカー・ノード上でホストされるコンテナーで使用できるリソースを決定します。すぐに使用できるように、ワーカー・ノードには、{{site.data.keyword.IBM_notm}} 管理の Docker エンジン、別個のコンピュート・リソース、ネットワーキング、ボリューム・サービスがセットアップされます。 標準装備のセキュリティー機能は、分離機能、リソース管理機能、そしてワーカー・ノードのセキュリティー・コンプライアンスを提供します。</dd>
</dl>

<p>
<figure>
 <img src="images/cs_org_ov.png" alt="{{site.data.keyword.containerlong_notm}} Kubernetes アーキテクチャー">
 <figcaption>{{site.data.keyword.containershort_notm}} アーキテクチャー</figcaption>
</figure>
</p>

{{site.data.keyword.containerlong_notm}} を他の製品やサービスと一緒に使用する方法をご覧になりたいですか? こちらで[統合](cs_integrations.html#integrations)についていくつか紹介しています。


<br />
