## シミュレーション対象デバイス アプリの作成

このセクションでは、デバイスからクラウドへのメッセージを IoT ハブに送信するデバイスをシミュレートする Windows コンソール アプリを作成します。

1. Visual Studio で新しい Visual c# Windows クラシック デスクトップ プロジェクトを使用して現在のソリューションを追加、 **コンソール アプリケーション** プロジェクト テンプレートです。 プロジェクトに **SimulatedDevice** という名前を付けます。

    ![][30]

2. ソリューション エクスプローラーで **SimulatedDevice** プロジェクトを右クリックし、**[NuGet パッケージの管理]** をクリックします。

3. **[NuGet パッケージ マネージャー]** ウィンドウで **[プレリリースを含める]** オプションがオンになっていることを確認します。 **Microsoft Azure Devices Client** を検索し、**[インストール]** をクリックして、使用条件に同意します。

    これをダウンロード、インストール、およびへの参照を追加、 [Azure IoT のデバイスの SDK の NuGet パッケージ ][lnk-device-nuget]します。

4. **Program.cs** ファイルの先頭に次の `using` ステートメントを追加します。

        using Microsoft.Azure.Devices.Client;
        using Newtonsoft.Json;
        using System.Threading;

5. **Program** クラスに次のフィールドを追加し、プレースホルダーの値を、「*IoT Hub の作成*」セクションで取得した IoT Hub のホスト名と「*デバイス ID の作成*」セクションで取得したデバイス キーにそれぞれ置き換えます。

        static DeviceClient deviceClient;
        static string iotHubUri = "{iot hub hostname}";
        static string deviceKey = "{device key}";

6. **Program** クラスに次のメソッドを追加します。

     private static async void SendDeviceToCloudMessagesAsync()
     {
         double avgWindSpeed = 10; // m/s
         Random rand = new Random();
    
         while (true)
         {
             double currentWindSpeed = avgWindSpeed + rand.NextDouble() * 4 - 2;
    
             var telemetryDataPoint = new
             {
                 deviceId = "myFirstDevice",
                 windSpeed = currentWindSpeed
             };
             var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
             var message = new Message(Encoding.ASCII.GetBytes(messageString));
    
             await deviceClient.SendEventAsync(message);
             Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, messageString);
    
             Thread.Sleep(1000);
         }
     }

 このメソッドは、デバイスからクラウドへの新しいメッセージを 1 秒おきに送信します。 このメッセージには、JSON 形式でシリアル化されたオブジェクトが、デバイス ID、ランダムに生成された番号と共に含まれ、これによって風速センサーがシミュレートされます。

7. 最後に、**Main** メソッドに次の行を追加します。

     Console.WriteLine("Simulated device\n");
     deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey("myFirstDevice", deviceKey));
    
     SendDeviceToCloudMessagesAsync();
     Console.ReadLine();

  既定では、**Create** メソッドは、IoT Hub と通信するために AMQP プロトコルを使用する **DeviceClient** を作成します。 HTTPS プロトコルを使用するには、プロトコルを引数として受け取る、**Create** メソッドのオーバーライドを使用します。 HTTPS プロトコルを使用するように選択する場合は、**Microsoft.AspNet.WebApi.Client** NuGet パッケージをプロジェクトに追加して、**System.Net.Http.Formatting** 名前空間を含める必要もあります。


> [AZURE.NOTE] わかりやすくするために、このチュートリアルでは再試行ポリシーは実装しません。 実稼働コードでは、MSDN の記事に示されている (など、指数関数的バックオフによる)、再試行ポリシーを実装する必要があります [一時的な障害処理 ][lnk-transient-faults]します。





[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/ 
[lnk-transient-faults]: https://msdn.microsoft.com/en-us/library/hh680901(v=pandp.50).aspx 
[30]: ./media/iot-hub-getstarted-device-csharp/create-identity-csharp1.png 

