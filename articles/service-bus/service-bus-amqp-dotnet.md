<properties 
   pageTitle="AMQP 1.0 での Service Bus と .NET | Microsoft Azure"
   description="AMQP で .NET から Service Bus を使用します。"
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
   ms.date="10/15/2015"
   ms.author="sethm" />


# AMQP 1.0 で .NET から Service Bus を使用する

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

## Service Bus SDK のダウンロード

AMQP 1.0 は、Service Bus SDK Version 2.1 以降でサポートされています。 最新の SDK をダウンロードする [NuGet][]します。

## AMQP 1.0 を使用するように .NET アプリケーションを構成する

既定では、Service Bus .NET クライアント ライブラリは SOAP ベースの専用プロトコルを使ってサービス バス サービスと通信します。 既定のプロトコルの代わりに AMQP 1.0 を使用するには、次のセクションの説明に従って、Service Bus 接続文字列を明示的に構成する必要があります。 AMQP 1.0 を使用する際、アプリケーション コードはこの変更以外には基本的に変更されません。

現在のリリースでは、AMQP を使用する際にサポートされていない API 機能がいくつかあります。 サポートされない機能は、セクションの後半に示す [機能、制限、および動作の違いがサポートされていない](#unsupported-features-restrictions-and-behavioral-differences)します。 そのほか、AMQP を使用すると意味が変わる詳細な構成設定もいくつかあります。

### App.config を使用した構成

アプリケーションの設定は、App.config 構成ファイルを使って保存することをお勧めします。 Service Bus アプリケーションの App.config を使ってサービス バスの設定を格納する **ConnectionString** 値。 App.config ファイルの例は次のとおりです。

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <appSettings>
            <add key="Microsoft.ServiceBus.ConnectionString"
                 value="Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
        </appSettings>
    </configuration>

`Microsoft.ServiceBus.ConnectionString` 設定の値は、Service Bus への接続を構成するために使用される Service Bus 接続文字列です。 その形式は次のとおりです。

    Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp

ここで `[namespace]` と `SharedAccessKey` から取得した、 [Azure クラシック ポータルの [][]します。 詳細については、次を参照してください。 [Service Bus を使用する方法 [] キューに入れ][]します。

AMQP を使用する場合は、を持つ接続文字列を追加 `です。TransportType = Amqp`します。 この表記により、クライアント ライブラリに対して、AMQP 1.0 を使用して Service Bus に接続するように通知します。

## メッセージのシリアル化

使用する .NET クライアント ライブラリの既定のシリアル化動作は、既定のプロトコルを使用する場合、 [DataContractSerializer][] シリアル化する型、 [BrokeredMessage:operator[]][] クライアント ライブラリと Service Bus サービスの間のトランスポートのインスタンス。 クライアント ライブラリがシリアル化するために AMQP 型システムを使用して AMQP トランスポート モードを使用する場合、 [仲介型メッセージ ][brokeredmessage] AMQP メッセージにします。 このシリアル化によって、別のプラットフォームで実行されている可能性のある受信側アプリケーション (JMS API を使用して Service Bus にアクセスする Java アプリケーションなど) で、メッセージを受信して解釈できるようになります。

構築する場合、 [BrokeredMessage:operator[]][] インスタンス、メッセージの本文として機能するコンス トラクターのパラメーターとして .NET オブジェクトを指定することができます。 AMQP プリミティブ型にマップできるオブジェクトの場合、本文は AMQP データ型にシリアル化されます。 オブジェクトは、AMQP プリミティブ型に直接マップできない場合つまり、カスタム型は、アプリケーションで定義されている、オブジェクトがシリアル化を使用して、 [DataContractSerializer][], 、され AMQP データ メッセージにシリアル化されたバイトが送信されます。

.NET 以外のクライアントとの相互運用性を高めるには、メッセージ本文の AMQP 型に直接シリアル化できる .NET 型のみを使用してください。 次の表は、それらの型と、対応する AMQP 型システムへのマッピングを示します。

| .NET 本文のオブジェクト型| 対応する AMQP の型| AMQP 本文セクションの型|
|--------------------------------|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| bool| boolean| AMQP 値|
| byte| ubyte| AMQP 値|
| ushort| ushort| AMQP 値|
| uint| uint| AMQP 値|
| ulong| ulong| AMQP 値|
| sbyte| byte| AMQP 値|
| short| short| AMQP 値|
| int| int| AMQP 値|
| long| long| AMQP 値|
| float| float| AMQP 値|
| double| double| AMQP 値|
| 小数点| decimal128| AMQP 値|
| char| char| AMQP 値|
| DateTime| timestamp| AMQP 値|
| Guid| uuid| AMQP 値|
| byte[]| binary| AMQP 値|
| string| string| AMQP 値|
| System.Collections.IList| list| AMQP 値: コレクションに含まれるアイテムとして指定できるのは、この表で定義されているアイテムのみです。|
| System.Array| array| AMQP 値: コレクションに含まれるアイテムとして指定できるのは、この表で定義されているアイテムのみです。|
| System.Collections.IDictionary| map| AMQP 値: コレクションに含まれるアイテムとして指定できるのは、この表で定義されている項目のみです。注: String キーのみがサポートされます。|
| Uri| 記述子付き string (次の表を参照)| AMQP 値|
| DateTimeOffset| 記述子付き long (次の表を参照)| AMQP 値|
| TimeSpan| 記述子付き long (次の表を参照)| AMQP 値|
| Stream| binary| AMQP データ (複数の場合あり)。データ セクションには、Stream オブジェクトから読み取られた未加工のバイトが格納されます。|
| その他のオブジェクト| binary| AMQP データ (複数の場合あり)。DataContractSerializer またはアプリケーションから提供されるシリアライザーを使用するオブジェクトのシリアル化されたバイナリが格納されます。|

| .NET 型| 対応する AMQP の記述子付き型| メモ|
|----------------|-------------------------------------------------------------------------------------------------------------------------|-------------------------|
| Uri| `< 型名"uri"クラスを = 制限付きのソースの = ="string">< 記述子 name="com.microsoft:uri"/></型 >`| Uri.AbsoluteUri|
| DateTimeOffset| `< 型名 =「datetime オフセット」クラス制限付きのソースの = ="long">< 記述子名前 ="com.microsoft:datetime-offset"/></型 >`| DateTimeOffset.UtcTicks|
| TimeSpan| `< 型名"timespan"クラスを = 制限付きのソースの = ="long">< 記述子 name="com.microsoft:timespan"/></型 > `| TimeSpan.Ticks|

## サポートされていない機能、制限、および動作の違い

AMQP を使用する場合、Service Bus .NET API の次の機能は、現在サポートされていません。

-   トランザクション。

-   転送先を経由した送信。

-   メッセージ シーケンス番号による受信。

-   メッセージとセッションの参照。

-   セッションの状態。

-   バッチベースの API。

-   受信のスケールアウト。

-   サブスクリプション ルールの実行時の操作。

-   セッション ロックの更新。

現在、AMQP を使用する際にサポートされていない API を具体的に以下に示します。

- [Microsoft.ServiceBus.Messaging.MessagingFactory.AcceptMessageSession][]
- [Microsoft.ServiceBus.Messaging.MessagingFactory.CreateMessageSender(System.String,System.String)][]

- [Microsoft.ServiceBus.Messaging.MessageSender.SendBatch(System.Collections.Generic.IEnumerable{Microsoft.ServiceBus.Messaging.BrokeredMessage})][]

- [Microsoft.ServiceBus.Messaging.MessageReceiver.Receive(System.Int64)][]
- [Microsoft.ServiceBus.Messaging.MessageReceiver.ReceiveBatch][]
- [Microsoft.ServiceBus.Messaging.MessageReceiver.CompleteBatch(System.Collections.Generic.IEnumerable{System.Guid})][]
- [Microsoft.ServiceBus.Messaging.MessageReceiver.Peek][]
- [Microsoft.ServiceBus.Messaging.MessageReceiver.PeekBatch][]

- [Microsoft.ServiceBus.Messaging.QueueClient.Peek][]
- [Microsoft.ServiceBus.Messaging.QueueClient.PeekBatch][]

- [Microsoft.ServiceBus.Messaging.TopicClient.SendBatch(System.Collections.Generic.IEnumerable{Microsoft.ServiceBus.Messaging.BrokeredMessage})][]

- [Microsoft.ServiceBus.Messaging.SubscriptionClient.Receive(System.Int64)][]
- [Microsoft.ServiceBus.Messaging.SubscriptionClient.ReceiveBatch][]
- [Microsoft.ServiceBus.Messaging.SubscriptionClient.CompleteBatch(System.Collections.Generic.IEnumerable{System.Guid})][]
- [Microsoft.ServiceBus.Messaging.SubscriptionClient.Peek][]
- [Microsoft.ServiceBus.Messaging.SubscriptionClient.PeekBatch][]
- [Microsoft.ServiceBus.Messaging.SubscriptionClient.AddRule][]
- [Microsoft.ServiceBus.Messaging.SubscriptionClient.RemoveRule(System.String)][]

- [Microsoft.ServiceBus.Messaging.MessageSession.GetState][]
- [Microsoft.ServiceBus.Messaging.MessageSession.SetState(System.IO.Stream)][]
- [Microsoft.ServiceBus.Messaging.MessageSession.RenewLock][]

- [Microsoft.ServiceBus.Messaging.BrokeredMessage.RenewLock][]

また、Service Bus .NET API の動作は、AMQP を使用する場合と既定のプロトコルを使用する場合では以下のようないくつかの細かい違いがあります。

-   [OperationTimeout:operator[]][] プロパティは無視されます。

-   `MessageReceiver.Receive(TimeSpan.Zero)` として実装 `MessageReceiver.Receive(TimeSpan.FromSeconds(10))`します。

## AMQP プロトコル設定を制御する

.NET API では、AMQP プロトコルの動作を制御するいくつかの設定が公開されています。

-   **MessageReceiver.PrefetchCount**: リンクに適用する初期のクレジットを制御します。 既定値は 0 です。

-   **MessagingFactorySettings.AmqpTransportSettings.MaxFrameSize**: 接続オープン時のネゴシエーションで提供される最大 AMQP フレーム サイズを制御します。 既定値は 65,536 バイトです。

-   **MessagingFactorySettings.AmqpTransportSettings.BatchFlushInterval**: 転送がバッチ可能である場合、この値でディスポジションを送信する場合の最大遅延が決まります。 既定では、送信側/受信側によって継承されます。 個々の送信側/受信側は、既定値 (20 ミリ秒) を上書きできます。

-   **MessagingFactorySettings.AmqpTransportSettings.UseSslStreamSecurity**: AMQP 接続を SSL 接続で確立するかどうかを制御します。 既定値は **true** です。

## 次のステップ

さらに詳しい情報については、 次のリンク先を参照してください。

- [Service Bus AMQP の概要]
- [Service Bus の AMQP 1.0 サポートは、キューおよびトピックにパーティション分割]
- [Service Bus for Windows Server での AMQP]


[how to use service bus queues]: service-bus-dotnet-how-to-use-queues.md 
[datacontractserializer]: https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx 
[brokeredmessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx 
[microsoft.servicebus.messaging.messagingfactory.acceptmessagesession]: https://msdn.microsoft.com/library/azure/jj657638.aspx 
[microsoft.servicebus.messaging.messagingfactory.createmessagesender(system.string,system.string)]: https://msdn.microsoft.com/library/azure/jj657703.aspx 
[microsoft.servicebus.messaging.messagesender.sendbatch(system.collections.generic.ienumerable{microsoft.servicebus.messaging.brokeredmessage})]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.sendbatch.aspx 
[microsoft.servicebus.messaging.messagereceiver.receive(system.int64)]: https://msdn.microsoft.com/library/azure/hh322665.aspx 
[microsoft.servicebus.messaging.messagereceiver.receivebatch]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.receivebatch.aspx 
[microsoft.servicebus.messaging.messagereceiver.completebatch(system.collections.generic.ienumerable{system.guid})]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.completebatch.aspx 
[microsoft.servicebus.messaging.messagereceiver.peek]: https://msdn.microsoft.com/library/azure/jj908731.aspx 
[microsoft.servicebus.messaging.messagereceiver.peekbatch]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.peekbatch.aspx 
[microsoft.servicebus.messaging.queueclient.peek]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peek.aspx 
[microsoft.servicebus.messaging.queueclient.peekbatch]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peekbatch.aspx 
[microsoft.servicebus.messaging.topicclient.sendbatch(system.collections.generic.ienumerable{microsoft.servicebus.messaging.brokeredmessage})]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.sendbatch.aspx 
[microsoft.servicebus.messaging.subscriptionclient.receive(system.int64)]: https://msdn.microsoft.com/library/azure/hh293110.aspx 
[microsoft.servicebus.messaging.subscriptionclient.receivebatch]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.receivebatch.aspx 
[microsoft.servicebus.messaging.subscriptionclient.completebatch(system.collections.generic.ienumerable{system.guid})]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.completebatch.aspx 
[microsoft.servicebus.messaging.subscriptionclient.peek]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.peek.aspx 
[microsoft.servicebus.messaging.subscriptionclient.peekbatch]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.peekbatch.aspx 
[microsoft.servicebus.messaging.subscriptionclient.addrule]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.addrule.aspx 
[microsoft.servicebus.messaging.subscriptionclient.removerule(system.string)]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.removerule.aspx 
[microsoft.servicebus.messaging.messagesession.getstate]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.getstate.aspx 
[microsoft.servicebus.messaging.messagesession.setstate(system.io.stream)]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.setstate.aspx 
[microsoft.servicebus.messaging.messagesession.renewlock]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.renewlock.aspx 
[microsoft.servicebus.messaging.brokeredmessage.renewlock]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx 
[operationtimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx 
[nuget]: http://nuget.org/packages/WindowsAzure.ServiceBus/ 
[azure classic portal]: http://manage.windowsazure.com 
[service bus amqp overview]: service-bus-amqp-overview.md 
[amqp 1.0 support for service bus partitioned queues and topics]: service-bus-partitioned-queues-and-topics-amqp-overview.md 
[amqp in service bus for windows server]: https://msdn.microsoft.com/library/dn574799.aspx 

