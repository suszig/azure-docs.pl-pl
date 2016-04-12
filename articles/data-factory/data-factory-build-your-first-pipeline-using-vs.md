<properties
    pageTitle="Visual Studio を使用した初めての Azure Data Factory パイプラインの作成"
    description="このチュートリアルでは、Visual Studio を使用して、サンプルの Azure Data Factory パイプラインを作成します。"
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
    ms.date="11/02/2015"
    ms.author="spelluru"/>

# Visual Studio を使用した初めての Azure Data Factory パイプラインの作成
> [AZURE.SELECTOR]
- [チュートリアルの概要](data-factory-build-your-first-pipeline.md)
- [Data Factory エディターの使用](data-factory-build-your-first-pipeline-using-editor.md)
- [PowerShell の使用](data-factory-build-your-first-pipeline-using-powershell.md)
- [Visual Studio の使用](data-factory-build-your-first-pipeline-using-vs.md)
- [Resource Manager テンプレートの使用](data-factory-build-your-first-pipeline-using-arm.md)


この記事では、Visual Studio を使用して最初のパイプラインを作成する方法を学習します。 このチュートリアルの手順は次のとおりです。

2.  リンクされたサービス (データ ストア、計算) を作成する。
3.  データセットを作成する。
3.  パイプラインを作成する。
4.  Data Factory を作成し、リンクされたサービス、データセット、パイプラインをデプロイする。 

この記事では、Azure Data Factory サービスの概念については説明しません。 サービスの詳細については、次を参照してください。、 [Azure Data Factory の概要](data-factory-introduction.md) 記事です。

> [AZURE.IMPORTANT] 参照してください、 [チュートリアル概要](data-factory-build-your-first-pipeline.md) 記事し、このチュートリアルを実行する前に、前提条件の手順を実行します。  

## チュートリアル: Visual Studio を使用して Data Factory のエンティティを作成してデプロイする 

### 前提条件

コンピューターに以下がインストールされている必要があります。 

- Visual Studio 2013 または Visual Studio 2015
- Azure SDK for Visual Studio 2013 または Visual Studio 2015 をダウンロードします。 移動 [Azure ダウンロード ページ](http://azure.microsoft.com/downloads/) ] をクリック **VS 2013** または **VS 2015** で、 **.NET** セクションです。
- Visual Studio の最新の Data Factory の Azuer プラグインをダウンロード: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) または [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005)します。 Visual Studio 2013 を使用している場合は、更新することも、プラグイン、次の手順に従って: メニューで、次のようにクリックします。 **ツール** ]-> [ **拡張機能と更新プログラム** ]-> [ **オンライン** ]-> [ **Visual Studio ギャラリー** ]-> [ **Visual Studio の Microsoft Azure Data Factory のツール** ]-> [ **更新**します。 
    
    

### Visual Studio プロジェクトの作成 
1. 起動 **Visual Studio 2013** または **Visual Studio 2015**します。 クリックして **ファイル**, 、] をポイント **新規**, 、] をクリック **プロジェクト**します。 確認する必要があります、 **新しいプロジェクト** ] ダイアログ ボックス。  
2.  **新しいプロジェクト** ダイアログ ボックスで、選択、 **DataFactory** テンプレート、およびクリック **Empty Data Factory Project**します。   

    ![[新しいプロジェクト] ダイアログ ボックス](./media/data-factory-build-your-first-pipeline-using-vs/new-project-dialog.png)

3. 入力、 **名前** 、プロジェクトの **場所**, との名前、 **ソリューション**, 、] をクリック **[ok]**します。

    ![ソリューション エクスプローラー](./media/data-factory-build-your-first-pipeline-using-vs/solution-explorer.png)

### リンクされたサービスの作成
この手順では、Azure ストレージ アカウントとオンデマンド Azure HDInsight クラスターをデータ ファクトリにリンクした後、Hive 処理からの出力データを表すデータセットを作成します。


#### Azure Storage のリンクされたサービスを作成する


4. 右クリック **リンクされたサービス** にあるソリューション エクスプ ローラーで、 **追加**, 、] をクリック **[新しい項目の**です。      
5.  **新しい項目の追加** ダイアログ ボックスで、 **Azure Storage Linked Service** ] をクリックし、一覧から **追加**します。 

    ![新規のリンクされたサービス](./media/data-factory-build-your-first-pipeline-using-vs/new-linked-service-dialog.png)
 
3. 置換 **accountname** と **accountkey** 、Azure ストレージ アカウントとそのキーの名前に置き換えます。 

    ![Azure Storage のリンクされたサービス](./media/data-factory-build-your-first-pipeline-using-vs/azure-storage-linked-service.png)

4. 保存、 **AzureStorageLinkedService1.json** ファイルです。

#### Azure HDInsight のリンクされたサービスを作成する
次に、Hive スクリプトの実行に使用するオンデマンド HDInsight クラスター用のリンクされたサービスを作成します。 

1.  **ソリューション エクスプ ローラー**, を右クリックして **リンクされたサービス**, 、] をポイント **追加**, 、] をクリック **[新しい項目の**です。
2. 選択 **HDInsight On Demand Linked Service**, 、] をクリック **追加**します。 
3. 置き換える、 **JSON** 伴って。

        {
          "name": "HDInsightOnDemandLinkedService",
          "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
              "version": "3.2",
              "clusterSize": 1,
              "timeToLive": "00:30:00",
              "linkedServiceName": "AzureStorageLinkedService1"
            }
          }
        }
    
    次の表に、このスニペットで使用される JSON プロパティの説明を示します。
    
    プロパティ | 説明
    -------- | -----------
    バージョン | 作成された HDInsight のバージョンが 3.2 になるように指定します。 
    ClusterSize | 1 ノードの HDInsight クラスターを作成します。 
    TimeToLive | 削除されるまでの HDInsight クラスターのアイドル時間を指定します。
    linkedServiceName | HDInsight によって生成されるログを保存するために使用されるストレージ アカウントを指定します。

4. 保存、 **HDInsightOnDemandLinkedService1.json** ファイルです。
 
### 出力データセットを作成します。
次に、Azure BLOB ストレージに格納されるデータを表す出力データセットを作成します。 

1.  **ソリューション エクスプ ローラー**, 、するポイントを右クリックして **追加**, 、] をクリック **[新しい項目の**です。 
2. 選択 **Azure Blob** ] をクリックし、一覧から **追加**します。 
3. 置き換える、 **JSON** に次のエディターで: という名前のデータセットを作成するこの JSON スニペットで、 **AzureBlobOutput**, と Hive スクリプトによって生成されるデータの構造を指定します。 結果はという名前の blob コンテナーに格納されていることを指定するさらに、 **データ** という名前のフォルダーと **partitioneddata**します。  **可用性** セクションでは、出力データセットが 1 か月ごとに生成されることを指定します。
    
        {
          "name": "AzureBlobOutput",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
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

4. 保存、 **AzureBlobLocation1.json** ファイルです。


### 最初のパイプラインを作成する
この手順では、最初のパイプラインを作成します。

1.  **ソリューション エクスプ ローラー**, を右クリックして **パイプライン**, 、] をポイント **追加**, 、] をクリック **新しい項目の追加。** 
2. 選択 **Hive 変換パイプライン** ] をクリックし、一覧から **追加**します。 
3. 置き換える、 **JSON** 次のコードにします。

    > [AZURE.IMPORTANT] 置換 **storageaccountname** 、ストレージ アカウントの名前に置き換えます。

        {
          "name": "MyFirstPipeline",
          "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
              {
                "type": "HDInsightHive",
                "typeProperties": {
                  "scriptPath": "script/partitionweblogs.hql",
                  "scriptLinkedService": "AzureStorageLinkedService1",
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

    この JSON スニペットでは、Hive を使用して HDInsight クラスターのデータを処理する 1 つのアクティビティで構成されるパイプラインを作成します。
    
    Hive スクリプト ファイルは、 **partitionweblogs.hql**, は Azure ストレージ アカウントに格納 (と呼ばれる scriptLinkedService によって指定 **AzureStorageLinkedService1**)、およびという名前のコンテナーで **スクリプト**します。

     **定義** セクションを使用して、Hive 構成値 (例: ${hiveconf:partitioneddata}) として hive スクリプトに渡される実行時の設定を指定します。

     **開始** と **エンド** パイプラインのプロパティは、パイプラインのアクティブな期間を指定します。

    リンクされたサービスで指定されたコンピューティングで Hive スクリプトを実行するよう指定する、アクティビティ JSON で **HDInsightOnDemandLinkedService**します。
3. 保存、 **HiveActivity1.json** ファイルです。

### 依存関係として partitionweblogs.hql を追加する 

1. 右クリック **の依存関係** で、 **ソリューション エクスプ ローラー** ウィンドウで、指す **追加**, 、] をクリック **既存項目の**です。  
2. 移動し、 **C:\ADFGettingStarted** 選択 **partitionweblogs.hql** ファイルを開き、をクリックして **追加**します。 

次の手順でソリューションを発行すると、Blob Storage のスクリプト コンテナーに HQL ファイルがアップロードされます。  

### Data Factory エンティティの発行/デプロイ

18. ソリューション エクスプ ローラーでプロジェクトを右クリックし、クリックして **発行**します。 
19. 表示される場合 **Microsoft アカウントにサイン イン** ダイアログ ボックスで、Azure サブスクリプションを持っているアカウントの資格情報の入力を **サインイン**します。
20. 次のダイアログ ボックスが表示されます。

    ![[発行] ダイアログ ボックス](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)

21. Data Factory の構成ページで、次の操作を行います。 
    1. 選択 **新しいデータ ファクトリの作成** オプション。
    2. 入力 **FirstPipelineUsingVS** の **名前**します。 
    
        > [AZURE.IMPORTANT] Azure Data Factory の名前はグローバルに一意である必要があります。 エラーが発生する場合 **データ ファクトリ名"FirstPipelineUsingVS"は使用できません** を発行する場合は、名前 (たとえば、yournameFirstPipelineUsingVS) を変更します。 参照してください [Data Factory - 名前付け規則](data-factory-naming-rules.md) Data Factory アーティファクトの名前付け規則についてのトピックです。 
        > 
        > データ ファクトリの名前は今後、DNS 名として登録される可能性があるため、一般ユーザーに表示される場合があります。
    3.最適なサブスクリプションを選択して、 **サブスクリプション** フィールドです。 
    4.選択、 **リソース グループ** データ ファクトリを作成するためです。 
    5.選択、 **地域** data factory のです。 
    6.をクリックして **次** に切り替える、 **項目の発行** ページです。 (キーを押して **タブ** 場合に、[名前] フィールドから移動する、 **次** ボタンが無効になります)。 
23.  **項目の発行** ] ページで、すべてのデータ ファクトリ エンティティが選択されていることを確認] をクリックして **次** に切り替える、 **概要** ページです。     
24. 概要を確認し、をクリックして **次** 展開プロセスとビューを起動する、 **展開ステータス**します。
25.  **展開ステータス** ] ページで、展開プロセスの状態を確認する必要があります。 デプロイメントが完了したら、[完了] をクリックします。 
 

## サーバー エクスプローラーを使用して Data Factory のエンティティを確認する

1.  **Visual Studio**, 、クリックして **ビュー** ] をクリックし、メニューの [ **サーバー エクスプ ローラー**します。
2. サーバー エクスプ ローラー] ウィンドウで、 **Azure** 展開 **Data Factory**します。 表示される場合 **Visual Studio にサインイン**, 、入力、 **アカウント** をクリックして、Azure サブスクリプションに関連付けられている **続行**します。 入力 **パスワード**, 、] をクリック **サインイン**します。 Visual Studio は、サブスクリプション内のすべての Azure データ ファクトリに関する情報を取得しようとします。 この操作のステータスを確認、 **Data Factory Task List** ウィンドウです。

    ![Server Explorer](./media/data-factory-build-your-first-pipeline-using-vs/server-explorer.png)
3. [Data factory を右クリックして選択できます **を新しいプロジェクトのデータ ファクトリのエクスポート** 既存のデータ ファクトリに基づいて Visual Studio プロジェクトを作成します。

    ![データ ファクトリのエクスポート](./media/data-factory-build-your-first-pipeline-using-vs/export-data-factory-menu.png)

## Visual Studio の Data Factory ツールを更新する

Visual Studio の Azure Data Factory ツールを更新するには、次のように行います。

1. クリックして **ツール** ] を選択し、メニューに **拡張機能と更新プログラム**します。
2. 選択 **更新** クリックして左ペインで **Visual Studio ギャラリー**します。
3. 選択 **Visual Studio の Azure Data Factory ツール** ] をクリック **更新**します。 このエントリが表示されない場合は、ツールは既に最新バージョンです。 

参照してください [データセットとパイプラインを監視](data-factory-monitor-manage-pipelines.md) このチュートリアルで作成した、Azure ポータルを使用して、パイプラインとデータセットを監視する方法についてです。
 

## 次のステップ
この記事では、オンデマンド HDInsight クラスターで Hive スクリプトを実行する変換アクティビティ (HDInsight アクティビティ) を含むパイプラインを作成しました。 Azure Blob から Azure SQL にデータをコピーするコピー アクティビティを使用する方法を確認するには、次を参照してください。 [チュートリアル: Azure SQL に blob、Azure からのデータをコピー](data-factory-get-started.md)します。
  


