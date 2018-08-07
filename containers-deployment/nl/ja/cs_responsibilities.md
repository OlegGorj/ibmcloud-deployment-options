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



# {{site.data.keyword.containerlong_notm}} を使用する際の責任
{{site.data.keyword.containerlong}} を使用する際のクラスターの管理責任とご利用条件について説明します。
{:shortdesc}

## クラスター管理の責任
{: #responsibilities}

お客様が IBM と分担する、クラスター管理の責任について確認してください。
{:shortdesc}

**IBM は以下について責任を持ちます。**

- クラスター作成時に、マスター、ワーカー・ノード、管理コンポーネント (Ingress アプリケーション・ロード・バランサーなど) をクラスター内にデプロイする
- クラスターの Kubernetes マスターのセキュリティー更新、モニタリング、分離、およびリカバリーを管理する
- ワーカー・ノードの正常性をモニタリングし、それらのワーカー・ノードの更新とリカバリーの自動化を提供する
- ワーカー・ノードの追加、ワーカー・ノードの削除、デフォルト・サブネットの作成などの、インフラストラクチャー・アカウントに対する自動化タスクを実行する
- クラスター内の運用コンポーネント (Ingress アプリケーション・ロード・バランサーやストレージ・プラグインなど) を管理、更新、リカバリーする
- ストレージ・ボリュームを、永続ボリューム請求で要求されたときにプロビジョンする
- すべてのワーカー・ノードにセキュリティー設定を提供する

</br>

**お客様は以下について責任を持ちます。**

- [IBM Cloud インフラストラクチャー (SoftLayer) ポートフォリオにアクセスするために {{site.data.keyword.Bluemix_notm}} アカウントを構成する](cs_troubleshoot_clusters.html#cs_credentials)
- [Kubernetes リソース (ポッド、サービス、デプロイメントなど) をクラスター内にデプロイして管理する](cs_app.html#app_cli)
- [アプリの高可用性が確保されるように、サービスと Kubernetes の機能を活用する](cs_app.html#highly_available_apps)
- [CLI を使用してワーカー・ノードを追加または削除することで、クラスターのキャパシティーを追加または削除する](cs_cli_reference.html#cs_worker_add)
- [クラスターのネットワーク分離のために IBM Cloud インフラストラクチャー (SoftLayer) でパブリック VLAN とプライベート VLAN を作成する ](/docs/infrastructure/vlans/getting-started.html#getting-started-with-vlans)
- [すべてのワーカー・ノードに、Kubernetes マスター URL へのネットワーク接続を設定する](cs_firewall.html#firewall) <p>**注**: ワーカー・ノードにパブリック VLAN とプライベート VLAN の両方が設定されている場合は、ネットワーク接続が構成されています。 ワーカー・ノードにプライベート VLAN だけをセットアップする場合は、代わりのネットワーク接続ソリューションを構成する必要があります。 詳しくは、[ワーカー・ノードの VLAN 接続](cs_clusters.html#worker_vlan_connection)を参照してください。 </p>
- [Kubernetes バージョンの更新が利用可能になったら、マスター kube-apiserver を更新する](cs_cluster_update.html#master)
- [ワーカー・ノードをメジャー・バージョン、マイナー・バージョン、およびパッチ・バージョンについて常に最新の状態に保持する](cs_cluster_update.html#worker_node)
- [トラブルが発生したワーカー・ノードをリカバリーする。これは、`kubectl` コマンド (`cordon` や `drain` など) を実行したり、`bx cs` コマンド (`reboot`、`reload`、`delete` など) を実行したりして行う](cs_cli_reference.html#cs_worker_reboot)
- [IBM Cloud インフラストラクチャー (SoftLayer) 内のサブネットを必要に応じて追加または解除する](cs_subnets.html#subnets)
- [IBM Cloud インフラストラクチャー (SoftLayer) で永続ストレージのデータのバックアップとリストアを実行する ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](../services/RegistryImages/ibm-backup-restore/index.html)
- [Autorecovery を使用したワーカー・ノードの正常性モニタリングの構成](cs_health.html#autorecovery)

<br />


## {{site.data.keyword.containerlong_notm}} の不正使用
{: #terms}

お客様は {{site.data.keyword.containershort_notm}} を不正使用してはいけません。
{:shortdesc}

不正使用には、以下が含まれます。

*   不法行為
*   マルウェアの配布や実行
*   {{site.data.keyword.containershort_notm}} に損害を与えたり、他のお客様による {{site.data.keyword.containershort_notm}} の使用に干渉したりすること
*   他のサービスやシステムに損害を与えたり、他のお客様による使用に干渉したりすること
*   サービスまたはシステムに対する無許可アクセス
*   サービスまたはシステムに対する無許可の変更
*   他のお客様の権利を侵害すること


すべての使用条件については、[クラウド・サービスのご利用条件](https://console.bluemix.net/docs/overview/terms-of-use/notices.html#terms)を参照してください。
