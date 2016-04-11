<properties 
    pageTitle="Azure Data Factory から Azure Data Lake Analytics で U-SQL スクリプトを実行する" 
    description="Azure Data Lake Analytics コンピューティング サービスで U-SQL スクリプトを実行してデータを処理する方法について説明します。" 
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
    ms.date="10/27/2015" 
    ms.author="spelluru"/>

# Azure Data Factory から Azure Data Lake Analytics で U-SQL スクリプトを実行する 
Azure Data Factory のパイプラインは、リンクされたコンピューティング サービスを使用して、リンクされたストレージ サービス内のデータを処理します。 パイプラインは、一連のアクティビティで構成されます。各アクティビティは、特定の処理操作を実行します。 この記事で説明、 **データ Lake 分析 U SQL アクティビティ** に実行する、  **U SQL** にスクリプト、 **Azure Data Lake 分析** リンクされたサービスを計算します。 

> [AZURE.NOTE] 
> Data Lake Analytics U-SQL アクティビティでパイプラインを作成する前に、Azure Data Lake Analytics アカウントを作成する必要があります。 Azure Data Lake 分析については、次を参照してください [Azure Data Lake 分析を使ってみる](../data-lake-analytics/data-lake-analytics-get-started-portal.md)します。
>  
> 参照してください、 [、パイプラインの最初のチュートリアルを構築](data-factory-build-your-first-pipeline.md) データ ファクトリを作成する詳細な手順については、サービス、データセット、およびパイプラインをリンクします。 Data Factory エディター、Visual Studio、または Azure PowerShell と JSON のスニペットを使用して、Data Factory エンティティを作成できます。

## Azure Data Lake Analytics リンク サービス
作成する、 **Azure Data Lake 分析** Azure Data Lake 分析をリンクするリンクされたサービスは、パイプラインの Data Lake 分析 U SQL アクティビティを使用する前に Azure data factory サービスを計算します。 

次の例は、Azure Data Lake Analytics リンク サービスの JSON 定義です。 

    {
        "name": "AzureDataLakeAnalyticsLinkedService",
        "properties": {
            "type": "AzureDataLakeAnalytics",
            "typeProperties": {
                "accountName": "adftestaccount",
                "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
                "authorization": "<authcode>",
                "sessionId": "<session ID>", 
                "subscriptionId": "<subscription id>",
                "resourceGroupName": "<resource group name>"
            }
        }
    }


次の表は、JSON 定義で使用されるプロパティの説明です。 

プロパティ | 説明 | 必須
-------- | ----------- | --------
型 | Type プロパティに設定する必要があります: **AzureDataLakeAnalytics**します。 | あり
accountName | Azure Data Lake Analytics アカウント名。 | あり
dataLakeAnalyticsUri | Azure Data Lake Analytics URI。 |  いいえ 
authorization | 認証コードがクリックすると自動的に取得される **Authorize** Data Factory エディターでのボタンをクリックし、OAuth のログインを完了します。 | あり 
subscriptionId | Azure サブスクリプション ID | いいえ (指定されていない場合は Data Factory のサブスクリプションが使用されます)。 
resourceGroupName | Azure リソース グループ名 |  いいえ (指定されていない場合は Data Factory のリソース グループが使用されます)。
sessionId | OAuth 承認セッションのセッション ID です。 各セッション ID は一意であり、1 回のみ使用できます。 セッション ID は、Data Factory Editor で自動生成されます。 | あり

   
 
## Data Lake Analytics U-SQL アクティビティ 

次の JSON のスニペットでは、Data Lake Analytics U-SQL アクティビティを使用してパイプラインを定義します。 このアクティビティ定義には、先ほど作成した Azure Data Lake Analytics リンク サービスへの参照が含まれています。   
  

    {
        "name": "ComputeEventsByRegionPipeline",
        "properties": {
            "description": "This is a pipeline to compute events for en-gb locale and date less than 2012/02/19.",
            "activities": 
            [
                {
                    "type": "DataLakeAnalyticsU-SQL",
                    "typeProperties": {
                        "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                        "scriptLinkedService": "StorageLinkedService",
                        "degreeOfParallelism": 3,
                        "priority": 100,
                        "parameters": {
                            "in": "/datalake/input/SearchLog.tsv",
                            "out": "/datalake/output/Result.tsv"
                        }
                    },
                    "inputs": [
                        {
                            "name": "DataLakeTable"
                        }
                    ],
                    "outputs": 
                    [
                        {
                            "name": "EventsByRegionTable"
                        }
                    ],
                    "policy": {
                        "timeout": "06:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "EventsByRegion",
                    "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
                }
            ],
            "start": "2015-08-08T00:00:00Z",
            "end": "2015-08-08T01:00:00Z",
            "isPaused": false
        }
    }


次の表は、このアクティビティに固有のプロパティの名前と説明です。 

プロパティ | 説明 | 必須
:-------- | :----------- | :--------
type | Type プロパティに設定する必要があります **DataLakeAnalyticsU SQL**します。 | あり
scriptPath | U-SQL スクリプトを含むフォルダーのパス。 | いいえ (スクリプトを使用する場合)
scriptLinkedService | Data Factory に対するスクリプトを含むストレージをリンクするリンク サービス | いいえ (スクリプトを使用する場合)
script (スクリプト) | scriptPath と scriptLinkedService を指定する代わりに、インライン スクリプトを指定します。 例: "script" : "CREATE DATABASE test". | いいえ (scriptPath と scriptLinkedService を使用する場合)
degreeOfParallelism | ジョブを実行するために同時に使用される最大ノード数。 | いいえ
priority | キューされているすべてのジョブのうち、先に実行するジョブを決定します。 数値が小さいほど、優先度は高くなります。 | いいえ 
parameters | U-SQL スクリプトのパラメーター | いいえ 


### 入力データセットと出力データセットの例

#### 入力データセット
この例では、入力データは Azure Data Lake Store 内 (datalake/input フォルダーの SearchLog.tsv ファイル) にあります。 

    {
        "name": "DataLakeTable",
        "properties": {
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
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }   

#### 出力データセット
この例では、U-SQL スクリプトで生成された出力データは、Azure Data Lake Store (datalake/output フォルダー) に格納されます。 

    {
        "name": "EventsByRegionTable",
        "properties": {
            "type": "AzureDataLakeStore",
            "linkedServiceName": "AzureDataLakeStoreLinkedService",
            "typeProperties": {
                "folderPath": "datalake/output/"
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

#### Azure Data Lake Store リンク サービスの例
上記の入力/出力データセットで使用される Azure Data Lake Store リンク サービスの定義例を次に示します。 

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

参照してください [Azure Data Lake ストアからデータを移動](data-factory-azure-datalake-connector.md) 上の Azure データ湖のストアに JSON プロパティの説明については、サービス、およびデータ セットの JSON スニペットをリンクします。 


