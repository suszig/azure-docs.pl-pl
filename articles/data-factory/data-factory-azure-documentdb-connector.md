<properties 
    pageTitle="DocumentDB との間でのデータの移動 | Azure Data Factory" 
    description="Azure Data Factory を使用して Azure DocumentDB コレクションに、または Azure DocumentDB コレクションからデータを移動する方法を説明します。" 
    services="data-factory, documentdb" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="multiple" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/15/2015" 
    ms.author="spelluru"/>


# Azure Data Factory を使用した DocumentDB との間でのデータの移動

この記事では、Azure Data Factory のコピー アクティビティを利用し、Azure DocumentDB と別のデータ ストアの間でデータを移動する方法について説明します。 この記事に基づき、 [データ移動アクティビティ](data-factory-data-movement-activities.md) コピー アクティビティとサポートされているデータ ストアの組み合わせでデータ移動の一般概要を説明する記事です。

## サンプル: Azure DocumentDB から Azure BLOB にデータをコピーする

下のサンプルで確認できる要素:

1. 型のリンクされたサービス [DocumentDb](#azure-documentdb-linked-service-properties)します。
2. 型のリンクされたサービス [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)します。
3. 入力 [データセット](data-factory-create-datasets.md) 型の [DocumentDbCollection](#azure-documentdb-dataset-type-properties)します。
4. An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. A [パイプライン](data-factory-create-pipelines.md) を使用するコピー アクティビティの [DocumentDbCollectionSource](#azure-documentdb-copy-activity-type-properties) と [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)します。

このサンプルでは、Azure DocumentDB のデータが Azure BLOB にコピーされます。 これらのサンプルで使用される JSON プロパティの説明はサンプルに続くセクションにあります。

**Azure DocumentDB のリンクされたサービス:**

    {
      "name": "DocumentDbLinkedService",
      "properties": {
        "type": "DocumentDb",
        "typeProperties": {
          "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
      }
    }

**Azure BLOB ストレージのリンクされたサービス:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure Document DB 入力データセット:**

このサンプルでは、「**Person**」というコレクションが Azure DocumentDB データベースにあるものと想定しています。

「“external”: ”true”」を設定して externalData ポリシーを指定すると、テーブルが Data Factory に対して外部にあり、Data Factory のアクティビティでは生成されていないことが Azure Data Factory のサービスに通知されます。

    {
      "name": "PersonDocumentDbTable",
      "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Azure BLOB の出力データセット:**

データは、時間の粒度で特定の日時を反映している BLOB のパスの新しい BLOB に 1 時間ごとにコピーされます。

    {
      "name": "PersonBlobTableOut",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "docdb",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "nullValue": "NULL"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

DocumentDB データベースの「Person」コレクションのサンプル JSON 文書:

    {
      "PersonId": 2,
      "Name": {
        "First": "Jane",
        "Middle": "",
        "Last": "Doe"
      }
    }

DocumentDB では、階層的な JSON 文書に SQL タイプの構文を使用し、文書にクエリを実行できます。

例: 
    As MiddleName, Person.Name.Last AS LastName Person.PersonId, Person.Name.First AS FirstName, Person.Name.Middle を選択してユーザーから

次のパイプラインは DocumentDB データベースの「Person」コレクションから Azure BLOB にデータをコピーします。 コピー アクティビティの一部として、入力データセットと出力データセットが指定されています。

    {
      "name": "DocDbToBlobPipeline",
      "properties": {
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
              "source": {
                "type": "DocumentDbCollectionSource",
                "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
                "nestingSeparator": "."
              },
              "sink": {
                "type": "BlobSink",
                "blobWriterAddHeader": true,
                "writeBatchSize": 1000,
                "writeBatchTimeout": "00:00:59"
              }
            },
            "inputs": [
              {
                "name": "PersonDocumentDbTable"
              }
            ],
            "outputs": [
              {
                "name": "PersonBlobTableOut"
              }
            ],
            "policy": {
              "concurrency": 1
            },
            "name": "CopyFromDocDbToBlob"
          }
        ],
        "start": "2015-04-01T00:00:00Z",
        "end": "2015-04-02T00:00:00Z"
      }
    }

## Sample: Azure BLOB から Azure DocumentDB にデータをコピーします。

下のサンプルで確認できる要素:

1. 型のリンクされたサービス [DocumentDb](#azure-documentdb-linked-service-properties)します。
2. 型のリンクされたサービス [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)します。
3. An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. 出力 [データセット](data-factory-create-datasets.md) 型の [DocumentDbCollection](#azure-documentdb-dataset-type-properties)します。
4. A [パイプライン](data-factory-create-pipelines.md) を使用するコピー アクティビティの [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) と [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties)します。


このサンプルでは、Azure BLOB から Azure DocumentDB にデータがコピーされます。 これらのサンプルで使用される JSON プロパティの説明はサンプルに続くセクションにあります。

**Azure BLOB ストレージのリンクされたサービス:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure DocumentDB のリンクされたサービス:**

    {
      "name": "DocumentDbLinkedService",
      "properties": {
        "type": "DocumentDb",
        "typeProperties": {
          "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
      }
    }

**Azure BLOB の入力データセット:**

    {
      "name": "PersonBlobTableIn",
      "properties": {
        "structure": [
          {
            "name": "Id",
            "type": "Int"
          },
          {
            "name": "FirstName",
            "type": "String"
          },
          {
            "name": "MiddleName",
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
          "fileName": "input.csv",
          "folderPath": "docdb",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "nullValue": "NULL"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Azure DocumentDB 出力データセット:**

このサンプルでは、「Person」という名前のコレクションにデータをコピーします。

    {
      "name": "PersonDocumentDbTableOut",
      "properties": {
        "structure": [
          {
            "name": "Id",
            "type": "Int"
          },
          {
            "name": "Name.First",
            "type": "String"
          },
          {
            "name": "Name.Middle",
            "type": "String"
          },
          {
            "name": "Name.Last",
            "type": "String"
          }
        ],
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

次のパイプラインは、Azure Blob からデータを DocumentDB のユーザー コレクションにコピーします。 コピー アクティビティの一部として、入力データセットと出力データセットが指定されています。

    {
      "name": "BlobToDocDbPipeline",
      "properties": {
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "DocumentDbCollectionSink",
                "nestingSeparator": ".",
                "writeBatchSize": 2,
                "writeBatchTimeout": "00:00:00"
              }
              "translator": {
                  "type": "TabularTranslator",
                  "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, Title: Title, Suffix: Suffix, EmailPromotion: EmailPromotion, rowguid: rowguid, ModifiedDate: ModifiedDate"
              }
            },
            "inputs": [
              {
                "name": "PersonBlobTableIn"
              }
            ],
            "outputs": [
              {
                "name": "PersonDocumentDbTableOut"
              }
            ],
            "policy": {
              "concurrency": 1
            },
            "name": "CopyFromBlobToDocDb"
          }
        ],
        "start": "2015-04-14T00:00:00Z",
        "end": "2015-04-15T00:00:00Z"
      }
    }

サンプル BLOB 入力が次の場合

    1,John,,Doe

DocumentDB の JSON は次のようになります。

    {
      "Id": 1,
      "Name": {
        "First": "John",
        "Middle": null,
        "Last": "Doe"
      },
      "id": "a5e8595c-62ec-4554-a118-3940f4ff70b6"
    }

DocumentDB は JSON 文書の NoSQL ストアであり、入れ子構造が許可されます。 Azure Data Factory を使用して階層を示すためにユーザーをで **nestingSeparator**, 、これは"です"。 この例で。 区切り記号により、コピー アクティビティで「Name」オブジェクトが 3 つの子要素 (First、Middle、Last) で生成されます。これはテーブル定義の「Name.First」、「Name.Middle」、「Name.Last」に基づきます。

## Azure DocumentDB のリンクされたサービスのプロパティ

次の表は、Azure DocumentDB のリンクされたサービスに固有の JSON 要素の説明をまとめたものです。

| **プロパティ**| **説明**| **必須**|
| -------- | ----------- | --------- |
| type| type プロパティを **DocumentDb** に設定する必要があります。| あり|
| connectionString| Azure DocumentDB データベースに接続するために必要な情報を指定します。| あり|

## Azure DocumentDB データベースの type プロパティ

セクションとデータセットの定義に使用できるプロパティの完全な一覧を参照してください、 [データセットを作成する](data-factory-create-datasets.md) 記事です。データセット JSON の構造、可用性、ポリシーなどのセクションはすべてのデータセット型 (Azure SQL、Azure BLOB、Azure テーブルなど) で同じです。

typeProperties セクションはデータセット型ごとに異なり、データ ストアのデータの場所などに関する情報を提供します。 **DocumentDbCollection** 型のデータセットの typeProperties セクションには次のプロパティがあります。

| **プロパティ**| **説明**| **必須**|
| -------- | ----------- | -------- |
| collectionName| DocumentDB ドキュメント コレクションの名前です。| あり|


例:

    {
      "name": "PersonDocumentDbTable",
      "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

## Azure DocumentDB のコピー アクティビティの type プロパティ

セクションとアクティビティの定義に使用できるプロパティの完全な一覧を参照してください、 [パイプラインの作成](data-factory-create-pipelines.md) 記事です。名前、説明、入力テーブル、出力テーブル、さまざまなポリシーなどのプロパティがあらゆる種類のアクティビティで利用できます。

**注:** コピー アクティビティは入力を 1 つだけ受け取り、出力を 1 つだけ生成します。

一方で、アクティビティの typeProperties セクションで利用できるプロパティはアクティビティの種類により異なり、コピー アクティビティの場合、source と sink の種類によって異なります。

Source の種類がコピー アクティビティが発生した場合 **DocumentDbCollectionSource**
次のプロパティをで使用できる **typeProperties** セクション。

| **プロパティ**| **説明**| **使用できる値**| **必須**|
| ------------ | --------------- | ------------------ | ------------ |
| query| データを読み取るためのクエリを指定します。| DocumentDB でサポートされているクエリ文字列。<p>例: SELECT c.BusinessEntityID c.PersonType、c.NameStyle、c.Title c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c 場所 c.ModifiedDate > \"2009-01-01T00:00:00\"</p>| いいえ <p>実行される SQL ステートメントを指定しない場合: 選択 <columns defined in structure> mycollection から </p>
| nestingSeparator| ドキュメントが入れ子であることを示す特殊文字| 任意の文字。<p>DocumentDB は、入れ子になった構造体が許可されている JSON 文書の NoSQL ストアです。Azure Data Factory を利用すると、nestingSeparator で階層が示されます。上記の例では「.」です。区切り記号でコピー アクティビティはオブジェクトの生成、"Name"3 つの子要素を持つ最初に、中間、最後に、「Name.First」、「Name.Middle」、「name.last」テーブルの定義に基づきます。</p>| いいえ

**DocumentDbCollectionSink** では次のプロパティがサポートされます。

| **プロパティ**| **説明**| **使用できる値**| **必須**|
| -------- | ----------- | -------------- | -------- |
| nestingSeparator| 入れ子になった文書が必要であることを示すソース列名の特殊文字。<p>上記の例: 出力テーブルの Name.First は DocumentDB 文書では、次の JSON 構造を生成する:</p><p>"Name": {<br/>  "First":"John"<br/>}、</p>| 入れ子のレベルを区切るために使用される文字。<p>既定値です。(ドット) です。</p>| 入れ子レベルの分割に使用される文字。<p>既定値です。(ドット) です。</p>| いいえ|
| writeBatchSize| ドキュメントを作成する DocumentDB サービスを並列要求の数。<p>このプロパティを使用してデータを DocumentDB との間をコピーすると、パフォーマンスを細かく調整できます。writeBatchSize を増やすとパフォーマンスが良くなります。DocumentDB に送信される並列要求の数が増えるためです。調整は回避する必要がありますが、エラー メッセージをスローできます。「Request rate is large"です。</p><p>スロットルは文書内の用語の数、ドキュメントのサイズを含む、インデックス作成対象となるコレクションなどのポリシーには、さまざまな要因によって決まります。コピー操作で使用可能なスループットを最大限にして、もっと良いコレクション (S3 など) を使用することができます (2,500 要求単位/秒)。</p>| 整数値| いいえ|
| writeBatchTimeout| タイムアウトする前に操作の完了を待つ時間です。| (単位 = 時間) 例: “00:30:00” (30 分)| いいえ|

## 付録

1. **質問:** 
    コピー アクティビティのサポートは、既存のレコードの更新しますか。

    **回答:** 
    番号

2. **質問:** 
    DocumentDB へのコピーの再試行は、既にコピーしたレコードをどのように処理でしょうか。

    **回答:** 
    レコードに"ID"フィールドがあり、ユーザーが同じ ID を持つレコードを挿入しようとするコピー操作は、コピー操作はエラーをスローします。

3. **質問:**
    Data Factory をサポートして [範囲やハッシュ ベースのデータがパーティション分割](https://azure.microsoft.com/documentation/articles/documentdb-partition-data/)でしょうか。

    **回答:**
    番号
4. **質問:**
    テーブルの 1 つ以上の DocumentDB コレクションを指定できますか。

    **回答:**
    番号 この時点では、1 つのコレクションを指定できます。






