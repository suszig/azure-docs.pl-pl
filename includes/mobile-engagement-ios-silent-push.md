> [AZURE.IMPORTANT] モバイル サービスからプッシュ通知を受信するを有効にする必要があります `Silent Remote Notifications` アプリケーションにします。 Info.plist ファイルの UIBackgroundModes 配列に、リモート通知の値を追加する必要があります。

1. プロジェクト内の `info.plist` ファイルを開きます
2. 一覧 (`Information Property List`) の一番上の項目を右クリックして、新しい行を追加します

    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push1.png)

3. 新しい行に `Required background modes` を入力します

    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push2.png)

4. 行を展開するには、左向きの矢印をクリックします
5. 項目 0 に、次の値を追加します `App downloads content in response to push notifications`

    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push3.png)

6. 変更を加えた後、 info.plist XML には次のようなキーと値を含める必要があります。

        <key>UIBackgroundModes</key>
        <array>
        <string>remote-notification</string>
        </array>

7. 使用している場合は、 **Xcode 7 以上** と **iOS 9 +**:
    - 有効にする **プッシュ通知** ターゲットで > ターゲット名 > の機能です。

