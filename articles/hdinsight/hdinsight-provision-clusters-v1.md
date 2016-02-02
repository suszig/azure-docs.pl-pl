<properties 
   pageTitle="HDInsight での Hadoop クラスターのカスタム プロビジョニング | Microsoft Azure" 
   description="Azure クラシック ポータル、Azure PowerShell、コマンド ライン、.NET SDK を使用して Azure HDInsight のクラスターのカスタム プロビジョニングを行う方法について説明します。" 
   services="hdinsight" 
   documentationCenter="" 
   authors="mumian" 
   manager="paulettm" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/21/2015"
   ms.author="jgao"/>


# Provision Hadoop clusters in HDInsight (HDInsight での Hadoop クラスターのプロビジョニング)

HDInsight クラスターのプロビジョニングを計画する方法について説明します。

[AZURE.INCLUDE [hdinsight-azure-portal](../../includes/hdinsight-azure-portal.md)]

* [HDInsight で Hadoop クラスターをプロビジョニングします。](hdinsight-provision-clusters.md)

**前提条件:**

この記事の手順を開始する前に、次の項目を用意する必要があります。

- Azure サブスクリプション。 参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。


## 基本的な構成オプション

- **クラスター名**

    クラスター名は、クラスターの識別に使用します。 クラスター名は、次のガイドラインに従う必要があります。

    - このフィールドには、3 ～ 63 文字の文字列を指定する必要があります。
    - このフィールドには、文字、数字、ハイフンのみを含めることができます。

- **サブスクリプション名**

    HDInsight クラスターは、1 つの Azure サブスクリプションに関連付けられます。

- **オペレーティング システム**

    次の 2 つのオペレーティング システムのいずれかで HDInsight クラスターをプロビジョニングすることができます。
    - **Windows (Windows Server 2012 R2 Datacenter) での HDInsight**:
    - **Linux (Ubuntu 12.04 LTS for Linux) での HDInsight (プレビュー)**: HDInsight は Azure で Linux クラスターを構成するオプションを提供します。 Linux または Unix に詳しい場合や、Linux 向けに構築された Hadoop エコシステム コンポーネントとの簡単な統合が必要な場合は、既存の Linux ベースの Hadoop ソリューションから移行することで Linux クラスターを構成します。 詳細については、次を参照してください。 [HDInsight の Linux での Hadoop 入門](hdinsight-hadoop-linux-get-started.md)します。

- **HDInsight のバージョン**

    このクラスターに使用する HDInsight のバージョンを指定するために使用します。詳細については、を参照してください [Hadoop クラスターのバージョンと HDInsight でのコンポーネント](https://go.microsoft.com/fwLink/?LinkID=320896&clcid=0x409)。

- **クラスターの種類** と * * (ルール サブタイプ) のクラスター サイズ データ ノード) * *

    HDInsight を使用すると、さまざまなクラスターの種類を、異なるデータ分析ワークロードにデプロイできるようになります。 現在提供されているクラスターの種類は次のとおりです。

    - Hadoop クラスター: クエリおよび分析ワークロード用
    - HBase クラスター: NoSQL ワークロード用
    - Storm クラスター: リアルタイム イベント処理ワークロード用
    - Spark クラスター (プレビュー): メモリ内処理、対話型クエリ、ストリーミング、機械学習の各ワークロード用

    ![HDInsight クラスター](./media/hdinsight-provision-clusters-v1/hdinsight.clusters.png)
    > [AZURE.NOTE] "Azure HDInsight クラスター" は、"HDInsight の Hadoop クラスター"、または "HDInsight クラスター" とも呼ばれます。****** また、"Hadoop クラスター" と同じ意味で使用される場合もあります。** これらはすべて、Microsoft Azure 環境でホストされている Hadoop クラスターを指します。      

    特定のクラスターの種類には、各種ノードのさまざまなロールがあります。これにより、ワークロードの詳細に適した特定のロールでノードのサイズを設定できます。 たとえば、Hadoop クラスターで実行する分析がメモリ消費型である場合、大量のメモリを備えた worker ノードをプロビジョニングできます。

    ![HDInsight Hadoop クラスター ロール](./media/hdinsight-provision-clusters-v1/HDInsight.Hadoop.roles.png)

    HDInsight の Hadoop クラスターは 2 つのロールでデプロイされます。

    - ヘッド ノード (2 ノード)
    - データ ノード (1 つ以上のノード)

    ![HDInsight Hadoop クラスター ロール](./media/hdinsight-provision-clusters-v1/HDInsight.HBase.roles.png)

    HDInsight の HBase クラスターは 3 つのロールでデプロイされます。
    - ヘッド サーバー (2 つのノード)
    - リージョン サーバー (1 つ以上のノード)
    - マスター/Zookeeper ノード (3 つのノード)

    ![HDInsight Hadoop クラスター ロール](./media/hdinsight-provision-clusters-v1/HDInsight.Storm.roles.png)

    HDInsight の Storm クラスターは 3 つのロールでデプロイされます。
    - Nimbus ノード (2 つのノード)
    - Supervisor サーバー (1 つ以上のノード)
    - Zookeeper ノード (3 つのノード)

    ![HDInsight Hadoop クラスター ロール](./media/hdinsight-provision-clusters-v1/HDInsight.Spark.roles.png)

    HDInsight の Spark クラスターは、3 つのロールでデプロイされます。
    - ヘッド ノード (2 ノード)
    - ワーカー ノード (1 つ以上のノード)
    - Zookeeper ノード (3 ノード) (A1 Zookeeper は無料)

    クラスターの有効期間中、これらのノードの使用量に対して課金されます。課金は、クラスターが作成された時点で開始され、クラスターが削除されると終了します (クラスターを割り当て解除したり、保留にしたりすることはできません)。クラスター サイズはクラスターの価格に影響します。学習目的の場合、データ ノードを 1 つ使用することをお勧めします。HDInsight の料金の詳細については、次を参照してください。 [HDInsight 料金](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409)します。

    >[AZURE.NOTE] クラスター サイズの制限は、Azure サブスクリプションによって異なります。 制限値を上げるには、課金サポートにお問い合わせください。

- * * リージョン/仮想ネットワーク (ルール サブタイプ) 場所) * *

    ![Azure リージョン](./media/hdinsight-provision-clusters-v1/Azure.regions.png)

    サポートされているリージョンの一覧を表示するには、クリックして、 **領域** ドロップダウン リストで [HDInsight 料金](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409)します。

- **ノード サイズ**

    ![HDInsight VM ノードのサイズ](./media/hdinsight-provision-clusters-v1/hdinsight.node.sizes.png)

    ノードの VM サイズを選択します。 詳細については、を参照してください [クラウド サービスのサイズ](cloud-services-sizes-specs.md)。

    VM の選択に基づき、価格が異なる場合があります。 HDInsight は、クラスター ノードにすべて Standard レベルの VM を使用します。 VM のサイズに応じた料金の方法については、次を参照してください。 <a href="http://azure.microsoft.com/pricing/details/hdinsight/" target="_blank">HDInsight 料金</a>します。

- **HDInsight ユーザー**

    HDInsight クラスターでは、プロビジョニング時に次の 2 つのユーザー アカウントを構成できます。

    - HTTP ユーザー。 既定のユーザー名は、Azure クラシック ポータルで基本構成を使用する admin です。
    - RDP ユーザー (Windows クラスター): RDP を使用してクラスターに接続する際に使用します。 アカウントの作成時に、当日から 90 日以内の有効期限を設定する必要があります。
    - SSH ユーザー (Linux クラスター): SSH を使用してクラスターに接続する際に使用します。 追加の SSH ユーザー アカウントを作成するには、次の手順に従って、クラスターを作成した後 [SSH Linux、Unix、または OS X から HDInsight 上の Linux ベースの Hadoop を使用する](hdinsight-hadoop-linux-use-ssh-unix.md)します。

- **Azure ストレージ アカウント**

    元の HDFS では、クラスターの多数のローカル ディスクを使用します。 HDInsight では、データ ストレージとして Azure BLOB ストレージを使用します。 Azure BLOB ストレージは、堅牢な汎用ストレージ ソリューションであり、HDInsight とシームレスに統合されます。 Hadoop 分散ファイル システム (HDFS) のインターフェイスを利用して、HDInsight のすべてのコンポーネントが BLOB ストレージの構造化データまたは非構造化データを直接操作できます。 BLOB ストレージにデータを格納した場合、計算で使用する HDInsight クラスターを削除してもユーザー データは失われません。

    構成時に、Azure ストレージ アカウントと、その Azure ストレージ アカウントの Azure BLOB ストレージ コンテナーを指定する必要があります。 一部のプロビジョニング プロセスには、事前に作成された Azure ストレージ アカウントと BLOB ストレージ コンテナーが必要です。 BLOB ストレージ コンテナーは、既定の保存先としてクラスターで使用されます。 必要に応じて、クラスターがアクセス可能なその他の Azure Storage アカウント (リンクされたストレージ) を指定できます。 また、クラスターは、完全なパブリック読み取りアクセスまたは BLOB だけを対象とするパブリック読み取りアクセスで構成された BLOB コンテナーにアクセスすることもできます。 アクセスの制限の詳細については、次を参照してください。 [Azure ストレージ リソースへのアクセスの管理](storage-manage-access-to-resources.md)します。

    ![HDInsight ストレージ](./media/hdinsight-provision-clusters-v1/HDInsight.storage.png)
    >[AZURE.NOTE] 次の図に示すように、BLOB ストレージ コンテナーには、一連の BLOB がまとめられています。

    ![Azure BLOB ストレージ](./media/hdinsight-provision-clusters-v1/Azure.blob.storage.jpg)

    >[AZURE.WARNING] 1 つの Blob ストレージ コンテナーを複数のクラスターで共有しないでください。 これはサポートされていません。 

    セカンダリの Blob ストアの使用の詳細については、次を参照してください。 [HDInsight で Azure Blob ストレージを使用して](hdinsight-use-blob-storage.md)します。

- **Hive/Oozie メタストア**

    メタストアには、Hive テーブル、パーティション、スキーマ、列などについての Hive と Oozie メタデータが格納されます。 メタストアを使用すると Hive と Oozie メタデータを保持できるため、新しいクラスターをプロビジョニングするときに、Hive テーブルまたは Oozie ジョブを再作成する必要はありません。 既定では、Hive は組み込みの Azure SQL Database を使用してこの情報を格納します。 組み込みデータベースは、クラスターが削除されるとメタデータを保持できません。 たとえば、Hive メタストアを使用してプロビジョニングされたクラスターがあるとします。 作成済みの Hive テーブルがいくつかあります。 クラスターを削除した後に、同じ Hive メタストアを使用してクラスターを再作成すると、元のクラスターで作成していた Hive テーブルがあることがわかります。

## 高度な構成オプション

>[AZURE.NOTE] 現在、このセクションは、Windows ベースの HDInsight クラスターにのみ適用されます。

### HDInsight クラスターのカスタマイズを使用したクラスターのカスタマイズ

構成ファイルを構成することが必要な場合があります。 次のような構成ファイルがあります。

- core-site.xml
- hdfs-site.xml
- mapred-site.xml
- yarn-site.xml
- hive-site.xml
- oozie-site.xml

再イメージ化により、クラスターは変更を保持できません。 詳細については、次を参照してください。 [によるロール インスタンスが再起動 OS のアップグレード](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx)します。 クラスターの有効期間中に変更を保持するには、プロビジョニング プロセスで HDInsight クラスターのカスタマイズを使用します。

Hive 構成をカスタマイズする Azure PowerShell スクリプトの例を次に示します。

    # hive-site.xml configuration 
    $hiveConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightHiveConfiguration'
    $hiveConfigValues.Configuration = @{ "hive.metastore.client.socket.timeout"="90" } #default 60
    
    $config = New-AzureHDInsightClusterConfig `
                -ClusterSizeInNodes $clusterSizeInNodes `
                -ClusterType $clusterType `
              | Set-AzureHDInsightDefaultStorage `
                -StorageAccountName $defaultStorageAccount `
                -StorageAccountKey $defaultStorageAccountKey `
                -StorageContainerName $defaultBlobContainer `
              | Add-AzureHDInsightConfigValues `
                -Hive $hiveConfigValues 
    
    New-AzureHDInsightCluster -Name $clusterName -Location $location -Credential $credential -OSType Windows -Config $config

他の構成ファイルのカスタマイズのサンプルを次に示します。

    # hdfs-site.xml configuration
    $HdfsConfigValues = @{ "dfs.blocksize"="64m" } #default is 128MB in HDI 3.0 and 256MB in HDI 2.1
    
    # core-site.xml configuration
    $CoreConfigValues = @{ "ipc.client.connect.max.retries"="60" } #default 50
    
    # mapred-site.xml configuration
    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'
    $MapRedConfigValues.Configuration = @{ "mapreduce.task.timeout"="1200000" } #default 600000
    
    # oozie-site.xml configuration
    $OozieConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightOozieConfiguration'
    $OozieConfigValues.Configuration = @{ "oozie.service.coord.normal.default.timeout"="150" }  # default 120

詳細については、Azim Uddin のブログ「を参照してください。 [をカスタマイズする HDInsight クラスターのプロビジョニング](http://blogs.msdn.com/b/bigdatasupport/archive/2014/04/15/customizing-hdinsight-cluster-provisioning-via-powershell-and-net-sdk.aspx)します。




### スクリプト アクションを使用したクラスターのカスタマイズ

追加コンポーネントをインストールするか、プロビジョニング中にスクリプトを使用してクラスターの構成をカスタマイズできます。 このようなスクリプトは、**スクリプト操作**を使用して実行します。これは Azure クラシック ポータル、HDInsight Windows PowerShell コマンドレット、HDInsight .NET SDK で使用できる構成オプションです。 詳細については、次を参照してください。 [HDInsight クラスターのカスタマイズ Script Action を使って](hdinsight-hadoop-customize-cluster.md)します。


### Azure Virtual Network の使用

[Azure Virtual Network](http://azure.microsoft.com/documentation/services/virtual-network/) ソリューションに必要なリソースを含む、セキュリティで保護された永続的なネットワークを作成することができます。 仮想ネットワークでは、次のことが可能になります。

* プライベート ネットワーク (クラウドのみ) 内でのクラウド リソース間の接続

    ![クラウドのみの構成の図](./media/hdinsight-provision-clusters-v1/hdinsight-vnet-cloud-only.png)

* 仮想プライベート ネットワーク (VPN) を使用したローカル データセンター ネットワークへのクラウド リソースの接続 (サイト間またはポイント対サイト)

    サイト間構成では、ハードウェア VPN を使用するか、ルーティングとリモート アクセス サービスを使用して、データセンターから複数のリソースを Azure 仮想ネットワークに接続できます。

    ![サイト間構成の図](./media/hdinsight-provision-clusters-v1/hdinsight-vnet-site-to-site.png)

    ポイント対サイト構成では、ソフトウェア VPN を使用して、特定のリソースを Azure 仮想ネットワークに接続できます。

    ![ポイント対サイト構成の図](./media/hdinsight-provision-clusters-v1/hdinsight-vnet-point-to-site.png)

仮想ネットワーク機能、利点、および機能の詳細については、次を参照してください。、 [Azure 仮想ネットワークの概要](../virtual-network/virtual-networks-overview.md)します。
> [AZURE.NOTE] HDInsight クラスターをプロビジョニングする前に、Azure 仮想ネットワークを作成する必要があります。 詳細については、次を参照してください。 [仮想ネットワークの構成タスク](../services/virtual-machines/)します。
>
> Azure HDInsight は場所ベースの Virtual Networks のみをサポートし、アフィニティ グループ ベースの Virtual Networks は現在取り扱っていません。 既存の Azure Virtual Network が場所ベースかどうかを確認するには、Azure PowerShell コマンドレットの Get-AzureVNetConfig を使用します。 Virtual Network が場所ベースでない場合、次のオプションがあります。
>
> -既存の仮想ネットワークの構成をエクスポートし、新しい仮想ネットワークを作成します。 すべての新しい仮想ネットワークは、既定で場所ベースです。
> -場所ベースの仮想ネットワークに移行します。 参照してください [リージョン スコープへの既存のサービスの移行](http://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/)します。
>
> 1 つのクラスターには単一のサブネットを指定することを強くお勧めします。

## プロビジョニング ツール

- Azure クラシック ポータル
- Azure PowerShell
- .NET SDK
- CLI

### Azure クラシック ポータルの使用

各フィールドについては、「基本的な構成オプション」および「高度な構成オプション」をご覧ください。

**カスタム作成オプションを使用して HDInsight クラスターを作成するには**

1. サインイン、 [Azure Classic Portal ][azure-management-portal]します。
2. ページ下部の **[+ 新規]** をクリックし、**[DATA SERVICES]**、**[HDINSIGHT]**、**[カスタム作成]** の順にクリックします。
3. **[クラスターの詳細]** ページで、次の値を入力または選択します。

    - クラスター名
    - サブスクリプション名
    - クラスターの種類
    - オペレーティング システム
    - HDInsight のバージョン

4. **[クラスターの構成]** ページで、次の値を入力または選択します。

    - データ ノード
    - リージョン/Virtual Network
    - ヘッド ノード サイズ
    - データ ノード サイズ

5. **[クラスター ユーザーの構成]** ページで、次の情報を指定します。

    - HTTP ユーザー名
    - HTTP パスワード / パスワードの確認
    - Enable remote desktop for cluster (クラスターのリモート デスクトップ アクセスを有効にする)
    - Hive/Oozie メタストアの入力

6. 前の画面で Hive/Oozie メタストアを入力することを選択した場合、**[Hive/Oozie メタストアの構成]** ページで次の値を指定します。

    - Hive メタストア データベース
    - データベース ユーザー
    - データベース ユーザー パスワード
    - Oozie メタストア データベース
    - データベース ユーザー
    - データベース ユーザー パスワード

7. **[ストレージ アカウント]** ページで、次の値を指定します。

    - ストレージ アカウント
    - アカウント名
    - 既定のコンテナー
    - 追加のストレージ アカウント

8. **[スクリプトのアクション]** ページで　**[スクリプト アクションの追加]** をクリックし、クラスターが作成される際に、クラスターのカスタマイズを実行するカスタム スクリプトに関する詳細を指定します。 詳細については、次を参照してください。 [をカスタマイズする HDInsight クラスターの Script Action を使って](hdinsight-hadoop-customize-cluster.md)します。

    - 名前: スクリプト アクションの名前を指定します。
    - スクリプト URI: クラスターをカスタマイズするために呼び出すスクリプトの URI (Uniform Resource Identifier) を指定します。
    - ノードの種類: カスタマイズ スクリプトが実行されるノードを指定します。 選択することができます <b>のすべてのノード</b>, 、<b>ヘッド ノードのみ</b>, 、または <b>ワーカー ノードのみ</b>します。
    - パラメーター: は、スクリプトで必要な場合に、パラメーターを指定します。</td></tr>

    複数のスクリプト操作を追加して、クラスターに複数のコンポーネントをインストールすることができます。 スクリプトを追加した後、チェック マークをオンにして、クラスターのプロビジョニングを開始します。

### Azure PowerShell の使用

Azure PowerShell は、Azure のワークロードのデプロイと管理を制御し自動化するために使用できる強力なスクリプティング環境です。 このセクションでは、Azure PowerShell を使用して HDInsight クラスターをプロビジョニングする方法について説明します。 HDInsight Windows PowerShell コマンドレットを実行するワークステーションの構成方法の詳細については、次を参照してください。 [をインストールし、Azure PowerShell を構成](../install-configure-powershell.md)します。 HDInsight で Azure PowerShell の使用の詳細については、次を参照してください。 [PowerShell を使用した HDInsight の管理](hdinsight-administer-use-powershell.md)します。 HDInsight Windows PowerShell コマンドレットの一覧で、次を参照してください。 [HDInsight コマンドレット リファレンス](https://msdn.microsoft.com/library/azure/dn858087.aspx)します。
> [AZURE.NOTE] このセクションのスクリプトは、Azure 仮想ネットワークの HDInsight クラスターの構成に使用できますが、Azure 仮想ネットワークは作成されません。 Azure の仮想ネットワークを作成する方法の詳細については、次を参照してください。 [仮想ネットワークの構成タスク](../services/virtual-machines/)します。

Azure PowerShell を使用して HDInsight クラスターをプロビジョニングするには、以下の手順が必要です。

- Azure のストレージ アカウントの作成
- Azure BLOB コンテナーの作成
- HDInsight クラスターの作成

Windows PowerShell コンソールまたは Windows PowerShell Integrated Scripting Environment (ISE) のいずれかを使用して、スクリプトを実行できます。

HDInsight は、既定のファイル システムとして Azure BLOB ストレージ コンテナーを使用します。 HDInsight クラスターを作成するには Azure ストレージ アカウントとストレージ コンテナーが必要です。 ストレージ アカウントは、HDInsight クラスターと同じデータ センターに置く必要があります。

**Azure アカウントに接続するには**

    Add-AzureAccount

Azure アカウント資格情報の入力を求められます。

**Azure ストレージ アカウントを作成するには**

    $storageAccountName = "<StorageAcccountName>"   # Provide a Storage account name
    $location = "<MicrosoftDataCenter>"             # For example, "West US"
    
    # Create an Azure Storage account
    New-AzureStorageAccount -StorageAccountName $storageAccountName -Location $location

既にストレージ アカウントを持っていて、アカウント名とアカウント キーがわからない場合は、次の Windows PowerShell コマンドを使ってその情報を取得できます。

    # List Storage accounts for the current subscription
    Get-AzureStorageAccount
    
    # List the keys for a Storage account
    Get-AzureStorageKey "<StorageAccountName>"

**Azure BLOB ストレージ コンテナーを作成するには**

    $storageAccountName = "<StorageAccountName>"    # Provide the Storage account name
    $containerName="<ContainerName>"                # Provide a container name
    
    # Create a storage context object
    $storageAccountKey = Get-AzureStorageKey $storageAccountName | %{ $_.Primary }
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName
                                           -StorageAccountKey $storageAccountKey  
    
    # Create a Blob storage container
    New-AzureStorageContainer -Name $containerName -Context $destContext

ストレージ アカウントを用意して、BLOB コンテナーを準備したら、クラスターを作成する準備は整いました。

**HDInsight クラスターをプロビジョニングするには**
> [AZURE.NOTE] Azure PowerShell コマンドレットは、HDInsight クラスターの構成変数を変更する方法として唯一お勧めする方法です。 リモート デスクトップ経由でクラスターに接続している間に Hadoop 構成ファイルに加えられた変更は、クラスター パッチの際に上書きされる可能性があります。 Azure PowerShell 経由で設定された構成値は、クラスターにパッチが適用された場合でも保持されます。

- 次のコマンドを Azure PowerShell コンソール ウィンドウから実行します。

      $subscriptionName = "<AzureSubscriptionName>"     # The Azure subscription used for the HDInsight cluster to be created
    
      $storageAccountName = "<AzureStorageAccountName>" # HDInsight cluster requires an existing Azure Storage account to be used as the default file system
    
      $clusterName = "<HDInsightClusterName>"           # The name for the HDInsight cluster to be created
      $clusterNodes = <ClusterSizeInNodes>              # The number of nodes in the HDInsight cluster
      $hadoopUserName = "<HadoopUserName>"              # User name for the Hadoop user. You will use this account to connect to the cluster and run jobs.
      $hadoopUserPassword = "<HadoopUserPassword>"
    
      $secPassword = ConvertTo-SecureString $hadoopUserPassword -AsPlainText -Force
      $credential = New-Object System.Management.Automation.PSCredential($hadoopUserName,$secPassword)
    
      # Get the storage primary key based on the account name
      Select-AzureSubscription $subscriptionName
      $storageAccountKey = Get-AzureStorageKey $storageAccountName | %{ $_.Primary }
      $containerName = $clusterName               # Azure Blob container that is used as the default file system for the HDInsight cluster
    
      # The location of the HDInsight cluster. It must be in the same data center as the Storage account.
      $location = Get-AzureStorageAccount -StorageAccountName $storageAccountName | %{$_.Location}
    
      # Create a new HDInsight cluster
      New-AzureHDInsightCluster -Name $clusterName -Credential $credential -Location $location -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" -DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainerName $containerName  -ClusterSizeInNodes $clusterNodes -ClusterType Hadoop

  >[AZURE.NOTE] $hadoopUserName コマンドと $hadoopUserPassword コマンドは、クラスター用 Hadoop ユーザー アカウントの作成に使用されます。 このアカウントを使用してクラスターに接続し、ジョブを実行します。 Azure クラシック ポータルから [簡易作成] オプションを使用してクラスターをプロビジョニングする場合は、既定の Hadoop ユーザー名は "admin" になります。 このアカウントと、リモート デスクトップ プロトコル (RDP) のユーザー アカウントとを混同しないでください。 RDP ユーザー アカウントは Hadoop ユーザー アカウントとは異なっている必要があります。 詳細については、次を参照してください。 [Azure Classic Portal ][hdinsight-admin-portal]します。

  クラスターのプロビジョニングが完了するまでに数分かかる場合があります。

  ![HDI.CLI.Provision][image-hdi-ps-provision]

**カスタム構成オプションを使用して HDInsight クラスターをプロビジョニングするには**

クラスターをプロビジョニングするときに、複数の Azure BLOB ストレージ コンテナーへの接続、仮想ネットワークの使用、Hive メタストアおよび Oozie メタストア用の Azure SQL データベースの使用など、その他の構成オプションを使用できます。 これにより、データとメタデータの寿命を、クラスターの寿命と切り離すことができます。
> [AZURE.NOTE] Windows PowerShell コマンドレットは、HDInsight クラスターの構成変数を変更する方法として唯一お勧めする方法です。 リモート デスクトップ経由でクラスターに接続している間に Hadoop 構成ファイルに加えられた変更は、クラスター パッチの際に上書きされる可能性があります。 Azure PowerShell 経由で設定された構成値は、クラスターにパッチが適用された場合でも保持されます。

- 次のコマンドを Windows PowerShell ウィンドウから実行します。

      $subscriptionName = "<SubscriptionName>"
      $clusterName = "<ClusterName>"
      $location = "<MicrosoftDataCenter>"
      $clusterNodes = <ClusterSizeInNodes>
    
      $storageAccountName_Default = "<DefaultFileSystemStorageAccountName>"
      $containerName_Default = "<DefaultFileSystemContainerName>"
    
      $storageAccountName_Add1 = "<AdditionalStorageAccountName>"
    
      $hiveSQLDatabaseServerName = "<SQLDatabaseServerNameForHiveMetastore>"
      $hiveSQLDatabaseName = "<SQLDatabaseDatabaseNameForHiveMetastore>"
      $oozieSQLDatabaseServerName = "<SQLDatabaseServerNameForOozieMetastore>"
      $oozieSQLDatabaseName = "<SQLDatabaseDatabaseNameForOozieMetastore>"
    
      # Get the virtual network ID and subnet name
      $vnetID = "<AzureVirtualNetworkID>"
      $subNetName = "<AzureVirtualNetworkSubNetName>"
    
      # Get the Storage account keys
      Select-AzureSubscription $subscriptionName
      $storageAccountKey_Default = Get-AzureStorageKey $storageAccountName_Default | %{ $_.Primary }
      $storageAccountKey_Add1 = Get-AzureStorageKey $storageAccountName_Add1 | %{ $_.Primary }
    
      $oozieCreds = Get-Credential -Message "Oozie metastore"
      $hiveCreds = Get-Credential -Message "Hive metastore"
    
      # Create a Blob storage container
      $dest1Context = New-AzureStorageContext -StorageAccountName $storageAccountName_Default -StorageAccountKey $storageAccountKey_Default  
      New-AzureStorageContainer -Name $containerName_Default -Context $dest1Context
    
      # Create a new HDInsight cluster
      $config = New-AzureHDInsightClusterConfig -ClusterSizeInNodes $clusterNodes |
          Set-AzureHDInsightDefaultStorage -StorageAccountName "$storageAccountName_Default.blob.core.windows.net" -StorageAccountKey $storageAccountKey_Default -StorageContainerName $containerName_Default |
          Add-AzureHDInsightStorage -StorageAccountName "$storageAccountName_Add1.blob.core.windows.net" -StorageAccountKey $storageAccountKey_Add1 |
          Add-AzureHDInsightMetastore -SqlAzureServerName "$hiveSQLDatabaseServerName.database.windows.net" -DatabaseName $hiveSQLDatabaseName -Credential $hiveCreds -MetastoreType HiveMetastore |
          Add-AzureHDInsightMetastore -SqlAzureServerName "$oozieSQLDatabaseServerName.database.windows.net" -DatabaseName $oozieSQLDatabaseName -Credential $oozieCreds -MetastoreType OozieMetastore |
              New-AzureHDInsightCluster -Name $clusterName -Location $location -VirtualNetworkId $vnetID -SubnetName $subNetName

  >[AZURE.NOTE] メタストアに使用される Azure SQL Database は、Azure HDInsight などの他の Azure サービスに接続できる必要があります。 Azure SQL データベース ダッシュボードの右側に表示されているサーバー名をクリックします。 これは、SQL Database インスタンスが実行されているサーバーです。 サーバー ビューが表示されたら、**[構成]** をクリックします。**[Azure サービス]** に対して **[はい]** をクリックし、**[保存]** をクリックします。

**HDInsight クラスターの一覧を表示するには**

- 次のコマンドを Azure PowerShell コンソール ウィンドウから実行して HDInsight クラスターを一覧表示し、クラスターが正常にプロビジョニングされたことを確認します。

        Get-AzureHDInsightCluster -Name <ClusterName>



### Azure CLI の使用

> [AZURE.NOTE] 2014 年 8 月 29 日の時点では、クラスターと Azure 仮想ネットワーク間の関連付けに Azure CLI を使用することはできません。

HDInsight クラスターをプロビジョニングするもう 1 つの方法は、Azure CLI を使用する方法です。 Azure CLI は Node.js で実装されます。 Windows、Mac、Linux など、Node.js をサポートするいずれのプラットフォームでも使用できます。 CLI は、次の場所からインストールできます。

- **Node.js SDK** - <a href="https://www.npmjs.com/package/azure-mgmt-hdinsight" target="_blank">https://www.npmjs.com/package/azure-mgmt-hdinsight</a>
- **Azure CLI** - <a href="https://github.com/azure/azure-xplat-cli/archive/hdinsight-February-18-2015.tar.gz" target="_blank">https://github.com/azure/azure-xplat-cli/archive/hdinsight-February-18-2015.tar.gz</a>

Azure CLI を使用する方法の一般的なガイドラインを参照してください。 [Mac、Linux および Windows 用 Azure CLI](../xplat-cli-install.md)します。

ここでは、Azure CLI を Linux および Windows にインストールする手順と、コマンド ラインを使用してクラスターをプロビジョニングする手順について説明します。

- [Linux 用 Azure CLI を設定します。](#clilin)
- [Windows 向け Azure CLI を設定します。](#cliwin)
- [Azure CLI を使用した HDInsight クラスターをプロビジョニングします。](#cliprovision)

#### <a id="clilin"></a>Linux 用 Azure CLI を設定します。

次の手順に従って、Azure コマンド ライン インターフェイス (Azure CLI) を使用できるように Linux コンピューターをセットアップします。

- Node.js パッケージ マネージャー (NPM) を使用して Azure CLI をインストールする
- Azure サブスクリプションへの接続

**NPM を使用して Azure CLI をインストールするには**

1.  Linux コンピューターでターミナル ウィンドウを開き、次のコマンドを実行します。

        sudo npm install -g https://github.com/azure/azure-xplat-cli/archive/hdinsight-February-18-2015.tar.gz

2.  次のコマンドを実行してインストールを確認します。

        azure hdinsight -h

    *-h* スイッチを使うと、さまざまなレベルでヘルプ情報を表示できます。 次に例を示します。

        azure -h
        azure hdinsight -h
        azure hdinsight cluster -h
        azure hdinsight cluster create -h


**Azure サブスクリプションに接続するには**

Azure CLI を使用する前に、ワークステーションと Azure の間の接続を構成する必要があります。 Azure CLI は、Azure のサブスクリプション情報を使用してアカウントに接続します。 この情報は、Azure から発行設定ファイルとして入手できます。 発行設定ファイルは、永続的なローカル構成設定としてインポートできます。インポートすると、Azure CLI のそれ以降の操作に使用されるようになります。 発行設定のインポートは 1 回だけ行う必要があります。
> [AZURE.NOTE] 発行設定ファイルには、機密情報が含まれます。 Microsoft では、このファイルを削除するか、追加の作業を行ってファイルのある user フォルダーを暗号化することをお勧めします。 Windows の場合、フォルダー プロパティを変更するか、BitLocker Drive Encryption を使用します。


1.  ターミナル ウィンドウを開きます。
2.  次のコマンドを実行して、Azure サブスクリプションにログインします。

        azure account download

    ![HDI.Linux.CLIAccountDownloadImport](./media/hdinsight-provision-clusters/HDI.Linux.CLIAccountDownloadImport.png)

    このコマンドは、発行設定ファイルのダウンロード元の Web ページを起動します。 Web ページが開いていない場合は、ターミナル ウィンドウのリンクをクリックしてブラウザー ページを開き、ポータルにログインします。

3.  発行設定ファイルをコンピューターにダウンロードします。
5.  コマンド プロンプト ウィンドウで次のコマンドを実行して、発行設定ファイルをインポートします。

        azure account import <path/to/the/file>



#### <a id="cliwin"></a>Windows 向け Azure CLI を設定します。

次の手順に従って、Azure CLI を使用するように Windows ベースのコンピューターを設定します。

- Azure CLI のインストール (NPM または Windows インストーラーを使用)
- Azure アカウントの発行設定のダウンロードとインポート


Azure CLI は NPM または Windows インストーラーを使用してインストールできます。 Microsoft では、この 2 つのオプションのいずれかを使用してインストールすることをお勧めします。

**NPM を使用して Azure CLI をインストールするには**

1.  ブラウザーで **www.nodejs.org** を開きます。
2.  **[INSTALL]** をクリックし、指示に従います。設定は既定の設定を使います。
3.  コンピューターから**コマンド プロンプト** (または *Azure コマンド プロンプト*、または *VS2012 の開発者コマンド プロンプト*) を開きます。
4.  コマンド プロンプト ウィンドウで次のコマンドを実行します。

        npm install -g https://github.com/azure/azure-xplat-cli/archive/hdinsight-February-18-2015.tar.gz

    > [AZURE.NOTE] NPM コマンドが見つからないというエラーが発生した場合は、次のパスが PATH 環境変数にあることを確認します <i>C:\Program Files (x86) \nodejs;。C:\Users\[username]\AppData\Roaming\npm</i> または <i>C:\Program \nodejs;C:\Users\[username]\AppData\Roaming\npm</i>

5.  次のコマンドを実行してインストールを確認します。

        azure hdinsight -h

    *-h* スイッチを使うと、さまざまなレベルでヘルプ情報を表示できます。 次に例を示します。

        azure -h
        azure hdinsight -h
        azure hdinsight cluster -h
        azure hdinsight cluster create -h


**Windows インストーラーを使用して Azure CLI をインストールするには**

1.  参照 **http://azure.microsoft.com/downloads/**します。
2.  下へスクロールして、**[コマンド ライン ツール]** セクションの **[Azure コマンド ライン インターフェイス]** をクリックし、Web プラットフォーム インストーラーのウィザードの指示に従います。

**発行設定をダウンロードしてインポートするには**

Azure CLI を使用する前に、ワークステーションと Azure の間の接続を構成する必要があります。 Azure CLI は、Azure のサブスクリプション情報を使用してアカウントに接続します。 この情報は、Azure から発行設定ファイルとして入手できます。 発行設定ファイルは、永続的なローカル構成設定としてインポートできます。インポートすると、Azure CLI のそれ以降の操作に使用されるようになります。 発行設定のインポートは 1 回だけ行う必要があります。
> [AZURE.NOTE] 発行設定ファイルには、機密情報が含まれます。 Microsoft では、このファイルを削除するか、追加の作業を行ってファイルのある user フォルダーを暗号化することをお勧めします。 Windows の場合、フォルダー プロパティを変更するか、または BitLocker を使用します。


1.  **コマンド プロンプト**を開きます。
2.  次のコマンドを実行して、発行設定ファイルをダウンロードします。

        azure account download

    ![HDI.CLIAccountDownloadImport][image-cli-account-download-import]

    このコマンドは、発行設定ファイルのダウンロード元の Web ページを起動します。

3.  ファイルを保存するためのプロンプトで、**[保存]** をクリックし、ファイルの保存先となる場所を指定します。
5.  コマンド プロンプト ウィンドウで次のコマンドを実行して、発行設定ファイルをインポートします。

        azure account import <path/to/the/file>


#### <a id="cliprovision"></a>Azure CLI を使用した HDInsight クラスターをプロビジョニングします。

Azure CLI を使用して HDInsight クラスターをプロビジョニングするには、以下の手順を実行する必要があります。

- Azure のストレージ アカウントの作成
- クラスターのプロビジョニング

**Azure ストレージ アカウントを作成するには**

HDInsight は、既定のファイル システムとして Azure BLOB ストレージ コンテナーを使用します。 HDInsight クラスターを作成するには Azure ストレージ アカウントが必要です。 ストレージ アカウントは、クラスターと同じデータ センターに配置する必要があります。

- コマンド プロンプト ウィンドウで次のコマンドを実行し、Azure ストレージ アカウントを作成します。

        azure storage account create [options] <StorageAccountName>

    場所を指定するよう求められたら、HDInsight クラスターをプロビジョニングできる場所を選択します。 このストレージは、HDInsight クラスターと同じ場所にある必要があります。 現在、HDInsight クラスターをホストできるリージョンは、**東アジア**、**東南アジア**、**北ヨーロッパ**、**西ヨーロッパ**、**米国東部**、**米国西部**、**米国中北部**、および**米国中南部**のみです。

Azure クラシック ポータルを使用して Azure ストレージ アカウントの作成方法の詳細については、次を参照してください。 [の作成、管理、またはストレージ アカウントを削除](../storage-create-storage-account.md)します。

既にストレージ アカウントを持っていて、アカウント名とアカウント キーがわからない場合は、次のコマンドを使ってその情報を取得できます。

    -- Lists Storage accounts
    azure storage account list
    
    -- Shows information for a Storage account
    azure storage account show <StorageAccountName>
    
    -- Lists the keys for a Storage account
    azure storage account keys list <StorageAccountName>

Azure クラシック ポータルを使用して情報を取得の詳細については、「、 *する方法: 表示、コピーおよび再生成するストレージ アクセス キー* の [の作成、管理、またはストレージ アカウントを削除](../storage-create-storage-account.md)します。

HDInsight クラスターでは、ストレージ アカウント内にコンテナーも必要です。 指定するストレージ アカウントにまだコンテナーがない場合は、*azure hdinsight cluster create* コマンドにより、コンテナー名を指定するよう求められ、コンテナーも作成されます。 ただし、コンテナーを事前に作成する場合は、次のコマンドを使用できます。

    azure storage container create --account-name <StorageAccountName> --account-key <StorageAccountKey> [ContainerName]

ストレージ アカウントを用意して、BLOB コンテナーを準備したら、クラスターを作成する準備は整いました。

**HDInsight クラスターをプロビジョニングするには**

- コマンド プロンプト ウィンドウで次のコマンドを実行します。

        azure hdinsight cluster create --clusterName <ClusterName> --storageAccountName "<StorageAccountName>.blob.core.windows.net" --storageAccountKey <storageAccountKey> --storageContainer <StorageContainerName> --dataNodeCount <NumberOfNodes> --location <DataCenterLocation> --userName <HDInsightClusterUsername> --password <HDInsightClusterPassword> --osType windows

    ![HDI.CLIClusterCreation][image-cli-clustercreation]


**構成ファイルを使用して HDInsight クラスターをプロビジョニングするには**

通常は、HDInsight クラスターをプロビジョニングして、ジョブを実行した後、クラスターを削除してコストを削減します。 Azure CLI を使用すると、構成をファイルに保存し、クラスターをプロビジョニングするたびにその構成を再利用することができます。

- コマンド プロンプト ウィンドウで次のコマンドを実行します。

      #Create the config file
      azure hdinsight cluster config create <file>
    
      #Add commands to create a basic cluster
      azure hdinsight cluster config set <file> --clusterName <ClusterName> --dataNodeCount <NumberOfNodes> --location "<DataCenterLocation>" --storageAccountName "<StorageAccountName>.blob.core.windows.net" --storageAccountKey "<StorageAccountKey>" --storageContainer "<BlobContainerName>" --userName "<Username>" --password "<UserPassword>" --osType windows
    
      #If required, include commands to use additional Blob storage with the cluster
      azure hdinsight cluster config storage add <file> --storageAccountName "<StorageAccountName>.blob.core.windows.net"
             --storageAccountKey "<StorageAccountKey>"
    
      #If required, include commands to use a SQL database as a Hive metastore
      azure hdinsight cluster config metastore set <file> --type "hive" --server "<SQLDatabaseName>.database.windows.net"
             --database "<HiveDatabaseName>" --user "<Username>" --metastorePassword "<UserPassword>"
    
      #If required, include commands to use a SQL database as an Oozie metastore
      azure hdinsight cluster config metastore set <file> --type "oozie" --server "<SQLDatabaseName>.database.windows.net"
             --database "<OozieDatabaseName>" --user "<SQLUsername>" --metastorePassword "<SQLPassword>"
    
      #Run this command to create a cluster by using the config file
      azure hdinsight cluster create --config <file>

  >[AZURE.NOTE] メタストアに使用される Azure SQL Database は、Azure HDInsight などの他の Azure サービスに接続できる必要があります。 Azure SQL データベース ダッシュボードの右側に表示されているサーバー名をクリックします。 これは、SQL Database インスタンスが実行されているサーバーです。 サーバー ビューが表示されたら、**[構成]** をクリックします。**[Azure サービス]** に対して **[はい]** をクリックし、**[保存]** をクリックします。

  ![HDI.CLIClusterCreationConfig][image-cli-clustercreation-config]


**クラスターの一覧と詳細を表示するには**

- クラスターの一覧と詳細を表示するには、次のコマンドを使用します。

        azure hdinsight cluster list
        azure hdinsight cluster show <ClusterName>

    ![HDI.CLIListCluster][image-cli-clusterlisting]


**クラスターを削除するには**

- クラスターを削除するには、次のコマンドを使用します。

        azure hdinsight cluster delete <ClusterName>




### HDInsight .NET SDK の使用

HDInsight .NET SDK は、.NET Framework アプリケーションから HDInsight を簡単に操作できる .NET クライアント ライブラリを提供します。

SDK を使用して HDInsight クラスターをプロビジョニングするには、以下の手順を実行する必要があります。

- HDInsight .NET SDK のインストール
- 自己署名証明書の作成
- コンソール アプリケーションの作成
- アプリケーションの実行


**HDInsight .NET SDK をインストールするには**

SDK からの公開された最新のビルドをインストールする [NuGet](http://nuget.codeplex.com/wikipage?title=Getting%20Started)します。 次の手順で、具体的な方法を説明します。

**自己署名証明書を取得するには**

自己署名証明書を作成し、それをコンピューターにインストールして、さらに、Azure サブスクリプションにアップロードします。 手順については、次を参照してください。 [自己署名証明書を作成する](http://go.microsoft.com/fwlink/?LinkId=511138)します。


**Visual Studio コンソール アプリケーションを作成するには**

1. Visual Studio 2013 または 2015 を開きます。

2. **[ファイル]** メニューの **[新規作成]** をクリックし、**[プロジェクト]** をクリックします。

3. **[新しいプロジェクト]** で、次の値を入力するか、選択します。

   | プロパティ| 値|
   |--------|-----|
   | テンプレート| テンプレート/Visual C#/Windows/コンソール アプリケーション|
   | 名前| CreateHDICluster|

4. **[OK]** をクリックしてプロジェクトを作成します。

5. **[ツール]** メニューで **[NuGet パッケージ マネージャー]**、**[パッケージ マネージャー コンソール]** の順にクリックします。

6. コンソールで次のコマンドを実行して、パッケージをインストールします。

        Install-Package Microsoft.Azure.Common.Authentication -pre
        Install-Package Microsoft.Azure.Management.HDInsight -Pre

    これらのコマンドは、.NET ライブラリおよび .NET ライブラリへの参照を現在の Visual Studio プロジェクトに追加します。

7. ソリューション エクスプローラーで **Program.cs** をダブルクリックして、このファイルを開きます。
8. コードを次のコードに置き換えます。

     using Microsoft.Azure;
     using Microsoft.Azure.Common.Authentication;
     using Microsoft.Azure.Common.Authentication.Factories;
     using Microsoft.Azure.Common.Authentication.Models;
     using Microsoft.Azure.Management.HDInsight;
     using Microsoft.Azure.Management.HDInsight.Models;
    
     namespace CreateHDICluster
     {
         internal class Program
         {
             private static HDInsightManagementClient _hdiManagementClient;
    
             private static Guid SubscriptionId = new Guid("<AZURE SUBSCRIPTION ID>");
             private const string ResourceGroupName = "<AZURE RESOURCEGROUP NAME>";
    
             private const string NewClusterName = "<HDINSIGHT CLUSTER NAME>";
             private const int NewClusterNumNodes = <NUMBER OF NODES>;
             private const string NewClusterLocation = "<LOCATION>";  // Must match the Azure Storage account location
             private const HDInsightClusterType NewClusterType = HDInsightClusterType.Hadoop;
             private const OSType NewClusterOSType = OSType.Windows;
             private const string NewClusterVersion = "3.2";
    
             private const string NewClusterUsername = "admin";
             private const string NewClusterPassword = "<HTTP USER PASSWORD>";
             private const string ExistingStorageName = "<STORAGE ACCOUNT NAME>.blob.core.windows.net";
             private const string ExistingStorageKey = "<STORAGE ACCOUNT KEY>";
             private const string ExistingContainer = "<DEFAULT CONTAINER NAME>"; 
    
    
             private static void Main(string[] args)
             {
                 var tokenCreds = GetTokenCloudCredentials();
                 var subCloudCredentials = GetSubscriptionCloudCredentials(tokenCreds, SubscriptionId);
    
                 _hdiManagementClient = new HDInsightManagementClient(subCloudCredentials);
    
                 CreateCluster();
             }
    
             public static SubscriptionCloudCredentials GetTokenCloudCredentials(string username = null, SecureString password = null)
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
    
             public static SubscriptionCloudCredentials GetSubscriptionCloudCredentials(SubscriptionCloudCredentials creds, Guid subId)
             {
                 return new TokenCloudCredentials(subId.ToString(), ((TokenCloudCredentials)creds).Token);
             }
    
    
             private static void CreateCluster()
             {
                 var parameters = new ClusterCreateParameters
                 {
                     ClusterSizeInNodes = NewClusterNumNodes,
                     Location = NewClusterLocation,
                     ClusterType = NewClusterType,
                     OSType = NewClusterOSType,
                     Version = NewClusterVersion,
    
                     UserName = NewClusterUsername,
                     Password = NewClusterPassword,
    
                     DefaultStorageAccountName = ExistingStorageName,
                     DefaultStorageAccountKey = ExistingStorageKey,
                     DefaultStorageContainer = ExistingContainer
                 };
    
                 _hdiManagementClient.Clusters.Create(ResourceGroupName, NewClusterName, parameters);
             }
         }
     }

10. クラス メンバーの値を置き換えます。

**アプリケーションを実行するには**

アプリケーションを Visual Studio で開いている間に、**F5** キーを押してアプリケーションを実行します。 コンソール ウィンドウが開き、アプリケーションの状態が表示されます。 HDInsight クラスターの作成は数分かかる場合があります。


## <a id="nextsteps"></a> 次のステップ

この記事では、HDInsight クラスターをプロビジョニングする方法をいくつか説明しました。 詳細については、次の記事を参照してください。

* [Azure HDInsight の概要](hdinsight-get-started.md) -HDInsight クラスターの使用を開始する方法について説明
* [HDInsight での Sqoop の使用](hdinsight-use-sqoop.md) -HDInsight と SQL Database または SQL Server の間でデータをコピーする方法について
* [PowerShell を使用した HDInsight の管理](hdinsight-administer-use-powershell.md) -Azure PowerShell を使用して HDInsight を操作する方法について説明
* [プログラムによる Hadoop の送信ジョブ](hdinsight-submit-hadoop-jobs-programmatically.md) -プログラムを使用して HDInsight にジョブを送信する方法について説明
* [Azure HDInsight SDK のドキュメント ][hdinsight-sdk-documentation] -HDInsight SDK の検出



[hdinsight-sdk-documentation]: http://msdn.microsoft.com/library/dn479185.aspx 
[azure-management-portal]: https://manage.windowsazure.com 

