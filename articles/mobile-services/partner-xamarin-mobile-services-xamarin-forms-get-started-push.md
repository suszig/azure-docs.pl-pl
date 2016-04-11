 <properties
    pageTitle="Mobile Services アプリケーション (Xamarin.Forms) へのプッシュ通知の追加  - Mobile Services"
    description="Azure Mobile Services の Xamarin.Forms アプリでプッシュ通知を使用する方法について説明します。"
    documentationCenter="xamarin"
    authors="wesmc7777"
    manager="dwrede"
    services="mobile-services"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.workload="mobile"
    ms.date="10/05/2015"
    ms.author="wesmc"/>

#  Xamarin.Forms アプリにプッシュ通知を追加する

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;

[AZURE.INCLUDE [mobile-services-selector-get-started-push](../../includes/mobile-services-selector-get-started-push.md)]

##概要

このチュートリアルでは、Azure Mobile Services を使用して、iOS、Android、および Windows Xamarin.Forms ソリューションの Windows Phone アプリにプッシュ通知を送信する方法を紹介します。 最初にモバイル サービスを作成します。 次に、スタート サンプルをダウンロードし、適切なプッシュ通知サービスに登録してから、これらのサービスから通知を受信できるようにコードをソリューションに追加します。

このチュートリアルを完了すると、いずれかのアプリケーションでユーザーがタスクを追加するたびに、モバイル サービスからプッシュ通知が送信されるようになります。 完全なサンプルを見つけることができます: [Completed Xamarin.Forms Azure Push Notification Sample]します。

このチュートリアルには、次のものが必要です。

+ iOS 8 デバイス (iOS シミュレーターでプッシュ通知をテストすることはできません)
+ iOS Developer Program メンバーシップ
+ [Xamarin.iOS Studio]
+ [Azure モバイル サービス コンポーネント]
+ アクティブな Google アカウント
+ [Google Cloud Messaging Client Component]. このコンポーネントは、チュートリアル中に追加します。

このトピックの内容:

1. [新しいモバイル サービスを作成する](#create-service)
2. [スタート サンプルをダウンロードおよび構成する](#download-starter-sample)
4. [Xamarin.Forms.iOS アプリにプッシュ通知を追加する](#iOS)
5. [Xamarin.Forms.Android アプリにプッシュ通知を追加する](#Android)
6. [Xamarin.Forms.Windows アプリにプッシュ通知を追加する](#Windows)
7. [すべてのアプリにプッシュ通知を送信する Azure テーブルの挿入スクリプトを更新する](#all-apps)

## <a name="create-service"> </a>新しいモバイル サービスを作成する

[AZURE.INCLUDE [mobile-services-create-new-service-data](../../includes/mobile-services-create-new-service-data.md)]

アプリケーション データを新しいモバイル サービスに格納できるようにするには、最初に新しいテーブルを作成する必要があります。

1. Azure クラシック ポータルで、クリックして **Mobile Services**, 、し、先ほど作成したモバイル サービスをクリックします。

2. クリックして、 **データ** ] タブの [ **[作成**します。

    ![][123]

    これを表示、 **新しいテーブルを作成する** ダイアログ。

3.  **テーブル名** 型 _TodoItem_, 、チェック ボタンをクリックします。

    ![][124]

    これが作成された新しいストレージ テーブル **TodoItem** で既定のアクセス許可セット、つまり、アプリの任意のユーザーがアクセスして、テーブル内のデータを変更できます。

    > [AZURE.NOTE] モバイル サービスのクイック スタートでも同じテーブル名を使用するとします。 ただし、それぞれのテーブルは、特定のモバイル サービスに固有のスキーマ内で作成されています。 これには、複数のモバイル サービスで同じデータベースを使用した場合にデータの競合を回避する目的があります。

4. [新規] をクリックして **TodoItem** テーブルし、データ行がないことを確認します。

5. クリックして、 **列** ] タブで、1 つのみがあることを確認し、 **id** 列が自動的に作成されます。

    これは、Mobile Services のテーブルの最小要件です。

    > [AZURE.NOTE] モバイル サービスで動的スキーマが有効な場合の新しい列と自動的に作成または更新操作が JSON オブジェクトに挿入して、モバイル サービスに送信されます。

これで、新しいモバイル サービスをアプリケーションのデータ ストレージとして使用する準備が整いました。

## <a name="download-starter-sample"></a>スタート サンプルをダウンロードおよび構成する
既存のサンプルにプッシュ通知を追加します。

1. 次のサンプルのダウンロード: [Xamarin.Forms Azure Push Notification Starter Sample]します。

2.  [Azure classic portal], 、] をクリックして **Mobile Services**, をモバイル サービスをクリックします。 クリックして、 **ダッシュ ボード** タブし、メモ、 **サイトの URL**します。 クリックし、 **キーの管理** し、メモ、 **アプリケーション キー**します。 これらの値は、アプリケーション コードからモバイル サービスにアクセスするときに必要になります。

3.   **Todoazure (portable)** 、開いているソリューションのプロジェクト、 **Constants.cs** ファイルで置き換えます `ApplicationURL` と `ApplicationKey` サイト URL とアプリケーションを前の手順で取得したキーします。

## <a name="iOS"></a>Xamarin.Forms.iOS アプリにプッシュ通知を追加する

Apple Push Notification サービス (APNS) を使用して、iOS アプリにプッシュ通知を追加します。 アクティブな Google アカウントを必要と [Google Cloud Messaging Client Component]します。

>[AZURE.IMPORTANT] Apple Push Notification サービス (APNS) の要件によりを展開して、エミュレーターで、iOS 対応デバイス (iPhone または iPad) の代わりにプッシュ通知をテストします。

APNS では、証明書を使用してモバイル サービスを認証します。 次の手順に従って、必要な証明書を作成し、Mobile Service にアップロードしてください。 公式な APNS 機能のドキュメントを参照してください。 [Apple Push Notification Service]します。

### <a name="certificates"></a>証明書の署名要求ファイルを生成する

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

### <a name="register"></a>アプリケーションをプッシュ通知に登録する

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

### <a name="profile"></a>アプリケーションのプロビジョニング プロファイルを作成する

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

7. Xcode で開いて、オーガナイザー [Devices ビューを選択 **Provisioning Profiles** で、 **ライブラリ** セクションの左側のウィンドウで、クリックして、 **更新** 中央のウィンドウの下部にあるボタンをクリックします。

### <a name="configure-mobileServices"></a>プッシュ要求を送信するように Mobile Services を構成する

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

### <a name="configure-app"></a>Xamarin.iOS アプリケーションを構成する

1. Xamarin.Studio または Visual Studio で開きます **Info.plist**, 、および更新、 **Bundle Identifier** 前に作成した ID を持つ。

    ![][121]

2. 下へスクロールして **Background Modes** を確認し、 **Enable Background Modes** ボックスおよび **リモート通知** ボックス。

    ![][122]

3. 開くソリューション パネルでプロジェクトをダブルクリックして **プロジェクト オプション**します。

4.  選択 **iOS Bundle Signing** [ **ビルド**, 、し、対応する選択 **Identity** と **プロビジョニング プロファイル** だけこのプロジェクトを設定する必要があります。

    ![][120]

    これで、Xamarin プロジェクトはコード署名のために新しいプロファイルを使用するようになります。 公式の Xamarin デバイス プロビジョニングのドキュメントについて、次を参照してください。 [Xamarin Device Provisioning]します。

### <a name="add-push"></a>アプリケーションにプッシュ通知を追加する

1. Xamarin.Studio または Visual Studio で、展開、 **ToDoAzure.iOS** プロジェクトを開き、 **AppDelegate** クラスし、し、置換、 **FinishedLaunching** イベントを次のコード。

        public override bool FinishedLaunching(UIApplication app, NSDictionary options)
        {
             // registers for push for iOS8
            var settings = UIUserNotificationSettings.GetSettingsForTypes(
                UIUserNotificationType.Alert
                | UIUserNotificationType.Badge
                | UIUserNotificationType.Sound,
                new NSSet());

            global::Xamarin.Forms.Forms.Init();
            instance = this;
            CurrentPlatform.Init();

            todoItemManager = new ToDoItemManager();
            App.SetTodoItemManager(todoItemManager);


            UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications();

            LoadApplication(new App());
            return base.FinishedLaunching(app, options);
        }

6.  **AppDelegate**, 、オーバーライド、 **RegisteredForRemoteNotifications** イベント。

        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            // Modify device token
            string _deviceToken = deviceToken.Description;
            _deviceToken = _deviceToken.Trim('<', '>').Replace(" ", "");

            // Get Mobile Services client
            MobileServiceClient client = todoItemManager.GetClient;

            // Register for push with Mobile Services
            IEnumerable<string> tag = new List<string>() { "uniqueTag" };

            const string template = "{\"aps\":{\"alert\":\"$(message)\"}}";

            var expiryDate = DateTime.Now.AddDays(90).ToString
                (System.Globalization.CultureInfo.CreateSpecificCulture("en-US"));

            var push = client.GetPush();

            push.RegisterTemplateAsync(_deviceToken, template, expiryDate, "myTemplate", tag)
        }

7.  **AppDelegate**, 、オーバーライド、 **ReceivedRemoteNotification** イベント。

        public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
        {
            NSObject inAppMessage;

            bool success = userInfo.TryGetValue(new NSString("inAppMessage"), out inAppMessage);

            if (success)
            {
                var alert = new UIAlertView("Got push notification", inAppMessage.ToString(), null, "OK", null);
                alert.Show();
            }
        }

これで、アプリケーションがプッシュ通知をサポートするように更新されました。

### <a name="update-scripts"></a>Azure クラシック ポータルで登録されている挿入スクリプトを更新する

1. Azure クラシック ポータルで、クリックして、 **データ** ] タブでをクリックし、 **TodoItem** テーブルです。

    ![][21]

2.  **Todoitem**, をクリックして、 **スクリプト** ] タブで選択し、 **挿入**します。

    ![][22]

    これで、挿入が発生したときに呼び出される関数が表示されます、 **TodoItem** テーブルです。

3. クリックして insert 関数を次のコードに置き換えます **保存**:

          function insert(item, user, request) {
          // Execute the request and send notifications.
             request.execute({
             success: function() {
              // Create a template-based payload.
              var payload = '{ "message" : "New item added: ' + item.text + '" }';

              // Write the default response and send a notification
              // to all platforms.
              push.send(null, payload, {
                  success: function(pushResponse){
                  console.log("Sent push:", pushResponse);
                  // Send the default response.
                  request.respond();
                  },
                  error: function (pushResponse) {
                      console.log("Error Sending push:", pushResponse);
                       // Send the an error response.
                      request.respond(500, { error: pushResponse });
                      }
               });
              }
           });
          }

    これで、新しい insert スクリプトが登録されます。このスクリプトは、挿入要求で指定されたデバイスにプッシュ通知 (挿入されたテキスト) を送信します。

   >[AZURE.NOTE] このスクリプトを提供する通知の送信を遅らせて時間、トースト通知を受信するアプリケーションを閉じます。

### <a name="test"></a>アプリケーションでプッシュ通知をテストする

1. キーを押して、 **実行** をクリックして、プロジェクトをビルドし、iOS 対応のデバイスでアプリケーションを開始 **OK** プッシュ通知を受け入れます

   >[AZURE.NOTE] アプリからプッシュ通知を明示的に同意する必要があります。 これが必要であるのは、初めてアプリケーションを実行するときだけです。

2. アプリケーションで、をクリックして、 **追加** ボタンをクリックし、タスクのタイトルを追加し、をクリックし、 **保存** ] ボタンをクリックします。

3. 確認通知を受信し、] をクリックして **OK** を通知を破棄します。


これで、このチュートリアルは終了です。

## <a name="Android"></a>Xamarin.Forms.Android アプリにプッシュ通知を追加します。

Google Cloud Messaging (GCM) サービスを使用して、Android アプリにプッシュ通知を追加します。 アクティブな Google アカウントを必要と [Google Cloud Messaging Client Component]します。

###<a id="register"></a>Google Cloud Messaging を有効にする

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

###<a id="configure"></a>プッシュ要求を送信するように Mobile Services を構成する

[AZURE.INCLUDE [mobile-services-android-configure-push](../../includes/mobile-services-android-configure-push.md)]

###<a id="update-scripts"></a>通知を送信するように登録済み挿入スクリプトを更新する

>[AZURE.NOTE] 次の手順では、Azure 旧ポータルで、TodoItem テーブルに対する挿入操作に登録されているスクリプトを更新する方法を説明します。 このモバイル サービス スクリプトは、Visual Studio のサーバー エクスプローラーの Azure ノードで、直接アクセスして編集することもできます。

 [Azure classic portal], をクリックして、 **データ** ] タブでをクリックし、 **TodoItem** テーブル。

   ![][21]

2.  **Todoitem**, をクリックして、 **スクリプト** ] タブで選択し、 **挿入**します。

   ![][22]

    これで、挿入が発生したときに呼び出される関数が表示されます、 **TodoItem** テーブルです。

3. クリックして insert 関数を次のコードに置き換えます **保存**:

          function insert(item, user, request) {
          // Execute the request and send notifications.
             request.execute({
             success: function() {
              // Create a template-based payload.
              var payload = '{ "message" : "New item added: ' + item.text + '" }';

              // Write the default response and send a notification
              // to all platforms.
              push.send(null, payload, {
                  success: function(pushResponse){
                  console.log("Sent push:", pushResponse);
                  // Send the default response.
                  request.respond();
                  },
                  error: function (pushResponse) {
                      console.log("Error Sending push:", pushResponse);
                       // Send the an error response.
                      request.respond(500, { error: pushResponse });
                      }
               });
              }
           });
          }


    これで、新しい insert スクリプトが登録されます。このスクリプトは、挿入要求で指定されたデバイスにプッシュ通知 (挿入されたテキスト) を送信します。

   >[AZURE.NOTE] このスクリプトを提供する通知の送信を遅らせて時間、トースト通知を受信するアプリケーションを閉じます。


###<a id="configure-app"></a>プッシュ通知の既存のプロジェクトを構成する

1. ソリューション ビューで、展開、 **コンポーネント** Xamarin.Android アプリケーションのフォルダーを Azure Mobile Services パッケージがインストールされているかどうかを確認します。

2. 右クリックし、 **コンポーネント** フォルダー] をクリックして  **コンポーネントの取得]**, 、検索、 **Google Cloud Messaging クライアント** コンポーネントをプロジェクトに追加します。

1. MainActivity.cs プロジェクト ファイルを開き、次の using ステートメントをクラスに追加します。

        using Gcm.Client;


4.   **MainActivity** クラスで、次のコードを追加、 **OnCreate** への呼び出し後のメソッド、 **LoadApplication** メソッド。

            try
            {
                // Check to ensure everything's setup right
                GcmClient.CheckDevice(this);
                GcmClient.CheckManifest(this);

                // Register for push notifications
                System.Diagnostics.Debug.WriteLine("Registering...");
                GcmClient.Register(this, PushHandlerBroadcastReceiver.SENDER_IDS);
            }
            catch (Java.Net.MalformedURLException)
            {
                CreateAndShowDialog(new Exception("There was an error creating the Mobile Service. Verify the URL"), "Error");
            }
            catch (Exception e)
            {
                CreateAndShowDialog(e, "Error");
            }

 **MainActivity** がプッシュ通知を追加するための準備が整いました。

###<a id="add-push"></a>アプリケーションにプッシュ通知コードを追加する

5. ToDoAzure.Droid プロジェクト内で、`GcmService` という名前の新しいクラスを作成します。

5. 以下を追加するステートメントを使用して **GcmService** クラス。

        using Gcm.Client;
        using Microsoft.WindowsAzure.MobileServices;

6. 間に次のアクセス許可要求を追加、 **を使用して** ステートメントおよび **名前空間** 宣言します。

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]

        //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]

7.  **GcmService.cs** プロジェクト ファイルでは、次のクラスを追加します。

        [BroadcastReceiver(Permission = Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE }, Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK }, Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY }, Categories = new string[] { "@PACKAGE_NAME@" })]

        public class PushHandlerBroadcastReceiver : GcmBroadcastReceiverBase<GcmService>
        {

            public static string[] SENDER_IDS = new string[] { "<PROJECT_NUMBER>" };

        }

    上記のコードで置き換える必要があります _`<PROJECT_NUMBER>`_ Google の開発者ポータルでアプリケーションをプロビジョニングしたときに Google によって割り当てられたプロジェクト番号です。

8. GcmService.cs プロジェクト ファイルで定義する次のコードを追加、 **GcmService** クラス。

         [Service]
         public class GcmService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }

             public GcmService()
                 : base(PushHandlerBroadcastReceiver.SENDER_IDS){}
         }


    このクラスから派生注 **GcmServiceBase** と、 **サービス** 属性は、このクラスに適用する必要があります。

    >[AZURE.NOTE] **GcmServiceBase** クラスが実装する、 **OnRegistered()**, 、**OnUnRegistered()**, 、**OnMessage()** と **OnError()** メソッドです。 これらのメソッドをオーバーライドする必要があります、 **GcmService** クラスです。

5. 次のコードを追加、 **GcmService** をオーバーライドするクラス、 **OnRegistered** イベント ハンドラーです。

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose(PushHandlerBroadcastReceiver.TAG, "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            createNotification("GcmService Registered...", "The device has been Registered, Tap to View!");

            MobileServiceClient client =  MainActivity.DefaultService.todoItemManager.GetClient;

            var push = client.GetPush();

            MainActivity.DefaultService.RunOnUiThread(() => Register(push, null));

        }
        public async void Register(Microsoft.WindowsAzure.MobileServices.Push push, IEnumerable<string> tags)
        {
            try
            {
                const string template = "{\"data\":{\"message\":\"$(message)\"}}";

                await push.RegisterTemplateAsync(RegistrationID, template, "mytemplate", tags);
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(ex.Message);
                Debugger.Break();
            }
        }

    このメソッドでは、返された GCM 登録 ID を使用して、プッシュ通知のために Azure に登録します。

10. オーバーライド、 **OnMessage** メソッド **GcmService** を次のコード。

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info(PushHandlerBroadcastReceiver.TAG, "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            //Store the message
            var prefs = GetSharedPreferences(context.PackageName, FileCreationMode.Private);
            var edit = prefs.Edit();
            edit.PutString("last_msg", msg.ToString());
            edit.Commit();

            string message = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty(message))
            {
                createNotification("New todo item!", "Todo item: " + message);
                return;
            }

            string msg2 = intent.Extras.GetString("msg");
            if (!string.IsNullOrEmpty(msg2))
            {
                createNotification("New hub message!", msg2);
                return;
            }

            createNotification("Unknown message details", msg.ToString());
        }

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show ui
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Create the notification
            var notification = new Notification(Android.Resource.Drawable.SymActionEmail, title);

            //Auto cancel will remove the notification once the user touches it
            notification.Flags = NotificationFlags.AutoCancel;

            //Set the notification info
            //we use the pending intent, passing our ui intent over which will get called
            //when the notification is tapped.
            notification.SetLatestEventInfo(this, title, desc, PendingIntent.GetActivity(this, 0, uiIntent, 0));

            //Show the notification
            notificationManager.Notify(1, notification);
        }

12. 用の次のメソッド オーバーライドを追加 **OnUnRegistered()** と **OnError()**, 、これらはプロジェクトをコンパイルするために必要です。

        protected override void OnError(Context context, string errorId)
        {
              Log.Error(PushHandlerBroadcastReceiver.TAG, "GCM Error: " + errorId);
        }

###<a id="test"></a>アプリケーションでプッシュ通知をテストする

Android フォンを USB ケーブルで直接接続するか、エミュレーターで仮想デバイスを使用する方法により、アプリケーションをテストできます。

このアプリケーションをエミュレーターで実行する場合は、Google API をサポートしている Android Virtual Device (AVD) を使用してください。

> [AZURE.IMPORTANT] Android Virtual Device でプッシュ通知を受信するために Google アカウントを設定する必要があります (エミュレーターに移動 **設定** ] をクリック **アカウントの追加**)。 さらに、エミュレーターがインターネットに接続されていることを確認します。

1.  **ツール**, 、クリックして **Android エミュレーター マネージャーを開く**, をデバイスを選択してクリックして **編集**します。

    ![][125]

2. 選択 **Google APIs** で **ターゲット**, 、] をクリックし、 **[ok]**します。

    ![][126]

3. 上部のツールバーをクリックして **実行**, 、アプリケーションを選択します。 これによりエミュレーターが起動し、アプリケーションが実行されます。

  アプリの取得、 *registrationId* GCM および通知ハブに登録します。

1. アプリケーションで、新しいタスクを追加します。

2. 画面の上端から下へスワイプし、通知を表示するためのデバイスの通知センターを開きます。

    ![][127]

## <a name="Windows"></a>Xamarin.Forms.Windows アプリにプッシュ通知を追加する

このチュートリアルでは、Azure Mobile Services を使用して、Xamarin.Forms ソリューションに含まれる Windows Phone Silverlight アプリにプッシュ通知を送信する方法を紹介します。

###<a id="update-app"></a>アプリケーションを更新して通知に登録する

アプリケーションがプッシュ通知を受信するには、通知チャネルを登録する必要があります。

1. Visual Studio で App.xaml.cs ファイルを開き、次の `using` ステートメントを追加します。

        using Microsoft.Phone.Notification;

3. 次のコードを App.xaml.cs に追加します。

        public static HttpNotificationChannel CurrentChannel { get; private set; }

        private void AcquirePushChannel()
        {
            CurrentChannel = HttpNotificationChannel.Find("MyPushChannel");

            if (CurrentChannel == null)
            {
                CurrentChannel = new HttpNotificationChannel("MyPushChannel");
                CurrentChannel.Open();
                CurrentChannel.BindToShellToast();
            }

            CurrentChannel.ChannelUriUpdated +=
                new EventHandler<NotificationChannelUriEventArgs>(async (o, args) =>
                {

                   // Register for notifications using the new channel
                    const string template =
                    "<?xml version=\"1.0\" encoding=\"utf-8\"?><wp:Notification " +
                    "xmlns:wp=\"WPNotification\"><wp:Toast><wp:Text1>$(message)</wp:Text1></wp:Toast></wp:Notification>";

                    await client.GetPush()
                        .RegisterTemplateAsync(CurrentChannel.ChannelUri.ToString(), template, "mytemplate");
                });
        }

    このコードは Windows Phone 8.x "Silverlight" で使用される Microsoft Push Notification Service (MPNS) からアプリの ChannelURI を取得し、それをプッシュ通知用に登録します。

    >[AZURE.NOTE]これでこのチュートリアルでは、モバイル サービス トースト通知を送信、デバイスにします。 タイル通知を送信するときに呼び出す必要があります、 **BindToShellTile** 、チャネル上のメソッドです。

4. 上部にある、 **Application_Launching** App.xaml.cs のイベント ハンドラーが、新規作成] を次の呼び出しを追加 **AcquirePushChannel** メソッド。

        AcquirePushChannel();

    これにより、ページが読み込まれるたびに登録が要求されるようになります。 アプリケーションでは、この登録が常に最新の状態となるように、定期的な登録のみ行うことができます。

5. キーを押して、 **f5 キーを押して** キーをアプリケーションを実行します。 登録キーを示すポップアップ ダイアログが表示されます。

6.  ソリューション エクスプ ローラーで、 **プロパティ**, 、WMAppManifest.xml ファイルを開き、をクリックして、 **機能** ] タブを確認して、 **_ _ _push_notification** 機能がオンです。

    ![VS で通知を有効にする](./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-app-enable-push-wp8.png)

    これにより、アプリケーションでトースト通知の使用が有効になります。

###<a id="update-scripts"></a>サーバー スクリプトを更新してプッシュ通知を送信する

最後に、通知を送信するためには、TodoItem テーブルの挿入操作に登録されているスクリプトを更新する必要があります。

1.  [Azure classic portal], をクリックして、 **データ** ] タブでをクリックし、 **TodoItem** テーブル。

    ![][21]

2.  **Todoitem**, をクリックして、 **スクリプト** ] タブで選択し、 **挿入**します。

    ![][22]

    これで、挿入が発生したときに呼び出される関数が表示されます、 **TodoItem** テーブルです。

3. クリックして insert 関数を次のコードに置き換えます **保存**:

          function insert(item, user, request) {
          // Execute the request and send notifications.
             request.execute({
             success: function() {
              // Create a template-based payload.
              var payload = '{ "message" : "New item added: ' + item.text + '" }';

              // Write the default response and send a notification
              // to all platforms.
              push.send(null, payload, {
                  success: function(pushResponse){
                  console.log("Sent push:", pushResponse);
                  // Send the default response.
                  request.respond();
                  },
                  error: function (pushResponse) {
                      console.log("Error Sending push:", pushResponse);
                       // Send the an error response.
                      request.respond(500, { error: pushResponse });
                      }
               });
              }
           });
          }


    これで、新しい insert スクリプトが登録されます。このスクリプトは、挿入要求で指定されたデバイスにプッシュ通知 (挿入されたテキスト) を送信します。

3. をクリックして、 **プッシュ** ] タブで、チェック **非認証プッシュ通知を有効にする**, 、] をクリックし、 **保存**します。

    これにより、非認証モードで MPNS に接続するモバイル サービスがプッシュ通知を送信できるようになります。

    >[AZURE.NOTE]このチュートリアルでは、非認証モードで MPNS を使用します。 このモードでは、MPNS はデバイス チャネルに送信できる通知の数を制限します。 この制限を削除するには、**[アップロード]** をクリックして証明書を生成してアップロードし、その証明書を選択する必要があります。 証明書を生成する方法の詳細については、「Setting up an authenticated web service to send push notifications for Windows Phone (認証済み Web サービスを、Windows Phone のプッシュ通知を送信するように設定する)」を参照してください。

###<a id="test"></a>アプリケーションでプッシュ通知をテストする

1. Visual Studio で、F5 キーを押してアプリケーションを実行します。

    >[AZURE.NOTE] Windows Phone エミュレーターでテストする場合は、401 許可されていないエラー「registrationauthorizationexception」が発生する可能性があります。 これは、Windows Phone エミュレーターのホスト PC の時計との同期方法によって `RegisterNativeAsync()` 呼び出し中に発生します。 セキュリティ トークンで拒否される場合があります。 これを解決するには、テストする前にエミュレーターの時計を手動で設定します。

5. アプリケーションで、新しいタスクの作成というタイトルの **こんにちは push**, 、すぐに開始] ボタンまたは [戻る] ボタンをクリックしてアプリケーションを閉じます。

    これにより、追加された項目を保存するための挿入要求がモバイル サービスに送信されます。 デバイスがというトースト通知を受信 **こんにちは push**します。

    ![受信したトースト通知](./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-quickstart-push5-wp8.png)

    >[AZURE.NOTE]アプリケーションでまだしたら、通知を受信しませんされます。 アプリがアクティブな間は、トースト通知を受信するには、処理、 [ShellToastNotificationReceived](http://msdn.microsoft.com/library/windowsphone/develop/microsoft.phone.notification.httpnotificationchannel.shelltoastnotificationreceived(v=vs.105).aspx) イベントです。

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

[5]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-services-ios-push-step5.png
[6]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-services-ios-push-step6.png
[7]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-services-ios-push-step7.png

[9]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-services-ios-push-step9.png
[10]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-services-ios-push-step10.png

[17]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-services-ios-push-step17.png
[18]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-services-selection.png
[19]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-push-tab-ios.png
[20]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-push-tab-ios-upload.png
[21]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-portal-data-tables.png
[22]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-insert-script-push2.png
[23]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-quickstart-push1-ios.png
[24]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-quickstart-push2-ios.png
[25]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-quickstart-push3-ios.png
[26]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-quickstart-push4-ios.png
[28]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-services-ios-push-step18.png

[101]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-services-ios-push-01.png
[102]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-services-ios-push-02.png
[103]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-services-ios-push-03.png
[104]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-services-ios-push-04.png
[105]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-services-ios-push-05.png
[106]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-services-ios-push-06.png
[107]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-services-ios-push-07.png
[108]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-services-ios-push-08.png

[110]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-services-ios-push-10.png
[111]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-services-ios-push-11.png
[112]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-services-ios-push-12.png
[113]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-services-ios-push-13.png
[114]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-services-ios-push-14.png
[115]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-services-ios-push-15.png
[116]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-services-ios-push-16.png
[117]: ./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-services-ios-push-17.png

[120]:./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-services-ios-push-20.png
[121]:./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-services-ios-push-21.png
[122]:./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-services-ios-push-22.png
[123]:./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-data-tab-empty.png
[124]:./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/mobile-create-todoitem-table.png
[125]:./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/notification-hub-create-android-app7.png
[126]:./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/notification-hub-create-android-app8.png
[127]:./media/partner-xamarin-mobile-services-xamarin-forms-get-started-push/notification-area-received.png


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
[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/
[Xamarin.Forms Azure Push Notification Starter Sample]: https://github.com/Azure/mobile-services-samples/tree/master/TodoListXamarinForms
[Completed Xamarin.Forms Azure Push Notification Sample]: https://github.com/Azure/mobile-services-samples/tree/master/GettingStartedWithPushXamarinForms


