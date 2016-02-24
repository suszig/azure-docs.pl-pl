###Mobile Engagement にプッシュ証明書へのアクセスを許可する

Mobile Engagement がユーザーに代わりプッシュ通知を送信できるようにするには、Mobile Engagement に証明書へのアクセスを許可する必要があります。 このためには、証明書を Mobile Engagement ポータルに構成および入力します。 説明したように、.p12 証明書を取得するかどうかを確認 [Apple のドキュメント](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)

1. Mobile Engagement ポータルに移動します。 正しいおよびをクリックすることを確認、 **の関心を引く** 下部にあるボタンをクリックします。

    ![](./media/mobile-engagement-ios-send-push/engage-button.png)

2. をクリックして、 **設定** Engagement ポータルのページです。 クリックし、 **ネイティブ プッシュ通知** セクション、p12 証明書をアップロードします。

    ![](./media/mobile-engagement-ios-send-push/engagement-portal.png)

3. p12 を選択してアップロードし、パスワードを入力します

    ![](./media/mobile-engagement-ios-send-push/native-push-settings.png)

##<a id="send"></a>アプリへ通知を送信します。

プッシュ通知をアプリに送る簡単なプッシュ通知キャンペーンを作成してみましょう。

1. 移動、 **到達** モバイル エンゲージメント ポータルでタブをクリックします。

2. クリックして **新しいお知らせ** プッシュ キャンペーンを作成するには

    ![](./media/mobile-engagement-ios-send-push/new-announcement.png)

3. キャンペーンの最初のフィールドを設定します

    ![](./media/mobile-engagement-ios-send-push/campaign-first-params.png)

    -   提供、 **名前** キャンペーン 
    -   選択、 **配信時刻** として **アプリ外のみ**: これは、テキストを扱う単純な Apple プッシュ通知タイプ。
    -   まず通知のテキスト入力、 **タイトル** これが、プッシュの最初の行になります。
    -   入力し、 **メッセージ** これが 2 番目の行になります

4. スクロール ダウンし、コンテンツ セクションの選択で **通知のみ**

    ![](./media/mobile-engagement-ios-send-push/campaign-content.png)

5. 最も基本的なキャンペーンの設定が完了しました。 今すぐ下にスクロールし、をクリックして **作成** してプッシュ通知キャンペーンを保存] ボタンをクリックします。 

6. 最後に -] をクリックして **Activate** プッシュ通知を送信します。 

    ![](./media/mobile-engagement-ios-send-push/campaign-activate.png)

7. 次のような通知センターの iOS デバイスで通知を受信することができます。

    ![](./media/mobile-engagement-ios-send-push/iphone-notification.png)

8. この iOS デバイスと組み合わせて使用する Apple Watch がある場合は、Apple Watch に通知が表示されます。

    ![](./media/mobile-engagement-ios-send-push/apple-watch.png)


 

 
