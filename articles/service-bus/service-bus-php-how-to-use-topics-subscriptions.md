<properties 
    pageTitle="Service Bus トピックの使用方法 (PHP) | Microsoft Azure" 
    description="Azure 上の PHP で Service Bus トピックを使用する方法について説明します。" 
    services="service-bus" 
    documentationCenter="php" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="10/06/2015" 
    ms.author="sethm"/>


# Service Bus のトピックとサブスクリプションの使用方法

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

この記事では、Service Bus のトピックとサブスクリプションの使用方法について説明します。 サンプルは PHP を使用で記述され、 [Azure SDK for PHP](../php-download-sdk.md)します。 紹介するシナリオ **トピックとサブスクリプションの作成**, 、**サブスクリプション フィルターの作成**, 、**トピックにメッセージを送信する**, 、**サブスクリプションからメッセージを受信**, 、および **トピックとサブスクリプションの削除**します。

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## PHP アプリケーションの作成

クラスを参照するのには、Azure Blob サービスにアクセスする PHP アプリケーションの作成の唯一の要件、 [Azure SDK for PHP](../php-download-sdk.md) からコード内で。 アプリケーションの作成には任意の開発ツールまたはメモ帳を使用できます。

> [AZURE.NOTE] PHP のインストールが必要、 [OpenSSL 拡張機能を](http://php.net/openssl) インストールして有効にします。

この記事では、PHP アプリケーション内でローカルで呼び出すことも、Azure の Web ロール、worker ロール、または Web サイト上で実行されるコード内で呼び出すこともできるサービス機能の使用方法について説明します。

## Azure クライアント ライブラリの入手

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## Service Bus を使用するようにアプリケーションを構成する

Service Bus API を使用するには、以下の手順を実行します。

1. 使用してオートローダー ファイルを参照、 [require_once][require-once] ステートメントです。
2. 使用する可能性のあるクラスを参照する

次の例では、オートローダー ファイルと参照を含める方法、 **ServiceBusService** クラスです。

> [AZURE.NOTE] この例 (および、この記事では、その他の例) は、Composer を使用して Azure 向け PHP クライアント ライブラリをインストールしたと仮定します。 参照する必要がありますライブラリを手動でまたは PEAR パッケージとしてインストールした場合、 **WindowsAzure.php** オートローダー ファイルです。

```
require_once 'vendor\autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

下のすべてのサンプルに `require_once` ステートメントが入っていますが、サンプルの実行に必要なクラスのみが参照されます。

## Service Bus 接続の設定

Azure Service Bus クライアントをインスタンス化するには、まず次の形式の有効な接続文字列が必要です。

```
Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]
```

ここで、`Endpoint` の一般的な形式は `https://[yourNamespace].servicebus.windows.net` です。

使用する必要がありますいずれの Azure サービス クライアントを作成する、 **ServicesBuilder** クラスです。 そのための方法は次のとおりです。

* 接続文字列を直接渡す
* 使用して、 **CloudConfigurationManager (CCM)** を接続文字列の複数の外部ソースを確認します。
    * 既定では 1 つの外部ソース (環境変数) のみサポートされています。
    * 新しいソースを追加するには拡張することによって、 **ConnectionStringSource** クラスです。

ここで概説している例では、接続文字列が直接渡されます。

```
require_once 'vendor\autoload.php';

use WindowsAzure\Common\ServicesBuilder;
    
$connectionString = "Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## トピックを作成する

使用して Service Bus トピックの管理操作を行うことができます、 **ServiceBusRestProxy** クラスです。 A **ServiceBusRestProxy** オブジェクト作成するには、 **:createservicebusservice** ファクトリ メソッドにそれを管理するトークン アクセス許可をカプセル化する適切な接続文字列。

次の例では、インスタンス化する方法、 **ServiceBusRestProxy** を呼び出すと **ServiceBusRestProxy createTopic]-> [** という名前のトピックを作成する `mytopic` 内で、 `MySBNamespace` 名前空間。

```
require_once 'vendor\autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\TopicInfo;
    
// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {       
    // Create topic.
    $topicInfo = new TopicInfo("mytopic");
    $serviceBusRestProxy->createTopic($topicInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/windowsazure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

> [AZURE.NOTE] 使用することができます、 `listTopics` メソッドを `ServiceBusRestProxy` で指定した名前のトピックがサービス名前空間に既に存在をチェックするオブジェクト。

## サブスクリプションを作成する

トピック サブスクリプションもで作成、 **ServiceBusRestProxy createSubscription]-> [** メソッドです。 サブスクリプションを指定し、サブスクリプションの仮想キューに渡すメッセージを制限するフィルターを設定することができます。

### 既定の (MatchAll) フィルターを適用したサブスクリプションの作成

 **MatchAll** フィルターは、新しいサブスクリプションが作成されるときにフィルターが指定されていない場合に使用される既定のフィルターです。 ときに、 **MatchAll** フィルターを使用して、トピックに発行されたすべてのメッセージがサブスクリプションの仮想キューに置かれます。 次の例は、'mysubscription' という名前のサブスクリプションを作成し、既定値を使用して **MatchAll** フィルター。

```
require_once 'vendor\autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\SubscriptionInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {
    // Create subscription.
    $subscriptionInfo = new SubscriptionInfo("mysubscription");
    $serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/windowsazure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

### フィルターを適用したサブスクリプションの作成

トピックに送信されたメッセージのうち、特定のトピック サブスクリプション内に表示されるメッセージを指定できるフィルターを設定することもできます。 サブスクリプションでサポートされるフィルターの最も柔軟性の高い型は、 **SqlFilter**, 、SQL92 のサブセットを実装します。 SQL フィルターは、トピックに発行されるメッセージのプロパティに対して適用されます。 Sqlfilter の詳細については、次を参照してください。 [SqlFilter.SqlExpression プロパティ][sqlfilter]します。

> [AZURE.NOTE] サブスクリプションの各ルールいない受信メッセージを処理、独立して結果メッセージ サブスクリプションに追加します。 さらに、新しいサブスクリプションごとには、既定値 **ルール** すべてのメッセージをトピックからサブスクリプションに追加するフィルターを持つオブジェクト。 独自のフィルターに一致するメッセージのみ受信するには、この既定のルールを削除する必要があります。 `ServiceBusRestProxy->deleteRule` メソッドを使用して既定のルールを削除できます。

という名前のサブスクリプションを作成する例を次 **HighMessages** で、 **SqlFilter** のみに、カスタム メッセージを選択する **MessageNumber** 3 を超えるプロパティ (を参照してください [トピックにメッセージを送信](#send-messages-to-a-topic) メッセージにカスタム プロパティを追加する方法について)。

```
$subscriptionInfo = new SubscriptionInfo("HighMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "HighMessages", '$Default');

$ruleInfo = new RuleInfo("HighMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber > 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "HighMessages", $ruleInfo);
```

このコードには追加の名前空間 `WindowsAzure\ServiceBus\Models\SubscriptionInfo` を使用する必要があります。

同様に、次の例がという名前のサブスクリプションを作成 **LowMessages** で、 **SqlFilter** のみを持つメッセージを選択する、 **MessageNumber** 3 のプロパティが。

```
$subscriptionInfo = new SubscriptionInfo("LowMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "LowMessages", '$Default');

$ruleInfo = new RuleInfo("LowMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber <= 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "LowMessages", $ruleInfo);
```

これでメッセージが `mytopic` に送信されると、そのメッセージは `mysubscription` サブスクリプションをサブスクライブした受信者に必ず配信され、さらにメッセージの内容に応じて、`HighMessages` および `LowMessages` トピック サブスクリプションをサブスクライブしている受信者に対して選択的に配信されます。

## メッセージをトピックに送信する

Service Bus のトピックでは、アプリケーション呼び出しにメッセージを送信する、 **ServiceBusRestProxy sendTopicMessage]-> [** メソッドです。 次のコードは、メッセージを送信する方法を示しています、 `mytopic` トピック内で作成した、
`MySBNamespace` サービス名前空間。

```
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
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/windowsazure/hh780775
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Service Bus トピックに送信されたメッセージは、 **BrokeredMessage** クラスです。 **BrokeredMessage** オブジェクトは、標準的なプロパティとメソッドのセットを持つ (など **getLabel**, 、**getTimeToLive**, 、**setLabel**, 、および **setTimeToLive**)、アプリケーション固有のカスタム プロパティを保持するために使用できるプロパティとします。 次の例は前に作成した `mytopic` トピックに 5 つのテスト メッセージを送信する方法を示しています。  **SetProperty** カスタム プロパティを追加するメソッドを使用 (`MessageNumber`) を各メッセージです。 注意してください、 `MessageNumber` プロパティの値がメッセージごとに異なります (この値を使用するにはのように、受信するサブスクリプションを決定する、 [サブスクリプションを作成](#create-a-subscription) セクション)。

```
for($i = 0; $i < 5; $i++){
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message ".$i);
            
    // Set custom property.
    $message->setProperty("MessageNumber", $i);
            
    // Send message.
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
```

Service Bus キューでは、最大 256 KB までのメッセージをサポートしています (標準とカスタムのアプリケーション プロパティが含まれるヘッダーの最大サイズは 64 KB です)。 キューで保持されるメッセージ数には上限がありませんが、キュー 1 つあたりが保持できるメッセージの合計サイズには上限があります。 このキュー サイズの上限は 5 GB です。 クォータの詳細については、次を参照してください。 [Azure キューと Service Bus キュー][]します。

## サブスクリプションからメッセージを受信する

サブスクリプションからメッセージを受信する最良の方法を使用して、 **ServiceBusRestProxy receiveSubscriptionMessage]-> [** メソッドです。 受信したメッセージが 2 つの異なるモードで動作できます: **ReceiveAndDelete** (既定値) と **PeekLock**します。

使用する場合、 **ReceiveAndDelete** モードでは、受信は単発の操作です。 つまり、Service Bus はサブスクリプション内のメッセージに対する読み取り要求を受け取る、メッセージを読み取り中としてマークし、アプリケーションに返します。 **ReceiveAndDelete** モードは最もシンプルなモデルであり、シナリオに最適な
障害発生時にアプリケーション側でメッセージを処理しないことを許容できるシナリオに最適です。 このことを理解するために、コンシューマーが受信要求を発行した後で、メッセージを処理する前にクラッシュしたというシナリオを考えてみましょう。 Service Bus はメッセージを読み取り済みとしてマークするため、アプリケーションが再起動してメッセージの読み取りを再開すると、クラッシュ前に読み取られていたメッセージは見落とされることになります。

 **PeekLock** モードでは、メッセージを受信するメッセージが失われることが許容できないアプリケーションをサポートすることが 2 段階の操作になります。 Service Bus は要求を受け取ると、次に読み取られるメッセージを検索して、他のコンシューマーが受信できないようロックしてから、アプリケーションにメッセージを返します。 受信したメッセージを渡すことによって、受信処理の第 2 段階を完了後、アプリケーションがメッセージの処理 (または後で処理するために確実に保存) **ServiceBusRestProxy deleteMessage]-> [**します。 Service Bus が確認、 **deleteMessage** 呼び出し、メッセージが読み取り中としてマークされ、キューから削除します。

次の例は、受信しを使用してメッセージを処理する方法を示しています。 **PeekLock** モード (既定モードではありません)。 

```
require_once 'vendor\autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Set receive mode to PeekLock (default is ReceiveAndDelete)
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();
    
    // Get message.
    $message = $serviceBusRestProxy->receiveSubscriptionMessage("mytopic", "mysubscription", $options);

    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";
        
    /*---------------------------
        Process message here.
    ----------------------------*/
        
    // Delete message. Not necessary if peek lock is not set.
    echo "Deleting message...<br />";
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
```

## 方法: アプリケーションのクラッシュと読み取り不能のメッセージを処理する

Service Bus には、アプリケーションにエラーが発生した場合や、メッセージの処理に問題がある場合に復旧を支援する機能が備わっています。 受信側アプリケーションが何らかの理由でのメッセージを処理できないかどうかは、呼び出すことができます、 **unlockMessage** 受信したメッセージに対してメソッド (の代わりに、 **deleteMessage** メソッド)。 このメソッドが呼び出されると、Service Bus によってキュー内のメッセージのロックが解除され、メッセージが再度受信できる状態に変わります。メッセージを受信するアプリケーションは、以前と同じものでも、別のものでもかまいません。

キュー内でロックされているメッセージには、タイムアウトも設定されています。アプリケーションがクラッシュした場合など、ロックがタイムアウトになる前にアプリケーションがメッセージの処理に失敗した場合は、Service Bus によってメッセージのロックが自動的に解除され、再度受信できる状態に変わります。

前に、メッセージを処理した後、アプリケーションがクラッシュすること、 **deleteMessage** 要求を発行し、再起動する際にそのメッセージがアプリケーションに再配信されます。 一般的に、 **1 回の処理には、少なくとも**。 つまり、すべてのメッセージが 1 回以上処理がは特定の状況で、同じメッセージが再配信されます。 重複処理が許されないシナリオの場合、重複メッセージの配信を扱うロジックをアプリケーションに追加する必要があります。 これは、多くの場合を使用して、 **getMessageId** 配信が試行されると、メッセージのメソッドです。

## トピックとサブスクリプションを削除する

トピックまたはサブスクリプションを削除する、 **ServiceBusRestProxy deleteTopic]-> [** または **ServiceBusRestProxy deleteSubscripton]-> [** メソッド、それぞれします。 トピックを削除すると、そのトピックに登録されたサブスクリプションもすべて削除されることに注意してください。

次のコードは、`mytopic` という名前のトピックとその登録されたサブスクリプションを削除する方法を示しています。

```
require_once 'vendor\autoload.php';

use WindowsAzure\ServiceBus\ServiceBusService;
use WindowsAzure\ServiceBus\ServiceBusSettings;
use WindowsAzure\Common\ServiceException;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {       
    // Delete topic.
    $serviceBusRestProxy->deleteTopic("mytopic");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/windowsazure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

使用して、 **deleteSubscription** メソッドすることができます、サブスクリプションを個別に削除します。

```
$serviceBusRestProxy->deleteSubscription("mytopic", "mysubscription");
```

## 次のステップ

これをサービス バス キューの基本を学習できました。、を参照してください。 [キュー、トピック、およびサブスクリプション][] の詳細。

[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[sqlfilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[require-once]: http://php.net/require_once
[Azure Queues and Service Bus queues]: service-bus-azure-and-service-bus-queues-compared-contrasted.md#capacity-and-quotas

