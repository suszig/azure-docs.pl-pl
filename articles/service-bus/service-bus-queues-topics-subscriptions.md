<properties 
   pageTitle="Service Bus のキュー、トピック、サブスクリプション | Microsoft Azure"
   description="Service Bus メッセージング エンティティの概要です。"
   services="service-bus"
   documentationCenter="na"
   authors="sethmanheim"
   manager="timlt"
   editor="tysonn" />
<tags 
   ms.service="service-bus"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/09/2015"
   ms.author="sethm" />


# Service Bus のキュー、トピック、サブスクリプション

Microsoft Azure Service Bus は、信頼性の高いメッセージ キュー機能や永続的なパブリッシュ/サブスクライブ メッセージング機能など、クラウドベースのメッセージ指向ミドルウェアの一連のテクノロジをサポートしています。 このような仲介型メッセージング機能は、分離されたメッセージング機能と考えることができます。これは、Service Bus メッセージング ファブリックを使用するパブリッシュ/サブスクライブ、一時的な切り離し、負荷分散のシナリオをサポートします。 分離型通信には、クライアントとサーバーを必要に応じて接続し、非同期に操作を実行できるなど多数の利点があります。

Service Bus の仲介型メッセージング機能の中核を形成するメッセージング エンティティは、キュー、トピック/サブスクリプション、ルール/アクション、Event Hubs です。

## キュー

キューでは、コンシューマーが競合している場合のメッセージ配信に先入先出法 (FIFO) を使用します。 つまり、通常、メッセージは、キューに追加された順番で受信され、処理されると予想されます。このとき、メッセージを受信して処理できるメッセージ コンシューマーは、メッセージ 1 件につき 1 つだけです。 キューを使用する主な利点は、アプリケーション コンポーネントの "一時的な切り離し" を達成することです。 言い換えると、メッセージはキューに永続的に格納されるため、プロデューサー (送信者) とコンシューマー (受信者) が同時にメッセージを送受信する必要はありません。 さらに、プロデューサーは、メッセージの処理と送信を続ける場合、メッセージ コンシューマーからの応答を待つ必要がありません。

関連する利点として "負荷平準化" があります。これにより、プロデューサーとコンシューマーは異なるレートでメッセージを送受信できます。 多くのアプリケーションでは、システム負荷が時間の経過とともに変化しますが、各作業単位に必要な処理時間は通常一定に保たれます。 仲介のメッセージ プロデューサーとコンシューマーでキューを使用する場合、コンシューマー側アプリケーションに必要なのは、ピーク時の負荷ではなく平均的な負荷を処理できるようにプロビジョニングしておくことだけです。 キューの深さは、受信の負荷の変化に応じて増減します。 このため、アプリケーション負荷への対応に必要なインフラストラクチャに関して直接費用を節約できます。 負荷の増大に合わせて、キューからの読み取りのためにワーカー プロセスを追加できます。 各メッセージは、ワーカー プロセスの中の 1 つのプロセスによって処理されます。 さらに、このプルベースの負荷分散では、各ワーカー コンピューターがそれぞれ独自の最大レートでメッセージをプルするため、それらのコンピューターの処理能力が異なる場合であっても使用を最適化できます。 このパターンは、"競合コンシューマー" パターンと呼ばれることもあります。

キューを使用してメッセージ プロデューサーとメッセージ コンシューマーの間を仲介すると、必然的にコンポーネント間の結び付きは緩くなります。 プロデューサーとコンシューマーは相互に認識しないため、プロデューサーに影響することなく、コンシューマーをアップグレードできます。

キューの作成は、複数の手順から成るプロセスです。 使用して Service Bus メッセージング エンティティ (キューおよびトピック) の管理操作を実行する、 [、Microsoft.ServiceBus.NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) クラスは、Service Bus 名前空間とユーザーの資格情報のベース アドレスを指定して作成します。 [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) 作成、列挙、およびメッセージング エンティティを削除するメソッドを提供します。 作成した後、 [Microsoft.ServiceBus.TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) SAS 名とキー、およびサービス名前空間の管理からのオブジェクトのオブジェクトを使用することができます、 [Microsoft.ServiceBus.NamespaceManager.CreateQueue](https://msdn.microsoft.com/library/azure/hh293157.aspx) キューを作成します。 次に例を示します。

```
// Create management credentials
TokenProvider credentials = TokenProvider. CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
// Create namespace client
NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials);
```

その後、Service Bus の URI を引数として使用して、キュー オブジェクトとメッセージング ファクトリを作成できます。 次に例を示します。

```
QueueDescription myQueue;
myQueue = namespaceClient.CreateQueue("TestQueue");
MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials); 
QueueClient myQueueClient = factory.CreateQueueClient("TestQueue");
```

その後は、キューにメッセージを送信できます。 呼ばれる仲介型メッセージの一覧がある場合など `MessageList`, 、次のようなコードが表示されます。

```
for (int count = 0; count < 6; count++)
{
    var issue = MessageList[count];
    issue.Label = issue.Properties["IssueTitle"].ToString();
    myQueueClient.Send(issue);
}
```

その後、次のように、キューからメッセージを受信できます。

```
while ((message = myQueueClient.Receive(new TimeSpan(hours: 0, minutes: 0, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();

        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

[ReceiveAndDelete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) モードで受信操作はシングル ショットでは、Service Bus は、要求を受け取るときに、メッセージを読み取り中としてマークして、アプリケーションに返します。 [ReceiveAndDelete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) モードは最もシンプルなモデルであり、障害発生時のメッセージを処理しないで、アプリケーションが許容できるシナリオに最適です。 このことを理解するために、コンシューマーが受信要求を発行した後で、メッセージを処理する前にクラッシュしたというシナリオを考えてみましょう。 Service Bus がメッセージを読み取り中としてマークするため、アプリケーションは、再起動してメッセージの読み取りを再開すると、クラッシュ前に読み取られていたメッセージを見落とすことになります。

[PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) モードでは、受信操作が 2 段階の場合、これによりメッセージが失われることが許容できないアプリケーションに対応します。 Service Bus は要求を受け取ると、次に読み取られるメッセージを検索して、他のコンシューマーが受信できないようロックしてから、アプリケーションにそのメッセージを返します。 呼び出して受信処理の第 2 段階を完了したら、アプリケーションがメッセージの処理 (または後で処理するために確実に保存) [完了](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) を完了します。 Service Bus が確認、 [完了](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) 呼び出し、メッセージを読み取り中としてマークします。

アプリケーションが何らかの理由のメッセージを処理できない場合は、呼び出すことができます、 [破棄](https://msdn.microsoft.com/library/azure/hh181837.aspx) 受信したメッセージに対してメソッド (の代わりに [完了](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx))します。 このメソッドが呼び出されると、Service Bus によってメッセージのロックが解除され、同じコンシューマーまたは競合する別のコンシューマーが再度そのメッセージを受信できるようになります。 第二に、ロックに関連付けられたタイムアウトがあります。アプリケーションがクラッシュした場合など、ロックがタイムアウトになる前にアプリケーションがメッセージの処理に失敗した場合は、Service Bus によってメッセージのロックが解除され、再度受信できるようになります。

前に、メッセージを処理した後、アプリケーションがクラッシュすることを注意してください、 [完了](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) 要求が発行される、再起動する際に、アプリケーションにメッセージが再配信されます。 一般的に、この動作は "*1 回以上*" の処理と呼ばれます。つまり、各メッセージは 1 回以上処理されますが、 特定の状況では、同じメッセージが再配信される可能性があります。 重複した処理を許容できないシナリオでは、メッセージの **MessageId** プロパティに基づいて実現可能な重複を検出するための追加のロジックがアプリケーションで必要になります。このプロパティはメッセージが再配信されても変わりません。 これは "*厳密に 1 回*" の処理と呼ばれます。

詳細については、作成し、およびメッセージを送信する方法の実例キューを参照してください、 [サービス バスの仲介型メッセージングに関する .NET チュートリアル](https://msdn.microsoft.com/library/azure/hh367512.aspx)します。

## トピックとサブスクリプション

各メッセージが 1 つのコンシューマーによって処理されるキューとは異なり、トピックとサブスクリプションでは、*パブリッシュ/サブスクライブ* パターンを使用した 1 対多形式の通信が行われます。 パブリッシュされた各メッセージは、これは非常に多くの受信者を対象とする場合に便利であり、トピックに登録している各サブスクリプションで使用できます。 メッセージは、トピックに送信された後、サブスクリプションごとに設定できるフィルター規則に基づいて、関連付けられている 1 つ以上のサブスクリプションに配信されます。 サブスクリプションでは、追加のフィルターを使用して、受信するメッセージを限定できます。 メッセージは、キューに送信される場合と同じようにトピックに送信されますが、トピックからメッセージを直接受信することはありません。 代わりに、メッセージはサブスクリプションから受信します。 トピック サブスクリプションは、トピックに送信されたメッセージのコピーを受け取る仮想キューのようなものです。 メッセージは、キューから受信する場合と同じ方法でサブスクリプションから受信します。

比較として、キューのメッセージ送信機能はそのままトピックに相当し、メッセージ受信機能はサブスクリプションに相当します。 特に、これは、サブスクリプションでは、競合コンシューマー、一時的な切り離し、負荷平滑化、負荷分散など、キューに関してこのセクションで既に説明したのと同じパターンがサポートされることを意味します。

トピックの作成は、前のセクションの例で示したキューの作成と似ています。 サービスの URI を作成し、使用、 [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) 名前空間クライアントを作成するクラス。 使用してトピックを作成することができますし、 [CreateTopic](https://msdn.microsoft.com/library/azure/hh293080.aspx) メソッドです。 次に例を示します。

```
TopicDescription dataCollectionTopic = namespaceClient.CreateTopic("DataCollectionTopic");
```

次に、必要に応じてサブスクリプションを追加します。

```
SubscriptionDescription myAgentSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Inventory");
SubscriptionDescription myAuditSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Dashboard");
```

その後、トピック クライアントを作成できます。 次に例を示します。

```
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider);
TopicClient myTopicClient = factory.CreateTopicClient(myTopic.Path)
```

前のセクションで示したように、メッセージの送信者を使用すると、トピックとの間でメッセージを送受信できます。 次に例を示します。

```
foreach (BrokeredMessage message in messageList)
{
    myTopicClient.Send(message);
    Console.WriteLine(
    string.Format("Message sent: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

キューと同様に、受信するサブスクリプションを使用してから、 [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) オブジェクトの代わりに、 [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) オブジェクトです。 サブスクリプション クライアントを作成し、トピックの名前とサブスクリプションの名前、さらに必要に応じて受信モードをパラメーターとして渡します。 たとえば、**Inventory** サブスクリプションでは、次のようになります。

```
// Create the subscription client
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider); 

SubscriptionClient agentSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Inventory", ReceiveMode.PeekLock);
SubscriptionClient auditSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Dashboard", ReceiveMode.ReceiveAndDelete); 

while ((message = agentSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Inventory...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
    message.Complete();
}          

// Create a receiver using ReceiveAndDelete mode
while ((message = auditSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Dashboard...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

### ルールとアクション

多くのシナリオでは、特性のあるメッセージは、異なる方法で処理する必要があります。 これを実現するために、目的のプロパティを持つメッセージを検索し、該当するプロパティに特定の変更を行うようにサブスクリプションを構成できます。 Service Bus のサブスクリプションには、トピックに送信されたすべてのメッセージが表示されますが、仮想サブスクリプション キューにコピーできるのは、これらのメッセージの一部のみです。 これを行うには、サブスクリプション フィルターを使用します。 このような変更は、*フィルター アクション*と呼ばれます。 サブスクリプションを作成する場合、メッセージのシステム プロパティ (**Label** など) とカスタム アプリケーション プロパティ (**StoreName** など) の両方で機能するフィルター式を指定できます。 この場合、SQL フィルター式は省略可能です。SQL フィルター式を指定しない場合は、サブスクリプションで定義されている任意のフィルター アクションが、そのサブスクリプションのすべてのメッセージに対して実行されます。

前の例を使用して、**Store1** からの受信メッセージだけを取り出すようにフィルターするには、Dashboard サブスクリプションを次のように作成します。

```
namespaceManager.CreateSubscription("IssueTrackingTopic", "Dashboard", new SqlFilter("StoreName = 'Store1'"));
```

このサブスクリプション フィルターを適用されているメッセージだけで、 `StoreName` プロパティに設定 `Store1` の仮想キューにコピーする、 `ダッシュ ボード` サブスクリプションです。

使用できるフィルター値の詳細については、ドキュメントを参照して、 [SqlFilter](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx) と [SqlRuleAction](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlruleaction.aspx) クラスです。 参照してください、 [仲介型メッセージング: 高度なフィルター](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749) サンプルです。

## Event Hubs

[Event Hubs](http://azure.microsoft.com/services/event-hubs/) イベント取り込みサービス、Azure に大きなスケールで低待機時間と高い信頼性イベントとテレメトリ受信を指定するために使用されます。 このサービスは、他のダウンストリーム サービスと共に使用した場合、特に、アプリケーションのインストルメンテーション、ユーザー エクスペリエンスやワークフローの処理、モノのインターネット(IoT) などのシナリオで役立ちます。

イベント ハブは、メッセージ ストリーミングの構造体であり、キューやトピックに似ているように見られる場合もありますが、実際には非常に異なる特性を持っています。 たとえば、イベント ハブには、メッセージの TTL、配信不能、トランザクション、受信確認の機能がありません。これらは、従来の仲介型メッセージング機能であり、ストリーミング機能ではないためです。 イベント ハブには、パーティション分割、順序の保持、ストリーム再生など、ストリーム関連のその他の機能が用意されています。

## 次のステップ

Service Bus の仲介型メッセージング エンティティの使用の詳細と例については、次の高度なトピックを参照してください。

- [Service Bus メッセージングの概要](service-bus-messaging-overview.md)
- [サービス バスの仲介型メッセージングに関する .NET チュートリアル](service-bus-brokered-tutorial-dotnet.md)
- [サービス バスの仲介型メッセージングの REST チュートリアル](service-bus-brokered-tutorial-rest.md)
- [Event Hubs 開発者ガイド](../event-hubs/event-hubs-programming-guide.md)
- [高度なフィルターを仲介型メッセージング。](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749)






