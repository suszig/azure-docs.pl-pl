<properties 
   pageTitle="Service Bus のペアの名前空間 | Microsoft Azure"
   description="ペアの名前空間の実装の詳細とコスト"
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

# ペアの名前空間の実装の詳細とコストの問題

 [PairNamespaceAsync][] メソッドを使用して、 [SendAvailabilityPairedNamespaceOptions][] インスタンス、あなたに代わって表示可能なタスクを実行します。 この機能の使用はコストがかかる場合があるので、それらの動作の動作時にそれを予期できるように、それらのタスクについて理解していると便利です。 API は、ユーザーに代わって、次の動作を自動的に行います。

-   バックログ キューの作成。
-   作成、 [MessageSender][] キューまたはトピックに伝えるオブジェクトです。
-   メッセージング エンティティが利用不可になると、そのエンティティが再度利用可能になるときを検出するために ping メッセージがエンティティに送信されます。
-   必要に応じて、バックログ キューからプライマリ キューにメッセージを移動する一連の “メッセージ ポンプ” を作成します。
-   プライマリとセカンダリの終了/エラーを調整 [MessagingFactory][] インスタンス。

この機能はおおよそ次のように動作します。プライマリ エンティティが正常な場合、動作の変更はありません。 ときに、 [FailoverInterval][] 一時的でないした後に期間が経過すると、試行のプライマリ エンティティが参照では送信 [MessagingException][] または [TimeoutException][], 、次の問題が発生します。

1.  プライマリ エンティティへの送信操作は無効になり、ping が正常配信されるまで、システムはプライマリ エンティティを ping します。

2.  ランダムなバックログ キューが選択されます。

3.  [BrokeredMessage][] オブジェクトは、選択されたバックログ キューに送られます。

1.  選択されたバックログ キューへの送信操作が失敗した場合、そのキューはローテーションから外され、新しいキューが選択されます。 すべての送信者、 [MessagingFactory][] インスタンス、エラーを知ります。

次の図にシーケンスを示します。 まず、送信側がメッセージを送信します。

![ペアの名前空間][0]

プライマリ キューへの送信に失敗すると、送信者はランダムに選択されたバックログ キューにメッセージの送信を開始します。 同時に、ping タスクを開始します。

![ペアの名前空間][1]

この時点で、メッセージはまだセカンダリ キュー内にあり、プライマリ キューに配布されていません。 プライマリ キューが正常に戻ったとき、少なくとも 1 つのプロセスがサイホンを実行している必要があります。 サイホンは、すべての各種バックログ キューから、適切な送信先のエンティティ (キューおよびトピック) にメッセージを配信します。

![ペアの名前空間][2]

このトピックの残りの部分では、これらの部分のしくみの具体的な詳細を説明します。

## バックログ キューの作成

 [SendAvailabilityPairedNamespaceOptions][] オブジェクトに渡される、 [PairNamespaceAsync][] メソッドを使用するバックログ キューの数を示します。 各バックログ キューが明示的に、次のプロパティを作成し、設定 (その他のすべての値に設定、 [QueueDescription][] の既定値)。

| パス                                   | [primary namespace]/x-servicebus-transfer/[index] ここで [index] は [0, BacklogQueueCount) の値 |
|----------------------------------------|------------------------------------------------------------------------------------------------------|
| MaxSizeInMegabytes                     | 5120                                                                                                 |
| MaxDeliveryCount                       | int.MaxValue                                                                                         |
| DefaultMessageTimeToLive               | TimeSpan.MaxValue                                                                                    |
| AutoDeleteOnIdle                       | TimeSpan.MaxValue                                                                                    |
| LockDuration                           | 1 分                                                                                             |
| EnableDeadLetteringOnMessageExpiration | true                                                                                                 |
| EnableBatchedOperations                | true                                                                                                 |

たとえば、最初のバックログ キューが名前空間の作成 **contoso** という `contoso/x-servicebus-transfer/0`します。

キューが作成される際、コードはそのようなキューが存在するかどうかをまず確認します。 キューが存在しない場合、キューは作成されます。 コードは "余分な" バックログ キューをクリーンアップしません。 具体的には場合、プライマリ名前空間を使用してアプリケーションを **contoso** 5 つのバックログ キューだけでなく、バックログ キューのパスを要求 `contoso/x-servicebus-transfer/7` 存在する場合、その余分なバックログ キューが存在していますが、使用されていません。 システムでは、使用されない余分なバックログ キューの存在を明示的に許可します。 名前空間の所有者は、不要な未使用のバックログ キューをクリーンアップする必要があります。 これは、Service Bus が名前空間内のすべてのキューの存在目的を把握していないためです。 さらに、キューが指定された名前で存在しますが、想定した満たしていないかどうかは [QueueDescription][], 、理由は、既定の動作を変更する、独自のです。 コードによって、バックログ キューへの変更が加えられる保証はありません。 変更は徹底的にテストしてください。

## カスタム MessageSender

送信する場合、すべてのメッセージを通過内部 [MessageSender][] すべて動作して、バックログへのリダイレクトは正常に動作するオブジェクトはキュー処理「を解除します」。 一時的でないエラーを受け取ると、タイマーが開始されます。 後に、 [TimeSpan][] 期間で構成される、 [FailoverInterval][] プロパティの値はその間成功したメッセージは送信されない、フェイル オーバーが進行中です。 この時点で、エンティティごとに次が行われます。

- Ping タスクを実行しすべて [PingPrimaryInterval][] エンティティがあるかを確認します。 このタスクが成功すると、このエンティティを使用するすべてのクライアント コードが、プライマリ名前空間に新しいメッセージの送信を直ちに開始します。

- 今後の要求の他のすべての送信者から同じエンティティに送信するが、 [BrokeredMessage][] するバックログ キューに留まるように変更を送信します。 変更の一部のプロパティの削除、 [BrokeredMessage][] オブジェクトし、別の場所に保存します。 次のプロパティがクリアされ、新しいエイリアスで追加されるため、Service Bus と SDK がメッセージを一貫して処理できるようになります。

| 古いプロパティ名       | 新しいプロパティ名 |
|-------------------------|-------------------|
| SessionId               | x-ms-sessionid    |
| TimeToLive              | x-ms-timetolive   |
| ScheduledEnqueueTimeUtc | x-ms-path         |

元の転送先のパスも x-path ms をという名前のプロパティとしてメッセージ内に保存されます。 この設計により、多くのエンティティのメッセージが 1 つのバックログ キューに共存できるようになります。 プロパティはサイホンによって変換されて戻されます。

カスタム [MessageSender][] オブジェクトでは、256 KB の制限し、フェールオーバーが進行中に問題を検出できます。 カスタム [MessageSender][] オブジェクトは、すべてのキューおよびトピックをまとめてバックログ キューのメッセージを格納します。 このオブジェクトは、バックログ キュー内で、多くのプライマリからのメッセージを混在させます。 不明な他の多くのクライアント間の負荷分散を処理するため、SDK をランダムに選択 1 つのバックログ キューごとに [QueueClient][] または [TopicClient][] コードで作成します。

## Ping

Ping メッセージは、空 [BrokeredMessage][] でその [ContentType][] プロパティ アプリケーション/vnd.ms-servicebus の ping に設定し、 [TimeToLive][] 1 秒の値。 この ping は、Service Bus で 1 つの特性を持つ: サーバーは ping を任意の呼び出し元が要求したときに配信しません、 [BrokeredMessage][]します。 そのため、これらのメッセージを受信し無視する方法を学習する必要はありません。 ごとに各エンティティ (一意のキューまたはトピック) [MessagingFactory][] 使用不可能と見なされる場合、クライアントごとにインスタンスが ping されます。 既定で、これは 1 分間に 1 回行われます。 Ping メッセージは通常の Service Bus メッセージと見なされ、帯域幅とメッセージの料金が発生します。 クライアントが使用可能なシステムであることを検出すると、メッセージは直ちに停止されます。

## サイホン

アプリケーションの少なくとも 1 つの実行可能プログラムが、サイホンをアクティブに実行している必要があります。 サイホンは 15 分間続く長いポーリングを実行します。 すべてのエンティティが利用可能で、バックログ キューが 10 ある場合、サイホンをホストするアプリケーションは、1 時間に 40 回、1 日に 960 回、30 日で 28,880 回受信操作を呼び出します。 サイホンがメッセージをアクティブにバックログからプライマリ キューに移動する場合、各メッセージで次の料金が発生します (メッセージ サイズと帯域幅の標準の料金がすべてのステージで適用されます)。

1.  バックログに送信します。

2.  バックログから受信します。

3.  プライマリに送信します。

4.  プライマリから受信します。

## 終了およびエラー動作

プライマリまたはセカンダリでは 1 回、サイホンをホストするアプリケーション内で [MessagingFactory][] エラーや障害が発生して閉じると、サイホンをサイホンがこの状態では、サイホン行為を検出することなきます。 場合、その他の [MessagingFactory][] が閉じられていない、サイホンがまだ開いているをフォールト 5 秒以内 [MessagingFactory][]します。

## 次のステップ

参照してください [Asynchronous messaging patterns and high availability] Service Bus の非同期メッセージングの詳細についてです。 

  [PairNamespaceAsync]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.pairnamespaceasync.aspx
  [SendAvailabilityPairedNamespaceOptions]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.aspx
  [MessageSender]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx
  [MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [FailoverInterval]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.pairednamespaceoptions.failoverinterval.aspx
  [MessagingException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
  [TimeoutException]: https://msdn.microsoft.com/library/azure/system.timeoutexception.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  [0]: ./media/service-bus-paired-namespaces/IC673405.png
  [1]: ./media/service-bus-paired-namespaces/IC673406.png
  [2]: ./media/service-bus-paired-namespaces/IC673407.png
  [QueueDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx
  [TimeSpan]: https://msdn.microsoft.com/library/azure/system.timespan.aspx
  [PingPrimaryInterval]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.pingprimaryinterval.aspx
  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx
  [ContentType]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx
  [TimeToLive]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx
  [Asynchronous messaging patterns and high availability]: service-bus-async-messaging.md
