<properties
    pageTitle="Mobile Services アプリケーションへのプッシュ通知の追加 (Xamarin.iOS) - Mobile Services"
    description="Azure Mobile Services を使用する Xamarin.iOS アプリでプッシュ通知を使用する方法について説明します。"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="dwrede"
    services="mobile-services"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="12/03/2015"
    ms.author="yuaxu"/>

# Mobile Services アプリへのプッシュ通知の追加

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-get-started-push](../../includes/mobile-services-selector-get-started-push.md)]

##概要

このトピックでは、Azure Mobile Services を使用して Xamarin.iOS 8 アプリケーションにプッシュ通知を送信する方法について説明します。 このチュートリアルでは、Apple Push Notification サービス (APNS) を使用したプッシュ通知を追加、 [Get started with Mobile Services] プロジェクトです。 完了すると、モバイル サービスは、レコードが挿入されるたびにプッシュ通知を送信します。

このチュートリアルには、次のものが必要です。

+ iOS 8 デバイス (iOS シミュレーターでプッシュ通知をテストすることはできません)
+ iOS Developer Program メンバーシップ
+ [Xamarin.iOS Studio]
+ [Azure モバイル サービス コンポーネント]

>[AZURE.IMPORTANT] APNS 要件によりを展開して、エミュレーターで、iOS 対応デバイス (iPhone または iPad) の代わりにプッシュ通知をテストします。

APNS では、証明書を使用してモバイル サービスを認証します。 次の手順に従って、必要な証明書を作成し、Mobile Service にアップロードしてください。 公式な APNS 機能のドキュメントを参照してください。 [Apple Push Notification Service]します。

## <a name="certificates"></a>証明書の署名要求ファイルを生成する

まず、Apple が署名証明書を生成するために使用する、証明書署名要求 (CSR: Certificate Signing Request) ファイルを生成する必要があります。

1. ユーティリティから実行、 **Keychain Access] ツール**します。

2. クリックして **キーチェーン アクセス**, 、展開 **Certificate Assistant**, 、順にクリックして **a Certificate Authority] 証明書を要求**します。

    ![][5]

3. 入力、 **ユーザーの電子メール アドレス**, を入力、 **共通名** 値のことを確認 **ディスクに保存** クリックして選択すると、 **続行**します。

    ![][6]

4. 証明書署名要求 (CSR) ファイルの名前を入力 **名前を付けて保存**, 、内の場所を選択して **場所**, 、順にクリックして **保存**します。

    ![][7]

    選択した場所を忘れないでください。

次に、アプリケーションを Apple に登録し、プッシュ通知を有効にし、このエクスポートした CSR をアップロードしてプッシュ通知を作成します。

## <a name="register"></a>アプリケーションをプッシュ通知に登録する

モバイル サービスから iOS アプリケーションにプッシュ通知を送信可能にするには、アプリケーションを Apple に登録し、プッシュ通知にも登録する必要があります。

1. アプリを登録済みでない場合に移動、 <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS プロビジョニング ポータル</a> 、Apple のデベロッパー センター、Apple ID でログイン] をクリックして **識別子**, 、順にクリックして **App IDs**, 、最後にをクリックし、 **+** 署名アプリがアプリ ID を作成します。

    ![][102]

2. アプリケーションの名前を入力 **説明**, を入力し、一意に注意してください **Bundle Identifier**, [App Services] セクションでプッシュ通知] オプションをオンにし、[クリックして **続行**します。 この例は、ID を使用して **MobileServices.Quickstart** が再利用しないこの同じ ID ようにアプリケーション Id はすべてのユーザー間で一意である必要がありますが、します。 そのため、自分のフル ネームやイニシャルをアプリケーション名の後に付加することをお勧めします。

    ![][103]

    アプリ ID を生成し、要求すると、この **送信** 情報。 クリックして **送信**します。

    ![][104]

    クリックすると **送信**, が表示されます、 **登録完了** 画面で、次のようにします。 クリックして **実行**します。

    ![][105]

3. 作成したアプリケーション ID を見つけ、その行をクリックします。

    ![][106]

    アプリケーション ID をクリックすると、アプリケーションとアプリケーション ID の詳細が表示されます。 クリックして、 **設定** ] ボタンをクリックします。

    ![][107]

4. 画面の下部までスクロールし、をクリックして、 **Create Certificate]** セクションの下のボタン **Development Push SSL Certificate**します。

    ![][108]

    これで、[Add iOS Certificate] アシスタントが表示されます。

    メモ: このチュートリアルでは開発証明書を使用します。 運用証明書の場合も同じ処理を行います。 証明書を Mobile Services にアップロードするときと同じ証明書タイプを設定してください。

5. クリックして **Choose File**, 、前の手順で CSR ファイルを保存した場所に移動してから、をクリックして、 **生成**します。

    ![][110]

6. ポータルで証明書を作成したら、 **ダウンロード** ボタンをクリックし、をクリックして **実行**します。

    ![][111]

    これによって、署名証明書がダウンロードされ、コンピューターの Downloads フォルダーに保存されます。

    ![][9]

    メモ: 既定では、ダウンロードした開発証明書ファイルの名前は <strong>aps_development.cer</strong> になっています。

7. ダウンロードしたプッシュ証明書をダブルクリック **aps_development.cer**します。

    下図のように、新しい証明書が Keychain にインストールされます。

    ![][10]

    メモ: 証明書の名前は異なることがありますが、名前の前に <strong>Apple Development iOS Push Notification Services:</strong> が付きます。

後で、この証明書を使用して .p12 ファイルを生成し、それを Mobile Service にアップロードして APNS による認証を有効にします。

## <a name="profile"></a>アプリケーションのプロビジョニング プロファイルを作成する

1. 戻り、 <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS プロビジョニング ポータル</a>, [ **Provisioning Profiles**, を選択 **すべて**, 、順にクリック、 **+** 新しいプロファイルを作成する] ボタンをクリックします。 これにより、起動、 **Add iOS Provisiong Profile** ウィザード。

    ![][112]

2. 選択 **iOS App Development** [ **開発** をクリックして、プロビジョニング プロファイルの種類として **続行**します。

3. モバイル サービスのクイック スタート アプリケーションのアプリ ID を次に、選択、 **App ID** ドロップダウン リストをクリック **続行**します。

    ![][113]

4.  **証明書の選択** 画面で、以前に作成された証明書を選択してクリックして **続行**します。

    ![][114]

5. 次に、選択、 **デバイス** テストを使用して [ **続行**します。

    ![][115]

6. プロファイルの名前を最後に、選択 **プロファイル名**, をクリックして **生成**, 、] をクリック **実行**します。

    ![][116]

    新しいプロビジョニング プロファイルが作成されます。

    ![][117]

## <a name="configure-mobileServices"></a>プッシュ要求を送信するように Mobile Services を構成する

アプリケーションを APNS に登録し、プロジェクトを構成した後で、モバイル サービスを APNS と統合するように構成する必要があります。

1. Keychain access では、新しい証明書を右クリックし、をクリックして **エクスポート**, 、ファイルの名前を選択、 **.p12** 、書式を設定し、クリックして **保存**します。

    ![][28]

    エクスポートした証明書のファイル名と場所を書き留めます。

2. ログオン、 [Azure classic portal], 、] をクリックして **Mobile Services**, 、アプリケーションをクリックします。

    ![][18]

3. クリックして、 **プッシュ** ] タブでをクリックし、 **アップロード** [ **apple プッシュ通知の設定**します。

    ![][19]

    [証明書のアップロード] ダイアログ ボックスが表示されます。

4. をクリックして **ファイル**, をエクスポートした証明書 .p12 ファイルを選択し、入力、 **パスワード**, 、ことを確認して、正しい **モード** は選択すると、チェック マーク アイコンをクリックし、クリックして **保存**します。

    ![][20]

APNS と連携するようにモバイル サービスが構成されました。

## <a name="configure-app"></a>Xamarin.iOS アプリケーションを構成する

1. Xamarin.Studio で開く **Info.plist**, 、および更新、 **Bundle Identifier** 前に作成した ID を持つ。

    ![][121]

2. 下へスクロールして **Background Modes** を確認し、 **Enable Background Modes** ボックスおよび **リモート通知** ボックス。

    ![][122]

3. 開くソリューション パネルでプロジェクトをダブルクリックして **プロジェクト オプション**します。

4.  選択 **iOS Bundle Signing** [ **ビルド**, 、し、対応する選択 **Identity** と **プロビジョニング プロファイル** だけこのプロジェクトを設定する必要があります。

    ![][120]

    これで、Xamarin プロジェクトはコード署名のために新しいプロファイルを使用するようになります。 公式の Xamarin デバイス プロビジョニングのドキュメントについて、次を参照してください。 [Xamarin Device Provisioning]します。

## <a name="add-push"></a>アプリケーションにプッシュ通知を追加する

1. Xamarin.Studio で AppDelegate.cs プロジェクト ファイルを開き、次のプロパティを追加します。

        public string DeviceToken { get; set; }

2. 開いている、 **TodoItem** クラスにし、次のプロパティを追加します。

        [JsonProperty(PropertyName = "deviceToken")]
        public string DeviceToken { get; set; }

3.  **QSTodoService**, 、する既存のクライアント宣言をオーバーライドします。

        public MobileServiceClient client { get; private set; }

4. そのため、次のメソッドを追加 **AppDelegate** がプッシュ通知を登録するには、後でクライアントを取得します。

        public MobileServiceClient GetClient {
            get{
                return client;
            }
        }

5.  **AppDelegate**, 、オーバーライド、 **FinishedLaunching** イベント。

        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            // registers for push for iOS8
            var settings = UIUserNotificationSettings.GetSettingsForTypes(
                UIUserNotificationType.Alert
                | UIUserNotificationType.Badge
                | UIUserNotificationType.Sound,
                new NSSet());

            UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications();

            return true;
        }

6.  **AppDelegate**, 、オーバーライド、 **RegisteredForRemoteNotifications** イベント。

        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            // Modify device token
            DeviceToken = deviceToken.Description;
            DeviceToken = DeviceToken.Trim ('<', '>').Replace (" ", "");

            // Get Mobile Services client
            MobileServiceClient client = QSTodoService.DefaultService.GetClient;

            // Register for push with Mobile Services
            IEnumerable<string> tag = new List<string>() { "uniqueTag" };
            var push = client.GetPush ();
            push.RegisterNativeAsync (DeviceToken, tag);
        }

7.  **AppDelegate**, 、オーバーライド、 **ReceivedRemoteNotification** イベント。

        public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
        {
            Debug.WriteLine(userInfo.ToString());
            NSObject inAppMessage;

            bool success = userInfo.TryGetValue(new NSString("inAppMessage"), out inAppMessage);

            if (success)
            {
                var alert = new UIAlertView("Got push notification", inAppMessage.ToString(), null, "OK", null);
                alert.Show();
            }
        }

8.  **QSTodoListViewController**, 、変更、 **OnAdd** に格納されているデバイス トークンを取得する操作 **AppDelegeate**, に格納し、 **TodoItem** 追加されます。

        string deviceToken = ((AppDelegate)UIApplication.SharedApplication.Delegate).DeviceToken;

        var newItem = new TodoItem()
        {
            Text = itemText.Text,
            Complete = false,
            DeviceToken = deviceToken
        };

これで、アプリケーションがプッシュ通知をサポートするように更新されました。

## <a name="update-scripts"></a>Azure クラシック ポータルで登録されている挿入スクリプトを更新する

1.  [Azure classic portal], をクリックして、 **データ** ] タブでをクリックし、 **TodoItem** テーブル。

    ![][21]

2.  **Todoitem**, をクリックして、 **スクリプト** ] タブで選択し、 **挿入**します。

    ![][22]

    これで、挿入が発生したときに呼び出される関数が表示されます、 **TodoItem** テーブルです。

3. クリックして insert 関数を次のコードに置き換えます **保存**:

        function insert(item, user, request) {
            request.execute();
            // Set timeout to delay the notification, to provide time for the
            // app to be closed on the device to demonstrate toast notifications
            setTimeout(function() {
                push.apns.send("uniqueTag", {
                    alert: "Toast: " + item.text,
                    payload: {
                        inAppMessage: "Hey, a new item arrived: '" + item.text + "'"
                    }
                });
            }, 2500);
        }

    これを使用してを新しい挿入スクリプトを登録、 [apns object] 、挿入要求で指定されたデバイスにプッシュ通知 (挿入されたテキスト) を送信します。

   >[AZURE.NOTE] このスクリプトを提供する通知の送信を遅らせて時間、トースト通知を受信するアプリケーションを閉じます。

## <a name="test"></a>アプリケーションでプッシュ通知をテストする

1. キーを押して、 **実行** をクリックして、プロジェクトをビルドし、iOS 対応のデバイスでアプリケーションを開始 **OK** プッシュ通知を受け入れます

    ![][23]

   >[AZURE.NOTE] アプリからプッシュ通知を明示的に同意する必要があります。 これが必要であるのは、初めてアプリケーションを実行するときだけです。

2. アプリケーションで、意味のあるテキストをなど入力 _新しいモバイル サービス タスク_ プラス記号をクリックして (**+**) のアイコン。

    ![][24]

3. 確認通知を受信し、] をクリックして **OK** を通知を破棄します。

    ![][25]

4. 手順 2 を繰り返してすぐにアプリケーションを閉じ、次のトーストが表示されることを確認します。

    ![][26]

これで、このチュートリアルは終了です。

<!-- Anchors. -->
[Generate the certificate signing request]: #certificates
[Register your app and enable push notifications]: #register
[Create a provisioning profile for the app]: #profile
[Configure Mobile Services]: #configure-mobileServices
[Configure the Xamarin.iOS App]: #configure-app
[Update scripts to send push notifications]: #update-scripts
[Add push notifications to the app]: #add-push
[Insert data to receive notifications]: #test

<!-- Images. -->

[5]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-ios-push-step5.png
[6]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-ios-push-step6.png
[7]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-ios-push-step7.png

[9]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-ios-push-step9.png
[10]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-ios-push-step10.png

[17]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-ios-push-step17.png
[18]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-selection.png
[19]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-push-tab-ios.png
[20]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-push-tab-ios-upload.png
[21]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-portal-data-tables.png
[22]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-insert-script-push2.png
[23]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-quickstart-push1-ios.png
[24]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-quickstart-push2-ios.png
[25]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-quickstart-push3-ios.png
[26]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-quickstart-push4-ios.png
[28]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-ios-push-step18.png

[101]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-ios-push-01.png
[102]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-ios-push-02.png
[103]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-ios-push-03.png
[104]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-ios-push-04.png
[105]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-ios-push-05.png
[106]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-ios-push-06.png
[107]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-ios-push-07.png
[108]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-ios-push-08.png

[110]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-ios-push-10.png
[111]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-ios-push-11.png
[112]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-ios-push-12.png
[113]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-ios-push-13.png
[114]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-ios-push-14.png
[115]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-ios-push-15.png
[116]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-ios-push-16.png
[117]: ./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-ios-push-17.png

[120]:./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-ios-push-20.png
[121]:./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-ios-push-21.png
[122]:./media/partner-xamarin-mobile-services-ios-get-started-push/mobile-services-ios-push-22.png

[Xamarin.iOS Studio]: http://xamarin.com/platform
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456
[Mobile Services iOS SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Apple Push Notification Service]: http://go.microsoft.com/fwlink/p/?LinkId=272584
[Get started with Mobile Services]: mobile-services-ios-get-started.md

[Xamarin Device Provisioning]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/


[Azure classic portal]: https://manage.windowsazure.com/
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333
[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
[completed example project]: http://go.microsoft.com/fwlink/p/?LinkId=331303
[Xamarin.iOS]: http://xamarin.com/download


