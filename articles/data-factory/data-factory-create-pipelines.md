<properties 
    pageTitle="パイプラインの作成" 
    description="Azure Data Factory のパイプラインについて、およびパイプラインを作成してデータを移動および変換して洞察を得るために使用できる情報を生成する方法について説明します" 
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
    ms.topic="article" y
    ms.date="12/08/2015" 
    ms.author="spelluru"/>

# パイプラインとアクティビティの概要
この記事では、Azure Data Factory のパイプラインとアクティビティの概要、およびそれらを利用して実際のシナリオやビジネスのためにエンド ツー エンドのデータ主導ワークフローを作成する方法を説明します。 この記事では、完了するいると想定しています、 [概要](data-factory-introduction.md) と [データセットの作成](data-factory-create-datasets.md) これより前の記事です。

## パイプラインとは
**パイプラインは、アクティビティの論理的なグループ**します。 パイプラインは、タスクを実行するユニットにアクティビティをグループ化するために使用されます。 パイプラインをより深く理解するには、最初にアクティビティを理解する必要があります。 

### アクティビティとは
アクティビティは、データに対して実行するアクションを定義します。 各アクティビティは、0 個以上 [データセット](data-factory-create-datasets.md) としての入力を出力として 1 つまたは複数のデータセットを生成します。 **アクティビティは、Azure Data Factory でのオーケストレーションの単位です。** 

たとえば、コピー アクティビティを使用して、データセット間のデータのコピーを調整できます。 同様に、Azure HDInsight クラスターに対して Hive クエリを実行する Hive アクティビティを使用して、データを変換および分析できます。 Azure Data Factory は、さまざまな [データ変換、分析](data-factory-data-transformation-activities.md), 、および [データ移動アクティビティ](data-factory-data-movement-activities.md)します。 また、カスタム .NET アクティビティを作成して独自のコードを実行することもできます。 

以下の 2 つのデータセットについて考えます。

**Azure SQL データセット**

テーブル "MyTable" には列 "timestampcolumn" が含まれ、データがデータベースに挿入されたときの日付と時刻を示します。 

    {
      "name": "AzureSqlInput",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "externalData": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
          }
        }
      }
    }

**Azure BLOB データセット** 

データは、時間の粒度で特定の日時を反映している BLOB のパスの新しい BLOB に 1 時間ごとにコピーされます。

    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
          "partitionedBy": [
            {
              "name": "Year",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyy"
              }
            },
            {
              "name": "Month",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "%M"
              }
            },
            {
              "name": "Day",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "%d"
              }
            },
            {
              "name": "Hour",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "%H"
              }
            }
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": "\t",
            "rowDelimiter": "\n"
          }
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }


下のパイプラインのコピー アクティビティは、Azure SQL から Azure Blob Storage にデータをコピーします。 1 時間ごとに入力データセットとして Azure SQL のテーブルを受け取り、"AzureBlobOutput" データセットによって表される Azure BLOB ストレージにデータを書き込みます。 出力データセットも 1 時間ごとです。 時間単位でデータをコピーする方法については、「スケジュールと実行」セクションを参照してください。 このパイプラインの有効期間は、"2015-01-01T08:00:00" から "2015-01-01T11:00:00" までの 3 時間です。 

**パイプライン:**
    
    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2015-01-01T08:00:00",
        "end":"2015-01-01T11:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
          {
            "name": "AzureSQLtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureSQLInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "SqlSource",
                "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
              },
              "sink": {
                "type": "BlobSink"
              }
            },
           "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
         ]
       }
    }

アクティビティがどういうものかわかったので、パイプラインに戻ります。
 
パイプラインは、アクティビティの論理的なグループです。 パイプラインは、タスクを実行するユニットにアクティビティをグループ化するために使用されます。 **パイプラインは、また、アクティビティのデプロイメントと管理の単位です。**たとえば、論理的に関連性のあるアクティビティを 1 つのパイプラインにして、まとめてアクティブ状態や一時停止状態にできます。 

アクティビティ間の依存関係を定義することで、パイプラインのあるアクティビティからの出力データセットを、同じまたは別のパイプラインの別のアクティビティの入力データセットにできます。  [スケジュールと実行](#scheduling-and-execution) セクションがこれを詳しく説明します。 

Azure Data Factory でパイプラインを作成する一般的な手順は次のとおりです。

1.  Data Factory を作成します (作成されていない場合)。
2.  データ ストアまたはコンピューティングごとにリンクされたサービスを作成します。
3.  入力データセットと出力データセットを作成します。
4.  上で定義したデータセットに使用するアクティビティでパイプラインを作成します。

![Data Factory のエンティティ](./media/data-factory-create-pipelines/entities.png)

パイプラインを定義する方法について詳しく説明します。

## パイプラインのしくみ  

パイプラインの一般的な構造は次のようになります。

    {
        "name": "PipelineName",
        "properties": 
        {
            "description" : "pipeline description",
            "activities":
            [
    
            ],
            "start": "<start date-time>",
            "end": "<end date-time>"
        }
    }

 **アクティビティ** セクションでは、1 つまたは複数のアクティビティを定義を持つことができます。 各アクティビティには、次のような最上位構造があります。

    {
        "name": "ActivityName",
        "description": "description", 
        "type": "<ActivityType>",
        "inputs":  "[]",
        "outputs":  "[]",
        "linkedServiceName": "MyLinkedService",
        "typeProperties":
        {
    
        },
        "policy":
        {
        }
        "scheduler":
        {
        }
    }

次の表では、アクティビティとパイプラインの JSON 定義で使用されるプロパティを説明します。

タグ | 説明 | 必須
--- | ----------- | --------
name | アクティビティまたはパイプラインの名前です。 アクションを表す名前を指定を行うには、アクティビティまたはパイプラインが構成されていること<br/><ul><li>最大文字数: 260</li><li>文字、数字、またはアンダー スコア (_) で始まり</li><li>次の文字は使用できません:"です"、"+","?"、"/"、"<",">"、"*"、"%"、"&"、":","\\"。</li></ul> | あり
description | アクティビティまたはパイプラインの用途を説明するテキスト | あり
type | アクティビティの種類を指定します。 参照してください、 [データ移動アクティビティ](data-factory-data-movement-activities.md) と [データ変換のアクティビティ](data-factory-data-transformation-activities.md) アクティビティのさまざまな種類のアーティクルです。 | あり
inputs | アクティビティによって使用される入力テーブル<p>1 つの入力テーブル<br/>「入力」: [{"name":"inputtable1"}]、</p><p>2 つのテーブル <br/>「入力」: [{"name":"inputtable1"}、{"name":"inputtable2"}]、</p> | あり
outputs | アクティビティで使用される出力テーブルです。<p>1 つの出力テーブル<br/>「出力」: [{"name":"outputtable1"}]、</p><p>2 つの出力テーブル<br/>「出力」: [{"name":"outputtable1"}、{"name":"outputtable2"}]、</p> | あり
linkedServiceName | アクティビティによって使用されるリンクされたサービスの名前です。 <p>アクティビティは、必要なコンピューティング環境にリンクするリンクされたサービスを指定する必要があります。</p> | HDInsight アクティビティ、および Azure Machine Learning バッチ スコアリング アクティビティに関する [はい] します。 <p>[いいえ] には、他のすべて</p>
typeProperties | TypeProperties セクションでプロパティは、アクティビティの型によって異なります。 各個別アクティビティの詳細については、この記事を参照してください。 | いいえ
policy | アクティビティの実行時の動作に影響するポリシーです。 指定されていない場合は、既定のポリシーが使用されます。 詳細については下にスクロールします。 | いいえ
start | 開始日時、パイプラインのです。 ある必要があります [ISO 形式](http://en.wikipedia.org/wiki/ISO_8601)します。 (例: 2014-10-14T16:32:41Z)。 <p>start プロパティと end プロパティで、パイプラインの有効期間を指定します。 出力スライスは、このアクティブ期間内でのみ生成されます。</p> | いいえ<p>End プロパティの値を指定する場合は、start プロパティの値を指定する必要があります。</p><p>開始および終了時刻は両方とも、パイプラインを作成するのには、空がパイプラインを実行するための有効期間の設定値をどちらも必要です。 開始および終了時刻を指定しない場合、パイプラインを作成するときに、セット AzureRmDataFactoryPipelineActivePeriod コマンドレットを使用して後でそれらを設定できます。</p>
end | パイプラインの日付と時刻を終了します。 ISO 形式で指定する必要があります。 例: 2014年-10-14T17:32:41Z <p>パイプラインを無期限に実行するには、end プロパティの値として 9999-09-09 を指定します。</p>| いいえ <p>start プロパティの値を指定する場合は、end プロパティの値を指定する必要があります</p><p>。ノートを参照してください、 **開始** プロパティです。</p>
isPaused | 場合は、パイプラインを true に設定は実行されません。 既定値 = false。 このプロパティを使用して、有効または無効にすることができます。 | いいえ 
スケジューラ | 「スケジューラ」プロパティを使用して、アクティビティの必要なスケジュールを定義します。 サブプロパティは下にあるものと同じ [データセットの availability プロパティ](data-factory-create-datasets.md#Availability)します。 | いいえ |   

### アクティビティの種類
Azure Data Factory は、さまざまな [データの移動](data-factory-data-movement-activities.md) と [データ変換](data-factory-data-transformation-activities.md) アクティビティ。

### ポリシー
ポリシーはアクティビティの実行時の動作に影響します。具体的には、テーブルのスライスがいつ処理されるかです。 次の表で詳細に説明します。

プロパティ | 使用できる値 | 既定値 | 説明
-------- | ----------- | -------------- | ---------------
同時実行 | Integer <p>最大値: 10</p> | 1 | アクティビティの同時実行の数です。<p>各種のスライスで発生することができる並列アクティビティ実行の数を決定します。 たとえば、アクティビティを実行する必要がある場合、大きな高い同時実行性を持つ、使用可能なデータのセットはデータ処理が速くなります。</p> 
executionPriorityOrder | と<p>OldestFirst</p> | OldestFirst | 処理されているデータ スライスの順序を決定します。<p>たとえばがある場合は、2 つのスライス (午後 4 時と午後 5 時のもう 1 つ) と、保留中の実行が両方とも。 executionPriorityOrder を NewestFirst に設定すると、午後 5 時のスライスが最初に処理されます。 同様にすると、executionPriorityORder を設定した場合、午後 4 時のスライス処理されます。</p> 
retry | Integer<p>最大値は 10 を指定できます。</p> | 3 | スライスのデータ処理が失敗としてマークされるまでの再試行回数。 データ スライスのアクティビティの実行は、指定された再試行回数まで再試行されます。 再試行は、障害発生後にできるだけ早く行われます。
timeout | TimeSpan | 00:00:00 | アクティビティのタイムアウト。 例: 00時 10分: 00 (タイムアウトが 10 分を意味します)<p>値が指定されていないか、または 0 の場合、タイムアウトは無限です。</p><p>スライスのデータ処理時間がタイムアウト値を超えている場合は、キャンセルすると、システムが、処理を再試行しようとします。 再試行の回数は、retry プロパティで指定します。 タイムアウトが発生すると、ステータスは TimedOut になります。</p>
delay | TimeSpan | 00:00:00 | スライスの開始のデータ処理までの遅延を指定します。<p>遅延時間が予想される実行時間を経過した後、データ スライスのアクティビティの実行が開始します。</p><p>例: 00時 10分: 00 (10 分の遅延を意味します)</p>
longRetry | Integer<p>最大値: 10</p> | 1 | 長い再試行の回数は、スライスの実行が失敗する前に試行します。<p>longRetry の試行は longRetryInterval の間隔でします。 再試行間隔の時間を指定する必要がある場合は、longRetry を使用します。 Retry と longRetry の両方が指定されている場合は、各 longRetry 試行に Retry 試行が含まおよび試行の最大数は Retry になります * longRetry します。</p><p>たとえば、次のように、次のアクティビティのポリシーにある場合。<br/>再試行: 3<br/>longRetry: 2<br/>longRetryInterval: 1時 00分: 00<br/></p><p>実行する 1 つだけのスライスが前提としています (ステータスは PendingExecution) アクティビティの実行には、毎回が失敗したとします。 最初に 3 つの連続する試行があります。 試行するたびには、スライスの状態は Retry になります。 最初の 3 つの試行の終了後、スライスのステータスは LongRetry になります。</p><p>時間 (つまり longRetryInteval の値) 後に、3 の連続する試行の別のセットがあります。 その後、スライスの状態は Failed になり、それ以上再試行は行われません。 そのため全体的な 6 つの試行が行われました。</p><p>注: いずれかの実行が成功すると、スライスの状態が準備完了になり、それ以上再試行が試行されます。</p><p>非確定的な時間に依存するデータが到着するか、環境全体がどのデータ処理が行われるそれの状況では、longRetry を使用できます。 1 つずつ再試行を行うこのような場合可能性がありに役立たないため、一定時間の結果、目的の出力にそうです。</p><p>注意: longRetry または longRetryInterval に大きい値を設定しないでください。 通常はこの軽視されているその他のシステムの懸案事項を示すもので値が大きいほど</p> 
longRetryInterval | TimeSpan | 00:00:00 | 長い再試行の間の遅延 

## パイプラインの作成と管理
Azure Data Factory は、(1 つ以上のアクティビティを含む) パイプラインを作成およびデプロイするさまざまなメカニズムを提供します。 

### Azure ポータルの使用

1. ログイン [Azure ポータル](https://portal.azure.com/)します。
2. パイプラインを作成する Azure Data Factory のインスタンスに移動します。
3. クリックして  **作成とデプロイ** タイルに、 **概要** レンズします。 
 
    ![[作成とデプロイ] タイル](./media/data-factory-create-pipelines/author-deploy-tile.png)

4. クリックして **新しいパイプライン** コマンド バーにします。 

    ![[新しいパイプライン] ボタン](./media/data-factory-create-pipelines/new-pipeline-button.png)

5. パイプライン JSON テンプレートを含むエディター ウィンドウが表示されます。

    ![パイプライン エディター](./media/data-factory-create-pipelines/pipeline-in-editor.png)

6. パイプラインの作成が完了したら後でクリックして **展開** 、コマンド バー、パイプラインをデプロイします。 

    **注:** 展開時に、Azure Data Factory サービスをいくつかの一般的な問題を修正する、いくつかの検証チェックを実行します。 エラーがある場合、対応する情報が表示されます。 修正処理を実行し、作成したパイプラインを再デプロイします。 エディターを使用してパイプラインを更新および削除できます。

### Visual Studio プラグインの使用
Visual Studio を使用して、パイプラインを作成して Azure Data Factory にデプロイできます。 詳細についてを参照してください [チュートリアル: Azure ストレージから Azure SQL (Visual Studio) にデータをコピー](data-factory-get-started-using-vs.md)します。

### Azure PowerShell の使用
Azure PowerShell を使用して、Azure Data Factory にパイプラインを作成できます。 たとえば、c:\DPWikisample.json のファイルでパイプライン JSON を定義しておきます。 次の例のように Azure Data Factory インスタンスにそれをアップロードできます。

    New-AzureRmDataFactoryPipeline -ResourceGroupName ADF -Name DPWikisample -DataFactoryName wikiADF -File c:\DPWikisample.json

このコマンドレットの詳細については、次を参照してください。 [新規 AzureRmDataFactoryPipeline コマンドレット](https://msdn.microsoft.com/library/mt619358.aspx)します。

### REST API の使用
REST API を使用してパイプラインを作成およびデプロイすることもできます。 このメカニズムを利用すると、プログラムでパイプラインを作成できます。 この詳細については、次を参照してください。 [を作成または更新パイプライン](https://msdn.microsoft.com/library/azure/dn906741.aspx)します。

### .NET SDK の使用
.NET SDK を使用してパイプラインを作成およびデプロイすることもできます。 このメカニズムを利用すると、プログラムでパイプラインを作成できます。 これについて」をご覧 [の作成、管理、およびプログラムを使用してデータ ファクトリを監視](data-factory-create-data-factories-programmatically.md)します。


## スケジュールと実行
パイプラインとアクティビティについては理解できました。 また、Azure Data Factory でのアクティビティの定義と概要も説明しました。 次に、実行方法を説明します。 

パイプラインは、開始時刻と終了時刻の間のみアクティブです。 開始時刻より前または終了時刻より後には実行されません。 パイプラインは、一時停止している場合、その開始時刻と終了時刻に関係なく実行されません。 パイプラインを実行するには、一時停止しないでください。 

実際には、実行されるのはパイプラインではありません。 実行されるのはパイプライン内のアクティビティです。 ただし、アクティビティはパイプラインの全体的なコンテキストで実行されます。 参照してください [スケジュールと実行](data-factory-scheduling-and-execution.md) Azure Data Factory でのスケジュールと実行のしくみを理解します。

## 管理と監視  
パイプラインをデプロイした後は、パイプライン、スライス、実行を管理および監視できます。 詳細については、ここで: [モニターおよび管理のパイプライン](data-factory-monitor-manage-pipelines.md)します。

## 次のステップ

- 理解 [スケジュールと実行 Azure Data Factory を](data-factory-scheduling-and-execution.md)します。  
- について、 [データの移動](data-factory-data-movement-activities.md) と [データ変換機能](data-factory-data-transformation-activities.md) Azure Data Factory で
- 理解 [管理と Azure Data Factory の監視](data-factory-monitor-manage-pipelines.md)します。
- [構築し、最初のパイプラインをデプロイ](data-factory-build-your-first-pipeline.md)します。 


 

   













 
 


 

 




