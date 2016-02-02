>[AZURE.NOTE]この手順を完了するには、検証済みの電子メール アドレスを持つ Google アカウントが必要になります。 新しい Google アカウントを作成するには <a href="http://go.microsoft.com/fwlink/p/?LinkId=268302" target="_blank">accounts.google.com</a>します。


1. 移動し、 <a href="http://cloud.google.com/console" target="_blank">Google Cloud Console</a> web サイトで、Google アカウント資格情報でサインインし、 **プロジェクトの作成**します。

    ![](./media/notification-hubs-android-get-started/mobile-services-google-new-project.png)
    >[AZURE.NOTE]既存のプロジェクトが既に存在する場合に表示される、 <strong>プロジェクト</strong> ページが表示されます。 ダッシュ ボードから新しいプロジェクトを作成するには展開 <strong>API プロジェクト</strong>, 、クリックして <strong>を作成する.</strong> [ <strong>他のプロジェクト</strong>, プロジェクト名を入力し、クリックして <strong>プロジェクトの作成</strong>します。

2. プロジェクトの名前を入力し、サービスの条件に同意して、**[Create]** をクリックします。 要求された SMS の確認を実行し、**[Create]** をもう一度クリックします。

3. **[Projects]** セクションに表示されたプロジェクト番号をメモしておきます。

    チュートリアルの後の方で、クライアントの PROJECT_ID 変数に、この値を設定します。

4. 左側の列の **[APIs & auth]** を展開して **[APIs]** をクリックしてから下へスクロールし、トグルをクリックして **[Google Cloud Messaging for Android]** を有効にして、サービスの条件に同意します。

    ![](./media/notification-hubs-android-get-started/mobile-services-google-enable-GCM.png)

5. **[Credentials]**、**[Create new Key]** の順にクリックします。

    ![](./media/notification-hubs-android-get-started/mobile-services-google-create-server-key.png)

6. **[Create a new key]** で、**[Server key]** をクリックします。 次のウィンドウで **[Create]** をクリックします。

    ![](./media/notification-hubs-android-get-started/mobile-services-google-create-server-key2.png)

7. **[API KEY]** の値をメモしておきます。

    ![](./media/notification-hubs-android-get-started/mobile-services-google-create-server-key3.png)

    この API キー値を使用して、Azure が GCM で認証し、アプリの代わりにプッシュ通知を送信できるようにします。






