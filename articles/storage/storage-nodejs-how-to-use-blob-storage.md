<properties
    pageTitle="Node.js から BLOB ストレージを使用する方法 | Microsoft Azure"
    description="Azure BLOB サービスを使用して、BLOB の内容をアップロード、ダウンロード、一覧表示、および削除する方法について説明します。 サンプルは Node.js で記述されています。"
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



# Node.js から BLOB ストレージを使用する方法

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## 概要

この記事では、Azure BLOB サービスを使用して一般的なシナリオを実行する方法について説明します。 サンプルは Node.js API を使用して記述されています。 紹介するシナリオには、アップロード、一覧表示、ダウンロード、および blob を削除する方法が含まれます。

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## Node.js アプリケーションの作成

Node.js アプリケーションを作成する方法の手順については、次を参照してください。 [作成と Azure の web サイトに Node.js アプリケーションを展開]、 [Node.js クラウド サービス][Node.js Cloud Service] (Windows PowerShell を使用)、または [Web app with WebMatrix]します。

## アプリケーションのストレージへのアクセスの構成

Azure Storage を使用するには、Azure Storage SDK for Node.js が必要です。ここには、ストレージ REST サービスと通信するための便利なライブラリのセットが含まれています。

### ノード パッケージ マネージャー (NPM) を使用してパッケージを取得する

1.  コマンド ライン インターフェイスを使用して **PowerShell** (Windows)、 **ターミナル** (Mac)、または **Bash** (Unix)、サンプル アプリケーションを作成したフォルダーに移動します。

2.  型 **npm install** コマンド ウィンドウにします。 コマンドの出力は次のコード例に似ています。

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

3.  手動で実行することができます、 **%.*ls** いることを確認するにはコマンド、 **node \_modules** フォルダーが作成されました。 そのフォルダーで検索、 **azure ストレージ** パッケージで、ストレージにアクセスする必要があるライブラリが含まれています。

### パッケージをインポートする

先頭に次のコードを追加メモ帳または別のテキスト エディターを使用して、 **server.js** 記憶域を使用するアプリケーションのファイル。

    var azure = require('azure-storage');

## Azure のストレージ接続文字列の設定

Azure モジュールは、Azure ストレージ アカウントに接続するために必要な情報として、環境変数 `AZURE_STORAGE_ACCOUNT` と `AZURE_STORAGE_ACCESS_KEY`、または `AZURE_STORAGE_CONNECTION_STRING` を読み取ります。 これらの環境変数が設定されていない場合を呼び出すときにアカウント情報を指定する必要があります **createBlobService**します。

環境変数を設定の例については、 [Azure ポータル](portal.azure.com) Azure web アプリでは、次を参照してください。 [Node.js Web Application with Storage]

## コンテナーを作成する

 **BlobService** オブジェクトでは、コンテナーおよび blob を操作することができます。 次のコード作成、 **BlobService** オブジェクトです。 次のコードの先頭付近にある追加 **server.js**:

    var blobSvc = azure.createBlobService();

> [AZURE.NOTE] 使用して、blob を匿名でアクセス **createBlobServiceAnonymous** ホスト アドレスを指定するとします。 たとえば、`var blobSvc = azure.createBlobServiceAnonymous('https://myblob.blob.core.windows.net/');` を使用します。

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

新しいコンテナーを作成するには使用 **createContainerIfNotExists**します。 次のコード例では、'mycontainer' という名前の新しいコンテナーを作成します。

    blobSvc.createContainerIfNotExists('mycontainer', function(error, result, response){
      if(!error){
        // Container exists and allows
        // anonymous read access to blob
        // content and metadata within this container
      }
    });

コンテナーが新規に作成された場合、`result` は true です。 コンテナーが既に存在する場合 `result` は false。 `response` を、操作に関する情報が含まれますを含め、 [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) 、コンテナーの情報です。

### コンテナーのセキュリティ

既定では、新しいコンテナーはプライベートであり、匿名でアクセスすることはできません。 コンテナー公開して匿名でアクセスできるように、アクセスを設定、コンテナーのレベルを **blob** または **コンテナー**します。

* **blob** -コンテナー内のすべての blob のリストなどのコンテナーのメタデータではなく、blob の内容およびこのコンテナー内のメタデータに匿名の読み取りアクセスを許可

* **コンテナー** -blob の内容とメタデータだけでなくコンテナーのメタデータへの匿名読み取りアクセスを許可

次のコード例では、アクセスのレベルを設定を示しています **blob**:

    blobSvc.createContainerIfNotExists('mycontainer', {publicAccessLevel : 'blob'}, function(error, result, response){
      if(!error){
        // Container exists and is private
      }
    });

使用して、コンテナーのアクセス レベルを変更する代わりに、 **setContainerAcl** アクセス レベルを指定します。 次のコード例では、アクセス レベルを container に変更します。

    blobSvc.setContainerAcl('mycontainer', null /* signedIdentifiers */, 'container' /* publicAccessLevel*/, function(error, result, response){
      if(!error){
        // Container access level set to 'container'
      }
    });

結果には、現在を含む、操作に関する情報が含まれています。 **ETag** コンテナーです。

### フィルター

オプションのフィルター操作を適用するにを使用して行われる操作に **BlobService**します。 フィルター操作には、ログ、自動的な再試行などが含まれる場合があります。フィルターは、次のシグネチャを持つメソッドを実装するオブジェクトです。

        function handle (requestOptions, next)

要求オプションに対するプリプロセスを行った後で、このメソッドは "next" を呼び出して、次のシグネチャのコールバックを渡す必要があります。

        function (returnObject, finalCallback, next)

このコールバックで、returnObject (サーバーへの要求からの応答) の処理の後に、コールバックは next を呼び出すか (他のフィルターの処理を続けるために next が存在する場合)、単に finalCallback を呼び出す必要があります (サービス呼び出しを終了する場合)。

再試行ロジックを実装する 2 つのフィルターが Azure SDK for Node.js に含まれて **ExponentialRetryPolicyFilter** と **LinearRetryPolicyFilter**します。 次の作成、 **BlobService** を使用するオブジェクト、 **ExponentialRetryPolicyFilter**:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var blobSvc = azure.createBlobService().withFilter(retryOperations);

## コンテナーに BLOB をアップロードする

BLOB はブロックベースまたはページ ベースのいずれにもできます。 ブロック blob は大量のデータを効率的にアップロードできる一方、ページ blob は読み取りと書き込みの操作に適しています。 詳細については、次を参照してください。 [ブロック blob とページ blob について](http://msdn.microsoft.com/library/azure/ee691964.aspx)します。

### ブロック blob

データをブロック blob にアップロードするには、以下のメソッドを使用します。

* **createBlockBlobFromLocalFile** - 新しいブロック blob を作成し、ファイルの内容をアップロード

* **createBlockBlobFromStream** - 新しいブロック blob を作成し、ストリームの内容をアップロード

* **createBlockBlobFromText** - 新しいブロック blob を作成し、文字列の内容をアップロード

* **createWriteStreamToBlockBlob** -ブロック blob への書き込みストリームを提供します。

次のコード例の内容をアップロードする、 **test.txt** ファイルを **myblob**します。

    blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

 `result` これらのメソッドによって返される、操作に関する情報を含む、 **ETag** blob のです。

### ページ blob

データをページ blob にアップロードするには、以下のメソッドを使用します。

* **createPageBlob** -特定の長さの新しいページ blob を作成

* **createPageBlobFromLocalFile** - 新しいページ blob を作成し、ファイルの内容をアップロード

* **createPageBlobFromStream** - を新しいページ blob を作成してストリームの内容をアップロード

* **createWriteStreamToExistingPageBlob** -既存のページ blob への書き込みストリームを提供します。

* **createWriteStreamToNewPageBlob** - 新しい blob を作成しに書き込むためのストリームを提供

次のコード例の内容をアップロードする、 **test.txt** ファイルを **mypageblob**します。

    blobSvc.createPageBlobFromLocalFile('mycontainer', 'mypageblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

> [AZURE.NOTE] ページ blob は、512 バイトの「ページ」で構成されます。 512 の倍数でないサイズのデータをアップロードするとエラーになる場合があります。

## コンテナー内の BLOB を一覧表示する

コンテナー内の blob の一覧を表示する、 **listBlobsSegmented** メソッドです。 特定のプレフィックスと共に blob を返す場合を使用して **listBlobsSegmentedWithPrefix**します。

    blobSvc.listBlobsSegmented('mycontainer', null, function(error, result, response){
      if(!error){
        // result.entries contains the entries
        // If not all blobs were returned, result.continuationToken has the continuation token.
      }
    });

`result` には `entries` コレクションが含まれます。これは、各 BLOB を記述するオブジェクトの配列です。 すべての BLOB を返すことができない場合は、`result` は、`continuationToken` も提供します。これは、追加のエントリを取得するための 2 つ目のパラメーターとして使用できます。

## BLOB をダウンロードする

BLOB からデータをダウンロードするには、以下のメソッドを使用します。

* **getBlobToLocalFile** -blob の内容をファイルに書き込みます

* **getBlobToStream** -blob の内容をストリームに書き込みます

* **getBlobToText** -blob の内容を文字列に書き込みます

* **createReadStream** -blob から読み取るためのストリームを提供します。

次のコード例では、使用方法を示します **getBlobToStream** の内容をダウンロード、 **myblob** blob を格納する、 **output.txt** ストリームを使用して、ファイル。

    var fs = require('fs');
    blobSvc.getBlobToStream('mycontainer', 'myblob', fs.createWriteStream('output.txt'), function(error, result, response){
      if(!error){
        // blob retrieved
      }
    });

 `result` 、Blob に関する情報を含むなど **ETag** 情報。

## BLOB を削除する

最後に、blob を削除する呼び出し **deleteBlob**します。 次のコード例は、という名前の blob を削除 **myblob**します。

    blobSvc.deleteBlob(containerName, 'myblob', function(error, response){
      if(!error){
        // Blob has been deleted
      }
    });

## 同時アクセス

複数のクライアントまたは複数のプロセス インスタンスを blob に同時アクセスをサポートする使用 **Etag** または **リース**します。

* **Etag** -blob またはコンテナーを別のプロセスによって変更されていることを検出する手段を提供します。

* **リース** に排他的で更新可能な取得、書き込み、または一定期間、blob へのアクセスを削除する方法を提供

### ETag

ETag は、複数のクライアントまたはインスタンスからの BLOB への同時書き込みを許可する必要がある場合に使用してください。 ETag を使用すると、最初の読み取りまたは作成以降にコンテナーまたは BLOB が変更されているかどうかを確認できることから、別のクライアントまたはプロセスによってコミットされた変更の上書きを回避できます。

ETag の条件は、オプションの `options.accessConditions` パラメーターを使用して設定できます。 次のコード例のアップロードだけ、 **test.txt** に含まれている場合は、blob が既に存在し、ETag 値を持つファイル `etagToMatch`します。

    blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', { accessConditions: { 'if-match': etagToMatch} }, function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

ETag を使用している場合、一般的なパターンは次のとおりです。

1. 作成、一覧表示、取得の操作の結果として ETag を取得する。

2. ETag 値が変更されていないことを確認するアクションを実行する。

値が変更されていた場合は、ETag 値を取得して以降、別のクライアントまたはインスタンスがこの BLOB またはコンテナーを変更したことを示しています。

### 占有

新しいリースを取得するにを使用して、 **acquireLease** 占有を取得する blob またはコンテナーを指定して、メソッドです。 たとえば、次のコードの占有を取得 **myblob**します。

    blobSvc.acquireLease('mycontainer', 'myblob', function(error, result, response){
      if(!error) {
        console.log('leaseId: ' + result.id);
      }
    });

後続の処理を **myblob** を提供する必要があります、 `options.leaseId` パラメーター。 ID として返されるリース `result.id` から **acquireLease**します。

> [AZURE.NOTE] 既定では、リース期間は無限です。 `options.leaseDuration` パラメーターを指定することで、有限の期間 (15 ～ 60 秒) を指定できます。

削除するには、リースを使用して **releaseLease**します。 使用する、リースを中断するものの、元の期間が終了するまで新しい占有が取得されないようにする、 **breakLease**します。

## 共有アクセス署名を操作する

共有アクセス署名 (SAS) は、ストレージ アカウント名やキーを指定せずに BLOB やコンテナーへのきめ細やかで安全なアクセスを提供する方法です。 SAS は、モバイル アプリからの BLOB へのアクセスを許可する場合など、データへの制限されたアクセスを提供する場合によく使用されます。

> [AZURE.NOTE] Blob への匿名アクセスを許可することも、中には、共有アクセス署名を使用すると、SAS を生成する必要がありますより制御されたアクセスを提供できます。

クラウド ベースのサービスなど信頼されたアプリケーションを使用して共有アクセス署名の生成、 **generateSharedAccessSignature** の **BlobService**, 、し、モバイル アプリなどの信頼されていないか部分的に信頼されたアプリケーションに提供します。 共有アクセス署名は、ポリシーを使用して作成されます。共有アクセス署名には、有効期間の開始日と終了日、共有アクセス署名フォルダーに付与されているアクセス レベルが記述されています。

次のコード例で読み取り操作を実行する共有アクセス署名の所有者を許可する新しい共有アクセス ポリシーの生成、 **myblob** 、blob が作成後 100 分が満了するとします。

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
        Start: startDate,
        Expiry: expiryDate
      },
    };

    var blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', 'myblob', sharedAccessPolicy);
    var host = blobSvc.host;

共有アクセス署名の保有者がコンテナーにアクセスするときに必要なホスト情報も提供する必要があることに注意してください。

クライアント アプリケーションは、共有アクセス署名を使用して **BlobServiceWithSAS** blob に対する操作を実行します。 次情報を取得する **myblob**します。

    var sharedBlobSvc = azure.createBlobServiceWithSas(host, blobSAS);
    sharedBlobSvc.getBlobProperties('mycontainer', 'myblob', function (error, result, response) {
      if(!error) {
        // retrieved info
      }
    });

共有アクセス署名は読み取り専用アクセスで生成されるので、BLOB を変更しようとすると、エラーが返されます。

### アクセス制御リスト

SAS のアクセス ポリシーを設定するために、アクセス制御リスト (ACL) も使用できます。 複数のクライアントにコンテナーへのアクセスを許可し、各クライアントに異なるアクセス ポリシーを提供する場合に便利です。

ACL は、アクセス ポリシーの配列と、各ポリシーに関連付けられた ID を使用して実装されます。 次のコード例では、2 つのポリシーを定義しています。1 つは "user1" 用、もう 1 つは "user2" 用です。

    var sharedAccessPolicy = [
      {
        AccessPolicy: {
          Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
          Start: startDate,
          Expiry: expiryDate
        },
        Id: 'user1'
      },
      {
        AccessPolicy: {
          Permissions: azure.BlobUtilities.SharedAccessPermissions.WRITE,
          Start: startDate,
          Expiry: expiryDate
        },
        Id: 'user2'
      }
    ];

次のコード例の現在の ACL を取得する **mycontainer**, を使用して新しいポリシーを追加および **setBlobAcl**します。 この手法で以下を実行できます。

    blobSvc.getBlobAcl('mycontainer', function(error, result, response) {
      if(!error){
        //push the new policy into signedIdentifiers
        result.signedIdentifiers.push(sharedAccessPolicy);
        blobSvc.setBlobAcl('mycontainer', result, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

ACL を設定したら、ポリシーの ID に基づいて共有アクセス署名を作成できます。 次のコード例では、'user2' の新しい共有アクセス署名を作成します。

    blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', { Id: 'user2' });

## 次のステップ

詳細については、次のリソースを参照してください。

-   [Azure Storage SDK for Node の API リファレンス][]
-   [Azure のストレージ チーム ブログ][]
-   [Azure Storage SDK for Node][] GitHub のリポジトリ
-   [Node.js デベロッパー センター](/develop/nodejs/)
-   [AzCopy コマンド ライン ユーティリティを使ったデータの転送](storage-use-azcopy)

[Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node
[Create and deploy a Node.js application to an Azure Web Site]: /develop/nodejs/tutorials/create-a-website-(mac)/
[Node.js Cloud Service with Storage]: ../storage-nodejs-use-table-storage-cloud-service-app.md
[Node.js Web Application with Storage]: ../storage-nodejs-use-table-storage-web-site.md
[Web app with WebMatrix]: ../web-sites-nodejs-use-webmatrix.md
[Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
[Azure Portal]: portal.azure.com
[Node.js Cloud Service]: ../cloud-services-nodejs-develop-deploy-app.md
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure Storage SDK for Node API Reference]: http://dl.windowsazure.com/nodestoragedocs/index.html


