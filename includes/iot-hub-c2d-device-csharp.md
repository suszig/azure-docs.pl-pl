## シミュレートされたデバイスでメッセージを受信

このセクションでは、「IoT Hub の概要」で作成したシミュレーション済みデバイス アプリケーションを変更して、クラウドからデバイスへのメッセージを IoT Hub から受信します。

1. Visual Studio の **SimulatedDevice** プロジェクトで、次のメソッドを **Program** クラスに追加します。

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

 `ReceiveAsync` メソッドは、デバイスで受信した時に、受信したメッセージを非同期的に戻ります。 指定可能なタイムアウト期間が経過したら、*null* が返されます (ここでは、既定の 1 分が使用されます)。 これが生じると、新しいメッセージを待機し続けるためのコードが必要になります。 これは、理由、 `場合 (receivedMessage null = =) 続行` 行です。

 呼び出し `CompleteAsync()` 、メッセージが正常に処理されたことと、それを安全に削除デバイス キューからは、IoT Hub を通知します。 デバイスのアプリケーションがメッセージの処理を完了できなくなる何らかの事態が生じると、IoT Hub はそれをもう一度送信します。デバイス アプリケーション内のメッセージ処理ロジックが*べき等*であることが重要です。つまり、複数回受信しても、同じメッセージであれば同じ結果が生成されます。 アプリケーションが一時的にできる `破棄` これには、後で使用します。 キューにメッセージを保持する IoT hub を含め、メッセージまたは `拒否` 、メッセージはキューからメッセージを完全に削除されます。 参照してください [IoT Hub 開発者ガイド ][iot hub developer guide - c2d] クラウドとデバイス間のメッセージのライフ サイクルの詳細についてです。

> [AZURE.NOTE] AMQP ではなく、トランスポートとして HTTP/1 を使用する場合、 `ReceiveAsync` はすぐに復帰します。 HTTP/1 を使用するクラウドからデバイスへのメッセージに関してサポートされているパターンは、メッセージを低頻度でチェックする (25 分ごとなど) デバイスに断続的に接続されます。 HTTP/1 受信の発行が多くなれば、IoT Hub で要求がスロットルされます。 参照してください [IoT Hub 開発者ガイド ][iot hub developer guide - c2d] AMQP と HTTP/1 のサポート、および調整の IoT Hub の違いの詳細についてです。

2. 次のメソッドを追加、 **Main** メソッドの直前、 `Console.ReadLine()` 行。

        ReceiveC2dAsync();


> [AZURE.NOTE] わかりやすくするために、このチュートリアルでは再試行ポリシーは実装しません。 実稼働のコードでは、MSDN 記事 (一時的な障害処理) で推奨されているように、再試行ポリシー (指数関数的バックオフなど) を実装することをお勧めします。







[iot hub developer guide - c2d]: iot-hub-devguide.md#c2d 

