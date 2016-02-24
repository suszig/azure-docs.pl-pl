<properties 
    pageTitle="Data Factory を使用したスケジュール設定と実行" 
    description="Azure Data Factory アプリケーション モデルのスケジュール設定と実行の側面について説明します。" 
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
    ms.date="10/20/2015" 
    ms.author="spelluru"/>

# Data Factory を使用したスケジュール設定と実行
  
この記事では、Azure Data Factory アプリケーション モデルのスケジュール設定と実行の側面について説明します。 この記事に基づき [パイプラインの作成](data-factory-create-pipelines.md) と [データセットの作成](data-factory-create-datasets.md) 記事 data factory アプリケーション モデルの概念の基礎を理解しておくことを前提としています。 アクティビティ、パイプライン、リンクされたサービス、およびデータセット。

## スケジュール設定のアクティビティ

 **スケジューラ** セクションで、アクティビティ JSON では、アクティビティの定期的なスケジュールを指定できます。 たとえば、次のように毎時間実行されるアクティビティをスケジュールできます。

    "scheduler": {
        "frequency": "Hour",
        "interval": 1
    },  
    
![スケジューラの例](./media/data-factory-scheduling-and-execution/scheduler-example.png)

上のように、毎時のスケジュールを指定すると、一連のタンブリング ウィンドウに対応するアクティビティ実行が作成されます。 タンブリング ウィンドウとは、一連の固定サイズで重複しない連続する時間間隔です。
 
現在実行中のアクティビティ実行のウィンドウの時間間隔を使用してアクセスできます **WindowStart** と **WindowEnd** アクティビティ JSON のシステム変数です。 アクティビティ JSON と、アクティビティに関連付けられたスクリプトで、時系列データを表す入力、出力データセットからデータを選択するなど、さまざまな用途にこれらの変数を使用できます。

特定の時刻のオフセット位置にあるスケジュール設定を含むスケジューラの使用可能なさまざまなプロパティの詳細については、配置するには、モードを設定または終了時に、ウィンドウの間隔の開始時の処理を参照してください、 [パイプラインの作成](data-factory-create-pipelines.md) 記事です。

## 時系列のデータセットとデータ スライス

時系列データは、連続するシーケンスのデータ ポイントです。通常、一定の間隔で行われる連続的な測定値で構成されます。 時系列データの一般的な例として、センサー データ、アプリケーション テレメトリ データなどがあります。

Azure Data Factory を使用すると、アクティビティ実行で時系列データを一括処理できます。 通常、入力データを受信してから、出力データを生成する必要がある定期的なパターンがあります。 このパターンが指定することによってモデル化 **可用性** セクション、データセットに次のようにします。

    "availability": {
      "frequency": "Hour",
      "interval": 1
    },

使用およびアクティビティの実行によって生成されるデータの各単位は、データと呼ばれる **スライス**します。 次の図は、可用性セットが毎時の頻度に設定された入力時系列データセットと出力時系列データセットを含むアクティビティの例です。

![可用性スケジューラ](./media/data-factory-scheduling-and-execution/availability-scheduler.png)

上の図には、入力と出力のデータセットについて、毎時のデータ スライスがあります。 図では、処理の準備ができた 3 つの入力スライスがあり、10-11AM 出力スライスを生成する 10-11AM アクティビティ実行が実行中です。 

生成されている現在のスライスに関連付けられている時間間隔は、データセット JSON で変数にアクセスできる **SliceStart** と **SliceEnd**します。

詳細については、さまざまなプロパティの可用性のために使用可能なセクションを参照してください、 [データセットの作成](data-factory-create-datasets.md) 記事です。

## サンプル - Azure SQL のアクティビティ移動データを Azure BLOB にコピーする

みましょう処理一括に示すように、コピー アクティビティのサンプルを再表示して、 [パイプラインの作成](data-factory-create-pipelines.md) データをコピーする Azure SQL テーブルから Azure blob に 1 時間ごとの記事です。

**入力: Azure SQL データセット**

    {
        "name": "AzureSqlInput",
        "properties": {
            "published": false,
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }


注意してください、 **頻度** に設定されている **時間** と **間隔** に設定されている **1** で、 **可用性** セクションです。 

**出力: Azure BLOB データセット**
    
    {
        "name": "AzureBlobOutput",
        "properties": {
            "published": false,
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mypath/{Year}/{Month}/{Day}/{Hour}",
                "format": {
                    "type": "TextFormat"
                },
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
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


注意してください、 **頻度** に設定されている **時間** と **間隔** に設定されている **1** で、 **可用性** セクションです。



**アクティビティ: コピー アクティビティ**

    {
        "name": "SamplePipeline",
        "properties": {
            "description": "copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "name": "AzureSQLtoBlob",
                    "description": "copy activity", 
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource",
                            "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 100000,
                            "writeBatchTimeout": "00:05:00"
                        }
                    },
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
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    }
                }
            ],
            "start": "2015-01-01T08:00:00Z",
            "end": "2015-01-01T11:00:00Z"
        }
    }


上の例で、アクティビティ スケジュールとデータセットの availability セクションは毎時の頻度に設定されています。 利用する方法のサンプルで、 **WindowStart** と **WindowEnd** 変数を指定したアクティビティに関連するデータの選択は実行し、適切な動的な blob に送信 **folderPath** 毎時のフォルダーをクリックしてパラメーター化します。

午前 8 時から 11 時に 3 つのスライスが実行されると、サンプル Azure テーブルと BLOB のようになります。

Azure SQL 内のデータは、次のようにあるとします。

![サンプル入力](./media/data-factory-scheduling-and-execution/sample-input-data.png)

上のパラメーターをデプロイすると、次のように Azure BLOB が構成されます。

1.  Mypath/2015年/1/1/8/データをファイルします。<Guid>データを .txt 

        10002345,334,2,2015-01-01 08:24:00.3130000
        10002345,347,15,2015-01-01 08:24:00.6570000
        10991568,2,7,2015-01-01 08:56:34.5300000

    **注:** <Guid> は実際の guid に置き換えられます。 ファイル名の例: Data.bcde1348-7620-4f93-bb89-0eed3455890b.txt
2.  Mypath/2015年/1/1/9/データをファイルします。<Guid>データを .txt:

        10002345,334,1,2015-01-01 09:13:00.3900000
        24379245,569,23,2015-01-01 09:25:00.3130000
        16777799,21,115,2015-01-01 09:47:34.3130000
3.  Mypath/2015年/1/1/10/データをファイルします。<Guid>データのない .txt です。


## データ スライス、パイプラインと同時スライス実行のためのアクティブな期間

 [パイプラインの作成](data-factory-create-pipelines.md) 記事には、設定で指定されたパイプラインの有効期間の概念が導入されました、 **開始** と **エンド** パイプラインのプロパティです。
 
パイプラインのアクティブな期間の開始日は過去に設定できます。Data Factory では、過去のすべてのデータ スライスが自動的に計算され (バック フィル)、処理が開始されます。

バック フィルされたデータ スライスでは、並行実行するように構成できます。 それには同時実行のプロパティを設定して **ポリシー** アクティビティ JSON のようにのセクション、 [パイプラインの作成](data-factory-create-pipelines.md) 記事です。

## 再実行が失敗したデータ スライスと自動データ依存関係追跡

わかりやすい画面でスライスの実行を監視できます。 参照してください [監視およびパイプラインを管理する](data-factory-monitor-manage-pipelines.md) の詳細については、資料です。 

2 つのアクティビティがある次の例を使って説明します。 Activity1 では、出力としてスライスを含む時系列データセットが生成されます。この出力は、Activity2 で入力として使用され、最終的な出力の時系列データセットが生成されます。

![失敗したスライス](./media/data-factory-scheduling-and-execution/failed-slice.png)

<br/>

上の図最近使用した 3 つのスライスからがあったことの 9-10 AM のスライスを生成するエラーを示しています。 **Dataset2**します。 Data Factory では、時系列データセットの依存関係が自動的に追跡され、結果として、9-10 AM ダウンストリーム スライスのアクティビティ実行の開始が保留されます。


Data Factory の監視および管理ツールを使用すると、失敗したスライスの診断ログを詳細に調べ、問題の根本原因を簡単に見つけて修正できます。 問題を解決した後は、アクティビティ実行を簡単に開始して失敗したスライスを生成することもできます。 再実行を開始する方法の詳細については、データ スライスをご覧くださいに状態遷移を理解、 [監視と管理](data-factory-monitor-manage-pipelines.md) 記事です。

再実行が開始され、dataset2 の 9-10AM スライスの準備ができると、下図のように、最終データセットに対して 9-10AM 依存のスライスの実行が開始されます。

![失敗したスライスの再実行](./media/data-factory-scheduling-and-execution/rerun-failed-slice.png)

依存関係の指定と、複雑なチェーンのアクティビティとデータセットの依存関係の追跡の詳細については、以下のセクションを参照してください。

## 頻度が異なるデータセットのモデル化

前述のサンプルでは、入力および出力データセットとアクティビティ スケジュール ウィンドウの頻度は同じでした。 シナリオによっては、1 つまたは複数の入力の頻度とは異なる頻度で出力を生成できる必要があります。 Data Factory は、このようなシナリオのモデル化をサポートしています。

### サンプル 1: 毎時取得できる入力データの出力レポートを 1 日に 1 回生成する

入力が使用可能なセンサーから測定データの Azure Blob に 1 時間ごとの平均、max、min などの統計を含む日毎の集計レポートを生成するシナリオを検討してください.data factory を使って、その日の [Hive アクティビティ](data-factory-hive-activity.md)します。

Data Factory を使用してこの処理をモデル化する方法を次に示します。

**入力 Azure BLOB データセット:**

特定の日のフォルダーに毎時の入力ファイルが生成されます。 入力の availability は Hourly に設定されています (frequency: Hour、interval: 1)。

    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**出力 Azure BLOB データセット**

毎日、その日のフォルダーに 1 つの出力ファイルが生成されます。 出力の availability は Daily に設定されています (frequency: Day、interval: 1)。


    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**アクティビティ: パイプラインの Hive アクティビティ**

Hive スクリプトを使用してパラメーターとして適切な日時情報を受信する、 **WindowStart** 変数次のようにします。 Hive スクリプトは、この変数を使用して、その日の適切なフォルダーからデータを読み込み、集計を実行して出力を生成します。

        {  
            "name":"SamplePipeline",
            "properties":{  
            "start":"2015-01-01T08:00:00",
            "end":"2015-01-01T11:00:00",
            "description":"hive activity",
            "activities": [
                {
                    "name": "SampleHiveActivity",
                    "inputs": [
                        {
                            "name": "AzureBlobInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adftutorial\\hivequery.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Year": "$$Text.Format('{0:yyyy}',WindowsStart)",
                            "Month": "$$Text.Format('{0:%M}',WindowStart)",
                            "Day": "$$Text.Format('{0:%d}',WindowStart)"
                        }
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },          
                    "policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "OldestFirst",
                        "retry": 2,
                        "timeout": "01:00:00"
                    }
                 }
             ]
           }
        }

データ依存関係の観点から見ると、次のようになります。

![データの依存関係](./media/data-factory-scheduling-and-execution/data-dependency.png)

毎日の出力スライスは、入力データセットの 24 時間のスライスに依存しています。 Data Factory では、生成する出力スライスと同じ期間に属する入力データ スライスを特定して、これらの依存関係を自動的に計算します。 24 個の入力スライスのいずれかを使用できない場合 (たとえば、アップストリームのそのスライスを生成するアクティビティで偶然処理が発生した場合など)、Data Factory では入力スライスの準備が完了するまで待機してから、毎日のアクティビティ実行を開始します。


### サンプル 2: 式と Data Factory の関数を使用して依存関係を指定する

別のシナリオについて考えてみましょう。 2 つのデータセットを処理する Hive アクティビティがあるとします。1 つは毎日新しいデータを取得しますが、もう 1 つは毎週新しいデータを取得します。 この例では 2 つの入力を結合し、出力を毎日生成します。
 
出力データ スライスの期間に合わせて入力データ スライスを含めることで、処理対象の適切な入力スライスを Data Factory で自動的に特定する、というこれまでの簡単な方法は、この例では利用できません。

個々のアクティビティ実行で、毎週の入力データセットについて最新の週のデータ スライスが Data Factory に使用されるように指定する方法が必要です。 この処理には、次のように Azure Data Factory の関数を使用できます。

**Input1: Azure BLOB**

最初の入力が更新される Azure blob **毎日**します。
    
    {
      "name": "AzureBlobInputDaily",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Input2: Azure BLOB**

Input2 は更新される Azure blob **毎週**します。

    {
      "name": "AzureBlobInputWeekly",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 7
        }
      }
    }

**出力: Azure BLOB**

毎日、その日のフォルダーに 1 つの出力ファイルが生成されます。 出力の availability は Daily に設定されています (frequency: Day、interval: 1)。
    
    {
      "name": "AzureBlobOutputDaily",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**アクティビティ: パイプラインの Hive アクティビティ**

Hive アクティビティは 2 つの入力を使用して、出力スライスを毎日生成します。 次のように、毎週の入力に対する最新の週の入力スライスに応じて、毎日の出力スライスを指定できます。
    
    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2015-01-01T08:00:00",
        "end":"2015-01-01T11:00:00",
        "description":"hive activity",
        "activities": [
          {
            "name": "SampleHiveActivity",
            "inputs": [
              {
                "name": "AzureBlobInputDaily"
              },
              {
                "name": "AzureBlobInputWeekly",
                "startTime": "Date.AddDays(SliceStart,  -7 - Date.DayOfWeek(SliceStart))",
                "endTime": "Date.AddDays(SliceEnd,  -Date.DayOfWeek(SliceEnd))"  
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutputDaily"
              }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "type": "HDInsightHive",
            "typeProperties": {
              "scriptPath": "adftutorial\\hivequery.hql",
              "scriptLinkedService": "StorageLinkedService",
              "defines": {
                "Year": "$$Text.Format('{0:yyyy}',WindowsStart)",
                "Month": "$$Text.Format('{0:%M}',WindowStart)",
                "Day": "$$Text.Format('{0:%d}',WindowStart)"
              }
            },
            "scheduler": {
              "frequency": "Day",
              "interval": 1
            },          
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 2,  
              "timeout": "01:00:00"
            }
           } 
         ]
       }
    }


## 連鎖するアクティビティ
2 つのアクティビティを連鎖させるには、一方のアクティビティの出力データセットを、もう一方のアクティビティの入力データセットとして指定します。 このとき、同じパイプラインのアクティビティと、別のパイプラインのアクティビティを指定できます。 2 つ目のアクティビティは、1 つ目のアクティビティが正常に完了した後にのみ実行されます。 この連鎖は、時間スライス レベルで発生します (データセット内の個別の単位)。   

## Data Factory のシステム変数

変数名 | 説明 | オブジェクトのスコープ | JSON のスコープと使用事例
------------- | ----------- | ------------ | ------------------------
WindowStart | 現在のアクティビティ実行ウィンドウの時間間隔の開始 | アクティビティ | <ol><li>データ選択クエリを指定します。 参照されているコネクタの記事を参照してください、 [データ移動アクティビティ](data-factory-data-movement-activities.md) 記事</li><li>。パラメーターを Hive スクリプト (上記のサンプル) に渡します。</li>
WindowEnd | 現在のアクティビティ実行ウィンドウの時間間隔の終了 | アクティビティ | 上記と同じ
SliceStart | 生成されているデータ スライスの時間間隔の開始 | アクティビティ<br/>データセット | <ol><li>動的フォルダー パスを指定し、ファイル名を使用した作業 [Azure Blob](data-factory-azure-blob-connector.md) と [ファイル システム データセット](data-factory-onprem-file-system-connector.md).</li><li>アクティビティ入力コレクションで data factory 関数では、入力の依存関係を指定します。</li></ol>
SliceEnd | 生成されている現在のデータ スライスの時間間隔の終了 | アクティビティ<br/>データセット | 上記と同じ。 

> [AZURE.NOTE] 現在のデータ ファクトリでは、アクティビティで正確に指定したスケジュールが出力データセットの availability に指定されたスケジュールを一致させることが必要です。 つまり、WindowStart、WindowEnd、SliceStart、および SliceEnd は、常に同じ期間と 1 つの出力スライスにマップされます。
 
## Data Factory 関数リファレンス

上記のシステム変数と Data Factory の関数を次の用途で使用できます。

1.  データ選択クエリを指定する (によって参照されるコネクタの記事を参照してください、 [データ移動アクティビティ](data-factory-data-movement-activities.md) 記事です。

    Data factory 関数を呼び出す構文は、: **$$<function>** データ選択クエリと、アクティビティおよびデータセットの他のプロパティです。  
2. アクティビティ入力コレクションで、Data Factory 関数を使用して入力の依存関係を指定する (上記のサンプルを参照してください)。

    入力の依存関係式を指定する場合、$$ は不要です。   

次の例で **sqlReaderQuery** によって返される値に JSON ファイルのプロパティが割り当てられている、 **Text.Format** 関数です。 このサンプルでは、というシステム変数も使用して **WindowStart**, 、アクティビティ実行ウィンドウの開始時刻を表します。
    
    {
        "Type": "SqlSource",
        "sqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
    }

### 関数

次の表は、Azure Data Factory の全関数の一覧です。

カテゴリ | 関数 | パラメーター | 説明
-------- | -------- | ---------- | ----------- 
Time | AddHours(X,Y) | X: DateTime <p>Y: int</p> | 指定した時間 X に Y 時間を追加します。 <p>例: 9/5/2013年 12時 00分: 00 PM + 2 時間 = 9/5/2013年 2時 00分: 00 PM</p>
Time | AddMinutes(X,Y) | X: DateTime <p>Y: int</p> | X に Y 分を追加します。<p>例: 9/15/2013年 12時 00分: 00 PM + 15 分 = 9/15/2013年 12時 15分: 00 PM</p>
Time | StartOfHour(X) | X: Datetime | X の時間コンポーネントで表される時間の開始時刻を取得します。 <p>例: StartOfHour 9/15/2013年 05時 10分: 23 PM が 9/15/2013年 05時 00分: 00 PM</p>
Date | AddDays(X,Y) | X: DateTime<p>Y: int</p> | X に Y 日を追加します。<p>例: 9/15/2013年 12時 00分: 00 PM + 2 日 = 9/17/2013年 12時 00分: 00 PM</p>
Date | AddMonths(X,Y) | X: DateTime<p>Y: int</p> | X に Y か月を追加します。<p>例: 9/15/2013年 12時 00分: 00 PM + 1 月 = 10/15/2013年 12時 00分: 00 PM</p> 
Date | AddQuarters(X,Y) | X: DateTime <p>Y: int</p> | 追加 Y * 3 か月を X。<p>例: 9/15/2013年 12時 00分: 00 PM + 1 四半期 = 12/15/2013年 12時 00分: 00 PM</p>
Date | AddWeeks(X,Y) | X: DateTime<p>Y: int</p> | 追加 Y * 7 日を X に<p>例: 9/15/2013年 12時 00分: 00 PM + 1 週 = 9/22/2013年 12時 00分: 00 PM</p>
Date | AddYears(X,Y) | X: DateTime<p>Y: int</p> | X に Y 年を追加します。<p>例: 9/15/2013年 12時 00分: 00 PM + 1 年 = 9/15/2014年 12時 00分: 00 PM</p>
Date | Day(X) | X: DateTime | X の日の部分を取得します。<p>例: 日 9/15/2013年 12時 00分: 00 PM は 9 です。 </p>
Date | DayOfWeek(X) | X: DateTime | X の曜日コンポーネントを取得します。<p>例: DayOfWeek 9/15/2013年 12時 00分: 00 PM は日曜日です。</p>
Date | DayOfYear(X) | X: DateTime | X の年コンポーネントで表される年間積算日を取得します。<p>次に例を示します。<br/>2015 年 12 月 1 日: 2015 年の 335 日目<br/>12/31/2015年: 2015 年の 365 日目<br/>12/31/2016年: 2016 (うるう年) の 366 日目</p>
Date | DaysInMonth(X) | X: DateTime | パラメーター X の月コンポーネントで表される月の日付を取得します。<p>例: 2013 年 9 月 15 日の DaysInMonth は 30 から 9 月である 30 日間です。</p>
Date | EndOfDay(X) | X: DateTime | X の日 (日コンポーネント) の側を表す日時を取得します。<p>例: 9/15/2013 05時 10分: 23 PM は、2013 年 9 月 15 日の 2013 年 9 月 15 日 11時 59分: 59 PM。</p>
Date | EndOfMonth(X) | X: DateTime | パラメーター X の月コンポーネントで表される月の終わりを取得します。 <p>例: EndOfMonth 9/15/2013年 05時 10分: 23 PM が 2013 年 9 月 30 日 11時 59分: 59 PM (9 月末を表す日付時刻)</p>
Date | StartOfDay(X) | X: DateTime | パラメーター X の日コンポーネントで表される日の開始を取得します。<p>例: StartOfDay 9/15/2013年 05時 10分: 23 PM が 9/15/2013年 12時 00分: 00 AM です。</p>
DateTime | From(X) | X: String | 文字列 X を日時にパースします。
DateTime | Ticks(X) | X: DateTime | パラメーター X の ticks プロパティを取得します。1 ティックは 100 ナノ秒です。 このプロパティの値は、0001 年 1 月 1 日深夜 12:00:00 以降の経過時間のティック数を表しています。 
Text | Format(X) | X: String 変数 | テキストの書式を設定します。

#### Text.Format の例

    "defines": { 
        "Year" : "$$Text.Format('{0:yyyy}',WindowStart)",
        "Month" : "$$Text.Format('{0:MM}',WindowStart)",
        "Day" : "$$Text.Format('{0:dd}',WindowStart)",
        "Hour" : "$$Text.Format('{0:hh}',WindowStart)"
    }

> [AZURE.NOTE] 別の関数内で関数を使用する場合を使用する必要はありません **$$** 内部関数のプリフィックス。 例: $$text.format('partitionkey ('PartitionKey eq \\'my_pkey_filter_value\\' および RowKey ge \\'{0:yyyy-MM-dd HH:mm:ss} \\ '、Time.addhours(slicestart,-6))。 この例でわかるように **$$** のプレフィックスは使用されません、 **Time.AddHours** 関数です。 
  

## データ依存関係の詳細情報

アクティビティ実行でデータセット スライスを生成するために、データ ファクトリは、次を使用して **依存モデル** アクティビティによって使用されるデータセットとアクティビティで生成されるデータセット間の関係を判断します。

出力データセット スライスを生成するために必要な入力データセットの時間範囲と呼ばれる、 **依存関係期間**します。

依存関係期間内の入力データセットのデータセット スライスを使用できるようになった後にのみ、アクティビティ実行でデータセット スライスが生成されます。 つまりすべての入力スライスを依存関係期間を構成する必要があります **準備** アクティビティ実行で生成する出力データセット スライスのステータス。 

データセット スライス [start、end] を生成するには、データセット スライスを依存関係期間にマップする関数が必要です。 この関数は、本質的には、データセット スライスの開始と終了を依存関係期間の開始と終了に変換する式です。 より形式的には次のようになります。 
    
    DatasetSlice = [start, end]
    DependecyPeriod = [f(start, end), g(start, end)]

この f と g は、各アクティビティの依存関係期間の開始と終了を計算するマップ関数です。

上のサンプルに示すように、ほとんどの場合、依存関係期間は、生成するデータ スライスの期間と同じです。 このような場合、Data Factory によって、依存関係期間に属する入力スライスが自動的に計算されます。  

例: 上の集計例では、出力が毎日生成され、入力データは毎時取得できます。データ スライス期間は 24 時間です。 Data Factory によって、この期間に関連する毎時の入力スライスが検出され、入力スライスに応じて出力スライスが作成されます。

また、上記のサンプルのように、入力の 1 つは毎週、出力スライスは毎日生成されるという独自のマップを依存関係期間に指定することもできます。
   
## データの依存関係と検証

必要に応じて、スライス実行を使用する前に、スライス実行で生成されるデータを検証する方法を指定した検証ポリシーをデータセットに定義することができます。 参照してください [データセットを作成する](data-factory-create-datasets.md) の詳細については、資料です。 

このような場合、スライスの実行が完了したら、出力スライスの状態が変更に **待機している** の副状態と **検証**します。 スライスの検証、スライスの状態に変わります **準備**します。
   
データ スライスが生成され、検証に合格しなかった場合、検証に失敗したスライスに依存するダウンストリーム スライスのアクティビティ実行は処理されません。

Data factory のデータ スライスのさまざまな状態は、「、 [モニターしパイプラインを管理する](data-factory-monitor-manage-pipelines.md) 記事です。

## 外部データ

データセットを external とマークすると (次の JSON を参照してください)、Azure Data Factory で生成されていないことを示します。 この場合、データセット ポリシーに、データセットの検証と再試行ポリシーを記述するパラメーター セットを追加できます。  参照してください [パイプラインの作成](data-factory-create-pipelines.md) すべてのプロパティの詳細についてです。 

外部データのデータ スライスが依存するスライスを処理する前に、準備が完了する必要があるデータ ファクトリによって生成されるデータセットに似ています。

    {
        "name": "AzureSqlInput",
        "properties": 
        {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": 
            {
                "tableName": "MyTable"  
            },
            "availability": 
            {
                "frequency": "Hour",
                "interval": 1     
            },
            "external": true,
            "policy": 
            {
                "externalData": 
                {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }  
        } 
    } 






  









 
 












      

  





