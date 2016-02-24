<properties
    pageTitle="Node.js から Azure Table ストレージを使用する方法 | Microsoft Azure"
    description="Azure Table Storage を使用する方法について説明します。 コード サンプルは Node.js API を使用して記述されています。"
    services="storage"
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="12/01/2015"
    ms.author="robmcm"/>


# Node.js から Azure Table Storage を使用する方法

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]


## 概要

このトピックでは、Node.js アプリケーションで Azure Table サービスを使用して一般的なシナリオを実行する方法について説明します。

このトピックのコード例は、既に Node.js アプリケーションがあることを前提としています。 Azure で Node.js アプリケーションを作成する方法については、次のいずれかのトピックを参照してください。

- [Node.js Web サイトの構築と Azure へのデプロイ](Node.js アプリケーションの作成と Azure Web サイトへのデプロイ)
- [WebMatrix を使用した Node.js Web サイトの構築と Azure へのデプロイ](WebMatrix を使用した Node.js アプリケーションの作成と Azure へのデプロイ)
- [Node.js アプリケーションの構築と Azure クラウド サービスへのデプロイ](Node.js クラウド サービス) (Windows PowerShell を使用)


[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]


## Azure Storage にアクセスするようにアプリケーションを構成する

Azure Storage を使用する必要があります、Azure Storage SDK for Node.js に便利なライブラリのセットが含まれています。
便利なライブラリのセットが含まれています。

### ノード パッケージ マネージャー (NPM) を使用してパッケージをインストールする

1.  コマンド ライン インターフェイスを使用して **PowerShell** (Windows)、 **ターミナル** (Mac)、または **Bash** (Unix) し、アプリケーションを作成したフォルダーに移動します。

2.  型 **npm install** コマンド ウィンドウにします。 このコマンドの出力は次の例のようになります。

        azure-storage@0.5.0 node_modules\azure-storage
        +-- extend@1.2.1
        +-- xmlbuilder@0.4.3
        +-- mime@1.2.11
        +-- node-uuid@1.4.3
        +-- validator@3.22.2
        +-- underscore@1.4.4
        +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
        +-- xml2js@0.2.7 (sax@0.5.2)
        +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)

3.  手動で実行することができます、 **%.*ls** いることを確認するにはコマンド、 **node \_modules** フォルダーが作成されました。 そのフォルダーがあります。、 **azure ストレージ** パッケージで、ストレージにアクセスするために必要なライブラリが含まれています。

### パッケージをインポートする

先頭に次のコードを追加、 **server.js** アプリケーションのファイル。

    var azure = require('azure-storage');

## Azure のストレージ接続文字列の設定

azure モジュールは、Azure のストレージ アカウントに接続するために必要な情報として、環境変数 AZURE\_STORAGE\_ACCOUNT および AZURE\_STORAGE\_ACCESS\_KEY、または AZURE\_STORAGE\_CONNECTION\_STRING を読み取ります。 これらの環境変数が設定されていない場合を呼び出すときにアカウント情報を指定する必要があります **TableService**します。

環境変数を設定の例については、 [Azure ポータル](portal.azure.com) Azure の web サイトを参照してください。 [Node.js web application with Storage]

## テーブルの作成

次のコード作成、 **TableService** オブジェクトを使用して、新しいテーブルを作成します。 次のコードの先頭付近にある追加 **server.js**します。

    var tableSvc = azure.createTableService();

呼び出し **createTableIfNotExists** が既に存在しない場合、指定した名前の新しいテーブルが作成されます。 次の例では、"mytable" という名前のテーブルが存在しない場合、このテーブルを作成します。

    tableSvc.createTableIfNotExists('mytable', function(error, result, response){
        if(!error){
            // Table exists or created
        }
    });

新しいテーブルを作成する場合、`result`は `true` になります。テーブルが既に存在する場合は `false` になります。 `response`には要求に関する情報が含まれます。

### フィルター

オプションのフィルター操作を使用して行われる操作に適用できます **TableService**します。 フィルター操作には、ログや自動的な再試行などが含まれる場合があります。フィルターは、次のシグネチャを持つメソッドを実装するオブジェクトです。

        function handle (requestOptions, next)

要求オプションに対するプリプロセスを行った後で、このメソッドは "next" を呼び出して、次のシグネチャのコールバックを渡す必要があります。

        function (returnObject, finalCallback, next)

このコールバックで、returnObject (サーバーへの要求からの応答) の処理の後に、コールバックは next を呼び出すか (他のフィルターの処理を続けるために next が存在する場合)、単に finalCallback を呼び出す必要があります (サービス呼び出しを終了する場合)。

再試行ロジックを実装する 2 つのフィルターが Azure SDK for Node.js に含まれて **ExponentialRetryPolicyFilter** と **LinearRetryPolicyFilter**します。 次の作成、 **TableService** を使用するオブジェクト、 **ExponentialRetryPolicyFilter**:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var tableSvc = azure.createTableService().withFilter(retryOperations);

## エンティティをテーブルに追加する

エンティティを追加するには、エンティティのプロパティを定義するオブジェクトを最初に作成します。 すべてのエンティティを含める必要があります、 **PartitionKey** と **RowKey**, 、これは、エンティティの一意の識別子。

* **PartitionKey** -にエンティティが格納されているパーティションを決定

* **RowKey** - 一意にパーティション内のエンティティを識別します。

両方とも **PartitionKey** と **RowKey** 文字列値にする必要があります。 詳細については、次を参照してください。 [テーブル サービス データ モデルについて](http://msdn.microsoft.com/library/azure/dd179338.aspx)します。

エンティティを定義する例を次に示します。 なお **dueDate** の型として定義されて **Edm.DateTime**します。 型の指定は省略可能です。型を指定しなかった場合、型は推測されます。

    var task = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'},
      description: {'_':'take out the trash'},
      dueDate: {'_':new Date(2015, 6, 20), '$':'Edm.DateTime'}
    };

> [AZURE.NOTE]  **タイムスタンプ** エンティティが挿入または更新するときに、Azure が設定された各レコードに対応するフィールドです。

使用することも、 **entityGenerator** エンティティを作成します。 次の例では、同じタスク エンティティを使用して、作成し、 **entityGenerator**します。

    var entGen = azure.TableUtilities.entityGenerator;
    var task = {
      PartitionKey: entGen.String('hometasks'),
      RowKey: entGen.String('1'),
      description: entGen.String('take out the trash'),
      dueDate: entGen.DateTime(new Date(Date.UTC(2015, 6, 20))),
    };

テーブルにエンティティを追加するには、エンティティ オブジェクトを渡します、 **insertEntity** メソッドです。

    tableSvc.insertEntity('mytable',task, function (error, result, response) {
        if(!error){
            // Entity inserted
        }
    });

操作が成功すると場合、 `result` が格納されます、 [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) 挿入されたレコードのおよび `response` 操作に関する情報が含まれます。

応答の例:

    { '.metadata': { etag: 'W/"datetime\'2015-02-25T01%3A22%3A22.5Z\'"' } }

> [AZURE.NOTE] 既定では、 **insertEntity** の一部として、挿入されたエンティティを返さない、 `response` 情報。 このエンティティに対して他の操作を実行する予定がある場合、または情報をキャッシュする場合は、`result` の一部として返されるようにすると便利です。 有効にしてこれを行う **echoContent** 次のようにします。
>
> `tableSvc.insertEntity('mytable', task, {echoContent: true}, function (error, result, response) {...}`

## エンティティを更新する

既存のエンティティを更新するには、複数のメソッドがあります。

* **updateEntity** -既存のエンティティを置換することで更新

* **mergeEntity** -新しいプロパティ値を既存のエンティティに結合することによって既存のエンティティを更新

* **insertOrReplaceEntity** -既存のエンティティを置換することで更新します。 エンティティが存在しない場合は、新しいエンティティが挿入されます。

* **insertOrMergeEntity** -既存に新しいプロパティ値をマージすることで既存のエンティティを更新します。 エンティティが存在しない場合は、新しいエンティティが挿入されます。

次の例は、更新、エンティティを使用して **updateEntity**:

    tableSvc.updateEntity('mytable', updatedTask, function(error, result, response){
      if(!error) {
        // Entity updated
      }
    });

> [AZURE.NOTE] 既定では、エンティティの更新をチェックしませんに更新されているデータが別のプロセスにより変更されていたかどうかを参照してください。 同時更新をサポートするには、次の手順を実行します。
>
> 1. 更新するオブジェクトの ETag を取得します。 これは、任意のエンティティに関連する操作の `response` の一部として返され、`response['.metadata'].etag` を通じて取得できます。
>
> 2. エンティティで更新操作を実行する場合は、以前に取得した ETag 情報を新しいエンティティに追加します。 次に例を示します。
>
>     `entity2['.metadata'].etag = currentEtag;`
>    
> 3. 更新操作を実行します。 アプリケーションの別のインスタンスなど、ETag 値を取得した後でエンティティが更新されている場合は、要求で指定された更新の条件が満たされていないことを示す `error` が返されます。

 **UpdateEntity** と **mergeEntity**, 、更新操作は失敗しに、更新されるエンティティが存在しない場合。 このためにエンティティを格納する場合に関係なくかどうかが既に存在するを使用して **insertOrReplaceEntity** または **insertOrMergeEntity**します。

 `result` が正常に更新操作が含まれている、 **Etag** 更新されたエンティティのです。

## エンティティのグループを操作する

状況によって、複数の操作をバッチとして送信し、サーバーによるアトミック処理を行うことが合理的である場合があります。 これを実現するには、 **TableBatch** クラスをバッチを作成し、使用して、 **executeBatch** メソッドの **TableService** バッチ操作を実行します。

 次の例に、2 つのエンティティをバッチで送信する方法を示します。

    var task1 = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'},
      description: {'_':'Take out the trash'},
      dueDate: {'_':new Date(2015, 6, 20)}
    };
    var task2 = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '2'},
      description: {'_':'Wash the dishes'},
      dueDate: {'_':new Date(2015, 6, 20)}
    };

    var batch = new azure.TableBatch();

    batch.insertEntity(task1, {echoContent: true});
    batch.insertEntity(task2, {echoContent: true});

    tableSvc.executeBatch('mytable', batch, function (error, result, response) {
      if(!error) {
        // Batch completed
      }
    });

バッチ操作が成功した場合、`result` にはバッチ内の各操作の情報が含まれます。

### バッチ操作の処理

バッチに追加された操作は、`operations` プロパティで確認できます。 次のメソッドを使用して操作を処理できます。

* **オフ** -バッチから操作をすべてクリア

* **getOperations** -バッチから操作を取得

* **hasOperations** -バッチに操作が含まれている場合は true を返します

* **removeOperations** -操作を削除

* **サイズ** -バッチ内の操作の数を返します

## キーを使用したエンティティを取得する

基づく特定のエンティティを返す、 **PartitionKey** と **RowKey**, を使用して、 **retrieveEntity** メソッドです。

    tableSvc.retrieveEntity('mytable', 'hometasks', '1', function(error, result, response){
      if(!error){
        // result contains the entity
      }
    });

この操作を完了すると、`result` にはエンティティが含まれます。

## エンティティのセットを照会する

テーブルを照会する、 **TableQuery** 次の句を使用してクエリ式を作成するオブジェクト。

* **選択** -クエリから返されるフィールド

* **ここで** -where 句

    * **** - `and` where 条件

    * **または** - `or` where 条件

* **上部** -取得する項目の数


次の例では、"hometasks" の PartitionKey で最初の 5 つの項目を返すクエリを作成します。

    var query = new azure.TableQuery()
      .top(5)
      .where('PartitionKey eq ?', 'hometasks');

 **選択** を使用しないすべてのフィールドが返されます。 テーブルに対してクエリを実行するには使用 **queryEntities**します。 次の例では、このクエリを使用して "mytable" からエンティティを返します。

    tableSvc.queryEntities('mytable',query, null, function(error, result, response) {
      if(!error) {
        // query was successful
      }
    });

成功した場合は、`result.entries` にはクエリに一致するエンティティの配列が含まれます。 場合は、クエリは、すべてのエンティティを返すことができませんでした `result.continuationToken` なります以外*null* の 3 番目のパラメーターとして使用できる **queryEntities** をさらに結果を取得します。 最初のクエリを使用して *null* 3 番目のパラメーターです。

### エンティティ プロパティのサブセットを照会する

テーブルに対するクエリでは、ごくわずかのフィールドだけをエンティティから取得できます。
この方法では、帯域幅の使用が削減され、クエリのパフォーマンスが向上します。特に、大量のエンティティがある場合に役立ちます。 使用して、 **選択** 句返されるフィールドの名前を渡します。 たとえば、次のクエリはのみを返し、 **説明** と **dueDate** フィールドです。

    var query = new azure.TableQuery()
      .select(['description', 'dueDate'])
      .top(5)
      .where('PartitionKey eq ?', 'hometasks');

## エンティティを削除する

パーティション キーと行キーを使用してエンティティを削除できます。 この例では、 **task1** オブジェクトを含む、 **RowKey** と **PartitionKey** を削除するエンティティの値。 オブジェクトが渡され、 **deleteEntity** メソッドです。

    var task = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'}
    };

    tableSvc.deleteEntity('mytable', task, function(error, response){
      if(!error) {
        // Entity deleted
      }
    });

> [AZURE.NOTE] 項目を削除するに Etag を使用して、項目を別のプロセスによって変更されていないことを確認することを検討してください。 参照してください [エンティティを更新する](#update-an-entity) については Etag を使用します。

## テーブルを削除する

次のコードは、ストレージ アカウントからテーブルを削除します。

    tableSvc.deleteTable('mytable', function(error, response){
        if(!error){
            // Table deleted
        }
    });

テーブルが存在するかどうかがわからない場合は使用して **deleteTableIfExists**します。

## 継続トークンを使用する

テーブルに対するクエリによって大量の結果が返される場合は、継続トークンを探します。 ある可能性があります大量データの可能性がありますいないことを認識するかを認識ビルドしないかどうか、クエリで使用します。
継続トークンが存在します。

エンティティを照会したときに返される結果オブジェクトは、このようなトークンが存在する場合に `continuationToken` プロパティを設定します。 クエリを実行するときにこれを使用して、後続のパーティションおよびテーブル エンティティに移動できます。

クエリを実行するとき、クエリ オブジェクト インスタンスとコールバック関数の間に continuationToken パラメーターを指定することができます。

```
var nextContinuationToken = null;
dc.table.queryEntities(tableName,
    query,
    nextContinuationToken,
    function (error, results) {
        if (error) throw error;

        // iterate through results.entries with results

        if (results.continuationToken) {
            nextContinuationToken = results.continuationToken;
        }

    });
```

`continuationToken` オブジェクトには `nextPartitionKey`、`nextRowKey`、`targetLocation` などのプロパティがあり、これらを使用してすべての結果を反復処理できます。

GitHub の Azure Storage の Node.js のリポジトリには継続のサンプルも用意されています。 `examples/samples/continuationsample.js` を参照してください。

## 共有アクセス署名を操作する

共有アクセス署名 (SAS) は、ストレージ アカウントの名前またはキーを指定せずにテーブルへの細密なアクセスを提供する安全な方法です。 多くの場合、SAS は、モバイル アプリでのレコードの照会などデータへの限定的なアクセスのために使用されます。

クラウド ベースのサービスなど信頼されたアプリケーションを使用して SAS を生成、 **generateSharedAccessSignature** の **TableService**, 、し、モバイル アプリなどの信頼されていないか部分的に信頼されたアプリケーションに提供します。 SAS は、SAS が有効である期間の開始日と終了日のほか、SAS の保有者に付与されたアクセス レベルを示したポリシーを使用して生成されます。

次の例では、SAS の保有者に対してテーブルのクエリ ('r') を許可し、作成時から 100 分後に期限が切れる、新しい共有アクセス ポリシーを作成しています。

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
        Start: startDate,
        Expiry: expiryDate
      },
    };

    var tableSAS = tableSvc.generateSharedAccessSignature('mytable', sharedAccessPolicy);
    var host = tableSvc.host;

SAS の保有者がテーブルにアクセスするときに必要なホスト情報も提供する必要があることに注意してください。

クライアント アプリケーションは、この SAS とを使用して **TableServiceWithSAS** テーブルに対する操作を実行します。 次の例では、テーブルに接続してクエリを実行しています。

    var sharedTableService = azure.createTableServiceWithSas(host, tableSAS);
    var query = azure.TableQuery()
      .where('PartitionKey eq ?', 'hometasks');

    sharedTableService.queryEntities(query, null, function(error, result, response) {
      if(!error) {
        // result contains the entities
      }
    });

SAS がクエリ アクセスのみで生成された場合は、エンティティの挿入、更新、または削除を試行するとエラーが返されます。

### アクセス制御リスト

SAS のアクセス ポリシーを設定するために、アクセス制御リスト (ACL) も使用できます。 複数のクライアントにテーブルへのアクセスを許可し、各クライアントに異なるアクセス ポリシーを提供する場合に便利です。

ACL は、アクセス ポリシーの配列と、各ポリシーに関連付けられた ID を使用して実装されます。 次の例では、2 つのポリシーを定義しています。1 つは "user1" 用、もう 1 つは "user2" 用です。

    var sharedAccessPolicy = [
      {
        AccessPolicy: {
          Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
          Start: startDate,
          Expiry: expiryDate
        },
        Id: 'user1'
      },
      {
        AccessPolicy: {
          Permissions: azure.TableUtilities.SharedAccessPermissions.ADD,
          Start: startDate,
          Expiry: expiryDate
        },
        Id: 'user2'
      }
    ];

次の例の現在の ACL を取得する、 **hometasks** を使用して新しいポリシーを追加し、 **setTableAcl**します。 この手法で以下を実行できます。

    tableSvc.getTableAcl('hometasks', function(error, result, response) {
      if(!error){
        //push the new policy into signedIdentifiers
        result.signedIdentifiers.push(sharedAccessPolicy);
        tableSvc.setTableAcl('hometasks', result, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

ACL を設定した後で、ポリシーの ID に基づいて SAS を作成できます。 次の例では、"user2" 用に新しい SAS を作成しています。

    tableSAS = tableSvc.generateSharedAccessSignature('hometasks', { Id: 'user2' });

## 次のステップ

詳細については、次のリソースを参照してください。

-   [Azure ストレージ チーム ブログ][]します。
-   [Azure Storage SDK for Node][] GitHub のリポジトリです。
-   [Node.js デベロッパー センター](/develop/nodejs/)

  [Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node
  [OData.org]: http://www.odata.org/
  [Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
  [Azure Portal]: portal.azure.com

  [Node.js Cloud Service]: ../cloud-services-nodejs-develop-deploy-app.md
  [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Website with WebMatrix]: ../web-sites-nodejs-use-webmatrix.md
  [Node.js Cloud Service with Storage]: ../storage-nodejs-use-table-storage-cloud-service-app.md
  [Node.js web application with Storage]: ../storage-nodejs-use-table-storage-web-site.md
  [Create and deploy a Node.js application to an Azure website]: ../web-sites-nodejs-develop-deploy-mac.md

