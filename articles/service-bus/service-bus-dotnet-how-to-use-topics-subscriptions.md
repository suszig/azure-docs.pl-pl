<properties
    pageTitle="Service Bus トピックの使用 (.NET) | Microsoft Azure"
    description="Azure における Service Bus のトピックとサブスクリプションを .NET で使用する方法について学習します。コード サンプルは .NET アプリケーション向けに作成されています。"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="10/15/2015"
    ms.author="sethm"/>


# Service Bus のトピックとサブスクリプションの使用方法

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

この記事では、Service Bus のトピックとサブスクリプションの使用方法について説明します。 サンプルは C# で記述され、.NET API を利用しています。 ここでは、トピックとサブスクリプションの作成、サブスクリプション フィルターの作成、トピックへのメッセージの送信、サブスクリプションからのメッセージの受信、トピックとサブスクリプションの削除などのシナリオについて説明します。 トピックとサブスクリプションの詳細については、次を参照してください。、 [次のステップ](#Next-steps) セクションです。

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## Service Bus を使用するためのアプリケーションの構成

Service Bus を使用するアプリケーションを作成するときには、Service Bus アセンブリに対する参照を追加して、対応する名前空間を含める必要があります。

## Service Bus NuGet パッケージの取得

Service Bus NuGet パッケージは、Service Bus API を取得し、Service Bus 依存関係をすべて備えたアプリケーションを構成する最も簡単な方法です。 NuGet Visual Studio 拡張機能を使用すると、Visual Studio や Visual Studio Express でのライブラリやツールのインストールと更新を簡単に行うことができます。 Service Bus NuGet パッケージは最も簡単な方法
Service Bus API を取得し、Service Bus 依存関係をすべて備えたアプリケーションを構成するには。

アプリケーションに NuGet パッケージをインストールするには、次のステップを行います。

1.  ソリューション エクスプ ローラーで右クリック **参照**, 、] をクリックし、
    **NuGet パッケージの管理**します。
2.  "Service Bus"を検索し、選択、* * Microsoft Azure
    Service bus の項目。 クリックして **インストール** 、インストールを完了するには
    次のダイアログ ボックスを閉じます。

    ![][7]

これで、Service Bus に対応するコードを作成する準備ができました。

## Service Bus の接続文字列の設定

Service Bus では、接続文字列を使用してエンドポイントと資格情報を格納します。 接続文字列は、ハードコーディングするのではなく、構成ファイルの中で指定します。

- Azure Cloud Services を使用するときには、Azure サービス構成システム (.csdef ファイルと .cscfg ファイル) を使用して接続文字列を格納することをお勧めします。
- Azure Websites または Azure Virtual Machines を使用する場合には、.NET 構成システム (Web.config ファイルなど) を使用して接続文字列を格納することをお勧めします。

いずれの場合も、この記事で後ほど説明する `CloudConfigurationManager.GetSetting` メソッドを使用して接続文字列を取得できます。

### Cloud Services を使用する場合の接続文字列の構成

サービス構成メカニズムは Azure クラウド サービス プロジェクトに特有と動的に構成設定を変更することができます、 [Azure クラシック ポータルの [][] 、アプリケーションを再デプロイしなくてもします。 たとえば、追加、 `設定` 、サービス定義にラベル (***.csdef**) ファイルは次の例に示すようにします。

```
<ServiceDefinition name="Azure1">
...
    <WebRole name="MyRole" vmsize="Small">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString" />
        </ConfigurationSettings>
    </WebRole>
...
</ServiceDefinition>
```

次に、サービス構成 (.cscfg) ファイルで値を指定します。

```
<ServiceConfiguration serviceName="Azure1">
...
    <Role name="MyRole">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString"
                     value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
        </ConfigurationSettings>
    </Role>
...
</ServiceConfiguration>
```

前のセクションで説明したように、Azure クラシック ポータルから取得した Shared Access Signature (SAS) のキー名とキー値を使用します。

### Azure Websites サイトまたは Azure Virtual Machines を使用する場合の接続文字列の構成

Websites または Virtual Machines を使用する場合には、.NET 構成システム (Web.config など) を使用することをお勧めします。使用して接続文字列を格納する、 `< appSettings >` 要素。

```
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
    </appSettings>
</configuration>
```

前のセクションで説明したように、Azure クラシック ポータルから取得した SAS 名とキー値を使用します。

## トピックを作成する

Service Bus のトピックとサブスクリプションの管理操作を行うことができます、 [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) クラスです。 このクラスにはトピックの作成、列挙、削除のためのメソッドが用意されています。

次の例を構築、 `NamespaceManager` オブジェクト、Azure を使用して `CloudConfigurationManager` クラス
Service Bus サービス名前空間と、適切なベース アドレスで構成される接続文字列を使用
SAS の資格情報で構成されます。 この接続文字列は、次のようになっています。

```
Endpoint=sb://<yourServiceNamespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey
```

Use the following example, given the configuration settings in the previous section.
```
既に存在しない場合は、トピックを作成します。
文字列の connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString) です。

場合 (! namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic("TestTopic") です。
{
```

There are overloads of the [CreateTopic](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.createtopic.aspx) method that enable you to tune properties of the topic, for example, to set the default time-to-live (TTL) value to be applied to messages sent to the topic. These settings are applied by using the [TopicDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.aspx) class. The following example shows how to create a topic named **TestTopic** with a maximum size of 5 GB and a default message TTL of 1 minute.
```
トピックの設定を構成します。
TopicDescription td = 新しい TopicDescription("TestTopic") です。
td します。MaxSizeInMegabytes = 5120 です。
td します。DefaultMessageTimeToLive = 新しい TimeSpan (0, 1, 0) です。

カスタム設定で新しいトピックを作成します。
文字列の connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString) です。

場合 (! namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic(td) です。
{
```

> [AZURE.NOTE] You can use the [TopicExists](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.topicexists.aspx) method on [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) objects to check whether a topic with a specified name already exists within a namespace.

## Create a subscription

You can also create topic subscriptions using the [`NamespaceManager`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) class. Subscriptions are named and can have an optional filter that
restricts the set of messages passed to the subscription's virtual queue.

### Create a subscription with the default (MatchAll) filter

The **MatchAll** filter is the default filter that is used if no filter is specified when a new subscription is created. When you use the **MatchAll** filter, all messages published to the topic are placed in the subscription's virtual queue. The following example creates a subscription named "AllMessages" and uses the default **MatchAll** filter.
```
文字列の connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString) です。

場合 (! namespaceManager.SubscriptionExists ("TestTopic"、"AllMessages"))
{
    namespaceManager.CreateSubscription ("TestTopic"、"AllMessages") です。
{
```

### Create subscriptions with filters

You can also set up filters that enable you to specify which messages sent to a topic should appear within a specific topic subscription.

The most flexible type of filter supported by subscriptions is the [SqlFilter][] class, which implements a subset of SQL92. SQL filters operate on the properties of the messages that are published to the topic. For more information about the expressions that can be used with a SQL filter, see the [SqlFilter.SqlExpression][] syntax.

The following example creates a subscription named **HighMessages** with a [SqlFilter][] object that only selects messages that have a custom **MessageNumber** property greater than 3.
```
フィルター選択された"HighMessages"サブスクリプションを作成します。
SqlFilter highMessagesFilter =
   新しい SqlFilter ("MessageNumber > 3") です。

namespaceManager.CreateSubscription ("TestTopic"、
   "HighMessages"
   highMessagesFilter) です。
```

Similarly, the following example creates a subscription named **LowMessages** with a [SqlFilter][] that only selects messages that have a **MessageNumber** property less than or equal to 3.
```
フィルター選択に"LowMessages"サブスクリプションを作成します。
SqlFilter lowMessagesFilter =
   新しい SqlFilter ("MessageNumber < = 3") です。

namespaceManager.CreateSubscription ("TestTopic"、
   "LowMessages"
   lowMessagesFilter) です。
```

Now when a message is sent to `TestTopic`, it is always delivered to receivers subscribed to the **AllMessages** topic subscription, and selectively delivered to receivers subscribed to the **HighMessages** and **LowMessages** topic subscriptions (depending on the message content).

## Send messages to a topic

To send a message to a Service Bus topic, your application creates a [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) object using the connection string.

The following code demonstrates how to create a [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) object for the **TestTopic** topic created earlier using the [`CreateFromConnectionString`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.createfromconnectionstring.aspx) API call.
```
文字列の connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

TopicClient クライアント =
    TopicClient.CreateFromConnectionString (connectionString、"TestTopic") です。

Client.Send (新しい BrokeredMessage()) です。
```

Messages sent to Service Bus topics are instances of the [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) class. [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) objects have a set of
standard properties (such as [Label](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) and [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), a dictionary that is used to hold custom application-specific properties, and a body of arbitrary application data. An application can set the body of the message by passing any serializable object to the constructor of the [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) object, and the appropriate **DataContractSerializer** is then used to serialize the object. Alternatively, a **System.IO.Stream** can be provided.

The following example demonstrates how to send five test messages to the **TestTopic** [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) object obtained in the previous code example. Note that the [MessageNumber](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.properties.aspx) property value of each message varies depending on the iteration of the loop (this determines which subscriptions receive it).
```
(int i = 0; は < 5; i + +)
{
  本文の文字列メッセージを渡すメッセージを作成します。
  BrokeredMessage メッセージ = 新しい BrokeredMessage ("テスト message"+ i) です。

  追加のカスタム アプリケーションに固有のプロパティを設定します。
  メッセージです。プロパティ ["MessageNumber"] = i です。

  メッセージをトピックに送信します。
  Client.Send(message) です。
{
```

Service Bus topics support a [maximum message size of 256 KB](service-bus-quotas.md) (the header, which includes the standard and custom application properties, can have a maximum size of 64 KB). There is no limit on the number of messages held in a topic but there is a cap on the total size of the messages held by a topic. This topic size is defined at creation time, with an upper limit of 5 GB. If partitioning is enabled, the upper limit is higher. For more information, see [Partitioning Messaging Entities](https://msdn.microsoft.com/library/azure/dn520246.aspx).

## How to receive messages from a subscription

The recommended way to receive messages from a subscription is to use a [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) object. [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) objects can work in two different modes: [ReceiveAndDelete and PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx).

When using the **ReceiveAndDelete** mode, receive is a single-shot operation; that is, when Service Bus receives a read request for a message in a subscription, it marks the message as being consumed and returns it to the application. **ReceiveAndDelete** mode is the simplest model and works best for scenarios in which an application can tolerate not processing a message in the event of a failure. To understand this, consider a scenario in which the consumer issues the receive request and then crashes before processing it. Because Service Bus will have marked the message as consumed, when the application restarts and begins consuming messages again, it will have missed the message that was consumed prior to the crash.

In **PeekLock** mode (which is the default mode), the receive process becomes a two-stage operation which makes it possible to support applications that cannot tolerate missing messages. When Service Bus receives a request,
it finds the next message to be consumed, locks it to prevent other consumers receiving it, and then returns it to the application. After the application finishes processing the message (or stores it reliably for future processing), it completes the second stage of the receive process by calling [Complete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) on the received message. When Service Bus sees the **Complete** call, it marks the message as being consumed and removes it from the subscription.

The following example demonstrates how messages can be received and processed using the default **PeekLock** mode. To specify a different [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) value, you can use another overload for [CreateFromConnectionString](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.createfromconnectionstring.aspx). This example uses the [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) callback to process messages as they arrive
into the **HighMessages** subscription.
```
文字列の connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

SubscriptionClient クライアント =
    SubscriptionClient.CreateFromConnectionString
            (接続文字列で"TestTopic"、"HighMessages") です。

コールバック オプションを構成します。
OnMessageOptions オプション = 新しい OnMessageOptions() です。
オプションです。AutoComplete = false。
オプションです。AutoRenewTimeout = TimeSpan.FromMinutes(1) です。

Client.OnMessage((message) = >
{
    try
    {
        サブスクリプションからメッセージを処理します。
        Console.WriteLine ("\n**高メッセージ**") です。
        Console.WriteLine ("本文:"+ メッセージです。GetBody<string>()) です。
        Console.WriteLine ("MessageID:"+ メッセージです。MessageId) です。
        Console.WriteLine ("メッセージ番号:"+
            メッセージです。Properties["MessageNumber"]) です。

        // Remove message from subscription.
        message.Complete();
    }
    catch (Exception)
    {
        // Indicates a problem, unlock message in subscription.
        message.Abandon();
    }

}、オプション)。
```

This example configures the [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) callback using an [OnMessageOptions](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.aspx) object. [AutoComplete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autocomplete.aspx) is set to **false** to enable manual control of when to call [Complete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) on the received message. [AutoRenewTimeout](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autorenewtimeout.aspx) is set to 1 minute, which causes the client to wait for up to one minute for a message before the call times out and the client makes a new call to check for messages. This property value reduces the number of times the client makes chargeable calls that do not retrieve messages.

## How to handle application crashes and unreadable messages

Service Bus provides functionality to help you gracefully recover from errors in your application or difficulties processing a message. If a receiving application is unable to process the message for some reason, then it can call the [Abandon](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.abandon.aspx) method on the received message (instead
of the [Complete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) method). This causes Service Bus to unlock the message within the subscription and make it available to be received again, either by the same consuming application or by another consuming application.

There is also a time-out associated with a message locked within the subscription, and if the application fails to process the message before the lock time-out expires (for example, if the application crashes), then Service Bus unlocks the message automatically and makes it available to be received again.

In the event that the application crashes after processing the message but before the [Complete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) request is issued, the message will be redelivered to the application when it restarts. This is often called *At Least Once processing*; that is, each message is processed at least once but in certain situations the same message may be
redelivered. If the scenario cannot tolerate duplicate processing, then application developers should add additional logic to their application to handle duplicate message delivery. This is often achieved using the
[MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) property of the message, which will remain constant across delivery attempts.

## Delete topics and subscriptions

The following example demonstrates how to delete the topic **TestTopic** from the **HowToSample** service namespace.
```
トピックを削除します。
namespaceManager.DeleteTopic("TestTopic") です。
```

Deleting a topic also deletes any subscriptions that are registered with the topic. Subscriptions can also be deleted independently. The following code demonstrates how to delete a subscription named **HighMessages** from the **TestTopic** topic.
```
namespaceManager.DeleteSubscription ("TestTopic"、"HighMessages") です。
```

## 次のステップ

これで、Service Bus のトピックとサブスクリプションの基本を学習できました。さらに詳細な情報が必要な場合は、次のリンク先を参照してください。

-   参照してください [キュー、トピック、およびサブスクリプションの][]します。
-   API リファレンス [SqlFilter:operator[]][]します。
-   サービス バス キューとメッセージを送受信する実用アプリケーションの作成: [サービス バス仲介型メッセージングに関する .NET チュートリアル][]します。
-   Service Bus のサンプル: からダウンロード [Azure サンプル][] または参照してください、 [概要](service-bus-samples.md)します。


[azure classic portal]: http://manage.windowsazure.com 
[7]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/getting-started-multi-tier-13.png 
[queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md 
[sqlfilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx 
[sqlfilter.sqlexpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx 
[service bus brokered messaging .net tutorial]: service-bus-brokered-tutorial-dotnet.md 
[azure samples]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2 

