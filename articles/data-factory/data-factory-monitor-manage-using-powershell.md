<properties 
    pageTitle="チュートリアル: コピー アクティビティがあるパイプラインを Azure PowerShell で作成する" 
    description="このチュートリアルでは、Azure PowerShell を使用して、コピー アクティビティがある Azure Data Factory パイプラインを作成します。" 
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
    ms.date="12/08/2015" 
    ms.author="spelluru"/>


# チュートリアル: Azure PowerShell を使用した Data Factory の作成と監視

> [AZURE.SELECTOR]
- [Tutorial Overview](data-factory-get-started.md)
- [Using Data Factory Editor](data-factory-get-started-using-editor.md)
- [Using PowerShell](data-factory-monitor-manage-using-powershell.md)
- [Using Visual Studio](data-factory-get-started-using-vs.md)



[[Adf--はじめに] Azure Data Factory を使ってみる][adf-get-started] チュートリアルで作成および使用して Azure data factory を監視する方法、 [Azure ポータルの ][azure-portal]します。 
このチュートリアルでは、Azure PowerShell コマンドレットを使用して Azure Data Factory を作成し、監視します。 このチュートリアルで作成するデータ ファクトリのパイプラインでは、Azure BLOB から Azure SQL Database にデータをコピーします。
> [AZURE.IMPORTANT] 
> 参照してください、 [チュートリアル概要](data-factory-get-started.md) 記事し、このチュートリアルを実行する前に、前提条件の手順を実行します。
>   
> この記事では、すべての Data Factory コマンドレットを取り上げているわけではありません。 参照してください [Data Factory コマンドレット リファレンス](https://msdn.microsoft.com/library/dn820234.aspx) Data Factory コマンドレットに関する包括的なドキュメントについてです。


## 前提条件

チュートリアルの「概要」トピックに記載されている前提条件とは別に、次をインストールする必要があります。

- **Azure PowerShell** 指示に従って [をインストールして、Azure PowerShell を構成する方法](../powershell-install-configure.md) 記事お使いのコンピューターに Azure PowerShell をインストールします。

Azure PowerShell を使用している場合 **バージョン < 1.0**, 、記載されているコマンドレットを使用する必要があります [ここ ][old-cmdlet-reference]します。また、Data Factory コマンドレットを使用する前に、次のコマンドを実行する必要があります。

1. Azure PowerShell を起動し、次のコマンドを実行します。 Azure PowerShell は、このチュートリアルが終わるまで開いたままにしておいてください。 Azure PowerShell を閉じて再度開いた場合は、これらのコマンドをもう一度実行する必要があります。
    1. 実行 **Add-azureaccount** ユーザー名と、Azure ポータルにサインインするために使用するパスワードを入力します。
    2. **Get-AzureSubscription** を実行して、このアカウントのサブスクリプションをすべて表示します。
    3. **Select-AzureSubscription** を実行して、使用するサブスクリプションを選択します。 このサブスクリプションは、Azure ポータルで使用したものと同じである必要があります。
4. Azure Data Factory コマンドレットは AzureResourceManager モードでのみ使用できるため、**Switch-AzureMode AzureResourceManager** を実行して、このモードに切り替えます。


## このチュートリアルの内容

次の表に、このチュートリアルの一部として実行する手順と、その説明を示します。

 手順| 説明
-----| -----------
 [手順 1: Azure Data Factory を作成します。](#CreateDataFactory)| この手順では、**ADFTutorialDataFactoryPSH** という名前の Azure Data Factory を作成します。
 [手順 2. リンクされたサービスを作成します。](#CreateLinkedServices)| この手順では、**StorageLinkedService** と **AzureSqlLinkedService** の 2 つのリンクされたサービスを作成します。StorageLinkedService は Azure ストレージを、AzureSqlLinkedService は Azure SQL Database を、それぞれ ADFTutorialDataFactoryPSH にリンクします。
 [手順 3: 入力し、出力データセット](#CreateInputAndOutputDataSets)| この手順では、2 つのデータセット (**EmpTableFromBlob** と **EmpSQLTable**) を定義します。これらは、次の手順で作成する ADFTutorialPipeline の**コピー アクティビティ**の入力テーブルおよび出力テーブルとして使用されます。
 [手順 4 を作成してパイプラインを実行](#CreateAndRunAPipeline)| この手順では、という名前のパイプラインを作成します **ADFTutorialPipeline** で、データ ファクトリ: **ADFTutorialDataFactoryPSH**します。.このパイプラインには、Azure BLOB から Azure データベース出力テーブルにデータをコピーする**コピー アクティビティ**があります。
 [手順 5. データセットとパイプラインを監視します。](#MonitorDataSetsAndPipeline)| この手順では、Azure PowerShell を使用してデータセットとパイプラインを監視します。

## <a name="CreateDataFactory"></a>手順 1: Azure Data Factory を作成します。

この手順では、Azure PowerShell を使用して、**ADFTutorialDataFactoryPSH** という名前の Azure Data Factory を作成します。

1. Azure PowerShell を起動し、次のコマンドを実行します。 Azure PowerShell は、このチュートリアルが終わるまで開いたままにしておいてください。 Azure PowerShell を閉じて再度開いた場合は、これらのコマンドをもう一度実行する必要があります。
    - 実行 **ログイン AzureRmAccount** ユーザー名と、Azure ポータルにサインインするために使用するパスワードを入力します。
    - **Get-AzureSubscription** を実行して、このアカウントのサブスクリプションをすべて表示します。
    - 実行 * * Select-azuresubscription <Name of the subscription>* * を使用するサブスクリプションを選択します。このサブスクリプションは、Azure ポータルで使用したものと同じである必要があります。
3. 次のコマンドを実行して、**ADFTutorialResourceGroup** という名前の Azure リソース グループを作成します。

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    このチュートリアルの一部の手順は、**ADFTutorialResourceGroup** という名前のリソース グループを使用することを前提としています。 異なるリソース グループを使用する場合は、このチュートリアルで ADFTutorialResourceGroup の代わりにそのリソース グループを使用する必要があります。
4. **New-AzureRmDataFactory** コマンドレットを実行し、**ADFTutorialDataFactoryPSH** という名前の Data Factory を作成します。

        New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH –Location "West US"

    Azure Data Factor の名前はグローバルに一意にする必要があります。 **""ADFTutorialDataFactoryPSH" という名前のデータ ファクトリは使用できません"** というエラーが発生した場合は、名前を変更します (yournameADFTutorialDataFactoryPSH など)。 このチュートリアルの手順の実行中に、この名前を ADFTutorialFactoryPSH の代わりに使用します。 参照してください [Data Factory - 名前付け規則](data-factory-naming-rules.md) Data Factory アーティファクトの名前付け規則についてのトピックです。
    > [AZURE.NOTE] データ ファクトリの名前は今後、DNS 名として登録される可能性があるため、一般ユーザーに表示される場合があります。

## <a name="CreateLinkedServices"></a>手順 2. リンクされたサービスを作成します。

リンクされたサービスは、データ ストアまたはコンピューティング サービスを Azure Data Factory にリンクします。 データ ストアには、Azure ストレージ、Azure SQL Database、またはオンプレミスの SQL Server データベースを指定できます。これらのデータ ストアには、Data Factory パイプラインの入力データが含まれているか、出力データが格納されています。 コンピューティング サービスは、入力データを処理し、出力データを生成するサービスです。

この手順では、**StorageLinkedService** と **AzureSqlLinkedService** の 2 つのリンクされたサービスを作成します。 リンクされたサービス StorageLinkedService は Azure ストレージ アカウントを、AzureSqlLinkedService は Azure SQL Database を **ADFTutorialDataFactoryPSH** という Data Factory にリンクします。 このチュートリアルの後半で、StorageLinkedService 内の BLOB コンテナーから AzureSqlLinkedService 内の SQL テーブルにデータをコピーするパイプラインを作成します。

### Azure ストレージ アカウント用にリンクされたサービスを作成する

1.  次の内容で **C:\ADFGetStartedPSH** に **StorageLinkedService.json** という名前の JSON ファイルを作成します。 ADFGetStartedPSH フォルダーがない場合は作成します。

            {
                "name": "StorageLinkedService",
                "properties": {
                    "type": "AzureStorage",
                    "typeProperties": {
                        "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
                    }
                }
            }

    **accountname** をストレージ アカウントの名前に、**accountkey** を Azure ストレージ アカウントのキーに置き換えます。
2.  **Azure PowerShell** で **ADFGetStartedPSH** フォルダーに切り替えます。
3.  **New-AzureRmDataFactoryLinkedService** コマンドレットを使用して、リンクされたサービスを作成できます。 このコマンドレットと、このチュートリアルで使用する他の Data Factory コマンドレットでは、**ResourceGroupName** および **DataFactoryName** パラメーターの値を渡す必要があります。 または、**Get-AzureRmDataFactory** を使用して DataFactory オブジェクトを取得すると、コマンドレットを実行するたびに ResourceGroupName と DataFactoryName を入力しなくてもオブジェクトを渡すことができます。 **Get-AzureRmDataFactory** コマンドレットの出力を変数 **$df** に割り当てるには、次のコマンドを実行します。

        $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH

4.  これで、**New-AzureRmDataFactoryLinkedService** コマンドレットを実行して、リンクされた **StorageLinkedService** サービスを作成できます。

        New-AzureRmDataFactoryLinkedService $df -File .\StorageLinkedService.json

    **Get-AzureRmDataFactory** コマンドレットを実行して出力を **$df** 変数に割り当てていない場合、ResourceGroupName および DataFactoryName パラメーターの値を次のように指定する必要があります。

        New-AzureRmDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName ADFTutorialDataFactoryPSH -File .\StorageLinkedService.json

    チュートリアルの途中で Azure PowerShell を閉じた場合、次に Azure PowerShell を起動したときに、Get-AzureRmDataFactory コマンドレットを実行してチュートリアルを完了する必要があります。

### Azure SQL Database 用にリンクされたサービスを作成する

1.  次の内容で AzureSqlLinkedService.json という名前の JSON ファイルを作成します。

            {
                "name": "AzureSqlLinkedService",
                "properties": {
                    "type": "AzureSqlDatabase",
                    "typeProperties": {
                        "connectionString": "Server=tcp:<server>.database.windows.net,1433;Database=<databasename>;User ID=user@server;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
                    }
                }
            }

    置換 **servername**, 、**databasename**, 、**username@servername**, 、および **パスワード** 、Azure SQL サーバー、データベース、ユーザー アカウントとパスワードの名前を持つ。

2.  次のコマンドを実行して、リンクされたサービスを作成します。

        New-AzureRmDataFactoryLinkedService $df -File .\AzureSqlLinkedService.json

    Azure SQL Server で **[Azure サービスへのアクセスを許可する]** の設定がオンになっていることを確認します。 この設定を確認してオンにするには、次の手順を実行します。

    1. 左側にある **[参照]** ハブをクリックし、**[SQL サーバー]** をクリックします。
    2. お使いのサーバーを選択し、[SQL Server] ブレードで **[設定]** をクリックします。
    3. **[設定]** ブレードで **[ファイアウォール]** をクリックします。
    4. **[ファイアウォールの設定]** ブレードの **[Azure サービスへのアクセスを許可する]** で **[オン]** をクリックします。
    5. 左側の **[アクティブ]** ハブをクリックして、元の **[Data Factory]** ブレードに切り替えます。


## <a name="CreateInputAndOutputDataSets"></a>手順 3: で、入出力テーブルの作成

前の手順では、**StorageLinkedService** と **AzureSqlLinkedService** というリンクされたサービスを作成し、Azure Storage アカウントと Azure SQL Database を **ADFTutorialDataFactoryPSH** という Data Factory にリンクしました。 このステップでは、次の手順で作成するパイプラインのコピー アクティビティ用に入力データと出力データを表すデータセットを作成します。

テーブルとは四角形のデータセットで、現在サポートされている唯一の種類のデータセットです。 このチュートリアルの入力テーブルは StorageLinkedService がポイントする Azure Storage 内の BLOB コンテナーを参照し、出力テーブルは AzureSqlLinkedService がポイントする Azure SQL Database 内の SQL テーブルを参照します。

### Azure BLOB ストレージと Azure SQL Database をチュートリアル用に準備する

チュートリアルを終了した場合、この手順をスキップ [[adf--はじめに] Azure Data Factory を使ってみる][adf-get-started] 記事です。

このチュートリアルで使用する Azure BLOB ストレージと Azure SQL Database を準備するには、次の手順を実行する必要があります。

* **StorageLinkedService** がポイントする Azure BLOB ストレージ内に **adftutorial** という名前の BLOB コンテナーを作成します。
* **emp.txt** という名前のテキスト ファイルを作成し、BLOB として **adftutorial** コンテナーにアップロードします。
* **AzureSqlLinkedService** がポイントする Azure SQL Database 内に **emp** という名前のテーブルを作成します。

1. メモ帳を起動し、次のテキストを貼り付け、**emp.txt** という名前でハード ドライブの **C:\ADFGetStartedPSH** フォルダーに保存します。

        John Doe
        加藤さん、Doe

2. ツールを使用して [Azure ストレージ エクスプ ローラー](https://azurestorageexplorer.codeplex.com/) を作成する、 **adftutorial** コンテナーにアップロードして、 **emp.txt** コンテナーにファイルです。

    ![Azure Storage Explorer][image-data-factory-get-started-storage-explorer]
3. 次の SQL スクリプトを使用して、**emp** テーブルを Azure SQL Database に作成します。

        CREATE TABLE dbo.emp 
        (
            テーブル IDENTITY(1,1) NOT NULL、int 型を ID します。
            FirstName varchar (50)
            LastName varchar (50)
        )
        GO

        クラスター化インデックス IX_emp_ID の作成に dbo.emp (ID)。

    お使いのコンピューターにインストールされている SQL Server 2014 があるかどうか: からの指示に従って [手順 2: の Azure SQL Database の管理 SQL Server Management Studio ][sql-management-studio] 資料、Azure SQL サーバーに接続し、SQL スクリプトを実行します。

    お使いのコンピューターにインストールされている Visual Studio 2013 があるかどうか: Azure ポータルで ([http://portal.azure.com](http://portal.sazure.com)), 、] をクリックして **参照** ハブで、左クリック **の SQL server**, 、データベースを選択し、クリックして **Visual Studio で開く** 、Azure SQL サーバーに接続し、スクリプトを実行するにはツールバーにボタンをクリックします。 クライアントから Azure SQL サーバーへのアクセスが許可されていない場合は、コンピューター (IP アドレス) からのアクセスを許可するように、Azure SQL Server のファイアウォールを構成する必要があります。 Azure SQL サーバーのファイアウォールを構成する手順については、上の記事を参照してください。

### 入力テーブルの作成

テーブルとは四角形のデータセットで、スキーマを持っています。 この手順では、リンクされたサービス **StorageLinkedService** が表す Azure Storage 内の BLOB コンテナーをポイントする **EmpBlobTable** という名前のテーブルを作成します。
 この BLOB コンテナー (**adftutorial**) には、**emp.txt** ファイルの入力データが含まれています。

1.  以下の内容を記述した **EmpBlobTable.json** という名前の JSON ファイルを **C:\ADFGetStartedPSH** フォルダー内に作成します。

            {
              "name": "EmpTableFromBlob",
              "properties": {
                "structure": [
                  {
                    "name": "FirstName",
                    "type": "String"
                  },
                  {
                    "name": "LastName",
                    "type": "String"
                  }
                ],
                "type": "AzureBlob",
                "linkedServiceName": "StorageLinkedService",
                "typeProperties": {
                  "fileName": "emp.txt",
                  "folderPath": "adftutorial/",
                  "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                  }
                },
                "external": true,
                "availability": {
                  "frequency": "Hour",
                  "interval": 1
                }
              }
            }

    以下の点に注意してください。

    - dataset の **type** は **AzureBlob** に設定されています。
    - **linkedServiceName** は **StorageLinkedService** に設定されています。
    - **folderPath** が **adftutorial** コンテナーに設定されています。
    - **fileName** は **emp.txt** に設定されています。 BLOB の名前を指定しない場合、コンテナー内のすべての BLOB からのデータが入力データと見なされます。
    - format の **type** が **TextFormat** に設定されています。
    - テキスト ファイル内に 2 つのフィールド (**FirstName** と **LastName**) があり、コンマ (**columnDelimiter**) で区切られています。
    - **availability** が **hourly** に設定されています (**frequency** は **hour**、**interval** は **1** に設定されています)。そのため、Data Factory サービスでは、指定した BLOB コンテナー (**adftutorial**) のルート フォルダーにある入力データを 1 時間ごとに検索します。

    **入力****テーブル**に **fileName** を指定しない場合、入力フォルダー (**folderPath**) のすべてのファイルまたは BLOB が入力と見なされます。 JSON で fileName を指定した場合は、指定されたファイル/BLOB のみが入力と見なされます。 サンプル ファイルを参照してください、 [チュートリアル ][adf-tutorial] 例です。

    指定しない場合、 **fileName** の **出力テーブル**, 、生成されるファイルに、 **folderPath** 次の形式で名前が: データ。<Guid\>します。txt (例: 付けられます-93ff-4c6f-b3be-f69616f1df7a.txt。)。

    **folderPath** と **fileName** を **SliceStart** 時刻に基づいて動的に設定するには、**partitionedBy** プロパティを使用します。 次の例では、folderPath に SliceStart (処理されるスライスの開始時刻) の年、月、日を使用し、fileName に SliceStart の時間を使用します。 たとえば、スライスが 2014-10-20T08:00:00 に生成されている場合、folderName は wikidatagateway/wikisampledataout/2014/10/20 に設定され、fileName は 08.csv に設定されます。

            "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
            "fileName": "{Hour}.csv",
            "partitionedBy": 
            [
                { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
                { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
                { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
            ],

    参照してください [JSON スクリプティング リファレンス](http://go.microsoft.com/fwlink/?LinkId=516971) JSON プロパティの詳細について。

2.  次のコマンドを実行して、Data Factory データセットを作成します。

        New-AzureRmDataFactoryDataset $df -File .\EmpBlobTable.json


### 出力テーブルの作成

手順のこの部分では、リンクされたサービス **AzureSqlLinkedService** が表す Azure SQL Database 内の SQL テーブル (**emp**) をポイントする **EmpSQLTable** という名前の出力テーブルを作成します。 パイプラインで入力 BLOB から **emp** テーブルにデータがコピーされます。

1.  以下の内容を記述した **EmpSQLTable.json** という名前の JSON ファイルを **C:\ADFGetStartedPSH** フォルダー内に作成します。

            {
              "name": "EmpSQLTable",
              "properties": {
                "structure": [
                  {
                    "name": "FirstName",
                    "type": "String"
                  },
                  {
                    "name": "LastName",
                    "type": "String"
                  }
                ],
                "type": "AzureSqlTable",
                "linkedServiceName": "AzureSqlLinkedService",
                "typeProperties": {
                  "tableName": "emp"
                },
                "availability": {
                  "frequency": "Hour",
                  "interval": 1
                }
              }
            }

     以下の点に注意してください。

    * dataset の **type** が **AzureSqlTable** に設定されています。
    * **linkedServiceName** は **AzureSqlLinkedService** に設定されています。
    * **tablename** は **emp** に設定されています。
    * データベース内の emp テーブルに 3 つの列 (**ID**、**FirstName**、**LastName**) がありますが、ID は識別子の列であるため、ここでは **FirstName** と **LastName** のみを指定する必要があります。
    * **availability** が **hourly** に設定されています (**frequency** は **hour**、**interval** は **1** に設定されています)。 Data Factory サービスは、Azure SQL Database 内の **emp** テーブルに 1 時間ごとに出力データ スライスを生成します。

2.  次のコマンドを実行して、Data Factory データセットを作成します。

        New-AzureRmDataFactoryDataset $df -File .\EmpSQLTable.json



## <a name="CreateAndRunAPipeline"></a>手順 4 を作成してパイプラインを実行

この手順では、**EmpTableFromBlob** を入力、**EmpSQLTable** を出力として使用し、**コピー アクティビティ**を持つパイプラインを作成します。

1.  以下の内容を記述した **ADFTutorialPipeline.json** という名前の JSON ファイルを **C:\ADFGetStartedPSH** フォルダー内に作成します。

             {
              "name": "ADFTutorialPipeline",
              "properties": {
                "description": "Copy data from a blob to Azure SQL table",
                "activities": [
                  {
                    "name": "CopyFromBlobToSQL",
                    "description": "Push Regional Effectiveness Campaign data to Azure SQL database",
                    "type": "Copy",
                    "inputs": [
                      {
                        "name": "EmpTableFromBlob"
                      }
                    ],
                    "outputs": [
                      {
                        "name": "EmpSQLTable"
                      }
                    ],
                    "typeProperties": {
                      "source": {
                        "type": "BlobSource"
                      },
                      "sink": {
                        "type": "SqlSink"
                      }
                    },
                    "Policy": {
                      "concurrency": 1,
                      "executionPriorityOrder": "NewestFirst",
                      "style": "StartOfInterval",
                      "retry": 0,
                      "timeout": "01:00:00"
                    }
                  }
                ],
                "start": "2015-12-09T00:00:00Z",
                "end": "2015-12-10T00:00:00Z",
                "isPaused": false
              }
            }

    以下の点に注意してください。

    - activities セクションに、**type** が **Copy** に設定されたアクティビティが 1 つだけあります。
    - アクティビティの入力は **EmpTableFromBlob** に設定され、アクティビティの出力は **EmpSQLTable** に設定されています。
    - **transformation** セクションでは、ソースの種類として **BlobSource** が指定され、シンクの種類として **SqlSink** が指定されています。

    **start** プロパティの値を現在の日付に置き換え、**end** プロパティの値を翌日の日付に置き換えます。 どちらも start し、end の datetimes がである必要があります [ISO 形式](http://en.wikipedia.org/wiki/ISO_8601)します。 (例: 2014-10-14T16:32:41Z)。 **end** の時刻は省略可能ですが、このチュートリアルでは使用します。

    **end** プロパティの値を指定しない場合、"**start + 48 時間**" として計算されます。 パイプラインを無期限に実行する場合は、**9/9/9999** を **end** プロパティの値として指定します。

    上記の例では、各データ スライスが 1 時間ごとに生成されるため、データ スライスは 24 個になります。

    参照してください [JSON スクリプティング リファレンス](http://go.microsoft.com/fwlink/?LinkId=516971) JSON プロパティの詳細について。
2.  次のコマンドを実行して、Data Factory テーブルを作成します。

        New-AzureRmDataFactoryPipeline $df -File .\ADFTutorialPipeline.json


**お疲れさまでした。**これで、Azure Data Factory、リンクされたサービス、テーブル、およびパイプラインの作成と、パイプラインのスケジュール設定が完了しました。

## <a name="MonitorDataSetsAndPipeline"></a>手順 5. データセットとパイプラインを監視します。

このステップでは、Azure PowerShell を使用して、Azure Data Factory の状況を監視します。

1.  **Get-AzureRmDataFactory** を実行して変数 $df に出力を割り当てます。

        $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH

2.  **Get-AzureRmDataFactorySlice** を実行し、**EmpSQLTable** のすべてのスライスの詳細を表示します。これは、パイプラインの出力テーブルです。

        Get-AzureRmDataFactorySlice $df -DatasetName EmpSQLTable -StartDateTime 2015-03-03T00:00:00

    **StartDateTime** パラメーターで、年、月、日の部分を現在の年、月、日に置き換えます。 この値は、JSON パイプラインの **Start** の値と一致します。

    現在の日付の 12 AM から次の日の 12 AM までの 1 時間ごとに 1 つずつ、合計 24 個のスライスが表示されます。

    **最初のスライス:**

        ResourceGroupName : ADFTutorialResourceGroup
        DataFactoryName   : ADFTutorialDataFactoryPSH
        TableName         : EmpSQLTable
        Start             : 3/3/2015 12:00:00 AM
        End               : 3/3/2015 1:00:00 AM
        RetryCount        : 0
        Status            : PendingExecution
        LatencyStatus     :
        LongRetryCount    : 0

    **最後のスライス:**

        ResourceGroupName : ADFTutorialResourceGroup
        DataFactoryName   : ADFTutorialDataFactoryPSH
        TableName         : EmpSQLTable
        Start             : 3/4/2015 11:00:00 PM
        End               : 3/4/2015 12:00:00 AM
        RetryCount        : 0
        Status            : PendingExecution
        LatencyStatus     : 
        LongRetryCount    : 0

3.  **Get-AzureRmDataFactoryRun** を実行して、**特定**のスライスに関するアクティビティの実行の詳細を取得します。 上記の出力のスライスの **Start** 時間と一致するように、**StartDateTime** パラメーターの値を変更します。 値、 **StartDateTime** である必要があります [ISO 形式](http://en.wikipedia.org/wiki/ISO_8601)します。 例: 2014-03-03T22:00:00Z。

        Get-AzureRmDataFactoryRun $df -DatasetName EmpSQLTable -StartDateTime 2015-03-03T22:00:00

    次のような出力が表示されます。

        Id                  : 3404c187-c889-4f88-933b-2a2f5cd84e90_635614488000000000_635614524000000000_EmpSQLTable
        ResourceGroupName   : ADFTutorialResourceGroup
        DataFactoryName     : ADFTutorialDataFactoryPSH
        TableName           : EmpSQLTable
        ProcessingStartTime : 3/3/2015 11:03:28 PM
        ProcessingEndTime   : 3/3/2015 11:04:36 PM
        PercentComplete     : 100
        DataSliceStart      : 3/8/2015 10:00:00 PM
        DataSliceEnd        : 3/8/2015 11:00:00 PM
        Status              : Succeeded
        Timestamp           : 3/8/2015 11:03:28 PM
        RetryAttempt        : 0
        Properties          : {}
        ErrorMessage        :
        ActivityName        : CopyFromBlobToSQL
        PipelineName        : ADFTutorialPipeline
        Type                : Copy


参照してください [Data Factory コマンドレット リファレンス ][cmdlet-reference] Data Factory コマンドレットに関する包括的なドキュメントについてです。




[adf-tutorial]: data-factory-tutorial.md 
[use-custom-activities]: data-factory-use-custom-activities.md 
[troubleshoot]: data-factory-troubleshoot.md 
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908 
[cmdlet-reference]: https://msdn.microsoft.com/library/azure/dn820234.aspx 
[old-cmdlet-reference]: https://msdn.microsoft.com/library/azure/dn820234(v=azure.98).aspx 
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/ 
[data-factory-create-storage]: ../storage-create-storage-account.md 
[adf-get-started]: data-factory-get-started.md 
[azure-portal]: http://portal.azure.com 
[download-azure-powershell]: ../powershell-install-configure.md 
[data-factory-introduction]: data-factory-introduction.md 
[image-data-factory-get-started-storage-explorer]: ./media/data-factory-monitor-manage-using-powershell/getstarted-storage-explorer.png 
[sql-management-studio]: ../sql-database-manage-azure-ssms.md#Step2 

