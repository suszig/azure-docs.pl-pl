## シミュレーション済みデバイスでのメッセージの受信

このセクションでは、「IoT Hub の概要」で作成したシミュレーション済みデバイス アプリケーションを変更して、クラウドからデバイスへのメッセージを IoT Hub から受信します。

1. Visual Studio での **SimulatedDevice** プロジェクトで、次のメソッドを追加、 **プログラム** クラスです。

        private static async void ReceiveC2dAsync()
        {
            Console.WriteLine("\nReceiving cloud to device messages from service");
            while (true)
            {
                Message receivedMessage = await deviceClient.ReceiveAsync();
                if (receivedMessage == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received message: {0}", Encoding.ASCII.GetString(receivedMessage.GetBytes()));
                Console.ResetColor();

                await deviceClient.CompleteAsync(receivedMessage);
            }
        }

    `ReceiveAsync` メソッドは、受信したメッセージを、そのメッセージがデバイスによって受信されたとき非同期で返します。 返す *null* (この場合は、1 分の既定値を使用) を指定できるタイムアウト期間後。 これが生じると、新しいメッセージを待機し続けるためのコードが必要になります。 このため、`if (receivedMessage == null) continue` 行が必要になります。

    `CompleteAsync()` への呼び出しにより、メッセージが正常に処理され、デバイスのキューから安全に削除できることが IoT Hub に伝えられます。 場合出来事が発生する、デバイスのアプリケーションがメッセージの処理を完了するを防ぐ、IoT Hub は配信がもう一度デバイス アプリのメッセージ処理ロジックがすることが重要では、 *アイデムポ テントな*, 、つまり同じ結果をもたらす何度も同じメッセージを受信する必要があります。 アプリケーションはメッセージに対して一時的な `Abandon` 処理を行うこともできます。この場合、IoT Hub は将来の使用に備えてメッセージをキュー内に保持します。または、メッセージに対して `Reject` 処理を行うこともできます。この場合、メッセージはキューから永久に削除されます。 参照してください [IoT Hub 開発者ガイド][IoT Hub Developer Guide - C2D] クラウドとデバイス間のメッセージのライフ サイクルの詳細についてです。

> [AZURE.NOTE] AMQP ではなく、トランスポートとして HTTP/1 を使用する場合、 `ReceiveAsync` はすぐに復帰します。 HTTP/1 を使用するクラウドからデバイスへのメッセージに関してサポートされているパターンは、メッセージを低頻度でチェックする (25 分ごとなど) デバイスに断続的に接続されます。 HTTP/1 受信の発行が多くなれば、IoT Hub で要求がスロットルされます。 参照してください [IoT Hub 開発者ガイド][IoT Hub Developer Guide - C2D] AMQP と HTTP/1 のサポート、および調整の IoT Hub の違いの詳細についてです。

2. 次のメソッドを追加、 **Main** メソッドの直前、 `Console.ReadLine()` 行。

        ReceiveC2dAsync();

> [AZURE.NOTE] わかりやすくするために、このチュートリアルは、再試行ポリシーを実装していません。 実稼働のコードでは、MSDN 記事 (一時的な障害処理) で推奨されているように、再試行ポリシー (指数関数的バックオフなど) を実装することをお勧めします。

<!-- Links -->
[IoT Hub Developer Guide - C2D]: iot-hub-devguide.md#c2d

<!-- Images -->


