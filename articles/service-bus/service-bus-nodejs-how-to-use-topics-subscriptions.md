<properties 
    pageTitle="Node.js での Service Bus トピックの使用方法 | Microsoft Azure" 
    description="Node.js アプリから Azure の Service Bus トピックとサブスクリプションを使用する方法を学習します。" 
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
    ms.date="10/07/2015" 
    ms.author="sethm"/>


# Service Bus のトピックとサブスクリプションの使用方法

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Service Bus のトピックとサブスクリプションを使用する方法を説明します。
Node.js アプリケーションです。 紹介するシナリオ **を作成します。
トピックとサブスクリプション**, 、**サブスクリプション フィルターの作成**, 、**を送信します。
メッセージ** トピックに **サブスクリプションからメッセージを受信**, と
**トピックとサブスクリプションの削除**します。 詳細についてはに関するトピック
サブスクリプションを参照し、 [次のステップ](#next-steps) セクションです。

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## Node.js アプリケーションの作成

空の Node.js アプリケーションを作成します。 Node.js アプリケーションを作成する方法の詳細については、次を参照してください。 [Create and deploy a Node.js application to an Azure Web Site], 、[Node.js クラウド サービス][Node.js Cloud Service] (Windows PowerShell を使用)、または WebMatrix による Web サイトです。

## Service Bus を使用するようにアプリケーションを構成する

Service Bus を使用するには、Node.js Azure パッケージをダウンロードします。 このパッケージには、一連ライブラリにはが含まれていますが
Service Bus REST サービスと通信するための便利なライブラリのセットが含まれています。

### ノード パッケージ マネージャー (NPM) を使用してパッケージを取得する

1.  コマンド ライン インターフェイスを使用して **PowerShell** (Windows)、 **ターミナル** (Mac)、または **Bash** (Unix)、サンプル アプリケーションを作成したフォルダーに移動します。

2.  型 **npm のインストール azure** コマンド ウィンドウにする必要があります
    次のような出力が生成されます。

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

3.  手動で実行することができます、 **%.*ls** いることを確認するにはコマンド、
    **node \_modules** フォルダーが作成されました。 そのフォルダーで
    **azure** にアクセスするために必要なライブラリを含むパッケージ
    Service Bus トピックにアクセスするために必要なライブラリが含まれています。

### モジュールのインポート

メモ帳などのテキスト エディターを使用して、アプリケーションの
 **server.js** 、アプリケーションのファイル。

```
var azure = require('azure');
```

### Service Bus 接続の設定

Azure モジュールは、Service Bus への接続に必要な情報を環境変数 azure \_servicebus\_namespace と azure \_servicebus\_access\_key を読み取ります。 これらの環境変数が設定されていない場合を呼び出すときにアカウント情報を指定する必要があります **createServiceBusService**します。

Azure クラウド サービスの構成ファイルで環境変数を設定の例は、次を参照してください。 [Node.js クラウド サービス ストレージに関する][]します。

環境変数を設定の例については、 [Azure クラシック ポータル][] Azure の web サイトを参照してください。 [使用する Node.js Web アプリケーション][]します。

## トピックを作成する

 **ServiceBusService** オブジェクトでは、トピックを操作することができます。 この
次のコードを作成、 **ServiceBusService** オブジェクトです。 付近に追加します
一番上、 **server.js** 、azure にインポートするステートメントの後のファイル
。

```
var serviceBusService = azure.createServiceBusService();
```

呼び出して **createTopicIfNotExists** 上、 **ServiceBusService**
オブジェクトを指定されたトピックが返されます (存在する場合、) か、または新しい
指定した名前のトピックが作成されます。 次のコードでは、
**createTopicIfNotExists** という名前のトピックへの接続を作成または
"MyTopic" という名前のトピックを作成、またはトピックに接続します。

```
serviceBusService.createTopicIfNotExists('MyTopic',function(error){
    if(!error){
        // Topic was created or exists
        console.log('topic created or exists.');
    }
});
```

**createServiceBusService** も追加のオプションをサポートします。
メッセージの time to などの既定のトピックの設定を上書きすることを有効にします。
または最大トピック サイズなどを上書きできます。 次の例では、最大トピック サイズを 5 GB に、有効期間を 1 分に設定します。

```
var topicOptions = {
        MaxSizeInMegabytes: '5120',
        DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createTopicIfNotExists('MyTopic', topicOptions, function(error){
    if(!error){
        // topic was created or exists
    }
});
```

### フィルター

オプションのフィルター操作を使用して行われる操作に適用できます **ServiceBusService**します。 フィルター操作には、ログや自動的な再試行などが含まれる場合があります。フィルターは、次のシグネチャを持つメソッドを実装するオブジェクトです。

```
function handle (requestOptions, next)
```

要求オプションに対するプリプロセスを実行した後で、このメソッドは `next` を呼び出して、次のシグネチャのコールバックを渡します。

```
function (returnObject, finalCallback, next)
```

このコールバックで、returnObject (サーバーへの要求からの応答) の処理の後に、コールバックは next を呼び出すか (他のフィルターの処理を続けるために next が存在する場合)、単に finalCallback を呼び出す必要があります (サービス呼び出しを終了する場合)。

再試行ロジックを実装する 2 つのフィルターが Azure SDK for Node.js に含まれて **ExponentialRetryPolicyFilter** と **LinearRetryPolicyFilter**します。 次の作成、 **ServiceBusService** を使用するオブジェクト、 **ExponentialRetryPolicyFilter**:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);

## サブスクリプションを作成する

トピック サブスクリプションもで作成、 **ServiceBusService**
できます。 サブスクリプションを指定し、
配信されたメッセージのセットを制限、サブスクリプションの仮想
。

> [AZURE.NOTE] サブスクリプションは永続的でありされるまで存在し続けます
または、関連付けられているトピックが削除されます。 アプリケーションが
アプリケーションが、サブスクリプションを作成するためのロジックを含む、最初にすべき
使用して、サブスクリプションが既に存在します
**getSubscription** メソッドです。

### 既定の (MatchAll) フィルターを適用したサブスクリプションの作成

 **MatchAll** フィルターはフィルターなしの場合に使用される既定のフィルター
に使用される既定のフィルターです。 ときに、 **MatchAll**
フィルターを使用すると、トピックに発行されたすべてのメッセージが
サブスクリプションの仮想キューに置かれます。 次の例では、
サブスクリプションは、'AllMessages' という名前し、既定値を使用して **MatchAll**
フィルターを使用します。

```
serviceBusService.createSubscription('MyTopic','AllMessages',function(error){
    if(!error){
        // subscription created
    }
});
```

### フィルターを適用したサブスクリプションの作成

送信メッセージのスコープには、フィルターを作成することもできます。
特定のトピック サブスクリプション内に表示されるメッセージに絞り込むフィルターを設定することもできます。

サブスクリプションでサポートされるフィルターのうち、最も柔軟性の高いものが、
**SqlFilter**, 、SQL92 のサブセットを実装します。 SQL フィルターは
トピックに発行されるメッセージのプロパティに対して適用されます。 次に
SQL フィルターで使用できる式の詳細については、
確認、 [SqlFilter.SqlExpression][SqlFilter.SqlExpression] 構文です。

使用して、サブスクリプションにフィルターを追加することができます、 **createRule**
メソッド、 **ServiceBusService** オブジェクトです。 このメソッドを使用すると、
新しいフィルターを既存のサブスクリプションに追加します。

> [AZURE.NOTE] 既定のフィルターが適用されるため自動的にすべてに新しい
サブスクリプションの場合、既定のフィルター最初に削除する必要がありますか
**MatchAll** その他のフィルターを指定することよりも優先されます。 Visual Studio Online アカウントは
使用して既定のルールを削除、 **deleteRule** のメソッド、
**ServiceBusService** オブジェクトです。

次の例は、という名前のサブスクリプションを作成 `HighMessages` で、
**SqlFilter** のみに、カスタム メッセージを選択する **messagenumber** 3 を超えるプロパティ。

```
serviceBusService.createSubscription('MyTopic', 'HighMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'HighMessages', 
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME, 
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber > 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic', 
            'HighMessages', 
            'HighMessageFilter', 
            ruleOptions, 
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

同様に、次の例では
`LowMessages`  **SqlFilter** のみを持つメッセージを選択します。
 **messagenumber** 3 のプロパティが。

```
serviceBusService.createSubscription('MyTopic', 'LowMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'LowMessages', 
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME, 
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber <= 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic', 
            'LowMessages', 
            'LowMessageFilter', 
            ruleOptions, 
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

メッセージを今すぐ送信と `MyTopic`, に必ず配信されます
サブスクライブした受信者、 `AllMessages` トピック サブスクリプションと
サブスクライブされた受信者に対して選択的に配信される、 `HighMessages` と
`LowMessages` トピック サブスクリプションは (メッセージの内容によっては)。

## メッセージをトピックに送信する方法

アプリケーションでサービス バス トピックにメッセージを送信するには、
**sendTopicMessage** のメソッド、 **ServiceBusService** オブジェクトです。
サービス バス トピックに送信されたメッセージは **BrokeredMessage** オブジェクトです。
**BrokeredMessage** オブジェクト (次のように標準的なプロパティ セットがあります。
**ラベルを付ける** と **TimeToLive**)、カスタムの保持に使用するディクショナリ
アプリケーション固有のプロパティ、および文字列データの本体。 この
アプリケーションは、文字列値を渡すことによってメッセージの本文を設定することができます。
 **sendTopicMessage** 必要なすべての標準的なプロパティがあります
既定値に設定します。

次の例では、
"MyTopic" に 5 通のテスト メッセージを送信する方法を示しています。 なお、 **messagenumber** の各プロパティの値
メッセージは (これによって決定されるループの反復処理では異なります。
受信サブスクリプション)。

```
var message = {
    body: '',
    customProperties: {
        messagenumber: 0
    }
}

for (i = 0;i < 5;i++) {
    message.customProperties.messagenumber=i;
    message.body='This is Message #'+i;
    serviceBusService.sendTopicMessage(topic, message, function(error) {
      if (error) {
        console.log(error);
      }
    });
}
```

Service Bus トピックでは、最大 256 MB までのメッセージをサポートしています
(標準とカスタムのアプリケーション プロパティが含まれるヘッダーは、
64 MB が最大サイズです)。 キューで保持されるメッセージ数には上限がありませんが、
1 つのトピックで保持できるメッセージの合計サイズには上限
があります。 このトピックのサイズはトピックの作成時に定義します。
上限は 5 GB です。

## サブスクリプションからメッセージを受信する

サブスクリプションからメッセージを受信するには、
**receiveSubscriptionMessage** メソッドを **ServiceBusService**
できます。 既定では、メッセージがサブスクリプションから削除されます。
は読み取られるただし、読み取って (ピークして) とせず、メッセージのロック
省略可能なパラメーターを設定して、サブスクリプションから削除します。
**isPeekLock** に **true**します。

受信操作の中で行われるメッセージの読み取りと削除の既定の動作は、
最もシンプルなモデルであり、
障害発生時にアプリケーション側でメッセージを処理しないことを許容できるシナリオに最適です。
エラー。 このことを理解するために、
コンシューマーが受信要求を発行した後で、メッセージを処理する前にクラッシュしたというシナリオを考えてみましょう。
これは。 Service Bus はメッセージを読み取り済みとしてマークするため、
アプリケーションが再起動してメッセージの読み取りを再開すると、
クラッシュ前に読み取られていたメッセージは見落とされることになります。

場合、 **isPeekLock** にパラメーターが設定されている **true**, 、受信になります
受信処理が 2 段階の動作になり、
メッセージが失われることが許容できないアプリケーションに対応することができます。 Service Bus は要求を受け取ると、
次に読み取られるメッセージを検索して、
他のコンシューマーが受信できないようロックしてから、アプリケーションにメッセージを返します。
アプリケーションがメッセージの処理を終えた後 (または
後で処理するために確実に保存した後)、
呼び出して受信処理 **deleteMessage** メソッドを提供して、
削除するメッセージをパラメーターとして指定して、受信処理の第 2 段階を完了します。  **DeleteMessage** メソッドは
メッセージが読み取り中としてマークし、サブスクリプションから削除します。

次の例では、メッセージを受信する方法について説明し、
使用して処理 **receiveSubscriptionMessage**します。 例では、最初
受信して 'LowMessages' サブスクリプションからメッセージを削除し、
使用して、"HighMessages"サブスクリプションからメッセージを受信します。
**isPeekLock** true に設定します。 次に、
**deleteMessage**:

    serviceBusService.receiveSubscriptionMessage('MyTopic', 'LowMessages', function(error, receivedMessage){
        if(!error){
            // Message received and deleted
            console.log(receivedMessage);
        }
    });
    serviceBusService.receiveSubscriptionMessage('MyTopic', 'HighMessages', { isPeekLock: true }, function(error, lockedMessage){
        if(!error){
            // Message received and locked
            console.log(lockedMessage);
            serviceBusService.deleteMessage(lockedMessage, function (deleteError){
                if(!deleteError){
                    // Message deleted
                    console.log('message has been deleted.');
                }
            }
        }
    });

## アプリケーションのクラッシュと読み取り不能のメッセージを処理する方法

Service Bus には、
アプリケーションにエラーが発生した場合や、メッセージの処理に問題がある場合に復旧を支援する機能が備わっています。 場合は、
受信側のアプリケーションがなんらかの理由によってメッセージを処理できない場合には、
呼び出すことができます、 **unlockMessage** メソッドを
**ServiceBusService** オブジェクトです。 このメソッドが呼び出されると、Service Bus によってキュー内のメッセージのロックが解除され、
サブスクリプション内でメッセージを受信できる状態になります
ここでも、いずれか、同じコンシューマー側アプリケーションまたは他のコンシューマー側
想定しています。

キュー内でロックされているメッセージにはタイムアウトも設定されています。
サブスクリプションの場合、アプリケーションにする前にメッセージの処理に失敗した場合
ロックがタイムアウトになる (アプリケーションがクラッシュした場合など、し、
Service Bus はメッセージを自動的に解除され、使用できるように
。

メッセージが処理された後、
前に、 **deleteMessage** メソッドが呼び出されると、メッセージは
アプリケーションが再起動する際にメッセージが再配信されます。 一般的に、
**1 回以上処理**, でのすべてのメッセージの処理は、
特定の状況では、同じメッセージが再配信される可能性があります。
再配信されます。 重複処理が許されないシナリオの場合、
重複メッセージの配信を扱うロジックを
アプリケーションに追加する必要があります。 通常、この問題はメッセージの
**MessageId** 間で一定に保たれますメッセージのプロパティ
。

## トピックとサブスクリプションを削除する

トピックおよびサブスクリプションは永続的であり、
使用するか、 [Azure クラシック ポータル][] またはプログラムを使用しています。
次の例では、`MyTopic` という名前のトピックを削除する方法を示します。

    serviceBusService.deleteTopic('MyTopic', function (error) {
        if (error) {
            console.log(error);
        }
    });

トピックを削除すると、そのトピックに登録されたサブスクリプションもすべて
削除されます。 サブスクリプションは、個別に削除することもできます。 この
次の例は、という名前のサブスクリプションを削除する方法を示しています。
`HighMessages`  `MyTopic` トピック。

    serviceBusService.deleteSubscription('MyTopic', 'HighMessages', function (error) {
        if(error) {
            console.log(error);
        }
    });

## 次のステップ

これで、Service Bus トピックの基本を学習できました。さらに詳細な情報が必要な場合は、次のリンク先を参照してください。

-   参照してください [キュー、トピック、およびサブスクリプション][]します。
-   API リファレンス [SqlFilter][]します。
-   参照してください、 [Azure SDK for Node][] GitHub のリポジトリです。

  [Azure SDK for Node]: https://github.com/WindowsAzure/azure-sdk-for-node
  [Azure classic portal]: http://manage.windowsazure.com
  [SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx
  [Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Create and deploy a Node.js application to an Azure Web Site]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Node.js Cloud Service with Storage]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Node.js Web Application with Storage]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
 

