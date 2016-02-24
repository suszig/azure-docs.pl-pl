
1.  [Azure ポータル]( https://azure.portal.com/), 、] をクリックして **参照** > **App Services**, 、モバイル アプリ バックエンド順にクリックして > **設定をすべて**, 、[ **Mobile** ] をクリックして **プッシュ**します。

2. プッシュ通知サービス] をクリックして **Windows (WNS)**, 、入力、 **セキュリティ キー** (クライアント シークレット) と **パッケージ SID** Live サービス サイトから取得した] をクリックし、 **保存**します。

    ![ポータルで GCM API キーを設定する](./media/app-service-mobile-configure-wns/mobile-push-wns-credentials.png)

これで、モバイル アプリ バックエンドは、通知ハブを使用している Windows アプリに WNS を使用してプッシュ通知を送信するように構成されました。

