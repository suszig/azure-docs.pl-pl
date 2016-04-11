<properties
    pageTitle="Service Bus トピックの使用 (.NET) | Microsoft Azure"
    description="Azure における Service Bus のトピックとサブスクリプションを .NET で使用する方法について学習します。 コード サンプルは .NET アプリケーション向けに作成されています。"
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
2.  "Service Bus"を検索し、選択、 **Microsoft Azure
    Service Bus** 項目。 クリックして **インストール** 、インストールを完了するには
    次のダイアログ ボックスを閉じます。

    ![][7]

これで、Service Bus に対応するコードを作成する準備ができました。

## Service Bus の接続文字列の設定

Service Bus では、接続文字列を使用してエンドポイントと資格情報を格納します。 接続文字列は、ハードコーディングするのではなく、構成ファイルの中で指定します。

- Azure Cloud Services を使用するときには、Azure サービス構成システム (.csdef ファイルと .cscfg ファイル) を使用して接続文字列を格納することをお勧めします。
- Azure Websites または Azure Virtual Machines を使用する場合には、.NET 構成システム (Web.config ファイルなど) を使用して接続文字列を格納することをお勧めします。

いずれの場合も、この記事で後ほど説明する `CloudConfigurationManager.GetSetting` メソッドを使用して接続文字列を取得できます。

### Cloud Services を使用する場合の接続文字列の構成

サービス構成メカニズムは Azure クラウド サービス プロジェクトに特有と動的に構成設定を変更することができます、 [Azure クラシック ポータル][] 、アプリケーションを再デプロイしなくてもします。 たとえば、追加、 `Setting` 、サービス定義にラベル (***.csdef**) ファイルは次の例に示すようにします。

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

Websites または Virtual Machines を使用する場合には、.NET 構成システム (Web.config など) を使用することをお勧めします。 `<appSettings>` 要素を使用して接続文字列を格納します。

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

前のセクションの構成設定であれば、次の例を使用します。

```
// Create the topic if it does not exist already.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic("TestTopic");
}
```

オーバー ロードがあり、 [CreateTopic](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.createtopic.aspx) など、トピックのプロパティを調整する、トピックに送信されるメッセージに適用される既定の有効期間 (TTL) 値を設定できるようにするメソッドです。 使用してこれらの設定が適用される、 [TopicDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.aspx) クラスです。 次の例は、という名前のトピックを作成する方法を示しています。 **TestTopic** 5 GB、既定値の最大サイズが、メッセージの 1 分の TTL であります。

```
// Configure Topic Settings.
TopicDescription td = new TopicDescription("TestTopic");
td.MaxSizeInMegabytes = 5120;
td.DefaultMessageTimeToLive = new TimeSpan(0, 1, 0);

// Create a new Topic with custom settings.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic(td);
}
```

> [AZURE.NOTE] 使用することができます、 [TopicExists](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.topicexists.aspx) メソッドを [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) オブジェクトを名前空間内で指定した名前のトピックが既に存在するかどうかを確認します。

## サブスクリプションを作成する

トピック サブスクリプションを作成することも、 [`NamespaceManager`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) クラスです。 サブスクリプションを指定し、
サブスクリプションの仮想キューに渡されるメッセージのセットを制限します。

### 既定の (MatchAll) フィルターを適用したサブスクリプションの作成

 **MatchAll** フィルターは、新しいサブスクリプションが作成されるときにフィルターが指定されていない場合に使用される既定のフィルターです。 使用すると、 **MatchAll** フィルター、トピックに発行されたすべてのメッセージがサブスクリプションの仮想キューに置かれます。 次の例は、"AllMessages"という名前のサブスクリプションを作成し、既定値を使用して **MatchAll** フィルター。

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.SubscriptionExists("TestTopic", "AllMessages"))
{
    namespaceManager.CreateSubscription("TestTopic", "AllMessages");
}
```

### フィルターを適用したサブスクリプションの作成

トピックに送信されたメッセージのうち、特定のトピック サブスクリプション内に表示されるメッセージを指定できるフィルターを設定することもできます。

サブスクリプションでサポートされるフィルターの最も柔軟性の高い型は、 [SqlFilter][] を SQL92 のサブセットを実装するクラス。 SQL フィルターは、トピックに発行されるメッセージのプロパティに対して適用されます。 SQL フィルターで使用できる式の詳細については、次を参照してください。、 [SqlFilter.SqlExpression][] 構文です。

という名前のサブスクリプションを作成する例を次 **HighMessages** で、 [SqlFilter][] のみ、カスタム メッセージを選択するオブジェクト **MessageNumber** プロパティが 3 を超える。

```
// Create a "HighMessages" filtered subscription.
SqlFilter highMessagesFilter =
   new SqlFilter("MessageNumber > 3");

namespaceManager.CreateSubscription("TestTopic",
   "HighMessages",
   highMessagesFilter);
```

同様に、次の例がという名前のサブスクリプションを作成 **LowMessages** で、 [SqlFilter][] のみを持つメッセージを選択する、 **MessageNumber** 3 のプロパティができます。

```
// Create a "LowMessages" filtered subscription.
SqlFilter lowMessagesFilter =
   new SqlFilter("MessageNumber <= 3");

namespaceManager.CreateSubscription("TestTopic",
   "LowMessages",
   lowMessagesFilter);
```

メッセージが送信されるタイミングに今すぐ `TestTopic`, をサブスクライブした受信者に必ず配信、 **AllMessages** トピック サブスクリプションにサブスクライブされた受信者に対して選択的に配信されると、 **HighMessages** と **LowMessages** (メッセージの内容に応じてトピックのサブスクリプション。

## メッセージをトピックに送信する

Service Bus トピックにメッセージを送信するアプリケーションを作成、 [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) オブジェクトの接続文字列を使用します。

次のコードを作成する方法の例、 [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) のオブジェクト、 **TestTopic** トピックが既に作成されて、 [`CreateFromConnectionString`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.createfromconnectionstring.aspx) API 呼び出し。

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

TopicClient Client =
    TopicClient.CreateFromConnectionString(connectionString, "TestTopic");

Client.Send(new BrokeredMessage());
```

Service Bus トピックに送信されたメッセージは、 [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) クラスです。 [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) オブジェクトのセットがあります。
標準的なプロパティ (よう [ラベル](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) と [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)) アプリケーションに固有のカスタム プロパティの保持に使用するディクショナリ、および任意のアプリケーション データの本体。 アプリケーションは、のコンス トラクターにシリアル化可能なオブジェクトを渡すことによって、メッセージの本文を設定することができます、 [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) オブジェクト、および適切な **DataContractSerializer** オブジェクトをシリアル化に使用されます。 または、 **System.IO.Stream** を指定できます。

次の例では、5 つのテスト メッセージを送信する方法、 **TestTopic** [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) 前のコード例でオブジェクトを取得します。 なお、 [MessageNumber](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.properties.aspx) 各メッセージのプロパティの値がループの反復回数に応じて変化 ([受信するサブスクリプションが決定されます)。

```
for (int i=0; i<5; i++)
{
  // Create message, passing a string message for the body.
  BrokeredMessage message = new BrokeredMessage("Test message " + i);

  // Set additional custom app-specific property.
  message.Properties["MessageNumber"] = i;

  // Send message to the topic.
  Client.Send(message);
}
```

Service Bus のトピックではサポート、 [256 KB のメッセージの最大サイズ](service-bus-quotas.md) (標準とカスタムのアプリケーション プロパティが含まれているヘッダーは 64 KB の最大サイズを設定できます)。 トピックで保持されるメッセージ数には上限がありませんが、1 つのトピックで保持できるメッセージの合計サイズには上限があります。 このトピックのサイズはトピックの作成時に定義します。上限は 5 GB です。 パーティション分割が有効な場合、上限が高くなります。 詳細については、次を参照してください。 [メッセージング エンティティのパーティション分割](https://msdn.microsoft.com/library/azure/dn520246.aspx)します。

## サブスクリプションからメッセージを受信する方法

サブスクリプションからメッセージを受信することをお勧めを使用して、 [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) オブジェクトです。 [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) オブジェクトには 2 つの異なるモードで動作します。 [ReceiveAndDelete と PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx)します。

使用する場合、 **ReceiveAndDelete** モードでは、受信は単発の操作です。 つまり、Service Bus はサブスクリプション内のメッセージに対する読み取り要求を受け取る、メッセージを読み取り中としてマークし、アプリケーションに返します。 **ReceiveAndDelete** モードは最もシンプルなモデルであり、障害発生時のメッセージを処理しない、アプリケーションが許容できるシナリオに最適です。 このことを理解するために、コンシューマーが受信要求を発行した後で、メッセージを処理する前にクラッシュしたというシナリオを考えてみましょう。 Service Bus はメッセージを読み取り済みとしてマークするため、アプリケーションが再起動してメッセージの読み取りを再開すると、クラッシュ前に読み取られていたメッセージは見落とされることになります。

 **PeekLock** モード (既定のモード) で、受信処理メッセージが失われることが許容できないアプリケーションに対応することが 2 段階の動作になります。 Service Bus は、要求を受け取る場合
次のメッセージを使用するを見つけ、その他のコンシューマーが受信できないようロックして、し、アプリケーションに返します。 呼び出して受信処理の第 2 段階を完了したら、アプリケーションがメッセージの処理 (または後で処理するために確実に保存) [完了](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) を完了します。 Service Bus が確認、 **完了** 呼び出し、メッセージを読み取り中としてマークし、サブスクリプションから削除されます。

次の例は、メッセージの受信および既定値を使用して処理を示しています。 **PeekLock** モードです。 異なる [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) 値、別のオーバー ロードを使用する [CreateFromConnectionString](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.createfromconnectionstring.aspx)します。 この例では、 [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) が到着するごとにメッセージを処理するコールバック
 **HighMessages** サブスクリプションです。

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

SubscriptionClient Client =
    SubscriptionClient.CreateFromConnectionString
            (connectionString, "TestTopic", "HighMessages");

// Configure the callback options.
OnMessageOptions options = new OnMessageOptions();
options.AutoComplete = false;
options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

Client.OnMessage((message) =>
{
    try
    {
        // Process message from subscription.
        Console.WriteLine("\n**High Messages**");
        Console.WriteLine("Body: " + message.GetBody<string>());
        Console.WriteLine("MessageID: " + message.MessageId);
        Console.WriteLine("Message Number: " +
            message.Properties["MessageNumber"]);

        // Remove message from subscription.
        message.Complete();
    }
    catch (Exception)
    {
        // Indicates a problem, unlock message in subscription.
        message.Abandon();
    }
}, options);
```

この例では構成、 [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) コールバックを使用して、 [OnMessageOptions](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.aspx) オブジェクトです。 [オートコンプリート](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autocomplete.aspx) に設定されている **false** を呼び出すときに手動で制御を有効にする [完了](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) 受信したメッセージに対してです。 [AutoRenewTimeout](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autorenewtimeout.aspx) により、クライアント呼び出しがタイムアウトになると、クライアントがメッセージを確認する新しい呼び出しを行う前に、メッセージの最大 1 分間の待機が 1 分間に設定されています。 このプロパティ値により、メッセージを取得しない課金対象呼び出しをクライアントが行う回数が減ります。

## アプリケーションのクラッシュと読み取り不能のメッセージを処理する方法

Service Bus には、アプリケーションにエラーが発生した場合や、メッセージの処理に問題がある場合に復旧を支援する機能が備わっています。 受信側のアプリケーションが何らかの理由でのメッセージを処理できないかどうかは、呼び出すことができます、 [破棄](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.abandon.aspx) メソッドを受信したメッセージ (代わりに
 [完了](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) メソッド)。 このメソッドが呼び出されると、Service Bus によりサブスクリプション内のメッセージのロックが解除され、メッセージが再度受信できる状態に変わります。このメッセージは、同じコンシューマー側アプリケーションまたは他のコンシューマー側アプリケーションで受信されます。

サブスクリプション内でロックされているメッセージにはタイムアウトも設定されています。アプリケーションがクラッシュした場合など、ロックがタイムアウトになる前にアプリケーションがメッセージの処理に失敗した場合には、Service Bus によってメッセージのロックが自動的に解除され、再度受信できる状態に変わります。

前に、メッセージを処理した後、アプリケーションがクラッシュすること、 [完了](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) 要求が発行される、再起動する際にそのメッセージがアプリケーションに再配信されます。 一般的に、 *At Least Once 処理*つまり、すべてのメッセージが 1 回以上処理がは特定の状況で、同じメッセージがあります。
再配信されます。 重複処理が許されないシナリオの場合、重複メッセージの配信を扱うロジックをアプリケーションに追加する必要があります。 通常、この問題はメッセージの
[MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) は一定に保たれる配信が試行されると、メッセージのプロパティです。

## トピックとサブスクリプションを削除する

次の例は、トピックを削除する方法を示します **TestTopic** から、 **HowToSample** サービス名前空間。

```
// Delete Topic.
namespaceManager.DeleteTopic("TestTopic");
```

トピックを削除すると、そのトピックに登録されたサブスクリプションもすべて削除されます。 サブスクリプションは、個別に削除することもできます。 次のコードでは、という名前のサブスクリプションを削除する方法の例 **HighMessages** から、 **TestTopic** トピックです。

```
namespaceManager.DeleteSubscription("TestTopic", "HighMessages");
```

## 次のステップ

これで、Service Bus のトピックとサブスクリプションの基本を学習できました。さらに詳細な情報が必要な場合は、次のリンク先を参照してください。

-   参照してください [キュー、トピック、およびサブスクリプション][]します。
-   API リファレンス [SqlFilter][]します。
-   サービス バス キューとメッセージを送受信する実用アプリケーションの作成: [サービス バスの仲介型メッセージングに関する .NET チュートリアル][]します。
-   Service Bus のサンプル: からダウンロード [Azure サンプル][] または参照してください、 [概要](service-bus-samples.md)します。

  [Azure classic portal]: http://manage.windowsazure.com

  [7]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/getting-started-multi-tier-13.png

  [Queues, Topics, and Subscriptions]: service-bus-queues-topics-subscriptions.md
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx
  [SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Service Bus Brokered Messaging .NET Tutorial]: service-bus-brokered-tutorial-dotnet.md
  [Azure Samples]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2

