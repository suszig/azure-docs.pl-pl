

##証明書の署名要求ファイルを生成する

Apple Push Notification サービス (APNS) では、証明書を使用してプッシュ通知を認証します。 次の手順に従って、通知を送受信するために必要なプッシュ証明書を作成します。 これらの概念の詳細については、公式を参照してください。 [Apple Push Notification Service](http://go.microsoft.com/fwlink/p/?LinkId=272584) ドキュメントです。

Apple が署名済みのプッシュ証明書を生成するために使用する、証明書署名要求 (CSR: Certificate Signing Request) ファイルを生成します。

1. Mac で、キーチェーン アクセス ツールを実行します。 開くことが、 **ユーティリティ** フォルダーまたは **他の** 起動パッド上のフォルダーです。

2. クリックして **キーチェーン アクセス**, 、展開 **Certificate Assistant**, 、順にクリックして **a Certificate Authority] 証明書を要求**します。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)

3. 選択、 **ユーザーの電子メール アドレス** と **共通名** , 、ことを確認して **ディスクに保存** クリックして選択すると、 **続行**します。 ままにして、 **CA 電子メール アドレス** フィールドを空白は必要ありません。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-csr-info.png)

4. 証明書署名要求 (CSR) ファイルの名前を入力 **名前を付けて保存**, 、内の場所を選択して **場所**, 、順にクリックして **保存**します。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-save-csr.png)

    指定選択した場所に CSR ファイルが保存されます。既定の場所は [Desktop] です。 このファイル用に選択した場所を忘れないでください。

次に、アプリケーションを Apple に登録し、プッシュ通知を有効にし、このエクスポートした CSR をアップロードしてプッシュ証明書を作成します。

##アプリケーションをプッシュ通知に登録する

iOS アプリケーションにプッシュ通知を送信できるようにするには、アプリケーションを Apple に登録し、プッシュ通知にも登録する必要があります。  

1. アプリを登録済みでない場合に移動、 <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS プロビジョニング ポータル</a> 、Apple のデベロッパー センター、Apple ID でログイン] をクリックして **識別子**, 、順にクリックして **App IDs**, 、最後にをクリックし、 **+** サインインすると、新しいアプリを登録します。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids.png)


2. 新しいアプリに次の 3 つのフィールドを更新し、クリックして **続行**:

    * **名前**: にアプリケーションのわかりやすい名前を入力、 **名前** フィールドに、 **アプリ ID の説明** セクションです。
    
    * **Bundle Identifier**: の下、 **明確なアプリ ID** セクションで、入力、 **Bundle Identifier** 形式で `<Organization Identifier>.<Product Name>` で説明したように、 [アプリ ディストリビューション ガイド](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8)します。  *組織 Id* と *製品名* を使用して、XCode プロジェクトを作成するときに使用する組織 id と製品名と一致します。 次のスクリーン ショットで *NotificationHubs* 組織 id として使用し、 *GetStarted* 製品名として使用されます。 XCode プロジェクトで使用する値と一致していることを確認することで、XCode で正しい発行プロファイルが使用できるようになります。 
    
    * **プッシュ通知**: 確認、 **プッシュ通知** オプション、 **App Services** ] セクションで、します。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-appid-info.png)

    これで、アプリケーション ID が生成され、情報の確認が求められます。 クリックして **送信**


    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-confirm-new-appid.png)


    Once you click **Submit**, you will see the **Registration complete** screen, as shown below. Click **Done**.


    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-registration-complete.png)


3. Developer センターで、アプリケーション ID の一覧から作成したアプリケーション ID を見つけ、その行をクリックします。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids2.png)

    アプリケーション ID をクリックすると、アプリケーションの詳細が表示されます。 クリックして、 **編集** 下部にあるボタンをクリックします。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-edit-appid.png)

4. 画面の下部までスクロールし、をクリックして、 **Create Certificate]** セクションの下のボタン **Development Push SSL Certificate**します。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)

    これで、[Add iOS Certificate] アシスタントが表示されます。

    > [AZURE.NOTE] このチュートリアルでは、開発証明書を使用します。 運用証明書の場合も同じ処理を行います。 通知の送信と同じ証明書の種類を使用するようにします。

5. をクリックして **Choose File**, 最初のタスクで作成した CSR ファイルを保存した場所に移動してから、をクリックして、 **生成**します。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)

6. ポータルで証明書を作成したら、 **ダウンロード** ボタンをクリックし、をクリックして **実行**します。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)

    これによって、証明書がダウンロードされ、コンピューターの Downloads フォルダーに保存されます。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)

    > [AZURE.NOTE] 既定では、ダウンロードしたファイルは開発証明書の名前は **aps_development.cer**します。

7. ダウンロードしたプッシュ証明書をダブルクリック **aps_development.cer**します。

    下図のように、新しい証明書が Keychain にインストールされます。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)

    > [AZURE.NOTE] 証明書の名前が異なる、可能性がありますが、それが付けられます **Apple Development iOS Push Services:**です。

8. Keychain access で作成した新しいプッシュ証明書を右クリックし、 **証明書** カテゴリ。 をクリックして **エクスポート**, でファイルの名前を選択、 **.p12** 書式設定、およびクリックして **保存**します。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-export-cert-p12.png)

    エクスポートした .p12 証明書のファイル名と場所を書き留めます。 APNS での認証には、これが使用されます。

    >[AZURE.NOTE] このチュートリアルでは QuickStart.p12 ファイルを作成します。 ファイル名と場所は同じである必要はありません。


##アプリケーションのプロビジョニング プロファイルを作成する

1. 戻り、 <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS プロビジョニング ポータル</a>, [ **Provisioning Profiles**, を選択 **すべて**, 、順にクリック、 **+** 新しいプロファイルを作成する] ボタンをクリックします。 これにより、起動、 **Add iOS Provisiong Profile** ウィザード

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)

2. 選択 **iOS App Development** [ **開発** をクリックして、プロビジョニング プロファイルの種類として **続行**します。 


3. 次から先ほど作成したアプリ ID を選択、 **App ID** ドロップダウン リストをクリック **続行**

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)


4.  **証明書の選択** 画面で、コードが署名に使用する通常の開発証明書を選択してクリックして **続行**します。 これは作成したプッシュの証明書ではありません。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)


5. 次に、選択、 **デバイス** テストを使用して [ **続行**

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)


6. 最後でプロファイルの名前を選んで **プロファイル名**, 、] をクリックして **生成**します。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)


7. 新しいプロビジョニング プロファイルが作成されたら、それをクリックしてダウンロードし、Xcode の開発用コンピューターにインストールします。 クリックして **実行**します。

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-profile-ready.png)



