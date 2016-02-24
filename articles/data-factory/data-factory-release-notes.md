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
リソースの種類 | JSON での現在の名前 | JSON での新しい名前
------------- | -------------------- | ----------------
リンクされているサービス (データ ソース) | AzureSqlLinkedService | AzureSqlDatabase
リンクされているサービス (データ ソース) | AzureStorageLinkedService | AzureStorage
リンクされているサービス (データ ソース) | DocumentDbLinkedService | DocumentDb
リンクされているサービス (データ ソース) | OnPremisesFileSystemLinkedService | OnPremisesFileServer
リンクされているサービス (データ ソース) | OnPremisesOracleLinkedService | OnPremisesOracle
リンクされているサービス (データ ソース) | OnPremisesSqlLinkedService | OnPremisesSqlServer
リンクされているサービス (データ ソース) | OnPremisesMySqlLinkedService | OnPremisesMySql
リンクされているサービス (データ ソース) | OnPremisesDb2LinkedService | OnPremisesDb2
リンクされているサービス (データ ソース) | OnPremisesTeradataLinkedService | OnPremisesTeradata
リンクされているサービス (データ ソース) | OnPremisesSybaseLinkedService | OnPremisesSybase
リンクされているサービス (データ ソース) | OnPremisesPostgreSqlLinkedService | OnPremisesPostgreSql
リンクされているサービス (コンピューティング) | AzureMlLinkedService | AzureML
リンクされているサービス (コンピューティング) | HDInsightBYOCLinkedService | HDInsight
リンクされているサービス (コンピューティング) | HDInsightOnDemandLinkedService | HDInsightOnDemand
リンクされているサービス (コンピューティング) | AzureBatchLinkedService | AzureBatch
Dataset | AzureBlobLocation | AzureBlob
Dataset | AzureTableLocation | AzureTable
Dataset | AzureSqlTableLocation | AzureSqlTable
Dataset | DocumentDbCollectionLocation | DocumentDbCollection 
Dataset | OnPremisesFileSystemLocation | FileShare
Dataset | OnPremisesOracleTableLocation | OracleTable
Dataset | OnPremisesSqlServerTableLocation | SqlServerTable
Dataset | RelationTableLocation | RelationalTable
アクティビティ | CopyActivity | コピー
アクティビティ | HDInsightActivity (Hive 変換) | HDInsightHive
アクティビティ | HDInsightActivity (Pig 変換) | HDInsightPig
アクティビティ | HDInsightActivity (MapReduce 変換) | HDInsightMapReduce
アクティビティ | HDInsightActivity (ストリーミング) | HDInsightHadoopStreaming
アクティビティ | AzureMLBatchScoringActivity | AzureMLBatchScoring
アクティビティ | StoredProcedureActivity | SqlServerStoredProcedure

## 新しい typeProperties 要素
新しい **typeProperties** 要素にリンクされたサービス/テーブル/アクティビティの型固有のプロパティが含まれています。 

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

-  **型** プロパティは、1 レベル上およびに設定されている **AzureStorage** (から変更 **AzureStorageLinkedService** に **AzureStorage**) 
- 新しい **typeProperties** を Azure ストレージのリンクされたサービスでサポートされるプロパティを含む要素 (**connectionString** この例では)。  

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

-  **型** プロパティは 1 レベル上に移動し、型名 **AzureTableLocation** に変更されました **AzureTable**します。
-  **LinkedServiceName** は 1 レベル上に移動します。 
-  **場所** 要素が削除されるなど、型固有のプロパティをで **tableName** で指定された、 **場所** 新しいセクションが指定された **typeProperties** セクションです。  

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

- 注意して、 **変換** 要素は、新しいに置き換えられました **typeProperties** 要素。

## waitOnExternal 要素が削除されている
 **WaitOnExternal** に新しい要素を交換 **外部** と **externalData** プロパティです。        

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

-  **WaitOnExternal** からプロパティを削除、 **可用性** セクション 
- 新しい **外部** プロパティは、1 レベル上に追加しに設定されている **true** 、外部テーブルに対してです。 
- プロパティ **waitOnExternal** などの要素 **retryInterval** を新しい追加 **externalData** セクション、 **ポリシー** 要素。
-  **ExternalData** 要素は省略可能な要素です。 
- 使用すると、 **externalData** 要素が必要、 **外部** プロパティに設定 **true**します。 
 

## BlobSink の新しい copyBehavior プロパティ
 **BlobSink** という名前の新しいプロパティをサポートしています: **copyBehavior**します。 ソースがの場合、このプロパティがコピー動作を定義 **BlobSource** または **FileSystem**します。 3 つの値がある、 **copyBehavior** プロパティです。 

**PreserveHierarchy**:: 保持対象のフォルダー、つまり、ソース フォルダーに対するソース ファイルの相対パスのファイル階層はターゲット フォルダーに対するターゲット ファイルの相対パスと同じです。 


**FlattenHierarchy**:: ソース フォルダーからすべてのファイルは、ターゲット フォルダーの最初のレベルになります。 ターゲット ファイルは、自動生成された名前になります。 


**MergeFiles**: ソース フォルダーからすべてのファイルを 1 つのファイルにマージします。 ファイル/Blob の名前を指定した場合、マージされたファイル名は指定した名前になります。それ以外は自動生成されたファイル名になります。 
 
## HDInsight のすべてのアクティビティの新しい getDebugInfo プロパティ
HDInsight のアクティビティ (Hive、Pig、MapReduce、Hadoop ストリーミング) をサポートして、新しいプロパティ:  **getDebugInfo** プロパティです。  **GetDebugInfo** プロパティは省略可能な要素です。 設定すると **エラー**, 、ログの実行エラー時にのみダウンロードされます。 設定すると **すべて**, 、ログが実行状態に関係なく常にダウンロードします。 設定すると **なし**, 、ログはダウンロードされません。

  
     

## Data Factory の 2015 年 04 月 10 日リリース ノート
表示されます、 **最近更新したスライス** と **最近失敗したスライス** 上に一覧表示、 **テーブル** ブレードようになりました。 これらの一覧は、スライスの更新時間で並べ替えられます。 次の状況では、スライスの更新時刻が変更されます。    

-  スライスのステータス手動で更新するなどを使用して、 **セット AzureRmDataFactorySliceStatus** () をクリックして **実行** 上、 **スライス** のあるスライス] ブレードです。
-  スライスの実行 (実行の開始、実行の終了と失敗、実行の終了と成功など) により、スライスの状態が変わります。

一覧のタイトルをクリックしてまたは **.(省略記号)** スライスの大規模な一覧を表示します。 クリックして **フィルター** 、ツールバーのスライスをフィルター処理します。
 
クリックして、スライスを時間で並べ替えられたスライスを表示することもできます。 **データ スライス (スライスの時刻) 別** を並べて表示します。 これらのコレクションのスライスは、スライスの時間で並べ替えられます。 たとえば、1 時間ごとのスケジュールがある場合、スライスは次のようになります。
- 2015 年 4 月 4 日進行中の午後 5 時 
- 2015 年 4 月 4 日の午後 4 時が成功しました
- 2015 年 4 月 4 日午後 3 時失敗

ただし、使用していないスライスを再実行した場合、ユーザーがそのスライスに最も関心がある場合でも、そのスライスは一覧の先頭に表示されません。

## Data Factory の 2015 年 3 月 31 日リリース ノート
- 更新 **Data Management Gateway** にインストール パッケージが投稿されました [Microsoft ダウンロード センター][adf-gateway-download]します。
- コピー **内部設置型ファイル システムの Azure blob** がサポートされています。 詳細については、次のトピックを参照してください。
    -  [オンプレミス ファイル システムのリンクされたサービス](https://msdn.microsoft.com/library/dn930836.aspx)
    -  [JSON テーブルの OnPremisesFileSystemLocation プロパティ](https://msdn.microsoft.com/library/dn894089.aspx#OnPremFileSystem)
    -  [サポートされているソースとシンク](https://msdn.microsoft.com/library/dn894007.aspx)します。 更新されたコピー マトリックスを表示し、 **FileSystemSource** プロパティです。 
-  コピー **内部設置型 Oracle データベースから Azure blob に** がサポートされています。 詳細については、次のトピックを参照してください。 
    -  [オンプレミスの Oracle のリンクされたサービス](https://msdn.microsoft.com/library/dn948537.aspx)
    -  [JSON テーブルの OnPremisesOracleTableLocation プロパティ](https://msdn.microsoft.com/library/dn894089.aspx#Oracle) 
    -  [サポートされているソースとシンク](https://msdn.microsoft.com/library/dn894007.aspx)します。 更新されたコピー マトリックスを表示し、 **OracleSource** プロパティです。
-  Azure BLOB 内のテキスト ファイルのエンコードを指定できます。 新しい表示 [encodingName プロパティ](https://msdn.microsoft.com/library/dn894089.aspx#AzureBlob)します。 
- SQL シンクにコピーするときに、追加のパラメーターを使用してストアド プロシージャを呼び出すことができます。    

Blob の投稿を参照してください: [Azure Data Factory の更新 - 新しいデータ ストア](http://azure.microsoft.com/blog/2015/03/30/azure-data-factory-update-new-data-stores/) の例を含む追加の情報です。  

## Data Factory の 2015 年 2 月 27 日リリース ノート

### 新機能
- **Azure Data Factory エディター**します。 Azure ポータルの一部である Data Factory エディターでは、リンクされたサービス、データ セット、パイプラインを定義する JSON ファイルを作成、編集、デプロイできます。このエディターの主な目的は、高速かつ軽量のユーザー インターフェイス (UI) を使用して、Azure PowerShell をインストールしたり PowerShell コマンドレットを導入せずに Azure Data Factory アーティファクトを作成することです。 参照してください、 [Azure Data Factory エディター - 軽量 Web エディター][adf-editor-blog] ブログの投稿の概要とビデオでは、Data Factory エディターをします。 エディターの詳細については、次を参照してください。、 [Data Factory エディター][adf-editor] 記事です。          

### 変更点

## Data Factory の 2015 年 1 月 26 日リリース ノート ##

### 変更点
- 更新 **Data Management Gateway** にインストール パッケージが投稿されました [Microsoft ダウンロード センター][adf-gateway-download]します。 今回のリリース以降、このダウンロード元から Azure Data Factory で使用できる最新の Data Management Gateway を検索できるようになりました。 このインストール パッケージは、Azure Data Factory と Power BI for Office 365 サービスの両方で機能します。 両方のサービスを使用している場合は、Data Factory と Power BI のゲートウェイを別のコンピューターにインストールし、Data Factory または Power BI のドキュメントのガイダンスに従って個別に構成する必要があります。
-  **コピー アクティビティ** 内部設置型 SQL Server データベースと Azure SQL データベース間でデータのコピーがサポートようになりました。 
- **SqlSink** 新しいプロパティをサポートしています: **WriteBatchTimeout**します。 このプロパティを使用すると、操作がタイムアウトになる前にバッチ挿入操作が完了するまでの待機時間を柔軟に構成できます。 ハイブリッド コピー (オンプレミスのデータ ソースとクラウドのデータ ソースが関連するコピー操作) の場合、このプロパティを使用するには 1.4 以上のバージョンのゲートウェイが必要です。 
- **SQL Server リンク サービス** ようになりました **Windows 認証**します。 
    - ポータルを使用して SQL Server のリンクされたサービスを作成する場合、Windows 認証を使用して適切な資格情報を設定できるようになりました。 この設定には 1.4 以上のバージョンのゲートウェイが必要です。 
    - Azure PowerShell を使用してサービスをリンクされた SQL Server を作成する、ときに、プレーン テキストで接続情報を指定または更新を使用して接続情報を暗号化 [新規 AzureRmDataFactoryEncryptValue コマンドレット](https://msdn.microsoft.com/library/mt603802.aspx) し、暗号化された接続文字列を使用してリンクされたサービスの JSON ペイロードの接続文字列プロパティです。 現時点では、暗号化機能は New-AzureRmDataFactoryEncryptValue コマンドレットでサポートされていません。 

## Data Factory の 2014 年 12 月 11 日リリース ノート ##

### 新機能

- Azure Machine Learning 統合
    - Azure Data Factory サービスのこのリリースでは、Azure Data Factory を使用して Azure Machine Learning (ML) と統合することができます **AzureMLLinkedService** と **AzureMLBatchScoringActivity**します。 参照してください [Data Factory および Azure Machine Learning を使用して予測パイプラインを作成する][adf-azure-ml] 詳細です。 
- ゲートウェイ バージョンのステータスが提供される
    - 現在インストールされているゲートウェイよりも新しいバージョンのゲートウェイを使用できる場合は、"NewVersionAvailable" ステータスが Azure ポータルおよび Get-AzureRmDataFactoryGateway コマンドレットの出力に表示されます。 ことができますし、ポータルの指示に従って、新しいインストール ファイル (.msi) をダウンロードして実行して最新のゲートウェイをインストールします。 追加の構成は必要ありません。

### 変更点

- HdInsightOnDemandLinkedService 内の JobsContainer が削除されました。
    - HDInsightOnDemandLinkedService の JSON 定義では指定する必要はありません **jobsContainer** プロパティです。 オンデマンドのリンクされたサービスで指定したプロパティがある場合、そのプロパティは無視されます。 リンクされたサービスの JSON 定義からそのプロパティを削除し、New-AzureRmDataFactoryLinkedService コマンドレットを使用してリンクされたサービスの定義を更新できます。
- HDInsightOnDemandLinkedService のオプションの構成パラメーター
    - このリリースでは、HDInsightOnDemandLinked (オンデマンドの HDInsight クラスター) 用の複数のオプションの構成パラメーターのサポートが導入されています。 参照してください [ClusterCreateParameters プロパティ][on-demand-hdi-parameters] 詳細です。
- ゲートウェイの場所が削除される
    - ポータルまたは PowerShell (New-AzureRmDataFactoryGateway) 経由で Azure Data Factory ゲートウェイを作成する場合は、ゲートウェイの場所を指定する必要がありません。 Data Factory 領域は継承されます。 同様に、JSON を使用して SQL Server のリンクされたサービスを構成する場合、"gatewayLocation" プロパティは必要ありません。 Data Factory .NET SDK もこれらの変更点を反映して更新されます。
    - 古いバージョンの SDK および Azure PowerShell を使用している場合は、引き続き場所を設定する必要があります。
 
     

#### 重大な変更
    
- CustomActivity から DotNetActivity へ
    - **ICustomActivity** インターフェイスの名前を変更する **IDotNetActivity**します。 Data Factory NuGet パッケージを更新し、カスタム アクティビティのソース コードで、ICustomActivity を IDotNetActivity に変更する必要があります。  
    - カスタム アクティビティの JSON 定義におけるカスタム アクティビティの種類を変更する必要があります **CustomActivity** に **DotNetActivity**します。 
    -  **CustomActivity** と **CustomActivityProperties** クラスに変更されました **DotNetActivity** と **DotNetActivityProperties** と同じプロパティ セット。

        古いバージョンの SDK および Azure PowerShell を使用している場合は、DotNetActivity の代わりに、引き続き CustomActivity を使用できます。
    
        参照してください [Azure Data Factory パイプラインでカスタム アクティビティを使用して][adf-custom-activities] カスタム アクティビティを作成し、Azure Data Factory パイプラインで使用する方法のチュートリアルです。  

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



 

