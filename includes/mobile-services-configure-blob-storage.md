新しい Todo 項目が挿入された場合に SAS を生成する、新しい挿入スクリプトが登録されます。

0. まだストレージ アカウントを作成していない場合は、次を参照してください。 [をストレージ アカウントを作成する方法](../storage/storage-create-storage-account.md)します。

1.  [Azure クラシック ポータル](https://manage.windowsazure.com/), 、] をクリックして **ストレージ**, ストレージ アカウントをクリックし、クリックして **キーの管理**します。 

2. 書き留めて、 **ストレージ アカウント名** と **アクセス キー**します。

    ![](./media/mobile-services-configure-blob-storage/mobile-blob-storage-account-keys.png)

3. モバイル サービス] をクリックして、 **構成** ] タブで、下へスクロールして **アプリ設定** を入力し、 **名前** と **値** ストレージ アカウントから取得した次のそれぞれの [ **保存**します。

    + `STORAGE_ACCOUNT_NAME`
    + `STORAGE_ACCOUNT_ACCESS_KEY`

    ![](./media/mobile-services-configure-blob-storage/mobile-blob-storage-app-settings.png)

    ストレージ アカウントのアクセス キーは暗号化された状態でアプリケーション設定に保存されます。 このキーには、実行時にどのサーバー スクリプトからでもアクセスできます。 詳細については、次を参照してください。 [App settings]します。

4. [構成] タブのことを確認して [動的スキーマ](http://msdn.microsoft.com/library/windowsazure/b6bb7d2d-35ae-47eb-a03f-6ee393e170f7) が有効になっています。 TodoItem テーブルに新しい列を追加するには、動的スキーマが有効になっている必要があります。 運用環境のサービスでは、動的スキーマを有効にしないでください。

4. クリックして、 **データ** ] タブでをクリックし、 **TodoItem** テーブルです。 

5.   **Todoitem**, をクリックして、 **スクリプト** ] タブで選択し、 **挿入**, 、insert 関数を次のコードに置き換えて] をクリックして **保存**:

        var azure = require('azure');
        var qs = require('querystring');
        var appSettings = require('mobileservice-config').appSettings;
        
        function insert(item, user, request) {
            // Get storage account settings from app settings. 
            var accountName = appSettings.STORAGE_ACCOUNT_NAME;
            var accountKey = appSettings.STORAGE_ACCOUNT_ACCESS_KEY;
            var host = accountName + '.blob.core.windows.net';
        
            if ((typeof item.containerName !== "undefined") && (
            item.containerName !== null)) {
                // Set the BLOB store container name on the item, which must be lowercase.
                item.containerName = item.containerName.toLowerCase();
        
                // If it does not already exist, create the container 
                // with public read access for blobs.        
                var blobService = azure.createBlobService(accountName, accountKey, host);
                blobService.createContainerIfNotExists(item.containerName, {
                    publicAccessLevel: 'blob'
                }, function(error) {
                    if (!error) {
        
                        // Provide write access to the container for the next 5 mins.        
                        var sharedAccessPolicy = {
                            AccessPolicy: {
                                Permissions: azure.Constants.BlobConstants.SharedAccessPermissions.WRITE,
                                Expiry: new Date(new Date().getTime() + 5 * 60 * 1000)
                            }
                        };
        
                        // Generate the upload URL with SAS for the new image.
                        var sasQueryUrl = 
                        blobService.generateSharedAccessSignature(item.containerName, 
                        item.resourceName, sharedAccessPolicy);
        
                        // Set the query string.
                        item.sasQueryString = qs.stringify(sasQueryUrl.queryString);
        
                        // Set the full path on the new new item, 
                        // which is used for data binding on the client. 
                        item.imageUri = sasQueryUrl.baseUrl + sasQueryUrl.path;
        
                    } else {
                        console.error(error);
                    }
                    request.execute();
                });
            } else {
                request.execute();
            }
        }

    この操作により、TodoItem テーブルで挿入が発生したときに呼び出される関数が、新しいスクリプトに置き換わります。 この新しいスクリプトは、挿入のための新しい SAS を生成し (5 分間有効)、生成された SAS の値を返された項目の `sasQueryString` プロパティに割り当てます。 `imageUri` プロパティに新しい BLOB のリソース パスが設定され、クライアント UI でのバインド中にイメージを表示できるようになります。

    >[AZURE.NOTE] このコードは、個々 の BLOB に対して SAS を作成します。 同じ SAS を使用してコンテナーに複数の blob をアップロードする場合は、代わりに呼び出せる、 [generateSharedAccessSignature メソッド](http://go.microsoft.com/fwlink/?LinkId=390455)</a> で次のように、空の blob リソース名。 
    >                 
    >     blobService.generateSharedAccessSignature(containerName, '', sharedAccessPolicy);

次に、挿入時に生成される SAS を使用してイメージ アップロード機能を追加する、クイック スタート アプリケーションを更新します。
 
<!-- Anchors. -->

<!-- Images. -->

<!-- URLs. -->
[App settings]: http://msdn.microsoft.com/library/windowsazure/b6bb7d2d-35ae-47eb-a03f-6ee393e170f7


