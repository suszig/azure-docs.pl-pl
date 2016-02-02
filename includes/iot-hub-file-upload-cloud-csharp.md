## Azure ストレージ アカウントのプロビジョニング

シミュレーションされたデバイスが Azure Storage BLOB にファイルをアップロードするため、Azure ストレージ アカウントが必要です。 、既存のものを使用したり、新しいを作成するのには、[Azure ストレージに関する] の手順に従います。 ストレージ アカウント接続文字列はメモしておいてください。

## シミュレート済みデバイスへの Azure BLOB URI の送信

このセクションでは、「IoT Hub を使用したクラウドからデバイスへのメッセージの送信」で作成した **SendCloudtoDevice** コンソール アプリケーションを変更して、共有アクセス署名と共に Azure BLOB URI が含まれるようにします。 これにより、クラウド バックエンドは、クラウドからデバイスへのメッセージの受信者にのみ BLOB への書き込みアクセス権を付与できます。

1. Visual Studio で **SendCloudtoDevice** プロジェクトを右クリックし、**[NuGet パッケージの管理...]** をクリックします。

    [NuGet パッケージの管理] ウィンドウが表示されます。

2. 検索 `WindowsAzure.Storage`, 、] をクリックして **インストール**, 、使用条件に同意します。

    これをダウンロード、インストール、およびへの参照を追加、 [Microsoft Azure Storage SDK](https://www.nuget.org/packages/WindowsAzure.Storage/)します。

3. **Program.cs** ファイルの先頭に、次のステートメントを追加します。

        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;

4. **Program** クラスで、次のクラス フィールドを追加し、ストレージ アカウントの接続文字列を置き換えます。

     static string storageConnectionString = "{storage connection string}";

 次に、次のメソッドを追加します (任意の BLOB コンテナー名に置き換えることができますが、このチュートリアルでは **iothubfileuploadtutorial** を使用します)。

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

5. 次の方法で **SendCloudToDeviceMessageAsync** を変更します。

     private async static Task SendCloudToDeviceMessageAsync()
     {
         var commandMessage = new Message();
         commandMessage.Properties["command"] = "FileUpload";
         commandMessage.Properties["fileUri"] = await GenerateBlobUriAsync();
         commandMessage.Ack = DeliveryAcknowledgement.Full;
    
         await serviceClient.SendAsync("myFirstDevice", commandMessage);
     }

 このメソッドでは、ファイルをアップロードするコマンドとしてこのメッセージを識別するプロパティと、BLOB URI を保持するプロパティという 2 つのアプリケーション プロパティを含む、クラウドからデバイスへのメッセージが送信されます。 また、完全な配信の受信確認も要求されます。 2 つのアプリケーション プロパティの情報はメッセージ本文にシリアル化することができますが、情報のシリアル化と逆シリアル化を行うための追加処理が必要になります。















[about azure storage]: https://azure.microsoft.com/en-us/documentation/articles/storage-create-storage-account/#create-a-storage-account 
[iot hub developer guide - c2d]: iot-hub-devguide.md#c2d 
[azure iot - service sdk nuget package]: https://www.nuget.org/packages/Microsoft.Azure.Devices/ 
[transient fault handling]: https://msdn.microsoft.com/en-us/library/hh680901(v=pandp.50).aspx 
[get started with iot hub]: iot-hub-csharp-csharp-getstarted.md 

