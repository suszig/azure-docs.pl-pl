
オプションで、プッシュ通知を Azure へ発行する前に、ローカル コンピューターまたは VM 上で稼働するモバイル サービスを使用してプッシュ通知をテストできます。 テストするには、アプリケーションが使用している通知ハブに関する情報を web.config ファイルに設定する必要があります。 この情報は、ローカルで動作して通知ハブに接続する場合にだけ使用され、Azure へ発行した場合は無視されます。

1. 戻り、 **プッシュ** ] タブをクリックして、モバイル サービスの **通知ハブ** リンクします。

    ![](./media/mobile-services-dotnet-backend-configure-local-push/link-to-notification-hub.png)

    これによって、モバイル サービスが使用している通知ハブに移動します。

2. 通知ハブのページで、通知ハブの名前を書き留めて] をクリックし、 **[接続文字列の**です。

    ![](./media/mobile-services-dotnet-backend-configure-local-push/notification-hub-page.png)

3.  **接続情報にアクセス**, 、コピー、 **DefaultFullSharedAccessSignature** 接続文字列。

    ![](./media/mobile-services-dotnet-backend-configure-local-push/notification-hub-connection-string.png)

4. Visual Studio でのモバイル サービス プロジェクトで、サービスの Web.config ファイルを開きます **connectionStrings**, の接続文字列を置き換える **MS_NotificationHubConnectionString** 前の手順から接続文字列を使用します。

5.  **AppSettings**, の値を置き換える、 **MS_NotificationHubName** 通知ハブの名前を使用してアプリ設定です。

これで、モバイル サービス プロジェクトは、ローカルでの実行時に Azure 内の通知ハブに接続されるように構成されました。 Azure 内で実行する際に、Web.config プロジェクトの設定はポータルの設定で上書きされるため、ポータルの設定と同じ通知ハブ名と接続文字列を使用する必要がある点に注意してください。
