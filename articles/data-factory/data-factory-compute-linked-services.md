<properties
    pageTitle="コンピューティングのリンクされたサービス | Microsoft Azure"
    description="Azure Data Factory パイプラインでデータの変換/処理に使用できるコンピューティング環境について学習します。"
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
    ms.date="12/10/2015"
    ms.author="spelluru"/>

# コンピューティングのリンクされたサービス

この記事では、データの処理または変換に利用できるさまざまなコンピューティング環境について説明します。 これらのコンピューティング環境を Azure Data Factory にリンクする「リンクされたサービス」の構成時に Data Factory でサポートされるさまざまな構成 (オンデマンドと Bring Your Own の比較) に関する詳細も提供します。

## オンデマンドのコンピューティング環境

この種類の構成では、コンピューティング環境は Azure Data Factory サービスにより完全管理されます。 データを処理するためのジョブが送信される前に Data Factory サービスにより自動的に作成され、ジョブの完了時に削除されます。 オンデマンドのコンピューティング環境のために「リンクされたサービス」を作成し、構成し、ジョブ実行、クラスター管理、ブートストラップ アクションの詳細設定を制御できます。

> [AZURE.NOTE] オンデマンド構成は Azure HDInsight クラスターにのみ現在サポートされています。

## Azure HDInsight のオンデマンドのリンクされたサービス

オンデマンド HDInsight クラスターはデータを処理するために Azure Data Factory サービスによって自動的に作成されます。 このクラスターはクラスターに関連付けられているストレージ アカウント (JSON の linkedServiceName プロパティ) と同じリージョンで作成されます。

次に注意してください **重要** 点、オンデマンド HDInsight のリンクされたサービス。

- 作成したオンデマンド HDInsight クラスターは Azure サブスクリプションに表示されません。Azure Data Factory サービスがあなたの代わりにオンデマンド HDInsight クラスターを管理します。
- オンデマンド HDInsight クラスターで実行されるジョブのログは HDInsight クラスターに関連付けられているストレージ アカウントにコピーされます。 Azure 旧ポータルからこれらのログにアクセスすることができます、 **アクティビティの実行の詳細** ブレードです。 参照してください [モニターおよび管理のパイプライン](data-factory-monitor-manage-pipelines.md) の詳細については、資料です。
- HDInsight クラスターが稼動し、ジョブを実行している時間に対してのみ課金されます。

> [AZURE.IMPORTANT] 通常以上 **15 分** オンデマンド Azure HDInsight クラスターをプロビジョニングします。

### 例

    {
      "name": "HDInsightOnDemandLinkedService",
      "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
          "clusterSize": 4,
          "timeToLive": "00:05:00",
          "version": "3.2",
          "osType": "linux",
          "linkedServiceName": "MyBlobStore",
          "hcatalogLinkedServiceName": "AzureSqlLinkedService",
          "additionalLinkedServiceNames": [
            "otherLinkedServiceName1",
            "otherLinkedServiceName2"
          ]
        }
      }
    }

### プロパティ

プロパティ | 説明 | 必須
-------- | ----------- | --------
type | Type プロパティに設定する必要があります **HDInsightOnDemand**します。 | あり
clusterSize | オンデマンド クラスターのサイズです。 このオンデマンド クラスターに配置するノードの数を指定します。 | はい
timetolive | <p>オンデマンド HDInsight クラスターの許容アイドル時間。 どのくらいの期間、オンデマンド HDInsight クラスターは残り続けることに、アクティビティのクラスター内の他のアクティブなジョブがない場合に実行が完了した後を指定します。</p><p>たとえば、アクティビティの実行に 6 分かかり、timetolive が 5 分に設定されている場合は、クラスターは存続 5 分後に 6 分間のアクティビティを処理を実行します。 別のアクティビティの実行が 6 分のウィンドウで実行した場合は、同じクラスターによって処理されます。</p><p>高価な操作が (しばらくかかる場合があります)、オンデマンド HDInsight クラスターを再利用して data factory のパフォーマンスを向上させるために必要に応じてこの設定は使用するには、オンデマンド HDInsight クラスターの作成です。</p><p>Timetolive 値を 0 に設定する場合は、アクティビティの実行処理されるとすぐに、クラスターが削除されます。 その一方で、高い値を設定した場合、クラスターは不必要にアイドル状態を維持し、コストの上昇を招きます。 そのため、これがのニーズに合わせて適切な値を設定することが重要です。</p><p>Timetolive プロパティ値が適切に設定されている場合、複数のパイプラインは、オンデマンド HDInsight クラスターの同じインスタンスを共有できます。</p> | あり
version | HDInsight クラスターのバージョン。 既定値は、Windows クラスターでは 3.1、Linux クラスターでは 3.2 です。 | いいえ
linkedServiceName | データの保存し、処理するためのオンデマンド クラスターで使用される BLOB ストアです。 | はい
additionalLinkedServiceNames | Data Factory サービスがあなたの代わりに登録できるように、HDInsight の「リンクされたサービス」の追加ストレージ アカウントを指定します。 | いいえ
osType | オペレーティング システムの種類。 使用可能な値:  Windows (既定値) および Linux | いいえ
hcatalogLinkedServiceName | HCatalog データベースを指す Azure SQL のリンクされたサービスの名前。 オンデマンド HDInsight クラスターは、Azure SQL データベースをメタストアとして使用して作成されます。 | いいえ

### 高度なプロパティ

次のプロパティを指定し、オンデマンド HDInsight クラスターを詳細に設定することもできます。

プロパティ | 説明 | 必須
:-------- | :----------- | :--------
coreConfiguration | 作成する HDInsight クラスターに core 構成パラメーター (core-site.xml と同じ) を指定します。 | いいえ
hBaseConfiguration | HDInsight クラスターに HBase 構成パラメーター (hbase-site.xml) を指定します。 | いいえ
hdfsConfiguration | HDInsight クラスターに HDFS 構成パラメーター (hdfs-site.xml) を指定します。 | いいえ
hiveConfiguration | HDInsight クラスターに hive 構成パラメーター (hive-site.xml) を指定します。 | いいえ
mapReduceConfiguration | HDInsight クラスターに MapReduce 構成パラメーター (mapred-site.xml) を指定します。 | いいえ
oozieConfiguration | HDInsight クラスターに Oozie 構成パラメーター (oozie-site.xml) を指定します。 | いいえ
stormConfiguration | HDInsight クラスターに Storm 構成パラメーター (storm-site.xml) を指定します。 | いいえ
yarnConfiguration | HDInsight クラスターに Yarn 構成パラメーター (yarn-site.xml) を指定します。 | いいえ

#### 例 – オンデマンド HDInsight クラスターと詳細なプロパティ

    {
      "name": " HDInsightOnDemandLinkedService",
      "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
          "clusterSize": 16,
          "timeToLive": "01:30:00",
          "version": "3.2",
          "linkedServiceName": "adfods1",
          "coreConfiguration": {
            "templeton.mapper.memory.mb": "5000"
          },
          "hiveConfiguration": {
            "templeton.mapper.memory.mb": "5000"
          },
          "mapReduceConfiguration": {
            "mapreduce.reduce.java.opts": "-Xmx4000m",
            "mapreduce.map.java.opts": "-Xmx4000m",
            "mapreduce.map.memory.mb": "5000",
            "mapreduce.reduce.memory.mb": "5000",
            "mapreduce.job.reduce.slowstart.completedmaps": "0.8"
          },
          "yarnConfiguration": {
            "yarn.app.mapreduce.am.resource.mb": "5000",
            "mapreduce.map.memory.mb": "5000"
          },
          "additionalLinkedServiceNames": [
            "datafeeds",
            "adobedatafeed"
          ]
        }
      }
    }

### ノードのサイズ
次のプロパティを使用して、ヘッド ノード、データ ノード、Zookeeper ノードのサイズを指定できます。 

プロパティ | 説明 | 必須
:-------- | :----------- | :--------
headNodeSize | ヘッド ノードのサイズを指定します。 既定値は "Large" です。 参照してください、 **ノード サイズを指定する** 詳細については後述します。 | いいえ
dataNodeSize | データ ノードのサイズを指定します。 既定値は "Large" です。 | いいえ
zookeeperNodeSize | Zookeeper ノードのサイズを指定します。 既定値は "Small" です。 | いいえ
 
#### ノードのサイズの指定
参照してください、 [仮想マシンのサイズ](../virtual-machines/virtual-machines-size-specs.md#size-tables) 記事文字列の値がこれらのプロパティを指定する必要があります。 準拠する必要がある、 **コマンドレットと API** 、記事内で参照します。 この記事に示すように、Large (既定値) サイズのデータ ノードのメモリ容量は 7 GB ですが、シナリオによってはこれでは不十分な場合があります。 

指定する必要がある D4 サイズのヘッドノードとワーカー ノードを作成する場合は、 **Standard_D4** headNodeSize および dataNodeSize プロパティの値として。 

    "headNodeSize": "Standard_D4",  
    "dataNodeSize": "Standard_D4",

これらのプロパティに対する不正な値を指定する場合は、次が表示される **エラー:** をクラスターを作成できませんでした。 例外: クラスター作成操作を完了できません。 コード '400' で操作が失敗しました。 取り残されたクラスターの状態: 'Error'。 メッセージ: 'PreClusterCreationValidationFailure'。" というエラー メッセージが表示される場合があります。 このエラーが発生した場合は、使用していることを確認、 **コマンドレットと API** 上記の文書にテーブルの名前。  



## Bring Your Own のコンピューティング環境

この種類の構成では、既存のコンピューティング環境を Data Factory の「リンクされたサービス」として登録できます。 このコンピューティング環境はユーザーにより管理され、Data Factory サービスをこの環境を利用し、アクティビティを実行します。

この種類の構成は次のコンピューティング環境でサポートされています。

- Azure HDInsight
- Azure Batch
- Azure Machine Learning

## Azure HDInsight のリンクされたサービス

Azure HDInsight の「リンクされたサービス」を作成し、独自の HDInsight クラスターを Data Factory に登録できます。

### 例

    {
      "name": "HDInsightLinkedService",
      "properties": {
        "type": "HDInsight",
        "typeProperties": {
          "clusterUri": " https://<hdinsightclustername>.azurehdinsight.net/",
          "userName": "admin",
          "password": "<password>",
          "location": "WestUS",
          "linkedServiceName": "MyHDInsightStoragelinkedService"
        }
      }
    }

### プロパティ

プロパティ | 説明 | 必須
-------- | ----------- | --------
type | Type プロパティに設定する必要があります **HDInsight**します。 | あり
clusterUri | HDInsight クラスターの URI です。 | はい
username | 既存の HDInsight クラスターに接続するために使用されるユーザーの名前を指定します。 | はい
パスワード | ユーザー アカウントのパスワードを指定します。 | あり
location | HDInsight クラスターの場所を指定します (例: WestUS)。 | はい
linkedServiceName | HDInsight クラスターで使用される BLOB ストレージの「リンクされたサービス」の名前です。 | はい

## Azure Batch の「リンクされたサービス」

Azure Batch の「リンクされたサービス」を作成し、仮想マシン (VM) の Batch プールを Data Factory に登録できます。 Azure Batch と Azure HDInsight のいずれかを利用し、.NET カスタム アクティビティを実行できます。

Azure Batch サービスの利用が初めての場合、次のトピックを参照してください。


- [Azure のバッチ基本](../batch/batch-technical-overview.md) Azure Batch サービスの概要についてです。
- [New-azurebatchaccount](https://msdn.microsoft.com/library/mt125880.aspx) Azure Batch アカウントを作成するコマンドレット (または) [Azure Classic Portal](../batch/batch-account-create-portal.md) Azure クラシック ポータルを使用して Azure Batch アカウントを作成します。 参照してください [Azure Batch アカウントを管理するには、[PowerShell の使用](http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx) コマンドレットの使用に関する詳細な手順についてのトピックです。
- [New-azurebatchpool](https://msdn.microsoft.com/library/mt125936.aspx) コマンドレットを Azure Batch プールを作成します。

### 例

    {
      "name": "AzureBatchLinkedService",
      "properties": {
        "type": "AzureBatch",
        "typeProperties": {
          "accountName": "<Azure Batch account name>",
          "accessKey": "<Azure Batch account key>",
          "poolName": "<Azure Batch pool name>",
          "linkedServiceName": "<Specify associated storage linked service reference here>"
        }
      }
    }

追加"**. < 地域名**"バッチ アカウントの名前に、 **accountName** プロパティです。 例:

            "accountName": "mybatchaccount.eastus"

もう 1 つの選択肢は下のように batchUri エンドポイントを指定することです。  

            "accountName": "adfteam",
            "batchUri": "https://eastus.batch.azure.com",

### プロパティ

プロパティ | 説明 | 必須
-------- | ----------- | --------
type | Type プロパティに設定する必要があります **AzureBatch**します。 | あり
accountName | Azure Batch アカウントの名前です。 | あり
accessKey | Azure Batch アカウントのアクセス キーです。 | はい
poolName | 仮想マシンのプールの名前です。 | はい
linkedServiceName | この Azure Batch の「リンクされたサービス」に関連付けられている Azure Storage の「リンクされたサービス」の名前です。 この「リンクされたサービス」はアクティビティの実行に必要なファイルのステージングとアクティビティ実行ログの保存に利用されます。 | はい


## Azure Machine Learning のリンクされたサービス

Azure Machine Learning の「リンクされたサービス」を作成し、Machine Learning のバッチ スコアリング エンドポイントを Data Factory に登録します。

### 例

    {
      "name": "AzureMLLinkedService",
      "properties": {
        "type": "AzureML",
        "typeProperties": {
          "mlEndpoint": "https://[batch scoring endpoint]/jobs",
          "apiKey": "<apikey>"
        }
      }
    }

### プロパティ

プロパティ | 説明 | 必須
-------- | ----------- | --------
型 | Type プロパティに設定する必要があります: **AzureML**します。 | あり
mlEndpoint | バッチ スコアリング URL です。 | はい
apiKey | 公開されたワークスペース モデルの API です。 | はい


## Azure Data Lake Analytics リンク サービス
作成する、 **Azure Data Lake 分析** Azure Data Lake 分析をリンクするリンクされたサービスはコンピューティング Azure data factory サービスを使用する前に、 [Data Lake 分析 U SQL アクティビティ](data-factory-usql-activity.md) パイプラインにします。

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
accountName | Azure Data Lake Analytics アカウント名。 | はい
dataLakeAnalyticsUri | Azure Data Lake Analytics URI。 |  いいえ
authorization | 認証コードがクリックすると自動的に取得される **Authorize** Data Factory エディターでのボタンをクリックし、OAuth のログインを完了します。 | あり
subscriptionId | Azure サブスクリプション ID | いいえ (指定されていない場合は Data Factory のサブスクリプションが使用されます)。
resourceGroupName | Azure リソース グループ名 |  いいえ (指定されていない場合は Data Factory のリソース グループが使用されます)。
sessionId | OAuth 承認セッションのセッション ID です。 各セッション ID は一意であり、1 回のみ使用できます。 セッション ID は、Data Factory Editor で自動生成されます。 | あり


## Azure SQL のリンクされたサービス

Azure SQL のリンクされたサービスを作成し、それを使用、 [ストアド プロシージャ アクティビティ](data-factory-stored-proc-activity.md) Data Factory パイプラインからストアド プロシージャを呼び出します。 参照してください [Azure SQL コネクタ](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) このリンク サービスに関する詳細については、資料です。

