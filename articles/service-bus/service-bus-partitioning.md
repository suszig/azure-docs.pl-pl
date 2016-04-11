<properties 
   pageTitle="パーティション分割されたメッセージング エンティティ | Microsoft Azure"
   description="複数のメッセージ ブローカーを使用したメッセージング エンティティをパーティション分割する方法について説明します。"
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
   ms.date="09/18/2015"
   ms.author="sethm" />

# パーティション分割されたメッセージング エンティティ

Azure Service Bus では、メッセージを処理する複数のメッセージ ブローカーとメッセージを格納する複数のメッセージング ストアを採用しています。 従来のキューまたはトピックは、単一のメッセージ ブローカーで処理されて 1 つのメッセージング ストアに格納されます。 Service Bus では、キューまたはトピックを複数のメッセージ ブローカーとメッセージング ストアにパーティション分割することもできます。 そのため、パーティション分割されたキューまたはトピックの全体のスループットは、単一のメッセージ ブローカーまたはメッセージング ストアのパフォーマンスによって制限されなくなりました。 また、1 つのメッセージング ストアが一時的に停止しても、パーティション分割されたキューまたはトピックは使用することができます。 パーティション分割されたキューとトピックには、トランザクションやセッションのサポートなど、あらゆる高度な Service Bus 機能を含めることができます。

サービス バスの内部構造についての詳細については、次を参照してください。、 [Service Bus アーキテクチャ][] トピックです。

## パーティション分割されたキューとトピック

パーティション分割されたキューまたはトピックはそれぞれ、複数のフラグメントで構成されます。 フラグメントはそれぞれ別々のメッセージング ストアに格納され、別々のメッセージ ブローカーで処理されます。 パーティション分割されたキューまたはトピックにメッセージが送信されると、Service Bus はそのメッセージをいずれかのフラグメントに割り当てます。 対象のフラグメントは、Service Bus によってランダムに選択されるか、送信側で指定できるパーティション キーによって選択されます。

クライアントがパーティション分割されたキューまたはパーティション分割されたトピックのサブスクリプションからメッセージを受信する場合、Service Bus はすべてのフラグメントを照会してメッセージを探し、いずれかのメッセージング ストアから返された最初のメッセージを受信側に返します。 Service Bus は、追加の受信要求を受信すると、他のメッセージをキャッシュして返します。 受信側クライアントはパーティション分割を認識しません。パーティション分割されたキューやトピックの動作 (読み込み、完了、遅延、配信不能、プリフェッチなど) は、クライアント側には通常のエンティティの動作と同一に見えます。

パーティション分割されたキューやトピックとのメッセージの送受信に追加費用は発生しません。

## パーティション分割の有効化

Microsoft Azure Service Bus でパーティション分割されたキューとトピックを使用するには、Azure SDK Version 2.2 以降を使用するか、HTTP 要求で `api-version=2013-10` を指定します。

Service Bus のキューとトピックは、1 GB、2 GB、3 GB、4 GB、5 GB で作成できます (既定値は 1 GB)。 パーティション分割が有効な場合、Service Bus は指定された GB 数に対して 1 GB ごとに 16 個のパーティションを作成します。 そのため、サイズが 5 GB のキューを作成する場合は、16 個のパーティションでキューの最大サイズになります (5 \ * 16) = 80 GB です。 パーティション分割されたキューまたはトピックの最大サイズを確認するに対応するエントリを見て、 [Azure クラシック ポータル][]します。

パーティション分割されたキューまたはトピックを作成する方法は複数あります。 アプリケーションからキューまたはトピックを作成するときは、それぞれ設定してキューまたはトピックのパーティション分割を有効にできます、 [QueueDescription.EnablePartitioning][] または [TopicDescription.EnablePartitioning][] プロパティを **true**します。 これらのプロパティは、キューまたはトピックの作成時に設定する必要があります。 既存のキューまたはトピックでこれらのプロパティを変更することはできません。 次に例を示します。

```
// Create partitioned topic
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

または、パーティション分割されたキューまたはか Visual Studio でのトピックを作成、 [Azure クラシック ポータル][]します。 確認して、ポータルで新しいキューまたはトピックを作成するときに、 **パーティション分割を有効にする** オプション、 **構成** キューまたはトピック ウィンドウのタブをクリックします。 Visual Studio で、[、 **パーティション分割を有効にする** 内のチェック ボックス、 **新しいキュー** または **新しいトピック** ] ダイアログ ボックス。

## パーティション キーの使用

パーティション分割されたキューまたはトピックにメッセージがエンキューされると、Service Bus はパーティション キーが存在するかどうかを調べます。 パーティション キーが見つかった場合は、そのキーに基づいてフラグメントを選択します。 パーティション キーが見つからない場合は、内部アルゴリズムに基づいてフラグメントを選択します。

### パーティション キーを使用する場合

セッションやトランザクションなどの一部のシナリオでは、メッセージを特定のフラグメントに格納する必要があります。 このようなシナリオでは必ず、パーティション キーを使用する必要があります。 同じパーティション キーを使用するメッセージはすべて、同じフラグメントに割り当てられます。 そのフラグメントが一時的に使用できなくなると、Service Bus はエラーを返します。

以下に示すように、シナリオに応じて、さまざまなメッセージ プロパティがパーティション キーとして使用されます。

**SessionId**: メッセージがある場合、 [BrokeredMessage.SessionId][] プロパティを設定し、Service Bus は、このプロパティをパーティション キーとして使用します。 このように、同じセッションに属するメッセージはすべて、同じメッセージ ブローカーによって処理されます。 この方法により、Service Bus はメッセージの順序と、セッション状態の整合性を保証することができます。

**PartitionKey**: メッセージがある場合、 [BrokeredMessage.PartitionKey][] プロパティではなく、 [BrokeredMessage.SessionId][] プロパティを設定し、サービス バスを使用して、 [PartitionKey][] プロパティをパーティション キーとして。 メッセージの両方があるあれば、 [SessionId][] と [PartitionKey][] プロパティを設定、両方のプロパティは、同一である必要があります。 場合、 [PartitionKey][] プロパティが異なる値に設定されている、 [SessionId][] プロパティ、サービス バスを返します、 **InvalidOperationException** 例外です。  [PartitionKey][] 送信者は、セッション以外に注意してくださいトランザクション メッセージを送信する場合、プロパティを使用する必要があります。 パーティション キーを使用することにより、トランザクション内で送信されるすべてのメッセージを同じメッセージング ブローカーで処理することができます。

**MessageId**: キューまたはトピックがある場合、 [QueueDescription.RequiresDuplicateDetection][] プロパティに設定 **true** と [BrokeredMessage.SessionId][] または [BrokeredMessage.PartitionKey][] プロパティが設定されていない、 [BrokeredMessage.MessageId][] プロパティは、パーティション キーとして機能します。 (送信元のアプリケーションでメッセージ ID が割り当てられていない場合は、Microsoft .NET および AMQP のライブラリによって自動的にメッセージ ID が割り当てられます)。この場合は、同じメッセージのすべてのコピーが同じメッセージ ブローカーによって処理されます。 これにより、Service Bus は重複したメッセージを検出し削除することができるようになります。 場合、 [QueueDescription.RequiresDuplicateDetection][] にプロパティが設定されていない **true**, 、Service Bus と見なしません、 [MessageId][] パーティション キーとしてのプロパティです。

### パーティション キーを使用しない場合

パーティション キーが存在しない場合、Service Bus は、ラウンドロビン方式で、パーティション分割されたキューまたはトピックのすべてのフラグメントにメッセージを配信します。 選択されたフラグメントが使用できない場合、Service Bus はメッセージを別のフラグメントに割り当てます。 このように、メッセージング ストアが一時的に使用できなくても送信操作は成功します。

サービス バスを提供するのに十分な時間キューへのメッセージを別のフラグメントに、 [MessagingFactorySettings.OperationTimeout][] メッセージは 15 秒より大きくなければなりませんを送信するクライアントによって指定された値。 設定することをお勧めしますが、 [OperationTimeout][] プロパティを 60 秒間の既定値にします。

パーティション キーは、メッセージを特定のフラグメントに "ピン留め" します。 このフラグメントを保持しているメッセージング ストアを使用できない場合、Service Bus はエラーを返します。 パーティション キーが存在しない場合、Service Bus は異なるフラグメントを選択できるので、操作は成功します。 したがって、パーティション キーが必要でない場合は、パーティション キーを指定しないことをお勧めします。

## 高度なトピック: パーティション分割されたエンティティでのトランザクションの使用

トランザクションの一部として送信されるメッセージでは、パーティション キーを指定する必要があります。 これは、次のプロパティのいずれかを指定できます: [BrokeredMessage.SessionId][], 、[BrokeredMessage.PartitionKey][], 、または [BrokeredMessage.MessageId][]します。 同じトランザクションの一部として送信されるすべてのメッセージで、同じパーティション キーを指定する必要があります。 Service Bus の取得のかどうかは、トランザクション内でパーティション キーを使用しないメッセージを送信すると、 **InvalidOperationException** 例外です。 Service Bus の取得のかどうかは、異なるパーティション キーが設定されている、同じトランザクション内の複数のメッセージを送信すると、 **InvalidOperationException** 例外です。 次に例を示します。

```
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.PartitionKey = "myPartitionKey";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

パーティション キーとなるプロパティが設定されている場合、Service Bus はメッセージを特定のフラグメントに "ピン留め" します。 この動作は、トランザクションが使用されているかどうかにかかわらず起こります。 必要でない限り、パーティション キーを指定しないことをお勧めします。

## パーティション分割されたエンティティでのセッションの使用

セッションを認識するトピックまたはキューへトランザクション メッセージを送信するメッセージが必要、 [BrokeredMessage.SessionId][] プロパティ セット。 場合、 [BrokeredMessage.PartitionKey][] プロパティが指定されても、それと同一である必要があります、 [SessionId][] プロパティです。 Service Bus 返しますのかどうかは異なる、 **InvalidOperationException** 例外です。

通常の (パーティション分割されていない) キューまたはトピックと異なり、単一のトランザクションを使用して複数のメッセージを別々のセッションに送信することはできません。 これを試みると、Service Bus は **InvalidOperationException** 例外を返します。 次に例を示します。

```
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.SessionId = "mySession";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

## パーティション分割されたエンティティでのメッセージの自動転送

Azure Service Bus では、パーティション分割されたエンティティを送信元または送信先とするメッセージの自動転送、およびパーティション分割されたエンティティ間でのメッセージの自動転送をサポートしています。 メッセージの自動転送を有効にするには設定、 [QueueDescription.ForwardTo][] ソース キューまたはサブスクリプションのプロパティです。 メッセージをパーティション キーを指定する場合 ([SessionId][], 、[PartitionKey][], 、または [MessageId][])、転送先エンティティのパーティション キーを使用します。

## パーティション分割されたエンティティの制限事項

現行の実装において、Service Bus はパーティション分割されたキューまたはトピックに以下の制限を適用します。

-   パーティション分割されたキューまたはトピックは、AMQP だけでなく、SBMP または HTTP/HTTPS を介して使用できます。

-   パーティション分割されたキューまたはトピックは、単一のトランザクションの別々のセッションに属するメッセージの送信はサポートしていません。

-   Service Bus は、現在、名前空間あたり最大 100 のパーティション分割されたキューまたはトピックをサポートします。 パーティション分割された各キューまたはトピックは、名前空間あたり 10,000 エンティティのクォータに対してカウントされます。

-   パーティション分割されたキューとトピックは、Service Bus for Windows Server Version 1.0 および 1.1 ではサポートされていません。

## 次のステップ

メッセージング エンティティのパーティション分割の詳細については、Service Bus のパーティション分割されたキューとトピックの AMQP 1.0 のサポートについての説明 (近日公開予定!) を参照してください。 

  [Service Bus Architecture]: service-bus-architecture.md
  [Azure classic portal]: http://manage.windowsazure.com
  [QueueDescription.EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx
  [TopicDescription.EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablepartitioning.aspx
  [BrokeredMessage.SessionId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx
  [BrokeredMessage.PartitionKey]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.partitionkey.aspx
  [SessionId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx
  [PartitionKey]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.partitionkey.aspx
  [QueueDescription.RequiresDuplicateDetection]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection.aspx
  [BrokeredMessage.MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [QueueDescription.RequiresDuplicateDetection]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection.aspx
  [MessagingFactorySettings.OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
  [OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
  [QueueDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.forwardto.aspx
  [AMQP 1.0 support for Service Bus partitioned queues and topics]: service-bus-partitioned-entities-amqp-overview.md

