<properties 
    pageTitle="Service Bus キューの使用方法 (Node.js) | Microsoft Azure" 
    description="Node.js アプリから Azure の Service Bus キューを使用する方法を学習します。" 
    services="service-bus" 
    documentationCenter="nodejs" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="10/06/2015" 
    ms.author="sethm"/>

# Service Bus キューの使用方法

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

この記事では、Service Bus キューの使用方法について説明します。 サンプルは JavaScript で記述され、Node.js Azure モジュールを利用しています。 紹介するシナリオ **キューの作成**, 、**メッセージを送受信する**, 、および **キューの削除**します。 キューの詳細については、「次のステップ[]」のセクションを参照してください。

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## Node.js アプリケーションの作成

空の Node.js アプリケーションを作成します。 Node.js アプリケーションを作成する方法の手順については、次を参照してください。 [作成 Azure の web サイト Node.js アプリケーションと展開][], 、または [Node.js クラウド サービス][] (Windows PowerShell を使用)。

## Service Bus を使用するようにアプリケーションを構成する

Azure Service Bus を使用するには、Node.js Azure パッケージをダウンロードして使用します。 このパッケージには、Service Bus REST サービスと通信するためのライブラリのセットが含まれています。

### ノード パッケージ マネージャー (NPM) を使用してパッケージを取得する

1. 使用して、 **Windows PowerShell for Node.js** コマンド ウィンドウに移動する、 **c:\\node\\sbqueues\\WebRole1** サンプル アプリケーションを作成したフォルダーです。

2. 型 **npm のインストール azure** コマンド ウィンドウで、次のような出力の生成されます。

    ```
    azure@0.7.5 node_modules\azure
        ├── dateformat@1.0.2-1.2.3
        ├── xmlbuilder@0.4.2
        ├── node-uuid@1.2.0
        ├── mime@1.2.9
        ├── underscore@1.4.4
        ├── validator@1.1.1
        ├── tunnel@0.0.2
        ├── wns@0.5.3
        ├── xml2js@0.2.7 (sax@0.5.2)
        └── request@2.21.0 (json-stringify-safe@4.0.0, forever-agent@0.5.0, aws-sign@0.3.0, tunnel-agent@0.3.0, oauth-sign@0.3.0, qs@0.6.5, cookie-jar@0.3.0, node-uuid@1.4.0, http-signature@0.9.11, form-data@0.0.8, hawk@0.13.1)
    ```

3. 手動で実行することができます、 **%.*ls** いることを確認するにはコマンド、 **node \_modules** フォルダーが作成されました。 フォルダーを検索、 **azure** パッケージで、Service Bus キューにアクセスするために必要なライブラリが含まれています。

### モジュールのインポート

先頭に次のコードを追加メモ帳または別のテキスト エディターを使用して、 **server.js** 、アプリケーションのファイル。

```
var azure = require('azure');
```

### Azure Service Bus 接続の設定

Azure モジュールは、環境変数を Service Bus への接続に必要な情報を取得するには、azure \_servicebus\_namespace と azure \_servicebus\_access\_key を読み取ります。 これらの環境変数が設定されていない場合を呼び出すときにアカウント情報を指定する必要があります **createServiceBusService**します。

Azure クラウド サービスの構成ファイルで環境変数を設定の例は、次を参照してください。 [Node.js クラウド サービス ストレージに関する][]します。

環境変数を設定の例については、 [Azure クラシック ポータル][] Azure の web サイトを参照してください。 [使用する Node.js Web アプリケーション][]します。

## キューを作成する

 **ServiceBusService** オブジェクトでは、Service Bus キューを操作することができます。 次のコード作成、 **ServiceBusService** オブジェクトです。 先頭付近にある追加の **server.js** Azure モジュールをインポートするステートメントの後のファイル。

```
var serviceBusService = azure.createServiceBusService();
```

呼び出して **createQueueIfNotExists** 上、 **ServiceBusService** オブジェクトの場合 (存在する場合は、指定されたキューが返される、または指定した名前で新しいキューを作成します。 次のコードでは **createQueueIfNotExists** という名前のキューへの接続を作成または `myqueue`:

```
serviceBusService.createQueueIfNotExists('myqueue', function(error){
    if(!error){
        // Queue exists
    }
});
```

**createServiceBusService** もメッセージ期間や最大キュー サイズなどの既定キューの設定を上書きすることを有効にする追加のオプションをサポートしています。 次の例では、最大キュー サイズを 5 GB に、有効期間を 1 分に設定します。

```
var queueOptions = {
      MaxSizeInMegabytes: '5120',
      DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createQueueIfNotExists('myqueue', queueOptions, function(error){
    if(!error){
        // Queue exists
    }
});
```

### フィルター

オプションのフィルター操作を使用して行われる操作に適用できます **ServiceBusService**します。 フィルター操作には、ログや自動的な再試行などが含まれる場合があります。フィルターは、次のシグネチャを持つメソッドを実装するオブジェクトです。

```
function handle (requestOptions, next)
```

要求オプションに対するプリプロセスを行った後で、このメソッドは `next` を呼び出して、次のシグネチャのコールバックを渡す必要があります。

```
function (returnObject, finalCallback, next)
```

このコールバックでは、処理後、 **returnObject** (要求からの応答、サーバーに)、コールバックを呼び出す必要がありますか、 `next` して他のフィルターの処理を続行するか、単を呼び出しますが存在する場合 `finalCallback`, 、サービスの呼び出しを終了します。

再試行ロジックを実装する 2 つのフィルターが Azure SDK for Node.js に含まれて **ExponentialRetryPolicyFilter** と **LinearRetryPolicyFilter**します。 次の作成、 **ServiceBusService** を使用するオブジェクト、 **ExponentialRetryPolicyFilter**:

```
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## メッセージをキューに送信する

Service Bus のキューでは、アプリケーション呼び出しにメッセージを送信する、 **sendQueueMessage** メソッドを **ServiceBusService** オブジェクトです。 メッセージ キューに送信された (およびから受信された) の Service Bus メッセージ **BrokeredMessage** オブジェクト、および標準的なプロパティ セットがあります (など **ラベル** と **TimeToLive**) は、カスタム アプリケーションの特定のプロパティの保持に使用するディクショナリ、および任意のアプリケーション データの本体。 アプリケーションはメッセージとして文字列値を渡すことでメッセージの本文を設定できます。 必須の標準プロパティには既定値が入力されます。

次の例では、という名前のキューにテスト メッセージを送信する方法 `myqueue` を使用して **sendQueueMessage**:

```
var message = {
    body: 'Test message',
    customProperties: {
        testproperty: 'TestValue'
    }};
serviceBusService.sendQueueMessage('myqueue', message, function(error){
    if(!error){
        // message sent
    }
});
```

Service Bus キューでは、最大 256 KB までのメッセージをサポートしています (標準とカスタムのアプリケーション プロパティが含まれるヘッダーの最大サイズは 64 KB です)。 キューで保持されるメッセージ数には上限がありませんが、キュー 1 つあたりが保持できるメッセージの合計サイズには上限があります。 このキュー サイズは作成時に定義され、上限は 5 GB です。 クォータの詳細については、次を参照してください。 [Azure キューと Service Bus キュー][]します。

## キューからメッセージを受信する

使用してキューからメッセージを受信、 **receiveQueueMessage** メソッドを **ServiceBusService** オブジェクトです。 読み取るときに、メッセージをキューから削除するよう既定では、ただし、読み取って (ピークして) し、省略可能なパラメーターを設定して、キューから削除することがなく、メッセージをロック **isPeekLock** に **true**します。

受信操作の中で行われるメッセージの読み取りと削除の既定の動作は、最もシンプルなモデルであり、障害発生時にアプリケーション側でメッセージを処理しないことを許容できるシナリオに最適です。 このことを理解するために、コンシューマーが受信要求を発行した後で、メッセージを処理する前にクラッシュしたというシナリオを考えてみましょう。 Service Bus はメッセージを読み取り済みとしてマークするため、アプリケーションが再起動してメッセージの読み取りを再開すると、クラッシュ前に読み取られていたメッセージは見落とされることになります。

場合、 **isPeekLock** にパラメーターが設定されている **true**, 、受信メッセージが失われることが許容できないアプリケーションをサポートすることが 2 段階の操作になります。 Service Bus は要求を受け取ると、次に読み取られるメッセージを検索して、他のコンシューマーが受信できないようロックしてから、アプリケーションにメッセージを返します。 呼び出して受信処理の第 2 段階を完了したら、アプリケーションがメッセージの処理 (または後で処理するために確実に保存) **deleteMessage** メソッドをパラメーターとして削除するメッセージ。  **DeleteMessage** メソッドは、メッセージが読み取り中としてマークされ、キューから削除します。

次の例では、受信しを使用してメッセージを処理する方法 **receiveQueueMessage**します。 例では、最初の受信、メッセージを削除およびを使用してメッセージを受信し、 **isPeekLock** 設定 **true**, を使用してメッセージを削除し、 **deleteMessage**:

```
serviceBusService.receiveQueueMessage('myqueue', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
    }
});
serviceBusService.receiveQueueMessage('myqueue', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
            }
        });
    }
});
```

## アプリケーションのクラッシュと読み取り不能のメッセージを処理する方法

Service Bus には、アプリケーションにエラーが発生した場合や、メッセージの処理に問題がある場合に復旧を支援する機能が備わっています。 受信側アプリケーションが何らかの理由でのメッセージを処理できないかどうかは、呼び出すことができます、 **unlockMessage** メソッドを **ServiceBusService** オブジェクトです。 このメソッドが呼び出されると、Service Bus によってキュー内のメッセージのロックが解除され、
キュー内でメッセージし、同じコンシューマー側アプリケーションまたは別のコンシューマー側アプリケーションのいずれかが再び受信できるようにします。

キュー内でロックされているメッセージにはタイムアウトも設定されています。アプリケーションがクラッシュした場合など、ロックがタイムアウトになる前にアプリケーションがメッセージの処理に失敗した場合には、Service Bus によりメッセージのロックが自動的に解除され、再度受信できる状態に変わります。

前に、メッセージを処理した後、アプリケーションがクラッシュすること、 **deleteMessage** メソッドが呼び出されると、再起動する際にそのメッセージがアプリケーションに再配信されます。 一般的に、 **1 回の処理には、少なくとも**, 、つまり、各メッセージが 1 回以上処理されますが、特定の状況で、同じメッセージが再配信されます。 重複処理が許されないシナリオの場合、重複メッセージの配信を扱うロジックをアプリケーションに追加する必要があります。 これは、多くの場合を使用して、 **MessageId** は一定に保たれる配信が試行されると、メッセージのプロパティです。

## 次のステップ

詳細については、次のリソースを参照してください。

-   [キュー、トピック、およびサブスクリプション][]
-   [Azure SDK for Node][] GitHub のリポジトリ
-   [Node.js デベロッパー センター](/develop/nodejs/)

  [Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
  [Azure classic portal]: http://manage.windowsazure.com
  
  [Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
  [Create and deploy a Node.js application to an Azure Website]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Node.js Cloud Service with Storage]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
  [Node.js Web Application with Storage]: ../storage/storage-nodejs-how-to-use-table-storage.md
  [Azure Queues and Service Bus queues]: service-bus-azure-and-service-bus-queues-compared-contrasted.md#capacity-and-quotas
 

