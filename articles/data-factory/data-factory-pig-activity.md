<properties 
    pageTitle="Pig アクティビティ" 
    description="Azure データ ファクトリで Pig アクティビティを使用して、オンデマンドまたは独自の HDInsight クラスターで Pig スクリプトを実行する方法について説明します。" 
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

# Pig アクティビティ

Data Factory での HDInsight Pig アクティビティ [パイプライン](data-factory-create-pipelines.md) で Pig クエリを実行 [独自](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) または [オンデマンド](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows または Linux ベースの HDInsight クラスター。 この記事に基づき、 [データ変換のアクティビティ](data-factory-data-transformation-activities.md) データ変換とサポートされる変換アクティビティの概要を説明する記事です。

## 構文

    {
        "name": "HiveActivitySamplePipeline",
        "properties": {
        "activities": [
            {
                "name": "Pig Activity",
                "description": "description",
                "type": "HDInsightPig",
                "inputs": [
                    {
                        "name": "input tables"
                    }
                ],
                "outputs": [
                    {
                        "name": "output tables"
                    }
                ],
                "linkedServiceName": "MyHDInsightLinkedService",
                "typeProperties": {
                    "script": "Pig script",
                    "scriptPath": "<pathtothePigscriptfileinAzureblobstorage>",
                    "defines": {
                        "param1": "param1Value"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                }
            }
        ]
      }
    }

## 構文の詳細

プロパティ | 説明 | 必須
-------- | ----------- | --------
name | アクティビティの名前 | はい
description | アクティビティの用途を説明するテキストです。 | いいえ
type | HDInsightPig | あり
inputs | Pig のアクティビティによって使用される入力 | いいえ
outputs | Pig のアクティビティによって生成される出力 | あり
linkedServiceName | Data Factory のリンクされたサービスとして登録されている HDInsight クラスターへの参照 | あり
script (スクリプト) | Pig スクリプトをインラインに指定します | いいえ
スクリプトのパス | Pig スクリプトを Azure BLOB ストレージに格納し、ファイルへのパスを指定します。 'script' プロパティまたは 'scriptPath' プロパティを使用します。 両方を同時に使用することはできません | いいえ
defines | Pig スクリプト内で参照するキーと値のペアとしてパラメーターを指定します | いいえ

## 例

ゲームのログ分析の例について考えてみましょう。ここでは、お客様の会社が発売したゲームをユーザーがプレイした時間を特定します。
 
以下はゲーム ログのサンプルです。コンマ (,) で区切られていて、ProfileID、SessionStart、Duration、SrcIPAddress、GameType の各フィールドが含まれています。

    1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
    1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
    1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
    1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
    .....

 **スクリプトを Pig** プロセスがこのデータは、次のようになります。

    PigSampleIn = LOAD 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/samplein/' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);
    
    GroupProfile = Group PigSampleIn all;
    
    PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);
    
    Store PigSampleOut into 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/sampleoutpig/' USING PigStorage (',');

Data Factory パイプラインでこの Pig スクリプトを実行するには、以下の手順を実行する必要があります。

1. 登録するリンクされたサービスを作成する [独自の HDInsight コンピューティング クラスター](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) または構成 [、オンデマンドの HDInsight コンピューティング クラスター](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service)します。 このリンクされたサービスを "HDInsightLinkedService" と呼ぶことにしましょう。
2.  作成、 [のリンクされたサービス](data-factory-azure-storage-connector.md) 、データをホストする Azure Blob ストレージへの接続を構成します。 このリンクされたサービスを "StorageLinkedService" と呼ぶことにしましょう。
3.  作成 [データセット](data-factory-create-datasets.md) 入力と出力データを指定します。 入力データセットは "PigSampleIn"、出力データセットは "PigSampleOut" と呼ぶことにしましょう。
4.  Azure Blob ストレージのファイルに Pig クエリのコピーは、上記の手順 2. で構成されます。 データをホストするためのリンクされたサービスがこのクエリ ファイルをホストしているものと異なる場合は、別の Azure ストレージのリンクされたサービスを作成し、アクティビティの構成のトークンを参照します。 使用して * * scriptPath * * pig スクリプト ファイルへのパスを指定し、 **scriptLinkedService** をスクリプト ファイルを含む Azure のストレージを指定します。 
    
    > [AZURE.NOTE] 使用して、アクティビティ定義で Pig スクリプトをインラインを提供できる、 **スクリプト** プロパティが、これは使用しないで、JSON ドキュメント内のスクリプトですべての特殊文字をエスケープする必要があるし、デバッグの問題の原因可能性があります。 手順 4. の使用をお勧めします。
5. HDInsightPig アクティビティで以下のパイプラインを作成し、データを処理します。

        {
          "name": "PigActivitySamplePipeline",
          "properties": {
            "activities": [
              {
                "name": "PigActivitySample",
                "type": "HDInsightPig",
                "inputs": [
                  {
                    "name": "PigSampleIn"
                  }
                ],
                "outputs": [
                  {
                    "name": "PigSampleOut"
                  }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                  "scriptPath": "adfwalkthrough\\scripts\\enrichlogs.pig",
                  "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                }
              }
            ]
          }
        } 
6. パイプラインをデプロイします。 参照してください [パイプラインを作成する](data-factory-create-pipelines.md) の詳細については、資料です。 
7. データ ファクトリの監視と管理のビューを使用して、パイプラインを監視します。 参照してください [監視し、Data Factory パイプラインを管理する](data-factory-monitor-manage-pipelines.md) の詳細については、資料です。

## defines 要素を使用する Pig スクリプトのパラメーターの指定

ゲームのログが Azure BLOB ストレージに毎日取り込まれ、日付と時刻で分割されたフォルダーに格納される例を考えてみましょう。 Pig スクリプトをパラメーター化し、実行時に入力フォルダーの場所を動的に渡し、さらに、日付と時刻で分割された出力も生成する必要があるとします。
 
パラメーター化された Pig スクリプトを使用するには、次の手順に従います。

- パラメーターを定義 **定義**します。

        {
            "name": "PigActivitySamplePipeline",
            "properties": {
            "activities": [
                {
                    "name": "PigActivitySample",
                    "type": "HDInsightPig",
                    "inputs": [
                        {
                            "name": "PigSampleIn"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "PigSampleOut"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "typeproperties": {
                        "scriptPath": "adfwalkthrough\\scripts\\samplepig.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Input": "$$Text.Format('wasb: //adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0: yyyy}/monthno={0: %M}/dayno={0: %d}/',SliceStart)",
                            "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)"
                        }
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    }
                }
            ]
          }
        }  
      
- Pig スクリプトを使用してパラメーターを参照してください '**$parameterName**' 次の例で示すようにします。

        PigSampleIn = LOAD '$Input' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);   
        GroupProfile = Group PigSampleIn all;       
        PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);      
        Store PigSampleOut into '$Output' USING PigStorage (','); 



