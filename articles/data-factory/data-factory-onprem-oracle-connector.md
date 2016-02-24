<properties 
    pageTitle="Oracle からのデータの移動 | Azure Data Factory" 
    description="Azure Data Factory を使用してオンプレミスの Oracle データベースとの間でデータを移動する方法を説明します。" 
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
    ms.date="11/12/2015" 
    ms.author="spelluru"/>

# Azure Data Factory を使用してオンプレミスの Oracle からデータを移動する 

この記事では、データ ファクトリのコピー アクティビティを使用して Oracle から他のデータ ストアにデータを移動する方法について説明します。 この記事に基づき、 [データ移動アクティビティ](data-factory-data-movement-activities.md) コピー アクティビティとサポートされているデータ ストアの組み合わせでデータ移動の一般概要を説明する記事です。

## インストール 
Azure Data Factory サービスをオンプレミスの Oracle データベースに接続できるようにするには、次をインストールする必要があります。 

- データベースをホストするコンピューターと同じコンピューター、またはデータベースとのリソースの競合を避けるために別のコンピューター上にインストールされた Data Management Gateway。 Data Management Gateway は、安全かつ管理された方法でオンプレミスのデータをクラウド サービスに接続するソフトウェアです。 参照してください [オンプレミスとクラウド間のデータ移動](data-factory-move-data-between-onprem-and-cloud.md) Data Management Gateway の詳細については、資料です。 
- [Windows 用の oracle のデータ アクセス コンポーネント (ODAC)](http://www.oracle.com/technetwork/topics/dotnet/downloads/index.html)します。 これは、ゲートウェイがインストールされているホスト コンピューターにインストールする必要があります。


## サンプル: Oracle から Azure BLOB にデータをコピーする

下のサンプルで確認できる要素:

1.  型のリンクされたサービス [OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties)します。
2.  型のリンクされたサービス [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)します。
3.  入力 [データセット](data-factory-create-datasets.md) 型の [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties)します。 
4.  出力 [データセット](data-factory-create-datasets.md) 型の [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)します。
5.  A [パイプライン](data-factory-create-pipelines.md) を使用するコピー アクティビティの [OracleSource](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) ソースとして、 [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) シンクとして。

このサンプルはオンプレミスの Oracle データベース内のテーブルから BLOB に 1 時間ごとにデータをコピーします。 下のサンプルで使用されるさまざまなプロパティの詳細については、サンプルに続くセクションの各プロパティのドキュメントを参照してください。

**Oracle のリンクされたサービス:**

    {
      "name": "OnPremisesOracleLinkedService",
      "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
          "ConnectionString": "data source=<data source>;User Id=<User Id>;Password=<Password>;",
          "gatewayName": "<gateway name>"
        }
      }
    }

**Azure BLOB ストレージのリンクされたサービス:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
      }
    }

**Oracle の入力データセット:**

このサンプルでは、Oracle で「MyTable」という名前のテーブルを作成し、時系列データ用に「timestampcolumn」という名前の列が含まれているものと想定しています。 

“external”: ”true” を設定して externalData ポリシーを指定すると、これがデータ ファクトリに対して外部にあるテーブルで、データ ファクトリのアクティビティでは生成されていないことが Data Factory に通知されます。

    {
        "name": "OracleInput",
        "properties": {
            "type": "OracleTable",
            "linkedServiceName": "OnPremisesOracleLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
               "external": true,
            "availability": {
                "offset": "01:00:00",
                "interval": "1",
                "anchorDateTime": "2014-02-27T12:00:00",
                "frequency": "Hour"
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


**Azure BLOB の出力データセット:**

データは新しい BLOB に 1 時間おきに書き込まれます (頻度: 時間、間隔: 1)。 BLOB のフォルダー パスとファイル名は、処理中のスライスの開始時間に基づき、動的に評価されます。 フォルダー パスは開始時間の年、月、日、時刻の部分を使用します。
    
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


**コピー アクティビティのあるパイプライン:**

上記の入力データセットと出力データセットを使用するように構成され、1 時間おきに実行するようにスケジュールされているコピー アクティビティがパイプラインに含まれています。 パイプライン JSON 定義で、 **ソース** に設定されている型 **RelationalSource** と **シンク** に設定されている型 **BlobSink**します。  指定された SQL クエリ **oracleReaderQuery** プロパティをコピーするには過去のデータを選択します。

    
    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
          {
            "name": "AzureSQLtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": " OracleInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "OracleSource",
                "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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

## Oracle のリンクされたサービスのプロパティ

次の表は、Oracle のリンクされたサービスに固有の JSON 要素の説明をまとめたものです。 

プロパティ | 説明 | 必須
-------- | ----------- | --------
type | Type プロパティに設定する必要があります: **OnPremisesOracle** | あり
connectionString | connectionString プロパティの Oracle Database インスタンスに接続するために必要な情報を指定します。 | はい 
gatewayName | オンプレミスの Oracle サーバーへの接続に使用されるゲートウェイの名前です | あり

参照してください [資格情報の設定とセキュリティ](data-factory-move-data-between-onprem-and-cloud.md#setting-credentials-and-security) の詳細については、内部設置型 Oracle データ ソースの資格情報を設定します。
## Oracle データセットの type プロパティ

セクションとデータセットの定義に使用できるプロパティの完全な一覧を参照してください、 [データセットを作成する](data-factory-create-datasets.md) 記事です。 データセット JSON の構造、可用性、ポリシーなどのセクションはすべてのデータセット型 (Oracle、Azure BLOB、Azure テーブルなど) で同じです。
 
typeProperties セクションはデータセット型ごとに異なり、データ ストアのデータの場所などに関する情報を提供します。 OracleTable 型のデータセットの typeProperties セクションには次のプロパティがあります。

プロパティ | 説明 | 必須
-------- | ----------- | --------
tableName | リンクされたサービスが参照する Oracle データベース インスタンスのテーブルの名前です。 | いいえ (場合 **oracleReaderQuery** の **SqlSource** が指定されている)

## Oracle のコピー アクティビティの type プロパティ

セクションとアクティビティの定義に使用できるプロパティの完全な一覧を参照してください、 [パイプラインの作成](data-factory-create-pipelines.md) 記事です。 名前、説明、入力テーブル、出力テーブル、さまざまなポリシーなどのプロパティがあらゆる種類のアクティビティで利用できます。 

**注:** コピー アクティビティは 1 つだけの入力を受け取りを 1 つだけの出力が生成されます。

一方で、アクティビティの typeProperties セクションで利用できるプロパティはアクティビティの種類により異なり、コピー アクティビティの場合、source と sink の種類によって異なります。

Source の種類がコピー アクティビティが発生した場合 **OracleSource** で、次のプロパティが利用 **typeProperties** セクション。

プロパティ | 説明 |使用できる値 | 必須
-------- | ----------- | ------------- | --------
oracleReaderQuery | カスタム クエリを使用してデータを読み取ります。 | SQL クエリ文字列。 
例: 選択 * from MyTable <p>実行される SQL ステートメントを指定しない場合: 選択 * from MyTable</p> | いいえ (場合 **tableName** の **データセット** が指定されている)

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### Oracle の型マッピング

説明したように、 [データ移動アクティビティ](data-factory-data-movement-activities.md) 記事コピー アクティビティは、シンクの次の 2 段階のアプローチで型を source から自動的に変換からの自動型変換を実行します。

1. ネイティブの source 型から .NET 型に変換する
2. .NET 型からネイティブの sink 型に変換する

Oracle からデータを移動すると、Oracle データ型から .NET 型へのマッピング (およびその逆) に、次のマッピングが使用されます。

Oracle データ型 | .NET Framework データ型
---------------- | ------------------------
BFILE | Byte[]
BLOB | Byte[]
CHAR | String
CLOB | String
DATE | DateTime
FLOAT | Decimal
INTEGER | Decimal
INTERVAL YEAR TO MONTH | Int32
INTERVAL DAY TO SECOND | TimeSpan
LONG | String
LONG RAW | Byte[]
NCHAR | String
NCLOB | String
NUMBER | Decimal
NVARCHAR2 | String
RAW | Byte[]
ROWID | String
TIMESTAMP | DateTime
TIMESTAMP WITH LOCAL TIME ZONE | DateTime
TIMESTAMP WITH TIME ZONE | DateTime
符号なし INTEGER | Number
VARCHAR2 | String
XML | String

## トラブルシューティングのヒント

* * 問題が発生します * *。
次を参照してください **エラー メッセージ**: コピー アクティビティは、無効なパラメーターを検出しました: 'UnknownParameterName'、メッセージの詳細: 要求された .Net が見つかりません Framework データ プロバイダーです。 インストールされていない可能性があります。  

**考えられる原因**

1. Oracle 用の .NET Framework Data Provider がインストールされていません。
2. Oracle 用の .NET Framework Data Provider が .NET Framework 2.0 にインストールされ、.NET Framework 4.0 フォルダーで検出されません。 

**解決/回避策**

1. .NET Provider for Oracle をインストールしていない場合は、次のようにしてください。 [インストール](http://www.oracle.com/technetwork/topics/dotnet/utilsoft-086879.html) シナリオを再試行してください。 
2. プロバイダーをインストールしてもエラー メッセージが表示される場合は、次の操作を行います。 
    1. フォルダーから .NET 2.0 のマシン構成を開きます。 <system disk>: \Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config です。
    2. 検索 **.NET 用の Oracle データ プロバイダー**, 、下にある次のようなエントリを検索することができ、 **system.data** ]-> [ **DbProviderFactories**:
            "、カルチャ = neutral, PublicKeyT for .NET" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" />"
2.  このエントリを次の v4.0 フォルダーに、machine.config ファイルにコピーします。 <system disk>: \Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config、および 4.xxx.x.x するバージョンを変更します。
3.  実行中"gacutil/i [プロバイダーのパス]"して、グローバル アセンブリ キャッシュ (GAC) に"< インストール パスが ODP.NET > \11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll"をインストールします。



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]




