<properties 
    pageTitle="チュートリアル: コピー アクティビティがあるパイプラインを Visual Studio で作成する" 
    description="このチュートリアルでは、Visual Studio を使用して、コピー アクティビティがある Azure Data Factory パイプラインを作成します。" 
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
    ms.topic="get-started-article" 
    ms.date="11/02/2015" 
    ms.author="spelluru"/>

# チュートリアル: Visual Studio を使用したデータ ファクトリの作成と監視
> [AZURE.SELECTOR]
- [チュートリアルの概要](data-factory-get-started.md)
- [Data Factory エディターを使用します。](data-factory-get-started-using-editor.md)
- [PowerShell の使用](data-factory-monitor-manage-using-powershell.md)
- [Visual Studio を使用](data-factory-get-started-using-vs.md)


##このチュートリアルの内容
このチュートリアルでは、Visual Studio 2013 で次の作業を行います。

1. 2 つのリンクされたサービスを作成します。 **AzureStorageLinkedService1** と **AzureSqlinkedService1**します。 AzureStorageLinkedService1 は Azure のストレージをリンクし、AzureSqlLinkedService1 は、Azure SQL データベースをデータ ファクトリにリンク: **ADFTutorialDataFactoryVS**します。 パイプラインの入力データは、Azure Blob Storage の BLOB コンテナーにあります。また出力データは、Azure SQL Database のテーブルに格納されます。 そのため、これら 2 つのデータ ストアをリンクされたサービスとしてデータ ファクトリに追加します。
2. 2 つのデータ ファクトリ テーブルを作成します。 **EmpTableFromBlob** と **EmpSQLTable**, 、データ ストアに格納されている入力/出力データを表します。 EmpTableFromBlob の場合、ソース データを持つ BLOB を含む BLOB コンテナーを指定します。EmpSQLTable の場合は、出力データを格納する SQL テーブルを指定します。 また、データの構造や可用性など、他のプロパティも指定します。
3. という名前のパイプラインを作成する **ADFTutorialPipeline** ADFTutorialDataFactoryVS にします。 パイプラインには、 **コピー アクティビティ** いる入力のコピー、Azure から blob にデータを Azure SQL 出力テーブルです。
4. Data Factory を作成し、リンクされたサービス、テーブル、パイプラインをデプロイする。    

## Visual Studio を使用して、Data Factory のエンティティを作成し、デプロイする 

### 前提条件
コンピューターに以下がインストールされている必要があります。 
- Visual Studio 2013
- Azure SDK for Visual Studio 2013 をダウンロードします。 移動 [Azure ダウンロード ページ](http://azure.microsoft.com/downloads/) ] をクリック **VS 2013 インストール** で、 **.NET** セクションです。

### チュートリアル

#### Visual Studio プロジェクトの作成 
1. 起動 **Visual Studio 2013**します。 クリックして **ファイル**, 、] をポイント **新規**, 、] をクリック **プロジェクト**します。 確認する必要があります、 **新しいプロジェクト** ] ダイアログ ボックス。  
2.  **新しいプロジェクト** ダイアログ ボックスで、選択、 **DataFactory** テンプレート、およびクリック **Empty Data Factory Project**します。 DataFactory テンプレートが表示されない場合は、Visual Studio を終了し、Azure SDK for Visual Studio 2013 をインストールし、Visual Studio を再度開きます。  

    ![[新しいプロジェクト] ダイアログ ボックス
](./media/data-factory-get-started-using-vs/new-project-dialog.png)

3. 入力、 **名前** 、プロジェクトの **場所**, との名前、 **ソリューション**, 、] をクリック **[ok]**します。

    ![Solution Explorer](./media/data-factory-get-started-using-vs/solution-explorer.png)   

#### リンクされたサービスの作成
リンクされたサービスは、データ ストアまたはコンピューティング サービスを Azure Data Factory にリンクします。 データ ストアには、Azure Storage、Azure SQL Database、またはオンプレミスの SQL Server データベースを指定できます。

この手順では、2 つのリンクされたサービスを作成します: **AzureStorageLinkedService1** と **AzureSqlLinkedService1**します。 これらは AzureStorageLinkedService1 リンク サービスは Azure ストレージ アカウントを AzureSqlLinkedService は Azure SQL データベースをデータ ファクトリにリンク: **ADFTutorialDataFactory**します。 

##### Azure Storage のリンクされたサービスを作成します。

4. 右クリック **リンクされたサービス** にあるソリューション エクスプ ローラーで、 **追加**, 、] をクリック **[新しい項目の**です。      
5.  **新しい項目の追加** ダイアログ ボックスで、 **Azure Storage Linked Service** ] をクリックし、一覧から **追加**します。 

    ![新規のリンクされたサービス](./media/data-factory-get-started-using-vs/new-linked-service-dialog.png)
 
3. 置換 **accountname** と **accountkey** 、Azure ストレージ アカウントとそのキーの名前に置き換えます。 

    ![Azure Storage のリンクされたサービス](./media/data-factory-get-started-using-vs/azure-storage-linked-service.png)

4. 保存、 **AzureStorageLinkedService1.json** ファイルです。

#### Azure SQL のリンクされたサービスを作成します。

5. 右クリックして **リンクされたサービス** 内のノード、 **ソリューション エクスプ ローラー** ] をポイントして、もう一度 **追加**, 、] をクリック **[新しい項目の**です。 
6. この時点で、[ **Azure SQL Linked Service**, 、] をクリック **追加**します。 
7.  **AzureSqlLinkedService1.json ファイル**, 、置き換える **servername**, 、**databasename**, 、**username@servername**, 、および **パスワード** 、Azure SQL サーバー、データベース、ユーザー アカウントとパスワードの名前を持つ。    
8.  保存、 **AzureSqlLinkedService1.json** ファイルです。 


### 入力テーブルと出力テーブルを作成する
リンクされたサービスを作成した前の手順で **AzureStorageLinkedService1** と **AzureSqlLinkedService1** Azure ストレージ アカウントと Azure SQL データベースをデータ ファクトリにリンクする: **ADFTutorialDataFactory**します。 この手順では、2 つのデータ ファクトリ テーブルを定義します **EmpTableFromBlob** と **EmpSQLTable** --それぞれ AzureStorageLinkedService1 と AzureSqlLinkedService1 が参照するデータ ストアに格納されている入力/出力データを表します。 EmpTableFromBlob のソース データや EmpSQLTable の blob を含む blob コンテナーを指定する場合は、出力データを保存する SQL テーブルを指定します。

#### 入力テーブルの作成

9. 右クリック **テーブル** で、 **ソリューション エクスプ ローラー**, 、] をポイント **追加**, 、] をクリック **[新しい項目の**です。
10.  **新しい項目の追加** ダイアログ ボックスで、 **Azure Blob**, 、] をクリック **追加**します。   
10. 次のテキストを JSON テキストを置換し、保存、 **AzureBlobLocation1.json** ファイルです。 

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
            "linkedServiceName": "AzureStorageLinkedService1",
            "typeProperties": {
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

#### 出力テーブルの作成

11. 右クリック **テーブル** で、 **ソリューション エクスプ ローラー** ] をポイントして、もう一度 **追加**, 、] をクリック **[新しい項目の**です。
12.  **新しい項目の追加** ダイアログ ボックスで、 **Azure SQL**, 、] をクリック **追加**します。 
13. 次の JSON に、JSON のテキストを置き換えてを保存、 **AzureSqlTableLocation1.json** ファイルです。

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
            "linkedServiceName": "AzureSqlLinkedService1",
            "typeProperties": {
              "tableName": "emp"
            },
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }

#### パイプラインの作成 
入力/出力のリンクされたサービスとテーブルの作成は完了しました。 持つパイプラインを作成するようになりましたが、 **コピー アクティビティ** をコピーする、Azure blob からデータを Azure SQL database。 


1. 右クリック **パイプライン** で、 **ソリューション エクスプ ローラー**, 、] をポイント **追加**, 、] をクリック **[新しい項目の**です。  
15. 選択 **Copy Data Pipeline** で、 **新しい項目の追加** ] ダイアログ ボックスをクリック **追加**します。 
16. 次の JSON に、JSON を置き換えてを保存、 **CopyActivity1.json** ファイル.
            
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
                    "type": "SqlSink",
                    "writeBatchSize": 10000,
                    "writeBatchTimeout": "60:00:00"
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
            "start": "2015-07-12T00:00:00Z",
            "end": "2015-07-13T00:00:00Z",
            "isPaused": false
          }
        }

#### Data Factory エンティティの発行/デプロイ
  
18. ソリューション エクスプ ローラーでプロジェクトを右クリックし、クリックして **発行**します。 
19. 表示される場合 **Microsoft アカウントにサイン イン** ダイアログ ボックスで、Azure サブスクリプションを持っているアカウントの資格情報の入力を **サインイン**します。
20. 次のダイアログ ボックスが表示されます。

    ![[発行] ダイアログ ボックス](./media/data-factory-get-started-using-vs/publish.png)

21. Data Factory の構成ページで、次の操作を行います。 
    1. 選択 **新しいデータ ファクトリの作成** オプション。
    2. 入力 **VSTutorialFactory** の **名前**します。  
    
        > [AZURE.NOTE]  
        > Azure Data Factor の名前はグローバルに一意にする必要があります。 発行時にデータ ファクトリの名前に関するエラーが発生した場合は、そのデータ ファクトリの名前を変更して (yournameVSTutorialFactory など)、発行し直してください。 参照してください [Data Factory - 名前付け規則](data-factory-naming-rules.md) Data Factory アーティファクトの名前付け規則についてのトピックです。
        > 
        > データ ファクトリの名前は今後、DNS 名として登録される可能性があるため、一般ユーザーに表示される場合があります。
    3.最適なサブスクリプションを選択して、 **サブスクリプション** フィールドです。 
    4.選択、 **リソース グループ** データ ファクトリを作成するためです。 
    5.選択、 **地域** data factory のです。 
    6.をクリックして **次** に切り替える、 **項目の発行** ページです。 
23.  **項目の発行** ] ページで、すべてのデータ ファクトリ エンティティが選択されていることを確認] をクリックして **次** に切り替える、 **概要** ページです。     
24. 概要を確認し、をクリックして **次** 展開プロセスとビューを起動する、 **展開ステータス**します。
25.  **展開ステータス** ] ページで、展開プロセスの状態を確認する必要があります。 デプロイメントが完了したら、[完了] をクリックします。 


## サーバー エクスプローラーを使用して Data Factory のエンティティを確認する

1.  **Visual Studio**, 、クリックして **ビュー** ] をクリックし、メニューの [ **サーバー エクスプ ローラー**します。
2. サーバー エクスプ ローラー] ウィンドウで、 **Azure** 展開 **Data Factory**します。 表示される場合 **Visual Studio にサインイン**, 、入力、 **アカウント** をクリックして、Azure サブスクリプションに関連付けられている **続行**します。 入力 **パスワード**, 、] をクリック **サインイン**します。 Visual Studio は、サブスクリプション内のすべての Azure データ ファクトリに関する情報を取得しようとします。 この操作のステータスを確認、 **Data Factory Task List** ウィンドウです。
    ![サーバー エクスプ ローラー](./media/data-factory-get-started-using-vs/server-explorer.png)
3. [Data factory を右クリックし、既存のデータ ファクトリに基づいて Visual Studio プロジェクトを作成する新しいプロジェクトにデータ ファクトリのエクスポートを選択できます。
    ![Data Factory の Visual Studio プロジェクトへのエクスポート](./media/data-factory-get-started-using-vs/export-data-factory-menu.png)  

## Visual Studio の Data Factory ツールを更新する
Visual Studio の Azure Data Factory ツールを更新するには、次のように行います。

1. クリックして **ツール** ] を選択し、メニューに **拡張機能と更新プログラム**します。 
2. 選択 **更新** クリックして左ペインで **Visual Studio ギャラリー**します。
4. 選択 **Visual Studio の Azure Data Factory ツール** ] をクリック **更新**します。 このエントリが表示されない場合は、ツールは既に最新バージョンです。 

参照してください [データセットとパイプラインを監視](data-factory-get-started-using-editor.md#MonitorDataSetsAndPipeline) このチュートリアルで作成した、Azure ポータルを使用して、パイプラインとデータセットを監視する方法についてです。



