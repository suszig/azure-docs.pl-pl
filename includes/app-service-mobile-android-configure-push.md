1. [Azure ポータル](https://azure.portal.com/), 、] をクリックして **参照** > **App Services**, を検索して、Mobile App バックエンドをクリックして > **設定をすべて**, 、[ **Mobile** ] をクリックして **プッシュ**します。

2. [プッシュ通知サービス] で、**[Google (GCM)]** をクリックし、前の手順で GCM から取得した **[API キー]** の値を入力し、**[保存]** をクリックします。

    ![ポータルで GCM API キーを設定する](./media/app-service-mobile-android-configure-push/mobile-push-api-key.png)

これで、モバイル アプリ バックエンドは、通知ハブを使用している Android デバイスで実行中のアプリに GCM を使用してプッシュ通知を使用するように構成されました。










