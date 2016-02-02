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

この記事では、Service Bus のトピックとサブスクリプションの使用方法について説明します。 サンプルは PHP を使用で記述され、 [Azure SDK for PHP](../php-download-sdk.md)します。 ここでは、**トピックとサブスクリプションの作成**、**サブスクリプション フィルターの作成**、**トピックへのメッセージの送信**、**サブスクリプションからのメッセージの受信**、**トピックとサブスクリプションの削除**などのシナリオについて説明します。

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## PHP アプリケーションの作成

クラスを参照するのには、Azure Blob サービスにアクセスする PHP アプリケーションの作成の唯一の要件、 [Azure SDK for PHP](../php-download-sdk.md) からコード内で。 アプリケーションの作成には任意の開発ツールまたはメモ帳を使用できます。
> [AZURE.NOTE] PHP のインストールが必要、 [OpenSSL 拡張機能を](http://php.net/openssl) インストールして有効にします。

この記事では、PHP アプリケーション内でローカルで呼び出すことも、Azure の Web ロール、worker ロール、または Web サイト上で実行されるコード内で呼び出すこともできるサービス機能の使用方法について説明します。

## Azure クライアント ライブラリの入手

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## Service Bus を使用するようにアプリケーションを構成する

Service Bus API を使用するには、以下の手順を実行します。

1. 使用してオートローダー ファイルを参照、 [require_once ][require-once] ステートメントです。
2. 使用する可能性のあるクラスを参照する

次の例では、オートローダー ファイルをインクルードし、**ServiceBusService** クラスを参照する方法を示しています。
> [AZURE.NOTE] この例 (およびこの記事のその他の例) では、Composer を使用して Azure 向け PHP クライアント ライブラリがインストールされていることを前提としています。 ライブラリを手動でまたは PEAR パッケージとしてインストールした場合は、**WindowsAzure.php** オートローダー ファイルを参照する必要があります。

```
require_once 'vendor\autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

次の例で、 `require_once` ステートメントは常に表示されますが、使用例を実行するために必要なクラスのみ参照されます。

## Service Bus 接続の設定

Azure Service Bus クライアントをインスタンス化するには、まず次の形式の有効な接続文字列が必要です。

```
Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]
```

ここで `エンドポイント` 形式の一般的な `https://[yourNamespace].servicebus.windows.net`します。

いずれの Azure サービス クライアントを作成するにも、**ServicesBuilder** クラスを使用する必要があります。 そのための方法は次のとおりです。

* 接続文字列を直接渡す
* **CloudConfigurationManager (CCM)** を使用して複数の外部ソースに対して接続文字列を確認する
    * 既定では 1 つの外部ソース (環境変数) のみサポートされています。
    * **ConnectionStringSource** クラスを継承して新しいソースを追加できます。

ここで概説している例では、接続文字列が直接渡されます。

```
require_once 'vendor\autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## トピックを作成する

Service Bus トピックの管理処理は **ServiceBusRestProxy** クラスを使用して実行できます。 **ServiceBusRestProxy** オブジェクトを作成するには、**ServicesBuilder::createServiceBusService** ファクトリ メソッドに、管理処理用のトークン アクセス許可をカプセル化した適切な接続文字列を渡します。

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

トピック サブスクリプションを **ServiceBusRestProxy->createSubscription** メソッドで作成することもできます。 サブスクリプションを指定し、サブスクリプションの仮想キューに渡すメッセージを制限するフィルターを設定することができます。

### 既定の (MatchAll) フィルターを適用したサブスクリプションの作成

**MatchAll** フィルターは、新しいサブスクリプションの作成時にフィルターが指定されていない場合に使用される既定のフィルターです。 **MatchAll** フィルターを使用すると、トピックに発行されたすべてのメッセージがサブスクリプションの仮想キューに置かれます。 次の例では、'mysubscription' という名前のサブスクリプションを作成し、既定の **MatchAll** フィルターを使用します。

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

トピックに送信されたメッセージのうち、特定のトピック サブスクリプション内に表示されるメッセージを指定できるフィルターを設定することもできます。 サブスクリプションでサポートされるフィルターのうち、最も柔軟性の高いものが、SQL92 のサブセットを実装する **SqlFilter** です。 SQL フィルターは、トピックに発行されるメッセージのプロパティに対して適用されます。 Sqlfilter の詳細については、次を参照してください。 [SqlFilter.SqlExpression プロパティ ][sqlfilter]します。
> [AZURE.NOTE] サブスクリプションのルールごとに受信メッセージが個別に処理され、処理後のメッセージがサブスクリプションに追加されます。 さらに、新しいサブスクリプションにはそれぞれ、既定の**ルール** オブジェクトがあり、トピックからのすべてのメッセージをサブスクリプションに追加するフィルターが設定されています。 独自のフィルターに一致するメッセージのみ受信するには、この既定のルールを削除する必要があります。 既定のルールを削除するを使用して、 `ServiceBusRestProxy deleteRule]-> [` メソッドです。

という名前のサブスクリプションを作成する例を次 **HighMessages** で、 **SqlFilter** のみに、カスタム メッセージを選択する **MessageNumber** 3 を超えるプロパティ (を参照してください [トピックにメッセージを送信](#send-messages-to-a-topic) メッセージにカスタム プロパティを追加する方法について)。

```
$subscriptionInfo = new SubscriptionInfo("HighMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "HighMessages", '$Default');

$ruleInfo = new RuleInfo("HighMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber > 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "HighMessages", $ruleInfo);
```

このコードには、追加の名前空間の使用が必要があります: `WindowsAzure\ServiceBus\Models\SubscriptionInfo`します。

同様に、次の例では **LowMessages** という名前のサブスクリプションを作成し、**SqlFilter** を適用します。このフィルターでは、**MessageNumber** プロパティが 3 以下のメッセージのみが選択されます。

```
$subscriptionInfo = new SubscriptionInfo("LowMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "LowMessages", '$Default');

$ruleInfo = new RuleInfo("LowMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber <= 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "LowMessages", $ruleInfo);
```

これで、メッセージが送信される場合に、 `mytopic` トピックには、常に配信をサブスクライブした受信者、 `mysubscription` サブスクリプションの場合にサブスクライブされた受信者に対して選択的に配信されると、 `HighMessages` と `LowMessages` サブスクリプション (メッセージの内容によっては)。

## メッセージをトピックに送信する

メッセージを Service Bus トピックに送信するには、アプリケーションで **ServiceBusRestProxy->sendTopicMessage** メソッドを呼び出します。 次のコードは、メッセージを送信する方法を示しています、 `mytopic` トピック内で作成した、
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

Service Bus トピックに送信されたメッセージは、**BrokeredMessage** クラスのインスタンスです。 **BrokeredMessage** オブジェクトには、一連の標準的なプロパティとメソッド (**getLabel**、**getTimeToLive**、**setLabel**、**setTimeToLive** など) だけでなく、アプリケーションに固有のカスタム プロパティの保持に使用できるプロパティも用意されています。 次の例に 5 つのテスト メッセージを送信する方法を示しています、 `mytopic` トピックの前に作成します。  **SetProperty** カスタム プロパティを追加するメソッドを使用 (`MessageNumber`) を各メッセージです。 注意してください、 `MessageNumber` プロパティの値がメッセージごとに異なります (この値を使用するにはのように、受信するサブスクリプションを決定する、 [サブスクリプションを作成](#create-a-subscription) セクション)。

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

Service Bus キューでは、最大 256 KB までのメッセージをサポートしています (標準とカスタムのアプリケーション プロパティが含まれるヘッダーの最大サイズは 64 KB です)。 キューで保持されるメッセージ数には上限がありませんが、キュー 1 つあたりが保持できるメッセージの合計サイズには上限があります。 このキュー サイズの上限は 5 GB です。 クォータの詳細については、次を参照してください。 [Azure キューと Service Bus キューの []][]します。

## サブスクリプションからメッセージを受信する

サブスクリプションからメッセージを受信する最良の方法は、**ServiceBusRestProxy->receiveSubscriptionMessage** メソッドを使用することです。 受信したメッセージは、2 つの異なるモード (**ReceiveAndDelete** (既定) と **PeekLock**) で受信できます。

**ReceiveAndDelete** モードを使用する場合、受信が 1 回ずつの動作になります。つまり、Service Bus はサブスクリプション内のメッセージに対する読み取り要求を受け取ると、メッセージを読み取り中としてマークし、アプリケーションに返します。 **ReceiveAndDelete** モードは最もシンプルなモデルであり、シナリオに最適な
障害発生時にアプリケーション側でメッセージを処理しないことを許容できるシナリオに最適です。 このことを理解するために、コンシューマーが受信要求を発行した後で、メッセージを処理する前にクラッシュしたというシナリオを考えてみましょう。 Service Bus はメッセージを読み取り済みとしてマークするため、アプリケーションが再起動してメッセージの読み取りを再開すると、クラッシュ前に読み取られていたメッセージは見落とされることになります。

**PeekLock** モードでは、メッセージの受信処理が 2 段階の動作になり、メッセージが失われることが許容できないアプリケーションに対応することができます。 サービス バスは要求を受け取ると、次に読み取られるメッセージを検索して、他のコンシューマーが受信できないようロックしてから、アプリケーションにメッセージを返します。 アプリケーションがメッセージの処理を終えた後 (または後で処理するために確実に保存した後)、受信したメッセージを **ServiceBusRestProxy->deleteMessage** に渡して受信処理の第 2 段階を完了します。 サービス バスが **deleteMessage** の呼び出しを確認すると、メッセージが読み取り中としてマークされ、キューから削除されます。

次の例は、**PeekLock** モード (既定ではないモード) を使用したメッセージの受信および処理の方法を示しています。

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

Service Bus には、アプリケーションにエラーが発生した場合や、メッセージの処理に問題がある場合に復旧を支援する機能が備わっています。 受信側のアプリケーションが何らかの理由によってメッセージを処理できない場合には、受信したメッセージについて (**deleteMessage** メソッドの代わりに) **unlockMessage** メソッドを呼び出すことができます。 このメソッドが呼び出されると、Service Bus によってキュー内のメッセージのロックが解除され、メッセージが再度受信できる状態に変わります。メッセージを受信するアプリケーションは、以前と同じものでも、別のものでもかまいません。

キュー内でロックされているメッセージには、タイムアウトも設定されています。アプリケーションがクラッシュした場合など、ロックがタイムアウトになる前にアプリケーションがメッセージの処理に失敗した場合は、Service Bus によってメッセージのロックが自動的に解除され、再度受信できる状態に変わります。

メッセージが処理された後、**deleteMessage** 要求が発行される前にアプリケーションがクラッシュした場合は、アプリケーションが再起動する際にメッセージが再配信されます。 一般的に、この動作は **1 回以上の処理**と呼ばれます。つまり、すべてのメッセージが 1 回以上処理されますが、特定の状況では、同じメッセージが再配信される可能性があります。 重複処理が許されないシナリオの場合、重複メッセージの配信を扱うロジックをアプリケーションに追加する必要があります。 通常、この問題はメッセージの **getMessageId** メソッドを使用して対処します。このプロパティは、配信が試行された後も同じ値を保持します。

## トピックとサブスクリプションを削除する

トピックまたはサブスクリプションを削除するには、**ServiceBusRestProxy->deleteTopic** または **ServiceBusRestProxy->deleteSubscripton** メソッドをそれぞれ使用します。 トピックを削除すると、そのトピックに登録されたサブスクリプションもすべて削除されることに注意してください。

次の例は、という名前のトピックを削除する方法を示しています。 `mytopic` とその登録されたサブスクリプション。

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

**deleteSubscription** メソッドを使用すると、サブスクリプションを個別に削除できます。

```
$serviceBusRestProxy->deleteSubscription("mytopic", "mysubscription");
```

## 次のステップ

これをサービス バス キューの基本を学習できました。、を参照してください。 [キュー、トピック、およびサブスクリプションの []][] の詳細。


[queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md 
[sqlfilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx 
[require-once]: http://php.net/require_once 
[azure queues and service bus queues]: service-bus-azure-and-service-bus-queues-compared-contrasted.md#capacity-and-quotas 

