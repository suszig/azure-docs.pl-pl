## デバイスからクラウドへのメッセージの処理

このセクションでは、IoT Hub からのデバイスからクラウドへのメッセージを処理する Windows コンソール アプリを作成します。 Iot Hub を公開、 [Event Hubs][Event Hubs Overview]-デバイスからクラウドへのメッセージを読み取るに互換性のあるエンドポイント。 このチュートリアルでは [EventProcessorHost] コンソール アプリでこれらのメッセージを処理します。 Event Hubs からメッセージを処理する方法の詳細についてを参照してください、 [Get Started with Event Hubs] チュートリアルです。

データ ポイント メッセージの信頼性の高いストレージを実装する場合や、対話型メッセージを転送する場合の主な課題は、Event Hubs のイベント処理がその進行状況をチェックポイントとするメッセージ コンシューマーに依存することです。 さらに、Event Hubs からの読み取り時に高スループットを実現するには、大規模なバッチでチェックポイントの実行が必要になるため、大量のメッセージの処理が重複する可能性があります。 このチュートリアルでは、イベント プロセッサ ホストのチェックポイントを使用して、Azure Storage の書き込みと Service Bus の重複除去期間を同期する方法を示します。

確実にメッセージを Azure ストレージに書き込み、するためにも行うを使用して、 [ブロック blob][Azure Block Blobs] 個々 のブロックのコミットの機能です。 イベント プロセッサでは、チェックポイントの実行時間になるまで (つまり、蓄積されたバッファーが最大ブロック サイズの 4 Mb より大きくなるか、Service Bus の重複除去期間が経過するまで)、メッセージがメモリに蓄積されます。 その後、チェックポイント処理の前に、新しいブロックが BLOB にコミットされます。

イベント プロセッサでは、ブロック ID として Event Hubs のメッセージ オフセットを使用します。 これにより、コミットされたブロックとチェックポイント間でクラッシュが発生する可能性がないかどうかを注意して、新しいブロックをストレージにコミットする前に、重複除去を確認できます。

> [AZURE.NOTE] このチュートリアルでは、単一のストレージ アカウントを使用して IoT Hub から取得したすべてのメッセージを書き込みます。 参照してください [Azure Storage scalability Guidelines] ソリューションに複数の Azure ストレージ アカウントが必要があるかを決定します。

対話型メッセージ処理での重複を避けるために、Service Bus の重複除去機能を使用します。 各対話型メッセージに一意の `MessageId` をスタンプすることで、Service Bus は、指定された重複除去期間で、同じ `MessageId` を持つ 2 つのメッセージが受信側に配信されないようにすることができます。 この重複除去を、Service Bus キューによって提供されるメッセージごとの完了セマンティクスと共に使用することで、対話型メッセージの信頼性の高い処理が非常に簡単になります。

重複除去期間外にメッセージが再送信されないように、イベント プロセッサ ホストのチェックポイント処理メカニズムを Service Bus キューの重複除去期間に同期します。 これは、チェックポイントを期間 (このチュートリアルでは 1 時間) が経過するたびに少なくとも 1 回強制実行することで実現されます。

> [AZURE.NOTE] このチュートリアルでは、対話型のすべてのメッセージは、IoT Hub から取得したプロセスに 1 つのパーティション分割された Service Bus キューを使用します。 ソリューションの必要性に応じてを参照して [Service Bus documentation] Service Bus キューを使用する方法の詳細。

### Azure ストレージ アカウントと Service Bus キューのプロビジョニング
使用するために [EventProcessorHost], 、Azure ストレージ アカウントが必要です。 既存のものを使用したり、指示に従います [About Azure Storage] 新規に作成します。 ストレージ アカウント接続文字列はメモしておいてください。

また、Service Bus キューで対話型メッセージの信頼性の高い処理を有効にする必要もあります。 できるキューを作成するプログラムを使用して、1 時間の重複除去ウィンドウとで説明したよう [サービス バス キューの使用方法][Service Bus Queue], を使用して、または、 [Azure portal], 、以下の手順。

1. をクリックして **新規** 、画面左下に、[ **App Services**, 、し **Service Bus**, 、し **キュー**, 、し、 **カスタム**, 、名前を選択 **d2ctutorial**, 、1 時間の重複除去ウィンドウを選択します。

    ![][30]

2. キューの名前をクリックして (**d2ctutorial**)、し **構成**, と呼ばれる 1 つ、2 つの共有アクセス ポリシーを作成し、 **送信** と **送信** アクセス許可、およびと呼ばれる 1 つ **リッスン** と **リッスン** アクセス許可。 クリックして **保存** したら、下部にあります。

    ![][31]

3. をクリックして **ダッシュ ボード** し、上部に **接続情報** 、下部にある、2 つの接続文字列をメモしています。

    ![][32]

### イベント プロセッサの作成

1. 現在の Visual Studio ソリューション] をクリックして **ファイルに追加]-> [プロジェクト]-> [** 、新しい Visual c# のデスクトップ アプリを使用してプロジェクトを作成する、 **コンソール アプリケーション** プロジェクト テンプレートです。 プロジェクトに名前を **ProcessDeviceToCloudMessages**します。

    ![][10]

2. ソリューション エクスプ ローラーでソリューションを右クリックし、をクリックし、 **NuGet パッケージの管理**します。

     **NuGet パッケージの管理** ] ダイアログ ボックスが表示されます。

3. 検索 `WindowsAzure.ServiceBus`, 、] をクリックして **インストール**, 、使用条件に同意します。

    これをダウンロード、インストール、およびへの参照を追加、 [Azure Service Bus NuGet パッケージ](https://www.nuget.org/packages/WindowsAzure.ServiceBus), 、すべての依存関係とします。

4. 検索 `Microsoft Azure Service Bus Event Hub - EventProcessorHost`, 、] をクリックして **インストール**, 、使用条件に同意します。

    これによって、[Azure Service Bus Event Hub - EventProcessorHost NuGet パッケージ](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost)への参照がすべての依存関係と共にダウンロード、インストール、追加されます。

5. 右クリックし、 **ProcessDeviceToCloudMessages** プロジェクトをクリックして **追加**, 、] をクリックし、 **クラス**します。 新しいクラスの名前を **StoreEventProcessor**, 、] をクリックし、 **OK** クラスを作成します。

6. StoreEventProcessor.cs ファイルの先頭に次のステートメントを追加します。

        using System.IO;
        using System.Diagnostics;
        using System.Security.Cryptography;
        using Microsoft.ServiceBus.Messaging;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;

    次に、クラスの本文に次のコードを置き換えます。

        class StoreEventProcessor : IEventProcessor
        {
            private const int MAX_BLOCK_SIZE = 4 * 1024 * 1024;
            public static string StorageConnectionString;
            public static string ServiceBusConnectionString;

            private CloudBlobClient blobClient;
            private CloudBlobContainer blobContainer;
            private QueueClient queueClient;

            private long currentBlockInitOffset;
            private MemoryStream toAppend = new MemoryStream(MAX_BLOCK_SIZE);

            private Stopwatch stopwatch;
            private TimeSpan MAX_CHECKPOINT_TIME = TimeSpan.FromHours(1);

            public StoreEventProcessor()
            {
                var storageAccount = CloudStorageAccount.Parse(StorageConnectionString);
                blobClient = storageAccount.CreateCloudBlobClient();
                blobContainer = blobClient.GetContainerReference("d2ctutorial");
                queueClient = QueueClient.CreateFromConnectionString(ServiceBusConnectionString, "d2ctutorial");
            }

            Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
            {
                Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
                return Task.FromResult<object>(null);
            }

            Task IEventProcessor.OpenAsync(PartitionContext context)
            {
                Console.WriteLine("StoreEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);

                if (!long.TryParse(context.Lease.Offset, out currentBlockInitOffset))
                {
                    currentBlockInitOffset = 0;
                }
                stopwatch = new Stopwatch();
                stopwatch.Start();

                return Task.FromResult<object>(null);
            }

            async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
            {
                foreach (EventData eventData in messages)
                {
                    byte[] data = eventData.GetBytes();

                    if (eventData.Properties.ContainsKey("messageType") && (string) eventData.Properties["messageType"] == "interactive")
                    {
                        var messageId = (string) eventData.SystemProperties["message-id"];

                        var queueMessage = new BrokeredMessage(new MemoryStream(data));
                        queueMessage.MessageId = messageId;
                        queueMessage.Properties["messageType"] = "interactive";
                        await queueClient.SendAsync(queueMessage);

                        WriteHighlightedMessage(string.Format("Received interactive message: {0}", messageId));
                        continue;
                    }

                    if (toAppend.Length + data.Length > MAX_BLOCK_SIZE || stopwatch.Elapsed > MAX_CHECKPOINT_TIME)
                    {
                        await AppendAndCheckpoint(context);
                    }
                    await toAppend.WriteAsync(data, 0, data.Length);

                    Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                        context.Lease.PartitionId, Encoding.UTF8.GetString(data)));
                }
            }

            private async Task AppendAndCheckpoint(PartitionContext context)
            {
                var blockIdString = String.Format("startSeq:{0}", currentBlockInitOffset.ToString("0000000000000000000000000"));
                var blockId = Convert.ToBase64String(ASCIIEncoding.ASCII.GetBytes(blockIdString));
                toAppend.Seek(0, SeekOrigin.Begin);
                byte[] md5 = MD5.Create().ComputeHash(toAppend);
                toAppend.Seek(0, SeekOrigin.Begin);

                var blobName = String.Format("iothubd2c_{0}", context.Lease.PartitionId);
                var currentBlob = blobContainer.GetBlockBlobReference(blobName);

                if (await currentBlob.ExistsAsync())
                {
                    await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
                    var blockList = await currentBlob.DownloadBlockListAsync();
                    var newBlockList = new List<string>(blockList.Select(b => b.Name));

                    if (newBlockList.Count() > 0 && newBlockList.Last() != blockId)
                    {
                        newBlockList.Add(blockId);
                        WriteHighlightedMessage(String.Format("Appending block id: {0} to blob: {1}", blockIdString, currentBlob.Name));
                    }
                    else
                    {
                        WriteHighlightedMessage(String.Format("Overwriting block id: {0}", blockIdString));
                    }
                    await currentBlob.PutBlockListAsync(newBlockList);
                }
                else
                {
                    await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
                    var newBlockList = new List<string>();
                    newBlockList.Add(blockId);
                    await currentBlob.PutBlockListAsync(newBlockList);

                    WriteHighlightedMessage(String.Format("Created new blob", currentBlob.Name));
                }

                toAppend.Dispose();
                toAppend = new MemoryStream(MAX_BLOCK_SIZE);

                // checkpoint.
                await context.CheckpointAsync();
                WriteHighlightedMessage(String.Format("Checkpointed partition: {0}", context.Lease.PartitionId));

                currentBlockInitOffset = long.Parse(context.Lease.Offset);
                stopwatch.Restart();
            }

            private void WriteHighlightedMessage(string message)
            {
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine(message);
                Console.ResetColor();
            }
        }

    このクラスにより呼び出される、 **EventProcessorHost** IoT Hub と blob コンテナーと Service Bus キューに対話型のメッセージの転送にメッセージが確実に格納するためのメイン ロジックを実装から受信したデバイスからクラウドへのメッセージを処理します。
     `OpenAsync()` メソッド、初期化、  `currentBlockInitOffset` 最初にこのイベントは、プロセッサによって読み取られるメッセージの現在のオフセットを追跡する変数 (各プロセッサでは 1 つのパーティションを担当することに注意してください)。

    `ProcessEventsAsync()` メソッドは、次のように処理される IoT Hub からメッセージのバッチを受信します。 対話型メッセージは Service Bus キューに送信されますが、データ ポイント メッセージは `toAppend` というメモリ バッファーに追加されます。 メモリ バッファーがブロック制限 (つまり、4 Mb) に達したか、前回のチェックポイント以降に Service Bus の重複除去期間 (このチュートリアルでは 1 時間) が経過した場合に、チェックポイントがトリガーされます。

    メソッド `AppendAndCheckpoint()` は最初に、追加されるブロックの blockId を生成します。 Azure Storage ではすべてのブロック ID が同じ長さである必要があるため、オフセットには先行ゼロが埋め込まれます (`currentBlockInitOffset.ToString("0000000000000000000000000")`)。 次に、この ID を持つブロックが既に BLOB にある場合は、メソッドは現在のもので上書きします。

> [AZURE.NOTE] コードを簡略化は、このチュートリアルは、メッセージを保存するのにパーティションごとに 1 つの blob ファイルを使用します。 実際のソリューションではファイル ローリングを実装します。その場合、特定のサイズ (Azure BLOB は最大 195 Gb にすることができます) に達したときか、一定時間 (`fileName_{partitionId}_{date-time}` など) の後に、追加ファイルを作成します。

7.  **プログラム** クラスで、次の追加を `using` ステートメントを先頭にします。

        using Microsoft.ServiceBus.Messaging;

    次に、変更、 **Main** メソッドを **プログラム** IoT Hub を置き換えて、次のようにクラス **iothubowner** 接続文字列 (から [入門 IoT Hub] チュートリアル)、ストレージ接続文字列とを持つサービス バス接続文字列 **送信** という名前のキューのアクセス許可 **d2ctutorial**:

        static void Main(string[] args)
        {
            string iotHubConnectionString = "{iot hub connection string}";
            string iotHubD2cEndpoint = "messages/events";
            StoreEventProcessor.StorageConnectionString = "{storage connection string}";
            StoreEventProcessor.ServiceBusConnectionString = "{service bus send connection string}";

            string eventProcessorHostName = Guid.NewGuid().ToString();
            EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, iotHubD2cEndpoint, EventHubConsumerGroup.DefaultGroupName, iotHubConnectionString, StoreEventProcessor.StorageConnectionString, "messages-events");
            Console.WriteLine("Registering EventProcessor...");
            eventProcessorHost.RegisterEventProcessorAsync<StoreEventProcessor>().Wait();

            Console.WriteLine("Receiving. Press enter key to stop worker.");
            Console.ReadLine();
            eventProcessorHost.UnregisterEventProcessorAsync().Wait();
        }

> [AZURE.NOTE] わかりやすくするために、このチュートリアルの 1 つのインスタンスを使用して [EventProcessorHost]します。 参照してください [Event Hubs Programming Guide] とデバイスからクラウドへのメッセージの処理の詳細については [デバイスからクラウドへのメッセージを処理] に関するチュートリアルです。

## 対話型メッセージの受信
このセクションでは、Service Bus キューから対話型メッセージを受信する Windows コンソール アプリケーションを作成します。 参照してください [Service Bus での多階層アプリケーションをビルド][] サービス バスを使用してソリューションを構築する方法についての詳細。

1. 現在の Visual Studio ソリューションで作成、新しい Visual c# のデスクトップ アプリ プロジェクトを使用して、 **コンソール アプリケーション** プロジェクト テンプレートです。 プロジェクトに名前を **ProcessD2cInteractiveMessages**します。

2. ソリューション エクスプ ローラーでソリューションを右クリックし、をクリックし、 **... ソリューションの NuGet パッケージの管理**します。

    [NuGet パッケージの管理] ウィンドウが表示されます。

3. 検索 `WindowsAzure.Service Bus`, 、] をクリックして **インストール**, 、使用条件に同意します。

    これをダウンロード、インストール、およびへの参照を追加、 [Azure Service Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus), 、すべての依存関係とします。

4. 次の追加 `using` の上部にあるステートメント、 **Program.cs** ファイル。

        using System.IO;
        using Microsoft.ServiceBus.Messaging;

5. 次の行を最後に、追加、 **Main** メソッドを持つ接続文字列の置換 **リッスン** という名前のキューのアクセス許可 **d2ctutorial**:

        Console.WriteLine("Process D2C Interactive Messages app\n");

        string connectionString = "{service bus listen connection string}";
        QueueClient Client = QueueClient.CreateFromConnectionString(connectionString, "d2ctutorial");

        OnMessageOptions options = new OnMessageOptions();
        options.AutoComplete = false;
        options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

        Client.OnMessage((message) =>
        {
            try
            {
                var bodyStream = message.GetBody<Stream>();
                bodyStream.Position = 0;
                var bodyAsString = new StreamReader(bodyStream, Encoding.ASCII).ReadToEnd();

                Console.WriteLine("Received message: {0} messageId: {1}", bodyAsString, message.MessageId);

                message.Complete();
            }
            catch (Exception)
            {
                message.Abandon();
            }
        }, options);

        Console.WriteLine("Receiving interactive messages from SB queue...");
        Console.WriteLine("Press any key to exit.");
        Console.ReadLine();

<!-- Links -->

[About Azure Storage]: ../storage/storage-create-storage-account.md#create-a-storage-account

[Azure IoT - Service SDK NuGet package]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

[Get Started with Event Hubs]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[IoT Hub Developer Guide - Identity Registry]: iot-hub-devguide.md#identityregistry

[Azure Storage scalability Guidelines]: ../storage/storage-scalability-targets.md

[Azure Block Blobs]: https://msdn.microsoft.com/library/azure/ee691964.aspx

[Service Bus documentation]: ../service-bus/service-bus-dotnet-how-to-use-queues.md

[Event Hubs Overview]: ../event-hubs/event-hubs-overview.md
[Scaled out event processing]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Event-Hub-45f43fc3

[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx

[Event Hubs Programming Guide]: ../event-hubs/event-hubs-programming-guide.md

[Azure preview portal]: https://portal.azure.com/

[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[Azure Portal]: https://manage.windowsazure.com/

[Service Bus Queue]: ../service-bus/service-bus-dotnet-how-to-use-queues.md

[Build multi-tier applications with Service Bus]: ../service-bus/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md


<!-- Images -->
[10]: ./media/iot-hub-process-d2c-cloud-csharp/create-identity-csharp1.png
[12]: ./media/iot-hub-getstarted-cloud-csharp/create-identity-csharp3.png

[20]: ./media/iot-hub-getstarted-cloud-csharp/create-storage1.png
[21]: ./media/iot-hub-getstarted-cloud-csharp/create-storage2.png
[22]: ./media/iot-hub-getstarted-cloud-csharp/create-storage3.png

[30]: ./media/iot-hub-process-d2c-cloud-csharp/createqueue2.png
[31]: ./media/iot-hub-process-d2c-cloud-csharp/createqueue3.png
[32]: ./media/iot-hub-process-d2c-cloud-csharp/createqueue4.png

