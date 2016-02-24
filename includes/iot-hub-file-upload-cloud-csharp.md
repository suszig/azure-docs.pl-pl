## Azure ストレージ アカウントのプロビジョニング
シミュレーションされたデバイスが Azure Storage BLOB にファイルをアップロードするため、Azure ストレージ アカウントが必要です。 既存のものを使用したり、指示に従います [About Azure Storage] 新規に作成します。 ストレージ アカウント接続文字列はメモしておいてください。

## シミュレート済みデバイスへの Azure BLOB URI の送信

このセクションで変更してみましょう、 **SendCloudtoDevice** [IoT Hub でメッセージを送信クラウド デバイス] で、Azure を含むように作成するコンソール アプリケーションの共有アクセス署名を使用した blob の URI。 これにより、クラウド バックエンドは、クラウドからデバイスへのメッセージの受信者にのみ BLOB への書き込みアクセス権を付与できます。

1. Visual Studio で右クリックし、 **SendCloudtoDevice** [プロジェクト] をクリックして **NuGet パッケージの管理...**します。 

    [NuGet パッケージの管理] ウィンドウが表示されます。

2. 検索 `WindowsAzure.Storage`, 、] をクリックして **インストール**, 、使用条件に同意します。 

    これをダウンロード、インストール、およびへの参照を追加、 [Microsoft Azure Storage SDK](https://www.nuget.org/packages/WindowsAzure.Storage/)します。

3.  **Program.cs** ファイル、ファイルの先頭に次のステートメントを追加します。

        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;

4.  **プログラム** クラスに次のクラス フィールドは、ストレージ アカウントの接続文字列の置換を追加します。

        static string storageConnectionString = "{storage connection string}";

    次のメソッドを追加し (任意の blob コンテナー名を置き換えることができます、このチュートリアルでは **iothubfileuploadtutorial**)。
   
        private static async Task<string> GenerateBlobUriAsync()
        {
            var storageAccount = CloudStorageAccount.Parse(storageConnectionString);
            var blobClient = storageAccount.CreateCloudBlobClient();
            var blobContainer = blobClient.GetContainerReference("iothubfileuploadtutorial");
            await blobContainer.CreateIfNotExistsAsync();

            var blobName = String.Format("deviceUpload_{0}", Guid.NewGuid().ToString());
            CloudBlockBlob blob = blobContainer.GetBlockBlobReference(blobName);

            SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
            sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
            sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
            sasConstraints.Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write;
            string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

            return blob.Uri + sasBlobToken;
        }

    このメソッドは、新しい blob の参照を作成し、」の説明に従って、共有アクセス署名 URI を生成 [を作成し、Blob サービスによる SAS を使用して](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-2/)します。 上記のメソッドで生成される署名 URI の有効期限は 24 時間です。 ターゲット デバイスでファイルをアップロードするのにこれ以上の時間を要する場合は (接続頻度が低い場合や、容量の大きいファイルをアップロードするには接続の信頼性が低い場合など)、署名の有効期限の延長を検討してください。

5. 変更、 **SendCloudToDeviceMessageAsync** 次のようにします。

        private async static Task SendCloudToDeviceMessageAsync()
        {
            var commandMessage = new Message();
            commandMessage.Properties["command"] = "FileUpload";
            commandMessage.Properties["fileUri"] = await GenerateBlobUriAsync();
            commandMessage.Ack = DeliveryAcknowledgement.Full;

            await serviceClient.SendAsync("myFirstDevice", commandMessage);
        }

    このメソッドでは、ファイルをアップロードするコマンドとしてこのメッセージを識別するプロパティと、BLOB URI を保持するプロパティという 2 つのアプリケーション プロパティを含む、クラウドからデバイスへのメッセージが送信されます。 また、完全な配信の受信確認も要求されます。 2 つのアプリケーション プロパティの情報はメッセージ本文にシリアル化することができますが、情報のシリアル化と逆シリアル化を行うための追加処理が必要になります。

<!-- Links -->

[About Azure Storage]: https://azure.microsoft.com/en-us/documentation/articles/storage-create-storage-account/#create-a-storage-account

[IoT Hub Developer Guide - C2D]: iot-hub-devguide.md#c2d
[Azure IoT - Service SDK NuGet package]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[Transient Fault Handling]: https://msdn.microsoft.com/en-us/library/hh680901(v=pandp.50).aspx
[Get started with IoT Hub]: iot-hub-csharp-csharp-getstarted.md

<!-- Images -->








