<properties 
    pageTitle="Azure BLOB との間でのデータの移動 | Azure Data Factory" 
    description="Azure Data Factory を使用して Azure BLOB Storage に、または Azure BLOB Storage からデータを移動する方法を説明します。" 
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
    ms.date="11/03/2015" 
    ms.author="spelluru"/>

# Azure Data Factory を使用した Azure BLOB との間でのデータの移動
この記事では、Azure Data Factory のコピー アクティビティを利用し、Azure BLOB と別のデータ ストアの間でデータを移動する方法について説明します。 この記事に基づき、 [データ移動アクティビティ](data-factory-data-movement-activities.md) コピー アクティビティとサポートされているデータ ストアの組み合わせでデータ移動の一般概要を説明する記事です。

## サンプル: Azure BLOB から Azure SQL にデータをコピーする
下のサンプルで確認できる要素:

1.  型のリンクされたサービス [AzureSqlDatabase](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties)します。
2.  型のリンクされたサービス [AzureStorage](#azure-storage-linked-service-properties)します。
3.  入力 [データセット](data-factory-create-datasets.md) 型の [AzureBlob](#azure-blob-dataset-type-properties)します。
4.  出力 [データセット](data-factory-create-datasets.md) 型の [AzureSqlTable](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties)します。
4.  A [パイプライン](data-factory-create-pipelines.md) を使用するコピー アクティビティの [BlobSource](#azure-blob-copy-activity-type-properties) と [SqlSink](data-factory-azure-sql-connector.md#azure-sql-copy-activity-type-properties)します。

このサンプルはある時系列に属するデータを 1 時間おきに Azure BLOB から Azure SQL Database のテーブルにコピーします。 これらのサンプルで使用される JSON プロパティの説明はサンプルに続くセクションにあります。 

**Azure SQL のリンクされたサービス:**

    {
      "name": "AzureSqlLinkedService",
      "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

**Azure Storage のリンクされたサービス:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure BLOB の入力データセット:**

データは新しい BLOB から 1 時間おきに取得されます (頻度: 時間、間隔: 1)。 BLOB のフォルダー パスとファイル名は、処理中のスライスの開始時間に基づき、動的に評価されます。 フォルダーのパスは、年、月、および日の開始時刻部分を使用し、ファイル名は開始時刻の時間部分を使用します。 "external":"true"」設定は、このテーブルがデータ ファクトリに対して外部にあり、data factory 内のアクティビティでは生成されないことを Data Factory サービスに通知します。

    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
          "fileName": "{Hour}.csv",
          "partitionedBy": [
            {
              "name": "Year",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyy"
              }
            },
            {
              "name": "Month",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "%M"
              }
            },
            {
              "name": "Day",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "%d"
              }
            },
            {
              "name": "Hour",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "%H"
              }
            }
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": "\n"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "externalData": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
          }
        }
      }
    }

**Azure SQL の出力データセット:**

このサンプルは Azure SQL Database の「MyTable」というテーブルにデータをコピーします。 BLOB CSV ファイルに含めることが予想される数の列で Azure SQL Database にテーブルを作成する必要があります。 新しい行は 1 時間ごとにテーブルに追加されます。

    {
      "name": "AzureSqlOutput",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**コピー アクティビティのあるパイプライン:**

上記の入力データセットと出力データセットを使用するように構成され、1 時間おきに実行するようにスケジュールされているコピー アクティビティがパイプラインに含まれています。 パイプライン JSON 定義で、 **ソース** に設定されている型 **BlobSource** と **シンク** に設定されている型 **SqlSink**します。 

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureSqlOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource",
                "blobColumnSeparators": ","
              },
              "sink": {
                "type": "SqlSink"
              }
            },
           "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
          ]
       }
    }

## サンプル: Azure SQL から Azure BLOB にデータをコピーする
下のサンプルで確認できる要素:

1.  型のリンクされたサービス [AzureSqlDatabase](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties)します。
2.  型のリンクされたサービス [AzureStorage](#azure-storage-linked-service-properties)します。
3.  入力 [データセット](data-factory-create-datasets.md) 型の [AzureSqlTable](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties)します。
4.  出力 [データセット](data-factory-create-datasets.md) 型の [AzureBlob](#azure-blob-dataset-type-properties)します。
4.  A [パイプライン](data-factory-create-pipelines.md) を使用するコピー アクティビティの [SqlSource](data-factory-azure-sql-connector.md#azure-sql-copy-activity-type-properties) と [BlobSink](#azure-blob-copy-activity-type-properties)します。


このサンプルはある時系列に属するデータを 1 時間おきに Azure SQL データベースのテーブルから BLOB にコピーします。 これらのサンプルで使用される JSON プロパティの説明はサンプルに続くセクションにあります。 

**Azure SQL のリンクされたサービス:**

    {
      "name": "AzureSqlLinkedService",
      "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

**Azure Storage のリンクされたサービス:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure SQL の入力データセット:**

このサンプルでは、Azure SQL で「MyTable」という名前のテーブルを作成し、時系列データ用に「timestampcolumn」という名前の列が含まれているものと想定しています。 

“external”: ”true” を設定して externalData ポリシーを指定すると、テーブルが Data Factory に対して外部にあり、Data Factory のアクティビティでは生成されてないことが Azure Data Factory のサービスに通知されます。

    {
      "name": "AzureSqlInput",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "externalData": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
          }
        }
      }
    }

**Azure BLOB の出力データセット:**

データは新しい BLOB に 1 時間おきに書き込まれます (頻度: 時間、間隔: 1)。 BLOB のフォルダー パスは、処理中のスライスの開始時間に基づき、動的に評価されます。 フォルダー パスは開始時間の年、月、日、時刻の部分を使用します。 
    
    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}/",
          "partitionedBy": [
            {
              "name": "Year",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyy"
              }
            },
            {
              "name": "Month",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "%M"
              }
            },
            {
              "name": "Day",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "%d"
              }
            },
            {
              "name": "Hour",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "%H"
              }
            }
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": "\t",
            "rowDelimiter": "\n"
          }
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**コピー アクティビティのあるパイプライン:**

上記の入力データセットと出力データセットを使用するように構成され、1 時間おきに実行するようにスケジュールされているコピー アクティビティがパイプラインに含まれています。 パイプライン JSON 定義で、 **ソース** に設定されている型 **SqlSource** と **シンク** に設定されている型 **BlobSink**します。 指定された SQL クエリ、 **SqlReaderQuery** プロパティをコピーするには過去のデータを選択します。


    {  
        "name":"SamplePipeline",
        "properties":{  
            "start":"2014-06-01T18:00:00",
            "end":"2014-06-01T19:00:00",
            "description":"pipeline for copy activity",
            "activities":[  
                {
                    "name": "AzureSQLtoBlob",
                    "description": "copy activity",
                    "type": "Copy",
                    "inputs": [
                      {
                        "name": "AzureSQLInput"
                      }
                    ],
                    "outputs": [
                      {
                        "name": "AzureBlobOutput"
                      }
                    ],
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource",
                            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "OldestFirst",
                        "retry": 0,
                        "timeout": "01:00:00"
                    }
                }
             ]
        }
    }

## Azure Storage のリンクされたサービスのプロパティ

Azure Storage のリンクされたサービスを利用し、Azure ストレージ アカウントを Azure Data Factory にリンクできます。 次の表は、Azure Storage のリンクされたサービスに固有の JSON 要素の説明をまとめたものです。

| プロパティ | 説明 | 必須 |
| -------- | ----------- | -------- |
| type | Type プロパティに設定する必要があります: **AzureStorage** | あり |
| connectionString | connectionString プロパティのために Azure ストレージに接続するために必要な情報を指定します。 Azure クラシック ポータルから Azure ストレージの connectionString を取得できます。 | あり |

## Azure BLOB データセットの type プロパティ

JSON セクションとデータセットの定義に使用できるプロパティの一覧については、次を参照してください。、 [データセットを作成する](data-factory-create-datasets.md) 記事です。 データセット JSON の構造、可用性、ポリシーなどのセクションはすべてのデータセット型 (Azure SQL、Azure BLOB、Azure テーブルなど) で同じです。

 **TypeProperties** セクションはデータセット型ごとに異なるを場所などに関する情報を提供書式データ ストア内のデータなどです。 型のデータセットの typeProperties セクション **AzureBlob** dataset には、次のプロパティが含まれています。

| プロパティ | 説明 | 必須 |
| -------- | ----------- | -------- | 
| folderPath | BLOB ストレージのコンテナーとフォルダーのパス。 例: myblobcontainer\myblobfolder\ | あり |
| fileName | <p>Blob の名前です。 ファイル名は省略できます。 </p><p>ファイル名を指定する場合、アクティビティ (コピーを含む) は、特定の Blob で動作します。</p><p>ファイル名が指定されていないとき、コピーは入力データセットの folderPath 内のすべての Blob を含めます。</p><p>生成されたファイルの名前を次にも見られる出力データセットに fileName が指定されていないときに次の形式: データ。<Guid>.txt (例:: 付けられます-93ff-4c6f-b3be-f69616f1df7a.txt</p> | いいえ |
| partitionedBy | partitionedBy は任意のプロパティです。 これを使用し、時系列データに動的な folderPath と fileName を指定できます。 たとえば、1 時間ごとのデータに対して folderPath をパラメーター化できます。 参照してください、 [partitionedBy プロパティのセクションを利用する](#Leveraging-partitionedBy-property) の下の詳細と例についてです。 | いいえ
| BlobSink の format | 2 種類の形式がサポートされている: **TextFormat**, 、**AvroFormat**します。 形式の下にある type プロパティをいずれかの値に設定する必要があります。 形式が TextFormat のとき、形式に追加で任意のプロパティを指定できます。 参照してください、 [TextFormat の指定](#specifying-textformat) 詳細については後述します。 | いいえ
| compression | データの圧縮の種類とレベルを指定します。 サポートされる種類: GZip、Deflate、および BZip2。サポートされるレベル: Optimal および Fastest。 参照してください [圧縮サポート](#compression-support) 詳細についてです。  | いいえ |

### partitionedBy プロパティの活用
前述のように、動的な folderPath と filename を含む時系列データを指定することができます、 **partitionedBy** セクション、Data Factory マクロ、およびシステム環境変数: SliceStart と SliceEnd、指定されたデータ スライスの開始および終了時刻を指定します。

参照してください [データ ファクトリのシステム変数](data-factory-scheduling-and-execution.md#data-factory-system-variables) と [Data Factory 関数リファレンス](data-factory-scheduling-and-execution.md#data-factory-functions-reference) Data Factory システム変数と partitionedBy セクションで使用できる関数について説明します。   

参照してください [データセットの作成](data-factory-create-datasets.md) と [スケジュールと実行](data-factory-scheduling-and-execution.md) 記事の時系列データセット、スケジュール設定の詳細については、スライスします。

#### サンプル 1

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy": 
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

上記の例では、{Slice} は、指定された形式 (YYYYMMDDHH) で、Data Factory システム変数の SliceStart の値で置換されます。 SliceStart はスライスの開始時刻です。 folderPath はスライスごとに異なります。 例: wikidatagateway/wikisampledataout/2014100103 または wikidatagateway/wikisampledataout/2014100104

#### サンプル 2

    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
     [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ],

上記の例では、SliceStart の年、月、日、時刻が folderPath プロパティと fileName プロパティで使用される個別の変数に抽出されます。

### TextFormat の指定

形式が設定されている場合 **TextFormat**, 、次を指定する **省略可能な** でプロパティ、 **形式** セクションです。

| プロパティ | 説明 | 必須 |
| -------- | ----------- | -------- |
| columnDelimiter | ファイルの列の区切り記号として使用される文字です。このタグは任意です。 既定値はコンマです (,)。 | いいえ |
| rowDelimiter | ファイルの行の区切り記号として使用される文字です。 このタグは任意です。 既定値は [“\r\n”, “\r”,” \n”] のいずれかになります。 | いいえ |
| escapeChar | <p>コンテンツに表示される列区切り記号のエスケープに使用される特殊文字です。 このタグは任意です。 既定値はありません。 このプロパティの 2 つ以上の文字を指定する必要があります。</p><p>たとえば、列区切り記号としてコンマ (,) がある場合、このコンマ文字をテキストにある (例:「こんにちは, world」)、エスケープ文字として '$' を定義して文字列を使用して「hello$, world」ソース。</p><p>EscapeChar と quoteChar のテーブルの両方を指定できないことに注意してください。</p> | いいえ | 
| quoteChar | <p>この特殊文字は文字列値を引用符で囲むために使用されます。 引用符文字内の列区切り文字と行区切り文字は文字列値の一部として処理されます。 このタグは任意です。 既定値はありません。 このプロパティの 2 つ以上の文字を指定する必要があります。</p><p>たとえば、列区切り記号としてコンマ (,) がある場合、このコンマ文字をテキストにある (例: < こんにちは, world >)、定義することができます '"' 引用符として文字し、ソースで文字列 <「こんにちは, world」> を使用します。 このプロパティは、入力と出力の両方のテーブルに適用されます。</p><p>EscapeChar と quoteChar のテーブルの両方を指定できないことに注意してください。</p> | いいえ |
| nullValue | <p>BLOB ファイル コンテンツで null 値を表すために使用する文字です。 このタグは任意です。 既定値は、"\N"です。</p><p>たとえば、上記のサンプルに基づき、blob 内の"NaN"として変換されます SQL Server などへのコピー中に null 値。</p> | いいえ |
| encodingName | エンコード名の指定。 有効なエンコード名の一覧を参照してください: [Encoding.EncodingName プロパティ](https://msdn.microsoft.com/library/system.text.encoding.aspx)します。 例: windows-1250 または shift_jis。 既定値は UTF-8 です。 | いいえ | 

#### サンプル
次の例は、TextFormat の format プロパティの一部を示します。

    "typeProperties":
    {
        "folderPath": "mycontainer/myfolder",
        "fileName": "myblobname"
        "format":
        {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": ";",
            "quoteChar": "\"",
            "NullValue": "NaN"
        }
    },

quoteChar ではなく escapeChar を使用するには、quoteChar の行を次で置き換えます。

    "escapeChar": "$",

### AvroFormat の指定
形式が AvroFormat に設定されている場合、typeProperties セクション内の Format セクションにプロパティを指定する必要はありません。 例:

    "format":
    {
        "type": "AvroFormat",
    }

Hive テーブルで Avro 形式を使用するを参照することができます [Apache Hive のチュートリアル](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe)します。

[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]


## Azure BLOB のコピー アクティビティの type プロパティ  
セクションとアクティビティの定義に使用できるプロパティの一覧については、次を参照してください。、 [パイプラインの作成](data-factory-create-pipelines.md) 記事です。 名前、説明、入力テーブル、出力テーブル、さまざまなポリシーなどのプロパティがあらゆる種類のアクティビティで利用できます。

一方で、アクティビティの typeProperties セクションで利用できるプロパティはアクティビティの種類により異なり、コピー アクティビティの場合、sources と sinks の種類によって異なります。

**BlobSource** で次のプロパティをサポートしている、 **typeProperties** セクション。

| プロパティ | 説明 | 使用できる値 | 必須 |
| -------- | ----------- | -------------- | -------- | 
| treatEmptyAsNull | null と空の文字列を NULL 値として処理するかどうかを指定します。 | TRUE<br/>FALSE | いいえ |
| skipHeaderLineCount | スキップする必要がある行数を指定します。 可能な入力データセットを使用する場合にのみ **TextFormat**します。 | 0 から最大値までの整数。 | いいえ | 
| recursive | データをサブ フォルダーから再帰的に読み取るか、指定したフォルダーからのみ読み取るかを指定します。 | True (既定値)、False | いいえ | 


**BlobSink** 、次のプロパティをサポートしている **typeProperties** セクション。

| プロパティ | 説明 | 使用できる値 | 必須 |
| -------- | ----------- | -------------- | -------- |
| blobWriterAddHeader | 列定義のヘッダーを追加するかどうかを指定します。 | TRUE<br/>FALSE (既定) | いいえ |
| copyBehavior | ソースが BlobSource または FileSystem である場合のコピー動作を定義します。 | <p>CopyBehavior プロパティの 3 つの値があります。 </p><ul><li>**PreserveHierarchy:** 保持対象のフォルダー、つまり、ソース フォルダーに対するソース ファイルの相対パスのファイル階層はターゲット フォルダーに対するターゲット ファイルの相対パスと同じです</li><li>**。FlattenHierarchy:** ソース フォルダーからすべてのファイルは、ターゲット フォルダーの最初のレベルになります。 ターゲット ファイルは、自動生成された名前になります。 </li><li>**MergeFiles: (既定値)** ソース フォルダーからすべてのファイルを 1 つのファイルにマージします。 ファイル名/BLOB 名を指定した場合、マージされたファイルの名前は指定した名前になります。それ以外は自動生成されたファイル名になります。</li></ul> | いいえ |

### recursive と copyBehavior の例
このセクションでは、recursive 値と copyBhavior 値の組み合わせごとに、Copy 操作で行われる動作について説明します。 

recursive | copyBehavior | 行われる動作
--------- | ------------ | --------
true | preserveHierarchy | <p>次の構造のソース フォルダー Folder1:</p> <p>Folder1<br/>& nbsp; & nbsp; & nbsp; & nbsp;File1<br/>& nbsp; & nbsp; & nbsp; & nbsp;File2<br/>& nbsp; & nbsp; & nbsp; & nbsp;Subfolder1<br/>& nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; & nbsp;ファイル 3 と指定<br/>& nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; & nbsp;File4<br/>& nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; & nbsp;File5</p>ターゲット フォルダー Folder1 は、ソースと同じ構造になります<p>Folder1<br/>& nbsp; & nbsp; & nbsp; & nbsp;File1<br/>& nbsp; & nbsp; & nbsp; & nbsp;File2<br/>& nbsp; & nbsp; & nbsp; & nbsp;Subfolder1<br/>& nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; & nbsp;ファイル 3 と指定<br/>& nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; & nbsp;File4<br/>& nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; & nbsp;File5</p>.  
true | flattenHierarchy | <p>次の構造のソース フォルダー Folder1:</p> <p>Folder1<br/>& nbsp; & nbsp; & nbsp; & nbsp;File1<br/>& nbsp; & nbsp; & nbsp; & nbsp;File2<br/>& nbsp; & nbsp; & nbsp; & nbsp;Subfolder1<br/>& nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; & nbsp;ファイル 3 と指定<br/>& nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; & nbsp;File4<br/>& nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; & nbsp;File5</p>ターゲット Folder1 には、次の構造があります。 <p>Folder1<br/>& nbsp; & nbsp; (& a) nbsp; & nbsp; ファイル 1 の自動生成された名前<br/>& nbsp; & nbsp; & nbsp; & nbsp; File2 の名前を自動生成されました。<br/>& nbsp; & nbsp; & nbsp; & nbsp; ファイル 3 と指定の名前を自動生成されました。<br/>& nbsp; & nbsp; & nbsp; & nbsp; File4 の自動生成された名前<br/>& nbsp; & nbsp; & nbsp; & nbsp; File5 の自動生成された名前</p>
true | mergeFiles | <p>次の構造のソース フォルダー Folder1:</p> <p>Folder1<br/>& nbsp; & nbsp; & nbsp; & nbsp;File1<br/>& nbsp; & nbsp; & nbsp; & nbsp;File2<br/>& nbsp; & nbsp; & nbsp; & nbsp;Subfolder1<br/>& nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; & nbsp;ファイル 3 と指定<br/>& nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; & nbsp;File4<br/>& nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; & nbsp;File5</p>ターゲット Folder1 には、次の構造があります。 <p>Folder1<br/>& nbsp; & nbsp; & nbsp; & nbsp;File1 + File2 + ファイル 3 と指定 + File4 + 5 ファイルの内容が自動生成されたファイル名を持つ 1 つのファイルにマージします。</p>
false | preserveHierarchy | <p>次の構造のソース フォルダー Folder1:</p> <p>Folder1<br/>& nbsp; & nbsp; & nbsp; & nbsp;File1<br/>& nbsp; & nbsp; & nbsp; & nbsp;File2<br/>& nbsp; & nbsp; & nbsp; & nbsp;Subfolder1<br/>& nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; & nbsp;ファイル 3 と指定<br/>& nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; & nbsp;File4<br/>& nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; & nbsp;File5</p>ターゲット フォルダー Folder1 は、次の構造には<p>Folder1<br/>& nbsp; & nbsp; & nbsp; & nbsp;File1<br/>& nbsp; & nbsp; & nbsp; & nbsp;File2<br/></p><p>ファイル 3 と指定、File4、File5 と Subfolder1 が選択されていません。</p>.
false | flattenHierarchy | <p>次の構造のソース フォルダー Folder1:</p> <p>Folder1<br/>& nbsp; & nbsp; & nbsp; & nbsp;File1<br/>& nbsp; & nbsp; & nbsp; & nbsp;File2<br/>& nbsp; & nbsp; & nbsp; & nbsp;Subfolder1<br/>& nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; & nbsp;ファイル 3 と指定<br/>& nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; & nbsp;File4<br/>& nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; & nbsp;File5</p>ターゲット フォルダー Folder1 は、次の構造には<p>Folder1<br/>& nbsp; & nbsp; (& a) nbsp; & nbsp; ファイル 1 の自動生成された名前<br/>& nbsp; & nbsp; & nbsp; & nbsp; File2 の名前を自動生成されました。<br/></p><p>ファイル 3 と指定、File4、File5 と Subfolder1 が選択されていません。</p>.
false | mergeFiles | <p>次の構造のソース フォルダー Folder1:</p> <p>Folder1<br/>& nbsp; & nbsp; & nbsp; & nbsp;File1<br/>& nbsp; & nbsp; & nbsp; & nbsp;File2<br/>& nbsp; & nbsp; & nbsp; & nbsp;Subfolder1<br/>& nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; & nbsp;ファイル 3 と指定<br/>& nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; & nbsp;File4<br/>& nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; (& a) nbsp; & nbsp;File5</p>ターゲット フォルダー Folder1 は、次の構造には<p>Folder1<br/>& nbsp; & nbsp; & nbsp; & nbsp;File1 + File2 内容は、自動生成されたファイル名を持つ 1 つのファイルに結合されます。 ファイル 1 の自動生成された名前</p><p>ファイル 3 と指定、File4、File5 と Subfolder1 が選択されていません。</p>.

  


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

[AZURE.INCLUDE [data-factory-type-conversion-sample](../../includes/data-factory-type-conversion-sample.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]




