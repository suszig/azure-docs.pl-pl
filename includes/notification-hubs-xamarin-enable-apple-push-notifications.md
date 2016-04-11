

Apple Push Notification サービス (APNS) を介してアプリ用にプッシュ通知を登録するには、新しいプッシュ証明書、アプリケーション ID、プロジェクトのプロビジョニング プロファイルを Apple の開発者ポータルで作成する必要があります。 アプリケーション ID には、プッシュ通知をアプリケーションで送受信できるようにする構成設定が含まれます。 これらの設定には、プッシュ通知を送受信するときに必要な Apple Push Notification サービス (APNS) と認証するためのプッシュ通知証明書が含まれます。 これらの概念の詳細については、公式を参照してください。 [Apple Push Notification Service](http://go.microsoft.com/fwlink/p/?LinkId=272584) ドキュメントです。


####プッシュ証明書用に証明書の署名要求ファイルを生成する

次の手順では、証明書の署名要求の作成手順について説明します。 これは、APNS で使用するプッシュ証明書の生成に使用されます。

1. Mac で、キーチェーン アクセス ツールを実行します。 開くことが、 **ユーティリティ** フォルダーまたは **他の** 起動パッド上のフォルダーです。

2. クリックして **キーチェーン アクセス**, 、展開 **Certificate Assistant**, 、順にクリックして **a Certificate Authority] 証明書を要求**します。

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)

3. 選択、 **ユーザーの電子メール アドレス** と **共通名** , 、ことを確認して **ディスクに保存** クリックして選択すると、 **続行**します。 ままにして、 **CA 電子メール アドレス** フィールドを空白は必要ありません。

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-csr-info.png)

4. 証明書署名要求 (CSR) ファイルの名前を入力 **名前を付けて保存**, 、内の場所を選択して **場所**, 、順にクリックして **保存**します。

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-save-csr.png)

    指定選択した場所に CSR ファイルが保存されます。既定の場所は [Desktop] です。 このファイル用に選択した場所を忘れないでください。


####アプリケーションをプッシュ通知に登録する

Apple でアプリケーション用の新しいの明示的なアプリケーション ID を作成し、さらにそれをプッシュ通知用に構成します。  

1. 移動、 [iOS プロビジョニング ポータル](http://go.microsoft.com/fwlink/p/?LinkId=272456) 、Apple のデベロッパー センター、Apple ID でログイン] をクリックして **識別子**, 、] をクリックし、 **App IDs**, 、最後にをクリックし、 **+** サインインすると、新しいアプリを登録します。

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-ios-appids.png)

2. 新しいアプリに次の 3 つのフィールドを更新し、クリックして **続行**:

    * **名前**: にアプリケーションのわかりやすい名前を入力、 **名前** フィールドに、 **アプリ ID の説明** セクションです。
    
    * **Bundle Identifier**: の下、 **明確なアプリ ID** セクションで、入力、 **Bundle Identifier** 形式で `<Organization Identifier>.<Product Name>` で説明したように、 [アプリ ディストリビューション ガイド](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8)します。 これは、アプリ用の XCode または Xamarin プロジェクトで使用されているものとも一致する必要があります。    
     
    * **プッシュ通知**: 確認、 **プッシュ通知** オプション、 **App Services** ] セクションで、します。

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-appid-info.png)

3.  確認アプリ ID 画面上の設定を確認し、後ことを確認する] をクリック **送信**

4.  新しいアプリ ID を送信したと表示されます、 **登録完了** 画面です。 クリックして **実行**します。

5. Developer センターで、アプリケーション ID の一覧から作成したアプリケーション ID を見つけ、その行をクリックします。 アプリケーション ID 行をクリックすると、アプリケーションの詳細が表示されます。 クリックして、 **編集** 下部にあるボタンをクリックします。

6. 画面の下部までスクロールし、をクリックして、 **Create Certificate]** セクションの下のボタン **Development Push SSL Certificate**します。

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)

    これで、"Add iOS Certificate" アシスタントが表示されます。

    > [AZURE.NOTE] このチュートリアルでは、開発証明書を使用します。 運用証明書の場合も同じ処理を行います。 通知の送信と同じ証明書の種類を使用するようにします。

7. クリックして **Choose File**, 、プッシュ証明書に、CSR を保存した場所に移動します。 クリックして **生成**します。

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)

8. ポータルで証明書を作成したら、 **ダウンロード** ] ボタンをクリックします。

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)

    これによって、署名証明書がダウンロードされ、コンピューターの Downloads フォルダーに保存されます。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)

    > [AZURE.NOTE] 既定では、ダウンロードしたファイルは開発証明書の名前は **aps_development.cer**します。

9. ダウンロードしたプッシュ証明書をダブルクリック **aps_development.cer**します。 下図のように、新しい証明書が Keychain にインストールされます。

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)

    > [AZURE.NOTE] 証明書の名前が異なる、可能性がありますが、それが付けられます **Apple Development iOS Push Services:**です。

10. Keychain access で、作成した新しいプッシュ証明書を右クリックし、 **証明書** カテゴリ。 をクリックして **エクスポート**, でファイルの名前を選択、 **.p12** 書式設定、およびクリックして **保存**します。

    エクスポートした .p12 証明書のファイル名と場所を記録します。 これは、Azure クラシック ポータルにアップロードして APNS との認証を有効にするために使用されます。



####アプリケーションのプロビジョニング プロファイルを作成する

1. 戻り、 <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS プロビジョニング ポータル</a>, [ **Provisioning Profiles**, を選択 **すべて**, 、順にクリック、 **+** 新しいプロファイルを作成する] ボタンをクリックします。 これにより、起動、 **Add iOS Provisiong Profile** ウィザード

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)

2. 選択 **iOS App Development** [ **開発** をクリックして、プロビジョニング プロファイルの種類として **続行**します。 


3. 次から先ほど作成したアプリ ID を選択、 **App ID** ドロップダウン リストをクリック **続行**

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)


4.  **証明書の選択** 画面で、コードが署名に使用する開発証明書を選択し、クリックして **続行**します。 これは署名証明書で作成したプッシュの証明書ではありません。

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)


5. 次に、選択、 **デバイス** テストを使用して [ **続行**

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)


6. 最後でプロファイルの名前を選んで **プロファイル名**, 、] をクリックして **生成**します。

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)



