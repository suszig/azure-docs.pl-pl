<properties 
    pageTitle="Azure Machine Learning 用にデータを Azure SQL Database に移動する | Azure" 
    description="SQL テーブルを作成して SQL テーブルにデータを読み込みます" 
    services="machine-learning" 
    documentationCenter="" 
    authors="fashah" 
    manager="jacob.spoelstra" 
    editor="" 
    videoId="" [hee
    scriptId="" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/12/2015" 
    ms.author="fashah;bradsev" /> 


# Azure Machine Learning 用にデータを Azure SQL Database に移動する

この**メニュー**は、Cortana Analytics Process (CAP) でデータを保存および処理できるターゲット環境にデータを取り込む方法について説明するトピックにリンクしています。

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

## はじめに

**このトピック**では、フラット ファイル (CSV 形式または TSV 形式) のデータまたはオンプレミスの SQL Server に格納されているデータを、Azure SQL Database に移動するためのオプションについて説明します。 クラウドにデータを移動するためのこれらのタスクは、Azure で提供される Cortana Analytics Process の一部です。

Machine Learning の内部設置型 SQL Server にデータを移動するオプションについて説明するトピックを参照してください。 [Azure の仮想マシンに SQL Server にデータを移動する](machine-learning-data-science-move-sql-server-virtual-machine.md)します。

次の表は、Azure SQL Database にデータを移動するためのオプションをまとめたものです。

 <b>ソース</b>| <b>Azure SQL データベースの移行先:</b>|
-------------- |--------------------------------|
 <b>フラット ファイル (CSV または TSV 形式)</b>| <a href="#bulk-insert-sql-query">一括挿入 SQL クエリ|
 <b>内部設置型 SQL Server</b>| 1.<a href="#export-flat-file">フラット ファイルにエクスポート<br> 2 です。<a href="#insert-tables-bcp">SQL Database 移行ウィザード<br> 3 です。<a href="#db-migration">データベースのバックアップと復元<br> 4 です。<a href="#adf">Azure Data Factory|


## <a name="prereqs"></a>前提条件

ここに記載されているこの手順には次のものが必要です。

* **Azure サブスクリプション**。 サインアップすることができます、サブスクリプションを持っていない場合、 [無料評価版](https://azure.microsoft.com/pricing/free-trial/)します。
* **Azure ストレージ アカウント**。 このチュートリアルのデータを格納するには、Azure ストレージ アカウントを使用します。 Azure ストレージ アカウントを取得していない場合、 [ストレージ アカウントの作成](storage-create-storage-account.md#create-a-storage-account) 記事です。 ストレージ アカウントを作成した後は、ストレージにアクセスするために使用するアカウント キーを取得する必要があります。 参照してください [表示、コピーおよび再生成するストレージ アクセス キー](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)します。
* **Azure SQL Database** へのアクセス権。 場合は、Azure SQL データベースをセットアップする必要があります [Microsoft Azure SQL Database の概要 ](sql-database-get-started.md) を Azure SQL データベースの新しいインスタンスをプロビジョニングする方法について説明します。
* **Azure PowerShell** がローカルにインストールされ構成されていること。 手順については、次を参照してください。 [をインストールして、Azure PowerShell を構成する方法](powershell-install-configure.md)します。

**データ**: を使用して移行プロセスが示されて、 [NYC タクシー データセット](http://chriswhong.com/open-data/foil_nyc_taxi/)します。 NYC タクシー データセット乗車データと展示について説明し、Azure blob ストレージにその投稿を説明したように、使用可能な: [NYC タクシー データ](http://www.andresmh.com/nyctaxitrips/)します。 サンプルとこれらのファイルの説明で記述されて [NYC タクシー乗車データセットの説明](machine-learning-data-science-process-sql-walkthrough.md#dataset)します。

ここで説明されている手順は、自身のデータに適用することも、NYC タクシー データセットを使用してこの手順に従って行うこともできます。 内部設置型 SQL Server データベースには、NYC タクシー データセットをアップロードするに記載されている手順に従って [SQL Server データベースにデータを一括インポート](machine-learning-data-science-process-sql-walkthrough.md#dbload)します。 これらは Azure Virtual Machine 上の SQL Server にアップロードする手順ですが、オンプレミスの SQL Server へのアップロード手順も同じです。

## <a name="file-to-azure-sql-database"></a> フラット ファイル ソースからデータを Azure SQL データベースを移動

フラット ファイル (CSV 形式または TSV 形式) のデータは、一括挿入 SQL クエリを使用して Azure SQL Database に移動できます。

### <a name="bulk-insert-sql-query"></a> 一括挿入 SQL クエリ

一括挿入 SQL クエリを使用する手順は、フラット ファイル ソースから Azure VM 上の SQL Server にデータを移動する手順と似ています。 詳細については、「 [一括挿入 SQL クエリ](machine-learning-data-science-move-sql-server-virtual-machine.md#insert-tables-bulkquery)します。

## <a name="sql-on-prem-to-sazure-sql-database"></a> 内部設置型 SQL Server から Azure SQL データベースへのデータの移動

ソース データがオンプレミスの SQL Server に保存されている場合は、さまざまな方法で Azure SQL Database にデータを移動できます。

1. [フラット ファイルにエクスポートします。](#export-flat-file)
2. [SQL Database 移行ウィザード](#insert-tables-bcp)
3. [データベースのバックアップと復元](#db-migration)
4. [Azure Data Factory](#adf)

最初の 3 つの手順は似ていますがこれらのセクションへ [Azure の仮想マシンに SQL Server にデータを移動する](machine-learning-data-science-move-sql-server-virtual-machine.md) 同じ手順に対応します。 そのトピック内の該当するセクションへのリンクを以下に示します。

### <a name="export-flat-file"></a>フラット ファイルにエクスポートします。

フラット ファイルにエクスポートするための手順は、管理対象のような [フラット ファイルにエクスポート](machine-learning-data-science-move-sql-server-virtual-machine.md#export-flat-file)します。

### <a name="insert-tables-bcp"></a>SQL Database 移行ウィザード

SQL Database 移行ウィザードを使用する手順は、管理対象のような [SQL Database 移行ウィザード](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-migration)します。

### <a name="db-migration"></a>データベースのバックアップと復元

データベースのバックアップと復元を使用する手順は、その管理対象のような [データベースのバックアップと復元](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-backup)します。

### <a name="adf"></a>Azure Data Factory

Azure SQL database と Azure Data Factory (ADF) にデータを移動する手順は、トピックでは提供 [Azure Data Factory を使って SQL azure、内部設置型 SQL server からデータを移動](machine-learning-data-science-move-sql-azure-adf.md)します。このトピックでは、ADF を使用して Azure Blob ストレージを使用して、Azure SQL データベースを内部設置型 SQL Server データベースからデータを移動する方法を説明します。

オンプレミスとクラウドの両方のリソースにアクセスするハイブリッド シナリオで、継続的にデータを移行する必要がある場合、および移行の過程で、データを処理する場合や、データに変更を加えたりビジネス ロジックを付加したりする必要がある場合には、ADF の使用を検討してください。 ADF では、定期的にデータの移動を管理するシンプルな JSON スクリプトを使用して、ジョブのスケジュールと監視ができます。 ADF には他にも、複雑な操作のサポートなどの機能があります。









