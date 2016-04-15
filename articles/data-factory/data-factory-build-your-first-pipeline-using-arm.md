<properties
    pageTitle="Azure PowerShell を使用した初めての Azure Data Factory パイプラインの作成"
    description="このチュートリアルでは、Azure PowerShell を使用して、サンプルの Azure Data Factory パイプラインを作成します。"
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
    ms.topic="hero-article"
    ms.date="12/15/2015"
    ms.author="spelluru"/>

# Azure PowerShell を使用した初めての Azure Data Factory パイプラインの作成
> [AZURE.SELECTOR]
- [チュートリアルの概要](data-factory-build-your-first-pipeline.md)
- [Data Factory エディターの使用](data-factory-build-your-first-pipeline-using-editor.md)
- [PowerShell の使用](data-factory-build-your-first-pipeline-using-powershell.md)
- [Visual Studio の使用](data-factory-build-your-first-pipeline-using-vs.md)
- [Resource Manager テンプレートの使用](data-factory-build-your-first-pipeline-using-arm.md)


この記事では、Azure リソース マネージャー テンプレートを使用して最初のパイプラインを作成する方法を学習します。 このチュートリアルの手順は次のとおりです。

1.  データ ファクトリを作成する。
2.  リンクされたサービス (データ ストア、計算) とデータセットを作成する。
3.  パイプラインを作成する。

 

## 前提条件
チュートリアルの「概要」トピックに記載されている前提条件とは別に、次をインストールする必要があります。

- **Azure PowerShell をインストール**します。 指示に従って [をインストールして、Azure PowerShell を構成する方法](../powershell-install-configure.md) 記事をお使いのコンピューターに Azure PowerShell の最新バージョンをインストールします。
- この記事では、Azure Data Factory サービスの概念については説明しません。 サービスの詳細については、目を通して [Azure Data Factory の概要](data-factory-introduction.md)します。 
- 参照してください [Azure リソース マネージャー テンプレートの作成](../resource-group-authoring-templates.md) Azure リソース マネージャー (ARM) テンプレートについて学習します。 
 

## 手順 1: ARM テンプレートを作成する

という名前の JSON ファイルを作成する **ADFTutorialARM.json** で **C:\ADFGetStarted** フォルダー以下の内容。 

> [AZURE.IMPORTANT] 値を変更 **storageAccountName** と **storageAccountKey** 変数です。 変更、 **dataFactoryName** すぎる名前は一意である必要があるためです。 

    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
        },
        "variables": {
            "dataFactoryName":  "TutorialDataFactoryARM",
            "storageAccountName":  "<stroage account name>" ,
            "storageAccountKey":  "<storage account key>",
            "apiVersion": "2015-10-01",
            "storageLinkedServiceName": "StorageLinkedService",
            "hdInsightOnDemandLinkedServiceName": "HDInsightOnDemandLinkedService",
            "blobOutputDataset": "AzureBlobOutput",
            "singleQuote": "'"
        },
        "resources": [
            {
                "name": "[variables('dataFactoryName')]",
                "apiVersion": "[variables('apiVersion')]",
                "type": "Microsoft.DataFactory/datafactories",
                "location": "westus",
                "resources": [
                    {
                        "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]" ],
                        "type": "linkedservices",
                        "name": "[variables('storageLinkedServiceName')]",
                        "apiVersion": "[variables('apiVersion')]",
                        "properties": {
                            "type": "AzureStorage",
                            "typeProperties": {
                                "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',variables('storageAccountName'),';AccountKey=',variables('storageAccountKey'))]"
                            }
                        }
                    },
                    {
                        "dependsOn": [
                            "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]",
                            "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'), '/linkedservices/', variables('storageLinkedServiceName'))]"
                        ],
                        "type": "linkedservices",
                        "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
                        "apiVersion": "[variables('apiVersion')]",
                        "properties": {
                            "type": "HDInsightOnDemand",
                            "typeProperties": {
                                "version": "3.2",
                                "clusterSize": 1,
                                "timeToLive": "00:30:00",
                                "linkedServiceName": "StorageLinkedService"
                            }
                        }
                    },
                    {
                        "dependsOn": [
                            "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]",
                            "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]"
                        ],
                        "type": "datasets",
                        "name": "[variables('blobOutputDataset')]",
                        "apiVersion": "[variables('apiVersion')]",
                          "properties": {
                            "type": "AzureBlob",
                            "linkedServiceName": "StorageLinkedService",
                            "typeProperties": {
                              "folderPath": "data/partitioneddata",
                              "format": {
                                "type": "TextFormat",
                                "columnDelimiter": ","
                              }
                            },
                            "availability": {
                              "frequency": "Month",
                              "interval": 1
                            }
                          }
                        },
                        {
                            "dependsOn": [
                                "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]",
                                "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]",
                                "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'), '/linkedServices/', variables('hdInsightOnDemandLinkedServiceName'))]",
                                "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'), '/datasets/', variables('blobOutputDataset'))]"
                            ],
                            "type": "datapipelines",
                            "name": "[variables('dataFactoryName')]",
                            "apiVersion": "[variables('apiVersion')]",
                            "properties": {
                                "description": "My first Azure Data Factory pipeline using ARM",
                                "activities": [
                                  {
                                    "type": "HDInsightHive",
                                    "typeProperties": {
                                      "scriptPath": "script/partitionweblogs.hql",
                                      "scriptLinkedService": "StorageLinkedService",
                                      "defines": {
                                        "partitionedtable": "[concat('wasb://data@', variables('storageAccountName'), '.blob.core.windows.net/partitioneddata')]"                                     }
                                    },
                                    "outputs": [
                                      {
                                        "name": "[variables('blobOutputDataset')]"
                                      }
                                    ],
                                    "scheduler": {
                                        "frequency": "Month",
                                        "interval": 1
                                    },
                                    "policy": {
                                      "concurrency": 1,
                                      "retry": 3
                                    },
                                    "name": "RunSampleHiveActivity",
                                    "linkedServiceName": "HDInsightOnDemandLinkedService"
                                  }
                                ],
                                "start": "2014-01-01",
                                "end": "2014-01-02"
                            }
                        }
                ]
            }
        ]
    }


## 手順 2: ARM テンプレートを使用して Data Factory エンティティをデプロイする

1. Azure PowerShell を起動し、次のコマンドを実行します。 Azure PowerShell は、このチュートリアルが終わるまで開いたままにしておいてください。 Azure PowerShell を閉じて再度開いた場合は、これらのコマンドをもう一度実行する必要があります。
    - 実行 **ログイン AzureRmAccount** ユーザー名と、Azure ポータルにサインインするために使用するパスワードを入力します。  
    - 実行 **Get-azuresubscription** このアカウントのすべてのサブスクリプションを表示します。
    - 実行 **Select-azuresubscription SubscriptionName** を使用するサブスクリプションを選択します。 このサブスクリプションは、Azure ポータルで使用したものと同じである必要があります。
1. 次のコマンドを実行し、手順 1. で作成した ARM テンプレートを使用して Data Factory エンティティをデプロイします。 

        New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFTutorialARM.json

## パイプラインの監視
 
1.  ログインした後、 [Azure ポータル](http://portal.azure.com/), 、] をクリックして **参照** 選択 **データ ファクトリ**します。
        ![[参照] の [データ ファクト]](./media/data-factory-build-your-first-pipeline-using-arm/BrowseDataFactories.png)
2.   **データ ファクトリ** ブレードで、[データ ファクトリ] をクリックして (**TutorialFactoryARM**) を作成します。   
2.   **Data Factory** 、データ ファクトリ] ブレードをクリックして **ダイアグラム**します。
        ![[ダイアグラム] タイル](./media/data-factory-build-your-first-pipeline-using-arm/DiagramTile.png)
4.   **ダイアグラム ビュー**, 、パイプラインの概要が表示されます、およびデータセットでは使用このチュートリアルです。
    
    ![Diagram View](./media/data-factory-build-your-first-pipeline-using-arm/DiagramView.png) 
8. ダイアグラム ビューで、データセットをダブルクリック **AzureBlobOutput**します。 現在処理中のスライスが表示されます。

    ![Dataset](./media/data-factory-build-your-first-pipeline-using-arm/AzureBlobOutput.png)
9. 処理を完了すると、スライスが表示されます **準備** 状態です。 オンデマンド HDInsight クラスターの作成には通常しばらく時間がかかることに注意してください。 

    ![Dataset](./media/data-factory-build-your-first-pipeline-using-arm/SliceReady.png) 
10. スライスが **準備** チェックの状態、 **partitioneddata** 内のフォルダー、 **データ** 出力データの blob ストレージ内のコンテナーです。  
 

