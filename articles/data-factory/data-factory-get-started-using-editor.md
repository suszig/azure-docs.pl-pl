<properties 
    pageTitle="チュートリアル: コピー アクティビティがあるパイプラインを Data Factory Editor で作成する" 
    description="このチュートリアルでは、Azure クラシック ポータルの Data Factory エディターを使用して、コピー アクティビティがある Azure Data Factory パイプラインを作成します。" 
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

# チュートリアル: コピー アクティビティがあるパイプラインを Data Factory Editor で作成する
> [AZURE.SELECTOR]
- [チュートリアルの概要](data-factory-get-started.md)
- [Data Factory エディターの使用](data-factory-get-started-using-editor.md)
- [PowerShell の使用](data-factory-monitor-manage-using-powershell.md)
- [Visual Studio の使用](data-factory-get-started-using-vs.md)



##このチュートリアルの内容
このチュートリアルで実行する手順は次のとおりです。

手順 | 説明
-----| -----------
[手順 1. Azure Data Factory を作成する](#CreateDataFactory) | この手順では、という名前の Azure data factory を作成します **ADFTutorialDataFactory**します。  
[手順 2. リンク サービスを作成する](#CreateLinkedServices) | この手順では、2 つのリンクされたサービスを作成します: **StorageLinkedService** と **AzureSqlLinkedService**します。 StorageLinkedService は Azure Storage を、AzureSqlLinkedService は Azure SQL Database を、それぞれ ADFTutorialDataFactory にリンクします。 パイプラインの入力データは、Azure Blob Storage の BLOB コンテナーにあります。また出力データは、Azure SQL Database のテーブルに格納されます。 そのため、これら 2 つのデータ ストアをリンクされたサービスとしてデータ ファクトリに追加します。      
[手順 3. 入力テーブルと出力テーブルを作成する](#CreateInputAndOutputDataSets) | 前の手順では、入力/出力データを含むデータ ストアを参照する、リンクされたサービスを作成しました。 この手順では、2 つのデータ ファクトリ テーブルを定義します **EmpTableFromBlob** と **EmpSQLTable** --データ ストアに格納されている入力/出力データを表します。 EmpTableFromBlob の場合、ソース データを持つ BLOB を含む BLOB コンテナーを指定します。EmpSQLTable の場合は、出力データを格納する SQL テーブルを指定します。 また、データの構造や可用性など、他のプロパティも指定します。 
[手順 4. パイプラインを作成して実行する](#CreateAndRunAPipeline) | この手順では、という名前のパイプラインを作成します **ADFTutorialPipeline** ADFTutorialDataFactory 内です。 パイプラインには、 **コピー アクティビティ** いる入力のコピー、Azure から blob にデータを Azure SQL 出力テーブルです。
[手順 5. スライスとパイプラインを監視する](#MonitorDataSetsAndPipeline) | この手順では、Azure ポータルを使用して、入力テーブルと出力テーブルのスライスを監視します。
 

## <a name="CreateDataFactory"></a>手順 1. Azure Data Factory を作成する
この手順で、という名前の Azure データ ファクトリを作成する Azure ポータルを使用して **ADFTutorialDataFactory**します。

1.  ログインした後、 [Azure ポータル][azure-portal], 、] をクリックして **新規** の左下隅から選択 **データ分析** で、 **作成** ブレードをクリック **Data Factory** で、 **データ分析** ブレードです。 

    ![New->DataFactory][image-data-factory-new-datafactory-menu]    

6.  **新しいデータ ファクトリ** ブレード。
    1. 入力 **ADFTutorialDataFactory** の **名前**します。 
    
        ![[新しいデータ ファクトリ] ブレード][image-data-factory-getstarted-new-data-factory-blade]
    2. クリックして **リソース グループ名** し、次の操作を行います。
        1. クリックして **新しいリソース グループを作成**します。
        2.  **リソース グループの作成** ブレードで、入力 **ADFTutorialResourceGroup** の **名前** 、リソース グループの **[ok]**します。 

            ![リソース グループの作成][image-data-factory-create-resource-group]

        このチュートリアルの手順の一部は、名前を使用することを前提としています: **ADFTutorialResourceGroup** リソース グループにします。 リソース グループの詳細については、次を参照してください。 [リソース グループを使用した Azure リソースの管理に](resource-group-overview.md)します。  
7.  **新しいデータ ファクトリ** ブレードで、ことに注意して **スタート画面に追加** が選択されています。
8. クリックして **作成** で、 **新しいデータ ファクトリ** ブレードです。

    Azure Data Factor の名前はグローバルに一意にする必要があります。 エラーが発生する場合: **データ ファクトリ名"ADFTutorialDataFactory"は使用できません**, 、data factory (yournameADFTutorialDataFactory など) の名前を変更し、もう一度作成してください。 参照してください [Data Factory - 名前付け規則](data-factory-naming-rules.md) Data Factory アーティファクトの名前付け規則についてのトピックです。  
     
    ![使用できない Data Factory 名][image-data-factory-name-not-available]
    
    > [AZURE.NOTE] データ ファクトリの名前は、今後しないためにパブリックに表示する DNS 名として登録することがあります。  

9. クリックして **通知** ハブで、左辺と、作成プロセスからの通知を探します。 クリックして **X** を閉じる、 **通知** ブレードが開いている場合。 
10. 表示の作成が完了した後、 **DATA FACTORY** ブレードが次のようにします。

    ![データ ファクトリのホーム ページ][image-data-factory-get-stated-factory-home-page]

## <a name="CreateLinkedServices"></a>手順 2. リンクされたサービスを作成する
リンクされたサービスは、データ ストアまたはコンピューティング サービスを Azure Data Factory にリンクします。 データ ストアには、Azure Storage、Azure SQL Database、またはオンプレミスの SQL Server データベースを指定できます。

この手順では、2 つのリンクされたサービスを作成します: **StorageLinkedService** と **AzureSqlLinkedService**します。 StorageLinkedService リンクされたサービス、Azure ストレージ アカウントを AzureSqlLinkedService は Azure SQL database を **ADFTutorialDataFactory**します。 このチュートリアルの後半で、StorageLinkedService 内の BLOB コンテナーから AzureSqlLinkedService 内の SQL テーブルにデータをコピーするパイプラインを作成します。

### Azure ストレージ アカウント用にリンクされたサービスを作成する
1.   **DATA FACTORY** ブレードで、をクリックして **作成者および展開** を起動するタイル、 **エディター** data factory のです。

    ![[作成とデプロイ] タイル][image-author-deploy-tile] 

    参照してください [Data Factory エディター][data-factory-editor] Data Factory エディターの詳細についてのトピックです。 
     
5.  **エディター**, 、クリックして **新しいデータ ストア** ] ボタンをクリックして [ **Azure ストレージ** 、ドロップ ダウン メニューからです。 Azure Storage のリンクされたサービスを作成するための JSON テンプレートが右側のウィンドウに表示されます。 

    ![エディターの [新しいデータ ストア] ボタン][image-editor-newdatastore-button]
    
6. 置換 **accountname** と **accountkey** アカウント名と、Azure ストレージ アカウントのアカウント キーの値を使用します。 

    ![エディターの Blob Storage JSON][image-editor-blob-storage-json]    
    
    参照してください [JSON スクリプティング リファレンス](http://go.microsoft.com/fwlink/?LinkId=516971) JSON プロパティの詳細について。

6. クリックして **展開** 、ツールバーの StorageLinkedService をデプロイします。 メッセージを表示することを確認 **リンクされたサービス作成が正常に** タイトル バーにします。

    ![エディターの Blob Storage デプロイ][image-editor-blob-storage-deploy]

### Azure SQL Database 用にリンクされたサービスを作成する
1.  **Data Factory エディター** , 、クリックして **新しいデータ ストア** ] ボタンをクリックして [ **Azure SQL データベース** 、ドロップ ダウン メニューからです。 Azure SQL のリンクされたサービスを作成するための JSON テンプレートが右側のウィンドウに表示されます。

    ![エディターの Azure SQL 設定][image-editor-azure-sql-settings]

2. 置換 **servername**, 、**databasename**, 、**username@servername**, 、および **パスワード** 、Azure SQL サーバー、データベース、ユーザー アカウントとパスワードの名前を持つ。 
3. クリックして **展開** 、ツールバーの AzureSqlLinkedService を作成してデプロイします。 
   

## <a name="CreateInputAndOutputDataSets"></a>手順 3. 入力テーブルと出力テーブルを作成する
リンクされたサービスを作成した前の手順で **StorageLinkedService** と **AzureSqlLinkedService** Azure ストレージ アカウントと Azure SQL データベースをデータ ファクトリにリンクする: **ADFTutorialDataFactory**します。 この手順では、2 つのデータ ファクトリ テーブルを定義します **EmpTableFromBlob** と **EmpSQLTable** --それぞれ StorageLinkedService と AzureSqlLinkedService が参照するデータ ストアに格納されている入力/出力データを表します。 EmpTableFromBlob のソース データや EmpSQLTable の blob を含む blob コンテナーを指定する場合は、出力データを保存する SQL テーブルを指定します。 

### 入力テーブルの作成 
テーブルとは四角形のデータセットで、スキーマを持っています。 この手順では、という名前のテーブルを作成します **EmpBlobTable** で表される Azure ストレージ内の blob コンテナーを指す、 **StorageLinkedService** のリンクされたサービスです。

1.  **エディター** Data Factory をクリックして **新しいデータセット** をクリックしてツール バー ボタン **Blob テーブル** 、ドロップ ダウン メニューからです。 
2. 右側のウィンドウの JSON スクリプトを、次の JSON スニペットに置き換えます。 

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
              "folderPath": "adftutorial/",
              "fileName": "emp.txt",
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
    - **linkedServiceName** に設定されている **StorageLinkedService**します。 このリンクされたサービスは手順 2. で作成しました。
    - **folderPath** に設定されている、 **adftutorial** コンテナーです。 フォルダー内の BLOB の名前を指定することもできます。 BLOB の名前を指定しない場合、コンテナー内のすべての BLOB からのデータが入力データと見なされます。  
    - 形式 **型** に設定されている **TextFormat**
    - テキスト ファイルに 2 つのフィールドが **FirstName** と **LastName** 、コンマで区切られて (**columnDelimiter**) 
    -  **可用性** に設定されている **時間単位** (**頻度** に設定されている **時間** と **間隔** に設定されている **1** ) よう Data Factory サービスは入力データの毎時 blob コンテナー内のルート フォルダーで、(**adftutorial**) を指定します。 
    

    if you don't specify a **fileName** for an **input** **table**, all files/blobs from the input folder (**folderPath**) are considered as inputs. If you specify a fileName in the JSON, only the specified file/blob is considered asn input. See the sample files in the [tutorial][adf-tutorial] for examples.
 
    If you do not specify a **fileName** for an **output table**, the generated files in the **folderPath** are named in the following format: Data.&lt;Guid\&gt;.txt (example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

    To set **folderPath** and **fileName** dynamically based on the **SliceStart** time, use the **partitionedBy** property. In the following example, folderPath uses Year, Month, and Day from from the SliceStart (start time of the slice being processed) and fileName uses Hour from the SliceStart. For example, if a slice is being produced for 2014-10-20T08:00:00, the folderName is set to wikidatagateway/wikisampledataout/2014/10/20 and the fileName is set to 08.csv. 

        "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
        "fileName": "{Hour}.csv",
        "partitionedBy": 
        [
            { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
            { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
            { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
            { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
        ],

    See [JSON Scripting Reference](http://go.microsoft.com/fwlink/?LinkId=516971) for details about JSON properties.

2. クリックして **展開** 、ツールバーの作成および展開する、 **EmpTableFromBlob** テーブルです。 表示されていることを確認、 **テーブルは正常に作成された** エディターのタイトル バーにメッセージです。

### 出力テーブルの作成
という名前の出力テーブルを作成する手順では、 **EmpSQLTable** によって表される Azure SQL 内の SQL テーブルへのポインターがデータベースにある、 **AzureSqlLinkedService** のリンクされたサービスです。 

1.  **エディター** Data Factory をクリックして **新しいデータセット** をクリックしてツール バー ボタン **Azure SQL テーブル** 、ドロップ ダウン メニューからです。 
2. 右側のウィンドウの JSON スクリプトを、次の JSON スニペットに置き換えます。

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
    
    * データセット **型** に設定されている **AzureSQLTable**します。
    * **linkedServiceName** に設定されている **AzureSqlLinkedService** (手順 2. でこのリンクされたサービスを作成した)。
    * **tablename** に設定されている **emp**します。
    * 3 つの列 **ID**, 、**FirstName**, と **LastName** -では、データベース内の emp テーブル ID には、id 列があるのみを指定する必要があるため **FirstName** と **LastName** ここです。
    *  **可用性** に設定されている **毎時** (**頻度** に設定 **時間** と **間隔** に設定 **1**)。  Data Factory サービスは、1 時間ごとに出力データ スライスには生成、 **emp** Azure SQL データベースのテーブルです。


3. クリックして **展開** 、ツールバーの作成および展開する、 **EmpSQLTable** テーブルです。


## <a name="CreateAndRunAPipeline"></a>手順 4. パイプラインを作成して実行する
持つパイプラインを作成するこの手順で、 **コピー アクティビティ** を使用する **EmpTableFromBlob** 入力としてと **EmpSQLTable** として出力します。

1.  **エディター** Data Factory をクリックして **新しいパイプライン** ツールバーのボタンをクリックします。 ボタンが表示されない場合は、ツール バーの **[...](省略記号)** をクリックします。 またを右クリックして **パイプライン** ツリー ビュー] をクリック **新しいパイプライン**します。

    ![エディターの [新しいパイプライン] ボタン][image-editor-newpipeline-button]
 
2. 右側のウィンドウの JSON スクリプトを、次の JSON スニペットに置き換えます。 
        
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
                  "retry": 0,
                  "timeout": "01:00:00"
                }
              }
            ],
            "start": "2015-07-12T00:00:00Z",
            "end": "2015-07-13T00:00:00Z"
          }
        } 

    以下の点に注意してください。

    - Activities セクションには、1 つだけ持つ **型** に設定されている **CopyActivity**します。
    - 入力に設定されているアクティビティは **EmpTableFromBlob** に設定されているアクティビティの出力と **EmpSQLTable**します。
    -  **変換** ] セクションで、 **BlobSource** ソースの種類として指定されたと **SqlSink** 、シンクの種類として指定します。

    値を置き換える、 **開始** を現在の日付プロパティと **終了** 値を次の日付です。 日付の部分のみを指定し、時刻の部分をスキップすることもできます。 たとえば、"2015-02-03" と "2015-02-03T00:00:00Z" は同じです。
    
    どちらも start し、end の datetimes がである必要があります [ISO 形式](http://en.wikipedia.org/wiki/ISO_8601)します。 (例: 2014-10-14T16:32:41Z)。  **エンド** 時刻は省略可能ですが、このチュートリアルで使用されます。 
    
    値が指定されていない場合、 **エンド** として計算されますプロパティには、"**start+48 hours**"です。 パイプラインを無期限に実行する指定 **9999-09-09** の値として、 **エンド** プロパティです。
    
    上記の例では、各データ スライスが 1 時間ごとに生成されるため、データ スライスは 24 個になります。
    
    参照してください [JSON スクリプティング リファレンス](http://go.microsoft.com/fwlink/?LinkId=516971) JSON プロパティの詳細について。

4. クリックして **展開** 、ツールバーの作成および展開する、 **ADFTutorialPipeline**します。 表示されていることを確認、 **パイプラインは正常に作成された** メッセージです。
5. これで、閉じる、 **エディター** ブレードをクリックして **X**です。クリックして **X** ツールバーおよびツリー ビューで [ADFTutorialDataFactory] ブレードを閉じます。 表示される場合 **、未保存の編集は破棄されます** メッセージで、をクリックして **OK**します。
6. する必要があります、 **DATA FACTORY** ブレード、 **ADFTutorialDataFactory**します。

**ご利用ありがとうございます。** これで、Azure Data Factory、リンクされたサービス、テーブル、およびパイプラインの作成と、パイプラインのスケジュール設定が完了しました。   
 
### ダイアグラム ビューでの Data Factory の表示 
1.  **DATA FACTORY** ブレードで、をクリックして **ダイアグラム**します。

    ![[Data Factory] ブレードの [ダイアグラム] タイル][image-datafactoryblade-diagramtile]

2. 以下のような図が表示されるはずです。 

    ![ダイアグラム ビュー][image-data-factory-get-started-diagram-blade]

    パイプラインとテーブルは、拡大、縮小、100% に拡大、ウィンドウのサイズに合わせて大きさを変更、自動的に配置などの表示が可能です。また、系列情報を表示 (選択した項目の上位項目や下位項目を強調表示) することもできます。  オブジェクト (入力/出力テーブルまたはパイプライン) をダブルクリックすると、そのオブジェクトのプロパティを表示できます。 
3. 右クリックして **ADFTutorialPipeline** ダイアグラムの表示] をクリック **パイプラインを開く**します。 パイプライン内のアクティビティに加えて、アクティビティの入力データセットと出力データセットが表示されます。 このチュートリアルでは、パイプラインのアクティビティ (コピー アクティビティ) は、入力データセットとして EmpTableBlob、出力データセットとして EmpSQLTable の 1 つだけです。   

    ![パイプラインを開く](./media/data-factory-get-started-using-editor/DiagramView-OpenPipeline.png)

4. クリックして **Data factory** ダイアグラム ビューに戻るために左上隅にある階層リンクでします。 ダイアグラム ビューには、すべてのパイプラインが表示されます。 この例では 1 つのパイプラインのみ作成しました。   
 

## <a name="MonitorDataSetsAndPipeline"></a>手順 5. データセットとパイプラインを監視する
このステップでは、Azure クラシック ポータルを使用して、Azure Data Factory の状況を監視します。 PowerShell コマンドレットを使用して、データセットとパイプラインを監視することもできます。 監視するためのコマンドレットの使用に関する詳細については、「 [モニター Data Factory と管理 PowerShell コマンドレットを使用して][monitor-manage-using-powershell]います。

1. 移動 [Azure Classic Portal (プレビュー)][azure-portal] をまだ開いていない場合。 
2. 場合ブレード **ADFTutorialDataFactory** いないを開きをクリックして開くには **ADFTutorialDataFactory** 上、 **スタート画面**します。 
3. 作成したテーブルの数と名前、およびパイプラインの名前がブレードに表示されます。

    ![名前のついたホーム ページ][image-data-factory-get-started-home-page-pipeline-tables]

4. をクリックして **データセット** を並べて表示します。
5.  **データセット** ブレードで、をクリックして **EmpTableFromBlob**します。 これは、用の入力テーブル、 **ADFTutorialPipeline**します。

    ![EmpTableFromBlob を選択したデータセット][image-data-factory-get-started-datasets-emptable-selected]   
5. 現在の時刻までのデータ スライスが既に生成されており、 **準備ができて** ため、 **emp.txt** ファイル、blob コンテナーに常に存在します。 **adftutorial \input**します。 スライス表示されていないことの確認、 **最近失敗したスライス** 下部のセクションです。

    両方とも **最近更新したスライス** と **最近失敗したスライス** リストは並んで、 **最終更新時刻**します。 次の状況では、スライスの更新時刻が変更されます。 
    

    -  You update the status of the slice manually, for example, by using the **Set-AzureRmDataFactorySliceStatus** (or) by clicking **RUN** on the **SLICE** blade for the slice.
    -  The slice changes status due to an execution (e.g. a run started, a run ended and failed, a run ended and succeeded, etc).
 
    Click on the title of the lists or **... (ellipses)** to see the larger list of slices. Click **Filter** on the toolbar to filter the slices.  
    
    To view the data slices sorted by the slice start/end times instead, click **Data slices (by slice time)** tile.   

    ![Data Slices by Slice Time][DataSlicesBySliceTime]   

6.  **データセット** ブレードで、をクリックして、 **EmpSQLTable**します。 これは、用の出力テーブル、 **ADFTutorialPipeline**します。

    ![[データセット] ブレード][image-data-factory-get-started-datasets-blade]



     
6. 確認する必要があります、 **EmpSQLTable** ブレードが次のようにします。

    ![[テーブル] ブレード][image-data-factory-get-started-table-blade]
 
7. 現在の時刻までのデータ スライスが既に生成されており、 **準備**します。 スライスが表示されていないで、 **問題のあるスライス** 下部のセクションです。
8. クリックして **.(省略記号)** すべてのスライスを表示します。

    ![[データ スライス] ブレード][image-data-factory-get-started-dataslices-blade]

9. 一覧からいずれかのデータ スライスをクリックして表示されます、 **データ スライス** ブレードです。

    ![[データ スライス] ブレード][image-data-factory-get-started-dataslice-blade]
  
    スライスのではない場合、 **準備ができて** 状態、準備ができていないし、現在のスライスの実行をブロックするアップ ストリームのスライスを確認することができます、 **準備ができていないアップ ストリーム スライス** ] ボックスの一覧です。 

11.  **データ スライス** ブレードで、下部にあるリストですべてのアクティビティ実行状況が表示する必要があります。 をクリックして、 **アクティビティの実行** を表示する、 **アクティビティの実行の詳細** ブレードです。 

    ![アクティビティの実行状況の詳細][image-data-factory-get-started-activity-run-details]

    
12. クリックして **X** ホーム ブレードに戻るまですべてのブレードを閉じる、 **ADFTutorialDataFactory**します。
14. (省略可能)クリックして **パイプライン** のホーム ページで **ADFTutorialDataFactory**, 、] をクリックして **ADFTutorialPipeline** で、 **パイプライン** ブレードで、入力テーブル (**Consumed**) または出力テーブル (**Produced**)。
15. 起動 **SQL Server Management Studio**, 、Azure SQL データベースに接続し、行が挿入されたことを確認、 **emp** データベース内のテーブルです。

    ![SQL クエリの結果][image-data-factory-get-started-sql-query-results]


## 概要 
このチュートリアルでは、Azure Data Factory を作成し、Azure BLOB から Azure SQL Database にデータをコピーしました。 また、Azure ポータルを使用して、データ ファクトリ、リンクされたサービス、テーブル、パイプラインを作成しました。 以下は、このチュートリアルで実行した手順の概要です。  

1.  Azure **データ ファクトリ**します。
2.  作成 **リンクされたサービス** データ ストアとコンピューティングをリンクする (と呼ばれる **リンクされたサービス**) データ ファクトリにします。
3.  作成 **テーブル** 入力データと出力データ パイプラインを記述します。
4.  作成 **パイプライン**します。 パイプラインは、入力を処理し、出力を生成する 1 つ以上のアクティビティで構成されます。 指定して、パイプラインのアクティブな期間を設定 **開始** 時間と **エンド** 時間です。 有効期間は、データ スライスが生成される期間を定義します。 


サポートされているアクティビティの一覧は、次を参照してください [パイプラインとアクティビティ。][msdn-activities] 
トピックと、サポートされているリンクされたサービスの一覧は、次を参照してください [リンクされたサービス。][msdn-linkedservices] 
MSDN ライブラリのトピックです。
 
Azure PowerShell を使用してこのチュートリアルには、次を参照してください。 [の作成と Azure PowerShell を使用してデータ ファクトリの監視][monitor-manage-using-powershell]します。  


<!--Link references-->
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[msdn-activities]: https://msdn.microsoft.com/library/dn834988.aspx
[msdn-linkedservices]: https://msdn.microsoft.com/library/dn834986.aspx
[data-factory-naming-rules]: https://msdn.microsoft.com/library/azure/dn835027.aspx

[azure-portal]: https://portal.azure.com/
[download-azure-powershell]: http://azure.microsoft.com/documentation/articles/install-configure-powershell
[sql-management-studio]: http://azure.microsoft.com/documentation/articles/sql-database-manage-azure-ssms/#Step2
[sql-cmd-exe]: https://msdn.microsoft.com/library/azure/ee336280.aspx

[data-factory-editor]: data-factory-editor.md
[monitor-manage-using-powershell]: data-factory-monitor-manage-using-powershell.md
[adf-tutorial]: data-factory-tutorial.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[data-factory-introduction]: data-factory-introduction.md
[data-factory-create-storage]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account
[data-factory-create-sql-database]: ../sql-database-create-configure.md


[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456

<!--Image references-->

[DataSlicesBySliceTime]: ./media/data-factory-get-started-using-editor/DataSlicesBySliceTime.png

[image-data-factory-getstarted-new-everything]: ./media/data-factory-get-started-using-editor/GetStarted-New-Everything.png

[image-data-factory-gallery-storagecachebackup]: ./media/data-factory-get-started-using-editor/getstarted-gallery-datastoragecachebackup.png

[image-data-factory-gallery-storagecachebackup-seeall]: ./media/data-factory-get-started-using-editor/getstarted-gallery-datastoragecachebackup-seeall.png

[image-data-factory-getstarted-data-services-data-factory-selected]: ./media/data-factory-get-started-using-editor/getstarted-data-services-data-factory-selected.png

[image-data-factory-getstarted-data-factory-create-button]: ./media/data-factory-get-started-using-editor/getstarted-data-factory-create-button.png

[image-data-factory-getstarted-new-data-factory-blade]: ./media/data-factory-get-started-using-editor/getstarted-new-data-factory.png

[image-data-factory-get-stated-factory-home-page]: ./media/data-factory-get-started-using-editor/getstarted-data-factory-home-page.png

[image-author-deploy-tile]: ./media/data-factory-get-started-using-editor/getstarted-author-deploy-tile.png

[image-editor-newdatastore-button]: ./media/data-factory-get-started-using-editor/getstarted-editor-newdatastore-button.png

[image-editor-blob-storage-json]: ./media/data-factory-get-started-using-editor/getstarted-editor-blob-storage-json.png

[image-editor-blob-storage-deploy]: ./media/data-factory-get-started-using-editor/getstarted-editor-blob-storage-deploy.png

[image-editor-azure-sql-settings]: ./media/data-factory-get-started-using-editor/getstarted-editor-azure-sql-settings.png

[image-editor-newpipeline-button]: ./media/data-factory-get-started-using-editor/getstarted-editor-newpipeline-button.png

[image-datafactoryblade-diagramtile]: ./media/data-factory-get-started-using-editor/getstarted-datafactoryblade-diagramtile.png


[image-data-factory-get-started-startboard]: ./media/data-factory-get-started-using-editor/getstarted-data-factory-startboard.png

[image-data-factory-get-started-linked-services-link]: ./media/data-factory-get-started-using-editor/getstarted-data-factory-linked-services-link.png

[image-data-factory-get-started-linked-services-add-store-button]: ./media/data-factory-get-started-using-editor/getstarted-linked-services-add-store-button.png

[image-data-factory-linked-services-get-started-new-data-store]: ./media/data-factory-get-started-using-editor/getstarted-linked-services-new-data-store.png

[image-data-factory-get-started-new-data-store-with-storage]: ./media/data-factory-get-started-using-editor/getstarted-linked-services-new-data-store-with-storage.png

[image-data-factory-get-started-storage-account-name-key]: ./media/data-factory-get-started-using-editor/getstarted-storage-account-name-key.png

[image-data-factory-get-started-linked-services-list-with-myblobstore]: ./media/data-factory-get-started-using-editor/getstarted-linked-services-list-with-myblobstore.png

[image-data-factory-get-started-linked-azure-sql-properties]: ./media/data-factory-get-started-using-editor/getstarted-linked-azure-sql-properties.png

[image-data-factory-get-started-azure-sql-connection-string]: ./media/data-factory-get-started-using-editor/getstarted-azure-sql-connection-string.png

[image-data-factory-get-started-linked-services-list-two-stores]: ./media/data-factory-get-started-using-editor/getstarted-linked-services-list-two-stores.png

[image-data-factory-get-started-storage-explorer]: ./media/data-factory-get-started-using-editor/getstarted-storage-explorer.png

[image-data-factory-get-started-diagram-link]: ./media/data-factory-get-started-using-editor/getstarted-diagram-link.png

[image-data-factory-get-started-diagram-blade]: ./media/data-factory-get-started-using-editor/getstarted-diagram-blade.png

[image-data-factory-get-started-home-page-pipeline-tables]: ./media/data-factory-get-started-using-editor/getstarted-datafactory-home-page-pipeline-tables.png

[image-data-factory-get-started-datasets-blade]: ./media/data-factory-get-started-using-editor/getstarted-datasets-blade.png

[image-data-factory-get-started-table-blade]: ./media/data-factory-get-started-using-editor/getstarted-table-blade.png

[image-data-factory-get-started-dataslices-blade]: ./media/data-factory-get-started-using-editor/getstarted-dataslices-blade.png

[image-data-factory-get-started-dataslice-blade]: ./media/data-factory-get-started-using-editor/getstarted-dataslice-blade.png

[image-data-factory-get-started-sql-query-results]: ./media/data-factory-get-started-using-editor/getstarted-sql-query-results.png

[image-data-factory-get-started-datasets-emptable-selected]: ./media/data-factory-get-started-using-editor/DataSetsWithEmpTableFromBlobSelected.png

[image-data-factory-get-started-activity-run-details]: ./media/data-factory-get-started-using-editor/ActivityRunDetails.png

[image-data-factory-create-resource-group]: ./media/data-factory-get-started-using-editor/CreateNewResourceGroup.png

[image-data-factory-database-connection-string]: ./media/data-factory-get-started-using-editor/DatabaseConnectionString.png

[image-data-factory-new-datafactory-menu]: ./media/data-factory-get-started-using-editor/NewDataFactoryMenu.png

[image-data-factory-sql-management-console]: ./media/data-factory-get-started-using-editor/getstarted-azure-sql-management-console.png

[image-data-factory-sql-management-console-2]: ./media/data-factory-get-started-using-editor/getstarted-azure-sql-management-console-2.png

[image-data-factory-name-not-available]: ./media/data-factory-get-started-using-editor/getstarted-data-factory-not-available.png
 

