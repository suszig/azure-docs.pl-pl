<properties
    pageTitle="Notification Hubs の使用 (Xamarin.Android アプリケーション) | Microsoft Azure"
    description="このチュートリアルでは、Azure Notification Hubs を使用して Xamarin.Android アプリケーションにプッシュ通知を送信する方法について学習します。"
    authors="wesmc7777"
    manager="dwrede"
    editor=""
    services="notification-hubs"
    documentationCenter="xamarin"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="11/17/2015"
    ms.author="wesmc"/>

# Xamarin for Android での Notification Hubs の使用

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##概要

このチュートリアルでは、Azure Notification Hubs を使用して Xamarin.Android アプリケーションにプッシュ通知を送信する方法について説明します。
Google Cloud Messaging (GCM) を使用してプッシュ通知を受信する空の Xamarin.Android アプリケーションを作成します。 完了すると、通知ハブを使用して、アプリケーションを実行するすべてのデバイスにプッシュ通知をブロードキャストできるようになります。 完成したコードが含まれている、 [NotificationHubs アプリケーション][GitHub] サンプルです。

このチュートリアルでは、Notification Hubs を使用した簡単なブロードキャスト シナリオのデモンストレーションを行います。


## 開始する前に

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

このチュートリアルの完全なコードは GitHub でご覧 [ここ](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid)します。



##前提条件

このチュートリアルには、次のものが必要です。

+ [Xamarin.Android]
+ アクティブな Google アカウント
+ [Azure モバイル サービス コンポーネント]
+ [Azure のメッセージング コンポーネント]
+ [Google Cloud Messaging のクライアント コンポーネント]

このチュートリアルを完了することは、Xamarin.Android アプリの他のすべての Notification Hubs チュートリアルの前提条件です。

> [AZURE.IMPORTANT] このチュートリアルを完了するには、アクティブな Azure アカウントが必要です。 アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、[Azure の無料試用版サイト](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F)をご覧ください。

##Google Cloud Messaging を有効にする

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

##通知ハブを構成する

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


<ol start="7">
<li><p>をクリックして、 <b>構成</b> 上部にあるタブで、入力、 <b>API キー</b> クリックして、前のセクションで取得した値 <b>保存</b>します。</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hub-configure-android.png)


これで、通知ハブが GCM と連動するように構成されました。接続文字列により、アプリが通知を受信すると共に、プッシュ通知を送信するように登録されます。

##通知ハブにアプリケーションを接続する

###新しいプロジェクトを作成する

1. Xamarin Studio で、[ **新しいソリューション**, 、] をクリックして **Android アプリ**, 、] をクリック **次**します。

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project1.png)

2. 入力、 **アプリ名** と **識別子**します。 をクリックして、 **ターゲット プラットフォームの場合は** をサポートし、をクリックする **次** と **作成**します。

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project2.png)


    This creates a new Android project.

2. [Solution] ビューで新しいプロジェクトを右クリックしてプロジェクトのプロパティを開きます **オプション**します。 選択、 **Android アプリケーション** 内の項目、 **ビルド** セクションです。

    最初の文字、 **パッケージ名** は小文字です。

    > [AZURE.IMPORTANT] パッケージ名の最初の文字を小文字にする必要があります。 それ以外の場合、登録するときにアプリケーション マニフェスト エラーが発生、 **BroadcastReceiver** と **IntentFilter** の次の通知をプッシュします。

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub--xamarin-android-app-options.png)


3. 必要に応じて、設定、 **最小 Android バージョン** API レベルの間です。

4. 必要に応じて、設定、 **ターゲット Android バージョン** を対象とする別の API バージョンに (あります API レベル 8 以上)。

クリックして **OK** プロジェクトのオプション ダイアログ ボックスを閉じます。


###プロジェクトに必要なコンポーネントを追加します。

Xamarin コンポーネント ストアから入手できる Google Cloud Messaging クライアントにより、Xamarin.Android でプッシュ通知をサポートするプロセスが効率化されます。

1. Xamarin.Android アプリケーションの [コンポーネント] フォルダーを右クリックし [ **Get More Components**します。

2. 検索、 **Azure メッセージング** コンポーネントをプロジェクトに追加します。

3. 検索、 **Google Cloud Messaging Client** コンポーネントをプロジェクトに追加します。


###プロジェクトで Notification Hubs を設定する

1. Android アプリケーションと通知ハブについて次の情報を収集します。

    - **GoogleProjectNumber**: Google の開発者ポータルでアプリの概要からこのプロジェクト番号の値を取得します。 この値は、以前ポータルでアプリを作成したときにメモしています。
    - **接続文字列をリッスン**: ダッシュ ボードに、 [Azure Classic Portal], 、クリックして **接続文字列を表示**します。 コピー、 *DefaultListenSharedAccessSignature* この値として接続文字列。
    - **ハブ名**: これはから得られるハブの名前、 [Azure Classic Portal]します。 たとえば、 *mynotificationhub2*します。

    作成、 **Constants.cs** Xamarin プロジェクトのクラスし、クラスに次の定数値を定義します。 プレースホルダーを実際の値に置き換えます。

        public static class Constants
        {
            public const string SenderID = "<GoogleProjectNumber>"; // Google API Project Number
            public const string ListenConnectionString = "<Listen connection string>";
            public const string NotificationHubName = "<hub name>";
        }

2. 以下を追加するステートメントを使用して **MainActivity.cs**:

        using Android.Util;
        using Gcm.Client;

3. アプリを実行しているときにアラート ダイアログを表示するために使用する `MainActivity` クラスにインスタンス変数を追加します。

        public static MainActivity instance;


3. 次のメソッドを作成、 **MainActivity** クラス。

        private void RegisterWithGCM()
        {
            // Check to ensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            Log.Info("MainActivity", "Registering...");
            GcmClient.Register(this, Constants.SenderID);
        }

4.  `OnCreate` メソッドの **MainActivity.cs**, 、初期化、 `instance` 変数への呼び出しを追加および `RegisterWithGCM`:

        protected override void OnCreate (Bundle bundle)
        {
            instance = this;

            base.OnCreate (bundle);

            // Set your view from the "main" layout resource
            SetContentView (Resource.Layout.Main);

            // Get your button from the layout resource,
            // and attach an event to it
            Button button = FindViewById<Button> (Resource.Id.myButton);

            RegisterWithGCM();
        }


4. 新しいクラスを作成 **MyBroadcastReceiver**します。

    > [AZURE.NOTE] 作成する手順を説明、 **BroadcastReceiver** 次からのクラスです。 ただし、手動で作成する代わりの **MyBroadcastReceiver.cs** を参照してください、 **GcmService.cs** 付属の Xamarin.Android サンプル プロジェクトで検出されたファイル、 [NotificationHubs のサンプル][GitHub]します。 複製 **GcmService.cs** も開始する最適なクラス名を変更するとします。

5. 以下を追加するステートメントを使用して **MyBroadcastReceiver.cs** (コンポーネントと前に追加したアセンブリを参照する)。

        using System.Collections.Generic;
        using System.Text;
        using Android.App;
        using Android.Content;
        using Android.Util;
        using Gcm.Client;
        using WindowsAzure.Messaging;

5.  **MyBroadcastReceiver.cs**, 、間に次のアクセス許可要求を追加、 **を使用して** ステートメントおよび **名前空間** 宣言します。

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]

        //GET_ACCOUNTS is needed only for Android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]

6.  **MyBroadcastReceiver.cs**, 、変更、 **MyBroadcastReceiver** を次のようにクラス。

        [BroadcastReceiver(Permission=Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        public class MyBroadcastReceiver : GcmBroadcastReceiverBase<PushHandlerService>
        {
            public static string[] SENDER_IDS = new string[] { Constants.SenderID };

            public const string TAG = "MyBroadcastReceiver-GCM";
        }

7. 別のクラスを追加 **MyBroadcastReceiver.cs** という **PushHandlerService**, から派生した **GcmServiceBase**します。 適用することを確認、 **サービス** 属性をクラス。

        [Service] // Must use the service tag
        public class PushHandlerService : GcmServiceBase
        {
            public static string RegistrationID { get; private set; }
            private NotificationHub Hub { get; set; }

            public PushHandlerService() : base(Constants.SenderID)
            {
                Log.Info(MyBroadcastReceiver.TAG, "PushHandlerService() constructor");
            }
        }


8. **GcmServiceBase** メソッドを実装する **OnRegistered()**, 、**OnUnRegistered()**, 、**OnMessage()**, 、**OnRecoverableError()**, 、および **OnError()**します。 当社 **PushHandlerService** 実装クラスは、これらのメソッドをオーバーライドする必要があり、これらのメソッドは、通知ハブとのやり取りに応答して起動します。


9. オーバーライド、 **OnRegistered()** メソッド **PushHandlerService** 次のコードを使用しています。

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            createNotification("PushHandlerService-GCM Registered...",
                                "The device has been Registered!");

            Hub = new NotificationHub(Constants.NotificationHubName, Constants.ListenConnectionString,
                                        context);
            try
            {
                Hub.UnregisterAll(registrationId);
            }
            catch (Exception ex)
            {
                Log.Error(MyBroadcastReceiver.TAG, ex.Message);
            }

            //var tags = new List<string>() { "falcons" }; // create tags if you want
            var tags = new List<string>() {};

            try
            {
                var hubRegistration = Hub.Register(registrationId, tags.ToArray());
            }
            catch (Exception ex)
            {
                Log.Error(MyBroadcastReceiver.TAG, ex.Message);
            }
        }

    > [AZURE.NOTE]  **OnRegistered()** 上記のコードで特定のメッセージング チャネルを登録するタグを指定できる点に注意してください。

10. オーバーライド、 **OnMessage** メソッド **PushHandlerService** 次のコードを使用しています。

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info(MyBroadcastReceiver.TAG, "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            string messageText = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty (messageText))
            {
                createNotification ("New hub message!", messageText);
            }
            else
            {
                createNotification ("Unknown message details", msg.ToString ());
            }
        }

11. 次の追加 **createNotification** と **dialogNotify** メソッド **PushHandlerService** 通知が受信したときにユーザーに通知します。

    >[AZURE.NOTE] Android バージョン 5.0 以降の通知デザインは、以前のバージョンの大幅な出発を表します。 Android 5.0 以降でこのテストを行う場合は、通知を受信するためにアプリを実行する必要があります。 詳細については、次を参照してください。 [Android の通知](http://go.microsoft.com/fwlink/?LinkId=615880)します。

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show UI
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Create the notification
            var notification = new Notification(Android.Resource.Drawable.SymActionEmail, title);

            //Auto-cancel will remove the notification once the user touches it
            notification.Flags = NotificationFlags.AutoCancel;

            //Set the notification info
            //we use the pending intent, passing our ui intent over, which will get called
            //when the notification is tapped.
            notification.SetLatestEventInfo(this, title, desc, PendingIntent.GetActivity(this, 0, uiIntent, 0));

            //Show the notification
            notificationManager.Notify(1, notification);
            dialogNotify (title, desc);
        }

        protected void dialogNotify(String title, String message)
        {

            MainActivity.instance.RunOnUiThread(() => {
                AlertDialog.Builder dlg = new AlertDialog.Builder(MainActivity.instance);
                AlertDialog alert = dlg.Create();
                alert.SetTitle(title);
                alert.SetButton("Ok", delegate {
                    alert.Dismiss();
                });
                alert.SetMessage(message);
                alert.Show();
            });
        }


12. 抽象メンバーをオーバーライドする **OnUnRegistered()**, 、**OnRecoverableError()**, 、および **OnError()** 、コードがコンパイルされるようにします。

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Unregistered: " + registrationId);

            createNotification("GCM Unregistered...", "The device has been unregistered!");
        }

        protected override bool OnRecoverableError(Context context, string errorId)
        {
            Log.Warn(MyBroadcastReceiver.TAG, "Recoverable Error: " + errorId);

            return base.OnRecoverableError (context, errorId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error(MyBroadcastReceiver.TAG, "GCM Error: " + errorId);
        }


##エミュレーターを使用してアプリケーションを実行する

このアプリケーションをエミュレーターで実行する場合は、Google API をサポートしている Android Virtual Device (AVD) を使用してください。

> [AZURE.IMPORTANT] プッシュ通知を受信するのには、Android Virtual Device で Google アカウントを設定する必要があります。 (エミュレーターに移動 **設定** ] をクリック **アカウントの追加**.)さらに、エミュレーターがインターネットに接続されていることを確認します。

>[AZURE.NOTE] Android バージョン 5.0 以降の通知デザインは、以前のバージョンの大幅な出発を表します。 詳細については、次を参照してください。 [Android の通知](http://go.microsoft.com/fwlink/?LinkId=615880)します。


1.  **ツール**, 、クリックして **Android エミュレーター マネージャーを開く**, をデバイスを選択してクリックして **編集**します。

    ![][18]

2. 選択 **Google Api** で **ターゲット**, 、順にクリック **OK**します。

    ![][19]

3. 上部のツールバーをクリックして **実行**, 、アプリケーションを選択します。 これによりエミュレーターが起動し、アプリケーションが実行されます。

  アプリの取得、 *registrationId* GCM および通知ハブに登録します。

##バックエンドから通知を送信する


通知を送信することによって、アプリで通知の受信をテストする、 [Azure Classic Portal] 通知ハブで、次の画面に示すように、[デバッグ] タブを使用しています。

![][30]


プッシュ通知は通常、互換性のあるライブラリを使用して Mobile Services などのバックエンド サービスや ASP.NET に送信されます。 ライブラリがバックエンドで利用できない場合、REST API を直接使用して通知メッセージを送信できます。

通知の送信方法を確認できるチュートリアルの一覧を次に示します。

- ASP.NET: を参照してください [Use Notification Hubs to push notifications to users]します。
- Azure Notification Hub Java SDK: を参照してください [Java から Notification Hubs の使用方法](notification-hubs-java-backend-how-to.md) Java からの通知を送信するためです。 これは Android の開発用に Eclipse でテストされています。
- PHP: を参照してください [PHP から Notification Hubs の使用方法](notification-hubs-php-backend-how-to.md)します。


チュートリアルの以下のサブセクションでは、.NET コンソール アプリと、ノード スクリプトを使用するモバイル サービスで通知を送信します。

####(省略可能).NET アプリケーションを使用して通知を送信する

このセクションでは、.NET コンソール アプリケーションを使用して通知を送信します。

1. Visual C# の新しいコンソール アプリケーションを作成します。

    ![][20]

2. Visual Studio で、[ **ツール**, 、クリックして **NuGet パッケージ マネージャー**, 、順にクリック **パッケージ マネージャー コンソール**します。

    Visual Studio のパッケージ マネージャー コンソールが表示されます。

3. パッケージ マネージャー コンソール] ウィンドウで、設定、 **既定のプロジェクト** 、新しいコンソール アプリケーション プロジェクトにし、コンソール ウィンドウには、次のコマンドを実行します。

        Install-Package Microsoft.Azure.NotificationHubs

    使用して、Azure Notification Hubs SDK への参照を追加します <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification ハブ NuGet パッケージ</a>.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

4. Program.cs ファイルを開き、次の `using` ステートメントを追加します。

        using Microsoft.Azure.NotificationHubs;

5. `Program` クラスで、次のメソッドを追加します。 テキストとプレース ホルダー テキストを更新、 *DefaultFullSharedAccessSignature* から接続文字列とハブ名、 [Azure Classic Portal]します。

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            await hub.SendGcmNativeNotificationAsync("{ \"data\" : {\"message\":\"Hello from Azure!\"}}");
        }

6. 次の行を追加、 **Main** メソッド。

         SendNotificationAsync();
         Console.ReadLine();

7. F5 キーを押してアプリケーションを実行します。 アプリで通知を受信します。

    ![][21]

####(省略可能) モバイル サービスを使用して通知を送信する

1. 次の [モバイル サービスの開始を取得] です。

1. サインイン、 [Azure Classic Portal], 、目的のモバイル サービスを選択します。

2. 選択、 **スケジューラ** 上部にあるタブをクリックします。

    ![][22]

3. 新しいスケジュール済みジョブを作成、名前を挿入し、 **オンデマンド**します。

    ![][23]

4. ジョブが作成されたら、ジョブ名をクリックします。 をクリックし、 **スクリプト** 上部バー タブをクリックします。

5. スケジューラ関数内に次のスクリプトを挿入します。 通知ハブの名前と接続文字列にプレース ホルダーを置換することを確認 *DefaultFullSharedAccessSignature* 既に取得してあります。 クリックして **保存**します。

        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string>');
        notificationHubService.gcm.send(null,'{"data":{"message" : "Hello from Mobile Services!"}}',
          function (error)
          {
            if (!error) {
               console.warn("Notification successful");
            }
            else
            {
              console.warn("Notification failed" + error);
            }
          }
        );

6. クリックして **一度だけ実行** 下部のバーにします。 トースト通知を受信します。

##次のステップ

この簡単な例では、すべての Android デバイスに通知をブロードキャストします。 特定のユーザーをターゲットとするには、このチュートリアルを参照してください [Use Notification Hubs to push notifications to users]します。 対象グループごとにユーザーをセグメント化するかどうか、確認できる [Use Notification Hubs to send breaking news]します。 通知ハブを使用する方法の詳細について [Notification Hubs Guidance] し、[、 [Notification Hubs How-To for Android]します。

<!-- Anchors. -->
[Enable Google Cloud Messaging]: #register
[Configure your Notification Hub]: #configure-hub
[Connecting your app to the Notification Hub]: #connecting-app
[Run your app with the emulator]: #run-app
[Send notifications from your back-end]: #send
[Next steps]:#next-steps

<!-- Images. -->

[11]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-configure-android.png

[13]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app1.png
[15]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app3.png

[18]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app7.png
[19]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app8.png

[20]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-android-toast.png
[22]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler2.png

[30]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-android/#create-new-service
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[Azure Classic Portal]: https://manage.windowsazure.com/
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Android]: http://msdn.microsoft.com/library/dn282661.aspx

[Use Notification Hubs to push notifications to users]: /manage/services/notification-hubs/notify-users-aspnet
[Use Notification Hubs to send breaking news]: /manage/services/notification-hubs/breaking-news-dotnet
[GCMClient Component page]: http://components.xamarin.com/view/GCMClient
[Xamarin.NotificationHub GitHub page]: https://github.com/SaschaDittmann/Xamarin.NotificationHub
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Xamarin.Android]: http://xamarin.com/download/
[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/
[Azure Messaging Component]: http://components.xamarin.com/view/azure-messaging

