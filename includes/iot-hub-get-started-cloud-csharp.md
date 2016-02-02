## デバイス ID の作成

このセクションでは、IoT ハブの ID レジストリに新しいデバイス ID を作成する Windows コンソール アプリケーションを作成します。 IoT Hub に接続するデバイスは、あらかじめデバイス ID レジストリに登録されている必要があります。 参照してください、 **デバイス Identity レジストリ** のセクションで、 [IoT Hub 開発者ガイド ][lnk-devguide-identity] の詳細。 このコンソール アプリケーションを実行すると、デバイスからクラウドへのメッセージを IoT Hub に送信する際にそのデバイスを識別する一意の ID とキーが生成されます。

1. Visual Studio で新しい Visual c# Windows クラシック デスクトップ プロジェクトを使用して現在のソリューションを追加、 **コンソール アプリケーション** プロジェクト テンプレートです。 プロジェクトに **CreateDeviceIdentity** という名前を付けます。

    ![][10]

2. ソリューション エクスプローラーで **CreateDeviceIdentity** プロジェクトを右クリックし、**[NuGet パッケージの管理]** をクリックします。

3. **[NuGet パッケージ マネージャー]** ウィンドウで **[プレリリースを含める]** オプションがオンになっていることを確認します。 そのうえで **Microsoft Azure Devices** を検索し、**[インストール]** をクリックして、使用条件に同意します。

    ![][11]

4. これをダウンロード、インストール、およびへの参照を追加、 [Microsoft Azure デバイス SDK ][lnk-nuget-device-sdk] NuGet パッケージ。

4. 次の追加 `を使用して` ステートメントの先頭に、 **Program.cs** ファイル。

        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;

5. **Program** クラスに次のフィールドを追加し、プレースホルダーの値を、前のセクションで作成した IoT Hub の接続文字列に置き換えます。

        static RegistryManager registryManager;
        static string connectionString = "{iothub connection string}";

6. **Program** クラスに次のメソッドを追加します。

        private async static Task AddDeviceAsync()
        {
            string deviceId = "myFirstDevice";
            Device device;
            try
            {
                device = await registryManager.AddDeviceAsync(new Device(deviceId));
            }
            catch (DeviceAlreadyExistsException)
            {
                device = await registryManager.GetDeviceAsync(deviceId);
            }
            Console.WriteLine("Generated device key: {0}", device.Authentication.SymmetricKey.PrimaryKey);
        }

    このメソッドは、**myFirstDevice** という ID で新しいデバイス ID を作成します (そのデバイス ID が既にレジストリに存在する場合は、単にその既存のデバイス情報を取得します)。 続けてその ID のプライマリ キーが表示されます。 シミュレーション対象デバイスでこのキーを使用して IoT Hub に接続することになります。

7. 最後に、**Main** メソッドに次の行を追加します。

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();

8. このアプリケーションを実行し、デバイスのキーを書き留めます。

    ![][12]

> [AZURE.NOTE] IoT Hub の ID レジストリには、ハブに対するアクセスの安全性を確保するためのデバイス ID だけが格納されます。 セキュリティ資格情報として使用するキーとデバイス ID、そして個々のデバイスについてアクセスを無効にすることのできる有効/無効フラグが格納されます。 その他デバイス固有のメタデータをアプリケーションで保存する必要がある場合は、アプリケーション固有のストアを使用する必要があります。 参照してください [IoT Hub 開発者ガイド ][lnk-devguide-identity] の詳細。

## デバイスからクラウドへのメッセージの受信

このセクションでは、IoT Hub からのデバイスからクラウドへのメッセージを読み込む Windows コンソール アプリケーションを作成します。 IoT hub を公開、 [Event Hubs ][lnk-event-hubs-overview]-デバイスからクラウドへのメッセージを読み取ることができるように互換性のあるエンドポイント。 わかりやすくするために、このチュートリアルで作成するリーダーは基本的なものであり、高スループットのデプロイには適していません。  [[Lnk processd2c チュートリアル] デバイスからクラウドへのメッセージを処理][lnk-processd2c-tutorial] 規模でのデバイスからクラウドへのメッセージを処理する方法を説明し、 [Event Hubs ][lnk-eventhubs-tutorial] チュートリアルについて説明しますさらに情報メッセージを処理する Event Hub からです。

1. Visual Studio で新しい Visual c# Windows クラシック デスクトップ プロジェクトを使用して現在のソリューションを追加、 **コンソール アプリケーション** プロジェクト テンプレートです。 プロジェクトに **ReadDeviceToCloudMessages** という名前を付けます。

    ![][10]

2. ソリューション エクスプローラーで、**[ReadDeviceToCloudMessages]** プロジェクトを右クリックし、**[NuGet パッケージの管理]** をクリックします。

3. **[NuGet パッケージ マネージャー]** ウィンドウで **[プレリリースを含める]** オプションがオンになっていることを確認します。 **WindowsAzure.ServiceBus** を検索し、**[インストール]** をクリックし、使用条件に同意します。

    これをダウンロード、インストール、およびへの参照を追加、 [Azure Service Bus ][lnk-servicebus-nuget], 、すべての依存関係とします。

4. **Program.cs** ファイルの先頭に次の `using` ステートメントを追加します。

        using Microsoft.ServiceBus.Messaging;

5. **Program** クラスに次のフィールドを追加し、プレースホルダーの値を、「*IoT Hub の作成*」セクションで作成した IoT Hub の接続文字列に置き換えます。

        static string connectionString = "{iothub connection string}";
        static string iotHubD2cEndpoint = "messages/events";
        static EventHubClient eventHubClient;

6. **Program** クラスに次のメソッドを追加します。

     private async static Task ReceiveMessagesFromDeviceAsync(string partition)
     {
         var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.Now);
         while (true)
         {
             EventData eventData = await eventHubReceiver.ReceiveAsync();
             if (eventData == null) continue;
    
             string data = Encoding.UTF8.GetString(eventData.GetBytes());
             Console.WriteLine(string.Format("Message received. Partition: {0} Data: '{1}'", partition, data));
         }
     }

 このメソッドは、**EventHubReceiver** インスタンスを使用して、IoT Hub のすべてのデバイスからクラウドへの受信パーティションからメッセージを受け取ります。 渡す方法に注意してください、 `DateTime.Now` パラメーターを作成するとき、 **EventHubReceiver** オブジェクトの開始後に送信されたメッセージのみ受信するようにします。

7. 最後に、**Main** メソッドに次の行を追加します。

     Console.WriteLine("Receive messages\n");
     eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, iotHubD2cEndpoint);
    
     var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;
    
     foreach (string partition in d2cPartitions)
     {
        ReceiveMessagesFromDeviceAsync(partition);
     }  
     Console.ReadLine();







[lnk-eventhubs-tutorial]: event-hubs-csharp-ephcs-getstarted.md 
[lnk-devguide-identity]: iot-hub-devguide.md#identityregistry 
[lnk-servicebus-nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus 
[lnk-event-hubs-overview]: event-hubs-overview.md 
[lnk-nuget-device-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/ 
[lnk-processd2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md 
[10]: ./media/iot-hub-getstarted-cloud-csharp/create-identity-csharp1.png 
[11]: ./media/iot-hub-getstarted-cloud-csharp/create-identity-csharp2.png 
[12]: ./media/iot-hub-getstarted-cloud-csharp/create-identity-csharp3.png 

