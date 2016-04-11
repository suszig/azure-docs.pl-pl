## クラウドからデバイスへのメッセージをアプリケーションのバックエンドから送信する

このセクションでは、デバイスからクラウドへのメッセージを、シミュレートする デバイス アプリに送信する Windows コンソール アプリを作成します。

1. 現在の Visual Studio ソリューションで作成、新しい Visual c# のデスクトップ アプリ プロジェクトを使用して、 **コンソール アプリケーション** プロジェクト テンプレートです。 プロジェクトに名前を **SendCloudToDevice**します。

    ![][20]

2. ソリューション エクスプ ローラーでソリューションを右クリックし、をクリックし、 **... ソリューションの NuGet パッケージの管理**します。 

    [NuGet パッケージの管理] ウィンドウが表示されます。

3. 検索 `Microsoft Azure Devices`, 、] をクリックして **インストール**, 、使用条件に同意します。 

    これをダウンロード、インストール、およびへの参照を追加、 [Azure IoT - Service SDK NuGet package]します。

4. 次の追加 `using` の上部にあるステートメント、 **Program.cs** ファイル。

        using Microsoft.Azure.Devices;

5. 次のフィールドを追加、 **プログラム** から IoT hub の接続文字列でプレース ホルダーの値に置き換えるクラス [Get started with IoT Hub]:

        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";

6. 次のメソッドを追加、 **プログラム** クラス。

        private async static Task SendCloudToDeviceMessageAsync()
        {
            var commandMessage = new Message(Encoding.ASCII.GetBytes("Cloud to device message."));
            await serviceClient.SendAsync("myFirstDevice", commandMessage);
        }

    このメソッドは、新しいクラウドからデバイスへのメッセージを ID `myFirstDevice` を持つデバイスに送信します。 このパラメーターは、適宜変更で使用されてから変更した場合に備えて [Get started with IoT Hub]します。

7. 次の行を最後に、追加、 **Main** メソッド。

        Console.WriteLine("Send Cloud-to-Device message\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);

        Console.WriteLine("Press any key to send a C2D message.");
        Console.ReadLine();
        SendCloudToDeviceMessageAsync().Wait();
        Console.ReadLine();

8. Visual Studio でソリューションを右クリックし、選択 **設定のスタートアップ プロジェクト]**します。 選択 **マルチ スタートアップ プロジェクト**, 選択してから、 **開始** のアクション **ProcessDeviceToCloudMessages**, 、**SimulatedDevice**, 、および **SendCloudToDevice**します。

9.  キーを押して **f5 キーを押して**, と開始すべての 3 つのアプリケーションを表示する必要があります。 選択、 **SendCloudToDevice** windows キーを押します **Enter**: シミュレーションのアプリケーションによって受信されるメッセージが表示されます。

    ![][21]

## 配信フィードバックの受信
クラウドからデバイスへの各メッセージに対して IoT Hub からの配信 (または有効期限) 確認を要求できます。 これにより、クラウド バックエンドで再試行または補正ロジックを簡単に通知できるようになります。 参照してください、 [IoT Hub 開発者ガイド][IoT Hub Developer Guide - C2D] クラウドとデバイスからのフィードバックの詳細についてです。

このセクションでは、変更、 **SendCloudToDevice** をアプリケーションにフィードバックの要求および IoT Hub から受信します。

1. Visual Studio での **SendCloudToDevice** プロジェクトで、次のメソッドを追加、 **プログラム** クラスです。
   
        private async static void ReceiveFeedbackAsync()
        {
            var feedbackReceiver = serviceClient.GetFeedbackReceiver();

            Console.WriteLine("\nReceiving c2d feedback from service");
            while (true)
            {
                var feedbackBatch = await feedbackReceiver.ReceiveAsync();
                if (feedbackBatch == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received feedback: {0}", string.Join(", ", feedbackBatch.Records.Select(f => f.StatusCode)));
                Console.ResetColor();

                await feedbackReceiver.CompleteAsync(feedbackBatch);
            }
        }

    受信パターンは、クラウドからデバイスへのメッセージをデバイス アプリから受信するために使用するものと同じであることに注意してください。

2. 次のメソッドを追加、 **Main** メソッドの直後、 `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` 行。

        ReceiveFeedbackAsync();

3. 内のプロパティを指定する必要が、クラウドとデバイス間のメッセージの配信のためのフィードバックを要求するために、 **SendCloudToDeviceMessageAsync** メソッドです。 `var commandMessage = new Message(...);` 行の直後に次の行を追加します。

        commandMessage.Ack = DeliveryAcknowledgement.Full;

4.  キーを押して、アプリの実行 **f5 キーを押して**, と開始すべての 3 つのアプリケーションを表示する必要があります。 選択、 **SendCloudToDevice** windows キーを押します **Enter**: 受信されると、数秒後に、シミュレートされたアプリケーションによって、フィードバック メッセージを受信しているメッセージを表示する必要があります、 **SendCloudToDevice** アプリです。

    ![][22]

> [AZURE.NOTE] わかりやすくするために、このチュートリアルは、再試行ポリシーを実装していません。 実稼働コードでは reccommended、MSDN の記事に示されている (指数関数的バックオフ) などの再試行ポリシーを実装する [Transient Fault Handling]します。

<!-- Links -->

[IoT Hub Developer Guide - C2D]: iot-hub-devguide.md#c2d
[Azure IoT - Service SDK NuGet package]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[Transient Fault Handling]: https://msdn.microsoft.com/en-us/library/hh680901(v=pandp.50).aspx
[Get started with IoT Hub]: iot-hub-csharp-csharp-getstarted.md

<!-- Images -->
[20]: ./media/iot-hub-c2d-cloud-csharp/create-identity-csharp1.png
[21]: ./media/iot-hub-c2d-cloud-csharp/sendc2d1.png
[22]: ./media/iot-hub-c2d-cloud-csharp/sendc2d2.png







