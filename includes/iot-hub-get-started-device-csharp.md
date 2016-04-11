## シミュレーション対象デバイス アプリの作成

このセクションでは、デバイスからクラウドへのメッセージを IoT ハブに送信するデバイスをシミュレートする Windows コンソール アプリを作成します。

1. Visual Studio で新しい Visual c# Windows クラシック デスクトップ プロジェクトを使用して現在のソリューションを追加、 **コンソール アプリケーション** プロジェクト テンプレートです。  プロジェクトに名前を **SimulatedDevice**します。

    ![][30]

2. ソリューション エクスプ ローラーで右クリックし、 **SimulatedDevice** [プロジェクト] をクリックして **NuGet パッケージの管理**します。

3.  **NuGet パッケージ マネージャー** ウィンドウで、ことを確認、 **プレリリースを含める** オプションがオンになっています。 検索 **Microsoft Azure のデバイス クライアントの**, 、] をクリックして **インストール**, 、し、使用条件に同意します。

    これをダウンロード、インストール、およびへの参照を追加、 [Azure IoT のデバイスの SDK の NuGet パッケージ][lnk-device-nuget]します。

4. 次の追加 `using` の上部にあるステートメント、 **Program.cs** ファイル。

        using Microsoft.Azure.Devices.Client;
        using Newtonsoft.Json;
        using System.Threading;

5. 次のフィールドを追加、 **プログラム** で取得した IoT hub のホスト名を持つプレース ホルダーの値を代入するクラス、 *IoT hub を作成する* セクションと、デバイスのキーで取得、 *デバイス id を作成* セクション。

        static DeviceClient deviceClient;
        static string iotHubUri = "{iot hub hostname}";
        static string deviceKey = "{device key}";

6. 次のメソッドを追加、 **プログラム** クラス。

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

7. 次の行を最後に、追加、 **Main** メソッド。

        Console.WriteLine("Simulated device\n");
        deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey("myFirstDevice", deviceKey));

        SendDeviceToCloudMessagesAsync();
        Console.ReadLine();

  既定では、 **作成** メソッドを作成、 **DeviceClient** IoT Hub をやり取りするために、AMQP プロトコルを使用します。 HTTPS プロトコルを使用するには、上書きを使用して、 **作成** メソッド プロトコルを指定することができます。 HTTPS プロトコルを使用して選択したかどうかも追加するあります、 **Microsoft.AspNet.WebApi.Client** NuGet パッケージを含めるプロジェクトを **System.Net.Http.Formatting** 名前空間。


> [AZURE.NOTE] わかりやすく、このチュートリアルは、再試行ポリシーを実装していません。 実稼働コードでは、MSDN の記事に示されている (など、指数関数的バックオフによる)、再試行ポリシーを実装する必要があります [一時的な障害処理][lnk-transient-faults]します。

<!-- Links -->

[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-transient-faults]: https://msdn.microsoft.com/en-us/library/hh680901(v=pandp.50).aspx

<!-- Images -->
[30]: ./media/iot-hub-getstarted-device-csharp/create-identity-csharp1.png

