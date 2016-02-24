<properties 
    pageTitle="SSIS コネクタを使用して Azure BLOB ストレージとの間でデータを移動する | Microsoft Azure" 
    description="SSIS コネクタを使用して Azure BLOB ストレージとの間でデータを移動します。" 
    services="machine-learning,storage" 
    documentationCenter="" 
    authors="bradsev" 
    manager="paulettm" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2015" 
    ms.author="bradsev" />

# SSIS コネクタを使用して Azure BLOB ストレージとの間でデータを移動する

以下のリンクから、Azure Blob ストレージとの間でデータを移動するために使用するテクノロジについてのガイダンスを参照してください。

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

## はじめに

 [Azure の SQL Server Integration Services Feature Pack](https://msdn.microsoft.com/library/mt146770.aspx) Azure と内部設置型のデータ ソース、および Azure に格納されているデータの処理の間で転送データを Azure に接続するコンポーネントを提供します。 

ユーザーは、オンプレミスのデータをクラウドに移動すると、そのデータにすべての Azure サービスからアクセスできるようになり、一連の Azure テクノロジを最大限に活用できます。 たとえば、Azure Machine Learning または HDInsight クラスター上で使用できます。 

これには、まずは一般に、 [SQL](https://azure.microsoft.com/documentation/articles/machine-learning-data-science-process-sql-walkthrough/) と [HDInsight](https://azure.microsoft.com/documentation/articles/machine-learning-data-science-process-hive-walkthrough/) のチュートリアルです。

ハイブリッド データ統合のシナリオで一般的なビジネス ニーズのために SSIS を使用する標準的なシナリオの詳細については、次を参照してください。 [Azure の SQL Server Integration Services Feature Pack で大きな効果](http://blogs.msdn.com/b/ssis/archive/2015/06/25/doing-more-with-sql-server-integration-services-feature-pack-for-azure.aspx) ブログです。

> [AZURE.NOTE] Azure blob ストレージに完全な概要についてを参照してください [Azure Blob の基礎](../storage-dotnet-how-to-use-blobs.md) と  [Azure Blob サービス](https://msdn.microsoft.com/library/azure/dd179376.aspx)します。 

## 前提条件

この記事で説明するタスクを実行するには、Azure サブスクリプションと Azure ストレージ アカウントの設定が必要です。 データをアップロードまたはダウンロードするために、Azure ストレージ アカウント名とアカウント キーを確認しておく必要があります。 

- 設定する、 **Azure サブスクリプション**, を参照してください [無料評価版の 1 か月](https://azure.microsoft.com/pricing/free-trial/)します。
- 作成する手順について、 **ストレージ アカウント** し、アカウントとキー情報を取得するため、次を参照してください。 [は Azure ストレージ アカウント](../storage-create-storage-account.md)します。


使用する、 **SSIS コネクタ** ダウンロードする必要があります。

- **SQL Server 2014 または 2016年標準 (以上)**: インストールを含む SQL Server Integration Services が含まれています。
- **Microsoft SQL Server 2014 または 2016 Integration Services Feature Pack for Azure**。 これらをダウンロードできるから、それぞれ、 [SQL Server 2014 Integration Services](http://www.microsoft.com/download/details.aspx?id=47366) と [SQL Server 2016 Integration Services](https://www.microsoft.com/download/details.aspx?id=49492) ページです。

> [AZURE.NOTE] SSIS は、SQL Server と共にインストールされますが、Express バージョンに含まれていません。 SQL Server のさまざまなエディションでどのようなアプリケーションが含まれている方法の詳細については、次を参照してください [SQL Server のエディション。](http://www.microsoft.com/en-us/server-cloud/products/sql-server-editions/)

SSIS のトレーニング資料を参照してください [SSIS の手でトレーニング。](http://www.microsoft.com/download/details.aspx?id=20766)

アップの実行を取得する方法の詳細について SISS を使用して単純な抽出、変換、および読み込み (ETL) パッケージの参照を構築する [SSIS チュートリアル: 簡単な ETL パッケージを作成する](https://msdn.microsoft.com/library/ms169917.aspx)です。

## NYC タクシー データセットをダウンロードする  
上の例では、パブリックに利用可能なデータセット--をここで使用して、 [NYC タクシー乗車](http://www.andresmh.com/nyctaxitrips/) データセット。 このデータセットは、2013 年のニューヨーク市における約 1 億 7300 万件のタクシー乗車情報で構成されています。 データには、乗車の詳細データと料金データの 2 種類があります。 月ごとにファイルが用意されているため、全部で 24 ファイルになり、各ファイルは圧縮されていない状態で約 2 GB になります。 


## Azure BLOB ストレージにデータをアップロードする
インスタンスを使用して、SSIS を使用してデータを feature pack は、オンプレミスから Azure blob ストレージに移動するには [**Azure Blob をアップロード Task**](https://msdn.microsoft.com/library/mt146776.aspx), 、下図のようします。

![configure-data-science-vm](./media/machine-learning-data-science-move-data-to-azure-blob-using-ssis/ssis-azure-blob-upload-task.png)


タスクで使用されるパラメーターについて、ここで説明します。


フィールド|説明|
----------------------|----------------|
**AzureStorageConnection**|既存の Azure Storage 接続マネージャーを指定するか、BLOB ファイルがホストされる場所を指す Azure ストレージ アカウントを参照する、新しい Azure Storage 接続マネージャーを作成します。|
**BlobContainer**|アップロードしたファイルを BLOB として保持する BLOB コンテナーの名前を指定します。|
**BlobDirectory**|アップロードしたファイルがブロック BLOB として格納される BLOB ディレクトリを指定します。 BLOB ディレクトリは仮想階層構造です。 BLOB が既に存在する場合は置き換えられます。|
**LocalDirectory**|アップロードするファイルが含まれているローカル ディレクトリを指定します。|
**FileName**|指定された名前のパターンを使用したファイルを選択するための名前フィルターを指定します。 たとえば、MySheet\*.xls\* には、MySheet001.xls MySheetABC.xlsx などのファイルが含まれます。|
**TimeRangeFrom/TimeRangeTo**|時間範囲フィルターを指定します。 ファイルを変更した後 *TimeRangeFrom* とする前に *TimeRangeTo* が含まれます。|


> [AZURE.NOTE]  **AzureStorageConnection** 正しい資格情報が必要な **かなり** 転送を試みる前に存在する必要があります。
 
## Azure BLOB ストレージからデータをダウンロードする

SSIS でのストレージの内部設置型に、Azure blob ストレージからデータをダウンロードするには、インスタンスを使用して、 [Azure Blob をアップロード Task](https://msdn.microsoft.com/library/mt146779.aspx)します。

##さらに高度な SSIS と Azure のシナリオ
SSIS 機能パックでは、複数のタスクを 1 つにパッケージ化することで、さらに複雑なフローを処理することができます。 たとえば、BLOB データを HDInsight クラスターに直接フィードし、HDInsight クラスターの出力を BLOB にダウンロードしてから、さらにオンプレミス ストレージにダウンロードすることができます。 SSIS は、追加の SSIS コネクタを使用することで、HDInsight クラスター上で Hive ジョブおよび Pig ジョブを実行できます。

- SSIS による Azure の HDInsight クラスターで Hive スクリプトを実行するには使用 [Azure HDInsight Hive Task](https://msdn.microsoft.com/library/mt146771.aspx)します。
- SSIS による Azure の HDInsight クラスターで Pig スクリプトを実行するには使用 [Azure HDInsight Pig Task](https://msdn.microsoft.com/library/mt146781.aspx)します。

