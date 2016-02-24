<properties
    pageTitle="Azure Data Lake Store の間でのデータの移動 | Azure Data Factory"
    description="Azure Data Factory を使用して Azure Data Lake Store に、または Azure Data Lake Store からデータを移動する方法を説明します。"
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
    ms.date="11/09/2015"
    ms.author="spelluru"/>

# Azure Data Factory を使用した Azure Data Lake Store との間でのデータの移動
この記事では、Azure Data Factory のコピー アクティビティを利用し、Azure Data Lake Store と別のデータ ストアの間でデータを移動する方法について説明します。 この記事に基づき、 [データ移動アクティビティ](data-factory-data-movement-activities.md) コピー アクティビティとサポートされているデータ ストアの組み合わせでデータ移動の一般概要を説明する記事です。

> [AZURE.NOTE]
> Azure Data Lake Store との間でデータを移動するには、コピー アクティビティを含むパイプラインを作成する前に Azure Data Lake Store アカウントを作成する必要があります。 Azure Data Lake ストアの詳細については、次を参照してください [Azure Data Lake ストアを使ってみる](../data-lake-store/data-lake-store-get-started-portal.md)します。
>  
> 参照してください、 [、パイプラインの最初のチュートリアルを構築](data-factory-build-your-first-pipeline.md) データ ファクトリを作成する詳細な手順については、サービス、データセット、およびパイプラインをリンクします。 Data Factory エディター、Visual Studio、または Azure PowerShell と JSON のスニペットを使用して、Data Factory エンティティを作成できます。

## サンプル: Azure BLOB から Azure Data Lake Store にデータをコピーする
下のサンプルで確認できる要素:

1.  型のリンクされたサービス [AzureStorage](#azure-storage-linked-service-properties)します。
2.  型のリンクされたサービス [AzureDataLakeStore](#azure-data-lake-linked-service-properties)します。
3.  入力 [データセット](data-factory-create-datasets.md) 型の [AzureBlob](#azure-blob-dataset-type-properties)します。
4.  出力 [データセット](data-factory-create-datasets.md) 型の [AzureDataLakeStore](#azure-data-lake-dataset-type-properties)します。
4.  A [パイプライン](data-factory-create-pipelines.md) を使用するコピー アクティビティの [BlobSource](#azure-blob-copy-activity-type-properties) と [AzureDataLakeStoreSink](#azure-data-lake-copy-activity-type-properties)します。

このサンプルでは、Azure Data Lake ストアへの Azure Blob ストレージから 1 時間ごと、時系列に属するデータをコピーします。 これらのサンプルで使用される JSON プロパティの説明はサンプルに続くセクションにあります。


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

**Azure Data Lake のリンクされたサービス:**

    {
        "name": "AzureDataLakeStoreLinkedService",
        "properties": {
            "type": "AzureDataLakeStore",
            "typeProperties": {
                "dataLakeUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
                "sessionId": "<session ID>",
                "authorization": "<authorization URL>"
            }
        }
    }

### Data Factory エディターを使用して Azure Data Lake のリンクされたサービスを作成するには
次の手順では、Data Factory エディターを使用して Azure Data Lake Store のリンクされたサービスを作成する手順について説明します。

1. クリックして **新しいデータ ストア** 選択し、コマンド バー **Azure Data Lake ストア**します。
2. JSON エディターでの **datalakeUri** プロパティには、データ湖の URI を入力します。
3. クリックして **Authorize** コマンド バーのボタンをクリックします。 ポップアップ ウィンドウが表示されます。

    ![Authorize button](./media/data-factory-azure-data-lake-connector/authorize-button.png)
4. サインイン用に、資格情報を使用して、 **承認** JSON のプロパティを今すぐ値に割り当てる必要があります。
5. (省略可能)省略可能なパラメーターの値を指定します。 **accountName**, 、**subscriptionID** と **resourceGroupName** 、JSON で (または) は、JSON からこれらのプロパティを削除します。
6. をクリックして **展開** 、コマンド バー、リンクされたサービスをデプロイします。


**Azure BLOB の入力データセット:**

データは新しい BLOB から 1 時間おきに取得されます (頻度: 時間、間隔: 1)。 BLOB のフォルダー パスとファイル名は、処理中のスライスの開始時間に基づき、動的に評価されます。 フォルダーのパスは、年、月、および日の開始時刻部分を使用し、ファイル名は開始時刻の時間部分を使用します。 "external":"true"」設定は、このテーブルがデータ ファクトリに対して外部にあり、data factory 内のアクティビティでは生成されないことを Data Factory サービスに通知します。

    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "Path": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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
                "format": "MM"
              }
            },
            {
              "name": "Day",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "dd"
              }
            },
            {
              "name": "Hour",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HH"
              }
            }
          ]
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


**Azure Data Lake 出力データセット:**

このサンプルは Azure Data Lake Store にデータをコピーします。 新しいデータが 1 時間おきに Data Lake Store にコピーされます。

    {
        "name": "AzureDataLakeStoreOutput",
        "properties": {
            "type": "AzureDataLakeStore",
            "linkedServiceName": " AzureDataLakeStoreLinkedService",
            "typeProperties": {
                "folderPath": "datalake/output/"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }



**コピー アクティビティのあるパイプライン:**

上記の入力データセットと出力データセットを使用するように構成され、1 時間おきに実行するようにスケジュールされているコピー アクティビティがパイプラインに含まれています。 パイプライン JSON 定義で、 **ソース** に設定されている型 **BlobSource** と **シンク** に設定されている型 **AzureDataLakeStoreSink**します。

    {  
        "name":"SamplePipeline",
        "properties":
        {  
            "start":"2014-06-01T18:00:00",
            "end":"2014-06-01T19:00:00",
            "description":"pipeline with copy activity",
            "activities":
            [  
                {
                    "name": "AzureBlobtoDataLake",
                    "description": "Copy Activity",
                    "type": "Copy",
                    "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                    ],
                    "outputs": [
                    {
                        "name": "AzureDataLakeStoreOutput"
                    }
                    ],
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource",
                            "treatEmptyAsNull": true,
                            "blobColumnSeparators": ","
                        },
                        "sink": {
                            "type": "AzureDataLakeStoreSink"
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

## サンプル: Azure Data Lake Store から Azure BLOB にデータをコピーする
下のサンプルで確認できる要素:

1.  型のリンクされたサービス [AzureDataLakeStore](#azure-data-lake-linked-service-properties)します。
2.  型のリンクされたサービス [AzureStorage](#azure-storage-linked-service-properties)します。
3.  入力 [データセット](data-factory-create-datasets.md) 型の [AzureDataLakeStore](#azure-data-lake-dataset-type-properties)します。
4.  出力 [データセット](data-factory-create-datasets.md) 型の [AzureBlob](#azure-blob-dataset-type-properties)します。
5.  A [パイプライン](data-factory-create-pipelines.md) を使用するコピー アクティビティの [AzureDataLakeStoreSource](#azure-data-lake-copy-activity-type-properties) と [BlobSink](#azure-blob-copy-activity-type-properties)

このサンプルはある時系列に属するデータを 1 時間おきに Azure Data Lake Store から Azure BLOB にコピーします。 これらのサンプルで使用される JSON プロパティの説明はサンプルに続くセクションにあります。

**Azure Data Lake Store のリンクされたサービス:**

    {
        "name": "AzureDataLakeStoreLinkedService",
        "properties": {
            "type": "AzureDataLakeStore",
            "typeProperties": {
                "dataLakeUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
                "sessionId": "<session ID>",
                "authorization": "<authorization URL>"
            }
        }
    }

> [AZURE.NOTE] 承認 URL を取得する前の例で手順を参照してください。  

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

**Azure Data Lake の入力データセット:**

設定 **"external": true** を指定して **externalData** ポリシーは、データ ファクトリに対して外部にあり、data factory のアクティビティでは生成されないテーブルであるを Azure Data Factory サービスに通知します。

    {
        "name": "AzureDataLakeStoreInput",
        "properties":
        {
            "type": "AzureDataLakeStore",
            "linkedServiceName": "AzureDataLakeStoreLinkedService",
            "typeProperties": {
                "folderPath": "datalake/input/",
                "fileName": "SearchLog.tsv",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
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

**Azure BLOB の出力データセット:**

データは新しい BLOB に 1 時間おきに書き込まれます (頻度: 時間、間隔: 1)。 BLOB のフォルダー パスは、処理中のスライスの開始時間に基づき、動的に評価されます。 フォルダー パスは開始時間の年、月、日、時刻の部分を使用します。

    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
                "format": "MM"
              }
            },
            {
              "name": "Day",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "dd"
              }
            },
            {
              "name": "Hour",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HH"
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

上記の入力データセットと出力データセットを使用するように構成され、1 時間おきに実行するようにスケジュールされているコピー アクティビティがパイプラインに含まれています。 パイプライン JSON 定義で、 **ソース** に設定されている型 **AzureDataLakeStoreSource** と **シンク** に設定されている型 **BlobSink**します。


    {  
        "name":"SamplePipeline",
        "properties":{  
            "start":"2014-06-01T18:00:00",
            "end":"2014-06-01T19:00:00",
            "description":"pipeline for copy activity",
            "activities":[  
                {
                    "name": "AzureDakeLaketoBlob",
                    "description": "copy activity",
                    "type": "Copy",
                    "inputs": [
                      {
                        "name": "AzureDataLakeStoreInput"
                      }
                    ],
                    "outputs": [
                      {
                        "name": "AzureBlobOutput"
                      }
                    ],
                    "typeProperties": {
                        "source": {
                            "type": "AzureDataLakeStoreSource",
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


## Azure Data Lake Store のリンクされたサービスのプロパティ

Azure Storage のリンクされたサービスを利用し、Azure Storage アカウントを Azure Data Factory にリンクできます。 次の表は、Azure Storage のリンクされたサービスに固有の JSON 要素の説明をまとめたものです。

| プロパティ | 説明 | 必須 |
| :-------- | :----------- | :-------- |
| type | Type プロパティに設定する必要があります: **AzureDataLakeStore** | あり |
| dataLakeUri | Azure Data Lake Store アカウントの情報を指定します。 次の形式である: https://<Azure Data Lake account name>.azuredatalakestore.net/webhdfs/v1 | あり |
| authorization | クリックして **Authorize** ボタンをクリックして、 **Data Factory エディター** し、資格情報を入力する承認を自動生成された URL をこのプロパティに割り当てられます。  | あり |
| sessionId | OAuth 承認セッションの OAuth セッション ID。 各セッション ID は一意であり、1 回のみ使用できます。 Data Factory エディターを使用すると自動的に生成されます。 | はい |  
| accountName | Data Lake アカウント名 | いいえ |
| subscriptionId | Azure サブスクリプション ID | いいえ (指定されていない場合は Data Factory のサブスクリプションが使用されます)。 |
| resourceGroupName |  Azure リソース グループ名 | いいえ (指定されていない場合は Data Factory のリソース グループが使用されます)。 |


## Azure Data Lake データセットの type プロパティ

JSON セクションとデータセットの定義に使用できるプロパティの一覧については、次を参照してください。、 [データセットを作成する](data-factory-create-datasets.md) 記事です。 データセット JSON の構造、可用性、ポリシーなどのセクションはすべてのデータセット型 (Azure SQL、Azure BLOB、Azure テーブルなど) で同じです。

 **TypeProperties** セクションはデータセット型ごとに異なるを場所などに関する情報を提供書式データ ストア内のデータなどです。 型のデータセットの typeProperties セクション **AzureDataLakeStore** dataset には、次のプロパティが含まれています。

| プロパティ | 説明 | 必須 |
| :-------- | :----------- | :-------- |
| folderPath | Azure Data Lake Store のコンテナーとフォルダーのパス。 | はい |
| fileName | <p>Azure Data Lake ストア内のファイルの名前です。 ファイル名は省略できます。 </p><p>ファイル名を指定する場合、アクティビティ (コピーを含む) は、特定のファイルで動作します。</p><p>ファイル名が指定されていないとき、コピーは入力データセットの folderPath 内のすべてのファイルを含めます。</p><p>生成されたファイルの名前を次にも見られる出力データセットに fileName が指定されていないときに次の形式: データ。<Guid>.txt (例:: 付けられます-93ff-4c6f-b3be-f69616f1df7a.txt</p> | いいえ |
| partitionedBy | partitionedBy は任意のプロパティです。 これを使用し、時系列データに動的な folderPath と fileName を指定できます。 たとえば、1 時間ごとのデータに対して folderPath をパラメーター化できます。 詳細と例については、「partitionedBy プロパティの活用」セクションを参照してください。 | いいえ |
| BlobSink の format | 2 種類の形式がサポートされている: **TextFormat**, 、**AvroFormat**します。 形式の下にある type プロパティをいずれかの値に設定する必要があります。 形式が TextFormat のとき、形式に追加で任意のプロパティを指定できます。 参照してください、 [TextFormat の指定](#specifying-textformat) 詳細については後述します。 | いいえ |
| compression | データの圧縮の種類とレベルを指定します。 サポートされる種類: GZip、Deflate、および BZip2。サポートされるレベル: Optimal および Fastest。 参照してください [圧縮サポート](#compression-support) 詳細についてです。  | いいえ |

### partitionedBy プロパティの活用
前述のように、動的な folderPath と filename を含む時系列データを指定することができます、 **partitionedBy** セクション、Data Factory マクロ、およびシステム環境変数: SliceStart と SliceEnd、指定されたデータ スライスの開始および終了時刻を指定します。

参照してください [データセットの作成](data-factory-create-datasets.md) と [スケジュールと実行](data-factory-scheduling-and-execution.md) 時系列データセット、スケジュール設定の詳細について理解する」の記事、スライスします。

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
        "fileName": "myfilename"
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


### 圧縮のサポート  
大量のデータセットを処理すると、I/O およびネットワークにボトルネックが生じる可能性があります。 そのため、データを圧縮して保存すると、ネットワークでのデータ転送速度が上昇してディスク領域を節約できるだけでなく、ビッグ データの処理性能を大幅に高めることができます。 現時点では、圧縮は Azure BLOB やオンプレミスのファイル システムなど、ファイルベースのデータ ストアでサポートされています。  

データセットの圧縮を指定するには、使用、 **圧縮** 次の例に示すように、データセット JSON のプロパティ。   

    {  
        "name": "AzureDatalakeStoreDataSet",  
        "properties": {  
            "availability": {  
                "frequency": "Day",  
                "interval": 1  
            },  
            "type": "AzureDatalakeStore",  
            "linkedServiceName": "DataLakeStoreLinkedService",  
            "typeProperties": {  
                "fileName": "pagecounts.csv.gz",  
                "folderPath": "compression/file/",  
                "compression": {  
                    "type": "GZip",  
                    "level": "Optimal"  
                }  
            }  
        }  
    }  
 
なお、 **圧縮** セクションには 2 つのプロパティ。  
  
- **型:** になる可能性が圧縮コーデック **GZIP**, 、**Deflate** または **BZIP2**します。  
- **レベル:** になる可能性が圧縮比率 **最適** または **Fastest**します。 
    - **最も高速な:** 、生成されたファイルが最適に圧縮されていない場合でも、圧縮操作をできるだけ早く完了する必要があります。 
    - **最適な**: 圧縮操作を最適に圧縮する、場合でも、操作を完了に時間がかかります。 
    
    参照してください [圧縮レベル](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) についても説明します。 

前述のサンプル データセットをコピー アクティビティの出力として使用し、コピー アクティビティで出力データを GZIP コーデックによって最適な圧縮率で圧縮してから、Azure Data Lake Store の pagecounts.csv.gz という名前のファイルに圧縮データを書き込む場合を考えます。   

入力データセットの JSON で compression プロパティを指定すると、パイプラインでソースから圧縮データを読み取ることができ、出力データセットの JSON で compression プロパティを指定すると、コピー アクティビティにより出力先に圧縮データを書き込むことができます。 いくつかのサンプル シナリオを次に示します。 

- Azure Data Lake Store から GZIP 圧縮データを読み取り、展開して、生成されたデータを Azure SQL Database に書き込みます。 この場合、Azure Data Lake Store 入力データセットを、compression JSON プロパティを使用して定義します。 
- オンプレミスのファイル システムのプレーンテキスト ファイルからデータを読み取り、GZip 形式で圧縮して、圧縮データを Azure Data Lake Store に書き込みます。 この場合、Azure Data Lake 出力データセットを、compression JSON プロパティを使用して定義します。  
- Azure Data Lake Store から GZIP 圧縮データを読み取って展開し、BZIP2 で圧縮して、生成されたデータを Azure Data Lake Store に書き込みます。 この場合、Azure Data Lake Store 入力データセットは圧縮タイプを GZIP に設定して定義し、Azure Data Lake Store 出力データセットは圧縮タイプを BZIP2 に設定して定義します。   


## Azure Data Lake のコピー アクティビティの type プロパティ  
セクションとアクティビティの定義に使用できるプロパティの一覧については、次を参照してください。、 [パイプラインの作成](data-factory-create-pipelines.md) 記事です。 名前、説明、入力テーブル、出力テーブル、さまざまなポリシーなどのプロパティがあらゆる種類のアクティビティで利用できます。

一方で、アクティビティの typeProperties セクションで利用できるプロパティはアクティビティの種類により異なり、コピー アクティビティの場合、sources と sinks の種類によって異なります。

**AzureDataLakeStoreSource** 、次のプロパティをサポートしている **typeProperties** セクション。

| プロパティ | 説明 | 使用できる値 | 必須 |
| -------- | ----------- | -------------- | -------- |
| recursive | データをサブ フォルダーから再帰的に読み取るか、指定したフォルダーからのみ読み取るかを指定します。 | True (既定値)、False | いいえ |



**AzureDataLakeStoreSink** 、次のプロパティをサポートしている **typeProperties** セクション。

| プロパティ | 説明 | 使用できる値 | 必須 |
| -------- | ----------- | -------------- | -------- |
| copyBehavior | コピー動作を指定します。 | <p>**PreserveHierarchy:** 保持対象のフォルダー、つまり、ソース フォルダーに対するソース ファイルの相対パスのファイル階層はターゲット フォルダーに対するターゲット ファイルの相対パスと同じです</p><p>**。FlattenHierarchy:** ソース フォルダーからすべてのファイルは、ターゲット フォルダーの最初のレベルになります。 ターゲット ファイルは自動的に生成された名前になります</p><p>**。MergeFiles:** (この機能は間もなく公開される予定) を 1 つのファイルにソース フォルダーからすべてのファイルをマージします。 マージされたファイル名が指定した名前になりますファイル/blob の名前が指定されている場合自動生成されたファイル名がそれ以外の場合、でしょう。</p> | いいえ |


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

[AZURE.INCLUDE [data-factory-type-conversion-sample](../../includes/data-factory-type-conversion-sample.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

