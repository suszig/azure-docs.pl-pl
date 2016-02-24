<properties 
    pageTitle="Service Bus キューの使用方法 (PHP) | Microsoft Azure" 
    description="Azure での Service Bus キューの使用方法を学習します。 コード サンプルは PHP で記述されています。" 
    services="service-bus" 
    documentationCenter="php" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="10/14/2015" 
    ms.author="sethm"/>

# Service Bus キューの使用方法

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

このガイドでは、Service Bus キューの使用方法について説明します。 サンプルは PHP を使用で記述され、 [Azure SDK for PHP](../php-download-sdk.md)します。 紹介するシナリオ **キューの作成**, 、**メッセージを送受信する**, 、および **キューの削除**します。

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## PHP アプリケーションの作成

唯一の要件の Azure Blob サービスにアクセスする PHP アプリケーションの作成は内のクラスを参照すること、 [Azure SDK for PHP](../php-download-sdk.md) からコード内で。 アプリケーションの作成には任意の開発ツールまたはメモ帳を使用できます。

> [AZURE.NOTE] PHP のインストールが必要、 [OpenSSL 拡張機能を](http://php.net/openssl) インストールして有効にします。

このガイドで使用するサービス機能は、PHP アプリケーション内からローカルで呼び出すことも、Azure の Web ロール、worker ロール、または Web サイト上で実行されるコード内で呼び出すこともできます。

## Azure クライアント ライブラリの入手

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## Service Bus を使用するようにアプリケーションを構成する

Azure Service Bus キュー API を使用するには、次の操作を行います。

1. 使用してオートローダー ファイルを参照、 [require_once][require_once] ステートメントです。
2. 使用する可能性のあるクラスを参照する

次の例では、オートローダー ファイルと参照を含める方法、 **ServicesBuilder** クラスです。

> [AZURE.NOTE] この例 (および、この記事では、その他の例) は、Composer を使用して Azure 向け PHP クライアント ライブラリをインストールしたと仮定します。 参照する必要がありますライブラリを手動でまたは PEAR パッケージとしてインストールした場合、 **WindowsAzure.php** オートローダー ファイルです。

    require_once 'vendor\autoload.php';
    use WindowsAzure\Common\ServicesBuilder;

下のすべてのサンプルに `require_once` ステートメントが入っていますが、サンプルの実行に必要なクラスのみが参照されます。

## Azure Service Bus 接続の設定

Service Bus クライアントをインスタンス化するには、まず次の形式の有効な接続文字列が必要です。

    Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]

ここで **エンドポイント** 形式の一般的な `https://[yourNamespace].servicebus.windows.net`です。

使用する必要がありますいずれの Azure サービス クライアントを作成する、 **ServicesBuilder** クラスです。 そのための方法は次のとおりです。

* 接続文字列を直接渡す
* 使用して、 **CloudConfigurationManager (CCM)** を接続文字列の複数の外部ソースを確認します。
    * 既定では 1 つの外部ソース (環境変数) のみがサポートされています。
    * 新しいソースを追加するには拡張することによって、 **ConnectionStringSource** クラス

ここで概説している例では、接続文字列を直接渡します。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;

    $connectionString = "Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]";

    $serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);


## 方法: キューを作成する

使用して Service Bus キューに対する管理操作を行うことができます、 **ServiceBusRestProxy** クラスです。 A **ServiceBusRestProxy** オブジェクト作成するには、 **:createservicebusservice** ファクトリ メソッドにそれを管理するトークン アクセス許可をカプセル化する適切な接続文字列。

次の例では、インスタンス化する方法、 **ServiceBusRestProxy** を呼び出すと **ServiceBusRestProxy createQueue]-> [** という名前のキューを作成する `myqueue` 内で、 `MySBNamespace` サービス名前空間。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;
    use WindowsAzure\ServiceBus\Models\QueueInfo;

    // Create Service Bus REST proxy.
        $serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
    try {
        $queueInfo = new QueueInfo("myqueue");
        
        // Create queue.
        $serviceBusRestProxy->createQueue($queueInfo);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here: 
        // http://msdn.microsoft.com/library/windowsazure/dd179357
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

> [AZURE.NOTE] 使用することができます、 `listQueues` メソッドを `ServiceBusRestProxy` と指定した名前のキューがサービス名前空間に既に存在をチェックするオブジェクト。

## 方法: キューにメッセージを送信する

Service Bus のキューでは、アプリケーション呼び出しにメッセージを送信する、 **ServiceBusRestProxy sendQueueMessage]-> [** メソッドです。 次のコードは、メッセージを送信する方法を示しています、 `myqueue` 内で作成したキュー、
`MySBNamespace` サービス名前空間。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;
    use WindowsAzure\ServiceBus\Models\BrokeredMessage;

    // Create Service Bus REST proxy.
    $serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
    try {
        // Create message.
        $message = new BrokeredMessage();
        $message->setBody("my message");
    
        // Send message.
        $serviceBusRestProxy->sendQueueMessage("myqueue", $message);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here: 
        // http://msdn.microsoft.com/library/windowsazure/hh780775
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

メッセージ キューは、のインスタンスに送信された (およびから受信された) の Service Bus メッセージ、 **BrokeredMessage** クラスです。 **BrokeredMessage** オブジェクトがある一連の標準的なメソッド (など **getLabel**, 、**getTimeToLive**, 、**setLabel**, 、および **setTimeToLive**) とカスタムのアプリケーションに固有のプロパティおよび任意のアプリケーション データの本体を保持するために使用されるプロパティです。

Service Bus キューでは、最大 256 KB までのメッセージをサポートしています (標準とカスタムのアプリケーション プロパティが含まれるヘッダーの最大サイズは 64 KB です)。 キューで保持されるメッセージ数には上限がありませんが、キュー 1 つあたりが保持できるメッセージの合計サイズには上限があります。 このキュー サイズの上限は 5 GB です。

## キューからメッセージを受信する方法

キューからメッセージを受信する最良の方法を使用して、 **ServiceBusRestProxy receiveQueueMessage]-> [** メソッドです。 2 つの異なるモードでメッセージを受信できます。 **ReceiveAndDelete** (既定値) と **PeekLock**します。

使用する場合、 **ReceiveAndDelete** モードでは、受信は単発の操作には、Service Bus はキュー内のメッセージに対する読み取り要求を受け取る、メッセージを読み取り中としてマークし、アプリケーションに返します。 **ReceiveAndDelete** モードは最もシンプルなモデルであり、障害発生時のメッセージを処理しない、アプリケーションが許容できるシナリオに最適です。 このことを理解するために、コンシューマーが受信要求を発行した後で、メッセージを処理する前にクラッシュしたというシナリオを考えてみましょう。 Service Bus はメッセージを読み取り済みとしてマークするため、アプリケーションが再起動してメッセージの読み取りを再開すると、クラッシュ前に読み取られていたメッセージは見落とされることになります。

 **PeekLock** モードでは、メッセージを受信するメッセージが失われることが許容できないアプリケーションをサポートすることが 2 段階の操作になります。 Service Bus は要求を受け取ると、次に読み取られるメッセージを検索して、他のコンシューマーが受信できないようロックしてから、アプリケーションにメッセージを返します。 受信したメッセージを渡すことによって、受信処理の第 2 段階を完了後、アプリケーションがメッセージの処理 (または後で処理するために確実に保存) **ServiceBusRestProxy deleteMessage]-> [**します。 Service Bus が確認、 **deleteMessage** 呼び出し、メッセージが読み取り中としてマークされ、キューから削除します。

次の例は、メッセージの受信方法を使用および処理 **PeekLock** モード (既定モードではありません)。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;
    use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

    // Create Service Bus REST proxy.
    $serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
    try {
        // Set the receive mode to PeekLock (default is ReceiveAndDelete).
        $options = new ReceiveMessageOptions();
        $options->setPeekLock();
        
        // Receive message.
        $message = $serviceBusRestProxy->receiveQueueMessage("myqueue", $options);
        echo "Body: ".$message->getBody()."<br />";
        echo "MessageID: ".$message->getMessageId()."<br />";
        
        /*---------------------------
            Process message here.
        ----------------------------*/
        
        // Delete message. Not necessary if peek lock is not set.
        echo "Message deleted.<br />";
        $serviceBusRestProxy->deleteMessage($message);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/windowsazure/hh780735
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## 方法: アプリケーションのクラッシュと読み取り不能のメッセージを処理する

Service Bus には、アプリケーションにエラーが発生した場合や、メッセージの処理に問題がある場合に復旧を支援する機能が備わっています。 受信側アプリケーションが何らかの理由でのメッセージを処理できないかどうかは、呼び出すことができます、 **unlockMessage** 受信したメッセージに対してメソッド (の代わりに、 **deleteMessage** メソッド)。 このメソッドが呼び出されると、Service Bus によってキュー内のメッセージのロックが解除され、メッセージが再度受信できる状態に変わります。メッセージを受信するアプリケーションは、以前と同じものでも、別のものでもかまいません。

キュー内でロックされているメッセージには、タイムアウトも設定されています。アプリケーションがクラッシュした場合など、ロックがタイムアウトになる前にアプリケーションがメッセージの処理に失敗した場合は、Service Bus によってメッセージのロックが自動的に解除され、再度受信できる状態に変わります。

前に、メッセージを処理した後、アプリケーションがクラッシュすること、 **deleteMessage** 要求を発行し、再起動する際にそのメッセージがアプリケーションに再配信されます。 一般的に、 **1 回の処理には、少なくとも**。 つまり、すべてのメッセージが 1 回以上処理がは特定の状況で、同じメッセージが再配信されます。 重複処理が許されないシナリオの場合、重複メッセージの配信を扱うロジックをアプリケーションに追加することをお勧めします。 これは、多くの場合を使用して、 **getMessageId** 配信が試行されると、メッセージのメソッドです。

## 次のステップ

これをサービス バス キューの基本を学習できました。、を参照してください。 [キュー、トピック、およびサブスクリプション][] の詳細。

詳細についても参照して、 [PHP デベロッパー センター](/develop/php/)します。

[Queues, Topics, and Subscriptions]: service-bus-queues-topics-subscriptions.md
[require_once]: http://php.net/require_once

 

