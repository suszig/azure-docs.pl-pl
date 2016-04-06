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
    ms.date="12/08/2015"
    ms.author="spelluru"/>

# Azure PowerShell を使用した初めての Azure Data Factory パイプラインの作成
> [AZURE.SELECTOR]
- [チュートリアルの概要](data-factory-build-your-first-pipeline.md)
- [Data Factory エディターの使用](data-factory-build-your-first-pipeline-using-editor.md)
- [PowerShell の使用](data-factory-build-your-first-pipeline-using-powershell.md)
- [Visual Studio の使用](data-factory-build-your-first-pipeline-using-vs.md)
- [Resource Manager テンプレートの使用](data-factory-build-your-first-pipeline-using-arm.md)


この記事では、Azure PowerShell を使用して最初のパイプラインを作成する方法を学習します。 このチュートリアルの手順は次のとおりです。

1.  データ ファクトリを作成する。
2.  リンクされたサービス (データ ストア、計算) とデータセットを作成する。
3.  パイプラインを作成する。

> [AZURE.IMPORTANT] 
> この記事では、Azure Data Factory サービスの概念については説明しません。 サービスの詳細については、次を参照してください。 [Azure Data Factory の概要](data-factory-introduction.md)します。 
> 
> この記事では、すべての Data Factory コマンドレットを取り上げているわけではありません。 参照してください [Data Factory コマンドレット リファレンス](https://msdn.microsoft.com/library/dn820234.aspx) Data Factory コマンドレットに関する包括的なドキュメントについてです。

## 前提条件
チュートリアルの「概要」トピックに記載されている前提条件とは別に、次をインストールする必要があります。

- **Azure PowerShell**します。 指示に従って [をインストールして、Azure PowerShell を構成する方法](../powershell-install-configure.md) 記事をお使いのコンピューターに Azure PowerShell の最新バージョンをインストールします。 

Azure PowerShell を使用している場合 **バージョン < 1.0**, 、記載されているコマンドレットを使用する必要があります [ここ][cmdlet-reference]します。 また、Data Factory コマンドレットを使用する前に、次のコマンドを実行する必要があります。 
 
1. Azure PowerShell を起動し、次のコマンドを実行します。 Azure PowerShell は、このチュートリアルが終わるまで開いたままにしておいてください。 Azure PowerShell を閉じて再度開いた場合は、これらのコマンドをもう一度実行する必要があります。
    1. 実行 **Add-azureaccount** ユーザー名と、Azure ポータルにサインインするために使用するパスワードを入力します。
    2. 実行 **Get-azuresubscription** このアカウントのすべてのサブスクリプションを表示します。
    3. 実行 **Select-azuresubscription** を使用するサブスクリプションを選択します。 このサブスクリプションは、Azure ポータルで使用したものと同じである必要があります。
4. Azure Data Factory コマンドレットはこのモードで利用可能な AzureResourceManager モードに切り替えます。 **Switch-azuremode AzureResourceManager**します。


## 手順 1: データ ファクトリを作成する

この手順では、Azure PowerShell を使用して、ADFTutorialDataFactoryPSH という名前の Azure Data Factory を作成します。

1. Azure PowerShell を起動し、次のコマンドを実行します。 Azure PowerShell は、このチュートリアルが終わるまで開いたままにしておいてください。 Azure PowerShell を閉じて再度開いた場合は、これらのコマンドをもう一度実行する必要があります。
    - 実行 **ログイン AzureRmAccount** ユーザー名と、Azure ポータルにサインインするために使用するパスワードを入力します。  
    - 実行 **Get-azuresubscription** このアカウントのすべてのサブスクリプションを表示します。
    - 実行 **Select-azuresubscription <Name of the subscription>** を使用するサブスクリプションを選択します。 このサブスクリプションは、Azure ポータルで使用したものと同じである必要があります。
3. という名前の Azure リソース グループを作成する **ADFTutorialResourceGroup** 次のコマンドを実行しています。

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    このチュートリアルの一部の手順は、ADFTutorialResourceGroup という名前のリソース グループを使用することを前提としています。 異なるリソース グループを使用する場合は、このチュートリアルで ADFTutorialResourceGroup の代わりにそのリソース グループを使用する必要があります。
4. 実行、 **新規 AzureRmDataFactory** コマンドレットを DataFactoryMyFirstPipelinePSH をという名前の data factory を作成します。  

        New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name DataFactoryMyFirstPipelinePSH –Location "West US"

    > [AZURE.IMPORTANT] Azure Data Factory の名前はグローバルに一意である必要があります。 エラーが発生する場合 **データ ファクトリ名"DataFactoryMyFirstPipelinePSH"は使用できません**, 、(yournameADFTutorialDataFactoryPSH など) の名前を変更します。 このチュートリアルの手順の実行中に、この名前を ADFTutorialFactoryPSH の代わりに使用します。 参照してください [Data Factory - 名前付け規則](data-factory-naming-rules.md) Data Factory アーティファクトの名前付け規則についてのトピックです。
    > 
    > データ ファクトリの名前は今後、DNS 名として登録される可能性があるため、一般ユーザーに表示される場合があります。

以降の手順では、このチュートリアルで使用するリンクされたサービス、データセット、およびパイプラインを作成する方法を説明します。

## 手順 2: リンクされたサービスとデータセットを作成する
この手順では、Azure ストレージ アカウントとオンデマンド Azure HDInsight クラスターをデータ ファクトリにリンクした後、Hive 処理からの出力データを表すデータセットを作成します。

### Azure Storage のリンクされたサービスを作成する
1.  次の内容で C:\ADFGetStartedPSH フォルダーに「StorageLinkedService.json」という名前の JSON ファイルを作成します。 ADFGetStartedPSH フォルダーがない場合は作成します。

        {
            "name": "StorageLinkedService",
            "properties": {
                "type": "AzureStorage",
                "description": "",
                "typeProperties": {
                    "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
                }
            }
        }

    置換 **アカウント名** を Azure ストレージ アカウントの名前と **アカウント キー** Azure ストレージ アカウントのアクセス キーを持つ。 ストレージ アクセス キーを取得する方法については、次を参照してください。 [表示、コピーおよび再生成するストレージ アクセス キー](http://azure.microsoft.com/documentation/articles/storage-create-storage-account/#view-copy-and-regenerate-storage-access-keys)します。

2.  Azure PowerShell で ADFGetStartedPSH フォルダーに切り替えます。
3.  使用することができます、 **新規 AzureRmDataFactoryLinkedService** コマンドレットをリンクされたサービスを作成します。 このコマンドレットでは、このチュートリアルで使用するその他の Data Factory コマンドレットには、値を渡す、 *ResourceGroupName* と *DataFactoryName* パラメーター。 また、使用することができます **Get AzureRmDataFactory** を取得する、 **DataFactory** オブジェクトを入力しなくても、オブジェクトを渡す *ResourceGroupName* と *DataFactoryName* コマンドレットを実行するたびにします。 出力を割り当てるには、次のコマンドを実行、 **Get AzureRmDataFactory** コマンドレットを **$df** 変数です。

        $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name DataFactoryMyFirstPipelinePSH

4.  これで、実行、 **新規 AzureRmDataFactoryLinkedService** コマンドレットでは、リンクの作成を **StorageLinkedService** サービスです。

        New-AzureRmDataFactoryLinkedService $df -File .\StorageLinkedService.json

    実行している場合、 **Get AzureRmDataFactory** コマンドレットと出力を **$df** 変数しなければならなくなる値を指定、 *ResourceGroupName* と *DataFactoryName* パラメーターは次のようにします。

        New-AzureRmDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName ADFTutorialDataFactoryPSH -File .\StorageLinkedService.json

    実行したはチュートリアルの途中で Azure PowerShell を閉じた場合、 **Get AzureRmDataFactory** コマンドレットの次のチュートリアルを完了する Azure PowerShell を起動したとき。

### Azure HDInsight のリンクされたサービスを作成する
次に、Hive スクリプトの実行に使用するオンデマンド Azure HDInsight クラスター用のリンクされたサービスを作成します。

1.  次の内容で C:\ADFGetStartedPSH に「HDInsightOnDemandLinkedService.json」という名前の JSON ファイルを作成します。


        {
          "name": "HDInsightOnDemandLinkedService",
          "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
              "version": "3.2",
              "clusterSize": 1,
              "timeToLive": "00:30:00",
              "linkedServiceName": "StorageLinkedService"
            }
          }
        }

    The following table provides descriptions for the JSON properties used in the snippet:

    Property | Description
    -------- | -----------
    Version | This specifies that the version of the HDInsight created to be 3.2.
    ClusterSize | This creates a one node HDInsight cluster.
    TimeToLive | This specifies that the idle time for the HDInsight cluster, before it is deleted.
    linkedServiceName | This specifies the storage account that will be used to store the logs that are generated by HDInsight
2. 実行、 **新規 AzureRmDataFactoryLinkedService** コマンドレットを HDInsightOnDemandLinkedService と呼ばれる、リンクされたサービスを作成します。

        New-AzureRmDataFactoryLinkedService $df -File .\HDInsightOnDemandLinkedService.json


### 出力データセットを作成します。
次に、Azure BLOB ストレージに格納されるデータを表す出力データセットを作成します。

1.  以下の内容を記述した「OutputTable.json」という名前の JSON ファイルを C:\ADFGetStartedPSH フォルダー内に作成します。

        {
          "name": "AzureBlobOutput",
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
        }

    前の例では、という名前のデータセットを作成する **AzureBlobOutput**, と Hive スクリプトによって生成されるデータの構造を指定します。 結果はという名前の blob コンテナーに格納されていることを指定するさらに、 **データ** という名前のフォルダーと **partitioneddata**します。  **可用性** セクションでは、出力データセットが 1 か月ごとに生成されることを指定します。

2. 次のコマンドを Azure PowerShell で実行して、Data Factory データセットを作成します。

        New-AzureRmDataFactoryDataset $df -File .\OutputTable.json

## 手順 3: 最初のパイプラインを作成する
この手順では、最初のパイプラインを作成します。

1.  以下の内容を記述した「MyFirstPipelinePSH.json」という名前の JSON ファイルを C:\ADFGetStartedPSH フォルダー内に作成します。

    > [AZURE.IMPORTANT] 置換 **storageaccountname** 、JSON でストレージ アカウントの名前に置き換えます。

        {
          "name": "MyFirstPipeline",
          "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
              {
                "type": "HDInsightHive",
                "typeProperties": {
                  "scriptPath": "script/partitionweblogs.hql",
                  "scriptLinkedService": "StorageLinkedService",
                  "defines": {
                    "partitionedtable": "wasb://data@<storageaccountname>.blob.core.windows.net/partitioneddata"
                  }
                },
                "outputs": [
                  {
                    "name": "AzureBlobOutput"
                  }
                ],
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

    前の例では、Hive を使用して HDInsight クラスターのデータを処理する 1 つのアクティビティで構成されるパイプラインを作成します。

    Hive スクリプト ファイル、partitionweblogs.hql、(StorageLinkedService と呼ばれる scriptLinkedService によって指定された) Azure のストレージ アカウントとという名前のコンテナーに格納されて **スクリプト**します。

     **定義** セクションを使用して、Hive 構成値 (例: ${hiveconf:partitioneddata}) として hive スクリプトに渡される実行時の設定を指定します。

     **開始** と **エンド** パイプラインのプロパティは、パイプラインのアクティブな期間を指定します。

    リンクされたサービスで指定されたコンピューターで Hive スクリプトを実行するよう指定する、アクティビティ JSON で **HDInsightOnDemandLinkedService**します。
2. 次のコマンドを実行して、Data Factory テーブルを作成します。

        New-AzureRmDataFactoryPipeline $df -File .\MyFirstPipelinePSH.json
5. これで、Azure PowerShell を使用して最初のパイプラインを作成できました。

### <a name="MonitorDataSetsAndPipeline"></a> データセットとパイプラインを監視する
このステップでは、Azure PowerShell を使用して、Azure Data Factory の状況を監視します。

1.  実行 **Get AzureRmDataFactory** への出力を割り当てると、 **$df** 変数です。

        $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name DataFactoryMyFirstPipelinePSH

2.  実行 **Get AzureRmDataFactorySlice** のすべてのスライスに関する詳細情報、 **EmpSQLTable**, 、これは、パイプラインの出力テーブルです。  

        Get-AzureRmDataFactorySlice $df -DatasetName AzureBlobOutput -StartDateTime 2014-01-01

    ここで指定した StartDateTime がパイプライン JSON で指定されている開始時刻と同じであることに注意してください。 次のような出力が表示されます。

        ResourceGroupName : ADFTutorialResourceGroup
        DataFactoryName   : DataFactoryMyFirstPipelinePSH
        TableName         : AzureBlobOutput
        Start             : 1/1/2014 12:00:00 AM
        End               : 2/1/2014 12:00:00 AM
        RetryCount        : 0
        Status            : InProgress
        LatencyStatus     :
        LongRetryCount    : 0

3.  実行 **Get AzureRmDataFactoryRun** アクティビティの特定のスライスの実行の詳細を取得します。

        Get-AzureRmDataFactoryRun $df -DatasetName AzureBlobOutput -StartDateTime 2014-01-01

    次のような出力が表示されます。

        Id                  : 4dbc6a07-537d-4005-a53e-6b9a4b844089_635241312000000000_635268096000000000_AzureBlobOutput
        ResourceGroupName   : ADFTutorialResourceGroup
        DataFactoryName     : DataFactoryMyFirstPipelinePSH
        TableName           : AzureBlobOutput
        ProcessingStartTime : 7/7/2015 1:14:18 AM
        ProcessingEndTime   : 12/31/9999 11:59:59 PM
        PercentComplete     : 0
        DataSliceStart      : 1/1/2014 12:00:00 AM
        DataSliceEnd        : 2/1/2014 12:00:00 AM
        Status              : AllocatingResources
        Timestamp           : 7/7/2015 1:14:18 AM
        RetryAttempt        : 0
        Properties          : {}
        ErrorMessage        :
        ActivityName        : RunSampleHiveActivity
        PipelineName        : MyFirstPipeline
        Type                : Script

    スライスが Ready 状態または Failed 状態になるまでこのコマンドレットを実行させたままにできます。 スライスが Ready 状態になったら、BLOB ストレージの data コンテナーの partitioneddata フォルダーで出力データを調べます。  オンデマンド HDInsight クラスターの作成には通常しばらく時間がかかることに注意してください。

参照してください [Data Factory コマンドレット リファレンス](https://msdn.microsoft.com/library/azure/dn820234.aspx) Data Factory コマンドレットに関する包括的なドキュメントについてです。



## 次のステップ
この記事では、オンデマンド Azure HDInsight クラスターで Hive スクリプトを実行する変換アクティビティ (HDInsight アクティビティ) を含むパイプラインを作成しました。 Azure Blob から Azure SQL にデータをコピーするコピー アクティビティを使用する方法を確認するには、次を参照してください。 [チュートリアル: Azure Blob から Azure SQL にデータをコピー](./data-factory-get-started.md)します。


[cmdlet-reference]: https://msdn.microsoft.com/library/azure/dn820234(v=azure.98).aspx

