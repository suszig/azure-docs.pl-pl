<properties 
   pageTitle="Service Bus のトピックとサブスクリプションを使用するアプリケーションを作成する | Microsoft Azure"
   description="Service Bus のトピックとサブスクリプションによって提供されるパブリッシュ/サブスクライブ機能について説明します。"
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


# Service Bus のトピックとサブスクリプションを使用するアプリケーションを作成する

Azure Service Bus は、信頼性の高いメッセージ キュー機能や永続的なパブリッシュ/サブスクライブ メッセージング機能など、クラウドベースのメッセージ指向ミドルウェアの一連のテクノロジをサポートしています。 この記事で提供される情報に基づき [Service Bus キューを使用してアプリケーションを作成](service-bus-create-queues.md) Service Bus のトピックで提供されるパブリッシュ/サブスクライブ機能を紹介します。

## 小売りシナリオの発展

この記事で使用される小売り業のシナリオを引き続き [Service Bus キューを使用してアプリケーションを作成](service-bus-create-queues.md)します。 個々 の Point of Sale (POS) 端末から売上データを在庫管理システムにルーティングし、在庫管理システムではそのデータを使用して在庫を補充する時期を決定するというシナリオです。 各 POS 端末は、**DataCollectionQueue** キューにメッセージを送信することによって売上データを報告します。次の図で示すように、メッセージは在庫管理システムが受け取るまでキューに留まっています。

![Service-Bus1](./media/service-bus-create-topics-subscriptions/IC657161.gif)

このシナリオを発展させるため、システムに新しい要件を追加します。つまり、店の所有者は、店の売上状況をリアルタイムで監視することを望んでいます。

この要件に対応するには、システムは売上データ ストリームを取り出す必要があります。 POS 端末によって送信された各メッセージはこれまでと同じように在庫管理システムに送信しますが、各メッセージのコピーを作成し、店の所有者にダッシュボード ビューを提供するために使用できるようにします。

Service Bus を使用する各メッセージを複数の当事者が使用する必要がある次のような状況で *トピック*します。 トピックはパブリッシュ/サブスクライブ パターンを提供し、発行された各メッセージをトピックに登録されている 1 つ以上のサブスクリプションで使用します。 これに対し、キューでは、各メッセージは 1 つのコンシューマーによって受信されます。

メッセージは、キューに送信される場合と同じ方法でトピックに送信されます。 ただし、メッセージをトピックから直接受信することはなく、サブスクリプションから受信します。 トピックにとってのサブスクリプションは、そのトピックに送信されたメッセージのコピーを受け取る仮想キューと考えることができます。 メッセージは、キューから受信する場合と同じ方法でサブスクリプションから受信します。

小売業のシナリオに戻り、キューをトピックに置き換えて、在庫管理システム コンポーネントによって使用されるサブスクリプションを追加します。 システムは次のようになります。

![Service-Bus2](./media/service-bus-create-topics-subscriptions/IC657165.gif)

ここでの構成は、前のキュー ベースの設計と同じように動作します。 つまり、トピックに送信されたメッセージは **Inventory** サブスクリプションにルーティングされ、そこから**在庫管理システム**がメッセージを使用します。

管理ダッシュボードをサポートするため、次に示すようにトピックに第 2 のサブスクリプションを作成します。

![Service-Bus3](./media/service-bus-create-topics-subscriptions/IC657166.gif)

この構成では、POS 端末からの各メッセージは、**Dashboard** サブスクリプションと **Inventory** サブスクリプションの両方で使用できます。

## コード

[Service Bus キューを使用してアプリケーションを作成](service-bus-create-queues.md) 説明方法、アカウントにサインアップする Service Bus には、およびサービス名前空間を作成します。 Service Bus 名前空間を使用するのには、アプリケーションが Microsoft.ServiceBus.dll 具体的には、サービス バス アセンブリを参照する必要があります。 Service Bus 依存関係を参照する最も簡単な方法は、Service Bus のインストールする [Nuget パッケージ](https://www.nuget.org/packages/WindowsAzure.ServiceBus/)します。 また、アセンブリは Azure SDK にも含まれます。 ダウンロード、 [Azure SDK のダウンロード ページ](http://azure.microsoft.com/downloads/)します。

### トピックとサブスクリプションを作成する

Service Bus メッセージング エンティティ (キューおよびトピック発行/サブスクライブ、トピック) を使用して行いますに対する管理操作を [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) クラスです。 適切な資格情報を作成するために必要な [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) 特定の名前空間用のインスタンス。 Service Bus を使用して、 [共有アクセス署名 (SAS)](service-bus-sas-overview.md) ベースのセキュリティ モデル。  [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) クラスは、いくつかの既知のトークン プロバイダーを返す組み込みファクトリ メソッドを持つセキュリティ トークン プロバイダーを表します。 使用して、 [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) メソッド SAS 資格情報を保持します。  [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) Service Bus 名前空間とトークン プロバイダーのベース アドレスを持つインスタンスが作成され、します。

[NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) クラスを作成、列挙、およびメッセージング エンティティを削除するメソッドを提供します。 次の番組に示すコード方法 [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) インスタンスが作成され、作成に使用される、 **DataCollectionTopic** トピックです。

```
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";

TokenProvider tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = new NamespaceManager(uri, tokenProvider);

namespaceManager.CreateTopic("DataCollectionTopic");
```

オーバー ロードがあることに注意してください、 [CreateTopic](https://msdn.microsoft.com/library/azure/hh293080.aspx) 、トピックのプロパティを設定するためのメソッドです。 たとえば、トピックに送信されるメッセージの既定の有効期限 (TTL) の値を設定できます。 次に、the **Inventory** サブスクリプションと **Dashboard** サブスクリプションを追加します。

```
namespaceManager.CreateSubscription("DataCollectionTopic", "Inventory");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard");
```

### トピックにメッセージを送信する

Service Bus のエンティティに対する操作の実行時たとえば、メッセージを送受信するには、アプリケーション作成する必要があります最初、 [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) オブジェクトです。 ような [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) 、クラス、 [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) サービス名前空間とトークン プロバイダーのベース アドレスからインスタンスを作成します。

```
MessagingFactory factory = MessagingFactory.Create(uri, tokenProvider);
```

メッセージに送信され、Service Bus のトピックから受け取るのインスタンス、 [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) クラスです。 このクラスは、標準的なプロパティ セットで構成されます (よう [ラベル](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) と [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), アプリケーション プロパティの保持に使用するディクショナリ、および任意のアプリケーション データの本体。 アプリケーションは任意のシリアル化可能なオブジェクトを渡すことで本文を設定することができます (で次の例では、 **SalesData** POS ターミナルからの売上データを表すオブジェクト) を使用する、 [DataContractSerializer](https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx) 、オブジェクトをシリアル化します。 または、 [ストリーム](https://msdn.microsoft.com/library/azure/system.io.stream.aspx) オブジェクトを指定することができます。

```
BrokeredMessage bm = new BrokeredMessage(salesData);
bm.Label = "SalesReport";
bm.Properties["StoreName"] = "Redmond";
bm.Properties["MachineID"] = "POS_1";
```

使用するメッセージをトピックに送信する最も簡単な方法は、 [CreateMessageSender](https://msdn.microsoft.com/library/azure/hh322659.aspx) を作成する、 [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) オブジェクトから直接、 [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) インスタンス。

```
MessageSender sender = factory.CreateMessageSender("DataCollectionQueue");
sender.Send(bm);
```

### サブスクリプションからメッセージを受信する

使用するサブスクリプションからメッセージを受信するキューを使用するような [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) から直接作成するオブジェクトを [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) を使用して [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx)します。 2 つの異なるのいずれかを使用する受信モード (**ReceiveAndDelete** と **PeekLock**) で説明したように、 [Service Bus キューを使用してアプリケーションを作成](service-bus-create-queues.md)します。

作成するときに注意してください、 [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) サブスクリプションの場合、 *entityPath* の形式は、パラメーター `topicPath/サブスクリプション/subscriptionName`します。 そのため、作成する、 [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) の **インベントリ** のサブスクリプション、 **DataCollectionTopic** トピック、 *entityPath* に設定する必要があります `DataCollectionTopic/サブスクリプション/インベントリ`します。 コードは次のようになります。

```
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionTopic/subscriptions/Inventory");
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

## サブスクリプション フィルター

ここまで、トピックに送信されるすべてのメッセージは、登録されているすべてのサブスクリプションで使用できます。 ここでの重要なフレーズは "使用可能にする" です。 Service Bus のサブスクリプションにはトピックに送信されたすべてのメッセージが含まれますが、これらのメッセージの一部のみを仮想サブスクリプション キューにコピーできます。 これを行うには、サブスクリプション *フィルター*を使用します。 サブスクリプションを作成するときは、システム プロパティ、メッセージのプロパティに対して動作する SQL92 スタイルの述語の形式でフィルター式を指定できます (たとえば、 [ラベル](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx)) およびアプリケーションのプロパティなど **StoreName** 前の例では。

これを示すためにシナリオを発展させ、2 番目の店を小売りシナリオに追加します。 両店舗のすべての POS 端末からの売上データを一元的な在庫管理システムにルーティングする必要があるのは同じですが、ダッシュボード ツールを使用する店のマネージャーはその店の実績だけに関心があります。 サブスクリプション フィルターを使用してこれを実現できます。 POS 端末はメッセージを発行するときにメッセージの **StoreName** アプリケーション プロパティを設定することを思い出してください。 たとえば **Redmond** と **Seattle** という 2 つの店があると、Redmond 店の POS 端末は売上データ メッセージの **StoreName** を **Redmond** に設定し、Seattle 店の POS 端末は **StoreName** を **Seattle** に設定します。 Redmond 店のマネージャーは、Redmond 店の POS 端末のデータにだけ関心があります。 システムは次のようになります。

![Service-Bus4](./media/service-bus-create-topics-subscriptions/IC657167.gif)

このルーティングを設定するには、**Dashboard** サブスクリプションを次のように作成します。

```
SqlFilter dashboardFilter = new SqlFilter("StoreName = 'Redmond'");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard", dashboardFilter);
```

このサブスクリプション フィルターでは、 **StoreName** プロパティが **Redmond** に設定されているメッセージだけが、**Dashboard** サブスクリプションの仮想キューにコピーされます。 ただし、サブスクリプション フィルターの機能はこれだけではありません。 アプリケーションでは、サブスクリプションごとに複数のフィルター ルールを使用するだけでなく、メッセージをサブスクリプションの仮想キューに渡すときにメッセージのプロパティを変更できます。

## 概要

すべてのキューの説明を使用する理由 [Service Bus キューを使用してアプリケーションを作成](service-bus-create-queues.md) も具体的には、トピックに適用します。

- 一時的な結合の解除 – メッセージのプロデューサーとコンシューマーは、同時にオンラインである必要はありません。

- 負荷平準化 – 負荷のピークをトピックによって平準化すると、コンシューマーのアプリケーションをピーク負荷ではなく平均的な負荷に合わせてプロビジョニングできます。

- 負荷分散 – キューと同様に、1 つのサブスクリプションをリッスンする複数の競合するコンシューマーを作成し、各メッセージを 1 つのコンシューマーだけに渡すことにより、負荷を分散できます。

- 疎結合 – 既存のエンドポイントに影響を及ぼさずに、メッセージング ネットワークを発展させることができます。たとえば、サブスクリプションを追加したり、新しいコンシューマーに対応するようにトピックのフィルターを変更したりできます。

## 次のステップ

参照してください [Service Bus のトピックとサブスクリプションを使用するアプリケーションを作成](service-bus-create-topics-subscriptions.md) POS 小売り業のシナリオでキューを使用する方法を確認します。




