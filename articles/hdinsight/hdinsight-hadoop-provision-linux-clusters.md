<properties
    pageTitle="HDInsight での Hadoop、HBase、Storm クラスター (Linux ベース) の作成 | Microsoft Azure"
    description="ブラウザー、Azure CLI、Azure PowerShell、REST を使用するか、SDK 経由で HDInsight に Linux ベースの Hadoop、HBase、または Storm クラスターを作成する方法について説明します。"
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="paulettm"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/23/2015"
    ms.author="nitinme"/>



# HDInsight での Linux ベース クラスターの作成

[AZURE.INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

このドキュメントでは、クラスターで使用できるオプションの構成に加え、Azure で Linux ベースの HDInsight クラスターを作成するさまざまな方法を学習します。 HDInsight は、Azure クラウド プラットフォーム上のサービスとして Apache Hadoop、Apache Storm、および Apache HBase を提供します。
> [AZURE.NOTE] このドキュメントでは、クラスターを作成するさまざまな手順を示します。 クラスターを作成するクイック スタートのアプローチを探している場合は、次を参照してください。 [Linux で Azure HDInsight の概要](../hdinsight-hadoop-linux-get-started.md)します。

## HDInsight クラスターについて

Hadoop クラスターは、クラスターでのタスクの分散処理に使用されるいくつかの仮想マシン (ノード) で構成されます。 Azure では個々のノードのインストールと構成の実装の詳細を抽象化しているため、提供する必要があるのは一般的な構成情報のみとなります。

### クラスターの種類

以下の複数の種類の HDInsight が使用可能です。

| クラスターの種類| 用途|
| ------------ | ----------------------------- |
| Hadoop| クエリと分析 (バッチ ジョブ)|
| HBase| NoSQL データ ストレージ|
| Storm| リアルタイム イベント処理|
| Spark (Windows のみのプレビュー)| メモリ内処理、対話型クエリ、マイクロバッチ ストリーム処理|

構成時に、クラスターに対してこれらの種類のいずれかを選択します。 色合い、Spark の場合、または R などその他のテクノロジをこれらの基本的な型に追加するにを使用して [Script Action](#scriptaction)します。

各クラスターの種類にはクラスター内のノードの独自の用語と、ノードの種類ごとにノード数および既定の VM サイズがあります。
> [AZURE.IMPORTANT] クラスター作成または作成後の拡大で 32 以上の worker ノードを予定している場合、コア数が 8 個以上で RAM が 14GB 以上のサイズのヘッド ノードを選択する必要があります。

![HDInsight Hadoop クラスター ノード](./media/hdinsight-provision-clusters/HDInsight.Hadoop.roles.png)

HDInsight 用 Hadoop クラスターには次の 2 種類のノードがあります。

- ヘッド ノード (2 ノード)
- データ ノード (1 つ以上のノード)

![HDInsight HBase クラスター ノード](./media/hdinsight-provision-clusters/HDInsight.HBase.roles.png)

HDInsight の HBase クラスターには、次の 3 つの種類のノードがあります。
- ヘッド サーバー (2 つのノード)
- リージョン サーバー (1 つ以上のノード)
- マスター/Zookeeper ノード (3 つのノード)

![HDInsight Storm クラスター ノード](./media/hdinsight-provision-clusters/HDInsight.Storm.roles.png)

HDInsight の storm クラスターでは、次の 3 つのノードの種類があります。
- Nimbus ノード (2 つのノード)
- Supervisor サーバー (1 つ以上のノード)
- Zookeeper ノード (3 つのノード)


![HDInsight Spark クラスター ノード](./media/hdinsight-provision-clusters/HDInsight.Spark.roles.png)

HDInsight の Spark クラスターには、次の 3 つの種類のノードがあります。
- ヘッド ノード (2 ノード)
- ワーカー ノード (1 つ以上のノード)
- Zookeeper ノード (3 ノード) (A1 Zookeeper VM サイズは無料)

### HDInsight 用 Azure Storage

各クラスターの種類には、クラスターに関連付けられている 1 つ以上の Azure Storage アカウントもあります。 HDInsight は、クラスターのデータ ストアとして、これらのストレージ アカウントの Azure BLOB を使用します。 データをクラスターから分離しておくことで、使用されていないクラスターを削除した場合でもデータを引き続き保持できます。 したがって、さらに分析が必要な場合に、新しいクラスターでも同じストレージ アカウントを使用できます。 詳細については、次を参照してください。 [Azure Blob ストレージの使用 HDInsight](../hdinsight-use-blob-storage.md)します。

## <a id="configuration"></a>基本的な構成オプション

次のセクションでは、HDInsight クラスターの作成時に使用できる必要な構成オプションについて説明します。

### クラスター名

クラスター名はクラスターの一意の識別子を示し、インターネット経由でクラスターにアクセスするためのドメイン名の一部として使用されます。 たとえば、_mycluster_ という名前のクラスターは、_mycluster_. azurehdinsight.net としてインターネット経由で使用できます。

クラスター名は、次のガイドラインに従う必要があります。

- このフィールドには、3 ～ 63 文字の文字列を含める必要があります。
- このフィールドには、文字、数字、ハイフンのみを含めることができます。

### クラスターの種類

クラスターの種類では、クラスターの特殊用途の構成を選択できます。 Linux ベースの HDInsight で使用可能な種類を以下に示します。

| クラスターの種類| 用途|
| ------------ | ----------------------------- |
| Hadoop| クエリと分析 (バッチ ジョブ)|
| HBase| NoSQL データ ストレージ|
| Storm| リアルタイム イベント処理|
| Spark (Windows のみのプレビュー)| メモリ内処理、対話型クエリ、マイクロバッチ ストリーム処理|

色合い、Spark の場合、または R などその他のテクノロジをこれらの基本的な型に追加するにを使用して [Script Action](#scriptaction)します。

### クラスターのオペレーティング システム

次の 2 つのオペレーティング システムのいずれかで HDInsight クラスターをプロビジョニングすることができます。

- **Windows 上の HDInsight (Windows Server 2012 R2 Datacenter)**: Hadoop クラスターで実行される Windows ベースのサービスとテクノロジを統合する必要がある場合や、既存の Windows ベースの Hadoop ディストリビューションから移行する場合は、このオプションを選択します。

- **Linux 上の HDInsight (Ubuntu 12.04 LTS for Linux)**: Linux または Unix に詳しい場合、既存の Linux ベースの Hadoop ソリューションから移行する場合、または Linux 向けに構築された Hadoop エコシステム コンポーネントとの簡単な統合が必要な場合は、このオプションを選択します。 詳細については、次を参照してください。 [HDInsight の Linux での Hadoop 入門](hdinsight-hadoop-linux-get-started.md)します。

> [AZURE.NOTE] このドキュメントの情報は、Linux ベースの HDInsight クラスターを使用することを前提としています。 Windows ベースのクラスターに固有の情報を参照してください。 [作成 Windows ベースの Hadoop クラスターの HDInsight](hdinsight-provision-clusters.md)します。

### サブスクリプション名

複数の Azure サブスクリプションがある場合は、これを使用して、HDInsight クラスターの作成時に使用するものを選択します。

### リソース グループ

アプリケーションは通常、Web アプリケーション、データベース、データベース サーバー、ストレージ、サード パーティのサービスなどの、複数のコンポーネントで構成されます。 Azure リソース マネージャー (ARM) を使用すると、アプリケーション内の複数のリソースを 1 つのグループ (Azure リソース グループと呼ばれます) と見なして作業できます。 アプリケーションのこれらすべてのリソースを、1 回の連携した操作でデプロイ、更新、監視、または削除できます。 デプロイメントにはテンプレートを使用しますが、このテンプレートは、テスト、ステージング、運用環境などのさまざまな環境に使用できます。 グループ全体のロールアップ コストを表示すると、組織の課金ついて明確に把握できます。 詳細については、次を参照してください。 [Azure リソース マネージャーの概要](resource-group-overview.md)します。

### 資格情報

HDInsight クラスターで使用される認証には以下の 2 種類があります。

* __Admin__ または __HTTPS__ ユーザー: クラスターによって公開される Web または REST サービスにアクセスすると際には、クラスターの管理者アカウントが主に使用されます。 クラスターに直接ログインする場合は使用できません。

* __の SSH ユーザー名__: An SSH ユーザー アカウントを使用して、リモートでアクセスを使用して、クラスター、 [Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell) クライアントです。 これは、クラスター ヘッド ノードへのリモート コマンドライン アクセスを提供するために最もよく使用されます。

管理者アカウントはパスワードで保護されます。管理者アカウントを使用するクラスターへのすべての Web 通信をセキュリティで保護された HTTPS 接続経由で実行する必要があります。

SSH ユーザーは、パスワードまたは証明書を使用して認証できます。 証明書の認証は最も安全なオプションですが、パブリックとプライベート証明書のペアの作成とメンテナンスが必要になります。

SSH キーの作成および使用方法を含む、HDInsight での SSH の使用方法の詳細については、次のいずれかの記事を参照してください。

* [Linux、Unix、または Mac OS X クライアント - HDInsight で SSH を使用しての](hdinsight-hadoop-linux-use-ssh-unix.md)
* [Windows クライアント - HDInsight で SSH を使用しての](hdinsight-hadoop-linux-use-ssh-windows.md)

### データ ソース

HDInsight では、クラスターの基になるストレージとして Azure BLOB Storage を使用します。 内部的には、クラスター上の Hadoop と他のソフトウェアは、このストレージを Hadoop 分散ファイル システム (HDFS) と互換性のあるシステムと見なします。

新しいクラスターを作成する場合は、新しい Azure Storage アカウントを作成するか、既存のものを使用する必要があります。
> [AZURE.IMPORTANT] クラスターは既定のストレージ アカウントと同じデータ センター内にある必要があるため、ストレージ アカウントに対して選択する地理的な場所によって HDInsight クラスターの場所が決まります。 
>
> サポートされているリージョンの一覧を表示するには、クリックして、 **領域** ドロップダウン リストで [HDInsight 料金](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409)します。

#### 既定のストレージ コンテナー

HDInsight では、ストレージ アカウントに _default ストレージ container_ も作成されます。 これが、HDInsight クラスターの既定のストレージです。

既定では、このコンテナーの名前はクラスター名と同じです。 HDInsight は Azure Blob ストレージで動作する方法の詳細については、次を参照してください。 [HDFS と互換性のある Azure Blob ストレージの使用 HDInsight での Hadoop](hdinsight-hadoop-use-blob-storage.md)します。
>[AZURE.WARNING] 1 つのコンテナーを複数のクラスターで共有しないでください。 これはサポートされていません。

### ノード サイズ

クラスターで使用されるコンピューティング リソースのサイズを選択できます。 たとえば、大量のメモリを必要とする操作を実行することがわかっている場合は、より多くのメモリを持つコンピューティング リソースを選択できます。

クラスターの種類によって、ノードの種類、ノードの数およびノードのサイズは異なります。 たとえば、Hadoop クラスターの種類に 2 つの _head nodes_ と、既定値は 4 つの _data nodes_ Storm クラスターの種類があり、2 つの _nimbus nodes_、3 つの _zookeeper nodes_、既定値は 4 つの _supervisor nodes_ あります。
> [AZURE.IMPORTANT] クラスター作成または作成後の拡大で 32 以上の worker ノードを予定している場合、コア数が 8 個以上で RAM が 14GB 以上のサイズのヘッド ノードを選択する必要があります。

Azure ポータルを使用してクラスターを構成する場合、ノード サイズは __[ノード価格レベル]__ ブレードを介して公開され、別のノード サイズに関連付けられているコストも表示されます。
> [AZURE.IMPORTANT] クラスターが作成されると課金が開始され、クラスターが削除された場合にのみ停止します。 料金の詳細については、次を参照してください。 [HDInsight の料金詳細](https://azure.microsoft.com/pricing/details/hdinsight/)します。

## <a id="optionalconfiguration"></a>オプションの構成

次のセクションでは、オプションの構成オプションと、これらの構成を必要とするシナリオについて説明します。

### HDInsight のバージョン

このクラスターに使用する HDInsight のバージョンを指定するためにこれを使用します。詳細については、を参照してください [Hadoop クラスターのバージョンと HDInsight でのコンポーネント](https://go.microsoft.com/fwLink/?LinkID=320896&clcid=0x409)。

### Azure Virtual Network の使用

[Azure Virtual Network](http://azure.microsoft.com/documentation/services/virtual-network/) ソリューションに必要なリソースを含む、セキュリティで保護された永続的なネットワークを作成することができます。 仮想ネットワークでは、次のことが可能になります。

* プライベート ネットワーク (クラウドのみ) 内でのクラウド リソース間の接続

    ![クラウドのみの構成の図](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-vnet-cloud-only.png)

* 仮想プライベート ネットワーク (VPN) を使用したローカル データセンター ネットワークへのクラウド リソースの接続 (サイト間またはポイント対サイト)

  | サイト間構成| ポイント対サイト構成|
  | -------------------------- | --------------------------- |
  | サイト間構成では、ハードウェア VPN またはルーティングとリモート アクセス サービスを使用して、データ センターから複数のリソースを Azure の仮想ネットワークに接続することができます。<br />![サイト対サイト構成のダイアグラム](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-vnet-site-to-site.png)| ポイント対サイト構成では、ソフトウェア VPN を使用して、特定のリソースを Azure の仮想ネットワークに接続することができます。<br />![ポイント対サイト構成のダイアグラム](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-vnet-point-to-site.png)|


仮想ネットワーク機能、利点、および機能の詳細については、次を参照してください。、 [Azure 仮想ネットワークの概要](http://msdn.microsoft.com/library/azure/jj156007.aspx)します。
> [AZURE.NOTE] クラスターを作成する前に、Azure Virtual Network を作成する必要があります。 詳細については、次を参照してください。 [仮想ネットワークを作成する方法](virtual-networks-create-vnet.md)します。
>
> Azure HDInsight は場所ベースの Virtual Networks のみをサポートし、アフィニティ グループ ベースの Virtual Networks は現在取り扱っていません。 既存の Azure Virtual Network が場所ベースかどうかを確認するには、Azure PowerShell コマンドレットの Get-AzureVNetConfig を使用します。 Virtual Network が場所ベースでない場合、次のオプションがあります。
>
> -既存の仮想ネットワークの構成をエクスポートし、新しい仮想ネットワークを作成します。 既定では、新しい Virtual Networks はすべて場所ベースになります。
> -場所ベースの仮想ネットワークに移行します。 参照してください [リージョン スコープへの既存のサービスの移行](http://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/)します。
>
> 1 つのクラスターには単一のサブネットを指定することを強くお勧めします。
>
> 現時点 (2015 年 8 月 25 日) において、Azure Virtual Network では 1 つの Linux ベースのクラスターのみを作成できます。
>
> Linux ベースの HDInsight で v1 (クラシック) Azure Virtual Network を使用することはできません。 Azure ポータルでの HDInsight クラスターの作成プロセスで Virtual Network をオプションとして表示したり、Azure CLI または Azure PowerShell からクラスターを作成するときに Virtual Network を使用できるようにしたりするには、Virtual Network が v2 (Azure リソース マネージャー) である必要があります。
>
> V1 ネットワーク上のリソースをしたし、HDInsight を仮想ネットワークを介してそれらのリソースに直接アクセスできるように、表示する [従来の Vnet を新しい Vnet に接続する](../virtual-network/virtual-networks-arm-asm-s2s.md) v2 の仮想ネットワークを v1 の仮想ネットワークに接続する方法の詳細。 この接続が確立されると、v2 Virtual Network で HDInsight クラスターを作成できます。

### メタストア

メタストアには、Hive テーブル、パーティション、スキーマ、列に関する情報などの Hive と Oozie メタデータが含まれます。 メタストアを使用すると Hive と Oozie メタデータを保持できるため、新しいクラスターを作成するときに、Hive テーブルまたは Oozie ジョブを再作成する必要はありません。

メタストアの構成オプションを使用すると、SQL Database を使用して外部のメタストアを指定することができます。 これにより、クラスターを削除するときに、データベースに外付けで格納されているメタデータ情報を保持できます。 Azure で SQL データベースを作成する方法については、次を参照してください。 [最初の Azure SQL Database を作成する](sql-database-get-started.md)します。
> [AZURE.NOTE] HBase のクラスターの種類では、メタストア構成は使用できません。

### <a id="scriptaction"></a>スクリプト操作

追加コンポーネントをインストールしたり、クラスターのプロビジョニング中にスクリプトを使用してクラスター構成をカスタマイズしたりできます。 このようなスクリプトは**スクリプト アクション**経由で呼び出されます。 詳細については、次を参照してください。 [HDInsight クラスターのカスタマイズ Script Action を使って](hdinsight-hadoop-customize-cluster-linux.md)します。
> [AZURE.IMPORTANT] クラスターの作成後にコンポーネントを追加することはできません。そのようなコンポーネントはクラスター ノードの再イメージ化後に利用できないためです。 スクリプト アクション経由でインストールされたコンポーネントは再イメージ化プロセスで再度インストールされます。

### その他のストレージ

場合によっては、クラスターへのストレージの追加が必要になることがあります。 たとえば、異なる地理的リージョンまたは異なるサービスの複数の Azure Storage アカウントがあるが、それらをすべて HDInsight で分析する必要がある場合などです。

セカンダリの blob ストアの使用の詳細については、次を参照してください。 [HDInsight で使用して Azure Blob ストレージ](../hdinsight-use-blob-storage.md)します。

## <a id="nextsteps"></a><a id="options"></a> 作成方法

この記事では、Linux ベースの HDInsight クラスターの作成に関する基本的な情報を提供しました。 ニーズに最も適した方法を使用してクラスターを作成する方法に関する特定の情報を検索するには、次の表を使用します。

| クラスターの作成時に使用する方法| Web ブラウザーを使用| コマンドラインを使用| REST API を使用| SDK を使用| Linux、Mac OS X、または Unix から| Windows から|
| ------------------------------- |:----------------------:|:--------------------:|:------------------:|:------------:|:-----------------------------:|:------------:|
| [Azure ポータル](hdinsight-hadoop-create-linux-clusters-portal.md)| ✔| &nbsp;| &nbsp;| &nbsp;| ✔| ✔|
| [Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md)| &nbsp;| ✔| &nbsp;| &nbsp;| ✔| ✔|
| [Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)| &nbsp;| ✔| &nbsp;| &nbsp;| &nbsp;| ✔|
| [cURL](hdinsight-hadoop-create-linux-clusters-curl.md)| &nbsp;| ✔| ✔| &nbsp;| ✔| ✔|
| [.NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet.md)| &nbsp;| &nbsp;| &nbsp;| ✔| ✔| ✔|




[hdinsight-use-mapreduce]: ../hdinsight-use-mapreduce/ 
[hdinsight-use-hive]: ../hdinsight-use-hive/ 
[hdinsight-use-pig]: ../hdinsight-use-pig/ 
[hdinsight-upload-data]: ../hdinsight-upload-data/ 
[hdinsight-sdk-documentation]: http://msdn.microsoft.com/library/dn479185.aspx 
[hdinsight-customize-cluster]: ../hdinsight-hadoop-customize-cluster/ 
[hdinsight-get-started]: ../hdinsight-get-started/ 
[hdinsight-admin-powershell]: ../hdinsight-administer-use-powershell/ 
[hdinsight-submit-jobs]: ../hdinsight-submit-hadoop-jobs-programmatically/ 
[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx 
[azure-management-portal]: https://manage.windowsazure.com/ 
[azure-command-line-tools]: ../xplat-cli/ 
[azure-create-storageaccount]: ../storage-create-storage-account/ 
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/ 
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/ 
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/ 
[hdi-remote]: http://azure.microsoft.com/documentation/articles/hdinsight-administer-use-management-portal/#rdp 
[powershell-install-configure]: ../install-configure-powershell/ 
[image-hdi-customcreatecluster]: ./media/hdinsight-get-started/HDI.CustomCreateCluster.png 
[image-hdi-customcreatecluster-clusteruser]: ./media/hdinsight-get-started/HDI.CustomCreateCluster.ClusterUser.png 
[image-hdi-customcreatecluster-storageaccount]: ./media/hdinsight-get-started/HDI.CustomCreateCluster.StorageAccount.png 
[image-hdi-customcreatecluster-addonstorage]: ./media/hdinsight-get-started/HDI.CustomCreateCluster.AddOnStorage.png 
[azure-preview-portal]: https://portal.azure.com 
[image-cli-account-download-import]: ./media/hdinsight-hadoop-provision-linux-clusters/HDI.CLIAccountDownloadImport.png 
[image-cli-clustercreation]: ./media/hdinsight-hadoop-provision-linux-clusters/HDI.CLIClusterCreation.png 
[image-cli-clustercreation-config]: ./media/hdinsight-hadoop-provision-linux-clusters/HDI.CLIClusterCreationConfig.png 
[image-cli-clusterlisting]: ./media/hdinsight-hadoop-provision-linux-clusters/HDI.CLIListClusters.png "List and show clusters"
[image-hdi-ps-provision]: ./media/hdinsight-hadoop-provision-linux-clusters/HDI.ps.provision.png 
[image-hdi-ps-config-provision]: ./media/hdinsight-hadoop-provision-linux-clusters/HDI.ps.config.provision.png 
[img-hdi-cluster]: ./media/hdinsight-hadoop-provision-linux-clusters/HDI.Cluster.png 
[89e2276a]: /documentation/articles/hdinsight-use-sqoop/ "Use Sqoop with HDInsight"

