<properties 
   pageTitle="Service Bus の非同期メッセージング |Microsoft Azure"
   description="Service Bus の非同期仲介型メッセージングに関する説明です。"
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

# 非同期メッセージング パターンと高可用性

非同期メッセージングはさまざまな方法で実装できます。 総称してメッセージング エンティティと呼ばれるキュー、トピック、サブスクリプションにより、Azure Service Bus は格納と転送のメカニズムを通じて非同期性をサポートします。 通常の (同期) 操作では、キューとトピックにメッセージを送信し、キューとサブスクリプションからメッセージを受信します。 作成するアプリケーションは、これらのエンティティが常に使用できることに依存します。 さまざまな状況によってエンティティの状態が変化した場合、ほとんどのニーズを満たすことができる、機能を低減したエンティティを提供する方法が必要になります。

通常、アプリケーションは非同期メッセージング パターンを使用して数多くの通信シナリオを実現します。 サービスが実行されていない場合でも、クライアントがサービスにメッセージを送信できるアプリケーションを構築できます。 通信が急増することがあるアプリケーションでは、キューによって、通信をバッファーに書き込む場所を提供することで、負荷を分散できます。 最後に、簡単であるが効果的なロード バランサーによって、複数のコンピューター間でメッセージを分散できます。

これらのエンティティの可用性を維持するために、永続的なメッセージング システムでこれらのエンティティが使用不可能と表示される可能性があるさまざまな方法を考えてみます。 一般的に、次のような方法でアプリケーションを記述すると、エンティティは使用不可能になります。

1.  メッセージを送信できない。

2.  メッセージを受信できない。

3.  エンティティを管理できない (エンティティの作成、取得、更新、または削除)。

4.  サービスにアクセスできない。

これらの障害のそれぞれについて、アプリケーションが機能をある程度抑えたレベルで継続して動作できるようにするさまざまな障害モードが存在します。 たとえば、メッセージを送信できるが受信できないシステムであっても、顧客の注文を受信することはできるが、注文を処理することはできないなどです。 このトピックでは、発生する可能性のある問題と、それらの問題を軽減する方法について説明します。 Service Bus では、選択する必要がある数多くの緩和策が導入されており、ここではこれらのオプトイン軽減策の使用に関するルールについても説明します。

## Service Bus の信頼性

メッセージとエンティティの問題を処理する方法は多数存在します。また、これらの緩和策の適切な使用に関するガイドラインがあります。 このガイドラインを理解するには、最初に Service Bus で障害が発生する可能性がある状況について理解する必要があります。 Azure システムの設計により、これらすべての問題は一時的である傾向があります。 大まかに言って、使用不可能となる原因は次のとおりです。

-   Service Bus が依存している外部システムからの調整。 調整は、ストレージ リソースやコンピューティング リソースとのやり取りから発生します。

-   Service Bus が依存しているシステムの問題。 たとえば、ストレージの特定の部分で問題が発生するなどです。

-   単一のサブシステムでの Service Bus の障害。 この状況では、コンピューティング ノードが一貫性のない状態になって再起動が必要になり、対応しているすべてのエンティティが他のノードに負荷を分散することになります。 これにより、メッセージの処理が一時的に低速になります。

-   Azure データセンター内での Service Bus の障害。 これは、数分または数時間にわたってシステムにアクセスできなくなる、以前からある "致命的な障害" です。

> [AZURE.NOTE] 用語 **ストレージ** Azure Storage および SQL Azure の両方が当てはまります。

Service Bus には、これらの問題に対する数多くの緩和策が用意されています。 以降のセクションでは、個々の問題とそれぞれの緩和策について説明します。

### 調整

Service Bus では、調整によりメッセージ レートを協調管理できます。 それぞれの Service Bus ノードが、個別に複数のエンティティを格納します。 これらの各エンティティにより、CPU、メモリ、ストレージ、その他のファセットに関してシステムへの要求が行われます。 これらのいずれかのファセットで、定義されたしきい値を超える使用が検出されると、Service Bus によって特定の要求が拒否される可能性があります。 呼び出し元の受信、 [ServerBusyException][] と 10 秒後に再試行します。

緩和策として、コードでエラーを読み取り、メッセージの再試行を少なくとも 10 秒間停止します。 顧客アプリケーションのさまざまな部分でエラーが発生する可能性があるため、各部分が独立して再試行ロジックを実行する必要があります。 コードでキューまたはトピックでパーティション化を有効にすることにより、調整される可能性を低減できます。

### Azure の依存関係の問題

Azure 内の他のコンポーネントで、サービスの問題が発生する場合があります。 たとえば、Service Bus が使用するシステムがアップグレードされた場合、そのシステムでは一時的に機能が低下することがあります。 このような種類の問題に対処するために、Service Bus では定期的に緩和策を調査し、実装しています。 これらの緩和策には副作用があります。 たとえば、ストレージに関する一時的な問題を処理するために、Service Bus はメッセージ送信操作が一貫して動作できるようにするシステムを実装しています。 緩和策の性質により、送信されたメッセージが影響するキューまたはサブスクリプションに表示されて受信操作の準備が整うまでに、最大 15 分かかることがあります。 一般的に、ほとんどのエンティティではこの問題は発生しません。 ただし、Azure 内の Service Bus のエンティティの数によっては、少数のサブセットの Service Bus の顧客でこの緩和策が必要になる場合があります。

### 単一のサブシステムでの Service Bus の障害。

どのアプリケーションでも、状況によっては Service Bus の内部コンポーネントに不整合が生じる場合があります。 Service Bus がこれを検出すると、アプリケーションからデータを収集し、何が起こったかを診断する手助けとします。 データが収集されると、アプリケーションは一貫性のある状態に戻すために再起動されます。 このプロセスは比較的迅速に発生し、エンティティが使用できない状態は最大で数分続きます。ただし、通常のダウンタイムはこれよりもはるかに短くなります。

このような場合、クライアント アプリケーションの生成、 [System.TimeoutException][] または [MessagingException][] 例外です。 Service Bus .NET SDK には、自動化されたクライアント再試行ロジックという形で、この問題の軽減策が備わっています。 再試行期間が終了してもメッセージが配信されない場合は、ペアの名前空間などの他の機能を試すことができます。 ペアの名前空間には他にも問題がありますが、それについてはこのドキュメントの後半で説明します。

### Azure データセンター内での Service Bus の障害。

Azure データセンターでの障害の理由として最も可能性が高いのは、Service Bus または依存システムのアップグレード デプロイの失敗です。 プラットフォームが成熟するにつれて、この種類の障害が発生する可能性は低くなります。 データセンターの障害は、次に挙げる理由から発生する可能性もあります。

-   停電 (電源供給と発電源が失われた)。
-   接続 (クライアントと Azure の間のインターネットの切断)。

いずれの場合も、自然災害または人的災害によって問題が発生します。 この問題に対処し、継続して確実にメッセージを送信できるようにするために、ペアの名前空間を使用して、プライマリの場所を再び正常にしている間は、メッセージを 2 番目の場所に送信するようにできます。 詳細については、次を参照してください。 [Insulating Service Bus アプリケーションに対する Service Bus の障害や障害のためのベスト プラクティス][]します。

## ペアの名前空間

ペアの名前空間機能は、データ センター内の Service Bus エンティティまたはデプロイが使用できなくなるシナリオをサポートします。 このイベントの発生頻度は低いものの、分散システムは最悪のシナリオに対処できるように準備しておく必要があります。 通常、このイベントが発生するのは、Service Bus が依存している一部の要素で、一時的な問題が発生したことが原因です。 障害中にアプリケーションの可用性を維持するため、Service Bus のユーザーは 2 つの異なる名前空間を (できれば異なるデータ センターで) 使用して、メッセージング エンティティをホストできます。 このセクションの残りの部分では、次の用語を使用します。

-   プライマリ名前空間: アプリケーションが送受信操作についてやり取りする名前空間。

-   セカンダリ名前空間: プライマリ名前空間のバックアップとして機能する名前空間。 アプリケーション ロジックはこの名前空間とやり取りしません。

-   フェールオーバー間隔: アプリケーションがプライマリ名前空間からセカンダリ名前空間に切り替える前に、通常の障害を受け入れる時間。

ペアの名前空間は送信の可用性をサポートします。 送信の可用性では、メッセージの送信機能の保持が重視されます。 送信の可用性を使用するには、アプリケーションは次の要件を満たす必要があります。

1.  メッセージはプライマリ名前空間からのみ受信されます。

2.  特定のキュー/トピックに送信されたメッセージは、順序を逸脱して配信されることがあります。

3.  アプリケーションがセッションを使用する場合: セッション内のメッセージは順序を逸脱して配信されることがあります。 これはセッションの通常の機能ではありません。 つまり、アプリケーションはセッションを使用してメッセージを論理的にグループ分けしています。 セッション状態はプライマリ名前空間でのみ維持されます。

4.  セッション内のメッセージは順序を逸脱して配信されることがあります。 これはセッションの通常の機能ではありません。 つまり、アプリケーションはセッションを使用してメッセージを論理的にグループ分けしています。

5.  セッション状態はプライマリ名前空間でのみ維持されます。

6.  プライマリ キューは、セカンダリ キューがすべてのメッセージをプライマリ キューに配信する前に、オンラインになってメッセージの受け入れを開始できます。

ここでは、API とその実装方法について説明し、この機能を使用するサンプル コードを示します。 この機能には課金が関連することに注意してください。

### MessagingFactory.PairNamespaceAsync API

ペアの名前空間機能では、次の新しい方法が導入されています、 [Microsoft.ServiceBus.Messaging.MessagingFactory][] クラス。

```
public Task PairNamespaceAsync(PairedNamespaceOptions options);
```

タスクが完了すると、名前空間のペアリングも完了し、いずれかに対して動作できるよう [MessageReceiver][], 、[QueueClient][], 、または [TopicClient][] で作成された、 [MessagingFactory][]します。 [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions][] できるペアリングのさまざまな種類が含まれる基本クラスは、 [MessagingFactory][]します。 現在、唯一の派生クラスでという名前 [SendAvailabilityPairedNamespaceOptions][], 、送信の可用性要件を実装します。 [SendAvailabilityPairedNamespaceOptions][] 相互に構築するコンス トラクターのセットがあります。 ほとんどのパラメーターを持つコンストラクターを見ると、他のコンストラクターの動作を理解できます。

```
public SendAvailabilityPairedNamespaceOptions(
    NamespaceManager secondaryNamespaceManager,
    MessagingFactory messagingFactory,
    int backlogQueueCount,
    TimeSpan failoverInterval,
    bool enableSyphon)
```

これらのパラメーターの意味は次のとおりです。

-   *secondaryNamespaceManager*: 初期化された [NamespaceManager][] セカンダリ名前空間のインスタンスを [PairNamespaceAsync][] セカンダリ名前空間を設定する方法を使用することができます。 名前空間のキューの一覧を取得し、必要なバックログ キューが存在することを確認するために、マネージャーが使用されます。 これらのキューが存在しない場合は作成されます。 [NamespaceManager][] でトークンを作成できる必要があります、 **管理** を要求します。

-   *messagingFactory*: [MessagingFactory][] セカンダリ名前空間のインスタンス。  [MessagingFactory][] を送信するために使用し、 [EnableSyphon][] にプロパティが設定されている **true**, 、バックログ キューからメッセージを受信します。

-   *backlogQueueCount*: 作成するバックログ キューの数。 この値は 1 以上にする必要があります。 バックログにメッセージを送信する場合、これらのキューの 1 つがランダムに選択されます。 値を 1 に設定した場合、1 つのキューのみを使用できます。 これが発生し、1 つのバックログ キューがエラーを生成した場合、クライアントは別のバックログ キューを試すことができず、メッセージの送信に失敗する可能性があります。 この値を大きな値に設定し、既定値は 10 に設定することをお勧めします。 1 日にアプリケーションが送信するデータの量に応じて、この値をより高い値または低い値に変更できます。 各バックログ キューは最大 5 GB のメッセージを保持できます。

-   *failoverInterval*: セカンダリ名前空間を 1 つのエンティティを切り替える前にプライマリ名前空間で障害を許容する時間。 フェールオーバーはエンティティを基準にして発生します。 1 つの名前空間のエンティティは、Service Bus の異なるノードに存在することが一般的です。 1 つのエンティティの障害は、別のエンティティの障害を意味しません。 この値に設定することができます [System.TimeSpan.Zero][] 、最初で一時的でない障害の直後に、セカンダリにフェールオーバーします。 フェールオーバー タイマーをトリガーする障害は [MessagingException][] を [IsTransient][] プロパティが false の場合、または [System.TimeoutException][]します。 その他の例外など [UnauthorizedAccessException][] 、フェイル オーバーをさせない場合、クライアントの構成が正しくないことを示すためです。 A [ServerBusyException][] は原因フェールオーバーが発生しない適切なパターンは、10 秒待機するため、メッセージを再送信します。

-   *enableSyphon*: この特定のペア サイホンすること、セカンダリ名前空間からのメッセージに、プライマリ名前空間を示します。 一般に、メッセージを送信するアプリケーションはこの値に設定する必要があります **false**; メッセージを受信するアプリケーションがこの値に設定する必要があります **true**します。 この理由は頻度であり、メッセージ受信者はメッセージ送信者よりも少ないためです。 受信者の数に応じて、1 つのアプリケーション インスタンスがサイホンの処理を行うようにする選択ができます。 多くの受信者を使用すると、各バックログ キューで課金への影響があります。

コードを使用するには、作成、プライマリ [MessagingFactory][] インスタンス、セカンダリ [MessagingFactory][] インスタンス、セカンダリ [NamespaceManager][] インスタンス、および [SendAvailabilityPairedNamespaceOptions][] インスタンス。 呼び出しは、次のように単純にすることもできます。

```
SendAvailabilityPairedNamespaceOptions sendAvailabilityOptions =
    new SendAvailabilityPairedNamespaceOptions(secondaryNamespaceManager, secondary);
primary.PairNamespaceAsync(sendAvailabilityOptions).Wait();
```

によって返されるタスクの場合、 [PairNamespaceAsync][] メソッドが完了すると、すべて設定はされ、使用できるようにします。 タスクが返る前に、ペアリングが正しく動作するために必要なすべてのバックグラウンド作業を完了していない可能性があります。 その結果、タスクが返るまではメッセージの送信を開始できません。 正しくない資格情報などの障害が発生した場合や、バックグラウンド キューの作成に失敗した場合、タスクが完了するとそれらの例外がスローされます。 タスクが返されるは、キューが発見またはを調べることによって作成されたことを確認、 [BacklogQueueCount][] プロパティを [SendAvailabilityPairedNamespaceOptions][] インスタンス。 前のコードでは、その操作は次のようになります。

```
if (sendAvailabilityOptions.BacklogQueueCount < 1)
{
    // Handle case where no queues were created.
}
```

## 次のステップ

Service Bus での非同期メッセージングの基本を学習できました、これでは、に関する詳細を確認 [paired namespaces and cost implications]します。

  [ServerBusyException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.serverbusyexception.aspx
  [System.TimeoutException]: https://msdn.microsoft.com/library/system.timeoutexception.aspx
  [MessagingException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
  [Best Practices for Insulating Service Bus Applications Against Service Bus Outages and Disasters]: service-bus-outages-disasters.md
  [Microsoft.ServiceBus.Messaging.MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [MessageReceiver]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx
  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx
  [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.pairednamespaceoptions.aspx
  [MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [SendAvailabilityPairedNamespaceOptions]:https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.aspx
  [NamespaceManager]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx
  [PairNamespaceAsync]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.pairnamespaceasync.aspx
  [EnableSyphon]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.enablesyphon.aspx
  [System.TimeSpan.Zero]: https://msdn.microsoft.com/library/azure/system.timespan.zero.aspx
  [IsTransient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.istransient.aspx
  [UnauthorizedAccessException]: https://msdn.microsoft.com/library/azure/system.unauthorizedaccessexception.aspx
  [BacklogQueueCount]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.backlogqueuecount.aspx
  [paired namespaces and cost implications]: service-bus-paired-namespaces.md
