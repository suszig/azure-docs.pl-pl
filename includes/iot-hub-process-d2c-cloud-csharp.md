## デバイスからクラウドへのメッセージの処理

このセクションでは、IoT Hub からのデバイスからクラウドへのメッセージを処理する Windows コンソール アプリを作成します。 Iot Hub を公開、 [Event Hubs ][event hubs overview]-デバイスからクラウドへのメッセージを読み取るに互換性のあるエンドポイント。 このチュートリアルでは、コンソール アプリケーションでこれらのメッセージを処理するのに [EventProcessorHost] を使用します。 Event Hubs からメッセージを処理する方法の詳細については、[Event Hubs では、「」チュートリアルを参照してください。

データ ポイント メッセージの信頼性の高いストレージを実装する場合や、対話型メッセージを転送する場合の主な課題は、Event Hubs のイベント処理がその進行状況をチェックポイントとするメッセージ コンシューマーに依存することです。 さらに、Event Hubs からの読み取り時に高スループットを実現するには、大規模なバッチでチェックポイントの実行が必要になるため、大量のメッセージの処理が重複する可能性があります。 このチュートリアルでは、イベント プロセッサ ホストのチェックポイントを使用して、Azure Storage の書き込みと Service Bus の重複除去期間を同期する方法を示します。

確実にメッセージを Azure ストレージに書き込み、するためにも行うを使用して、 [ブロック blob の ][azure block blobs] 個々 のブロックのコミットの機能です。 イベント プロセッサでは、チェックポイントの実行時間になるまで (つまり、蓄積されたバッファーが最大ブロック サイズの 4 Mb より大きくなるか、Service Bus の重複除去期間が経過するまで)、メッセージがメモリに蓄積されます。 その後、チェックポイント処理の前に、新しいブロックが BLOB にコミットされます。

イベント プロセッサでは、ブロック ID として Event Hubs のメッセージ オフセットを使用します。 これにより、コミットされたブロックとチェックポイント間でクラッシュが発生する可能性がないかどうかを注意して、新しいブロックをストレージにコミットする前に、重複除去を確認できます。
> [AZURE.NOTE] このチュートリアルでは 1 つのストレージ アカウントを使用して、IoT Hub から取得したすべてのメッセージを書き込みます。 ソリューションでは、複数の Azure ストレージ アカウントが必要なかどうかを決定する [Azure ストレージのスケーラビリティのガイドライン] を参照してください。

対話型メッセージ処理での重複を避けるために、Service Bus の重複除去機能を使用します。 ユニークな各対話型のメッセージを書き込むことによって `MessageId`, 、Service Bus は、指定された重複除去の時間帯に 2 メッセージなしでは、同じすれば `MessageId` 、受信者に配信されます。 この重複除去を、Service Bus キューによって提供されるメッセージごとの完了セマンティクスと共に使用することで、対話型メッセージの信頼性の高い処理が非常に簡単になります。

重複除去期間外にメッセージが再送信されないように、イベント プロセッサ ホストのチェックポイント処理メカニズムを Service Bus キューの重複除去期間に同期します。 これは、チェックポイントを期間 (このチュートリアルでは 1 時間) が経過するたびに少なくとも 1 回強制実行することで実現されます。
> [AZURE.NOTE] このチュートリアルでは 1 つのパーティション分割された Service Bus キューを使用して、IoT Hub から取得した対話型メッセージをすべて処理します。 ソリューションのニーズに応じて、サービス バス キューの使用方法の詳細についての [Service Bus のドキュメント] を参照してください。

### Azure ストレージ アカウントと Service Bus キューのプロビジョニング

[EventProcessorHost] を使用するのには、Azure ストレージ アカウントが必要です。 、既存のものを使用したり、新しいを作成するのには、[Azure ストレージに関する] の手順に従います。 ストレージ アカウント接続文字列はメモしておいてください。

また、Service Bus キューで対話型メッセージの信頼性の高い処理を有効にする必要もあります。 できるキューを作成するプログラムを使用して、1 時間の重複除去ウィンドウとで説明したよう [Service Bus キュー ][service bus queue], 、ポータルを使用して、[Azure]、または以下の手順。

1. 左下隅にある **[新規]** をクリックしてから、**[App Services]**、**[Service Bus]**、**[キュー]**、**[カスタム]** の順にクリックし、**d2ctutorial** という名前を選択してから 1 時間の重複除去期間を選択します。

    ![][30]

2. キューの名前 (**d2ctutorial**) をクリックしてから **[構成]** をクリックし、2 つの共有アクセス ポリシー (1 つは **Send**アクセス許可を持つ **send**、もう 1 つは **Listen** アクセス許可を持つ **listen**) を作成します。 完了したら、下部の **[保存]** をクリックします。

    ![][31]

3. 上部の **[ダッシュボード]** をクリックしてから、下部の **[接続情報]** をクリックし、2 つの接続文字列をメモします。

    ![][32]

### イベント プロセッサの作成

1. 現在の Visual Studio ソリューション] をクリックして **ファイルに追加]-> [プロジェクト]-> [** 、新しい Visual c# のデスクトップ アプリを使用してプロジェクトを作成する、 **コンソール アプリケーション** プロジェクト テンプレートです。 プロジェクトに **ProcessDeviceToCloudMessages** という名前を付けます。

    ![][10]

2. ソリューション エクスプローラーでソリューションを右クリックし、**[NuGet パッケージの管理]** をクリックします。

    **[NuGet パッケージの管理]** ダイアログ ボックスが表示されます。

3. `WindowsAzure.ServiceBus` を検索し、**[インストール]** をクリックし、使用条件に同意します。

    これをダウンロード、インストール、およびへの参照を追加、 [Azure Service Bus NuGet パッケージ](https://www.nuget.org/packages/WindowsAzure.ServiceBus), 、すべての依存関係とします。

4. `Microsoft Azure Service Bus Event Hub - EventProcessorHost` を検索し、**[インストール]** をクリックして、使用条件に同意します。

    これをダウンロード、インストール、およびへの参照を追加、 [Azure Service Bus Event Hub - EventProcessorHost NuGet パッケージ](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), 、すべての依存関係とします。

5. **[ProcessDeviceToCloudMessages]** プロジェクトを右クリックし、**[追加]** をクリックしてから **[クラス]** をクリックします。 新しいクラスに **SimpleEventProcessor** という名前を付けてから、**[OK]** をクリックしてクラスを作成します。

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

 このクラスは、IoT Hub から受信したデバイスからクラウドへのメッセージを処理するために **EventProcessorHost** によって呼び出され、BLOB コンテナーにメッセージを確実に格納し、対話型メッセージを Service Bus キューに転送するためのメイン ロジックを実装します。
 `OpenAsync()` メソッド、初期化、  `currentBlockInitOffset` 最初にこのイベントは、プロセッサによって読み取られるメッセージの現在のオフセットを追跡する変数 (各プロセッサでは 1 つのパーティションを担当することに注意してください)。

 `ProcessEventsAsync()` メソッドは、次のように処理される IoT Hub からメッセージのバッチを受け取ります。 データ ポイントのメッセージと呼ばれる memoryBuffer に追加されます。 中に、対話型のメッセージが Service Bus キューに送信されます `toAppend`します。 メモリ バッファーがブロック制限 (つまり、4 Mb) に達したか、前回のチェックポイント以降に Service Bus の重複除去期間 (このチュートリアルでは 1 時間) が経過した場合に、チェックポイントがトリガーされます。

 メソッド `AppendAndCheckpoint()` 最初に追加されるブロックのブロック Id が生成されます。 オフセットがの先頭にゼロが埋め込まれた Azure ストレージでは、すべてのブロック id が同じ長さである必要がある、ため (`currentBlockInitOffset.ToString("0000000000000000000000000")`)。 次に、この ID を持つブロックが既に BLOB にある場合は、メソッドは現在のもので上書きします。

> [AZURE.NOTE] コードを簡略化するために、このチュートリアルでは、メッセージを格納するパーティションごとに 1 つの BLOB ファイルを使用します。 特定のサイズ (メモから Azure blob が多くて 195 Gb であることができます) に到達したときに、追加のファイルを作成することで、または一定の期間後、ファイルのロール、実際のソリューションを実装 (例: `fileName_ {partitionId} _ {日時}`)。

7. **Program** クラスには、上部に次の `using` ステートメントを追加します。

     using Microsoft.ServiceBus.Messaging;

 次に、以下のように **Main** メソッドを **Program** クラスに変更します。その場合、IoT Hub の **iothubowner** 接続文字列 (「IoT Hub の概要」チュートリアルを参照)、ストレージ接続文字列、および Service Bus の接続文字列を **d2ctutorial** という名前のキューの **Send** アクセス許可に置き換えます。

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


> [AZURE.NOTE] このチュートリアルでは、わかりやすくするために、[EventProcessorHost] の 1 つのインスタンスを使用します。 [Event Hub プログラミング ガイド] を参照してくださいとデバイスからクラウドへのメッセージの処理の詳細については [デバイスからクラウドへのメッセージを処理] に関するチュートリアルです。

## 対話型メッセージの受信

このセクションでは、Service Bus キューから対話型メッセージを受信する Windows コンソール アプリケーションを作成します。 参照してください [サービス バスので多層アプリケーションを構築][] サービス バスを使用してソリューションを構築する方法についての詳細。

1. 現在の Visual Studio ソリューションで作成、新しい Visual c# のデスクトップ アプリ プロジェクトを使用して、 **コンソール アプリケーション** プロジェクト テンプレートです。 プロジェクトに **ProcessD2cInteractiveMessages** という名前を付けます。

2. ソリューション エクスプローラーでソリューションを右クリックし、**[ソリューション用 NuGet パッケージの管理]** をクリックします。

    [NuGet パッケージの管理] ウィンドウが表示されます。

3. `WindowsAzure.Service Bus` を検索し、**[インストール]** をクリックし、使用条件に同意します。

    これをダウンロード、インストール、およびへの参照を追加、 [Azure Service Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus), 、すべての依存関係とします。

4. **Program.cs** ファイルの先頭に次の `using` ステートメントを追加します。

        using System.IO;
        using Microsoft.ServiceBus.Messaging;

5. 最後に、次の行を **Main** メソッドに追加します。その場合、接続文字列を **d2ctutorial** という名前のキューの **Listen** アクセス許可に置き換えます。

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






[about azure storage]: ../storage/storage-create-storage-account.md#create-a-storage-account 
[azure iot - service sdk nuget package]: https://www.nuget.org/packages/Microsoft.Azure.Devices/ 
[get started with event hubs]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md 
[iot hub developer guide - identity registry]: iot-hub-devguide.md#identityregistry 
[azure storage scalability guidelines]: ../storage/storage-scalability-targets.md 
[azure block blobs]: https://msdn.microsoft.com/library/azure/ee691964.aspx 
[service bus documentation]: ../service-bus/service-bus-dotnet-how-to-use-queues.md 
[event hubs overview]: ../event-hubs/event-hubs-overview.md 
[scaled out event processing]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Event-Hub-45f43fc3 
[eventprocessorhost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx 
[event hubs programming guide]: ../event-hubs/event-hubs-programming-guide.md 
[azure preview portal]: https://portal.azure.com/ 
[transient fault handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx 
[azure portal]: https://manage.windowsazure.com/ 
[service bus queue]: ../service-bus/service-bus-dotnet-how-to-use-queues.md 
[build multi-tier applications with service bus]: ../service-bus/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md 
[10]: ./media/iot-hub-process-d2c-cloud-csharp/create-identity-csharp1.png 
[12]: ./media/iot-hub-getstarted-cloud-csharp/create-identity-csharp3.png 
[20]: ./media/iot-hub-getstarted-cloud-csharp/create-storage1.png 
[21]: ./media/iot-hub-getstarted-cloud-csharp/create-storage2.png 
[22]: ./media/iot-hub-getstarted-cloud-csharp/create-storage3.png 
[30]: ./media/iot-hub-process-d2c-cloud-csharp/createqueue2.png 
[31]: ./media/iot-hub-process-d2c-cloud-csharp/createqueue3.png 
[32]: ./media/iot-hub-process-d2c-cloud-csharp/createqueue4.png 

