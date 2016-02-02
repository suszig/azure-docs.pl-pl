<properties 
    pageTitle="インデクサーを使用した DocumentDB と Azure Search の接続 | Microsoft Azure" 
    description="この記事では、Azure Search インデクサーとデータ ソースとして DocumentDB を使用する方法を説明します。"
    services="documentdb" 
    documentationCenter="" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="mimig"/>

<tags 
    ms.service="documentdb" 
    ms.devlang="rest-api" 
    ms.topic="article" 
    ms.tgt_pltfrm="NA" 
    ms.workload="data-services" 
    ms.date="10/28/2015" 
    ms.author="anhoh"/>


# インデクサーを使用した DocumentDB と Azure Search の接続

DocumentDB データの優れた検索機能を実装する場合は、DocumentDB の Azure Search インデクサーをご使用ください。 この記事では、インデックス作成のインフラストラクチャを保守するコードを記述することなく Azure DocumentDB と Azure Search を統合する方法を説明します。

これを設定するには、する必要がある [Azure Search アカウントをセットアップ](../search-get-started.md#start-with-the-free-service) (がなくても標準の検索にアップグレードする)、しを呼び出す、 [Azure Search REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) DocumentDB を作成する **データ ソース** と **インデクサー** データ ソースのためです。

## <a id="Concepts"></a>Azure Search インデクサーの概念

Azure Search では、データ ソース (DocumentDB を含む) とそのデータ ソースに対して動作するインデクサーの作成および管理をサポートしています。

**データ ソース**により、インデックスを作成する必要があるデータ、データにアクセスするための資格情報、および Azure Search でのデータの変更 (コレクション内のドキュメントの変更や削除など) を効率的に識別するためのポリシーを指定します。 データ ソースは、複数のインデクサーから使用できるように、独立したリソースとして定義します。

**インデクサー**は、データ ソースからターゲットの検索インデックスにデータが流れるしくみを説明します。 ターゲット インデックスとデータ ソースの組み合わせごとにインデクサーを 1 つ作成するように設計する必要があります。 複数のインデクサーから同じインデックスへ書き込むことができますが、1 つのインデクサーから書き込むことができるのは 1 つのインデックスのみです。 インデクサーは次の目的で使用します。

- 1 回限りのデータのコピーを実行して、インデックスを作成する。
- スケジュールに従ってデータ ソースの変更とインデックスを同期する。 スケジュールは、インデクサーの定義の一部です。
- 必要に応じてインデックスのオンデマンド更新を呼び出す。

## <a id="CreateDataSource"></a>手順 1: データ ソースを作成します。

HTTP POST 要求を発行して、次の要求ヘッダーを含む新しいデータ ソースを Azure Search サービスに作成します。

    POST https://[Search service name].search.windows.net/datasources?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]

`Api バージョン` が必要です。 有効な値は `2015年-02-28` 以降のバージョン。

要求の本文には、次のフィールドを含むデータ ソースの定義が含まれている必要があります。

- **name**: データ ソースの名前。

- **型**: 使用 `documentdb`します。

- **credentials**:

    - **connectionString**: 必須。次の形式で Azure DocumentDB データベースへの接続情報を指定します: `AccountEndpoint < DocumentDB エンドポイントの url > を =;AccountKey < DocumentDB 認証キー > を =; Database = < DocumentDB データベース id >`

- **container**:

    - **name**: 必須。 インデックスを作成する DocumentDB コレクションを指定します。

    - **query**: 省略可能。 任意の JSON ドキュメントを、Azure Search がインデックスを作成できるフラット スキーマにフラット化するクエリを指定できます。

- **dataChangeDetectionPolicy**: 省略可能。 参照してください [データ変更検出ポリシー](#DataChangeDetectionPolicy) 以下です。

- **dataDeletionDetectionPolicy**: 省略可能。 参照してください [データ削除検出ポリシー](#DataDeletionDetectionPolicy) 以下です。

### <a id="DataChangeDetectionPolicy"></a>ドキュメントの変更のキャプチャ

データ変更の検出ポリシーの目的は、変更されたデータ項目を効率的に識別することです。 現在、唯一サポートされているポリシーは、 `High Watermark` ポリシーを使用して、 `_ts` 最終更新タイムスタンプ プロパティを次のように指定されている、DocumentDB によって指定します。

    { 
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "_ts" 
    } 

追加する必要がありますも `_ts` 投影でと `、` 、クエリの句。 次に例を示します。

    SELECT s.id, s.Title, s.Abstract, s._ts FROM Sessions s WHERE s._ts > @HighWaterMark

### <a id="DataDeletionDetectionPolicy"></a>ドキュメントの削除のキャプチャ

ソース テーブルから行が削除されると、検索インデックスからも同様に行を削除する必要があります。 データ削除の検出ポリシーの目的は、削除されたデータ項目を効率的に識別することです。 現在、唯一サポートされているポリシーは、 `論理的な削除` ポリシー (削除は何らかのフラグでマークされた)、次のように指定します。

    { 
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the property that specifies whether a document was deleted", 
        "softDeleteMarkerValue" : "the value that identifies a document as deleted" 
    }

> [AZURE.NOTE] カスタムのプロジェクションを使用している場合は、SELECT 句にプロパティを含める必要があります。

### <a id="CreateDataSourceExample"></a>要求本文の例

次の例では、カスタム クエリとポリシーのヒントと一緒にデータ ソースを作成します。

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": {
            "name": "myDocDbCollectionId",
            "query": "SELECT s.id, s.Title, s.Abstract, s._ts FROM Sessions s WHERE s._ts > @HighWaterMark" 
        },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        },
        "dataDeletionDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName": "isDeleted",
            "softDeleteMarkerValue": "true"
        }
    }

### 応答

データ ソースが正常に作成された場合は、HTTP 201 Created 応答を受け取ります。

## <a id="CreateIndex"></a>手順 2: インデックスを作成します。

ターゲットの Azure Search インデックスがまだない場合は、インデックスを作成します。 これを行うことができます、 [Azure クラシック ポータル UI](../search-get-started.md#test-service-operations) またはを使用して、 [インデックス作成 API](https://msdn.microsoft.com/library/azure/dn798941.aspx)します。

    POST https://[Search service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]

ターゲット インデックスのスキーマがソース JSON ドキュメントのスキーマまたはカスタムのクエリ プロジェクションの出力と互換性があることを確認します。

### 図 A: JSON データ型と Azure Search データ型間のマッピング

| JSON データ型| 互換性のあるターゲット インデックス フィールドの型|
|---|---|
| ブール値| Edm.Boolean、Edm.String|
| 整数などの数値| Edm.Int32、Edm.Int64、Edm.String|
| 浮動小数点などの数値| Edm.Double、Edm.String|
| String| Edm.String|
| プリミティブ型の配列。例: "a"、"b"、"c"| Collection(Edm.String)|
| 日付などの文字列| Edm.DateTimeOffset、Edm.String|
| GeoJSON オブジェクト。例: { "type": "Point", "coordinates": [ long, lat ] }| Edm.GeographyPoint|
| その他の JSON オブジェクト| 該当なし|

### <a id="CreateIndexExample"></a>要求本文の例

次の例では、ID と説明のフィールドを持つインデックスを作成します。

    {
       "name": "mysearchindex",
       "fields": [{
         "name": "id",
         "type": "Edm.String",
         "key": true,
         "searchable": false
       }, {
         "name": "description",
         "type": "Edm.String",
         "filterable": false,
         "sortable": false,
         "facetable": false,
         "suggestions": true
       }]
     }

### Response

インデックスが正常に作成された場合、HTTP 201 Created 応答を受け取ります。

## <a id="CreateIndexer"></a>手順 3: インデクサーを作成します。

HTTP POST 要求を次のヘッダーと共に使用して、Azure Search サービス内に新しいインデクサーを作成できます。

    POST https://[Search service name].search.windows.net/indexers?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]

要求の本文には、次のフィールドを含むインデクサーの定義が含まれている必要があります。

- **name**: 必須。 インデクサーの名前。

- **dataSourceName**: 必須。 既存のデータ ソースの名前。

- **targetIndexName**: 必須。 既存のインデックスの名前。

- **schedule**: 省略可能。 参照してください [インデックス作成のスケジュール](#IndexingSchedule) 以下です。

### <a id="IndexingSchedule"></a>スケジュールに従ったインデクサーの実行

インデクサーには、必要に応じてスケジュールを指定できます。 スケジュールが存在する場合、インデクサーはスケジュールに従って定期的に実行されます。 スケジュールには次の属性があります。

- **interval**: 必須。 インデクサーが実行される間隔または期間を指定する時間の値。 許可される最短の間隔は 5 分です。最長は 1 日です。 XSD"dayTimeDuration"値としてフォーマットされている必要があります (の制限されたサブセットが [ISO 8601 期間](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) 値)。 このパターンは、: `P(nD)(T(nH)(nM))`します。 例: `PT15M` 、15 分ごとに `PT2H` 2 時間ごとです。

- **startTime**: 必須。 インデクサーの実行を開始する時刻を指定する UTC 日時。

### <a id="CreateIndexerExample"></a>要求本文の例

次の例は、データによって参照されるコレクションをコピーするインデクサーを作成、 `myDocDbDataSource` データ ソース、 `mySearchIndex` UTC 2015 年 1 月 1 日に開始して 1 時間ごとに実行するスケジュール上のインデックス。

    {
        "name" : "mysearchindexer",
        "dataSourceName" : "mydocdbdatasource",
        "targetIndexName" : "mysearchindex",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" }
    }

### Response

インデクサーが正常に作成された場合、HTTP 201 Created 応答を受け取ります。

## <a id="RunIndexer"></a>手順 4: インデクサーを実行します。

スケジュールに従って定期的に実行されるだけでなく、次の HTTP POST 要求を発行して、オンデマンドでインデクサーを呼び出すこともできます。

    POST https://[Search service name].search.windows.net/indexers/[indexer name]/run?api-version=[api-version]
    api-key: [Search service admin key]

### 応答

インデクサーが正常に呼び出された場合、HTTP 202 Accepted 応答を受け取ります。

## <a name="GetIndexerStatus"></a>手順 5: インデクサー状態を取得します。

HTTP GET 要求を発行して、インデクサーの現在の状態と実行の履歴を取得することができます。

    GET https://[Search service name].search.windows.net/indexers/[indexer name]/status?api-version=[api-version]
    api-key: [Search service admin key]

### 応答

インデクサーの全体的な正常性状態、インデクサーの最後の呼び出し、および (存在する場合は) インデクサー呼び出しの最近の履歴に関する情報が含まれる応答の本文と共に、HTTP 200 OK 応答が表示されます。

応答は、次のようになります。

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

実行履歴には、最近完了した 50 件の実行内容が含まれ、時系列の逆の順に並べられます (そのため、最近の実行内容が応答の最初に表示されます)。

## <a name="NextSteps"></a>次のステップ

ご利用ありがとうございます。 DocumentDB のインデクサーを使用して、Azure DocumentDB を Azure Search と統合する方法についての説明は以上で終了です。

 - Azure DocumentDB の詳細についてをクリックして [ここ](/services/documentdb/)します。

 - Azure Search についての詳細については、次のようにクリックします。 [ここ](/services/search/)します。






