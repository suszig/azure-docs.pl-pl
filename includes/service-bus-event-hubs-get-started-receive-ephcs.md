## EventProcessorHost を使用したメッセージの受信

[EventProcessorHost][] 永続的なチェックポイントの管理によって Event Hubs からのイベントの受信を簡素化する .NET クラスは、並列はそれらの Event Hubs から受信します。 使用して [EventProcessorHost][], 、さまざまなノードでホストされている場合でも、複数の受信側間でイベントを分割することができます。 この例は、使用する方法を示しています。 [EventProcessorHost][] の受信者は 1 つです。  [イベントの処理を拡大/縮小][] サンプルを使用する方法を示します [EventProcessorHost][] 複数の受信者とします。

使用するために [EventProcessorHost][], 、する必要があります、 [Azure ストレージ アカウント][]:

1. ログオン、 [Azure クラシック ポータル][], 、] をクリック **新規** 、画面の下部にあります。

2. をクリックして **Data Services**, 、し **ストレージ**, 、し **簡易作成**, 、ストレージ アカウントの名前を入力します。 目的のリージョンを選択し、クリックして **ストレージ アカウントの作成**します。

    ![][11]

3. 新しく作成されたストレージ アカウントをクリックし、クリックして **アクセス キーの管理**:

    ![][12]

    このチュートリアルの後で使用するため、アクセス キーをメモしておきます。

4. Visual Studio で、新しい Visual c# のデスクトップ アプリを使用してプロジェクトを作成、 **コンソール アプリケーション** プロジェクト テンプレートです。 プロジェクトに名前を **受信者**します。

    ![][14]

5. ソリューション エクスプ ローラーでソリューションを右クリックし、をクリックし、 **NuGet パッケージの管理**します。

     **NuGet パッケージの管理** ] ダイアログ ボックスが表示されます。

6. 検索 `Microsoft Azure Service Bus Event Hub - EventProcessorHost`, 、] をクリックして **インストール**, 、使用条件に同意します。

    ![][13]

    これによって、[Azure Service Bus Event Hub - EventProcessorHost NuGet パッケージ](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost)への参照がすべての依存関係と共にダウンロード、インストール、追加されます。

7. 右クリックし、 **受信者** プロジェクトをクリックして **追加**, 、] をクリックし、 **クラス**します。 新しいクラスの名前を **SimpleEventProcessor**, 、] をクリックし、 **OK** クラスを作成します。

8. SimpleEventProcessor.cs ファイルの先頭に次のステートメントを追加します。

    ```
    using Microsoft.ServiceBus.Messaging;
    using System.Diagnostics;
    using System.Threading.Tasks;
    ```

    次に、クラスの本文に次のコードを置き換えます。

    ```
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());

                Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                    context.Lease.PartitionId, data));
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }
    ````

    このクラスにより呼び出される、 **EventProcessorHost** Event Hub から受信したイベントを処理します。 なお、 `SimpleEventProcessor` クラスは、ストップウォッチを使用してで定期的にチェックポイント メソッドを呼び出します、 **EventProcessorHost** コンテキスト。 これにより、受信側を再起動すると、処理の作業の 5 分以内に機能が失われます。

9.  **プログラム** クラスで、次の追加を `using` ステートメントをファイルの先頭にします。

    ```
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.Threading;
    using System.Threading.Tasks;
    ```

    次に、`Program` クラスの `Main` メソッドを以下のとおりに変更して、前のセクションでコピーした Event Hub 名と接続文字列、ストレージ アカウントとキーを代入します。

    ```
    static void Main(string[] args)
    {
      string eventHubConnectionString = "{event hub connection string}";
      string eventHubName = "{event hub name}";
      string storageAccountName = "{storage account name}";
      string storageAccountKey = "{storage account key}";
      string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

      string eventProcessorHostName = Guid.NewGuid().ToString();
      EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
      Console.WriteLine("Registering EventProcessor...");
      eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>().Wait();

      Console.WriteLine("Receiving. Press enter key to stop worker.");
      Console.ReadLine();
      eventProcessorHost.UnregisterEventProcessorAsync().Wait();
    }
    ````

> [AZURE.NOTE] このチュートリアルの 1 つのインスタンスを使用して [EventProcessorHost][]します。 スループットを増加することをお勧めの複数のインスタンスを実行すること [EventProcessorHost][], のように、 [イベントの処理を拡大/縮小][] サンプルです。 このような場合、受信したイベントの負荷を分散するために、さまざまなインスタンスが自動的に連携します。 各プロセスの複数の受信側の場合 *すべて* 使用する必要があります、イベント、 **ConsumerGroup** 概念です。 さまざまなコンピューターからイベントを受信するときに名を指定すると便利なあります [EventProcessorHost][] インスタンスに基づいてコンピューター (またはロール) で展開されます。 これらのトピックの詳細については、次を参照してください。、 [Event Hubs の概要][] と [Event Hub プログラミング ガイド][] トピックです。

<!-- Links -->
[Event Hubs Overview]: event-hubs-overview.md
[Event Hubs Programming Guide]: event-hubs-programming-guide.md
[Scaled out event processing]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[Azure Storage account]: ../storage/storage-create-storage-account.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[Azure classic portal]: http://manage.windowsazure.com

<!-- Images -->

[11]: ./media/service-bus-event-hubs-getstarted/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-getstarted/create-eph-csharp3.png
[13]: ./media/service-bus-event-hubs-getstarted/create-eph-csharp1.png
[14]: ./media/service-bus-event-hubs-getstarted/create-sender-csharp1.png



