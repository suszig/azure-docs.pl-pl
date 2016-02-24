<properties
    pageTitle="Service Bus キューの使用方法 (.NET) | Microsoft Azure"
    description="Azure での Service Bus キューの使用方法を学習します。 コード サンプルは .NET API を使用して C# で記述されています。"
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

1.  ソリューション エクスプ ローラーで右クリック **参照**, 、クリックして **NuGet パッケージの管理**します。
2.  "Service Bus"を検索し、選択、 **Microsoft Azure Service Bus** 項目。 クリックして **インストール** インストールを完了するには、このダイアログ ボックスを閉じます。

    ![][7]

これで、Service Bus に対応するコードを作成する準備ができました。

## Service Bus の接続文字列の設定

Service Bus では、接続文字列を使用してエンドポイントと資格情報を格納します。 接続文字列は、ハードコーディングするのではなく、構成ファイルの中で指定します。

- Azure Cloud Services を使用するときには、Azure サービス構成システム (.csdef ファイルと .cscfg ファイル) を使用して接続文字列を格納することをお勧めします。
- Azure Websites または Azure Virtual Machines を使用する場合には、.NET 構成システム (Web.config ファイルなど) を使用して接続文字列を格納することをお勧めします。

どちらの場合も、使用して接続文字列を取得できます、 [説明する CloudConfigurationManager.GetSetting][GetSetting] メソッドは、この記事で後ほど説明します。

### Cloud Services を使用する場合の接続文字列の構成

サービス構成メカニズムは Azure クラウド サービス プロジェクトに特有と動的に構成設定を変更することができます、 [Azure クラシック ポータル][] 、アプリケーションを再デプロイしなくてもします。 たとえば、次の例に示すように、サービス定義 (.csdef) ファイルに `Setting` ラベルを追加します。

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

Web サイトまたは仮想マシンを使用する場合は、.NET 構成システムを使用することをお勧めしますが (たとえば、 **Web.config**)。 `<appSettings>` 要素を使用して接続文字列を格納します。

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

使用して Service Bus キューに対する管理操作を行うことができます、 [NamespaceManager][] クラスです。 このクラスにはキューの作成、列挙、削除のためのメソッドが用意されています。

この例で作成、 [NamespaceManager][] オブジェクト、Azure を使用して [CloudConfigurationManager][] それを管理するアクセス許可を持つ資格情報クラスの Service Bus サービス名前空間と、適切なベース アドレスで構成される接続文字列を使用します。 この接続文字列は、次の例に示すような形式になっています。

````
Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedSecretValue=yourKey
````

前のセクションの構成設定であれば、次の例を使用します。

```
// Create the queue if it does not exist already.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.QueueExists("TestQueue"))
{
    namespaceManager.CreateQueue("TestQueue");
}
```

オーバー ロードがあり、 [CreateQueue](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.createqueue.aspx) プロパティを調整できるようにする方法
キューの (キューに送信されるメッセージに適用される既定の有効期間 (TTL) 値を設定する場合など)。 使用してこれらの設定が適用される、 [QueueDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx) クラスです。 次の例では、名前が `TestQueue`、最大サイズが 5 GB、メッセージの既定の TTL が 1 分であるキューを作成する方法を示しています。

```
// Configure queue settings.
QueueDescription qd = new QueueDescription("TestQueue");
qd.MaxSizeInMegabytes = 5120;
qd.DefaultMessageTimeToLive = new TimeSpan(0, 1, 0);

// Create a new queue with custom settings.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.QueueExists("TestQueue"))
{
    namespaceManager.CreateQueue(qd);
}
```

> [AZURE.NOTE] 使用することができます、 [QueueExists](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.queueexists.aspx) メソッドを [NamespaceManager][] と指定した名前のキューがサービス名前空間に既に存在をチェックするオブジェクト。

## メッセージをキューに送信する

アプリケーションで Service Bus キューにメッセージを送信するには、
[QueueClient][] オブジェクトの接続文字列を使用します。

次のコードを作成する方法の例、 [QueueClient][] のオブジェクト、 `TestQueue` を使用して作成したキュー、 [CreateFromConnectionString](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.createfromconnectionstring.aspx) API 呼び出し。

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

QueueClient Client =
    QueueClient.CreateFromConnectionString(connectionString, "TestQueue");

Client.Send(new BrokeredMessage());
```

メッセージには、送信、Service Bus から受信したキューのインスタンス、 [BrokeredMessage][] クラスです。 [BrokeredMessage][] オブジェクトがある一連の標準的なプロパティ (よう [ラベル](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) と [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx))、ディクショナリ
カスタム アプリケーション固有のプロパティおよび任意のアプリケーション データの本体を保持するために使用されます。 アプリケーションは、のコンス トラクターにシリアル化可能なオブジェクトを渡すことによって、メッセージの本文を設定することができます、[BrokeredMessage][] オブジェクト、および適切な **DataContractSerializer** オブジェクトをシリアル化に使用されます。 または、提供、 **System.IO.Stream** オブジェクトです。

次の例では、5 つのテスト メッセージを送信する方法、 `TestQueue` [QueueClient][] 前のコード例でオブジェクトを取得します。

```
for (int i=0; i<5; i++)
{
  // Create message, passing a string message for the body.
  BrokeredMessage message = new BrokeredMessage("Test message " + i);

  // Set some addtional custom app-specific properties.
  message.Properties["TestProperty"] = "TestValue";
  message.Properties["Message number"] = i;

  // Send message to the queue.
  Client.Send(message);
}
```

Service Bus キューのサポート、 [256 Kb のメッセージの最大サイズ](service-bus-quotas.md) (標準とカスタムのアプリケーション プロパティが含まれているヘッダーは 64 KB の最大サイズを設定できます)。 キューで保持されるメッセージ数には上限がありませんが、キュー 1 つあたりが保持できるメッセージの合計サイズには上限があります。 このキュー サイズは作成時に定義され、上限は 5 GB です。 パーティション分割が有効な場合、上限が高くなります。 詳細については、次を参照してください。 [メッセージング エンティティのパーティション分割](service-bus-partitioning.md)します。

## キューからメッセージを受信する方法

キューからメッセージを受信することをお勧めを使用して、 [QueueClient][] オブジェクトです。 [QueueClient][] オブジェクトには 2 つの異なるモードで動作します。 [ReceiveAndDelete と PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx)します。

使用する場合、 **ReceiveAndDelete** モードでは、受信は単発の操作では、Service Bus はキュー内のメッセージに対する読み取り要求を受け取る、メッセージを読み取り中としてマークし、アプリケーションに返します。 **ReceiveAndDelete** は最もシンプルなモデルであり、障害発生時のメッセージを処理しない、アプリケーションが許容できるシナリオに最適です。 このことを理解するために、コンシューマーが受信要求を発行した後で、メッセージを処理する前にクラッシュしたというシナリオを考えてみましょう。 Service Bus はマークするため
アプリケーションが再起動し、メッセージ読み取り中として、メッセージは失われて、クラッシュ前に読み取られていたメッセージです。

 **PeekLock** モード (既定のモード) で、受信メッセージが失われることが許容できないアプリケーションをサポートすることが 2 段階の動作になります。 Service Bus は、要求を受け取る場合
次のメッセージを使用するを見つけ、その他のコンシューマーが受信できないようロックして、し、アプリケーションに返します。 呼び出して受信処理の第 2 段階を完了したら、アプリケーションがメッセージの処理 (または後で処理するために確実に保存) [完了][] を完了します。 Service Bus が確認、 [完了][] 呼び出し、メッセージを読み取り中としてマークし、キューから削除します。

次の例は、メッセージの受信および既定値を使用して処理を示しています。 **PeekLock** モードです。 異なる [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) 値、別のオーバー ロードを使用することができます
[上で CreateFromConnectionString](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.createfromconnectionstring.aspx)します。 この例では、 [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.onmessage.aspx) コールバック
到着するごとにメッセージを処理する `TestQueue`です。

```
string connectionString =
  CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
QueueClient Client =
  QueueClient.CreateFromConnectionString(connectionString, "TestQueue");

// Configure the callback options.
OnMessageOptions options = new OnMessageOptions();
options.AutoComplete = false;
options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

// Callback to handle received messages.
Client.OnMessage((message) =>
{
    try
    {
        // Process message from queue.
        Console.WriteLine("Body: " + message.GetBody<string>());
        Console.WriteLine("MessageID: " + message.MessageId);
        Console.WriteLine("Test Property: " +
        message.Properties["TestProperty"]);

        // Remove message from queue.
        message.Complete();
    }
    catch (Exception)
    {
        // Indicates a problem, unlock message in queue.
        message.Abandon();
    }
}, options);
```

この例では構成、 [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.onmessage.aspx) コールバックを使用して、 [OnMessageOptions](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.aspx) オブジェクトです。 [オートコンプリート](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autocomplete.aspx)
設定されている **false** を呼び出すタイミングを手動で制御を有効にする [完了][] を完了します。 [AutoRenewTimeout](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autorenewtimeout.aspx) により、クライアント呼び出しがタイムアウトになると、クライアントがメッセージを確認する新しい呼び出しを行う前に、メッセージの最大 1 分間の待機が 1 分間に設定されています。 このプロパティ値により、メッセージを取得しない課金対象呼び出しをクライアントが行う回数が減ります。

## アプリケーションのクラッシュと読み取り不能のメッセージを処理する方法

Service Bus には、アプリケーションにエラーが発生した場合や、メッセージの処理に問題がある場合に復旧を支援する機能が備わっています。 受信側アプリケーションが何らかの理由でのメッセージを処理できないかどうかは、呼び出すことができます、 [破棄](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.abandon.aspx) 受信したメッセージに対してメソッド (の代わりに、 [完了][] メソッド)。 このメソッドが呼び出されると、Service Bus によってキュー内のメッセージのロックが解除され、メッセージが再度受信できる状態に変わります。メッセージを受信するアプリケーションは、以前と同じものでも、別のものでもかまいません。

キュー内でロックされているメッセージにはタイムアウトも設定されています。アプリケーションがクラッシュした場合など、ロックがタイムアウトになる前にアプリケーションがメッセージの処理に失敗した場合には、Service Bus によってメッセージのロックが自動的に解除され、再度受信できる状態に変わります。

前に、メッセージを処理した後、アプリケーションがクラッシュすること、 [完了][] 要求が発行される、再起動する際にそのメッセージがアプリケーションに再配信されます。 一般的に、 **1 回の処理には、少なくとも**。 つまり、すべてのメッセージが 1 回以上処理がは特定の状況で、同じメッセージが再配信されます。 重複処理が許されないシナリオの場合、重複メッセージの配信を扱うロジックをアプリケーションに追加する必要があります。 これは、多くの場合を使用して、 [MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) 配信が試行されると、メッセージのプロパティです。

## 次のステップ

これで、Service Bus キューの基本を学習できました。さらに詳細な情報が必要な場合は、次のリンク先を参照してください。

-   Service Bus メッセージングでのエンティティについて [キュー、トピック、およびサブスクリプション][]します。
-   サービス バス キューとメッセージを送受信する実用アプリケーションの作成、 [サービス バスが仲介するメッセージングに関する .NET チュートリアル][]します。
-   サービス バス サンプルのダウンロード [Azure サンプル][] または参照してください、 [Service Bus のサンプルの概要][]します。

  [Azure classic portal]: http://manage.windowsazure.com
  [7]: ./media/service-bus-dotnet-how-to-use-queues/getting-started-multi-tier-13.png
  [Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
  [Service Bus brokered messaging .NET tutorial]: service-bus-brokered-tutorial-dotnet.md
  [Azure samples]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
  [overview of Service Bus samples]: service-bus-samples.md
  [GetSetting]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.getsetting.aspx
  [CloudConfigurationManager]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager
  [NamespaceManager]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [Complete]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx
