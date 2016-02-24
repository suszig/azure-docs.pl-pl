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

Event Hubs をサポートする .NET クラスは、Microsoft.ServiceBus.dll アセンブリの一部です。 Service Bus API を参照し、すべての Service Bus 依存関係を備えたアプリケーションを構成する最も簡単な方法は Service Bus NuGet パッケージをダウンロードすることです。 詳細については、次を参照してください。 [NuGet Service Bus パッケージを使用して](https://msdn.microsoft.com/library/azure/dn741354.aspx)します。 また、使用することができます、 [パッケージ マネージャー コンソール](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) Visual Studio でします。 これを行うには、次のコマンドを発行、 [パッケージ マネージャー コンソール](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) ウィンドウ。

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

このメソッドは `appSettings` セクションにある App.config ファイルの Service Bus 情報を使用します。 例については、 `appSettings` Service Bus の接続情報を格納するために使用する XML ドキュメントを参照して、 [Microsoft.ServiceBus.Messaging.EventHubClient.Create(System.String)](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.create.aspx) メソッドです。

接続文字列からクライアントを作成することもできます。 この選択肢は Azure worker ロールを使用する場合に効果があります。worker の設定プロパティに文字列を格納できるためです。 次に例を示します。

```
EventHubClient.CreateFromConnectionString("your_connection_string");
```

接続文字列は前のメソッドの App.config ファイルに表示される形式と同じ形式になります。

```
Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=Manage;SharedAccessKey=[key]
```

最後に、作成することはまた、 [EventHubClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx) オブジェクトから、 [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) インスタンスに、次の例に示すようにします。

```
var factory = MessagingFactory.CreateFromConnectionString("your_connection_string");
var client = factory.CreateEventHubClient("MyEventHub");
```

さらに注意することが重要 [EventHubClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx) メッセージング ファクトリ インスタンスから作成されたオブジェクトが同じ基になる TCP 接続を再利用します。 そのため、これらのオブジェクトでは、スループットにクライアント側の制限があります。  [作成](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.create.aspx) メソッドは、1 つのメッセージング ファクトリを再利用します。 1 つの送信者から非常に高いスループットが必要なかどうかは、複数のメッセージ ファクトリと 1 つを作成する [EventHubClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx) 各メッセージング ファクトリからのオブジェクト。

## Event Hub にイベントを送信する

作成することで Event Hub にイベントを送信、 [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) インスタンスと経由の送信、 [送信](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.send.aspx) メソッドです。 このメソッドは、1 つの [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) instance パラメーターに同期的に Event Hub に送信します。

## イベントのシリアル化

 [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) クラスには [4 つのオーバー ロードされたコンス トラクター](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) さまざまなオブジェクトとシリアライザー、バイト配列、ストリームなどのパラメーターを受け取ります。 インスタンスを作成することも、 [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) クラスし、その後、本文のストリームを設定します。 使用して、JSON を使用する場合 [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx), 、使用する **Encoding.UTF8.GetBytes()** JSON でエンコードされた文字列のバイト配列を取得します。

## パーティション キー

 [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) クラスには、 [PartitionKey](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.partitionkey.aspx) 、送信側はパーティション割り当てを作成するハッシュ値を指定できるようにするプロパティです。 パーティション キーを利用すると、同じキーのすべてのイベントが Event Hub の同じパーティションに送信されます。 共通のパーティション キーにはユーザー セッション ID と一意の送信者 ID が含まれます。  [PartitionKey](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.partitionkey.aspx) プロパティは省略可能なを使用する場合に指定することができます、 [Microsoft.ServiceBus.Messaging.EventHubClient.Send(Microsoft.ServiceBus.Messaging.EventData)](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) または [Microsoft.ServiceBus.Messaging.EventHubClient.SendAsync(Microsoft.ServiceBus.Messaging.EventData)](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) メソッドです。 値が提供されていない場合 [PartitionKey](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.partitionkey.aspx), 、送信されたイベントはラウンドロビン モデルを使用してパーティションに配信します。

## イベントの一括送信操作

イベントを一括送信すると、スループットが劇的に上がります。  [SendBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.sendbatch.aspx) メソッドには、 **IEnumerable** 型のパラメーター [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) バッチ全体を分割不可能な操作として Event Hub に送信します。

```
public void SendBatch(IEnumerable<EventData> eventDataList);
```

これは重要なことですが、1 回のバッチがイベントの 256 KB 制限を超えてはならないことに注意してください。 また、バッチの各メッセージで同じ発行元 ID が使用されます。 バッチが最大イベント サイズを超えないようにすることは送信者側の責任となります。 場合は、クライアント **送信** エラーが生成されます。

## 非同期送信と大規模送信

Event Hub にイベントを非同期送信することもできます。 非同期送信を利用すると、クライアントがイベントを送信する速度が上がります。 両方の [送信](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.send.aspx) と [SendBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.sendbatch.aspx) メソッドを返す非同期バージョンで利用可能な [タスク](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) オブジェクトです。 この手法ではスループットが上がりますが、Event Hubs で調整中でもクライアントはイベントの送信を続けるので、適切に実装されていない場合、クライアントに障害が発生したり、メッセージが失われたりすることがあります。 さらに、使用、 [RetryPolicy](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.cliententity.retrypolicy.aspx) クライアントの再試行オプションを制御するクライアント上のプロパティです。

## パーティション送信者の作成

パーティション キーを持つ Event Hub にイベントを送信するのが最も一般的ですが、特定のパーティションにイベントを直接送信することもあります。 次に例を示します。

```
var partitionedSender = client.CreatePartitionedSender(description.PartitionIds[0]);
```

[CreatePartitionedSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.createpartitionedsender.aspx) 返します、 [EventHubSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubsender.aspx) 、特定の Event Hub パーティションにイベントを発行するように使用できるオブジェクトします。

## イベント コンシューマー

Event Hubs はイベント使用のための 2 つの主要なモデル。 ダイレクト レシーバーと上位の抽象化など、 [EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx)します。 ダイレクト レシーバーはコンシューマー グループ内のパーティションの独自のアクセス調整を担当します。

### ダイレクト コンシューマー

コンシューマー グループ内のパーティションから読み取る最も直接的な方法を使用して、 [EventHubReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubreceiver.aspx) クラスです。 このクラスのインスタンスを作成するには、インスタンスを使用する必要があります、 [EventHubConsumerGroup](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubconsumergroup.aspx) クラスです。 次の例では、コンシューマー グループの受信者を作成するときにパーティション ID を指定する必要があります。

```
EventHubConsumerGroup group = client.GetDefaultConsumerGroup();
var receiver = group.CreateReceiver(client.GetRuntimeInformation().PartitionIds[0]);
```

 [CreateReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubconsumergroup.createreceiver.aspx) メソッドを作成中のリーダーの制御を容易にするいくつかのオーバー ロードがあります。 これらのメソッドでは文字列とタイムスタンプのいずれかをオフセットとして指定します。返されたストリームにこの指定したオフセットを含めるか、その後に開始するか指定できます。 レシーバーを作成したら、返されたオブジェクトでイベントの受信を開始できます。  [受信](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubreceiver.receive.aspx) メソッドや時間を待機するバッチ サイズなどの受信操作パラメーターを制御する 4 つのオーバー ロードがあります。 これらのメソッドの非同期バージョンを利用し、コンシューマーのスループットを上げることができます。 次に例を示します。

```
bool receive = true;
string myOffset;
while(receive)
{
    var message = receiver.Receive();
    myOffset = message.Offset;
    string body = Encoding.UTF8.GetString(message.GetBytes());
    Console.WriteLine(String.Format("Received message offset: {0} \nbody: {1}", myOffset, body));
}
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

