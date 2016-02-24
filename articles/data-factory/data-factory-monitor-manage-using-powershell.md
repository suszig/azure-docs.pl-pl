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
- [チュートリアルの概要](data-factory-get-started.md)
- [Data Factory エディターを使用します。](data-factory-get-started-using-editor.md)
- [PowerShell の使用](data-factory-monitor-manage-using-powershell.md)
- [Visual Studio を使用](data-factory-get-started-using-vs.md)


 [Azure Data Factory を使ってみる][adf-get-started] チュートリアルで作成および使用して Azure data factory を監視する方法、 [Azure ポータル][azure-portal]します。 
このチュートリアルでは、Azure PowerShell コマンドレットを使用して Azure Data Factory を作成し、監視します。 このチュートリアルで作成するデータ ファクトリのパイプラインでは、Azure BLOB から Azure SQL Database にデータをコピーします。  

> [AZURE.IMPORTANT] 
> 参照してください、 [チュートリアル概要](data-factory-get-started.md) 記事し、このチュートリアルを実行する前に、前提条件の手順を実行します。
>   
> この記事では、すべての Data Factory コマンドレットを取り上げているわけではありません。 参照してください [Data Factory コマンドレット リファレンス](https://msdn.microsoft.com/library/dn820234.aspx) Data Factory コマンドレットに関する包括的なドキュメントについてです。
  

##前提条件
チュートリアルの「概要」トピックに記載されている前提条件とは別に、次をインストールする必要があります。

- **Azure PowerShell**します。 指示に従って [をインストールして、Azure PowerShell を構成する方法](../powershell-install-configure.md) 記事お使いのコンピューターに Azure PowerShell をインストールします。

Azure PowerShell を使用している場合 **バージョン < 1.0**, 、記載されているコマンドレットを使用する必要があります [ここ][old-cmdlet-reference]します。 また、Data Factory コマンドレットを使用する前に、次のコマンドを実行する必要があります。 

1. Azure PowerShell を起動し、次のコマンドを実行します。 Azure PowerShell は、このチュートリアルが終わるまで開いたままにしておいてください。 Azure PowerShell を閉じて再度開いた場合は、これらのコマンドをもう一度実行する必要があります。
    1. 実行 **Add-azureaccount** ユーザー名と、Azure ポータルにサインインするために使用するパスワードを入力します。
    2. 実行 **Get-azuresubscription** このアカウントのすべてのサブスクリプションを表示します。
    3. 実行 **Select-azuresubscription** を使用するサブスクリプションを選択します。 このサブスクリプションは、Azure ポータルで使用したものと同じである必要があります。
4. Azure Data Factory コマンドレットはこのモードで利用可能な AzureResourceManager モードに切り替えます。 **Switch-azuremode AzureResourceManager**します。
  

##このチュートリアルの内容
次の表に、このチュートリアルの一部として実行する手順と、その説明を示します。 

手順 | 説明
-----| -----------
[手順 1. Azure Data Factory を作成する](#CreateDataFactory) | この手順では、という名前の Azure data factory を作成します **ADFTutorialDataFactoryPSH**します。 
[手順 2. リンク サービスを作成する](#CreateLinkedServices) | この手順では、2 つのリンクされたサービスを作成します: **StorageLinkedService** と **AzureSqlLinkedService**します。 StorageLinkedService は Azure Storage を、AzureSqlLinkedService は Azure SQL Database を、それぞれ ADFTutorialDataFactoryPSH にリンクします。
[手順 3. 入力データセットと出力データセットを作成する](#CreateInputAndOutputDataSets) | この手順では、2 つのデータセットを定義します (**EmpTableFromBlob** と **EmpSQLTable**) の入力呼び出し力テーブルとして使用される、 **コピー アクティビティ** は、次の手順で作成する ADFTutorialPipeline のです。
[手順 4. パイプラインを作成して実行する](#CreateAndRunAPipeline) | この手順では、という名前のパイプラインを作成します **ADFTutorialPipeline** で、データ ファクトリ: **ADFTutorialDataFactoryPSH**します。 . パイプラインには、 **コピー アクティビティ** 、Azure からデータをコピーする Azure データベース出力テーブルに blob です。
[手順 5. データセットとパイプラインを監視する](#MonitorDataSetsAndPipeline) | この手順では、Azure PowerShell を使用してデータセットとパイプラインを監視します。

## <a name="CreateDataFactory"></a>手順 1. Azure Data Factory を作成する
この手順で、という名前の Azure データ ファクトリを作成する Azure PowerShell を使用して **ADFTutorialDataFactoryPSH**します。

1. Azure PowerShell を起動し、次のコマンドを実行します。 Azure PowerShell は、このチュートリアルが終わるまで開いたままにしておいてください。 Azure PowerShell を閉じて再度開いた場合は、これらのコマンドをもう一度実行する必要があります。
    - 実行 **ログイン AzureRmAccount** ユーザー名と、Azure ポータルにサインインするために使用するパスワードを入力します。  
    - 実行 **Get-azuresubscription** このアカウントのすべてのサブスクリプションを表示します。
    - 実行 **Select-azuresubscription <Name of the subscription>** を使用するサブスクリプションを選択します。 このサブスクリプションは、Azure ポータルで使用したものと同じである必要があります。
3. という名前の Azure リソース グループを作成します。 **ADFTutorialResourceGroup** 次のコマンドを実行しています。
   
        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    このチュートリアルの手順の一部はという名前のリソース グループを使用することを前提としています **ADFTutorialResourceGroup**します。 異なるリソース グループを使用する場合は、このチュートリアルで ADFTutorialResourceGroup の代わりにそのリソース グループを使用する必要があります。 
4. 実行、 **新規 AzureRmDataFactory** という名前の data factory を作成するコマンドレット: **ADFTutorialDataFactoryPSH**します。  

        New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH –Location "West US"

    Azure Data Factor の名前はグローバルに一意にする必要があります。 エラーが発生する場合: **データ ファクトリ名"ADFTutorialDataFactoryPSH"は使用できません**, 、(yournameADFTutorialDataFactoryPSH など) の名前を変更します。 このチュートリアルの手順の実行中に、この名前を ADFTutorialFactoryPSH の代わりに使用します。 参照してください [Data Factory - 名前付け規則](data-factory-naming-rules.md) Data Factory アーティファクトの名前付け規則についてのトピックです。

    > [AZURE.NOTE] データ ファクトリの名前は、今後しないためにパブリックに表示する DNS 名として登録することがあります。

## <a name="CreateLinkedServices"></a>手順 2. リンク サービスを作成する
リンクされたサービスは、データ ストアまたはコンピューティング サービスを Azure Data Factory にリンクします。 データ ストアには、Azure Storage、Azure SQL Database、またはオンプレミスの SQL Server データベースを指定できます。これらのデータ ストアには、Data Factory パイプラインの入力データが含まれているか、出力データが格納されています。 コンピューティング サービスは、入力データを処理し、出力データを生成するサービスです。 

この手順では、2 つのリンクされたサービスを作成します: **StorageLinkedService** と **AzureSqlLinkedService**します。 StorageLinkedService リンク サービスを Azure ストレージ アカウントを AzureSqlLinkedService は Azure SQL データベースをデータ ファクトリにリンク: **ADFTutorialDataFactoryPSH**します。 このチュートリアルの後半で、StorageLinkedService 内の BLOB コンテナーから AzureSqlLinkedService 内の SQL テーブルにデータをコピーするパイプラインを作成します。

### Azure ストレージ アカウント用にリンクされたサービスを作成する
1.  という名前の JSON ファイルを作成する **StorageLinkedService.json** で、 **C:\ADFGetStartedPSH** 以下の内容。 ADFGetStartedPSH フォルダーがない場合は作成します。

            {
                "name": "StorageLinkedService",
                "properties": {
                    "type": "AzureStorage",
                    "typeProperties": {
                        "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
                    }
                }
            }

    置換 **accountname** 、ストレージ アカウントの名前と **accountkey** Azure ストレージ アカウントのキーを使用しています。
2.   **Azure PowerShell**, に切り替えて、 **ADFGetStartedPSH** フォルダーです。 
3.  使用することができます、 **新規 AzureRmDataFactoryLinkedService** コマンドレットをリンクされたサービスを作成します。 このコマンドレットでは、このチュートリアルで使用するその他の Data Factory コマンドレットには、値を渡す、 **ResourceGroupName** と **DataFactoryName** パラメーター。 また、使用することができます **Get AzureRmDataFactory** DataFactory オブジェクトを取得し、たびに ResourceGroupName と DataFactoryName を入力しなくても、オブジェクトを渡すには、コマンドレットを実行します。 出力を割り当てるには、次のコマンドを実行、 **Get AzureRmDataFactory** コマンドレットでは、変数を: **$df**します。 

        $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH

4.  これで、実行、 **新規 AzureRmDataFactoryLinkedService** コマンドレットをリンクされたサービスを作成します。 **StorageLinkedService**します。 

        New-AzureRmDataFactoryLinkedService $df -File .\StorageLinkedService.json

    実行している場合、 **Get AzureRmDataFactory** コマンドレットに出力 **$df** 変数しなければならなくなる次のように、ResourceGroupName および DataFactoryName パラメーターの値を指定します。   
        
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

    いることを確認、 **Azure サービスへのアクセスを許可する** 、Azure SQL サーバーで設定がオンにします。 この設定を確認してオンにするには、次の手順を実行します。

    1. クリックして **参照** ハブをクリックして左 **の SQL server**します。
    2. サーバーを選択し、クリックして **設定** SQL SERVER] ブレードでします。
    3.  **設定** ブレードで、をクリックして **ファイアウォール**します。
    4.  **ファイアウォール設定** ブレードで、をクリックして **ON** の **Azure サービスへのアクセスを許可する**です。
    5. クリックして **ACTIVE** に切り替えるには、左側のハブ、 **Data Factory** ブレードです。
    

## <a name="CreateInputAndOutputDataSets"></a>手順 3. 入力テーブルと出力テーブルを作成する

リンクされたサービスを作成した前の手順で **StorageLinkedService** と **AzureSqlLinkedService** Azure ストレージ アカウントと Azure SQL データベースをデータ ファクトリにリンクする: **ADFTutorialDataFactoryPSH**します。 このステップでは、次の手順で作成するパイプラインのコピー アクティビティ用に入力データと出力データを表すデータセットを作成します。 

テーブルとは四角形のデータセットで、現在サポートされている唯一の種類のデータセットです。 このチュートリアルの入力テーブルは StorageLinkedService がポイントする Azure Storage 内の BLOB コンテナーを参照し、出力テーブルは AzureSqlLinkedService がポイントする Azure SQL Database 内の SQL テーブルを参照します。  

### Azure BLOB ストレージと Azure SQL Database をチュートリアル用に準備する
チュートリアルを終了した場合、この手順をスキップ [Azure Data Factory を使ってみる][adf-get-started] 記事です。 

このチュートリアルで使用する Azure BLOB ストレージと Azure SQL Database を準備するには、次の手順を実行する必要があります。 
 
* という名前の blob コンテナーを作成する **adftutorial** 、Azure blob ストレージを **StorageLinkedService** をポイントします。 
* 作成し、テキスト ファイルをアップロード **emp.txt**, を blob として、 **adftutorial** コンテナーです。 
* という名前のテーブルを作成する **emp** 、Azure SQL Database で Azure sql データベースにある **AzureSqlLinkedService** をポイントします。


1. メモ帳を起動し、次のテキストを貼り付け、として保存 **emp.txt** に **C:\ADFGetStartedPSH** 、ハード ドライブ上のフォルダーです。 

        John, Doe
        Jane, Doe
                
2. ツールを使用して [Azure ストレージ エクスプ ローラー](https://azurestorageexplorer.codeplex.com/) を作成する、 **adftutorial** コンテナーにアップロードして、 **emp.txt** コンテナーにファイルです。

    ![Azure ストレージ エクスプローラー][image-data-factory-get-started-storage-explorer]
3. 作成する次の SQL スクリプトを使用して、 **emp** Azure SQL データベース内のテーブルです。  


        CREATE TABLE dbo.emp 
        (
            ID int IDENTITY(1,1) NOT NULL,
            FirstName varchar(50),
            LastName varchar(50),
        )
        GO

        CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID); 

    If you have SQL Server 2014 installed on your computer: follow instructions from [Step 2: Connect to SQL Database of the Managing Azure SQL Database using SQL Server Management Studio][sql-management-studio] article to connect to your Azure SQL server and run the SQL script.

    If you have Visual Studio 2013 installed on your computer: in the Azure Portal ([http://portal.azure.com](http://portal.sazure.com)), click **BROWSE** hub on the left, click **SQL servers**, select your database, and click **Open in Visual Studio** button on toolbar to connect to your Azure SQL server and run the script. If your client is not allowed to access the Azure SQL server, you will need to configure firewall for your Azure SQL server to allow access from your machine (IP Address). See the article above for steps to configure the firewall for your Azure SQL server.
        
### 入力テーブルの作成 
テーブルとは四角形のデータセットで、スキーマを持っています。 この手順では、という名前のテーブルを作成します **EmpBlobTable** で表される Azure ストレージ内の blob コンテナーを指す、 **StorageLinkedService** のリンクされたサービスです。 この blob コンテナー (**adftutorial**)、ファイル内の入力データが含まれています: **emp.txt**します。 

1.  という名前の JSON ファイルを作成する **EmpBlobTable.json** で、 **C:\ADFGetStartedPSH** フォルダー以下の内容。

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
    
    - データセット **型** に設定されている **AzureBlob**します。
    - **linkedServiceName** に設定されている **StorageLinkedService**します。 
    - **folderPath** に設定されている、 **adftutorial** コンテナーです。 
    - **ファイル名** に設定されている **emp.txt**します。 BLOB の名前を指定しない場合、コンテナー内のすべての BLOB からのデータが入力データと見なされます。  
    - 形式 **型** に設定されている **TextFormat**
    - テキスト ファイルに 2 つのフィールドが **FirstName** と **LastName** 、コンマで区切られて (**columnDelimiter**) 
    -  **可用性** に設定されている **1 時間ごと** (**頻度** に設定されている **時間** と **間隔** に設定されている **1** ) であるため、Data Factory サービスは入力データの 1 時間ごと、ルート フォルダー検索 blob コンテナーに、(**adftutorial**) に指定しました。

    指定しない場合、 **fileName** の **入力** **テーブル**, 、入力フォルダーからすべてのファイルまたは blob (**folderPath**) の入力と見なされます。 JSON で fileName を指定した場合は、指定されたファイル/BLOB のみが入力と見なされます。 サンプル ファイルを参照してください、 [チュートリアル][adf-tutorial] 例です。
 
    指定しない場合、 **ファイル名** の **出力テーブル**, 、生成されるファイルに、 **folderPath** 次の形式で名前が: データ。 < Guid\ > .txt (例: 付けられます-93ff-4c6f-b3be-f69616f1df7a.txt。)。

    設定する **folderPath** と **fileName** に基づいて動的に、 **SliceStart** 今度は、使用して、 **partitionedBy** プロパティです。 次の例では、folderPath に SliceStart (処理されるスライスの開始時刻) の年、月、日を使用し、fileName に SliceStart の時間を使用します。 たとえば、スライスが 2014-10-20T08:00:00 に生成されている場合、folderName は wikidatagateway/wikisampledataout/2014/10/20 に設定され、fileName は 08.csv に設定されます。 

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
という名前の出力テーブルを作成する手順では、 **EmpSQLTable** SQL テーブルを指す (**emp**) で表される Azure SQL データベースで、 **AzureSqlLinkedService** のリンクされたサービスです。 パイプラインは、入力 blob からデータをコピー、 **emp** テーブルです。 

1.  という名前の JSON ファイルを作成する **EmpSQLTable.json** で、 **C:\ADFGetStartedPSH** フォルダー以下の内容。
        
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
    
    * データセット **型** に設定されている **AzureSqlTable**します。
    * **linkedServiceName** に設定されている **AzureSqlLinkedService**します。
    * **tablename** に設定されている **emp**します。
    * 3 つの列 **ID**, 、**FirstName**, と **LastName** -では、データベース内の emp テーブル ID には、id 列があるのみを指定する必要があるため **FirstName** と **LastName** ここです。
    *  **可用性** に設定されている **毎時** (**頻度** に設定 **時間** と **間隔** に設定 **1**)。  Data Factory サービスは、1 時間ごとに出力データ スライスには生成、 **emp** Azure SQL データベースのテーブルです。

2.  次のコマンドを実行して、Data Factory データセットを作成します。 
    
        New-AzureRmDataFactoryDataset $df -File .\EmpSQLTable.json


## <a name="CreateAndRunAPipeline"></a>手順 4. パイプラインを作成して実行する
持つパイプラインを作成するこの手順で、 **コピー アクティビティ** を使用する **EmpTableFromBlob** 入力としてと **EmpSQLTable** として出力します。

1.  という名前の JSON ファイルを作成する **ADFTutorialPipeline.json** で、 **C:\ADFGetStartedPSH** フォルダー以下の内容。 
    
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

    - Activities セクションには、1 つだけ持つ **型** に設定されている **コピー**します。
    - 入力に設定されているアクティビティは **EmpTableFromBlob** に設定されているアクティビティの出力と **EmpSQLTable**します。
    -  **変換** ] セクションで、 **BlobSource** ソースの種類として指定されたと **SqlSink** 、シンクの種類として指定します。

    値を置き換える、 **開始** を現在の日付プロパティと **終了** 値を次の日付です。 どちらも start し、end の datetimes がである必要があります [ISO 形式](http://en.wikipedia.org/wiki/ISO_8601)します。 (例: 2014-10-14T16:32:41Z)。  **エンド** 時刻は省略可能ですが、このチュートリアルで使用されます。 
    
    値が指定されていない場合、 **エンド** として計算されますプロパティには、"**start+48 hours**"です。 パイプラインを無期限に実行する指定 **9/9/9999** の値として、 **エンド** プロパティです。
    
    上記の例では、各データ スライスが 1 時間ごとに生成されるため、データ スライスは 24 個になります。
    
    参照してください [JSON スクリプティング リファレンス](http://go.microsoft.com/fwlink/?LinkId=516971) JSON プロパティの詳細について。
2.  次のコマンドを実行して、Data Factory テーブルを作成します。 
        
        New-AzureRmDataFactoryPipeline $df -File .\ADFTutorialPipeline.json

**おめでとうございます!**これで、Azure Data Factory、リンクされたサービス、テーブル、およびパイプラインの作成と、パイプラインのスケジュール設定が完了しました。

## <a name="MonitorDataSetsAndPipeline"></a>手順 5. データセットとパイプラインを監視します。
このステップでは、Azure PowerShell を使用して、Azure Data Factory の状況を監視します。

1.  実行 **Get AzureRmDataFactory** $df 変数に出力を割り当てます。

        $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH
 
2.  実行 **Get AzureRmDataFactorySlice** のすべてのスライスに関する詳細情報、 **EmpSQLTable**, 、これは、パイプラインの出力テーブルです。  

        Get-AzureRmDataFactorySlice $df -DatasetName EmpSQLTable -StartDateTime 2015-03-03T00:00:00

    年、月、および日の部分を置き換える、 **StartDateTime** パラメーターと現在の年、月、および日付です。 これと同じ、 **開始** パイプライン JSON 内の値。 

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

3.  実行 **Get AzureRmDataFactoryRun** アクティビティがの実行の詳細を取得するのには、 **特定** スライスします。 値を変更、 **StartDateTime** と一致するパラメーター、 **開始** 上記の出力のスライスの時間。 値、 **StartDateTime** である必要があります [ISO 形式](http://en.wikipedia.org/wiki/ISO_8601)します。 例: 2014-03-03T22:00:00Z。

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

参照してください [Data Factory コマンドレット リファレンス][cmdlet-reference] Data Factory コマンドレットに関する包括的なドキュメントについてです。 



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
 
