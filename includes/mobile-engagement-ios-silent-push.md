> [AZURE.IMPORTANT] モバイル サービスからプッシュ通知を受信するを有効にする必要があります `サイレント リモート通知` アプリケーションにします。 Info.plist ファイルの UIBackgroundModes 配列に、リモート通知の値を追加する必要があります。

1. 開いている `info.plist` プロジェクト内のファイル
2. 一覧の一番上の項目を右クリックして (`情報プロパティ リスト`) し、新しい行を追加

    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push1.png)

3. 新しい行に入力 `バック グラウンド モードに必要な`

    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push2.png)

4. 行を展開するには、左向きの矢印をクリックします
5. 項目の 0 に、次の値を追加 `アプリへのプッシュ通知の応答のコンテンツをダウンロードする`

    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push3.png)

6. 変更を加えた後、 info.plist XML には次のようなキーと値を含める必要があります。

        <key>UIBackgroundModes</key>
        <array>
        <string>remote-notification</string>
        </array>

7. **Xcode 7+** および **iOS 9+** を使用している場合:
    - [ターゲット]、ターゲット名、[機能] の順に選択し、**[プッシュ通知]** を有効にします。





