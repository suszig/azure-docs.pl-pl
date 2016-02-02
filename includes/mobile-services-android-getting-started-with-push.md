1. **アプリ** プロジェクト、ファイルを開きます `AndroidManifest.xml`します。 次の 2 つの手順でコードで置き換えます _`* * my_app_package * *`_ は、プロジェクトのアプリケーション パッケージの名前の値である、 `パッケージ` の属性、 `マニフェスト` タグ。

2. 既存の後に次の新しいアクセス許可を追加 `使用権限` 要素。

        <permission android:name="**my_app_package**.permission.C2D_MESSAGE" 
            android:protectionLevel="signature" />
        <uses-permission android:name="**my_app_package**.permission.C2D_MESSAGE" /> 
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="android.permission.GET_ACCOUNTS" />
        <uses-permission android:name="android.permission.WAKE_LOCK" />

3. 後は、次のコードを追加、 `アプリケーション` 開始タグ。

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
                                        android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="**my_app_package**" />
            </intent-filter>
        </receiver>

4. アプリケーション ディレクトリ ファイルにある **build.gradle** ファイルの*依存関係*にこの行を追加して、Gradle をプロジェクトに再同期させます。

        compile(group: 'com.microsoft.azure', name: 'azure-notifications-handler', version: '1.0.1', ext: 'jar')

5. ファイル *ToDoItemActivity.java* を開き、次の import ステートメントを追加します。

        import com.microsoft.windowsazure.notifications.NotificationsManager;

6. 次のプライベート変数をクラスに追加します。 _ を置き換える`< PROJECT_NUMBER >`_ は、前の手順でアプリに Google によって割り当てられたプロジェクト番号。

        public static final String SENDER_ID = "<PROJECT_NUMBER>";

7. *MobileServiceClient* の定義を **private** から **public static** に変更し、次のようにします。

        public static MobileServiceClient mClient;

8. 次に、通知を処理する新しいクラスを追加する必要があります。 プロジェクト エクスプ ローラーで開く、 **src** = > **メイン** = > **java** ノード、およびパッケージ名のノードを右クリックします。] をクリック **新規**, 、順にクリック **Java クラス**します。

9. **名** 型 `MyHandler`, 、順にクリックして **OK**します。

    ![](./media/mobile-services-android-get-started-push/android-studio-create-class.png)

10. MyHandler ファイルで、クラス宣言を以下の内容と置き換えます。

        public class MyHandler extends NotificationsHandler {

11. 次の import ステートメントを追加、 `MyHandler` クラス。

        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.AsyncTask;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;

12. 次に、次のメンバーを追加、 `MyHandler` クラス。

        public static final int NOTIFICATION_ID = 1;
        private NotificationManager mNotificationManager;
        NotificationCompat.Builder builder;
        Context ctx;

13. `MyHandler` クラスでをオーバーライドする次のコードを追加、 **onRegistered** メソッドで、モバイル サービス通知ハブにデバイスを登録します。

    @Override
    public void onRegistered(Context context,  final String gcmRegistrationId) {
        super.onRegistered(context, gcmRegistrationId);
    
        new AsyncTask<Void, Void, Void>() {
    
            protected Void doInBackground(Void... params) {
                try {
                    ToDoActivity.mClient.getPush().register(gcmRegistrationId, null);
                    return null;
                }
                catch(Exception e) { 
                    // handle error             
                }
                return null;            
            }
        }.execute();
    }

14. `MyHandler` クラスでをオーバーライドする次のコードを追加、 **onReceive** を受信したときに表示される通知を行うメソッドです。

    @Override
    public void onReceive(Context context, Bundle bundle) {
        ctx = context;
        String nhMessage = bundle.getString("message");
    
        sendNotification(nhMessage);
    }
    
    private void sendNotification(String msg) {
        mNotificationManager = (NotificationManager)
                  ctx.getSystemService(Context.NOTIFICATION_SERVICE);
    
        PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
              new Intent(ctx, ToDoActivity.class), 0);
    
        NotificationCompat.Builder mBuilder =
              new NotificationCompat.Builder(ctx)
              .setSmallIcon(R.drawable.ic_launcher)
              .setContentTitle("Notification Hub Demo")
              .setStyle(new NotificationCompat.BigTextStyle()
                         .bigText(msg))
              .setContentText(msg);
    
         mBuilder.setContentIntent(contentIntent);
         mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
    }

15. TodoActivity.java ファイルに戻り、*ToDoActivity* クラスの **onCreate** メソッドを更新して、通知のハンドラー クラスを登録します。 *MobileServiceClient* がインスタンス化された後に、このコードを追加してください。

        NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);

    これで、アプリケーションがプッシュ通知をサポートするように更新されました。



[mobile services android sdk]: http://aka.ms/Iajk6q 

