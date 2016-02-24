>[AZURE.NOTE] この手順を完了するには、検証済みの電子メール アドレスを持つ Google アカウントが必要です。 新しい Google アカウントを作成するには、<a href="http://go.microsoft.com/fwlink/p/?LinkId=268302" target="_blank">accounts.google.com</a> にアクセスしてください。

1. 移動し、 [Google Cloud Console](https://console.developers.google.com/project) 、Google アカウント資格情報でサインインし、クリックして **プロジェクトの作成**します。

    ![](./media/mobile-engagement-enable-google-cloud-messaging/new-project.png)   

    ![](./media/mobile-engagement-enable-google-cloud-messaging/new-project-2.png)   

2. プロジェクト名を入力して、サービスの条項に同意し、] をクリックして **作成**します。 要求されると、SMS の確認を実行し、クリックして **作成** 再度します。

3. プロジェクト番号を書き留めて、 **プロジェクト** セクションです。 この番号は、チュートリアルの後半で Android マニフェスト ファイルを設定する際に必要になります。 

    ![](./media/mobile-engagement-enable-google-cloud-messaging/project-number.png)   

4. 左側の列で [ **APIs & auth**, 、] をクリックして **Api** 下へスクロールし、をクリックし、 **Cloud Messaging for Android**します。 次のページをクリックして **を有効にする API** し、サービスの条項に同意します。 

    ![](./media/mobile-engagement-enable-google-cloud-messaging/enable-GCM.png)

    ![](./media/mobile-engagement-enable-google-cloud-messaging/enable-gcm-2.png)

5. クリックして **資格情報**, 、クリックして **資格情報の追加**]-> [**API キー** 

    ![](./media/mobile-engagement-enable-google-cloud-messaging/create-server-key.png)

6.  **新しいキーを作成**, 、クリックして **サーバー キー**します。 次のウィンドウでをクリックして **作成**します。

    ![](./media/mobile-engagement-enable-google-cloud-messaging/create-server-key5.png)


    ![](./media/mobile-engagement-enable-google-cloud-messaging/create-server-key6.png) 

7. 書き留めて、 **API キー** 値。 この API キーの値は、後で [ネイティブ プッシュ通知] セクションで構成する際に使用するされます。


