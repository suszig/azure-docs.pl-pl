<properties
   pageTitle="Hdinsight での Apache Spark クラスターのプロビジョニング | Microsoft Azure"
   description="Azure クラシック ポータル、Azure PowerShell、コマンド ライン、または HDInsight .NET SDK を使用して、Azure HDInsight 用の Spark クラスターをプロビジョニングする方法について説明します"
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
    ms.date="11/07/2015"
    ms.author="nitinme"/>

# カスタム オプションを使用した HDInsight での Apache Spark クラスターのプロビジョニング

ほとんどの場合、簡易を使用してクラスターに明記されているメソッドを作成する Spark をプロビジョニングできます [HDInsight での Apache Spark を使ってみる](hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql.md)します。 特定のシナリオでは、カスタマイズされたクラスターのプロビジョニングが必要になる場合があります。 たとえば、外部メタデータ ストアをアタッチしてクラスターの有効期間より長く永続的 Hive メタデータを保持する場合や、クラスターで追加のストレージを使用する場合です。

このようなシナリオのため、この記事では、Azure クラシック ポータル、Azure PowerShell、または HDInsight .NET SDK を使用して、HDInsight 上のカスタマイズされた Spark クラスターをプロビジョニングする方法を説明します。  


**前提条件:**

この記事の手順を開始する前に、Azure サブスクリプションが必要です。 参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。

##<a id="configuration"></a>別の構成オプションとは

###その他のストレージ

構成中に、Azure BLOB ストレージ アカウントと既定のコンテナーを指定する必要があります。 このコンテナーは、既定の保存先としてクラスターで使用されます。 必要に応じて、クラスターに関連付ける追加の Azure Storage アカウントBLOB も指定できます。

>[AZURE.NOTE] 1 つの Blob ストレージ コンテナーを複数のクラスターを共有しないでください。 これはサポートされていません。

セカンダリの Blob ストアの使用の詳細については、次を参照してください。 [HDInsight で Azure Blob ストレージを使用して](hdinsight-use-blob-storage.md)します。

###メタストア

Spark では、生データに対してスキーマおよび Hive テーブルを定義できます。 これらのスキーマおよびテーブル メタデータは外部のメタストアに保存できます。 メタストアを使用すると Hive メタデータを保持できるため、新しいクラスターをプロビジョニングするときに、Hive テーブルを再作成する必要はありません。 既定では、Hive は組み込みデータベースを使用してこの情報を格納します。 組み込みデータベースは、クラスターが削除されるとメタデータを保持できません。

Azure で SQL データベースを作成する方法については、次を参照してください。 [最初の Azure SQL Database を作成する](sql-database-get-started.md)です。

### クラスターのカスタマイズ

追加コンポーネントをインストールするか、プロビジョニング中にスクリプトを使用してクラスターの構成をカスタマイズできます。 このようなスクリプトを使用して実行します。 **Script Action**, 、これは、Azure 旧ポータル、HDInsight Windows PowerShell コマンドレットまたは HDInsight .NET SDK から使用できる構成オプションです。 詳細については、次を参照してください。 [HDInsight クラスターのカスタマイズ Script Action を使って][hdinsight-customize-cluster]します。


###仮想ネットワーク

[Azure の仮想ネットワーク](http://azure.microsoft.com/documentation/services/virtual-network/) ソリューションに必要なリソースを含む、セキュリティで保護された永続的なネットワークを作成することができます。 仮想ネットワークでは、次のことが可能になります。

* プライベート ネットワーク (クラウドのみ) 内でのクラウド リソース間の接続

    ![クラウドのみの構成の図](./media/hdinsight-apache-spark-provision-clusters/hdinsight-vnet-cloud-only.png)

* 仮想プライベート ネットワーク (VPN) を使用したローカル データセンター ネットワークへのクラウド リソースの接続 (サイト間またはポイント対サイト)

    サイト間構成では、ハードウェア VPN を使用するか、ルーティングとリモート アクセス サービスを使用して、データセンターから複数のリソースを Azure 仮想ネットワークに接続できます。

    ![サイト間構成の図](./media/hdinsight-apache-spark-provision-clusters/hdinsight-vnet-site-to-site.png)

    ポイント対サイト構成では、ソフトウェア VPN を使用して、特定のリソースを Azure 仮想ネットワークに接続できます。

    ![ポイント対サイト構成の図](./media/hdinsight-apache-spark-provision-clusters/hdinsight-vnet-point-to-site.png)

仮想ネットワーク機能、利点、および機能の詳細については、次を参照してください。、 [Azure 仮想ネットワークの概要](../virtual-network/virtual-networks-overview.md)します。

> [AZURE.NOTE] クラスターをプロビジョニングする前に、Azure の仮想ネットワークを作成する必要があります。 詳細については、次を参照してください。 [仮想ネットワークを作成する方法](virtual-networks-create-vnet.md)します。
>
> Azure HDInsight は場所ベースの仮想ネットワークのみをサポートし、アフィニティ グループ ベースの仮想ネットワークは現在取り扱っていません。
>
> 1 つのクラスターには単一のサブネットを指定することを強くお勧めします。

##Azure ポータルの使用

HDInsight 上の Spark クラスターは、既定のファイル システムとして Azure BLOB ストレージ コンテナーを使用します。 HDInsight クラスターを作成するには、同じデータ センターにある Azure Storage アカウントが必要です。 詳細については、次を参照してください。 [HDInsight で Azure Blob ストレージの使用](hdinsight-hadoop-use-blob-storage.md)します。 Azure ストレージ アカウントの作成の詳細については、「 [をストレージ アカウントを作成する方法][azure-create-storageaccount]します。

**カスタム作成オプションを使用して HDInsight クラスターを作成するには**

1. サインイン、 [Azure ポータル](https://portal.azure.com)します。
2. クリックして **新規**, 、] をクリックして **データ分析**, 、] をクリックし、 **HDInsight**します。

    ![Azure ポータルでの新しいクラスターの作成](./media/hdinsight-apache-spark-provision-clusters/HDI.CreateCluster.1.png "Creating a new cluster in the Azure Portal")

3. 入力、 **クラスター名**, [ **Spark** の **クラスターの種類**, との間、 **クラスターのオペレーティング システム** ドロップダウン リストで、[ **Windows Server 2012 R2 Datacenter**します。 クラスターを使用できる場合は、クラスター名の横に緑色のチェック マークが表示されます。

    ![クラスターの名前と種類の入力](./media/hdinsight-apache-spark-provision-clusters/HDI.CreateCluster.2.png "Enter cluster name and type")

4. 複数のサブスクリプションの場合はクリックして、 **サブスクリプション** エントリをクラスターに使用される Azure サブスクリプションを選択します。

5. クリックして **リソース グループ** を既存のリソース グループの一覧を表示しでクラスターを作成する 1 つを選択します。 またはをクリックして **新規作成** し、新しいリソース グループの名前を入力します。 新しいグループ名を使用できる場合は、緑のチェック マークが表示されます。

    > [AZURE.NOTE] このエントリは、いずれかが使用可能な場合、既存のリソース グループのいずれかに設定されます。

6. をクリックして **資格情報**, を入力し、 **クラスター ログイン ユーザー名** と **クラスター ログインのパスワード**します。 クラスター ノードでリモート デスクトップを有効にする **リモート デスクトップを有効にする**, 、] をクリックして **はい**します。 リモート デスクトップがクラスターにアクセスできる有効期限が切れる日付を選択し、リモート デスクトップ ユーザーのユーザー名/パスワードを指定します。 クリックして **選択** 下部の資格情報の構成を保存します。

    ![クラスターの資格情報の指定](./media/hdinsight-apache-spark-provision-clusters/HDI.CreateCluster.3.png "Provide cluster credentials")

7. クリックして **データソース** クラスターの場合、既存のデータ ソースを選択するか、新しいものを作成します。

    ![[データ ソース] ブレード](./media/hdinsight-apache-spark-provision-clusters/HDI.CreateCluster.4.png "Provide data source configuration")

    現在、HDInsight クラスターのデータ ソースとして Azure Storage アカウントを選択できます。 エントリを理解するために、次を使用して、 **データソース** ブレードです。

    - **選択方法**: これを設定 **すべてのサブスクリプションから** すべてのサブスクリプションからストレージ アカウントの参照を有効にします。 これを設定 **アクセス キー** を入力する場合、 **ストレージ名** と **アクセス キー** の既存のストレージ アカウント。

    - **ストレージ アカウントを選択]、[新規作成**: クリックして **ストレージ アカウントを選択** を参照して、クラスターに関連付ける既存のストレージ アカウントを選択します。 または、[ **新規作成** 新しいストレージ アカウントを作成します。 表示されたフィールドに、ストレージ アカウントの名前を入力します。 名前を使用できる場合は、緑色のチェック マークが表示されます。

    - **既定のコンテナーを選択して**: これを使用して、クラスターで使用する既定のコンテナーの名前を入力します。 任意の名前を入力できますが、コンテナーが特定のクラスターで使用されていることを簡単に認識できるように、クラスターと同じ名前を使用することをお勧めします。

    - **場所**: リージョン、ストレージ アカウントには、やで作成されます。

        > [AZURE.IMPORTANT] 既定のデータ ソースの場所を選択すると、HDInsight クラスターの場所も設定されます。 クラスターと既定のデータ ソースは、同じリージョンに存在する必要があります。

    クリックして **選択** をデータ ソースの構成を保存します。

8. クリックして **ノード料金レベル** このクラスター用に作成するノードに関する情報を表示します。 クラスターで必要な worker ノードの数を設定します。 クラスターの推定コストがブレード内に表示されます。

    ![[ノード価格レベル] ブレード](./media/hdinsight-apache-spark-provision-clusters/HDI.CreateCluster.5.png "Specify number of cluster nodes")

    をクリックして **選択** 価格構成ノードを保存します。

9. をクリックして **オプションの構成** をクラスターのバージョンを選択できるだけでなく、その他のオプションの設定を追加するなど、 **仮想ネットワーク**, で、設定を **外部メタストア** Hive と Oozie のデータを保持するために、Script Action を使用して、カスタム コンポーネントをインストールするのには、クラスターをカスタマイズするか、クラスターで追加のストレージ アカウントを使用しています。

    * クリックして、 **HDInsight Version** ドロップダウンをクリックし、クラスターを使用するバージョンを選択します。 詳細については、次を参照してください。 [HDInsight クラスター バージョン](hdinsight-component-versioning.md)です。

    * クリックして **仮想ネットワーク** を仮想ネットワークの一部としてクラスターを構成する構成の詳細を提供します。  **仮想ネットワーク** ブレードで、をクリックして **仮想ネットワーク** し使用するネットワークをクリックします。 同様に、選択、 **サブネット** のネットワーク、およびクリック **選択** 仮想ネットワークの構成を保存します。

        ![[仮想ネットワーク] ブレード](./media/hdinsight-apache-spark-provision-clusters/HDI.CreateCluster.6.png "Specify virtual network details")

    * クリックして **外部メタストア** を使用して、クラスターに関連付けられている Hive と Oozie メタデータを保存する SQL データベースを指定します。

        ![[カスタム メタストア] ブレード](./media/hdinsight-apache-spark-provision-clusters/HDI.CreateCluster.7.png "Specify external metastores")

         **Hive 用の既存の SQL DB を使用して** メタデータ、] をクリックして **はい**, SQL データベースを選択して、データベースのユーザー名とパスワードを提供します。 する場合は、次の手順を繰り返します **Oozie メタデータに対して既存の SQL DB を使用**します。 クリックして **選択** にするまで、 **オプションの構成** ブレードです。

        >[AZURE.NOTE] メタストアに使用される Azure SQL データベースには、Azure HDInsight などの Azure サービスへの接続を許可する必要があります。 Azure SQL データベース ダッシュボードの右側に表示されているサーバー名をクリックします。 これは、SQL Database インスタンスが実行されているサーバーです。 サーバー ビューに表示されたらクリックして **構成**, 、に対して **Azure Services**, 、] をクリックして **[はい]**, 、] をクリックし、 **保存**します。

    * をクリックして **Script Action** として、クラスターが作成されるカスタム スクリプトを使用してクラスターをカスタマイズする場合。 スクリプト アクションの詳細については、次を参照してください。 [をカスタマイズする HDInsight クラスターの Script Action を使って](hdinsight-hadoop-customize-cluster.md)します。 [Script Action] ブレードで、次の画面キャプチャに示すように、詳細を指定します。

        ![[スクリプト アクション] ブレード](./media/hdinsight-apache-spark-provision-clusters/HDI.CreateCluster.8.png "Specify script action")

        クリックして **選択** スクリプト アクションの構成の変更を保存します。

    * クリックして **Azure ストレージ キー** をクラスターに関連付ける追加のストレージ アカウントを指定します。  **Azure ストレージ キー** ブレードで、をクリックして **ストレージ キーを追加する**, 、既存のストレージ アカウントを選択するか、新しいアカウントを作成します。

        ![[追加ストレージ] ブレード](./media/hdinsight-apache-spark-provision-clusters/HDI.CreateCluster.9.png "Specify additional storage accounts")

        クリックして **選択** にするまで、 **新しい HDInsight クラスター** ブレードです。

10.  **新しい HDInsight クラスター** ブレードで、いることを確認 **スタート画面にピン** クリックして選択すると、 **作成**します。 これでクラスターが作成され、Azure クラシック ポータルのスタート画面にクラスター用のタイルが追加されます。 アイコンはクラスターがプロビジョニング中であることを示し、プロビジョニングが完了すると、[HDInsight] アイコンを表示するように変化します。

    | プロビジョニング中 | プロビジョニング完了 |
    | ------------------ | --------------------- |
    | ![スタート画面のプロビジョニング中インジケーター](./media/hdinsight-apache-spark-provision-clusters/provisioning.png) | ![プロビジョニングされたクラスターのタイル](./media/hdinsight-apache-spark-provision-clusters/provisioned.png) |

    > [AZURE.NOTE] クラスターを作成、通常は約 15 分間での時間がかかります。 タイルを使用して、スタート画面で、または **通知** プロビジョニング プロセスをチェックするページの左にあるエントリです。

11. プロビジョニングが完了したら、スタート画面でクラスター用のタイルをクリックして、クラスター ブレードを起動します。 クラスター ブレードには、名前、属しているリソース グループ、場所、オペレーティング システム、クラスター ダッシュボードの URL などのクラスターに関する基本情報が表示されます。

    ![クラスター ブレード](./media/hdinsight-apache-spark-provision-clusters/HDI.Cluster.Blade.png "Cluster properties")

    このブレードの上部にあるアイコンを理解して、次を使用して、 **Essentials** と **クイック リンク** セクション。

    * **設定** と **すべての設定**: 表示、 **設定** ブレードで、クラスターのクラスターの詳細な構成情報にアクセスすることができます。

    * **ダッシュ ボード** と **URL**: これらは、クラスターでジョブを実行する Web ポータルは、クラスターのダッシュ ボードにアクセスするすべての方法です。

    * **リモート デスクトップ**: クラスター ノードでリモート デスクトップを有効/無効にできます。

    * **クラスターのスケール**: このクラスターのワーカー ノードの数を変更することができます。

    * **削除**: HDInsight クラスターを削除します。

    * **クイック スタート** (![クラウドとサンダー ボルトのアイコンのクイック スタートを =](./media/hdinsight-apache-spark-provision-clusters/quickstart.png)): のに役立つ情報を表示 HDInsight の概要です。

    * **ユーザー** (![ユーザー アイコン](./media/hdinsight-apache-spark-provision-clusters/users.png)): アクセス許可を設定することができます _ポータル管理_ 、Azure サブスクリプションに他のユーザーは、このクラスターのです。

        > [AZURE.IMPORTANT] これは、 _のみ_ アクセスと Azure ポータルでは、このクラスターへのアクセス許可に影響しへの接続または HDInsight クラスターにジョブを送信できるユーザーに影響を与えません。

    * **タグ** (![タグ アイコン](./media/hdinsight-apache-spark-provision-clusters/tags.png)): タグでは、クラウド サービスのカスタム分類を定義するキー/値ペアを設定することができます。 たとえば、という名前のキーを作成することがあります __プロジェクト__, 、し、特定のプロジェクトに関連付けられているすべてのサービスの一般的な値を使用します。

    * **クラスター ダッシュ ボード**: すること自体には、クラスターのダッシュ ボードの起動、または Zeppelin および Jupyter notebook を起動からクラスターのダッシュ ボードのブレードを起動します。


## Azure PowerShell の使用

参照してください [作成 HDInsight クラスターの](hdinsight-provision-clusters.md#create-using-azure-powershell)です。

New-AzureRmHDInsightCluster コマンドレットで Spark クラスターの種類のスイッチを指定します。

    -ClusterType Spark

## ARM ベースの HDInsight .NET SDK の使用

参照してください [作成 HDInsight クラスターの](hdinsight-provision-clusters.md#create-using-the-hdinsight-net-sdk)です。

Spark クラスターの種類を指定します。

    private const HDInsightClusterType NewClusterType = HDInsightClusterType.Spark;


## 次のステップ

* 参照してください [BI ツールを使用して HDInsight で Spark を使用して対話型データ分析の実行](hdinsight-apache-spark-use-bi-tools.md) を Power BI や Tableau などの BI ツールを HDInsight での Apache Spark を使用する方法を参照してください。
* 参照してください [機械学習アプリケーションを構築するための HDInsight での Spark の使用](hdinsight-apache-spark-ipython-notebook-machine-learning.md) を HDInsight での Apache Spark を使用して機械学習アプリケーションを作成する方法について説明します。
* 参照してください [リアルタイム ストリーミング アプリケーションを構築するための HDInsight での Spark の使用](hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming.md) を HDInsight での Apache Spark を使用してストリーミング アプリケーションを作成する方法について説明します。
* 参照してください [Azure HDInsight で Apache Spark クラスターのリソースを管理](hdinsight-apache-spark-resource-manager.md) にリソース マネージャーを使用して Spark クラスターに割り当てられたリソースを管理する方法について説明します。


[hdinsight-sdk-documentation]: http://msdn.microsoft.com/library/dn479185.aspx
[hdinsight-hbase-custom-provision]: http://azure.microsoft.com/documentation/articles/hdinsight-hbase-get-started/

[hdinsight-customize-cluster]: ../hdinsight-hadoop-customize-cluster/
[hdinsight-get-started]: ../hdinsight-get-started/

[hdinsight-admin-powershell]: ../hdinsight-administer-use-powershell/
[hdinsight-admin-portal]: ../hdinsight-administer-use-management-portal/
[hadoop-hdinsight-intro]: ../hdinsight-hadoop-introduction/
[hdinsight-submit-jobs]: ../hdinsight-submit-hadoop-jobs-programmatically/
[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx
[hdinsight-storm-get-started]: ../hdinsight-storm-getting-started/

[azure-management-portal]: https://manage.windowsazure.com/

[azure-command-line-tools]: ../xplat-cli/

[azure-create-storageaccount]: ../storage-create-storage-account/

[apache-hadoop]: http://go.microsoft.com/fwlink/?LinkId=510084
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[hdi-remote]: http://azure.microsoft.com/documentation/articles/hdinsight-administer-use-management-portal/#rdp


[powershell-install-configure]: ../install-configure-powershell/


[azure-preview-portal]: https://portal.azure.com

[89e2276a]: /documentation/articles/hdinsight-use-sqoop/ "Use Sqoop with HDInsight"

