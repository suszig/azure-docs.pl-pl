## デバイス ID の作成

このセクションでは、IoT ハブの ID レジストリに新しいデバイス ID を作成する Windows コンソール アプリケーションを作成します。 IoT Hub に接続するデバイスは、あらかじめデバイス ID レジストリに登録されている必要があります。 参照してください、 **デバイス Identity レジストリ** のセクションで、 [IoT Hub 開発者ガイド][lnk-devguide-identity] の詳細。 このコンソール アプリケーションを実行すると、デバイスからクラウドへのメッセージを IoT Hub に送信する際にそのデバイスを識別する一意の ID とキーが生成されます。

1. Visual Studio で新しい Visual c# Windows クラシック デスクトップ プロジェクトを使用して現在のソリューションを追加、 **コンソール アプリケーション** プロジェクト テンプレートです。 プロジェクトに名前を **CreateDeviceIdentity**します。

    ![][10]

2. ソリューション エクスプ ローラーで右クリックし、 **CreateDeviceIdentity** [プロジェクト] をクリックして **NuGet パッケージの管理**します。

3.  **NuGet パッケージ マネージャー** ウィンドウで、ことを確認、 **プレリリースを含める** オプションがオンになっています。 検索し、 **Microsoft Azure デバイス**, 、] をクリックして **インストール**, 、使用条件に同意します。

    ![][11]

4. これをダウンロード、インストール、およびへの参照を追加、 [Microsoft Azure デバイス SDK][lnk-nuget-device-sdk] NuGet パッケージ。

4. 次の追加 `using` ステートメントの先頭に、 **Program.cs** ファイル。

        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;

5. 次のフィールドを追加、 **プログラム** クラスのプレース ホルダーの値を前のセクションで作成した IoT hub の接続文字列に置き換えます。

        static RegistryManager registryManager;
        static string connectionString = "{iothub connection string}";

6. 次のメソッドを追加、 **プログラム** クラス。

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

    このメソッドは、ID を持つ新しいデバイス id を作成 **myFirstDevice** (レジストリにそのデバイス ID が既に存在する場合、コードは単に取得、既存のデバイス情報)。 続けてその ID のプライマリ キーが表示されます。 シミュレーション対象デバイスでこのキーを使用して IoT Hub に接続することになります。

7. 次の行を最後に、追加、 **Main** メソッド。

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();

8. このアプリケーションを実行し、デバイスのキーを書き留めます。

    ![][12]

> [AZURE.NOTE] IoT Hub identity レジストリは、ハブに安全にアクセスを有効にするデバイスの id を格納するのみです。 セキュリティ資格情報として使用するキーとデバイス ID、そして個々のデバイスについてアクセスを無効にすることのできる有効/無効フラグが格納されます。 その他デバイス固有のメタデータをアプリケーションで保存する必要がある場合は、アプリケーション固有のストアを使用する必要があります。 参照してください [IoT Hub 開発者ガイド][lnk-devguide-identity] の詳細。

## デバイスからクラウドへのメッセージの受信

このセクションでは、IoT Hub からのデバイスからクラウドへのメッセージを読み込む Windows コンソール アプリケーションを作成します。 IoT hub を公開、 [Event Hubs][lnk-event-hubs-overview]-デバイスからクラウドへのメッセージを読み取ることができるように互換性のあるエンドポイント。 わかりやすくするために、このチュートリアルで作成するリーダーは基本的なものであり、高スループットのデプロイメントには適していません。  [デバイスからクラウドへのメッセージを処理][lnk-processd2c-tutorial] 規模でのデバイスからクラウドへのメッセージを処理する方法を説明し、 [Event Hubs の使用開始][lnk-eventhubs-tutorial] チュートリアルについて説明しますさらに情報メッセージを処理する Event Hub からです。

1. Visual Studio で新しい Visual c# Windows クラシック デスクトップ プロジェクトを使用して現在のソリューションを追加、 **コンソール アプリケーション** プロジェクト テンプレートです。 プロジェクトに名前を **ReadDeviceToCloudMessages**します。

    ![][10]

2. ソリューション エクスプ ローラーで右クリックし、 **ReadDeviceToCloudMessages** [プロジェクト] をクリックして **NuGet パッケージの管理**します。

3.  **NuGet パッケージ マネージャー** ウィンドウで、ことを確認、 **プレリリースを含める** オプションがオンになっています。 検索し、 **WindowsAzure.ServiceBus**, 、] をクリックして **インストール**, 、使用条件に同意します。

    これをダウンロード、インストール、およびへの参照を追加、 [Azure Service Bus][lnk-servicebus-nuget], 、すべての依存関係とします。

4. 次の追加 `using` の上部にあるステートメント、 **Program.cs** ファイル。

        using Microsoft.ServiceBus.Messaging;

5. 次のフィールドを追加、 **プログラム** クラスで作成した IoT hub の接続文字列プレース ホルダーの値を置き換えて、 *IoT hub を作成する* セクション。

        static string connectionString = "{iothub connection string}";
        static string iotHubD2cEndpoint = "messages/events";
        static EventHubClient eventHubClient;

6. 次のメソッドを追加、 **プログラム** クラス。

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

    このメソッドを使用して、 **EventHubReceiver** すべて、IoT hub デバイスとクラウドからメッセージを受信するインスタンスは、パーティションを受信します。 渡す方法に注意してください、 `DateTime.Now` パラメーターを作成するとき、 **EventHubReceiver** オブジェクトの開始後に送信されたメッセージのみ受信するようにします。

7. 次の行を最後に、追加、 **Main** メソッド。

        Console.WriteLine("Receive messages\n");
        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, iotHubD2cEndpoint);

        var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;

        foreach (string partition in d2cPartitions)
        {
           ReceiveMessagesFromDeviceAsync(partition);
        }  
        Console.ReadLine();


<!-- Links -->

[lnk-eventhubs-tutorial]: event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide.md#identityregistry
[lnk-servicebus-nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-event-hubs-overview]: event-hubs-overview.md

[lnk-nuget-device-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-processd2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

<!-- Images -->
[10]: ./media/iot-hub-getstarted-cloud-csharp/create-identity-csharp1.png
[11]: ./media/iot-hub-getstarted-cloud-csharp/create-identity-csharp2.png
[12]: ./media/iot-hub-getstarted-cloud-csharp/create-identity-csharp3.png

