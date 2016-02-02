<properties
    pageTitle="Azure Data Factory を使用してデータをオンプレミスの SQL Server から SQL Azure に移動する | Azure"
    description="オンプレミスとクラウド内のデータベース間で毎日同時にデータを移動する 2 つのデータ移行アクティビティを構成する ADF パイプラインを設定します。"
    services="machine-learning"
    documentationCenter=""
    authors="fashah"
    manager="jacob.spoelstra"
    editor=""
    videoId=""
    scriptId="" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2015"
    ms.author="fashah;bradsev" />



# Azure Data Factory を使用してオンプレミスの SQL Server から SQL Azure にデータを移動する

このトピックでは、Azure Data Factory (ADF) を使用して、オンプレミスの SQL Server データベースから Azure BLOB ストレージを経由して SQL Azure データベースにデータを移動する方法を説明します。

## <a name="intro"></a>概要: ADF とその使用すべき状況データを移行するでしょうか。

Azure Data Factory は、データの移動や変換を調整し自動化する、完全に管理されたクラウドベースのデータ統合サービスです。 ADF モデルにおける主要な概念は、パイプラインです。 パイプラインはアクティビティの論理グループで、それぞれがデータセットに含まれているデータに対して実行するアクションを定義します。 リンクされたサービスは、Data Factory がデータ リソースに接続するために必要な情報を定義するために使用されます。

ADF を使用すると、既存のデータ処理サービスを、可用性が高く、クラウドで管理されるデータ パイプラインに組み込むことができます。 これらのデータ パイプラインは、データの取り込み、準備、変換、分析、および発行のためのスケジュールが設定できます。ADF が複雑なデータと処理の依存関係をすべて管理して調整します。 増加するオンプレミスのデータ ソースとクラウドのデータ ソースを接続するソリューションをクラウド内で迅速に構築してデプロイすることができます。

オンプレミスとクラウドの両方のリソースにアクセスするハイブリッド シナリオで、継続的にデータを移行する必要がある場合、および移行の過程で、データを処理する場合や、データに変更を加えたりビジネス ロジックを付加したりする必要がある場合には、ADF の使用を検討してください。 ADF では、定期的にデータの移動を管理するシンプルな JSON スクリプトを使用して、ジョブのスケジュールと監視ができます。 ADF には他にも、複雑な操作のサポートなどの機能があります。 ADF の詳細については、あるドキュメントを参照してください。 [Azure Data Factory (ADF)](http://azure.microsoft.com/services/data-factory/)します。

## <a name="scenario"></a>シナリオでは、

オンプレミスの SQL Database とクラウド内の Azure SQL Database 間で毎日同時にデータを移動する 2 つのデータ移行アクティビティを構成する ADF パイプラインを設定します。 2 つのアクティビティは次のとおりです。

* オンプレミスの SQL Server データベースから Azure BLOB ストレージ アカウントにデータをコピーする
* Azure BLOB ストレージ アカウントから Azure SQL Database にデータをコピーする

**参照**: ここより詳細なチュートリアルからの抜粋した示された手順と、 [内部設置型データを処理するパイプライン](data-factory-use-onpremises-datasources.md) 指定された、ADF でチームとそのトピックの関連するセクションへの参照が該当する場合。


## <a name="prereqs"></a>前提条件

このチュートリアルでは、以下があることを前提としています。

* **Azure サブスクリプション**。 サインアップすることができます、サブスクリプションを持っていない場合、 [無料評価版](https://azure.microsoft.com/pricing/free-trial/)します。
* **Azure ストレージ アカウント**。 このチュートリアルのデータを格納するには、Azure ストレージ アカウントを使用します。 Azure ストレージ アカウントを取得していない場合、 [ストレージ アカウントの作成](storage-create-storage-account.md#create-a-storage-account) 記事です。 ストレージ アカウントを作成した後は、ストレージにアクセスするために使用するアカウント キーを取得する必要があります。 参照してください [表示、コピーおよび再生成するストレージ アクセス キー](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)します。
* **Azure SQL Database** へのアクセス権。 場合は、Azure SQL データベースをセットアップする必要があります [Microsoft Azure SQL Database の概要 ](sql-database-get-started.md) を Azure SQL データベースの新しいインスタンスをプロビジョニングする方法について説明します。
* **Azure PowerShell** がローカルにインストールされ構成されていること。 手順については、次を参照してください。 [をインストールして、Azure PowerShell を構成する方法](powershell-install-configure.md)します。

> [AZURE.NOTE] ここでは、 [Azure ポータル](https://ms.portal.azure.com/)します。

## <a name="upload-data"></a> 内部設置型 SQL Server にデータをアップロードします。

使用して、 [NYC タクシー データセット](http://chriswhong.com/open-data/foil_nyc_taxi/) 移行プロセスを説明します。 NYC タクシー データセットは、Azure blob ストレージにその投稿を説明したように、使用可能な [NYC タクシー データ](http://www.andresmh.com/nyctaxitrips/)します。 データは、2 つのファイル、乗車の詳細を含む trip_data.csv ファイルおよび各乗車に対して支払われた料金の詳細を含む trip_far.csv ファイルができます。 サンプルとこれらのファイルの説明で記述されて [NYC タクシー乗車データセットの説明](machine-learning-data-science-process-sql-walkthrough.md#dataset)します。


ここに示されている手順は、自身のデータに適用することも、NYC タクシー データセットを使用してこの手順に従って行うこともできます。 内部設置型 SQL Server データベースには、NYC タクシー データセットをアップロードするに記載されている手順に従って [SQL Server データベースにデータを一括インポート](machine-learning-data-science-process-sql-walkthrough.md#dbload)します。 これらは Azure Virtual Machine 上の SQL Server にアップロードする手順ですが、オンプレミスの SQL Server へのアップロード手順も同じです。

## <a name="create-adf"></a> Azure Data Factory を作成します。

新しい Azure Data Factory および内のリソース グループを作成する方法、について、 [Azure ポータル](https://ms.portal.azure.com/) もの [Azure Data Factory を作成](data-factory-build-your-first-pipeline-using-editor.md#step-1-creating-the-data-factory)します。 新しい ADF インスタンスに *adfdsp* という名前を付け、作成されたリソース グループに *adfdsprg* という名前を付けます。

## Data Management Gateway をインストールして構成する

Azure Data Factory 内でパイプラインがオンプレミスの SQL Server を使用できるようにするには、リンクされたサービスとしてその SQL Server を ADF に追加する必要があります。 オンプレミスの SQL Server にリンクされたサービスを作成するには、まず Microsoft Data Management Gateway をダウンロードしてオンプレミスのコンピューターにインストールし、オンプレミスのデータ ソースがゲートウェイを使用できるようにリンクされたサービスを構成する必要があります。 Data Management Gateway では、データのシリアル化と逆シリアル化、データがホストされているコンピューター上のデータの同期が行われます。

セットアップ手順と Data Management Gateway の詳細については、を参照してください [内部設置型データを処理するパイプライン](data-factory-use-onpremises-datasources.md)。


## <a name="adflinkedservices"></a>データ リソースに接続するリンクされたサービスを作成します。

リンクされたサービスは、Azure Data Factory がデータ リソースに接続するために必要な情報を定義します。 リンクされたサービスを作成するための手順が記載 [リンクされたサービスを作成する](data-factory-use-onpremises-datasources.md#step-2-create-linked-services)します。

このシナリオには、リンクされたサービスを必要とする 3 つのリソースがあります。

1. [内部設置型 SQL Server のリンクされたサービス](#adf-linked-service-onprem-sql)
2. [Azure Blob ストレージのリンクされたサービス](#adf-linked-service-blob-store)
3. [Azure SQL データベースのリンクされたサービス](#adf-linked-service-azure-sql)


### <a name="adf-linked-service-onprem-sql"></a>内部設置型 SQL Server データベースのリンクされたサービス

オンプレミスの SQL Server用にリンクされたサービスを作成するには、Azure クラシック ポータルで ADF ランディング ページ内の **[データ ストア]** をクリックし、*SQL* を選択してオンプレミスの SQL Server の*ユーザー名*と*パスワード*に対して資格情報を入力します。 サーバー名は**完全修飾サーバー名、バックスラッシュ、インスタンス名 (servername\instancename)** で入力する必要があります。 リンクされたサービスに *adfonpremsql* という名前を付けます。

### <a name="adf-linked-service-blob-store"></a>Blob のリンクされたサービス

Azure BLOB ストレージ アカウント用にリンクされたサービスを作成するには、Azure クラシック ポータルで ADF ランディング ページ内の **[データ ストア]** をクリックし、*ADF ストレージアカウント*を選択して Azure BLOB ストレージ アカウント キーとコンテナー名を入力します。 リンク サービスに *adfds* という名前を付けます。

### <a name="adf-linked-service-azure-sql"></a>Azure SQL データベースのリンクされたサービス

Azure SQL Database 用にリンクされたサービスを作成するには、Azure クラシック ポータルで ADF ランディング ページ内の **[データ ストア]** をクリックし、*Azure SQL* を選択して Azure SQL Database の*ユーザー名*と*パスワード*に対して資格情報を入力します。  *Username* として指定する必要があります *user@servername*します。


## <a name="adf-tables"></a>定義し、データセットにアクセスする方法を指定するテーブルを作成

以下のスクリプトベースの手順に従って、データセットの構造、場所、可用性を指定するテーブルを作成します。 テーブルを定義するには、JSON ファイルを使用します。 これらのファイルの構造の詳細については、次を参照してください。 [データセット](data-factory-create-datasets.md)します。
> [AZURE.NOTE]  実行する必要があります、 `Add-azureaccount` コマンドレットを実行する前に、 [New-azuredatafactorytable](https://msdn.microsoft.com/library/azure/dn835096.aspx) コマンドレットをコマンドの実行権限の Azure のサブスクリプションが選択されていることを確認します。 このコマンドレットのドキュメントについては、次を参照してください。 [Add-azureaccount](https://msdn.microsoft.com/library/azure/dn790372.aspx)します。

テーブル内の JSON ベースの定義では、次の名前が使用されます。

* オンプレミスの SQL サーバーでは、**テーブル名**は *nyctaxi_data* です。
* Azure BLOB ストレージ アカウントでは、**コンテナー名**は *containername* です。

この ADF パイプラインには、次の 3 つのテーブル定義が必要です。

1. [内部設置型の SQL テーブル](#adf-table-onprem-sql)
2. [Blob テーブル ](#adf-table-blob-store)
3. [SQL Azure テーブル](#adf-table-azure-sql)

> [AZURE.NOTE]  次の手順では、Azure PowerShell を使用して ADF アクティビティの定義と作成を行います。 これらのタスクは、Azure ポータルを使用しても行えます。 詳細については、「 [入力と出力データセット](data-factory-use-onpremises-datasources.md#step-3-create-input-and-output-datasets)します。

### <a name="adf-table-onprem-sql"></a>内部設置型の SQL テーブル

オンプレミスの SQL Server のテーブル定義は、次の JSON ファイルで指定されます。

        {
            "name": "OnPremSQLTable",
            "properties":
            {
                "location":
                {
                "type": "OnPremisesSqlServerTableLocation",
                "tableName": "nyctaxi_data",
                "linkedServiceName": "adfonpremsql"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1,   
                "waitOnExternal":
                {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
                }
    
                }
            }
        }

列名が指定されていないここでは、ここで、副選択列の名前にできます (に関する詳細情報がチェックをご覧ください、 [ADF ドキュメント](data-factory-copy-activity.md))します。

テーブルの JSON 定義を *onpremtabledef.json* というファイルにコピーし、それを既知の場所に保存します (ここでは、*C:\temp\onpremtabledef.json*)。 次の Azure PowerShell コマンドレッドを使用して、ADF 内にテーブルを作成します。

    New-AzureDataFactoryTable -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp –File C:\temp\onpremtabledef.json

### <a name="adf-table-blob-store"></a>Blob テーブル

以下は、出力 BLOB の場所用のテーブル定義です (これはオンプレミスから取り込まれたデータを Azure BLOB にマップします)。

        {
            "name": "OutputBlobTable",
            "properties":
            {
                "location":
                {
                "type": "AzureBlobLocation",
                "folderPath": "containername",
                "format":
                {
                "type": "TextFormat",
                "columnDelimiter": "\t"
                },
                "linkedServiceName": "adfds"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1
                }
            }
        }

テーブルの JSON 定義を *bloboutputtabledef.json* というファイルにコピーし、それを既知の場所に保存します (ここでは、*C:\temp\bloboutputtabledef.json*)。 次の Azure PowerShell コマンドレッドを使用して、ADF 内にテーブルを作成します。

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\bloboutputtabledef.json  

### <a name="adf-table-azure-sq"></a>SQL Azure テーブル

以下は、SQL Azure 出力の場所用のテーブル定義です (このスキーマは BLOB からのデータをマップします)。

    {
        "name": "OutputSQLAzureTable",
        "properties":
        {
            "structure":
            [
                { "name": "column1", type": "String"},
                { "name": "column2", type": "String"}                
            ],
            "location":
            {
                "type": "AzureSqlTableLocation",
                "tableName": "your_db_name",
                "linkedServiceName": "adfdssqlazure_linked_servicename"
            },
            "availability":
            {
                "frequency": "Day",
                "interval": 1            
            }
        }
    }

テーブルの JSON 定義を *AzureSqlTable.json* というファイルにコピーし、それを既知の場所に保存します (ここでは、*C:\temp\AzureSqlTable.json*)。 次の Azure PowerShell コマンドレッドを使用して、ADF 内にテーブルを作成します。

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\AzureSqlTable.json  

## <a name="adf-pipeline"></a>定義し、パイプラインの作成

次のスクリプトベースの手順に従って、パイプラインに属するアクティビティを指定し、パイプラインを作成します。 パイプラインのプロパティを定義するため、JSON ファイルを使用します。

* このスクリプトでは、**パイプライン名**を *AMLDSProcessPipeline* としています。
* また、既定の実行時間 (12 am UTC) を使用して、ジョブが毎日実行されるようにパイプラインの周期性を設定していることにも注目してください。

> [AZURE.NOTE]  次の手順では、Azure PowerShell を使用して ADF パイプラインの定義と作成を行います。 このタスクは、Azure ポータルを使用しても行えます。 詳細については、「 [の作成および実行パイプライン](data-factory-use-onpremises-datasources.md#step-4-create-and-run-a-pipeline)します。

上記のテーブル定義を使用して、ADF のパイプライン定義を次のように指定します。

        {
            "name": "AMLDSProcessPipeline",
            "properties":
            {
                "description" : "This pipeline has one Copy activity that copies data from an on-premise SQL to Azure blob",
                 "activities":
                [
                    {
                        "name": "CopyFromSQLtoBlob",
                        "description": "Copy data from on-premise SQL server to blob",     
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OnPremSQLTable"} ],
                        "outputs": [ {"name": "OutputBlobTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "SqlSource",
                                "sqlReaderQuery": "select * from nyctaxi_data"
                            },
                            "sink":
                            {
                                "type": "BlobSink"
                            }   
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 0,
                            "timeout": "01:00:00"
                        }       
    
                     },
    
                    {
                        "name": "CopyFromBlobtoSQLAzure",
                        "description": "Push data to Sql Azure",        
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OutputBlobTable"} ],
                        "outputs": [ {"name": "OutputSQLAzureTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "BlobSource"
                            },
                            "sink":
                            {
                                "type": "SqlSink",
                                "WriteBatchTimeout": "00:5:00",             
                            }           
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 2,
                            "timeout": "02:00:00"
                        }
                     }
                ]
            }
        }

パイプラインのこの JSON 定義を *pipelinedef.json* というファイルにコピーし、それを既知の場所に保存します (ここでは、*C:\temp\pipelinedef.json*)。 次の Azure PowerShell コマンドレッドを使用して、ADF 内にパイプラインを作成します。

    New-AzureDataFactoryPipeline  -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\pipelinedef.json

Azure クラシック ポータルで (図をクリックすると) ADF 上にパイプラインが次のように表示されることを確認します。

![](media/machine-learning-data-science-move-sql-azure-adf/DJP1kji.png)

## <a name="adf-pipeline-start"></a>パイプラインを開始します。

これで、次のコマンドを使用してパイプラインを実行できます。

    Set-AzureDataFactoryPipelineActivePeriod -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp -StartDateTime startdateZ –EndDateTime enddateZ –Name AMLDSProcessPipeline

*startdate* と *enddate* のパラメーター値を、パイプラインを実行する実際の開始日と終了日に置き換える必要があります。

パイプラインを実行すると、BLOB に選択したコンテナー内に表示されるデータを確認することができます (1 日につき 1 ファイル)。

ADF が提供するデータを段階的にパイプ処理する機能をまだ活用していないことに注意してください。 ADF によって提供されるその他の機能を実行する方法の詳細については、次を参照してください。、 [ADF ドキュメント](http://azure.microsoft.com/services/data-factory/)します。





