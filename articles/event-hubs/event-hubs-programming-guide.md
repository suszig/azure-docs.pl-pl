<properties 
   pageTitle="Azure Event Hubs のプログラミング ガイド |Microsoft Azure"
   description="Azure .NET SDK を使用し、Azure Event Hubs でプログラミングする方法について説明します。"
   services="event-hubs"
   documentationCenter="na"
   authors="sethmanheim"
   manager="timlt"
   editor="" />
<tags 
   ms.service="event-hubs"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="09/30/2015"
   ms.author="sethm" />


# Event Hubs のプログラミング ガイド

このトピックでは、Azure .NET SDK を使用し、Azure Event Hubs でプログラミングする方法について説明します。 Event Hubs の予備知識があることを前提としています。 概念、Event Hub の概要を参照してください、 [Event Hubs の概要](event-hubs-overview.md)します。

## イベント発行元

イベントは Event Hub に HTTP POST と AMQP 1.0 接続のいずれかを利用して送信されます。 何をいつ利用するかは、対処される特定のシナリオによって決まります。 AMQP 1.0 接続は Service Bus の仲介型接続として課金され、メッセージ量が常に多く、待ち時間要件の低いシナリオに適しています。固定のメッセージング チャンネルが提供されるためです。

Event Hub が作成され、管理を使用して、 [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) クラスです。 Event Hubs にデータを発行するための基本的な構造が、管理 Api を .NET を使用するときに、 [EventHubClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx) と [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) クラスです。  [EventHubClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx) クラスには、どのイベントが Event Hub に送信される AMQP 通信チャネルが用意されています。  [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) クラスはイベントを表し、Event Hub にメッセージを公開するために使用します。 このクラスには、本文、いくつかのメタデータ、イベントに関するヘッダー情報が含まれます。 他のプロパティには追加、 [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx)オブジェクトの Event Hub に渡されます。

## 作業開始

Event Hubs をサポートする .NET クラスは、Microsoft.ServiceBus.dll アセンブリの一部です。 Service Bus API を参照し、すべての Service Bus 依存関係を備えたアプリケーションを構成する最も簡単な方法は Service Bus NuGet パッケージをダウンロードすることです。 詳細については、次を参照してください。 [NuGet Service Bus パッケージを使用して](https://msdn.microsoft.com/library/azure/dn741354.aspx)します。 また、使用することができます、 [パッケージ マネージャー コンソール](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) Visual Studio でします。 これを行うには、次のコマンドを発行、 [パッケージ マネージャー コンソール](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) ウィンドウ。

```powershell
Install-Package WindowsAzure.ServiceBus
```

## Event Hub を作成する

使用することができます、 [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) クラス Event Hub を作成します。 次に例を示します。

```c
var manager = new Microsoft.ServiceBus.NamespaceManager("mynamespace.servicebus.windows.net");
var description = manager.CreateEventHub("MyEventHub");
```

ほとんどの場合は勧めを使用すること、 [CreateEventHubIfNotExists](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.createeventhubifnotexists.aspx) 、サービスが再起動した場合の例外の発生を防ぐ方法をします。 次に例を示します。

```
var description = manager.CreateEventHubIfNotExists("MyEventHub");
```

など、すべての Event Hub 作成操作 [CreateEventHubIfNotExists](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.createeventhubifnotexists.aspx), 、必要な **管理** 問題の名前空間に対するアクセス許可。 発行元またはコンシューマー アプリケーションのアクセス許可を制限する場合、アクセス許可を制限して資格情報を利用するとき、運用コードでこれらの作成操作呼び出しを回避できます。

[EventHubDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubdescription.aspx) クラスには、承認規則、メッセージの保有期間、パーティション Id、状態、およびパスを含む、Event Hub に関する詳細情報が含まれています。 このクラスを使用し、Event Hub でメタデータを更新できます。

## Event Hub クライアントの作成

Event Hubs とやり取りするためのプライマリ クラスは [Microsoft.ServiceBus.Messaging.EventHubClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx)します。 このクラスは送信機能と受信機能の両方を提供します。 使用してこのクラスをインスタンス化する、 [作成](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.create.aspx) メソッドを次の例に示すようにします。

```
var client = EventHubClient.Create(description.Path);
```

このメソッドで、App.config ファイル内の Service Bus 接続情報を使用して、 `appSettings` セクションです。 例については、 `appSettings` Service Bus の接続情報を格納するために使用する XML ドキュメントを参照して、 [Microsoft.ServiceBus.Messaging.EventHubClient.Create(System.String)](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.create.aspx) メソッドです。

接続文字列からクライアントを作成することもできます。 この選択肢は Azure worker ロールを使用する場合に効果があります。worker の設定プロパティに文字列を格納できるためです。 次に例を示します。

```
EventHubClient.CreateFromConnectionString("your_connection_string");
```

The connection string will be in the same format as it appears in the App.config file for the previous methods:
```
Endpoint=sb://[namespace].servicebus.windows.net/です。SharedAccessKeyName = 管理します。SharedAccessKey = [キー]
```

Finally, it is also possible to create an [EventHubClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx) object from a [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) instance, as shown in the following example.
```
var ファクトリ = MessagingFactory.CreateFromConnectionString("your_connection_string") です。
var client = ファクトリ。CreateEventHubClient("MyEventHub") です。
```

It is important to note that additional [EventHubClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx) objects created from a messaging factory instance will reuse the same underlying TCP connection. Therefore, these objects have a client-side limit on throughput. The [Create](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.create.aspx) method reuses a single messaging factory. If you need very high throughput from a single sender, then you can create multiple message factories and one [EventHubClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx) object from each messaging factory.

## Send events to an Event Hub

You send events to an Event Hub by creating an [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) instance and sending it via the [Send](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.send.aspx) method. This method takes a single [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) instance parameter and synchronously sends it to an Event Hub.

## Event serialization

The [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) class has [four overloaded constructors](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) that take a variety of parameters, such as an object and serializer, a byte array, or a stream. It is also possible to instantiate the [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) class and set the body stream afterwards. When using JSON with [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx), you can use **Encoding.UTF8.GetBytes()** to retrieve the byte array for a JSON-encoded string.

## Partition key

The [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) class has a [PartitionKey](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.partitionkey.aspx) property that enables the sender to specify a value that is hashed to produce a partition assignment. Using a partition key ensures that all the events with the same key are sent to the same partition in the Event Hub. Common partition keys include user session IDs and unique sender IDs. The [PartitionKey](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.partitionkey.aspx) property is optional and can be provided when using the [Microsoft.ServiceBus.Messaging.EventHubClient.Send(Microsoft.ServiceBus.Messaging.EventData)](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) or [Microsoft.ServiceBus.Messaging.EventHubClient.SendAsync(Microsoft.ServiceBus.Messaging.EventData)](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) methods. If you do not provide a value for [PartitionKey](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.partitionkey.aspx), sent events are distributed to partitions using a round-robin model.

## Batch event send operations

Sending events in batches can dramatically increase throughput. The [SendBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.sendbatch.aspx) method takes an **IEnumerable** parameter of type [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) and sends the entire batch as an atomic operation to the Event Hub.
```
public void SendBatch (IEnumerable<EventData> eventDataList) です。
```

It is important to note that a single batch must not exceed the 256 KB limit of an event. Additionally, each message in the batch uses the same publisher identity. It is the responsibility of the sender to ensure that the batch does not exceed the maximum event size. If it does, a client **Send** error is generated.

## Send asynchronously and send at scale

You can also send events to an Event Hub asynchronously. Sending asynchronously can increase the rate at which a client is able to send events. Both the [Send](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.send.aspx) and [SendBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.sendbatch.aspx) methods are available in asynchronous versions that return a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) object. While this technique can increase throughput, it can also cause the client to continue to send events even while it is being throttled by the Event Hubs service and can result in the client experiencing failures or lost messages if not properly implemented. In addition, you can use the [RetryPolicy](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.cliententity.retrypolicy.aspx) property on the client to control client retry options.

## Create a partition sender

Although it is most common to send events to an Event Hub with a partition key, in some cases you might want to send events directly to a given partition. For example:
```
var partitionedSender = クライアントです。CreatePartitionedSender (説明します。PartitionIds[0]) です。
```

[CreatePartitionedSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.createpartitionedsender.aspx) returns an [EventHubSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubsender.aspx) object that you can use to publish events to a specific Event Hub partition.

## Event consumers

Event Hubs has two primary models for event consumption: direct receivers and higher-level abstractions, such as [EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx). Direct receivers are responsible for their own coordination of access to partitions within a consumer group.

### Direct consumer

The most direct way to read from a partition within a consumer group is to use the [EventHubReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubreceiver.aspx) class. To create an instance of this class, you must use an instance of the [EventHubConsumerGroup](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubconsumergroup.aspx) class. In the following example, the partition ID must be specified when creating the receiver for the consumer group.
```
EventHubConsumerGroup グループ = クライアントです。GetDefaultConsumerGroup() です。
var 受信者 = グループです。CreateReceiver (クライアントです。GetRuntimeInformation() します。PartitionIds[0]) です。
```

The [CreateReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubconsumergroup.createreceiver.aspx) method has several overloads that facilitate control over the reader being created. These methods include specifying an offset as either a string or timestamp, and the ability to specify whether to include this specified offset in the returned stream, or start after it. After you create the receiver, you can start receiving events on the returned object. The [Receive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubreceiver.receive.aspx) method has four overloads that control the receive operation parameters, such as batch size and wait time. You can use the asynchronous versions of these methods to increase the throughput of a consumer. For example:
```
bool 受信 = true;
文字列 myOffset です。
while(receive)
{
    var メッセージ受信者を = です。Receive() です。
    myOffset = メッセージです。オフセットです。
    本文を文字列 = Encoding.UTF8.GetString (メッセージです。GetBytes()) です。
    Console.WriteLine (String.Format ("受信メッセージのオフセット: {0} \nbody: \{1\}"、myOffset、本体)) です。
{
```

特定のパーティションについては、メッセージはそれが Event Hub に送信された順序で受信されます。 オフセットはパーティションのメッセージ識別に使用される文字列トークンです。

コンシューマー グループ内の 1 つのパーティションが 5 個の同時読み取りをいつでも接続を持たないに注意してください。 重要です。 リーダーが接続または切断されると、そのセッションは数分間アクティブの状態を維持し、それから切断がサービスにより認識されることがあります。 その間にパーティションに再接続すると、失敗することがあります。 Event Hubs のダイレクト レシーバーの書き込みの完全な例を参照してください、 [Service Bus Event Hubs Direct Receivers](https://code.msdn.microsoft.com/Event-Hub-Direct-Receivers-13fa95c6) サンプルです。

### イベント プロセッサ ホスト

[EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx) クラスは、Event Hub からデータを処理します。 .NET プラットフォームでのイベント リーダーを作成するときに、この実装を使用する必要があります。 [EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx) し、チェックポイントおよびパーティション リース管理も提供するイベント プロセッサ実装のスレッド セーフでマルチ プロセスで安全なランタイム環境を提供します。

使用する、 [EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx) 実装するクラス、 [IEventProcessor](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ieventprocessor.aspx)します。 このインターフェイスには 3 つのメソッドが含まれています。

- [OpenAsync](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ieventprocessor.openasync.aspx)

- [CloseAsync](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ieventprocessor.closeasync.aspx)

- [ProcessEventsAsync](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ieventprocessor.processeventsasync.aspx)

イベント処理を開始するにはインスタンス化 [EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx), 、Event Hub の適切なパラメーターを提供します。 まず [RegisterEventProcessorAsync](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.registereventprocessorasync.aspx) を登録する、 [IEventProcessor](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ieventprocessor.aspx) 実装をランタイム。 この時点で、ホストは「どん欲な」アルゴリズムを利用して Event Hub にあるすべてのパーティションでリースの取得を試行します。 これらのリースは一定の期間存続し、その後、更新する必要があります。 新しいノード (この場合は worker インスタンス) がオンラインになると、新しいノードはリースを予約し、時間と共にリースの追加取得を試行し、負荷がノード間を移動します。

![イベント プロセッサ ホスト](./media/event-hubs-programming-guide/IC759863.png)

時間と共に、均衡が確立されます。 この動的機能により、スケールアップとスケールダウンの両方で、CPU に基づく自動スケールがコンシューマーに適用されます。 Event Hubs にはメッセージ カウントの直接的概念がないため、平均的な CPU 利用率が、多くの場合、バックエンドまたはコンシューマー スケールを測定する最良のメカニズムとなります。 発行元がコンシューマーが処理できる数を超えたイベントを発行し始めた場合、コンシューマーの CPU 増加を利用し、worker インスタンス カウントを自動拡張できます。

[EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx) クラスも Azure ストレージ ベースのチェックポイント処理メカニズムを実装します。 このメカニズムはパーティション基準でオフセットを格納します。そのため、各コンシューマーは前のコンシューマーからの最後のチェックポイントを判断できます。 パーティションはリース経由でノード間を移動するため、これは負荷移動を円滑にする同期メカニズムとなります。

## 発行元失効

高度なランタイム機能に加えて [EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx), 、Event Hubs は、特定のパブリッシャーの Event Hub にイベントを送信するをブロックするためにパブリッシャーの失効を有効します。 このような機能は特に、発行元のトークンが侵害された、あるいはソフトウェア更新により不適切な動作が発生している場合に便利です。 そのような状況では、SAS トークンの一部である発行元 ID を利用してイベントの発行をブロックできます。

パブリッシャーの失効およびパブリッシャーとして Event Hubs に送信する方法の詳細については、次を参照してください。、 [サービス バス イベント ハブ Large Scale Secure Publishing](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab) サンプルです。

## 次のステップ

Event Hubs シナリオに関する詳細については、次のリンク先を参照してください。

- [Event Hubs API 概要](event-hubs-api-overview.md)
- [Event Hubs の概要](event-hubs-overview.md)
- [Event Hubs コード サンプル](http://code.msdn.microsoft.com/site/search?query=event hub & f [0]。値 = hub & f [0]。入力 = SearchText & ac = 5)
- [イベント プロセッサ ホスト API リファレンス](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx)





