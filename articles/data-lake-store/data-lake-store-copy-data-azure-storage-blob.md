<properties 
   pageTitle="Azure Storage Blob のデータを Data Lake Store にコピーする | Microsoft Azure"
   description="AdlCopy ツールを使用して Azure Storage Blob のデータを Data Lake Store にコピーします" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="paulettm" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="12/11/2015"
   ms.author="nitinme"/>

# Azure Storage BLOB から Data Lake Store へのデータのコピー

コマンド ライン ツールを提供する azure Data Lake ストア [AdlCopy](http://aka.ms/downloadadlcopy), 、Azure ストレージ Blob からデータをデータ湖のストアにコピーします。 ツールには 2 つの使用方法があります。

* **スタンドアロン**, 、タスクを実行するツールが Data Lake ストアのリソースを使用します。
* **Data Lake 分析アカウントを使用して**, Data Lake 分析アカウントに割り当てられた単位がコピー操作の実行に使用されます。 予測可能な方法でコピー操作を実行したい場合は、このオプションを利用できます。

##前提条件

この記事を読み始める前に、次の項目を用意する必要があります。

- **Azure サブスクリプション**します。 参照してください [取得 Azure 無料試用版](https://azure.microsoft.com/pricing/free-trial/)します。
- **Azure サブスクリプションを有効にする** 湖のデータ ストアのパブリック プレビューします。 参照してください [指示](data-lake-store-get-started-portal.md#signup)します。 
- **Azure のストレージ Blob** いくつかのデータを含むコンテナーです。
- **Azure Data Lake 分析アカウント (省略可能)** -を参照してください [Azure Data Lake 分析を使ってみる](data-lake-analytics/data-lake-analytics-get-started-portal.md) 湖のデータ ストアのアカウントを作成する方法についてです。
- **AdlCopy ツール**します。 ツールをインストールする AdlCopy から [http://aka.ms/downloadadlcopy](http://aka.ms/downloadadlcopy)します。

## AdlCopy ツールの構文

AdlCopy ツールを操作するには、次の構文を使用します。

    AdlCopy /Source <Blob source> /Dest <ADLS destination> /SourceKey <Key for Blob account> /Account <ADLA account> /Unit <Number of Analytics units>

構文のパラメーターを次に示します。

| オプション    | 説明                                                                                                                                                                                                                                                                                                                                                                                                          |
|-----------|------------|
| Source    | Azure Storage Blob のソース データの場所を指定します。 ソースには BLOB コンテナーまたは BLOB を使用できます。                                                                                                                                                                                                                                                                                                    |
| Dest      | Data Lake Store のコピー先を指定します。                                                                                                                                                                                                                                                                                                                                                                |
| SourceKey | Azure Storage Blob ソースのストレージ アクセス キーを指定します。                                                                                                                                                                                                                                                                                                                                                  |
| アカウント   | **省略可能な**です。 Azure Data Lake Analytics アカウントを使用してコピー ジョブを実行する場合に、このオプションを使用します。 構文に /Account オプションを使用して、Data Lake Analytics アカウントを指定しない場合、AdlCopy は既定のアカウントを使用してジョブを実行します。 また、このオプションを使用する場合、ソース (Azure Storage Blob) とコピー先 (Azure Data Lake Store) を Data Lake Analytics アカウントのデータ ソースとして追加する必要があります。  |
| Units     |  コピー ジョブに使用する Data Lake Analytics ユニット数を指定します。 このオプションを使用する場合は必須です、 **/アカウント** Data Lake 分析アカウントを指定するにはオプションです。                                                                                                                                                                                                                                                                                                                                               



## スタンドアロンとして AdlCopy ツールを使用する

1. コマンド プロンプトを開き、AdlCopy がインストールされているディレクトリ (通常は `%HOMEPATH%\Documents\adlcopy`) に移動します。

2. 次のコマンドを実行して、ソース コンテナーの特定の BLOB を Data Lake Store にコピーします。

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    次に例を示します。

        AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== 

    Data Lake Store アカウントがある Azure サブスクリプションの資格情報を入力するように求められます。 次のような出力が表示されます。

        Initializing Copy.
        Copy Started.
        ...............
        The total progress of copy operation is 0.00%.
        . . .
        The total progress of copy operation is 100.00%.
        Copy Completed.

1. また、あるコンテナーのすべての BLOB を Data Lake Store アカウントにコピーするには、次のコマンドも使用できます。

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/ /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>        

    次に例を示します。

        AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== 

    

## Data Lake Analytics アカウントで AdlCopy を使用する

Data Lake Analytics アカウントを使用して、AdlCopy ジョブを実行し、Azure Storage Blob のデータを Data Lake Store にコピーすることもできます。 通常、このオプションを使用するのは、移動対象のデータが GB および TB の範囲で、パフォーマンスのスループットを向上し、予測可能にする場合です。

Data Lake Analytics アカウントと AdlCopy を使用するには、ソース (Azure Storage Blob) と コピー先 (Azure Data Lake Store) の両方を Data Lake Analytics アカウントのデータ ソースとして追加する必要があります。 Data Lake 分析アカウントに追加のデータ ソースを追加する方法の詳細については、次を参照してください。 [Data Lake 分析の管理アカウントのデータ ソース](data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-account-data-sources)します。

次のコマンドを実行します。

    AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Account <data_lake_analytics_account> /Unit <number_of_data_lake_analytics_units_to_be_used>

次に例を示します。

    AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Account mydatalakeaccount /Units 2 

## 課金

* AdlCopy ツールをスタンドアロンとして使用する場合、ソースの Azure Storage アカウントが Data Lake Store と同じリージョン内になければ、データの移動に関する送信コストがかかります。

* 場合は、Data Lake 分析で AdlCopy ツールを使用するアカウントの場合、標準的な [料金を課金 Data Lake 分析](https://azure.microsoft.com/pricing/details/data-lake-analytics/) が適用されます。 

## 次のステップ

- [Data Lake Store のデータをセキュリティで保護する](data-lake-store-secure-data.md)
- [Data Lake Store で Azure Data Lake Analytics を使用する](data-lake-analytics-get-started-portal.md)
- [Data Lake Store で Azure HDInsight を使用する](data-lake-store-hdinsight-hadoop-use-portal.md)

