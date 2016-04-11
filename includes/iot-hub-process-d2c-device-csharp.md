## シミュレーション済みデバイスからの対話型メッセージの送信

このセクションでは、デバイスからクラウドへの対話式メッセージが IoT Hub に送信されるように、シミュレーション済みデバイス アプリケーションを変更します。

1. Visual Studio での **SimulatedDevice** プロジェクトで、次のメソッドを追加、 **プログラム** クラスです。
   
        private static async void SendDeviceToCloudInteractiveMessagesAsync()
        {
            while (true)
            {
                var interactiveMessageString = "Alert message!";
                var interactiveMessage = new Message(Encoding.ASCII.GetBytes(interactiveMessageString));
                interactiveMessage.Properties["messageType"] = "interactive";
                interactiveMessage.MessageId = Guid.NewGuid().ToString();

                await deviceClient.SendEventAsync(interactiveMessage);
                Console.WriteLine("{0} > Sending interactive message: {1}", DateTime.Now, interactiveMessageString);

                Thread.Sleep(10000);
            }
        }

    このメソッドはよく似て、 `SendDeviceToCloudMessagesAsync()` メソッドで作成された、 [Get started with IoT Hub], 、されていることが唯一の違い、 `MessageId` システム プロパティと、ユーザーのプロパティと呼ばれる `messageType` が設定されました。
    `MessageId` プロパティはグローバルに一意な ID (guid) に設定されており、これは受信するメッセージの重複除去に使用されます。 `messageType` プロパティは、データ ポイント メッセージから対話式メッセージを識別するために使用されます。 この情報が渡されるメッセージ プロパティでは、代わりに、メッセージの本文、バック エンドのイベント プロセッサが、全メッセージのルーティングを行うためだけを逆シリアル化する必要があるないようにします。

> [AZURE.NOTE] 重要ですが、 `MessageId`, 、対話型のメッセージの重複を除去、断続的なネットワーク通信 (またはその他の障害) を受けたり、デバイスから同じメッセージの複数の retrasmissions に応じて、デバイス内に作成するために使用します。 グリッドとは対照的に、セマンティック メッセージ ID (関連するメッセージ データ フィールドのハッシュ) を使用することもできます。

2. 次のメソッドを追加、 **Main** メソッドの直前、 `Console.ReadLine()` 行。

        SendDeviceToCloudInteractiveMessagesAsync();

> [AZURE.NOTE] わかりやすくするために、このチュートリアルは、再試行ポリシーを実装していません。 実稼働のコードでは、MSDN 記事 (一時的な障害処理) で推奨されているように、再試行ポリシー (指数関数的バックオフなど) を実装することをお勧めします。

<!-- Links -->
[Get started with IoT Hub]: iot-hub-csharp-csharp-getstarted.md
[IoT Hub Developer Guide - C2D]: iot-hub-devguide.md#c2d

<!-- Images -->
[10]: ./media/iot-hub-getstarted-cloud-csharp/create-identity-csharp1.png
[12]: ./media/iot-hub-getstarted-cloud-csharp/create-identity-csharp3.png

[20]: ./media/iot-hub-getstarted-cloud-csharp/create-storage1.png
[21]: ./media/iot-hub-getstarted-cloud-csharp/create-storage2.png
[22]: ./media/iot-hub-getstarted-cloud-csharp/create-storage3.png






