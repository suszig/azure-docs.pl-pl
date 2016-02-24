<properties 
    pageTitle="Azure Data Factory エディター" 
    description="軽量の Web ベース UI を使用して、リンクされたサービス、テーブル、パイプラインの JSON 定義を作成、編集、デプロイできる、Azure Data Factory エディターについて説明します。" 
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
    ms.date="09/22/2015" 
    ms.author="spelluru"/>

# Azure Data Factory エディター
Azure Data Factory エディターは、Azure ポータルの一部である軽量の Web エディターであり、Azure Data Factory のすべてのエンティティの JSON ファイルを作成、編集、デプロイできます。 このエディターでは、Data Factory サービスに付属する JSON テンプレートを使用し、リンクされたサービス、データ セット、およびパイプラインを作成できます。 初めて PowerShell を使う場合でも、このエディターがあると、Azure データ ファクトリを作成するために Azure PowerShell をインストールして起動する必要がなくなります。

## Data Factory エディターの起動
Data Factory エディターを起動するにはクリックして **作成者とデプロイ** タイルを **Data Factory** Azure data factory のブレードです。 

![タイルの作成とデプロイ][author-and-deploy-tile]

次の画像のように Data Factory エディターが表示されます。
 
![Data Factory エディター][data=factory-editor]
 
## 新しいリンクされたサービス、データ セット、パイプラインの作成
ツール バーには、Azure Data Factory のエンティティ作成に使用できる 4 つのボタンがあります。
 
- **新しいデータ ストア** ストアのリンクされたサービスをデータを作成します。 このボタンをクリックすると、Azure Storage、Azure SQL Database、オンプレミス SQL Server データベースがメニューにオプションとして表示されます。
- **新しいコンピューティング** 計算を作成するためのリンクされたサービスです。 このボタンをクリックすると、オンデマンド HDInsight クラスター、HDInsight クラスター、AzureML のリンクされたサービスがメニューにオプションとして表示されます。      
- **新しいデータセット** データセットを作成します。 このボタンをクリックすると、BLOB テーブル、Azure SQL テーブル、オンプレミス テーブルのオプションが表示されます。  
- **新しいパイプライン** パイプラインを作成します。 クリックして **.(省略記号)** ツールバーのツールバーで、このボタンが表示されない場合。
 
### ストレージのリンクされたサービスを作成するには
1. クリックして **新しいデータ ストア**, 、メニューのオプションのいずれかをクリックします。
 
    ![新しいデータ ストア メニュー][new-data-store-menu] 
2. [ストレージのリンクされたサービスを作成するための JSON テンプレートが表示されます、 **エディター キャンバス** 右側にします。 ドラフト ノードが表示されることもわかります **下書き**します。 以下の手順を実行します。
    1.  **Azure ストレージ**: 置換 **< accountname\ >** と **< accountkey > \** 名前と、Azure ストレージ アカウントのキーを使用します。
    2.  **Azure SQL データベース**: 置換 **< servername > \** 、Azure SQL サーバーの名前と **< databasename\ >** 、データベースの名前に置き換えます **< ユーザー名 \ > @< servername > \** 、ユーザーの名前と **< パスワード >** ユーザー アカウントのパスワードを使用しています。 
    3.  **内部設置型 SQL server データベース**: 置換 **< servername > \** 、内部設置型 SQL server の名前の **< databasename\ >** 、データベースの名前に置き換えます **< ユーザー名 \ >** 、ユーザーの名前と **< パスワード >** ユーザー アカウントのパスワードを使用しています。
4. をクリックして **展開** 、ツールバーのリンク サービスをデプロイします。 クリックして **破棄** と JSON ドラフトを作成します。
 
    ![デプロイ ボタン][deploy-button]

1. タイトル バーには、デプロイ操作のステータスが表示されます。

    ![デプロイの成功メッセージ][deploy-success-message]
2. デプロイ操作が成功した場合、作成済みのリンクされたサービスが左側のツリー ビューに表示されます。
  
    ![ツリー ビューの StorageLinkedService][storagelinkedservice-in-treview]

### コンピューティングのリンクされたサービスを作成するには
1. クリックして **新しいコンピューティング** メニューのオプションのいずれかをクリックします。
 
    ![新しいコンピューティング メニュー][new-compute-menu] 
2. 右側の [エディター キャンバス] 内に、コンピューティングのリンクされたサービスを作成するための JSON テンプレートが表示されます。 以下の手順を実行します。
    1.  **、オンデマンド HDInsight クラスター**, 、次のプロパティの値を指定します。 
        1.  **ClusterSize** プロパティには、Data Factory サービスは、実行時に作成する HDInsight クラスターのサイズを指定します。 
        3.  **TimeToLive** プロパティには、HDInsight クラスターが削除されるまでの許容アイドル時間を指定します。 たとえば 00:05:00 と指定すると、アイドル時間の 5 分が経過した後にクラスターが削除されることになります。
        4.  **バージョン** プロパティには、クラスターの HDInsight のバージョンの指定 (既定: version 3.1)。
        5.  **LinkedServiceName** プロパティには、HDInsight クラスターに関連付けられる Azure ストレージのリンクされたサービスを指定します。 
    6.  **HDInsight クラスター** 、次のプロパティの値を指定 (Bring your-独自)。
        1.  **ClusterUri** プロパティには、HDInsight クラスターの URL を指定します。 
        2.  **UserName** プロパティには、Data Factory サービスが、HDInsight クラスターへの接続に使用するユーザー アカウントを指定します。 
        3.  **パスワード** プロパティには、ユーザー アカウントのパスワードを指定します。 
        4.  **LinkedServiceName** プロパティには、HDInsight クラスターに関連付けられている Azure ストレージのリンクされたサービスを指定します。
    5.  **AzureML のリンクされたサービス**, 、次のプロパティを指定します。
        1.  **mlEndPoint** プロパティには、Azure Machine Learning バッチ スコアリング URL を指定します。
        2.  **apiKey** プロパティには、発行されたワークスペース モデルの API キーを指定します。
3. をクリックして **展開** 、ツールバーのリンク サービスをデプロイします。

> [AZURE.NOTE] 参照してください、 [リンクされたサービス][msdn-linkedservices-reference] リンクされたサービスの説明については、Azure Data Factory の定義に使用される JSON 要素の MSDN ライブラリのトピック.  

### 新しいデータセットを作成するには
1. クリックして **新しいデータセット** メニューのオプションのいずれかをクリックします。
2. 右側の [エディター キャンバス] 内に、データセットを作成するための JSON テンプレートが表示されます。 以下の手順を実行します。 
    1.  **Blob テーブル**, 、次のプロパティの値を指定します。
    2.  **Azure SQL テーブル** または **オンプレミス テーブル**, 、次のプロパティの値を指定します。 
        1.  **場所** セクション。 
            2.  **LinkedServiceName** プロパティには、サービス、Azure SQL またはオンプレミスの SQL Server データベースには参照をリンクの名前を指定します。
            2.  **TableName** プロパティには、Azure SQL Database インスタンスまたはオンプレミスの SQL server のテーブルの名前を指定するには、リンクされたサービスを参照します。
        3.  **可用性** セクション。
            1.  **頻度** プロパティには、データ スライス生成の時間単位を指定します。 サポートされている頻度値は、Minute、Hour、Day、Week、Month です。
            2.  **間隔** プロパティには、定義された頻度で間隔を指定します。 **頻度** に設定 **時間** と **間隔** 設定 **1** 新しいデータ スライスは 1 時間ごと生成されることを示します。 
        3.  **構造** セクション。 
            1. 次の例に示すように、列の名前と型を指定します。
                
                    "structure":
                    [
                        { "name": "FirstName", "type": "String"},
                        { "name": "LastName", "type": "String"}
                    ],
         
> [AZURE.NOTE] 参照してください、 [テーブル][msdn-tables-reference] Azure Data Factory テーブルの定義に使用される JSON 要素の説明については、MSDN ライブラリのトピックです。  
                   
### パイプラインを作成してアクティブ化するには 
1. クリックして **新しいパイプライン** ツールバーです。 表示されない場合、 **新しいパイプライン** ] ボタン、[ **.(省略記号)** 表示されます。   
2. 右側の [エディター キャンバス] 内に、パイプラインを作成するための JSON テンプレートが表示されます。 以下の手順を実行します。 
    1.  **説明** プロパティには、パイプラインの説明を指定します。
    2.  **アクティビティ** セクションで、パイプラインにアクティビティを追加します。 例:
     
            "activities":   
            [
                {
                    "name": "CopyFromBlobToSQL",
                    "description": "Push Regional Effectiveness Campaign data to Azure SQL",
                    "type": "CopyActivity",
                    "inputs": [ {"name": "EmpTableFromBlob"} ],
                    "outputs": [ {"name": "EmpSQLTable"} ],     
                    "transformation":
                    {
                        "source":
                        {                               
                            "type": "BlobSource"
                        },
                        "sink":
                        {
                            "type": "SqlSink"
                        }   
                    },
                    "Policy":
                    {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "style": "StartOfInterval",
                        "retry": 0,
                        "timeout": "01:00:00"
                    }       
                }
            ]
    3.  **開始** プロパティには、データ処理が開始されるか、データ スライスの処理日時を指定します。 例: 2014-05-01T00:00:00Z。
    4.  **エンド** プロパティには、データ処理の終了日時を指定します。 例: 2014-05-01T00:00:00Z。       

> [AZURE.NOTE] 参照してください、 [パイプラインとアクティビティ][msdn-pipelines-reference] MSDN ライブラリの「Azure Data Factory パイプラインを定義するために使用される JSON 要素の説明をします。

## パイプライン JSON にアクティビティ定義を追加するには
クリックして、パイプラインの JSON にアクティビティ定義を追加することができます **[アクティビティの追加** ツールバーです。 このボタンをクリックしてから、パイプラインに追加するアクティビティの種類を選択します。  

![アクティビティの追加オプション][add-activity-options]

かどうかに、Azure SQL データベースからデータを Azure blob ストレージにコピーし、HDInsight クラスターで Pig スクリプトを使用して、blob ストレージにデータを処理する、最初に追加する、 **コピー アクティビティ** し、追加、 **Pig アクティビティ** パイプラインにします。 これは、セクションを作成します 2 つのセクションで、アクティビティで、パイプライン JSON のです。 Pig アクティビティとは、Pig 変換を含む HDInsight アクティビティにすぎません。 

    "activities": [
        {
            "name": "CopyFromTabletoBlob",
            "type": "CopyActivity",
            ...
        }
        {
            "name": "ProcessBlobDataWithPigScript",
            "type": "HDInsightActivity",
            ...
            "transformation": {
                "type": "Pig",
                ...
            }
        }
    ]

## パイプラインの開始
JSON の start および end プロパティの値を指定することにより、パイプラインの開始日時と終了日時を指定できます。 

    {
        "name": "ADFTutorialPipeline",
        "properties":
        {   
            "description" : "Copy data from a blob to Azure SQL table",
            "activities":   
            [
                ...
            ],
    
            "start": "2015-02-13T00:00:00Z",
            "end": "2015-02-14T00:00:00Z",
            "isPaused": false
        }
    } 
  
## エディター内のドラフト
ドラフトを使用すると、コンテキストを切り替える場合や、Data Factory 内の異なるエンティティに移動する場合に、一時的に作業を保存できます。 ドラフトの有効期間は、ブラウザー セッションに関連付けられています。 ブラウザーを閉じるか、別のコンピューターを使用すると、ドラフトは使用できなくなります。

## Data Factory エンティティの JSON ドラフトを破棄するには
クリックして、Azure Data Factory エンティティの JSON 定義を破棄する **破棄** ツールバーのボタンをクリックします。   

## Data Factory エンティティを複製するには
既存の Azure Data Factory エンティティ (リンクされたサービス、テーブル、またはパイプライン) を複製するには、ツリー ビューでエンティティを選択し、クリックすると、 **複製** ツールバーのボタンをクリックします。

![データ ファクトリのエンティティの複製][clone-datafactory-entity]

作成された新しいドラフトが表示されます **下書き** ツリー ビュー内のノードです。 

## Data Factory のエンティティを削除するには
Azure Data Factory エンティティ (リンクされたサービス、テーブル、またはパイプライン) を削除、ツリー ビューでエンティティを選択し、をクリックして **削除** ツールバーのエンティティを右クリックし、クリックして (または) **削除**します。

![データ ファクトリのエンティティの削除][delete-datafactory-entity] 

## 関連項目
参照してください、 [Azure Data Factory を使ってみる][data-factory-get-started] Data Factory エディターを使用して、Azure data factory を作成する手順の詳細についてのトピックです。 

[msdn-tables-reference]: https://msdn.microsoft.com/library/dn835002.aspx
[msdn-linkedservices-reference]: https://msdn.microsoft.com/library/dn834986.aspx       
[msdn-pipelines-reference]: https://msdn.microsoft.com/library/dn834988.aspx  

[data-factory-get-started]: data-factory-get-started.md

[author-and-deploy-tile]: ./media/data-factory-editor/author-and-deploy-tile.png
[data=factory-editor]: ./media/data-factory-editor/data-factory-editor.png
[new-data-store-menu]: ./media/data-factory-editor/new-data-store-menu.png
[new-compute-menu]: ./media/data-factory-editor/new-compute-menu.png
[deploy-button]: ./media/data-factory-editor/deploy-button.png
[deploy-success-message]: ./media/data-factory-editor/deploy-success-message.png
[storagelinkedservice-in-treview]: ./media/data-factory-editor/storagelinkedservice-in-treeview.png
[delete-datafactory-entity]: ./media/data-factory-editor/delete-datafactory-entity.png
[clone-datafactory-entity]: ./media/data-factory-editor/clone-datafactory-entity.png
[add-activity-options]: ./media/data-factory-editor/add-activity-options.png 
