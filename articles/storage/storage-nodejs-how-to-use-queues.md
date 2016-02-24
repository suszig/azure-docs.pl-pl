<properties 
    pageTitle="Node.js から Queue ストレージを使用する方法 | Microsoft Azure" 
    description="Azure Queue サービスを使用して、キューの作成と削除のほか、メッセージの挿入、取得、および削除を行う方法を説明します。 サンプルは Node.js で記述されています。" 
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


# Node.js から Queue ストレージを使用する方法

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

## 概要

このガイドは、Microsoft を使用して一般的なシナリオを実行する方法を示します
一般的なシナリオを実行する方法について説明します。 サンプルは Node.js
API を使用して記述されています。 紹介するシナリオ **挿入**, 、**ピーク**,、
**取得する**, 、および **を削除する** キュー メッセージだけでなく **を作成し、
キューの削除**します。

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## Node.js アプリケーションの作成

空の Node.js アプリケーションを作成します。 Node.js アプリケーションを作成する手順については、次を参照してください。 [Create and deploy a Node.js application to an Azure Web Site], 、[Node.js クラウド サービス][Node.js Cloud Service] (Windows PowerShell を使用)、または [Web Site with WebMatrix]します。

## アプリケーションのストレージへのアクセスの構成

Azure Storage を使用するには、Azure Storage SDK for Node.js が必要です。ここには、ストレージ REST サービスと通信するための
便利なライブラリのセットが含まれています。

### ノード パッケージ マネージャー (NPM) を使用してパッケージを取得する

1.  コマンド ライン インターフェイスを使用して **PowerShell** (Windows)、 **ターミナル** (Mac)、または **Bash** (Unix)、サンプル アプリケーションを作成したフォルダーに移動します。

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

3.  手動で実行することができます、 **%.*ls** いることを確認するにはコマンド、
    **node \_modules** フォルダーが作成されました。 フォルダー内で
    検索、 **azure ストレージ** に必要なライブラリを含むパッケージ
    含まれています。

### パッケージをインポートする

メモ帳などのテキスト エディターを使用して、ストレージを使用するアプリケーションの
**server.js** 記憶域を使用するアプリケーションのファイル。

    var azure = require('azure-storage');

## Azure のストレージ接続文字列の設定

azure モジュールは、Azure のストレージ アカウントに接続するために必要な情報として、環境変数 AZURE\_STORAGE\_ACCOUNT および AZURE\_STORAGE\_ACCESS\_KEY、または AZURE\_STORAGE\_CONNECTION\_STRING を読み取ります。 これらの環境変数が設定されていない場合を呼び出すときにアカウント情報を指定する必要があります **createQueueService**します。

環境変数を設定の例については、 [Azure ポータル](portal.azure.com) Azure の web サイトを参照してください。 [Node.js Web Application with Storage]

## 方法: キューを作成する

次のコード作成、 **QueueService** を使用すると、オブジェクト
これによってキューを操作できるようにします。

    var queueSvc = azure.createQueueService();

使用して、 **createQueueIfNotExists** を返す、指定されたメソッド
既に存在する場合はキューを返し、まだ存在しない場合は指定された
名前の新しいキューを作成します。

    queueSvc.createQueueIfNotExists('myqueue', function(error, result, response){
      if(!error){
        // Queue created or exists
      }
    });

キューが作成されると、`result` は true になります。 キューが存在する場合は、`result` は false です。

### フィルター

オプションのフィルター操作を使用して行われる操作に適用できます **QueueService**します。 フィルター操作には、ログや自動的な再試行などが含まれる場合があります。フィルターは、次のシグネチャを持つメソッドを実装するオブジェクトです。

        function handle (requestOptions, next)

要求オプションに対するプリプロセスを行った後で、このメソッドは "next" を呼び出して、次のシグネチャのコールバックを渡す必要があります。

        function (returnObject, finalCallback, next)

このコールバックで、returnObject (サーバーへの要求からの応答) の処理の後に、コールバックは next を呼び出すか (他のフィルターの処理を続けるために next が存在する場合)、単に finalCallback を呼び出す必要があります (サービス呼び出しを終了する場合)。

再試行ロジックを実装する 2 つのフィルターが Azure SDK for Node.js に含まれて **ExponentialRetryPolicyFilter** と **LinearRetryPolicyFilter**します。 次の作成、 **QueueService** を使用するオブジェクト、 **ExponentialRetryPolicyFilter**:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var queueSvc = azure.createQueueService().withFilter(retryOperations);

## 方法: メッセージをキューに挿入する

キューにメッセージを挿入するには、使用、 **createMessage** メソッド
新しいメッセージを作成し、キューに追加します。

    queueSvc.createMessage('myqueue', "Hello world!", function(error, result, response){
      if(!error){
        // Message inserted
      }
    });

## 方法: 次のメッセージをピークする

キューの先頭にあるメッセージをキューから削除せずにピークするには、
呼び出してキューから、 **peekMessages** メソッドです。 既定では、
**peekMessages** 1 つのメッセージをピークします。

    queueSvc.peekMessages('myqueue', function(error, result, response){
      if(!error){
        // Message text is in messages[0].messagetext
      }
    });

`result` にはメッセージが含まれます。

> [AZURE.NOTE] 使用して **peekMessages** キューにメッセージがない場合にエラーが返されない、ただし、メッセージは返されません。

## 方法: 次のメッセージをデキューする

メッセージは、次の 2 段階のプロセスで処理されます。

1. メッセージをデキューする。

2. メッセージを削除する。

メッセージをキューから削除するには使用 **getMessages**します。 これによりメッセージはキューで参照できなくなるため、他のクライアントがこれを処理することもできません。 アプリケーションがメッセージを処理すると、呼び出す **deleteMessage** をキューから削除します。 次に、メッセージを取得してから削除する例を示します。

    queueSvc.getMessages('myqueue', function(error, result, response){
      if(!error){
        // Message text is in messages[0].messagetext
        var message = result[0];
        queueSvc.deleteMessage('myqueue', message.messageid, message.popreceipt, function(error, response){
          if(!error){
            //message deleted
          }
        });
      }
    });

> [AZURE.NOTE] 既定では、30 秒間、それ以降後は他のクライアントに表示されるメッセージはしか表示されません。 使用して別の値を指定する `options.visibilityTimeout` と **getMessages**します。

> [AZURE.NOTE]
> キューにメッセージがないときに **getMessages** を使用した場合、エラーは返されませんが、メッセージも返されません。

## 方法: キューに配置されたメッセージの内容を変更する

メッセージ内の場所のキューを使用して、内容を変更した **updateMessage**します。 次に、メッセージのテキストを更新する例を示します。

    queueSvc.getMessages('myqueue', function(error, result, response){
      if(!error){
        // Got the message
        var message = result[0];
        queueSvc.updateMessage('myqueue', message.messageid, message.popreceipt, 10, {messageText: 'new text'}, function(error, result, response){
          if(!error){
            // Message updated successfully
          }
        });
      }
    });

## 方法: メッセージをデキューするための追加オプション

キューからのメッセージの取得をカスタマイズする方法は 2 つあります。

* `options.numOfMessages` - メッセージをバッチで取得する (最大 32 個)。
* `options.visibilityTimeout` - 非表示タイムアウトを長くするか、短くする。

次の例では、 **getMessages** 1 回の呼び出しで 15 個のメッセージを取得します。 その後、for ループを使用して、
各メッセージを処理します。 また、このメソッドで返されるすべてのメッセージの非表示タイムアウトを 5 分に設定します。

    queueSvc.getMessages('myqueue', {numOfMessages: 15, visibilityTimeout: 5 * 60}, function(error, result, response){
      if(!error){
        // Messages retreived
        for(var index in result){
          // text is available in result[index].messageText
          var message = result[index];
          queueSvc.deleteMessage(queueName, message.messageid, message.popreceipt, function(error, response){
            if(!error){
              // Message deleted
            }
          });
        }
      }
    });

## 方法: キューの長さを取得する

 **GetQueueMetadata** キューで待機しているメッセージの概数をなど、キューに関するメタデータを返します。

    queueSvc.getQueueMetadata('myqueue', function(error, result, response){
      if(!error){
        // Queue length is available in result.approximatemessagecount
      }
    });

## 方法: キューを一覧表示する

キューの一覧を取得するには使用 **listQueuesSegmented**します。 特定のプレフィックスでフィルター処理した一覧を取得するには使用 **listQueuesSegmentedWithPrefix**します。

    queueSvc.listQueuesSegmented(null, function(error, result, response){
      if(!error){
        // result.entries contains the list of queues
      }
    });

すべてのキューを返すできない場合 `result.continuationToken` の最初のパラメーターとして使用できます **listQueuesSegmented** の 2 番目のパラメーターまたは **listQueuesSegmentedWithPrefix** をさらに結果を取得します。

## 方法: キューを削除する

キューおよびキューに含まれているすべてのメッセージを削除するには、
**deleteQueue** 、キュー オブジェクトに対してメソッドです。

    queueSvc.deleteQueue(queueName, function(error, response){
        if(!error){
            // Queue has been deleted
        }
    });

キューからのすべてのメッセージを削除せずにクリアするには使用 **clearMessages**します。

## 方法: 共有アクセス署名を操作する

共有アクセス署名 (SAS) は、ストレージ アカウントの名前またはキーを指定せずにキューへの細密なアクセスを提供する安全な方法です。 SAS は、モバイル アプリによるメッセージの送信を許可する場合など、キューへの制限されたアクセスを提供する場合によく使用されます。

クラウド ベースのサービスなど信頼されたアプリケーションを使用して SAS を生成、 **generateSharedAccessSignature** の **QueueService**, 、し、信頼されていないか、部分的に信頼されたアプリケーションに提供します。 たとえば、モバイル アプリなどです。 SAS は、SAS が有効である期間の開始日と終了日のほか、SAS の保有者に付与されたアクセス レベルを示したポリシーを使用して生成されます。

次の例では、SAS の保有者によるキューへのメッセージの追加を許可する新しい共有アクセス ポリシーを生成します。このポリシーは作成後 100 分が経過すると期限切れになります。

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);
    
    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

    var queueSAS = queueSvc.generateSharedAccessSignature('myqueue', sharedAccessPolicy);
    var host = queueSvc.host;

SAS の保有者がキューにアクセスするときに必要なホスト情報も提供する必要があることに注意してください。

クライアント アプリケーションは、この SAS とを使用して **QueueServiceWithSAS** キューに対する操作を実行します。 次に、キューに接続してメッセージを作成する例を示します。

    var sharedQueueService = azure.createQueueServiceWithSas(host, queueSAS);
    sharedQueueService.createMessage('myqueue', 'Hello world from SAS!', function(error, result, response){
      if(!error){
        //message added
      }
    });

生成された SAS が持つ権限は追加アクセスであるため、メッセージの読み取り、更新、または削除を試行した場合は、エラーが返されます。

### アクセス制御リスト

SAS のアクセス ポリシーを設定するために、アクセス制御リスト (ACL) も使用できます。 複数のクライアントにキューへのアクセスを許可し、各クライアントに異なるアクセス ポリシーを提供する場合に便利です。

ACL は、アクセス ポリシーの配列と、各ポリシーに関連付けられた ID を使用して実装されます。 次の例では、2 つのポリシー"user1"と"user2"用のいずれかです。

    var sharedAccessPolicy = [
      {
        AccessPolicy: {
          Permissions: azure.QueueUtilities.SharedAccessPermissions.PROCESS,
          Start: startDate,
          Expiry: expiryDate
        },
        Id: 'user1'
      },
      {
        AccessPolicy: {
          Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
          Start: startDate,
          Expiry: expiryDate
        },
        Id: 'user2'
      }
    ];

次の例の現在の ACL を取得する **myqueue**, を使用して新しいポリシーを追加 **setQueueAcl**します。 この手法で以下を実行できます。

    queueSvc.getQueueAcl('myqueue', function(error, result, response) {
      if(!error){
        //push the new policy into signedIdentifiers
        result.signedIdentifiers.push(sharedAccessPolicy);
        queueSvc.setQueueAcl('myqueue', result, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

ACL を設定した後で、ポリシーの ID に基づいて SAS を作成できます。 次の例では、"user2" 用に新しい SAS を作成しています。

    queueSAS = queueSvc.generateSharedAccessSignature('myqueue', { Id: 'user2' });

## 次のステップ

これで、キュー ストレージの基本を学習できました。
さらに複雑なストレージ タスクについて学習するには、次のリンク先を参照してください。

-   参照してください、 [Azure ストレージ チーム ブログ][]します。
-   参照してください、 [Azure Storage SDK for Node][] GitHub のリポジトリです。

  [Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node
  [using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
  [Azure Portal]: portal.azure.com
  [Create and deploy a Node.js application to an Azure Web Site]: ../web-sites-nodejs-develop-deploy-mac.md
  [Node.js Cloud Service with Storage]: ../storage-nodejs-use-table-storage-cloud-service-app.md
  [Node.js Web Application with Storage]: ../storage-nodejs-use-table-storage-web-site.md

  
  [Queue1]: ./media/storage-nodejs-how-to-use-queues/queue1.png
  [plus-new]: ./media/storage-nodejs-how-to-use-queues/plus-new.png
  [quick-create-storage]: ./media/storage-nodejs-how-to-use-queues/quick-storage.png
  
  
  
  [Node.js Cloud Service]: ../cloud-services-nodejs-develop-deploy-app.md
  [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Web Site with WebMatrix]: ../web-sites-nodejs-use-webmatrix.md
 
