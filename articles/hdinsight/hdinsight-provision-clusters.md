<properties
   pageTitle="HDInsight で Hadoop クラスターを作成する | Microsoft Azure"
    description="Azure ポータル、Azure PowerShell、コマンド ライン、.NET SDK を使用して、Azure HDInsight のクラスターを作成する方法について説明します。"
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="paulettm"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="12/11/2015"
   ms.author="jgao"/>

# HDInsight で Hadoop クラスターを作成する

HDInsight クラスターの作成を計画する方法について説明します。


###前提条件:

この記事の手順を開始する前に、次の項目を用意する必要があります。

- Azure サブスクリプション。 参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。


## 基本的な構成オプション

HDInsight クラスターを作成する際の基本的な構成オプションを以下に示します。

- **クラスター名**

    クラスター名は、クラスターの識別に使用します。 クラスター名は、次のガイドラインに従う必要があります。

    - このフィールドには、3 ～ 63 文字の文字列を指定する必要があります。
    - このフィールドには、文字、数字、ハイフンのみを含めることができます。

- **サブスクリプション名**

    HDInsight クラスターは、1 つの Azure サブスクリプションに関連付けられます。

- **リソース グループ名**

    Azure リソース マネージャー (ARM) では、アプリケーションで、グループとしてリソースを使用することができます。 
    Azure リソース グループと呼ばれます。 デプロイ、更新、監視、すべてのリソースの削除 
    1 つのアプリケーションの操作を調整します。 詳細については、 
    参照してください [Azure リソース マネージャーの概要](resource-group-overview.md)します。  
    
- **オペレーティング システム**

    次の 2 つのオペレーティング システムのいずれかで、HDInsight クラスターを作成できます。
    - **Windows (Windows Server 2012 R2 Datacenter) での HDInsight**:
    - **Linux (Ubuntu 12.04 LTS for Linux) での HDInsight**: HDInsight は Azure で Linux クラスターを構成するオプションを提供します。 Linux または Unix に詳しい場合や、Linux 向けに構築された Hadoop エコシステム コンポーネントとの簡単な統合が必要な場合は、既存の Linux ベースの Hadoop ソリューションから移行することで Linux クラスターを構成します。 詳細については、次を参照してください。 [HDInsight の Linux での Hadoop 入門](hdinsight-hadoop-linux-get-started.md)します。

- **クラスターの種類** と **クラスター サイズ (データ ノードとも呼ばれます)**

    HDInsight を使用すると、さまざまなクラスターの種類を、異なるデータ分析ワークロードにデプロイできるようになります。 現在提供されているクラスターの種類は次のとおりです。

    - Hadoop クラスター: クエリおよび分析ワークロード用
    - HBase クラスター: NoSQL ワークロード用
    - Storm クラスター: リアルタイム イベント処理ワークロード用
    - Spark クラスター (プレビュー): メモリ内処理、対話型クエリ、ストリーミング、機械学習の各ワークロード用

    ![HDInsight クラスター](./media/hdinsight-provision-clusters/hdinsight.clusters.png)

    > [AZURE.NOTE] *Azure HDInsight クラスター* ともいいます *HDInsight の Hadoop クラスター*, 、または *HDInsight クラスター*します。 場合によってを使用している同じ意味で *Hadoop クラスター*します。 これらはすべて、Microsoft Azure 環境でホストされている Hadoop クラスターを指します。

    特定のクラスターの種類には、各種ノードのさまざまなロールがあります。これにより、ワークロードの詳細に適した特定のロールでノードのサイズを設定できます。 たとえば、Hadoop クラスターで実行する分析がメモリ消費型である場合、大量のメモリを備えた worker ノードを作成できます。

    ![HDInsight Hadoop クラスター ロール](./media/hdinsight-provision-clusters/HDInsight.Hadoop.roles.png)

    HDInsight の Hadoop クラスターは 2 つのロールでデプロイされます。

    - ヘッド ノード (2 ノード)
    - データ ノード (1 つ以上のノード)

    ![HDInsight Hadoop クラスター ロール](./media/hdinsight-provision-clusters/HDInsight.HBase.roles.png)

    HDInsight の HBase クラスターは 3 つのロールでデプロイされます。
    - ヘッド サーバー (2 つのノード)
    - リージョン サーバー (1 つ以上のノード)
    - マスター/Zookeeper ノード (3 つのノード)

    ![HDInsight Hadoop クラスター ロール](./media/hdinsight-provision-clusters/HDInsight.Storm.roles.png)

    HDInsight の Storm クラスターは 3 つのロールでデプロイされます。
    - Nimbus ノード (2 つのノード)
    - Supervisor サーバー (1 つ以上のノード)
    - Zookeeper ノード (3 つのノード)


    ![HDInsight Hadoop cluster roles](./media/hdinsight-provision-clusters/HDInsight.Spark.roles.png)

    Spark clusters for HDInsight are deployed with three roles:
    - Head node (2 nodes)
    - Worker node (at least 1 node)
    - Zookeeper nodes (3 nodes) (Free for A1 Zookeepers)

    Customers are billed for the usage of those nodes for the duration of the cluster’s life. Billing starts once a cluster is created and stops when the cluster is deleted (clusters can’t be de-allocated or put on hold). The cluster size affects the cluster price. For learning purposes, it is recommended to use 1 data node. For more information about HDInsight pricing, see [HDInsight pricing](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).


    >[AZURE.NOTE] The cluster size limit varies among Azure subscriptions. Contact billing support to increase the limit.

- **HDInsight のバージョン**

    このクラスターに使用する HDInsight のバージョンを指定するために使用します。 詳細については、次を参照してください [Hadoop クラスターのバージョンと HDInsight でのコンポーネント。](https://go.microsoft.com/fwLink/?LinkID=320896&clcid=0x409)


- **場所 (リージョン)**

    HDInsight クラスターとその既定のストレージ アカウントは、同じ Azure の場所に配置されている必要があります。
    
    ![Azure Azure リージョン](./media/hdinsight-provision-clusters/Azure.regions.png)

    サポートされているリージョンの一覧を表示するには、クリックして、 **地域** ドロップダウン リストで [HDInsight 料金](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409)します。

- **ノード サイズ**

    ![HDInsight VM ノードのサイズ](./media/hdinsight-provision-clusters/hdinsight.node.sizes.png)

    ノードの VM サイズを選択します。 詳細については、次を参照してください [クラウド サービスのサイズ。](cloud-services-sizes-specs.md)

    VM の選択に基づき、料金が異なる場合があります。 HDInsight は、クラスター ノードにすべて Standard レベルの VM を使用します。 VM サイズに応じた料金の詳細については、「<a href="http://azure.microsoft.com/pricing/details/hdinsight/" target="_blank">HDInsight 料金</a>」をご覧ください。


- **HDInsight ユーザー**

    HDInsight クラスターでは、プロビジョニング時に次の 2 つのユーザー アカウントを構成できます。

    - HTTP ユーザー。 既定のユーザー名は、Azure ポータルで基本構成を使用する admin です。
    - RDP ユーザー (Windows クラスター): RDP を使用してクラスターに接続する際に使用します。 アカウントの作成時に、当日から 90 日以内の有効期限を設定する必要があります。
    - SSH ユーザー (Linux クラスター): SSH を使用してクラスターに接続する際に使用します。 追加の SSH ユーザー アカウントを作成するには、次の手順に従って、クラスターを作成した後 [SSH Linux、Unix、または OS X から HDInsight 上の Linux ベースの Hadoop を使用する](hdinsight-hadoop-linux-use-ssh-unix.md)です。



- **Azure ストレージ アカウント**

    元の HDFS では、クラスターの多数のローカル ディスクを使用します。 HDInsight では、データ ストレージとして Azure BLOB ストレージを使用します。 Azure BLOB ストレージは、堅牢な汎用ストレージ ソリューションであり、HDInsight とシームレスに統合されます。 Hadoop 分散ファイル システム (HDFS) のインターフェイスを利用して、HDInsight のすべてのコンポーネントが BLOB ストレージの構造化データまたは非構造化データを直接操作できます。 BLOB ストレージにデータを格納した場合、計算で使用する HDInsight クラスターを削除してもユーザー データは失われません。

    構成時に、Azure ストレージ アカウントと、その Azure ストレージ アカウントの Azure BLOB ストレージ コンテナーを指定する必要があります。 一部の作成プロセスには、事前に作成された Azure ストレージ アカウントと BLOB ストレージ コンテナーが必要です。  BLOB ストレージ コンテナーは、既定の保存先としてクラスターで使用されます。 必要に応じて、クラスターがアクセス可能なその他の Azure Storage アカウント (リンクされたストレージ) を指定できます。 また、クラスターは、完全なパブリック読み取りアクセスまたは BLOB だけを対象とするパブリック読み取りアクセスで構成された BLOB コンテナーにアクセスすることもできます。  アクセスの制限の詳細については、次を参照してください。 [Azure ストレージ リソースへのアクセスの管理](storage-manage-access-to-resources.md)します。

    ![HDInsight ストレージ](./media/hdinsight-provision-clusters/HDInsight.storage.png)

    >[AZURE.NOTE] Blob ストレージ コンテナーは、図のように blob のセットをグループ化を提供します。

    ![Azure BLOB ストレージ](./media/hdinsight-provision-clusters/Azure.blob.storage.jpg)


    >[AZURE.WARNING] 1 つの Blob ストレージ コンテナーを複数のクラスターを共有しないでください。 これはサポートされていません。

    For more information on using secondary Blob stores, see [Using Azure Blob Storage with HDInsight](hdinsight-use-blob-storage.md).

- **Hive/Oozie メタストア**

    メタストアには、Hive テーブル、パーティション、スキーマ、列などについての Hive と Oozie メタデータが格納されます。 メタストアを使用すると Hive と Oozie メタデータを保持できるため、新しいクラスターを作成するときに、Hive テーブルまたは Oozie ジョブを再作成する必要はありません。 既定では、Hive は組み込みの Azure SQL Database を使用してこの情報を格納します。 組み込みデータベースは、クラスターが削除されるとメタデータを保持できません。 たとえば、Hive メタストアを使用して作成されたクラスターがあるとします。 作成済みの Hive テーブルがいくつかあります。 クラスターを削除した後に、同じ Hive メタストアを使用してクラスターを再作成すると、元のクラスターで作成していた Hive テーブルがあることがわかります。
    
    > [AZURE.NOTE] メタストアの構成では、HBase クラスターの種類を使用できません。

## 高度な構成オプション

### HDInsight クラスターのカスタマイズを使用したクラスターのカスタマイズ

次の構成ファイルを構成することが必要な場合があります。

- core-site.xml
- hdfs-site.xml
- mapred-site.xml
- yarn-site.xml
- hive-site.xml
- oozie-site.xml

再イメージ化により、クラスターは変更を保持できません。 詳細については、 
参照してください [によるロール インスタンスが再起動 OS のアップグレード](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx)します。 
クラスターの有効期間中に変更を保持するには、作成プロセスで HDInsight クラスターのカスタマイズを使用します。

Hive 構成をカスタマイズする Azure PowerShell スクリプトの例を次に示します。

    # hive-site.xml configuration
    $hiveConfigValues = @{ "hive.metastore.client.socket.timeout"="90" }
    
    $config = New-AzureRmHDInsightClusterConfig `
        | Set-AzureRmHDInsightDefaultStorage `
            -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
            -StorageAccountKey $defaultStorageAccountKey `
        | Add-AzureRmHDInsightConfigValues `
            -HiveSite $hiveConfigValues 
    
    New-AzureRmHDInsightCluster `
        -ResourceGroupName $existingResourceGroupName `
        -ClusterName $clusterName `
        -Location $location `
        -ClusterSizeInNodes $clusterSizeInNodes `
        -ClusterType Hadoop `
        -OSType Windows `
        -Version "3.2" `
        -HttpCredential $httpCredential `
        -Config $config 

他の構成ファイルのカスタマイズのサンプルを次に示します。

    # hdfs-site.xml configuration
    $HdfsConfigValues = @{ "dfs.blocksize"="64m" } #default is 128MB in HDI 3.0 and 256MB in HDI 2.1

    # core-site.xml configuration
    $CoreConfigValues = @{ "ipc.client.connect.max.retries"="60" } #default 50

    # mapred-site.xml configuration
    $MapRedConfigValues = @{ "mapreduce.task.timeout"="1200000" } #default 600000

    # oozie-site.xml configuration
    $OozieConfigValues = @{ "oozie.service.coord.normal.default.timeout"="150" }  # default 120

詳細については、Azim Uddin のブログ「を参照してください。 [HDInsight クラスターのカスタマイズ creationg](http://blogs.msdn.com/b/bigdatasupport/archive/2014/04/15/customizing-hdinsight-cluster-provisioning-via-powershell-and-net-sdk.aspx)します。




### スクリプト アクションを使用したクラスターのカスタマイズ

追加コンポーネントをインストールするか、作成中にスクリプトを使用してクラスターの構成をカスタマイズできます。 このようなスクリプトを使用して実行します。 **Script Action**, 、これは、ポータルから使用できる構成オプション HDInsight Windows PowerShell コマンドレットまたは HDInsight .NET SDK です。 詳細については、次を参照してください。 [HDInsight クラスターのカスタマイズ Script Action を使って](hdinsight-hadoop-customize-cluster.md)します。


### Azure Virtual Network の使用

[Azure の仮想ネットワーク](http://azure.microsoft.com/documentation/services/virtual-network/) ソリューションに必要なリソースを含む、セキュリティで保護された永続的なネットワークを作成することができます。 仮想ネットワークでは、次のことが可能になります。

* プライベート ネットワーク (クラウドのみ) 内でのクラウド リソース間の接続

    ![クラウドのみの構成の図](./media/hdinsight-provision-clusters/hdinsight-vnet-cloud-only.png)

* 仮想プライベート ネットワーク (VPN) を使用したローカル データセンター ネットワークへのクラウド リソースの接続 (サイト間またはポイント対サイト)

    サイト間構成では、ハードウェア VPN を使用するか、ルーティングとリモート アクセス サービスを使用して、データセンターから複数のリソースを Azure 仮想ネットワークに接続できます。

    ![サイト間構成の図](./media/hdinsight-provision-clusters/hdinsight-vnet-site-to-site.png)

    ポイント対サイト構成では、ソフトウェア VPN を使用して、特定のリソースを Azure 仮想ネットワークに接続できます。

    ![ポイント対サイト構成の図](./media/hdinsight-provision-clusters/hdinsight-vnet-point-to-site.png)

仮想ネットワーク機能、利点、および機能の詳細については、次を参照してください。、 [Azure 仮想ネットワークの概要](../virtual-network/virtual-networks-overview.md)します。

> [AZURE.NOTE] HDInsight クラスターをプロビジョニングする前に、Azure の仮想ネットワークを作成する必要があります。 詳細については、次を参照してください。 [Hadoop クラスターを仮想ネットワークを作成する](hdinsight-hbase-provision-vnet.md#provision-an-hbase-cluster-into-a-virtual-network)です。
>
> Azure HDInsight は場所ベースの Virtual Networks のみをサポートし、アフィニティ グループ ベースの Virtual Networks は現在取り扱っていません。 既存の Azure Virtual Network が場所ベースかどうかを確認するには、Azure PowerShell コマンドレットの Get-AzureVNetConfig を使用します。 Virtual Network が場所ベースでない場合、次のオプションがあります。
>
> - 既存の Virtual Network 構成をエクスポートし、新しい Virtual Network を作成します。 すべての新しい仮想ネットワークは、既定で場所ベースです。
> - 場所ベースの Virtual Network に移行します。  参照してください [リージョン スコープへの既存のサービスの移行](http://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/)します。
>
> 1 つのクラスターには単一のサブネットを指定することを強くお勧めします。

## ポータルを使用した作成

参照することができます、 [基本的な構成オプション](#basic-configuration-options), 、および [詳細構成オプション](#advanced-configuration-options) にフィールドの説明。

**HDInsight クラスターを作成するには**

1. サインイン、 [Azure ポータル][azure-preview-portal]します。
2. クリックして **新規**, 、] をクリックして **データ分析**, 、] をクリックし、 **HDInsight**します。

    ![Azure ポータルでの新しいクラスターの作成](./media/hdinsight-provision-clusters/HDI.CreateCluster.1.png "Creating a new cluster in the Azure Portal")

3. 次の値を入力または選択します。

  * **クラスター名**: クラスターの名前を入力します。 その名前を使用できる場合は、クラスター名の横に緑色のチェック マークが表示されます。
  * **クラスターの種類**: 選択 **Hadoop**します。
  * **オペレーティング システムをクラスター**: 選択 **Windows Server 2012 R2 Datacenter**します。
  * **サブスクリプション**: このクラスターの作成に使用される Azure サブスクリプションを選択します。
  * **リソース グループ**: 既存を選択するか、新しいリソース グループを作成します。 このエントリには、既存のリソース グループを使用できる場合は、そのうちの 1 つが既定値として設定されます。
  * **資格情報**: ユーザー名とパスワード、Hadoop ユーザー (HTTP ユーザー) を構成します。 クラスターのリモート デスクトップを有効にする場合は、リモート デスクトップ ユーザーのユーザー名とパスワード、アカウントの有効期限を構成する必要があります。 クリックして **選択** 下部には、変更を保存します。

        ![Provide cluster credentials](./media/hdinsight-provision-clusters/HDI.CreateCluster.3.png "Provide cluster credentials")

  * **データ ソース**: 新しいを作成するか、クラスターの既定のファイル システムとして使用する既存の Azure ストレージ アカウントを選択します。

        ![Data source blade](./media/hdinsight-provision-clusters/HDI.CreateCluster.4.png "Provide data source configuration")

        * **Selection Method**: Set this to **From all subscriptions** to enable browsing of storage accounts from all your subscriptions. Set this to **Access Key** if you want to enter the **Storage Name** and **Access Key** of an existing storage account.
        * **Select storage account / Create New**: Click **Select storage account** to browse and select an existing storage account you want to associate with the cluster. Or, click **Create New** to create a new storage account. Use the field that appears to enter the name of the storage account. A green check will appear if the name is available.
        * **Choose Default Container**: Use this to enter the name of the default container to use for the cluster. While you can enter any name here, we recommend using the same name as the cluster so that you can easily recognize that the container is used for this specific cluster.
        * **Location**: The geographic region that the storage account is in, or will be created in. This location will determine the cluster location.  The cluster and its default storage account must co-locate in the same Azure data center.
    
  * **ノードの価格レベルの**: クラスターに必要なワーカー ノードの数を設定します。 クラスターの推定コストがブレード内に表示されます。
  

        ![Node pricing tiers blade](./media/hdinsight-provision-clusters/HDI.CreateCluster.5.png "Specify number of cluster nodes")


  * **オプションの構成** をクラスターのバージョンを選択できるだけでなく、その他のオプションの設定を追加するなど、 **仮想ネットワーク**, で、設定を **外部メタストア** Hive と Oozie のデータを保持するために、Script Action を使用して、カスタム コンポーネントをインストールするのには、クラスターをカスタマイズするか、クラスターで追加のストレージ アカウントを使用しています。

        * **HDInsight Version**: Select the version you want to use for the cluster. For more information, see [HDInsight cluster versions](hdinsight-component-versioning.md).
        * **Virtual Network**: Select an Azure virtual network and the subnet if you want to place the cluster into a virtual network.  

            ![Virtual network blade](./media/hdinsight-provision-clusters/HDI.CreateCluster.6.png "Specify virtual network details")

            >[AZURE.NOTE] Windows based HDInsight cluster can only be placed into a classical virtual network.
        
        * **External Metastores**: Specify an Azure SQL database to store Hive and Oozie metadata associated with the cluster.
 
            > [AZURE.NOTE] Metastore configuration is not available for HBase cluster types.

            ![Custom metastores blade](./media/hdinsight-provision-clusters/HDI.CreateCluster.7.png "Specify external metastores")

            For **Use an existing SQL DB for Hive** metadata, click **Yes**, select a SQL database, and then provide the username/password for the database. Repeat these steps if you want to **Use an existing SQL DB for Oozie metadata**. Click **Select** till you are back on the **Optional Configuration** blade.

            >[AZURE.NOTE] The Azure SQL database used for the metastore must allow connectivity to other Azure services, including Azure HDInsight. On the Azure SQL database dashboard, on the right side, click the server name. This is the server on which the SQL database instance is running. Once you are on the server view, click **Configure**, and then for **Azure Services**, click **Yes**, and then click **Save**.
        
        * **Script Actions** if you want to use a custom script to customize a cluster, as the cluster is being created. For more information about script actions, see [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md). On the Script Actions blade provide the details as shown in the screen capture.

            ![Script action blade](./media/hdinsight-provision-clusters/HDI.CreateCluster.8.png "Specify script action")

        * **Azure Storage Keys**: Specify additional storage accounts to associate with the cluster. In the **Azure Storage Keys** blade, click **Add a storage key**, and then select an existing storage account or create a new account.

            ![Additional storage blade](./media/hdinsight-provision-clusters/HDI.CreateCluster.9.png "Specify additional storage accounts")

4. クリックして **作成**します。 選択すると **スタート画面にピン** 、ポータルのスタート画面にクラスターは、タイルを追加します。 アイコンはクラスターが作成中であることを示し、作成が完了すると、[HDInsight] アイコンを表示するように変化します。


  	| While creating | Creationg complete |
  	| ------------------ | --------------------- |
  	| ![Provisioning indicator on startboard](./media/hdinsight-provision-clusters/provisioning.png) | ![Provisioned cluster tile](./media/hdinsight-provision-clusters/provisioned.png) |


    
    > [AZURE.NOTE] It will take some time for the cluster to be created, usually around 15 minutes. Use the tile on the Startboard, or the **Notifications** entry on the left of the page to check on the provisioning process.
    

5. 作成が完了したら、スタート画面でクラスター用のタイルをクリックして、クラスター ブレードを起動します。 クラスター ブレードには、名前、属しているリソース グループ、場所、オペレーティング システム、クラスター ダッシュボードの URL などのクラスターに関する基本情報が表示されます。


    ![Cluster blade](./media/hdinsight-provision-clusters/HDI.Cluster.Blade.png "Cluster properties")


    Use the following to understand the icons at the top of this blade, and in the **Essentials** section:


    * **Settings** and **All Settings**: Displays the **Settings** blade for the cluster, which allows you to access detailed configuration information for the cluster.
    * **Dashboard**, **Cluster Dashboard**, and **URL**: These are all ways to access the cluster dashboard, which is a Web portal to run jobs on the cluster.
    * **Remote Desktop**: Enables you to enable/disable remote desktop on the cluster nodes.
    * **Scale Cluster**: Allows you to change the number of worker nodes for this cluster.
    * **Delete**: Deletes the HDInsight cluster.
    * **Quickstart** (![cloud and thunderbolt icon = quickstart](./media/hdinsight-provision-clusters/quickstart.png)): Displays information that will help you get started using HDInsight.
    * **Users** (![users icon](./media/hdinsight-provision-clusters/users.png)): Allows you to set permissions for _portal management_ of this cluster for other users on your Azure subscription.
    

        > [AZURE.IMPORTANT] This _only_ affects access and permissions to this cluster in the Portal, and has no effect on who can connect to or submit jobs to the HDInsight cluster.
        
    * **Tags** (![tag icon](./media/hdinsight-provision-clusters/tags.png)): Tags allows you to set key/value pairs to define a custom taxonomy of your cloud services. For example, you may create a key named __project__, and then use a common value for all services associated with a specific project.

## ARM のテンプレートを使用した作成

Azure リソース マネージャー (ARM) のテンプレートを使用すると、クラスターのデプロイと再デプロイが容易になります。 次の手順では、Linux ベースの HDInsight クラスターを作成します。

**ARM のテンプレートを使用してクラスターをデプロイするには**

1. Json ファイルの保存 [付録 A](#appendix-a---arm-template) をワークステーションにします。
2. 必要な場合は、パラメーターを設定します。
3. 次の PowerShell スクリプトを使用して、テンプレートを実行します。

        $subscriptionId = "<Azure Subscription ID"
        
        $newResourceGroupName = "<Azure Resource Group Name>"
        $Location = "EAST US 2" # for creating ARM group
                
        $armDeploymentName = "New-HDInsigt-Cluster-" + (Get-Date -Format MMdd)
        $newClusterName = "<HDInsight Cluster Name>"
        $clusterStorageAccountName = "<Default Storage Account Name>"
                
        # Connect to Azure
        #Login-AzureRmAccount
        #Select-AzureRmSubscription -SubscriptionId $subscriptionId
                
        # Create a resource group
        New-AzureRmResourceGroup -Name $newResourceGroupName -Location $Location
                
        # Create cluster and the dependent storage accounge
        $parameters = @{clusterName="$newClusterName";clusterStorageAccountName="$clusterStorageAccountName"}
                
        New-AzureRmResourceGroupDeployment `
            -Name $armDeploymentName `
            -ResourceGroupName $newResourceGroupName `
            -TemplateFile E:\HDITutorials-ARM\Create-clusters\hdinsight-arm-template.json `
            -TemplateParameterObject $parameters
                
        # List cluster
        Get-AzureRmHDInsightCluster -ResourceGroupName $newResourceGroupName -ClusterName $newClusterName 

    PowerShell スクリプトは、クラスター名とストレージ アカウント名のみを構成します。  ARM テンプレートのその他の値は設定可能です。 
    
その他のメソッドを使用して ARM テンプレートを展開するため、次を参照してください。 [Azure リソース マネージャー テンプレートを使用してアプリケーションを配置](resource-group-template-deploy.md)します。


## Azure PowerShell を使用した作成
Azure PowerShell は、Azure のワークロードのデプロイメントと管理を制御し自動化するために使用できる強力なスクリプティング環境です。 このセクションでは、Azure PowerShell を使用して HDInsight クラスターをプロビジョニングする方法について説明します。 HDInsight Windows PowerShell コマンドレットを実行するワークステーションの構成方法の詳細については、次を参照してください。 [をインストールし、Azure PowerShell を構成](../install-configure-powershell.md)します。 HDInsight で Azure PowerShell の使用の詳細については、次を参照してください。 [PowerShell を使用した HDInsight の管理](hdinsight-administer-use-powershell.md)します。 HDInsight Windows PowerShell コマンドレットの一覧で、次を参照してください。 [HDInsight コマンドレット リファレンス](https://msdn.microsoft.com/library/azure/dn858087.aspx)します。


Azure PowerShell を使用して HDInsight クラスターを作成するには、以下の手順が必要です。

- Azure リソース グループの作成
- Azure Storage アカウントの作成
- Azure BLOB コンテナーの作成
- HDInsight クラスターの作成


    $subscriptionId = "<Azure Subscription ID>"
    
    $newResourceGroupName = "<Azure Resource Group Name>"
    $location = "<Azure Location>" # for example, "East US 2"
    $newDefaultStorageAccountName = "<Azure Storage Account Name>"
    $newClusterName = "<Azure HDInsight Cluster Name>"
    $clusterSizeInNodes = 1
    
    ###########################################
    # login Azure
    ###########################################
    Login-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionId $subscriptionId
    
    ###########################################
    # Create the resource group
    ###########################################
    New-AzureRmResourceGroup -Name $newRresourceGroupName -Location $location
    
    ###########################################
    # Preapre default storage account and container
    ###########################################
    New-AzureRmStorageAccount -ResourceGroupName $newResourceGroupName -Name $newDefaultStorageAccountName -Location $location
    
    $defaultStorageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $newResourceGroupName -Name $newDefaultStorageAccountName |  %{ $_.Key1 }
    $defaultStorageContext = New-AzureStorageContext -StorageAccountName $newDefaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer -Name $newClusterName -Context $defaultStorageContext #use the cluster name as the container name
        
    ###########################################
    # Create the cluster
    ###########################################
    $httpCredential =Get-Credential -Message "Enter the HTTP account credential:"
    New-AzureRmHDInsightCluster `
        -ResourceGroupName $newResourceGroupName `
        -ClusterName $newClusterName `
        -Location $location `
        -ClusterSizeInNodes $clusterSizeInNodes `
        -ClusterType Hadoop `
        -OSType Windows `
        -Version "3.2" `
        -HttpCredential $httpCredential 



## HDInsight .NET SDK を使用した作成
HDInsight .NET SDK は、.NET Framework アプリケーションから HDInsight を簡単に操作できる .NET クライアント ライブラリを提供します。 Visual Studio コンソール アプリケーションを作成し、クラスターを作成するためのコードを貼り付けるには、次の手順に従います。

**Visual Studio コンソール アプリケーションを作成するには**

1. Visual Studio で、新しい C# コンソール アプリケーションを作成します。
2. NuGet パッケージ管理コンソールで次の Nuget コマンドを実行します。

        Install-Package Microsoft.Azure.Common.Authentication -pre
        Install-Package Microsoft.Azure.Management.HDInsight -Pre

6. ソリューション エクスプ ローラーでダブルクリック **Program.cs** を開くには、次のコードを貼り付けるし、変数の値を指定します。

        using System;
        using System.Security;
        using Microsoft.Azure;
        using Microsoft.Azure.Common.Authentication;
        using Microsoft.Azure.Common.Authentication.Factories;
        using Microsoft.Azure.Common.Authentication.Models;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Models;
        
        namespace CreateHDInsightCluster
        {
            class Program
            {
                private static HDInsightManagementClient _hdiManagementClient;
        
                private static Guid SubscriptionId = new Guid("<Azure Subscription ID");
                private const string ExistingResourceGroupName = "<Azure Resource Group Name>";
                private const string ExistingStorageName = "<Default Storage Account Name>.blob.core.windows.net";
                private const string ExistingStorageKey = "<Default Storage Account Key>";
                private const string ExistingBlobContainer = "<Default Blob Container Name>";
                private const string NewClusterName = "<HDInsight Cluster Name>";
                private const int NewClusterNumNodes = 1;
                private const string NewClusterLocation = "EAST US 2";     // Must be the same as the default Storage account
                private const OSType NewClusterOsType = OSType.Windows;
                private const HDInsightClusterType NewClusterType = HDInsightClusterType.Hadoop;
                private const string NewClusterVersion = "3.2";
                private const string NewClusterUsername = "admin";
                private const string NewClusterPassword = "<HTTP User password";
        
                static void Main(string[] args)
                {
                    System.Console.WriteLine("Running");
        
                    var tokenCreds = GetTokenCloudCredentials();
                    var subCloudCredentials = GetSubscriptionCloudCredentials(tokenCreds, SubscriptionId);
        
                    _hdiManagementClient = new HDInsightManagementClient(subCloudCredentials);
                
                    var parameters = new ClusterCreateParameters
                    {
                        ClusterSizeInNodes = NewClusterNumNodes,
                        UserName = NewClusterUsername,
                        Password = NewClusterPassword,
                        Location = NewClusterLocation,
                        DefaultStorageAccountName = ExistingStorageName,
                        DefaultStorageAccountKey = ExistingStorageKey,
                        DefaultStorageContainer = ExistingBlobContainer,
                        ClusterType = NewClusterType,
                        OSType = NewClusterOsType
                    };
        
                    _hdiManagementClient.Clusters.Create(ExistingResourceGroupName, NewClusterName, parameters);
                }
                private static void CreateCluster()
                {
                    var parameters = new ClusterCreateParameters
                    {
                        ClusterSizeInNodes = NewClusterNumNodes,
                        UserName = NewClusterUsername,
                        Password = NewClusterPassword,
                        Location = NewClusterLocation,
                        DefaultStorageAccountName = ExistingStorageName,
                        DefaultStorageAccountKey = ExistingStorageKey,
                        DefaultStorageContainer = ExistingBlobContainer,
                        ClusterType = NewClusterType,
                        OSType = NewClusterOsType
                    };
        
                    _hdiManagementClient.Clusters.Create(ExistingResourceGroupName, NewClusterName, parameters);
                }
        
                public static TokenCloudCredentials GetTokenCloudCredentials(string username = null, SecureString password = null)
                {
                    var authFactory = new AuthenticationFactory();
        
                    var account = new AzureAccount { Type = AzureAccount.AccountType.User };
        
                    if (username != null && password != null)
                        account.Id = username;
        
                    var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
        
                    var accessToken =
                        authFactory.Authenticate(account, env, AuthenticationFactory.CommonAdTenant, password, ShowDialog.Auto)
                            .AccessToken;
        
                    return new TokenCloudCredentials(accessToken);
                }
        
                public static SubscriptionCloudCredentials GetSubscriptionCloudCredentials(TokenCloudCredentials creds, Guid subId)
                {
                    return new TokenCloudCredentials(subId.ToString(), creds.Token);
        
                }
            }
        }

7. キーを押して **f5 キーを押して** アプリケーションを実行します。 コンソール ウィンドウが開き、アプリケーションの状態が表示されます。 Azure アカウントの資格情報の入力も求められます。 HDInsight クラスターの作成は数分かかる場合があります。

## オンプレミスの SQL Server Integration Services を利用した作成

SQL Server Integration Services (SSIS) を利用して HDInsight クラスターを作成または削除することもできます。 Azure Feature Pack for SSIS には、HDInsight クラスターと連動する次のコンポーネントがあります。


- [Azure HDInsight クラスターの作成タスク][ssisclustercreate]
- [Azure HDInsight クラスターの削除タスク][ssisclusterdelete]
- [Azure サブスクリプション接続マネージャー][connectionmanager]

SSIS を Azure Feature Pack について学ぶこと [ここ][ssispack]します。


##次のステップ
この記事では、HDInsight クラスターを作成する方法をいくつか説明しました。 詳細については、次の記事を参照してください。

* [Azure HDInsight の概要](hdinsight-get-started.md) -HDInsight クラスターの使用を開始する方法について説明
* [HDInsight での Sqoop の使用](hdinsight-use-sqoop.md) -HDInsight と SQL Database または SQL Server の間でデータをコピーする方法について
* [PowerShell を使用した HDInsight の管理](hdinsight-administer-use-powershell.md) -Azure PowerShell を使用して HDInsight を操作する方法について説明
* [プログラムによる Hadoop ジョブの送信](hdinsight-submit-hadoop-jobs-programmatically.md) -プログラムを使用して HDInsight にジョブを送信する方法について説明
* [Azure HDInsight SDK のドキュメント] [hdinsight-sdk-documentation] -HDInsight SDK の検出



##付録 A - ARM テンプレート

次に示す Azure リソース マネージャーのテンプレートは、Azure ストレージ アカウントに依存する Hadoop クラスターを作成します。

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "location": {
          "type": "string",
          "defaultValue": "North Europe",
          "allowedValues": [
            "North Europe"
          ],
          "metadata": {
            "description": "The location where all azure resources will be deployed."
          }
        },
        "clusterName": {
          "type": "string",
          "metadata": {
            "description": "The name of the HDInsight cluster to create."
          }
        },
        "clusterLoginUserName": {
          "type": "string",
          "defaultValue": "admin",
          "metadata": {
            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
          }
        },
        "clusterLoginPassword": {
          "type": "securestring",
          "metadata": {
            "description": "The password for the cluster login."
          }
        },
        "sshUserName": {
          "type": "string",
          "defaultValue": "username",
          "metadata": {
            "description": "These credentials can be used to remotely access the cluster and the edge node virtual machine."
          }
        },
        "sshPassword": {
          "type": "securestring",
          "metadata": {
            "description": "The password for the ssh user."
          }
        },
        "clusterStorageAccountName": {
          "type": "string",
          "metadata": {
            "description": "The name of the storage account to be created and be used as the cluster's storage."
          }
        },
        "clusterStorageType": {
          "type": "string",
          "defaultValue": "Standard_LRS",
          "allowedValues": [
            "Standard_LRS",
            "Standard_GRS",
            "Standard_ZRS"
          ]
        },
        "clusterWorkerNodeCount": {
          "type": "int",
          "defaultValue": 4,
          "metadata": {
            "description": "The number of nodes in the HDInsight cluster."
          }
        }
      },
      "variables": {},
      "resources": [
        {
          "name": "[parameters('clusterStorageAccountName')]",
          "type": "Microsoft.Storage/storageAccounts",
          "location": "[parameters('location')]",
          "apiVersion": "2015-05-01-preview",
          "dependsOn": [],
          "tags": {},
          "properties": {
            "accountType": "[parameters('clusterStorageType')]"
          }
        },
        {
          "name": "[parameters('clusterName')]",
          "type": "Microsoft.HDInsight/clusters",
          "location": "[parameters('location')]",
          "apiVersion": "2015-03-01-preview",
          "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"
          ],
          "tags": {},
          "properties": {
            "clusterVersion": "3.2",
            "osType": "Linux",
            "clusterDefinition": {
              "kind": "hadoop",
              "configurations": {
                "gateway": {
                  "restAuthCredential.isEnabled": true,
                  "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                  "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                }
              }
            },
            "storageProfile": {
              "storageaccounts": [
                {
                  "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                  "isDefault": true,
                  "container": "[parameters('clusterName')]",
                  "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), '2015-05-01-preview').key1]"
                }
              ]
            },
            "computeProfile": {
              "roles": [
                {
                  "name": "headnode",
                  "targetInstanceCount": "1",
                  "hardwareProfile": {
                    "vmSize": "Large"
                  },
                  "osProfile": {
                    "linuxOperatingSystemProfile": {
                      "username": "[parameters('sshUserName')]",
                      "password": "[parameters('sshPassword')]"
                    }
                  }
                },
                {
                  "name": "workernode",
                  "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                  "hardwareProfile": {
                    "vmSize": "Large"
                  },
                  "osProfile": {
                    "linuxOperatingSystemProfile": {
                      "username": "[parameters('sshUserName')]",
                      "password": "[parameters('sshPassword')]"
                    }
                  }
                }
              ]
            }
          }
        }
      ],
      "outputs": {
        "cluster": {
          "type": "object",
          "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
        }
      }
    }


[hdinsight-sdk-documentation]: http://msdn.microsoft.com/library/dn479185.aspx
[azure-preview-portal]: https://manage.windowsazure.com
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx
[ssisclustercreate]: http://msdn.microsoft.com/library/mt146774(v=sql.120).aspx
[ssisclusterdelete]: http://msdn.microsoft.com/library/mt146778(v=sql.120).aspx


