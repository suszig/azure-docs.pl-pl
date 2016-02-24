

>[AZURE.NOTE]この手順を完了するには、検証済みの電子メール アドレスを持つ Google アカウントが必要です。 新しい Google アカウントを作成するには、<a href="http://go.microsoft.com/fwlink/p/?LinkId=268302" target="_blank">accounts.google.com</a> にアクセスしてください。


1. 移動し、 <a href="http://cloud.google.com/console" target="_blank">Google Cloud Console</a> web サイトで、Google アカウント資格情報でサインインし、 **プロジェクトの作成**します。

    ![](./media/notification-hubs-android-get-started/mobile-services-google-new-project.png)   

    >[AZURE.NOTE]既存のプロジェクトが既に存在する場合に表示される、 <strong>プロジェクト</strong> ページが表示されます。 ダッシュボードで新しいプロジェクトを作成するには、<strong>[API Project]</strong> を展開し、<strong>[Other projects]</strong> の下の <strong>[Create]</strong> をクリックして、プロジェクト名を入力してから <strong>[Create project]</strong> をクリックします。

2. プロジェクト名を入力して、サービスの条項に同意し、] をクリックして **作成**します。 要求されると、SMS の確認を実行し、クリックして **作成** 再度します。

3. プロジェクト番号を書き留めて、 **プロジェクト** セクションです。 

    チュートリアルの後の方で、クライアントの PROJECT_ID 変数に、この値を設定します。

4. 左側の列で [ **APIs & auth**, 、] をクリックして **Api** 下へスクロールし、表示/非表示を有効にする] をクリックし、 **Google Cloud Messaging for Android** し、サービスの条項に同意します。 

    ![](./media/notification-hubs-android-get-started/mobile-services-google-enable-GCM.png)

5. クリックして **資格情報**, 、クリックして **新しいキーの作成** 

    ![](./media/notification-hubs-android-get-started/mobile-services-google-create-server-key.png)

6.  **新しいキーを作成**, 、クリックして **サーバー キー**します。 次のウィンドウでをクリックして **作成**します。

    ![](./media/notification-hubs-android-get-started/mobile-services-google-create-server-key2.png)

7. 書き留めて、 **API キー** 値。

    ![](./media/notification-hubs-android-get-started/mobile-services-google-create-server-key3.png) 

    この API キー値を使用して、Azure が GCM で認証し、アプリの代わりにプッシュ通知を送信できるようにします。


