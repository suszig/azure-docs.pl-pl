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

## <a name="intro"></a>概要: ADF の説明とデータの移行に ADF を使用するべきタイミング

Azure Data Factory は、データの移動や変換を調整し自動化する、完全に管理されたクラウドベースのデータ統合サービスです。 ADF モデルにおける主要な概念は、パイプラインです。 パイプラインはアクティビティの論理グループで、それぞれがデータセットに含まれているデータに対して実行するアクションを定義します。 リンクされたサービスは、Data Factory がデータ リソースに接続するために必要な情報を定義するために使用されます。

ADF を使用すると、既存のデータ処理サービスを、可用性が高く、クラウドで管理されるデータ パイプラインに組み込むことができます。 これらのデータ パイプラインは、データの取り込み、準備、変換、分析、および発行のためのスケジュールが設定できます。ADF が複雑なデータと処理の依存関係をすべて管理して調整します。 増加するオンプレミスのデータ ソースとクラウドのデータ ソースを接続するソリューションをクラウド内で迅速に構築してデプロイすることができます。

オンプレミスとクラウドの両方のリソースにアクセスするハイブリッド シナリオで、継続的にデータを移行する必要がある場合、および移行の過程で、データを処理する場合や、データに変更を加えたりビジネス ロジックを付加したりする必要がある場合には、ADF の使用を検討してください。 ADF では、定期的にデータの移動を管理するシンプルな JSON スクリプトを使用して、ジョブのスケジュールと監視ができます。 ADF には他にも、複雑な操作のサポートなどの機能があります。 ADF の詳細については、あるドキュメントを参照してください。 [Azure Data Factory (ADF)](http://azure.microsoft.com/services/data-factory/)します。

## <a name="scenario"></a>シナリオ

オンプレミスの SQL Database とクラウド内の Azure SQL Database 間で毎日同時にデータを移動する 2 つのデータ移行アクティビティを構成する ADF パイプラインを設定します。 2 つのアクティビティは次のとおりです。

* オンプレミスの SQL Server データベースから Azure BLOB ストレージ アカウントにデータをコピーする
* Azure BLOB ストレージ アカウントから Azure SQL Database にデータをコピーする

**参照**: ここより詳細なチュートリアルからの抜粋した示された手順と、 [内部設置型データを処理するパイプライン](data-factory-use-onpremises-datasources.md) 指定された、ADF でチームとそのトピックの関連するセクションへの参照が該当する場合。


## <a name="prereqs"></a>前提条件
このチュートリアルでは、以下があることを前提としています。

*  **Azure サブスクリプション**します。 サインアップすることができます、サブスクリプションを持っていない場合、 [無料評価版](https://azure.microsoft.com/pricing/free-trial/)します。
*  **Azure ストレージ アカウント**します。 このチュートリアルのデータを格納するには、Azure ストレージ アカウントを使用します。 Azure ストレージ アカウントを取得していない場合、 [ストレージ アカウントの作成](storage-create-storage-account.md#create-a-storage-account) 記事です。 ストレージ アカウントを作成した後は、ストレージにアクセスするために使用するアカウント キーを取得する必要があります。 参照してください [表示、コピーおよび再生成するストレージ アクセス キー](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)します。
* アクセスを **Azure SQL Database**します。 場合は、Azure SQL データベースをセットアップする必要があります [Microsoft Azure SQL Database の概要 ](sql-database-get-started.md) を Azure SQL データベースの新しいインスタンスをプロビジョニングする方法について説明します。
* インストールおよび構成さ **Azure PowerShell** ローカルにします。 手順については、次を参照してください。 [をインストールして、Azure PowerShell を構成する方法](powershell-install-configure.md)します。

> [AZURE.NOTE] ここでは、 [Azure ポータル](https://ms.portal.azure.com/)します。

##<a name="upload-data"></a>オンプレミスの SQL Server にデータをアップロードする

使用して、 [NYC タクシー データセット](http://chriswhong.com/open-data/foil_nyc_taxi/) 移行プロセスを説明します。 NYC タクシー データセットは、Azure blob ストレージにその投稿を説明したように、使用可能な [NYC タクシー データ](http://www.andresmh.com/nyctaxitrips/)します。 データは、2 つのファイル、乗車の詳細を含む trip_data.csv ファイルおよび各乗車に対して支払われた料金の詳細を含む trip_far.csv ファイルができます。 サンプルとこれらのファイルの説明で記述されて [NYC タクシー乗車データセットの説明](machine-learning-data-science-process-sql-walkthrough.md#dataset)します。


ここに示されている手順は、自身のデータに適用することも、NYC タクシー データセットを使用してこの手順に従って行うこともできます。 内部設置型 SQL Server データベースには、NYC タクシー データセットをアップロードするに記載されている手順に従って [SQL Server データベースにデータを一括インポート](machine-learning-data-science-process-sql-walkthrough.md#dbload)します。 これらは Azure Virtual Machine 上の SQL Server にアップロードする手順ですが、オンプレミスの SQL Server へのアップロード手順も同じです。

##<a name="create-adf"></a>Azure Data Factory を作成する

新しい Azure Data Factory および内のリソース グループを作成する方法、について、 [Azure ポータル](https://ms.portal.azure.com/) もの [Azure Data Factory を作成](data-factory-build-your-first-pipeline-using-editor.md#step-1-creating-the-data-factory)します。 名前を新しい ADF インスタンス *adfdsp* し作成するリソース グループの名前を付けます *adfdsprg*します。

## Data Management Gateway をインストールして構成する

Azure Data Factory 内でパイプラインがオンプレミスの SQL Server を使用できるようにするには、リンクされたサービスとしてその SQL Server を ADF に追加する必要があります。 オンプレミスの SQL Server にリンクされたサービスを作成するには、まず Microsoft Data Management Gateway をダウンロードしてオンプレミスのコンピューターにインストールし、オンプレミスのデータ ソースがゲートウェイを使用できるようにリンクされたサービスを構成する必要があります。 Data Management Gateway では、データのシリアル化と逆シリアル化、データがホストされているコンピューター上のデータの同期が行われます。

セットアップ手順と Data Management Gateway の詳細については、次を参照してください [、パイプラインの内部設置型データを操作を有効にする。](data-factory-use-onpremises-datasources.md)


## <a name="adflinkedservices"></a>データ リソースに接続するためにリンクされたサービスを作成する

リンクされたサービスは、Azure Data Factory がデータ リソースに接続するために必要な情報を定義します。 リンクされたサービスを作成するための手順が記載 [リンクされたサービスを作成する](data-factory-use-onpremises-datasources.md#step-2-create-linked-services)です。

このシナリオには、リンクされたサービスを必要とする 3 つのリソースがあります。

1. [オンプレミスの SQL Server 用のリンクされたサービス](#adf-linked-service-onprem-sql)
2. [Azure BLOB ストレージ用のリンクされたサービス](#adf-linked-service-blob-store)
3. [Azure SQL データベース用のリンクされたサービス](#adf-linked-service-azure-sql)


###<a name="adf-linked-service-onprem-sql"></a>オンプレミスの SQL Server データベース用のリンクされたサービス
内部設置型 SQL Server のリンクされたサービスを作成するには、をクリックして、 **データ ストア** Azure クラシック ポータルで [ADF ランディング ページで、選択 *SQL* の資格情報を入力し、 *ユーザー名* と *パスワード* 内部設置型 SQL Server 用です。 としてサーバー名を入力する必要があります、 **完全修飾サーバー名の円記号のインスタンス名 (servername \instancename)**します。 リンクされたサービスの名前を *adfonpremsql*します。

###<a name="adf-linked-service-blob-store"></a>BLOB 用のリンクされたサービス
Azure Blob ストレージ アカウントのリンクされたサービスを作成するには、をクリックして、 **データ ストア** Azure クラシック ポータルで [ADF ランディング ページで、選択 *Azure ストレージ アカウント* と Azure Blob ストレージのアカウント キー、コンテナー名を入力します。 リンク サービスの名前を *adfds*します。

###<a name="adf-linked-service-azure-sql"></a>Azure SQL データベース用のリンクされたサービス
Azure SQL Database のリンクされたサービスを作成するには、をクリックして、 **データ ストア** Azure クラシック ポータルで [ADF ランディング ページで、選択 *Azure SQL* の資格情報を入力し、 *ユーザー名* と *パスワード* for Azure SQL Database。  *Username* として指定する必要があります *user@servername*します。   


##<a name="adf-tables"></a>データセットへのアクセス方法を指定するためのテーブルを定義して作成する

以下のスクリプトベースの手順に従って、データセットの構造、場所、可用性を指定するテーブルを作成します。 テーブルを定義するには、JSON ファイルを使用します。 これらのファイルの構造の詳細については、次を参照してください。 [データセット](data-factory-create-datasets.md)します。

> [AZURE.NOTE]  実行する必要があります、 `Add-AzureAccount` コマンドレットを実行する前に、 [New-azuredatafactorytable](https://msdn.microsoft.com/library/azure/dn835096.aspx) コマンドレットをコマンドの実行権限の Azure のサブスクリプションが選択されていることを確認します。 このコマンドレットのドキュメントについては、次を参照してください。 [Add-azureaccount](https://msdn.microsoft.com/library/azure/dn790372.aspx)します。

テーブル内の JSON ベースの定義では、次の名前が使用されます。

*  **テーブル名** サーバーは、内部設置型 SQL *nyctaxi_data*
*  **コンテナー名** アカウントは、Azure Blob ストレージに *containername*  

この ADF パイプラインには、次の 3 つのテーブル定義が必要です。

1. [オンプレミスの SQL テーブル](#adf-table-onprem-sql)
2. [BLOB テーブル ](#adf-table-blob-store)
3. [SQL Azure テーブル](#adf-table-azure-sql)

> [AZURE.NOTE]  次の手順では、Azure PowerShell を使用して、定義および ADF アクティビティを作成します。 これらのタスクは、Azure ポータルを使用しても行えます。 詳細については、「 [入力し、出力データセット](data-factory-use-onpremises-datasources.md#step-3-create-input-and-output-datasets)します。

###<a name="adf-table-onprem-sql"></a>オンプレミスの SQL テーブル

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
列名が指定されていないここでは、ここで、副選択列の名前にできます (に関する詳細情報がチェックをご覧ください、 [ADF ドキュメント](data-factory-copy-activity.md))。

テーブルの JSON 定義をファイルと呼ばれるコピー *onpremtabledef.json* ファイルし、既知の場所に保存 (ここであると見なされます *C:\temp\onpremtabledef.json*)。 次の Azure PowerShell コマンドレッドを使用して、ADF 内にテーブルを作成します。

    New-AzureDataFactoryTable -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp –File C:\temp\onpremtabledef.json


###<a name="adf-table-blob-store"></a>BLOB テーブル
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

テーブルの JSON 定義をファイルと呼ばれるコピー *bloboutputtabledef.json* ファイルし、既知の場所に保存 (ここであると見なされます *C:\temp\bloboutputtabledef.json*)。 次の Azure PowerShell コマンドレッドを使用して、ADF 内にテーブルを作成します。

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\bloboutputtabledef.json  

###<a name="adf-table-azure-sq"></a>SQL Azure テーブル
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

テーブルの JSON 定義をファイルと呼ばれるコピー *AzureSqlTable.json* ファイルし、既知の場所に保存 (ここであると見なされます *C:\temp\AzureSqlTable.json*)。 次の Azure PowerShell コマンドレッドを使用して、ADF 内にテーブルを作成します。

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\AzureSqlTable.json  

##<a name="adf-pipeline"></a>パイプラインを作成して定義する

次のスクリプトベースの手順に従って、パイプラインに属するアクティビティを指定し、パイプラインを作成します。 パイプラインのプロパティを定義するため、JSON ファイルを使用します。

* スクリプトが想定する、 **パイプライン名** は *AMLDSProcessPipeline*します。
* また、既定の実行時間 (12 am UTC) を使用して、ジョブが毎日実行されるようにパイプラインの周期性を設定していることにも注目してください。

> [AZURE.NOTE]  次の手順では、Azure PowerShell を使用して、定義および ADF パイプラインを作成します。 このタスクは、Azure ポータルを使用しても行えます。 詳細については、「 [の作成および実行パイプライン](data-factory-use-onpremises-datasources.md#step-4-create-and-run-a-pipeline)します。

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

ファイルに、パイプラインのこの JSON 定義と呼ばれるコピー *pipelinedef.json* ファイルし、既知の場所に保存 (ここであると見なされます *C:\temp\pipelinedef.json*)。 次の Azure PowerShell コマンドレッドを使用して、ADF 内にパイプラインを作成します。

    New-AzureDataFactoryPipeline  -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\pipelinedef.json

Azure クラシック ポータルで (図をクリックすると) ADF 上にパイプラインが次のように表示されることを確認します。

![](media/machine-learning-data-science-move-sql-azure-adf/DJP1kji.png)

##<a name="adf-pipeline-start"></a>パイプラインを開始する
これで、次のコマンドを使用してパイプラインを実行できます。

    Set-AzureDataFactoryPipelineActivePeriod -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp -StartDateTime startdateZ –EndDateTime enddateZ –Name AMLDSProcessPipeline

 *Startdate* と *enddate* パラメーターの値をパイプラインを実行する実際の日付に置き換えられる必要があります。

パイプラインを実行すると、BLOB に選択したコンテナー内に表示されるデータを確認することができます (1 日につき 1 ファイル)。

ADF が提供するデータを段階的にパイプ処理する機能をまだ活用していないことに注意してください。 ADF によって提供されるその他の機能を実行する方法の詳細については、次を参照してください。、 [ADF ドキュメント](http://azure.microsoft.com/services/data-factory/)します。


