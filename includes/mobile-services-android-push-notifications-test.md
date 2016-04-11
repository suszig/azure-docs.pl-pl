
###テスト用の Android エミュレーターの設定
このアプリケーションをエミュレーターで実行する場合は、Google API をサポートしている Android Virtual Device (AVD) を使用してください。

> [AZURE.IMPORTANT] Android Virtual Device でプッシュ通知を受信するために Google アカウントを設定する必要があります (エミュレーターに移動 **設定** ] をクリック **アカウントの追加**)。 さらに、エミュレーターがインターネットに接続されていることを確認します。

1.  **ツール**, 、クリックして **Android エミュレーター マネージャーを開く**, をデバイスを選択してクリックして **編集**します。

    ![Android Virtual Device Manager](./media/mobile-services-android-push-notifications-test/notification-hub-create-android-app7.png)

2. 選択 **Google APIs** で **ターゲット**, 、] をクリックし、 **[ok]**します。

    ![Android Virtual Device の編集](./media/mobile-services-android-push-notifications-test/notification-hub-create-android-app8.png)

3. 上部のツールバーをクリックして **実行**, 、アプリケーションを選択します。 これによりエミュレーターが起動し、アプリケーションが実行されます。

  アプリの取得、 *registrationId* GCM および通知ハブに登録します。

###新しい項目を挿入すると、通知が生成されます。

1. アプリケーションで、意味のあるテキストをなど入力 _新しいモバイル サービス タスク_ ] をクリックし、 **追加** ] ボタンをクリックします。

2. 画面の上端から下へスワイプし、通知を表示するためのデバイスの通知センターを開きます。

    ![通知センターでの通知の表示](./media/mobile-services-android-push-notifications-test/notification-area-received.png)
