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



# {{site.data.keyword.containerlong_notm}} 概説
{: #container_index}

Kubernetes クラスターで稼動する Docker コンテナーに可用性の高いアプリをデプロイすることによって、{{site.data.keyword.containerlong}} ですぐに作業を始めることができます。
{:shortdesc}

コンテナーは、環境間でアプリをシームレスに移動できるようにアプリとそのすべての従属物をパッケージ化するための標準的な手段です。 仮想マシンとは異なり、コンテナーはオペレーティング・システムをバンドルしません。 アプリのコード、ランタイム、システム・ツール、ライブラリー、設定値のみがコンテナー内にパッケージされます。 コンテナーは、仮想マシンより軽量で移植しやすく、効率的です。


開始するための以下のオプションをクリックしてください。

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/cs_public_dedicated_options.png" width="440" alt="アイコンをクリックすると、すぐに {{site.data.keyword.containershort_notm}} を開始できます。{{site.data.keyword.Bluemix_dedicated_notm}} では、このアイコンをクリックするとオプションが表示されます。" style="width:440px;" />
<map name="home_map" id="home_map">
<area href="#clusters" alt="{{site.data.keyword.Bluemix_notm}} の Kubernetes クラスターの概説" title="{{site.data.keyword.Bluemix_notm}} の Kubernetes クラスターの概説" shape="rect" coords="-7, -8, 108, 211" />
<area href="cs_cli_install.html" alt="CLI をインストールします。" title="CLI をインストールします。" shape="rect" coords="155, -1, 289, 210" />
<area href="cs_dedicated.html#dedicated_environment" alt="{{site.data.keyword.Bluemix_dedicated_notm}} クラウド環境" title="{{site.data.keyword.Bluemix_notm}} クラウド環境" shape="rect" coords="326, -10, 448, 218" />
</map>


## クラスターの概説
{: #clusters}

コンテナーにアプリをデプロイするときには、 その前に、 まず Kubernetes クラスターを作成してください。 Kubernetes はコンテナー用のオーケストレーション・ツールです。 Kubernetes を使用すると、開発者はクラスターの高度な機能や柔軟性を活用して、可用性の高いアプリをすぐにデプロイできます。
{:shortdesc}

ところで、クラスターとは一体何でしょうか。 クラスターとは、アプリの高可用性を維持する一連のリソース、ワーカー・ノード、ネットワーク、ストレージ・デバイスのことです。 クラスターを作成したら、アプリをコンテナーにデプロイできます。

**始める前に**

トライアル、従量制課金、またはサブスクリプションの [{{site.data.keyword.Bluemix_notm}} アカウント](https://console.bluemix.net/registration/)が必要です。

トライアル・アカウントでは、サービスに慣れるために 21 日間使用できるフリー・クラスターを 1 つ作成できます。従量制課金アカウントまたはサブスクリプション・アカウントでは、無料のトライアル・クラスターをやはり 1 つ作成できますが、標準クラスターで使用する IBM Cloud インフラストラクチャー (SoftLayer) リソースをプロビジョンすることもできます。
{:tip}

フリー・クラスターを作成するには、以下のようにします。

1.  [{{site.data.keyword.Bluemix_notm}} **カタログ** ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/catalog/?category=containers) で、**「Kubernetes クラスター内のコンテナー」**を選択し、**「作成」**をクリックします。クラスター構成ページが開きます。デフォルトでは、**「フリー・クラスター (Free cluster)」**が選択されています。

2. 固有の名前をクラスターに付けます。

3.  **「クラスターの作成」**をクリックします。 ワーカー・ノードが作成されます。プロビジョンには数分かかることがありますが、**「ワーカー・ノード」**タブで進行状況を確認できます。状況が `Ready` になったら、クラスターを使い始めることができます。

おつかれさまでした。 これで、1 つ目の Kubernetes クラスターが作成されました。 フリー・クラスターの詳細は次のとおりです。

*   **マシン・タイプ**: フリー・クラスターでは、CPU 2 つとメモリー 4 GB を備えた仮想ワーカー・ノード 1 つをアプリで使用できます。 標準クラスターの作成時には、物理 (ベア・メタル) マシンと仮想マシンのどちらにするかや、さまざまなマシン・サイズを選択できます。
*   **管理マスター**: ワーカー･ノードは、{{site.data.keyword.IBM_notm}} が所有する可用性の高い専用のマスターで一元的にモニターされ、管理されます。Kubernetes マスターは、クラスター内のすべての Kubernetes リソースを制御および管理します。 作業担当者はこのマスターの管理のことは気にせずに、ワーカー・ノードとそのワーカー・ノードにデプロイされるアプリに集中できます。
*   **インフラストラクチャー・リソース**: VLANS や IP アドレスなどの、クラスターの実行に必要なリソースは、{{site.data.keyword.IBM_notm}} 所有の IBM Cloud インフラストラクチャー (SoftLayer) アカウントで管理されます。 標準クラスターの作成時は、独自の IBM Cloud インフラストラクチャー (SoftLayer) アカウントでこれらのリソースを管理します。 標準クラスターの作成時に、これらのリソースおよび[必要なアクセス権](cs_users.html#infra_access)について確認できます。
*   **その他のオプション**: フリー・クラスターは選択した地域内にデプロイされますが、ロケーション (データ・センター) は選択できません。ロケーション、ネットワーキング、永続ストレージを制御するには、標準クラスターを作成してください。[フリー・クラスターと標準クラスターの利点について詳しくは、こちらを参照してください](cs_why.html#cluster_types)。


**次の作業**
これから 21 日の間に、フリー・クラスターでさまざまな機能を試してください。

* [CLI をインストールして、クラスターでの作業を開始します。](cs_cli_install.html#cs_cli_install)
* [クラスターにアプリをデプロイします。](cs_app.html#app_cli)
* [可用性を高めるために、複数のノードを使用する標準クラスターを作成します。](cs_clusters.html#clusters_ui)
* [{{site.data.keyword.Bluemix_notm}} にプライベート・レジストリーをセットアップし、Docker イメージを保管して他のユーザーと共有します。](/docs/services/Registry/index.html)
