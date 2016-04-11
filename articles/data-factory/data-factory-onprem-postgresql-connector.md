<properties 
    pageTitle="PostgreSQL からデータを移動する | Azure Data Factory" 
    description="Azure Data Factory を使用して PostgreSQL Databases からデータを移動する方法を説明します。" 
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

# Azure Data Factory を使用して PostgreSQL からデータを移動する

この記事では、Azure Data Factory のコピー アクティビティを使用して、PostgreSQL と他のデータ ストアとの間でデータを移動する方法について説明します。 この記事に基づき、 [データ移動アクティビティ](data-factory-data-movement-activities.md) コピー アクティビティとサポートされているデータ ストアの組み合わせでデータ移動の一般概要を説明する記事です。

Data Factory のサービスでは、Data Management Gateway を使用したオンプレミスの PostgreSQL ソースへの接続をサポートします。 参照してください [内部設置型の場所とクラウド間のデータ移動](data-factory-move-data-between-onprem-and-cloud.md) Data Management Gateway とゲートウェイを設定する方法の詳細な手順について学習します。 

**注:** Azure IaaS Vm でホストされている場合でも、PostgreSQL への接続にゲートウェイを利用する必要があります。 クラウドでホストされている PostgreSQL のインスタンスに接続しようとしている場合は、IaaS VM でゲートウェイ インスタンスをインストールすることもできます。

Data Factory は、PostgreSQL から他のデータ ストアへのデータ移動をサポートしますが、他のデータ ストアから PostgreSQL へのデータ移動はサポートしません。

## インストール 

PostgreSQL データベースへの接続に Data Management gateway をインストールする必要があります。、 [PostgreSQL の Ngpsql データ プロバイダー](http://go.microsoft.com/fwlink/?linkid=282716) Data Management Gateway と同じシステムにします。

## サンプル: PostgreSQL から Azure BLOB にデータをコピーする

下のサンプルで確認できる要素:

1.  型のリンクされたサービス [OnPremisesPostgreSql](data-factory-onprem-postgresql-connector.md#postgresql-linked-service-properties)します。
2.  型のリンクされたサービス [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)します。
3.  入力 [データセット](data-factory-create-datasets.md) 型の [RelationalTable](data-factory-onprem-postgresql-connector.md#postgresql-dataset-type-properties)します。
4.  出力 [データセット](data-factory-create-datasets.md) 型の [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)します。
4.   [パイプライン](data-factory-create-pipelines.md) を使用するコピー アクティビティの [RelationalSource](data-factory-onprem-postgresql-connector.md#postgresql-copy-activity-type-properties) と [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)します。 

このサンプルは PostgreSQL データベースのクエリ結果のデータを BLOB に 1 時間ごとにコピーします。 これらのサンプルで使用される JSON プロパティの説明はサンプルに続くセクションにあります。 

まず、セットアップの指示に従って data management gateway、 [、オンプレミスの場所とクラウド間のデータ移動](data-factory-move-data-between-onprem-and-cloud.md) 記事です。

**PostgreSQL のリンクされたサービス:**

    {
        "name": "OnPremPostgreSqlLinkedService",
        "properties": {
            "type": "OnPremisesPostgreSql",
            "typeProperties": {
                "server": "<server>",
                "database": "<database>",
                "schema": "<schema>",
                "authenticationType": "<authentication type>",
                "username": "<username>",
                "password": "<password>",
                "gatewayName": "<gatewayName>"
            }
        }
    }

**Azure BLOB ストレージのリンクされたサービス:**
        
    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
        }
      }
    }

**PostgreSQL の入力データセット:**

このサンプルでは、PostgreSQL で「MyTable」という名前のテーブルを作成し、時系列データ用に「timestamp」という名前の列が含まれているものと想定しています。

「“external”: true」を設定して externalData ポリシーを指定すると、このテーブルが Data Factory に対して外部にあるテーブルで、 Data Factory のアクティビティでは生成されていないことが Data Factory に通知されます。

    {
        "name": "PostgreSqlDataSet",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "OnPremPostgreSqlLinkedService",
            "typeProperties": {},
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
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
        "name": "AzureBlobPostgreSqlDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/postgresql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
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
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**コピー アクティビティ:**

上記の入力データセットと出力データセットを使用するように構成され、1 時間おきに実行するようにスケジュールされているコピー アクティビティがパイプラインに含まれています。 パイプライン JSON 定義で、 **ソース** に設定されている型 **RelationalSource** と **シンク** に設定されている型 **BlobSink**します。 指定された SQL クエリ、 **クエリ** プロパティは、PostgreSQL データベースの public.usstates テーブルからデータを選択します。
    
    {
        "name": "CopyPostgreSqlToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "select * from public.usstates"
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "inputs": [
                        {
                            "name": "PostgreSqlDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobPostgreSqlDataSet"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "PostgreSqlToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }


## PostgreSQL のリンクされたサービスのプロパティ

次の表は、PostgreSQL のリンクされたサービスに固有の JSON 要素の説明をまとめたものです。

プロパティ | 説明 | 必須
-------- | ----------- | --------
type | Type プロパティに設定する必要があります: **OnPremisesPostgreSql** | あり
server | PostgreSQL サーバーの名前です。 | はい 
database | PostgreSQL データベースの名前です。 | はい 
schema | データベース内のスキーマの名前です。 | いいえ 
authenticationType | PostgreSQL データベースへの接続に使用される認証の種類です。 Anonymous、Basic、Windows のいずれかの値になります。 | はい 
username | Basic または Windows 認証を使用している場合は、ユーザー名を指定します。 | いいえ 
パスワード | ユーザー名に指定したユーザー アカウントのパスワードを指定します。 | いいえ 
gatewayName | Data Factory サービスが、オンプレミスの PostgreSQL データベースへの接続に使用するゲートウェイの名前です。 | あり 

参照してください [資格情報の設定とセキュリティ](data-factory-move-data-between-onprem-and-cloud.md#setting-credentials-and-security) の詳細については、内部設置型 PostgreSQL のデータ ソースの資格情報を設定します。

## PostgreSQL データセットの type プロパティ

セクションとデータセットの定義に使用できるプロパティの一覧については、次を参照してください。、 [データセットを作成する](data-factory-create-datasets.md) 記事です。 データセット JSON の構造、可用性、ポリシーなどのセクションはすべてのデータセット型 (Azure SQL、Azure BLOB、Azure テーブルなど) で同じです。

typeProperties セクションはデータセット型ごとに異なり、データ ストアのデータの場所などに関する情報を提供します。 型のデータセットの typeProperties セクション **RelationalTable** 、次のプロパティを持つ (を含む PostgreSQL データセットです)。

プロパティ | 説明 | 必須
-------- | ----------- | --------
tableName | リンクされたサービスが参照する PostgreSQL Databases インスタンスのテーブルの名前です。 | いいえ (場合 **クエリ** の **RelationalSource** が指定されている) 

## PostgreSQL のコピー アクティビティの type プロパティ

セクションとアクティビティの定義に使用できるプロパティの一覧については、次を参照してください。、 [パイプラインの作成](data-factory-create-pipelines.md) 記事です。 名前、説明、入力テーブル、出力テーブル、さまざまなポリシーなどのプロパティがあらゆる種類のアクティビティで利用できます。 

一方で、アクティビティの typeProperties セクションで利用できるプロパティはアクティビティの種類により異なり、コピー アクティビティの場合、source と sink の種類によって異なります。

Source の種類がコピー アクティビティが発生した場合 **RelationalSource** (を PostgreSQL を含む)、次のプロパティは、typeProperties セクションで使用します。

プロパティ | 説明 | 使用できる値 | 必須
-------- | ----------- | -------------- | --------
query | カスタム クエリを使用してデータを読み取ります。 | SQL クエリ文字列。 例: Select * from MyTable。 | いいえ (場合 **tableName** の **データセット** が指定されている)

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## PostgreSQL の型マッピング

説明したように、 [データ移動アクティビティ](data-factory-data-movement-activities.md) 記事コピー アクティビティは、シンクの次の 2 段階のアプローチで型を source から自動的に変換からの自動型変換を実行します。

1. ネイティブの source 型から .NET 型に変換する
1. .NET 型からネイティブの sink 型に変換する

PostgreSQL にデータを移動する場合、PostgreSQL 型から .NET 型に対する次のマッピングが使用されます。

PostgreSQL Databases 型 |  PostgreSQL エイリアス | .NET Framework 型
------------------------ | -------------------- | ---------------------
abstime |  | Datetime
bigint | int8 | Int64
bigserial | serial8 | Int64
bit [ (n) ] |  | Byte[]、文字列
bit varying [ (n) ] | varbit | Byte[]、文字列
boolean | bool | Boolean
box |  | Byte[]、文字列
bytea |  | Byte[]、文字列
character [ (n) ] | char [ (n) ] | String
character varying [ (n) ] | varchar [ (n) ] | String
cid |  | String
cidr | | String
circle | | Byte[]、文字列
date | | Datetime
daterange | | String
double precision| float8 | Double
inet | | Byte[]、文字列
intarry | | String
int4range | | String
int8range | | String
integer | int, int4 | Int32
interval [ fields ] [ (p) ] | | Timespan
json | | String
jsonb | | Byte[]
line | | Byte[]、文字列
lseg | | Byte[]、文字列
macaddr | | Byte[]、文字列
money | | Decimal
numeric [ (p, s) ] | decimal [ (p, s) ] | Decimal
numrange | | String
oid | | Int32
path | | Byte[]、文字列
pg_lsn | | Int64
point | | Byte[]、文字列
polygon | | Byte[]、文字列
real | float4 | Single
smallint | int2 | Int16
smallserial | serial2 | Int16
serial | serial4 | Int32
text | | String


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]


