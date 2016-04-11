<properties 
    pageTitle="SQL Server ストアド プロシージャ アクティビティ" 
    description="SQL Server ストアド プロシージャ アクティビティを使用して、Data Factory パイプラインから Azure SQL Database または Azure SQL Data Warehouse でストアド プロシージャを呼び出す方法について説明します。" 
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
    ms.date="10/12/2015" 
    ms.author="spelluru"/>

# SQL Server ストアド プロシージャ アクティビティ

SQL Server ストアド プロシージャ アクティビティを使用するには Data Factory で [パイプライン](data-factory-create-pipelines.md) 内のストアド プロシージャを呼び出す、 **Azure SQL Database** または **Azure SQL Data Warehouse** します。 この記事に基づき、 [データ変換のアクティビティ](data-factory-data-transformation-activities.md) データ変換とサポートされる変換アクティビティの概要を説明する記事です。

## 構文
    {
        "name": "SQLSPROCActivity",
        "description": "description", 
        "type": "SqlServerStoredProcedure",
        "inputs":  [ { "name": "inputtable"  } ],
        "outputs":  [ { "name": "outputtable" } ],
        "typeProperties":
        {
            "storedProcedureName": "<name of the stored procedure>",
            "storedProcedureParameters":  
            {
                "param1": "param1Value"
                …
            }
        }
    }

## 構文の詳細

プロパティ | 説明 | 必須
-------- | ----------- | --------
name | アクティビティの名前 | はい
description | アクティビティの用途を説明するテキストです。 | いいえ
type | SqlServerStoredProcedure | はい
inputs | 続行するストアド プロシージャ アクティビティに使用できる状態 (’Ready’ 状態) である必要がある入力データセット。 ストアド プロシージャ アクティビティへの入力は、このアクティビティと他のアクティビティを関連付けるときの依存関係管理にのみ使用されます。 入力データセットは、ストアド プロシージャでパラメーターとして使用できません。 | いいえ
outputs | ストアド プロシージャ アクティビティで生成された出力データセット。 出力テーブルでは、必ず Azure SQL Database または Azure SQL Data Warehouse を Data Factory にリンクするリンク サービスを使用します。 ストアド プロシージャ アクティビティの出力は、後続の処理にストアド プロシージャ アクティビティの結果を渡すための手段として、およびこのアクティビティと他のアクティビティを関連付けるときの依存関係管理に使用できます。 | はい
storedProcedureName | Azure SQL データベースまたは出力テーブルで使用されるリンクされたサービスによって表される Azure SQL Data Warehouse でストアド プロシージャの名前を指定します。 | あり
storedProcedureParameters | ストアド プロシージャのパラメーター値を指定します | いいえ

## サンプルのチュートリアル

### サンプル テーブルとストアド プロシージャ
1. 次の作成 **テーブル** を SQL Server Management Studio またはその他の作業に慣れているツールを使用して Azure SQL Database。 datetimestamp 列は、対応する ID が生成された日付と時刻です。 

        CREATE TABLE dbo.sampletable
        (
            Id uniqueidentifier,
            datetimestamp nvarchar(127)
        )
        GO

        CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable(Id);
        GO

    Id は一意の識別し、datetimestamp 列は、日付と時刻に対応する ID が生成される場合。
    ![サンプル データ](./media/data-factory-stored-proc-activity/sample-data.png)

2. 次の作成 **ストアド プロシージャ** にデータを挿入する、 **sampletable**します。

        CREATE PROCEDURE sp_sample @DateTime nvarchar(127)
        AS
        
        BEGIN
            INSERT INTO [sampletable]
            VALUES (newid(), @DateTime)
        END

    > [AZURE.IMPORTANT] **名** と **大文字小文字の区別** パラメーター (この例では DateTime) のと同じパイプライン/アクティビティ JSON で指定されたパラメーター。 ストアド プロシージャの定義で **@** パラメーターにプレフィックスとして使用します。
    
### Data Factory を作成する。  
4. ログインした後、 [Azure ポータル](http://portal.azure.com/), 、次の操作を行います。
    1.  クリックして **新規** 左側のメニュー。 
    2.  クリックして **データ分析** で、 **作成** ブレードです。
    3.  クリックして **Data Factory** 上、 **データ分析** ブレードです。
4.   **新しいデータ ファクトリ** ブレードで、入力 **SProcDF** 名前にします。 Azure Data Factory の名前はグローバルで一意となります。 ファクトリを作成するには、データ ファクトリの名前の先頭にあなたの名前を付ける必要があります。 
3.  任意のリソース グループを作成していない場合は、リソース グループを作成する必要があります。 これを行うには、次の手順を実行します。
    1.  をクリックして **リソース グループ名**します。
    2.  選択 **新しいリソース グループを作成する** で、 **リソース グループ** ブレードです。
    3.  入力 **ADFTutorialResourceGroup** の **名前** で、 **リソース グループの作成** ブレードです。
    4.  Click **OK**.
4.  リソース グループを選択した後、データ ファクトリを作成する正しいサブスクリプションを使用していることを確認します。
5.  をクリックして **作成** 上、 **新しいデータ ファクトリ** ブレードです。
6.  作成されているデータ ファクトリが表示されます、 **スタート画面** Azure ポータルのです。 データ ファクトリが正常に作成されると、データ ファクトリの内容を表示するデータ ファクトリ ページが表示されます。

### Azure SQL のリンク サービスを作成する  
データ ファクトリの作成後、Azure SQL Database をデータ ファクトリに関連付ける Azure SQL リンク サービスを作成します。 これは、sampletable テーブルと sp_sample ストアド プロシージャを含んだデータベースです。

7.  をクリックして **作成者および展開** 上、 **DATA FACTORY** ブレード **SProcDF**します。 Data Factory エディタが起動します。 
2.  クリックして **新しいデータ ストア** コマンドの横棒グラフし、選択 **Azure SQL**します。 Azure SQL のリンク サービスを作成するための JSON スクリプトがエディターに表示されます。 
4. 置換 **servername** 、Azure SQL Database サーバーの名前で **databasename** テーブルとストアド プロシージャを作成したデータベースで **username@servername** をデータベースへのアクセスを持つユーザー アカウントでと **パスワード** ユーザー アカウントのパスワードを使用しています。 
5. をクリックして **展開** 、コマンド バー、リンクされたサービスをデプロイします。

### 出力データセットの作成
6. クリックして **新しいデータセット** 選択し、コマンド バー **Azure SQL**します。
7. 次の JSON スクリプトをコピーして JSON エディターに貼り付けます。

        {               
            "name": "sprocsampleout",
            "properties": {
                "type": "AzureSqlTable",
                "linkedServiceName": "AzureSqlLinkedService",
                "typeProperties": {
                    "tableName": "sampletable"
                },
                "availability": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
        }
7. をクリックして **展開** 、コマンド バー、データセットをデプロイします。 

### SqlServerStoredProcedure アクティビティでパイプラインを作成する
今度は、SqlServerStoredProcedure アクティビティでパイプラインを作成しましょう。
 
9. クリックして **.(省略記号)** コマンド バーで **新しいパイプライン**します。 
9. 次の JSON スニペットをコピーして貼り付けます。   **ストアド プロシージャ名** 設定 **sp_sample**します。 名前とパラメーターの大文字と小文字 **DateTime** ストアド プロシージャの定義内のパラメーターの大文字小文字の区別と名前が一致する必要があります。  

        {
            "name": "SprocActivitySamplePipeline",
            "properties": {
                "activities": [
                    {
                        "type": "SqlServerStoredProcedure",
                        "typeProperties": {
                            "storedProcedureName": "sp_sample",
                            "storedProcedureParameters": {
                                "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                            }
                        },
                        "outputs": [
                            {
                                "name": "sprocsampleout"
                            }
                        ],
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "SprocActivitySample"
                    }
                ],
                "start": "2015-01-02T00:00:00Z",
                "end": "2015-01-03T00:00:00Z",
                "isPaused": false
            }
        }
9. クリックして **展開** 、ツールバーのパイプラインをデプロイします。  

### パイプラインの監視

6. クリックして **X** Data Factory エディターのブレードを閉じ、Data Factory] ブレードに戻ると、をクリックする **ダイアグラム**します。
7. ダイアグラム ビューに、パイプラインの概要と、このチュートリアルで使用されるデータセットが表示されます。 
8. ダイアグラム ビューで、データセットをダブルクリック **sprocsampleout**します。 スライスが準備完了状態として表示されます。 スライスは 2015/01/02 から 2015/01/03 まで毎時生成されるため、スライス数は 24 となります。 
10. スライスがの場合に **準備ができて** 実行状態、 **選択 * sampledata から** に対してクエリを実行、Azure SQL データベースのデータが、ストアド プロシージャによって、テーブルに挿入されていることを確認します。

    ![出力データ](./media/data-factory-stored-proc-activity/output.png)

    参照してください [、パイプラインを監視](data-factory-monitor-manage-pipelines.md) 詳細については、Azure Data Factory パイプラインを監視します。  

> [AZURE.NOTE] 上記の例では、SprocActivitySample に入力がないです。 (つまり、アップ ストリーム アクティビティにこれを連結する場合 前の処理)、アップ ストリーム アクティビティの出力は、このアクティビティの入力として使用できます。  このような場合は、アップ ストリーム アクティビティが完了し、アップ ストリーム アクティビティの出力を使用できる状態 (Ready 状態) になるまでこのアクティビティは実行されません。 入力は、ストアド プロシージャ アクティビティのパラメーターとして直接使用できません。

## 静的な値を渡す 
次に、‘Document sample’ という静的値を含む ‘Scenario’ という別の列をテーブルに追加する例を考えてましょう。

![サンプル データ 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

    CREATE PROCEDURE sp_sample @DateTime nvarchar(127) , @Scenario nvarchar(127)
    
    AS
    
    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime, @Scenario)
    END

この処理を実行するには、ストアド プロシージャ アクティビティから Scenario パラメーターとその値を渡します。 上のサンプルの typeProperties セクションは次のようになります。

    "typeProperties":
    {
        "storedProcedureName": "sp_sample",
        "storedProcedureParameters": 
        {
            "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
            "Scenario": "Document sample"
        }
    }

