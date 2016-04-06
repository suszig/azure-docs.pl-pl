<properties
    pageTitle="Azure Notification Hubs の使用 (Kindle アプリ) | Microsoft Azure"
    description="このチュートリアルでは、Azure Notification Hubs を使用して Kindle アプリケーションにプッシュ通知を送信する方法について学習します。"
    services="notification-hubs"
    documentationCenter=""
    authors="wesmc7777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-kindle"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="11/04/2015"
    ms.author="wesmc"/>

# Notification Hubs の使用 (Kindle アプリ)

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##概要

このチュートリアルでは、Azure Notification Hubs を使用して Kindle アプリケーションにプッシュ通知を送信する方法について説明します。
Amazon Device Messaging (ADM) を使用してプッシュ通知を受信する空の Kindle アプリケーションを作成します。

##前提条件

このチュートリアルには、次のものが必要です。

+ <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android サイト</a>から Android SDK を入手します (Eclipse の使用を想定)。
+ 「<a href="https://developer.amazon.com/appsandservices/resources/development-tools/ide-tools/tech-docs/01-setting-up-your-development-environment">開発環境を設定する</a>」の手順に従い、Kindle の開発環境を設定します。

##開発者ポータルへの新しいアプリの追加

1. アプリを最初に、作成、 [Amazon developer portal]します。

    ![][0]

2. コピー、 **アプリケーション キー**します。

    ![][1]

3. ポータルで、アプリの名前をクリックし、 **Device Messaging** ] タブをクリックします。

    ![][2]

4. をクリックして **新しいセキュリティ プロファイルを作成**, 、新しいセキュリティ プロファイルを作成し、(たとえば、 **TestAdm セキュリティ プロファイル**)。 クリックして **保存**します。

    ![][3]

5. クリックして **セキュリティ プロファイル** 作成したセキュリティ プロファイルを表示します。 コピー、 **クライアント ID** と **クライアント シークレット** 後で使用できる値です。

    ![][4]

## API キーの作成

1. 管理者特権でコマンド プロンプトを開きます。
2. Android SDK フォルダーに移動します。
3. 次のコマンドを入力します。

        keytool -list -v -alias androiddebugkey -keystore ./debug.keystore

    ![][5]

4.   **キーストア** パスワードで、「 **android**します。

5.  コピー、 **MD5** 指紋です。
6.  開発者ポータルに戻り、 **メッセージング** ] タブをクリックして **Android/kindle** アプリのパッケージの名前を入力し、(たとえば、 **com.sample.notificationhubtest**) および **MD5** 値に設定して、をクリックして **API キーの生成**します。

## ハブへの資格情報の追加

ポータルで、追加、クライアント シークレットとクライアントの ID を **構成** 通知ハブのタブをクリックします。

## アプリケーションの設定

> [AZURE.NOTE] アプリケーションを作成するときに使用して、少なくとも API レベル 17 です。

ADM ライブラリを Eclipse プロジェクトに追加します。

1. ADM ライブラリを取得する [download the SDK]します。 SDK zip ファイルを解凍します。
2. Eclipse で、プロジェクトを右クリックし、順にクリックして **プロパティ**します。 選択 **Java Build Path** 左側で、クリックして、* * ライブラリ * * 上部にあるタブです。 クリックして **外部 Jar の追加**, 、ファイルを選択して `\SDK\Android\DeviceMessaging\lib\amazon-device-messaging-*.jar` 、Amazon SDK を解凍したディレクトリからです。
3. NotificationHubs Android SDK (リンク) をダウンロードします。
4. パッケージを解凍し、`notification-hubs-sdk.jar` ファイルを Eclipse の `libs` フォルダーにドラッグします。

アプリケーション マニフェストを編集して、ADM をサポートするようにします。

1. ルート マニフェスト要素に Amazon 名前空間を追加します。


        xmlns:amazon="http://schemas.amazon.com/apk/res/android"

2. マニフェスト要素の下の最初の要素としてアクセス許可を追加します。 代替 **[YOUR PACKAGE NAME]** パッケージ アプリを作成するために使用するとします。

        <permission
         android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE"
         android:protectionLevel="signature" />

        <uses-permission android:name="android.permission.INTERNET"/>

        <uses-permission android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE" />

        <!-- This permission allows your app access to receive push notifications
        from ADM. -->
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE" />

        <!-- ADM uses WAKE_LOCK to keep the processor from sleeping when a message is received. -->
        <uses-permission android:name="android.permission.WAKE_LOCK" />

3. 次の要素をアプリケーション要素の最初の子として挿入します。 置き換えます **[YOUR SERVICE NAME]** (パッケージを含む)、次のセクションで作成して、置換する ADM メッセージ ハンドラーの名前で **[YOUR PACKAGE NAME]** はアプリを作成したパッケージ名にします。

        <amazon:enable-feature
              android:name="com.amazon.device.messaging"
                     android:required="true"/>
        <service
            android:name="[YOUR SERVICE NAME]"
            android:exported="false" />

        <receiver
            android:name="[YOUR SERVICE NAME]$Receiver" />

            <!-- This permission ensures that only ADM can send your app registration broadcasts. -->
            android:permission="com.amazon.device.messaging.permission.SEND" >

            <!-- To interact with ADM, your app must listen for the following intents. -->
            <intent-filter>
          <action android:name="com.amazon.device.messaging.intent.REGISTRATION" />
          <action android:name="com.amazon.device.messaging.intent.RECEIVE" />

          <!-- Replace the name in the category tag with your app's package name. -->
          <category android:name="[YOUR PACKAGE NAME]" />
            </intent-filter>
        </receiver>

## ADM メッセージ ハンドラーの作成

1. `com.amazon.device.messaging.ADMMessageHandlerBase` を継承する新しいクラスを作成し、次の図に示すように `MyADMMessageHandler` という名前を付けます。

    ![][6]

2. 次の `import` ステートメントを追加します。

        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.support.v4.app.NotificationCompat;
        import com.amazon.device.messaging.ADMMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub

3. 作成したクラスに次のコードを追加します。 ハブ名および接続文字列 (listen) を置き換えます。

        public static final int NOTIFICATION_ID = 1;
        private NotificationManager mNotificationManager;
        NotificationCompat.Builder builder;
        private static NotificationHub hub;
        public static NotificationHub getNotificationHub(Context context) {
            Log.v("com.wa.hellokindlefire", "getNotificationHub");
            if (hub == null) {
                hub = new NotificationHub("[hub name]", "[listen connection string]", context);
            }
            return hub;
        }

        public MyADMMessageHandler() {
                super("MyADMMessageHandler");
            }

            public static class Receiver extends ADMMessageReceiver
            {
                public Receiver()
                {
                    super(MyADMMessageHandler.class);
                }
            }

            private void sendNotification(String msg) {
                Context ctx = getApplicationContext();

             mNotificationManager = (NotificationManager)
                    ctx.getSystemService(Context.NOTIFICATION_SERVICE);

            PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                new Intent(ctx, MainActivity.class), 0);

            NotificationCompat.Builder mBuilder =
                new NotificationCompat.Builder(ctx)
                .setSmallIcon(R.mipmap.ic_launcher)
                .setContentTitle("Notification Hub Demo")
                .setStyle(new NotificationCompat.BigTextStyle()
                         .bigText(msg))
                .setContentText(msg);

            mBuilder.setContentIntent(contentIntent);
            mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
        }

4. 次のコードを `OnMessage()` メソッドに追加します。

        String nhMessage = intent.getExtras().getString("msg");
        sendNotification(nhMessage);

5. 次のコードを `OnRegistered` メソッドに追加します。

            try {
        getNotificationHub(getApplicationContext()).register(registrationId);
            } catch (Exception e) {
        Log.e("[your package name]", "Fail onRegister: " + e.getMessage(), e);
            }

6.  次のコードを `OnUnregistered` メソッドに追加します。

            try {
                getNotificationHub(getApplicationContext()).unregister();
            } catch (Exception e) {
                Log.e("[your package name]", "Fail onUnregister: " + e.getMessage(), e);
            }

7. `MainActivity` メソッドで、次の import ステートメントを追加します。

        import com.amazon.device.messaging.ADM;

8. `OnCreate` メソッドの末尾に次のコードを追加します。

        final ADM adm = new ADM(this);
        if (adm.getRegistrationId() == null)
        {
           adm.startRegister();
        } else {
            new AsyncTask() {
                  @Override
                  protected Object doInBackground(Object... params) {
                     try {                       MyADMMessageHandler.getNotificationHub(getApplicationContext()).register(adm.getRegistrationId());
                     } catch (Exception e) {
                         Log.e("com.wa.hellokindlefire", "Failed registration with hub", e);
                         return e;
                     }
                     return null;
                 }
               }.execute(null, null, null);
        }

## アプリへの API キーの追加

1. Eclipse で、という名前の新しいファイルを作成 **api_key.txt** プロジェクトのディレクトリ アセットにします。
2. ファイルを開き、Amazon 開発者ポータルで生成した API キーをコピーします。

## アプリの実行

1. エミュレーターを起動します。
2. エミュレーターで、上部からスワイプして **設定**, 、順にクリック **アカウント** し、有効な Amazon アカウントに登録します。
3. Eclipse で、アプリを実行します。

> [AZURE.NOTE] 問題が発生した場合は、エミュレーター (またはデバイス) の時間を確認します。 時刻値は正確である必要があります。 Kindle エミュレーターの時刻を変更するには、Android SDK platform-tools ディレクトリで次のコマンドを実行します。

        adb shell  date -s "yyyymmdd.hhmmss"

## メッセージの送信

.NET を使用してメッセージを送信するには:

        static void Main(string[] args)
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("[conn string]", "[hub name]");

            hub.SendAdmNativeNotificationAsync("{\"data\":{\"msg\" : \"Hello from .NET!\"}}").Wait();
        }

![][7]

<!-- URLs. -->
[Amazon developer portal]: https://developer.amazon.com/home.html
[download the SDK]: https://developer.amazon.com/public/resources/development-tools/sdk

[0]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal1.png
[1]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal2.png
[2]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal3.png
[3]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal4.png
[4]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal5.png
[5]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-cmd-window.png
[6]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-new-java-class.png
[7]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-notification.png


