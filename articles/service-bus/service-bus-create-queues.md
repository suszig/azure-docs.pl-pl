<properties 
   pageTitle="Service Bus キューを使用するアプリケーションを作成する | Microsoft Azure"
   description="Service Bus を使用する簡単なキュー ベースのアプリケーションを作成する方法。"
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
   ms.date="09/16/2015"
   ms.author="sethm" />


# Service Bus キューを使用するアプリケーションを作成する

このトピックでは、Service Bus キューについて説明し、Service Bus を使用する簡単なキュー ベースのアプリケーションを作成する方法を示します。

個々 の Point of Sale (POS) 端末から売上データを在庫管理システムにルーティングし、在庫管理システムではそのデータを使用して在庫を補充する時期を決定する、小売業界のシナリオについて検討します。 このソリューションでは、次の図に示すように、端末と在庫管理システムの間の通信に Service Bus メッセージングを使用します。

![Service-Bus-Queues-Img1](./media/service-bus-create-queues/IC657161.gif)

各 POS 端末は、**DataCollectionQueue** にメッセージを送信することによって、売上データを報告します。 これらのメッセージは、在庫管理システムによって取り出されるまでこのキューに残っています。 POS 端末は処理を続行するために在庫管理システムからの応答を待機する必要がないため、このパターンは*非同期メッセージング*と呼ばれることがよくあります。

## キューを使用する理由

このアプリケーションを設定するために必要なコードを見る前に、POS 端末が在庫管理システムと直接 (同期的に) 対話するのではなく、キューをこのシナリオで使用することの利点について検討します。

### 一時的な結合の解除

非同期メッセージング パターンでは、プロデューサーとコンシューマーが同時にオンラインになっている必要はありません。 このメッセージング インフラストラクチャは、コンシューマーがメッセージを受信する準備ができるまで、メッセージを確実に格納します。 これにより、分散型アプリケーションのコンポーネントをメンテナンスやコンポーネント クラッシュの場合でもシステム全体に影響を与えずに自動的に切断できます。 さらに、コンシューマー アプリケーションがオンラインになっている必要がある時間は、1 日のうち一定の時間だけで済みます。 たとえば、この小売業のシナリオでは、在庫管理システムは営業時間の終了後にオンラインになるだけで済むかもしれません。

### 負荷平準化

多くのアプリケーションでは、システム負荷が時間の経過とともに変化しますが、各作業単位に必要な処理時間は 通常一定に保たれます。 メッセージ プロデューサーとメッセージ コンシューマーの間をキューで仲介することは、コンシューマー側アプリケーション (worker) はピーク時ではなく平均時の負荷に対応できるようにプロビジョニングすればいいということを意味します。 キューの深さは、受信する負荷の変化に合わせて増減します。 このため、アプリケーション負荷への対応に必要なインフラストラクチャに関して直接費用を節約できます。

![Service-Bus-Queues-Img2](./media/service-bus-create-queues/IC657162.gif)

### 負荷分散

負荷の増大に合わせて、worker キューからの読み取りのために worker プロセスを追加できます。 各メッセージは、ワーカー プロセスの中の 1 つのプロセスによって処理されます。 さらに、このプルベースの負荷分散では、各 worker コンピューターがそれぞれ独自の最大レートでメッセージをプルするため、それらのコンピューターの処理能力が異なる場合であっても使用を最適化できます。 このパターンは、競合コンシューマー パターンと 呼ばれることもあります。

![Service-Bus-Queues-Img3](./media/service-bus-create-queues/IC657163.gif)

### 疎結合

メッセージ キューを使用してメッセージ プロデューサーとメッセージ コンシューマーの間を仲介すると、必然的にコンポーネント間の結び付きは緩くなります。 プロデューサーとコンシューマーは相互に認識しないため、プロデューサーに影響することなく、コンシューマーをアップグレードできます。 さらに、既存のエンドポイントに影響を与えずメッセージング トポロジを拡張できます。 これについては、パブリッシュ/サブスクライブのところでさらに詳しく説明します。

## コード

以下では、Service Bus を使用してこのアプリケーションを作成する方法を示します。

### Service Bus のアカウントとサブスクリプションにサインアップします。

Service Bus の使用を開始するには、Azure アカウントが必要です。 無料評価版にサインアップすることがない場合は既に 1 つ、 [ここ](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A85619ABF)します。

### サービス名前空間の作成

サブスクリプションを作成した後は、新しい名前空間を作成できます。 新しい名前空間には、すべての Service Bus アカウントについて一意の名前を指定する必要があります。 各名前空間は、一連の Service Bus エンティティに対するコンテナーの役割を果たします。 詳細については、次を参照してください。 [How To: 作成または Service Bus Service 名前空間を変更する](https://msdn.microsoft.com/library/azure/hh690931.aspx)します。

### NuGet パッケージのインストール

Service Bus サービス名前空間を使用するのには、アプリケーションが Microsoft.ServiceBus.dll 具体的には、サービス バス アセンブリを参照する必要があります。 Microsoft Azure SDK の一部としてこのアセンブリを検索してでは、ダウンロード、 [Azure SDK のダウンロード ページ](http://azure.microsoft.com/downloads/)します。 ただし、Service Bus API を取得し、Service Bus 依存関係をすべて備えたアプリケーションを構成する最も簡単な方法は、Service Bus NuGet パッケージです。 NuGet および Service Bus パッケージの使用に関する詳細については、「 [NuGet Service Bus パッケージを使用して](https://msdn.microsoft.com/library/dn741354.aspx)します。

### キューの作成

Service Bus メッセージング エンティティ (キューおよびトピック発行/サブスクライブ、トピック) を使用して行いますに対する管理操作を [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) クラスです。 Service Bus を使用して、 [共有アクセス署名 (SAS)](service-bus-sas-overview.md) ベースのセキュリティ モデル。  [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) クラスは、いくつかの既知のトークン プロバイダーを返す組み込みファクトリ メソッドを持つセキュリティ トークン プロバイダーを表します。 使用して、 [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) メソッド SAS 資格情報を保持します。  [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) Service Bus 名前空間とトークン プロバイダーのベース アドレスを持つインスタンスが作成され、します。

[NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) クラスを作成、列挙、およびメッセージング エンティティを削除するメソッドを提供します。 次の番組に示すコードが、どのように [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) インスタンスが作成され、作成するために使用、 **DataCollectionQueue** キューです。

```
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", 
                "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";

TokenProvider tokenProvider = 
    TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = 
    new NamespaceManager(uri, tokenProvider);
namespaceManager.CreateQueue("DataCollectionQueue");
```

オーバー ロードがあることに注意してください、 [CreateQueue](https://msdn.microsoft.com/library/azure/hh322663.aspx) をチューニングするキューのプロパティを有効にするメソッドです。 たとえば、キューに送信されるメッセージの既定の有効期限 (TTL) の値を設定できます。

### キューへのメッセージの送信

Service Bus のエンティティに対する操作の実行時たとえば、メッセージを送受信するには、アプリケーション作成する必要があります最初、 [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) オブジェクトです。 ような [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) 、クラス、 [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) サービス名前空間とトークン プロバイダーのベース アドレスからインスタンスを作成します。

```
 BrokeredMessage bm = new BrokeredMessage(salesData);
 bm.Label = "SalesReport";
 bm.Properties["StoreName"] = "Redmond";
 bm.Properties["MachineID"] = "POS_1";
```

メッセージに送信され、Service Bus キューから受信したのインスタンス、 [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) クラスです。 このクラスは、標準的なプロパティ セットで構成されます (よう [ラベル](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) と [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), アプリケーション プロパティの保持に使用するディクショナリ、および任意のアプリケーション データの本体。 アプリケーションは任意のシリアル化可能なオブジェクトを渡すことで本文を設定することができます (で次の例では、 **SalesData** POS ターミナルからの売上データを表すオブジェクト) を使用する、 [DataContractSerializer](https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx) 、オブジェクトをシリアル化します。 または、 [ストリーム](https://msdn.microsoft.com/library/azure/system.io.stream.aspx) オブジェクトを指定することができます。

この例では、特定のキューにメッセージを送信する最も簡単な方法、 **DataCollectionQueue**, を使用して [CreateMessageSender](https://msdn.microsoft.com/library/azure/hh322659.aspx) を作成する、 [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) オブジェクトから直接、 [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) インスタンス。

```
MessageSender sender = factory.CreateMessageSender("DataCollectionQueue");
sender.Send(bm);
```

### キューからのメッセージの受信

キューからメッセージを受信する最も簡単な方法が使用するには、 [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) から直接作成できるオブジェクトを [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) を使用して [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx)します。 メッセージ レシーバーは、**ReceiveAndDelete** と **PeekLock** という 2 つの異なるモードで動作できます。  [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) へのパラメーターとしてメッセージの受信者の作成時に設定されている、 [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx) 呼び出します。


**ReceiveAndDelete** モードでは、受信は単発の操作です。つまり、Service Bus は要求を受信すると、メッセージを読み取り中としてマークしてアプリケーションに返します。 **ReceiveAndDelete** モードは最もシンプルなモデルであり、障害が発生した場合にアプリケーション側でメッセージを処理しないことを許容できるシナリオに最適です。 このことを理解するために、コンシューマーが受信要求を発行した後で、メッセージを処理する前にクラッシュしたというシナリオを考えてみましょう。 Service Bus がメッセージを読み取り中としてマークするため、アプリケーションは、再起動してメッセージの読み取りを再開すると、クラッシュ前に読み取られていたメッセージを見落とすことになります。

**PeekLock** モードでは、メッセージの受信処理が 2 段階の動作になり、メッセージが失われることが許容できないアプリケーションに対応することができます。 Service Bus は要求を受け取ると、次に読み取られるメッセージを検索して、他のコンシューマーが受信できないようロックしてから、アプリケーションにメッセージを返します。 呼び出して受信処理の第 2 段階を完了したら、アプリケーションがメッセージの処理 (または後で処理するために確実に保存) [完了](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) を完了します。 Service Bus が確認、 [完了](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) 呼び出し、メッセージを読み取り中としてマークします。

結果としては他に 2 つの可能性があります。 最初に、アプリケーションが何らかの理由のメッセージを処理できない場合は、呼び出すことができます [破棄](https://msdn.microsoft.com/library/azure/hh181837.aspx) 受信したメッセージに対して (の代わりに [完了](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx))します。 このメソッドが呼び出されると、Service Bus によってメッセージのロックが解除され、同じコンシューマーまたは競合する別のコンシューマーが再度そのメッセージを受信できるようになります。 第 2 に、ロックに関連付けられたタイムアウトがあります。アプリケーションがクラッシュした場合など、ロックがタイムアウトになる前にアプリケーションがメッセージの処理できない場合は、Service Bus によってメッセージのロックが解除され、再度受信できるようになります。

注の後に、アプリケーションがクラッシュした場合、メッセージを処理する、その前に、 [完了](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) 要求が発行されて、再起動する際にそのメッセージがアプリケーションに再配信されます。 一般に、この動作は "1 回以上" の処理と呼ばれます。 つまり、すべてのメッセージが 1 回以上処理されますが、特定の状況では、同じメッセージが再配信される可能性があります。 重複した処理を許されないシナリオの場合は、アプリケーションで重複を検出する追加ロジックが必要です。 これを行うに基づいて、 [MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) 、メッセージのプロパティです。 このプロパティの値は、配信の試行を通じて変化しません。 これは "*厳密に 1 回*" の処理と呼ばれます。

ここで示すようにコードを受信してメッセージを使用して、処理、 **PeekLock** モードではない場合の既定 [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) 値が明示的に指定します。

```
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionQueue");
BrokeredMessage receivedMessage = receiver.Receive();
try
{
    ProcessMessage(receivedMessage);
    receivedMessage.Complete();
}
catch (Exception e)
{
    receivedMessage.Abandon();
}
```

### キュー クライアントの使用

作成したこのセクションの前半の例は、 [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) と [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) オブジェクトから直接、 [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) をそれぞれ、キューからメッセージを送受信します。 別の方法が使用する、 [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) クラスをサポートする送信および受信セッションなどのより高度な機能に加えて操作します。

```
QueueClient queueClient = factory.CreateQueueClient("DataCollectionQueue");
queueClient.Send(bm);

BrokeredMessage message = queueClient.Receive();

try
{
    ProcessMessage(message);
    message.Complete();
}
catch (Exception e)
{
    message.Abandon();
} 
```

## 次のステップ

これをキューの基本を学習できました。、を参照してください。 [Service Bus のトピックとサブスクリプションを使用するアプリケーションを作成](service-bus-create-topics-subscriptions.md) メッセージングが Service Bus の機能はパブリッシュ/サブスクライブを使用してこの議論を継続するために仲介します。




