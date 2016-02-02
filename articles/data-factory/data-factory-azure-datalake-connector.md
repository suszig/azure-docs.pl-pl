<properties
    pageTitle="Azure Data Lake ストアからデータを移動 |Azure Data Factory"
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

この記事では、Azure data factory でコピー アクティビティを使用して Azure Data Lake ストアを別のデータからのデータとデータを Azure Data Lake ストアから別のデータ ストアに移動する方法について説明します。 この記事に基づき、 [データ移動アクティビティ](data-factory-data-movement-activities.md) コピー アクティビティとサポートされているデータ ストアの組み合わせでデータ移動の一般概要を説明する記事です。
> [AZURE.NOTE]
> Azure Data Lake Store との間でデータを移動するには、コピー アクティビティを含むパイプラインを作成する前に Azure Data Lake Store アカウントを作成する必要があります。 Azure Data Lake ストアの詳細については、次を参照してください [Azure Data Lake ストアを使ってみる](../data-lake-store/data-lake-store-get-started-portal.md)します。
>  
> 参照してください、 [、パイプラインの最初のチュートリアルを構築](data-factory-build-your-first-pipeline.md) データ ファクトリを作成する詳細な手順については、サービス、データセット、およびパイプラインをリンクします。 Data Factory エディター、Visual Studio、または Azure PowerShell と JSON のスニペットを使用して、Data Factory エンティティを作成できます。

## サンプル: Azure Blob から Azure Data Lake ストアにデータをコピーします。

下のサンプルで確認できる要素:

1.  型のリンクされたサービス [AzureStorage](#azure-storage-linked-service-properties)します。
2.  型のリンクされたサービス [AzureDataLakeStore](#azure-data-lake-linked-service-properties)します。
3.  An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](#azure-blob-dataset-type-properties).
4.  出力 [データセット](data-factory-create-datasets.md) 型の [AzureDataLakeStore](#azure-data-lake-dataset-type-properties)します。
4.  A [パイプライン](data-factory-create-pipelines.md) を使用するコピー アクティビティの [BlobSource](#azure-blob-copy-activity-type-properties) と [AzureDataLakeStoreSink](#azure-data-lake-copy-activity-type-properties)します。

このサンプルでは、Azure Data Lake ストアへの Azure Blob ストレージから 1 時間ごと、時系列に属するデータをコピーします。 これらのサンプルで使用される JSON プロパティの説明はサンプルに続くセクションにあります。


**Azure ストレージのリンクされたサービス:**

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
2. JSON エディターで、**datalakeUri** プロパティに Data Lake の URI を入力します。
3. コマンド バーの **[承認する]** をクリックします。 ポップアップ ウィンドウが表示されます。

    ![Authorize button](./media/data-factory-azure-data-lake-connector/authorize-button.png)
4. 資格情報を使用してサインインし、JSON の **authorization** プロパティに今すぐ値を割り当てる必要があります。
5. (省略可能) JSON の **accountName**、**subscriptionID**、**resourceGroupName** などの省略可能なパラメーターの値を指定するか、これらのプロパティを JSON から削除します。
6. コマンド バーの **[デプロイ]** をクリックして、リンク サービスをデプロイします。


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

## サンプル: Azure Data Lake ストアから Azure Blob にデータをコピーします。

下のサンプルで確認できる要素:

1.  型のリンクされたサービス [AzureDataLakeStore](#azure-data-lake-linked-service-properties)します。
2.  型のリンクされたサービス [AzureStorage](#azure-storage-linked-service-properties)します。
3.  入力 [データセット](data-factory-create-datasets.md) 型の [AzureDataLakeStore](#azure-data-lake-dataset-type-properties)します。
4.  An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](#azure-blob-dataset-type-properties).
5.  A [パイプライン](data-factory-create-pipelines.md) を使用するコピー アクティビティの [AzureDataLakeStoreSource](#azure-data-lake-copy-activity-type-properties) と [BlobSink](#azure-blob-copy-activity-type-properties)

このサンプルでは、Azure blob に、Azure Data Lake ストアから 1 時間ごと、時系列に属するデータをコピーします。 これらのサンプルで使用される JSON プロパティの説明はサンプルに続くセクションにあります。

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

> [AZURE.NOTE] 承認 URL を取得するには、前の例の手順を参照してください。  

**Azure ストレージのリンクされたサービス:**

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

## Azure Data Lake ストアのリンクされたサービス プロパティ

Azure Storage のリンクされたサービスを利用し、Azure ストレージ アカウントを Azure Data Factory にリンクできます。 次の表は、Azure Storage のリンクされたサービスに固有の JSON 要素の説明をまとめたものです。

| プロパティ| 説明| 必須|
| :-------- | :----------- | :-------- |
| type| Type プロパティに設定する必要があります: **AzureDataLakeStore**| はい|
| dataLakeUri| Azure Data Lake Store アカウントの情報を指定します。次の形式である: https://<Azure Data Lake account name>.azuredatalakestore.net/webhdfs/v1| はい|
| authorization| **Data Factory エディター**で **[承認する]** をクリックして資格情報を入力すると、自動生成された承認 URL がこのプロパティに割り当てられます。| はい|
| sessionId| OAuth 承認セッションの OAuth セッション ID。各セッション ID は一意であり、1 回のみ使用できます。Data Factory エディターを使用すると自動的に生成されます。| はい|
| accountName| Data Lake アカウント名| いいえ|
| subscriptionId| Azure サブスクリプション id。| いいえ (指定されていない場合は Data Factory のサブスクリプションが使用されます)。|
| resourceGroupName| Azure リソース グループ名| いいえ (指定されていない場合は Data Factory のリソース グループが使用されます)。|


## Azure Data Lake データセットの type プロパティ

JSON セクションとデータセットの定義に使用できるプロパティの一覧については、次を参照してください。、 [データセットを作成する](data-factory-create-datasets.md) 記事です。データセット JSON の構造、可用性、ポリシーなどのセクションはすべてのデータセット型 (Azure SQL、Azure BLOB、Azure テーブルなど) で同じです。

**typeProperties** セクションはデータセット型ごとに異なり、データ ストアのデータの場所や書式などに関する情報を提供します。 型のデータセットの typeProperties セクション **AzureDataLakeStore** dataset には、次のプロパティが含まれています。

| プロパティ| 説明| 必須|
| :-------- | :----------- | :-------- |
| folderPath| コンテナーと Azure Data Lake 内のフォルダーへのパスを格納します。| はい|
| fileName| <p>Azure Data Lake ストア内のファイルの名前です。ファイル名は省略できます。</p><p>ファイル名を指定する場合、アクティビティ (コピーを含む) は、特定のファイルで動作します。</p><p>とファイル名が指定されていないコピーには入力データセットの folderPath のすべてのファイルが含まれます</p>。<p>、生成されたファイルの名前を次にも見られる出力データセットに fileName が指定されていないときに次の形式: データ。<Guid>します。txt (例:: 付けられます-93ff-4c6f-b3be-f69616f1df7a.txt</p>| いいえ|
| partitionedBy| partitionedBy は任意のプロパティです。これを使用し、時系列データに動的な folderPath と fileName を指定できます。たとえば、1 時間ごとのデータに対して folderPath をパラメーター化できます。詳細と例については以下の活用する partitionedBy プロパティ セクションを参照してください。| いいえ|
| BlobSink の format| **TextFormat** と **AvroFormat** の 2 種類の形式がサポートされています。形式の下にある type プロパティをいずれかの値に設定する必要があります。形式が TextFormat のとき、形式に追加で任意のプロパティを指定できます。参照してください、 [TextFormat の指定](#specifying-textformat) 詳細については後述します。| いいえ|
| compression| データの圧縮の種類とレベルを指定します。サポートされる種類: GZip、Deflate、および BZip2。サポートされるレベル: Optimal および Fastest。参照してください [圧縮サポート](#compression-support) 詳細についてです。| いいえ|

### partitionedBy プロパティの活用

前述のように、時系列データに対して動的な folderPath と fileName を指定するときに、**partitionedBy** セクション、Data Factory マクロ、特定のデータ スライスの開始時刻と終了時刻を示すシステム変数の SliceStart と SliceEnd を使用できます。

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

書式が **TextFormat** に設定されている場合、次の**任意の**プロパティを **Format** セクションで指定できます。

| プロパティ| 説明| 必須|
| -------- | ----------- | -------- |
| columnDelimiter| ファイルの列の区切り記号として使用される文字です。このタグは任意です。既定値はコンマです (,)。| いいえ|
| rowDelimiter| ファイルの行の区切り記号として使用される文字です。このタグは任意です。既定値は [“\r\n”, “\r”,” \n”] のいずれかになります。| いいえ|
| escapeChar| <p>コンテンツに表示される列区切り記号のエスケープに使用される特殊文字です。このタグは任意です。既定値はありません。このプロパティの 2 つ以上の文字を指定する必要があります。</p><p>たとえば、列区切り記号としてコンマ (,) がテキストにもコンマ文字が必要 (例:「こんにちは, world」)、エスケープ文字として '$' を定義して文字列を使用して「hello$, world」ソースの</p>。<p>EscapeChar と quoteChar のテーブルの両方を指定できません。</p>| いいえ|
| quoteChar| <p>特殊文字は、文字列値を引用符に使用されます。引用符文字内の列区切り文字と行区切り文字は文字列値の一部として処理されます。このタグは任意です。既定値はありません。このプロパティの 2 つ以上の文字を指定する必要があります。</p><p>たとえば、列区切り記号としてコンマ (,) がテキストにもコンマ文字が必要 (例: <Hello, world>) を定義することができます '"' 引用符として文字し、文字列を使用して <"Hello, world"> ソースにします。このプロパティは、入力と出力の両方のテーブルに適用されます。</p><p>EscapeChar と quoteChar のテーブルの両方を指定できません。</p>| いいえ|
| nullValue| <p>Blob ファイル コンテンツで null 値を表すために使用する文字です。このタグは任意です。既定値は、"\N"です。</p><p>たとえば、上記のサンプルに基づき、blob 内の"NaN"として変換されます SQL Server などへのコピー中に null 値。</p>| いいえ|
| encodingName| エンコード名の指定。有効なエンコード名の一覧を参照してください: [Encoding.EncodingName プロパティ](https://msdn.microsoft.com/library/system.text.encoding.aspx)します。例: windows-1250 または shift_jis。既定値は UTF-8 です。| いいえ|

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

データセットの圧縮を指定するには、次の例のように、データセットの JSON で **compression** プロパティを使用します。

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

**compression** セクションには次の 2 つのプロパティがあります。

- **type:** 圧縮コーデックです。**GZIP**、**Deflate**、または **BZIP2** を指定できます。
- **level:** 圧縮率です。**Optimal** または **Fastest** を指定できます。
    - **Fastest:** 圧縮操作は可能な限り短時間で完了しますが、生成ファイルが最適に圧縮されない場合があります。
    - **Optimal**: 圧縮操作では最適に圧縮されますが、操作完了までの時間が増加する場合があります。

    参照してください [圧縮レベル](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) についても説明します。

上記のサンプル データセットは、コピー アクティビティの出力として使用すると、コピー アクティビティと GZIP コーデックが Azure Data Lake ストアに pagecounts.csv.gz という名前のファイルに最適な割合と、書き込み、圧縮されたデータを使用して出力データを圧縮します。

入力データセットの JSON で compression プロパティを指定すると、パイプラインでソースから圧縮データを読み取ることができ、出力データセットの JSON で compression プロパティを指定すると、コピー アクティビティにより出力先に圧縮データを書き込むことができます。 いくつかのサンプル シナリオを次に示します。

- Azure Data Lake ストアから読み取り GZIP で圧縮されたデータは、圧縮解除し、結果データを Azure SQL データベースに書き込みます。 ここでは圧縮 JSON プロパティで入力の Azure Data Lake ストア データセットを定義します。
- 内部設置型ファイル システムからプレーン テキスト ファイルからデータを読み取る、GZip 形式を使用して圧縮および圧縮されたデータを Azure Data Lake ストアに書き込みます。 ここでは圧縮 JSON プロパティと出力 Azure Data Lake データセットを定義します。
- Azure Data Lake ストアから GZIP で圧縮されたデータを読み取り、ファイルを展開、BZIP2 を使用して圧縮および結果データを Azure Data Lake ストアに書き込みます。 圧縮の種類が圧縮の種類が BZIP2 にここで設定 GZIP と出力データセットに設定には、Azure Data Lake ストアの入力データセットを定義します。


## Azure Data Lake コピー アクティビティの type プロパティ

セクションとアクティビティの定義に使用できるプロパティの一覧については、次を参照してください。、 [パイプラインの作成](data-factory-create-pipelines.md) 記事です。名前、説明、入力テーブル、出力テーブル、さまざまなポリシーなどのプロパティがあらゆる種類のアクティビティで利用できます。

一方で、アクティビティの typeProperties セクションで利用できるプロパティはアクティビティの種類により異なり、コピー アクティビティの場合、sources と sinks の種類によって異なります。

**AzureDataLakeStoreSource** 、次のプロパティをサポートしている **typeProperties** セクション。

| プロパティ| 説明| 使用できる値| 必須|
| -------- | ----------- | -------------- | -------- |
| recursive| データをサブ フォルダーから再帰的に読み取るか、指定したフォルダーからのみ読み取るかを指定します。| True (既定値)、False| いいえ|



**AzureDataLakeStoreSink** 、次のプロパティをサポートしている **typeProperties** セクション。

| プロパティ| 説明| 使用できる値| 必須|
| -------- | ----------- | -------------- | -------- |
| copyBehavior| コピー動作を指定します。| <p>**PreserveHierarchy:** 保持対象のフォルダー、つまり、ソース フォルダーに対するソース ファイルの相対パスのファイル階層はターゲット フォルダーに対するターゲット ファイルの相対パスと同じです</p>。<p>**FlattenHierarchy:** ソース フォルダーからすべてのファイルは、ターゲット フォルダーの最初のレベルになります。ターゲット ファイルは、自動生成された名前になります。</p><p>**MergeFiles:** (この機能は間もなく公開される予定) を 1 つのファイルにソース フォルダーからすべてのファイルをマージします。マージされたファイル名が指定した名前になりますファイル/blob の名前が指定されている場合自動生成されたファイル名がそれ以外の場合、でしょう。</p>| いいえ|


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

[AZURE.INCLUDE [data-factory-type-conversion-sample](../../includes/data-factory-type-conversion-sample.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]





