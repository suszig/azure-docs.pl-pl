
1. 移動し、 [Google Cloud Console](https://console.developers.google.com/project), 、Google アカウントの資格情報でサインインします。 
 
2. クリックして **プロジェクトの作成**, プロジェクト名を入力して、をクリックして **作成**します。 要求されると、SMS の確認を実行し、クリックして **作成** 再度します。

    ![](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-new-project.png)   

     新しい入力 **プロジェクト名** ] をクリック **プロジェクトの作成**します。

3. プロジェクト番号を書き留めて、 **プロジェクト** セクションです。 この値として設定する必要があります、 *PROJECT_ID* クライアントで変数です。

4. プロジェクト ダッシュ ボード] をクリックして **Google Api を使用して** > **Cloud Messaging for Android**, 、次のページをクリックして **を有効にする API**します。 

5. API マネージャーをクリックして **資格情報** > **資格情報の追加** > **API キー**します。 

    ![](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-create-server-key.png)

6.  **新しいキーを作成**, 、] をクリックして **サーバー キー**, をキーの名前を入力して、クリックして **作成**します。

7. 書き留めて、 **API キー** 値。

    この API キー値を使用して、Azure が GCM で認証し、アプリの代わりにプッシュ通知を送信できるようにします。


