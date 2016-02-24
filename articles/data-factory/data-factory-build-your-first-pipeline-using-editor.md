<properties
    pageTitle="Data Factory Editor を使用した初めての Azure Data Factory パイプラインの作成"
    description="このチュートリアルでは、Azure ポータルで Data Factory Editor を使用して、サンプルの Azure Data Factory パイプラインを作成します。"
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

# Data Factory Editor (Azure ポータル) を使用した初めての Azure Data Factory パイプラインの作成
> [AZURE.SELECTOR]
- [チュートリアルの概要](data-factory-build-your-first-pipeline.md)
- [Data Factory エディターを使用します。](data-factory-build-your-first-pipeline-using-editor.md)
- [PowerShell の使用](data-factory-build-your-first-pipeline-using-powershell.md)
- [Visual Studio を使用](data-factory-build-your-first-pipeline-using-vs.md)
- [リソース マネージャー テンプレートを使用します。](data-factory-build-your-first-pipeline-using-arm.md)


この記事では、使用する方法を説明します、 [Azure ポータル](https://portal.azure.com/) 最初のパイプラインを作成します。 このチュートリアルの手順は次のとおりです。

1.  データ ファクトリを作成する
2.  リンクされたサービス (データ ストア、計算) とデータセットを作成する
3.  パイプラインを作成する

この記事では、Azure Data Factory サービスの概念については説明しません。 サービスの詳細については、次を参照してください。、 [Azure Data Factory の概要](data-factory-introduction.md) 記事です。

> [AZURE.IMPORTANT] 参照してください、 [チュートリアル概要](data-factory-build-your-first-pipeline.md) 記事し、このチュートリアルを実行する前に、前提条件の手順を実行します。  

## 手順 1: データ ファクトリを作成する

1.  ログインした後、 [Azure ポータル](http://portal.azure.com/), 、次の操作を行います。
    1.  クリックして **新規** 左側のメニュー。 
    2.  クリックして **データ分析** で、 **作成** ブレードです。
    3.  クリックして **Data Factory** 上、 **データ分析** ブレードです。

        ![[作成] ブレード](./media/data-factory-build-your-first-pipeline-using-editor/create-blade.png)

2.   **新しいデータ ファクトリ** ブレードで、入力 **DataFactoryMyFirstPipeline** 名前にします。

    ![[新しいデータ ファクトリ] ブレード](./media/data-factory-build-your-first-pipeline-using-editor/new-data-factory-blade.png)

    > [AZURE.IMPORTANT] Azure データ ファクトリの名前はグローバルに一意である必要があります。 エラーが発生する場合: **データ ファクトリ名"DataFactoryMyFirstPipeline"は使用できません**, 、data factory (たとえば、yournameDataFactoryMyFirstPipeline) の名前を変更し、もう一度作成してください。 参照してください [Data Factory - 名前付け規則](data-factory-naming-rules.md) Data Factory アーティファクトの名前付け規則についてのトピックです。
    >  
    > データ ファクトリの名前は今後、DNS 名として登録される可能性があるため、一般ユーザーに表示される場合があります。

3.  任意のリソース グループを作成していない場合は、リソース グループを作成する必要があります。 これを行うには、次の手順を実行します。
    1.  をクリックして **リソース グループ名**します。
    2.  選択 **新しいリソース グループを作成する** で、 **リソース グループ** ブレードです。
    3.  入力 **ADF** の **名前** で、 **リソース グループの作成** ブレードです。
    4.  Click **OK**.
    
        ![リソース グループの作成](./media/data-factory-build-your-first-pipeline-using-editor/create-resource-group.png)
4.  リソース グループを選択した後、データ ファクトリを作成する正しいサブスクリプションを使用していることを確認します。
5.  をクリックして **作成** 上、 **新しいデータ ファクトリ** ブレードです。
6.  作成されているデータ ファクトリが表示されます、 **スタート画面** の次のように、Azure ポータル。   

    ![Data factory を作成中の状態](./media/data-factory-build-your-first-pipeline-using-editor/creating-data-factory-image.png)
7. ご利用ありがとうございます。 これで、最初のデータ ファクトリが正常に作成されました。 データ ファクトリが正常に作成されると、データ ファクトリの内容を表示するデータ ファクトリ ページが表示されます。  

    ![[Data Factory] ブレード](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-blade.png)

以降の手順では、このチュートリアルで使用するリンクされたサービス、データセット、およびパイプラインを作成する方法を説明します。 

## 手順 2: リンクされたサービスとデータセットを作成する
この手順では、Azure ストレージ アカウントとオンデマンド Azure HDInsight クラスターをデータ ファクトリにリンクした後、Hive 処理からの出力データを表すデータセットを作成します。

### Azure Storage のリンクされたサービスを作成する
1.  をクリックして **作成者および展開** 上、 **DATA FACTORY** ブレード **DataFactoryFirstPipeline**します。 Data Factory エディタが起動します。 
     
    ![[作成とデプロイ] タイル](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-author-deploy.png)
2.  クリックして **新しいデータ ストア** 選択 **Azure ストレージ**
    
    ![Azure Storage のリンクされたサービス](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)

    Azure Storage のリンクされたサービスを作成するための JSON スクリプトがエディターに表示されます。 
4. 置換 **アカウント名** を Azure ストレージ アカウントの名前と **アカウント キー** Azure ストレージ アカウントのアクセス キーを持つ。 ストレージ アクセス キーを取得する方法については、次を参照してください [表示、コピーおよび再生成するストレージ アクセス キー。](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)
5. をクリックして **展開** 、コマンド バー、リンクされたサービスをデプロイします。

    ![デプロイ ボタン](./media/data-factory-build-your-first-pipeline-using-editor/deploy-button.png)

### Azure HDInsight のリンクされたサービスを作成する
次に、Hive スクリプトの実行に使用するオンデマンド HDInsight クラスター用のリンクされたサービスを作成します。 

1.  **Data Factory エディター**, をクリックして **新しいコンピューティング** 選択し、コマンド バー **、オンデマンド HDInsight クラスター**します。

    ![新しいコンピューティング](./media/data-factory-build-your-first-pipeline-using-editor/new-compute-menu.png)
2. 次のスニペットをコピーして、[Draft-1] ウィンドウに貼り付けます。 この JSON スニペットは、HDInsight クラスターをオンデマンドで作成するために使用されるプロパティを記述します。 

        {
          "name": "HDInsightOnDemandLinkedService",
          "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
              "version": "3.2",
              "clusterSize": 1,
              "timeToLive": "00:30:00",
              "linkedServiceName": "StorageLinkedService"
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
3. をクリックして **展開** 、コマンド バー、リンクされたサービスをデプロイします。 
4. StorageLinkedService と HDInsightOnDemandLinkedService が両方とも左側のツリー ビューに表示されていることを確認します。

    ![リンクされたサービスを表示しているツリー ビュー](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-linked-services.png)
 
### 出力データセットを作成します。
次に、Azure BLOB ストレージに格納されるデータを表す出力データセットを作成します。 

1.  **Data Factory エディター**, 、クリックして **新しいデータセット** 選択し、コマンド バー **Azure Blob ストレージ**します。  

    ![新しいデータセット](./media/data-factory-build-your-first-pipeline-using-editor/new-data-set.png)
2. 次のスニペットをコピーして、[Draft-1] ウィンドウに貼り付けます。 この JSON スニペットでは、という名前のデータセットを作成する **AzureBlobOutput**, と Hive スクリプトによって生成されるデータの構造を指定します。 結果はという名前の blob コンテナーに格納されていることを指定するさらに、 **データ** という名前のフォルダーと **partitioneddata**します。  **可用性** セクションでは、出力データセットが 1 か月ごとに生成されることを指定します。
    
        {
          "name": "AzureBlobOutput",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
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

3. をクリックして **展開** 、コマンド バー、新しく作成したデータセットをデプロイします。
4. データセットが正常に作成されたことを確認します。

    ![リンクされたサービスを表示しているツリー ビュー](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-data-set.png)

## 手順 3: 最初のパイプラインを作成する
この手順では、最初のパイプラインを作成します。

1.  **Data Factory エディター**, 、クリックして **省略符号 (...)** ] をクリックし、 **新しいパイプライン**します。
    
    ![[新しいパイプライン] ボタン](./media/data-factory-build-your-first-pipeline-using-editor/new-pipeline-button.png)
2. 次のスニペットをコピーして、[Draft-1] ウィンドウに貼り付けます。

    > [AZURE.IMPORTANT] 置換 **storageaccountname** 、JSON でストレージ アカウントの名前に置き換えます。

        {
          "name": "MyFirstPipeline",
          "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
              {
                "type": "HDInsightHive",
                "typeProperties": {
                  "scriptPath": "script/partitionweblogs.hql",
                  "scriptLinkedService": "StorageLinkedService",
                  "defines": {
                    "partitionedtable": "wasb://data@<storageaccountname>.blob.core.windows.net/partitioneddata"
                  }
                },
                "outputs": [
                  {
                    "name": "AzureBlobOutput"
                  }
                ],
                "scheduler": {
                    "frequency": "Month",
                    "interval": 1
                },
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
    
    Hive スクリプト ファイルは、 **partitionweblogs.hql**, は Azure ストレージ アカウントに格納 (と呼ばれる scriptLinkedService によって指定 **StorageLinkedService**)、およびという名前のコンテナーで **スクリプト**します。

     **定義** セクションを使用して、Hive 構成値 (例: ${hiveconf:partitioneddata}) として hive スクリプトに渡される実行時の設定を指定します。

     **開始** と **エンド** パイプラインのプロパティは、パイプラインのアクティブな期間を指定します。

    リンクされたサービスで指定されたコンピューティングで Hive スクリプトを実行するよう指定する、アクティビティ JSON で **HDInsightOnDemandLinkedService**します。
3. をクリックして **展開** 、コマンド バー、パイプラインをデプロイします。
4. ツリー ビューにパイプラインが表示されることを確認します。

    ![パイプラインを表示しているツリー ビュー](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-pipeline.png)
5. これで、最初のパイプラインが正常に作成されました。
6. クリックして **X** Data Factory エディターのブレードを閉じ、Data Factory] ブレードに戻ると、をクリックする **ダイアグラム**します。
  
    ![Diagram tile](./media/data-factory-build-your-first-pipeline-using-editor/diagram-tile.png)
7. ダイアグラム ビューに、パイプラインの概要と、このチュートリアルで使用されるデータセットが表示されます。
    
    ![Diagram View](./media/data-factory-build-your-first-pipeline-using-editor/diagram-view-2.png) 
8. ダイアグラム ビューで、データセットをダブルクリック **AzureBlobOutput**します。 現在処理中のスライスが表示されます。

    ![Dataset](./media/data-factory-build-your-first-pipeline-using-editor/dataset-blade.png)
9. 処理を完了すると、スライスが表示されます **準備** 状態です。 オンデマンド HDInsight クラスターの作成には通常しばらく時間がかかることに注意してください。 

    ![Dataset](./media/data-factory-build-your-first-pipeline-using-editor/dataset-slice-ready.png) 
10. スライスが **準備** チェックの状態、 **partitioneddata** 内のフォルダー、 **データ** 出力データの blob ストレージ内のコンテナーです。  
 

 

## 次のステップ
この記事では、オンデマンド HDInsight クラスターで Hive スクリプトを実行する変換アクティビティ (HDInsight アクティビティ) を含むパイプラインを作成しました。 Azure Blob から Azure SQL にデータをコピーするコピー アクティビティを使用する方法を確認するには、次を参照してください。 [チュートリアル: Azure SQL に blob、Azure からのデータをコピー](./data-factory-get-started.md)します。
  


