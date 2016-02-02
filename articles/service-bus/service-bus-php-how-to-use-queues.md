<properties 
    pageTitle="Service Bus キューの使用方法 (PHP) | Microsoft Azure" 
    description="Azure での Service Bus キューの使用方法を学習します。コード サンプルは PHP で記述されています。" 
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

このガイドでは、Service Bus キューの使用方法について説明します。 サンプルは PHP を使用で記述され、 [Azure SDK for PHP](../php-download-sdk.md)します。 紹介するシナリオは、**キューの作成**、**メッセージの送受信**、**キューの削除**です。

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## PHP アプリケーションの作成

唯一の要件の Azure Blob サービスにアクセスする PHP アプリケーションの作成は内のクラスを参照すること、 [Azure SDK for PHP](../php-download-sdk.md) からコード内で。 アプリケーションの作成には任意の開発ツールまたはメモ帳を使用できます。
> [AZURE.NOTE] PHP のインストールが必要、 [OpenSSL 拡張機能を](http://php.net/openssl) インストールして有効にします。

このガイドで使用するサービス機能は、PHP アプリケーション内からローカルで呼び出すことも、Azure の Web ロール、worker ロール、または Web サイト上で実行されるコード内で呼び出すこともできます。

## Azure クライアント ライブラリの入手

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## Service Bus を使用するようにアプリケーションを構成する

Azure Service Bus キュー API を使用するには、次の操作を行います。

1. 使用してオートローダー ファイルを参照、 [require_once ][require_once] ステートメントです。
2. 使用する可能性のあるクラスを参照する

次の例では、オートローダー ファイルをインクルードし、**ServicesBuilder** クラスを参照する方法を示しています。
> [AZURE.NOTE] この例 (およびこの記事のその他の例) では、Composer を使用して Azure 向け PHP クライアント ライブラリがインストールされていることを前提としています。 ライブラリを手動でまたは PEAR パッケージとしてインストールした場合は、**WindowsAzure.php** オートローダー ファイルを参照する必要があります。

    require_once 'vendor\autoload.php';
    use WindowsAzure\Common\ServicesBuilder;

以下の例で、 `require_once` ステートメントは常に表示されますが、使用例を実行するために必要なクラスのみ参照されます。

## Azure Service Bus 接続の設定

Service Bus クライアントをインスタンス化するには、まず次の形式の有効な接続文字列が必要です。

    Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]

ここで **エンドポイント** 形式の一般的な `https://[yourNamespace].servicebus.windows.net`します。

いずれの Azure サービス クライアントを作成するにも、**ServicesBuilder** クラスを使用する必要があります。 そのための方法は次のとおりです。

* 接続文字列を直接渡す
* **CloudConfigurationManager (CCM)** を使用して複数の外部ソースに対して接続文字列を確認する
    * 既定では 1 つの外部ソース (環境変数) のみがサポートされています。
    * **ConnectionStringSource** クラスを継承して新しいソースを追加できます。

ここで概説している例では、接続文字列を直接渡します。

    require_once 'vendor\autoload.php';
    
    use WindowsAzure\Common\ServicesBuilder;
    
    $connectionString = "Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]";
    
    $serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

## 方法: キューを作成する

Service Bus キューの管理処理は **ServiceBusRestProxy** クラスを使用して実行できます。 **ServiceBusRestProxy** オブジェクトを作成するには、**ServicesBuilder::createServiceBusService** ファクトリ メソッドに、管理処理用のトークン アクセス許可をカプセル化した適切な接続文字列を渡します。

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

メッセージを Service Bus キューに送信するには、アプリケーションで **ServiceBusRestProxy->sendQueueMessage** メソッドを呼び出します。 次のコードは、メッセージを送信する方法を示しています、 `myqueue` キュー内で作成した、
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

Service Bus キューに送信されたメッセージ (および Service Bus キューから受信したメッセージ) は、**BrokeredMessage** クラスのインスタンスになります。 **BrokeredMessage** オブジェクトには (**getLabel**、**getTimeToLive**、**setLabel**、**setTimeToLive** など) 一連の標準メソッド、カスタム アプリケーション固有プロパティを保有するためのプロパティ、任意のアプリケーション データの本体があります。

Service Bus キューでは、最大 256 KB までのメッセージをサポートしています (標準とカスタムのアプリケーション プロパティが含まれるヘッダーの最大サイズは 64 KB です)。 キューで保持されるメッセージ数には上限がありませんが、キュー 1 つあたりが保持できるメッセージの合計サイズには上限があります。 このキュー サイズの上限は 5 GB です。

## キューからメッセージを受信する方法

キューからメッセージを受信する最良の方法は **ServiceBusRestProxy->receiveQueueMessage** メソッドを使用することです。 メッセージは 2 つの異なるモード (**ReceiveAndDelete** (既定) と **PeekLock**) で受信できます。

**ReceiveAndDelete** モードを使用する場合、受信が 1 回ずつの動作になります。つまり、Service Bus はキュー内のメッセージに対する読み取り要求を受け取ると、メッセージを読み取り中としてマークし、アプリケーションに返します。 **ReceiveAndDelete** モードは最もシンプルなモデルであり、障害発生時にアプリケーション側でメッセージを処理しないことを許容できるシナリオに最適です。 このことを理解するために、コンシューマーが受信要求を発行した後で、メッセージを処理する前にクラッシュしたというシナリオを考えてみましょう。 Service Bus はメッセージを読み取り済みとしてマークするため、アプリケーションが再起動してメッセージの読み取りを再開すると、クラッシュ前に読み取られていたメッセージは見落とされることになります。

**PeekLock** モードでは、メッセージの受信処理が 2 段階の動作になり、メッセージが失われることが許容できないアプリケーションに対応することができます。 サービス バス は要求を受け取ると、次に読み取られるメッセージを検索して、他のコンシューマーが受信できないようロックしてから、アプリケーションにメッセージを返します。 アプリケーションがメッセージの処理を終えた後 (または後で処理するために確実に保存した後)、受信したメッセージを **ServiceBusRestProxy->deleteMessage** に渡して受信処理の第 2 段階を完了します。 サービス バスが **deleteMessage** の呼び出しを確認すると、メッセージが読み取り中としてマークされ、キューから削除されます。

次の例では、**PeekLock** モード (既定ではないモード) を使用したメッセージの受信および処理の方法を示しています。

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

Service Bus には、アプリケーションにエラーが発生した場合や、メッセージの処理に問題がある場合に復旧を支援する機能が備わっています。 受信側のアプリケーションが何らかの理由によってメッセージを処理できない場合には、受信したメッセージについて (**deleteMessage** メソッドの代わりに) **unlockMessage** メソッドを呼び出すことができます。 このメソッドが呼び出されると、Service Bus によってキュー内のメッセージのロックが解除され、メッセージが再度受信できる状態に変わります。メッセージを受信するアプリケーションは、以前と同じものでも、別のものでもかまいません。

キュー内でロックされているメッセージには、タイムアウトも設定されています。アプリケーションがクラッシュした場合など、ロックがタイムアウトになる前にアプリケーションがメッセージの処理に失敗した場合は、Service Bus によってメッセージのロックが自動的に解除され、再度受信できる状態に変わります。

メッセージが処理された後、**deleteMessage** 要求が発行される前にアプリケーションがクラッシュした場合は、アプリケーションが再起動する際にメッセージが再配信されます。 一般的に、この動作は **1 回以上の処理**と呼ばれます。つまり、すべてのメッセージが 1 回以上処理されますが、特定の状況では、同じメッセージが再配信される可能性があります。 重複処理が許されないシナリオの場合、重複メッセージの配信を扱うロジックをアプリケーションに追加することをお勧めします。 通常、この問題はメッセージの **getMessageId** メソッドを使用して対処します。このプロパティは、配信が試行された後も同じ値を保持します。

## 次のステップ

これをサービス バス キューの基本を学習できました。、を参照してください。 [キュー、トピック、およびサブスクリプションの []][] の詳細。

詳細についても参照して、 [PHP デベロッパー センター](/develop/php/)します。


[queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md 
[require_once]: http://php.net/require_once 

