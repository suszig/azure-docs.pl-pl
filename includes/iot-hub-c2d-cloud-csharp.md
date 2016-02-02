## クラウドからデバイスへのメッセージをアプリケーションのバックエンドから送信する

このセクションでは、デバイスからクラウドへのメッセージを、シミュレートする デバイス アプリに送信する Windows コンソール アプリを作成します。

1. 現在の Visual Studio ソリューションで作成、新しい Visual c# のデスクトップ アプリ プロジェクトを使用して、 **コンソール アプリケーション** プロジェクト テンプレートです。 プロジェクトに **SendCloudToDevice** という名前を付けます。

    ![][20]

2. ソリューション エクスプローラーでソリューションを右クリックし、**[ソリューション用 NuGet パッケージの管理]** をクリックします。

    [NuGet パッケージの管理] ウィンドウが表示されます。

3. 検索 `Microsoft Azure のデバイス`, 、] をクリックして **インストール**, 、使用条件に同意します。

    これによってダウンロード、インストールすると、され [Azure IoT - サービス SDK の NuGet パッケージ] への参照を追加します。

4. **Program.cs** ファイルの先頭に次の `using` ステートメントを追加します。

        using Microsoft.Azure.Devices;

5. 次のフィールドを追加、 **プログラム** から IoT hub の接続文字列でプレース ホルダーの値に置き換えるクラス [を使ってみる IoT Hub]。

        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";

6. **Program** クラスに次のメソッドを追加します。

        private async static Task SendCloudToDeviceMessageAsync()
        {
            var commandMessage = new Message(Encoding.ASCII.GetBytes("Cloud to device message."));
            await serviceClient.SendAsync("myFirstDevice", commandMessage);
        }

    このメソッドは、id を持つデバイスをクラウドとデバイスの新しいメッセージを送信 `myFirstDevice`します。 このパラメーターは、適宜変更で使用されてから変更した場合に [を使ってみる IoT Hub] です。

7. 最後に、**Main** メソッドに次の行を追加します。

     Console.WriteLine("Send Cloud-to-Device message\n");
     serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
    
     Console.WriteLine("Press any key to send a C2D message.");
     Console.ReadLine();
     SendCloudToDeviceMessageAsync().Wait();
     Console.ReadLine();

8. Visual Studio 内でソリューションを右クリックし、**[スタートアップ プロジェクトの設定...]** を選択します。 **[マルチ スタートアップ プロジェクト]** を選択してから、**ProcessDeviceToCloudMessages**、**SimulatedDevice**、および **SendCloudToDevice** の **[開始]** アクションを選択します。

9.  **F5** キーを押すと、3 つすべてのアプリケーションが開始されていることを確認できます。 **[SendCloudToDevice]** ウィンドウを選択し、**Enter** キーを押します。シミュレートするアプリケーションでメッセージが受信されていることがわかります。

    ![][21]

## 配信フィードバックの受信

クラウドからデバイスへの各メッセージに対して IoT Hub からの配信 (または有効期限) 確認を要求できます。 これにより、クラウド バックエンドで再試行または補正ロジックを簡単に通知できるようになります。 参照してください、 [IoT Hub 開発者ガイド ][iot hub developer guide - c2d] クラウドとデバイスからのフィードバックの詳細についてです。

このセクションでは、**SendCloudToDevice** アプリを、フィードバックを要求し、IoT Hub からそれらを受信するように変更します。

1. Visual Studio の **SendCloudToDevice** プロジェクトで、次のメソッドを **Program** クラスに追加します。

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

3. クラウドからデバイスへのメッセージの配信のためのフィードバックを要求するには、**SendCloudToDeviceMessageAsync** メソッドにプロパティを指定する必要があります。 直後に、次の行を追加、 `var commandMessage = 新しい Message(...);` 行。

        commandMessage.Ack = DeliveryAcknowledgement.Full;

4.  **F5** キーを押してアプリを実行すると、3 つすべてのアプリケーションが開始されていることを確認できます。 **[SendCloudToDevice]** ウィンドウを選択して、**Enter** キーを押します。シミュレーション対象アプリによってメッセージが送信され、**SendCloudToDevice** アプリによりフィードバック メッセージが受信されたことがわかります。

    ![][22]

> [AZURE.NOTE] わかりやすくするために、このチュートリアルでは再試行ポリシーは実装しません。 実稼働のコードでは、MSDN 記事 (一時的な障害処理) で推奨されているように、再試行ポリシー (指数関数的バックオフなど) を実装することをお勧めします。





[iot hub developer guide - c2d]: iot-hub-devguide.md#c2d 
[azure iot - service sdk nuget package]: https://www.nuget.org/packages/Microsoft.Azure.Devices/ 
[transient fault handling]: https://msdn.microsoft.com/en-us/library/hh680901(v=pandp.50).aspx 
[get started with iot hub]: iot-hub-csharp-csharp-getstarted.md 
[20]: ./media/iot-hub-c2d-cloud-csharp/create-identity-csharp1.png 
[21]: ./media/iot-hub-c2d-cloud-csharp/sendc2d1.png 
[22]: ./media/iot-hub-c2d-cloud-csharp/sendc2d2.png 

