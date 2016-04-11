<properties 
    pageTitle="Azure Machine Learning バッチ実行アクティビティを使用して予測パイプラインを作成する | Microsoft Azure" 
    description="Azure Data Factory と Azure Machine Learning を使用して予測パイプラインを作成する方法について説明します" 
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
    ms.date="12/06/2015" 
    ms.author="spelluru"/>

# Azure Machine Learning アクティビティを使用して予測パイプラインを作成する   
## 概要

> [AZURE.NOTE] 参照してください [Azure Data Factory の概要](data-factory-introduction.md) と [最初のパイプラインの作成](data-factory-build-your-first-pipeline.md) Azure Data Factory サービスをすぐに始める記事です。

## はじめに

[Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) ビルド、テスト、および予測分析ソリューションを配置することができます。 大まかに次の 3 つの手順で行われます。 

1. **トレーニング実験を作成する**です。 Azure ML Studio を使用します。これは、供給するトレーニング データを活用した予測分析モデルのトレーニングとテストに使用できる、コラボレーションと視覚化に対応した開発環境です。
2. **予測の実験に変換**します。 既存のデータでモデルがトレーニングされ、それを使用して新しいデータをスコア付けする準備ができると、スコア付け用に実験を用意し、合理化します。
3. **Web サービスとして展開**します。 Azure Web サービスとしてスコア付け実験を発行できます。 この Web サービスのエンドポイントを使用して、ユーザーはモデルにデータを送信し、モデルの予測を受信できます。  

Azure Data Factory では、簡単に、パブリッシュされたを活用するパイプラインを作成することができます [Azure Machine Learning][azure-machine-learning] web 予測分析用のサービスです。 使用して、 **バッチ実行アクティビティ** を Azure Data Factory パイプラインでは、バッチ内のデータを予測する、Azure ML web サービスを呼び出すことができます。 参照してください [バッチ実行アクティビティを使用して Azure ML web サービスを呼び出す](#invoking-an-azure-ml-web-service-using-the-batch-execution-activity) 詳細に関する説明。

時間の経過と共に、Azure ML スコア付け実験の予測モデルには、新しい入力データセットを使用した再トレーニングが必要になります。 次の手順を実行することで、Data Factory パイプラインから Azure ML モデルを再トレーニングできます。 

1. 予測実験ではなく、トレーニング実験を Web サービスとして発行します。 前のシナリオで予測実験を Web サービスとして公開したのと同様に Azure ML Studio で行います。
2. Azure ML バッチ実行アクティビティを使用して、トレーニング実験用 Web サービスを呼び出します。 基本的には、Azure ML バッチ実行アクティビティを使用して、トレーニング Web サービスとスコア付け Web サービスの両方を呼び出すことができます。 
  
再トレーニングを実行したら、スコア付け Web サービス (Web サービスとして公開した予測実験) を新しくトレーニングを行ったモデルで更新する必要があります。 次の手順に従って行います。 

1. スコア付け Web サービスに既定以外のエンドポイントを追加します。 Web サービスの既定のエンドポイントは更新できません。そのため、Azure クラシック ポータルを使用して既定以外のエンドポイントを新しく作成する必要があります。 参照してください、 [エンドポイントの作成](../machine-learning/machine-learning-create-endpoint.md) 概念情報と操作手順の両方のアーティクルです。
2. 既存のスコア付け用 Azure ML のリンクされたサービスを、既定以外のエンドポイントを使用するように更新します。 更新された Web サービスを使用するには、新しいエンドポイントの使用を開始する必要があります。
3. 使用して、 **Azure ML リソース更新のアクティビティ** 新しくトレーニングを行ったモデルを web サービスを更新します。  

参照してください [リソースの更新プログラムのアクティビティを使用して、Azure ML の更新モデル](#updating-azure-ml-models-using-the-update-resource-activity) 詳細に関する説明。 

## バッチ実行アクティビティを使用して Azure ML Web サービスを呼び出す

データの移動と処理を調整するために Azure Data Factory を使用して、Azure Machine Learning を使用してバッチの実行を実行するとします。 これを実現するのには、以下を行う必要があります。

1. Azure Machine Learning のリンクされたサービスを作成します。 以下のものが必要ですが。
    1. **要求の URI** API のバッチ実行用です。 をクリックして、要求の URI を見つけることができます、 **バッチ実行** (下図参照)、web サービス ページにリンクします。
    1. **API キー** 公開済みの Azure Machine Learning の web サービスです。 API キーは、発行した Web サービスをクリックするとわかります。 
 2. 使用して、 **AzureMLBatchExecution** アクティビティ。

    ![Machine Learning Dashboard](./media/data-factory-azure-ml-batch-execution-activity/AzureMLDashboard.png)

    ![Batch URI](./media/data-factory-azure-ml-batch-execution-activity/batch-uri.png)


### シナリオ: Azure Blob Storage のデータを参照する Web サービスの入力/出力を使用する
このシナリオの Azure Machine Learning Web サービスは、Azure BLOB ストレージ内のファイルのデータを使用して予測を作成し、BLOB ストレージに予測結果を保存します。 次の JSON では、AzureMLBatchExecution アクティビティを使用する Azure Data Factory パイプラインが定義されています。 アクティビティが持っているデータセット **DecisionTreeInputBlob** 入力として、 **DecisionTreeResultBlob** 出力として。  **DecisionTreeInputBlob** を使用して Web サービスへの入力として渡される、 **webServiceInput** JSON プロパティと **DecisionTreeResultBlob** を使用して Web サービスに出力として、 **webserviceoutputs にある** JSON のプロパティです。  

> [AZURE.NOTE] データセットで参照されている、 **webServiceInput** と **webserviceoutputs にある** プロパティ (で **typeProperties**)、アクティビティにも含める必要があります **入力** と **出力**します。


    {
      "name": "PredictivePipeline",
      "properties": {
        "description": "use AzureML model",
        "activities": [
          {
            "name": "MLActivity",
            "type": "AzureMLBatchExecution",
            "description": "prediction analysis on batch input",
            "inputs": [
              {
                "name": "DecisionTreeInputBlob"
              }
            ],
            "outputs": [
              {
                "name": "DecisionTreeResultBlob"
              }
            ],
            "linkedServiceName": "MyAzureMLLinkedService",
            "typeProperties":
            {
                "webServiceInput": "DecisionTreeInputBlob ",
                "webServiceOutputs": {
                    "output1": "DecisionTreeResultBlob "
                }                
            },
            "policy": {
              "concurrency": 3,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            }
          }
        ],
        "start": "2015-02-13T00:00:00Z",
        "end": "2015-02-14T00:00:00Z"
      }
    }

> [AZURE.NOTE] 唯一の入力と AzureMLBatchExecution アクティビティの出力は、Web サービスにパラメーターとして渡すことができます。 たとえば、上の JSON スニペットでは、DecisionTreeInputBlob は AzureMLBatchExecution アクティビティへの入力であり、webServiceInput パラメーターを使用して Web サービスに入力として渡されます。   

### 例

この例では、Azure Storage を使用して、入力データと出力データの両方を保持します。 

を介して行われることをお勧めします [Data Factory を使って最初のパイプラインの作成][adf-build-1st-pipeline] チュートリアルこの例では、この例と Data Factory アーティファクト (リンクされたサービス、データセット、パイプライン) を作成する Data Factory エディターの使用を読み進める前にします。   
 

1. 作成、 **のリンクされたサービス** の **Azure Storage**します。 入力ファイルと出力ファイルが異なるストレージ アカウントにある場合は、リンクされたサービスが 2 つ必要です。 JSON の例を次に示します。

        {
          "name": "StorageLinkedService",
          "properties": {
            "type": "AzureStorage",
            "typeProperties": {
              "connectionString": "DefaultEndpointsProtocol=https;AccountName=[acctName];AccountKey=[acctKey]"
            }
          }
        }

2. 作成、 **入力** Azure Data Factory **データセット**します。 その他の Data Factory のデータセットとは異なりこれら必要があります両方が含まれる両方 **folderPath** と **fileName** 値。 パーティション分割を使用すると、各バッチ実行 (各データ スライス) で一意の入力ファイルと出力ファイルを処理または生成できます。 入力を CSV ファイル形式に変換し、これを各スライスのストレージ アカウントに配置するには、いくつかのアップストリーム アクティビティを含める必要があります。 その場合を含めない、 **外部** と **externalData** 次の例と、DecisionTreeInputBlob に表示される設定が別のアクティビティの出力データセットになります。

        {
          "name": "DecisionTreeInputBlob",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
              "folderPath": "azuremltesting/input",
              "fileName": "in.csv",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "external": true,
            "availability": {
              "frequency": "Day",
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
    
    入力 csv ファイルには列ヘッダー行がある必要があります。 使用している場合、 **コピー アクティビティ** 作成/csv を blob ストレージに移動するには、シンクのプロパティを設定する必要があります **blobWriterAddHeader** に **true**します。 次に例を示します。
    
         sink: 
         {
             "type": "BlobSink",     
             "blobWriterAddHeader": true 
         }
     
    csv ファイルにヘッダー行がない場合、次のエラーが表示される場合があります: **アクティビティ エラー: 文字列の読み取りエラー。予期しないトークン: StartObject。パス ''、行 1、位置 1**。
3. 作成、 **出力** Azure Data Factory **データセット**します。 この例では、パーティション分割を使用して各スライスの実行ごとに一意の出力パスを作成します。 これを行わないと、アクティビティはファイルを上書きします。

        {
          "name": "DecisionTreeResultBlob",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
              "folderPath": "azuremltesting/scored/{folderpart}/",
              "fileName": "{filepart}result.csv",
              "partitionedBy": [
                {
                  "name": "folderpart",
                  "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "yyyyMMdd"
                  }
                },
                {
                  "name": "filepart",
                  "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "HHmmss"
                  }
                }
              ],
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "availability": {
              "frequency": "Day",
              "interval": 15
            }
          }
        }

4. 作成、 **のリンクされたサービス** 型の: **AzureMLLinkedService**, API キーを提供すること、およびバッチ実行 URL をモデル化します。
        
        {
          "name": "MyAzureMLLinkedService",
          "properties": {
            "type": "AzureML",
            "typeProperties": {
              "mlEndpoint": "https://[batch execution endpoint]/jobs",
              "apiKey": "[apikey]"
            }
          }
        }
5. 最後を含むパイプラインを作成、 **AzureMLBatchExecution** アクティビティ。 このパイプラインは、入力データセットから入力ファイルの場所を取得し、Azure Machine Learning バッチ実行 API を呼び出して、出力データセットで指定された BLOB にバッチ実行の出力をコピーします。 

    > [AZURE.NOTE] AzureMLBatchExecution アクティビティは、0 個以上の入力と 1 つまたは複数の出力ができます。

        {
          "name": "PredictivePipeline",
          "properties": {
            "description": "use AzureML model",
            "activities": [
              {
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [
                  {
                    "name": "DecisionTreeInputBlob"
                  }
                ],
                "outputs": [
                  {
                    "name": "DecisionTreeResultBlob"
                  }
                ],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties":
                {
                    "webServiceInput": "DecisionTreeInputBlob ",
                    "webServiceOutputs": {
                        "output1": "DecisionTreeResultBlob "
                    }                
                },
                "policy": {
                  "concurrency": 3,
                  "executionPriorityOrder": "NewestFirst",
                  "retry": 1,
                  "timeout": "02:00:00"
                }
              }
            ],
            "start": "2015-02-13T00:00:00Z",
            "end": "2015-02-14T00:00:00Z"
          }
        }

    両方とも **開始** と **エンド** datetimes がである必要があります [ISO 形式](http://en.wikipedia.org/wiki/ISO_8601)します。 (例: 2014-10-14T16:32:41Z)。  **エンド** 時刻は省略可能です。 値が指定されていない場合、 **エンド** として計算されますプロパティには、"**start+48 hours**"です。 パイプラインを無期限に実行する指定 **9999-09-09** の値として、 **エンド** プロパティです。 参照してください [JSON スクリプティング リファレンス](https://msdn.microsoft.com/library/dn835050.aspx) JSON プロパティの詳細について。

    > [AZURE.NOTE] アクティビティは、AzureMLBatchExecution への入力の指定は省略可能です。 

### シナリオ: リーダー/ライター モジュールを使用してさまざまなストレージ内のデータを参照する

Azure ML を実験するときのもう 1 つの一般的なシナリオは、リーダー モジュールとライター モジュールを使用することです。 リーダー モジュールは実験にデータを読み込むために使用し、ライター モジュールは実験からデータを保存するために使用します。 リーダーとライター モジュールに関する詳細については、「 [リーダー](https://msdn.microsoft.com/library/azure/dn905997.aspx) と [ライター](https://msdn.microsoft.com/library/azure/dn905984.aspx) MSDN ライブラリのトピックです。     

リーダー/ライター モジュールの各プロパティには Web サービスのパラメーターを使用するのがよい方法です。 これらの Web パラメーターを使用すると、実行時に値を構成できます。 たとえば、Azure SQL Database: XXX.database.windows.net を使用するリーダー モジュールで実験を作成できます。 Web サービスをデプロイした後、Web サービスのコンシューマーを有効にして、YYY.database.windows.net という名前の別の Azure SQL Server を指定できます。 Web サービスのパラメーターを使用して、この値を構成できます。

> [AZURE.NOTE] Web サービスの入力と出力は、Web サービス パラメーターと異なるのです。 最初のシナリオでは、Azure ML Web サービスに対して入力と出力を指定する方法を説明しました。 このシナリオでは、リーダー/ライター モジュールのプロパティに対応するパラメーターを Web サービスに渡します。 

Web サービス パラメーターを使用するシナリオを見てみましょう。 Azure Machine Learning がサポートするデータ ソース (例: Azure SQL Database) の 1 つからデータを読み取るためにリーダー モジュールを使用する Azure Machine Learning Web サービスをデプロイしてあります。 バッチ実行が実行された後、ライター モジュールを使用して結果が書き込まれます (Azure SQL Database)。  Web サービスの入力と出力は実験では定義されていません。 この場合は、リーダー/ライター モジュールに関連する Web サービス パラメーターを設定することをお勧めします。 これにより、AzureMLBatchExecution アクティビティを使用するときにリーダー/ライター モジュールを構成できます。 指定されている Web サービス パラメーターを指定する、 **globalParameters** アクティビティ JSON のセクションを次のようにします。 


    "typeProperties": {
        "globalParameters": {
            "Param 1": "Value 1",
            "Param 2": "Value 2"
        }
    }

使用することも [Data Factory 関数](https://msdn.microsoft.com/library/dn835056.aspx) Web の値を渡すにはパラメーターを次の例で示すようにサービスします。

    "typeProperties": {
        "globalParameters": {
           "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
        }
    }
 
> [AZURE.NOTE] Web サービス パラメーターでは大文字小文字の区別、名前のアクティビティを指定する JSON と一致する Web サービスによって公開されているようにします。 

### リーダー モジュールを使用して Azure BLOB の複数のファイルからデータを読み取る
ビッグ データ パイプライン (Pig、Hive など) は、拡張子が付いていない出力ファイルを 1 つ以上生成できます。 たとえば、外部 Hive テーブルを指定するとき、外部 Hive テーブルのデータを 000000_0 という名前で Azure BLOB ストレージに格納できます。 実験では、リーダー モジュールを使用して複数のファイルを読み取り、予測に使用できます。 

Azure Machine Learning の実験でリーダー モジュールを使用する場合は、入力として Azure BLOB を指定できます。 Azure BLOB ストレージ内のファイルは、HDInsight で実行する Pig および Hive スクリプトによって生成される出力ファイル (例: 000000_0) でもかまいません。 リーダー モジュールを使用すると、構成、(拡張子がない) ファイルを読み取る、 **コンテナー、ディレクトリまたは blob へのパス** を次に示すようにファイルを含むコンテナー/フォルダーを指すリーダー モジュールのプロパティです。 」では、アスタリスク (つまり \ *) **ことを指定コンテナー/フォルダー内のすべてのファイル (に対するデータ/aggregateddata/年のつまり = 2014年/月 ~ 6/\ *)** は、実験の一部として読み取られます。

![Azure BLOB のプロパティ](./media/data-factory-create-predictive-pipelines/azure-blob-properties.png)


### 例 
#### Web サービス パラメーターを使用した AzureMLBatchExecution のパイプライン

    {
      "name": "MLWithSqlReaderSqlWriter",
      "properties": {
        "description": "Azure ML model with sql azure reader/writer",
        "activities": [
          {
            "name": "MLSqlReaderSqlWriterActivity",
            "type": "AzureMLBatchExecution",
            "description": "test",
            "inputs": [
              {
                "name": "MLSqlInput"
              }
            ],
            "outputs": [
              {
                "name": "MLSqlOutput"
              }
            ],
            "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
            "typeProperties":
            {
                "webServiceInput": "MLSqlInput",
                "webServiceOutputs": {
                    "output1": "MLSqlOutput"
                }
                "globalParameters": {
                    "Database server name": "<myserver>.database.windows.net",
                    "Database name": "<database>",
                    "Server user account name": "<user name>",
                    "Server user account password": "<password>"
                }              
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            },
          }
        ],
        "start": "2015-02-13T00:00:00Z",
        "end": "2015-02-14T00:00:00Z"
      }
    }
 
上の JSON の例に関する説明:

- デプロイされた Azure Machine Learning Web サービスは、リーダー モジュールとライター モジュールを使用して、Azure SQL Database のデータを読み書きします。 この Web サービスは、次の 4 つのパラメーターを公開します。 データベース サーバー名、データベース名、Server user account name、およびサーバーのユーザー アカウントのパスワード。  
- 両方とも **開始** と **エンド** datetimes がである必要があります [ISO 形式](http://en.wikipedia.org/wiki/ISO_8601)します。 (例: 2014-10-14T16:32:41Z)。  **エンド** 時刻は省略可能です。 値が指定されていない場合、 **エンド** として計算されますプロパティには、"**start+48 hours**"です。 パイプラインを無期限に実行する指定 **9999-09-09** の値として、 **エンド** プロパティです。 参照してください [JSON スクリプティング リファレンス](https://msdn.microsoft.com/library/dn835050.aspx) JSON プロパティの詳細について。

### その他のシナリオ

#### Web サービスに入力が必要ではない

Azure ML バッチ実行 Web サービスを使用すると、R や Python のスクリプトなど、入力が必要ではない任意のワークフローを実行できます。 また、GlobalParameters を公開しない Reader モジュールを使用して構成する方法を実験することもできます。 この場合、AzureMLBatchExecution アクティビティは次のように構成されます。

    {
        "name": "scoring service",
        "type": "AzureMLBatchExecution",
        "outputs": [
            {
                "name": "myBlob"
            }
        ],
        "typeProperties": {
            "webServiceOutputs": {
                "output1": "myBlob"
            }              
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },
   

#### Web サービスに入力/出力が必要ではない
Azure ML Batch 実行 Web サービスに Web サービスの出力を構成しない場合があります。 この例では、Web サービスの入力も出力ありません。また、GlobalParameters も構成されていません。 アクティビティ自体に構成されている出力があっても、webServiceOutput として出力されません。

    {
        "name": "retraining",
        "type": "AzureMLBatchExecution",
        "outputs": [
            {
                "name": "placeholderOutputDataset"
            }
        ],
        "typeProperties": {
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },

#### Web サービスはリーダーとライターを使用し、他のアクティビティが成功した場合にのみアクティビティを実行する

Azure ML Web サービスのリーダーとライター モジュールは、GlobalParameters あり、またはなしで構成できます。 ただし、データセットの依存関係を使用するサービスの呼び出しを処理パイプラインに組み込み、一部のアップストリーム処理が完了した場合にのみサービスを呼び出し、バッチ実行が完了した後にその他のアクションをトリガーすることもできます。 この場合、Web サービスの入力または出力として指定せずに、アクティビティの入力と出力を使用する依存関係を表現することができます。

    {
        "name": "retraining",
        "type": "AzureMLBatchExecution",
        "inputs": [
            {
                "name": "upstreamData1"
            },
            {
                "name": "upstreamData2"
            }
        ],
        "outputs": [
            {
                "name": "downstreamData"
            }
        ],
        "typeProperties": {
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },

 **にとっての利点** は。

-   実験用エンドポイントが webServiceInput を使用している場合、そのエンドポイントは BLOB データセットで表され、アクティビティの入力と webServiceInput プロパティに含まれます。 使用していない場合、webServiceInput プロパティは省略されます。 
-   実験用エンドポイントが webServiceOutput を使用している場合、そのエンドポイントは BLOB データセットで表され、アクティビティの出力と webServicepOutputs プロパティに含まれます (実験の各出力の名前でマップされます)。 使用していない場合、webServiceOutputs プロパティは省略されます。
-   実験用エンドポイントが globalParameter を公開している場合、アクティビティの globalParameters プロパティでキーと値のペアとして指定されます。 公開していない場合、globalParameters プロパティは省略されます。 キーは大文字と小文字が区別されます。 [Azure Data Factory 関数](data-factory-scheduling-and-execution.md#data-factory-functions-reference) 値を使用することがあります。 
- アクティビティの入力と出力のプロパティには、アクティビティの typeProperties から参照せずに含めることができるその他のデータセットがあります。 このようなデータセットは実行の制御にスライスの依存関係を使用しますが、それ以外の場合は、AzureMLBatchExecution アクティビティから無視されます。 


## 更新リソース アクティビティを使用して Azure ML モデルを更新する
時間の経過と共に、Azure ML スコア付け実験の予測モデルには、新しい入力データセットを使用した再トレーニングが必要になります。 再トレーニングが完了したら、再トレーニング済みの ML モデルでスコア付け Web サービスを更新する必要があります。 Web サービスを使用して Azure ML モデルの再トレーニングと更新を有効にするための標準的な手順を次に示します。 

1. 実験を作成する [Azure ML Studio](https://studio.azureml.net)します。 
2. モデルに満足したらは、Azure ML Studio を使用して、両方の web サービスを公開する、 **トレーニング実験** データとスコア付け/**予測実験**します。

次の表で、この例で使用する Web サービスについて説明します。  参照してください [再トレーニングの機械学習モデルをプログラムによって](../machine-learning/machine-learning-retrain-models-programmatically.md) 詳細です。

| Web サービスの種類 | description 
| :------------------ | :---------- 
| **トレーニング Web サービス** | トレーニング データを受信し、トレーニング済みのモデルを作成します。 再トレーニングの出力は、Azure Blob Storage 内の .ilearner ファイルになります。   **既定のエンドポイント** トレーニングを発行するときに、実験を web サービスとして自動的に作成します。 エンドポイントは複数作成することができますが、この例では、既定のエンドポイントのみを使用します。 |
| **スコア付け Web サービス** | ラベルの付いていないデータの例を受信し、予測を作成します。 予測の出力は、実験の構成に応じてさまざまな形式 (.csv ファイル、Azure SQL Database の行など) をとります。 既定のエンドポイントが、予測実験を Web サービスとして発行するときに自動的に作成されます。 もう 1 つを作成する必要があります **と既定以外の更新可能なエンドポイント** を使用して、 [Azure Classic Portal](https://manage.windowsazure.com)します。 エンドポイントは複数作成することができますが、この例では、更新可能な既定以外のエンドポイントを 1 つだけ使用します。 参照してください、 [エンドポイントの作成](../machine-learning/machine-learning-create-endpoint.md) 手順についての記事です。       
 
次の図は、Azure ML でのトレーニングとスコア付けのエンドポイントの関係を示しています。 

![Web サービス](./media/data-factory-azure-ml-batch-execution-activity/web-services.png)


起動するには、 **web サービスのトレーニング** を使用して、 **Azure ML バッチ実行アクティビティ**します。 これは、データのスコア付け用 Azure ML Web サービス (スコア付け Web サービス) を呼び出す場合と同じです。 前のセクションで、Azure Data Factory パイプラインから Azure ML Web サービスを呼び出す方法について詳しく説明しています。 
  
起動するには、 **web サービスをスコア付け** を使用して、 **Azure ML リソース更新のアクティビティ** 新しくトレーニングを行ったモデルを web サービスを更新します。 前の表で説明したように、更新可能な既定以外のエンドポイントを作成して使用する必要があります。 データ ファクトリ内の既存のリンクされたサービスも、既定以外のエンドポイントを使用するように更新して、常に最新の再トレーニングを行ったモデルが使用されるようにする必要があります。 

次のシナリオでは、Azure Data Factory パイプラインから Azure ML モデルの再トレーニングと更新を行う方法について、例を用いて詳しく説明します。 
 
### シナリオ: Azure ML モデルの再トレーニングと更新
このセクションで説明したサンプル パイプラインを使用する、 **Azure ML バッチ実行アクティビティ** モデルの再トレーニングして、 **Azure ML 更新リソース アクティビティ** スコアを付ける web サービスでモデルを更新します。 この例では、すべてのリンクされたサービス、データ セット、およびパイプラインの JSON スニペットも提供されます。 

サンプル パイプラインのダイアグラム ビューを次に示します。 ご覧のように、Azure ML バッチ実行アクティビティはトレーニングの入力を受け取り、トレーニングの出力 (iLearner ファイル) を作成します。 Azure ML 更新リソース アクティビティはトレーニングの出力を受け取り、スコア付け Web サービスのエンドポイントでモデルを更新します。 更新リソース アクティビティは出力を作成しません。 placeholderBlob は、パイプラインを実行するために、Azure Data Factory サービスで必要とされるダミーの出力データセットです。 

![pipeline diagram](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)


#### Azure BLOB ストレージのリンクされたサービス:
Azure Storage には次のデータが格納されています。

- トレーニング データ。 これは、Azure ML トレーニング Web サービス用の入力データです。  
- iLearner ファイル。 これは、Azure ML トレーニング Web サービスの出力です。 更新リソース アクティビティへの入力としても使用します。  
   
リンクされたサービスのサンプルの JSON 定義を次に示します。 

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=name;AccountKey=key"
            }
        }
    }


#### トレーニングの入力データセット:
次のデータセットは、Azure ML トレーニング Web サービス用の入力トレーニング データを示しています。 Azure ML バッチ実行アクティビティはこのデータセットを入力として使用します。 

    {
        "name": "trainingData",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "labeledexamples",
                "fileName": "labeledexamples.arff",
                "format": {
                    "type": "TextFormat"
                }
            },
            "availability": {
                "frequency": "Week",
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

#### トレーニングの出力データセット:
次のデータセットは、Azure ML トレーニング Web サービスの出力 iLearner ファイルを示しています。 Azure ML バッチ実行アクティビティがこのデータセットを作成します。 このデータセットは、Azure ML 更新リソース アクティビティへの入力としても使用されます。

    {
        "name": "trainedModelBlob",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "trainingoutput",
                "fileName": "model.ilearner",
                "format": {
                    "type": "TextFormat"
                }
            },
            "availability": {
                "frequency": "Week",
                "interval": 1
            }
        }
    }

#### Azure ML トレーニング エンドポイント用のリンクされたサービス 
次の JSON スニペットでは、トレーニングの web サービスの既定のエンドポイントをポイントする Azure Machine Learning リンク サービスを定義します。 

    {   
        "name": "trainingEndpoint",
        "properties": {
            "type": "AzureML",
            "typeProperties": {
                "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--training experiment--/jobs",
                "apiKey": "myKey"
            }
        }
    }

 **Azure ML Studio**, の値を取得する次の操作を **mlEndpoint** と **apiKey**:

1. クリックして **WEB サービス** 左側のメニュー。
2. クリックして、 **web サービスのトレーニング** web サービスの一覧にします。 
3. 横に [コピー] をクリックして **API キー** テキスト ボックスは、API キーをクリップボードにコピーします。 Data Factory JSON エディターにこの API キーを貼り付けます。
4.  **Azure ML studio**, 、] をクリックして **バッチ実行** リンクで、コピー、 **要求の URI** から、 **要求** セクションし、データ ファクトリの JSON エディターに貼り付けます。   


#### Azure ML の更新可能なスコア付けエンドポイント用のリンクされたサービス:
次の JSON スニペットは、スコア付け Web サービスの更新可能な既定以外のエンドポイントを参照する Azure Machine Learning のリンクされたサービスを定義します。  

    {
        "name": "updatableScoringEndpoint2",
        "properties": {
            "type": "AzureML",
            "typeProperties": {
                "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--scoring experiment--/jobs",
                "apiKey": "endpoint2Key",
                "updateResourceEndpoint": "https://management.azureml.net/workspaces/xxx/webservices/--scoring experiment--/endpoints/endpoint2"
            }
        }
    }


作成と Azure ML の展開にリンクされたサービスが前に、の手順で [エンドポイントの作成](../machine-learning/machine-learning-create-endpoint.md) 資料をスコア付け web サービスの 2 つ目の (既定ではないと更新可能な) のエンドポイントを作成します。

既定以外の更新可能なエンドポイントを作成した後にをクリックして **バッチ実行** URI 値を取得する、 **mlEndpoint** JSON のプロパティ] をクリック **更新リソース** URI 値を取得するリンク、 **updateResourceEndpoint** JSON のプロパティです。 API キーは、エンドポイントのページにあります (右下隅)。 

![updatable endpoint](./media/data-factory-azure-ml-batch-execution-activity/updatable-endpoint.png)

 
#### プレースホルダーの出力データセット:
Azure ML 更新リソース アクティビティは出力を作成しませんが、Azure Data Factory では、パイプラインのスケジュールを進めるために出力データセットが必要です。この例では、ダミー/プレースホルダーのデータセットを使用しています。  

    {
        "name": "placeholderBlob",
        "properties": {
            "availability": {
                "frequency": "Week",
                "interval": 1
            },
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "any",
                "format": {
                    "type": "TextFormat"
                }
            }
        }
    }


#### パイプライン
パイプラインが 2 つのアクティビティ: **AzureMLBatchExecution** と **AzureMLUpdateResource**します。 Azure ML バッチ実行アクティビティはトレーニング データを入力として使用し、.iLearner ファイルを出力として作成します。 このアクティビティは、トレーニング Web サービス (Web サービスとして公開されたトレーニング実験) と入力トレーニング データを呼び出し、Web サービスから ilearner ファイルを受け取ります。 placeholderBlob は、パイプラインを実行するために、Azure Data Factory サービスで必要とされるダミーの出力データセットです。 

![pipeline diagram](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)


    {
        "name": "pipeline",
        "properties": {
            "activities": [
                {
                    "name": "retraining",
                    "type": "AzureMLBatchExecution",
                    "inputs": [
                        {
                            "name": "trainingData"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "trainedModelBlob"
                        }
                    ],
                    "typeProperties": {
                        "webServiceInput": "trainingData",
                        "webServiceOutputs": {
                            "output1": "trainedModelBlob"
                        }              
                     },
                    "linkedServiceName": "trainingEndpoint",
                    "policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1,
                        "timeout": "02:00:00"
                    }
                },
                {
                    "type": "AzureMLUpdateResource",
                    "typeProperties": {
                        "trainedModelName": "Training Exp for ADF ML [trained model]",
                        "trainedModelDatasetName" :  "trainedModelBlob"
                    },
                    "inputs": [
                        {
                            "name": "trainedModelBlob"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "placeholderBlob"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "retry": 3
                    },
                    "name": "AzureML Update Resource",
                    "linkedServiceName": "updatableScoringEndpoint2"
                }
            ],
            "start": "2015-02-13T00:00:00Z",
            "end": "2015-02-14T00:00:00Z"
        }
    }


### リーダーとライター モジュール

Web サービス パラメーターの使用を伴う一般的なシナリオとして、Azure SQL のリーダーとライターを使用するシナリオがあります。 リーダー モジュールは、Azure Machine Learning Studio の外部のデータ管理サービスからデータを実験に読み込むために使用します。ライター モジュールは、Azure Machine Learning Studio の外部のデータ管理サービスに実験のデータを保存するために使用します。  

Azure Blob と Azure SQL リーダー/ライターの詳細については、「 [リーダー](https://msdn.microsoft.com/library/azure/dn905997.aspx) と [ライター](https://msdn.microsoft.com/library/azure/dn905984.aspx) MSDN ライブラリのトピックです。 前のセクションの例では、Azure BLOB リーダーと Azure BLOB ライターを使用しています。 ここでは、Azure SQL リーダーと Azure SQL ライターの使用について説明します。


## よく寄せられる質問

**Q:** 、ビッグ データ パイプラインによって生成される複数のファイルがあります。 AzureMLBatchExecution アクティビティを使用して、すべてのファイルで処理できますか。

**A:** [はい] です。 参照してください、 **リーダー モジュールを使用して Azure Blob で複数のファイルからデータを読み取る** 詳細に関する説明。 

## Azure ML バッチ スコアリング アクティビティ
使用している場合、 **AzureMLBatchScoring** Azure Machine Learning と統合するアクティビティの最新バージョンを使用することをお勧め **AzureMLBatchExecution** アクティビティ。 

AzureMLBatchExecution アクティビティは、Azure SDK および Azure PowerShell の 2015 年 8 月のリリースで導入されました。

AzureMLBatchScoring アクティビティを引き続き使用する場合は、このセクションを読んでください。  

### 入力/出力に Azure Storage を使用する Azure ML バッチ スコアリング アクティビティ 

    {
      "name": "PredictivePipeline",
      "properties": {
        "description": "use AzureML model",
        "activities": [
          {
            "name": "MLActivity",
            "type": "AzureMLBatchScoring",
            "description": "prediction analysis on batch input",
            "inputs": [
              {
                "name": "ScoringInputBlob"
              }
            ],
            "outputs": [
              {
                "name": "ScoringResultBlob"
              }
            ],
            "linkedServiceName": "MyAzureMLLinkedService",
            "policy": {
              "concurrency": 3,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            }
          }
        ],
        "start": "2015-02-13T00:00:00Z",
        "end": "2015-02-14T00:00:00Z"
      }
    }

### Web サービス パラメーター
追加、 **typeProperties** セクションに、 **AzureMLBatchScoringActivty** セクションでは、次の例に示すように Web サービス パラメーターの値を指定する JSON パイプラインのセクション。 

    "typeProperties": {
        "webServiceParameters": {
            "Param 1": "Value 1",
            "Param 2": "Value 2"
        }
    }


使用することも [Data Factory 関数](https://msdn.microsoft.com/library/dn835056.aspx) Web の値を渡すにはパラメーターを次の例で示すようにサービスします。

    "typeProperties": {
        "webServiceParameters": {
           "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
        }
    }
 
> [AZURE.NOTE] Web サービス パラメーターでは大文字小文字の区別、名前のアクティビティを指定する JSON と一致する Web サービスによって公開されているようにします。 

## 関連項目

- [Azure ブログの投稿: Azure Data Factory と Azure Machine Learning の概要](https://azure.microsoft.com/blog/getting-started-with-azure-data-factory-and-azure-machine-learning-4/)





[adf-build-1st-pipeline]: data-factory-build-your-first-pipeline.md

[azure-machine-learning]: http://azure.microsoft.com/services/machine-learning/


 

