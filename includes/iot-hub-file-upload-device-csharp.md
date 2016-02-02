## シミュレーションされたデバイスからのファイルのアップロード

このセクションでは、「IoT Hub を使用したクラウドからデバイスへのメッセージの送信」で作成したシミュレーション済みデバイス アプリケーションを変更して、クラウドからデバイスへのメッセージを IoT Hub から受信します。

1. Visual Studio で **SimulatedDevice** プロジェクトを右クリックし、**[NuGet パッケージの管理...]** をクリックします。

    [NuGet パッケージの管理] ウィンドウが表示されます。

2. 検索 `WindowsAzure.Storage`, 、] をクリックして **インストール**, 、使用条件に同意します。

    これをダウンロード、インストール、およびへの参照を追加、 [Microsoft Azure Storage SDK](https://www.nuget.org/packages/WindowsAzure.Storage/)します。

3. **Program.cs** ファイルの先頭に、次のステートメントを追加します。

        using System.IO;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;

4. **Program** クラスで、**ReceiveC2dAsync** メソッドを次の方法で変更します。

     private static async void ReceiveC2dAsync()
     {
         Console.WriteLine("\nReceiving cloud to device messages from service");
         while (true)
         {
             Message receivedMessage = await deviceClient.ReceiveAsync();
             if (receivedMessage == null) continue;
    
             if (receivedMessage.Properties.ContainsKey("command") && receivedMessage.Properties["command"] == "FileUpload")
             {
                 UploadFileToBlobAsync(receivedMessage);
                 continue;
             }
    
             Console.ForegroundColor = ConsoleColor.Yellow;
             Console.WriteLine("Received message: {0}", Encoding.ASCII.GetString(receivedMessage.GetBytes()));
             Console.ResetColor();
         }
     }

 これにより、 **ReceiveC2dAsync** を持つメッセージを区別するため、 `コマンド` プロパティに設定 `FileUpload`, 、によって処理される、 **UploadFileToBlobAsync** メソッドです。

 以下のメソッドを追加して、ファイル アップロード コマンドを処理します。

     private static async Task UploadFileToBlobAsync(Message fileUploadCommand)
     {
         var fileUri = fileUploadCommand.Properties["fileUri"];
         var blob = new CloudBlockBlob(new Uri(fileUri));
    
         byte[] data = new byte[10 * 1024 * 1024];
         Random rng = new Random();
         rng.NextBytes(data);
    
         MemoryStream msWrite = new MemoryStream(data);
         msWrite.Position = 0;
         using (msWrite)
         {
             await blob.UploadFromStreamAsync(msWrite);
         }
         Console.ForegroundColor = ConsoleColor.Yellow;
         Console.WriteLine("Uploaded file to: {0}", fileUri);
         Console.ResetColor();
    
         await deviceClient.CompleteAsync(fileUploadCommand);
     }

 このメソッドでは、Azure Storage SDK を使用してランダムに生成された 10Mb の BLOB が、指定された URI にアップロードされます。 参照してください [Azure ストレージ - blob を使用する方法] の blob をアップロードする方法の詳細。

> [AZURE.NOTE] シミュレーション済みデバイスのこの実装によって、BLOB がアップロードされた後にのみ、クラウドからデバイスへのメッセージが処理されるという方法に留意してください。 この方法では、配信の受信確認を処理するためにアップロードされたファイルの可用性を表すために、バック エンドにアップロードされたファイルの処理が簡略化します。 説明するよう、 [IoT Hub 開発者ガイド ][iot hub developer guide - c2d], 、ただし、前に完了していないメッセージ、 *表示タイムアウト* (通常は 1 分間) にデバイスのキューに戻されると、 **ReceiveAsync()** メソッドが再度表示されます。 ファイル アップロードに時間がかかるシナリオの場合、シミュレーション済みデバイスで現在のアップロード ジョブの永続的ストアを保持するほうが良い場合があります。 これにより、シミュレートされたデバイスをファイルのアップロードが完了する前に、クラウドとデバイス間のメッセージを完了し、完了のバックエンドを通知するデバイスからクラウドへのメッセージを送信できます。












[iot hub developer guide - c2d]: iot-hub-devguide.md#c2d 
[azure storage - how to use blobs]: https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-blobs/#upload-a-blob-into-a-container 

