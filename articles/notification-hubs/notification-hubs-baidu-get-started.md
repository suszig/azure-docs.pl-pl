<properties
    pageTitle="Baidu での Azure Notification Hubs の使用 | Microsoft Azure"
    description="このチュートリアルでは、Baidu で Azure Notification Hubs を使用して、Android デバイスにプッシュ通知を送信する方法について学習します。"
    services="notification-hubs"
    documentationCenter="android"
    authors="wesmc7777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="mobile-baidu"
    ms.workload="mobile"
    ms.date="11/25/2015"
    ms.author="wesmc"/>

# Baidu での Notification Hubs の使用

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##概要

Baidu クラウド プッシュは、プッシュ通知をモバイル デバイスに送信するために使用することのできる中国のクラウド サービスです。 このサービスは、さまざまなアプリケーション ストアやプッシュ サービスの存在に加え、GCM (Google Cloud Messaging) に接続されていることが少ない Android デバイスの可用性にも差があるために Android へのプッシュ通知の送信方法が複雑な中国では特に便利です。

##前提条件

このチュートリアルには、次のものが必要です。

+ Android SDK (とみなし Eclipse を使用) からダウンロードできますが、 <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android サイト</a>
+ [モバイル サービス Android SDK]
+ [Baidu Push Android SDK]

>[AZURE.NOTE] このチュートリアルを完了するには、アクティブな Azure アカウントが必要です。 アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、[Azure の無料試用版サイト](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-baidu-get-started%2F)をご覧ください。


##Baidu アカウントを作成する

Baidu を使用するには、Baidu アカウントが必要です。 既にいずれかがある場合は、[Baidu ポータル] にログインし、次の手順に進みます。 アカウントを持っていない場合は、Baidu アカウントの作成方法に関する次の手順を参照してください。  

1. [Baidu ポータル] に移動し、をクリックして、 **[登录** (**ログイン**) リンクします。 クリックして **立即注册** アカウント登録プロセスを開始します。

    ![][1]

2. 必要な情報を入力してください: 電話番号/電子メール アドレス、パスワード、および検証コード-] をクリック **サインアップ**します。

    ![][2]

3. 入力した電子メール アドレスに、Baidu アカウントをアクティブ化するリンクが記載された電子メールが届きます。

    ![][3]

4. 電子メール アカウントにログインし、Baidu のアクティブ化メールを開き、リンクをクリックして Baidu アカウントをアクティブ化します。

    ![][4]

アクティブな Baidu アカウントを作成したら、[Baidu ポータル] にログインします。

##Baidu 開発者として登録する

1. [Baidu ポータル] にログインしたら、クリックして **更多 >>** (**詳細**)。

    ![][5]

2. スクロール ダウンして、 **站长与开发者服务 (web 管理者および開発者向けサービス)** セクションを **百度开放云平台** (**Baidu クラウド プラットフォームの開く**)。

    ![][6]

3. 次のページをクリックして **发者服务** (**開発者向けサービス**) 右上隅にあります。

    ![][7]

4. 次のページをクリックして **注册开发者** (**登録されている開発者**) 右上隅にあるメニューからです。

    ![][8]

5. 検証のテキスト メッセージを受信するため、名前、説明、および携帯電話番号を入力し、クリックして **送验证码** (**確認コードを送信**)。 国際電話番号の場合は、国コードをかっこで囲む必要があります。 たとえば、アメリカ合衆国の番号にはなります **(1) 1234567890**します。

    ![][9]

6. その後、次の例に示すような、認証番号を含むテキスト メッセージが届きます。

    ![][10]

7. 内のメッセージから確認番号を入力して **[验证码** (**確認コード**)。

8. 最後に、Baidu 契約書に同意し、クリックして、開発者の登録を完了 **[提交** (**送信**)。 登録が正常に終了すると、次のページが表示されます。

    ![][11]

##Baidu クラウド プッシュ プロジェクトを作成する

Baidu クラウド プッシュ プロジェクトを作成すると、アプリケーション ID、API キー、およびシークレット キーが届きます。

1. [Baidu ポータル] にログインしたら、クリックして **更多 >>** (**詳細**)。

    ![][5]

2. スクロール ダウンして、 **站长与开发者服务** (**web 管理者および開発者向けサービス**) セクションを **百度开放云平台** (**Baidu クラウド プラットフォームを開く**)。

    ![][6]

3. 次のページをクリックして **发者服务** (**開発者向けサービス**) 右上隅にあります。

    ![][7]

4. 次のページをクリックして **[云推送** (**クラウド プッシュ**) から、 **务** (**クラウド サービス**) セクションです。

    ![][12]

5. 登録済み開発者であればと表示されます **管理控制台** (**管理コンソール**)、上部メニュー。 クリックして **开发者服务管理** (**開発者サービスの管理**)。

    ![][13]

6. 次のページをクリックして **[创建工程** (**プロジェクトの作成**)。

    ![][14]

7. アプリケーション名を入力し、クリックして **创建** (**作成**)。

    ![][15]

8. Baidu クラウド プッシュ プロジェクトの正常に作成を含むページが表示されます **AppID**, 、**API キー**, 、および **シークレット キー**します。 API キーとシークレット キーは後で使用するので書き留めておきます。

    ![][16]

9. クリックして、プッシュ通知のプロジェクトを構成 **[云推送** (**クラウド プッシュ**) 左側のウィンドウでします。

    ![][31]

10. 次のページをクリックして、 **推送设置** (**設定プッシュ**) ボタンをクリックします。

    ![][32]  

11. [構成] ページで、Android プロジェクトで使用するパッケージ名を追加、 **应用包名** (**アプリケーション パッケージ**) フィールドに、クリックして **设** (**保存**)。  

    ![][33]

**"保存成功！"** (**正常に保存されました!**)メッセージです。

##通知ハブを構成する

1. [Azure クラシック ポータル、] にサインインし、をクリックし、 **+ 新規** 、画面の下部にあります。

2. クリックして **App Services**, 、] をクリックして **Service Bus**, 、] をクリックして **通知ハブ**, 、] をクリックし、 **簡易作成**します。

3. 名前を指定、 **通知ハブ**, を選択、 **地域** と **名前空間** しこの通知ハブが作成する] をクリックし、 **新しい通知ハブの作成**します。  

    ![][17]

4. 通知ハブを作成した名前空間をクリックし、クリックして **Notification Hubs** 上部にあります。

    ![][18]

5. をクリックし、作成した通知ハブを選択して **構成** 上部のメニューからです。

    ![][19]

6. 下へスクロールして、 **baidu 通知の設定** セクションし、API キーと秘密キーが以前に入手した Baidu コンソールから、Baidu クラウド プッシュ プロジェクトを入力します。 クリックして **保存**します。

    ![][20]

7. クリックして、 **ダッシュ ボード** 、通知ハブの上部にあるタブをクリックして **[接続文字列の**です。

    ![][21]

8. 書き留めて、 **DefaultListenSharedAccessSignature** と **DefaultFullSharedAccessSignature** から、 **接続情報にアクセス** ウィンドウです。

    ![][22]

##通知ハブにアプリケーションを接続する

1. Eclipse ADT で新しい Android プロジェクトを作成します (**ファイル** > **新規** > **Android アプリケーション プロジェクト**)。

    ![][23]

2. 入力、 **アプリケーション名** いることを確認し、 **最小必須 SDK** に設定されているバージョン **API 16: Android 4.1**します。

    ![][24]

3. をクリックして **次** までウィザードに従って操作を続行し、 **アクティビティの作成** ウィンドウが表示されます。 確認して **Blank Activity** が選択されて、最後に **完了** 新しい Android アプリケーションを作成します。

    ![][25]

4. 確認して、 **Project Build Target** が正しく設定されています。

    ![][26]

5. 通知ハブ-0.4.jar ファイルをダウンロード、 **ファイル** のタブ、 [通知ハブ-Android-SDK Bintray に](https://bintray.com/microsoftazuremobile/SDK/Notification-Hubs-Android-SDK/0.4)します。 ファイルを追加、 **libs** Eclipse のプロジェクト、および更新のフォルダー、 *libs* フォルダーです。

6. ダウンロードして解凍 [Baidu Push Android SDK] を開く、 **libs** フォルダー、および [コピー、 **pushservice x.y.z** jar ファイルと **armeabi** & **mips** 内のフォルダー、 **libs** 、Android アプリケーションのフォルダーです。

7. 開いている、 **AndroidManifest.xml** ファイル、Android のプロジェクトを Baidu SDK に必要なアクセス許可を追加します。

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.READ_PHONE_STATE" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.WRITE_SETTINGS" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
        <uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
        <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
        <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
        <uses-permission android:name="android.permission.ACCESS_DOWNLOAD_MANAGER" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />

8. 追加、 **android: 名** プロパティを **アプリケーション** 内の要素 **AndroidManifest.xml**, 、置換 *yourprojectname* (例: **com.example.BaiduTest**)。 このプロジェクト名が、Baidu コンソールで構成したプロジェクト名と一致していることを確認します。

        <application android:name="yourprojectname.DemoApplication"

9. 後のアプリケーション要素内で次の構成を追加、 **です。MainActivity** アクティビティ要素交換 *yourprojectname* (例: **com.example.BaiduTest**)。

        <receiver android:name="yourprojectname.MyPushMessageReceiver">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.MESSAGE" />
                <action android:name="com.baidu.android.pushservice.action.RECEIVE" />
                <action android:name="com.baidu.android.pushservice.action.notification.CLICK" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.PushServiceReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
                <action android:name="com.baidu.android.pushservice.action.notification.SHOW" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.RegistrationReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.METHOD" />
                <action android:name="com.baidu.android.pushservice.action.BIND_SYNC" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.PACKAGE_REMOVED"/>
                <data android:scheme="package" />
            </intent-filter>
        </receiver>

        <service
            android:name="com.baidu.android.pushservice.PushService"
            android:exported="true"
            android:process=":bdservice_v1"  >
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.PUSH_SERVICE" />
            </intent-filter>
        </service>

9. いう新しいクラスを追加 **ConfigurationSettings.java** をプロジェクトにします。

    ![][28]

    ![][29]

10. このクラスに次のコードを追加します。

        public class ConfigurationSettings {
                public static String API_KEY = "...";
                public static String NotificationHubName = "...";
                public static String NotificationHubConnectionString = "...";
            }

    値を設定 **API_KEY** 取得したもの、Baidu クラウド プロジェクトから前の手順では、 **NotificationHubName** Azure クラシック ポータルから、通知ハブの名前と、 **NotificationHubConnectionString** Azure クラシック ポータルの DefaultListenSharedAccessSignature をします。

11. いう新しいクラスを追加 **DemoApplication.java**, を次のコードを追加します。

        import com.baidu.frontia.FrontiaApplication;

        public class DemoApplication extends FrontiaApplication {
            @Override
            public void onCreate() {
                super.onCreate();
            }
        }

12. 呼ばれる新しいクラスを追加 **MyPushMessageReceiver.java**, を次のコードを追加します。 これは、Baidu プッシュ サーバーから受信したプッシュ通知を処理するクラスです。

        import java.util.List;
        import android.content.Context;
        import android.os.AsyncTask;
        import android.util.Log;
        import com.baidu.frontia.api.FrontiaPushMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub;

        public class MyPushMessageReceiver extends FrontiaPushMessageReceiver {
            /** TAG to Log */
            public static NotificationHub hub = null;
            public static String mChannelId, mUserId;
            public static final String TAG = MyPushMessageReceiver.class
                    .getSimpleName();

            @Override
            public void onBind(Context context, int errorCode, String appid,
                    String userId, String channelId, String requestId) {
                String responseString = "onBind errorCode=" + errorCode + " appid="
                        + appid + " userId=" + userId + " channelId=" + channelId
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
                mChannelId = channelId;
                mUserId = userId;

                try {
                 if (hub == null) {
                        hub = new NotificationHub(
                                ConfigurationSettings.NotificationHubName,
                                ConfigurationSettings.NotificationHubConnectionString,
                                context);
                        Log.i(TAG, "Notification hub initialized");
                    }
                } catch (Exception e) {
                   Log.e(TAG, e.getMessage());
                }

                registerWithNotificationHubs();
            }

            private void registerWithNotificationHubs() {
               new AsyncTask<Void, Void, Void>() {
                  @Override
                  protected Void doInBackground(Void... params) {
                     try {
                         hub.registerBaidu(mUserId, mChannelId);
                         Log.i(TAG, "Registered with Notification Hub - '"
                                + ConfigurationSettings.NotificationHubName + "'"
                                + " with UserId - '"
                                + mUserId + "' and Channel Id - '"
                                + mChannelId + "'");
                     } catch (Exception e) {
                         Log.e(TAG, e.getMessage());
                     }
                     return null;
                 }
               }.execute(null, null, null);
            }

            @Override
            public void onSetTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onSetTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onDelTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onDelTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onListTags(Context context, int errorCode, List<String> tags,
                    String requestId) {
                String responseString = "onListTags errorCode=" + errorCode + " tags="
                        + tags;
                Log.d(TAG, responseString);
            }

            @Override
            public void onUnbind(Context context, int errorCode, String requestId) {
                String responseString = "onUnbind errorCode=" + errorCode
                        + " requestId = " + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onNotificationClicked(Context context, String title,
                    String description, String customContentString) {
                String notifyString = "title=\"" + title + "\" description=\""
                        + description + "\" customContent=" + customContentString;
                Log.d(TAG, notifyString);
            }

            @Override
            public void onMessage(Context context, String message,
                    String customContentString) {
                String messageString = "message=\"" + message + "\" customContentString=" + customContentString;
                Log.d(TAG, messageString);
            }
        }

13. 開いている **MainActivity.java**, 、次のコードを追加し、 **onCreate** メソッド。

            PushManager.startWork(getApplicationContext(),
                    PushConstants.LOGIN_TYPE_API_KEY, ConfigurationSettings.API_KEY);

14. 先頭に、次の import ステートメントを追加します。

            import com.baidu.android.pushservice.PushConstants;
            import com.baidu.android.pushservice.PushManager;

##アプリケーションに通知を送信する


以下の画面に示すように、通知ハブの [デバッグ] タブを使用して、Azure クラシック ポータルで通知を送信し、アプリケーションで通知の受信テストを行うことができます。

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-debug.png)

プッシュ通知は通常、互換性のあるライブラリを使用して Mobile Services などのバックエンド サービスや ASP.NET に送信されます。 ライブラリがバックエンドで利用できない場合、REST API を直接使用して通知メッセージを送信できます。 

このチュートリアルでは、バックエンド サービスではなく、コンソール アプリケーションの通知ハブに .NET SDK を使用して通知を送信することで例を単純にして、クライアント アプリのテスト手順のみを説明します。 お勧め、 [通知ハブを使用したユーザーにプッシュ通知を](notification-hubs-aspnet-backend-windows-dotnet-notify-users.md) 」チュートリアルでは、次に、ASP.NET バックエンドから通知を送信するためです。 ただし、通知の送信には、次の方法も使用できます。

* **REST インターフェイス**: バックエンド プラットフォームを使用して通知をサポートすることができます、 [REST インターフェイス](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx)します。

* **Microsoft Azure Notification Hubs .NET SDK**: で、Nuget パッケージ マネージャーを実行している Visual Studio の [Install-package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)します。

* **Node.js** : [Node.js から Notification Hubs の使用方法](notification-hubs-nodejs-how-to-use-notification-hubs.md)します。

* **Azure Mobile Services**: Notification Hubs と統合されている Azure Mobile Services バックエンドから通知を送信する方法の例は、作業開始 Mobile Services でプッシュ通知の使用」を参照してください ([.NET バックエンド](../mobile-services/mobile-services-javascript-backend-windows-store-dotnet-get-started-push.md) | [JavaScript バックエンド](../mobile-services/mobile-services-javascript-backend-windows-store-dotnet-get-started-push.md))。

* **Java または PHP**: REST Api を使用して通知を送信する方法の例は、「JAVA/PHP から Notification Hubs を使用する方法」を参照してください ([Java](notification-hubs-java-backend-how-to.md) | [PHP](notification-hubs-php-backend-how-to.md))。

##(省略可能) .NET コンソール アプリケーションから通知を送信する

このセクションでは、.NET コンソール アプリケーションを使用した通知の送信方法について説明します。

1. Visual C# の新しいコンソール アプリケーションを作成します。

    ![][30]

2. パッケージ マネージャー コンソール] ウィンドウで、設定、 **既定のプロジェクト** 、新しいコンソール アプリケーション プロジェクトにし、コンソール ウィンドウには、次のコマンドを実行します。

        Install-Package Microsoft.Azure.NotificationHubs

    使用して、Azure Notification Hubs SDK への参照を追加します <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification ハブ NuGet パッケージ</a>.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

3. ファイルを開く **Program.cs** し、以下の追加ステートメントを使用します。

        using Microsoft.Azure.NotificationHubs;

4.  `Program` クラスの次のメソッドを追加および置換 *DefaultFullSharedAccessSignatureSASConnectionString* と *NotificationHubName* がある値を持つ。

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("DefaultFullSharedAccessSignatureSASConnectionString", "NotificationHubName");
            string message = "{\"title\":\"((Notification title))\",\"description\":\"Hello from Azure\"}";
            var result = await hub.SendBaiduNativeNotificationAsync(message);
        }

5. 次の行を追加、 **Main** メソッド。

         SendNotificationAsync();
         Console.ReadLine();

##アプリケーションをテストする

実際の電話でこのアプリケーションをテストする場合は、USB ケーブルを使用してコンピューターに電話を接続するだけです。 これで、接続した電話にアプリケーションが読み込まれます。

上部の Eclipse ツールバーで、エミュレーターでは、このアプリケーションをテストするにはクリックして **実行**, 、アプリケーションを選択します。 これによりエミュレーターが起動し、アプリケーションが読み込まれて実行されます。

アプリケーションが Baidu プッシュ通知サービスから 'userId' と 'channelId' を取得し、通知ハブに登録します。

テスト通知は、Azure クラシック ポータルの [デバッグ] タブを使用して送信できます。 Visual Studio で .Net コンソール アプリケーションを作成する場合、Visual Studio で F5 キーを押してアプリケーションを実行します。 アプリケーションから通知が送信され、デバイスまたはエミュレーターの上部の通知領域に表示されます。


<!-- Images. -->
[1]: ./media/notification-hubs-baidu-get-started/BaiduRegistration.png
[2]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationInput.png
[3]: ./media/notification-hubs-baidu-get-started/BaiduConfirmation.png
[4]: ./media/notification-hubs-baidu-get-started/BaiduActivationEmail.png
[5]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationMore.png
[6]: ./media/notification-hubs-baidu-get-started/BaiduOpenCloudPlatform.png
[7]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperServices.png
[8]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperRegistration.png
[9]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationInput.png
[10]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationConfirmation.png
[11]: ./media/notification-hubs-baidu-get-started/BaiduDevConfirmationFinal.png
[12]: ./media/notification-hubs-baidu-get-started/BaiduCloudPush.png
[13]: ./media/notification-hubs-baidu-get-started/BaiduDevSvcMgmt.png
[14]: ./media/notification-hubs-baidu-get-started/BaiduCreateProject.png
[15]: ./media/notification-hubs-baidu-get-started/BaiduCreateProjectInput.png
[16]: ./media/notification-hubs-baidu-get-started/BaiduProjectKeys.png
[17]: ./media/notification-hubs-baidu-get-started/AzureNHCreation.png
[18]: ./media/notification-hubs-baidu-get-started/NotificationHubs.png
[19]: ./media/notification-hubs-baidu-get-started/NotificationHubsConfigure.png
[20]: ./media/notification-hubs-baidu-get-started/NotificationHubBaiduConfigure.png
[21]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionStringView.png
[22]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionString.png
[23]: ./media/notification-hubs-baidu-get-started/EclipseNewProject.png
[24]: ./media/notification-hubs-baidu-get-started/EclipseProjectCreation.png
[25]: ./media/notification-hubs-baidu-get-started/EclipseBlankActivity.png
[26]: ./media/notification-hubs-baidu-get-started/EclipseProjectBuildProperty.png
[27]: ./media/notification-hubs-baidu-get-started/EclipseBaiduReferences.png
[28]: ./media/notification-hubs-baidu-get-started/EclipseNewClass.png
[29]: ./media/notification-hubs-baidu-get-started/EclipseConfigSettingsClass.png
[30]: ./media/notification-hubs-baidu-get-started/ConsoleProject.png
[31]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig1.png
[32]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig2.png
[33]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig3.png

<!-- URLs. -->
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Baidu Push Android SDK]: http://developer.baidu.com/wiki/index.php?title=docs/cplat/push/sdk/clientsdk
[Azure Classic Portal]: https://manage.windowsazure.com/
[Baidu portal]: http://www.baidu.com/

