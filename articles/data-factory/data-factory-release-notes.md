<properties 
    pageTitle="Data Factory - リリース ノート | Microsoft Azure" 
    description="Data Factory - リリース ノート" 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/09/2015" 
    ms.author="spelluru"/>


# Azure Data Factory リリース ノート

参照してください、 [Data Factory - .NET API の変更ログ](data-factory-api-change-log.md) 記事を特定のリリースで Data Factory .NET SDK の変更点について説明します。

## Data Factory の 2015 年 7 月 17 日リリース ノート

2015 年 7 月リリースの Azure PowerShell では、次の JSON の変更が導入されています。

## リンクされているサービス、テーブル、およびアクティビティの種類の更新

 リソースの種類| JSON での現在の名前| JSON での新しい名前
------------- | -------------------- | ----------------
 リンクされているサービス (データ ソース)| AzureSqlLinkedService| AzureSqlDatabase
 リンクされているサービス (データ ソース)| AzureStorageLinkedService| AzureStorage
 リンクされているサービス (データ ソース)| DocumentDbLinkedService| DocumentDb
 リンクされているサービス (データ ソース)| OnPremisesFileSystemLinkedService| OnPremisesFileServer
 リンクされているサービス (データ ソース)| OnPremisesOracleLinkedService| OnPremisesOracle
 リンクされているサービス (データ ソース)| OnPremisesSqlLinkedService| OnPremisesSqlServer
 リンクされているサービス (データ ソース)| OnPremisesMySqlLinkedService| OnPremisesMySql
 リンクされているサービス (データ ソース)| OnPremisesDb2LinkedService| OnPremisesDb2
 リンクされているサービス (データ ソース)| OnPremisesTeradataLinkedService| OnPremisesTeradata
 リンクされているサービス (データ ソース)| OnPremisesSybaseLinkedService| OnPremisesSybase
 リンクされているサービス (データ ソース)| OnPremisesPostgreSqlLinkedService| OnPremisesPostgreSql
 リンクされているサービス (コンピューティング)| AzureMlLinkedService| AzureML
 リンクされているサービス (コンピューティング)| HDInsightBYOCLinkedService| HDInsight
 リンクされているサービス (コンピューティング)| HDInsightOnDemandLinkedService| HDInsightOnDemand
 リンクされているサービス (コンピューティング)| AzureBatchLinkedService| AzureBatch
 Dataset| AzureBlobLocation| AzureBlob
 Dataset| AzureTableLocation| AzureTable
 Dataset| AzureSqlTableLocation| AzureSqlTable
 Dataset| DocumentDbCollectionLocation| DocumentDbCollection
 Dataset| OnPremisesFileSystemLocation| FileShare
 Dataset| OnPremisesOracleTableLocation| OracleTable
 Dataset| OnPremisesSqlServerTableLocation| SqlServerTable
 Dataset| RelationTableLocation| RelationalTable
 アクティビティ| CopyActivity| コピー
 アクティビティ| HDInsightActivity (Hive 変換)| HDInsightHive
 アクティビティ| HDInsightActivity (Pig 変換)| HDInsightPig
 アクティビティ| HDInsightActivity (MapReduce 変換)| HDInsightMapReduce
 アクティビティ| HDInsightActivity (ストリーミング)| HDInsightHadoopStreaming
 アクティビティ| AzureMLBatchScoringActivity| AzureMLBatchScoring
 アクティビティ| StoredProcedureActivity| SqlServerStoredProcedure

## 新しい typeProperties 要素

新しい **typeProperties** 要素には、リンクされているサービス/テーブル/アクティビティの型固有のプロパティが含まれています。

### 古いリンクされたサービスの JSON

    {
        "name": "StorageLinkedService",
        "properties":
        {
            "type": "AzureStorageLinkedService",
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>" "
        }
    }

### 新しいリンクされたサービスの JSON

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
      }
    }

以下の点に注意してください。

- **type** プロパティは 1 レベル上に移動し、**AzureStorage** (**AzureStorageLinkedService** から **AzureStorage** に変更) に設定されます。
- 新しい **typeProperties** 要素には、Azure Storage のリンクされたサービスでサポートされているプロパティが含まれています (この例では **connectionString**)。

### 古いデータセットの JSON

    {
        "name": "EmpTable",
        "properties":
        {
            "location":
            {
                "type": "AzureTableLocation",
                "tableName": "myazuretable",
                "linkedServiceName": "MyLinkedService"
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }

### 新しいデータセットの JSON

    {
        "name": "EmpTable",
        "properties": {
            "type": "AzureTable",
            "linkedServiceName": "MyLinkedServiceName",
            "typeProperties": {
                "tableName": "myazuretable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }

次に注意してください。

- **type** プロパティは 1 レベル上に移動し、型の名前 **AzureTableLocation** が **AzureTable** に変更されています。
- **linkedServiceName**は 1 レベル上に移動します。
- **location** 要素は削除され、**location** セクションで指定された **tableName** などの型固有のプロパティが新しい **typeProperties** セクションで指定されています。

### 古いアクティビティの JSON

    {
        "name": "CopyFromSQLtoBlob   ",
        "description": "description", 
        "type": "CopyActivity",
        "inputs":  [ { "name": "InputSqlDA"  } ],
        "outputs":  [ { "name": "OutputBlobDA" } ],
        "transformation":
        {
            "source":
            {
                "type": "SqlSource",
                "sqlReaderQuery": "select * from MyTable"
            },
            "sink":
            {
                "type": "BlobSink"
            }
        }
    }   

### 新しいアクティビティの JSON

    {
        "name": "CopyFromSQLtoBlob   ",
        "description": "description", 
        "type": "Copy",
        "inputs":  [ { "name": "InputSqlDA"  } ],
        "outputs":  [ { "name": "OutputBlobDA" } ],
        "typeProperties":
        {
            "source":
            {
                "type": "SqlSource",
                "sqlReaderQuery": "select * from MyTable"
            },
            "sink":
            {
                "type": "BlobSink"
            }
        }
    }

次に注意してください。

- **transformation** 要素が新しい **typeProperties** 要素に置き換えられています。

## waitOnExternal 要素が削除されている

**waitOnExternal** 要素は新しい **external** と **externalData** プロパティに置き換えられています。

### 古い JSON

    {
        "name": "EmpTableFromBlob",
        "properties":
        {
            "location": 
            {
                "type": "AzureBlobLocation",
                "folderPath": "adftutorial/",
                "format":
                {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                },
                "linkedServiceName": "StorageLinkedService"
            },
            "availability": 
            {
                "frequency": "hour",
                "interval": 1,
                "waitOnExternal": 
                {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": "3"         
                }
            }
        }
    } 

### 新しい JSON

    {
      "name": "EmpTableFromBlob",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "adftutorial/",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "external": true,
        "availability": {
          "frequency": "hour",
          "interval": 1
        },
        "policy": {
          "externalData": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": "3"
          }
        }
      }
    }

次に注意してください。

- **waitOnExternal** プロパティは **availability** セクションから削除されています。
- 新しい **external** プロパティは 1 レベル上に追加され、外部テーブルに対して **true** に設定されています。
- **retryInterval** などの **waitOnExternal** 要素のプロパティは、**Policy** 要素の新しい **externalData** セクションに追加されています。
- **externalData** 要素は省略可能な要素です。
- **externalData** 要素を使用する場合は、**external** プロパティを **true** に設定する必要があります。


## BlobSink の新しい copyBehavior プロパティ

**BlobSink** は **copyBehavior** という名前の新しいプロパティをサポートしています。 このプロパティは、ソースが **BlobSource** または **FileSystem** である場合のコピー動作を定義します。 **copyBehavior** プロパティには次の 3 つの値を指定できます。

**PreserveHierarchy**: ターゲット フォルダーのファイル階層を保持します。つまり、ソース フォルダーに対するソース ファイルの相対パスと、ターゲット フォルダーに対するターゲット ファイルの相対パスが一致します。


**FlattenHierarchy**: ソース フォルダーのすべてのファイルがターゲット フォルダーの最初のレベルになります。 ターゲット ファイルは、自動生成された名前になります。


**MergeFiles**: ソース フォルダーのすべてのファイルを 1 つのファイルにマージします。 ファイル/Blob の名前を指定した場合、マージされたファイル名は指定した名前になります。それ以外は自動生成されたファイル名になります。

## HDInsight のすべてのアクティビティの新しい getDebugInfo プロパティ

HDInsight のアクティビティ (Hive、Pig、MapReduce、Hadoop ストリーミング) をサポートして、新しいプロパティ:  **getDebugInfo** プロパティです。 **getDebugInfo** プロパティは、省略可能な要素です。 **Failure** に設定されていると、ログは実行エラー時にのみダウンロードされます。 **All** に設定されていると、ログは実行状態に関係なく常にダウンロードされます。 **None** に設定されていると、ログはダウンロードされません。




## Data Factory の 2015 年 04 月 10 日リリース ノート

**[テーブル]** ブレードに **[最近更新したスライス]** と **[最近失敗したスライス]** の一覧が表示されるようになりました。 これらの一覧は、スライスの更新時間で並べ替えられます。 次の状況では、スライスの更新時刻が変更されます。

-  **Set-AzureRmDataFactorySliceStatus** を使用したり、スライスの **[スライス]** ブレードで **[実行]** をクリックしたりすることで、スライスの状態を手動で更新した場合。
-  スライスの実行 (実行の開始、実行の終了と失敗、実行の終了と成功など) により、スライスの状態が変わります。

一覧のタイトルをクリックするか、**[...] (省略記号)** をクリックすると、さらに多くのスライスが一覧表示されます。 スライスをフィルター処理するには、ツールバーの **[フィルター]** をクリックします。

**[データ スライス (スライスの時刻別)]** タイルをクリックして、スライスを時間別に並べ替えて表示することもできます。 これらのコレクションのスライスは、スライスの時間で並べ替えられます。 たとえば、1 時間ごとのスケジュールがある場合、スライスは次のようになります。
- 2015 年 4 月 4 日進行中の午後 5 時
- 2015 年 4 月 4 日の午後 4 時が成功しました
- 2015 年 4 月 4 日午後 3 時失敗

ただし、使用していないスライスを再実行した場合、ユーザーがそのスライスに最も関心がある場合でも、そのスライスは一覧の先頭に表示されません。

## Data Factory の 2015 年 3 月 31 日リリース ノート

- 更新 **Data Management Gateway** にインストール パッケージが投稿されました [Microsoft ダウンロード センター ][adf-gateway-download]します。
- **オンプレミスのファイル システムから Azure BLOB への**コピーがサポートされるようになりました。 詳細については、次のトピックを参照してください。
    -  [内部設置型ファイル システムのリンクされたサービス](https://msdn.microsoft.com/library/dn930836.aspx)
    -  [JSON テーブルの OnPremisesFileSystemLocation プロパティ](https://msdn.microsoft.com/library/dn894089.aspx#OnPremFileSystem)
    -  [サポートされているソースとシンク](https://msdn.microsoft.com/library/dn894007.aspx)します。 更新されたコピー マトリックスと **FileSystemSource** プロパティを参照してください。
-  **オンプレミスの Oracle データベースから Azure BLOB への**コピーがサポートされるようになりました。 詳細については、次のトピックを参照してください。
    -  [内部設置型 Oracle のリンクされたサービス](https://msdn.microsoft.com/library/dn948537.aspx)
    -  [JSON テーブルの OnPremisesOracleTableLocation プロパティ](https://msdn.microsoft.com/library/dn894089.aspx#Oracle)
    -  [サポートされているソースとシンク](https://msdn.microsoft.com/library/dn894007.aspx)します。 更新されたコピー マトリックスと **OracleSource** プロパティを参照してください。
-  Azure BLOB 内のテキスト ファイルのエンコードを指定できます。 新しい表示 [encodingName プロパティ](https://msdn.microsoft.com/library/dn894089.aspx#AzureBlob)します。
- SQL シンクにコピーするときに、追加のパラメーターを使用してストアド プロシージャを呼び出すことができます。

Blob の投稿を参照してください: [Azure Data Factory の更新 - 新しいデータ ストア](http://azure.microsoft.com/blog/2015/03/30/azure-data-factory-update-new-data-stores/) の例を含む追加の情報です。

## Data Factory の 2015 年 2 月 27 日リリース ノート

### 新機能

- **Azure Data Factory エディター** Azure ポータルの一部である Data Factory エディターでは、リンクされたサービス、データ セット、パイプラインを定義する JSON ファイルを作成、編集、デプロイできます。このエディターの主な目的は、高速かつ軽量のユーザー インターフェイス (UI) を使用して、Azure PowerShell をインストールしたり PowerShell コマンドレットを導入せずに Azure Data Factory アーティファクトを作成することです。 参照してください、 [Azure Data Factory エディター - 軽量 Web エディター ][adf-editor-blog] ブログの投稿の概要とビデオでは、Data Factory エディターをします。 エディターの詳細については、次を参照してください。、 [Data Factory エディター ][adf-editor] 記事です。

### 変更点

## Data Factory の 2015 年 1 月 26 日リリース ノート

### 変更点

- 更新 **Data Management Gateway** にインストール パッケージが投稿されました [Microsoft ダウンロード センター ][adf-gateway-download]します。 今回のリリース以降、このダウンロード元から Azure Data Factory で使用できる最新の Data Management Gateway を検索できるようになりました。 このインストール パッケージは、Azure Data Factory と Power BI for Office 365 サービスの両方で機能します。 両方のサービスを使用している場合は、Data Factory と Power BI のゲートウェイを別のコンピューターにインストールし、Data Factory または Power BI のドキュメントのガイダンスに従って個別に構成する必要があります。
- **コピー アクティビティ**で、オンプレミスの SQL Server データベースと Azure SQL Database の間でのデータのコピーがサポートされるようになりました。
- **SqlSink** で新しいプロパティ **WriteBatchTimeout** がサポートされるようになりました。 このプロパティを使用すると、操作がタイムアウトになる前にバッチ挿入操作が完了するまでの待機時間を柔軟に構成できます。 ハイブリッド コピー (オンプレミスのデータ ソースとクラウドのデータ ソースが関連するコピー操作) の場合、このプロパティを使用するには 1.4 以上のバージョンのゲートウェイが必要です。
- **SQL Server のリンクされたサービス** で、**Windows 認証**がサポートようになりました。
    - ポータルを使用して SQL Server のリンクされたサービスを作成する場合、Windows 認証を使用して適切な資格情報を設定できるようになりました。 この設定には 1.4 以上のバージョンのゲートウェイが必要です。
    - Azure PowerShell を使用してサービスをリンクされた SQL Server を作成する、ときに、プレーン テキストで接続情報を指定または更新を使用して接続情報を暗号化 [新規 AzureRmDataFactoryEncryptValue コマンドレット](https://msdn.microsoft.com/library/mt603802.aspx) し、暗号化された接続文字列を使用してリンクされたサービスの JSON ペイロードの接続文字列プロパティです。 現時点では、暗号化機能は New-AzureRmDataFactoryEncryptValue コマンドレットでサポートされていません。

## Data Factory の 2014 年 12 月 11 日リリース ノート

### 新機能

- Azure Machine Learning 統合
    - Azure Data Factory サービスのこのリリースでは、**AzureMLLinkedService** と **AzureMLBatchScoringActivity** を使用して、Azure Data Factory と Azure Machine Learning (ML) を統合できます。 参照してください [Data Factory および Azure Machine Learning ][adf-azure-ml] 詳細です。
- ゲートウェイ バージョンのステータスが提供される
    - 現在インストールされているゲートウェイよりも新しいバージョンのゲートウェイを使用できる場合は、"NewVersionAvailable" ステータスが Azure ポータルおよび Get-AzureRmDataFactoryGateway コマンドレットの出力に表示されます。 ことができますし、ポータルの指示に従って、新しいインストール ファイル (.msi) をダウンロードして実行して最新のゲートウェイをインストールします。 追加の構成は必要ありません。

### 変更点

- HdInsightOnDemandLinkedService 内の JobsContainer が削除されました。
    - HDInsightOnDemandLinkedService の JSON 定義で、**jobsContainer** プロパティを指定する必要がありません。 オンデマンドのリンクされたサービスで指定したプロパティがある場合、そのプロパティは無視されます。 リンクされたサービスの JSON 定義からそのプロパティを削除し、New-AzureRmDataFactoryLinkedService コマンドレットを使用してリンクされたサービスの定義を更新できます。
- HDInsightOnDemandLinkedService のオプションの構成パラメーター
    - このリリースでは、HDInsightOnDemandLinked (オンデマンドの HDInsight クラスター) 用の複数のオプションの構成パラメーターのサポートが導入されています。 参照してください [ClusterCreateParameters プロパティ ][on-demand-hdi-parameters] 詳細です。
- ゲートウェイの場所が削除される
    - ポータルまたは PowerShell (New-AzureRmDataFactoryGateway) 経由で Azure Data Factory ゲートウェイを作成する場合は、ゲートウェイの場所を指定する必要がありません。 Data Factory 領域は継承されます。 同様に、JSON を使用して SQL Server のリンクされたサービスを構成する場合、"gatewayLocation" プロパティは必要ありません。 Data Factory .NET SDK もこれらの変更点を反映して更新されます。
    - 古いバージョンの SDK および Azure PowerShell を使用している場合は、引き続き場所を設定する必要があります。



#### 重大な変更

- CustomActivity から DotNetActivity へ
  - **ICustomActivity** インターフェイスの名前が **IDotNetActivity** に変更されました。 Data Factory NuGet パッケージを更新し、カスタム アクティビティのソース コードで、ICustomActivity を IDotNetActivity に変更する必要があります。
  - カスタム アクティビティの JSON 定義におけるカスタム アクティビティの種類を、**CustomActivity** から **DotNetActivity** に変更する必要があります。
  - **CustomActivity** と **CustomActivityProperties** クラスの名前は、それぞれ、**DotNetActivity** と **DotNetActivityProperties** に変更されました (プロパティ セットは変わりません)。

    If you use the older verion of SDK and Azure PowerShell, you can continue using CustomActivity instead of DotNetActivity.
    
    See [Use custom activities in an Azure Data Factory pipeline][adf-custom-activities] for a walkthrough on how to create a custom activity and use it in an Azure Data Factory pipeline.  



[adf-azure-ml]: data-factory-create-predictive-pipelines.md 
[adf-custom-activities]: data-factory-use-custom-activities.md 
[adf-editor]: data-factory-editor.md 
[adf-editor-video]: http://channel9.msdn.com/Blogs/Windows-Azure/New-Azure-Data-Factory-Editor-UI 
[adf-editor-blog]: http://azure.microsoft.com/blog/2015/03/02/azure-data-factory-editor-a-light-weight-web-editor/ 
[on-demand-hdi-parameters]: http://msdn.microsoft.com/library/microsoft.windowsazure.management.hdinsight.clustercreateparameters_properties.aspx 
[adf-gateway-download]: http://www.microsoft.com/download/details.aspx?id=39717 
[adf-github-samples]: https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON 
[adf-msdn-linked-services]: https://msdn.microsoft.com/library/dn834986.aspx 
[adf-encrypt-value-cmdlet]: https://msdn.microsoft.com/library/dn834940.aspx 

