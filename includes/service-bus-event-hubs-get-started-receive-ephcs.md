## EventProcessorHost を使用したメッセージの受信

[EventProcessorHost:operator[]][] 永続的なチェックポイントの管理によって Event Hubs からのイベントの受信を簡素化する .NET クラスは、並列はそれらの Event Hubs から受信します。 使用して [EventProcessorHost:operator[]][], 、さまざまなノードでホストされている場合でも、複数の受信側間でイベントを分割することができます。 この例は、使用する方法を示しています。 [EventProcessorHost []][] 受信者は 1 つのです。  [スケール アウト イベント処理][] サンプルを使用する方法を示します [EventProcessorHost:operator[]][] 複数の受信者とします。

使用するために [EventProcessorHost:operator[]][], 、する必要があります、 [Azure ストレージ アカウントを][]:

1. ログオン、 [Azure クラシック ポータルの [][], 、] をクリック **新規** 、画面の下部にあります。

2. **[Data Services]**、**[Storage]**、**[簡易作成]** の順にクリックし、ストレージ アカウントの名前を入力します。 目的のリージョンを選択し、**[ストレージ アカウントの作成]** をクリックします。

    ![][11]

3. 新しく作成したストレージ アカウントをクリックし、**[アクセス キーの管理]** をクリックします。

    ![][12]

    このチュートリアルの後で使用するため、アクセス キーをメモしておきます。

4. Visual Studio で、新しい Visual c# のデスクトップ アプリを使用してプロジェクトを作成、 **コンソール アプリケーション** プロジェクト テンプレートです。 プロジェクトの名前として「**Receiver**」と入力します。

    ![][14]

5. ソリューション エクスプローラーでソリューションを右クリックし、**[NuGet パッケージの管理]** をクリックします。

    **[NuGet パッケージの管理]** ダイアログ ボックスが表示されます。

6. `Microsoft Azure Service Bus Event Hub - EventProcessorHost` を検索し、**[インストール]** をクリックして、使用条件に同意します。

    ![][13]

    これをダウンロード、インストール、およびへの参照を追加、 [Azure Service Bus Event Hub - EventProcessorHost NuGet パッケージ](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), 、すべての依存関係とします。

7. **[Receiver]** プロジェクトを右クリックし、**[追加]**、**[クラス]** の順にクリックします。 クラスの名前として「**SimpleEventProcessor**」と入力し、**[OK]** をクリックしてクラスを作成します。

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
    ```

    このクラスは、**EventProcessorHost** から呼び出されて、Event Hub から受信したイベントを処理します。 なお、 `SimpleEventProcessor` クラスは、ストップウォッチを使用してで定期的にチェックポイント メソッドを呼び出します、 **EventProcessorHost** コンテキスト。 これにより、受信側を再起動すると、処理の作業の 5 分以内に機能が失われます。

9. **プログラム** クラスで、次の追加を `を使用して` ステートメントをファイルの先頭にします。

    ```
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.Threading;
    using System.Threading.Tasks;
    ```

    次に、変更、 `Main` メソッドで、 `プログラム` Event Hub 名と接続文字列とストレージ アカウントと、前のセクションでコピーしたキーに置き換えて次のようにクラスします。

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
    ```

> [AZURE.NOTE] このチュートリアルの 1 つのインスタンスを使用して [EventProcessorHost:operator[]][]します。 スループットを増加することをお勧めの複数のインスタンスを実行する [EventProcessorHost][], のように、 [スケール アウト イベント処理][] サンプルです。 このような場合、受信したイベントの負荷を分散するために、さまざまなインスタンスが自動的に連携します。 複数の受信側でぞれぞれ*すべて*のイベントを処理する場合、**ConsumerGroup** 概念を使用する必要があります。 さまざまなコンピューターからイベントを受信するときに名を指定すると便利なあります [EventProcessorHost][] インスタンスに基づいてコンピューター (またはロール) で展開されます。 これらのトピックの詳細については、次を参照してください。、 [Event Hubs の概要 []][] と [イベント ハブのプログラミング ガイド []][] トピックです。





[event hubs overview]: event-hubs-overview.md 
[event hubs programming guide]: event-hubs-programming-guide.md 
[scaled out event processing]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3 
[azure storage account]: ../storage/storage-create-storage-account.md 
[eventprocessorhost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx 
[azure classic portal]: http://manage.windowsazure.com 
[11]: ./media/service-bus-event-hubs-getstarted/create-eph-csharp2.png 
[12]: ./media/service-bus-event-hubs-getstarted/create-eph-csharp3.png 
[13]: ./media/service-bus-event-hubs-getstarted/create-eph-csharp1.png 
[14]: ./media/service-bus-event-hubs-getstarted/create-sender-csharp1.png 

