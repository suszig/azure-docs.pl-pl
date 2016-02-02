<properties
    pageTitle="Service Bus キューの使用方法 (.NET) | Microsoft Azure"
    description="Azure での Service Bus キューの使用方法を学習します。コード サンプルは .NET API を使用して C# で記述されています。"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="10/07/2015"
    ms.author="sethm"/>


# Service Bus キューの使用方法

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

この記事では、Service Bus キューの使用方法について説明します。 サンプルは C# で記述され、.NET API を利用しています。 紹介するシナリオはキューの作成とメッセージの送受信です。 キューの詳細については、次を参照してください。、 [次のステップ](#next-steps) セクションです。

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## Service Bus を使用するためのアプリケーションの構成

Service Bus を使用するアプリケーションを作成するときには、Service Bus アセンブリに対する参照を追加して、対応する名前空間を含める必要があります。

## Service Bus NuGet パッケージの追加

Service Bus **NuGet** パッケージは、Service Bus API を取得し、Service Bus 依存関係をすべて備えたアプリケーションを構成する最も簡単な方法です。 NuGet Visual Studio 拡張機能を使用すると、Visual Studio や Visual Studio Express でのライブラリやツールのインストールと更新を簡単に行うことができます。 Service Bus NuGet パッケージは最も簡単な方法
Service Bus API を取得し、Service Bus 依存関係をすべて備えたアプリケーションを構成するには。

アプリケーションに NuGet パッケージをインストールするには、次のステップを行います。

1.  ソリューション エクスプローラーで **[参照]** を右クリックし、 **[NuGet パッケージの管理]** をクリックします。
2.  "Service Bus" を検索して、**[Microsoft Azure Service Bus]** 項目を選択します。 **[インストール]** をクリックし、インストールが完了したら、このダイアログ ボックスを閉じます。

    ![][7]

これで、Service Bus に対応するコードを作成する準備ができました。

## Service Bus の接続文字列の設定

Service Bus では、接続文字列を使用してエンドポイントと資格情報を格納します。 接続文字列は、ハードコーディングするのではなく、構成ファイルの中で指定します。

- Azure Cloud Services を使用するときには、Azure サービス構成システム (.csdef ファイルと .cscfg ファイル) を使用して接続文字列を格納することをお勧めします。
- Azure Websites または Azure Virtual Machines を使用する場合には、.NET 構成システム (Web.config ファイルなど) を使用して接続文字列を格納することをお勧めします。

どちらの場合も、使用して接続文字列を取得できます、 [CloudConfigurationManager.GetSetting][getsetting] メソッドは、この記事で後ほど説明します。

### Cloud Services を使用する場合の接続文字列の構成

サービス構成メカニズムは Azure クラウド サービス プロジェクトに特有と動的に構成設定を変更することができます、 [Azure クラシック ポータルの [][] 、アプリケーションを再デプロイしなくてもします。 たとえば、追加、 `設定` 次の例に示すように、サービス定義 (.csdef) ファイルにラベルを付けます。

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
次の例に示すように、サービス構成 (.cscfg) ファイルで値を指定します。

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

### Websites または Azure Virtual Machines を使用する場合の接続文字列の構成

Websites または Virtual Machines を使用する場合には、.NET 構成システム (**Web.config** など) を使用することをお勧めします。使用して接続文字列を格納する、 `< appSettings >` 要素。

```
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
    </appSettings>
</configuration>
```

前のセクションで説明したように、Azure クラシック ポータルから取得した SAS 名とキー値を使用します。

## キューを作成する

使用して Service Bus キューに対する管理操作を行うことができます、 [NamespaceManager:operator[]][] クラスです。 このクラスにはキューの作成、列挙、削除のためのメソッドが用意されています。

この例で作成、 [NamespaceManager][] オブジェクト、Azure を使用して [CloudConfigurationManager][] それを管理するアクセス許可を持つ資格情報クラスの Service Bus サービス名前空間と、適切なベース アドレスで構成される接続文字列を使用します。 この接続文字列は、次の例に示すような形式になっています。

````
Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedSecretValue=yourKey
````

Use the following example, given the configuration settings in the previous section.
````
既に存在しない場合は、キューを作成します。
文字列の connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString) です。

場合 (! namespaceManager.QueueExists("TestQueue"))
{
    namespaceManager.CreateQueue("TestQueue") です。
{
```

There are overloads of the [CreateQueue](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.createqueue.aspx) method that enable you to tune properties
of the queue (for example, to set the default time-to-live (TTL) value to be applied to messages sent to the queue). These settings are applied using the [QueueDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx) class. The following example shows how to create a queue named `TestQueue` with a maximum size of 5 GB and a default message TTL of 1 minute.
```
キューの設定を構成します。
QueueDescription qd = 新しい QueueDescription("TestQueue") です。
qd します。MaxSizeInMegabytes = 5120 です。
qd します。DefaultMessageTimeToLive = 新しい TimeSpan (0, 1, 0) です。

カスタム設定で新しいキューを作成します。
文字列の connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString) です。

場合 (! namespaceManager.QueueExists("TestQueue"))
{
    namespaceManager.CreateQueue(qd) です。
{
```

> [AZURE.NOTE] You can use the [QueueExists](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.queueexists.aspx) method on [NamespaceManager][] objects to check if a queue with a specified name already exists within a service namespace.

## Send messages to a queue

To send a message to a Service Bus queue, your application creates a
[QueueClient][] object using the connection string.

The following code demonstrates how to create a [QueueClient][] object for the `TestQueue` queue you just created using the [CreateFromConnectionString](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.createfromconnectionstring.aspx) API call.
```
文字列の connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

QueueClient クライアント =
    QueueClient.CreateFromConnectionString (connectionString、"TestQueue") です。

Client.Send (新しい BrokeredMessage()) です。
```

Messages sent to, and received from Service Bus queues are instances of the [BrokeredMessage][] class. [BrokeredMessage][] objects have a set of standard properties (such as [Label](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) and [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), a dictionary
that is used to hold custom application specific properties, and a body of arbitrary application data. An application can set the body of the message by passing any serializable object into the constructor of the[BrokeredMessage][] object, and the appropriate **DataContractSerializer** is then used to serialize the object. Alternatively, you can provide a **System.IO.Stream** object.

The following example demonstrates how to send five test messages to the `TestQueue` [QueueClient][] object obtained in the previous code example.
```
(int i = 0; は < 5; i + +)
{
  本文の文字列メッセージを渡すメッセージを作成します。
  BrokeredMessage メッセージ = 新しい BrokeredMessage ("テスト message"+ i) です。

  いくつか追加のカスタム アプリケーションに固有のプロパティを設定します。
  メッセージです。プロパティ ["TestProperty"] ="testvalue"というです。
  メッセージです。プロパティ [「メッセージ番号」] = i です。

  メッセージをキューに送信します。
  Client.Send(message) です。
{
```

Service Bus queues support a [maximum message size of 256 Kb](service-bus-quotas.md) (the header, which includes  the standard and custom application properties, can have a maximum size of 64 KB). There is no limit on the number of messages held in a queue but there is a cap on the total size of the messages held by a queue. This queue size is defined at creation time, with an upper limit of 5 GB. If partitioning is enabled, the upper limit is higher. For more information, see [Partitioning Messaging Entities](service-bus-partitioning.md).

## How to receive messages from a queue

The recommended way to receive messages from a queue is to use a [QueueClient][] object. [QueueClient][] objects can work in two different modes: [ReceiveAndDelete and PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx).

When using the **ReceiveAndDelete** mode, the receive is a single-shot operation - that is, when Service Bus receives a read request for a message in a queue, it marks the message as consumed, and returns it to the application. **ReceiveAndDelete** is the simplest model and works best for scenarios in which an application can tolerate not processing a message in the event of a failure. To understand this, consider a scenario in which the consumer issues the receive request and then crashes before processing it. Because Service Bus will have marked
the message as consumed, when the application restarts and begins consuming messages again, it will have missed the message that was consumed prior to the crash.

In **PeekLock** mode (which is the default mode), the receive becomes a two-stage operation, which makes it possible to support applications that cannot tolerate missing messages. When Service Bus receives a request,
it finds the next message to be consumed, locks it to prevent other consumers receiving it, and then returns it to the application. After the application finishes processing the message (or stores it reliably for future processing), it completes the second stage of the receive process by calling [Complete][] on the received message. When Service Bus sees the [Complete][] call, it marks the message as consumed, and removes it from the queue.

The following example demonstrates how messages can be received and processed using the default **PeekLock** mode. To specify a different [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) value, you can use another overload of
[CreateFromConnectionString](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.createfromconnectionstring.aspx). This example uses the [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.onmessage.aspx) callback
to process messages as they arrive into `TestQueue`.
```
文字列の connectionString =
  CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
QueueClient クライアント =
  QueueClient.CreateFromConnectionString (connectionString、"TestQueue") です。

コールバック オプションを構成します。
OnMessageOptions オプション = 新しい OnMessageOptions() です。
オプションです。AutoComplete = false。
オプションです。AutoRenewTimeout = TimeSpan.FromMinutes(1) です。

ハンドルが受信したメッセージにコールバックします。
Client.OnMessage((message) = >
{
    try
    {
        キューからメッセージを処理します。
        Console.WriteLine ("本文:"+ メッセージです。GetBody<string>()) です。
        Console.WriteLine ("MessageID:"+ メッセージです。MessageId) です。
        Console.WriteLine ("プロパティをテスト:"+
        メッセージです。Properties["TestProperty"]) です。

        // Remove message from queue.
        message.Complete();
    }
    catch (Exception)
    {
        // Indicates a problem, unlock message in queue.
        message.Abandon();
    }

}、オプション)。
```

この例では構成、 [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.onmessage.aspx) コールバックを使用して、 [OnMessageOptions](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.aspx) オブジェクトです。 [オートコンプリート](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autocomplete.aspx)
設定されている **false** を呼び出すタイミングを手動で制御を有効にする [完了][] 受信したメッセージに対してです。 [AutoRenewTimeout](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autorenewtimeout.aspx) により、クライアント呼び出しがタイムアウトになると、クライアントがメッセージを確認する新しい呼び出しを行う前に、メッセージの最大 1 分間の待機が 1 分間に設定されています。 このプロパティ値により、メッセージを取得しない課金対象呼び出しをクライアントが行う回数が減ります。

## アプリケーションのクラッシュと読み取り不能のメッセージを処理する方法

Service Bus には、アプリケーションにエラーが発生した場合や、メッセージの処理に問題がある場合に復旧を支援する機能が備わっています。 受信側アプリケーションが何らかの理由でのメッセージを処理できないかどうかは、呼び出すことができます、 [破棄](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.abandon.aspx) 受信したメッセージに対してメソッド (の代わりに、 [完了][] メソッド)。 このメソッドが呼び出されると、Service Bus によってキュー内のメッセージのロックが解除され、メッセージが再度受信できる状態に変わります。メッセージを受信するアプリケーションは、以前と同じものでも、別のものでもかまいません。

キュー内でロックされているメッセージにはタイムアウトも設定されています。アプリケーションがクラッシュした場合など、ロックがタイムアウトになる前にアプリケーションがメッセージの処理に失敗した場合には、Service Bus によってメッセージのロックが自動的に解除され、再度受信できる状態に変わります。

前に、メッセージを処理した後、アプリケーションがクラッシュすること、 [完了][] 要求が発行される、再起動する際にそのメッセージがアプリケーションに再配信されます。 一般的に、この動作は **1 回以上の処理**と呼ばれます。つまり、すべてのメッセージが 1 回以上処理されますが、特定の状況では、同じメッセージが再配信される可能性があります。 重複処理が許されないシナリオの場合、重複メッセージの配信を扱うロジックをアプリケーションに追加する必要があります。 これは、多くの場合を使用して、 [MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) 配信が試行されると、メッセージのプロパティです。

## 次のステップ

これで、Service Bus キューの基本を学習できました。さらに詳細な情報が必要な場合は、次のリンク先を参照してください。

-   Service Bus メッセージングでのエンティティについて [キュー、トピック、およびサブスクリプションの][]します。
-   サービス バス キューとメッセージを送受信する実用アプリケーションの作成、 [サービス バスが仲介するメッセージングに関する .NET チュートリアル][]します。
-   サービス バス サンプルのダウンロード [Azure サンプル][] または参照してください、 [Service Bus の概要サンプル][]します。


[azure classic portal]: http://manage.windowsazure.com 
[7]: ./media/service-bus-dotnet-how-to-use-queues/getting-started-multi-tier-13.png 
[queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md 
[service bus brokered messaging .net tutorial]: service-bus-brokered-tutorial-dotnet.md 
[azure samples]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2 
[overview of service bus samples]: service-bus-samples.md 
[getsetting]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.getsetting.aspx 
[cloudconfigurationmanager]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager 
[namespacemanager]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx 
[brokeredmessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx 
[queueclient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx 
[complete]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx 

